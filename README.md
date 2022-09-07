# CSCI 1100 Gateway to Computer Science

### Fall 2022

---

## Week 2: Working with Unix

### 1 The Unix Command Line

Unix is a computer operating system that exists in many variants, most notably Linux and (loosely) MacOS. All of these Unix-based systems have one thing in common - the Unix *command line interpreter* (CLI), which is the foundation of these modern operating systems.

The Unix CLI, called “Terminal” on Macs, is a text-based user interface as opposed to a graphical user interface (buttons, symbolic icons, etc). Although the CLI can seem “hacker”-ish or intimidating, it’s safe to think of it as simply an alternative version of a graphical user interface (GUI). Instead of clicking on icons to launch programs, you type the name of the program you want to launch. Instead of navigating through folders by clicking on folder icons, we use text commands like cd and ls. Practically everything you do in a GUI and more is doable in a CLI, from opening and downloading files, editing text, checking the time, or even browsing the web or playing games. In fact, there are tasks that an average computer user will occasionally come across that can be done more efficiently with a CLI and its tools.

### 2 Getting Started

When you first open the terminal, a natural response is “help!”, and if you type that, you’ve just tried your first Unix program:

```bash
help
```

As just a sample of other programs you can run, try the commands `date` and `cal`. You can also run an OCaml shell by typing `ocaml` (`Ctrl+D` to quit). You can also use the `man` command to access the manual of any command:

```bash
man ls
```

Press `q` to exit any manual.

**Paths**

A basic concept is that of a **file path** -- a sequence of directory (aka folder) names separated by a forward slash `/` on Unix and a backward slash `\` on Windows. The last (rightmost) symbol in the path may be a directory or it may be a non-directory filename. For example, `/usr/local/bin/` is a file path starting with a root `/`, then going down into directory `usr/` then down into directory `local/` etc. The `bin` abbreviates "binary", this is a folder containing binary executable programs.

All CLI programs are run by typing in the name of the program, followed by `Enter`. Many programs, in addition, take arguments, just like functions in your favorite programming language - the `man` command, for example, takes in the name of a program whose manual you want to access (indeed, you can also run man man). Argument(s) are provided after the initial program, and are separated by spaces. Arguments are akin to, for example, specifying a file to open in a text editor, and you’ll find they are quite similar to OCaml and Python function arguments as well.

Many programs will also have “flags”. These are options that further specify the behavior of a program, and are typically specified by another name preceded by a dash or two, such as `-f` or `--name`. Command line flags are very similar to toggling options on and off in GUI-based programs, such as changing the display font and size of Microsoft Word.

### 3 Navigation

The most important skill in the Unix command line is moving around.

- `pwd` - **Print Working Directory**. This shows where you are in your filesystem.

- `ls path` - Lists the contents of directory `path`. If `path` is not specified (i.e. you just type in `ls`), this lists the contents of your *current working directory*.

- `cd path` - **Change Directory**. This command takes an argument `path`, which is the name of the folder you would like to navigate into. You can provide this as an absolute path (e.g. `/home/username/Documents`) or a relative path (e.g. the name of a folder inside your current directory). For example, if you’re located inside a directory, and there is a folder called `Pictures` inside your current directory, the command `cd Pictures` will move you to that directory.

  - –  The command `cd ..` will take you up one directory. In general, the string `..` refers to the directory one level up, and the string `.` refers to the current directory.

  - –  Note: Almost everything in the Terminal is case-sensitive. If there are spaces in your folder names, you will either need to wrap the name of your folder in quotes (e.g. `cd "My Pictures"`) or use a backslash to “escape” the space, as in

    ```bash
    cd My\ Pictures
    ```

**Exercise: Getting Ready** Navigate to the `Home` directory inside the `harness` folder in this lab. Once there, list the contents of the directory.

### 4 Files

+ `cp path1 path2` - copies the contents of file `path1` into a new filename or location `path2`. This will overwrite any existing file located at `path2`!
  + `cp image1.jpg /pathto/pics` - copies the file `image1.jpg` into the folder Pictures (so that there will be another `image1.jpg` located in the directory `Pictures` Note that you need to provide CLI with the entire path to the file you want stuff copied to, so it might be a good idea to first type `pwd` (print working directory) at the destination, copy path name and save for later, and then paste the result into where `path2` stands.
  + `cp image1.jpg image1_copy.jpg` - makes a copy of the file `image1.jpg` called `image1_copy.jpg` in the same directory. Note that you can change the name of `path2` to make a copy of the same file with a different name.

+ `mv path1 path2` - moves the file `path1` into a new filename or location `path2`. This command is the same as `cp` except the original file no longer exists. This command is also used to *rename* files, by simply moving a file within one directory, e.g.

  ```bash
  mv oldname.txt newname.txt
  ```

  will overwrite any existing file located at `path2`!

+ `rm path` - removes the file `path`. This does not work for directories unless the `-r` (recursive) flag is specified. Be careful with this command! Unlike deleting files in most GUIs, there is no trash can or recycle bin that stores deleted files. Files removed this way are lost permanently.
  
+ A common idiom is `rm -r path` to remove a directory and all of its files, as well as `rm *` to remove all files in the current directory. Again, be careful with these!
  
+ `mkdir path` and `rmdir path` - makes and removes directories, respectively. `rmdir` only works if the directory is empty, i.e. there are no files in it. Thus, it may be necessary to remove the files inside `path` with `rm` first.

**Exercise: Tidying Up** Time to organize your `Home` folder. Make a `Pictures` directory inside `Home`, and move the image `BC1.jpg` there. Because you love Boston College so much, make two more copies of `BC1.jpg` called `BC2.jpg` and `BC3.jpg` in the same folder. Last, the folder junk contains some spam email. Try getting rid of it.

> **Tab Completion, Wildcards** Most systems support a handy feature, where if you begin to type the name of file/directory/command and press the `tab` key,, so long as it is unambiguous as to what file you are referring to, the CLI will automatically complete the name for you. For example, typing `oca<TAB>` will autocomplete your command to `ocaml`. Similarly, typing `cd Pic<TAB>` when in your `Home` folder will autocomplete your command to `cd Pictures`.

### 5 OCaml’s Build Manager dune

For our problemsets we’ll be using the `dune` command. What does the `dune` command do? Well, `dune` is a *build manager*, it manages the compilation, linking and execution of different software projects. When `dune` is run, as in

```bash
dune exec bin/main.exe
```

it searches for a file named `dune` in the `bin` directory. These files will always be provided for you. The `dune` file contains instructions for how to compile, link and run your project.

OCaml can be run in two ways; in an interpreted mode, where code is sent to an interactive *read-eval-print loop* (REPL) and evaluated, and a compiled mode, where code is compiled into a machine level executable file. Speaking generally, this executable file is no longer OCaml code; it is a kind of universal machine code that can be run on any computer even if OCaml is not installed.

When we issue the `dune exec bin/main.exe` command, we are essentially instructing the system to run the executable file in the current directory. This distinguishes it from other executable files named main.exe that may come preinstalled on your system.

### 6 File IO

Besides text editing and file compilation, another useful application for CLIs in the context of CS is modifying text files.

- `less path` - A file viewer that displays the contents of file `path`. In this viewer, typing the character `j` or

  `k`, or the arrow keys, page up and down. `q` quits.

- `cat path1 path2 path3...` - Short for *concatenate*, `cat` combines all of the files supplied as arguments into one file and outputs the result. However, it is also commonly used for displaying the contents of one text file, if only one file is specified, such as `cat essay.txt`.

  If no files are specified, cat will output whatever you manually input into the terminal. Although this seems useless, it will actually come in handy for several applications.

- `grep text path1 path2 path3...` - Searches for the text string `text` in any of the files `path1, path2, path3, ...`. For example,

  ```bash
  grep pear fruitlist.txt
  ```

   searches for occurrences of the word "pear" in the file `fruitlist.txt`. If no files are specified, `grep` will search whatever text you input manually into the terminal. This will also become useful later on.

- `find path -name symbol` - The most common usage of `find`, this command searches the directory `path` for files with the name `symbol`. It's common to use wildcards `*` when searching for names. For example, 

  ```bash
  find . -name "*.txt" 
  ```

  searches for all files ending in `.txt` in the current directory and subdirectories.

- `wc TEXTFILE` - The `wc` command counts the number of lines, words, and characters in the given text file. You can also have it output only one of the statistics with any of the flags `-l`, `-w`, and `-m`, respectively. For example,

  ```bash
  wc -w gettysburg.txt
  ```

  will output the number of words in `gettysburg.txt`. Once again, if no files are specified, `wc` will count the words you manually type into the terminal.

- `sort TEXTFILE` - Sorts the given` TEXTFILE` in ascending alphabetical order by line, and outputs the result. The flag `-n` sorts numerically. For example, 

  ```bash
  sort fruitlist.txt
  ```

  will sort the list of fruits in the file by name.

- `curl URI` - Retrieves the file located at the given internet *Uniform Resource Identifier* URI and outputs the result. In essence, this “downloads” a file.

**Exercise: Looking for Stuff**

- The text file `my_languages.txt` contains all of the programming languages you know. Do you know *Lisp*? Try using both a file viewing method (`less` or `cat`) with the searching method (`grep`).

- How many fruits are listed in `fruitlist.txt`?

- Try displaying the contents of `BC1.jpg`. Is it what you expected?

+ Like baseball? If you have a favorite baseball player, try searching for him in the `Players.csv` file in the `Baseball` directory. Search for the string *FirstName, LastName*, as that is how the name is organized in the file. If you don’t have a favorite player, try David Ortiz. To see what all of the fields in the row mean, look at the first line of `Players.csv`, which tells you what each column means.
+ According to `2013salaries.csv`, who was the highest-paid baseball player in 2013? The file `2013salaries.csv` only contains baseball reference IDs - if you don’t recognize the name, you may need to search for the specific ID in `Players.csv`.

### 7 Input/Output Redirection

This is where stuff gets really interesting. Unix commands generally take some kind of input, and produce some kind of output. We can control where the commands get their input and output using *I/O Redirection*. Although this discussion can get technical, a very basic explanation of this is that many commands’ output can be redirected to other places besides just spitting out text in the terminal. Most notably, we can give the output of one command into a new file, or the input of another command. We have several tools available at our disposal to do this.

This is also the explanation to the initially odd default behavior of commands like `cat` and `grep`. Using these operators, we can feed in input to these commands without having to specify a specific file.

- The `>` character is the *output redirection operator*. After writing a command, we can use `>` followed by a filename to feed the output of a command into the file. For example,

  ```bash
  curl URI > path
  ```

  will retreive the file located at the given `URI` and save it or overwrite into a new file `path`.

  ```bash
  grep pear fruitlist.txt > pearlist.txt
  ```

  will grab all of the lines grep finds with the string "pear", but instead of outputting back to the terminal, the content will be stored in a new file `pearlist.txt`.

- The `<` character is the *input redirection operator*. It allows many programs to take in the contents of files as input. This is perhaps less useful on its own, since many commands can take files directly. But, there are still many uses for it.

- The `|` character is the *pipe operator*. It essentially combines `<` and `>`. Basically, `command1 | command2` runs `command1` and then feeds the output of that command as the input of `command2`. For example,

  ```bash
  grep pear fruitlist.txt | wc -l
  ```

  will output the number of lines found in `fruitlist.txt` that contain the word "pear". This is essentially a combined version of creating `pearlist.txt` as above and then running `wc -l pearlist.txt`.

  ``` bash
  help | grep if
  ```
  
  makes `grep` search for `if`, showing the syntax of *if statements* in the command line.

+ The symbol `>>` is the *append operator*. It appends the output of any program onto the end of any file. For example,

  ```bash
  cat >> file.txt
  ```

  is a common idiom. Since `cat` outputs whatever input you give it, running this command, then manually typing in some text and then pressing `Ctrl+D` to end input will add the inputted text onto the end of `file.txt`.

**Bonus Exercises**

- In `2013Salaries.txt`, which Boston Red Sox (BOS) baseball players had the greatest and smallest salary in 2013? Can you use redirection to find this in one command? You may need to use the `sort -n` flag for numerical sort.
- Create a file named `AlexPlayers.csv` which contains all of the players and their statistics from `Players.csv` with first or last names containing the string Alex. Out of these players with Alex in their name, determine how many of them were born or had passed away in 1962.
- **Free Audiobooks** MacOS users have a cool program called `say`, which, well, says anything you give it. You’re tired of paying ridiculous amounts of money for eBooks (let alone real textbooks) so you’ve decided to take matters into your own hands. Find a way to get a free eBook from a publicly-available source like [Project Gutenberg](http://www.gutenberg.org). Use the `say` command, to recite the Gettysburg Address.
