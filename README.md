# jakartaee-tutorial-playbook
Root repo for building the Jakarta EE Tutorial site (from different repos).

Issue tracker: https://virtua.atlassian.net/jira/software/c/projects/JETUT/issues

Wiki: https://virtua.atlassian.net/wiki/spaces/JETR/overview

## Building

To build, run:

```
mvn clean verify
```

The output will be in `target/generated-docs`. To view, just open `target/generated-docs/index.html` in a browser.

### Author Mode

Antora supports an Author Mode that lets you work with local branches and your local worktree. This requires that you keep a local copy of `antora-playbook.yml`. Read [these instructions](https://docs.antora.org/antora/latest/playbook/author-mode/) for details. 

Once you've created this file, you can use the `author-mode` Maven profile:

```
mvn compile -Pauthor-mode
```

The output will still be in the same location, but it'll be generated from your local clone of the repos instead of the remote.

## Deploying

This site is currently deployed via GitHub Pages via GitHub Actions. For details, see the [workflow file](.github/workflows/build-and-deploy.yml).




