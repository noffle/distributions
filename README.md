# distributions

Script to build the dist.ipfs.io pages.

## How this repo works

The goal is to generate a file hierarchy that looks like this:

```
dist/index.html -- listing of all bundles available
dist/<dist> -- all versions of <dist>
dist/<dist>/README.md -- simple readme for <dist>
dist/<dist>/latest -- points to latest <version>
dist/<dist>/<version> -- dist version
dist/<dist>/<version>/README.md -- readme for <version> listing
dist/<dist>/<version>/<platform>.tar.gz -- archive for <platform>
```

Definitions:
- `<dist>` is a distribution, meaning a program or library we release.
- `<version>` is the version of the `<dist>`
- `<platform>` is a supported platform of `<dist>@<version>`

So for example, if we had `<dist>` `go-ipfs` and `native-app`, we might see a hierarchy like:

```
.
├── go-ipfs
│   ├── latest -> v0.3.7
│   ├── v0.3.6
│   │   ├── README.md
│   │   ├── go-ipfs_v0.3.6_darwin-386.tar.gz
│   │   ├── go-ipfs_v0.3.6_darwin-amd64.tar.gz
│   │   ├── go-ipfs_v0.3.6_linux-386.tar.gz
│   │   ├── go-ipfs_v0.3.6_linux-amd64.tar.gz
│   │   └── hashes
│   └── v0.3.7
├── index.html
└── native-app
    ├── latest -> v0.2.1
    └── v0.2.1
        ├── README.md
        ├── hashes
        ├── ipfs-native-app_v0.2.1_linux.tar.gz
        └── ipfs-native-app_v0.2.1_osx.zip

7 directories, 11 files
```

We call this the **distribution index**, the listing of all distributions, their versions, and platform assets.

Note how they each describe `<platform>` differently. This is likely to be inevitable as different platform identifiers will be used by different communities.

### distribution index versions / updates

The **distribution index** changes over time, kind of like a git repository. [Since we don't yet have commits](https://github.com/ipfs/notes/issues/23), we will just do a poor-man's versioning for the index itself. We will write all version hashes to a file `versions` in this repository.

A site like `dist.ipfs.io` or `ipfs.io/dist` would just serve the _latest_ version of the index.

### How assets are made

Each `<dist>` has a directory in the root of this repo. inside it there is a `Makefile` and other necessary scripts. Running

```
make
```

Should:
- figure out what the latest released version is (from github tags)
- figure out what versions are missing from the index
- construct the missing `<dist>/<version>` directories

## Do it

This project uses a makefile + scripts to build all the things.

```
make
```
should do everything.


## Make

Running make will create a releases folder with: 

```
$ tree releases -L 5
releases
├── electron-app
│   └── 0.1.0
│       ├── darwin
│       │   └── x64
│       │       └── IPFS.app
│       ├── linux
│       │   ├── ia32
│       │   │   ├── IPFS
│       │   │   ├── LICENSE
│       │   │   ├── content_shell.pak
│       │   │   ├── icudtl.dat
│       │   │   ├── libffmpegsumo.so
│       │   │   ├── libgcrypt.so.11
│       │   │   ├── libnode.so
│       │   │   ├── libnotify.so.4
│       │   │   ├── locales
│       │   │   ├── natives_blob.bin
│       │   │   ├── resources
│       │   │   ├── snapshot_blob.bin
│       │   │   └── version
│       │   └── x64
│       │       ├── IPFS
│       │       ├── LICENSE
│       │       ├── content_shell.pak
│       │       ├── icudtl.dat
│       │       ├── libffmpegsumo.so
│       │       ├── libgcrypt.so.11
│       │       ├── libnode.so
│       │       ├── libnotify.so.4
│       │       ├── locales
│       │       ├── natives_blob.bin
│       │       ├── resources
│       │       ├── snapshot_blob.bin
│       │       └── version
│       └── win32
│           ├── ia32
│           │   └── IPFS-win32
│           └── x64
│               └── IPFS-win32
└── go-ipfs
    ├── v0.3.2
    │   ├── hashes
    │   ├── ipfs_v0.3.2_darwin-386.zip
    │   ├── ipfs_v0.3.2_darwin-amd64.zip
    │   ├── ipfs_v0.3.2_linux-386.zip
    │   ├── ipfs_v0.3.2_linux-amd64.zip
    │   └── ipfs_v0.3.2_linux-arm.zip
    ├── v0.3.4
    │   ├── hashes
    │   ├── ipfs_v0.3.4_darwin-386.zip
    │   ├── ipfs_v0.3.4_darwin-amd64.zip
    │   ├── ipfs_v0.3.4_linux-386.zip
    │   ├── ipfs_v0.3.4_linux-amd64.zip
    │   └── ipfs_v0.3.4_linux-arm.zip
    ├── v0.3.5
    │   ├── hashes
    │   ├── ipfs_v0.3.5_darwin-386.zip
    │   ├── ipfs_v0.3.5_darwin-amd64.zip
    │   ├── ipfs_v0.3.5_linux-386.zip
    │   ├── ipfs_v0.3.5_linux-amd64.zip
    │   └── ipfs_v0.3.5_linux-arm.zip
    ├── v0.3.6
    │   ├── hashes
    │   ├── ipfs_v0.3.6_darwin-386.zip
    │   ├── ipfs_v0.3.6_darwin-amd64.zip
    │   ├── ipfs_v0.3.6_freebsd-amd64.zip
    │   ├── ipfs_v0.3.6_linux-386.zip
    │   ├── ipfs_v0.3.6_linux-amd64.zip
    │   ├── ipfs_v0.3.6_linux-arm.zip
    │   ├── ipfs_v0.3.6_windows-386.zip
    │   └── ipfs_v0.3.6_windows-amd64.zip
    └── v0.3.7
        ├── hashes
        ├── ipfs_v0.3.7_darwin-386.zip
        ├── ipfs_v0.3.7_darwin-amd64.zip
        ├── ipfs_v0.3.7_freebsd-amd64.zip
        ├── ipfs_v0.3.7_linux-386.zip
        ├── ipfs_v0.3.7_linux-amd64.zip
        ├── ipfs_v0.3.7_linux-arm.zip
        ├── ipfs_v0.3.7_windows-386.zip
        └── ipfs_v0.3.7_windows-amd64.zip
```

