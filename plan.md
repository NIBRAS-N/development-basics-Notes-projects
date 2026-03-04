### We'll create a single‑entity module mis-configuration with soft delete.
### The entity contains all fields, and we'll use a composite primary key with a materialized view for form_id lookups. 
### Below are the step‑by‑step instructions for Cursor Composer.

## Step 1: Generate the Module and Folder Structure
creates mis-configuration.module.ts in src/modules/mis-configuration/.
Inside that folder, create the following subfolders:

- entities
- dtos
- controllers
- services

## Step 2: Create the Entity (Single Table)
File: src/modules/mis-configuration/entities/mis-configuration.entity.ts

```ts
import { Entity, Column, GeneratedUuidColumn } from '@iaminfinity/express-cassandra';
import { BaseEntity } from '../../../shared/entities/base-entity';
import { ApiHideProperty } from '@nestjs/swagger'; // to hide soft delete fields from Swagger

@Entity({
  table_name: 'mis_configuration',
  // Partition key: (component_id, zone_id, year_id) allows querying by these three
  // Clustering column: form_id ensures uniqueness and ordering
  key: [['component_id', 'zone_id', 'year_id'], 'form_id'],
  // Materialized view to allow direct lookup by form_id
  materialized_views: {
    mis_configuration_by_form: {
      key: [['form_id'], 'component_id', 'zone_id', 'year_id'],
      select: ['*'],
    },
  },
})
export class MisConfigurationEntity extends BaseEntity {
  @GeneratedUuidColumn()
  id?: any; // auto-generated, not part of primary key

  @Column({ type: 'uuid' })
  component_id: any;

  @Column({ type: 'uuid' })
  zone_id: any;

  @Column({ type: 'uuid' })
  year_id: any;

  @Column({ type: 'uuid' })
  form_id: any;

  @Column({ type: 'boolean' })
  user_to_add_yearly_target: boolean;

  @Column({ type: 'list<uuid>' })
  selected_user_to_add_yearly_target: any[];

  @Column({ type: 'boolean' })
  user_to_add_monthly_target: boolean;

  @Column({ type: 'list<uuid>' })
  selected_user_to_add_monthly_target: any[];

  @Column({ type: 'boolean' })
  user_to_edit_existing_item: boolean;

  @Column({ type: 'list<uuid>' })
  selected_user_to_edit_existing_item: any[];

  @Column({ type: 'boolean' })
  required_yearly_target: boolean;

  @Column({ type: 'list<uuid>' })
  users_required_yearly_target: any[];

  @Column({ type: 'boolean' })
  required_monthly_target: boolean;

  @Column({ type: 'list<uuid>' })
  users_required_monthly_target: any[];

  @Column({ type: 'boolean' })
  required_sub_question: boolean;

  // Soft delete fields – hidden from Swagger
  @ApiHideProperty()
  @Column({ type: 'boolean', default: false })
  is_deleted: boolean;

  @ApiHideProperty()
  @Column({ type: 'timestamp', default: null })
  deleted_at?: Date;

  @ApiHideProperty()
  @Column({ type: 'uuid', default: null })
  deleted_by?: any;
}
```

#### Explanation:

- Primary key allows efficient queries by (component_id, zone_id, year_id) and optionally filtered by form_id.

- Materialized view mis_configuration_by_form lets us fetch a single record by form_id directly.

- Soft delete fields are hidden from Swagger using @ApiHideProperty().

- All UUID columns accept plain strings – the ORM will convert them


### Step 3: Create DTOs
We need DTOs for creating and updating configurations. For responses, we can reuse the entity but omit soft delete fields from the Swagger documentation. 
We'll create a base DTO and extend it.

 - 3.1 create-configuration.dto.ts
```ts
import { ApiProperty } from '@nestjs/swagger';
import { IsUUID, IsBoolean, IsArray, IsOptional } from 'class-validator';

export class CreateConfigurationDto {
  @ApiProperty()
  @IsUUID()
  component_id: string;

  @ApiProperty()
  @IsUUID()
  zone_id: string;

  @ApiProperty()
  @IsUUID()
  year_id: string;

  @ApiProperty()
  @IsUUID()
  form_id: string;

  @ApiProperty()
  @IsBoolean()
  required_yearly_target: boolean;

  @ApiProperty({ type: [String], required: false })
  @IsArray()
  @IsUUID('4', { each: true })
  @IsOptional()
  selected_user_to_add_yearly_target?: string[];

  @ApiProperty()
  @IsBoolean()
  required_monthly_target: boolean;

  @ApiProperty({ type: [String], required: false })
  @IsArray()
  @IsUUID('4', { each: true })
  @IsOptional()
  selected_user_to_add_monthly_target?: string[];

  @ApiProperty()
  @IsBoolean()
  required_edit_existing_item: boolean;

  @ApiProperty({ type: [String], required: false })
  @IsArray()
  @IsUUID('4', { each: true })
  @IsOptional()
  selected_user_to_edit_existing_item?: string[];

  @ApiProperty()
  @IsBoolean()
  required_sub_question: boolean;
}
```

### 3.2 update-configuration.dto.ts
All fields optional except we need to identify which record to update. 
We'll use a partial type and require form_id in the URL parameter, not in the body.
```ts
import { PartialType } from '@nestjs/swagger';
import { CreateConfigurationDto } from './create-configuration.dto';

export class UpdateConfigurationDto extends PartialType(CreateConfigurationDto) {}
```

### 3.3 Response DTO (optional) – we can use the entity directly but with soft delete fields hidden. However, we might want to exclude them from the API response entirely. We'll create a response DTO that omits those fields.

File: src/modules/mis-configuration/dtos/mis-configuration-response.dto.ts

```
import { ApiProperty } from '@nestjs/swagger';
import { BaseDTO } from '../../../shared/dtos/base.dto'; // adjust path

export class MisConfigurationResponseDto extends BaseDTO {
import { ApiProperty } from '@nestjs/swagger';
import { BaseDTO } from '../../../shared/dtos/base.dto'; // adjust path

export class MisConfigurationResponseDto extends BaseDTO {
  @ApiProperty()
  id: string;

  @ApiProperty()
  component_id: string;

  @ApiProperty()
  zone_id: string;

  @ApiProperty()
  year_id: string;

  @ApiProperty()
  form_id: string;

  @ApiProperty()
  required_yearly_target: boolean;

  @ApiProperty({ type: [String] })
  selected_user_to_add_yearly_target: string[];

  @ApiProperty()
  required_monthly_target: boolean;

  @ApiProperty({ type: [String] })
  selected_user_to_add_monthly_target: string[];

  @ApiProperty()
  required_edit_existing_item: boolean;

  @ApiProperty({ type: [String] })
  selected_user_to_edit_existing_item: string[];

  @ApiProperty()
  required_sub_question: boolean;
}
```

### Step 4: Create the Service
File: src/modules/mis-configuration/services/mis-configuration.service.ts

```
import { Injectable, BadRequestException, NotFoundException } from '@nestjs/common';
import { InjectRepository, Repository } from '@iaminfinity/express-cassandra';
import { from, Observable, of } from 'rxjs';
import { map, mergeMap, catchError } from 'rxjs/operators';
import { MisConfigurationEntity } from '../entities/mis-configuration.entity';
import { CreateConfigurationDto } from '../dtos/create-configuration.dto';
import { UpdateConfigurationDto } from '../dtos/update-configuration.dto';
import { MisConfigurationResponseDto } from '../dtos/mis-configuration-response.dto';
import { isValidUuid } from '../../../shared/utils/uuid-validation.util'; // adjust path

@Injectable()
export class MisConfigurationService {
  constructor(
    @InjectRepository(MisConfigurationEntity)
    private readonly configRepo: Repository<MisConfigurationEntity>,
  ) {}

  private toResponse(entity: MisConfigurationEntity): MisConfigurationResponseDto {
    return {
      id: entity.id,
      component_id: entity.component_id,
      zone_id: entity.zone_id,
      year_id: entity.year_id,
      form_id: entity.form_id,
      required_yearly_target: entity.required_yearly_target,
      selected_user_to_add_yearly_target: entity.selected_user_to_add_yearly_target || [],
      required_monthly_target: entity.required_monthly_target,
      selected_user_to_add_monthly_target: entity.selected_user_to_add_monthly_target || [],
      required_edit_existing_item: entity.required_edit_existing_item,
      selected_user_to_edit_existing_item: entity.selected_user_to_edit_existing_item || [],
      required_sub_question: entity.required_sub_question,
      created_at: entity.created_at,
      updated_at: entity.updated_at,
      created_by: entity.created_by,
      updated_by: entity.updated_by,
    };
  }

  create(dto: CreateConfigurationDto, userId: string): Observable<MisConfigurationResponseDto> {
    ['component_id', 'zone_id', 'year_id', 'form_id'].forEach(field => {
      if (!isValidUuid(dto[field])) throw new BadRequestException(`Invalid ${field}`);
    });

    const yearlyList = dto.required_yearly_target ? dto.selected_user_to_add_yearly_target || [] : [];
    const monthlyList = dto.required_monthly_target ? dto.selected_user_to_add_monthly_target || [] : [];
    const editList = dto.required_edit_existing_item ? dto.selected_user_to_edit_existing_item || [] : [];

    const entity = this.configRepo.create({
      component_id: dto.component_id,
      zone_id: dto.zone_id,
      year_id: dto.year_id,
      form_id: dto.form_id,
      required_yearly_target: dto.required_yearly_target,
      selected_user_to_add_yearly_target: yearlyList,
      required_monthly_target: dto.required_monthly_target,
      selected_user_to_add_monthly_target: monthlyList,
      required_edit_existing_item: dto.required_edit_existing_item,
      selected_user_to_edit_existing_item: editList,
      required_sub_question: dto.required_sub_question,
      created_by: userId,
      updated_by: userId,
      is_deleted: false,
    });

    return from(this.configRepo.save(entity)).pipe(
      map(saved => this.toResponse(saved)),
      catchError(err => { throw new BadRequestException('Create failed: ' + err.message); })
    );
  }

  findAll(
    componentId: string,
    zoneId: string,
    yearId: string,
  ): Observable<MisConfigurationResponseDto[]> {
    [componentId, zoneId, yearId].forEach(id => {
      if (!isValidUuid(id)) throw new BadRequestException('Invalid UUID');
    });

    return this.configRepo.find(
      { component_id: componentId, zone_id: zoneId, year_id: yearId, is_deleted: false },
      { raw: true }
    ).pipe(
      map(entities => entities.map(e => this.toResponse(e))),
    );
  }

  findOneByFormId(formId: string): Observable<MisConfigurationResponseDto | null> {
    if (!isValidUuid(formId)) throw new BadRequestException('Invalid form_id');

    return this.configRepo.find(
      { form_id: formId, is_deleted: false },
      { materialized_view: 'mis_configuration_by_form', raw: true }
    ).pipe(
      map(entities => entities.length ? this.toResponse(entities[0]) : null),
    );
  }

  update(
    formId: string,
    dto: UpdateConfigurationDto,
    userId: string,
  ): Observable<MisConfigurationResponseDto> {
    if (!isValidUuid(formId)) throw new BadRequestException('Invalid form_id');

    return this.findOneByFormId(formId).pipe(
      mergeMap(existing => {
        if (!existing) throw new NotFoundException('Configuration not found');

        return this.configRepo.findOne(
          { form_id: formId },
          { materialized_view: 'mis_configuration_by_form', raw: true }
        ).pipe(
          mergeMap(fullEntity => {
            if (!fullEntity) throw new NotFoundException('Configuration not found');

            const updatedData: any = { updated_by: userId, updated_at: new Date() };

            if (dto.component_id !== undefined) {
              if (!isValidUuid(dto.component_id)) throw new BadRequestException('Invalid component_id');
              updatedData.component_id = dto.component_id;
            }
            if (dto.zone_id !== undefined) {
              if (!isValidUuid(dto.zone_id)) throw new BadRequestException('Invalid zone_id');
              updatedData.zone_id = dto.zone_id;
            }
            if (dto.year_id !== undefined) {
              if (!isValidUuid(dto.year_id)) throw new BadRequestException('Invalid year_id');
              updatedData.year_id = dto.year_id;
            }
            if (dto.required_yearly_target !== undefined) {
              updatedData.required_yearly_target = dto.required_yearly_target;
              updatedData.selected_user_to_add_yearly_target = dto.required_yearly_target
                ? dto.selected_user_to_add_yearly_target || []
                : [];
            }
            if (dto.required_monthly_target !== undefined) {
              updatedData.required_monthly_target = dto.required_monthly_target;
              updatedData.selected_user_to_add_monthly_target = dto.required_monthly_target
                ? dto.selected_user_to_add_monthly_target || []
                : [];
            }
            if (dto.required_edit_existing_item !== undefined) {
              updatedData.required_edit_existing_item = dto.required_edit_existing_item;
              updatedData.selected_user_to_edit_existing_item = dto.required_edit_existing_item
                ? dto.selected_user_to_edit_existing_item || []
                : [];
            }
            if (dto.required_sub_question !== undefined) {
              updatedData.required_sub_question = dto.required_sub_question;
            }

            const updatedEntity = { ...fullEntity, ...updatedData };
            return from(this.configRepo.save(updatedEntity)).pipe(
              map(saved => this.toResponse(saved))
            );
          })
        );
      })
    );
  }

  softDelete(formId: string, userId: string): Observable<void> {
    if (!isValidUuid(formId)) throw new BadRequestException('Invalid form_id');

    return this.configRepo.findOne(
      { form_id: formId },
      { materialized_view: 'mis_configuration_by_form', raw: true }
    ).pipe(
      mergeMap(entity => {
        if (!entity) throw new NotFoundException('Configuration not found');
        if (entity.is_deleted) return of(void 0);

        entity.is_deleted = true;
        entity.deleted_at = new Date();
        entity.deleted_by = userId;
        entity.updated_by = userId;
        entity.updated_at = new Date();

        return from(this.configRepo.save(entity)).pipe(map(() => void 0));
      })
    );
  }

  restore(formId: string, userId: string): Observable<MisConfigurationResponseDto> {
    if (!isValidUuid(formId)) throw new BadRequestException('Invalid form_id');

    return this.configRepo.findOne(
      { form_id: formId },
      { materialized_view: 'mis_configuration_by_form', raw: true }
    ).pipe(
      mergeMap(entity => {
        if (!entity) throw new NotFoundException('Configuration not found');
        if (!entity.is_deleted) return of(this.toResponse(entity));

        entity.is_deleted = false;
        entity.deleted_at = null;
        entity.deleted_by = null;
        entity.updated_by = userId;
        entity.updated_at = new Date();

        return from(this.configRepo.save(entity)).pipe(map(saved => this.toResponse(saved)));
      })
    );
  }
}
```

Step 5: Create the Controller
File: src/modules/mis-configuration/controllers/mis-configuration.controller.ts

```ts
import {
  Controller, Post, Get, Patch, Delete, Body, Param, Query,
  HttpCode, HttpStatus, UseInterceptors, NotFoundException, BadRequestException
} from '@nestjs/common';
import { ApiTags, ApiOkResponse, ApiBearerAuth, ApiQuery } from '@nestjs/swagger';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';
import { MisConfigurationService } from '../services/mis-configuration.service';
import { CreateConfigurationDto } from '../dtos/create-configuration.dto';
import { UpdateConfigurationDto } from '../dtos/update-configuration.dto';
import { MisConfigurationResponseDto } from '../dtos/mis-configuration-response.dto';
import { CurrentUser } from 'libs/auth/src/decorators/current-user.decorator'; // adjust
import { ApiGuard } from 'libs/auth/src/decorators/api.guard.decorator'; // adjust
import { LogsInterceptor } from '@idp/common'; // adjust

@ApiTags('MIS Configuration')
@Controller('mis-configuration')
@ApiBearerAuth()
export class MisConfigurationController {
  constructor(private readonly configService: MisConfigurationService) {}

  @Post()
  @HttpCode(HttpStatus.OK)
  @ApiGuard()
  @ApiOkResponse({ type: MisConfigurationResponseDto })
  @UseInterceptors(LogsInterceptor)
  create(
    @Body() dto: CreateConfigurationDto,
    @CurrentUser('user_id') userId: string,
  ): Observable<MisConfigurationResponseDto> {
    return this.configService.create(dto, userId);
  }

  @Get()
  @HttpCode(HttpStatus.OK)
  @ApiGuard()
  @ApiOkResponse({ type: [MisConfigurationResponseDto] })
  @ApiQuery({ name: 'component_id', required: true, type: String })
  @ApiQuery({ name: 'zone_id', required: true, type: String })
  @ApiQuery({ name: 'year_id', required: true, type: String })
  findAll(
    @Query('component_id') componentId: string,
    @Query('zone_id') zoneId: string,
    @Query('year_id') yearId: string,
  ): Observable<MisConfigurationResponseDto[]> {
    if (!componentId || !zoneId || !yearId) {
      throw new BadRequestException('component_id, zone_id, and year_id are required');
    }
    return this.configService.findAll(componentId, zoneId, yearId);
  }

  @Get(':formId')
  @HttpCode(HttpStatus.OK)
  @ApiGuard()
  @ApiOkResponse({ type: MisConfigurationResponseDto })
  findOne(@Param('formId') formId: string): Observable<MisConfigurationResponseDto> {
    return this.configService.findOneByFormId(formId).pipe(
      map(config => {
        if (!config) throw new NotFoundException('Configuration not found');
        return config;
      })
    );
  }

  @Patch(':formId')
  @HttpCode(HttpStatus.OK)
  @ApiGuard()
  @ApiOkResponse({ type: MisConfigurationResponseDto })
  @UseInterceptors(LogsInterceptor)
  update(
    @Param('formId') formId: string,
    @Body() dto: UpdateConfigurationDto,
    @CurrentUser('user_id') userId: string,
  ): Observable<MisConfigurationResponseDto> {
    return this.configService.update(formId, dto, userId);
  }

  @Delete(':formId')
  @HttpCode(HttpStatus.OK)
  @ApiGuard()
  @ApiOkResponse({ description: 'Soft delete successful' })
  @UseInterceptors(LogsInterceptor)
  delete(
    @Param('formId') formId: string,
    @CurrentUser('user_id') userId: string,
  ): Observable<{ message: string }> {
    return this.configService.softDelete(formId, userId).pipe(
      map(() => ({ message: 'Deleted successfully' }))
    );
  }

  @Patch(':formId/restore')
  @HttpCode(HttpStatus.OK)
  @ApiGuard()
  @ApiOkResponse({ type: MisConfigurationResponseDto })
  @UseInterceptors(LogsInterceptor)
  restore(
    @Param('formId') formId: string,
    @CurrentUser('user_id') userId: string,
  ): Observable<MisConfigurationResponseDto> {
    return this.configService.restore(formId, userId);
  }
}

```

### Step 6: Update the Module

### Step 7: UUID Validation Utility
If you don't already have a shared UUID validator, create it:

File: src/shared/utils/uuid-validation.util.ts


Step 8: Testing
provide every apis body and response to test in postman.

Test with Postman or similar:


