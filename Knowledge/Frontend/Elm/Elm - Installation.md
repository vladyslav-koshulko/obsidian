#elm #install
# Install Instructions

The pre-compiled binary for Linux works on a very wide range of distributions.

It should be possible to install it by running the following commands in your terminal:

```bash
# Move to your Desktop so you can see what is going on easier.
#
cd ~/Desktop/

# Download the 0.19.1 binary for Linux.
#
# +-----------+----------------------+
# | FLAG      | MEANING              |
# +-----------+----------------------+
# | -L        | follow redirects     |
# | -o elm.gz | name the file elm.gz |
# +-----------+----------------------+
#
curl -L -o elm.gz https://github.com/elm/compiler/releases/download/0.19.1/binary-for-linux-64-bit.gz

# There should now be a file named `elm.gz` on your Desktop.
#
# The downloaded file is compressed to make it faster to download.
# This next command decompresses it, replacing `elm.gz` with `elm`.
#
gunzip elm.gz

# There should now be a file named `elm` on your Desktop!
#
# Every file has "permissions" about whether it can be read, written, or executed.
# So before we use this file, we need to mark this file as executable:
#
chmod +x elm

# The `elm` file is now executable. That means running `~/Desktop/elm --help`
# should work. Saying `./elm --help` works the same.
#
# But we want to be able to say `elm --help` without specifying the full file
# path every time. We can do this by moving the `elm` binary to one of the
# directories listed in your `PATH` environment variable:
#
sudo mv elm /usr/local/bin/

# Now it should be possible to run the `elm` binary just by saying its name!
#
elm --help
```


## Wait, what is the `PATH` variable?

When you run a command like `elm make src/Main.elm`, your computer starts by trying to find an executable file called `elm`.

The `PATH` is the list of directories that get searched. You can see these directories by running:

```bash
echo $PATH
```

This prints `/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin` on my computer. The directories are separated by a `:` so there are five possibilities listed here.

When I run `elm make src/Main.elm`, my terminal starts by searching these five directories for an executable file named `elm`. It finds `/usr/local/bin/elm` and then runs `/usr/local/bin/elm make src/Main.elm` with whatever arguments I gave.

So the `PATH` environment variable is a convention that allows you to refer to a specific executable file without knowing exactly where it lives on your computer. This is actually how all "terminal commands" work! Commands like `ls` are really executable files that live in directories listed in your `PATH` variable.

So the point of running `sudo mv elm /usr/local/bin/` is to turn the `elm` binary into a terminal command, allowing us to call it just like `ls` and `cd`.

**Note:** Why do we need to use `sudo` for that one command? Imagine if some program was able to add executables named `ls` or `cd` to `/usr/local/bin` that did something tricky and unexpected. That would be a security problem! Many distributions make this scenario less likely by requiring special permissions to modify the `/usr/local/bin/` directory.


## Uninstall

The following commands should remove everything:

```bash
# Remove the `elm` executable.
#
sudo rm /usr/local/bin/elm

# Remove any cached files. The files here reduce compile times when
# starting new projects and make it possible to work offline in more
# cases. No need to keep it around if you are uninstalling though!
#
rm -r ~/.elm/
```

If you have any Elm projects still on your computer, you can remove their `elm-stuff/` directories as well.

## elm init

You can start an Elm project by running:

```
elm init
```

Try running this command to create an `elm.json` file and a `src/` directory:

- [`elm.json`](https://github.com/elm/compiler/blob/master/docs/elm.json/application.md) describes your project.
- `src/` holds all of your Elm files.

Now try creating a file called `src/Main.elm` in your editor, and copying in the code from [the buttons example](https://elm-lang.org/examples/buttons).

## elm reactor

`elm reactor` helps you build Elm projects without messing with the terminal too much. You just run it at the root of your project, like this:

```
elm reactor
```

This starts a server at [`http://localhost:8000`](http://localhost:8000/). You can navigate to any Elm file and see what it looks like. Run `elm reactor`, follow the localhost link, and try to check out your `src/Main.elm` file!

## elm make

You can compile Elm code to HTML or JavaScript with commands like this:

```
# Create an index.html file that you can open in your browser.
elm make src/Main.elm

# Create an optimized JS file to embed in a custom HTML document.
elm make src/Main.elm --optimize --output=elm.js
```

Try running these commands on your `src/Main.elm` file.

This is the most general way to compile Elm code. It is extremely useful once your project becomes too advanced for `elm reactor`.

This command produces the same messages you have been seeing in the online editor and with `elm reactor`. Years of work has gone into them so far, but please report any unhelpful messages [here](https://github.com/elm/error-message-catalog/issues). I am sure there are ways to improve!

## elm install

Elm packages all live at [`package.elm-lang.org`](https://package.elm-lang.org/).

Say you look around and decide you need [`elm/http`](https://package.elm-lang.org/packages/elm/http/latest) and [`elm/json`](https://package.elm-lang.org/packages/elm/json/latest) to make some HTTP requests. You can get them set up in your project with the following commands:

```
elm install elm/http
elm install elm/json
```

This adds these dependencies into your `elm.json` file, making these packages available in your project. That will let you say `import Http` and use functions like `Http.get` in your programs.
# Связи
- [[Elm - Overview]]
- [[Elm - The Elm Architecture]]