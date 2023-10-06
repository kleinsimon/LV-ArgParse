# LV-ArgParse
 A parser for command line arguments of compiled LabVIEW applications. Based on syntax and usage of the python argparse package.

Supports generated help messages and version info
```
>start /b /wait Application.exe -h

Argparse Test
usage: Application [-cfg [Path]] [-h] [-m [String]] [-n] [-v] [message]

This application demonstrates the abilities of LV ArgParse

positional arguments:
 message             Message to show

options:
 -cfg, --config      Config Path (default: .\config.ini)
 -h, --help          Display this help message
 -m, --mode          Mode to use. Choices: dummy,mockup (default: dummy)
 -n, --number        Version Numbers
 -v, --version       Show Version
```

* Supports optional, required and positional parameters.
* Supports --option value, --option=value and --option="long value" notation
* Supports handling of basic data types: string, path, int64, uint64, double, date

# Usage
## Parsing
Just call the setup method with the definitions of the arguments you would like to provide.
![Setup](https://github.com/kleinsimon/LV-ArgParse/assets/4790227/60821c86-d692-4f9a-8158-a06307f55ab9)

Parameters:
* name: The id of the argument. Used for the help message and for identification when reading values. If no short or long id is given, the argument is positional.
* short: The argument for -arg notation. Optional
* long: The argument for --argument notation. Optional
* action: What to do with values.
  * store: One single value, if provided.
  * store-const: Store the configured value, if the switch is provided
  * store-true: Store a true, if the switch is provided
  * store-false: Store a false, if the switch is provided
  * append: Add multiple values for one parameter to an array
  * append-const: Add the configured value to an array for every time a switch was provided
  * count: Count the number of times, an argument was provided
  * help: The help tag. Defaults to -h and --help
  * version: The version tag. Defaults to -v and --version
* choices: A set of allowed values. If given, wrong parameters raise an error.
* const: The constant value for the const actions.
* default: A default value. If provided, the parser holds this value even if the parameter was not provided.
* dest: A alternative name to access the data.
* help: The description of the argument to show in the help text.
* number: The number of times, an argument can be given.
  * 0, One or None: The argument may either be provided one time or omitted.
  * -1, Any: The argument may be provided any number of times or omitted.
  * -2, At least one: The argument must be provided one time or any number of times.
* required: If true, an error is raised if the argument is not provided.
* type: The type of the value.

## Access
Arguments are read using their name (or dest). The parsed state is stored in an action-engine and is globally available. 
![Access](https://github.com/kleinsimon/LV-ArgParse/assets/4790227/91462fd0-a0ba-4857-8c72-42d088d47259)

Methods exists for variant and typed data.

# Compilation
To make the arguments available to the compiled application, the option "pass all command line arguments to application" has to be activated under "advanced" for the target.

# Calling
The exe can be called in the known way:  
>Application.exe -arg --long-arg --long-arg="bla" pos-arg1

Due to the way the runtime-engine starts, the console is not blocked during the run. A workaround is to use the windows start command:
>start /b /wait Application.exe -h
