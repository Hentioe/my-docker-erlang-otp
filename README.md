# my-docker-erlang-otp

[![status-badge](https://woodpecker-ci.hentioe.dev/api/badges/1/status.svg)](https://woodpecker-ci.hentioe.dev/repos/1)

Erlang images maintained by Hentioe, also the base of [`my-docker-elixir`](https://github.com/Hentioe/my-docker-elixir).

## Versions

The base images are Void Linux, Alpine Linux, and Debian, and the versions are as follows:

| image                      | base                                               |
| :------------------------- | :------------------------------------------------- |
| `hentioe/erlang:26-void`   | `ghcr.io/void-linux/void-glibc-busybox:20240526R1` |
| `hentioe/erlang:26-alpine` | `alpine:3.20`                                      |
| `hentioe/erlang:26-debian` | `debian:bookworm`                                  |
| `hentioe/erlang:27-void`   | `ghcr.io/void-linux/void-glibc-busybox:20240526R1` |
| `hentioe/erlang:27-alpine` | `alpine:3.20`                                      |
| `hentioe/erlang:27-debian` | `debian:bookworm`                                  |

## Note

All images do not contain Rebar3.
