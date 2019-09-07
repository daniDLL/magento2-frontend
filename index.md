# Magento Themes
----

## Overview
Ver [Magento DevDocs](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/themes/theme-overview.html)

> A theme is a component of Magento application which provides a consistent look and feel (visual design) for entire application area (for example, storefront or Magento admin) using a combination of custom templates, layouts, styles or images.


Un tema es uno de los componentes de Magento encargado de proporcionar el "look & feel" de la aplicación, tanto para su parte de Frontend (la tienda) como el Backend (el admin).

Los temas están diseñados para poder sobreescribir o extender (personalizar) facilmente los distintos recursos de la aplicación, como páginas completas o secciones más concretas.
Estas modificaciones se puede llevar a cabo tanto desde un módulo, como desde librerias, o a traves del propio tema "custom".

En Magento, por defecto tenemos 2 temas disponibles: El tema LUMA como tema demostrativo, y el tema BLANK como tema base de donde partir nuestros temas personalizados. 

#### Magento Luma
----
![Magento Luma Theme Example](https://bssthemes.com/wp/wp-content/uploads/2018/04/Magento_2_Luma_theme_demo_2-1024x710.jpg "Magento Luma Theme Example")

#### Magento Blank
----
![Magento Blank Theme Example](https://bssthemes.com/wp/wp-content/uploads/2018/04/Magento_2_Luma_theme_demo_1-1024x688.jpg "Magento Blank Theme Example")

También existe la posibilidad de poder hacer un tema completamente de cero, pero es poco recomendable ya que pierdes todas las estructuras propias básicas de la aplicación: templates, layouts, estilos básicos. Lo ideal, siempre que se vaya a desarrollar un tema custom, es partir siempre del tema BLANK, nunca del tema LUMA, ya que el tema LUMA tiene fines demostrativos y no están del todo bien optimizadas sus funcionalidades. Otra de las razones por las que no partir del tema LUMA es la dificultad de poder hacer un tema "nieto" (3er nivel/4o nivel) que tiene su complejidad.
Evidentemente queda por supuesto que NUNCA hay que realizar las modificaciones dentro de los distintos temas de Magento dado que en siguientes actualizaciones de la versión de Magento, estás modificaciones quedarian sobreescritas y se perderian, a parte de que es una mala praxis.

### Create a new Storefront Theme

Al crear un nuevo tema no se aplica directamente a la tienda, requiere de un proceso posterior en la parte del admin de aplicar dicho tema al "Store" correspondiente.

#### Steps

1. Create a directory for the theme under "<magento_root>/app/design/frontend/<vendor_name>/<theme_name>"
2. Add a declaration file theme.xml and optionally create etc directory and create a file named view.xml to the theme directory
3. Add a composer.json file.
4. Add registration.php
5. Create directories for CSS, JavaScript, images, and fonts
6. Configure your theme in the Admin panel


##### Declare your theme

1. Add or copy from an existing theme.xml file to your theme directory app/design/frontend/<Vendor>/<theme>.
2. Configure it using the following example

```xml
	 <theme xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/theme.xsd">
	      <title>New theme</title> <!-- your theme's name -->
	      <parent>Magento/blank</parent> <!-- the parent theme, in case your theme inherits from an existing theme -->
	      <media>
	          <preview_image>media/preview.jpg</preview_image> <!-- the path to your theme's preview image -->
	      </media>
	 </theme>
```

##### Make your theme a Composer package

To distribute your theme as a package, add a composer.json file to the theme directory and register the package on a packaging server.

##### Add registration.php

To register your theme in the system, add a registration.php file in your theme directory with the following content.

Con este fichero le estamos informando a Magento de nuestro nuevo componente que estamos creando.

```php
<?php
/**
 * Copyright © Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */

use \Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(ComponentRegistrar::THEME, 'frontend/<Vendor>/<theme>', __DIR__);
```
Como se puede observar le estamos indicando en el Register que nuestro componente es de tipo THEME.

##### Configure images

Product image sizes and other properties used on the storefront are configured in a view.xml configuration file inside "etc" theme folder. It is required for a theme, but is optional if exists in the parent theme.

## Example

Configure all storefront product image sizes in the view.xml file. For example, you can make the category grid view product images square by specifying a size of 250 x 250 pixels:

```xml
...
    <image id="category_page_grid" type="small_image">
        <width>250</width>
        <height>250</height>
    </image>
...
```
##### Create directories for static files

Your theme will likely contain several types of static files:

> Styles
> Fonts
> JavaScript
> Images

Each type should be stored in a separate sub-directory of web in your theme folder:

```sh
app/design/<area>/<Vendor>/<theme>/
├── web/
│ ├── css/
│ │ ├── source/ 
│ ├── fonts/
│ ├── images/
│ ├── js/
```

In the **.../<theme>/web/images** directory, you store the general theme-related static files. For example, a theme logo is stored in **...<theme>/web/images**.

It is likely that your theme will also contain module-specific files, which are stored in the corresponding sub-directories, like **.../<theme>/<Namespace_Module>/web/css** and similar.

##### Your theme directory structure now

At this point your theme file structure looks as follows:

```sh
app/design/frontend/<Vendor>/
├── <theme>/
│   ├── etc/
│   │   ├── view.xml
│   ├── web/
│   │   ├── images
│   │   │   ├── logo.svg
│   ├── registration.php
│   ├── theme.xml
│   ├── composer.json
```


##### Theme logo

In the Magento application, the default format and name of a logo image is logo.svg. When you put a logo.svg image in the conventional location, which is the **<theme_dir>/web/images** directory, it is automatically recognized as the theme logo.

##### Declaring theme logo.

To declare a theme logo, add an extending **<theme_dir>/Magento_Theme/layout/default.xml** layout.

En caso de que se quiera usar un logo con un nombre de fichero distinto, sino con crear un fichero logo.svg automaticamente se reconoce y se aplica al tema "padre".


### Apply and configure a storefront theme

##### Apply a theme

1. In Admin, go to CONTENT > Design > Configuration. A Design Configuration page opens. It contains a grid with the available configuration scopes.
2. In the configuration record corresponding to your store view, click Edit. The page with design configuration for the selected scope opens
3. On the Default Theme tab, in the Applied Theme drop-down, select your newly created theme.
4. Click Save Configuration
5. If caching is enabled, clear the cache.
6. To see your changes applied, reload the storefront pages

##### Add a design exception

Design exceptions enable you to specify an alternative theme for particular user-agents, instead of creating a separate store views for them

1. In Admin, go to CONTENT > Design > Configuration
2. In the configuration record corresponding to your store view, click Edit.
3. On the Design Rule tab, click Add New User Agent Rule.
4. In the Search String box specify the user-agent using either normal strings or regular expressions (PCRE). In the Theme Name drop-down list select the theme to be used for matching agent.
5. Click Save Configuration or Save and Continue.
6. If caching is enabled, clear the cache.
7. To see your changes applied, reload the storefront pages.

##### Add a theme-independent logo

You might want to set a permanent store logo that displays on the storefront no matter what theme is applied

1. In Admin, go to CONTENT > Design > Configuration
2. In the configuration record corresponding to your store view, click Edit.
3. Expand the Header tab.
4. In the Logo Image field browse to the logo file saved in your file system.
5. Upload the file. Allowed file types include .png, .gif, .jpg, and .jpeg. To add an .svg logo, see Declaring theme logo.
6. Optionally, specify the desired width, height, and the alternative text for the logo in the corresponding fields.
7. Click Save Configuration or Save and Continue.
8. If caching is enabled, clear the cache.
9. To see your changes applied, reload the storefront pages.

The logo you add here is stored in the /pub/media/logo/default/ directory.

### Uninstall a storefront theme


