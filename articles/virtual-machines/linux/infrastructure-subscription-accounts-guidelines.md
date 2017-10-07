---
title: "aaaSubscription a účet pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro předplatných a účtů v Azure."
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
ms.openlocfilehash: 9025a40783c008310ebd0f674deb4a9001ae974a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a>Azure předplatného a účty pokyny pro virtuální počítače s Linuxem

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení, jak se zvětšuje tooapproach předplatné a účet správy jako vaše prostředí a uživatelské základny.

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Postup implementace pro předplatných a účtů
Rozhodnutí:

* Jaká sada z předplatných a účtů potřebujete toohost IT zatížení nebo infrastrukturu?
* Jak toobreak dolů hello hierarchie toofit organizaci?

Úlohy:

* Definujte hierarchii logické organizace, jako byste chtěli toomanage z úrovně předplatného.
* toomatch tuto logické hierarchii, zadejte hello účty požadované a předplatná každý účet.
* Vytvořte sadu hello předplatných a účtů pomocí zásady vytváření názvů.

## <a name="subscriptions-and-accounts"></a>Předplatná a účty
toowork s Azure, budete potřebovat jeden nebo víc předplatných Azure. Prostředky, jako virtuální počítače (VM) nebo virtuální sítě existuje v těchto předplatných.

* Podnikoví zákazníci obvykle mají podnikového zápisu, který je hello nejvyšší prostředků v hierarchii hello a přidružené tooone nebo další účty.
* Příjemci a zákazníky bez podnikového zápisu hello nejvyšší prostředek je účet hello.
* Odběry jsou přidružené tooaccounts a může být jeden nebo více odběrů na účtu. Fakturační informace na úrovni předplatného hello Azure záznamy.

Z důvodu omezení toohello úrovní dvě hierarchie v relaci hello účet nebo předplatné je důležité tooalign hello zásady vytváření názvů účtům a předplatným toohello fakturace potřebám. Například pokud globální společnost používá Azure, mohou si vyberou toohave jeden účet na oblast a odběry na spravované hello úrovně oblast:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Můžete například použít hello strukturu:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Pokud se v oblasti rozhodne toohave více než jedno předplatné přidružené tooa konkrétní skupiny, zásady vytváření názvů hello by měla zahrnovat tooencode způsob hello doplňující data na účtu hello nebo název odběru hello. Tato organizace umožňuje massaging fakturační data toogenerate hello nové úrovně hierarchie během fakturace sestavy:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Hello organizace může vypadat hello následující ukázka:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Poskytujeme podrobné fakturace prostřednictvím soubor ke stažení pro jeden účet, nebo pro všechny účty v smlouvu enterprise agreement.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

