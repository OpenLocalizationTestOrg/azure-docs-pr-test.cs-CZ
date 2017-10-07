---
title: "zobrazení skupiny toosecurity aaaIntroduction v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello schopností zobrazení zabezpečení sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: c2f6dbbffd0aedbb9db4b69d1758f2e66dd7abb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonetwork-security-group-view-in-azure-network-watcher"></a><span data-ttu-id="721fd-103">Zobrazení skupiny zabezpečení Úvod toonetwork v sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="721fd-103">Introduction toonetwork security group view in Azure Network Watcher</span></span>

<span data-ttu-id="721fd-104">Skupiny zabezpečení sítě na úrovni podsítě nebo na úrovni seskupování souvisejí.</span><span class="sxs-lookup"><span data-stu-id="721fd-104">Network Security groups are associated at a subnet level or at a NIC level.</span></span> <span data-ttu-id="721fd-105">Když přidružené na úrovni podsítě, bude se vztahovat tooall hello instance virtuálních počítačů v podsíti hello.</span><span class="sxs-lookup"><span data-stu-id="721fd-105">When associated at a subnet level, it applies tooall hello VM instances in hello subnet.</span></span> <span data-ttu-id="721fd-106">Skupina zabezpečení sítě zobrazení vrátí všechny hello nakonfigurované skupiny Nsg a pravidel, které jsou přidružené úrovni síťových Adaptérů a podsítě pro virtuální počítač poskytuje přehled o konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="721fd-106">Network Security Group view returns all hello configured NSGs and rules that are associated at a NIC and subnet level for a virtual machine providing insight into hello configuration.</span></span> <span data-ttu-id="721fd-107">Kromě toho pravidla efektivní zabezpečení hello se vrátí pro každou hello síťových adaptérů ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="721fd-107">In addition, hello effective security rules are returned for each of hello NICs in a VM.</span></span> <span data-ttu-id="721fd-108">Zobrazení pomocí skupiny zabezpečení sítě, můžete vyhodnotit virtuální počítač chyb zabezpečení sítě, jako je například otevřené porty.</span><span class="sxs-lookup"><span data-stu-id="721fd-108">Using Network Security Group view, you can assess a VM for network vulnerabilities such as open ports.</span></span> <span data-ttu-id="721fd-109">Můžete také ověřit, pokud vaše skupina zabezpečení sítě funguje podle očekávání, na základě [porovnání mezi hello nakonfigurované a hello pravidla efektivní zabezpečení](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="721fd-109">You can also validate if your Network Security Group is working as expected based on a [comparison between hello configured and hello effective security rules](network-watcher-nsg-auditing-powershell.md).</span></span>

<span data-ttu-id="721fd-110">Více rozšířených případ použití se dodržování předpisů pro zabezpečení a auditování.</span><span class="sxs-lookup"><span data-stu-id="721fd-110">A more extended use case is in security compliance and auditing.</span></span> <span data-ttu-id="721fd-111">Můžete definovat doporučený sadu pravidel zabezpečení jako model pro řízení zabezpečení ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="721fd-111">You can define a prescriptive set of security rules as a model for security governance in your organization.</span></span> <span data-ttu-id="721fd-112">Audit pravidelné dodržování předpisů, můžou se implementovat v programový způsob tak, že porovnáte hello doporučený pravidla s hello efektivní pravidla pro každou hello virtuálních počítačů ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="721fd-112">A periodic compliance audit can be implemented in a programmatic way by comparing hello prescriptive rules with hello effective rules for each of hello VMs in your network.</span></span>

<span data-ttu-id="721fd-113">V hello portálu pravidla jsou dělený platné, podsítě, síťové a výchozí.</span><span class="sxs-lookup"><span data-stu-id="721fd-113">In hello portal rules are divided by Effective, Subnet, Network Interface, and Default.</span></span> <span data-ttu-id="721fd-114">To poskytuje jednoduché zobrazení do hello pravidla použít tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="721fd-114">This provides a simple view into hello rules applied tooa virtual machine.</span></span> <span data-ttu-id="721fd-115">Tlačítko Stáhnout se tooeasily stáhnout všechna pravidla zabezpečení hello bez ohledu na to, karta hello do souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="721fd-115">A download button is provided tooeasily download all hello security rules no matter hello tab into a CSV file.</span></span>

![zobrazení skupiny zabezpečení][1]

<span data-ttu-id="721fd-117">Pravidla lze vybrat a otevře se nové okno tooshow hello skupinu zabezpečení sítě a zdrojové a cílové předpony.</span><span class="sxs-lookup"><span data-stu-id="721fd-117">Rules can be selected and a new blade opens up tooshow hello Network Security Group and source and destination prefixes.</span></span> <span data-ttu-id="721fd-118">V tomto okně můžete přejít přímo prostředků toohello skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="721fd-118">From this blade you can navigate directly toohello Network Security Group resource.</span></span>

![Přejít k podrobnostem][2]

### <a name="next-steps"></a><span data-ttu-id="721fd-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="721fd-120">Next steps</span></span>

<span data-ttu-id="721fd-121">Zjistěte, jak tooaudit zabezpečení sítě skupiny nastavení, navštivte stránky [nastavení auditu skupinu zabezpečení sítě pomocí prostředí PowerShell](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="721fd-121">Learn how tooaudit your Network Security Group settings by visiting [Audit Network Security Group settings with PowerShell](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









