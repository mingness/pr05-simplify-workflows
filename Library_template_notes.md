# Library Template

The library template is a Github repo that is a template for creating a new Processing library.
It provides Gradle tasks for easy building and assembling of release artifacts.

## Design principles
1. Make it easy to use for all users, from beginner to advanced skillsets. This means, keep it simple, and explain clearly, don't assume preexisting knowledge.

2. Adopt current standards, such as Kotlin as default Gradle language

3. Automate

## Existing Templates
There were two existing templates for developing libraries for Processing on 
Github that were used as initial models:
- https://github.com/processing/processing-library-template-gradle
- https://github.com/enkatsu/processing-library-template-gradle

Also notable are these two repositories:
- https://github.com/hamoid/processing-library-template - used enkatsu's template as a model. used jitpack to resolve Processing
- https://github.com/jjeongin/creative-machine - library by Jeongin, on of the authors of the processing gradle template

Some specific differences between the two model templates:

- https://github.com/processing/processing-library-template-gradle
  - The developer interaction for configuring the build is to fill out the 
    /resources/build.properties file with all build and release parameters
  - It resolves Processing by pointing to the locally installed jar files
  - The build.gradle file has 141 lines; processing-library.gradle 159 lines
  - It provides gradle tasks for releasing the library
  - This template provides fully documented example code, and an example
- https://github.com/enkatsu/processing-library-template-gradle
  - The developer interaction for configuring the build is to add dependencies
    to the build.gradle
  - It resolves Processing from Maven, using an unofficial version, 3.3.7
  - The build.gradle file has 56 lines
  - It does not provide scripts for releasing the library
  - This template provides example code and example, but it is not commented 
    extensively

Here are some thoughts, related to each bullet point above:

- Which way of interacting with a template will be most intuitive, adding 
parameters to a text file that is then parsed, or editing directly into files? 
There is no clear winner for me at the moment. Best to let the users tell us - do 
user testing. Leave both options available for the duration of the grant, and 
solicit interviews with library developers.

- Stef suggested that the best way to resolve Processing at this time is to use
jitpack to compile a Github repository. There is an unofficial repository for
resolving Processing4 with jitpack at https://github.com/micycle1/processing-core-4/

- The build.gradle + processing-library.gradle files of the processing template 
are large, and aren't easy to digest on first look. That's because they are not 
designed to be edited. Is this a missed opportunity for gaining familiarity with 
gradle? enkatsu's template is simple, invites and requires editing to configure
to your library. As mentioned in the first comment, I'm not sure which user 
interaction is most intuitive, but will develop one with a simpler gradle file for
direct editing.

- It is desired to provide scripts for releasing the library. Will adopt the gradle 
tasks for releasing from processing template.

- It is desired for the repository be clear and intuitive to even a beginner at 
developing libraries. Ideally, the template should run and build and release, 
without input from the user. I want to extend the example build to require 
dependencies, to help show how to add them.

Additionally, Stef suggested that we should move to Kotlin, as it is the current
default; the gradle files in both repositories described above use Groovy.
It's clear that both templates provide great value, and there is a benefit to
combining the two. 


----

## Future-facing questions

1. What is the `paragraph` used for? I could not find it used anywhere.
I noticed the Sound library from Processing doesn't define a `paragraph` in 
its `library.properties` file. Are there plans to use this field in the future?
2. Is there an easier way of dealing with max and min revisions? Right now the 
default values in the `library.properties` file are `0`. This is replaced with `228` if it is a "broken" library, which is a manual designation. Is there
a way to programmatically determine the revision range?
