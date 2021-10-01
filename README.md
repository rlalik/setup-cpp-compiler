[![Test](https://github.com/rlalik/setup-cpp-compiler/actions/workflows/build.yml/badge.svg)](https://github.com/rlalik/setup-cpp-compiler/actions/workflows/build.yml)

This GitHub action sets up selected C/C++ compiler in your workflow run.

The action requires specyfing compiler version using `compiler` input.

Use it in your workflow like this:
```yml
    - name: Install compiler
      uses: rlalik/setup-cpp-compiler@v1
      with:
        compiler: latest
```
Output variables:
* `cc` - holds C compiler
* `cxx` - holds C++ compiler

Example usage:
```
    - name: Use compiler
        shell: bash
        env:
            CC: ${{ steps.install_cc.outputs.cc }}
            CXX: ${{ steps.install_cc.outputs.cxx }}
```

# OS selection

Currently only Ubuntu and Windows are fully supported. For MacOS it is possible to install selected `gcc` however `clang` will always fallback to system default.



# Compiler selection
The input `compiler` can take following names:
* `latest` - resolves to the default gcc on a given system
* `gcc` or `gcc-latest` - resolves to the default gcc on a given system
* `clang` or `clang-latest` - resolves to the default clang on a given system
* other values - depending on selected OS, see sections below.

Exceptions:
* on MacOS, each `gcc` must have specified version, thus `latest`, `gcc`, `gcc-latest` are not allowed
* on MacOS, `clang` versions are not supported

## Available compilers
The available compilers are specific to the selected operating system.

You can check available versions here:
* Linux (Ubuntu)
  * gcc/g++ - https://packages.ubuntu.com/focal/devel/gcc and https://packages.ubuntu.com/focal/devel/g++
  * clang - https://packages.ubuntu.com/focal/devel/clang
* Windows
  * gcc - https://community.chocolatey.org/packages/mingw#versionhistory
  * clang - https://community.chocolatey.org/packages/llvm#versionhistory
* MacOs
  * gcc - https://formulae.brew.sh/formula/gcc
  * clang - https://formulae.brew.sh/formula/llvm (currently clang will always fallback to system default version)

## Example values of complilers

```yml
      matrix:
        os-version: [ ubuntu-latest ]
        compiler: [ latest, gcc-latest, gcc-9, gcc-10, g++-11, clang, clang-10, clang-11, clang++-12 ]
```

```yml
      matrix:
        os-version: [ windows-latest ]
        compiler: [ latest, gcc-latest, gcc-6.4.0, gcc-10.2.0, g++-11.2.0, clang, clang-8.0.0, clang-11.1.0, clang++-12.0.1 ]
```
