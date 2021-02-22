---
title: GNS3 & VMWare Player - Alterando o IP da vmnet1
date: 2021-02-22 10:00:00 +0000
categories: [GNS3]
tags: [tips]
---

Tive problemas para usar o GNS3 enquanto estava conectado à VPN de uma empresa por conflito do endereçamento interno do VMWare Player.

Para alterar o endereço da interface vmnet1 (host-only) adapter deve ter feito o seguinte:

Executar a aplicação que gerencia os arquivos do VMWare Player a partir do terminal.

`:~$ sudo vmware-netcfg`

Troque o endereço no campo indicado.

![vmare-netcfg](/assets/img/Screenshot-20210222113244-640x630.png)

Isso irá alterar os arquivos de configuração do VMWare Player que ficam em: */etc/vmware/vmnet1/dhcpd/dhcpd.conf,* esse arquivo não deve ser alterado manualmente, apenas usando o utilitário da VMWare.

Depois disso, se o GNS3 já estiver instalado, ele não reconhecerá mais a GNS3 VM e ainda ocorrerá erros na inicialização. Deve-se então abrir o diretório de configurações do GNS3, geralmente em *~/.config/GNS3/\<versão\>/* e editar o arquivo *gns3_server.conf*.

Se quiser conferir o diretório dos arquivos de configuração pode ver nas preferencias do GNS3.

![GNS3 Preferencias](/assets/img/Screenshot-20210222114911-1000x735.png)

Dentro do aquivo *gns3_server.conf* altere o *host* para o mesmo IP da interface vmnet1. 

```
[Server]
path = /usr/bin/gns3server
ubridge_path = /usr/local/bin/ubridge
host = 192.168.20.1
port = 3080
images_path = /home/mauricio_prates/GNS3/images
projects_path = /home/mauricio_prates/GNS3/projects
appliances_path = /home/mauricio_prates/GNS3/appliances
additional_images_paths =
symbols_path = /home/mauricio_prates/GNS3/symbols
configs_path = /home/mauricio_prates/GNS3/configs
report_errors = True
auto_start = True
allow_console_from_anywhere = False
auth = True
user = admin
password = worhpq0SB2IkkXfy1vV5yDc8Kk1dPuQl75jrakjpZbQfovkbfClTfDxVWlPTRZrT
protocol = http
console_start_port_range = 5000
console_end_port_range = 10000
udp_start_port_range = 10000
udp_end_port_range = 20000

[VMware]
host_type = player
vmnet_start_range = 2
vmnet_end_range = 100
block_host_traffic = False
```

Caso precise ver o endereço IP da vmnet1 use:

```
:~$ ip address show dev vmnet1
9: vmnet1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
    link/ether 00:50:56:c0:00:01 brd ff:ff:ff:ff:ff:ff
    inet 192.168.20.1/24 brd 192.168.20.255 scope global vmnet1
       valid_lft forever preferred_lft forever
    inet6 fe80::250:56ff:fec0:1/64 scope link
       valid_lft forever preferred_lft forever
```

Após isso é só fechar e abrir novamente o GNS3.