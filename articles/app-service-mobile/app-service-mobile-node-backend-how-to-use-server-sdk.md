---
title: aaaHow toowork se serverem back-end Node.js hello SDK pro Mobile Apps | Microsoft Docs
description: "Zjistěte, jak toowork s hello serveru back-end Node.js SDK pro Azure App Service Mobile Apps."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2b1ea5fda6f6ca422b92fe29ff8d16bf035018d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-nodejs-sdk"></a>Jak toouse hello Azure Mobile Apps Node.js SDK
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Tento článek obsahuje podrobné informace a příklady zobrazující jak toowork s back-end Node.js v Azure App Service Mobile Apps.

## <a name="Introduction"></a>Úvod
Azure App Service Mobile Apps poskytuje hello schopností tooadd mobile optimalizovaná data přístup webového rozhraní API tooa webové aplikace.  Hello Azure App Service Mobile Apps SDK se poskytuje pro webové aplikace ASP.NET a Node.js.  Hello SDK poskytuje hello následující operace:

* Operace s tabulkou (pro čtení, příkaz Insert, Update, Delete) pro přístup k datům
* Operace vlastní rozhraní API

Obě operace zajištění ověřování napříč všech zprostředkovatelů identity povolené ve službě Azure App Service, včetně poskytovatelů sociálních identit jako je Facebook, Twitter, Google a Microsoft a také Azure Active Directory pro podnikové identity.

Můžete najít ukázky pro každý případ použití v hello [ukázky adresáře na Githubu].

## <a name="supported-platforms"></a>Podporované platformy
Hello Azure Mobile Apps uzlu SDK podporuje hello LTS aktuální verzi uzlu a později.  Od verze zápis, nejnovější verze LTS hello je v4.5.0 uzlu.  Jiné verze uzlu může fungovat, ale nejsou podporovány.

Hello Azure Mobile Apps uzlu SDK podporuje dvě databáze ovladače – hello uzlu mssql ovladač podporuje SQL Azure a místní instance systému SQL Server.  ovladač sqlite3 Hello podporuje pouze do jedné instance databáze SQLite.

### <a name="howto-cmdline-basicapp"></a>Postupy: vytvoření základní Node.js back-end pomocí hello příkazového řádku
Všechny back-end Azure App Service Mobile aplikace Node.js se spustí jako ExpressJS aplikace.  ExpressJS je hello nejoblíbenější webové služby rozhraní k dispozici pro Node.js.  Můžete vytvořit základní [Express] aplikace následujícím způsobem:

1. V příkazu nebo v okně prostředí PowerShell vytvořte adresář pro váš projekt.

        mkdir basicapp
2. Spusťte struktury balíčku npm init tooinitialize hello.

        cd basicapp
        npm init

    příkaz init npm Hello požádá sadu otázky tooinitialize hello projektu.  V tématu hello příklad výstupu:

    ![výstup init npm Hello][0]
3. Nainstalujte hello express a aplikace azure mobile knihovny z úložiště npm hello.

        npm install --save express azure-mobile-apps
4. Vytvoření app.js souboru tooimplement hello základní mobilní serveru.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Tato aplikace vytvoří WebAPI se mobile optimalizované s jeden koncový bod (`/tables/TodoItem`), který poskytuje přístup bez ověřování tooan základní úložiště dat SQL pomocí dynamické schématu.  Je vhodné pro následující rychlé spuštění knihovny klienta:

* [Rychlý start Android klienta]
* [Rychlý start Apache Cordova klienta]
* [iOS klienta rychlý start]
* [Rychlé spuštění klienta Windows Store]
* [Rychlý start Xamarin.iOS klienta]
* [Rychlé spuštění klienta Xamarin.Android]
* [Rychlý start Xamarin.Forms klienta]

Můžete najít hello kód pro tuto základní aplikaci v hello [basicapp ukázce na Githubu].

### <a name="howto-vs2015-basicapp"></a>Postupy: vytvoření back-end uzlu pomocí sady Visual Studio 2015
Visual Studio 2015 vyžaduje rozšíření toodevelop Node.js aplikací v rámci hello IDE.  toostart, instalace hello [Node.js Tools 1.1 pro sadu Visual Studio].  Po instalaci nástrojů pro Node.js hello pro sadu Visual Studio vytvořte aplikaci 4.x Express:

1. Otevřete hello **nový projekt** dialogové okno (z **soubor** > **nový** > **projekt...** ).
2. Rozbalte položku **šablony** > **JavaScript** > **Node.js**.
3. Vyberte hello **základní Azure Node.js Express 4 aplikační**.
4. Zadejte název projektu hello.  Klikněte na tlačítko *OK*.

    ![Nový projekt sady Visual Studio 2015][1]
5. Klikněte pravým tlačítkem na hello **npm** uzel a vyberte možnost **nainstalovat nové balíčky npm...** .
6. Může být nutné toorefresh hello npm katalogu na vytvoření vaší první aplikace Node.js.  Klikněte na tlačítko **aktualizovat** v případě potřeby.
7. Zadejte *aplikace azure mobile* hello vyhledávacího pole.  Klikněte na tlačítko hello **azure mobile apps 2.0.0** balíček a potom klikněte na **instalovat balíček**.

    ![Instalace nové balíčky npm][2]
8. Klikněte na **Zavřít**.
9. Otevřete hello *app.js* tooadd podpora pro Azure Mobile Apps SDK hello souborů.  V řádku 6 at hello dolní hello knihovně vyžadují příkazy, přidejte následující kód hello:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Na přibližně řádku 27 po hello přidat další příkazy app.use hello následující kód:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Uložte soubor hello.
10. Spusťte místně aplikace hello (hello rozhraní API je vyhovovat na http://localhost: 3000) nebo publikování tooAzure.

### <a name="create-node-backend-portal"></a>Postupy: vytvoření back-end Node.js, pomocí hello portálu Azure
Můžete vytvořit právo back-end mobilní aplikace hello [portál Azure]. Můžete buď provést hello následující kroky nebo vytvořte klienta a serveru společně následující hello [vytvoření mobilní aplikace](app-service-mobile-ios-get-started.md) kurzu. kurz Hello obsahuje zjednodušenou verzi tyto pokyny a je nejvhodnější pro ověření projekty na konceptu.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Zpět v hello *Začínáme* okno, v části **vytvořit tabulku rozhraní API**, zvolte **Node.js** jako vaše **back-end jazyk**.
Zaškrtněte políčko hello pro "**beru na vědomí, že tato akce přepíše všechny lokality obsahu.**", pak klikněte na tlačítko **vytvořit tabulku TodoItem**.

### <a name="download-quickstart"></a>Postupy: stažení hello Node.js back-end rychlé spuštění kódu projektu pomocí Git
Při vytváření back-end mobilní aplikace Node.js pomocí portálu hello **úvodní** okně projekt Node.js se vytvoří a nasazené tooyour lokality. Můžete přidat tabulky a rozhraní API a upravit soubory kódu pro back-end Node.js hello hello portálu. Můžete také použít různé nasazení nástroje toodownload hello back-end projektu, takže můžete přidat nebo upravit tabulek a rozhraní API a pak znovu publikovat hello projektu. Další informace najdete v tématu [Příručka pro nasazení Azure App Service]. Hello následující postup používá kód Git úložiště toodownload hello rychlé spuštění projektu.

1. Pokud jste tak již neučinili, nainstalujte Git. Hello kroky požadované tooinstall Git lišit podle operačních systémů. V tématu [instalace Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) distribuce specifické pro operační systém a instalaci.
2. Postupujte podle kroků hello v [povolit hello úložiště aplikace služby App Service](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello úložiště Git pro vaši lokalitu back-end, provádění poznamenejte si hello nasazení uživatelské jméno a heslo.
3. V okně hello back-endu mobilní aplikace, poznamenejte si hello **adresy URL pro klon Git** nastavení.
4. Spuštění hello `git clone` příkaz pomocí hello Git adresa URL klonování, zadat heslo, pokud jsou povinné, jako v následujícím příkladu:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Byly staženy procházet toolocal adresář, který v předchozím příkladu hello je /todolist a Všimněte si, že soubory projektu. Vyhledejte hello `todoitem.json` souboru v hello `/tables` adresáře.  Tento soubor definuje oprávnění v tabulce.  Také umožňuje vyhledat hello `todoitem.js` souboru v hello stejný adresář, který definuje, že operace CRUD skriptů pro tabulku hello.
6. Po provedení změn tooproject soubory, spusťte následující příkazy tooadd, potvrzení a pak nahrajte webu toohello změny hello:

        $ git commit -m "updated hello table script"
        $ git push origin master

    Když přidáte nový projekt toohello soubory, musíte nejprve tooexecute hello `git add .` příkaz.

Hello lokality znovu publikován pokaždé, když je novou sadu potvrzení nabídnutých toohello lokality.

### <a name="howto-publish-to-azure"></a>Postupy: publikování vašeho tooAzure back-end Node.js
Microsoft Azure poskytuje mnoho mechanismy pro publikování váš back-end Azure App Service Mobile aplikace Node.js hello služby Azure.  Jedná se o použití nástroje pro nasazení, které jsou integrované do sady Visual Studio, nástroje příkazového řádku a průběžné nasazování pro různé zdrojového kódu.  Další informace v tomto tématu najdete v tématu [Příručka pro nasazení Azure App Service].

Aplikační služba Azure má konkrétní Rady, jak aplikaci Node.js, která si měli projít před nasazením:

* Jak příliš[zadejte hello uzlu verze]
* Jak příliš[použijte moduly uzlu]

### <a name="howto-enable-homepage"></a>Postupy: povolení domovskou stránku pro aplikaci
Mnoho aplikací jsou kombinací webových a mobilních aplikací a hello ExpressJS framework vám umožní toocombine dva omezující vlastnosti.  V některých případech ale můžete tooonly implementace mobilní rozhraní.  Je užitečné tooprovide cílová stránka tooensure hello aplikační služby je spuštěná.  Můžete zadat vlastní domovské stránky, nebo povolit dočasné domovskou stránku.  tooenable dočasné domovskou stránku, použijte následující tooinstantiate Azure Mobile Apps hello:

    var mobile = azureMobileApps({ homePage: true });

Pokud chcete pouze tuto možnost k dispozici při vývoji místně, můžete přidat tato nastavení tooyour `azureMobile.js` souboru.

## <a name="TableOperations"></a>Operace s tabulkou
Hello aplikace azure mobile Node.js Server SDK poskytuje mechanismy tooexpose data tabulek uložených v databázi SQL Azure jako WebAPI.  Jsou k dispozici pět operace.

| Operace | Popis |
| --- | --- |
| GET /tables/*tablename* |Získat všechny záznamy v tabulce hello |
| GET /tables/*tablename*/:id |Získejte konkrétní záznam v tabulce hello |
| POST /tables/*tablename* |Vytvořit záznam v tabulce hello |
| Oprava /tables/*tablename*/:id |Aktualizace záznamu v tabulce hello |
| ODSTRANĚNÍ /tables/*tablename*/:id |Odstranit záznam v tabulce hello |

Podporuje tato WebAPI [OData] a rozšiřuje hello tabulky schématu toosupport [synchronizaci dat offline].

### <a name="howto-dynamicschema"></a>Postupy: definování tabulky pomocí dynamické schématu
Před použitím tabulky, musí být definovaný.  Tabulky lze definovat pomocí statické schématu (kde hello vývojáře definuje hello sloupců ve schématu hello) nebo dynamicky (kde hello SDK řídí hello schéma založené na příchozí požadavky). Kromě toho můžete řídit hello vývojáře konkrétních aspektů hello WebAPI přidáním definice toohello kódu Javascript.

Jako osvědčený postup musí definovat každá tabulka v souboru jazyka Javascript v adresáři hello tabulky, potom použijte tables.import() metoda tooimport hello tabulky.  Rozšíření hello basic – aplikace, souboru app.js hello se upraví:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define hello database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definovat hello tabulky ve. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for hello table goes here

    module.exports = table;

Tabulky ve výchozím nastavení používá dynamické schématu.  tooturn vypnout dynamické schématu globálně, nastavte hello nastavení aplikace **MS_DynamicSchema** toofalse v rámci hello portálu Azure.

Úplný příklad můžete najít v hello [todo ukázce na Githubu].

### <a name="howto-staticschema"></a>Postupy: definování tabulky pomocí statické schématu
Můžete definovat explicitně hello sloupce tooexpose prostřednictvím hello WebAPI.  Hello aplikace azure mobile Node.js SDK automaticky přidá žádné další sloupce, které jsou potřebné pro seznam toohello synchronizaci dat offline, který zadáte.  Například klientské aplikace rychlý start vyžadovat tabulku s dva sloupce: text (řetězec) a dokončete (boolean).  
Tabulka Hello je možné definovat v hello tabulky definice soubor JavaScript (umístěný v adresáři tabulky hello) následujícím způsobem:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Pokud definujete tabulky staticky, pak musíte také zavolat hello tables.initialize() metoda toocreate hello schéma databáze při spuštění.  vrátí metoda tables.initialize() Hello [Promise] tak, aby hello webová služba neobsluhuje žádosti před hello databáze během inicializace.

### <a name="howto-sqlexpress-setup"></a>Postupy: použití SQL Express jako úložiště dat vývoj v místním počítači
Hello Azure Mobile Apps hello AzureMobile aplikace uzlu SDK poskytuje tři možnosti pro obsluhující data předinstalované hello: Sada SDK poskytuje tři možnosti obsluhující data předinstalované hello:

* Použití hello **paměti** úložiště příklad tooprovide dočasnou ovladačů
* Použití hello **mssql** tooprovide ovladačů úložiště dat SQL Express pro vývoj
* Použití hello **mssql** tooprovide ovladačů úložiště dat služby Azure SQL Database pro produkční prostředí

Hello Azure Mobile Apps Node.js SDK používá hello [mssql Node.js balíček] tooestablish a použití tooboth připojení SQL Express a databáze SQL.  Tento balíček vyžaduje, abyste povolili připojení TCP ve vaší instanci SQL Express.

> [!TIP]
> Ovladač paměti Hello neposkytuje kompletní sadu zařízení pro testování.  Pokud chcete tootest váš back-end místně, doporučujeme hello použití úložiště dat SQL Express a hello mssql ovladačů.
>
>

1. Stáhněte a nainstalujte [Microsoft SQL Server 2014 Express].  Zkontrolujte, zda že nainstalovat SQL Server 2014 Express hello s edicí nástroje.  Pokud požadujete explicitně podpora 64bitových technologií, hello 32bitová verze spotřebovává méně paměti při spuštění.
2. Spusťte Správce konfigurace systému SQL Server 2014 hello.

   1. Rozbalte hello **konfigurace sítě serveru SQL Server** uzlu v nabídce levé stromu hello.
   2. Klikněte na tlačítko **protokoly pro funkci SQLEXPRESS**.
   3. Klikněte pravým tlačítkem na **TCP/IP** a vyberte **povolit**.  Klikněte na tlačítko **OK** v místním dialogovém okně hello.
   4. Klikněte pravým tlačítkem na **TCP/IP** a vyberte **vlastnosti**.
   5. Klikněte na tlačítko hello **IP adresy** kartě.
   6. Najde hello **IPAll** uzlu.  V hello **TCP Port** zadejte **1433**.

          ![Configure SQL Express for TCP/IP][3]
   7. Klikněte na **OK**.  Klikněte na tlačítko **OK** v místním dialogovém okně hello.
   8. Klikněte na tlačítko **služby SQL Server** v nabídce levé stromu hello.
   9. Klikněte pravým tlačítkem na **SQL Server (SQLEXPRESS)** a vyberte **restartovat**
   10. Zavřete hello Správce konfigurace systému SQL Server 2014.
3. Spustit hello SQL Server 2014 Management Studio a připojte tooyour místní instance SQL Express

   1. Klikněte pravým tlačítkem na instanci v hello Průzkumník objektů a vyberte **vlastnosti**
   2. Vyberte hello **zabezpečení** stránky.
   3. Ujistěte se, hello **režimu SQL Server a ověřování systému Windows** je vybrána
   4. Klikněte na tlačítko **OK**.

          ![Configure SQL Express Authentication][4]
   5. Rozbalte položku **zabezpečení** > **přihlášení** v hello Průzkumník objektů
   6. Klikněte pravým tlačítkem na **přihlášení** a vyberte **nové přihlašovací údaje...**
   7. Zadejte přihlašovací jméno.  Vyberte **Ověřování SQL Serveru**.  Zadejte heslo a potom zadejte hello stejné heslo v **potvrzení hesla**.  Hello heslo musí splňovat požadavky na složitost systému Windows.
   8. Klikněte na tlačítko **OK**.

          ![Add a new user tooSQL Express][5]
   9. Klikněte pravým tlačítkem na nové přihlášení a vyberte **vlastnosti**
   10. Vyberte hello **role serveru** stránky
   11. Zkontrolujte další toohello pole hello **dbcreator** role serveru
   12. Klikněte na tlačítko **OK**.
   13. Zavřete hello SQL Server 2015 Management Studio

Zajistěte, můžete záznam hello uživatelské jméno a heslo, které jste vybrali.  V závislosti na požadavcích vaší konkrétní databáze může být nutné tooassign dalších serverových rolí nebo oprávnění.

přečte Hello aplikace Node.js hello **SQLCONNSTR_MS_TableConnectionString** proměnnou prostředí pro hello připojovací řetězec pro tuto databázi.  Tuto proměnnou lze nastavit v rámci vašeho prostředí.  Můžete například použít PowerShell tooset této proměnné prostředí:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Přístup k databázi hello přes připojení TCP/IP a zadejte uživatelské jméno a heslo pro připojení hello.

### <a name="howto-config-localdev"></a>Postupy: nakonfigurujete svůj projekt pro místní vývoj
Azure Mobile Apps přečte soubor JavaScript s názvem *azureMobile.js* z místního systému souborů hello.  Použít tento soubor tooconfigure hello Azure Mobile Apps SDK v provozním prostředí není – použijte nastavení aplikace v rámci hello [portál Azure] místo.  Hello *azureMobile.js* soubor by měl exportovat objekt konfigurace.  jsou Hello nejběžnější nastavení:

* Nastavení databáze
* Nastavení protokolování diagnostiky
* Alternativní nastavení CORS

Příklad *azureMobile.js* soubor implementace hello předcházející způsobem nastavení databáze:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Doporučujeme, abyste přidali *azureMobile.js* tooyour *.gitignore* souboru (nebo jiné zdrojového kódu ignorovat souboru) tooprevent hesla z ukládají v cloudu hello.  Vždy konfigurujte nastavení produkční v nastavení aplikace v rámci hello [portál Azure].

### <a name="howto-appsettings"></a>Postupy: Konfigurace nastavení aplikace pro mobilní aplikace
Většina nastavení v hello *azureMobile.js* soubor mít ekvivalentní nastavení aplikace v hello [portál Azure].  Použití hello následující seznam tooconfigure vaší aplikace v nastavení aplikace:

| Nastavení aplikace | *azureMobile.js* nastavení | Popis | Platné hodnoty |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |jméno |Hello název aplikace hello |Řetězec |
| **MS_MobileLoggingLevel** |Logging.level |Minimální protokolu úroveň toolog zprávy |Chyba, upozornění, informace o podrobné nastavení, ladění, i |
| **MS_DebugMode** |Ladění |Povolit nebo zakázat režim ladění |Hodnota TRUE, false |
| **MS_TableSchema** |data.Schema |Výchozí název schématu pro tabulky SQL |String (výchozí: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Povolit nebo zakázat režim ladění |Hodnota TRUE, false |
| **MS_DisableVersionHeader** |verze (sada tooundefined) |Zakáže hello X-záhlaví ZUMO-Server-Version záhlaví |Hodnota TRUE, false |
| **MS_SkipVersionCheck** |skipversioncheck |Zakáže Kontrola verze klientského rozhraní API hello |Hodnota TRUE, false |

tooset nastavení aplikace:

1. Přihlaste se toohello [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** pak klikněte na název hello mobilní aplikace.
3. Otevře se okno nastavení Hello ve výchozím nastavení. Pokud není, klikněte na tlačítko **nastavení**.
4. Klikněte na tlačítko **nastavení aplikace** v nabídce Obecné hello.
5. Posuňte se toohello část nastavení aplikace.
6. Pokud vaše aplikace, nastavení už existuje, klikněte na tlačítko hello hodnota hello aplikace nastavení tooedit hello hodnoty.
7. Pokud vaše aplikace nastavení neexistuje, zadejte do hello klíč a hello hodnota v poli hodnota hello hello nastavení aplikace.
8. Po dokončení, klikněte na tlačítko **Uložit**.

Změna většinu nastavení aplikace vyžaduje restartování služby.

### <a name="howto-use-sqlazure"></a>Postupy: použití SQL Database jako produkční úložiště dat.
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Používání Azure SQL Database jako úložiště dat je stejný jako mezi všechny typy aplikací Azure App Service. Pokud jste to ještě neudělali, postupujte podle těchto kroků toocreate back-end mobilní aplikace.

1. Přihlaste se toohello [portál Azure].
2. V hello levém horním rohu okna hello, klikněte na tlačítko hello **+ nový** tlačítko > **Web + mobilní** > **mobilní aplikace**, pak zadejte název pro váš back-end mobilní aplikace.
3. V hello **skupiny prostředků** zadejte hello stejný název jako vaše aplikace.
4. plán aplikační služby výchozí Hello je vybrán.  Pokud chcete toochange plán služby App Service, můžete provést kliknutím hello plán služby App Service > **+ vytvořit nový**.  Zadejte název nového plánu služby App Service hello a vyberte požadované místo.  Klikněte na tlačítko hello cenová úroveň a vyberte příslušné cenovou úroveň služby hello. Vyberte **zobrazit všechny** tooview více ceny možnosti, jako například **volné** a **sdílené**.  Jakmile vyberete cenovou úroveň, klikněte na tlačítko hello **vyberte** tlačítko.  Zpět v hello **plán služby App Service** okně klikněte na tlačítko **OK**.
5. Klikněte na možnost **Vytvořit**. Zřizování back-end mobilní aplikace může trvat několik minut.  Po zřízení back-end mobilní aplikace hello hello portál otevře hello **nastavení** okno pro back-end mobilní aplikace hello.

Po vytvoření back-end mobilní aplikace hello tooeither můžete připojit existující SQL databáze tooyour mobilní aplikace back-end nebo vytvořte novou databázi SQL.  V této části vytvoříme databázi SQL.

> [!NOTE]
> Pokud už máte databázi v hello stejné umístění jako back-end mobilní aplikace hello, můžete místo toho zvolíte **použít existující databázi** a pak vyberte tuto databázi. z důvodu vyšší latence se nedoporučuje Hello použití databáze v jiném umístění.
>
>

1. V hello nový mobilní back-end aplikace, klikněte na **nastavení** > **mobilní aplikace** > **Data** > **+ přidat**.
2. V hello **přidat datové připojení** okně klikněte na tlačítko **SQL Database – nakonfigurujte požadovaná nastavení** > **vytvořit novou databázi**.  Zadejte název nové databáze hello hello do hello **název** pole.
3. Klikněte na tlačítko **Server**.  V hello **nový server** okno, zadejte název jedinečné serveru v hello **název serveru** pole a zadejte vhodný **přihlašovací jméno správce serveru** a **heslo**.  Ujistěte se, **povolit službám azure tooaccess serveru** je zaškrtnuté.  Klikněte na **OK**.

    ![Vytvoření databáze Azure SQL][6]
4. Na hello **novou databázi** okně klikněte na tlačítko **OK**.
5. Zpět na hello **přidat datové připojení** vyberte **připojovací řetězec**, zadejte hello přihlašovací jméno a heslo, které jste zadali při vytváření databáze hello.  Pokud použijete existující databázi, zadejte hello přihlašovací údaje pro tuto databázi.  Po zadání, klikněte na tlačítko **OK**.
6. Zpět na hello **přidat datové připojení** znovu, klikněte na **OK** toocreate hello databáze.

<!--- END OF ALTERNATE INCLUDE -->

Vytvoření databáze hello může trvat několik minut.  Použití hello **oznámení** oblasti toomonitor hello průběh nasazení hello.  Není průběhu dokud hello databáze byla úspěšně nasazena.  Jakmile úspěšně nasazena, je pro instanci služby SQL Database hello ve vaší mobilní back-end nastavení aplikace vytvořit připojovací řetězec.  Zobrazí se toto nastavení aplikace hello **nastavení** > **nastavení aplikace** > **připojovací řetězce**.

### <a name="howto-tables-auth"></a>Postupy: ověřování vyžadovat pro přístup k tootables
Pokud chcete toouse ověřování aplikace služby s koncovým bodem hello tabulky, musíte nakonfigurovat aplikaci služby ověřování v hello [portál Azure] první.  Pro další podrobnosti o konfiguraci ověřování v Azure App Service, zkontrolujte hello Průvodci konfigurací pro zprostředkovatele identity hello hodláte toouse:

* [Jak tooconfigure ověřování Azure Active Directory]
* [Jak tooconfigure ověřování Facebook]
* [Jak tooconfigure ověřování Google]
* [Jak tooconfigure Microsoft Authentication]
* [Jak tooconfigure ověřování služby Twitter]

Každá tabulka měla vlastností, které lze použít toocontrol přístup toohello tabulky.  Následující ukázka Hello zobrazuje staticky definovaných tabulku s je vyžadováno ověření.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

Vlastnost Hello přístupu může trvat jednu ze tří hodnot

* *Anonymní* označuje, že klientská aplikace hello je povolit tooread dat bez ověřování
* *ověření* indikuje, že klientská aplikace hello musí poslat platný ověřovací token požadavku hello
* *zakázané* označuje, že tato tabulka je aktuálně zakázán

Pokud vlastnost hello přístupu není definován, je povolený přístup bez ověřování.

### <a name="howto-tables-getidentity"></a>Postupy: použití ověřování deklarací identity s tabulkami
Můžete nastavit různé deklarace identity, které jsou požadovány při nastavení ověřování.  Tyto deklarace nejsou obvykle dostupné prostřednictvím hello `context.user` objektu.  Však mohou být načteny pomocí hello `context.user.getIdentity()` metoda.  Hello `getIdentity()` metoda vrátí Promise, která přeloží tooan objektu.  objekt Hello se ukládá do klíčů metodou ověřování (facebook, google, twitter, microsoftaccount nebo aad).

Například pokud nastavíte Account Microsoft ověřování a deklarace identity požadavek hello e-mailové adresy, můžete přidat záznam hello e-mailové adresy toohello pomocí následující tabulky řadiče hello:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit hello context query toothose records with hello authenticated user email address
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds hello email address from hello claims toohello context item - used for
    * insert operations
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when hello client does a request
    // READ - only return records belonging toohello authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite hello userId based on hello authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong toohello authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong toohello authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

toosee jaké deklarace identity jsou k dispozici, použijte webové prohlížeče tooview hello `/.auth/me` koncový bod vašeho webu.

### <a name="howto-tables-disabled"></a>Postupy: operace s tabulkou toospecific zakázat přístup
V přidání tooappearing v tabulce hello může být vlastnost přístupu hello použité toocontrol jednotlivých operací.  Existují čtyři operace:

* *Přečtěte si* hello RESTful získat operace pro tabulku hello
* *Vložit* hello RESTful POST operace v tabulce hello
* *Aktualizovat* hello RESTful oprava operace v tabulce hello
* *Odstranit* hello RESTful odstranit operace v tabulce hello

Například může přát tooprovide neověřené tabulky jen pro čtení:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Postupy: Úprava hello dotazu, který se používá s operace s tabulkou
Běžné požadavky pro operace s tabulkou je tooprovide omezeným zobrazením dat hello.  Například může zadat tabulku, která je označené hello ověřeného ID uživatele tak, že můžete pouze pro čtení nebo aktualizovat vlastní záznamy.  Tuto funkci zajišťuje Hello následující definici tabulky:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for hello table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for hello authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite hello userId with hello authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Operace, které obvykle spuštění dotazu mají vlastnost dotazu, můžete upravit místo, kde klauzule. Vlastnost Hello dotaz je [QueryJS] objektu, který je použité tooconvert toosomething dotazu OData, která hello back-end dat může zpracovat.  Jednoduché rovnosti případech (např. hello předcházející jeden) lze mapu. Můžete také přidat konkrétní klauzule SQL:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Postupy: Konfigurace obnovitelného odstranění na tabulce
Obnovitelného odstranění neodstraní ve skutečnosti záznamy.  Místo toho se označí je jako v databázi hello odstranit nastavením tootrue hello odstranit sloupce.  dočasně odstraněné záznamy Hello Azure Mobile Apps SDK automaticky odebere z výsledků, pokud hello Mobile Client SDK používá IncludeDeleted().  tooconfigure odstranit tabulku soft, nastavte hello `softDelete` vlastnost v souboru definice tabulky hello:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

Byste měli zavést mechanismus pro vymazání záznamů – buď z klientské aplikace, prostřednictvím webové úlohy, funkce Azure nebo prostřednictvím vlastní rozhraní API.

### <a name="howto-tables-seeding"></a>Postupy: počáteční hodnoty databázi daty
Při vytváření nové aplikace, můžete tooseed tabulku s daty.  To lze provést v rámci soubor JavaScript definice tabulky hello následujícím způsobem:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

Synchronizace replik indexů data je možné pouze při vytváření tabulky hello podle hello Azure Mobile Apps SDK.  Pokud hello tabulka již existuje v rámci hello databáze, je žádná data vloženy do tabulky hello.  Pokud je zapnutá dynamická schéma, schéma odvodit z dat hello nasadí.

Doporučujeme, abyste výslovně volání hello `tables.initialize()` metoda toocreate hello tabulky při spuštění služby hello.

### <a name="Swagger"></a>Postupy: Povolení podpory Swagger
Azure App Service Mobile Apps se dodává s integrovanou [Swagger] podporovat.  tooenable Swagger podpory nejdřív nainstalovat swagger uživatelského rozhraní hello jako závislost:

    npm install --save swagger-ui

Po instalaci můžete povolit podporu Swagger v konstruktoru Azure Mobile Apps hello:

    var mobile = azureMobileApps({ swagger: true });

Budete pravděpodobně pouze chcete tooenable Swagger podporovaných v edicích vývoj.  Můžete to provést s využitím `NODE_ENV` nastavení aplikace:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Hello koncového bodu swaggeru se nachází v http://*yoursite*.azurewebsites.net/swagger.  Dostanete hello uživatelské rozhraní Swagger prostřednictvím hello `/swagger/ui` koncový bod.  Pokud zvolíte ověřování toorequire napříč celou aplikaci, vytvoří Swagger k chybě.  Nejlepších výsledků dosáhnete, zvolte tooallow neověřené požadavky prostřednictvím v hello ověřování služby Azure App / nastavení autorizace, pak řídit ověřování pomocí hello `table.access` vlastnost.

Můžete také přidat hello Swagger možnost tooyour `azureMobile.js` souboru, pouze pokud chcete podporu Swagger při vývoji místně.

## <a name="a-namepushpush-notifications"></a><a name="push">Nabízená oznámení
Mobilní aplikace se integruje s Azure Notification Hubs tooenable jste toosend cílem nabízená oznámení toomillions zařízení pro všechny hlavní platformy. Pomocí centra oznámení můžete odesílat nabízená oznámení tooiOS, zařízení s Androidem a Windows. toolearn informace o všech funkcí, které můžete provést s Notification Hubs najdete v části [přehledu této služby](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Postupy: odesílání nabízených oznámení
Hello následující kód ukazuje, jak toouse hello nabízené objekt toosend vysílání nabízených oznámení tooregistered iOS zařízení:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do hello push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

Vytvořením šablony nabízené registrace z hello klienta, můžete místo toho odeslat šablony nabízená zpráva toodevices na všech podporovaných platformách. Následující kód ukazuje, jak Hello toosend šablony oznámení:

    // Define hello template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do hello push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }


### <a name="push-user"></a>Postupy: odeslání nabízených oznámení tooan ověřit uživatele pomocí značek
Pokud ověřený uživatel zaregistruje pro nabízená oznámení, značku ID uživatele se automaticky přidá toohello registrace. Pomocí této značky může odesílat nabízená oznámení tooall zařízení registrovaná podle konkrétního uživatele. Hello následující kód získá hello identifikátor SID uživatele, který vytvořil požadavek hello a odešle registrace zařízení k tooevery šablony nabízených oznámení pro tohoto uživatele:

    // Only do hello push if configured
    if (context.push) {
        // Send a notification toohello current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

Při registraci pro nabízená oznámení z ověřený klient, ujistěte se, že před pokusem o registraci dokončením ověřování.

## <a name="CustomAPI"></a>Vlastní rozhraní API
### <a name="howto-customapi-basic"></a>Postupy: definování vlastní rozhraní API
Kromě toho toohello přístup k datům rozhraní API prostřednictvím koncového bodu /tables hello, Azure Mobile Apps můžete zadat vlastní pokrytí rozhraní API.  Vlastní rozhraní API jsou definovány v podobné definice tabulek toohello způsob a přístup k veškerým hello stejné zařízení, včetně ověřování.

Pokud chcete toouse ověřování aplikace služby s rozhraním API vlastní, musíte nakonfigurovat aplikaci služby ověřování v hello [portál Azure] první.  Pro další podrobnosti o konfiguraci ověřování v Azure App Service, zkontrolujte hello Průvodci konfigurací pro zprostředkovatele identity hello hodláte toouse:

* [Jak tooconfigure ověřování Azure Active Directory]
* [Jak tooconfigure ověřování Facebook]
* [Jak tooconfigure ověřování Google]
* [Jak tooconfigure Microsoft Authentication]
* [Jak tooconfigure ověřování služby Twitter]

Vlastní rozhraní API jsou definovány v hodně hello stejným způsobem, jak hello tabulky rozhraní API.

1. Vytvoření **rozhraní api** adresáře
2. Vytvořte soubor JavaScript definice rozhraní API v hello **rozhraní api** adresáře.
3. Použití hello import metoda tooimport hello **rozhraní api** adresáře.

Zde je definice rozhraní api prototypu hello na základě vzorku basic aplikace hello, které jsme použili předtím.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Podívejme příklad rozhraní API, které vrátí datum server hello pomocí hello *Date.now()* metoda.  Zde je soubor api/date.js hello:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Každý parametr je jedním z hello standardní RESTful příkazy - GET, POST, PATCH nebo odstranit.  Metoda Hello je standard [ExpressJS Middleware] funkce, která odesílá výstup hello vyžaduje.

### <a name="howto-customapi-auth"></a>Postupy: ověřování vyžadovat pro přístup k tooa vlastního rozhraní API
Azure Mobile Apps SDK implementuje ověřování v hello stejným způsobem jako pro koncový bod tabulky hello i vlastní rozhraní API.  Chcete-li přidat rozhraní API ověřování toohello vyvinuté v předchozí části hello, přidejte **přístup** vlastnost:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Můžete také zadat ověřování na konkrétní operace:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // hello GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

stejný token, který se používá pro koncový bod tabulky hello Hello musí použít pro vlastní rozhraní API, které vyžadují ověřování.

### <a name="howto-customapi-auth"></a>Postupy: zpracování nahrávání velkých souborů
Azure Mobile Apps SDK používá hello [textu analyzátor middleware](https://github.com/expressjs/body-parser) tooaccept a dekódování obsah textu v vaše žádost.  Můžete předkonfigurovat textu analyzátor tooaccept větší nahrávání souborů:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

soubor Hello je před samotným přenosem kódování base-64.  Zvyšuje se tím velikost hello skutečné nahrávání hello (a proto hello velikost, kterou musíte vzít v úvahu pro).

### <a name="howto-customapi-sql"></a>Postup: provedení vlastní SQL příkazy
Hello Azure Mobile Apps SDK umožňuje přístup toohello celý kontextu prostřednictvím objektu žádosti hello, což vám tooexecute snadno parametry zprostředkovatel definované datové toohello příkazy SQL:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on tooa later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define hello query - anything that can be handled by hello mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute hello query.  hello context for Azure Mobile Apps is available through
            // request.azureMobile - hello data object contains hello configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Ladění, snadno tabulek a snadno rozhraní API
### <a name="howto-diagnostic-logs"></a>Postupy: ladění, Diagnostika a řešení potíží s Azure Mobile apps
Hello Azure App Service poskytuje několik ladění a řešení potíží s techniky pro aplikace Node.js.
Odkažte toohello následující články tooget spuštěna při řešení potíží váš back-end Node.js Mobile:

* [Monitorování služby Azure App Service]
* [Povolit protokolování diagnostiky v Azure App Service]
* [Řešení potíží s Azure App Service v sadě Visual Studio]

Aplikace Node.js mají přístup tooa širokou škálu protokolů diagnostiky nástroje.  Interně hello používá Azure Mobile Apps Node.js SDK [Winstona] pro protokolování diagnostiky.  Povolení režimu ladění, nebo nastavení hello automaticky povoleno protokolování **MS_DebugMode** tootrue nastavení aplikace v hello [portál Azure]. Vygenerovaný protokolů se objeví v hello diagnostické protokoly na hello [portál Azure].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Postupy: práce s tabulkami snadno v hello portálu Azure
Snadno tabulek portálu hello umožňují vytváření a práci s pravé tabulky hello portálu. Dokonce můžete upravit operace s tabulkou pomocí hello Editor služby aplikace.

Když kliknete na tlačítko **snadno tabulky** v nastavení vašeho back-end serveru, můžete přidávat, upravovat nebo odstranění tabulky. Můžete také zobrazit data v tabulce hello.

![Práce s tabulkami snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Hello následující příkazy jsou k dispozici na panelu příkazů hello pro tabulku:

* **Změna oprávnění** – upravit hello oprávnění pro čtení, vložit, aktualizovat a odstranit operací v tabulce hello.
  Možnosti jsou tooallow anonymní přístup, toorequire ověřování nebo toodisable všechny toohello operace přístupu k.
* **Upravte skript** -hello souboru skriptu pro tabulku hello je otevřen v hello Editor aplikace služby.
* **Správa schématu** – přidání nebo odstranění sloupce nebo změna hello tabulky indexu.
* **Odstranit tabulku** -zkrátí existující tabulky se odstraňuje všechny řádky dat, ale ponechat hello schématu beze změny.
* **Odstranit řádky** -odstranit jednotlivé řádky dat.
* **Protokoly streamování zobrazení** -propojení toohello streamování služby protokolování pro svůj web.

### <a name="work-easy-apis"></a>Postupy: práci s rozhraními API snadno v hello portálu Azure
Snadno rozhraní API portálu hello umožňují vytvářet a pracovat s vlastní práva rozhraní API portálu hello. Můžete upravit skriptů rozhraní API pomocí hello Editor aplikace služby.

Když kliknete na tlačítko **rozhraní API pro snadný** v nastavení vašeho back-end serveru, můžete přidávat, upravovat nebo odstranit vlastní koncový bod rozhraní API.

![Práci s rozhraními API snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Hello portálu můžete změnit hello přístupová oprávnění pro danou akci HTTP, upravte soubor skriptu rozhraní API hello v editoru služby aplikace nebo zobrazit protokoly streamování hello.

### <a name="online-editor"></a>Postupy: úpravy kódu v hello Editor služby aplikace
Hello portál Azure umožňuje upravit soubory skriptu back-end Node.js v hello Editor služby aplikace bez nutnosti stáhnout hello projektu tooyour místního počítače. soubory skriptu tooedit v online editoru hello:

1. V okně back-end mobilní aplikace, klikněte na tlačítko **všechna nastavení** > buď **snadno tabulky** nebo **rozhraní API pro snadný**, klikněte na tabulku nebo rozhraní API a pak klikněte na tlačítko **upravte skript**. soubor skriptu Hello je otevřen v hello Editor aplikace služby.

    ![Editor služby aplikace](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Zkontrolujte souboru změny toohello kódu v editoru online hello. Změny se při psaní automaticky uloží.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Rychlý start Android klienta]: app-service-mobile-android-get-started.md
[Rychlý start Apache Cordova klienta]: app-service-mobile-cordova-get-started.md
[iOS klienta rychlý start]: app-service-mobile-ios-get-started.md
[Rychlý start Xamarin.iOS klienta]: app-service-mobile-xamarin-ios-get-started.md
[Rychlé spuštění klienta Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
[Rychlý start Xamarin.Forms klienta]: app-service-mobile-xamarin-forms-get-started.md
[Rychlé spuštění klienta Windows Store]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[synchronizaci dat offline]: app-service-mobile-offline-data-sync.md
[Jak tooconfigure ověřování Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Jak tooconfigure ověřování Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Jak tooconfigure ověřování Google]: app-service-mobile-how-to-configure-google-authentication.md
[Jak tooconfigure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak tooconfigure ověřování služby Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md
[Příručka pro nasazení Azure App Service]: ../app-service-web/web-sites-deploy.md
[Monitorování služby Azure App Service]: ../app-service-web/web-sites-monitor.md
[Povolit protokolování diagnostiky v Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Řešení potíží s Azure App Service v sadě Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[zadejte hello uzlu verze]: ../nodejs-specify-node-version-azure-apps.md
[použijte moduly uzlu]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[portál Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp ukázce na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo ukázce na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[ukázky adresáře na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 pro sadu Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js balíček]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winstona]: https://github.com/winstonjs/winston
