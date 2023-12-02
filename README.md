[![build workflow](https://github.com/kraudcloud/go-libzfs/actions/workflows/test.yaml/badge.svg)](https://github.com/kraudcloud/go-libzfs/actions)
[![PkgGoDev](https://pkg.go.dev/badge/github.com/kraudcloud/go-libzfs)](https://pkg.go.dev/github.com/kraudcloud/go-libzfs)
[![Chat](https://discordapp.com/api/guilds/822439761263198239/widget.png)](https://discord.gg/wppeemfAn9)

## ZFS On Linux compatibility

- Version tagged as v2.22. is compatible with ZOL version 2.2.

## Installing

```sh
go get github.com/kraudcloud/go-libzfs/v2
```

### Prerequisites
Make the following `go env` look like this, else you won't be able to
run `go get` and `go run` on this module.

```
CGO_ENABLED='1'
GO111MODULE='auto'
```

## Usage example

```go
// Create map to represent ZFS dataset properties. This is equivalent to
// list of properties you can get from ZFS CLI tool, and some more
// internally used by libzfs.
props := make(map[ZFSProp]Property)

// I choose to create (block) volume 1GiB in size. Size is just ZFS dataset
// property and this is done as map of strings. So, You have to either
// specify size as base 10 number in string, or use strconv package or
// similar to convert in to string (base 10) from numeric type.
strSize := "1073741824"

props[DatasetPropVolsize] = Property{Value: strSize}
// In addition I explicitly choose some more properties to be set.
props[DatasetPropVolblocksize] = Property{Value: "4096"}
props[DatasetPropReservation] = Property{Value: strSize}

// Lets create desired volume
d, err := DatasetCreate("TESTPOOL/VOLUME1", DatasetTypeVolume, props)
if err != nil {
	println(err.Error())
	return
}
// Dataset have to be closed for memory cleanup
defer d.Close()

println("Created zfs volume TESTPOOL/VOLUME1")
```

## Testing

```sh
docker buildx build --progress=plain -f ci.Dockerfile . -t zz
docker run --privileged --shm-size=4G zz
```

## Special thanks to

this was forked from the original work by [bicomsystems](https://github.com/bicomsystems/go-libzfs)
