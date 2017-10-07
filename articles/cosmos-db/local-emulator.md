---
title: "aaaDevelop místně s hello emulátoru DB Cosmos Azure | Microsoft Docs"
description: "Pomocí hello emulátoru DB Cosmos Azure, můžete vývoj a testování aplikace místně pro uvolnění bez vytváření předplatného Azure."
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
ms.openlocfilehash: fb5449489e5f71664e72d8e11e583315be371bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cosmos-db-emulator-for-local-development-and-testing"></a>Použití hello emulátoru DB Cosmos Azure pro místní vývoj a testování

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
  
Hello emulátoru DB Cosmos Azure poskytuje místní prostředí, které emuluje hello služby Azure Cosmos DB pro účely vývoje. Hello emulátoru DB Cosmos Azure můžete vyvíjet a testovat svou aplikaci lokálně, bez vytváření předplatného Azure nebo nákladům. Až budete spokojeni s jak aplikace funguje v hello emulátoru DB Cosmos Azure, můžete přepnout toousing účet Azure Cosmos DB v cloudu hello.

Tento článek se zabývá hello následující úlohy: 

> [!div class="checklist"]
> * Instalace hello emulátoru
> * Systémem Docker pro Windows hello emulátoru
> * Ověřování požadavků
> * Pomocí Průzkumníku dat hello v emulátoru hello
> * Export certifikáty SSL
> * Volání hello emulátoru z příkazového řádku hello
> * Shromažďování trasovacích souborů

Doporučujeme začít sledování hello následující video, kde Kirill Gavrylyuk ukazuje, jak tooget pracovat s hello emulátoru DB Cosmos Azure. Všimněte si, že hello video odkazuje toohello emulátoru jako hello DocumentDB emulátoru, ale byla přejmenována hello nástroj sám sebe hello emulátoru DB Cosmos Azure od zaznamenávat hello video. Všechny informace v hello video je stále správné pro hello emulátoru DB Cosmos Azure. 

> [!VIDEO https://channel9.msdn.com/Events/Connect/2016/192/player]
> 
> 

## <a name="how-hello-emulator-works"></a>Jak funguje hello emulátoru
Hello emulátoru DB Cosmos Azure poskytuje zachováním emulace Dobrý den služby Azure Cosmos DB. Podporuje stejné funkce jako Azure Cosmos databáze, včetně podpory pro vytváření a dotazování dokumentů JSON, zřizování a škálování kolekce a provádění uložené procedury a triggery. Můžete vyvíjet a testovat aplikace pomocí hello emulátoru DB Cosmos Azure a nasadit je tooAzure v globálním měřítku tím, že právě jednu konfiguraci změnit toohello koncového bodu připojení pro Azure Cosmos DB.

Když jsme vytvořili místní emulace zachováním hello skutečné Azure Cosmos DB služby, se liší od služby hello hello implementace hello emulátoru DB Cosmos Azure. Například hello emulátoru DB Cosmos Azure používá standardní součásti operačního systému, například hello místního systému souborů pro trvalosti a zásobník protokolu HTTPS pro připojení k síti. To znamená, že některé funkce, které jsou závislé na infrastrukturu Azure jako globální replikace, jednociferné milisekundu latence pro čtení/zápisu a přizpůsobitelné úrovně konzistence nejsou k dispozici prostřednictvím hello emulátoru DB Cosmos Azure.

> [!NOTE]
> V tento čas hello Průzkumníku dat v hello emulátoru podporuje pouze hello vytvoření kolekce DocumentDB rozhraní API a kolekcí MongoDB. Hello Průzkumníku dat v emulátoru hello v současné době nepodporuje vytvoření hello tabulky a grafy. 

## <a name="system-requirements"></a>Požadavky na systém
Hello emulátoru DB Cosmos Azure má hello následující hardwarové a softwarové požadavky:

* Požadavky na software
  * Windows Server 2012 R2, Windows Server 2016 nebo Windows 10
*   Minimální požadavky na Hardware
  * 2 GB PAMĚTI RAM
  * 10 GB volného místa na disku

## <a name="installation"></a>Instalace
Můžete stáhnout a nainstalovat hello emulátoru DB Cosmos Azure z hello [Microsoft Download Center](https://aka.ms/cosmosdb-emulator). 

> [!NOTE]
> tooinstall, konfiguraci a spuštění hello Azure Cosmos DB emulátoru, musíte mít oprávnění správce na počítači hello.

## <a name="running-on-docker-for-windows"></a>Systémem Docker pro Windows

Hello emulátoru DB Cosmos Azure můžete spustit na Docker pro systém Windows. Hello emulátoru na Docker pro Oracle Linux nefunguje.

Jakmile máte [Docker pro systém Windows](https://www.docker.com/docker-windows) nainstalován, můžete můžete načítat bitová kopie emulátoru hello z úložiště Docker Hub tak, že spustíte následující příkaz z své oblíbené prostředí hello (cmd.exe, prostředí PowerShell, atd.).

```      
docker pull microsoft/azure-cosmosdb-emulator 
```
toostart hello bitovou kopii, spusťte následující příkazy hello.

``` 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>nul
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:c:\CosmosDBEmulator\CosmosDBEmulatorCert -P -t -i microsoft/azure-cosmosdb-emulator 
```

Hello odpověď bude vypadat podobně jako toohello následující:

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import hello SSL certificate from an administrator command prompt on hello host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

Zavřením hello interaktivní prostředí po hello spuštění emulátoru bude vypnutí hello emulátoru kontejnerů.

Použijte hello koncový bod a hlavního klíče v odpovědi hello v vašeho klienta a importovat certifikát SSL hello do svého hostitele. tooimport hello certifikát SSL hello následující z příkazového řádku správce:

```
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```


## <a name="start-hello-emulator"></a>Spusťte emulátor hello

toostart hello emulátoru DB Cosmos Azure, vyberte tlačítko Start hello nebo stiskněte klávesu Windows hello. Začněte psát **emulátoru DB Cosmos Azure**a vyberte hello emulátoru hello seznamu aplikací. 

![Vyberte hello Start tlačítka nebo klikněte na tlačítko hello Windows klíč, začněte psát ** Azure Cosmos DB emulátoru ** a vyberte hello emulátor ze hello seznam aplikací](./media/local-emulator/database-local-emulator-start.png)

Když je spuštěný emulátor hello, uvidíte na ikonu v oznamovací oblasti hlavního panelu Windows hello. ![Azure Cosmos DB místní emulátoru panelu oznámení](./media/local-emulator/database-local-emulator-taskbar.png)

Hello emulátoru DB Cosmos Azure ve výchozím nastavení spouští v hello místním počítači ("localhost") naslouchá na portu 8081.

Hello emulátoru DB Cosmos Azure je nainstalována ve výchozím nastavení toohello `C:\Program Files\Azure Cosmos DB Emulator` adresáře. Můžete také spustit a zastavit hello emulátor ze hello příkazového řádku. V tématu [odkaz na nástroj příkazového řádku](#command-line) Další informace.

## <a name="start-data-explorer"></a>Spuštění Průzkumníka dat

Při spuštění hello Azure Cosmos DB emulátoru se automaticky otevře hello Průzkumníku dat DB Cosmos Azure v prohlížeči. Hello adresa se zobrazí jako [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html). Pokud jste zavřete hello Průzkumníka a chcete toore otevřete ho později, můžete otevřít hello adresu URL v prohlížeči nebo spustit z hello emulátoru DB Cosmos Azure v hello ikonu na hlavním panelu systému Windows, jak je uvedeno níže.

![Azure Cosmos DB místní emulátoru data Průzkumníka Spouštěče](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Kontrola aktualizací
Průzkumník dat určuje, zda je k dispozici ke stažení nové aktualizace. 

> [!NOTE]
> Data vytvořená ve verzi hello emulátoru DB Azure Cosmos není zaručena toobe dostupné při použití jiné verze. Pokud budete potřebovat toopersist data pro dlouhodobé hello, doporučuje se uložit data v Azure Cosmos DB účet, nikoli hello emulátoru DB Cosmos Azure. 

## <a name="authenticating-requests"></a>Ověřování požadavků
Stejně jako s Azure DB Cosmos v cloudu hello každého požadavku, které vytváříte, proti hello emulátoru DB Cosmos Azure musí ověřit. Hello Azure Cosmos DB emulátoru podporuje jeden pevný účet a dobře známé ověřovací klíč pro ověřování hlavního klíče. Tento účet a klíč jsou pouze povolené pro použití s hello emulátoru DB Cosmos Azure přihlašovací údaje v hello. Jsou:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> hlavní klíč Hello nepodporuje hello emulátoru DB Cosmos Azure je určena pro použití pouze pomocí emulátoru hello. Váš účet Azure Cosmos DB produkční a klíč nelze použít s hello emulátoru DB Cosmos Azure. 

> [!NOTE] 
> Pokud jste spustili hello emulátoru s hello /Key možnost, pak pomocí klíče generované hello místo "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="

Navíc, podobně jako hello služby Azure Cosmos DB, hello Azure Cosmos DB emulátoru podporuje pouze zabezpečenou komunikaci prostřednictvím protokolu SSL.

## <a name="running-hello-emulator-on-a-local-network"></a>Spuštěného emulátoru hello v místní síti

Emulátor hello můžete spustit v místní síti. tooenable přístup k síti, zadejte možnost /AllowNetworkAccess hello v hello [příkazového řádku](#command-line-syntax), což také vyžaduje, že zadáváte /Key = key_string nebo/keyfile = název_souboru. Můžete použít /GenKeyFile = název_souboru toogenerate soubor s předem náhodný klíč.  Pak můžete předat tuto příliš/KeyFile = název_souboru nebo /Key = contents_of_file.

přístup k síti tooenable pro hello hello nový uživatel by měl emulátoru hello vypnutí a odstranit adresář dat hello emulátoru (C:\Users\user_name\AppData\Local\CosmosDBEmulator).

## <a name="developing-with-hello-emulator"></a>Vývoj s hello emulátoru
Jakmile máte hello Azure Cosmos DB emulátoru spuštěna na pracovní ploše, můžete použít libovolnou podporované [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) nebo hello [REST API služby Azure Cosmos DB](/rest/api/documentdb/) toointeract s hello emulátor. Hello emulátoru DB Cosmos Azure také zahrnuje integrovanou Průzkumníku dat, která umožňuje vytvářet kolekce pro hello DocumentDB a rozhraní API MongoDB a zobrazení a úpravám dokumentů bez psaní jakéhokoli kódu.   

    // Connect toohello Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

Pokud používáte [Azure Cosmos DB podporou protokolů pro MongoDB](mongodb-introduction.md), použijte prosím hello následující připojovací řetězec:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true&3t.sslSelfSignedCerts=true

Můžete použít stávající nástroje, například [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) tooconnect toohello emulátoru DB Cosmos Azure. Můžete také migrovat data mezi hello emulátoru DB Cosmos Azure a službou Azure Cosmos DB hello pomocí hello [nástroj pro migraci dat Azure Cosmos DB](https://github.com/azure/azure-documentdb-datamigrationtool).

> [!NOTE] 
> Pokud jste spustili hello emulátoru s hello /Key možnost, pak pomocí klíče generované hello místo "C2y6yDjf5/R + ob0N8A7Cgv30VRDJIWEHLM + 4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw =="

Pomocí emulátoru hello Azure Cosmos DB, ve výchozím nastavení, můžete vytvořit kolekce tvořené jedním oddílem too25 nebo 1 dělenou kolekci. Další informace o změně tuto hodnotu najdete v tématu [nastavení hodnoty PartitionCount hello](#set-partitioncount).

## <a name="export-hello-ssl-certificate"></a>Exportovat certifikát SSL hello

Jazyky rozhraní .NET a toosecurely úložiště certifikátů Windows hello použití modulu runtime připojit místní emulátoru toohello Azure Cosmos DB. Další jazyky mít vlastní metodu správy a použití certifikátů. Java používá vlastní [úložiště certifikátů](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) zatímco používá Python [soketu obálky](https://docs.python.org/2/library/ssl.html).

V pořadí tooobtain toouse certifikát s jazyky a moduly runtime, který nelze integrovat do úložiště certifikátů Windows hello je budou potřebovat tooexport pomocí Správce certifikátů Windows hello. Můžete spusťte ji spuštěním certlm.msc nebo postupujte podle pokynů hello krok za krokem v [exportu hello Azure Cosmos DB emulátoru certifikátů](./local-emulator-export-ssl-certificates.md). Jakmile správce certifikátů hello pracuje, otevřete hello osobní certifikáty, jak je uvedeno níže a export hello certifikát s popisným názvem hello "DocumentDBEmulatorCertificate" jako kódováním BASE-64 souboru X.509 (.cer).

![Certifikát SSL místní emulátoru služby Azure Cosmos DB](./media/local-emulator/database-local-emulator-ssl_certificate.png)

certifikát X.509 Hello lze importovat do úložiště certifikátů Java hello podle následujících pokynů hello v [přidání certifikátu toohello, úložiště certifikátů certifikační Autority Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store). Jakmile je hello certifikát importován do úložiště certifikátů hello, aplikací Java a MongoDB bude mít tooconnect toohello emulátoru DB Cosmos Azure.

Při připojování toohello emulátoru z Pythonu a Node.js SDK, je zakázáno ověřování SSL.

## <a id="command-line"></a>Odkaz na nástroj příkazového řádku
Z umístění instalace hello můžete pomocí příkazového řádku toostart hello a zastavit hello emulátoru, nakonfigurujte možnosti a provádění dalších operací.

### <a name="command-line-syntax"></a>Syntaxe příkazového řádku

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

tooview hello seznam možností, typ `CosmosDB.Emulator.exe /?` hello příkazového řádku.

<table>
<tr>
  <td><strong>Možnost</strong></td>
  <td><strong>Popis</strong></td>
  <td><strong>Příkaz</strong></td>
  <td><strong>Argumenty</strong></td>
</tr>
<tr>
  <td>[Žádný argument]</td>
  <td>Spuštění hello emulátoru DB Cosmos Azure s výchozím nastavením.</td>
  <td>CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>[Help]</td>
  <td>Zobrazí seznam hello podporované argumenty příkazového řádku.</td>
  <td>CosmosDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>Vypnutí</td>
  <td>Vypne hello emulátoru DB Cosmos Azure.</td>
  <td>/ CosmosDB.Emulator.exe Shutdown</td>
  <td></td>
</tr>
<tr>
  <td>DataPath</td>
  <td>Určuje cestu hello v které toostore datové soubory. Výchozí hodnota je % LocalAppdata%\CosmosDBEmulator.</td>
  <td>CosmosDB.Emulator.exe /DataPath =&lt;datapath&gt;</td>
  <td>&lt;DataPath&gt;: přístupná cesta</td>
</tr>
<tr>
  <td>Port</td>
  <td>Určuje číslo toouse hello port pro emulátor hello.  Výchozí hodnota je 8081.</td>
  <td>/ CosmosDB.Emulator.exe port =&lt;portu&gt;</td>
  <td>&lt;port&gt;: jedno číslo portu</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>Určuje hello toouse číslo portu pro MongoDB kompatibility rozhraní API. Výchozí hodnota je 10255.</td>
  <td>CosmosDB.Emulator.exe /MongoPort =&lt;mongoport&gt;</td>
  <td>&lt;mongoport&gt;: jedno číslo portu</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Určuje hello toouse porty pro přímé připojení k síti. Výchozí hodnoty jsou 10251,10252,10253,10254.</td>
  <td>CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</td>
  <td>&lt;directports&gt;: seznam s položkami oddělenými čárkou 4 porty</td>
</tr>
<tr>
  <td>Klíč</td>
  <td>Autorizační klíč pro emulátor hello. Klíč musí být hello kódování base-64 vektoru 64 bajtů.</td>
  <td>CosmosDB.Emulator.exe /Key:&lt;klíč&gt;</td>
  <td>&lt;klíč&gt;: klíč musí být hello kódování base-64 vektoru 64 bajtů</td>
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
  <td>Nezobrazovat hello emulátoru uživatelské rozhraní.</td>
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
  <td>Určuje maximální počet dělené kolekce hello. V tématu [změnit hello počet kolekcí](#set-partitioncount) Další informace.</td>
  <td>CosmosDB.Emulator.exe /PartitionCount =&lt;partitioncount&gt;</td>
  <td>&lt;partitioncount&gt;: maximální počet povolených kolekce tvořené jedním oddílem. Výchozí hodnota je 25. Maximální povolený počet je 250.</td>
</tr>
<tr>
  <td>DefaultPartitionCount</td>
  <td>Určuje hello výchozí počet oddílů pro dělenou kolekci.</td>
  <td>CosmosDB.Emulator.exe /DefaultPartitionCount =&lt;defaultpartitioncount&gt;</td>
  <td>&lt;defaultpartitioncount&gt; výchozí hodnota je 25.</td>
</tr>
<tr>
  <td>AllowNetworkAccess</td>
  <td>Umožňuje přístup k emulátoru toohello přes síť. Je třeba předat také /Key =&lt;key_string&gt; nebo/keyfile =&lt;název_souboru&gt; tooenable přístup k síti.</td>
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
  <td>Vygenerovat nový klíč autorizace a uložte toohello zadaný soubor. Hello vygenerovat klíč můžete používat s hello /Key nebo možnosti/keyfile.</td>
  <td>CosmosDB.Emulator.exe /GenKeyFile =&lt;tookey cestě k souboru&gt;</td>
  <td></td>
</tr>
<tr>
  <td>Konzistence</td>
  <td>Nastaví úroveň konzistence výchozí hello hello účtu.</td>
  <td>CosmosDB.Emulator.exe /Consistency =&lt;konzistence&gt;</td>
  <td>&lt;konzistence&gt;: hodnota musí být jedna z následujících hello [úrovně konzistence](consistency-levels.md): relace silného, Eventual nebo BoundedStaleness.  Hello výchozí hodnota je relace.</td>
</tr>
<tr>
  <td>?</td>
  <td>Zobrazit zprávu nápovědy hello.</td>
  <td></td>
  <td></td>
</tr>
</table>

## <a name="differences-between-hello-azure-cosmos-db-emulator-and-azure-cosmos-db"></a>Rozdíly mezi hello emulátoru DB Cosmos Azure a Azure Cosmos DB 
Protože hello emulátoru DB Cosmos Azure poskytuje emulované prostředí spuštěna na vývojáře místní pracovní stanici, existují určité rozdíly ve funkcích mezi hello emulátoru a účet Azure Cosmos DB v cloudu hello:

* Hello Azure Cosmos DB emulátoru podporuje pouze jeden účet opravené a dobře známé hlavní klíč.  Opětovné generování klíče není možné v hello emulátoru DB Cosmos Azure.
* Hello emulátoru DB Azure Cosmos není škálovatelné služby a nebude podporovat velké množství kolekcí.
* Hello emulátoru DB Azure Cosmos není simulovat různé [úrovně konzistence Azure Cosmos DB](consistency-levels.md).
* Hello emulátoru DB Azure Cosmos není simulovat [replikace více oblast](distribute-data-globally.md).
* Hello emulátoru DB Cosmos Azure nepodporuje přepsání hello služby kvóty, které jsou k dispozici ve službě Azure Cosmos DB hello (např. omezení velikosti dokumentu, dělenou kolekci výraznější úložiště).
* Jako vaší kopie aplikace hello emulátoru DB Cosmos Azure nemusí být až toodate s hello poslední změny se službou Azure Cosmos DB hello, prosím [Plánovač kapacity Azure Cosmos DB](https://www.documentdb.com/capacityplanner) tooaccurately odhad produkční propustnost (ruština) potřebám vaší aplikace.

## <a id="set-partitioncount"></a>Změnit hello počet kolekcí

Ve výchozím nastavení můžete vytvořit kolekce tvořené jedním oddílem too25 nebo 1 dělenou kolekci pomocí hello emulátoru DB Cosmos Azure. Změnou hello **PartitionCount** hodnotu, můžete vytvořit kolekce tvořené jedním oddílem too250 nebo 10 dělené kolekce nebo libovolnou kombinaci hello dva, které není delší než 250 jedním oddíly (kde 1 rozdělena na oddíly kolekce = 25 kolekce tvořené jedním oddílem).

Když zkusíte toocreate kolekci po překročení hello aktuální počet oddílů, hello emulátoru vyhodí výjimku ServiceUnavailable, s následující zprávou hello.

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    toobring more and more capacity online, and encourage you tootry again. 
    Please do not hesitate tooemail docdbswat@microsoft.com at any time or 
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

toochange hello počet kolekcí k dispozici toohello Azure Cosmos DB emulátoru, hello následující:

1. Odstranit všechna místní data emulátoru DB Cosmos Azure kliknutím pravým tlačítkem na hello **emulátoru DB Cosmos Azure** ikonu na hlavním panelu systému hello a klepnutím **obnovit Data...** .
2. Odstraňte všechna data emulátoru v této složce C:\Users\user_name\AppData\Local\CosmosDBEmulator.
3. Ukončete všechny otevřené instance kliknutím pravým tlačítkem na hello **emulátoru DB Cosmos Azure** ikonu na panelu systému hello a pak levým na **ukončení**. Může trvat několik minut pro všechny instance tooexit.
4. Nainstalujte nejnovější verzi hello hello [emulátoru DB Cosmos Azure](https://aka.ms/cosmosdb-emulator).
5. Spusťte emulátor hello s hello PartitionCount příznak nastavením hodnoty < = 250. Například: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="troubleshooting"></a>Řešení potíží

Použijte následující tipy toohelp řešení problémů, které zaznamenáte pomocí emulátoru Azure Cosmos DB hello hello:

- Pokud nainstaluje novou verzi hello emulátoru a dochází k chybám, ověřte, že resetovat vaše data. Pravým tlačítkem myši na ikonu emulátoru DB Cosmos Azure hello na hlavním panelu systému hello a potom kliknutím na Obnovit Data můžete obnovit data... Pokud se chyby hello nevyřeší, můžete odinstalovat a znovu nainstalujte aplikaci hello. V tématu [odinstalovat místní emulátoru hello](#uninstall) pokyny.

- Pokud dojde k chybě hello Azure Cosmos DB emulátoru, shromažďovat výpis soubory ze složky c:\Users\user_name\AppData\Local\CrashDumps, zkomprimovat a připojte je příliš e-mailu tooan[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Pokud se setkáváte s havárií v CosmosDB.StartupEntryPoint.exe, spusťte následující příkaz z příkazového řádku správce hello:`lodctr /R` 

- Pokud narazíte na potíže s připojením [shromažďování trasovacích souborů](#trace-files)zkomprimovat a připojte je příliš tooan e-mailu[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Pokud se zobrazí **služba není k dispozici** zprávy, hello emulátoru může dojít k selhání tooinitialize hello síťových protokolů. Pokud máte hello Pulse zabezpečené klienta nebo Juniper sítě nainstalovaným klientem, jako jejich ovladače filtru sítě může způsobit hello problém, zkontrolujte toosee. Odinstalace ovladače filtru třetích stran sítě obvykle řeší problém, hello.

### <a id="trace-files"></a>Shromažďování trasovacích souborů

toocollect ladění trasování, spusťte následující příkazy z příkazového řádku pro správu hello:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Kukátko hello systému panelu toomake zda hello program byl vypnut, může trvat několik minut. Můžete také stačí kliknout na **ukončení** hello Azure Cosmos DB emulátoru uživatelské rozhraní.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Reprodukujte problém hello. Pokud Průzkumníku dat nefunguje, potřebujete jenom toowait pro tooopen prohlížeče hello pár sekund toocatch hello došlo k chybě.
5. `CosmosDB.Emulator.exe /stoptraces`
6. Přejděte příliš`%ProgramFiles%\Azure Cosmos DB Emulator` a najít soubor docdbemulator_000001.etl hello.
7. Odeslat soubor .etl hello návodem zkopírujte příliš[ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) pro ladění.

### <a id="uninstall"></a>Odinstalujte hello místní emulátoru

1. Ukončete všechny otevřené instance hello místní emulátoru pravým tlačítkem myši na ikonu emulátoru DB Cosmos Azure hello na hlavním panelu systému hello, a potom kliknutím na ukončení. Může trvat několik minut pro všechny instance tooexit.
2. Hello Windows vyhledávacího pole zadejte **aplikace a funkce** a klikněte na hello **aplikace a funkce (nastavení systému)** výsledek.
3. V hello seznam aplikací, posuňte příliš**emulátoru DB Cosmos Azure**, vyberte ho, klikněte na **odinstalace**, potvrďte a klikněte na **odinstalovat** znovu.
4. Při odinstalaci aplikace hello přejděte tooC:\Users\<uživatele > \AppData\Local\CosmosDBEmulator, odstraňte složku hello. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Nainstalovat hello místní emulátoru
> * Rand – hello emulátoru na Docker pro Windows
> * Ověřené žádosti
> * Použít hello Průzkumníku dat v emulátoru hello
> * Exportovaný certifikáty SSL
> * Volat hello emulátoru z příkazového řádku hello
> * Shromážděné trasovací soubory

V tomto kurzu naučili jak toouse hello místní emulátor pro volné místní vývoj. Teď můžete pokračovat dalším kurzu toohello a zjistěte, jak certifikáty SSL emulátoru tooexport. 

> [!div class="nextstepaction"]
> [Exportu certifikátů emulátoru DB Cosmos Azure hello](local-emulator-export-ssl-certificates.md)
