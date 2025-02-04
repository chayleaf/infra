We provide machines as public builders for the nix community.

`x86_64-linux`

```
build-box.nix-community.org ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIElIQ54qAy7Dh63rBudYKdbzJHrrbrrMXLYl7Pkmk88H
```

`aarch64-darwin`, `x86_64-darwin`

```
darwin-build-box.nix-community.org ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKX7W1ztzAtVXT+NBMITU+JLXcIE5HTEOd7Q3fQNu80S
```

#### Access

If you want access read the security guide lines on [aarch64-build-box](https://github.com/nix-community/aarch64-build-box). Than add your username to [`nixos/community-builder/users.nix`](https://github.com/nix-community/infra/blob/master/modules/nixos/community-builder/users.nix) or [`darwin/community-builder/users.nix`](https://github.com/nix-community/infra/blob/master/modules/darwin/community-builder/users.nix) Don't keep any important data in your home! We will regularly delete `/home` without further notice.

#### Using your NixOS home-manager configuration on the hosts

If you happen to have your NixOS & home-manager configurations intertwined but you'd like your familiar environment on our infrastructure you can evaluate `pkgs.writeShellScript "hm-activate" config.systemd.services.home-manager-<yourusername>.serviceConfig.ExecStart` from your NixOS configuration, and send this derivation to be realized remotely: (in case you aren't a Nix trusted user)

```console
# somehow get the .drv of the above expression into $path
$ nix copy --to ssh://build-box.nix-community.org --derivation $path
$ ssh build-box.nix-community.org
$ nix-store -r $path
$ $path
```

_(My [implementation](https://github.com/ckiee/nixfiles/blob/aac57f56e417e31f00fd495d8a30fb399ecbc19b/deploy/hm-only.nix#L10) of [this](https://github.com/ckiee/nixfiles/blob/aac57f56e417e31f00fd495d8a30fb399ecbc19b/bin/c#L92-L95) ~ckie)_
