---
title: "Azure Functions tooperform aaaUse databázi vyčištění úloh | Microsoft Docs"
description: "Použití Azure Functions tooschedule úlohu, která připojí tooAzure SQL Database tooperiodically čistí řádky."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 076f5f95-f8d2-42c7-b7fd-6798856ba0bb
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/22/2017
ms.author: glenga
ms.openlocfilehash: 063a25fe8d14a75d54e9b72cec9fc1e25fa3ff44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-functions-tooconnect-tooan-azure-sql-database"></a>Použití Azure Functions tooconnect tooan Azure SQL Database
Toto téma ukazuje, jak toouse Azure Functions toocreate naplánované úlohy, která vyčistí řádky v tabulce v Azure SQL Database. Hello nové C# funkce se vytvoří na základě šablony aktivační událost časovače předem definovaných v hello portálu Azure. toosupport v tomto scénáři je nutné také nastavit připojovací řetězec databáze jako nastavení v aplikaci funkce hello. Tento scénář používá hromadné operace hello databázi. toohave vaší funkce proces jednotlivé operace CRUD v tabulce Mobile Apps, měli byste místo toho použít [Mobile Apps vazby](functions-bindings-mobile-apps.md).

## <a name="prerequisites"></a>Požadavky

+ Toto téma používá funkci spustí časovač. Dokončení hello kroky v tématu hello [vytvořit funkci v Azure, který je aktivován časovač](functions-create-scheduled-function.md) toocreate C# verze této funkce.   

+ Toto téma popisuje příkaz Transact-SQL, který spustí hromadné operace čištění hello **SalesOrderHeader** tabulky v ukázkové databáze AdventureWorksLT hello. ukázkové databáze AdventureWorksLT toocreate hello dokončení hello kroky v tématu hello [vytvoření Azure SQL database v hello portál Azure](../sql-database/sql-database-get-started-portal.md). 

## <a name="get-connection-information"></a>Získání informací o připojení

Je třeba tooget hello připojovacího řetězce pro databázi hello jste vytvořili, když jste dokončili [vytvoření Azure SQL database v hello portál Azure](../sql-database/sql-database-get-started-portal.md).

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
 
3. Vyberte **databází SQL** z nabídky na levé straně hello a vyberte svou databázi na hello **databází SQL** stránky.

4. Vyberte **zobrazit databázové připojovací řetězce** a dokončení kopírování hello **ADO.NET** připojovací řetězec.

    ![Zkopírujte připojovací řetězec ADO.NET hello.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-hello-connection-string"></a>Nastavit hello připojovací řetězec 

Funkce aplikace hostuje hello provádění funkcí v Azure. Je nejlepší postup toostore připojovací řetězce a jiné tajné klíče v aplikaci nastavení funkce. Pomocí nastavení aplikace zabraňuje náhodného zpřístupnění hello připojovací řetězec s vašeho kódu. 

1. Přejděte jste vytvořili aplikaci funkce tooyour [vytvořit funkci v Azure, který je aktivován časovač](functions-create-scheduled-function.md).

2. Vyberte **funkce** > **nastavení aplikace**.
   
    ![Nastavení aplikace pro funkce aplikace hello.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. Posuňte se dolů příliš**připojovací řetězce** a přidat připojovací řetězec pomocí nastavení hello uvedených v tabulce hello.
   
    ![Přidáte nastavení aplikace toohello funkce připojovacího řetězce.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | Nastavení       | Navrhovaná hodnota | Popis             | 
    | ------------ | ------------------ | --------------------- | 
    | **Název**  |  sqldb_connection  | Použít tooaccess hello uložené připojovací řetězec v kódu funkce.    |
    | **Hodnota** | Řetězec zkopírovaný  | Po hello připojovací řetězec jste zkopírovali v předchozím oddílu hello. |
    | **Typ** | SQL Database | Pomocí připojení k databázi SQL výchozí hello. |   

3. Klikněte na **Uložit**.

Teď můžete přidat hello C# funkce kód, který připojí tooyour databáze SQL.

## <a name="update-your-function-code"></a>Aktualizace kódu – funkce

1. V aplikaci funkce vyberte funkce aktivovaného časovačem hello.
 
3. Přidejte následující odkazy na sestavení v horní části hello hello existující funkce kódu hello:

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. Přidejte následující hello `using` funkce toohello příkazy:
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. Nahradit stávající hello **spustit** funkce s hello následující kód:
    ```cs
    public static async Task Run(TimerInfo myTimer, TraceWriter log)
    {
        var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
        using (SqlConnection conn = new SqlConnection(str))
        {
            conn.Open();
            var text = "UPDATE SalesLT.SalesOrderHeader " + 
                    "SET [Status] = 5  WHERE ShipDate < GetDate();";

            using (SqlCommand cmd = new SqlCommand(text, conn))
            {
                // Execute hello command and log hello # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    Tento ukázkový příkaz aktualizuje hello **stav** sloupec založen na datum expedice hello. 32 řádků dat je by měl aktualizovat.

5. Klikněte na tlačítko **Uložit**, sledovat hello **protokoly** windows pro hello další funkce spuštění a pak hello počet řádků v hello aktualizována Poznámka: **SalesOrderHeader** tabulky.

    ![Zobrazit protokoly funkcí hello.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a>Další kroky

Dále se naučíte, jak funguje toouse s Logic Apps toointegrate s jinými službami.

> [!div class="nextstepaction"] 
> [Vytvoří funkci, která se integruje s Logic Apps](functions-twitter-email.md)

Další informace o funkcích najdete v tématu hello následující témata:

* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.  
