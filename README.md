# Jakarta EE Documentation

This is the repo for building the Jakarta EE Documentation site (from different repos); currently this consists of the Jakarta EE Tutorial and Eclipse Cargo Tracker.

- Repo for the tutorial content: [https://github.com/jakartaee/jakartaee-tutorial/](https://github.com/jakartaee/jakartaee-tutorial/)
- Repo for the Cargo Tracker content (note that the content resides in the docs branch): [https://github.com/eclipse-ee4j/cargotracker/tree/docs](https://github.com/eclipse-ee4j/cargotracker/tree/docs)
- Repo for the documentation UI: [https://github.com/jakartaee/jakartaee-documentation-ui/](https://github.com/jakartaee/jakartaee-documentation-ui/)

## Prerequisites

The tools below are all required for building this project. 

Maven drives the entire process, requires the Java Development Kit (JDK).
Asciidoctor, which process the documentation content, requires Ruby.
Antora, which builds the documentation site (using YaML configuration), uses Node.js and npm,
but Maven automatically handles installation and execution.

- [JDK](https://jdk.java.net/)
- [Maven](https://maven.apache.org/)
- [Ruby](https://rvm.io/)

> NOTE: We assume you're using a UNIX/Linux shell such as bash, zsh, or sh. 
On Windows, we assume you're using Git Bash or Windows Subsystem for Linux. If not, you'll have to translate these commands to PowerShell or Command Prompt commands.

## Setup

### JDK

Any recent JDK will do. 
If you don't have a Java installed, you can get a recent version here: https://jdk.java.net/. 
If you want to manage multiple JDKs on your system, 
consider using [SDKMan](https://sdkman.io/) or [jenv](https://www.jenv.be/).

### Maven

Any recent version of Maven will do.
If you don't have it installed, [download Maven](https://maven.apache.org/download.cgi) 
and then install it manually by following [these directions](https://maven.apache.org/install.html).

### Ruby

If `ruby -v` returns something like `Command 'ruby' not found` then [read the instructions](https://rvm.io/rvm/install) to install "RVM stable".

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

If you face a build failure with the following log entry as the last one before the failure, basically saying "Command not found: asciidoctor-pdf":

> [INFO] {"level":"fatal","time":1684333903235,"name":"antora","hint":"Add the --stacktrace option to see the cause of the error.","msg":"Command not found: asciidoctor-pdf"}

Then you need to run this command beforehand:

```bash
source ~/.rvm/scripts/rvm
```

You will need to make sure this is executed every time you open a new terminal. You could also simply add the script to your login profile.

### Viewing Ouput

The output will be in `target/generated-docs`.
To view the Tutorial, open [`target/generated-docs/jakartaee-tutorial/current/index.html`](target/generated-docs/jakartaee-tutorial/current/index.html) in a browser.

Linux
```bash
browse target/generated-docs/jakartaee-tutorial/current/index.html
```

macOS
```bash
open target/generated-docs/jakartaee-tutorial/current/index.html
```

Windows
```bash
start target/generated-docs/jakartaee-tutorial/current/index.html
```

To view Cargo Tracker, open [`target/generated-docs/cargotracker-documentation/current/index.html`](target/generated-docs/cargotracker-documentation/current/index.html) in a browser.

Linux
```bash
browse target/generated-docs/cargotracker-documentation/current/index.html
```

macOS
```bash
open target/generated-docs/cargotracker-documentation/current/index.html
```

Windows
```bash
start target/generated-docs/cargotracker-documentation/current/index.html
```

### Author Mode

Antora supports an Author Mode that lets you work with local branches and your local worktree.
This requires that you keep a local copy of `antora-playbook.yml` as `local-antora-playbook.yml`.

We recommend cloning other repos which have content you want to modify.
For example, let's say you want to modify content in the 
[jakartaee-tutorial](https://github.com/jakartaee/jakartaee-tutorial/) and 
[cargotracker](https://github.com/eclipse-ee4j/cargotracker/tree/docs) repos.

You'd clone the repos in the same parent folder as this repo, so you'd end up with this directory structure:

  ```bash
  ├── parent-directory
      ├── jakartaee-documentation/
      └── jakartaee-tutorial/
      └── cargotracker/
  ```

You can [Use Author Mode :: Antora Docs](https://docs.antora.org/antora/latest/playbook/author-mode/) for details,
but here is the summary of the process:

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

You can then view it as described in the [Viewing Output](#viewing-output) section above.

## Deploying

This site is currently deployed via GitHub Pages via GitHub Actions.
For details, see the [workflow file](.github/workflows/build-and-deploy.yml).

The current URL is [https://jakartaee.github.io/jakartaee-documentation/](https://jakartaee.github.io/jakartaee-documentation/).
