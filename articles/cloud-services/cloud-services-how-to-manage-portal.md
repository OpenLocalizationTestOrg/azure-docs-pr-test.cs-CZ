---
title: "úlohy správy služby cloud aaaCommon | Microsoft Docs"
description: "Zjistěte, jak toomanage cloudových služeb v hello portálu Azure. Tyto příklady použití hello portálu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: ade8a18a7754edbaae4903230c26c009fef63ed7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-cloud-services"></a><span data-ttu-id="9d9ad-104">Jak tooManage cloudových služeb</span><span class="sxs-lookup"><span data-stu-id="9d9ad-104">How tooManage Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9d9ad-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9d9ad-105">Azure portal</span></span>](cloud-services-how-to-manage-portal.md)
> * [<span data-ttu-id="9d9ad-106">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="9d9ad-106">Azure classic portal</span></span>](cloud-services-how-to-manage.md)
>
>

<span data-ttu-id="9d9ad-107">V hello **cloudové služby (klasické)** oblasti hello Azure portálu, můžete aktualizovat roli služby nebo nasazení, zvýšení úrovně tooproduction dvoufázového nasazení, propojte prostředky tooyour cloudové služby, můžete zobrazit hello prostředky závislosti a škálování hello prostředky společně a odstraňte cloudovou službu nebo nasazení.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-107">In hello **Cloud Services (classic)** area of hello Azure portal, you can update a service role or a deployment, promote a staged deployment tooproduction, link resources tooyour cloud service so that you can see hello resource dependencies and scale hello resources together, and delete a cloud service or a deployment.</span></span>

<span data-ttu-id="9d9ad-108">Další informace o tom, jak tooscale Cloudová služba je k dispozici [zde](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-108">More information about how tooscale your cloud service is available [here](cloud-services-how-to-scale-portal.md).</span></span>

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a><span data-ttu-id="9d9ad-109">Postupy: aktualizovat roli služby cloudu nebo nasazení</span><span class="sxs-lookup"><span data-stu-id="9d9ad-109">How to: Update a cloud service role or deployment</span></span>
<span data-ttu-id="9d9ad-110">Pokud potřebujete kód aplikace hello tooupdate pro cloudové služby, použijte **aktualizace** v okně služby hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-110">If you need tooupdate hello application code for your cloud service, use **Update** on hello cloud service blade.</span></span> <span data-ttu-id="9d9ad-111">Můžete aktualizovat roli jednoho nebo všech rolí.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-111">You can update a single role or all roles.</span></span> <span data-ttu-id="9d9ad-112">tooupdate, můžete nahrát nový balíček služby nebo konfigurační soubor služby.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-112">tooupdate, you can upload a new service package or service configuration file.</span></span>

1. <span data-ttu-id="9d9ad-113">V hello [portál Azure][Azure portal], vyberte chcete tooupdate hello cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-113">In hello [Azure portal][Azure portal], select hello cloud service you want tooupdate.</span></span> <span data-ttu-id="9d9ad-114">Tento krok otevře se okno instance služby cloud hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-114">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="9d9ad-115">V okně hello, klikněte na tlačítko hello **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-115">In hello blade, click hello **Update** button.</span></span>

    ![Tlačítko Aktualizovat](./media/cloud-services-how-to-manage-portal/update-button.png)

3. <span data-ttu-id="9d9ad-117">Hello nasazení aktualizace pomocí nového souboru balíku služeb (.cspkg) a konfigurační soubor služby (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-117">Update hello deployment with a new service package file (.cspkg) and service configuration file (.cscfg).</span></span>

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. <span data-ttu-id="9d9ad-119">**Volitelně** aktualizovat označení nasazení hello a účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-119">**Optionally** update hello deployment label and hello storage account.</span></span>
5. <span data-ttu-id="9d9ad-120">Pokud žádné role mít pouze jednu instanci role, vyberte hello **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** upgradu tooproceed tooenable hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-120">If any roles have only one role instance, select hello **Deploy even if one or more roles contain a single instance** tooenable hello upgrade tooproceed.</span></span>

    <span data-ttu-id="9d9ad-121">Azure můžete pouze zaručit 99,95 % dostupnost služby při aktualizaci služby cloud pokud každá role má aspoň dvě instance role (virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-121">Azure can only guarantee 99.95 percent service availability during a cloud service update if each role has at least two role instances (virtual machines).</span></span> <span data-ttu-id="9d9ad-122">Se dvěma instancemi role jeden virtuální počítač zpracovává požadavky na klienta během hello jiných je aktualizována.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-122">With two role instances, one virtual machine processes client requests while hello other is updated.</span></span>

6. <span data-ttu-id="9d9ad-123">Zkontrolujte **spuštění nasazení** aktualizace hello toohave použijí po dokončení nahrávání hello hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-123">Check **Start deployment** toohave hello update applied after hello upload of hello package has finished.</span></span>
7. <span data-ttu-id="9d9ad-124">Klikněte na tlačítko **OK** toobegin aktualizaci služby hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-124">Click **OK** toobegin updating hello service.</span></span>

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a><span data-ttu-id="9d9ad-125">Postupy: Prohodit nasazení toopromote tooproduction dvoufázového nasazení</span><span class="sxs-lookup"><span data-stu-id="9d9ad-125">How to: Swap deployments toopromote a staged deployment tooproduction</span></span>
<span data-ttu-id="9d9ad-126">Při rozhodování toodeploy novou verzi cloudové služby, fáze a otestovat novou verzi v prostředí s pracovní cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-126">When you decide toodeploy a new release of a cloud service, stage and test your new release in your cloud service staging environment.</span></span> <span data-ttu-id="9d9ad-127">Použití **Prohodit** tooswitch hello adresy URL, které hello dvě nasazení se podrobněji a zvýšení úrovně nového tooproduction verze.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-127">Use **Swap** tooswitch hello URLs by which hello two deployments are addressed and promote a new release tooproduction.</span></span>

<span data-ttu-id="9d9ad-128">Můžete zaměnit nasazení z hello **cloudové služby** stránky nebo hello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-128">You can swap deployments from hello **Cloud Services** page or hello dashboard.</span></span>

1. <span data-ttu-id="9d9ad-129">V hello [portál Azure][Azure portal], vyberte chcete tooupdate hello cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-129">In hello [Azure portal][Azure portal], select hello cloud service you want tooupdate.</span></span> <span data-ttu-id="9d9ad-130">Tento krok otevře se okno instance služby cloud hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-130">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="9d9ad-131">V okně hello, klikněte na tlačítko hello **Prohodit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-131">In hello blade, click hello **Swap** button.</span></span>

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. <span data-ttu-id="9d9ad-133">Hello následující potvrzovací výzvu otevře.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-133">hello following confirmation prompt opens.</span></span>

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. <span data-ttu-id="9d9ad-135">Po ověření, informace o nasazení hello, klikněte na tlačítko **OK** tooswap hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-135">After you verify hello deployment information, click **OK** tooswap hello deployments.</span></span>

    <span data-ttu-id="9d9ad-136">swap nasazení Hello dochází rychle hello jediné, co se změní je hello virtuální IP adresy (VIP) pro nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-136">hello deployment swap happens quickly because hello only thing that changes is hello virtual IP addresses (VIPs) for hello deployments.</span></span>

    <span data-ttu-id="9d9ad-137">náklady na výpočetní toosave, můžete odstranit hello pracovní nasazení, jakmile ověříte, že produkčního nasazení funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-137">toosave compute costs, you can delete hello staging deployment after you verify that your production deployment is working as expected.</span></span>

### <a name="common-questions-about-swapping-deployments"></a><span data-ttu-id="9d9ad-138">Časté otázky týkající se nasazení vzájemná záměna</span><span class="sxs-lookup"><span data-stu-id="9d9ad-138">Common questions about swapping deployments</span></span>

<span data-ttu-id="9d9ad-139">**Jaké jsou požadavky hello pro odkládací nasazení?**</span><span class="sxs-lookup"><span data-stu-id="9d9ad-139">**What are hello prerequisites for swapping deployments?**</span></span>

<span data-ttu-id="9d9ad-140">Existují dva klíče požadavky pro úspěšné nasazení prohození:</span><span class="sxs-lookup"><span data-stu-id="9d9ad-140">There are two key prerequisites for a successful deployment swap:</span></span>

- <span data-ttu-id="9d9ad-141">Pokud chcete pro produkční slot toouse statickou IP adresu, musíte rezervovat jeden pro vaše přípravný slot.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-141">If you would like toouse a static IP address for your production slot, you must reserve one for your staging slot as well.</span></span> <span data-ttu-id="9d9ad-142">Hello odkládacího souboru, jinak selže.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-142">Otherwise, hello swap fails.</span></span>

- <span data-ttu-id="9d9ad-143">Všechny instance role musí být spuštěna, než bude možné provést hello swap.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-143">All instances of your roles must be running before you can perform hello swap.</span></span> <span data-ttu-id="9d9ad-144">Můžete zkontrolovat stav hello vašimi instancemi v okně Přehled hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-144">You can check hello status of your instances in hello overview blade of hello Azure portal.</span></span> <span data-ttu-id="9d9ad-145">Alternativně můžete použít hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) příkazu v prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-145">Alternatively, you can use hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) command in Windows PowerShell.</span></span>

<span data-ttu-id="9d9ad-146">Všimněte si, že hostovaného operačního systému aktualizace a opravy operace služby může také způsobit nasazení prohození toofail.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-146">Note that Guest OS updates and service healing operations can also cause deployment swaps toofail.</span></span> <span data-ttu-id="9d9ad-147">Další informace najdete v tématu [Poradce při potížích se nasazení služby cloud](cloud-services-troubleshoot-deployment-problems.md).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-147">For more information, see [Troubleshoot cloud service deployment problems](cloud-services-troubleshoot-deployment-problems.md).</span></span>

<span data-ttu-id="9d9ad-148">**Nesnižuje prohození výpadek své aplikaci? Jak by měly zpracovávat ho?**</span><span class="sxs-lookup"><span data-stu-id="9d9ad-148">**Does a swap incur downtime for my application? How should I handle it?**</span></span>

<span data-ttu-id="9d9ad-149">Jak je popsáno v poslední části hello, prohození nasazení je obvykle rychlé, protože je právě změnu konfigurace pro vyrovnávání zatížení Azure hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-149">As described in hello last section, a deployment swap is typically fast since it is just a configuration change in hello Azure load balancer.</span></span> <span data-ttu-id="9d9ad-150">V některých případech ale ho můžete trvat deset nebo víc sekund a způsobit selhání přechodný připojení.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-150">In some cases, however, it can take ten or more seconds and result in transient connection failures.</span></span> <span data-ttu-id="9d9ad-151">toolimit dopad tooyour zákazníků, zvažte implementaci [logika opakovaných pokusů klienta](../best-practices-retry-general.md).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-151">toolimit impact tooyour customers, consider implementing [client retry logic](../best-practices-retry-general.md).</span></span>

## <a name="how-to-link-a-resource-tooa-cloud-service"></a><span data-ttu-id="9d9ad-152">Postupy: odkaz prostředků tooa cloudové služby</span><span class="sxs-lookup"><span data-stu-id="9d9ad-152">How to: Link a resource tooa cloud service</span></span>
<span data-ttu-id="9d9ad-153">Hello portál Azure neobsahuje odkazy na prostředky společně jako hello aktuální portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-153">hello Azure portal does not link resources together like hello current Azure classic portal does.</span></span> <span data-ttu-id="9d9ad-154">Místo toho nasaďte další prostředky toohello stejnou skupinu prostředků, které jsou používány hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-154">Instead, deploy additional resources toohello same resource group being used by hello Cloud Service.</span></span>

## <a name="how-to-delete-deployments-and-a-cloud-service"></a><span data-ttu-id="9d9ad-155">Postupy: odstranění nasazení a cloudové služby</span><span class="sxs-lookup"><span data-stu-id="9d9ad-155">How to: Delete deployments and a cloud service</span></span>
<span data-ttu-id="9d9ad-156">Před odstraněním cloudové služby, je nutné odstranit každé existující nasazení.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-156">Before you can delete a cloud service, you must delete each existing deployment.</span></span>

<span data-ttu-id="9d9ad-157">náklady na výpočetní toosave, můžete odstranit hello pracovní nasazení, jakmile ověříte, že produkčního nasazení funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-157">toosave compute costs, you can delete hello staging deployment after you verify that your production deployment is working as expected.</span></span> <span data-ttu-id="9d9ad-158">Fakturuje se výpočetní náklady pro instance nasazené rolí, které jsou zastaveny.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-158">You are billed for compute costs for deployed role instances that are stopped.</span></span>

<span data-ttu-id="9d9ad-159">Použijte následující postup toodelete hello nasazení nebo cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-159">Use hello following procedure toodelete a deployment or your cloud service.</span></span>

1. <span data-ttu-id="9d9ad-160">V hello [portál Azure][Azure portal], vyberte chcete toodelete hello cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-160">In hello [Azure portal][Azure portal], select hello cloud service you want toodelete.</span></span> <span data-ttu-id="9d9ad-161">Tento krok otevře se okno instance služby cloud hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-161">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="9d9ad-162">V okně hello, klikněte na tlačítko hello **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-162">In hello blade, click hello **Delete** button.</span></span>

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. <span data-ttu-id="9d9ad-164">Hello celý cloudové služby můžete odstranit kontrolou **Cloudová služba a její nasazení** nebo zvolte buď hello **produkční nasazení** nebo hello **pracovní nasazení**.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-164">You can delete hello entire cloud service by checking **Cloud service and its deployments** or choose either hello **Production deployment** or hello **Staging deployment**.</span></span>

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. <span data-ttu-id="9d9ad-166">Klikněte na tlačítko hello **odstranit** tlačítko dole v hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-166">Click hello **Delete** button at hello bottom.</span></span>
5. <span data-ttu-id="9d9ad-167">toodelete hello Cloudová služba, klikněte na tlačítko **odstranění Cloudová služba**.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-167">toodelete hello cloud service, click **Delete cloud service**.</span></span> <span data-ttu-id="9d9ad-168">Klikněte na výzvy k potvrzení hello **Ano**.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-168">Then, at hello confirmation prompt, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="9d9ad-169">Pokud Cloudová služba je Odstraněná a podrobné monitorování je nastavena, musíte odstranit hello data ručně z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-169">When a cloud service is deleted, and verbose monitoring is configured, you must delete hello data manually from your storage account.</span></span> <span data-ttu-id="9d9ad-170">Informace o kde toofind hello metriky tabulky najdete v tématu [to](cloud-services-how-to-monitor.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-170">For information about where toofind hello metrics tables, see [this](cloud-services-how-to-monitor.md) article.</span></span>


## <a name="how-to-find-more-information-about-failed-deployments"></a><span data-ttu-id="9d9ad-171">Postupy: Další informace o selhání nasazení</span><span class="sxs-lookup"><span data-stu-id="9d9ad-171">How to: Find more information about failed deployments</span></span>
<span data-ttu-id="9d9ad-172">Hello **přehled** okno obsahuje stavového řádku v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-172">hello **Overview** blade has a status bar at hello top.</span></span> <span data-ttu-id="9d9ad-173">Po kliknutí na tlačítko panelu hello, nové okno se zobrazí veškeré informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-173">When you click hello bar, a new blade opens and displays any error information.</span></span> <span data-ttu-id="9d9ad-174">Pokud nasazení hello neobsahuje žádné chyby, okno informace hello je prázdný.</span><span class="sxs-lookup"><span data-stu-id="9d9ad-174">If hello deployment does not contain any errors, hello information blade is blank.</span></span>

![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a><span data-ttu-id="9d9ad-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d9ad-176">Next steps</span></span>
* <span data-ttu-id="9d9ad-177">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-177">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="9d9ad-178">Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-178">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="9d9ad-179">Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-179">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="9d9ad-180">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9d9ad-180">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
