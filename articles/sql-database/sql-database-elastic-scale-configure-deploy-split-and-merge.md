---
title: "aaaDeploy rozdělení sloučení služby | Microsoft Docs"
description: "Rozdělování a slučování pomocí nástroje elastické databáze"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 9a993c0f-7052-46cd-aa59-073bea8d535a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6fef641cbc1e73ce34a851c53298a072dade8f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-split-merge-service"></a>Nasazení služby dělení a slučování
Hello rozdělení sloučení nástroj umožňuje přesun dat mezi horizontálně dělené databáze. V tématu [přesouvání dat mezi instancemi cloudu databáze](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-hello-split-merge-packages"></a>Stáhnout balíčky rozdělení sloučení hello
1. Stažení nejnovější verze NuGet hello z [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).
2. Otevřete příkazový řádek a přejděte toohello adresáře, kam jste stáhli nuget.exe. zahrnuje Hello stažení commmands prostředí PowerShell.
3. Stáhněte si nejnovější balíček rozdělení sloučení hello do aktuální adresář hello s hello následující příkaz:
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

Hello soubory jsou umístěny v adresáři s názvem **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** kde *x.x.xxx.x* zobrazuje číslo verze hello. Najde hello soubory služby rozdělení sloučení v hello **content\splitmerge\service** podadresář a hello skriptů prostředí PowerShell rozdělení sloučení (a knihoven DLL klienta) v hello **content\splitmerge\powershell** podadresář.

## <a name="prerequisites"></a>Požadavky
1. Vytvořte databázi Azure SQL DB, který se použije jako hello rozdělení sloučení stavu databáze. Přejděte toohello [portál Azure](https://portal.azure.com). Vytvořte novou **SQL Database**. Zadejte název databáze hello a vytvořte nový správce a heslo. Být, že toorecord hello jméno a heslo pro pozdější použití.
2. Zajistěte, aby váš server Azure SQL DB povolovalo tooit tooconnect služeb Azure. V portálu hello hello **nastavení brány Firewall**, ujistěte se, že hello **povolit přístup k službám tooAzure** nastavení je příliš**na**. Klikněte na "ikonu uložit" hello.
   
   ![Povolené služby][1]
3. Vytvoření účtu úložiště Azure, který se použije pro výstup diagnostiky. Přejděte toohello portálu Azure. Hello levém panelu klikněte na **nový**, klikněte na tlačítko **Data + úložiště**, pak **úložiště**.
4. Vytvoření cloudové služby Azure, která bude obsahovat služby rozdělení sloučení.  Přejděte toohello portálu Azure. Hello levém panelu klikněte na **nový**, pak **výpočetní**, **Cloudová služba**, a **vytvořit**. 

## <a name="configure-your-split-merge-service"></a>Konfigurace služby rozdělení sloučení
### <a name="split-merge-service-configuration"></a>Konfigurace služby rozdělení sloučení
1. Hello složky, kam jste stáhli hello rozdělení sloučení sestavení, vytvořit kopii hello **ServiceConfiguration.Template.cscfg** soubor, který se dodává spolu s **SplitMergeService.cspkg** a přejmenujte ji **Souboru ServiceConfiguration.cscfg**.
2. Otevřete **souboru ServiceConfiguration.cscfg** v textovém editoru, jako je například Visual Studio, která ověřuje vstupy například hello formát kryptografické otisky certifikátu.
3. Vytvořit novou databázi nebo vyberte existující databázi tooserve jako hello stav databáze pro operace sloučení rozdělení a načtení připojovacího řetězce hello této databáze. 
   
   > [!IMPORTANT]
   > V tomto okamžiku hello stav databáze musí používat kolaci Latin hello (SQL\_Latin1\_Obecné\_CP1\_CI\_AS). Další informace najdete v tématu [název kolace systému Windows (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).
   >

   S Azure SQL DB hello připojovací řetězec je obvykle ve formátu hello:
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. Zadejte tento připojovací řetězec v hello soubor .cscfg v obou hello **SplitMergeWeb** a **SplitMergeWorker** role části hello ElasticScaleMetadata nastavení.
5. Pro hello **SplitMergeWorker** role, zadejte platný připojovací řetězec tooAzure úložiště pro hello **WorkerRoleSynchronizationStorageAccountConnectionString** nastavení.

### <a name="configure-security"></a>Konfigurace zabezpečení
Podrobné pokyny tooconfigure hello zabezpečení hello služby, najdete v části toohello [konfigurace zabezpečení rozdělení sloučení](sql-database-elastic-scale-split-merge-security-configuration.md).

Pro účely hello jednoduchá testovací nasazení pro tento kurz provést nejzákladnější konfigurace, která bude kroky tooget hello služby fungovaly. Tyto kroky aktivují jenom hello jeden počítač nebo účet jejich spuštění toocommunicate službou hello.

### <a name="create-a-self-signed-certificate"></a>Vytvořit certifikát podepsaný svým držitelem
Vytvořte nový adresář a z tohoto adresáře execute hello následující pomocí příkazu [příkazový řádek vývojáře pro sadu Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) okno:

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

Zobrazí se výzva pro heslo tooprotect hello privátní klíč. Zadejte silné heslo a potvrďte ho. Potom budete vyzváni k toobe hello heslo použít jednou po který. Klikněte na tlačítko **Ano** v hello end tooimport ho toohello úložiště Důvěryhodné kořenové certifikační autority.

### <a name="create-a-pfx-file"></a>Vytvořte soubor PFX
Spustit následující příkaz z hello hello stejné okno, kde byl proveden makecert; použití hello stejné heslo, můžete použít toocreate hello certifikátu:

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-hello-client-certificate-into-hello-personal-store"></a>Importovat hello klientský certifikát do osobního úložiště hello
1. V Průzkumníku Windows, klikněte dvakrát na **MyCert.pfx**.
2. V hello **Průvodce importem certifikátu** vyberte **aktuální uživatel** a klikněte na tlačítko **Další**.
3. Potvrďte hello cesta k souboru a klikněte na **Další**.
4. Zadejte hello heslo, nechte **obsahovat všechny rozšířené vlastnosti** zaškrtnutí a klikněte na tlačítko **Další**.
5. Nechte **úložiště certifikátů automaticky vybere hello [...]**  zaškrtnutí a klikněte na tlačítko **Další**.
6. Klikněte na tlačítko **Dokončit** a **OK**.

### <a name="upload-hello-pfx-file-toohello-cloud-service"></a>Nahrát hello PFX souboru toohello cloudové služby
1. Přejděte toohello [portálu Azure](https://portal.azure.com).
2. Vyberte **cloudových služeb**.
3. Vyberte hello cloudovou službu, kterou jste vytvořili výše pro službu rozdělení či sloučení hello.
4. Klikněte na tlačítko **certifikáty** v horní nabídce hello.
5. Klikněte na tlačítko **nahrát** v dolním panelu hello.
6. Vyberte soubor PFX hello a zadejte hello stejné heslo, jak je uvedeno výše.
7. Po dokončení kopírování hello kryptografický otisk certifikátu z hello novou položku v seznamu hello.

### <a name="update-hello-service-configuration-file"></a>Aktualizovat konfigurační soubor služby hello
Vložte kryptografický otisk certifikátu hello výše zkopírují do atribut hello kryptografický otisk nebo hodnotu z těchto nastavení.
Pro roli pracovního procesu hello:
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Pro roli webové hello:

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Upozorňujeme, že pro nasazení v produkčním prostředí samostatné certifikáty se mají použít pro hello certifikační Autority, k šifrování, hello certifikátu serveru a klientské certifikáty. Podrobné pokyny v tomto najdete v tématu [konfigurace zabezpečení](sql-database-elastic-scale-split-merge-security-configuration.md).

## <a name="deploy-your-service"></a>Nasazení služby
1. Přejděte toohello [portál Azure](https://manage.windowsazure.com).
2. Klikněte na tlačítko hello **cloudové služby** na levé straně hello a vyberte hello Cloudová služba, kterou jste vytvořili dříve.
3. Klikněte na tlačítko **řídicí panel**.
4. Zvolte hello pracovní prostředí, a klikněte na **nahrát nový pracovní nasazení**.
   
   ![Pracovní][3]
5. V dialogovém okně hello zadejte označení nasazení. Pro 'Balíček' i 'konfiguraci, klikněte na tlačítko 'Z Local' a vyberte hello **SplitMergeService.cspkg** Souborová služba a který jste dříve nakonfigurovali souboru .cscfg.
6. Ujistěte se, toto zaškrtávací políčko hello s názvem bez přípony **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** je zaškrtnuté.
7. Stiskněte tlačítko hello značek v hello dolní správné toobegin hello nasazení. Jeho očekávat tootake toocomplete několik minut.

   ![Odeslat][4]

## <a name="troubleshoot-hello-deployment"></a>Řešení potíží s hello nasazení
Pokud vaši webovou roli selže toocome online, je pravděpodobně problém s konfigurací zabezpečení hello. Zkontrolujte, že hello, který je nakonfigurovaný protokol SSL, jak je popsáno výše.

Pokud své role pracovního procesu selže toocome online, ale vaši webovou roli úspěšné, je pravděpodobně problém s připojením databáze toohello stav, který jste vytvořili dříve.

* Ujistěte se, že hello připojovací řetězec v vašeho souboru .cscfg je přesný.
* Zkontrolujte, že hello server a databáze existují a správnost hello id uživatele a heslo.
* Pro databáze SQL Azure musí mít připojovací řetězec hello hello formuláře:

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* Ujistěte se, že název serveru hello nezačíná **https://**.
* Zajistěte, aby váš server Azure SQL DB povolovalo tooit tooconnect služeb Azure. toodo, otevřete https://manage.windowsazure.com, na tlačítko "Databází SQL" na hello vlevo, klikněte na tlačítko "Servery" hello horní a vyberte svůj server. Klikněte na tlačítko **konfigurace** v hello top a ujistěte se, že hello **služeb Azure** nastavení je příliš "Ano". (Viz požadavky hello na hello horní části tohoto článku v části).

## <a name="test-hello-service-deployment"></a>Testovací nasazení pro službu hello
### <a name="connect-with-a-web-browser"></a>Připojení s webovým prohlížečem
Určení hello koncový bod webové služby rozdělení sloučení. To můžete najít v hello portálu Azure Classic podle budete toohello **řídicí panel** cloudové služby a vyhledávání v části **adresa URL webu** na pravé straně hello. Nahraďte **http://** s **https://** vzhledem k tomu, že výchozí nastavení zabezpečení hello zakázat hello HTTP koncový bod. Načíst stránku hello pro tuto adresu URL do prohlížeče.

### <a name="test-with-powershell-scripts"></a>Testování pomocí skriptů prostředí PowerShell
Hello nasazení a prostředí můžete otestovat pomocí spouštění skriptů prostředí PowerShell ukázkový hello zahrnuty.

soubory skriptu Hello zahrnuté jsou:

1. **SetupSampleSplitMergeEnvironment.ps1** -nastaví datovou vrstvu testu pro rozdělení/Merge (v tabulce níže najdete podrobný popis)
2. **ExecuteSampleSplitMerge.ps1** -provádí operace test na hello testovací datové vrstvy (v tabulce níže najdete podrobný popis)
3. **GetMappings.ps1** – nejvyšší úrovně ukázkový skript, který vytiskne hello aktuální stav mapování horizontálních hello.
4. **ShardManagement.psm1** – Pomocník skript, který zabalí hello ShardManagement rozhraní API
5. **SqlDatabaseHelpers.psm1** – Pomocník skriptu pro vytváření a správu databází SQL
   
   <table style="width:100%">
     <tr>
       <th>Soubor PowerShell</th>
       <th>Kroky</th>
     </tr>
     <tr>
       <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
       <td>1.    Vytvoří databázi správce horizontálního oddílu mapy</td>
     </tr>
     <tr>
       <td>2.    Vytvoří 2 horizontálního oddílu databáze.
     </tr>
     <tr>
       <td>3.    Vytvoří mapu horizontálního oddílu pro tyto databáze (odstraní všechny existující horizontálního oddílu mapování na tyto databáze). </td>
     </tr>
     <tr>
       <td>4.    Vytvoří tabulku malé ukázkové v obou hello horizontálních oddílů a naplní hello tabulky v jednom z hello horizontálních oddílů.</td>
     </tr>
     <tr>
       <td>5.    Deklaruje hello SchemaInfo pro horizontálně dělenou tabulku hello.</td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th>Soubor PowerShell</th>
       <th>Kroky</th>
     </tr>
   <tr>
       <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
       <td>1.    Odešle rozdělení požadavek toohello rozdělení sloučení služby webové front-end, která rozdělí poloviční hello data z hello první horizontálního oddílu toohello druhý horizontálního oddílu.</td>
     </tr>
     <tr>
       <td>2.    Hlasování hello front-endu webové pro hello rozdělení stavu požadavku a čeká, až do dokončení požadavku hello.</td>
     </tr>
     <tr>
       <td>3.    Odešle sloučení požadavek toohello rozdělení sloučení služby webové front-end, který přesouvá hello data z hello druhý horizontálního oddílu back toohello první horizontálního oddílu.</td>
     </tr>
     <tr>
       <td>4.    Hlasování hello front-endu webové pro stav žádosti o sloučení hello a čeká, až do dokončení požadavku hello.</td>
     </tr>
   </table>
   
## <a name="use-powershell-tooverify-your-deployment"></a>Pomocí prostředí PowerShell tooverify nasazení
1. Otevřete nové okno prostředí PowerShell a přejděte toohello adresáře, kam jste stáhli balíček rozdělení sloučení hello a pak přejděte do adresáře "powershell" hello.
2. Vytvoření serveru Azure SQL database (nebo vyberte existující server) kde bude vytvořen hello horizontálního oddílu mapa správce a horizontálních oddílů.
   
   > [!NOTE]
   > Hello SetupSampleSplitMergeEnvironment.ps1 skript vytvoří těchto databází na hello stejný server výchozí tookeep hello skript jednoduché. Toto není omezení hello rozdělení sloučení služby sám sebe.
   >
   
   Přihlášení k ověřování SQL s toohello přístup pro čtení a zápis, Operations Manager bude nutná pro hello sloučení rozdělení dat toomove služby a mapovat horizontálního oddílu hello aktualizace. Vzhledem k tomu, že hello rozdělení sloučení služba běží v cloudu hello, nepodporuje aktuálně integrované ověřování.
   
   Ověřte, zda je server Azure SQL hello nakonfigurována tooallow přístup z hello IP adresu počítače hello spuštění těchto skriptů. Můžete najít toto nastavení v rámci serveru Azure SQL hello / configuration / povolené IP adresy.
3. Spusťte hello SetupSampleSplitMergeEnvironment.ps1 skriptu toocreate hello ukázkové prostředí.
   
   Spuštěním tohoto skriptu budou vymazat všechny existující data správy mapy horizontálního oddílu struktury na hello horizontálního oddílu mapa správce databáze a hello horizontálních oddílů. To může být užitečné toorerun hello skriptu, pokud chcete inicializovat toore hello horizontálního oddílu mapy nebo horizontálních oddílů.
   
   Ukázka příkazového řádku:

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. Spustíte hello Getmappings.ps1 skriptu tooview hello mapování, která momentálně existují v prostředí ukázka hello.
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. Hello ExecuteSampleSplitMerge.ps1 skriptu tooexecute provést operaci rozdělení (přesouvání poloviční hello dat na hello první horizontálního oddílu toohello druhý horizontálních) a pak operace sloučení (přesouvání hello dat zpět na první horizontálního oddílu hello). Pokud jste nakonfigurovali, SSL a koncový bod http levém hello zakázáno, ujistěte se, použijte místo toho hello https:// koncový bod.
   
   Ukázka příkazového řádku:

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   Pokud se zobrazí hello níže chyba, je pravděpodobně k potížím s certifikátem váš koncový bod webové. Pokuste se připojit koncový bod webové toohello pomocí Oblíbené webový prohlížeč a zkontrolujte, jestli Chyba certifikátu.
   
     ```
     Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLSsecure channel.
     ```
   
   Pokud byly úspěšné, hello výstup by měl vypadat jako hello níže:
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. Experimentujte s jiné datové typy! Všechny tyto skripty trvat volitelný parametr - ShardKeyType, který vám umožní typ klíče toospecify hello. Výchozí hodnota Hello je Int32, ale můžete také určit Int64, identifikátor Guid nebo Binary.

## <a name="create-requests"></a>Požadavky na vytvoření
Hello služby lze pomocí hello webového uživatelského rozhraní nebo importováním a modulu SplitMerge.psm1 PowerShell hello, které se budou odesílat své žádosti prostřednictvím hello webové role.

Hello služby můžete přesouvat data v horizontálně dělené tabulky a referenční tabulky. Horizontálně dělenou tabulku má klíčový sloupec horizontálního dělení a má jiný řádek dat v každém horizontálního oddílu. Referenční tabulka není horizontálně dělené tak, aby obsahovala hello stejný řádek dat v každé horizontálního oddílu. Referenční tabulky jsou užitečné pro data, která se nemění často a je použité tooJOIN s horizontálně dělené tabulky v dotazech.

V pořadí tooperform operace sloučení rozdělení musí deklarovat hello horizontálně dělené tabulky a referenční tabulky, které chcete přesunout toohave. To je provedeno pomocí hello **SchemaInfo** rozhraní API. Toto rozhraní API je v hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** oboru názvů.

1. Pro každou horizontálně dělenou tabulku, vytvořte **ShardedTableInfo** objekt popisující název schématu nadřazená tabulka hello (volitelné, výchozí příliš "dbo"), hello název tabulky a název sloupce v této tabulce, který obsahuje klíč horizontálního dělení hello hello.
2. Pro každý odkaz na tabulku vytvořit **ReferenceTableInfo** objekt popisující název schématu nadřazená tabulka hello (volitelné, použije se výchozí hodnota příliš "dbo") a název tabulky hello.
3. Přidat hello výše TableInfo objekty tooa nové **SchemaInfo** objektu.
4. Získat odkaz na tooa **ShardMapManager** objekt a volání **GetSchemaInfoCollection**.
5. Přidat hello **SchemaInfo** toohello **SchemaInfoCollection**, poskytuje název pro mapování horizontálních hello.

Příklad toho si můžete prohlédnout ve hello SetupSampleSplitMergeEnvironment.ps1 skriptu.

Hello rozdělení sloučení služby nevytvoří hello cílová databáze (nebo schéma pro všechny tabulky v databázi hello) za vás. Musí být předem vytvořené před odesláním požadavku toohello služby.

## <a name="troubleshooting"></a>Řešení potíží
Při spouštění skriptů prostředí powershell ukázkový text hello, mohou se zobrazit hello následující zpráva:

   ```
   Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLS secure channel.
   ```

Tato chyba znamená, že svůj certifikát SSL není správně nakonfigurován. Postupujte podle pokynů hello v části "připojení s webovým prohlížečem.

Pokud nelze odeslat požadavky mohou se zobrazit tato:

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

V takovém případě zkontrolujte konfigurační soubor, v nastavení konkrétní hello **WorkerRoleSynchronizationStorageAccountConnectionString**. Tato chyba obvykle značí, že tato role pracovního procesu hello nelze úspěšně spustit hello metadata databáze při prvním použití. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

