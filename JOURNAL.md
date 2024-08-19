# Project Diary

### Week 6-7 (August 5 - 18, 2024)

We started planning and designing the [adding contribution workflow](Adding_contribution_workflow_notes.md). The next steps will be to create a new test repo, with a newly configured database file. This will be the
source of truth, from which the files needed by the contributions manager
and website can be built. The idea is then in the future, these consumers
could also be refactored to interact with the database file directly. Changes to this database file will be made via a pull request. To make
this setup friendly to users, we would like to format the database file as
a list of yaml objects (so that it is human readable), and initiate the
pull request with an issue form.

I've also worked on adding support for MkDocs in the library template repository. There is a starter website, and a Github workflow that deploys the website as a Github Pages project website. Feedback from Stef and Raphaël are to provide a reduced complexity how to, and a separate location for more detailed information, and to take out any requirement for installing Python.


### Week 5 (July 29 - August 4, 2024)

Work continued to develop the library template further. Katsuya Endoh, one of the developers of the library templates used as a model, has been an active contributor. The template now builds a jar, builds javadocs, assembles the release artifact to a zip, creates also a pdex file (zip file with different extension), and it has a release workflow. Further work includes integrating mkdocs builds, for easy documentation development, and automatic release to Github Pages. There are also a few bits to tidy in the template.

### Week 4 (July 22 - 28, 2024)

After a few weeks swimming in Gradle, this repo has emerged. The approach I'm 
taking with it, is to start from an empty Gradle project, and add to it functionality
learned from the two model repos. Key feature is I want it buildable as is. It
currently will build a jar, and it will release a zip. 

Some todos (Github issues are already created) before I release it for user testing are:
- Include a Processing library dependency. This will likely require the user to 
input where their sketchbook folder is. The gradle release tasks will need to be
augmented to also include Processing library dependencies in the release bundle.
- Once the sketchbook location is known, the library can be installed into Processing
- Check the javadoc output is as expected

One additional difference between the two model template repositories, is enkatsu's
creates a shadow jar, including all dependencies inside the jar. The processing
template includes all dependencies in the release zip file as jars. Both do the job,
but including dependency jars in the release zip makes it more transparent what 
the dependencies are, so we implement that here.


### Weeks 1 - 3 (July 1 - 21, 2024)

Having discussed with Stef, my mentor, about the general approach to the entire 
project, I will tackle the library template first, then the workflow for adding 
library contributions, while conducting stakeholder interviews. After that, I will
take a look at the documentation, and hopefully have time to look at the process 
for tools and modes as well.

I value intuitiveness in a template, and to achieve this, I have interviewed 
members of the community who have developed libraries recently. Their responses 
have guided me on what has worked well, and what leaves room for confusion. I will 
provide more information on findings from these interviews at a later point.

There are two existing templates for developing libraries for Processing on 
Github. These first few weeks, I have worked with these two templates, to better 
understand them. I was able to use both to build a small library, without hiccups. Some specific 
differences between the two templates are listed in [Library template notes](Library_template_notes.md)