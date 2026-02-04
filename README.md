# disk-go

Cross-platform disk and filesystem utilities for Go.

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](LICENSE)
[![Go Reference](https://pkg.go.dev/badge/github.com/kypello-io/disk-go.svg)](https://pkg.go.dev/github.com/kypello-io/disk-go)

## Overview

`disk-go` provides low-level disk I/O operations, statistics gathering, and filesystem information for Go applications. It offers comprehensive cross-platform support with optimized implementations for Linux, Windows, macOS, BSD variants, and Solaris.

This package originated from the MinIO Object Storage project and is maintained by the Kypello community as a public fork.

## Features

- **Disk Information**: Get disk space, inode counts, filesystem type
- **I/O Statistics**: Read/write operations, sectors, ticks, and more
- **Direct I/O**: Open files with O_DIRECT for cache bypass
- **Cross-Platform**: Extensive support across operating systems
- **Optimized**: Platform-specific implementations for best performance

## Installation

```bash
go get github.com/kypello-io/disk-go@v0.1.0
```

## Usage

```go
package main

import (
    "fmt"
    "log"

    "github.com/kypello-io/disk-go"
)

func main() {
    // Get disk information
    info, err := disk.GetInfo("/", true)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("Total: %d bytes\n", info.Total)
    fmt.Printf("Free: %d bytes\n", info.Free)
    fmt.Printf("Filesystem: %s\n", info.FSType)

    // Get I/O statistics (Linux)
    stats, err := disk.GetDriveStats(info.Major, info.Minor)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("Read IOs: %d\n", stats.ReadIOs)
    fmt.Printf("Write IOs: %d\n", stats.WriteIOs)
}
```

## API Reference

### Data Types

- **`Info`**: Disk information (Total, Free, Used, FSType, etc.)
- **`IOStats`**: I/O statistics (ReadIOs, WriteIOs, etc.)

### Functions

- `GetInfo(path string, firstTime bool) (Info, error)` - Get disk space info
- `GetDriveStats(major, minor uint32) (IOStats, error)` - Get I/O stats
- `SameDisk(disk1, disk2 string) (bool, error)` - Compare disk paths
- `IsRootDisk(diskPath, rootDisk string) (bool, error)` - Check if root disk
- `OpenFileDirectIO(filePath string, flag int, perm os.FileMode) (*os.File, error)` - Open with O_DIRECT
- `DisableDirectIO(f *os.File) error` - Disable direct I/O
- `AlignedBlock(blockSize int) []byte` - Allocate aligned buffer
- `Fdatasync(f *os.File) error` - Sync file data (Linux/Unix)

See [pkg.go.dev](https://pkg.go.dev/github.com/kypello-io/disk-go) for complete documentation.

## Platform Support

- Linux (x64, ARM, ARM64, s390x, 32-bit)
- Windows
- macOS / Darwin
- FreeBSD
- NetBSD
- OpenBSD
- Solaris

## Attribution

This package originated from [MinIO](https://github.com/minio/minio) (Copyright 2015-2021 MinIO, Inc.) and is maintained as part of the [Kypello](https://github.com/kypello-io/kypello) public fork.

Kypello is a community-maintained fork of MinIO designed to preserve critical enterprise functionality within the open-source ecosystem. This package has been extracted into a standalone module to enable broader usage.

**Note**: Kypello is not affiliated with, endorsed by, or sponsored by MinIO, Inc.

## License

GNU Affero General Public License v3.0 (AGPL-3.0)

See [LICENSE](LICENSE) for the full license text.

## Contributing

Contributions are welcome! Please ensure:
- Code follows existing patterns
- Platform-specific code uses appropriate build tags
- Tests pass on relevant platforms
- Changes maintain AGPL-3.0 compliance
