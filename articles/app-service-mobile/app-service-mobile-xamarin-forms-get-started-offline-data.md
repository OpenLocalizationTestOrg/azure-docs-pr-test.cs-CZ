---
title: "offline synchronizace aaaEnable mobilní aplikace Azure (Xamarin.Forms) | Microsoft Docs"
description: "Zjistěte, jak toouse aplikace služby mobilní aplikace toocache a synchronizaci dat offline v aplikaci Xamarin.Forms"
documentationcenter: xamarin
author: ggailey777
manager: yochayk
editor: 
services: app-service\mobile
ms.assetid: acf0f874-3ea5-4410-bd22-b0e72140f3b5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: glenga
ms.openlocfilehash: 4b41404fc9507e82068fdf8aa612d57cbeb366bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Povolení offline synchronizace pro mobilní aplikace Xamarin.Forms
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Přehled
Tento kurz představuje hello offline synchronizace funkci Azure Mobile Apps pro Xamarin.Forms. Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací – zobrazení, přidání nebo úprava dat – i když dojde k dispozici žádné síťové připojení. Změny se ukládají do místní databáze. Jakmile hello zařízení do režimu online, tyto změny se synchronizují s hello vzdálené služby.

V tomto kurzu vychází hello rychlé spuštění řešení Xamarin.Forms pro mobilní aplikace, kterou vytvoříte po dokončení tohoto kurzu [vytvoření aplikace pro Xamarin iOS]. Hello rychlé spuštění řešení Xamarin.Forms obsahuje offline synchronizace toosupport se kód hello, který se právě musí toobe povolena. V tomto kurzu aktualizujete hello rychlý start řešení tooturn na hello offline funkce Azure Mobile Apps. Také jsme zvýrazněte hello offline konkrétní kódu v aplikaci hello. Pokud nepoužijete hello stáhli rychlý start řešení, musíte přidat hello data přístup rozšíření balíčky tooyour projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps][1].

toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps][2].

## <a name="enable-offline-sync-functionality-in-hello-quickstart-solution"></a>Povolit funkci offline synchronizace v řešení hello rychlý start
Hello offline synchronizace kódu je součástí projektu hello pomocí C# direktivy preprocesoru. Když hello **OFFLINE\_SYNCHRONIZACE\_povoleno** symbol definován, tyto cesty kódu jsou součástí sestavení hello. Pro aplikace pro Windows musíte také nainstalovat hello SQLite platformy.

1. V sadě Visual Studio, klikněte pravým tlačítkem na řešení hello > **spravovat balíčky NuGet pro řešení...** , vyhledejte a nainstalujte **Microsoft.Azure.Mobile.Client.SQLiteStore** balíček NuGet pro všechny projekty v řešení hello.
2. V Průzkumníku řešení hello, otevřete soubor TodoItemManager.cs hello z projektu hello s **přenosné** v názvu text hello, což je projektu knihovny přenosných tříd, poté zrušte komentář u následující direktivy preprocesoru hello:

        #define OFFLINE_SYNC_ENABLED
3. (Volitelné) toosupport zařízení se systémem Windows, instalovat jeden z následujících balíčků runtime SQLite hello:

   * **Modul Runtime pro Windows 8.1:** nainstalovat [SQLite pro Windows 8.1][3].
   * **Windows Phone 8.1:** nainstalovat [SQLite pro Windows Phone 8.1][4].
   * **Univerzální platformu Windows** nainstalovat [SQLite pro univerzální univerzální pro Windows hello][5].

     I když rychlý start hello neobsahuje Universal Windows project, hello univerzální platformy Windows jsou podporovány pro Xamarin Forms.
4. (Volitelné) V každé projekt aplikace Windows, klikněte pravým tlačítkem na **odkazy** > **přidat odkaz na...** , rozbalte položku hello **Windows** složky > **rozšíření**.
    Povolit hello odpovídající **SQLite pro systém Windows** SDK společně s hello **Visual C++ 2013 modulu Runtime pro Windows** SDK.
    Hello SQLite SDK názvy jsou mírně odlišné s každou platformu Windows.

## <a name="review-hello-client-sync-code"></a>Zkontrolujte kód synchronizace klienta hello
Zde je stručný přehled co je již součástí hello kódu uvnitř hello `#if OFFLINE_SYNC_ENABLED` direktivy. Funkci offline synchronizace je v souboru projektu hello TodoItemManager.cs v projektu knihovny přenosných tříd hello. Koncepční přehled hello funkce, najdete v tématu [Offline synchronizací dat v Azure Mobile Apps][2].

* Před provedením jakékoli operace s tabulkou, musí být inicializován hello místního úložiště. Hello místního úložiště databáze je v inicializovat hello **TodoItemManager** konstruktoru třídy pomocí hello následující kód:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Tento kód vytvoří novou místní databázi SQLite pomocí hello **MobileServiceSQLiteStore** třídy.

    Hello **DefineTable** metoda vytvoří tabulku v hello místního úložiště, který odpovídá hello polí v hello zadaný typ.  Typ Hello nemá tooinclude všechny hello sloupců, které jsou v hello vzdálené databáze. Je možné toostore podmnožinu sloupců.
* Hello **todoTable** pole **TodoItemManager** je **IMobileServiceSyncTable** zadejte místo **IMobileServiceTable**. Tato třída používá hello místní databáze pro všechny vytvářet, číst, aktualizovat a odstranit tabulku operace. Rozhodnete, když jsou tyto změny nabídnutých back-end mobilní aplikace toohello voláním **PushAsync** na hello **IMobileServiceSyncContext**. Hello kontext synchronizace umožňuje zachovat relace mezi tabulkami sledováním a vkládání změny ve všech tabulkách klientské aplikace změnil při **PushAsync** je volána.

    Následující Hello **SyncAsync** metoda je volána toosync s back-end mobilní aplikace hello:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting tooserver's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Tato ukázka používá jednoduchý s hello výchozí synchronizace obslužnou rutinu pro zpracování chyb. Reálné aplikaci byste zpracování hello různých chybám, jako jsou síťové podmínky a server je v konfliktu s použitím vlastní **IMobileServiceSyncHandler** implementace.

## <a name="offline-sync-considerations"></a>Důležité informace o offline synchronizace
V ukázce hello hello **SyncAsync** metoda je volána pouze na spuštění a pokud je požadována synchronizace.  tooinitiate synchronizace v aplikaci Android nebo iOS vyžádání dolů v seznamu položek hello; pro Windows, použijte hello **synchronizace** tlačítko. V reálné aplikaci může způsobují hello synchronizace aktivační událost při změně stavu sítě hello.

Když je spustit vyžádání pro tabulku, která má čekající místní aktualizace sleduje hello kontext, který pull operace automaticky aktivuje předchozí push kontextu. Při aktualizaci, přidání a dokončení položky v této ukázce, můžete vynechat hello explicitní **PushAsync** volání.

V kódu hello poskytuje, jsou předmětem dotazování všechny záznamy v hello vzdálenou tabulku TodoItem, ale je také možné toofilter záznamy předáním id dotazu a dotaz příliš**PushAsync**. Další informace najdete v tématu hello *přírůstkové synchronizace* v [Offline synchronizací dat v Azure Mobile Apps][2].

## <a name="run-hello-client-app"></a>Spustit klientskou aplikaci hello
S offline synchronizací teď povolené spusťte klientskou aplikaci hello alespoň jednou na každou databázi platformy toopopulate hello místního úložiště. Později simulovat offline scénář a upravit data hello v místním úložišti hello aplikace hello je offline.

## <a name="update-hello-sync-behavior-of-hello-client-app"></a>Aktualizace chování synchronizace hello hello klientské aplikace
V této části upravte hello klienta projektu toosimulate offline scénář pomocí adresy URL Neplatná aplikace pro váš back-end. Alternativně můžete vypnout připojení k síti přesunutím zařízení příliš "Režim v letadle."  Při přidání nebo změna datových položek, jsou tyto změny uchovávat v místním úložišti hello, ale položka nebyla synchronizována toohello back-end datové úložiště, dokud nebude znovu navázané připojení hello.

1. V Průzkumníku řešení hello, otevřete soubor projektu Constants.cs hello z hello **přenosné** projektu a změnit hodnotu hello `ApplicationURL` toopoint tooan neplatná adresa URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. Otevřete hello TodoItemManager.cs soubor z hello **přenosné** projektu a pak přidejte **catch** pro hello základní **výjimka** toohello třída **try... catch** blokovat **SyncAsync**. To **catch** bloku zapíše hello výjimka zpráva toohello konzoly, následujícím způsobem:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. Sestavení a spuštění klienta aplikace hello.  Přidáte nové položky. Všimněte si, že hello konzoly pro každý pokus o toosync s back-end hello protokolu výjimku. Tyto nové položky existovat pouze v místním úložišti hello, dokud se můžou poslat toohello mobilního back-endu. klientská aplikace Hello se chová, pokud je připojený toohello back-end, podpora všechny vytvářet, číst, update, operace odstranění (CRUD).
4. Zavření aplikace hello a restartujte ji tooverify že hello nové položky, kterou jste vytvořili jsou trvalé toohello místního úložiště.
5. (Volitelné) Pomocí sady Visual Studio tooview vaše toosee tabulky Azure SQL Database, nezměnil hello data v databázi back-end hello.

    V sadě Visual Studio otevřete **Průzkumníka serveru**. Přejděte tooyour databáze v **Azure**->**databází SQL**. Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**. Teď můžete procházet tooyour tabulce databáze SQL a její obsah.

## <a name="update-hello-client-app-tooreconnect-your-mobile-backend"></a>Aktualizovat hello klienta aplikace tooreconnect mobilního back-endu
V této části znovu se připojte hello toohello mobilní back-end aplikace, který simuluje hello aplikace vracející se zpět tooan stavu online. Při provádění gesto hello aktualizace dat je synchronizovaná tooyour mobilního back-endu.

1. Znovu otevřete Constants.cs. Správné hello `applicationURL` toopoint toohello opravte adresu URL.
2. Znovu sestavit a spustit klientskou aplikaci hello. Hello toosync pokusy o aplikaci s back-end mobilní aplikace hello po spuštění. Ověřte, že žádné výjimky jsou zaznamenány do konzoly ladění hello.
3. (Volitelné) Zobrazení hello aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler nebo [Postman][6]. Všimněte si hello data umístění byl synchronizován mezi hello back-endovou databázi a hello místního úložiště.

    Všimněte si hello data umístění byl synchronizován mezi hello databáze a místní úložiště hello a obsahuje hello položky, které jste přidali v době, kdy byl odpojen vaší aplikace.

## <a name="additional-resources"></a>Další zdroje
* [Synchronizaci dat offline v Azure Mobile Apps][2]
* [Mobilní aplikace Azure .NET SDK postupy][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
