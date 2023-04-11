Policies can be used to limit visibility of secrets, and system tools for a given user. There are many capabilities that can be applied an example for my usage is below. Here I have created a new policy by which I will connect via token to the api. The token can 
> read its own token information.
> renew its token
> revoke its token
> create|

```json
path "auth/token/lookup-self" {
    capabilities = ["read"]
}

path "auth/token/renew-self" {
    capabilities = ["update"]
}

path "auth/token/revoke-self" {
    capabilities = ["update"]
}

path "secret/data/proxmox" {
    capabilities = ["create", "read", "list"]
}
```