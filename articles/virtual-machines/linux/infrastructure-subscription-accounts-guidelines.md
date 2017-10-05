---
title: "Předplatné a účet pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro předplatných a účtů v Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19343826-7eef-42a1-98be-4ec65b0f377a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 19695a9960d8e8f0dfca4bf0ca10761fe6ae7ff0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a>Azure předplatného a účty pokyny pro virtuální počítače s Linuxem

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se týká vědět, jak se přístup ke správě předplatné a účet jako prostředí a zvětšování uživatelské základny.

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Postup implementace pro předplatných a účtů
Rozhodnutí:

* Jaká sada předplatných a účtů proveďte potřebujete k hostování zatížení IT nebo infrastrukturu?
* Jak rozdělení hierarchii podle vaší organizace?

Úlohy:

* Definujte hierarchii logické organizace, jako byste chtěli spravovat z úrovně předplatného.
* Tak, aby odpovídala této logické hierarchii, zadejte požadované účty a předplatná každý účet.
* Vytvořte sadu předplatných a účtů pomocí zásady vytváření názvů.

## <a name="subscriptions-and-accounts"></a>Předplatná a účty
Chcete-li pracovat s Azure, je třeba jeden nebo víc předplatných Azure. Prostředky, jako virtuální počítače (VM) nebo virtuální sítě existuje v těchto předplatných.

* Podnikoví zákazníci obvykle mají podnikového zápisu, který je nejvyšší prostředků v hierarchii a je přidružen k jedné nebo více účtů.
* Příjemci a zákazníky bez podnikového zápisu je prostředek nejvyšší účet.
* Odběry jsou přidružené k účtům a může být jeden nebo více odběrů na účtu. Fakturační informace na úrovni předplatného Azure záznamy.

Z důvodu limit dvě úrovně hierarchie v relaci účet nebo předplatné je důležité sladit zásady vytváření názvů účtů a odběry fakturace potřebám. Například pokud globální společnost používá Azure, může zvolit tak, aby měl jeden účet na oblast a odběry na spravované úrovně oblast:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Můžete například použít následující strukturou:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Pokud oblast rozhodne mít více než jedno předplatné, které jsou přidružené k určité skupiny, by měly zásady vytváření názvů začlenit způsob, jak kódování doplňující data na účtu nebo název odběru. Tato organizace umožňuje massaging fakturační údaje, které během fakturace sestavy generovat nové úrovně hierarchie:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Organizace může vypadat podobně jako v následujícím příkladu:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Poskytujeme podrobné fakturace prostřednictvím soubor ke stažení pro jeden účet, nebo pro všechny účty v smlouvu enterprise agreement.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

