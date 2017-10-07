---
title: "pomocí Azure Traffic Manager metodu směrování provozu aaaConfigure výkonu | Microsoft Docs"
description: "Tento článek vysvětluje, jak tooconfigure Traffic Manager tooroute provozu toohello koncový bod s nejnižší latencí"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: d0ccd4c9de411844c6f36068859265244f4aa52b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-performance-traffic-routing-method"></a>Konfigurace metody směrování provozu hello výkonu

Hello výkonu metodu směrování provozu umožňuje toodirect provoz toohello koncový bod s nejnižší latencí hello z hello klienta sítě. Hello datacenter s nejnižší latencí hello je obvykle hello nejbližší v zeměpisné vzdálenosti. Tuto metodu směrování provozu nelze účet v reálném čase změny v konfiguraci sítě nebo zatížení.

##  <a name="tooconfigure-performance-routing-method"></a>metody směrování podle výkonu tooconfigure

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com). Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/). 
2. V panelu vyhledávání hello portál, vyhledejte hello **profily Traffic Manageru** a pak klikněte na název profilu hello, který chcete tooconfigure hello metody směrování pro.
3. V hello **profil služby Traffic Manager** okno, ověřte, že oba hello cloudové služby a nejsou weby, že chcete tooinclude ve vaší konfiguraci.
4. V hello **nastavení** klikněte na tlačítko **konfigurace**a v hello **konfigurace** okně dokončení následujícím způsobem:
    1. Pro **nastavení metodu směrování provozu**, pro **metoda směrování** vyberte **výkonu**.
    2. Sada hello **nastavení monitorování koncového bodu** stejné pro všechny každý koncový bod v tomto profilu následujícím způsobem:
        1. Vyberte odpovídající hello **protokol**a zadejte hello **Port** číslo. 
        2. Pro **cesta** zadejte lomítkem  */* . toomonitor koncových bodů, musíte zadat cestu a název souboru. A dopředné lomítko "/" je platná položka pro hello relativní cestu a znamená, že soubor hello je v kořenovém adresáři hello (výchozí).
        3. V horní části hello hello stránky, klikněte na tlačítko **Uložit**.
5.  Testovací hello změny v konfiguraci následujícím způsobem:
    1.  V panelu vyhledávání hello portál vyhledejte název profilu Traffic Manageru hello a klikněte na profil služby Traffic Manager hello ve výsledcích hello této hello zobrazí.
    2.  V hello **Traffic Manager** profilu okno, klikněte na tlačítko **přehled**.
    3.  Hello **profil služby Traffic Manager** zobrazuje název DNS hello vaše nově vytvořený profil služby Traffic Manager. Toho můžete využít všechny klienty (například přechodem tooit pomocí webového prohlížeče) tooget směrovány toohello pravý koncový bod určeného typ směrování hello. V tomto případě jsou všechny žádosti směrované toohello koncový bod s nejnižší latenci hello z hello klienta sítě.
6. Po váš profil služby Traffic Manager funguje, upravte záznam DNS hello na vaše autoritativní server toopoint DNS domény název toohello Traffic Manager název domény vaší společnosti.

![Konfigurace metody směrování výkonu provozu pomocí Traffic Manager][1]

## <a name="next-steps"></a>Další kroky

- Další informace o [vážené metodu směrování provozu](traffic-manager-configure-weighted-routing-method.md).
- Další informace o [metody směrování s prioritou](traffic-manager-configure-priority-routing-method.md).
- Další informace o [geografické metody směrování](traffic-manager-configure-geographic-routing-method.md).
- Zjistěte, jak příliš[Traffic Manager nastavení testů](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png