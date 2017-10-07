---
title: "aaaConfigure geografické provozu metody směrování pomocí Azure Traffic Manageru | Microsoft Docs"
description: "Tento článek vysvětluje, jak tooconfigure hello metodu směrování provozu geografické pomocí Azure Traffic Manager"
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
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 4142389211ae54e7feea6564641e01e4477491e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-geographic-traffic-routing-method-using-traffic-manager"></a>Konfigurace metody směrování hello geografické provozu pomocí Traffic Manager

Hello metodu směrování provozu geografické umožňuje toodirect provoz toospecific koncových bodů podle hello zeměpisného umístění, kde mají původ hello požadavky. V tomto kurzu se dozvíte, jak toocreate Traffic Manager profilu u této metody směrování a nakonfigurujte hello koncové body tooreceive provoz z konkrétní zeměpisných oblastí.

## <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Správce provozu

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com). Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/).
2. V nabídce centra hello, klikněte na tlačítko **nový** > **sítě** > **zobrazit všechny**a pak klikněte na tlačítko **profil služby Traffic Manager**tooopen hello **profil služby Traffic Manager vytvořit** okno.
3. Na hello **profil služby Traffic Manager vytvořit** okno:
    1. Zadejte název pro svůj profil. Tento název musí toobe jedinečné v rámci zóny trafficmanager.net hello a bude mít za následek název DNS hello <profilename>, trafficmanager.net, který bude možné použít tooaccess vašeho profilu Traffic Manageru.
    2. Vyberte hello **Geographic** metody směrování.
    3. Vyberte hello odběr, který chcete toocreate tento profil v části.
    4. Použít existující skupinu prostředků nebo vytvořte novou tooplace skupiny prostředků tohoto profilu v části. Pokud si zvolíte toocreate novou skupinu prostředků, použijte hello **umístění skupiny prostředků** rozevírací toospecify hello umístění skupiny prostředků hello. Toto nastavení odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na hello profil Traffic Manageru, které se nasadí globálně.
    5. Po kliknutí na tlačítko **vytvořit**, váš profil služby Traffic Manager se vytvoří a nasadí globálně.

![Vytvoření profilu Traffic Manageru](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Přidat koncové body

1. Vyhledejte název profilu Traffic Manageru hello, kterou jste právě vytvořili, v panelu vyhledávání hello portál a klikněte na výsledek hello je zobrazen.
2. Přejděte příliš**nastavení** -> **koncové body** v okně hello Traffic Manager.
3. Klikněte na tlačítko **přidat** tooshow hello **přidat koncový bod** okno.
3. V hello **koncové body** okně klikněte na tlačítko **přidat** a v hello **přidání koncového bodu** okno, které se zobrazí, dokončení následujícím způsobem:
4. Vyberte **typ** v závislosti na typu hello přidáváte koncového bodu. Zeměpisná směrování profily, které jsou určené v produkčním prostředí doporučujeme pomocí typy vnořených koncových bodů obsahující profil podřízené s více než jeden koncový bod. Další podrobnosti najdete v tématu [nejčastější dotazy o metodách směrování provozu geografické](traffic-manager-FAQs.md).
5. Zadejte **název** podle kterého chcete toorecognize tento koncový bod.
6. Určitá pole v tomto okně, závisí na typu hello koncového bodu, který chcete přidat:
    1. Chcete-li přidat koncový bod Azure, vyberte hello **cíle typ prostředku** a hello **cíl** podle prostředků hello chce toodirect provoz
    2. Chcete-li přidat **externí** koncový bod, zadejte hello **plně kvalifikovaný název domény (FQDN)** pro svůj koncový bod.
    3. Pokud přidáváte **koncový bod vnořené**, vyberte hello **cíle prostředků** odpovídající toohello podřízené profil toouse a určit hello **koncové body podřízené minimální počet**.
7. V části geografická mapování hello hello použijte rozevírací nabídku tooadd hello oblastí, ze kterého má koncový bod toothis toobe odeslat provoz. Je nutné přidat alespoň jednu oblast a může mít více oblastí namapované.
8. Tento postup opakujte pro všechny koncové body chcete tooadd pod tento profil

![Přidání koncového bodu Traffic Manager](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-hello-traffic-manager-profile"></a>Použít profil služby Traffic Manager hello
1.  V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** název, že jste vytvořili v předcházející části hello a že hello zobrazí výsledky, klikněte na profil správce provozu hello v hello.
2. V hello **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**.
3. Hello **profil služby Traffic Manager** zobrazuje název DNS hello vaše nově vytvořený profil služby Traffic Manager. Toho můžete využít všechny klienty (například přechodem tooit pomocí webového prohlížeče) tooget směrovány toohello pravý koncový bod určeného typ směrování hello.  V případě hello geografické směrování Traffic Manager porovná hello zdrojové IP adresy hello příchozích požadavků a určuje hello oblast, ve kterém je pocházející. Tuto oblast je namapované tooan koncový bod, je-li směrované toothere. Pokud tato oblast není namapované tooan koncový bod, Traffic Manager vrátí odpověď NODATA dotazu.

## <a name="next-steps"></a>Další kroky

- Další informace o [metoda směrování provozu Geographic](traffic-manager-routing-methods.md#geographic).
- Zjistěte, jak příliš[Traffic Manager nastavení testů](traffic-manager-testing-settings.md).
