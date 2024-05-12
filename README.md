# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioning. for it tagging.

(semver.org)[https://semver.org/]

The genaral format:

**MAJOR.MINOR.PATCH**, example `1.0.1 ` `

- **MAJOR**version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixe

## Install the Terraform CLI

### Considerations with the Terraform CLI changes

The installation instructions for Terraform CLI have been updated to account for recent changes to gpg keyring. The updated instructions can be found in Terraform Documentation and changes have been made to the installation script.

(Install Terraform CLI) (https://developer.hashicorp.com/terraform/install)

### Considerations for Linux Distributions

This project is built against Ubuntu. Please consider checking your Linux Distribution and change according to distribution needs.
[How to Check OS Version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of Checking OS Version:

```
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.4 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy 
```
### Refactoring into Bash Scripts

We recently encountered some issues with Terraform CLI gpg deprecation and noticed that bash script steps were quite lengthy. In order to simplify the process, we have created a new bash script to install the Terraform CLI.

This script can be found at: [./bin/install_terraform_cli](./bin/install_terraform_cli)

There are several reasons why we decided to create this script.

- It helps to keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml)) neat and organized.
- It is easier to debug and manually execute the installation of the Terraform CLI.
- It allows for greater portability for other projects that require the installation of the Terraform CLI.

#### Shebang Consideration

A Shebang (pronounced Sha-bang)tells the bash script what program will interpret the script. eg.`#!/bin/bash`

ChatGPT recommended this format for bash: `#!/usr/bin/env bash`

- for portability for different OS distributions
- Will search the user's PATH for the bash executable.

https://en.wikipedia.org/wiki/Shebang_(Unix)

## Execution Consideration


When executing the bash script we can use the `./` shorthand notation to execute the bash script.

eg. `./bin/install_terraform_cli`

If we are using a script in .gitpod.yml we need to point the script to a program to interpret it.


#### Linux Permission

In order to make our bash script executable we need to change Linux permission for the fix to be executable at the user mode.

```sh
chmod u+x ./bin/install_terraform_cli
```

alternatively:

```sh
chmod 744 ./bin/install_terraform_cli
```

https://en.wikipedia.org/wiki/Chmod

### Gitpod Lifecycle (Before, Init, command)

We should be cautious when using the Init command as it won't rerun on restarting an existing workspace.

https://www.gitpod.io/docs/configure/workspaces

### Working Env Vars

#### env command

To filter specific Environment Variables using grep, we can use the following command: `env | grep AWS_`.

#### Setting and Unsetting Env Vars

To set an environment variable in the terminal, we can use the command `export HELLO='WORLD'`. Similarly, to unset a variable, we can use the command `unset HELLO`.


We can set an env var temporarily when just running a command
```sh
HELLO='world' ./bin/print_message
```
Within a bash script, we can set env without writing export eg.

```sh
#!/usr/bin/env bash

HELLO='World'

echo $HELLO
```

#### Printing Vars

We can print an env using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open up new bash terminals in Vscode it will not be aware of env vars that you have set in another window.

If you want Env Vars to persist across all future bash terminals that are open you need to set env vars in your bash profile.eg. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='World`
```

All future workspaces launched will set the env vars for all bash terminals opened in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.