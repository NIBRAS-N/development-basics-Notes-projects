

### We'll implement the single‑table design that matches your new input structure. Follow these steps in order. Each step provides the exact code to add or modify.

New Input Strucure is:

[{
	year_id: uuid
	component_id:
	form_id:
	question_wise_data:[
		{
			question_id: uuid	
			branches: []
			months: []
			monthly_target:
			yearly_target:
		}
	]
	
}
]

## Here is the step by step plan :

# Step 1: Entity Design
File: src/modules/target-setup/entities/target-setup.entity.ts

Replace the existing entity with this new one. It stores one row per (year_id, question_id, branch_id, month) combination.
A rule_id groups rows that came from the same input rule

```ts
import {
  Column,
  Entity,
  GeneratedUuidColumn,
} from '@iaminfinity/express-cassandra';
import { BaseEntity } from '../../../shared/entities/base-entity';

@Entity({
  table_name: 'target_setup',
  key: [['year_id'], 'question_id', 'branch_id', 'month'],
  // Optional materialized view for querying by rule_id
  materialized_views: {
    target_setup_by_rule: {
      key: [['rule_id'], 'year_id', 'question_id', 'branch_id', 'month'],
      select: ['*'],
    },
  },
})
export class TargetSetupEntity extends BaseEntity {
  @GeneratedUuidColumn()
  id?: any; // auto-generated, not part of primary key

  @Column({ type: 'uuid' })
  year_id: any;

  @Column({ type: 'uuid' })
  component_id: any;

  @Column({ type: 'uuid' })
  form_id: any;

  @Column({ type: 'uuid' })
  question_id: any;

  @Column({ type: 'uuid' })
  branch_id: any;

  @Column({ type: 'text' })
  month: string;

  @Column({ type: 'int' })
  yearly_target: number;

  @Column({ type: 'int' })
  monthly_target: number;

  @Column({ type: 'uuid' })
  rule_id: any; // groups rows from the same rule
}
```

#### Important:

- All uuid columns are defined with type 'uuid'. When saving, pass plain string UUIDs – the ORM will convert them automatically.

- The primary key [['year_id'], 'question_id', 'branch_id', 'month'] allows efficient queries by year, then by question, then by branch, and finally by month.

# Step 2: DTO Design
Create these new DTO files in src/modules/target-setup/dtos/. They exactly match the JSON structure you provided.

```ts
import { ApiProperty } from '@nestjs/swagger';
import { IsNumber, IsArray, IsUUID } from 'class-validator';

export class TargetRuleDto {
  @ApiProperty()
  @IsNumber()
  monthly_target: number;

  @ApiProperty()
  @IsNumber()
  yearly_target: number;

  @ApiProperty({ type: [String] })
  @IsArray()
  @IsUUID('4', { each: true })
  branchIds: string[];

  @ApiProperty({ type: [String] })
  @IsArray()
  months: string[];
}
```
### 2.1 target-rule.dto.ts
```
import { ApiProperty } from '@nestjs/swagger';
import { IsNumber, IsArray, IsUUID } from 'class-validator';

export class TargetRuleDto {
  @ApiProperty()
  @IsNumber()
  monthly_target: number;

  @ApiProperty()
  @IsNumber()
  yearly_target: number;

  @ApiProperty({ type: [String] })
  @IsArray()
  @IsUUID('4', { each: true })
  branchIds: string[];

  @ApiProperty({ type: [String] })
  @IsArray()
  months: string[];
}

```

### 2.2 question-target.dto.ts
```ts
import { ApiProperty } from '@nestjs/swagger';
import { Type } from 'class-transformer';
import { IsUUID, IsArray, ValidateNested } from 'class-validator';
import { TargetRuleDto } from './target-rule.dto';

export class QuestionTargetDto {
  @ApiProperty()
  @IsUUID()
  question_id: string;

  @ApiProperty({ type: [TargetRuleDto] })
  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => TargetRuleDto)
  data: TargetRuleDto[];
}
```

### 2.3 create-target-entry.dto.ts
```ts
import { ApiProperty } from '@nestjs/swagger';
import { Type } from 'class-transformer';
import { IsUUID, IsArray, ValidateNested } from 'class-validator';
import { QuestionTargetDto } from './question-target.dto';

export class CreateTargetEntryDto {
  @ApiProperty()
  @IsUUID()
  year_id: string;

  @ApiProperty()
  @IsUUID()
  component_id: string;

  @ApiProperty()
  @IsUUID()
  form_id: string;

  @ApiProperty({ type: [QuestionTargetDto] })
  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => QuestionTargetDto)
  question_wise_target: QuestionTargetDto[];
}
```

### 2.4 create-target-setup.dto.ts
```ts
import { ApiProperty } from '@nestjs/swagger';
import { Type } from 'class-transformer';
import { IsArray, ValidateNested } from 'class-validator';
import { CreateTargetEntryDto } from './create-target-entry.dto';

export class CreateTargetSetupDto {
  @ApiProperty({ type: [CreateTargetEntryDto] })
  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => CreateTargetEntryDto)
  entries: CreateTargetEntryDto[];
}
```
Note: The existing TargetSetupResponseDto can remain unchanged – it already matches the flat row structure returned by GET endpoints.

# Step 3: Controller Design
File: src/modules/target-setup/target-setup.controller.ts

Update the create method to accept the new CreateTargetSetupDto. The GET methods stay exactly as they are – they already call service methods that return flat arrays.

```ts
import { /* existing imports */ } from '...';
import { CreateTargetSetupDto } from './dtos/create-target-setup.dto'; // new DTO

@ApiTags('Target Setup APIs')
@Controller('target-setup')
export class TargetSetupController {
  constructor(private readonly targetSetupService: TargetSetupService) {}

  @Post()
  @HttpCode(HttpStatus.OK)
  @ApiGuard()
  @ApiOkResponse({ type: () => SwaggerResponseType(TargetSetupResponseDto, true) })
  @UseInterceptors(LogsInterceptor)
  public create(
    @Body() body: CreateTargetSetupDto,   // <-- new DTO
    @CurrentUser('user_id') userId: string,
  ): Observable<{ result: string; resultset: any }> {
    return this.targetSetupService.create(body, userId).pipe(
      map(result => ({
        result: 'ok',
        resultset: result,
      })),
    );
  }

  // GET methods remain unchanged
  // ...
}
```
No changes needed for the GET endpoints – they already use the correct query parameters and service methods.

# Step 4: Service Design
File: src/modules/target-setup/target-setup.service.ts

Replace the entire service with the implementation below. It uses plain strings for UUIDs to avoid ORM conversion issues, and includes duplicate prevention.

```ts
import { Injectable, BadRequestException } from '@nestjs/common';
import { InjectRepository, Repository } from '@iaminfinity/express-cassandra';
import { from, Observable, of } from 'rxjs';
import { map, mergeMap, toArray, reduce } from 'rxjs/operators';
import { v4 as uuidv4 } from 'uuid'; // install: npm install uuid @types/uuid
import { CreateTargetSetupDto } from './dtos/create-target-setup.dto';
import { TargetSetupEntity } from './entities/target-setup.entity';
import { isValidUuid } from './utils/uuid-validation.util'; // keep your existing validator

@Injectable()
export class TargetSetupService {
  constructor(
    @InjectRepository(TargetSetupEntity)
    private readonly targetRepo: Repository<TargetSetupEntity>,
  ) {}

  create(dto: CreateTargetSetupDto, userId: string): Observable<any> {
    const rows: TargetSetupEntity[] = [];
    const usedCombos = new Set<string>(); // track (year,question,branch,month) within this request

    for (const entry of dto.entries) {
      const yearId = entry.year_id;
      if (!isValidUuid(yearId)) throw new BadRequestException('Invalid year_id');

      const componentId = entry.component_id;
      if (!isValidUuid(componentId)) throw new BadRequestException('Invalid component_id');

      const formId = entry.form_id;
      if (!isValidUuid(formId)) throw new BadRequestException('Invalid form_id');

      for (const qt of entry.question_wise_target) {
        const questionId = qt.question_id;
        if (!isValidUuid(questionId)) throw new BadRequestException('Invalid question_id');

        for (const rule of qt.data) {
          const ruleId = uuidv4(); // generate a unique rule_id for this group

          for (const branchId of rule.branchIds) {
            if (!isValidUuid(branchId)) throw new BadRequestException('Invalid branch_id in rule');

            for (const month of rule.months) {
              const comboKey = `${yearId}|${questionId}|${branchId}|${month}`;
              if (usedCombos.has(comboKey)) {
                throw new BadRequestException(
                  `Duplicate target definition for year ${yearId}, question ${questionId}, branch ${branchId}, month ${month}`,
                );
              }
              usedCombos.add(comboKey);

              const entity = this.targetRepo.create({
                year_id: yearId,            // plain string – ORM accepts it
                component_id: componentId,
                form_id: formId,
                question_id: questionId,
                branch_id: branchId,
                month,
                yearly_target: rule.yearly_target,
                monthly_target: rule.monthly_target,
                rule_id: ruleId,
                created_by: userId,         // plain string
                updated_by: userId,
              });
              rows.push(entity);
            }
          }
        }
      }
    }

    // Save all rows
    return from(rows).pipe(
      mergeMap(entity => this.targetRepo.save(entity)),
      toArray(),
      map(saved => ({
        created: saved.length,
        ruleIds: [...new Set(saved.map(r => r.rule_id))], // unique rule IDs
      })),
    );
  }

  // ---- Query methods (remain largely the same) ----

  findByYear(yearId: string): Observable<TargetSetupEntity[]> {
    if (!isValidUuid(yearId)) throw new BadRequestException('Invalid year_id');
    return this.targetRepo.find({ year_id: yearId }, { raw: true });
  }

  findByYearAndQuestion(yearId: string, questionId: string): Observable<TargetSetupEntity[]> {
    if (!isValidUuid(yearId) || !isValidUuid(questionId)) throw new BadRequestException('Invalid UUID');
    return this.targetRepo.find(
      { year_id: yearId, question_id: questionId },
      { raw: true }
    );
  }

  findByYearQuestionAndBranch(yearId: string, questionId: string, branchId: string): Observable<TargetSetupEntity[]> {
    if (!isValidUuid(yearId) || !isValidUuid(questionId) || !isValidUuid(branchId)) throw new BadRequestException('Invalid UUID');
    return this.targetRepo.find(
      { year_id: yearId, question_id: questionId, branch_id: branchId },
      { raw: true }
    );
  }

  // Optional: find by rule_id using materialized view
  findByRuleId(ruleId: string): Observable<TargetSetupEntity[]> {
    if (!isValidUuid(ruleId)) throw new BadRequestException('Invalid rule_id');
    return this.targetRepo.find(
      { rule_id: ruleId },
      { materialized_view: 'target_setup_by_rule', raw: true }
    );
  }

  // findById can remain if needed
  findById(id: string): Observable<TargetSetupEntity | null> {
    if (!isValidUuid(id)) throw new BadRequestException('Invalid id');
    return this.targetRepo.findOne({ id }, { raw: true });
  }
}
```

# Step 5: Module Update
File: src/modules/target-setup/target-setup.module.ts

Update the module to import only the new entity (remove any old entities). It should look like this:

```ts
import { Module } from '@nestjs/common';
import { ExpressCassandraModule } from '@iaminfinity/express-cassandra';
import { TargetSetupController } from './target-setup.controller';
import { TargetSetupService } from './target-setup.service';
import { TargetSetupEntity } from './entities/target-setup.entity';

@Module({
  imports: [
    ExpressCassandraModule.forFeature([TargetSetupEntity], 'idp'),
  ],
  controllers: [TargetSetupController],
  providers: [TargetSetupService],
  exports: [TargetSetupService],
})
export class TargetSetupModule {}
```

# Step 6: Testing
After applying all changes, restart your application and test the endpoints.

POST /target-setup with your sample payload (only one entry, one rule). Expect a response like:
```ts
{
  "result": "ok",
  "resultset": {
    "created": 1,          // number of rows created (for your sample: 1)
    "ruleIds": ["generated-uuid-here"]
  }
}
```
GET /target-setup?year_id=... should return a flat array of rows, matching the old response format.

If you encounter any uuid object errors, double‑check that you are passing plain strings to this.targetRepo.create(). The isValidUuid function should remain as a validator, not a converter.
