# Gitlab

Gitlab is a richly featured platform with a wide variety of modern software development and release features. It has broad capabilities, including:

- GitOps
- Source Control
- Kubernetes Cluster Administration

It is critical that each team member have familiarity with the way Gitlab works, as every software operation flows through it. Gitlab runs on their own 'hosted' servers, or locally on a company's own servers. 

## Git 101

Git is an open-source version control system. It is fast, flexible, and lightweight, as a result, is used very widely. A full implementation of Git requires a remote and a local instance. All of its local functionality is available via its CLI, but there a are a few commonly used UI helpers to aid in visualization of common tasks.

### How Git Works

As a version control system, Git's main job is to keep track of changes to a set of files. At it's most basic, this means that if Bob changes a file, and then Alice changes the same line of the same file, if Git is being used to track those changes, it will alert Alice to this fact and (hopefully) prevent Bob's code from being overwritten, or both changes being present, causing a failure.

At it's core is a branching structure, which can be visualized as seen below, with `main` being the primarily branch and `feature` being a branch with a change that is being worked on by an individual developer. When taking on this work, the developer will start with `main`, and then create a branch called feature. They will do their work on their local computer, and when completed, they will use Gitlab to demonstrate their changes (using what is called in Gitlab a Merge Request, or MR).

```
     /------feature-----\
    /                    \
-------------main----------------
```

Git tracks the changes, sorts out the order in which they were made, and provides feedback to the developer when they are committing changes. Gitlab provides visibility into these functions while providing easy-to-use workflows. Other functionality provided in Gitlab extends out of this core, Git-based feature set.


## Git Tools

As mentioned before, Git consists of at least two "ends" - a local and a remote. Each local instance will need some copy of Git's software in order to interact with the remote. Every [VSCode Dev Container](vscode.md#dev-container) will come with Git installed in the CLI.

In addition to using the command line inside VSCode, we recommends using the GitLens extension that is available inside VSCode, and will typically be installed inside the Dev Container as well.

## Project Structure

## GitOps
