---
title: "Přehled nástroje pro vyrovnávání zatížení Internetové | Microsoft Docs"
description: "Přehled pro internetový bod nástroj pro vyrovnávání zatížení a jejich funkce. Jak funguje nástroj pro vyrovnávání zatížení pro Azure pomocí virtuální počítače a cloudové služby."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 5b9ffeadf6b1ffc4eaf4f49b85ba752c27da0e46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="internet-facing-load-balancer-overview"></a>Přehled nástroje pro vyrovnávání zatížení přístupných Internetu

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Nástroje pro vyrovnávání zatížení Azure mapuje veřejnou IP adresu a port číslo příchozí provoz privátní IP adresu a číslo portu virtuálního počítače a naopak přenosům odpověď z virtuálního počítače. Pravidla Vyrovnávání zatížení umožňují distribuovat specifické typy přenosů mezi několik virtuálních počítačů nebo služeb. Můžete například šíří zatížení webový požadavek provoz napříč více webové servery nebo webové role.

Pro cloudovou službu, která obsahuje instance webové role nebo rolí pracovního procesu můžete definovat veřejný koncový bod v souboru definice (.csdef) služby.

*Servicedefinition.csdef* soubor obsahuje konfiguraci koncového bodu a až budete mít více instancí role pro nasazení role web nebo worker, instalační program pro něj bude nástroj pro vyrovnávání zatížení. Počet instancí v konfiguračním souboru služby (.csfg) mění způsob přidání instancí do cloudu nasazení.

## <a name="example-of-an-internet-facing-load-balancer"></a>Příklad internetovým nástroj pro vyrovnávání zatížení

Následující obrázek znázorňuje koncový bod Vyrovnávání zatížení sítě pro webový provoz, která je sdílena mezi tři virtuální počítače pro veřejné a privátní port TCP 80. Tyto tři virtuální počítače jsou v sadě s vyrovnáváním zatížení.

![Příklad nástroje pro vyrovnávání zatížení veřejnou](./media/load-balancer-internet-overview/IC727496.png)

Obrázek 1 – endpoint pro webový provoz s vyrovnáváním zatížení

Pokud internetoví klienti odesílat žádosti o webovou stránku na veřejnou IP adresu cloudové služby na portu TCP 80, Vyrovnávání zatížení Azure distribuuje požadavky mezi tři virtuální počítače v sadě s vyrovnáváním zatížení. Další informace o algoritmy Vyrovnávání zatížení, najdete v článku [stránku přehled nástroje pro vyrovnávání zatížení](load-balancer-overview.md#load-balancer-features).

Ve výchozím nastavení nástroj pro vyrovnávání zatížení Azure distribuuje síťový provoz mezi více instancí virtuálního počítače. Můžete také nakonfigurovat spřažení relace, další informace najdete v tématu [režim distribuce nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Další kroky

Další informace o [nástroj pro vyrovnávání zatížení pro vnitřní](load-balancer-internal-overview.md) abyste lépe pochopili, které nástroj pro vyrovnávání zatížení je lepší přizpůsobení pro vaše nasazení cloudu.

Můžete také [začínáte s vytvářením internetovým nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace, jaký druh [režim distribuce](load-balancer-distribution-mode.md) konkrétní zatížení vyrovnávání síťové přenosy chování.

Pokud vaše aplikace potřebuje udržovat aktivní připojení serverů, které jsou za nástrojem pro vyrovnávání zatížení, můžete zjistit více o [nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md). Pomůže vám to pochopit chování nečinného připojení při používání služby Azure Load Balancer.
