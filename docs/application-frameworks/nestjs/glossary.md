# Glossary

A collection of definitions that I found while perusing the Nest documentation. Placed here to try and keep everything straight:

- [Glossary](#glossary)
  - [Controller:](#controller)
  - [Provider:](#provider)
    - [Service:](#service)
      - [Repository:](#repository)
      - [Factory:](#factory)
      - [Helper:](#helper)
  - [Dependency Injection:](#dependency-injection)
  - [Entity:](#entity)
  - [DTO:](#dto)
  - [Schema:](#schema)

## Controller:

Controllers should handle HTTP requests and delegate more complex tasks to providers.

## Provider:

Providers are plain JavaScript classes that are declared as providers in a module. The main idea of a provider is that it can be injected as a dependency; this means objects can create various relationships with each other, and the function of "wiring up" these objects can largely be delegated to the Nest runtime system.

Types of providers:

### Service:

Something responsible for a task, such as data storage and retreival. Used by the controller. Encapsulates the application's business logic. Can interact with databases, perform data processing, implement core functionality. Typically injected into controllers to handle HTTP requests.

#### Repository:

Encapsulate database operations. Often associated with ORM libraries. Provide abstraction layer for database interactions, like querying data.

#### Factory:

Used to create instances of classes or objects dynamically. Helpful when you need to generate objects with complex configurations or when you want to centralize creation of objects with specific dependencies.

#### Helper:

Encompass a broad category of providers that offer utility functions, reusable code, configurations. Not tied to a specific application layer.

A LITTLE EXAMPLE:
You might have a UserService (a service) that relies on UserRepository (a repository) to interact with the database. UserRepository may use a LoggerService (a helper) for for logging and a UserFactory (a factory) for creating user objects with specific configurations.

## Dependency Injection:

Two main roles exist in the DI system: dependency consumer and dependency provider. When a dependency is requested, the injector checks its registry to see if there is an instance already available there. If not, a new instance is created and stored in the registry. Angular creates an application-wide injector (also known as "root" injector) during the application bootstrap process, as well as any other injectors as needed. In most cases you don't need to manually create injectors, but you should know that there is a layer that connects providers and consumers.

## Entity:

Typically represents a data model or an object that can be stored and retrieved from a database. Always a typescript class. Can define both properties and relationships. Used with built-in decorators (@Entity or @Schema, depending on the database type), if appropriate.

## DTO:

An object used to transfer data between parts of your application, such as between clients and server. It's a way to encapsulate and structure data when it's moving between components, helping to avoid transferring unnecessary data and ensuring the data transferred is in the expected format. So an entity and a DTO can look the same, but they don't necessarily look the same.

## Schema:

Another built-in decorator, comparable to an Entity. It marks a class as a schema definition, for example to a MongoDB collection of the same name. (The Nest schema may be Cat, and the Mongo collection may be Cats.) This decorator accepts a single optional argument, the schema options object. 