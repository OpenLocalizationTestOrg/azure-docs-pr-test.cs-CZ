---
title: "Ověření virtuální síť Azure pro použití s Azure Remoteappem | Microsoft Docs"
description: "Naučte se ujistěte se, že virtuální sítě Azure je připravené k použití s Azure Remoteappem"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b573ba02-4587-4be5-9821-27bd891a73b2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 05c8a0ff04293947cec391b6467cc4adddb6a7b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Ověření virtuální síť Azure pro použití s Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Než použijete o virtuální síť Azure s Azure Remoteappem, můžete ověřit virtuální sítě. To pomáhá zabránit problémům s připojením.

Ověření virtuální sítě Azure, postupujte takto:

1. Vytvořte virtuální počítač Azure uvnitř podsíť virtuální sítě Azure, kterou chcete použít s Azure Remoteappem.
2. Připojit k tohoto virtuálního počítače pomocí **Connect** možnost v portálu pro správu.
3. Připojení virtuálního počítače do stejné domény, který chcete použít s Azure Remoteappem. Pokud vytváříte hybridní kolekci, která se připojuje k vaší místní síti, připojení k místní doméně virtuálního počítače.

Pokud budou úspěšní, virtuální síť Azure je připravené k použití s Remoteappem.

Další informace o pracovním postupu začátku do konce hybridní kolekci najdete v následujících článcích:

* [Postup plánování virtuální sítě Azure RemoteApp](remoteapp-planvnet.md)
* [Vytvoření hybridní kolekce](remoteapp-create-hybrid-deployment.md)
* [Nasaďte kolekci Azure Remoteappu k virtuální síti Azure (s podporou pro ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)

