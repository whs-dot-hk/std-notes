# Hive
```nix
{inputs,...}:{config,...}:
{inputs,config,...}:
```
```nix
{}:{pkgs, ...}
{config,...}:let inherit (config._module.args) pkgs;in
```
```nix
# builtins.scopedImport
let
  inherit (inputs) nixpkgs;
  inherit (cell) block;
  inherit (config._module.args) pkgs;
  inherit (options) services;
in {}
```
# Std
https://github.com/divnix/std/blob/676d8356a3c2d3bac7aa9e27508c2f6651cfbe0e/src/std/fwlib/blockTypes/pkgs.nix#L12
```nix
(my "thing" {cli = false;})
```
