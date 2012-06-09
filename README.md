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

### Activate an environment

Activate the example environment that came with pathify.
(The example just adds `/sbin` to your path)

    pathify example

Pathify comes with bash and zsh tab completion so you can type

    pathify <TAB>

to see a list of pathify environments you can use.

The load command will also activate an enrionment

    pathify load example

After you've activated the example, your prompt should show `[example]`.

This example adds `/sbin` to your path.  You can now run commands from `/sbin` without the full path.  

Example: Without `/sbin` in your path, you would have to run `/sbin/shutdown`.  But now that you are in the example pathify environment, you can call shutdown without its path.  To show the shutdown help, run

    shutdown -h

You can view all your environment variables by running

    printenv

### Deactivate an environment

Deactivate the example environment

    unpathify example

Pathify will remember your previous activations, so you can call deactivate without an argument to unpathify the last loaded environment

    unpathify

Use the unload command performs the same unpathify action

    pathify unload example

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

Activate your new environment

    pathify fluffy

Deactivate fluffy

    unpathify fluffy

### Edit an environment

Pathify comes with a edit command to edit an environment

    pathify edit fluffy

You should set the environment variable `$EDITOR` to your preferred editor in your shell's config file, e.g. `~/.bashrc`), or else pathify will open vim (the author's preferred editor)

    export EDITOR="vim"

You can also manually call an editor

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

After the `PATH` section, add some new lines

    _OLD_FLUFFY_PATH="$PATH"
    PATH="/sbin:$PATH"
    export PATH

    _OLD_FLUFFY_HELLO_WORLD="$HELLO_WORLD"
    HELLO_WORLD="hiya"
    export HELLO_WORLD

#### Restore old value in the deactivation function

In the deactivation function, add some lines after the `PATH` section

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

Call `repathify` to unload and load an environment

    repathify fluffy

This is equivalent to calling

    unpathify fluffy
    pathify fluffy

`repathify` without any arguments will attempt to reload the last environment you loaded.

    repathify

Also, the reload command performs the same

    pathify reload fluffy

-----

__Todo__

* create a "add new environment variable" helper command
* fix bash completion to add commands?

__License__

GPL2 or later
