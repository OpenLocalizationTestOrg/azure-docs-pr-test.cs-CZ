---
title: "Pokyny pro virtuální počítače Azure Linux | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny k nasazení virtuálních počítačů Linux do Azure"
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 740767d7-9a40-407b-93e7-c29dd976ffd7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.openlocfilehash: 2d886795b301ab79272cae10a53831fd44c18f10
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-virtual-machines-guidelines-for-linux"></a>Virtuální počítače Azure pokyny pro Linux
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení požadované kroky plánování pro vytváření a správu virtuálních počítačů (VM) v prostředí Azure.

## <a name="implementation-guidelines-for-vms"></a>Postup implementace pro virtuální počítače
Rozhodnutí:

* Kolik virtuálních počítačů můžete vyžadují různé úrovně aplikace a součásti infrastruktury?
* Jaké prostředky procesoru a paměti každý virtuální počítač potřebuje, a jaké jsou požadavky na úložiště?

Úlohy:

* Definování pracovních zátěží pro vaše aplikace a prostředky, které vyžadují virtuálních počítačů.
* Zarovnává požadavky prostředků pro každý virtuální počítač s příslušným typem velikosti a úložiště virtuálních počítačů.
* Definování skupin prostředků pro různé úrovně a součástí infrastruktury.
* Definujte vaše zásady vytváření názvů virtuálních počítačů.
* Vytvořte virtuální počítače pomocí rozhraní příkazového řádku Azure, webový portál, nebo pomocí šablony Resource Manageru.

## <a name="virtual-machines"></a>Virtuální počítače
Jedním z hlavních prostředků v rámci prostředí Azure je pravděpodobně virtuálních počítačů. Tento prostředek je místo, kde spouštíte vaší aplikace, databáze, ověřovací služby, atd.

Je důležité si uvědomit, [různé velikosti virtuálních počítačů](sizes.md) k správně velikost vašeho prostředí z hlediska výkonu a nákladů. Pokud vaše virtuální počítače nemají dostatek jader procesoru nebo paměti, bez ohledu na to, jak dobře je navržena a vyvinuté ke snížení výkonu aplikace. Zkontrolujte navrhované úlohy u každé série virtuálních počítačů jako výchozí bod jako rozhodnete velikost, ve které chcete použít pro jednotlivé komponenty v infrastruktuře virtuální počítač. Můžete [změňte velikost virtuálního počítače](change-vm-size.md) po nasazení.

Úložiště hraje klíčovou roli ve výkonu virtuálních počítačů. Můžete použít standardní úložiště, který používá regulární roztočený disky, nebo storage úrovně Premium pro vysoké vstupně-výstupních úloh a výkon ve špičce, které používají disky SSD. Jako s velikost virtuálního počítače existuje jsou náklady aspekty při výběru paměťového média. Můžete si přečíst [úložiště infrastruktury pokyny článku](infrastructure-storage-solutions-guidelines.md) vám pomůže porozumět procesu návrhu příslušné úložiště pro optimální výkon virtuálních počítačů.

## <a name="resource-groups"></a>Skupiny prostředků
Komponenty, například virtuální počítače jsou logicky seskupeny pro snadné správy a údržby pomocí [skupiny prostředků Azure](../../azure-resource-manager/resource-group-overview.md). Pomocí skupin prostředků, můžete vytvořit, spravovat a monitorovat všechny prostředky, které tvoří k dané aplikaci. Můžete taky implementovat [řízení přístupu na základě role](../../active-directory/role-based-access-control-what-is.md) udělit přístup ostatním uživatelům v rámci týmu pouze na prostředky, které vyžadují. Časově naplánování vaší skupiny prostředků a přiřazení rolí. Existují různé přístupy k ve skutečnosti návrh a implementaci skupin prostředků, takže nezapomeňte si přečíst [článek pokyny skupiny prostředků](infrastructure-resource-groups-guidelines.md) pochopit, jak nejlépe k sestavení se virtuální počítače.

## <a name="templates"></a>Šablony
Můžete vytvořit šablony, které jsou definované deklarativní soubory JSON, chcete-li vytvořit virtuální počítače. Šablony obvykle také vytvořit požadované úložiště, sítě, síťová rozhraní, IP adresy, atd. společně s vlastních virtuálních počítačů. Šablony můžete použít k vytvoření konzistentní a reprodukovatelné prostředí pro vývoj a testování účely snadno replikovat produkčních prostředí a naopak. Další informace o [vytváření a používání šablon](../../azure-resource-manager/resource-group-overview.md#template-deployment) pochopit, jak je můžete použít pro vytvoření a nasazení virtuálních počítačů.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

