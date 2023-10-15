# NEST JS

- [Manual set up](#manual-set-up)
- [CLI](#cli)
- [Main file](#main-file)
- [Controllers](#controllers)
- [Modules](#modules)
- [Services](#services)
- [Repositories](#repositories)
- [Pipes](#pipes)
- [Entities](#entities)
- [Interceptors](#interceptors)
- [Guards](#guards)
- [Error response](#error-responses)
- [Unit Testing](#unit-testing)
- [e2e Testing](#e2e-testing)
- [App configuration](#app-configuration)

## Manual set up

Creating a NestJS project from scratch:  

- Create a folder and run `npm init -y` inside of it
- Run the following command to install the dependencies :  

```bash
npm install 
@nestjs/common@7.6.17 
@nestjs/core@7.6.17 
@nestjs/platform-express@7.6.17 
reflect-metadata@0.1.13 
typescript@4.3.2
```

## CLI

The NestJS CLI makes it easier to generate files.

```bash
npm i -g @nestjs/cli
```

### Generate files

- Module : `nest generate | g module moduleName` // will create a module file. Will also create a module folder if none exists  
- Controller : `nest generate | g controller folerName/controllerName --flat` // will create a controller file inside the desired folder. If no folder is provided then a new folder will be created. The **--flat** flag will prevent the creation of an extra _controller_ folder.
- Service : `nest generate | g service service name`

## Main file

This file is the entry to the app.

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap(){
  const app = await NestFactory.create(AppModule) // will use the Main Module to run the app

  await app.listen(3000) // can be any port
}

bootstrap() // will bootstrap this application by calling the bootstrap function
```

## Controllers

Controllers will handle the incoming request from the client:  

- [Get request](#get-request)
- [Post request](#post-request)
- [Patch request](#patch-request)
- [Delete request](#delete-request)

### Get request

```typescript
import {Controller, Get} from @nestjs/common

@Controller() // this decorator tells nestjs that it wants to create a Controller
export class AppController{
  @Get() // this decorator tells nestjs that it wants to create a Get request
  getMainRoute(){
    return this.serviceName.getAll()
  }

  @Get('/:id')
  getOne(@Param('id') id: string){
    return this.serviceName.getOne(id)
  }
}
```

### Post request

```typescript
import {Controller, Body, Post} from @nestjs/common

@Controller() 
export class AppController{
  @Post() // this decorator tells nestjs that it wants to create a Post request
  createItem(@Body() body: Type){
    return this.serviceName.create(body)
  }
}
```

### Patch request

```typescript
import {Controller, Body, Patch} from @nestjs/common

@Controller() 
export class AppController{
  @Patch() // this decorator tells nestjs that it wants to create a Patch request
  createItem(@Body() body: Type){
    return this.serviceName.update(body)
  }
}
```

### Delete request

```typescript
import {Controller, Param, Delete} from @nestjs/common

@Controller() 
export class AppController{
  @Patch('/:id') // this decorator tells nestjs that it wants to create a Delete request
  createItem(@Param('id') id: Type){
    return this.serviceName.delete(id)
  }
}
```

## Modules

NestJS needs at least one **Module** file to run the application:  

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';

@Module({
  controllers: [AppController]
})
export class AppModule{}
```

## Services

## Repositories

The **Repositoty** file will be the one interacting with the **database**.

### TypeORM

**TypeORM** will create the **repository** file for us.

## Pipes

**Pipes** can be used to validate data on incoming requests.  
The ValidationPipe module is needed as well as the **class-validator** and the **class-transformer** packages.  

### class-validator

The **class-validator** package will be responsible for checking the incoming request against a DTO model (Data Transfer Object).  

### class-transformer

The **class-transformer** will modify the DTO.

Decorators:  

- @Exclude() // will remove a property when turning an instance of an Entity to turn it into a JSON object.

```typescript
import { Exclude } from 'class-transformer';
import {Entity, Column, PrimaryGeneratedColumn, AfterInsert, AfterUpdate, AfterRemove} from 'typeorm';

@Entity()
export class User{
  @PrimaryGeneratedColumn()
  id: string;
  
  @Column()
  email: string;
  
  @Column()
  @Exclude() // this property won't be returned when requesting a User
  password: string;
}
```

### Set up the validation

Add the **ValidationPipe** module to the **main.ts** file:

```typescript
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new  ValidationPipe({
    whiteList: true // this property will strip down all keys that are not in the DTO file aswell as all property that is not decorated
  }))
  await app.listen(3000);
}
bootstrap();
```

Install the **class-validator** and the **class-transformer** packages via npm:

`npm install class-validator class-transformer`

Create a DTO file to define the request object type:  

```typescript
import { IsEmail, IsString } from 'class-validator';

export class CreateUserDto{
  @IsEmail()
  email: number;
  
  @IsString()
  password: string;
}
```

In the Controller file add the validation:  

```typescript
import { Body, Controller, Get, Post } from '@nestjs/common';
import { CreateUserDto } from './dtos/create-user.dto';

@Controller('auth')
export class UsersController {
  @Post('/signup')
  createUser(@Body() body: CreateUserDto):void{
    console.log(body)
  }
}
```

## Entities

Entities are classes that act like a model. They define the structure of an object:

```typescript
// The following Entity is used with TypeORM
import { Exclude } from 'class-transformer';
import {Entity, Column, PrimaryGeneratedColumn, AfterInsert, AfterUpdate, AfterRemove} from 'typeorm';

export class EntityName{
  @PrimaryGeneratedColumn()
  id: string;
  
  @Column()
  email: string;
  
  @Column()
  @Exclude()
  password: string;

  @AfterInsert()
  logInsert(){
    console.log('User inserted with the following id:', this.id)
  }

  @AfterUpdate()
  logUpdate(){
    console.log('User updated with the following id:', this.id)
  }

  @AfterRemove()
  logRemove(){
    console.log('User removed')
  }
}
```

## Interceptors

**Interceptors** act a bit like **middlewares**. They _intercept_ incoming request and outgoing response. They can then modify those responses.

```typescript
import { CallHandler, ExecutionContext, NestInterceptor, UseInterceptors } from '@nestjs/common';
import { plainToInstance } from 'class-transformer';
import { map, Observable } from 'rxjs';

export function Serialize(dto: any){
  return UseInterceptors(new SerializeInterceptor(dto))
}

export class SerializeInterceptor implements NestInterceptor {

  constructor(private dto: any){}
  
  intercept(context: ExecutionContext, next: CallHandler<any>): Observable<any> | Promise<Observable<any>> {
    // Run something before a request is handled by the request handler
    console.log('Im running before the handler', context)
    
    return next.handle().pipe(
      map((data: any) => {
        // Run something before the response is sent out
        console.log('Im running before the response is sent out', data)
        return plainToInstance(this.dto, data, {
          excludeExtraneousValues: true
          // excludeExtraneousValues: false
        })
      })
    )
  }
}
```

## Guards

**Guards** are classes that will prevent the access to certain routes depending on a condition, usually a role or if the user is signed in.

```typescript
(auth.guard.ts)
import { CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

export class AuthGuard implements CanActivate{
  canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean> {
      const request = context.switchToHttp().getRequest()

      return request.session.userId
  }
}
```

**Importing** the guard file:  

```typescript
(user.controller.ts)
import { AuthGuard } from 'src/guards/auth.guard';

@Controller()
export class UsersController {
  @Get('/someRoute')
  @UseGuards(AuthGuard)
  getSomething(){
    return whatever
  }
}
```

## Error responses

NestJS comes with built in error responses that will handle the error responses: the error code and type.

### NotFoundException()

```typescript
if(!user) {
  throw new NotFoundException('User not found...')
}
```

### BadRequestException()

```typescript
if(!emails.length) {
  throw new BadRequestException('Email already in use...')
}
```

## Unit Testing

- [Unit Test Description](#unit-test-description)
- [Speed up the tests](#speed-up-the-tests)
- [Flow](#flow)

### Unit Test Description

Unit testing is for testing a single file at a time.

Mock required service:  

- create a file or const with the required values returned.

Fake/Mock the services in the beforeEach() method.  
**example here**  
Import a test helper file in the providers array:  
**example here**

### Speed up the tests

=> adding `--maxWorkers=1` will speed up the test and eventually avoid runtime errors:  

```json
(package.json)
{
    "scripts": {
    "test:watch": "cross-env NODE_ENV=test jest --watch --maxWorkers=1",
    "test:e2e": "cross-env NODE_ENV=test jest --config ./test/jest-e2e.json --maxWorkers=1"
  },
}
```

### Flow

- Provider: it is a function or file that can be injected into other classes

Here are some **jest methods**:

- spyOn => will watch a function. Can be used to check if the function was run
-  => 

### Template

```typescript
it('<DESCRIPTION>', async () => {
  // insert code here
})

// Example
it('Here is the description of what the test is testing', async () => {
  const spy = jest.spyOn(service, 'someFunctionName')
  await service.someFunctionName;
  expect(spy).toHaveBeenCalled()
})
```

## e2e Testing

**End-to-end** or **e2e** or **integration testing** tests a flow from one end of the aplication to the other, like a signup route.

Adding the following line to the beforeEach/beforeAll methods will allow the validation pipes to work when making request from the e2e file:

```typescript
app.useGlobalPipes(new ValidationPipe());
await app.init();
```

### Deleting the test database

It is necessary or a good practice to delete the test database before each test to avoid errors.  
Add this line to the **jest-e2e.json** file:

```json
{
  "setupFilesAfterEnv": ["<rootDir>/setup.ts"]
}
```

## App configuration

`npm i @nestjs/config`  
`npm i cross-env`
