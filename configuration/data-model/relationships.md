# Relationships

> Relationships are a crucial part of any relational database. Directus supports all standard relationship types, as well as a few more of its own _compound_ types, which offer greater flexibility.

[[toc]]

:::tip Before You Begin

<!-- [Data Model](/configuration/data-model). -->

Regardless of the relationship you want to configure, we recommend you read every section of this document, in order, at least once.
This is because, in Directus, _you must understand how M2Os work to understand O2Ms, you must understand M2Os and O2Ms to understand M2Ms, and so on._

:::

<!--
:::tip Learn more

ok

:::
-->

## Overview

Directus makes _the process_ of configuring relational data models easier, faster and more intuitive. Directus _does not_ enforce any opinionated schema, rule system, or limitations to your data models. Aside from the core requirements for relational databases, like having a primary key field for every collection or a data type assigned to every field, you are free to build the data model as you want, _for better or worse._

Therefore, in order to build an appropriate, efficient, and effective relational data model, you still need a rock solid _conceptual_ understanding of relational data models. In this guide, we will go over the following:

- What kinds of relationships exist in Directus and what they do to the data model.
- When it would be appropriate to use a given type of relationship.
- How to configure the desired relationship within the Directus data studio.

:::tip Parent vs. Related Collections

As you read through this document, remember that relationships are always described relative to their "parent" collection. However, this naming convention is based on perspective. For example, a [many-to-one relationship](#many-to-one-m2o) can also be seen as a [one-to-many relationship](#one-to-many-o2m), it just depends on which collection you consider the parent.

:::

## Many-to-One (M2O)

<video title="Configure an M2O Relationship" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

The easiest way to configure an M2O is to follow the guide on how to [create a field (standard)](/configruation/data-model/fields/#create-a-field-standard) and select the M2O Interface from the template wizard.

In an M2O relationship, multiple items from the parent collection are linked to one single item in the related collection.
For example, a city can only be in one country, but a country can have many cities.

![Many-to-One Relational Diagram](image.webp)

To achieve this kind of relationship, the parent collection must have a foreign key field to link items from the related collection. Therefore, we create a `country_id` field in the `cities` collection, to store the ID of the relevant country.

```
cities (the "many" collection)
- id (primary key)
- name
- country_id (a Foreign Key field, stores a key from countries.id)
```

```
countries (the "one" collection)
- id (primary key)
- name
```

Once the M2O field is configured, you will be able to view and interact with the related collection's items within the parent collection, `cities`. However, an M2O field does not provide a way display or interact with the data inside the related collection, `countries`. This is where O2M fields come in to play.

## One-to-Many (O2M)

<video title="Configure an M2O Relationship" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

The easiest way to configure an O2M is to follow the guide on how to [create a field (standard)](/configruation/data-model/fields/#create-a-field-standard) and select
the O2M field type from the template wizard.

To explain what an O2M is, let's continue on with the example relationship used above, linking the `city` and `country` collections.

![One-to-Many Relational Diagram](image.webp)

An O2M relationship is the exact same relationship as an M2O, seen from the other collection's perspective. Therefore, configuring an O2M field does not produce a new column in the database. In Directus, O2Ms are stored as an [Alias](/getting-started/glossary/#alias) field, which dynamically lists all items connected via the M2O.

In other words, when you are in the content module on the parent collections' item detail page:

- When you configure an M2O, one country can be viewed and managed on item detail pages in the `city` collection
- When you configure an O2M, all cities can be viewed and managed on item detail pages in the `country` collection.

At first glance, this O2M alias field might make it _look and feel_ like a new `cities` column was created for the `countries` data table:

```
countries (the "One" Collection)
- id (primary key)
- name
- cities (the O2M alias field, displays cities linked to the country)
```

```
cities (the "Many" Collection)
- id (primary key)
- name
- country_id (an M2O field, stores the key from countries.id, creating the relationship in the data model)
```

But again, this O2M alias field _does not add a column_ to the data table. This field type is _virtual_, just an Interface generated by Directus, so that you can also view and manage cities from within the `countries` collection.

## One-to-One (O2O)

<video title="Configure an O2O Relationship" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

In an O2O relationship, one item from the parent collection can be linked with one item from the related collection. Directus does not include a dedicated One-to-One (O2O) relationship type or Interface. However, in the database, O2O is almost exactly the same as an M2O. The only difference is that an O2O enforces _cardinality_, or in other words, it enforces this rule:

_If an item in the parent collection is linked to an item in the child collection, it cannot be linked to another item, and vice-versa_.

The easiest way to configure an O2O is to follow the guide on how to [create a field (standard)](/configruation/data-model/fields/#create-a-field-standard) _and select the **M2O** field type_ from the template wizard. Then, [configure the field's Schema](/configuration/data-model/fields/schema), toggling on **Unique** so that each value in the M2O field is unique, resulting in an O2O relationship.

To understand when you'd need to use an O2O relationship, let's continue the example from the above sections on M2O and O2M relationships. You may want to link a `current_world_leaders` collection to our `countries` collection. Since one given country has only one president, prime minister, dictator, king, _etc_., at a given point in time, we'll link them with an O2O relationship.

![One-to-One Relational Diagram](image.webp)

To further demonstrate how similar this is to an M2O, let's take a look at the raw schema. We simply add a relational field to the `current_world_leaders` collection:

```
current_world_leaders
- id
- leader_name
- country_id (an M2O, configured so that each value must be unique)
```

```
countries
- id (primary key)
- name
```

With an O2O, we could also choose to add the relational field to the `countries` collection:

```
countries
- id (primary key)
- name
- current_leader_id (an M2O, configured so that each value must be unique)
```

```
current_world_leaders
- id (primary key)
- leader_name
```

It doesn't matter which collection you create the O2O relational field on. However, since the O2M field is really just an M2O field behind the scenes and since Directus doesn't automatically display M2O fields in the related collection, you may also want to [configure an O2M field](#one-to-many-o2m) so that you can add, view, and manage a related item from the related collection as well.

## Many-to-Many (M2M)

<video title="Configure an O2O Relationship" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

The easiest way to configure an O2O is to follow the guide on how to [create a field (standard)](/configruation/data-model/fields/#create-a-field-standard) and select **Many to Many** from the template wizard.

The relationship types we have seen so far only required one column _(and thus one field)_ in the parent collection to store foreign keys from the related collection. For the M2O and O2O fields, _remember we saw an O2O is really an M2O that enforces cardinality_, the column existed in the parent collection _(on the 'M' side of the relationship)_. In the O2M alias fields, this column existed in the related collection _(also on the 'M' side)_.

In relational databases, M2M relations require _two real columns in the database_. These two columns go on neither the parent collection nor related collection. Instead, these two columns must be stored in a third collection, called a _junction collection_.

Junction tables are required in M2M relationships because the number of relationships created can _(and often will)_ outnumber the number of rows in either data
table. Junction tables provide a place to store all the relationships between items, no matter how many exist.

To demonstrate this, let's think about the relationship between recipes and ingredients: a _recipe_ can have many _ingredients_, and _ingredients_ can be in many _recipes_.

![Many-to-Many Relational Diagram](image.webp)

Let's take a look at the schema created when we configure an M2M in Directus:

```
recipes (collection)
- id (primary key)
- name
- ingredients (an O2M alias field, displays items linked from recipe_ingredients)
```

```
recipes_ingredients (junction collection)
- id (primary key)
- recipe (an M2O, stores foreign key from recipes.id)
- ingredient (an M2O, stores foreign key from ingredients.id)
- quantity (optional a regular field, stores other data on the connection)
```

```
ingredients (collection)
- id (primary key)
- name
```

Note the following points, demonstrated in the example schema above. When we create an M2M in Directus:

- Our junction collection, `recipe_ingeredients`, is generated with two M2Os.
- An O2M [alias](/getting-started/glossary/#alias) field is created on `ingredients`.
- The M2M relationship is created within the `recipes` collection. Directus does not automatically add an O2M for the `ingredients` collection.
  However, you can configure another O2M if desired:

```
ingredients (collection)
- id (primary key)
- name
- recipes (an O2M alias field, displays the recipes linked to each ingredient)
```

- Notice that the junction collection also has a `quantity` field, which tracks how much of each
  ingredient is needed for the recipe. You can add any number of fields needed to a junction collection.

::: tip Self-Referencing M2Ms

You can also have an M2M relationship that connects items within the _same collection_. One example of this is "Related
Articles", where an article might relate to many other articles.

:::

## Many-to-Any (M2A)

<video title="Configure an M2A Relationship" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

The easiest way to configure an M2A is to follow the guide on how to [create a field (standard)](/configruation/data-model/fields/#create-a-field-standard) and select the **Many to Any** Interface from the template wizard.

Sometimes called a _matrix field_ or _replicator_, the M2A relational type allows you to link items from the parent collection to any item in any collection in the database. Similar to the M2M, the M2A requires a junction collection and an O2M [Alias](/getting-started/glossary/#alias) field is automatically created for you. The major difference is that the junction collection on an M2A also has a field to store the **collection key** for related collections.

One common example of when M2As are used is for _page builders_, which have a `pages` collection that combines multiple _page section_ collections, such as `heading`, `text`, `image`, _etc_.

![Many-to-Any Relational Diagram](image.webp)

Let's look at the raw schema:

```
pages (collection)
- id
- name
- sections (an O2M alias field, displays items from page_sections)
```

```
page_sections (junction collection)
- id
- pages_id (an M2O, stores foreign key, from pages.id)
- collection (an M2O, stores name of the related collection, for example "headings", "text", or "images")
- item (an M2O, stores foreign key, from either headings.id, text.id or images.id)
```

```
headings (collection)
- id
- title
```

```
text (collection)
- id
- text
```

```
images (collection)
- id
- file
```

Note the following points from the schema above. When we create an M2A in Directus:

- Compared to the M2O and M2M relationships, there is often a lower likelihood that you will need to configure O2Ms on related collections, such as `headings`, `text` and `images`.
- Each collection in a database has a unique name, so this serves as an adequate unique identifier in the `page_sections.collection` field.

## Translations (O2M)

<video title="Configure an M2A Relationship" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

The easiest way to configure a Translations relationship is to follow the guide on how to [create a field (standard)](/configruation/data-model/fields/#create-a-field-standard) and select the **Translations O2M** Interface from the template wizard.

Directus provides this special Interface, which is optimized Translations. When you create a Translations O2M:

- An M2M, aka a junction collection, links your parent collection and a related `languages` collection.
- All your translations are stored within context fields, configured by you, on the junction collection.
- A special Translations O2M [Alias](/getting-started/glossary/#alias) field, designed specifically to display and manage translations, is created on the parent collection.

To demonstrate, let's create a translation relation for `articles`, a common content type that you may want to translate:

![Translations O2M](image.webp)

Let's take a look at the schema:

```
articles (collection)
- id (primary key)
- author (a field that is not translated)
- date_published (a field that is not translated)
- translations (an O2M alias field, displays items from article_translations)
```

```
article_translations (collection)
- id (primary key)
- article (an M2O, stores foreign key article.id)
- language (an M2O, stores foreign key languages.language_code)
- title (stores one translation of the Article Title)
- text (stores one translation of the Article Text)
```

```
languages (collection)
- language_code (a primary key, it is a manually typed language code, e.g., "en-US")
- name  (stores the language name, e.g., "English")
```

Note the following points from the schema above. When we create a Translations relation in Directus:

- You add **non-translated** fields, such as the `author` and `publish_date`, to the parent collection.
- As demonstrated by `article_translations.title` and `article_translations.text`, any **translated** fields should be added as context fields on the Translations
  junction collection.

:::tip

Remember, you are not bound to use this relationship for translations, you can build your data model as desired. You could create individual fields for each translation, such as `title_english`, `title_german`, `title_french`, and so on. However, this is not easily extensible and it creates a sub-optimal experience to have every single translation of every field on the item details page.

:::

::: tip Translating Parent Fields

There may come a time when you want to make an pre-existing parent field translatable. To do this, you can [duplicate a field](/configuration/data-model/fields/#duplicate-a-field), move it to the translation collection, and then delete the parent field. However, be aware that duplicating a field does _not_ duplicate any existing field values.

:::
