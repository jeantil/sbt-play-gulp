# SBT Play Gulp Plugin
> Gulp Asset Pipeline for Play Framework

SBT Play Gulp Plugin is an SBT plugin which allows you to use Gulp for static assets compilation in Play Framework projects.

## Features

This plugin allows you to:
- Automatically run various user-defined gulp tasks such as JavaScript obfuscation, css concatenation and CDNification on the `compile`, `run`, `stage`, `dist` and `clean` stages.
- Manually run the npm, bower and gulp commands inside the Play sbt console.

## For Whom and Why

This plugin is assumed to be mainly for those who have been familiar with Gulp and would like to utilize Gulp instead of the official web-jar ecosystem for static asset compilation in Play Framework. Play Gulp Plugin is largely a modification of the [play-yeoman plugin](https://github.com/tuplejump/play-yeoman), which uses Grunt rather than Gulp. I created this custom plugin after having found that Gulp configuration is more streamlined and easier to use compared with Grunt.

## How to Use

1. Install npm, bower and gulp if you do not have them.

2. If you do not have any existing Play project, create a plain one like the play-scala template and specify in `<your-project-root>/project/build.properties` the sbt version `sbt.version=0.13.5`, which needs to be 0.13.5 or higher.

3. Add the play gulp plugin to the `<your-project-root>/project/plugins.sbt` file along with the play sbt plugin `addSbtPlugin("com.typesafe.play" % "sbt-plugin" % ${playVersion})` and let the project depend on the play gulp plugin:
  ```
  addSbtPlugin("com.typesafe.play" % "sbt-plugin" % "2.4.2")

  addSbtPlugin("com.mmizutani" % "sbt-play-gulp" % "0.0.1")
  ```

4. Add the following routing settings in the `<your-project-root>/conf/routes` file:
  ```
  GET     /ui         com.mmizutani.sbt.gulp.Yeoman.index

  ->      /ui/        gulp.Routes
  ```

5. Create an `<your-project-root>/ui` folder in the project top directory and inside of it create `package.json`, `bower.json` and `gulpfile.js` files.

6. Edit `package.json`, `bower.json` and `gulpfile.js`, enter the play-sbt console, and install public libraries:
  ```bash
  $ sbt (or activator)
  [your-play-project] $ npm install
  [your-play-project] $ bower install
  ```

7. Create HTML/JavaScript/CSS files in the `<your-project-root>/` diretory.

8. Compile and run the Play project:
  ```bash
  $ sbt
  [your-play-project] $ compilegulp
  Will run: [gulp, --gulpfile=gulpfile.js, --force] in /home/path/to/your/play/project/ui
  ...
  [your-play-project] $ run
  ```
  You will see the compiled app at http://localhost:9000/ui/

Built upon the SBT AutoPlugin architecture, the Play Gulp plugin adds itself automatically to projects that have the sbt-play plugin enabled once you add it in `project/plugins.sbt`. It is not necessary to manually add `enablePlugins(PlayGulpPlugin)` to `build.sbt`. When compilation or testing takes place, then the `PlayGulpPlugin` runs all required tasks on your Play projects, copies the output to the Play assets jar.

To see the plugin in action, you can clone and run this Gulp-enabled [example Play application](https://github.com/mmizutani/sbt-play-gulp/play-gulp-demo).