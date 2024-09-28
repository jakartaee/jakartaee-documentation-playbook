# Jakarta EE Documentation

This is the repo for building the Jakarta EE Documentation site (from different repos); currently this consists of the Jakarta EE Tutorial and Eclipse Cargo Tracker.

- Repo for the tutorial content: [https://github.com/jakartaee/jakartaee-tutorial/](https://github.com/jakartaee/jakartaee-tutorial/)
- Repo for the Cargo Tracker content (note that the content resides in the docs branch): [https://github.com/eclipse-ee4j/cargotracker/tree/docs](https://github.com/eclipse-ee4j/cargotracker/tree/docs)
- Repo for the documentation UI: [https://github.com/jakartaee/jakartaee-documentation-ui/](https://github.com/jakartaee/jakartaee-documentation-ui/)

## Prerequisites

- [JDK](https://jdk.java.net/)
- [Maven](https://maven.apache.org/)
- [Ruby](https://rvm.io/)

## Setup

JDK and Maven speak for themselves.

For Ruby, if `ruby -v` returns something like `Command 'ruby' not found` then [read the instructions](https://rvm.io/rvm/install) to install "RVM stable".
Summarized:

```bash
gpg --keyserver keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable --ruby
source ~/.rvm/scripts/rvm
```

Finally, you will also need to install the [`asciidoctor-pdf`](https://rubygems.org/gems/asciidoctor-pdf) gem:

```bash
gem install asciidoctor-pdf
```

## Building

To build, run:

```bash
mvn clean package
```

The output will be in `target/generated-docs`.

To view the Tutorial, open [`target/generated-docs/jakartaee-tutorial/current/index.html`](target/generated-docs/jakartaee-tutorial/current/index.html) in a browser.

```bash
browse target/generated-docs/jakartaee-tutorial/current/index.html
```

To view Cargo Tracker, open [`target/generated-docs/cargotracker-documentation/current/index.html`](target/generated-docs/cargotracker-documentation/current/index.html) in a browser.

```bash
browse target/generated-docs/cargotracker-documentation/current/index.html
```

If you face a build failure with the following log entry as the last one before the failure, basically saying "Command not found: asciidoctor-pdf":

> [INFO] {"level":"fatal","time":1684333903235,"name":"antora","hint":"Add the --stacktrace option to see the cause of the error.","msg":"Command not found: asciidoctor-pdf"}

Then you need to run this command beforehand:

```bash
source ~/.rvm/scripts/rvm
```

You will need to make sure this is executed every time you open a new terminal. You could also simply add the script to your login profile.

### Author Mode

Antora supports an Author Mode that lets you work with local branches and your local worktree.
This requires that you keep a local copy of `antora-playbook.yml` as `local-antora-playbook.yml`.
Read [Use Author Mode :: Antora Docs](https://docs.antora.org/antora/latest/playbook/author-mode/) for details. 

Summarized:

1. Copy and paste `antora-playbook.yml` in same folder as `local-antora-playbook.yml`.
2. In case you wish to use a different `jakartaee-tutorial` branch, edit the `content` entry.
    * In case you wish to use the current local repo and branch:
        ```
        content:
          sources:
          - url: ../jakartaee-tutorial
            start_paths:
              - src/main/antora
            branches:
              - HEAD
        ```
    * In case you wish to use a different remote branch, e.g. when you have forked the `jakartaee-tutorial` repo:
        ```
        content:
          sources:
          - url: https://github.com/yourGitUserName/jakartaee-tutorial.git
            start_paths:
              - src/main/antora
            branches:
              - yourBranchName
        ```
3. In case you wish to use a different `cargotracker` branch, edit the `content` entry.
    * In case you wish to use the current local repo and branch:
        ```
        content:
          sources:
            - url: ../cargotracker
              start_paths:
                - src/main/antora
              branches:
                - docs
        ```
    * In case you wish to use a different remote branch, e.g. when you have forked the `cargotracker` repo:
        ```
        content:
          sources:
          - url: https://github.com/yourGitUserName/cargotracker.git
            start_paths:
              - src/main/antora
            branches:
              - yourBranchName
        ```
              
4. In case you wish to use a different `jakartaee-documentation-ui` bundle, edit the `ui` entry.
    * In case you wish to use the locally built `jakartaee-documentation-ui` bundle as instructed in "Package the UI" section of the README over there:
        ```
        ui:
          output_dir: _
          bundle:
            url: ../jakartaee-documentation-ui/build/ui-bundle.zip
            snapshot: true
        ```
        Note that this assumes that you have the UI repo checked out in the same parent folder as the current repo.
    * In case you wish to use a different release, e.g. when you have forked the `jakartaee-documentation-ui` repo:
        ```
        ui:
          output_dir: _
          bundle:
            url: https://github.com/yourGitUserName/jakartaee-documentation-ui/releases/download/latest/ui-bundle.zip
            snapshot: true
        ```

Once you've created the `local-antora-playbook.yml` file, you can use the `author-mode` Maven profile:

```bash
mvn compile -Pauthor-mode
```

The output will still be in the same location, but it'll be generated from your local clone of the repos instead of the remote.

```bash
browse target/generated-docs/jakartaee-tutorial/current/index.html
```

```bash
browse target/generated-docs/cargotracker-documentation/current/index.html
```

## Deploying

This site is currently deployed via GitHub Pages via GitHub Actions.
For details, see the [workflow file](.github/workflows/build-and-deploy.yml).

The current URL is [https://jakartaee.github.io/jakartaee-documentation/](https://jakartaee.github.io/jakartaee-documentation/).
