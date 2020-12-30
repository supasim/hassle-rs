🌤 hassle-rs
========
[![Actions Status](https://github.com/Traverse-Research/hassle-rs/workflows/Continuous%20integration/badge.svg)](https://github.com/Traverse-Research/hassle-rs/actions)
[![Latest version](https://img.shields.io/crates/v/hassle-rs.svg)](https://crates.io/crates/hassle-rs)
[![Documentation](https://docs.rs/hassle-rs/badge.svg)](https://docs.rs/hassle-rs)
[![Lines of code](https://tokei.rs/b1/github/Traverse-Research/hassle-rs)](https://github.com/Traverse-Research/hassle-rs)
![MIT](https://img.shields.io/badge/license-MIT-blue.svg)
[![Contributor Covenant](https://img.shields.io/badge/contributor%20covenant-v1.4%20adopted-ff69b4.svg)](../master/CODE_OF_CONDUCT.md)

[![Banner](banner.png)](https://traverseresearch.nl)

This crate provides an FFI layer and idiomatic rust wrappers for the new [DirectXShaderCompiler](https://github.com/Microsoft/DirectXShaderCompiler) library.

- [Documentation](https://docs.rs/hassle-rs)

## Usage

Add this to your `Cargo.toml`:

```toml
[dependencies]
hassle-rs = "0.4.0"
```

Then acquire `dxcompiler.dll` on Windows or `libdxcompiler.so` on Linux directly from [AppVeyor](https://ci.appveyor.com/project/antiagainst/directxshadercompiler/branch/master/artifacts), or compile it from source according to the instructions in the [DirectXShaderCompiler](https://github.com/Microsoft/DirectXShaderCompiler) GitHub repository and make sure it's in the executable environment.

DxcValidator also requires `dxil.dll` which can be grabbed from any recent Windows 10 SDK flight.
More info: https://www.wihlidal.com/blog/pipeline/2018-09-16-dxil-signing-post-compile/

## Compile HLSL into SPIR-V:

```rust
let spirv = compile_hlsl(
    "shader_filename.hlsl",
    code,
    "copyCs",
    "cs_6_5",
    &vec!["-spirv"],
    &vec![
        ("MY_DEFINE", Some("Value")),
        ("OTHER_DEFINE", None)
    ],
);
```

## Compile HLSL into DXIL and validate it:

```rust
let dxil = compile_hlsl("test.cs.hlsl", test_cs, "main", "cs_6_5", args, &[]).unwrap();
let result = validate_dxil(&dxil); // Only a Windows machine in Developer Mode can run non-validated DXIL

if let Some(err) = result.err() {
    println!("validation failed: {}", err);
}
```

## macOS support

Currently macOS support is quite early, however, one can build a libdxcompiler.dynlib from source by building this commit: https://github.com/microsoft/DirectXShaderCompiler/pull/3062/commits/9527fe6346391ac9a31799d730c7744b482e2386 and following these steps: https://github.com/microsoft/DirectXShaderCompiler/blob/master/docs/DxcOnUnix.rst#building-dxc

## License

Licensed under MIT license ([LICENSE](LICENSE) or http://opensource.org/licenses/MIT)

## Contibutions

 - Graham Wihlidal
 - Tiago Carvalho
 - Marijn Suijten
 - Tomasz Stachowiak

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in this crate by you, shall be licensed as above, without any additional terms or conditions.

Contributions are always welcome; please look at the [issue tracker](https://github.com/Traverse-Research/hassle-rs/issues) to see what known improvements are documented.
