# Relationship

This pane is only shown when configuring relational fields (including images and translations). Depending on the type of
relationship, you'll be presented with one of the following set of options:

- [Many-to-One](/configuration/data-model/relationships/#many-to-one-m2o)
- [One-to-Many](/configuration/data-model/relationships/#one-to-many-o2m)
- [Many-to-Many](/configuration/data-model/relationships/#many-to-many-m2m)
- [Many-to-Any](/configuration/data-model/relationships/#many-to-many-m2m)
- [Translations](/configuration/data-model/relationships/#translations-o2m)
- [One-to-One]()

::: tip Corresponding Field

[Relationships go both ways](/configuration/data-model/relationships/#perspective-matters), so when creating a new
relation Field, Directus offers to automatically create the corresponding Field on the related Collection.

:::

<!--
M2O

If you select or enter an **existing** related collection, then the primary key field is known and
automatically selected. If you enter the name of a **new** related collection, which doesn't already exist, you will also be
prompted to enter the name of its primary key field, which will default to an auto-incremented integer.

**Relational Triggers** allow you to control what happens when a relationship is broken. There is one option:

- **On Delete of [Related Collection]** — When the related item (O2M) is deleted...
  - Nullify the parent M2O field (default)
  - Set the parent M2O field to its default value
  - Delete the parent collection's item (cascade)
  - Prevent the deletion
-->

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

<!--
M2M
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

### Unconfigure an M2M

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

<!--
M2A

You can then artificially limit which collections are valid through a related collections "allow list".

### Configure an M2A
The parent collection and field are already known (it's the field you're currently creating), so configuring those are

Next, we configure the [Junction Collection](/getting-started/glossary/#junction-collections), which sits between the
related collections, storing all links between them. You can leave this set to "Auto-Fill", which sets intelligent
naming defaults, or disable it to select existing options or enter custom names.

Lastly, you should select any desired Related Collections. Unlike other relationships, you can't _create_ these related
collections here, so ensure all related collections you need are created beforehand.

The optional **Sort Field** can be used to enable manual reordering of items within this M2A field. This is configured
by selecting an existing numeric type field (highlights green) from the Junction Collection, or entering the name of a
new field to be created.


::: tip Auto-generating

If you enter a collection/field name that doesn't exist yet, Directus will auto-generate these collections/fields once
you save the changes on the new M2A field.

:::

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

<!--
Translations
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
-->
