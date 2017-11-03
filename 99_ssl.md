# Certificate Management

The `config/ssl` directory contains all files related to SSL. The name of the domain (FQDN) is always taken from the file names (i.e. `example.com.key`).

In the first step you must generate the signing request. A signing key is automatically generated when needed.
```
$ make config/ssl/example.com.csr
```

Once you receive the signed certificate it should be saved under `config/ssl/example.com.crt`.
