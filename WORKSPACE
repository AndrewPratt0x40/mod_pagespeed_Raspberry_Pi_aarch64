workspace(name = "mod_pagespeed")

# BEGIN gcc-toolchain SPECIFIC
# NOTE: The following code is a modified version of the code snippet at: https://github.com/f0rmiga/gcc-toolchain/issues/88
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# Skylib
# gcc-toolchain depends on bazel-skylib
http_archive(
    name = "bazel_skylib",
    sha256 = "f7be3474d42aae265405a592bb7da8e171919d74c16f082a5457840f06054728",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-skylib/releases/download/1.2.1/bazel-skylib-1.2.1.tar.gz",
        "https://github.com/bazelbuild/bazel-skylib/releases/download/1.2.1/bazel-skylib-1.2.1.tar.gz",
    ],
)

load("@bazel_skylib//:workspace.bzl", "bazel_skylib_workspace")

bazel_skylib_workspace()

http_archive(
    name = "aspect_gcc_toolchain",
    sha256 = "3341394b1376fb96a87ac3ca01c582f7f18e7dc5e16e8cf40880a31dd7ac0e1e",
    strip_prefix = "gcc-toolchain-0.4.2",
    url = "https://github.com/aspect-build/gcc-toolchain/archive/refs/tags/0.4.2.tar.gz",
)

load("@aspect_gcc_toolchain//toolchain:defs.bzl", "ARCHS", "gcc_register_toolchain")

gcc_register_toolchain(
    name = "gcc_toolchain_aarch64",
    gcc_version = "10.3.0",
    sysroot_variant = "aarch64",
    target_arch = ARCHS.aarch64,
)
# END gcc-toolchain SPECIFIC

load("//bazel:repositories.bzl", "mod_pagespeed_dependencies")

mod_pagespeed_dependencies()

local_repository(
    name = "envoy_build_config",
    path = ".",
)

load("@envoy//bazel:api_binding.bzl", "envoy_api_binding")

envoy_api_binding()

load("@envoy//bazel:api_repositories.bzl", "envoy_api_dependencies")

envoy_api_dependencies()

load("@envoy//bazel:repositories.bzl", "envoy_dependencies")

envoy_dependencies()

load("@envoy//bazel:repositories_extra.bzl", "envoy_dependencies_extra")

envoy_dependencies_extra()

load("@envoy//bazel:python_dependencies.bzl", "envoy_python_dependencies")

envoy_python_dependencies()

load("@envoy//bazel:dependency_imports.bzl", "envoy_dependency_imports")

envoy_dependency_imports()

# For PIP support:
load("@rules_python//python:pip.bzl", "pip_install")

# This rule translates the specified requirements.txt into
# @my_deps//:requirements.bzl, which itself exposes a pip_install method.
pip_install(
    name = "python_pip_deps",
    requirements = "//:requirements.txt",
)
