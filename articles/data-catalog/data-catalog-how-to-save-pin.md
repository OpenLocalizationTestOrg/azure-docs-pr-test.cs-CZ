---
title: "aaaSave hledání a kód pin datových prostředcích v Azure Data Catalog | Microsoft Docs"
description: "Jak tooarticle zvýraznění možnosti v Azure Data Catalog pro ukládání zdroje dat a datových prostředků pro pozdější použití."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0ad0a31d4b84782fed9d80acc2734912eecd6d74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Uložte hledání a kód pin datových prostředků v Azure Data Catalog
## <a name="introduction"></a>Úvod
Azure Data Catalog poskytuje možnosti pro zjišťování zdroje. Můžete rychle vyhledávání a filtrování hello katalogu toolocate datové zdroje a pochopení jejich zamýšlený účel, takže je jednodušší toofind hello správná data pro úlohu hello po ruce.

Ale co když musíte tooregularly pracovat s hello stejná data? A co když vy a ostatní uživatelé pravidelně přispívat vaší toohello znalostní báze stejného zdroje dat v katalogu hello? V těchto situacích s toorepeatedly problém hello stejné hledání může být neefektivní. Toto je, kde může pomoci uložené hledání a definovaného datových prostředků.

## <a name="saved-searches"></a>Uložená hledání
Uložené hledání v katalogu Data Catalog je opakovaně využívat, definice vyhledávání na uživatele. Můžete definovat hledání, včetně podmínek vyhledávání, značky a ostatní filtry a pak ho uložte. Můžete znovu spustit hello uložené hledání definice novější tooreturn žádné datových prostředků, které odpovídají jeho kritéria vyhledávání.

### <a name="create-a-saved-search"></a>Vytvoření uloženého hledání
toocreate uloženého hledání hello následující:
1. V portálu Azure Data Catalog hello v hello **aktuální vyhledávání** okně klikněte na tlačítko **Uložit**. 

    ![Aktuální odkaz Uložit nastavení vyhledávání](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Zadejte kritéria hledání hello má tooreuse a pak klikněte na tlačítko **Uložit**.

    ![Aktuální nastavení vyhledávání uložit název vyhledávání](./media/data-catalog-how-to-save-pin/02-name.png)

3. Když se zobrazí výzva, zadejte název pro uložené hledání hello. Vyberte název, který má smysl a která popisuje hello datové prostředky, které vrátí vyhledávání hello.

### <a name="manage-saved-searches"></a>Spravovat uložená hledání
Po uložení jeden nebo více hledání **uložená hledání** možnost se zobrazí pod hello **aktuální vyhledávání** pole. Po rozbalení seznamu hello se zobrazují všechna uložená hledání.

 ![Seznam uložených hledání](./media/data-catalog-how-to-save-pin/03-list.png)

Proveďte jeden z následujících hello:

* tooexecute vyhledávání, vyberte v seznamu hello uloženého hledání.

* tooview seznam možností správy pro uložené hledání, klikněte na tlačítko hello dolů šipku další toohello vyhledávací název.

    ![Možnosti pro správu uložených hledání](./media/data-catalog-how-to-save-pin/04-managing.png)

* Vyberte tooenter nový název pro vyhledávání uložit hello **přejmenovat**. definice vyhledávání Hello se nezmění.

* tooremove hello uložené hledání v seznamu vyberte **odstranit**a pak potvrďte odstranění hello.

* hledání toomark hello uložit jako výchozí hledání, vyberte **uložit jako výchozí**. Pokud provádíte "prázdný" vyhledávání z domovské stránky hello Azure Data Catalog, se spustí vyhledávání výchozí. Kromě toho hello vyhledávání, který je označen jako první hello hello je zobrazeno výchozí vyhledávání hello **uložená hledání** seznamu.

### <a name="organizational-saved-searches"></a>Organizační uložených hledání
Všechny uživatele ve vaší organizaci můžete uložit hledání pro vlastní použití. Správci katalogu dat můžete také uložit hledání pro všechny uživatele v rámci organizace hello. Když správci uložte hledání, že máte na výběr **sdílenou složku v rámci společnosti hello** možnost. Výběrem této možnosti hello sdílené složky uložené hledání pro všechny uživatele v organizaci hello.

 ![Organizační uložených hledání](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Definovaného datových prostředků
S uložená hledání můžete uložit a opakovaně používat definice vyhledávání. časem jako obsah hello hello katalogu změny může změnit Hello datové prostředky, které jsou výsledky hledání hello. Při připnete datových prostředků, můžete identifikovat explicitně toomake prostředky údaje specifické pro jejich snazší tooaccess bez nutnosti toouse vyhledávání.

Připnutí datový prostředek je jednoduchá. seznam tooadd hello data asset tooyour připnutý, klepněte na tlačítko hello **pin** ikonu. Zobrazí se ikona Hello v hello rohu hello asset dlaždici v zobrazení tile hello a ve sloupci nejvíce vlevo hello v zobrazení seznamu hello v portálu Azure Data Catalog hello.

![ikonu Připnutí datový prostředek Hello](./media/data-catalog-how-to-save-pin/05-pinning.png)

Odepnutí datový prostředek je stejně jednoduché. Jednoduše klikněte na tlačítko hello **Odepnout** ikonu tootoggle hello nastavení pro vybraný prostředek hello.

![Ikona Odepnout Hello datový – prostředek](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="hello-my-assets-section"></a>Hello části Moje prostředky
Hello katalogu Data Catalog zahrnuje domovské stránky portálu **Moje prostředky** oddíl, který zobrazuje prostředky, které vás zajímají toohello aktuálního uživatele. Tato část zahrnuje obě definovaného prostředky a uložená hledání.

![Hello části Moje prostředky na domovskou stránku hello](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Souhrn
Azure Data Catalog poskytuje možnosti, které jednodušší zdroje dat hello toodiscover, které budete potřebovat, takže vám i ostatním členům organizace může trávit méně času, hledá dat a další čas pracovat s ním. Uložená hledání a připnuté data, která prostředky sestavení na tyto základní možnosti, aby uživatelé mohli snadno identifikovat zdrojů dat, které pracují s, opakovaně.
