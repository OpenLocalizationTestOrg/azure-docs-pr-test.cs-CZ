---
title: "aaaCreate skupin zabezpečení - sítě, prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení skupin zabezpečení sítě pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9cef62b8-d889-4d16-b4d0-58639539a418
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1c8db773febb163d9cb010d23f2913b5ebe0fa94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-powershell"></a><span data-ttu-id="ce228-103">Vytvoření sítě pomocí prostředí PowerShell skupin zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ce228-103">Create network security groups using PowerShell</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

<span data-ttu-id="ce228-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="ce228-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="ce228-105">Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="ce228-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="ce228-106">Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ce228-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span> <span data-ttu-id="ce228-107">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="ce228-107">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="ce228-108">Můžete také [vytvářet skupiny Nsg v modelu nasazení classic hello](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ce228-108">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="ce228-109">Ukázka Hello jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na scénář hello výše.</span><span class="sxs-lookup"><span data-stu-id="ce228-109">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="ce228-110">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, vytvoření nasazením nejprve hello testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů Pokud potřeby a postupujte podle pokynů hello v hello portálu.</span><span class="sxs-lookup"><span data-stu-id="ce228-110">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="ce228-111">Jak toocreate hello skupina NSG pro podsítě front end hello</span><span class="sxs-lookup"><span data-stu-id="ce228-111">How toocreate hello NSG for hello front end subnet</span></span>
<span data-ttu-id="ce228-112">toocreate skupinu NSG s názvem *NSG front-endu* podle hello scénář, dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce228-112">toocreate an NSG named *NSG-FrontEnd* based on hello scenario, complete hello following steps:</span></span>

1. <span data-ttu-id="ce228-113">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, projděte si téma [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů hello všechny toohello hello způsob ukončení toosign do Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="ce228-113">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="ce228-114">Vytvořte pravidlo zabezpečení povolení přístupu z Internetu tooport hello 3389.</span><span class="sxs-lookup"><span data-stu-id="ce228-114">Create a security rule allowing access from hello Internet tooport 3389.</span></span>

    ```powershell
    $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
    ```

3. <span data-ttu-id="ce228-115">Vytvořte pravidlo zabezpečení povolení přístupu z Internetu tooport hello 80.</span><span class="sxs-lookup"><span data-stu-id="ce228-115">Create a security rule allowing access from hello Internet tooport 80.</span></span>

    ```powershell
    $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
    -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80
    ```

4. <span data-ttu-id="ce228-116">Přidat pravidla hello vytvořili výše tooa s názvem nová skupina NSG **NSG front-endu**.</span><span class="sxs-lookup"><span data-stu-id="ce228-116">Add hello rules created above tooa new NSG named **NSG-FrontEnd**.</span></span>

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG -Location westus `
    -Name "NSG-FrontEnd" -SecurityRules $rule1,$rule2
    ```

5. <span data-ttu-id="ce228-117">Zkontrolujte hello pravidel vytvořených v hello NSG zadáním hello následující:</span><span class="sxs-lookup"><span data-stu-id="ce228-117">Check hello rules created in hello NSG by typing hello following:</span></span>

    ```powershell
    $nsg
    ```
   
    <span data-ttu-id="ce228-118">Výstup zobrazuje jenom hello zabezpečení pravidla:</span><span class="sxs-lookup"><span data-stu-id="ce228-118">Output showing just hello security rules:</span></span>
   
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
                                   "Description": "Allow RDP",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "3389",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 100,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "web-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
                                   "Description": "Allow HTTP",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "80",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 101,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
6. <span data-ttu-id="ce228-119">Přidružení hello NSG vytvořili výše toohello *front-endu* podsítě.</span><span class="sxs-lookup"><span data-stu-id="ce228-119">Associate hello NSG created above toohello *FrontEnd* subnet.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="ce228-120">Výstup zobrazuje pouze hello *front-endu* nastavení podsítě, hello hodnota upozornění pro hello **skupinu zabezpečení sítě** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="ce228-120">Output showing only hello *FrontEnd* subnet settings, notice hello value for hello **NetworkSecurityGroup** property:</span></span>
   
                    Subnets           : [
                                          {
                                            "Name": "FrontEnd",
                                            "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                            "AddressPrefix": "192.168.1.0/24",
                                            "IpConfigurations": [
                                              {
                                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                              },
                                              {
                                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                              }
                                            ],
                                            "NetworkSecurityGroup": {
                                              "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                            },
                                            "RouteTable": null,
                                            "ProvisioningState": "Succeeded"
                                          }
   
   > [!WARNING]
   > <span data-ttu-id="ce228-121">výstup Hello výše hello příkazu zobrazuje obsah hello hello virtuální sítě konfigurace objektu, který existuje pouze na hello počítače, kde běží prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ce228-121">hello output for hello command above shows hello content for hello virtual network configuration object, which only exists on hello computer where you are running PowerShell.</span></span> <span data-ttu-id="ce228-122">Je třeba toorun hello `Set-AzureRmVirtualNetwork` rutiny toosave tooAzure těchto nastavení.</span><span class="sxs-lookup"><span data-stu-id="ce228-122">You need toorun hello `Set-AzureRmVirtualNetwork` cmdlet toosave these settings tooAzure.</span></span>
   > 
   > 
7. <span data-ttu-id="ce228-123">Uložte hello tooAzure nastavení nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ce228-123">Save hello new VNet settings tooAzure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="ce228-124">Výstup zobrazuje pouze část NSG hello:</span><span class="sxs-lookup"><span data-stu-id="ce228-124">Output showing just hello NSG portion:</span></span>
   
        "NetworkSecurityGroup": {
          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
        }

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="ce228-125">Jak toocreate hello skupina NSG pro podsíť back-end hello</span><span class="sxs-lookup"><span data-stu-id="ce228-125">How toocreate hello NSG for hello back-end subnet</span></span>
<span data-ttu-id="ce228-126">toocreate skupinu NSG s názvem *NSG back-end* podle hello scénář výše, dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ce228-126">toocreate an NSG named *NSG-BackEnd* based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="ce228-127">Vytvořte pravidlo zabezpečení povolení přístupu z podsítě front-endu tooport hello 1433 (výchozí port používaný systémem SQL Server).</span><span class="sxs-lookup"><span data-stu-id="ce228-127">Create a security rule allowing access from hello front-end subnet tooport 1433 (default port used by SQL Server).</span></span>

    ```powershell
    $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name frontend-rule `
    -Description "Allow FE subnet" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
    -SourceAddressPrefix 192.168.1.0/24 -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 1433
    ```

2. <span data-ttu-id="ce228-128">Vytvořte pravidlo zabezpečení blokování toohello přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="ce228-128">Create a security rule blocking access toohello Internet.</span></span>

    ```powershell
    $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule `
    -Description "Block Internet" `
    -Access Deny -Protocol * -Direction Outbound -Priority 200 `
    -SourceAddressPrefix * -SourcePortRange * `
    -DestinationAddressPrefix Internet -DestinationPortRange *
    ```

3. <span data-ttu-id="ce228-129">Přidat pravidla hello vytvořili výše tooa s názvem nová skupina NSG **NSG back-end**.</span><span class="sxs-lookup"><span data-stu-id="ce228-129">Add hello rules created above tooa new NSG named **NSG-BackEnd**.</span></span>

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG `
    -Location westus -Name "NSG-BackEnd" `
    -SecurityRules $rule1,$rule2
    ```

4. <span data-ttu-id="ce228-130">Přidružení hello NSG vytvořili výše toohello *back-end* podsítě.</span><span class="sxs-lookup"><span data-stu-id="ce228-130">Associate hello NSG created above toohello *BackEnd* subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd ` 
    -AddressPrefix 192.168.2.0/24 -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="ce228-131">Výstup zobrazuje pouze hello *back-end* nastavení podsítě, hello hodnota upozornění pro hello **skupinu zabezpečení sítě** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="ce228-131">Output showing only hello *BackEnd* subnet settings, notice hello value for hello **NetworkSecurityGroup** property:</span></span>
   
        Subnets           : [
                      {
                        "Name": "BackEnd",
                        "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                        "AddressPrefix": "192.168.2.0/24",
                        "IpConfigurations": [...],
                        "NetworkSecurityGroup": {
                          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "RouteTable": null,
                        "ProvisioningState": "Succeeded"
                      }
5. <span data-ttu-id="ce228-132">Uložte hello tooAzure nastavení nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="ce228-132">Save hello new VNet settings tooAzure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

## <a name="how-tooremove-an-nsg"></a><span data-ttu-id="ce228-133">Jak tooremove skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="ce228-133">How tooremove an NSG</span></span>
<span data-ttu-id="ce228-134">toodelete existující skupina NSG, nazývá *NSG front-endu* v takovém případě postupujte podle kroku hello níže:</span><span class="sxs-lookup"><span data-stu-id="ce228-134">toodelete an existing NSG, called *NSG-Frontend* in this case, follow hello step below:</span></span>

<span data-ttu-id="ce228-135">Spustit hello **odebrat AzureRmNetworkSecurityGroup** vidíte níže a že tooinclude hello prostředků skupiny hello je skupina NSG v.</span><span class="sxs-lookup"><span data-stu-id="ce228-135">Run hello **Remove-AzureRmNetworkSecurityGroup** shown below and be sure tooinclude hello resource group hello NSG is in.</span></span>

```powershell
Remove-AzureRmNetworkSecurityGroup -Name "NSG-FrontEnd" -ResourceGroupName "TestRG"
```

