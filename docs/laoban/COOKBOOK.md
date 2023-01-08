# Cookbook

Here are some examples of how to make scripts: either adhoc with 'run' or by modifying `laoban.json`

* [Execute a script if a guard is true](#guardTrue)
* [Execute a script depending on the value of a guard](#guardValue)
* [Execute a script which has a default of 'true'](#guardDefault)
* [ahdoc scripts](#adhoc)

<div id="guardTrue"></div>
# Execute a script if a guard is true

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
    "compile-noguard": {
      "description": "runs compile without a guard",
      "commands":    [{"name": "tsc", "command": "tsc --noEmit false --outDir dist", "status": true}]
    },
    "ls-compile":      {
      "description": "lists projects that will be compiled",
      "guard":       "${packageDetails.details.compile}",
      "commands":    ["js:process.cwd()"]
    }
```

# Execute a script differently depending on a guard value <a name="guardValue"></a>

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

# Execute a script with a default value<a name="guardDefault"></a>

For example we want to test projects unless the project has `test` set to false. Thus if it's not defined the tests will
still run

```json
    {
  "test": {
    "description": "runs ${packageManager} test",
    "guard":       {"value": "${projectDetails.details.test}", "default": true},
    "commands":    [{"name": "test", "command": "${packageManager} test", "status": true}]
  }}
```

# Adhoc<a name="adhoc"></a>

```shell
| Purpose | Command |
| --- | --- |
| Copy a license file to each project | laoban run 'cp ${laobanDirectory}/LICENSE.md ${packageDirectory}' |
| remove the dist directories  | laoban run 'rm -rf dist' |
| remove the target directories  | laoban run 'rm -rf dist' |
| remove node_modules directories  | laoban run 'rm -rf node_modules' |
