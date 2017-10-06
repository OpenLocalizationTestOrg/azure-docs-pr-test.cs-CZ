---
title: aaaRun MariaDB (MySQL) clusteru v Azure | Microsoft Docs
description: "Vytvoření MariaDB + Galera MySQL clusteru na virtuálních počítačích Azure"
services: virtual-machines-linux
documentationcenter: 
author: sabbour
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: f9a4d6c45d76478a8a3526b407c7bbe6aeb40423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a><span data-ttu-id="b2c3d-103">MariaDB (MySQL) clusteru: kurz pro Azure</span><span class="sxs-lookup"><span data-stu-id="b2c3d-103">MariaDB (MySQL) cluster: Azure tutorial</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b2c3d-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="b2c3d-105">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-105">This article covers hello classic deployment model.</span></span> <span data-ttu-id="b2c3d-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model hello Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-106">Microsoft recommends that most new deployments use hello Azure Resource Manager model.</span></span>

> [!NOTE]
> <span data-ttu-id="b2c3d-107">MariaDB Enterprise clusteru je teď dostupná v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-107">MariaDB Enterprise cluster is now available in hello Azure Marketplace.</span></span> <span data-ttu-id="b2c3d-108">Nová nabídka Hello automaticky nasadí cluster MariaDB Galera na Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-108">hello new offering will automatically deploy a MariaDB Galera cluster on Azure Resource Manager.</span></span> <span data-ttu-id="b2c3d-109">Měli byste použít novou nabídku hello z [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span><span class="sxs-lookup"><span data-stu-id="b2c3d-109">You should use hello new offering from [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span></span>
>
>

<span data-ttu-id="b2c3d-110">Tento článek ukazuje, jak toocreate více hlavní [Galera](http://galeracluster.com/products/) cluster [MariaDBs](https://mariadb.org/en/about/) (robustní, škálovatelnou a spolehlivé drop-in nahrazení pro databázi MySQL) toowork v prostředí s vysokou dostupností v Azure virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-110">This article shows you how toocreate a multi-Master [Galera](http://galeracluster.com/products/) cluster of [MariaDBs](https://mariadb.org/en/about/) (a robust, scalable, and reliable drop-in replacement for MySQL) toowork in a highly available environment on Azure virtual machines.</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="b2c3d-111">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="b2c3d-111">Architecture overview</span></span>
<span data-ttu-id="b2c3d-112">Tento článek popisuje, jak toocomplete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b2c3d-112">This article describes how toocomplete hello following steps:</span></span>

- <span data-ttu-id="b2c3d-113">Vytvořte tři uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-113">Create a three-node cluster.</span></span>
- <span data-ttu-id="b2c3d-114">Samostatné hello datových disků z hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-114">Separate hello data disks from hello OS disk.</span></span>
- <span data-ttu-id="b2c3d-115">Vytvoření hello datové disky v tooincrease RAID-0 nebo rozdělená nastavení IOPS.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-115">Create hello data disks in RAID-0/striped setting tooincrease IOPS.</span></span>
- <span data-ttu-id="b2c3d-116">Funkci Vyrovnávání zatížení Azure toobalance hello zatížení pro hello tři uzly.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-116">Use Azure Load Balancer toobalance hello load for hello three nodes.</span></span>
- <span data-ttu-id="b2c3d-117">toominimize opakovaných fungovat, vytvořte image virtuálního počítače, který obsahuje MariaDB + Galera a použít ho toocreate hello jiných clusteru virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-117">toominimize repetitive work, create a VM image that contains MariaDB + Galera and use it toocreate hello other cluster VMs.</span></span>

![Architektura systému](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> <span data-ttu-id="b2c3d-119">Toto téma používá hello [rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) nástroje, tak zkontrolujte, zda toodownload je a jejich připojení pokyny podle toohello tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-119">This topic uses hello [Azure CLI](../../../cli-install-nodejs.md) tools, so make sure toodownload them and connect them tooyour Azure subscription according toohello instructions.</span></span> <span data-ttu-id="b2c3d-120">Pokud potřebujete příkazy toohello referenční dokumentace, která je k dispozici v hello příkazového řádku Azure CLI, přečtěte si hello [reference k příkazům rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="b2c3d-120">If you need a reference toohello commands available in hello Azure CLI, see hello [Azure CLI command reference](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="b2c3d-121">Budete také potřebovat příliš[vytvoření klíče SSH pro ověřování] a poznamenejte si umístění souboru .pem hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-121">You will also need too[create an SSH key for authentication] and make note of hello .pem file location.</span></span>
>
>

## <a name="create-hello-template"></a><span data-ttu-id="b2c3d-122">Vytvoření šablony hello</span><span class="sxs-lookup"><span data-stu-id="b2c3d-122">Create hello template</span></span>
### <a name="infrastructure"></a><span data-ttu-id="b2c3d-123">Infrastruktura</span><span class="sxs-lookup"><span data-stu-id="b2c3d-123">Infrastructure</span></span>
1. <span data-ttu-id="b2c3d-124">Vytvořte na skupinu vztahů toohold hello prostředky společně.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-124">Create an affinity group toohold hello resources together.</span></span>

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. <span data-ttu-id="b2c3d-125">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-125">Create a virtual network.</span></span>

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. <span data-ttu-id="b2c3d-126">Vytvořte všechny naše disky toohost účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-126">Create a storage account toohost all our disks.</span></span> <span data-ttu-id="b2c3d-127">Více než 40 vytíženou disky by neměly umístit na hello stejné tooavoid účet úložiště nedosáhli limitu účet úložiště IOPS hello 20 000.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-127">You shouldn't place more than 40 heavily used disks on hello same storage account tooavoid hitting hello 20,000 IOPS storage account limit.</span></span> <span data-ttu-id="b2c3d-128">V takovém případě jste dobře nižší než toto omezení, takže všechno, co budete uložit na stejný účet pro jednoduchost hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-128">In this case, you're well below that limit, so you'll store everything on hello same account for simplicity.</span></span>

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. <span data-ttu-id="b2c3d-129">Najít název hello hello CentOS 7 bitovou kopii virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-129">Find hello name of hello CentOS 7 virtual machine image.</span></span>

        azure vm image list | findstr CentOS
   <span data-ttu-id="b2c3d-130">výstup Hello se něco podobného jako `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-130">hello output will be something like `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span></span>

   <span data-ttu-id="b2c3d-131">Použijte tento název v hello následující krok.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-131">Use that name in hello following step.</span></span>
5. <span data-ttu-id="b2c3d-132">Vytvořit šablonu virtuálního počítače hello a nahraďte /path/to/key.pem hello cestu, kde je uložený hello vygenerovat klíč SSH .pem.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-132">Create hello VM template and replace /path/to/key.pem with hello path where you stored hello generated .pem SSH key.</span></span>

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. <span data-ttu-id="b2c3d-133">Připojte čtyři 500 GB dat toohello disky virtuálních počítačů pro použití v konfiguraci RAID hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-133">Attach four 500-GB data disks toohello VM for use in hello RAID configuration.</span></span>

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. <span data-ttu-id="b2c3d-134">Použití SSH toosign v toohello šablony virtuálního počítače, který jste vytvořili na mariadbhatemplate.cloudapp.net:22 a připojte se pomocí soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-134">Use SSH toosign in toohello template VM that you created at mariadbhatemplate.cloudapp.net:22, and connect by using your private key.</span></span>

### <a name="software"></a><span data-ttu-id="b2c3d-135">Software</span><span class="sxs-lookup"><span data-stu-id="b2c3d-135">Software</span></span>
1. <span data-ttu-id="b2c3d-136">Získejte kořenový hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-136">Get hello root.</span></span>

        sudo su

2. <span data-ttu-id="b2c3d-137">Instalace podpory RAID:</span><span class="sxs-lookup"><span data-stu-id="b2c3d-137">Install RAID support:</span></span>

    <span data-ttu-id="b2c3d-138">a.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-138">a.</span></span> <span data-ttu-id="b2c3d-139">Nainstalujte mdadm.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-139">Install mdadm.</span></span>

              yum install mdadm

    <span data-ttu-id="b2c3d-140">b.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-140">b.</span></span> <span data-ttu-id="b2c3d-141">Vytvořte konfiguraci 0/stripe hello EXT4 systémem souborů.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-141">Create hello RAID0/stripe configuration with an EXT4 file system.</span></span>

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    <span data-ttu-id="b2c3d-142">c.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-142">c.</span></span> <span data-ttu-id="b2c3d-143">Vytvoření adresáře hello přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-143">Create hello mount point directory.</span></span>

              mkdir /mnt/data
    <span data-ttu-id="b2c3d-144">d.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-144">d.</span></span> <span data-ttu-id="b2c3d-145">Načtěte hello UUID hello nově vytvořený RAID zařízení.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-145">Retrieve hello UUID of hello newly created RAID device.</span></span>

              blkid | grep /dev/md0
    <span data-ttu-id="b2c3d-146">e.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-146">e.</span></span> <span data-ttu-id="b2c3d-147">Upravte /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-147">Edit /etc/fstab.</span></span>

              vi /etc/fstab
    <span data-ttu-id="b2c3d-148">f.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-148">f.</span></span> <span data-ttu-id="b2c3d-149">Přidat hello zařízení tooenable automatické připojení na restartování, nahraďte hodnotou hello hello UUID získat z předchozích hello **blkid** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-149">Add hello device tooenable auto mounting on reboot, replacing hello UUID with hello value obtained from hello previous **blkid** command.</span></span>

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    <span data-ttu-id="b2c3d-150">g.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-150">g.</span></span> <span data-ttu-id="b2c3d-151">Připojte nový oddíl hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-151">Mount hello new partition.</span></span>

              mount /mnt/data

3. <span data-ttu-id="b2c3d-152">Nainstalujte MariaDB.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-152">Install MariaDB.</span></span>

    <span data-ttu-id="b2c3d-153">a.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-153">a.</span></span> <span data-ttu-id="b2c3d-154">Vytvořte soubor MariaDB.repo hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-154">Create hello MariaDB.repo file.</span></span>

                vi /etc/yum.repos.d/MariaDB.repo

    <span data-ttu-id="b2c3d-155">b.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-155">b.</span></span> <span data-ttu-id="b2c3d-156">Zadejte soubor úložišti hello s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="b2c3d-156">Fill hello repo file with hello following content:</span></span>

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    <span data-ttu-id="b2c3d-157">c.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-157">c.</span></span> <span data-ttu-id="b2c3d-158">tooavoid konfliktů, odeberte existující operátory a mariadb knihovny.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-158">tooavoid conflicts, remove existing postfix and mariadb-libs.</span></span>

           yum remove postfix mariadb-libs-*
    <span data-ttu-id="b2c3d-159">d.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-159">d.</span></span> <span data-ttu-id="b2c3d-160">Nainstalujte MariaDB s Galera.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-160">Install MariaDB with Galera.</span></span>

           yum install MariaDB-Galera-server MariaDB-client galera

4. <span data-ttu-id="b2c3d-161">Přesuňte hello MySQL dat adresáře toohello RAID bloku zařízení.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-161">Move hello MySQL data directory toohello RAID block device.</span></span>

    <span data-ttu-id="b2c3d-162">a.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-162">a.</span></span> <span data-ttu-id="b2c3d-163">Zkopírujte aktuální adresář MySQL hello do nového umístění a odebrat adresář staré hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-163">Copy hello current MySQL directory into its new location and remove hello old directory.</span></span>

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    <span data-ttu-id="b2c3d-164">b.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-164">b.</span></span> <span data-ttu-id="b2c3d-165">Nastavte oprávnění pro nový adresář hello odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-165">Set permissions for hello new directory accordingly.</span></span>

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    <span data-ttu-id="b2c3d-166">c.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-166">c.</span></span> <span data-ttu-id="b2c3d-167">Vytvořte symlink, který odkazuje hello staré toohello nové umístění adresáře na hello RAID oddílu.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-167">Create a symlink that points hello old directory toohello new location on hello RAID partition.</span></span>

           ln -s /mnt/data/mysql /var/lib/mysql

5. <span data-ttu-id="b2c3d-168">Protože [SELinux naruší operace clusteru hello](http://galeracluster.com/documentation-webpages/configuration.html#selinux), je nutné toodisable pro hello aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-168">Because [SELinux interferes with hello cluster operations](http://galeracluster.com/documentation-webpages/configuration.html#selinux), it is necessary toodisable it for hello current session.</span></span> <span data-ttu-id="b2c3d-169">Upravit `/etc/selinux/config` toodisable pro následné restartování.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-169">Edit `/etc/selinux/config` toodisable it for subsequent restarts.</span></span>

            setenforce 0

            then editing `/etc/selinux/config` tooset `SELINUX=permissive`
6. <span data-ttu-id="b2c3d-170">Ověření spuštění MySQL.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-170">Validate MySQL runs.</span></span>

   <span data-ttu-id="b2c3d-171">a.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-171">a.</span></span> <span data-ttu-id="b2c3d-172">Spusťte MySQL.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-172">Start MySQL.</span></span>

           service mysql start
   <span data-ttu-id="b2c3d-173">b.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-173">b.</span></span> <span data-ttu-id="b2c3d-174">Zabezpečení instalace hello MySQL, nastavit hello kořenové heslo, odeberte anonymní uživatelé toodisable kořenové vzdálené přihlášení a odebrání hello testovací databáze.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-174">Secure hello MySQL installation, set hello root password, remove anonymous users toodisable remote root login, and remove hello test database.</span></span>

           mysql_secure_installation
   <span data-ttu-id="b2c3d-175">c.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-175">c.</span></span> <span data-ttu-id="b2c3d-176">Vytvořte uživatele na hello databáze pro operace clusteru a volitelně pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-176">Create a user on hello database for cluster operations, and optionally for your applications.</span></span>

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* too'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   <span data-ttu-id="b2c3d-177">d.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-177">d.</span></span> <span data-ttu-id="b2c3d-178">Zastavte MySQL.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-178">Stop MySQL.</span></span>

            service mysql stop
7. <span data-ttu-id="b2c3d-179">Vytvoření konfigurace zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-179">Create a configuration placeholder.</span></span>

   <span data-ttu-id="b2c3d-180">a.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-180">a.</span></span> <span data-ttu-id="b2c3d-181">Upravte hello MySQL konfigurace toocreate zástupný symbol pro nastavení clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-181">Edit hello MySQL configuration toocreate a placeholder for hello cluster settings.</span></span> <span data-ttu-id="b2c3d-182">Nepřepisovat existující hello  **`<Variables>`**  nebo zrušte komentář u teď.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-182">Do not replace hello **`<Variables>`** or uncomment now.</span></span> <span data-ttu-id="b2c3d-183">Který se stane po vytvoření virtuálního počítače z této šablony.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-183">That will happen after you create a VM from this template.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="b2c3d-184">b.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-184">b.</span></span> <span data-ttu-id="b2c3d-185">Upravit hello  **[galera]**  části a vyčistit ho.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-185">Edit hello **[galera]** section and clear it out.</span></span>

   <span data-ttu-id="b2c3d-186">c.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-186">c.</span></span> <span data-ttu-id="b2c3d-187">Upravit hello **[mariadb]** části.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-187">Edit hello **[mariadb]** section.</span></span>

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set too0.0.0.0, hello server listens tooremote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for hello SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set hello node name of this server
8. <span data-ttu-id="b2c3d-188">Otevřené požadované porty v bráně firewall hello pomocí FirewallD na CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-188">Open required ports on hello firewall by using FirewallD on CentOS 7.</span></span>

   * <span data-ttu-id="b2c3d-189">MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="b2c3d-189">MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span></span>
   * <span data-ttu-id="b2c3d-190">GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="b2c3d-190">GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span></span>
   * <span data-ttu-id="b2c3d-191">GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="b2c3d-191">GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span></span>
   * <span data-ttu-id="b2c3d-192">RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="b2c3d-192">RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span></span>
   * <span data-ttu-id="b2c3d-193">Znovu načtete hello brány firewall:`firewall-cmd --reload`</span><span class="sxs-lookup"><span data-stu-id="b2c3d-193">Reload hello firewall: `firewall-cmd --reload`</span></span>

9. <span data-ttu-id="b2c3d-194">Optimalizujte hello systému pro výkon.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-194">Optimize hello system for performance.</span></span> <span data-ttu-id="b2c3d-195">Další informace najdete v tématu [strategie ladění výkonu](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3d-195">For more information, see [performance tuning strategy](optimize-mysql.md).</span></span>

   <span data-ttu-id="b2c3d-196">a.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-196">a.</span></span> <span data-ttu-id="b2c3d-197">Upravte konfigurační soubor MySQL hello znovu.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-197">Edit hello MySQL configuration file again.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="b2c3d-198">b.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-198">b.</span></span> <span data-ttu-id="b2c3d-199">Upravit hello **[mariadb]** části a připojte hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="b2c3d-199">Edit hello **[mariadb]** section and append hello following content:</span></span>

   > [!NOTE]
   > <span data-ttu-id="b2c3d-200">Doporučujeme, abyste tento innodb\_vyrovnávací paměti\_pool_size je 70 procent paměti Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-200">We recommend that innodb\_buffer\_pool_size is 70 percent of your VM's memory.</span></span> <span data-ttu-id="b2c3d-201">V tomto příkladu ho je nastavená na 2.45 GB pro střední hello virtuální počítač Azure s 3.5 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-201">In this example, it has been set at 2.45 GB for hello medium Azure VM with 3.5 GB of RAM.</span></span>
   >
   >

           innodb_buffer_pool_size = 2508M # hello buffer pool contains buffered data and hello index. This is usually set too70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give hello server more time toorecycle idled connections
           innodb_file_per_table = 1 # Speed up hello table space transmission and optimize hello debris management performance
           innodb_log_buffer_size = 128M # hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit
           innodb_flush_log_at_trx_commit = 2 # hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. <span data-ttu-id="b2c3d-202">Zastavit MySQL, zakázat službu MySQL na spuštění tooavoid přerušení hello clusteru při přidávání uzlu spustit a zrušit jejich zřízení počítače hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-202">Stop MySQL, disable MySQL service from running on startup tooavoid disrupting hello cluster when adding a node, and deprovision hello machine.</span></span>

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. <span data-ttu-id="b2c3d-203">Zaznamenejte hello virtuálního počítače přes portál hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-203">Capture hello VM through hello portal.</span></span> <span data-ttu-id="b2c3d-204">(V současné době [vydání #1268 v nástrojích příkazového řádku Azure CLI hello](https://github.com/Azure/azure-xplat-cli/issues/1268) popisuje hello fakt, že obrazů z nástrojů příkazového řádku Azure hello není zachycen hello připojené datových disků.)</span><span class="sxs-lookup"><span data-stu-id="b2c3d-204">(Currently, [issue #1268 in hello Azure CLI tools](https://github.com/Azure/azure-xplat-cli/issues/1268) describes hello fact that images captured by hello Azure CLI tools do not capture hello attached data disks.)</span></span>

    <span data-ttu-id="b2c3d-205">a.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-205">a.</span></span> <span data-ttu-id="b2c3d-206">Vypněte počítač hello prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-206">Shut down hello machine through hello portal.</span></span>

    <span data-ttu-id="b2c3d-207">b.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-207">b.</span></span> <span data-ttu-id="b2c3d-208">Klikněte na tlačítko **zaznamenat** a zadejte název bitové kopie hello jako **mariadb-galera image**.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-208">Click **Capture** and specify hello image name as **mariadb-galera-image**.</span></span> <span data-ttu-id="b2c3d-209">Zadejte popis a zkontrolujte "I jste spustili příkaz waagent."</span><span class="sxs-lookup"><span data-stu-id="b2c3d-209">Provide a description and check "I have run waagent."</span></span>
      
      ![Zachytit virtuální počítač hello](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-hello-cluster"></a><span data-ttu-id="b2c3d-211">Vytvoření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="b2c3d-211">Create hello cluster</span></span>
<span data-ttu-id="b2c3d-212">Vytvořte tři virtuální počítače pomocí šablony hello vytvořili a potom nakonfigurovat a spustit hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-212">Create three VMs with hello template you created, and then configure and start hello cluster.</span></span>

1. <span data-ttu-id="b2c3d-213">Vytvořte hello první virtuální počítač CentOS 7 z hello mariadb-galera-image bitovou kopii, kterou jste vytvořili, poskytuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="b2c3d-213">Create hello first CentOS 7 VM from hello mariadb-galera-image image you created, providing hello following information:</span></span>

 - <span data-ttu-id="b2c3d-214">Název virtuální sítě: mariadbvnet</span><span class="sxs-lookup"><span data-stu-id="b2c3d-214">Virtual network name: mariadbvnet</span></span>
 - <span data-ttu-id="b2c3d-215">Podsítě: mariadb</span><span class="sxs-lookup"><span data-stu-id="b2c3d-215">Subnet: mariadb</span></span>
 - <span data-ttu-id="b2c3d-216">Počítač velikost: střední</span><span class="sxs-lookup"><span data-stu-id="b2c3d-216">Machine size: medium</span></span>
 - <span data-ttu-id="b2c3d-217">Název cloudové služby: mariadbha (nebo jakýkoli název chcete přistupovat prostřednictvím mariadbha.cloudapp.net toobe)</span><span class="sxs-lookup"><span data-stu-id="b2c3d-217">Cloud service name: mariadbha (or whatever name you want toobe accessed through mariadbha.cloudapp.net)</span></span>
 - <span data-ttu-id="b2c3d-218">Název počítače: mariadb1</span><span class="sxs-lookup"><span data-stu-id="b2c3d-218">Machine name: mariadb1</span></span>
 - <span data-ttu-id="b2c3d-219">Uživatelské jméno: azureuser</span><span class="sxs-lookup"><span data-stu-id="b2c3d-219">Username: azureuser</span></span>
 - <span data-ttu-id="b2c3d-220">Přístup SSH: povoleno</span><span class="sxs-lookup"><span data-stu-id="b2c3d-220">SSH access: enabled</span></span>
 - <span data-ttu-id="b2c3d-221">Předávání soubor .pem certifikátu hello SSH a nahraďte /path/to/key.pem hello cestu, kam jste uložili hello vygenerovat klíč SSH .pem.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-221">Passing hello SSH certificate .pem file and replacing /path/to/key.pem with hello path where you stored hello generated .pem SSH key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b2c3d-222">Hello následující příkazy jsou rozděleny na více řádků pro přehlednost, ale měli zadejte každou na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-222">hello following commands are split over multiple lines for clarity, but you should enter each as one line.</span></span>
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
2. <span data-ttu-id="b2c3d-223">Vytvořte dva další virtuální počítače propojením toohello mariadbha cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-223">Create two more virtual machines by connecting them toohello mariadbha cloud service.</span></span> <span data-ttu-id="b2c3d-224">Změnit název virtuálního počítače hello a hello port tooa jedinečný portu SSH není v konfliktu se ostatní virtuální počítače v hello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-224">Change hello VM name and hello SSH port tooa unique port not conflicting with other VMs in hello same cloud service.</span></span>

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  <span data-ttu-id="b2c3d-225">Pro MariaDB3:</span><span class="sxs-lookup"><span data-stu-id="b2c3d-225">For MariaDB3:</span></span>

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
3. <span data-ttu-id="b2c3d-226">Budete potřebovat tooget hello interní IP adresu jednotlivých virtuálních počítačů hello tři hello další krok:</span><span class="sxs-lookup"><span data-stu-id="b2c3d-226">You will need tooget hello internal IP address of each of hello three VMs for hello next step:</span></span>

    ![Získání IP adresy](./media/mariadb-mysql-cluster/IP.png)
4. <span data-ttu-id="b2c3d-228">Použití SSH toosign v toohello tři virtuální počítače a upravovat soubor konfigurace hello na každý z nich.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-228">Use SSH toosign in toohello three VMs and edit hello configuration file on each of them.</span></span>

        sudo vi /etc/my.cnf.d/server.cnf

    <span data-ttu-id="b2c3d-229">Zrušením komentáře u  **`wsrep_cluster_name`**  a  **`wsrep_cluster_address`**  odebráním hello  **#**  od začátku hello hello řádku.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-229">Uncomment **`wsrep_cluster_name`** and **`wsrep_cluster_address`** by removing hello **#** at hello beginning of hello line.</span></span>
    <span data-ttu-id="b2c3d-230">Kromě toho nahradit  **`<ServerIP>`**  v  **`wsrep_node_address`**  a  **`<NodeName>`**  v  **`wsrep_node_name`**  s hello Virtuálního počítače IP adres a name, a zrušte komentář u také tyto řádky.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-230">Additionally, replace **`<ServerIP>`** in **`wsrep_node_address`** and **`<NodeName>`** in **`wsrep_node_name`** with hello VM's IP address and name, respectively, and uncomment those lines as well.</span></span>
5. <span data-ttu-id="b2c3d-231">Spusťte hello clusteru na MariaDB1 a nechat ji spustit při spuštění.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-231">Start hello cluster on MariaDB1 and let it run at startup.</span></span>

        sudo service mysql bootstrap
        chkconfig mysql on
6. <span data-ttu-id="b2c3d-232">Spusťte MySQL na MariaDB2 a MariaDB3 a nechat ji spustit při spuštění.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-232">Start MySQL on MariaDB2 and MariaDB3 and let it run at startup.</span></span>

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-hello-cluster"></a><span data-ttu-id="b2c3d-233">Cluster hello Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b2c3d-233">Load balance hello cluster</span></span>
<span data-ttu-id="b2c3d-234">Při vytváření virtuálních počítačů hello v clusteru byly přidány do skupiny dostupnosti názvem clusteravset tooensure, jejich umístění v různých doménách selhání a aktualizace, a že Azure nikdy nemá údržby na všech počítačích najednou.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-234">When you created hello clustered VMs, you added them into an availability set called clusteravset tooensure that they were put on different fault and update domains and that Azure never does maintenance on all machines at once.</span></span> <span data-ttu-id="b2c3d-235">Tato konfigurace splňuje požadavky hello toobe nepodporuje hello smlouvu o úrovni Azure služeb (SLA).</span><span class="sxs-lookup"><span data-stu-id="b2c3d-235">This configuration meets hello requirements toobe supported by hello Azure service level agreement (SLA).</span></span>

<span data-ttu-id="b2c3d-236">Teď použijte nástroj pro vyrovnávání zatížení Azure toobalance požadavky mezi hello tři uzly.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-236">Now use Azure Load Balancer toobalance requests between hello three nodes.</span></span>

<span data-ttu-id="b2c3d-237">Spusťte následující příkazy v počítači pomocí rozhraní příkazového řádku Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-237">Run hello following commands on your machine by using hello Azure CLI.</span></span>

<span data-ttu-id="b2c3d-238">Struktura parametry příkazu Hello je:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span><span class="sxs-lookup"><span data-stu-id="b2c3d-238">hello command parameters structure is: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span></span>

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

<span data-ttu-id="b2c3d-239">Hello rozhraní příkazového řádku nastaví hello zatížení vyrovnávání testu interval too15 sekund, což může být trochu příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-239">hello CLI sets hello load balancer probe interval too15 seconds, which might be a bit too long.</span></span> <span data-ttu-id="b2c3d-240">Změnit hello portálu v části **koncové body** pro všechny virtuální počítače hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-240">Change it in hello portal under **Endpoints** for any of hello VMs.</span></span>

![Upravit koncový bod](./media/mariadb-mysql-cluster/Endpoint.PNG)

<span data-ttu-id="b2c3d-242">Vyberte **Reconfigure hello architektuře s vyrovnáváním zatížení nastavit**.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-242">Select **Reconfigure hello Load-Balanced Set**.</span></span>

![Překonfigurujte hello-s vyrovnáváním zatížení](./media/mariadb-mysql-cluster/Endpoint2.PNG)

<span data-ttu-id="b2c3d-244">Změna **Interval sběru dat** too5 sekund a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-244">Change **Probe Interval** too5 seconds and save your changes.</span></span>

![Interval kontroly změn](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-hello-cluster"></a><span data-ttu-id="b2c3d-246">Ověření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="b2c3d-246">Validate hello cluster</span></span>
<span data-ttu-id="b2c3d-247">provádí náročné práce Hello.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-247">hello hard work is done.</span></span> <span data-ttu-id="b2c3d-248">Hello clusteru musí být nyní přístupná na `mariadbha.cloudapp.net:3306`, který dotkne hello nástroj pro vyrovnávání zatížení a požadavky na směrování mezi hello tři virtuální počítače efektivně a bez obtíží.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-248">hello cluster should be now accessible at `mariadbha.cloudapp.net:3306`, which hits hello load balancer and route requests between hello three VMs smoothly and efficiently.</span></span>

<span data-ttu-id="b2c3d-249">Použijte váš oblíbený klienta tooconnect MySQL, nebo se připojte z jednoho z tooverify hello virtuální počítače, který pracuje v tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-249">Use your favorite MySQL client tooconnect, or connect from one of hello VMs tooverify that this cluster is working.</span></span>

     mysql -u cluster -h mariadbha.cloudapp.net -p

<span data-ttu-id="b2c3d-250">Pak vytvořte databázi a naplnit určitými daty.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-250">Then create a database and populate it with some data.</span></span>

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

<span data-ttu-id="b2c3d-251">Hello databáze, kterou jste vytvořili vrátí hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="b2c3d-251">hello database you created returns hello following table:</span></span>

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="b2c3d-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2c3d-252">Next steps</span></span>
<span data-ttu-id="b2c3d-253">V tomto článku jste vytvořili tři uzly MariaDB + Galera vysoce dostupný cluster v Azure virtuální počítače spuštěné CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-253">In this article, you created a three-node MariaDB + Galera highly available cluster on Azure virtual machines running CentOS 7.</span></span> <span data-ttu-id="b2c3d-254">virtuální počítače, Hello jsou zatížení vyrovnávaném pomocí vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="b2c3d-254">hello VMs are load balanced with Azure Load Balancer.</span></span>

<span data-ttu-id="b2c3d-255">Můžete chtít toolook v [jiný způsob toocluster MySQL v systému Linux](mysql-cluster.md) a způsoby příliš[optimalizace a testování výkonu databáze MySQL na virtuálních počítačích Azure Linux](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3d-255">You might want toolook at [another way toocluster MySQL on Linux](mysql-cluster.md) and ways too[optimize and test MySQL performance on Azure Linux VMs](optimize-mysql.md).</span></span>

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating hello template]:#creating-the-template
[Creating hello cluster]:#creating-the-cluster
[Load balancing hello cluster]:#load-balancing-the-cluster
[Validating hello cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
[Galera]:http://galeracluster.com/products/
[MariaDBs]:https://mariadb.org/en/about/
[vytvoření klíče SSH pro ověřování]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in hello Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
