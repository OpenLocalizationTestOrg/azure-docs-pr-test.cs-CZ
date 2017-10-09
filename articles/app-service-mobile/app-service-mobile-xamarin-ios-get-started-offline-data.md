---
title: "offline synchronizace aaaEnable mobilní aplikace Azure (Xamarin iOS)"
description: "Zjistěte, jak toouse aplikace služby mobilní aplikace toocache a synchronizaci dat offline v aplikaci Xamarin iOS"
documentationcenter: xamarin
author: ggailey777
manager: cfowler
editor: 
services: app-service\mobile
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5243f2d292377d8de103a40f45d649394f11b97c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Povolení offline synchronizace pro mobilní aplikace Xamarin.iOS
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Přehled
Tento kurz představuje hello offline synchronizace funkci Azure Mobile Apps pro Xamarin.iOS. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací – zobrazení, přidání nebo úprava dat – i když dojde k dispozici žádné síťové připojení. Změny se ukládají do místní databáze. Jakmile hello zařízení do režimu online, tyto změny se synchronizují s hello vzdálené služby.

V tomto kurzu aktualizovat projekt aplikace Xamarin.iOS hello z [vytvoření aplikace pro Xamarin iOS] toosupport hello offline funkce Azure Mobile Apps. Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello data přístup rozšiřující balíčky do projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps].

## <a name="update-hello-client-app-toosupport-offline-features"></a>Aktualizovat hello klientských aplikací toosupport offline funkcí
Offline funkce mobilní aplikace Azure umožňuje toointeract s místní databází v případě, že jste ve scénáři s offline. Inicializace toouse tyto funkce ve vaší aplikaci [SyncContext] tooa místního úložiště. Referenční tabulka prostřednictvím rozhraní hello [IMobileServiceSyncTable]. SQLite slouží jako místní úložiště hello na hello zařízení.

1. Otevřete hello Správce balíčků NuGet v hello projekt, který jste dokončili v hello [vytvoření aplikace pro Xamarin iOS] kurzu potom hledat a instalovat hello **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet balíček.
2. Otevřete soubor QSTodoService.cs hello a zrušte komentář u hello `#define OFFLINE_SYNC_ENABLED` definice.
3. Znovu sestavit a spustit klientskou aplikaci hello. aplikace Hello funguje stejně jako jeho nebyla předtím, než jste povolili offline synchronizace hello. Však hello místní databáze je nyní obsahuje data, která lze použít v případě pomocí offline.

## <a name="update-sync"></a>Aktualizovat toodisconnect aplikace hello z back-end hello
V této části můžete rozdělit hello připojení tooyour mobilní aplikace back-end toosimulate offline situaci. Když přidáte datových položek, vaší obslužné rutiny výjimek zjistíte, že tuto aplikaci hello je v offline režimu. V tomto stavu nové položky přidán do místní hello ukládání a se budou synchronizovat s hello back-end mobilní aplikace při dalším spuštění nabízené v připojeném stavu.

1. Upravte QSToDoService.cs v hello sdílený projekt. Změna hello **applicationURL** toopoint tooan neplatná adresa URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Zakázání Wi-Fi a mobilní sítě na hello zařízení nebo pomocí režim v letadle, může také ukázat offline chování.
2. Sestavení a spuštění aplikace hello. Všimněte si vaše synchronizace se nezdařila při obnovení hello při spuštění aplikace.
3. Zadejte nové položky a Všimněte si, že nabízené nezdaří se stavem [CancelledByNetworkError] pokaždé, když kliknete na tlačítko **Uložit**. Však nové položky todo hello existovat v místním úložišti hello, dokud se můžou poslat back-end mobilní aplikace toohello.  V produkční aplikace, je-li potlačit tyto výjimky, které se chová hello klientskou aplikaci, pokud je stále připojená back-end mobilní aplikace toohello.
4. Zavření aplikace hello a restartujte ji tooverify že hello nové položky, kterou jste vytvořili jsou trvalé toohello místního úložiště.
5. (Volitelné) Pokud máte na počítači nainstalovanou sadu Visual Studio, otevřete **Průzkumníka serveru**. Přejděte tooyour databáze v **Azure**-> **databází SQL**. Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**. Teď můžete procházet tooyour tabulce databáze SQL a její obsah. Ověřte, že se nezměnil hello data v databázi back-end hello.
6. (Volitelné) Použijte nástroj REST, například aplikaci Fiddler nebo Postman tooquery vaší mobilní back-end, pomocí dotazu GET ve tvaru `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizovat tooreconnect aplikace hello váš back-end mobilní aplikace
V této části se znovu připojte back-end hello aplikace toohello mobilní aplikace. To se simuluje aplikace hello přechod ze stavu online tooan offline stavu s back-end mobilní aplikace hello.   Pokud jste simuluje hello sítě poškození vypnutí připojení k síti, je potřeba žádné změny kódu.
Hello sítě znovu zapněte.  Při prvním spuštění aplikace hello, hello `RefreshDataAsync` metoda je volána. Naopak volá `SyncAsync` toosync na místní ukládání s databází hello back-end.

1. Otevřete v projektu sdíleného hello QSToDoService.cs a obnovit vaše změna hello **applicationURL** vlastnost.
2. Opětovné sestavení a spuštění aplikace hello. Hello aplikace synchronizuje místní změny s back-end mobilní aplikace Azure hello pomocí nabízení a vyžadování operace při hello `OnRefreshItemsSelected` metody.
3. (Volitelné) Zobrazení hello aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler. Všimněte si hello data umístění byl synchronizován mezi databáze back-end mobilní aplikace Azure hello a hello místní úložiště.
4. V aplikaci hello, klikněte na tlačítko zaškrtnutí hello pole vedle několik položek toocomplete je v místním úložišti hello.

   `CompleteItemAsync`volání `SyncAsync` položku toosync každý byla dokončena s back-end mobilní aplikace hello. `SyncAsync`volá nabízení a vyžadování.
   **Při každém spuštění přijetí změn pro tabulku tohoto klienta hello udělal změny, push na kontext synchronizace klienta hello je vždy spustit nejprve automaticky**. implicitní nabízené Hello zajistí, že všechny tabulky v místním úložišti hello společně s relací zůstaly konzistentní. Další informace o toto chování najdete v tématu [Offline synchronizací dat v Azure Mobile Apps].

## <a name="review-hello-client-sync-code"></a>Zkontrolujte kód synchronizace klienta hello
Hello Xamarin klientského projektu, který jste stáhli, když jste dokončili kurz hello [vytvoření aplikace pro Xamarin iOS] již obsahuje kód podpora offline synchronizace pomocí místní databáze SQLite. Zde je stručný přehled co je již součástí hello kódu. Koncepční přehled hello funkce, najdete v tématu [Offline synchronizací dat v Azure Mobile Apps].

* Před provedením jakékoli operace s tabulkou, musí být inicializován hello místního úložiště. Hello místního úložiště databáze inicializovaná při `QSTodoListViewController.ViewDidLoad()` provede `QSTodoService.InitializeStoreAsync()`. Tato metoda vytvoří novou místní databázi SQLite pomocí hello `MobileServiceSQLiteStore` třída poskytované hello mobilní aplikace Azure klienta SDK.

    Hello `DefineTable` metoda vytvoří tabulku v hello místního úložiště, který odpovídá hello polí v hello zadaný typ, `ToDoItem` v tomto případě. Typ Hello nemá tooinclude všechny hello sloupců, které jsou v hello vzdálené databáze. Je možné toostore pouze podmnožinu sloupců.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses hello default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* Hello `todoTable` členem `QSTodoService` je hello `IMobileServiceSyncTable` zadejte místo `IMobileServiceTable`. Hello IMobileServiceSyncTable směrovat všechny vytvářet, číst, aktualizovat a odstraňovat (CRUD) tabulky operations toohello místního úložiště databáze.

    Rozhodnete, když jsou tyto změny nabídnutých back-end mobilní aplikace Azure toohello voláním `IMobileServiceSyncContext.PushAsync()`. Hello kontext synchronizace umožňuje zachovat relace mezi tabulkami sledováním a vkládání změny ve všech tabulkách klientské aplikace změnil při `PushAsync` je volána.

    Hello zadat kód zavolá metodu `QSTodoService.SyncAsync()` toosync pokaždé, když se aktualizují hello todoitem seznamu nebo úkolu je přidána nebo byla dokončena. Synchronizace aplikace po každé změně místní. Pokud vyžádání spouští před tabulku, která má čekající místní aktualizace sledovat hello kontextu, že vyžádanou operaci automaticky aktivují push kontextu nejdřív.

    V kódu hello poskytuje všechny zaznamenává hello vzdálené `TodoItem` tabulky jsou předmětem dotazování, ale je také možné toofilter záznamy předáním id dotazu a dotaz příliš`PushAsync`. Další informace najdete v tématu hello *přírůstkové synchronizace* v [Offline synchronizací dat v Azure Mobile Apps].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

## <a name="additional-resources"></a>Další zdroje
* [Offline synchronizací dat v Azure Mobile Apps]
* [Mobilní aplikace Azure .NET SDK postupy][8]

<!-- Images -->

<!-- URLs. -->
[vytvoření aplikace pro Xamarin iOS]: app-service-mobile-xamarin-ios-get-started.md
[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
