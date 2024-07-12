# Scripts with Laoban

The 'whole point' of `laoban` is to allow scripts to be easily created that will execute for each [project](PACKAGES.md)
.

## Getting going

### Using run

Actually you can execute a 'command' in every [project](PACKAGES.md) already. The follow will execute the command in
quotes in each project. We are using a [variable](VARIABLES.md) `${packageDirectory}` because linux and windows have
different ways of finding the current directory but the variable works anyway

```shell
laoban run 'echo "Hello ${packageDirectory}"' 
```

This is very useful. I regularly do things like the following

```shell
laoban run 'rm -rf node_modules
laoban run 'rm -rf target' 
```

### Making our first script

Let's add a new script which we will call 'helloworld' to be traditional

Open the [`laoban.json`](LAOBAN.JSON.md) file in your editor. If it has a scripts section already, just edit it,
otherwise add the following

```json
{
  "scripts": {
    "helloWorld": {
      "description": "Our first script that echos 'hello directory' in each project",
      "commands":    ["echo Hello ${packageDirectory}"]
    }
  }
}
```

You can now execute this with `laoban helloWorld`, find it in the help with `laoban --help` and it has it's own help
section with `laoban helloWorld --help`

## What can we run

### Shell commands

The default type of command is a shell command. For example `"echo Hello ${packageDirectory}`

### Javascript commands

Arbitrary javascript commands can be executed. For example `"js:process.cwd()"`

### File commands

Since Windows and linux have different ways of deleting files, we can use a file command to delete a file.
The same is true to view a file. 

| Command | Purpose
| ----------| ---
| file:rm(<filename>) | Deletes a file
| file:rmDir(<directory>) | Deletes a directory
| file: rmLog | Deletes the log file (is special because logs are in use by laoban). Only really used by the `clean` command
| file:cat(<filename>) | Displays the contents of a file
| file:tail(<filename>,n) | Displays the last n lines of a file

## Types of commands

### Simple commands

```json
{
  "helloWorld": {
    "description": "Our first script that echos 'hello directory' in each project",
    "commands":    ["echo Hello ${packageDirectory}"]
  }
}
```

After adding this command `laoban --help` will now display the command and the description. and `laoban helloWorld` will
execute it

<a id='complexCommands'></a>

### More complex commands

```json
{
  "test": {
    "description": "runs ${packageManager} test",
    "commands":    [
      {"name": "test", "command": "${packageManager} test", "status": true}
    ]
  }
}
```

Here we can see that the command has one step with name `test`. Because status is true the step results will be visible
in the status. This is very useful when we have 'things we want to pass' in many projects. It's important that the
projects all compile, that the tests execute and that they publish successfully. If we have many projects it can be hard
to 'check the logs' and see that happened. Here the command `laoban status` will tell us about the `named steps`
and whether the last time they executed they were successful.

```
  name?: string,     // The name of the command
  command: string,   // the command that is actually executed
  status?: boolean,  // should the commands status be remembered
  eachLink?: boolean,// see below (hardly every used)
  osGuard?: string,  // Is the command only able to run in one operating system
  pmGuard?: string,  // Is the command only able to run with one package manager
  directory?: string // override the directory that the command is executed in
```

### eachLink

Some commands need to be accessed once for each link defined in the projectDetails file.

```
    "remoteLink"   : {
      "commands"       : [
        {"name": "remoteLink", "command": "${packageManager} link ${link}", "eachLink": true, "status": true},
        ]}
```

Here we can see that the command will be executed once for each link. The variable `${link}` holds the value of the link

### directory

If a command needs to run in a different directory (typically a sub directory) the directory can be set

``` 
   "install"    : {
      "commands"   : [
        {"name": "link", "command": "${packageManager} link", "status": true, "directory": "dist"},
```             

This link command is now executed in the `dist` sub directory

## Guards

Guards start very simple, but can get complex (if you want). The guards are used to prevent commands from being
executed. A good example is `publish` which should only be executed if the project is marked as being able to be
published.

Details of guards and how to use them can be found [here](GUARDS.md)

## Setting environment variables with scripts

cwd is automatically added as an environment variable to represent the current directory other environment
[variables](VARIABLES.md) can be added to scripts such as

```json
{
  "ls-pact": {
    "osGuard":     "Windows_NT",
    "description": "lists the projects with pact files in them",
    "guard":       "${projectDetails.details.packport}",
    "commands":    ["echo %PORT%  %cwd%"],
    "env":         {
      "PORT": "${projectDetails.details.packport}"
    }
  }
}
```

## inLinksOrder

Some scripts don't parallelise that well. For example if we are compiling projects, and some projects depend on other
projects then we want to compile them in 'the right order'.

Setting 'inLinksOrder' means that the links in the projectDetails are used to determine the order in which things are
executed

This can be seen using `-g | --generationPlan` as an option. This behavior can also be forced on any command by
selecting `-l, --links`

# showShell

Some commands we want to be 'quiet' if nothing is wrong. For example compile: if everything is OK we want no output.
Others are more verbose: such as test. For these we can set 'showShell:true' and the output will be shown as though 
the -s options was used




