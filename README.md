pathify
=======

A modular shell environment helper to manage your PATH and other environment variables.  

Use pathify to setup an environment with a custom PATH so that you can run scripts without having to specify the path.

Or create a pathify environment with customised CFLAGS and LDFLAGS for a custom build environment.

Inspired by virtualenv and virtualenvwrapper.

Install
-------

    cd
    git clone git://github.com/kristi/pathify.git .pathify

    # Install for this shell session only
    source ~/.pathify/pathify

    # Install for all future bash sessions
    echo "source ~/.pathify/pathify" >> ~/.bashrc
    # (Some distros use ~/.bash_profile)
    # echo "source ~/.pathify/pathify" >> ~/.bash_profile
    
    # Install for all future zsh sessions
    echo "source ~/.pathify/pathify" >> ~/.zshrc

Usage
-----

### Load a pathify environment

Load the `example` environment that came with pathify.
(`example` just adds `/sbin` to your path)

    pathify load example

Pathify comes with bash and zsh tab completion so you can type

    pathify load <TAB>

to see a list of pathify environments you can use.

Also, as a shortcut you can omit the `load` command and pathify
assume you are loading an environment.

    pathify example

After you've activated the example, your prompt should show `[example]`.

    [example] user@host $

Since `example` added `/sbin` to your path, you can now run commands from `/sbin` without the full path.  

Example: Without `/sbin` in your path, you would have to run `/sbin/shutdown`.  But now that you are in the example pathify environment, you can call shutdown without its path.  To show the shutdown help, run

    shutdown -h

You can view all your environment variables by running

    printenv

### Unload an environment

Unload the example environment

    pathify unload example

Pathify will remember your previous loads, so you can call `unload` without an argument to unload the last loaded environment

    pathify unload

Pathify also comes with a shortcut `unpathify` which does the same thing as `pathify unload`

    unpathify example

### Create a new environment

We're going to create a new environment called `fluffy`  (You can call your environment whatever you want as long as there are no spaces in the name.)  

    pathify new fluffy

You can also manually create a new environment by creating a file in the `PATHIFY_HOME` (`~/.pathify`) folder.

    cd ~/.pathify
    cp template fluffy
    # Change variable names
    sed -i -e 's/@env@/fluffy/g' -e 's/@ENV@/FLUFFY/g' fluffy
    # Hack away
    vim fluffy

Load your new environment

    pathify fluffy

Unload fluffy

    pathify unload fluffy

### Edit an environment

Pathify comes with a edit command to edit an environment

    pathify edit fluffy

You should set the environment variable `$EDITOR` to your preferred editor in your shell's config file, e.g. `~/.bashrc`), or else pathify will open vim (the author's preferred editor)

    export EDITOR="vim"

You can also manually edit an environment

    vim ~/.pathify/fluffy

### Setting the environment's `PATH`

Suppose we want to add "~/fluffy/bin" to the fluffy environment's `PATH`.  Edit the fluffy environment and change the `PATH=` line to

    PATH="$HOME/fluffy/bin:$PATH"

Save and reload the environment

    pathify reload fluffy

### Setting other environment variables

There are two parts: setting the new value, and restoring the old value in the deactivation function.

For example, suppose we want to add a environment variable called `HELLO_WORLD`

#### Set the new value

After the `PATH` section, add some new lines to save the previous value of `HELLO_WORLD` and set the new value

    _OLD_FLUFFY_PATH="$PATH"
    PATH="/sbin:$PATH"
    export PATH

    _OLD_FLUFFY_HELLO_WORLD="$HELLO_WORLD"
    HELLO_WORLD="hiya"
    export HELLO_WORLD

#### Restore old value in the deactivation function

In the deactivation function, add some lines after the `PATH` section to restore `HELLO_WORLD` to the old value

    deactivate_fluffy () {
        # reset old environment variables
        if [ -n "$_OLD_FLUFFY_PATH" ] ; then
            PATH="$_OLD_FLUFFY_PATH"
            export PATH
            unset _OLD_FLUFFY_PATH
        fi

        if [ -n "$_OLD_FLUFFY_HELLO_WORLD" ] ; then
            HELLO_WORLD="$_OLD_FLUFFY_HELLO_WORLD"
            export HELLO_WORLD
            unset _OLD_FLUFFY_HELLO_WORLD
        fi

### Reload an environment

Use `reload` to unload and load an environment

    pathify reload fluffy

This is equivalent to calling

    pathify unload fluffy
    pathify load fluffy

`repathify` is a shortcut for `pathify reload`.  Either without any arguments will attempt to reload the last environment you loaded.

    pathify unload
    repathify


-----

__Todo__

* create a "add new environment variable" helper command
* fix bash completion to complete commands?

__License__

GPL2 or later
