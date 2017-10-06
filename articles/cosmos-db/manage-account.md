---
title: "aaaManage účet Azure Cosmos DB prostřednictvím hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage vaše Azure DB Cosmos účet prostřednictvím hello portálu Azure. Najít průvodce na pomocí tooview hello portálu Azure, kopírování, odstranění a přístup k účtům."
keywords: "Portál Azure, azure documentdb, Microsoft azure"
services: cosmos-db
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kirillg
ms.openlocfilehash: 77ad953cf558a519674be08ad913a12202f69496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-an-azure-cosmos-db-account"></a>Jak toomanage účet Azure Cosmos DB
Zjistěte, jak pracovat s klíči tooset globální konzistence a odstranit účet Azure Cosmos DB v hello portálu Azure.

## <a id="consistency"></a>Spravovat nastavení konzistence Azure Cosmos DB
Výběr úrovně konzistence správné hello závisí na sémantiku hello vaší aplikace. Byste si měli přečíst hello úrovně konzistence dostupné v Azure Cosmos DB načtením [pomocí konzistence úrovně toomaximize dostupnosti a výkonu v Azure Cosmos DB][consistency]. Azure Cosmos DB poskytuje konzistence, dostupnosti a výkonu záruky, na všech úrovních konzistence k dispozici pro váš účet databáze. Konfigurace účtu databáze s úrovní konzistence silným vyžaduje, aby vaše data uzavřeného tooa jedné oblasti Azure a globálně dostupnou. Na druhé straně text hello, hello úrovně konzistence volný - typu s ohraničenou prošlostí, session nebo případné povolit jste tooassociate libovolný počet oblastí Azure s vaším účtem databáze. Hello následující jednoduchými kroky vám ukážou, jak tooselect hello výchozí úroveň konzistence pro váš účet databáze. 

### <a name="toospecify-hello-default-consistency-for-an-azure-cosmos-db-account"></a>toospecify hello výchozí konzistence účtu Azure Cosmos DB
1. V hello [portál Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB.
2. V okně účtu hello, klikněte na tlačítko **výchozí konzistence**.
3. V hello **výchozí konzistence** , vyberte hello novou úroveň konzistence a klikněte na **Uložit**.
    ![Výchozí konzistence relace][5]

## <a id="keys"></a>Zobrazení, kopírování a opětovné vytváření přístupových klíčů
Při vytváření účtu Azure Cosmos DB hello služba generuje dva hlavní přístupové klíče, které lze použít pro ověření při přístupu k účtu Azure Cosmos DB hello. Poskytnutím dvou přístupových klíčů Azure Cosmos DB vám umožní tooregenerate hello klíče bez přerušení tooyour účet Azure Cosmos DB. 

V hello [portál Azure](https://portal.azure.com/), hello přístup **klíče** okno na v nabídce hello prostředků hello **účet Azure Cosmos DB** okno tooview, kopírování a opětovné vytváření hello přístupových klíčů, který jsou použité tooaccess účtu Azure Cosmos DB.

![Azure Portal snímku obrazovky okna klíče](./media/manage-account/keys.png)

> [!NOTE]
> Hello **klíče** okno také obsahuje primární a sekundární připojovací řetězce, které se dají použít tooconnect tooyour účet z hello [nástroj pro migraci dat](import-data.md).
> 
> 

Klíče jen pro čtení jsou také k dispozici v tomto okně. Čtení a dotazy jsou jen pro čtení operace, než vytvoří, odstranění, a nahradí nejsou.

### <a name="copy-an-access-key-in-hello-azure-portal"></a>Zkopírovat přístupový klíč v hello portálu Azure
Na hello **klíče** okně klikněte na tlačítko hello **kopie** toohello tlačítko napravo od hello klíč chcete toocopy.

![Zobrazení a zkopírovat přístupový klíč v hello portálu Azure, okna klíče](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a>Opětovné vygenerování přístupových klíčů
Měli byste změnit účet Azure Cosmos DB hello přístupu k klíče tooyour pravidelně toohelp připojení lépe zabezpečit. Dva přístupové klíče se přiřazují tooenable hello je toomaintain připojení toohello účet Azure Cosmos DB používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč.

> [!WARNING]
> Opětovné generování přístupových klíčů ovlivní všechny aplikace, které jsou závislé na aktuální klíč hello. Všichni klienti, kteří používají účet Azure Cosmos DB hello přístupu k klíče tooaccess hello musí být aktualizované toouse hello nový klíč.
> 
> 

Pokud máte aplikace nebo cloudové služby pomocí účtu Azure Cosmos DB hello, pokud, ztratíte hello připojení obnovit klíče, klíče nezaregistrujete. Hello následující kroky popisují proces hello účastní vrácení klíče.

1. Aktualizace hello přístupový klíč ve vaší aplikaci kód tooreference hello sekundární přístupový klíč hello účet Azure Cosmos DB.
2. Znovu vygenerujte primární přístupový klíč hello pro váš účet Azure Cosmos DB. V hello [portálu Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB.
3. V hello **Azure Cosmos DB účet** okně klikněte na tlačítko **klíče**.
4. Na hello **klíče** okně klikněte na tlačítko znovu generovat hello a pak klikněte na **Ok** tooconfirm, které chcete toogenerate nový klíč.
    ![Opětovné vygenerování přístupových klíčů](./media/manage-account/regenerate-keys.png)
5. Jakmile si ověříte, že je k dispozici pro použití této hello nový klíč (přibližně 5 minut po opětovné generování), aktualizujte hello přístupový klíč ve vaší aplikaci kód tooreference hello nové primární přístupový klíč.
6. Znova vygenerujte sekundární přístupový klíč hello.
   
    ![Opětovné vygenerování přístupových klíčů](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Může trvat několik minut, než nově vygenerovaný klíč může být použité tooaccess účtu Azure Cosmos DB.
> 
> 

## <a name="get-hello--connection-string"></a>Získat připojovací řetězec hello
tooretrieve váš připojovací řetězec, hello následující: 

1. V hello [portál Azure](https://portal.azure.com), přístup k účtu Azure Cosmos DB.
2. V nabídce hello prostředků, klikněte na tlačítko **klíče**.
3. Klikněte na tlačítko hello **kopie** tlačítko Další toohello **primární připojovací řetězec** nebo **sekundární připojovací řetězec** pole. 

Pokud používáte hello připojovací řetězec v hello [nástroj pro migraci databáze Azure Cosmos DB](import-data.md), připojit hello databáze název toohello konec hello připojovací řetězec. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a>Odstranit účet Azure Cosmos DB
tooremove Azure DB Cosmos účet z hello Azure portál, který už používáte, klikněte pravým tlačítkem na název účtu hello a klikněte na tlačítko **odstranit účet**.

![Jak toodelete Azure DB Cosmos účet v hello portálu Azure](./media/manage-account/deleteaccount.png)

1. V hello [portál Azure](https://portal.azure.com/), přístup k účtu Azure Cosmos DB hello chcete toodelete.
2. Na hello **účet Azure Cosmos DB** , klikněte pravým tlačítkem na účet hello a pak klikněte na tlačítko **odstranit účet**. 
3. V okně potvrzení výsledné hello zadejte hello Azure Cosmos DB účet název tooconfirm, které chcete toodelete hello účtu.
4. Klikněte na tlačítko hello **odstranit** tlačítko.

![Jak toodelete Azure DB Cosmos účet v hello portálu Azure](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Další kroky
Zjistěte, jak příliš[začít pracovat s vaším účtem Azure Cosmos DB](http://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
