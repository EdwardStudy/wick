Binaries - Wick
===============

At the moment, Wick does not have a lot of commands that it can perform, but the structure is in place so other commands can be picked up from any [parent].  There is a help system and a late-initialization callback that can be performed so commands can register themselves with this help system even if the help system has not yet been loaded.


[parent]: ../doc/parents.md


[//]: # (AUTOGENERATED CONTENT START)


`WICK_DIR`
----------

Public: Location of the `wick` binary.  Available any time that `wick` is running. Absolute path.


`wick()`
--------

Public: Run wick commands.  These are provided by the other `wick-*` functions in the [`bin/`](./) folder.

* $1   - The command to execute.  If not found, prints helpful message.
* $2-@ - Passed to the command.

Returns the result of the command.


`WICK_ROOT`
-----------

Location of top-level folder where Wick formulas, functions, and other config is stored.


`wickIsValidScript()`
---------------------

Internal: Test if a given file could be a valid script that should be sourced.

* $1 - Filename to test.

Returns true on success, false when it is not a valid script.


`wickLoadLib()`
---------------

Internal: Load library functions.  Starting at the highest parent, load all scripts in lib/, then work down through all children.  Children can override a parent's functions.  It does this by sourcing the files, so let's hope that they only contain functions instead of commands to execute.

* $1 - Directory to process.

Returns true (0) when something was loaded. Returns 1 if nothing was found to load.


`wickOnLoad()`
--------------

Internal: Set an "on load" hook, which will execute once all of the libraries and binaries are loaded into Wick.

Updates the variable `onLoadCommands`, which is defined in the `wick` function.

* $@ - Command to execute when everything is loaded

Returns nothing.


`WICK_COMMANDS`
---------------

Internal: List of commands that are available in Wick.


`WICK_COMMAND_HELP`
-------------------

Internal: List of helpful messages for what each command does.


`WICK_COMMAND_FUNCTIONS`
------------------------

Internal: Function to run when the command with the same index is executed. Note:  WICK_COMMANDS[n] relates to WICK_COMMAND_FUNCTIONS[n].


`wickAddCommand()`
------------------

Public: Adds a command to the help system

* $1 - Name of command
* $2 - Function to execute
* $3 - Help description (optional)

Examples

    wickOnLoad wickAddCommand elephant "Make an elephant appear"

Returns nothing.


`wickExplorer()`
----------------

Public: Run an explorer.

* $1 - Variable name for the result
* $2 - Formula name
* $3 - Explorer name

Returns true when explorer ran successfully, false otherwise.


`wickFind()`
------------

Public: Find a file in the parent chain.  Finds the lowest descendant that contains the file because children can override parents.

* $1 - Variable name to store the result
* $2 - Name of file to find

Returns true when file is found successfully.


`WICK_FORMULA_LIST`
-------------------

Public: List of formulas to execute.  It is better to log this list instead of the one with arguments because arguments may contain sensitive information.


`WICK_FORMULA_LIST_WITH_ARGS`
-----------------------------

Public: List of formulas with their arguments.  Try to avoid logging these.


`WICK_FORMULA_COMMANDS`
-----------------------

Public: Commands to execute in order to make the formula execute.  This will only add an entry if there is a "run" script.  The command may have arguments appended so try to avoid logging these.


`wickFormula()`
---------------

Public: Mark a formula as required for a given role or as a dependency for other formulas.

* $1   - Formula to require
* $2-@ - Arguments for formula's run script

If the formula was already marked as required, this will error as long as the arguments don't match and there are arguments for this invocation. We can't do this another way because the `depends` file would already have been executed with a set of arguments.

Examples

    wickFormula test --arg=1
    wickFormula test          # Will not error
    wickFormula test --blue   # Errors

Returns true if formula was found and added to the list.


`wickHelp()`
------------

Public: Show the help messages saved with wickAddCommand.  Writes output to stdout.

Returns nothing.


`wickLoadRole()`
----------------

Public: Locates a role file in the ancestry chain and loads it.  This will provide a list of formulas to execute.

When loading the role, sets `WICK_ROLE_DIR` to be the directory of the role that is being loaded.

* $1   - Role to load
* $2-@ - Extra arguments to pass to the role script

Returns 1 if there is a problem loading the role.


`wickRun()`
-----------

Public: Runs a single role and passes additional arguments to the role.

* $1   - Role to execute
* $2-@ - Arguments to pass to the role's script.

Returns true on success, 1 if there are problems with the role.


`wickRunFormula()`
------------------

Public: This exists so arguments passed to wickRun are not passed to formulas.

* $1 - Formula command to execute, including arguments.  Already escaped.
* $2 - Number of the formula, for logging purposes.

*Escaping note:* Formulas that are passed in are already quoted and must not be quoted again if you want this to run properly.  See `wickFormula` for quoting.

Sets `WICK_FORMULA_DIR` to the directory of the currently executing formula. Sets `WICK_FORMULA_NAME` to just the name of the currently executing formula.

Returns the status code from the formula.


