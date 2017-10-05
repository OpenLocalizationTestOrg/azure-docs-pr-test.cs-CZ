---
title: "Škálování aplikace v Azure | Microsoft Docs"
description: "Zjistěte, jak škálování aplikace v Azure App Service k přidání kapacity a funkcí."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.openlocfilehash: 75ddbacbd4dd14597b786d26f0730477f6c85811
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-up-an-app-in-azure"></a><span data-ttu-id="2cbba-103">Škálování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="2cbba-103">Scale up an app in Azure</span></span>
<span data-ttu-id="2cbba-104">Tento článek ukazuje, jak škálování aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2cbba-104">This article shows you how to scale your app in Azure App Service.</span></span> <span data-ttu-id="2cbba-105">Existují dva pracovní postupy pro škálování, škálování nahoru a škálování a tento článek vysvětluje rozšiřování škálování využívajících pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="2cbba-105">There are two workflows for scaling, scale up and scale out, and this article explains the scale up workflow.</span></span>

* <span data-ttu-id="2cbba-106">[Vertikální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): získat další procesoru, paměti, místa na disku a speciálních funkcí, jako vyhrazené virtuální počítače (VM), vlastní domény a certifikáty, přípravné sloty, automatické škálování a další.</span><span class="sxs-lookup"><span data-stu-id="2cbba-106">[Scale up](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Get more CPU, memory, disk space, and extra features like dedicated virtual machines (VMs), custom domains and certificates, staging slots, autoscaling, and more.</span></span> <span data-ttu-id="2cbba-107">Škálovat změnou cenové úrovně plánu služby App Service, který je součástí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2cbba-107">You scale up by changing the pricing tier of the App Service plan that your app belongs to.</span></span>
* <span data-ttu-id="2cbba-108">[Horizontální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): zvyšte počet instancí virtuálního počítače, které spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="2cbba-108">[Scale out](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Increase the number of VM instances that run your app.</span></span>
  <span data-ttu-id="2cbba-109">Můžete škálovat na až 20 instance, v závislosti na cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="2cbba-109">You can scale out to as many as 20 instances, depending on your pricing tier.</span></span> <span data-ttu-id="2cbba-110">[Prostředí App Service](../app-service/app-service-app-service-environments-readme.md) v **Premium** vrstvy další zvýší spočítat Škálováním na více systémů na 50 instancí.</span><span class="sxs-lookup"><span data-stu-id="2cbba-110">[App Service Environments](../app-service/app-service-app-service-environments-readme.md) in **Premium** tier will further increase your scale-out count to 50 instances.</span></span> <span data-ttu-id="2cbba-111">Další informace o škálování najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="2cbba-111">For more information about scaling out, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md).</span></span> <span data-ttu-id="2cbba-112">Zde najdete informace o tom, jak používat automatické škálování, což je pro škálování počtu instancí automaticky na základě předdefinovaných pravidel a plány.</span><span class="sxs-lookup"><span data-stu-id="2cbba-112">There you will find out how to use autoscaling, which is to scale instance count automatically based on predefined rules and schedules.</span></span>

<span data-ttu-id="2cbba-113">Nastavení škálování trvat jenom sekund použít a vliv na všechny aplikace ve vaší [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2cbba-113">The scale settings take only seconds to apply and affect all apps in your [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
<span data-ttu-id="2cbba-114">Změna kódu nebo znovu nasadit aplikace nevyžadují.</span><span class="sxs-lookup"><span data-stu-id="2cbba-114">They do not require you to change your code or redeploy your application.</span></span>

<span data-ttu-id="2cbba-115">Informace o cenách a funkce jednotlivých plány služby App Service najdete v tématu [podrobnosti o cenách na aplikaci služby](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="2cbba-115">For information about the pricing and features of individual App Service plans, see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>  

> [!NOTE]
> <span data-ttu-id="2cbba-116">Předtím, než přepnete z plán služby App Service **volné** úroveň, je nutné nejprve odebrat [limitů útraty](https://azure.microsoft.com/pricing/spending-limits/) nastavené pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2cbba-116">Before you switch an App Service plan from the **Free** tier, you must first remove the [spending limits](https://azure.microsoft.com/pricing/spending-limits/) in place for your Azure subscription.</span></span> <span data-ttu-id="2cbba-117">Zobrazit nebo změnit možnosti pro vaše předplatné Microsoft Azure App Service naleznete v tématu [předplatná Microsoft Azure][azuresubscriptions].</span><span class="sxs-lookup"><span data-stu-id="2cbba-117">To view or change options for your Microsoft Azure App Service subscription, see [Microsoft Azure Subscriptions][azuresubscriptions].</span></span>
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a><span data-ttu-id="2cbba-118">Škálování cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="2cbba-118">Scale up your pricing tier</span></span>
1. <span data-ttu-id="2cbba-119">V prohlížeči otevřete [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="2cbba-119">In your browser, open the [Azure portal][portal].</span></span>
2. <span data-ttu-id="2cbba-120">V okně vaší aplikace, klikněte na tlačítko **všechna nastavení**a potom klikněte na **vertikálně navýšit kapacitu**.</span><span class="sxs-lookup"><span data-stu-id="2cbba-120">In your app's blade, click **All settings**, and then click **Scale Up**.</span></span>
   
    ![Přejděte do škálovat aplikaci Azure.][ChooseWHP]
3. <span data-ttu-id="2cbba-122">Zvolte úroveň a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="2cbba-122">Choose your tier, and then click **Select**.</span></span>
   
    <span data-ttu-id="2cbba-123">**Oznámení** karta bude flash zelená **úspěch** po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="2cbba-123">The **Notifications** tab will flash a green **SUCCESS** after the operation is complete.</span></span>

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a><span data-ttu-id="2cbba-124">Škálování související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="2cbba-124">Scale related resources</span></span>
<span data-ttu-id="2cbba-125">Pokud vaše aplikace závisí na jiné služby, například Azure SQL Database nebo úložiště Azure, můžete škálovat také tyto prostředky na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="2cbba-125">If your app depends on other services, such as Azure SQL Database or Azure Storage, you can also scale up those resources based on your needs.</span></span> <span data-ttu-id="2cbba-126">Tyto prostředky nejsou změněna pomocí plán služby App Service a musí být změněna samostatně.</span><span class="sxs-lookup"><span data-stu-id="2cbba-126">These resources are not scaled with the App Service plan and must be scaled separately.</span></span>

1. <span data-ttu-id="2cbba-127">V **Essentials**, klikněte **skupiny prostředků** odkaz.</span><span class="sxs-lookup"><span data-stu-id="2cbba-127">In **Essentials**, click the **Resource group** link.</span></span>
   
    ![Škálování souvisejících prostředků Azure aplikace](./media/web-sites-scale/RGEssentialsLink.png)
2. <span data-ttu-id="2cbba-129">V **Souhrn** součástí **skupiny prostředků** okno, klepněte na prostředek, který chcete škálovat.</span><span class="sxs-lookup"><span data-stu-id="2cbba-129">In the **Summary** part of the **Resource group** blade, click a resource that you want to scale.</span></span> <span data-ttu-id="2cbba-130">Následující snímek obrazovky ukazuje prostředků databáze SQL a prostředek služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2cbba-130">The following screenshot shows a SQL Database resource and an Azure Storage resource.</span></span>
   
    ![Přejděte do okna prostředků skupiny škálování aplikace Azure](./media/web-sites-scale/ResourceGroup.png)
3. <span data-ttu-id="2cbba-132">Pro databáze SQL prostředek, klikněte na tlačítko **nastavení** > **cenová úroveň** škálování cenové úrovně.</span><span class="sxs-lookup"><span data-stu-id="2cbba-132">For a SQL Database resource, click **Settings** > **Pricing tier** to scale the pricing tier.</span></span>
   
    ![Škálování SQL databáze back-end pro vaše aplikace Azure](./media/web-sites-scale/ScaleDatabase.png)
   
    <span data-ttu-id="2cbba-134">Můžete také zapnout [geografická replikace](../sql-database/sql-database-geo-replication-overview.md) pro instanci databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="2cbba-134">You can also turn on [geo-replication](../sql-database/sql-database-geo-replication-overview.md) for your SQL Database instance.</span></span>
   
    <span data-ttu-id="2cbba-135">Prostředek Azure Storage, klikněte na tlačítko **nastavení** > **konfigurace** škálování možnosti úložiště.</span><span class="sxs-lookup"><span data-stu-id="2cbba-135">For an Azure Storage resource, click **Settings** > **Configuration** to scale up your storage options.</span></span>
   
    ![Škálování účtu úložiště Azure používat aplikaci Azure](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a><span data-ttu-id="2cbba-137">Další informace o funkcích vývojáře</span><span class="sxs-lookup"><span data-stu-id="2cbba-137">Learn about developer features</span></span>
<span data-ttu-id="2cbba-138">V závislosti na cenovou úroveň jsou k dispozici následující funkce orientovaných na vývojáře:</span><span class="sxs-lookup"><span data-stu-id="2cbba-138">Depending on the pricing tier, the following developer-oriented features are available:</span></span>

### <a name="bitness"></a><span data-ttu-id="2cbba-139">Počet bitů</span><span class="sxs-lookup"><span data-stu-id="2cbba-139">Bitness</span></span>
* <span data-ttu-id="2cbba-140">**Základní**, **standardní**, a **Premium** vrstev podporovat 64bitové a 32bitové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2cbba-140">The **Basic**, **Standard**, and **Premium** tiers support 64-bit and 32-bit applications.</span></span>
* <span data-ttu-id="2cbba-141">**Volné** a **sdílené** plán vrstev podporují pouze 32bitové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2cbba-141">The **Free** and **Shared** plan tiers support 32-bit applications only.</span></span>

### <a name="debugger-support"></a><span data-ttu-id="2cbba-142">Podpora ladění</span><span class="sxs-lookup"><span data-stu-id="2cbba-142">Debugger support</span></span>
* <span data-ttu-id="2cbba-143">Ladicí program podpora je k dispozici pro **volné**, **sdílené**, a **základní** režimy na jedno připojení na plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2cbba-143">Debugger support is available for the **Free**, **Shared**, and **Basic** modes at one connection per App Service plan.</span></span>
* <span data-ttu-id="2cbba-144">Ladicí program podpora je k dispozici pro **standardní** a **Premium** režimy v pěti souběžných připojení na plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2cbba-144">Debugger support is available for the **Standard** and **Premium** modes at five concurrent connections per App Service plan.</span></span>

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a><span data-ttu-id="2cbba-145">Další informace o dalších funkcích</span><span class="sxs-lookup"><span data-stu-id="2cbba-145">Learn about other features</span></span>
* <span data-ttu-id="2cbba-146">Podrobné informace o všechny ostatní funkce ve službě App Service plánů, včetně ceny a funkcích pro všechny uživatele (včetně vývojáři), najdete v části [podrobnosti o cenách na aplikaci služby](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="2cbba-146">For detailed information about all of the remaining features in the App Service plans, including pricing and features of interest to all users (including developers), see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>

> [!NOTE]
> <span data-ttu-id="2cbba-147">Pokud chcete začít pracovat s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/) kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="2cbba-147">If you want to get started with Azure App Service before you sign up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/) where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="2cbba-148">Je vyžadována žádná platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="2cbba-148">No credit cards are required and there are no commitments.</span></span>
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a><span data-ttu-id="2cbba-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2cbba-149">Next steps</span></span>
* <span data-ttu-id="2cbba-150">Chcete-li začít s Azure, přečtěte si téma [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2cbba-150">To get started with Azure, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="2cbba-151">Informace o cenách, podpory a smlouvy o úrovni služeb získáte pomocí následujících odkazů.</span><span class="sxs-lookup"><span data-stu-id="2cbba-151">For information about pricing, support, and SLA, visit the following links.</span></span>
  
    [<span data-ttu-id="2cbba-152">Podrobnosti o cenách přenos dat</span><span class="sxs-lookup"><span data-stu-id="2cbba-152">Data Transfers Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [<span data-ttu-id="2cbba-153">Plánům podpory Azure Microsoft</span><span class="sxs-lookup"><span data-stu-id="2cbba-153">Microsoft Azure Support Plans</span></span>](https://azure.microsoft.com/support/plans/)
  
    [<span data-ttu-id="2cbba-154">smlouvám o úrovni služeb</span><span class="sxs-lookup"><span data-stu-id="2cbba-154">Service Level Agreements</span></span>](https://azure.microsoft.com/support/legal/sla/)
  
    [<span data-ttu-id="2cbba-155">Podrobnosti o cenách na SQL databázi</span><span class="sxs-lookup"><span data-stu-id="2cbba-155">SQL Database Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/sql-database/)
  
    <span data-ttu-id="2cbba-156">[Virtuální počítač a velikost cloudových služeb pro Microsoft Azure][vmsizes]</span><span class="sxs-lookup"><span data-stu-id="2cbba-156">[Virtual Machine and Cloud Service Sizes for Microsoft Azure][vmsizes]</span></span>
  
    [<span data-ttu-id="2cbba-157">Podrobnosti o cenách služby App Service</span><span class="sxs-lookup"><span data-stu-id="2cbba-157">App Service Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/app-service/)
  
    [<span data-ttu-id="2cbba-158">Podrobnosti o - připojení SSL cenách služby App Service</span><span class="sxs-lookup"><span data-stu-id="2cbba-158">App Service Pricing Details - SSL Connections</span></span>](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* <span data-ttu-id="2cbba-159">Informace o službě Azure App Service osvědčených postupů, včetně vytváření škálovatelná a flexibilní architektuře, najdete v části [osvědčené postupy: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span><span class="sxs-lookup"><span data-stu-id="2cbba-159">For information about Azure App Service best practices, including building a scalable and resilient architecture, see [Best Practices: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span></span>
* <span data-ttu-id="2cbba-160">Videa o škálování služby App Service aplikace najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="2cbba-160">For videos about scaling App Service apps, see the following resources:</span></span>
  
  * [<span data-ttu-id="2cbba-161">Kdy škálovat weby Azure - s Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="2cbba-161">When to Scale Azure Websites - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [<span data-ttu-id="2cbba-162">Automatické škálování webů Azure, využití procesoru nebo naplánované – s Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="2cbba-162">Auto Scaling Azure Websites, CPU or Scheduled - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [<span data-ttu-id="2cbba-163">Jak Azure měřítko weby - s Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="2cbba-163">How Azure Websites Scale - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
