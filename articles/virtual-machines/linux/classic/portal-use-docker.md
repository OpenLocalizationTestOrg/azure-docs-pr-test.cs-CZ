---
title: "Pomocí Docker rozšíření virtuálního počítače pro Linux | Microsoft Docs"
description: "Popisuje Docker a rozšíření Azure virtuálních počítačů a postupy k vytvoření virtuálních počítačů Azure, které jsou hostitelů docker pomocí rozhraní příkazového řádku Azure v modelu nasazení classic."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 19cf64e8-f92c-43ad-a120-8976cd9102ac
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/27/2016
ms.author: rasquill
ms.openlocfilehash: 932744208d9d53c87e31dcdf9e34539750be4bdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a><span data-ttu-id="54bcf-103">Použití rozšíření Docker VM s klasickým portálem Azure</span><span class="sxs-lookup"><span data-stu-id="54bcf-103">Using the Docker VM Extension with the Azure classic portal</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="54bcf-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="54bcf-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="54bcf-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="54bcf-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="54bcf-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="54bcf-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="54bcf-107">[Docker](https://www.docker.com/) je jednou z nejčastěji používané virtualizace přístupy, které používá [Linux kontejnery](http://en.wikipedia.org/wiki/LXC) místo virtuální počítače jako způsob oddělením dat a výpočty na sdílených prostředků.</span><span class="sxs-lookup"><span data-stu-id="54bcf-107">[Docker](https://www.docker.com/) is one of the most popular virtualization approaches that uses [Linux containers](http://en.wikipedia.org/wiki/LXC) rather than virtual machines as a way of isolating data and computing on shared resources.</span></span> <span data-ttu-id="54bcf-108">Můžete použít rozšíření virtuálního počítače Docker spravuje [Azure Linux Agent] vytvoření Docker virtuálního počítače, který je hostitelem libovolný počet kontejnerů pro vaše aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="54bcf-108">You can use the Docker VM extension managed by [Azure Linux Agent] to create a Docker VM that hosts any number of containers for your applications on Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="54bcf-109">Toto téma popisuje, jak vytvořit virtuální počítač Docker z portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="54bcf-109">This topic describes how to create a Docker VM from the Azure classic portal.</span></span> <span data-ttu-id="54bcf-110">Jak vytvořit virtuální počítač Docker na příkazovém řádku, najdete v sekci [jak používat rozšíření virtuálního počítače z rozhraní příkazového řádku (Azure CLI) Docker].</span><span class="sxs-lookup"><span data-stu-id="54bcf-110">To see how to create a Docker VM at the command line, see [How to use the Docker VM Extension from the Azure Command-line Interface (Azure CLI)].</span></span> <span data-ttu-id="54bcf-111">Podrobný popis kontejnery a jejich výhody, najdete v sekci [Docker vysokou úroveň tabulí](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span><span class="sxs-lookup"><span data-stu-id="54bcf-111">To see a high-level discussion of containers and their advantages, see the [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span></span>
> 
> 

## <a name="create-a-new-vm-from-the-image-gallery"></a><span data-ttu-id="54bcf-112">Vytvoření nového virtuálního počítače z Galerie obrázků</span><span class="sxs-lookup"><span data-stu-id="54bcf-112">Create a new VM from the Image Gallery</span></span>
<span data-ttu-id="54bcf-113">Prvním krokem vyžaduje virtuální počítač Azure z Linux bitovou kopii, která podporuje Docker rozšíření virtuálního počítače, pomocí image Ubuntu 14.04 LTS z Galerie obrázků jako obrázek příkladu server a Ubuntu 14.04 Desktop jako klient.</span><span class="sxs-lookup"><span data-stu-id="54bcf-113">The first step requires an Azure VM from a Linux image that supports the Docker VM Extension, using an Ubuntu 14.04 LTS image from the Image Gallery as an example server image and Ubuntu 14.04 Desktop as a client.</span></span> <span data-ttu-id="54bcf-114">Na portálu, klikněte na tlačítko **+ nový** v levém dolním rohu k vytvoření nové instance virtuálního počítače a vyberte bitovou kopii Ubuntu 14.04 LTS z dostupných možnostech nebo dokončení Galerie obrázků, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="54bcf-114">In the portal, click **+ New** in the bottom left corner to create a new VM instance and select an Ubuntu 14.04 LTS image from the selections available or from the complete Image Gallery, as shown below.</span></span>

> [!NOTE]
> <span data-ttu-id="54bcf-115">V současné době podporují pouze Ubuntu 14.04 LTS obrázky novější než červenec 2014 Docker rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="54bcf-115">Currently, only Ubuntu 14.04 LTS images more recent than July 2014 support the Docker VM Extension.</span></span>
> 
> 

![Vytvořit novou bitovou kopii Ubuntu](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a><span data-ttu-id="54bcf-117">Vytvoření Docker certifikátů</span><span class="sxs-lookup"><span data-stu-id="54bcf-117">Create Docker Certificates</span></span>
<span data-ttu-id="54bcf-118">Po vytvoření virtuálního počítače, zkontrolujte, zda je na klientském počítači nainstalovaný Docker.</span><span class="sxs-lookup"><span data-stu-id="54bcf-118">After the VM has been created, ensure that Docker is installed on your client computer.</span></span> <span data-ttu-id="54bcf-119">(Podrobnosti najdete v tématu [pokyny k instalaci Docker](https://docs.docker.com/installation/#installation).)</span><span class="sxs-lookup"><span data-stu-id="54bcf-119">(For details, see [Docker installation instructions](https://docs.docker.com/installation/#installation).)</span></span>

<span data-ttu-id="54bcf-120">Vytvořte certifikát a klíč soubory pro komunikaci Docker podle [systémem Docker s protokolem https] a umístěte je do  **`~/.docker`**  adresář na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="54bcf-120">Create the certificate and key files for Docker communication according to [Running Docker with https] and place them in the **`~/.docker`** directory on your client computer.</span></span>

> [!NOTE]
> <span data-ttu-id="54bcf-121">Rozšíření virtuálního počítače Docker portálu aktuálně vyžaduje přihlašovací údaje, které kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="54bcf-121">The Docker VM Extension in the portal currently requires credentials that are base64 encoded.</span></span>
> 
> 

<span data-ttu-id="54bcf-122">Na příkazovém řádku použít  **`base64`**  nebo jiný Oblíbené kódování nástroj k vytváření témat s kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="54bcf-122">At the command line, use **`base64`** or another favorite encoding tool to create base64-encoded topics.</span></span> <span data-ttu-id="54bcf-123">Díky tomuto s jednoduchou sadou souborů certifikát a klíč může vypadat podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="54bcf-123">Doing this with a simple set of certificate and key files might look similar to this:</span></span>

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a><span data-ttu-id="54bcf-124">Přidání rozšíření virtuálního počítače Docker</span><span class="sxs-lookup"><span data-stu-id="54bcf-124">Add the Docker VM Extension</span></span>
<span data-ttu-id="54bcf-125">Chcete-li přidat rozšíření virtuálního počítače Docker, najděte instance virtuálního počítače, který jste vytvořili a přejděte dolů k položce **rozšíření** a klikněte na něj se zprovoznit rozšíření virtuálních počítačů, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="54bcf-125">To add the Docker VM Extension, locate the VM instance you created and scroll down to **Extensions** and click it to bring up VM Extensions, as shown below.</span></span>

> [!NOTE]
> <span data-ttu-id="54bcf-126">Tato funkce je podporována pouze na portálu preview: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="54bcf-126">This functionality is supported in the preview portal only: https://portal.azure.com/</span></span>
> 
> 

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a><span data-ttu-id="54bcf-127">Přidání rozšíření</span><span class="sxs-lookup"><span data-stu-id="54bcf-127">Add an Extension</span></span>
<span data-ttu-id="54bcf-128">Klikněte **+ přidat** zobrazíte možné rozšíření virtuálních počítačů můžete přidat do tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="54bcf-128">Click the **+ Add** to display the possible VM Extensions you can add to this VM.</span></span>

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-the-docker-vm-extension"></a><span data-ttu-id="54bcf-129">Vyberte rozšíření virtuálního počítače Docker</span><span class="sxs-lookup"><span data-stu-id="54bcf-129">Select the Docker VM Extension</span></span>
<span data-ttu-id="54bcf-130">Zvolte Docker rozšíření virtuálního počítače, které se vyvolá Docker popis a důležité odkazy, a pak klikněte na tlačítko **vytvořit** v dolní části zahájíte proces instalace.</span><span class="sxs-lookup"><span data-stu-id="54bcf-130">Choose the Docker VM Extension, which brings up the Docker description and important links, and then click **Create** at the bottom to begin the installation procedure.</span></span>

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a><span data-ttu-id="54bcf-131">Přidáte certifikát a soubory klíčů:</span><span class="sxs-lookup"><span data-stu-id="54bcf-131">Add your Certificate and Key Files:</span></span>
<span data-ttu-id="54bcf-132">Pole formuláře zadejte kódováním base64 verze váš certifikát certifikační Autority, certifikátu serveru a váš klíč serveru, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="54bcf-132">In the form fields, enter the base64-encoded versions of your CA Certificate, your Server Certificate, and your Server Key, as shown in the following graphic.</span></span>

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> <span data-ttu-id="54bcf-133">Všimněte si, že (jako v předchozí obrázek). 2376 se vyplní ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="54bcf-133">Note that (as in the preceding image) the 2376 is filled in by default.</span></span> <span data-ttu-id="54bcf-134">Můžete zadat libovolný koncový bod, ale dalším krokem bude otevře odpovídající koncový bod.</span><span class="sxs-lookup"><span data-stu-id="54bcf-134">You can enter any endpoint here, but the next step will be to open up the matching endpoint.</span></span> <span data-ttu-id="54bcf-135">Pokud změníte výchozí, ujistěte se, že jste otevře odpovídající koncový bod v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="54bcf-135">If you change the default, make sure to open up the matching endpoint in the next step.</span></span>
> 
> 

## <a name="add-the-docker-communication-endpoint"></a><span data-ttu-id="54bcf-136">Přidání koncového bodu komunikace Docker</span><span class="sxs-lookup"><span data-stu-id="54bcf-136">Add the Docker Communication Endpoint</span></span>
<span data-ttu-id="54bcf-137">Při zobrazení skupiny prostředků, který jste vytvořili, vyberte skupinu zabezpečení sítě spojené s virtuální počítač a klikněte na **příchozí pravidla zabezpečení** zobrazte pravidla, jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="54bcf-137">When viewing the resource group you've created, select the Network Security Group associated with your VM, and click **Inbound Security Rules** to view the rules as shown here.</span></span>

![](media/portal-use-docker/AddingEndpoint.png)

<span data-ttu-id="54bcf-138">Klikněte na tlačítko **+ přidat** chcete přidat jiné pravidlo a v případě výchozí, zadejte název pro koncový bod (v tomto příkladu **Docker**) a. 2376 'rozsah cílových portů'.</span><span class="sxs-lookup"><span data-stu-id="54bcf-138">Click **+ Add** to add another rule, and in the default case, enter a name for the endpoint (in this example, **Docker**), and 2376 'Destination Port Range'.</span></span> <span data-ttu-id="54bcf-139">Nastavit zobrazuje hodnota protokol **TCP**a klikněte na tlačítko **OK** vytvoření pravidla.</span><span class="sxs-lookup"><span data-stu-id="54bcf-139">Set the protocol value showing **TCP**, and Click **OK** to create the rule.</span></span>

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a><span data-ttu-id="54bcf-140">Testování vaší Docker klientských a hostitelských Azure Docker</span><span class="sxs-lookup"><span data-stu-id="54bcf-140">Test your Docker Client and Azure Docker Host</span></span>
<span data-ttu-id="54bcf-141">Vyhledejte a zkopírujte název domény Virtuálního počítače a na příkazovém řádku klientského počítače, typ `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (kde *dockerextension* je nahrazena poddomény pro virtuální počítač).</span><span class="sxs-lookup"><span data-stu-id="54bcf-141">Locate and copy the name of your VM's domain, and at the command line of your client computer, type `docker --tls -H tcp://`*dockerextension*`.cloudapp.net:2376 info` (where *dockerextension* is replaced by the subdomain for your VM).</span></span>

<span data-ttu-id="54bcf-142">Výsledek by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="54bcf-142">The result should appear similar to this:</span></span>

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

<span data-ttu-id="54bcf-143">Po dokončení výše uvedené kroky máte nyní plně funkční Docker hostitele se systémem na virtuální počítač Azure, nakonfigurované k připojení k vzdáleně z jiných klientů.</span><span class="sxs-lookup"><span data-stu-id="54bcf-143">Once you complete the above steps, you now have a fully functional Docker host running on an Azure VM, configured to be connected to remotely from other clients.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="54bcf-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54bcf-144">Next steps</span></span>
<span data-ttu-id="54bcf-145">Jste připravení přejít ke [Docker uživatelská příručka] a používat virtuální počítač Docker.</span><span class="sxs-lookup"><span data-stu-id="54bcf-145">You are ready to go to the [Docker User Guide] and use your Docker VM.</span></span> <span data-ttu-id="54bcf-146">Pokud chcete automatizovat vytváření hostitelů Docker na virtuálních počítačích Azure prostřednictvím rozhraní příkazového řádku, přečtěte si téma [jak používat rozšíření virtuálního počítače z rozhraní příkazového řádku (Azure CLI) Docker]</span><span class="sxs-lookup"><span data-stu-id="54bcf-146">If you want to automate creating Docker hosts on Azure VMs through command line interface, see [How to use the Docker VM Extension from the Azure Command-line Interface (Azure CLI)]</span></span>

<!--Anchors-->
[Create a new VM from the Image Gallery]:#createvm
[Create Docker Certificates]:#dockercerts
[Add the Docker VM Extension]:#adddockerextension
[Test Docker Client and Azure Docker Host]:#testclientandserver
[Next steps]:#next-steps

<!--Image references-->
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[6]:./media/markdown-template-for-new-articles/pretty49.png
[7]:./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
<span data-ttu-id="54bcf-147">[jak používat rozšíření virtuálního počítače z rozhraní příkazového řádku (Azure CLI) Docker]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/</span><span class="sxs-lookup"><span data-stu-id="54bcf-147">[How to use the Docker VM Extension from the Azure Command-line Interface (Azure CLI)]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/</span></span>
<span data-ttu-id="54bcf-148">[Azure Linux Agent]:../agent-user-guide.md</span><span class="sxs-lookup"><span data-stu-id="54bcf-148">[Azure Linux Agent]:../agent-user-guide.md</span></span>
[Link 3 to another azure.microsoft.com documentation topic]:../storage-whatis-account.md

<span data-ttu-id="54bcf-149">[systémem Docker s protokolem https]:http://docs.docker.com/articles/https/</span><span class="sxs-lookup"><span data-stu-id="54bcf-149">[Running Docker with https]:http://docs.docker.com/articles/https/</span></span>
<span data-ttu-id="54bcf-150">[Docker uživatelská příručka]:https://docs.docker.com/userguide/</span><span class="sxs-lookup"><span data-stu-id="54bcf-150">[Docker User Guide]:https://docs.docker.com/userguide/</span></span>
