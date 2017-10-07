---
title: "aaaAutomate Azure Application Insights zpracovává s Flow Microsoft"
description: "Zjistěte, jak lze pomocí Microsoft Flow tooquickly automatizovat opakované procesy pomocí konektoru hello Application Insights."
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
ms.openlocfilehash: b34488a6b8b8b0a6add960a67f1426cbbbc13552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-azure-application-insights-processes-with-hello-connector-for-microsoft-flow"></a><span data-ttu-id="56384-103">Automatizovat procesy Azure Application Insights s hello connector pro Microsoft Flow</span><span class="sxs-lookup"><span data-stu-id="56384-103">Automate Azure Application Insights processes with hello connector for Microsoft Flow</span></span>

<span data-ttu-id="56384-104">Se přistihnete opakovaně spuštění hello stejné dotazů na váš toocheck data telemetrie, které vaše služba funguje správně?</span><span class="sxs-lookup"><span data-stu-id="56384-104">Do you find yourself repeatedly running hello same queries on your telemetry data toocheck that your service is functioning properly?</span></span> <span data-ttu-id="56384-105">Jsou vypadající tooautomate můžete tyto dotazy pro hledání trendů a anomálií a následně vytvořit vlastní pracovní postupy je obcházet? Hello konektor služby Azure Application Insights (preview) pro Microsoft Flow je pro tyto účely hello nejvhodnější nástroj.</span><span class="sxs-lookup"><span data-stu-id="56384-105">Are you looking tooautomate these queries for finding trends and anomalies and then build your own workflows around them? hello Azure Application Insights connector (preview) for Microsoft Flow is hello right tool for these purposes.</span></span>

<span data-ttu-id="56384-106">Díky této integraci teď procesy můžete automatizovat množství bez nutnosti napsat jediný řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="56384-106">With this integration, you can now automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="56384-107">Po vytvoření tokem pomocí akce Application Insights, hello toku automaticky spustí dotaz Application Insights Analytics.</span><span class="sxs-lookup"><span data-stu-id="56384-107">After you create a flow by using an Application Insights action, hello flow automatically runs your Application Insights Analytics query.</span></span> 

<span data-ttu-id="56384-108">Můžete přidat i další akce.</span><span class="sxs-lookup"><span data-stu-id="56384-108">You can add additional actions as well.</span></span> <span data-ttu-id="56384-109">Microsoft Flow zpřístupní stovky akce.</span><span class="sxs-lookup"><span data-stu-id="56384-109">Microsoft Flow makes hundreds of actions available.</span></span> <span data-ttu-id="56384-110">Můžete například použít Microsoft Flow tooautomatically odesílání e-mailových oznámení nebo vytvoření chyby ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="56384-110">For example, you can use Microsoft Flow tooautomatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="56384-111">Můžete také použít jeden z hello mnoho [šablony](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) které jsou k dispozici pro hello konektor pro Flow společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="56384-111">You can also use one of hello many [templates](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) that are available for hello connector for Microsoft Flow.</span></span> <span data-ttu-id="56384-112">Tyto šablony zrychlení hello proces vytváření k toku.</span><span class="sxs-lookup"><span data-stu-id="56384-112">These templates speed up hello process of creating a flow.</span></span> 

<!--hello Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a><span data-ttu-id="56384-113">Vytvoření postup pro službu Application Insights</span><span class="sxs-lookup"><span data-stu-id="56384-113">Create a flow for Application Insights</span></span>

<span data-ttu-id="56384-114">V tomto kurzu se dozvíte, jak toocreate toku, který používá hello Analytics clusteru automaticky algoritmus toogroup atributy v hello data pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56384-114">In this tutorial, you will learn how toocreate a flow that uses hello Analytics auto-cluster algorithm toogroup attributes in hello data for a web application.</span></span> <span data-ttu-id="56384-115">tok Hello automaticky odesílá hello výsledky e-mailem, pouze příklad, jak můžete používat Microsoft Flow a analýza Statistika aplikace společně.</span><span class="sxs-lookup"><span data-stu-id="56384-115">hello flow automatically sends hello results by email, just one example of how you can use Microsoft Flow and Application Insights Analytics together.</span></span> 

### <a name="step-1-create-a-flow"></a><span data-ttu-id="56384-116">Krok 1: Vytvoření toku</span><span class="sxs-lookup"><span data-stu-id="56384-116">Step 1: Create a flow</span></span>
1. <span data-ttu-id="56384-117">Přihlaste se příliš[Microsoft Flow](http://flow.microsoft.com)a potom vyberte **Moje toků**.</span><span class="sxs-lookup"><span data-stu-id="56384-117">Sign in too[Microsoft Flow](http://flow.microsoft.com), and then select **My Flows**.</span></span>
2. <span data-ttu-id="56384-118">Klikněte na tlačítko **vytvořit tokem z prázdné**.</span><span class="sxs-lookup"><span data-stu-id="56384-118">Click **Create a flow from blank**.</span></span>

### <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="56384-119">Krok 2: Vytvoření aktivační událost pro vaše tok</span><span class="sxs-lookup"><span data-stu-id="56384-119">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="56384-120">Vyberte **plán**a potom vyberte **plán - opakování**.</span><span class="sxs-lookup"><span data-stu-id="56384-120">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
2. <span data-ttu-id="56384-121">V hello **frekvence** vyberte **den**a v hello **Interval** zadejte **1**.</span><span class="sxs-lookup"><span data-stu-id="56384-121">In hello **Frequency** box, select **Day**, and in hello **Interval** box, enter **1**.</span></span>

    ![Dialogové okno Microsoft Flow aktivační události](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="56384-123">Krok 3: Přidání Application Insights akce</span><span class="sxs-lookup"><span data-stu-id="56384-123">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="56384-124">Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="56384-124">Click **New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="56384-125">Vyhledejte **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="56384-125">Search for **Azure Application Insights**.</span></span>
3. <span data-ttu-id="56384-126">Klikněte na tlačítko **Azure Application Insights – vizualizovat analýzy dotaz Preview**.</span><span class="sxs-lookup"><span data-stu-id="56384-126">Click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    ![Spusťte okno dotazu Analytics](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a><span data-ttu-id="56384-128">Krok 4: Připojení tooan prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="56384-128">Step 4: Connect tooan Application Insights resource</span></span>

<span data-ttu-id="56384-129">toocomplete tohoto kroku budete potřebovat ID aplikací a klíč rozhraní API pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="56384-129">toocomplete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="56384-130">Můžete je znovu načíst z hello portál Azure, jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="56384-130">You can retrieve them from hello Azure portal, as shown in hello following diagram:</span></span>

![ID aplikace v hello portálu Azure](./media/app-insights-automate-with-flow/appid.png) 

- <span data-ttu-id="56384-132">Zadejte název připojení, spolu s hello aplikace ID a klíč API.</span><span class="sxs-lookup"><span data-stu-id="56384-132">Provide a name for your connection, along with hello application ID and API key.</span></span>

    ![Okno Microsoft Flow připojení](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a><span data-ttu-id="56384-134">Krok 5: Zadejte hello Analytics dotazu a typ grafu</span><span class="sxs-lookup"><span data-stu-id="56384-134">Step 5: Specify hello Analytics query and chart type</span></span>
<span data-ttu-id="56384-135">Tento příklad dotaz vybere hello se nezdařilo požadavky v rámci hello poslední den a je koreluje s výjimkami, k nimž došlo v rámci operace hello.</span><span class="sxs-lookup"><span data-stu-id="56384-135">This example query selects hello failed requests within hello last day and correlates them with exceptions that occurred as part of hello operation.</span></span> <span data-ttu-id="56384-136">Analýza korelaci je založena na hello operation_Id identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="56384-136">Analytics correlates them based on hello operation_Id identifier.</span></span> <span data-ttu-id="56384-137">dotaz Hello pak segmenty hello výsledky pomocí algoritmu autocluster hello.</span><span class="sxs-lookup"><span data-stu-id="56384-137">hello query then segments hello results by using hello autocluster algorithm.</span></span> 

<span data-ttu-id="56384-138">Když vytvoříte vlastní dotazy, ověřte, že fungují správně v Analytics předtím, než ho přidáte tooyour toku.</span><span class="sxs-lookup"><span data-stu-id="56384-138">When you create your own queries, verify that they are working properly in Analytics before you add it tooyour flow.</span></span>

- <span data-ttu-id="56384-139">Přidejte následující dotaz Analytics hello a pak vyberte typ grafu tabulky HTML hello.</span><span class="sxs-lookup"><span data-stu-id="56384-139">Add hello following Analytics query, and then select hello HTML table chart type.</span></span> 

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

### <a name="step-6-configure-hello-flow-toosend-email"></a><span data-ttu-id="56384-141">Krok 6: Konfigurace hello toku toosend e-mailu</span><span class="sxs-lookup"><span data-stu-id="56384-141">Step 6: Configure hello flow toosend email</span></span>

1. <span data-ttu-id="56384-142">Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="56384-142">Click **New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="56384-143">Vyhledejte **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="56384-143">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="56384-144">Klikněte na tlačítko **Office 365 Outlook – e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="56384-144">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Okno Výběr Outlook Office 365](./media/app-insights-automate-with-flow/flow2b.png)

4. <span data-ttu-id="56384-146">V hello **e-mailovou zprávu** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="56384-146">In hello **Send an email** window, do hello following:</span></span>

   <span data-ttu-id="56384-147">a.</span><span class="sxs-lookup"><span data-stu-id="56384-147">a.</span></span> <span data-ttu-id="56384-148">Zadejte e-mailová adresa příjemce hello hello.</span><span class="sxs-lookup"><span data-stu-id="56384-148">Type hello email address of hello recipient.</span></span>

   <span data-ttu-id="56384-149">b.</span><span class="sxs-lookup"><span data-stu-id="56384-149">b.</span></span> <span data-ttu-id="56384-150">Zadejte předmět e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="56384-150">Type a subject for hello email.</span></span>

   <span data-ttu-id="56384-151">c.</span><span class="sxs-lookup"><span data-stu-id="56384-151">c.</span></span> <span data-ttu-id="56384-152">Klikněte kamkoli do hello **textu** pole a zvolte v nabídce hello dynamického obsahu které se otevře ve správné hello **textu**.</span><span class="sxs-lookup"><span data-stu-id="56384-152">Click anywhere in hello **Body** box and then, on hello dynamic content menu that opens at hello right, select **Body**.</span></span>

   <span data-ttu-id="56384-153">d.</span><span class="sxs-lookup"><span data-stu-id="56384-153">d.</span></span> <span data-ttu-id="56384-154">Klikněte na tlačítko **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="56384-154">Click **Show advanced options**.</span></span>

    ![Konfigurace aplikace Outlook Office 365](./media/app-insights-automate-with-flow/flow5.png)

5. <span data-ttu-id="56384-156">V nabídce dynamického obsahu hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="56384-156">On hello dynamic content menu, do hello following:</span></span>

    <span data-ttu-id="56384-157">a.</span><span class="sxs-lookup"><span data-stu-id="56384-157">a.</span></span> <span data-ttu-id="56384-158">Vyberte **název přílohy**.</span><span class="sxs-lookup"><span data-stu-id="56384-158">Select **Attachment Name**.</span></span>

    <span data-ttu-id="56384-159">b.</span><span class="sxs-lookup"><span data-stu-id="56384-159">b.</span></span> <span data-ttu-id="56384-160">Vyberte **obsah přílohy**.</span><span class="sxs-lookup"><span data-stu-id="56384-160">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="56384-161">c.</span><span class="sxs-lookup"><span data-stu-id="56384-161">c.</span></span> <span data-ttu-id="56384-162">V hello **je HTML** vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="56384-162">In hello **Is HTML** box, select **Yes**.</span></span>

    ![Okno Konfigurace e-mailu Office 365](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a><span data-ttu-id="56384-164">Krok 7: Uložit a testování vaší toku</span><span class="sxs-lookup"><span data-stu-id="56384-164">Step 7: Save and test your flow</span></span>
- <span data-ttu-id="56384-165">V hello **toku název** pole, přidejte název vaší toku a pak klikněte na tlačítko **vytvořit toku**.</span><span class="sxs-lookup"><span data-stu-id="56384-165">In hello **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span>

    ![Postup vytvoření okna](./media/app-insights-automate-with-flow/flow8.png)

<span data-ttu-id="56384-167">Počkejte, až hello aktivační událost toorun tuto akci, nebo můžete spustit hello toku okamžitě nástrojem [spuštěná aktivační událost hello na vyžádání](https://flow.microsoft.com/blog/run-now-and-six-more-services/).</span><span class="sxs-lookup"><span data-stu-id="56384-167">You can wait for hello trigger toorun this action, or you can run hello flow immediately by [running hello trigger on demand](https://flow.microsoft.com/blog/run-now-and-six-more-services/).</span></span>

<span data-ttu-id="56384-168">Po spuštění hello toku hello příjemce, které jste zadali v seznamu e-mailu hello přijímat e-mailovou zprávu, která vypadá jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="56384-168">When hello flow runs, hello recipients you have specified in hello email list receive an email message that looks like hello following:</span></span>

![Ukázkového e-mailu](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a><span data-ttu-id="56384-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56384-170">Next steps</span></span>

- <span data-ttu-id="56384-171">Další informace o vytváření [analytické dotazy](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="56384-171">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="56384-172">Další informace o [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="56384-172">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



<!--Link references-->





