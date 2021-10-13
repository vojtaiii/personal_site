---
title: "Create an executable from TornadoFX (JavaFX) project in IntelliJ IDEA"
date: 2021-08-20T21:00:00-04:00
categories:
  - tools
tags:
  - tornadofx
  - javafx
---

[TornadoFX](tornadofxurl) is a tool for creating standalone desktop applications based on the JavaFX framework. It uses Kotlin language
and utilizes powerful builders for various UI components, viewmodels for handling logic and easy CSS style modifications. 

So when you choose this platform you inevitably come to the point when the application needs to be turned into an executable file for 
user distribution. Here is described the detailed process, using JetBrains IntelliJ IDEA IDE.

### Create an executable JAR
The normal process would cover creating an *artifact* for the JAR file in the IDE and then selecting `Build -> Build artifacts`. This, however, ultimately failed in my case with JAR file being somehow corrupted. The solution which finally worked involves a so-called *fat JAR*.

In your `build.gradle.kts` include the following block introducing the fat JAR:

```kotlin
val fatJar = task("fatJar", type = Jar::class) {
    baseName = "${project.name}-fat"
    manifest {
        attributes["Implementation-Title"] = "Gradle Jar File Example"
        attributes["Implementation-Version"] = version
        attributes["Main-Class"] = <path to your your main class>
    }
    from(configurations.runtimeClasspath.get().map { if (it.isDirectory) it else zipTree(it) })
    with(tasks.jar.get() as CopySpec)
}

tasks {
    "build" {
        dependsOn(fatJar)
    }
}
```

To build a fat JAR, select the Gradle panel from the right side of the IDE. Go to `Tasks -> other` and execute the `fatJAR` task. 

Upon a successful build, the final executable JAR file should be located in project folder under `build/libs/` directory. 

### Create .exe from JAR file
Suppose now we are on Windows and the final form is the standard .exe file. Luckily, there are many tools which accomplish this, one of them being [Launch4J](launch4jurl). 

![alt text][launch4jpic]

As you can see there are several options which we can modify. Mandatory is only the ouput file, the source executable .jar file and name of the main class (on the *Classpath* panel). But you can also specify the icon of the solftware, command line arguments and even custom message displayed when the given device in not equipped with the sufficient Java kit. 

Upon specifying necessary fields, save the configuration and build it. After the process is complete, the .exe file should be ready to execute. 


[tornadofxurl]: https://edvin.gitbooks.io/tornadofx-guide/content/
[launch4jurl]: http://launch4j.sourceforge.net/
[launch4jpic]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/launch4j.JPG?raw=true