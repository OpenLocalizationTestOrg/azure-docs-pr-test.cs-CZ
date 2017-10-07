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
# <a name="how-tooinstall-mysql-on-azure"></a><span data-ttu-id="102c9-103">Jak tooinstall MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="102c9-103">How tooinstall MySQL on Azure</span></span>
<span data-ttu-id="102c9-104">V tomto článku se dozvíte, jak tooinstall a konfiguraci databáze MySQL na virtuální počítač Azure s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="102c9-104">In this article, you will learn how tooinstall and configure MySQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a><span data-ttu-id="102c9-105">Nainstalujte MySQL na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="102c9-105">Install MySQL on your virtual machine</span></span>
> [!NOTE]
> <span data-ttu-id="102c9-106">Musíte už mít na virtuálním počítači Microsoft Azure s Linuxem v toocomplete pořadí v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="102c9-106">You must already have a Microsoft Azure virtual machine running Linux in order toocomplete this tutorial.</span></span> <span data-ttu-id="102c9-107">Najdete v tématu [kurzu virtuální počítač Azure s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate a nastavení virtuálního počítače s Linuxem pomocí `mysqlnode` jako název virtuálního počítače hello a `azureuser` jako uživatel, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="102c9-107">Please see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate and set up a Linux VM with `mysqlnode` as hello VM name and `azureuser` as user before proceeding.</span></span>
> 
> 

<span data-ttu-id="102c9-108">V takovém případě 3306 port použijte jako hello MySQL portu.</span><span class="sxs-lookup"><span data-stu-id="102c9-108">In this case, use 3306 port as hello MySQL port.</span></span>  

<span data-ttu-id="102c9-109">Připojte toohello vytvořené prostřednictvím putty virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="102c9-109">Connect toohello Linux VM you created via putty.</span></span> <span data-ttu-id="102c9-110">Pokud je to hello prvním použití virtuálního počítače s Linuxem Azure, najdete v části připojení tooa virtuálního počítače s Linuxem toouse putty [zde](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="102c9-110">If this is hello first time you use Azure Linux VM, see how toouse putty connect tooa Linux VM [here](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="102c9-111">Jako příklad v tomto článku budeme používat úložiště balíčku tooinstall MySQL5.6.</span><span class="sxs-lookup"><span data-stu-id="102c9-111">We will use repository package tooinstall MySQL5.6 as an example in this article.</span></span> <span data-ttu-id="102c9-112">Ve skutečnosti MySQL5.6 má další zlepšování výkonu než MySQL5.5.</span><span class="sxs-lookup"><span data-stu-id="102c9-112">Actually, MySQL5.6 has more improvement in performance than MySQL5.5.</span></span>  <span data-ttu-id="102c9-113">Další informace [zde](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).</span><span class="sxs-lookup"><span data-stu-id="102c9-113">More information [here](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).</span></span>

### <a name="how-tooinstall-mysql56-on-ubuntu"></a><span data-ttu-id="102c9-114">Jak tooinstall MySQL5.6 na Ubuntu</span><span class="sxs-lookup"><span data-stu-id="102c9-114">How tooinstall MySQL5.6 on Ubuntu</span></span>
<span data-ttu-id="102c9-115">Virtuální počítač s Linuxem s Ubuntu z Azure použijeme sem.</span><span class="sxs-lookup"><span data-stu-id="102c9-115">We will use Linux VM with Ubuntu from Azure here.</span></span>

* <span data-ttu-id="102c9-116">Krok 1: Instalace MySQL serveru 5.6 přepínač příliš`root` uživatele:</span><span class="sxs-lookup"><span data-stu-id="102c9-116">Step 1: Install MySQL Server 5.6   Switch too`root` user:</span></span>
  
            #[azureuser@mysqlnode:~]sudo su -
  
    <span data-ttu-id="102c9-117">Mysql-server 5.6 nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="102c9-117">Install mysql-server 5.6:</span></span>
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    <span data-ttu-id="102c9-118">Během instalace, zobrazí se okno poping dialogové okno až tooask jste tooset MySQL kořenové heslo níže a musí nastavit hello heslo sem.</span><span class="sxs-lookup"><span data-stu-id="102c9-118">During installation, you will see a dialog window poping up tooask you tooset MySQL root password below, and you need set hello password here.</span></span>
  
    ![Bitové kopie](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    <span data-ttu-id="102c9-120">Vstupní hello heslo znovu tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="102c9-120">Input hello password again tooconfirm.</span></span>

    ![Bitové kopie](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* <span data-ttu-id="102c9-122">Krok 2: Server MySQL přihlášení</span><span class="sxs-lookup"><span data-stu-id="102c9-122">Step 2: Login MySQL Server</span></span>
  
    <span data-ttu-id="102c9-123">Po dokončení instalace serveru MySQL, spustí se automaticky službu MySQL.</span><span class="sxs-lookup"><span data-stu-id="102c9-123">When MySQL server installation finished, MySQL service will be started automatically.</span></span> <span data-ttu-id="102c9-124">Můžete se přihlásit MySQL Server s `root` uživatele.</span><span class="sxs-lookup"><span data-stu-id="102c9-124">You can login MySQL Server with `root` user.</span></span>
    <span data-ttu-id="102c9-125">Použijte hello níže příkaz toologin a zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="102c9-125">Use hello below command toologin and input password.</span></span>
  
             #[root@mysqlnode ~]# mysql -uroot -p
* <span data-ttu-id="102c9-126">Krok 3: Spravujte hello službou MySQL</span><span class="sxs-lookup"><span data-stu-id="102c9-126">Step 3: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="102c9-127">(a) získat stav službu MySQL</span><span class="sxs-lookup"><span data-stu-id="102c9-127">(a) Get status of MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql status
  
    <span data-ttu-id="102c9-128">(b) spustit službu MySQL</span><span class="sxs-lookup"><span data-stu-id="102c9-128">(b) Start MySQL Service</span></span>
  
             #[root@mysqlnode ~]# service mysql start
  
    <span data-ttu-id="102c9-129">(c) zastavit službu MySQL</span><span class="sxs-lookup"><span data-stu-id="102c9-129">(c) Stop MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql stop
  
    <span data-ttu-id="102c9-130">(d) restartujte službu MySQL hello</span><span class="sxs-lookup"><span data-stu-id="102c9-130">(d) Restart hello MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-tooinstall-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a><span data-ttu-id="102c9-131">Jak tooinstall MySQL na řada operačního systému Red Hat jako CentOS, Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="102c9-131">How tooinstall MySQL on Red Hat OS family like CentOS, Oracle Linux</span></span>
<span data-ttu-id="102c9-132">Virtuální počítač s Linuxem pomocí CentOS nebo Oracle Linux použijeme sem.</span><span class="sxs-lookup"><span data-stu-id="102c9-132">We will use Linux VM with CentOS or Oracle Linux here.</span></span>

* <span data-ttu-id="102c9-133">Krok 1: Přidání hello MySQL Yum úložiště přepínač příliš`root` uživatele:</span><span class="sxs-lookup"><span data-stu-id="102c9-133">Step 1: Add hello MySQL Yum repository   Switch too`root` user:</span></span>
  
            #[azureuser@mysqlnode:~]sudo su -
  
    <span data-ttu-id="102c9-134">Stáhněte a nainstalujte hello MySQL verze balíčku:</span><span class="sxs-lookup"><span data-stu-id="102c9-134">Download and install hello MySQL release package:</span></span>
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* <span data-ttu-id="102c9-135">Krok 2: Upravte následující soubor tooenable hello MySQL úložiště pro stažení balíčku MySQL5.6 hello.</span><span class="sxs-lookup"><span data-stu-id="102c9-135">Step 2: Edit below file tooenable hello MySQL repository for downloading hello MySQL5.6 package.</span></span>
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    <span data-ttu-id="102c9-136">Aktualizujte každý hodnotu toobelow tento soubor:</span><span class="sxs-lookup"><span data-stu-id="102c9-136">Update each value of this file toobelow:</span></span>
  
        \# *Enable toouse MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* <span data-ttu-id="102c9-137">Krok 3: Instalace MySQL z úložiště MySQL MySQL nainstalovat:</span><span class="sxs-lookup"><span data-stu-id="102c9-137">Step 3: Install MySQL from MySQL repository   Install MySQL:</span></span>
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    <span data-ttu-id="102c9-138">Balíček MySQL RPM a všechny související balíčky budou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="102c9-138">MySQL RPM package and all related packages will be installed.</span></span>
* <span data-ttu-id="102c9-139">Krok 4: Správa hello službou MySQL</span><span class="sxs-lookup"><span data-stu-id="102c9-139">Step 4: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="102c9-140">(a) zkontrolujte stav služby hello hello MySQL serveru:</span><span class="sxs-lookup"><span data-stu-id="102c9-140">(a) Check hello service status of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]#service mysqld status
  
    <span data-ttu-id="102c9-141">(b) zkontrolujte, zda text hello výchozí port MySQL server běží:</span><span class="sxs-lookup"><span data-stu-id="102c9-141">(b) Check whether hello default port of  MySQL server is running:</span></span>
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    <span data-ttu-id="102c9-142">(c) server databáze MySQL hello spuštění:</span><span class="sxs-lookup"><span data-stu-id="102c9-142">(c) Start hello MySQL server:</span></span>

           #[root@mysqlnode ~]#service mysqld start

    <span data-ttu-id="102c9-143">(d) zastavte hello MySQL serveru:</span><span class="sxs-lookup"><span data-stu-id="102c9-143">(d) Stop hello MySQL server:</span></span>

           #[root@mysqlnode ~]#service mysqld stop

    <span data-ttu-id="102c9-144">(e) nastavte MySQL toostart při hello systému spouštěcí up:</span><span class="sxs-lookup"><span data-stu-id="102c9-144">(e) Set MySQL toostart when hello system boot-up:</span></span>

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-tooinstall-mysql-on-suse-linux"></a><span data-ttu-id="102c9-145">Jak tooinstall MySQL v systému SUSE Linux</span><span class="sxs-lookup"><span data-stu-id="102c9-145">How tooinstall MySQL on SUSE Linux</span></span>
<span data-ttu-id="102c9-146">Virtuální počítač s Linuxem pomocí OpenSUSE použijeme sem.</span><span class="sxs-lookup"><span data-stu-id="102c9-146">We will use Linux VM with OpenSUSE here.</span></span>

* <span data-ttu-id="102c9-147">Krok 1: Stáhněte a nainstalujte MySQL Server</span><span class="sxs-lookup"><span data-stu-id="102c9-147">Step 1: Download and install MySQL Server</span></span>
  
    <span data-ttu-id="102c9-148">Přepínač příliš`root` uživatele prostřednictvím následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="102c9-148">Switch too`root` user through below command:</span></span>  
  
           #sudo su -
  
    <span data-ttu-id="102c9-149">Stáhněte a nainstalujte balíček MySQL:</span><span class="sxs-lookup"><span data-stu-id="102c9-149">Download and install MySQL package:</span></span>
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* <span data-ttu-id="102c9-150">Krok 2: Správa hello službou MySQL</span><span class="sxs-lookup"><span data-stu-id="102c9-150">Step 2: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="102c9-151">(a) zkontrolujte stav hello hello MySQL serveru:</span><span class="sxs-lookup"><span data-stu-id="102c9-151">(a) Check hello status of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]# rcmysql status
  
    <span data-ttu-id="102c9-152">(b) zkontrolujte, zda text hello výchozí port serveru MySQL hello:</span><span class="sxs-lookup"><span data-stu-id="102c9-152">(b) Check whether hello default port of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    <span data-ttu-id="102c9-153">(c) server databáze MySQL hello spuštění:</span><span class="sxs-lookup"><span data-stu-id="102c9-153">(c) Start hello MySQL server:</span></span>

           #[root@mysqlnode ~]# rcmysql start

    <span data-ttu-id="102c9-154">(d) zastavte hello MySQL serveru:</span><span class="sxs-lookup"><span data-stu-id="102c9-154">(d) Stop hello MySQL server:</span></span>

           #[root@mysqlnode ~]# rcmysql stop

    <span data-ttu-id="102c9-155">(e) nastavte MySQL toostart při hello systému spouštěcí up:</span><span class="sxs-lookup"><span data-stu-id="102c9-155">(e) Set MySQL toostart when hello system boot-up:</span></span>

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a><span data-ttu-id="102c9-156">Další krok</span><span class="sxs-lookup"><span data-stu-id="102c9-156">Next Step</span></span>
<span data-ttu-id="102c9-157">Najít další využití a informace týkající se MySQL [zde](https://www.mysql.com/).</span><span class="sxs-lookup"><span data-stu-id="102c9-157">Find more usage and information regarding MySQL [Here](https://www.mysql.com/).</span></span>

