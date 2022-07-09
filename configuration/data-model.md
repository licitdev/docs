# Data Model

> The Directus App enables no-code configuration and management for any SQL database, with no arbitrary restrictions on
> how you build your Data Model. You also have control over how data is displayed, which data is displayed, and how App
> users interact with it.

<!--
To make data more user-friendly, the structure of your database data tables, composed of columns and rows
In Directus, we think of data tables, composed of rows and columns [Collections](/app/content-collections/) (database tables) and [Fields](/reference/system/fields/) (database columns).
-->

[[toc]]

:::tip Before You Begin

<!-- Learn Directus
Configuration > Overview -->

It will be helpful if you have a foundational understanding of relational database concepts.

:::

:::tip Learn More

You can also configure your Data Model programmatically with the API. To learn more, see our API documentation on
[Collections](/reference/collections/) and [Fields](reference/fields/).

:::

## Relational Data Models

Directus is a wrapper for any SQL database. In order to understand how Directus handles data models, you will need an
understanding of what a relational data models are. This section provides a conceptual introduction. Data Analysts,
Database Administrators, and Developers who know about relational data models with SQL may want to skip to
[Data Models in Directus](#data-models-in-directus).

### Databases

A database is a set of data stored in a computer. Databases enable you to store your data in a structured way, to make
it more easily accessible. The way you structure your data is called your data model. SQl databases, which are
relational databases, store data in data tables.

![A Data Table](image.webp)

<!-- image should note rows and columns. -->

### Data Tables

Data tables are composed of rows and columns. Each data table typically contains one specific type of information like a
blog post, user profile, IoT event, bank transaction, _or anything_.

### Rows

Each row represents one unique _thing_ in your data table. Data tables can also have any number of rows of data, from
one to one billion, _and beyond!_ In order to sort and find a specific row efficiently, every data table must have a
Primary Key column. A **Primary Key** is a unique ID for each row. You use it to find the row quickly and efficiently.

 <!--
 ### Primary Keys
 Every [Item](/getting-started/glossary/#items) in a relational database has a unique
[Primary Key](/getting-started/glossary/#primary-key-pk) (or "PK") that identifies it within its
[Collection](/getting-started/glossary/#collections). Because it's required, the key is the first
[field](/getting-started/glossary/#fields) created within a collection, typically storing an "auto-increment" number, an
automatically generated unique hash, or a manually entered value. They are often abbreviated to "PK" (Primary Key), "ID"
(Identifier), "UID" (Unique Identifier), or "UUID" (Universally Unique Identifier), depending on the type of value they
store. After it's created, the value of an item's PK should _never_ change.

To link items together relationally, you simply save a reference to an item's PK in a different field. That _reference_
is called a Foreign Key (FK). If the primary key is a _person_, the foreign key is like their _business card_. It
references the actual person, providing just enough information to find them.

::: tip Composite & Compound Keys

In the above explanation we've ignored _composite_ and _compound_ keys, which are essentially unique keys created by
combining _multiple_ field/column values. Currently, Directus does not support composite primary keys.

:::  -->

### Columns

Tables can also have many columns of data. Every column is labeled with a descriptive name. Here are some examples:

- **a blog:**`title`, `blog_content`, `date_published`, `author`, etc.
- **user details:**`username`, `email`, `phone_number`, `age`, etc.
- **IoT events:**`ip_address`, `temperature`, `time`, etc.
- **bank transaction:**`time`, `sending_account`, `receiving_account`, and `amount`.
<!-- Turn this list into a video montage??? -->

The columns you choose to add in a data table will completely depend on the information you need to store. There is no
specific, set-in-stone rule for the kinds of information to include.

### Cell Values

Typically, you will want to store raw data in the database and you will probably want to break information up into the
smallest pieces possible. This makes the data easier to work with.

### Data Types

You may have noticed that columns are basically categories, storing one kind of information. This helps keep the data
consistent, which is important. To help maintain consistency, when you create a column, you must even define its data
type. For example, an `age` column might be assigned `INTEGER` and a `blog_content` data table might be `TEXT`.

In computer programming, using the right data type is extremely important. For example, `2` could be treated as an
`INTEGER` or as a `STRING` _(as in a string of text)_. If you use the `INTEGER` version of `2`, when you try to add
`2 + 2`, the computer will calculate `4`. If you use the `STRING` version of `2`, when you try to add `2 + 2`, the
computer will concatenate them into`22`. Therefore, it is important to set the right data type when creating a column.

There are many different data types. Each SQL database flavor _(such as MySQL, SQLite, and Postgres)_ supports slightly
different data types.

### Avoid Data Redundancy

Let's consider the `author` column from the blog table in the example above. Right now, there is one column for the
author, which stores their name. However, we will likely want to store more information about the authors, such as their
email address, social media, and so on.

We _could_ put this author information directly into the blog data table, but let's say in our data model we also treat
authors as users. So we will want to add information such as their name and social media again separately over in the
`user_details` data table.

![Redundant Data Model - In the Blog and a User Details Table](image.webp)

However, there are big problems with this approach:

We have no way to link information from user details over to blog posts. If, later in the future, we wanted to change
the blog posts to include the author's social media, this could be very difficult. If two authors had the same name, it
would be difficult or impossible to know which one's social media information to use.... You may think, _well what if we
linked the social media to both the user details and blog tables?_

If the author decides to change their social media information under `user_details`, that author will have to go through
and update their social media on every single row containing their blog posts. With just 10 or even 100 blog posts, this
would be annoying but perhaps not a massive problem.

However, as the data increases in volume, redundant data does becomes a serious problem. If you have the same
information repeated again and again over trillions of bank transactions, storage is wasted and the time it takes to get
information out of the database drops on a massive scale... _Not to mention the risk of having conflicting or outdated
banking information across the database!_

### Why We Use Relational Data Models

To avoid data redundancy, it is always best practice to keep your data model D.R.Y. (which stands for Don't Repeat
Yourself). This is where _relational_ part of relational data models comes into play. You want to make sure that all
data is unique. It should be input one time, in one location. The best way to do this is to think of table rows as if they were distinct items and only assign columns which store information those specific items. To continue the blog and authors example, your blogs table would only contain information about the blog posts. The user details table will only contain information about authors. Then
you relationally link the two data tables, with their Primary Keys.

![Blogs and Users Relationally Linked](image.webp)

Instead of just placing the author name into a column on the `blogs` data table _(and creating redundancy)_, we create a
column to add the Primary Key from `user_details`. Remember, each Primary Key in a data table is unique, which means if
we can find the primary key for our blog post's author, we can be certain the data _(the author name, social media
link)_ is all accurate and up to date. Also, whenever you have a column for another data table's Primary Key, it is
called a _Foreign Key_. This naming convention helps avoid confusion.

There are several ways you can relationally link tables:

- **One to One** — Each row in a parent data table can link to one other row in the related table.
- **Many to One** — Many rows in a parent data table can link to one row in another data table.
- **One to Many** — Each row in a data table can link to many rows in another data table.
- **Many to Many** — Many rows in a data table are linked to many rows in another data table.\
  Many to many relationships require a special junction data table to manage these relationships.
- **Many to Any** — Many Rows in a data table link to many rows across any other data tables.\
  requires a junction data table, like many-to-many relationships, but also requires the a column for the data table name.

_Note that in the data model, Many-to-One and One-to-Many relationship are exactly the same type of relationship._

### Working With Relational Data Models

<!-- Create video or Vue Component demonstrating each of these components. -->

Even for developers with strong data and SQL skills, building out APIs and GUIs to build and manage a data model
is time consuming. To those who are unfamiliar, the SQL language, classic ORMs, querying/viewing raw data, and
traditional relational database jargon can be confusing, counterintuitive, and technical. In fact, business users may be
completely unaccustomed to think about certain things, such as blog posts or geo-positions, in terms of queries and
relational data tables. It is not practical to teach everyone on the team how to work and think in terms of raw data.
Business users may find it difficult to think of data without thinking about how it is represented and interacted with:
colorful, stylized, embedded on a map, etc.

## Data Models in Directus

All relational data model concepts listed above apply in Directus. However, Directus offers a data studio for no-code relational data model configuration. Much of the traditional relational database jargon is replaced with more user-friendly terms, to make things easier for business users. The data model is seen as _[Collections](#collections) of [Items](#items), composed of [Fields](#fields)_.

In order to make data easier to work with, the Directus App lets you customize how data is displayed and interacted
with. You will be able to provide an intuitive data management experience for even the most non-technical users.

<video title="Settings > Data Model" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Data Model configuration takes place across the following pages:

**Settings > Data Model > [Collection] > [Field]**

Across those pages, you have the power to do the following things, without a line of code or SQL:

- Configure and manage your relational data model and asset storage.
- Configure how data is displayed in the data studio.
- Configure how data is interacted with by users in the data studio.
- Translate your entire app and all its data into any language.

The following sections will further introduce **Settings > Data Model** and also map Directus' data model concepts to
the classic relational database concepts described in [Relational Data Models](#relational-data-models).

## Collections

<video title="Collections" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

A Collection is a data table, and is composed of [Items](#items), which are a group of [Fields](#fields). You can see a
list of all the Collections, including System Collections, under **Settings > Data Model**. From there, click a
Collection to open its Configuration Page. To learn more, see our guides on
[Collections](/configuration/data-model/collections) and
[System Collections](/configuration/data-model/system-collections).

:::warning Composite Keys

Directus does not currently support Composite Keys. If your project uses composite keys, you will need to make an
adjustment to the data model.

:::

:::warning SQL Views

Directus does not currently support creation of virtual tables via SQL Views.

:::

## Items

<video title="Collections" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Items are rows within the database. We use the term Items because rows frequently represent some real life Item, such as
a user, blog post, IoT event, bank transaction, etc. You typically view and manage Items across the other app modules,
such as [Content](/app/content), [User Directory](/app/user-directory), and [File Library](/app/file-library), though
this is not always true for [System Collections](/configuration/data-model/system-collections).

## Fields

<video title="Fields" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Fields are database columns. You can configure a Collection's Fields under **Settings > Data Model > [Collection] >
[Field]**, which opens the field configuration drawer, which is composed of the following sections.

- [Schema]() — Controls the technical details of the Field's database column.
- [Relationship]() — Only shown when configuring relational fields. Configures relationships and relational Triggers.
- [Field]() —
- [Interface]() — Customization options that set how users interact with the data.
- [Display]() — This pane includes any customization options that may be defined by the Display.
- [Validation]() — Set logic to determine whether an input value is valid and can be stored.
- [Conditions]() — Alter the current Field's configuration based on the values from other Fields in the Item.

Consider these Field Configuration sections carefully. An SQL database columns store pure, raw data. From there, developers build out custom logic and permissions to determine how this data is displayed and interacted with. In Directus, when you configure a field, you configure the database column as well as the logic the Directus data studio uses to display and interact with data.

## Data Types

In order to support all flavors of SQL, Directus abstracts away data type differences with a
[Data Type Superset](/getting-started/glossary/#data-type-superset)

## Keys and IDs

<video title="Keys and IDs" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

When you create a Collection, you must add an ID Field _(Primary Key)_ so the database can keep proper track of the
Collection's Items and their [relations](#relations) in other Collections, if any exist. Directus supports the following
types of IDs and you will define your ID every time you
[create a Collection](/configuration/data-model/collections/#create-a-collection).

- **Auto-Incremented Integer**
- **Auto-Incremented Big Integer** _(MySQL and PostgreSQL only)_
- **Generated UUID**
- **Manually Entered String**

## Relationships

<video title="Relations" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Directus supports all standard relationship types, as well as a few more of its own _compound_ types.To learn more,
see our guide on [Relationships](/configuration/data-model/relationships)
