---
title: "Sledování změn virtuálního počítače - & mřížky událostí Azure Logic Apps | Microsoft Docs"
description: "Kontrolovat změny konfigurace ve virtuálních počítačích (VM) s použitím mřížky událostí Azure a Logic Apps"
keywords: "aplikace logiky, událostí mřížky, virtuální počítač VM"
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 08/16/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 4d4c16860dbec10162797a13c8f9f57106abd17f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a><span data-ttu-id="31eba-104">Sledování změn virtuálního počítače se mřížka událostí Azure a Logic Apps</span><span class="sxs-lookup"><span data-stu-id="31eba-104">Monitor virtual machine changes with Azure Event Grid and Logic Apps</span></span>

<span data-ttu-id="31eba-105">Můžete spustit automatickou [aplikace logiky pracovního postupu](../logic-apps/logic-apps-what-are-logic-apps.md) při určité události dojde v Azure prostředků nebo jiných prostředků.</span><span class="sxs-lookup"><span data-stu-id="31eba-105">You can start an automated [logic app workflow](../logic-apps/logic-apps-what-are-logic-apps.md) when specific events happen in Azure resources or third-party resources.</span></span> <span data-ttu-id="31eba-106">Tyto prostředky můžete publikovat tyto události [Azure událostí mřížky](../event-grid/overview.md).</span><span class="sxs-lookup"><span data-stu-id="31eba-106">These resources can publish those events to an [Azure event grid](../event-grid/overview.md).</span></span> <span data-ttu-id="31eba-107">Pak mřížky událostí nabízených oznámení události odběratelům, které mají fronty, webhooků, nebo [služby event hubs](../event-hubs/event-hubs-what-is-event-hubs.md) jako koncové body.</span><span class="sxs-lookup"><span data-stu-id="31eba-107">In turn, the event grid pushes those events to subscribers that have queues, webhooks, or [event hubs](../event-hubs/event-hubs-what-is-event-hubs.md) as endpoints.</span></span> <span data-ttu-id="31eba-108">Jako odběratel můžete před spuštěním automatizované pracovní postupy k provádění úloh – bez psaní jakéhokoli kódu aplikace logiky počkejte tyto události z události mřížky.</span><span class="sxs-lookup"><span data-stu-id="31eba-108">As a subscriber, your logic app can wait for those events from the event grid before running automated workflows to perform tasks - without you writing any code.</span></span>

<span data-ttu-id="31eba-109">Tady jsou například některé události, které vydavatelů můžete odesílat odběratelům prostřednictvím služby Azure událostí mřížky:</span><span class="sxs-lookup"><span data-stu-id="31eba-109">For example, here are some events that publishers can send to subscribers through the Azure Event Grid service:</span></span>

* <span data-ttu-id="31eba-110">Vytvořit, číst, aktualizovat nebo odstranit prostředek.</span><span class="sxs-lookup"><span data-stu-id="31eba-110">Create, read, update, or delete a resource.</span></span> <span data-ttu-id="31eba-111">Například můžete sledovat změny, které může platit poplatky na vaše předplatné Azure a vliv na vašem vyúčtování.</span><span class="sxs-lookup"><span data-stu-id="31eba-111">For example, you can monitor changes that might incur charges on your Azure subscription and affect your bill.</span></span> 
* <span data-ttu-id="31eba-112">Přidat nebo odebrat osoby z předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="31eba-112">Add or remove a person from an Azure subscription.</span></span>
* <span data-ttu-id="31eba-113">Aplikace provede určité akce.</span><span class="sxs-lookup"><span data-stu-id="31eba-113">Your app performs a particular action.</span></span>
* <span data-ttu-id="31eba-114">Zobrazí se nové zprávy ve frontě.</span><span class="sxs-lookup"><span data-stu-id="31eba-114">A new message appears in a queue.</span></span>

<span data-ttu-id="31eba-115">V tomto kurzu vytvoří aplikace logiky, která sleduje změny k virtuálnímu počítači a odešle e-mailů o těchto změnách.</span><span class="sxs-lookup"><span data-stu-id="31eba-115">This tutorial creates a logic app that monitors changes to a virtual machine and sends emails about those changes.</span></span> <span data-ttu-id="31eba-116">Když vytvoříte aplikaci logiky s předplatným události pro prostředek služby Azure, toku událostí z prostředku prostřednictvím mřížce událostí do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-116">When you create a logic app with an event subscription for an Azure resource, events flow from that resource through an event grid to the logic app.</span></span> <span data-ttu-id="31eba-117">Tento kurz vás provede procesem vytváření této logiky aplikace:</span><span class="sxs-lookup"><span data-stu-id="31eba-117">The tutorial walks you through building this logic app:</span></span>

![Přehled – monitorování virtuální počítač s událostí mřížky a logiku aplikace](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

<span data-ttu-id="31eba-119">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="31eba-119">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="31eba-120">Vytvoření aplikace logiky, která sleduje události z mřížky události.</span><span class="sxs-lookup"><span data-stu-id="31eba-120">Create a logic app that monitors events from an event grid.</span></span>
> * <span data-ttu-id="31eba-121">Přidejte podmínku, která konkrétně kontroluje změny virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="31eba-121">Add a condition that specifically checks for virtual machine changes.</span></span>
> * <span data-ttu-id="31eba-122">Odeslání e-mailu, když se změní virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="31eba-122">Send email when your virtual machine changes.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31eba-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="31eba-123">Prerequisites</span></span>

* <span data-ttu-id="31eba-124">E-mailový účet z [všechny e-mailu zprostředkovatele nepodporuje Azure Logic Apps](../connectors/apis-list.md), jako jsou například aplikace Office 365 Outlook, Outlook.com nebo z Gmailu pro odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="31eba-124">An email account from [any email provider supported by Azure Logic Apps](../connectors/apis-list.md), such as Office 365 Outlook, Outlook.com, or Gmail, for sending notifications.</span></span> <span data-ttu-id="31eba-125">Tento kurz používá Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="31eba-125">This tutorial uses Office 365 Outlook.</span></span>

* <span data-ttu-id="31eba-126">A [virtuální počítač](https://azure.microsoft.com/services/virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="31eba-126">A [virtual machine](https://azure.microsoft.com/services/virtual-machines).</span></span> <span data-ttu-id="31eba-127">Pokud jste tak již neučinili, vytvořte virtuální počítač prostřednictvím [vytvořit virtuální počítač kurzu](https://docs.microsoft.com/azure/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="31eba-127">If you haven't already done so, create a virtual machine through a [Create a VM tutorial](https://docs.microsoft.com/azure/virtual-machines/).</span></span> <span data-ttu-id="31eba-128">Chcete-li virtuální počítač publikování událostí, můžete [nemusíte dělat cokoliv jiného](../event-grid/overview.md).</span><span class="sxs-lookup"><span data-stu-id="31eba-128">To make the virtual machine publish events, you [don't need to do anything else](../event-grid/overview.md).</span></span>

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a><span data-ttu-id="31eba-129">Vytvoření aplikace logiky, která sleduje události z mřížky události</span><span class="sxs-lookup"><span data-stu-id="31eba-129">Create a logic app that monitors events from an event grid</span></span>

<span data-ttu-id="31eba-130">Nejprve vytvořte aplikaci logiky a přidejte mřížce aktivační událost, která monitoruje skupinu prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="31eba-130">First, create a logic app and add an Event grid trigger that monitors the resource group for your virtual machine.</span></span> 

1. <span data-ttu-id="31eba-131">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="31eba-131">Sign in to the [Azure portal](https://portal.azure.com).</span></span> 

2. <span data-ttu-id="31eba-132">V levém horním rohu hlavní Azure nabídky vyberte příkaz **nový** > **Enterprise integrace** > **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="31eba-132">From the upper left corner of the main Azure menu, choose **New** > **Enterprise Integration** > **Logic App**.</span></span>

   ![Vytvoření aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. <span data-ttu-id="31eba-134">Vytvoření aplikace logiky s nastavení zadané v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="31eba-134">Create your logic app with the settings specified in the following table:</span></span>

   ![Zadejte podrobnosti o aplikaci logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | <span data-ttu-id="31eba-136">Nastavení</span><span class="sxs-lookup"><span data-stu-id="31eba-136">Setting</span></span> | <span data-ttu-id="31eba-137">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="31eba-137">Suggested value</span></span> | <span data-ttu-id="31eba-138">Popis</span><span class="sxs-lookup"><span data-stu-id="31eba-138">Description</span></span> | 
   | ------- | --------------- | ----------- | 
   | <span data-ttu-id="31eba-139">**Název**</span><span class="sxs-lookup"><span data-stu-id="31eba-139">**Name**</span></span> | <span data-ttu-id="31eba-140">*{logiku--název_aplikace}*</span><span class="sxs-lookup"><span data-stu-id="31eba-140">*{your-logic-app-name}*</span></span> | <span data-ttu-id="31eba-141">Zadejte název jedinečné logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="31eba-141">Provide a unique logic app name.</span></span> | 
   | <span data-ttu-id="31eba-142">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="31eba-142">**Subscription**</span></span> | <span data-ttu-id="31eba-143">*{vaše Azure předplatné}*</span><span class="sxs-lookup"><span data-stu-id="31eba-143">*{your-Azure-subscription}*</span></span> | <span data-ttu-id="31eba-144">V tomto kurzu vyberte stejné předplatné Azure, ke všem službám.</span><span class="sxs-lookup"><span data-stu-id="31eba-144">Select the same Azure subscription for all services in this tutorial.</span></span> | 
   | <span data-ttu-id="31eba-145">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="31eba-145">**Resource group**</span></span> | <span data-ttu-id="31eba-146">*{vaše Azure-resource-group}*</span><span class="sxs-lookup"><span data-stu-id="31eba-146">*{your-Azure-resource-group}*</span></span> | <span data-ttu-id="31eba-147">V tomto kurzu vyberte stejnou skupinu prostředků Azure pro všechny služby.</span><span class="sxs-lookup"><span data-stu-id="31eba-147">Select the same Azure resource group for all services in this tutorial.</span></span> | 
   | <span data-ttu-id="31eba-148">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="31eba-148">**Location**</span></span> | <span data-ttu-id="31eba-149">*{vaše Azure oblast}*</span><span class="sxs-lookup"><span data-stu-id="31eba-149">*{your-Azure-region}*</span></span> | <span data-ttu-id="31eba-150">Vyberte stejnou oblast pro všechny služby v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="31eba-150">Select the same region for all services in this tutorial.</span></span> | 
   | | | 

4. <span data-ttu-id="31eba-151">Až budete připraveni, vyberte **připnout na řídicí panel**a zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="31eba-151">When you're ready, select **Pin to dashboard**, and choose **Create**.</span></span>

   <span data-ttu-id="31eba-152">Nyní jste vytvořili prostředek služby Azure pro svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-152">You've now created an Azure resource for your logic app.</span></span> 
   <span data-ttu-id="31eba-153">Po Azure nasadí aplikaci logiky, Návrhář aplikace logiky ukazuje šablon pro běžné vzory, abyste mohli začít rychleji.</span><span class="sxs-lookup"><span data-stu-id="31eba-153">After Azure deploys your logic app, the Logic Apps Designer shows you templates for common patterns so you can get started faster.</span></span>

   > [!NOTE] 
   > <span data-ttu-id="31eba-154">Když vyberete **připnout na řídicí panel**, aplikace logiky automaticky otevře v návrháři aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-154">When you select **Pin to dashboard**, your logic app automatically opens in Logic Apps Designer.</span></span> <span data-ttu-id="31eba-155">Jinak můžete ručně najít a otevřít aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-155">Otherwise, you can manually find and open your logic app.</span></span>

5. <span data-ttu-id="31eba-156">Teď zvolte šablonu logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="31eba-156">Now choose a logic app template.</span></span> <span data-ttu-id="31eba-157">V části **šablony**, zvolte **prázdné aplikace logiky** , můžete vytvořit svou aplikaci logiky od začátku.</span><span class="sxs-lookup"><span data-stu-id="31eba-157">Under **Templates**, choose **Blank Logic App** so you can build your logic app from scratch.</span></span>

   ![Výběr šablony aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   <span data-ttu-id="31eba-159">Návrhář aplikace logiky nyní ukazuje [ *konektory* ](../connectors/apis-list.md) a [ *aktivační události* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) , můžete použít ke spuštění aplikace logiky a také akce, můžete přidat aktivační událost k provádění úloh po.</span><span class="sxs-lookup"><span data-stu-id="31eba-159">The Logic Apps Designer now shows you [*connectors*](../connectors/apis-list.md) and [*triggers*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) that you can use to start your logic app, and also actions that you can add after a trigger to perform tasks.</span></span> <span data-ttu-id="31eba-160">Aktivační událost je událost, která vytvoří instanci aplikace logiky a spustí pracovní postup aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-160">A trigger is an event that creates a logic app instance and starts your logic app workflow.</span></span> 
   <span data-ttu-id="31eba-161">Aplikace logiky musí aktivační událost jako první položka.</span><span class="sxs-lookup"><span data-stu-id="31eba-161">Your logic app needs a trigger as the first item.</span></span>

6. <span data-ttu-id="31eba-162">Do vyhledávacího pole zadejte "události mřížky" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="31eba-162">In the search box, enter "event grid" as your filter.</span></span> <span data-ttu-id="31eba-163">Vyberte této aktivační události: **mřížky událostí Azure - na události prostředků**</span><span class="sxs-lookup"><span data-stu-id="31eba-163">Select this trigger: **Azure Event Grid - On a resource event**</span></span>

   ![Vyberte této aktivační události: "Azure událostí mřížky - na události prostředků"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. <span data-ttu-id="31eba-165">Po zobrazení výzvy, přihlaste se k mřížce událostí Azure pomocí svých přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="31eba-165">When prompted, sign in to Azure Event Grid with your Azure credentials.</span></span>

   ![Přihlaste se pomocí přihlašovacích údajů Azure](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > <span data-ttu-id="31eba-167">Pokud jste přihlášení pomocí osobního účtu Microsoft, jako například @outlook.com nebo @hotmail.com, aktivační události mřížky, se nemusí zobrazit správně.</span><span class="sxs-lookup"><span data-stu-id="31eba-167">If you're signed in with a personal Microsoft account, such as @outlook.com or @hotmail.com, the Event Grid trigger might not appear correctly.</span></span> <span data-ttu-id="31eba-168">Jako alternativní řešení, zvolte [připojit pomocí objektu služby](/azure-resource-manager/resource-group-create-service-principal-portal.md), nebo jako člen skupiny Azure Active Directory, který je spojen s předplatným Azure, například ověřit *uživatelské jméno*@emailoutlook.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="31eba-168">As a workaround, choose [Connect with Service Principal](/azure-resource-manager/resource-group-create-service-principal-portal.md), or authenticate as a member of the Azure Active Directory that's associated with your Azure subscription, for example, *user-name*@emailoutlook.onmicrosoft.com.</span></span>

8. <span data-ttu-id="31eba-169">Teď přihlášení k odběru aplikace logiky na události vydavatele.</span><span class="sxs-lookup"><span data-stu-id="31eba-169">Now subscribe your logic app to publisher events.</span></span> <span data-ttu-id="31eba-170">Zadejte podrobnosti pro vaše předplatné událostí uvedených v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="31eba-170">Provide the details for your event subscription as specified in the following table:</span></span>

   ![Zadejte podrobnosti pro předplatné událostí](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | <span data-ttu-id="31eba-172">Nastavení</span><span class="sxs-lookup"><span data-stu-id="31eba-172">Setting</span></span> | <span data-ttu-id="31eba-173">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="31eba-173">Suggested value</span></span> | <span data-ttu-id="31eba-174">Popis</span><span class="sxs-lookup"><span data-stu-id="31eba-174">Description</span></span> | 
   | ------- | --------------- | ----------- | 
   | <span data-ttu-id="31eba-175">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="31eba-175">**Subscription**</span></span> | <span data-ttu-id="31eba-176">*{virtuální počítače Azure předplatnými}*</span><span class="sxs-lookup"><span data-stu-id="31eba-176">*{virtual-machine-Azure-subscription}*</span></span> | <span data-ttu-id="31eba-177">Vyberte předplatné Azure vydavatel události.</span><span class="sxs-lookup"><span data-stu-id="31eba-177">Select the event publisher's Azure subscription.</span></span> <span data-ttu-id="31eba-178">V tomto kurzu vyberte předplatné Azure pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="31eba-178">For this tutorial, select the Azure subscription for your virtual machine.</span></span> | 
   | <span data-ttu-id="31eba-179">**Typ prostředku**</span><span class="sxs-lookup"><span data-stu-id="31eba-179">**Resource Type**</span></span> | <span data-ttu-id="31eba-180">Microsoft.Resources.resourceGroups</span><span class="sxs-lookup"><span data-stu-id="31eba-180">Microsoft.Resources.resourceGroups</span></span> | <span data-ttu-id="31eba-181">Vyberte typ prostředku vydavatel události.</span><span class="sxs-lookup"><span data-stu-id="31eba-181">Select the event publisher's resource type.</span></span> <span data-ttu-id="31eba-182">V tomto kurzu vyberte zadanou hodnotu, aby aplikace logiky monitoruje jenom skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="31eba-182">For this tutorial, select the specified value so your logic app monitors only resource groups.</span></span> | 
   | <span data-ttu-id="31eba-183">**Název prostředku**</span><span class="sxs-lookup"><span data-stu-id="31eba-183">**Resource Name**</span></span> | <span data-ttu-id="31eba-184">*{virtuální machine--název skupiny prostředků}*</span><span class="sxs-lookup"><span data-stu-id="31eba-184">*{virtual-machine-resource-group-name}*</span></span> | <span data-ttu-id="31eba-185">Vyberte název prostředku vydavatele.</span><span class="sxs-lookup"><span data-stu-id="31eba-185">Select the publisher's resource name.</span></span> <span data-ttu-id="31eba-186">V tomto kurzu vyberte název skupiny prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="31eba-186">For this tutorial, select the name of the resource group for your virtual machine.</span></span> | 
   | <span data-ttu-id="31eba-187">Volitelné nastavení, vyberte **zobrazit rozšířené možnosti**.</span><span class="sxs-lookup"><span data-stu-id="31eba-187">For optional settings, choose **Show advanced options**.</span></span> | <span data-ttu-id="31eba-188">*{v tématu Popis}*</span><span class="sxs-lookup"><span data-stu-id="31eba-188">*{see descriptions}*</span></span> | <span data-ttu-id="31eba-189">* **Předpony filtru**: v tomto kurzu ponechte prázdné toto nastavení.</span><span class="sxs-lookup"><span data-stu-id="31eba-189">* **Prefix Filter**: For this tutorial, leave this setting empty.</span></span> <span data-ttu-id="31eba-190">Výchozí chování odpovídá všechny hodnoty.</span><span class="sxs-lookup"><span data-stu-id="31eba-190">The default behavior matches all values.</span></span> <span data-ttu-id="31eba-191">Řetězec předpony můžete však zadat jako filtr, například cestu a parametr pro konkrétní zdroje.</span><span class="sxs-lookup"><span data-stu-id="31eba-191">However, you can specify a prefix string as a filter, for example, a path and a parameter for a specific resource.</span></span> <p><span data-ttu-id="31eba-192">* **Přípona filtru**: v tomto kurzu ponechte prázdné toto nastavení.</span><span class="sxs-lookup"><span data-stu-id="31eba-192">* **Suffix Filter**: For this tutorial, leave this setting empty.</span></span> <span data-ttu-id="31eba-193">Výchozí chování odpovídá všechny hodnoty.</span><span class="sxs-lookup"><span data-stu-id="31eba-193">The default behavior matches all values.</span></span> <span data-ttu-id="31eba-194">Řetězec přípony můžete však zadat jako filtr, například příponu názvu souboru, pokud chcete pouze určité typy souborů.</span><span class="sxs-lookup"><span data-stu-id="31eba-194">However, you can specify a suffix string as a filter, for example, a file name extension, when you want only specific file types.</span></span><p><span data-ttu-id="31eba-195">* **Název odběru**: Zadejte jedinečný název pro vaše předplatné událostí.</span><span class="sxs-lookup"><span data-stu-id="31eba-195">* **Subscription Name**: Provide a unique name for your event subscription.</span></span> |
   | | | 

   <span data-ttu-id="31eba-196">Když jste hotovi, vaše mřížky aktivační událost může vypadat jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="31eba-196">When you're done, your event grid trigger might look like this example:</span></span>
   
   ![Podrobnosti o aktivační události příklad události mřížky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. <span data-ttu-id="31eba-198">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-198">Save your logic app.</span></span> <span data-ttu-id="31eba-199">Na panelu nástrojů návrháře zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="31eba-199">On the designer toolbar, choose **Save**.</span></span> <span data-ttu-id="31eba-200">Chcete-li sbalení a skrytí podrobnosti akce v aplikaci logiky, zvolte akce záhlaví.</span><span class="sxs-lookup"><span data-stu-id="31eba-200">To collapse and hide an action's details in your logic app, choose the action's title bar.</span></span>

   ![Uložení aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   <span data-ttu-id="31eba-202">Při ukládání aplikace logiky s aktivační signál události mřížky, Azure automaticky vytvoří odběr událostí pro svou aplikaci logiky pro vybraný zdroj.</span><span class="sxs-lookup"><span data-stu-id="31eba-202">When you save your logic app with an event grid trigger, Azure automatically creates an event subscription for your logic app to your selected resource.</span></span> <span data-ttu-id="31eba-203">Proto když prostředek publikuje událost k mřížce událostí, mřížky této události automaticky doručí události do aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-203">So when the resource publishes an event to the event grid, that event grid automatically pushes the event to your logic app.</span></span> <span data-ttu-id="31eba-204">Tato událost se aktivuje aplikaci logiky potom vytvoří a spustí instance pracovního postupu, který definujete v tyto další kroky.</span><span class="sxs-lookup"><span data-stu-id="31eba-204">This event triggers your logic app, then creates and runs an instance of the workflow that you define in these next steps.</span></span>

<span data-ttu-id="31eba-205">Aplikace logiky je nyní za provozu a naslouchá na události z události mřížky, ale nic se neděje dokud nepřidáte do pracovního postupu akce.</span><span class="sxs-lookup"><span data-stu-id="31eba-205">Your logic app is now live and listens to events from the event grid, but doesn't do anything until you add actions to the workflow.</span></span> 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a><span data-ttu-id="31eba-206">Přidat podmínku, která kontroluje změny virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="31eba-206">Add a condition that checks for virtual machine changes</span></span>

<span data-ttu-id="31eba-207">Chcete-li spustit pracovní postup aplikace logiky jenom v případě, že se stane konkrétní události, přidejte podmínku, která kontroluje pro virtuální počítač "" operací zápisu.</span><span class="sxs-lookup"><span data-stu-id="31eba-207">To run your logic app workflow only when a specific event happens, add a condition that checks for virtual machine "write" operations.</span></span> <span data-ttu-id="31eba-208">Při splnění této podmínky svou aplikaci logiky odešle že e-mailu s podrobnostmi o aktualizované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="31eba-208">When this condition is true, your logic app sends you email with details about the updated virtual machine.</span></span>

1. <span data-ttu-id="31eba-209">V návrháři aplikace logiky, vyberte v části mřížky aktivační události, **nový krok** > **přidat podmínku**.</span><span class="sxs-lookup"><span data-stu-id="31eba-209">In Logic App Designer, under the event grid trigger, choose **New step** > **Add a condition**.</span></span>

   ![Přidat podmínku do aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   <span data-ttu-id="31eba-211">Návrhář aplikace logiky přidá podmínku prázdný do pracovního postupu, včetně cesty akce provést na základě, zda je podmínka vyhodnocena jako true nebo false.</span><span class="sxs-lookup"><span data-stu-id="31eba-211">The Logic App Designer adds an empty condition to your workflow, including action paths to follow based whether the condition is true or false.</span></span>

   ![Prázdný podmínky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. <span data-ttu-id="31eba-213">V **podmínku** vyberte **upravit v rozšířeném režimu**.</span><span class="sxs-lookup"><span data-stu-id="31eba-213">In the **Condition** box, choose **Edit in advanced mode**.</span></span>
<span data-ttu-id="31eba-214">Zadejte tento výraz:</span><span class="sxs-lookup"><span data-stu-id="31eba-214">Enter this expression:</span></span>

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   <span data-ttu-id="31eba-215">Vaše podmínky nyní vypadá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="31eba-215">Your condition now looks like this example:</span></span>

   ![Prázdný podmínky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   <span data-ttu-id="31eba-217">Tento výraz kontroluje události `body` pro `data` objekt kde `operationName` vlastnost je `Microsoft.Compute/virtualMachines/write` operace.</span><span class="sxs-lookup"><span data-stu-id="31eba-217">This expression checks the event `body` for a `data` object where the `operationName` property is the `Microsoft.Compute/virtualMachines/write` operation.</span></span> 
   <span data-ttu-id="31eba-218">Další informace o [schématu události událostí mřížky](../event-grid/event-schema.md).</span><span class="sxs-lookup"><span data-stu-id="31eba-218">Learn more about [Event Grid event schema](../event-grid/event-schema.md).</span></span>

3. <span data-ttu-id="31eba-219">Pokud chcete zadat popis pro podmínku, vyberte **výpustky** (**...** ) tlačítko obrazec podmínky, a potom vyberte **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="31eba-219">To provide a description for the condition, choose the **ellipses** (**...**) button on the condition shape, then choose **Rename**.</span></span>

   > [!NOTE] 
   > <span data-ttu-id="31eba-220">Novější příklady v tomto kurzu také popisují kroky v pracovním postupu aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-220">The later examples in this tutorial also provide descriptions for steps in the logic app workflow.</span></span>

4. <span data-ttu-id="31eba-221">Teď zvolte **upravit v režimu základní** tak, aby výraz automaticky vyřeší, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="31eba-221">Now choose **Edit in basic mode** so that the expression automatically resolves as shown:</span></span>

   ![Stav aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. <span data-ttu-id="31eba-223">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-223">Save your logic app.</span></span>

## <a name="send-email-when-your-virtual-machine-changes"></a><span data-ttu-id="31eba-224">Odeslání e-mailu, když se změní virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="31eba-224">Send email when your virtual machine changes</span></span>

<span data-ttu-id="31eba-225">Nyní přidejte [ *akce* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) , abyste měli e-mailu, když je zadaná podmínka vyhodnocena jako true.</span><span class="sxs-lookup"><span data-stu-id="31eba-225">Now add an [*action*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) so that you get an email when the specified condition is true.</span></span>

1. <span data-ttu-id="31eba-226">V podmínce pro **v případě hodnoty true** vyberte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="31eba-226">In the condition's **If true** box, choose **Add an action**.</span></span>

   ![Akce pro když je splněna podmínka pro přidání](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. <span data-ttu-id="31eba-228">Do vyhledávacího pole zadejte "e-mailu" jako filtr.</span><span class="sxs-lookup"><span data-stu-id="31eba-228">In the search box, enter "email" as your filter.</span></span> <span data-ttu-id="31eba-229">Založená na zprostředkovateli e-mailu, najděte a vyberte odpovídající konektor.</span><span class="sxs-lookup"><span data-stu-id="31eba-229">Based on your email provider, find and select the matching connector.</span></span> <span data-ttu-id="31eba-230">Vyberte akci "odeslání e-mailu" pro vaše konektor.</span><span class="sxs-lookup"><span data-stu-id="31eba-230">Then select the "send email" action for your connector.</span></span> <span data-ttu-id="31eba-231">Například:</span><span class="sxs-lookup"><span data-stu-id="31eba-231">For example:</span></span> 

   * <span data-ttu-id="31eba-232">Azure pracovní nebo školní účet, vyberte konektor Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="31eba-232">For an Azure work or school account, select the Office 365 Outlook connector.</span></span> 
   * <span data-ttu-id="31eba-233">Pro osobní účty Microsoft vyberte konektor Outlook.com.</span><span class="sxs-lookup"><span data-stu-id="31eba-233">For personal Microsoft accounts, select the Outlook.com connector.</span></span> 
   * <span data-ttu-id="31eba-234">Pro účty z Gmailu vyberte konektor z Gmailu.</span><span class="sxs-lookup"><span data-stu-id="31eba-234">For Gmail accounts, select the Gmail connector.</span></span> 

   <span data-ttu-id="31eba-235">Vytvoříme pokračujte konektor Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="31eba-235">We're going to continue with the Office 365 Outlook connector.</span></span> 
   <span data-ttu-id="31eba-236">Pokud chcete použít k jinému zprostředkovateli, kroky zůstávají stejné, ale uživatelské rozhraní se může objevit jiný.</span><span class="sxs-lookup"><span data-stu-id="31eba-236">If you use a different provider, the steps remain the same, but your UI might appear different.</span></span> 

   ![Vyberte možnost odeslat e-mailu panel.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. <span data-ttu-id="31eba-238">Pokud ještě nemáte připojení pro poskytovatele e-mailu, přihlaste se ke svému účtu e-mailu po zobrazení dotazu pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="31eba-238">If you don't already have a connection for your email provider, sign in to your email account when you're asked for authentication.</span></span>

4. <span data-ttu-id="31eba-239">Zadejte podrobnosti pro e-mailu uvedeného v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="31eba-239">Provide details for the email as specified in the following table:</span></span>

   ![Akce prázdný e-mailu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > <span data-ttu-id="31eba-241">Pokud chcete vyberte pole, které jsou k dispozici v pracovním postupu, klikněte na tlačítko do textového pole, která **dynamický obsah** seznamu otevře, nebo zvolte **přidávat dynamický obsah**.</span><span class="sxs-lookup"><span data-stu-id="31eba-241">To select from fields available in your workflow, click in an edit box so that the **Dynamic content** list opens, or choose **Add dynamic content**.</span></span> <span data-ttu-id="31eba-242">Pro další pole, zvolte **Další informace naleznete v** pro každý oddíl v seznamu.</span><span class="sxs-lookup"><span data-stu-id="31eba-242">For more fields, choose **See more** for each section in the list.</span></span> <span data-ttu-id="31eba-243">Zavřete **dynamický obsah** vyberte **přidávat dynamický obsah**.</span><span class="sxs-lookup"><span data-stu-id="31eba-243">To close the **Dynamic content** list, choose **Add dynamic content**.</span></span>

   | <span data-ttu-id="31eba-244">Nastavení</span><span class="sxs-lookup"><span data-stu-id="31eba-244">Setting</span></span> | <span data-ttu-id="31eba-245">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="31eba-245">Suggested value</span></span> | <span data-ttu-id="31eba-246">Popis</span><span class="sxs-lookup"><span data-stu-id="31eba-246">Description</span></span> | 
   | ------- | --------------- | ----------- | 
   | <span data-ttu-id="31eba-247">**K**</span><span class="sxs-lookup"><span data-stu-id="31eba-247">**To**</span></span> | <span data-ttu-id="31eba-248">*{příjemce e-mailovou adresu}*</span><span class="sxs-lookup"><span data-stu-id="31eba-248">*{recipient-email-address}*</span></span> |<span data-ttu-id="31eba-249">Zadejte příjemce e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="31eba-249">Enter the recipient's email address.</span></span> <span data-ttu-id="31eba-250">Pro účely testování můžete použít vlastní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="31eba-250">For testing purposes, you can use your own email address.</span></span> | 
   | <span data-ttu-id="31eba-251">**Předmět**</span><span class="sxs-lookup"><span data-stu-id="31eba-251">**Subject**</span></span> | <span data-ttu-id="31eba-252">Prostředek aktualizovat: **subjektu**</span><span class="sxs-lookup"><span data-stu-id="31eba-252">Resource updated: **Subject**</span></span>| <span data-ttu-id="31eba-253">Zadejte obsah subjektu k e-mailu.</span><span class="sxs-lookup"><span data-stu-id="31eba-253">Enter the content for the email's subject.</span></span> <span data-ttu-id="31eba-254">V tomto kurzu, zadejte text, návrhy a vyberte události **subjektu** pole.</span><span class="sxs-lookup"><span data-stu-id="31eba-254">For this tutorial, enter the suggested text and select the event's **Subject** field.</span></span> <span data-ttu-id="31eba-255">Zde vaše předmět e-mailu obsahuje název pro aktualizovaný prostředek (virtuálním počítači).</span><span class="sxs-lookup"><span data-stu-id="31eba-255">Here, your email subject includes the name for the updated resource (virtual machine).</span></span> | 
   | <span data-ttu-id="31eba-256">**Text**</span><span class="sxs-lookup"><span data-stu-id="31eba-256">**Body**</span></span> | <span data-ttu-id="31eba-257">Skupina prostředků: **tématu**</span><span class="sxs-lookup"><span data-stu-id="31eba-257">Resource group: **Topic**</span></span> <p><span data-ttu-id="31eba-258">Typ události: **typ události**</span><span class="sxs-lookup"><span data-stu-id="31eba-258">Event type: **Event Type**</span></span><p><span data-ttu-id="31eba-259">ID události: **ID**</span><span class="sxs-lookup"><span data-stu-id="31eba-259">Event ID: **ID**</span></span><p><span data-ttu-id="31eba-260">Čas: **čas události**</span><span class="sxs-lookup"><span data-stu-id="31eba-260">Time: **Event Time**</span></span> | <span data-ttu-id="31eba-261">Zadejte obsah obsahu e-mailu.</span><span class="sxs-lookup"><span data-stu-id="31eba-261">Enter the content for the email's body.</span></span> <span data-ttu-id="31eba-262">V tomto kurzu, zadejte text, návrhy a vyberte události **tématu**, **typ události**, **ID**, a **čas události** pole tak, že e-mailu obsahují název skupiny prostředků, typ události, časové razítko události a ID události pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="31eba-262">For this tutorial, enter the suggested text and select the event's **Topic**, **Event Type**, **ID**, and **Event Time** fields so that your email includes the resource group name, event type, event timestamp, and event ID for the update.</span></span> <p><span data-ttu-id="31eba-263">Chcete-li přidat prázdné řádky v obsahu, stiskněte Shift + Enter.</span><span class="sxs-lookup"><span data-stu-id="31eba-263">To add blank lines in your content, press Shift + Enter.</span></span> | 
   | | | 

   > [!NOTE] 
   > <span data-ttu-id="31eba-264">Pokud vyberete pole, které představuje pole, návrhář automaticky přidá **pro každou** smyčky kolem akce, která odkazuje na pole.</span><span class="sxs-lookup"><span data-stu-id="31eba-264">If you select a field that represents an array, the designer automatically adds a **For each** loop around the action that references the array.</span></span> <span data-ttu-id="31eba-265">Tímto způsobem svou aplikaci logiky tuto akci provede na jednotlivé položky pole.</span><span class="sxs-lookup"><span data-stu-id="31eba-265">That way, your logic app performs that action on each array item.</span></span>

   <span data-ttu-id="31eba-266">Teď může vaše e-mailu akce vypadat jako tento ukázkový:</span><span class="sxs-lookup"><span data-stu-id="31eba-266">Now, your email action might look like this example:</span></span>

   ![Vyberte výstupy chcete zahrnout do e-mailu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   <span data-ttu-id="31eba-268">A aplikace logiky dokončení může vypadat jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="31eba-268">And your finished logic app might look like this example:</span></span>

   ![Aplikace logiky dokončení](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. <span data-ttu-id="31eba-270">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-270">Save your logic app.</span></span> <span data-ttu-id="31eba-271">Chcete-li sbalení a skrytí podrobnosti každá akce v aplikaci logiky, zvolte akce záhlaví.</span><span class="sxs-lookup"><span data-stu-id="31eba-271">To collapse and hide each action's details in your logic app, choose the action's title bar.</span></span>

   ![Uložení aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   <span data-ttu-id="31eba-273">Aplikace logiky je nyní za provozu, ale čeká změny k virtuálnímu počítači před jakékoli akce.</span><span class="sxs-lookup"><span data-stu-id="31eba-273">Your logic app is now live, but waits for changes to your virtual machine before doing anything.</span></span> 
   <span data-ttu-id="31eba-274">K testování aplikace logiky nyní, pokračujte v další části.</span><span class="sxs-lookup"><span data-stu-id="31eba-274">To test your logic app now, continue to the next section.</span></span>

## <a name="test-your-logic-app-workflow"></a><span data-ttu-id="31eba-275">Testování pracovního postupu aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="31eba-275">Test your logic app workflow</span></span>

1. <span data-ttu-id="31eba-276">Pokud chcete zkontrolovat, že je aplikace logiky získávání určených událostí, aktualizujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="31eba-276">To check that your logic app is getting the specified events, update your virtual machine.</span></span> 

   <span data-ttu-id="31eba-277">Například můžete změnit velikost virtuálního počítače na portálu Azure nebo [změnit velikost virtuálního počítače v prostředí Azure PowerShell](../virtual-machines/windows/resize-vm.md).</span><span class="sxs-lookup"><span data-stu-id="31eba-277">For example, you can resize your virtual machine in the Azure portal or [resize your VM with Azure PowerShell](../virtual-machines/windows/resize-vm.md).</span></span> 

   <span data-ttu-id="31eba-278">Po chvíli měli byste obdržet e-mailu.</span><span class="sxs-lookup"><span data-stu-id="31eba-278">After a few moments, you should get an email.</span></span> <span data-ttu-id="31eba-279">Například:</span><span class="sxs-lookup"><span data-stu-id="31eba-279">For example:</span></span>

   ![E-mailu o aktualizace virtuálního počítače](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. <span data-ttu-id="31eba-281">Chcete-li zkontrolovat je spuštěn a aktivuje historie pro svou aplikaci logiky, v nabídce aplikace logiky, zvolte **přehled**.</span><span class="sxs-lookup"><span data-stu-id="31eba-281">To review the runs and trigger history for your logic app, on your logic app menu, choose **Overview**.</span></span> <span data-ttu-id="31eba-282">Chcete-li zobrazit další podrobnosti o spuštění, vyberte řádek pro tento spustit.</span><span class="sxs-lookup"><span data-stu-id="31eba-282">To view more details about a run, choose the row for that run.</span></span>

   ![Spuštění aplikace logiky historie](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. <span data-ttu-id="31eba-284">Chcete-li zobrazit vstupy a výstupy pro každý krok, rozbalte krok, který chcete zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="31eba-284">To view the inputs and outputs for each step, expand the step that you want to review.</span></span> <span data-ttu-id="31eba-285">Tyto informace můžete diagnostikovat a ladit problémy v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="31eba-285">This information can help you diagnose and debug problems in your logic app.</span></span>
 
   ![Podrobnosti o historii spuštění aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

<span data-ttu-id="31eba-287">Blahopřejeme, jste vytvořili a spusťte aplikaci logiky, která sleduje události prostředků prostřednictvím mřížce událostí a odešle e-mail, když tyto události dojít.</span><span class="sxs-lookup"><span data-stu-id="31eba-287">Congratulations, you've created and run a logic app that monitors resource events through an event grid and emails you when those events happen.</span></span> <span data-ttu-id="31eba-288">Také jste zjistili, jak snadno můžete vytvořit pracovní postupy, které automatizovat procesy a integrovat systémy a cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="31eba-288">You also learned how easily you can create workflows that automate processes and integrate systems and cloud services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="31eba-289">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="31eba-289">Clean up resources</span></span>

<span data-ttu-id="31eba-290">Tento kurz používá prostředky a provádí akce, které platit poplatky na vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="31eba-290">This tutorial uses resources and performs actions that incur charges on your Azure subscription.</span></span> <span data-ttu-id="31eba-291">Když jste hotovi s kurz a testování, ujistěte se, že tak zakázat nebo odstranit všechny prostředky, které nechcete platit poplatky.</span><span class="sxs-lookup"><span data-stu-id="31eba-291">So when you're done with the tutorial and testing, make sure that you disable or delete any resources where you don't want to incur charges.</span></span>

<span data-ttu-id="31eba-292">Zastavit můžete svou aplikaci logiky spuštěná a odesílání e-mailu bez odstranění aplikace.</span><span class="sxs-lookup"><span data-stu-id="31eba-292">You can stop your logic app from running and sending email without deleting the app.</span></span> <span data-ttu-id="31eba-293">V nabídce aplikace logiky, vyberte **přehled**.</span><span class="sxs-lookup"><span data-stu-id="31eba-293">On your logic app menu, choose **Overview**.</span></span> <span data-ttu-id="31eba-294">Na panelu nástrojů vyberte **zakázat**.</span><span class="sxs-lookup"><span data-stu-id="31eba-294">On the toolbar, choose **Disable**.</span></span>

![Vypnout aplikaci logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a><span data-ttu-id="31eba-296">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="31eba-296">FAQ</span></span>

<span data-ttu-id="31eba-297">**Q**: co jiným virtuálním počítačem monitorování úloh lze provést pomocí událostí mřížky a aplikacích logiky?</span><span class="sxs-lookup"><span data-stu-id="31eba-297">**Q**: What other virtual machine monitoring tasks can I perform with event grids and logic apps?</span></span> </br><span data-ttu-id="31eba-298">
**A**: můžete sledovat další změny v konfiguraci, například:</span><span class="sxs-lookup"><span data-stu-id="31eba-298">
**A**: You can monitor other configuration changes, for example:</span></span>

* <span data-ttu-id="31eba-299">Virtuální počítač získá přístup na základě rolí práva řízení (RBAC).</span><span class="sxs-lookup"><span data-stu-id="31eba-299">A virtual machine gets role-based access control (RBAC) rights.</span></span>
* <span data-ttu-id="31eba-300">Skupina zabezpečení sítě (NSG) v síťovém rozhraní (NIC) jsou provedeny změny.</span><span class="sxs-lookup"><span data-stu-id="31eba-300">Changes are made to a network security group (NSG) on a network interface (NIC).</span></span>
* <span data-ttu-id="31eba-301">Disky pro virtuální počítač po přidání nebo odebrání.</span><span class="sxs-lookup"><span data-stu-id="31eba-301">Disks for a virtual machine are added or removed.</span></span>
* <span data-ttu-id="31eba-302">Veřejná IP adresa je přiřazena k virtuálnímu počítači síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="31eba-302">A public IP address is assigned to a virtual machine NIC.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31eba-303">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31eba-303">Next steps</span></span>

* [<span data-ttu-id="31eba-304">Přehled služby Event mřížky</span><span class="sxs-lookup"><span data-stu-id="31eba-304">Event Grid Overview</span></span>](../event-grid/overview.md)
* [<span data-ttu-id="31eba-305">Koncepty mřížky událostí</span><span class="sxs-lookup"><span data-stu-id="31eba-305">Event Grid Concepts</span></span>](../event-grid/concepts.md)
* [<span data-ttu-id="31eba-306">Rychlý úvod: Vytvoření a trasy vlastních událostí s událostí mřížky</span><span class="sxs-lookup"><span data-stu-id="31eba-306">Quickstart: Create and route custom events with Event Grid</span></span>](../event-grid/custom-event-quickstart.md)
* [<span data-ttu-id="31eba-307">Událost schématu události mřížky</span><span class="sxs-lookup"><span data-stu-id="31eba-307">Event Grid event schema</span></span>](../event-grid/event-schema.md)
* [<span data-ttu-id="31eba-308">Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="31eba-308">Azure Logic Apps</span></span>](../logic-apps/logic-apps-what-are-logic-apps.md)
* [<span data-ttu-id="31eba-309">Vytvořit pracovní postupy aplikace logiky pomocí předdefinovaných šablon</span><span class="sxs-lookup"><span data-stu-id="31eba-309">Create logic app workflows with predefined templates</span></span>](../logic-apps/logic-apps-use-logic-app-templates.md)