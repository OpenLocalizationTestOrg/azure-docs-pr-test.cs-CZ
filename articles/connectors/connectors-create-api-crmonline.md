---
title: "Připojit k aplikaci Dynamics 365 (online) z Azure Logic Apps | Microsoft Docs"
description: "Vytvořit logiku aplikace pracovní postupy, které spravují Dynamics 365 entity (online) prostřednictvím rozhraní API poskytované konektor Dynamics 365"
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: d35647921ff540167a3a591fb489d3bab031a5c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-dynamics-365-from-logic-app-workflows"></a><span data-ttu-id="e9247-103">Připojit k aplikaci Dynamics 365 z pracovní postupy aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e9247-103">Connect to Dynamics 365 from logic app workflows</span></span>

<span data-ttu-id="e9247-104">S Logic Apps můžete připojit k aplikaci Dynamics 365 (online) a vytvořit toky užitečné firmy, které vytvořit záznamy, aktualizovat položky nebo vrátí seznam záznamů.</span><span class="sxs-lookup"><span data-stu-id="e9247-104">With Logic Apps, you can connect to Dynamics 365 (online) and create useful business flows that create records, update items, or return a list of records.</span></span> <span data-ttu-id="e9247-105">Tento konektor Dynamics 365 můžete:</span><span class="sxs-lookup"><span data-stu-id="e9247-105">With the Dynamics 365 connector, you can:</span></span>

* <span data-ttu-id="e9247-106">Sestavení vaší firmy toku na základě dat, které můžete získat z Dynamics 365 (online).</span><span class="sxs-lookup"><span data-stu-id="e9247-106">Build your business flow based on the data you get from Dynamics 365 (online).</span></span>
* <span data-ttu-id="e9247-107">Pomocí akcí, které se odpověď a pak proveďte výstup k dispozici pro další akce.</span><span class="sxs-lookup"><span data-stu-id="e9247-107">Use actions that get a response and then make the output available for other actions.</span></span> <span data-ttu-id="e9247-108">Při aktualizaci položky v Dynamics 365 (online), můžete odeslat e-mailu pomocí Office 365.</span><span class="sxs-lookup"><span data-stu-id="e9247-108">For example, when an item is updated in Dynamics 365 (online), you can send an email using Office 365.</span></span>

<span data-ttu-id="e9247-109">Toto téma ukazuje, jak vytvořit aplikaci logiky, která vytvoří úlohu v Dynamics 365 vždy, když nové zájemce je vytvořen v Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="e9247-109">This topic shows you how to create a logic app that creates a task in Dynamics 365 whenever a new lead is created in Dynamics 365.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9247-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e9247-110">Prerequisites</span></span>
* <span data-ttu-id="e9247-111">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="e9247-111">An Azure account.</span></span>
* <span data-ttu-id="e9247-112">Účet Dynamics 365 (online).</span><span class="sxs-lookup"><span data-stu-id="e9247-112">A Dynamics 365 (online) account.</span></span>

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a><span data-ttu-id="e9247-113">Vytvoření úlohy při vytváření nového zájemce v Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="e9247-113">Create a task when a new lead is created in Dynamics 365</span></span>

1.  <span data-ttu-id="e9247-114">[Přihlaste se k Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e9247-114">[Sign in to Azure](https://portal.azure.com).</span></span>

2.  <span data-ttu-id="e9247-115">Do pole vyhledávání systému Azure, zadejte `Logic apps`, a stiskněte klávesu ENTER.</span><span class="sxs-lookup"><span data-stu-id="e9247-115">In the Azure search box, type `Logic apps`, and press ENTER.</span></span>

      ![Najít Logic Apps](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  <span data-ttu-id="e9247-117">V části **Logic apps**, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e9247-117">Under **Logic apps**, click **Add**.</span></span>

      ![Přidat LogicApp](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  <span data-ttu-id="e9247-119">Chcete-li vytvořit aplikaci logiky, proveďte **název**, **předplatné**, **skupiny prostředků**, a **umístění** pole a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e9247-119">To create the logic app, complete the **Name**, **Subscription**, **Resource Group**, and **Location** fields, and then click **Create**.</span></span>

5.  <span data-ttu-id="e9247-120">Vyberte nové aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="e9247-120">Select the new logic app.</span></span> <span data-ttu-id="e9247-121">Když se zobrazí **nasazení bylo úspěšné** oznámení, klikněte na tlačítko **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="e9247-121">When you receive the **Deployment Succeeded** notification, click **Refresh**.</span></span>

6.  <span data-ttu-id="e9247-122">V části **nástroje pro vývoj**, klikněte na tlačítko **návrhář aplikace na základě logiky**.</span><span class="sxs-lookup"><span data-stu-id="e9247-122">Under **Development Tools**, click **Logic App Designer**.</span></span> <span data-ttu-id="e9247-123">V seznamu šablon klikněte na **prázdné aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="e9247-123">In the template list, click **Blank Logic App**.</span></span>

7.  <span data-ttu-id="e9247-124">Do vyhledávacího pole zadejte `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="e9247-124">In the search box, type `Dynamics 365`.</span></span> <span data-ttu-id="e9247-125">Vyberte ze seznamu aktivační události Dynamics 365 **Dynamics 365 – když se vytvoří záznam**.</span><span class="sxs-lookup"><span data-stu-id="e9247-125">From the Dynamics 365 triggers list, select **Dynamics 365 – When a record is created**.</span></span>

8.  <span data-ttu-id="e9247-126">Pokud se zobrazí výzva k přihlášení k aplikaci Dynamics 365, udělejte to teď.</span><span class="sxs-lookup"><span data-stu-id="e9247-126">If you are prompted to sign in to Dynamics 365, do so now.</span></span>

9.  <span data-ttu-id="e9247-127">V podrobnostech aktivační události zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="e9247-127">In the trigger details, enter the following information:</span></span>

  * <span data-ttu-id="e9247-128">**Název organizace**.</span><span class="sxs-lookup"><span data-stu-id="e9247-128">**Organization Name**.</span></span> <span data-ttu-id="e9247-129">Vyberte, které chcete aplikaci logiky pro naslouchání na instanci Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="e9247-129">Select the Dynamics 365 instance that you want the logic app to listen to.</span></span>

  * <span data-ttu-id="e9247-130">**Název entity**.</span><span class="sxs-lookup"><span data-stu-id="e9247-130">**Entity Name**.</span></span> <span data-ttu-id="e9247-131">Vyberte typ entity, která chcete pro naslouchání na.</span><span class="sxs-lookup"><span data-stu-id="e9247-131">Select the entity that you want to listen to.</span></span> <span data-ttu-id="e9247-132">Tato událost funguje jako aktivační událost a spusťte aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="e9247-132">This event acts as a trigger to start the logic app.</span></span> 
  <span data-ttu-id="e9247-133">V tomto návodu **vede** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="e9247-133">In this walkthrough, **Leads** is selected.</span></span>

  * <span data-ttu-id="e9247-134">**Jak často chcete zkontrolovat položky?**</span><span class="sxs-lookup"><span data-stu-id="e9247-134">**How often do you want to check for items?**</span></span> <span data-ttu-id="e9247-135">Tyto hodnoty nastaveny, jak často aplikaci logiky kontrolovat aktualizace související se aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="e9247-135">These values set how often the logic app checks for updates related to the trigger.</span></span> <span data-ttu-id="e9247-136">Výchozí nastavení je ke kontrole aktualizací každé tři minuty.</span><span class="sxs-lookup"><span data-stu-id="e9247-136">The default setting is to check for updates every three minutes.</span></span>

    * <span data-ttu-id="e9247-137">**Frekvence**.</span><span class="sxs-lookup"><span data-stu-id="e9247-137">**Frequency**.</span></span> <span data-ttu-id="e9247-138">Vyberte sekund, minut, hodin nebo dnů.</span><span class="sxs-lookup"><span data-stu-id="e9247-138">Select seconds, minutes, hours, or days.</span></span>

    * <span data-ttu-id="e9247-139">**Interval**.</span><span class="sxs-lookup"><span data-stu-id="e9247-139">**Interval**.</span></span> <span data-ttu-id="e9247-140">Zadejte počet sekund, minut, hodin nebo dnů, které chcete předat před další kontroly.</span><span class="sxs-lookup"><span data-stu-id="e9247-140">Enter the number of seconds, minutes, hours, or days that you want to pass before the next check.</span></span>

      ![Podrobnosti o logiku aktivační událostí aplikace](./media/connectors-create-api-crmonline/trigger-details.png)

10. <span data-ttu-id="e9247-142">Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="e9247-142">Click **New step**, and then click **Add an action**.</span></span>

11. <span data-ttu-id="e9247-143">Do vyhledávacího pole zadejte `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="e9247-143">In the search box, type `Dynamics 365`.</span></span> <span data-ttu-id="e9247-144">Vyberte ze seznamu akce **Dynamics 365 – vytvořit nový záznam**.</span><span class="sxs-lookup"><span data-stu-id="e9247-144">From the actions list, select **Dynamics 365 – Create a new record**.</span></span>

12. <span data-ttu-id="e9247-145">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="e9247-145">Enter the following information:</span></span>

    * <span data-ttu-id="e9247-146">**Název organizace**.</span><span class="sxs-lookup"><span data-stu-id="e9247-146">**Organization Name**.</span></span> <span data-ttu-id="e9247-147">Vyberte instanci Dynamics 365 místo postup pro vytvoření záznamu.</span><span class="sxs-lookup"><span data-stu-id="e9247-147">Select the Dynamics 365 instance where you want the flow to create the record.</span></span> 
    <span data-ttu-id="e9247-148">Všimněte si, že nemusí být na stejnou instanci, kde je aktivována událost z této instance.</span><span class="sxs-lookup"><span data-stu-id="e9247-148">Notice that this instance doesn’t have to be the same instance where the event is triggered from.</span></span>

    * <span data-ttu-id="e9247-149">**Název entity**.</span><span class="sxs-lookup"><span data-stu-id="e9247-149">**Entity Name**.</span></span> <span data-ttu-id="e9247-150">Vyberte entity, který chcete vytvořit záznam, když je aktivována událost.</span><span class="sxs-lookup"><span data-stu-id="e9247-150">Select the entity that you want to create a record when the event is triggered.</span></span> 
    <span data-ttu-id="e9247-151">V tomto návodu **úlohy** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="e9247-151">In this walkthrough, **Tasks** is selected.</span></span>

13. <span data-ttu-id="e9247-152">Kliknutím na tlačítko ve **subjektu** pole, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="e9247-152">Click in the **Subject** box that appears.</span></span> <span data-ttu-id="e9247-153">Dynamické obsahu seznamu, které se zobrazí můžete si vybrat jednu z těchto polí:</span><span class="sxs-lookup"><span data-stu-id="e9247-153">From the dynamic content list that appears, you can select either of these fields:</span></span>

    * <span data-ttu-id="e9247-154">**Příjmení**.</span><span class="sxs-lookup"><span data-stu-id="e9247-154">**Last Name**.</span></span> <span data-ttu-id="e9247-155">Příjmení zájemce výběru v tomto poli vloží do pole Předmět pro úlohu, když je vytvořen záznam úloh.</span><span class="sxs-lookup"><span data-stu-id="e9247-155">Selecting this field inserts the last name for the lead into the Subject field for the task, when the task record is created.</span></span>
    * <span data-ttu-id="e9247-156">**Téma**.</span><span class="sxs-lookup"><span data-stu-id="e9247-156">**Topic**.</span></span> <span data-ttu-id="e9247-157">Výběrem toto pole vloží pole tématu zájemce do poli pro předmět pro úlohy, když je vytvořen záznam úloh.</span><span class="sxs-lookup"><span data-stu-id="e9247-157">Selecting this field inserts the Topic field for the lead into the Subject field for the task, when the task record is created.</span></span> 
    <span data-ttu-id="e9247-158">Klikněte na tlačítko **tématu** to přidat **subjektu** pole.</span><span class="sxs-lookup"><span data-stu-id="e9247-158">Click **Topic** to add that to the **Subject** box.</span></span>

      ![Logiku aplikace vytvoří nové podrobnosti záznamu](./media/connectors-create-api-crmonline/create-record-details.png)

14. <span data-ttu-id="e9247-160">Na panelu nástrojů návrháře aplikace logiky, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e9247-160">On the Logic App Designer toolbar, click **Save**.</span></span>

    ![Návrhář aplikace na základě nástrojů Logika uložit](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. <span data-ttu-id="e9247-162">Chcete-li spustit aplikaci logiky, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="e9247-162">To start the Logic App, click **Run**.</span></span>

    ![Návrhář aplikace na základě nástrojů Logika uložit](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. <span data-ttu-id="e9247-164">Teď vytvořte záznam realizace v 365 Dynamics pro prodej a v tématu vaše toku v akci!</span><span class="sxs-lookup"><span data-stu-id="e9247-164">Now create a lead record in Dynamics 365 for Sales and see your flow in action!</span></span>

## <a name="set-advanced-options-for-a-logic-app-step"></a><span data-ttu-id="e9247-165">Upřesnit možnosti pro krok aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e9247-165">Set advanced options for a logic app step</span></span>

<span data-ttu-id="e9247-166">Určete, jak se k filtrování dat v kroku aplikaci logiky, klikněte na tlačítko **zobrazit rozšířené možnosti** v tomto kroku přidáte filtru nebo pořadí dotazem.</span><span class="sxs-lookup"><span data-stu-id="e9247-166">To specify how to filter data in a logic app step, click **Show advanced options** in that step, then add a filter or order by query.</span></span>

<span data-ttu-id="e9247-167">Například můžete použít dotaz filter získat pouze aktivní účty a pořadí podle názvu účtu.</span><span class="sxs-lookup"><span data-stu-id="e9247-167">For example, you can use a filter query to get only active accounts and order by the account name.</span></span> <span data-ttu-id="e9247-168">K provedení této úlohy, zadejte dotaz filtru OData `statuscode eq 1`a vyberte **název účtu** ze seznamu dynamického obsahu.</span><span class="sxs-lookup"><span data-stu-id="e9247-168">To perform this task, enter the OData filter query `statuscode eq 1`, and select **Account Name** from the dynamic content list.</span></span> <span data-ttu-id="e9247-169">Další informace: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) a [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span><span class="sxs-lookup"><span data-stu-id="e9247-169">More information: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) and [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span></span>

![Aplikace logiky rozšířené možnosti](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a><span data-ttu-id="e9247-171">Osvědčené postupy při použití rozšířené možnosti</span><span class="sxs-lookup"><span data-stu-id="e9247-171">Best practices when using advanced options</span></span>

<span data-ttu-id="e9247-172">Při přidejte hodnotu do pole, musí odpovídat typ pole, ať už zadejte hodnotu, nebo vyberte hodnotu ze seznamu dynamického obsahu.</span><span class="sxs-lookup"><span data-stu-id="e9247-172">When you add a value to a field, you must match the field type whether you type a value or select a value from the dynamic content list.</span></span>

<span data-ttu-id="e9247-173">Typ pole</span><span class="sxs-lookup"><span data-stu-id="e9247-173">Field type</span></span>  |<span data-ttu-id="e9247-174">Způsob použití</span><span class="sxs-lookup"><span data-stu-id="e9247-174">How to use</span></span>  |<span data-ttu-id="e9247-175">Kde najít</span><span class="sxs-lookup"><span data-stu-id="e9247-175">Where to find</span></span>  |<span data-ttu-id="e9247-176">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e9247-176">Name</span></span>  |<span data-ttu-id="e9247-177">Datový typ</span><span class="sxs-lookup"><span data-stu-id="e9247-177">Data type</span></span>  
---------|---------|---------|---------|---------
<span data-ttu-id="e9247-178">Textová pole</span><span class="sxs-lookup"><span data-stu-id="e9247-178">Text fields</span></span>|<span data-ttu-id="e9247-179">Textová pole vyžadují jeden řádek textu nebo dynamický obsah, který je pole typu text.</span><span class="sxs-lookup"><span data-stu-id="e9247-179">Text fields require a single line of text or dynamic content that is a text type field.</span></span> <span data-ttu-id="e9247-180">Mezi příklady patří pole kategorie a dílčí kategorie.</span><span class="sxs-lookup"><span data-stu-id="e9247-180">Examples include the Category and Sub-Category fields.</span></span>|<span data-ttu-id="e9247-181">Nastavení > Přizpůsobení > Přizpůsobení systému > entity > úlohy > pole</span><span class="sxs-lookup"><span data-stu-id="e9247-181">Settings > Customizations > Customize the System > Entities > Task > Fields</span></span> |<span data-ttu-id="e9247-182">category</span><span class="sxs-lookup"><span data-stu-id="e9247-182">category</span></span> |<span data-ttu-id="e9247-183">Jeden řádek textu</span><span class="sxs-lookup"><span data-stu-id="e9247-183">Single Line of Text</span></span>        
<span data-ttu-id="e9247-184">Pole celé číslo</span><span class="sxs-lookup"><span data-stu-id="e9247-184">Integer fields</span></span> | <span data-ttu-id="e9247-185">Některá pole vyžadují celé číslo nebo dynamický obsah, který je pole typu integer.</span><span class="sxs-lookup"><span data-stu-id="e9247-185">Some fields require integer or dynamic content that is an integer type field.</span></span> <span data-ttu-id="e9247-186">Mezi příklady patří dokončeno % a doby trvání.</span><span class="sxs-lookup"><span data-stu-id="e9247-186">Examples include Percent Complete and Duration.</span></span> |<span data-ttu-id="e9247-187">Nastavení > Přizpůsobení > Přizpůsobení systému > entity > úlohy > pole</span><span class="sxs-lookup"><span data-stu-id="e9247-187">Settings > Customizations > Customize the System > Entities > Task > Fields</span></span> |<span data-ttu-id="e9247-188">Procento dokončení</span><span class="sxs-lookup"><span data-stu-id="e9247-188">percentcomplete</span></span> |<span data-ttu-id="e9247-189">Celé číslo</span><span class="sxs-lookup"><span data-stu-id="e9247-189">Whole Number</span></span>         
<span data-ttu-id="e9247-190">Datum pole</span><span class="sxs-lookup"><span data-stu-id="e9247-190">Date fields</span></span> | <span data-ttu-id="e9247-191">Některá pole vyžadují datum ve formátu mm/dd/rrrr nebo dynamický obsah, který je pole typu datum.</span><span class="sxs-lookup"><span data-stu-id="e9247-191">Some fields require a date entered in mm/dd/yyyy format or dynamic content that is a date type field.</span></span> <span data-ttu-id="e9247-192">Příklady jsou vytvořené na počáteční datum, skutečné zahájení poslední na dobu uchování, skutečný konec a datum splatnosti.</span><span class="sxs-lookup"><span data-stu-id="e9247-192">Examples include Created On, Start Date, Actual Start, Last on Hold Time, Actual End, and Due Date.</span></span> | <span data-ttu-id="e9247-193">Nastavení > Přizpůsobení > Přizpůsobení systému > entity > úlohy > pole</span><span class="sxs-lookup"><span data-stu-id="e9247-193">Settings > Customizations > Customize the System > Entities > Task > Fields</span></span> |<span data-ttu-id="e9247-194">createdon</span><span class="sxs-lookup"><span data-stu-id="e9247-194">createdon</span></span> |<span data-ttu-id="e9247-195">Datum a čas</span><span class="sxs-lookup"><span data-stu-id="e9247-195">Date and Time</span></span>
<span data-ttu-id="e9247-196">Zadejte pole, které vyžadují záznamu ID i vyhledávání</span><span class="sxs-lookup"><span data-stu-id="e9247-196">Fields that require both a record ID and lookup type</span></span> |<span data-ttu-id="e9247-197">Některá pole, které odkazují na jiný záznam entity vyžadují ID záznamu a typ vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="e9247-197">Some fields that reference another entity record require both the record ID and the lookup type.</span></span> |<span data-ttu-id="e9247-198">Nastavení > Přizpůsobení > Přizpůsobení systému > entity > účet > pole</span><span class="sxs-lookup"><span data-stu-id="e9247-198">Settings > Customizations > Customize the System > Entities > Account > Fields</span></span>  | <span data-ttu-id="e9247-199">ID účtu</span><span class="sxs-lookup"><span data-stu-id="e9247-199">accountid</span></span>  | <span data-ttu-id="e9247-200">Primární klíč</span><span class="sxs-lookup"><span data-stu-id="e9247-200">Primary Key</span></span>

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a><span data-ttu-id="e9247-201">Zadejte další příklady pole, které vyžadují ID záznamu a vyhledávání</span><span class="sxs-lookup"><span data-stu-id="e9247-201">More examples of fields that require both a record ID and lookup type</span></span>
<span data-ttu-id="e9247-202">Rozšíření v předchozí tabulce, zde jsou další příklady pole, která není pracovat s hodnotami vybraný ze seznamu dynamického obsahu.</span><span class="sxs-lookup"><span data-stu-id="e9247-202">Expanding on the previous table, here are more examples of fields that don't work with values selected from the dynamic content list.</span></span> <span data-ttu-id="e9247-203">Místo toho tato pole vyžadují obě záznamu ID a vyhledávací typ zadali do pole v PowerApps.</span><span class="sxs-lookup"><span data-stu-id="e9247-203">Instead, these fields require both a record ID and lookup type entered into the fields in PowerApps.</span></span>  
* <span data-ttu-id="e9247-204">Vlastník a typ vlastníka.</span><span class="sxs-lookup"><span data-stu-id="e9247-204">Owner and Owner Type.</span></span> <span data-ttu-id="e9247-205">V poli vlastník musí být platné ID záznamu uživateli nebo týmu</span><span class="sxs-lookup"><span data-stu-id="e9247-205">The Owner field must be a valid user or team record ID.</span></span> <span data-ttu-id="e9247-206">Typ vlastník musí být buď **systemusers** nebo **týmy**.</span><span class="sxs-lookup"><span data-stu-id="e9247-206">The Owner Type must be either **systemusers** or **teams**.</span></span>
* <span data-ttu-id="e9247-207">Zákazníka a typ odběratele.</span><span class="sxs-lookup"><span data-stu-id="e9247-207">Customer and Customer Type.</span></span> <span data-ttu-id="e9247-208">Pole musí být platný účet nebo se obraťte na ID záznamu.</span><span class="sxs-lookup"><span data-stu-id="e9247-208">The Customer field must be a valid account or contact record ID.</span></span> <span data-ttu-id="e9247-209">Typ vlastník musí být buď **účty** nebo **kontakty**.</span><span class="sxs-lookup"><span data-stu-id="e9247-209">The Owner Type must be either **accounts** or **contacts**.</span></span>
* <span data-ttu-id="e9247-210">A typu.</span><span class="sxs-lookup"><span data-stu-id="e9247-210">Regarding and Regarding Type.</span></span> <span data-ttu-id="e9247-211">Pole týká musí být platný záznam ID, jako je například účet nebo se obraťte na ID záznamu.</span><span class="sxs-lookup"><span data-stu-id="e9247-211">The Regarding field must be a valid record ID, such as an account or contact record ID.</span></span> <span data-ttu-id="e9247-212">Typ ohledně musí být typ vyhledávání na záznam, jako například **účty** nebo **kontakty**.</span><span class="sxs-lookup"><span data-stu-id="e9247-212">The Regarding Type must be the lookup type for the record, such as **accounts** or **contacts**.</span></span>

<span data-ttu-id="e9247-213">Následující příklad akce vytvoření úloh přidá záznam klienta, která odpovídá na ID záznamu přidáním do pole týkající úlohy.</span><span class="sxs-lookup"><span data-stu-id="e9247-213">The following task creation action example adds an account record that corresponds to the record ID adding it to the regarding field of the task.</span></span>

![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid-type-account.png)

<span data-ttu-id="e9247-215">Tento příklad také přiřadí úlohu pro konkrétního uživatele na základě ID uživatele záznamu.</span><span class="sxs-lookup"><span data-stu-id="e9247-215">This example also assigns the task to a specific user based on the user's record ID.</span></span>

![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid-type-user.png)

<span data-ttu-id="e9247-217">ID záznamu najdete v následující části: *najít ID záznamu*</span><span class="sxs-lookup"><span data-stu-id="e9247-217">To find a record's ID, see the following section: *Find the record ID*</span></span>

## <a name="find-the-record-id"></a><span data-ttu-id="e9247-218">Najít ID záznamu</span><span class="sxs-lookup"><span data-stu-id="e9247-218">Find the record ID</span></span>

1. <span data-ttu-id="e9247-219">Otevřete záznam, jako je například záznam klienta.</span><span class="sxs-lookup"><span data-stu-id="e9247-219">Open a record, such as an account record.</span></span>

2. <span data-ttu-id="e9247-220">Na panelu nástrojů Akce klikněte na tlačítko **Pop Out** ![záznam zobrazovat](./media/connectors-create-api-crmonline/popout-record.png).</span><span class="sxs-lookup"><span data-stu-id="e9247-220">On the actions toolbar, click **Pop Out** ![popout record](./media/connectors-create-api-crmonline/popout-record.png).</span></span>
<span data-ttu-id="e9247-221">Případně, na panelu nástrojů Akce, pokud chcete zkopírovat úplnou adresu URL do výchozí program e-mailu, kliknutím na tlačítko **odkazu A e-MAILU**.</span><span class="sxs-lookup"><span data-stu-id="e9247-221">Alternatively, on the actions toolbar, to copy the full URL into your default email program, click **EMAIL A LINK**.</span></span>

   <span data-ttu-id="e9247-222">ID záznamu se zobrazí mezi % 7b a %7 d kódování znaků adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e9247-222">The record ID is displayed in between the %7b and %7d encoding characters of the URL.</span></span>

   ![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a><span data-ttu-id="e9247-224">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="e9247-224">Troubleshooting</span></span>
<span data-ttu-id="e9247-225">Chcete-li vyřešit vadný krok v aplikaci logiky, zobrazte podrobnosti o stavu události.</span><span class="sxs-lookup"><span data-stu-id="e9247-225">To troubleshoot a failed step in a logic app, view the status details of the event.</span></span>

1. <span data-ttu-id="e9247-226">V části **Logic Apps**, vyberte svou aplikaci logiky a pak klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="e9247-226">Under **Logic Apps**, select your logic app, and then click **Overview**.</span></span> 

   <span data-ttu-id="e9247-227">V oblasti souhrnu se zobrazí a poskytuje stav spuštění pro aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="e9247-227">The Summary area is shown and provides the run status for the logic app.</span></span> 

   ![Stav spuštění aplikace logiky](./media/connectors-create-api-crmonline/tshoot1.png)

2. <span data-ttu-id="e9247-229">Chcete-li zobrazit další informace o všech došlo k chybě spuštění, klikněte na možnost neúspěšné události.</span><span class="sxs-lookup"><span data-stu-id="e9247-229">To view more information about any failed runs, click the failed event.</span></span> <span data-ttu-id="e9247-230">Chcete-li rozšířit vadný krok, klikněte na tento krok.</span><span class="sxs-lookup"><span data-stu-id="e9247-230">To expand a failed step, click that step.</span></span>

   ![Rozbalte vadný krok](./media/connectors-create-api-crmonline/tshoot2.png)

   <span data-ttu-id="e9247-232">Podrobnosti krok zobrazí a mohou pomoci při řešení příčiny selhání.</span><span class="sxs-lookup"><span data-stu-id="e9247-232">The step details appear and can help troubleshoot the cause of the failure.</span></span>

   ![Krok podrobnosti neúspěšné](./media/connectors-create-api-crmonline/tshoot3.png)

<span data-ttu-id="e9247-234">Další informace o odstraňování potíží s aplikací logiky najdete v tématu [diagnostikování selhání aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="e9247-234">For more information about troubleshooting logic apps, see [Diagnosing logic app failures](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="e9247-235">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="e9247-235">Connector-specific details</span></span>

<span data-ttu-id="e9247-236">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/crm/).</span><span class="sxs-lookup"><span data-stu-id="e9247-236">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/crm/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e9247-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9247-237">Next steps</span></span>
<span data-ttu-id="e9247-238">Prozkoumejte dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="e9247-238">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
