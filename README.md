# ndjson-env - Loads environment from ndjson files, selected by `jq ._name`

## Description

I have a tool called [pgpass-env](https://github.com/forbesmyester/psql-tools#pgpass-env) that, given a suitably commented PostgreSQL `~/.pgpass` file will allow you to easily set the environmental variables for software that doesn't support reading from that format.

Thinking about this problem, it seems to be a general problem:

 * Given a file, which somehow has a list of sets of values of environmental variables you may want to apply.
 * And a way to identify which values you wish to apply.
 * Apply those environmental variables.

This is what this software does.

## Example

The environmental variables are stored in any file as NDJSON. For example a file that sets up environmental variables for the JDK might look something like this:


    {"_name": "OPENJDK", "JAVA_HOME": "/use/local/openjdk", "PATH": "${PATH}:/usr/local/openjdk/bin" }
    {"_name": "NODEJS", "PATH": "${PATH}:node_modules/.bin" }

This could might be stored in `~/.jdk-env-vars`, if it were you could apply the `JDK111` environmental variables using the following:

```bash
. ndjson-env -f ~/.jdk-env-vars JDK111
```

Once you've ran this the `JAVA_HOME` and `CLASSPATH` environmental variables will be set up. NOTE: The `.` before the command is important, it pulls the environmental variables defined in the script into your current environment.

## Usage

```
NAME:
  ./ndjson-env - Loads environment from ndjson files, selected by `jq ._name`

USAGE:
  ./ndjson-env -f FILE [NAME_OF_ENV_SET]

GLOBAL OPTIONS:
  -f    The file to load environment sets from
  -r    Output raw (for scripting and navi)
  -h    Get (this) help

ARGUMENTS:
  NAME_OF_ENV_SET    The name of the environment to list (will list all if not specified)
```

Using the `-r` flag and the [`ndjson-env.cheat`](./ndjson-env.cheat) file in this repository allows the following usage with [navi](https://github.com/denisidoro/navi/)

```
. $(navi --best ndjson-env --print)
```

## Installation

Installation is simple with BASH:

```shell
mkdir -p ~/.local/bin && cp ./ndjson-env ~/.local/bin/ndjson-env && chmod +x ~/.local/bin/ndjson-env
```

However nowadays I use [basher](https://github.com/basherpm/basher) to insall:

```
basher install forbesmyester/ndjson-env
```

## Versions

 1.0.0 - Initial Release
 1.0.1 - 2019-09-29: Fix: Don't show an empty named config for empty lines.
