# Adding contribution workflow notes

These notes concern the workflow of adding a new library, or updating library information. Once a new library is developed, and release artifacts created and available on a website, this library needs to be added to the contributions manager, and the processing website. The contributions manager looks at `pde/contribs.txt` in the `processing-contributions` repo. The website looks at the `content/contributions` folder in its repo, `processing-website`.

The `contribs.txt` file has information for each library sequentially in the file, in key=value pairs, with an empty line in between, except for the first line, which is the type, without the key. For example,

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

The `content/contributions` folder in the website repo has json files, one for each library. These files are identical to the json files in the `sources` folder in the 
`processing-contributions` repo, except that the `id` key-value pair, which exists in the `processing-contributions` json files, are removed in the `processing-website` files. 

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

As you might notice, the `contribs.txt` file has version information, while the json files in the `content/contributions` folder do not.

## Current process

1. The library developer will email the Processing librarian, with the url of the release artifacts.
2. The librarian will feedback on any recommendations for changes to the library developer.
3. The librarian adds the library manually to `sources.conf`. This file lists the categories, under which, in each row, is an id number and the url to the properties file (*.txt) file for the applicable contribution.
3. The librarian then runs some scripts to add the library into necessary locations.
    1. https://github.com/processing/processing-contributions/blob/master/scripts/build_contribs.py is run as github action, daily. This generates `pde/contribs.txt` file - used by contributions manager. This script visits the url to the properties file (*.txt), and pulls the values from this file, and updates them in the `pde/contribs.txt` file. If any of the ids are in the  `skipped.conf` file, the library is skipped. If any of the ids are in the `broken.conf` file, the `maxRevision` is pinned to 228, which is an older version. These two *.conf files are also manually updated.
    2. run `update-contributions.js` script in `processing-website` repo. this copies files from the `sources` folder in the `processing-contributions` repo, into the `content/contributions` folder in the `processing-website` repo, while removing the `id` key-value pair. The `sources` folder contains json files, one for each contribution. 


## Design goals
1. Automate where possible
2. Make data transparent and intuitive
3. Make non-breaking changes for the website and contributions manager. 


## Possible changes
1. The library developer creates a PR in the `processing-contributions` repo. The change is to add a line to a new database file.
2. There is a new database file in `processing-contributions`, that lists the information in `sources.conf`, `broken.conf`, and `skipped.conf`, in tabular form. This will put all the data in one place, and make hopefully more transparent to humans. The columns could be, `id`, `name`, `version`, `prettyVersion`, `minRevision`, `maxRevision`, `is_broken`, `is_missing`, `authors`, `url`, `download`, `props`, `type`, `date_added`, `last_updated`, `categories`, `sentence`, `paragraph`. The database could include a line for every version of a library - it allows for properties to change for a given version.
3. A script runs regularly to check if the properties file (*.txt) is changed for a library. If so, a new PR is created to edit the line in the database file.


## Questions
1. Does the contributions manager need the `id` of the library, from the `processing-contributions` repo? If not, then the `id` can be kept internal to the database, and doesn't need to be explicit.
2. Is there any information that the contribution manager not use from `contribs.txt`?
3. Can the website accept the version information? If so, then the `contribs.txt` file can be synchronized with the json files, allowing easy migration to a new source of information in the future. Eventually, the website might want to show the version information. 
4. How often does the `update-contributions.js` script get run for the website?
5. What is the P5 Robot, that changes some of the *.json files in the `sources` folder, in the contributions repo?