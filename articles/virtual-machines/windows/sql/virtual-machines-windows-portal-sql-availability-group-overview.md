---
title: "aaaSQL skupin dostupnosti serveru – virtuální počítače Azure – přehled | Microsoft Docs"
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
ms.openlocfilehash: ecac8b8c5073021af2aa22a05490bb8c4c20ed17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a><span data-ttu-id="4e96d-103">Představení skupiny dostupnosti SQL serveru Always On na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="4e96d-103">Introducing SQL Server Always On availability groups on Azure virtual machines</span></span> #

<span data-ttu-id="4e96d-104">Tento článek představuje skupiny dostupnosti systému SQL Server na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="4e96d-104">This article introduces SQL Server availability groups on Azure Virtual Machines.</span></span> 

<span data-ttu-id="4e96d-105">Always On skupiny dostupnosti ve virtuálních počítačích Azure jsou podobné tooAlways o skupinách dostupnosti místně.</span><span class="sxs-lookup"><span data-stu-id="4e96d-105">Always On availability groups on Azure Virtual Machines are similar tooAlways On availability groups on premises.</span></span> <span data-ttu-id="4e96d-106">Další informace najdete v tématu [skupin dostupnosti Always On (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e96d-106">For more information, see [Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span></span> 

<span data-ttu-id="4e96d-107">Hello diagram znázorňuje hello částí celé skupiny dostupnosti SQL Server v Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="4e96d-107">hello diagram illustrates hello parts of a complete SQL Server Availability Group in Azure Virtual Machines.</span></span>

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

<span data-ttu-id="4e96d-109">Hello klíčovým rozdílem pro skupiny dostupnosti v Azure Virtual Machines je, který hello virtuální počítače Azure, vyžadují [nástroj pro vyrovnávání zatížení](../../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4e96d-109">hello key difference for an Availability Group in Azure Virtual Machines is that hello Azure virtual machines, require a [load balancer](../../../load-balancer/load-balancer-overview.md).</span></span> <span data-ttu-id="4e96d-110">Nástroj pro vyrovnávání zatížení Hello obsahuje hello IP adresy pro naslouchací proces skupiny dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="4e96d-110">hello load balancer holds hello IP addresses for hello availability group listener.</span></span> <span data-ttu-id="4e96d-111">Pokud máte více než jedna skupina dostupnosti vyžaduje každá skupina naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="4e96d-111">If you have more than one availability group each group requires a listener.</span></span> <span data-ttu-id="4e96d-112">Jedna služba Vyrovnávání zatížení může podporovat více naslouchací procesy.</span><span class="sxs-lookup"><span data-stu-id="4e96d-112">One load balancer can support multiple listeners.</span></span>

<span data-ttu-id="4e96d-113">Pokud jste připravené toobuild aroup dostupnosti systému SQL Server na virtuálních počítačích Azure, naleznete v nápovědě toothese kurzy.</span><span class="sxs-lookup"><span data-stu-id="4e96d-113">When you are ready toobuild a SQL Server availability aroup on Azure Virtual Machines, refer toothese tutorials.</span></span>

## <a name="automatically-create-an-availability-group-from-a-template"></a><span data-ttu-id="4e96d-114">Automaticky vytvořit skupinu dostupnosti ze šablony</span><span class="sxs-lookup"><span data-stu-id="4e96d-114">Automatically create an availability group from a template</span></span>

[<span data-ttu-id="4e96d-115">Konfigurace skupiny dostupnosti Always On ve virtuálním počítači Azure automaticky - Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4e96d-115">Configure Always On availability group in Azure VM automatically - Resource Manager</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a><span data-ttu-id="4e96d-116">Ručně vytvořit skupinu dostupnosti na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4e96d-116">Manually create an availability group in Azure portal</span></span>

<span data-ttu-id="4e96d-117">Můžete také vytvořit virtuální počítače hello sami bez hello šablony.</span><span class="sxs-lookup"><span data-stu-id="4e96d-117">You can also create hello virtual machines yourself without hello template.</span></span> <span data-ttu-id="4e96d-118">Nejprve dokončete hello požadavky a potom vytvořit skupinu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="4e96d-118">First, complete hello prerequisites, then create hello availability group.</span></span> <span data-ttu-id="4e96d-119">V tématu hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="4e96d-119">See hello following topics:</span></span> 

- [<span data-ttu-id="4e96d-120">Konfigurace požadavků pro skupiny dostupnosti SQL serveru Always On na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="4e96d-120">Configure prerequisites for SQL Server Always On availability groups on Azure Virtual Machines</span></span>](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [<span data-ttu-id="4e96d-121">Vytvoření vždy na skupiny dostupnosti tooimprove dostupnosti a zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="4e96d-121">Create Always On Availability Group tooimprove availability and disaster recovery</span></span>](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a><span data-ttu-id="4e96d-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e96d-122">Next steps</span></span>

<span data-ttu-id="4e96d-123">[Konfigurace systému SQL Server vždy na skupiny dostupnosti na virtuálních počítačích, které jsou v různých oblastech Azure](virtual-machines-windows-portal-sql-availability-group-dr.md).</span><span class="sxs-lookup"><span data-stu-id="4e96d-123">[Configure a SQL Server Always On Availability Group on Azure Virtual Machines in Different Regions](virtual-machines-windows-portal-sql-availability-group-dr.md).</span></span>
