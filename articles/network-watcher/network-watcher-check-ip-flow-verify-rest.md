---
title: "Ověřte aaaVerify provoz s tokem IP sledovací proces sítě Azure - REST | Microsoft Docs"
description: "Tento článek popisuje, jak toocheck Pokud tooor provoz z virtuálního počítače povolený nebo zakázaný"
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
ms.openlocfilehash: 956db0d326db597c6c402a9e8d4a5522c47c02d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="84813-103">Zkontrolujte, jestli je povolené nebo zakázané s tokem IP přenosy ověřte součást sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="84813-103">Check if traffic is allowed or denied with IP flow verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="84813-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84813-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="84813-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="84813-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="84813-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="84813-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="84813-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="84813-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="84813-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="84813-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="84813-109">Tok IP ověřte, že je funkce sledovací proces sítě, která umožňuje tooverify Pokud provoz je povolený tooor z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84813-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="84813-110">ověření Hello lze spustit pro příchozí nebo odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="84813-110">hello validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="84813-111">Tento scénář je užitečné tooget aktuálním stavu o tom, jestli virtuální počítač může kontaktovat tooan externí prostředek nebo back-end.</span><span class="sxs-lookup"><span data-stu-id="84813-111">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="84813-112">Tok IP ověření může být použité tooverify, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a řešení potíží s toky, kteří jsou Blokovaní pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="84813-112">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="84813-113">Dalším důvodem pro použití IP tok ověření je tooensure přenosy, které chcete blokovat je správně blokován nastavením hello NSG.</span><span class="sxs-lookup"><span data-stu-id="84813-113">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="84813-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="84813-114">Before you begin</span></span>

<span data-ttu-id="84813-115">ARMclient je použité toocall hello REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84813-115">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="84813-116">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="84813-116">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="84813-117">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="84813-117">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="84813-118">Scénář</span><span class="sxs-lookup"><span data-stu-id="84813-118">Scenario</span></span>

<span data-ttu-id="84813-119">Tento scénář používá tooverify ověřte toku IP, pokud virtuální počítač může kontaktovat počítač tooanother přes port 443.</span><span class="sxs-lookup"><span data-stu-id="84813-119">This scenario uses IP flow Verify tooverify if a virtual machine can talk tooanother machine over port 443.</span></span> <span data-ttu-id="84813-120">Pokud je odepřená hello provoz, vrátí hello pravidlo zabezpečení, které je odepřen, aby provoz.</span><span class="sxs-lookup"><span data-stu-id="84813-120">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="84813-121">toolearn Další informace o toku IP ověřit, navštivte [IP tok ověření – přehled](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="84813-121">toolearn more about IP flow Verify, visit [IP flow verify overview](network-watcher-ip-flow-verify-overview.md)</span></span>

<span data-ttu-id="84813-122">V tomto scénáři můžete:</span><span class="sxs-lookup"><span data-stu-id="84813-122">In this scenario, you:</span></span>

* <span data-ttu-id="84813-123">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="84813-123">Retrieve a virtual machine</span></span>
* <span data-ttu-id="84813-124">Ověření volání IP toku</span><span class="sxs-lookup"><span data-stu-id="84813-124">Call IP flow verify</span></span>
* <span data-ttu-id="84813-125">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="84813-125">Verify results</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="84813-126">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="84813-126">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="84813-127">Načíst virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="84813-127">Retrieve a virtual machine</span></span>

<span data-ttu-id="84813-128">Spusťte následující skript tooreturn hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="84813-128">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="84813-129">Hello následující kód potřebuje hodnoty pro proměnné hello:</span><span class="sxs-lookup"><span data-stu-id="84813-129">hello following code needs values for hello variables:</span></span>

* <span data-ttu-id="84813-130">**ID předplatného** -hello toouse Id předplatného.</span><span class="sxs-lookup"><span data-stu-id="84813-130">**subscriptionId** - hello subscription Id toouse.</span></span>
* <span data-ttu-id="84813-131">**Název skupiny prostředků** – hello název skupiny prostředků, která obsahuje virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="84813-131">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="84813-132">Hello informace, které je potřeba je hello id v rámci hello typu `Microsoft.Compute/virtualMachines`.</span><span class="sxs-lookup"><span data-stu-id="84813-132">hello information that is needed is hello id under hello type `Microsoft.Compute/virtualMachines`.</span></span> <span data-ttu-id="84813-133">výsledky Hello by měl vypadat podobně jako toohello následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="84813-133">hello results should be similar toohello following code sample:</span></span>

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

## <a name="call-ip-flow-verify"></a><span data-ttu-id="84813-134">Volání IP toku ověřte</span><span class="sxs-lookup"><span data-stu-id="84813-134">Call IP flow Verify</span></span>

<span data-ttu-id="84813-135">Hello následující příklad vytvoří přenosem hello tooverify požadavku pro zadaný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="84813-135">hello following example creates a request tooverify hello traffic for a specified virtual machine.</span></span> <span data-ttu-id="84813-136">Hello odpověď se vrátí, pokud hello provoz je povolený, nebo pokud hello provoz byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="84813-136">hello response returns if hello traffic is allowed or if hello traffic is denied.</span></span> <span data-ttu-id="84813-137">Pokud je provoz odepřela také vrátí hodnotu jaké bloky pravidel hello provoz.</span><span class="sxs-lookup"><span data-stu-id="84813-137">If traffic is denied it also returns what rule blocks hello traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="84813-138">Tok IP ověření vyžaduje, aby hello prostředků virtuálního počítače je přidělená.</span><span class="sxs-lookup"><span data-stu-id="84813-138">IP flow verify requires that hello VM resource is allocated.</span></span>

<span data-ttu-id="84813-139">skript Hello vyžaduje Id virtuálního počítače a síťový adaptér na virtuálním počítači hello hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="84813-139">hello script requires hello resource Id of a virtual machine and of a network interface card on hello virtual machine.</span></span> <span data-ttu-id="84813-140">Tyto hodnoty jsou poskytovány hello předcházející výstup.</span><span class="sxs-lookup"><span data-stu-id="84813-140">These values are provided by hello preceding output.</span></span>

> [!Important]
> <span data-ttu-id="84813-141">Pro všechny REST sledovací proces sítě hello volání název skupiny prostředků v žádosti o hello, že je identifikátor URI hello ten, který obsahuje instanci hello sledovací proces sítě, není hello prostředků, kterou provádíte hello diagnostiky akce na.</span><span class="sxs-lookup"><span data-stu-id="84813-141">For all Network Watcher REST calls hello resource group name in hello request URI is hello one that contains hello Network Watcher instance, not hello resources you are performing hello diagnostic actions on.</span></span>

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

## <a name="understanding-hello-results"></a><span data-ttu-id="84813-142">Seznámení s výsledky hello</span><span class="sxs-lookup"><span data-stu-id="84813-142">Understanding hello results</span></span>

<span data-ttu-id="84813-143">Hello odpověď, kterou jste se vrátit poznáte, jestli je povolené nebo zakázané hello přenosy.</span><span class="sxs-lookup"><span data-stu-id="84813-143">hello response you get back tells you whether hello traffic is allowed or denied.</span></span> <span data-ttu-id="84813-144">odpověď Hello vypadá jako jeden z hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="84813-144">hello response looks like one of hello following examples:</span></span>

<span data-ttu-id="84813-145">**Povoleno**</span><span class="sxs-lookup"><span data-stu-id="84813-145">**Allowed**</span></span>

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

<span data-ttu-id="84813-146">**Odepřen**</span><span class="sxs-lookup"><span data-stu-id="84813-146">**Denied**</span></span>

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a><span data-ttu-id="84813-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84813-147">Next steps</span></span>

<span data-ttu-id="84813-148">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn Další informace o skupinách zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="84813-148">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn more about Network Security Groups.</span></span>












