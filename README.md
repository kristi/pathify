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

Activate the example environment
(The example just adds /sbin to your path)

    pathify example

Deactivate the example environment

    unpathify example

Create a new environment

    cd ~/.pathify
    cp example fluffy
    # Hack away
    vim fluffy

Activate your new environment

    pathify fluffy

Deactivate fluffy

    unpathify fluffy
