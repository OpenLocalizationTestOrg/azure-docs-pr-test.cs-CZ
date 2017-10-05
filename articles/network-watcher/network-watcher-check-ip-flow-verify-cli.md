---
title: "Ověřte provozu pomocí Azure sítě sledovacích procesů IP toku ověřit - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak zkontrolovat, pokud provoz nebo z virtuálního počítače povolený nebo zakázaný pomocí rozhraní příkazového řádku Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 92b857ed-c834-4c1b-8ee9-538e7ae7391d
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 0b52257a6c38a4392573672b7190d2269c2f145a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="8cb63-103">Zkontrolujte, jestli je provoz povolen nebo odepřen do nebo z virtuálního počítače s tok ověření IP součást sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="8cb63-103">Check if traffic is allowed or denied to or from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="8cb63-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8cb63-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="8cb63-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8cb63-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="8cb63-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8cb63-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="8cb63-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8cb63-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="8cb63-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="8cb63-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="8cb63-109">Tok IP ověřte je funkce sledovací proces sítě, která vám umožní ověřit, pokud provoz je povolený do nebo z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8cb63-109">IP Flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="8cb63-110">Tento scénář je vhodný pro zjištění aktuálního stavu o tom, jestli virtuální počítač může kontaktovat na externí prostředek nebo back-end.</span><span class="sxs-lookup"><span data-stu-id="8cb63-110">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or backend.</span></span> <span data-ttu-id="8cb63-111">Ověřte toku IP umožňuje ověřit, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a vyřešit toky, kteří jsou Blokovaní pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="8cb63-111">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="8cb63-112">Dalším důvodem pro použití IP tok ověření, je potřeba zajistit provoz, který chcete blokovat, je správně blokován nastavením NSG.</span><span class="sxs-lookup"><span data-stu-id="8cb63-112">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

<span data-ttu-id="8cb63-113">Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="8cb63-113">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="8cb63-114">Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="8cb63-114">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8cb63-115">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8cb63-115">Before you begin</span></span>

<span data-ttu-id="8cb63-116">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě nebo máte existující instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="8cb63-116">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="8cb63-117">Tento scénář také předpokládá, že skupina prostředků se platný virtuální počítač existuje má být použit.</span><span class="sxs-lookup"><span data-stu-id="8cb63-117">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="8cb63-118">Scénář</span><span class="sxs-lookup"><span data-stu-id="8cb63-118">Scenario</span></span>

<span data-ttu-id="8cb63-119">Tento scénář používá k ověření, pokud virtuální počítač může kontaktovat známé Bing IP adresu IP tok ověření.</span><span class="sxs-lookup"><span data-stu-id="8cb63-119">This scenario uses IP Flow Verify to verify if a virtual machine can talk to a known Bing IP address.</span></span> <span data-ttu-id="8cb63-120">Pokud je odepřená provoz, vrátí pravidlo zabezpečení, které je odepřen, aby provoz.</span><span class="sxs-lookup"><span data-stu-id="8cb63-120">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="8cb63-121">Další informace o toku ověřte IP, navštivte [přehled tok ověření IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="8cb63-121">To learn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

## <a name="get-a-vm"></a><span data-ttu-id="8cb63-122">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="8cb63-122">Get a VM</span></span>

<span data-ttu-id="8cb63-123">Tok IP ověřte testy přenosy do nebo z IP adresy na virtuální počítač do nebo z vzdálený cíl.</span><span class="sxs-lookup"><span data-stu-id="8cb63-123">IP flow verify tests traffic to or from an IP address on a virtual machine to or from a remote destination.</span></span> <span data-ttu-id="8cb63-124">Id virtuálního počítače je vyžadováno pro rutinu.</span><span class="sxs-lookup"><span data-stu-id="8cb63-124">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="8cb63-125">Pokud už znáte ID virtuálního počítače používat, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="8cb63-125">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```azurecli
az vm show --resource-group MyResourceGroup5431 --name MyVM-Web
```

## <a name="get-the-nics"></a><span data-ttu-id="8cb63-126">Získat síťové karty</span><span class="sxs-lookup"><span data-stu-id="8cb63-126">Get the NICS</span></span>

<span data-ttu-id="8cb63-127">IP adresa síťového adaptéru na virtuálním počítači je potřeba v tomto příkladu, nemůžeme načíst síťové adaptéry na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="8cb63-127">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="8cb63-128">Pokud už znáte IP adresu, která chcete testovat na virtuálním počítači, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="8cb63-128">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```azurecli
az network nic show --resource-group MyResourceGroup5431 --name MyNic-Web
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="8cb63-129">Ověření spuštění toku IP</span><span class="sxs-lookup"><span data-stu-id="8cb63-129">Run IP flow verify</span></span>

<span data-ttu-id="8cb63-130">Teď, když máme informace potřebné ke spuštění rutiny jsme spustit `az network watcher test-ip-flow` rutiny k otestování provozu.</span><span class="sxs-lookup"><span data-stu-id="8cb63-130">Now that we have the information needed to run the cmdlet, we run the `az network watcher test-ip-flow` cmdlet to test the traffic.</span></span> <span data-ttu-id="8cb63-131">V tomto příkladu používáme první IP adresu na první síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="8cb63-131">In this example, we are using the first IP address on the first NIC.</span></span>

```azurecli
az network watcher test-ip-flow --resource-group resourceGroupName --direction directionInboundorOutbound --protocol protocolTCPorUDP --local ipAddressandPort --remote ipAddressandPort --vm vmNameorID --nic nicNameorID
```

> [!NOTE]
> <span data-ttu-id="8cb63-132">Tok IP ověření vyžaduje, aby prostředků virtuálního počítače je přidělená ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="8cb63-132">IP Flow verify requires that the VM resource is allocated to run.</span></span>

## <a name="review-results"></a><span data-ttu-id="8cb63-133">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="8cb63-133">Review Results</span></span>

<span data-ttu-id="8cb63-134">Po spuštění `az network watcher test-ip-flow` budou vráceny výsledky, v následujícím příkladu je výsledky vrácené z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="8cb63-134">After running `az network watcher test-ip-flow` the results are returned, the following example is the results returned from the preceding step.</span></span>

```azurecli
{
    "access": "Allow",
    "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

## <a name="next-steps"></a><span data-ttu-id="8cb63-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8cb63-135">Next steps</span></span>

<span data-ttu-id="8cb63-136">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) sledovat pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="8cb63-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

<span data-ttu-id="8cb63-137">Naučte se audit nastavení NSG navštivte stránky [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8cb63-137">Learn to audit your NSG settings by visiting [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md).</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
