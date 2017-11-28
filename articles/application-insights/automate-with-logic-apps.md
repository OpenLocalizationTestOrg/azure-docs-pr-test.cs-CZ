---
title: "aaaAutomate Azure Application Insights zpracuje pomocí Logic Apps."
description: "Zjistěte, jak rychle procesy můžete automatizovat opakované přidáním hello Application Insights konektor tooyour logiku aplikace."
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
ms.openlocfilehash: 8565aadf0644ffb10da8a0964821901907db2e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a><span data-ttu-id="694f5-103">Automatizovat procesy Application Insights pomocí Logic Apps</span><span class="sxs-lookup"><span data-stu-id="694f5-103">Automate Application Insights processes by using Logic Apps</span></span>

<span data-ttu-id="694f5-104">Se přistihnete opakovaně spuštění hello stejné dotazů na váš telemetrická data toocheck zda správně pracuje služby?</span><span class="sxs-lookup"><span data-stu-id="694f5-104">Do you find yourself repeatedly running hello same queries on your telemetry data toocheck whether your service is functioning properly?</span></span> <span data-ttu-id="694f5-105">Jsou vypadající tooautomate můžete tyto dotazy pro hledání trendů a anomálií a následně vytvořit vlastní pracovní postupy je obcházet? Hello konektor služby Azure Application Insights (preview) pro Logic Apps je hello nejvhodnější nástroj pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="694f5-105">Are you looking tooautomate these queries for finding trends and anomalies and then build your own workflows around them? hello Azure Application Insights connector (preview) for Logic Apps is hello right tool for this purpose.</span></span>

<span data-ttu-id="694f5-106">Díky této integraci můžete automatizovat procesy množství bez nutnosti napsat jediný řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="694f5-106">With this integration, you can automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="694f5-107">Můžete vytvořit aplikaci logiky s hello Application Insights konektor tooquickly automatizovat jakýkoli proces Application Insights.</span><span class="sxs-lookup"><span data-stu-id="694f5-107">You can create a logic app with hello Application Insights connector tooquickly automate any Application Insights process.</span></span> 

<span data-ttu-id="694f5-108">Můžete přidat i další akce.</span><span class="sxs-lookup"><span data-stu-id="694f5-108">You can add additional actions as well.</span></span> <span data-ttu-id="694f5-109">Díky funkci Hello Logic Apps služby Azure App Service stovky akce k dispozici.</span><span class="sxs-lookup"><span data-stu-id="694f5-109">hello Logic Apps feature of Azure App Service makes hundreds of actions available.</span></span> <span data-ttu-id="694f5-110">Například pomocí aplikace logiky můžete můžete automaticky odesílat e-mailových oznámení nebo vytvoření chyby ve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="694f5-110">For example, by using a logic app, you can automatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="694f5-111">Můžete použít také jeden z hello k dispozici mnoho [šablony](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp urychlení hello proces vytváření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="694f5-111">You can also use one of hello many available [templates](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp speed up hello process of creating your logic app.</span></span> 

## <a name="create-a-logic-app-for-application-insights"></a><span data-ttu-id="694f5-112">Vytvoření aplikace logiky pro službu Application Insights</span><span class="sxs-lookup"><span data-stu-id="694f5-112">Create a logic app for Application Insights</span></span>

<span data-ttu-id="694f5-113">V tomto kurzu zjistíte, jak toocreate aplikace logiky, která používá hello Analytics autocluster algoritmus toogroup atributy v hello data pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="694f5-113">In this tutorial, you learn how toocreate a logic app that uses hello Analytics autocluster algorithm toogroup attributes in hello data for a web application.</span></span> <span data-ttu-id="694f5-114">tok Hello automaticky odesílá hello výsledky e-mailem, pouze příklad, jak můžete použít Application Insights analýzy a Logic Apps společně.</span><span class="sxs-lookup"><span data-stu-id="694f5-114">hello flow automatically sends hello results by email, just one example of how you can use Application Insights Analytics and Logic Apps together.</span></span> 

### <a name="step-1-create-a-logic-app"></a><span data-ttu-id="694f5-115">Krok 1: Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="694f5-115">Step 1: Create a logic app</span></span>
1. <span data-ttu-id="694f5-116">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="694f5-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="694f5-117">V hello **nový** podokně, vyberte **Web + mobilní**a potom vyberte **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="694f5-117">In hello **New** pane, select **Web + Mobile**, and then select **Logic App**.</span></span>

    ![Nové okno aplikace logiky](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a><span data-ttu-id="694f5-119">Krok 2: Vytvoření aktivační událost pro svou aplikaci logiky</span><span class="sxs-lookup"><span data-stu-id="694f5-119">Step 2: Create a trigger for your logic app</span></span>
1. <span data-ttu-id="694f5-120">V hello **návrhář aplikace na základě logiky** okno, v části **začínat běžné aktivační událost**, vyberte **opakování**.</span><span class="sxs-lookup"><span data-stu-id="694f5-120">In hello **Logic App Designer** window, under **Start with a common trigger**, select **Recurrence**.</span></span>

    ![Okno Návrhář aplikace logiky](./media/automate-with-logic-apps/logicapp2.png)

2. <span data-ttu-id="694f5-122">V hello **frekvence** vyberte **den** a pak na hello **Interval** zadejte **1**.</span><span class="sxs-lookup"><span data-stu-id="694f5-122">In hello **Frequency** box, select **Day** and then, in hello **Interval** box, type **1**.</span></span>

    ![Návrhář aplikace logiky "Recurrence" okna](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="694f5-124">Krok 3: Přidání Application Insights akce</span><span class="sxs-lookup"><span data-stu-id="694f5-124">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="694f5-125">Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="694f5-125">Click **New step**, and then click **Add an action**.</span></span>

2. <span data-ttu-id="694f5-126">V hello **vybrat akci** vyhledávací pole, typ **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="694f5-126">In hello **Choose an action** search box, type **Azure Application Insights**.</span></span>

3. <span data-ttu-id="694f5-127">V části **akce**, klikněte na tlačítko **Azure Application Insights – vizualizovat analýzy dotaz Preview**.</span><span class="sxs-lookup"><span data-stu-id="694f5-127">Under **Actions**, click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    !["Vyberte akci" návrhář aplikace na základě logiky – okno](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a><span data-ttu-id="694f5-129">Krok 4: Připojení tooan prostředek Application Insights</span><span class="sxs-lookup"><span data-stu-id="694f5-129">Step 4: Connect tooan Application Insights resource</span></span>

<span data-ttu-id="694f5-130">toocomplete tohoto kroku budete potřebovat ID aplikací a klíč rozhraní API pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="694f5-130">toocomplete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="694f5-131">Můžete je znovu načíst z hello portál Azure, jak je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="694f5-131">You can retrieve them from hello Azure portal, as shown in hello following diagram:</span></span>

![ID aplikace v hello portálu Azure](./media/automate-with-logic-apps/appid.png) 

<span data-ttu-id="694f5-133">Zadejte název vašeho připojení, ID aplikace hello a klíč rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="694f5-133">Provide a name for your connection, hello application ID, and hello API key.</span></span>

![Okno připojení toku návrhář aplikace na základě logiky](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a><span data-ttu-id="694f5-135">Krok 5: Zadejte hello Analytics dotazu a typ grafu</span><span class="sxs-lookup"><span data-stu-id="694f5-135">Step 5: Specify hello Analytics query and chart type</span></span>
<span data-ttu-id="694f5-136">V následujícím příkladu hello hello dotaz vybere hello se nezdařilo požadavky v rámci hello poslední den a jejich koreluje s výjimkami, k nimž došlo v rámci operace hello.</span><span class="sxs-lookup"><span data-stu-id="694f5-136">In hello following example, hello query selects hello failed requests within hello last day and correlates them with exceptions that occurred as part of hello operation.</span></span> <span data-ttu-id="694f5-137">Analýza korelaci požadavky hello se nezdařilo, založené na identifikátor operation_Id hello.</span><span class="sxs-lookup"><span data-stu-id="694f5-137">Analytics correlates hello failed requests, based on hello operation_Id identifier.</span></span> <span data-ttu-id="694f5-138">dotaz Hello pak segmenty hello výsledky pomocí algoritmu autocluster hello.</span><span class="sxs-lookup"><span data-stu-id="694f5-138">hello query then segments hello results by using hello autocluster algorithm.</span></span> 

<span data-ttu-id="694f5-139">Když vytvoříte vlastní dotazy, ověřte, že fungují správně v Analytics předtím, než ho přidáte tooyour toku.</span><span class="sxs-lookup"><span data-stu-id="694f5-139">When you create your own queries, verify that they are working properly in Analytics before you add it tooyour flow.</span></span>

1. <span data-ttu-id="694f5-140">V hello **dotazu** pole, přidejte následující dotaz Analytics hello:</span><span class="sxs-lookup"><span data-stu-id="694f5-140">In hello **Query** box, add hello following Analytics query:</span></span> 

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

2. <span data-ttu-id="694f5-141">V hello **typ grafu** vyberte **tabulky Html**.</span><span class="sxs-lookup"><span data-stu-id="694f5-141">In hello **Chart Type** box, select **Html Table**.</span></span>

    ![Okno Konfigurace analýzy dotazu](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-hello-logic-app-toosend-email"></a><span data-ttu-id="694f5-143">Krok 6: Konfigurace hello logiku aplikace toosend e-mailu</span><span class="sxs-lookup"><span data-stu-id="694f5-143">Step 6: Configure hello logic app toosend email</span></span>

1. <span data-ttu-id="694f5-144">Klikněte na tlačítko **nový krok**a potom vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="694f5-144">Click **New step**, and then select **Add an action**.</span></span>

2. <span data-ttu-id="694f5-145">Hello vyhledávacího pole zadejte **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="694f5-145">In hello search box, type **Office 365 Outlook**.</span></span>

3. <span data-ttu-id="694f5-146">Klikněte na tlačítko **Office 365 Outlook – e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="694f5-146">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Výběr aplikace Outlook Office 365](./media/automate-with-logic-apps/flow2b.png)

4. <span data-ttu-id="694f5-148">V hello **e-mailovou zprávu** okně hello následující:</span><span class="sxs-lookup"><span data-stu-id="694f5-148">In hello **Send an email** window, do hello following:</span></span>

   <span data-ttu-id="694f5-149">a.</span><span class="sxs-lookup"><span data-stu-id="694f5-149">a.</span></span> <span data-ttu-id="694f5-150">Zadejte e-mailová adresa příjemce hello hello.</span><span class="sxs-lookup"><span data-stu-id="694f5-150">Type hello email address of hello recipient.</span></span>

   <span data-ttu-id="694f5-151">b.</span><span class="sxs-lookup"><span data-stu-id="694f5-151">b.</span></span> <span data-ttu-id="694f5-152">Zadejte předmět e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="694f5-152">Type a subject for hello email.</span></span>

   <span data-ttu-id="694f5-153">c.</span><span class="sxs-lookup"><span data-stu-id="694f5-153">c.</span></span> <span data-ttu-id="694f5-154">Klikněte kamkoli do hello **textu** pole a zvolte v nabídce hello dynamického obsahu které se otevře ve správné hello **textu**.</span><span class="sxs-lookup"><span data-stu-id="694f5-154">Click anywhere in hello **Body** box and then, on hello dynamic content menu that opens at hello right, select **Body**.</span></span>

   <span data-ttu-id="694f5-155">d.</span><span class="sxs-lookup"><span data-stu-id="694f5-155">d.</span></span> <span data-ttu-id="694f5-156">Klikněte na tlačítko **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="694f5-156">Click **Show advanced options**.</span></span>

      ![Konfigurace aplikace Outlook Office 365](./media/automate-with-logic-apps/flow5.png)

5. <span data-ttu-id="694f5-158">V nabídce dynamického obsahu hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="694f5-158">On hello dynamic content menu, do hello following:</span></span>

    <span data-ttu-id="694f5-159">a.</span><span class="sxs-lookup"><span data-stu-id="694f5-159">a.</span></span> <span data-ttu-id="694f5-160">Vyberte **název přílohy**.</span><span class="sxs-lookup"><span data-stu-id="694f5-160">Select **Attachment Name**.</span></span>

    <span data-ttu-id="694f5-161">b.</span><span class="sxs-lookup"><span data-stu-id="694f5-161">b.</span></span> <span data-ttu-id="694f5-162">Vyberte **obsah přílohy**.</span><span class="sxs-lookup"><span data-stu-id="694f5-162">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="694f5-163">c.</span><span class="sxs-lookup"><span data-stu-id="694f5-163">c.</span></span> <span data-ttu-id="694f5-164">V hello **je HTML** vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="694f5-164">In hello **Is HTML** box, select **Yes**.</span></span>

      ![Konfigurační obrazovce pro Office 365 e-mailu](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a><span data-ttu-id="694f5-166">Krok 7: Uložit a testování aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="694f5-166">Step 7: Save and test your logic app</span></span>
* <span data-ttu-id="694f5-167">Klikněte na tlačítko **Uložit** toosave změny.</span><span class="sxs-lookup"><span data-stu-id="694f5-167">Click **Save** toosave your changes.</span></span>

<span data-ttu-id="694f5-168">Počkejte, až aplikace logiky hello aktivační událost toorun hello, nebo můžete spustit aplikace logiky hello okamžitě výběrem **spustit**.</span><span class="sxs-lookup"><span data-stu-id="694f5-168">You can wait for hello trigger toorun hello logic app, or you can run hello logic app immediately by selecting **Run**.</span></span>

![Obrazovky vytvoření aplikace logiky](./media/automate-with-logic-apps/step7.png)

<span data-ttu-id="694f5-170">Při spuštění aplikace logiky, hello příjemce, které jste zadali v seznamu e-mailu hello obdrží e-mailu, který může vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="694f5-170">When your logic app runs, hello recipients you specified in hello email list will receive an email that looks like hello following:</span></span>

![Logiku aplikace e-mailové zprávy](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a><span data-ttu-id="694f5-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="694f5-172">Next steps</span></span>

- <span data-ttu-id="694f5-173">Další informace o vytváření [analytické dotazy](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="694f5-173">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="694f5-174">Další informace o [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span><span class="sxs-lookup"><span data-stu-id="694f5-174">Learn more about [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span></span>



<!--Link references-->





