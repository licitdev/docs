# Fields

> Fields are essentially database columns. But they also allow you to custom configure how values are displayed and interacted with in the data studio.

[[toc]]

## Overview

<video title="Fields Overview" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

**Fields and Layout** — Create, view, and configure a collection's fields as well as adjust how they are displayed and ordered on the [Item Details Page](/app/content/collections/#item-page). This section also provides access to the **Field Context Menu** and **Field Configuration Drawer**, described below.

**Field Context Menu** — Contains the following controls:

- [<span mi icon>edit</span> Edit Field](#configure-a-field) — Opens the Field Configuration Drawer.
- [<span mi icon>content_copy</span> Duplicate Field](#duplicate-a-field) — Duplicates a field along with all of its configuration options.
- [<span mi icon>visibility_off</span> Hide Field on Detail](#toggle-field-visibility-for-admins) — Toggle field visibility on the Item Detail Page for Admin Users.
- [Width](#adjust-field-width) — Fields have three different width options:
  - <span mi icon>border_vertical</span> **Half Width** — The field is shown at half the form width.
  - <span mi icon>border_right</span> **Full Width** — The field is shown at the full form width. _The Default_
  - <span mi icon>aspect_ratio</span> **Fill Width** — The field is shown filling the full width of the page area.

**The Field Configuration Drawer** — Provides all [field configuration](#configure-a-field) options.

## Field Types

<!--
### Standard Fields
### Presentation and Aliases
### Field Groups
### Single File
### Multiple Files
### Relationship Fields
### Translation Fields
-->

## Create a Field (Standard)

<video title="Create a Field (Standard)" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

To make field configuration as intuitive and easy as possible, a template wizard is provided so that you can create fields pre-configured for common use-cases. When you create a field this way, you will still have full power to [configure the field](#configure-a-field) as desired.

1. Navigate to **Settings > Data Model > [Collection]**.
2. Under **Fields & Layout**, click the **Create Field** button.\
   A side drawer will open, with various pre-configured Interfaces to choose from.
3. Click to select the desired field and a basic configuration menu will open.
4. Add a **Field Key**, _which is also used as the default field name_.\
   Optional: Configure the the other field details as desired.\
   Optional: Click [Continue in Advanced Field Creation Mode](#create-a-field-advanced).
5. When you are ready, click **Save** to confirm.

## Create a Field (Advanced)

<video title="Create a Field (Standard)" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

This field creation method opens the field configuration drawer, so you can customize every detail, from the start. To create a field in advanced mode, follow these steps.

1. Navigate to **Settings > Data Model > [Collection Name]**.
2. Under **Fields & Layout**, click the **Create Field in Advanced Mode** button.\
   A dropdown menu will appear with various field types to choose from.
3. Click to choose the field type and the Field Configuration Drawer will open.
4. Configure your field as desired.
5. Click <span mi btn>check</span> to confirm.

::: tip Database Columns

Remember, a field is a database column. Therefore, you can create a column directly in the
database and it will automatically appear within Directus. You can then enhance the
experience further by configuring it as desired.

:::

## Configure a Field

<video title="Configure a Field" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

The Field Configuration Drawer is composed of eight sections. These provide extensive customization options, from
the database column's technical details, to how it is displayed and interacted with, and even custom input validation
and conditional display logic. To configure a field, follow these steps.

1. Navigate to **Settings > Data Model > [Collection Name]**.
2. Under **Fields & Layout**, click the field you want to update.\
   The field configuration drawer will open.
3. Navigate to one of these sections and configure the field as desired:
   - [Schema](/configuration/data-model/fields/schema) — Defines the database column schema for the field.
   - [Relationship](/configuration/data-model/fields/relationship) — Controls _and only appears when configuring relational_ field details.
   - [Translations](/configuration/data-model/fields/translations) — Controls _and only appears when configuring translation_ field details.
   - [Field](/configuration/data-model/fields/field) — Sets details for the field input, which is displayed on the [item page](/app/content/items/).
   - [Interface](/configuration/data-model/fields/interface) — Configures how you interact with the field's values.
   - [Display](/configuration/data-model/fields/display) — Configures how field values are displayed in the Data Studio.
   - [Validation](/configuration/data-model/fields/validation) — Creates a filter to determine valid user input.
   - [Conditions](/configuration/data-model/fields/conditions) — Alters the current field's setup based on the values of other fields.
4. Click <span mi btn>check</span> to confirm.

::: tip Fields in System Collections

While all out-of-the-box fields within system collections are locked from configuration or deletion, you are able to
create new fields within system collections. To get started, expand system collections from the bottom of **Settings > Data Model**.

:::

## Duplicate a Field

<video title="Duplicate a Field" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

To duplicate a field, follow these steps:

1. Navigate to **Settings > Data Model > [Collection Name]**.
2. Click the <span mi icon>more_vert</span> icon for the field you want to duplicate.
3. Click the <span mi icon>content_copy</span> **Duplicate Field** option.
4. Choose the collection you'd like to create the field in and set the Field Name.

::: warning Relational and Primary Key Fields

Currently, it is not possible to duplicate relational fields or a collection's primary key.

:::

:::tip

When you duplicate a field, all of its configuration settings will be copied as well. However, values stored within that field will not be copied.

:::

## Toggle Field Visibility (for Admins)

<video title="Toggle Field Visibility (for Admins)" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

For users with any non-admin role, A field's visibility can be adjusted via [access permissions](/configuration/users-roles-permissions.md). However, you may want to hide certain fields for admins as well. This is handy if the field is distracting or has no need to be seen on the item details page.

## Adjust Field Width

<video title="Group and Sort Field" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Adjusting the field width in **Fields and Layout** will change field width on the [Item Detail Page](/app/content/#item-page). To adjust field width, follow these steps.

1. Click <span mi icon>more_vert</span> to open the field's context menu.
1. Choose one of the following:
   - <span mi icon>border_vertical</span> **Half Width** — The field is shown at half the form width.
   - <span mi icon>border_right</span> **Full Width** — The field is shown at the full form width. _The Default._
   - <span mi icon>aspect_ratio</span> **Fill Width** — The field is shown filling the full width of the page area.

## Manually Sort Fields

<video title="Group and Sort Field" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Adjusting the field order in **Fields and Layout** will change the order on the [Item Detail Page](/app/content/collections/#item-page). To manually sort fields, click <span mi icon>drag_indicator</span> to drag and drop the field as desired.

<!-- MOVED TO configuration/data-model/collections.md
## Configure a Sort Field

<video autoplay muted loop controls title="Batch Edit Items">
	<source src="https://cdn.directus.io/docs/v9/app-guide/content/content-collections/content-collections-20220415A/manually-sort-items-20220415A.mp4" type="video/mp4" />
</video>

The sort feature enables Users to manually sort Items within the Directus App. This is typically shown on the Collection Page and it can
also be used for sorting Items within [Junction Tables](/getting-started/glossary/#junction-collections). To configure a Sort Field,
follow these steps.

1. [Create a Field](#create-a-field-standard) with an `INTEGER` data type.\
	You may want to set this field to be "hidden" so it doesn't show up within the Item Details Page.
2. Choose your Sort Field from the dropdown under **Settings > Data Model > [Collection] > Sort**.
3. Next, you may want to learn how to [manually sort Items](/app/content/collections/#manually-sort-items).

::: tip Automatic Setup

When you [create a Collection](/configuration/data-model/collections/#create-a-collection), you have the option of creating a
**Sort** field. If you choose to include this Field, the Collection's sort settings will automatically be configured for you.

:::

::: tip Interface Sorting

To configure manual sorting within an Interface (eg: M2M, O2M, or M2A), configure as above, but also set the **Sort
Field** on the Field's Relationship pane.

:::
-->

## Delete a Field

<video autoplay muted loop controls title="">
	<source src="" type="video/mp4" />
</video>

To permanently delete a field and all its stored values, follow these steps.

1. Navigate to **Settings > Data Model > [Collection Name]**.
2. Click the <span mi icon>more_vert</span> icon for the field you want to delete.
3. Click the <span mi icon>delete</span> **Delete Field** option.
4. Confirm this decision by clicking **Delete** in the dialog.

::: danger Irreversible Change

This action is permanent and can not be undone. Please proceed with caution.

:::
