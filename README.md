# my-docker-erlang-otp

[![status-badge](https://ci.hentioe.dev/api/badges/10/status.svg)](https://ci.hentioe.dev/repos/10)

The Erlang images maintained by Hentioe mainly provide the basis for the [Elixir](https://github.com/Hentioe/my-docker-elixir) images.

## Basics

The base images are Void Linux, Alpine Linux, and Debian, and the versions are as follows:

| image                      | base                                               |
| :------------------------- | :------------------------------------------------- |
| `hentioe/erlang:28-void`   | `ghcr.io/void-linux/void-glibc-busybox:20250701R1` |
| `hentioe/erlang:28-alpine` | `alpine:3.22`                                      |
| `hentioe/erlang:28-debian` | `debian:bookworm`                                  |
| `hentioe/erlang:27-void`   | `ghcr.io/void-linux/void-glibc-busybox:20250701R1` |
| `hentioe/erlang:27-alpine` | `alpine:3.22`                                      |
| `hentioe/erlang:27-debian` | `debian:bookworm`                                  |

## Difference

Unlike the unorganized Erlang images provided by Docker, these do not include [Rebar3](https://github.com/erlang/rebar3).
