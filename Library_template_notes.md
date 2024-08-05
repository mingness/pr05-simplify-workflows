# Project Context

## Open Call Description

This project aims to improve the developer experience for libraries, tools, and mode contributors. The current system for submitting contributions and updating the contributions list is cumbersome and error-prone. Your work will automate the process and improve the quality of life for other developers, making Processing contributions more inviting to new contributors.


## Open Call Expected Outcomes
- An improved system for submitting new contributions via a PR template.
- A simplified workflow for updating the contributions list in Processing and on the processing.org website.
- Repository templates for libraries using the new Gradle templates.
- Automation for building and exporting artifacts, including PDEX/PDEZ bundles.
- CI/CD deployment of the reference to GitHub pages including install links using the new pdez:// and pdex:// protocols.
- Updates to the library contribution documentation to reflect these changes.
- Optional: a backend system for keeping track of library downloads within Processing.


## Workflow for contributing to libraries

Below is a broad overview of the process of contributing to libraries, tools, and modes, and the existing resources for this. The expected project outcomes are also mapped to the step in the process that it applies to.

| Step | Current workflow | Expected Outcomes | Notes |
|------|------------------|-------------------|-------|
| The idea | | | An invitation on the website? |
|The information | Information on how to develop a library can be found in the Github wiki documents for processing 4, in three separate pages, and then there is a page for modes. <br><br> https://github.com/benfry/processing4/wiki/Library-Basics <br> https://github.com/benfry/processing4/wiki/Library-Guidelines  <br> https://github.com/benfry/processing4/wiki/Library-Overview  <br> https://github.com/benfry/processing4/wiki/Mode-Overview | <ul><li>Updates to the library contribution documentation to reflect changes.</ul> | Update information, with an eye on intuitive organization, and the needs of our broad audience. Potentially also put on website. |
| The development | There are a series of templates that have been developed. The older ones are using Eclipse and Ant, newer ones with Intellij and Gradle. In addition to the executable, the developer is required to create a website that holds documentation for the library. <br><br> https://github.com/processing/processing-library-template <br> https://github.com/processing/processing-library-template-gradle <br> https://github.com/processing/processing-templates <br> https://github.com/processing/processing-tool-template <br> https://github.com/processing/processing-android-library-template | <ul><li>Repository templates for libraries using the new Gradle templates. <li>Automation for building and exporting artifacts, including PDEX/PDEZ bundles. <li>CI/CD deployment of the reference to GitHub pages including install links using the new pdez:// and pdex:// protocols.</ul> | Try to make template straightforward to follow by beginner. Don't shy away from learning opportunities; don't patronize the user. <br><br> PDEX bundles allow for the possibility of installing libraries in other ways other than the contributions manager, for example on the library's documentation pages. |
| The integration - into the Processing environment | The current process is to contact the Processing librarian with a url to your release. <br><br> https://forum.processing.org/one/topic/publishing-contributed-libraries-and-tools | <ul><li>An improved system for submitting new contributions via a PR template. <li>A simplified workflow for updating the contributions list in Processing and on the processing.org website.</ul> | Develop automated pipeline where possible. Reduce workload on Processing librarian. |



