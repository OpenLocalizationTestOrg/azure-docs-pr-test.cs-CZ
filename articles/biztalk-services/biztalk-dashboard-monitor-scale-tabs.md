---
title: "aaaDashboard, monitorování, měřítko, konfigurovat a hybridní připojení ve službě BizTalk Services | Microsoft Docs"
description: "Další informace o ovládacích prvcích hello a sledovat výkon na hello klasického portálu karty služby BizTalk Services: řídicí panel, sledování, škálování, konfigurovat a hybridní připojení. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c9fafdad20489571ee3849bbacd2c2b10933154f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="review-hello-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a><span data-ttu-id="f5315-104">Zkontrolujte hello karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="f5315-104">Review hello Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="f5315-105">Po vytvoření služby BizTalk a nasazení aplikace, můžete změnit některá nastavení služby BizTalk hello a monitorování výkonu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-105">After you create your BizTalk Service and deploy your application, you can change some of hello BizTalk Service settings and monitor hello application performance.</span></span> 

<span data-ttu-id="f5315-106">Když otevřete hello portál Azure classic, je automaticky umístí na hello **všechny položky** tooview kartě službě BizTalk, vyberte svoji službu BizTalk v hello **všechny položky** kartě nebo vyberte hello **BIZTALK SERVICES** kartě; a potom vyberte název vaší služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-106">When you open hello Azure classic portal, you are automatically placed at hello **ALL ITEMS** tab. tooview your BizTalk Service, select your BizTalk Service in hello **ALL ITEMS** tab or select hello **BIZTALK SERVICES** tab; and then select your BizTalk Service name.</span></span>

<span data-ttu-id="f5315-107">Otevře se nové okno s hello následující karty.</span><span class="sxs-lookup"><span data-stu-id="f5315-107">This opens a new window with hello following tabs.</span></span> <span data-ttu-id="f5315-108">Toto téma popisuje těchto karet.</span><span class="sxs-lookup"><span data-stu-id="f5315-108">This topic describes these tabs.</span></span>

## <a name="quickstart-quickstartquickstart"></a><span data-ttu-id="f5315-109">(Rychlý start</span><span class="sxs-lookup"><span data-stu-id="f5315-109">Quickstart (</span></span>![Rychlý start][Quickstart]<span data-ttu-id="f5315-111">)</span><span class="sxs-lookup"><span data-stu-id="f5315-111">)</span></span>
<span data-ttu-id="f5315-112">V závislosti na hello edici služby BizTalk nemusí být k dispozici všechny možnosti uvedené.</span><span class="sxs-lookup"><span data-stu-id="f5315-112">Depending on hello BizTalk Services Edition, all options listed may not be available.</span></span> 

<table border="1">
    <tr>
        <td><span data-ttu-id="f5315-113"><strong>Získat nástroje hello</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-113"><strong>Get hello tools</strong></span></span></td>
        <td><span data-ttu-id="f5315-114">Stáhněte si hello BizTalk Services SDK tooinstall hello šablony projektů Visual Studio ve svém vývojovém počítači místně.</span><span class="sxs-lookup"><span data-stu-id="f5315-114">Download hello BizTalk Services SDK tooinstall hello Visual Studio project templates on your on-premises development computer.</span></span> <span data-ttu-id="f5315-115">Tyto šablony vytvořit hello <strong>BizTalk Services</strong> (mostu) a hello <strong>artefaktů služby BizTalk</strong> projektů sady Visual Studio (transformace), které jsou nasazené tooyour služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-115">These templates create hello <strong>BizTalk Services</strong> (bridge) and hello <strong>BizTalk Service Artifacts</strong> (Transform) Visual Studio projects that are deployed tooyour BizTalk Service.</span></span>
        <br/><br/><span data-ttu-id="f5315-116">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Jak začít používat hello Azure BizTalk Services SDK </a> a <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">hello instalace sady SDK Azure BizTalk Services</a> seznamy hello kroky tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="f5315-116">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335"> How do I Start Using hello Azure BizTalk Services SDK </a> and <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Installing hello Azure BizTalk Services SDK</a> lists hello steps tooget started.</span></span>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="f5315-117"><strong>Vytvořte partnery smlouvy</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-117"><strong>Create partner agreements</strong></span></span></td>
        <td><span data-ttu-id="f5315-118">Otevře se okno hello portálu služby Azure BizTalk hostované v Azure, kde je přidat partnery a vytvořit X12 AS2, smlouvy EDIFACT EDI a.</span><span class="sxs-lookup"><span data-stu-id="f5315-118">Opens hello Azure BizTalk Services Portal hosted on Azure where you add partners and create X12, AS2, and EDIFACT EDI agreements.</span></span>
        <br/><br/><span data-ttu-id="f5315-119">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> seznamy hello kroky tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="f5315-119">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> lists hello steps tooget started.</span></span>
        </td>
    </tr>

<tr>
        <td><span data-ttu-id="f5315-120"><strong>Další informace o BizTalk Services</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-120"><strong>Learn more about BizTalk Services</strong></span></span></td>
        <td><span data-ttu-id="f5315-121">Přejděte toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">učení center</a> toolearn Další informace o Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="f5315-121">Go toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> toolearn more about Azure BizTalk Services.</span></span></td>
</tr>
</table>


<span data-ttu-id="f5315-122">Hello hlavním panelu v dolní části hello můžete:</span><span class="sxs-lookup"><span data-stu-id="f5315-122">In hello task bar at hello bottom, you can:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="f5315-123"><strong>Spravovat</strong> nasazení vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f5315-123"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="f5315-124">Otevřete portál služby Azure BizTalk Services hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-124">Opens hello Azure BizTalk Services portal.</span></span> <span data-ttu-id="f5315-125">Hello portál služeb BizTalk je hello vstupu tooEDI konfigurace, včetně přidání partnery a vytvoření X12 AS2, EDIFACT smlouvy a.</span><span class="sxs-lookup"><span data-stu-id="f5315-125">hello BizTalk Services Portal is hello entrance tooEDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="f5315-126">Toto je stejný jako hello <strong>vytvořte smlouvy partnera</strong> na hello <strong>rychlý Start</strong> kartě.</span><span class="sxs-lookup"><span data-stu-id="f5315-126">This is hello same as <strong>Create partner agreements</strong> on hello <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="f5315-127">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> poskytuje další informace o hello portál služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-127">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on hello BizTalk Services Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="f5315-128"><strong>Informace o připojení</strong> z hello Namespace řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="f5315-128"><strong>Connection Information</strong> of hello Access Control Namespace</span></span></td>
<td><span data-ttu-id="f5315-129">Když vyberete informace o připojení, potom hello Namespace řízení přístupu, výchozí vystavitele a výchozí klíč jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="f5315-129">When you select Connection Information, then hello Access Control Namespace, Default Issuer, and Default Key are displayed.</span></span> <span data-ttu-id="f5315-130">Můžete zkopírovat tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f5315-130">You can copy these values.</span></span>
<br/><br/>
<span data-ttu-id="f5315-131">Můžete také otevřít hello portál pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="f5315-131">You can also open hello Access Control Portal.</span></span> <span data-ttu-id="f5315-132"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Vytvoření ovládacího prvku přístup Namespace</a> poskytuje další informace o hello portál pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="f5315-132"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Create an Access control Namespace</a> provides more information on hello Access Control Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="f5315-133"><strong>Klíče synchronizovat</strong> v hello účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="f5315-133"><strong>Sync Keys</strong> in hello Storage Account</span></span></td>
<td><span data-ttu-id="f5315-134">Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-134">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="f5315-135">Tyto šifrovací klíče řízení přístupu tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5315-135">These encryption Keys control access tooyour Storage Account.</span></span> <span data-ttu-id="f5315-136">Vaše služba BizTalk automaticky používá hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-136">Your BizTalk Service automatically uses hello Primary Key.</span></span> <span data-ttu-id="f5315-137"><strong>Klíče synchronizovat</strong> povolit uživatelům tooswitch mezi hello primární klíč a sekundární klíč hello bez přerušení hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-137"><strong>Sync Keys</strong> enable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="f5315-138">Například chcete hello služby BizTalk toouse nový primární klíč pro hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5315-138">For example, you want hello BizTalk Service toouse a new Primary Key for hello Storage Account.</span></span> <span data-ttu-id="f5315-139">toodo toto:</span><span class="sxs-lookup"><span data-stu-id="f5315-139">toodo this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="f5315-140">Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>.</span><span class="sxs-lookup"><span data-stu-id="f5315-140">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="f5315-141">Vyberte hello sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-141">Select hello Secondary Key.</span></span> <span data-ttu-id="f5315-142">Když to uděláte, spustí hello služby BizTalk, pomocí hello sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-142">When you do this, hello BizTalk Service starts using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="f5315-143">V hello portál Azure classic vyberte svůj účet úložiště a obnovit hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-143">In hello Azure classic portal, select your Storage account and Regenerate hello Primary Key.</span></span> <span data-ttu-id="f5315-144">Pamatujte si, že se vaše služba BizTalk používá hello sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-144">Remember, your BizTalk Service is using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="f5315-145">Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>.</span><span class="sxs-lookup"><span data-stu-id="f5315-145">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="f5315-146">Nyní vyberte hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-146">Now, select hello Primary Key.</span></span> <span data-ttu-id="f5315-147">To je hello nové můžete znovu vygenerovat primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-147">This is hello new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="f5315-148">V hello portál Azure classic vyberte svůj účet úložiště a znovu vygenerovat sekundární klíč hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-148">In hello Azure classic portal, select your Storage account and Regenerate hello Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="f5315-149">Tento proces se nazývá "výměny klíčů".</span><span class="sxs-lookup"><span data-stu-id="f5315-149">This process is called "rollover keys".</span></span> <span data-ttu-id="f5315-150">účelem Hello je tooenable uživatelé tooswitch mezi hello primární klíč a sekundární klíč hello bez přerušení hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-150">hello purpose is tooenable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="f5315-151"><strong>Odstranit</strong> vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f5315-151"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="f5315-152">Když vyberete odstranit, vaše služba BizTalk a jsou odebrány všechny položky, které jsou nasazené tooit.</span><span class="sxs-lookup"><span data-stu-id="f5315-152">When you select Delete, your BizTalk Service and all items deployed tooit are removed.</span></span></td>
</tr>
</table>


## <a name="dashboard"></a><span data-ttu-id="f5315-153">Řídicí panel</span><span class="sxs-lookup"><span data-stu-id="f5315-153">Dashboard</span></span>
<span data-ttu-id="f5315-154">V závislosti na hello edici služby BizTalk nemusí být k dispozici všechny možnosti uvedené.</span><span class="sxs-lookup"><span data-stu-id="f5315-154">Depending on hello BizTalk Services Edition, all options listed may not be available.</span></span> 

<span data-ttu-id="f5315-155">Když vyberete název vaší služby BizTalk, zobrazí se karta řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-155">When you select your BizTalk Service name, hello Dashboard tab is displayed.</span></span> <span data-ttu-id="f5315-156">Na řídicím panelu můžete:</span><span class="sxs-lookup"><span data-stu-id="f5315-156">In Dashboard, you can:</span></span>

##### <a name="usage-overview-shows-hello-number-of-used-hybrid-connections"></a><span data-ttu-id="f5315-157">Přehled využití: Zobrazuje hello počet použitých hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="f5315-157">Usage Overview: Shows hello number of used Hybrid Connections</span></span>
<span data-ttu-id="f5315-158">Také zobrazuje hello využití dat v GB.</span><span class="sxs-lookup"><span data-stu-id="f5315-158">Also displays hello data usage in GB.</span></span> 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a><span data-ttu-id="f5315-159">Metriky grafu: Zobrazuje pevnou seznam metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="f5315-159">Metric Graph: Shows a fixed list of performance metrics</span></span>
<span data-ttu-id="f5315-160">Tyto metriky zadejte v reálném čase hodnoty týkající se stavu hello hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-160">These metrics provide real-time values regarding hello health of hello BizTalk Service.</span></span> <span data-ttu-id="f5315-161">Můžete také hello **relativní** nebo **absolutní** hodnoty a hello čas rozsah **Interval** hello metrik, která se zobrazí v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-161">You can also choose hello **Relative** or **Absolute** values and hello time range **Interval** of hello metrics that are displayed in hello graph.</span></span> 

<span data-ttu-id="f5315-162">Popis tyto metriky výkonu, přejděte příliš[dostupné metriky](#Metrics) v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="f5315-162">For a description of these performance metrics, go too[Available Metrics](#Metrics) in this topic.</span></span>

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a><span data-ttu-id="f5315-163">Rychlého přehledu: Seznamy vlastnosti vaší služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="f5315-163">Quick Glance: Lists your BizTalk Service properties</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="f5315-164"><strong>Aktualizujte přihlašovací údaje databáze sledování</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-164"><strong>Update Tracking Database credentials</strong></span></span></td>
<td><span data-ttu-id="f5315-165">Změny hello uživatelské jméno a heslo použít toolog do hello sledování databáze.</span><span class="sxs-lookup"><span data-stu-id="f5315-165">Changes hello user name and password used toolog into hello Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-166"><strong>Aktualizovat certifikát SSL</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-166"><strong>Update SSL Certificate</strong></span></span></td>
<td><span data-ttu-id="f5315-167">Můžete aktualizovat hello služby BizTalk toouse jiný certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="f5315-167">Can update hello BizTalk Service toouse a different SSL certificate.</span></span> <span data-ttu-id="f5315-168">Když automaticky vytvoří certifikát podepsaný svým držitelem SSL můžete <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">vytvořit hello služby BizTalk</a>.</span><span class="sxs-lookup"><span data-stu-id="f5315-168">A self-signed SSL certificate is automatically created when you <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">create hello BizTalk Service</a>.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-169"><strong>Stažení certifikátu</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-169"><strong>Download Certificate</strong></span></span></td>
<td><span data-ttu-id="f5315-170">Můžete si stáhnout certifikát SSL hello používá vaše služba BizTalk tooa místního počítače.</span><span class="sxs-lookup"><span data-stu-id="f5315-170">You can download hello SSL certificate used by your BizTalk Service tooa local machine.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-171"><strong>Stav</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-171"><strong>Status</strong></span></span></td>
<td><span data-ttu-id="f5315-172">Zobrazí aktuální stav hello vaší služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-172">Displays hello current status of your BizTalk Service.</span></span> <span data-ttu-id="f5315-173">V tématu <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Tabulka stavů služby</a>.</span><span class="sxs-lookup"><span data-stu-id="f5315-173">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Service state chart</a>.</span></span> </td>
</tr>
<tr>
<td><span data-ttu-id="f5315-174"><strong>Adresa URL služby</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-174"><strong>Service URL</strong></span></span></td>
<td><span data-ttu-id="f5315-175">Adresa URL Hello pro vaši službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-175">hello URL for your BizTalk Service.</span></span> <span data-ttu-id="f5315-176">Toto je stejný hello jako hello <strong>adresa URL domény</strong> zadali při vytváření služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-176">This is hello same as hello <strong>Domain URL</strong> entered when your BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-177"><strong>Veřejná virtuální IP (VIP) Adresa</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-177"><strong>Public Virtual IP (VIP) Address</strong></span></span></td>
<td><span data-ttu-id="f5315-178">přidělit IP adresu Hello tooyour služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-178">hello IP address assigned tooyour BizTalk Service.</span></span> <span data-ttu-id="f5315-179">To se používá pro všechny vstupní koncové body a je hello zdrojové adresy odchozího provozu.</span><span class="sxs-lookup"><span data-stu-id="f5315-179">It is used for all input endpoints and is hello source address for outbound traffic.</span></span> <span data-ttu-id="f5315-180">Tato IP adresa patří tooyour služby BizTalk, dokud se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="f5315-180">This IP address belongs tooyour BizTalk Service as long as it is created.</span></span> <span data-ttu-id="f5315-181">Pokud odstraníte hello služby BizTalk, je přiřazená IP adresa hello tooanother služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-181">If you delete hello BizTalk Service, hello IP address is assigned tooanother BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-182"><strong>Namespace služby ACS</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-182"><strong>ACS Namespace</strong></span></span></td>
<td><span data-ttu-id="f5315-183">Ověřuje se hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-183">Authenticates with hello BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-184"><strong>Edice</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-184"><strong>Edition</strong></span></span></td>
<td><span data-ttu-id="f5315-185">Uvádí hello edice zadá, když je vytvořena hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-185">Lists hello Edition entered when hello BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-186"><strong>Umístění</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-186"><strong>Location</strong></span></span></td>
<td><span data-ttu-id="f5315-187">Zobrazí hello zeměpisnou oblast, který je hostitelem služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-187">Displays hello geographic region that hosts your BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-188"><strong>Vytvořit</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-188"><strong>Created</strong></span></span></td>
<td><span data-ttu-id="f5315-189">Zobrazí hello datum a čas hello služba BizTalk byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="f5315-189">Displays hello date and time hello BizTalk Service was created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-190"><strong>Sledování databáze</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-190"><strong>Tracking Database</strong></span></span></td>
<td><span data-ttu-id="f5315-191">Název databáze SQL Azure Hello, který ukládá hello sledování tabulky, které používá vaše služba BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-191">hello Azure SQL Database name that stores hello tracking tables used by your BizTalk Service.</span></span> 
<br/><br/><span data-ttu-id="f5315-192">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Požadavky na Explained</a> poskytuje podrobné informace o hello sledování databáze.</span><span class="sxs-lookup"><span data-stu-id="f5315-192">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on hello Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-193"><strong>Monitorování/archivaci úložiště</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-193"><strong>Monitoring/Archiving Storage</strong></span></span></td>
<td><span data-ttu-id="f5315-194">název účtu úložiště Azure Hello uchovávající hello monitorování výstup svoji službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-194">hello Azure Storage account name that stores hello monitoring output of your BizTalk Service.</span></span>
<br/><br/><span data-ttu-id="f5315-195">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Požadavky na Explained</a> poskytuje podrobné informace o hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5315-195">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on hello Storage account.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-196"><strong>Název odběru</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-196"><strong>Subscription Name</strong></span></span></td>
<td><span data-ttu-id="f5315-197">Uvádí hello odběr, který je hostitelem služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-197">Lists hello subscription that hosts your BizTalk Service.</span></span> <span data-ttu-id="f5315-198">předplatné Hello řídí toohello přístup k portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="f5315-198">hello subscription governs access toohello Azure classic portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-199"><strong>ID předplatného</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-199"><strong>Subscription ID</strong></span></span></td>
<td><span data-ttu-id="f5315-200">Když je vytvořen předplatné, se automaticky vygeneroval ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="f5315-200">When a subscription is created, a subscription ID is automatically generated.</span></span> <span data-ttu-id="f5315-201">Při použití rozhraní REST API, musíte tooenter hello ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="f5315-201">When using REST APIs, you may need tooenter hello Subscription ID.</span></span></td>
</tr>
</table>

<span data-ttu-id="f5315-202">[BizTalk Services: Zřízení pomocí Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) seznamy hello kroky toocreate služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-202">[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) lists hello steps toocreate a BizTalk Service.</span></span>

##### <a name="manage-connection-information-sync-keys-and-delete-in-hello-task-bar"></a><span data-ttu-id="f5315-203">Spravovat informace o připojení, synchronizace klíčů a odstranit hello hlavním panelu:</span><span class="sxs-lookup"><span data-stu-id="f5315-203">Manage, Connection Information, Sync Keys, and Delete in hello task bar:</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="f5315-204"><strong>Spravovat</strong> nasazení vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f5315-204"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="f5315-205">Otevře hello portálu služby Azure BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-205">Opens hello Azure BizTalk Services Portal.</span></span> <span data-ttu-id="f5315-206">Hello portál služeb BizTalk je hello vstupu tooEDI konfigurace, včetně přidání partnery a vytvoření X12 AS2, EDIFACT smlouvy a.</span><span class="sxs-lookup"><span data-stu-id="f5315-206">hello BizTalk Services Portal is hello entrance tooEDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="f5315-207">Toto je stejný jako hello <strong>vytvořte smlouvy partnera</strong> na hello <strong>rychlý Start</strong> kartě.</span><span class="sxs-lookup"><span data-stu-id="f5315-207">This is hello same as <strong>Create partner agreements</strong> on hello <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="f5315-208">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> poskytuje další informace o hello portál služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-208">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on hello BizTalk Services Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-209"><strong>Informace o připojení</strong> z hello Namespace řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="f5315-209"><strong>Connection Information</strong> of hello Access Control Namespace</span></span></td>
<td><span data-ttu-id="f5315-210">Zobrazí hello Namespace řízení přístupu, výchozí vystavitele a klíč výchozí hodnoty; které je možné zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="f5315-210">Displays hello Access Control Namespace, Default Issuer, and Default Key values; which can be copied.</span></span>
<br/><br/>
<span data-ttu-id="f5315-211">Můžete také otevřít hello portál pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="f5315-211">You can also open hello Access Control Portal.</span></span> <span data-ttu-id="f5315-212">Tento portál pro řízení přístupu je hello stejné jako možnost služby Active Directory v levém navigačním podokně hello hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-212">This Access Control Portal is hello same as using hello Active Directory option in hello left navigation pane.</span></span>
<br/><br/><span data-ttu-id="f5315-213">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Správa vašeho Namespace ACS</a> poskytuje další informace o hello portál pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="f5315-213">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Managing Your ACS Namespace</a> provides more information on hello Access Control Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-214"><strong>Klíče synchronizovat</strong> v hello účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="f5315-214"><strong>Sync Keys</strong> in hello Storage Account</span></span></td>
<td><span data-ttu-id="f5315-215">Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-215">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="f5315-216">Tyto šifrovací klíče řízení přístupu tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5315-216">These encryption Keys control access tooyour Storage Account.</span></span> <span data-ttu-id="f5315-217">Vaše služba BizTalk automaticky používá hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-217">Your BizTalk Service automatically uses hello Primary Key.</span></span> <span data-ttu-id="f5315-218"><strong>Klíče synchronizovat</strong> povolit uživatelům tooswitch mezi hello primární klíč a sekundární klíč hello bez přerušení hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-218"><strong>Sync Keys</strong> enable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="f5315-219">Například chcete hello služby BizTalk toouse nový primární klíč pro hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5315-219">For example, you want hello BizTalk Service toouse a new Primary Key for hello Storage Account.</span></span> <span data-ttu-id="f5315-220">toodo toto:</span><span class="sxs-lookup"><span data-stu-id="f5315-220">toodo this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="f5315-221">Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>.</span><span class="sxs-lookup"><span data-stu-id="f5315-221">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="f5315-222">Vyberte hello sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-222">Select hello Secondary Key.</span></span> <span data-ttu-id="f5315-223">Když to uděláte, spustí hello služby BizTalk, pomocí hello sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-223">When you do this, hello BizTalk Service starts using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="f5315-224">V hello portál Azure classic vyberte svůj účet úložiště a obnovit hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-224">In hello Azure classic portal, select your Storage account and Regenerate hello Primary Key.</span></span> <span data-ttu-id="f5315-225">Pamatujte si, že se vaše služba BizTalk používá hello sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-225">Remember, your BizTalk Service is using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="f5315-226">Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>.</span><span class="sxs-lookup"><span data-stu-id="f5315-226">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="f5315-227">Nyní vyberte hello primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-227">Now, select hello Primary Key.</span></span> <span data-ttu-id="f5315-228">To je hello nové můžete znovu vygenerovat primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f5315-228">This is hello new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="f5315-229">V hello portál Azure classic vyberte svůj účet úložiště a znovu vygenerovat sekundární klíč hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-229">In hello Azure classic portal, select your Storage account and Regenerate hello Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="f5315-230">Tento proces se nazývá "výměny klíčů".</span><span class="sxs-lookup"><span data-stu-id="f5315-230">This process is called "rollover keys".</span></span> <span data-ttu-id="f5315-231">účelem Hello je tooenable uživatelé tooswitch mezi hello primární klíč a sekundární klíč hello bez přerušení hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-231">hello purpose is tooenable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="f5315-232"><strong>Odstranit</strong> vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="f5315-232"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="f5315-233">Vaše služba BizTalk a všechny položky, které jsou nasazené tooit se odeberou.</span><span class="sxs-lookup"><span data-stu-id="f5315-233">Your BizTalk Service and all items deployed tooit are removed.</span></span></td>
</tr>
</table>


## <a name="monitor"></a><span data-ttu-id="f5315-234">Monitorování</span><span class="sxs-lookup"><span data-stu-id="f5315-234">Monitor</span></span>
<span data-ttu-id="f5315-235">Doporučení se netýká toohello edice Free.</span><span class="sxs-lookup"><span data-stu-id="f5315-235">Does not apply toohello Free Edition.</span></span>

<span data-ttu-id="f5315-236">Když vyberete název vaší služby BizTalk, karta sledovat hello je k dispozici a zobrazí hello následující:</span><span class="sxs-lookup"><span data-stu-id="f5315-236">When you select your BizTalk Service name, hello Monitor tab is available and displays hello following:</span></span>

##### <a name="metric-graph-displays-hello-selected-performance-metrics"></a><span data-ttu-id="f5315-237">Metriky grafu: Zobrazí hello vybrali metriky výkonu</span><span class="sxs-lookup"><span data-stu-id="f5315-237">Metric Graph: Displays hello selected performance metrics</span></span>
<span data-ttu-id="f5315-238">Tyto metriky zadejte v reálném čase hodnoty týkající se stavu hello hello služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-238">These metrics provide real-time values regarding hello health of hello BizTalk Service.</span></span> <span data-ttu-id="f5315-239">Zvolíte, které metriky výkonu zobrazují.</span><span class="sxs-lookup"><span data-stu-id="f5315-239">You choose which performance metrics are displayed.</span></span> <span data-ttu-id="f5315-240">Nesmí být delší než šest metriky výkonu lze zobrazit najednou.</span><span class="sxs-lookup"><span data-stu-id="f5315-240">A maximum of six performance metrics can be displayed simultaneously.</span></span> 

<span data-ttu-id="f5315-241">Můžete také hello **relativní** nebo **absolutní** hodnoty a hello čas rozsah **Interval** hello metrik, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f5315-241">You can also choose hello **Relative** or **Absolute** values and hello time range **Interval** of hello metrics that are displayed.</span></span> 

##### <a name="tooremove-or-display-metrics-in-hello-graph"></a><span data-ttu-id="f5315-242">Metrika tooremove nebo zobrazení v grafu hello:</span><span class="sxs-lookup"><span data-stu-id="f5315-242">tooremove or display metrics in hello graph:</span></span>
1. <span data-ttu-id="f5315-243">Vyberte hello **monitorování** kartě.</span><span class="sxs-lookup"><span data-stu-id="f5315-243">Select hello **Monitor** tab.</span></span>
2. <span data-ttu-id="f5315-244">Vyberte **přidat metriky** hello hlavním panelu:</span><span class="sxs-lookup"><span data-stu-id="f5315-244">Select **Add Metrics** in hello task bar:</span></span>  
   <span data-ttu-id="f5315-245">![Vyberte Přidat metriky][AddMetrics]</span><span class="sxs-lookup"><span data-stu-id="f5315-245">![Select Add Metrics][AddMetrics]</span></span>
3. <span data-ttu-id="f5315-246">Zkontrolujte chcete toodisplay metriky výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-246">Check hello performance metrics you want toodisplay.</span></span>
4. <span data-ttu-id="f5315-247">Vyberte hello zaškrtnutí tooreturn toohello **monitorování** kartě.</span><span class="sxs-lookup"><span data-stu-id="f5315-247">Select hello checkmark tooreturn toohello **Monitor** tab.</span></span>
5. <span data-ttu-id="f5315-248">Vyberte hello kruh další toohello metriky toodisplay tuto metriku hodnota v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-248">Select hello circle next toohello metric toodisplay that metric's value in hello graph.</span></span>  
   
    <span data-ttu-id="f5315-249">Například hello **využití procesoru** metrika je zobrazeno šedě; není v grafu hello zobrazí jeho výstup:</span><span class="sxs-lookup"><span data-stu-id="f5315-249">For example, hello **CPU Usage** metric is grayed out; its output is not displayed in hello graph:</span></span>  
   <span data-ttu-id="f5315-250">![Je zobrazeno šedě metriky využití procesoru][GrayedMetric]</span><span class="sxs-lookup"><span data-stu-id="f5315-250">![CPU Usage metric is grayed out][GrayedMetric]</span></span>  
   
    <span data-ttu-id="f5315-251">Vyberte hello šedě kruh tooenable hello **využití procesoru** metriky toodisplay jeho výstup v grafu hello:</span><span class="sxs-lookup"><span data-stu-id="f5315-251">Select hello grayed out circle tooenable hello **CPU Usage** metric toodisplay its output in hello graph:</span></span>  
   <span data-ttu-id="f5315-252">![Využití procesoru metrika je povoleno.][EnabledMetric]</span><span class="sxs-lookup"><span data-stu-id="f5315-252">![CPU Usage metric is enabled][EnabledMetric]</span></span>
6. <span data-ttu-id="f5315-253">Vyberte tooremove metrika z hello zobrazení grafu a seznamu hello **odstranit metrika** hello hlavním panelu.</span><span class="sxs-lookup"><span data-stu-id="f5315-253">tooremove a metric from hello display graph and hello list, select **Delete Metric** in hello task bar.</span></span> <span data-ttu-id="f5315-254">tooadd hello metriky back toohello seznamu, vyberte **přidat metriky** hello hlavním panelu, zkontrolujte metrika hello a vyberte hello zaškrtnutí tooreturn toohello **monitorování** kartě. Vyberte hello šedě metrika hello tooenable kruh.</span><span class="sxs-lookup"><span data-stu-id="f5315-254">tooadd hello metric back toohello list, select **Add Metrics** in hello task bar, check hello metric, and select hello checkmark tooreturn toohello **Monitor** tab. Select hello grayed out circle tooenable hello metric.</span></span>

## <span data-ttu-id="f5315-255"><a name="Metrics"></a>Dostupné metriky</span><span class="sxs-lookup"><span data-stu-id="f5315-255"><a name="Metrics"></a>Available Metrics</span></span>
<span data-ttu-id="f5315-256">k dispozici jsou následující čítače výkonu nebo metriky Hello:</span><span class="sxs-lookup"><span data-stu-id="f5315-256">hello following performance counters/metrics are available:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="f5315-257"><strong>Latence RountdTrip</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-257"><strong>RountdTrip Latency</strong></span></span></td>
<td><span data-ttu-id="f5315-258">Zobrazí hello Průměrná doba v milisekundách (ms) tooprocess, je přijetí zprávu od hello čas uvítací zprávu, dokud hello služby BizTalk mezi všechny mostů plně zpracovává uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="f5315-258">Displays hello average time taken in milliseconds (ms) tooprocess a message from hello time hello message is received until hello message is fully processed by hello BizTalk Service across all bridges.</span></span> <span data-ttu-id="f5315-259">Započítávají se pouze zprávy úspěšně zpracoval.</span><span class="sxs-lookup"><span data-stu-id="f5315-259">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="f5315-260">Pokud dojde k hello následující události, je vytvořen časové razítko:</span><span class="sxs-lookup"><span data-stu-id="f5315-260">When hello following events occur, a timestamp is created:</span></span>
<ul>
<li><span data-ttu-id="f5315-261">Zpráva zadá hello brány</span><span class="sxs-lookup"><span data-stu-id="f5315-261">Message enters hello gateway</span></span></li>
<li><span data-ttu-id="f5315-262">Zpráva je cílový směrované toohello</span><span class="sxs-lookup"><span data-stu-id="f5315-262">Message is routed toohello destination</span></span></li>
<li><span data-ttu-id="f5315-263">Cílový odpověď přijata</span><span class="sxs-lookup"><span data-stu-id="f5315-263">Destination response is received</span></span></li>
<li><span data-ttu-id="f5315-264">Cílový potvrzení odpovědi odeslané toohello brány</span><span class="sxs-lookup"><span data-stu-id="f5315-264">Destination acknowledgement response sent toohello gateway</span></span></li>
</ul>
<br/>
<span data-ttu-id="f5315-265">Tato metrika uvádí hello výsledek hello následující výpočet:</span><span class="sxs-lookup"><span data-stu-id="f5315-265">This metric shows hello result of hello following calculation:</span></span>
<br/><br/>
<span data-ttu-id="f5315-266">[Cílové potvrzení odpovědi odeslané toohello brány] - [zpráva zadá hello brány]</span><span class="sxs-lookup"><span data-stu-id="f5315-266">[Destination acknowledgement response sent toohello gateway] - [Message enters hello gateway]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-267"><strong>Selhání u zdroje</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-267"><strong>Failures At Source</strong></span></span></td>
<td><span data-ttu-id="f5315-268">Zobrazí celkový počet zpráv, které se nezdařily hello podle hello služby BizTalk při stahování zprávy ze zdroje hello koncové body.</span><span class="sxs-lookup"><span data-stu-id="f5315-268">Displays hello total number of messages that failed by hello BizTalk Service when pulling messages from hello source endpoints.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-269"><strong>Využití procesoru</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-269"><strong>CPU Usage</strong></span></span></td>
<td><span data-ttu-id="f5315-270">Uvádí hello průměrný % času procesoru všech instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="f5315-270">Lists hello average %Processor Time of all role instances.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-271"><strong>Čekací doba zpracování</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-271"><strong>Processing Latency</strong></span></span></td>
<td><span data-ttu-id="f5315-272">Zobrazí hello Průměrná doba v milisekundách (ms) tooprocess zprávu zpracovat hello služby BizTalk mezi všechny mostů, s výjimkou hello čas věnovaný cíle.</span><span class="sxs-lookup"><span data-stu-id="f5315-272">Displays hello average time taken In milliseconds (ms) tooprocess a message by hello BizTalk Service across all bridges, excluding hello time spent in destinations.</span></span> <span data-ttu-id="f5315-273">Započítávají se pouze zprávy úspěšně zpracoval.</span><span class="sxs-lookup"><span data-stu-id="f5315-273">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="f5315-274">Pokud dojde k každý hello následující události, se vytvoří časové razítko:</span><span class="sxs-lookup"><span data-stu-id="f5315-274">When each of hello following events occur, a timestamp is created:</span></span>

<ul>
<li><span data-ttu-id="f5315-275">Zpráva zadá hello brány</span><span class="sxs-lookup"><span data-stu-id="f5315-275">Message enters hello gateway</span></span></li>
<li><span data-ttu-id="f5315-276">Zpráva je cílový směrované toohello</span><span class="sxs-lookup"><span data-stu-id="f5315-276">Message is routed toohello destination</span></span></li>
<li><span data-ttu-id="f5315-277">Cílový odpověď přijata</span><span class="sxs-lookup"><span data-stu-id="f5315-277">Destination response is received</span></span></li>
<li><span data-ttu-id="f5315-278">Cílový potvrzení odpovědi odeslané toohello brány</span><span class="sxs-lookup"><span data-stu-id="f5315-278">Destination acknowledgement response sent toohello gateway</span></span></li>
</ul>
<br/><span data-ttu-id="f5315-279">Tato metrika uvádí hello výsledek hello následující výpočet:</span><span class="sxs-lookup"><span data-stu-id="f5315-279">This metric shows hello result of hello following calculation:</span></span><br/><br/>
<span data-ttu-id="f5315-280">[Cílové potvrzení odpovědi odeslané toohello brány] - [zpráva zadá hello brány] - [cílové odpověď přijata] + [zpráva je cílový směrované toohello]</span><span class="sxs-lookup"><span data-stu-id="f5315-280">[Destination acknowledgement response sent toohello gateway] - [Message enters hello gateway] - [Destination response is received] + [Message is routed toohello destination]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-281"><strong>Selhání v procesu</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-281"><strong>Failures In Process</strong></span></span></td>
<td><span data-ttu-id="f5315-282">Zobrazí celkový počet zpráv, které se nezdařilo během zpracování v časovém intervalu hello služby BizTalk mezi všechny mostů hello hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-282">Displays hello total number of messages that failed during processing by hello BizTalk Service across all hello bridges within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-283"><strong>Zprávy odeslané</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-283"><strong>Messages Sent</strong></span></span></td>
<td><span data-ttu-id="f5315-284">Zobrazí hello celkový počet zprávy odeslané přes všechny mostů hello služby BizTalk v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="f5315-284">Displays hello total number of messages sent by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="f5315-285">Tato metrika se zvýší, když se dosáhne cílové trasy hello zprávy odeslané z kanálu.</span><span class="sxs-lookup"><span data-stu-id="f5315-285">This metric is incremented when a message sent from a pipeline reaches hello route destination.</span></span> <span data-ttu-id="f5315-286">Tato metrika neznamená, že je zpráva úspěšně zpracována.</span><span class="sxs-lookup"><span data-stu-id="f5315-286">This metric does not indicate that a message is successfully processed.</span></span><br/><br/>
<span data-ttu-id="f5315-287">V případě požadavku a odpovědi hello metrika se zvýší, když cílový trasy hello odesílá zpět toohello kanál potvrzení přijetí.</span><span class="sxs-lookup"><span data-stu-id="f5315-287">In a Request-Reply scenario, hello metric is incremented when hello route destination sends a receipt acknowledgement back toohello pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-288"><strong>Přijatých zpráv</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-288"><strong>Messages Received</strong></span></span></td>
<td><span data-ttu-id="f5315-289">Zobrazí hello celkový počet zpráv přijatých hello služby BizTalk mezi všechny mostů v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="f5315-289">Displays hello total number of messages received by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="f5315-290">Tato metrika se zvýší, když novou zprávu přijme hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="f5315-290">This metric is incremented when a new message is received by hello pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-291"><strong>Zprávy v procesu</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-291"><strong>Messages In Process</strong></span></span></td>
<td><span data-ttu-id="f5315-292">Zobrazí hello celkový počet zpráv, které jsou aktuálně zpracovávaných hello služby BizTalk v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="f5315-292">Displays hello total number of messages currently being processed by hello BizTalk Service within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="f5315-293"><strong>Počet zpracovaných zpráv</strong></span><span class="sxs-lookup"><span data-stu-id="f5315-293"><strong>Messages Processed</strong></span></span></td>
<td><span data-ttu-id="f5315-294">Zobrazí hello celkový počet zpráv úspěšně zpracoval hello služby BizTalk mezi všechny mostů v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="f5315-294">Displays hello total number of messages successfully processed by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="f5315-295">Tato metrika se zvýší, když zprávu úspěšně přijme hello kanálu a úspěšně směrované toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="f5315-295">This metric is incremented when a message is successfully received by hello pipeline and successfully routed toohello destination.</span></span></td>
</tr>
</table>


## <a name="scale"></a><span data-ttu-id="f5315-296">Měřítko</span><span class="sxs-lookup"><span data-stu-id="f5315-296">Scale</span></span>
<span data-ttu-id="f5315-297">Na kartě hello škálování můžete přidat nebo odečíst hello počet jednotek, které používá vaše služba BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-297">In hello Scale tab, you can add or subtract hello number of units used by your BizTalk Service.</span></span> <span data-ttu-id="f5315-298">Ve výchozím nastavení není nakonfigurované jednotky.</span><span class="sxs-lookup"><span data-stu-id="f5315-298">By default, there is one Unit configured.</span></span> <span data-ttu-id="f5315-299">Další jednotky lze přidat tooscale svoji službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="f5315-299">Additional Units can be added tooscale your BizTalk Service.</span></span> <span data-ttu-id="f5315-300">Pokud zvýšíte hello škálování, jsou zvýšit propustnost.</span><span class="sxs-lookup"><span data-stu-id="f5315-300">When you increase hello scale, you are increasing throughput.</span></span> <span data-ttu-id="f5315-301">Hello objem prostředků také zvyšuje, včetně nasazené mostech, smlouvy, obchodní připojení a výpočetní výkon.</span><span class="sxs-lookup"><span data-stu-id="f5315-301">hello amount of resources also increases, including deployed bridges, agreements, LOB connections, and processing power.</span></span> <span data-ttu-id="f5315-302">Například můžete zvýšit hello škále od 1 jednotka too2 jednotky.</span><span class="sxs-lookup"><span data-stu-id="f5315-302">For example, you increase hello scale from 1 Unit too2 Units.</span></span> <span data-ttu-id="f5315-303">V takovém případě můžete nasadit dvojité hello počet mostů, double hello smluv, double hello LOB připojení a dvojité hello výpočetní výkon.</span><span class="sxs-lookup"><span data-stu-id="f5315-303">In this situation, you can deploy double hello number of bridges, double hello agreements, double hello LOB connections, and double hello processing power.</span></span>

<span data-ttu-id="f5315-304">Některých edicích BizTalk nenabízejí možnost škálování.</span><span class="sxs-lookup"><span data-stu-id="f5315-304">Some BizTalk editions do not offer a scale option.</span></span> <span data-ttu-id="f5315-305">V takovém případě je povolená jednu jednotku.</span><span class="sxs-lookup"><span data-stu-id="f5315-305">In this situation, one Unit is permitted.</span></span> <span data-ttu-id="f5315-306">toodetermine je možné rozšířit vaše edice, kolik jednotek odkazovat příliš[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="f5315-306">toodetermine how many units your edition can be scaled, refer too[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>

<span data-ttu-id="f5315-307">Zvýšením počtu hello jednotky může mít vliv na ceny.</span><span class="sxs-lookup"><span data-stu-id="f5315-307">Increasing hello number of units may impact pricing.</span></span> <span data-ttu-id="f5315-308">Pokud zvýšíte hello jednotky, pak výběrem **Uložit** zobrazí zpráva s informacemi, pokud je ovlivněno fakturace.</span><span class="sxs-lookup"><span data-stu-id="f5315-308">If you increase hello Units, selecting **Save** displays a message that tells you if billing is impacted.</span></span> <span data-ttu-id="f5315-309">Potom vyberte toocontinue.</span><span class="sxs-lookup"><span data-stu-id="f5315-309">You then choose toocontinue.</span></span> <span data-ttu-id="f5315-310">Pokud zvýšíte počet jednotek hello, hello stav služby BizTalk se změní z aktivní tooUpdating.</span><span class="sxs-lookup"><span data-stu-id="f5315-310">When you increase hello number of Units, hello BizTalk Service status changes from Active tooUpdating.</span></span> <span data-ttu-id="f5315-311">V hello aktualizace stavu pokračuje svoji službu BizTalk toorun.</span><span class="sxs-lookup"><span data-stu-id="f5315-311">In hello Updating state, your BizTalk Service continues toorun.</span></span>

<span data-ttu-id="f5315-312">[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md) definuje "Jednotka".</span><span class="sxs-lookup"><span data-stu-id="f5315-312">[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md) defines a "Unit".</span></span>

## <a name="configure"></a><span data-ttu-id="f5315-313">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="f5315-313">Configure</span></span>
<span data-ttu-id="f5315-314">Nevztahuje tooHybrid připojení.</span><span class="sxs-lookup"><span data-stu-id="f5315-314">Does not apply tooHybrid Connections.</span></span>

<span data-ttu-id="f5315-315">Nastaví stav zálohování tooNone hello nebo automaticky.</span><span class="sxs-lookup"><span data-stu-id="f5315-315">Sets hello Backup Status tooNone or Automatic.</span></span> <span data-ttu-id="f5315-316">Pokud nastavíte tooNone, se automaticky vytvoří žádné zálohy.</span><span class="sxs-lookup"><span data-stu-id="f5315-316">When set tooNone, no backups are automatically created.</span></span> <span data-ttu-id="f5315-317">Pokud nastavíte tooAutomatic, nakonfigurujte umístění zálohy hello, hello četnost zálohování hello a jak dlouho tookeep hello záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="f5315-317">When set tooAutomatic, you configure hello backup location, hello frequency of hello backup, and how long tookeep hello backup files.</span></span> 

<span data-ttu-id="f5315-318">[Služba BizTalk Services: Zálohování a obnovení](biztalk-backup-restore.md) poskytuje podrobnosti hello.</span><span class="sxs-lookup"><span data-stu-id="f5315-318">[BizTalk Services: Backup and Restore](biztalk-backup-restore.md) provides hello details.</span></span> 

## <span data-ttu-id="f5315-319"><a name="HybridConnections"></a>Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="f5315-319"><a name="HybridConnections"></a>Hybrid Connections</span></span>
<span data-ttu-id="f5315-320">Hybridní připojení slouží k připojení aplikací Azure, jako je Web Apps nebo Mobile Apps v Azure App Service, tooan místnímu prostředku, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb.</span><span class="sxs-lookup"><span data-stu-id="f5315-320">Hybrid Connections connect an Azure application, like Web Apps or Mobile Apps in Azure App Service, tooan on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="f5315-321">Hybridní připojení se spravují ve službě BizTalk Services v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="f5315-321">Hybrid Connections are managed in  BizTalk Services in hello Azure classic portal.</span></span>

<span data-ttu-id="f5315-322">toocreate hybridní připojení ve službě Azure App Service najdete v části [přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f5315-322">toocreate Hybrid Connections in Azure App Service, see [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

<span data-ttu-id="f5315-323">toocreate nebo Správa hybridních připojení ve službě Azure BizTalk Services najdete v tématu [hybridní připojení](integration-hybrid-connection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5315-323">toocreate or manage Hybrid Connections in Azure BizTalk Services, see [Hybrid Connections](integration-hybrid-connection-overview.md).</span></span>

## <a name="next"></a><span data-ttu-id="f5315-324">Další</span><span class="sxs-lookup"><span data-stu-id="f5315-324">Next</span></span>
<span data-ttu-id="f5315-325">Teď, když jste obeznámeni s různých kartách hello, můžete se další informace o funkcích služby Azure BizTalk Services hello:</span><span class="sxs-lookup"><span data-stu-id="f5315-325">Now that you're familiar with hello different tabs, you can learn more about hello Azure BizTalk Services features:</span></span>

* [<span data-ttu-id="f5315-326">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="f5315-326">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)  
* [<span data-ttu-id="f5315-327">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="f5315-327">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)  
* [<span data-ttu-id="f5315-328">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="f5315-328">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)

## <a name="see-also"></a><span data-ttu-id="f5315-329">Viz také</span><span class="sxs-lookup"><span data-stu-id="f5315-329">See Also</span></span>
* [<span data-ttu-id="f5315-330">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="f5315-330">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)  
* [<span data-ttu-id="f5315-331">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="f5315-331">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](biztalk-editions-feature-chart.md)  
* [<span data-ttu-id="f5315-332">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="f5315-332">BizTalk Services: Provisioning Using Azure classic portal</span></span>](biztalk-provision-services.md)  
* [<span data-ttu-id="f5315-333">BizTalk Services: Tabulka stav služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="f5315-333">BizTalk Services: BizTalk Service State Chart</span></span>](biztalk-service-state-chart.md)  
* [<span data-ttu-id="f5315-334">Jak začít používat hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="f5315-334">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

