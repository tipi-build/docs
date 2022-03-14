---
title: Project configuration
order: 4
---

## How to create a project configuration ?

you can define the configuration of a project in the file : `.tipi/deps`

The syntax of the configuration is the same as for the dependencies. Also you can add dependencies after the configuration or in the require part of the configuration.

configuration example:  

```json
{
    "s" : ["<src-disambiguation>", ...], 
    "x" : ["<excluded-directory>", ...],
    "u" : <use-cmakelists::Boolean>,
    "packages": ["<Package Config name>", ...],
    "targets": ["<target name>", ...], 
    "find_mode": "",
    "requires" : { ... }
}
```
> All project configuration attributes are optional and can be ommitted.