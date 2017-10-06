---
title: "offline synchronizace aaaEnable pro vaši aplikaci pro univerzální platformu Windows (UWP) s Mobile Apps | Microsoft Docs"
description: "Zjistěte, jak toouse mobilní aplikace Azure toocache a synchronizaci dat offline v aplikaci pro univerzální platformu Windows (UWP)."
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: 8fe51773-90de-4014-8a38-41544446d9b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: a9f4ad02e92c2c423f10f07b7f1a4270aafd6c6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-windows-app"></a>Zapnutí offline synchronizace u aplikace pro Windows
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak tooadd offline podporují aplikace tooa univerzální platformu Windows (UWP) pomocí back-end mobilní aplikace Azure. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací – zobrazení, přidávat a upravovat data - i v případě, že není žádné síťové připojení. Změny se ukládají do místní databáze. Jakmile je zařízení hello zpět do režimu online, tyto změny synchronizované s hello vzdálené back-end.

V tomto kurzu aktualizujete projekt aplikace UPW hello z hello kurzu [vytvoření aplikace pro Windows] toosupport hello offline funkce Azure Mobile Apps. Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello data přístup rozšíření balíčky tooyour projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps].

## <a name="requirements"></a>Požadavky
Tento kurz vyžaduje hello následující požadavky:

* Visual Studio 2013 běžící na Windows 8.1 nebo novějším.
* Dokončení [vytvoření aplikace pro Windows][Vytvoření aplikace pro windows].
* [Azure Mobile Services SQLite úložiště][sqlite store nuget]
* [SQLite pro vývoj pro univerzální platformu Windows](http://www.sqlite.org/downloads)

## <a name="update-hello-client-app-toosupport-offline-features"></a>Aktualizovat hello klientských aplikací toosupport offline funkcí
Offline funkce mobilní aplikace Azure umožňuje toointeract s místní databází v případě, že jste ve scénáři s offline. toouse inicializaci tyto funkce ve vaší aplikaci [SyncContext] [ synccontext] tooa místního úložiště. Potom referenční tabulku prostřednictvím hello [IMobileServiceSyncTable][IMobileServiceSyncTable] rozhraní. SQLite slouží jako místní úložiště hello na hello zařízení.

1. Nainstalujte hello [SQLite runtime pro univerzální platformu Windows hello](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).
2. V sadě Visual Studio otevřete hello Správce balíčků NuGet pro projekt aplikace UPW hello, který jste dokončili v hello [vytvoření aplikace pro Windows] kurzu.
    Vyhledání a instalace hello **Microsoft.Azure.Mobile.Client.SQLiteStore** balíček NuGet.
3. V Průzkumníku řešení klikněte pravým tlačítkem na **odkazy** > **přidat odkaz na...** >**Universal Windows** > **rozšíření**, potom povolte obě **SQLite pro univerzální platformu Windows** a **2015 modulu Runtime Visual C++ pro Universal Windows Platform apps**.

    ![Přidat odkaz na SQLite UWP][1]
4. Otevřete soubor MainPage.xaml.cs hello a zrušte komentář u hello `#define OFFLINE_SYNC_ENABLED` definice.
5. V sadě Visual Studio, stiskněte klávesu hello **F5** klíčů toorebuild a spuštění hello klientskou aplikaci. aplikace Hello funguje stejně jako jeho nebyla předtím, než jste povolili offline synchronizace hello. Však hello místní databáze je nyní obsahuje data, která lze použít v případě pomocí offline.

## <a name="update-sync"></a>Aktualizovat toodisconnect aplikace hello z back-end hello
V této části můžete rozdělit hello připojení tooyour mobilní aplikace back-end toosimulate offline situaci. Když přidáte datových položek, vaší obslužné rutiny výjimek zjistíte, že tuto aplikaci hello je v offline režimu. V tomto stavu nové položky přidán do místní hello ukládání a se budou synchronizovat s hello back-end mobilní aplikace při dalším spuštění nabízené v připojeném stavu.

1. Upravte App.xaml.cs v hello sdílený projekt. Komentář hello inicializace hello **MobileServiceClient** a přidejte následující řádek, který používá adresu URL neplatný mobilní aplikace hello:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Můžete také ukazují offline chování zakázáním Wi-Fi a mobilní sítě na zařízení hello nebo použít režim v letadle.
2. Stiskněte klávesu **F5** toobuild a spuštění aplikace hello. Všimněte si vaše synchronizace se nezdařila při obnovení hello při spuštění aplikace.
3. Zadejte nové položky a Všimněte si, že nabízené nezdaří a zobrazí se [CancelledByNetworkError] stav pokaždé, když kliknete na tlačítko **Uložit**. Však nové položky todo hello existovat v místním úložišti hello, dokud se můžou poslat back-end mobilní aplikace toohello.  V produkční aplikace, je-li potlačit tyto výjimky, které se chová hello klientskou aplikaci, pokud je stále připojená back-end mobilní aplikace toohello.
4. Zavření aplikace hello a restartujte ji tooverify že hello nové položky, kterou jste vytvořili jsou trvalé toohello místního úložiště.
5. (Volitelné) V sadě Visual Studio otevřete **Průzkumníka serveru**. Přejděte tooyour databáze v **Azure**->**databází SQL**. Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**. Teď můžete procházet tooyour tabulce databáze SQL a její obsah. Ověřte, že se nezměnil hello data v databázi back-end hello.
6. (Volitelné) Použijte nástroj REST, například aplikaci Fiddler nebo Postman tooquery vaší mobilní back-end, pomocí dotazu GET ve tvaru `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizovat tooreconnect aplikace hello váš back-end mobilní aplikace
V této části je znovu připojit back-end hello aplikace toohello mobilní aplikace. Tyto změny simulovat opětovné připojení sítě na aplikace hello.

Při prvním spuštění aplikace hello, hello `OnNavigatedTo` volání obslužné rutiny události `InitLocalStoreAsync`. Tato metoda volá `SyncAsync` toosync na místní ukládání s databází hello back-end. aplikace Hello pokusí toosync při spuštění.

1. Otevřete v projektu sdíleného hello App.xaml.cs a zrušte komentář u vaší předchozí inicializace `MobileServiceClient` URL toouse hello správné hello mobilní aplikace.
2. Stiskněte klávesu hello **F5** klíčů toorebuild a aplikaci spusťte hello. Hello aplikace synchronizuje místní změny s back-end mobilní aplikace Azure hello pomocí nabízení a vyžadování operace při hello `OnNavigatedTo` spustí obslužnou rutinu události.
3. (Volitelné) Zobrazení hello aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler. Všimněte si hello data umístění byl synchronizován mezi databáze back-end mobilní aplikace Azure hello a hello místní úložiště.
4. V aplikaci hello, klikněte na tlačítko zaškrtnutí hello pole vedle několik položek toocomplete je v místním úložišti hello.

   `UpdateCheckedTodoItem`volání `SyncAsync` položku toosync každý byla dokončena s back-end mobilní aplikace hello. `SyncAsync`volá nabízení a vyžadování. Ale **vždy, když je spustit vyžádání pro tabulku tohoto klienta hello udělal změny, push se vždycky spustí automaticky**. To zaručuje, že všechny tabulky v místním úložišti hello společně s relací zůstaly konzistentní. Toto chování může způsobit neočekávané push.  Další informace o toto chování najdete v tématu [Offline synchronizací dat v Azure Mobile Apps].

## <a name="api-summary"></a>Souhrn rozhraní API
Funkce offline toosupport hello mobilních služeb, použili jsme hello [IMobileServiceSyncTable] rozhraní a inicializovat [MobileServiceClient.SyncContext] [ synccontext] s místní databáze SQLite. Když do režimu offline, hello normální operace CRUD mobilních aplikací pracovat, jako kdyby aplikace hello stále připojený. během operace hello probíhají proti hello místního úložiště. Hello následující metody jsou použité toosynchronize hello místní úložiště s hello serveru:

* **[PushAsync]**  vzhledem k tomu, že tato metoda je členem skupiny [IMobileServicesSyncContext], změny mezi všechny tabulky odesílají toohello back-end. Pouze záznamy s místní změny jsou odesílány toohello serveru.
* **[PullAsync]**  vyžádání se spouští z [IMobileServiceSyncTable]. Pokud je v tabulce hello sledovaných změn, implicitní nabízené běží toomake jistotu, že všechny tabulky v místním úložišti hello společně s relací zůstaly konzistentní. Hello *pushOtherTables* parametr řídí, zda jiné tabulky v kontextu hello odesílají v implicitní push. Hello *dotazu* přebírá parametr [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] nebo data vrácená hello toofilter řetězec dotazu OData. Hello *queryId* parametr se používá toodefine přírůstkové synchronizace. Další informace najdete v tématu [Offline synchronizací dat v Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).
* **[PurgeAsync]**  vaše aplikace by měly volat pravidelně tato metoda toopurge zastaralá data z místního úložiště hello. Použití hello *vynutit* parametr, pokud budete potřebovat toopurge veškeré změny, které dosud nebyly synchronizovány.

Další informace o těchto postupech najdete v tématu [Offline synchronizací dat v Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Další informace
Hello následující témata obsahují další informace o funkci offline synchronizace hello mobilních aplikací:

* [Offline synchronizací dat v Azure Mobile Apps]
* [Mobilní aplikace Azure .NET SDK postupy][8]

<!-- Anchors. -->
[Update hello app toosupport offline features]: #enable-offline-app
[Update hello sync behavior of hello app]: #update-sync
[Update hello app tooreconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Vytvoření aplikace pro windows]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
