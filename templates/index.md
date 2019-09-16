[Volver al menú](/magento2-frontend)

# Magento Templates
----

## Table of Contents
1. [Overview](#overview)
1. [Templates customization walkthrough](#templates-customization-walkthrough)
1. [Templates basic concepts](#templates-basic-concepts)
1. [Illustration of customizing templates](#illustration-of-customizing-templates)
1. [Customize email templates](#customize-email-templates)
1. [Practice (Layouts y Templates)](#practice-layouts-y-templates)


## Overview

In Magento application templates are the part of the view layer. Templates define exactly how the content of layout blocks is presented on a page: order, CSS classes, elements grouping, and so on. In most cases, templates do not contain any logic about whether they will or will not be rendered, this is typically handled by the layout files. Once a template is called in a layout, it will be displayed.

> Default Magento templates are PHTML files.

## Templates customization walkthrough

To customize a template:

1. Locate the template which is associated with the page/block you want to change using [template hints](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/themes/debug-theme.html)
1. Copy the template to your theme folder according to the [template storing convention](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/templates/template-override.html#template-convention)
1. Make the required changes

To add a new template in a theme:

1. Add a template in your theme directory according to the template storing convention.
1. Assign your template to a block in the corresponding layout file.

Ver [Ejemplo](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/templates/template-walkthrough.html)

## Templates basic concepts

Templates are initiated in layout files, and each layout block has an associated template.

The template is specified in the template attribute of the <block> layout instruction.

```xml
<block class="Magento\Catalog\Block\Category\View" name="category.image" template="Magento_Catalog::category/image.phtml">
```

The category.image block is rendered by the image.phtml template in the category subdirectory of the Magento_Catalog module templates directory.

The templates directory of Magento_Catalog is app/code/Magento/Catalog/view/frontend/templates.

### Template location

Templates are stored in the following locations:

1. Module templates: `<module_dir>/view/frontend/templates/<path_to_templates>`
1. Theme templates: `<theme_dir>/<Namespace>_<Module>/templates/<path_to_templates>`

### Template overrides

For template files with the same name, the following override rules apply:

1. Theme templates override module templates
1. Child theme templates override parent theme templates

To change the output defined by an existing template, override the template in your custom theme. This concept is the basis of template customization in Magento.

### Root template

`<Magento_Theme_module_dir>/view/base/templates/root.phtml` is the root template for all storefront pages in the Magento application. This file can be overridden in a theme just like any other template file.

Unlike other templates, `root.phtml` contains the doctype specification and contributes to `<head>` and `<body>` sections of all pages rendered by Magento application.

## Illustration of customizing templates

Ver [Ejemplo](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/templates/template-sample.html)

## Customize email templates

Ver [DevDoc](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/templates/template-email.html)

## Practice (Layouts y Templates)

> Create a Custom Page that show a list of teachers with their students as follow: 

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

### Steps
> Create a custom layout page (FrontName_ControllerName_ActionName.xml) :

> Create a Controller that use the created layout page.

> Create a block that retrieve and build the information to display.

> Create a template to print the information aquired by the block