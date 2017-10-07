---
title: aaaNetworking vzory pro Azure Service Fabric | Microsoft Docs
description: "Popisuje běžné sítě vzory pro Service Fabric a jak toocreate cluster pomocí Azure síťových funkcí."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: 5973e3f9917076c6a36e71443ec256e0f414ff87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-networking-patterns"></a><span data-ttu-id="6f0e4-103">Vzory sítě Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6f0e4-103">Service Fabric networking patterns</span></span>
<span data-ttu-id="6f0e4-104">Cluster Azure Service Fabric může integrovat další funkce Azure sítě.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-104">You can integrate your Azure Service Fabric cluster with other Azure networking features.</span></span> <span data-ttu-id="6f0e4-105">V tomto článku jsme ukážeme, jak toocreate clusterů této hello používá následující funkce:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-105">In this article, we show you how toocreate clusters that use hello following features:</span></span>

- [<span data-ttu-id="6f0e4-106">Existující virtuální síť nebo podsíť</span><span class="sxs-lookup"><span data-stu-id="6f0e4-106">Existing virtual network or subnet</span></span>](#existingvnet)
- [<span data-ttu-id="6f0e4-107">Statickou veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="6f0e4-107">Static public IP address</span></span>](#staticpublicip)
- [<span data-ttu-id="6f0e4-108">Pouze interní zátěže.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-108">Internal-only load balancer</span></span>](#internallb)
- [<span data-ttu-id="6f0e4-109">Nástroj pro vyrovnávání zatížení pro vnitřní a vnější</span><span class="sxs-lookup"><span data-stu-id="6f0e4-109">Internal and external load balancer</span></span>](#internalexternallb)

<span data-ttu-id="6f0e4-110">Service Fabric se spustí ve škálovací sadě standardní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-110">Service Fabric runs in a standard virtual machine scale set.</span></span> <span data-ttu-id="6f0e4-111">Žádné funkce, které můžete použít v škálovací sadu virtuálních počítačů, můžete se cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-111">Any functionality that you can use in a virtual machine scale set, you can use with a Service Fabric cluster.</span></span> <span data-ttu-id="6f0e4-112">Hello sítě části hello šablon Azure Resource Manageru pro sady škálování virtuálního počítače a služby prostředků infrastruktury jsou identické.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-112">hello networking sections of hello Azure Resource Manager templates for virtual machine scale sets and Service Fabric are identical.</span></span> <span data-ttu-id="6f0e4-113">Poté, co nasadíte tooan existující virtuální sítě, je snadné tooincorporate dalších síťových funkcí, jako je Azure ExpressRoute, Azure VPN Gateway, skupinu zabezpečení sítě a partnerský vztah virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-113">After you deploy tooan existing virtual network, it's easy tooincorporate other networking features, like Azure ExpressRoute, Azure VPN Gateway, a network security group, and virtual network peering.</span></span>

<span data-ttu-id="6f0e4-114">Service Fabric se liší od dalších síťových funkcí v jeden aspekt.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-114">Service Fabric is unique from other networking features in one aspect.</span></span> <span data-ttu-id="6f0e4-115">Hello [portál Azure](https://portal.azure.com) interně používá hello Service Fabric prostředků zprostředkovatele toocall tooa clusteru tooget informace o uzly a aplikací.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-115">hello [Azure portal](https://portal.azure.com) internally uses hello Service Fabric resource provider toocall tooa cluster tooget information about nodes and applications.</span></span> <span data-ttu-id="6f0e4-116">Poskytovatel prostředků Service Fabric Hello vyžaduje veřejně přístupná příchozí přístup toohello HTTP brány port (port 19080, ve výchozím nastavení) na koncový bod správy hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-116">hello Service Fabric resource provider requires publicly accessible inbound access toohello HTTP gateway port (port 19080, by default) on hello management endpoint.</span></span> <span data-ttu-id="6f0e4-117">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) používá hello toomanage koncový bod správy clusteru.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-117">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) uses hello management endpoint toomanage your cluster.</span></span> <span data-ttu-id="6f0e4-118">Poskytovatel prostředků Service Fabric Hello také používá tento port tooquery informace o clusteru, toodisplay v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-118">hello Service Fabric resource provider also uses this port tooquery information about your cluster, toodisplay in hello Azure portal.</span></span> 

<span data-ttu-id="6f0e4-119">Pokud port 19080 není přístupný ze zprostředkovatele prostředků hello Service Fabric, třeba zprávu *uzly nebyl nalezen* se zobrazí v hello portál, a seznamu uzlů a aplikace, zobrazí se prázdný.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-119">If port 19080 is not accessible from hello Service Fabric resource provider, a message like *Nodes Not Found* appears in hello portal, and your node and application list appears empty.</span></span> <span data-ttu-id="6f0e4-120">Pokud chcete toosee cluster v hello portálu Azure, nástroj pro vyrovnávání zatížení musí vystavit veřejnou IP adresu a vaše skupina zabezpečení sítě musí umožňovat příchozí port 19080 provoz.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-120">If you want toosee your cluster in hello Azure portal, your load balancer must expose a public IP address, and your network security group must allow incoming port 19080 traffic.</span></span> <span data-ttu-id="6f0e4-121">Pokud vaše instalace těchto požadavků nesplňuje, nezobrazuje hello portál Azure hello stav clusteru.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-121">If your setup does not meet these requirements, hello Azure portal does not display hello status of your cluster.</span></span>

## <a name="templates"></a><span data-ttu-id="6f0e4-122">Šablony</span><span class="sxs-lookup"><span data-stu-id="6f0e4-122">Templates</span></span>

<span data-ttu-id="6f0e4-123">Všechny šablony Service Fabric se v [jeden soubor ke stažení souboru](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip).</span><span class="sxs-lookup"><span data-stu-id="6f0e4-123">All Service Fabric templates are in [one download file](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip).</span></span> <span data-ttu-id="6f0e4-124">Musí být schopný toodeploy hello šablony jako-pomocí hello následující příkazy prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-124">You should be able toodeploy hello templates as-is by using hello following PowerShell commands.</span></span> <span data-ttu-id="6f0e4-125">Pokud nasazujete hello existující virtuální síť Azure nebo hello statické veřejné IP šablona, nejdřív přečíst hello [počáteční instalace](#initialsetup) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-125">If you are deploying hello existing Azure Virtual Network template or hello static public IP template, first read hello [Initial setup](#initialsetup) section of this article.</span></span>

<a id="initialsetup"></a>
## <a name="initial-setup"></a><span data-ttu-id="6f0e4-126">Počáteční nastavení</span><span class="sxs-lookup"><span data-stu-id="6f0e4-126">Initial setup</span></span>

### <a name="existing-virtual-network"></a><span data-ttu-id="6f0e4-127">Existující virtuální síť</span><span class="sxs-lookup"><span data-stu-id="6f0e4-127">Existing virtual network</span></span>

<span data-ttu-id="6f0e4-128">V následujícím příkladu hello, můžeme začít s existující virtuální síť s názvem ExistingRG-vnet v hello **ExistingRG** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-128">In hello following example, we start with an existing virtual network named ExistingRG-vnet, in hello **ExistingRG** resource group.</span></span> <span data-ttu-id="6f0e4-129">podsíť Hello se nazývá výchozí.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-129">hello subnet is named default.</span></span> <span data-ttu-id="6f0e4-130">Tyto výchozí prostředky jsou vytvořeny při použití hello Azure portálu toocreate standardní virtuální počítač (VM).</span><span class="sxs-lookup"><span data-stu-id="6f0e4-130">These default resources are created when you use hello Azure portal toocreate a standard virtual machine (VM).</span></span> <span data-ttu-id="6f0e4-131">Můžete vytvořit hello virtuální síť a podsíť bez vytvoření hello virtuálních počítačů, ale přidání clusteru tooan existující virtuální síť hello hlavním cílem je tooprovide síťové připojení tooother virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-131">You could create hello virtual network and subnet without creating hello VM, but hello main goal of adding a cluster tooan existing virtual network is tooprovide network connectivity tooother VMs.</span></span> <span data-ttu-id="6f0e4-132">Vytváření hello virtuálních počítačů poskytuje dobrým příkladem jak se obvykle používá existující virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-132">Creating hello VM gives a good example of how an existing virtual network typically is used.</span></span> <span data-ttu-id="6f0e4-133">Pokud váš cluster Service Fabric používá jenom k interním pro vyrovnávání zatížení, bez veřejnou IP adresu, můžete použít hello virtuální počítač a jeho veřejná IP adresa jako zabezpečený *přejít pole*.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-133">If your Service Fabric cluster uses only an internal load balancer, without a public IP address, you can use hello VM and its public IP as a secure *jump box*.</span></span>

### <a name="static-public-ip-address"></a><span data-ttu-id="6f0e4-134">Statickou veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="6f0e4-134">Static public IP address</span></span>

<span data-ttu-id="6f0e4-135">Statickou veřejnou IP adresu obecně je vyhrazený prostředek, který je spravován od něj odděleně hello virtuálního počítače nebo virtuální počítače je přiřazena k.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-135">A static public IP address generally is a dedicated resource that's managed separately from hello VM or VMs it's assigned to.</span></span> <span data-ttu-id="6f0e4-136">Ve skupině prostředků vyhrazené sítě je zřízený (jako názvem na rozdíl od tooin hello Service Fabric skupinu prostředků clusteru sám sebe).</span><span class="sxs-lookup"><span data-stu-id="6f0e4-136">It's provisioned in a dedicated networking resource group (as opposed tooin hello Service Fabric cluster resource group itself).</span></span> <span data-ttu-id="6f0e4-137">Vytvořit statickou veřejnou IP adresu s názvem staticIP1 v hello stejnou skupinu prostředků ExistingRG v hello portál Azure nebo pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-137">Create a static public IP address named staticIP1 in hello same ExistingRG resource group, either in hello Azure portal or by using PowerShell:</span></span>

```powershell
PS C:\Users\user> New-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

Name                     : staticIP1
ResourceGroupName        : ExistingRG
Location                 : westus
Id                       : /subscriptions/1237f4d2-3dce-1236-ad95-123f764e7123/resourceGroups/ExistingRG/providers/Microsoft.Network/publicIPAddresses/staticIP1
Etag                     : W/"fc8b0c77-1f84-455d-9930-0404ebba1b64"
ResourceGuid             : 77c26c06-c0ae-496c-9231-b1a114e08824
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Static
IpAddress                : 40.83.182.110
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : null
DnsSettings              : {
                             "DomainNameLabel": "sfnetworking",
                             "Fqdn": "sfnetworking.westus.cloudapp.azure.com"
                           }
```

### <a name="service-fabric-template"></a><span data-ttu-id="6f0e4-138">Šablona Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6f0e4-138">Service Fabric template</span></span>

<span data-ttu-id="6f0e4-139">V příkladech hello v tomto článku používáme template.json Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-139">In hello examples in this article, we use hello Service Fabric template.json.</span></span> <span data-ttu-id="6f0e4-140">Hello standardní portálu Průvodce toodownload hello šablonu z portálu hello můžete použít, před vytvořením clusteru.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-140">You can use hello standard portal wizard toodownload hello template from hello portal before you create a cluster.</span></span> <span data-ttu-id="6f0e4-141">Také můžete použít jednu z šablon hello v hello [Galerie šablon](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), jako je hello [pěti uzly clusteru Service Fabric](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).</span><span class="sxs-lookup"><span data-stu-id="6f0e4-141">You also can use one of hello templates in hello [template gallery](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), like hello [five-node Service Fabric cluster](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).</span></span>

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a><span data-ttu-id="6f0e4-142">Existující virtuální síť nebo podsíť</span><span class="sxs-lookup"><span data-stu-id="6f0e4-142">Existing virtual network or subnet</span></span>

1. <span data-ttu-id="6f0e4-143">Změňte hello podsíť parametr toohello název hello existující podsíť a pak přidejte dva nové parametry tooreference hello existující virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-143">Change hello subnet parameter toohello name of hello existing subnet, and then add two new parameters tooreference hello existing virtual network:</span></span>

    ```
        "subnet0Name": {
                "type": "string",
                "defaultValue": "default"
            },
            "existingVNetRGName": {
                "type": "string",
                "defaultValue": "ExistingRG"
            },

            "existingVNetName": {
                "type": "string",
                "defaultValue": "ExistingRG-vnet"
            },
            /*
            "subnet0Name": {
                "type": "string",
                "defaultValue": "Subnet-0"
            },
            "subnet0Prefix": {
                "type": "string",
                "defaultValue": "10.0.0.0/24"
            },*/
    ```


2. <span data-ttu-id="6f0e4-144">Změna hello `vnetID` proměnné toopoint toohello existující virtuální síť:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-144">Change hello `vnetID` variable toopoint toohello existing virtual network:</span></span>

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. <span data-ttu-id="6f0e4-145">Odebrat `Microsoft.Network/virtualNetworks` z vašich prostředků, takže Azure nevytvoří nové virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-145">Remove `Microsoft.Network/virtualNetworks` from your resources, so Azure does not create a new virtual network:</span></span>

    ```
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properities": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
            {
                "name": "[parameters('subnet0Name')]",
                "properties": {
                    "addressPrefix": "[parameters('subnet0Prefix')]"
                }
            }
        ]
    },
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    }
    },*/
    ```

4. <span data-ttu-id="6f0e4-146">Komentář hello virtuální sítě z hello `dependsOn` atribut `Microsoft.Compute/virtualMachineScaleSets`, takže nemusíte závisí na vytvoření nové virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-146">Comment out hello virtual network from hello `dependsOn` attribute of `Microsoft.Compute/virtualMachineScaleSets`, so you don't depend on creating a new virtual network:</span></span>

    ```
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

5. <span data-ttu-id="6f0e4-147">Nasazení šablony hello:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-147">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    <span data-ttu-id="6f0e4-148">Po nasazení by měla obsahovat vaše virtuální síť hello nové škálovací sady virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-148">After deployment, your virtual network should include hello new scale set VMs.</span></span> <span data-ttu-id="6f0e4-149">Typ uzlu sady škálování virtuálního počítače Hello by měl zobrazit hello existující virtuální síť a podsíť.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-149">hello virtual machine scale set node type should show hello existing virtual network and subnet.</span></span> <span data-ttu-id="6f0e4-150">Můžete taky použít protokol RDP (Remote Desktop) tooaccess hello virtuální počítač, který již byl ve virtuální síti hello a tooping hello nové škálovací sady virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-150">You also can use Remote Desktop Protocol (RDP) tooaccess hello VM that was already in hello virtual network, and tooping hello new scale set VMs:</span></span>

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

<span data-ttu-id="6f0e4-151">Jiný příklad najdete v tématu [ten, který není konkrétní tooService Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="6f0e4-151">For another example, see [one that is not specific tooService Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).</span></span>


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a><span data-ttu-id="6f0e4-152">Statickou veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="6f0e4-152">Static public IP address</span></span>

1. <span data-ttu-id="6f0e4-153">Přidání parametrů pro název hello hello existující statická skupina prostředků IP, název a plně kvalifikovaný název domény (FQDN):</span><span class="sxs-lookup"><span data-stu-id="6f0e4-153">Add parameters for hello name of hello existing static IP resource group, name, and fully qualified domain name (FQDN):</span></span>

    ```
    "existingStaticIPResourceGroup": {
                "type": "string"
            },
            "existingStaticIPName": {
                "type": "string"
            },
            "existingStaticIPDnsFQDN": {
                "type": "string"
    }
    ```

2. <span data-ttu-id="6f0e4-154">Odebrat hello `dnsName` parametr.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-154">Remove hello `dnsName` parameter.</span></span> <span data-ttu-id="6f0e4-155">(hello statickou IP adresu již jedna.)</span><span class="sxs-lookup"><span data-stu-id="6f0e4-155">(hello static IP address already has one.)</span></span>

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. <span data-ttu-id="6f0e4-156">Přidejte proměnnou tooreference hello existující statickou IP adresu:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-156">Add a variable tooreference hello existing static IP address:</span></span>

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. <span data-ttu-id="6f0e4-157">Odebrat `Microsoft.Network/publicIPAddresses` z vašich prostředků, takže Azure nevytvoří novou IP adresu:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-157">Remove `Microsoft.Network/publicIPAddresses` from your resources, so Azure does not create a new IP address:</span></span>

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

5. <span data-ttu-id="6f0e4-158">Komentář hello IP adresu z hello `dependsOn` atribut `Microsoft.Network/loadBalancers`, takže nemusíte závisí na vytvoření novou IP adresu:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-158">Comment out hello IP address from hello `dependsOn` attribute of `Microsoft.Network/loadBalancers`, so you don't depend on creating a new IP address:</span></span>

    ```
    "apiVersion": "[variables('lbIPApiVersion')]",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB', '-', parameters('clusterName'), '-', parameters('vmNodeType0Name'))]",
    "location": "[parameters('computeLocation')]",
    /*
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', concat(parameters('lbIPName'), '-', '0'))]"
    ], */
    "properties": {
    ```

6. <span data-ttu-id="6f0e4-159">V hello `Microsoft.Network/loadBalancers` prostředků, změna hello `publicIPAddress` element `frontendIPConfigurations` tooreference hello existující statické IP adresy místo nově vytvořený jeden:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-159">In hello `Microsoft.Network/loadBalancers` resource, change hello `publicIPAddress` element of `frontendIPConfigurations` tooreference hello existing static IP address instead of a newly created one:</span></span>

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                "publicIPAddress": {
                                    /*"id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"*/
                                    "id": "[variables('existingStaticIP')]"
                                }
                            }
                        }
                    ],
    ```

7. <span data-ttu-id="6f0e4-160">V hello `Microsoft.ServiceFabric/clusters` prostředků, změna `managementEndpoint` toohello plně kvalifikovaný název domény DNS hello statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-160">In hello `Microsoft.ServiceFabric/clusters` resource, change `managementEndpoint` toohello DNS FQDN of hello static IP address.</span></span> <span data-ttu-id="6f0e4-161">Pokud používáte zabezpečení clusteru, ujistěte se, změníte *http://* příliš*https://*.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-161">If you are using a secure cluster, make sure you change *http://* too*https://*.</span></span> <span data-ttu-id="6f0e4-162">(Všimněte si, že tento krok platí jenom tooService clustery infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-162">(Note that this step applies only tooService Fabric clusters.</span></span> <span data-ttu-id="6f0e4-163">Pokud používáte škálovací sadu virtuálních počítačů, tento krok přeskočte.)</span><span class="sxs-lookup"><span data-stu-id="6f0e4-163">If you are using a virtual machine scale set, skip this step.)</span></span>

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. <span data-ttu-id="6f0e4-164">Nasazení šablony hello:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-164">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

<span data-ttu-id="6f0e4-165">Po nasazení můžete zjistit, že je nástroj pro vyrovnávání zatížení vázané toohello veřejnou statickou IP adresu z hello jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-165">After deployment, you can see that your load balancer is bound toohello public static IP address from hello other resource group.</span></span> <span data-ttu-id="6f0e4-166">Hello koncového bodu připojení klienta Service Fabric a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) koncový bod bod toohello plně kvalifikovaný název domény DNS hello statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-166">hello Service Fabric client connection endpoint and [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint point toohello DNS FQDN of hello static IP address.</span></span>

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a><span data-ttu-id="6f0e4-167">Pouze interní zátěže.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-167">Internal-only load balancer</span></span>

<span data-ttu-id="6f0e4-168">Tento scénář nahrazuje hello externím vyrovnáváním zatížení v šabloně Service Fabric výchozí hello s nástrojem pro vyrovnávání zatížení pouze interní.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-168">This scenario replaces hello external load balancer in hello default Service Fabric template with an internal-only load balancer.</span></span> <span data-ttu-id="6f0e4-169">Důsledky pro hello portál Azure a pro poskytovatele prostředků hello Service Fabric najdete v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-169">For implications for hello Azure portal and for hello Service Fabric resource provider, see hello preceding section.</span></span>

1. <span data-ttu-id="6f0e4-170">Odebrat hello `dnsName` parametr.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-170">Remove hello `dnsName` parameter.</span></span> <span data-ttu-id="6f0e4-171">(Jej není nutné.)</span><span class="sxs-lookup"><span data-stu-id="6f0e4-171">(It's not needed.)</span></span>

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. <span data-ttu-id="6f0e4-172">Pokud používáte metodu statického přidělování, Volitelně můžete přidat parametr statických adres IP.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-172">Optionally, if you use a static allocation method, you can add a static IP address parameter.</span></span> <span data-ttu-id="6f0e4-173">Pokud používáte metody dynamického přidělení, není nutné toodo tento krok.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-173">If you use a dynamic allocation method, you do not need toodo this step.</span></span>

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. <span data-ttu-id="6f0e4-174">Odebrat `Microsoft.Network/publicIPAddresses` z vašich prostředků, takže Azure nevytvoří novou IP adresu:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-174">Remove `Microsoft.Network/publicIPAddresses` from your resources, so Azure does not create a new IP address:</span></span>

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

4. <span data-ttu-id="6f0e4-175">Odeberte hello IP adresu `dependsOn` atribut `Microsoft.Network/loadBalancers`, takže nemusíte závisí na vytvoření nové adresy IP.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-175">Remove hello IP address `dependsOn` attribute of `Microsoft.Network/loadBalancers`, so you don't depend on creating a new IP address.</span></span> <span data-ttu-id="6f0e4-176">Přidat virtuální síť hello `dependsOn` atributů, protože nástroj pro vyrovnávání zatížení hello teď závisí na podsíť hello z virtuální sítě hello:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-176">Add hello virtual network `dependsOn` attribute because hello load balancer now depends on hello subnet from hello virtual network:</span></span>

    ```
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. <span data-ttu-id="6f0e4-177">Změnit Vyrovnávání zatížení hello `frontendIPConfigurations` ze pomocí nastavení `publicIPAddress`, toousing podsíť a `privateIPAddress`.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-177">Change hello load balancer's `frontendIPConfigurations` setting from using a `publicIPAddress`, toousing a subnet and `privateIPAddress`.</span></span> <span data-ttu-id="6f0e4-178">`privateIPAddress`používá předdefinovanou statické interní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-178">`privateIPAddress` uses a predefined static internal IP address.</span></span> <span data-ttu-id="6f0e4-179">toouse dynamickou IP adresu, odeberte hello `privateIPAddress` element a poté změňte `privateIPAllocationMethod` příliš**dynamické**.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-179">toouse a dynamic IP address, remove hello `privateIPAddress` element, and then change `privateIPAllocationMethod` too**Dynamic**.</span></span>

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /*
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                } */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
    ```

6. <span data-ttu-id="6f0e4-180">V hello `Microsoft.ServiceFabric/clusters` prostředků, změna `managementEndpoint` adresa služby Vyrovnávání zatížení pro vnitřní toohello toopoint.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-180">In hello `Microsoft.ServiceFabric/clusters` resource, change `managementEndpoint` toopoint toohello internal load balancer address.</span></span> <span data-ttu-id="6f0e4-181">Pokud používáte zabezpečení clusteru, ujistěte se, změníte *http://* příliš*https://*.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-181">If you use a secure cluster, make sure you change *http://* too*https://*.</span></span> <span data-ttu-id="6f0e4-182">(Všimněte si, že tento krok platí jenom tooService clustery infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-182">(Note that this step applies only tooService Fabric clusters.</span></span> <span data-ttu-id="6f0e4-183">Pokud používáte škálovací sadu virtuálních počítačů, tento krok přeskočte.)</span><span class="sxs-lookup"><span data-stu-id="6f0e4-183">If you are using a virtual machine scale set, skip this step.)</span></span>

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. <span data-ttu-id="6f0e4-184">Nasazení šablony hello:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-184">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

<span data-ttu-id="6f0e4-185">Po nasazení používá nástroj pro vyrovnávání zatížení hello 10.0.0.250 privátní statickou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-185">After deployment, your load balancer uses hello private static 10.0.0.250 IP address.</span></span> <span data-ttu-id="6f0e4-186">Pokud máte jiný počítač ve stejné virtuální síti, můžete přejít toohello interní [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-186">If you have another machine in that same virtual network, you can go toohello internal [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint.</span></span> <span data-ttu-id="6f0e4-187">Všimněte si, že se připojí tooone uzlů hello za nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-187">Note that it connects tooone of hello nodes behind hello load balancer.</span></span>

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a><span data-ttu-id="6f0e4-188">Nástroj pro vyrovnávání zatížení pro vnitřní a vnější</span><span class="sxs-lookup"><span data-stu-id="6f0e4-188">Internal and external load balancer</span></span>

<span data-ttu-id="6f0e4-189">V tomto scénáři začínat hello existující typ s jedním uzlem externí nástroj pro vyrovnávání zatížení a přidejte interní nástroj pro hello stejný typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-189">In this scenario, you start with hello existing single-node type external load balancer, and add an internal load balancer for hello same node type.</span></span> <span data-ttu-id="6f0e4-190">Fond back-end adresy tooa back-end port připojený lze přiřadit pouze tooa jeden nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-190">A back-end port attached tooa back-end address pool can be assigned only tooa single load balancer.</span></span> <span data-ttu-id="6f0e4-191">Zvolte, které nástroj pro vyrovnávání zatížení bude mít vaše aplikace porty a který nástroj pro vyrovnávání zatížení bude mít vaše koncové body správy (porty 19000 a 19080).</span><span class="sxs-lookup"><span data-stu-id="6f0e4-191">Choose which load balancer will have your application ports, and which load balancer will have your management endpoints (ports 19000 and 19080).</span></span> <span data-ttu-id="6f0e4-192">Když vložíte koncových bodů pro správu hello na Vyrovnávání zatížení interní hello, mějte na paměti hello Service Fabric prostředků zprostředkovatele omezení popsané dříve v článku hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-192">If you put hello management endpoints on hello internal load balancer, keep in mind hello Service Fabric resource provider restrictions discussed earlier in hello article.</span></span> <span data-ttu-id="6f0e4-193">V příkladu hello, které používáme koncových bodů pro správu hello zůstanou hello externím vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-193">In hello example we use, hello management endpoints remain on hello external load balancer.</span></span> <span data-ttu-id="6f0e4-194">Také přidat na port 80 aplikace portu a umístěte ji na Vyrovnávání zatížení interní hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-194">You also add a port 80 application port, and place it on hello internal load balancer.</span></span>

<span data-ttu-id="6f0e4-195">V typu dva uzlu clusteru je jeden typ uzlu na hello externím vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-195">In a two-node-type cluster, one node type is on hello external load balancer.</span></span> <span data-ttu-id="6f0e4-196">Hello jiný typ uzlu je na Vyrovnávání zatížení interní hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-196">hello other node type is on hello internal load balancer.</span></span> <span data-ttu-id="6f0e4-197">toouse dva typ uzlu clusteru, v hello portál vytvořit typ uzlu dvě šablony (který se dodává s dva nástroje pro vyrovnávání zatížení), přepínače hello druhý vyrovnávání tooan interní služby load Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-197">toouse a two-node-type cluster, in hello portal-created two-node-type template (which comes with two load balancers), switch hello second load balancer tooan internal load balancer.</span></span> <span data-ttu-id="6f0e4-198">Další informace najdete v tématu hello [nástroj pro vyrovnávání zatížení pouze pro interní](#internallb) části.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-198">For more information, see hello [Internal-only load balancer](#internallb) section.</span></span>

1. <span data-ttu-id="6f0e4-199">Přidejte hello statické interní služby load vyrovnávání IP adresu parametr.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-199">Add hello static internal load balancer IP address parameter.</span></span> <span data-ttu-id="6f0e4-200">(Poznámky související toousing dynamickou IP adresu, naleznete v předchozích částech tohoto článku.)</span><span class="sxs-lookup"><span data-stu-id="6f0e4-200">(For notes related toousing a dynamic IP address, see earlier sections of this article.)</span></span>

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. <span data-ttu-id="6f0e4-201">Přidáte parametr aplikace port 80.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-201">Add an application port 80 parameter.</span></span>

3. <span data-ttu-id="6f0e4-202">Interní verze tooadd hello existující sítě proměnné, zkopírovat a vložit a přidejte "-Int" toohello název:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-202">tooadd internal versions of hello existing networking variables, copy and paste them, and add "-Int" toohello name:</span></span>

    ```
    /* Add internal load balancer networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* Internal load balancer networking variables end */
    ```

4. <span data-ttu-id="6f0e4-203">Pokud spustíte pomocí generované portál šablony hello, který používá aplikace port 80, hello výchozí šablony portálu přidá AppPort1 (port 80) na hello externím vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-203">If you start with hello portal-generated template that uses application port 80, hello default portal template adds AppPort1 (port 80) on hello external load balancer.</span></span> <span data-ttu-id="6f0e4-204">V takovém případě odebrání hello externím vyrovnáváním zatížení AppPort1 `loadBalancingRules` a sondy, takže ho můžete přidat toohello interní vyrovnávání zátěže:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-204">In this case, remove AppPort1 from hello external load balancer `loadBalancingRules` and probes, so you can add it toohello internal load balancer:</span></span>

    ```
    "loadBalancingRules": [
        {
            "name": "LBHttpRule",
            "properties":{
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[variables('lbHttpProbeID0')]"
                },
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from hello external load balancer.
        {
            "name": "AppPortLBRule1",
            "properties": {
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('loadBalancedAppPort1')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[concate(variables('lbID0'), '/probes/AppPortProbe1')]"
                },
                "protocol": "tcp"
            }
        }*/

    ],
    "probes": [
        {
            "name": "FabricGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricTcpGatewayPort')]",
                "protocol": "tcp"
            }
        },
        {
            "name": "FabricHttpGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricHttpGatewayPort')]",
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from hello external load balancer.
        {
            "name": "AppPortProbe1",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('loadBalancedAppPort1')]",
                "protocol": "tcp"
            }
        } */

    ],
    "inboundNatPools": [
    ```

5. <span data-ttu-id="6f0e4-205">Přidejte druhý `Microsoft.Network/loadBalancers` prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-205">Add a second `Microsoft.Network/loadBalancers` resource.</span></span> <span data-ttu-id="6f0e4-206">Vypadá to, podobně jako toohello interní nástroj pro vyrovnávání zatížení vytvořené v hello [nástroj pro vyrovnávání zatížení pouze pro interní](#internallb) části, ale používá hello "-Int" zatížení vyrovnávání proměnné a implementuje jenom hello aplikace port 80.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-206">It looks similar toohello internal load balancer created in hello [Internal-only load balancer](#internallb) section, but it uses hello "-Int" load balancer variables, and implements only hello application port 80.</span></span> <span data-ttu-id="6f0e4-207">Tím se taky odeberou `inboundNatPools`, tookeep koncových bodů protokolu RDP na Vyrovnávání zatížení veřejnou hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-207">This also removes `inboundNatPools`, tookeep RDP endpoints on hello public load balancer.</span></span> <span data-ttu-id="6f0e4-208">Pokud chcete na Vyrovnávání zatížení interní hello RDP, přesuňte `inboundNatPools` z toothis nástroje pro vyrovnávání zatížení externí hello interní nástroj pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-208">If you want RDP on hello internal load balancer, move `inboundNatPools` from hello external load balancer toothis internal load balancer:</span></span>

    ```
            /* Add a second load balancer, configured with a static privateIPAddress and hello "-Int" load balancer variables. */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add "-Internal" toohello name. */
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal')]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /* Remove public IP dependsOn, add vnet dependsOn
                    "[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"
                    */
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /* Switch from Public tooPrivate IP address
                                */
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                }
                                */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
                    "backendAddressPools": [
                        {
                            "name": "LoadBalancerBEAddressPool",
                            "properties": {}
                        }
                    ],
                    "loadBalancingRules": [
                        /* Add hello AppPort rule. Be sure tooreference hello "-Int" versions of backendAddressPool, frontendIPConfiguration, and hello probe variables. */
                        {
                            "name": "AppPortLBRule1",
                            "properties": {
                                "backendAddressPool": {
                                    "id": "[variables('lbPoolID0-Int')]"
                                },
                                "backendPort": "[parameters('loadBalancedAppPort1')]",
                                "enableFloatingIP": "false",
                                "frontendIPConfiguration": {
                                    "id": "[variables('lbIPConfig0-Int')]"
                                },
                                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                                "idleTimeoutInMinutes": "5",
                                "probe": {
                                    "id": "[concat(variables('lbID0-Int'),'/probes/AppPortProbe1')]"
                                },
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "probes": [
                    /* Add hello probe for hello app port. */
                    {
                            "name": "AppPortProbe1",
                            "properties": {
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2,
                                "port": "[parameters('loadBalancedAppPort1')]",
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "inboundNatPools": [
                    ]
                },
                "tags": {
                    "resourceType": "Service Fabric",
                    "clusterName": "[parameters('clusterName')]"
                }
            },
    ```

6. <span data-ttu-id="6f0e4-209">V `networkProfile` pro hello `Microsoft.Compute/virtualMachineScaleSets` prostředků, přidat fond adres interní back-end hello:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-209">In `networkProfile` for hello `Microsoft.Compute/virtualMachineScaleSets` resource, add hello internal back-end address pool:</span></span>

    ```
    "loadBalancerBackendAddressPools": [
                                                        {
                                                            "id": "[variables('lbPoolID0')]"
                                                        },
                                                        {
                                                            /* Add internal BE pool */
                                                            "id": "[variables('lbPoolID0-Int')]"
                                                        }
    ],
    ```

7. <span data-ttu-id="6f0e4-210">Nasazení šablony hello:</span><span class="sxs-lookup"><span data-stu-id="6f0e4-210">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

<span data-ttu-id="6f0e4-211">Po nasazení se zobrazí dvě služby Vyrovnávání zatížení ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-211">After deployment, you can see two load balancers in hello resource group.</span></span> <span data-ttu-id="6f0e4-212">Pokud hello nástroje pro vyrovnávání zatížení, uvidíte hello veřejnou IP adresu a správa koncových bodů (porty 19000 a 19080) přiřazen toohello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-212">If you browse hello load balancers, you can see hello public IP address and management endpoints (ports 19000 and 19080) assigned toohello public IP address.</span></span> <span data-ttu-id="6f0e4-213">Taky uvidíte hello statické interní IP adresu a aplikace koncového bodu (port 80) přiřazen toohello interní nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-213">You also can see hello static internal IP address and application endpoint (port 80) assigned toohello internal load balancer.</span></span> <span data-ttu-id="6f0e4-214">Současně pomocí nástroje pro vyrovnávání zátěže hello stejné škálovací sady virtuálních počítačů fond back-end.</span><span class="sxs-lookup"><span data-stu-id="6f0e4-214">Both load balancers use hello same virtual machine scale set back-end pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f0e4-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f0e4-215">Next steps</span></span>
[<span data-ttu-id="6f0e4-216">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="6f0e4-216">Create a cluster</span></span>](service-fabric-cluster-creation-via-arm.md)
