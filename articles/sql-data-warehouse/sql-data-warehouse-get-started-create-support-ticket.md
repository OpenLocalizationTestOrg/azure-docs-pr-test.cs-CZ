---
title: "Jak vytvořit lístek podpory pro SQL Data Warehouse | Dokumentace Microsoftu"
description: "Jak vytvořit lístek podpory v Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 058ff1229acee5d03db7c0305c5565ae95a85758
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a><span data-ttu-id="e99e9-103">Jak vytvořit lístek podpory pro SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="e99e9-103">How to create a support ticket for SQL Data Warehouse</span></span>
<span data-ttu-id="e99e9-104">Pokud máte se službou SQL Data Warehouse nějaké problémy, můžete si vytvořit lístek podpory, aby vám mohl náš technický tým pomoct.</span><span class="sxs-lookup"><span data-stu-id="e99e9-104">If you are having any issues with your SQL Data Warehouse, please create a support ticket so that our engineering team can assist you.</span></span>

> [!NOTE] 
> <span data-ttu-id="e99e9-105">Od 20. 12. 2016 je kontrola stavu prostředků na webu Azure Portal nepřesná.</span><span class="sxs-lookup"><span data-stu-id="e99e9-105">As of 12/20/2016, the resource health check in the Azure portal is not accurate.</span></span> <span data-ttu-id="e99e9-106">Na vyřešení tohoto problému aktivně pracujeme.</span><span class="sxs-lookup"><span data-stu-id="e99e9-106">We are actively working to fix this issue.</span></span> 


## <a name="create-a-support-ticket"></a><span data-ttu-id="e99e9-107">Vytvoření lístku podpory</span><span class="sxs-lookup"><span data-stu-id="e99e9-107">Create a support ticket</span></span>
1. <span data-ttu-id="e99e9-108">Otevřete [Azure Portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="e99e9-108">Open the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="e99e9-109">Na domovské obrazovce klikněte na dlaždici **Nápovědy a podpora**.</span><span class="sxs-lookup"><span data-stu-id="e99e9-109">On the Home screen, click the **Help + support** tile.</span></span>
   
    ![Nápověda a podpora](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. <span data-ttu-id="e99e9-111">V okně Nápověda a podpora klikněte na **Vytvořit žádost o podporu**.</span><span class="sxs-lookup"><span data-stu-id="e99e9-111">On the Help + Support blade, click **Create support request**.</span></span>
   
    ![Nová žádost o podporu](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. <span data-ttu-id="e99e9-113">Vyberte **Typ žádosti**.</span><span class="sxs-lookup"><span data-stu-id="e99e9-113">Select the **Request Type**.</span></span>
   
    ![Typ žádosti](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > <span data-ttu-id="e99e9-115">Ve výchozím nastavení obsahuje každý server SQL (např. myserver.database.windows.net) **kvóty DTU** o hodnotě 45 000.</span><span class="sxs-lookup"><span data-stu-id="e99e9-115">By default, each SQL server (e.g. myserver.database.windows.net) has a **DTU Quota** of 45,000.</span></span> <span data-ttu-id="e99e9-116">Tato kvóta je jednoduše bezpečnostní omezení.</span><span class="sxs-lookup"><span data-stu-id="e99e9-116">This quota is simply a safety limit.</span></span> <span data-ttu-id="e99e9-117">Kvótu můžete zvýšit vytvořením lístku podpory a výběrem *kvóty* jako typu požadavku.</span><span class="sxs-lookup"><span data-stu-id="e99e9-117">You can increase your quota by creating a support ticket and selecting *Quota* as the request type.</span></span> <span data-ttu-id="e99e9-118">Chcete-li vypočítat potřebné DTU, vynásobte celkové potřebné [DWU][DWU] číslem 7,5.</span><span class="sxs-lookup"><span data-stu-id="e99e9-118">To calculate your DTU needs, multiply the 7.5 by the total [DWU][DWU] needed.</span></span> <span data-ttu-id="e99e9-119">Pokud například chcete hostovat dvě DW6000 na jednom serveru SQL, měli byste požadovat kvóty DTU o hodnotě 90 000.</span><span class="sxs-lookup"><span data-stu-id="e99e9-119">For example, you would like to host two DW6000s on one SQL server, then you should request a DTU quota of 90,000.</span></span>  <span data-ttu-id="e99e9-120">Vaši aktuální spotřebu DTU z okna serveru SQL můžete zobrazit na portálu.</span><span class="sxs-lookup"><span data-stu-id="e99e9-120">You can view your current DTU consumption from the SQL server blade in the portal.</span></span> <span data-ttu-id="e99e9-121">Pozastavené i nepozastavené databáze se započítávají do kvóty DTU.</span><span class="sxs-lookup"><span data-stu-id="e99e9-121">Both paused and un-paused databases count toward the DTU quota.</span></span> 
   > 
   > 
5. <span data-ttu-id="e99e9-122">Vyberte **předplatné**, pod které spadá databáze s problémem, který chcete nahlásit.</span><span class="sxs-lookup"><span data-stu-id="e99e9-122">Select the **Subscription** that hosts the database with the problem you are reporting.</span></span>
   
    ![Předplatné](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. <span data-ttu-id="e99e9-124">Jako prostředek vyberte **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="e99e9-124">Select **SQL Data Warehouse** as the Resource.</span></span>
   
    ![Prostředek](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. <span data-ttu-id="e99e9-126">Vyberte svůj [plán podpory Azure][Azure support plan].</span><span class="sxs-lookup"><span data-stu-id="e99e9-126">Select your [Azure support plan][Azure support plan].</span></span>
   
   * <span data-ttu-id="e99e9-127">**Podpora k fakturaci, správě kvót a předplatného** je k dispozici na všech úrovních podpory.</span><span class="sxs-lookup"><span data-stu-id="e99e9-127">**Billing, quota and subscription management** support is available at all support levels.</span></span>
   * <span data-ttu-id="e99e9-128">Podpora k problémům vyžadujícím **opravu** je poskytována prostřednictvím plánů podpory [Developer Support][Developer], [Standard Support][Standard], [Professional Direct][Professional Direct] nebo [Premier Support][Premier].</span><span class="sxs-lookup"><span data-stu-id="e99e9-128">**Break-fix** support is provided through [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] or [Premier][Premier] support.</span></span> <span data-ttu-id="e99e9-129">Problémy vyžadující opravu jsou problémy, které mají zákazníci při používání Azure a u kterých se dá předpokládat, že je způsobila společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e99e9-129">Break-fix issues are problems experienced by customers while using Azure where there is a reasonable expectation that Microsoft caused the problem.</span></span>
   * <span data-ttu-id="e99e9-130">V rámci podpory na úrovni plánů [Professional Direct][Professional Direct] a [Premier Support][Premier] jsou k dispozici služby **mentorování vývojářů** a **poradenské služby**.</span><span class="sxs-lookup"><span data-stu-id="e99e9-130">**Developer mentoring** and **advisory services** are available at the [Professional Direct][Professional Direct] and [Premier][Premier] support levels.</span></span> 
     
     <span data-ttu-id="e99e9-131">Pokud máte plán podpory Premier Support, můžete také problémy týkající se služby SQL Data Warehouse nahlásit na [online portálu Microsoft Premier][Microsoft Premier online portal].</span><span class="sxs-lookup"><span data-stu-id="e99e9-131">If you have a Premier support plan, you can also report SQL Data Warehouse related issues on the [Microsoft Premier online portal][Microsoft Premier online portal].</span></span>  <span data-ttu-id="e99e9-132">Další informace o různých plánech podpory, včetně rozsahu, doby odezvy, cen atd. najdete v tématu věnovaném [plánům podpory Azure][Azure support plan].  Nejčastější dotazy týkající se podpory Azure najdete v [nejčastějších dotazech týkajících se podpory k Azure][Azure support FAQs].</span><span class="sxs-lookup"><span data-stu-id="e99e9-132">See [Azure support plans][Azure support plan] to learn more about the various support plans, including scope, response times, pricing, etc.  For frequently asked questions about Azure support, see [Azure support FAQs][Azure support FAQs].</span></span>  
     
     ![Plán podpory](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. <span data-ttu-id="e99e9-134">Vyberte **typ problému** a **kategorii**.</span><span class="sxs-lookup"><span data-stu-id="e99e9-134">Select the **Problem Type** and **Category**.</span></span> <span data-ttu-id="e99e9-135">V tomto příkladu jsme vybrali typ problému „Nástroje“ a kategorii „Klientské nástroje“.</span><span class="sxs-lookup"><span data-stu-id="e99e9-135">In this example, we have chosen "Tools" as the Problem type and "Client tools" as the category.</span></span> 
   
    ![Kategorie typu problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. <span data-ttu-id="e99e9-137">Popište problém a zvolte úroveň dopadu na chod firmy.</span><span class="sxs-lookup"><span data-stu-id="e99e9-137">Describe the problem and choose the level of business impact.</span></span>
   
    ![Popis problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. <span data-ttu-id="e99e9-139">Předvyplní se vaše **kontaktní údaje** pro tento lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="e99e9-139">Your **contact information** for this support ticket will be pre-filled.</span></span> <span data-ttu-id="e99e9-140">V případě potřeby je aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="e99e9-140">Update this if necessary.</span></span>
    
    ![Kontaktní údaje](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. <span data-ttu-id="e99e9-142">Kliknutím na **Vytvořit** odešlete žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="e99e9-142">Click **Create** to submit the support request.</span></span>

## <a name="monitor-a-support-ticket"></a><span data-ttu-id="e99e9-143">Monitorování lístku podpory</span><span class="sxs-lookup"><span data-stu-id="e99e9-143">Monitor a support ticket</span></span>
<span data-ttu-id="e99e9-144">Po odeslání žádosti o podporu vás bude kontaktovat tým podpory Azure.</span><span class="sxs-lookup"><span data-stu-id="e99e9-144">After you have submitted the support request, the Azure support team will contact you.</span></span> <span data-ttu-id="e99e9-145">Pokud chcete zkontrolovat stav a podrobnosti žádosti, klikněte na řídicím panelu na **Spravovat žádosti o podporu**.</span><span class="sxs-lookup"><span data-stu-id="e99e9-145">To check your request status and details, click **Manage support requests** on the dashboard.</span></span>

![Zkontrolování stavu](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a><span data-ttu-id="e99e9-147">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="e99e9-147">Other Resources</span></span>
<span data-ttu-id="e99e9-148">Kromě toho se můžete spojit s komunitou SQL Data Warehouse na [Stack Overflow][Stack Overflow] nebo na [fóru pro Azure SQL Data Warehouse na webu MSDN][Azure SQL Data Warehouse MSDN forum].</span><span class="sxs-lookup"><span data-stu-id="e99e9-148">Additionally, you can connect with the SQL Data Warehouse community on [Stack Overflow][Stack Overflow] or on the [Azure SQL Data Warehouse MSDN forum][Azure SQL Data Warehouse MSDN forum].</span></span>

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

