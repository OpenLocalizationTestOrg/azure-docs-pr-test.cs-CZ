---
title: "Nastavení databáze MySQL na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Informace o instalaci zásobníku MySQL na virtuální počítač s Linuxem (Ubuntu nebo RedHat rodiny operačního systému) v Azure"
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
ms.openlocfilehash: 0ee70bda954cf0a193d43b5b47702e7b2c37844d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-mysql-on-azure"></a><span data-ttu-id="5f3e7-103">Jak nainstalovat MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="5f3e7-103">How to install MySQL on Azure</span></span>
<span data-ttu-id="5f3e7-104">V tomto článku se dozvíte, jak nainstalovat a nakonfigurovat MySQL na virtuální počítač Azure s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-104">In this article, you will learn how to install and configure MySQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a><span data-ttu-id="5f3e7-105">Nainstalujte MySQL na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="5f3e7-105">Install MySQL on your virtual machine</span></span>
> [!NOTE]
> <span data-ttu-id="5f3e7-106">Musíte už mít virtuálního počítače Microsoft Azure se systémem Linux k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-106">You must already have a Microsoft Azure virtual machine running Linux in order to complete this tutorial.</span></span> <span data-ttu-id="5f3e7-107">Najdete v tématu [kurzu virtuální počítač Azure s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) k vytvoření a nastavení virtuálního počítače s Linuxem pomocí `mysqlnode` jako název virtuálního počítače a `azureuser` jako uživatel, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-107">Please see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to create and set up a Linux VM with `mysqlnode` as the VM name and `azureuser` as user before proceeding.</span></span>
> 
> 

<span data-ttu-id="5f3e7-108">V takovém případě 3306 port použijte jako MySQL port.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-108">In this case, use 3306 port as the MySQL port.</span></span>  

<span data-ttu-id="5f3e7-109">Připojte k systému Linux vytvořené prostřednictvím putty virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-109">Connect to the Linux VM you created via putty.</span></span> <span data-ttu-id="5f3e7-110">Pokud je při prvním použití virtuálního počítače s Linuxem Azure, naleznete v části použití klienta putty připojit k virtuální počítač s Linuxem [zde](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5f3e7-110">If this is the first time you use Azure Linux VM, see how to use putty connect to a Linux VM [here](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="5f3e7-111">K instalaci MySQL5.6 jako příklad v tomto článku budeme používat úložiště balíčku.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-111">We will use repository package to install MySQL5.6 as an example in this article.</span></span> <span data-ttu-id="5f3e7-112">Ve skutečnosti MySQL5.6 má další zlepšování výkonu než MySQL5.5.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-112">Actually, MySQL5.6 has more improvement in performance than MySQL5.5.</span></span>  <span data-ttu-id="5f3e7-113">Další informace [zde](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).</span><span class="sxs-lookup"><span data-stu-id="5f3e7-113">More information [here](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).</span></span>

### <a name="how-to-install-mysql56-on-ubuntu"></a><span data-ttu-id="5f3e7-114">Postup instalace MySQL5.6 v Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5f3e7-114">How to install MySQL5.6 on Ubuntu</span></span>
<span data-ttu-id="5f3e7-115">Virtuální počítač s Linuxem s Ubuntu z Azure použijeme sem.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-115">We will use Linux VM with Ubuntu from Azure here.</span></span>

* <span data-ttu-id="5f3e7-116">Krok 1: Instalace MySQL serveru 5.6 přepínač tak, aby `root` uživatele:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-116">Step 1: Install MySQL Server 5.6   Switch to `root` user:</span></span>
  
            #[azureuser@mysqlnode:~]sudo su -
  
    <span data-ttu-id="5f3e7-117">Mysql-server 5.6 nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-117">Install mysql-server 5.6:</span></span>
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    <span data-ttu-id="5f3e7-118">Během instalace zobrazí se dialogové okno okno poping než zeptejte se, že vám umožní nastavit MySQL kořenové heslo níže a můžete potřebovat nastavit heslo sem.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-118">During installation, you will see a dialog window poping up to ask you to set MySQL root password below, and you need set the password here.</span></span>
  
    ![Bitové kopie](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    <span data-ttu-id="5f3e7-120">Zadejte heslo znovu pro potvrzení.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-120">Input the password again to confirm.</span></span>

    ![Bitové kopie](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* <span data-ttu-id="5f3e7-122">Krok 2: Server MySQL přihlášení</span><span class="sxs-lookup"><span data-stu-id="5f3e7-122">Step 2: Login MySQL Server</span></span>
  
    <span data-ttu-id="5f3e7-123">Po dokončení instalace serveru MySQL, spustí se automaticky službu MySQL.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-123">When MySQL server installation finished, MySQL service will be started automatically.</span></span> <span data-ttu-id="5f3e7-124">Můžete se přihlásit MySQL Server s `root` uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-124">You can login MySQL Server with `root` user.</span></span>
    <span data-ttu-id="5f3e7-125">Použití níže příkaz heslo k přihlášení a vstup.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-125">Use the below command to login and input password.</span></span>
  
             #[root@mysqlnode ~]# mysql -uroot -p
* <span data-ttu-id="5f3e7-126">Krok 3: Spravujte spuštěnou službu MySQL</span><span class="sxs-lookup"><span data-stu-id="5f3e7-126">Step 3: Manage the running MySQL service</span></span>
  
    <span data-ttu-id="5f3e7-127">(a) získat stav službu MySQL</span><span class="sxs-lookup"><span data-stu-id="5f3e7-127">(a) Get status of MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql status
  
    <span data-ttu-id="5f3e7-128">(b) spustit službu MySQL</span><span class="sxs-lookup"><span data-stu-id="5f3e7-128">(b) Start MySQL Service</span></span>
  
             #[root@mysqlnode ~]# service mysql start
  
    <span data-ttu-id="5f3e7-129">(c) zastavit službu MySQL</span><span class="sxs-lookup"><span data-stu-id="5f3e7-129">(c) Stop MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql stop
  
    <span data-ttu-id="5f3e7-130">(d) restartujte službu MySQL</span><span class="sxs-lookup"><span data-stu-id="5f3e7-130">(d) Restart the MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a><span data-ttu-id="5f3e7-131">Postup instalace MySQL Red Hat operačních systémů jako CentOS, Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="5f3e7-131">How to install MySQL on Red Hat OS family like CentOS, Oracle Linux</span></span>
<span data-ttu-id="5f3e7-132">Virtuální počítač s Linuxem pomocí CentOS nebo Oracle Linux použijeme sem.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-132">We will use Linux VM with CentOS or Oracle Linux here.</span></span>

* <span data-ttu-id="5f3e7-133">Krok 1: Přidání úložiště MySQL Yum přepínače k `root` uživatele:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-133">Step 1: Add the MySQL Yum repository   Switch to `root` user:</span></span>
  
            #[azureuser@mysqlnode:~]sudo su -
  
    <span data-ttu-id="5f3e7-134">Stáhněte a nainstalujte balíček MySQL verze:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-134">Download and install the MySQL release package:</span></span>
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* <span data-ttu-id="5f3e7-135">Krok 2: Upravte následující soubor povolit úložiště MySQL pro stažení balíčku MySQL5.6.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-135">Step 2: Edit below file to enable the MySQL repository for downloading the MySQL5.6 package.</span></span>
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    <span data-ttu-id="5f3e7-136">Aktualizujte každý hodnotu tohoto souboru do níže:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-136">Update each value of this file to below:</span></span>
  
        \# *Enable to use MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* <span data-ttu-id="5f3e7-137">Krok 3: Instalace MySQL z úložiště MySQL MySQL nainstalovat:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-137">Step 3: Install MySQL from MySQL repository   Install MySQL:</span></span>
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    <span data-ttu-id="5f3e7-138">Balíček MySQL RPM a všechny související balíčky budou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-138">MySQL RPM package and all related packages will be installed.</span></span>
* <span data-ttu-id="5f3e7-139">Krok 4: Správa spuštěnou službu MySQL</span><span class="sxs-lookup"><span data-stu-id="5f3e7-139">Step 4: Manage the running MySQL service</span></span>
  
    <span data-ttu-id="5f3e7-140">(a) zkontrolujte stav služby MySQL serveru:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-140">(a) Check the service status of the MySQL server:</span></span>
  
           #[root@mysqlnode ~]#service mysqld status
  
    <span data-ttu-id="5f3e7-141">(b) zkontrolujte, zda je spuštěn výchozí port MySQL server:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-141">(b) Check whether the default port of  MySQL server is running:</span></span>
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    <span data-ttu-id="5f3e7-142">(c) spusťte server, MySQL:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-142">(c) Start the MySQL server:</span></span>

           #[root@mysqlnode ~]#service mysqld start

    <span data-ttu-id="5f3e7-143">(d) zastavení MySQL serveru:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-143">(d) Stop the MySQL server:</span></span>

           #[root@mysqlnode ~]#service mysqld stop

    <span data-ttu-id="5f3e7-144">(e) MySQL sady při spuštění systému spouštěcí up:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-144">(e) Set MySQL to start when the system boot-up:</span></span>

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-to-install-mysql-on-suse-linux"></a><span data-ttu-id="5f3e7-145">Postup instalace databáze MySQL na SUSE Linux</span><span class="sxs-lookup"><span data-stu-id="5f3e7-145">How to install MySQL on SUSE Linux</span></span>
<span data-ttu-id="5f3e7-146">Virtuální počítač s Linuxem pomocí OpenSUSE použijeme sem.</span><span class="sxs-lookup"><span data-stu-id="5f3e7-146">We will use Linux VM with OpenSUSE here.</span></span>

* <span data-ttu-id="5f3e7-147">Krok 1: Stáhněte a nainstalujte MySQL Server</span><span class="sxs-lookup"><span data-stu-id="5f3e7-147">Step 1: Download and install MySQL Server</span></span>
  
    <span data-ttu-id="5f3e7-148">Přepnout na `root` uživatele prostřednictvím následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-148">Switch to `root` user through below command:</span></span>  
  
           #sudo su -
  
    <span data-ttu-id="5f3e7-149">Stáhněte a nainstalujte balíček MySQL:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-149">Download and install MySQL package:</span></span>
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* <span data-ttu-id="5f3e7-150">Krok 2: Správa spuštěnou službu MySQL</span><span class="sxs-lookup"><span data-stu-id="5f3e7-150">Step 2: Manage the running MySQL service</span></span>
  
    <span data-ttu-id="5f3e7-151">(a) zkontrolujte stav serveru MySQL:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-151">(a) Check the status of the MySQL server:</span></span>
  
           #[root@mysqlnode ~]# rcmysql status
  
    <span data-ttu-id="5f3e7-152">(b) zkontrolujte, zda výchozí port serveru MySQL:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-152">(b) Check whether the default port of the MySQL server:</span></span>
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    <span data-ttu-id="5f3e7-153">(c) spusťte server, MySQL:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-153">(c) Start the MySQL server:</span></span>

           #[root@mysqlnode ~]# rcmysql start

    <span data-ttu-id="5f3e7-154">(d) zastavení MySQL serveru:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-154">(d) Stop the MySQL server:</span></span>

           #[root@mysqlnode ~]# rcmysql stop

    <span data-ttu-id="5f3e7-155">(e) MySQL sady při spuštění systému spouštěcí up:</span><span class="sxs-lookup"><span data-stu-id="5f3e7-155">(e) Set MySQL to start when the system boot-up:</span></span>

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a><span data-ttu-id="5f3e7-156">Další krok</span><span class="sxs-lookup"><span data-stu-id="5f3e7-156">Next Step</span></span>
<span data-ttu-id="5f3e7-157">Najít další využití a informace týkající se MySQL [zde](https://www.mysql.com/).</span><span class="sxs-lookup"><span data-stu-id="5f3e7-157">Find more usage and information regarding MySQL [Here](https://www.mysql.com/).</span></span>

