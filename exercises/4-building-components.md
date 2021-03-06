# Component-based Development with Drupal 8

## 1 - Building Components (continued...)

Now that we have built a couple of pretty simple component in which we introduced essential steps, information and patterns, we can probably take this to a new level.


### 1.3 - Creating more advanced components
During this training we will build a page that lists speakers and their profiles.  One of the advantages of components is being able to reuse them and even alter them without having to rebuild them from scratch.  We will start by building a **Speaker** card component which will consist of a speaker photo, role, name, short bio and social media channels to connect with the speaker.

Once our single component is ready, we will create a page in which we reuse the speaker component to list as many speakers as we need by passing data from Drupal to populate each speaker card.

In this section we will introduce the use of **Twig includes**.  Which is a way to nest one component into another.  Very powerful technique for building advanced components.


#### Let's create the Speaker component now

Inside the `badcamp` directory do one of these two things:

1. If you are using Mediacurrent's theme generator:
   * Run `npx -p yo -p generator-mc-d8-theme -c 'yo mc-d8-theme:component "Speaker"'`

A new component, **speaker**, will be created inside `/src/components/`.  Inside the **speaker** component you will notice the same 3 files we created by hand in previous components (`speaker.json`, `speaker.scss`, and `speaker.twig`).  The generator does its best to add dummy data for the component but we will most likely end up replacing it or modifying it with the real data we need for the component.

2. If you are **NOT** using Mediacurrent's theme generator,  you can create the component by hand just like we did before.

   * Inside `src/components/` create a new directory called **speaker**.

   * Inside the **speaker** directory, create the 3 files listed above (`speaker.json`,`speaker.scss`, and `speaker.twig`).


### 1.3.1 - Creating data source for our component (JSON object)

A JSON Object takes the place of a database when building components.  In it we can find all the data we need to pass to our component in a `key | value` format.

> **_NOTE_**
> If you used the Mediacurrent's theme generator to create your **speaker** component, you should notice **speaker.json** already has content.  I'd say copy the code snippet below and replace the current content in the **speaker.json**.  If you did not use the theme generator to create your **speaker** component your **speaker.json** will be empty.  Copy the code below into it.

**speaker.json**
```json
{
  "name": "AmyJune Hineline",
  "photo": "<img src='../assets/amyjune.jpg' alt='AmyJune Hineline'>",
  "role": "Community Lead",
  "bio": "I am Drupal Sitebuilder and Community Lead for Hook 42. I have been an active participant in the Drupal community for almost 2 years by contributing to projects and helping with documentation.",
  "items": [
    {
      "icon": "twitter",
      "url": "#"
    },
    {
      "icon": "facebook",
      "url": "#"
    },
    {
      "icon": "instagram",
      "url": "#"
    },
    {
      "icon": "youtube",
      "url": "#"
    }
  ],
  "class": ""
}
```
We have created individual variables for each of the fields of the Speaker component.  However, for the social media icons we have created an array which holds the url and name of the social channel.  This array will allow us to add as many or as little social media channels per speaker.


### 1.3.2 - Writing Twig Markup
The next step in the process is to write the markup the **speaker** components needs to be rendered.  In addition, we will pass the data from the JSON object we created in the previous step.

**speaker.twig**
```php
{{ attach_library('badcamp/speaker') }}

<article class="speaker {{ class|default('') }}">

<div class="speaker__header">
  <div class="speaker__photo">
    {{ photo }}
  </div>
  <h2 class="speaker__name">{{ name }}</h2>
</div>

{% if role or bio  %}
  <div class="speaker__content">
    {%
      include '@badcamp/eyebrow/eyebrow.twig' with {
        'eyebrow': role,
        'class': 'speaker__eyebrow',
      } only
    %}
    {% if bio %}
      <p class="speaker__bio">{{ bio }}</p>
    {% endif %}
  </div>
{% endif %}

{% if items %}
  <div class="speaker__social-icons--wrapper">
    {%
      include '@badcamp/social-icons/social-icons.twig' with {
        'items': items,
        'class': 'speaker__social-icons',
      } only
    %}
  </div>
{% endif %}
</article>
```
Using [BEM](https://css-tricks.com/bem-101/) to name our css classes, the speaker component's markup is now in place.  In addition, we are passing the json data using twig syntax.  We opted to use conditionals (`if` statements) for each field to ensure there is data in each field before it is rendered.

We will discuss in detail during training everything inside the twig template to ensure everyone has a full understanding of what's happening.  For now, proceed with the instructions.



### 1.3.3 - Writing CSS Styles
The final step in this process is to write the styles to make our component look and feel as shown in our design comps.  Copy all of the code below into your **speaker.scss**

**speaker.scss**
```css
// Speaker
//
// This is the speaker component.
//
// Markup: speaker.twig
//
// Style guide: Components.Speaker

// Import site utilities.
@import '../../global/utils/init';

.speaker {
  border: 1px solid #003954;
  max-width: 400px;
  position: relative;
}

.speaker__header {
  background: #003954 url('../assets/speaker-bg.png') 0 0 no-repeat;
  line-height: 0.8;
  padding: 20px 0;
  position: relative;
}

.speaker__photo img {
  border-radius: 50%;
  display: block;
  width: 140px;
  height: 140px;
  margin: 0 auto 20px;
  border: 1px solid #003954;
}

.speaker__name {
  color: $color-white;
  font-size: 18px;
  font-weight: 400;
  letter-spacing: 0.15rem;
  line-height: 1;
  text-align: center;
  width: 100%;
}

.speaker__content {
  margin: 20px;
  min-height: 250px;
  overflow: hidden;
  margin-bottom: 30px;
}

.speaker__eyebrow {
  margin-bottom: 20px;
}

.speaker__social-icons--wrapper {
  background: #003954;
  padding: 10px 0;
  height: 45px;
  width: 100%;
  position: absolute;
  bottom: 0;
}

.speaker__social-icons {
  display: flex;
  justify-content: space-between;
  line-height: 1;
  margin: 0 auto;
  width: 200px;

  .social-icons--icon {
    fill: $color-white;
  }
}
```

Another advantage of components is their unique name makes our css styles easy to read and easier to maintain.  If you notice there is no nesting at all in our styles which makes overriding styles very easy if needed.


### 1.4 - Creating the speaker library
Libraries are the recommended way for adding CSS and JavaScript to pages in Drupal 8.  Although our component is not talking to Drupal yet, we will add the "Speaker" library so it's ready when we need to use it in Drupal.  Also, creating the library now feels natural since we are working with the Speaker component.

* Open your **libraries.yml** file located in your theme's root (i.e. `badcamp.libraries.yml`).  If your theme name is not badcamp, your libraries.yml file will include your theme's name.

* Since our theme was created using Mediacurrent's Theme Generator, your libraries.yml probably includes examples of how to declare libraries for Drupal to consume.  Take a moment to review the commented code in the libraries file for ideas on how libraries work or visit this page to learn more about [Drupal 8 Libraries](https://www.drupal.org/docs/8/theming-drupal-8/adding-stylesheets-css-and-javascript-js-to-a-drupal-8-theme).

* Add the following code snippet somewhere in your `badcamp.libraries.yml` file to create the Speaker library

```
speaker:
  css:
    component:
      dist/css/speaker.css: {}
```
 * Since our compiling gulp task compiles all CSS into `dist/css/filename.css`, we are indicating in the Speaker library that any CSS the Speaker component needs will be located in `dist/css/speaker.css`.

 * Should our component need Javascript, we would append to our Speaker library the location of the JavaScript file, like this:

**Example of Speaker library including JavaScript**
```
 speaker:
  css:
    component:
      dist/css/speaker.css: {}
  js:
    js/speaker.js: {}
```

* **speaker**: Name of library used when attaching it to any twig template.  See **speaker.twig** above and you will notice first line includes the following: `{{ attach_library('badcamp/speaker') }}`.  As previously mentioned, the library is only used by Drupal.  The styleguide does not need the library to render CSS or Javascript as these are included globally within KSS Node.

* **css/js**:  This indicates whether we are including CSS or Javascript to our library.

* **component**:  This determines a file organization and aggregation strategy used by Drupal.  Drupall follows a SMACSS-style categorization of its CSS rules.  Read more about the [separation of concern for CSS as well as file organization with SMACSS](https://www.drupal.org/docs/develop/standards/css/css-file-organization-for-drupal-8).

* **dist/css/speaker.css: {}** or **js/speaker.js: {}**:  This represents the path where your CSS or Javascript are located in relation to your theme's root for this particular library.



### 1.5 - Compiling Styleguide
Now that the speaker component is finished we need to compile the styleguie.  Run the command below from within the root of your theme (i.e. badcamp).

```
npm run build
```

#### Preview of the Speaker card component
![Speaker Card Component](assets/speaker.png)



#### View component in styleguide
Let's take a look to make sure our new component looks and behaves as expected

```
http://your-local/themes/custom/badcamp/dist/style-guide/
```

---
Previous exercise:  [Building basic components](3-building-components.md)


Next exercise:  [Preparing Drupal for integration](5-prepare-drupal.md)
