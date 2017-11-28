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
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a><span data-ttu-id="97290-103">Povolení offline synchronizace pro mobilní aplikace Xamarin.Forms</span><span class="sxs-lookup"><span data-stu-id="97290-103">Enable offline sync for your Xamarin.Forms mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="97290-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="97290-104">Overview</span></span>
<span data-ttu-id="97290-105">Tento kurz představuje hello offline synchronizace funkci Azure Mobile Apps pro Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="97290-105">This tutorial introduces hello offline sync feature of Azure Mobile Apps for Xamarin.Forms.</span></span> <span data-ttu-id="97290-106">Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací – zobrazení, přidání nebo úprava dat – i když dojde k dispozici žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="97290-106">Offline sync allows end users to interact with a mobile app--viewing, adding, or modifying data--even when there is no network connection.</span></span> <span data-ttu-id="97290-107">Změny se ukládají do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="97290-107">Changes are stored in a local database.</span></span> <span data-ttu-id="97290-108">Jakmile hello zařízení do režimu online, tyto změny se synchronizují s hello vzdálené služby.</span><span class="sxs-lookup"><span data-stu-id="97290-108">Once hello device is back online, these changes are synced with hello remote service.</span></span>

<span data-ttu-id="97290-109">V tomto kurzu vychází hello rychlé spuštění řešení Xamarin.Forms pro mobilní aplikace, kterou vytvoříte po dokončení tohoto kurzu [vytvoření aplikace pro Xamarin iOS].</span><span class="sxs-lookup"><span data-stu-id="97290-109">This tutorial is based on hello Xamarin.Forms quickstart solution for Mobile Apps that you create when you complete the tutorial [Create a Xamarin iOS app].</span></span> <span data-ttu-id="97290-110">Hello rychlé spuštění řešení Xamarin.Forms obsahuje offline synchronizace toosupport se kód hello, který se právě musí toobe povolena.</span><span class="sxs-lookup"><span data-stu-id="97290-110">hello quickstart solution for Xamarin.Forms contains hello code toosupport offline sync, which just needs toobe enabled.</span></span> <span data-ttu-id="97290-111">V tomto kurzu aktualizujete hello rychlý start řešení tooturn na hello offline funkce Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="97290-111">In this tutorial, you update hello quickstart solution tooturn on hello offline features of Azure Mobile Apps.</span></span> <span data-ttu-id="97290-112">Také jsme zvýrazněte hello offline konkrétní kódu v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="97290-112">We also highlight hello offline-specific code in hello app.</span></span> <span data-ttu-id="97290-113">Pokud nepoužijete hello stáhli rychlý start řešení, musíte přidat hello data přístup rozšíření balíčky tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="97290-113">If you do not use hello downloaded quickstart solution, you must add hello data access extension packages tooyour project.</span></span> <span data-ttu-id="97290-114">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="97290-114">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps][1].</span></span>

<span data-ttu-id="97290-115">toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="97290-115">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="enable-offline-sync-functionality-in-hello-quickstart-solution"></a><span data-ttu-id="97290-116">Povolit funkci offline synchronizace v řešení hello rychlý start</span><span class="sxs-lookup"><span data-stu-id="97290-116">Enable offline sync functionality in hello quickstart solution</span></span>
<span data-ttu-id="97290-117">Hello offline synchronizace kódu je součástí projektu hello pomocí C# direktivy preprocesoru.</span><span class="sxs-lookup"><span data-stu-id="97290-117">hello offline sync code is included in hello project by using C# preprocessor directives.</span></span> <span data-ttu-id="97290-118">Když hello **OFFLINE\_SYNCHRONIZACE\_povoleno** symbol definován, tyto cesty kódu jsou součástí sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="97290-118">When hello **OFFLINE\_SYNC\_ENABLED** symbol is defined, these code paths are included in hello build.</span></span> <span data-ttu-id="97290-119">Pro aplikace pro Windows musíte také nainstalovat hello SQLite platformy.</span><span class="sxs-lookup"><span data-stu-id="97290-119">For Windows apps, you must also install hello SQLite platform.</span></span>

1. <span data-ttu-id="97290-120">V sadě Visual Studio, klikněte pravým tlačítkem na řešení hello > **spravovat balíčky NuGet pro řešení...** , vyhledejte a nainstalujte **Microsoft.Azure.Mobile.Client.SQLiteStore** balíček NuGet pro všechny projekty v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="97290-120">In Visual Studio, right-click hello solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in hello solution.</span></span>
2. <span data-ttu-id="97290-121">V Průzkumníku řešení hello, otevřete soubor TodoItemManager.cs hello z projektu hello s **přenosné** v názvu text hello, což je projektu knihovny přenosných tříd, poté zrušte komentář u následující direktivy preprocesoru hello:</span><span class="sxs-lookup"><span data-stu-id="97290-121">In hello Solution Explorer, open hello TodoItemManager.cs file from hello project with **Portable** in hello name, which is Portable Class Library project, then uncomment hello following preprocessor directive:</span></span>

        #define OFFLINE_SYNC_ENABLED
3. <span data-ttu-id="97290-122">(Volitelné) toosupport zařízení se systémem Windows, instalovat jeden z následujících balíčků runtime SQLite hello:</span><span class="sxs-lookup"><span data-stu-id="97290-122">(Optional) toosupport Windows devices, install one of hello following SQLite runtime packages:</span></span>

   * <span data-ttu-id="97290-123">**Modul Runtime pro Windows 8.1:** nainstalovat [SQLite pro Windows 8.1][3].</span><span class="sxs-lookup"><span data-stu-id="97290-123">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="97290-124">**Windows Phone 8.1:** nainstalovat [SQLite pro Windows Phone 8.1][4].</span><span class="sxs-lookup"><span data-stu-id="97290-124">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="97290-125">**Univerzální platformu Windows** nainstalovat [SQLite pro univerzální univerzální pro Windows hello][5].</span><span class="sxs-lookup"><span data-stu-id="97290-125">**Universal Windows Platform** Install [SQLite for hello Universal Windows Universal][5].</span></span>

     <span data-ttu-id="97290-126">I když rychlý start hello neobsahuje Universal Windows project, hello univerzální platformy Windows jsou podporovány pro Xamarin Forms.</span><span class="sxs-lookup"><span data-stu-id="97290-126">Although hello quickstart does not contain a Universal Windows project, hello Universal Windows platform is supported with Xamarin Forms.</span></span>
4. <span data-ttu-id="97290-127">(Volitelné) V každé projekt aplikace Windows, klikněte pravým tlačítkem na **odkazy** > **přidat odkaz na...** , rozbalte položku hello **Windows** složky > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="97290-127">(Optional) In each Windows app project, right-click **References** > **Add Reference...**, expand hello **Windows** folder > **Extensions**.</span></span>
    <span data-ttu-id="97290-128">Povolit hello odpovídající **SQLite pro systém Windows** SDK společně s hello **Visual C++ 2013 modulu Runtime pro Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="97290-128">Enable hello appropriate **SQLite for Windows** SDK along with hello **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="97290-129">Hello SQLite SDK názvy jsou mírně odlišné s každou platformu Windows.</span><span class="sxs-lookup"><span data-stu-id="97290-129">hello SQLite SDK names vary slightly with each Windows platform.</span></span>

## <a name="review-hello-client-sync-code"></a><span data-ttu-id="97290-130">Zkontrolujte kód synchronizace klienta hello</span><span class="sxs-lookup"><span data-stu-id="97290-130">Review hello client sync code</span></span>
<span data-ttu-id="97290-131">Zde je stručný přehled co je již součástí hello kódu uvnitř hello `#if OFFLINE_SYNC_ENABLED` direktivy.</span><span class="sxs-lookup"><span data-stu-id="97290-131">Here is a brief overview of what is already included in hello tutorial code inside hello `#if OFFLINE_SYNC_ENABLED` directives.</span></span> <span data-ttu-id="97290-132">Funkci offline synchronizace je v souboru projektu hello TodoItemManager.cs v projektu knihovny přenosných tříd hello.</span><span class="sxs-lookup"><span data-stu-id="97290-132">The offline sync functionality is in hello TodoItemManager.cs project file in hello Portable Class Library project.</span></span> <span data-ttu-id="97290-133">Koncepční přehled hello funkce, najdete v tématu [Offline synchronizací dat v Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="97290-133">For a conceptual overview of hello feature, see [Offline Data Sync in Azure Mobile Apps][2].</span></span>

* <span data-ttu-id="97290-134">Před provedením jakékoli operace s tabulkou, musí být inicializován hello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="97290-134">Before any table operations can be performed, hello local store must be initialized.</span></span> <span data-ttu-id="97290-135">Hello místního úložiště databáze je v inicializovat hello **TodoItemManager** konstruktoru třídy pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="97290-135">hello local store database is initialized in hello **TodoItemManager** class constructor by using hello following code:</span></span>

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    <span data-ttu-id="97290-136">Tento kód vytvoří novou místní databázi SQLite pomocí hello **MobileServiceSQLiteStore** třídy.</span><span class="sxs-lookup"><span data-stu-id="97290-136">This code creates a new local SQLite database using hello **MobileServiceSQLiteStore** class.</span></span>

    <span data-ttu-id="97290-137">Hello **DefineTable** metoda vytvoří tabulku v hello místního úložiště, který odpovídá hello polí v hello zadaný typ.</span><span class="sxs-lookup"><span data-stu-id="97290-137">hello **DefineTable** method creates a table in hello local store that matches hello fields in hello provided type.</span></span>  <span data-ttu-id="97290-138">Typ Hello nemá tooinclude všechny hello sloupců, které jsou v hello vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="97290-138">hello type doesn't have tooinclude all hello columns that are in hello remote database.</span></span> <span data-ttu-id="97290-139">Je možné toostore podmnožinu sloupců.</span><span class="sxs-lookup"><span data-stu-id="97290-139">It is possible toostore a subset of columns.</span></span>
* <span data-ttu-id="97290-140">Hello **todoTable** pole **TodoItemManager** je **IMobileServiceSyncTable** zadejte místo **IMobileServiceTable**.</span><span class="sxs-lookup"><span data-stu-id="97290-140">hello **todoTable** field in **TodoItemManager** is an **IMobileServiceSyncTable** type instead of **IMobileServiceTable**.</span></span> <span data-ttu-id="97290-141">Tato třída používá hello místní databáze pro všechny vytvářet, číst, aktualizovat a odstranit tabulku operace.</span><span class="sxs-lookup"><span data-stu-id="97290-141">This class uses hello local database for all create, read, update, and delete (CRUD) table operations.</span></span> <span data-ttu-id="97290-142">Rozhodnete, když jsou tyto změny nabídnutých back-end mobilní aplikace toohello voláním **PushAsync** na hello **IMobileServiceSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="97290-142">You decide when those changes are pushed toohello Mobile App backend by calling **PushAsync** on hello **IMobileServiceSyncContext**.</span></span> <span data-ttu-id="97290-143">Hello kontext synchronizace umožňuje zachovat relace mezi tabulkami sledováním a vkládání změny ve všech tabulkách klientské aplikace změnil při **PushAsync** je volána.</span><span class="sxs-lookup"><span data-stu-id="97290-143">hello sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when **PushAsync** is called.</span></span>

    <span data-ttu-id="97290-144">Následující Hello **SyncAsync** metoda je volána toosync s back-end mobilní aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="97290-144">hello following **SyncAsync** method is called toosync with hello Mobile App backend:</span></span>

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

    <span data-ttu-id="97290-145">Tato ukázka používá jednoduchý s hello výchozí synchronizace obslužnou rutinu pro zpracování chyb.</span><span class="sxs-lookup"><span data-stu-id="97290-145">This sample uses simple error handling with hello default sync handler.</span></span> <span data-ttu-id="97290-146">Reálné aplikaci byste zpracování hello různých chybám, jako jsou síťové podmínky a server je v konfliktu s použitím vlastní **IMobileServiceSyncHandler** implementace.</span><span class="sxs-lookup"><span data-stu-id="97290-146">A real application would handle hello various errors like network conditions and server conflicts by using a custom **IMobileServiceSyncHandler** implementation.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="97290-147">Důležité informace o offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="97290-147">Offline sync considerations</span></span>
<span data-ttu-id="97290-148">V ukázce hello hello **SyncAsync** metoda je volána pouze na spuštění a pokud je požadována synchronizace.</span><span class="sxs-lookup"><span data-stu-id="97290-148">In hello sample, hello **SyncAsync** method is only called on start-up and when a sync is requested.</span></span>  <span data-ttu-id="97290-149">tooinitiate synchronizace v aplikaci Android nebo iOS vyžádání dolů v seznamu položek hello; pro Windows, použijte hello **synchronizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97290-149">tooinitiate a sync in an Android or iOS app, pull down on hello items list; for Windows, use hello **Sync** button.</span></span> <span data-ttu-id="97290-150">V reálné aplikaci může způsobují hello synchronizace aktivační událost při změně stavu sítě hello.</span><span class="sxs-lookup"><span data-stu-id="97290-150">In a real-world application, you could also make hello sync trigger when hello network state changes.</span></span>

<span data-ttu-id="97290-151">Když je spustit vyžádání pro tabulku, která má čekající místní aktualizace sleduje hello kontext, který pull operace automaticky aktivuje předchozí push kontextu.</span><span class="sxs-lookup"><span data-stu-id="97290-151">When a pull is executed against a table that has pending local updates tracked by hello context, that pull operation automatically triggers a preceding context push.</span></span> <span data-ttu-id="97290-152">Při aktualizaci, přidání a dokončení položky v této ukázce, můžete vynechat hello explicitní **PushAsync** volání.</span><span class="sxs-lookup"><span data-stu-id="97290-152">When refreshing, adding, and completing items in this sample, you can omit hello explicit **PushAsync** call.</span></span>

<span data-ttu-id="97290-153">V kódu hello poskytuje, jsou předmětem dotazování všechny záznamy v hello vzdálenou tabulku TodoItem, ale je také možné toofilter záznamy předáním id dotazu a dotaz příliš**PushAsync**.</span><span class="sxs-lookup"><span data-stu-id="97290-153">In hello provided code, all records in hello remote TodoItem table are queried, but it is also possible toofilter records by passing a query id and query too**PushAsync**.</span></span> <span data-ttu-id="97290-154">Další informace najdete v tématu hello *přírůstkové synchronizace* v [Offline synchronizací dat v Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="97290-154">For more information, see hello section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="run-hello-client-app"></a><span data-ttu-id="97290-155">Spustit klientskou aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="97290-155">Run hello client app</span></span>
<span data-ttu-id="97290-156">S offline synchronizací teď povolené spusťte klientskou aplikaci hello alespoň jednou na každou databázi platformy toopopulate hello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="97290-156">With offline sync now enabled, run hello client application at least once on each platform toopopulate hello local store database.</span></span> <span data-ttu-id="97290-157">Později simulovat offline scénář a upravit data hello v místním úložišti hello aplikace hello je offline.</span><span class="sxs-lookup"><span data-stu-id="97290-157">Later, simulate an offline scenario and modify hello data in hello local store while hello app is offline.</span></span>

## <a name="update-hello-sync-behavior-of-hello-client-app"></a><span data-ttu-id="97290-158">Aktualizace chování synchronizace hello hello klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="97290-158">Update hello sync behavior of hello client app</span></span>
<span data-ttu-id="97290-159">V této části upravte hello klienta projektu toosimulate offline scénář pomocí adresy URL Neplatná aplikace pro váš back-end.</span><span class="sxs-lookup"><span data-stu-id="97290-159">In this section, modify hello client project toosimulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="97290-160">Alternativně můžete vypnout připojení k síti přesunutím zařízení příliš "Režim v letadle."</span><span class="sxs-lookup"><span data-stu-id="97290-160">Alternatively, you can turn off network connections by moving your device too"Airplane mode."</span></span>  <span data-ttu-id="97290-161">Při přidání nebo změna datových položek, jsou tyto změny uchovávat v místním úložišti hello, ale položka nebyla synchronizována toohello back-end datové úložiště, dokud nebude znovu navázané připojení hello.</span><span class="sxs-lookup"><span data-stu-id="97290-161">When you add or change data items, these changes are held in hello local store, but not synced toohello backend data store until hello connection is re-established.</span></span>

1. <span data-ttu-id="97290-162">V Průzkumníku řešení hello, otevřete soubor projektu Constants.cs hello z hello **přenosné** projektu a změnit hodnotu hello `ApplicationURL` toopoint tooan neplatná adresa URL:</span><span class="sxs-lookup"><span data-stu-id="97290-162">In hello Solution Explorer, open hello Constants.cs project file from hello **Portable** project and change hello value of `ApplicationURL` toopoint tooan invalid URL:</span></span>

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. <span data-ttu-id="97290-163">Otevřete hello TodoItemManager.cs soubor z hello **přenosné** projektu a pak přidejte **catch** pro hello základní **výjimka** toohello třída **try... catch** blokovat **SyncAsync**.</span><span class="sxs-lookup"><span data-stu-id="97290-163">Open hello TodoItemManager.cs file from hello **Portable** project, then add a **catch** for hello base **Exception** class toohello **try...catch** block in **SyncAsync**.</span></span> <span data-ttu-id="97290-164">To **catch** bloku zapíše hello výjimka zpráva toohello konzoly, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="97290-164">This **catch** block writes hello exception message toohello console, as follows:</span></span>

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. <span data-ttu-id="97290-165">Sestavení a spuštění klienta aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="97290-165">Build and run hello client app.</span></span>  <span data-ttu-id="97290-166">Přidáte nové položky.</span><span class="sxs-lookup"><span data-stu-id="97290-166">Add some new items.</span></span> <span data-ttu-id="97290-167">Všimněte si, že hello konzoly pro každý pokus o toosync s back-end hello protokolu výjimku.</span><span class="sxs-lookup"><span data-stu-id="97290-167">Notice that an exception is logged in hello console for each attempt toosync with hello backend.</span></span> <span data-ttu-id="97290-168">Tyto nové položky existovat pouze v místním úložišti hello, dokud se můžou poslat toohello mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="97290-168">These new items exist only in hello local store until they can be pushed toohello mobile backend.</span></span> <span data-ttu-id="97290-169">klientská aplikace Hello se chová, pokud je připojený toohello back-end, podpora všechny vytvářet, číst, update, operace odstranění (CRUD).</span><span class="sxs-lookup"><span data-stu-id="97290-169">hello client app behaves as if it is connected toohello backend, supporting all create, read, update, delete (CRUD) operations.</span></span>
4. <span data-ttu-id="97290-170">Zavření aplikace hello a restartujte ji tooverify že hello nové položky, kterou jste vytvořili jsou trvalé toohello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="97290-170">Close hello app and restart it tooverify that hello new items you created are persisted toohello local store.</span></span>
5. <span data-ttu-id="97290-171">(Volitelné) Pomocí sady Visual Studio tooview vaše toosee tabulky Azure SQL Database, nezměnil hello data v databázi back-end hello.</span><span class="sxs-lookup"><span data-stu-id="97290-171">(Optional) Use Visual Studio tooview your Azure SQL Database table toosee that hello data in hello backend database has not changed.</span></span>

    <span data-ttu-id="97290-172">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="97290-172">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="97290-173">Přejděte tooyour databáze v **Azure**->**databází SQL**.</span><span class="sxs-lookup"><span data-stu-id="97290-173">Navigate tooyour database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="97290-174">Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="97290-174">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="97290-175">Teď můžete procházet tooyour tabulce databáze SQL a její obsah.</span><span class="sxs-lookup"><span data-stu-id="97290-175">Now you can browse tooyour SQL database table and its contents.</span></span>

## <a name="update-hello-client-app-tooreconnect-your-mobile-backend"></a><span data-ttu-id="97290-176">Aktualizovat hello klienta aplikace tooreconnect mobilního back-endu</span><span class="sxs-lookup"><span data-stu-id="97290-176">Update hello client app tooreconnect your mobile backend</span></span>
<span data-ttu-id="97290-177">V této části znovu se připojte hello toohello mobilní back-end aplikace, který simuluje hello aplikace vracející se zpět tooan stavu online.</span><span class="sxs-lookup"><span data-stu-id="97290-177">In this section, reconnect hello app toohello mobile backend, which simulates hello app coming back tooan online state.</span></span> <span data-ttu-id="97290-178">Při provádění gesto hello aktualizace dat je synchronizovaná tooyour mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="97290-178">When you perform hello refresh gesture, data is synced tooyour mobile backend.</span></span>

1. <span data-ttu-id="97290-179">Znovu otevřete Constants.cs.</span><span class="sxs-lookup"><span data-stu-id="97290-179">Reopen Constants.cs.</span></span> <span data-ttu-id="97290-180">Správné hello `applicationURL` toopoint toohello opravte adresu URL.</span><span class="sxs-lookup"><span data-stu-id="97290-180">Correct hello `applicationURL` toopoint toohello correct URL.</span></span>
2. <span data-ttu-id="97290-181">Znovu sestavit a spustit klientskou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="97290-181">Rebuild and run hello client app.</span></span> <span data-ttu-id="97290-182">Hello toosync pokusy o aplikaci s back-end mobilní aplikace hello po spuštění.</span><span class="sxs-lookup"><span data-stu-id="97290-182">hello app attempts toosync with hello mobile app backend after launching.</span></span> <span data-ttu-id="97290-183">Ověřte, že žádné výjimky jsou zaznamenány do konzoly ladění hello.</span><span class="sxs-lookup"><span data-stu-id="97290-183">Verify that no exceptions are logged in hello debug console.</span></span>
3. <span data-ttu-id="97290-184">(Volitelné) Zobrazení hello aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler nebo [Postman][6].</span><span class="sxs-lookup"><span data-stu-id="97290-184">(Optional) View hello updated data using either SQL Server Object Explorer or a REST tool like Fiddler or [Postman][6].</span></span> <span data-ttu-id="97290-185">Všimněte si hello data umístění byl synchronizován mezi hello back-endovou databázi a hello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="97290-185">Notice hello data has been synchronized between hello backend database and hello local store.</span></span>

    <span data-ttu-id="97290-186">Všimněte si hello data umístění byl synchronizován mezi hello databáze a místní úložiště hello a obsahuje hello položky, které jste přidali v době, kdy byl odpojen vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="97290-186">Notice hello data has been synchronized between hello database and hello local store and contains hello items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97290-187">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="97290-187">Additional Resources</span></span>
* <span data-ttu-id="97290-188">[Synchronizaci dat offline v Azure Mobile Apps][2]</span><span class="sxs-lookup"><span data-stu-id="97290-188">[Offline Data Sync in Azure Mobile Apps][2]</span></span>
* <span data-ttu-id="97290-189">[Mobilní aplikace Azure .NET SDK postupy][8]</span><span class="sxs-lookup"><span data-stu-id="97290-189">[Azure Mobile Apps .NET SDK HOWTO][8]</span></span>

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
