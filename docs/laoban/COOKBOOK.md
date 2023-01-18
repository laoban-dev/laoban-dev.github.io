# Cookbook

Here are some examples of how to make scripts: either adhoc with 'run' or by modifying `laoban.json`

* [Execute a command if a guard is true](#guardTrue)
* [Execute a command in a different directory](#differentDirectory)
* [Execute a command depending on the value of a guard](#guardValue)
* [Execute a command which has a default of 'true'](#guardDefault)
* [Execute a command which behaves differently on windows or linux](#guardOs)
* [Execute a command which behaves depending on the packageManager](#packageManager)
* [ahdoc scripts](#adhoc)

<div id="guardTrue"></div>

# Execute a command if a guard is true

For example compile

```json
{
  "compile": {
    "description": "Compiles the projects",
    "guard":       {"value": "${packageDetails.details.compile}", "default": true},
    "commands":    [{"name": "tsc", "command": "tsc --noEmit false --outDir dist", "status": true}]
  }
}
```

When I make a command like this I often also include

```shell
    "ls-compile":      {
      "description": "lists projects that will be compiled",
      "guard":       "${packageDetails.details.compile}",
      "commands":    ["js:process.cwd()"]
    }
```

<div name="differentDirectory"></div>

## A command that executes in a different directory

Here we have a command that is executed in the 'dist' subdirectory of each package
```json
{
  "demosRunningInADifferentDirectory": {
    "description": "shows how to run a script in a different directory",
    "commands":    [
      {"command": "js:process.cwd()", "directory": "dist"}
    ]
  }
}
```

<div name="guardValue"></div>

# Execute a command differently depending on a guard value

This depending on the language defined in the `package.details.json` file. If the language is not defined then nothing
will be executed

```json
{
  "compile": {
    "description": "Compiles the project (either java or typescript)",
    "commands":    [
      {
        "name":  "compile", "command": "tsc", "status": true,
        "guard": {"value": "${packageDetails.details.language}", "equals": "typescript"}
      },
      {
        "name":  "compile", "command": "mvn compile", "status": true,
        "guard": {"value": "${packageDetails.details.language}", "equals": "java"}
      }
    ]
  }
}
```

<div name="guardDefault"></div>

# Execute a command with a default value

For example we want to test projects unless the project has `test` set to false. Thus if it's not defined the tests will
still run

```json
    {
  "test": {
    "description": "runs ${packageManager} test",
    "guard":       {"value": "${projectDetails.details.test}", "default": true},
    "commands":    [{"name": "test", "command": "${packageManager} test", "status": true}]
  }
}
```

<div name="guardOs"></div>

# Execute a command differently on windows or linux

For example we want to run a script differently on windows or linux.

```json
{
  "clean": {
    "description": "Compiles the project (either java or typescript)",
    "commands":    [
      {
        "name":  "compile", "command": "echo Linux ${packageDirectory}", "status": true,
        "guard": {"value": "${os}", "equals": "Windows_NT"}
      },
      {
        "name":  "compile", "command": "echo Windows ${packageDirectory}", "status": true,
        "guard": {"value": "${os}", "equals": "Linux"}
      }
    ]
  }
}
```

<div name="packageManager"></div>

# Execute a command depending on the packageManager

A good example here is the `npm install` command which on yarn is just `yarn`. There is no real point doing yarn in each
package if you are using workspaces, but not everyone does.

```json
{
  "install": {
    "description": "Installs the project",
    "commands":    [
      {
        "name":  "install", "command": "yarn", "status": true,
        "guard": {"value": "${packageManager}", "equals": "yarn"}
      },
      {
        "name":  "install", "command": "npm install", "status": true,
        "guard": {"value": "${packageManager}", "equals": "npm"}
      }
    ]
  }
}
```

<div name="adhoc"></div>

# Adhoc

| Purpose | Command |
| --- | --- |
| Copy a license file to each project | `laoban run 'cp ${laobanDirectory}/LICENSE.md ${packageDirectory}'` |
| remove the dist directories  | `laoban run 'file:rmDir(dist)'` |
| remove the target directories  | `laoban run 'file:rmDir(target)'` |
| remove node_modules directories  | `laoban run 'file:rmDir(node_modules)'` |
