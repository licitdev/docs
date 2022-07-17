# Collections

> Collections are data tables and come with all the same power and functionality, despite the less technical name. Directus also provides table and field configuration options to fit common uses cases.

[[toc]]

<!--
:::tip Before You Begin

Quickstart Guide
Read Data Model Introduction

:::
:::tip Learn more

API documentation

:::
-->

## Overview

<video title="Overview" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

<!-- General introduction. Goal is to balance and contextualize "System Collections". -->

### System Collections

System Collections store the data and configuration details required to power your Directus project.

<video title="System Collections" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Since System Collections store configuration details integral to your Directus project, you cannot edit or delete them or any of their default fields, as reconfigurations would break your project. However, you _can_ create and configure new fields on any System Collection. This enables you to safely customize your data model as desired.

System collections are not displayed in the Content Module. The following sections will go over the logic and functionality each System Collection is responsible for, as well as where to find relevant App and API documentation.

| System Collection | Purpose                                                                                                                                     | APP                                                                                 | API                                              |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------ |
| Activity          | Tracks and stores all events within Directus, giving full accountability over everything.                                                   | [Actvitiy Log](/configuration/activity-log/)                                        | [Activity](/reference/system/activity/)          |
| Collections       | Stores data table & configuration details each and every Collection.                                                                        | [Collections](/configuration/collections)                                           | [Collections](reference/system/collections)      |
| Dashboards        | Stores dashboard configuration details and all relationally linked panels.                                                                  | [Insights](/app/insights)                                                           | [Dashboards](/reference/system/dashboards)       |
| Fields            | Stores configuration details for each field.                                                                                                | [Fields](/configuration/data-model/fields)                                          | [Fields](/reference/system/fields)               |
| Files             | This stores file location from asset storage as well as any data associated with that file.                                                 | [File Library](/app/file-library)                                                   | [Files](/reference/system/files)                 |
| Flows             | Stores basic logic required for Flows, which enable event-triggered task automation.                                                        | [Flows](/configuration/flows)                                                       | [Flows](/reference/system/flows)                 |
| Folders           | Stores information required for Folders, which provide virtual file asset management.                                                       | [Folders](/app/file-library/#folders)                                               | [Folders](/reference/system/folders)             |
| Migrations        | Used by our install/upgrade process to track when migration scripts for a specific release have been run.                                   | It has no relevant app functionality.                                               | **N/A**                                          |
| Notifications     | Stores details about in-app notifications.                                                                                                  | [Module Bar](/app/overview/#_1-module-bar) and [Sidebar](/app/overview/#_4-sidebar) | [Notifications](/reference/system/notifications) |
| Operations        | Stores information required for Operations, which are a part of [Flows](#flows).                                                            | [Operations](/configuration/flows)                                                  | [Operations](/reference/system/operations.md)    |
| Panels            | This stores information about individual analytics panels, which are displayed on [Dashboards](#dashboards).                                | [Insights](/app/insights)                                                           | [Panels](/reference/system/panels)               |
| Permissions       | This stores the access permissions configured for roles.                                                                                    | [Users, Roles & Permissions](/configuration/users-roles-permissions)                | [Permissions](/reference/system/permissions)     |
| Presets           | This stores details for presets and bookmarks.                                                                                              | [Presents & Bookmarks](/configuration/presets-bookmarks/)                           | [Presets](/reference/system/presets/)            |
| Relations         | This stores information about relationships between Collections.                                                                            | [Relationships](/configuration/data-model/relationships)                            | [Relations](/reference/system/relations/)        |
| Revisions         | Revisions, which are changes/edits made to Items. Directus keeps track of edits made, so you're able to revert to a previous state at will. | [Revert an Item](/app/content/items/#revert-an-item)                                | [Revisions](/reference/system/revisions/)        |
| Roles             | Stores information about each Role created.                                                                                                 | [Users, Roles, and Permissions](/configuration/users-roles-permissions)             | [Roles](/reference/system/roles/)                |
| Sessions          | Stores information about each user session, for system purposes.                                                                            | **N/A**                                                                             | **N/A**                                          |
| Settings          | Stores all configurations made within **Settings > Project Settings**.                                                                      | [Project Settings](/configuration/project-settings)                                 | [settings](/reference/system/settings/)          |
| Shares            | Stores all information regarding data shares.                                                                                               | [Data Sharing](/app/content/data-sharing/)                                          |                                                  |
| Users             | Stores information about each User within the platform.                                                                                     | [User Directory](/app/user-directory)                                               | [Users](/reference/system/relations/)            |
| Webhooks          | Stores all information about configured Webhooks.                                                                                           | [Webhooks](/configuration/webhooks/)                                                | [Webhooks](/reference/system/relations/)         |

:::warning

Webhooks are a deprecated feature and will be removed from the platform. This functionality has been fully replaced by [Flows](#flows).

:::

## Create a Collection

<video title="Create a Collection" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

To create a Collection, follow these steps.

1. Navigate to **Settings > Data Model** and click <span mi btn>add</span> in the page header and a drawer will open.
2. Enter a unique **Collection Name**. This cannot be modified later, but can be translated.\
   This is used as the data table key, API collection key, and default collection name throughout the App.
3. Optional: Make any other configurations as desired.
   - **Singleton** — Toggles whether the Collection is a [Singleton](#configure-a-collection-collection-setup).
   - **Primary Key Field** — Sets the name of the primary key field, defaults to `id`.
   - **Type** — Sets the [type of ID](/configuration/data-model/fields/#keys-and-ids) to use for this Collection.
4. Click <span mi btn>arrow_forward</span> to confirm.
5. Enable and rename the other Optional System Fields as desired:
   - **Status** — Stores item status.
   - **Sort** — Adds a field which enables drag-and-drop sorting of items.
   - **Created On** — Logs the date an item was created.
   - **Created By** — Logs the user that created this item.
   - **Updated On** — Logs the date an item was last updated.
   - **Updated By** — Stores the last user to edit the file.
6. Click <span mi btn>check</span> to confirm and create the collection.

::: danger Immutable Keys

The collection name from step two cannot be modified after collection creation. However, you can override how it is
displayed with [Collection Naming Translations](#collection-setup).

:::

:::warning Composite Keys

Directus does not currently support composite keys. If your project uses composite keys, you will need to make an
adjustment to the data model.

:::

:::warning SQL Views

Directus does not currently support creation of virtual tables via SQL Views.

:::

::: tip Database Tables

Remember, a collection is simply a database table. Therefore, you can import or create a table directly in the
database and it will automatically appear within you Directus project. The first time you manage that table, a
`directus_collections` record will be created with default values.

:::

## Toggle Collection Visibility

<video title="Hide a Collection" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

To toggle whether a collection is hidden by default in the Content Module, follow these steps.

1. Navigate to **Settings > Data Model**.
2. Click <span mi icon>more_vert</span> to open the Collection's Context Menu.
3. Select **View Content** or **Make Collection Hidden** to adjust visibility as desired.

:::tip

Assuming a user has [access permissions](/configuration/users-roles-permissions), hidden collections can still be
viewed. They must right-click on the Navigation Bar and choose <span mi icon>visibility</span> **Show Hidden Collections**.

:::

## Create a Folder

<video title="Create a Folder" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Folders allow you to sub-nest and group how collections are displayed. This feature simply changes how the collections
are displayed under **Settings > Data Model** and in the Content Module. It has no impact on the data model. To create a
folder, follow these steps.

1. Navigate to **Settings > Data Model**.
2. Click <span mi btn>create_new_folder</span> in the page header.
3. Set a folder key, which will also be used as the folder's name.
4. Optional: Set the folder icon, color, note and translations as desired.
5. Click **Save** to create your folder.

## Toggle Folder Display

<video title="Toggle Folder Display" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

To toggle folder display, follow these steps.

1. Navigate to **Settings > Data Model**.
2. Click <span mi icon>folder</span> on the desired collection to toggle the following displays:
   - **Start Open**
   - **Start Collapsed**
   - **Always Open**

## Sort and Nest Collections

<video title="Configure a Collection" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

To sort and nest collections in **Settings > Data Model** and **Content Module**, follow these steps.

1. Navigate to **Settings > Data Model**.
2. Click and drag <span mi icon>drag_handle</span> to position collections as desired.\
   To nest a collection, drag below and to the right of an intended parent folder or collection.

## Configure a Collection

<video title="Configure a Collection" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

To configure a collection, follow these steps.

1. Navigate to **Settings > Data Model** and click the desired collection.\
   The collection's configuration page will open.
2. Make configurations as desired.
3. Click <span mi btn>check</span> to confirm.

The configuration options available on this page are broken into the following six categories.

- [Fields and Layout](/configuration/data-model/collections/#configure-a-collection-fields-layout)
- [Collection Setup](/configuration/data-model/collections/#configure-a-collection-collection-setup)
- [Archive](/configuration/data-model/collections/#configure-a-collection-archive)
- [Sort](/configuration/data-model/collections/#configure-a-collection-sorts)
- [Accountability](/configuration/data-model/collections/#configure-a-collection-accountability)
- [Duplication](/configuration/data-model/collections/#configure-a-collection-duplication)

### Fields & Layout

![Fields and Layout](image.webp)

This section allows you to create and configure fields, as well as configure how fields are displayed on the
[Item Details Page](/app/content/items). To learn more, please see the documentation on [Fields](/configuration/data-model/fields).

### Collection Setup

<video title="Collection Setup" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

These controls allow you to modify how the collection is displayed within the Content Module.

- **Collection Name** — Displays the collection's name, which cannot be modified. However, you can override the displayed name with Collection Naming Translations, shown lower on this list.
- **Note** — Set a helpful note that explains the collection's purpose.
- **Icon** — Set an icon used throughout the app when referencing this collection.
- **Color** — Set a color for the icon, shown in the Navigation Bar and its page header.
- **Display Template** — Create a [Display Template](/app/display-templates) for the collection.
- **Hidden** — Toggle whether the collection should be globally hidden in the other app modules, even for admin users. You cannot hide collections for admins via permissions like other roles, so this comes in handy to tidy up the Content Module.
- **Singleton** — Toggle to bypass the [Collection Page](/app/content/collections/) and take users to the
  [Item Details Page](/app/content/items/).\
- **Collection Naming Translations** — Translate the collection name across multiple languages. When the default
  language is changed in [Project Settings](/configuration/project-settings/#general) or
  [User Details Page](/app/user-directory/#user-details-page), the relevant translation, if any exists, will be used
  throughout the App.

:::tip Collection Naming Translations

By default, Directus uses the [Title Formatter](/getting-started/glossary/#title-formatter) to display collection keys
as human readable names, but you can also use translations to explicitly rename more technical table keys.

:::

:::tip What's a Singleton?

A Singleton is a collection that only contains one single item, such as the data for an **About Us** page on a website.

:::

### Archive

<video title="Archive" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

Selects a field to handle archiving items. This provides a _soft-delete_ functionality for items in a collection.
Archived items will still exist in the collection and database, but are filtered within the Data Studio.\
To configure an archive field, follow these steps.

- **Archive Field** — Selects the archive field from a dropdown menu.
- **Archive App Filter** — Toggles whether app users can
  [filter for archived items](/app/content/collections/#view-archived-items).
- **Archive Value** — A value that is assigned to the field when an item is
  [archived](/app/content/items/#archive-an-item).
- **Unarchive Value** — A value that is assigned to the field when an item is
  [unarchived](/app/content/items/#archive-an-item).

:::tip Automatic Setup

When you [create a collection](#create-a-collection), you have the option to create an optional Status Field. If
you choose to include this field, the collection's archive settings will be automatically configured for you.

:::

:::tip Archive Field Values

The archive fields can contain any number of additional values besides the archived and un-archived values defined above.

:::

:::tip Archived Item Management via API

Archived items are hidden in the app by default, but they are still returned normally via the API unless explicitly
filtered out. This gives you the flexibility to manage archived items however you want when working with the API.

:::

### Sort Field

<video autoplay muted loop controls title="Batch Edit Items">
	<source src="https://cdn.directus.io/docs/v9/app-guide/content/content-collections/content-collections-20220415A/manually-sort-items-20220415A.mp4" type="video/mp4" />
</video>

The sort feature enables users to [manually sort Items](/app/content/collection/#manually-sort-items) within the Data Studio. This is typically shown on the **Content Module > Collection Page**. It can
also be used for sorting items within [Junction Collections](/getting-started/glossary/#junction-collections). A standard field, configured with an `INTEGER` data type, is required. As shown in the video, fields which cannot serve as sort fields will be grayed out and un-selectable in the Sort Field dropdown. To configure a sort field, follow these steps.

1. [Create a Field](configuration/data-model/fields/#create-a-field-standard) with an `INTEGER` data type.\
2. Choose a field from the dropdown under **Settings > Data Model > [Collection] > Sort**.
3. Click <span mi btn>check</span> to confirm.
4. Once you sort field is configured, you may want to learn how to [manually sort Items](/app/content/collections/#manually-sort-items).

**Sort Field** — Select a Field to custom sort and order items. Click **Deselect** to disable.

::: tip Automatic Setup

When you [create a Collection](/configuration/data-model/collections/#create-a-collection), you have the option of creating a **Sort** field. If you choose to include this field, the collection's sort settings will automatically be configured for you.

:::

::: tip Interface Sorting

To configure manual sorting within a relational Interface (eg: M2M, O2M, or M2A), configure as above, but also set the **Sort Field** within the relationship section of the field's configuration drawer.

:::

:::tip Configure to Hidden

You may want to set this field to be **Hidden** so it doesn't show up within the Item Details Page.

:::

### Accountability

<video title="Accountability" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

By default, your Directus Project tracks all [activity](/reference/system/activity) and
[revisions](/reference/system/revisions/) for collections. However, you can override this and choose what data is
tracked.

- **Activity & Revision Tracking** — The following options are supported:
  - **Track Activity & Revisions**
  - **Only Track Activity**
  - **Do Not Track Anything**

:::tip Accountability vs. Telemetry

Accountability is a log of _who does what_ in your project. It is for your team's own use. This is different from
[telemetry](/self-hosted/config-options/#telemetry), which is configured under [environment variables](/self-hosted/config-options/#telemetry).

:::

### Duplication

<video title="Duplication" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

The **Save as Copy** option on the Item Details Page offers a way to effectively duplicate the current item. Since there may be unique or relational data
within the item, it's important to control exactly what will be copied. Duplication lets
you configure which parent & relational field values will be copied when you use **Save as Copy** on an item.

- **Item Duplication Fields** — Check the field(s) to copy values for when duplicating an item.

## Delete a Collection

<video title="Delete a Collection" autoplay muted loop controls>
	<source src="" type="video/mp4" />
</video>

To delete a collection, follow these steps.

1. Navigate to **Settings > Data Model > [Collection Name]**.
2. Click <span mi btn dngr>delete</span> in the page header.
3. Confirm this decision by clicking **Delete** in the dialog.

::: danger

This action is permanent and irreversible. Please proceed with caution.

:::
