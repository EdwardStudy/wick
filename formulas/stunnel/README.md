Stunnel
=======

Installs stunnel and starts it at boot. Provides functions to install service specific configurations.

* --start - Also start the daemon at the end of the script.

Examples

    wickFormula stunnel

Returns nothing.


`stunnelAddConfig()`
--------------------

Installs a configuration for a specific service into stunnel.

* --name    - The name to give this config, required.
* --server  - A boolean flag, is this a server (i.e. listening) config.
* --accept  - The listening `ip:port` the port alone can be provided, in which case drop the colon (`--accept=80`). Required.
* --target  - The target of the onward tunnel, in `ip:port` format, again only the port needs to be provided. Required.
* --cert    - The path to the cert on the local machine. This function copies it to the right place. Required.
* --ca-cert - The path to the CA Cert, comment about --cert above is relevant here too. Required.
* --verify  - Can be 0, 2, 3 or 4. Explanation in the Verification section below. The default is to perform no verification.
* --delay   - Delay DNS lookup for connect option, useful when using a DNS name instead of an IP address for the onward connection.

The `--cert` and `--ca-cert` options will use files from the filesystem, not files from formulas (eg `files/` and `templates/` folders in formulas).

Verification

The --verify option can take any of the following arguments. The default is no verification.

* 0 - Request and ignore the peer certificate.
* 1 - Verify the peer certificate if present.
* 2 - Verify the peer certificate.
* 3 - Verify the peer with locally installed certificate.
* 4 - Ignore the CA chain and only verify the peer certificate.

Examples

    # Forward to a remote HTTP server without cert verification.
    stunnelAddConfig --name=remote-http --server --accept=80 \
                     --target=remote.service.consul:80 --cert=/path/to/cert \
                     --ca-cert=/path/to/ca.crt

    # Sets up a server that listens on 127.0.0.1:5333 and will forward
    # connections to example.com:5444.
    stunnelAddConfig --server \
                     --name="foo" \
                     --accept="127.0.0.1:5333" \
                     --target="example.com:5444" \
                     --cert="/vagrant/files/foo.crt" \
                     --ca-cert="/vagrant/files/ca.crt" \
                     --verify=3 \
                     --delay

Returns nothing.


