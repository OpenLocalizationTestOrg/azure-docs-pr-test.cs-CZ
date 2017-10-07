---
title: "Vyrovnávání zatížení aaaCreate směřujících Internetu – portál Azure classic | Microsoft Docs"
description: "Zjistěte, jak toocreate k Internetu přístupných pro vyrovnávání zatížení pomocí modelu nasazení classic hello portál Azure classic"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 27b0d5af6e7b493fa94a9dfbfa260483ae95a2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-classic-portal"></a>Začínáte s vytvářením internetovým Vyrovnávání zatížení (klasické) v hello portál Azure classic

> [!div class="op_single_selector"]
> * [Portál Azure Classic](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model. Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md). Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku. Tento článek se týká modelu nasazení classic hello. Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Nastavení internetového nástroje pro vyrovnávání zatížení pro virtuální počítače

V pořadí tooload vyrovnávání síťové přenosy z Internetu hello napříč hello virtuální počítače cloudové služby je nutné vytvořit sadu s vyrovnáváním zatížení. Tento postup předpokládá, že jste již vytvořili hello virtuálních počítačů a že jsou všechny v rámci hello stejné cloudové služby.

**tooconfigure sadu Vyrovnávání zatížení sítě pro virtuální počítače**

1. V hello portál Azure classic, klikněte na **virtuální počítače**a potom klikněte na název hello virtuálního počítače v sadě hello Vyrovnávání zatížení sítě.
2. Klikněte na **Koncové body** a potom na **Přidat**.
3. Na hello **přidat se koncový bod tooa virtuální počítač** klikněte na šipku vpravo hello.
4. Na hello **zadejte podrobnosti hello koncového bodu hello** stránky:

   * V **název**, zadejte název pro koncový bod hello nebo vyberte název hello ze seznamu předdefinovaných koncové body pro běžné protokoly hello.
   * V **protokol**, vyberte protokol hello vyžadované hello typu koncového bodu, TCP nebo UDP, podle potřeby.
   * V **Port veřejné a privátní Port**, zadejte hello čísla portů, která chcete hello toouse virtuální počítač podle potřeby. Můžete vytvořit privátní port hello a pravidel brány firewall na přenosy dat virtuálních počítačů tooredirect hello způsobem, který je vhodný pro aplikaci. stejné jako veřejný port hello můžete hello Hello privátní port. Například pro koncový bod pro provoz webu (HTTP), může přiřadit port 80 tooboth hello veřejné a privátní port.

5. Vyberte **vytvořit sadu s vyrovnáváním zatížení**a potom klikněte na šipku vpravo hello.
6. Na hello **konfigurace sady vyrovnáváním zatížení hello** stránky, zadejte název pro sadu hello Vyrovnávání zatížení sítě a pak mu přiřaďte hodnoty hello pro chování testu hello Vyrovnávání zatížení Azure. Hello Vyrovnávání zatížení používá sondy toodetermine, pokud jsou k dispozici tooreceive příchozí provoz hello virtuální počítače ve sady hello vyrovnáváním zatížení.
7. Klikněte na tlačítko hello zaškrtnutí toocreate hello Vyrovnávání zatížení sítě endpoint. Zobrazí se **Ano** v hello **název skupiny s vyrovnáváním zatížení** sloupec hello **koncové body** stránku hello virtuálního počítače.
8. Hello portálu, klikněte na tlačítko **virtuální počítače**, klikněte na název hello další virtuálního počítače v sadě hello Vyrovnávání zatížení sítě, klikněte na **koncové body**a potom klikněte na **přidat**.
9. Na hello **přidat se koncový bod tooa virtuální počítač** klikněte na tlačítko **přidat koncový bod tooan existující Vyrovnávání zatížení sítě sady**, vyberte název hello sady hello Vyrovnávání zatížení sítě a potom klikněte na šipku vpravo hello.
10. Na hello **zadejte podrobnosti hello koncového bodu hello** stránky, zadejte název pro koncový bod hello a pak klikněte na tlačítko zaškrtnutí hello.

Pro hello dalších virtuálních počítačů v sadě hello Vyrovnávání zatížení sítě opakujte kroky 8-10.

## <a name="next-steps"></a>Další kroky

[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
