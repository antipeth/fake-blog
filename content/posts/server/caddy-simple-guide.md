+++
title = "Debian/NixOS使用caddy实现简单反代"
date = 2024-12-19
+++
# Debian/NixOS使用caddy实现简单反代
2024-12-19
参考
1. https://caddyserver.com/docs/quick-starts/reverse-proxy
2. https://wiki.nixos.org/wiki/Caddy

## debian安装caddy
官方教程https://caddyserver.com/docs/install#debian-ubuntu-raspbian
```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

## 使用Caddyfile
编辑`/etc/caddy/Caddyfile`
删除原来的`:80`配置(全删了)，
添加如下配置
```Caddyfile
example1.com {
	log {
		output file /var/log/caddy/access-example1.com.log
	}

	reverse_proxy localhost:9000 {
		header_up Host {upstream_hostport}
	}
}

example2.com  {
	log {
		output file /var/log/caddy/access-example2.com.log
	}

	reverse_proxy ip:port {
		header_up Host {upstream_hostport}
	}
}

```

## 使用命令行反代
```bash
caddy reverse-proxy \
	--from example.com \
	--to localhost:9000 \
	--change-host-header

```
tls证书可以自动获取

# nixos使用caddy反代

```caddy.nix
{
  config,
  pkgs,
  host,
  ...
}:
{
  services.caddy = {
    enable = true;
    virtualHosts."example1.com"={
    extraConfig = ''
      reverse_proxy localhost:9000 {
          header_up Host {upstream_hostport}
      }
    '';
    };
    virtualHosts."example2.com"={
    extraConfig = ''
      reverse_proxy localhost:9001 {
          header_up Host {upstream_hostport}
      }
    '';
    };
  };

}
```

然后，在配置文件中引用`caddy.nix`

```
{
  config,
  pkgs,
  host,
  ...
}:
{
  ...
  imports = [
    caddy.nix
  ];
  ...
}
```
然后，重构系统
```bash
sudo nixos-rebuild switch
```
或者
使用flake
```bash
sudo nixos-rebuild switch --flake .#"$HOSTNAME"
```

访问域名，确认反代成功
