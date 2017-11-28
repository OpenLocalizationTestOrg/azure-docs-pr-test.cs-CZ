---
title: "Vytvořte svůj první pracovní postup spojující cloudové aplikace a cloudové služby – Azure Logic Apps | Dokumentace Microsoftu"
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
ms.openlocfilehash: 204bf123509729b60b55c306050cef54aa7fecc5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-first-logic-app-workflow-to-automate-processes-between-cloud-apps-and-cloud-services"></a><span data-ttu-id="5dc54-104">Vytvořte svůj první pracovní postup aplikace logiky pro automatizaci procesů mezi cloudovými aplikacemi a cloudovými službami</span><span class="sxs-lookup"><span data-stu-id="5dc54-104">Create your first logic app workflow to automate processes between cloud apps and cloud services</span></span>

<span data-ttu-id="5dc54-105">Bez psaní jakéhokoli kódu můžete snáze a rychleji automatizovat firemní procesy díky vytváření a spouštění pracovních postupů v [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="5dc54-105">Without writing any code, you can automate business processes more easily and quickly when you create and run workflows with [Azure Logic Apps](logic-apps-what-are-logic-apps.md).</span></span> <span data-ttu-id="5dc54-106">Tento první příklad ukazuje, jak vytvořit základní aplikaci logiky, která podle informačního kanálu RSS kontroluje nový obsah na webu.</span><span class="sxs-lookup"><span data-stu-id="5dc54-106">This first example shows how to create a basic logic app workflow that checks an RSS feed for new content on a website.</span></span> <span data-ttu-id="5dc54-107">Jakmile se v informačním kanálu webu objeví nové položky, aplikace logiky odešle e-mailu z účtu v Outlooku nebo Gmailu.</span><span class="sxs-lookup"><span data-stu-id="5dc54-107">When new items appear in the website's feed, the logic app sends email from an Outlook or Gmail account.</span></span>

<span data-ttu-id="5dc54-108">K vytvoření a spuštění aplikace logiky potřebujete tyto věci:</span><span class="sxs-lookup"><span data-stu-id="5dc54-108">To create and run a logic app, you need these items:</span></span>

* <span data-ttu-id="5dc54-109">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc54-109">An Azure subscription.</span></span> <span data-ttu-id="5dc54-110">Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="5dc54-110">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="5dc54-111">Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="5dc54-111">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

  <span data-ttu-id="5dc54-112">Vaše předplatné se používá pro účtování využívání aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-112">Your Azure subscription is used for billing logic app usage.</span></span> <span data-ttu-id="5dc54-113">Přečtěte si, jak funguje [měření využití](../logic-apps/logic-apps-pricing.md) a jaké jsou [ceny](https://azure.microsoft.com/pricing/details/logic-apps) Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="5dc54-113">Learn how [usage metering](../logic-apps/logic-apps-pricing.md) and [pricing](https://azure.microsoft.com/pricing/details/logic-apps) work for Azure Logic Apps.</span></span>

<span data-ttu-id="5dc54-114">Tento příklad také vyžaduje následující položky:</span><span class="sxs-lookup"><span data-stu-id="5dc54-114">Also, this example requires these items:</span></span>

* <span data-ttu-id="5dc54-115">Účet Outlook.com, Office 365 Outlook nebo Gmail</span><span class="sxs-lookup"><span data-stu-id="5dc54-115">An Outlook.com, Office 365 Outlook, or Gmail account</span></span>

    > [!TIP]
    > <span data-ttu-id="5dc54-116">Pokud máte osobní [účet Microsoft](https://account.microsoft.com/account), máte účet Outlook.com.</span><span class="sxs-lookup"><span data-stu-id="5dc54-116">If you have a personal [Microsoft account](https://account.microsoft.com/account), you have an Outlook.com account.</span></span> <span data-ttu-id="5dc54-117">Pokud máte pracovní nebo školní účet Azure, máte účet **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-117">Otherwise, if you have an Azure work or school account, you have an **Office 365 Outlook** account.</span></span>

* <span data-ttu-id="5dc54-118">Odkaz na informační kanál RSS nějakého webu.</span><span class="sxs-lookup"><span data-stu-id="5dc54-118">A link to a website's RSS feed.</span></span> <span data-ttu-id="5dc54-119">V tomto příkladu se používá [informační kanál RSS pro hlavní zprávy z webu CNN.com](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`</span><span class="sxs-lookup"><span data-stu-id="5dc54-119">This example uses the [RSS feed for top stories from the CNN.com website](http://rss.cnn.com/rss/cnn_topstories.rss): `http://rss.cnn.com/rss/cnn_topstories.rss`</span></span>

## <a name="add-a-trigger-that-starts-your-workflow"></a><span data-ttu-id="5dc54-120">Přidání triggeru, který spustí váš pracovní postup</span><span class="sxs-lookup"><span data-stu-id="5dc54-120">Add a trigger that starts your workflow</span></span>

<span data-ttu-id="5dc54-121">[*Trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) je událost, která spustí pracovní postup vaší aplikace logiky a je první položkou, kterou aplikace logiky potřebuje.</span><span class="sxs-lookup"><span data-stu-id="5dc54-121">A [*trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts your logic app workflow and is the first item that your logic app needs.</span></span>

1. <span data-ttu-id="5dc54-122">Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="5dc54-122">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="5dc54-123">V levé nabídce zvolte **Nový** > **Podniková integrace** > **Aplikace logiky**, jak vidíte na tomto obrázku:</span><span class="sxs-lookup"><span data-stu-id="5dc54-123">From the left menu, choose **New** > **Enterprise Integration** > **Logic App** as shown here:</span></span>

     ![Azure Portal, Nový, Podniková integrace, Aplikace logiky](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > <span data-ttu-id="5dc54-125">Můžete také zvolit **Nový**, do vyhledávacího pole zadat `logic app` a stisknout klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="5dc54-125">You can also choose **New**, then in the search box, type `logic app`, and press Enter.</span></span> <span data-ttu-id="5dc54-126">Pak zvolte **Aplikace logiky** > **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-126">Then choose **Logic App** > **Create**.</span></span>

3. <span data-ttu-id="5dc54-127">Pojmenujte svoji novou aplikaci logiky a vyberte své předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc54-127">Name your logic app and select your Azure subscription.</span></span> <span data-ttu-id="5dc54-128">Nyní vytvořte a vyberte skupinu prostředků Azure, která vám pomůže organizovat a spravovat související prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc54-128">Now create or select an Azure resource group, which helps you organize and manage related Azure resources.</span></span> <span data-ttu-id="5dc54-129">Nakonec vyberte umístění datacentra pro hostování aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-129">Finally, select the datacenter location for hosting your logic app.</span></span> <span data-ttu-id="5dc54-130">Až budete hotovi, vyberte **Připnout na řídicí panel** a pak **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-130">When you're ready, choose **Pin to dashboard** and then **Create**.</span></span>

     ![Podrobnosti aplikace logiky](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > <span data-ttu-id="5dc54-132">Když vyberte možnost **Připnout na řídicí panel**, vaše aplikace logiky se po nasazení objeví na řídicím panelu Azure a automaticky se otevře.</span><span class="sxs-lookup"><span data-stu-id="5dc54-132">When you select **Pin to dashboard**, your logic app appears on the Azure dashboard after deployment, and opens automatically.</span></span> <span data-ttu-id="5dc54-133">Pokud se aplikace logiky na řídicím panelu neobjeví, na dlaždici **Všechny prostředky** zvolte **Zobrazit další** a vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-133">If your logic app doesn't appear on the dashboard, on the **All resources** tile, choose **See More**, and select your logic app.</span></span> <span data-ttu-id="5dc54-134">Nebo v nabídce vlevo klikněte na **Další služby**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-134">Or on the left menu, choose **More services**.</span></span> <span data-ttu-id="5dc54-135">V části **Podniková integrace** zvolte **Aplikace logiky** a vyberte svoji aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-135">Under **Enterprise Integration**, choose **Logic Apps**, and select your logic app.</span></span>

4. <span data-ttu-id="5dc54-136">Při prvním otevření aplikace logiky vám Návrhář aplikací logiky nabídne šablony, které můžete použít pro začátek.</span><span class="sxs-lookup"><span data-stu-id="5dc54-136">When you open your logic app for the first time, the Logic App Designer shows templates that you can use to get started.</span></span> <span data-ttu-id="5dc54-137">Tentokrát klikněte na **Prázdná aplikace logiky**, abyste mohli aplikaci vytvořit od začátku.</span><span class="sxs-lookup"><span data-stu-id="5dc54-137">For now, choose **Blank Logic App** so you can build your logic app from scratch.</span></span>

    <span data-ttu-id="5dc54-138">Návrhář aplikace logiky otevře a zobrazuje dostupné služby a *aktivační události* , můžete použít v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-138">The Logic App Designer opens and shows  available services and *triggers* that  you can use in your logic app.</span></span>

5. <span data-ttu-id="5dc54-139">Do vyhledávacího pole zadejte `RSS` a vyberte trigger **RSS – když se publikuje položka informačního zdroje** .</span><span class="sxs-lookup"><span data-stu-id="5dc54-139">In the search box, type `RSS`, and select this trigger: **RSS - When a feed item is published**</span></span> 

    ![Trigger RSS](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. <span data-ttu-id="5dc54-141">Zadejte odkaz na informační zdroj RSS webu, který chcete sledovat.</span><span class="sxs-lookup"><span data-stu-id="5dc54-141">Enter the link for the website's RSS feed that you want to track.</span></span> 

     <span data-ttu-id="5dc54-142">Můžete také změnit **frekvenci** a **interval**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-142">You can also change **Frequency** and **Interval**.</span></span> 
     <span data-ttu-id="5dc54-143">Tato nastavení určují, jak často má aplikace logiky kontrolovat nové položky a vracet všechny položky nalezené za určité období.</span><span class="sxs-lookup"><span data-stu-id="5dc54-143">These settings determine how often your logic app checks for new items and returns all items found during that time span.</span></span>

     <span data-ttu-id="5dc54-144">V našem příkladu budeme hlavní zprávy na webu CNN kontrolovat každý den.</span><span class="sxs-lookup"><span data-stu-id="5dc54-144">For this example, let's check every day for   top stories posted to the CNN website.</span></span>

     ![Nastavení triggeru s informačním kanálem RSS, frekvencí a intervalem](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. <span data-ttu-id="5dc54-146">Teď svoji práci uložte.</span><span class="sxs-lookup"><span data-stu-id="5dc54-146">Save your work for now.</span></span> <span data-ttu-id="5dc54-147">(Na panelu příkazů Návrháře zvolte **Uložit**.)</span><span class="sxs-lookup"><span data-stu-id="5dc54-147">(On the designer command bar, choose **Save**.)</span></span>

   ![Uložení aplikace logiky](media/logic-apps-create-a-logic-app/save-logic-app.png)

   <span data-ttu-id="5dc54-149">Uložením přejde aplikace logiky do aktivního provozu, ale v současné době pouze kontroluje nové položky v zadaném informačním kanálu RSS.</span><span class="sxs-lookup"><span data-stu-id="5dc54-149">When you save, your logic app goes live, but currently, your logic app only checks for new items in the specified RSS feed.</span></span> 
   <span data-ttu-id="5dc54-150">Aby byl tento příklad o něco užitečnější, přidáme akci, kterou aplikace logiky provede po splnění triggeru.</span><span class="sxs-lookup"><span data-stu-id="5dc54-150">To make this example more useful, we add an action that your logic app performs after your trigger fires.</span></span>

## <a name="add-an-action-that-responds-to-your-trigger"></a><span data-ttu-id="5dc54-151">Přidání akce, která reaguje na trigger</span><span class="sxs-lookup"><span data-stu-id="5dc54-151">Add an action that responds to your trigger</span></span>

<span data-ttu-id="5dc54-152">[*Akce*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) je úloha, kterou provádí pracovní postup vaší aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-152">An [*action*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="5dc54-153">Když do aplikace logiky přidáte trigger, můžete přidat akci, která bude provádět určitou operaci s daty generovanými triggerem.</span><span class="sxs-lookup"><span data-stu-id="5dc54-153">After you add a trigger to your logic app, you can add an action to perform operations with data generated by that trigger.</span></span> <span data-ttu-id="5dc54-154">V našem příkladu přidáme akci, která odešle e-mail, jakmile se v informačním kanálu RSS webu objeví nové položky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-154">For our example, we now add an action that sends email when new items appear in the website's RSS feed.</span></span>

1. <span data-ttu-id="5dc54-155">V Návrháři pod triggerem zvolte **Nový krok** > **Přidat akci**, ukazuje tento obrázek:</span><span class="sxs-lookup"><span data-stu-id="5dc54-155">In the designer, under your trigger, choose **New step** > **Add an action** as shown here:</span></span>

   ![Přidání akce](media/logic-apps-create-a-logic-app/add-new-action.png)

   <span data-ttu-id="5dc54-157">Návrhář zobrazí [dostupné konektory](../connectors/apis-list.md), ze kterých můžete vybrat akci, která se má spustit v reakci na trigger.</span><span class="sxs-lookup"><span data-stu-id="5dc54-157">The designer shows [available connectors](../connectors/apis-list.md) so that you can select an action to perform when your trigger fires.</span></span>

2. <span data-ttu-id="5dc54-158">Podle typu vašeho e-mailového účtu postupujte podle kroků pro Outlook nebo Gmail.</span><span class="sxs-lookup"><span data-stu-id="5dc54-158">Based on your email account, follow the steps for Outlook or Gmail.</span></span>

   * <span data-ttu-id="5dc54-159">Když chcete odeslat e-mail z účtu Outlooku, zadejte do vyhledávacího pole `outlook`.</span><span class="sxs-lookup"><span data-stu-id="5dc54-159">To send email from your Outlook account, in the search box, enter `outlook`.</span></span> <span data-ttu-id="5dc54-160">V části **Služby** zvolte **Outlook.com** pro osobní účty Microsoft nebo **Office 365 Outlook** pro pracovní nebo školní účty Azure.</span><span class="sxs-lookup"><span data-stu-id="5dc54-160">Under **Services**, choose **Outlook.com** for personal Microsoft accounts, or choose **Office 365 Outlook** for Azure work or school accounts.</span></span> 
   <span data-ttu-id="5dc54-161">V části **Akce** zvolte **Odeslat e-mail**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-161">Under **Actions**, select **Send an email**.</span></span>

       ![Výběr akce Outlooku „Odeslat e-mail“](media/logic-apps-create-a-logic-app/actions.png)

   * <span data-ttu-id="5dc54-163">Když chcete odeslat e-mail z účtu Gmailu, zadejte do vyhledávacího pole `gmail`.</span><span class="sxs-lookup"><span data-stu-id="5dc54-163">To send email from your Gmail account, in the search box, enter `gmail`.</span></span> 
   <span data-ttu-id="5dc54-164">V části **Akce** zvolte **Odeslat e-mail**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-164">Under **Actions**, select **Send email**.</span></span>

       ![Výběr akce Gmailu „Odeslat e-mail“](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. <span data-ttu-id="5dc54-166">Když se zobrazí výzva k zadání přihlašovacích údajů, zadejte uživatelské jméno a heslo k e-mailovému účtu.</span><span class="sxs-lookup"><span data-stu-id="5dc54-166">When you're prompted for credentials, sign in with the username and password for your email account.</span></span> 

4. <span data-ttu-id="5dc54-167">Zadejte podrobnosti pro tuto akci, jako je cílová e-mailová adresa, a vyberte parametry dat, která se mají do e-mailu zahrnout, například:</span><span class="sxs-lookup"><span data-stu-id="5dc54-167">Provide the details for this action, like the destination email address, and choose the parameters for the data to include in the email, for example:</span></span>

   ![Výběr dat pro zahrnutí do e-mailu](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    <span data-ttu-id="5dc54-169">Pokud jste zvolili Outlook, vaše aplikace logiky může vypadat podobně jako v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="5dc54-169">So if you chose Outlook,  your logic app might look like this example:</span></span>

    ![Hotová aplikace logiky](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  <span data-ttu-id="5dc54-171">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="5dc54-171">Save your changes.</span></span> <span data-ttu-id="5dc54-172">(Na panelu příkazů Návrháře zvolte **Uložit**.)</span><span class="sxs-lookup"><span data-stu-id="5dc54-172">(On the designer command bar, choose **Save**.)</span></span>

6. <span data-ttu-id="5dc54-173">Nyní můžete aplikaci logiky ručně spustit a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="5dc54-173">You can now manually run your logic app for testing.</span></span> <span data-ttu-id="5dc54-174">Na panelu příkazů Návrháře zvolte **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-174">On the designer command bar, choose **Run**.</span></span> <span data-ttu-id="5dc54-175">Můžete také nechat aplikaci logiky zkontrolovat zadaný informační kanál RSS podle nastaveného plánu.</span><span class="sxs-lookup"><span data-stu-id="5dc54-175">Otherwise, you can let your logic app check the specified RSS feed based on the schedule that you set up.</span></span>

   <span data-ttu-id="5dc54-176">Pokud aplikace logiky najde nové položky, odešle e-mail obsahující požadovaná data.</span><span class="sxs-lookup"><span data-stu-id="5dc54-176">If your logic app finds new items, the logic app sends email that includes your selected data.</span></span> 
   <span data-ttu-id="5dc54-177">Když žádné nové položky nenajde, přeskočí akci, která odesílá e-mail.</span><span class="sxs-lookup"><span data-stu-id="5dc54-177">If no new items are found, your logic app skips the action that sends email.</span></span>

7. <span data-ttu-id="5dc54-178">Ke sledování a kontrole činnosti a historie spouštění vaší aplikaci logiky vyberte v nabídce aplikace logiky možnost **Přehled**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-178">To monitor and check your logic app's run and trigger history, on your logic app menu, choose **Overview**.</span></span>

   ![Sledování a kontrola činnosti a historie spouštění aplikaci logiky](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > <span data-ttu-id="5dc54-180">Pokud nenajdete očekávaná data, na panelu příkazů zkuste vybrat možnost **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-180">If you don't find the data that you expect, on the command bar, try choosing **Refresh**.</span></span>

   <span data-ttu-id="5dc54-181">Další informace o stavu a historie spouštění vaší aplikace logiky a o její diagnostice najdete v článku [Odstraňovaní potíží s aplikací logiky](logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="5dc54-181">To learn more about your logic app's status or run and trigger history, or to diagnose your logic app, see [Troubleshoot your logic app](logic-apps-diagnosing-failures.md).</span></span>

      > [!NOTE]
      > <span data-ttu-id="5dc54-182">Aplikace logiky se bude spouštět dál, dokud ji nevypnete.</span><span class="sxs-lookup"><span data-stu-id="5dc54-182">Your logic app continues running until you turn off your app.</span></span> <span data-ttu-id="5dc54-183">Pokud chcete aplikaci dočasně vypnout, v nabídce aplikace logiky zvolte **Přehled**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-183">To turn off your app for now, on your logic app menu, choose **Overview**.</span></span> <span data-ttu-id="5dc54-184">Na panelu příkazů zvolte **Zakázat**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-184">On the command bar, choose **Disable**.</span></span>

<span data-ttu-id="5dc54-185">Gratulujeme, právě jste vytvořili a spustili svoji první základní aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-185">Congratulations, you just set up and run your first basic logic app.</span></span> <span data-ttu-id="5dc54-186">Také jste se naučili, jak snadno vytvořit pracovní postupy pro automatizaci procesů, jak a integrovat cloudové aplikace a cloudové služby – a všechno bez programování.</span><span class="sxs-lookup"><span data-stu-id="5dc54-186">You also learned how easily you can create workflows that automate processes, and integrate cloud apps and cloud services - all without code.</span></span>

## <a name="manage-your-logic-app"></a><span data-ttu-id="5dc54-187">Správa aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="5dc54-187">Manage your logic app</span></span>

<span data-ttu-id="5dc54-188">Při správě své aplikace můžete provádět úkoly jako je kontrola stavu, úpravy, zobrazení historie, vypnutí nebo odstranění aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-188">To manage your app, you can perform tasks like check the status, edit, view history, turn off, or delete your logic app.</span></span>

1. <span data-ttu-id="5dc54-189">Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="5dc54-189">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span>

2. <span data-ttu-id="5dc54-190">V nabídce vlevo klikněte na **Další služby**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-190">On the left menu, choose **More services**.</span></span> <span data-ttu-id="5dc54-191">V části **Podniková integrace** zvolte **Logic Apps**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-191">Under **Enterprise Integration**, choose **Logic Apps**.</span></span> <span data-ttu-id="5dc54-192">Vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="5dc54-192">Select your logic app.</span></span> 

   <span data-ttu-id="5dc54-193">V nabídce aplikace logiky najdete tyto úkoly správy:</span><span class="sxs-lookup"><span data-stu-id="5dc54-193">In the logic app menu, you can find these logic app management tasks:</span></span>

   |<span data-ttu-id="5dc54-194">Úkol</span><span class="sxs-lookup"><span data-stu-id="5dc54-194">Task</span></span>|<span data-ttu-id="5dc54-195">Kroky</span><span class="sxs-lookup"><span data-stu-id="5dc54-195">Steps</span></span>| 
   |:---|:---| 
   | <span data-ttu-id="5dc54-196">Zobrazení stavu aplikace, historie spouštění a obecných informací</span><span class="sxs-lookup"><span data-stu-id="5dc54-196">View your app's status, execution history, and general information</span></span>| <span data-ttu-id="5dc54-197">Zvolte **Přehled**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-197">Choose **Overview**.</span></span>| 
   | <span data-ttu-id="5dc54-198">Úprava aplikace</span><span class="sxs-lookup"><span data-stu-id="5dc54-198">Edit your app</span></span> | <span data-ttu-id="5dc54-199">Zvolte **Návrhář aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-199">Choose **Logic App Designer**.</span></span> | 
   | <span data-ttu-id="5dc54-200">Zobrazení definice JSON pracovního postupu vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="5dc54-200">View your app's workflow JSON definition</span></span> | <span data-ttu-id="5dc54-201">Zvolte **Zobrazení kódu aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-201">Choose **Logic App Code View**.</span></span> | 
   | <span data-ttu-id="5dc54-202">Zobrazení operací, které aplikace logiky provedla</span><span class="sxs-lookup"><span data-stu-id="5dc54-202">View operations performed on your logic app</span></span> | <span data-ttu-id="5dc54-203">Zvolte **Protokol aktivit**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-203">Choose **Activity log**.</span></span> | 
   | <span data-ttu-id="5dc54-204">Zobrazení předchozích verzí aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="5dc54-204">View past versions for your logic app</span></span> | <span data-ttu-id="5dc54-205">Zvolte **Verze**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-205">Choose **Versions**.</span></span> | 
   | <span data-ttu-id="5dc54-206">Dočasné vypnutí aplikace</span><span class="sxs-lookup"><span data-stu-id="5dc54-206">Turn off your app temporarily</span></span> | <span data-ttu-id="5dc54-207">Zvolte **Přehled** a pak na panelu příkazů **Zakázat**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-207">Choose **Overview**, then on the command bar, choose **Disable**.</span></span> | 
   | <span data-ttu-id="5dc54-208">Odstranění aplikace</span><span class="sxs-lookup"><span data-stu-id="5dc54-208">Delete your app</span></span> | <span data-ttu-id="5dc54-209">Zvolte **Přehled** a pak na panelu příkazů **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-209">Choose **Overview**, then on the command bar, choose **Delete**.</span></span> <span data-ttu-id="5dc54-210">Zadejte název aplikace logiky a zvolte **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="5dc54-210">Enter your logic app's name, and choose **Delete**.</span></span> | 

## <a name="get-help"></a><span data-ttu-id="5dc54-211">Podpora</span><span class="sxs-lookup"><span data-stu-id="5dc54-211">Get help</span></span>

<span data-ttu-id="5dc54-212">Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="5dc54-212">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="5dc54-213">Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="5dc54-213">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5dc54-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5dc54-214">Next steps</span></span>

*  [<span data-ttu-id="5dc54-215">Přidání podmínek a spuštění pracovních postupů</span><span class="sxs-lookup"><span data-stu-id="5dc54-215">Add conditions and run workflows</span></span>](../logic-apps/logic-apps-use-logic-app-features.md)
*    [<span data-ttu-id="5dc54-216">Šablony pro aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="5dc54-216">Logic app templates</span></span>](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [<span data-ttu-id="5dc54-217">Vytváření aplikací logiky ze šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="5dc54-217">Create logic apps from Azure Resource Manager templates</span></span>](../logic-apps/logic-apps-arm-provision.md)
