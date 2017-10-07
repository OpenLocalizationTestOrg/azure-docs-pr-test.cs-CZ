---
title: "offline synchronizace aaaEnable mobilní aplikace Azure (Xamarin Android)"
description: "Zjistěte, jak toouse aplikace služby mobilní aplikace toocache a synchronizaci dat offline v aplikaci Xamarin Android"
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 91d59e4b-abaa-41f4-80cf-ee7933b32568
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 216ba76ae49f583273cc61b63114a415eca2477b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Zapnutí offline synchronizace pro mobilní aplikace Xamarin.Android
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Přehled
Tento kurz představuje hello offline synchronizace funkci Azure Mobile Apps pro Xamarin.Android. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací – zobrazení, přidání nebo úprava dat – i když dojde k dispozici žádné síťové připojení. Změny se ukládají do místní databáze.
Jakmile hello zařízení do režimu online, tyto změny se synchronizují s hello vzdálené služby.

V tomto kurzu aktualizujete hello projektu klienta z hello kurzu [vytvoření aplikace Xamarin.Android] toosupport hello offline funkce Azure Mobile Apps. Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello data přístup rozšíření balíčky tooyour projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps].

## <a name="update-hello-client-app-toosupport-offline-features"></a>Aktualizovat hello klientských aplikací toosupport offline funkcí
Offline funkce mobilní aplikace Azure umožňuje toointeract s místní databází v případě, že jste ve scénáři s offline. toouse inicializaci tyto funkce ve vaší aplikaci [SyncContext] tooa místního úložiště. Potom referenční tabulku prostřednictvím hello [IMobileServiceSyncTable] [IMobileServiceSyncTable] rozhraní. SQLite slouží jako místní úložiště hello na hello zařízení.

1. V sadě Visual Studio, otevřete Správce balíčků NuGet hello v hello projekt, který jste dokončili v hello [vytvoření aplikace Xamarin.Android] kurzu.  Vyhledání a instalace hello **Microsoft.Azure.Mobile.Client.SQLiteStore** balíček NuGet.
2. Otevřete soubor ToDoActivity.cs hello a zrušte komentář u hello `#define OFFLINE_SYNC_ENABLED` definice.
3. V sadě Visual Studio, stiskněte klávesu hello **F5** klíčů toorebuild a spuštění hello klientskou aplikaci. aplikace Hello funguje stejně jako jeho nebyla předtím, než jste povolili offline synchronizace hello. Však hello místní databáze je nyní obsahuje data, která lze použít v případě pomocí offline.

## <a name="update-sync"></a>Aktualizovat toodisconnect aplikace hello z back-end hello
V této části můžete rozdělit hello připojení tooyour mobilní aplikace back-end toosimulate offline situaci. Když přidáte datových položek, vaší obslužné rutiny výjimek zjistíte, že tuto aplikaci hello je v offline režimu. V tomto stavu nové položky přidán do místní hello ukládání a jsou synchronizované s back-end mobilní aplikace hello při push v připojeném stavu.

1. Upravte ToDoActivity.cs v hello sdílený projekt. Změna hello **applicationURL** toopoint tooan neplatná adresa URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Zakázání Wi-Fi a mobilní sítě na hello zařízení nebo pomocí režim v letadle, může také ukázat offline chování.
2. Stiskněte klávesu **F5** toobuild a spuštění aplikace hello. Všimněte si vaše synchronizace se nezdařila při obnovení hello při spuštění aplikace.
3. Zadejte nové položky a Všimněte si, že nabízené nezdaří se stavem [CancelledByNetworkError] pokaždé, když kliknete na tlačítko **Uložit**. Však nové položky todo hello existovat v místním úložišti hello, dokud se můžou poslat back-end mobilní aplikace toohello.  V produkční aplikace, je-li potlačit tyto výjimky, které se chová hello klientskou aplikaci, pokud je stále připojená back-end mobilní aplikace toohello.
4. Zavření aplikace hello a restartujte ji tooverify že hello nové položky, kterou jste vytvořili jsou trvalé toohello místního úložiště.
5. (Volitelné) V sadě Visual Studio otevřete **Průzkumníka serveru**. Přejděte tooyour databáze v **Azure**->**databází SQL**. Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**. Teď můžete procházet tooyour tabulce databáze SQL a její obsah. Ověřte, že se nezměnil hello data v databázi back-end hello.
6. (Volitelné) Použijte nástroj REST, například aplikaci Fiddler nebo Postman tooquery vaší mobilní back-end, pomocí dotazu GET ve tvaru `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizovat tooreconnect aplikace hello váš back-end mobilní aplikace
V této části se znovu připojte back-end hello aplikace toohello mobilní aplikace. Při prvním spuštění aplikace hello, hello `OnCreate` volání obslužné rutiny události `OnRefreshItemsSelected`. Tato metoda volá `SyncAsync` toosync na místní ukládání s databází hello back-end.

1. Otevřete v projektu sdíleného hello ToDoActivity.cs a obnovit vaše změna hello **applicationURL** vlastnost.
2. Stiskněte klávesu hello **F5** klíčů toorebuild a aplikaci spusťte hello. Hello aplikace synchronizuje místní změny s back-end mobilní aplikace Azure hello pomocí nabízení a vyžadování operace při hello `OnRefreshItemsSelected` metody.
3. (Volitelné) Zobrazení hello aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler. Všimněte si hello data umístění byl synchronizován mezi databáze back-end mobilní aplikace Azure hello a hello místní úložiště.
4. V aplikaci hello, klikněte na tlačítko zaškrtnutí hello pole vedle několik položek toocomplete je v místním úložišti hello.

   `CheckItem`volání `SyncAsync` položku toosync každý byla dokončena s back-end mobilní aplikace hello. `SyncAsync`volá nabízení a vyžadování. **Při každém spuštění přijetí změn pro tabulku tohoto klienta hello udělal změny, push se vždycky spustí automaticky**. Tím se zajistí, že všechny tabulky v místním úložišti společně s vztahy zůstaly konzistentní. Toto chování může způsobit neočekávané push. Další informace o toto chování najdete v tématu [Offline synchronizací dat v Azure Mobile Apps].

## <a name="review-hello-client-sync-code"></a>Zkontrolujte kód synchronizace klienta hello
Hello Xamarin klientského projektu, který jste stáhli, když jste dokončili kurz hello [vytvoření aplikace Xamarin.Android] již obsahuje kód podpora offline synchronizace pomocí místní databáze SQLite. Zde je stručný přehled co je již součástí hello kódu. Koncepční přehled hello funkce, najdete v tématu [Offline synchronizací dat v Azure Mobile Apps].

* Před provedením jakékoli operace s tabulkou, musí být inicializován hello místního úložiště. Hello místního úložiště databáze inicializovaná při `ToDoActivity.OnCreate()` provede `ToDoActivity.InitLocalStoreAsync()`. Tato metoda vytvoří místní databázi SQLite pomocí hello `MobileServiceSQLiteStore` třída poskytované hello Azure Mobile Apps klienta SDK.

    Hello `DefineTable` metoda vytvoří tabulku v hello místního úložiště, který odpovídá hello polí v hello zadaný typ, `ToDoItem` v tomto případě. Typ Hello nemá tooinclude všechny hello sloupců, které jsou v hello vzdálené databáze. Je možné toostore pouze podmnožinu sloupců.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code tooinitialize hello SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses hello default conflict handler, which fails on conflict
            // toouse a different conflict handler, pass a parameter tooInitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }
* Hello `toDoTable` členem `ToDoActivity` je hello `IMobileServiceSyncTable` zadejte místo `IMobileServiceTable`. Hello IMobileServiceSyncTable směrovat všechny vytvářet, číst, aktualizovat a odstraňovat (CRUD) tabulky operations toohello místního úložiště databáze.

    Rozhodnete, když jsou změny nabídnutých back-end mobilní aplikace Azure toohello voláním `IMobileServiceSyncContext.PushAsync()`. Hello kontext synchronizace umožňuje zachovat relace mezi tabulkami sledováním a vkládání změny ve všech tabulkách klientské aplikace změnil při `PushAsync` je volána.

    Hello zadat kód zavolá metodu `ToDoActivity.SyncAsync()` toosync pokaždé, když se aktualizují hello todoitem seznamu nebo úkolu je přidána nebo byla dokončena. Hello synchronizacemi kódu po každé změně místní.

    V kódu hello poskytuje všechny zaznamenává hello vzdálené `TodoItem` tabulky jsou předmětem dotazování, ale je také možné toofilter záznamy předáním id dotazu a dotaz příliš`PushAsync`. Další informace najdete v tématu hello *přírůstkové synchronizace* v [Offline synchronizací dat v Azure Mobile Apps].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating hello Mobile Service. Verify hello URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Další zdroje
* [Offline synchronizací dat v Azure Mobile Apps]
* [Mobilní aplikace Azure .NET SDK postupy][8]

<!-- URLs. -->
[vytvoření aplikace Xamarin.Android]: ../app-service-mobile-xamarin-android-get-started.md
[Offline synchronizací dat v Azure Mobile Apps]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Vytvoření aplikace Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
[Synchronizace offline dat v prostředí Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
