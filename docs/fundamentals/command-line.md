# Command line basics {docsify-ignore-all}

After developing an application, you want to serve this applicaton on a server. Since most servers are typically Linux based, it's important to get familiar with these essential commands so that you are able to navigate around easily.

There may be times you might be asked to navigate a server to view the logs in order to determine the cause of a bug or you might be asked to complete a manual task that can be automated with a shell script and a cron job.

You could refer to the repository: [Learn Shell](https://github.com/thoughtworks-jumpstart/learn-shell)

## Shell

A shell is a command line user interface. Most Unix-like operating systems use Bash.

However, the default shell for macOS, since 10.15 Catalina, is Z shell. Prior versions of macOS used Bash. Windows does not have Bash, but an emulator can be installed.

- Use the `Tab` key on your keyboard to complete arguments or list available commands
- Use `CTRL` + `R` to search through previously used commands
- Use `CTRL` + `A` to move the cursor to the beginning of the line
- Use `CTRL` + `E` to move the cursor to the end of the line
- Use `CTRL` + `L` to clear the screen
- Use `CTRL` + `W` to delete a word
- Use `CTRL` + `U` to delete the entire line

Read more tips on [everyday uses for Bash](https://github.com/jlevy/the-art-of-command-line#everyday-use).

## Navigating around the terminal

### pwd

Always know which directory you are currently in with `pwd`.

Examples:

```sh
$ pwd
```

### ls

Use this tool to list files and directories

Examples:

```sh
$ ls ~/Documents
$ ls -lah
```

### cd

Use this tool to change between directories.

Examples:

```sh
$ cd ~/Documents
$ cd ..
$ cd ../..
$ cd -
```

## Reading files

### cat

View a file in its entirety with `cat`.

### head

View the top of a file with `head`.

### tail

View the end of a file with `tail`.

This program is commonly used to view logfiles. To view 'live' updates of a logfile you can append the `-f` argument like so `tail -f /path/to/my/file`.

To learn more about `tail` you can do `man tail`.

### less

`less` is a program that is used to view large files. It's more efficient when opening large files because it does not have to load the entire file to display the first page.

To learn more about `less` you can do `man less` or `less --help`.

### man

The `man` pages actually uses `less` to display its content, so learning how to navigate through pages with `less` is essential.

Unfortunately, Windows users do not have `man` pages because the emulator does not typically come with it.

To learn more about `man` you can do `man man`.

## Miscellaneous

### history

Type `history` to list previous commands used; follow up with `!n`, where `n` is the command number, to execute that command (e.g `!1234`).

```sh
$ history
123 cd ~
124 ls -lah
125 npm install

$ !125
$ npm install
```

### Linux file system and directory structure

For those of us who are curious to learn about the Linux file system and directory structure, you can watch this video.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/HbgzrKJvDRw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
