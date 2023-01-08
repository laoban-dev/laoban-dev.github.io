# Guards

Quite often we want to conditionally execute scripts. There are three main reasons:

* we might want our script to work differently with linux/windows
* we might want a slightly different script depending on the package manage
* we might have different types of projects. For example we might have some javascript projects and some java projects.
  When we test we might want to execute a different command. (`npm test` for javascript and `mvn test` for java)

We can set guards at two levels: one for a whole script (i.e. only execute the `laoban publish` script if the guard
saying 'publish me' is true)
or at a command level. This last is most useful when the script has to be 'different' depending on the package
manager/operating system or some other property.

## Script level guards

A command can be set to only execute if a guard is defined. Unlike the `osGuard` and `pmGuard` this does not cause an
error message: only scripts that match are executed. Note that that we are using a [variable](VARIABLES.md) in the
guard. The example of ls-ports here:

```json
{
  "ls-ports": {
    "description": "lists the projects that have a port defined in package.details.json",
    "guard":       "${projectDetails.details.port}",
    "commands":    ["js:process.cwd()"]
  }
}
```

Another good example is

```json
    {
  "start": {
    "description": "${packageManager} start for all projects that have a port defined in package.details.json",
    "guard":       "${projectDetails.details.port}",
    "commands":    ["${packageManager} start"],
    "env":         {"PORT": "${projectDetails.details.port}"}
  }
}
```

So by setting 'ports' to a numeric value in the  `package.details.json` we have  'marked' the directory in such a way
that executing `laoban start` will start up the project. This lets us spin up multiple react projects at once. It's a
good idea if all the projects have different ports, but laoban doesn't check this

## Command level guards

A good example would be if you wanted to have a `laoban compile` and some projects were javascript, others typescript
and others java. For this you would need to add a 'language' property to the `package.details.json` file for each
project. Then a script like this would execute `tsc` if the language is typescript and `mvn compile` if the language is
java

```json
{
  "compile": {
    "description": "Compiles the project",
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

# How to specify a guard

## Guards as a string

The simplest way is as a string. The following executes the script if the data in the `package.details.json` file has
a `details.language` defined

```json
{
  "listProjectsWithALangugeSet": {
    "command": "echo ${packageDirectory}", "guard": "${packageDetails.details.language}"
  }
}
```

## Guards as an object

This is identical to defining it as a string. It's more complex so we would only do this if we wanted to do something
else

```json
{
  "listProjectsWithALangugeSet": {
    "description": "lists all the projects which have a language defined in the package.details.json file",
    "command": "echo ${packageDirectory}", "guard": {"value": "${packageDetails.details.language}"}
  }
}
```

## Guard with default value

Often we want to set a default value for the guard. For example if the guard is not set then we want it to default to
true. This example shows how we can do that

```json
{
  "defaultTrueGuard": {
    "description": "lists all the projects unless the guard is set to false",
    "commands":    ["js:process.cwd()"],
    "guard":       {"value": "${packageDetails.details.guard}", "default": true}
  }
}
```

### Guard which matches a value

Suppose we want to do something if a project has value 'A' in it's `package.details.json` file. We can do that with

```json
{
  "guardMatchingA": {
    "description": "lists all the projects with the guardValue is set to A",
    "commands":    ["js:process.cwd()"],
    "guard":       {
      "value": "${packageDetails.details.valueGuard}", "equals": "A"
    }
  }
}
```
