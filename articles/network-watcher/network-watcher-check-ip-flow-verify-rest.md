---
title: "Ověřte provoz s tokem IP sledovací proces sítě Azure ověřit - REST | Microsoft Docs"
description: "Tento článek popisuje, jak zkontrolovat, pokud je povolené nebo zakázané přenosy do nebo z virtuálního počítače"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 6d3ce00a7d4f9c0cd57fa8815625a1065b03b5b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="b9f31-103">Zkontrolujte, jestli je povolené nebo zakázané s tokem IP přenosy ověřte součást sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="b9f31-103">Check if traffic is allowed or denied with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="b9f31-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b9f31-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="b9f31-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b9f31-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="b9f31-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b9f31-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="b9f31-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b9f31-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="b9f31-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="b9f31-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="b9f31-109">Tok IP ověřte, že je funkce sledovací proces sítě, která vám umožní ověřit, pokud provoz je povolený do nebo z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b9f31-109">IP flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="b9f31-110">Ověření lze spustit pro příchozí nebo odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="b9f31-110">The validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="b9f31-111">Tento scénář je vhodný pro zjištění aktuálního stavu o tom, jestli virtuální počítač může kontaktovat na externí prostředek nebo back-end.</span><span class="sxs-lookup"><span data-stu-id="b9f31-111">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="b9f31-112">Ověřte toku IP umožňuje ověřit, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a vyřešit toky, kteří jsou Blokovaní pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="b9f31-112">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="b9f31-113">Dalším důvodem pro použití IP tok ověření, je potřeba zajistit provoz, který chcete blokovat, je správně blokován nastavením NSG.</span><span class="sxs-lookup"><span data-stu-id="b9f31-113">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b9f31-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b9f31-114">Before you begin</span></span>

<span data-ttu-id="b9f31-115">ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b9f31-115">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="b9f31-116">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="b9f31-116">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="b9f31-117">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="b9f31-117">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="b9f31-118">Scénář</span><span class="sxs-lookup"><span data-stu-id="b9f31-118">Scenario</span></span>

<span data-ttu-id="b9f31-119">Tento scénář používá IP toku ověřte, zda chcete-li ověřit, pokud virtuální počítač může kontaktovat k jinému počítači přes port 443.</span><span class="sxs-lookup"><span data-stu-id="b9f31-119">This scenario uses IP flow Verify to verify if a virtual machine can talk to another machine over port 443.</span></span> <span data-ttu-id="b9f31-120">Pokud je odepřená provoz, vrátí pravidlo zabezpečení, které je odepřen, aby provoz.</span><span class="sxs-lookup"><span data-stu-id="b9f31-120">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="b9f31-121">Další informace o toku IP ověřte naleznete [IP tok ověření – přehled](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b9f31-121">To learn more about IP flow Verify, visit [IP flow verify overview](network-watcher-ip-flow-verify-overview.md)</span></span>

<span data-ttu-id="b9f31-122">V tomto scénáři můžete:</span><span class="sxs-lookup"><span data-stu-id="b9f31-122">In this scenario, you:</span></span>

* <span data-ttu-id="b9f31-123">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="b9f31-123">Retrieve a virtual machine</span></span>
* <span data-ttu-id="b9f31-124">Ověření volání IP toku</span><span class="sxs-lookup"><span data-stu-id="b9f31-124">Call IP flow verify</span></span>
* <span data-ttu-id="b9f31-125">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="b9f31-125">Verify results</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="b9f31-126">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="b9f31-126">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="b9f31-127">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="b9f31-127">Retrieve a virtual machine</span></span>

<span data-ttu-id="b9f31-128">Spusťte následující skript pro vrácení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b9f31-128">Run the following script to return a virtual machine.</span></span> <span data-ttu-id="b9f31-129">Následující kód potřebuje hodnoty pro proměnné:</span><span class="sxs-lookup"><span data-stu-id="b9f31-129">The following code needs values for the variables:</span></span>

* <span data-ttu-id="b9f31-130">**ID předplatného** -Id použít předplatného.</span><span class="sxs-lookup"><span data-stu-id="b9f31-130">**subscriptionId** - The subscription Id to use.</span></span>
* <span data-ttu-id="b9f31-131">**Název skupiny prostředků** -název skupiny prostředků, která obsahuje virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="b9f31-131">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="b9f31-132">Informace, které je potřeba je id v části typ `Microsoft.Compute/virtualMachines`.</span><span class="sxs-lookup"><span data-stu-id="b9f31-132">The information that is needed is the id under the type `Microsoft.Compute/virtualMachines`.</span></span> <span data-ttu-id="b9f31-133">Výsledky by měl vypadat podobně jako následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="b9f31-133">The results should be similar to the following code sample:</span></span>

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a><span data-ttu-id="b9f31-134">Volání IP toku ověřte</span><span class="sxs-lookup"><span data-stu-id="b9f31-134">Call IP flow Verify</span></span>

<span data-ttu-id="b9f31-135">Následující příklad vytvoří požadavek na ověření přenosy dat pro zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b9f31-135">The following example creates a request to verify the traffic for a specified virtual machine.</span></span> <span data-ttu-id="b9f31-136">Odpověď se vrátí, pokud je povolen přenos nebo pokud provoz byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="b9f31-136">The response returns if the traffic is allowed or if the traffic is denied.</span></span> <span data-ttu-id="b9f31-137">Pokud je provoz odepřela také vrátí hodnotu co pravidlo blokuje provoz.</span><span class="sxs-lookup"><span data-stu-id="b9f31-137">If traffic is denied it also returns what rule blocks the traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="b9f31-138">Tok IP ověření vyžaduje, aby prostředků virtuálního počítače je přidělená.</span><span class="sxs-lookup"><span data-stu-id="b9f31-138">IP flow verify requires that the VM resource is allocated.</span></span>

<span data-ttu-id="b9f31-139">Skript vyžaduje Id virtuálního počítače a síťový adaptér na virtuálním počítači prostředku.</span><span class="sxs-lookup"><span data-stu-id="b9f31-139">The script requires the resource Id of a virtual machine and of a network interface card on the virtual machine.</span></span> <span data-ttu-id="b9f31-140">Tyto hodnoty jsou poskytovány tento výstup.</span><span class="sxs-lookup"><span data-stu-id="b9f31-140">These values are provided by the preceding output.</span></span>

> [!Important]
> <span data-ttu-id="b9f31-141">Pro všechna volání REST sledovací proces sítě název skupiny prostředků v identifikátoru URI požadavku je ten, který obsahuje sledovací proces sítě instanci, není diagnostiky akce se provádí na prostředky.</span><span class="sxs-lookup"><span data-stu-id="b9f31-141">For all Network Watcher REST calls the resource group name in the request URI is the one that contains the Network Watcher instance, not the resources you are performing the diagnostic actions on.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-the-results"></a><span data-ttu-id="b9f31-142">Seznámení s výsledky</span><span class="sxs-lookup"><span data-stu-id="b9f31-142">Understanding the results</span></span>

<span data-ttu-id="b9f31-143">Odpověď, kterou jste se vrátit poznáte, jestli provoz povolený nebo zakázaný.</span><span class="sxs-lookup"><span data-stu-id="b9f31-143">The response you get back tells you whether the traffic is allowed or denied.</span></span> <span data-ttu-id="b9f31-144">Odpověď vypadá jako jednu z následujících příkladech:</span><span class="sxs-lookup"><span data-stu-id="b9f31-144">The response looks like one of the following examples:</span></span>

<span data-ttu-id="b9f31-145">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="b9f31-145">**Allowed**</span></span>

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

<span data-ttu-id="b9f31-146">**Odepřen**</span><span class="sxs-lookup"><span data-stu-id="b9f31-146">**Denied**</span></span>

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a><span data-ttu-id="b9f31-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9f31-147">Next steps</span></span>

<span data-ttu-id="b9f31-148">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) Další informace o skupinách zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="b9f31-148">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to learn more about Network Security Groups.</span></span>












