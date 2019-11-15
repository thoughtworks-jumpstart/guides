# Command line tools {docsify-ignore-all}

Get familiar with these essential command line tools.

## Shell

A shell is a command line user interface. Most Unix-like operating systems use Bash. The default shell for macOS, since 10.15 Catalina, is Z shell.

- Use the `Tab` key on your keyboard to complete arguments or list available commands
- Use `CTRL` + `R` to search through previously used commands
- Use `CTRL` + `A` to move the cursor to the beginning of the line
- Use `CTRL` + `E` to move the cursor to the end of the line
- Use `CTRL` + `L` to clear the screen
- Use `CTRL` + `W` to delete a word
- Use `CTRL` + `U` to delete the entire line

Read more tips on [everyday uses for Bash](https://github.com/jlevy/the-art-of-command-line#everyday-use).

## Navigating around

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

### head

### tail

### less

### man

The `man` pages actually uses `less` to display its content, so learning how to navigate through pages with `less` is essential.

## Miscellaneous

### history

Type `history` to list previous commands used; follow up with `!n`, where `n` is the command number, to execute that command (e.g `!1234`)

```sh
$ history
123 cd ~
124 ls -lah
125 npm install

$ !125
$ npm install
```
