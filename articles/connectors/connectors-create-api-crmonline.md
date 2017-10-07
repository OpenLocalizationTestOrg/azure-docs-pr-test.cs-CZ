---
title: aaaConnect tooDynamics 365 (online) z Azure Logic Apps | Microsoft Docs
description: "Vytvořit logiku aplikace pracovní postupy, které spravovat Dynamics 365 entity (online) prostřednictvím hello rozhraní API poskytované hello Dynamics 365 konektoru"
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
ms.openlocfilehash: 183d7a6b8e5d2c0eecc70da0da3806e06c382df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toodynamics-365-from-logic-app-workflows"></a><span data-ttu-id="c766e-103">Připojit tooDynamics 365 z pracovních aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="c766e-103">Connect tooDynamics 365 from logic app workflows</span></span>

<span data-ttu-id="c766e-104">S Logic Apps můžete připojit tooDynamics 365 (online) a vytvořit toky užitečné firmy, které vytvořit záznamy, aktualizovat položky nebo vrátí seznam záznamů.</span><span class="sxs-lookup"><span data-stu-id="c766e-104">With Logic Apps, you can connect tooDynamics 365 (online) and create useful business flows that create records, update items, or return a list of records.</span></span> <span data-ttu-id="c766e-105">Konektor hello Dynamics 365 můžete:</span><span class="sxs-lookup"><span data-stu-id="c766e-105">With hello Dynamics 365 connector, you can:</span></span>

* <span data-ttu-id="c766e-106">Sestavení vaší firmy toku na základě hello dat, které můžete získat z Dynamics 365 (online).</span><span class="sxs-lookup"><span data-stu-id="c766e-106">Build your business flow based on hello data you get from Dynamics 365 (online).</span></span>
* <span data-ttu-id="c766e-107">Pomocí akcí, které se odpověď a pak proveďte výstup hello k dispozici pro další akce.</span><span class="sxs-lookup"><span data-stu-id="c766e-107">Use actions that get a response and then make hello output available for other actions.</span></span> <span data-ttu-id="c766e-108">Při aktualizaci položky v Dynamics 365 (online), můžete odeslat e-mailu pomocí Office 365.</span><span class="sxs-lookup"><span data-stu-id="c766e-108">For example, when an item is updated in Dynamics 365 (online), you can send an email using Office 365.</span></span>

<span data-ttu-id="c766e-109">Toto téma ukazuje, jak toocreate aplikace logiky, vytvoří úlohu v Dynamics 365 vždy, když se vytvoří nové zájemce v Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="c766e-109">This topic shows you how toocreate a logic app that creates a task in Dynamics 365 whenever a new lead is created in Dynamics 365.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c766e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c766e-110">Prerequisites</span></span>
* <span data-ttu-id="c766e-111">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c766e-111">An Azure account.</span></span>
* <span data-ttu-id="c766e-112">Účet Dynamics 365 (online).</span><span class="sxs-lookup"><span data-stu-id="c766e-112">A Dynamics 365 (online) account.</span></span>

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a><span data-ttu-id="c766e-113">Vytvoření úlohy při vytváření nového zájemce v Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="c766e-113">Create a task when a new lead is created in Dynamics 365</span></span>

1.  <span data-ttu-id="c766e-114">[Přihlaste se tooAzure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c766e-114">[Sign in tooAzure](https://portal.azure.com).</span></span>

2.  <span data-ttu-id="c766e-115">Hello Azure vyhledávacího pole zadejte `Logic apps`, a stiskněte klávesu ENTER.</span><span class="sxs-lookup"><span data-stu-id="c766e-115">In hello Azure search box, type `Logic apps`, and press ENTER.</span></span>

      ![Najít Logic Apps](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  <span data-ttu-id="c766e-117">V části **Logic apps**, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c766e-117">Under **Logic apps**, click **Add**.</span></span>

      ![Přidat LogicApp](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  <span data-ttu-id="c766e-119">aplikace logiky toocreate hello, dokončení hello **název**, **předplatné**, **skupiny prostředků**, a **umístění** pole a pak klikněte na tlačítko  **Vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="c766e-119">toocreate hello logic app, complete hello **Name**, **Subscription**, **Resource Group**, and **Location** fields, and then click **Create**.</span></span>

5.  <span data-ttu-id="c766e-120">Vyberte nové aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-120">Select hello new logic app.</span></span> <span data-ttu-id="c766e-121">Po přijetí hello **nasazení bylo úspěšné** oznámení, klikněte na tlačítko **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="c766e-121">When you receive hello **Deployment Succeeded** notification, click **Refresh**.</span></span>

6.  <span data-ttu-id="c766e-122">V části **nástroje pro vývoj**, klikněte na tlačítko **návrhář aplikace na základě logiky**.</span><span class="sxs-lookup"><span data-stu-id="c766e-122">Under **Development Tools**, click **Logic App Designer**.</span></span> <span data-ttu-id="c766e-123">V seznamu hello šablony, klikněte na tlačítko **prázdné aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="c766e-123">In hello template list, click **Blank Logic App**.</span></span>

7.  <span data-ttu-id="c766e-124">Hello vyhledávacího pole zadejte `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="c766e-124">In hello search box, type `Dynamics 365`.</span></span> <span data-ttu-id="c766e-125">Z hello Dynamics 365 aktivuje seznamu, vyberte **Dynamics 365 – když se vytvoří záznam**.</span><span class="sxs-lookup"><span data-stu-id="c766e-125">From hello Dynamics 365 triggers list, select **Dynamics 365 – When a record is created**.</span></span>

8.  <span data-ttu-id="c766e-126">Pokud jste výzvami toosign v tooDynamics 365, udělejte to teď.</span><span class="sxs-lookup"><span data-stu-id="c766e-126">If you are prompted toosign in tooDynamics 365, do so now.</span></span>

9.  <span data-ttu-id="c766e-127">V podrobnostech aktivační událost hello zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="c766e-127">In hello trigger details, enter hello following information:</span></span>

  * <span data-ttu-id="c766e-128">**Název organizace**.</span><span class="sxs-lookup"><span data-stu-id="c766e-128">**Organization Name**.</span></span> <span data-ttu-id="c766e-129">Vyberte hello Dynamics 365 instance, která má toolisten aplikace logiky hello k.</span><span class="sxs-lookup"><span data-stu-id="c766e-129">Select hello Dynamics 365 instance that you want hello logic app toolisten to.</span></span>

  * <span data-ttu-id="c766e-130">**Název entity**.</span><span class="sxs-lookup"><span data-stu-id="c766e-130">**Entity Name**.</span></span> <span data-ttu-id="c766e-131">Vyberte hello entita, která chcete toolisten k.</span><span class="sxs-lookup"><span data-stu-id="c766e-131">Select hello entity that you want toolisten to.</span></span> <span data-ttu-id="c766e-132">Tato událost funguje jako aplikace logiky hello toostart aktivační události.</span><span class="sxs-lookup"><span data-stu-id="c766e-132">This event acts as a trigger toostart hello logic app.</span></span> 
  <span data-ttu-id="c766e-133">V tomto návodu **vede** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="c766e-133">In this walkthrough, **Leads** is selected.</span></span>

  * <span data-ttu-id="c766e-134">**Jak často chcete toocheck pro položky?**</span><span class="sxs-lookup"><span data-stu-id="c766e-134">**How often do you want toocheck for items?**</span></span> <span data-ttu-id="c766e-135">Tyto hodnoty nastaveny, jak často hello logiku aplikace zkontroluje aktualizace související toohello aktivační události.</span><span class="sxs-lookup"><span data-stu-id="c766e-135">These values set how often hello logic app checks for updates related toohello trigger.</span></span> <span data-ttu-id="c766e-136">Hello výchozí nastavení je toocheck aktualizací každé tři minuty.</span><span class="sxs-lookup"><span data-stu-id="c766e-136">hello default setting is toocheck for updates every three minutes.</span></span>

    * <span data-ttu-id="c766e-137">**Frekvence**.</span><span class="sxs-lookup"><span data-stu-id="c766e-137">**Frequency**.</span></span> <span data-ttu-id="c766e-138">Vyberte sekund, minut, hodin nebo dnů.</span><span class="sxs-lookup"><span data-stu-id="c766e-138">Select seconds, minutes, hours, or days.</span></span>

    * <span data-ttu-id="c766e-139">**Interval**.</span><span class="sxs-lookup"><span data-stu-id="c766e-139">**Interval**.</span></span> <span data-ttu-id="c766e-140">Zadejte hello počet sekund, minut, hodin nebo dnů, kdy chcete toopass před hello další kontroly.</span><span class="sxs-lookup"><span data-stu-id="c766e-140">Enter hello number of seconds, minutes, hours, or days that you want toopass before hello next check.</span></span>

      ![Podrobnosti o logiku aktivační událostí aplikace](./media/connectors-create-api-crmonline/trigger-details.png)

10. <span data-ttu-id="c766e-142">Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="c766e-142">Click **New step**, and then click **Add an action**.</span></span>

11. <span data-ttu-id="c766e-143">Hello vyhledávacího pole zadejte `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="c766e-143">In hello search box, type `Dynamics 365`.</span></span> <span data-ttu-id="c766e-144">Hello akce seznamu, vyberte **Dynamics 365 – vytvořit nový záznam**.</span><span class="sxs-lookup"><span data-stu-id="c766e-144">From hello actions list, select **Dynamics 365 – Create a new record**.</span></span>

12. <span data-ttu-id="c766e-145">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="c766e-145">Enter hello following information:</span></span>

    * <span data-ttu-id="c766e-146">**Název organizace**.</span><span class="sxs-lookup"><span data-stu-id="c766e-146">**Organization Name**.</span></span> <span data-ttu-id="c766e-147">Vyberte instanci hello Dynamics 365 místo hello toku toocreate hello záznamu.</span><span class="sxs-lookup"><span data-stu-id="c766e-147">Select hello Dynamics 365 instance where you want hello flow toocreate hello record.</span></span> 
    <span data-ttu-id="c766e-148">Všimněte si, že tato instance nemá toobe hello stejné instance, kde je aktivována událost hello z.</span><span class="sxs-lookup"><span data-stu-id="c766e-148">Notice that this instance doesn’t have toobe hello same instance where hello event is triggered from.</span></span>

    * <span data-ttu-id="c766e-149">**Název entity**.</span><span class="sxs-lookup"><span data-stu-id="c766e-149">**Entity Name**.</span></span> <span data-ttu-id="c766e-150">Vyberte, chcete toocreate záznam, když je aktivována událost hello entity hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-150">Select hello entity that you want toocreate a record when hello event is triggered.</span></span> 
    <span data-ttu-id="c766e-151">V tomto návodu **úlohy** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="c766e-151">In this walkthrough, **Tasks** is selected.</span></span>

13. <span data-ttu-id="c766e-152">Klikněte na tlačítko v hello **subjektu** pole, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c766e-152">Click in hello **Subject** box that appears.</span></span> <span data-ttu-id="c766e-153">Z hello dynamického obsahu seznamu, který se zobrazí můžete si vybrat jednu z těchto polí:</span><span class="sxs-lookup"><span data-stu-id="c766e-153">From hello dynamic content list that appears, you can select either of these fields:</span></span>

    * <span data-ttu-id="c766e-154">**Příjmení**.</span><span class="sxs-lookup"><span data-stu-id="c766e-154">**Last Name**.</span></span> <span data-ttu-id="c766e-155">Hello příjmení pro hello realizace výběru v tomto poli vloží do pole Subject hello hello úlohy, při záznamu úloh hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-155">Selecting this field inserts hello last name for hello lead into hello Subject field for hello task, when hello task record is created.</span></span>
    * <span data-ttu-id="c766e-156">**Téma**.</span><span class="sxs-lookup"><span data-stu-id="c766e-156">**Topic**.</span></span> <span data-ttu-id="c766e-157">Pokud vyberete toto pole vloží hello tématu pole pro hello realizace do pole Předmět hello hello úkolu, hello úloh záznam vytvoření.</span><span class="sxs-lookup"><span data-stu-id="c766e-157">Selecting this field inserts hello Topic field for hello lead into hello Subject field for hello task, when hello task record is created.</span></span> 
    <span data-ttu-id="c766e-158">Klikněte na tlačítko **tématu** tooadd této toohello **subjektu** pole.</span><span class="sxs-lookup"><span data-stu-id="c766e-158">Click **Topic** tooadd that toohello **Subject** box.</span></span>

      ![Logiku aplikace vytvoří nové podrobnosti záznamu](./media/connectors-create-api-crmonline/create-record-details.png)

14. <span data-ttu-id="c766e-160">Na panelu nástrojů hello návrhář aplikace logiky, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c766e-160">On hello Logic App Designer toolbar, click **Save**.</span></span>

    ![Návrhář aplikace na základě nástrojů Logika uložit](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. <span data-ttu-id="c766e-162">Klikněte na tlačítko toostart hello aplikace logiky **spustit**.</span><span class="sxs-lookup"><span data-stu-id="c766e-162">toostart hello Logic App, click **Run**.</span></span>

    ![Návrhář aplikace na základě nástrojů Logika uložit](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. <span data-ttu-id="c766e-164">Teď vytvořte záznam realizace v 365 Dynamics pro prodej a v tématu vaše toku v akci!</span><span class="sxs-lookup"><span data-stu-id="c766e-164">Now create a lead record in Dynamics 365 for Sales and see your flow in action!</span></span>

## <a name="set-advanced-options-for-a-logic-app-step"></a><span data-ttu-id="c766e-165">Upřesnit možnosti pro krok aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="c766e-165">Set advanced options for a logic app step</span></span>

<span data-ttu-id="c766e-166">jak toofilter dat v kroku aplikaci logiky, klikněte na tlačítko toospecify **zobrazit rozšířené možnosti** v tomto kroku přidáte filtru nebo pořadí dotazem.</span><span class="sxs-lookup"><span data-stu-id="c766e-166">toospecify how toofilter data in a logic app step, click **Show advanced options** in that step, then add a filter or order by query.</span></span>

<span data-ttu-id="c766e-167">Můžete například použít pouze aktivní účty filtru dotazu tooget a pořadí podle názvu účtu hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-167">For example, you can use a filter query tooget only active accounts and order by hello account name.</span></span> <span data-ttu-id="c766e-168">tooperform to úloh, zadejte dotaz filtru OData hello `statuscode eq 1`a vyberte **název účtu** hello dynamického obsahu seznamu.</span><span class="sxs-lookup"><span data-stu-id="c766e-168">tooperform this task, enter hello OData filter query `statuscode eq 1`, and select **Account Name** from hello dynamic content list.</span></span> <span data-ttu-id="c766e-169">Další informace: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) a [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span><span class="sxs-lookup"><span data-stu-id="c766e-169">More information: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) and [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span></span>

![Aplikace logiky rozšířené možnosti](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a><span data-ttu-id="c766e-171">Osvědčené postupy při použití rozšířené možnosti</span><span class="sxs-lookup"><span data-stu-id="c766e-171">Best practices when using advanced options</span></span>

<span data-ttu-id="c766e-172">Při přidání pole tooa hodnotu, zda zadejte hodnotu, nebo vyberte hodnotu ze seznamu obsahu dynamických hello musí odpovídat typ pole hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-172">When you add a value tooa field, you must match hello field type whether you type a value or select a value from hello dynamic content list.</span></span>

<span data-ttu-id="c766e-173">Typ pole</span><span class="sxs-lookup"><span data-stu-id="c766e-173">Field type</span></span>  |<span data-ttu-id="c766e-174">Jak toouse</span><span class="sxs-lookup"><span data-stu-id="c766e-174">How toouse</span></span>  |<span data-ttu-id="c766e-175">Kde toofind</span><span class="sxs-lookup"><span data-stu-id="c766e-175">Where toofind</span></span>  |<span data-ttu-id="c766e-176">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c766e-176">Name</span></span>  |<span data-ttu-id="c766e-177">Datový typ</span><span class="sxs-lookup"><span data-stu-id="c766e-177">Data type</span></span>  
---------|---------|---------|---------|---------
<span data-ttu-id="c766e-178">Textová pole</span><span class="sxs-lookup"><span data-stu-id="c766e-178">Text fields</span></span>|<span data-ttu-id="c766e-179">Textová pole vyžadují jeden řádek textu nebo dynamický obsah, který je pole typu text.</span><span class="sxs-lookup"><span data-stu-id="c766e-179">Text fields require a single line of text or dynamic content that is a text type field.</span></span> <span data-ttu-id="c766e-180">Mezi příklady patří pole kategorie a dílčí kategorie hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-180">Examples include hello Category and Sub-Category fields.</span></span>|<span data-ttu-id="c766e-181">Nastavení > Přizpůsobení > Přizpůsobit hello System > entity > úlohy > pole</span><span class="sxs-lookup"><span data-stu-id="c766e-181">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="c766e-182">category</span><span class="sxs-lookup"><span data-stu-id="c766e-182">category</span></span> |<span data-ttu-id="c766e-183">Jeden řádek textu</span><span class="sxs-lookup"><span data-stu-id="c766e-183">Single Line of Text</span></span>        
<span data-ttu-id="c766e-184">Pole celé číslo</span><span class="sxs-lookup"><span data-stu-id="c766e-184">Integer fields</span></span> | <span data-ttu-id="c766e-185">Některá pole vyžadují celé číslo nebo dynamický obsah, který je pole typu integer.</span><span class="sxs-lookup"><span data-stu-id="c766e-185">Some fields require integer or dynamic content that is an integer type field.</span></span> <span data-ttu-id="c766e-186">Mezi příklady patří dokončeno % a doby trvání.</span><span class="sxs-lookup"><span data-stu-id="c766e-186">Examples include Percent Complete and Duration.</span></span> |<span data-ttu-id="c766e-187">Nastavení > Přizpůsobení > Přizpůsobit hello System > entity > úlohy > pole</span><span class="sxs-lookup"><span data-stu-id="c766e-187">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="c766e-188">Procento dokončení</span><span class="sxs-lookup"><span data-stu-id="c766e-188">percentcomplete</span></span> |<span data-ttu-id="c766e-189">Celé číslo</span><span class="sxs-lookup"><span data-stu-id="c766e-189">Whole Number</span></span>         
<span data-ttu-id="c766e-190">Datum pole</span><span class="sxs-lookup"><span data-stu-id="c766e-190">Date fields</span></span> | <span data-ttu-id="c766e-191">Některá pole vyžadují datum ve formátu mm/dd/rrrr nebo dynamický obsah, který je pole typu datum.</span><span class="sxs-lookup"><span data-stu-id="c766e-191">Some fields require a date entered in mm/dd/yyyy format or dynamic content that is a date type field.</span></span> <span data-ttu-id="c766e-192">Příklady jsou vytvořené na počáteční datum, skutečné zahájení poslední na dobu uchování, skutečný konec a datum splatnosti.</span><span class="sxs-lookup"><span data-stu-id="c766e-192">Examples include Created On, Start Date, Actual Start, Last on Hold Time, Actual End, and Due Date.</span></span> | <span data-ttu-id="c766e-193">Nastavení > Přizpůsobení > Přizpůsobit hello System > entity > úlohy > pole</span><span class="sxs-lookup"><span data-stu-id="c766e-193">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="c766e-194">createdon</span><span class="sxs-lookup"><span data-stu-id="c766e-194">createdon</span></span> |<span data-ttu-id="c766e-195">Datum a čas</span><span class="sxs-lookup"><span data-stu-id="c766e-195">Date and Time</span></span>
<span data-ttu-id="c766e-196">Zadejte pole, které vyžadují záznamu ID i vyhledávání</span><span class="sxs-lookup"><span data-stu-id="c766e-196">Fields that require both a record ID and lookup type</span></span> |<span data-ttu-id="c766e-197">Některá pole, které odkazují na jiný záznam entity vyžadují ID záznamu hello a typ vyhledávání hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-197">Some fields that reference another entity record require both hello record ID and hello lookup type.</span></span> |<span data-ttu-id="c766e-198">Nastavení > Přizpůsobení > Přizpůsobit hello System > entity > účet > pole</span><span class="sxs-lookup"><span data-stu-id="c766e-198">Settings > Customizations > Customize hello System > Entities > Account > Fields</span></span>  | <span data-ttu-id="c766e-199">ID účtu</span><span class="sxs-lookup"><span data-stu-id="c766e-199">accountid</span></span>  | <span data-ttu-id="c766e-200">Primární klíč</span><span class="sxs-lookup"><span data-stu-id="c766e-200">Primary Key</span></span>

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a><span data-ttu-id="c766e-201">Zadejte další příklady pole, které vyžadují ID záznamu a vyhledávání</span><span class="sxs-lookup"><span data-stu-id="c766e-201">More examples of fields that require both a record ID and lookup type</span></span>
<span data-ttu-id="c766e-202">Rozšíření v předchozí tabulce hello, zde jsou další příklady pole, která není pracovat s hodnotami vybraný ze seznamu obsahu dynamických hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-202">Expanding on hello previous table, here are more examples of fields that don't work with values selected from hello dynamic content list.</span></span> <span data-ttu-id="c766e-203">Místo toho tato pole vyžadují obě ID a vyhledávací typ záznamu zadali do pole hello v PowerApps.</span><span class="sxs-lookup"><span data-stu-id="c766e-203">Instead, these fields require both a record ID and lookup type entered into hello fields in PowerApps.</span></span>  
* <span data-ttu-id="c766e-204">Vlastník a typ vlastníka.</span><span class="sxs-lookup"><span data-stu-id="c766e-204">Owner and Owner Type.</span></span> <span data-ttu-id="c766e-205">v poli Vlastník Hello musí být platné ID záznamu uživateli nebo týmu</span><span class="sxs-lookup"><span data-stu-id="c766e-205">hello Owner field must be a valid user or team record ID.</span></span> <span data-ttu-id="c766e-206">Hello vlastníka typ musí být buď **systemusers** nebo **týmy**.</span><span class="sxs-lookup"><span data-stu-id="c766e-206">hello Owner Type must be either **systemusers** or **teams**.</span></span>
* <span data-ttu-id="c766e-207">Zákazníka a typ odběratele.</span><span class="sxs-lookup"><span data-stu-id="c766e-207">Customer and Customer Type.</span></span> <span data-ttu-id="c766e-208">Hello zákazníka pole musí být platný účet nebo se obraťte na ID záznamu.</span><span class="sxs-lookup"><span data-stu-id="c766e-208">hello Customer field must be a valid account or contact record ID.</span></span> <span data-ttu-id="c766e-209">Hello vlastníka typ musí být buď **účty** nebo **kontakty**.</span><span class="sxs-lookup"><span data-stu-id="c766e-209">hello Owner Type must be either **accounts** or **contacts**.</span></span>
* <span data-ttu-id="c766e-210">A typu.</span><span class="sxs-lookup"><span data-stu-id="c766e-210">Regarding and Regarding Type.</span></span> <span data-ttu-id="c766e-211">Hello týkající se pole musí být platný záznam ID, jako je například účet nebo se obraťte na ID záznamu.</span><span class="sxs-lookup"><span data-stu-id="c766e-211">hello Regarding field must be a valid record ID, such as an account or contact record ID.</span></span> <span data-ttu-id="c766e-212">Hello ohledně typu musí být hello vyhledávání typu hello záznam, jako například **účty** nebo **kontakty**.</span><span class="sxs-lookup"><span data-stu-id="c766e-212">hello Regarding Type must be hello lookup type for hello record, such as **accounts** or **contacts**.</span></span>

<span data-ttu-id="c766e-213">Hello následující příklad akce vytvoření úloh přidá záznam klienta, která odpovídá ID záznamu toohello přidáním toohello týkající se pole hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="c766e-213">hello following task creation action example adds an account record that corresponds toohello record ID adding it toohello regarding field of hello task.</span></span>

![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid-type-account.png)

<span data-ttu-id="c766e-215">Tento příklad také přiřadí hello úloh tooa konkrétního uživatele na základě ID uživatele hello záznamu.</span><span class="sxs-lookup"><span data-stu-id="c766e-215">This example also assigns hello task tooa specific user based on hello user's record ID.</span></span>

![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid-type-user.png)

<span data-ttu-id="c766e-217">toofind záznam je ID, najdete v následující části hello: *najít ID záznamu hello*</span><span class="sxs-lookup"><span data-stu-id="c766e-217">toofind a record's ID, see hello following section: *Find hello record ID*</span></span>

## <a name="find-hello-record-id"></a><span data-ttu-id="c766e-218">Najít ID záznamu hello</span><span class="sxs-lookup"><span data-stu-id="c766e-218">Find hello record ID</span></span>

1. <span data-ttu-id="c766e-219">Otevřete záznam, jako je například záznam klienta.</span><span class="sxs-lookup"><span data-stu-id="c766e-219">Open a record, such as an account record.</span></span>

2. <span data-ttu-id="c766e-220">Na panelu nástrojů hello akce, klikněte na tlačítko **Pop Out** ![záznam zobrazovat](./media/connectors-create-api-crmonline/popout-record.png).</span><span class="sxs-lookup"><span data-stu-id="c766e-220">On hello actions toolbar, click **Pop Out** ![popout record](./media/connectors-create-api-crmonline/popout-record.png).</span></span>
<span data-ttu-id="c766e-221">Případně na panelu nástrojů hello akce toocopy hello úplnou adresu URL do výchozí program e-mailu, klikněte na tlačítko **odkazu A e-MAILU**.</span><span class="sxs-lookup"><span data-stu-id="c766e-221">Alternatively, on hello actions toolbar, toocopy hello full URL into your default email program, click **EMAIL A LINK**.</span></span>

   <span data-ttu-id="c766e-222">ID záznamu Hello se zobrazí mezi hello % 7b a %7 d kódování znaků z adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-222">hello record ID is displayed in between hello %7b and %7d encoding characters of hello URL.</span></span>

   ![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a><span data-ttu-id="c766e-224">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="c766e-224">Troubleshooting</span></span>
<span data-ttu-id="c766e-225">tootroubleshoot vadný krok v aplikaci logiky, zobrazení Podrobnosti o stavu hello hello události.</span><span class="sxs-lookup"><span data-stu-id="c766e-225">tootroubleshoot a failed step in a logic app, view hello status details of hello event.</span></span>

1. <span data-ttu-id="c766e-226">V části **Logic Apps**, vyberte svou aplikaci logiky a pak klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="c766e-226">Under **Logic Apps**, select your logic app, and then click **Overview**.</span></span> 

   <span data-ttu-id="c766e-227">Hello v oblasti souhrnu se zobrazí a poskytuje stav hello spustit pro aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-227">hello Summary area is shown and provides hello run status for hello logic app.</span></span> 

   ![Stav spuštění aplikace logiky](./media/connectors-create-api-crmonline/tshoot1.png)

2. <span data-ttu-id="c766e-229">tooview Další informace o všechny neúspěšné spuštění, klikněte na tlačítko hello se nezdařilo událostí.</span><span class="sxs-lookup"><span data-stu-id="c766e-229">tooview more information about any failed runs, click hello failed event.</span></span> <span data-ttu-id="c766e-230">Klikněte na tlačítko tooexpand vadný krok tohoto kroku.</span><span class="sxs-lookup"><span data-stu-id="c766e-230">tooexpand a failed step, click that step.</span></span>

   ![Rozbalte vadný krok](./media/connectors-create-api-crmonline/tshoot2.png)

   <span data-ttu-id="c766e-232">Podrobnosti krok Hello zobrazí a mohou pomoci při odstraňování hello příčinu selhání hello.</span><span class="sxs-lookup"><span data-stu-id="c766e-232">hello step details appear and can help troubleshoot hello cause of hello failure.</span></span>

   ![Krok podrobnosti neúspěšné](./media/connectors-create-api-crmonline/tshoot3.png)

<span data-ttu-id="c766e-234">Další informace o odstraňování potíží s aplikací logiky najdete v tématu [diagnostikování selhání aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="c766e-234">For more information about troubleshooting logic apps, see [Diagnosing logic app failures](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="c766e-235">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="c766e-235">Connector-specific details</span></span>

<span data-ttu-id="c766e-236">Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/crm/).</span><span class="sxs-lookup"><span data-stu-id="c766e-236">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/crm/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c766e-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c766e-237">Next steps</span></span>
<span data-ttu-id="c766e-238">Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="c766e-238">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
