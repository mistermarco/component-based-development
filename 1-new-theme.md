# Component-based Development with Drupal 8

## New Drupal 8 Theme
Creating a new Drupal theme or updating an existing one is one of the first things we need to do to begin building components.
In this training we will assume you are using [Mediacurrent's Theme Generator](https://github.com/mediacurrent/theme_generator_8) to create a new theme.  Follow the instructions on the theme generator project to get your theme setup.

## Using included theme
Alternatively, you can use the `component_training` theme which is included with this project.
* Copy the `component_training` folder into `docroot/themes/custom/`
* Follow instructions below

### Compiling the theme for first time
After you create your theme, navigato to your new theme's folder and execute the following commands to compile Sass, Javascript and Styleguide.

```
npm install
```

```
nvm install
```

```
nvm use
```

```
npm run build
```

### View Styleguide.

```
http://d8.local/themes/custom/component_training/dist/style-guide/index.html
```