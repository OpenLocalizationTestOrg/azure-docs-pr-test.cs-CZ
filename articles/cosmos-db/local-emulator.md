---
title: "Vývoj místně pomocí emulátoru DB Cosmos Azure | Microsoft Docs"
description: "Pomocí emulátoru DB Cosmos Azure, můžete vývoj a testování vaší aplikace místně pro bezplatné bez vytváření předplatného Azure."
services: cosmos-db
documentationcenter: 
keywords: "Emulátor Azure Cosmos DB"
author: arramac
manager: jhubbard
editor: 
ms.assetid: 90b379a6-426b-4915-9635-822f1a138656
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: arramac
ms.openlocfilehash: a0f6a845a345ebd4ef0a58abf4934ce400103109
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-azure-cosmos-db-emulator-for-local-development-and-testing"></a>Použití emulátoru DB Cosmos Azure pro místní vývoj a testování

<table>
<tr>
  <td><strong>Binární soubory</strong></td>
  <td>[Stáhněte si instalační služby MSI](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><strong>Docker</strong></td>
  <td>[Úložiště docker Hub](https://hub.docker.com/r/microsoft/azure-documentdb-emulator/)</td>
</tr>
<tr>
  <td><strong>Zdroj docker</strong></td>
  <td>[GitHub](https://github.com/azure/azure-documentdb-emulator-docker)</td>
</tr>
</table>
  
Emulátor DB Cosmos Azure poskytuje místní prostředí, které emuluje služby Azure Cosmos DB pro účely vývoje. Pomocí emulátoru DB Cosmos Azure, můžete vyvíjet a testovat svou aplikaci lokálně, bez vytváření předplatného Azure nebo nákladům. Až budete spokojeni s jak funguje aplikaci v emulátoru DB Cosmos Azure, můžete přejít k používání účtu Azure Cosmos DB v cloudu.

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Instalace v emulátoru
> * Emulátor systémem Docker pro Windows
> * Ověřování požadavků
> * Pomocí Průzkumníku dat v emulátoru
> * Export certifikáty SSL
> * Volání metody emulátoru z příkazového řádku
> * Shromažďování trasovacích souborů

Doporučujeme začít následujícím videem, kde Kirill Gavrylyuk ukazuje, jak začít pracovat s emulátoru Azure DB Cosmos. Všimněte si, že odkazuje na video emulátoru jako DocumentDB emulátor, ale nástroj sám byl přejmenován emulátoru Azure Cosmos DB od zaznamenávat videa. Všechny informace ve videu jsou stále správné pro emulátor Azure DB Cosmos. 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-the-emulator-works"></a>Jak funguje v emulátoru
Emulátor DB Cosmos Azure poskytuje zachováním emulace služby Azure Cosmos DB. Podporuje stejné funkce jako Azure Cosmos databáze, včetně podpory pro vytváření a dotazování dokumentů JSON, zřizování a škálování kolekce a provádění uložené procedury a triggery. Můžete vyvíjet a testovat aplikace pomocí emulátoru DB Cosmos Azure a jejich nasazení do Azure v globálním měřítku tím, že právě konfigurací jedné změňte koncového bodu připojení pro Azure Cosmos DB.

Když jsme vytvořili místní emulace zachováním skutečné služby Azure Cosmos DB, se liší od služby implementace emulátoru Azure DB Cosmos. Například emulátoru DB Cosmos Azure používá standardní součásti operačního systému, například místního systému souborů pro trvalosti a zásobník protokolu HTTPS pro připojení k síti. To znamená, že některé funkce, které jsou závislé na infrastrukturu Azure jako globální replikace, jednociferné milisekundu latence pro čtení/zápisu a přizpůsobitelné úrovně konzistence nejsou k dispozici prostřednictvím emulátoru Azure DB Cosmos.

> [!NOTE]
> V tuto chvíli Průzkumníku dat v emulátoru podporuje pouze vytvoření kolekce DocumentDB rozhraní API a kolekcí MongoDB. Průzkumníku dat v emulátoru v současné době nepodporuje vytvoření tabulky a grafy. 

## <a name="system-requirements"></a>Požadavky na systém
Emulátor DB Cosmos Azure má následující požadavky na hardware a software:

* Požadavky na software
  * Windows Server 2012 R2, Windows Server 2016 nebo Windows 10
*   Minimální požadavky na Hardware
  * 2 GB PAMĚTI RAM
  * 10 GB volného místa na disku

## <a name="installation"></a>Instalace
Můžete stáhnout a nainstalovat emulátoru DB Cosmos Azure z [Microsoft Download Center](https://aka.ms/cosmosdb-emulator). 

> [!NOTE]
> Pro instalaci, konfiguraci a spuštění emulátoru Azure Cosmos DB, musíte mít oprávnění správce v počítači.

## <a name="running-on-docker-for-windows"></a>Systémem Docker pro Windows

Emulátor DB Cosmos Azure můžete spustit na Docker pro systém Windows. Emulátor na Docker pro Oracle Linux nefunguje.

Jakmile máte [Docker pro systém Windows](https://www.docker.com/docker-windows) nainstalován, můžete můžete načítat bitová kopie emulátoru z úložiště Docker Hub spuštěním následujícího příkazu z své oblíbené prostředí (cmd.exe, prostředí PowerShell, atd.).

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
Chcete-li spustit bitovou kopii, spusťte následující příkazy.

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

Odpověď bude vypadat podobně jako následující:

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

Zavřením interaktivní prostředí, jakmile emulátoru už spustit v emulátoru kontejnerů dojde k vypnutí.

Použijte koncový bod a hlavního klíče v z odpovědi v vašeho klienta a certifikát SSL naimportovat do svého hostitele. Chcete-li importovat certifikát SSL, proveďte následující z příkazového řádku správce:

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-the-emulator"></a>Spusťte emulátor

Spuštění emulátoru DB Cosmos Azure, vyberte tlačítko Start a stiskněte klávesu Windows. Začněte psát **emulátoru DB Cosmos Azure**a vyberte emulátor ze seznamu aplikací. 

![Kliknutím na tlačítko Start nebo stiskněte klávesu Windows, začněte psát ** Azure Cosmos DB emulátoru ** a vyberte emulátor ze seznamu aplikací](./media/local-emulator/database-local-emulator-start.png)

Když na emulátoru běží, se zobrazí na ikonu v oznamovací oblasti hlavního panelu Windows. ![Azure Cosmos DB místní emulátoru panelu oznámení](./media/local-emulator/database-local-emulator-taskbar.png)

Emulátor DB Cosmos Azure ve výchozím nastavení spouští v místním počítači ("localhost") naslouchá na portu 8081.

Emulátor DB Cosmos Azure je nainstalována ve výchozím nastavení `C:\Program Files\Azure Cosmos DB Emulator` adresáře. Můžete také spustit a zastavit emulátoru z příkazového řádku. V tématu [odkaz na nástroj příkazového řádku](#command-line) Další informace.

## <a name="start-data-explorer"></a>Spuštění Průzkumníka dat

Při spuštění v Azure Cosmos DB emulátoru se automaticky otevře Průzkumníku dat Azure Cosmos DB v prohlížeči. Adresa se zobrazí jako [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html). Pokud ho zavřete v Exploreru a chtěli později ho znovu otevřete, můžete otevřít adresu URL v prohlížeči nebo spustit z emulátoru DB Cosmos Azure v oznamovací ikoně Windows, jak je uvedeno níže.

![Azure Cosmos DB místní emulátoru data Průzkumníka Spouštěče](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Kontrola aktualizací
Průzkumník dat určuje, zda je k dispozici ke stažení nové aktualizace. 

> [!NOTE]
> Data vytvořená ve verzi emulátoru DB Cosmos Azure nemusí být dostupné při použití jiné verze. Pokud potřebujete zachovat data pro z dlouhodobého hlediska, doporučujeme uložit data v Azure Cosmos DB účet, nikoli v emulátoru Azure DB Cosmos. 

## <a name="authenticating-requests"></a>Ověřování požadavků
Stejně jako s Azure DB Cosmos v cloudu, musí být ověřeny každého požadavku, které vytváříte emulátoru DB Cosmos Azure. Emulátor DB Cosmos Azure podporuje jeden pevný účet a dobře známé ověřovací klíč pro ověřování hlavního klíče. Tento účet a klíč jsou pouze povolené pro použití s emulátoru Azure Cosmos DB přihlašovací údaje. Jsou:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> Hlavní klíč nepodporuje emulátor DB Cosmos Azure je určený pro použití pouze pomocí emulátoru. Váš účet Azure Cosmos DB produkční a klíč nelze použít s emulátoru Azure DB Cosmos. 

> [!NOTE] 
> Pokud jste spustili emulátoru s parametrem /Key, použijte generovaný klíč místo "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="

Kromě toho právě jako služba Azure Cosmos DB emulátoru DB Cosmos Azure podporuje pouze zabezpečenou komunikaci prostřednictvím protokolu SSL.

## <a name="running-the-emulator-on-a-local-network"></a>Spuštěný v emulátoru v místní síti

Emulátor serveru můžete spustit v místní síti. Pokud chcete povolit přístup k síti, zadejte možnost /AllowNetworkAccess na [příkazového řádku](#command-line-syntax), což také vyžaduje, že zadáváte /Key = key_string nebo/keyfile = název_souboru. Můžete použít /GenKeyFile = název_souboru můžete vytvořit soubor s předem náhodný klíč.  Pak můžete předat, že k/keyfile = název_souboru nebo /Key = contents_of_file.

Pokud chcete povolit přístup k síti první uživatel by měl vypnutí emulátoru a odstranit adresář data na emulátoru (C:\Users\user_name\AppData\Local\CosmosDBEmulator).

## <a name="developing-with-the-emulator"></a>Vývoj v emulátoru
Jakmile máte emulátoru DB Cosmos Azure spuštěna na pracovní ploše, můžete použít libovolnou podporované [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) nebo [REST API služby Azure Cosmos DB](/rest/api/documentdb/) pro interakci s emulátor. Emulátor DB Cosmos Azure také zahrnuje integrovanou Průzkumníku dat, která umožňuje vytvářet kolekce pro DocumentDB a rozhraní API MongoDB a zobrazení a úpravám dokumentů bez psaní jakéhokoli kódu.   

    // Connect to the Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

Pokud používáte [Azure Cosmos DB podporou protokolů pro MongoDB](mongodb-introduction.md), použijte následující připojovací řetězec:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

Můžete použít stávající nástroje, například [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) pro připojení k emulátoru Azure DB Cosmos. Můžete také migrovat data mezi emulátoru DB Cosmos Azure a pomocí služby Azure Cosmos DB [nástroj pro migraci dat Azure Cosmos DB](https://github.com/azure/azure-documentdb-datamigrationtool).

> [!NOTE] 
> Pokud jste spustili emulátoru s parametrem /Key, použijte generovaný klíč místo "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="

Pomocí emulátoru služby v Azure Cosmos DB, ve výchozím nastavení, je možné vytvořit až 25 kolekce tvořené jedním oddílem nebo 1 dělenou kolekci. Další informace o změně tuto hodnotu najdete v tématu [nastavení hodnoty PartitionCount](#set-partitioncount).

## <a name="export-the-ssl-certificate"></a>Export certifikátu protokolu SSL

Jazyky rozhraní .NET a prostředí runtime použití úložiště certifikátů systému Windows k bezpečnému připojování k místní emulátoru Azure Cosmos DB. Další jazyky mít vlastní metodu správy a použití certifikátů. Java používá vlastní [úložiště certifikátů](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) zatímco používá Python [soketu obálky](https://docs.python.org/2/library/ssl.html).

Aby bylo možné získat certifikát pro použití s jazyky a moduly runtime, který nelze integrovat do úložiště certifikátů Windows, musíte ho exportovat pomocí Správce certifikátů systému Windows. Můžete spusťte ji spuštěním certlm.msc nebo pokyny krok za krokem v [exportu certifikátů emulátoru Azure Cosmos DB](./local-emulator-export-ssl-certificates.md). Jakmile správce certifikátů pracuje, otevřete osobní certifikáty, jak je uvedeno níže a exportujte certifikát s popisným názvem "DocumentDBEmulatorCertificate" jako kódováním BASE-64 souboru X.509 (.cer).

![Certifikát SSL místní emulátoru služby Azure Cosmos DB](./media/local-emulator/database-local-emulator-ssl_certificate.png)

Certifikát X.509 lze importovat do úložiště certifikátů Java podle pokynů v [přidání certifikátu do úložiště certifikátů certifikační Autority Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store). Jakmile je certifikát importován do úložiště certifikátů, bude možné se připojit k emulátoru DB Cosmos Azure aplikací Java a MongoDB.

Při připojování k emulátoru z Pythonu a Node.js SDK, je zakázáno ověřování SSL.

## <a id="command-line"></a>Odkaz na nástroj příkazového řádku
Z umístění instalace můžete na příkazovém řádku spuštění a zastavení emulátoru, nakonfigurujte možnosti a provádění dalších operací.

### <a name="command-line-syntax"></a>Syntaxe příkazového řádku

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

Chcete-li zobrazit seznam možností, zadejte `CosmosDB.Emulator.exe /?` na příkazovém řádku.

<table>
<tr>
  <td><strong>Možnost</strong></td>
  <td><strong>Popis</strong></td>
  <td><strong>Příkaz</strong></td>
  <td><strong>Argumenty</strong></td>
</tr>
<tr>
  <td>[Žádný argument]</td>
  <td>Spuštění emulátoru Azure Cosmos databáze s výchozím nastavením.</td>
  <td>CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>[Help]</td>
  <td>Zobrazí seznam podporovaných argumentů příkazového řádku.</td>
  <td>CosmosDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>Vypnutí</td>
  <td>Vypne emulátoru Azure DB Cosmos.</td>
  <td>/ CosmosDB.Emulator.exe Shutdown</td>
  <td></td>
</tr>
<tr>
  <td>DataPath</td>
  <td>Určuje cestu, do kterého chcete uložit datové soubory. Výchozí hodnota je % LocalAppdata%\CosmosDBEmulator.</td>
  <td>CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</td>
  <td>&lt;DataPath&gt;: přístupná cesta</td>
</tr>
<tr>
  <td>Port</td>
  <td>Určuje číslo portu pro použití pro emulátor.  Výchozí hodnota je 8081.</td>
  <td>/ CosmosDB.Emulator.exe port =&lt;portu&gt;</td>
  <td>&lt;port&gt;: jedno číslo portu</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>Udává číslo portu, který chcete použít pro MongoDB kompatibility rozhraní API. Výchozí hodnota je 10255.</td>
  <td>CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</td>
  <td>&lt;mongoport&gt;: jedno číslo portu</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Určuje porty, které chcete použít pro přímé připojení k síti. Výchozí hodnoty jsou 10251,10252,10253,10254.</td>
  <td>CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</td>
  <td>&lt;directports&gt;: seznam s položkami oddělenými čárkou 4 porty</td>
</tr>
<tr>
  <td>Klíč</td>
  <td>Autorizační klíč pro emulátor. Klíč musí být kódování base-64 vektoru 64 bajtů.</td>
  <td>CosmosDB.Emulator.exe /Key:&lt;klíč&gt;</td>
  <td>&lt;klíč&gt;: klíč musí být kódování base-64 vektoru 64 bajtů</td>
</tr>
<tr>
  <td>EnableRateLimiting</td>
  <td>Určuje, že rychlost požadavků, omezení chování je povolena.</td>
  <td>CosmosDB.Emulator.exe /EnableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>DisableRateLimiting</td>
  <td>Určuje, že rychlost požadavků, omezení chování je zakázán.</td>
  <td>CosmosDB.Emulator.exe /DisableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>NoUI</td>
  <td>Nezobrazovat emulátoru uživatelské rozhraní.</td>
  <td>/ Noui CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>NoExplorer</td>
  <td>Při spuštění nezobrazovat Průzkumníka dokumentů.</td>
  <td>CosmosDB.Emulator.exe /NoExplorer</td>
  <td></td>
</tr>
<tr>
  <td>PartitionCount</td>
  <td>Určuje maximální počet dělené kolekce. V tématu [změnit počet kolekcí](#set-partitioncount) Další informace.</td>
  <td>CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</td>
  <td>&lt;partitioncount&gt;: maximální počet povolených kolekce tvořené jedním oddílem. Výchozí hodnota je 25. Maximální povolený počet je 250.</td>
</tr>
<tr>
  <td>DefaultPartitionCount</td>
  <td>Určuje výchozí počet oddílů pro dělenou kolekci.</td>
  <td>CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</td>
  <td>&lt;defaultpartitioncount&gt; výchozí hodnota je 25.</td>
</tr>
<tr>
  <td>AllowNetworkAccess</td>
  <td>Umožňuje přístup k emulátoru přes síť. Je třeba předat také /Key =&lt;key_string&gt; nebo/keyfile =&lt;název_souboru&gt; povolit přístup k síti.</td>
  <td>CosmosDB.Emulator.exe AllowNetworkAccess /Key =&lt;key_string&gt;<br><br>nebo<br><br>/ Keyfile /AllowNetworkAccess CosmosDB.Emulator.exe =&lt;název_souboru&gt;</td>
  <td></td>
</tr>
<tr>
  <td>NoFirewall</td>
  <td>Nemáte úprava pravidla brány firewall, když se používá /AllowNetworkAccess.</td>
  <td>CosmosDB.Emulator.exe /NoFirewall</td>
  <td></td>
</tr>
<tr>
  <td>GenKeyFile</td>
  <td>Vygenerovat nový klíč autorizace a uložte do zadaného souboru. Generovaný klíč lze použít s možností /Key nebo/keyfile.</td>
  <td>CosmosDB.Emulator.exe /GenKeyFile =&lt;cestu k souboru klíče&gt;</td>
  <td></td>
</tr>
<tr>
  <td>Konzistence</td>
  <td>Nastavte výchozí úroveň konzistence pro účet.</td>
  <td>CosmosDB.Emulator.exe /Consistency =&lt;konzistence&gt;</td>
  <td>&lt;konzistence&gt;: hodnota musí být jeden z následujících [úrovně konzistence](consistency-levels.md): relace silného, Eventual nebo BoundedStaleness.  Výchozí hodnota je relace.</td>
</tr>
<tr>
  <td>?</td>
  <td>Zobrazit zprávy nápovědy.</td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-the-azure-cosmos-db-emulator-and-azure-cosmos-db"></a>Rozdíly mezi Azure Cosmos DB emulátoru a Azure Cosmos DB 
Protože emulátor DB Cosmos Azure poskytuje emulované prostředí spuštěna na vývojáře místní pracovní stanici, existují určité rozdíly ve funkcích mezi emulátoru a účet Azure Cosmos DB v cloudu:

* Emulátor DB Cosmos Azure podporuje pouze jeden účet opravené a dobře známé hlavní klíč.  Opětovné generování klíče není možné v emulátoru Azure DB Cosmos.
* Emulátor Azure DB Cosmos není škálovatelné služby a nebude podporovat velké množství kolekcí.
* Emulátor Azure DB Cosmos není simulovat různé [úrovně konzistence Azure Cosmos DB](consistency-levels.md).
* Emulátor Azure DB Cosmos není simulovat [replikace více oblast](distribute-data-globally.md).
* Emulátor DB Cosmos Azure nepodporuje přepsání kvót služby, které jsou k dispozici ve službě Azure Cosmos DB (např. omezení velikosti dokumentu, dělenou kolekci výraznější úložiště).
* Jako vaší kopie emulátoru DB Cosmos Azure nemusí být aktuální. nejnovější změny ve službě Azure Cosmos DB, prosím [Plánovač kapacity Azure Cosmos DB](https://www.documentdb.com/capacityplanner) přesně odhadnout provozním potřebám propustnost (ruština) vaší aplikace.

## <a id="set-partitioncount"></a>Změnit počet kolekcí

Ve výchozím nastavení je možné vytvořit až 25 kolekce tvořené jedním oddílem, nebo 1 dělenou kolekci pomocí emulátoru Azure DB Cosmos. Změnou **PartitionCount** hodnotu, je možné vytvořit až 250 kolekce tvořené jedním oddílem nebo 10 dělené kolekce nebo libovolnou kombinaci dva, které není delší než 250 jeden oddíly (kde 1 oddíly kolekce = 25 kolekce tvořené jedním oddílem).

Pokud se pokusíte vytvořit kolekci po překročení aktuální počet oddílů, emulátoru vyhodí výjimku ServiceUnavailable, s následující zprávou.

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    to bring more and more capacity online, and encourage you to try again. 
    Please do not hesitate to email docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

Chcete-li změnit počet kolekcí, které jsou k dispozici na emulátoru DB Cosmos Azure, postupujte takto:

1. Odstranit všechna místní data emulátoru DB Cosmos Azure kliknutím pravým tlačítkem myši **emulátoru DB Cosmos Azure** ikonu na hlavním panelu a potom kliknutím na **obnovit Data...** .
2. Odstraňte všechna data emulátoru v této složce C:\Users\user_name\AppData\Local\CosmosDBEmulator.
3. Ukončete všechny otevřené instance kliknutím pravým tlačítkem myši **emulátoru DB Cosmos Azure** ikonu na hlavním panelu a potom kliknutím na **ukončení**. Může trvat několik minut pro všechny instance ukončíte.
4. Nainstalujte nejnovější verzi [emulátoru DB Cosmos Azure](https://aka.ms/cosmosdb-emulator).
5. Spusťte emulátor s příznakem PartitionCount nastavením hodnoty < = 250. Například: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="troubleshooting"></a>Řešení potíží

Použijte následující tipy k řešení potíží, které zaznamenáte pomocí emulátoru Cosmos databázi Azure:

- Pokud jste nainstalovali novou verzi emulátoru a dochází k chybám, zajistěte, aby že resetovat vaše data. Pravým tlačítkem myši na ikonu emulátoru DB Cosmos Azure na hlavním panelu a potom kliknutím na Obnovit Data můžete obnovit data... Pokud se chyby nevyřeší, můžete odinstalovat a znovu nainstalovat aplikaci. V tématu [odinstalovat místní emulátoru](#uninstall) pokyny.

- Pokud dojde k chybě v Azure Cosmos DB emulátoru, shromažďovat výpis soubory ze složky c:\Users\user_name\AppData\Local\CrashDumps, zkomprimovat a připojte je k e-mailu na [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).

- Pokud se setkáváte s havárií v CosmosDB.StartupEntryPoint.exe, spusťte následující příkaz z příkazového řádku správce:`lodctr /R` 

- Pokud narazíte na potíže s připojením [shromažďování trasovacích souborů](#trace-files)zkomprimovat a připojte je k e-mailu na [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).

- Pokud se zobrazí **služba není k dispozici** zpráva emulátoru může dojít k selhání inicializace sady síťových protokolů. Zkontrolujte, zda máte Pulse zabezpečené klienta nebo Juniper sítě nainstalovaným klientem, jako jejich ovladače filtru sítě může dojít k potížím. Odinstalace ovladače filtru třetích stran sítě obvykle opravy problému.

### <a id="trace-files"></a>Shromažďování trasovacích souborů

Chcete-li shromažďovat trasování ladění, spusťte následující příkazy z příkazového řádku pro správu:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Sledování na hlavním panelu a ujistěte se program byl vypnut, může trvat několik minut. Můžete také stačí kliknout na **ukončení** v uživatelském rozhraní emulátoru Azure Cosmos DB.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Reprodukujte problém. Pokud Průzkumníku dat nefunguje, stačí čekat na prohlížeči otevřít pro několik sekund, catch k chybě.
5. `CosmosDB.Emulator.exe /stoptraces`
6. Přejděte na `%ProgramFiles%\Azure Cosmos DB Emulator` a najít soubor docdbemulator_000001.etl.
7. Odeslat soubor .etl spolu se nepodařilo kroky [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) pro ladění.

### <a id="uninstall"></a>Odinstalujte místní emulátoru

1. Ukončete všechny otevřené instance místní emulátoru pravým tlačítkem myši na ikonu emulátoru DB Cosmos Azure na hlavním panelu a potom kliknutím na ukončení. Může trvat několik minut pro všechny instance ukončíte.
2. Do vyhledávacího pole Windows, zadejte **aplikace a funkce** a klikněte na **aplikace a funkce (nastavení systému)** výsledek.
3. V seznamu aplikací, posuňte **emulátoru DB Cosmos Azure**, vyberte ho, klikněte na **odinstalace**, potvrďte a klikněte na **odinstalovat** znovu.
4. Při odinstalaci aplikace, přejděte na C:\Users\<uživatele > \AppData\Local\CosmosDBEmulator a odstranit složku. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Nainstalována na místní emulátoru
> * Rand – emulátoru na Docker pro Windows
> * Ověřené žádosti
> * Použít Průzkumníku dat v emulátoru
> * Exportovaný certifikáty SSL
> * Volat emulátoru z příkazového řádku
> * Shromážděné trasovací soubory

V tomto kurzu jste zjistili, jak používat místní emulátor pro volné místní vývoj. Teď můžete pokračovat v dalším kurzu a zjistěte, jak k exportu certifikátů SSL emulátor. 

> [!div class="nextstepaction"]
> [Exportu certifikátů emulátoru DB Cosmos Azure](local-emulator-export-ssl-certificates.md)
