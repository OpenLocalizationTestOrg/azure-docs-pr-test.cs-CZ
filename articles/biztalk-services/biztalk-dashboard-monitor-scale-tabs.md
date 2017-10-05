---
title: "Řídicí panel, sledování, měřítko, konfigurovat a hybridní připojení ve službě BizTalk Services | Microsoft Docs"
description: "Další informace o ovládací prvky a sledovat výkon na kartě klasického portálu služby BizTalk Services: řídicí panel, sledování, škálování, konfigurovat a hybridní připojení. MABS, WABS"
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
ms.openlocfilehash: 4ec88d9a681a5692b31f7e3990d1c153296b18ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a><span data-ttu-id="2ee01-104">Kontrola karet řídicího panelu, monitorování, škálování, konfigurace a hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="2ee01-104">Review the Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="2ee01-105">Po vytvoření služby BizTalk a nasazení aplikace, můžete změnit některá nastavení služby BizTalk a monitorování výkonu aplikací.</span><span class="sxs-lookup"><span data-stu-id="2ee01-105">After you create your BizTalk Service and deploy your application, you can change some of the BizTalk Service settings and monitor the application performance.</span></span> 

<span data-ttu-id="2ee01-106">Když otevřete portál Azure classic, je automaticky umístí na **všechny položky** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ee01-106">When you open the Azure classic portal, you are automatically placed at the **ALL ITEMS** tab.</span></span> <span data-ttu-id="2ee01-107">Chcete-li zobrazit vaše služba BizTalk, vyberte svoji službu BizTalk v **všechny položky** kartě nebo vyberte **BIZTALK SERVICES** kartě; a potom vyberte název vaší služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-107">To view your BizTalk Service, select your BizTalk Service in the **ALL ITEMS** tab or select the **BIZTALK SERVICES** tab; and then select your BizTalk Service name.</span></span>

<span data-ttu-id="2ee01-108">Otevře se nové okno s následujícími kartami.</span><span class="sxs-lookup"><span data-stu-id="2ee01-108">This opens a new window with the following tabs.</span></span> <span data-ttu-id="2ee01-109">Toto téma popisuje těchto karet.</span><span class="sxs-lookup"><span data-stu-id="2ee01-109">This topic describes these tabs.</span></span>

## <a name="quickstart-quickstartquickstart"></a><span data-ttu-id="2ee01-110">(Rychlý start</span><span class="sxs-lookup"><span data-stu-id="2ee01-110">Quickstart (</span></span>![Rychlý start][Quickstart]<span data-ttu-id="2ee01-112">)</span><span class="sxs-lookup"><span data-stu-id="2ee01-112">)</span></span>
<span data-ttu-id="2ee01-113">V závislosti na edici služby BizTalk nemusí být k dispozici všechny možnosti, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="2ee01-113">Depending on the BizTalk Services Edition, all options listed may not be available.</span></span> 

<table border="1">
    <tr>
        <td><span data-ttu-id="2ee01-114"><strong>Získat nástroje</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-114"><strong>Get the tools</strong></span></span></td>
        <td><span data-ttu-id="2ee01-115">Stáhněte si sadu SDK BizTalk Services nainstalovat šablony projektů Visual Studio na vývojovém počítači místně.</span><span class="sxs-lookup"><span data-stu-id="2ee01-115">Download the BizTalk Services SDK to install the Visual Studio project templates on your on-premises development computer.</span></span> <span data-ttu-id="2ee01-116">Tyto šablony vytvořit <strong>BizTalk Services</strong> (mostu) a <strong>artefaktů služby BizTalk</strong> projektů sady Visual Studio (transformace), které jsou nasazeny do vaší služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-116">These templates create the <strong>BizTalk Services</strong> (bridge) and the <strong>BizTalk Service Artifacts</strong> (Transform) Visual Studio projects that are deployed to your BizTalk Service.</span></span>
        <br/><br/><span data-ttu-id="2ee01-117">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Jak spustit pomocí sady SDK Azure BizTalk Services </a> a <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">instalace sady SDK Azure BizTalk Services</a> jsou uvedeny kroky, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="2ee01-117">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335"> How do I Start Using the Azure BizTalk Services SDK </a> and <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Installing the Azure BizTalk Services SDK</a> lists the steps to get started.</span></span>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="2ee01-118"><strong>Vytvořte partnery smlouvy</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-118"><strong>Create partner agreements</strong></span></span></td>
        <td><span data-ttu-id="2ee01-119">Otevřete portál Azure BizTalk Services hostované v Azure, kde je přidat partnery a vytvořit X12 AS2, smlouvy EDIFACT EDI a.</span><span class="sxs-lookup"><span data-stu-id="2ee01-119">Opens the Azure BizTalk Services Portal hosted on Azure where you add partners and create X12, AS2, and EDIFACT EDI agreements.</span></span>
        <br/><br/><span data-ttu-id="2ee01-120">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> jsou uvedeny kroky, abyste mohli začít.</span><span class="sxs-lookup"><span data-stu-id="2ee01-120">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> lists the steps to get started.</span></span>
        </td>
    </tr>

<tr>
        <td><span data-ttu-id="2ee01-121"><strong>Další informace o BizTalk Services</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-121"><strong>Learn more about BizTalk Services</strong></span></span></td>
        <td><span data-ttu-id="2ee01-122">Přejděte na <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">učení center</a> Další informace o Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="2ee01-122">Go to the <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> to learn more about Azure BizTalk Services.</span></span></td>
</tr>
</table>


<span data-ttu-id="2ee01-123">Na hlavním panelu v dolní části můžete:</span><span class="sxs-lookup"><span data-stu-id="2ee01-123">In the task bar at the bottom, you can:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="2ee01-124"><strong>Spravovat</strong> nasazení vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="2ee01-124"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="2ee01-125">Otevřete portál služby Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="2ee01-125">Opens the Azure BizTalk Services portal.</span></span> <span data-ttu-id="2ee01-126">Na portálu služby BizTalk se vstupem EDI konfigurace, včetně přidání partnery a vytvoření X12 AS2, EDIFACT smlouvy a.</span><span class="sxs-lookup"><span data-stu-id="2ee01-126">The BizTalk Services Portal is the entrance to EDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="2ee01-127">Je to stejné nastavení jako <strong>vytvořte smlouvy partnera</strong> na <strong>rychlý Start</strong> kartě.</span><span class="sxs-lookup"><span data-stu-id="2ee01-127">This is the same as <strong>Create partner agreements</strong> on the <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="2ee01-128">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> poskytuje další informace o portálu služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-128">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on the BizTalk Services Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="2ee01-129"><strong>Informace o připojení</strong> z Namespace řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="2ee01-129"><strong>Connection Information</strong> of the Access Control Namespace</span></span></td>
<td><span data-ttu-id="2ee01-130">Když vyberete informace o připojení, pak se zobrazují Namespace řízení přístupu, výchozí vystavitele a výchozí klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-130">When you select Connection Information, then the Access Control Namespace, Default Issuer, and Default Key are displayed.</span></span> <span data-ttu-id="2ee01-131">Můžete zkopírovat tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2ee01-131">You can copy these values.</span></span>
<br/><br/>
<span data-ttu-id="2ee01-132">Můžete také otevřít portál pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-132">You can also open the Access Control Portal.</span></span> <span data-ttu-id="2ee01-133"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Vytvoření ovládacího prvku přístup Namespace</a> poskytuje další informace o portálu řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-133"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Create an Access control Namespace</a> provides more information on the Access Control Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="2ee01-134"><strong>Klíče synchronizovat</strong> v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="2ee01-134"><strong>Sync Keys</strong> in the Storage Account</span></span></td>
<td><span data-ttu-id="2ee01-135">Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-135">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="2ee01-136">Tyto šifrovací klíče řízení přístupu k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ee01-136">These encryption Keys control access to your Storage Account.</span></span> <span data-ttu-id="2ee01-137">Vaše služba BizTalk automaticky používá primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-137">Your BizTalk Service automatically uses the Primary Key.</span></span> <span data-ttu-id="2ee01-138"><strong>Klíče synchronizovat</strong> uživatelé přepínat mezi primární klíč a sekundární klíč bez přerušení služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-138"><strong>Sync Keys</strong> enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="2ee01-139">Například chcete službu BizTalk používat nový primární klíč pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ee01-139">For example, you want the BizTalk Service to use a new Primary Key for the Storage Account.</span></span> <span data-ttu-id="2ee01-140">Použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="2ee01-140">To do this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="2ee01-141">Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>.</span><span class="sxs-lookup"><span data-stu-id="2ee01-141">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="2ee01-142">Vyberte sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-142">Select the Secondary Key.</span></span> <span data-ttu-id="2ee01-143">Když to uděláte, spuštění služby BizTalk pomocí sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-143">When you do this, the BizTalk Service starts using the Secondary Key.</span></span></li>
<li><span data-ttu-id="2ee01-144">Na portálu Azure classic vyberte svůj účet úložiště a obnovit primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-144">In the Azure classic portal, select your Storage account and Regenerate the Primary Key.</span></span> <span data-ttu-id="2ee01-145">Pamatujte si, že se vaše služba BizTalk používá sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-145">Remember, your BizTalk Service is using the Secondary Key.</span></span></li>
<li><span data-ttu-id="2ee01-146">Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>.</span><span class="sxs-lookup"><span data-stu-id="2ee01-146">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="2ee01-147">Nyní vyberte primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-147">Now, select the Primary Key.</span></span> <span data-ttu-id="2ee01-148">Toto je nový primární klíč, můžete se znova vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="2ee01-148">This is the new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="2ee01-149">Na portálu Azure classic vyberte svůj účet úložiště a obnovit sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-149">In the Azure classic portal, select your Storage account and Regenerate the Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="2ee01-150">Tento proces se nazývá "výměny klíčů".</span><span class="sxs-lookup"><span data-stu-id="2ee01-150">This process is called "rollover keys".</span></span> <span data-ttu-id="2ee01-151">Účelem je umožnit uživatelům přepínat mezi primární klíč a sekundární klíč bez přerušení služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-151">The purpose is to enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="2ee01-152"><strong>Odstranit</strong> vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="2ee01-152"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="2ee01-153">Když vyberete odstranit, vaše služba BizTalk a jsou odebrány všechny položky nasadili do ní.</span><span class="sxs-lookup"><span data-stu-id="2ee01-153">When you select Delete, your BizTalk Service and all items deployed to it are removed.</span></span></td>
</tr>
</table>


## <a name="dashboard"></a><span data-ttu-id="2ee01-154">Řídicí panel</span><span class="sxs-lookup"><span data-stu-id="2ee01-154">Dashboard</span></span>
<span data-ttu-id="2ee01-155">V závislosti na edici služby BizTalk nemusí být k dispozici všechny možnosti, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="2ee01-155">Depending on the BizTalk Services Edition, all options listed may not be available.</span></span> 

<span data-ttu-id="2ee01-156">Když vyberete název vaší služby BizTalk, se zobrazí řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="2ee01-156">When you select your BizTalk Service name, the Dashboard tab is displayed.</span></span> <span data-ttu-id="2ee01-157">Na řídicím panelu můžete:</span><span class="sxs-lookup"><span data-stu-id="2ee01-157">In Dashboard, you can:</span></span>

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a><span data-ttu-id="2ee01-158">Přehled využití: Zobrazuje počet použitých hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="2ee01-158">Usage Overview: Shows the number of used Hybrid Connections</span></span>
<span data-ttu-id="2ee01-159">Také zobrazuje využití dat v GB.</span><span class="sxs-lookup"><span data-stu-id="2ee01-159">Also displays the data usage in GB.</span></span> 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a><span data-ttu-id="2ee01-160">Metriky grafu: Zobrazuje pevnou seznam metrik výkonu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-160">Metric Graph: Shows a fixed list of performance metrics</span></span>
<span data-ttu-id="2ee01-161">Tyto metriky zadejte v reálném čase hodnoty týkající se stavu služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-161">These metrics provide real-time values regarding the health of the BizTalk Service.</span></span> <span data-ttu-id="2ee01-162">Můžete také **relativní** nebo **absolutní** hodnoty a časové rozmezí **Interval** metrik, která se zobrazí v grafu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-162">You can also choose the **Relative** or **Absolute** values and the time range **Interval** of the metrics that are displayed in the graph.</span></span> 

<span data-ttu-id="2ee01-163">Popis tyto metriky výkonu, přejděte na [dostupné metriky](#Metrics) v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-163">For a description of these performance metrics, go to [Available Metrics](#Metrics) in this topic.</span></span>

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a><span data-ttu-id="2ee01-164">Rychlého přehledu: Seznamy vlastnosti vaší služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="2ee01-164">Quick Glance: Lists your BizTalk Service properties</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="2ee01-165"><strong>Aktualizujte přihlašovací údaje databáze sledování</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-165"><strong>Update Tracking Database credentials</strong></span></span></td>
<td><span data-ttu-id="2ee01-166">Změní uživatelské jméno a heslo použité pro přihlášení k databázi pro sledování.</span><span class="sxs-lookup"><span data-stu-id="2ee01-166">Changes the user name and password used to log into the Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-167"><strong>Aktualizovat certifikát SSL</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-167"><strong>Update SSL Certificate</strong></span></span></td>
<td><span data-ttu-id="2ee01-168">Můžete aktualizovat službu BizTalk používat jiný certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="2ee01-168">Can update the BizTalk Service to use a different SSL certificate.</span></span> <span data-ttu-id="2ee01-169">Když automaticky vytvoří certifikát podepsaný svým držitelem SSL můžete <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">vytvořit službu BizTalk</a>.</span><span class="sxs-lookup"><span data-stu-id="2ee01-169">A self-signed SSL certificate is automatically created when you <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">create the BizTalk Service</a>.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-170"><strong>Stažení certifikátu</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-170"><strong>Download Certificate</strong></span></span></td>
<td><span data-ttu-id="2ee01-171">Můžete si stáhnout certifikát SSL používaný službou BizTalk do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="2ee01-171">You can download the SSL certificate used by your BizTalk Service to a local machine.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-172"><strong>Stav</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-172"><strong>Status</strong></span></span></td>
<td><span data-ttu-id="2ee01-173">Zobrazí aktuální stav služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-173">Displays the current status of your BizTalk Service.</span></span> <span data-ttu-id="2ee01-174">V tématu <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Tabulka stavů služby</a>.</span><span class="sxs-lookup"><span data-stu-id="2ee01-174">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Service state chart</a>.</span></span> </td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-175"><strong>Adresa URL služby</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-175"><strong>Service URL</strong></span></span></td>
<td><span data-ttu-id="2ee01-176">Adresa URL pro svoji službu BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-176">The URL for your BizTalk Service.</span></span> <span data-ttu-id="2ee01-177">Je to stejné jako <strong>adresa URL domény</strong> zadali při vytváření služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-177">This is the same as the <strong>Domain URL</strong> entered when your BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-178"><strong>Veřejná virtuální IP (VIP) Adresa</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-178"><strong>Public Virtual IP (VIP) Address</strong></span></span></td>
<td><span data-ttu-id="2ee01-179">IP adresa přiřazenou k vaší službě BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-179">The IP address assigned to your BizTalk Service.</span></span> <span data-ttu-id="2ee01-180">To se používá pro všechny vstupní koncové body a je zdrojovou adresu odchozího provozu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-180">It is used for all input endpoints and is the source address for outbound traffic.</span></span> <span data-ttu-id="2ee01-181">Tato IP adresa patří do vaší služby BizTalk, dokud se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="2ee01-181">This IP address belongs to your BizTalk Service as long as it is created.</span></span> <span data-ttu-id="2ee01-182">Pokud odstraníte službu BizTalk, je jiná služba BizTalk přidělit IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-182">If you delete the BizTalk Service, the IP address is assigned to another BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-183"><strong>Namespace služby ACS</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-183"><strong>ACS Namespace</strong></span></span></td>
<td><span data-ttu-id="2ee01-184">Ověřuje se služba BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-184">Authenticates with the BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-185"><strong>Edice</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-185"><strong>Edition</strong></span></span></td>
<td><span data-ttu-id="2ee01-186">Seznamy edice zadali při vytvoření služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-186">Lists the Edition entered when the BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-187"><strong>Umístění</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-187"><strong>Location</strong></span></span></td>
<td><span data-ttu-id="2ee01-188">Zobrazí zeměpisnou oblast, která je hostitelem služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-188">Displays the geographic region that hosts your BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-189"><strong>Vytvořit</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-189"><strong>Created</strong></span></span></td>
<td><span data-ttu-id="2ee01-190">Zobrazí datum a čas vytvoření služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-190">Displays the date and time the BizTalk Service was created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-191"><strong>Sledování databáze</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-191"><strong>Tracking Database</strong></span></span></td>
<td><span data-ttu-id="2ee01-192">Název databáze SQL Azure, který ukládá sledovacích tabulek, které vaše služba BizTalk používá.</span><span class="sxs-lookup"><span data-stu-id="2ee01-192">The Azure SQL Database name that stores the tracking tables used by your BizTalk Service.</span></span> 
<br/><br/><span data-ttu-id="2ee01-193">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Požadavky na Explained</a> poskytuje podrobné informace o sledování databáze.</span><span class="sxs-lookup"><span data-stu-id="2ee01-193">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on the Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-194"><strong>Monitorování/archivaci úložiště</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-194"><strong>Monitoring/Archiving Storage</strong></span></span></td>
<td><span data-ttu-id="2ee01-195">Název účtu úložiště Azure, který ukládá výstup monitorování vaší služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-195">The Azure Storage account name that stores the monitoring output of your BizTalk Service.</span></span>
<br/><br/><span data-ttu-id="2ee01-196">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Požadavky na Explained</a> poskytuje podrobné informace o účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ee01-196">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on the Storage account.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-197"><strong>Název odběru</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-197"><strong>Subscription Name</strong></span></span></td>
<td><span data-ttu-id="2ee01-198">Uvádí odběr, který je hostitelem služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-198">Lists the subscription that hosts your BizTalk Service.</span></span> <span data-ttu-id="2ee01-199">Předplatné řídí přístup k portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="2ee01-199">The subscription governs access to the Azure classic portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-200"><strong>ID předplatného</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-200"><strong>Subscription ID</strong></span></span></td>
<td><span data-ttu-id="2ee01-201">Když je vytvořen předplatné, se automaticky vygeneroval ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="2ee01-201">When a subscription is created, a subscription ID is automatically generated.</span></span> <span data-ttu-id="2ee01-202">Při použití rozhraní REST API, budete muset zadat ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="2ee01-202">When using REST APIs, you may need to enter the Subscription ID.</span></span></td>
</tr>
</table>

<span data-ttu-id="2ee01-203">[BizTalk Services: Zřízení pomocí Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) obsahuje kroky k vytvoření služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-203">[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) lists the steps to create a BizTalk Service.</span></span>

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a><span data-ttu-id="2ee01-204">Spravovat informace o připojení, synchronizace klíče a odstranění na hlavním panelu:</span><span class="sxs-lookup"><span data-stu-id="2ee01-204">Manage, Connection Information, Sync Keys, and Delete in the task bar:</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="2ee01-205"><strong>Spravovat</strong> nasazení vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="2ee01-205"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="2ee01-206">Otevře na portálu Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="2ee01-206">Opens the Azure BizTalk Services Portal.</span></span> <span data-ttu-id="2ee01-207">Na portálu služby BizTalk se vstupem EDI konfigurace, včetně přidání partnery a vytvoření X12 AS2, EDIFACT smlouvy a.</span><span class="sxs-lookup"><span data-stu-id="2ee01-207">The BizTalk Services Portal is the entrance to EDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="2ee01-208">Je to stejné nastavení jako <strong>vytvořte smlouvy partnera</strong> na <strong>rychlý Start</strong> kartě.</span><span class="sxs-lookup"><span data-stu-id="2ee01-208">This is the same as <strong>Create partner agreements</strong> on the <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="2ee01-209">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> poskytuje další informace o portálu služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-209">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on the BizTalk Services Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-210"><strong>Informace o připojení</strong> z Namespace řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="2ee01-210"><strong>Connection Information</strong> of the Access Control Namespace</span></span></td>
<td><span data-ttu-id="2ee01-211">Zobrazí Namespace řízení přístupu, výchozí vystavitele a klíč výchozí hodnoty; které je možné zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="2ee01-211">Displays the Access Control Namespace, Default Issuer, and Default Key values; which can be copied.</span></span>
<br/><br/>
<span data-ttu-id="2ee01-212">Můžete také otevřít portál pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-212">You can also open the Access Control Portal.</span></span> <span data-ttu-id="2ee01-213">Tento portál pro řízení přístupu je stejná jako možnost služby Active Directory v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="2ee01-213">This Access Control Portal is the same as using the Active Directory option in the left navigation pane.</span></span>
<br/><br/><span data-ttu-id="2ee01-214">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Správa vašeho Namespace ACS</a> poskytuje další informace o portálu řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-214">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Managing Your ACS Namespace</a> provides more information on the Access Control Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-215"><strong>Klíče synchronizovat</strong> v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="2ee01-215"><strong>Sync Keys</strong> in the Storage Account</span></span></td>
<td><span data-ttu-id="2ee01-216">Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-216">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="2ee01-217">Tyto šifrovací klíče řízení přístupu k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ee01-217">These encryption Keys control access to your Storage Account.</span></span> <span data-ttu-id="2ee01-218">Vaše služba BizTalk automaticky používá primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-218">Your BizTalk Service automatically uses the Primary Key.</span></span> <span data-ttu-id="2ee01-219"><strong>Klíče synchronizovat</strong> uživatelé přepínat mezi primární klíč a sekundární klíč bez přerušení služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-219"><strong>Sync Keys</strong> enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="2ee01-220">Například chcete službu BizTalk používat nový primární klíč pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ee01-220">For example, you want the BizTalk Service to use a new Primary Key for the Storage Account.</span></span> <span data-ttu-id="2ee01-221">Použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="2ee01-221">To do this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="2ee01-222">Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>.</span><span class="sxs-lookup"><span data-stu-id="2ee01-222">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="2ee01-223">Vyberte sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-223">Select the Secondary Key.</span></span> <span data-ttu-id="2ee01-224">Když to uděláte, spuštění služby BizTalk pomocí sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-224">When you do this, the BizTalk Service starts using the Secondary Key.</span></span></li>
<li><span data-ttu-id="2ee01-225">Na portálu Azure classic vyberte svůj účet úložiště a obnovit primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-225">In the Azure classic portal, select your Storage account and Regenerate the Primary Key.</span></span> <span data-ttu-id="2ee01-226">Pamatujte si, že se vaše služba BizTalk používá sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-226">Remember, your BizTalk Service is using the Secondary Key.</span></span></li>
<li><span data-ttu-id="2ee01-227">Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>.</span><span class="sxs-lookup"><span data-stu-id="2ee01-227">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="2ee01-228">Nyní vyberte primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-228">Now, select the Primary Key.</span></span> <span data-ttu-id="2ee01-229">Toto je nový primární klíč, můžete se znova vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="2ee01-229">This is the new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="2ee01-230">Na portálu Azure classic vyberte svůj účet úložiště a obnovit sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="2ee01-230">In the Azure classic portal, select your Storage account and Regenerate the Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="2ee01-231">Tento proces se nazývá "výměny klíčů".</span><span class="sxs-lookup"><span data-stu-id="2ee01-231">This process is called "rollover keys".</span></span> <span data-ttu-id="2ee01-232">Účelem je umožnit uživatelům přepínat mezi primární klíč a sekundární klíč bez přerušení služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-232">The purpose is to enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="2ee01-233"><strong>Odstranit</strong> vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="2ee01-233"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="2ee01-234">Vaše služba BizTalk a nasadili do ní všechny položky se odeberou.</span><span class="sxs-lookup"><span data-stu-id="2ee01-234">Your BizTalk Service and all items deployed to it are removed.</span></span></td>
</tr>
</table>


## <a name="monitor"></a><span data-ttu-id="2ee01-235">Monitorování</span><span class="sxs-lookup"><span data-stu-id="2ee01-235">Monitor</span></span>
<span data-ttu-id="2ee01-236">Nevztahuje se na edice Free.</span><span class="sxs-lookup"><span data-stu-id="2ee01-236">Does not apply to the Free Edition.</span></span>

<span data-ttu-id="2ee01-237">Když vyberete název vaší služby BizTalk, na kartě monitorování je k dispozici a zobrazí se následující:</span><span class="sxs-lookup"><span data-stu-id="2ee01-237">When you select your BizTalk Service name, the Monitor tab is available and displays the following:</span></span>

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a><span data-ttu-id="2ee01-238">Metriky grafu: Zobrazí metriky výkonu vybrané</span><span class="sxs-lookup"><span data-stu-id="2ee01-238">Metric Graph: Displays the selected performance metrics</span></span>
<span data-ttu-id="2ee01-239">Tyto metriky zadejte v reálném čase hodnoty týkající se stavu služby BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-239">These metrics provide real-time values regarding the health of the BizTalk Service.</span></span> <span data-ttu-id="2ee01-240">Zvolíte, které metriky výkonu zobrazují.</span><span class="sxs-lookup"><span data-stu-id="2ee01-240">You choose which performance metrics are displayed.</span></span> <span data-ttu-id="2ee01-241">Nesmí být delší než šest metriky výkonu lze zobrazit najednou.</span><span class="sxs-lookup"><span data-stu-id="2ee01-241">A maximum of six performance metrics can be displayed simultaneously.</span></span> 

<span data-ttu-id="2ee01-242">Můžete také **relativní** nebo **absolutní** hodnoty a časové rozmezí **Interval** metrik, která se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="2ee01-242">You can also choose the **Relative** or **Absolute** values and the time range **Interval** of the metrics that are displayed.</span></span> 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a><span data-ttu-id="2ee01-243">Odebrat nebo zobrazování metrik v grafu:</span><span class="sxs-lookup"><span data-stu-id="2ee01-243">To remove or display metrics in the graph:</span></span>
1. <span data-ttu-id="2ee01-244">Vyberte **monitorování** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ee01-244">Select the **Monitor** tab.</span></span>
2. <span data-ttu-id="2ee01-245">Vyberte **přidat metriky** na hlavním panelu:</span><span class="sxs-lookup"><span data-stu-id="2ee01-245">Select **Add Metrics** in the task bar:</span></span>  
   <span data-ttu-id="2ee01-246">![Vyberte Přidat metriky][AddMetrics]</span><span class="sxs-lookup"><span data-stu-id="2ee01-246">![Select Add Metrics][AddMetrics]</span></span>
3. <span data-ttu-id="2ee01-247">Zkontrolujte metrik výkonu, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="2ee01-247">Check the performance metrics you want to display.</span></span>
4. <span data-ttu-id="2ee01-248">Vyberte na značku zaškrtnutí se vrátíte do **monitorování** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ee01-248">Select the checkmark to return to the **Monitor** tab.</span></span>
5. <span data-ttu-id="2ee01-249">Vyberte kroužek vedle metrika zobrazíte tuto metriku hodnota v grafu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-249">Select the circle next to the metric to display that metric's value in the graph.</span></span>  
   
    <span data-ttu-id="2ee01-250">Například **využití procesoru** metrika je zobrazeno šedě; není v grafu zobrazí jeho výstup:</span><span class="sxs-lookup"><span data-stu-id="2ee01-250">For example, the **CPU Usage** metric is grayed out; its output is not displayed in the graph:</span></span>  
   <span data-ttu-id="2ee01-251">![Je zobrazeno šedě metriky využití procesoru][GrayedMetric]</span><span class="sxs-lookup"><span data-stu-id="2ee01-251">![CPU Usage metric is grayed out][GrayedMetric]</span></span>  
   
    <span data-ttu-id="2ee01-252">Vyberte si kruhu. Chcete-li povolit šedým **využití procesoru** metrika se používá k zobrazení jeho výstupu v grafu:</span><span class="sxs-lookup"><span data-stu-id="2ee01-252">Select the grayed out circle to enable the **CPU Usage** metric to display its output in the graph:</span></span>  
   <span data-ttu-id="2ee01-253">![Využití procesoru metrika je povoleno.][EnabledMetric]</span><span class="sxs-lookup"><span data-stu-id="2ee01-253">![CPU Usage metric is enabled][EnabledMetric]</span></span>
6. <span data-ttu-id="2ee01-254">Metriky odebrat ze seznamu a zobrazení grafu, vyberte **odstranit metrika** na hlavním panelu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-254">To remove a metric from the display graph and the list, select **Delete Metric** in the task bar.</span></span> <span data-ttu-id="2ee01-255">Chcete-li přidat metriky zpět do seznamu, vyberte **přidat metriky** na hlavním panelu metriku a vyberte značku zaškrtnutí se vraťte do **monitorování** kartě.</span><span class="sxs-lookup"><span data-stu-id="2ee01-255">To add the metric back to the list, select **Add Metrics** in the task bar, check the metric, and select the checkmark to return to the **Monitor** tab.</span></span> <span data-ttu-id="2ee01-256">Vyberte šedým kruhu. Chcete-li povolit metriku.</span><span class="sxs-lookup"><span data-stu-id="2ee01-256">Select the grayed out circle to enable the metric.</span></span>

## <span data-ttu-id="2ee01-257"><a name="Metrics"></a>Dostupné metriky</span><span class="sxs-lookup"><span data-stu-id="2ee01-257"><a name="Metrics"></a>Available Metrics</span></span>
<span data-ttu-id="2ee01-258">K dispozici jsou následující čítače výkonu nebo metriky:</span><span class="sxs-lookup"><span data-stu-id="2ee01-258">The following performance counters/metrics are available:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="2ee01-259"><strong>Latence RountdTrip</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-259"><strong>RountdTrip Latency</strong></span></span></td>
<td><span data-ttu-id="2ee01-260">Průměrná doba v milisekundách (ms), jakou trvalo zpracování zprávy ze čas, že doručení zprávy, dokud služba BizTalk je plně zpracovat zprávu zobrazuje napříč všechny mostů.</span><span class="sxs-lookup"><span data-stu-id="2ee01-260">Displays the average time taken in milliseconds (ms) to process a message from the time the message is received until the message is fully processed by the BizTalk Service across all bridges.</span></span> <span data-ttu-id="2ee01-261">Započítávají se pouze zprávy úspěšně zpracoval.</span><span class="sxs-lookup"><span data-stu-id="2ee01-261">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="2ee01-262">Když dojde k následujícím událostem, vytvoří se časovým razítkem:</span><span class="sxs-lookup"><span data-stu-id="2ee01-262">When the following events occur, a timestamp is created:</span></span>
<ul>
<li><span data-ttu-id="2ee01-263">Zpráva zadá brány</span><span class="sxs-lookup"><span data-stu-id="2ee01-263">Message enters the gateway</span></span></li>
<li><span data-ttu-id="2ee01-264">Zpráva směrována na cíl</span><span class="sxs-lookup"><span data-stu-id="2ee01-264">Message is routed to the destination</span></span></li>
<li><span data-ttu-id="2ee01-265">Cílový odpověď přijata</span><span class="sxs-lookup"><span data-stu-id="2ee01-265">Destination response is received</span></span></li>
<li><span data-ttu-id="2ee01-266">Cílový potvrzení odpovědi odeslané k bráně.</span><span class="sxs-lookup"><span data-stu-id="2ee01-266">Destination acknowledgement response sent to the gateway</span></span></li>
</ul>
<br/>
<span data-ttu-id="2ee01-267">Tato metrika uvádí výsledek tohoto výpočtu:</span><span class="sxs-lookup"><span data-stu-id="2ee01-267">This metric shows the result of the following calculation:</span></span>
<br/><br/>
<span data-ttu-id="2ee01-268">[Cílové potvrzení odeslané odpovědi k bráně] - [zpráva zadá brány]</span><span class="sxs-lookup"><span data-stu-id="2ee01-268">[Destination acknowledgement response sent to the gateway] - [Message enters the gateway]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-269"><strong>Selhání u zdroje</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-269"><strong>Failures At Source</strong></span></span></td>
<td><span data-ttu-id="2ee01-270">Zobrazí celkový počet zpráv, které se nezdařily službou BizTalk při stahování zprávy ze zdroje koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="2ee01-270">Displays the total number of messages that failed by the BizTalk Service when pulling messages from the source endpoints.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-271"><strong>Využití procesoru</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-271"><strong>CPU Usage</strong></span></span></td>
<td><span data-ttu-id="2ee01-272">Zobrazí průměrný % času procesoru všech instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="2ee01-272">Lists the average %Processor Time of all role instances.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-273"><strong>Čekací doba zpracování</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-273"><strong>Processing Latency</strong></span></span></td>
<td><span data-ttu-id="2ee01-274">Průměrná doba v milisekundách (ms) ke zpracování zprávy pomocí služby BizTalk se zobrazí napříč všechny mostů, s výjimkou času stráveného v cíle.</span><span class="sxs-lookup"><span data-stu-id="2ee01-274">Displays the average time taken In milliseconds (ms) to process a message by the BizTalk Service across all bridges, excluding the time spent in destinations.</span></span> <span data-ttu-id="2ee01-275">Započítávají se pouze zprávy úspěšně zpracoval.</span><span class="sxs-lookup"><span data-stu-id="2ee01-275">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="2ee01-276">Pokud dojde k každý z následujících událostí, je vytvořen časové razítko:</span><span class="sxs-lookup"><span data-stu-id="2ee01-276">When each of the following events occur, a timestamp is created:</span></span>

<ul>
<li><span data-ttu-id="2ee01-277">Zpráva zadá brány</span><span class="sxs-lookup"><span data-stu-id="2ee01-277">Message enters the gateway</span></span></li>
<li><span data-ttu-id="2ee01-278">Zpráva směrována na cíl</span><span class="sxs-lookup"><span data-stu-id="2ee01-278">Message is routed to the destination</span></span></li>
<li><span data-ttu-id="2ee01-279">Cílový odpověď přijata</span><span class="sxs-lookup"><span data-stu-id="2ee01-279">Destination response is received</span></span></li>
<li><span data-ttu-id="2ee01-280">Cílový potvrzení odpovědi odeslané k bráně.</span><span class="sxs-lookup"><span data-stu-id="2ee01-280">Destination acknowledgement response sent to the gateway</span></span></li>
</ul>
<br/><span data-ttu-id="2ee01-281">Tato metrika uvádí výsledek tohoto výpočtu:</span><span class="sxs-lookup"><span data-stu-id="2ee01-281">This metric shows the result of the following calculation:</span></span><br/><br/>
<span data-ttu-id="2ee01-282">[Cílové potvrzení odeslané odpovědi k bráně] - [zpráva zadá brány] - [cílové odpověď přijata] + [zpráva směrována na cílovém]</span><span class="sxs-lookup"><span data-stu-id="2ee01-282">[Destination acknowledgement response sent to the gateway] - [Message enters the gateway] - [Destination response is received] + [Message is routed to the destination]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-283"><strong>Selhání v procesu</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-283"><strong>Failures In Process</strong></span></span></td>
<td><span data-ttu-id="2ee01-284">Zobrazí celkový počet zpráv, které selhaly během zpracování službou BizTalk mezi všechny mostů v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-284">Displays the total number of messages that failed during processing by the BizTalk Service across all the bridges within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-285"><strong>Zprávy odeslané</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-285"><strong>Messages Sent</strong></span></span></td>
<td><span data-ttu-id="2ee01-286">Zobrazí celkový počet zpráv odeslaných službou BizTalk mezi všechny mostů v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-286">Displays the total number of messages sent by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="2ee01-287">Tato metrika se zvýší, když zprávy odeslané z kanálu dosáhne cílové trasy.</span><span class="sxs-lookup"><span data-stu-id="2ee01-287">This metric is incremented when a message sent from a pipeline reaches the route destination.</span></span> <span data-ttu-id="2ee01-288">Tato metrika neznamená, že je zpráva úspěšně zpracována.</span><span class="sxs-lookup"><span data-stu-id="2ee01-288">This metric does not indicate that a message is successfully processed.</span></span><br/><br/>
<span data-ttu-id="2ee01-289">V případě požadavku a odpovědi metrika se zvýší, když cíl trasy odešle potvrzení přijetí zpátky do kanálu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-289">In a Request-Reply scenario, the metric is incremented when the route destination sends a receipt acknowledgement back to the pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-290"><strong>Přijatých zpráv</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-290"><strong>Messages Received</strong></span></span></td>
<td><span data-ttu-id="2ee01-291">Zobrazí celkový počet zpráv přijatých službu BizTalk mezi všechny mostů v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-291">Displays the total number of messages received by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="2ee01-292">Tato metrika se zvýší, když novou zprávu přijetí v kanálu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-292">This metric is incremented when a new message is received by the pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-293"><strong>Zprávy v procesu</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-293"><strong>Messages In Process</strong></span></span></td>
<td><span data-ttu-id="2ee01-294">Zobrazí celkový počet zpráv, které jsou aktuálně zpracovávaných službu BizTalk v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-294">Displays the total number of messages currently being processed by the BizTalk Service within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="2ee01-295"><strong>Počet zpracovaných zpráv</strong></span><span class="sxs-lookup"><span data-stu-id="2ee01-295"><strong>Messages Processed</strong></span></span></td>
<td><span data-ttu-id="2ee01-296">Zobrazí celkový počet zpráv úspěšně zpracovaných službou BizTalk mezi všechny mostů v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="2ee01-296">Displays the total number of messages successfully processed by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="2ee01-297">Tato metrika se zvýší, když zprávu úspěšně přijme kanál a úspěšně směrovány do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="2ee01-297">This metric is incremented when a message is successfully received by the pipeline and successfully routed to the destination.</span></span></td>
</tr>
</table>


## <a name="scale"></a><span data-ttu-id="2ee01-298">Měřítko</span><span class="sxs-lookup"><span data-stu-id="2ee01-298">Scale</span></span>
<span data-ttu-id="2ee01-299">Na kartě škálování můžete přidat nebo odečíst počet jednotek, které používá vaše služba BizTalk.</span><span class="sxs-lookup"><span data-stu-id="2ee01-299">In the Scale tab, you can add or subtract the number of units used by your BizTalk Service.</span></span> <span data-ttu-id="2ee01-300">Ve výchozím nastavení není nakonfigurované jednotky.</span><span class="sxs-lookup"><span data-stu-id="2ee01-300">By default, there is one Unit configured.</span></span> <span data-ttu-id="2ee01-301">Škálování služby BizTalk můžete přidat další jednotky.</span><span class="sxs-lookup"><span data-stu-id="2ee01-301">Additional Units can be added to scale your BizTalk Service.</span></span> <span data-ttu-id="2ee01-302">Pokud zvýšíte měřítka, jsou zvýšit propustnost.</span><span class="sxs-lookup"><span data-stu-id="2ee01-302">When you increase the scale, you are increasing throughput.</span></span> <span data-ttu-id="2ee01-303">Objem prostředků také zvyšuje, včetně nasazené mostech, smlouvy, obchodní připojení a výpočetní výkon.</span><span class="sxs-lookup"><span data-stu-id="2ee01-303">The amount of resources also increases, including deployed bridges, agreements, LOB connections, and processing power.</span></span> <span data-ttu-id="2ee01-304">Například můžete zvýšit škále od 1 jednotka do 2 jednotky.</span><span class="sxs-lookup"><span data-stu-id="2ee01-304">For example, you increase the scale from 1 Unit to 2 Units.</span></span> <span data-ttu-id="2ee01-305">V takovém případě můžete nasadit double počet mostů, dvakrát smlouvy, dvakrát LOB připojení a dvakrát výpočetní výkon.</span><span class="sxs-lookup"><span data-stu-id="2ee01-305">In this situation, you can deploy double the number of bridges, double the agreements, double the LOB connections, and double the processing power.</span></span>

<span data-ttu-id="2ee01-306">Některých edicích BizTalk nenabízejí možnost škálování.</span><span class="sxs-lookup"><span data-stu-id="2ee01-306">Some BizTalk editions do not offer a scale option.</span></span> <span data-ttu-id="2ee01-307">V takovém případě je povolená jednu jednotku.</span><span class="sxs-lookup"><span data-stu-id="2ee01-307">In this situation, one Unit is permitted.</span></span> <span data-ttu-id="2ee01-308">Chcete-li zjistit, kolik jednotek je možné rozšířit vaše edice, přečtěte [BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="2ee01-308">To determine how many units your edition can be scaled, refer to [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>

<span data-ttu-id="2ee01-309">Zvýšením počtu jednotek může mít vliv na ceny.</span><span class="sxs-lookup"><span data-stu-id="2ee01-309">Increasing the number of units may impact pricing.</span></span> <span data-ttu-id="2ee01-310">Pokud zvýšíte jednotky, pak výběrem **Uložit** zobrazí zpráva s informacemi, pokud je ovlivněno fakturace.</span><span class="sxs-lookup"><span data-stu-id="2ee01-310">If you increase the Units, selecting **Save** displays a message that tells you if billing is impacted.</span></span> <span data-ttu-id="2ee01-311">Potom budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="2ee01-311">You then choose to continue.</span></span> <span data-ttu-id="2ee01-312">Pokud zvýšíte počet jednotek, změny stavu služby BizTalk z aktivní na aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2ee01-312">When you increase the number of Units, the BizTalk Service status changes from Active to Updating.</span></span> <span data-ttu-id="2ee01-313">Ve stavu aktualizace svoji službu BizTalk dál běžet.</span><span class="sxs-lookup"><span data-stu-id="2ee01-313">In the Updating state, your BizTalk Service continues to run.</span></span>

<span data-ttu-id="2ee01-314">[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md) definuje "Jednotka".</span><span class="sxs-lookup"><span data-stu-id="2ee01-314">[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md) defines a "Unit".</span></span>

## <a name="configure"></a><span data-ttu-id="2ee01-315">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="2ee01-315">Configure</span></span>
<span data-ttu-id="2ee01-316">Nevztahuje se na hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="2ee01-316">Does not apply to Hybrid Connections.</span></span>

<span data-ttu-id="2ee01-317">Nastaví stav zálohování na hodnotu None nebo automaticky.</span><span class="sxs-lookup"><span data-stu-id="2ee01-317">Sets the Backup Status to None or Automatic.</span></span> <span data-ttu-id="2ee01-318">Pokud nastavíte na hodnotu None, žádné zálohy se vytvoří automaticky.</span><span class="sxs-lookup"><span data-stu-id="2ee01-318">When set to None, no backups are automatically created.</span></span> <span data-ttu-id="2ee01-319">Pokud nastavíte na hodnotu automaticky, nakonfigurujte umístění zálohy, frekvenci zálohování a jak dlouho chcete zachovat záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="2ee01-319">When set to Automatic, you configure the backup location, the frequency of the backup, and how long to keep the backup files.</span></span> 

<span data-ttu-id="2ee01-320">[Služba BizTalk Services: Zálohování a obnovení](biztalk-backup-restore.md) nabízí podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="2ee01-320">[BizTalk Services: Backup and Restore](biztalk-backup-restore.md) provides the details.</span></span> 

## <span data-ttu-id="2ee01-321"><a name="HybridConnections"></a>Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="2ee01-321"><a name="HybridConnections"></a>Hybrid Connections</span></span>
<span data-ttu-id="2ee01-322">Hybridní připojení aplikací Azure, jako je Web Apps nebo Mobile Apps v Azure App Service připojit k místnímu prostředku, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb.</span><span class="sxs-lookup"><span data-stu-id="2ee01-322">Hybrid Connections connect an Azure application, like Web Apps or Mobile Apps in Azure App Service, to an on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="2ee01-323">Hybridní připojení se spravují v BizTalk Services na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="2ee01-323">Hybrid Connections are managed in  BizTalk Services in the Azure classic portal.</span></span>

<span data-ttu-id="2ee01-324">Vytvoření hybridní připojení ve službě Azure App Service naleznete v tématu [přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2ee01-324">To create Hybrid Connections in Azure App Service, see [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

<span data-ttu-id="2ee01-325">Pokud chcete vytvořit ani spravovat hybridní připojení ve službě Azure BizTalk Services, přečtěte si téma [hybridní připojení](integration-hybrid-connection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2ee01-325">To create or manage Hybrid Connections in Azure BizTalk Services, see [Hybrid Connections](integration-hybrid-connection-overview.md).</span></span>

## <a name="next"></a><span data-ttu-id="2ee01-326">Další</span><span class="sxs-lookup"><span data-stu-id="2ee01-326">Next</span></span>
<span data-ttu-id="2ee01-327">Teď, když jste obeznámeni s různých kartách, další informace o funkcích služby Azure BizTalk Services:</span><span class="sxs-lookup"><span data-stu-id="2ee01-327">Now that you're familiar with the different tabs, you can learn more about the Azure BizTalk Services features:</span></span>

* [<span data-ttu-id="2ee01-328">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="2ee01-328">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)  
* [<span data-ttu-id="2ee01-329">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="2ee01-329">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)  
* [<span data-ttu-id="2ee01-330">BizTalk Services: Zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="2ee01-330">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)

## <a name="see-also"></a><span data-ttu-id="2ee01-331">Viz také</span><span class="sxs-lookup"><span data-stu-id="2ee01-331">See Also</span></span>
* [<span data-ttu-id="2ee01-332">Hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="2ee01-332">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)  
* [<span data-ttu-id="2ee01-333">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="2ee01-333">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](biztalk-editions-feature-chart.md)  
* [<span data-ttu-id="2ee01-334">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="2ee01-334">BizTalk Services: Provisioning Using Azure classic portal</span></span>](biztalk-provision-services.md)  
* [<span data-ttu-id="2ee01-335">BizTalk Services: Tabulka stav služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="2ee01-335">BizTalk Services: BizTalk Service State Chart</span></span>](biztalk-service-state-chart.md)  
* [<span data-ttu-id="2ee01-336">Jak začít používat sadu SDK Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="2ee01-336">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

