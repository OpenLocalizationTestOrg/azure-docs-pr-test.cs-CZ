---
title: "aaaCreate prvního pracovního postupu mezi cloudové aplikace a cloudové služby - Azure Logic Apps | Microsoft Docs"
description: "Přečtěte si, jak automatizovat firemní procesy pro systémovou integraci a integraci podnikových aplikací (EAI) pomocí vytváření a spouštění pracovních postupů v Azure Logic Apps."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
keywords: "pracovní postup, cloudové aplikace, cloudové služby, firemní procesy, systémová integrace, integrace podnikových aplikací, enterprise application integration, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.openlocfilehash: 17ec589b1c8923b5ad3e6479fc856b6ac81754ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-logic-app-workflow-tooautomate-processes-between-cloud-apps-and-cloud-services"></a><span data-ttu-id="fc9e5-104">Vytvoření první aplikace logiky pracovního postupu tooautomate procesy mezi cloudových aplikací a cloudové služby</span><span class="sxs-lookup"><span data-stu-id="fc9e5-104">Create your first logic app workflow tooautomate processes between cloud apps and cloud services</span></span>

<span data-ttu-id="fc9e5-105">Bez psaní jakéhokoli kódu můžete snáze a rychleji automatizovat firemní procesy díky vytváření a spouštění pracovních postupů v [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="fc9e5-105">Without writing any code, you can automate business processes more easily and quickly when you create and run workflows with [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span></span> <span data-ttu-id="fc9e5-106">Tento první příklad ukazuje, jak toocreate aplikace základní logika pracovního postupu, který kontroluje informačního kanálu RSS kanálu pro nový obsah na webu.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-106">This first example shows how toocreate a basic logic app workflow that checks an RSS feed for new content on a website.</span></span> <span data-ttu-id="fc9e5-107">Jakmile se zobrazí nových položek v informačním kanálu hello webu, aplikace logiky hello odešle e-mailu z účtu aplikaci Outlook nebo z Gmailu.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-107">When new items appear in hello website's feed, hello logic app sends email from an Outlook or Gmail account.</span></span>

<span data-ttu-id="fc9e5-108">toocreate a spusťte aplikaci logiky, budete potřebovat tyto položky:</span><span class="sxs-lookup"><span data-stu-id="fc9e5-108">toocreate and run a logic app, you need these items:</span></span>

* <span data-ttu-id="fc9e5-109">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-109">An Azure subscription.</span></span> <span data-ttu-id="fc9e5-110">Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="fc9e5-110">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="fc9e5-111">Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="fc9e5-111">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

  <span data-ttu-id="fc9e5-112">Vaše předplatné se používá pro účtování využívání aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-112">Your Azure subscription is used for billing logic app usage.</span></span> <span data-ttu-id="fc9e5-113">Přečtěte si, jak funguje [měření využití](../logic-apps/logic-apps-pricing.md) a jaké jsou [ceny](https://azure.microsoft.com/pricing/details/logic-apps) Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-113">Learn how [usage metering](../logic-apps/logic-apps-pricing.md) and [pricing](https://azure.microsoft.com/pricing/details/logic-apps) work for Azure Logic Apps.</span></span>

<span data-ttu-id="fc9e5-114">Tento příklad také vyžaduje následující položky:</span><span class="sxs-lookup"><span data-stu-id="fc9e5-114">Also, this example requires these items:</span></span>

* <span data-ttu-id="fc9e5-115">Účet Outlook.com, Office 365 Outlook nebo Gmail</span><span class="sxs-lookup"><span data-stu-id="fc9e5-115">An Outlook.com, Office 365 Outlook, or Gmail account</span></span>

    > [!TIP]
    > <span data-ttu-id="fc9e5-116">Pokud máte osobní [účet Microsoft](https://account.microsoft.com/account), máte účet Outlook.com.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-116">If you have a personal [Microsoft account](https://account.microsoft.com/account), you have an Outlook.com account.</span></span> <span data-ttu-id="fc9e5-117">Pokud máte pracovní nebo školní účet Azure, máte účet **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-117">Otherwise, if you have an Azure work or school account, you have an **Office 365 Outlook** account.</span></span>

* <span data-ttu-id="fc9e5-118">Tooa odkaz informačního kanálu RSS webu.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-118">A link tooa website's RSS feed.</span></span> <span data-ttu-id="fc9e5-119">Tento příklad používá hello [RSS pro horní scénářů z webu CNN.com hello](http://rss.cnn.com/rss/cnn_topstories.rss):`http://rss.cnn.com/rss/cnn_topstories.rss`</span><span class="sxs-lookup"><span data-stu-id="fc9e5-119">This example uses hello [RSS feed for top stories from hello CNN.com website](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`</span></span>

## <a name="add-a-trigger-that-starts-your-workflow"></a><span data-ttu-id="fc9e5-120">Přidání triggeru, který spustí váš pracovní postup</span><span class="sxs-lookup"><span data-stu-id="fc9e5-120">Add a trigger that starts your workflow</span></span>

<span data-ttu-id="fc9e5-121">A [ *aktivační událost* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) událost, která spustí pracovní postup aplikace logiky a je hello první položku, která potřebuje aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-121">A [*trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts your logic app workflow and is hello first item that your logic app needs.</span></span>

1. <span data-ttu-id="fc9e5-122">Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="fc9e5-122">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="fc9e5-123">V levé nabídce hello zvolte **nový** > **Enterprise integrace** > **aplikace logiky** jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="fc9e5-123">From hello left menu, choose **New** > **Enterprise Integration** > **Logic App** as shown here:</span></span>

     ![Azure Portal, Nový, Podniková integrace, Aplikace logiky](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > <span data-ttu-id="fc9e5-125">Můžete také **nový**, zadejte do vyhledávacího pole hello `logic app`, a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-125">You can also choose **New**, then in hello search box, type `logic app`, and press Enter.</span></span> <span data-ttu-id="fc9e5-126">Pak zvolte **Aplikace logiky** > **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-126">Then choose **Logic App** > **Create**.</span></span>

3. <span data-ttu-id="fc9e5-127">Pojmenujte svoji novou aplikaci logiky a vyberte své předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-127">Name your logic app and select your Azure subscription.</span></span> <span data-ttu-id="fc9e5-128">Nyní vytvořte a vyberte skupinu prostředků Azure, která vám pomůže organizovat a spravovat související prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-128">Now create or select an Azure resource group, which helps you organize and manage related Azure resources.</span></span> <span data-ttu-id="fc9e5-129">Nakonec vyberte hello datacenter umístění pro hostování aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-129">Finally, select hello datacenter location for hosting your logic app.</span></span> <span data-ttu-id="fc9e5-130">Až budete připraveni, zvolte **Pin toodashboard** a potom **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-130">When you're ready, choose **Pin toodashboard** and then **Create**.</span></span>

     ![Podrobnosti aplikace logiky](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > <span data-ttu-id="fc9e5-132">Když vyberete **Pin toodashboard**, se zobrazí na hello řídicí panel Azure po nasazení aplikace logiky a automaticky otevře.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-132">When you select **Pin toodashboard**, your logic app appears on hello Azure dashboard after deployment, and opens automatically.</span></span> <span data-ttu-id="fc9e5-133">Pokud svou aplikaci logiky nezobrazí na řídicím panelu hello na hello **všechny prostředky** dlaždici, zvolte **najdete v části více**a vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-133">If your logic app doesn't appear on hello dashboard, on hello **All resources** tile, choose **See More**, and select your logic app.</span></span> <span data-ttu-id="fc9e5-134">Nebo vyberte v levé nabídce hello **další služby**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-134">Or on hello left menu, choose **More services**.</span></span> <span data-ttu-id="fc9e5-135">V části **Podniková integrace** zvolte **Aplikace logiky** a vyberte svoji aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-135">Under **Enterprise Integration**, choose **Logic Apps**, and select your logic app.</span></span>

4. <span data-ttu-id="fc9e5-136">Při prvním otevření aplikace logiky pro hello ukazuje hello logiku aplikace Návrhář šablony, které můžete použít tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-136">When you open your logic app for hello first time, hello Logic App Designer shows templates that you can use tooget started.</span></span> <span data-ttu-id="fc9e5-137">Tentokrát klikněte na **Prázdná aplikace logiky**, abyste mohli aplikaci vytvořit od začátku.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-137">For now, choose **Blank Logic App** so you can build your logic app from scratch.</span></span>

    <span data-ttu-id="fc9e5-138">Hello logiku aplikace Designer otevře a zobrazuje dostupné služby a *aktivační události* , můžete použít v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-138">hello Logic App Designer opens and shows  available services and *triggers* that  you can use in your logic app.</span></span>

5. <span data-ttu-id="fc9e5-139">Hello vyhledávacího pole zadejte `RSS`a vyberte této aktivační události: **RSS – Pokud je publikovaná v položku informačního kanálu**</span><span class="sxs-lookup"><span data-stu-id="fc9e5-139">In hello search box, type `RSS`, and select this trigger: **RSS - When a feed item is published**</span></span> 

    ![Trigger RSS](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. <span data-ttu-id="fc9e5-141">Zadejte hello odkaz pro informační kanál RSS hello webu, které chcete tootrack.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-141">Enter hello link for hello website's RSS feed that you want tootrack.</span></span> 

     <span data-ttu-id="fc9e5-142">Můžete také změnit **frekvenci** a **interval**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-142">You can also change **Frequency** and **Interval**.</span></span> 
     <span data-ttu-id="fc9e5-143">Tato nastavení určují, jak často má aplikace logiky kontrolovat nové položky a vracet všechny položky nalezené za určité období.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-143">These settings determine how often your logic app checks for new items and returns all items found during that time span.</span></span>

     <span data-ttu-id="fc9e5-144">V tomto příkladu Podíváme se každý den pro horní scénářů odeslány toohello CNN webu.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-144">For this example, let's check every day for   top stories posted toohello CNN website.</span></span>

     ![Nastavení triggeru s informačním kanálem RSS, frekvencí a intervalem](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. <span data-ttu-id="fc9e5-146">Teď svoji práci uložte.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-146">Save your work for now.</span></span> <span data-ttu-id="fc9e5-147">(Na hello návrháře řádku nabídek zvolte **Uložit**.)</span><span class="sxs-lookup"><span data-stu-id="fc9e5-147">(On hello designer command bar, choose **Save**.)</span></span>

   ![Uložení aplikace logiky](media/logic-apps-create-a-logic-app/save-logic-app.png)

   <span data-ttu-id="fc9e5-149">Pokud uložíte, aplikace logiky přejde za provozu, ale v současné době svou aplikaci logiky pouze vyhledává nové položky v hello zadaná informační kanál RSS.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-149">When you save, your logic app goes live, but currently, your logic app only checks for new items in hello specified RSS feed.</span></span> 
   <span data-ttu-id="fc9e5-150">toomake aktivuje se v tomto příkladu užitečnější, že přidáme akci, která provede aplikace logiky po aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-150">toomake this example more useful, we add an action that your logic app performs after your trigger fires.</span></span>

## <a name="add-an-action-that-responds-tooyour-trigger"></a><span data-ttu-id="fc9e5-151">Přidat akci, která odpovídá tooyour aktivační události</span><span class="sxs-lookup"><span data-stu-id="fc9e5-151">Add an action that responds tooyour trigger</span></span>

<span data-ttu-id="fc9e5-152">[*Akce*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) je úloha, kterou provádí pracovní postup vaší aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-152">An [*action*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="fc9e5-153">Po přidání aplikace logiky tooyour aktivační událost, můžete přidat akci tooperform operace s daty generovanými této aktivační události.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-153">After you add a trigger tooyour logic app, you can add an action tooperform operations with data generated by that trigger.</span></span> <span data-ttu-id="fc9e5-154">Pro náš příklad teď nemůžeme přidat akci, která se pošle e-mailu, když se objeví nové položky v informačního kanálu RSS hello webu.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-154">For our example, we now add an action that sends email when new items appear in hello website's RSS feed.</span></span>

1. <span data-ttu-id="fc9e5-155">V Návrháři hello pod aktivační událost, zvolte **nový krok** > **přidat akci** jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="fc9e5-155">In hello designer, under your trigger, choose **New step** > **Add an action** as shown here:</span></span>

   ![Přidání akce](media/logic-apps-create-a-logic-app/add-new-action.png)

   <span data-ttu-id="fc9e5-157">Hello návrháře ukazuje [dostupných konektorů](../connectors/apis-list.md) tak, aby tooperform akce můžete vybrat, pokud aktivuje aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-157">hello designer shows [available connectors](../connectors/apis-list.md) so that you can select an action tooperform when your trigger fires.</span></span>

2. <span data-ttu-id="fc9e5-158">Podle e-mailový účet, postupujte podle kroků hello pro aplikaci Outlook nebo z Gmailu.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-158">Based on your email account, follow hello steps for Outlook or Gmail.</span></span>

   * <span data-ttu-id="fc9e5-159">e-mailu toosend z účtu Outlook hello vyhledávacího pole zadejte `outlook`.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-159">toosend email from your Outlook account, in hello search box, enter `outlook`.</span></span> <span data-ttu-id="fc9e5-160">V části **Služby** zvolte **Outlook.com** pro osobní účty Microsoft nebo **Office 365 Outlook** pro pracovní nebo školní účty Azure.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-160">Under **Services**, choose **Outlook.com** for personal Microsoft accounts, or choose **Office 365 Outlook** for Azure work or school accounts.</span></span> 
   <span data-ttu-id="fc9e5-161">V části **Akce** zvolte **Odeslat e-mail**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-161">Under **Actions**, select **Send an email**.</span></span>

       ![Výběr akce Outlooku „Odeslat e-mail“](media/logic-apps-create-a-logic-app/actions.png)

   * <span data-ttu-id="fc9e5-163">e-mailu toosend z účtu z Gmailu hello vyhledávacího pole zadejte `gmail`.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-163">toosend email from your Gmail account, in hello search box, enter `gmail`.</span></span> 
   <span data-ttu-id="fc9e5-164">V části **Akce** zvolte **Odeslat e-mail**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-164">Under **Actions**, select **Send email**.</span></span>

       ![Výběr akce Gmailu „Odeslat e-mail“](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. <span data-ttu-id="fc9e5-166">Když se zobrazí výzva k zadání pověření, přihlaste se pomocí hello uživatelské jméno a heslo pro e-mailový účet.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-166">When you're prompted for credentials, sign in with hello username and password for your email account.</span></span> 

4. <span data-ttu-id="fc9e5-167">Zadejte hello podrobnosti pro tuto akci, jako je hello cílové e-mailovou adresu a zvolte hello parametry pro hello data tooinclude hello e-mailem, například:</span><span class="sxs-lookup"><span data-stu-id="fc9e5-167">Provide hello details for this action, like hello destination email address, and choose hello parameters for hello data tooinclude in hello email, for example:</span></span>

   ![Vyberte data tooinclude v e-mailu](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    <span data-ttu-id="fc9e5-169">Pokud jste zvolili Outlook, vaše aplikace logiky může vypadat podobně jako v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="fc9e5-169">So if you chose Outlook,  your logic app might look like this example:</span></span>

    ![Hotová aplikace logiky](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  <span data-ttu-id="fc9e5-171">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-171">Save your changes.</span></span> <span data-ttu-id="fc9e5-172">(Na hello návrháře řádku nabídek zvolte **Uložit**.)</span><span class="sxs-lookup"><span data-stu-id="fc9e5-172">(On hello designer command bar, choose **Save**.)</span></span>

6. <span data-ttu-id="fc9e5-173">Nyní můžete aplikaci logiky ručně spustit a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-173">You can now manually run your logic app for testing.</span></span> <span data-ttu-id="fc9e5-174">Na hello návrháře řádku nabídek zvolte **spustit**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-174">On hello designer command bar, choose **Run**.</span></span> <span data-ttu-id="fc9e5-175">Jinak můžete je nechat aplikaci logiky, zkontrolujte, zda text hello zadal informační kanál RSS podle plánu hello, které jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-175">Otherwise, you can let your logic app check hello specified RSS feed based on hello schedule that you set up.</span></span>

   <span data-ttu-id="fc9e5-176">Pokud svou aplikaci logiky najde nové položky, hello logiku aplikace odešle e-mailu, který zahrnuje vybraná data.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-176">If your logic app finds new items, hello logic app sends email that includes your selected data.</span></span> 
   <span data-ttu-id="fc9e5-177">Pokud nebudou nalezeny žádné nové položky, přeskočí aplikace logiky hello akci, která odešle e-mail.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-177">If no new items are found, your logic app skips hello action that sends email.</span></span>

7. <span data-ttu-id="fc9e5-178">toomonitor a zkontrolujte svou aplikaci logiky je spuštění a aktivaci historie, v nabídce aplikace logiky, vyberte **přehled**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-178">toomonitor and check your logic app's run and trigger history, on your logic app menu, choose **Overview**.</span></span>

   ![Sledování a kontrola činnosti a historie spouštění aplikaci logiky](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > <span data-ttu-id="fc9e5-180">Pokud nenajdete hello data, která očekáváte, na panelu příkazů hello, opakujte výběr **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-180">If you don't find hello data that you expect, on hello command bar, try choosing **Refresh**.</span></span>

   <span data-ttu-id="fc9e5-181">Další informace o stavu aplikace logiky toolearn nebo spuštění a aktivaci historie nebo toodiagnose svou aplikaci logiky najdete v tématu [řešení aplikace logiky](logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="fc9e5-181">toolearn more about your logic app's status or run and trigger history, or toodiagnose your logic app, see [Troubleshoot your logic app](logic-apps-diagnosing-failures.md).</span></span>

      > [!NOTE]
      > <span data-ttu-id="fc9e5-182">Aplikace logiky se bude spouštět dál, dokud ji nevypnete.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-182">Your logic app continues running until you turn off your app.</span></span> <span data-ttu-id="fc9e5-183">Zvolte tooturn vypnout vaší aplikace pro tuto chvíli se v nabídce aplikace logiky, **přehled**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-183">tooturn off your app for now, on your logic app menu, choose **Overview**.</span></span> <span data-ttu-id="fc9e5-184">Na panelu příkazů hello, zvolte **zakázat**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-184">On hello command bar, choose **Disable**.</span></span>

<span data-ttu-id="fc9e5-185">Gratulujeme, právě jste vytvořili a spustili svoji první základní aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-185">Congratulations, you just set up and run your first basic logic app.</span></span> <span data-ttu-id="fc9e5-186">Také jste se naučili, jak snadno vytvořit pracovní postupy pro automatizaci procesů, jak a integrovat cloudové aplikace a cloudové služby – a všechno bez programování.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-186">You also learned how easily you can create workflows that automate processes, and integrate cloud apps and cloud services - all without code.</span></span>

## <a name="manage-your-logic-app"></a><span data-ttu-id="fc9e5-187">Správa aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="fc9e5-187">Manage your logic app</span></span>

<span data-ttu-id="fc9e5-188">toomanage aplikace, můžete provádět úlohy, jako zkontrolujte stav hello, upravit, zobrazit historii, vypnout nebo odstranit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-188">toomanage your app, you can perform tasks like check hello status, edit, view history, turn off, or delete your logic app.</span></span>

1. <span data-ttu-id="fc9e5-189">Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="fc9e5-189">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="fc9e5-190">V levé nabídce hello zvolte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-190">On hello left menu, choose **More services**.</span></span> <span data-ttu-id="fc9e5-191">V části **Podniková integrace** zvolte **Logic Apps**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-191">Under **Enterprise Integration**, choose **Logic Apps**.</span></span> <span data-ttu-id="fc9e5-192">Vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-192">Select your logic app.</span></span> 

   <span data-ttu-id="fc9e5-193">V nabídce aplikace logiky hello můžete najít tyto úlohy správy logiku aplikace:</span><span class="sxs-lookup"><span data-stu-id="fc9e5-193">In hello logic app menu, you can find these logic app management tasks:</span></span>

   |<span data-ttu-id="fc9e5-194">Úkol</span><span class="sxs-lookup"><span data-stu-id="fc9e5-194">Task</span></span>|<span data-ttu-id="fc9e5-195">Kroky</span><span class="sxs-lookup"><span data-stu-id="fc9e5-195">Steps</span></span>| 
   |:---|:---| 
   | <span data-ttu-id="fc9e5-196">Zobrazení stavu aplikace, historie spouštění a obecných informací</span><span class="sxs-lookup"><span data-stu-id="fc9e5-196">View your app's status, execution history, and general information</span></span>| <span data-ttu-id="fc9e5-197">Zvolte **Přehled**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-197">Choose **Overview**.</span></span>| 
   | <span data-ttu-id="fc9e5-198">Úprava aplikace</span><span class="sxs-lookup"><span data-stu-id="fc9e5-198">Edit your app</span></span> | <span data-ttu-id="fc9e5-199">Zvolte **Návrhář aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-199">Choose **Logic App Designer**.</span></span> | 
   | <span data-ttu-id="fc9e5-200">Zobrazení definice JSON pracovního postupu vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="fc9e5-200">View your app's workflow JSON definition</span></span> | <span data-ttu-id="fc9e5-201">Zvolte **Zobrazení kódu aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-201">Choose **Logic App Code View**.</span></span> | 
   | <span data-ttu-id="fc9e5-202">Zobrazení operací, které aplikace logiky provedla</span><span class="sxs-lookup"><span data-stu-id="fc9e5-202">View operations performed on your logic app</span></span> | <span data-ttu-id="fc9e5-203">Zvolte **Protokol aktivit**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-203">Choose **Activity log**.</span></span> | 
   | <span data-ttu-id="fc9e5-204">Zobrazení předchozích verzí aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="fc9e5-204">View past versions for your logic app</span></span> | <span data-ttu-id="fc9e5-205">Zvolte **Verze**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-205">Choose **Versions**.</span></span> | 
   | <span data-ttu-id="fc9e5-206">Dočasné vypnutí aplikace</span><span class="sxs-lookup"><span data-stu-id="fc9e5-206">Turn off your app temporarily</span></span> | <span data-ttu-id="fc9e5-207">Zvolte **přehled**, potom na panelu příkazů hello, zvolte **zakázat**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-207">Choose **Overview**, then on hello command bar, choose **Disable**.</span></span> | 
   | <span data-ttu-id="fc9e5-208">Odstranění aplikace</span><span class="sxs-lookup"><span data-stu-id="fc9e5-208">Delete your app</span></span> | <span data-ttu-id="fc9e5-209">Zvolte **přehled**, potom na panelu příkazů hello, zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-209">Choose **Overview**, then on hello command bar, choose **Delete**.</span></span> <span data-ttu-id="fc9e5-210">Zadejte název aplikace logiky a zvolte **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fc9e5-210">Enter your logic app's name, and choose **Delete**.</span></span> | 

## <a name="get-help"></a><span data-ttu-id="fc9e5-211">Podpora</span><span class="sxs-lookup"><span data-stu-id="fc9e5-211">Get help</span></span>

<span data-ttu-id="fc9e5-212">tooask otázky, odpovědi na otázky a zjistěte, jaké další Azure Logic Apps uživatelé dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="fc9e5-212">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="fc9e5-213">toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="fc9e5-213">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc9e5-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc9e5-214">Next steps</span></span>

*  [<span data-ttu-id="fc9e5-215">Přidání podmínek a spuštění pracovních postupů</span><span class="sxs-lookup"><span data-stu-id="fc9e5-215">Add conditions and run workflows</span></span>](../logic-apps/logic-apps-use-logic-app-features.md)
*    [<span data-ttu-id="fc9e5-216">Šablony pro aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="fc9e5-216">Logic app templates</span></span>](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [<span data-ttu-id="fc9e5-217">Vytváření aplikací logiky ze šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="fc9e5-217">Create logic apps from Azure Resource Manager templates</span></span>](../logic-apps/logic-apps-arm-provision.md)
