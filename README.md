# my-docker-erlang-otp

[![status-badge](https://ci.hentioe.dev/api/badges/10/status.svg)](https://ci.hentioe.dev/repos/10)

我维护的 Erlang 镜像列表，主要用于为[我的 Elixir 镜像](https://github.com/Hentioe/my-docker-elixir)提供基础。它们也可以单独使用。

## 基础

不同的后缀对应不同的基础系统镜像，主要是 Void Linux、Alpine Linux 和 Debian。具体关系如下：

| 完整镜像名                 | 基础镜像                                           |
| :------------------------- | :------------------------------------------------- |
| `hentioe/erlang:28-void`   | `ghcr.io/void-linux/void-glibc-busybox:20260401R1` |
| `hentioe/erlang:28-alpine` | `alpine:3.23`                                      |
| `hentioe/erlang:28-debian` | `debian:bookworm`                                  |
| `hentioe/erlang:27-void`   | `ghcr.io/void-linux/void-glibc-busybox:20260401R1` |
| `hentioe/erlang:27-alpine` | `alpine:3.22`                                      |
| `hentioe/erlang:27-debian` | `debian:bookworm`                                  |

本仓库还提供无 JIT 的镜像，后缀为 `-nojit`，版本和以上基础镜像相同。这类镜像可以节省运行时的内存开销。

## 不同

与 Docker 官方提供的 Erlang 镜像不同，本仓库的镜像不包含 [Rebar3](https://github.com/erlang/rebar3)。将 Rebar3 内置在镜像中，对于构建 Elixir 镜像而言是多余的。
