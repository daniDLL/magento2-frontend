# Magento Layouts
----

## Table of Contents
1. [Overview](#overview)
1. [Layout instructions](#layout-instructions)
1. [Layout file types](#layout-file-types)
1. [Product layouts](#product-layouts)
1. [Extend a layout](#extend-a-layout)
1. [Override a layout](#override-a-layout)
1. [Common layout customization tasks](#common-layout-customization-tasks)
1. [Customizing layout illustration](#customizing-layout-illustration)
1. [Practice](#practice)


## Overview

In Magento, the basic components of page design are layouts, containers, and blocks. A layout represents the structure of a web page (1). Containers represent the placeholders within that web page structure (2). And blocks represent the UI controls or components within the container placeholders (3). These terms are illustrated and defined below.

![Magento Layouts Example](https://devdocs.magento.com/common/images/layouts_block_containers_defn21.png "Magento Layouts Example")

1. **Layouts** provide the structures for web pages using an XML file that identifies all the containers and blocks composing the page. The details of layout XML files are described later in this section.

1. **Containers** assign content structure to a page using container tags within a layout XML file. A container has no additional content except the content of included elements. Examples of containers include the header, left column, main column, and footer

1. **Blocks** render the UI elements on a page using block tags within a layout XML file. Blocks use templates to generate the HTML to insert into its parent structural block. Examples of blocks include a category list, a mini cart, product tags, and product listing.

> Al final lo que conseguimos con los **Layouts** que nos proporciona Magento es una estructura básica de Ecommerce bastante genérica de la que partir y de ahí personalizar los temas según las especificaciones del Cliente.

### Basic layouts

1. Empty Layout: `<Magento_Theme_module>/view/base/page_layout/empty.xml`

```xml
<?xml version="1.0"?>
<layout xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_layout.xsd">
    <container name="root">
        <container name="after.body.start" as="after.body.start" before="-" label="Page Top"/>
        <container name="page.wrapper" as="page_wrapper" htmlTag="div" htmlClass="page-wrapper">
            <container name="global.notices" as="global_notices" before="-"/>
            <container name="main.content" htmlTag="main" htmlId="maincontent" htmlClass="page-main">
                <container name="columns.top" label="Before Main Columns"/>
                <container name="columns" htmlTag="div" htmlClass="columns">
                    <container name="main" label="Main Content Container" htmlTag="div" htmlClass="column main"/>
                </container>
            </container>
            <container name="page.bottom.container" as="page_bottom_container" label="Before Page Footer Container" after="main.content" htmlTag="div" htmlClass="page-bottom"/>
            <container name="before.body.end" as="before_body_end" after="-" label="Page Bottom"/>
        </container>
    </container>
</layout>
```

2. 1-Column Layout: `<Magento_Theme_module>/view/frontend/page_layout/1column.xml`

```xml
<?xml version="1.0"?>
<layout xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_layout.xsd">
    <update handle="empty"/>
    <referenceContainer name="page.wrapper">
        <container name="header.container" as="header_container" label="Page Header Container" htmlTag="header" htmlClass="page-header" before="main.content"/>
        <container name="page.top" as="page_top" label="After Page Header" after="header.container"/>
        <container name="footer-container" as="footer" before="before.body.end" label="Page Footer Container" htmlTag="footer" htmlClass="page-footer"/>
    </referenceContainer>
</layout>
```

3. 2-Columns-left

4. 2-Columns-right

5. 3-Columns


### Layout handles

A layout handle is a uniquely identified set of layout instructions that serves as a name of a layout file.

There are three kinds of layout handles:

1. page-type layout handles. **Example: catalog_product_view**
1. page layout handles. **Example: catalog_product_view_type_simple_id_128**
1. arbitrary handles. **Example: customer_account.xml**

### Module and theme layout files

The following terms are used to distinguish layouts provided by different application components:

1. **Base** layouts: Layout files provided by modules
    * Page configuration and generic layout files: `<module_dir>/view/frontend/layout`
    * Page layout files: `<module_dir>/view/frontend/page_layout`
1. **Theme** layouts: Layout files provided by themes
    * Page configuration and generic layout files: `<theme_dir>/<Namespace>_<Module>/layout`
    * Page layout files: `<theme_dir>/<Namespace>_<Module>/page_layout`

### Layout files processing

The Magento application processes layout files in the following order:

1. Module base files loaded
1. Module area files loaded
1. Collects all layout files from modules. The order is determined by the modules order in the module list from the app/etc/config.php file. (If their priorities are equal, they are sorted according to their alphabetical priority.)
1. Determines the sequence of inherited themes `[<parent_theme>, ..., <parent1_theme>] <current_theme>`
1. Iterates the sequence of themes from last ancestor to current
1. Merges all layout files from the list

> Layout files that belong to inactive modules or modules with disabled output are ignored.

## Layout instructions

### Manage layouts

To make layout changes available on every page, modify the default.xml file. For example, layout changes added to app/code/Vendor/Module/view/frontend/layout/default.xml are loaded on all pages. To add layout changes to a specific page, use a layout file that corresponds to the page’s path. For example, changes to the app/code/Vendor/Module/view/frontend/layout/catalog_product_view.xml page are loaded on the product details page.

Use these layout instructions to:

1. Move a page element to another parent element
1. Add content
1. Remove a page element
1. Arrange the element position

### Common layout instructions

* `<block>`
* `<container>`
* `before and after attributes`
* `<action>`
* `<referenceBlock> and <referenceContainer>`
* `<move>`
* `<remove>`
* `<update>`
* `<argument>`
* `<block> vs <container>`

Ver [in depth Common layout instructions Explanation](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/layouts/xml-instructions.html#fedg_layout_xml-instruc_ex)

## Layout file types

1. **Page layout**: empty, 1-column, 2-columns-left, 2-columns-right, 3-columns
1. **Page configuration**: catalog_product_view.xml
1. **Generic Layout**: Specific content. Example: Infinite Scroll


Ver [Magento DevDoc Layout file types](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/layouts/layout-types.html)

## Product layouts

### Product view page

Ver [Magento DevDoc Product layouts](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/layouts/product-layouts.html)

## Extend a layout

### Create a theme extending file

To add an extending page configuration or generic layout file:

```xml
<theme_dir>
 |__/<Namespace>_<Module>
   |__/layout
     |--<layout1>.xml
     |--<layout2>.xml
```

> For example, to customize the layout defined in `<Magento_Catalog_module_dir>/view/frontend/layout/catalog_product_view.xml`, you need to add a layout file with the same name in your custom theme, such as: `<theme_dir>/Magento_Catalog/layout/catalog_product_view.xml`



To add an extending page layout file:

```xml
<theme_dir>
 |__/<Namespace>_<Module>
   |__/page_layout
     |--<layout1>.xml
     |--<layout2>.xml
```

> Al final Magento procesa todos los XMLs de los layouts y los mergea en un XML "gigante", por lo que nuestras personalizaciones a los layouts del core se añaden y agrupan.

## Override a layout

> Not all layout customizations can be performed by extending layouts


## Practice

> Create a custom layout page (FrontName_ControllerName_ActionName.xml) that show a list of teachers with their students as follow:

```
-> Teacher1
    -> Student1
    -> Student2
    -> ---
-> Teacher2
    -> StudentN
    -> ---
-> TeacherN
```