# Crate Universe Extra `crate_features` Bug

Running:

```bash
bazel build //extra_crate_features/wasm:wasm
```

causes errors like `unresolved import 'crate::sys::IoSourceState'`. This is because the file
`external/crate_index_extra_crate_features__tokio-1.25.0/BUILD.bazel`
has the "net" feature in `crate_features` from the unrelated [net](net/)
crate.

If the `net` crate is removed from WORKSPACE.bazel and Cargo.toml and then running the command

```bash
cargo generate-lockfile && CARGO_BAZEL_REPIN=true bazel build //extra_crate_features/wasm:wasm
```

the build passes, because there is no "net" feature anymore snuck from the net crate.

Furthermore running:

```
cd wasm && cargo build --target=wasm32-unknown-unknown
```

works fine which means cargo using `resolver = "2"` is able to skip adding a feature to the
WASM build from an unrelated crate.
