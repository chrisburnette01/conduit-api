# Conduit API

> API for Real World App (Conduit) using Node.js + TypeScript with Functional Programming

## Required global dependencies

- Node.js v16
- Yarn v1

## Up & Running

1. Install local dependencies:

```terminal
yarn
```

2. Tests (watch mode):

```terminal
yarn test:watch
```

## Scripts

| Script                        | Description                                                   |
| ----------------------------- | ------------------------------------------------------------- |
| `yarn start`                  | Run production server                                         |
| `yarn dev`                    | Get database up and run dev server                            |
| `yarn server`                 | Run only dev server                                           |
| `yarn docker:up`              | Get docker configuration up                                   |
| `yarn docker:down`            | Get docker configuration down                                 |
| `yarn docker:destroy`         | Destroy database and all docker data                          |
| `yarn migrate <name>`         | Run Prisma migration (You must provide a name)                |
| `yarn migration`              | Run existent Prisma migrations                                |
| `yarn migration:prod`         | Run existent Prisma migrations on production                  |
| `yarn test`                   | Run unit and integration tests once (great to be used in CI)  |
| `yarn test:watch`             | Run unit tests in watch mode                                  |
| `yarn test:integration`       | Run integration tests once                                    |
| `yarn test:integration:watch` | Run integration tests in watch mode                           |
| `yarn lint`                   | Run linter                                                    |
| `yarn lint:fix`               | Fix lint errors                                               |
| `yarn type-check`             | TS typechecking                                               |
| `yarn prepare`                | Not suposed to be manually used. It's just to configure husky |
| `yarn build`                  | Generates production build                                    |
| `yarn ci`                     | Run lint, typechecking and tests (meant to be used in a CI)   |

## Tree structure

This project uses Hexagonal Architecture (Ports & Adapters) with Functional Programming.

```terminal
.
├── src
│   ├── config
│   │   ├── tests
│   │   │   └── fixtures
│   │   └── module-alias.ts
│   ├── core
│   │   ├── <domain / entity>
│   │   │   ├── use-cases
│   │   │   │   ├── <use-case>-adapter.ts
│   │   │   │   ├── <use-case>.test.ts
│   │   │   │   ├── <use-case>.ts
│   │   │   └── types.ts
│   │   └── types
│   │       ├── <type>.test.ts
│   │       └── <type>.ts
│   ├── ports
│   │   ├── adapters
│   │   │   └── <port-adapter>
│   │   └── <port>
│   ├── app.ts
│   └── index.ts
├── .env.example
├── jest.config.integration.js
└── jest.config.js

```
| Directory / File                     | Description                                                                                                                                                         |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `src`                                | All source code write in TypeScript must be in this directory.                                                                                                      |
| `src/index.ts`                       | Main entry point for initial configuration of the project. **Do not edit this file.** Start with `src/app.ts`.                                                      |
| `src/app.ts`                         | Project entry point. You can call your first `port` here and boot your providers that should start before your server.                                              |
| `src/config`                         | All configurations can live here.                                                                                                                                   |
| `src/config/tests/fixtures`          | Helpers for using in tests.                                                                                                                                         |
| `src/config/module-alias.ts`         | Module configurations for using `@/` instead of `../../` on dev, tests and production environments.                                                                 |
| `src/core`                           | Pure domain implementations. `core` files must **not know** any `port` or `adapter`, nor anything ouside `core` directory.                                          |
| `src/core/<domain/entity>`           | Inside the `core` directory, you can organize your files by domain or entity.                                                                                       |
| `src/core/<domain/entity>/types`     | Start point for modelling your domain / entity with TypeScript types.                                                                                               |
| `src/core/<domain/entity>/use-cases` | Here you can put your functions with business rules for this specific domain / entity, and the adapters that `ports` will use.                                      |
| `src/core/types`                     | Here you can put the types that are not related with any of your domains or entities.                                                                               |
| `src/helpers`                        | Here you can put your global helpers.                                                                                                                               |
| `src/ports`                          | Anything with external world contact. When you need to access something on `core`, you must use an `adapter`.                                                       |
| `src/ports/adapters`                 | Adapters for ports. For example: You can have a `database` adapter that can use `Postgres` ou `MariaDB`. An `http` adapter that can consume `express` or `fastify`. |
| `src/ports/<port>`                   | Here you can create your raw `ports` with real implementation: `express` / `fastify` as http server, `postgres` / `mariadb` as databases, etc.                      |
| `.env.example`                       | List of Environment Variables. Please, copy this file and create a new `.env` file to use Env Vars.                                                                 |
| `jest.config.integration.js`         | Jest configuration file for integration tests.                                                                                                                      |
| `jest.config.js`                     | Main Jest configuration file.                                                                                                                                       |

## Important usage information

### Environment Variables

You can use env vars by copying the `.env.example` file to a new `.env` file on the root of the project.
To document all used env vars, and get autocomplete when use the function `env('YOU_VAR')`,
just put all your env vars on file `src/helpers/env.ts`.

### Global import

All files and dirs inside `src` directory can be imported using `@/`.
Prefer using this way over local import (`../../`).

## License

MIT
