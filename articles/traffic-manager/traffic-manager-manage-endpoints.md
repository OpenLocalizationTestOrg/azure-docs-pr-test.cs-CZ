---
title: "aaaManage koncových bodů v Azure Traffic Manageru | Microsoft Docs"
description: "Tento článek vám pomůže při přidávání, odebírání, povolování a zakazování koncových bodů v Azure Traffic Manageru."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kumud
ms.openlocfilehash: fc65874ae2eaeb6fca5d8c4f33403c258307bdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-disable-enable-or-delete-endpoints"></a>Přidávání, zakazování, povolování nebo odstraňování koncových bodů

Hello funkce Web Apps v Azure App Service již poskytuje převzetí služeb při selhání a funkce směrování provozu kruhovým dotazováním pro weby v datacentru, bez ohledu na režim webu hello. Azure Traffic Manager umožňuje toospecify převzetí služeb při selhání a směrování provozu kruhovým dotazováním pro weby a služby cloud services v různých datových centrech. Hello první krok nezbytné tooprovide že jsou funkce tooadd hello cloudové služby nebo web koncového bodu tooTraffic správce.

Můžete také zakázat jednotlivé koncové body, které jsou součástí profilu Traffic Manageru. Zakázaný koncový bod zůstává součástí profilu hello, ale profil hello chová, jako koncový bod hello není zahrnutý v ní. Tato akce je užitečná pro dočasné odebrání koncového bodu, který se nachází v režimu údržby nebo který se znovu nasazuje. Jakmile je koncový bod hello znovu spuštěn a, může být povoleno.

> [!NOTE]
> Zakázání koncového bodu nijak nesouvisí toodo s jeho stavem nasazení v Azure. Funkční koncový bod zůstává nahoru a musí umožňovat provoz tooreceive i v případě, že v Traffic Manageru zakázán. Zakázání koncový bod v jednom profilu navíc nemá vliv na jeho stav v jiném profilu.

## <a name="tooadd-a-cloud-service-or-an-app-service-endpoint-tooa-traffic-manager-profile"></a>tooadd cloudové služby nebo tooa koncový bod služby aplikace profil služby Traffic Manager

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).
2. V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** jméno, které chcete toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky této hello zobrazí.
3. V hello **profil služby Traffic Manager** okno, v hello **nastavení** klikněte na tlačítko **koncové body**.
4. V hello **koncové body** okno, které se zobrazí, klikněte na tlačítko **přidat**.
5. V hello **přidání koncového bodu** okně dokončení následujícím způsobem:
    1. V části **Typ** klikněte na **Koncový bod Azure**.
    2. Zadejte **název** podle kterého chcete toorecognize tento koncový bod.
    3. Pro **cíle typ prostředku**, z hello rozevíracího seznamu, vyberte typ prostředku odpovídající hello.
    4. Pro **cíle prostředků**, z hello rozevíracího seznamu, vyberte prostředek, příslušná cílová hello hello tooshow hello seznamu prostředků v rámci stejného předplatného v hello **prostředky okno**. V hello **prostředků** okno, které se zobrazí, vyberte hello služby má tooadd jako hello první koncový bod.
    5. V části **Priorita** vyberte **1**. Výsledkem je veškerý provoz směřující toothis koncový bod, pokud je v pořádku.
    6. Políčko **Přidat jako zakázaný** ponechte nezaškrtnuté.
    7. Klikněte na tlačítko **OK**.
6.  Opakujte kroky 4 a 5 tooadd hello další koncového bodu Azure. Ujistěte se, že tooadd se s jeho **s prioritou** hodnota nastavena na **2**.
7.  Po dokončení přidávání hello obou koncových bodů jsou zobrazeny v hello **profil služby Traffic Manager** okno spolu s jejich stav monitorování jako **Online**.

> [!NOTE]
> Po přidání nebo odebrání koncového bodu z profilu pomocí hello *převzetí služeb při selhání* metoda směrování provozu, nemusí být seřazené hello seznam priorit převzetí služeb při selhání, budou požadovaným způsobem. Můžete upravit pořadí hello hello seznam priorit převzetí služeb při selhání na stránce konfigurace hello. Další informace najdete v části [Konfigurování směrování provozu s převzetím služeb při selhání](traffic-manager-configure-failover-routing-method.md).

## <a name="toodisable-an-endpoint"></a>toodisable koncový bod

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).
2. V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** název má toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky, které jsou zobrazeny.
3. V hello **profil služby Traffic Manager** okno, v hello **nastavení** klikněte na tlačítko **koncové body**. 
4. Klikněte na koncový bod hello, které chcete toodisable a pak na hello **koncový bod** okno, které se zobrazí, klikněte na tlačítko **upravit**.
5. V hello **koncový bod** okně změnit stav koncového bodu hello příliš**zakázané**a potom klikněte na **Uložit**.
6. Klienti dál toosend provoz toohello koncový bod pro dobu trvání hello Time-to-Live (TTL). Na stránce konfigurace hello hello profil služby Traffic Manager hello TTL můžete změnit.

## <a name="tooenable-an-endpoint"></a>tooenable koncový bod

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).
2. V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** název má toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky, které jsou zobrazeny.
3. V hello **profil služby Traffic Manager** okno, v hello **nastavení** klikněte na tlačítko **koncové body**. 
4. Klikněte na koncový bod hello, které chcete toodisable a pak na hello **koncový bod** okno, které se zobrazí, klikněte na tlačítko **upravit**.
5. V hello **koncový bod** okně změnit stav koncového bodu hello příliš**povoleno**a potom klikněte na **Uložit**.
6. Klienti dál toosend provoz toohello koncový bod pro dobu trvání hello Time-to-Live (TTL). Na stránce konfigurace hello hello profil služby Traffic Manager hello TTL můžete změnit.

## <a name="toodelete-an-endpoint"></a>toodelete koncový bod

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).
2. V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** název má toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky, které jsou zobrazeny.
3. V hello **profil služby Traffic Manager** okno, v hello **nastavení** klikněte na tlačítko **koncové body**. 
4. Klikněte na koncový bod hello, které chcete toodisable a pak na hello **koncový bod** okno, které se zobrazí, klikněte na tlačítko **upravit**.
5. V hello **koncový bod** okně změnit stav koncového bodu hello příliš**povoleno**a potom klikněte na **Uložit**.


## <a name="next-steps"></a>Další kroky

* [Správa profilů Traffic Manager](traffic-manager-manage-profiles.md)
* [Konfigurace metod směrování](traffic-manager-configure-routing-method.md)
* [Řešení potíží při sníženém výkonu Traffic Manageru](traffic-manager-troubleshooting-degraded.md)
* [Důležité informace o výkonu nástroje Traffic Manager](traffic-manager-performance-considerations.md)
* [Operace v Traffic Manageru (referenční informace k rozhraní API REST)](http://go.microsoft.com/fwlink/p/?LinkID=313584)

