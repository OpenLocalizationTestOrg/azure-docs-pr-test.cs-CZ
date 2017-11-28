---
title: "konektor Azure SQL Database hello aaaAdd ve vašich Logic Apps | Microsoft Docs"
description: "Přehled konektoru Azure SQL Database s parametry rozhraní REST API"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a9ca0f446d05dc00f310a908eee8d50e41fcd82b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-azure-sql-database-connector"></a><span data-ttu-id="77648-103">Začínáme s Azure SQL Database konektor hello</span><span class="sxs-lookup"><span data-stu-id="77648-103">Get started with hello Azure SQL Database connector</span></span>
<span data-ttu-id="77648-104">Pomocí konektoru Azure SQL Database hello vytvořte pracovní postupy pro vaši organizaci, které spravují data v tabulkách.</span><span class="sxs-lookup"><span data-stu-id="77648-104">Using hello Azure SQL Database connector, create workflows for your organization that manage data in your tables.</span></span> 

<span data-ttu-id="77648-105">S databází SQL můžete:</span><span class="sxs-lookup"><span data-stu-id="77648-105">With SQL Database, you:</span></span>

* <span data-ttu-id="77648-106">Vytvořte pracovní postup přidávání nové databáze zákazníci tooa zákazníka nebo aktualizaci pořadí, v databázi objednávky.</span><span class="sxs-lookup"><span data-stu-id="77648-106">Build your workflow by adding a new customer tooa customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="77648-107">Pomocí akce tooget řádek dat, vložit nový řádek a ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="77648-107">Use actions tooget a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="77648-108">Například v aplikaci Dynamics CRM Online (aktivační událost) je vytvořen záznam, pak vložte řádek v Azure SQL Database (akce).</span><span class="sxs-lookup"><span data-stu-id="77648-108">For example,  when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Azure SQL Database (an action).</span></span> 

<span data-ttu-id="77648-109">Toto téma ukazuje, jak toouse hello konektor databáze SQL v aplikaci logiky, a také seznamy hello akce.</span><span class="sxs-lookup"><span data-stu-id="77648-109">This topic shows you how toouse hello SQL Database connector in a logic app, and also lists hello actions.</span></span>

<span data-ttu-id="77648-110">toolearn Další informace o Logic Apps, najdete v části [co jsou logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="77648-110">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooazure-sql-database"></a><span data-ttu-id="77648-111">Připojit tooAzure databáze SQL</span><span class="sxs-lookup"><span data-stu-id="77648-111">Connect tooAzure SQL Database</span></span>
<span data-ttu-id="77648-112">Než se aplikace logiky k jakékoli služby, nejprve vytvořit *připojení* toohello služby.</span><span class="sxs-lookup"><span data-stu-id="77648-112">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="77648-113">Připojení poskytuje připojení mezi aplikace logiky a jiné služby.</span><span class="sxs-lookup"><span data-stu-id="77648-113">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="77648-114">Například tooconnect tooSQL databáze, je nejprve vytvořit databázi SQL *připojení*.</span><span class="sxs-lookup"><span data-stu-id="77648-114">For example, tooconnect tooSQL Database, you first create a SQL Database *connection*.</span></span> <span data-ttu-id="77648-115">toocreate připojení, zadejte přihlašovací údaje hello normálně používat tooaccess hello služby, ke kterému se připojujete.</span><span class="sxs-lookup"><span data-stu-id="77648-115">toocreate a connection, you enter hello credentials you normally use tooaccess hello service you are connecting to.</span></span> <span data-ttu-id="77648-116">Ano v databázi SQL, zadejte vaše toocreate hello připojení k databázi SQL přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="77648-116">So, in SQL Database, enter your SQL Database credentials toocreate hello connection.</span></span> 

#### <a name="create-hello-connection"></a><span data-ttu-id="77648-117">Vytvoření připojení hello</span><span class="sxs-lookup"><span data-stu-id="77648-117">Create hello connection</span></span>
> [!INCLUDE [Create hello connection tooSQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="77648-118">Použít aktivační událost</span><span class="sxs-lookup"><span data-stu-id="77648-118">Use a trigger</span></span>
<span data-ttu-id="77648-119">Tento konektor nemá žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="77648-119">This connector does not have any triggers.</span></span> <span data-ttu-id="77648-120">Použijte jiné aktivační události toostart hello aplikace logiky, jako jsou aktivační událost opakování, aktivační události Webhooku protokolu HTTP, aktivační události, které jsou k dispozici ostatní konektory a další.</span><span class="sxs-lookup"><span data-stu-id="77648-120">Use other triggers toostart hello logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="77648-121">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) poskytuje příklad.</span><span class="sxs-lookup"><span data-stu-id="77648-121">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="77648-122">Použít akci.</span><span class="sxs-lookup"><span data-stu-id="77648-122">Use an action</span></span>
<span data-ttu-id="77648-123">Akce je operace, provádí v pracovním postupu hello definované v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="77648-123">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="77648-124">[Další informace o akcích](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="77648-124">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="77648-125">Vyberte znaménko plus hello.</span><span class="sxs-lookup"><span data-stu-id="77648-125">Select hello plus sign.</span></span> <span data-ttu-id="77648-126">Zobrazí několik možností: **přidat akci**, **přidat podmínku**, nebo jeden z hello **Další** možnosti.</span><span class="sxs-lookup"><span data-stu-id="77648-126">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. <span data-ttu-id="77648-127">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="77648-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="77648-128">Hello textového pole zadejte "sql" tooget seznam všechny dostupné akce hello.</span><span class="sxs-lookup"><span data-stu-id="77648-128">In hello text box, type “sql” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. <span data-ttu-id="77648-129">V našem příkladu zvolte **systému SQL Server - Get řádek**.</span><span class="sxs-lookup"><span data-stu-id="77648-129">In our example, choose **SQL Server - Get row**.</span></span> <span data-ttu-id="77648-130">Pokud připojení již existuje, pak vyberte hello **název tabulky** z rozevíracího seznamu hello seznamu a zadejte hello **ID řádku** chcete tooreturn.</span><span class="sxs-lookup"><span data-stu-id="77648-130">If a connection already exists, then select hello **Table name** from hello drop-down list, and enter hello **Row ID** you want tooreturn.</span></span>
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    <span data-ttu-id="77648-131">Pokud budete vyzváni k hello informace o připojení, zadejte hello podrobnosti toocreate hello připojení.</span><span class="sxs-lookup"><span data-stu-id="77648-131">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="77648-132">[Vytvoření připojení hello](connectors-create-api-sqlazure.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="77648-132">[Create hello connection](connectors-create-api-sqlazure.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="77648-133">V tomto příkladu jsme vrátí řádek z tabulky.</span><span class="sxs-lookup"><span data-stu-id="77648-133">In this example, we return a row from a table.</span></span> <span data-ttu-id="77648-134">toosee hello data v tomto řádku přidat akci, která se vytvoří soubor pomocí hello polí z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="77648-134">toosee hello data in this row, add another action that creates a file using hello fields from hello table.</span></span> <span data-ttu-id="77648-135">Například přidejte OneDrive akci, která používá hello FirstName a LastName pole toocreate nový soubor v hello účet cloudového úložiště.</span><span class="sxs-lookup"><span data-stu-id="77648-135">For example, add a OneDrive action that uses hello FirstName and LastName fields toocreate a new file in hello cloud storage account.</span></span> 
   > 
   > 
5. <span data-ttu-id="77648-136">**Uložit** změny (levém horním rohu hello panelu nástrojů).</span><span class="sxs-lookup"><span data-stu-id="77648-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="77648-137">Aplikace logiky je uloženo a automaticky povolí.</span><span class="sxs-lookup"><span data-stu-id="77648-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="77648-138">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="77648-138">Connector-specific details</span></span>

<span data-ttu-id="77648-139">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/sql/).</span><span class="sxs-lookup"><span data-stu-id="77648-139">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sql/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="77648-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77648-140">Next steps</span></span>
<span data-ttu-id="77648-141">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="77648-141">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="77648-142">Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="77648-142">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

