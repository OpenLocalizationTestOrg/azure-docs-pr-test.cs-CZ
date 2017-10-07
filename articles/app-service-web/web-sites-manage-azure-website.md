---
title: "aaaManage webové aplikace ve službě Azure App Service"
description: "Odkazy tooresources pro správu webové aplikace ve službě Azure App Service."
services: app-service\web
documentationcenter: 
author: erikre
manager: erikre
editor: 
ms.assetid: d5e2887a-84f9-4747-a573-867635cb8b39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: rachelap
ms.openlocfilehash: daf69245e66068b0e97e3ae1c3fb5fce45605b91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a><span data-ttu-id="95bf0-103">Správa webové aplikace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="95bf0-103">Manage a web app in Azure App Service</span></span>
<span data-ttu-id="95bf0-104">Toto téma obsahuje odkazy tooresources pro správu webové aplikace ve [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="95bf0-104">This topic contains links tooresources for managing a web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="95bf0-105">Správa zahrnuje všechny hello úlohy, které udržují vaší webové aplikace plynulý chod.</span><span class="sxs-lookup"><span data-stu-id="95bf0-105">Management includes all of hello tasks that keep your web app running smoothly.</span></span> 

<span data-ttu-id="95bf0-106">Průběhu životnosti hello webové aplikace budete provádět úlohy správy různých, jako jste odpojili od počátečního nasazení toonormal operace, Údržba a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="95bf0-106">Over hello lifetime of a web app, you will perform different management tasks, as you move from initial deployment toonormal operation, maintenance, and updates.</span></span>

<span data-ttu-id="95bf0-107">Mnoho úloh správy webové aplikace lze provést v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="95bf0-107">Many web app management tasks can be performed in hello Azure Portal.</span></span>

## <a name="before-you-deploy-your-web-app-tooproduction"></a><span data-ttu-id="95bf0-108">Před nasazením tooproduction vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="95bf0-108">Before you deploy your web app tooproduction</span></span>
### <a name="choose-a-tier"></a><span data-ttu-id="95bf0-109">Zvolte úroveň</span><span class="sxs-lookup"><span data-stu-id="95bf0-109">Choose a tier</span></span>
<span data-ttu-id="95bf0-110">Aplikační služba Azure je k dispozici v pěti vrstev: Free, sdílené, Basic, Standard a Premium.</span><span class="sxs-lookup"><span data-stu-id="95bf0-110">Azure App Service is offered in five tiers: Free, Shared, Basic, Standard, and Premium.</span></span> <span data-ttu-id="95bf0-111">Informace o funkcích hello a ceny pro každou vrstvu najdete v tématu [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="95bf0-111">For information about hello features and pricing for each tier, see [Pricing details](https://azure.microsoft.com/pricing/details/app-service/).</span></span> 

* <span data-ttu-id="95bf0-112">[Plánů služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) umožňují seskupit více webových aplikací v rámci hello stejné vrstvě.</span><span class="sxs-lookup"><span data-stu-id="95bf0-112">[App Service plans](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) let you group multiple web apps under hello same tier.</span></span>
* <span data-ttu-id="95bf0-113">Vždy můžete [přepínač vrstvy](web-sites-scale.md) po vytvoření webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="95bf0-113">You can always [switch tiers](web-sites-scale.md) after you create your web app.</span></span>

### <a name="configuration"></a><span data-ttu-id="95bf0-114">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="95bf0-114">Configuration</span></span>
<span data-ttu-id="95bf0-115">Použití hello [portálu Azure](https://portal.azure.com/) tooset možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="95bf0-115">Use hello [Azure Portal](https://portal.azure.com/) tooset various configuration options.</span></span> <span data-ttu-id="95bf0-116">Podrobnosti najdete v tématu [konfigurace webových aplikací ve službě Azure App Service](web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="95bf0-116">For details, see [Configure web apps in Azure App Service](web-sites-configure.md).</span></span> <span data-ttu-id="95bf0-117">Tady je rychlé kontrolní seznam:</span><span class="sxs-lookup"><span data-stu-id="95bf0-117">Here is a quick checklist:</span></span>

* <span data-ttu-id="95bf0-118">Vyberte **runtime verze** pro rozhraní .NET, PHP, Java nebo Python, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="95bf0-118">Select **runtime versions** for .NET, PHP, Java, or Python, if needed.</span></span>
* <span data-ttu-id="95bf0-119">Povolit **Websocket** Pokud vaše webová aplikace používá protokol WebSocket hello.</span><span class="sxs-lookup"><span data-stu-id="95bf0-119">Enable **WebSockets** if your web app uses hello WebSocket protocol.</span></span> <span data-ttu-id="95bf0-120">(To zahrnuje aplikace, které používají [ASP.NET SignalR](http://www.asp.net/signalr) nebo [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span><span class="sxs-lookup"><span data-stu-id="95bf0-120">(This includes apps that use [ASP.NET SignalR](http://www.asp.net/signalr) or [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span></span>
* <span data-ttu-id="95bf0-121">Běží nepřetržité webové úlohy?</span><span class="sxs-lookup"><span data-stu-id="95bf0-121">Are you running continuous web jobs?</span></span> <span data-ttu-id="95bf0-122">Pokud ano, povolit **Always On**.</span><span class="sxs-lookup"><span data-stu-id="95bf0-122">If so, enable **Always On**.</span></span>
* <span data-ttu-id="95bf0-123">Sada hello **výchozí dokument**, jako je například index.html.</span><span class="sxs-lookup"><span data-stu-id="95bf0-123">Set hello **default document**, such as index.html.</span></span>

<span data-ttu-id="95bf0-124">V přidání toothese základní nastavení konfigurace může být vhodné tooconfigure hello následující:</span><span class="sxs-lookup"><span data-stu-id="95bf0-124">In addition toothese basic configuration settings, you may want tooconfigure hello following:</span></span>

* <span data-ttu-id="95bf0-125">**Secure Socket Layer (SSL)** šifrování.</span><span class="sxs-lookup"><span data-stu-id="95bf0-125">**Secure Socket Layer (SSL)** encryption.</span></span> <span data-ttu-id="95bf0-126">toouse SSL s vlastní název domény, musíte získat protokolem SSL certifikátů a konfigurace vaší webové aplikace toouse ho.</span><span class="sxs-lookup"><span data-stu-id="95bf0-126">toouse SSL with a custom domain name, you must get an SSL certificate and configure your web app toouse it.</span></span> <span data-ttu-id="95bf0-127">V tématu [povolit HTTPS pro webové aplikace ve službě Azure App Service](app-service-web-tutorial-custom-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="95bf0-127">See [Enable HTTPS for a web app in Azure App Service](app-service-web-tutorial-custom-ssl.md).</span></span>
* <span data-ttu-id="95bf0-128">**Vlastní název domény.**</span><span class="sxs-lookup"><span data-stu-id="95bf0-128">**Custom domain name.**</span></span> <span data-ttu-id="95bf0-129">Webové aplikace automaticky má subdoména pod azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="95bf0-129">Your web app automatically has a subdomain under azurewebsites.net.</span></span> <span data-ttu-id="95bf0-130">Můžete přidružit vlastní název domény, například contoso.com. V tématu [konfigurace vlastního názvu domény v Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="95bf0-130">You can associate a custom domain name, such as contoso.com. See [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span>

<span data-ttu-id="95bf0-131">Konfigurace pro specifický jazyk:</span><span class="sxs-lookup"><span data-stu-id="95bf0-131">Language-specific configuration:</span></span>

* <span data-ttu-id="95bf0-132">**PHP**: [nakonfigurovat PHP ve službě Azure App Service Web Apps](web-sites-php-configure.md).</span><span class="sxs-lookup"><span data-stu-id="95bf0-132">**PHP**: [Configure PHP in Azure App Service Web Apps](web-sites-php-configure.md).</span></span>
* <span data-ttu-id="95bf0-133">**Python**: [konfigurace Python službou Azure App Service webové aplikace](web-sites-python-configure.md)</span><span class="sxs-lookup"><span data-stu-id="95bf0-133">**Python**: [Configuring Python with Azure App Service Web Apps](web-sites-python-configure.md)</span></span>

## <a name="while-your-web-app-is-running"></a><span data-ttu-id="95bf0-134">Když běží vaše webová aplikace</span><span class="sxs-lookup"><span data-stu-id="95bf0-134">While your web app is running</span></span>
<span data-ttu-id="95bf0-135">Když běží vaše webová aplikace, budete chtít toomake, že je k dispozici, a že se škáluje provozu generovaného uživateli toomeet.</span><span class="sxs-lookup"><span data-stu-id="95bf0-135">While your web app is running, you want toomake sure it is available, and that it scales toomeet user traffic.</span></span> <span data-ttu-id="95bf0-136">Také můžete potřebovat tootroubleshoot chyby.</span><span class="sxs-lookup"><span data-stu-id="95bf0-136">You may also need tootroubleshoot errors.</span></span>

### <a name="monitoring"></a><span data-ttu-id="95bf0-137">Monitorování</span><span class="sxs-lookup"><span data-stu-id="95bf0-137">Monitoring</span></span>
* <span data-ttu-id="95bf0-138">Prostřednictvím hello portálu Azure, můžete [přidat metriky výkonu](web-sites-monitor.md) například využití procesoru a počet klientských požadavků.</span><span class="sxs-lookup"><span data-stu-id="95bf0-138">Through hello Azure Portal, you can [add performance metrics](web-sites-monitor.md) such as CPU usage and number of client requests.</span></span>
* <span data-ttu-id="95bf0-139">[Škálování webové aplikace](web-sites-scale.md) v tootraffic odpovědi.</span><span class="sxs-lookup"><span data-stu-id="95bf0-139">[Scale your web app](web-sites-scale.md) in response tootraffic.</span></span> <span data-ttu-id="95bf0-140">V závislosti na úrovni je možné škálovat hello počet virtuálních počítačů nebo velikost hello hello instance virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="95bf0-140">Depending on your tier, you can scale hello number of VMs and/or hello size of hello VM instances.</span></span> <span data-ttu-id="95bf0-141">V hello Standard a Premium vrstvy můžete také nastavíte automatické škálování, tak škáluje vaší webové aplikace automaticky podle pevného plánu nebo v tooload odpovědi.</span><span class="sxs-lookup"><span data-stu-id="95bf0-141">In hello Standard and Premium tiers, you can also set up autoscaling, so your web app scales automatically, either on a fixed schedule, or in response tooload.</span></span>  

### <a name="backups"></a><span data-ttu-id="95bf0-142">Zálohování</span><span class="sxs-lookup"><span data-stu-id="95bf0-142">Backups</span></span>
* <span data-ttu-id="95bf0-143">Nastavit [automatické zálohování](web-sites-backup.md) vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="95bf0-143">Set [automatic backups](web-sites-backup.md) of your web app.</span></span> <span data-ttu-id="95bf0-144">Další informace o zálohování v [toto video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span><span class="sxs-lookup"><span data-stu-id="95bf0-144">Learn more about backups in [this video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span></span>
* <span data-ttu-id="95bf0-145">Další informace o možnosti hello [obnovení databáze](../sql-database/sql-database-business-continuity.md) ve službě Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="95bf0-145">Learn about hello options for [database recovery](../sql-database/sql-database-business-continuity.md) in Azure SQL Database.</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="95bf0-146">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="95bf0-146">Troubleshooting</span></span>
* <span data-ttu-id="95bf0-147">Pokud dojde k chybě, můžete [řešení v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), pomocí protokolů diagnostiky a živé ladění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="95bf0-147">If something goes wrong, you can [troubleshoot in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), using diagnostic logs and live debugging in hello cloud.</span></span> 
* <span data-ttu-id="95bf0-148">Mimo Visual Studio existují různé způsoby toocollect diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="95bf0-148">Outside of Visual Studio, there are various ways toocollect diagnostic logs.</span></span> <span data-ttu-id="95bf0-149">V tématu [povolit protokolování diagnostiky pro webové aplikace v Azure App Service](web-sites-enable-diagnostic-log.md).</span><span class="sxs-lookup"><span data-stu-id="95bf0-149">See [Enable diagnostics logging for web apps in Azure App Service](web-sites-enable-diagnostic-log.md).</span></span>
* <span data-ttu-id="95bf0-150">Aplikace Node.js, naleznete v [jak toodebug Node.js webové aplikace v Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="95bf0-150">For Node.js applications, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

### <a name="restoring-data"></a><span data-ttu-id="95bf0-151">Obnovení dat</span><span class="sxs-lookup"><span data-stu-id="95bf0-151">Restoring Data</span></span>
* <span data-ttu-id="95bf0-152">[Obnovit](web-sites-restore.md) webovou aplikaci, která byla dříve zálohovaná.</span><span class="sxs-lookup"><span data-stu-id="95bf0-152">[Restore](web-sites-restore.md) a web app that was previously backed up.</span></span>

## <a name="when-you-update-your-web-app"></a><span data-ttu-id="95bf0-153">Při aktualizaci webové aplikace</span><span class="sxs-lookup"><span data-stu-id="95bf0-153">When you update your web app</span></span>
<span data-ttu-id="95bf0-154">Pokud jste nepovolili automatické zálohování, můžete vytvořit [ruční zálohy](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="95bf0-154">If you have not enabled automatic backups, you can create a [manual backup](web-sites-backup.md).</span></span>

<span data-ttu-id="95bf0-155">Zvažte použití [připravený nasazení](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="95bf0-155">Consider using [staged deployment](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="95bf0-156">Tato možnost umožňuje publikovat aktualizace tooa pracovní nasazení, které běží souběžně sdílená s produkčním nasazení.</span><span class="sxs-lookup"><span data-stu-id="95bf0-156">This option lets you publish updates tooa staging deployment that runs side-by-side with your production deployment.</span></span> 


<!-- Anchors. -->

[Before you deploy your site tooproduction]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website


