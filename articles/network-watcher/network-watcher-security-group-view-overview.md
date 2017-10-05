---
title: "Úvod do zobrazení skupiny zabezpečení v sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje přehled funkce zobrazení zabezpečení sledovací proces sítě"
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
ms.openlocfilehash: 2c581a2d152a6d3f16de8f249e27a426aa9f844f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a><span data-ttu-id="243db-103">Úvod do zobrazení skupiny zabezpečení sítě v sledovací proces sítě Azure</span><span class="sxs-lookup"><span data-stu-id="243db-103">Introduction to network security group view in Azure Network Watcher</span></span>

<span data-ttu-id="243db-104">Skupiny zabezpečení sítě na úrovni podsítě nebo na úrovni seskupování souvisejí.</span><span class="sxs-lookup"><span data-stu-id="243db-104">Network Security groups are associated at a subnet level or at a NIC level.</span></span> <span data-ttu-id="243db-105">Když přidružené na úrovni podsítě, bude se vztahovat na všechny instance virtuálního počítače v podsíti.</span><span class="sxs-lookup"><span data-stu-id="243db-105">When associated at a subnet level, it applies to all the VM instances in the subnet.</span></span> <span data-ttu-id="243db-106">Skupina zabezpečení sítě zobrazení vrátí nakonfigurované skupiny Nsg a pravidel, které jsou přidružené úrovni síťových Adaptérů a podsítě pro virtuální počítač poskytuje přehled o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="243db-106">Network Security Group view returns all the configured NSGs and rules that are associated at a NIC and subnet level for a virtual machine providing insight into the configuration.</span></span> <span data-ttu-id="243db-107">Kromě toho pravidla efektivní zabezpečení jsou vráceny pro jednotlivé síťové adaptéry ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="243db-107">In addition, the effective security rules are returned for each of the NICs in a VM.</span></span> <span data-ttu-id="243db-108">Zobrazení pomocí skupiny zabezpečení sítě, můžete vyhodnotit virtuální počítač chyb zabezpečení sítě, jako je například otevřené porty.</span><span class="sxs-lookup"><span data-stu-id="243db-108">Using Network Security Group view, you can assess a VM for network vulnerabilities such as open ports.</span></span> <span data-ttu-id="243db-109">Můžete také ověřit, pokud vaše skupina zabezpečení sítě funguje podle očekávání, na základě [srovnání nakonfigurované a pravidla efektivní zabezpečení](network-watcher-nsg-auditing-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="243db-109">You can also validate if your Network Security Group is working as expected based on a [comparison between the configured and the effective security rules](network-watcher-nsg-auditing-powershell.md).</span></span>

<span data-ttu-id="243db-110">Více rozšířených případ použití se dodržování předpisů pro zabezpečení a auditování.</span><span class="sxs-lookup"><span data-stu-id="243db-110">A more extended use case is in security compliance and auditing.</span></span> <span data-ttu-id="243db-111">Můžete definovat doporučený sadu pravidel zabezpečení jako model pro řízení zabezpečení ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="243db-111">You can define a prescriptive set of security rules as a model for security governance in your organization.</span></span> <span data-ttu-id="243db-112">Audit pravidelné dodržování předpisů, můžou se implementovat v programový způsob tak, že porovnáte doporučený pravidla s efektivní pravidla pro jednotlivé virtuální počítače ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="243db-112">A periodic compliance audit can be implemented in a programmatic way by comparing the prescriptive rules with the effective rules for each of the VMs in your network.</span></span>

<span data-ttu-id="243db-113">Na portálu, které se dál dělí platné, podsítě, síťové a výchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="243db-113">In the portal rules are divided by Effective, Subnet, Network Interface, and Default.</span></span> <span data-ttu-id="243db-114">To poskytuje jednoduché zobrazení do pravidla používaná k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="243db-114">This provides a simple view into the rules applied to a virtual machine.</span></span> <span data-ttu-id="243db-115">Všechna pravidla zabezpečení bez ohledu na to, na kartě snadno stáhnout do souboru CSV je k dispozici tlačítko Stáhnout.</span><span class="sxs-lookup"><span data-stu-id="243db-115">A download button is provided to easily download all the security rules no matter the tab into a CSV file.</span></span>

![zobrazení skupiny zabezpečení][1]

<span data-ttu-id="243db-117">Pravidla lze vybrat a otevře nové okno se na skupinu zabezpečení sítě a zdrojové a cílové předpony.</span><span class="sxs-lookup"><span data-stu-id="243db-117">Rules can be selected and a new blade opens up to show the Network Security Group and source and destination prefixes.</span></span> <span data-ttu-id="243db-118">V tomto okně můžete přejít přímo na prostředek skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="243db-118">From this blade you can navigate directly to the Network Security Group resource.</span></span>

![Přejít k podrobnostem][2]

### <a name="next-steps"></a><span data-ttu-id="243db-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="243db-120">Next steps</span></span>

<span data-ttu-id="243db-121">Zjistěte, jak auditovat nastavení skupinu zabezpečení sítě, navštivte stránky [nastavení auditu skupinu zabezpečení sítě pomocí prostředí PowerShell](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="243db-121">Learn how to audit your Network Security Group settings by visiting [Audit Network Security Group settings with PowerShell](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









