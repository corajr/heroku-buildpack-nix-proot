heroku-buildpack-nix-proot
==========================

This buildpack installs dependencies using the [Nix](http://nixos.org/nix) package manager.
Nix has a binary cache of many dependencies (including GHC and many Haskell libraries),
with the potential to greatly accelerate the build process.

This is a work-in-progress. Presently the resulting app must be run within PRoot, incurring a performance penalty.
Additionally, the Nix store for a barebones Yesod application is larger than 1.5G. Some form of
cloud storage such as Amazon S3 may be needed to make this useful for Haskell deployment.

Usage
-----

```bash
heroku create -b https://github.com/chrisjr/heroku-buildpack-nix-proot.git
```

Refs
----

These scripts make use of the [PRoot instructions](https://nixos.org/wiki/How_to_install_nix_in_home_(on_another_distribution)#PRoot_Installation)
on the Nix wiki.

Credit to [Brian McKenna](https://twitter.com/puffnfresh/status/521647022135590913) for the idea.