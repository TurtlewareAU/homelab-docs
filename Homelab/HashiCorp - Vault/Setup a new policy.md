Policies can be used to limit visibility of secrets, and system tools for a given user. There are many capabilities that can be applied an example for my usage is below. Here I have created a 

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