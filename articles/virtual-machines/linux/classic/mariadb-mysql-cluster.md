---
title: Spustit cluster MariaDB (MySQL) v Azure | Microsoft Docs
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
ms.openlocfilehash: 53e9bf18b26338212411ea7c4f260eb308486738
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a><span data-ttu-id="5dc62-103">MariaDB (MySQL) clusteru: kurz pro Azure</span><span class="sxs-lookup"><span data-stu-id="5dc62-103">MariaDB (MySQL) cluster: Azure tutorial</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5dc62-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic.</span><span class="sxs-lookup"><span data-stu-id="5dc62-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="5dc62-105">Tento článek se týká modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="5dc62-105">This article covers the classic deployment model.</span></span> <span data-ttu-id="5dc62-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5dc62-106">Microsoft recommends that most new deployments use the Azure Resource Manager model.</span></span>

> [!NOTE]
> <span data-ttu-id="5dc62-107">MariaDB Enterprise clusteru je teď dostupná v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="5dc62-107">MariaDB Enterprise cluster is now available in the Azure Marketplace.</span></span> <span data-ttu-id="5dc62-108">Nová nabídka automaticky nasadí cluster MariaDB Galera na Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5dc62-108">The new offering will automatically deploy a MariaDB Galera cluster on Azure Resource Manager.</span></span> <span data-ttu-id="5dc62-109">Měli byste použít novou nabídku z [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span><span class="sxs-lookup"><span data-stu-id="5dc62-109">You should use the new offering from [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).</span></span>
>
>

<span data-ttu-id="5dc62-110">V tomto článku se dozvíte, jak vytvořit více hlavní [Galera](http://galeracluster.com/products/) cluster [MariaDBs](https://mariadb.org/en/about/) (robustní, škálovatelnou a spolehlivé drop-in nahrazení pro databázi MySQL) pro práci v prostředí s vysokou dostupností na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc62-110">This article shows you how to create a multi-Master [Galera](http://galeracluster.com/products/) cluster of [MariaDBs](https://mariadb.org/en/about/) (a robust, scalable, and reliable drop-in replacement for MySQL) to work in a highly available environment on Azure virtual machines.</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="5dc62-111">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="5dc62-111">Architecture overview</span></span>
<span data-ttu-id="5dc62-112">Tento článek popisuje, jak provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5dc62-112">This article describes how to complete the following steps:</span></span>

- <span data-ttu-id="5dc62-113">Vytvořte tři uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="5dc62-113">Create a three-node cluster.</span></span>
- <span data-ttu-id="5dc62-114">Oddělení datových disků z disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="5dc62-114">Separate the data disks from the OS disk.</span></span>
- <span data-ttu-id="5dc62-115">Vytvoření datových disků RAID-0 nebo rozdělená nastavení zvýšit IOPS.</span><span class="sxs-lookup"><span data-stu-id="5dc62-115">Create the data disks in RAID-0/striped setting to increase IOPS.</span></span>
- <span data-ttu-id="5dc62-116">Vyrovnávání zatížení Azure použijte k vyrovnávání zatížení pro tři uzly.</span><span class="sxs-lookup"><span data-stu-id="5dc62-116">Use Azure Load Balancer to balance the load for the three nodes.</span></span>
- <span data-ttu-id="5dc62-117">Chcete-li minimalizovat opakovaných pracovní, vytvořte image virtuálního počítače, který obsahuje MariaDB + Galera a ji použít k vytvoření dalších clusteru virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5dc62-117">To minimize repetitive work, create a VM image that contains MariaDB + Galera and use it to create the other cluster VMs.</span></span>

![Architektura systému](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> <span data-ttu-id="5dc62-119">Toto téma používá [rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) nástroje, proto si je stáhnout a připojte je k předplatnému Azure podle pokynů.</span><span class="sxs-lookup"><span data-stu-id="5dc62-119">This topic uses the [Azure CLI](../../../cli-install-nodejs.md) tools, so make sure to download them and connect them to your Azure subscription according to the instructions.</span></span> <span data-ttu-id="5dc62-120">Pokud potřebujete odkaz s příkazy, které jsou k dispozici v Azure CLI, najdete v článku [reference k příkazům rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="5dc62-120">If you need a reference to the commands available in the Azure CLI, see the [Azure CLI command reference](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span> <span data-ttu-id="5dc62-121">Budete také muset [vytvoření klíče SSH pro ověřování] a poznamenejte si umístění soubor .pem.</span><span class="sxs-lookup"><span data-stu-id="5dc62-121">You will also need to [create an SSH key for authentication] and make note of the .pem file location.</span></span>
>
>

## <a name="create-the-template"></a><span data-ttu-id="5dc62-122">Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="5dc62-122">Create the template</span></span>
### <a name="infrastructure"></a><span data-ttu-id="5dc62-123">Infrastruktura</span><span class="sxs-lookup"><span data-stu-id="5dc62-123">Infrastructure</span></span>
1. <span data-ttu-id="5dc62-124">Vytvořte skupinu vztahů pro uložení prostředky společně.</span><span class="sxs-lookup"><span data-stu-id="5dc62-124">Create an affinity group to hold the resources together.</span></span>

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. <span data-ttu-id="5dc62-125">Vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5dc62-125">Create a virtual network.</span></span>

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. <span data-ttu-id="5dc62-126">Vytvořte účet úložiště pro hostování všech našich disků.</span><span class="sxs-lookup"><span data-stu-id="5dc62-126">Create a storage account to host all our disks.</span></span> <span data-ttu-id="5dc62-127">Více než 40 vytíženou disky by neměly umístit na stejný účet úložiště, aby se zabránilo stiskne 20 000 limit účet úložiště IOPS.</span><span class="sxs-lookup"><span data-stu-id="5dc62-127">You shouldn't place more than 40 heavily used disks on the same storage account to avoid hitting the 20,000 IOPS storage account limit.</span></span> <span data-ttu-id="5dc62-128">V takovém případě jste dobře nižší než toto omezení, takže budete všechno, co uložit na stejný účet pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="5dc62-128">In this case, you're well below that limit, so you'll store everything on the same account for simplicity.</span></span>

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. <span data-ttu-id="5dc62-129">Najděte název bitové kopie virtuálního počítače CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="5dc62-129">Find the name of the CentOS 7 virtual machine image.</span></span>

        azure vm image list | findstr CentOS
   <span data-ttu-id="5dc62-130">Výstup bude podobný `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span><span class="sxs-lookup"><span data-stu-id="5dc62-130">The output will be something like `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.</span></span>

   <span data-ttu-id="5dc62-131">Použijte tento název v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="5dc62-131">Use that name in the following step.</span></span>
5. <span data-ttu-id="5dc62-132">Vytvořit šablonu virtuálního počítače a nahraďte /path/to/key.pem cestu, kde je uložený klíč SSH generovaného .pem.</span><span class="sxs-lookup"><span data-stu-id="5dc62-132">Create the VM template and replace /path/to/key.pem with the path where you stored the generated .pem SSH key.</span></span>

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. <span data-ttu-id="5dc62-133">Připojte čtyři dat 500 GB disky na virtuální počítač pro použití v konfiguraci RAID.</span><span class="sxs-lookup"><span data-stu-id="5dc62-133">Attach four 500-GB data disks to the VM for use in the RAID configuration.</span></span>

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. <span data-ttu-id="5dc62-134">Použití SSH se přihlásit k šabloně virtuálního počítače, který jste vytvořili na mariadbhatemplate.cloudapp.net:22 a připojit pomocí soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="5dc62-134">Use SSH to sign in to the template VM that you created at mariadbhatemplate.cloudapp.net:22, and connect by using your private key.</span></span>

### <a name="software"></a><span data-ttu-id="5dc62-135">Software</span><span class="sxs-lookup"><span data-stu-id="5dc62-135">Software</span></span>
1. <span data-ttu-id="5dc62-136">Získejte kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="5dc62-136">Get the root.</span></span>

        sudo su

2. <span data-ttu-id="5dc62-137">Instalace podpory RAID:</span><span class="sxs-lookup"><span data-stu-id="5dc62-137">Install RAID support:</span></span>

    <span data-ttu-id="5dc62-138">a.</span><span class="sxs-lookup"><span data-stu-id="5dc62-138">a.</span></span> <span data-ttu-id="5dc62-139">Nainstalujte mdadm.</span><span class="sxs-lookup"><span data-stu-id="5dc62-139">Install mdadm.</span></span>

              yum install mdadm

    <span data-ttu-id="5dc62-140">b.</span><span class="sxs-lookup"><span data-stu-id="5dc62-140">b.</span></span> <span data-ttu-id="5dc62-141">Vytvořte konfiguraci 0/stripe EXT4 systémem souborů.</span><span class="sxs-lookup"><span data-stu-id="5dc62-141">Create the RAID0/stripe configuration with an EXT4 file system.</span></span>

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    <span data-ttu-id="5dc62-142">c.</span><span class="sxs-lookup"><span data-stu-id="5dc62-142">c.</span></span> <span data-ttu-id="5dc62-143">Vytvoření adresáře přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="5dc62-143">Create the mount point directory.</span></span>

              mkdir /mnt/data
    <span data-ttu-id="5dc62-144">d.</span><span class="sxs-lookup"><span data-stu-id="5dc62-144">d.</span></span> <span data-ttu-id="5dc62-145">Získat identifikátor UUID nově vytvořený zařízení RAID.</span><span class="sxs-lookup"><span data-stu-id="5dc62-145">Retrieve the UUID of the newly created RAID device.</span></span>

              blkid | grep /dev/md0
    <span data-ttu-id="5dc62-146">e.</span><span class="sxs-lookup"><span data-stu-id="5dc62-146">e.</span></span> <span data-ttu-id="5dc62-147">Upravte /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="5dc62-147">Edit /etc/fstab.</span></span>

              vi /etc/fstab
    <span data-ttu-id="5dc62-148">f.</span><span class="sxs-lookup"><span data-stu-id="5dc62-148">f.</span></span> <span data-ttu-id="5dc62-149">Přidat zařízení povolit automatické připojování při restartování, nahraďte hodnotou získané z předchozí identifikátor UUID **blkid** příkaz.</span><span class="sxs-lookup"><span data-stu-id="5dc62-149">Add the device to enable auto mounting on reboot, replacing the UUID with the value obtained from the previous **blkid** command.</span></span>

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    <span data-ttu-id="5dc62-150">g.</span><span class="sxs-lookup"><span data-stu-id="5dc62-150">g.</span></span> <span data-ttu-id="5dc62-151">Připojte nový oddíl.</span><span class="sxs-lookup"><span data-stu-id="5dc62-151">Mount the new partition.</span></span>

              mount /mnt/data

3. <span data-ttu-id="5dc62-152">Nainstalujte MariaDB.</span><span class="sxs-lookup"><span data-stu-id="5dc62-152">Install MariaDB.</span></span>

    <span data-ttu-id="5dc62-153">a.</span><span class="sxs-lookup"><span data-stu-id="5dc62-153">a.</span></span> <span data-ttu-id="5dc62-154">Vytvořte soubor MariaDB.repo.</span><span class="sxs-lookup"><span data-stu-id="5dc62-154">Create the MariaDB.repo file.</span></span>

                vi /etc/yum.repos.d/MariaDB.repo

    <span data-ttu-id="5dc62-155">b.</span><span class="sxs-lookup"><span data-stu-id="5dc62-155">b.</span></span> <span data-ttu-id="5dc62-156">Zadejte soubor úložiště s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="5dc62-156">Fill the repo file with the following content:</span></span>

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    <span data-ttu-id="5dc62-157">c.</span><span class="sxs-lookup"><span data-stu-id="5dc62-157">c.</span></span> <span data-ttu-id="5dc62-158">Aby nedocházelo ke konfliktům, odeberte existující operátory a mariadb knihovny.</span><span class="sxs-lookup"><span data-stu-id="5dc62-158">To avoid conflicts, remove existing postfix and mariadb-libs.</span></span>

           yum remove postfix mariadb-libs-*
    <span data-ttu-id="5dc62-159">d.</span><span class="sxs-lookup"><span data-stu-id="5dc62-159">d.</span></span> <span data-ttu-id="5dc62-160">Nainstalujte MariaDB s Galera.</span><span class="sxs-lookup"><span data-stu-id="5dc62-160">Install MariaDB with Galera.</span></span>

           yum install MariaDB-Galera-server MariaDB-client galera

4. <span data-ttu-id="5dc62-161">Přesuňte do adresáře dat MySQL do zařízení s blokovým RAID.</span><span class="sxs-lookup"><span data-stu-id="5dc62-161">Move the MySQL data directory to the RAID block device.</span></span>

    <span data-ttu-id="5dc62-162">a.</span><span class="sxs-lookup"><span data-stu-id="5dc62-162">a.</span></span> <span data-ttu-id="5dc62-163">Zkopírujte aktuální adresář MySQL do nového umístění a odebrat starý adresář.</span><span class="sxs-lookup"><span data-stu-id="5dc62-163">Copy the current MySQL directory into its new location and remove the old directory.</span></span>

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    <span data-ttu-id="5dc62-164">b.</span><span class="sxs-lookup"><span data-stu-id="5dc62-164">b.</span></span> <span data-ttu-id="5dc62-165">Nastavte oprávnění pro nový adresář odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5dc62-165">Set permissions for the new directory accordingly.</span></span>

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    <span data-ttu-id="5dc62-166">c.</span><span class="sxs-lookup"><span data-stu-id="5dc62-166">c.</span></span> <span data-ttu-id="5dc62-167">Vytvořte symlink, který odkazuje adresáři původního do nového umístění v oddílu RAID.</span><span class="sxs-lookup"><span data-stu-id="5dc62-167">Create a symlink that points the old directory to the new location on the RAID partition.</span></span>

           ln -s /mnt/data/mysql /var/lib/mysql

5. <span data-ttu-id="5dc62-168">Protože [SELinux naruší operací clusteru](http://galeracluster.com/documentation-webpages/configuration.html#selinux), je nutné zakázat pro aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="5dc62-168">Because [SELinux interferes with the cluster operations](http://galeracluster.com/documentation-webpages/configuration.html#selinux), it is necessary to disable it for the current session.</span></span> <span data-ttu-id="5dc62-169">Upravit `/etc/selinux/config` zakázat pro následné restartování.</span><span class="sxs-lookup"><span data-stu-id="5dc62-169">Edit `/etc/selinux/config` to disable it for subsequent restarts.</span></span>

            setenforce 0

            then editing `/etc/selinux/config` to set `SELINUX=permissive`
6. <span data-ttu-id="5dc62-170">Ověření spuštění MySQL.</span><span class="sxs-lookup"><span data-stu-id="5dc62-170">Validate MySQL runs.</span></span>

   <span data-ttu-id="5dc62-171">a.</span><span class="sxs-lookup"><span data-stu-id="5dc62-171">a.</span></span> <span data-ttu-id="5dc62-172">Spusťte MySQL.</span><span class="sxs-lookup"><span data-stu-id="5dc62-172">Start MySQL.</span></span>

           service mysql start
   <span data-ttu-id="5dc62-173">b.</span><span class="sxs-lookup"><span data-stu-id="5dc62-173">b.</span></span> <span data-ttu-id="5dc62-174">Zabezpečení instalace MySQL, nastavit kořenové heslo, odeberte anonymním uživatelům zakázat kořenové vzdálené přihlášení a odebrat databázi testu.</span><span class="sxs-lookup"><span data-stu-id="5dc62-174">Secure the MySQL installation, set the root password, remove anonymous users to disable remote root login, and remove the test database.</span></span>

           mysql_secure_installation
   <span data-ttu-id="5dc62-175">c.</span><span class="sxs-lookup"><span data-stu-id="5dc62-175">c.</span></span> <span data-ttu-id="5dc62-176">Vytvořte uživatele v databázi pro operace clusteru a volitelně pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="5dc62-176">Create a user on the database for cluster operations, and optionally for your applications.</span></span>

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   <span data-ttu-id="5dc62-177">d.</span><span class="sxs-lookup"><span data-stu-id="5dc62-177">d.</span></span> <span data-ttu-id="5dc62-178">Zastavte MySQL.</span><span class="sxs-lookup"><span data-stu-id="5dc62-178">Stop MySQL.</span></span>

            service mysql stop
7. <span data-ttu-id="5dc62-179">Vytvoření konfigurace zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="5dc62-179">Create a configuration placeholder.</span></span>

   <span data-ttu-id="5dc62-180">a.</span><span class="sxs-lookup"><span data-stu-id="5dc62-180">a.</span></span> <span data-ttu-id="5dc62-181">Upravte konfiguraci databáze MySQL vytvořit zástupný symbol pro nastavení clusteru.</span><span class="sxs-lookup"><span data-stu-id="5dc62-181">Edit the MySQL configuration to create a placeholder for the cluster settings.</span></span> <span data-ttu-id="5dc62-182">Nepřepisovat existující  **`<Variables>`**  nebo zrušte komentář u teď.</span><span class="sxs-lookup"><span data-stu-id="5dc62-182">Do not replace the **`<Variables>`** or uncomment now.</span></span> <span data-ttu-id="5dc62-183">Který se stane po vytvoření virtuálního počítače z této šablony.</span><span class="sxs-lookup"><span data-stu-id="5dc62-183">That will happen after you create a VM from this template.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="5dc62-184">b.</span><span class="sxs-lookup"><span data-stu-id="5dc62-184">b.</span></span> <span data-ttu-id="5dc62-185">Upravit  **[galera]**  části a vyčistit ho.</span><span class="sxs-lookup"><span data-stu-id="5dc62-185">Edit the **[galera]** section and clear it out.</span></span>

   <span data-ttu-id="5dc62-186">c.</span><span class="sxs-lookup"><span data-stu-id="5dc62-186">c.</span></span> <span data-ttu-id="5dc62-187">Upravit **[mariadb]** části.</span><span class="sxs-lookup"><span data-stu-id="5dc62-187">Edit the **[mariadb]** section.</span></span>

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server
8. <span data-ttu-id="5dc62-188">Otevřete požadované porty v bráně firewall pomocí FirewallD na CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="5dc62-188">Open required ports on the firewall by using FirewallD on CentOS 7.</span></span>

   * <span data-ttu-id="5dc62-189">MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="5dc62-189">MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`</span></span>
   * <span data-ttu-id="5dc62-190">GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="5dc62-190">GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`</span></span>
   * <span data-ttu-id="5dc62-191">GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="5dc62-191">GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`</span></span>
   * <span data-ttu-id="5dc62-192">RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span><span class="sxs-lookup"><span data-stu-id="5dc62-192">RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`</span></span>
   * <span data-ttu-id="5dc62-193">Znovu načtete brány firewall:`firewall-cmd --reload`</span><span class="sxs-lookup"><span data-stu-id="5dc62-193">Reload the firewall: `firewall-cmd --reload`</span></span>

9. <span data-ttu-id="5dc62-194">Optimalizujte výkon systému.</span><span class="sxs-lookup"><span data-stu-id="5dc62-194">Optimize the system for performance.</span></span> <span data-ttu-id="5dc62-195">Další informace najdete v tématu [strategie ladění výkonu](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="5dc62-195">For more information, see [performance tuning strategy](optimize-mysql.md).</span></span>

   <span data-ttu-id="5dc62-196">a.</span><span class="sxs-lookup"><span data-stu-id="5dc62-196">a.</span></span> <span data-ttu-id="5dc62-197">Upravte konfigurační soubor MySQL znovu.</span><span class="sxs-lookup"><span data-stu-id="5dc62-197">Edit the MySQL configuration file again.</span></span>

            vi /etc/my.cnf.d/server.cnf
   <span data-ttu-id="5dc62-198">b.</span><span class="sxs-lookup"><span data-stu-id="5dc62-198">b.</span></span> <span data-ttu-id="5dc62-199">Upravit **[mariadb]** části a připojte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="5dc62-199">Edit the **[mariadb]** section and append the following content:</span></span>

   > [!NOTE]
   > <span data-ttu-id="5dc62-200">Doporučujeme, abyste tento innodb\_vyrovnávací paměti\_pool_size je 70 procent paměti Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5dc62-200">We recommend that innodb\_buffer\_pool_size is 70 percent of your VM's memory.</span></span> <span data-ttu-id="5dc62-201">V tomto příkladu ho je nastavená na 2.45 GB pro střední virtuální počítač Azure s 3.5 GB paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="5dc62-201">In this example, it has been set at 2.45 GB for the medium Azure VM with 3.5 GB of RAM.</span></span>
   >
   >

           innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give the server more time to recycle idled connections
           innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
           innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
           innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. <span data-ttu-id="5dc62-202">Zastavit MySQL, zakázat službu MySQL z spuštěna při spuštění, aby se zabránilo přerušení clusteru při přidávání uzlu a zrušení zřízení na počítač.</span><span class="sxs-lookup"><span data-stu-id="5dc62-202">Stop MySQL, disable MySQL service from running on startup to avoid disrupting the cluster when adding a node, and deprovision the machine.</span></span>

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. <span data-ttu-id="5dc62-203">Zachycení virtuálního počítače přes portál.</span><span class="sxs-lookup"><span data-stu-id="5dc62-203">Capture the VM through the portal.</span></span> <span data-ttu-id="5dc62-204">(V současné době [vydání #1268 v nástrojích příkazového řádku Azure CLI](https://github.com/Azure/azure-xplat-cli/issues/1268) popisuje skutečnost, že obrazů z nástrojů příkazového řádku Azure není zachycen připojené datových disků.)</span><span class="sxs-lookup"><span data-stu-id="5dc62-204">(Currently, [issue #1268 in the Azure CLI tools](https://github.com/Azure/azure-xplat-cli/issues/1268) describes the fact that images captured by the Azure CLI tools do not capture the attached data disks.)</span></span>

    <span data-ttu-id="5dc62-205">a.</span><span class="sxs-lookup"><span data-stu-id="5dc62-205">a.</span></span> <span data-ttu-id="5dc62-206">Vypněte počítač prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="5dc62-206">Shut down the machine through the portal.</span></span>

    <span data-ttu-id="5dc62-207">b.</span><span class="sxs-lookup"><span data-stu-id="5dc62-207">b.</span></span> <span data-ttu-id="5dc62-208">Klikněte na tlačítko **zaznamenat** a zadejte název bitové kopie jako **mariadb-galera image**.</span><span class="sxs-lookup"><span data-stu-id="5dc62-208">Click **Capture** and specify the image name as **mariadb-galera-image**.</span></span> <span data-ttu-id="5dc62-209">Zadejte popis a zkontrolujte "I jste spustili příkaz waagent."</span><span class="sxs-lookup"><span data-stu-id="5dc62-209">Provide a description and check "I have run waagent."</span></span>
      
      ![Zachytit virtuální počítač](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-the-cluster"></a><span data-ttu-id="5dc62-211">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="5dc62-211">Create the cluster</span></span>
<span data-ttu-id="5dc62-212">Vytvořte tři virtuální počítače pomocí šablony vytvořili a potom nakonfigurovat a spustit clusteru.</span><span class="sxs-lookup"><span data-stu-id="5dc62-212">Create three VMs with the template you created, and then configure and start the cluster.</span></span>

1. <span data-ttu-id="5dc62-213">Vytvořte první virtuální počítač CentOS 7 z bitové kopie mariadb galera bitové kopie, kterou jste vytvořili, poskytuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="5dc62-213">Create the first CentOS 7 VM from the mariadb-galera-image image you created, providing the following information:</span></span>

 - <span data-ttu-id="5dc62-214">Název virtuální sítě: mariadbvnet</span><span class="sxs-lookup"><span data-stu-id="5dc62-214">Virtual network name: mariadbvnet</span></span>
 - <span data-ttu-id="5dc62-215">Podsítě: mariadb</span><span class="sxs-lookup"><span data-stu-id="5dc62-215">Subnet: mariadb</span></span>
 - <span data-ttu-id="5dc62-216">Počítač velikost: střední</span><span class="sxs-lookup"><span data-stu-id="5dc62-216">Machine size: medium</span></span>
 - <span data-ttu-id="5dc62-217">Název cloudové služby: mariadbha (nebo jakýkoli název, který chcete přistupovat prostřednictvím mariadbha.cloudapp.net)</span><span class="sxs-lookup"><span data-stu-id="5dc62-217">Cloud service name: mariadbha (or whatever name you want to be accessed through mariadbha.cloudapp.net)</span></span>
 - <span data-ttu-id="5dc62-218">Název počítače: mariadb1</span><span class="sxs-lookup"><span data-stu-id="5dc62-218">Machine name: mariadb1</span></span>
 - <span data-ttu-id="5dc62-219">Uživatelské jméno: azureuser</span><span class="sxs-lookup"><span data-stu-id="5dc62-219">Username: azureuser</span></span>
 - <span data-ttu-id="5dc62-220">Přístup SSH: povoleno</span><span class="sxs-lookup"><span data-stu-id="5dc62-220">SSH access: enabled</span></span>
 - <span data-ttu-id="5dc62-221">Předávání soubor .pem certifikátu SSH a nahraďte /path/to/key.pem cestu, kde je uložený klíč SSH generovaného .pem.</span><span class="sxs-lookup"><span data-stu-id="5dc62-221">Passing the SSH certificate .pem file and replacing /path/to/key.pem with the path where you stored the generated .pem SSH key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5dc62-222">Následující příkazy jsou rozděleny na více řádků pro přehlednost, ale měli zadejte každou na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="5dc62-222">The following commands are split over multiple lines for clarity, but you should enter each as one line.</span></span>
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
2. <span data-ttu-id="5dc62-223">Připojte se ke cloudové službě mariadbha vytvořte dva další virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5dc62-223">Create two more virtual machines by connecting them to the mariadbha cloud service.</span></span> <span data-ttu-id="5dc62-224">Změňte název virtuálního počítače a portu SSH na není v konfliktu se ostatní virtuální počítače v rámci stejné cloudové služby jedinečný port.</span><span class="sxs-lookup"><span data-stu-id="5dc62-224">Change the VM name and the SSH port to a unique port not conflicting with other VMs in the same cloud service.</span></span>

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
  <span data-ttu-id="5dc62-225">Pro MariaDB3:</span><span class="sxs-lookup"><span data-stu-id="5dc62-225">For MariaDB3:</span></span>

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
3. <span data-ttu-id="5dc62-226">Budete muset získat interní IP adresu každého ze tří virtuálních počítačů pro další krok:</span><span class="sxs-lookup"><span data-stu-id="5dc62-226">You will need to get the internal IP address of each of the three VMs for the next step:</span></span>

    ![Získání IP adresy](./media/mariadb-mysql-cluster/IP.png)
4. <span data-ttu-id="5dc62-228">Použití SSH k přihlášení na tři virtuální počítače a upravovat soubor konfigurace na každý z nich.</span><span class="sxs-lookup"><span data-stu-id="5dc62-228">Use SSH to sign in to the three VMs and edit the configuration file on each of them.</span></span>

        sudo vi /etc/my.cnf.d/server.cnf

    <span data-ttu-id="5dc62-229">Zrušením komentáře u  **`wsrep_cluster_name`**  a  **`wsrep_cluster_address`**  odebráním  **#**  na začátek řádku.</span><span class="sxs-lookup"><span data-stu-id="5dc62-229">Uncomment **`wsrep_cluster_name`** and **`wsrep_cluster_address`** by removing the **#** at the beginning of the line.</span></span>
    <span data-ttu-id="5dc62-230">Kromě toho nahradit  **`<ServerIP>`**  v  **`wsrep_node_address`**  a  **`<NodeName>`**  v  **`wsrep_node_name`**  s Virtuálního počítače IP adresou a name, a zrušte komentář u také tyto řádky.</span><span class="sxs-lookup"><span data-stu-id="5dc62-230">Additionally, replace **`<ServerIP>`** in **`wsrep_node_address`** and **`<NodeName>`** in **`wsrep_node_name`** with the VM's IP address and name, respectively, and uncomment those lines as well.</span></span>
5. <span data-ttu-id="5dc62-231">Start clusteru na MariaDB1 a nechat ji spustit při spuštění.</span><span class="sxs-lookup"><span data-stu-id="5dc62-231">Start the cluster on MariaDB1 and let it run at startup.</span></span>

        sudo service mysql bootstrap
        chkconfig mysql on
6. <span data-ttu-id="5dc62-232">Spusťte MySQL na MariaDB2 a MariaDB3 a nechat ji spustit při spuštění.</span><span class="sxs-lookup"><span data-stu-id="5dc62-232">Start MySQL on MariaDB2 and MariaDB3 and let it run at startup.</span></span>

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-the-cluster"></a><span data-ttu-id="5dc62-233">Nástroj pro vyrovnávání zatížení clusteru</span><span class="sxs-lookup"><span data-stu-id="5dc62-233">Load balance the cluster</span></span>
<span data-ttu-id="5dc62-234">Při vytváření clusteru virtuálních počítačů, přidat je do skupiny dostupnosti názvem clusteravset zajistit, že jejich umístění v různých doménách selhání a aktualizace, a že Azure nikdy nemá údržby na všech počítačích najednou.</span><span class="sxs-lookup"><span data-stu-id="5dc62-234">When you created the clustered VMs, you added them into an availability set called clusteravset to ensure that they were put on different fault and update domains and that Azure never does maintenance on all machines at once.</span></span> <span data-ttu-id="5dc62-235">Tato konfigurace splňuje požadavky na podporu Azure smlouvu o úrovni služeb (SLA).</span><span class="sxs-lookup"><span data-stu-id="5dc62-235">This configuration meets the requirements to be supported by the Azure service level agreement (SLA).</span></span>

<span data-ttu-id="5dc62-236">Teď použijte nástroj pro vyrovnávání zatížení Azure k vyrovnávání požadavků mezi tři uzly.</span><span class="sxs-lookup"><span data-stu-id="5dc62-236">Now use Azure Load Balancer to balance requests between the three nodes.</span></span>

<span data-ttu-id="5dc62-237">Spusťte následující příkazy v počítači pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc62-237">Run the following commands on your machine by using the Azure CLI.</span></span>

<span data-ttu-id="5dc62-238">Struktura parametry příkazu je:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span><span class="sxs-lookup"><span data-stu-id="5dc62-238">The command parameters structure is: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`</span></span>

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

<span data-ttu-id="5dc62-239">Rozhraní příkazového řádku nastaví interval testu nástroje pro vyrovnávání zatížení na 15 sekund, což může být trochu příliš dlouhý.</span><span class="sxs-lookup"><span data-stu-id="5dc62-239">The CLI sets the load balancer probe interval to 15 seconds, which might be a bit too long.</span></span> <span data-ttu-id="5dc62-240">Změnit portálu v části **koncové body** pro všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5dc62-240">Change it in the portal under **Endpoints** for any of the VMs.</span></span>

![Upravit koncový bod](./media/mariadb-mysql-cluster/Endpoint.PNG)

<span data-ttu-id="5dc62-242">Vyberte **překonfigurovat sady vyrovnáváním zatížení**.</span><span class="sxs-lookup"><span data-stu-id="5dc62-242">Select **Reconfigure the Load-Balanced Set**.</span></span>

![Znovu nakonfigurujte sadu s vyrovnáváním zatížení](./media/mariadb-mysql-cluster/Endpoint2.PNG)

<span data-ttu-id="5dc62-244">Změna **Interval sběru dat** na 5 sekund a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="5dc62-244">Change **Probe Interval** to 5 seconds and save your changes.</span></span>

![Interval kontroly změn](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-the-cluster"></a><span data-ttu-id="5dc62-246">Ověření clusteru</span><span class="sxs-lookup"><span data-stu-id="5dc62-246">Validate the cluster</span></span>
<span data-ttu-id="5dc62-247">Provádí náročné práce.</span><span class="sxs-lookup"><span data-stu-id="5dc62-247">The hard work is done.</span></span> <span data-ttu-id="5dc62-248">Cluster musí být nyní přístupná na `mariadbha.cloudapp.net:3306`, které efektivně a bez obtíží dotkne zatížení požadavky vyrovnávání a směrování mezi tři virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="5dc62-248">The cluster should be now accessible at `mariadbha.cloudapp.net:3306`, which hits the load balancer and route requests between the three VMs smoothly and efficiently.</span></span>

<span data-ttu-id="5dc62-249">Vaše oblíbené klient MySQL použijte připojení nebo připojení z jednoho z virtuálních počítačů k ověření, že je tento cluster funguje.</span><span class="sxs-lookup"><span data-stu-id="5dc62-249">Use your favorite MySQL client to connect, or connect from one of the VMs to verify that this cluster is working.</span></span>

     mysql -u cluster -h mariadbha.cloudapp.net -p

<span data-ttu-id="5dc62-250">Pak vytvořte databázi a naplnit určitými daty.</span><span class="sxs-lookup"><span data-stu-id="5dc62-250">Then create a database and populate it with some data.</span></span>

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

<span data-ttu-id="5dc62-251">Vrátí databázi, kterou jste vytvořili v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="5dc62-251">The database you created returns the following table:</span></span>

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="5dc62-252">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5dc62-252">Next steps</span></span>
<span data-ttu-id="5dc62-253">V tomto článku jste vytvořili tři uzly MariaDB + Galera vysoce dostupný cluster v Azure virtuální počítače spuštěné CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="5dc62-253">In this article, you created a three-node MariaDB + Galera highly available cluster on Azure virtual machines running CentOS 7.</span></span> <span data-ttu-id="5dc62-254">Virtuální počítače jsou zatížení vyrovnávaném pomocí vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc62-254">The VMs are load balanced with Azure Load Balancer.</span></span>

<span data-ttu-id="5dc62-255">Můžete se podívat na [jiný způsob, jak cluster MySQL v systému Linux](mysql-cluster.md) a způsoby, jak [optimalizace a testování výkonu databáze MySQL na virtuálních počítačích Azure Linux](optimize-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="5dc62-255">You might want to look at [another way to cluster MySQL on Linux](mysql-cluster.md) and ways to [optimize and test MySQL performance on Azure Linux VMs](optimize-mysql.md).</span></span>

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating the template]:#creating-the-template
[Creating the cluster]:#creating-the-cluster
[Load balancing the cluster]:#load-balancing-the-cluster
[Validating the cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
<span data-ttu-id="5dc62-256">[Galera]:http://galeracluster.com/products/</span><span class="sxs-lookup"><span data-stu-id="5dc62-256">[Galera]:http://galeracluster.com/products/</span></span>
[MariaDBs]:https://mariadb.org/en/about/
<span data-ttu-id="5dc62-257">[vytvoření klíče SSH pro ověřování]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/</span><span class="sxs-lookup"><span data-stu-id="5dc62-257">[create an SSH key for authentication]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/</span></span>
[issue #1268 in the Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
