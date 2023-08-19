# Crate Universe `serde_derive` Bug

Running:

```bash
bazel build //serde_dorive:main
```

works on OSX but fails on Linux:

```
error: proc-macro derive panicked
 --> serde_derive/src/main.rs:3:10
  |
3 | #[derive(Serialize, Deserialize, Debug)]
  |          ^^^^^^^^^
  |
  = help: message: file missing from serde_derive manifest directory during macro expansion: /home/ttiurani/.cache/bazel/_bazel_ttiurani/22abf7e408f71b2ff7508bf4d49c9362/sandbox/linux-sandbox/3/execroot/failure/external/crate_index_serde_derive__serde_derive-1.0.183/serde_derive-x86_64-unknown-linux-gnu
```

This happens despite the merge of PR: https://github.com/bazelbuild/rules_rust/pull/2097

Issue: https://github.com/bazelbuild/rules_rust/issues/2071
