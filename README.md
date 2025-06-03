# Pre-commit hook for clangd-tidy

A hook for running [`clangd-tidy`](https://github.com/lljbash/clangd-tidy) with pre-commit.
`clangd-tidy` is a faster alternative to clang-tidy for C++

## Prerequisite

Make sure your project generates a compilation database.
If you are using CMake, you can add this line to your CMakeLists.txt:

```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```

If you are using GNU Make, you can have a look at [bear](https://github.com/rizsotto/Bear)

It's recommanded to have a `.clang-tidy` [configuration file](https://clangd.llvm.org/config)
at the root of your repo. Here's an example config file:

```
# Checks, first line is activated checks, all subsequent lines are disabled ones
Checks:
  "cppcoreguidelines-*,clang-diagnostic-*,clang-analyzer-*,modernize-*,readability-*,performance-*,bugprone-*,\
  -modernize-use-trailing-return-type,-readability-magic-numbers,-performance-move-const-arg,-cppcoreguidelines-avoid-magic-numbers,\
  -cppcoreguidelines-pro-type-reinterpret-cast,-bugprone-easily-swappable-parameters,-cppcoreguidelines-pro-type-vararg"

# Treat all warnings as error
# Uncomment this line if you want to be strict
WarningsAsErrors: "*"

# Use the .clang-format file in the project
FormatStyle: file

# For now only naming conventions are defined here
CheckOptions:
  # Member variables
  - key: readability-identifier-naming.ClassConstantCase
    value: UPPER_CASE
  - key: readability-identifier-naming.ClassMemberPrefix
    value: ""
  - key: readability-identifier-naming.ClassMemberSuffix
    value: "_"
  - key: readability-identifier-naming.PrivateMemberSuffix
    value: "_"
  - key: readability-identifier-naming.ProtectedMemberSuffix
    value: "_"
```

> [!IMPORTANT]
> If you want the hook to be blocking commits if checks are not
> respected make sure to have `WarningsAsErrors: "*"`
> or pass in `--warnings-as-errors="*"` in the configuration below

## Usage

Add these lines to your .pre-commit-config.yaml

```
- repo: https://github.com/thibthibaut/clangd-format-pre-commit
  rev: v1.0.0
  hooks:
    - id: clangd-tidy
      args: [-p build/] # Path to the directory where compile_commands.json is generated
```

> [!INFO]
> Check [this page](https://github.com/lljbash/clangd-tidy#usage)
> for the full list of arguments that can be passed to clangd-tidy
