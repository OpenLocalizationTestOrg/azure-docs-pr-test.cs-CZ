---
title: Automatizovat procesy Azure Application Insights s Flow Microsoft
description: "Zjistěte, jak Microsoft Flow můžete rychle automatizovat opakované procesy pomocí konektoru služby Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: bwren
ms.openlocfilehash: 510f4f284bbd0dbe4171896899f7ade7dee19e39
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a><span data-ttu-id="5eb88-103">Automatizovat procesy Azure Application Insights s konektorem pro Flow Microsoft</span><span class="sxs-lookup"><span data-stu-id="5eb88-103">Automate Azure Application Insights processes with the connector for Microsoft Flow</span></span>

<span data-ttu-id="5eb88-104">Se přistihnete opakovaně spuštění stejné dotazů na data telemetrie zkontrolujte, zda je vaše služba funguje správně?</span><span class="sxs-lookup"><span data-stu-id="5eb88-104">Do you find yourself repeatedly running the same queries on your telemetry data to check that your service is functioning properly?</span></span> <span data-ttu-id="5eb88-105">Hledáte automatizovat tyto dotazy pro hledání trendů a anomálií, a následně vytvořit vlastní pracovní postupy je obcházet?</span><span class="sxs-lookup"><span data-stu-id="5eb88-105">Are you looking to automate these queries for finding trends and anomalies and then build your own workflows around them?</span></span> <span data-ttu-id="5eb88-106">Konektor služby Azure Application Insights (preview) pro Microsoft Flow je ten nejvhodnější nástroj pro tyto účely.</span><span class="sxs-lookup"><span data-stu-id="5eb88-106">The Azure Application Insights connector (preview) for Microsoft Flow is the right tool for these purposes.</span></span>

<span data-ttu-id="5eb88-107">Díky této integraci teď procesy můžete automatizovat množství bez nutnosti napsat jediný řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="5eb88-107">With this integration, you can now automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="5eb88-108">Po vytvoření tokem pomocí akce Application Insights, tok automaticky spustí dotaz Application Insights Analytics.</span><span class="sxs-lookup"><span data-stu-id="5eb88-108">After you create a flow by using an Application Insights action, the flow automatically runs your Application Insights Analytics query.</span></span> 

<span data-ttu-id="5eb88-109">Můžete přidat i další akce.</span><span class="sxs-lookup"><span data-stu-id="5eb88-109">You can add additional actions as well.</span></span> <span data-ttu-id="5eb88-110">Microsoft Flow zpřístupní stovky akce.</span><span class="sxs-lookup"><span data-stu-id="5eb88-110">Microsoft Flow makes hundreds of actions available.</span></span> <span data-ttu-id="5eb88-111">Například můžete Flow Microsoft automaticky odesílat e-mailových oznámení nebo vytvoření chyby ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="5eb88-111">For example, you can use Microsoft Flow to automatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="5eb88-112">Můžete také použít jednu z dalších [šablony](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) které jsou k dispozici pro konektor pro Flow společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5eb88-112">You can also use one of the many [templates](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) that are available for the connector for Microsoft Flow.</span></span> <span data-ttu-id="5eb88-113">Tyto šablony urychlit proces vytváření k toku.</span><span class="sxs-lookup"><span data-stu-id="5eb88-113">These templates speed up the process of creating a flow.</span></span> 

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a><span data-ttu-id="5eb88-114">Vytvoření postup pro službu Application Insights</span><span class="sxs-lookup"><span data-stu-id="5eb88-114">Create a flow for Application Insights</span></span>

<span data-ttu-id="5eb88-115">V tomto kurzu se dozvíte, jak vytvořit toku, který používá algoritmus automatického clusteru Analytics skupiny atributů v datech pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5eb88-115">In this tutorial, you will learn how to create a flow that uses the Analytics auto-cluster algorithm to group attributes in the data for a web application.</span></span> <span data-ttu-id="5eb88-116">Tok automaticky odesílá výsledky e-mailem, pouze příklad, jak můžete používat Microsoft Flow a analýza Statistika aplikace společně.</span><span class="sxs-lookup"><span data-stu-id="5eb88-116">The flow automatically sends the results by email, just one example of how you can use Microsoft Flow and Application Insights Analytics together.</span></span> 

### <a name="step-1-create-a-flow"></a><span data-ttu-id="5eb88-117">Krok 1: Vytvoření toku</span><span class="sxs-lookup"><span data-stu-id="5eb88-117">Step 1: Create a flow</span></span>
1. <span data-ttu-id="5eb88-118">Přihlaste se k [Microsoft Flow](http://flow.microsoft.com)a potom vyberte **Moje toků**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-118">Sign in to [Microsoft Flow](http://flow.microsoft.com), and then select **My Flows**.</span></span>
2. <span data-ttu-id="5eb88-119">Klikněte na tlačítko **vytvořit tokem z prázdné**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-119">Click **Create a flow from blank**.</span></span>

### <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="5eb88-120">Krok 2: Vytvoření aktivační událost pro vaše tok</span><span class="sxs-lookup"><span data-stu-id="5eb88-120">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="5eb88-121">Vyberte **plán**a potom vyberte **plán - opakování**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-121">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
2. <span data-ttu-id="5eb88-122">V **frekvence** vyberte **den**a v **Interval** zadejte **1**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-122">In the **Frequency** box, select **Day**, and in the **Interval** box, enter **1**.</span></span>

    ![Dialogové okno Microsoft Flow aktivační události](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="5eb88-124">Krok 3: Přidání Application Insights akce</span><span class="sxs-lookup"><span data-stu-id="5eb88-124">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="5eb88-125">Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-125">Click **New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="5eb88-126">Vyhledejte **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-126">Search for **Azure Application Insights**.</span></span>
3. <span data-ttu-id="5eb88-127">Klikněte na tlačítko **Azure Application Insights – vizualizovat analýzy dotaz Preview**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-127">Click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    ![Spusťte okno dotazu Analytics](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a><span data-ttu-id="5eb88-129">Krok 4: Připojení k prostředku Application Insights</span><span class="sxs-lookup"><span data-stu-id="5eb88-129">Step 4: Connect to an Application Insights resource</span></span>

<span data-ttu-id="5eb88-130">K dokončení tohoto kroku, musíte aplikaci ID a klíč rozhraní API pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="5eb88-130">To complete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="5eb88-131">Můžete je znovu načíst z portálu Azure, jak je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="5eb88-131">You can retrieve them from the Azure portal, as shown in the following diagram:</span></span>

![ID aplikace v portálu Azure](./media/app-insights-automate-with-flow/appid.png) 

- <span data-ttu-id="5eb88-133">Zadejte název připojení, spolu s klíč rozhraní API a ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="5eb88-133">Provide a name for your connection, along with the application ID and API key.</span></span>

    ![Okno Microsoft Flow připojení](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a><span data-ttu-id="5eb88-135">Krok 5: Zadejte typ dotazu a graf analýzy</span><span class="sxs-lookup"><span data-stu-id="5eb88-135">Step 5: Specify the Analytics query and chart type</span></span>
<span data-ttu-id="5eb88-136">Tento příklad dotaz vybere neúspěšných požadavků v rámci poslední den a jejich koreluje s výjimkami, k nimž došlo v rámci operace.</span><span class="sxs-lookup"><span data-stu-id="5eb88-136">This example query selects the failed requests within the last day and correlates them with exceptions that occurred as part of the operation.</span></span> <span data-ttu-id="5eb88-137">Analýza korelaci je na základě identifikátoru operation_Id.</span><span class="sxs-lookup"><span data-stu-id="5eb88-137">Analytics correlates them based on the operation_Id identifier.</span></span> <span data-ttu-id="5eb88-138">Výsledky dotazu pak segmenty pomocí algoritmu autocluster.</span><span class="sxs-lookup"><span data-stu-id="5eb88-138">The query then segments the results by using the autocluster algorithm.</span></span> 

<span data-ttu-id="5eb88-139">Když vytvoříte vlastní dotazy, ověřte, že fungují správně v Analytics předtím, než ho přidáte do vašeho toku.</span><span class="sxs-lookup"><span data-stu-id="5eb88-139">When you create your own queries, verify that they are working properly in Analytics before you add it to your flow.</span></span>

- <span data-ttu-id="5eb88-140">Přidejte následující dotaz analýzy a pak vyberte typ grafu tabulky HTML.</span><span class="sxs-lookup"><span data-stu-id="5eb88-140">Add the following Analytics query, and then select the HTML table chart type.</span></span> 

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
    
    ![Okno Konfigurace analýzy dotazu](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-the-flow-to-send-email"></a><span data-ttu-id="5eb88-142">Krok 6: Konfigurace toku k odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="5eb88-142">Step 6: Configure the flow to send email</span></span>

1. <span data-ttu-id="5eb88-143">Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-143">Click **New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="5eb88-144">Vyhledejte **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-144">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="5eb88-145">Klikněte na tlačítko **Office 365 Outlook – e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-145">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Okno Výběr Outlook Office 365](./media/app-insights-automate-with-flow/flow2b.png)

4. <span data-ttu-id="5eb88-147">V **e-mailovou zprávu** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5eb88-147">In the **Send an email** window, do the following:</span></span>

   <span data-ttu-id="5eb88-148">a.</span><span class="sxs-lookup"><span data-stu-id="5eb88-148">a.</span></span> <span data-ttu-id="5eb88-149">Zadejte e-mailová adresa příjemce.</span><span class="sxs-lookup"><span data-stu-id="5eb88-149">Type the email address of the recipient.</span></span>

   <span data-ttu-id="5eb88-150">b.</span><span class="sxs-lookup"><span data-stu-id="5eb88-150">b.</span></span> <span data-ttu-id="5eb88-151">Zadejte předmět e-mailu.</span><span class="sxs-lookup"><span data-stu-id="5eb88-151">Type a subject for the email.</span></span>

   <span data-ttu-id="5eb88-152">c.</span><span class="sxs-lookup"><span data-stu-id="5eb88-152">c.</span></span> <span data-ttu-id="5eb88-153">Klikněte kamkoli do **textu** pole a pak na dynamické kontextové nabídky, které se otevře napravo, vyberte **textu**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-153">Click anywhere in the **Body** box and then, on the dynamic content menu that opens at the right, select **Body**.</span></span>

   <span data-ttu-id="5eb88-154">d.</span><span class="sxs-lookup"><span data-stu-id="5eb88-154">d.</span></span> <span data-ttu-id="5eb88-155">Klikněte na tlačítko **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-155">Click **Show advanced options**.</span></span>

    ![Konfigurace aplikace Outlook Office 365](./media/app-insights-automate-with-flow/flow5.png)

5. <span data-ttu-id="5eb88-157">V nabídce dynamického obsahu postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5eb88-157">On the dynamic content menu, do the following:</span></span>

    <span data-ttu-id="5eb88-158">a.</span><span class="sxs-lookup"><span data-stu-id="5eb88-158">a.</span></span> <span data-ttu-id="5eb88-159">Vyberte **název přílohy**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-159">Select **Attachment Name**.</span></span>

    <span data-ttu-id="5eb88-160">b.</span><span class="sxs-lookup"><span data-stu-id="5eb88-160">b.</span></span> <span data-ttu-id="5eb88-161">Vyberte **obsah přílohy**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-161">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="5eb88-162">c.</span><span class="sxs-lookup"><span data-stu-id="5eb88-162">c.</span></span> <span data-ttu-id="5eb88-163">V **je HTML** vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-163">In the **Is HTML** box, select **Yes**.</span></span>

    ![Okno Konfigurace e-mailu Office 365](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a><span data-ttu-id="5eb88-165">Krok 7: Uložit a testování vaší toku</span><span class="sxs-lookup"><span data-stu-id="5eb88-165">Step 7: Save and test your flow</span></span>
- <span data-ttu-id="5eb88-166">V **toku název** pole, přidejte název vaší toku a pak klikněte na tlačítko **vytvořit toku**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-166">In the **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span>

    ![Postup vytvoření okna](./media/app-insights-automate-with-flow/flow8.png)

<span data-ttu-id="5eb88-168">Počkejte, spouštějí tuto akci, nebo můžete spustit toku okamžitě nástrojem [spuštění aktivační události na vyžádání](https://flow.microsoft.com/blog/run-now-and-six-more-services/).</span><span class="sxs-lookup"><span data-stu-id="5eb88-168">You can wait for the trigger to run this action, or you can run the flow immediately by [running the trigger on demand](https://flow.microsoft.com/blog/run-now-and-six-more-services/).</span></span>

<span data-ttu-id="5eb88-169">Při spuštění toku, příjemce, které jste zadali v seznamu e-mailu dostávat e-mailovou zprávu, která vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="5eb88-169">When the flow runs, the recipients you have specified in the email list receive an email message that looks like the following:</span></span>

![Ukázkového e-mailu](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a><span data-ttu-id="5eb88-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5eb88-171">Next steps</span></span>

- <span data-ttu-id="5eb88-172">Další informace o vytváření [analytické dotazy](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="5eb88-172">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="5eb88-173">Další informace o [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="5eb88-173">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



<!--Link references-->





