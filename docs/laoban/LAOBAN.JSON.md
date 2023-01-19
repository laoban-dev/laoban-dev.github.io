# laoban.json

This is a file that configures `laoban`. The key variable values can be seen by `laoban config`

The existence of the file marks that this is the root of a 'big project' which is composed of one or more sub projects

## Further documentation

* [Projects](PACKAGES.md)
* [Templates](TEMPLATES.md)
* [Scripts](SCRIPTS.md)

## Parents

It is rare to just have 'a single laoban.json with all the stuff'. That would probably violate the principle of 'separation of concerns'.

Instead, we have the ability to 'merge together' lots of parents. A typical
`laoban.json` file might look like this:


```json
{
  "parents":        [
    "https://raw.githubusercontent.com/phil-rice/laoban/master/common/laoban.json/core.laoban.json",
    "https://raw.githubusercontent.com/phil-rice/laoban/master/common/laoban.json/typescript.laoban.json",
    "https://raw.githubusercontent.com/phil-rice/laoban/master/common/laoban.json/typescript.publish.laoban.json"
  ]
}
```
* The `core` parent pulls in useful things that are language dependant
* The `typescript` parent pulls in scripts for compiling and testing typescript
* The `typescript.publish` parent pulls in scripts for publishing to `npmjs`

## Important values in laoban,json
Note that most of these have defaults if you include the `core` in parents.

* `templateDir`: the directory that the [templates]((documentation/TEMPLATES.md)) can be found in
* `log`: the name of the log file found in the project directories holding the log of executing the commands
* `status`: the name of the file (in each project directory) that holds the status of 'important commands' executed
* `scriptDir`: A place for bash scripts that can be accessed by the laoban commands. You can put your own scripts here
* `packageManager`: Probably `npm` or `yarn`
* `throttle` sets the maximum number of parallel activities that will be executed. The default is 0 which doesn't limit
  things

<div id="envVariableDefaults" /></div

## Environment variable defaults

Let me start with some strong advice. DO NOT STORE SECRETS IN `laoban.json` [Here is why](https://blog.gitguardian.com/secrets-credentials-api-git/).
laoban.json goes into git, and it is a bad idea to have secrets in git. Instead, use environment variables.

A lot of scripts that use secrets need an environment variable, and because of the nature of (other) configuration
tools will often crash if the environment variable is not set. So, we have a mechanism for setting
the DEFAULT value of environment variables. i.e. the value if not set in the environment.

A good example of this is in the '.npmrc' files in the typescript templates. Their content is
```text
//registry.npmjs.org/:_authToken=${NPM_TOKEN}
```

This is needed if you want to publish code. BUT it will break many npm/yarn commands if the environment variable 
is not set. To solve this we add a section to `laoban.json`. This stops the crash. [PLEASE DO NOT USE THIS FOR SETTING 
THE SECRET](https://blog.gitguardian.com/secrets-credentials-api-git/) 
```json
{
  "defaultEnv": {
    "NPM_TOKEN": ""
  }
}
```
