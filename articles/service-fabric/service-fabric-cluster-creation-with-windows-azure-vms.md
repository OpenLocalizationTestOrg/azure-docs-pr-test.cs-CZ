---
title: "Vytvoření clusteru s podporou samostatné s virtuálními počítači Azure s Windows | Microsoft Docs"
description: "Naučte se vytvářet a spravovat cluster služby Azure Service Fabric na virtuálních počítačích Azure s Windows serverem."
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
ms.openlocfilehash: f8a0305a22c00f9bdbdb1bdb06dc299cccee23dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a><span data-ttu-id="430a6-103">Vytvoření clusteru Service Fabric samostatné tři uzly s virtuálními počítači Azure s Windows serverem</span><span class="sxs-lookup"><span data-stu-id="430a6-103">Create a three node standalone Service Fabric cluster with Azure virtual machines running Windows Server</span></span>
<span data-ttu-id="430a6-104">Tento článek popisuje postup vytvoření clusteru v systému Windows Azure virtuální počítače (VM), pomocí Service Fabric samostatný instalační program systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="430a6-104">This article describes how to create a cluster on Windows-based Azure virtual machines (VMs), using the standalone Service Fabric installer for Windows Server.</span></span> <span data-ttu-id="430a6-105">Tento scénář je zvláštní případ [vytvořit a spravovat cluster se systémem Windows Server](service-fabric-cluster-creation-for-windows-server.md) virtuální počítače, kde jsou [virtuální počítače Azure se systémem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), ale nejsou vytváření [Azure cluster Service Fabric cloudové](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="430a6-105">The scenario is a special case of [Create and manage a cluster running on Windows Server](service-fabric-cluster-creation-for-windows-server.md) where the VMs are [Azure VMs running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), however you are not creating [an Azure cloud-based Service Fabric cluster](service-fabric-cluster-creation-via-portal.md).</span></span> <span data-ttu-id="430a6-106">Rozdíl v následujících tento vzor je, že cluster Service Fabric samostatné vytvořené pomocí následujících kroků je zcela spravován, zatímco Azure Service Fabric clustery založené na cloudu jsou spravovaná a upgradovat poskytovatelem prostředků Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="430a6-106">The distinction in following this pattern is that the standalone Service Fabric cluster created by the following steps is entirely managed by you, whereas the Azure cloud-based Service Fabric clusters are managed and upgraded by the Service Fabric resource provider.</span></span>

## <a name="steps-to-create-the-standalone-cluster"></a><span data-ttu-id="430a6-107">Postup vytvoření clusteru samostatné</span><span class="sxs-lookup"><span data-stu-id="430a6-107">Steps to create the standalone cluster</span></span>
1. <span data-ttu-id="430a6-108">Přihlaste se k portálu Azure a vytvoření nového systému Windows Server 2012 R2 Datacenter nebo virtuálního počítače Windows serveru 2016 Datacenter ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="430a6-108">Sign in to the Azure portal and create a new Windows Server 2012 R2 Datacenter or Windows Server 2016 Datacenter VM in a resource group.</span></span> <span data-ttu-id="430a6-109">Přečtěte si článek [vytvoření virtuálního počítače s Windows v portálu Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="430a6-109">Read the article [Create a Windows VM in the Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for more details.</span></span>
2. <span data-ttu-id="430a6-110">Přidejte několik více virtuálních počítačů do stejné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="430a6-110">Add a couple more VMs to the same resource group.</span></span> <span data-ttu-id="430a6-111">Zajistěte, aby všech virtuálních počítačích stejné uživatelské jméno správce a heslo při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="430a6-111">Ensure that each of the VMs has the same administrator user name and password when created.</span></span> <span data-ttu-id="430a6-112">Po vytvoření, měli byste vidět všechny tři virtuální počítače ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="430a6-112">Once created you should see all three VMs in the same virtual network.</span></span>
3. <span data-ttu-id="430a6-113">Připojit k jednotlivým virtuální počítače a vypnout bránu Windows Firewall pomocí [správce serveru, řídicí panel místní Server](https://technet.microsoft.com/library/jj134147.aspx).</span><span class="sxs-lookup"><span data-stu-id="430a6-113">Connect to each of the VMs and turn off the Windows Firewall using the [Server Manager, Local Server dashboard](https://technet.microsoft.com/library/jj134147.aspx).</span></span> <span data-ttu-id="430a6-114">Tím se zajistí, že síťový provoz můžete komunikaci mezi počítači.</span><span class="sxs-lookup"><span data-stu-id="430a6-114">This ensures that the network traffic can communicate between the machines.</span></span> <span data-ttu-id="430a6-115">Při připojení k každý počítač, získat IP adresu otevřením příkazového řádku a zadáním `ipconfig`.</span><span class="sxs-lookup"><span data-stu-id="430a6-115">While connected to each machine, get the IP address by opening a command prompt and typing `ipconfig`.</span></span> <span data-ttu-id="430a6-116">Případně se zobrazí IP adresu každého počítače, na portálu, výběrem zdroje virtuální sítě pro skupinu prostředků a kontrola rozhraní sítě vytvořené pro každou z těchto počítačů.</span><span class="sxs-lookup"><span data-stu-id="430a6-116">Alternatively you can see the IP address of each machine on the portal, by selecting the virtual network resource for the resource group and checking the network interfaces created for each of these machines.</span></span>
4. <span data-ttu-id="430a6-117">Připojit k virtuální počítače a otestovat, zda dostupný příkazem ping dva virtuální počítače úspěšně.</span><span class="sxs-lookup"><span data-stu-id="430a6-117">Connect to one of the VMs and test that you can ping the other two VMs successfully.</span></span>
5. <span data-ttu-id="430a6-118">Připojení k jednomu z virtuálních počítačů a [Stáhnout balíček Service Fabric samostatné pro systém Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) do nové složky v počítači a extrahování balíčku.</span><span class="sxs-lookup"><span data-stu-id="430a6-118">Connect to one of the VMs and [download the standalone Service Fabric package for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) into a new folder on the machine and extract the package.</span></span>
6. <span data-ttu-id="430a6-119">Otevřete *ClusterConfig.Unsecure.MultiMachine.json* souboru v programu Poznámkový blok a upravit každý uzel s tři IP adresy počítačů.</span><span class="sxs-lookup"><span data-stu-id="430a6-119">Open the *ClusterConfig.Unsecure.MultiMachine.json* file in Notepad and edit each node with the three IP addresses of the machines.</span></span> <span data-ttu-id="430a6-120">Změňte název clusteru v horní části a uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="430a6-120">Change the cluster name at the top and save the file.</span></span>  <span data-ttu-id="430a6-121">Částečné příklad manifestu clusteru jsou uvedené dole.</span><span class="sxs-lookup"><span data-stu-id="430a6-121">A partial example of the cluster manifest is shown below.</span></span>
   
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
7. <span data-ttu-id="430a6-122">Pokud chcete jako cluster s podporou zabezpečení, rozhodněte, bezpečnostní opatření, které chcete použít a postupujte podle kroků v přidružené odkaz: [X509 certifikátu](service-fabric-windows-cluster-x509-security.md) nebo [zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md).</span><span class="sxs-lookup"><span data-stu-id="430a6-122">If you intend this to be a secure cluster, decide the security measure you would like to use and follow the steps at the associated link: [X509 Certificate](service-fabric-windows-cluster-x509-security.md) or [Windows Security](service-fabric-windows-cluster-windows-security.md).</span></span> <span data-ttu-id="430a6-123">Pokud nastavení cluster pomocí zabezpečení systému Windows, musíte nastavit řadič domény ke správě služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="430a6-123">If setting up the cluster using Windows Security, you will need to set up a domain controller to manage Active Directory.</span></span> <span data-ttu-id="430a6-124">Všimněte si, že používání počítačů řadiče domény jako Service Fabric uzel není podporován.</span><span class="sxs-lookup"><span data-stu-id="430a6-124">Note that using a domain controller machine as a Service Fabric node is not supported.</span></span>
8. <span data-ttu-id="430a6-125">Otevřete [okno prostředí PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span><span class="sxs-lookup"><span data-stu-id="430a6-125">Open a [PowerShell ISE window](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).</span></span> <span data-ttu-id="430a6-126">Přejděte do složky, kde jste extrahovali balíček stažené samostatný instalační program a uložili konfiguračního souboru clusteru.</span><span class="sxs-lookup"><span data-stu-id="430a6-126">Navigate to the folder where you extracted the downloaded standalone installer package and saved the cluster configuration file.</span></span> <span data-ttu-id="430a6-127">Spusťte následující příkaz prostředí PowerShell pro nasazení clusteru:</span><span class="sxs-lookup"><span data-stu-id="430a6-127">Run the following PowerShell command to deploy the cluster:</span></span>
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

<span data-ttu-id="430a6-128">Skript se vzdáleně konfigurovat cluster Service Fabric a musí hlásit průběh jako prostřednictvím zobrazí souhrn nasazení.</span><span class="sxs-lookup"><span data-stu-id="430a6-128">The script will remotely configure the Service Fabric cluster and should report progress as deployment rolls through.</span></span>

9. <span data-ttu-id="430a6-129">Po o několik minut, můžete zkontrolovat, pokud je cluster funkční propojíte pomocí jedné z tohoto počítače IP adresy, například pomocí Service Fabric Explorer `http://10.1.0.5:19080/Explorer/index.html`.</span><span class="sxs-lookup"><span data-stu-id="430a6-129">After about a minute, you can check if the cluster is operational by connecting to the Service Fabric Explorer using one of the machine's IP addresses, for example by using `http://10.1.0.5:19080/Explorer/index.html`.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="430a6-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="430a6-130">Next steps</span></span>
* [<span data-ttu-id="430a6-131">Vytvoření samostatných clusterů Service Fabric ve Windows Serveru nebo Linuxu</span><span class="sxs-lookup"><span data-stu-id="430a6-131">Create standalone Service Fabric clusters on Windows Server or Linux</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="430a6-132">Přidávat nebo odebírat uzly do clusteru s podporou samostatné Service Fabric</span><span class="sxs-lookup"><span data-stu-id="430a6-132">Add or remove nodes to a standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="430a6-133">Nastavení konfigurace pro samostatný cluster Windows</span><span class="sxs-lookup"><span data-stu-id="430a6-133">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="430a6-134">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí zabezpečení systému Windows</span><span class="sxs-lookup"><span data-stu-id="430a6-134">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="430a6-135">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty</span><span class="sxs-lookup"><span data-stu-id="430a6-135">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

