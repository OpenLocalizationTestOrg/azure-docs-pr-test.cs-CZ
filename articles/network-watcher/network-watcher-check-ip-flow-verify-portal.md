---
title: "Ověřte aaaVerify provoz s tokem IP sledovací proces sítě Azure - portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toocheck Pokud tooor provoz z virtuálního počítače povolený nebo zakázaný"
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
ms.openlocfilehash: abf639f36d32f3416dd927e66b635267b746e62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a><span data-ttu-id="a7029-103">Zkontrolujte, zda je provoz povolen nebo odepřen tooor z virtuálního počítače s tok ověření IP a součást sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="a7029-103">Check if traffic is allowed or denied tooor from a VM with IP Flow Verify a component of Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="a7029-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a7029-104">Azure portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)
> - [<span data-ttu-id="a7029-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a7029-105">PowerShell</span></span>](network-watcher-check-ip-flow-verify-powershell.md)
> - [<span data-ttu-id="a7029-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a7029-106">CLI 1.0</span></span>](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [<span data-ttu-id="a7029-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a7029-107">CLI 2.0</span></span>](network-watcher-check-ip-flow-verify-cli.md)
> - [<span data-ttu-id="a7029-108">Rozhraní API Azure REST</span><span class="sxs-lookup"><span data-stu-id="a7029-108">Azure REST API</span></span>](network-watcher-check-ip-flow-verify-rest.md)


<span data-ttu-id="a7029-109">Tok IP ověřte, že je funkce sledovací proces sítě, která umožňuje tooverify Pokud provoz je povolený tooor z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7029-109">IP flow verify is a feature of Network Watcher that allows you tooverify if traffic is allowed tooor from a virtual machine.</span></span> <span data-ttu-id="a7029-110">ověření Hello lze spustit pro příchozí nebo odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="a7029-110">hello validation can be run for incoming or outgoing traffic.</span></span> <span data-ttu-id="a7029-111">Tento scénář je užitečné tooget aktuálním stavu o tom, jestli virtuální počítač může kontaktovat tooan externí prostředek nebo jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="a7029-111">This scenario is useful tooget a current state of whether a virtual machine can talk tooan external resource or another resource.</span></span> <span data-ttu-id="a7029-112">Tok IP ověření může být použité tooverify, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a řešení potíží s toky, kteří jsou Blokovaní pravidla NSG.</span><span class="sxs-lookup"><span data-stu-id="a7029-112">IP flow verify can be used tooverify if your Network Security Group (NSG) rules are properly configured and troubleshoot flows that are being blocked by NSG rules.</span></span> <span data-ttu-id="a7029-113">Dalším důvodem pro použití IP tok ověření je tooensure přenosy, které chcete blokovat je správně blokován nastavením hello NSG.</span><span class="sxs-lookup"><span data-stu-id="a7029-113">Another reason for using IP flow verify is tooensure traffic that you want blocked is being blocked properly by hello NSG.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a7029-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a7029-114">Before you begin</span></span>

<span data-ttu-id="a7029-115">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě nebo existující instanci sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="a7029-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher or have an existing instance of Network Watcher.</span></span> <span data-ttu-id="a7029-116">scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.</span><span class="sxs-lookup"><span data-stu-id="a7029-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="a7029-117">Scénář</span><span class="sxs-lookup"><span data-stu-id="a7029-117">Scenario</span></span>

<span data-ttu-id="a7029-118">Tento scénář používá tok ověření IP tooverify, pokud virtuální počítač může kontaktovat počítač tooanother přes port 443.</span><span class="sxs-lookup"><span data-stu-id="a7029-118">This scenario uses IP Flow Verify tooverify if a virtual machine can talk tooanother machine over port 443.</span></span> <span data-ttu-id="a7029-119">Pokud je odepřená hello provoz, vrátí hello pravidlo zabezpečení, které je odepřen, aby provoz.</span><span class="sxs-lookup"><span data-stu-id="a7029-119">If hello traffic is denied, it returns hello security rule that is denying that traffic.</span></span> <span data-ttu-id="a7029-120">toolearn Další informace o toku ověřit IP, navštivte [přehled tok ověření IP](network-watcher-ip-flow-verify-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a7029-120">toolearn more about IP Flow Verify, visit [IP Flow Verify Overview](network-watcher-ip-flow-verify-overview.md)</span></span>

### <a name="run-ip-flow-verify"></a><span data-ttu-id="a7029-121">Ověření spuštění toku IP</span><span class="sxs-lookup"><span data-stu-id="a7029-121">Run IP flow verify</span></span>

<span data-ttu-id="a7029-122">Přejděte tooyour sledovací proces sítě a klikněte na tlačítko **IP tok ověření**.</span><span class="sxs-lookup"><span data-stu-id="a7029-122">Navigate tooyour Network Watcher and click **IP flow verify**.</span></span> <span data-ttu-id="a7029-123">Vyberte virtuální počítač hello a síťové rozhraní, které chcete tooverify provoz z.</span><span class="sxs-lookup"><span data-stu-id="a7029-123">Select hello virtual machine and network interface you want tooverify traffic from.</span></span> <span data-ttu-id="a7029-124">Zadejte veškeré další filtrování informace a klikněte na tlačítko **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="a7029-124">Enter any additional filtering information and click **Check**.</span></span>

<span data-ttu-id="a7029-125">Po kliknutí na tlačítko **zkontrolujte**, se kontroluje hello toku na základě kritérií hello jste zadali.</span><span class="sxs-lookup"><span data-stu-id="a7029-125">Once you click **Check**, hello flow based on hello criteria you provided is checked.</span></span> <span data-ttu-id="a7029-126">výsledek Hello je buď **povoleného přístupu** nebo **byl odepřen přístup**.</span><span class="sxs-lookup"><span data-stu-id="a7029-126">hello result is either **Access allowed** or **Access denied**.</span></span> <span data-ttu-id="a7029-127">Pokud byl odepřen přístup, hello skupina zabezpečení sítě (NSG) a pravidlo zabezpečení, která blokuje komunikaci, je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a7029-127">If access is denied, hello Network Security Group (NSG) and security rule that block traffic is provided.</span></span> <span data-ttu-id="a7029-128">Pokud hello odmítnutí provoz je očekávané chování, hello pravidlo bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="a7029-128">If hello denial of traffic is expected behavior, then hello rule was successful.</span></span>

> [!NOTE]
> <span data-ttu-id="a7029-129">Tok IP ověření vyžaduje, aby hello prostředků virtuálního počítače je přidělená.</span><span class="sxs-lookup"><span data-stu-id="a7029-129">IP flow verify requires that hello VM resource is allocated.</span></span>

<span data-ttu-id="a7029-130">Jak vidíte z hello následující bitové kopie, byla povolena hello odchozí komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a7029-130">As you can see from hello following image, hello outbound HTTPS traffic was allowed.</span></span>

![tok IP ověřte – přehled][1]

<span data-ttu-id="a7029-132">Jak je vidět v hello následující bitové kopie, provoz mění tooinbound a hello příchozí too123 port změnit.</span><span class="sxs-lookup"><span data-stu-id="a7029-132">As seen in hello following image, traffic is changed tooinbound and hello inbound port changed too123.</span></span> <span data-ttu-id="a7029-133">Provoz je nyní odepřen, uvítací zprávu "Přístup byl odepřen" je k dispozici společně s hello pravidla zabezpečení sítě skupiny a zabezpečení, zakážou provoz hello.</span><span class="sxs-lookup"><span data-stu-id="a7029-133">Traffic is now denied, hello message "Access denied" is provided along with hello network security group and security rule that deny hello traffic.</span></span>

![výsledky toku IP][2]

## <a name="next-steps"></a><span data-ttu-id="a7029-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7029-135">Next steps</span></span>

<span data-ttu-id="a7029-136">Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které jsou definovány.</span><span class="sxs-lookup"><span data-stu-id="a7029-136">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













