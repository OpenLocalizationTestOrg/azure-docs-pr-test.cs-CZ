---
title: "aaaUse hello emulátoru úložiště Azure pro vývoj a testování | Microsoft Docs"
description: "emulátor úložiště Azure Hello poskytuje volné místní vývojové prostředí pro vývoj a testování aplikací s Azure Storage. Zjistěte, jak žádosti o ověření, jak tooconnect toohello emulátoru z vaší aplikace a jak toouse hello nástroj příkazového řádku."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: f480b059-df8a-4a63-b05a-7f2f5d1f5c2a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: marsma
ms.openlocfilehash: 7cbe6ef5f172bb526c9d8a329b2fe9e6c0ec59d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-storage-emulator-for-development-and-testing"></a>Používejte hello emulátoru úložiště Azure pro vývoj a testování

emulátor úložiště Microsoft Azure Hello poskytuje místní prostředí, které emuluje hello služby Azure Blob, Queue a Table pro účely vývoje. Pomocí emulátoru úložiště hello, můžete otestovat aplikaci proti místně, služba hello úložiště bez vytváření předplatného Azure nebo nákladům. Až budete spokojeni s jak funguje aplikaci v emulátoru hello, můžete přepnout toousing účet úložiště Azure v cloudu hello.

## <a name="get-hello-storage-emulator"></a>Získat emulátor úložiště hello
Hello emulátor úložiště je k dispozici jako součást hello [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Emulátor úložiště hello můžete nainstalovat také pomocí hello [samostatný instalační program systému](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) (přímé ke stažení). emulátor úložiště hello tooinstall, musíte mít oprávnění správce v počítači.

emulátor úložiště Hello aktuálně běží jenom v systému Windows. Pro ty zvažování emulátor úložiště pro Linux, jednou z možností je hello komunity udržovat, emulátor úložiště s otevřeným zdrojem [Azurite](https://github.com/arafato/azurite).

> [!NOTE]
> Data vytvořená ve verzi emulátoru úložiště hello není zaručena toobe dostupné při použití jiné verze. Pokud budete potřebovat toopersist data pro dlouhodobé hello, doporučujeme uložit data v účtu úložiště Azure, ne jako emulátor úložiště hello.
> <p/>
> emulátor úložiště Hello závisí na konkrétní verze knihoven hello OData. Nahrazování hello OData knihovny DLL používané hello emulátor úložiště s jinými verzemi není podporován a můžou způsobit neočekávané chování. Všechny verze OData podporován službou úložiště hello však může být použité toosend požadavky toohello emulátor.
>

## <a name="how-hello-storage-emulator-works"></a>Jak funguje emulátor úložiště hello
emulátor úložiště Hello používá místní instanci systému Microsoft SQL Server a služby Azure storage tooemulate systému místního souboru hello. Ve výchozím nastavení hello emulátor úložiště používá databázi v Microsoft SQL Server 2012 Express LocalDB. Můžete tooaccess emulátor úložiště hello tooconfigure místní instanci systému SQL Server namísto hello instanci LocalDB. Další informace najdete v tématu hello [spuštění a inicializaci hello emulátor úložiště](#start-and-initialize-the-storage-emulator) později v tomto článku.

emulátor úložiště Hello připojí tooSQL serveru nebo LocalDB pomocí ověřování systému Windows.

Existují některé rozdíly ve funkcích mezi hello emulátor úložiště a služby úložiště Azure. Další informace o tyto rozdíly, najdete v části hello [rozdíly mezi emulátor úložiště hello a Azure Storage](#differences-between-the-storage-emulator-and-azure-storage) později v tomto článku.

## <a name="start-and-initialize-hello-storage-emulator"></a>Spuštění a inicializaci emulátor úložiště hello
emulátor úložiště Azure toostart hello:
1. Vyberte hello **spustit** tlačítko nebo klikněte na tlačítko hello **Windows** klíč.
1. Začněte psát `Azure Storage Emulator`.
1. Vyberte hello emulátoru hello seznamu zobrazených aplikací.

Když se spustí hello emulátor úložiště, zobrazí se okno příkazového řádku. Můžete použít tuto konzolu okno toostart a ukončení hello emulátor úložiště, vymazat data, získat stav a inicializace hello emulátor. Další informace najdete v tématu hello [odkaz na nástroj příkazového řádku emulátor úložiště](#storage-emulator-command-line-tool-reference) později v tomto článku.

Když je spuštěný emulátor hello, uvidíte na ikonu v oznamovací oblasti hlavního panelu Windows hello.

Když zavřete okno příkazového řádku emulátor úložiště hello hello emulátor úložiště bude toorun. toobring do okna konzoly emulátor úložiště hello znovu, postupujte podle předchozích krocích, jako kdyby spouštění emulátor úložiště hello hello.

Hello prvním spuštění hello emulátor úložiště, místní úložiště prostředí hello je inicializován pro vás. Proces inicializace Hello vytvoří databázi v instanci LocalDB a rezerv HTTP portů pro každou službu místní úložiště.

emulátor úložiště Hello je ve výchozím nastavení nainstalovaná příliš`C:\Program Files (x86)\Microsoft SDKs\Azure\Storage Emulator`.

> [!TIP]
> Můžete použít hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) toowork s prostředky emulátor místního úložiště. Vyhledejte "(vývoj)" v části "Účty úložiště" ve stromu prostředky Storage Explorer hello po nainstalována a spuštěna emulátor úložiště hello.
>

### <a name="initialize-hello-storage-emulator-toouse-a-different-sql-database"></a>Inicializace toouse emulátor úložiště hello do jiné databáze SQL
Můžete použít hello úložiště emulátoru nástroj příkazového řádku tooinitialize hello úložiště emulátoru toopoint tooa instance databáze serveru SQL než instanci LocalDB výchozí hello:

1. Okno konzoly emulátor úložiště otevřete hello jak je popsáno v hello [spuštění a inicializaci hello emulátor úložiště](#start-and-initialize-the-storage-emulator) části.
1. V okně konzoly hello zadejte hello následující příkaz, kde `<SQLServerInstance>` je hello název instance systému SQL Server hello. Zadejte toouse LocalDB, `(localdb)\MSSQLLocalDb` jako instanci systému SQL Server hello.

  `AzureStorageEmulator.exe init /server <SQLServerInstance>`

  Můžete také použít hello následující příkaz, který přesměruje hello emulátoru toouse hello výchozí instanci SQL serveru:

  `AzureStorageEmulator.exe init /server .\\`

  Nebo můžete použít následující příkaz, který znovu inicializuje instanci LocalDB hello databáze toohello výchozí hello:

  `AzureStorageEmulator.exe init /forceCreate`

Další informace o těchto příkazech najdete v tématu [odkaz na nástroj příkazového řádku emulátor úložiště](#storage-emulator-command-line-tool-reference).

> [!TIP]
> Můžete použít hello [Microsoft SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) instance systému SQL Server, včetně instalace LocalDB hello toomanage (SSMS). V hello SMSS **připojit tooServer** dialogové okno, zadejte `(localdb)\MSSQLLocalDb` v hello **název serveru:** instanci LocalDB toohello tooconnect pole.

## <a name="authenticating-requests-against-hello-storage-emulator"></a>Ověřování požadavků na emulátor úložiště hello
Jakmile jste instalaci a spuštění hello emulátor úložiště, můžete otestovat svůj kód s ho. Stejně jako u Azure Storage v cloudu hello, musí být ověřeny všechny žádosti, které provedete emulátoru úložiště hello, pokud je požadavek anonymní. Požadavky na emulátoru úložiště hello pomocí ověřování sdílený klíč, nebo pomocí sdíleného přístupového podpisu (SAS), můžete ověřovat.

### <a name="authenticate-with-shared-key-credentials"></a>Ověření pomocí sdíleného klíče pověření
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Další informace o připojovacích řetězcích najdete v tématu [připojovacích řetězců Azure Storage konfigurace](../storage-configure-connection-string.md).

### <a name="authenticate-with-a-shared-access-signature"></a>Ověření pomocí sdíleného přístupového podpisu
Některé knihovny klienta úložiště Azure, jako je například hello knihovně Xamarin, podporují pouze ověřování pomocí tokenu sdíleného přístupového podpisu (SAS). Můžete vytvořit token SAS hello pomocí nástroje, například hello [Storage Explorer](http://storageexplorer.com/) nebo jiné aplikace, která podporuje ověření sdíleným klíčem.

Můžete také vygenerovat SAS token pomocí prostředí Azure PowerShell. Hello následující příklad vytvoří SAS token se kontejner objektů blob tooa úplná oprávnění:

1. Pokud jste to ještě neudělali, nainstalujte prostředí Azure PowerShell (pomocí hello nejnovější verzi rutin prostředí Azure PowerShell hello je doporučeno). Pokyny k instalaci naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps).
2. Otevřete prostředí Azure PowerShell a spusťte následující příkazy, nahraďte hello `ACCOUNT_NAME` a `ACCOUNT_KEY==` s svoje vlastní přihlašovací údaje a `CONTAINER_NAME` s názvem vašeho výběru:

```powershell
$context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="

New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context

$now = Get-Date

New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri
```

Hello výsledné sdílený přístupový podpis identifikátor URI pro nový kontejner hello by měl vypadat podobně jako:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss
```

sdílený přístupový podpis Hello vytvořené pomocí v tomto příkladu je platný pro jeden den. podpis Hello uděluje úplný přístup (pro čtení, zápisu, odstranění, seznamu) tooblobs hello kontejneru.

Další informace o sdílených přístupových podpisů najdete v tématu [pomocí sdílené přístupové podpisy (SAS) ve službě Azure Storage](../storage-dotnet-shared-access-signature-part-1.md).

## <a name="addressing-resources-in-hello-storage-emulator"></a>Přidělování prostředků v emulátoru úložiště hello
Koncové body služby Hello pro emulátor úložiště hello se liší od těch, které účet úložiště Azure. Hello rozdílem je, protože hello místního počítače nebude provádět překlad názvu domény, které vyžadují hello úložiště emulátoru koncové body toobe místní adresy.

Při adresování prostředků v účtu úložiště Azure, je použít následující schéma hello. název účtu Hello je součástí názvu hostitele hello URI a hello prostředků adresovaném je část cesty identifikátoru URI hello:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Například hello následující identifikátor URI je platnou adresu pro objekt blob v účtu úložiště Azure:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Ale hello emulátor úložiště, protože hello místního počítače nebude provádět překlad názvu domény, název účtu hello je část cesty identifikátoru URI hello místo názvu hostitele hello. Použijte následující formát identifikátoru URI pro prostředek v emulátoru úložiště hello hello:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Například hello následující adresy se dají používat pro přístup k objektu blob v emulátoru úložiště hello:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

Koncové body služby Hello pro emulátor úložiště hello jsou:

* Služba objektů blob:`http://127.0.0.1:10000/<account-name>/<resource-path>`
* Služba front:`http://127.0.0.1:10001/<account-name>/<resource-path>`
* Služba Table:`http://127.0.0.1:10002/<account-name>/<resource-path>`

### <a name="addressing-hello-account-secondary-with-ra-grs"></a>Adresování hello účet sekundární s RA-GRS
Od verze 3.1, emulátor úložiště hello podporuje geo redundantní replikaci přístup pro čtení (RA-GRS). Pro prostředky úložiště v cloudu hello i v emulátoru místního hello, dostanete hello sekundárního umístění pomocí připojení – název účtu sekundární toohello. Například hello následující adresy se dají používat pro přístup k objektu blob pomocí sekundární hello jen pro čtení v emulátoru úložiště hello:

`http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt`

> [!NOTE]
> Pro toohello programový přístup sekundární pomocí emulátoru úložiště hello použijte hello Klientská knihovna pro úložiště pro .NET verze 3.2 nebo vyšší. V tématu hello [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) podrobnosti.
>
>

## <a name="storage-emulator-command-line-tool-reference"></a>Odkaz na nástroj příkazového řádku emulátor úložiště
Od verze 3.0 se do okna konzoly se zobrazí při spuštění hello emulátor úložiště. Použijte příkazový řádek hello v hello konzoly okno toostart a ukončení hello emulátoru, jakož i dotazu pro stav a provádění dalších operací.

> [!NOTE]
> Pokud máte hello Microsoft Azure výpočetní emulátor nainstalovaný, se zobrazí při spuštění hello emulátor úložiště ikonu na hlavním panelu. Klikněte pravým tlačítkem myši na ikonu tooreveal hello nabídky, která poskytuje grafický způsob toostart a zastavit hello emulátoru úložiště.
>
>

### <a name="command-line-syntax"></a>Syntaxe příkazového řádku
`AzureStorageEmulator.exe [start] [stop] [status] [clear] [init] [help]`

### <a name="options"></a>Možnosti
tooview hello seznam možností, typ `/help` hello příkazového řádku.

| Možnost | Popis | Příkaz | Argumenty |
| --- | --- | --- | --- |
| **Start** |Spustí se emulátor úložiště hello. |`AzureStorageEmulator.exe start [-inprocess]` |*-inprocess*: Spusťte emulátor hello v aktuálním procesu hello místo vytvoření nového procesu. |
| **Stop** |Emulátor úložiště hello zastaví. |`AzureStorageEmulator.exe stop` | |
| **Stav** |Výtisků hello stav emulátor úložiště hello. |`AzureStorageEmulator.exe status` | |
| **Zrušte zaškrtnutí** |Vymaže data hello ve všech služeb zadat na příkazovém řádku hello. |`AzureStorageEmulator.exe clear [blob] [table] [queue] [all]                                                    ` |*objekt BLOB*: blob, které vymaže data. <br/>*fronty*: vymaže data fronty. <br/>*Tabulka*: vymaže data tabulky. <br/>*všechny*: Vymaže všechna data v všechny služby. |
| **Init –** |Provede jednorázovou inicializace tooset si emulátor hello. |<code>AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate&#124;-skipcreate] [-reserveports&#124;-unreserveports] [-inprocess]</code> |*-server serverName\instanceName*: Určuje hostování hello instance systému SQL server hello. <br/>*-sqlinstance instanceName*: Určuje název hello toobe instance SQL hello používá v hello výchozí instanci serveru. <br/>*-forcecreate*: Vynutí vytvoření hello SQL databáze, i v případě, že již existuje. <br/>*-skipcreate*: přeskočí vytvoření databáze SQL hello. To má přednost před - forcecreate.<br/>*-reserveports*: pokusí porty hello HTTP tooreserve souvisejících se službou hello.<br/>*-unreserveports*: pokusí tooremove rezervace hello HTTP porty souvisejících se službou hello. To má přednost před - reserveports.<br/>*-inprocess*: provede inicializaci v aktuálním procesu hello místo například nový proces. aktuální proces Hello musí být spuštěn se zvýšenými oprávněními, pokud změnit port rezervace. |

## <a name="differences-between-hello-storage-emulator-and-azure-storage"></a>Rozdíly mezi emulátor úložiště hello a Azure Storage
Protože hello emulátor úložiště je emulovaných prostředí spuštěné v místní instanci SQL, existují rozdíly ve funkcích mezi hello emulátoru a účet úložiště Azure v cloudu hello:

* emulátor úložiště Hello podporuje pouze jeden účet opravené a dobře známé ověřovací klíč.
* emulátor úložiště Hello není služba škálovatelné úložiště a nepodporuje velký počet souběžných klientů.
* Jak je popsáno v [adresování prostředků v emulátoru úložiště hello](#addressing-resources-in-the-storage-emulator), prostředky jsou řešit odlišně v emulátoru úložiště hello a účet úložiště Azure. Tento rozdíl je, protože překlad názvu domény je k dispozici v hello cloudu, ale ne v místním počítači hello.
* Od verze 3.1, účet emulátor úložiště hello podporuje geo redundantní replikaci přístup pro čtení (RA-GRS). V emulátoru hello všechny účty mít povoleno RA-GRS a nikdy neexistuje žádné prodleva mezi hello primární a sekundární repliky. Operace získání statistiky služby objektů Blob, získat statistiky fronty služby a získat statistiky tabulky služby Hello jsou podporovány v účtu hello sekundární a vždy vrátí hodnotu hello hello `LastSyncTime` element odpovědi jako hello aktuální čas podle toohello základní Databáze SQL.
* Hello Souborová služba a koncové body služby protokolu SMB nejsou aktuálně podporovány v emulátoru úložiště hello.
* Pokud používáte verzi služby hello úložiště, který ještě není podporovaný emulátorem hello, emulátor úložiště hello vrátí chybu VersionNotSupportedByEmulator (kód stavu HTTP 400 – Chybný požadavek).

### <a name="differences-for-blob-storage"></a>Rozdíly pro úložiště objektů Blob
Hello následující rozdíly použít tooBlob úložiště v emulátoru hello:

* emulátor úložiště Hello podporuje pouze velikost objektu blob až too2 GB.
* Přírůstkové kopie umožňuje snímky z zkopírovali, toobe přepsána objekty BLOB, který vrátí chybu ve službě hello.
* Diff rozsahů stránek Get nefunguje mezi snímky zkopírovat pomocí přírůstkové kopie objektů Blob.
* Operace Put objekt Blob může být úspěšné proti objekt blob, který již existuje v emulátoru úložiště hello s aktivní zapůjčení, i v případě, že v požadavku hello nebylo zadáno ID zapůjčení hello.
* Append – objekt Blob emulátorem hello nepodporuje operace. Probíhá pokus operaci na doplňovací objekt blob vrátí chybu FeatureNotSupportedByEmulator (kód stavu HTTP 400 – Chybný požadavek).

### <a name="differences-for-table-storage"></a>Rozdíly pro úložiště tabulek
Hello následující rozdíly použít tooTable úložiště v emulátoru hello:

* Vlastnosti kalendářních dat v hello služby Table v emulátoru úložiště hello podporují pouze hello rozsahu nepodporuje (jsou požadované toobe později než 1. ledna 1753) systému SQL Server 2005. Došlo ke změně všechna data před 1. ledna 1753 toothis hodnotu. Hello přesnost kalendářních dat je omezená toohello přesnost systému SQL Server 2005, což znamená, že data jsou přesné too1/300th sekundy.
* emulátor úložiště Hello podporuje oddílu klíč řádku klíče hodnoty vlastností a menší než 512 bajtů. Kromě toho hello celková velikost hello název účtu, název tabulky a názvů vlastností klíče společně nesmí přesahovat 900 bajtů.
* Celková velikost řádku tabulky v emulátoru úložiště hello Hello je omezené tooless než 1 MB.
* V emulátoru úložiště hello, zadejte vlastnosti dat `Edm.Guid` nebo `Edm.Binary` podpora pouze hello `Equal (eq)` a `NotEqual (ne)` operátory porovnání v dotazu filtrovat řetězce.

### <a name="differences-for-queue-storage"></a>Rozdíly pro úložiště
Neexistují žádné konkrétní tooQueue úložiště rozdíly v emulátoru hello.

## <a name="storage-emulator-release-notes"></a>Poznámky k verzi emulátoru úložiště
### <a name="version-52"></a>Verze 5.2
* emulátor úložiště Hello teď podporuje verze 2017-04-17 hello služby úložiště na koncové body služby objektů Blob, fronty a tabulky.
* Opravit chyby, kde nebyly se správně kódovaný hodnoty vlastností tabulky.

### <a name="version-51"></a>Verze 5.1
* Pevné chyby, kde byla emulátor úložiště hello vrácení hello `DataServiceVersion` záhlaví v některé odpovědi, kde je služba hello nebyla.

### <a name="version-50"></a>Verze 5.0
* Instalační program emulátor úložiště Hello už existující MSSQL kontroluje a nainstaluje rozhraní .NET Framework.
* Instalační program emulátor úložiště Hello už vytvoří hello databázi jako součást instalace. Stále se vytvoří databáze v případě potřeby jako součást spuštění.
* Vytvoření databáze už se vyžaduje zvýšení oprávnění.
* Rezervace port již nejsou potřebné pro spuštění.
* Přidá hello následující možnosti příliš`init`: `-reserveports` (vyžaduje zvýšení oprávnění), `-unreserveports` (vyžaduje zvýšení oprávnění), `-skipcreate`.
* Hello uživatelské prostředí emulátoru úložiště možnost na ikonu na hlavním panelu hello teď spustí hello rozhraní příkazového řádku. původní grafickým uživatelským rozhraním Hello již není k dispozici.
* Některých knihoven DLL přesunutý nebo přejmenovaný.

### <a name="version-46"></a>Verze 4.6
* emulátor úložiště Hello teď podporuje verze 2016-05-31 hello služby úložiště na koncové body služby objektů Blob, fronty a tabulky.

### <a name="version-45"></a>verze 4.5
* Opravit chyby, která způsobila inicializace a instalaci toofail emulátor úložiště hello, pokud byla přejmenována hello zálohování databáze.

### <a name="version-44"></a>Verze 4.4
* emulátor úložiště Hello teď podporuje verze 2015-12-11 hello služby úložiště na koncové body služby objektů Blob, fronty a tabulky.
* Hello uvolňování paměti emulátor úložiště dat blob je nyní efektivnější při plánování práce s velkého počtu objektů BLOB.
* Opravit chyby, která způsobila, že kontejner seznamu ACL XML toobe ověřit trochu jinak z jak služba úložiště hello dělá.
* Pevné chyby, který může někdy dojít maximum a toobe hodnoty data a času min jsou uvedeny v časovém pásmu nesprávný hello.

### <a name="version-43"></a>Verze 4.3
* emulátor úložiště Hello teď podporuje verze 2015-07-08 hello služby úložiště na koncové body služby objektů Blob, fronty a tabulky.

### <a name="version-42"></a>Verzi 4.2
* emulátor úložiště Hello teď podporuje verze 2015-04-05 hello služby úložiště na koncové body služby objektů Blob, fronty a tabulky.

### <a name="version-41"></a>Verze 4.1
* emulátor úložiště Hello teď podporuje verze 2015-02-21 hello služby úložiště na objekt Blob, Queue a Table koncové body služby, s výjimkou hello nové funkce připojit objektů Blob.
* Pokud používáte verzi služby hello úložiště, který ještě není podporovaný emulátorem hello, vrátí hello emulátoru smysluplný chybová zpráva. Doporučujeme používat nejnovější verze emulátoru hello hello. Pokud dojde k chybě VersionNotSupportedByEmulator (kód stavu HTTP 400 – Chybný požadavek), stáhněte si prosím nejnovější verze emulátoru úložiště hello hello.
* Opravit chyby, ve kterém soupeření podmínku způsobila tabulka entity dat toobe nesprávný během operace souběžných sloučení.

### <a name="version-40"></a>verze 4.0
* emulátor úložiště Hello spustitelný soubor přejmenoval příliš*AzureStorageEmulator.exe*.

### <a name="version-32"></a>Verze 3.2
* emulátor úložiště Hello teď podporuje verze 2014-02-14 hello služby úložiště na koncové body služby objektů Blob, fronty a tabulky. Koncové body služby souboru nejsou aktuálně podporované v emulátoru úložiště hello. V tématu [Správa verzí pro hello služby úložiště Azure](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) podrobnosti o verzi 2014-02-14.

### <a name="version-31"></a>Verze 3.1
* Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) se teď podporuje v emulátoru úložiště hello. Hello získání objektu Blob služby statistiky, získat statistiky fronty služby a získat statistiky API služby tabulky jsou podporovány pro účet hello sekundární a vždy vrátí hodnotu hello elementu hello LastSyncTime odpovědi jako hello aktuální čas podle toohello základní SQL databáze. Pro toohello programový přístup sekundární pomocí emulátoru úložiště hello použijte hello Klientská knihovna pro úložiště pro .NET verze 3.2 nebo vyšší. Podrobnosti najdete v hello Microsoft Azure Storage Client Library pro .NET – referenční informace.

### <a name="version-30"></a>Verze 3.0
* emulátor úložiště Azure Hello je již poskytuje hello stejný balíček emulátoru služby výpočty hello.
* Hello úložiště emulátoru grafické uživatelské rozhraní je zastaralé považuje Skriptovatelná rozhraní příkazového řádku. Podrobnosti na hello rozhraní příkazového řádku najdete v tématu odkaz nástroj příkazového řádku emulátoru úložiště. grafické rozhraní Hello bude pokračovat toobe existuje ve verzi 3.0, ale jenom k němu při instalaci hello výpočetní emulátor kliknutím pravým tlačítkem myši na ikonu hello na hlavním panelu a výběrem možnosti zobrazit uživatelské prostředí emulátoru úložiště.
* Teď je plně podporuje verze 2013-08-15 hello služeb úložiště Azure. (Dřív byla tato verze pouze podporované v emulátoru úložiště verze 2.2.1 Preview.)

## <a name="next-steps"></a>Další kroky

* Vyhodnocení emulátor úložiště napříč platformami, udržuje komunitní s otevřeným zdrojem hello [Azurite](https://github.com/arafato/azurite). 
* [Ukázek Azure Storage pomocí rozhraní .NET](../storage-samples-dotnet.md) obsahuje ukázky kódu tooseveral odkazy můžete použít při vývoji aplikace.
* Můžete použít hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) toowork s prostředky ve vašem cloudu účet úložiště a v emulátoru úložiště hello.
