heroku-buildpack-nix-proot
==========================

This buildpack installs dependencies using the [Nix](http://nixos.org/nix) package manager.
Nix has a binary cache of many dependencies (including GHC and many Haskell libraries),
with the potential to greatly accelerate the build process.

This is a work-in-progress. The resulting app must be run within PRoot, incurring a
performance penalty. An Amazon Web Services account with an S3 Bucket is also required
to save the Nix closure between builds.

Usage
-----

Create a Procfile like this (run_proot.sh will be created by the build scripts):

```bash
web: run_proot.sh myapp
```

Then create your application on Heroku, setting the S3 access info as follows:

```bash
heroku create -b https://github.com/chrisjr/heroku-buildpack-nix-proot.git
heroku config:set NIX_S3_KEY=... \
                  NIX_S3_SECRET=... \
                  NIX_S3_BUCKET=...
```

Finally, push the app to initiate a build:

```bash
git push heroku master
```

Refs
----

* the [PRoot instructions](https://nixos.org/wiki/How_to_install_nix_in_home_(on_another_distribution)#PRoot_Installation)
on the Nix wiki.
* [Tim Kay's AWS tool](http://timkay.com/aws/)

Credit to [Brian McKenna](https://twitter.com/puffnfresh/status/521647022135590913) for the idea.