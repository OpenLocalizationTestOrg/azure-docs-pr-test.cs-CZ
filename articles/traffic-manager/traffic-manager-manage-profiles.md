---
title: "aaaManage profilů Azure Traffic Manageru | Microsoft Docs"
description: "Tento článek vám pomůže vytvořit, zakázat, povolit nebo odstranit profil Azure Traffic Manageru."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: kumud
ms.openlocfilehash: 0c6ab0c451581d039514a9de0b525b3937e45a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-traffic-manager-profile"></a>Správa profilu Azure Traffic Manageru

Profily Traffic Manageru pomocí směrování provozu metody toocontrol hello distribuce přenosů tooyour cloudové služby nebo koncových bodů webu. Tento článek vysvětluje, jak toocreate a správy těchto profilů.

## <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Traffic Manageru

Profil Traffic Manageru můžete vytvořit pomocí hello portálu Azure. Po vytvoření profilu, můžete nakonfigurovat koncové body, monitorování a další nastavení v hello portálu Azure. Traffic Manager podporuje až too200 koncových bodů na jeden profil. Většina scénářů použití však vyžaduje jen několik koncových bodů.

### <a name="toocreate-a-traffic-manager-profile"></a>toocreate profil Traffic Manageru

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com). Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free/). 
2. Na hello **rozbočovače** nabídky, klikněte na tlačítko **nový** > **sítě** > **zobrazit všechny**, klikněte na tlačítko **provoz Správce** profil tooopen hello **profil služby Traffic Manager vytvořit** okno, pak klikněte na tlačítko **vytvořit**.
3. Na hello **profil služby Traffic Manager vytvořit** okně dokončení následujícím způsobem:
    1. Do pole **Název** zadejte název profilu. Tento název musí toobe jedinečné v rámci zóny trafficmanager.net hello a výsledkem název DNS hello <name>, trafficmanager.net, který je použité tooaccess vašeho profilu Traffic Manageru.
    2. V **metoda směrování**, vyberte hello **s prioritou** metody směrování.
    3. V **předplatné**, vyberte předplatné hello toocreate chcete tento profil v části
    4. V **skupiny prostředků**, vytvořit nové tooplace skupiny prostředků v tomto profilu.
    5. V **umístění skupiny prostředků**, vyberte umístění hello hello skupiny prostředků. Toto nastavení odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na hello profil Traffic Manageru, která bude nasazena globálně.
    6. Klikněte na možnost **Vytvořit**.
    7. Po dokončení nasazení globální hello vašeho profilu Traffic Manager je uveden ve skupině příslušných prostředků jako jeden z prostředků hello.

## <a name="disable-enable-or-delete-a-profile"></a>Zakázání, povolení nebo odstranění profilu

Existující profil můžete zakázat tak, že Traffic Manager neodkazuje uživatelské požadavky toohello nakonfigurované koncové body. Pokud profil Traffic Manageru zakážete, hello profil a hello informace obsažené v profilu hello zůstanou beze změn a lze ho upravovat v rozhraní Traffic Manageru hello.  Referenční seznamy pokračovat, až bude znovu povolíte hello profilu. Když vytvoříte profil Traffic Manageru v hello portálu Azure, je automaticky povolené. Pokud se rozhodnete, že profil již nebude potřebný, můžete ho odstranit.

### <a name="toodisable-a-profile"></a>toodisable profil

1. Pokud používáte vlastní název domény, změňte záznam CNAME hello na serveru DNS pro Internet tak, aby už odkazuje tooyour profil služby Traffic Manager.
2. Zastaví provoz vrácení směrované toohello koncových bodů prostřednictvím nastavení profilu Traffic Manageru hello.
3. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).
2. V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** jméno, které chcete toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky této hello zobrazí.
3. V hello **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**, v okně Přehled hello klikněte na tlačítko **zakázat**a pak potvrďte profil služby Traffic Manager toodisable hello.

### <a name="tooenable-a-profile"></a>tooenable profil

1. V prohlížeči přihlásit toohello [portál Azure](http://portal.azure.com).
2. V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** jméno, které chcete toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky této hello zobrazí.
3. V hello **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**a potom v okně Přehled hello klikněte na tlačítko **povolit**.
5. Pokud používáte vlastní název domény, vytvořte záznam prostředku CNAME na Internetu název serveru DNS toopoint toohello domény vašeho profilu Traffic Manageru.
6. Přenosy jsou koncové body směrovanou toohello znovu.

### <a name="toodelete-a-profile"></a>toodelete profil

1. Ujistěte se, že hello záznam prostředku DNS na serveru DNS pro Internet již nepoužívá záznam prostředku CNAME, který odkazuje toohello název domény vašeho profilu Traffic Manageru.
2. V panelu vyhledávání hello portál, vyhledejte hello **profil služby Traffic Manager** jméno, které chcete toomodify a pak klikněte na profil služby Traffic Manager hello v hello výsledky této hello zobrazí.
3. V hello **profil služby Traffic Manager** okně klikněte na tlačítko **přehled**, v okně Přehled hello klikněte na tlačítko **odstranit**a pak potvrďte profil služby Traffic Manager toodelete hello.

## <a name="next-steps"></a>Další kroky

* [Přidání koncového bodu](traffic-manager-endpoints.md)
* [Konfigurace metody prioritního směrování](traffic-manager-configure-priority-routing-method.md)
* [Konfigurace metody geografického směrování](traffic-manager-configure-geographic-routing-method.md) 
* [Konfigurace metody váženého směrování](traffic-manager-configure-weighted-routing-method.md)
* [Konfigurace metody směrování podle výkonu](traffic-manager-configure-performance-routing-method.md)
