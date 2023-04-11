When you have created your first secret and policy, and generated a new token for that policy, you may want to access this key from the vault api, for use in a web app. To do this you will need a few things.

API Url: `http://servername:8200/v1/secret/data` this is the base url
Secret Path: `obsidian`
header: X-Vault-Token = "token created earlier"