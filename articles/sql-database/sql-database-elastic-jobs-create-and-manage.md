---
title: "aaaManage skupiny databází Azure SQL | Microsoft Docs"
description: "Provede procesem vytváření a správa elastické úlohy."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: a73c4fb24c332fae0e917c18272724cccd56f29a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Vytvoření a správa škálovaný databází Azure SQL pomocí elastické úlohy (preview)


**Elastické databáze úlohy** zjednodušení správy skupin databáze spuštěním operace správy, například změny schématu, Správa přihlašovacích údajů, aktualizace dat odkaz, shromažďování dat výkonu nebo telemetrie klienta (zákazníka) kolekce. Úlohy elastické databáze je aktuálně k dispozici prostřednictvím hello portál Azure a rutin prostředí PowerShell. Však hello Azure portálu povrchy omezené funkčnosti omezené tooexecution napříč všechny databáze [elastického fondu (preview)](sql-database-elastic-pool.md). nastavit další funkce tooaccess a spouštění skriptů napříč skupinou databází, včetně vlastní definovanou kolekci nebo horizontálního oddílu (vytvořený [klientské knihovny pro elastické databáze](sql-database-elastic-scale-introduction.md)), najdete v části [vytváření a Správa úlohy pomocí prostředí PowerShell](sql-database-elastic-jobs-powershell.md). Další informace o úlohách najdete v tématu [přehled úlohy elastické databáze](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* Elastický fond. V tématu [o elastické fondy](sql-database-elastic-pool.md).
* Instalace součásti služby úlohy elastické databáze. V tématu [instalací služby úlohy elastické databáze hello](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Vytváření úloh
1. Pomocí hello [portál Azure](https://portal.azure.com), z existující fond elastické databáze úlohy, klikněte na tlačítko **vytvořit úlohu**.
2. Zadejte hello uživatelské jméno a heslo správce databáze hello (vytvořeny při instalaci úloh) pro databázi řízení úloh hello (metadata úložiště pro úlohy).
   
    ![Název úlohy hello, zadejte nebo vložte kód a klikněte na tlačítko Spustit][1]
3. V hello **vytvoření úlohy** okno, zadejte název pro úlohu hello.
4. Zadejte hello uživatelské jméno a heslo tooconnect toohello cílovým databázím s dostatečnými oprávněními toosucceed provádění skriptu.
5. Vložit nebo zadejte ve skriptu hello T-SQL.
6. Klikněte na tlačítko **Uložit** a pak klikněte na **spustit**.
   
    ![Vytvoření úlohy a spuštění][5]

## <a name="run-idempotent-jobs"></a>Spuštění úloh idempotent
Při spuštění skriptu pro sadu databází, musí být jisti, že skript hello je idempotent. To znamená, hello skriptu musí být schopný toorun vícekrát, i v případě, že se nezdařil před nekompletnímu stavu. Například pokud skript selže, hello úlohy bude automaticky opakovat, dokud neproběhne úspěšně (v rámci omezení, jako hello opakování logiku nakonec přestanou hello opakování). toodo Hello způsob, jak je to toouse hello klauzuli "Pokud existuje" a odstranit všechny nalezen instanci před vytvořením nového objektu. Zde je uveden příklad:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Můžete taky použijte klauzuli "Pokud není existuje" před vytvořením nové instance:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Tento skript potom aktualizuje hello tabulky, které jste vytvořili dříve.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Kontrola stavu úloh
Po zahájení úlohy, můžete zkontrolovat v jeho průběhu.

1. Na stránce hello elastického fondu, klikněte na tlačítko **spravovat úlohy**.
   
    ![Klikněte na tlačítko "Spravovat úlohy"][2]
2. Klikněte na hello název (a) úlohy. Hello **stav** může být "Dokončeno" nebo "Se nezdařilo." s jeho datum a čas vytvoření a spuštění zobrazují podrobnosti úlohy Hello (b). Hello seznamu hello zobrazující průběh hello hello skriptu na každou databázi ve fondu hello poskytnutí podrobnosti datum a čas (c).
   
    ![Kontrola dokončení úlohy][3]

## <a name="checking-failed-jobs"></a>Kontrola neúspěšné úlohy
Pokud úloha selže, můžete najít protokolu jeho spuštění. Klikněte na tlačítko hello název úlohy toosee hello se nezdařilo její podrobnosti.

![Podívejte se neúspěšnou úlohu][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


