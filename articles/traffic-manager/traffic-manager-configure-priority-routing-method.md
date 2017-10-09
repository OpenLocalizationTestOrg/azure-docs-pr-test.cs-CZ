---
title: "aaaConfigure priority provozu pomocí Azure Traffic Manager metody směrování | Microsoft Docs"
description: "Tento článek vysvětluje, jak tooconfigure hello priority přenosů metody směrování Traffic Manager"
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
ms.openlocfilehash: dd3e3bb2a727e5ea087cee35962c8e6f7c357282
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Konfigurace metody směrování provozu s prioritou v Traffic Manageru

Bez ohledu na režim hello webu poskytují weby Azure již funkce převzetí služeb při selhání pro weby v rámci datového centra (označované také jako oblast). Traffic Manager poskytuje převzetí služeb při selhání pro weby v různých datových centrech.

Běžný vzor pro převzetí služeb při selhání služby je primární služba tooa toosend provozu a poskytují sadu identické zálohování služby pro převzetí služeb při selhání. Hello následující kroky popisují, jak se tooconfigure to nastavovat převzetí služeb při selhání s Azure cloud services a weby:

## <a name="tooconfigure-hello-priority-traffic-routing-method"></a>směrování metodu tooconfigure hello priority provozu

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com). Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/). 
2. V panelu vyhledávání hello portál, vyhledejte hello **profily Traffic Manageru** a pak klikněte na název profilu hello, který chcete tooconfigure hello metody směrování pro.
3. V hello **profil služby Traffic Manager** okno, ověřte, že oba hello cloudové služby a nejsou weby, že chcete tooinclude ve vaší konfiguraci.
4. V hello **nastavení** klikněte na tlačítko **konfigurace**a v hello **konfigurace** okně dokončení následujícím způsobem:
    1. Pro **nastavení metodu směrování provozu**, ověřte, zda text hello metodu směrování provozu je **s prioritou**. Pokud není, klikněte na tlačítko **s prioritou** z rozevíracího seznamu hello.
    2. Sada hello **nastavení monitorování koncového bodu** stejné pro všechny každý koncový bod v tomto profilu následujícím způsobem:
        1. Vyberte odpovídající hello **protokol**a zadejte hello **Port** číslo. 
        2. Pro **cesta** zadejte lomítkem  */* . toomonitor koncových bodů, musíte zadat cestu a název souboru. A dopředné lomítko "/" je platná položka pro hello relativní cestu a znamená, že soubor hello je v kořenovém adresáři hello (výchozí).
        3. V horní části hello hello stránky, klikněte na tlačítko **Uložit**.
5. V hello **nastavení** klikněte na tlačítko **koncové body**.
6. V hello **koncové body** okno, zkontrolujte hello pořadí priorit pro koncové body. Když vyberete hello **s prioritou** záleží na pořadí hello koncových bodů hello vybrali metodu směrování provozu. Ověřte hello pořadí podle priority koncových bodů.  primární koncový bod Hello je nahoře. Zkontrolujte na hello pořadí, ve kterém se zobrazí. všechny žádosti směrované toohello první koncový bod, který se Traffic Manager zjistí, je-li být není v pořádku, provoz hello automaticky převezme toohello další koncový bod. 
7. toochange hello pořadí podle priority koncový bod, klikněte na tlačítko hello koncovému bodu a v hello **koncový bod** okno, které se zobrazí, klikněte na tlačítko **upravit** a změňte hello **s prioritou** hodnoty podle potřeby . 
8. Klikněte na tlačítko **Uložit** toosave změnit nastavení pro koncový bod hello.
9. Po dokončení změny konfigurace, klikněte na tlačítko **Uložit** v hello dolní části stránky hello.
10. Testovací hello změny v konfiguraci následujícím způsobem:
    1.  V panelu vyhledávání hello portál vyhledejte název profilu Traffic Manageru hello a klikněte na profil služby Traffic Manager hello ve výsledcích hello této hello zobrazí.
    2.  V hello **Traffic Manager** profilu okno, klikněte na tlačítko **přehled**.
    3.  Hello **profil služby Traffic Manager** zobrazuje název DNS hello vaše nově vytvořený profil služby Traffic Manager. Toho můžete využít všechny klienty (například přechodem tooit pomocí webového prohlížeče) tooget směrovány toohello pravý koncový bod určeného typ směrování hello. V takovém případě všechny požadavky jsou směrované toohello první koncový bod a Traffic Manager zjistí, je-li být není v pořádku, provoz hello automaticky převezme toohello další koncový bod.
11. Po váš profil služby Traffic Manager funguje, upravte záznam DNS hello na vaše autoritativní server toopoint DNS domény název toohello Traffic Manager název domény vaší společnosti.

![Konfigurace metody směrování priority provozu pomocí Traffic Manager][1]

## <a name="next-steps"></a>Další kroky


- Další informace o [vážené metodu směrování provozu](traffic-manager-configure-weighted-routing-method.md).
- Další informace o [metody směrování podle výkonu](traffic-manager-configure-performance-routing-method.md).
- Další informace o [geografické metody směrování](traffic-manager-configure-geographic-routing-method.md).
- Zjistěte, jak příliš[Traffic Manager nastavení testů](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png