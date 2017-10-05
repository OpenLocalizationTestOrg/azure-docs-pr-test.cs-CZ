---
title: "Přidejte konektor databáze Oracle do Azure Logic Apps | Microsoft Docs"
description: "Použití konektoru databáze Oracle v aplikaci logiky"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: mandia; ladocs
ms.openlocfilehash: cc64441617eb5e7d5e70c1cf5c491a672428bc51
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-the-oracle-database-connector"></a><span data-ttu-id="57c75-103">Začínáme s konektorem databáze Oracle</span><span class="sxs-lookup"><span data-stu-id="57c75-103">Get started with the Oracle Database connector</span></span>

<span data-ttu-id="57c75-104">Pomocí konektoru databáze Oracle, vytvoření organizační pracovní postupy, které pomocí data v existující databázi.</span><span class="sxs-lookup"><span data-stu-id="57c75-104">Using the Oracle Database connector, you create organizational workflows that use data in your existing database.</span></span> <span data-ttu-id="57c75-105">Tento konektor se může připojit k databázi Oracle místní, nebo virtuální počítač Azure s nainstalovaná databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="57c75-105">This connector can connect to an on-premises Oracle Database, or an Azure virtual machine with Oracle Database installed.</span></span> <span data-ttu-id="57c75-106">Tento konektor můžete:</span><span class="sxs-lookup"><span data-stu-id="57c75-106">With this connector, you can:</span></span>

* <span data-ttu-id="57c75-107">Vytvořte pracovní postup přidáním nového zákazníka k databázi zákazníků nebo aktualizaci pořadí, v databázi objednávky.</span><span class="sxs-lookup"><span data-stu-id="57c75-107">Build your workflow by adding a new customer to a customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="57c75-108">Pomocí akcí pro získání řádku dat, vložit nový řádek a i odstranění.</span><span class="sxs-lookup"><span data-stu-id="57c75-108">Use actions to get a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="57c75-109">Například v aplikaci Dynamics CRM Online (aktivační událost) je vytvořen záznam, pak vložte řádek v databázi Oracle (akce).</span><span class="sxs-lookup"><span data-stu-id="57c75-109">For example, when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Oracle Database (an action).</span></span> 

<span data-ttu-id="57c75-110">Toto téma ukazuje, jak používat konektor databáze Oracle v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="57c75-110">This topic shows you how to use the Oracle Database connector in a logic app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57c75-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="57c75-111">Prerequisites</span></span>

* <span data-ttu-id="57c75-112">Podporované verze Oracle:</span><span class="sxs-lookup"><span data-stu-id="57c75-112">Supported Oracle versions:</span></span> 
    * <span data-ttu-id="57c75-113">Oracle 9 a novějším</span><span class="sxs-lookup"><span data-stu-id="57c75-113">Oracle 9 and later</span></span>
    * <span data-ttu-id="57c75-114">Klientský software Oracle 8.1.7 a novější</span><span class="sxs-lookup"><span data-stu-id="57c75-114">Oracle client software 8.1.7 and later</span></span>

* <span data-ttu-id="57c75-115">Nainstalujte bránu dat na místě.</span><span class="sxs-lookup"><span data-stu-id="57c75-115">Install the on-premises data gateway.</span></span> <span data-ttu-id="57c75-116">[Připojit k místním datům z aplikace logiky](../logic-apps/logic-apps-gateway-connection.md) jsou uvedené kroky.</span><span class="sxs-lookup"><span data-stu-id="57c75-116">[Connect to on-premises data from logic apps](../logic-apps/logic-apps-gateway-connection.md) lists the steps.</span></span> <span data-ttu-id="57c75-117">Brána je vyžadován pro připojení k databázi Oracle místní, nebo nainstalovat virtuální počítač Azure s databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="57c75-117">The gateway is required to connect to an on-premises Oracle Database, or an Azure VM with Oracle DB installed.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="57c75-118">Bránu dat místní funguje jako mostu a poskytuje přenos zabezpečených dat mezi místními daty (data, která není v cloudu) a aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="57c75-118">The on-premises data gateway acts as a bridge, and provides a secure data transfer between on-premises data (data that is not in the cloud) and your logic apps.</span></span> <span data-ttu-id="57c75-119">S více služeb a více zdrojů dat lze použít stejné bráně.</span><span class="sxs-lookup"><span data-stu-id="57c75-119">The same gateway can be used with multiple services, and multiple data sources.</span></span> <span data-ttu-id="57c75-120">Ano jenom musíte nainstalovat jednou bránu.</span><span class="sxs-lookup"><span data-stu-id="57c75-120">So, you may only need to install the gateway once.</span></span>

* <span data-ttu-id="57c75-121">Na počítači, kam jste nainstalovali bránu dat na místě instalace klienta Oracle.</span><span class="sxs-lookup"><span data-stu-id="57c75-121">Install the Oracle Client on the machine where you installed the on-premises data gateway.</span></span> <span data-ttu-id="57c75-122">Nezapomeňte nainstalovat 64-bit poskytovatele dat Oracle pro .NET z databáze Oracle:</span><span class="sxs-lookup"><span data-stu-id="57c75-122">Be sure to install the 64-bit Oracle Data Provider for .NET from Oracle:</span></span>  

  [<span data-ttu-id="57c75-123">64-bit ODAC 12c verze 4 (12.1.0.2.4) pro Windows x64</span><span class="sxs-lookup"><span data-stu-id="57c75-123">64-bit ODAC 12c Release 4 (12.1.0.2.4) for Windows x64</span></span>](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > <span data-ttu-id="57c75-124">Pokud není nainstalován Klient Oracle, dojde k chybě při pokusu o vytvoření nebo použití připojení.</span><span class="sxs-lookup"><span data-stu-id="57c75-124">If the Oracle client is not installed, an error occurs when you try to create or use the connection.</span></span> <span data-ttu-id="57c75-125">Najdete běžné chyby v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="57c75-125">See the common errors in this topic.</span></span>


## <a name="add-the-connector"></a><span data-ttu-id="57c75-126">Přidejte konektor</span><span class="sxs-lookup"><span data-stu-id="57c75-126">Add the connector</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57c75-127">Tento konektor nemá žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="57c75-127">This connector does not have any triggers.</span></span> <span data-ttu-id="57c75-128">Má pouze akce.</span><span class="sxs-lookup"><span data-stu-id="57c75-128">It has only actions.</span></span> <span data-ttu-id="57c75-129">Proto při vytváření aplikace logiky, přidejte další aktivační události spusťte aplikaci logiky, jako **plán - opakování**, nebo **požadavku nebo odpovědi - odpovědi**.</span><span class="sxs-lookup"><span data-stu-id="57c75-129">So when you create your logic app, add another trigger to start your logic app, such as **Schedule - Recurrence**, or **Request / Response - Response**.</span></span> 

1. <span data-ttu-id="57c75-130">V [portál Azure](https://portal.azure.com), vytvoření aplikace logiky prázdné.</span><span class="sxs-lookup"><span data-stu-id="57c75-130">In the [Azure portal](https://portal.azure.com), create a blank logic app.</span></span>

2. <span data-ttu-id="57c75-131">Na začátku aplikace logiky, vyberte **požadavku nebo odpovědi - požadavek** aktivační události:</span><span class="sxs-lookup"><span data-stu-id="57c75-131">At the start of your logic app, select the **Request / Response - Request** trigger:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. <span data-ttu-id="57c75-132">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="57c75-132">Select **Save**.</span></span> <span data-ttu-id="57c75-133">Při ukládání, se automaticky vygeneroval adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="57c75-133">When you save, a request URL is automatically generated.</span></span> 

4. <span data-ttu-id="57c75-134">Vyberte **nový krok**a vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="57c75-134">Select **New step**, and select **Add an action**.</span></span> <span data-ttu-id="57c75-135">Zadejte `oracle` zobrazte dostupné akce:</span><span class="sxs-lookup"><span data-stu-id="57c75-135">Type in `oracle` to see the available actions:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > <span data-ttu-id="57c75-136">Toto je také nejrychlejší způsob, jak zobrazit triggery a akce, které jsou k dispozici pro všechny konektory.</span><span class="sxs-lookup"><span data-stu-id="57c75-136">This is also the quickest way to see the triggers and actions available for any connector.</span></span> <span data-ttu-id="57c75-137">Zadejte v části název konektoru, například `oracle`.</span><span class="sxs-lookup"><span data-stu-id="57c75-137">Type in part of the connector name, such as `oracle`.</span></span> <span data-ttu-id="57c75-138">Návrhář uvádí žádné aktivační události a všechny akce.</span><span class="sxs-lookup"><span data-stu-id="57c75-138">The designer lists any triggers and any actions.</span></span> 

5. <span data-ttu-id="57c75-139">Vyberte jednu z akcí, jako například **Oracle Database - Get řádek**.</span><span class="sxs-lookup"><span data-stu-id="57c75-139">Select one of the actions, such as **Oracle Database - Get row**.</span></span> <span data-ttu-id="57c75-140">Vyberte **připojit prostřednictvím místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="57c75-140">Select **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="57c75-141">Zadejte název serveru Oracle, metodu ověřování, uživatelské jméno, heslo a vyberte bránu:</span><span class="sxs-lookup"><span data-stu-id="57c75-141">Enter the Oracle server name, authentication method, username, password, and select the gateway:</span></span>

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. <span data-ttu-id="57c75-142">Po připojení, vyberte tabulku ze seznamu a zadejte ID řádek do tabulky.</span><span class="sxs-lookup"><span data-stu-id="57c75-142">Once connected, select a table from the list, and enter the row ID to your table.</span></span> <span data-ttu-id="57c75-143">Je třeba vědět identifikátor do tabulky.</span><span class="sxs-lookup"><span data-stu-id="57c75-143">You need to know the identifier to the table.</span></span> <span data-ttu-id="57c75-144">Pokud si nejste jisti, obraťte se na správce databáze Oracle a získat výstup `select * from yourTableName`.</span><span class="sxs-lookup"><span data-stu-id="57c75-144">If you don't know, contact your Oracle DB administrator, and get the output from `select * from yourTableName`.</span></span> <span data-ttu-id="57c75-145">To vám dává osobní údaje, které potřebujete, aby bylo možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="57c75-145">This gives you the identifiable information you need to proceed.</span></span>

    <span data-ttu-id="57c75-146">V následujícím příkladu se vrací data úlohy z databáze lidských zdrojů:</span><span class="sxs-lookup"><span data-stu-id="57c75-146">In the following example, job data is being returned from a Human Resources database:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. <span data-ttu-id="57c75-147">V tomto kroku další všechny ostatní konektory můžete použít k vytvoření pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="57c75-147">In this next step, you can use any of the other connectors to build your workflow.</span></span> <span data-ttu-id="57c75-148">Pokud chcete testovat získávání dat z databáze Oracle, pošlete sami e-mailu s daty Oracle pomocí některého z konektorů e-mailu odesílání takové Office 365 nebo z Gmailu.</span><span class="sxs-lookup"><span data-stu-id="57c75-148">If you want to test getting data from Oracle, then send yourself an email with the Oracle data using one of the send email connectors, such Office 365 or Gmail.</span></span> <span data-ttu-id="57c75-149">Použít k vytvoření dynamických tokeny z tabulky Oracle `Subject` a `Body` vašeho e-mailu:</span><span class="sxs-lookup"><span data-stu-id="57c75-149">Use the dynamic tokens from the Oracle table to build the `Subject` and `Body` of your email:</span></span>

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. <span data-ttu-id="57c75-150">**Uložit** vaší aplikace logiky a potom vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="57c75-150">**Save** your logic app, and then select **Run**.</span></span> <span data-ttu-id="57c75-151">Zavřít návrháře a podívejte se na spuštění historie stavu.</span><span class="sxs-lookup"><span data-stu-id="57c75-151">Close the designer, and look at the runs history for the status.</span></span> <span data-ttu-id="57c75-152">Pokud se nezdaří, vyberte řádek zpráv se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="57c75-152">If it fails, select the failed message row.</span></span> <span data-ttu-id="57c75-153">Návrhář otevře a ukáže vám, který krok se nezdařila a také zobrazuje informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="57c75-153">The designer opens, and shows you which step failed, and also shows the error information.</span></span> <span data-ttu-id="57c75-154">Pokud se aktivace podaří, měli byste obdržet e-mail s informací, které jste přidali.</span><span class="sxs-lookup"><span data-stu-id="57c75-154">If it succeeds, then you should receive an email with the information you added.</span></span>


### <a name="workflow-ideas"></a><span data-ttu-id="57c75-155">Nápady pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="57c75-155">Workflow ideas</span></span>

* <span data-ttu-id="57c75-156">Chcete monitorovat #oracle hashtag a umístí tweetů v databázi, mohou být dotazována a používaných v rámci jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="57c75-156">You want to monitor the #oracle hashtag, and put the tweets in a database so they can be queried, and used within other applications.</span></span> <span data-ttu-id="57c75-157">V aplikaci logiky, přidejte `Twitter - When a new tweet is posted` aktivovat a zadejte **#oracle** hashtag.</span><span class="sxs-lookup"><span data-stu-id="57c75-157">In a logic app, add the `Twitter - When a new tweet is posted` trigger, and enter the **#oracle** hashtag.</span></span> <span data-ttu-id="57c75-158">Poté, přidejte `Oracle Database - Insert row` akce a vyberte tabulku:</span><span class="sxs-lookup"><span data-stu-id="57c75-158">Then, add the `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* <span data-ttu-id="57c75-159">Zprávy jsou odesílány do fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="57c75-159">Messages are sent to a Service Bus queue.</span></span> <span data-ttu-id="57c75-160">Chcete získat tyto zprávy a umístí je do databáze.</span><span class="sxs-lookup"><span data-stu-id="57c75-160">You want to get these messages, and put them in a database.</span></span> <span data-ttu-id="57c75-161">V aplikaci logiky, přidejte `Service Bus - when a message is received in a queue` aktivovat a vyberte frontu.</span><span class="sxs-lookup"><span data-stu-id="57c75-161">In a logic app, add the `Service Bus - when a message is received in a queue` trigger, and select the queue.</span></span> <span data-ttu-id="57c75-162">Poté, přidejte `Oracle Database - Insert row` akce a vyberte tabulku:</span><span class="sxs-lookup"><span data-stu-id="57c75-162">Then, add the `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a><span data-ttu-id="57c75-163">Běžné chyby</span><span class="sxs-lookup"><span data-stu-id="57c75-163">Common errors</span></span>

#### <a name="error-cannot-reach-the-gateway"></a><span data-ttu-id="57c75-164">**Chyba**: Nelze kontaktovat bránu.</span><span class="sxs-lookup"><span data-stu-id="57c75-164">**Error**: Cannot reach the Gateway</span></span>

<span data-ttu-id="57c75-165">**Příčina**: místní data brána není možné se připojit ke cloudu.</span><span class="sxs-lookup"><span data-stu-id="57c75-165">**Cause**: The on-premises data gateway is not able to connect to the cloud.</span></span> 

<span data-ttu-id="57c75-166">**Zmírnění dopadů**: Zkontrolujte, že vaše brána je spuštěný na místním počítači, kde jste ho nainstalovali, a který se může připojit k Internetu.</span><span class="sxs-lookup"><span data-stu-id="57c75-166">**Mitigation**: Make sure your gateway is running on the on-premises machine where you installed it, and that it can connect to the internet.</span></span>  <span data-ttu-id="57c75-167">Doporučujeme není bránu nainstalovat na počítač, který může být vypnutý nebo v režimu spánku.</span><span class="sxs-lookup"><span data-stu-id="57c75-167">We recommend not installing the gateway on a computer that may be turned off or sleep.</span></span> <span data-ttu-id="57c75-168">Můžete také restartovat službu brány dat místní (PBIEgwService).</span><span class="sxs-lookup"><span data-stu-id="57c75-168">You can also restart the on-premises data gateway service (PBIEgwService).</span></span>

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a><span data-ttu-id="57c75-169">**Chyba**: použitý zprostředkovatel je zastaralý: ' System.Data.OracleClient vyžaduje Oracle verze klientského softwaru 8.1.7 nebo vyšší. ".</span><span class="sxs-lookup"><span data-stu-id="57c75-169">**Error**: The provider being used is deprecated: 'System.Data.OracleClient requires Oracle client software version 8.1.7 or greater.'.</span></span> <span data-ttu-id="57c75-170">Navštivte [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) a nainstalujte si oficiálního zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="57c75-170">Please visit [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) to install the official provider.</span></span>

<span data-ttu-id="57c75-171">**Příčina**: Sada SDK není nainstalovaná na počítači, kde je spuštěna místní brána dat klienta Oracle.</span><span class="sxs-lookup"><span data-stu-id="57c75-171">**Cause**: The Oracle client SDK is not installed on the machine where the on-premises data gateway is running.</span></span>  

<span data-ttu-id="57c75-172">**Řešení**: Stáhněte a nainstalujte klienta Oracle SDK na stejném počítači jako místní brána data gateway.</span><span class="sxs-lookup"><span data-stu-id="57c75-172">**Resolution**: Download and install the Oracle client SDK on the same computer as the on-premises data gateway.</span></span>

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a><span data-ttu-id="57c75-173">**Chyba**: Tabulka [Tablename] nedefinuje žádné klíčové sloupce</span><span class="sxs-lookup"><span data-stu-id="57c75-173">**Error**: Table '[Tablename]' does not define any key columns</span></span>

<span data-ttu-id="57c75-174">**Příčina**: tabulka nemá žádné primární klíč.</span><span class="sxs-lookup"><span data-stu-id="57c75-174">**Cause**: The table does not have any primary key.</span></span>  

<span data-ttu-id="57c75-175">**Řešení**: databáze Oracle connector vyžaduje použít tabulku se sloupcem primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="57c75-175">**Resolution**: The Oracle Database connector requires that a table with a primary key column be used.</span></span>

#### <a name="currently-not-supported"></a><span data-ttu-id="57c75-176">Aktuálně není podporována</span><span class="sxs-lookup"><span data-stu-id="57c75-176">Currently not supported</span></span>

* <span data-ttu-id="57c75-177">Zobrazení a uložených procedur</span><span class="sxs-lookup"><span data-stu-id="57c75-177">Views and stored procedures</span></span> 
* <span data-ttu-id="57c75-178">Všechny tabulky s složené klíče</span><span class="sxs-lookup"><span data-stu-id="57c75-178">Any table with composite keys</span></span>
* <span data-ttu-id="57c75-179">Vnořené typy objektů v tabulkách</span><span class="sxs-lookup"><span data-stu-id="57c75-179">Nested object types in tables</span></span>
 
## <a name="connector-specific-details"></a><span data-ttu-id="57c75-180">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="57c75-180">Connector-specific details</span></span>

<span data-ttu-id="57c75-181">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/oracle/).</span><span class="sxs-lookup"><span data-stu-id="57c75-181">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/oracle/).</span></span> 

## <a name="get-some-help"></a><span data-ttu-id="57c75-182">Získat pomoc</span><span class="sxs-lookup"><span data-stu-id="57c75-182">Get some help</span></span>

<span data-ttu-id="57c75-183">[Fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) je skvělým místem klást otázky, odpovědi na otázky a zjistit, co dělají jiní uživatelé Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="57c75-183">The [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) is a great place to ask questions, answer questions, and see what other Logic Apps users are doing.</span></span> 

<span data-ttu-id="57c75-184">Vám může pomoct zlepšit konektory a aplikace logiky hlasování a odesílání vašich nápadů v [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="57c75-184">You can help improve Logic Apps and connectors by voting and submitting your ideas at [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span></span> 


## <a name="next-steps"></a><span data-ttu-id="57c75-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57c75-185">Next steps</span></span>
<span data-ttu-id="57c75-186">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)a seznamte se s dostupných konektorů v logiku aplikace na náš [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="57c75-186">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md), and explore the available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
