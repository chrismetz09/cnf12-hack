---
title: "XII. Admin processes"
url: /admin-processes-talk
date: "2018-09-01"
description: "codebase stuff."
image: "img/unsplash-photos-nehfi_SfqtU.jpg"
credit: "https://unsplash.com/photos/nehfi_SfqtU"
thumbnail: "img/unsplash-photos-nehfi_SfqtU.tn-500x500.jpg"
categories:
- Demo
---

### Run admin/management tasks as one-off processes

The [process formation](./concurrency) is the array of processes that are used to do the CNF's regular business (such as handling web requests) as it runs.  Separately, developers will often wish to do one-off administrative or maintenance tasks for the CNF, such as:

* Running database migrations (e.g. `manage.py migrate` in Django, `rake db:migrate` in Rails).
* Running a console (also known as a [REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop) shell) to run arbitrary code or inspect the CNF's models against the live database.  Most languages provide a REPL by running the interpreter without any arguments (e.g. `python` or `perl`) or in some cases have a separate command (e.g. `irb` for Ruby, `rails console` for Rails).
* Running one-time scripts committed into the CNF's repo (e.g. `php scripts/fix_bad_records.php`).

One-off admin processes should be run in an identical environment as the regular [long-running processes](./processes) of the CNF.  They run against a [release](./build-release-run), using the same [codebase](./codebase) and [config](./config) as any process run against that release.  Admin code must ship with application code to avoid synchronization issues.

The same [dependency isolation](./dependencies) techniques should be used on all process types.  For example, if the Ruby web process uses the command `bundle exec thin start`, then a database migration should use `bundle exec rake db:migrate`.  Likewise, a Python program using Virtualenv should use the vendored `bin/python` for running both the Tornado webserver and any `manage.py` admin processes.

In a local deploy, developers invoke one-off admin processes by a direct shell command inside the CNF's checkout directory.  In a production deploy, developers can use ssh or other remote command execution mechanism provided by that deploy's execution environment to run such a process.