# Markdroid

A simple Gradle plug-in that makes it easy to convert Markdown documents into native Android layout XML files.

Useful for apps that require rich content and an easy way to maintain.

| ![Screenshot 1](https://raw.github.com/mindsnacks/markdroid/master/readme_assets/markdroid_1.png)         | ![Screenshot 2](https://raw.github.com/mindsnacks/markdroid/master/readme_assets/markdroid_2.png)           |
| ------------- | ------------- |

## Build Process Integration
Add Markdroid to your buildscript depdencies and apply the plug-in:

```groovy
buildscript {
  repositories {
    mavenCentral()
  }
  
  dependencies {
    classpath 'com.android.tools.build:gradle:0.7+'
    classpath 'com.mindsnacks:markdroid:CURRENT-VERSION'
  }
}

apply plugin: 'markdroid'
```

Then create a task to convert your Markdown files:

```groovy
import com.mindsnacks.markdroid.gradle.ConvertMarkdownDirectoryToAndroidResourcesDirectoryTask
import com.android.build.gradle.tasks.ProcessAndroidResources

task generateMarkdownResources(type: ConvertMarkdownDirectoryToAndroidResourcesDirectoryTask) {
  def inputTree = file('path/to/directory/containing_only_your_markdown/and_images')
  inputs.dir inputTree
  outputs.dir file("markdroid_gen/res")
}

tasks.withType(ProcessAndroidResources) { processResourcesTask ->
  processResourcesTask.dependsOn generateMarkdownResources
}
```

Finally, just add your generated XML directory to your source set:

```groovy
android {
  sourceSets {
    main {
      res.srcDir 'markdroid_gen/res'
    }
  }
}
```

## App Integration

### Loading Layouts

Since there is not one super common way of using the generated XML, it's up to you to inflate the layouts and integrate them into your app. Markdroid provides a very small helper library to make this easy, just add it as an app dependency:

```groovy
compile 'com.mindsnacks:markdroid-helper:0.8.5-SNAPSHOT@aar'
```

This library provides a few methods for easily loading layout files and enabling rich text. To see an example, check out [MainActivity.java in the example project](https://github.com/mindsnacks/markdroid/blob/master/markdroid-example/src/main/java/com/markdroid/example/MainActivity.java).

### Styling Layouts

TODO: Finish this section.

## Markdown Support
Markdown support is still somewhat limited. The following features work:

1. Text formatting (bold and italics)
2. Headings
3. Blockquotes
4. Unordered and ordered lists
5. Images (only local images, can't be inline with text)
6. Links

