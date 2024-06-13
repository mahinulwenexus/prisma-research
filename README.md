# Contents
* [Installing the environment](#installing-the-environment)  <br>
        - [Create a new schema file](#first-initiate-or-create-a-new-schema-file)
        - [Write Schema](#second-write-the-schema)
        - [Perform Migration](#thrid-migration) 

`npm install -g prisma`

![image](https://github.com/mahinulintern/prisma-research/assets/167665561/2787fbf0-7bf8-4964-8344-5c4e6968a637)
![image](https://github.com/mahinulintern/prisma-research/assets/167665561/f2cdd9aa-9dd0-4c94-8f43-939a1eedfd30)
![image](https://github.com/mahinulintern/prisma-research/assets/167665561/560460de-aa38-42d2-b830-6d880944e5c6)
![image](https://github.com/mahinulintern/prisma-research/assets/167665561/b5a98c61-ccd4-444a-a248-281479600071)





<br>
<br>

# Installing the environment
* Setup ORM
* Perform Prisma Migrate [it simply resets the database(hypothesis)] , have to fix it. <b>Know what it is </b>
* There are two things I need to install. 
    - `npm install prisma`
    - `npm install @prisma/client` 
* There are a couple of approaches to installing Prisma. I'll be selecting a simple one.

<br>

## First Initiate or Create a new Schema file.
* `npx prisma init` - this will create `schema.prisma` file.
* Completed the first Step of the `prisma installation`.


<!---
================ DROPDOWN ====================================================================================
--->
<details><summary>A little bit more to the Concept</summary>
  
* You might found `env` file. Don't delete it.
* Put the `link` which can be used to connect to your `required` database in that `env` file.

</details>



<br>
<br>

## Second: Write the schema
* The schema concept is big. So, I'll note it down later.
* For now, here is a dummy schema that can connect to a PostgreSQL server.


<!---
================ Prisma Schema Code ====================================================================================
--->
```prisma
// no need to change this
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql" // it is saying that the data source is PostGreSQL.
  url      = env("DATABASE_URL")
 // I can't put a manual string here(env("DATABASE_URL")). It gives me an error.
// Instead go to .env file created by prisma and put that string there.

}

// this is a model.
model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String?
}
```
> What I did so far? <br> Initiate `schema.prisma` file. <br> Now how does it get compiled or executed?

### Special Note that, I'm connecting to a specific database through that `DataBase URL`. A database URL contains all the information needed to connect to the database.

## Third: Migration
```javascript
npx prisma migrate dev --name init

// default key : npx prisma migrate dev
// name: if you don't put name field then it will manually ask for a name within the process.
// name: you can set any name. It should be fine.
```
![image](https://github.com/mahinulintern/prisma-research/assets/167665561/1feaf37c-327e-4178-b250-88b22cec6f4b)

* What is migration?
* In Prisma, migrations are a mechanism for managing changes to your database schema over time. When you modify your Prisma schema definition, Prisma provides tools to generate and execute migration scripts that safely update your database tables to reflect the new schema structure.
* When I change something in the schema - perform migrate()
* Database gets synced with Prisma schema. 
* <b> If the schema doesn't match with the current database, it will erase the whole database along with data to match with Prisma schema. </b>

<!---
================ DROPDOWN ====================================================================================
--->
<details><summary><b> <i>A little bit more to the Concept</i></b> </summary>
  
* `npx prisma migrate dev --name init` installs something more behind the scene.
* It automatically installs `@prisma/client`.
* It automatically executes `npx prisma generate`.
</details>

## Fourth: 
* Using `@prisma/client` to perform the prisma operation.
```javascript
import { PrismaClient } from '@prisma/client'
// automatically installed by npx prisma migrate dev --name init

const prisma = new PrismaClient()
//initiate prisma client


// function can be any name
// what left is to execute the function somewhere else.
async function main() {
  const getUser = await prisma.user.findMany();
  console.log(getUser);
}


const x = async () => {
    const data = await main();
    await prisma.$disconnect(); // disconnect prisma client when the process is done.
}
```

