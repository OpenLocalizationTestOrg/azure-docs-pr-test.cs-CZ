---
title: "Auditování v Azure SQL Data Warehouse | Microsoft Docs"
description: "Začínáme s auditování v Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 08/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: f851c82ebeaa647f663d499a4d327c3479e36121
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a><span data-ttu-id="dbfd7-103">Auditování v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="dbfd7-103">Auditing in Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dbfd7-104">Auditování</span><span class="sxs-lookup"><span data-stu-id="dbfd7-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="dbfd7-105">Detekce hrozeb</span><span class="sxs-lookup"><span data-stu-id="dbfd7-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

<span data-ttu-id="dbfd7-106">Auditování SQL Data Warehouse umožňuje záznam, který protokolují se události v databázi auditování v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-106">SQL Data Warehouse auditing allows you to record events in your database to an audit log in your Azure Storage account.</span></span> <span data-ttu-id="dbfd7-107">Auditování vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-107">Auditing can help you maintain regulatory compliance, understand  database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span> <span data-ttu-id="dbfd7-108">Auditování SQL Data Warehouse taky integruje s Microsoft Power BI pro procházení analýz a generování sestav.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-108">SQL Data Warehouse auditing also integrates with Microsoft Power BI for drill-down reporting and analysis.</span></span>

<span data-ttu-id="dbfd7-109">Nástroje auditování povolit a zajištění dodržování standardů dodržování předpisů, ale nezaručují dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-109">Auditing tools enable and facilitate adherence to compliance standards but don't guarantee compliance.</span></span> <span data-ttu-id="dbfd7-110">Další informace o Azure programy dodržování standardů tuto podporu, najdete v části <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Centrum zabezpečení Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-110">For more information about Azure programs that support standards compliance, see the <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Trust Center</a>.</span></span>

* <span data-ttu-id="dbfd7-111">[Základy auditování databáze]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-111">[Database Auditing basics]</span></span>
* <span data-ttu-id="dbfd7-112">[Nastavení auditování pro vaši databázi]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-112">[Set up auditing for your database]</span></span>
* <span data-ttu-id="dbfd7-113">[Analýza protokolů auditu a sestavy]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-113">[Analyze audit logs and reports]</span></span>

## <span data-ttu-id="dbfd7-114"><a id="subheading-1"></a>Základy Azure SQL Data Warehouse databáze auditování</span><span class="sxs-lookup"><span data-stu-id="dbfd7-114"><a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing basics</span></span>
<span data-ttu-id="dbfd7-115">Auditování databáze SQL Data Warehouse umožňuje:</span><span class="sxs-lookup"><span data-stu-id="dbfd7-115">SQL Data Warehouse database auditing allows you to:</span></span>

* <span data-ttu-id="dbfd7-116">**Zachovat** monitorovat vybrané události.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-116">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="dbfd7-117">Můžete definovat kategorie akce databáze k auditování.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-117">You can define categories of database actions  to be audited.</span></span>
* <span data-ttu-id="dbfd7-118">**Sestava** v databázové aktivitě.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-118">**Report** on database activity.</span></span> <span data-ttu-id="dbfd7-119">Abyste mohli rychle začít s aktivity a události vytváření sestav můžete předem nakonfigurované sestavy a řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-119">You can use preconfigured reports and a dashboard to get started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="dbfd7-120">**Analýza** sestavy.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-120">**Analyze** reports.</span></span> <span data-ttu-id="dbfd7-121">Můžete najít podezřelé události, neobvyklé aktivity a trendy.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-121">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="dbfd7-122">Můžete nakonfigurovat auditování pro kategorii událostí následující:</span><span class="sxs-lookup"><span data-stu-id="dbfd7-122">You can configure auditing for the following event categories:</span></span>

<span data-ttu-id="dbfd7-123">**Nešifrovaná SQL** a **parametry SQL** pro které jsou klasifikovány protokolů auditu shromážděných jako</span><span class="sxs-lookup"><span data-stu-id="dbfd7-123">**Plain SQL** and **Parameterized SQL** for which the collected audit logs are classified as</span></span>  

* <span data-ttu-id="dbfd7-124">**Přístup k datům**</span><span class="sxs-lookup"><span data-stu-id="dbfd7-124">**Access to data**</span></span>
* <span data-ttu-id="dbfd7-125">**Změny schématu (DDL)**</span><span class="sxs-lookup"><span data-stu-id="dbfd7-125">**Schema changes (DDL)**</span></span>
* <span data-ttu-id="dbfd7-126">**Změny dat (DML)**</span><span class="sxs-lookup"><span data-stu-id="dbfd7-126">**Data changes (DML)**</span></span>
* <span data-ttu-id="dbfd7-127">**Účty, rolí a oprávnění (DCL)**</span><span class="sxs-lookup"><span data-stu-id="dbfd7-127">**Accounts, roles, and permissions (DCL)**</span></span>
* <span data-ttu-id="dbfd7-128">**Uložené procedury**, **přihlášení** a, **transakce správu**.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-128">**Stored Procedure**, **Login** and, **Transaction Management**.</span></span>

<span data-ttu-id="dbfd7-129">Pro každou kategorii událostí auditování z **úspěch** a **selhání** operace jsou nakonfigurovány odděleně.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-129">For each Event Category, Auditing of **Success** and **Failure** operations are configured separately.</span></span>

<span data-ttu-id="dbfd7-130">Další informace o aktivitách a auditovat události najdete v tématu <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">odkazu formátu protokolu auditu (soubor dokumentace ke stažení)</a>.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-130">For more information about the activities and events audited, see the <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Audit Log Format Reference (doc file download)</a>.</span></span>

<span data-ttu-id="dbfd7-131">Protokoly auditu jsou uložené v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-131">Audit logs are stored in your Azure storage account.</span></span> <span data-ttu-id="dbfd7-132">Můžete definovat po dobu uchování protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-132">You can define an audit log retention period.</span></span>

<span data-ttu-id="dbfd7-133">Zásady pro auditování lze definovat pro konkrétní databázi nebo jako výchozí zásady serveru.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-133">An auditing policy can be defined for a specific database or as a default server policy.</span></span> <span data-ttu-id="dbfd7-134">Výchozí zásady auditování serveru se vztahuje na všechny databáze na serveru, které nemají konkrétní přepsání databáze auditování definované zásady.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-134">A default server auditing policy applies to all databases on a server, which do not have a specific overriding database auditing policy defined.</span></span>

<span data-ttu-id="dbfd7-135">Před nastavením auditování kontrolu, pokud používáte auditu ["Starších klientů."](sql-data-warehouse-auditing-downlevel-clients.md)</span><span class="sxs-lookup"><span data-stu-id="dbfd7-135">Before setting up audit auditing check if you are using a ["Downlevel Client."](sql-data-warehouse-auditing-downlevel-clients.md)</span></span>

## <span data-ttu-id="dbfd7-136"><a id="subheading-2"></a>Nastavení auditování pro vaši databázi</span><span class="sxs-lookup"><span data-stu-id="dbfd7-136"><a id="subheading-2"></a>Set up auditing for your database</span></span>
1. <span data-ttu-id="dbfd7-137">Spusťte <a href="https://portal.azure.com" target="_blank">portál Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-137">Launch the <a href="https://portal.azure.com" target="_blank">Azure portal</a>.</span></span>
2. <span data-ttu-id="dbfd7-138">Přejděte na **nastavení** okna SQL Data Warehouse, které chcete auditovat.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-138">Go to the **Settings** blade of the SQL Data Warehouse you want to audit.</span></span> <span data-ttu-id="dbfd7-139">V **nastavení** vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-139">In the **Settings** blade, select **Auditing & Threat detection**.</span></span>
   
    ![][1]
3. <span data-ttu-id="dbfd7-140">Dál povolte auditování kliknutím **ON** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-140">Next, enable auditing by clicking the **ON** button.</span></span>
   
    ![][3]
4. <span data-ttu-id="dbfd7-141">V okně Konfigurace auditování vyberte **podrobnosti úložiště** otevřete okno úložiště protokoly auditu.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-141">In the auditing configuration blade, select **STORAGE DETAILS** to open the Audit Logs Storage blade.</span></span> <span data-ttu-id="dbfd7-142">Vyberte účet úložiště Azure, kam bude uložena protokoly a dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-142">Select the Azure storage account where logs will be saved and, the retention period.</span></span> 
>[!TIP]
><span data-ttu-id="dbfd7-143">Použijte stejný účet úložiště pro všechny databáze, auditované k plnému využití mimo šablony předem nakonfigurované sestavy.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-143">Use the same storage account for all audited databases to get the most out of the pre-configured reports templates.</span></span>
   
    ![][4]
5. <span data-ttu-id="dbfd7-144">Klikněte **OK** tlačítko pro uložení podrobnosti o konfiguraci úložiště.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-144">Click the **OK** button to save the storage details configuration.</span></span>
6. <span data-ttu-id="dbfd7-145">V části **protokolování podle událostí**, klikněte na tlačítko **úspěch** a **selhání** protokolovaly všechny události, nebo zvolte kategorií jednotlivých událostí.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-145">Under **LOGGING BY EVENT**, click **SUCCESS** and **FAILURE** to log all events, or choose individual event categories.</span></span>
7. <span data-ttu-id="dbfd7-146">Pokud konfigurujete auditování pro databázi, musíte změnit připojovací řetězec vašeho klienta zajistit, že data auditování pořízen správně.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-146">If you are configuring Auditing for a database, you may need to alter the connection string of your client to ensure data auditing is correctly captured.</span></span> <span data-ttu-id="dbfd7-147">Zkontrolujte [upravit FDQN serveru v připojovacím řetězci](sql-data-warehouse-auditing-downlevel-clients.md) tématu pro připojení klientů nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-147">Check the [Modify Server FDQN in the connection string](sql-data-warehouse-auditing-downlevel-clients.md) topic for downlevel client connections.</span></span>
8. <span data-ttu-id="dbfd7-148">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-148">Click **OK**.</span></span>

## <span data-ttu-id="dbfd7-149"><a id="subheading-3"></a>Analýza protokolů auditu a sestavy</span><span class="sxs-lookup"><span data-stu-id="dbfd7-149"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="dbfd7-150">Protokoly auditu je agregován v kolekci úložiště tabulek s **SQLDBAuditLogs** předpony v účtu úložiště Azure, který jste zvolili během instalace.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-150">Audit logs are aggregated in a collection of Store Tables with a **SQLDBAuditLogs** prefix in the Azure storage account you chose during setup.</span></span> <span data-ttu-id="dbfd7-151">Můžete zobrazit soubory protokolů pomocí některého nástroje, jako třeba <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-151">You can view log files using a tool such as <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.</span></span>

<span data-ttu-id="dbfd7-152">Šablona sestavy předkonfigurované řídicího panelu je k dispozici jako <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">ke stažení tabulku aplikace Excel</a> můžete rychle analyzovat data protokolu.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-152">A preconfigured dashboard report template is available as a <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadable Excel spreadsheet</a> to help you quickly analyze log data.</span></span> <span data-ttu-id="dbfd7-153">Abyste mohli použít šablonu na vaše protokoly auditu, budete potřebovat Excel 2013 nebo novější a Power Query, kterou si můžete stáhnout <a href="http://www.microsoft.com/download/details.aspx?id=39379">zde</a>.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-153">To use the template on your audit logs, you need Excel 2013 or later and Power Query, which you can download <a href="http://www.microsoft.com/download/details.aspx?id=39379">here</a>.</span></span>

<span data-ttu-id="dbfd7-154">Šablona má fiktivních ukázkových dat v něm a můžete nastavit Power Query pro import auditní protokol přímo z vašeho účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-154">The template has fictional sample data in it, and you can set up Power Query to import your audit log directly from your Azure storage account.</span></span>

## <span data-ttu-id="dbfd7-155"><a id="subheading-4"></a>Opětovné generování klíče úložiště</span><span class="sxs-lookup"><span data-stu-id="dbfd7-155"><a id="subheading-4"></a>Storage key regeneration</span></span>
<span data-ttu-id="dbfd7-156">V produkčním prostředí budete pravděpodobně pravidelně aktualizovat klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-156">In production, you are likely to refresh your storage keys periodically.</span></span> <span data-ttu-id="dbfd7-157">Při aktualizaci klíče, je nutné uložit zásadu.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-157">When refreshing your keys, you need to save the policy.</span></span> <span data-ttu-id="dbfd7-158">Proces je následující:</span><span class="sxs-lookup"><span data-stu-id="dbfd7-158">The process is as follows:</span></span>

1. <span data-ttu-id="dbfd7-159">V okně Konfigurace auditování (popsaný výše v instalačním programu auditování část) přepnout **přístupový klíč k úložišti** z *primární* k *sekundární* a **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-159">In the auditing configuration blade (described above in the setup auditing section) switch the **Storage Access Key** from *Primary* to *Secondary* and **SAVE**.</span></span>

   ![][4]
2. <span data-ttu-id="dbfd7-160">Přejděte do okna konfigurace úložiště a **znovu vygenerovat** *primární přístupový klíč*.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-160">Go to the storage configuration blade and **regenerate** the *Primary Access Key*.</span></span>
3. <span data-ttu-id="dbfd7-161">Přejděte zpět do okna Konfigurace auditování, přepněte **přístupový klíč k úložišti** z *sekundární* k *primární* a stiskněte klávesu **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-161">Go back to the auditing configuration blade, switch the **Storage Access Key** from *Secondary* to *Primary* and press **SAVE**.</span></span>
4. <span data-ttu-id="dbfd7-162">Přejděte zpět na úložiště uživatelského rozhraní a **znovu vygenerovat** *sekundární přístupový klíč* (jako příprava pro další aktualizace cyklu klíče.</span><span class="sxs-lookup"><span data-stu-id="dbfd7-162">Go back to the storage UI and **regenerate** the *Secondary Access Key* (as preparation for the next keys refresh cycle.</span></span>

## <span data-ttu-id="dbfd7-163"><a id="subheading-5"></a>Automatizace (prostředí PowerShell nebo REST API)</span><span class="sxs-lookup"><span data-stu-id="dbfd7-163"><a id="subheading-5"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="dbfd7-164">Můžete taky nakonfigurovat auditování v Azure SQL Data Warehouse pomocí následujících nástrojů automatizace:</span><span class="sxs-lookup"><span data-stu-id="dbfd7-164">You can also configure auditing in Azure SQL Data Warehouse by using the following automation tools:</span></span>

* <span data-ttu-id="dbfd7-165">**Rutiny prostředí PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="dbfd7-165">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="dbfd7-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="dbfd7-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="dbfd7-168">[Odebrat AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-168">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="dbfd7-169">[Odebrat AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-169">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="dbfd7-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="dbfd7-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="dbfd7-172">[Použití AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="dbfd7-172">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

<!--Anchors-->
[Základy auditování databáze]: #subheading-1
[Nastavení auditování pro vaši databázi]: #subheading-2
[Analýza protokolů auditu a sestavy]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy