---
title: "Analyzovat zabezpečení sítě s Azure sítě sledovacích procesů zabezpečení skupiny zobrazení – REST API | Microsoft Docs"
description: "Tento článek popisuje, jak pomocí prostředí PowerShell k analýze zabezpečení virtuálních počítačů s zobrazení skupiny zabezpečení."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a2f418fe-f5d2-43ed-8dc3-df0ed2a4d4ac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: afced52b3ae6f3b7f400364f5ec7d049aa166590
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-rest-api"></a><span data-ttu-id="ea3aa-103">Analýza zabezpečení vašeho virtuálního počítače s zobrazení skupiny zabezpečení pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="ea3aa-103">Analyze your Virtual Machine security with Security Group View using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="ea3aa-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ea3aa-104">PowerShell</span></span>](network-watcher-security-group-view-powershell.md)
> - [<span data-ttu-id="ea3aa-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ea3aa-105">CLI 1.0</span></span>](network-watcher-security-group-view-cli-nodejs.md)
> - [<span data-ttu-id="ea3aa-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ea3aa-106">CLI 2.0</span></span>](network-watcher-security-group-view-cli.md)
> - [<span data-ttu-id="ea3aa-107">REST API</span><span class="sxs-lookup"><span data-stu-id="ea3aa-107">REST API</span></span>](network-watcher-security-group-view-rest.md)

<span data-ttu-id="ea3aa-108">Zobrazení skupiny zabezpečení vrátí pravidla zabezpečení sítě nakonfigurované a efektivní, která se použijí k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-108">Security group view returns configured and effective network security rules that are applied to a virtual machine.</span></span> <span data-ttu-id="ea3aa-109">Tato možnost je užitečná k auditování a diagnostice skupin zabezpečení sítě a pravidel, které jsou nakonfigurované na virtuálním počítači zajistit provoz se správně povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-109">This capability is useful to audit and diagnose Network Security Groups and rules that are configured on a VM to ensure traffic is being correctly allowed or denied.</span></span> <span data-ttu-id="ea3aa-110">V tomto článku jsme ukazují, jak načíst pravidla zabezpečení efektivní a použité k virtuálnímu počítači pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="ea3aa-110">In this article, we show you how to retrieve the effective and applied security rules to a virtual machine using REST API</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ea3aa-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ea3aa-111">Before you begin</span></span>

<span data-ttu-id="ea3aa-112">V tomto scénáři můžete volat rozhraní API Rest sledovací proces sítě získat zobrazení skupiny zabezpečení pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-112">In this scenario, you call the Network Watcher Rest API to get the security group view for a virtual machine.</span></span> <span data-ttu-id="ea3aa-113">ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-113">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="ea3aa-114">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="ea3aa-114">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="ea3aa-115">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="ea3aa-116">Tento scénář také předpokládá, že skupina prostředků se platný virtuální počítač existuje má být použit.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-116">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="ea3aa-117">Scénář</span><span class="sxs-lookup"><span data-stu-id="ea3aa-117">Scenario</span></span>

<span data-ttu-id="ea3aa-118">Scénář popsaná v tomto článku načte pravidla zabezpečení efektivní a použité pro daný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-118">The scenario covered in this article retrieves the effective and applied security rules for a given virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="ea3aa-119">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="ea3aa-119">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="ea3aa-120">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="ea3aa-120">Retrieve a virtual machine</span></span>

<span data-ttu-id="ea3aa-121">Následující kód, spusťte následující skript k vrácení virtuální machineThe potřebuje proměnné:</span><span class="sxs-lookup"><span data-stu-id="ea3aa-121">Run the following script to return a virtual machineThe following code needs variables:</span></span>

- <span data-ttu-id="ea3aa-122">**ID předplatného** -pomocí můžete také načíst id předplatného **Get-AzureRMSubscription** rutiny.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-122">**subscriptionId** - The subscription id can also be retrieved with the **Get-AzureRMSubscription** cmdlet.</span></span>
- <span data-ttu-id="ea3aa-123">**Název skupiny prostředků** -název skupiny prostředků, která obsahuje virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-123">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="ea3aa-124">Informace, které je potřeba **id** v části typ `Microsoft.Compute/virtualMachines` v odpovědi, jak je vidět v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ea3aa-124">The information that is needed is the **id** under the type `Microsoft.Compute/virtualMachines` in response, as seen in the following example:</span></span>

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft
.Network/networkInterfaces/{nicName}"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Com
pute/virtualMachines/{vmName}/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute
/virtualMachines/{vmName}",
      "name": "{vmName}"
    }
  ]
}
```

## <a name="get-security-group-view-for-virtual-machine"></a><span data-ttu-id="ea3aa-125">Získat zobrazení skupiny zabezpečení pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="ea3aa-125">Get security group view for virtual machine</span></span>

<span data-ttu-id="ea3aa-126">Následující příklad požádá o zobrazení skupiny zabezpečení cílové virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-126">The following example requests the security group view of a targeted virtual machine.</span></span> <span data-ttu-id="ea3aa-127">Výsledky z tohoto příkladu slouží k porovnání pravidel a zabezpečení, které jsou definované vzniku hledání odlišily konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-127">The results from this example can be used to compare to the rules and security defined by the origination to look for configuration drift.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName

$requestBody = @"
{
    'targetResourceId': '${targetUri}'

}
"@
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/securityGroupView?api-version=2016-12-01" $requestBody -verbose
```

## <a name="view-the-response"></a><span data-ttu-id="ea3aa-128">Zobrazení do odpovědi</span><span class="sxs-lookup"><span data-stu-id="ea3aa-128">View the response</span></span>

<span data-ttu-id="ea3aa-129">Následující příklad je odpovědi vrácené z předchozího příkazu.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-129">The following sample is the response returned from the preceding command.</span></span> <span data-ttu-id="ea3aa-130">Výsledky zobrazit všechna pravidla zabezpečení efektivní a použitých na virtuálním počítači rozdělení v skupiny **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, a **EffectiveSecurityRules**.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-130">The results show all the effective and applied security rules on the virtual machine broken down in groups of **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, and **EffectiveSecurityRules**.</span></span>

```json

{
  "networkInterfaces": [
    {
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "securityRules": [
            {
              "name": "default-allow-rdp",
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
              "properties": {
                "provisioningState": "Succeeded",
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "3389",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 1000,
                "direction": "Inbound"
              }
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "name": "AllowVnetInBound",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/",
            "properties": {
              "provisioningState": "Succeeded",
              "description": "Allow inbound traffic from all VMs in VNET",
              "protocol": "*",
              "sourcePortRange": "*",
              "destinationPortRange": "*",
              "sourceAddressPrefix": "VirtualNetwork",
              "destinationAddressPrefix": "VirtualNetwork",
              "access": "Allow",
              "priority": 65000,
              "direction": "Inbound"
            }
          },
          ...
        ],
        "effectiveSecurityRules": [
          {
            "name": "DefaultOutboundDenyAll",
            "protocol": "All",
            "sourcePortRange": "0-65535",
            "destinationPortRange": "0-65535",
            "sourceAddressPrefix": "*",
            "destinationAddressPrefix": "*",
            "access": "Deny",
            "priority": 65500,
            "direction": "Outbound"
          },
          ...
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="ea3aa-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ea3aa-131">Next steps</span></span>

<span data-ttu-id="ea3aa-132">Navštivte [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-security-group-view-powershell.md) se dozvíte, jak automatizovat ověření skupin zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="ea3aa-132">Visit [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-security-group-view-powershell.md) to learn how to automate validation of Network Security Groups.</span></span>


