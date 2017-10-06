---
title: "konektor databáze Oracle hello aaaAdd v Azure Logic Apps | Microsoft Docs"
description: "Použití konektoru databáze Oracle hello v aplikaci logiky"
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
ms.openlocfilehash: 8a802a6c4782e210ff71848614152cb46ba5d651
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-oracle-database-connector"></a><span data-ttu-id="4a6a4-103">Začínáme s konektorem databáze Oracle hello</span><span class="sxs-lookup"><span data-stu-id="4a6a4-103">Get started with hello Oracle Database connector</span></span>

<span data-ttu-id="4a6a4-104">Pomocí konektoru hello databáze Oracle, vytvoření organizační pracovní postupy, které pomocí data v existující databázi.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-104">Using hello Oracle Database connector, you create organizational workflows that use data in your existing database.</span></span> <span data-ttu-id="4a6a4-105">Tento konektor se může připojit tooan instalaci místní databázi Oracle nebo virtuální počítač Azure s databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-105">This connector can connect tooan on-premises Oracle Database, or an Azure virtual machine with Oracle Database installed.</span></span> <span data-ttu-id="4a6a4-106">Tento konektor můžete:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-106">With this connector, you can:</span></span>

* <span data-ttu-id="4a6a4-107">Vytvořte pracovní postup přidávání nové databáze zákazníci tooa zákazníka nebo aktualizaci pořadí, v databázi objednávky.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-107">Build your workflow by adding a new customer tooa customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="4a6a4-108">Pomocí akce tooget řádek dat, vložit nový řádek a ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-108">Use actions tooget a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="4a6a4-109">Například v aplikaci Dynamics CRM Online (aktivační událost) je vytvořen záznam, pak vložte řádek v databázi Oracle (akce).</span><span class="sxs-lookup"><span data-stu-id="4a6a4-109">For example, when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Oracle Database (an action).</span></span> 

<span data-ttu-id="4a6a4-110">Toto téma ukazuje, jak toouse hello databáze Oracle konektor v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-110">This topic shows you how toouse hello Oracle Database connector in a logic app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a6a4-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4a6a4-111">Prerequisites</span></span>

* <span data-ttu-id="4a6a4-112">Podporované verze Oracle:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-112">Supported Oracle versions:</span></span> 
    * <span data-ttu-id="4a6a4-113">Oracle 9 a novějším</span><span class="sxs-lookup"><span data-stu-id="4a6a4-113">Oracle 9 and later</span></span>
    * <span data-ttu-id="4a6a4-114">Klientský software Oracle 8.1.7 a novější</span><span class="sxs-lookup"><span data-stu-id="4a6a4-114">Oracle client software 8.1.7 and later</span></span>

* <span data-ttu-id="4a6a4-115">Nainstalujte bránu dat místní hello.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-115">Install hello on-premises data gateway.</span></span> <span data-ttu-id="4a6a4-116">[Připojení místní tooon dat z aplikace logiky](../logic-apps/logic-apps-gateway-connection.md) seznamy hello kroky.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-116">[Connect tooon-premises data from logic apps](../logic-apps/logic-apps-gateway-connection.md) lists hello steps.</span></span> <span data-ttu-id="4a6a4-117">Brána Hello je požadované tooconnect tooan místní databázi Oracle nebo virtuální počítač Azure s Oracle DB nainstalována.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-117">hello gateway is required tooconnect tooan on-premises Oracle Database, or an Azure VM with Oracle DB installed.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="4a6a4-118">Brána dat místní Hello funguje jako mostu a poskytuje přenos zabezpečených dat mezi místními daty (data, která není v cloudu hello) a aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-118">hello on-premises data gateway acts as a bridge, and provides a secure data transfer between on-premises data (data that is not in hello cloud) and your logic apps.</span></span> <span data-ttu-id="4a6a4-119">Hello stejnou bránou lze použít s více službami a několik datových zdrojů.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-119">hello same gateway can be used with multiple services, and multiple data sources.</span></span> <span data-ttu-id="4a6a4-120">Ano jenom musíte tooinstall hello brány jednou.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-120">So, you may only need tooinstall hello gateway once.</span></span>

* <span data-ttu-id="4a6a4-121">Nainstalujte na hello počítače, kam jste nainstalovali hello místní data brána hello klienta Oracle.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-121">Install hello Oracle Client on hello machine where you installed hello on-premises data gateway.</span></span> <span data-ttu-id="4a6a4-122">Ujistěte se, tooinstall hello 64-bit poskytovatele dat Oracle pro .NET z databáze Oracle:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-122">Be sure tooinstall hello 64-bit Oracle Data Provider for .NET from Oracle:</span></span>  

  [<span data-ttu-id="4a6a4-123">64-bit ODAC 12c verze 4 (12.1.0.2.4) pro Windows x64</span><span class="sxs-lookup"><span data-stu-id="4a6a4-123">64-bit ODAC 12c Release 4 (12.1.0.2.4) for Windows x64</span></span>](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > <span data-ttu-id="4a6a4-124">Pokud není nainstalován klient hello Oracle, dojde k chybě při akci toocreate nebo používat hello připojení.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-124">If hello Oracle client is not installed, an error occurs when you try toocreate or use hello connection.</span></span> <span data-ttu-id="4a6a4-125">Najdete běžné chyby hello v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-125">See hello common errors in this topic.</span></span>


## <a name="add-hello-connector"></a><span data-ttu-id="4a6a4-126">Přidejte konektor hello</span><span class="sxs-lookup"><span data-stu-id="4a6a4-126">Add hello connector</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a6a4-127">Tento konektor nemá žádné aktivační události.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-127">This connector does not have any triggers.</span></span> <span data-ttu-id="4a6a4-128">Má pouze akce.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-128">It has only actions.</span></span> <span data-ttu-id="4a6a4-129">Proto při vytváření aplikace logiky, přidejte jiný toostart aktivační událost aplikace logiky, jako **plán - opakování**, nebo **požadavku nebo odpovědi - odpovědi**.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-129">So when you create your logic app, add another trigger toostart your logic app, such as **Schedule - Recurrence**, or **Request / Response - Response**.</span></span> 

1. <span data-ttu-id="4a6a4-130">V hello [portál Azure](https://portal.azure.com), vytvoření aplikace logiky prázdné.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-130">In hello [Azure portal](https://portal.azure.com), create a blank logic app.</span></span>

2. <span data-ttu-id="4a6a4-131">Při spuštění hello aplikace logiky, vyberte hello **požadavku nebo odpovědi - požadavek** aktivační události:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-131">At hello start of your logic app, select hello **Request / Response - Request** trigger:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. <span data-ttu-id="4a6a4-132">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-132">Select **Save**.</span></span> <span data-ttu-id="4a6a4-133">Při ukládání, se automaticky vygeneroval adrese URL žádosti.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-133">When you save, a request URL is automatically generated.</span></span> 

4. <span data-ttu-id="4a6a4-134">Vyberte **nový krok**a vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-134">Select **New step**, and select **Add an action**.</span></span> <span data-ttu-id="4a6a4-135">Zadejte `oracle` toosee hello dostupné akce:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-135">Type in `oracle` toosee hello available actions:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > <span data-ttu-id="4a6a4-136">Toto je také hello nejrychlejší způsob, jak toosee hello triggery a akce, které jsou k dispozici pro všechny konektory.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-136">This is also hello quickest way toosee hello triggers and actions available for any connector.</span></span> <span data-ttu-id="4a6a4-137">Zadejte v části název konektoru hello, například `oracle`.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-137">Type in part of hello connector name, such as `oracle`.</span></span> <span data-ttu-id="4a6a4-138">Návrhář Hello uvádí žádné aktivační události a všechny akce.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-138">hello designer lists any triggers and any actions.</span></span> 

5. <span data-ttu-id="4a6a4-139">Vyberte jednu z hello akcí, jako například **Oracle Database - Get řádek**.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-139">Select one of hello actions, such as **Oracle Database - Get row**.</span></span> <span data-ttu-id="4a6a4-140">Vyberte **připojit prostřednictvím místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-140">Select **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="4a6a4-141">Zadejte název serveru Oracle hello, metodu ověřování, uživatelské jméno, heslo a vyberte hello brány:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-141">Enter hello Oracle server name, authentication method, username, password, and select hello gateway:</span></span>

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. <span data-ttu-id="4a6a4-142">Po připojení, vyberte tabulku ze seznamu hello a zadejte hello řádek ID tooyour tabulky.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-142">Once connected, select a table from hello list, and enter hello row ID tooyour table.</span></span> <span data-ttu-id="4a6a4-143">Je nutné tooknow hello identifikátor toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-143">You need tooknow hello identifier toohello table.</span></span> <span data-ttu-id="4a6a4-144">Pokud si nejste jisti, obraťte se na správce databáze Oracle a získat výstup hello `select * from yourTableName`.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-144">If you don't know, contact your Oracle DB administrator, and get hello output from `select * from yourTableName`.</span></span> <span data-ttu-id="4a6a4-145">Tato poskytuje hello osobní údaje, které budete potřebovat tooproceed.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-145">This gives you hello identifiable information you need tooproceed.</span></span>

    <span data-ttu-id="4a6a4-146">V následujícím příkladu hello se vrací data úlohy z databáze lidských zdrojů:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-146">In hello following example, job data is being returned from a Human Resources database:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. <span data-ttu-id="4a6a4-147">V tomto kroku další můžete použít všechny hello jiných konektorů toobuild pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-147">In this next step, you can use any of hello other connectors toobuild your workflow.</span></span> <span data-ttu-id="4a6a4-148">Pokud chcete tootest získávání dat z databáze Oracle a potom odeslat e-mail s hello Oracle data pomocí jedné z hello sami odešlete e-mailu konektory, takové Office 365 nebo z Gmailu.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-148">If you want tootest getting data from Oracle, then send yourself an email with hello Oracle data using one of hello send email connectors, such Office 365 or Gmail.</span></span> <span data-ttu-id="4a6a4-149">Použití dynamické tokeny hello od hello Oracle tabulky toobuild hello `Subject` a `Body` vašeho e-mailu:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-149">Use hello dynamic tokens from hello Oracle table toobuild hello `Subject` and `Body` of your email:</span></span>

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. <span data-ttu-id="4a6a4-150">**Uložit** vaší aplikace logiky a potom vyberte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-150">**Save** your logic app, and then select **Run**.</span></span> <span data-ttu-id="4a6a4-151">Zavřít návrháře hello a podívejte se na hello spustí historie stavu hello.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-151">Close hello designer, and look at hello runs history for hello status.</span></span> <span data-ttu-id="4a6a4-152">Pokud se nezdaří, vyberte řádek neúspěšné zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-152">If it fails, select hello failed message row.</span></span> <span data-ttu-id="4a6a4-153">Návrhář Hello otevře a ukáže vám, který krok se nezdařil a také ukazuje hello informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-153">hello designer opens, and shows you which step failed, and also shows hello error information.</span></span> <span data-ttu-id="4a6a4-154">Pokud se aktivace podaří, měli byste obdržet e-mail s hello informace, které jste přidali.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-154">If it succeeds, then you should receive an email with hello information you added.</span></span>


### <a name="workflow-ideas"></a><span data-ttu-id="4a6a4-155">Nápady pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="4a6a4-155">Workflow ideas</span></span>

* <span data-ttu-id="4a6a4-156">Chcete toomonitor hello #oracle hashtag a put hello tweetů v databázi, mohou být dotazována a používaných v rámci jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-156">You want toomonitor hello #oracle hashtag, and put hello tweets in a database so they can be queried, and used within other applications.</span></span> <span data-ttu-id="4a6a4-157">V aplikaci logiky, aplikaci přidat hello `Twitter - When a new tweet is posted` aktivovat a potom zadejte hello **#oracle** hashtag.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-157">In a logic app, add hello `Twitter - When a new tweet is posted` trigger, and enter hello **#oracle** hashtag.</span></span> <span data-ttu-id="4a6a4-158">Pak přidejte hello `Oracle Database - Insert row` akce a vyberte tabulku:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-158">Then, add hello `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* <span data-ttu-id="4a6a4-159">Zprávy jsou odesílány tooa fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-159">Messages are sent tooa Service Bus queue.</span></span> <span data-ttu-id="4a6a4-160">Chcete tyto zprávy tooget a umístí je do databáze.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-160">You want tooget these messages, and put them in a database.</span></span> <span data-ttu-id="4a6a4-161">V aplikaci logiky, aplikaci přidat hello `Service Bus - when a message is received in a queue` aktivační události a vyberte hello fronty.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-161">In a logic app, add hello `Service Bus - when a message is received in a queue` trigger, and select hello queue.</span></span> <span data-ttu-id="4a6a4-162">Pak přidejte hello `Oracle Database - Insert row` akce a vyberte tabulku:</span><span class="sxs-lookup"><span data-stu-id="4a6a4-162">Then, add hello `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a><span data-ttu-id="4a6a4-163">Běžné chyby</span><span class="sxs-lookup"><span data-stu-id="4a6a4-163">Common errors</span></span>

#### <a name="error-cannot-reach-hello-gateway"></a><span data-ttu-id="4a6a4-164">**Chyba**: Nelze kontaktovat hello brány</span><span class="sxs-lookup"><span data-stu-id="4a6a4-164">**Error**: Cannot reach hello Gateway</span></span>

<span data-ttu-id="4a6a4-165">**Příčina**: hello místní data brána není možné tooconnect toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-165">**Cause**: hello on-premises data gateway is not able tooconnect toohello cloud.</span></span> 

<span data-ttu-id="4a6a4-166">**Zmírnění dopadů**: Zajistěte, aby vaše brána je spuštěná na hello na místním počítači, kde jste ho nainstalovali, a to může připojit toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-166">**Mitigation**: Make sure your gateway is running on hello on-premises machine where you installed it, and that it can connect toohello internet.</span></span>  <span data-ttu-id="4a6a4-167">Doporučujeme není hello bránu nainstalovat na počítač, který může být vypnutý nebo v režimu spánku.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-167">We recommend not installing hello gateway on a computer that may be turned off or sleep.</span></span> <span data-ttu-id="4a6a4-168">Můžete také restartovat hello místní služba brány dat (PBIEgwService).</span><span class="sxs-lookup"><span data-stu-id="4a6a4-168">You can also restart hello on-premises data gateway service (PBIEgwService).</span></span>

#### <a name="error-hello-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-tooinstall-hello-official-provider"></a><span data-ttu-id="4a6a4-169">**Chyba**: používaný hello zprostředkovatel je zastaralý: ' System.Data.OracleClient vyžaduje Oracle verze klientského softwaru 8.1.7 nebo vyšší. ".</span><span class="sxs-lookup"><span data-stu-id="4a6a4-169">**Error**: hello provider being used is deprecated: 'System.Data.OracleClient requires Oracle client software version 8.1.7 or greater.'.</span></span> <span data-ttu-id="4a6a4-170">Navštivte [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello oficiálního zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-170">Please visit [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello official provider.</span></span>

<span data-ttu-id="4a6a4-171">**Příčina**: Sada SDK není nainstalovaná na počítači hello kde hello klienta Oracle hello místní brána dat je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-171">**Cause**: hello Oracle client SDK is not installed on hello machine where hello on-premises data gateway is running.</span></span>  

<span data-ttu-id="4a6a4-172">**Řešení**: Stáhněte a nainstalujte klienta Oracle hello SDK na hello stejného počítače jako brány dat místní hello.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-172">**Resolution**: Download and install hello Oracle client SDK on hello same computer as hello on-premises data gateway.</span></span>

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a><span data-ttu-id="4a6a4-173">**Chyba**: Tabulka [Tablename] nedefinuje žádné klíčové sloupce</span><span class="sxs-lookup"><span data-stu-id="4a6a4-173">**Error**: Table '[Tablename]' does not define any key columns</span></span>

<span data-ttu-id="4a6a4-174">**Příčina**: hello tabulka neobsahuje žádné primární klíč.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-174">**Cause**: hello table does not have any primary key.</span></span>  

<span data-ttu-id="4a6a4-175">**Řešení**: hello databáze Oracle connector vyžaduje použít tabulku se sloupcem primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-175">**Resolution**: hello Oracle Database connector requires that a table with a primary key column be used.</span></span>

#### <a name="currently-not-supported"></a><span data-ttu-id="4a6a4-176">Aktuálně není podporována</span><span class="sxs-lookup"><span data-stu-id="4a6a4-176">Currently not supported</span></span>

* <span data-ttu-id="4a6a4-177">Zobrazení a uložených procedur</span><span class="sxs-lookup"><span data-stu-id="4a6a4-177">Views and stored procedures</span></span> 
* <span data-ttu-id="4a6a4-178">Všechny tabulky s složené klíče</span><span class="sxs-lookup"><span data-stu-id="4a6a4-178">Any table with composite keys</span></span>
* <span data-ttu-id="4a6a4-179">Vnořené typy objektů v tabulkách</span><span class="sxs-lookup"><span data-stu-id="4a6a4-179">Nested object types in tables</span></span>
 
## <a name="connector-specific-details"></a><span data-ttu-id="4a6a4-180">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="4a6a4-180">Connector-specific details</span></span>

<span data-ttu-id="4a6a4-181">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/oracle/).</span><span class="sxs-lookup"><span data-stu-id="4a6a4-181">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/oracle/).</span></span> 

## <a name="get-some-help"></a><span data-ttu-id="4a6a4-182">Získat pomoc</span><span class="sxs-lookup"><span data-stu-id="4a6a4-182">Get some help</span></span>

<span data-ttu-id="4a6a4-183">Hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) je skvělá umístit tooask otázky, odpovědi na otázky a najdete v části činnosti jiných uživatelů Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="4a6a4-183">hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) is a great place tooask questions, answer questions, and see what other Logic Apps users are doing.</span></span> 

<span data-ttu-id="4a6a4-184">Vám může pomoct zlepšit konektory a aplikace logiky hlasování a odesílání vašich nápadů v [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="4a6a4-184">You can help improve Logic Apps and connectors by voting and submitting your ideas at [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span></span> 


## <a name="next-steps"></a><span data-ttu-id="4a6a4-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a6a4-185">Next steps</span></span>
<span data-ttu-id="4a6a4-186">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)a seznamte se s hello dostupných konektorů v logiku aplikace na náš [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="4a6a4-186">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md), and explore hello available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
