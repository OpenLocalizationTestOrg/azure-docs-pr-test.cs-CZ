---
title: "Zapnutí offline synchronizace pro mobilní aplikace Azure (Cordova) | Microsoft Docs"
description: "Další informace o použití aplikace služby mobilní aplikace do mezipaměti a synchronizaci dat offline v aplikaci Cordova"
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
ms.openlocfilehash: 45e80ca672dfdb6defc6e5c1aac3d29f5479125c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a><span data-ttu-id="9279b-103">Zapnutí offline synchronizace pro mobilní aplikaci Cordova</span><span class="sxs-lookup"><span data-stu-id="9279b-103">Enable offline sync for your Cordova mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

<span data-ttu-id="9279b-104">Tento kurz představuje offline synchronizace funkci Azure Mobile Apps pro Cordova.</span><span class="sxs-lookup"><span data-stu-id="9279b-104">This tutorial introduces the offline sync feature of Azure Mobile Apps for Cordova.</span></span> <span data-ttu-id="9279b-105">Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="9279b-105">Offline sync allows end users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="9279b-106">Změny se ukládají do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="9279b-106">Changes are stored in a local database.</span></span>  <span data-ttu-id="9279b-107">Když je zařízení do režimu online, tyto změny se synchronizují s vzdálené služby.</span><span class="sxs-lookup"><span data-stu-id="9279b-107">Once the device is back online, these changes are synced with the remote service.</span></span>

<span data-ttu-id="9279b-108">V tomto kurzu vychází z řešení rychlý start Cordova pro mobilní aplikace, kterou vytvoříte po dokončení tohoto kurzu [Apache Cordova úvodní].</span><span class="sxs-lookup"><span data-stu-id="9279b-108">This tutorial is based on the Cordova quickstart solution for Mobile Apps that you create when you complete the tutorial [Apache Cordova quick start].</span></span> <span data-ttu-id="9279b-109">V tomto kurzu aktualizujete řešení rychlý start pro přidání offline funkcí Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="9279b-109">In this tutorial, you update the quickstart solution to add offline features of Azure Mobile Apps.</span></span>  <span data-ttu-id="9279b-110">Také jsme zvýrazněte offline konkrétní kódu v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9279b-110">We also highlight the offline-specific code in the app.</span></span>

<span data-ttu-id="9279b-111">Další informace o funkci offline synchronizace, naleznete v tématu [Offline synchronizací dat v Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="9279b-111">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps].</span></span> <span data-ttu-id="9279b-112">Podrobnosti využití rozhraní API najdete v tématu [dokumentaci k rozhraní API](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="9279b-112">For details of API usage, see the [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

## <a name="add-offline-sync-to-the-quickstart-solution"></a><span data-ttu-id="9279b-113">Přidejte offline synchronizace k řešení rychlý start</span><span class="sxs-lookup"><span data-stu-id="9279b-113">Add offline sync to the quickstart solution</span></span>
<span data-ttu-id="9279b-114">Kód offline synchronizace je nutné přidat do aplikace.</span><span class="sxs-lookup"><span data-stu-id="9279b-114">The offline sync code must be added to the app.</span></span> <span data-ttu-id="9279b-115">Offline synchronizace vyžaduje úložiště sqlite cordova modul plug-in, který automaticky získá přidán do vaší aplikace při modul plug-in Azure Mobile Apps je zahrnutý v projektu.</span><span class="sxs-lookup"><span data-stu-id="9279b-115">Offline sync requires the cordova-sqlite-storage plugin, which automatically gets added to your app when the Azure Mobile Apps plugin is included in the project.</span></span> <span data-ttu-id="9279b-116">Projektu pro rychlý start zahrnuje obě tyto moduly plug-in.</span><span class="sxs-lookup"><span data-stu-id="9279b-116">The Quickstart project includes both of these plugins.</span></span>

1. <span data-ttu-id="9279b-117">V Průzkumníku řešení v sadě Visual Studio otevřete index.js a nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="9279b-117">In Visual Studio's Solution Explorer, open index.js and replace the following code</span></span>

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable;      // Reference to a table endpoint on backend

    <span data-ttu-id="9279b-118">s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="9279b-118">with this code:</span></span>

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable,      // Reference to a table endpoint on backend
           syncContext;        // Reference to offline data sync context

2. <span data-ttu-id="9279b-119">Potom nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="9279b-119">Next, replace the following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    <span data-ttu-id="9279b-120">s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="9279b-120">with this code:</span></span>

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

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    <span data-ttu-id="9279b-121">Předchozí kód přidání inicializovat místní úložiště a definovat místní tabulku, která odpovídá sloupci hodnoty používané ve vaší službě Azure back-end.</span><span class="sxs-lookup"><span data-stu-id="9279b-121">The preceding code additions initialize the local store and define a local table that matches the column values used in your Azure back end.</span></span> <span data-ttu-id="9279b-122">(Není potřeba mají být zahrnuty všechny hodnoty ve sloupcích tento kód.)  `version` Pole se spravuje pomocí mobilního back-endu a používá se pro řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="9279b-122">(You don't need to include all column values in this code.)  The `version` field is maintained by the mobile backend and is used for conflict resolution.</span></span>

    <span data-ttu-id="9279b-123">Získat odkaz na kontext synchronizace voláním **getSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="9279b-123">You get a reference to the sync context by calling **getSyncContext**.</span></span> <span data-ttu-id="9279b-124">Kontext synchronizace umožňuje zachovat relace mezi tabulkami sledováním a vkládání změny ve všech tabulkách klientské aplikace změnil při `.push()` je volána.</span><span class="sxs-lookup"><span data-stu-id="9279b-124">The sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when `.push()` is called.</span></span>

3. <span data-ttu-id="9279b-125">Aktualizujte adresu URL aplikace na adresu URL aplikace mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="9279b-125">Update the application URL to your Mobile App application URL.</span></span>

4. <span data-ttu-id="9279b-126">Potom nahraďte tento kód:</span><span class="sxs-lookup"><span data-stu-id="9279b-126">Next, replace this code:</span></span>

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    <span data-ttu-id="9279b-127">s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="9279b-127">with this code:</span></span>

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    <span data-ttu-id="9279b-128">Předchozí kód inicializuje kontext synchronizace a potom volá getSyncTable (namísto jít) k získání odkazu na místní tabulku.</span><span class="sxs-lookup"><span data-stu-id="9279b-128">The preceding code initializes the sync context and then calls getSyncTable (instead of getTable) to get a reference to the local table.</span></span>

    <span data-ttu-id="9279b-129">Tento kód používá pro všechny místní databázi vytvořit, číst, aktualizovat a odstranit tabulku operace.</span><span class="sxs-lookup"><span data-stu-id="9279b-129">This code uses the local database for all create, read, update, and delete (CRUD) table operations.</span></span>

    <span data-ttu-id="9279b-130">Tato ukázka provádí zpracování na konflikty synchronizace jednoduché chyb.</span><span class="sxs-lookup"><span data-stu-id="9279b-130">This sample performs simple error handling on sync conflicts.</span></span> <span data-ttu-id="9279b-131">Reálné aplikaci byste zpracovávat různé chyby jako síťové podmínky, server je v konfliktu a dalších.</span><span class="sxs-lookup"><span data-stu-id="9279b-131">A real application would handle the various errors like network conditions, server conflicts, and others.</span></span> <span data-ttu-id="9279b-132">Příklady kódu najdete v tématu [offline synchronizace ukázka].</span><span class="sxs-lookup"><span data-stu-id="9279b-132">For code examples, see the [offline sync sample].</span></span>

5. <span data-ttu-id="9279b-133">Dál přidejte této funkci můžete provést skutečné synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="9279b-133">Next, add this function to perform the actual sync.</span></span>

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    <span data-ttu-id="9279b-134">Rozhodnete, kdy k replikaci změn na back-end mobilní aplikace při volání **syncContext.push()**.</span><span class="sxs-lookup"><span data-stu-id="9279b-134">You decide when to push changes to the Mobile App backend by calling **syncContext.push()**.</span></span> <span data-ttu-id="9279b-135">Například může volat **syncBackend** v obslužné rutiny události tlačítka vázaný na tlačítko synchronizovat.</span><span class="sxs-lookup"><span data-stu-id="9279b-135">For example, you could call **syncBackend** in a button event handler tied to a sync button.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="9279b-136">Důležité informace o offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="9279b-136">Offline sync considerations</span></span>

<span data-ttu-id="9279b-137">V ukázce **nabízené** metodu **syncContext** je volána pouze při spuštění aplikace ve funkci zpětného volání pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9279b-137">In the sample, the **push** method of **syncContext** is only called on app startup in the callback function for login.</span></span>  <span data-ttu-id="9279b-138">V reálné aplikaci může také mít tato funkce synchronizaci spustit ručně nebo při změně stavu sítě.</span><span class="sxs-lookup"><span data-stu-id="9279b-138">In a real-world application, you could also make this sync functionality triggered manually or when the network state changes.</span></span>

<span data-ttu-id="9279b-139">Když je spustit vyžádání pro tabulku, která má čekající místní aktualizace sleduje kontext, který pull operace automaticky aktivuje push.</span><span class="sxs-lookup"><span data-stu-id="9279b-139">When a pull is executed against a table that has pending local updates tracked by the context, that pull operation automatically triggers a push.</span></span> <span data-ttu-id="9279b-140">Při aktualizaci, přidávání a dokončení položky v této ukázce, můžete vynechat explicitní **nabízené** volat, protože je redundantní.</span><span class="sxs-lookup"><span data-stu-id="9279b-140">When refreshing, adding, and completing items in this sample, you can omit the explicit **push** call, since it may be redundant.</span></span>

<span data-ttu-id="9279b-141">V zadané kódu jsou předmětem dotazování všechny záznamy v tabulce vzdálené todoItem, ale je také možné filtrování záznamů předáním id dotazu a dotazy s cílem **nabízené**.</span><span class="sxs-lookup"><span data-stu-id="9279b-141">In the provided code, all records in the remote todoItem table are queried, but it is also possible to filter records by passing a query id and query to **push**.</span></span> <span data-ttu-id="9279b-142">Další informace najdete v části *přírůstkové synchronizace* v [Offline synchronizací dat v Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="9279b-142">For more information, see the section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="optional-disable-authentication"></a><span data-ttu-id="9279b-143">(Volitelné) Zakázat ověřování</span><span class="sxs-lookup"><span data-stu-id="9279b-143">(Optional) Disable authentication</span></span>

<span data-ttu-id="9279b-144">Pokud nechcete nastavit ověřování před testování offline synchronizace, komentář funkce zpětného volání pro přihlášení, ale nechte kód uvnitř uncommented funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="9279b-144">If you don't want to set up authentication before testing offline sync, comment out the callback function for login, but leave the code inside the callback function uncommented.</span></span>  <span data-ttu-id="9279b-145">Po komentářů řádkům přihlášení, následuje kód:</span><span class="sxs-lookup"><span data-stu-id="9279b-145">After commenting out the login lines, the code follows:</span></span>

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

<span data-ttu-id="9279b-146">Nyní v aplikaci synchronizace s Azure back-end při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="9279b-146">Now, the app syncs with the Azure backend when you run the app.</span></span>

## <a name="run-the-client-app"></a><span data-ttu-id="9279b-147">Spustit klientskou aplikaci</span><span class="sxs-lookup"><span data-stu-id="9279b-147">Run the client app</span></span>
<span data-ttu-id="9279b-148">S offline synchronizací teď povolená můžete spustit klientskou aplikaci alespoň jednou na každou platformu k naplnění databáze místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="9279b-148">With offline sync now enabled, you can run the client application at least once on each platform to populate the local store database.</span></span> <span data-ttu-id="9279b-149">Později simulovat offline scénář a upravit data v místním úložišti, zatímco aplikace v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="9279b-149">Later, simulate an offline scenario and modify the data in the local store while the app is offline.</span></span>

## <a name="optional-test-the-sync-behavior"></a><span data-ttu-id="9279b-150">(Volitelné) Testování chování synchronizace</span><span class="sxs-lookup"><span data-stu-id="9279b-150">(Optional) Test the sync behavior</span></span>
<span data-ttu-id="9279b-151">V této části upravíte projektu klienta k simulaci offline scénář pomocí adresy URL Neplatná aplikace pro váš back-end.</span><span class="sxs-lookup"><span data-stu-id="9279b-151">In this section, you modify the client project to simulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="9279b-152">Při přidání nebo změna datových položek, tyto změny jsou uložené v místním úložišti, ale nejsou synchronizovány do úložiště dat back-end, dokud nebude znovu navázané připojení.</span><span class="sxs-lookup"><span data-stu-id="9279b-152">When you add or change data items, these changes are held in the local store, but are not synced to the backend data store until the connection is re-established.</span></span>

1. <span data-ttu-id="9279b-153">V Průzkumníku řešení otevřete soubor projektu index.js a změňte adresu URL aplikace tak, aby odkazovalo na neplatnou adresu URL, například následující kód:</span><span class="sxs-lookup"><span data-stu-id="9279b-153">In the Solution Explorer, open the index.js project file and change the application URL to point to  an invalid URL, like the following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. <span data-ttu-id="9279b-154">V index.html, aktualizace CSP `<meta>` element se stejným neplatná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="9279b-154">In index.html, update the CSP `<meta>` element with the same invalid URL.</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. <span data-ttu-id="9279b-155">Sestavit a spustit klientskou aplikaci a Všimněte si, že výjimku přihlášen v konzole, když se aplikace pokusí synchronizovat s back-end po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9279b-155">Build and run the client app and notice that an exception is logged in the console when the app attempts to sync with the backend after login.</span></span> <span data-ttu-id="9279b-156">Všechny nové položky, které přidáte existovat pouze v místním úložišti, dokud se instaluje do mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="9279b-156">Any new items you add exist only in the local store until they are pushed to the mobile backend.</span></span> <span data-ttu-id="9279b-157">Klientská aplikace se chová jako kdyby je připojené k back-end.</span><span class="sxs-lookup"><span data-stu-id="9279b-157">The client app behaves as if it is connected to the backend.</span></span>

4. <span data-ttu-id="9279b-158">Zavřete aplikaci a restartujte ji k ověření, že nové položky, které jste vytvořili, jsou nastavené jako trvalé do místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="9279b-158">Close the app and restart it to verify that the new items you created are persisted to the local store.</span></span>

5. <span data-ttu-id="9279b-159">(Volitelné) Pomocí sady Visual Studio zobrazíte tabulka Azure SQL Database v tématu, že se nezměnil na data v databázi back-end.</span><span class="sxs-lookup"><span data-stu-id="9279b-159">(Optional) Use Visual Studio to view your Azure SQL Database table to see that the data in the backend database has not changed.</span></span>

    <span data-ttu-id="9279b-160">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="9279b-160">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="9279b-161">Přejděte k vaší databázi v **Azure**->**databází SQL**.</span><span class="sxs-lookup"><span data-stu-id="9279b-161">Navigate to your database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="9279b-162">Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="9279b-162">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="9279b-163">Teď můžete procházet tabulku databáze SQL a její obsah.</span><span class="sxs-lookup"><span data-stu-id="9279b-163">Now you can browse to your SQL database table and its contents.</span></span>

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a><span data-ttu-id="9279b-164">(Volitelné) Testování obnovení připojení na vaše mobilní back-end</span><span class="sxs-lookup"><span data-stu-id="9279b-164">(Optional) Test the reconnection to your mobile backend</span></span>

<span data-ttu-id="9279b-165">V této části je znovu připojit aplikaci mobilního back-end, který simuluje aplikace vracející se zpět do stavu online.</span><span class="sxs-lookup"><span data-stu-id="9279b-165">In this section, you reconnect the app to the mobile backend, which simulates the app coming back to an online state.</span></span> <span data-ttu-id="9279b-166">Při přihlášení, data se synchronizované s vaší mobilní back-end.</span><span class="sxs-lookup"><span data-stu-id="9279b-166">When you log in, data is synced to your mobile backend.</span></span>

1. <span data-ttu-id="9279b-167">Index.js znovu otevřete a obnovte adresy URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="9279b-167">Reopen index.js and restore the application URL.</span></span>
2. <span data-ttu-id="9279b-168">Otevřete index.html a opravte adresu URL aplikace v CSP `<meta>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9279b-168">Reopen index.html and correct the application URL in the CSP `<meta>` element.</span></span>
3. <span data-ttu-id="9279b-169">Znovu sestavit a spustit klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9279b-169">Rebuild and run the client app.</span></span> <span data-ttu-id="9279b-170">Aplikace se pokusí synchronizovat s back-end mobilní aplikace po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9279b-170">The app attempts to sync with the mobile app backend after login.</span></span> <span data-ttu-id="9279b-171">Ověřte, že žádné výjimky jsou zaznamenány v konzole pro ladění.</span><span class="sxs-lookup"><span data-stu-id="9279b-171">Verify that no exceptions are logged in the debug console.</span></span>
4. <span data-ttu-id="9279b-172">(Volitelné) Zobrazte aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler.</span><span class="sxs-lookup"><span data-stu-id="9279b-172">(Optional) View the updated data using either SQL Server Object Explorer or a REST tool like Fiddler.</span></span> <span data-ttu-id="9279b-173">Všimněte si, že data umístění byl synchronizován mezi back-endovou databázi a místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="9279b-173">Notice the data has been synchronized between the backend database and the local store.</span></span>

    <span data-ttu-id="9279b-174">Všimněte si data umístění byl synchronizován mezi databází a místní úložiště a obsahuje položky, které jste přidali v době, kdy byl odpojen vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9279b-174">Notice the data has been synchronized between the database and the local store and contains the items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9279b-175">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9279b-175">Additional resources</span></span>
* <span data-ttu-id="9279b-176">[Offline synchronizací dat v Azure Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="9279b-176">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="9279b-177">[Visual Studio Tools for Apache Cordova]</span><span class="sxs-lookup"><span data-stu-id="9279b-177">[Visual Studio Tools for Apache Cordova]</span></span>

## <a name="next-steps"></a><span data-ttu-id="9279b-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9279b-178">Next steps</span></span>
* <span data-ttu-id="9279b-179">Kontrola více rozšířené funkce offline synchronizace, jako je řešení konfliktů v [offline synchronizace ukázka]</span><span class="sxs-lookup"><span data-stu-id="9279b-179">Review more advanced offline sync features such as conflict resolution in the [offline sync sample]</span></span>
* <span data-ttu-id="9279b-180">Zkontrolujte odkaz na offline synchronizace rozhraní API v [dokumentaci k rozhraní API](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="9279b-180">Review the offline sync API reference in the [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
<span data-ttu-id="9279b-181">[Apache Cordova úvodní]: app-service-mobile-cordova-get-started.md</span><span class="sxs-lookup"><span data-stu-id="9279b-181">[Apache Cordova quick start]: app-service-mobile-cordova-get-started.md</span></span>
<span data-ttu-id="9279b-182">[offline synchronizace ukázka]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling</span><span class="sxs-lookup"><span data-stu-id="9279b-182">[offline sync sample]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling</span></span>
<span data-ttu-id="9279b-183">[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="9279b-183">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>
[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
<span data-ttu-id="9279b-184">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span><span class="sxs-lookup"><span data-stu-id="9279b-184">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span></span>
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
