# Configuring a Judge

A judge is configured using a YAML file that defines its runtimes, problem directories, and other settings.
You can refer to a sample configuration file here:
[https://github.com/huythedev/k23oj-docs/blob/master/sample_files/judge_conf.yml](https://github.com/huythedev/k23oj-docs/blob/master/sample_files/judge_conf.yml)

---

## Runtimes

Runtimes are defined under the `runtime` section.
Most supported runtimes can be automatically detected through `dmoj-autoconf` if they exist in your system’s `$PATH`.
Any runtime **not** available in `$PATH` must be configured manually in this section.

---

## Problems

The judge discovers problems through the `problem_storage_globs` field.

This is a list of glob patterns (using Python’s `glob` syntax).
Every directory matching any of the patterns **and containing an `init.yml`** is treated as a problem.

Examples:

* `/mnt/problems/folder1/*` → matches `/mnt/problems/folder1/problem1`
* `/mnt/problems/folder2/**/` → matches all nested folders under `folder2`
* `/mnt/problems/folder3/year20[0-9][0-9]` → matches `/mnt/problems/folder3/year2023`

This allows you to organize problems by folders, categories, or year.

---

## ID

The `id` field defines the judge’s display name.
It must match the name configured on the site (in `/admin/judge/`).

---

## Key

The `key` field contains the authentication key used to validate the judge’s connection to the bridge.
This must match the key shown in the Django admin interface for the same judge.