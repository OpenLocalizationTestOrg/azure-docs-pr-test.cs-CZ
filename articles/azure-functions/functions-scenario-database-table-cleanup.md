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
# <a name="use-azure-functions-tooconnect-tooan-azure-sql-database"></a><span data-ttu-id="637d8-103">Použití Azure Functions tooconnect tooan Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="637d8-103">Use Azure Functions tooconnect tooan Azure SQL Database</span></span>
<span data-ttu-id="637d8-104">Toto téma ukazuje, jak toouse Azure Functions toocreate naplánované úlohy, která vyčistí řádky v tabulce v Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="637d8-104">This topic shows you how toouse Azure Functions toocreate a scheduled job that cleans up rows in a table in an Azure SQL Database.</span></span> <span data-ttu-id="637d8-105">Hello nové C# funkce se vytvoří na základě šablony aktivační událost časovače předem definovaných v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="637d8-105">hello new C# function is created based on a pre-defined timer trigger template in hello Azure portal.</span></span> <span data-ttu-id="637d8-106">toosupport v tomto scénáři je nutné také nastavit připojovací řetězec databáze jako nastavení v aplikaci funkce hello.</span><span class="sxs-lookup"><span data-stu-id="637d8-106">toosupport this scenario, you must also set a database connection string as a setting in hello function app.</span></span> <span data-ttu-id="637d8-107">Tento scénář používá hromadné operace hello databázi.</span><span class="sxs-lookup"><span data-stu-id="637d8-107">This scenario uses a bulk operation against hello database.</span></span> <span data-ttu-id="637d8-108">toohave vaší funkce proces jednotlivé operace CRUD v tabulce Mobile Apps, měli byste místo toho použít [Mobile Apps vazby](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="637d8-108">toohave your function process individual CRUD operations in a Mobile Apps table, you should instead use [Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="637d8-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="637d8-109">Prerequisites</span></span>

+ <span data-ttu-id="637d8-110">Toto téma používá funkci spustí časovač.</span><span class="sxs-lookup"><span data-stu-id="637d8-110">This topic uses a timer triggered function.</span></span> <span data-ttu-id="637d8-111">Dokončení hello kroky v tématu hello [vytvořit funkci v Azure, který je aktivován časovač](functions-create-scheduled-function.md) toocreate C# verze této funkce.</span><span class="sxs-lookup"><span data-stu-id="637d8-111">Complete hello steps in hello topic [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md) toocreate a C# version of this function.</span></span>   

+ <span data-ttu-id="637d8-112">Toto téma popisuje příkaz Transact-SQL, který spustí hromadné operace čištění hello **SalesOrderHeader** tabulky v ukázkové databáze AdventureWorksLT hello.</span><span class="sxs-lookup"><span data-stu-id="637d8-112">This topic demonstrates a Transact-SQL command that executes a bulk cleanup operation in hello **SalesOrderHeader** table in hello AdventureWorksLT sample database.</span></span> <span data-ttu-id="637d8-113">ukázkové databáze AdventureWorksLT toocreate hello dokončení hello kroky v tématu hello [vytvoření Azure SQL database v hello portál Azure](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="637d8-113">toocreate hello AdventureWorksLT sample database, complete hello steps in hello topic [Create an Azure SQL database in hello Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span> 

## <a name="get-connection-information"></a><span data-ttu-id="637d8-114">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="637d8-114">Get connection information</span></span>

<span data-ttu-id="637d8-115">Je třeba tooget hello připojovacího řetězce pro databázi hello jste vytvořili, když jste dokončili [vytvoření Azure SQL database v hello portál Azure](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="637d8-115">You need tooget hello connection string for hello database you created when you completed [Create an Azure SQL database in hello Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span>

1. <span data-ttu-id="637d8-116">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="637d8-116">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
 
3. <span data-ttu-id="637d8-117">Vyberte **databází SQL** z nabídky na levé straně hello a vyberte svou databázi na hello **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="637d8-117">Select **SQL Databases** from hello left-hand menu, and select your database on hello **SQL databases** page.</span></span>

4. <span data-ttu-id="637d8-118">Vyberte **zobrazit databázové připojovací řetězce** a dokončení kopírování hello **ADO.NET** připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="637d8-118">Select **Show database connection strings** and copy hello complete **ADO.NET** connection string.</span></span>

    ![Zkopírujte připojovací řetězec ADO.NET hello.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-hello-connection-string"></a><span data-ttu-id="637d8-120">Nastavit hello připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="637d8-120">Set hello connection string</span></span> 

<span data-ttu-id="637d8-121">Funkce aplikace hostuje hello provádění funkcí v Azure.</span><span class="sxs-lookup"><span data-stu-id="637d8-121">A function app hosts hello execution of your functions in Azure.</span></span> <span data-ttu-id="637d8-122">Je nejlepší postup toostore připojovací řetězce a jiné tajné klíče v aplikaci nastavení funkce.</span><span class="sxs-lookup"><span data-stu-id="637d8-122">It is a best practice toostore connection strings and other secrets in your function app settings.</span></span> <span data-ttu-id="637d8-123">Pomocí nastavení aplikace zabraňuje náhodného zpřístupnění hello připojovací řetězec s vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="637d8-123">Using application settings prevents accidental disclosure of hello connection string with your code.</span></span> 

1. <span data-ttu-id="637d8-124">Přejděte jste vytvořili aplikaci funkce tooyour [vytvořit funkci v Azure, který je aktivován časovač](functions-create-scheduled-function.md).</span><span class="sxs-lookup"><span data-stu-id="637d8-124">Navigate tooyour function app you created [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md).</span></span>

2. <span data-ttu-id="637d8-125">Vyberte **funkce** > **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="637d8-125">Select **Platform features** > **Application settings**.</span></span>
   
    ![Nastavení aplikace pro funkce aplikace hello.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. <span data-ttu-id="637d8-127">Posuňte se dolů příliš**připojovací řetězce** a přidat připojovací řetězec pomocí nastavení hello uvedených v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="637d8-127">Scroll down too**Connection strings** and add a connection string using hello settings as specified in hello table.</span></span>
   
    ![Přidáte nastavení aplikace toohello funkce připojovacího řetězce.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | <span data-ttu-id="637d8-129">Nastavení</span><span class="sxs-lookup"><span data-stu-id="637d8-129">Setting</span></span>       | <span data-ttu-id="637d8-130">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="637d8-130">Suggested value</span></span> | <span data-ttu-id="637d8-131">Popis</span><span class="sxs-lookup"><span data-stu-id="637d8-131">Description</span></span>             | 
    | ------------ | ------------------ | --------------------- | 
    | <span data-ttu-id="637d8-132">**Název**</span><span class="sxs-lookup"><span data-stu-id="637d8-132">**Name**</span></span>  |  <span data-ttu-id="637d8-133">sqldb_connection</span><span class="sxs-lookup"><span data-stu-id="637d8-133">sqldb_connection</span></span>  | <span data-ttu-id="637d8-134">Použít tooaccess hello uložené připojovací řetězec v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="637d8-134">Used tooaccess hello stored connection string in your function code.</span></span>    |
    | <span data-ttu-id="637d8-135">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="637d8-135">**Value**</span></span> | <span data-ttu-id="637d8-136">Řetězec zkopírovaný</span><span class="sxs-lookup"><span data-stu-id="637d8-136">Copied string</span></span>  | <span data-ttu-id="637d8-137">Po hello připojovací řetězec jste zkopírovali v předchozím oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="637d8-137">Past hello connection string you copied in hello previous section.</span></span> |
    | <span data-ttu-id="637d8-138">**Typ**</span><span class="sxs-lookup"><span data-stu-id="637d8-138">**Type**</span></span> | <span data-ttu-id="637d8-139">SQL Database</span><span class="sxs-lookup"><span data-stu-id="637d8-139">SQL Database</span></span> | <span data-ttu-id="637d8-140">Pomocí připojení k databázi SQL výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="637d8-140">Use hello default SQL Database connection.</span></span> |   

3. <span data-ttu-id="637d8-141">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="637d8-141">Click **Save**.</span></span>

<span data-ttu-id="637d8-142">Teď můžete přidat hello C# funkce kód, který připojí tooyour databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="637d8-142">Now, you can add hello C# function code that connects tooyour SQL Database.</span></span>

## <a name="update-your-function-code"></a><span data-ttu-id="637d8-143">Aktualizace kódu – funkce</span><span class="sxs-lookup"><span data-stu-id="637d8-143">Update your function code</span></span>

1. <span data-ttu-id="637d8-144">V aplikaci funkce vyberte funkce aktivovaného časovačem hello.</span><span class="sxs-lookup"><span data-stu-id="637d8-144">In your function app, select hello timer-triggered function.</span></span>
 
3. <span data-ttu-id="637d8-145">Přidejte následující odkazy na sestavení v horní části hello hello existující funkce kódu hello:</span><span class="sxs-lookup"><span data-stu-id="637d8-145">Add hello following assembly references at hello top of hello existing function code:</span></span>

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. <span data-ttu-id="637d8-146">Přidejte následující hello `using` funkce toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="637d8-146">Add hello following `using` statements toohello function:</span></span>
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. <span data-ttu-id="637d8-147">Nahradit stávající hello **spustit** funkce s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="637d8-147">Replace hello existing **Run** function with hello following code:</span></span>
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

    <span data-ttu-id="637d8-148">Tento ukázkový příkaz aktualizuje hello **stav** sloupec založen na datum expedice hello.</span><span class="sxs-lookup"><span data-stu-id="637d8-148">This sample command updates hello **Status** column based on hello ship date.</span></span> <span data-ttu-id="637d8-149">32 řádků dat je by měl aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="637d8-149">It should update 32 rows of data.</span></span>

5. <span data-ttu-id="637d8-150">Klikněte na tlačítko **Uložit**, sledovat hello **protokoly** windows pro hello další funkce spuštění a pak hello počet řádků v hello aktualizována Poznámka: **SalesOrderHeader** tabulky.</span><span class="sxs-lookup"><span data-stu-id="637d8-150">Click **Save**, watch hello **Logs** windows for hello next function execution, then note hello number of rows updated in hello **SalesOrderHeader** table.</span></span>

    ![Zobrazit protokoly funkcí hello.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a><span data-ttu-id="637d8-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="637d8-152">Next steps</span></span>

<span data-ttu-id="637d8-153">Dále se naučíte, jak funguje toouse s Logic Apps toointegrate s jinými službami.</span><span class="sxs-lookup"><span data-stu-id="637d8-153">Next, learn how toouse Functions with Logic Apps toointegrate with other services.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="637d8-154">Vytvoří funkci, která se integruje s Logic Apps</span><span class="sxs-lookup"><span data-stu-id="637d8-154">Create a function that integrates with Logic Apps</span></span>](functions-twitter-email.md)

<span data-ttu-id="637d8-155">Další informace o funkcích najdete v tématu hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="637d8-155">For more information about Functions, see hello following topics:</span></span>

* [<span data-ttu-id="637d8-156">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="637d8-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="637d8-157">Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="637d8-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="637d8-158">Testování Azure Functions</span><span class="sxs-lookup"><span data-stu-id="637d8-158">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="637d8-159">Toto téma popisuje různé nástroje a techniky pro testování funkcí.</span><span class="sxs-lookup"><span data-stu-id="637d8-159">Describes various tools and techniques for testing your functions.</span></span>  
