# Workshops
Docs and code samples from workshops, including software setup and PyTorch/TensorFlow tutorials

- [Software Setup](#software-setup)
  - [Integrated Development Environment (IDE)](#ide)
  - [Version Control](#version-control)
  - [Dependency Management and Virtual Environment](#dependency-venv)
  - [Installing PyTorch](#installing-pytorch)

# Software Setup <a name="software-setup"></a>

## Integrated Development Environment (IDE) <a name="ide"></a>

An integrated development environment (or IDE) is the application you use to write your code (i.e. a "source code editor"), and it's often bundled with debugging tools, a version control GUI, clickable class hierarchies, build tools, and so on. We'll be using VSCode, which is a popular IDE used in both research and industry. (PyCharm is another great option, particularly to debug Python — but we'll stick with VSCode.)

### Installing VSCode

Visit Microsoft's [Visual Studio Code site](https://code.visualstudio.com) to download the stable build of VSCode for your operating system.

### Install VSCode extensions

Click the extensions section. (This is on the vertical bar at the very left of the VSCode window.) Install the following extensions.
- Python
- Pylance
- Remote - SSH
- Remote - SSH: Editing Configuration
- Git History
- GitLens

### Useful VSCode settings

Open your VSCode settings (`Code > Preferences > Settings`).
- Search for "trim". Enable `Files: Trim Trailing Whitespace`.
- (Optional) By default, VSCode uses "preview mode", where files don't permanently open until you double-click or edit them. If this is unintuitive to you, you can search for "enable preview" and uncheck `Workbench > Editor: Enable Preview` and `Workbench: Enable Preview From Quick Open`. 
  - _Note from Faiaz: I keep preview mode on since it's useful when you're clicking through a large codebase and don't want all the files you view briefly to remain open in your editor._

### Using VSCode

Here are some useful hotkeys you can use when working in VSCode. Note: These are based on Mac, so if you're using a PC, replace ⌘ (`CMD`) with `CTRL`, and `OPTION` with `ALT`.
- `CTRL` + ` (backtick): open the terminal
  - _Note: This is the CTRL key on Mac, not the CMD key._
- ⌘ + `P`: search for a file (by name) in your workspace
- ⌘ + `SHIFT` + `F`: search for any string through all the files in your workspace
- ⌘ + `SHIFT` + `L`: select all occurrences of the current selection
- `OPTION` + click: select multiple locations, i.e. producing multiple cursors where you can type to simultaneously

## Version Control <a name="version-control"></a>

Version control allows you to automatically track changes to software code over time, creating a rich version history you can compare changes across. If a change causes the code to break, you can use your version control system to revert to a previous version. **Git** is the version control system used by **GitHub**, a a hosting service for managing Git repositories. GitHub makes it easy to collaborate with others on the same codebase, work on multiple branches of a repository, and merge changes together.

### Setting up Git on your local machine

The [GitHub Docs](https://docs.github.com/en/get-started/quickstart/set-up-git) explain in detail how to set up Git. We'll go over the essential steps here, but go through the GitHub Docs to read through more detailed explanations.

First, install Git for your operating system from the [Git site](https://git-scm.com/downloads). Next, open your terminal and set your username in Git. We're using the `--global` flag so it sets your Git username for every repository on your computer. (General note: Terminal commands are prefixed with `$` — you don't need to type that in the command, it just means that the remainder of the line should be typed into the terminal. Similarly, terminal output is typically prefixed with `>`, or nothing.)

```
$ git config --global user.name "Mona Lisa"
```

You can check that this works as follows.

```
$ git config --global user.name
> Mona Lisa
```

Next, set your commit email address on GitHub by following the instructions in the [GitHub Docs](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-user-account/managing-email-preferences/setting-your-commit-email-address). Then, you can set your email address for every repository on your computer.

```
$ git config --global user.email "email@example.com"
```

You can check the same way as last time.

```
$ git config --global user.email
> email@example.com
```

### Authenticating with GitHub from Git via SSH

Now that you've got Git set up on your machine, you need to authenticate with GitHub so that when you connect to a GitHub repository from Git in your terminal, youll have authorization to push changes to the code.

You can connect either over HTTPS or SSH. While HTTPS is a bit easier initially, SSH is used more in industry (and automates some processes when you push changes), so we'll use that.

First, if you don't already have an SSH key, generate an SSH key (refer to [these docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)).

In your terminal, do the following. (When it prompts you to enter a file, just press Enter to use the default location. For the passphrase, you could leave it empty.)
```
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```
If you system doesn't have the Ed25519 algorithm, use the following.
```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Next, add your SSH key to the ssh-agent.
```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

Then, check if your `~/.ssh/config` file exists in the default location. 
```
$ open ~/.ssh/config
> The file /Users/you/.ssh/config does not exist.
```

If it doesn't exist, create the file.
```
$ touch ~/.ssh/config
```

Open your `~/.ssh/config` file, then modify it to contain the following lines. Note that if your SSH key file has a different name (e.g. if you used RSA to generate your key, it'll probably look like `~/.ssh/id_rsa`). 
```
Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

If you had a passphrase for your key, you need to include an additional line.
```
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Next, add your SSH private key to the ssh-agent.
```
$ ssh-add -K ~/.ssh/id_ed25519
```

Finally, you need to add your new SSH key to your GitHub account. Copy the SSH public key to your clipboard using the appropriate command for your operating system.
```
# Mac
$ pbcopy < ~/.ssh/id_ed25519.pub
# Copies the contents of the id_ed25519.pub file to your clipboard

# Windows
$ clip < ~/.ssh/id_ed25519.pub
# Copies the contents of the id_ed25519.pub file to your clipboard

# Linux
$ cat ~/.ssh/id_ed25519.pub
# Then select and copy the contents of the id_ed25519.pub file
# displayed in the terminal to your clipboard
```

Then go to GitHub's website, click your profile icon in the top right, and click Settings. Go to SSH and GPG keys in the sidebar, and press `New SSH Key` or `Add SSH Key`. For the title, you can call it something like `faiaz-macbook` or `faiaz-work-laptop` (e.g. identifying yourself and the device you're using, in case later on you add additional SSH keys from different devices). Paste your key into the Key field, press Add, and you're done!

### Cloning your project repository

Now, you can clone repositories with SSH.

## Dependency Management and Virtual Environment <a name="dependency-venv"></a>

### Creating a virtual environment with conda

### Creating a `requirements.txt` file and freezing dependencies

### Adding, commiting, and pushing changes with Git

## Installing PyTorch <a name="installing-pytorch"></a>

PyTorch can be installed very easily using conda. Visit [PyTorch's website](https://pytorch.org) and select the configuration based on your machine. This will be `Stable` for your build, `Conda` for your package, `Python` for your language, and `CPU` for your compute platform. (Later on, when we run model training on GPU with CUDA, we'll install the appropriate CUDA versions, but don't worry about that for now!)

<img width="811" alt="image" src="https://user-images.githubusercontent.com/42232624/138920194-c443ef1c-88ac-4435-929c-3043a5408077.png">

Make sure your virtual environment is active in your terminal instance, and then run the `conda install ...` command that the PyTorch site has generated for you based on your configuration.
