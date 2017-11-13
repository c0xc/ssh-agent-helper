ssh-agent-helper
================

A simple helper script that makes sure the ssh agent is running.

Some desktop environments have an ssh agent running by default.
Others don't.
Or maybe they do, but the SSH client does not find the agent
because the variables are not set for some reason (i.e., it does not work).
This helper script was written for this scenario.
It usually goes like this:
You type `ssh ...` and it asks for the passphrase.
You could start a new agent and add your key ("ssh-add"),
then you won't have to type in your passphrase again. At least in that shell.
Then you open a new terminal window, type `ssh ...` and
you're once again asked for the passphrase because
it doesn't see the running agent.
This is where this helper script comes in,
it makes sure that SSH knows about the running agent.
You won't have to enter your passphrase a second time because it's already
stored in memory by the running agent.



Functionality
-------------

Whenever you run `ssh`, this script checks if the agent has any keys loaded.
If not, it'll try to load your keys from `~/.ssh`. All of them.

When a shell session is started, this script checks the
environment variables that are used to identify the ssh agent process.
If those are not found, it starts a new agent and
saves them in a temporary file.
All shell sessions that are started afterwards use this status file
and set the variables to identify the ssh agent process.



Installation
------------

Copy the script to your home directory:

    $ cp -vi ssh-agent-start ~/bin/ssh-agent-start

Add this to your `~/.bashrc` to enable the helper script:

    # SSH agent helper
    if [ -x "$HOME/bin/ssh-agent-start" ]; then
        source "$HOME/bin/ssh-agent-start"
    fi



Notes
-----

As attentive readers of this file may have noticed,
this is a rather crude helper script.
It looks for private keys in `~/.ssh` and loads them all.

The status file is saved in `/tmp` by default.
If a directory called "tmp" exists in your $HOME,
it's saved there instead.
The status file's timestamp is updated whenever SSH is called
to make it obvious that the file is still in use;
and it won't be removed by a cleanup job that
deletes old files in ~/tmp that way.

If you set this environment variable, all keys will be loaded (again):

    SSH_AGENT_HELPER_LOAD_KEYS



Author
------

Philip Seeger (philip@philip-seeger.de)



License
-------

Please see the file called LICENSE.

