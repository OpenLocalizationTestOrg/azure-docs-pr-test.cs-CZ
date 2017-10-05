---
title: "Běžné úlohy správy cloudové služby | Microsoft Docs"
description: "Naučte se spravovat cloudové služby na portálu Azure. Tyto příklady použití portálu Azure."
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
ms.openlocfilehash: 4650cebe18153e3b10bbec685a66a590348c99e9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-manage-cloud-services"></a><span data-ttu-id="b3dbc-104">Jak spravovat cloudové služby</span><span class="sxs-lookup"><span data-stu-id="b3dbc-104">How to Manage Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b3dbc-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b3dbc-105">Azure portal</span></span>](cloud-services-how-to-manage-portal.md)
> * [<span data-ttu-id="b3dbc-106">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="b3dbc-106">Azure classic portal</span></span>](cloud-services-how-to-manage.md)
>
>

<span data-ttu-id="b3dbc-107">V **cloudové služby (klasické)** oblasti Azure portálu, můžete aktualizovat roli služby nebo nasazení, zvýšení úrovně dvoufázového nasazení do produkčního prostředí, prostředky propojit cloudové služby, aby mohli zobrazit závislosti prostředků a škálování prostředky společně a odstraňte cloudovou službu nebo nasazení.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-107">In the **Cloud Services (classic)** area of the Azure portal, you can update a service role or a deployment, promote a staged deployment to production, link resources to your cloud service so that you can see the resource dependencies and scale the resources together, and delete a cloud service or a deployment.</span></span>

<span data-ttu-id="b3dbc-108">Další informace o tom, jak škálování cloudové služby jsou k dispozici [zde](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-108">More information about how to scale your cloud service is available [here](cloud-services-how-to-scale-portal.md).</span></span>

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a><span data-ttu-id="b3dbc-109">Postupy: aktualizovat roli služby cloudu nebo nasazení</span><span class="sxs-lookup"><span data-stu-id="b3dbc-109">How to: Update a cloud service role or deployment</span></span>
<span data-ttu-id="b3dbc-110">Pokud je potřeba aktualizovat kód aplikace pro cloudové služby, použijte **aktualizace** v okně cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-110">If you need to update the application code for your cloud service, use **Update** on the cloud service blade.</span></span> <span data-ttu-id="b3dbc-111">Můžete aktualizovat roli jednoho nebo všech rolí.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-111">You can update a single role or all roles.</span></span> <span data-ttu-id="b3dbc-112">Pokud chcete aktualizovat, můžete nahrát nový balíček služby nebo konfigurační soubor služby.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-112">To update, you can upload a new service package or service configuration file.</span></span>

1. <span data-ttu-id="b3dbc-113">V [portál Azure][Azure portal], vyberte cloudovou službu, kterou chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-113">In the [Azure portal][Azure portal], select the cloud service you want to update.</span></span> <span data-ttu-id="b3dbc-114">Tento krok se otevře okno instance služby pro cloud.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-114">This step opens the cloud service instance blade.</span></span>
2. <span data-ttu-id="b3dbc-115">V okně klikněte **aktualizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-115">In the blade, click the **Update** button.</span></span>

    ![Tlačítko Aktualizovat](./media/cloud-services-how-to-manage-portal/update-button.png)

3. <span data-ttu-id="b3dbc-117">Aktualizujte nasazení nového souboru balíku služeb (.cspkg) a konfigurační soubor služby (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-117">Update the deployment with a new service package file (.cspkg) and service configuration file (.cscfg).</span></span>

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. <span data-ttu-id="b3dbc-119">**Volitelně** aktualizovat označení nasazení a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-119">**Optionally** update the deployment label and the storage account.</span></span>
5. <span data-ttu-id="b3dbc-120">Pokud žádné role mít pouze jednu instanci role, vyberte **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** povolit upgradu pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-120">If any roles have only one role instance, select the **Deploy even if one or more roles contain a single instance** to enable the upgrade to proceed.</span></span>

    <span data-ttu-id="b3dbc-121">Azure můžete pouze zaručit 99,95 % dostupnost služby při aktualizaci služby cloud pokud každá role má aspoň dvě instance role (virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-121">Azure can only guarantee 99.95 percent service availability during a cloud service update if each role has at least two role instances (virtual machines).</span></span> <span data-ttu-id="b3dbc-122">Se dvěma instancemi role jeden virtuální počítač zpracovává požadavky na klienta během dalších je aktualizována.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-122">With two role instances, one virtual machine processes client requests while the other is updated.</span></span>

6. <span data-ttu-id="b3dbc-123">Zkontrolujte **spuštění nasazení** tak, aby měl použijí po dokončení nahrávání balíčku aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-123">Check **Start deployment** to have the update applied after the upload of the package has finished.</span></span>
7. <span data-ttu-id="b3dbc-124">Klikněte na tlačítko **OK** spustíte aktualizaci služby.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-124">Click **OK** to begin updating the service.</span></span>

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a><span data-ttu-id="b3dbc-125">Postupy: Prohodit nasazení na podporu dvoufázového nasazení do produkčního prostředí</span><span class="sxs-lookup"><span data-stu-id="b3dbc-125">How to: Swap deployments to promote a staged deployment to production</span></span>
<span data-ttu-id="b3dbc-126">Pokud se rozhodnete nasadit novou verzi cloudové služby, fáze a otestovat novou verzi v prostředí s pracovní cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-126">When you decide to deploy a new release of a cloud service, stage and test your new release in your cloud service staging environment.</span></span> <span data-ttu-id="b3dbc-127">Použití **Prohodit** přepnout adresy URL, které dvě nasazení se podrobněji a povyšte novou verzí do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-127">Use **Swap** to switch the URLs by which the two deployments are addressed and promote a new release to production.</span></span>

<span data-ttu-id="b3dbc-128">Můžete zaměnit nasazení z **cloudové služby** stránku nebo řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-128">You can swap deployments from the **Cloud Services** page or the dashboard.</span></span>

1. <span data-ttu-id="b3dbc-129">V [portál Azure][Azure portal], vyberte cloudovou službu, kterou chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-129">In the [Azure portal][Azure portal], select the cloud service you want to update.</span></span> <span data-ttu-id="b3dbc-130">Tento krok se otevře okno instance služby pro cloud.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-130">This step opens the cloud service instance blade.</span></span>
2. <span data-ttu-id="b3dbc-131">V okně klikněte **Prohodit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-131">In the blade, click the **Swap** button.</span></span>

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. <span data-ttu-id="b3dbc-133">Otevře se následující potvrzovací výzvu.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-133">The following confirmation prompt opens.</span></span>

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. <span data-ttu-id="b3dbc-135">Po ověření, informace o nasazení, klikněte na tlačítko **OK** chcete Prohodit nasazení.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-135">After you verify the deployment information, click **OK** to swap the deployments.</span></span>

    <span data-ttu-id="b3dbc-136">Swap nasazení dochází rychle jediné, co se změní je virtuální IP adresy (VIP) pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-136">The deployment swap happens quickly because the only thing that changes is the virtual IP addresses (VIPs) for the deployments.</span></span>

    <span data-ttu-id="b3dbc-137">Pokud chcete uložit výpočetní náklady, můžete odstranit pracovní nasazení po ověření, že produkčního nasazení funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-137">To save compute costs, you can delete the staging deployment after you verify that your production deployment is working as expected.</span></span>

### <a name="common-questions-about-swapping-deployments"></a><span data-ttu-id="b3dbc-138">Časté otázky týkající se nasazení vzájemná záměna</span><span class="sxs-lookup"><span data-stu-id="b3dbc-138">Common questions about swapping deployments</span></span>

<span data-ttu-id="b3dbc-139">**Jaké jsou požadavky pro odkládací nasazení?**</span><span class="sxs-lookup"><span data-stu-id="b3dbc-139">**What are the prerequisites for swapping deployments?**</span></span>

<span data-ttu-id="b3dbc-140">Existují dva klíče požadavky pro úspěšné nasazení prohození:</span><span class="sxs-lookup"><span data-stu-id="b3dbc-140">There are two key prerequisites for a successful deployment swap:</span></span>

- <span data-ttu-id="b3dbc-141">Pokud chcete používat statickou IP adresu pro vaše produkční slot, jeden pro přípravný slot také musíte rezervovat.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-141">If you would like to use a static IP address for your production slot, you must reserve one for your staging slot as well.</span></span> <span data-ttu-id="b3dbc-142">Prohození, jinak selže.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-142">Otherwise, the swap fails.</span></span>

- <span data-ttu-id="b3dbc-143">Všechny instance role musí být spuštěna, než bude možné provést prohození.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-143">All instances of your roles must be running before you can perform the swap.</span></span> <span data-ttu-id="b3dbc-144">Můžete zkontrolovat stav vašimi instancemi v okně Přehled portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-144">You can check the status of your instances in the overview blade of the Azure portal.</span></span> <span data-ttu-id="b3dbc-145">Alternativně můžete použít [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) příkazu v prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-145">Alternatively, you can use the [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) command in Windows PowerShell.</span></span>

<span data-ttu-id="b3dbc-146">Všimněte si, že hostovaného operačního systému, aktualizace a opravy operace služby může také způsobit nasazení záměna selhání.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-146">Note that Guest OS updates and service healing operations can also cause deployment swaps to fail.</span></span> <span data-ttu-id="b3dbc-147">Další informace najdete v tématu [Poradce při potížích se nasazení služby cloud](cloud-services-troubleshoot-deployment-problems.md).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-147">For more information, see [Troubleshoot cloud service deployment problems](cloud-services-troubleshoot-deployment-problems.md).</span></span>

<span data-ttu-id="b3dbc-148">**Nesnižuje prohození výpadek své aplikaci? Jak by měly zpracovávat ho?**</span><span class="sxs-lookup"><span data-stu-id="b3dbc-148">**Does a swap incur downtime for my application? How should I handle it?**</span></span>

<span data-ttu-id="b3dbc-149">Jak je popsáno v poslední části, je obvykle rychlé prohození nasazení, protože je právě změnu konfigurace pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-149">As described in the last section, a deployment swap is typically fast since it is just a configuration change in the Azure load balancer.</span></span> <span data-ttu-id="b3dbc-150">V některých případech ale ho můžete trvat deset nebo víc sekund a způsobit selhání přechodný připojení.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-150">In some cases, however, it can take ten or more seconds and result in transient connection failures.</span></span> <span data-ttu-id="b3dbc-151">Chcete-li omezit vliv na vaše zákazníky, zvažte implementaci [logika opakovaných pokusů klienta](../best-practices-retry-general.md).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-151">To limit impact to your customers, consider implementing [client retry logic](../best-practices-retry-general.md).</span></span>

## <a name="how-to-link-a-resource-to-a-cloud-service"></a><span data-ttu-id="b3dbc-152">Postupy: odkaz prostředek cloudové služby</span><span class="sxs-lookup"><span data-stu-id="b3dbc-152">How to: Link a resource to a cloud service</span></span>
<span data-ttu-id="b3dbc-153">Portál Azure neobsahuje odkazy prostředky společně jako nemá na aktuálním portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-153">The Azure portal does not link resources together like the current Azure classic portal does.</span></span> <span data-ttu-id="b3dbc-154">Místo toho nasaďte další prostředky do stejné skupiny prostředků používá cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-154">Instead, deploy additional resources to the same resource group being used by the Cloud Service.</span></span>

## <a name="how-to-delete-deployments-and-a-cloud-service"></a><span data-ttu-id="b3dbc-155">Postupy: odstranění nasazení a cloudové služby</span><span class="sxs-lookup"><span data-stu-id="b3dbc-155">How to: Delete deployments and a cloud service</span></span>
<span data-ttu-id="b3dbc-156">Před odstraněním cloudové služby, je nutné odstranit každé existující nasazení.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-156">Before you can delete a cloud service, you must delete each existing deployment.</span></span>

<span data-ttu-id="b3dbc-157">Pokud chcete uložit výpočetní náklady, můžete odstranit pracovní nasazení po ověření, že produkčního nasazení funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-157">To save compute costs, you can delete the staging deployment after you verify that your production deployment is working as expected.</span></span> <span data-ttu-id="b3dbc-158">Fakturuje se výpočetní náklady pro instance nasazené rolí, které jsou zastaveny.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-158">You are billed for compute costs for deployed role instances that are stopped.</span></span>

<span data-ttu-id="b3dbc-159">Pomocí následujícího postupu můžete odstranit nasazení nebo cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-159">Use the following procedure to delete a deployment or your cloud service.</span></span>

1. <span data-ttu-id="b3dbc-160">V [portál Azure][Azure portal], vyberte cloudovou službu, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-160">In the [Azure portal][Azure portal], select the cloud service you want to delete.</span></span> <span data-ttu-id="b3dbc-161">Tento krok se otevře okno instance služby pro cloud.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-161">This step opens the cloud service instance blade.</span></span>
2. <span data-ttu-id="b3dbc-162">V okně klikněte **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-162">In the blade, click the **Delete** button.</span></span>

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. <span data-ttu-id="b3dbc-164">Celý cloudové služby můžete odstranit kontrolou **Cloudová služba a její nasazení** nebo zvolit buď **produkční nasazení** nebo **pracovní nasazení**.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-164">You can delete the entire cloud service by checking **Cloud service and its deployments** or choose either the **Production deployment** or the **Staging deployment**.</span></span>

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. <span data-ttu-id="b3dbc-166">Klikněte **odstranit** tlačítko dole.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-166">Click the **Delete** button at the bottom.</span></span>
5. <span data-ttu-id="b3dbc-167">Chcete-li odstranit cloudovou službu, klikněte na tlačítko **odstranění Cloudová služba**.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-167">To delete the cloud service, click **Delete cloud service**.</span></span> <span data-ttu-id="b3dbc-168">Potom v potvrzovací výzvu, klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-168">Then, at the confirmation prompt, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="b3dbc-169">Pokud Cloudová služba je Odstraněná a podrobné monitorování je nastavena, musíte odstranit data ručně z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-169">When a cloud service is deleted, and verbose monitoring is configured, you must delete the data manually from your storage account.</span></span> <span data-ttu-id="b3dbc-170">Informace o tom, kde najít metriky tabulky najdete v tématu [to](cloud-services-how-to-monitor.md) článku.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-170">For information about where to find the metrics tables, see [this](cloud-services-how-to-monitor.md) article.</span></span>


## <a name="how-to-find-more-information-about-failed-deployments"></a><span data-ttu-id="b3dbc-171">Postupy: Další informace o selhání nasazení</span><span class="sxs-lookup"><span data-stu-id="b3dbc-171">How to: Find more information about failed deployments</span></span>
<span data-ttu-id="b3dbc-172">**Přehled** okno obsahuje stavového řádku v horní části.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-172">The **Overview** blade has a status bar at the top.</span></span> <span data-ttu-id="b3dbc-173">Po kliknutí na tlačítko panelu, nové okno se zobrazí veškeré informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-173">When you click the bar, a new blade opens and displays any error information.</span></span> <span data-ttu-id="b3dbc-174">Pokud nasazení neobsahuje žádné chyby, v okně informace je prázdný.</span><span class="sxs-lookup"><span data-stu-id="b3dbc-174">If the deployment does not contain any errors, the information blade is blank.</span></span>

![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a><span data-ttu-id="b3dbc-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3dbc-176">Next steps</span></span>
* <span data-ttu-id="b3dbc-177">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-177">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="b3dbc-178">Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-178">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="b3dbc-179">Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-179">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="b3dbc-180">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b3dbc-180">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
