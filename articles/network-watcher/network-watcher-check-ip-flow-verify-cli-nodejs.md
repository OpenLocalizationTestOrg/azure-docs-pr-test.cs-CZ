---
title: "provoz aaaVerify s Azure sítě sledovacích procesů IP toku ověřit - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocheck Pokud tooor provoz z virtuálního počítače povolený nebo zakázaný pomocí rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: c6becc5c142837b04d15490b2b3bd11124434570
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="496ec-103">Zkontrolujte, zda je provoz povolen nebo odepřen tooor z virtuálního počítače s tok ověření IP a součást sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="496ec-103">Check if traffic is allowed or denied tooor from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="496ec-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="496ec-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="496ec-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="496ec-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="496ec-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="496ec-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="496ec-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="496ec-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="496ec-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="496ec-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="496ec-109">Tok IP ověřte je funkce sledovací proces sítě, která umožňuje tooverify Pokud provoz je povolený tooor z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="496ec-109">IP Flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="496ec-110">Tento scénář je užitečné tooget aktuálním stavu o tom, jestli virtuální počítač může kontaktovat tooan externí prostředek nebo back-end.</span><span class="sxs-lookup"><span data-stu-id="496ec-110">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or backend.</span></span> <span data-ttu-id="496ec-111">Tok IP ověření může být použité tooverify, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a řešení potíží s toky, kteří jsou Blokovaní pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="496ec-111">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="496ec-112">Dalším důvodem pro použití IP tok ověření je tooensure přenosy, které chcete blokovat je správně blokován nastavením hello NSG.</span><span class="sxs-lookup"><span data-stu-id="496ec-112">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

<span data-ttu-id="496ec-113">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="496ec-113">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="496ec-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="496ec-114">Before you begin</span></span>

<span data-ttu-id="496ec-115">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě nebo existující instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="496ec-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="496ec-116">scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.</span><span class="sxs-lookup"><span data-stu-id="496ec-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="496ec-117">Scénář</span><span class="sxs-lookup"><span data-stu-id="496ec-117">Scenario</span></span>

<span data-ttu-id="496ec-118">Tento scénář používá tok ověření IP tooverify, pokud virtuální počítač může kontaktovat tooa známé Bing IP adresu.</span><span class="sxs-lookup"><span data-stu-id="496ec-118">This scenario uses IP Flow Verify tooverify if a virtual machine can talk tooa known Bing IP address.</span></span> <span data-ttu-id="496ec-119">Pokud je odepřená hello provoz, vrátí hello pravidlo zabezpečení, které je odepřen, aby provoz.</span><span class="sxs-lookup"><span data-stu-id="496ec-119">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="496ec-120">toolearn Další informace o toku ověřit IP, navštivte [přehled tok ověření IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="496ec-120">toolearn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>


## <a name="get-a-vm"></a><span data-ttu-id="496ec-121">Získat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="496ec-121">Get a VM</span></span>

<span data-ttu-id="496ec-122">Tok IP ověřte testy tooor provoz z IP adresy na virtuální počítač tooor z vzdálený cíl.</span><span class="sxs-lookup"><span data-stu-id="496ec-122">IP flow verify tests traffic tooor from an IP address on a virtual machine tooor from a remote destination.</span></span> <span data-ttu-id="496ec-123">Id virtuálního počítače je vyžadováno pro rutinu hello.</span><span class="sxs-lookup"><span data-stu-id="496ec-123">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="496ec-124">Pokud už znáte ID hello toouse hello virtuálního počítače, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="496ec-124">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="get-hello-nics"></a><span data-ttu-id="496ec-125">Získat hello síťové karty</span><span class="sxs-lookup"><span data-stu-id="496ec-125">Get hello NICS</span></span>

<span data-ttu-id="496ec-126">v tomto příkladu, nemůžeme načíst hello síťové adaptéry na virtuálním počítači, je potřeba Hello IP adresa síťového adaptéru na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="496ec-126">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="496ec-127">Pokud už znáte hello IP adresu, kterou chcete tootest hello virtuálního počítače, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="496ec-127">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```
azure network nic show -g resourceGroupName -n nicName
```

## <a name="run-ip-flow-verify"></a><span data-ttu-id="496ec-128">Ověření spuštění toku IP</span><span class="sxs-lookup"><span data-stu-id="496ec-128">Run IP flow verify</span></span>

<span data-ttu-id="496ec-129">Teď, když máme hello informace potřebné toorun hello rutiny, spustíme hello `network watcher ip-flow-verify` rutiny tootest hello provoz.</span><span class="sxs-lookup"><span data-stu-id="496ec-129">Now that we have hello information needed toorun hello cmdlet, we run hello `network watcher ip-flow-verify` cmdlet tootest hello traffic.</span></span> <span data-ttu-id="496ec-130">V tomto příkladu používáme hello první IP adresou na síťovém adaptéru hello první.</span><span class="sxs-lookup"><span data-stu-id="496ec-130">In this example, we are using hello first IP address on hello first NIC.</span></span>

```
azure network watcher ip-flow-verify -g resourceGroupName -n networkWatcherName -t targetResourceId -d directionInboundorOutbound -p protocolTCPorUDP -o localPort -m remotePort -l localIpAddr -r remoteIpAddr
```

> [!NOTE]
> <span data-ttu-id="496ec-131">Tok IP ověření vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.</span><span class="sxs-lookup"><span data-stu-id="496ec-131">IP Flow verify requires that hello VM resource is allocated toorun.</span></span>

## <a name="review-results"></a><span data-ttu-id="496ec-132">Zkontrolujte výsledky</span><span class="sxs-lookup"><span data-stu-id="496ec-132">Review Results</span></span>

<span data-ttu-id="496ec-133">Po spuštění `network watcher ip-flow-verify` hello výsledky se vrátí, hello následující příklad je hello výsledky vrácené z hello předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="496ec-133">After running `network watcher ip-flow-verify` hello results are returned, hello following example is hello results returned from hello preceding step.</span></span>

```
data:    Access                          : Deny
data:    Rule Name                       : defaultSecurityRules/DefaultInboundDenyAll
info:    network watcher ip-flow-verify command OK
```

## <a name="next-steps"></a><span data-ttu-id="496ec-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="496ec-134">Next steps</span></span>

<span data-ttu-id="496ec-135">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="496ec-135">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

<span data-ttu-id="496ec-136">Další nastavení NSG tooaudit navštivte stránky [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="496ec-136">Learn tooaudit your NSG settings by visiting [Auditing Network Security Groups (NSG) with Network Watcher](network-watcher-nsg-auditing-powershell.md).</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png
