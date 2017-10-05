---
title: "Zotavení po havárii pro účet Integrace B2B – Azure Logic Apps | Microsoft Docs"
description: "Zotavení po havárii B2B aplikace logiky"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 4896d9da456bcc17b1a4d92259ef3d57f8575d8b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a><span data-ttu-id="50fc8-103">Zotavení po havárii mezi oblastmi B2B aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="50fc8-103">Logic Apps B2B cross-region disaster recovery</span></span>

<span data-ttu-id="50fc8-104">Úlohy B2B zahrnovat peníze transakce jako objednávky a faktury.</span><span class="sxs-lookup"><span data-stu-id="50fc8-104">B2B workloads involve money transactions like orders and invoices.</span></span> <span data-ttu-id="50fc8-105">Během události po havárii, je velmi důležitá pro podnik k rychle obnovit do splnění smluv SLA firemní úrovni dohodnutých s jejich partnery.</span><span class="sxs-lookup"><span data-stu-id="50fc8-105">During a disaster event, it's critical for a business to quickly recover to meet the business-level SLAs agreed upon with their partners.</span></span> <span data-ttu-id="50fc8-106">Tento článek ukazuje, jak sestavit plán kontinuity obchodních pro úlohy B2B.</span><span class="sxs-lookup"><span data-stu-id="50fc8-106">This article demonstrates how to build a business continuity plan for B2B workloads.</span></span> 

* <span data-ttu-id="50fc8-107">Připravenost obnovení po havárii</span><span class="sxs-lookup"><span data-stu-id="50fc8-107">Disaster Recovery readiness</span></span> 
* <span data-ttu-id="50fc8-108">Převzetí služeb při selhání pro sekundární oblast během události po havárii</span><span class="sxs-lookup"><span data-stu-id="50fc8-108">Fail over to secondary region during a disaster event</span></span> 
* <span data-ttu-id="50fc8-109">Vrátit zpět na primární oblasti po události po havárii</span><span class="sxs-lookup"><span data-stu-id="50fc8-109">Fall back to primary region after a disaster event</span></span>

## <a name="disaster-recovery-readiness"></a><span data-ttu-id="50fc8-110">Připravenost obnovení po havárii</span><span class="sxs-lookup"><span data-stu-id="50fc8-110">Disaster Recovery readiness</span></span>  

1. <span data-ttu-id="50fc8-111">Určete sekundární oblasti a vytvořte [integrace účet](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-111">Identify a secondary region and create an [integration account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) in the secondary region.</span></span>

2. <span data-ttu-id="50fc8-112">Přidání partnery, schémat a smluv pro požadované zpráva toky, kde stav spuštění musí replikovat na sekundární oblasti integrace účet.</span><span class="sxs-lookup"><span data-stu-id="50fc8-112">Add partners, schemas, and agreements for the required message flows where the run status needs to be replicated to secondary region integration account.</span></span>

   > [!TIP]
   > <span data-ttu-id="50fc8-113">Ujistěte se, že konzistence v účtu artefaktů integrace zásady vytváření názvů v oblastech.</span><span class="sxs-lookup"><span data-stu-id="50fc8-113">Make sure there's consistency in the integration account artifact's naming convention across regions.</span></span> 

3. <span data-ttu-id="50fc8-114">Stav spuštění načítat z primární oblasti, vytvoření aplikace logiky v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-114">To pull the run status from the primary region, create a logic app in the secondary region.</span></span> 

   <span data-ttu-id="50fc8-115">Musí mít tuto aplikaci logiky *aktivační událost* a *akce*.</span><span class="sxs-lookup"><span data-stu-id="50fc8-115">This logic app should have a *trigger* and an *action*.</span></span> 
   <span data-ttu-id="50fc8-116">Aktivační události by se měly připojit primární oblasti integrace účtu a akci by se měly připojit sekundární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-116">The trigger should connect to primary region integration account, and the action should connect to secondary region integration account.</span></span> 
   <span data-ttu-id="50fc8-117">Podle toho, časový interval, aktivační událost dotazování primární oblasti spustit stav tabulce a vrátí nové záznamy, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="50fc8-117">Based on the time interval, the trigger polls the primary region run status table and pulls the new records, if any.</span></span> <span data-ttu-id="50fc8-118">Akce aktualizace je sekundární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-118">The action updates them to secondary region integration account.</span></span> 
   <span data-ttu-id="50fc8-119">To pomůže získat přírůstkové běhový stav od primární oblasti sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-119">This helps to get incremental runtime status from primary region to secondary region.</span></span>

4. <span data-ttu-id="50fc8-120">Kontinuita podnikových procesů v účtu integrace Logic Apps je navržen pro podporu založené na protokolech B2B - X12 AS2, EDIFACT a.</span><span class="sxs-lookup"><span data-stu-id="50fc8-120">Business continuity in Logic Apps integration account is designed to support based on B2B protocols - X12, AS2, and EDIFACT.</span></span> <span data-ttu-id="50fc8-121">Podrobné kroky, vyberete na příslušné odkazy.</span><span class="sxs-lookup"><span data-stu-id="50fc8-121">To find detailed steps, select the respective links.</span></span>

5. <span data-ttu-id="50fc8-122">Doporučuje se pro všechny prostředky primární oblasti v sekundární oblasti příliš nasazení.</span><span class="sxs-lookup"><span data-stu-id="50fc8-122">The recommendation is to deploy all primary region resources in a secondary region too.</span></span> 

   <span data-ttu-id="50fc8-123">Primární oblasti prostředky zahrnují Azure SQL Database nebo Azure Cosmos DB, Azure Service Bus a Azure Event Hubs použít pro zasílání zpráv Azure API Management a funkce Azure Logic Apps v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="50fc8-123">Primary region resources include Azure SQL Database or Azure Cosmos DB, Azure Service Bus and Azure Event Hubs used for messaging, Azure API Management, and the Azure Logic Apps feature in Azure App Service.</span></span>   

6. <span data-ttu-id="50fc8-124">Navázání spojení mezi primární oblasti v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-124">Establish a connection from a primary region to a secondary region.</span></span> <span data-ttu-id="50fc8-125">Stav spuštění načítat z primární oblasti, vytvoření aplikace logiky v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-125">To pull the run status from a primary region, create a logic app in a secondary region.</span></span> 

   <span data-ttu-id="50fc8-126">Aplikace logiky měli aktivační události a akce.</span><span class="sxs-lookup"><span data-stu-id="50fc8-126">The logic app should have a trigger and an action.</span></span> 
   <span data-ttu-id="50fc8-127">Aktivační události by měl připojení k účtu integrace primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-127">The trigger should connect to a primary region integration account.</span></span> 
   <span data-ttu-id="50fc8-128">Akce by měl připojení k účtu integrace sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="50fc8-128">The action should connect to a secondary region integration account.</span></span> 
   <span data-ttu-id="50fc8-129">Podle toho, časový interval, aktivační událost dotazování primární oblasti spustit stav tabulce a vrátí nové záznamy, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="50fc8-129">Based on the time interval, the trigger polls the primary region run status table and pulls the new records, if any.</span></span> 
   <span data-ttu-id="50fc8-130">Akce aktualizace je účet integrace sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="50fc8-130">The action updates them to a secondary region integration account.</span></span> 
   <span data-ttu-id="50fc8-131">Tento proces pomáhá získání přírůstkové běhového stavu od primární oblasti sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="50fc8-131">This process helps to get incremental runtime status from the primary region to the secondary region.</span></span>

<span data-ttu-id="50fc8-132">Kontinuita podnikových procesů v účtu integrace Logic Apps poskytuje podporu na základě protokolů B2B X12, AS2, EDIFACT a.</span><span class="sxs-lookup"><span data-stu-id="50fc8-132">Business continuity in a Logic Apps integration account provides support based on the B2B protocols X12, AS2, and EDIFACT.</span></span> <span data-ttu-id="50fc8-133">Podrobné pokyny týkající se použití X12 a AS2 najdete v tématu [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) a [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="50fc8-133">For detailed steps on using X12 and AS2, see [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) and [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) in this article.</span></span>

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a><span data-ttu-id="50fc8-134">Převzetí služeb při selhání pro sekundární oblast během události po havárii</span><span class="sxs-lookup"><span data-stu-id="50fc8-134">Fail over to a secondary region during a disaster event</span></span>

<span data-ttu-id="50fc8-135">Během události po havárii, když primární oblasti není k dispozici pro kontinuitu podnikových procesů, přímé přenosy sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="50fc8-135">During a disaster event, when the primary region is not available for business continuity, direct traffic to the secondary region.</span></span> <span data-ttu-id="50fc8-136">Pomáhá sekundární oblasti a obchodní obnovení funkce rychle tak, aby splňoval RPO/RTO dohodnutých jejich partnery.</span><span class="sxs-lookup"><span data-stu-id="50fc8-136">A secondary region helps a business to recover functions quickly to meet the RPO/RTO agreed upon by their partners.</span></span> <span data-ttu-id="50fc8-137">Minimalizuje se také ve snaze o převzetí služeb při selhání z jedné oblasti jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-137">It also minimizes efforts to fail over from one region to another region.</span></span> 

<span data-ttu-id="50fc8-138">Při kopírování řízení čísla od primární oblasti v sekundární oblasti očekávané latence neexistuje.</span><span class="sxs-lookup"><span data-stu-id="50fc8-138">There is an expected latency while copying control numbers from a primary region to a secondary region.</span></span> <span data-ttu-id="50fc8-139">Nechcete-li odesílání duplicitní generovaného řízení čísla partnerům během události po havárii, doporučuje se má zvýšit číslo ovládacího prvku v sekundární oblasti smlouvy pomocí [rutiny prostředí PowerShell](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="50fc8-139">To avoid sending duplicate generated control numbers to partners during a disaster event, the recommendation is to increment the control numbers in the secondary region agreements by using [PowerShell cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span></span>

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a><span data-ttu-id="50fc8-140">Vrátit zpět na primární oblasti událost po havárii</span><span class="sxs-lookup"><span data-stu-id="50fc8-140">Fall back to a primary region post-disaster event</span></span>

<span data-ttu-id="50fc8-141">K návratu do primární oblasti případě, že je k dispozici, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="50fc8-141">To fall back to a primary region when it is available, follow these steps:</span></span>

1. <span data-ttu-id="50fc8-142">Zastavte příjem zpráv od partnerů v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-142">Stop accepting messages from partners in the secondary region.</span></span>  

2. <span data-ttu-id="50fc8-143">Zvýšit číslo generované ovládací prvek pro všechny smlouvy primární oblasti s použitím [rutiny prostředí PowerShell](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="50fc8-143">Increment the generated control numbers for all the primary region agreements by using [PowerShell cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span></span>  

3. <span data-ttu-id="50fc8-144">Přímé přenosy z sekundární oblast na primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-144">Direct traffic from the secondary region to the primary region.</span></span>

4. <span data-ttu-id="50fc8-145">Zkontrolujte, zda je povolena v sekundární oblasti pro stahování, spusťte z primární oblasti stav vytvořit aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="50fc8-145">Check that the logic app created in the secondary region for pulling run status from the primary region is enabled.</span></span>

## <a name="x12"></a><span data-ttu-id="50fc8-146">X12</span><span class="sxs-lookup"><span data-stu-id="50fc8-146">X12</span></span> 

<span data-ttu-id="50fc8-147">Kontinuita podnikových procesů pro EDI X 12 dokumentů je založena na ovládací prvek čísla:</span><span class="sxs-lookup"><span data-stu-id="50fc8-147">Business continuity for EDI X12 documents is based on control numbers:</span></span>

> [!TIP]
> <span data-ttu-id="50fc8-148">Můžete také [X12 rychlý start šablony](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) k vytvoření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="50fc8-148">You can also use the [X12 quick start template](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) to create logic apps.</span></span> <span data-ttu-id="50fc8-149">Vytváření primární a sekundární integrace účty jsou nezbytné požadavky pro použití šablony.</span><span class="sxs-lookup"><span data-stu-id="50fc8-149">Creating primary and secondary integration accounts are prerequisites to use the template.</span></span> <span data-ttu-id="50fc8-150">Šablona pomůže vytvořit dvě logiku aplikace, jednu pro čísla přijaté řízení a druhou pro generovaný řízení čísla.</span><span class="sxs-lookup"><span data-stu-id="50fc8-150">The template helps to create two logic apps, one for received control numbers and another for generated control numbers.</span></span> <span data-ttu-id="50fc8-151">Příslušné triggery a akce se vytvoří v aplikace logiky, připojení k účtu primární integrace a akce účet sekundární integrace aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="50fc8-151">Respective triggers and actions are created in the logic apps, connecting the trigger to the primary integration account and the action to the secondary integration account.</span></span>

<span data-ttu-id="50fc8-152">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="50fc8-152">**Prerequisites**</span></span>

<span data-ttu-id="50fc8-153">Chcete-li povolit zotavení po havárii pro příchozí zprávy, vyberte duplicitní zkontrolujte nastavení v X12 nastavení přijmout smlouvy.</span><span class="sxs-lookup"><span data-stu-id="50fc8-153">To enable disaster recovery for inbound messages, select the duplicate check settings in the X12 agreement's Receive Settings.</span></span>

![Vyberte nastavení duplicitní kontroly](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. <span data-ttu-id="50fc8-155">Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-155">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in a secondary region.</span></span>    

2. <span data-ttu-id="50fc8-156">Hledání **X12**a vyberte **X12-při změně číslo řízení**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-156">Search on **X12**, and select **X12 - When a control number is modified**.</span></span>   

   ![Vyhledejte X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   <span data-ttu-id="50fc8-158">Aktivační událost zobrazí výzvu k navázání připojení k účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="50fc8-158">The trigger prompts you to establish a connection to an integration account.</span></span> 
   <span data-ttu-id="50fc8-159">Aktivační události by měly být připojené k primární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-159">The trigger should be connected to a primary region integration account.</span></span>

3. <span data-ttu-id="50fc8-160">Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* ze seznamu a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-160">Enter a connection name, select your *primary region integration account* from the list, and choose **Create**.</span></span>   

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. <span data-ttu-id="50fc8-162">**Datum a čas k řízení číslo synchronizaci spustit** nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="50fc8-162">The **DateTime to start control number sync** setting is optional.</span></span> <span data-ttu-id="50fc8-163">**Frekvence** může být nastaven na **den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-163">The **Frequency** can be set to **Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>   

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. <span data-ttu-id="50fc8-165">Vyberte **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-165">Select **New step** > **Add an action**.</span></span>

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. <span data-ttu-id="50fc8-167">Hledání **X12**a vyberte **X12-přidáte nebo aktualizujete řízení čísla**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-167">Search on **X12**, and select **X12 - Add or update control numbers**.</span></span>   

   ![Přidat nebo aktualizovat čísla ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. <span data-ttu-id="50fc8-169">Chcete-li akce připojení k účtu integrace sekundární oblasti, vyberte **změnit připojení** > **přidat nové připojení** seznam účtů k dispozici integrace.</span><span class="sxs-lookup"><span data-stu-id="50fc8-169">To connect an action to a secondary region integration account, select **Change connection** > **Add new connection** for a list of the available integration accounts.</span></span> <span data-ttu-id="50fc8-170">Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* ze seznamu a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-170">Enter a connection name, select your *secondary region integration account* from the list, and choose **Create**.</span></span> 

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. <span data-ttu-id="50fc8-172">Přepněte na nezpracovaná vstupy kliknutím na ikonu v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-172">Switch to raw inputs by clicking on the icon in upper right corner.</span></span>

   ![Přepnout na nezpracovaná vstupy](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. <span data-ttu-id="50fc8-174">Vybrat text z dynamického obsahu výběr a uložte aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="50fc8-174">Select Body from the dynamic content picker, and save the logic app.</span></span>

   ![Dynamická pole obsahu](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   <span data-ttu-id="50fc8-176">Založený na časový interval, aktivační události dotazuje primární oblasti přijatých řízení číslo v tabulce a vrátí nové záznamy.</span><span class="sxs-lookup"><span data-stu-id="50fc8-176">Based on the time interval, the trigger polls the primary region received control number table and pulls the new records.</span></span> 
   <span data-ttu-id="50fc8-177">Akce aktualizace záznamů v sekundární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-177">The action updates the records in the secondary region integration account.</span></span> 
   <span data-ttu-id="50fc8-178">Pokud nejsou dostupné žádné aktualizace, stav aktivační události zobrazuje jako **vynecháno**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-178">If there are no updates, the trigger status appears as **Skipped**.</span></span>   

   ![Číslo tabulky ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

<span data-ttu-id="50fc8-180">Podle toho, časový interval, přírůstkové běhový stav replikuje z primární oblasti v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-180">Based on the time interval, the incremental runtime status replicates from a primary region to a secondary region.</span></span> <span data-ttu-id="50fc8-181">Během události po havárii, když primární oblasti není k dispozici, přímé přenosy sekundární oblast pro kontinuitu podnikových procesů.</span><span class="sxs-lookup"><span data-stu-id="50fc8-181">During a disaster event, when the primary region is not available, direct traffic to the secondary region for business continuity.</span></span> 

## <a name="edifact"></a><span data-ttu-id="50fc8-182">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="50fc8-182">EDIFACT</span></span> 

<span data-ttu-id="50fc8-183">Kontinuita podnikových procesů pro dokumenty EDI EDIFACT je založena na ovládací prvek čísla.</span><span class="sxs-lookup"><span data-stu-id="50fc8-183">Business continuity for EDI EDIFACT documents is based on control numbers.</span></span>

<span data-ttu-id="50fc8-184">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="50fc8-184">**Prerequisites**</span></span>

<span data-ttu-id="50fc8-185">Pokud chcete povolit obnovení po havárii pro příchozí zprávy, vyberte v nastavení přijímat smlouvy EDIFACT duplicitní zkontrolujte nastavení.</span><span class="sxs-lookup"><span data-stu-id="50fc8-185">To enable disaster recovery for inbound messages, select the duplicate check settings in your EDIFACT agreement's Receive Settings.</span></span>

![Vyberte nastavení duplicitní kontroly](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. <span data-ttu-id="50fc8-187">Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-187">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in a secondary region.</span></span>    

2. <span data-ttu-id="50fc8-188">Hledání **EDIFACT**a vyberte **EDIFACT - při změně číslo řízení**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-188">Search on **EDIFACT**, and select **EDIFACT - When a control number is modified**.</span></span>

   ![Vyhledejte EDIFACT](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   <span data-ttu-id="50fc8-190">Aktivační událost zobrazí výzvu k navázání připojení k účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="50fc8-190">The trigger prompts you to establish a connection to an integration account.</span></span> 
   <span data-ttu-id="50fc8-191">Aktivační události by měly být připojené k primární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-191">The trigger should be connected to a primary region integration account.</span></span> 

3. <span data-ttu-id="50fc8-192">Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* ze seznamu a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-192">Enter a connection name, select your *primary region integration account* from the list, and choose **Create**.</span></span>    

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. <span data-ttu-id="50fc8-194">**Datum a čas k řízení číslo synchronizaci spustit** nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="50fc8-194">The **DateTime to start control number sync** setting is optional.</span></span> <span data-ttu-id="50fc8-195">**Frekvence** může být nastaven na **den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-195">The **Frequency** can be set to **Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>    

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. <span data-ttu-id="50fc8-197">Vyberte **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-197">Select **New step** > **Add an action**.</span></span>    

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. <span data-ttu-id="50fc8-199">Hledání **EDIFACT**a vyberte **EDIFACT - přidáte nebo aktualizujete řízení čísla**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-199">Search on **EDIFACT**, and select **EDIFACT - Add or update control numbers**.</span></span>   

   ![Přidat nebo aktualizovat čísla ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. <span data-ttu-id="50fc8-201">Chcete-li akce připojení k účtu integrace sekundární oblasti, vyberte **změnit připojení** > **přidat nové připojení** seznam účtů k dispozici integrace.</span><span class="sxs-lookup"><span data-stu-id="50fc8-201">To connect an action to a secondary region integration account, select **Change connection** > **Add new connection** for a list of the available integration accounts.</span></span> <span data-ttu-id="50fc8-202">Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* ze seznamu a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-202">Enter a connection name, select your *secondary region integration account* from the list, and choose **Create**.</span></span>

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. <span data-ttu-id="50fc8-204">Přepněte na nezpracovaná vstupy kliknutím na ikonu v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-204">Switch to raw inputs by clicking on the icon in upper right corner.</span></span>

   ![Přepnout na nezpracovaná vstupy](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. <span data-ttu-id="50fc8-206">Vybrat text z dynamického obsahu výběr a uložte aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="50fc8-206">Select Body from the dynamic content picker, and save the logic app.</span></span>   

   ![Dynamická pole obsahu](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   <span data-ttu-id="50fc8-208">Založený na časový interval, aktivační události dotazuje primární oblasti přijatých řízení číslo v tabulce a vrátí nové záznamy.</span><span class="sxs-lookup"><span data-stu-id="50fc8-208">Based on the time interval, the trigger polls the primary region received control number table and pulls the new records.</span></span>
   <span data-ttu-id="50fc8-209">Akce aktualizace záznamy k účtu integrace sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="50fc8-209">The action updates the records to the secondary region integration account.</span></span> 
   <span data-ttu-id="50fc8-210">Pokud nejsou dostupné žádné aktualizace, stav aktivační události zobrazuje jako **vynecháno**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-210">If there are no updates, the trigger status appears as **Skipped**.</span></span>

   ![Číslo tabulky ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

<span data-ttu-id="50fc8-212">Podle toho, časový interval, přírůstkové běhový stav replikuje z primární oblasti v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-212">Based on the time interval, the incremental runtime status replicates from a primary region to a secondary region.</span></span> <span data-ttu-id="50fc8-213">Během události po havárii, když primární oblasti není k dispozici, přímé přenosy sekundární oblast pro kontinuitu podnikových procesů.</span><span class="sxs-lookup"><span data-stu-id="50fc8-213">During a disaster event, when the primary region is not available, direct traffic to the secondary region for business continuity.</span></span> 

## <a name="as2"></a><span data-ttu-id="50fc8-214">AS2</span><span class="sxs-lookup"><span data-stu-id="50fc8-214">AS2</span></span> 

<span data-ttu-id="50fc8-215">Kontinuita podnikových procesů pro dokumenty, které používají protokol AS2 podle ID zprávy a hodnota povinná kontrola úrovně Důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-215">Business continuity for documents that use the AS2 protocol is based on the message ID and the MIC value.</span></span>

> [!TIP]
> <span data-ttu-id="50fc8-216">Můžete také [šablony rychlý start AS2](https://github.com/Azure/azure-quickstart-templates/pull/3302) k vytvoření aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="50fc8-216">You can also use the [AS2 quick start template](https://github.com/Azure/azure-quickstart-templates/pull/3302) to create logic apps.</span></span> <span data-ttu-id="50fc8-217">Vytváření primární a sekundární integrace účty jsou nezbytné požadavky pro použití šablony.</span><span class="sxs-lookup"><span data-stu-id="50fc8-217">Creating primary and secondary integration accounts are prerequisites to use the template.</span></span> <span data-ttu-id="50fc8-218">Šablona pomůže vytvořit aplikaci logiky, který má aktivační události a akce.</span><span class="sxs-lookup"><span data-stu-id="50fc8-218">The template helps create a logic app that has a trigger and an action.</span></span> <span data-ttu-id="50fc8-219">Aplikace logiky vytvoří připojení z aktivační událost primární integrace účtu a akci, která má účet sekundární integrace.</span><span class="sxs-lookup"><span data-stu-id="50fc8-219">The logic app creates a connection from a trigger to a primary integration account and an action to a secondary integration account.</span></span>

1. <span data-ttu-id="50fc8-220">Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="50fc8-220">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in the secondary region.</span></span>  

2. <span data-ttu-id="50fc8-221">Hledání **AS2**a vyberte **AS2 - hodnota při povinná kontrola úrovně Důvěryhodnosti je vytvořena**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-221">Search on **AS2**, and select **AS2 - When a MIC value is created**.</span></span>   

   ![Vyhledejte AS2](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   <span data-ttu-id="50fc8-223">Aktivační událost zobrazí výzvu k navázání připojení k účtu integrace.</span><span class="sxs-lookup"><span data-stu-id="50fc8-223">A trigger prompts you to establish a connection to an integration account.</span></span> 
   <span data-ttu-id="50fc8-224">Aktivační události by měly být připojené k primární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-224">The trigger should be connected to a primary region integration account.</span></span> 
   
3. <span data-ttu-id="50fc8-225">Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* ze seznamu a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-225">Enter a connection name, select your *primary region integration account* from the list, and choose **Create**.</span></span>

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. <span data-ttu-id="50fc8-227">**Datum a čas k spuštění synchronizace hodnota povinná kontrola úrovně Důvěryhodnosti** nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="50fc8-227">The **DateTime to start MIC value sync** setting is optional.</span></span> <span data-ttu-id="50fc8-228">**Frekvence** může být nastaven na **den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-228">The **Frequency** can be set to **Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>   

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. <span data-ttu-id="50fc8-230">Vyberte **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-230">Select **New step** > **Add an action**.</span></span>  

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. <span data-ttu-id="50fc8-232">Hledání **AS2**a vyberte **AS2 - přidáte nebo aktualizujete obsah povinná kontrola úrovně Důvěryhodnosti**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-232">Search on **AS2**, and select **AS2 - Add or update MIC contents**.</span></span>  

   ![Povinná kontrola úrovně Důvěryhodnosti přidání nebo aktualizace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. <span data-ttu-id="50fc8-234">Pokud chcete připojit k sekundární integrace účet akce, vyberte **změnit připojení** > **přidat nové připojení** seznam účtů k dispozici integrace.</span><span class="sxs-lookup"><span data-stu-id="50fc8-234">To connect an action to a secondary integration account, select **Change connection** > **Add new connection** for a list of the available integration accounts.</span></span> <span data-ttu-id="50fc8-235">Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* ze seznamu a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-235">Enter a connection name, select your *secondary region integration account* from the list, and choose **Create**.</span></span>

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. <span data-ttu-id="50fc8-237">Přepněte na nezpracovaná vstupy kliknutím na ikonu v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="50fc8-237">Switch to raw inputs by clicking on the icon in upper right corner.</span></span>

   ![Přepnout na nezpracovaná vstupy](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. <span data-ttu-id="50fc8-239">Vybrat text z dynamického obsahu výběr a uložte aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="50fc8-239">Select Body from the dynamic content picker, and save the logic app.</span></span>   

   ![Dynamický obsah](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   <span data-ttu-id="50fc8-241">Založený na časový interval, aktivační události dotazuje primární oblasti tabulku a vrátí nové záznamy.</span><span class="sxs-lookup"><span data-stu-id="50fc8-241">Based on the time interval, the trigger polls the primary region table and pulls the new records.</span></span> <span data-ttu-id="50fc8-242">Akce aktualizace je účet integrace sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="50fc8-242">The action updates them to the secondary region integration account.</span></span> 
   <span data-ttu-id="50fc8-243">Pokud nejsou dostupné žádné aktualizace, stav aktivační události zobrazuje jako **vynecháno**.</span><span class="sxs-lookup"><span data-stu-id="50fc8-243">If there are no updates, the trigger status appears as **Skipped**.</span></span>  

   ![Primární oblasti tabulku](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

<span data-ttu-id="50fc8-245">Podle toho, časový interval, přírůstkové běhový stav replikuje z primární oblasti sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="50fc8-245">Based on the time interval, the incremental runtime status replicates from the primary region to the secondary region.</span></span> <span data-ttu-id="50fc8-246">Během události po havárii, když primární oblasti není k dispozici, přímé přenosy sekundární oblast pro kontinuitu podnikových procesů.</span><span class="sxs-lookup"><span data-stu-id="50fc8-246">During a disaster event, when the primary region is not available, direct traffic to the secondary region for business continuity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="50fc8-247">Další kroky</span><span class="sxs-lookup"><span data-stu-id="50fc8-247">Next steps</span></span>

[<span data-ttu-id="50fc8-248">Monitorování zpráv B2B</span><span class="sxs-lookup"><span data-stu-id="50fc8-248">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md)

