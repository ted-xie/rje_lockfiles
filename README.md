# WORKSPACE build + bzlmod Maven lockfile doesn't work

## Problem description

I need to support both WORKSPACE and bzlmod builds for my Bazel rules.
My codebase depends on a Maven install through rules_jvm_external. I am using version **6.1**.

I would prefer to have the MODULE.bazel list of artifacts be the source of truth for the Maven install.
Ideally, I would do `bazel run --enable_bzlmod @maven//:pin` to generate this lockfile, and then have
`lock_file = //:maven_install.json` in MODULE.bazel and `maven_install_json = //:maven_install.json` in WORKSPACE.

However, if I set up my repo this way, generate the lockfile with `--enable_bzlmod` and run `bazel build @maven//...`, I get this error:

```
ERROR: no such package '@@com_google_guava_guava_testlib_31_1_jre//file`
```

If I reverse this configuration (i.e. generate the lockfile `--noenable_bzlmod`), then the build succeeds.

### Further details

If I generate Maven lockfiles with bzlmod enabled and disabled (see maven_install_bzlmod.json and maven_install_nobzlmod.json), we can see that the files actually differ. It appears that the one generated in WORKSPACE mod is a superset of the one generated from bzlmod.

### Expectation

I would expect that the lockfile generation works identically between WORKSPACE and bzlmod mode. 
