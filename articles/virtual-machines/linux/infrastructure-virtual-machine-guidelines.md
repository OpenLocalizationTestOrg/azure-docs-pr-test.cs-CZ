---
title: "aaaAzure pokyny pro virtuální počítače Linux | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro nasazení virtuálních počítačů Linux do Azure"
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
ms.openlocfilehash: 0f779f791005441b6b16e106a3dc5a095bb33034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-guidelines-for-linux"></a>Virtuální počítače Azure pokyny pro Linux
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení hello požadované plánování kroky pro vytváření a správu virtuálních počítačů (VM) v prostředí Azure.

## <a name="implementation-guidelines-for-vms"></a>Postup implementace pro virtuální počítače
Rozhodnutí:

* Kolik virtuálních počítačů můžete vyžadují různé úrovně aplikace a součásti infrastruktury?
* Jaké prostředky procesoru a paměti každý virtuální počítač potřebuje, a jaké jsou požadavky na úložiště hello?

Úlohy:

* Definujte hello pracovních zátěží pro vaší aplikace a hello hello prostředky, které virtuální počítače vyžadují.
* Zarovnává hello požadavků na prostředky pro každý virtuální počítač s hello odpovídající typ virtuálního počítače velikost a úložiště.
* Definování skupin prostředků pro hello různých vrstev a součástí infrastruktury.
* Definujte vaše zásady vytváření názvů virtuálních počítačů.
* Vytvořte virtuální počítače pomocí hello rozhraní příkazového řádku Azure, webový portál, nebo pomocí šablony Resource Manageru.

## <a name="virtual-machines"></a>Virtuální počítače
Jedním z hello hlavní prostředků v prostředí Azure je pravděpodobně virtuálních počítačů. Tento prostředek je místo, kde spouštíte vaší aplikace, databáze, ověřovací služby, atd.

Je důležité toounderstand hello [různé velikosti virtuálních počítačů](sizes.md) toocorrectly velikost vašeho prostředí z hlediska výkonu a nákladů. Pokud vaše virtuální počítače nemají dostatek jader procesoru nebo paměti, bez ohledu na to, jak dobře je navržena a vyvinuté ke snížení výkonu aplikace. Zkontrolujte hello navrhované úlohy u každé série virtuálních počítačů jako výchozí bod, jako je rozhodnout, které toouse velikost virtuálního počítače pro jednotlivé komponenty v infrastruktuře. Můžete [změnit hello velikost virtuálního počítače](change-vm-size.md) po nasazení.

Úložiště hraje klíčovou roli ve výkonu virtuálních počítačů. Můžete použít standardní úložiště, který používá regulární roztočený disky, nebo storage úrovně Premium pro vysoké vstupně-výstupních úloh a výkon ve špičce, které používají disky SSD. Jako s hello velikost virtuálního počítače existuje jsou náklady aspekty tooselecting hello paměťového média. Můžete si přečíst hello [úložiště infrastruktury pokyny článku](infrastructure-storage-solutions-guidelines.md) toounderstand jak toodesign příslušné úložiště pro optimální výkon virtuálních počítačů.

## <a name="resource-groups"></a>Skupiny prostředků
Komponenty, například virtuální počítače jsou logicky seskupeny pro snadné správy a údržby pomocí [skupiny prostředků Azure](../../azure-resource-manager/resource-group-overview.md). Pomocí skupin prostředků, můžete vytvořit, spravovat a monitorovat všechny hello prostředky, které tvoří k dané aplikaci. Můžete taky implementovat [řízení přístupu na základě role](../../active-directory/role-based-access-control-what-is.md) toogrant tooothers přístup v rámci vašich prostředků hello team tooonly vyžadují. Trvat tooplan čas skupiny prostředků a přiřazení rolí. Existují různé přístupy tooactually návrhu a implementace skupiny prostředků, tak buďte hello zda tooread [článek pokyny skupiny prostředků](infrastructure-resource-groups-guidelines.md) toounderstand jak nejlepší toobuild limitu virtuálních počítačů.

## <a name="templates"></a>Šablony
Můžete vytvořit šablony, které jsou definované deklarativní soubory JSON, toocreate virtuální počítače. Šablony obvykle také vytvořit hello vyžaduje úložiště, sítě, síťové rozhraní, IP adresy, atd. společně s vlastních virtuálních počítačů hello. Šablony toocreate konzistentní a reprodukovatelné prostředí můžete použít pro vývoj a testování tooeasily účely replikovat produkčních prostředí a naopak. Další informace o [vytváření a používání šablon](../../azure-resource-manager/resource-group-overview.md#template-deployment) toounderstand, jak je můžete použít pro vytvoření a nasazení virtuálních počítačů.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

