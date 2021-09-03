# Copyright 2018 The Bazel Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

workspace(
    name = "com_grail_bazel_toolchain",
)

## Toolchain example with a sysroot.
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# This sysroot is used by github.com/vsco/bazel-toolchains.
http_archive(
    name = "org_chromium_sysroot_linux_x64",
    build_file_content = """
filegroup(
  name = "sysroot",
  srcs = glob(["*/**"]),
  visibility = ["//visibility:public"],
)
""",
    sha256 = "84656a6df544ecef62169cfe3ab6e41bb4346a62d3ba2a045dc5a0a2ecea94a3",
    urls = ["https://commondatastorage.googleapis.com/chrome-linux-sysroot/toolchain/2202c161310ffde63729f29d27fe7bb24a0bc540/debian_stretch_amd64_sysroot.tar.xz"],
)

LLVM_VERSION = "12.0.0"

load("@com_grail_bazel_toolchain//toolchain:deps.bzl", "bazel_toolchain_dependencies")

bazel_toolchain_dependencies()

load("@com_grail_bazel_toolchain//toolchain:rules.bzl", "llvm_toolchain")

llvm_toolchain(
    name = "llvm_toolchain",
    llvm_version = LLVM_VERSION,
    sha256 = {
        "darwin": "d3f9247d73cd077308c1c3afd4956fb58cf19f7ec823d3de89dcbeb5468279fb",
        "linux": "855377b52e65b6a2975e03efccf8616043398506d455775ee39387e854e38952",
    },
    strip_prefix = {
        "darwin": "clang+llvm-%s-x86_64-apple-darwin" % LLVM_VERSION,
        "linux": "llvm-project-%s" % LLVM_VERSION,
    },
    sysroot = {
        # Keep in sync with the _sysroot attribute in clang_tidy_test and ci-protoc.
        "linux": "@org_chromium_sysroot_linux_x64//:sysroot",
        "windows": "//windows_sysroot:sysroot",
    },
    urls = {
        "darwin": ["https://github.com/CodeIntelligenceTesting/llvm-project/releases/download/rel/clang+afl_driver+llvm-%s-x86_64-apple-darwin.tar.xz" % LLVM_VERSION],
        "linux": ["https://s3.eu-central-1.amazonaws.com/public.code-intelligence.com/llvm/llvm-project-%s.tar.xz" % LLVM_VERSION],
    },
)

load("@llvm_toolchain//:toolchains.bzl", "llvm_register_toolchains")

llvm_register_toolchains()

# Well known repos; present here only for testing.

http_archive(
    name = "com_google_googletest",
    sha256 = "9dc9157a9a1551ec7a7e43daea9a694a0bb5fb8bec81235d8a1e6ef64c716dcb",
    strip_prefix = "googletest-release-1.10.0",
    urls = ["https://github.com/google/googletest/archive/release-1.10.0.tar.gz"],
)

http_archive(
    name = "com_github_google_benchmark",
    sha256 = "3c6a165b6ecc948967a1ead710d4a181d7b0fbcaa183ef7ea84604994966221a",
    strip_prefix = "benchmark-1.5.0",
    urls = ["https://github.com/google/benchmark/archive/v1.5.0.tar.gz"],
)

http_archive(
    name = "com_google_absl",
    sha256 = "0db0d26f43ba6806a8a3338da3e646bb581f0ca5359b3a201d8fb8e4752fd5f8",
    strip_prefix = "abseil-cpp-20200225.1",
    urls = ["https://github.com/abseil/abseil-cpp/archive/20200225.1.tar.gz"],
)

http_archive(
    name = "openssl",
    build_file = "//tests/openssl:openssl.bazel",
    sha256 = "f6fb3079ad15076154eda9413fed42877d668e7069d9b87396d0804fdb3f4c90",
    strip_prefix = "openssl-1.1.1c",
    urls = ["https://www.openssl.org/source/openssl-1.1.1c.tar.gz"],
)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "e6a6c016b0663e06fa5fccf1cd8152eab8aa8180c583ec20c872f4f9953a7ac5",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.22.1/rules_go-v0.22.1.tar.gz",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.22.1/rules_go-v0.22.1.tar.gz",
    ],
)

load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains()

http_archive(
    name = "platforms",
    sha256 = "079945598e4b6cc075846f7fd6a9d0857c33a7afc0de868c2ccb96405225135d",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/platforms/releases/download/0.0.4/platforms-0.0.4.tar.gz",
        "https://github.com/bazelbuild/platforms/releases/download/0.0.4/platforms-0.0.4.tar.gz",
    ],
)
