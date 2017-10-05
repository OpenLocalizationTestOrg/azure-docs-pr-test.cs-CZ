---
title: "Ověřte provoz s IP Adresou sledovací proces sítě Azure tok ověření - portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak zkontrolovat, pokud je povolené nebo zakázané přenosy do nebo z virtuálního počítače"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7db29c186cf6e6f3b40a680ab76f1d2763f806ba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="4d2ea-103">Zkontrolujte, jestli je provoz povolen nebo odepřen do nebo z virtuálního počítače s tok ověření IP součást sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="4d2ea-103">Check if traffic is allowed or denied to or from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="4d2ea-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4d2ea-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="4d2ea-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d2ea-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="4d2ea-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4d2ea-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="4d2ea-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4d2ea-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="4d2ea-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="4d2ea-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="4d2ea-109">Tok IP ověřte, že je funkce sledovací proces sítě, která vám umožní ověřit, pokud provoz je povolený do nebo z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-109">IP flow verify is a feature of Network Watcher that allows you to verify if traffic is allowed to or from a virtual machine.</span></span> <span data-ttu-id="4d2ea-110">Ověření lze spustit pro příchozí nebo odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-110">The validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="4d2ea-111">Tento scénář je vhodný pro zjištění aktuálního stavu o tom, jestli virtuální počítač může kontaktovat na externí prostředek nebo jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-111">This scenario is useful to get a current state of whether a virtual machine can talk to an external resource or another resource.</span></span> <span data-ttu-id="4d2ea-112">Ověřte toku IP umožňuje ověřit, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a vyřešit toky, kteří jsou Blokovaní pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-112">IP flow verify can be used to verify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="4d2ea-113">Dalším důvodem pro použití IP tok ověření, je potřeba zajistit provoz, který chcete blokovat, je správně blokován nastavením NSG.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-113">Another reason for using IP flow verify is to ensure traffic that you want blocked is being blocked properly by the NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4d2ea-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4d2ea-114">Before you begin</span></span>

<span data-ttu-id="4d2ea-115">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě nebo máte existující instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="4d2ea-116">Tento scénář také předpokládá, že skupina prostředků se platný virtuální počítač existuje má být použit.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-116">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="4d2ea-117">Scénář</span><span class="sxs-lookup"><span data-stu-id="4d2ea-117">Scenario</span></span>

<span data-ttu-id="4d2ea-118">Tento scénář používá k ověření, pokud virtuální počítač může kontaktovat k jinému počítači přes port 443 tok ověření IP.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-118">This scenario uses IP Flow Verify to verify if a virtual machine can talk to another machine over port 443.</span></span> <span data-ttu-id="4d2ea-119">Pokud je odepřená provoz, vrátí pravidlo zabezpečení, které je odepřen, aby provoz.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-119">If the traffic is denied, it returns the security rule that is denying that traffic.</span></span> <span data-ttu-id="4d2ea-120">Další informace o toku ověřte IP, navštivte [přehled tok ověření IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4d2ea-120">To learn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

### <a name="run-ip-flow-verify"></a><span data-ttu-id="4d2ea-121">Ověření spuštění toku IP</span><span class="sxs-lookup"><span data-stu-id="4d2ea-121">Run IP flow verify</span></span>

<span data-ttu-id="4d2ea-122">Přejděte do vaší sledovací proces sítě a klikněte na tlačítko **IP tok ověření**.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-122">Navigate to your Network Watcher and click **IP flow verify**.</span></span> <span data-ttu-id="4d2ea-123">Vyberte virtuální počítač a chcete ověřit provoz z síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-123">Select the virtual machine and network interface you want to verify traffic from.</span></span> <span data-ttu-id="4d2ea-124">Zadejte veškeré další filtrování informace a klikněte na tlačítko **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-124">Enter any additional filtering information and click **Check**.</span></span>

<span data-ttu-id="4d2ea-125">Po kliknutí na tlačítko **zkontrolujte**, se kontroluje toku na základě kritérií, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-125">Once you click **Check**, the flow based on the criteria you provided is checked.</span></span> <span data-ttu-id="4d2ea-126">Výsledkem je buď **povoleného přístupu** nebo **byl odepřen přístup**.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-126">The result is either **Access allowed** or **Access denied**.</span></span> <span data-ttu-id="4d2ea-127">Pokud byl odepřen přístup, je k dispozici skupina zabezpečení sítě (NSG) a zabezpečení pravidlo, které blokování provozu.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-127">If access is denied, the Network Security Group (NSG) and security rule that block traffic is provided.</span></span> <span data-ttu-id="4d2ea-128">Pokud odmítnutí provoz je očekávané chování, pravidlo bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-128">If the denial of traffic is expected behavior, then the rule was successful.</span></span>

> [!NOTE]
> <span data-ttu-id="4d2ea-129">Tok IP ověření vyžaduje, aby prostředků virtuálního počítače je přidělená.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-129">IP flow verify requires that the VM resource is allocated.</span></span>

<span data-ttu-id="4d2ea-130">Jak je vidět na následujícím obrázku, byla povolena odchozí provoz HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-130">As you can see from the following image, the outbound HTTPS traffic was allowed.</span></span>

![tok IP ověřte – přehled][1]

<span data-ttu-id="4d2ea-132">Jak je vidět na následujícím obrázku, provoz se změní na příchozí a příchozí port změnit tak, aby 123.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-132">As seen in the following image, traffic is changed to inbound and the inbound port changed to 123.</span></span> <span data-ttu-id="4d2ea-133">Provoz je nyní odepřen, zprávy poskytnuté společně s pravidlo zabezpečení sítě skupiny a zabezpečení, odepřít provoz "Přístup byl odepřen".</span><span class="sxs-lookup"><span data-stu-id="4d2ea-133">Traffic is now denied, the message "Access denied" is provided along with the network security group and security rule that deny the traffic.</span></span>

![výsledky toku IP][2]

## <a name="next-steps"></a><span data-ttu-id="4d2ea-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d2ea-135">Next steps</span></span>

<span data-ttu-id="4d2ea-136">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) sledovat pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="4d2ea-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













