---
title: "aaaHow toocreate lístek podpory pro SQL Data Warehouse | Microsoft Docs"
description: "Jak toocreate podporu lístek v Azure SQL Data Warehouse."
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
ms.openlocfilehash: 72f7eac82112fb7f1bfb05abca4ce40aeb3c828c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-support-ticket-for-sql-data-warehouse"></a><span data-ttu-id="eee72-103">Jak toocreate podporu lístek pro datový sklad SQL</span><span class="sxs-lookup"><span data-stu-id="eee72-103">How toocreate a support ticket for SQL Data Warehouse</span></span>
<span data-ttu-id="eee72-104">Pokud máte se službou SQL Data Warehouse nějaké problémy, můžete si vytvořit lístek podpory, aby vám mohl náš technický tým pomoct.</span><span class="sxs-lookup"><span data-stu-id="eee72-104">If you are having any issues with your SQL Data Warehouse, please create a support ticket so that our engineering team can assist you.</span></span>

> [!NOTE] 
> <span data-ttu-id="eee72-105">Od 12/20/2016 není přesný hello Kontrola stavu prostředků v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="eee72-105">As of 12/20/2016, hello resource health check in hello Azure portal is not accurate.</span></span> <span data-ttu-id="eee72-106">Aktivně pracujeme toofix tento problém.</span><span class="sxs-lookup"><span data-stu-id="eee72-106">We are actively working toofix this issue.</span></span> 


## <a name="create-a-support-ticket"></a><span data-ttu-id="eee72-107">Vytvoření lístku podpory</span><span class="sxs-lookup"><span data-stu-id="eee72-107">Create a support ticket</span></span>
1. <span data-ttu-id="eee72-108">Otevřete hello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="eee72-108">Open hello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="eee72-109">Na domovské obrazovce hello, klikněte na tlačítko hello **Nápověda a podpora** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="eee72-109">On hello Home screen, click hello **Help + support** tile.</span></span>
   
    ![Nápověda a podpora](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. <span data-ttu-id="eee72-111">Na hello nápovědy + podporu okno, klikněte na tlačítko **vytvořit žádost o podporu**.</span><span class="sxs-lookup"><span data-stu-id="eee72-111">On hello Help + Support blade, click **Create support request**.</span></span>
   
    ![Nová žádost o podporu](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. <span data-ttu-id="eee72-113">Vyberte hello **typ žádosti**.</span><span class="sxs-lookup"><span data-stu-id="eee72-113">Select hello **Request Type**.</span></span>
   
    ![Typ žádosti](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > <span data-ttu-id="eee72-115">Ve výchozím nastavení obsahuje každý server SQL (např. myserver.database.windows.net) **kvóty DTU** o hodnotě 45 000.</span><span class="sxs-lookup"><span data-stu-id="eee72-115">By default, each SQL server (e.g. myserver.database.windows.net) has a **DTU Quota** of 45,000.</span></span> <span data-ttu-id="eee72-116">Tato kvóta je jednoduše bezpečnostní omezení.</span><span class="sxs-lookup"><span data-stu-id="eee72-116">This quota is simply a safety limit.</span></span> <span data-ttu-id="eee72-117">Vytvoření lístku podpory a výběrem může zvýšení kvóty *kvóty* jako typ požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="eee72-117">You can increase your quota by creating a support ticket and selecting *Quota* as hello request type.</span></span> <span data-ttu-id="eee72-118">toocalculate potřebuje, vynásobte vaší DTU hello 7.5 pomocí hello celkový [DWU] [ DWU] potřeby.</span><span class="sxs-lookup"><span data-stu-id="eee72-118">toocalculate your DTU needs, multiply hello 7.5 by hello total [DWU][DWU] needed.</span></span> <span data-ttu-id="eee72-119">Například byste chtěli toohost dva DW6000s na jednom SQL serveru a pak měli požádat o kvótě DTU z 90,000.</span><span class="sxs-lookup"><span data-stu-id="eee72-119">For example, you would like toohost two DW6000s on one SQL server, then you should request a DTU quota of 90,000.</span></span>  <span data-ttu-id="eee72-120">Vaše aktuální spotřeba DTU z okna hello SQL serveru můžete zobrazit na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="eee72-120">You can view your current DTU consumption from hello SQL server blade in hello portal.</span></span> <span data-ttu-id="eee72-121">Databáze pozastavený a zrušení pozastavený započítávat hello kvóty DTU.</span><span class="sxs-lookup"><span data-stu-id="eee72-121">Both paused and un-paused databases count toward hello DTU quota.</span></span> 
   > 
   > 
5. <span data-ttu-id="eee72-122">Vyberte hello **předplatné** , hostitelé hello databáze s problémem hello podávají zprávy.</span><span class="sxs-lookup"><span data-stu-id="eee72-122">Select hello **Subscription** that hosts hello database with hello problem you are reporting.</span></span>
   
    ![Předplatné](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. <span data-ttu-id="eee72-124">Vyberte **SQL Data Warehouse** jako hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="eee72-124">Select **SQL Data Warehouse** as hello Resource.</span></span>
   
    ![Prostředek](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. <span data-ttu-id="eee72-126">Vyberte svůj [plán podpory Azure][Azure support plan].</span><span class="sxs-lookup"><span data-stu-id="eee72-126">Select your [Azure support plan][Azure support plan].</span></span>
   
   * <span data-ttu-id="eee72-127">**Podpora k fakturaci, správě kvót a předplatného** je k dispozici na všech úrovních podpory.</span><span class="sxs-lookup"><span data-stu-id="eee72-127">**Billing, quota and subscription management** support is available at all support levels.</span></span>
   * <span data-ttu-id="eee72-128">Podpora k problémům vyžadujícím **opravu** je poskytována prostřednictvím plánů podpory [Developer Support][Developer], [Standard Support][Standard], [Professional Direct][Professional Direct] nebo [Premier Support][Premier].</span><span class="sxs-lookup"><span data-stu-id="eee72-128">**Break-fix** support is provided through [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] or [Premier][Premier] support.</span></span> <span data-ttu-id="eee72-129">Opravu jsou problémy, které mají zákazníci při použití Azure tam, kde existuje se dá předpokládat tohoto problému hello způsobila společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="eee72-129">Break-fix issues are problems experienced by customers while using Azure where there is a reasonable expectation that Microsoft caused hello problem.</span></span>
   * <span data-ttu-id="eee72-130">**Mentorování vývojářů** a **poradenské služby** jsou k dispozici na hello [Professional Direct] [ Professional Direct] a [úrovně Premier] [ Premier] úrovních podpory.</span><span class="sxs-lookup"><span data-stu-id="eee72-130">**Developer mentoring** and **advisory services** are available at hello [Professional Direct][Professional Direct] and [Premier][Premier] support levels.</span></span> 
     
     <span data-ttu-id="eee72-131">Pokud máte plán podpory Premier Support, můžete SQL Data Warehouse nahlásit na hello problémy související s [online portálu Microsoft Premier][Microsoft Premier online portal].</span><span class="sxs-lookup"><span data-stu-id="eee72-131">If you have a Premier support plan, you can also report SQL Data Warehouse related issues on hello [Microsoft Premier online portal][Microsoft Premier online portal].</span></span>  <span data-ttu-id="eee72-132">V tématu [plánům podpory Azure] [ Azure support plan] toolearn Další informace o různých podporovat plány, včetně rozsahu, doby odezvy, cen, hello atd.  Nejčastější dotazy týkající se podpory Azure najdete v [nejčastějších dotazech týkajících se podpory k Azure][Azure support FAQs].</span><span class="sxs-lookup"><span data-stu-id="eee72-132">See [Azure support plans][Azure support plan] toolearn more about hello various support plans, including scope, response times, pricing, etc.  For frequently asked questions about Azure support, see [Azure support FAQs][Azure support FAQs].</span></span>  
     
     ![Plán podpory](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. <span data-ttu-id="eee72-134">Vyberte hello **typ problému** a **kategorie**.</span><span class="sxs-lookup"><span data-stu-id="eee72-134">Select hello **Problem Type** and **Category**.</span></span> <span data-ttu-id="eee72-135">V tomto příkladu jsme vybrali "Nástroje" jako hello typ problému a "Client tools" jako hello kategorie.</span><span class="sxs-lookup"><span data-stu-id="eee72-135">In this example, we have chosen "Tools" as hello Problem type and "Client tools" as hello category.</span></span> 
   
    ![Kategorie typu problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. <span data-ttu-id="eee72-137">Popište problém hello a zvolte hello úroveň dopadu na firmu.</span><span class="sxs-lookup"><span data-stu-id="eee72-137">Describe hello problem and choose hello level of business impact.</span></span>
   
    ![Popis problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. <span data-ttu-id="eee72-139">Předvyplní se vaše **kontaktní údaje** pro tento lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="eee72-139">Your **contact information** for this support ticket will be pre-filled.</span></span> <span data-ttu-id="eee72-140">V případě potřeby je aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="eee72-140">Update this if necessary.</span></span>
    
    ![Kontaktní údaje](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. <span data-ttu-id="eee72-142">Klikněte na tlačítko **vytvořit** žádost o podporu toosubmit hello.</span><span class="sxs-lookup"><span data-stu-id="eee72-142">Click **Create** toosubmit hello support request.</span></span>

## <a name="monitor-a-support-ticket"></a><span data-ttu-id="eee72-143">Monitorování lístku podpory</span><span class="sxs-lookup"><span data-stu-id="eee72-143">Monitor a support ticket</span></span>
<span data-ttu-id="eee72-144">Po odeslání žádosti o podporu hello tým podpory Azure hello vás kontaktovat.</span><span class="sxs-lookup"><span data-stu-id="eee72-144">After you have submitted hello support request, hello Azure support team will contact you.</span></span> <span data-ttu-id="eee72-145">toocheck vaše stav a podrobnosti žádosti, klikněte na tlačítko **spravovat žádosti o podporu** na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="eee72-145">toocheck your request status and details, click **Manage support requests** on hello dashboard.</span></span>

![Zkontrolování stavu](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a><span data-ttu-id="eee72-147">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="eee72-147">Other Resources</span></span>
<span data-ttu-id="eee72-148">Kromě toho můžete propojit s hello komunitou SQL Data Warehouse na [Stack Overflow] [ Stack Overflow] nebo na hello [fórum Azure SQL Data Warehouse na MSDN] [ Azure SQL Data Warehouse MSDN forum].</span><span class="sxs-lookup"><span data-stu-id="eee72-148">Additionally, you can connect with hello SQL Data Warehouse community on [Stack Overflow][Stack Overflow] or on hello [Azure SQL Data Warehouse MSDN forum][Azure SQL Data Warehouse MSDN forum].</span></span>

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

