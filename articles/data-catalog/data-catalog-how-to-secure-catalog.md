---
title: "aaaHow toosecure přístup tooAzure katalogu Data Catalog | Microsoft Docs"
description: "Tento článek vysvětlují, jak toosecure katalog data catalog a jeho datové prostředky."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: d7c35fd57d8add1cdc152b75f111879288e1548f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-access-toodata-catalog-and-data-assets"></a>Jak toosecure přistupovat k toodata katalogu a data prostředků
> [!IMPORTANT]
> Tato funkce je k dispozici pouze v edici standard hello systému Azure Data Catalog.

Azure Data Catalog umožňuje toospecify, kdo má přístup k katalogu dat hello a jaké operace (zaregistrovat, opatřit poznámkami, převzít vlastnictví) můžete provádět na metadata v katalogu hello. 

## <a name="catalog-users-and-permissions"></a>Uživatelé katalogu a oprávnění
toogive a uživatele nebo skupiny hello přistupovat ke katalogu dat tooa a nastavte oprávnění:

1. Na hello [domovskou stránku katalogu dat](http://www.azuredatacatalog.com), klikněte na tlačítko **nastavení** na panelu nástrojů hello.

    ![data catalog – nastavení](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Na stránce nastavení hello rozbalte hello **uživatelé katalogu** části.
    ![Katalogu uživatelé – přidat](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. Klikněte na tlačítko **Přidat**.
4. Zadejte plně kvalifikovaný hello **uživatelské jméno** nebo název hello **skupiny zabezpečení** v Azure Active Directory (AAD) přidružené k hello katalogu hello. Čárkou (', ') jako oddělovač, chcete-li přidat více než jeden uživatel nebo skupina.
    ![Uživatelé katalogu - uživatele nebo skupiny](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Stiskněte klávesu **ENTER** nebo **KARTĚ** mimo hello textové pole. 
6.  Potvrďte, že všechna oprávnění (**Poznámka**, **zaregistrovat**, a **převzít vlastnictví**) jsou přiřazeny toothese uživatele nebo skupiny ve výchozím nastavení. To znamená, hello uživatel nebo skupina může [zaregistrovat datových prostředků]( data-catalog-how-to-register.md), [opatřit poznámkami datové prostředky]( data-catalog-how-to-annotate.md), a [převzetí vlastnictví datových prostředků]( data-catalog-how-to-manage.md). 
    ![Uživatelé katalogu – výchozí oprávnění](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  toogive uživatele nebo skupinu pouze ke čtení hello, přístup toohello katalogu, zrušte zaškrtnutí hello **opatřit poznámkami** možnost pro tohoto uživatele nebo skupinu. Pokud tak učiníte, hello uživatele nebo skupinu nelze opatřit poznámkami datové prostředky v katalogu hello, ale je mohou zobrazit. 
8.  toodeny uživatele nebo skupinu registraci datových prostředků, zrušte hello **zaregistrovat** možnost pro tohoto uživatele nebo skupinu.
9.  toodeny uživatele z převzetí vlastnictví datový prostředek, zrušte hello **převzít vlastnictví** možnost pro tohoto uživatele nebo skupinu. 
10. Klikněte na tlačítko toodelete uživateli nebo skupině od uživatelů katalogu **x** pro hello uživatele nebo skupiny v hello dolní části seznamu hello. 
    ![Uživatelé katalogu – odstranit uživatele](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > Doporučujeme přidat přímo uživatelů toocatalog zabezpečení skupiny místo přidání uživatelů a přiřaďte oprávnění. Pak přidejte skupiny zabezpečení toohello uživatelé, které odpovídají jejich rolí a jejich katalogu toohello požadovaný přístup.

## <a name="special-considerations"></a>Zvláštní upozornění

- oprávnění Hello přiřazeny toosecurity skupiny jsou aditivní. Vyslovte uživatel je ve dvou skupinách. Jedna skupina má opatřit poznámkami oprávnění a jiné skupiny není mají opatřit poznámkami oprávnění. Potom uživatel má opatřit poznámkami oprávnění. 
- Hello oprávnění explicitně přiřazovat tooa uživatele přepsání hello oprávnění přiřazená toogroups toowhich hello uživatel patří. V předchozím příkladu hello Řekněme, explicitně přidali hello uživatele toocatalog uživatele a nepřiřazujte opatřit poznámkami oprávnění. Hello uživatele nelze opatřit poznámkami datové prostředky, i když hello uživatel je členem skupiny, která nemá opatřit poznámkami oprávnění.

## <a name="next-steps"></a>Další kroky
- [Začínáme s Azure Data Catalogem](data-catalog-get-started.md)

