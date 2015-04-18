It would be nice if the devs could all read this so we're all on the same page.

I was recently approached by an OLPC dev working on getting nxt-python included with the Fedora-based Sugar desktop for XO laptops (or something like that; I don't really know what's going on with OLPC right now). He was concerned about a number of things mainly relating to API changes based on versioning. In hindsight, I realize that I've been handling the API changes in nxt-python a bit badly. I should have changed the module name to nxt2 when the API changed in v2.0, but I was treating nxt-python more as a development tool than a packageable library. Hopefully this will change with the inclusion of nxt-python in PyPI and fedora's package manager.

There are four main issues which need to be addressed with relation to the development, packaging, and use of nxt-python:
  1. API Changes
  1. The SVN
  1. Transparency as a library
  1. The examples directory

At this point things are starting to break down into a slide presentation. I'm going to lay down some rules/conventions for each section individually:

## API Changes ##
API changes are sometimes needed to make it easier for the program to be used. In the past, changes have been made most notably to the motor and sensor modules. The general trend here has been moving from lower to higher level functions. This is not necessarily bad, but we need to keep the lower-level functions available should they be needed. We must remember that the basis of nxt-python, NXT\_Python 0.7, was quite literally a set of python functions (and in some cases even classes) which mapped straight to the direct command opcodes outlined in LEGO's "BDK". We've come quite a ways since then. But I have strayed from the topic.
  * API changes may be made across major versions, that is, for example, 1.x-2.x. Starting with v3 (if we get that far), the nxt module's name will change from nxt to nxt3, and so on with further versions.
  * API changes are forbidden across minor versions, that is, 2.x-2.x. Do not commit to the trunk anything which breaks API across revisions. Internal changes and restructuring are the main focus of minor versions. Additional functions which do not break existing scripts may be allowed in some cases. Check with the mailing list first. An exception to this is sensors: New sensor classes may be added to the trunk at any time.
  * ONLY bugfix changes are allowed across bugfix versions, that is, 1.3.x-1.3.x.

## The SVN ##
Development on a major version takes place in the trunk. When a new minor version's x.y.0 release occurs, a the code is branched into branches/vx.y. From this branch, a tag called tags/vx.y.0 is created, which the packages are subsequently built from. Bugfixes to a minor release are done on the trunk and then merged into the branch, from which is created a new tag from which packages are built. If the trunk has changed too much for changes to be merged directly, a separate fix may be made to the minor version branch.

When a developer wants to do some major restructuring without breaking the API, he should create a new branch from the trunk which is named appropriately. At the completion of the changes, they should be tested and when confirmed working the branch should be merged back into the trunk and then deleted unless further changes are to be made to it.

When the API is going to be changed for an upcoming major version, a new branch should be made. Historically, it has been named after a developer and then renamed to "vx" when the API changes are confirmed to be the basis of the new version. Once the branch is moderately stable, development has shifted to it primarily, and no more API changes will be made, the code of the current major version which is now in the trunk is copied into a new branch and the trunk is deleted. The naming of this branch will differ depending on the status of the current major version. If no new changes will be made, it will be named as a usual minor version branch and treated accordingly (with only bugfix changes being made). If there will be new changes made to the major version, the branch will be named branches/vx, where x is the major version number of the branch. The branch for the new major version is then used to create a new trunk and deleted. This means that, and this is important, _the trunk's API is **never** changed_. API changes are done on branches ONLY.

## Transparency as a library ##
In the past nxt-python has been treated by me as a plaform for development. As it is more and more being used as an actual library, I'm trying to make it behave more like one. This means not printing anything if asked to be quiet, not changing between minor versions, being in the PyPI, and a few other things. There really aren't any rules or procedures to be followed here, just keep this in mind while making changes.

## The examples directory ##
This is our largest bit of documentation. It's grown quite a bit since 0.7, and needs to be maintained as well. New features (especially sensors) should have examples added for them, and all examples should be updated to reflect any major version API changes. The examples directory was not included with any of the Windows installers, which was one of the chief reasons I decided not to include them after v2.0.1/v1.2.0.

This whole thing was made up by Marcus Wanner one afternoon while he was bored. It may be partially or entirely a bad idea. Discuss.