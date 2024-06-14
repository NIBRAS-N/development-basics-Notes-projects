# Key Concepts

  - `Controller:` Handles incoming HTTP requests and returns responses to the client. Controllers are responsible for defining the routes and handling the HTTP methods like GET, POST, PUT, and DELETE.

  - `Service:` Contains the business logic of the application. Services are used to handle data processing, database interactions, and any other operations that need to be abstracted from the controllers.

  - `Provider:` Any class that can be injected into another class using dependency injection. Services, repositories, and factories can be providers.

  - `Module:` A module is a class annotated with the @Module decorator. It is used to organize related components like controllers, services, and providers into a single cohesive unit. Modules are the basic building blocks of a NestJS application.

# Simple CRUD Operation

### Step 1: Install NestJS CLI and Create a New Project
```CLI
  npm i -g @nestjs/cli
  nest new crud-example
  cd crud-example
```
### Step 2:Generate a Module, Controller, and Service
```CLI
nest generate module items
nest generate controller items
nest generate service items 
```
### Step 3: Define the Item Entity
```ts
// src/items/item.entity.ts
import { IsString, IsNotEmpty, IsOptional, IsNumber } from 'class-validator';

export class Item {
  @IsNumber()
  id: number;

  @IsString()
  @IsNotEmpty()
  name: string;

  @IsString()
  @IsOptional()
  description?: string;
}

```
### Step 4: Implement the Items Service


```ts
// src/items/items.service.ts
import { Injectable } from '@nestjs/common';
import { Item } from './item.entity';

@Injectable()
export class ItemsService {
  private items: Item[] = [];
  private idCounter = 1;

  findAll(): Item[] {
    return this.items;
  }

  findOne(id: number): Item {
    return this.items.find(item => item.id === id);
  }

  create(item: Item): Item {
    item.id = this.idCounter++;
    this.items.push(item);
    return item;
  }

  update(id: number, updateItem: Partial<Item>): Item {
    const item = this.findOne(id);
    if (item) {
      Object.assign(item, updateItem);
    }
    return item;
  }

  remove(id: number): void {
    this.items = this.items.filter(item => item.id !== id);
  }
}

```


### Step 5: Implement the Items Controller
      Routes are:
      GET /items - Retrieve all items
      GET /items/:id - Retrieve a specific item by id
      POST /items - Create a new item (send a JSON body with name and description)
      PUT /items/:id - Update an existing item (send a JSON body with name and/or description)
      DELETE /items/:id - Delete an item by id
```ts
// src/items/items.controller.ts
import { Controller, Get, Post, Put, Delete, Param, Body } from '@nestjs/common';
import { ItemsService } from './items.service';
import { Item } from './item.entity';

@Controller('items')
export class ItemsController {
  constructor(private readonly itemsService: ItemsService) {}

  @Get()
  findAll(): Item[] {
    return this.itemsService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: number): Item {
    return this.itemsService.findOne(id);
  }

  @Post()
  create(@Body() item: Item): Item {
    return this.itemsService.create(item);
  }

  @Put(':id')
  update(@Param('id') id: number, @Body() updateItem: Partial<Item>): Item {
    return this.itemsService.update(id, updateItem);
  }

  @Delete(':id')
  remove(@Param('id') id: number): void {
    this.itemsService.remove(id);
  }
}

```
### Step 6: Register the Items Module

```TS
// src/app.module.ts
import { Module } from '@nestjs/common';
import { ItemsModule } from './items/items.module';

@Module({
  imports: [ItemsModule],
  controllers: [],
  providers: [],
})
export class AppModule {}

```

