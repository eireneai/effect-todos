# EffectTodos

## Devcontainer Configuration

Using a devcontainer requires docker to be installed on your machine. The easiest way to get started with docker is by installing docker desktop.

Create a new file at `.devcontainer/.env` and add the following values:
```
MONGO_USERNAME=root
MONGO_PASSWORD=password
```
You should now be able to open the project in a devcontainer. Explore the `.devcontainer` directory and try to answer the following questions:
1) How are different services run together in the workspace?
2) How is node installed?
3) How are other dependencies including global npm packages installed?
4) How is VS code configuration stored as part of the devcontainer environment?
5) How are environment variables set in different services?

## NX Monorepo CLI

Once you are up and running within the devcontainer, you should have the NX CLI installed as well as the VS code NX extension. Use the cli and the `@nx/node` plugin to scaffold a node app preconfigured with eslint, jest, webpack and an end-to-end test by running the following command:

```sh
nx g @nx/node:app --name=server --directory=apps/server --linter=eslint --unitTestRunner=jest --e2eTestRunner=jest --bundler=webpack --dryRun
```

The command will preview the files and directories to be created. Once you've run the command and know what to expect to be generated, remove the dry run flag and run the command again to actually generate the files.

To run the generated node application, use the following commands:

```
nx build server
nx serve server
nx e2e server
```

Try to answer the following questions about NX:
1) Where are NX project configurations found?
2) What is the difference between NX generators and executors?
3) What is the NX dep graph?
4) What are NX plugins?

## Install effect

Review the instructions on the effect ts website for manual installation:

https://effect.website/docs/quickstart#nodejs

Some steps will not be necessary to follow. For example, NX has configured typescript and the ts compiler for us. To install effect, we simply need to run `npm install effect @effect/platform @effect/platform-node` and enable strict mode in our root `tsconfig.json`. 

Once effect is installed in the workspace, remove the started code from the server application we generated in the previous step and replace it with a hello world effect application:

```
import { Effect } from 'effect'
import { NodeRuntime } from '@effect/platform-node'

const program = Effect.logInfo('Hello world!')

NodeRuntime.runMain(program)
```

## Rewrite an example todos API using effect

Visit the following public repository:

https://github.com/andremartingo/node-rest-api-todo

Rewrite the express app using effect and effect platform in the new node server project that was scaffolded with NX.

The code in the example express app is organized by technical concern. Use the following effect modules to address each technical concern:

- config - `Config` module from `effect` to read configuration values from the environment  
- controllers - `HttpApi` and `HttpApiBuilder` modules from `@effect/platform` to define http endpoints and implement request handlers
- db - `Layer` module from `effect` to define the connection to mongoose as a managed resource
- middleware - `HttpApiMiddleware` from `@effect/platform` to define authentication middleware
- models - `Schema` module from `effect` to define todo and user models
- routes - `HttpApiBuilder` from `@effect/platform` and `NodeHttpServer` from `@effect/platform-node`

Read through the following links to better understand what the various effect modules provide and how to use them:

- https://effect.website/docs/configuration/
- https://effect.website/docs/requirements-management/layers/#creating-layers
- https://effect.website/docs/schema/introduction/#the-schema-type
- https://github.com/Effect-TS/effect/blob/main/packages/platform/README.md#http-api

Make sure to reach out on slack for help and clarification while completing this step.