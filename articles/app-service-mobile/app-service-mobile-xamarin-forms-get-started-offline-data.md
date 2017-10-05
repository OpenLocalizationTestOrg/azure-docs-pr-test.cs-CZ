---
title: "Zapnutí offline synchronizace pro mobilní aplikace Azure (Xamarin.Forms) | Microsoft Docs"
description: "Další informace o použití aplikace služby mobilní aplikace do mezipaměti a synchronizaci dat offline v aplikaci Xamarin.Forms"
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
ms.openlocfilehash: f2bed0a7124517319cc82405c4ab6b4d79aacfe1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a><span data-ttu-id="f21d2-103">Povolení offline synchronizace pro mobilní aplikace Xamarin.Forms</span><span class="sxs-lookup"><span data-stu-id="f21d2-103">Enable offline sync for your Xamarin.Forms mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="f21d2-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f21d2-104">Overview</span></span>
<span data-ttu-id="f21d2-105">Tento kurz představuje offline synchronizace funkci Azure Mobile Apps pro Xamarin.Forms.</span><span class="sxs-lookup"><span data-stu-id="f21d2-105">This tutorial introduces the offline sync feature of Azure Mobile Apps for Xamarin.Forms.</span></span> <span data-ttu-id="f21d2-106">Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací – zobrazení, přidání nebo úprava dat – i když dojde k dispozici žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="f21d2-106">Offline sync allows end users to interact with a mobile app--viewing, adding, or modifying data--even when there is no network connection.</span></span> <span data-ttu-id="f21d2-107">Změny se ukládají do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="f21d2-107">Changes are stored in a local database.</span></span> <span data-ttu-id="f21d2-108">Když je zařízení do režimu online, tyto změny se synchronizují s vzdálené služby.</span><span class="sxs-lookup"><span data-stu-id="f21d2-108">Once the device is back online, these changes are synced with the remote service.</span></span>

<span data-ttu-id="f21d2-109">V tomto kurzu vychází rychlé spuštění řešení Xamarin.Forms pro mobilní aplikace, kterou vytvoříte po dokončení tohoto kurzu [vytvoření aplikace pro Xamarin iOS].</span><span class="sxs-lookup"><span data-stu-id="f21d2-109">This tutorial is based on the Xamarin.Forms quickstart solution for Mobile Apps that you create when you complete the tutorial [Create a Xamarin iOS app].</span></span> <span data-ttu-id="f21d2-110">Rychlý start řešení pro Xamarin.Forms obsahuje kód pro podporu offline synchronizace, který právě musí být povolena.</span><span class="sxs-lookup"><span data-stu-id="f21d2-110">The quickstart solution for Xamarin.Forms contains the code to support offline sync, which just needs to be enabled.</span></span> <span data-ttu-id="f21d2-111">V tomto kurzu aktualizujete řešení rychlý start zapnutí offline funkce Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="f21d2-111">In this tutorial, you update the quickstart solution to turn on the offline features of Azure Mobile Apps.</span></span> <span data-ttu-id="f21d2-112">Také jsme zvýrazněte offline konkrétní kódu v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f21d2-112">We also highlight the offline-specific code in the app.</span></span> <span data-ttu-id="f21d2-113">Pokud nepoužijete řešení stažené rychlé spuštění, je nutné přidat data přístup rozšiřující balíčky do projektu.</span><span class="sxs-lookup"><span data-stu-id="f21d2-113">If you do not use the downloaded quickstart solution, you must add the data access extension packages to your project.</span></span> <span data-ttu-id="f21d2-114">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="f21d2-114">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps][1].</span></span>

<span data-ttu-id="f21d2-115">Další informace o funkci offline synchronizace, naleznete v tématu [Offline synchronizací dat v Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="f21d2-115">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a><span data-ttu-id="f21d2-116">Povolit funkci offline synchronizace v řešení pro rychlý start</span><span class="sxs-lookup"><span data-stu-id="f21d2-116">Enable offline sync functionality in the quickstart solution</span></span>
<span data-ttu-id="f21d2-117">Kód offline synchronizace je zahrnutý v projektu s použitím jazyka C# direktivy preprocesoru.</span><span class="sxs-lookup"><span data-stu-id="f21d2-117">The offline sync code is included in the project by using C# preprocessor directives.</span></span> <span data-ttu-id="f21d2-118">Když **OFFLINE\_SYNCHRONIZACE\_povoleno** symbol definován, tyto cesty kódu jsou součástí sestavení.</span><span class="sxs-lookup"><span data-stu-id="f21d2-118">When the **OFFLINE\_SYNC\_ENABLED** symbol is defined, these code paths are included in the build.</span></span> <span data-ttu-id="f21d2-119">Pro aplikace pro Windows musíte také nainstalovat platformou SQLite.</span><span class="sxs-lookup"><span data-stu-id="f21d2-119">For Windows apps, you must also install the SQLite platform.</span></span>

1. <span data-ttu-id="f21d2-120">V sadě Visual Studio, klikněte pravým tlačítkem na řešení > **spravovat balíčky NuGet pro řešení...** , vyhledejte a nainstalujte **Microsoft.Azure.Mobile.Client.SQLiteStore** balíček NuGet pro všechny projekty v řešení.</span><span class="sxs-lookup"><span data-stu-id="f21d2-120">In Visual Studio, right-click the solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in the solution.</span></span>
2. <span data-ttu-id="f21d2-121">V Průzkumníku řešení otevřete soubor TodoItemManager.cs z projektu s **přenosné** v názvu, který je projektu knihovny přenosných tříd, poté zrušte komentář u následující direktivy preprocesoru:</span><span class="sxs-lookup"><span data-stu-id="f21d2-121">In the Solution Explorer, open the TodoItemManager.cs file from the project with **Portable** in the name, which is Portable Class Library project, then uncomment the following preprocessor directive:</span></span>

        #define OFFLINE_SYNC_ENABLED
3. <span data-ttu-id="f21d2-122">(Volitelné) Pro podporu zařízení se systémem Windows, instalovat jeden z následujících balíčků runtime SQLite:</span><span class="sxs-lookup"><span data-stu-id="f21d2-122">(Optional) To support Windows devices, install one of the following SQLite runtime packages:</span></span>

   * <span data-ttu-id="f21d2-123">**Modul Runtime pro Windows 8.1:** nainstalovat [SQLite pro Windows 8.1][3].</span><span class="sxs-lookup"><span data-stu-id="f21d2-123">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="f21d2-124">**Windows Phone 8.1:** nainstalovat [SQLite pro Windows Phone 8.1][4].</span><span class="sxs-lookup"><span data-stu-id="f21d2-124">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="f21d2-125">**Univerzální platformu Windows** nainstalovat [SQLite pro univerzální Universal Windows][5].</span><span class="sxs-lookup"><span data-stu-id="f21d2-125">**Universal Windows Platform** Install [SQLite for the Universal Windows Universal][5].</span></span>

     <span data-ttu-id="f21d2-126">I když rychlý start neobsahuje Universal Windows project, univerzální platformy Windows jsou podporovány pro Xamarin Forms.</span><span class="sxs-lookup"><span data-stu-id="f21d2-126">Although the quickstart does not contain a Universal Windows project, the Universal Windows platform is supported with Xamarin Forms.</span></span>
4. <span data-ttu-id="f21d2-127">(Volitelné) V každé projekt aplikace Windows, klikněte pravým tlačítkem na **odkazy** > **přidat odkaz na...** , rozbalte **Windows** složky > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="f21d2-127">(Optional) In each Windows app project, right-click **References** > **Add Reference...**, expand the **Windows** folder > **Extensions**.</span></span>
    <span data-ttu-id="f21d2-128">Povolit vhodnou **SQLite pro systém Windows** SDK spolu s **Visual C++ 2013 modulu Runtime pro Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="f21d2-128">Enable the appropriate **SQLite for Windows** SDK along with the **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="f21d2-129">Názvy SQLite SDK mírně lišit s každou platformu Windows.</span><span class="sxs-lookup"><span data-stu-id="f21d2-129">The SQLite SDK names vary slightly with each Windows platform.</span></span>

## <a name="review-the-client-sync-code"></a><span data-ttu-id="f21d2-130">Zkontrolujte kód synchronizace klienta</span><span class="sxs-lookup"><span data-stu-id="f21d2-130">Review the client sync code</span></span>
<span data-ttu-id="f21d2-131">Zde je stručný přehled co je již zahrnut v kurzu kódu uvnitř `#if OFFLINE_SYNC_ENABLED` direktivy.</span><span class="sxs-lookup"><span data-stu-id="f21d2-131">Here is a brief overview of what is already included in the tutorial code inside the `#if OFFLINE_SYNC_ENABLED` directives.</span></span> <span data-ttu-id="f21d2-132">Funkci offline synchronizace je v souboru projektu TodoItemManager.cs v projektu knihovny přenosných tříd.</span><span class="sxs-lookup"><span data-stu-id="f21d2-132">The offline sync functionality is in the TodoItemManager.cs project file in the Portable Class Library project.</span></span> <span data-ttu-id="f21d2-133">Koncepční přehled funkce, najdete v části [Offline synchronizací dat v Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="f21d2-133">For a conceptual overview of the feature, see [Offline Data Sync in Azure Mobile Apps][2].</span></span>

* <span data-ttu-id="f21d2-134">Před provedením jakékoli operace s tabulkou, musí být inicializován místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="f21d2-134">Before any table operations can be performed, the local store must be initialized.</span></span> <span data-ttu-id="f21d2-135">Místní úložiště databáze je inicializován v **TodoItemManager** konstruktoru třídy pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="f21d2-135">The local store database is initialized in the **TodoItemManager** class constructor by using the following code:</span></span>

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    <span data-ttu-id="f21d2-136">Tento kód vytvoří novou místní SQLite databáze pomocí **MobileServiceSQLiteStore** třídy.</span><span class="sxs-lookup"><span data-stu-id="f21d2-136">This code creates a new local SQLite database using the **MobileServiceSQLiteStore** class.</span></span>

    <span data-ttu-id="f21d2-137">**DefineTable** metoda vytvoří tabulku v místním úložišti odpovídající pole v zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="f21d2-137">The **DefineTable** method creates a table in the local store that matches the fields in the provided type.</span></span>  <span data-ttu-id="f21d2-138">Typ nemusí obsahovat všechny sloupce, které jsou v vzdálené databáze.</span><span class="sxs-lookup"><span data-stu-id="f21d2-138">The type doesn't have to include all the columns that are in the remote database.</span></span> <span data-ttu-id="f21d2-139">Je možné uložit podmnožinu sloupců.</span><span class="sxs-lookup"><span data-stu-id="f21d2-139">It is possible to store a subset of columns.</span></span>
* <span data-ttu-id="f21d2-140">**TodoTable** pole **TodoItemManager** je **IMobileServiceSyncTable** zadejte místo **IMobileServiceTable**.</span><span class="sxs-lookup"><span data-stu-id="f21d2-140">The **todoTable** field in **TodoItemManager** is an **IMobileServiceSyncTable** type instead of **IMobileServiceTable**.</span></span> <span data-ttu-id="f21d2-141">Toto využívá třída pro všechny místní databázi vytvořit, číst, aktualizovat a odstranit tabulku operace.</span><span class="sxs-lookup"><span data-stu-id="f21d2-141">This class uses the local database for all create, read, update, and delete (CRUD) table operations.</span></span> <span data-ttu-id="f21d2-142">Rozhodnete, když se tyto změny odesílají na back-end mobilní aplikace při volání **PushAsync** na **IMobileServiceSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="f21d2-142">You decide when those changes are pushed to the Mobile App backend by calling **PushAsync** on the **IMobileServiceSyncContext**.</span></span> <span data-ttu-id="f21d2-143">Kontext synchronizace umožňuje zachovat relace mezi tabulkami sledováním a vkládání změny ve všech tabulkách klientské aplikace změnil při **PushAsync** je volána.</span><span class="sxs-lookup"><span data-stu-id="f21d2-143">The sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when **PushAsync** is called.</span></span>

    <span data-ttu-id="f21d2-144">Následující **SyncAsync** metoda je volána pro synchronizaci s back-end mobilní aplikace:</span><span class="sxs-lookup"><span data-stu-id="f21d2-144">The following **SyncAsync** method is called to sync with the Mobile App backend:</span></span>

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
                        //Update failed, reverting to server's copy.
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

    <span data-ttu-id="f21d2-145">Tato ukázka používá jednoduchý zpracování chyb pomocí výchozí obslužnou rutinu synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f21d2-145">This sample uses simple error handling with the default sync handler.</span></span> <span data-ttu-id="f21d2-146">Reálné aplikaci byste zpracovávat různé chyby jako síťové podmínky a server je v konfliktu s použitím vlastní **IMobileServiceSyncHandler** implementace.</span><span class="sxs-lookup"><span data-stu-id="f21d2-146">A real application would handle the various errors like network conditions and server conflicts by using a custom **IMobileServiceSyncHandler** implementation.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="f21d2-147">Důležité informace o offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="f21d2-147">Offline sync considerations</span></span>
<span data-ttu-id="f21d2-148">V ukázce **SyncAsync** metoda je volána pouze na spuštění a pokud je požadována synchronizace.</span><span class="sxs-lookup"><span data-stu-id="f21d2-148">In the sample, the **SyncAsync** method is only called on start-up and when a sync is requested.</span></span>  <span data-ttu-id="f21d2-149">Spusťte synchronizaci v aplikaci pro Android nebo iOS, stáhněte dolů v seznamu položek; pro Windows, použijte **synchronizace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f21d2-149">To initiate a sync in an Android or iOS app, pull down on the items list; for Windows, use the **Sync** button.</span></span> <span data-ttu-id="f21d2-150">V reálné aplikaci může také mít aktivační událost synchronizace při změně stavu sítě.</span><span class="sxs-lookup"><span data-stu-id="f21d2-150">In a real-world application, you could also make the sync trigger when the network state changes.</span></span>

<span data-ttu-id="f21d2-151">Když je spustit vyžádání pro tabulku, která má čekající místní aktualizace sleduje kontext, který pull operace automaticky aktivuje předchozí push kontextu.</span><span class="sxs-lookup"><span data-stu-id="f21d2-151">When a pull is executed against a table that has pending local updates tracked by the context, that pull operation automatically triggers a preceding context push.</span></span> <span data-ttu-id="f21d2-152">Při aktualizaci, přidávání a dokončení položky v této ukázce, můžete vynechat explicitní **PushAsync** volání.</span><span class="sxs-lookup"><span data-stu-id="f21d2-152">When refreshing, adding, and completing items in this sample, you can omit the explicit **PushAsync** call.</span></span>

<span data-ttu-id="f21d2-153">V zadané kódu jsou předmětem dotazování všechny záznamy v vzdálenou tabulku TodoItem, ale je také možné filtrování záznamů předáním id dotazu a dotazy s cílem **PushAsync**.</span><span class="sxs-lookup"><span data-stu-id="f21d2-153">In the provided code, all records in the remote TodoItem table are queried, but it is also possible to filter records by passing a query id and query to **PushAsync**.</span></span> <span data-ttu-id="f21d2-154">Další informace najdete v části *přírůstkové synchronizace* v [Offline synchronizací dat v Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="f21d2-154">For more information, see the section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps][2].</span></span>

## <a name="run-the-client-app"></a><span data-ttu-id="f21d2-155">Spustit klientskou aplikaci</span><span class="sxs-lookup"><span data-stu-id="f21d2-155">Run the client app</span></span>
<span data-ttu-id="f21d2-156">S offline synchronizací teď povolené spusťte klientskou aplikaci alespoň jednou na každou platformu k naplnění databáze místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="f21d2-156">With offline sync now enabled, run the client application at least once on each platform to populate the local store database.</span></span> <span data-ttu-id="f21d2-157">Později simulovat offline scénář a upravit data v místním úložišti, zatímco aplikace v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="f21d2-157">Later, simulate an offline scenario and modify the data in the local store while the app is offline.</span></span>

## <a name="update-the-sync-behavior-of-the-client-app"></a><span data-ttu-id="f21d2-158">Aktualizovat synchronizační chování klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="f21d2-158">Update the sync behavior of the client app</span></span>
<span data-ttu-id="f21d2-159">V této části upravíte projektu klienta k simulaci offline scénář pomocí adresy URL Neplatná aplikace pro váš back-end.</span><span class="sxs-lookup"><span data-stu-id="f21d2-159">In this section, modify the client project to simulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="f21d2-160">Alternativně můžete vypnout připojení k síti přesunutím zařízení do "Režim v letadle."</span><span class="sxs-lookup"><span data-stu-id="f21d2-160">Alternatively, you can turn off network connections by moving your device to "Airplane mode."</span></span>  <span data-ttu-id="f21d2-161">Při přidání nebo změna datových položek, tyto změny uchovávat v místním úložišti, ale není synchronizované do úložiště dat back-end, dokud nebude znovu navázané připojení.</span><span class="sxs-lookup"><span data-stu-id="f21d2-161">When you add or change data items, these changes are held in the local store, but not synced to the backend data store until the connection is re-established.</span></span>

1. <span data-ttu-id="f21d2-162">V Průzkumníku řešení otevřete soubor projektu Constants.cs z **přenosné** projektu a změňte hodnotu `ApplicationURL` tak, aby odkazovalo na neplatnou adresu URL:</span><span class="sxs-lookup"><span data-stu-id="f21d2-162">In the Solution Explorer, open the Constants.cs project file from the **Portable** project and change the value of `ApplicationURL` to point to an invalid URL:</span></span>

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. <span data-ttu-id="f21d2-163">Otevřete soubor TodoItemManager.cs z **přenosné** projektu a pak přidejte **catch** pro základ **výjimka** třídy k **try... catch** blokovat **SyncAsync**.</span><span class="sxs-lookup"><span data-stu-id="f21d2-163">Open the TodoItemManager.cs file from the **Portable** project, then add a **catch** for the base **Exception** class to the **try...catch** block in **SyncAsync**.</span></span> <span data-ttu-id="f21d2-164">To **catch** bloku zapíše zpráva o výjimce konzole, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f21d2-164">This **catch** block writes the exception message to the console, as follows:</span></span>

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. <span data-ttu-id="f21d2-165">Sestavení a spuštění klienta aplikace.</span><span class="sxs-lookup"><span data-stu-id="f21d2-165">Build and run the client app.</span></span>  <span data-ttu-id="f21d2-166">Přidáte nové položky.</span><span class="sxs-lookup"><span data-stu-id="f21d2-166">Add some new items.</span></span> <span data-ttu-id="f21d2-167">Všimněte si, že je v konzole pro každý pokus o synchronizaci s back-end protokolována výjimku.</span><span class="sxs-lookup"><span data-stu-id="f21d2-167">Notice that an exception is logged in the console for each attempt to sync with the backend.</span></span> <span data-ttu-id="f21d2-168">Tyto nové položky existovat pouze v místním úložišti, dokud se můžou poslat na mobilní back-end.</span><span class="sxs-lookup"><span data-stu-id="f21d2-168">These new items exist only in the local store until they can be pushed to the mobile backend.</span></span> <span data-ttu-id="f21d2-169">Klientská aplikace se chová jako kdyby je připojené k back-end, podpora všechny vytvářet, číst, aktualizovat, operace odstranění (CRUD).</span><span class="sxs-lookup"><span data-stu-id="f21d2-169">The client app behaves as if it is connected to the backend, supporting all create, read, update, delete (CRUD) operations.</span></span>
4. <span data-ttu-id="f21d2-170">Zavřete aplikaci a restartujte ji k ověření, že nové položky, které jste vytvořili, jsou nastavené jako trvalé do místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="f21d2-170">Close the app and restart it to verify that the new items you created are persisted to the local store.</span></span>
5. <span data-ttu-id="f21d2-171">(Volitelné) Pomocí sady Visual Studio zobrazíte tabulka Azure SQL Database v tématu, že se nezměnil na data v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="f21d2-171">(Optional) Use Visual Studio to view your Azure SQL Database table to see that the data in the backend database has not changed.</span></span>

    <span data-ttu-id="f21d2-172">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="f21d2-172">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="f21d2-173">Přejděte k vaší databázi v **Azure**->**databází SQL**.</span><span class="sxs-lookup"><span data-stu-id="f21d2-173">Navigate to your database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="f21d2-174">Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="f21d2-174">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="f21d2-175">Teď můžete procházet tabulku databáze SQL a její obsah.</span><span class="sxs-lookup"><span data-stu-id="f21d2-175">Now you can browse to your SQL database table and its contents.</span></span>

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a><span data-ttu-id="f21d2-176">Aktualizace klienta aplikace se znovu připojit mobilního back-endu</span><span class="sxs-lookup"><span data-stu-id="f21d2-176">Update the client app to reconnect your mobile backend</span></span>
<span data-ttu-id="f21d2-177">V této části znovu se připojte aplikaci mobilního back-end, který simuluje aplikace vracející se zpět do stavu online.</span><span class="sxs-lookup"><span data-stu-id="f21d2-177">In this section, reconnect the app to the mobile backend, which simulates the app coming back to an online state.</span></span> <span data-ttu-id="f21d2-178">Při provádění gesto aktualizace dat je synchronizované s vaší mobilní back-end.</span><span class="sxs-lookup"><span data-stu-id="f21d2-178">When you perform the refresh gesture, data is synced to your mobile backend.</span></span>

1. <span data-ttu-id="f21d2-179">Znovu otevřete Constants.cs.</span><span class="sxs-lookup"><span data-stu-id="f21d2-179">Reopen Constants.cs.</span></span> <span data-ttu-id="f21d2-180">Opravte `applicationURL` tak, aby odkazoval na správnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f21d2-180">Correct the `applicationURL` to point to the correct URL.</span></span>
2. <span data-ttu-id="f21d2-181">Znovu sestavit a spustit klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f21d2-181">Rebuild and run the client app.</span></span> <span data-ttu-id="f21d2-182">Aplikace se pokusí synchronizovat s back-end mobilní aplikace po spuštění.</span><span class="sxs-lookup"><span data-stu-id="f21d2-182">The app attempts to sync with the mobile app backend after launching.</span></span> <span data-ttu-id="f21d2-183">Ověřte, že žádné výjimky jsou zaznamenány v konzole pro ladění.</span><span class="sxs-lookup"><span data-stu-id="f21d2-183">Verify that no exceptions are logged in the debug console.</span></span>
3. <span data-ttu-id="f21d2-184">(Volitelné) Zobrazit aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler nebo [Postman][6].</span><span class="sxs-lookup"><span data-stu-id="f21d2-184">(Optional) View the updated data using either SQL Server Object Explorer or a REST tool like Fiddler or [Postman][6].</span></span> <span data-ttu-id="f21d2-185">Všimněte si, že data umístění byl synchronizován mezi back-endovou databázi a místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="f21d2-185">Notice the data has been synchronized between the backend database and the local store.</span></span>

    <span data-ttu-id="f21d2-186">Všimněte si data umístění byl synchronizován mezi databází a místní úložiště a obsahuje položky, které jste přidali v době, kdy byl odpojen vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f21d2-186">Notice the data has been synchronized between the database and the local store and contains the items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f21d2-187">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f21d2-187">Additional Resources</span></span>
* <span data-ttu-id="f21d2-188">[Synchronizaci dat offline v Azure Mobile Apps][2]</span><span class="sxs-lookup"><span data-stu-id="f21d2-188">[Offline Data Sync in Azure Mobile Apps][2]</span></span>
* <span data-ttu-id="f21d2-189">[Mobilní aplikace Azure .NET SDK postupy][8]</span><span class="sxs-lookup"><span data-stu-id="f21d2-189">[Azure Mobile Apps .NET SDK HOWTO][8]</span></span>

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
