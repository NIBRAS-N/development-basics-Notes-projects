We need to refactor the existing single-table design into a two-table design (Option B) to eliminate data duplication when targets are the same across all branches and months. The new design will use:

  - metadata table: stores common fields (question details, yearly/monthly targets) plus frozen sets of branch IDs and months.

  - details table: stores each branch+month combination, referencing the metadata.

The API should remain backward-compatible where possible, but internal logic will change.

## Step-by-Step Implementation Plan

### 1. Create New Entity Files


#### File: src/modules/target-setup/entities/target-setup-metadata.entity.ts

```ts
import { Entity, Column, GeneratedUuidColumn } from '@iaminfinity/express-cassandra';
import { BaseEntity } from '../../../shared/entities/base-entity';

@Entity({
  table_name: 'target_setup_metadata',
  key: ['id'],
})
export class TargetSetupMetadataEntity extends BaseEntity {
  @GeneratedUuidColumn()
  id: any;

  @Column({ type: 'uuid' })
  year_id: any;

  @Column({ type: 'text' })
  questions_title: string;

  @Column({ type: 'uuid' })
  questions_id: any;

  @Column({ type: 'text' })
  question_number: string;

  @Column({ type: 'int' })
  yearly_target: number;

  @Column({ type: 'int' })
  monthly_target: number;

  @Column({ type: 'frozen<set<uuid>>' })
  branch_ids: Set<any>;  // stored as frozen set

  @Column({ type: 'frozen<set<text>>' })
  months: Set<string>;

  // BaseEntity provides created_at, updated_at, created_by, updated_by
}
```

#### File: src/modules/target-setup/entities/target-setup-details.entity.ts

```ts
import { Entity, Column, GeneratedUuidColumn } from '@iaminfinity/express-cassandra';
import { BaseEntity } from '../../../shared/entities/base-entity';

@Entity({
  table_name: 'target_setup_details',
  key: [['year_id'], 'questions_id', 'branch_id', 'month'],
})
export class TargetSetupDetailsEntity extends BaseEntity {
  @GeneratedUuidColumn()
  id: any;   // optional, but useful for referencing

  @Column({ type: 'uuid' })
  year_id: any;

  @Column({ type: 'uuid' })
  questions_id: any;

  @Column({ type: 'uuid' })
  branch_id: any;

  @Column({ type: 'text' })
  month: string;

  @Column({ type: 'uuid' })
  metadata_id: any;   // references metadata.id
}
```

### 2. Update DTOs (Optional – Keep Existing but Adjust Validation)

We will keep the existing DTOs unchanged to maintain API contract, but we'll add validation in the service to ensure consistency.

No changes to:

  - CreateTargetSetupItemDto

  - CreateTargetSetupDto

  - UpdateTargetSetupItemDto

  - UpdateTargetSetupDto

  - TargetSetupResponseDto

But note: TargetSetupResponseDto currently contains all fields. After refactor, we will construct it from metadata+details.

### 3. Update Module Imports

Modify target-setup.module.ts to import the new repositories:

```ts
import { ExpressCassandraModule } from '@iaminfinity/express-cassandra';
import { TargetSetupMetadataEntity } from './entities/target-setup-metadata.entity';
import { TargetSetupDetailsEntity } from './entities/target-setup-details.entity';
// Remove old TargetSetupEntity import

@Module({
  imports: [
    ExpressCassandraModule.forFeature(
      [TargetSetupMetadataEntity, TargetSetupDetailsEntity],
      'idp'
    ),
  ],
  controllers: [TargetSetupController],
  providers: [TargetSetupService],
  exports: [TargetSetupService],
})
export class TargetSetupModule {}
```

### 4. Update Service (target-setup.service.ts)

Inject both repositories:
```ts
@InjectRepository(TargetSetupMetadataEntity)
private readonly metadataRepo: Repository<TargetSetupMetadataEntity>,

@InjectRepository(TargetSetupDetailsEntity)
private readonly detailsRepo: Repository<TargetSetupDetailsEntity>,
```
4.1 Create Method
  - Accept CreateTargetSetupDto as before.
      
  - Validate that all items in targets array have the same questions_title, questions_id, question_number, yearly_target, monthly_target. If not, throw BadRequestException.
      
  - Extract unique branch IDs and months from the items.
      
  - Create a metadata entity with the common fields and the sets.
      
  - Save metadata.
      
  - For each unique combination of branch_id and month, create a details entity with the same year_id, questions_id, and reference metadata_id.
      
  - Save all details (use Promise.all or forkJoin with observables). Return an array of created details (with their ids) to match the old return type.
    
4.2 Update Method
    
- Accept UpdateTargetSetupDto.
    
- For each item in targets:
    
- Fetch details by item.id using detailsRepo.findOne({ id: toUuidOrThrow(item.id) }).
    
- If not found, mark as unsuccessful.
    
- Otherwise, get metadata_id from details, then fetch metadata.
    
- Update metadata's yearly_target and/or monthly_target if provided in the item.
    
- Save metadata.
    
- Note: If multiple items belong to the same metadata, we will update the metadata multiple times. To avoid redundant saves, we can collect unique metadata ids and update each once, but that adds complexity. For simplicity, we'll update per item; it's acceptable because metadata updates are idempotent and rare.
    
- Return summary of successful/unsuccessful ids (the details ids).

4.3 Find Methods:

- findById(id: string): fetch details, then metadata, then combine.

- findByYear(year_id): fetch details (by year_id), collect metadata_ids, fetch all metadata in one query (IN clause), then merge in code.

- findByYearAndQuestion(year_id, questions_id): similar.

- findByYearQuestionAndBranch(year_id, questions_id, branch_id): similar.

##### Create a helper function
    combineDetailsWithMetadata(details: TargetSetupDetailsEntity[], metadataMap: Map<string, TargetSetupMetadataEntity>) 
  that returns an array of objects matching the old entity shape (all fields). This will be used in all GET endpoints.

### 5. Update Controller
   
  The controller methods can remain largely unchanged. They call service methods and map responses.
  However, we need to ensure that the create endpoint returns the ids of created details (which it already does).
  The update endpoint also returns details ids.

  The get endpoints return arrays of combined objects. The response DTO TargetSetupResponseDto already has all fields, so no change needed there.

### 6. Remove Old Entity and Repository
  - After  code changes, remove TargetSetupEntity and its repository from the module. Also remove any unused imports


### 7. Testing

- Test create with a payload containing multiple branches and months. Verify that metadata is created once and details for each combination.

- Test get by year, year+question, year+question+branch – ensure returned data matches old shape.

- Test update of targets – verify that all details under the same metadata reflect the new values.

- Test update of a single detail id – verify metadata updates and all details change.

### 8. Update Swagger Documentation (Optional)
- If needed, update the API documentation to reflect that targets are shared across branches/months.

