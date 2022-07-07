# Relationships

> Relationships are a crucial part of any relational database. Directus supports all standard relationship types, as well as a few more of its own _compound_ types, which offer greater flexibility.

[[toc]]

<!--
:::tip Before You Begin

ok

:::

:::tip Learn more

ok

:::
-->

## Overview

Directus makes _the process_ of configuring relational data models easier, faster and more intuitive. Directus _does not_ enforce any opinionated schema, rule system, or limitations to your data models. Aside from the bare requirements for relational data models, like having a primary key field for every collection or a data type assigned to every field, you are free to build the data model as you want, _for better or worse._

Therefore, in order to build an appropriate, efficient, and effective relational data model, you still need a rock solid _conceptual_ understanding of relational data models. In this guide, we will go over the following:

- What kinds of relationships exist in Directus and what they do to the data model.
- When it would be appropriate to use a given type of relationship.
- How to configure the desired relationship within the Directus data studio.

:::tip Parent vs. Related Collections

As you read through this document, note that relationships are always described relative to their "parent" collection. However, this naming convention is based on perspective. For example, a [many-to-one relationship](#many-to-one-m2o) can also be seen as a [one-to-many relationship](#one-to-many-o2m), it just depends on which collection you consider the parent.

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

<!--
If you select or enter an **existing** related collection, then the primary key field is known and
automatically selected. If you enter the name of a **new** related collection, which doesn't already exist, you will also be
prompted to enter the name of its primary key field, which will default to an auto-incremented integer.
-->

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
- cities (the O2M alias field, shows cities that have this country's ID stored)
```

```
cities (the "Many" Collection)
- id (primary key)
- name
- country_id (a Foreign Key field, stores the key from countries.id)
```

But again, this O2M alias field _does not add a column_ to the data table. This field type is _virtual_, just an Interface generated by Directus, so that you can also view and manage cities from within the `countries` collection.

<!--
### Unconfigure an O2M

**Relational Triggers** allow for control over what happens when a relationship is broken. There are two options:

- **On Deselect of [Related Collection]** — When the value of the M2O is deselected...
  - Nullify the M2O field value (default)
  - Delete the M2O item (cascade)
- **On Delete of [Parent Collection Item]** — When this O2M item is deleted...
  - Nullify the related M2O field (default)
  - Set the related M2O field to its default value
  - Delete the related collection's item (cascade)
  - Prevent the deletion
-->

## One-to-One (O2O)

<video title="Configure an O2O Relationship" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

In an O2O relationship, one item from the parent collection can be linked with one item from the related collection. Directus does not include a dedicated One-to-One (O2O) relationship type or Interface. However, in the database, O2O is almost exactly the same as an M2O. The only difference is that an O2O enforces _cardinality_, or in other words, it enforces this rule:

_If an item in the parent collection is linked to an item in the child collection, it cannot be linked to another item, and vice-versa_.

The easiest way to configure an O2O is to follow the guide on how to [create a field (standard)](/configruation/data-model/fields/#create-a-field-standard) _and select the **M2O** field type_ from the template wizard. Then, [configure the field's Schema](/configuration/data-model/fields/schema), toggling on **Unique** so that each value in the M2O field is unique, resulting in an O2O relationship.

<!--
To configure an O2O, follow these steps:
1. Follow the guide on how to [create a field (standard)](/configruation/data-model/fields/#create-a-field-standard).
2. Select the M2O field type from the template wizard.
3. Toggle **Unique** under the Schema section of the M2O's **field configuration drawer**.
4. Click <span mi btn>check</span> to confirm.
-->

To understand when you'd need to use an O2O relationship, let's continue the example from the above sections on M2O and O2M relationships. You may want to link a `current_world_leaders` collection to our `countries` collection. Since one given country has only one president, prime minister, dictator, king, _etc_., at a given point in time, we'll link them with an O2O relationship.

![One-to-One Relational Diagram](image.webp)

To further demonstrate how similar this is to an M2O, let's take a look at the raw schema.\
We simply add a relational field to the `current_world_leaders` collection:

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

It simply doesn't matter which collection you configure an O2O relationship to. However, since the O2M field is really just an M2O field behind the scenes, and within the data studio, M2O fields are only visible from the parent collection, you may also want to [configure an O2M field](#one-to-many-o2m) so that you can add, view, and manage a related item from the related collection's item detail page.

## Many-to-Many (M2M)

The Many-to-Many relationship is actually just **two relationships combined** (O2M+M2O) that join together three
different collections. The M2M is stored as an [Alias](/getting-started/glossary/#alias) field on its parent Collection
that dynamically lists all items connected via a [junction collection](/getting-started/glossary/#junction-collections).
For example, a _recipe_ can have many _ingredients_, and _ingredients_ can be in many _recipes_.

Below is an example of an M2M relationship:

```
recipes (Collection)
- id (Primary Key)
- name
- *ingredients* (M2M/O2M alias field that lists references from "recipe_ingredients")

ingredients (Collection)
- id (Primary Key)
- name
- *recipes* (M2M/O2M alias field that lists references from "recipe_ingredients")

recipe_ingredients (Junction Collection)
- id (Primary Key)
- recipe (stores the recipe key)
- ingredient (stores ingredient key)
- quantity (other optional data on the connection)
```

Notice that the example above also has a `quantity` field on the Junction Collection, which tracks how much of each
ingredient is needed for the recipe. You can add any other _contextual_ fields to the junction, and they will also be
included at the top of the App's relational Item Page form.

::: tip Self-Referencing

You can also have a M2M relationship that connects items within the _same_ collection. An example of this is "Related
Articles", where an article might relate to many other articles.

:::

### Configure an M2M

![M2M](https://cdn.directus.io/docs/v9/configuration/data-model/relationships/M2M-20220216A.webp)

The parent collection and field are already known (it's the field you're currently creating), so configuring those are
disabled.

Next you should configure the Related Collection. If you select or enter an **existing** Related Collection (highlights
green) then the primary key field is known and automatically selected. If you enter the name of a **new** Related
Collection (doesn't already exist), you will also be prompted to enter the name of its primary key field, which will
default to an auto-increment integer type.

Lastly, we configure the [Junction Collection](/getting-started/glossary/#junction-collections), which sits between the
two related collections, storing all links between the two. You can leave this set to "Auto-Fill", which sets
intelligent naming defaults, or disable it to select existing options or enter custom names.

You also have the option to create a **Corresponding Field** during this process. This allows you to more easily create
the reverse M2M field on the _related_ collection.

The optional **Sort Field** can be used to enable manual reordering of items within this O2M field. This is configured
by selecting an existing numeric type field (highlights green) from the Junction Collection, or entering the name of a
new field to be created.

<!-- ### Unconfigure an M2M

**Relational Triggers** allow for control over what happens when a relationship is broken. There are three options:

- **On Deselect of [Junction Collection]** — When the value of this M2M is deselected...
  - Nullify the junction field (default)
  - Delete the junction item (cascade)
- **On Delete of [Parent Collection Item]** — When this M2M item is deleted...
  - Nullify the junction field (default)
  - Set the junction field to its default value
  - Delete the related junction item (cascade)
  - Prevent the deletion
- **On Delete of [Related Collection Item]** — When the related M2M item is deleted...
  - Nullify the junction field (default)
  - Set the junction field to its default value
  - Delete the related junction item (cascade)
  - Prevent the deletion -->

## Many-to-Any (M2A)

Sometimes called a "matrix field" or "replicator". Like the M2M, the M2A is stored as an
[Alias](/getting-started/glossary/#alias) field on its parent Collection that dynamically lists all items connected via
a [junction collection](/getting-started/glossary/#junction-collections). However, there is one key difference: one side
of the junction also stores a **collection key**. This combination of collection name and primary key means that you can
effectively store a reference to _any_ item in the database. You can then artificially limit which collections are valid
through a related collections "allow list".

A common example of a M2A is a "Page Builder", which has a _Pages_ collection that includes any number of different
"section" Collections, such as: "Heading", "Text", and "Image". In this example the junction table will link different
sections (from different collections) with a page, creating relational layouts.

Below is an example of a M2A relationship:

```
pages (Collection)
- id
- name
- *sections* (O2M/M2A alias field that lists references from "page_sections")

page_sections (Junction Collection)
- id
- pages_id (stores the primary key of the parent page)
- collection (stores name of the related collection, for example "headings", "text", or "images")
- item (stores the primary key of the related item)

headings (Collection)
- id
- title

text (Collection)
- id
- text

images (Collection)
- id
- file
```

### Configure an M2A

<!--
The parent collection and field are already known (it's the field you're currently creating), so configuring those are

Next, we configure the [Junction Collection](/getting-started/glossary/#junction-collections), which sits between the
related collections, storing all links between them. You can leave this set to "Auto-Fill", which sets intelligent
naming defaults, or disable it to select existing options or enter custom names.

Lastly, you should select any desired Related Collections. Unlike other relationships, you can't _create_ these related
collections here, so ensure all related collections you need are created beforehand.
-->

::: tip Auto-generating

If you enter a collection/field name that doesn't exist yet, Directus will auto-generate these collections/fields once
you save the changes on the new M2A field.

:::

The optional **Sort Field** can be used to enable manual reordering of items within this M2A field. This is configured
by selecting an existing numeric type field (highlights green) from the Junction Collection, or entering the name of a
new field to be created.

<!--
**Relational Triggers** allow for control over what happens when a relationship is broken. There are three options:

- **On Delete of [Parent Collection Item]** — When a M2A item is deleted...
  - Nullify the junction field (default)
  - Set the junction field to its default value
  - Delete the related junction item (cascade)
  - Prevent the deletion
- **On Deselect of [Junction Collection]** — When the value of this M2A is deselected...
  - Nullify the junction field (default)
  - Delete the junction item (cascade)
-->

## Translations (O2M)

The Translations relationship is just a special version of the standard O2M. Just like the O2M, it creates an
[Alias](/getting-started/glossary/#alias) field that is used to list all related items (the translations). Translations
themselves are stored in a separate collection, which is then related to a _third_ collection that stores all
languages.

While you could create individual fields for each translation, such as `title_english`, `title_german`, `title_french`,
and so on, this is not easily extensible, and creates a less than ideal form layout. Instead, you can use the Directus
_relational_ [Translations O2M](/configuration/data-model/relationships/#translations-o2m) interface. This uses a
separate collection to store an endless number of translations, and a separate collection of languages that can easily
be added to without having to change the schema.

Let's take a look at a basic example for "Articles":

- **`articles` Collection**
  - `id` — (Primary Key)
  - `author` — Field that is not translated
  - `date_published` — Field that is not translated
  - `translations` — A O2M relational field to `article_translations`
- **`article_translations` Collection**
  - `id` — (Primary Key)
  - `article` — The key of the article this belongs to
  - `language` — The language key of this translation
  - `title` — The translated Article Title
  - `text` — The translated Article Text
- **`languages` Collection**
  - `language_code` — (Primary Key) eg: "en-US"
  - `name` — The language name, eg: "English"

As you can see above, you add **non-translated** fields, such as the `author` and `publish_date`, to the parent
collection. Any **multilingual** fields, such as Title or Text, should be added directly to the Translation Collection.
You can not simply drag or shift fields from the parent to translations, they must be _created_ in the correct
collection.

::: tip Translating Parent Fields

To make an existing parent field translatable, you can choose "Duplicate Field" from its context menu, move it to the
translation collection, and then delete the parent field. However, be aware that this does **not** maintain any existing
field values in the process.

:::

### Configure an O2M for Translations

![Translations](https://cdn.directus.io/docs/v9/configuration/data-model/relationships/translations-wizard-20220216A.webp)

The easiest way to add translations is to use the wizard, which only asks for the Translation field name. All required
fields and relationships will then be automatically created and configured.

![Translations](https://cdn.directus.io/docs/v9/configuration/data-model/relationships/translations-relations-20220216A.webp)

If you choose to switch to **manual setup**, you will be presented with a similar relationship setup to O2M or M2M. The
parent collection and primary key are known, so those fields are disabled.

Next, we configure the Translation Collection. Set to "Auto Fill" by default, this will enter intelligent naming based
on related names, and disables all fields. Disabling Auto Fill will enable all fields, allowing you to name the
collection that holds the translations, as well as the two fields (each a M2O) that store foreign keys to the parent
item and language.

Lastly, you would select/create the Languages collection, which stores the languages available for this translations
field. It is common practice to reuse a single languages collection throughout your project, unless translation fields
need to support different language sets. For the language `code` we recommend using the IETF language tag (eg: `en-US`)
which combines the ISO 639-1 and ISO 3166‑1 standards, but anything can be used (eg: `english`).