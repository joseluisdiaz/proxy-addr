# proxy-addr [![Build Status](https://travis-ci.org/expressjs/proxy-addr.svg?branch=master)](https://travis-ci.org/expressjs/proxy-addr) [![NPM version](https://badge.fury.io/js/proxy-addr.svg)](http://badge.fury.io/js/proxy-addr)

Determine address of proxied request

## Install

    npm install proxy-addr

## API

    var proxyaddr = require('proxy-addr');

### proxyaddr(req, trust)

Return the address of the request, using the given `trust` parameter.

The `trust` argument is a function that returns `true` if you trust
the address, `false` if you don't. The closest untrusted address is
returned.

    proxyaddr(req, function(addr){ return addr === '127.0.0.1' })

The `trust` arugment may also be a single IP address string or an
array of trusted addresses, as plain IP addresses, CIDR-formatted
strings, or IP/netmask strings.

    proxyaddr(req, '127.0.0.1')
    proxyaddr(req, ['127.0.0.0/8', '10.0.0.0/8'])
    proxyaddr(req, ['127.0.0.0/255.0.0.0', '192.168.0.0/255.255.0.0'])

This module also supports IPv6. Your IPv6 addresses will be normalized
automatically (i.e. `fe80::00ed:1` equals `fe80:0:0:0:0:0:ed:1`).

    proxyaddr(req, '::1')
    proxyaddr(req, ['::1/128', 'fe80::/10'])
    proxyaddr(req, ['fe80::/ffc0::'])

This module will automatically work with IPv4-mapped IPv6 addresses
as well to support node.js in IPv6-only mode. This means that you do
not have to specify both `::ffff:a00:1` and `10.0.0.1`.

As a convenience, this module also takes certain pre-defined names
in addition to IP addresses, which expand into IP addresses:

    proxyaddr(req, 'loopback')
    proxyaddr(req, ['loopback', 'fc00:ac:1ab5:fff::1/64'])

  * `loopback`: IPv4 and IPv6 loopback addresses (like `::1` and
    `127.0.0.1`).
  * `linklocal`: IPv4 and IPv6 link-local addresses (like
    `fe80::1:1:1:1` and `169.254.0.1`).
  * `uniquelocal`: IPv4 private addresses and IPv6 unique-local
    addresses (like `fc00:ac:1ab5:fff::1` and `192.168.0.1`).

### proxyaddr.all(req, trust)

Return all the addresses of the request. This array is ordered from
closest to furthest (i.e. `arr[0] === req.connection.remoteAddress`).

## License

[MIT](LICENSE)
