---
title: "aaaInternet čelí přehled nástroje pro vyrovnávání zatížení | Microsoft Docs"
description: "Přehled pro internetový bod nástroj pro vyrovnávání zatížení a jejich funkce. Jak funguje nástroj pro vyrovnávání zatížení pro Azure pomocí virtuální počítače a cloudové služby."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3514f945d69ec576ed256cdd01069491e3e43936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="internet-facing-load-balancer-overview"></a>Přehled nástroje pro vyrovnávání zatížení přístupných Internetu

Nástroje pro vyrovnávání zatížení Azure mapuje hello veřejnou IP adresu a číslo portu příchozí provoz toohello privátní IP adresu a číslo portu hello virtuálního počítače a naopak přenosům hello odpověď z hello virtuálního počítače. Pravidla Vyrovnávání zatížení povolit toodistribute specifické typy přenosů mezi několik virtuálních počítačů nebo služeb. Můžete například šíří hello zátěž webové žádosti o provozu mezi více webové servery nebo webové role.

Pro cloudovou službu, která obsahuje instance webové role nebo rolí pracovního procesu můžete definovat veřejný koncový bod v souboru definice (.csdef) služby hello.

Hello *servicedefinition.csdef* soubor obsahuje hello konfigurace koncového bodu a až budete mít více instancí role pro nasazení role web nebo worker, bude pro vyrovnávání zatížení hello nastavená. nasazení cloudu Hello způsob tooadd instance tooyour mění hello počet instancí v konfiguračním souboru služby hello (.csfg).

Hello následující obrázek znázorňuje koncový bod Vyrovnávání zatížení sítě pro webový provoz, která je sdílena mezi tři virtuální počítače pro hello veřejné a privátní port TCP 80. Tyto tři virtuální počítače jsou v sadě s vyrovnáváním zatížení.

![Příklad nástroje pro vyrovnávání zatížení veřejnou](./media/load-balancer-internet-overview/IC727496.png)

Obrázek 1 – endpoint pro webový provoz s vyrovnáváním zatížení

Pokud internetoví klienti odesílat žádosti o webovou stránku toohello veřejnou IP adresu hello cloudové služby na portu TCP 80, distribuuje hello Vyrovnávání zatížení Azure hello požadavky mezi hello tři virtuální počítače v sadě hello Vyrovnávání zatížení sítě. Další informace o algoritmy Vyrovnávání zatížení, najdete v části hello [stránku přehled nástroje pro vyrovnávání zatížení](load-balancer-overview.md#load-balancer-features).

Ve výchozím nastavení nástroj pro vyrovnávání zatížení Azure distribuuje síťový provoz mezi více instancí virtuálního počítače. Můžete také nakonfigurovat spřažení relace, další informace najdete v tématu [režim distribuce nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Další kroky

Další informace o [nástroj pro vyrovnávání zatížení pro vnitřní](load-balancer-internal-overview.md) toobetter pochopit, jaké nástroj pro vyrovnávání zatížení je lepší přizpůsobení pro vaše nasazení cloudu.

Můžete také [začínáte s vytvářením internetovým nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md) a konfigurace, jaký druh [režim distribuce](load-balancer-distribution-mode.md) konkrétní zatížení vyrovnávání síťové přenosy chování.

Pokud aplikace potřebuje tookeep připojení zachování připojení pro servery za službou Vyrovnávání zatížení, můžete lépe porozumět o [nečinnosti nastavení časového limitu TCP pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md). Pokud používáte nástroj pro vyrovnávání zatížení Azure ho pomůže toolearn o chování nečinné připojení.
