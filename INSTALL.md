<div align="center">
<p>

[Back to Main Page](./README.md)
</p>

# Installation Guide

</div>

## Table of Contents

- [Installation Guide](#installation-guide)
  - [Table of Contents](#table-of-contents)
  - [Compatibility](#compatibility)
  - [Dependencies](#dependencies)
  - [Installation](#installation)

## Compatibility

**Supported Platforms:**

- [x] Linux
- [X] Windows
- [X] MacOS
- [x] WASM

## Dependencies

You will be required to install raylib on your system to use Raylib.nelua

To obtain Raylib, you can either download it from Raylib's github page or build it yourself. - Recommended, Please follow Raylib guide on obtaining the library.

## Installation

To use Raylib.nelua and its following submodules you will have to have it located in the working directory of your code e.g:

```
MyCodeFolder:
          --> main.nelua
          --> raylib.nelua
          --> rayeasings.nelua (optional)
          --> rcamera.nelua (optional)
```


> Note:
>
> You can also use .neluacfg to setup custom locations for your libraries. [Please see the following example.](https://github.com/edubart/nelua-lang/discussions/67)`

Once successfully installed, simply you have to call it from your code by adding `require "raylib"` and run/build your code.