```
#Install jq
sudo yum install jq -y

#Install vault
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install vault
```




config.hcl
```
tee config.hcl <<EOF
ui = true
disable_mlock = true

storage "raft" {
  path    = "./vault/data"
  node_id = "node1"
}

listener "tcp" {
  address     = "0.0.0.0:8200"
  tls_disable = "true"
}

api_addr = "http://<Public IP>:8200"
cluster_addr = "https://127.0.0.1:8201"
EOF
```

`mkdir -p ./vault/data`


`vault server -config=config.hcl`


https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-ui#start-web-ui

generic Username & Password
Enable Method



vault login -address=http://vault.minchan.link:8200


```
vault write -address=[Domain or IP] \
totp/keys/[totp key] \
generate=true \
issuer=[User name] \
algorithm=SHA256 \
account_name=[User Email]
```


정책 추가
enfocement 포함 추가

error 2개 Passcode -> Enterprise

정리 노트
vault login -address=http://vault.minchan.link:8200 -method=userpass username=kmc0722

---
vault write -address=http://vault.minchan.link:8200 sys/mfa/method/totp/my_totp issuer=Vault key_size=30 algorithm=SHA1


???
curl \
    --header "X-Vault-Token: [token]" \
    --request GET \
    http://vault.minchan.link:8200/v1/sys/mfa/method/totp/MFA_key/generate



2f66330e-3280-0041-5c05-0b6fe8316d0b

