---
title: "aaaHow tooconfigure cloudové služby (portál) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure cloudových služeb v Azure. Další konfigurace tooupdate hello cloudové služby a konfigurace vzdáleného přístupu toorole instance. Tyto příklady použití hello portálu Azure."
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
ms.openlocfilehash: 969a08558473e8c79153192942bfda587eb5ada5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-cloud-services"></a><span data-ttu-id="7df22-105">Jak tooConfigure cloudových služeb</span><span class="sxs-lookup"><span data-stu-id="7df22-105">How tooConfigure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7df22-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7df22-106">Azure portal</span></span>](cloud-services-how-to-configure-portal.md)
> * [<span data-ttu-id="7df22-107">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="7df22-107">Azure classic portal</span></span>](cloud-services-how-to-configure.md)
>
>

<span data-ttu-id="7df22-108">Hello nejčastěji používaná nastavení pro cloudové služby můžete nakonfigurovat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7df22-108">You can configure hello most commonly used settings for a cloud service in hello Azure portal.</span></span> <span data-ttu-id="7df22-109">Nebo, pokud chcete tooupdate přímo, konfigurační soubory stáhnout tooupdate soubor konfigurace služby a pak nahrajte hello aktualizovat soubor a aktualizace hello Cloudová služba se změny konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="7df22-109">Or, if you like tooupdate your configuration files directly, download a service configuration file tooupdate, and then upload hello updated file and update hello cloud service with hello configuration changes.</span></span> <span data-ttu-id="7df22-110">V obou případech aktualizace konfigurace hello jsou instalováni tooall instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="7df22-110">Either way, hello configuration updates are pushed out tooall role instances.</span></span>

<span data-ttu-id="7df22-111">Můžete také spravovat hello instancí role cloudové služby nebo vzdálené plochy do nich.</span><span class="sxs-lookup"><span data-stu-id="7df22-111">You can also manage hello instances of your cloud service roles, or remote desktop into them.</span></span>

<span data-ttu-id="7df22-112">Azure můžete pouze zajistit dostupnost služby 99,95 % během hello aktualizace konfigurace Pokud máte aspoň dvě instance role pro každou roli.</span><span class="sxs-lookup"><span data-stu-id="7df22-112">Azure can only ensure 99.95 percent service availability during hello configuration updates if you have at least two role instances for every role.</span></span> <span data-ttu-id="7df22-113">Umožňující jeden virtuální počítač tooprocess klientských požadavků v průběhu aktualizace hello jiné.</span><span class="sxs-lookup"><span data-stu-id="7df22-113">That enables one virtual machine tooprocess client requests while hello other is being updated.</span></span> <span data-ttu-id="7df22-114">Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="7df22-114">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="change-a-cloud-service"></a><span data-ttu-id="7df22-115">Změnit cloudové služby</span><span class="sxs-lookup"><span data-stu-id="7df22-115">Change a cloud service</span></span>
<span data-ttu-id="7df22-116">Po otevření hello [portál Azure](https://portal.azure.com/), přejděte tooyour cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7df22-116">After opening hello [Azure portal](https://portal.azure.com/), navigate tooyour cloud service.</span></span> <span data-ttu-id="7df22-117">Tady můžete spravovat mnoho aspektů.</span><span class="sxs-lookup"><span data-stu-id="7df22-117">From here, you manage many aspects of it.</span></span>

![Nastavení stránky](./media/cloud-services-how-to-configure-portal/cloud-service.png)

<span data-ttu-id="7df22-119">Hello **nastavení** nebo **všechna nastavení** odkazy se otevře hello **nastavení** okno, kde můžete změnit hello **vlastnosti**, změňte hello **Konfigurace**, spravovat hello **certifikáty**, instalační program **výstrah pravidla**a spravovat hello **uživatelé** a které mají přístup toothis Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="7df22-119">hello **Settings** or **All settings** links will open up hello **Settings** blade where you can change hello **Properties**, change hello **Configuration**, manage hello **Certificates**, setup **Alert rules**, and manage hello **Users** who have access toothis cloud service.</span></span>

![Okno nastavení služby Azure cloud](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a><span data-ttu-id="7df22-121">Správa verzí hostovaného operačního systému</span><span class="sxs-lookup"><span data-stu-id="7df22-121">Manage Guest OS version</span></span>

<span data-ttu-id="7df22-122">Ve výchozím nastavení Azure pravidelně aktualizuje hostovaného operačního systému toohello nejnovější podporovanou bitové kopie v rámci hello řada operačního systému, který jste zadali v konfiguraci služby (.cscfg), například Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="7df22-122">By default, Azure periodically updates your guest OS toohello latest supported image within hello OS family that you've specified in your service configuration (.cscfg), such as Windows Server 2016.</span></span>

<span data-ttu-id="7df22-123">Pokud potřebujete tootarget konkrétní verze operačního systému, můžete ho nastavit v hello **konfigurace** okno.</span><span class="sxs-lookup"><span data-stu-id="7df22-123">If you need tootarget a specific OS version, you can set it in hello **Configuration** blade.</span></span>

![Nastavit verzi operačního systému](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> <span data-ttu-id="7df22-125">Výběr konkrétní verze operačního systému zakáže automatické OS aktualizace a zajišťuje opravy vaší povinností.</span><span class="sxs-lookup"><span data-stu-id="7df22-125">Choosing a specific OS version disables automatic OS updates and makes patching your responsibility.</span></span> <span data-ttu-id="7df22-126">Ujistěte se, že instance role jsou přijímá aktualizace nebo může odhalí ohrožení zabezpečení vaší aplikace toosecurity.</span><span class="sxs-lookup"><span data-stu-id="7df22-126">You must ensure that your role instances are receiving updates or you may expose your application toosecurity vulnerabilities.</span></span>

## <a name="monitoring"></a><span data-ttu-id="7df22-127">Monitorování</span><span class="sxs-lookup"><span data-stu-id="7df22-127">Monitoring</span></span>
<span data-ttu-id="7df22-128">Můžete přidat výstrahy tooyour cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7df22-128">You can add alerts tooyour cloud service.</span></span> <span data-ttu-id="7df22-129">Klikněte na tlačítko **nastavení** > **pravidla výstrahy** > **přidat upozornění**.</span><span class="sxs-lookup"><span data-stu-id="7df22-129">Click **Settings** > **Alert Rules** > **Add alert**.</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

<span data-ttu-id="7df22-130">Zde můžete nastavit výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7df22-130">From here, you can setup an alert.</span></span> <span data-ttu-id="7df22-131">S hello **metrika** rozevíracího pole, můžete nastavit upozornění pro následující typy dat hello.</span><span class="sxs-lookup"><span data-stu-id="7df22-131">With hello **Metric** drop down box, you can setup an alert for hello following types of data.</span></span>

* <span data-ttu-id="7df22-132">Čtení z disku</span><span class="sxs-lookup"><span data-stu-id="7df22-132">Disk read</span></span>
* <span data-ttu-id="7df22-133">Zápis disku</span><span class="sxs-lookup"><span data-stu-id="7df22-133">Disk write</span></span>
* <span data-ttu-id="7df22-134">Sítě v</span><span class="sxs-lookup"><span data-stu-id="7df22-134">Network in</span></span>
* <span data-ttu-id="7df22-135">Sítě out</span><span class="sxs-lookup"><span data-stu-id="7df22-135">Network out</span></span>
* <span data-ttu-id="7df22-136">Procento CPU</span><span class="sxs-lookup"><span data-stu-id="7df22-136">CPU percentage</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a><span data-ttu-id="7df22-137">Konfigurace monitorování z metriky dlaždice</span><span class="sxs-lookup"><span data-stu-id="7df22-137">Configure monitoring from a metric tile</span></span>
<span data-ttu-id="7df22-138">Místo použití **nastavení** > **pravidla výstrah**, můžete kliknutím na jednu z hello metriky dlaždice v hello **monitorování** části hello **cloudu Služba** okno.</span><span class="sxs-lookup"><span data-stu-id="7df22-138">Instead of using **Settings** > **Alert Rules**, you can click on one of hello metric tiles in hello **Monitoring** section of hello **Cloud service** blade.</span></span>

![Cloudová služba monitorování](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

<span data-ttu-id="7df22-140">Tady můžete přizpůsobit graf hello použít s hello dlaždice nebo přidání pravidla výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7df22-140">From here you can customize hello chart used with hello tile, or add an alert rule.</span></span>

## <a name="reboot-reimage-or-remote-desktop"></a><span data-ttu-id="7df22-141">Restartování, obnovení z Image nebo vzdálené plochy</span><span class="sxs-lookup"><span data-stu-id="7df22-141">Reboot, reimage, or remote desktop</span></span>
<span data-ttu-id="7df22-142">V tuto chvíli nelze konfigurovat vzdálenou plochu pomocí hello **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="7df22-142">At this time you cannot configure remote desktop using hello **Azure portal**.</span></span> <span data-ttu-id="7df22-143">Však můžete nastavit ji prostřednictvím hello [portál Azure classic](cloud-services-role-enable-remote-desktop.md), [prostředí PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), nebo pomocí [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="7df22-143">However, you can set it up through hello [Azure classic portal](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), or through [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span></span>

<span data-ttu-id="7df22-144">První klikněte na instanci služby hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="7df22-144">First, click on hello cloud service instance.</span></span>

![Instance cloudové služby](./media/cloud-services-how-to-configure-portal/cs-instance.png)

<span data-ttu-id="7df22-146">Okno, které se otevře, můžete z hello můžete iniciovat připojení ke vzdálené ploše, vzdáleně restartovat hello instance, nebo vzdáleně obnovení z Image (začněte s novou bitovou kopii) hello.</span><span class="sxs-lookup"><span data-stu-id="7df22-146">From hello blade that opens you can initiate a remote desktop connection, remotely reboot hello instance, or remotely reimage (start with a fresh image) hello instance.</span></span>

![Cloudové služby Instance tlačítka](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a><span data-ttu-id="7df22-148">Překonfigurujte vaší .cscfg</span><span class="sxs-lookup"><span data-stu-id="7df22-148">Reconfigure your .cscfg</span></span>
<span data-ttu-id="7df22-149">Může být nutné tooreconfigure cloudové služby prostřednictvím hello [konfigurace služby (cscfg)](cloud-services-model-and-package.md#cscfg) souboru.</span><span class="sxs-lookup"><span data-stu-id="7df22-149">You may need tooreconfigure your cloud service through hello [service config (cscfg)](cloud-services-model-and-package.md#cscfg) file.</span></span> <span data-ttu-id="7df22-150">Nejdřív je potřeba toodownload vaše .cscfg souboru, upravte ho a pak nahrajte ho.</span><span class="sxs-lookup"><span data-stu-id="7df22-150">First you need toodownload your .cscfg file, modify it, then upload it.</span></span>

1. <span data-ttu-id="7df22-151">Klikněte na hello **nastavení** ikony nebo hello **všechna nastavení** odkaz tooopen až hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="7df22-151">Click on hello **Settings** icon or hello **All settings** link tooopen up hello **Settings** blade.</span></span>

    ![Nastavení stránky](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. <span data-ttu-id="7df22-153">Klikněte na hello **konfigurace** položky.</span><span class="sxs-lookup"><span data-stu-id="7df22-153">Click on hello **Configuration** item.</span></span>

    ![Okno Konfigurace](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. <span data-ttu-id="7df22-155">Klikněte na hello **Stáhnout** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7df22-155">Click on hello **Download** button.</span></span>

    ![Ke stažení](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. <span data-ttu-id="7df22-157">Po aktualizaci konfigurační soubor služby hello, odesílání a aktualizace konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="7df22-157">After you update hello service configuration file, upload and apply hello configuration updates:</span></span>

    ![Odeslat](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. <span data-ttu-id="7df22-159">Vyberte soubor .cscfg hello a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="7df22-159">Select hello .cscfg file and click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7df22-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7df22-160">Next steps</span></span>
* <span data-ttu-id="7df22-161">Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7df22-161">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="7df22-162">Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7df22-162">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="7df22-163">[Správa služby cloud](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7df22-163">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="7df22-164">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7df22-164">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
