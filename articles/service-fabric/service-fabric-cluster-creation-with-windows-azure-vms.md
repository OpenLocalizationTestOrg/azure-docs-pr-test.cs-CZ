---
title: "aaaCreate samostatný cluster s virtuálními počítači Azure s Windows | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat cluster služby Azure Service Fabric na virtuálních počítačích Azure s Windows serverem."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 7eeb40d2-fb22-4a77-80ca-f1b46b22edbc
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: ryanwi;chackdan
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: 8900204fe69887a7a0ca54b06e0d32534421bcfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a><span data-ttu-id="a94d3-103">Vytvoření clusteru Service Fabric samostatné tři uzly s virtuálními počítači Azure s Windows serverem</span><span class="sxs-lookup"><span data-stu-id="a94d3-103">Create a three node standalone Service Fabric cluster with Azure virtual machines running Windows Server</span></span>
<span data-ttu-id="a94d3-104">Tento článek popisuje, jak pomocí toocreate a clusteru v systému Windows Azure virtuální počítače (VM), hello samostatný instalační program Service Fabric systému pro Windows Server.</span><span class="sxs-lookup"><span data-stu-id="a94d3-104">This article describes how toocreate a cluster on Windows-based Azure virtual machines (VMs), using hello standalone Service Fabric installer for Windows Server.</span></span> <span data-ttu-id="a94d3-105">scénář Hello se zvláštním případem [vytvořit a spravovat cluster se systémem Windows Server](service-fabric-cluster-creation-for-windows-server.md) kde hello virtuální počítače jsou [virtuální počítače Azure se systémem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), ale nejsou vytváření [Azure cloudový cluster Service Fabric](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a94d3-105">hello scenario is a special case of [Create and manage a cluster running on Windows Server](service-fabric-cluster-creation-for-windows-server.md) where hello VMs are [Azure VMs running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), however you are not creating [an Azure cloud-based Service Fabric cluster](service-fabric-cluster-creation-via-portal.md).</span></span> <span data-ttu-id="a94d3-106">Hello rozdíl v následujících tento vzor je tento cluster Service Fabric hello samostatné vytvořené hello následující kroky zcela spravovaná, zatímco hello Azure Service Fabric cloudu clustery jsou spravovaná a upgradovat přes hello Service Fabric Zprostředkovatel prostředků.</span><span class="sxs-lookup"><span data-stu-id="a94d3-106">hello distinction in following this pattern is that hello standalone Service Fabric cluster created by hello following steps is entirely managed by you, whereas hello Azure cloud-based Service Fabric clusters are managed and upgraded by hello Service Fabric resource provider.</span></span>

## <a name="steps-toocreate-hello-standalone-cluster"></a><span data-ttu-id="a94d3-107">Kroky toocreate hello samostatné clusteru</span><span class="sxs-lookup"><span data-stu-id="a94d3-107">Steps toocreate hello standalone cluster</span></span>
1. <span data-ttu-id="a94d3-108">Přihlaste toohello portál Azure a vytvoření nového systému Windows Server 2012 R2 Datacenter nebo virtuálního počítače Windows serveru 2016 Datacenter ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a94d3-108">Sign in toohello Azure portal and create a new Windows Server 2012 R2 Datacenter or Windows Server 2016 Datacenter VM in a resource group.</span></span> <span data-ttu-id="a94d3-109">Přečíst článek hello [vytvoření virtuálního počítače s Windows v portálu Azure hello](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a94d3-109">Read hello article [Create a Windows VM in hello Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for more details.</span></span>
2. <span data-ttu-id="a94d3-110">Přidat několik více virtuálních počítačů toohello stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a94d3-110">Add a couple more VMs toohello same resource group.</span></span> <span data-ttu-id="a94d3-111">Ujistěte se, že každý hello virtuálních počítačů má hello stejné uživatelské jméno správce a heslo při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a94d3-111">Ensure that each of hello VMs has hello same administrator user name and password when created.</span></span> <span data-ttu-id="a94d3-112">Po vytvoření, měli byste vidět všechny tři virtuální počítače v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="a94d3-112">Once created you should see all three VMs in hello same virtual network.</span></span>
3. <span data-ttu-id="a94d3-113">Připojit tooeach hello virtuální počítače a vypnout nastavení hello brány Windows Firewall pomocí hello [správce serveru, řídicí panel místní Server](https://technet.microsoft.com/library/jj134147.aspx).</span><span class="sxs-lookup"><span data-stu-id="a94d3-113">Connect tooeach of hello VMs and turn off hello Windows Firewall using hello [Server Manager, Local Server dashboard](https://technet.microsoft.com/library/jj134147.aspx).</span></span> <span data-ttu-id="a94d3-114">Tím se zajistí, že může komunikovat hello síťový provoz mezi počítači hello.</span><span class="sxs-lookup"><span data-stu-id="a94d3-114">This ensures that hello network traffic can communicate between hello machines.</span></span> <span data-ttu-id="a94d3-115">Při počítače připojené tooeach získat IP adresu hello otevřením příkazového řádku a zadáním `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="a94d3-115">While connected tooeach machine, get hello IP address by opening a command prompt and typing `ipconfig`.</span></span> <span data-ttu-id="a94d3-116">Případně můžete zobrazit hello IP adresu každého počítače, na portálu hello výběrem zdroje hello virtuální sítě pro skupinu prostředků hello a kontrola hello síťová rozhraní pro každé z těchto počítačů.</span><span class="sxs-lookup"><span data-stu-id="a94d3-116">Alternatively you can see hello IP address of each machine on hello portal, by selecting hello virtual network resource for hello resource group and checking hello network interfaces created for each of these machines.</span></span>
4. <span data-ttu-id="a94d3-117">Připojte tooone hello virtuální počítače a test, který může odeslat příkaz ping hello další dva virtuální počítače úspěšně.</span><span class="sxs-lookup"><span data-stu-id="a94d3-117">Connect tooone of hello VMs and test that you can ping hello other two VMs successfully.</span></span>
5. <span data-ttu-id="a94d3-118">Připojit tooone hello virtuální počítače a [Stáhnout balíček Service Fabric hello samostatné pro systém Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) do nové složky na hello počítač a rozbalte balíček hello.</span><span class="sxs-lookup"><span data-stu-id="a94d3-118">Connect tooone of hello VMs and [download hello standalone Service Fabric package for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) into a new folder on hello machine and extract hello package.</span></span>
6. <span data-ttu-id="a94d3-119">Otevřete hello *ClusterConfig.Unsecure.MultiMachine.json* souboru v programu Poznámkový blok a upravit každý uzel s IP adresami hello tři hello počítačů.</span><span class="sxs-lookup"><span data-stu-id="a94d3-119">Open hello *ClusterConfig.Unsecure.MultiMachine.json* file in Notepad and edit each node with hello three IP addresses of hello machines.</span></span> <span data-ttu-id="a94d3-120">Změňte název clusteru hello hello horní a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="a94d3-120">Change hello cluster name at hello top and save hello file.</span></span>  <span data-ttu-id="a94d3-121">Částečné příklad hello manifestu clusteru jsou uvedené dole.</span><span class="sxs-lookup"><span data-stu-id="a94d3-121">A partial example of hello cluster manifest is shown below.</span></span>
   
    ```
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "01-2017",
        "nodes": [
        {
            "nodeName": "standalonenode0",
            "iPAddress": "10.1.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "standalonenode1",
            "iPAddress": "10.1.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "standalonenode2",
            "iPAddress": "10.1.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
        ],
    ```
7. <span data-ttu-id="a94d3-122">Pokud máte v úmyslu tento toobe zabezpečení clusteru, rozhodněte, hello bezpečnostní opatření chcete vytvořit toouse a postupujte podle kroků hello v hello přidružené odkaz: [X509 certifikátu](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="a94d3-122">If you intend this toobe a secure cluster, decide hello security measure you would like toouse and follow hello steps at hello associated link: [X509 Certificate](service-fabric-windows-cluster-x509-security.md) or [Windows Security](service-fabric-windows-cluster-windows-security.md).</span></span> <span data-ttu-id="a94d3-123">Pokud nastavujete hello clusteru pomocí zabezpečení systému Windows, budete potřebovat tooset nahoru toomanage řadič domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a94d3-123">If setting up hello cluster using Windows Security, you will need tooset up a domain controller toomanage Active Directory.</span></span> <span data-ttu-id="a94d3-124">Všimněte si, že používání počítačů řadiče domény jako Service Fabric uzel není podporován.</span><span class="sxs-lookup"><span data-stu-id="a94d3-124">Note that using a domain controller machine as a Service Fabric node is not supported.</span></span>
8. <span data-ttu-id="a94d3-125">Otevřete [okno prostředí PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span><span class="sxs-lookup"><span data-stu-id="a94d3-125">Open a [PowerShell ISE window](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span></span> <span data-ttu-id="a94d3-126">Přejděte toohello složku, kde jste extrahovali hello samostatné stažený instalační balíček a uložili hello clusteru konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="a94d3-126">Navigate toohello folder where you extracted hello downloaded standalone installer package and saved hello cluster configuration file.</span></span> <span data-ttu-id="a94d3-127">Spusťte hello následující clusteru hello toodeploy příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a94d3-127">Run hello following PowerShell command toodeploy hello cluster:</span></span>
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

<span data-ttu-id="a94d3-128">skript Hello se vzdáleně konfigurovat cluster Service Fabric hello a musí hlásit průběh jako prostřednictvím zobrazí souhrn nasazení.</span><span class="sxs-lookup"><span data-stu-id="a94d3-128">hello script will remotely configure hello Service Fabric cluster and should report progress as deployment rolls through.</span></span>

9. <span data-ttu-id="a94d3-129">Po o minuty, můžete zkontrolovat, zda je hello clusteru provozní připojením toohello Service Fabric Explorer pomocí jedné z počítače hello IP adres, například pomocí `http://10.1.0.5:19080/Explorer/index.html`.</span><span class="sxs-lookup"><span data-stu-id="a94d3-129">After about a minute, you can check if hello cluster is operational by connecting toohello Service Fabric Explorer using one of hello machine's IP addresses, for example by using `http://10.1.0.5:19080/Explorer/index.html`.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a94d3-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a94d3-130">Next steps</span></span>
* [<span data-ttu-id="a94d3-131">Vytvoření samostatných clusterů Service Fabric ve Windows Serveru nebo Linuxu</span><span class="sxs-lookup"><span data-stu-id="a94d3-131">Create standalone Service Fabric clusters on Windows Server or Linux</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="a94d3-132">Přidat nebo odebrat cluster Service Fabric samostatné tooa uzly</span><span class="sxs-lookup"><span data-stu-id="a94d3-132">Add or remove nodes tooa standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="a94d3-133">Nastavení konfigurace pro samostatný cluster Windows</span><span class="sxs-lookup"><span data-stu-id="a94d3-133">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="a94d3-134">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí zabezpečení systému Windows</span><span class="sxs-lookup"><span data-stu-id="a94d3-134">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="a94d3-135">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty</span><span class="sxs-lookup"><span data-stu-id="a94d3-135">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

