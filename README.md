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
```sh
nixos-generate --flake .#repo-larva -f install-iso
writedisk /nix/store/*-bootstrap-*-linux.iso/iso/bootstrap-*-linux.iso
nixos-anywhere --flake .#lina-lavinox root@lavinox
```
```nix
# iso
let
  inherit (inputs.hive) bootstrap;
in {
  larva = {
    bee.system = "x86_64-linux";
    bee.pkgs = inputs.nixos.legacyPackages;
    imports = [bootstrap.profiles.bootstrap];
    nix.registry.hive.flake = {inherit (inputs.self) outPath;}; # only handy if you want to execute `disko` from the usb live stick when no network access can be procured
  };
}
```
```nix
# devshell
      imports = [bootstrap.shell.bootstrap];
```
# Std
https://github.com/divnix/std/blob/676d8356a3c2d3bac7aa9e27508c2f6651cfbe0e/src/std/fwlib/blockTypes/pkgs.nix#L12
```nix
(my "thing" {cli = false;})
```
```yaml
  deployments:
    needs: [discover]
    name: Perpare deployment data
    if: fromJSON(needs.discover.outputs.hits).deployments.apply != '{}'
    steps:
      - name: Group deployment
        id: group
        shell: bash
        run: |
          dev-preview="$(jq 'map(select(.name == \"dev-preview\"))' <<< ${{ toJSON(fromJSON(needs.discover.outputs.hits).deployments.apply) }})"
          dev-preprod="$(jq 'map(select(.name == \"dev-preprod\"))' <<< ${{ toJSON(fromJSON(needs.discover.outputs.hits).deployments.apply) }})"
          printf "%s\n" \
            "dev-preview=$dev-preview" \
            "dev-preprod=$dev-preprod" \
              >> "$GITHUB_OUTPUT"
```
```yaml
-        target: ${{ fromJSON(needs.discover.outputs.hits).deployments.apply }}
+        target: ${{ fromJSON(needs.deployments.outputs.dev-preview) }}
```
```nix
  cog = (std.lib.dev.mkNixago std.data.configs.cog) {
    data = {
      changelog = {
        remote = "github.com";
        repository = "hive";
        owner = "divnix";
      };
      post_bump_hooks = dmerge.append [
        "echo Go to and post: https://discourse.nixos.org/..."
      ];
    };
  };
```
