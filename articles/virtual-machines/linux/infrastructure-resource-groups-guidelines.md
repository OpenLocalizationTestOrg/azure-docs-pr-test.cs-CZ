---
title: "Skupiny prostředků pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro nasazení skupin prostředků v služby infrastruktury Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 93ab9d93-965a-46b9-9c87-a10d652a6422
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 452acde571164a3ab4ce2dcccf99d2aed90361fe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-resource-group-guidelines-for-linux-vms"></a>Pokyny pro skupinu prostředků Azure pro virtuální počítače s Linuxem 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na vědět, jak se logicky vytváří prostředí a skupiny všechny součásti ve skupinách prostředků.

## <a name="implementation-guidelines-for-resource-groups"></a>Postup implementace pro skupiny prostředků
Rozhodnutí:

* Chystáte se vytváří skupiny prostředků základních komponent infrastruktury, nebo nasazení aplikace dokončena?
* Potřebujete omezit přístup ke skupinám prostředků pomocí řízení přístupu na základě rolí?

Úlohy:

* Definujte, co základní součásti infrastruktury a vyhrazené skupiny prostředků, je nutné.
* Zkontrolujte, jak implementovat šablony Resource Manageru pro konzistentní a reprodukovatelné nasazení.
* Definování rolí přístup jaké uživatele je nutné pro řízení přístupu ke skupinám prostředků.
* Vytvořte sadu skupin prostředků pomocí zásady vytváření názvů. Můžete použít rozhraní příkazového řádku Azure nebo portálu.

## <a name="resource-groups"></a>Skupiny prostředků
V Azure logicky seskupit související prostředky, například účty úložiště, virtuální sítě a virtuální počítače (VM) nasadit, spravovat a udržovat je jako jedna entita. Tento přístup je jednodušší, k nasazení aplikací a zachovat přitom všechny související prostředky z hlediska správy nebo jiné udělit přístup k této skupině prostředků. Názvy skupin prostředků, může být maximálně 90 znaků. Pro získat komplexnější přehled o skupinách prostředků, si můžete přečíst [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).

Klíčovou funkcí na skupiny prostředků je schopnost vytvářet prostředí pomocí souboru JSON, který deklaruje úložiště, sítě, a výpočetní prostředky. Můžete také definovat vlastní související skripty nebo konfigurace, které platí. Pomocí těchto šablon JSON vytvářet konzistentní a reprodukovatelné nasazení pro vaše aplikace. Tento přístup umožňuje vytvořit prostředí při vývoji a pak použít tento stejné šablony pro vytvoření produkční nasazení, nebo naopak. Lépe porozumět tomu, o pomocí šablon, přečtěte si [názorný Průvodce šablonou](../../azure-resource-manager/resource-manager-template-walkthrough.md) který vás provede každého kroku vytváření šablony JSON.

Existují dva různé přístupy, které můžete provést při návrhu prostředí ke skupinám prostředků:

* Skupiny prostředků pro každé nasazení aplikace, které kombinuje účty úložiště, virtuální sítě a podsítě virtuálních počítačů, načíst vyrovnávání atd.
* Centralizované skupin prostředků, které obsahují vaše základní virtuální sítě a podsítě nebo úložiště účtů. Aplikace se pak ve své vlastní skupiny prostředků, které obsahovat pouze virtuální počítače, nástroje pro vyrovnávání zatížení, síťových rozhraní, atd.

Jak vám horizontální navýšení kapacity, vytváření centralizované skupiny prostředků pro virtuální sítě a podsítě usnadňuje sestavení mezi různými místy síťová připojení pro možnosti hybridní připojení. Alternativní způsob je pro každou aplikaci tak, aby měl své vlastní virtuální síť, která vyžaduje konfiguraci a údržbu. [Řízení přístupu na základě rolí](../../active-directory/role-based-access-control-what-is.md) poskytnout podrobné způsob, jak řídit přístup ke skupinám prostředků. Aplikacích v produkčním prostředí můžete řídit uživatele, kteří mohou přistupovat k tyto prostředky, nebo pro prostředky infrastruktury základní můžete omezit pouze technici infrastruktury pro práci s nimi. Vaše vlastníci aplikace mají jenom přístup k součásti aplikace v rámci jejich skupiny prostředků a ne jádro infrastrukturu Azure vašeho prostředí. Při navrhování prostředí, vezměte v úvahu uživatele, kteří potřebují přístup k prostředkům a odpovídajícím způsobem návrh vaší skupiny prostředků. 

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

