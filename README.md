# Röda Bilar - Easy library management for Röda

Bilar (a recursive acronym for _**B**ilar-**I**mportable **L**ibrary **Ar**chive_) is a package
manager designed as an easy way to install Röda libraries, including many versions of a same
library. Futhermore, it supports installation of Röda executables and programs that are provided
alongside the libraries.

The core of the Bilar system is the `require` function, that allows one to automatically select and
import the correct versions of the dependencies during the initialization of the program. When
possible, the most latest compatible version is used.

## Installation

In itself, Bilar does not need to be "installed". Just placing it to a directory is sufficient to
begin using it.

By default, the system will use the directory `$HOME/.bilar` to store its files, including the
installed packages. This behaviour can be changed by setting the `$BILAR_HOME` environment variable.

Because Bilar itself can't use `require` and the current importing system of Röda is rather
cumbersome, it is recommended to create the following shell scripts and use them instead of
directly executing the Röda interpreter. Just replace `/path/to` with whatever directories you use.

### The `bilar` command

This command is a tool that can be used to install new packages.

```sh
#!/bin/sh
java -jar /path/to/röda.jar "/path/to/bilar/tool.röd" "/path/to/bilar" "$@"
```

### The `röda` command

This command is used to invoke the Röda interpreter. It executes a bit of code that loads Bilar
before doing anything else.

```sh
#!/bin/sh
java -jar /path/to/röda.jar -e '{BILAR_PATH:="/path/to/bilar";localImport(BILAR_PATH.."/loader.röd")}' "$@"
```

### Package indices

Bilar is able to download packages from the Internet if it has been supplied a package index.

A package index that contains most bil packages created by me is located in `https://iikka.kapsi.fi/bilar/index.pmf`.

To use the index, create a file named `~/.bilar/sources.pmf` with the following content:

	# sources[]:
	https://iikka.kapsi.fi/bilar/index.pmf

## Basic usage

Bilar is a package manager that operates on packages called _bils_. One bil is a single, unitary
module, typically a library. Bilar can install these packages and import them to programs.

Bils are distributed as .tar.xz archives. Each bil package must be named
`package-name.x.y.z.bil.tar.xz`, where `x.y.z` is the version number of the package.

To install a bil, use the `bilar install` command.

	bilar install case.1.0.0.bil.tar.xz

If the sources file is present, Bilar can automatically download and install packages. First,
update the index with `bilar update`, and then install the demanded Bil package by invoking
`bilar get`.

	bilar update
	bilar get case

The `case` library contains the single function `case` that can be used to simulate
`case`/`select`/`switch` statements in Röda. That function can be imported using the `require`
function.

	{
		case := require("case")
	}
	
	function f(str) {
		case.case(str) | pull(matches, somethingElse)
		if matches("[ab]+") do
			/* ... */
		done
		if matches("[bc]+") do
			/* ... */
		done
		if somethingElse do
			/* ... */
		done
	}
