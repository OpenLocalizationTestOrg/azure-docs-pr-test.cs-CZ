---
title: aaaAuditing v Azure SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 948de74fa052ef206cf1aa65c0d81f084b18cb00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a><span data-ttu-id="19a84-103">Auditování v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="19a84-103">Auditing in Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="19a84-104">Auditování</span><span class="sxs-lookup"><span data-stu-id="19a84-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="19a84-105">Detekce hrozeb</span><span class="sxs-lookup"><span data-stu-id="19a84-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

<span data-ttu-id="19a84-106">Auditování SQL Data Warehouse vám umožní toorecord události z protokolu auditu tooan databáze ve vašem účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="19a84-106">SQL Data Warehouse auditing allows you toorecord events in your database tooan audit log in your Azure Storage account.</span></span> <span data-ttu-id="19a84-107">Auditování vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="19a84-107">Auditing can help you maintain regulatory compliance, understand  database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span> <span data-ttu-id="19a84-108">Auditování SQL Data Warehouse taky integruje s Microsoft Power BI pro procházení analýz a generování sestav.</span><span class="sxs-lookup"><span data-stu-id="19a84-108">SQL Data Warehouse auditing also integrates with Microsoft Power BI for drill-down reporting and analysis.</span></span>

<span data-ttu-id="19a84-109">Nástroje auditování povolit a zajištění dodržování standardů toocompliance ale nezaručují dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="19a84-109">Auditing tools enable and facilitate adherence toocompliance standards but don't guarantee compliance.</span></span> <span data-ttu-id="19a84-110">Další informace o Azure programy dodržování standardů tuto podporu, najdete v části hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Centrum zabezpečení Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="19a84-110">For more information about Azure programs that support standards compliance, see hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Trust Center</a>.</span></span>

* <span data-ttu-id="19a84-111">[Základy auditování databáze]</span><span class="sxs-lookup"><span data-stu-id="19a84-111">[Database Auditing basics]</span></span>
* <span data-ttu-id="19a84-112">[Nastavení auditování pro vaši databázi]</span><span class="sxs-lookup"><span data-stu-id="19a84-112">[Set up auditing for your database]</span></span>
* <span data-ttu-id="19a84-113">[Analýza protokolů auditu a sestavy]</span><span class="sxs-lookup"><span data-stu-id="19a84-113">[Analyze audit logs and reports]</span></span>

## <span data-ttu-id="19a84-114"><a id="subheading-1"></a>Základy Azure SQL Data Warehouse databáze auditování</span><span class="sxs-lookup"><span data-stu-id="19a84-114"><a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing basics</span></span>
<span data-ttu-id="19a84-115">Auditování databáze SQL Data Warehouse umožňuje:</span><span class="sxs-lookup"><span data-stu-id="19a84-115">SQL Data Warehouse database auditing allows you to:</span></span>

* <span data-ttu-id="19a84-116">**Zachovat** monitorovat vybrané události.</span><span class="sxs-lookup"><span data-stu-id="19a84-116">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="19a84-117">Můžete definovat kategorie toobe databáze akce auditovat.</span><span class="sxs-lookup"><span data-stu-id="19a84-117">You can define categories of database actions  toobe audited.</span></span>
* <span data-ttu-id="19a84-118">**Sestava** v databázové aktivitě.</span><span class="sxs-lookup"><span data-stu-id="19a84-118">**Report** on database activity.</span></span> <span data-ttu-id="19a84-119">Můžete vytvořit předem nakonfigurované sestavy a řídicí panel tooget, rychle začít s aktivity a události vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="19a84-119">You can use preconfigured reports and a dashboard tooget started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="19a84-120">**Analýza** sestavy.</span><span class="sxs-lookup"><span data-stu-id="19a84-120">**Analyze** reports.</span></span> <span data-ttu-id="19a84-121">Můžete najít podezřelé události, neobvyklé aktivity a trendy.</span><span class="sxs-lookup"><span data-stu-id="19a84-121">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="19a84-122">Můžete nakonfigurovat auditování pro hello následující kategorie události:</span><span class="sxs-lookup"><span data-stu-id="19a84-122">You can configure auditing for hello following event categories:</span></span>

<span data-ttu-id="19a84-123">**Nešifrovaná SQL** a **parametry SQL** pro které hello protokolů auditu shromážděných jsou klasifikovány jako</span><span class="sxs-lookup"><span data-stu-id="19a84-123">**Plain SQL** and **Parameterized SQL** for which hello collected audit logs are classified as</span></span>  

* <span data-ttu-id="19a84-124">**Toodata přístup**</span><span class="sxs-lookup"><span data-stu-id="19a84-124">**Access toodata**</span></span>
* <span data-ttu-id="19a84-125">**Změny schématu (DDL)**</span><span class="sxs-lookup"><span data-stu-id="19a84-125">**Schema changes (DDL)**</span></span>
* <span data-ttu-id="19a84-126">**Změny dat (DML)**</span><span class="sxs-lookup"><span data-stu-id="19a84-126">**Data changes (DML)**</span></span>
* <span data-ttu-id="19a84-127">**Účty, rolí a oprávnění (DCL)**</span><span class="sxs-lookup"><span data-stu-id="19a84-127">**Accounts, roles, and permissions (DCL)**</span></span>
* <span data-ttu-id="19a84-128">**Uložené procedury**, **přihlášení** a, **transakce správu**.</span><span class="sxs-lookup"><span data-stu-id="19a84-128">**Stored Procedure**, **Login** and, **Transaction Management**.</span></span>

<span data-ttu-id="19a84-129">Pro každou kategorii událostí auditování z **úspěch** a **selhání** operace jsou nakonfigurovány odděleně.</span><span class="sxs-lookup"><span data-stu-id="19a84-129">For each Event Category, Auditing of **Success** and **Failure** operations are configured separately.</span></span>

<span data-ttu-id="19a84-130">Další informace o hello aktivit a událostí auditovat najdete v tématu hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">odkazu formátu protokolu auditu (soubor dokumentace ke stažení)</a>.</span><span class="sxs-lookup"><span data-stu-id="19a84-130">For more information about hello activities and events audited, see hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Audit Log Format Reference (doc file download)</a>.</span></span>

<span data-ttu-id="19a84-131">Protokoly auditu jsou uložené v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="19a84-131">Audit logs are stored in your Azure storage account.</span></span> <span data-ttu-id="19a84-132">Můžete definovat po dobu uchování protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="19a84-132">You can define an audit log retention period.</span></span>

<span data-ttu-id="19a84-133">Zásady pro auditování lze definovat pro konkrétní databázi nebo jako výchozí zásady serveru.</span><span class="sxs-lookup"><span data-stu-id="19a84-133">An auditing policy can be defined for a specific database or as a default server policy.</span></span> <span data-ttu-id="19a84-134">Výchozí zásady auditování serveru se vztahuje tooall databází na serveru, které nemají konkrétní přepsání databáze auditování definované zásady.</span><span class="sxs-lookup"><span data-stu-id="19a84-134">A default server auditing policy applies tooall databases on a server, which do not have a specific overriding database auditing policy defined.</span></span>

<span data-ttu-id="19a84-135">Před nastavením auditování kontrolu, pokud používáte auditu ["Starších klientů."](sql-data-warehouse-auditing-downlevel-clients.md)</span><span class="sxs-lookup"><span data-stu-id="19a84-135">Before setting up audit auditing check if you are using a ["Downlevel Client."](sql-data-warehouse-auditing-downlevel-clients.md)</span></span>

## <span data-ttu-id="19a84-136"><a id="subheading-2"></a>Nastavení auditování pro vaši databázi</span><span class="sxs-lookup"><span data-stu-id="19a84-136"><a id="subheading-2"></a>Set up auditing for your database</span></span>
1. <span data-ttu-id="19a84-137">Spusťte hello <a href="https://portal.azure.com" target="_blank">portál Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="19a84-137">Launch hello <a href="https://portal.azure.com" target="_blank">Azure portal</a>.</span></span>
2. <span data-ttu-id="19a84-138">Přejděte toohello **nastavení** okno hello chcete tooaudit SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="19a84-138">Go toohello **Settings** blade of hello SQL Data Warehouse you want tooaudit.</span></span> <span data-ttu-id="19a84-139">V hello **nastavení** vyberte **auditování a detekce hrozeb**.</span><span class="sxs-lookup"><span data-stu-id="19a84-139">In hello **Settings** blade, select **Auditing & Threat detection**.</span></span>
   
    ![][1]
3. <span data-ttu-id="19a84-140">Dál povolte auditování kliknutím hello **ON** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="19a84-140">Next, enable auditing by clicking hello **ON** button.</span></span>
   
    ![][3]
4. <span data-ttu-id="19a84-141">V okně Konfigurace auditování hello, vyberte **podrobnosti úložiště** tooopen hello úložiště protokolů auditu okno.</span><span class="sxs-lookup"><span data-stu-id="19a84-141">In hello auditing configuration blade, select **STORAGE DETAILS** tooopen hello Audit Logs Storage blade.</span></span> <span data-ttu-id="19a84-142">Vyberte hello účtu úložiště Azure, kam bude uložena protokoly a hello dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="19a84-142">Select hello Azure storage account where logs will be saved and, hello retention period.</span></span> 
>[!TIP]
><span data-ttu-id="19a84-143">Hello použít stejný účet úložiště pro všechny tooget hello auditování databází naplno hello předem nakonfigurované sestavy šablony.</span><span class="sxs-lookup"><span data-stu-id="19a84-143">Use hello same storage account for all audited databases tooget hello most out of hello pre-configured reports templates.</span></span>
   
    ![][4]
5. <span data-ttu-id="19a84-144">Klikněte na tlačítko hello **OK** tlačítko toosave hello úložiště podrobnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="19a84-144">Click hello **OK** button toosave hello storage details configuration.</span></span>
6. <span data-ttu-id="19a84-145">V části **protokolování podle událostí**, klikněte na tlačítko **úspěch** a **selhání** toolog všechny události, nebo zvolte kategorií jednotlivých událostí.</span><span class="sxs-lookup"><span data-stu-id="19a84-145">Under **LOGGING BY EVENT**, click **SUCCESS** and **FAILURE** toolog all events, or choose individual event categories.</span></span>
7. <span data-ttu-id="19a84-146">Pokud konfigurujete auditování pro databázi, musíte tooalter hello připojovací řetězec vaší tooensure klienta, které auditování dat pořízen správně.</span><span class="sxs-lookup"><span data-stu-id="19a84-146">If you are configuring Auditing for a database, you may need tooalter hello connection string of your client tooensure data auditing is correctly captured.</span></span> <span data-ttu-id="19a84-147">Zkontrolujte hello [upravit FDQN serveru v připojovacím řetězci hello](sql-data-warehouse-auditing-downlevel-clients.md) tématu pro připojení klientů nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="19a84-147">Check hello [Modify Server FDQN in hello connection string](sql-data-warehouse-auditing-downlevel-clients.md) topic for downlevel client connections.</span></span>
8. <span data-ttu-id="19a84-148">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="19a84-148">Click **OK**.</span></span>

## <span data-ttu-id="19a84-149"><a id="subheading-3"></a>Analýza protokolů auditu a sestavy</span><span class="sxs-lookup"><span data-stu-id="19a84-149"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="19a84-150">Protokoly auditu je agregován v kolekci úložiště tabulek s **SQLDBAuditLogs** předponu hello účtu úložiště Azure, které jste zvolili během instalace.</span><span class="sxs-lookup"><span data-stu-id="19a84-150">Audit logs are aggregated in a collection of Store Tables with a **SQLDBAuditLogs** prefix in hello Azure storage account you chose during setup.</span></span> <span data-ttu-id="19a84-151">Můžete zobrazit soubory protokolů pomocí některého nástroje, jako třeba <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.</span><span class="sxs-lookup"><span data-stu-id="19a84-151">You can view log files using a tool such as <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.</span></span>

<span data-ttu-id="19a84-152">Šablona sestavy předkonfigurované řídicího panelu je k dispozici jako <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">ke stažení tabulku aplikace Excel</a> toohelp rychle analyzovat data protokolu.</span><span class="sxs-lookup"><span data-stu-id="19a84-152">A preconfigured dashboard report template is available as a <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadable Excel spreadsheet</a> toohelp you quickly analyze log data.</span></span> <span data-ttu-id="19a84-153">Šablona hello toouse na vaše protokoly auditu, budete potřebovat Excel 2013 nebo novější a Power Query, kterou si můžete stáhnout <a href="http://www.microsoft.com/download/details.aspx?id=39379">zde</a>.</span><span class="sxs-lookup"><span data-stu-id="19a84-153">toouse hello template on your audit logs, you need Excel 2013 or later and Power Query, which you can download <a href="http://www.microsoft.com/download/details.aspx?id=39379">here</a>.</span></span>

<span data-ttu-id="19a84-154">Šablona Hello má fiktivních ukázkových dat v něm a můžete nastavit Power Query tooimport auditní protokol přímo z vašeho účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="19a84-154">hello template has fictional sample data in it, and you can set up Power Query tooimport your audit log directly from your Azure storage account.</span></span>

## <span data-ttu-id="19a84-155"><a id="subheading-4"></a>Opětovné generování klíče úložiště</span><span class="sxs-lookup"><span data-stu-id="19a84-155"><a id="subheading-4"></a>Storage key regeneration</span></span>
<span data-ttu-id="19a84-156">V produkčním prostředí budete pravděpodobně toorefresh úložiště klíčů pravidelně.</span><span class="sxs-lookup"><span data-stu-id="19a84-156">In production, you are likely toorefresh your storage keys periodically.</span></span> <span data-ttu-id="19a84-157">Při aktualizaci klíče, je nutné toosave hello zásad.</span><span class="sxs-lookup"><span data-stu-id="19a84-157">When refreshing your keys, you need toosave hello policy.</span></span> <span data-ttu-id="19a84-158">proces Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="19a84-158">hello process is as follows:</span></span>

1. <span data-ttu-id="19a84-159">Přepněte na hello auditování okno konfigurace (popsaný výše v instalačním programu hello auditování část) hello **přístupový klíč k úložišti** z *primární* příliš*sekundární* a  **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="19a84-159">In hello auditing configuration blade (described above in hello setup auditing section) switch hello **Storage Access Key** from *Primary* too*Secondary* and **SAVE**.</span></span>

   ![][4]
2. <span data-ttu-id="19a84-160">Okno Konfigurace úložiště přejděte toohello a **znovu vygenerovat** hello *primární přístupový klíč*.</span><span class="sxs-lookup"><span data-stu-id="19a84-160">Go toohello storage configuration blade and **regenerate** hello *Primary Access Key*.</span></span>
3. <span data-ttu-id="19a84-161">Přejděte zpět toohello auditování okně konfigurace přepínače hello **přístupový klíč k úložišti** z *sekundární* příliš*primární* a stiskněte klávesu **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="19a84-161">Go back toohello auditing configuration blade, switch hello **Storage Access Key** from *Secondary* too*Primary* and press **SAVE**.</span></span>
4. <span data-ttu-id="19a84-162">Přejděte zpět toohello úložiště uživatelského rozhraní a **znovu vygenerovat** hello *sekundární přístupový klíč* (jako příprava pro hello další klíče obnovit cyklu.</span><span class="sxs-lookup"><span data-stu-id="19a84-162">Go back toohello storage UI and **regenerate** hello *Secondary Access Key* (as preparation for hello next keys refresh cycle.</span></span>

## <span data-ttu-id="19a84-163"><a id="subheading-5"></a>Automatizace (prostředí PowerShell nebo REST API)</span><span class="sxs-lookup"><span data-stu-id="19a84-163"><a id="subheading-5"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="19a84-164">Můžete taky nakonfigurovat auditování v Azure SQL Data Warehouse pomocí hello následující automatizace nástroje:</span><span class="sxs-lookup"><span data-stu-id="19a84-164">You can also configure auditing in Azure SQL Data Warehouse by using hello following automation tools:</span></span>

* <span data-ttu-id="19a84-165">**Rutiny prostředí PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="19a84-165">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="19a84-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="19a84-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="19a84-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="19a84-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="19a84-168">[Odebrat AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="19a84-168">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="19a84-169">[Odebrat AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="19a84-169">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="19a84-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="19a84-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="19a84-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="19a84-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="19a84-172">[Použití AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="19a84-172">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

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