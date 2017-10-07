---
title: aaaScale aplikaci v Azure | Microsoft Docs
description: "Zjistěte, jak tooscale aplikaci v Azure App Service tooadd kapacitu a funkce."
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
ms.openlocfilehash: 97f46f77aeef95aec90d38e8396a023820e3caeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-up-an-app-in-azure"></a><span data-ttu-id="41758-103">Škálování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="41758-103">Scale up an app in Azure</span></span>
<span data-ttu-id="41758-104">Tento článek ukazuje, jak tooscale vaší aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="41758-104">This article shows you how tooscale your app in Azure App Service.</span></span> <span data-ttu-id="41758-105">Existují dva pracovní postupy pro škálování, škálování nahoru a škálování a tento článek vysvětluje hello rozšiřování škálování využívajících pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="41758-105">There are two workflows for scaling, scale up and scale out, and this article explains hello scale up workflow.</span></span>

* <span data-ttu-id="41758-106">[Vertikální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): získat další procesoru, paměti, místa na disku a speciálních funkcí, jako vyhrazené virtuální počítače (VM), vlastní domény a certifikáty, přípravné sloty, automatické škálování a další.</span><span class="sxs-lookup"><span data-stu-id="41758-106">[Scale up](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Get more CPU, memory, disk space, and extra features like dedicated virtual machines (VMs), custom domains and certificates, staging slots, autoscaling, and more.</span></span> <span data-ttu-id="41758-107">Škálovat tak, že změníte hello cenovou úroveň plánu služby App Service, který je součástí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="41758-107">You scale up by changing hello pricing tier of the App Service plan that your app belongs to.</span></span>
* <span data-ttu-id="41758-108">[Horizontální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): zvyšte počet hello instancí virtuálních počítačů, které spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="41758-108">[Scale out](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Increase hello number of VM instances that run your app.</span></span>
  <span data-ttu-id="41758-109">Je možné škálovat na tooas mnoho jako 20 instance, v závislosti na cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="41758-109">You can scale out tooas many as 20 instances, depending on your pricing tier.</span></span> <span data-ttu-id="41758-110">[Prostředí App Service](../app-service/app-service-app-service-environments-readme.md) v **Premium** vrstvy další zvýší vaše instance too50 počet Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="41758-110">[App Service Environments](../app-service/app-service-app-service-environments-readme.md) in **Premium** tier will further increase your scale-out count too50 instances.</span></span> <span data-ttu-id="41758-111">Další informace o škálování najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="41758-111">For more information about scaling out, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md).</span></span> <span data-ttu-id="41758-112">Zde najdete na více systémů jak toouse automatické škálování, což je počet instancí tooscale automaticky na základě předdefinovaných pravidel a plány.</span><span class="sxs-lookup"><span data-stu-id="41758-112">There you will find out how toouse autoscaling, which is tooscale instance count automatically based on predefined rules and schedules.</span></span>

<span data-ttu-id="41758-113">Hello škálování nastavení proveďte pouze sekund tooapply a vliv na všechny aplikace ve vaší [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="41758-113">hello scale settings take only seconds tooapply and affect all apps in your [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
<span data-ttu-id="41758-114">Nevytvářejí vyžadují jste toochange kódu ani znovu nasaďte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41758-114">They do not require you toochange your code or redeploy your application.</span></span>

<span data-ttu-id="41758-115">Informace o cenách hello a funkce jednotlivých plány služby App Service najdete v tématu [podrobnosti o cenách na aplikaci služby](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="41758-115">For information about hello pricing and features of individual App Service plans, see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>  

> [!NOTE]
> <span data-ttu-id="41758-116">Předtím, než přepnete plán služby App Service z hello **volné** úroveň, je nutné nejprve odebrat hello [limitů útraty](https://azure.microsoft.com/pricing/spending-limits/) nastavené pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="41758-116">Before you switch an App Service plan from hello **Free** tier, you must first remove hello [spending limits](https://azure.microsoft.com/pricing/spending-limits/) in place for your Azure subscription.</span></span> <span data-ttu-id="41758-117">tooview nebo změňte možnosti pro vaše předplatné Microsoft Azure App Service najdete v části [předplatná Microsoft Azure][azuresubscriptions].</span><span class="sxs-lookup"><span data-stu-id="41758-117">tooview or change options for your Microsoft Azure App Service subscription, see [Microsoft Azure Subscriptions][azuresubscriptions].</span></span>
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a><span data-ttu-id="41758-118">Škálování cenové úrovně</span><span class="sxs-lookup"><span data-stu-id="41758-118">Scale up your pricing tier</span></span>
1. <span data-ttu-id="41758-119">V prohlížeči otevřete hello [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="41758-119">In your browser, open hello [Azure portal][portal].</span></span>
2. <span data-ttu-id="41758-120">V okně vaší aplikace, klikněte na tlačítko **všechna nastavení**a potom klikněte na **vertikálně navýšit kapacitu**.</span><span class="sxs-lookup"><span data-stu-id="41758-120">In your app's blade, click **All settings**, and then click **Scale Up**.</span></span>
   
    ![Přejděte tooscale si aplikaci Azure.][ChooseWHP]
3. <span data-ttu-id="41758-122">Zvolte úroveň a pak klikněte na **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="41758-122">Choose your tier, and then click **Select**.</span></span>
   
    <span data-ttu-id="41758-123">Hello **oznámení** karta bude flash zelená **úspěch** po dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="41758-123">hello **Notifications** tab will flash a green **SUCCESS** after hello operation is complete.</span></span>

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a><span data-ttu-id="41758-124">Škálování související informační zdroje</span><span class="sxs-lookup"><span data-stu-id="41758-124">Scale related resources</span></span>
<span data-ttu-id="41758-125">Pokud vaše aplikace závisí na jiné služby, například Azure SQL Database nebo úložiště Azure, můžete škálovat také tyto prostředky na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="41758-125">If your app depends on other services, such as Azure SQL Database or Azure Storage, you can also scale up those resources based on your needs.</span></span> <span data-ttu-id="41758-126">Tyto prostředky nejsou změněna pomocí hello plán služby App Service a musí být změněna samostatně.</span><span class="sxs-lookup"><span data-stu-id="41758-126">These resources are not scaled with hello App Service plan and must be scaled separately.</span></span>

1. <span data-ttu-id="41758-127">V **Essentials**, klikněte na tlačítko hello **skupiny prostředků** odkaz.</span><span class="sxs-lookup"><span data-stu-id="41758-127">In **Essentials**, click hello **Resource group** link.</span></span>
   
    ![Škálování souvisejících prostředků Azure aplikace](./media/web-sites-scale/RGEssentialsLink.png)
2. <span data-ttu-id="41758-129">V hello **Souhrn** součástí hello **skupiny prostředků** okno, klepněte na prostředků, které chcete tooscale.</span><span class="sxs-lookup"><span data-stu-id="41758-129">In hello **Summary** part of hello **Resource group** blade, click a resource that you want tooscale.</span></span> <span data-ttu-id="41758-130">Hello následující snímek obrazovky ukazuje prostředků databáze SQL a prostředek služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="41758-130">hello following screenshot shows a SQL Database resource and an Azure Storage resource.</span></span>
   
    ![Přejděte tooresource skupiny okno tooscale si aplikaci Azure](./media/web-sites-scale/ResourceGroup.png)
3. <span data-ttu-id="41758-132">Pro databáze SQL prostředek, klikněte na tlačítko **nastavení** > **cenová úroveň** tooscale hello cenová úroveň.</span><span class="sxs-lookup"><span data-stu-id="41758-132">For a SQL Database resource, click **Settings** > **Pricing tier** tooscale hello pricing tier.</span></span>
   
    ![Škálování hello SQL databáze back-end pro vaše aplikace Azure](./media/web-sites-scale/ScaleDatabase.png)
   
    <span data-ttu-id="41758-134">Můžete také zapnout [geografická replikace](../sql-database/sql-database-geo-replication-overview.md) pro instanci databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="41758-134">You can also turn on [geo-replication](../sql-database/sql-database-geo-replication-overview.md) for your SQL Database instance.</span></span>
   
    <span data-ttu-id="41758-135">Prostředek Azure Storage, klikněte na tlačítko **nastavení** > **konfigurace** tooscale vaše možnosti úložiště.</span><span class="sxs-lookup"><span data-stu-id="41758-135">For an Azure Storage resource, click **Settings** > **Configuration** tooscale up your storage options.</span></span>
   
    ![Škálování účtu úložiště Azure hello používá vaše aplikace Azure](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a><span data-ttu-id="41758-137">Další informace o funkcích vývojáře</span><span class="sxs-lookup"><span data-stu-id="41758-137">Learn about developer features</span></span>
<span data-ttu-id="41758-138">V závislosti na hello cenovou úroveň hello následující funkce orientovaných na vývojáře jsou k dispozici:</span><span class="sxs-lookup"><span data-stu-id="41758-138">Depending on hello pricing tier, hello following developer-oriented features are available:</span></span>

### <a name="bitness"></a><span data-ttu-id="41758-139">Počet bitů</span><span class="sxs-lookup"><span data-stu-id="41758-139">Bitness</span></span>
* <span data-ttu-id="41758-140">Hello **základní**, **standardní**, a **Premium** vrstev podporovat 64bitové a 32bitové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41758-140">hello **Basic**, **Standard**, and **Premium** tiers support 64-bit and 32-bit applications.</span></span>
* <span data-ttu-id="41758-141">Hello **volné** a **sdílené** plán vrstev podporují pouze 32bitové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41758-141">hello **Free** and **Shared** plan tiers support 32-bit applications only.</span></span>

### <a name="debugger-support"></a><span data-ttu-id="41758-142">Podpora ladění</span><span class="sxs-lookup"><span data-stu-id="41758-142">Debugger support</span></span>
* <span data-ttu-id="41758-143">Ladicí program podpora je k dispozici pro hello **volné**, **sdílené**, a **základní** režimy na jedno připojení na plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="41758-143">Debugger support is available for hello **Free**, **Shared**, and **Basic** modes at one connection per App Service plan.</span></span>
* <span data-ttu-id="41758-144">Ladicí program podpora je k dispozici pro hello **standardní** a **Premium** režimy v pěti souběžných připojení na plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="41758-144">Debugger support is available for hello **Standard** and **Premium** modes at five concurrent connections per App Service plan.</span></span>

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a><span data-ttu-id="41758-145">Další informace o dalších funkcích</span><span class="sxs-lookup"><span data-stu-id="41758-145">Learn about other features</span></span>
* <span data-ttu-id="41758-146">Podrobné informace o všech hello zbývající funkcí v hello služby App Service plánů, včetně ceny a funkce, které vás zajímají tooall uživatelů (včetně vývojáři), najdete v části [podrobnosti o cenách na aplikaci služby](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="41758-146">For detailed information about all of hello remaining features in hello App Service plans, including pricing and features of interest tooall users (including developers), see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>

> [!NOTE]
> <span data-ttu-id="41758-147">Pokud chcete, aby tooget začít s Azure App Service, ještě než si zaregistrujete účet Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/) kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="41758-147">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/) where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="41758-148">Je vyžadována žádná platební karta a bez jakýchkoli závazků.</span><span class="sxs-lookup"><span data-stu-id="41758-148">No credit cards are required and there are no commitments.</span></span>
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a><span data-ttu-id="41758-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41758-149">Next steps</span></span>
* <span data-ttu-id="41758-150">tooget začít s Azure, najdete v části [bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="41758-150">tooget started with Azure, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="41758-151">Informace o cenách, podpory a SLA najdete na adrese hello následující odkazy.</span><span class="sxs-lookup"><span data-stu-id="41758-151">For information about pricing, support, and SLA, visit hello following links.</span></span>
  
    [<span data-ttu-id="41758-152">Podrobnosti o cenách přenos dat</span><span class="sxs-lookup"><span data-stu-id="41758-152">Data Transfers Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [<span data-ttu-id="41758-153">Plánům podpory Azure Microsoft</span><span class="sxs-lookup"><span data-stu-id="41758-153">Microsoft Azure Support Plans</span></span>](https://azure.microsoft.com/support/plans/)
  
    [<span data-ttu-id="41758-154">smlouvám o úrovni služeb</span><span class="sxs-lookup"><span data-stu-id="41758-154">Service Level Agreements</span></span>](https://azure.microsoft.com/support/legal/sla/)
  
    [<span data-ttu-id="41758-155">Podrobnosti o cenách na SQL databázi</span><span class="sxs-lookup"><span data-stu-id="41758-155">SQL Database Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/sql-database/)
  
    <span data-ttu-id="41758-156">[Virtuální počítač a velikost cloudových služeb pro Microsoft Azure][vmsizes]</span><span class="sxs-lookup"><span data-stu-id="41758-156">[Virtual Machine and Cloud Service Sizes for Microsoft Azure][vmsizes]</span></span>
  
    [<span data-ttu-id="41758-157">Podrobnosti o cenách služby App Service</span><span class="sxs-lookup"><span data-stu-id="41758-157">App Service Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/app-service/)
  
    [<span data-ttu-id="41758-158">Podrobnosti o - připojení SSL cenách služby App Service</span><span class="sxs-lookup"><span data-stu-id="41758-158">App Service Pricing Details - SSL Connections</span></span>](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* <span data-ttu-id="41758-159">Informace o službě Azure App Service osvědčených postupů, včetně vytváření škálovatelná a flexibilní architektuře, najdete v části [osvědčené postupy: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span><span class="sxs-lookup"><span data-stu-id="41758-159">For information about Azure App Service best practices, including building a scalable and resilient architecture, see [Best Practices: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span></span>
* <span data-ttu-id="41758-160">Videa o škálování aplikací App Service naleznete v hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="41758-160">For videos about scaling App Service apps, see hello following resources:</span></span>
  
  * [<span data-ttu-id="41758-161">Když tooScale weby Azure - s Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="41758-161">When tooScale Azure Websites - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [<span data-ttu-id="41758-162">Automatické škálování webů Azure, využití procesoru nebo naplánované – s Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="41758-162">Auto Scaling Azure Websites, CPU or Scheduled - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [<span data-ttu-id="41758-163">Jak Azure měřítko weby - s Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="41758-163">How Azure Websites Scale - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

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
