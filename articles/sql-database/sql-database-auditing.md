---
title: "aaaGet začít s auditování databáze Azure SQL | Microsoft Docs"
description: "Začínáme s auditování databáze Azure SQL"
services: sql-database
documentationcenter: 
author: giladm
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: giladm
ms.openlocfilehash: 5494c602d702ac41992520f900c393a98cc7c989
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-auditing"></a>Začínáme s auditem databáze SQL
Auditování databáze SQL Azure sleduje události databáze a zapisuje je tooan protokolu auditování v účtu úložiště Azure. Auditování také:

* Pomáhá zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které by mohly být známkou problémů obchodního charakteru nebo by mohly vzbuzovat podezření narušení zabezpečení.

* Umožňuje a zjednodušuje dodržování standardů toocompliance, i když není zárukou toho, dodržování předpisů. Další informace o Azure programy dodržování standardů tuto podporu, najdete v části hello [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/compliance/).


## <a id="subheading-1"></a>Databáze SQL Azure auditování – přehled
Můžete použít auditování databáze SQL pro:


* **Zachovat** monitorovat vybrané události. Můžete definovat kategorie toobe databáze akce auditovat.
* **Sestava** v databázové aktivitě. Můžete vytvořit předem nakonfigurované sestavy a řídicí panel tooget, rychle začít s aktivity a události vytváření sestav.
* **Analýza** sestavy. Můžete najít podezřelé události, neobvyklé aktivity a trendy.

Můžete nakonfigurovat auditování pro různé typy událostí kategorie, jak je popsáno v hello [nastavit auditování pro databázi](#subheading-2) části.

Protokoly auditu se zapisují tooAzure úložiště objektů Blob na vaše předplatné Azure.


## <a id="subheading-8"></a>Definovat úroveň serveru oproti auditování zásady na úrovni databáze

Zásady auditu je možné definovat pro konkrétní databázi nebo jako výchozí zásady serveru:

* Zásady serveru platí tooall stávající a nově vytvořené databáze na serveru hello.

* Pokud *auditování objektů blob serveru zapnutá*, ho *vždy použije toohello databáze* (tedy hello databáze auditování), bez ohledu na to databáze hello nastavení auditování.

* Povolení auditování pro databázi hello v přidání tooenabling ho na hello server, objektů blob budou *není* přepsat nebo změnit libovolné nastavení hello auditování objektů blob server hello. Obě audity, budou existovat vedle sebe. Jinými slovy databáze hello dvakrát paralelně auditování (jednou zásadami hello serveru a jednou zásadami hello databáze).

   > [!NOTE]
   > Povolení auditování objektů blob serveru i auditování databáze blob společně, pokud byste neměli:
    > * Chcete toouse jiné *účet úložiště* nebo *dobu uchování* pro konkrétní databázi.
    > * Chcete typů událostí tooaudit nebo kategorie pro konkrétní databázi, která se liší od typů událostí nebo kategorie, auditovaných pro hello zbytek hello databáze na serveru hello. Například můžete mít vložení tabulky, které je třeba toobe Audituje jenom pro konkrétní databázi.
   > 
   > Jinak doporučujeme, abyste povolili auditování objektů blob jenom úrovni serveru a nechte hello databáze auditování na úrovni zakázáno pro všechny databáze.


## <a id="subheading-2"></a>Nastavení auditování pro vaši databázi
Hello následující část popisuje hello konfigurace auditování pomocí hello portálu Azure.

1. Přejděte toohello [portál Azure](https://portal.azure.com).
2. Přejděte toohello **nastavení** okno hello chcete tooaudit serveru SQL database nebo SQL server. V hello **nastavení** vyberte **auditování a detekce hrozeb**.

    <a id="auditing-screenshot"></a>![Navigačním podokně][1]
3. Pokud dáváte přednost tooset až zásady auditování serveru (která bude použita existující tooall a nově vytvořené databáze na tomto serveru), můžete vybrat hello **zobrazit nastavení serveru** odkaz v okně auditování databáze hello. Můžete pak zobrazit nebo upravit nastavení auditování serveru hello.

    ![Navigační podokno][2]
4. Pokud dáváte přednost tooenable auditování objektů blob na úrovni databáze hello (v přidání tooor místo auditování na úrovni serveru), pro **auditování**, vyberte **ON**a pro **auditování typ** , vyberte **Blob**.

    Pokud je auditování objektů blob serveru je povoleno, budou existovat audit databáze konfigurována hello node souběžně s auditování objektů blob server hello.  

    ![Navigační podokno][3]
5. tooopen hello **úložiště protokolů auditu** vyberte **podrobnosti úložiště**. Vyberte účet úložiště Azure hello, kde bude uložena protokoly a pak vyberte dobu uchování hello, po které hello se odstraní starých protokolů. Pak klikněte na **OK**. 
   >[!TIP] 
   >hello tooget hello naplno hello auditování šablon sestav, použijte stejný účet úložiště pro všechny auditování databáze. 

    <a id="storage-screenshot"></a>![Navigačním podokně][4]
6. Pokud chcete toocustomize hello auditovat události, můžete to provést pomocí prostředí PowerShell nebo hello REST API. Další podrobnosti najdete v tématu hello [automatizace (prostředí PowerShell nebo REST API)](#subheading-7) části.
7. Po dokončení konfigurace nastavení auditování, můžete zapnout hello novou funkci detekce hrozeb a konfigurovat výstrahy zabezpečení tooreceive e-mailů. Pokud používáte detekce hrozeb, obdržíte proaktivní výstrahy na nezvyklé databázové aktivity, které může znamenat potenciální bezpečnostní hrozby. Další podrobnosti najdete v tématu [Začínáme s detekce hrozeb](sql-database-threat-detection-get-started.md).
8. Klikněte na **Uložit**.





## <a id="subheading-3"></a>Analýza protokolů auditu a sestavy
Protokoly auditu je agregován v hello účtu úložiště Azure, které jste zvolili během instalace. Protokoly auditu můžete prozkoumat pomocí nástroje, jako například [Azure Storage Explorer](http://storageexplorer.com/).

Protokoly auditování objektů BLOB jsou uloženy jako kolekce souborů, objektů blob do kontejneru, s názvem **sqldbauditlogs**.

Další podrobnosti o hierarchii hello složce úložiště protokolů auditování objektů blob hello, zásady vytváření názvů objektů blob a formát protokolu najdete v tématu hello [Blob auditu protokolu referenční příručka pro formátování (soubor .docx ke stažení)](https://go.microsoft.com/fwlink/?linkid=829599).

Existuje několik metod, které můžete použít objekt blob tooview auditování protokoly:

* Použití hello [portál Azure](https://portal.azure.com).  Otevřete hello příslušné databáze. Na nejvyšší hello hello databáze **auditování a detekce hrozeb** okně klikněte na tlačítko **zobrazit protokoly auditu**.

    ![Navigační podokno][7]

    **Audit záznamy** otevře se okno, ze kterých budete moct tooview hello protokoly.

    - Kliknutím můžete zobrazit konkrétní kalendářní data **filtru** hello horní části hello **Audit záznamy** okno.
    - Můžete přepínat mezi záznamy auditu, které byly vytvořeny auditu pro zásady zásady nebo databázi serveru.

       ![Navigační podokno][8]

* Pomocí funkce systému hello **sys.fn_get_audit_file** data protokolu (T-SQL) tooreturn hello auditu v tabulkovém formátu. Další informace o použití této funkce najdete v tématu hello [sys.fn_get_audit_file dokumentaci](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).


* Použití **sloučení soubory auditu** v aplikaci SQL Server Management Studio (počínaje SSMS 17):  
    1. Hello SSMS nabídce vyberte **soubor** > **otevřete** > **sloučení soubory auditu**.

        ![Navigační podokno][9]
    2. Hello **přidat soubory auditu** otevře se dialogové okno. Vyberte jednu z hello **přidat** možnost zvolte, zda toomerge auditu soubory z místního disku nebo importovat z Azure Storage (vám bude požadované tooprovide podrobnosti úložiště Azure a klíč účtu).

    3. Po přidání všech souborů toomerge, klikněte na tlačítko **OK** operace sloučení toocomplete hello.

    4. Hello sloučené soubor se otevře v aplikaci SSMS, kde můžete zobrazit a analyzovat je i jako export ho tooan souboru XEL nebo CSV souboru nebo tooa tabulky.

* Použití hello [synchronizace aplikace](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) které jsme vytvořili. Jeho spuštění v Azure a využívá analýzy protokolů Operations Management Suite (OMS) veřejné rozhraní API toopush SQL protokoly auditu do OMS. synchronizace aplikace Hello doručí protokoly auditu SQL do OMS analýzy protokolů pro používání prostřednictvím řídicího panelu analýzy protokolů OMS hello. 

* Pomocí Power BI. Můžete zobrazit a analyzovat data protokolu auditování v Power BI. Další informace o [Power BI a přístup ke stažení šablony](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Stáhnout soubory protokolů z vašeho kontejneru objektů blob Azure Storage prostřednictvím hello portálu nebo pomocí nástroje, jako například [Azure Storage Explorer](http://storageexplorer.com/).
    * Po stažení souboru protokolu, který je místně, můžete dvakrát kliknete na soubor tooopen hello, zobrazit a analyzovat protokoly hello v aplikaci SSMS.
    * Můžete také stáhnout víc souborů současně prostřednictvím Azure Storage Explorer. Klikněte pravým tlačítkem na konkrétní podsložku (například do podsložky, která obsahuje všechny soubory protokolu pro konkrétní datum) a vyberte **uložit jako** toosave do místní složky.

* Další metody:
   * Po stažení několik souborů (nebo na podsložku, která zahrnuje soubory protokolu pro celý den, jak je popsáno v předchozí položce hello v tomto seznamu), můžete je sloučit místně popsané v pokynech soubory auditu sloučení SSMS hello popsané výše.

   * Auditování objektů blob k zobrazení protokolů prostřednictvím kódu programu:

     * Použití hello [rozšířené události čtečky](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) knihovna jazyka C#.
     * [Dotaz Rozšířené události soubory](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) pomocí prostředí PowerShell.




## <a id="subheading-5"></a>Provozní postupy
<!--hello description in this section refers toopreceding screen captures.-->

### <a id="subheading-6">Auditování geograficky replikované databáze</a>
Pokud používáte geograficky replikované databáze, je možné tooset až auditování na hello primární databázi, hello sekundární databáze nebo obojí, v závislosti na typu auditu hello.

Postupujte podle těchto pokynů (Nezapomeňte, že auditování objektů blob lze zapnout nebo vypnout pouze z primární databáze hello nastavení auditování):

* **Primární databáze**. Zapnout auditování objektů blob, hello server buď hello databáze, jak je popsáno v hello [nastavit auditování pro databázi](#subheading-2) části.
* **Sekundární databáze**. Zapnout auditování objektů blob v hello primární databázi, jak je popsáno v hello [nastavit auditování pro databázi](#subheading-2) části. 
   * Auditování objektů BLOB musí být povolená na hello *primární databázi, samotné*, ne server hello.
   * Povolíte auditování objektů blob na hello primární databázi, bude ho také přístupné na hello sekundární databáze.

     >[!IMPORTANT]
     >Ve výchozím nastavení úložiště hello hello sekundární databáze bude identické toothose hello primární databázi, způsobuje provoz mezi místní. To se můžete vyhnout povolením auditování objektů blob na sekundárním serveru hello a konfigurace místní úložiště v nastavení úložiště sekundární server hello. Tím se přepíše umístění úložiště hello hello sekundární databáze a výsledkem každou databázi ukládání jeho úložiště toolocal protokoly auditu.  
<br>

### <a id="subheading-6">Opětovné generování klíče úložiště</a>
V produkčním prostředí budete pravděpodobně toorefresh úložiště klíčů pravidelně. Při aktualizaci klíče, je nutné zásad auditu tooresave hello. proces Hello vypadá takto:

1. Otevřete hello **podrobnosti úložiště** okno. V hello **přístupový klíč k úložišti** vyberte **sekundární**a klikněte na tlačítko **OK**. Pak klikněte na tlačítko **Uložit** hello horní části hello okno Konfigurace auditování.

    ![Navigační podokno][5]
2. Přejděte toohello úložiště konfigurace okno a znovu vygenerovat primární přístupový klíč hello.

    ![Navigační podokno][6]
3. Přejděte zpět toohello auditování okno konfigurace, přepínač přístupový klíč k úložišti hello ze sekundární tooprimary a pak klikněte na tlačítko **OK**. Pak klikněte na tlačítko **Uložit** hello horní části hello okno Konfigurace auditování.
4. Vraťte se zpátky toohello úložiště konfigurace okno a znovu generovat hello sekundární přístupový klíč (v rámci přípravy cyklus aktualizace hello Další klíč).

## <a id="subheading-7"></a>Automatizace (prostředí PowerShell nebo REST API)
Můžete taky nakonfigurovat auditování ve službě Azure SQL Database pomocí hello následující automatizace nástroje:

* **Rutiny prostředí PowerShell**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Odebrat AzureRMSqlDatabaseAuditing][103]
   * [Odebrat AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Použití AzureRMSqlServerAuditingPolicy][107]

   Příklad skriptu najdete v tématu [konfigurace auditování a zjišťování hrozeb pomocí prostředí PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

* **REST API – auditování objektů Blob**:

   * [Vytvořit nebo aktualizovat zásady auditování Blob databáze](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [Vytvořit nebo aktualizovat Server Blob zásady auditování](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [Získání objektu Blob databáze zásady auditování](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [Získání objektu Blob serveru zásady auditování](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [Získání objektu Blob serveru auditování výsledek operace](https://msdn.microsoft.com/library/azure/mt771862.aspx)


<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)  

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy
