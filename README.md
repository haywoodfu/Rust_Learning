# Rust_Learning
Record Issues when using Rust &amp; Dioxus

## compile dioxus fullstack project with Ring throws error
When compile Ring throw the error blow, it means LLVM need to be installed and config env:
```
error: failed to run custom build command for `ring v0.17.8`

Caused by:
  process didn't exit successfully: `/Users/brian/Develop/chimera/target/dioxus-server/build/ring-5d4fb93703437501/build-script-build` (exit status: 1)
  --- stdout
  cargo:rerun-if-env-changed=RING_PREGENERATE_ASM
  cargo:rustc-env=RING_CORE_PREFIX=ring_core_0_17_8_
  OPT_LEVEL = Some(2)
  TARGET = Some(wasm32-unknown-unknown)
  OUT_DIR = Some(/Users/brian/Develop/chimera/target/wasm32-unknown-unknown/dioxus-server/build/ring-fc3eae76940ff1f8/out)
  HOST = Some(aarch64-apple-darwin)
  cargo:rerun-if-env-changed=CC_wasm32-unknown-unknown
  CC_wasm32-unknown-unknown = None
  cargo:rerun-if-env-changed=CC_wasm32_unknown_unknown
  CC_wasm32_unknown_unknown = None
  cargo:rerun-if-env-changed=TARGET_CC
  TARGET_CC = None
  cargo:rerun-if-env-changed=CC
  CC = None
  cargo:rerun-if-env-changed=CC_ENABLE_DEBUG_OUTPUT
  RUSTC_WRAPPER = None
  cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
  CRATE_CC_NO_DEFAULTS = None
  cargo:rerun-if-env-changed=WASI_SYSROOT
  WASI_SYSROOT = None
  DEBUG = Some(true)
  cargo:rerun-if-env-changed=CFLAGS_wasm32-unknown-unknown
  CFLAGS_wasm32-unknown-unknown = None
  cargo:rerun-if-env-changed=CFLAGS_wasm32_unknown_unknown
  CFLAGS_wasm32_unknown_unknown = None
  cargo:rerun-if-env-changed=TARGET_CFLAGS
  TARGET_CFLAGS = None
  cargo:rerun-if-env-changed=CFLAGS
  CFLAGS = None
  cargo:warning=error: unable to create target: 'No available targets are compatible with triple "wasm32-unknown-unknown"'
  cargo:warning=1 error generated.
  exit status: 1
  cargo:warning=ToolExecError: Command "clang" "-O2" "-ffunction-sections" "-fdata-sections" "-fPIC" "-fno-exceptions" "-g" "-fno-omit-frame-pointer" "--target=wasm32-unknown-unknown" "-I" "include" "-I" "/Users/brian/Develop/chimera/target/wasm32-unknown-unknown/dioxus-server/build/ring-fc3eae76940ff1f8/out" "-Wall" "-Wextra" "-fvisibility=hidden" "-std=c1x" "-Wall" "-Wbad-function-cast" "-Wcast-align" "-Wcast-qual" "-Wconversion" "-Wmissing-field-initializers" "-Wmissing-include-dirs" "-Wnested-externs" "-Wredundant-decls" "-Wshadow" "-Wsign-compare" "-Wsign-conversion" "-Wstrict-prototypes" "-Wundef" "-Wuninitialized" "-g3" "-nostdlibinc" "-DNDEBUG" "-DRING_CORE_NOSTDLIBINC=1" "-o" "/Users/brian/Develop/chimera/target/wasm32-unknown-unknown/dioxus-server/build/ring-fc3eae76940ff1f8/out/fad98b632b8ce3cc-curve25519.o" "-c" "crypto/curve25519/curve25519.c" with args clang did not execute successfully (status code exit status: 1).cargo:warning=error: unable to create target: 'No available targets are compatible with triple "wasm32-unknown-unknown"'
  cargo:warning=1 error generated.
```

try following these steps:
```
Install llvm via home brew:
1. brew install llvm
2. Link to this home brew version
echo 'export AR=/opt/homebrew/opt/llvm/bin/llvm-ar' >> ~/.zshrc
echo 'export CC=/opt/homebrew/opt/llvm/bin/clang' >> ~/.zshrc
```

## Can not init complicate value in #[props(default = **)]
#[props(default)]  can not used to init complicate type: Signal etc. 


## Multiple move will cause code not execute
This code won't excute after wait, can not output "login succ" & "in async move error"
```
    let mut is_logining = use_signal(|| false);
    let perform_login = move || {
        info!("in perform_login");
            info!("in perform_login future");
            async move {
                let mut user_state = user_state.clone();
                info!("in async move");
                match login(email).await {
                    Ok(user) => {
                        info!("login succ");
                    }
                    Err(e) => {
                         info!("in async move error");
                    }
                }
            }
    }
    let on_click = move |_| {
        is_logining.set(true);
        perform_login();
    }
    ***
    rsx!{
        button {
            on_click,
            disable: *is_login.read()
        }
    }
```
