---
title: "Pomocí Docker rozšíření virtuálního počítače pro systém Linux na Azure"
description: "Popisuje Docker a rozšíření Azure Virtual Machines a ukazuje, jak programově vytvářet virtuální počítače na platformě Azure, které jsou hostitelů docker z příkazového řádku pomocí rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: a542332c921862241f1f000e6a8f0a0ae0e8a934
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a><span data-ttu-id="b8569-103">Použití rozšíření Docker VM z rozhraní příkazového řádku Azure (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="b8569-103">Using the Docker VM Extension from the Azure Command-line Interface (Azure CLI)</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="b8569-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b8569-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b8569-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="b8569-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="b8569-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b8569-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="b8569-107">Informace o rozšíření virtuálního počítače Docker pomocí modelu Resource Manager najdete v tématu [zde](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8569-107">For information about using the Docker VM extension with the Resource Manager model, see [here](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="b8569-108">Toto téma popisuje postup vytvoření virtuálního počítače s Docker rozšíření virtuálního počítače z režimu service management (asm) v rozhraní příkazového řádku Azure na jakékoli platformě.</span><span class="sxs-lookup"><span data-stu-id="b8569-108">This topic describes how to create a VM with the Docker VM Extension from the service management (asm) mode in Azure CLI on any platform.</span></span> <span data-ttu-id="b8569-109">[Docker](https://www.docker.com/) je jednou z nejčastěji používané virtualizace přístupy, které používá [Linux kontejnery](http://en.wikipedia.org/wiki/LXC) místo virtuální počítače jako způsob oddělením dat a výpočty na sdílených prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8569-109">[Docker](https://www.docker.com/) is one of the most popular virtualization approaches that uses [Linux containers](http://en.wikipedia.org/wiki/LXC) rather than virtual machines as a way of isolating data and computing on shared resources.</span></span> <span data-ttu-id="b8569-110">Můžete použít rozšíření virtuálního počítače Docker a [Azure Linux Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vytvoření Docker virtuálního počítače, který je hostitelem libovolný počet kontejnerů pro vaše aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="b8569-110">You can use the Docker VM extension and the [Azure Linux Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to create a Docker VM that hosts any number of containers for your applications on Azure.</span></span> <span data-ttu-id="b8569-111">Podrobný popis kontejnery a jejich výhody, najdete v sekci [Docker vysokou úroveň tabulí](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span><span class="sxs-lookup"><span data-stu-id="b8569-111">To see a high-level discussion of containers and their advantages, see the [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span></span>

## <a name="how-to-use-the-docker-vm-extension-with-azure"></a><span data-ttu-id="b8569-112">Jak používat rozšíření virtuálního počítače Docker s Azure</span><span class="sxs-lookup"><span data-stu-id="b8569-112">How to use the Docker VM Extension with Azure</span></span>
<span data-ttu-id="b8569-113">Pokud chcete používat rozšíření virtuálního počítače Docker s Azure, musíte nainstalovat verzi [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) vyšší než 0.8.6 (jak psaní tohoto textu aktuální verze je 0.10.0).</span><span class="sxs-lookup"><span data-stu-id="b8569-113">To use the Docker VM extension with Azure, you must install a version of the [Azure Command-Line Interface](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) higher than 0.8.6 (as of this writing the current version is 0.10.0).</span></span> <span data-ttu-id="b8569-114">Rozhraní příkazového řádku Azure můžete nainstalovat na Mac, Linux a Windows.</span><span class="sxs-lookup"><span data-stu-id="b8569-114">You can install the Azure CLI on Mac, Linux, and Windows.</span></span>

<span data-ttu-id="b8569-115">Proces dokončení použít Docker v Azure je jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="b8569-115">The complete process to use Docker on Azure is simple:</span></span>

* <span data-ttu-id="b8569-116">Instalace rozhraní příkazového řádku Azure a jeho závislé součásti na počítači, ze kterého chcete řízení Azure (v systému Windows, bude jím Linux distribuční spuštěná jako virtuální počítač)</span><span class="sxs-lookup"><span data-stu-id="b8569-116">Install the Azure CLI and its dependencies on the computer from which you want to control Azure (on Windows, this will be a Linux distribution running as a virtual machine)</span></span>
* <span data-ttu-id="b8569-117">Vytvořit virtuální počítač Docker hostitele v Azure pomocí příkazů Docker rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b8569-117">Use the Azure CLI Docker commands to create a VM Docker host in Azure</span></span>
* <span data-ttu-id="b8569-118">Použijte místní Docker příkazy ke správě Docker kontejnerů v Docker virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="b8569-118">Use the local Docker commands to manage your Docker containers in your Docker VM in Azure.</span></span>

### <a name="install-the-azure-command-line-interface-azure-cli"></a><span data-ttu-id="b8569-119">Nainstalovat Azure rozhraní příkazového řádku (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="b8569-119">Install the Azure Command-Line Interface (Azure CLI)</span></span>
<span data-ttu-id="b8569-120">Instalace a konfigurace rozhraní příkazového řádku Azure CLI, najdete v části [postup instalace rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="b8569-120">To install and configure the Azure CLI, see [How to install the Azure Command-Line Interface](../../../cli-install-nodejs.md).</span></span> <span data-ttu-id="b8569-121">Pokud chcete ověřit instalaci, zadejte `azure` na příkazovém řádku a po krátké chvíli byste měli vidět obrázky ASCII rozhraní příkazového řádku Azure, který obsahuje seznam základních příkazů, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b8569-121">To confirm the installation, type `azure` at the command prompt and after a short moment you should see the Azure CLI ASCII art, which lists the basic commands available to you.</span></span> <span data-ttu-id="b8569-122">Pokud instalace fungovala správně, byste měli mít na typ `azure help vm` a zjistíte, že jeden z uvedených příkazů "docker".</span><span class="sxs-lookup"><span data-stu-id="b8569-122">If the installation worked correctly, you should be able to type `azure help vm` and see that one of the listed commands is "docker".</span></span>

> [!NOTE]
> <span data-ttu-id="b8569-123">Docker má nástroje pro systém Windows, [počítač Docker](https://docs.docker.com/installation/windows/), který můžete také použít k automatizaci tvorby docker klienta, který můžete použít pro práci s virtuálními počítači Azure jako hostitelů docker.</span><span class="sxs-lookup"><span data-stu-id="b8569-123">Docker has tools for Windows, [Docker Machine](https://docs.docker.com/installation/windows/), which you can also use to automate the creation of a docker client that you can use to work with Azure VMs as docker hosts.</span></span>
> 
> 

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a><span data-ttu-id="b8569-124">Azure CLI pro připojení k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="b8569-124">Connect the Azure CLI to to your Azure Account</span></span>
<span data-ttu-id="b8569-125">Abyste mohli používat rozhraní příkazového řádku Azure je nutné přidružit přihlašovací údaje účtu Azure pomocí Azure CLI na vaši platformu.</span><span class="sxs-lookup"><span data-stu-id="b8569-125">Before you can use the Azure CLI you must associate your Azure account credentials with the Azure CLI on your platform.</span></span> <span data-ttu-id="b8569-126">V části [jak se připojit k předplatnému Azure](../../../xplat-cli-connect.md) vysvětluje, jak stáhnout a naimportovat vaše **.publishsettings** souboru nebo Azure CLI přidružit id organizace.</span><span class="sxs-lookup"><span data-stu-id="b8569-126">The section [How to connect to your Azure subscription](../../../xplat-cli-connect.md) explains how to either download and import your **.publishsettings** file or associate your Azure CLI with an organizational id.</span></span>

> [!NOTE]
> <span data-ttu-id="b8569-127">Existují určité rozdíly v chování při použití jednoho nebo jiné metody ověřování, takže nezapomeňte si přečíst dokument výše a pochopit různé funkce.</span><span class="sxs-lookup"><span data-stu-id="b8569-127">There are some differences in behavior when using one or the other methods of authentication, so do be sure to read the document above to understand the different functionality.</span></span>
> 
> 

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a><span data-ttu-id="b8569-128">Nainstalujete Docker a Docker rozšíření virtuálního počítače pro Azure.</span><span class="sxs-lookup"><span data-stu-id="b8569-128">Install Docker and use the Docker VM Extension for Azure</span></span>
<span data-ttu-id="b8569-129">Postupujte podle [pokyny k instalaci Docker](https://docs.docker.com/installation/#installation) nainstalujte Docker místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="b8569-129">Follow the [Docker installation instructions](https://docs.docker.com/installation/#installation) to install Docker locally on your computer.</span></span>

<span data-ttu-id="b8569-130">Použití Docker s virtuální počítače Azure, musí mít bitovou kopii systému Linux používat pro virtuální počítač [agenta virtuálního počítače Azure Linux](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="b8569-130">To use Docker with an Azure Virtual Machine, the Linux image used for the VM must have the [Azure Linux VM Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) installed.</span></span> <span data-ttu-id="b8569-131">V současné době existují jenom dva typy bitových kopií, které poskytují toto:</span><span class="sxs-lookup"><span data-stu-id="b8569-131">Currently, there are only two types of images that provide this:</span></span>

* <span data-ttu-id="b8569-132">Ubuntu obrázek z Galerie obrázků Azure nebo</span><span class="sxs-lookup"><span data-stu-id="b8569-132">An Ubuntu image from the Azure Image Gallery or</span></span>
* <span data-ttu-id="b8569-133">Vlastní image Linux, kterou jste vytvořili pomocí Azure Linux virtuálního počítače Agent nainstalován a nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="b8569-133">A custom Linux image that you have created with the Azure Linux VM Agent installed and configured.</span></span> <span data-ttu-id="b8569-134">V tématu [agenta virtuálního počítače Azure Linux](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o tom, jak vytvářet vlastní virtuální počítač s Linuxem pomocí agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="b8569-134">See [Azure Linux VM Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information about how to build a custom Linux VM with the Azure VM Agent.</span></span>

### <a name="using-the-azure-image-gallery"></a><span data-ttu-id="b8569-135">Pomocí Galerie obrázků Azure</span><span class="sxs-lookup"><span data-stu-id="b8569-135">Using the Azure Image Gallery</span></span>
<span data-ttu-id="b8569-136">Z Bash nebo relaci Terminálové služby použijte následující příkaz rozhraní příkazového řádku Azure nalézt nejnovější Ubuntu obrázek v galerii virtuálních počítačů použít zadáním</span><span class="sxs-lookup"><span data-stu-id="b8569-136">From a Bash or Terminal session, use the following Azure CLI command to locate the most recent Ubuntu image in the VM gallery to use by typing</span></span>

`azure vm image list | grep Ubuntu-14_04`

<span data-ttu-id="b8569-137">a vyberte jednu z bitové kopie názvy, například `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`a použijte následující příkaz k vytvoření nového virtuálního počítače pomocí této bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b8569-137">and select one of the image names, such as `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, and use the following command to create a new VM using that image.</span></span>

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

<span data-ttu-id="b8569-138">Kde:</span><span class="sxs-lookup"><span data-stu-id="b8569-138">where:</span></span>

* <span data-ttu-id="b8569-139">*&lt;název virtuálního počítače cloudservice&gt;*  je název virtuálního počítače, který se stane kontejner Docker hostitelský počítač v Azure</span><span class="sxs-lookup"><span data-stu-id="b8569-139">*&lt;vm-cloudservice name&gt;* is the name of the VM that will become the Docker container host computer in Azure</span></span>
* <span data-ttu-id="b8569-140">*&lt;uživatelské jméno&gt;*  je uživatelské jméno uživatele root výchozí virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b8569-140">*&lt;username&gt;* is the username of the default root user of the VM</span></span>
* <span data-ttu-id="b8569-141">*&lt;heslo&gt;*  je heslo *uživatelské jméno* účet, který splňuje požadavky na složitost pro Azure.</span><span class="sxs-lookup"><span data-stu-id="b8569-141">*&lt;password&gt;* is the password of the *username* account that meets the standards of complexity for Azure</span></span>

> [!NOTE]
> <span data-ttu-id="b8569-142">V současné době heslo musí být dlouhé alespoň 8 znaků, musí obsahovat jeden malá písmena a jedno velké písmeno, číslo a zvláštní znak například jeden z následujících znaků: `!@#$%^&+=`.</span><span class="sxs-lookup"><span data-stu-id="b8569-142">Currently, a password must be at least 8 characters, contain one lower case and one upper case character, a number, and a special character such as one of the following characters: `!@#$%^&+=`.</span></span> <span data-ttu-id="b8569-143">Ne, tečka na konci předcházející věty není speciální znak.</span><span class="sxs-lookup"><span data-stu-id="b8569-143">No, the period at the end of the preceding sentence is NOT a special character.</span></span>
> 
> 

<span data-ttu-id="b8569-144">Pokud se příkaz úspěšně dokončil, byste měli vidět něco podobného jako následující příkaz, v závislosti na přesné argumentů a možnosti, které jste použili:</span><span class="sxs-lookup"><span data-stu-id="b8569-144">If the command was successful, you should see something like the following, depending on the precise arguments and options you used:</span></span>

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> <span data-ttu-id="b8569-145">Vytvoření virtuálního počítače může trvat několik minut, ale poté, co se zřizují (je hodnota stavu `ReadyRole`) spustí démon Docker (služba Docker) a může připojit k hostiteli kontejner Docker.</span><span class="sxs-lookup"><span data-stu-id="b8569-145">Creating a virtual machine can take a few minutes, but after it has been provisioned (the state value is `ReadyRole`) the Docker daemon (the Docker service) starts and you can connect to the Docker container host.</span></span>
> 
> 

<span data-ttu-id="b8569-146">Chcete-li otestovat Docker virtuálních počítačů, které jste vytvořili v Azure, zadejte</span><span class="sxs-lookup"><span data-stu-id="b8569-146">To test the Docker VM you have created in Azure, type</span></span>

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

<span data-ttu-id="b8569-147">kde  *&lt;vm název--použili&gt;*  je název virtuálního počítače, který jste použili v volání `azure vm docker create`.</span><span class="sxs-lookup"><span data-stu-id="b8569-147">where *&lt;vm-name-you-used&gt;* is the name of the virtual machine that you used in your call to `azure vm docker create`.</span></span> <span data-ttu-id="b8569-148">Měli byste vidět něco podobného jako následující příkaz, který označuje, zda je virtuální počítač Docker hostitele a běží v Azure a čekání příkazech.</span><span class="sxs-lookup"><span data-stu-id="b8569-148">You should see something similar to the following, which indicates that your Docker Host VM is up and running in Azure and waiting for your commands.</span></span> 

<span data-ttu-id="b8569-149">Nyní můžete zkusit připojení pomocí vašeho klienta docker se získat informace o (v některých nastavení klienta Docker, například v systému Mac, které možná budete muset použít `sudo`):</span><span class="sxs-lookup"><span data-stu-id="b8569-149">Now you can try to connect using your docker client to obtain information (in some Docker client setups, such as that on Mac, you may have to use `sudo`):</span></span>

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

<span data-ttu-id="b8569-150">Stačí si být jisti, že je všechny pracovní, můžete zkontrolovat ve virtuálním počítači Docker rozšíření:</span><span class="sxs-lookup"><span data-stu-id="b8569-150">Just to be certain that it's all working, you can examine the VM for the Docker extension:</span></span>

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a><span data-ttu-id="b8569-151">Ověřování docker hostitele virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b8569-151">Docker Host VM Authentication</span></span>
<span data-ttu-id="b8569-152">Kromě vytvoření virtuální počítač Docker `azure vm docker create` příkaz také automaticky vytvoří potřebné certifikáty umožňující klientský počítač Docker pro připojení k hostiteli kontejner Azure pomocí protokolu HTTPS a certifikáty jsou uložené na jak klientských a hostitelských počítačích, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="b8569-152">In addition to creating the Docker VM, the `azure vm docker create` command also automatically creates the necessary certificates to allow your Docker client computer to connect to the Azure container host using HTTPS, and the certificates are stored on both the client and host machines, as appropriate.</span></span> <span data-ttu-id="b8569-153">Na následné pokusy existující certifikáty jsou opakovaně a sdílet s novým hostitelem.</span><span class="sxs-lookup"><span data-stu-id="b8569-153">On subsequent attempts, the existing certificates are reused and shared with the new host.</span></span>

<span data-ttu-id="b8569-154">Ve výchozím nastavení, certifikáty jsou umístěny v `~/.docker`, a Docker bude nakonfigurován ke spuštění na portu **. 2376**.</span><span class="sxs-lookup"><span data-stu-id="b8569-154">By default, certificates are placed in `~/.docker`, and Docker will be configured to run on port **2376**.</span></span> <span data-ttu-id="b8569-155">Pokud chcete použít jiný port nebo adresář, pak můžete použít jednu z následujících `azure vm docker create` možnosti příkazového řádku ke konfiguraci vaší kontejner Docker hostitele virtuálního počítače použít jiný port nebo různé certifikáty pro připojení klientů:</span><span class="sxs-lookup"><span data-stu-id="b8569-155">If you would like to use a different port or directory, then you may use one of the following `azure vm docker create` command line options to configure your Docker container host VM to use a different port or different certificates for connecting clients:</span></span>

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

<span data-ttu-id="b8569-156">Démon Docker v hostiteli je nakonfigurován k naslouchání a ověření připojení klienta na zadaný port pomocí certifikáty generované infrastrukturou `azure vm docker create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="b8569-156">The Docker daemon on the host is configured to listen for and authenticate client connections on the specified port using the certificates generated by the `azure vm docker create` command.</span></span> <span data-ttu-id="b8569-157">Tyto certifikáty k získání přístupu k hostiteli Docker musí mít klientský počítač.</span><span class="sxs-lookup"><span data-stu-id="b8569-157">The client machine must have these certificates to gain access to the Docker host.</span></span>

> [!NOTE]
> <span data-ttu-id="b8569-158">Síťová hostitele se systémem bez těchto certifikátů bude citlivé na každý uživatel, který můžete připojit k počítači.</span><span class="sxs-lookup"><span data-stu-id="b8569-158">A networked host running without these certificates will be vulnerable to anyone that can to connect to the machine.</span></span> <span data-ttu-id="b8569-159">Než změníte výchozí konfiguraci, ujistěte se, že rozumíte rizika pro vaše počítače a aplikace.</span><span class="sxs-lookup"><span data-stu-id="b8569-159">Before you modify the default configuration, ensure that you understand the risks to your computers and applications.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="b8569-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8569-160">Next steps</span></span>
* <span data-ttu-id="b8569-161">Jste připravení přejít ke [Docker uživatelská příručka] a používat virtuální počítač Docker.</span><span class="sxs-lookup"><span data-stu-id="b8569-161">You are ready to go to the [Docker User Guide] and use your Docker VM.</span></span> <span data-ttu-id="b8569-162">Vytvoření virtuálního počítače s povoleným Docker v nového portálu, najdete v tématu [jak používat rozšíření virtuálního počítače Docker s portálem].</span><span class="sxs-lookup"><span data-stu-id="b8569-162">To create a Docker-enabled VM in the new portal, see [How to use the Docker VM Extension with the Portal].</span></span>
* <span data-ttu-id="b8569-163">Rozšíření virtuálního počítače Azure Docker podporuje také Docker Compose, která používá soubor deklarativní YAML trvat aplikaci na vývojáře modelován v každém prostředí a generování konzistentní nasazení.</span><span class="sxs-lookup"><span data-stu-id="b8569-163">The Azure Docker VM extension also supports Docker Compose, which uses a declarative YAML file to take a developer-modeled application across any environment and generate a consistent deployment.</span></span> <span data-ttu-id="b8569-164">V tématu [začít pracovat s Docker a vytvářené definovat a spusťte aplikaci služby kontejneru na virtuální počítač Azure].</span><span class="sxs-lookup"><span data-stu-id="b8569-164">See [Get Started with Docker and Compose to define and run a multi-container application on an Azure virtual machine].</span></span>  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How to use the Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]:../storage-whatis-account.md
<span data-ttu-id="b8569-165">[jak používat rozšíření virtuálního počítače Docker s portálem]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/</span><span class="sxs-lookup"><span data-stu-id="b8569-165">[How to use the Docker VM Extension with the Portal]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/</span></span>

<span data-ttu-id="b8569-166">[Docker uživatelská příručka]:https://docs.docker.com/userguide/</span><span class="sxs-lookup"><span data-stu-id="b8569-166">[Docker User Guide]:https://docs.docker.com/userguide/</span></span>

<span data-ttu-id="b8569-167">[začít pracovat s Docker a vytvářené definovat a spusťte aplikaci služby kontejneru na virtuální počítač Azure]:../docker-compose-quickstart.md</span><span class="sxs-lookup"><span data-stu-id="b8569-167">[Get Started with Docker and Compose to define and run a multi-container application on an Azure virtual machine]:../docker-compose-quickstart.md</span></span>
