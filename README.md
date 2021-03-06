# [shellfire]

[shellfire] is a MIT licensed framework for building modular applications in POSIX-compliant shell script. It is designed to:-

* abstract away the differences (and myriad bugs) between multiple shell interpreters
* implement common idioms and functionality
* promote re-use of shell code with a modern, modular set of functions to get practical things done fast
* work effectively with structured data formats such as JSON, XML and Debian control files
* enable the development of fully standalone scripts, complete with resources (snippets)
* allow shell scripts to automatically determine and install their dependencies
* but have a minimal need to 'shell out' to binaries that might not be there - or might not behave

[shellfire] consists of a number of github repositories, called modules. Each module contains functions or resources in a specific namespace. You create a [shellfire] application by making a new repository (typically on GitHub) with a skeleton structure, and then adding the modules you need. You populate a template shell script, and then just code away. It couldn't be easier. [shellfire] scripts work straightaway from source control. When you're ready to do a release, you can use [fatten] to make a standalone script, and [swaddle] to then deploy it to GitHub releases, pages, etc as tarballs, debs, etc.

## Impatient to Get Started? There's a [tutorial]
Try the overdrive [tutorial]. You'll be up and running with your first [shellfire] application in 10 mins.

## So what's included?
In homage to Python, batteries are included. Here's the list of modules and namespaces:-

* [core], the daddy of them all. Includes such beauties as
  * Compatiblity functions, abstracting away differences and allowing things like `pushd` in all shells
  * A set of base64 decode functions which decode regardless of what binaries you've got (if any at all)
  * A dependency framework to document required binaries, set up the `PATH` and automatically install packages that contain them, so you can rely on `date` doing the same thing on Mac OS X as well as Linux, say
  * A fully-fledged command line parser, which handles long options, argumented and unargumented values, golfed arguments with no third party dependencies, help messages, version information, verbosity settings, non-options and more
  * Snippets, so you can embed data, text, here docs or even specialised binaries inside shell scripts. Important to version control data, resources and here docs separately.
  * One-liner child process clean-up
  * Terminal aware colour coded messages (which know about ANSI and tput and fallback gracefully)
  * A hierachial configuration framework, so administrator and users can have their own settings for any command line switch
	* Configuration can be broken into fragments (like Debian run-parts), so that common across environments can be checked in, and passwords kept separate
    * Insecurely permissioned files aren't loaded
    * Administrators can also prevent overrides of sensitive values
  * Validation and path testing functions
  * Indirect variable access, a boolean type, string functions (startsWith, contains, etc)
  * Trivial to use, but always secure, random and cleaned up temporary files and folders
  * Signal management
  * Arrays even for shells without them, and more!
* [build], functions for creating build scripts as well as a ready-to-use binary, `build` to drop in and make builds easy-peasy.
* [byte], functions for setting and testing bits.
* [compress], a simple consistent interface to compression, file extensions and associated MIME types.
* [configure], an additional framework for more advanced configuration. Used by [swaddle] to let users define simple configuration files for package and repository definitions.
* [cpucount], functions to help with enumerating CPU numbers and deciding on load averages.
* [curl], superb interface to curl that works with the shell. Securely wraps curl so URLs, headers and credentials don't leak in process lists or environment variables, and parses headers, etc afterwards.
* [debian], a small framework that includes a complete Debian control file parser.
* [github api], a REST interface that uses [curl], [urlencode], [jsonreader] and [jsonwriter]. Currently, only supports enough to support [swaddle], but, if you're looking for an open source project, this is one to do. A complete command line, shell based GitHub client would be a real win. And one for Linode. And Digital Ocean. And ...
* [jsonreader], a pure shell JSON reader. Raises events rather than creating an anaemic DOM of objects and lists - which has always been the _right_ way to deal with structured data... Think SAX for JSON.
* [jsonwriter], writes JSON.
* [random], a small framework to obtain random numbers and characters in shell script, using fallbacks to progressively less random sources.
* [unicode], to correctly encode code points in UTF-8 and UTF-16 (uggh).
* [urlencode], for all the myriad URL encodings possible. Includes a URI template Level 4 encoder.
* [version], a simple module to compare version numbers. Yes, I know that's normally bad, but some things (like [curl]) need it as we can't do feature detection.
* [xmlwriter], writes XML.

Of course, this is just a start. If there's something you'd like to see, code it and submit a pull request.

Additionally, there's also

* [fatten], to make standalone shell scripts
* [paths.d], which contains common locations and package names for different package managers. Used by [core]'s dependency framework as a source of information on where to find programs.
* [swaddle], to package everything up, create repositories and push to GitHub, whether as a Deb, RPM, tarball or 7z. Or more.
* [tutorial], the tutorial

## Why?
Because the shell matters, as shellshock showed us. Because the shell is powerful, but is a poorly understood programming language with too many variants and gotchas. And it needs proper constructs. And lastly, because we're fed up with having to install half-an-universe's worth of Ruby, Python and Perl to bootstrap our servers or run CI jobs\*. We like [Rob Landley's Aboriginal Linux](http://landley.net/aboriginal/). And if we want to build our own bespoke, single purpose servers, managed switches and embedded routers post-Snowden, less is more.

\* And if you've had to work with some of the backwards-is-forwards sysadmins I have had to in some strange organisations, doing it all in the shell is the only way of getting it done at all.

_PS: If we could have our time again with the syntax of POSIX shell script, we probably would. But we're stuck with it. The flip side is, it hasn't changed in 16 years, so today it works almost anywhere._

## Yeah, cool. So what uses it?

Well, not much yet, but, who knows? There's currently:-

* [swaddle], a tool for packaging, building package repositories, signing them, deploying their keys and publishing them on GitHub pages or wherever.
* [bish-bosh], a complete [MQTT](http://mqtt.org/) 3.1.1 client. Totally portable, totally scriptable, minimally dependent.
* [fatten], shellfire's own fattening tool

## You're mad. You should grow up and use Ruby, Python or Go.
We're proficient in all of them. And we've delivered some seriously hard core stuff in our time: message queue brokers that handle 1,000,000 simultaneous users in C. Postgresql network protocols in Java. Static webframeworks in Ruby. Devops automation in Python, oh, and a portfolio trading system in C#. A professional uses the language most appropriate to the problem domain. We do have beards and sandles, though.

## What platforms or shells are supported?

Code is known to run well on:-

* Linux (including as old as CentOS 5, which is pretty obsolescent)
* Mac OS X (using Homebrew where necessary)
* The BSDs (all of them)
* Cygwin
* AIX
* Solaris

We plan to support major distributions whilst there owners support them, as long as we can access without cost to the underlying technologies. We have limited interest in supporting obscure, dying or dead commercial platforms (eg HP-UX, Tru64).

We work well on POSIX shells that support a `local` keyword or alias:-

* bash (3.2+)
* bash as sh
* ash derivatives
  * dash
  * BusyBox ash
* ksh88 derivatives
  * mksh
  * pdksh
  * AIX ksh88
  * Solaries ksh88

yash is not there yet but could be if there's interest. We're not going to support ksh93 as it's just too different, and zsh, great as it is an interactive shell, is a bit hit and miss. MinGW MSYS uses bash 3.1, which mostly works, but has some terrible IFS handling bugs (in particular, this affects arrays; this can be worked around, but a generic solution handicaps all other shells).

## Original Developers
[shellfire] has been open sourced and enhanced from code previously used in-house by [stormmq](http://stormmq.com/) and [KisanHub](http://www.kisanhub.com/). The lead developer is [Raphael Cohn](https://github.com/raphaelcohn).

## Ready to get stated?
Follow the [tutorial] and you'll be up and running your first [shellfire] application in 10 mins.

[shellfire]: https://github.com/shellfire-dev "shellfire homepage"
[fatten]: https://github.com/shellfire-dev/fatten "fatten homepage"
[swaddle]: https://github.com/raphaelcohn/swaddle "Swaddle homepage"
[bish-bosh]: https://github.com/raphaelcohn/bish-bosh "bish-bosh homepage"
[core]: https://github.com/shellfire-dev/core "shellfire core module homepage"
[build]: https://github.com/shellfire-dev/build "shellfire build module homepage"
[byte]: https://github.com/shellfire-dev/byte "shellfire byte module homepage"
[compress]: https://github.com/shellfire-dev/compress "shellfire compress module homepage"
[configure]: https://github.com/shellfire-dev/configure "shellfire configure module homepage"
[cpucount]: https://github.com/shellfire-dev/cpucount "shellfire cpucount module homepage"
[curl]: https://github.com/shellfire-dev/curl "shellfire curl module homepage"
[debian]: https://github.com/shellfire-dev/debian "shellfire debian module homepage"
[github api]: https://github.com/shellfire-dev/github "shellfire github api module homepage"
[jsonreader]: https://github.com/shellfire-dev/jsonreader "shellfire jsonreader module homepage"
[jsonwriter]: https://github.com/shellfire-dev/jsonwriter "shellfire jsonwriter module homepage"
[random]: https://github.com/shellfire-dev/random "shellfire random module homepage"
[unicode]: https://github.com/shellfire-dev/unicode "shellfire unicode module homepage"
[urlencode]: https://github.com/shellfire-dev/urlencode "shellfire urlencode module homepage"
[version]: https://github.com/shellfire-dev/version "shellfire version module homepage"
[xmlwriter]: https://github.com/shellfire-dev/xmlwriter "shellfire xmlwriter module homepage"
[paths.d]: https://github.com/shellfire-dev/paths.d "shellfire paths.d path data homepage"
[tutorial]: https://github.com/shellfire-dev/tutorial "shellfire tutorial homepage"
