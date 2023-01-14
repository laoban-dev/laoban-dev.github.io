# Scripts cookbook

## A script that behaves differently on Windows or linux

```json
{
  "demosWindowsOrLinux": {
    "description": "shows how to run a script differently on windows or linux",
    "commands":    [
      {"command": "echo Linux ${packageDirectory}", "guard": {"value": "${os}", "equals": "Windows_NT"}},
      {"command": "echo Windows ${packageDirectory}", "guard": {"value": "${os}", "equals": "Linux"}}
    ]
  }
}
```

## A script that executes in a different directory

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

