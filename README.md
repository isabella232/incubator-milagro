![Milagro Logo](/images/MILAGRO_LOGO.png)

The Apache Milagro (incubating) website
============================

Milagro has several repos. The application repos are being overhauled to reflect new, modern architectures. Expect for these to be merged into one repo called Milagro Server. More detail is available at:

* [Milagro JIRA](https://issues.apache.org/jira/projects/MILAGRO/issues/MILAGRO-18?filter=allopenissues)
* [Milagro Wiki](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=115529045)

The most current libraries are the core crypto libraries:

* [Milagro Crypto](https://github.com/apache/incubator-milagro-crypto)
* [Milagro Crypto C Library](https://github.com/apache/incubator-milagro-crypto-c)
* [Milagro Crypto JavaScript Library](https://github.com/apache/incubator-milagro-javascript)
* [Milagro Crypto Java Library](https://github.com/apache/incubator-milagro-java)

Website Detail
============================

This repository contains the source for the website of [Apache Milagro (incubating)](http://milagro.apache.org/). Milagro contains a set of crypto libraries and core security infrastructure for decentralized networks.

The Milagro website itself is completely static, being automatically generated by [Jekyll](https://jekyllrb.com/) prior to deployment. The content of the website is written in a mixture of HTML and Markdown, with dynamic portions written using [liquid templating](https://jekyllrb.com/docs/templates/). Templated content is interpreted only a build time, with the final result being completely static.

To facilitate ease of development and testing, this repository also contains a build script, `build.sh`, the usage of which is documented below.

Table of contents
-----------------

* [Repository structure](#repository-structure)
* [Build prerequisites](#build-prerequisites)
* [Testing changes locally](#testing-changes-locally)
* [Publishing changes](#publishing-changes)

Repository structure
--------------------

In addition to the `LICENSE` and `NOTICE` files required of any proper Apache-licensed project, the repository contains the following critical files:

| Filename      | Description
| ------------- | -----------
| `_config.yml` | Configuration information controlling the Jekyll build.  This is a standard Jekyll file. See: [Jekyll directory structure](https://jekyllrb.com/docs/structure/)
| `build.sh`    | The website build script (usage documented below).
| `doc/`        | Per-release documentation for Apache Milagro (incubating). This directory contains one subdirectory per release, where each subdirectory contains the overall manual and API documentation for each part of the Milagro (incubating) core crypto libraries and server applications. Files in this directory are not interpreted by Jekyll, as there are far too many files for this to be reasonable. They are instead copied into place by the `build.sh` script.
| `images/`     | Images which are referenced within the website HTML and CSS.
| `pub/`        | Miscellaneous public files, such as test scripts. The test scripts in this directory have historically been shared to users to help with debugging.
| `styles/`     | All CSS files referenced by the website HTML.
| `_companies/` | Documents which contain metadata describing third-party companies that provide support for Apache Milagro (incubating). The content of these documents is rendered as the description for that company on the support page.
| `_includes/`  | Common HTML fragments used by Jekyll and referenced in other templates, such as the website header and footer. These *must* be HTML only. This is a standard Jekyll directory. See: [Jekyll directory structure](https://jekyllrb.com/docs/structure/)
| `_layouts/`   | Templates describing the structure of different types of content. This is a standard Jekyll directory. See: [Jekyll directory structure](https://jekyllrb.com/docs/structure/)
| `_links/`     | Documents which contain metadata describing the links which should appear in the site navigation menu. The documents here are completely empty except for the metadata.
| `_releases`   | Release notes for Milagro (incubating) releases, including metadata describing the files and documentation associated with those releases.

Build prerequisites
-------------------

The build depends entirely on Jekyll, thus this must be installed first. If you do not yet have Jekyll installed, please see [Jekyll's own installation instructions](https://jekyllrb.com/docs/installation/) to get started.

Beyond Jekyll, you will also need:

* A POSIX-compliant implementation of `sh` (to run `build.sh`)
* git (to clone this repository and to publish changes)
* `mktemp` (used by `build.sh` when staging changes prior to publishing)

The `build.sh` script will check whether the required programs are installed, and will fail early with an appropriate message if a required program cannot be found.

The build script handles three primary variations of common build tasks:

1. The build itself, when invoked as `./build.sh`. This invokes Jekyll and recursively copies the `doc/` (part of the website source) and `_site/` (generated by Jekyll during the build) to the `content/` directory.
2. Serving a local copy of the website, when invoked as `./build.sh PORT`.
3. Staging local changes for commit to the special "asf-site" branch, when invoked as `./build.sh stage`. Once changes are committed and pushed to "asf-site", the public website will be updated by the ASF's gitpubsub.

Testing changes locally
-----------------------

To test your changes to the website, you can either invoke `./build.sh` to build a static copy of the site to the `content/` subdirectory, or invoke `./build.sh PORT` (where `PORT` a TCP port number) to both build the static copy of the site *and* run Ruby's web server to serve `content/` locally at the given port:

    $ ./build.sh 8080
    Configuration file: /home/mjumper/apache-Milagro (incubating)/Milagro (incubating)-website/_config.yml
                Source: /home/mjumper/apache-Milagro (incubating)/Milagro (incubating)-website
           Destination: /home/mjumper/apache-Milagro (incubating)/Milagro (incubating)-website/_site
     Incremental build: disabled. Enable with --incremental
          Generating...
                        done in 0.563 seconds.
     Auto-regeneration: disabled. Use --watch to enable.
    Copying "doc/" and built site to "content/" ...
    Done. Full site is now within the "content/" directory.
    [2016-04-26 12:35:36] INFO  WEBrick 1.3.1
    [2016-04-26 12:35:36] INFO  ruby 2.1.7 (2015-08-18) [x86_64-linux]
    [2016-04-26 12:35:36] INFO  WEBrick::HTTPServer#start: pid=18927 port=8080

When done testing your local changes, press `Ctrl`&nbsp;+&nbsp;`C` to stop the web server and return to the shell.

Publishing changes
------------------

Changes to the website are published using Apache's [gitpubsub](https://blogs.apache.org/infra/entry/git_based_websites_available) which relies on a special branch called "asf-site" containing *all website content* within a directory called `content/`.

In the Apache Milagro (incubating) website repository, the "asf-site" branch is an "orphan" branch. The branch has no commits in common with the "master" branch and consists solely of the `content/` subdirectory. Updating the website thus involves:

1. Making and testing your changes locally
2. Replacing the entire contents of the `content/` directory within "asf-site" with the newly-generated `content/` directory.

The `build.sh` script can take care of all this for you when invoked as `./build.sh stage`:

    $ ./build.sh stage
    Configuration file: /home/mjumper/apache-Milagro (incubating)/Milagro (incubating)-website/_config.yml
                Source: /home/mjumper/apache-Milagro (incubating)/Milagro (incubating)-website
           Destination: /home/mjumper/apache-Milagro (incubating)/Milagro (incubating)-website/_site
     Incremental build: disabled. Enable with --incremental
          Generating...
                        done in 0.568 seconds.
     Auto-regeneration: disabled. Use --watch to enable.
    Copying "doc/" and built site to "content/" ...
    Done. Full site is now within the "content/" directory.
    Switched to branch 'asf-site'
    Removing README.md
    Removing _site/
    Content staged for commit. Use git to commit the results when ready.
    NOTE: The build.sh script will no longer exist. To serve the staged
    contents, run:

        ruby -run -e httpd content/ -p PORT

    where PORT is the port number where the HTTP server should listen.
    $

At this point, you will be on the "asf-site" branch and the current directory will contain only the `content/` subdirectory:

    $ ls
    content
    $

Keep in mind that the new content is only *staged* for commit. It has not yet been committed. Once you have verified that the staged content is as expected, commit your changes (along with a useful commit message describing the changes at a high level) using `git commit` and publish the update using `git push origin`.

If you wish to unstage your changes, use `git reset --hard HEAD` to return to the original state of "asf-site", wiping out any local modifications made by `build.sh`. You can then return to whichever branch you were working on with `git checkout`.
