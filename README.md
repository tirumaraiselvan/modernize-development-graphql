# API Days: Modernize development with GraphQL workshop

## Getting started

We will work with Hasura Cloud for this workshop:

1. Head to cloud.hasura.io
2. Create a new project and get a free db from Heroku


#### For local Docker setup

In case you prefer to work locally with docker containers instead:

Use `docker-compose up -d`

View your new Hasura GraphQL Engine Console at [http://localhost:8080](http://localhost:8080) (admin secret from docker-compose.yaml: `adminsecret`)

*Note*: To end a session, use `docker-compose down -v`


## Loading initial data

#### Via console
- Head to the Data tab and go to the Run SQL window
- Run northwind_ddl.sql
- Run northwind_data.sql

#### For local Docker setup

Use the following command to set up initial tables and data into your postgres container:

```
psql postgres://postgres:mypassword@localhost:6432/postgres < northwind_ddl.sql 
psql postgres://postgres:mypassword@localhost:6432/postgres < northwind_data.sql 

```

If you do not have `psql` available, you can copy the `northwind_ddl.sql` and `northwind_data.sql` file to the postgres container and execute the `psql` command via inside it:


*Note*:  You can find `<postgres-container-ID>` with `docker ps`

```
docker cp northwind_ddl.sql <postgres-container-ID>:/
docker cp northwind_data.sql <postgres-container-ID>:/
docker exec -ti <postgres-container-ID> /bin/bash
psql -U postgres < northwind_ddl.sql
psql -U postgres < northwind_data.sql
```

## Track tables and foreign-key relations

![Track tables in console](images/Hasura_setup_track_tables.png)

Return to the Hasura GraphQL Engine console and select the Data tab. In the central view, there should be a section "Untracked tables or views" with several tables listed and a "Track All" option available. Select "Track All", and then "Track All" again for untracked foreign-key relations.

Now you're all set! You should see your tables listed in the left-hand panel.

Go to the Graphiql tab and start trying out queries, mutations, and subscriptions.


## Dataset

The Northwind dataset includes sample data for the following.

- Suppliers: Suppliers and vendors of Northwind
- Customers: Customers who buy products from Northwind
- Employees: Employee details of Northwind traders
- Products: Product information
- Shippers: The details of the shippers who ship the products from the traders to the end-customers
- Orders and Order_Details: Sales Order transactions taking place between the customers & the company


## External App

**Table**: Customer, Order, Shipper
**Role**: customer

Rules:

1. Customer can only select their own row in customer table
2. Customer can get their orders.


## Internal App

**Table**: employees
**Role**: employee, hr

Rules:

1. Employee can see and edit their own information.
2. HRs can see and edit information of all employees

## Public facing app

**Table**: products
**role**: api

Rules:

1. Only consumers with valid api key can see the products table

## RBAC

A role is a collection of permissions. Permissions determine what operations are allowed on a resource. 
When you grant a role to a user, all permissions in the role are automatically granted to the user.

Hasura has role-based schemas.

## ABAC

Attributes are values that are associated with a user or resource. 

Hasura can use user attributes and data attributes


## Actions (Bring your REST APIs)

## Remote Schema (Bring your GraphQL APIs)
