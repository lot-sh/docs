# RFC 002 Resource locator

**Feature Name:** Resource locator

**Type:** feature

**Start Date:** 2020-05-10

**Author:** @rubeniskov, @kelvur

**Standards:**
[Template](https://gist.github.com/michaelcurry/e0132058fcd6d588a1299afd69638df4) [Keywords](https://www.ietf.org/rfc/rfc2119.txt)

## Summary

A resource locator is, as mention its own name, a system which provides a reference to a resource wherever is located, defining the scheme logic used to resolve it.

## Motivation

A vast and commonly used resource identificator is [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)

This allowus to define a list of possible locations to obtain the resource.

## URI

- **schema**: Define the scheme logic used to resolve the resource data.
- **path**: Consumer part of the scheme which defines the resource identity
- **tag**(Optional): The part of the path which specifies the version or the state of the resource

```
news:comp.infosystems.www.servers.unix@0.0.1
└┬─┘ └─────────────┬─────────────────┘ └─┬─┘
scheme            path                  tag
```

## Available Schemes

### Lot

![Lot diagram](../images/uri-lot-diagram.svg)

```
lot:<cid>
lot:<namespace>/<name>[@<tag>]
```

- **cid**: The string identifier under the define scheme.
- **namespace**: A prefix which envolves the name to prevent name colissions and identify the context, owner organization which creates the resource.
- **name**: The human redable name of the script.
- **tag**: The string identifier under the define scheme.

A resource identifier is always linked to a CID

```textplain
namespace/name@tag -> cidv1
```

### Gist

![Gist diagram](../images/uri-gist-diagram.svg)

```
gist:<username>/<gist>[@<tag|hash>]
```

- **username**: The username
- **gist**: The unique identificator of the gist resource
- **tag**: hash of the revision, or tag, but only accepts `latest` which is by default when not specified.

### File System

https://en.wikipedia.org/wiki/File_URI_scheme

`file://<hostname>/<path>`

`file:///<path>`

## Some examples

```
lot:QmcRD4wkPPi6dig81r5sLj9Zm1gDCL4zgpEj9CfuRrGbzF
lot:rubeniskov/test
lot:rubeniskov/test@0.0.1
```

```
gist:rubeniskov/semver-patcher
gist:rubeniskov/semver-patcher@0.0.2
```

```
file://localhost/etc/fstab
file:///etc/fstab
```

> Note: to enable discovering versions in gist the content of the resource must contain the header defined by the [Resource Info](./resource-info.md) standard.

```
ipfs:<hash>[/<path>]
ipns:<hash>[/<path>]
```

```
http://www.example.com/filename
https://www.example.com/filename
```

```
ftp://www.example.com/filename
ftps://www.example.com/filename
```

## Schemes

- **ftp**: File transfer protocol
- **ftps**: File transfer protocol secured
- **http**: Hypertext transfer protocol
- **https**: Hypertext transfer protocol Secured
- **ipfs**: Interplanetary file system
- **lot**: lot.sh scheme locator
- **gist**: gist.github.com scheme locator

## Implementation

```shell
type ResourceLocator interface {
    Scheme() string
    Path() string
    Tag() string
}
```

## References

- https://github.com/ipfs/go-ipfs/issues/1678
- https://proto.school/#/anatomy-of-a-cid/06
- https://github.com/multiformats/cid
