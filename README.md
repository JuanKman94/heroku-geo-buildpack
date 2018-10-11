Heroku buildpack: Geospatial Libraries
======================================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) that
vendors main geo/gis libraries like geos, proj and gdal.

We forked this from [cyberdelia/heroku-geo-buildpack](https://github.com/cyberdelia/heroku-geo-buildpack) since it's associated S3 bucket didn't have support for the `heroku-18` stack yet, and we'd like to use github's [releases](https://github.com/CheesecakeLabs/heroku-geo-buildpack/releases) to make the libs available instead of an AWS S3 bucket.

**:warning: Make sure your heroku stack matches the releases' binaries (`heroku-18`).**

Usage
-----

Example usage:

```
$ heroku buildpacks:set https://github.com/CheesecakeLabs/heroku-geo-buildpack.git
$ heroku buildpacks:add heroku/python
```

Run `heroku buildpacks` to make sure that `heroku-geo-buildpack` is added before
the language buildpacks.

```
$ heroku buildpacks
=== sushi Buildpack URLs
1. https://github.com/CheesecakeLabs/heroku-geo-buildpack.git
2. heroku/python
```

Updating
--------
 Binaries for `geos`, `gdal` and `proj` are build on the appropriate heroku stack
image using Docker & attached to a [release](https://github.com/CheesecakeLabs/heroku-geo-buildpack/releases).
 To update or rebuild these:
* Set the versions and stack environment in `support/docker-compose.yml`
* If updating the stack, also update it in `support/Dockerfile`
* *Make sure you've deleted any cached heroku docker images*
* Build with `cd support && docker-compose run geo-builder`
* Wait 20 minutes, and check the contents of your `/tmp/` folder for any `.tgz` files. Those files must be uploaded with a `manifest.<lib>` indicating the correct lib version to your [release](https://github.com/CheesecakeLabs/heroku-geo-buildpack/releases).

Testing
-------

On Django Shell:

```python
>>> from django.contrib.gis import gdal
>>> gdal.GDAL_VERSION
(2, 2, 0)
```
