<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo_text.svg" width="320" alt="Nest Logo" /></a>
</p>

## Nest CLI

#### Install Nest CLU

```bash
npm i -g @nestjs/cli
```

#### Create New Project

```bash
nest new project name
```

## Environment

<p>
Sources :

[NestJS Official Documentation](https://docs.nestjs.com/techniques/configuration)

[Petros Kyriakou Blog](https://dev.to/pitops/managing-multiple-environments-in-nestjs-71l)

</p>

#### 1- Installation

```bash
npm i --save @nestjs/config
```

#### 2- .gitignore Modification

Add to .gitinore file the following content:

```
# Env
env/*
!env/sample.env
```

#### 3- Folder and Files Architecture

Create on the root of the project an architectire of folder and files like this :

```
env/
---- production.env
---- development.env
---- staging.env
---- sample.env
```

_Nota Bene : sample.env is just a file where you put all the necessary environment keys to run the project, with mockup values if you want._

#### 4- Scripts Modification

To take into account the good environment file when running the project, add to package.json scripts the good values :

```json
  "scripts": {
    "start": "NODE_ENV=development nest start",
    "start:dev": "NODE_ENV=development nest start --watch",
    "start:debug": "NODE_ENV=development nest start --debug --watch",
    "start:prod": "NODE_ENV=production node dist/main",
  }
```

#### 5- Activate Environment Files

Add to src/app.module.ts the ConfigModule. In the following example, we have also included the envFilePath configuration element to select the right env file.

```ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';

@Module({
	imports: [
		ConfigModule.forRoot({
			envFilePath: `env/${process.env.NODE_ENV}.env`,
		}),
	],
})
export class AppModule {}
```

## Mongoose

<p>Sources :

[NestJS Official Documentation](https://docs.nestjs.com/techniques/mongodb)

</p>

#### 1- Dependencies Installation

```bash
npm install --save @nestjs/mongoose mongoose
```

#### 2- Env + Module Import

Import MongooseModule into the root AppModule (src/app.module.ts). In the following exemple, ConfigModule has also been added to manage environment variables.

```ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import { MongooseModule } from '@nestjs/mongoose';

@Module({
	imports: [
		ConfigModule.forRoot({
			envFilePath: `env/${process.env.NODE_ENV}.env`,
		}),
		MongooseModule.forRoot(
			`mongodb+srv://${process.env.MONGODB_USER}:${process.env.MONGODB_PASSWORD}@${process.env.MONGODB_DATABASE_URL}/${process.env.MONGODB_DATABASE}`
		),
	],
})
export class AppModule {}
```

#### 3- Define your Schemas

Structure for defining schemas. Exemples with a "user" module.

```
/src
--- users/
--------- schemas/
------------------ user.schema.ts
```

the following schema :

```ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

export type UserDocument = User & Document;

@Schema()
export class User {
	@Prop({ required: true })
	email: string;

	@Prop()
	firstname: string;

	@Prop()
	password: string;
}

export const UserSchema = SchemaFactory.createForClass(User);
```
