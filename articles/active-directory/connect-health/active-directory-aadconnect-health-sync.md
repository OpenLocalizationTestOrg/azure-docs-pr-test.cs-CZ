---
title: "Používání služby Azure AD Connect Health se synchronizací | Dokumentace Microsoftu"
description: "Toto je stránka o službě Azure AD Connect Health, která popisuje sledování synchronizace Azure AD Connect."
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
ms.openlocfilehash: 4b06338cb62cc458e7b097db36023f0746d4e969
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-azure-ad-connect-sync-with-azure-ad-connect-health"></a><span data-ttu-id="028ad-103">Sledování synchronizace Azure AD Connect pomocí služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="028ad-103">Monitor Azure AD Connect sync with Azure AD Connect Health</span></span>
<span data-ttu-id="028ad-104">Následující dokumentace se věnuje sledování služby Azure AD Connect (Sync) pomocí služby Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="028ad-104">The following documentation is specific to monitoring Azure AD Connect (Sync) with Azure AD Connect Health.</span></span>  <span data-ttu-id="028ad-105">Informace o sledování služby AD FS pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md).</span><span class="sxs-lookup"><span data-stu-id="028ad-105">For information on monitoring AD FS with Azure AD Connect Health see [Using Azure AD Connect Health with AD FS](active-directory-aadconnect-health-adfs.md).</span></span> <span data-ttu-id="028ad-106">Informace o sledování služby Active Directory Domain Services pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md).</span><span class="sxs-lookup"><span data-stu-id="028ad-106">Additionally, for information on monitoring Active Directory Domain Services with Azure AD Connect Health see [Using Azure AD Connect Health with AD DS](active-directory-aadconnect-health-adds.md).</span></span>

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a><span data-ttu-id="028ad-108">Upozornění služby Azure AD Connect Health pro synchronizaci</span><span class="sxs-lookup"><span data-stu-id="028ad-108">Alerts for Azure AD Connect Health for sync</span></span>
<span data-ttu-id="028ad-109">Část pojednávající o výstrahách služby Azure AD Connect Health pro synchronizaci uvádí seznam aktivních upozornění.</span><span class="sxs-lookup"><span data-stu-id="028ad-109">The Azure AD Connect Health Alerts for sync section provides you the list of active alerts.</span></span> <span data-ttu-id="028ad-110">Každé upozornění obsahuje důležité informace, postup řešení a odkazy na související dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="028ad-110">Each alert includes relevant information, resolution steps, and links to related documentation.</span></span> <span data-ttu-id="028ad-111">Výběrem aktivního nebo vyřešeného upozornění zobrazíte nové okno s doplňujícími informacemi, kroky, které můžete k vyřešení upozornění použít, a odkazy na další dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="028ad-111">By selecting an active or resolved alert you will see a new blade with additional information, as well as steps you can take to resolve the alert, and links to additional documentation.</span></span> <span data-ttu-id="028ad-112">Můžete si zobrazit i historické údaje o dříve vyřešených upozorněních.</span><span class="sxs-lookup"><span data-stu-id="028ad-112">You can also view historical data on alerts that were resolved in the past.</span></span>

<span data-ttu-id="028ad-113">Výběrem některého upozornění zobrazíte doplňující informace, kroky, které můžete k vyřešení upozornění použít, a odkazy na další dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="028ad-113">By selecting an alert you will be provided with additional information as well as steps you can take to resolve the alert and links to additional documentation.</span></span>

![Chyba synchronizace služby Azure AD Connect](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a><span data-ttu-id="028ad-115">Omezené vyhodnocení upozornění</span><span class="sxs-lookup"><span data-stu-id="028ad-115">Limited Evaluation of Alerts</span></span>
<span data-ttu-id="028ad-116">Pokud služba Azure AD Connect nepoužívá výchozí konfiguraci (například když je filtrování atributů změněné z výchozí konfigurace na vlastní), agent služby Azure AD Connect Health nebude odesílat chybové události související se službou Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="028ad-116">If Azure AD Connect is NOT using the default configuration (for example, if Attribute Filtering is changed from the default configuration to a custom configuration), then the Azure AD Connect Health agent will not upload the error events related to Azure AD Connect.</span></span>

<span data-ttu-id="028ad-117">Služba tak bude při vyhodnocování upozornění omezená.</span><span class="sxs-lookup"><span data-stu-id="028ad-117">This limits the evaluation of alerts by the service.</span></span> <span data-ttu-id="028ad-118">Zobrazí se banner, který v rámci služby upozorňuje na tento stav na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="028ad-118">You'd will see a banner that indicates this condition in the Azure Portal under your service.</span></span>

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/banner.png)

<span data-ttu-id="028ad-120">Můžete to změnit kliknutím na „Nastavení“ a povolením, aby agent služby Azure AD Connect Health mohl odesílat všechny protokoly chyb.</span><span class="sxs-lookup"><span data-stu-id="028ad-120">You can change this by clicking "Settings" and allowing Azure AD Connect Health agent to upload all error logs.</span></span>

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a><span data-ttu-id="028ad-122">Analýza synchronizace</span><span class="sxs-lookup"><span data-stu-id="028ad-122">Sync Insight</span></span>
<span data-ttu-id="028ad-123">Správce často zajímá, jak dlouho trvá synchronizace změn do služby Azure AD, a množství změn, které probíhají.</span><span class="sxs-lookup"><span data-stu-id="028ad-123">Admins Frequently want to know about the time it takes to sync changes to Azure AD and the amount of changes taking place.</span></span> <span data-ttu-id="028ad-124">Tato funkce poskytuje snadný způsob vizualizace těchto informací pomocí uvedených grafů:</span><span class="sxs-lookup"><span data-stu-id="028ad-124">This feature provides an easy way to visualize this using the below graphs:</span></span>   

* <span data-ttu-id="028ad-125">sledování latence operací synchronizace</span><span class="sxs-lookup"><span data-stu-id="028ad-125">Latency of sync operations</span></span>
* <span data-ttu-id="028ad-126">sledování trendu změn objektů</span><span class="sxs-lookup"><span data-stu-id="028ad-126">Object Change trend</span></span>

### <a name="sync-latency"></a><span data-ttu-id="028ad-127">Latence synchronizace</span><span class="sxs-lookup"><span data-stu-id="028ad-127">Sync Latency</span></span>
<span data-ttu-id="028ad-128">Tato funkce poskytuje grafické zobrazení trendu latence operací synchronizace (import, export atd.) pro jednotlivé konektory.</span><span class="sxs-lookup"><span data-stu-id="028ad-128">This feature provides a graphical trend of latency of the sync operations (import, export, etc.) for connectors.</span></span>  <span data-ttu-id="028ad-129">Díky tomu se nejen rychle a snadno seznámíte s latencí operací (latence je větší, pokud máte velké sady změn), ale budete moct i zjišťovat anomálie v latenci, které můžou vyžadovat další šetření.</span><span class="sxs-lookup"><span data-stu-id="028ad-129">This provides a quick and easy way to understand not only the latency of your operations (larger if you have a large set of changes occurring) but also a way to detect anomalies in the latency that may require further investigation.</span></span>

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/synclatency02.png)

<span data-ttu-id="028ad-131">Ve výchozím nastavení se zobrazuje jenom latence operace „exportu“ na konektoru Azure AD.</span><span class="sxs-lookup"><span data-stu-id="028ad-131">By default, only the latency of the 'Export' operation for the Azure AD connector is shown.</span></span>  <span data-ttu-id="028ad-132">Pokud chcete zobrazit další operace na konektoru nebo zobrazit operace z jiných konektorů, klikněte pravým tlačítkem na graf, vyberte Upravit graf, nebo klikněte na tlačítko Upravit graf latence a zvolte konkrétní operaci a konektor.</span><span class="sxs-lookup"><span data-stu-id="028ad-132">To see more operations on the connector or to view operations from other connectors, right-click on the chart,  select Edit Chart or click on the "Edit Latency Chart" button and choose the specific operation and connectors.</span></span>

### <a name="sync-object-changes"></a><span data-ttu-id="028ad-133">Změny objektů synchronizace</span><span class="sxs-lookup"><span data-stu-id="028ad-133">Sync Object Changes</span></span>
<span data-ttu-id="028ad-134">Tato funkce nabízí grafické zobrazení trendu v počtu změn, které se vyhodnocují a exportují do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="028ad-134">This feature provides a graphical trend of the number of changes that are being evaluated and exported to Azure AD.</span></span>  <span data-ttu-id="028ad-135">V současné době je snaha o shromáždění těchto informací z protokolů synchronizace obtížná.</span><span class="sxs-lookup"><span data-stu-id="028ad-135">Today, trying to gather this information from the sync logs is difficult.</span></span>  <span data-ttu-id="028ad-136">Graf poskytuje nejen jednodušší způsob sledování počtu změn ve vašem prostředí, ale i vizuální zobrazení chyb, ke kterým dochází.</span><span class="sxs-lookup"><span data-stu-id="028ad-136">The chart gives you, not only a simpler way of monitoring the number of changes that are occurring in your environment, but also a visual view of the failures that are occurring.</span></span>

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a><span data-ttu-id="028ad-138">Sestava chyb synchronizace na úrovni objektu (verze Preview)</span><span class="sxs-lookup"><span data-stu-id="028ad-138">Object Level Synchronization Error Report (Preview)</span></span>
<span data-ttu-id="028ad-139">Tato funkce poskytuje sestavu chyb synchronizace, ke kterým může dojít při synchronizaci dat identity mezi službou Windows Server AD a Azure AD pomocí služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="028ad-139">This feature provides a report about synchronization errors that can occur when identity data is synchronized between Windows Server AD and Azure AD using Azure AD Connect.</span></span>

* <span data-ttu-id="028ad-140">Sestava obsahuje chyby zaznamenané klientem synchronizace (Azure AD Connect verze 1.1.281.0 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="028ad-140">The report covers errors recorded by the sync client (Azure AD Connect version 1.1.281.0 or higher)</span></span>
* <span data-ttu-id="028ad-141">Zahrnuje chyby, ke kterým došlo při poslední operaci synchronizace u synchronizačního modulu.</span><span class="sxs-lookup"><span data-stu-id="028ad-141">It includes the errors that occurred in the last synchronization operation on the sync engine.</span></span> <span data-ttu-id="028ad-142">(Export v konektoru Azure AD)</span><span class="sxs-lookup"><span data-stu-id="028ad-142">("Export" on the Azure AD Connector.)</span></span>
* <span data-ttu-id="028ad-143">Agent služby Azure AD Connect Health pro synchronizaci musí mít odchozí připojení k požadovaným koncovým bodům, aby se v sestavě mohla promítnout nejnovější data.</span><span class="sxs-lookup"><span data-stu-id="028ad-143">Azure AD Connect Health agent for sync must have outbound connectivity to the required end points for the report to include the latest data.</span></span>
* <span data-ttu-id="028ad-144">Sestava se **aktualizuje každých 30 minut** pomocí dat nahraných agentem služby Azure AD Connect Health pro synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="028ad-144">The report is **updated after every 30 minutes** using the data uploaded by Azure AD Connect Health agent for sync.</span></span>
  <span data-ttu-id="028ad-145">Poskytuje následující klíčové funkce:</span><span class="sxs-lookup"><span data-stu-id="028ad-145">It provides the following key capabilities</span></span>

  * <span data-ttu-id="028ad-146">Kategorizace chyb</span><span class="sxs-lookup"><span data-stu-id="028ad-146">Categorization of errors</span></span>
  * <span data-ttu-id="028ad-147">Seznam chybných objektů podle kategorie</span><span class="sxs-lookup"><span data-stu-id="028ad-147">List of objects with error per category</span></span>
  * <span data-ttu-id="028ad-148">Všechna data o chybách na jednom místě</span><span class="sxs-lookup"><span data-stu-id="028ad-148">All the data about the errors at one place</span></span>
  * <span data-ttu-id="028ad-149">Souběžné porovnání objektů, u kterých došlo k chybě z důvodu konfliktu</span><span class="sxs-lookup"><span data-stu-id="028ad-149">Side by side comparison of Objects with error due to a conflict</span></span>
  * <span data-ttu-id="028ad-150">Stažení sestavy chyb ve formátu CSV (připravuje se)</span><span class="sxs-lookup"><span data-stu-id="028ad-150">Download the error report as a CVS (coming soon)</span></span>

### <a name="categorization-of-errors"></a><span data-ttu-id="028ad-151">Kategorizace chyb</span><span class="sxs-lookup"><span data-stu-id="028ad-151">Categorization of Errors</span></span>
<span data-ttu-id="028ad-152">Sestava zařazuje stávající chyby synchronizace do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="028ad-152">The report categorizes the existing synchronization errors in the following categories:</span></span>

| <span data-ttu-id="028ad-153">Kategorie</span><span class="sxs-lookup"><span data-stu-id="028ad-153">Category</span></span> | <span data-ttu-id="028ad-154">Popis</span><span class="sxs-lookup"><span data-stu-id="028ad-154">Description</span></span> |
| --- | --- |
| <span data-ttu-id="028ad-155">Duplicitní atribut</span><span class="sxs-lookup"><span data-stu-id="028ad-155">Duplicate Attribute</span></span> |<span data-ttu-id="028ad-156">Chyby vzniklé při pokusu služby Azure AD Connect o vytvoření nebo aktualizaci objektů s duplicitními hodnotami atributů ve službě Azure AD, které musí být v tenantovi jedinečné, například proxyAddresses, UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="028ad-156">Errors when Azure AD Connect attempts create or update objects with duplicated values of one or more attributes in Azure AD that must be unique in a Tenant, such as proxyAddresses, UserPrincipalName.</span></span> |
| <span data-ttu-id="028ad-157">Neshoda dat</span><span class="sxs-lookup"><span data-stu-id="028ad-157">Data Mismatch</span></span> |<span data-ttu-id="028ad-158">Chyby synchronizace vzniklé v důsledku neúspěšného měkkého párování objektů</span><span class="sxs-lookup"><span data-stu-id="028ad-158">Errors when the soft-match fails to match objects that result in synchronization errors.</span></span> |
| <span data-ttu-id="028ad-159">Chyba ověřování dat</span><span class="sxs-lookup"><span data-stu-id="028ad-159">Data Validation Failure</span></span> |<span data-ttu-id="028ad-160">Chyby vzniklé v důsledku neplatných dat, jako jsou nepodporované znaky v klíčových atributech (např. UserPrincipalName), chyby formátování, které se před zápisem do Azure AD nepodaří ověřit</span><span class="sxs-lookup"><span data-stu-id="028ad-160">Errors due to invalid data, such as unsupported characters in critical attributes such as UserPrincipalName, format errors that fail validation before being written in Azure AD.</span></span> |
| <span data-ttu-id="028ad-161">Rozsáhlý atribut</span><span class="sxs-lookup"><span data-stu-id="028ad-161">Large Attribute</span></span> |<span data-ttu-id="028ad-162">Chyby vzniklé v důsledku toho, že některé atributy překračují povolenou velikost, délku nebo počet</span><span class="sxs-lookup"><span data-stu-id="028ad-162">Errors when one or more attributes are larger than the allowed size, length or count.</span></span> |
| <span data-ttu-id="028ad-163">Ostatní</span><span class="sxs-lookup"><span data-stu-id="028ad-163">Other</span></span> |<span data-ttu-id="028ad-164">Všechny ostatní chyby, které nevyhovují uvedeným kategoriím</span><span class="sxs-lookup"><span data-stu-id="028ad-164">All other errors that don't fit in the above categories.</span></span> <span data-ttu-id="028ad-165">Na základě zpětné vazby rozdělíme tuto kategorii do podkategorií.</span><span class="sxs-lookup"><span data-stu-id="028ad-165">Based on feedback, this category will be split in sub categories.</span></span> |

<span data-ttu-id="028ad-166">![Souhrnná sestava chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Kategorie sestavy chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport02.png)</span><span class="sxs-lookup"><span data-stu-id="028ad-166">![Sync Error Report Summary](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Sync Error Report Categories](./media/active-directory-aadconnect-health-sync/errorreport02.png)</span></span>

### <a name="list-of-objects-with-error-per-category"></a><span data-ttu-id="028ad-167">Seznam chybných objektů podle kategorie</span><span class="sxs-lookup"><span data-stu-id="028ad-167">List of objects with error per category</span></span>
<span data-ttu-id="028ad-168">Rozbalením jednotlivých kategorií zobrazíte seznam objektů, které mají chybu v dané kategorii.</span><span class="sxs-lookup"><span data-stu-id="028ad-168">Drilling into each category will provide the list of objects having the error in that category.</span></span>
<span data-ttu-id="028ad-169">![Seznam sestav chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport03.png)</span><span class="sxs-lookup"><span data-stu-id="028ad-169">![Sync Error Report List](./media/active-directory-aadconnect-health-sync/errorreport03.png)</span></span>

### <a name="error-details"></a><span data-ttu-id="028ad-170">Podrobnosti o chybě</span><span class="sxs-lookup"><span data-stu-id="028ad-170">Error Details</span></span>
<span data-ttu-id="028ad-171">Následující data jsou k dispozici v podrobném zobrazení jednotlivých chyb.</span><span class="sxs-lookup"><span data-stu-id="028ad-171">Following data is available in the detailed view for each error</span></span>

* <span data-ttu-id="028ad-172">Identifikátory příslušného *objektu AD*</span><span class="sxs-lookup"><span data-stu-id="028ad-172">Identifiers for the *AD Object* involved</span></span>
* <span data-ttu-id="028ad-173">Identifikátory příslušného *objektu Azure AD* (podle vhodnosti)</span><span class="sxs-lookup"><span data-stu-id="028ad-173">Identifiers for the *Azure AD Object* involved (as applicable)</span></span>
* <span data-ttu-id="028ad-174">Popis chyby a její řešení</span><span class="sxs-lookup"><span data-stu-id="028ad-174">Error description and how to fix</span></span>
* <span data-ttu-id="028ad-175">Související články</span><span class="sxs-lookup"><span data-stu-id="028ad-175">Related articles</span></span>

![Podrobnosti sestavy chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a><span data-ttu-id="028ad-177">Stažení sestavy chyb ve formátu CSV</span><span class="sxs-lookup"><span data-stu-id="028ad-177">Download the error report as CSV</span></span>
<span data-ttu-id="028ad-178">Pomocí tlačítka Exportovat můžete stáhnout soubor CSV s podrobnými informacemi o všech chybách.</span><span class="sxs-lookup"><span data-stu-id="028ad-178">By selecting the "Export" button you can download a CSV file with all the details about all the errors.</span></span>

## <a name="related-links"></a><span data-ttu-id="028ad-179">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="028ad-179">Related links</span></span>
* [<span data-ttu-id="028ad-180">Řešení chyb při synchronizaci</span><span class="sxs-lookup"><span data-stu-id="028ad-180">Troubleshooting Errors during synchronization</span></span>](../connect/active-directory-aadconnect-troubleshoot-sync-errors.md)
* [<span data-ttu-id="028ad-181">Odolnost duplicitních atributů</span><span class="sxs-lookup"><span data-stu-id="028ad-181">Duplicate Attribute Resiliency</span></span>](../connect/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [<span data-ttu-id="028ad-182">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="028ad-182">Azure AD Connect Health</span></span>](active-directory-aadconnect-health.md)
* [<span data-ttu-id="028ad-183">Instalace agenta služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="028ad-183">Azure AD Connect Health Agent Installation</span></span>](active-directory-aadconnect-health-agent-install.md)
* [<span data-ttu-id="028ad-184">Operace služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="028ad-184">Azure AD Connect Health Operations</span></span>](active-directory-aadconnect-health-operations.md)
* [<span data-ttu-id="028ad-185">Používání služby Azure AD Connect Health se službou AD FS</span><span class="sxs-lookup"><span data-stu-id="028ad-185">Using Azure AD Connect Health with AD FS</span></span>](active-directory-aadconnect-health-adfs.md)
* [<span data-ttu-id="028ad-186">Používání služby Azure AD Connect Health se službou AD DS</span><span class="sxs-lookup"><span data-stu-id="028ad-186">Using Azure AD Connect Health with AD DS</span></span>](active-directory-aadconnect-health-adds.md)
* [<span data-ttu-id="028ad-187">Azure AD Connect Health – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="028ad-187">Azure AD Connect Health FAQ</span></span>](active-directory-aadconnect-health-faq.md)
* [<span data-ttu-id="028ad-188">Historie verzí služby Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="028ad-188">Azure AD Connect Health Version History</span></span>](active-directory-aadconnect-health-version-history.md)