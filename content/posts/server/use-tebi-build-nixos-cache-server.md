+++
title = "使用tebi作为s3,搭建nix缓存服务器"
date = 2024-11-12
+++

# 使用tebi作为s3,搭建nix缓存服务器
2024-11-12
参考
1. https://nixos-and-flakes.thiscute.world/zh/nix-store/host-your-own-binary-cache-server
## 使用tebi作为s3 provider
进入s3
创建桶your-bucket-name
创建access_key/secret_access_key

直接上传`nix-cache-info` 文件到桶的根目录，文件内容如下
```nix-cache-info
StoreDir: /nix/store
WantMassQuery: 1
Priority: 40
```
## 创建s3通行证
转到nixos上,创建`~/.aws/credentials`,内容如下
```credentials
[config-name]
aws_access_key_id=<your_access_key>
aws_secret_access_key=<your_secret_access_key>
```
这里的`<your_access_key>`和`<your_secret_access_key>`去tebi后台找

## 上传缓存
### 创建密钥对
```bash
nix key generate-secret --key-name your-want-name-1 > ~/.config/nix/secret.key
nix key convert-secret-to-public < ~/.config/nix/secret.key > ~/.config/nix/public.key
cat ~/.config/nix/public.key
# => your-want-name-1:m0J/oDlLEuG6ezc6MzmpLCN2MYjssO3NMIlr9JdxkTs=
```
> 名字是任意的，推荐后面加上版本号

### 推送存储路径到二进制缓存
先给本地存储路径签名
```bash
nix store sign --recursive --key-file ~/.config/nix/secret.key /run/current-system
```
将这些路径复制到缓存
```bash
nix copy --to 's3://your-bucket-name?profile=config-name&endpoint=s3.tebi.io' /run/current-system
```

### 添加这个缓存源
将以下内容放入 configuration.nix 或您的任何自定义 NixOS 模块中

```bash
{
  nix = {
    settings = {
      extra-substituters = [
        "https://s3.tebi.io/your-bucket-name/"
      ];
      extra-trusted-public-keys = [
        "your-want-name-1:m0J/oDlLEuG6ezc6MzmpLCN2MYjssO3NMIlr9JdxkTs="
      ];
    };
  };
}
```

参考:
https://nixos-and-flakes.thiscute.world/zh/nix-store/host-your-own-binary-cache-server
https://fzakaria.com/2020/07/15/setting-up-a-nix-s3-binary-cache.html
