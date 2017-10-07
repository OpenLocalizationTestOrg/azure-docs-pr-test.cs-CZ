---
title: "úlohy elastické databáze aaaInstalling | Microsoft Docs"
description: "Provede procesem instalace funkce elastické úlohy hello."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: cbe0aa2b-17e3-4b6f-a16f-6ebc1f5a66af
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 0349f66a4428f81d00d43681d7f2177f273ec032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="installing-elastic-database-jobs-overview"></a>Instalace úlohy elastické databáze – přehled
[**Elastické databáze úlohy** ](sql-database-elastic-jobs-overview.md) lze nainstalovat pomocí prostředí PowerShell nebo prostřednictvím hello Azure Classic Portal.You můžete získat přístup k toocreate a spravovat úlohy pomocí hello rozhraní API prostředí PowerShell, pouze v případě, že budete instalovat balíček hello prostředí PowerShell. Kromě toho hello rozhraní API prostředí PowerShell poskytuje výrazně víc funkcí než hello portál v tuto chvíli.

Pokud jste již nainstalovali **úlohy elastické databáze** prostřednictvím hello portálu ze stávajícího **elastický fond**, hello nejnovější prostředí Powershell preview zahrnuje skripty tooupgrade vaši existující instalaci. Vysoce je doporučeno tooupgrade vaší instalace toohello nejnovější **úlohy elastické databáze** hello součásti v pořadí tootake využívat nové funkce, které jsou zveřejňovány prostřednictvím rozhraní API prostředí PowerShell.

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* Azure Powershell Nainstalujte nejnovější verzi hello pomocí hello [instalačního programu webové platformy](http://go.microsoft.com/fwlink/p/?linkid=320376). Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
* [Nástroj příkazového řádku NuGet](https://nuget.org/nuget.exe) je použité tooinstall hello elastické databáze úlohy balíček. Další informace najdete v tématu http://docs.nuget.org/docs/start-here/installing-nuget.

## <a name="download-and-import-hello-elastic-database-jobs-powershell-package"></a>Stahování a import balíčku prostředí PowerShell úlohy elastické databáze hello
1. Spusťte příkazové okno prostředí PowerShell Microsoft Azure a přejděte toohello adresáře, kam jste stáhli Nástroj příkazového řádku NuGet (nuget.exe).
2. Stahování a import **úlohy elastické databáze** balíček do aktuální adresář hello s hello následující příkaz:
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    Hello **úlohy elastické databáze** soubory jsou umístěny do místního adresáře hello ve složce s názvem **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** kde *x.x.xxxx.x* Zobrazuje číslo verze hello. rutiny prostředí PowerShell Hello (včetně klienta knihoven DLL) jsou umístěné v hello **tools\ElasticDatabaseJobs** podadresář a hello tooinstall skripty prostředí PowerShell, upgradu a odinstalaci také nacházet v hello  **Nástroje pro** podadresář.
3. Přejděte toohello nástroje podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x hello zadáním disk cd nástroje, například:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. Spusťte hello.\InstallElasticDatabaseJobsCmdlets.ps1 skriptu toocopy hello ElasticDatabaseJobs adresáře do $home\Documents\WindowsPowerShell\Modules. To také automaticky importuje hello modulu pro použití, například:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-hello-elastic-database-jobs-components-using-powershell"></a>Instalace součásti úlohy hello elastické databáze pomocí prostředí PowerShell
1. Spusťte příkazové okno Microsoft Azure PowerShell a přejděte toohello \tools podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x hello: Zadejte \tools disku cd
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. Spustit skript prostředí PowerShell.\InstallElasticDatabaseJobs.ps1 hello a poskytnout hodnoty pro její požadované proměnné. Tento skript vytvoří hello komponent popsaných v [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing) společně s konfigurací hello Azure Cloud Service tooappropriately použít hello závislé součásti.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

Když spustíte tento příkaz, okno otevře požadující **uživatelské jméno** a **heslo**. Toto není vaše přihlašovací údaje Azure, zadejte hello uživatelské jméno a heslo, které budou přihlašovací údaje správce hello toocreate chcete pro nový server hello.

pro požadovaná nastavení můžete upravit parametry Hello k dispozici na toto ukázkové volání. Následující Hello poskytuje další informace o chování hello každý parametr:

<table style="width:100%">
  <tr>
    <th>Parametr</th>
    <th>Popis</th>
  </tr>

<tr>
    <td>Název skupiny prostředků</td>
    <td>Poskytuje název skupiny prostředků Azure hello vytvořit toocontain hello nově vytvořený Azure součásti. Tento parametr výchozí příliš "__ElasticDatabaseJob". Není doporučeno toochange tuto hodnotu.</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Poskytuje toobe umístění Azure hello používá pro nově vytvořený Azure součásti hello. Tento parametr výchozí toohello střed USA umístění.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Poskytuje hello počet tooinstall pracovníky služby. Tento parametr výchozí too1. Vyšší počet pracovních procesů může být použité tooscale out hello služby a tooprovide vysokou dostupnost. Doporučujeme toouse "2" pro nasazení, která vyžadují vysokou dostupnost služby hello.</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>Poskytuje hello velikost virtuálního počítače pro použití v rámci hello cloudové služby. Tento parametr výchozí tooA0. Hodnoty parametrů A0 nebo A1 nebo A2 nebo A3, jsou přijaty které způsobit hello pracovní toouse role pro velikost: mimořádně malý nebo malá nebo střední nebo velké. Další informace o velikosti role pracovního procesu, viz Fo [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Poskytuje hello cíle na úrovni služby pro Standard edition. Tento parametr výchozí tooS0. Hodnoty parametru S0/S1 nebo S2/S3 přijímají, které způsobují hello Azure SQL Database toouse hello příslušných SLO. Další informace o slo databáze SQL najdete v tématu [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Poskytuje hello uživatelské jméno správce pro hello nově vytvořený serveru Azure SQL Database. Není-li zadána, otevře se okno přihlašovací údaje prostředí PowerShell tooprompt hello přihlašovacích údajů.</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>Poskytuje hello heslo správce pro hello nově vytvořený serveru Azure SQL Database. Když není zadaná, otevře se okno přihlašovací údaje prostředí PowerShell tooprompt hello přihlašovacích údajů.</td>
</tr>
</table>

Pro systémy, které cílí má velký počet úloh spuštěných současně pro velký počet databází, se doporučuje toospecify parametry, jako: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>Aktualizace stávající elastické databáze úlohy součásti instalace pomocí prostředí PowerShell
**Elastické databáze úlohy** můžete aktualizovat v rámci existující instalace pro škálování a vysokou dostupností. Tento proces umožňuje pro budoucí upgrady kódu služby bez nutnosti toodrop a znovu vytvořte hello řízení databáze. Tento proces lze také v rámci hello stejné verze toomodify hello služby počet virtuálních počítačů velikost nebo hello serveru worker.

velikost virtuálního počítače tooupdate hello instalace, spusťte hello následující skript s parametry aktualizovat toohello hodnoty podle svého výběru.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Parametr</th>
  <th>Popis</th>
</tr>

  <tr>
    <td>Název skupiny prostředků</td>
    <td>Určuje název skupiny prostředků Azure hello používá při hello elastické databáze úlohy součásti byly původně nainstaloval. Tento parametr výchozí příliš "__ElasticDatabaseJob". Vzhledem k tomu, že není doporučeno toochange tuto hodnotu toospecify by neměl mít tento parametr.</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>Poskytuje hello počet tooinstall pracovníky služby.  Tento parametr výchozí too1.  Vyšší počet pracovních procesů může být použité tooscale out hello služby a tooprovide vysokou dostupnost.  Doporučujeme toouse "2" pro nasazení, která vyžadují vysokou dostupnost služby hello.</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>Poskytuje hello velikost virtuálního počítače pro použití v rámci hello cloudové služby. Tento parametr výchozí tooA0. Hodnoty parametrů A0 nebo A1 nebo A2 nebo A3, jsou přijaty které způsobit hello pracovní toouse role pro velikost: mimořádně malý nebo malá nebo střední nebo velké. Další informace o velikosti role pracovního procesu, viz Fo [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</table>

## <a name="install-hello-elastic-database-jobs-components-using-hello-portal"></a>Nainstalujte komponenty úlohy elastické databáze hello pomocí hello portálu
Jakmile máte [vytvoření fondu elastické databáze](sql-database-elastic-pool-manage-portal.md), můžete nainstalovat **úlohy elastické databáze** součásti tooenable provádění úloh správy na každou databázi v hello elastického fondu. Na rozdíl od při použití hello **úlohy elastické databáze** rozhraní API prostředí PowerShell, rozhraní portálu hello je momentálně omezené tooonly provádění na existující fond.

**Odhadovaný čas toocomplete:** 10 minut.

1. V zobrazení řídicího panelu hello hello elastického fondu prostřednictvím hello [portálu Azure](https://portal.azure.com/#) , klikněte na tlačítko **vytvořit úlohu**.
2. Pokud vytváříte úlohu pro hello poprvé, musíte nainstalovat **úlohy elastické databáze** kliknutím **podmínky**.
3. Přijměte podmínky hello kliknutím hello zaškrtávací políčko.
4. V zobrazení hello "instalace služby", klikněte na tlačítko **úlohy pověření**.
   
    ![Instalace služby hello][1]
5. Zadejte uživatelské jméno a heslo pro správce databáze Jako součást instalace hello vytvoří se nový server Azure SQL Database. Novou databázi, označuje jako hello řízení databáze, v rámci tohoto nového serveru se vytvoří a používá toocontain hello metadata pro úlohy elastické databáze. Hello uživatelské jméno a heslo vytvořená zde se používají pro účel hello protokolování v databázi toohello ovládacího prvku. Samostatné přihlašovací údaje se používá pro spuštění skriptu s databázemi hello v rámci fondu hello.
   
    ![Vytvořte uživatelské jméno a heslo][2]
6. Klikněte na tlačítko OK hello. Hello součásti jsou vytvořeny pro vás za pár minut v nové [skupiny prostředků](../azure-resource-manager/resource-group-overview.md). Hello novou skupinu prostředků je připnutá toohello spustit Tabule, jak je uvedeno níže. Úlohy po vytvoření, elastické databáze (Cloudová služba, databáze SQL, Service Bus a úložiště) jsou vytvořeny ve skupině hello.
   
    ![Skupina prostředků v Tabule start][3]
7. Pokud pokus toocreate nebo spravovat úlohy, při instalaci úlohy elastické databáze, pokud poskytuje **pověření** zobrazí se následující zpráva hello.
   
    ![Stále probíhá nasazení][4]

Pokud odinstalaci je potřeba, odstraňte skupinu prostředků hello. V tématu [jak toouninstall hello elastické databáze úlohy součásti](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Další kroky
Zkontrolujte přihlašovací údaje s hello příslušná práva pro spuštění skriptu se vytvoří na každou databázi ve skupině hello, další informace najdete v tématu [zabezpečení databáze SQL](sql-database-manage-logins.md).
V tématu [vytváření a Správa úloh elastické databáze](sql-database-elastic-jobs-create-and-manage.md) tooget spuštěna.

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
