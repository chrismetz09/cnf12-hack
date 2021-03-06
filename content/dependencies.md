---
title: "II. Dependencies"
url: /dependencies-talk
date: "2018-09-01"
description: "codebase stuff."
image: "img/unsplash-photos-nehfi_SfqtU.jpg"
credit: "https://unsplash.com/photos/nehfi_SfqtU"
thumbnail: "img/unsplash-photos-nehfi_SfqtU.tn-500x500.jpg"
weight: "3"
categories:
- Demo
---
### Explicitly declare and isolate dependencies

Most programming languages offer a packaging system for distributing support libraries, such as [CPAN](http://www.cpan.org/) for Perl or [Rubygems](http://rubygems.org/) for Ruby.  Libraries installed through a packaging system can be installed system-wide (known as "site packages") or scoped into the directory containing the app (known as "vendoring" or "bundling").

**A X-factor CNF never relies on implicit existence of system-wide packages.**  It declares all dependencies, completely and exactly, via a *dependency declaration* manifest.  Furthermore, it uses a *dependency isolation* tool during execution to ensure that no implicit dependencies "leak in" from the surrounding system.  The full and explicit dependency specification is applied uniformly to both production and development.

For example, [Bundler](https://bundler.io/) for Ruby offers the `Gemfile` manifest format for dependency declaration and `bundle exec` for dependency isolation.  In Python there are two separate tools for these steps -- [Pip](http://www.pip-installer.org/en/latest/) is used for declaration and [Virtualenv](http://www.virtualenv.org/en/latest/) for isolation.  Even C has [Autoconf](http://www.gnu.org/s/autoconf/) for dependency declaration, and static linking can provide dependency isolation.  No matter what the toolchain, dependency declaration and isolation must always be used together -- only one or the other is not sufficient to satisfy X-factor.

One benefit of explicit dependency declaration is that it simplifies setup for developers new to the CNF.  The new developer can check out the CNF's codebase onto their development machine, requiring only the language runtime and dependency manager installed as prerequisites.  They will be able to set up everything needed to run the CNF's code with a deterministic *build command*.  For example, the build command for Ruby/Bundler is `bundle install`, while for Clojure/[Leiningen](https://github.com/technomancy/leiningen#readme) it is `lein deps`.

X-factor CNFs also do not rely on the implicit existence of any system tools.  Examples include shelling out to ImageMagick or `curl**.  While these tools may exist on many or even most systems, there is no guarantee that they will exist on all systems where the CNF may run in the future, or whether the version found on a future system will be compatible with the CNF.  If the CNF needs to shell out to a system tool, that tool should be vendored into the CNF.

### TODO
* Provide guidance for building CNFs which require hardware such as SR-IOV.
