# Windows support

This project includes useful functionality such as loading secrets as environment
variables, but excluding the secrets from version control.

However, some of this functionality is Unix-based, and not supported natively on
Windows. This page covers how to configure your Windows system to use this Unix-based
functionality in its entirety.

## Requirements

```{warning}
We have only tested Windows support with the following requirements. Your experience may differ.
```

* Windows 10 64-bit system
* user account with administrator privileges
* [Git for Windows installed with the correct settings](#git-for-windows-setup)
* [install and configure the Windows release of `direnv`](#setting-up-direnv)
* set up `Make` via `git-bash`

## Git for Windows setup

[If you do not have Git for Windows installed, follow the fresh installs
instructions](#fresh-installs). Otherwise, [check the configuration options for your
existing install](#existing-installs).

### Fresh installs

Following these steps if you do not have Git for Windows installed on your machine

1. [download Git for Windows][git-for-windows]
2. double-click on the executable `.exe` file. It should be called
   `Git-{VERSION}-64-bit.exe`, where `{VERSION}` is the version of Git for Windows
3. go through all the installation instructions until Step 10 "Configuring extra
   options", and check the `Enable symbolic links` checkbox
4. continue with the rest of the installation instructions, and finish the installation
5. [check that symbolic links have been configured](#existing-installs)

### Existing installs

If you have installed Git for Windows, follow these steps to check that the correct
symbolic links have been configured.

1. open the Run dialog box (shortcut: Win + R)
2. type `%PROGRAMDATA%/Git` in the text field and press the "OK" button
3. in the File Explorer, you should see a file called `config`. Open this in your
   favourite text editor, and check that the `symlinks` variable is set to `true`
4. If not, you will need to re-open the `config` file in your text editor with
   administrator privileges to change the `symlinks` value to `true`

Once the symbolic links have been configured, [you can set up
`direnv`](#setting-up-direnv).

## Setting up `direnv`

First, [set up Git for Windows, and ensure symbolic links are
configured](#git-for-windows-setup). Then follow these sections to install, and
configure `direnv`.

### Download `direnv`

[Download the latest Windows 64-bit release of `direnv`][direnv-releases]; this is
usually called `direnv.windows-amd64.exe`.

For maintainability, create a `direnv` folder within your Git for Windows installation
location, for example `C:\Program Files\Git\usr\bin\direnv`, and move the download
there. You may need administrator privileges to do this. Now [set up the symbolic
link](#set-up-direnv-symbolic-link).

### Set up `direnv` symbolic link

Next you need to set up a symbolic link to `direnv`, so it runs directly from the Git
for Windows prompt.

1. open the Run dialog box (shortcut: Win + R)
2. type `cmd` to open the command prompt
3. create a [symbolic link][git-for-windows-symbolic-links] so that entering the
   `direnv` command runs the `direnv.windows-amd64.exe` executable file.
   ```bash
   mklink direnv {FILEPATH}
   ```
   where `{FILEPATH}` is the filepath to the `direnv.windows-amd64.exe` executable
   file. Remember, if your filepath contains spaces, you will need to double quote it,
   for example:
   ```bash
   mklink direnv "C:\Program Files\Git\usr\bin\direnv\direnv.windows-amd64.exe"
   ```

[Now you need to add `direnv` hooks to Git for Windows](#add-direnv-hooks-to-git-bash).

### Add `direnv` hooks to `git-bash`

Adding hooks to `git-bash` ensures `direnv` is ready to run as soon as you open Git
Bash.

1. open `git-bash` — in the Start menu this should be listed in the Git folder as
   "Git Bash"
2. add the `direnv` hook to your `.bash_profile`
   ```bash
   echo 'eval "$(direnv hook bash)"' >> ~/.bash_profile
   ```
3. Restart `git-bash` by closing it, and opening `git-bash` again
4. Check that `direnv` has been correctly installed by entering the `direnv` command
   ```bash
   direnv
   ```
   You should see a list of available `direnv` commands printed out.

## Setting up `Make` in `git-bash`

`Make` allows you to run the helper commands in the `Makefile`. This is an optional
feature of `govcookiecutter`; you can see what each of the `make` commands do in the
`Makefile`.

To enable this optional feature on Windows, [first install Git for
Windows](#git-for-windows-setup). Then follow these steps [to install `ezwinports` to
enable `Make` in `git-bash`][so-ezwinports].

1. [download the `make-{VERSION}-without-guile-w32-bin.zip` file from
   `ezwinports`][ezwinports], where `{VERSION}` is the version of the `Make` port
2. unzip the download, and copy all the folders _except_ `share`
3. paste the folders into the `mingw64` folder of your Git installation, for example at
   `C:\Program Files\Git\mingw64`. Do not overwrite or replace any files — only merge
   them
6. Open `git-bash`, and type `make` — you should see the following message:
   ```bash
   make: *** No targets specified and no makefile found.  Stop.
   ```

To get all the `make` commands in the `Makefile` work, you also need to create a
symbolic link to the Windows `more` command:

1. open the Run dialog box (shortcut: Win + R)
2. type `cmd` to open the command prompt
3. enter the following command:
   ```bash
   mklink more C:\Windows\System32\more.com
   ```

You should now be able to run all the `make` commands within the `Makefile`.

## Other useful instructions

Here are links to other useful instructions for getting this project running on Windows:

* [using `git-bash` in PyCharm's terminal][so-pycharm-git-bash]
* [setting up Anaconda for `git-bash`][so-anaconda-git-bash] — note you should `echo`
  into `~/.bash_profile` not `~/.profile`

[ezwinports]: https://sourceforge.net/projects/ezwinports/files/
[direnv-releases]: https://github.com/direnv/direnv/releases
[git-for-windows]: https://gitforwindows.org/
[git-for-windows-symbolic-links]: https://github.com/git-for-windows/git/wiki/Symbolic-Links
[so-anaconda-git-bash]: https://stackoverflow.com/a/56170202
[so-ezwinports]: https://stackoverflow.com/a/43779544
[so-pycharm-git-bash]: https://stackoverflow.com/a/20611422
