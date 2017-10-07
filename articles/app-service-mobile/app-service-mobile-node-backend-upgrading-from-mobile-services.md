---
title: "aaaUpgrade z mobilní služby tooAzure aplikační služby - Node.js"
description: "Zjistěte, jak tooeasily upgradu vaší aplikace tooan s Mobile Services aplikace služby mobilní aplikace"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 722cda244d4f633247827f58ea6f1397137ea600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-tooapp-service"></a>Upgrade existující Node.js Azure Mobile Service tooApp služby
Mobile App Service je nový způsob toobuild mobilních aplikací pomocí Microsoft Azure. Další, najdete v části toolearn [co jsou Mobile Apps?].

Toto téma popisuje, jak tooupgrade existující aplikaci back-end Node.js z Azure Mobile Services tooa nové App Service Mobile Apps. Při provádění tohoto upgradu, aplikace pro Mobile Services můžete dál toooperate.  Pokud potřebujete tooupgrade aplikace back-end Node.js, podívejte se příliš[upgradu vaší .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).

Když mobilní back-end je upgradovaná tooAzure služby App Service, má přístup tooall funkce služby App Service a se účtují podle příliš[služby App Service – ceny], není Mobile Services ceny.

## <a name="migrate-vs-upgrade"></a>Migrace a upgrade
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Je doporučeno, které [provést migraci](app-service-mobile-migrating-from-mobile-services.md) před zahájením upgradu. Tímto způsobem můžete vložit obě verze aplikace hello stejný plán služby App Service a způsobit bez dalších nákladů.
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Vylepšení v serveru mobilní aplikace Node.js SDK
Upgrade toohello nové [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) poskytuje mnoho vylepšení, včetně:

* Podle hello [Express framework](http://expressjs.com/en/index.html), hello novou sadu SDK uzlu je šedé – a je navržená tak tookeep si nové verze uzlu jako pocházejí. Můžete přizpůsobit chování aplikace hello s Express middleware.
* Významné zlepšení výkonu porovná toohello Mobile Services SDK.
* Web můžete hostovat spolu s vaší mobilní back-end; Podobně je snadno tooadd hello Azure Mobile SDK tooany existující express.v4 aplikace.
* Vytvořené pro vývoj pro různé platformy a místní, hello Mobile Apps SDK může být vyvinuté a spustit místně na platformách Windows, Linux a OSX. Nyní je snadno toouse běžné uzlu vývoj techniky jako spuštěná [téměř jako](https://mochajs.org/) testy předchozí toodeployment.

## <a name="overview"></a>Základní přehled upgradu
tooaid v rámci aktualizace back-end Node.js, Azure App Service poskytl balíček kompatibility.  Po upgradu budete mít niew lokality, které můžou být nasazené tooa nový Web App Service.

Hello klientem Mobile Services SDK jsou **není** kompatibilní s hello nový Mobile Apps server SDK. V pořadí tooprovide kontinuitu poskytování služeb pro vaši aplikaci by neměl publikovat změny tooa lokality aktuálně obsluhující publikované klientů. Místo toho by měla vytvoříte novou mobilní aplikaci, která slouží jako duplicitní. Tuto aplikaci můžete umístit na hello stejné služby App Service plánování tooavoid by docházelo k další finanční náklady.

Pak bude mít dvě verze aplikace hello: ten, který zůstává stejný hello slouží publikované aplikace v hello zástupné a jiných, která pak můžete upgradovat a cíl s nového klienta verze. Můžete přesunout a otestujte svůj kód vaší tempem, ale ujistěte se, že všechny opravy chyb, které provedete získat použité tooboth. Jakmile si myslíte, že požadovaný počet klientských aplikací v zástupné aktualizovaly toohello nejnovější verzi, můžete odstranit původní migrovanou aplikaci hello vyžadujete-li. Ji není vynakládá žádné další peněžní, pokud je hostovaná v hello stejné služby App Service plánování jako mobilní aplikace.

Hello úplné osnovy pro proces upgradu hello vypadá takto:

1. Stáhněte si existující Mobile Service (migrované) Azure.
2. Převést hello projektu tooan mobilní aplikace Azure pomocí balíčku kompatibility hello.
3. Opravte případné rozdíly (například nastavení ověřování).
4. Nasazení vaší převedený projektu tooa mobilní aplikace Azure nové služby App Service.
5. Vydání nové verze klienta aplikace, použijte hello nové mobilní aplikace.
6. (Volitelné) Odstraňte původní aplikace migrované mobilní služby.

Odstranění může dojít, když nevidíte přenosy dat na původní migrované mobilní službě.

## <a name="install-npm-package"></a>Nainstalujte požadavky hello
[Uzel] musíte nainstalovat na místním počítači.  Také byste měli nainstalovat balíček kompatibility hello.  Po instalaci uzlu můžete spustit hello z nové cmd nebo příkazovém řádku prostředí PowerShell následující příkaz:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Získat skripty Azure Mobile Services
* Přihlaste se toohello [portálu Azure].
* Pomocí **všechny prostředky** nebo **App Services**, najít váš web Mobile Services.
* V rámci lokality hello, klikněte na **nástroje** -> **Kudu** -> **přejděte** tooopen hello Kudu lokality.
* Klikněte na **ladění konzoly** -> **prostředí PowerShell** konzolou pro ladění tooopen hello.
* Přejděte příliš`site/wwwroot/App_Data/config` pak kliknutím na každý adresář
* Klikněte na další toohello ikonu hello stažení `scripts` adresáře.

To bude stahovat hello skripty ve formátu ZIP.  Vytvořte nový adresář na místním počítači a rozbalte hello `scripts.ZIP` soubor v adresáři hello.  Tím se vytvoří `scripts` adresáře.

## <a name="scaffold-app"></a>Vygenerované uživatelské rozhraní hello nový Azure Mobile Apps back-end
Spusťte následující příkaz z adresáře hello, která obsahuje adresář skripty hello hello:

```scaffold-mobile-app scripts out```

Tím se vytvoří vygenerované back-end Azure Mobile Apps v hello `out` adresáře.  Ačkoli to není vyžadováno, je vhodné toocheck hello `out` adresář do úložiště zdrojového kódu podle svého výběru.

## <a name="deploy-ama-app"></a>Nasazení váš back-end Azure Mobile Apps
Během nasazení budete potřebovat toodo hello následující:

1. Vytvoření nové mobilní aplikace v hello [portálu Azure].
2. Spustit hello `createViews.sql` skript v připojené databázi.
3. Odkaz hello databázi, která je propojená tooyour Mobile Service tooyour nové služby App Service.
4. Odkaz jiné prostředky (například Notification Hubs) toohello nové služby App Service.
5. Nasaďte nové lokality tooyour hello vygenerovat kód.

### <a name="create-a-new-mobile-app"></a>Vytvoření nové mobilní aplikace
1. Přihlaste se na hello [portálu Azure].
2. Klikněte na **+NOVÝ** > **Web + mobilní zařízení** > **Mobilní aplikace** a potom zadejte název back-endu mobilní aplikace .
3. Pro hello **skupiny prostředků**, vyberte existující skupinu prostředků nebo vytvořte novou (pomocí hello stejný název jako vaše aplikace.)

    Můžete buď vybrat jiný plán služby App Service nebo vytvořit nový. Další informace o plánech aplikační služby a jak toocreate nového plánu v různých cenových vrstvy a v požadovaném umístění najdete v části [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. Pro hello **plán služby App Service**, hello výchozí plán (v hello [úrovně Standard](https://azure.microsoft.com/pricing/details/app-service/)) je vybrána. Můžete také vybrat jiný plán nebo [vytvořit nový](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Hello plán služby App Service určuje nastavení hello [umístění, funkce a nákladové a výpočetní prostředky](https://azure.microsoft.com/pricing/details/app-service/) přidružené k vaší aplikaci.

    Jakmile se rozhodnete hello plán, klikněte na tlačítko **vytvořit**. Tím vytvoříte back-end mobilní aplikace hello.

### <a name="run-createviewssql"></a>Spustit CreateViews.SQL
Hello vygenerované aplikaci, která obsahuje soubor s názvem `createViews.sql`.  Tento skript je třeba spustit v cílové databázi.  Hello připojovací řetězec pro hello cílová databáze můžete získat z migrovaných mobilní služby z hello **nastavení** okno pod **připojovací řetězce**.  Je název `MS_TableConnectionString`.

Můžete spustit tento skript v SQL Server Management Studio nebo Visual Studio.

### <a name="link-hello-database-tooyour-app-service"></a>Odkaz hello tooyour databáze služby App Service
Odkaz hello existující databáze tooyour služby App Service:

* V hello [portálu Azure], otevřete App Service.
* Vyberte **všechna nastavení** -> **datová připojení**.
* Klikněte na **+ přidat**.
* V hello rozevíracího seznamu vyberte **databáze SQL**
* V části **SQL Database**, vyberte existující databázi a pak klikněte na **vyberte**.
* V části **připojovací řetězec**, zadejte hello uživatelské jméno a heslo pro hello databáze a pak klikněte na **OK**.
* V hello **přidat datová připojení** okně klikněte na **OK**.

zobrazením hello připojovací řetězec pro hello cílová databáze ve službě migrované Mobile lze nalézt Hello uživatelské jméno a heslo.

### <a name="set-up-authentication"></a>Nastavení ověřování
Azure Mobile Apps umožňuje tooconfigure Azure Active Directory, Facebook, Google, Microsoft a Twitter, ověřování v rámci služby hello.  Vlastní ověřování bude potřebovat toobe vyvinuté samostatně.  Odkazovat na hello [konceptů ověřování] dokumentaci a [rychlý start ověřování] Další informace naleznete v dokumentaci.  

## <a name="updating-clients"></a>Aktualizace mobilních klientů
Až budete mít provozní back-end mobilní aplikace, můžete pracovat na novou verzi klientskou aplikaci, která se využívá. Mobilní aplikace také zahrnuje novou verzi klienta hello sady SDK a podobné upgrade serveru v toohello výše, budete potřebovat tooremove všech odkazech toohello Mobile Services SDK před instalací verze mobilní aplikace.

Jednou z hlavních změn hello mezi verzemi hello je hello konstruktory už nebudou potřebovat klíč aplikace.
Nyní jednoduše předáte v adrese URL hello mobilní aplikace. Například na hello klientů .NET, hello `MobileServiceClient` konstruktor je nyní:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of hello Mobile App
        );

Další informace o instalaci hello nové sady SDK a použitím hello nové struktury prostřednictvím hello odkazy níže:

* [Verzi systému Android 2.2 nebo vyšší](app-service-mobile-android-how-to-use-client-library.md)
* [iOS verze 3.0.0 nebo novější](app-service-mobile-ios-how-to-use-client-library.md)
* [Rozhraní .NET (Windows nebo Xamarin) verze 2.0.0 nebo novější](app-service-mobile-dotnet-how-to-use-client-library.md)
* [Apache Cordova verze 2.0 nebo novější](app-service-mobile-cordova-how-to-use-client-library.md)

Pokud vaše aplikace provede pomocí nabízených oznámení, poznamenejte si hello registrace konkrétní pokyny pro každou platformu, protože byly některé změny existuje také.

Až budete mít hello novou verzi klienta připraven, vyzkoušejte ji proti projektu upgradovaný server. Po ověření, že bude fungovat, můžete vydat novou verzi toocustomers vaší aplikace. Nakonec Jakmile vaši zákazníci mohli dříve prvního tooreceive těchto aktualizací, můžete odstranit hello Mobile Services verzi vaší aplikace. V tomto okamžiku jste zcela upgradovali tooan aplikace služby mobilní aplikace pomocí hello nejnovější Mobile Apps serveru SDK.

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[co jsou Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[služby App Service – ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[konceptů ověřování]: ../app-service/app-service-authentication-overview.md
[rychlý start ověřování]: app-service-mobile-auth.md

[portálu Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
