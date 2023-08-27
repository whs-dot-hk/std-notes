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
