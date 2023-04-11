Policies can be used to limit visibility of secrets, and system tools for a given user. There are many capabilities that can be applied an example for my usage is below. Here I have created a new policy by which I will connect via token to the api. 

To add a new policy click on the Policies Link and you are shown all of the ACL Policies visible within the Vault.

![](./img/vault-policies-page.png)

From here we will click on the Create ACL Policy + button to add a new policy

![](./img/vault-policiy-obsidian-example.png)

To complete the process we click on the create policy button to lock down our secret to root, and anyone who will have the obsidian policy.

![](./img/vault-policies-updated-obsidian.png)

The token can:
> read its own token information.
> renew its token
> revoke its token
> create|read|list anything under the secret - Proxmox

Below is the complete polic in HCL format.

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

path "secret/data/obsidian" {
    capabilities = ["create", "read", "list"]
}
```

Creating a new Token for the policy above.

I used the auth token create end point which is `http://servername:8200/v1/auth/token/create` from here I provided the json body of { "policies": "obsidian"}. The response gave me a client_token which will be used to login. The accessor is used for further access to other parts of Vault.

![](postman-create-token.png)

Copying the client_token and logging into the ui you can find the secrets. Because this token does not have the appropriate information to list all the secrets. When login as this user via the ui, and visiting the secrets page. I see no secrets visible. If you type in obsidian into the secret path we can see the secrets we are after and only those. 

![](vault-obsidian-search.png)

Clicking on the view secret will load the secret, only if we have visibility to it.

![](vault-obsidian-visible.png)

Having two users logged in we can see the difference. 

Below you can see that the root user can see a lot more then the proxmox user can see. Having this separation works well as you know the proxmox user is not able to edit,update any other secrets in the vault.

![](./img/Pasted%20image%2020230411194614.png)
