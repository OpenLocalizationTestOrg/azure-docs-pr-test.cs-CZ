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
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Představení skupiny dostupnosti SQL serveru Always On na virtuálních počítačích Azure #

Tento článek představuje skupiny dostupnosti systému SQL Server na virtuálních počítačích Azure. 

Always On skupiny dostupnosti ve virtuálních počítačích Azure jsou podobné tooAlways o skupinách dostupnosti místně. Další informace najdete v tématu [skupin dostupnosti Always On (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx). 

Hello diagram znázorňuje hello částí celé skupiny dostupnosti SQL Server v Azure Virtual Machines.

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

Hello klíčovým rozdílem pro skupiny dostupnosti v Azure Virtual Machines je, který hello virtuální počítače Azure, vyžadují [nástroj pro vyrovnávání zatížení](../../../load-balancer/load-balancer-overview.md). Nástroj pro vyrovnávání zatížení Hello obsahuje hello IP adresy pro naslouchací proces skupiny dostupnosti hello. Pokud máte více než jedna skupina dostupnosti vyžaduje každá skupina naslouchací proces. Jedna služba Vyrovnávání zatížení může podporovat více naslouchací procesy.

Pokud jste připravené toobuild aroup dostupnosti systému SQL Server na virtuálních počítačích Azure, naleznete v nápovědě toothese kurzy.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Automaticky vytvořit skupinu dostupnosti ze šablony

[Konfigurace skupiny dostupnosti Always On ve virtuálním počítači Azure automaticky - Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Ručně vytvořit skupinu dostupnosti na portálu Azure

Můžete také vytvořit virtuální počítače hello sami bez hello šablony. Nejprve dokončete hello požadavky a potom vytvořit skupinu dostupnosti hello. V tématu hello následující témata: 

- [Konfigurace požadavků pro skupiny dostupnosti SQL serveru Always On na virtuálních počítačích Azure](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Vytvoření vždy na skupiny dostupnosti tooimprove dostupnosti a zotavení po havárii](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Další kroky

[Konfigurace systému SQL Server vždy na skupiny dostupnosti na virtuálních počítačích, které jsou v různých oblastech Azure](virtual-machines-windows-portal-sql-availability-group-dr.md).
