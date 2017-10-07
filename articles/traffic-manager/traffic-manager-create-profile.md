---
title: aaaCreate profil Traffic Manageru v Azure | Microsoft Docs
description: "Tento článek popisuje, jak toocreate profil Traffic Manageru"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: kumud
ms.openlocfilehash: 5cd3d2903552c9a0427da41a73f6f38f6b0afc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Traffic Manageru

Tento článek popisuje, jak profil s **s prioritou** tooroute koncové body Azure Web Apps tootwo uživatelů lze vytvořit typ směrování. Pomocí hello **s prioritou** směrování typu, veškerý provoz je první koncový bod směrované toohello při hello druhý se ukládají jako záložní. Uživatelé v důsledku toho může být směrované toohello druhý koncový bod, pokud první koncový bod hello se změní na není v pořádku.

V tomto článku jsou dva koncové body vytvořené webové aplikace Azure přidružené toothis nově vytvořený profil služby Traffic Manager. Další informace o tom toolearn koncových bodů webové aplikace Azure toocreate, navštivte hello [stránce dokumentace Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/). Můžete přidat libovolný koncový bod, který má název DNS a je dostupný přes hello veřejný internet, a že jako příklad používáme koncové body Azure Web Apps.

### <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Traffic Manageru
1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com). Pokud účet nemáte, můžete pro registraci [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/free/). 
2. Na hello **rozbočovače** nabídky, klikněte na tlačítko **nový** > **sítě** > **zobrazit všechny**, klikněte na tlačítko **provoz Správce** profil tooopen hello **profil služby Traffic Manager vytvořit** okno, pak klikněte na tlačítko **vytvořit**.
3. Na hello **profil služby Traffic Manager vytvořit** okně dokončení následujícím způsobem:
    1. Do pole **Název** zadejte název profilu. Tento název musí toobe jedinečné v rámci zóny trafficmanager.net hello a výsledkem název DNS hello <name>, trafficmanager.net, což je použité tooaccess vašeho profilu Traffic Manageru.
    2. V **metoda směrování**, vyberte hello **s prioritou** metody směrování.
    3. V **předplatné**, vyberte předplatné hello toocreate chcete tento profil v části
    4. V **skupiny prostředků**, vytvořit nové tooplace skupiny prostředků v tomto profilu.
    5. V **umístění skupiny prostředků**, vyberte umístění hello hello skupiny prostředků. Toto nastavení odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na hello profil Traffic Manageru, která bude nasazena globálně.
    6. Klikněte na možnost **Vytvořit**.
    7. Po dokončení nasazení globální hello vašeho profilu Traffic Manager je uveden ve skupině příslušných prostředků jako jeden z prostředků hello.

    ![Vytvoření profilu Traffic Manageru](./media/traffic-manager-create-profile/Create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Přidat koncové body Traffic Manager

1. V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** výsledky této hello zobrazí název, který jste vytvořili v předcházející části a klikněte na profil správce provozu hello v hello hello.
2. V hello **profil služby Traffic Manager** okno, v hello **nastavení** klikněte na tlačítko **koncové body**.
3. V hello **koncové body** okno, které se zobrazí, klikněte na tlačítko **přidat**.
4. V hello **přidání koncového bodu** okně dokončení následujícím způsobem:
    1. V části **Typ** klikněte na **Koncový bod Azure**.
    2. Zadejte **název** podle kterého chcete toorecognize tento koncový bod.
    3. Pro **cíle typ prostředku**, klikněte na tlačítko **služby App Service**.
    4. Pro **cíle prostředků**, klikněte na tlačítko **vybrat aplikační službu** tooshow hello seznam hello webové aplikace v rámci hello stejného předplatného. V hello **prostředků** okno, které se zobrazí, vyberte hello má tooadd jako hello první koncový bod služby App service.
    5. V části **Priorita** vyberte **1**. Výsledkem je veškerý provoz směřující toothis koncový bod, pokud je v pořádku.
    6. Políčko **Přidat jako zakázaný** ponechte nezaškrtnuté.
    7. Klikněte na tlačítko **OK**.
5.  Zopakujte kroky 3 a 4 pro hello další koncový bod Azure Web Apps. Ujistěte se, že tooadd se s jeho **s prioritou** hodnota nastavena na **2**.
6.  Po dokončení přidávání hello obou koncových bodů jsou zobrazeny v hello **profil služby Traffic Manager** okno spolu s jejich stav monitorování jako **Online**.

    ![Přidání koncového bodu Traffic Manager](./media/traffic-manager-create-profile/add-traffic-manager-endpoint.png)

## <a name="use-hello-traffic-manager-profile"></a>Použít profil služby Traffic Manager hello
1.  V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** název, který jste vytvořili v předcházející části hello. V hello výsledky, které se zobrazí klikněte na profil správce provozu hello.
2. V hello **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**.
3. Hello **profil služby Traffic Manager** zobrazuje název DNS hello vaše nově vytvořený profil služby Traffic Manager. Toho můžete využít všechny klienty (například přechodem tooit pomocí webového prohlížeče) tooget směrovány toohello pravý koncový bod určeného typ směrování hello. V takovém případě všechny požadavky jsou směrované toohello první koncový bod a Traffic Manager zjistí, je-li být není v pořádku, provoz hello automaticky převezme toohello další koncový bod.

## <a name="delete-hello-traffic-manager-profile"></a>Odstranit profil služby Traffic Manager hello
Pokud již nepotřebujete, odstraňte skupinu prostředků hello a hello profil služby Traffic Manager, kterou jste vytvořili. toodo Ano, vyberte skupinu prostředků hello z hello **profil služby Traffic Manager** a klikněte na **odstranit**.

## <a name="next-steps"></a>Další kroky

- Další informace o [směrování typy](traffic-manager-routing-methods.md).
- Další informace o typy koncových bodů [typy koncových bodů](traffic-manager-endpoint-types.md).
- Další informace o [monitorování koncového bodu](traffic-manager-monitoring.md).



