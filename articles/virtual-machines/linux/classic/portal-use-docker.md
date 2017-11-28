---
title: "aaaUsing Docker rozšíření virtuálního počítače pro Linux | Microsoft Docs"
description: "Popisuje Docker a rozšíření Azure Virtual Machines hello a jak hello toocreate virtuálních počítačích Azure, které jsou hostitelů docker pomocí rozhraní příkazového řádku Azure v modelu nasazení classic."
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
ms.openlocfilehash: 9d455b63c48b0c1b6f14862e072f899a73b46153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-with-hello-azure-classic-portal"></a><span data-ttu-id="09c60-103">Hello Docker rozšíření virtuálního počítače pomocí hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="09c60-103">Using hello Docker VM Extension with hello Azure classic portal</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="09c60-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="09c60-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="09c60-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="09c60-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="09c60-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="09c60-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="09c60-107">[Docker](https://www.docker.com/) je jedním z hello nejoblíbenější virtualizace přístupy, které používá [Linux kontejnery](http://en.wikipedia.org/wiki/LXC) místo virtuální počítače jako způsob oddělením dat a výpočty na sdílených prostředků.</span><span class="sxs-lookup"><span data-stu-id="09c60-107">[Docker](https://www.docker.com/) is one of hello most popular virtualization approaches that uses [Linux containers](http://en.wikipedia.org/wiki/LXC) rather than virtual machines as a way of isolating data and computing on shared resources.</span></span> <span data-ttu-id="09c60-108">Můžete použít rozšíření virtuálního počítače Docker hello spravuje [Azure Linux Agent] toocreate Docker virtuální počítač, který je hostitelem libovolný počet kontejnerů pro vaše aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="09c60-108">You can use hello Docker VM extension managed by [Azure Linux Agent] toocreate a Docker VM that hosts any number of containers for your applications on Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="09c60-109">Toto téma popisuje, jak hello toocreate a virtuální počítač Docker z portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="09c60-109">This topic describes how toocreate a Docker VM from hello Azure classic portal.</span></span> <span data-ttu-id="09c60-110">toosee jak zjistit, toocreate virtuální počítač Docker v příkazovém řádku hello, [jak toouse hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)].</span><span class="sxs-lookup"><span data-stu-id="09c60-110">toosee how toocreate a Docker VM at hello command line, see [How toouse hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)].</span></span> <span data-ttu-id="09c60-111">toosee podrobný popis kontejnery a jejich výhody, najdete v části hello [Docker vysokou úroveň tabulí](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span><span class="sxs-lookup"><span data-stu-id="09c60-111">toosee a high-level discussion of containers and their advantages, see hello [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span></span>
> 
> 

## <a name="create-a-new-vm-from-hello-image-gallery"></a><span data-ttu-id="09c60-112">Vytvoření nového virtuálního počítače z Galerie obrázků hello</span><span class="sxs-lookup"><span data-stu-id="09c60-112">Create a new VM from hello Image Gallery</span></span>
<span data-ttu-id="09c60-113">prvním krokem Hello vyžaduje virtuální počítač Azure z Linux bitovou kopii, která podporuje hello Docker rozšíření virtuálního počítače, pomocí bitové kopie Ubuntu 14.04 LTS z Galerie obrázků hello jako obrázek příkladu server a Ubuntu 14.04 Desktop jako klient.</span><span class="sxs-lookup"><span data-stu-id="09c60-113">hello first step requires an Azure VM from a Linux image that supports hello Docker VM Extension, using an Ubuntu 14.04 LTS image from hello Image Gallery as an example server image and Ubuntu 14.04 Desktop as a client.</span></span> <span data-ttu-id="09c60-114">Hello portálu, klikněte na tlačítko **+ nový** v hello zůstane dolního rohu toocreate nové instance virtuálního počítače a vyberte bitovou kopii Ubuntu 14.04 LTS z dostupných možnostech hello nebo hello dokončete Galerie obrázků, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="09c60-114">In hello portal, click **+ New** in hello bottom left corner toocreate a new VM instance and select an Ubuntu 14.04 LTS image from hello selections available or from hello complete Image Gallery, as shown below.</span></span>

> [!NOTE]
> <span data-ttu-id="09c60-115">V současné době podporují pouze Ubuntu 14.04 LTS obrázky novější než červenec 2014 hello Docker rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="09c60-115">Currently, only Ubuntu 14.04 LTS images more recent than July 2014 support hello Docker VM Extension.</span></span>
> 
> 

![Vytvořit novou bitovou kopii Ubuntu](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a><span data-ttu-id="09c60-117">Vytvoření Docker certifikátů</span><span class="sxs-lookup"><span data-stu-id="09c60-117">Create Docker Certificates</span></span>
<span data-ttu-id="09c60-118">Po hello virtuálního počítače existuje, zkontrolujte, zda je nainstalován Docker na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="09c60-118">After hello VM has been created, ensure that Docker is installed on your client computer.</span></span> <span data-ttu-id="09c60-119">(Podrobnosti najdete v tématu [pokyny k instalaci Docker](https://docs.docker.com/installation/#installation).)</span><span class="sxs-lookup"><span data-stu-id="09c60-119">(For details, see [Docker installation instructions](https://docs.docker.com/installation/#installation).)</span></span>

<span data-ttu-id="09c60-120">Vytvořit certifikát a klíč soubory pro Docker komunikaci podle příliš hello[systémem Docker s protokolem https] a umístit je hello  **`~/.docker`**  adresář na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="09c60-120">Create hello certificate and key files for Docker communication according too[Running Docker with https] and place them in hello **`~/.docker`** directory on your client computer.</span></span>

> [!NOTE]
> <span data-ttu-id="09c60-121">Hello rozšíření virtuálního počítače Docker portálu hello aktuálně vyžaduje přihlašovací údaje, které kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="09c60-121">hello Docker VM Extension in hello portal currently requires credentials that are base64 encoded.</span></span>
> 
> 

<span data-ttu-id="09c60-122">Hello příkazového řádku, použijte  **`base64`**  nebo jiné Oblíbené kódování nástroj toocreate kódováním base64 témata.</span><span class="sxs-lookup"><span data-stu-id="09c60-122">At hello command line, use **`base64`** or another favorite encoding tool toocreate base64-encoded topics.</span></span> <span data-ttu-id="09c60-123">Díky tomuto s jednoduchou sadou souborů certifikát a klíč může vypadat podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="09c60-123">Doing this with a simple set of certificate and key files might look similar toothis:</span></span>

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

## <a name="add-hello-docker-vm-extension"></a><span data-ttu-id="09c60-124">Přidat hello Docker rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="09c60-124">Add hello Docker VM Extension</span></span>
<span data-ttu-id="09c60-125">tooadd hello Docker rozšíření virtuálního počítače, vyhledejte hello instance virtuálního počítače jste vytvořili a posuňte se dolů příliš**rozšíření** a klikněte na něj toobring si rozšíření virtuálních počítačů, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="09c60-125">tooadd hello Docker VM Extension, locate hello VM instance you created and scroll down too**Extensions** and click it toobring up VM Extensions, as shown below.</span></span>

> [!NOTE]
> <span data-ttu-id="09c60-126">Tato funkce je podporována na portálu preview hello pouze: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="09c60-126">This functionality is supported in hello preview portal only: https://portal.azure.com/</span></span>
> 
> 

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a><span data-ttu-id="09c60-127">Přidání rozšíření</span><span class="sxs-lookup"><span data-stu-id="09c60-127">Add an Extension</span></span>
<span data-ttu-id="09c60-128">Klikněte na tlačítko hello **+ přidat** toodisplay hello možné rozšíření virtuálních počítačů můžete přidat toothis virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="09c60-128">Click hello **+ Add** toodisplay hello possible VM Extensions you can add toothis VM.</span></span>

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-hello-docker-vm-extension"></a><span data-ttu-id="09c60-129">Vyberte hello Docker rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="09c60-129">Select hello Docker VM Extension</span></span>
<span data-ttu-id="09c60-130">Zvolte hello rozšíření Docker virtuálních počítačů, které se vyvolá hello Docker popis a důležité odkazy, a pak klikněte na tlačítko **vytvořit** na postup instalace hello dolní toobegin hello.</span><span class="sxs-lookup"><span data-stu-id="09c60-130">Choose hello Docker VM Extension, which brings up hello Docker description and important links, and then click **Create** at hello bottom toobegin hello installation procedure.</span></span>

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a><span data-ttu-id="09c60-131">Přidáte certifikát a soubory klíčů:</span><span class="sxs-lookup"><span data-stu-id="09c60-131">Add your Certificate and Key Files:</span></span>
<span data-ttu-id="09c60-132">Hello pole formuláře zadejte hello kódováním base64 verze váš certifikát certifikační Autority, certifikátu serveru a váš klíč serveru, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="09c60-132">In hello form fields, enter hello base64-encoded versions of your CA Certificate, your Server Certificate, and your Server Key, as shown in hello following graphic.</span></span>

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> <span data-ttu-id="09c60-133">Všimněte si, že (jako v předchozím obrázku hello) hello. 2376 se vyplní ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="09c60-133">Note that (as in hello preceding image) hello 2376 is filled in by default.</span></span> <span data-ttu-id="09c60-134">Můžete zadat libovolný koncový bod, ale hello dalším krokem bude tooopen až hello odpovídající koncový bod.</span><span class="sxs-lookup"><span data-stu-id="09c60-134">You can enter any endpoint here, but hello next step will be tooopen up hello matching endpoint.</span></span> <span data-ttu-id="09c60-135">Pokud změníte výchozí hello, ujistěte se, že tooopen až hello odpovídající koncového bodu v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="09c60-135">If you change hello default, make sure tooopen up hello matching endpoint in hello next step.</span></span>
> 
> 

## <a name="add-hello-docker-communication-endpoint"></a><span data-ttu-id="09c60-136">Přidat hello bod komunikace Docker</span><span class="sxs-lookup"><span data-stu-id="09c60-136">Add hello Docker Communication Endpoint</span></span>
<span data-ttu-id="09c60-137">Při zobrazení skupiny prostředků hello jste vytvořili, vyberte hello skupinu zabezpečení sítě spojené s virtuální počítač a klikněte na **příchozí pravidla zabezpečení** tooview hello pravidla, jak je vidět tady.</span><span class="sxs-lookup"><span data-stu-id="09c60-137">When viewing hello resource group you've created, select hello Network Security Group associated with your VM, and click **Inbound Security Rules** tooview hello rules as shown here.</span></span>

![](media/portal-use-docker/AddingEndpoint.png)

<span data-ttu-id="09c60-138">Klikněte na tlačítko **+ přidat** tooadd jiné pravidlo a v případě výchozí hello, zadejte název pro koncový bod hello (v tomto příkladu **Docker**) a. 2376 'rozsah cílových portů'.</span><span class="sxs-lookup"><span data-stu-id="09c60-138">Click **+ Add** tooadd another rule, and in hello default case, enter a name for hello endpoint (in this example, **Docker**), and 2376 'Destination Port Range'.</span></span> <span data-ttu-id="09c60-139">Nastavit hello protokol hodnota zobrazující **TCP**a klikněte na tlačítko **OK** toocreate hello pravidlo.</span><span class="sxs-lookup"><span data-stu-id="09c60-139">Set hello protocol value showing **TCP**, and Click **OK** toocreate hello rule.</span></span>

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a><span data-ttu-id="09c60-140">Testování vaší Docker klientských a hostitelských Azure Docker</span><span class="sxs-lookup"><span data-stu-id="09c60-140">Test your Docker Client and Azure Docker Host</span></span>
<span data-ttu-id="09c60-141">Vyhledejte a zkopírujte hello název domény Virtuálního počítače a v příkazovém řádku hello klientského počítače, typ `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (kde *dockerextension* je nahrazena hello poddomény pro virtuální počítač).</span><span class="sxs-lookup"><span data-stu-id="09c60-141">Locate and copy hello name of your VM's domain, and at hello command line of your client computer, type `docker --tls -H tcp://`*dockerextension*`.cloudapp.net:2376 info` (where *dockerextension* is replaced by hello subdomain for your VM).</span></span>

<span data-ttu-id="09c60-142">výsledek Hello by měla vypadat podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="09c60-142">hello result should appear similar toothis:</span></span>

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

<span data-ttu-id="09c60-143">Po dokončení hello výše kroky máte nyní plně funkční Docker hostitele se systémem na virtuální počítač Azure, nakonfigurované toobe připojené tooremotely z jiných klientů.</span><span class="sxs-lookup"><span data-stu-id="09c60-143">Once you complete hello above steps, you now have a fully functional Docker host running on an Azure VM, configured toobe connected tooremotely from other clients.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="09c60-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09c60-144">Next steps</span></span>
<span data-ttu-id="09c60-145">Jsou připraveny toogo toohello [Docker uživatelská příručka] a používat virtuální počítač Docker.</span><span class="sxs-lookup"><span data-stu-id="09c60-145">You are ready toogo toohello [Docker User Guide] and use your Docker VM.</span></span> <span data-ttu-id="09c60-146">Pokud chcete, aby tooautomate vytváření hostitelů Docker na virtuálních počítačích Azure prostřednictvím rozhraní příkazového řádku, přečtěte si téma [jak toouse hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)]</span><span class="sxs-lookup"><span data-stu-id="09c60-146">If you want tooautomate creating Docker hosts on Azure VMs through command line interface, see [How toouse hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)]</span></span>

<!--Anchors-->
[Create a new VM from hello Image Gallery]:#createvm
[Create Docker Certificates]:#dockercerts
[Add hello Docker VM Extension]:#adddockerextension
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
[jak toouse hello Docker rozšíření virtuálního počítače z hello rozhraní příkazového řádku Azure (Azure CLI)]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux Agent]:../agent-user-guide.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md

[systémem Docker s protokolem https]:http://docs.docker.com/articles/https/
[Docker uživatelská příručka]:https://docs.docker.com/userguide/
