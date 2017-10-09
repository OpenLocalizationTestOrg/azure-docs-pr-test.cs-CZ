---
title: "offline synchronizace aaaEnable mobilní aplikace Azure (Cordova) | Microsoft Docs"
description: "Zjistěte, jak toouse aplikace služby mobilní aplikace toocache a synchronizaci dat offline v aplikaci Cordova"
documentationcenter: cordova
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 4e6ae96c3d96dac8ebb3749354b83a04686831b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Zapnutí offline synchronizace pro mobilní aplikaci Cordova
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Tento kurz představuje hello offline synchronizace funkci Azure Mobile Apps pro Cordova. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení. Změny se ukládají do místní databáze.  Jakmile hello zařízení do režimu online, tyto změny se synchronizují s hello vzdálené služby.

V tomto kurzu vychází hello Cordova rychlý start řešení pro mobilní aplikace, které vytvoříte po dokončení hello kurzu [Apache Cordova úvodní]. V tomto kurzu aktualizujete hello rychlý start řešení tooadd offline funkce Azure Mobile Apps.  Také jsme zvýrazněte hello offline konkrétní kódu v aplikaci hello.

toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps]. Podrobnosti využití rozhraní API najdete v tématu hello [dokumentaci k rozhraní API](https://azure.github.io/azure-mobile-apps-js-client).

## <a name="add-offline-sync-toohello-quickstart-solution"></a>Přidat řešení rychlý start toohello offline synchronizace
Hello offline synchronizace kód musí být přidaný toohello aplikace. Offline synchronizace vyžaduje hello úložiště sqlite cordova-plugin, který automaticky přidá tooyour aplikace při hello plugin Azure Mobile Apps je součástí projektu hello. projekt Hello rychlý start zahrnuje obě tyto moduly plug-in.

1. V Průzkumníku řešení v sadě Visual Studio otevřete index.js a nahraďte hello následující kód

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable;      // Reference tooa table endpoint on backend

    s tímto kódem:

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable,      // Reference tooa table endpoint on backend
           syncContext;        // Reference toooffline data sync context

2. Potom nahraďte hello následující kód:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    s tímto kódem:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              complete: 'boolean',
              version: 'string'
          }
        });

        // Get hello sync context from hello client
        syncContext = client.getSyncContext();

    Hello předchozí kód přidání inicializovat hello místního úložiště a definovat místní tabulku, která odpovídá hello sloupec hodnoty používané ve vaší službě Azure back-end. (Není potřeba tooinclude všechny hodnoty ve sloupcích v tento kód.)  Hello `version` pole se spravuje pomocí mobilního back-endu hello a používá se pro řešení konfliktů.

    Získat kontext synchronizace toohello odkaz voláním **getSyncContext**. Hello kontext synchronizace umožňuje zachovat relace mezi tabulkami sledováním a vkládání změny ve všech tabulkách klientské aplikace změnil při `.push()` je volána.

3. Aktualizujte hello aplikace URL tooyour mobilní aplikace adresu URL aplikace.

4. Potom nahraďte tento kód:

        todoItemTable = client.getTable('todoitem'); // todoitem is hello table name

    s tímto kódem:

        // Initialize hello sync context with hello store
        syncContext.initialize(store).then(function () {

        // Get hello local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle hello conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert tooserver's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle hello error
                  // In hello simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function tooperform hello actual sync
        syncBackend();

        // Refresh hello todoItems
        refreshDisplay();

        // Wire up hello UI Event Handler for hello Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Hello předchozí kód inicializuje kontext synchronizace hello a pak zavolá tooget getSyncTable (namísto jít) na místní tabulku toohello odkaz.

    Tento kód používá hello místní databáze pro všechny vytvářet, číst, aktualizovat a odstranit tabulku operace.

    Tato ukázka provádí zpracování na konflikty synchronizace jednoduché chyb. Reálné aplikaci byste zpracování hello různých chybám, jako je síťové podmínky, server je v konfliktu a dalších. Příklady kódu najdete v tématu hello [offline synchronizace ukázka].

5. Dál přidejte funkce skutečného synchronizace tooperform hello tento.

        function syncBackend() {

          // Sync local store tooAzure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from hello Azure table after syncing tooAzure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Rozhodnete, že když toopush změny back-end mobilní aplikace toohello voláním **syncContext.push()**. Například může volat **syncBackend** v tlačítko Synchronizovat vázanou tooa tlačítko událost obslužné rutiny.

## <a name="offline-sync-considerations"></a>Důležité informace o offline synchronizace

V ukázce hello hello **nabízené** metodu **syncContext** je volána pouze při spuštění aplikace v hello funkce zpětného volání pro přihlášení.  V reálné aplikaci může také mít tato funkce synchronizaci spustit ručně nebo při změně stavu sítě hello.

Když je spustit vyžádání pro tabulku, která má čekající místní aktualizace sleduje hello kontext, který pull operace automaticky aktivuje push. Při aktualizaci, přidání a dokončení položky v této ukázce, můžete vynechat hello explicitní **nabízené** volat, protože je redundantní.

V kódu hello poskytuje, jsou předmětem dotazování všechny záznamy v tabulce vzdálené todoItem hello, ale je také možné toofilter záznamy předáním id dotazu a dotaz příliš**nabízené**. Další informace najdete v tématu hello *přírůstkové synchronizace* v [Offline synchronizací dat v Azure Mobile Apps].

## <a name="optional-disable-authentication"></a>(Volitelné) Zakázat ověřování

Pokud nemáte má tooset až ověřování před testováním offline synchronizace, komentář hello funkce zpětného volání pro přihlášení, ale ponechte hello kód uvnitř uncommented hello funkce zpětného volání.  Po komentářů hello přihlášení řádky, následuje hello kódu:

      // Login toohello service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave hello rest of hello code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Nyní hello s hello Azure back-end při spuštění aplikace hello synchronizacemi aplikace.

## <a name="run-hello-client-app"></a>Spustit klientskou aplikaci hello
S offline synchronizací teď povolená můžete spustit hello klientská aplikace alespoň jednou na každou platformu k naplnění databáze hello místního úložiště. Později simulovat offline scénář a upravit data hello v místním úložišti hello aplikace hello je offline.

## <a name="optional-test-hello-sync-behavior"></a>(Volitelné) Test hello synchronizace chování
V této části upravíte hello klienta projektu toosimulate offline scénář pomocí adresy URL Neplatná aplikace pro váš back-end. Při přidání nebo změna datových položek, tyto změny jsou uložené v místním úložišti, ale nejsou synchronizovaná toohello back-end úložišti, dokud nebude znovu navázané připojení hello.

1. V Průzkumníku řešení hello otevřete soubor projektu index.js hello a změňte toopoint adresu URL aplikace hello neplatná adresa URL, jako je hello následující kód:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. V index.html, aktualizace hello CSP `<meta>` element s hello stejné neplatná adresa URL.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Sestavit a spustit hello klientskou aplikaci a Všimněte si, že výjimku protokolu konzole hello když aplikace hello pokusí synchronizovat s back-end hello po přihlášení. Všechny nové položky, které přidáte existovat pouze v místním úložišti hello, dokud se odesílají toohello mobilního back-endu. klientská aplikace Hello se chová, pokud je připojený toohello back-end.

4. Zavření aplikace hello a restartujte ji tooverify že hello nové položky, kterou jste vytvořili jsou trvalé toohello místního úložiště.

5. (Volitelné) Pomocí sady Visual Studio tooview vaše toosee tabulky Azure SQL Database, nezměnil hello data v databázi back-end hello.

    V sadě Visual Studio otevřete **Průzkumníka serveru**. Přejděte tooyour databáze v **Azure**->**databází SQL**. Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**. Teď můžete procházet tooyour tabulce databáze SQL a její obsah.

## <a name="optional-test-hello-reconnection-tooyour-mobile-backend"></a>(Volitelné) Test hello opětovné připojení tooyour mobilní back-end

V této části je znovu připojit hello toohello mobilní back-end aplikace, který simuluje hello aplikace vracející se zpět do stavu online. Při přihlášení, data jsou synchronizovaná tooyour mobilního back-endu.

1. Index.js znovu otevřete a obnovte hello adresa URL aplikace.
2. Otevřete index.html a opravte hello adresa URL aplikace v hello CSP `<meta>` elementu.
3. Znovu sestavit a spustit klientskou aplikaci hello. Hello toosync pokusy o aplikaci s back-end mobilní aplikace hello po přihlášení. Ověřte, že žádné výjimky jsou zaznamenány do konzoly ladění hello.
4. (Volitelné) Zobrazení hello aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler. Všimněte si hello data umístění byl synchronizován mezi hello back-endovou databázi a hello místního úložiště.

    Všimněte si hello data umístění byl synchronizován mezi hello databáze a místní úložiště hello a obsahuje hello položky, které jste přidali v době, kdy byl odpojen vaší aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Offline synchronizací dat v Azure Mobile Apps]
* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Další kroky
* Kontrola více rozšířené funkce offline synchronizace, jako je řešení konfliktů v hello [offline synchronizace ukázka]
* Zkontrolujte hello offline synchronizace referenční dokumentace rozhraní API v hello [dokumentaci k rozhraní API](https://azure.github.io/azure-mobile-apps-js-client).

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova úvodní]: app-service-mobile-cordova-get-started.md
[offline synchronizace ukázka]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with hello .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
