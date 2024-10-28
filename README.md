# Compile CPython to Wasm
## Prepare
1. Install wasi-sdk
2. `git clone https://github.com/python/cpython`
3. Modify `cpython/Tools/wasm/wasm_build.py`'s `WASI_SDK_PATH` to that of your wasi-sdk
4. Add `cmd.append(f"--prefix={BUILDDIR}/out")` to function `configure_cmd`

## Build
```bash
cd cpython
./Tools/wasm/wasm_build.py wasi
cd builddir/wasi
make install
```
If everything is good, dir `bin`, `include`, `lib`, `share` will be in `builddir/out`.

## Run it with wasmtime
To run wasm-cpython with interactive mode(my version is python3.14, you should change it to that of yours):
```bash
wasmtime run --dir {cpython-directory}/builddir/out/lib/python3.14 {cpython-directory}/builddir/out/bin/python3.14.wasm
```
To run any runnable python file with wasm-cpython:
```bash
wasmtime run --dir {cpython-directory}/builddir/out/lib/python3.14 --dir .::/code {cpython-directory}/builddir/out/bin/python3.14.wasm -- /code/{your_code.py}
```
The first `--dir` argument gives `python3.14.wasm` the access right to where lib files(aka python modules and libaries) lie. 

The second `--dir` argument gives `python3.14.wasm`the access right to the current working dir, so that it can access the arbitrary python file.
