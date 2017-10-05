---
title: "Skupiny dostupnosti SQL serveru – virtuální počítače Azure – přehled | Microsoft Docs"
description: "Tento článek představuje skupin dostupnosti SQL Server na virtuálních počítačích Azure."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: 2cbb9ff3b2d13996b1b8dc64aa833807c264c877
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a><span data-ttu-id="95945-103">Představení skupiny dostupnosti SQL serveru Always On na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="95945-103">Introducing SQL Server Always On availability groups on Azure virtual machines</span></span> #

<span data-ttu-id="95945-104">Tento článek představuje skupiny dostupnosti systému SQL Server na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="95945-104">This article introduces SQL Server availability groups on Azure Virtual Machines.</span></span> 

<span data-ttu-id="95945-105">Always On skupin dostupnosti ve virtuálních počítačích Azure se podobá Always On skupiny dostupnosti místně.</span><span class="sxs-lookup"><span data-stu-id="95945-105">Always On availability groups on Azure Virtual Machines are similar to Always On availability groups on premises.</span></span> <span data-ttu-id="95945-106">Další informace najdete v tématu [skupin dostupnosti Always On (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="95945-106">For more information, see [Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span></span> 

<span data-ttu-id="95945-107">Diagram znázorňuje části celé skupiny dostupnosti SQL Server v Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="95945-107">The diagram illustrates the parts of a complete SQL Server Availability Group in Azure Virtual Machines.</span></span>

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

<span data-ttu-id="95945-109">Klíčovým rozdílem pro skupinu dostupnosti v Azure Virtual Machines je virtuální počítače Azure, vyžadují, aby se [nástroj pro vyrovnávání zatížení](../../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="95945-109">The key difference for an Availability Group in Azure Virtual Machines is that the Azure virtual machines, require a [load balancer](../../../load-balancer/load-balancer-overview.md).</span></span> <span data-ttu-id="95945-110">Nástroje pro vyrovnávání zatížení obsahuje IP adresy pro naslouchací proces skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="95945-110">The load balancer holds the IP addresses for the availability group listener.</span></span> <span data-ttu-id="95945-111">Pokud máte více než jedna skupina dostupnosti vyžaduje každá skupina naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="95945-111">If you have more than one availability group each group requires a listener.</span></span> <span data-ttu-id="95945-112">Jedna služba Vyrovnávání zatížení může podporovat více naslouchací procesy.</span><span class="sxs-lookup"><span data-stu-id="95945-112">One load balancer can support multiple listeners.</span></span>

<span data-ttu-id="95945-113">Jakmile budete připraveni k sestavení aroup dostupnosti systému SQL Server na virtuálních počítačích Azure, naleznete v těchto kurzech.</span><span class="sxs-lookup"><span data-stu-id="95945-113">When you are ready to build a SQL Server availability aroup on Azure Virtual Machines, refer to these tutorials.</span></span>

## <a name="automatically-create-an-availability-group-from-a-template"></a><span data-ttu-id="95945-114">Automaticky vytvořit skupinu dostupnosti ze šablony</span><span class="sxs-lookup"><span data-stu-id="95945-114">Automatically create an availability group from a template</span></span>

[<span data-ttu-id="95945-115">Konfigurace skupiny dostupnosti Always On ve virtuálním počítači Azure automaticky - Resource Manager</span><span class="sxs-lookup"><span data-stu-id="95945-115">Configure Always On availability group in Azure VM automatically - Resource Manager</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a><span data-ttu-id="95945-116">Ručně vytvořit skupinu dostupnosti na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="95945-116">Manually create an availability group in Azure portal</span></span>

<span data-ttu-id="95945-117">Můžete také vytvořit virtuální počítače sami bez šablony.</span><span class="sxs-lookup"><span data-stu-id="95945-117">You can also create the virtual machines yourself without the template.</span></span> <span data-ttu-id="95945-118">Nejprve splnit požadavky a potom vytvořit skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="95945-118">First, complete the prerequisites, then create the availability group.</span></span> <span data-ttu-id="95945-119">Najdete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="95945-119">See the following topics:</span></span> 

- [<span data-ttu-id="95945-120">Konfigurace požadavků pro skupiny dostupnosti SQL serveru Always On na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="95945-120">Configure prerequisites for SQL Server Always On availability groups on Azure Virtual Machines</span></span>](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [<span data-ttu-id="95945-121">Vytvoření vždy na skupiny dostupnosti ke zlepšení dostupnosti a zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="95945-121">Create Always On Availability Group to improve availability and disaster recovery</span></span>](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a><span data-ttu-id="95945-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95945-122">Next steps</span></span>

<span data-ttu-id="95945-123">[Konfigurace systému SQL Server vždy na skupiny dostupnosti na virtuálních počítačích, které jsou v různých oblastech Azure](virtual-machines-windows-portal-sql-availability-group-dr.md).</span><span class="sxs-lookup"><span data-stu-id="95945-123">[Configure a SQL Server Always On Availability Group on Azure Virtual Machines in Different Regions](virtual-machines-windows-portal-sql-availability-group-dr.md).</span></span>
