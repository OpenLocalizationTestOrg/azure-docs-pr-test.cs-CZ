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
# <a name="auditing-in-azure-sql-data-warehouse"></a>Auditování v Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Auditování](sql-data-warehouse-auditing-overview.md)
> * [Detekce hrozeb](sql-data-warehouse-security-threat-detection.md)
> 
> 

Auditování SQL Data Warehouse vám umožní toorecord události z protokolu auditu tooan databáze ve vašem účtu úložiště Azure. Auditování vám může pomoct zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení. Auditování SQL Data Warehouse taky integruje s Microsoft Power BI pro procházení analýz a generování sestav.

Nástroje auditování povolit a zajištění dodržování standardů toocompliance ale nezaručují dodržování předpisů. Další informace o Azure programy dodržování standardů tuto podporu, najdete v části hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Centrum zabezpečení Azure</a>.

* [Základy auditování databáze]
* [Nastavení auditování pro vaši databázi]
* [Analýza protokolů auditu a sestavy]

## <a id="subheading-1"></a>Základy Azure SQL Data Warehouse databáze auditování
Auditování databáze SQL Data Warehouse umožňuje:

* **Zachovat** monitorovat vybrané události. Můžete definovat kategorie toobe databáze akce auditovat.
* **Sestava** v databázové aktivitě. Můžete vytvořit předem nakonfigurované sestavy a řídicí panel tooget, rychle začít s aktivity a události vytváření sestav.
* **Analýza** sestavy. Můžete najít podezřelé události, neobvyklé aktivity a trendy.

Můžete nakonfigurovat auditování pro hello následující kategorie události:

**Nešifrovaná SQL** a **parametry SQL** pro které hello protokolů auditu shromážděných jsou klasifikovány jako  

* **Toodata přístup**
* **Změny schématu (DDL)**
* **Změny dat (DML)**
* **Účty, rolí a oprávnění (DCL)**
* **Uložené procedury**, **přihlášení** a, **transakce správu**.

Pro každou kategorii událostí auditování z **úspěch** a **selhání** operace jsou nakonfigurovány odděleně.

Další informace o hello aktivit a událostí auditovat najdete v tématu hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">odkazu formátu protokolu auditu (soubor dokumentace ke stažení)</a>.

Protokoly auditu jsou uložené v účtu úložiště Azure. Můžete definovat po dobu uchování protokolu auditu.

Zásady pro auditování lze definovat pro konkrétní databázi nebo jako výchozí zásady serveru. Výchozí zásady auditování serveru se vztahuje tooall databází na serveru, které nemají konkrétní přepsání databáze auditování definované zásady.

Před nastavením auditování kontrolu, pokud používáte auditu ["Starších klientů."](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Nastavení auditování pro vaši databázi
1. Spusťte hello <a href="https://portal.azure.com" target="_blank">portál Azure</a>.
2. Přejděte toohello **nastavení** okno hello chcete tooaudit SQL Data Warehouse. V hello **nastavení** vyberte **auditování a detekce hrozeb**.
   
    ![][1]
3. Dál povolte auditování kliknutím hello **ON** tlačítko.
   
    ![][3]
4. V okně Konfigurace auditování hello, vyberte **podrobnosti úložiště** tooopen hello úložiště protokolů auditu okno. Vyberte hello účtu úložiště Azure, kam bude uložena protokoly a hello dobu uchování. 
>[!TIP]
>Hello použít stejný účet úložiště pro všechny tooget hello auditování databází naplno hello předem nakonfigurované sestavy šablony.
   
    ![][4]
5. Klikněte na tlačítko hello **OK** tlačítko toosave hello úložiště podrobnosti konfigurace.
6. V části **protokolování podle událostí**, klikněte na tlačítko **úspěch** a **selhání** toolog všechny události, nebo zvolte kategorií jednotlivých událostí.
7. Pokud konfigurujete auditování pro databázi, musíte tooalter hello připojovací řetězec vaší tooensure klienta, které auditování dat pořízen správně. Zkontrolujte hello [upravit FDQN serveru v připojovacím řetězci hello](sql-data-warehouse-auditing-downlevel-clients.md) tématu pro připojení klientů nižší úrovně.
8. Klikněte na **OK**.

## <a id="subheading-3"></a>Analýza protokolů auditu a sestavy
Protokoly auditu je agregován v kolekci úložiště tabulek s **SQLDBAuditLogs** předponu hello účtu úložiště Azure, které jste zvolili během instalace. Můžete zobrazit soubory protokolů pomocí některého nástroje, jako třeba <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.

Šablona sestavy předkonfigurované řídicího panelu je k dispozici jako <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">ke stažení tabulku aplikace Excel</a> toohelp rychle analyzovat data protokolu. Šablona hello toouse na vaše protokoly auditu, budete potřebovat Excel 2013 nebo novější a Power Query, kterou si můžete stáhnout <a href="http://www.microsoft.com/download/details.aspx?id=39379">zde</a>.

Šablona Hello má fiktivních ukázkových dat v něm a můžete nastavit Power Query tooimport auditní protokol přímo z vašeho účtu úložiště Azure.

## <a id="subheading-4"></a>Opětovné generování klíče úložiště
V produkčním prostředí budete pravděpodobně toorefresh úložiště klíčů pravidelně. Při aktualizaci klíče, je nutné toosave hello zásad. proces Hello vypadá takto:

1. Přepněte na hello auditování okno konfigurace (popsaný výše v instalačním programu hello auditování část) hello **přístupový klíč k úložišti** z *primární* příliš*sekundární* a  **Uložit**.

   ![][4]
2. Okno Konfigurace úložiště přejděte toohello a **znovu vygenerovat** hello *primární přístupový klíč*.
3. Přejděte zpět toohello auditování okně konfigurace přepínače hello **přístupový klíč k úložišti** z *sekundární* příliš*primární* a stiskněte klávesu **Uložit**.
4. Přejděte zpět toohello úložiště uživatelského rozhraní a **znovu vygenerovat** hello *sekundární přístupový klíč* (jako příprava pro hello další klíče obnovit cyklu.

## <a id="subheading-5"></a>Automatizace (prostředí PowerShell nebo REST API)
Můžete taky nakonfigurovat auditování v Azure SQL Data Warehouse pomocí hello následující automatizace nástroje:

* **Rutiny prostředí PowerShell**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Odebrat AzureRMSqlDatabaseAuditing][103]
   * [Odebrat AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Použití AzureRMSqlServerAuditingPolicy][107]

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