---
title: "aaaLinux a Open-Source výpočty v Azure | Microsoft Docs"
description: "Obsahuje seznam Linux a Open-Source výpočetní článků v Azure, včetně základní Linux využití některé základní pojmy o spuštěna nebo k odesílání imagí Linux na Azure a další obsah o konkrétní technologie a optimalizace."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: a7e608b5-26ea-41e0-b46b-1a483a257754
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/27/2016
ms.author: rasquill
ms.openlocfilehash: 3ce0dcc65f28d0dddb29f654409f6dae56213bc7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="linux-and-open-source-computing-on-azure"></a>Zpracování dat s Linuxem a open sourcem v Azure 
Vyhledejte všechny hello dokumentaci potřebujete toocreate a správa virtuálních počítačů založených na Linuxu v modelu nasazení classic hello.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

## <a name="get-started"></a>Začínáme
* [Úvod pro Linux v Azure](intro-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Časté otázky o Azure virtuální počítače vytvořené pomocí modelu nasazení classic hello](classic/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [O bitových kopií pro virtuální počítače](../windows/classic/about-images.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Odesílání bitové Distro](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) (a také pokyny pro použití [Azure-Endorsed distribuční](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json))
* [Přihlaste se pomocí virtuálních počítačů Linux hello tooa portál Azure classic](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="set-up"></a>Nastavení
* [Nainstalovat Azure rozhraní příkazového řádku (Azure CLI)](../../cli-install-nodejs.md)

## <a name="tutorials"></a>Kurzy
* [Nainstalujte na virtuální počítač s Linuxem v Azure hello svítilna zásobníku](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ruby, na které webové aplikace ve virtuálním počítači Azure](classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
* [Postupy: instalace Apache Qpid kanálem C pro AMQP a Service Bus](../../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Databáze
* [Optimalizace výkonu databáze MySQL na Azure](classic/optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Clustery MySQL](classic/mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Spuštěné Cassandra s Linuxem v Azure a k ní přistupují z Node.js](classic/cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Vytvořit více hlavních cluster MariaDbs](classic/mariadb-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="hpc"></a>HPC
* [Začínáme s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Spuštění NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="docker"></a>Docker
* [Pomocí hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)](classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Pomocí hello Docker rozšíření virtuálního počítače z hello portálu Azure](classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Jak toouse docker počítače v Azure](docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="ubuntu"></a>Ubuntu
* [Postupy: MySQL clustery](classic/mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Postupy: Node.js a Cassandra](classic/cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="opensuse"></a>OpenSUSE
* [Postupy: instalace a spuštění MySQL](classic/mysql-on-opensuse.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="coreos"></a>CoreOS
* [Postupy: použití CoreOS v Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="planning"></a>Plánování
* [Pokyny pro implementaci služeb infrastruktury Azure](../windows/infrastructure-subscription-accounts-guidelines.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Výběr uživatelských jmen systému Linux](usernames.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Jak tooconfigure sadu dostupnosti pro virtuální počítače v modelu nasazení classic hello](../windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Jak tooSchedule plánovaná údržby na virtuálních počítačích Azure](classic/planned-maintenance-schedule.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Spravovat hello dostupnosti virtuálních počítačů](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Plánované údržby pro virtuální počítače s Linuxem v Azure](planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="deployment"></a>Nasazení
* [Vytvoření vlastní virtuální počítače se systémem Linux](../windows/classic/createportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hello základy: zachycení virtuálního počítače s Linuxem tooMake šablonu](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="management"></a>Správa
* [SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Jak tooReset heslo nebo SSH vlastnosti pro Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Pomocí kořenového adresáře](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="azure-resources"></a>Prostředky Azure
* [Hello Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Rozšíření virtuálního počítače Azure a funkce](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Vložení vlastní Data do toouse virtuálních počítačů s inicializací cloudu](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="storage"></a>Úložiště
* [Připojení tooa datový Disk virtuálního počítače s Linuxem](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Odpojování datový Disk od virtuálního počítače s Linuxem](classic/detach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [DISKOVÉHO POLE RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Sítě
* [Jak tooset až koncových bodů na klasické virtuální počítač v Azure](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="troubleshooting"></a>Řešení potíží
* [Řešení potíží s Secure Shell (SSH) připojení tooa systémem Linux virtuálního počítače Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Řešení problémů nasazení classic s vytvoření nového virtuálního počítače Linux v Azure](classic/troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)  
* [Řešení problémů nasazení classic s restartováním nebo změnou velikosti existující virtuální počítač Linux v Azure](../windows/restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) 

## <a name="reference"></a>Referenční informace
* [Azure CLI příkazy v režimu Azure Service Management (asm)](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Rozhraní REST API pro správu služby Azure](https://msdn.microsoft.com/library/azure/ee460799.aspx)

## <a name="general-links"></a>Obecné odkazy
Hello následující odkazy jsou pro blogy od Microsoftu, stránky Technet a externí servery, nikoli Azure.com dokumentaci, jak je uvedeno výše. Jako Azure a hello open source výpočetní world fast přesouváte cíle, je téměř určité, které hello následující odkazy jsou zastaralé, *navzdory* hello fakt, že provedeme naše nejlepší toocontinually novější témata přidávat a odebírat zastaralé ty, které jsou. Pokud jsme jeden provedena, prosím dejte nám vědět, v komentářích hello nebo odeslání tooour žádost o přijetí změn [úložiště GitHub](https://github.com/Azure/azure-content/).

* [ASP.NET 5 a systémem Linux pomocí Docker kontejnery](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
* [Jak tooDeploy CentOS Image virtuálního počítače z OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
* [SUSE aktualizace infrastruktury](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
* [SUSE Linux Enterprise Server knihovny zařízení cloudu SAP](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
* [Vytváření vysoce dostupných Linux na Azure na 12 kroků](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
* [Automatizovat zřizování Linux na Azure pomocí rozhraní příkazového řádku Azure, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
* [GlusterFS v Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
* [Spuštění FreeBSD v Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
* [Snadné nasazení FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
* [Nasazení bitové kopie FreeBSD](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
* [Kaspersky AV pro Linux souborový Server](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL
* [8 databáze NoSql open source pro Azure.](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
* [Slideshare (MSOpenTech): Prostředí CouchDb v Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
* [Spuštění CouchDB jako služba pomocí node.js, CORS a Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)
* [Redis v systému Windows v hello Azure Redis Cache Service](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
* [Uvedení poskytovatele stavu relace ASP.NET pro verzi Preview Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)
* [Blog: RavenHQ nyní k dispozici v hello Azure Marketplace](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Velký objem dat
* [Instalace na virtuálních počítačích Azure Linux Hadoop](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
* [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relační databáze
* [Architektura MySQL vysokou dostupnost v Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
* [Instalace Postgres s corosync, pg_bouncer pomocí ILB](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux s vysokým výkonem (HPC)
* [Šablony rychlý start: číselníku clusteru SLURM](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (a [příspěvku na blogu](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
* [Šablony rychlý start: vytvoření clusteru prostředí HPC s Linux výpočetních uzlů](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, správu a optimalizace
Hello world devops, správu a optimalizace je poměrně obsáhlou a změna velmi rychle, měli byste zvážit hello seznamu počáteční bod.

* [Video: Virtuální počítače Azure: pomocí Chef, Puppet a Dockeru pro správu virtuálních počítačů Linux](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)
* [Dokončení Průvodce tooautomated Kubernetes nasazení clusteru s CoreOS a vlákna](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
* [Vizualizátor Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Podřízený volaných modulu Plug-in pro Azure.](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
* [Úložiště GitHub: Plug-In Jenkins Storage pro Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
* [Jiný výrobce: Plug-in Hudson Slave pro Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
* [Jiný výrobce: Plug-in Hudson Storage pro Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Video: Co je Chef a jak to funguje?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)
* [Video: Jak tooUse Azure Automation se virtuální počítače s Linuxem](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* [Blog: Jak toodo Powershell DSC pro Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
* [GitHub: DockerClientDSC](https://github.com/anweiss/DockerClientDSC)
* [Modul plug-in balírna pro Azure](https://github.com/msopentech/packer-azure)

