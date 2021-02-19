---
title: Acesso ao NSX Edge
date: 2021-02-19 10:00:00 +0000
categories: [VMWare, NSX]
tags: [tips, edge]
---

Todas informações listadas aqui foram retiradas do [KB2149630](https://kb.vmware.com/s/article/2149630?src=vmw_so_vex_mmenk_1154) da VMWARE

Caso seja necessário acessar um edge do NSX por SSH será necessário seguir o KB da VMWare para pegar a senha do edge específico. Para isso o seguinte procedimento é necessário:

* Logar no NSX manager com a senha do usuário admin `ssh admin@<Endereço IP>` -> Nome ou IP do NSX Manager (Ex.: *nsxmgr.seu.dominio*). A senha do usuário admin foi definida na intalação do NSX Manager.

* Elevar-se para o modo privilegiado usando o comando `enable`.
* O comando `start engineer` deve ser usado para cair no bash do NSX Manager.
* Após aceitar o aviso, a senha  **IAmOnThePhoneWithTechSupport** deve ser usada.
* Para listar a senha de admin de todos os edges use:

    `/home/secureall/secureall/sem/WEB-INF/classes/./GetSpockEdgePassword.sh`
* Para listar a senha de root de todos os edges use:

    `/home/secureall/secureall/sem/WEB-INF/classes/./GetCliUserEdgePassword.sh`
```
user@linux:~$ ssh admin@nsxmgr.seu.dominio
admin@nsxmgr.seu.dominio's password: ******
nsxmgr>
nsxmgr> enable
Password: ******
nsxmgr#
nsxmgr# start engineer
Engineering Mode: The authorized NSX Manager system administrator is requesting a shell 
which is able to perform lower level unix commands/diagnostics and make changes to the 
appliance. VMware asks that you do so only in conjunction with a support call to prevent 
breaking your virtual infrastructure. Please enter the shell diagnostics string before 
proceeding.Type Exit to return to the NSX shell. Type y to continue: y
Password: IAmOnThePhoneWithTechSupport
[root@nsxmgr ~]#
[root@nsxmgr ~]# /home/secureall/secureall/sem/WEB-INF/classes/./GetSpockEdgePassword.sh edge-992
Edge root password:
	edge-992	-> pjPg3VEgOaP*L
[root@nsxnsxmgr1mgr002p ~]# /home/secureall/secureall/sem/WEB-INF/classes/./GetCliUserEdgePassword.sh edge-992
Edge cli user password:
	edge-992	-> admin/wIdq9R%IBP7C
	edge-992	-> admin/wIdq9R%IBP7C
[root@nsxmgr ~]#
```
Lembre de certificar-se de que o acesso SSH está habilitado no Edge. Isso pode ser feito direto da console do vSphere Client em . <span style="color:green">*Menu* -> *Network and Security* -> *NSX Edges*</span>.
![Edge](/assets/img/2020-02-19-edge1.png)