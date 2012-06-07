pathify
=======

Shell environment helper to setup environment variables.  Inspired by virtualenv and virtualenvwrapper.

Install
-------

    cd
    git clone git://github.com/kristi/pathify.git .pathify

    # Temporary install for this shell session
    source ~/.pathify/pathify

    # Install for all future bash sessions
    echo "source ~/.pathify/pathify" >> ~/.bashrc

Usage
-----

Activate the example environment that came with pathify.
(The example just adds `/sbin` to your path)

    pathify example

After you've activated the example, your prompt should show `[example]`.

This example adds `/sbin` to your path.  You can now run commands from `/sbin` without the full path.  Example: show the shutdown help

    shutdown -h

You can view all your environment variables by running

    printenv

Deactivate the example environment

    unpathify example

### Create a new environment

We're going to create a new environment called `fluffy`

    cd ~/.pathify
    cp example fluffy
    # Hack away
    # You should probably start with a couple of search/replaces, e.g.
    # :%s/example/fluffy/g
    # :%s/EXAMPLE/FLUFFY/g
    vim fluffy

Activate your new environment

    pathify fluffy

Deactivate fluffy

    unpathify fluffy

-----

__Todo__

* create a "add new environment" command
* remember previous pathify activations so we can just call `unpathify` without any arguments to undo the last environment

__License__

GPL2 or later