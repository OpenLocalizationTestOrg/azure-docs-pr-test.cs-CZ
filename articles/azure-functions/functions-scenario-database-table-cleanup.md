---
title: "Pomocí Azure Functions provádět databázi úlohy čištění | Microsoft Docs"
description: "Pomocí Azure Functions můžete naplánovat úlohu, která se připojuje k databázi SQL Azure a pravidelně čistí řádky."
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
ms.openlocfilehash: 6fd0e32374827b249f5aba1cbfc39117c88c6272
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-functions-to-connect-to-an-azure-sql-database"></a><span data-ttu-id="1a99c-103">Používat Azure Functions k připojení k databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="1a99c-103">Use Azure Functions to connect to an Azure SQL Database</span></span>
<span data-ttu-id="1a99c-104">Toto téma ukazuje, jak používat Azure Functions vytvořit naplánovanou úlohu, která vyčistí řádky v tabulce v Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1a99c-104">This topic shows you how to use Azure Functions to create a scheduled job that cleans up rows in a table in an Azure SQL Database.</span></span> <span data-ttu-id="1a99c-105">Nové funkce jazyka C# se vytvoří na základě šablony aktivační událost časovače předem definovaných na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1a99c-105">The new C# function is created based on a pre-defined timer trigger template in the Azure portal.</span></span> <span data-ttu-id="1a99c-106">Pro podporu tohoto scénáře, musíte taky nastavit připojovací řetězec databáze jako nastavení v aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="1a99c-106">To support this scenario, you must also set a database connection string as a setting in the function app.</span></span> <span data-ttu-id="1a99c-107">Tento scénář používá hromadné operace v databázi.</span><span class="sxs-lookup"><span data-stu-id="1a99c-107">This scenario uses a bulk operation against the database.</span></span> <span data-ttu-id="1a99c-108">Pokud chcete, aby funkce zpracovat jednotlivé operace CRUD v tabulce Mobile Apps, měli byste místo toho použít [Mobile Apps vazby](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="1a99c-108">To have your function process individual CRUD operations in a Mobile Apps table, you should instead use [Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a99c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1a99c-109">Prerequisites</span></span>

+ <span data-ttu-id="1a99c-110">Toto téma používá funkci spustí časovač.</span><span class="sxs-lookup"><span data-stu-id="1a99c-110">This topic uses a timer triggered function.</span></span> <span data-ttu-id="1a99c-111">Proveďte kroky v tématu [vytvořit funkci v Azure, který je aktivován časovač](functions-create-scheduled-function.md) verzi jazyka C# této funkce.</span><span class="sxs-lookup"><span data-stu-id="1a99c-111">Complete the steps in the topic [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md) to create a C# version of this function.</span></span>   

+ <span data-ttu-id="1a99c-112">Příkaz Transact-SQL, který provádí hromadné operace čištění v tomto tématu se dozvíte **SalesOrderHeader** tabulky v ukázkové databáze AdventureWorksLT.</span><span class="sxs-lookup"><span data-stu-id="1a99c-112">This topic demonstrates a Transact-SQL command that executes a bulk cleanup operation in the **SalesOrderHeader** table in the AdventureWorksLT sample database.</span></span> <span data-ttu-id="1a99c-113">K vytvoření ukázkové databáze AdventureWorksLT, proveďte kroky v tématu [vytvoření Azure SQL database na portálu Azure](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1a99c-113">To create the AdventureWorksLT sample database, complete the steps in the topic [Create an Azure SQL database in the Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span> 

## <a name="get-connection-information"></a><span data-ttu-id="1a99c-114">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="1a99c-114">Get connection information</span></span>

<span data-ttu-id="1a99c-115">Je nutné získat připojovací řetězec pro databázi, který jste vytvořili, když jste dokončili [vytvoření Azure SQL database na portálu Azure](../sql-database/sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1a99c-115">You need to get the connection string for the database you created when you completed [Create an Azure SQL database in the Azure portal](../sql-database/sql-database-get-started-portal.md).</span></span>

1. <span data-ttu-id="1a99c-116">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1a99c-116">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
 
3. <span data-ttu-id="1a99c-117">Vyberte **databází SQL** z nabídky na levé straně a vyberte svou databázi na **databází SQL** stránky.</span><span class="sxs-lookup"><span data-stu-id="1a99c-117">Select **SQL Databases** from the left-hand menu, and select your database on the **SQL databases** page.</span></span>

4. <span data-ttu-id="1a99c-118">Vyberte **zobrazit databázové připojovací řetězce** a zkopírujte kompletní **ADO.NET** připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="1a99c-118">Select **Show database connection strings** and copy the complete **ADO.NET** connection string.</span></span>

    ![Zkopírujte připojovací řetězec ADO.NET.](./media/functions-scenario-database-table-cleanup/adonet-connection-string.png)

## <a name="set-the-connection-string"></a><span data-ttu-id="1a99c-120">Nastavení připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="1a99c-120">Set the connection string</span></span> 

<span data-ttu-id="1a99c-121">Provádění funkcí v Azure je hostováno v aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="1a99c-121">A function app hosts the execution of your functions in Azure.</span></span> <span data-ttu-id="1a99c-122">Je osvědčeným postupem k ukládání připojovacích řetězců a jiné tajné v aplikaci nastavení funkce.</span><span class="sxs-lookup"><span data-stu-id="1a99c-122">It is a best practice to store connection strings and other secrets in your function app settings.</span></span> <span data-ttu-id="1a99c-123">Pomocí nastavení aplikace zabraňuje náhodného zpřístupnění připojovacího řetězce s vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="1a99c-123">Using application settings prevents accidental disclosure of the connection string with your code.</span></span> 

1. <span data-ttu-id="1a99c-124">Přejděte do aplikace funkce, který jste vytvořili [vytvořit funkci v Azure, který je aktivován časovač](functions-create-scheduled-function.md).</span><span class="sxs-lookup"><span data-stu-id="1a99c-124">Navigate to your function app you created [Create a function in Azure that is triggered by a timer](functions-create-scheduled-function.md).</span></span>

2. <span data-ttu-id="1a99c-125">Vyberte **funkce** > **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1a99c-125">Select **Platform features** > **Application settings**.</span></span>
   
    ![Nastavení aplikace pro funkce aplikace.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings.png)

2. <span data-ttu-id="1a99c-127">Přejděte dolů k položce **připojovací řetězce** a přidat připojovací řetězec pomocí nastavení uvedeného v tabulce.</span><span class="sxs-lookup"><span data-stu-id="1a99c-127">Scroll down to **Connection strings** and add a connection string using the settings as specified in the table.</span></span>
   
    ![Nastavení funkce aplikace přidáte připojovací řetězec.](./media/functions-scenario-database-table-cleanup/functions-app-service-settings-connection-strings.png)

    | <span data-ttu-id="1a99c-129">Nastavení</span><span class="sxs-lookup"><span data-stu-id="1a99c-129">Setting</span></span>       | <span data-ttu-id="1a99c-130">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="1a99c-130">Suggested value</span></span> | <span data-ttu-id="1a99c-131">Popis</span><span class="sxs-lookup"><span data-stu-id="1a99c-131">Description</span></span>             | 
    | ------------ | ------------------ | --------------------- | 
    | <span data-ttu-id="1a99c-132">**Název**</span><span class="sxs-lookup"><span data-stu-id="1a99c-132">**Name**</span></span>  |  <span data-ttu-id="1a99c-133">sqldb_connection</span><span class="sxs-lookup"><span data-stu-id="1a99c-133">sqldb_connection</span></span>  | <span data-ttu-id="1a99c-134">Slouží k přístupu uložené připojovací řetězec v kódu funkce.</span><span class="sxs-lookup"><span data-stu-id="1a99c-134">Used to access the stored connection string in your function code.</span></span>    |
    | <span data-ttu-id="1a99c-135">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="1a99c-135">**Value**</span></span> | <span data-ttu-id="1a99c-136">Řetězec zkopírovaný</span><span class="sxs-lookup"><span data-stu-id="1a99c-136">Copied string</span></span>  | <span data-ttu-id="1a99c-137">Po připojovací řetězec jste zkopírovali v předchozím oddílu.</span><span class="sxs-lookup"><span data-stu-id="1a99c-137">Past the connection string you copied in the previous section.</span></span> |
    | <span data-ttu-id="1a99c-138">**Typ**</span><span class="sxs-lookup"><span data-stu-id="1a99c-138">**Type**</span></span> | <span data-ttu-id="1a99c-139">SQL Database</span><span class="sxs-lookup"><span data-stu-id="1a99c-139">SQL Database</span></span> | <span data-ttu-id="1a99c-140">Použijte výchozí připojení k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="1a99c-140">Use the default SQL Database connection.</span></span> |   

3. <span data-ttu-id="1a99c-141">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1a99c-141">Click **Save**.</span></span>

<span data-ttu-id="1a99c-142">Teď můžete přidat funkce kódu C#, která se připojuje k vaší databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="1a99c-142">Now, you can add the C# function code that connects to your SQL Database.</span></span>

## <a name="update-your-function-code"></a><span data-ttu-id="1a99c-143">Aktualizace kódu – funkce</span><span class="sxs-lookup"><span data-stu-id="1a99c-143">Update your function code</span></span>

1. <span data-ttu-id="1a99c-144">V aplikaci funkce vyberte funkce spustí časovač.</span><span class="sxs-lookup"><span data-stu-id="1a99c-144">In your function app, select the timer-triggered function.</span></span>
 
3. <span data-ttu-id="1a99c-145">Přidejte následující odkazy na sestavení v horní části existující kód funkce:</span><span class="sxs-lookup"><span data-stu-id="1a99c-145">Add the following assembly references at the top of the existing function code:</span></span>

    ```cs
    #r "System.Configuration"
    #r "System.Data"
    ```

3. <span data-ttu-id="1a99c-146">Přidejte následující `using` příkazy funkce:</span><span class="sxs-lookup"><span data-stu-id="1a99c-146">Add the following `using` statements to the function:</span></span>
    ```cs
    using System.Configuration;
    using System.Data.SqlClient;
    using System.Threading.Tasks;
    ```

4. <span data-ttu-id="1a99c-147">Nahradit existující **spustit** funkce následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1a99c-147">Replace the existing **Run** function with the following code:</span></span>
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
                // Execute the command and log the # rows affected.
                var rows = await cmd.ExecuteNonQueryAsync();
                log.Info($"{rows} rows were updated");
            }
        }
    }
    ```

    <span data-ttu-id="1a99c-148">Tento ukázkový příkaz aktualizuje **stav** sloupec založen na datum expedice.</span><span class="sxs-lookup"><span data-stu-id="1a99c-148">This sample command updates the **Status** column based on the ship date.</span></span> <span data-ttu-id="1a99c-149">32 řádků dat je by měl aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="1a99c-149">It should update 32 rows of data.</span></span>

5. <span data-ttu-id="1a99c-150">Klikněte na tlačítko **Uložit**, sledovat **protokoly** windows pro další funkce spuštění a pak Poznámka: počet řádků v aktualizován **SalesOrderHeader** tabulky.</span><span class="sxs-lookup"><span data-stu-id="1a99c-150">Click **Save**, watch the **Logs** windows for the next function execution, then note the number of rows updated in the **SalesOrderHeader** table.</span></span>

    ![Zobrazte protokoly funkcí.](./media/functions-scenario-database-table-cleanup/functions-logs.png)

## <a name="next-steps"></a><span data-ttu-id="1a99c-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a99c-152">Next steps</span></span>

<span data-ttu-id="1a99c-153">V dalším kroku Další informace o použití funkce s Logic Apps pro integraci s jinými službami.</span><span class="sxs-lookup"><span data-stu-id="1a99c-153">Next, learn how to use Functions with Logic Apps to integrate with other services.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="1a99c-154">Vytvoří funkci, která se integruje s Logic Apps</span><span class="sxs-lookup"><span data-stu-id="1a99c-154">Create a function that integrates with Logic Apps</span></span>](functions-twitter-email.md)

<span data-ttu-id="1a99c-155">Další informace o funkcích najdete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="1a99c-155">For more information about Functions, see the following topics:</span></span>

* [<span data-ttu-id="1a99c-156">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="1a99c-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="1a99c-157">Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.</span><span class="sxs-lookup"><span data-stu-id="1a99c-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="1a99c-158">Testování Azure Functions</span><span class="sxs-lookup"><span data-stu-id="1a99c-158">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="1a99c-159">Toto téma popisuje různé nástroje a techniky pro testování funkcí.</span><span class="sxs-lookup"><span data-stu-id="1a99c-159">Describes various tools and techniques for testing your functions.</span></span>  
