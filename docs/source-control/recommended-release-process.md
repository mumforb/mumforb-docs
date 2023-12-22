# Release Process

This document is meant to provide a starting point for a potential release cadence. For applications that consume (or are consumed) APIs, artifacts, or other dependencies, having a predictable, consistent release cadence is critically important. This discipline, combined with tooling that provides predictability and lowers developer friction, will create pathways to faster development with fewer mistakes, which will lead to more time and energy to take up a new hobby. (Or whatever.)

## Branch Structure

We will be taking as assumed that we are using `git` (hosted on Gitlab) for this process.

There are a few different branching models that are compatible with this release strategy, but the most important is that there is a specific branch for releases. This can be the main/default branch, or it can be a secondary branch off of that called `release`. We will be making an assumption of the latter process in this document.

So, your branch structure might look something like this:

![Release Branch Structure Stage 1](../img/release-branch-structure-stage-001.drawio.svg "Release Branch Structure Stage 1")

(Bugfixes, etc., omitted for clarity. Obviously.)

So, there are a few ways to go from this very simple starting place. You can have a release triggered after every commit to our main branch (`develop` in this example), or you can manually trigger releases via some other mechanism (maybe building locally). Another option, however, would be to add an additional branch, called release. This allows for collecting `x` commits to the main branch, and deploying only those commits, perhaps after a testing cycle, or on a pre-determined cadence matching sprints.

![Release Branch Structure Stage 2](../img/release-branch-structure-stage-002.drawio.svg "Release Branch Structure Stage 2")

In this second example, a determination was made to release following the check-in of `Feature-001`. `Feature-002` is reserved for the following release.

## Release Process

So, based on this second structure, we can build one of a few different release processes, all of which are [supported and documented by Gitlab](https://docs.gitlab.com/ee/user/project/releases/). We'll pick a specific one for the purposes of this proposal, understanding that there are a variety of valid approaches.

### Create a Release

One good way to specify a release is by using a `Git` tag. A "tag" is a marker within the repository that creates a commit. This can then be utilized as a way to reference the repository as it exists at a certain point in time. By creating tags off the Release branch, we can restrict full releases to our package repositories as only taking place when we intend, but have flexibility to include whatever we need to as a release. (This allows for certain, unavoidable situations when the main branch and the release branch must differ, perhaps due to production hotfixes.)

![Release Branch Structure Stage 3](../img/release-branch-structure-stage-003.drawio.svg "Release Branch Structure Stage 3")

So, either by using the Gitlab UI or a local Git client, on the Release branch, create a tag that matches the version number you would like to commit to. We will utilize a [Semantic Versioning](https://semver.org/) based approach to versioning.

!!! alert
    This manual versioning is very much, if you will allow the term, an `alpha` product. We will rapidly explore automatic versioning number bumping systems that make sense within the individual repositories.

### Release a Release

Once this release is created, the individual repository's `.gitlab-ci.yml` will extend the default Release templates and execute the packaging and uploading processes defined therein.

!!! alert
    These default Release templates are WIP - specific usage notes will be updated here once they're further along. 

### Release Notes

Also in the release templates is an "Update Release Notes" process, which can also be leveraged with your own strategy to automatically pull in information and create documentation on what is included in the most recent release. This can include resolved Issues directly from Gitlab, and/or a listing of all the commits that are new since the last tag, or something else. Regardless, this build step will log and distribute the release notes to provide transparency and predictability.


