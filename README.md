# Game Closure DevKit Documentation

## Repo Structure

This repository is separated in to the following directories:

* `api` ---Markdown files and images used to generate the API Reference, ordered by namespace.
* `guide` ---Markdown files and images used to generate user guides and tutorials.
* `native` ---Markdown files and images used to generate native guides.
* `html` ---The static html and styles for the doc website. Generated files are also placed in here.
* `build` ---Build script and template used to generate the documentation.

## Editing

All documentation is written in markdown and converted to html
using Pandoc. Subsequently, any of Pandoc's [supported syntax](http://johnmacfarlane.net/pandoc/README.html) 
will work in these documents.

Trivial edits may be done directly through the
GitHub interface: just select the file and click the
*Edit* button in the upper-right corner, make your
changes and submit a commit message. This will be
applied directly to the branch you are browsing (probably
**master**), so care should be taken.

For non-trivial edits, please create a separate branch
and submit a pull request---the same as other software projects.

### Including Screenshots and Assets

Images, diagrams, screenshots, and other assets should be
placed in the `assets` directory located within the guide's
respective root directory: `api`, `guide`, or `native`.

Reference the image in the guide using a relative path from
the root directory, for example:

~~~
<img src="./assets/guide-dir/screenshot.png">
~~~

### Update the Navigation Bar

If adding a new link to the nav bar displayed at the top of
each page, you must edit two files:

* `./html/index.html`
* `./build/template.html`

This is required because the site is statically generated
and the relative paths in `index.html` differ from the rest
of the site.

## Build a Local Copy

To build a local copy of the documentation, install
[Pandoc](http://johnmacfarlane.net/pandoc/) to convert the
markdown to html. With that, simply run the shell script:

~~~
$ ./build/make-doc
~~~

This will output the generated documentation in the `html` directory.

## License

This documentation is licensed under the [Creative Commons Attribution-ShareAlike](http://creativecommons.org/licenses/by-nc-sa/3.0/) license.
