---
title: "obnovení aaaDisaster pro účet Integrace B2B – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: e86564a3c5a2607d22514936c606e2843cba0416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a><span data-ttu-id="e464b-103">Zotavení po havárii mezi oblastmi B2B aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="e464b-103">Logic Apps B2B cross-region disaster recovery</span></span>

<span data-ttu-id="e464b-104">Úlohy B2B zahrnovat peníze transakce jako objednávky a faktury.</span><span class="sxs-lookup"><span data-stu-id="e464b-104">B2B workloads involve money transactions like orders and invoices.</span></span> <span data-ttu-id="e464b-105">Během události po havárii je velmi důležitá pro hello toomeet obnovit tooquickly firmy, které firemní úrovni SLA dohodnutých s jejich partnery.</span><span class="sxs-lookup"><span data-stu-id="e464b-105">During a disaster event, it's critical for a business tooquickly recover toomeet hello business-level SLAs agreed upon with their partners.</span></span> <span data-ttu-id="e464b-106">Tento článek ukazuje, jak toobuild kontinuity podnikových procesů plánu pro úlohy B2B.</span><span class="sxs-lookup"><span data-stu-id="e464b-106">This article demonstrates how toobuild a business continuity plan for B2B workloads.</span></span> 

* <span data-ttu-id="e464b-107">Připravenost obnovení po havárii</span><span class="sxs-lookup"><span data-stu-id="e464b-107">Disaster Recovery readiness</span></span> 
* <span data-ttu-id="e464b-108">Převzetí služeb při selhání toosecondary oblast během události po havárii</span><span class="sxs-lookup"><span data-stu-id="e464b-108">Fail over toosecondary region during a disaster event</span></span> 
* <span data-ttu-id="e464b-109">Vrátit zpět tooprimary oblast po události po havárii</span><span class="sxs-lookup"><span data-stu-id="e464b-109">Fall back tooprimary region after a disaster event</span></span>

## <a name="disaster-recovery-readiness"></a><span data-ttu-id="e464b-110">Připravenost obnovení po havárii</span><span class="sxs-lookup"><span data-stu-id="e464b-110">Disaster Recovery readiness</span></span>  

1. <span data-ttu-id="e464b-111">Určete sekundární oblasti a vytvořte [integrace účet](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) v sekundární oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-111">Identify a secondary region and create an [integration account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) in hello secondary region.</span></span>

2. <span data-ttu-id="e464b-112">Přidání partnery, schémat a smluv pro toky zpráva hello požadované kde hello spustit stav potřebuje toobe replikovat toosecondary oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-112">Add partners, schemas, and agreements for hello required message flows where hello run status needs toobe replicated toosecondary region integration account.</span></span>

   > [!TIP]
   > <span data-ttu-id="e464b-113">Ujistěte se, že konzistence v hello integrace účet artefaktů na zásady vytváření názvů v oblastech.</span><span class="sxs-lookup"><span data-stu-id="e464b-113">Make sure there's consistency in hello integration account artifact's naming convention across regions.</span></span> 

3. <span data-ttu-id="e464b-114">hello toopull spustit stav z hello primární oblasti, vytvoření aplikace logiky v sekundární oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-114">toopull hello run status from hello primary region, create a logic app in hello secondary region.</span></span> 

   <span data-ttu-id="e464b-115">Musí mít tuto aplikaci logiky *aktivační událost* a *akce*.</span><span class="sxs-lookup"><span data-stu-id="e464b-115">This logic app should have a *trigger* and an *action*.</span></span> 
   <span data-ttu-id="e464b-116">aktivační událost Hello by se měly připojit tooprimary oblasti integrace účet a hello akce by se měly připojit toosecondary oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-116">hello trigger should connect tooprimary region integration account, and hello action should connect toosecondary region integration account.</span></span> 
   <span data-ttu-id="e464b-117">Podle hello časový interval, aktivační událost hello dotazování hello primární oblasti spustit stav tabulce a vrátí nové záznamy hello, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="e464b-117">Based on hello time interval, hello trigger polls hello primary region run status table and pulls hello new records, if any.</span></span> <span data-ttu-id="e464b-118">Akce Hello je aktualizuje toosecondary oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-118">hello action updates them toosecondary region integration account.</span></span> 
   <span data-ttu-id="e464b-119">To pomáhá tooget přírůstkové běhový stav z oblasti toosecondary primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-119">This helps tooget incremental runtime status from primary region toosecondary region.</span></span>

4. <span data-ttu-id="e464b-120">Kontinuita podnikových procesů v Logic Apps je integrace účet určený toosupport založené na protokolech B2B - X12 AS2, EDIFACT a.</span><span class="sxs-lookup"><span data-stu-id="e464b-120">Business continuity in Logic Apps integration account is designed toosupport based on B2B protocols - X12, AS2, and EDIFACT.</span></span> <span data-ttu-id="e464b-121">toofind podrobné kroky, vyberte hello příslušné odkazy.</span><span class="sxs-lookup"><span data-stu-id="e464b-121">toofind detailed steps, select hello respective links.</span></span>

5. <span data-ttu-id="e464b-122">Hello doporučení je toodeploy všechny prostředky primární oblasti v sekundární oblasti příliš.</span><span class="sxs-lookup"><span data-stu-id="e464b-122">hello recommendation is toodeploy all primary region resources in a secondary region too.</span></span> 

   <span data-ttu-id="e464b-123">Primární oblasti prostředky zahrnují Azure SQL Database nebo Azure Cosmos DB, Azure Service Bus a Azure Event Hubs použít pro zasílání zpráv Azure API Management a funkce hello Azure Logic Apps v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e464b-123">Primary region resources include Azure SQL Database or Azure Cosmos DB, Azure Service Bus and Azure Event Hubs used for messaging, Azure API Management, and hello Azure Logic Apps feature in Azure App Service.</span></span>   

6. <span data-ttu-id="e464b-124">Připojení z primární oblasti tooa sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-124">Establish a connection from a primary region tooa secondary region.</span></span> <span data-ttu-id="e464b-125">hello toopull spustit stav od primární oblasti, vytvoření aplikace logiky v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-125">toopull hello run status from a primary region, create a logic app in a secondary region.</span></span> 

   <span data-ttu-id="e464b-126">aplikace logiky Hello by měl mít aktivační události a akce.</span><span class="sxs-lookup"><span data-stu-id="e464b-126">hello logic app should have a trigger and an action.</span></span> 
   <span data-ttu-id="e464b-127">aktivační událost Hello by měl připojit tooa primární oblasti integrace účet.</span><span class="sxs-lookup"><span data-stu-id="e464b-127">hello trigger should connect tooa primary region integration account.</span></span> 
   <span data-ttu-id="e464b-128">Hello akce by měl připojit tooa sekundární oblasti integrace účet.</span><span class="sxs-lookup"><span data-stu-id="e464b-128">hello action should connect tooa secondary region integration account.</span></span> 
   <span data-ttu-id="e464b-129">Podle hello časový interval, aktivační událost hello dotazování hello primární oblasti spustit stav tabulce a vrátí nové záznamy hello, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="e464b-129">Based on hello time interval, hello trigger polls hello primary region run status table and pulls hello new records, if any.</span></span> 
   <span data-ttu-id="e464b-130">Akce Hello je aktualizuje tooa sekundární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-130">hello action updates them tooa secondary region integration account.</span></span> 
   <span data-ttu-id="e464b-131">Tento proces pomáhá tooget přírůstkové běhový stav ze sekundární oblasti toohello hello primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-131">This process helps tooget incremental runtime status from hello primary region toohello secondary region.</span></span>

<span data-ttu-id="e464b-132">Kontinuita podnikových procesů v účtu integrace Logic Apps poskytuje podporu na základě hello B2B protokoly X12, AS2 a EDIFACT.</span><span class="sxs-lookup"><span data-stu-id="e464b-132">Business continuity in a Logic Apps integration account provides support based on hello B2B protocols X12, AS2, and EDIFACT.</span></span> <span data-ttu-id="e464b-133">Podrobné pokyny týkající se použití X12 a AS2 najdete v tématu [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) a [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e464b-133">For detailed steps on using X12 and AS2, see [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) and [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) in this article.</span></span>

## <a name="fail-over-tooa-secondary-region-during-a-disaster-event"></a><span data-ttu-id="e464b-134">Převzetí služeb při selhání tooa sekundární oblasti během události po havárii</span><span class="sxs-lookup"><span data-stu-id="e464b-134">Fail over tooa secondary region during a disaster event</span></span>

<span data-ttu-id="e464b-135">Během události po havárii, když primární oblasti hello není k dispozici pro kontinuitu podnikových procesů, přímé přenosy toohello sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="e464b-135">During a disaster event, when hello primary region is not available for business continuity, direct traffic toohello secondary region.</span></span> <span data-ttu-id="e464b-136">Obchodní toorecover funkce rychle toomeet hello RPO/RTO dohodnutých jejich partnery pomáhá sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="e464b-136">A secondary region helps a business toorecover functions quickly toomeet hello RPO/RTO agreed upon by their partners.</span></span> <span data-ttu-id="e464b-137">Minimalizuje také úsilí toofail přes z jedné oblasti tooanother oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-137">It also minimizes efforts toofail over from one region tooanother region.</span></span> 

<span data-ttu-id="e464b-138">Při kopírování řízení čísla od primární oblasti sekundární oblasti tooa očekávané latence neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e464b-138">There is an expected latency while copying control numbers from a primary region tooa secondary region.</span></span> <span data-ttu-id="e464b-139">tooavoid odesílání duplicitní generovaného řízení čísla toopartners během události po havárii, hello doporučení je tooincrement hello řízení čísla ve smlouvách sekundární oblasti hello pomocí [rutiny prostředí PowerShell](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="e464b-139">tooavoid sending duplicate generated control numbers toopartners during a disaster event, hello recommendation is tooincrement hello control numbers in hello secondary region agreements by using [PowerShell cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span></span>

## <a name="fall-back-tooa-primary-region-post-disaster-event"></a><span data-ttu-id="e464b-140">Vrátit zpět tooa primární oblasti po havárii událostí</span><span class="sxs-lookup"><span data-stu-id="e464b-140">Fall back tooa primary region post-disaster event</span></span>

<span data-ttu-id="e464b-141">toofall back tooa primární oblasti případě, že je k dispozici, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e464b-141">toofall back tooa primary region when it is available, follow these steps:</span></span>

1. <span data-ttu-id="e464b-142">Zastavte příjem zpráv od partnerů v sekundární oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-142">Stop accepting messages from partners in hello secondary region.</span></span>  

2. <span data-ttu-id="e464b-143">Zvýšit číslo řízení hello generované pro všechny smlouvy primární oblasti hello pomocí [rutiny prostředí PowerShell](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="e464b-143">Increment hello generated control numbers for all hello primary region agreements by using [PowerShell cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span></span>  

3. <span data-ttu-id="e464b-144">Přímý přenos dat z primární oblasti toohello hello sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="e464b-144">Direct traffic from hello secondary region toohello primary region.</span></span>

4. <span data-ttu-id="e464b-145">Zkontrolujte, zda je povoleno aplikaci logiky hello vytvořen v sekundární oblasti hello pro stahování, spusťte stavu od primární oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-145">Check that hello logic app created in hello secondary region for pulling run status from hello primary region is enabled.</span></span>

## <a name="x12"></a><span data-ttu-id="e464b-146">X12</span><span class="sxs-lookup"><span data-stu-id="e464b-146">X12</span></span> 

<span data-ttu-id="e464b-147">Kontinuita podnikových procesů pro EDI X 12 dokumentů je založena na ovládací prvek čísla:</span><span class="sxs-lookup"><span data-stu-id="e464b-147">Business continuity for EDI X12 documents is based on control numbers:</span></span>

> [!TIP]
> <span data-ttu-id="e464b-148">Můžete taky hello [X12 rychlý start šablony](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="e464b-148">You can also use hello [X12 quick start template](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logic apps.</span></span> <span data-ttu-id="e464b-149">Vytváření primární a sekundární integrace účty jsou požadavky toouse hello šablony.</span><span class="sxs-lookup"><span data-stu-id="e464b-149">Creating primary and secondary integration accounts are prerequisites toouse hello template.</span></span> <span data-ttu-id="e464b-150">Hello šablony pomáhá toocreate dvě aplikace logiky, jednu pro čísla přijaté řízení a druhou pro generovaný řízení čísla.</span><span class="sxs-lookup"><span data-stu-id="e464b-150">hello template helps toocreate two logic apps, one for received control numbers and another for generated control numbers.</span></span> <span data-ttu-id="e464b-151">Příslušné triggery a akce se vytvoří v hello aplikace logiky, připojení hello aktivační událost toohello primární integrace účet a hello akce toohello sekundární integrace účet.</span><span class="sxs-lookup"><span data-stu-id="e464b-151">Respective triggers and actions are created in hello logic apps, connecting hello trigger toohello primary integration account and hello action toohello secondary integration account.</span></span>

<span data-ttu-id="e464b-152">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="e464b-152">**Prerequisites**</span></span>

<span data-ttu-id="e464b-153">tooenable zotavení po havárii pro příchozí zprávy, vyberte v nastavení přijímat hello X12 smlouvy hello duplicitní zkontrolujte nastavení.</span><span class="sxs-lookup"><span data-stu-id="e464b-153">tooenable disaster recovery for inbound messages, select hello duplicate check settings in hello X12 agreement's Receive Settings.</span></span>

![Vyberte nastavení duplicitní kontroly](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. <span data-ttu-id="e464b-155">Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-155">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in a secondary region.</span></span>    

2. <span data-ttu-id="e464b-156">Hledání **X12**a vyberte **X12-při změně číslo řízení**.</span><span class="sxs-lookup"><span data-stu-id="e464b-156">Search on **X12**, and select **X12 - When a control number is modified**.</span></span>   

   ![Vyhledejte X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   <span data-ttu-id="e464b-158">aktivační událost Hello vyzve k zadání tooestablish účet připojení tooan integrace.</span><span class="sxs-lookup"><span data-stu-id="e464b-158">hello trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="e464b-159">aktivační událost Hello by měl být připojený tooa primární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-159">hello trigger should be connected tooa primary region integration account.</span></span>

3. <span data-ttu-id="e464b-160">Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e464b-160">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>   

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. <span data-ttu-id="e464b-162">Hello **data a času toostart řízení číslo synchronizace** nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="e464b-162">hello **DateTime toostart control number sync** setting is optional.</span></span> <span data-ttu-id="e464b-163">Hello **frekvence** lze nastavit příliš**den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.</span><span class="sxs-lookup"><span data-stu-id="e464b-163">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>   

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. <span data-ttu-id="e464b-165">Vyberte **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="e464b-165">Select **New step** > **Add an action**.</span></span>

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. <span data-ttu-id="e464b-167">Hledání **X12**a vyberte **X12-přidáte nebo aktualizujete řízení čísla**.</span><span class="sxs-lookup"><span data-stu-id="e464b-167">Search on **X12**, and select **X12 - Add or update control numbers**.</span></span>   

   ![Přidat nebo aktualizovat čísla ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. <span data-ttu-id="e464b-169">Vyberte účet akce tooa sekundární oblasti integrace tooconnect **změnit připojení** > **přidat nové připojení** pro seznam účtů, k dispozici integrace hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-169">tooconnect an action tooa secondary region integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="e464b-170">Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e464b-170">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span> 

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. <span data-ttu-id="e464b-172">Kliknutím na ikonu hello v pravém horním rohu přepínače tooraw vstupy.</span><span class="sxs-lookup"><span data-stu-id="e464b-172">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Vstupy tooraw přepínače](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. <span data-ttu-id="e464b-174">Vyberte textu v hello dynamického obsahu pro výběr a uložte aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-174">Select Body from hello dynamic content picker, and save hello logic app.</span></span>

   ![Dynamická pole obsahu](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   <span data-ttu-id="e464b-176">Podle hello časový interval, aktivační události hello dotazování hello primární oblasti přijatých řízení číslo tabulky a vrátí nové záznamy hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-176">Based on hello time interval, hello trigger polls hello primary region received control number table and pulls hello new records.</span></span> 
   <span data-ttu-id="e464b-177">Akce Hello aktualizuje hello záznamy v hello sekundární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-177">hello action updates hello records in hello secondary region integration account.</span></span> 
   <span data-ttu-id="e464b-178">Pokud nejsou dostupné žádné aktualizace, stav hello aktivační události zobrazuje jako **vynecháno**.</span><span class="sxs-lookup"><span data-stu-id="e464b-178">If there are no updates, hello trigger status appears as **Skipped**.</span></span>   

   ![Číslo tabulky ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

<span data-ttu-id="e464b-180">Podle hello časový interval, hello přírůstkové běhový stav replikuje z sekundární oblasti tooa primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-180">Based on hello time interval, hello incremental runtime status replicates from a primary region tooa secondary region.</span></span> <span data-ttu-id="e464b-181">Během události po havárii, když primární oblasti hello není k dispozici, přímé přenosy toohello sekundární oblast pro kontinuitu podnikových procesů.</span><span class="sxs-lookup"><span data-stu-id="e464b-181">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="edifact"></a><span data-ttu-id="e464b-182">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="e464b-182">EDIFACT</span></span> 

<span data-ttu-id="e464b-183">Kontinuita podnikových procesů pro dokumenty EDI EDIFACT je založena na ovládací prvek čísla.</span><span class="sxs-lookup"><span data-stu-id="e464b-183">Business continuity for EDI EDIFACT documents is based on control numbers.</span></span>

<span data-ttu-id="e464b-184">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="e464b-184">**Prerequisites**</span></span>

<span data-ttu-id="e464b-185">tooenable zotavení po havárii pro příchozí zprávy, vyberte v nastavení přijímat smlouvy EDIFACT hello duplicitní zkontrolujte nastavení.</span><span class="sxs-lookup"><span data-stu-id="e464b-185">tooenable disaster recovery for inbound messages, select hello duplicate check settings in your EDIFACT agreement's Receive Settings.</span></span>

![Vyberte nastavení duplicitní kontroly](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. <span data-ttu-id="e464b-187">Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-187">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in a secondary region.</span></span>    

2. <span data-ttu-id="e464b-188">Hledání **EDIFACT**a vyberte **EDIFACT - při změně číslo řízení**.</span><span class="sxs-lookup"><span data-stu-id="e464b-188">Search on **EDIFACT**, and select **EDIFACT - When a control number is modified**.</span></span>

   ![Vyhledejte EDIFACT](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   <span data-ttu-id="e464b-190">aktivační událost Hello vyzve k zadání tooestablish účet připojení tooan integrace.</span><span class="sxs-lookup"><span data-stu-id="e464b-190">hello trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="e464b-191">aktivační událost Hello by měl být připojený tooa primární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-191">hello trigger should be connected tooa primary region integration account.</span></span> 

3. <span data-ttu-id="e464b-192">Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e464b-192">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>    

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. <span data-ttu-id="e464b-194">Hello **data a času toostart řízení číslo synchronizace** nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="e464b-194">hello **DateTime toostart control number sync** setting is optional.</span></span> <span data-ttu-id="e464b-195">Hello **frekvence** lze nastavit příliš**den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.</span><span class="sxs-lookup"><span data-stu-id="e464b-195">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>    

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. <span data-ttu-id="e464b-197">Vyberte **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="e464b-197">Select **New step** > **Add an action**.</span></span>    

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. <span data-ttu-id="e464b-199">Hledání **EDIFACT**a vyberte **EDIFACT - přidáte nebo aktualizujete řízení čísla**.</span><span class="sxs-lookup"><span data-stu-id="e464b-199">Search on **EDIFACT**, and select **EDIFACT - Add or update control numbers**.</span></span>   

   ![Přidat nebo aktualizovat čísla ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. <span data-ttu-id="e464b-201">Vyberte účet akce tooa sekundární oblasti integrace tooconnect **změnit připojení** > **přidat nové připojení** pro seznam účtů, k dispozici integrace hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-201">tooconnect an action tooa secondary region integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="e464b-202">Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e464b-202">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span>

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. <span data-ttu-id="e464b-204">Kliknutím na ikonu hello v pravém horním rohu přepínače tooraw vstupy.</span><span class="sxs-lookup"><span data-stu-id="e464b-204">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Vstupy tooraw přepínače](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. <span data-ttu-id="e464b-206">Vyberte textu v hello dynamického obsahu pro výběr a uložte aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-206">Select Body from hello dynamic content picker, and save hello logic app.</span></span>   

   ![Dynamická pole obsahu](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   <span data-ttu-id="e464b-208">Podle hello časový interval, aktivační události hello dotazování hello primární oblasti přijatých řízení číslo tabulky a vrátí nové záznamy hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-208">Based on hello time interval, hello trigger polls hello primary region received control number table and pulls hello new records.</span></span>
   <span data-ttu-id="e464b-209">Akce Hello aktualizuje hello záznamy toohello sekundární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-209">hello action updates hello records toohello secondary region integration account.</span></span> 
   <span data-ttu-id="e464b-210">Pokud nejsou dostupné žádné aktualizace, stav hello aktivační události zobrazuje jako **vynecháno**.</span><span class="sxs-lookup"><span data-stu-id="e464b-210">If there are no updates, hello trigger status appears as **Skipped**.</span></span>

   ![Číslo tabulky ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

<span data-ttu-id="e464b-212">Podle hello časový interval, hello přírůstkové běhový stav replikuje z sekundární oblasti tooa primární oblasti.</span><span class="sxs-lookup"><span data-stu-id="e464b-212">Based on hello time interval, hello incremental runtime status replicates from a primary region tooa secondary region.</span></span> <span data-ttu-id="e464b-213">Během události po havárii, když primární oblasti hello není k dispozici, přímé přenosy toohello sekundární oblast pro kontinuitu podnikových procesů.</span><span class="sxs-lookup"><span data-stu-id="e464b-213">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="as2"></a><span data-ttu-id="e464b-214">AS2</span><span class="sxs-lookup"><span data-stu-id="e464b-214">AS2</span></span> 

<span data-ttu-id="e464b-215">Kontinuita podnikových procesů pro dokumenty, které používají protokol AS2 hello je na základě ID zprávy hello a hello povinná kontrola úrovně Důvěryhodnosti hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e464b-215">Business continuity for documents that use hello AS2 protocol is based on hello message ID and hello MIC value.</span></span>

> [!TIP]
> <span data-ttu-id="e464b-216">Můžete taky hello [šablony rychlý start AS2](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="e464b-216">You can also use hello [AS2 quick start template](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logic apps.</span></span> <span data-ttu-id="e464b-217">Vytváření primární a sekundární integrace účty jsou požadavky toouse hello šablony.</span><span class="sxs-lookup"><span data-stu-id="e464b-217">Creating primary and secondary integration accounts are prerequisites toouse hello template.</span></span> <span data-ttu-id="e464b-218">Šablona Hello pomáhá vytvářet aplikace logiky, který má aktivační události a akce.</span><span class="sxs-lookup"><span data-stu-id="e464b-218">hello template helps create a logic app that has a trigger and an action.</span></span> <span data-ttu-id="e464b-219">aplikace logiky Hello vytvoří připojení z účet primární integrace tooa aktivace a účet akce tooa sekundární integrace.</span><span class="sxs-lookup"><span data-stu-id="e464b-219">hello logic app creates a connection from a trigger tooa primary integration account and an action tooa secondary integration account.</span></span>

1. <span data-ttu-id="e464b-220">Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-220">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in hello secondary region.</span></span>  

2. <span data-ttu-id="e464b-221">Hledání **AS2**a vyberte **AS2 - hodnota při povinná kontrola úrovně Důvěryhodnosti je vytvořena**.</span><span class="sxs-lookup"><span data-stu-id="e464b-221">Search on **AS2**, and select **AS2 - When a MIC value is created**.</span></span>   

   ![Vyhledejte AS2](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   <span data-ttu-id="e464b-223">Aktivační událost vyzve tooestablish účet připojení tooan integrace.</span><span class="sxs-lookup"><span data-stu-id="e464b-223">A trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="e464b-224">aktivační událost Hello by měl být připojený tooa primární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-224">hello trigger should be connected tooa primary region integration account.</span></span> 
   
3. <span data-ttu-id="e464b-225">Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e464b-225">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. <span data-ttu-id="e464b-227">Hello **synchronizace hodnotu data a času toostart povinná kontrola úrovně Důvěryhodnosti** nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="e464b-227">hello **DateTime toostart MIC value sync** setting is optional.</span></span> <span data-ttu-id="e464b-228">Hello **frekvence** lze nastavit příliš**den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.</span><span class="sxs-lookup"><span data-stu-id="e464b-228">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>   

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. <span data-ttu-id="e464b-230">Vyberte **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="e464b-230">Select **New step** > **Add an action**.</span></span>  

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. <span data-ttu-id="e464b-232">Hledání **AS2**a vyberte **AS2 - přidáte nebo aktualizujete obsah povinná kontrola úrovně Důvěryhodnosti**.</span><span class="sxs-lookup"><span data-stu-id="e464b-232">Search on **AS2**, and select **AS2 - Add or update MIC contents**.</span></span>  

   ![Povinná kontrola úrovně Důvěryhodnosti přidání nebo aktualizace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. <span data-ttu-id="e464b-234">Vyberte účet akce tooa sekundární integrace tooconnect **změnit připojení** > **přidat nové připojení** pro seznam účtů, k dispozici integrace hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-234">tooconnect an action tooa secondary integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="e464b-235">Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e464b-235">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span>

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. <span data-ttu-id="e464b-237">Kliknutím na ikonu hello v pravém horním rohu přepínače tooraw vstupy.</span><span class="sxs-lookup"><span data-stu-id="e464b-237">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Vstupy tooraw přepínače](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. <span data-ttu-id="e464b-239">Vyberte textu v hello dynamického obsahu pro výběr a uložte aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-239">Select Body from hello dynamic content picker, and save hello logic app.</span></span>   

   ![Dynamický obsah](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   <span data-ttu-id="e464b-241">Podle hello časový interval, aktivační události hello dotazování hello primární oblasti tabulku a vrátí nové záznamy hello.</span><span class="sxs-lookup"><span data-stu-id="e464b-241">Based on hello time interval, hello trigger polls hello primary region table and pulls hello new records.</span></span> <span data-ttu-id="e464b-242">Akce Hello je aktualizuje toohello sekundární oblasti integrace účtu.</span><span class="sxs-lookup"><span data-stu-id="e464b-242">hello action updates them toohello secondary region integration account.</span></span> 
   <span data-ttu-id="e464b-243">Pokud nejsou dostupné žádné aktualizace, stav hello aktivační události zobrazuje jako **vynecháno**.</span><span class="sxs-lookup"><span data-stu-id="e464b-243">If there are no updates, hello trigger status appears as **Skipped**.</span></span>  

   ![Primární oblasti tabulku](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

<span data-ttu-id="e464b-245">Podle hello časový interval, hello přírůstkové běhový stav replikuje z hello primární oblasti toohello sekundární oblast.</span><span class="sxs-lookup"><span data-stu-id="e464b-245">Based on hello time interval, hello incremental runtime status replicates from hello primary region toohello secondary region.</span></span> <span data-ttu-id="e464b-246">Během události po havárii, když primární oblasti hello není k dispozici, přímé přenosy toohello sekundární oblast pro kontinuitu podnikových procesů.</span><span class="sxs-lookup"><span data-stu-id="e464b-246">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e464b-247">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e464b-247">Next steps</span></span>

[<span data-ttu-id="e464b-248">Monitorování zpráv B2B</span><span class="sxs-lookup"><span data-stu-id="e464b-248">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md)

