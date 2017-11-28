---
title: "aaaUsing Azure AD Connect Health se synchronizací | Microsoft Docs"
description: "Toto je stránka hello Azure AD Connect Health, která bude popisují, jak synchronizovat toomonitor Azure AD Connect."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 1dfbeaba-bda2-4f68-ac89-1dbfaf5b4015
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56f0582be30e664026cedf15350bc23501998bfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-ad-connect-sync-with-azure-ad-connect-health"></a><span data-ttu-id="524d7-103">Sledování synchronizace Azure AD Connect pomocí služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="524d7-103">Monitor Azure AD Connect sync with Azure AD Connect Health</span></span>
<span data-ttu-id="524d7-104">Následující dokumentace Hello je konkrétní toomonitoring Azure AD Connect (Sync) s Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="524d7-104">hello following documentation is specific toomonitoring Azure AD Connect (Sync) with Azure AD Connect Health.</span></span>  <span data-ttu-id="524d7-105">Informace o sledování služby AD FS pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md).</span><span class="sxs-lookup"><span data-stu-id="524d7-105">For information on monitoring AD FS with Azure AD Connect Health see [Using Azure AD Connect Health with AD FS](active-directory-aadconnect-health-adfs.md).</span></span> <span data-ttu-id="524d7-106">Informace o sledování služby Active Directory Domain Services pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md).</span><span class="sxs-lookup"><span data-stu-id="524d7-106">Additionally, for information on monitoring Active Directory Domain Services with Azure AD Connect Health see [Using Azure AD Connect Health with AD DS](active-directory-aadconnect-health-adds.md).</span></span>

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a><span data-ttu-id="524d7-108">Upozornění služby Azure AD Connect Health pro synchronizaci</span><span class="sxs-lookup"><span data-stu-id="524d7-108">Alerts for Azure AD Connect Health for sync</span></span>
<span data-ttu-id="524d7-109">Hello Azure AD Connect stavu výstrahy pro část synchronizace poskytuje že Hello seznam aktivních výstrah.</span><span class="sxs-lookup"><span data-stu-id="524d7-109">hello Azure AD Connect Health Alerts for sync section provides you hello list of active alerts.</span></span> <span data-ttu-id="524d7-110">Každé upozornění obsahuje důležité informace, postup řešení a odkazy toorelated dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="524d7-110">Each alert includes relevant information, resolution steps, and links toorelated documentation.</span></span> <span data-ttu-id="524d7-111">Výběrem aktivního nebo vyřešeného upozornění se zobrazí nové okno s další informace, jakož i kroky, které můžete provést tooresolve hello výstrahy a odkazy tooadditional dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="524d7-111">By selecting an active or resolved alert you will see a new blade with additional information, as well as steps you can take tooresolve hello alert, and links tooadditional documentation.</span></span> <span data-ttu-id="524d7-112">Můžete také zobrazit historická data na výstrahy, které byly vyřešeny hello posledních.</span><span class="sxs-lookup"><span data-stu-id="524d7-112">You can also view historical data on alerts that were resolved in hello past.</span></span>

<span data-ttu-id="524d7-113">Výběrem některého upozornění zobrazíte další informace a také kroky můžete využít tooresolve hello výstrahy a odkazy tooadditional dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="524d7-113">By selecting an alert you will be provided with additional information as well as steps you can take tooresolve hello alert and links tooadditional documentation.</span></span>

![Chyba synchronizace služby Azure AD Connect](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a><span data-ttu-id="524d7-115">Omezené vyhodnocení upozornění</span><span class="sxs-lookup"><span data-stu-id="524d7-115">Limited Evaluation of Alerts</span></span>
<span data-ttu-id="524d7-116">Pokud Azure AD Connect není používá hello výchozí konfiguraci (například pokud je filtrování atributů změněné z hello výchozí konfigurace tooa vlastní konfigurace), pak agenta hello Azure AD Connect Health nebude odesílat hello chybové události související tooAzure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="524d7-116">If Azure AD Connect is NOT using hello default configuration (for example, if Attribute Filtering is changed from hello default configuration tooa custom configuration), then hello Azure AD Connect Health agent will not upload hello error events related tooAzure AD Connect.</span></span>

<span data-ttu-id="524d7-117">Toto nastavení omezuje hello vyhodnocení upozornění službou hello.</span><span class="sxs-lookup"><span data-stu-id="524d7-117">This limits hello evaluation of alerts by hello service.</span></span> <span data-ttu-id="524d7-118">Zobrazí se banner, který označuje tuto podmínku, která v hello portálu Azure v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="524d7-118">You'd will see a banner that indicates this condition in hello Azure Portal under your service.</span></span>

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/banner.png)

<span data-ttu-id="524d7-120">Můžete změnit kliknutím na "Nastavení" a povolení tooupload agenta Azure AD Connect Health všechny protokoly chyb.</span><span class="sxs-lookup"><span data-stu-id="524d7-120">You can change this by clicking "Settings" and allowing Azure AD Connect Health agent tooupload all error logs.</span></span>

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a><span data-ttu-id="524d7-122">Analýza synchronizace</span><span class="sxs-lookup"><span data-stu-id="524d7-122">Sync Insight</span></span>
<span data-ttu-id="524d7-123">Správci často mají tooknow o hello době potřebné změny tooAzure toosync AD a hello množství probíhající změny.</span><span class="sxs-lookup"><span data-stu-id="524d7-123">Admins Frequently want tooknow about hello time it takes toosync changes tooAzure AD and hello amount of changes taking place.</span></span> <span data-ttu-id="524d7-124">Tato funkce poskytuje toovisualize snadný způsob, jak to pomocí hello níže grafy:</span><span class="sxs-lookup"><span data-stu-id="524d7-124">This feature provides an easy way toovisualize this using hello below graphs:</span></span>   

* <span data-ttu-id="524d7-125">sledování latence operací synchronizace</span><span class="sxs-lookup"><span data-stu-id="524d7-125">Latency of sync operations</span></span>
* <span data-ttu-id="524d7-126">sledování trendu změn objektů</span><span class="sxs-lookup"><span data-stu-id="524d7-126">Object Change trend</span></span>

### <a name="sync-latency"></a><span data-ttu-id="524d7-127">Latence synchronizace</span><span class="sxs-lookup"><span data-stu-id="524d7-127">Sync Latency</span></span>
<span data-ttu-id="524d7-128">Tato funkce poskytuje grafické zobrazení trendu latence operací hello synchronizace (import, export atd.) pro konektory.</span><span class="sxs-lookup"><span data-stu-id="524d7-128">This feature provides a graphical trend of latency of hello sync operations (import, export, etc.) for connectors.</span></span>  <span data-ttu-id="524d7-129">To poskytuje rychlý a snadný způsob toounderstand nejen hello latence vaše operace (větší, pokud máte velké sady změn), ale také způsob toodetect anomálií hello latence, které můžou vyžadovat další šetření.</span><span class="sxs-lookup"><span data-stu-id="524d7-129">This provides a quick and easy way toounderstand not only hello latency of your operations (larger if you have a large set of changes occurring) but also a way toodetect anomalies in hello latency that may require further investigation.</span></span>

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/synclatency02.png)

<span data-ttu-id="524d7-131">Ve výchozím nastavení se zobrazí pouze hello latence operace "Export" hello pro konektor Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="524d7-131">By default, only hello latency of hello 'Export' operation for hello Azure AD connector is shown.</span></span>  <span data-ttu-id="524d7-132">toosee další operace na konektoru hello nebo tooview operace z jiných konektorů, klikněte pravým tlačítkem na graf hello vyberte upravit graf nebo klikněte na tlačítko "Upravit graf latence" hello a zvolte konkrétní operaci hello a konektory.</span><span class="sxs-lookup"><span data-stu-id="524d7-132">toosee more operations on hello connector or tooview operations from other connectors, right-click on hello chart,  select Edit Chart or click on hello "Edit Latency Chart" button and choose hello specific operation and connectors.</span></span>

### <a name="sync-object-changes"></a><span data-ttu-id="524d7-133">Změny objektů synchronizace</span><span class="sxs-lookup"><span data-stu-id="524d7-133">Sync Object Changes</span></span>
<span data-ttu-id="524d7-134">Tato funkce poskytuje grafické zobrazení trendu hello počet změn, které se vyhodnocují a exportují tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="524d7-134">This feature provides a graphical trend of hello number of changes that are being evaluated and exported tooAzure AD.</span></span>  <span data-ttu-id="524d7-135">Dnes, snažíme toogather tyto informace z protokolů synchronizace hello je obtížné.</span><span class="sxs-lookup"><span data-stu-id="524d7-135">Today, trying toogather this information from hello sync logs is difficult.</span></span>  <span data-ttu-id="524d7-136">Hello graf poskytuje nejen jednodušší způsob sledování hello počet změn, ke kterým dochází ve vašem prostředí, ale i vizuální zobrazení hello chyby, ke kterým dochází.</span><span class="sxs-lookup"><span data-stu-id="524d7-136">hello chart gives you, not only a simpler way of monitoring hello number of changes that are occurring in your environment, but also a visual view of hello failures that are occurring.</span></span>

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a><span data-ttu-id="524d7-138">Sestava chyb synchronizace na úrovni objektu (verze Preview)</span><span class="sxs-lookup"><span data-stu-id="524d7-138">Object Level Synchronization Error Report (Preview)</span></span>
<span data-ttu-id="524d7-139">Tato funkce poskytuje sestavu chyb synchronizace, ke kterým může dojít při synchronizaci dat identity mezi službou Windows Server AD a Azure AD pomocí služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="524d7-139">This feature provides a report about synchronization errors that can occur when identity data is synchronized between Windows Server AD and Azure AD using Azure AD Connect.</span></span>

* <span data-ttu-id="524d7-140">Sestava Hello pokrývá chyb zaznamenaných klientem synchronizace hello (Azure AD Connect verze 1.1.281.0 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="524d7-140">hello report covers errors recorded by hello sync client (Azure AD Connect version 1.1.281.0 or higher)</span></span>
* <span data-ttu-id="524d7-141">V poslední operaci synchronizace hello na hello synchronizační modul obsahuje hello chybách, ke kterým došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="524d7-141">It includes hello errors that occurred in hello last synchronization operation on hello sync engine.</span></span> <span data-ttu-id="524d7-142">("Export" na hello Azure AD Connector.)</span><span class="sxs-lookup"><span data-stu-id="524d7-142">("Export" on hello Azure AD Connector.)</span></span>
* <span data-ttu-id="524d7-143">Agent Azure AD Connect Health pro synchronizaci musí mít odchozí připojení toohello požadované koncové body pro hello sestavy tooinclude hello nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="524d7-143">Azure AD Connect Health agent for sync must have outbound connectivity toohello required end points for hello report tooinclude hello latest data.</span></span>
* <span data-ttu-id="524d7-144">Sestava Hello je **aktualizované po každých 30 minut** pomocí dat hello odeslaný agenta Azure AD Connect Health pro synchronizaci. Poskytuje následující klíčové funkce hello</span><span class="sxs-lookup"><span data-stu-id="524d7-144">hello report is **updated after every 30 minutes** using hello data uploaded by Azure AD Connect Health agent for sync. It provides hello following key capabilities</span></span>

  * <span data-ttu-id="524d7-145">Kategorizace chyb</span><span class="sxs-lookup"><span data-stu-id="524d7-145">Categorization of errors</span></span>
  * <span data-ttu-id="524d7-146">Seznam chybných objektů podle kategorie</span><span class="sxs-lookup"><span data-stu-id="524d7-146">List of objects with error per category</span></span>
  * <span data-ttu-id="524d7-147">Všechny hello data o chybách hello na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="524d7-147">All hello data about hello errors at one place</span></span>
  * <span data-ttu-id="524d7-148">Souběžně straně porovnání objektů s chybou z důvodu konfliktu tooa</span><span class="sxs-lookup"><span data-stu-id="524d7-148">Side by side comparison of Objects with error due tooa conflict</span></span>
  * <span data-ttu-id="524d7-149">Stáhnout hello chybách jako systém CVS (už brzy)</span><span class="sxs-lookup"><span data-stu-id="524d7-149">Download hello error report as a CVS (coming soon)</span></span>

### <a name="categorization-of-errors"></a><span data-ttu-id="524d7-150">Kategorizace chyb</span><span class="sxs-lookup"><span data-stu-id="524d7-150">Categorization of Errors</span></span>
<span data-ttu-id="524d7-151">Sestava Hello rozděluje hello existující chyby synchronizace v hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="524d7-151">hello report categorizes hello existing synchronization errors in hello following categories:</span></span>

| <span data-ttu-id="524d7-152">Kategorie</span><span class="sxs-lookup"><span data-stu-id="524d7-152">Category</span></span> | <span data-ttu-id="524d7-153">Popis</span><span class="sxs-lookup"><span data-stu-id="524d7-153">Description</span></span> |
| --- | --- |
| <span data-ttu-id="524d7-154">Duplicitní atribut</span><span class="sxs-lookup"><span data-stu-id="524d7-154">Duplicate Attribute</span></span> |<span data-ttu-id="524d7-155">Chyby vzniklé při pokusu služby Azure AD Connect o vytvoření nebo aktualizaci objektů s duplicitními hodnotami atributů ve službě Azure AD, které musí být v tenantovi jedinečné, například proxyAddresses, UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="524d7-155">Errors when Azure AD Connect attempts create or update objects with duplicated values of one or more attributes in Azure AD that must be unique in a Tenant, such as proxyAddresses, UserPrincipalName.</span></span> |
| <span data-ttu-id="524d7-156">Neshoda dat</span><span class="sxs-lookup"><span data-stu-id="524d7-156">Data Mismatch</span></span> |<span data-ttu-id="524d7-157">Chyby při hello konfigurace soft-match selže toomatch objekty, které mít za následek chyby synchronizace.</span><span class="sxs-lookup"><span data-stu-id="524d7-157">Errors when hello soft-match fails toomatch objects that result in synchronization errors.</span></span> |
| <span data-ttu-id="524d7-158">Chyba ověřování dat</span><span class="sxs-lookup"><span data-stu-id="524d7-158">Data Validation Failure</span></span> |<span data-ttu-id="524d7-159">Chyby z důvodu tooinvalid dat, jako jsou nepodporované znaky v důležitých atributů, jako jsou třeba UserPrincipalName, formátu chyby, které nesplní ověření před zápisem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="524d7-159">Errors due tooinvalid data, such as unsupported characters in critical attributes such as UserPrincipalName, format errors that fail validation before being written in Azure AD.</span></span> |
| <span data-ttu-id="524d7-160">Rozsáhlý atribut</span><span class="sxs-lookup"><span data-stu-id="524d7-160">Large Attribute</span></span> |<span data-ttu-id="524d7-161">Chyby při jeden nebo více atributů jsou větší než hello povolená velikost, délka nebo count.</span><span class="sxs-lookup"><span data-stu-id="524d7-161">Errors when one or more attributes are larger than hello allowed size, length or count.</span></span> |
| <span data-ttu-id="524d7-162">Ostatní</span><span class="sxs-lookup"><span data-stu-id="524d7-162">Other</span></span> |<span data-ttu-id="524d7-163">Všechny ostatní chyby, které nevyhovují hello výše uvedené skupiny.</span><span class="sxs-lookup"><span data-stu-id="524d7-163">All other errors that don't fit in hello above categories.</span></span> <span data-ttu-id="524d7-164">Na základě zpětné vazby rozdělíme tuto kategorii do podkategorií.</span><span class="sxs-lookup"><span data-stu-id="524d7-164">Based on feedback, this category will be split in sub categories.</span></span> |

<span data-ttu-id="524d7-165">![Souhrnná sestava chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Kategorie sestavy chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport02.png)</span><span class="sxs-lookup"><span data-stu-id="524d7-165">![Sync Error Report Summary](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Sync Error Report Categories](./media/active-directory-aadconnect-health-sync/errorreport02.png)</span></span>

### <a name="list-of-objects-with-error-per-category"></a><span data-ttu-id="524d7-166">Seznam chybných objektů podle kategorie</span><span class="sxs-lookup"><span data-stu-id="524d7-166">List of objects with error per category</span></span>
<span data-ttu-id="524d7-167">Procházení do každé kategorie poskytne hello seznam objektů, které chybu hello do této kategorie spadají.</span><span class="sxs-lookup"><span data-stu-id="524d7-167">Drilling into each category will provide hello list of objects having hello error in that category.</span></span>
<span data-ttu-id="524d7-168">![Seznam sestav chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport03.png)</span><span class="sxs-lookup"><span data-stu-id="524d7-168">![Sync Error Report List](./media/active-directory-aadconnect-health-sync/errorreport03.png)</span></span>

### <a name="error-details"></a><span data-ttu-id="524d7-169">Podrobnosti o chybě</span><span class="sxs-lookup"><span data-stu-id="524d7-169">Error Details</span></span>
<span data-ttu-id="524d7-170">Následující data jsou k dispozici v hello podrobné zobrazení pro jednotlivé chyby</span><span class="sxs-lookup"><span data-stu-id="524d7-170">Following data is available in hello detailed view for each error</span></span>

* <span data-ttu-id="524d7-171">Identifikátory pro hello *objekt AD* související se situací</span><span class="sxs-lookup"><span data-stu-id="524d7-171">Identifiers for hello *AD Object* involved</span></span>
* <span data-ttu-id="524d7-172">Identifikátory pro hello *objekt Azure AD* podílejí (podle vhodnosti)</span><span class="sxs-lookup"><span data-stu-id="524d7-172">Identifiers for hello *Azure AD Object* involved (as applicable)</span></span>
* <span data-ttu-id="524d7-173">Popis chyby a jak toofix</span><span class="sxs-lookup"><span data-stu-id="524d7-173">Error description and how toofix</span></span>
* <span data-ttu-id="524d7-174">Související články</span><span class="sxs-lookup"><span data-stu-id="524d7-174">Related articles</span></span>

![Podrobnosti sestavy chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-hello-error-report-as-csv"></a><span data-ttu-id="524d7-176">Stáhnout hello chybách jako sdílený svazek clusteru</span><span class="sxs-lookup"><span data-stu-id="524d7-176">Download hello error report as CSV</span></span>
<span data-ttu-id="524d7-177">Výběrem hello "Export" tlačítko si můžete stáhnout soubor CSV s všechny hello podrobnosti o všech chyb hello.</span><span class="sxs-lookup"><span data-stu-id="524d7-177">By selecting hello "Export" button you can download a CSV file with all hello details about all hello errors.</span></span>

## <a name="related-links"></a><span data-ttu-id="524d7-178">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="524d7-178">Related links</span></span>
* [<span data-ttu-id="524d7-179">Řešení chyb při synchronizaci</span><span class="sxs-lookup"><span data-stu-id="524d7-179">Troubleshooting Errors during synchronization</span></span>](../connect/active-directory-aadconnect-troubleshoot-sync-errors.md)
* [<span data-ttu-id="524d7-180">Odolnost duplicitních atributů</span><span class="sxs-lookup"><span data-stu-id="524d7-180">Duplicate Attribute Resiliency</span></span>](../connect/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [<span data-ttu-id="524d7-181">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="524d7-181">Azure AD Connect Health</span></span>](active-directory-aadconnect-health.md)
* [<span data-ttu-id="524d7-182">Instalace agenta služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="524d7-182">Azure AD Connect Health Agent Installation</span></span>](active-directory-aadconnect-health-agent-install.md)
* [<span data-ttu-id="524d7-183">Operace služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="524d7-183">Azure AD Connect Health Operations</span></span>](active-directory-aadconnect-health-operations.md)
* [<span data-ttu-id="524d7-184">Používání služby Azure AD Connect Health se službou AD FS</span><span class="sxs-lookup"><span data-stu-id="524d7-184">Using Azure AD Connect Health with AD FS</span></span>](active-directory-aadconnect-health-adfs.md)
* [<span data-ttu-id="524d7-185">Používání služby Azure AD Connect Health se službou AD DS</span><span class="sxs-lookup"><span data-stu-id="524d7-185">Using Azure AD Connect Health with AD DS</span></span>](active-directory-aadconnect-health-adds.md)
* [<span data-ttu-id="524d7-186">Azure AD Connect Health – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="524d7-186">Azure AD Connect Health FAQ</span></span>](active-directory-aadconnect-health-faq.md)
* [<span data-ttu-id="524d7-187">Historie verzí služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="524d7-187">Azure AD Connect Health Version History</span></span>](active-directory-aadconnect-health-version-history.md)