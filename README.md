# Lotus node (lotusd): releases

Binaries of the Lotus node software built by LotusDevOrg: `lotusd` and its
tools, the GPU miner and the Lotus Miner desktop app.

**Download the latest version: [Releases → Latest](https://github.com/LotusDevOrg/lotusd-releases/releases/latest)**

This repository holds releases only: the downloads, their checksums and
the release notes. It has no source code.

> **Operators: run 12.5.1 or later before 2026-12-21 20:50 UTC.** That is
> when the Napoleon network upgrade activates new consensus rules on
> mainnet (median time past of the previous block at or after
> 1797886200). Earlier versions, 12.4.x included, do not follow the chain
> after the fork. Until then 12.5.x follows the same rules as 12.4.x, so
> you can upgrade now. The latest version is **12.5.2**; the notes of each
> release say what changes and what to do. Upgrade guide:
> [lotusia.network/upgrades](https://lotusia.network/upgrades/).

> **Merged-mining pools, from Napoleon:** Litecoin pools that already
> merge-mine Lotus with standard AuxPoW need no change. SHA-256d pools
> (eCash, Bitcoin, Bitcoin Cash) name their chain in one place:
> `createauxblock <address> xec|btc|bch`, or `-auxpowalgo=<chain>` on the
> Lotus node. Details: [lotusia.network/napoleon](https://lotusia.network/napoleon/#pools).

## What a release contains

Every file is named `<name>-<version>-<os>-<arch>` (`.tar.gz`, `.zip` for
Windows). `SHA256SUMS` lists them all.

| | Linux x86_64 | Windows x86_64 | macOS x86_64 |
|---|---|---|---|
| Node: `lotusd` | yes | yes | yes |
| Tools: `lotus-cli`, `lotus-tx`, `lotus-wallet` | yes | yes | yes |
| `lotus-seeder` | yes | no | yes |
| All of the above in one archive: `lotus-binaries` | yes | yes | yes |
| GPU miner: `lotus-gpu-miner-nvidia`, `lotus-gpu-miner-amd` | yes | no | no |
| Desktop app: `lotus-miner-gui` | yes | yes | yes (arm64 and universal builds when a release has them) |

Container images of the same version are on GitHub's registry:
`ghcr.io/lotusdevorg/lotusd` (node), `lotus-cli`, `lotus-tx`,
`lotus-wallet`, `lotus-seeder`, `lotus-gpu-miner` (NVIDIA) and
`lotus-gpu-miner-amd`, each tagged with the version, with its `X.Y` line
and with `latest`.

## Install

Replace `12.5.2` with the version you want. The release page lists every
file with its size and checksum.

### Linux

```sh
V=12.5.2
BASE=https://github.com/LotusDevOrg/lotusd-releases/releases/download/v$V
curl -fsSLO $BASE/SHA256SUMS
curl -fsSLO $BASE/lotus-binaries-$V-linux-x86_64.tar.gz
sha256sum -c --ignore-missing SHA256SUMS     # must print: OK
tar -xzf lotus-binaries-$V-linux-x86_64.tar.gz
./lotus-binaries-$V-linux-x86_64/lotusd -version
```

Copy the binaries where you keep them (for example `/usr/local/bin`) and
start `lotusd`. Its data directory is `~/.lotus` unless you pass
`-datadir`; `lotusd -help` lists every option.

### macOS

The same steps with `lotus-binaries-$V-macos-x86_64.tar.gz`; check with
`shasum -a 256 -c --ignore-missing SHA256SUMS`. The binaries are built for
Intel Macs and are not notarised by Apple: macOS asks for confirmation
before the first launch.

### Windows

Download `lotus-binaries-<version>-windows-x86_64.zip` and `SHA256SUMS`
from the release page. In PowerShell,
`Get-FileHash .\lotus-binaries-<version>-windows-x86_64.zip -Algorithm SHA256`
must give the value on that file's line in `SHA256SUMS`. Then unzip.

### Docker

```sh
docker pull ghcr.io/lotusdevorg/lotusd:12.5.2
```

Pull a version, not only `latest`, so that an upgrade is something you
decide. The release notes give the image digests; pulling by digest
(`ghcr.io/lotusdevorg/lotusd@sha256:...`) pins the exact image.

## Upgrade

1. Read the notes of the release, and of every release between yours and
   it: they say when a version needs a reindex or changes a default.
2. Back up `wallet.dat`.
3. Stop the node and wait until it has exited (`lotus-cli stop`; with
   Docker, give it time: `docker compose stop -t 120`).
4. Replace the binaries, or change the image tag.
5. Start it and check the version it reports (`lotusd -version`,
   `lotus-cli getnetworkinfo`).

The data directory is kept as it is. Going back to an older version is
not always possible once a newer one has written to it; the notes say
when it is not.

## Checksums and signatures

- **`SHA256SUMS`** lists the SHA-256 of every file of the release. It is
  produced by the same automated build that produces the files, and a
  release is published only after what GitHub holds has been compared with
  what was built. Always check your download against it.
- **The files are not signed today.** There is no GPG signature of
  `SHA256SUMS`, and the node binaries carry no code signature. What you
  rely on is this repository: download from
  `github.com/LotusDevOrg/lotusd-releases` over HTTPS and nowhere else. A
  Lotus binary offered on another site, with or without a checksum beside
  it, is not ours.
- The desktop app is signed for Windows and macOS when a release says so
  in its notes. Otherwise it is unsigned (ad-hoc signed on macOS) and the
  system asks for confirmation on first launch.
- Container images: the digest in the release notes identifies the image
  exactly. Compare it with
  `docker image inspect --format '{{index .RepoDigests 0}}' <image>`.
- When signing is added it will be announced here and in the release
  notes, with the key and how to check it. Nothing else is an announcement
  of a key.

## Third-party data

From 12.5.2 the node embeds an IP-to-country and IP-to-ASN table for the
peer flags of its web UI: DB-IP "IP to Country Lite" and "IP to ASN Lite"
([db-ip.com](https://db-ip.com)), licensed CC BY 4.0. The lookup runs inside
the node; no address is sent anywhere.

## Links

- Releases: https://github.com/LotusDevOrg/lotusd-releases/releases
- Container images: https://github.com/orgs/LotusDevOrg/packages
- A problem with a download or an install: open an issue on this repository.
