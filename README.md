# s5cmd snap

[s5cmd](https://github.com/peak/s5cmd) is a very fast S3 and local filesystem execution tool. This snap packages upstream `v2.3.0` and runs under strict confinement.

## Install

```
sudo snap install s5cmd
```

This installs from `latest/stable`. Check the installed version:

```
s5cmd version
```

Expected: `v2.3.0-991c9fb`.

## AWS credentials

Strict confinement hides the host `~/.aws`. The snap sees its own home directory, `~/snap/s5cmd/current`, so copy your credentials there:

```
cp -r ~/.aws ~/snap/s5cmd/current/
```

This creates `~/snap/s5cmd/current/.aws`, which s5cmd reads as `$HOME/.aws`.

Copies are independent of `~/.aws`. Update them in both places when credentials rotate.

## Usage

```
s5cmd ls s3://your-bucket/
s5cmd cp ./local-file.txt s3://your-bucket/
s5cmd cp 's3://your-bucket/prefix/*' ./local-dir/
```

For public buckets, pass `--no-sign-request`:

```
s5cmd --no-sign-request ls s3://noaa-gfs-bdp-pds/
```

Local-to-local `cp` is refused by s5cmd by design. Use it for S3 transfers only.

## Notes

- Built on `core24`. The Go toolchain comes from the `go/1.22/stable` snap at build time.
- Only `amd64` is built.
- Bash completion from upstream is not wired into the snap.
- Upstream is MIT-licensed; see its `LICENSE` file.

## Maintainers

This repo holds packaging only. `snapcraft.yaml` fetches the `v2.3.0` tag from GitHub and builds it with `go build -mod=vendor`. To build locally:

```
snapcraft pack
```

Output: `s5cmd_2.3.0_amd64.snap`. Local builds are unsigned; install with `sudo snap install --dangerous ./s5cmd_2.3.0_amd64.snap` only for testing.
