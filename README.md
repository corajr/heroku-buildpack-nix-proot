heroku-buildpack-nix-proot
==========================

# DEPRECATION WARNING:

This repo is left up for historical interest, but is presently unmaintained. If you are interested in this approach, I suggest you check out [ocharles' fork](https://github.com/ocharles/heroku-buildpack-nix-proot) which uses Hydra in lieu of S3 for cached binaries.

----------

This buildpack installs dependencies using the [Nix](http://nixos.org/nix) package manager.
Nix has a binary cache of many dependencies (including GHC and many Haskell libraries),
with the potential to greatly accelerate the build process.

The resulting app must be run within PRoot, incurring a performance penalty.
An Amazon Web Services account with an S3 Bucket is also required, as the buildpack
must save and restore the Nix closure in order to install. (Note that for
Haskell apps using larger frameworks like Yesod, this may be over 1.5 GB;
make sure that your S3 bucket is in the same region as the dyno
to take advantage of free data transfer.)

Usage
-----

Create a Procfile something like this (run_proot.sh will be created by the build scripts):

```bash
web: run_proot.sh myapp
```

Then create your application on Heroku, setting the S3 access info as follows:

```bash
heroku create -b https://github.com/corajr/heroku-buildpack-nix-proot.git
heroku config:set NIX_S3_KEY=... \
                  NIX_S3_SECRET=... \
                  NIX_S3_BUCKET=...
```

The default behavior is to have the first `git push heroku master` set up Nix in
the build cache and create a build environment, but not to build. If you want Nix
to attempt a complete build on push, also set: 

```bash
heroku config:set NIX_BUILD_ON_PUSH=1
```

(Simply unset it to turn off. Build on push is *not recommended* for the first
push of a larger app, as it may exceed the 15-minute build limit.)

Then push the app:

```bash
git push heroku master
```

If you've specified to build on push above, nix will try to install your app.
Otherwise, run a build command on a one-off dyno like so:

```
heroku run --size=PX build
```

Then re-deploy the app:

```
git commit --amend --no-edit
git push -f heroku master
```

See https://github.com/corajr/heroku-deploy-nix-example for a trivial
complete example.

Refs
----

This approach is heavily inspired by Miëtek Bak's [Haskell on Heroku](https://haskellonheroku.com/),
simply using Nix instead of Halcyon for installation.

Credit to [Brian McKenna](https://twitter.com/puffnfresh/status/521647022135590913) for the idea of a Nix buildpack.

The Nix install script is derived from the [PRoot instructions](https://nixos.org/wiki/How_to_install_nix_in_home_(on_another_distribution)#PRoot_Installation)
on the Nix wiki.

Finally, this buildpack uses [Tim Kay's AWS tool](http://timkay.com/aws/) to communicate with S3.
