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

Basic knowledge of relational database concepts and some experience with SQL are strongly recommended.

:::

:::tip Learn More

You can also configure your data model programmatically via the API. To learn more, see our API documentation on
[Collections](/reference/collections/), [Fields](reference/fields/), and [Relations](/reference/system/relations/).

:::

## Relational Data Models

In order to understand how Directus handles data models, you will need an understanding of what relational data models are. This section
provides a brief summary of the core concepts. It may be useful as a review, or for business users working on your team that want a simple
explanation of how data models work. If you have a firm knowledge of relational data model concepts, such as databases, data tables, columns,
data types, primary/foreign keys, rows, relationships, and schemas then feel free to jump to [Data Models in Directus](#data-models-in-directus).

### Databases

Directus is an SQL database wrapper. A database is a set of data stored in a computer, in a structured way, making it organized, accessible, and scalable. The specific way you structure your data within a database is called your data model.

![A Database Schema](image.webp)

### Database vs Excel

Let's use Excel to make a comparison most business users can relate with. You know how you can build a table on one sheet in Excel, build another table on another sheet, then link the rows of each table together? That is pretty much how a relational data model works. But there are some key points where Excel and relational databases two differ.

<!-- a database is *somewhat similar* to building multiple spreadsheets in a Excel workbook and dynamically linking their rows together.  -->

![Data in an Excel Spreadsheet](image.webp)

Many times, we store data as a table in Excel, but that's not always the case, as the program serves tons of other purposes. Excel lets you make your data stylized _(bold, italicized, colored, custom fonts, etc.)_, set dynamic functions in cells, add graphics like charts and graphs, and input any kind of data into any cell you'd like with no enforced structure. Your Excel spreadsheet is a blank canvas, designed to store up to tens of thousands of rows of information.

![Data in a Database](image.webp)

There is no stylization within databases. They strictly store raw data values in a structured way. Any time you want to style data, build a function, put data into a graph, _etc.,_ you must use a dashboard, website, app, or some other kind of UI which applies stylization logic to the raw data. Databases store raw, un-stylized, structured data and are designed to handle millions, _and in some cases billions and trillions_, of rows of information.

### Data Tables

![A Data Table](image.webp)

<!-- image should note rows and columns. -->

SQL databases store data across data tables. Data tables typically store information about one distinct type of record, object, or observation, such as a financial transaction, blog post, geo-position, user, IoT event, _or anything_. Data tables are further broken down into columns and rows.

### Columns

![bank transaction](image.webp)

Columns are categories that store one kind of information. Each column has a unique, descriptive name and stores one unit of information in each of its [cell values](#cell-values). Columns helps keep the data organized, consistent, and easily accessible. The columns you choose to add to a data table will completely depend on the information you need to store.

<!-- For example, in a database for IoT devices monitoring the weather, an `iot_events` data table may contain columns `device_id`, `location`, `time`, `temperature`, `pressure`, `humidity`, etc. -->

### Cell Values

Each value in a column is stored in its own cell. In general, you want to create columns that save _atomic_ values. That means create the column to store the smallest indivisible units. There is no restriction for the kinds of information to include in a column, but there are good and bad practices. For example:

- **A Bad Column:** A `city_state_zipcode` column.
- **Good Columns:** Separate `city`, `state` and `zipcode` columns.

### Data Types

To further maintain structure and consistency, when you create a column, you must also define its data type. For example, an `age` column might be assigned `INTEGER` and a `blog_content` column may be assigned a `STRING` or `TEXT` data type. There are countless incongruent, unexpected, and potentially dangerous behaviors that could emerge when a data type is misconstrued. Therefore, it is important to set the right data type when creating a column.

For example, if you type the character `2`, it may be stored as an
`INTEGER` or as `STRING`. If you stored `2` as an `INTEGER`, when you try to add
`2 + 2`, the computer may calculate `4`. If you stored the character `2` as a `STRING`, when you try to add `2 + 2`, the
computer will concatenate them into`22`.

### Rows

![Rows](image.webp)

Each row stores data associated to a unique record, event, object, entity, observation, etc.
Data tables can contain millions, _even billions and trillions_ of rows of data.

### Primary Keys

![Primary Key](image.webp)

In order to uniquely identify and track each row, every data table must have a primary key column. A primary key is a unique ID that identifies a specific row. Any type of system could be used to generate unique primary keys, so long as it guarantees each key is unique. Perhaps the most common is incrementing integers, where the primary key on each new row increments as follows `1`, `2`, `3`, `4`, etc... The primary key column guarantees you can always find a row and differentiate it from other rows.

### Foreign Keys

![Foreign Keys in a Data Table](image.webp)

Since primary keys uniquely identify each and every row in a data table, they are the perfect tool to create relationships. If you want to relationally link two data tables, you create a column to store primary keys from a related table. In this case, we use the term _foreign key_, to signify it is the key from a foreign table.

### Types of Relationships

Note that when we talk about two related tables, we refer to them as the _parent table_ and the _related table_. These two terms are based solely on perspective, similar to the terms _this_ and _that_ or the terms _here_ and _there_. They signify the perspective from which you are looking at the relationship.There are several ways you can relationally link tables:

- **One to One** — Each row in the parent data table can link to one row in the related table.
- **Many to One** — Many rows in the parent data table can link to one row in the related table.
- **One to Many** — Each row in a data table can be link to many rows in another data table.
  _Note that in the data model, a Many-to-One and One-to-Many relationships are exactly the same._
- **Many to Many** — Many rows in the parent table can link to many rows in the related table.
  M2M relationships require a third table, called junction data table, to manage its relationships.
- **Many to Any** — Many Rows in a data table can link to many rows across _any other table in the database_.
  Similar to M2M relationships, M2As require a junction data table, but also requires an additional column to store the related tables' names.

To learn more about how these relationships work conceptually as well as how they are implemented within Directus, see our guide on [relationships](/configuration/data-model/relationships).

### Database Schemas

So far, we have seen data tables presented like this:

![A Data Table](image.webp)

As you design your relational data model, you will need to create a schema to keep track of its complexity. A schema is a blueprint for your data model, which defines its data tables, columns in each table, details about each column and relationships between tables. It does not include the actual data points stored. Here is is a simple schema of two relationally linked tables:

```
table_one
- column1 (primary key)
- column2 (data type, optionally explain what the column stores)
- column3 (...)
```

```
table_two
- column1 (primary key)
- column2 (...)
- column3 (...)
- table_one_id (foreign key, relationally links rows via table_one.column1)
```

In this schema, we defined two tables with overtly generic names `table_one`, `table_two` and `column1`, `column2`, etc. The names you choose for data tables and columns are up to you. Pick any unique, memorable name that represents the data contents stored within.

Our schema above is fairly basic, just like the data model it represents. With more complex schemas, containing dozens _(or maybe hundreds!)_ of relationally linked data tables, you may want a visualization of how each and every table interlinks.

![A Complex Schema](img.webp)

### Avoid Data Redundancy

Relational databases allow us to build data models that avoid redundant data. Data redundancy is when you have the same data for the same object or observation in two locations in your database. This is inefficient and dangerous.

To give an example of this, let's consider a `blogs` table. In a blog, you may want to display the author's details, so you add an `author_name` column.

```
blogs
- id
- title
- hero_image
- blog_content
- author_name
```

The table above stores the author name directly inside of the `blogs` data table. However, throughout our project, we may likely need more information about authors, such as their email address, social media, etc. We _could_ put this author information into the `blogs` data table:

```
blogs
- id
- title
- hero_image
- blog_content
- author_name
- author_email
- author_twitter
```

You should immediately start to realize this data table no longer represents one single object, but two: blog posts and authors. This is _almost always_ a sign the data should be split across different tables and relationally linked.

Continuing on, let's say that for our project, authors are one type of user. All user details are stored in a `users` data table and its data is displayed on each user profile page, as well as for messaging and other types of transactions, _which is a common situation in many projects_. In this case, the author name and other details would also need to exist in the `users` table.

![blogs and users](image.webp)

This creates data redundancy. There are two big problems with data redundancy:

First, it becomes difficult or impossible to maintain accurate information. If the author decides to change their social media information under `users`, someone would have to go through and update author details on every single row containing their blog posts. With just 10 or even 100 blog posts, this would be annoying but perhaps not a massive problem. However, as volume of data grows to millions or billions of rows, updating redundant data becomes a serious problem.

Second, it wastes storage space and slows down performance. With a data model containing a few hundred blog posts, duplicate data may not take up enough space to
cause huge drops in performance. But again, if you have the same information repeated again and again over millions or billions of rows, storage is wasted on a
massive scale.

### Why We Use Relational Data Models

As shown from the previous section, you want to make sure that every single data point is unique. This is where the _relational_ part of relational data models comes into play. To avoid data redundancy, it is always best practice to _normalize your data model_, which is the technical term used to describe designing a data model so that there is no duplicate information stored _at all_.

There is a lot to learn to master database normalization and a thorough education in the practice goes beyond the scope of this document. There are plenty of resources to learn about it online. However, to provide one simple example by improving the example `blogs` data table provided in the previous [Avoid Data Redundancy](#avoid-data-redundancy) section:

```
blogs
- id
- title
- hero_image
- blog_content
- author_name
- author_email
- author_twitter
```

As described in the section on [Rows](/configuration/data-model/#rows), we want each row in a date table to represent one unique record, event, object, entity, observation, etc. To do this, we can remove the `author_name` column from the `blogs` table and replace it with an `author` table, which stores keys from the `users` table.

```
blog
- id
- title
- hero_image
- blog_content
- author_id (stores foreign key from users.id)
```

```
users
- id
- name
- email
- role
- email
- twitter
```

Notice the difference. Before, we placed the author's name directly into a column on the `blogs` data table _(creating redundancy)_.
Now we have replaced that `author_name` column, which contained an author's actual name, with the `author_id` column, which contains foreign keys from `users`. From there we can use the foreign key to relationally link up data from a row in `blogs` with data from the related row in `users`.

### Working With Relational Data Models

![Database, Backend, Frontend]()

Once you've designed your data model conceptually, you typically build and interact with it using SQL, or Structured Querying Language. This language is used to create, read & query, update, and delete anything and everything in the database.

Once the initial data tables are designed and built, a common next step is to build a backed using something like Node.js or Flask. In your backend, you must code custom API endpoints and logic to create, read, query, update, and delete data for your specific data model. However, when the backend accesses data, it is still raw, with no stylization. Raw data is easy to work with for computers, but often quite difficult to work with for humans.

To those who are unfamiliar, the SQL language, raw data, and
traditional relational database jargon can feel un-intuitive and overly-technical.

It may not be practical to teach everyone on the team how to work with and think in terms of raw data. In some cases, business users may find it difficult or nearly impossible to work with raw data. People are accustomed to see information displayed _colorfully, stylized, embedded on a map, etc._ For example, _most people in most situations_ would find it easy to work with pinpoints on a map, yet find it nearly impossible to identify a position on a world map from raw latitude and longitude points stored as JSON.

Therefore, developers need to build front-ends with polished UIs and custom display logic to make working with the data human-friendly. However, even for
developers with strong SQL database skills, building out APIs and GUIs to build and manage a data model is time consuming.

## Data Models in Directus

<video title="Settings > Data Model" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

All relational data model concepts listed above apply in Directus. You get complete, un-opinionated, relational data model design and configuration.
The difference is that Directus handles all SQL, builds the API, and provides frontend UX and logic for you.

The Data Studio also offers features and functionalities to display and interact with your data in a more human-friendly way.
Once your data model is configured, the data is accessible across the other [modules](/getting-started/glossary/#modules). Data model configuration takes place across the following pages and menus:

**Settings > Data Model > [Collection] > [Field] > Field Configuration Drawer > [Section]**

You have the power to do the following things, without a line of code or SQL:

- View, configure, and manage your relational data model and asset storage.
- Configure how data is displayed within the Data Studio.
- Configure how data is interacted with by users in the Data Studio.
- Translate any and all text in the Data Studio into any language.

Directus replaces traditional relational database jargon with more user-friendly terms and concepts. Please keep in mind that while traditional relational database
jargon strictly encompasses database concepts, some of the new Directus terms encompass these relational database concepts _plus display and interaction logic_.
The following sections will introduce Directus terms and map them to classic relational database concepts.

## Collections

<video title="Collections" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

A collection is a data table.

You access all collections, including built-in system collections required to power your project, under **Settings > Data Model**. From there, click a collection to open its configurations page. To learn more, see our guide on [collections](/configuration/data-model/collections).

## Fields

<video title="Fields" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Fields are database columns, but with a twist.

Remember, SQL database columns store pure, raw data. From there, developers build out custom logic and UIs to determine how this data is displayed and interacted with. In Directus, fields encompass all database-level column configurations, as well as the logic that the Data Studio uses to display and interact with the column's data, such as [Displays](/getting-started/glossary/#displays), [Interfaces](/getting-started/glossary/#interfaces), validation, conditions, and more. Directus also has [alias fields](/getting-started/glossary/#alias), which are virtual and do not match directly to a column. To learn more, see our guide on [fields](/configuration/data-model/fields).

## Items

<video title="Collections" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Items are data table rows, but with a twist.

Since _rows can be related to other rows from other data tables_, we use the term items to emphasize that it represents all the information for one complete unit, or thing, or observation, _or well... item!_ Remember from our discussion above about traditional databases, the ideal relational database is _normalized_. Unfortunately, normalized data is not always the easiest for people to imagine or envision because related data is spread across multiple data tables.

Therefore, when you access an item, you may get more than just the current collection's row level-data, _in some cases an item may provide access to the data in related rows._ You can access items from within the other app modules, such as [Content](/app/content), [User Directory](/app/user-directory), and [File Library](/app/file-library).

## Data Types

In order to support all flavors of SQL, Directus abstracts away data type differences with a [Data Type Superset](/getting-started/glossary/#data-type-superset).

## Keys and IDs

<video title="Keys and IDs" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Primary keys are called IDs in Directus fairly frequently. When you [create a collection](/configuration/data-model/collections/#create-a-collection), you must add an `id` field. Directus supports the following types of IDs:

- **Auto-Incremented Integer**
- **Auto-Incremented Big Integer** _(MySQL and PostgreSQL only)_
- **Generated UUID**
- **Manually Entered String**

## Relationships

<video title="Relations" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Directus supports all standard [types of relationships](#types-of-relationships), as well as a few more of its own _compound_ types. To learn more,
see our guide on [relationships](/configuration/data-model/relationships).
