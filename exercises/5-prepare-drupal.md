# Component-based Development with Drupal 8

## 2 - Prepare Drupal for Components Integration
Now that our components are built, we can move on to Drupal to build the infrastructure necessary to integrate our components.

### 2.1 Enable Modules
Enable the following modules and their dependencies (if any):
* Devel and Kint
* Paragraphs
* Components Libraries


### 2.2 Create Paragraph Types (Entities)
We will use the paragraphs module to integrate our components into Drupal by creating custom paragraph types.  However, components can be integrated using other entities types such as custom blocks, custom content types, etc.


* Create a **Speaker** paragraph type
  * Login to your drupal site with administrator previleges
  * Click **Structure | Paragraphs types**
  * Click the **Add Paragraphs type** blue button
  * For label type **Speaker** and click **Save and manage fields**

* Add the following fields and properties to the Speaker paragraph type:

Field label | Field type        | Field machine name     | Allowed # of values
----------- | ----------------- | ---------------------- | -------------------
Photo       | Image             | field_speaker_photo    | Limited 1
Name        | Text (plain)      | field_speaker_name     | Limited 1
Role        | Text (plain)      | field_speaker_role     | Limited 1
Bio         | Text (plain, long)| field_speaker_bio      | Limited 1
Social Icon | Link              | field_speaker_social   | Unlimited



### 2.2.1 Add the Speaker paragraph type as an entity reference field to Basic Page Content type

Paragraphs on their own are useless.  In order to make use of our Speaker paragraph type we need to add it as an entity reference field to a content type or custom block.  For the purpose of this training, we will use the **Basic Page** content type.

1. Click **Structure | Content Types**

2. Click the **Manage Fields** button next to _Basic Page_

3. Click the **Add field** blue button

4. From the _Add a new field_ dropdown, select **Paragraph**

5. In the label field type **Speaker** and click **Save and continue**

6. Leave _Type of item to reference_ as **Paragraph**

7. Change the _Allowed number of values_ to **Limited 1**

8. Click **Save field settings**

9. Under the _Reference Type_ fieldset, check the box next to **Speaker** and click **Save Settings**


### 2.2.2 Create a new node of type Basic Page

Now that we have added the Speaker paragraph type to the Basic Page content type, let's create a new node of type Basic Page and populate all the fields available.

**NOTE**:  For the Social Icons field, type the url to the social channel and for link text type the social network name (i.e. `twitter`, `facebook`, `instagram`, `youtube`, etc.).  The theme offers a macro which allows us to easily add SVG icons to our content.  We'll go over this macro during training.
Add 3 or 4 social channels per speaker.

Our component is rendered however we still need to do a little more work so Drupal knows we have a component available we want to use.


### 2.3 Components Libraries Module

In order for Drupal to be able to use the Speaker component we created, we need the _Components Libraries_ module, which allows us to create a namespace for our theme.  Using a namespace makes it easier to reference twig templates that are not in the default **/templates** directory Drupal typically looks for twig templates in.

The namespace typically matches the name of our theme.  In our case, the namespace will be `@badcamp`.  More on this later.

If you haven't enabled the Components Libraries module please do so.

#### 2.3.1 Adding a namespace to our theme
The namespace is added in the `theme-name.info.yml` file.  Since we created our theme using Mediacurrent's theme generator, the namespace has already been added.  However if you did not use the theme generator, add the following snippet to your `.info.yml` file:

```
# This section is used by the contrib module, Component Libraries. It allows you
# to reference .twig files in your sass/ directory by using the Twig namespace:
# @badcamp
# See https://www.drupal.org/project/components for more information.
component-libraries:
  badcamp:
    paths:
      - src/components
      - src/layout
      - src/templates
```
The code above assigns the namespace of `badcamp` which can be referenced by modules and themes within our project.  In addition, it assignes multiple paths where Drupal can look for twig templates.  By default Drupal 8 only looks for custom twig templates inside the `/templates` directory, but with the namespace in place, we've added `src/components` and `src/layout` as extra directories where twig templates may exist.

All of our components will be stored inside `src/components` making it possible for Drupal to easily access them using the new namespace.


### 2.4 - Enable Twig debugging and disable Drupal's cache

While integrating components we work with Drupal twig templates to manipulate markup and Kint to print variables we need to pass to our component.  Before we start we need to disable CSS and JavaScript aggregation which are on by default in Drupal 8.  We also need to enable twig debugging.  More on this later, for now follow these steps:


#### 2.4.1 Disable Drupal CSS/JS Aggregation

* Click **Configuration | Performance**

* Clear the _Aggregate CSS files_ and _Aggregate JavaScript files_ checkboxes

* Click **Save Configuration**


#### 2.4.2 Disable Drupal Caching and enable twig debugging

Follow the instructions on this article to [disable drupal caching and enable twig debugging](https://www.drupal.org/node/2598914).


### 2.7 - Enable the custom theme (i.e. Badcamp)

Enable the Badcamp theme before proceeding.

* Click **Appearance**

* Click on **Install and set as default** next to the Badcamp theme (or, your own custom theme)


---

Previous exercise:  [Building advanced components](4-building-components.md)


Next exercise:  [Integrate Components](6-integrating-components.md)
