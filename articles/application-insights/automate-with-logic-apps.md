---
title: "Automatizovat procesy Azure Application Insights pomocí Logic Apps."
description: "Zjistěte, jak rychle procesy můžete automatizovat opakované přidáním konektor Application Insights pro svou aplikaci logiky."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: bwren
ms.openlocfilehash: 36df5bc0a019f4197d40fd6fa5a2a9957820c8b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a><span data-ttu-id="4748b-103">Automatizovat procesy Application Insights pomocí Logic Apps</span><span class="sxs-lookup"><span data-stu-id="4748b-103">Automate Application Insights processes by using Logic Apps</span></span>

<span data-ttu-id="4748b-104">Se přistihnete opakovaně spuštění stejné dotazů na data telemetrie zkontrolujte, zda správně pracuje služby?</span><span class="sxs-lookup"><span data-stu-id="4748b-104">Do you find yourself repeatedly running the same queries on your telemetry data to check whether your service is functioning properly?</span></span> <span data-ttu-id="4748b-105">Hledáte automatizovat tyto dotazy pro hledání trendů a anomálií, a následně vytvořit vlastní pracovní postupy je obcházet?</span><span class="sxs-lookup"><span data-stu-id="4748b-105">Are you looking to automate these queries for finding trends and anomalies and then build your own workflows around them?</span></span> <span data-ttu-id="4748b-106">Konektor služby Azure Application Insights (preview) pro Logic Apps je ten nejvhodnější nástroj pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="4748b-106">The Azure Application Insights connector (preview) for Logic Apps is the right tool for this purpose.</span></span>

<span data-ttu-id="4748b-107">Díky této integraci můžete automatizovat procesy množství bez nutnosti napsat jediný řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="4748b-107">With this integration, you can automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="4748b-108">Aplikace logiky můžete vytvořit s konektorem služby Application Insights rychle automatizovat jakýkoli proces Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4748b-108">You can create a logic app with the Application Insights connector to quickly automate any Application Insights process.</span></span> 

<span data-ttu-id="4748b-109">Můžete přidat i další akce.</span><span class="sxs-lookup"><span data-stu-id="4748b-109">You can add additional actions as well.</span></span> <span data-ttu-id="4748b-110">Díky funkci Logic Apps služby Azure App Service stovky akce k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4748b-110">The Logic Apps feature of Azure App Service makes hundreds of actions available.</span></span> <span data-ttu-id="4748b-111">Například pomocí aplikace logiky můžete můžete automaticky odesílat e-mailových oznámení nebo vytvoření chyby ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="4748b-111">For example, by using a logic app, you can automatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="4748b-112">Můžete také použít jednu z dostupných mnoho [šablony](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) chcete urychlit proces vytváření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="4748b-112">You can also use one of the many available [templates](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) to help speed up the process of creating your logic app.</span></span> 

## <a name="create-a-logic-app-for-application-insights"></a><span data-ttu-id="4748b-113">Vytvoření aplikace logiky pro službu Application Insights</span><span class="sxs-lookup"><span data-stu-id="4748b-113">Create a logic app for Application Insights</span></span>

<span data-ttu-id="4748b-114">V tomto kurzu zjistěte, jak vytvořit aplikaci logiky, která používá algoritmus autocluster Analytics skupiny atributů v datech pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4748b-114">In this tutorial, you learn how to create a logic app that uses the Analytics autocluster algorithm to group attributes in the data for a web application.</span></span> <span data-ttu-id="4748b-115">Tok automaticky odesílá výsledky e-mailem, pouze příklad, jak můžete použít Application Insights analýzy a Logic Apps společně.</span><span class="sxs-lookup"><span data-stu-id="4748b-115">The flow automatically sends the results by email, just one example of how you can use Application Insights Analytics and Logic Apps together.</span></span> 

### <a name="step-1-create-a-logic-app"></a><span data-ttu-id="4748b-116">Krok 1: Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="4748b-116">Step 1: Create a logic app</span></span>
1. <span data-ttu-id="4748b-117">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4748b-117">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4748b-118">V **nový** podokně, vyberte **Web + mobilní**a potom vyberte **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="4748b-118">In the **New** pane, select **Web + Mobile**, and then select **Logic App**.</span></span>

    ![Nové okno aplikace logiky](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a><span data-ttu-id="4748b-120">Krok 2: Vytvoření aktivační událost pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="4748b-120">Step 2: Create a trigger for your logic app</span></span>
1. <span data-ttu-id="4748b-121">V **návrhář aplikace na základě logiky** okno, v části **začínat běžné aktivační událost**, vyberte **opakování**.</span><span class="sxs-lookup"><span data-stu-id="4748b-121">In the **Logic App Designer** window, under **Start with a common trigger**, select **Recurrence**.</span></span>

    ![Okno Návrhář aplikace logiky](./media/automate-with-logic-apps/logicapp2.png)

2. <span data-ttu-id="4748b-123">V **frekvence** vyberte **den** a pak na **Interval** zadejte **1**.</span><span class="sxs-lookup"><span data-stu-id="4748b-123">In the **Frequency** box, select **Day** and then, in the **Interval** box, type **1**.</span></span>

    ![Návrhář aplikace logiky "Recurrence" okna](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="4748b-125">Krok 3: Přidání Application Insights akce</span><span class="sxs-lookup"><span data-stu-id="4748b-125">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="4748b-126">Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="4748b-126">Click **New step**, and then click **Add an action**.</span></span>

2. <span data-ttu-id="4748b-127">V **vybrat akci** vyhledávací pole, typ **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="4748b-127">In the **Choose an action** search box, type **Azure Application Insights**.</span></span>

3. <span data-ttu-id="4748b-128">V části **akce**, klikněte na tlačítko **Azure Application Insights – vizualizovat analýzy dotaz Preview**.</span><span class="sxs-lookup"><span data-stu-id="4748b-128">Under **Actions**, click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    !["Vyberte akci" návrhář aplikace na základě logiky – okno](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a><span data-ttu-id="4748b-130">Krok 4: Připojení k prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="4748b-130">Step 4: Connect to an Application Insights resource</span></span>

<span data-ttu-id="4748b-131">K dokončení tohoto kroku, musíte aplikaci ID a klíč rozhraní API pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="4748b-131">To complete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="4748b-132">Můžete je znovu načíst z portálu Azure, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="4748b-132">You can retrieve them from the Azure portal, as shown in the following diagram:</span></span>

![ID aplikace v portálu Azure](./media/automate-with-logic-apps/appid.png) 

<span data-ttu-id="4748b-134">Zadejte název připojení, aplikace ID a klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4748b-134">Provide a name for your connection, the application ID, and the API key.</span></span>

![Okno připojení toku návrhář aplikace na základě logiky](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a><span data-ttu-id="4748b-136">Krok 5: Zadejte typ dotazu a graf analýzy</span><span class="sxs-lookup"><span data-stu-id="4748b-136">Step 5: Specify the Analytics query and chart type</span></span>
<span data-ttu-id="4748b-137">V následujícím příkladu dotaz vybere neúspěšných požadavků v rámci poslední den a jejich koreluje s výjimkami, k nimž došlo v rámci operace.</span><span class="sxs-lookup"><span data-stu-id="4748b-137">In the following example, the query selects the failed requests within the last day and correlates them with exceptions that occurred as part of the operation.</span></span> <span data-ttu-id="4748b-138">Analýza korelaci neúspěšné požadavky zobrazíte na základě identifikátoru operation_Id.</span><span class="sxs-lookup"><span data-stu-id="4748b-138">Analytics correlates the failed requests, based on the operation_Id identifier.</span></span> <span data-ttu-id="4748b-139">Výsledky dotazu pak segmenty pomocí algoritmu autocluster.</span><span class="sxs-lookup"><span data-stu-id="4748b-139">The query then segments the results by using the autocluster algorithm.</span></span> 

<span data-ttu-id="4748b-140">Když vytvoříte vlastní dotazy, ověřte, že fungují správně v Analytics předtím, než ho přidáte do vašeho toku.</span><span class="sxs-lookup"><span data-stu-id="4748b-140">When you create your own queries, verify that they are working properly in Analytics before you add it to your flow.</span></span>

1. <span data-ttu-id="4748b-141">V **dotazu** pole, přidejte následující dotaz Analytics:</span><span class="sxs-lookup"><span data-stu-id="4748b-141">In the **Query** box, add the following Analytics query:</span></span> 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

2. <span data-ttu-id="4748b-142">V **typ grafu** vyberte **tabulky Html**.</span><span class="sxs-lookup"><span data-stu-id="4748b-142">In the **Chart Type** box, select **Html Table**.</span></span>

    ![Okno Konfigurace analýzy dotazu](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a><span data-ttu-id="4748b-144">Krok 6: Konfigurace aplikace logiky k odesílání e-mailu</span><span class="sxs-lookup"><span data-stu-id="4748b-144">Step 6: Configure the logic app to send email</span></span>

1. <span data-ttu-id="4748b-145">Klikněte na tlačítko **nový krok**a potom vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="4748b-145">Click **New step**, and then select **Add an action**.</span></span>

2. <span data-ttu-id="4748b-146">Do vyhledávacího pole zadejte **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="4748b-146">In the search box, type **Office 365 Outlook**.</span></span>

3. <span data-ttu-id="4748b-147">Klikněte na tlačítko **Office 365 Outlook – e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="4748b-147">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Výběr aplikace Outlook Office 365](./media/automate-with-logic-apps/flow2b.png)

4. <span data-ttu-id="4748b-149">V **e-mailovou zprávu** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4748b-149">In the **Send an email** window, do the following:</span></span>

   <span data-ttu-id="4748b-150">a.</span><span class="sxs-lookup"><span data-stu-id="4748b-150">a.</span></span> <span data-ttu-id="4748b-151">Zadejte e-mailová adresa příjemce.</span><span class="sxs-lookup"><span data-stu-id="4748b-151">Type the email address of the recipient.</span></span>

   <span data-ttu-id="4748b-152">b.</span><span class="sxs-lookup"><span data-stu-id="4748b-152">b.</span></span> <span data-ttu-id="4748b-153">Zadejte předmět e-mailu.</span><span class="sxs-lookup"><span data-stu-id="4748b-153">Type a subject for the email.</span></span>

   <span data-ttu-id="4748b-154">c.</span><span class="sxs-lookup"><span data-stu-id="4748b-154">c.</span></span> <span data-ttu-id="4748b-155">Klikněte kamkoli do **textu** pole a pak na dynamické kontextové nabídky, které se otevře napravo, vyberte **textu**.</span><span class="sxs-lookup"><span data-stu-id="4748b-155">Click anywhere in the **Body** box and then, on the dynamic content menu that opens at the right, select **Body**.</span></span>

   <span data-ttu-id="4748b-156">d.</span><span class="sxs-lookup"><span data-stu-id="4748b-156">d.</span></span> <span data-ttu-id="4748b-157">Klikněte na tlačítko **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="4748b-157">Click **Show advanced options**.</span></span>

      ![Konfigurace aplikace Outlook Office 365](./media/automate-with-logic-apps/flow5.png)

5. <span data-ttu-id="4748b-159">V nabídce dynamického obsahu postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4748b-159">On the dynamic content menu, do the following:</span></span>

    <span data-ttu-id="4748b-160">a.</span><span class="sxs-lookup"><span data-stu-id="4748b-160">a.</span></span> <span data-ttu-id="4748b-161">Vyberte **název přílohy**.</span><span class="sxs-lookup"><span data-stu-id="4748b-161">Select **Attachment Name**.</span></span>

    <span data-ttu-id="4748b-162">b.</span><span class="sxs-lookup"><span data-stu-id="4748b-162">b.</span></span> <span data-ttu-id="4748b-163">Vyberte **obsah přílohy**.</span><span class="sxs-lookup"><span data-stu-id="4748b-163">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="4748b-164">c.</span><span class="sxs-lookup"><span data-stu-id="4748b-164">c.</span></span> <span data-ttu-id="4748b-165">V **je HTML** vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="4748b-165">In the **Is HTML** box, select **Yes**.</span></span>

      ![Konfigurační obrazovce pro Office 365 e-mailu](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a><span data-ttu-id="4748b-167">Krok 7: Uložit a testování aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="4748b-167">Step 7: Save and test your logic app</span></span>
* <span data-ttu-id="4748b-168">Klikněte na tlačítko **Uložit** uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="4748b-168">Click **Save** to save your changes.</span></span>

<span data-ttu-id="4748b-169">Počkejte, spusťte aplikaci logiky aktivační událost, nebo aplikaci logiky můžete spustit okamžitě výběrem **spustit**.</span><span class="sxs-lookup"><span data-stu-id="4748b-169">You can wait for the trigger to run the logic app, or you can run the logic app immediately by selecting **Run**.</span></span>

![Obrazovky vytvoření aplikace logiky](./media/automate-with-logic-apps/step7.png)

<span data-ttu-id="4748b-171">Při spuštění aplikace logiky, příjemce, který jste zadali v seznamu e-mailu obdrží e-mail, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4748b-171">When your logic app runs, the recipients you specified in the email list will receive an email that looks like the following:</span></span>

![Logiku aplikace e-mailové zprávy](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a><span data-ttu-id="4748b-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4748b-173">Next steps</span></span>

- <span data-ttu-id="4748b-174">Další informace o vytváření [analytické dotazy](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="4748b-174">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="4748b-175">Další informace o [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span><span class="sxs-lookup"><span data-stu-id="4748b-175">Learn more about [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span></span>



<!--Link references-->





