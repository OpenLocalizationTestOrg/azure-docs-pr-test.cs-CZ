---
title: "hello aaaUsing Docker rozšíření virtuálního počítače pro systém Linux na Azure"
description: "Popisuje Docker a rozšíření Azure Virtual Machines hello a ukazuje, jak tooprogrammatically vytvořit virtuální počítače na platformě Azure, které jsou hostitelů docker z příkazového řádku hello pomocí hello rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: eaff75e3-d929-4931-a4a1-8c377a8e7302
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/29/2016
ms.author: rasquill
ms.openlocfilehash: 1e192ad7c273aa9c997ea7bfa53b7de0b41a43c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-from-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="5a575-103">Pomocí hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="5a575-103">Using hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="5a575-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5a575-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5a575-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="5a575-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="5a575-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="5a575-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="5a575-107">Informace o rozšíření virtuálního počítače Docker hello pomocí modelu Resource Manager hello najdete v tématu [zde](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5a575-107">For information about using hello Docker VM extension with hello Resource Manager model, see [here](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="5a575-108">Toto téma popisuje, jak toocreate virtuální počítač s hello Docker rozšíření virtuálního počítače z hello služby režim správy (asm) v rozhraní příkazového řádku Azure na jakékoli platformě.</span><span class="sxs-lookup"><span data-stu-id="5a575-108">This topic describes how toocreate a VM with hello Docker VM Extension from hello service management (asm) mode in Azure CLI on any platform.</span></span> <span data-ttu-id="5a575-109">[Docker](https://www.docker.com/) je jedním z hello nejoblíbenější virtualizace přístupy, které používá [Linux kontejnery](http://en.wikipedia.org/wiki/LXC) místo virtuální počítače jako způsob oddělením dat a výpočty na sdílených prostředků.</span><span class="sxs-lookup"><span data-stu-id="5a575-109">[Docker](https://www.docker.com/) is one of hello most popular virtualization approaches that uses [Linux containers](http://en.wikipedia.org/wiki/LXC) rather than virtual machines as a way of isolating data and computing on shared resources.</span></span> <span data-ttu-id="5a575-110">Můžete použít rozšíření virtuálního počítače Docker hello a hello [Azure Linux Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate Docker virtuální počítač, který je hostitelem libovolný počet kontejnerů pro vaše aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="5a575-110">You can use hello Docker VM extension and hello [Azure Linux Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate a Docker VM that hosts any number of containers for your applications on Azure.</span></span> <span data-ttu-id="5a575-111">toosee podrobný popis kontejnery a jejich výhody, najdete v části hello [Docker vysokou úroveň tabulí](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span><span class="sxs-lookup"><span data-stu-id="5a575-111">toosee a high-level discussion of containers and their advantages, see hello [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span></span>

## <a name="how-toouse-hello-docker-vm-extension-with-azure"></a><span data-ttu-id="5a575-112">Jak toouse hello Docker rozšíření virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="5a575-112">How toouse hello Docker VM Extension with Azure</span></span>
<span data-ttu-id="5a575-113">toouse hello Docker rozšíření virtuálního počítače Azure, musíte nainstalovat verzi hello [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) vyšší než 0.8.6 (jako je tento zápis hello aktuální verze je 0.10.0).</span><span class="sxs-lookup"><span data-stu-id="5a575-113">toouse hello Docker VM extension with Azure, you must install a version of hello [Azure Command-Line Interface](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) higher than 0.8.6 (as of this writing hello current version is 0.10.0).</span></span> <span data-ttu-id="5a575-114">Hello rozhraní příkazového řádku Azure můžete nainstalovat na Mac, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="5a575-114">You can install hello Azure CLI on Mac, Linux, and Windows.</span></span>

<span data-ttu-id="5a575-115">dokončení procesu toouse Hello Docker v Azure je jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="5a575-115">hello complete process toouse Docker on Azure is simple:</span></span>

* <span data-ttu-id="5a575-116">Nainstalujte hello rozhraní příkazového řádku Azure a jeho závislosti hello počítače, ze kterého mají být toocontrol Azure (v systému Windows, bude jím Linux distribuční spuštěná jako virtuální počítač)</span><span class="sxs-lookup"><span data-stu-id="5a575-116">Install hello Azure CLI and its dependencies on hello computer from which you want toocontrol Azure (on Windows, this will be a Linux distribution running as a virtual machine)</span></span>
* <span data-ttu-id="5a575-117">Použít hello Azure CLI Docker příkazy toocreate hostitelů Docker virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="5a575-117">Use hello Azure CLI Docker commands toocreate a VM Docker host in Azure</span></span>
* <span data-ttu-id="5a575-118">Použijte hello místní Docker příkazy toomanage Docker kontejnerů v Docker virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="5a575-118">Use hello local Docker commands toomanage your Docker containers in your Docker VM in Azure.</span></span>

### <a name="install-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="5a575-119">Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="5a575-119">Install hello Azure Command-Line Interface (Azure CLI)</span></span>
<span data-ttu-id="5a575-120">tooinstall a nakonfigurovat hello příkazového řádku Azure CLI, najdete v části [jak tooinstall hello rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5a575-120">tooinstall and configure hello Azure CLI, see [How tooinstall hello Azure Command-Line Interface](../../../cli-install-nodejs.md).</span></span> <span data-ttu-id="5a575-121">tooconfirm hello instalace typu `azure` hello příkazového řádku a po krátké chvíli byste měli vidět, hello obrázky ASCII rozhraní příkazového řádku Azure, kde jsou uvedeny hello basic příkazy tooyou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5a575-121">tooconfirm hello installation, type `azure` at hello command prompt and after a short moment you should see hello Azure CLI ASCII art, which lists hello basic commands available tooyou.</span></span> <span data-ttu-id="5a575-122">Pokud instalace hello fungovala správně, musí být schopný tootype `azure help vm` a zjistíte, že jeden z uvedených hello příkazů "docker".</span><span class="sxs-lookup"><span data-stu-id="5a575-122">If hello installation worked correctly, you should be able tootype `azure help vm` and see that one of hello listed commands is "docker".</span></span>

> [!NOTE]
> <span data-ttu-id="5a575-123">Docker má nástroje pro systém Windows, [počítač Docker](https://docs.docker.com/installation/windows/), které můžete použít také vytvoření hello tooautomate docker klienta, které jako hostitelů docker můžete toowork s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="5a575-123">Docker has tools for Windows, [Docker Machine](https://docs.docker.com/installation/windows/), which you can also use tooautomate hello creation of a docker client that you can use toowork with Azure VMs as docker hosts.</span></span>
> 
> 

### <a name="connect-hello-azure-cli-tootooyour-azure-account"></a><span data-ttu-id="5a575-124">Připojení rozhraní příkazového řádku Azure tootooyour hello účet Azure</span><span class="sxs-lookup"><span data-stu-id="5a575-124">Connect hello Azure CLI tootooyour Azure Account</span></span>
<span data-ttu-id="5a575-125">Než budete moci použít hello rozhraní příkazového řádku Azure je nutné přidružit přihlašovací údaje účtu Azure s hello rozhraní příkazového řádku Azure na vaši platformu.</span><span class="sxs-lookup"><span data-stu-id="5a575-125">Before you can use hello Azure CLI you must associate your Azure account credentials with hello Azure CLI on your platform.</span></span> <span data-ttu-id="5a575-126">Hello části [jak tooconnect tooyour předplatného Azure](../../../xplat-cli-connect.md) vysvětluje, jak tooeither stahování a import vaše **.publishsettings** souboru nebo Azure CLI přidružit id organizace.</span><span class="sxs-lookup"><span data-stu-id="5a575-126">hello section [How tooconnect tooyour Azure subscription](../../../xplat-cli-connect.md) explains how tooeither download and import your **.publishsettings** file or associate your Azure CLI with an organizational id.</span></span>

> [!NOTE]
> <span data-ttu-id="5a575-127">Existují určité rozdíly v chování při použití jednoho nebo hello jiných metod ověřování, takže se zda dokument hello tooread výše toounderstand hello různé funkce.</span><span class="sxs-lookup"><span data-stu-id="5a575-127">There are some differences in behavior when using one or hello other methods of authentication, so do be sure tooread hello document above toounderstand hello different functionality.</span></span>
> 
> 

### <a name="install-docker-and-use-hello-docker-vm-extension-for-azure"></a><span data-ttu-id="5a575-128">Docker nainstalovat a používat hello Docker rozšíření virtuálního počítače pro Azure.</span><span class="sxs-lookup"><span data-stu-id="5a575-128">Install Docker and use hello Docker VM Extension for Azure</span></span>
<span data-ttu-id="5a575-129">Postupujte podle hello [pokyny k instalaci Docker](https://docs.docker.com/installation/#installation) tooinstall Docker místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5a575-129">Follow hello [Docker installation instructions](https://docs.docker.com/installation/#installation) tooinstall Docker locally on your computer.</span></span>

<span data-ttu-id="5a575-130">toouse Docker s virtuální počítač Azure, hello Linux obrázek použitý pro hello virtuálního počítače musí mít hello [agenta virtuálního počítače Azure Linux](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="5a575-130">toouse Docker with an Azure Virtual Machine, hello Linux image used for hello VM must have hello [Azure Linux VM Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) installed.</span></span> <span data-ttu-id="5a575-131">V současné době existují jenom dva typy bitových kopií, které poskytují toto:</span><span class="sxs-lookup"><span data-stu-id="5a575-131">Currently, there are only two types of images that provide this:</span></span>

* <span data-ttu-id="5a575-132">Ubuntu obrázek z Galerie obrázků Azure hello nebo</span><span class="sxs-lookup"><span data-stu-id="5a575-132">An Ubuntu image from hello Azure Image Gallery or</span></span>
* <span data-ttu-id="5a575-133">Vlastní image Linux, kterou jste vytvořili pomocí hello Azure Linux Agent virtuálního počítače nainstalovaný a nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="5a575-133">A custom Linux image that you have created with hello Azure Linux VM Agent installed and configured.</span></span> <span data-ttu-id="5a575-134">V tématu [agenta virtuálního počítače Azure Linux](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pro další informace o toobuild vlastní virtuální počítač s Linuxem pomocí hello agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="5a575-134">See [Azure Linux VM Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information about how toobuild a custom Linux VM with hello Azure VM Agent.</span></span>

### <a name="using-hello-azure-image-gallery"></a><span data-ttu-id="5a575-135">Pomocí Galerie obrázků Azure hello</span><span class="sxs-lookup"><span data-stu-id="5a575-135">Using hello Azure Image Gallery</span></span>
<span data-ttu-id="5a575-136">Z Bash nebo relaci Terminálové služby použijte následující příkaz rozhraní příkazového řádku Azure toolocate hello nejnovější bitovou kopii Ubuntu v toouse Galerie virtuálních počítačů hello zadáním hello</span><span class="sxs-lookup"><span data-stu-id="5a575-136">From a Bash or Terminal session, use hello following Azure CLI command toolocate hello most recent Ubuntu image in hello VM gallery toouse by typing</span></span>

`azure vm image list | grep Ubuntu-14_04`

<span data-ttu-id="5a575-137">a vyberte jednu z hello image názvy, například `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, a použijte hello následující příkaz toocreate nového virtuálního počítače pomocí této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="5a575-137">and select one of hello image names, such as `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, and use hello following command toocreate a new VM using that image.</span></span>

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

<span data-ttu-id="5a575-138">Kde:</span><span class="sxs-lookup"><span data-stu-id="5a575-138">where:</span></span>

* <span data-ttu-id="5a575-139">*&lt;název virtuálního počítače cloudservice&gt;*  je název hello hello virtuální počítač, který se stane hello Docker kontejneru hostitelský počítač v Azure</span><span class="sxs-lookup"><span data-stu-id="5a575-139">*&lt;vm-cloudservice name&gt;* is hello name of hello VM that will become hello Docker container host computer in Azure</span></span>
* <span data-ttu-id="5a575-140">*&lt;uživatelské jméno&gt;*  je uživatelské jméno hello hello výchozí kořenový uživatel hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5a575-140">*&lt;username&gt;* is hello username of hello default root user of hello VM</span></span>
* <span data-ttu-id="5a575-141">*&lt;heslo&gt;*  je hello heslo hello *uživatelské jméno* účet, který splňuje standardy hello složitější pro Azure.</span><span class="sxs-lookup"><span data-stu-id="5a575-141">*&lt;password&gt;* is hello password of hello *username* account that meets hello standards of complexity for Azure</span></span>

> [!NOTE]
> <span data-ttu-id="5a575-142">V současné době heslo musí být dlouhé alespoň 8 znaků, musí obsahovat jeden malá písmena a jedno velké písmeno, číslo a zvláštní znak například jeden z hello následující znaky: `!@#$%^&+=`.</span><span class="sxs-lookup"><span data-stu-id="5a575-142">Currently, a password must be at least 8 characters, contain one lower case and one upper case character, a number, and a special character such as one of hello following characters: `!@#$%^&+=`.</span></span> <span data-ttu-id="5a575-143">Ne, hello tečka na konci hello hello předcházející větu není speciální znak.</span><span class="sxs-lookup"><span data-stu-id="5a575-143">No, hello period at hello end of hello preceding sentence is NOT a special character.</span></span>
> 
> 

<span data-ttu-id="5a575-144">Pokud příkaz hello byl úspěšný, byste měli vidět něco podobného jako hello následující, v závislosti na přesné argumenty hello a možnosti, které používáte:</span><span class="sxs-lookup"><span data-stu-id="5a575-144">If hello command was successful, you should see something like hello following, depending on hello precise arguments and options you used:</span></span>

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> <span data-ttu-id="5a575-145">Vytvoření virtuálního počítače může trvat několik minut, ale poté, co se zřizují (hodnota stavu hello je `ReadyRole`) hello spustí Docker démon (hello Docker service) a hostitele kontejner Docker toohello se můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="5a575-145">Creating a virtual machine can take a few minutes, but after it has been provisioned (hello state value is `ReadyRole`) hello Docker daemon (hello Docker service) starts and you can connect toohello Docker container host.</span></span>
> 
> 

<span data-ttu-id="5a575-146">hello tootest Docker virtuálních počítačů, které jste vytvořili v Azure, typu</span><span class="sxs-lookup"><span data-stu-id="5a575-146">tootest hello Docker VM you have created in Azure, type</span></span>

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

<span data-ttu-id="5a575-147">kde  *&lt;vm název--použili&gt;*  je název hello hello virtuálního počítače, který jste použili v volání příliš`azure vm docker create`.</span><span class="sxs-lookup"><span data-stu-id="5a575-147">where *&lt;vm-name-you-used&gt;* is hello name of hello virtual machine that you used in your call too`azure vm docker create`.</span></span> <span data-ttu-id="5a575-148">Měli byste vidět něco podobné toohello následující text, který označuje, že je váš virtuální počítač Docker hostitele se spuštěným v Azure a čekání příkazech.</span><span class="sxs-lookup"><span data-stu-id="5a575-148">You should see something similar toohello following, which indicates that your Docker Host VM is up and running in Azure and waiting for your commands.</span></span> 

<span data-ttu-id="5a575-149">Nyní můžete zkusit tooconnect pomocí vaše informace o docker klienta tooobtain (v některých nastavení klienta Docker, jako je například v systému Mac, který může mít toouse `sudo`):</span><span class="sxs-lookup"><span data-stu-id="5a575-149">Now you can try tooconnect using your docker client tooobtain information (in some Docker client setups, such as that on Mac, you may have toouse `sudo`):</span></span>

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

<span data-ttu-id="5a575-150">Jenom toobe jisti, že je všechny funkční, můžete zkontrolovat hello virtuálních počítačů pro hello Docker rozšíření:</span><span class="sxs-lookup"><span data-stu-id="5a575-150">Just toobe certain that it's all working, you can examine hello VM for hello Docker extension:</span></span>

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a><span data-ttu-id="5a575-151">Ověřování docker hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5a575-151">Docker Host VM Authentication</span></span>
<span data-ttu-id="5a575-152">Kromě toho toocreating hello virtuální počítač Docker, hello `azure vm docker create` příkaz také automaticky vytvoří hello potřebné certifikáty tooallow Docker klientský počítač tooconnect toohello kontejner Azure hostiteli pomocí protokolu HTTPS a hello certifikáty jsou uložené na obou Hello klientských a hostitelských počítačů, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5a575-152">In addition toocreating hello Docker VM, hello `azure vm docker create` command also automatically creates hello necessary certificates tooallow your Docker client computer tooconnect toohello Azure container host using HTTPS, and hello certificates are stored on both hello client and host machines, as appropriate.</span></span> <span data-ttu-id="5a575-153">Při dalších pokusech hello existující certifikáty jsou opakovaně a sdílet s hello nového hostitele.</span><span class="sxs-lookup"><span data-stu-id="5a575-153">On subsequent attempts, hello existing certificates are reused and shared with hello new host.</span></span>

<span data-ttu-id="5a575-154">Ve výchozím nastavení, certifikáty jsou umístěny v `~/.docker`, a Docker budou nakonfigurované toorun na portu **. 2376**.</span><span class="sxs-lookup"><span data-stu-id="5a575-154">By default, certificates are placed in `~/.docker`, and Docker will be configured toorun on port **2376**.</span></span> <span data-ttu-id="5a575-155">Pokud byste chtěli toouse jiný port nebo adresář, pak můžete použít jednu z následujících akcí hello `azure vm docker create` příkazového řádku možnosti tooconfigure Docker kontejneru hostitele virtuálních počítačů toouse jiný port nebo různé certifikáty pro připojení klientů:</span><span class="sxs-lookup"><span data-stu-id="5a575-155">If you would like toouse a different port or directory, then you may use one of hello following `azure vm docker create` command line options tooconfigure your Docker container host VM toouse a different port or different certificates for connecting clients:</span></span>

```
-dp, --docker-port [port]              Port toouse for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

<span data-ttu-id="5a575-156">Hello Docker démon na hostiteli hello je nakonfigurované toolisten pro a ověření klienta připojení na hello zadaný port přes hello certifikáty generované hello `azure vm docker create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5a575-156">hello Docker daemon on hello host is configured toolisten for and authenticate client connections on hello specified port using hello certificates generated by hello `azure vm docker create` command.</span></span> <span data-ttu-id="5a575-157">Hello klientský počítač musí mít tyto certifikáty toogain přístup toohello Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="5a575-157">hello client machine must have these certificates toogain access toohello Docker host.</span></span>

> [!NOTE]
> <span data-ttu-id="5a575-158">Síťová hostitele se systémem bez tyto certifikáty budou snadno napadnutelný tooanyone, který může tooconnect toohello počítače.</span><span class="sxs-lookup"><span data-stu-id="5a575-158">A networked host running without these certificates will be vulnerable tooanyone that can tooconnect toohello machine.</span></span> <span data-ttu-id="5a575-159">Než změníte hello výchozí konfiguraci, ujistěte se, že rozumíte hello rizika tooyour počítače a aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a575-159">Before you modify hello default configuration, ensure that you understand hello risks tooyour computers and applications.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="5a575-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a575-160">Next steps</span></span>
* <span data-ttu-id="5a575-161">Jsou připraveny toogo toohello [Docker uživatelská příručka] a používat virtuální počítač Docker.</span><span class="sxs-lookup"><span data-stu-id="5a575-161">You are ready toogo toohello [Docker User Guide] and use your Docker VM.</span></span> <span data-ttu-id="5a575-162">toocreate virtuální počítač Docker povoleno hello nového portálu, najdete v části [jak toouse hello Docker rozšíření virtuálního počítače s hello portál].</span><span class="sxs-lookup"><span data-stu-id="5a575-162">toocreate a Docker-enabled VM in hello new portal, see [How toouse hello Docker VM Extension with hello Portal].</span></span>
* <span data-ttu-id="5a575-163">Hello rozšíření virtuálního počítače Azure Docker také podporuje Docker Compose, který používá deklarativní tootake souboru YAML aplikaci na vývojáře modelován v každém prostředí a generování konzistentní nasazení.</span><span class="sxs-lookup"><span data-stu-id="5a575-163">hello Azure Docker VM extension also supports Docker Compose, which uses a declarative YAML file tootake a developer-modeled application across any environment and generate a consistent deployment.</span></span> <span data-ttu-id="5a575-164">V tématu [začít pracovat s Docker a napište toodefine a spusťte aplikaci služby kontejneru na virtuální počítač Azure].</span><span class="sxs-lookup"><span data-stu-id="5a575-164">See [Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine].</span></span>  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How toouse hello Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 tooanother azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 tooanother azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md
[jak toouse hello Docker rozšíření virtuálního počítače s hello portál]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker uživatelská příručka]:https://docs.docker.com/userguide/

[začít pracovat s Docker a napište toodefine a spusťte aplikaci služby kontejneru na virtuální počítač Azure]:../docker-compose-quickstart.md
