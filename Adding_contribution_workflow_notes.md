# Adding contribution workflow notes

These notes concern the workflow of adding a new library, or updating library information. 
Once a new library is developed, and release artifacts created and available on a website, 
this library needs to be added to the contributions manager, and the processing website. 
The contributions manager looks at `pde/contribs.txt` in the `processing-contributions` repo. 
The website looks at the `content/contributions` folder in its repo, `processing-website`.

The `contribs.txt` file has information for each library sequentially in the file, in 
key=value pairs, with an empty line in between, except for the first line, which is the type, 
without the key. For example,

```
library
name=Sound
categories=Sound
authors=The Processing Foundation
url=https://processing.org/reference/libraries/sound/
sentence=Provides a simple way to work with audio.
paragraph=
version=18
prettyVersion=2.4.0
lastUpdated=0
minRevision=229
maxRevision=0
id=153
type=library
download=https://github.com/processing/processing-sound/releases/download/latest/sound.zip

```

The `content/contributions` folder in the website repo has json files, one for each library. 
These files are identical to the json files in the `sources` folder in the 
`processing-contributions` repo, except that the `id` key-value pair, which exists in the 
`processing-contributions` json files, are removed in the `processing-website` files. 

Example sources/.*json file:
```
{
  "name": "Sound",
  "categories": [
    "Sound"
  ],
  "authors": [
    "The Processing Foundation"
  ],
  "url": "https://processing.org/reference/libraries/sound/",
  "sentence": "Provides a simple way to work with audio.",
  "paragraph": "",
  "lastUpdated": "0",
  "id": "153",
  "type": "library",
  "packages": [
    {
      "mode": "java",
      "minRevision": "229",
      "maxRevision": "0",
      "props": "https://github.com/processing/processing-sound/releases/download/latest/sound.txt",
      "download": "https://github.com/processing/processing-sound/releases/download/latest/sound.zip"
    }
  ]
}
```

As you might notice, the `contribs.txt` file has version information, while the json files 
in the `content/contributions` folder do not.

## Current process

1. The library developer will email the Processing librarian, with the url of the release artifacts.
2. The librarian will feedback on any recommendations for changes to the library developer.
3. The librarian adds the library manually to `sources.conf`. This file lists the categories, under 
which, in each row, is an id number and the url to the properties file (*.txt) file for the applicable contribution.
4. The librarian then runs some scripts to add the library into necessary locations.
    1. https://github.com/processing/processing-contributions/blob/master/scripts/build_contribs.py 
    is run as github action, daily. This generates `pde/contribs.txt` file - used by contributions 
    manager. This script visits the url to the properties file (*.txt), and pulls the values from 
    this file, and updates them in the `pde/contribs.txt` file. If any of the ids are in the  
    `skipped.conf` file, the library is skipped. If any of the ids are in the `broken.conf` file, 
    the `maxRevision` is pinned to 228, which is an older version. These two *.conf files are also 
    manually updated.
    2. run `update-contributions.js` script in `processing-website` repo. this copies files from 
    the `sources` folder in the `processing-contributions` repo, into the `content/contributions` 
    folder in the `processing-website` repo, while removing the `id` key-value pair. The `sources` 
    folder contains json files, one for each contribution. 


## Design goals
1. Automate where possible
2. Make data transparent and intuitive
3. Make non-breaking changes for the website and contributions manager. 


## Possible changes
1. The library developer creates a PR in the `processing-contributions` repo. The change is to add 
a line to a new database file.
2. There is a new database file in `processing-contributions`, that lists the information in 
`sources.conf`, `broken.conf`, and `skipped.conf`. This will put all the data in one place, and make 
hopefully more transparent to humans. The fields could be, all the fields in `library.properties`.
Also, `is_broken`, `is_missing`, `download`, `props`, `type`, `date_added`, `last_updated`. 
For best readability, the data 
will be a list of yaml objects. Tabular data quickly leaves the header far away from the bottom of 
the table, and a separate line for each item makes it clear what has changed, when a record is edited. 
The database could include a line for every version of a library - it allows for properties to change 
for a given version.
3. A script runs regularly to check if the properties file (*.txt) is changed for a library. If so, 
the change is added automatically to the database file.


## Questions
1. Does the contributions manager need the `id` of the library, from the `processing-contributions` repo? 
If not, then the `id` can be kept internal to the database, and doesn't need to be explicit.
   - Contributions are parsed in https://github.com/benfry/processing4/blob/6a2cf8cda35552c62a1a794bb1e20f43fe8ffcda/app/src/processing/app/contrib/AvailableContribution.java#L39. The `id` is not retrieved.
   - Downloaded contributions properties file is read in, and the `id` from that is read in. But it's not clear if it's used. 
   https://github.com/benfry/processing4/blob/6a2cf8cda35552c62a1a794bb1e20f43fe8ffcda/app/src/processing/app/contrib/LocalContribution.java#L67
2. Is there any information that the contribution manager not use from `contribs.txt`?
   - in addition to the `id`, the `categories` is not parsed for a given contribution in the contributions 
   manager. The categories is processed before it is added to the `contribs.txt` file. 
3. Can the website accept the version information? If so, then the `contribs.txt` file can be synchronized 
with the json files, allowing easy migration to a new source of information in the future. Eventually, 
the website might want to show the version information. 
   - should not be a problem to add more key-value pairs.
4. How often does the `update-contributions.js` script get run for the website?
   - seems last change to the `content/contributions` folder in the `processing-website` repo was a year ago.
5. What is the P5 Robot, that changes some of the *.json files in the `sources` folder, in the contributions repo?
   - from Stef, this is a Github action that runs build_contribs.py, and check-changes.sh. https://github.com/processing/processing-contributions/actions/runs/10363261300/workflow
6. Is there a way to input a new line, and check the data in that line, using a script, or gui?
   - The suggested way now is to have the new contributor create a new issue, filling out the issue 
   form, which will then automatically create the pull request.
7. Do we want to store version information, and dates updated?
   - This information might be interesting to users, but we won't need to store version specific 
   information - the library only needs to be listed once, with most current version, plus, a list of previous versions.


## Next steps

1. Create a new repo for testing the ideas listed here, called `processing-contributions-new`. 
The `processing-contributions` repo does not allow forking.
2. Create new database file, called `contributions.yaml`. The objective is to keep the output 
files the same as they are currently. In this first implementation, the source of information 
will be from the `contribs.txt` and `sources/*.json` files. This means the filtering of deprecated 
or skipped contributions has already happened, and these contributions will be missing from the 
database file. This first version will just have fields existing already in the output files.
3. Create scripts to create `pde/contribs.txt`, and `content/contributions/*.json` files. Kotlin + 
gradle is the goal, but I'm using Python for now, for ease of use.
4. Create issue form for new library contributors to fill, which then automatically creates the pull 
request that changes the database file.
5. Test output files with website and contribution manager.
6. Expand database file to include fields related to filtering, like `is_deprecated`, `is_skipped`,
`is_broken`. These fields will replicate the information in the `source.conf`, `skipped.conf`, and
`broken.conf`. The database will then have more contributions listed. Modify scripts to do the filtering.
7. Rewrite scripts in Kotlin. Process data such that required fields are validated, but we also allow
other fields.
8. Write script for checking online property txt files for each contribution. If there is an update,
edit the database file, and recreate contribs.txt and json files.


## Design of database file
* The file will be named `contributions.yaml`
* By default, all information about a contribution will be included. This includes all the fields in 
the json files, plus `version` and `prettyVersion`; alternatively described, it is all the fields in 
the `contribs.txt` plus the `props` url. 
* The fields from the `library.properties` file are: `id`, `name`, `version`, `prettyVersion`, 
`minRevision`, `maxRevision`, `authors`, `url`, `download`, `props`, `type`, `categories`, `sentence`, `paragraph`. 
* The `id` in all existing files is a 3-character string representation of a number, that is 
left-padded with zeros. Store the `id` in the database as a number, but convert to a string when output.
* Other fields we'd like to include are [WIP]
   * `is_deprecated` - These are libraries that are commented out of `source.conf`
   * `is_skipped` - These are libraries listed in `skipped.conf`
   * `is_broken` - These are libraries listed in `broken.conf`
   * `error` - Explains why a library is deprecated, or skipped
   * `previous_versions` - a list of previous `prettyVersion` values 
   * `date_added` - Date library was added to contributions. This is a future facing field
   * `last_updated` - Date library was last updated in the repo. This is a future facing field


## Future-facing questions and tasks

2. Understand the rules for inclusion of a library id in the `skipped.conf` and `broken.conf` and compare 
with current state of props url.
