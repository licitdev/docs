# Data Model

> The Directus data studio enables no-code configuration and management for any SQL database, with no arbitrary restrictions on
> how you build your data model. You get control over technical details like table, column and relationship configuration, but also how how users view and interact with data from within the data studio.

[[toc]]

:::tip Before You Begin

<!--
Learn Directus
Please see the [Quickstart Guide]().
Configuration > Overview
-->

You'll need at least basic knowledge of relational database concepts.

:::

:::tip Learn More

You can also configure your data model programmatically with the API. To learn more, see our API documentation on
[Collections](/reference/collections/) and [Fields](reference/fields/).

:::

## Relational Data Models

In order to understand how Directus handles data models, you will need an understanding of what relational data models are. This section
provides a brief summary of the core concepts. It may be useful as a review, or for business users working on your team that want a simple
explanation of how data models work. If you have a firm knowledge of relational data model concepts, such as databases, data tables, columns,
data types, primary/foreign keys, rows, relationships, and schemas then feel free to jump to [Data Models in Directus](#data-models-in-directus).

### Databases

Directus is an SQL database wrapper. A database is a set of data stored in a computer. They enable you to store your data in a structured way, to make it more easily accessible. The way you structure your data is called your data model. SQL databases store data across data tables.

![Image of an ORM](image.webp)

### Database vs Excel

To make a comparison most business users can relate with, Excel. You know how you can build a table on one sheet in Excel, build another table on another sheet, then link the rows of each table together? That is how a relational database works. But there are some key points where the two differ.

<!-- a database is *somewhat similar* to building multiple spreadsheets in a Excel workbook and dynamically linking their rows together.  -->

![Data in an Excel Spreadsheet](image.webp)

Excel lets you make your data stylized _(bold, italicized, colored, custom fonts, etc.)_, set dynamic functions in cells, add graphics like charts and graphs, and input any kind of data into any cell you'd like, with no enforced rules or structure. Many times, we store data as tables, but that's not always the case, as excel can serve tons of other purposes. Your excel spreadsheet is a blank canvas, designed to store up to tens of thousands of rows of information.

![Data in a Database](image.webp)

Databases store raw data. There is no stylization as databases only store raw data values. Any time you want to style data, build a function, or put data into a graph, you have to code a dashboards, website, user interface, etc. to add this logic. Relational databases store data in a structured way. This allows them to handle millions, _and in some cases_ billions and trillions of rows of information.

### Data Tables

![A Data Table](image.webp)

<!-- image should note rows and columns. -->

Data tables typically store data about one distinct type of record, object, or observation, such as a financial transaction,
blog post, geo-position, user, IoT event, _or anything_. Data tables are composed of columns and rows.

### Columns

![bank transaction](image.webp)

Columns are categories that store one kind of information. Each column has a unique, descriptive name and stores one unit of information in its [cell values](#cell-values). Columns helps keep the data organized, consistent, and easily accessible. The columns you choose to add to a data table will completely depend on the information you need to store.

<!-- For example, in a database for IoT devices monitoring the weather, an `iot_events` data table may contain columns `device_id`, `location`, `time`, `temperature`, `pressure`, `humidity`, etc. -->

### Data Types

To further maintain structure and consistency, when you create a column, you must also define its data type. For example, an `age` column might be assigned `INTEGER` and a `blog_content` data table might be `TEXT`.

It is important to set the right data type when creating a column. For example, if you type the character `2`, it could be stored as an
`INTEGER` or as `STRING` _(as in a string of text)_. If you use the `INTEGER` version of `2`, when you try to add
`2 + 2`, the computer may calculate `4`. If you use the `STRING` version of `2`, when you try to add `2 + 2`, the
computer will concatenate them into`22`. There are countless incongruent, unexpected, and potentially dangerous behaviors that emerge when a data type is misconstrued.

### Cell Values

Each value in a column is stored in its own cell. The term cell value just means a unique value in the column. In general, you want to create columns that save _atomic_ values. That means create the column to store the smallest indivisible units. There is no restriction for the kinds of information to include in a column, but there are good and bad practices.

<!--
For example:
- bad example: `logins` table with a `date_and_ip` column
- good example: `logins`  with `date` and `ip_address` columns
-->

### Rows

![Rows](image.webp)

Fundamentally, a row is just a grouping of cell values from the columns within a data table.
When you want to create new data in a data table, a new row is created in the database.
Each row stores the data associated to a unique record, event, object, person, observation, etc.
Data tables can contain millions, _even billions and trillions_ of rows of data.

### Primary Keys

In order to keep track of each row, every data table must have a primary key column. The primary key column stores a unique ID for each row.
It guarantees you can always find a row and differentiate it from other rows, even if all other column values between two rows are identical.

A primary key can be any type of value.

### Avoid Data Redundancy

Relational databases allow us to build data models that avoid redundant data. Data redundancy is when you have the same data represented in two locations in your database. This is inefficient and dangerous.

Let's consider the `author` column from the blog table in the example above. Right now, there is one column for the
author, which stores their name. However, we will likely want to store more information about the authors, such as their
email address, social media, and so on. We _could_ put this author information directly into the blog data table:

![Redundant Data Model - In the Blog and a User Details Table](image.webp)

But let's say authors are also users, so we need the same information for user profiles. We _could also_ store their names and social media information over in the `user_details` data table. However, this creates data redundancy.

![user-details](image.webp)

There are two big problems with this approach:

<!-- First, we have no way to link information from user details over to blog posts. In the future, if we wanted to change include the author's social media, this could be very difficult or impossible to manage. If two authors had the same name, it
would be difficult or impossible to know which one's social media information to use. -->

First, it becomes difficult or impossible to maintain accurate information. If the author decides to change their social media information under `user_details`, that author will have to go through and update their social media on every single row containing their blog posts. With just 10 or even 100 blog posts, this would be annoying but perhaps not a massive problem. However, as volume of data increases to millions or billions of rows, updating redundant data can become a serious problem.

Second, it wastes storage space and can slow down performance. With a data model containing few hundred blog posts, duplicate data may not be not cause huge drops in performance. But again, if you have the same information repeated again and again over millions or billions of rows, storage is wasted on a massive scale.

### Why We Use Relational Data Models

![Blogs and Users, Relationally Linked](image.webp)

To avoid data redundancy, it is always best practice to keep your data model D.R.Y, which is an acronym for _Don't Repeat Yourself_. You want to make sure that all data is unique. It should be input one time, in one location, then relationships are created between the data tables. This is where the _relational_ part of relational data models comes into play.

To keep data _D.R.Y._ in a relational database, the best strategy is to design each data table to store information for one unique record, object, or observation. You can then relationally link the data tables. This process of designing a relational database using relationships to separate relevant information and ensure there is no duplicate information is called _normalizing_ your database. There is a lot to learn to master database normalization, a thorough explanation goes beyond the scope of this document, and there are plenty of resources to learn about database normalization online. However, we will give one quick and simple example.

<!--
Normalize blogs/authors with an M2O

To continue the blog and authors example, your blogs table would only contain information about the blog posts. The user details table will only contain information about authors.

![database normalization](image.webp)

Instead of just placing the author name into a column on the `blogs` data table _(and creating redundancy)_, we create a
column to add the Primary Key from `user_details`. Remember, each Primary Key in a data table is unique, which means if
we can find the primary key for our blog post's author, we can be certain the data _(the author name, social media
link)_ is all accurate and up to date. Also, whenever you have a column for another data table's Primary Key, it is
called a _Foreign Key_. This naming convention helps avoid confusion.
-->

### Types of Relationships

There are several ways you can relationally link tables:

- **One to One** — Each row in a parent data table can link to one other row in the related table.
- **Many to One** — Many rows in a parent data table can link to one row in another data table.
  _Note that in the data model, Many-to-One and One-to-Many relationship are exactly the same._
- **One to Many** — Each row in a data table can link to many rows in another data table.
- **Many to Many** — Many rows in a data table are linked to many rows in another data table.\
  Many to many relationships require a special junction data table to manage these relationships.
- **Many to Any** — Many Rows in a data table link to many rows across any other data tables.\
  requires a junction data table, like many-to-many relationships, but also requires the a column to store the related data table's name.

To learn more about how these relationships work within Directus, see our guide on [relationships](/configuration/data-model/relationships).

### Database Schemas

As you design your relational data model, you will need to create a schema to keep track of its complexity. A schema is a blueprint for your data model. It defines the data tables, the columns and column details in each data table, and [relationships](#relationships) between tables. It does not include the actual rows of data being stored.

Schemas can be confusing at first glance because you typically think of columns as horizontal, going from left to right, but in a schema, columns are typically listed vertically, with additional technical details to the side of each column.

<!--
![A Schema](image.webp)
Table Name
Columns (technical details: data types, relationship, fk/pk, etc.)
-->

### Working With Relational Data Models

<!-- Create video or Vue Component demonstrating each of these components. -->

Typically, once you've designed your data model, you build it using SQL. SQL, or Structured Querying Language, is the programming language of most relational databases, used to create, read & query, update, and delete anything and everything within the database; from the data to the data model itself.

Once the initial data model is built, you create a backed using something like Node.js or Flask. In your backend, you must code the API endpoints and add in logic to create, read, query, update, and delete data yourself. However, at this point the data is still raw, with no stylization.

To those who are unfamiliar, the SQL language, classic ORMs, querying and working with raw data, and
traditional relational database jargon can feel un-intuitive and overly-technical.

Business users may find it difficult to think of data without thinking about how it is represented and interacted with: colorful, stylized, embedded on a map, etc.
It may not be practical to teach everyone on the team how to work with and think in terms of raw data. For example, _most people in most situations_ would find it easier to see geo-coordinates as pinpoints on a map than as raw latitude and longitude stored as JSON.

<!-- In fact, they would be completely unable to see a geo-position and approximate where it rested on a map of the Earth. -->

Therefore, developers need to build a frontend with a polished UI and custom display logic to make working with the data more human-friendly. However, even for developers with strong SQL database skills, building out APIs and GUIs to build and manage a data model
is time consuming.

## Data Models in Directus

<video title="Settings > Data Model" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

All relational data model concepts listed above apply in Directus.

You get complete, un-opinionated, relational data model design and configuration.
The difference is that Directus handles all SQL, builds the API, and provides frontend UX and logic for you.
The Data Studio also offers features and functionalities to display and interact with your data in a more human-friendly way.
Once your data model is configured, the data is accessed from within the other [modules](/getting-started/glossary/#modules).

Data model configuration takes place across the following pages and menus in the Data Studio:

<!-- You get the best of both worlds: full control over your data model an intuitive experience, even for the most non-technical users. -->

**Settings > Data Model > [Collection] > [Field] > Field Configuration Drawer > [Section]**

You have the power to do the following things, without a line of code or SQL:

- View, configure, and manage your relational data model and asset storage.
- Configure how data is displayed within the Data Studio.
- Configure how data is interacted with by users in the Data Studio.
- Translate any and all text in the Data Studio into any language.

Directus replaces traditional relational database jargon with more user-friendly terms and concepts. Please keep in mind that while traditional relational database
jargon strictly encompasses database concepts, some of the new Directus terms encompass _these relational database concepts plus their display and interaction logic_.
The following sections will further introduce Directus' data model concepts and map them to
the classic relational database concepts described above, in [Relational Data Models](#relational-data-models).

## Collections

<video title="Collections" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

A collection is a data table. You access all collections, including system collections, under **Settings > Data Model**. From there, click a
collection to open its configurations page. To learn more, see our guide on [collections](/configuration/data-model/collections).

## Fields

<video title="Fields" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

A field is a database column, but with a twist.

Remember, SQL database columns store pure, raw data. From there, developers build out custom logic and UIs to determine how this data is displayed and interacted with. In Directus, the concept of a field couples all this in one. A field encompasses all database-level column configuration, as well as the logic that the Data Studio uses to display and interact with the column's data.

You configure fields under **Settings > Data Model > [Collection] > [Field] > Field Configuration Drawer**. To learn more, see our guide on [fields](/configuration/data-model/fields).

<!--
@TODO: Uncomment for 2nd Configuration PR
 which is composed of the following sections:

   - [Schema](/configuration/data-model/fields/schema) — Defines the database column schema for the field.
   - [Relationship](/configuration/data-model/fields/relationship) — Controls _and only appears when configuring relational_ field details.
   - [Translations](/configuration/data-model/fields/translations) — Controls _and only appears when configuring translation_ field details.
   - [Field](/configuration/data-model/fields/field) — Sets details for the field input, which is displayed on the [item page](/app/content/items/).
   - [Interface](/configuration/data-model/fields/interface) — Configures how you interact with the field's values.
   - [Display](/configuration/data-model/fields/display) — Configures how field values are displayed in the data studio.
   - [Validation](/configuration/data-model/fields/validation) — Creates a filter to determine valid user input.
   - [Conditions](/configuration/data-model/fields/conditions) — Alters the current field's setup based on the values of other fields.
   -->

## Alias Fields

<video title="Alias Fields" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

This type of field is a virtual column. That is to say, they are presented as fields on the item details page, _they may look and feel like they might be columns_, but do not actually map to a unique column in the database. Alias fields enhance how you present, group and manage data. Some alias fields let you access data or information which is integrally _related_ to the current collection, but not stored in the current collection _(i.e., relationally linked rows)_.

## Items

<video title="Collections" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Items map to the concept of database rows, but with a twist.

Since _rows can be related to other rows form other data tables_, we use the term items to stress the idea that it represents all the information for one complete unit, or thing, or observation, or well... _item_. Remember from our discussion above about traditional databases, the ideal relational database is _normalized_. Unfortunately, normalized data is not always the easiest for people to imagine or envision because related data is spread across multiple data tables.

Therefore, when you access an item, you may get more than just the current collection's row level-data, _in some cases an item may even provide access to the data in related rows._ You can access items from within the other app modules, such as [Content](/app/content), [User Directory](/app/user-directory), and [File Library](/app/file-library).

## Data Types

In order to support all flavors of SQL, Directus abstracts away data type differences with a [Data Type Superset](/getting-started/glossary/#data-type-superset).

## Keys and IDs

<video title="Keys and IDs" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Primary keys are called IDs in Directus fairly frequently. When you [create a collection](/configuration/data-model/collections/#create-a-collection), you must add an `ID` field to keep proper track of the collection's items, and also to use as foreign keys when create [relationships](#relations). Directus supports the following types of IDs:

- **Auto-Incremented Integer**
- **Auto-Incremented Big Integer** _(MySQL and PostgreSQL only)_
- **Generated UUID**
- **Manually Entered String**

## Relationships

<video title="Relations" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Directus supports all standard relationship types, as well as a few more of its own _compound_ types. To learn more,
see our guide on [relationships](/configuration/data-model/relationships)
