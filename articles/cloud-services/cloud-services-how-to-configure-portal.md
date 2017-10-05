---
title: "Postup konfigurace cloudové služby (portál) | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat cloudové služby v Azure. Naučte se aktualizovat konfiguraci této cloudové služby a konfigurace vzdáleného přístupu k instancí rolí. Tyto příklady použití portálu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: a7e891d05ffe4cc2b4f68dce072a81499cc6de80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-cloud-services"></a><span data-ttu-id="2fad4-105">Postup konfigurace cloudové služby</span><span class="sxs-lookup"><span data-stu-id="2fad4-105">How to Configure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2fad4-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2fad4-106">Azure portal</span></span>](cloud-services-how-to-configure-portal.md)
> * [<span data-ttu-id="2fad4-107">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="2fad4-107">Azure classic portal</span></span>](cloud-services-how-to-configure.md)
>
>

<span data-ttu-id="2fad4-108">Můžete nakonfigurovat nastavení nejčastěji používaných pro cloudové služby na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2fad4-108">You can configure the most commonly used settings for a cloud service in the Azure portal.</span></span> <span data-ttu-id="2fad4-109">Pokud chcete místo toho aktualizovat konfigurační soubory přímo, stáhněte si příslušný konfigurační soubor služby a pak ho aktualizujte a nahrajte. Tím aktualizujete konfiguraci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2fad4-109">Or, if you like to update your configuration files directly, download a service configuration file to update, and then upload the updated file and update the cloud service with the configuration changes.</span></span> <span data-ttu-id="2fad4-110">V obou případech se aktualizovaná konfigurace projeví ve všech instancích rolí.</span><span class="sxs-lookup"><span data-stu-id="2fad4-110">Either way, the configuration updates are pushed out to all role instances.</span></span>

<span data-ttu-id="2fad4-111">Můžete také spravovat instance role cloudové služby nebo vzdálené plochy do nich.</span><span class="sxs-lookup"><span data-stu-id="2fad4-111">You can also manage the instances of your cloud service roles, or remote desktop into them.</span></span>

<span data-ttu-id="2fad4-112">Azure můžete pouze zajistit dostupnost služby 99,95 % během aktualizace konfigurace Pokud máte aspoň dvě instance role pro každou roli.</span><span class="sxs-lookup"><span data-stu-id="2fad4-112">Azure can only ensure 99.95 percent service availability during the configuration updates if you have at least two role instances for every role.</span></span> <span data-ttu-id="2fad4-113">Umožňující jeden virtuální počítač ke zpracování požadavků klientů dalších během aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2fad4-113">That enables one virtual machine to process client requests while the other is being updated.</span></span> <span data-ttu-id="2fad4-114">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="2fad4-114">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="change-a-cloud-service"></a><span data-ttu-id="2fad4-115">Změnit cloudové služby</span><span class="sxs-lookup"><span data-stu-id="2fad4-115">Change a cloud service</span></span>
<span data-ttu-id="2fad4-116">Po otevření [portál Azure](https://portal.azure.com/), přejděte do cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2fad4-116">After opening the [Azure portal](https://portal.azure.com/), navigate to your cloud service.</span></span> <span data-ttu-id="2fad4-117">Tady můžete spravovat mnoho aspektů.</span><span class="sxs-lookup"><span data-stu-id="2fad4-117">From here, you manage many aspects of it.</span></span>

![Nastavení stránky](./media/cloud-services-how-to-configure-portal/cloud-service.png)

<span data-ttu-id="2fad4-119">**Nastavení** nebo **všechna nastavení** se otevře odkazy **nastavení** okno, kde můžete změnit **vlastnosti**, změnit **konfigurace**, spravovat **certifikáty**, instalační program **výstrah pravidla**a spravovat **uživatelé** kteří mají přístup k této cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2fad4-119">The **Settings** or **All settings** links will open up the **Settings** blade where you can change the **Properties**, change the **Configuration**, manage the **Certificates**, setup **Alert rules**, and manage the **Users** who have access to this cloud service.</span></span>

![Okno nastavení služby Azure cloud](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a><span data-ttu-id="2fad4-121">Správa verzí hostovaného operačního systému</span><span class="sxs-lookup"><span data-stu-id="2fad4-121">Manage Guest OS version</span></span>

<span data-ttu-id="2fad4-122">Ve výchozím nastavení Azure pravidelně aktualizuje hostovaný operační systém na nejnovější podporovanou bitovou kopii v rámci řada operačního systému, který jste zadali v konfiguraci služby (.cscfg), například Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="2fad4-122">By default, Azure periodically updates your guest OS to the latest supported image within the OS family that you've specified in your service configuration (.cscfg), such as Windows Server 2016.</span></span>

<span data-ttu-id="2fad4-123">Pokud budete se muset zaměřit na konkrétní verzi operačního systému, můžete ho nastavit **konfigurace** okno.</span><span class="sxs-lookup"><span data-stu-id="2fad4-123">If you need to target a specific OS version, you can set it in the **Configuration** blade.</span></span>

![Nastavit verzi operačního systému](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> <span data-ttu-id="2fad4-125">Výběr konkrétní verze operačního systému zakáže automatické OS aktualizace a zajišťuje opravy vaší povinností.</span><span class="sxs-lookup"><span data-stu-id="2fad4-125">Choosing a specific OS version disables automatic OS updates and makes patching your responsibility.</span></span> <span data-ttu-id="2fad4-126">Ujistěte se, že instance role jsou přijímá aktualizace nebo může odhalí aplikace k ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2fad4-126">You must ensure that your role instances are receiving updates or you may expose your application to security vulnerabilities.</span></span>

## <a name="monitoring"></a><span data-ttu-id="2fad4-127">Monitorování</span><span class="sxs-lookup"><span data-stu-id="2fad4-127">Monitoring</span></span>
<span data-ttu-id="2fad4-128">Výstrahy můžete přidat do cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2fad4-128">You can add alerts to your cloud service.</span></span> <span data-ttu-id="2fad4-129">Klikněte na tlačítko **nastavení** > **pravidla výstrahy** > **přidat upozornění**.</span><span class="sxs-lookup"><span data-stu-id="2fad4-129">Click **Settings** > **Alert Rules** > **Add alert**.</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

<span data-ttu-id="2fad4-130">Zde můžete nastavit výstrahy.</span><span class="sxs-lookup"><span data-stu-id="2fad4-130">From here, you can setup an alert.</span></span> <span data-ttu-id="2fad4-131">Pomocí **metrika** rozevíracího pole, můžete nastavit upozornění pro následující typy dat.</span><span class="sxs-lookup"><span data-stu-id="2fad4-131">With the **Metric** drop down box, you can setup an alert for the following types of data.</span></span>

* <span data-ttu-id="2fad4-132">Čtení z disku</span><span class="sxs-lookup"><span data-stu-id="2fad4-132">Disk read</span></span>
* <span data-ttu-id="2fad4-133">Zápis disku</span><span class="sxs-lookup"><span data-stu-id="2fad4-133">Disk write</span></span>
* <span data-ttu-id="2fad4-134">Sítě v</span><span class="sxs-lookup"><span data-stu-id="2fad4-134">Network in</span></span>
* <span data-ttu-id="2fad4-135">Sítě out</span><span class="sxs-lookup"><span data-stu-id="2fad4-135">Network out</span></span>
* <span data-ttu-id="2fad4-136">Procento CPU</span><span class="sxs-lookup"><span data-stu-id="2fad4-136">CPU percentage</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a><span data-ttu-id="2fad4-137">Konfigurace monitorování z metriky dlaždice</span><span class="sxs-lookup"><span data-stu-id="2fad4-137">Configure monitoring from a metric tile</span></span>
<span data-ttu-id="2fad4-138">Místo použití **nastavení** > **pravidla výstrah**, můžete kliknutím na jednu z metriky dlaždice v **monitorování** části **Cloudová služba** okno.</span><span class="sxs-lookup"><span data-stu-id="2fad4-138">Instead of using **Settings** > **Alert Rules**, you can click on one of the metric tiles in the **Monitoring** section of the **Cloud service** blade.</span></span>

![Cloudová služba monitorování](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

<span data-ttu-id="2fad4-140">Tady můžete přizpůsobit graf použít s dlaždice nebo přidání pravidla výstrahy.</span><span class="sxs-lookup"><span data-stu-id="2fad4-140">From here you can customize the chart used with the tile, or add an alert rule.</span></span>

## <a name="reboot-reimage-or-remote-desktop"></a><span data-ttu-id="2fad4-141">Restartování, obnovení z Image nebo vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="2fad4-141">Reboot, reimage, or remote desktop</span></span>
<span data-ttu-id="2fad4-142">V tuto chvíli nelze konfigurovat pomocí vzdálené plochy **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="2fad4-142">At this time you cannot configure remote desktop using the **Azure portal**.</span></span> <span data-ttu-id="2fad4-143">Však můžete nastavit ji prostřednictvím [portál Azure classic](cloud-services-role-enable-remote-desktop.md), [prostředí PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), nebo pomocí [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="2fad4-143">However, you can set it up through the [Azure classic portal](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), or through [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span></span>

<span data-ttu-id="2fad4-144">První klikněte na instanci služby cloudu.</span><span class="sxs-lookup"><span data-stu-id="2fad4-144">First, click on the cloud service instance.</span></span>

![Instance cloudové služby](./media/cloud-services-how-to-configure-portal/cs-instance.png)

<span data-ttu-id="2fad4-146">V okně, otevře se iniciovat připojení ke vzdálené ploše, vzdáleně restartovat instanci nebo vzdáleně obnovit z Image (začínat novou bitovou kopii) instance.</span><span class="sxs-lookup"><span data-stu-id="2fad4-146">From the blade that opens you can initiate a remote desktop connection, remotely reboot the instance, or remotely reimage (start with a fresh image) the instance.</span></span>

![Cloudové služby Instance tlačítka](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a><span data-ttu-id="2fad4-148">Překonfigurujte vaší .cscfg</span><span class="sxs-lookup"><span data-stu-id="2fad4-148">Reconfigure your .cscfg</span></span>
<span data-ttu-id="2fad4-149">Budete muset překonfigurovat cloudové služby prostřednictvím [konfigurace služby (cscfg)](cloud-services-model-and-package.md#cscfg) souboru.</span><span class="sxs-lookup"><span data-stu-id="2fad4-149">You may need to reconfigure your cloud service through the [service config (cscfg)](cloud-services-model-and-package.md#cscfg) file.</span></span> <span data-ttu-id="2fad4-150">Je třeba nejprve stáhněte soubor .cscfg, upravte ho a pak nahrajte ho.</span><span class="sxs-lookup"><span data-stu-id="2fad4-150">First you need to download your .cscfg file, modify it, then upload it.</span></span>

1. <span data-ttu-id="2fad4-151">Klikněte na **nastavení** ikonu nebo **všechna nastavení** odkaz otevře **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="2fad4-151">Click on the **Settings** icon or the **All settings** link to open up the **Settings** blade.</span></span>

    ![Nastavení stránky](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. <span data-ttu-id="2fad4-153">Klikněte na **konfigurace** položky.</span><span class="sxs-lookup"><span data-stu-id="2fad4-153">Click on the **Configuration** item.</span></span>

    ![Okno Konfigurace](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. <span data-ttu-id="2fad4-155">Klikněte na **Stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2fad4-155">Click on the **Download** button.</span></span>

    ![Ke stažení](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. <span data-ttu-id="2fad4-157">Po aktualizaci konfigurační soubor služby, odesílání a aktualizace konfigurace:</span><span class="sxs-lookup"><span data-stu-id="2fad4-157">After you update the service configuration file, upload and apply the configuration updates:</span></span>

    ![Odeslat](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. <span data-ttu-id="2fad4-159">Vyberte soubor .cscfg a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2fad4-159">Select the .cscfg file and click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fad4-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2fad4-160">Next steps</span></span>
* <span data-ttu-id="2fad4-161">Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2fad4-161">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="2fad4-162">Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2fad4-162">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="2fad4-163">[Správa služby cloud](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2fad4-163">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="2fad4-164">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2fad4-164">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
