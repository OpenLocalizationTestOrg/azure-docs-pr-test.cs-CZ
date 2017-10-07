---
title: "aaaUpgrade z tooAzure mobilní služby App Service"
description: "Zjistěte, jak tooeasily upgradu vaší aplikace tooan s Mobile Services aplikace služby mobilní aplikace"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 9c0ac353-afb6-462b-ab94-d91b8247322f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2b75a1b824e8ef0d36c9053f2f19af051479f928
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-net-azure-mobile-service-tooapp-service"></a>Upgrade stávající služba Mobile Azure .NET tooApp služby
Mobile App Service je nový způsob toobuild mobilních aplikací pomocí Microsoft Azure. Další, najdete v části toolearn [co jsou Mobile Apps?].

Toto téma popisuje, jak tooupgrade existující .NET back-end aplikace z Azure Mobile Services tooa nové App Service Mobile Apps. Při provádění tohoto upgradu, aplikace pro Mobile Services můžete dál toooperate.   Pokud potřebujete tooupgrade aplikace back-end Node.js, podívejte se příliš[upgradu vaší Node.js Mobile Services](app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Když mobilní back-end je upgradovaná tooAzure služby App Service, má přístup tooall funkce služby App Service a se účtují podle příliš[služby App Service – ceny], není Mobile Services ceny.

## <a name="migrate-vs-upgrade"></a>Migrace a upgrade
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Je doporučeno, které [provést migraci](app-service-mobile-migrating-from-mobile-services.md) před zahájením upgradu. Tímto způsobem můžete vložit obě verze aplikace hello stejný plán služby App Service a způsobit bez dalších nákladů.
>
>

### <a name="improvements-in-mobile-apps-net-server-sdk"></a>Vylepšení v nástroji server mobilní aplikace .NET SDK
Upgrade toohello nové [Mobile Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) poskytuje hello následující výhody:

* Větší flexibilitu v NuGet závislosti. Hello hostování prostředí už poskytuje vlastní verzích balíčky NuGet, abyste je mohli používat alternativní verze kompatibilní. Pokud existují nové kritické bugfixes nebo toohello aktualizace zabezpečení Mobile serveru SDK nebo závislosti, je však nutné ručně aktualizovat služby.
* Větší míra flexibility v hello mobilní SDK. Funkce, které můžete řídit explicitně a trasy jsou nastaveny, jako je ověřování, tabulka rozhraní API a hello nabízené registrace koncového bodu. Další, najdete v části toolearn [jak toouse hello .NET server SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).
* Podpora pro jiné typy projektů ASP.NET a trasy. Nyní můžete hostovat řadiče MVC a webového rozhraní API v hello stejné projektu jako projektu mobilního back-endu.
* Podpora nových funkcích služby App Service ověřování, které umožňují toouse obvyklé konfigurace ověřování napříč webových a mobilních aplikací.

## <a name="overview"></a>Základní přehled upgradu
V mnoha případech upgradu bude stejně jednoduché jako přepínání toohello nový Mobile Apps server SDK a opětovné publikování kódu do nové instance mobilní aplikace. Existují, ale některé scénáře, které se vyžadují určitou další konfiguraci, jako je například pokročilého ověřování scénáře a pracovat s naplánované úlohy. Každá z těchto součástí hello později částech.

> [!TIP]
> Doporučujeme přečíst si a pochopit hello zbývající část tohoto tématu úplně před zahájením upgradu. Poznamenejte si všechny funkce, pomocí kterého jsou vyznačeny níže.
>
>

Hello klientem Mobile Services SDK jsou **není** kompatibilní s hello nový Mobile Apps server SDK. V pořadí tooprovide kontinuitu poskytování služeb pro vaši aplikaci by neměl publikovat změny tooa lokality aktuálně obsluhující publikované klientů. Místo toho by měla vytvoříte novou mobilní aplikaci, která slouží jako duplicitní. Tuto aplikaci můžete umístit na hello stejné služby App Service plánování tooavoid by docházelo k další finanční náklady.

Pak bude mít dvě verze aplikace hello: jeden které zůstane stejný hello a slouží publikovaných aplikací v hello divoký a hello jiných, která pak můžete upgradovat a cíl s novou verzí klienta. Můžete přesunout a otestujte svůj kód vaší tempem, ale ujistěte se, že všechny opravy chyb, které provedete získat použité tooboth. Jakmile si myslíte, že požadovaný počet klientských aplikací v hello divoký aktualizovaly toohello nejnovější verzi, můžete odstranit původní migrovanou aplikaci hello vyžadujete-li.

Hello úplné osnovy pro proces upgradu hello vypadá takto:

1. Vytvoření nové mobilní aplikace
2. Aktualizace hello projektu toouse hello nové sady SDK serveru
3. Vydání nové verze klientské aplikace
4. (Volitelné) Odstranit původní migrované instanci

## <a name="mobile-app-version"></a>Vytváření druhé instance aplikace
Hello prvním krokem při upgradu je toocreate hello mobilní aplikace prostředku, který bude hostitelem hello novou verzi vaší aplikace. Pokud jste již migrovali stávající mobilní službu, budete chtít toocreate touto verzí na hello stejný plán, který hostování. Otevřete hello [portál Azure] a přejděte tooyour migrovat aplikace. Poznamenejte si plán služby App Service je spuštěn na hello.

Dále vytvořte druhou instanci aplikace hello podle následující hello [pokyny pro vytvoření rozhraní .NET back-end](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Když výzvami tooselect, můžete plán služby App Service nebo "hostování plán" zvolit hello plán migrované aplikace.

Budete pravděpodobně chtít toouse hello stejnou databázi a Centrum oznámení, jako je nebyla v Mobile Services. Tyto hodnoty můžete zkopírovat otevřením [portál Azure] a navigace toohello původní aplikace, klikněte na tlačítko **nastavení** > **nastavení aplikace**. V části **připojovací řetězce**, kopie `MS_NotificationHubConnectionString` a `MS_TableConnectionString`. Přejděte tooyour nové upgradu lokality a vložit do přepsal všechny existující hodnoty. Tento postup opakujte pro všechny ostatní nastavení aplikace potřebám vaší aplikace. Pokud nepoužíváte migrované službě, můžete číst nastavení aplikace a připojovacích řetězců z hello **konfigurace** kartě hello Mobile Services část hello [portál Azure classic].

Vytvořit kopii projekt ASP.NET hello pro vaši aplikaci a publikujete ho v tooyour nové lokality. Pomocí kopie klientskou aplikaci aktualizovat hello nové adrese URL, ověřte, že vše funguje podle očekávání.

## <a name="updating-hello-server-project"></a>Aktualizace projektu server hello
Mobile Apps poskytuje novou [mobilní aplikace Server SDK] který velkou část hello nabízí stejné funkce jako runtime hello Mobile Services. Nejdřív byste měli odebrat všechny odkazy na toohello Mobile Services balíčky. V modulu snap-in Správce balíčků NuGet hello vyhledejte `WindowsAzure.MobileServices.Backend`. Většina aplikací se zobrazí několik balíčků tady, včetně `WindowsAzure.MobileServices.Backend.Tables` a `WindowsAzure.MobileServices.Backend.Entity`. V takovém případě začínat hello nejnižší balíček v hello strom závislosti, jako například `Entity`a jeho odebrání. Po zobrazení výzvy, neodstraňujte všechny závislé balíčky. Tento postup opakujte, dokud neodeberete `WindowsAzure.MobileServices.Backend` sám sebe.

V tomto okamžiku budete mít projekt, který již neodkazuje na Mobile Services SDK.

Dále přidáte odkazy hello Mobile Apps SDK. Pro tento upgrade, bude Většina vývojářů chcete toodownload a nainstalovat hello `Microsoft.Azure.Mobile.Server.Quickstart` balíček, jak to bude pro vyžádání obsahu v celé sadě požadované hello.

Bude mnoha chyby kompilátoru vyplývající z rozdíly mezi hello sady SDK, ale jsou snadno tooaddress a jsou popsané v hello zbývající část tohoto oddílu.

### <a name="base-configuration"></a>Základní konfigurace
Potom v WebApiConfig.cs, můžete nahradit:

        // Use this class tooset configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class tooset WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

S

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

> [!NOTE]
> Pokud toolearn chcete další informace o hello nový server .NET SDK a jak funkce tooadd nebo odebrat z vaší aplikace, najdete v tématu hello [jak toouse hello .NET server SDK] tématu.
>
>

Pokud vaše aplikace umožňuje používání funkce hello ověřování, budete také potřebovat tooregister OWIN middleware. V takovém případě byste měli přesunout hello výše konfigurace kódu do nové třídy OWIN při spuštění.

1. Přidání balíčku NuGet hello `Microsoft.Owin.Host.SystemWeb` Pokud již není zahrnutý v projektu.
2. V sadě Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **přidat** -> **novou položku**. Vyberte **webové** -> **Obecné** -> **třídy pro spuštění OWIN**.
3. Přesunutí hello výše kód pro MobileAppConfiguration z `WebApiConfig.Register()` toohello `Configuration()` metoda třídy nové spuštění.

Ujistěte se, zda text hello `Configuration()` metoda končí:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Existují související tooauthentication další změny, které jsou popsané v následující části hello úplného ověření.

### <a name="working-with-data"></a>Práce s daty
V Mobile Services název mobilní aplikace hello zpracovat jako hello výchozí název schématu v instalačním programu hello Entity Framework.

tooensure, zda máte hello stejné schéma se na ně odkazovat jako předtím hello použít následující schéma hello tooset v hello DbContext pro aplikaci:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Zkontrolujte prosím, že máte MS_MobileServiceName nastavit, pokud hello výše. Také můžete zadat jiný název schématu pokud dříve vaší aplikace to přizpůsobit.

### <a name="system-properties"></a>Vlastnosti systému
#### <a name="naming"></a>Pojmenování
V Azure Mobile Services server hello SDK, vlastnosti systému vždycky obsahovat dvojité podtržítko (`__`) předponu pro hello vlastnosti:

* __createdAt
* __updatedAt
* __deleted
* __version

Hello klientem Mobile Services SDK mít speciální logiku pro analýzu vlastnosti systému v tomto formátu.

V Azure Mobile Apps vlastnosti systému už mít speciální formátování a mít hello následující názvy:

* CreatedAt
* updatedAt
* Odstranit
* Verze

Hello Mobile Apps klienta SDK použití hello nové systému vlastnosti názvy, takže nejsou žádné změny vyžaduje tooclient kódu. Pokud jste přímo provedení REST zavolá službu tooyour pak musíte změnit své dotazy odpovídajícím způsobem.

#### <a name="local-store"></a>Místní úložiště
Hello změny toohello názvy vlastností systému znamená, že místní databázi offline synchronizace pro Mobile Services není kompatibilní s Mobile Apps. Pokud je to možné Vyhněte se upgrade klientských aplikací z tooMobile Mobile Services, které byly odeslány aplikace až po změny čekající na zpracování toohello serveru. Potom hello upgradovaný aplikace by měl používat nový název souboru databáze.

Pokud klientské aplikace upgradován z mobilní služby tooMobile aplikace při v hello operace fronty jsou čekající změny v režimu offline, hello systémové databáze musí být aktualizované toouse hello nové názvy sloupců. V systému iOS toho lze dosáhnout pomocí názvy sloupců hello toochange lightweight migrace. Na zařízení s Androidem a hello spravovaný klient .NET byste měli zapsat vlastní SQL toorename hello sloupce pro vaše data objektu tabulky.

V systému iOS měli byste změnit schéma základní Data pro vaše data entity toomatch hello následující. Všimněte si, že hello vlastnosti `createdAt`, `updatedAt` a `version` již nemáte `ms_` předpony:

| Atribut | Typ | Poznámka |
| --- | --- | --- |
| id |Řetězec, označen jako požadovaný |primární klíč v vzdáleného úložiště |
| CreatedAt |Datum |Vlastnost systému toocreatedAt mapy (volitelné) |
| updatedAt |Datum |Vlastnost systému tooupdatedAt mapy (volitelné) |
| Verze |Řetězec |(volitelné) používané toodetect konfliktů, tooversion mapy |

#### <a name="querying-system-properties"></a>Dotaz na vlastnosti systému
V Azure Mobile Services nejsou odesílány vlastnosti systému ve výchozím nastavení, ale jenom v případě, že jsou vyžádané pomocí řetězce dotazu hello `__systemProperties`. Naopak v systému Azure Mobile Apps jsou vlastnosti **vždycky vybraná** vzhledem k tomu, že jsou součástí objektový model sady SDK serveru hello.

Tato změna ovlivňuje především vlastních implementací správce domény, jako je například rozšíření `MappedEntityDomainManager`. V Mobile Services, pokud klient požaduje nikdy všechny vlastnosti systému, je možné toouse `MappedEntityDomainManager` který nemapují ve skutečnosti všechny vlastnosti. V Azure Mobile Apps, ale tyto nenamapovaný vlastnosti dojde k chybě v dotazech GET.

Nejjednodušší způsob, jak tooresolve hello problém Hello je toomodify vaší DTOs tak, aby se dědí `ITableData` místo `EntityData`. Pak přidejte hello `[NotMapped]` atribut toohello pole, která by měla být vynechán.

Například následující hello definuje `TodoItem` bez vlastností systému:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Poznámka: Pokud dojde k chybám `NotMapped`, přidejte referenční sestavení při toohello `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS
Mobilní služby zahrnuty některé podpora CORS nástrojem pro zabalení hello řešení ASP.NET CORS. Tuto vrstvu zabalení byla odebrána toogive hello vývojáře větší kontrolu, tak můžete přímo využít [podporu pro ASP.NET CORS](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

Hello hlavní oblastí zájmu Pokud pomocí CORS jsou tohoto hello `eTag` a `Location` hlavičky musí být povoleno v pořadí pro hello klientské sady SDK toowork správně.

### <a name="push-notifications"></a>Nabízená oznámení
Nabízená instalace hello hlavní položku, která bude možná chybí hello Server SDK je třída PushRegistrationHandler hello. Registrace zpracovávané trochu jinak v Mobile Apps a tagless registrace se ve výchozím nastavení povolené. Správa značky mohou možné dosáhnout použitím vlastního rozhraní API. Najdete v tématu hello [registrace pro značky](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) pokyny pro další informace.

### <a name="scheduled-jobs"></a>Naplánované úlohy
Naplánované úlohy nejsou součástí Mobile Apps, všechny existující úlohy, které máte ve vašem .NET back-end bude nutné toobe upgradovat jednotlivě. Jednou z možností je toocreate naplánované [webovou úlohu] v lokalitě kód mobilní aplikace hello. Můžete také nastavit kontroler, který obsahuje kód úlohy a nakonfigurovat hello [Azure Scheduler] toohit tohoto koncového bodu podle plánu hello očekává.

### <a name="miscellaneous-changes"></a>Ostatní změny
Všechny ApiControllers, které budou využívat mobilního klienta musí mít teď hello `[MobileAppController]` atribut. To již není zahrnuta ve výchozím nastavení tak, aby ostatní ApiControllers toogo nehodou zasažena hello mobilní formátování.

Hello `ApiServices` objekt je už součástí hello SDK. tooaccess nastavení mobilní aplikace, můžete použít následující hello:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Podobně protokolování se nyní provádí pomocí hello zápis trasování standardní ASP.NET:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

## <a name="authentication"></a>Důležité informace o ověřování
Hello ověřování součástí Mobile Services nyní byly přesunuty do funkce hello ověřování/autorizace služby App Service. Dozvíte se o to povolení pro svůj web ve čtení hello [přidat ověřování tooyour mobilní aplikace](app-service-mobile-ios-get-started-users.md) tématu.

Pro někteří poskytovatelé, například AAD a Facebook, Google musí být schopný tooleverage hello existující registraci z vaší kopie aplikace. Jednoduše potřebovat portál toonavigate toohello poskytovatele identit a přidat nové registrace toohello adresa URL přesměrování. Nakonfigurujte ověřování/autorizace služby App Service se hello ID klienta a tajný klíč.

### <a name="controller-action-authorization"></a>Autorizace akce kontroleru
Všechny instance hello `[AuthorizeLevel(AuthorizationLevel.User)]` atribut musí být nyní změněné toouse hello standardní ASP.NET `[Authorize]` atribut. Kromě toho řadiče jsou nyní anonymní ve výchozím nastavení, jako ostatní aplikace ASP.NET.
Pokud používáte jednu z hello jiné možnosti AuthorizeLevel, například správce nebo aplikaci, Upozorňujeme, že jsou pryč. Místo toho můžete nastavit AuthorizationFilters pro sdílené tajné klíče nebo bezpečně nakonfigurovat volání služba služba tooenable objektu služby AAD.

### <a name="getting-additional-user-information"></a>Získání informací o další uživatele
Můžete získat další uživatelské informace, včetně přístupových tokenů prostřednictvím hello `GetAppServiceIdentityAsync()` metoda:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Kromě toho pokud vaše aplikace používá závislosti na uživatelské ID, jako je například ukládání je v databázi, je důležité, toonote, který hello ID uživatele Mobile Services a App Service Mobile Apps se liší. Hello ID uživatele Mobile Services, můžete pořád dostat, když. Všechny hello ProviderCredentials podtřídy mít vlastnost ID uživatele. Z příkladu hello před pokračováním proto:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Pokud vaše aplikace využít všechny závislosti na ID uživatele, je důležité, budete moci využít hello stejnou registraci pomocí zprostředkovatele identity, pokud je to možné. ID uživatelů jsou obvykle vymezená toohello registrace aplikace, která byla použita, proto Představujeme nové registrace může způsobit problémy s odpovídajícími uživatelům tootheir data.

### <a name="custom-authentication"></a>Vlastního ověřování
Pokud vaše aplikace používá vlastní řešení ověřování, budete chtít toomake hello upgradovaný lokality má přístup toohello systému. Postupujte podle pokynů nové hello pro vlastní ověřování v hello [Přehled sady SDK serveru .NET] toointegrate řešení. Upozorňujeme, že hello vlastního ověřování součásti jsou stále ve verzi preview.

## <a name="updating-clients"></a>Aktualizace klientů
Až budete mít provozní back-end mobilní aplikace, můžete pracovat na novou verzi klientskou aplikaci, která se využívá. Mobilní aplikace také zahrnuje novou verzi klienta hello sady SDK a podobné upgrade serveru v toohello výše, budete potřebovat tooremove všech odkazech toohello Mobile Services SDK před instalací verze mobilní aplikace hello.

Jednou z hlavních změn hello mezi verzemi hello je hello konstruktory už nebudou potřebovat klíč aplikace. Nyní jednoduše předáte v adrese URL hello mobilní aplikace. Například na hello klientů .NET, hello `MobileServiceClient` konstruktor je nyní:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of hello Mobile App
        );

Další informace o instalaci hello nové sady SDK a použitím hello nové struktury prostřednictvím hello odkazy níže:

* [iOS verze 3.0.0 nebo novější](app-service-mobile-ios-how-to-use-client-library.md)
* [Rozhraní .NET (Windows nebo Xamarin) verze 2.0.0 nebo novější](app-service-mobile-dotnet-how-to-use-client-library.md)

Pokud vaše aplikace provede pomocí nabízených oznámení, poznamenejte si hello registrace konkrétní pokyny pro každou platformu, protože byly některé změny existuje také.

Až budete mít hello novou verzi klienta připraven, vyzkoušejte ji proti projektu upgradovaný server. Po ověření, že bude fungovat, můžete vydat novou verzi toocustomers vaší aplikace. Nakonec Jakmile vaši zákazníci mohli dříve prvního tooreceive těchto aktualizací, můžete odstranit hello Mobile Services verzi vaší aplikace. V tomto okamžiku jste zcela upgradovali tooan aplikace služby mobilní aplikace pomocí hello nejnovější Mobile Apps serveru SDK.

<!-- URLs. -->

[portál Azure]: https://portal.azure.com/
[portál Azure classic]: https://manage.windowsazure.com/
[co jsou Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[mobilní aplikace Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[webovou úlohu]: ../app-service-web/websites-webjobs-resources.md
[jak toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[služby App Service – ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Přehled sady SDK serveru .NET]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
