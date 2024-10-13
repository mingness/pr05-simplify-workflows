# Project Diary

### Week 14-15 (September 30 - October 13, 2024)

Feedback on the issue-to-PR workflow from Stef, was to use third party actions,
so as to simplify the workflow. Also, these actions will contain their own
documentation, which simplifies our need to document. There is no action that
does precisely what we want to do, which is make a robust workflow that can be
run multiple times, that creates a new branch with changes, linked to an issue 
started by the contributor, and creates a pull request to merge that branch into
`main`. But each component of this can be run with github cli commands, and
github marketplace actions. To make it robust to repeated runs, the workflow needs
to delete the previous branch, before creating it anew. This has been implemented.

Also, the issue form was missing the contribution type as an input - previously, 
the librarian would have added the new contribution to `sources.conf`, where in
the heading, was the contribution type (possible values are `library`, `examples`,
`tool`, or `mode`). This was added in.

If the processing of retrieving the properties text file and parsing it fails,
a comment will be added to the issue, mentioning the error in human language, except
for the validation step - the full error message will be return, so that the
contributor will know where the error lies.

For this workflow, and for the creation of the database file, there were a number 
of additional data processing steps that were considered, to create an accurate 
database file.

* Some library properties files use `authorList` instead of `authors`, or
`category` instead of `categories`
* The default category for libraries is `Other`, but for other types, there is no
value for `categories`
* Some values for `categories` deviate from the canonical list, or are not accurate,
and the librarian manually gave the library a new category label. These override
the values in the properties file, and are defined in the `override` field in the database.
* If a library was commented out of the `sources.conf`, this library should have
a status of `DEPRECATED`. If a library was listed in `skipped.conf`, this library
should have a status of `BROKEN`. If a library was listed in `broken.conf`, this
library will have in its `override` field, an entry for `maxRevision`, with value `228`.
* The `download` value might need to be constructed from the properties url.

The output files were tested with a locally run Processing website, and the local
Processing. To test the website, the outputted source json files were copied into 
the website repo, and the website run locally. For testing the contribution manager, 
the outputted `contribs.txt` file was put in my processing folder, which on linux, 
was ~/.var/app/org.processing.processingide/config/processing/contribs.txt.
Then, the internet needs to be off, since Processing, upon start up, will hit a 
download url, https://download.processing.org/contribs, rewrite the `contribs.txt` file, 
and then read them into memory. If the internet is not available, it will default
to the local file.

Remaining discrepancies:
These libraries are labeled as broken after the processing, but are not.
37, Patchy - doesn't have prettyVersion
96, Applet Maker & Signer - min/maxRevision are not valid
149, Kinect4WinSDK - required headers user-agent and accept
185, Ptmx - required headers user-agent and accept
236, VLCJVideo - required headers user-agent and accept
169, Python Mode for Processing 3 - needed to use temporary url
274, Python Mode for Processing 4 - needed to use temporary url
also a few example contributions have the `categories` field set as Book; all other
non-library contributions have the `categories` field empty. This was manually
conditioned in the output. The output of these scripts now mimic the originals well.


### Week 12-13 (September 16 - 29, 2024)

I created a script for creating source json files from the yaml database file. A
todo is to test these output files on the the contribution manager, and a local
copy of the website, if possible.

I should also mention, that in this process, a bug in how the properties files
were parsed with the original script was discovered - there was one case where a 
link on a following line was parsed as a key-value pair, because the url had an 
equals sign.

Work was also completed on a workflow that starts with an issue form, and ends
with a pull request being created, for the new contribution. This just needs to
be reviewed by Stef.


### Week 10-11 (September 2 - 15, 2024)

Work on the guide on how to use the library template was completed. 
The template will be announced in the next newsletter, to invite testing and feedback.

Work started on the contributions workflow. The priority is to get
a database file created, and then to build scripts converting the database
file to the contribs.txt and source json files. I am most comfortable using 
Python for scripting, and therefore will do initial work in Python. The goal is
to have the scripts in Kotlin, to have consistency in possible build environments.

I was able to create a yaml database file, by using the data in the contribs.txt
primarily, but extended with the `props` url. Future work on this database file is
to order the contributions by id, such that adding new contributions will be intuitive,
just by seeing what the previous id was. Also, the `id` for the contributions is a 
3 character string representing a number, left-padded with zeros. To better interact 
with the id, this could be stored as a number in the database, but then converted to 
the string representation in the output files (`contribs.txt`, and json files).

I also created a script for creating a contribs.txt file from the yaml database file.
In implementing this, I checked the fields used in contribs.txt, and aside from the expected
`name`, `authors`, `url`, `categories`, `sentence`, `paragraph`, `version`, `prettyVersion`,
`minRevision`, `maxRevision`, `id`, `type`, and `download`, there were also a few instances 
of additional fields: `lastUpdated`, `imports`, `modes`, `minVersion`, and `maxVersion`. 
More research is needed to understand if these additional fields are used.


### Week 8-9 (August 19 - September 1, 2024)

Work continued to finalize the library template.
The `build.gradle.kts` was reviewed in terms of readability, and the comments for beginners was greatly improved.
Additionally, MkDocs was used to more fully document the library.

A couple of issues were investigated, but we decided not to pursue them:
1. special scripts to clean the repository for use as a template. Some repositories have special scripts to remove 
files, such as the readme, when the repository is used as a template. Adding this would create extra burden of 
maintenance, and the repository is quite light as it is.
2. returning the version of the library as a library method, and synchronizing with the value of the version in 
gradle, and in the `library.properties` file. To do this, either the user must maintain the version manually, or it 
could be synchronized via code. This was done previously by inputting this value into the `library.properties` file,
and propagating that value into the code with a search and replace script. Another solution could be to read in
a properties file and return the value; this would require the properties file to be part of the library. Other
ways were explored, to see if the Gradle file could be made aware of the Java method. In the end, it was decided
not to pursue this further, since the need for the version to be reported by the library would only be in cases of
debugging an error, so hopefully very rarely. We are also aware that any file or structure included in the template
might be regarded as required, so we only want to offer what is required in the template.


### Week 6-7 (August 5 - 18, 2024)

We started planning and designing the [adding contribution workflow](Adding_contribution_workflow_notes.md). 
The next steps will be to create a new test repo, with a newly configured database file. This will be the
source of truth, from which the files needed by the contributions manager
and website can be built. The idea is then in the future, these consumers
could also be refactored to interact with the database file directly. Changes to this database file will 
be made via a pull request. To make
this setup friendly to users, we would like to format the database file as
a list of yaml objects (so that it is human readable), and initiate the
pull request with an issue form.

I've also worked on adding support for MkDocs in the library template repository. There is a starter website, 
and a Github workflow that deploys the website as a Github Pages project website. Feedback from Stef and Raphaël 
are to provide a reduced complexity how to, and a separate location for more detailed information, and to take 
out any requirement for installing Python.


### Week 5 (July 29 - August 4, 2024)

Work continued to develop the library template further. Katsuya Endoh, one of the developers of the library 
templates used as a model, has been an active contributor. The template now builds a jar, builds javadocs, 
assembles the release artifact to a zip, creates also a pdex file (zip file with different extension), and 
it has a release workflow. Further work includes integrating mkdocs builds, for easy documentation 
development, and automatic release to Github Pages. There are also a few bits to tidy in the template.

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