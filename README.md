# Docker Registry Htpasswd

This provides a ready to use setup for a docker registry running with htpasswd
authentication.

You only need to replace the `certs/registry.crt` and `certs/registry.key`
with your own certificate and key file and generate your own `bcrypt` username
and password combinations and put them in `registry/conf/htpasswd` using:

```bash
htpasswd -B registry/conf/htpasswd jane
New password:
Re-type new password: 
Adding password for user jane
```

By default these users are configured:

```
john:doe
foo:bar
healthchecker:healthchecker
```

You can remove any of these users **but** if you remove the `healthchecker`
you have to adjust the `health` section in `registry/conf/config.yml`.

# Logging in

You have to call `docker login 0.0.0.0:443` with whatever URL you have
configure your registry to run on.

If you don't want to interactively enter a username and password, you can
store it in `~/.docker/config.json` like so:

```json
{
  "auths": {
    "0.0.0.0:443": {
      "auth": "Zm9vOmJhcg==",
        "email": "foo@bar.com"
    }
  }
}
```

# Test Docker v2 HTTP API

To manaully test the Docker v2 HTTP API you can run this command:

```
curl -k -v -H "Authorization: Basic Zm9vOmJhcg==" https://0.0.0.0:443/v2/
```

It should give you an `HTTP 200 OK`.

Have fun!
