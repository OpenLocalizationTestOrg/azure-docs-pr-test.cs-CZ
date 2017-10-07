---
title: "aaaSubscription a účty pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pokyny pro předplatných a účtů v Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 761fa847-78b0-4078-a33a-d95d198d1029
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f9dc712af559b04490be1dc721a9b9f7fe9ed88f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-windows-vms"></a>Azure předplatného a účty pokyny pro virtuální počítače Windows

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

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

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Můžete například použít hello strukturu:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Pokud se v oblasti rozhodne toohave více než jedno předplatné přidružené tooa konkrétní skupiny, zásady vytváření názvů hello by měla zahrnovat tooencode způsob hello doplňující data na účtu hello nebo název odběru hello. Tato organizace umožňuje massaging fakturační data toogenerate hello nové úrovně hierarchie během fakturace sestavy:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Hello organizace může vypadat hello následující ukázka:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Poskytujeme podrobné fakturace prostřednictvím soubor ke stažení pro jeden účet, nebo pro všechny účty v smlouvu enterprise agreement.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

