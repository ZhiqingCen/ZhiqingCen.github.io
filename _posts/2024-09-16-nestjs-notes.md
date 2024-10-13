---
title: NestJS Notes
date: 2024-09-16 10:00:00 +1000
categories: [Programming, NestJS]
tags: [nestjs, backend development, nodejs]
description: "Notes on building scalable server-side applications using NestJS, exploring its modular architecture, decorators, and powerful dependency injection system."
---


# NestJS Notes

## Start Project

```shell
# create new project
$ nest new [project_name]

# start the development server at http://localhost:3000
$ npm run start:dev
```

- `src/` - contains the main application code, including modules, controllers, and services
- `app.module.ts` - the root module that organizes other modules
- `app.controller.ts` - contains the API route handlers
- `app.service.ts` - a service that handles the business logic

```shell
# generate moduel, service and controller
$ nest g module [name]
$ nest g service [name]
$ nest g controller [name]
```

Steps to add new feature

1. define entity with strucutre of feature item
2. implement service to handle the creation of a new feature item
3. implement controller to handle API request for creating a new feature item
4. register module in the main application module

- `@Injectable()`
  - purpose: marks a class as a service or provider that can be managed by NestJS’s dependency injection system
  - usage: applied to any class that should be injected as a dependency into other classes, such as services or repositories
- `@Controller('[route]')`
  - purpose: marks a class as a controller that will handle incoming HTTP requests and send responses back to the client
  - usage: applied to classes that define HTTP request handling methods (like GET, POST, PUT, DELETE) for a specific route

**Database**

The `ConfigModule` will load the environment variables from the `.env` file, making them accessible via the `ConfigService`

- `ConfigModule.forRoot()`: loads environment variables from the `.env` file. `Setting isGlobal: true` makes the configuration available throughout the entire application without needing to import it repeatedly
- `TypeOrmModule.forRootAsync()`: asynchronously sets up the TypeORM connection using the `ConfigService` to inject environment variables
- `autoLoadEntities`: automatically loads all entities in the application. This is useful during development
- `synchronize`: automatically synchronizes the database schema with your entities. Be cautious when using this in production as it may cause data loss
