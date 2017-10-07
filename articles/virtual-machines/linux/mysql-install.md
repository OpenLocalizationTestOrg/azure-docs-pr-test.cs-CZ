---
title: "aaaSet zálohu databáze MySQL na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello MySQL zásobníku na virtuální počítač s Linuxem (Ubuntu nebo RedHat rodiny operačního systému) v Azure"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: e47d0de7f0eb5bb873ad20e4bc35f1b5f8d33bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-mysql-on-azure"></a>Jak tooinstall MySQL v Azure
V tomto článku se dozvíte, jak tooinstall a konfiguraci databáze MySQL na virtuální počítač Azure s Linuxem.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>Nainstalujte MySQL na virtuálním počítači
> [!NOTE]
> Musíte už mít na virtuálním počítači Microsoft Azure s Linuxem v toocomplete pořadí v tomto kurzu. Najdete v tématu [kurzu virtuální počítač Azure s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate a nastavení virtuálního počítače s Linuxem pomocí `mysqlnode` jako název virtuálního počítače hello a `azureuser` jako uživatel, než budete pokračovat.
> 
> 

V takovém případě 3306 port použijte jako hello MySQL portu.  

Připojte toohello vytvořené prostřednictvím putty virtuálního počítače s Linuxem. Pokud je to hello prvním použití virtuálního počítače s Linuxem Azure, najdete v části připojení tooa virtuálního počítače s Linuxem toouse putty [zde](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Jako příklad v tomto článku budeme používat úložiště balíčku tooinstall MySQL5.6. Ve skutečnosti MySQL5.6 má další zlepšování výkonu než MySQL5.5.  Další informace [zde](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

### <a name="how-tooinstall-mysql56-on-ubuntu"></a>Jak tooinstall MySQL5.6 na Ubuntu
Virtuální počítač s Linuxem s Ubuntu z Azure použijeme sem.

* Krok 1: Instalace MySQL serveru 5.6 přepínač příliš`root` uživatele:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Mysql-server 5.6 nainstalujte:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    Během instalace, zobrazí se okno poping dialogové okno až tooask jste tooset MySQL kořenové heslo níže a musí nastavit hello heslo sem.
  
    ![Bitové kopie](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Vstupní hello heslo znovu tooconfirm.

    ![Bitové kopie](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* Krok 2: Server MySQL přihlášení
  
    Po dokončení instalace serveru MySQL, spustí se automaticky službu MySQL. Můžete se přihlásit MySQL Server s `root` uživatele.
    Použijte hello níže příkaz toologin a zadání hesla.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* Krok 3: Spravujte hello službou MySQL
  
    (a) získat stav službu MySQL
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) spustit službu MySQL
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) zastavit službu MySQL
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) restartujte službu MySQL hello
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-tooinstall-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Jak tooinstall MySQL na řada operačního systému Red Hat jako CentOS, Oracle Linux
Virtuální počítač s Linuxem pomocí CentOS nebo Oracle Linux použijeme sem.

* Krok 1: Přidání hello MySQL Yum úložiště přepínač příliš`root` uživatele:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Stáhněte a nainstalujte hello MySQL verze balíčku:
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* Krok 2: Upravte následující soubor tooenable hello MySQL úložiště pro stažení balíčku MySQL5.6 hello.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    Aktualizujte každý hodnotu toobelow tento soubor:
  
        \# *Enable toouse MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* Krok 3: Instalace MySQL z úložiště MySQL MySQL nainstalovat:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    Balíček MySQL RPM a všechny související balíčky budou nainstalovány.
* Krok 4: Správa hello službou MySQL
  
    (a) zkontrolujte stav služby hello hello MySQL serveru:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) zkontrolujte, zda text hello výchozí port MySQL server běží:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) server databáze MySQL hello spuštění:

           #[root@mysqlnode ~]#service mysqld start

    (d) zastavte hello MySQL serveru:

           #[root@mysqlnode ~]#service mysqld stop

    (e) nastavte MySQL toostart při hello systému spouštěcí up:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-tooinstall-mysql-on-suse-linux"></a>Jak tooinstall MySQL v systému SUSE Linux
Virtuální počítač s Linuxem pomocí OpenSUSE použijeme sem.

* Krok 1: Stáhněte a nainstalujte MySQL Server
  
    Přepínač příliš`root` uživatele prostřednictvím následující příkaz:  
  
           #sudo su -
  
    Stáhněte a nainstalujte balíček MySQL:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* Krok 2: Správa hello službou MySQL
  
    (a) zkontrolujte stav hello hello MySQL serveru:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) zkontrolujte, zda text hello výchozí port serveru MySQL hello:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) server databáze MySQL hello spuštění:

           #[root@mysqlnode ~]# rcmysql start

    (d) zastavte hello MySQL serveru:

           #[root@mysqlnode ~]# rcmysql stop

    (e) nastavte MySQL toostart při hello systému spouštěcí up:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>Další krok
Najít další využití a informace týkající se MySQL [zde](https://www.mysql.com/).

