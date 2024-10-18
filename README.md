# jakartaee-documentation

This is the repo for building the Jakarta EE Documentation site (from different repos); currently this consists of the Jakarta EE Tutorial.

- Repo for the tutorial content: [https://github.com/jakartaee/jakartaee-tutorial/](https://github.com/jakartaee/jakartaee-tutorial/)
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

And finally install the [`asciidoctor-pdf`](https://rubygems.org/gems/asciidoctor-pdf) gem:

```bash
gem install asciidoctor-pdf
```

## Building

To build, run:

```bash
mvn clean package
```

The output will be in `target/generated-docs`.
To view, just open [`target/generated-docs/jakartaee-tutorial/current/index.html`](target/generated-docs/jakartaee-tutorial/current/index.html) in a browser.

```bash
browse target/generated-docs/jakartaee-tutorial/current/index.html
```

If you face a build failure with the following log entry as the last one before the failure, basically saying "Command not found: asciidoctor-pdf":

> [INFO] {"level":"fatal","time":1684333903235,"name":"antora","hint":"Add the --stacktrace option to see the cause of the error.","msg":"Command not found: asciidoctor-pdf"}

Then you need to run beforehand:

```bash
source ~/.rvm/scripts/rvm
```

Or to make sure this is executed every time you open a new terminal.

### Author Mode

Antora supports an Author Mode that lets you work with local branches and your local worktree.
This requires that you keep a local copy of `antora-playbook.yml` as `local-antora-playbook.yml`.
Read [Use Author Mode :: Antora Docs](https://docs.antora.org/antora/latest/playbook/author-mode/) for details.

Summarized:

1. Copy and paste `antora-playbook.yml` in same folder as `local-antora-playbook.yml`.
2. In case you wish to use a different `jakartaee-tutorial` branch, edit the `content` entry.
   - In case you wish to use the current local repo and branch:
     ```
     content:
       sources:
       - url: ../jakartaee-tutorial
         start_paths:
           - src/main/antora
         branches:
           - HEAD
     ```
   - In case you wish to use a different remote branch, e.g. when you have forked the `jakartaee-tutorial` repo:
     ```
     content:
       sources:
       - url: https://github.com/yourGitUserName/jakartaee-tutorial.git
         start_paths:
           - src/main/antora
         branches:
           - yourBranchName
     ```
3. In case you wish to use a different `jakartaee-documentation-ui` bundle, edit the `ui` entry.
   - In case you wish to use the locally built `jakartaee-documentation-ui` bundle as instructed in "Package the UI" section of the README over there:
     ```
     ui:
       output_dir: _
       bundle:
         url: ../jakartaee-documentation-ui/build/ui-bundle.zip
         snapshot: true
     ```
     Note that this assumes that you have the UI repo checked out in the same parent folder as the current repo.
   - In case you wish to use a different release, e.g. when you have forked the `jakartaee-documentation-ui` repo:
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

Linux:
```bash
browse target/generated-docs/jakartaee-tutorial/current/index.html
```

macOS:
```bash
open target/generated-docs/jakartaee-tutorial/current/index.html
```

### Run and Build Jakarta EE Documentation in Author Mode Using `build-jakartaee-docs.sh` Script

This documentation describes how to use `build-jakartaee-docs.sh`
script to automate the process of building the Jakarta EE
Documentation UI and Jakarta EE Documentation repositories in author
mode.
This script simplifies the workflow by handling UI bundle creation,
playbook configuration, and documentation building, while ensuring
platform compatibility (Linux,
macOS, and Windows).

### Prerequisites

Before using the script, ensure that:

- You have cloned both repositories `jakartaee-documentation-ui` and `jakartaee-documentation` in the same parent directory:

  ```bash
  ├── parent-directory
      ├── jakartaee-documentation-ui/
      └── jakartaee-documentation/
  ```

- Node.js and Gulp are installed for building the UI.
- Apache Maven is installed for building the documentation.
- You are running the script from the root of the `jakartaee-documentation` repository.

### Using the Script

1. The `build-jakartaee-docs.sh` script is in the root directory of
   `jakartaee-documentation`.

2. Ensure both `jakartaee-documentation-ui` and
   `jakartaee-documentation` repositories are cloned in the same parent
   directory.

3. Make the script executable by running.

   ```bash
   chmod +x build-jakartaee-docs.sh
   ```

4. Run the script from the terminal or command line:

   ```bash
   ./build-and-run.sh
   ```

5. The script will automatically:
   - Build the Jakarta EE Documentation UI.
   - Copy and modify the playbook.
   - Build the Jakarta EE Documentation in author mode.
   - Open the resulting index.html in your default browser.

### Script Overview

The script performs the following steps:

1. Build `jakartaee-documentation-ui:`

   - Navigates to the `jakartaee-documentation-ui` repository and builds the UI using gulp bundle.

2. Switch Back to `jakartaee-documentation:`

   - Navigates back to the `jakartaee-documentation` repository after building the UI.

3. Copy the Antora Playbook:

   - Copies the existing `antora-playbook.yml` to a new file called `local-antora-playbook.yml` to be used for the local setup.

4. Modify `local-antora-playbook.yml:`

   - Updates the `ui.bundle.url` in the playbook to point to the locally
     built UI bundle located at `../jakartaee-documentation-ui/build/
ui-bundle.zip` instead of the remote prebuilt bundle.

5. Build the Jakarta EE Documentation:

   - Builds the documentation in author mode by running the command
     `mvn compile -Pauthor-mode`.

6. Open the Generated `index.html`:
   - After the Maven build completes, the script navigates to the
     `target/generated-docs` directory and opens the `index.html` file
     in the default web browser. The script supports opening the file
     on Linux, macOS, and Windows.

## Deploying

This site is currently deployed via GitHub Pages via GitHub Actions.
For details, see the [workflow file](.github/workflows/build-and-deploy.yml).

The current URL is [https://jakartaee.github.io/jakartaee-documentation/](https://jakartaee.github.io/jakartaee-documentation/).
