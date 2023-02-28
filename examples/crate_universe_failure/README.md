# Crate Universe Missing `crate_features` Bug

Running:

```bash
cargo generate-lockfile && CARGO_BAZEL_REPIN=true bazel build //aead_dep:aead_dep
```

causes `unresolved import 'crypto_common::rand_core'`. This is because the file
`external/crate_index_crypto_common_failure__crypto-common-0.1.6/BUILD.bazel`
has invalid `crate_features`:

```starlark
rust_library(
    name = "crypto_common",
    deps = [
        "@crate_index_crypto_common_failure__generic-array-0.14.6//:generic_array",
        "@crate_index_crypto_common_failure__rand_core-0.6.4//:rand_core",
        "@crate_index_crypto_common_failure__typenum-1.16.0//:typenum",
    ],
    compile_data = glob(
        include = ["**"],
        exclude = [
            "**/* *",
            "BUILD",
            "BUILD.bazel",
            "WORKSPACE",
            "WORKSPACE.bazel",
        ],
    ),
    crate_features = [
        "std",
    ],
    crate_root = "src/lib.rs",
    edition = "2018",
    rustc_flags = ["--cap-lints=allow"],
    srcs = glob(["**/*.rs"]),
    tags = [
        "cargo-bazel",
        "crate-name=crypto-common",
        "manual",
        "noclippy",
        "norustfmt",
    ],
    version = "0.1.6",
)
```

Now if the `age_dep` crate is removed from WORKSPACE.bazel and Cargo.toml and the
above command is run again, the build passes, because `crate_features` are correct:

```starlark
rust_library(
    name = "crypto_common",
    deps = [
        "@crate_index_crypto_common_failure__generic-array-0.14.6//:generic_array",
        "@crate_index_crypto_common_failure__rand_core-0.6.4//:rand_core",
        "@crate_index_crypto_common_failure__typenum-1.16.0//:typenum",
    ],
    compile_data = glob(
        include = ["**"],
        exclude = [
            "**/* *",
            "BUILD",
            "BUILD.bazel",
            "WORKSPACE",
            "WORKSPACE.bazel",
        ],
    ),
    crate_features = [
        "getrandom",
        "rand_core",
        "std",
    ],
    crate_root = "src/lib.rs",
    edition = "2018",
    rustc_flags = ["--cap-lints=allow"],
    srcs = glob(["**/*.rs"]),
    tags = [
        "cargo-bazel",
        "crate-name=crypto-common",
        "manual",
        "noclippy",
        "norustfmt",
    ],
    version = "0.1.6",
)
```
