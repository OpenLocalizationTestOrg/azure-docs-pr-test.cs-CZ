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
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a><span data-ttu-id="4dd97-103">Zapnutí offline synchronizace pro mobilní aplikaci Cordova</span><span class="sxs-lookup"><span data-stu-id="4dd97-103">Enable offline sync for your Cordova mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

<span data-ttu-id="4dd97-104">Tento kurz představuje hello offline synchronizace funkci Azure Mobile Apps pro Cordova.</span><span class="sxs-lookup"><span data-stu-id="4dd97-104">This tutorial introduces hello offline sync feature of Azure Mobile Apps for Cordova.</span></span> <span data-ttu-id="4dd97-105">Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="4dd97-105">Offline sync allows end users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="4dd97-106">Změny se ukládají do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="4dd97-106">Changes are stored in a local database.</span></span>  <span data-ttu-id="4dd97-107">Jakmile hello zařízení do režimu online, tyto změny se synchronizují s hello vzdálené služby.</span><span class="sxs-lookup"><span data-stu-id="4dd97-107">Once hello device is back online, these changes are synced with hello remote service.</span></span>

<span data-ttu-id="4dd97-108">V tomto kurzu vychází hello Cordova rychlý start řešení pro mobilní aplikace, které vytvoříte po dokončení hello kurzu [Apache Cordova úvodní].</span><span class="sxs-lookup"><span data-stu-id="4dd97-108">This tutorial is based on hello Cordova quickstart solution for Mobile Apps that you create when you complete hello tutorial [Apache Cordova quick start].</span></span> <span data-ttu-id="4dd97-109">V tomto kurzu aktualizujete hello rychlý start řešení tooadd offline funkce Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="4dd97-109">In this tutorial, you update hello quickstart solution tooadd offline features of Azure Mobile Apps.</span></span>  <span data-ttu-id="4dd97-110">Také jsme zvýrazněte hello offline konkrétní kódu v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="4dd97-110">We also highlight hello offline-specific code in hello app.</span></span>

<span data-ttu-id="4dd97-111">toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="4dd97-111">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps].</span></span> <span data-ttu-id="4dd97-112">Podrobnosti využití rozhraní API najdete v tématu hello [dokumentaci k rozhraní API](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="4dd97-112">For details of API usage, see hello [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

## <a name="add-offline-sync-toohello-quickstart-solution"></a><span data-ttu-id="4dd97-113">Přidat řešení rychlý start toohello offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="4dd97-113">Add offline sync toohello quickstart solution</span></span>
<span data-ttu-id="4dd97-114">Hello offline synchronizace kód musí být přidaný toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dd97-114">hello offline sync code must be added toohello app.</span></span> <span data-ttu-id="4dd97-115">Offline synchronizace vyžaduje hello úložiště sqlite cordova-plugin, který automaticky přidá tooyour aplikace při hello plugin Azure Mobile Apps je součástí projektu hello.</span><span class="sxs-lookup"><span data-stu-id="4dd97-115">Offline sync requires hello cordova-sqlite-storage plugin, which automatically gets added tooyour app when hello Azure Mobile Apps plugin is included in hello project.</span></span> <span data-ttu-id="4dd97-116">projekt Hello rychlý start zahrnuje obě tyto moduly plug-in.</span><span class="sxs-lookup"><span data-stu-id="4dd97-116">hello Quickstart project includes both of these plugins.</span></span>

1. <span data-ttu-id="4dd97-117">V Průzkumníku řešení v sadě Visual Studio otevřete index.js a nahraďte hello následující kód</span><span class="sxs-lookup"><span data-stu-id="4dd97-117">In Visual Studio's Solution Explorer, open index.js and replace hello following code</span></span>

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable;      // Reference tooa table endpoint on backend

    <span data-ttu-id="4dd97-118">s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="4dd97-118">with this code:</span></span>

        var client,            // Connection toohello Azure Mobile App backend
           todoItemTable,      // Reference tooa table endpoint on backend
           syncContext;        // Reference toooffline data sync context

2. <span data-ttu-id="4dd97-119">Potom nahraďte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="4dd97-119">Next, replace hello following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    <span data-ttu-id="4dd97-120">s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="4dd97-120">with this code:</span></span>

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

    <span data-ttu-id="4dd97-121">Hello předchozí kód přidání inicializovat hello místního úložiště a definovat místní tabulku, která odpovídá hello sloupec hodnoty používané ve vaší službě Azure back-end.</span><span class="sxs-lookup"><span data-stu-id="4dd97-121">hello preceding code additions initialize hello local store and define a local table that matches hello column values used in your Azure back end.</span></span> <span data-ttu-id="4dd97-122">(Není potřeba tooinclude všechny hodnoty ve sloupcích v tento kód.)  Hello `version` pole se spravuje pomocí mobilního back-endu hello a používá se pro řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="4dd97-122">(You don't need tooinclude all column values in this code.)  hello `version` field is maintained by hello mobile backend and is used for conflict resolution.</span></span>

    <span data-ttu-id="4dd97-123">Získat kontext synchronizace toohello odkaz voláním **getSyncContext**.</span><span class="sxs-lookup"><span data-stu-id="4dd97-123">You get a reference toohello sync context by calling **getSyncContext**.</span></span> <span data-ttu-id="4dd97-124">Hello kontext synchronizace umožňuje zachovat relace mezi tabulkami sledováním a vkládání změny ve všech tabulkách klientské aplikace změnil při `.push()` je volána.</span><span class="sxs-lookup"><span data-stu-id="4dd97-124">hello sync context helps preserve table relationships by tracking and pushing changes in all tables a client app has modified when `.push()` is called.</span></span>

3. <span data-ttu-id="4dd97-125">Aktualizujte hello aplikace URL tooyour mobilní aplikace adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dd97-125">Update hello application URL tooyour Mobile App application URL.</span></span>

4. <span data-ttu-id="4dd97-126">Potom nahraďte tento kód:</span><span class="sxs-lookup"><span data-stu-id="4dd97-126">Next, replace this code:</span></span>

        todoItemTable = client.getTable('todoitem'); // todoitem is hello table name

    <span data-ttu-id="4dd97-127">s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="4dd97-127">with this code:</span></span>

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

    <span data-ttu-id="4dd97-128">Hello předchozí kód inicializuje kontext synchronizace hello a pak zavolá tooget getSyncTable (namísto jít) na místní tabulku toohello odkaz.</span><span class="sxs-lookup"><span data-stu-id="4dd97-128">hello preceding code initializes hello sync context and then calls getSyncTable (instead of getTable) tooget a reference toohello local table.</span></span>

    <span data-ttu-id="4dd97-129">Tento kód používá hello místní databáze pro všechny vytvářet, číst, aktualizovat a odstranit tabulku operace.</span><span class="sxs-lookup"><span data-stu-id="4dd97-129">This code uses hello local database for all create, read, update, and delete (CRUD) table operations.</span></span>

    <span data-ttu-id="4dd97-130">Tato ukázka provádí zpracování na konflikty synchronizace jednoduché chyb.</span><span class="sxs-lookup"><span data-stu-id="4dd97-130">This sample performs simple error handling on sync conflicts.</span></span> <span data-ttu-id="4dd97-131">Reálné aplikaci byste zpracování hello různých chybám, jako je síťové podmínky, server je v konfliktu a dalších.</span><span class="sxs-lookup"><span data-stu-id="4dd97-131">A real application would handle hello various errors like network conditions, server conflicts, and others.</span></span> <span data-ttu-id="4dd97-132">Příklady kódu najdete v tématu hello [offline synchronizace ukázka].</span><span class="sxs-lookup"><span data-stu-id="4dd97-132">For code examples, see hello [offline sync sample].</span></span>

5. <span data-ttu-id="4dd97-133">Dál přidejte funkce skutečného synchronizace tooperform hello tento.</span><span class="sxs-lookup"><span data-stu-id="4dd97-133">Next, add this function tooperform hello actual sync.</span></span>

        function syncBackend() {

          // Sync local store tooAzure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from hello Azure table after syncing tooAzure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    <span data-ttu-id="4dd97-134">Rozhodnete, že když toopush změny back-end mobilní aplikace toohello voláním **syncContext.push()**.</span><span class="sxs-lookup"><span data-stu-id="4dd97-134">You decide when toopush changes toohello Mobile App backend by calling **syncContext.push()**.</span></span> <span data-ttu-id="4dd97-135">Například může volat **syncBackend** v tlačítko Synchronizovat vázanou tooa tlačítko událost obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="4dd97-135">For example, you could call **syncBackend** in a button event handler tied tooa sync button.</span></span>

## <a name="offline-sync-considerations"></a><span data-ttu-id="4dd97-136">Důležité informace o offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="4dd97-136">Offline sync considerations</span></span>

<span data-ttu-id="4dd97-137">V ukázce hello hello **nabízené** metodu **syncContext** je volána pouze při spuštění aplikace v hello funkce zpětného volání pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4dd97-137">In hello sample, hello **push** method of **syncContext** is only called on app startup in hello callback function for login.</span></span>  <span data-ttu-id="4dd97-138">V reálné aplikaci může také mít tato funkce synchronizaci spustit ručně nebo při změně stavu sítě hello.</span><span class="sxs-lookup"><span data-stu-id="4dd97-138">In a real-world application, you could also make this sync functionality triggered manually or when hello network state changes.</span></span>

<span data-ttu-id="4dd97-139">Když je spustit vyžádání pro tabulku, která má čekající místní aktualizace sleduje hello kontext, který pull operace automaticky aktivuje push.</span><span class="sxs-lookup"><span data-stu-id="4dd97-139">When a pull is executed against a table that has pending local updates tracked by hello context, that pull operation automatically triggers a push.</span></span> <span data-ttu-id="4dd97-140">Při aktualizaci, přidání a dokončení položky v této ukázce, můžete vynechat hello explicitní **nabízené** volat, protože je redundantní.</span><span class="sxs-lookup"><span data-stu-id="4dd97-140">When refreshing, adding, and completing items in this sample, you can omit hello explicit **push** call, since it may be redundant.</span></span>

<span data-ttu-id="4dd97-141">V kódu hello poskytuje, jsou předmětem dotazování všechny záznamy v tabulce vzdálené todoItem hello, ale je také možné toofilter záznamy předáním id dotazu a dotaz příliš**nabízené**.</span><span class="sxs-lookup"><span data-stu-id="4dd97-141">In hello provided code, all records in hello remote todoItem table are queried, but it is also possible toofilter records by passing a query id and query too**push**.</span></span> <span data-ttu-id="4dd97-142">Další informace najdete v tématu hello *přírůstkové synchronizace* v [Offline synchronizací dat v Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="4dd97-142">For more information, see hello section *Incremental Sync* in [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="optional-disable-authentication"></a><span data-ttu-id="4dd97-143">(Volitelné) Zakázat ověřování</span><span class="sxs-lookup"><span data-stu-id="4dd97-143">(Optional) Disable authentication</span></span>

<span data-ttu-id="4dd97-144">Pokud nemáte má tooset až ověřování před testováním offline synchronizace, komentář hello funkce zpětného volání pro přihlášení, ale ponechte hello kód uvnitř uncommented hello funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="4dd97-144">If you don't want tooset up authentication before testing offline sync, comment out hello callback function for login, but leave hello code inside hello callback function uncommented.</span></span>  <span data-ttu-id="4dd97-145">Po komentářů hello přihlášení řádky, následuje hello kódu:</span><span class="sxs-lookup"><span data-stu-id="4dd97-145">After commenting out hello login lines, hello code follows:</span></span>

      // Login toohello service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave hello rest of hello code in this callback function  uncommented.
                ...
        });
      // }, handleError);

<span data-ttu-id="4dd97-146">Nyní hello s hello Azure back-end při spuštění aplikace hello synchronizacemi aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dd97-146">Now, hello app syncs with hello Azure backend when you run hello app.</span></span>

## <a name="run-hello-client-app"></a><span data-ttu-id="4dd97-147">Spustit klientskou aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="4dd97-147">Run hello client app</span></span>
<span data-ttu-id="4dd97-148">S offline synchronizací teď povolená můžete spustit hello klientská aplikace alespoň jednou na každou platformu k naplnění databáze hello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="4dd97-148">With offline sync now enabled, you can run hello client application at least once on each platform to populate hello local store database.</span></span> <span data-ttu-id="4dd97-149">Později simulovat offline scénář a upravit data hello v místním úložišti hello aplikace hello je offline.</span><span class="sxs-lookup"><span data-stu-id="4dd97-149">Later, simulate an offline scenario and modify hello data in hello local store while hello app is offline.</span></span>

## <a name="optional-test-hello-sync-behavior"></a><span data-ttu-id="4dd97-150">(Volitelné) Test hello synchronizace chování</span><span class="sxs-lookup"><span data-stu-id="4dd97-150">(Optional) Test hello sync behavior</span></span>
<span data-ttu-id="4dd97-151">V této části upravíte hello klienta projektu toosimulate offline scénář pomocí adresy URL Neplatná aplikace pro váš back-end.</span><span class="sxs-lookup"><span data-stu-id="4dd97-151">In this section, you modify hello client project toosimulate an offline scenario by using an invalid application URL for your backend.</span></span> <span data-ttu-id="4dd97-152">Při přidání nebo změna datových položek, tyto změny jsou uložené v místním úložišti, ale nejsou synchronizovaná toohello back-end úložišti, dokud nebude znovu navázané připojení hello.</span><span class="sxs-lookup"><span data-stu-id="4dd97-152">When you add or change data items, these changes are held in the local store, but are not synced toohello backend data store until hello connection is re-established.</span></span>

1. <span data-ttu-id="4dd97-153">V Průzkumníku řešení hello otevřete soubor projektu index.js hello a změňte toopoint adresu URL aplikace hello neplatná adresa URL, jako je hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="4dd97-153">In hello Solution Explorer, open hello index.js project file and change hello application URL toopoint to  an invalid URL, like hello following code:</span></span>

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. <span data-ttu-id="4dd97-154">V index.html, aktualizace hello CSP `<meta>` element s hello stejné neplatná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="4dd97-154">In index.html, update hello CSP `<meta>` element with hello same invalid URL.</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. <span data-ttu-id="4dd97-155">Sestavit a spustit hello klientskou aplikaci a Všimněte si, že výjimku protokolu konzole hello když aplikace hello pokusí synchronizovat s back-end hello po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4dd97-155">Build and run hello client app and notice that an exception is logged in hello console when hello app attempts to sync with hello backend after login.</span></span> <span data-ttu-id="4dd97-156">Všechny nové položky, které přidáte existovat pouze v místním úložišti hello, dokud se odesílají toohello mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="4dd97-156">Any new items you add exist only in hello local store until they are pushed toohello mobile backend.</span></span> <span data-ttu-id="4dd97-157">klientská aplikace Hello se chová, pokud je připojený toohello back-end.</span><span class="sxs-lookup"><span data-stu-id="4dd97-157">hello client app behaves as if it is connected toohello backend.</span></span>

4. <span data-ttu-id="4dd97-158">Zavření aplikace hello a restartujte ji tooverify že hello nové položky, kterou jste vytvořili jsou trvalé toohello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="4dd97-158">Close hello app and restart it tooverify that hello new items you created are persisted toohello local store.</span></span>

5. <span data-ttu-id="4dd97-159">(Volitelné) Pomocí sady Visual Studio tooview vaše toosee tabulky Azure SQL Database, nezměnil hello data v databázi back-end hello.</span><span class="sxs-lookup"><span data-stu-id="4dd97-159">(Optional) Use Visual Studio tooview your Azure SQL Database table toosee that hello data in hello backend database has not changed.</span></span>

    <span data-ttu-id="4dd97-160">V sadě Visual Studio otevřete **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="4dd97-160">In Visual Studio, open **Server Explorer**.</span></span> <span data-ttu-id="4dd97-161">Přejděte tooyour databáze v **Azure**->**databází SQL**.</span><span class="sxs-lookup"><span data-stu-id="4dd97-161">Navigate tooyour database in **Azure**->**SQL Databases**.</span></span> <span data-ttu-id="4dd97-162">Klikněte pravým tlačítkem na databázi a vyberte **otevřít v Průzkumníku objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="4dd97-162">Right-click your database and select **Open in SQL Server Object Explorer**.</span></span> <span data-ttu-id="4dd97-163">Teď můžete procházet tooyour tabulce databáze SQL a její obsah.</span><span class="sxs-lookup"><span data-stu-id="4dd97-163">Now you can browse tooyour SQL database table and its contents.</span></span>

## <a name="optional-test-hello-reconnection-tooyour-mobile-backend"></a><span data-ttu-id="4dd97-164">(Volitelné) Test hello opětovné připojení tooyour mobilní back-end</span><span class="sxs-lookup"><span data-stu-id="4dd97-164">(Optional) Test hello reconnection tooyour mobile backend</span></span>

<span data-ttu-id="4dd97-165">V této části je znovu připojit hello toohello mobilní back-end aplikace, který simuluje hello aplikace vracející se zpět do stavu online.</span><span class="sxs-lookup"><span data-stu-id="4dd97-165">In this section, you reconnect hello app toohello mobile backend, which simulates hello app coming back to an online state.</span></span> <span data-ttu-id="4dd97-166">Při přihlášení, data jsou synchronizovaná tooyour mobilního back-endu.</span><span class="sxs-lookup"><span data-stu-id="4dd97-166">When you log in, data is synced tooyour mobile backend.</span></span>

1. <span data-ttu-id="4dd97-167">Index.js znovu otevřete a obnovte hello adresa URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dd97-167">Reopen index.js and restore hello application URL.</span></span>
2. <span data-ttu-id="4dd97-168">Otevřete index.html a opravte hello adresa URL aplikace v hello CSP `<meta>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4dd97-168">Reopen index.html and correct hello application URL in hello CSP `<meta>` element.</span></span>
3. <span data-ttu-id="4dd97-169">Znovu sestavit a spustit klientskou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="4dd97-169">Rebuild and run hello client app.</span></span> <span data-ttu-id="4dd97-170">Hello toosync pokusy o aplikaci s back-end mobilní aplikace hello po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4dd97-170">hello app attempts toosync with hello mobile app backend after login.</span></span> <span data-ttu-id="4dd97-171">Ověřte, že žádné výjimky jsou zaznamenány do konzoly ladění hello.</span><span class="sxs-lookup"><span data-stu-id="4dd97-171">Verify that no exceptions are logged in hello debug console.</span></span>
4. <span data-ttu-id="4dd97-172">(Volitelné) Zobrazení hello aktualizovaná data pomocí Průzkumníka objektů systému SQL Server nebo REST nástroje, například aplikaci Fiddler.</span><span class="sxs-lookup"><span data-stu-id="4dd97-172">(Optional) View hello updated data using either SQL Server Object Explorer or a REST tool like Fiddler.</span></span> <span data-ttu-id="4dd97-173">Všimněte si hello data umístění byl synchronizován mezi hello back-endovou databázi a hello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="4dd97-173">Notice hello data has been synchronized between hello backend database and hello local store.</span></span>

    <span data-ttu-id="4dd97-174">Všimněte si hello data umístění byl synchronizován mezi hello databáze a místní úložiště hello a obsahuje hello položky, které jste přidali v době, kdy byl odpojen vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dd97-174">Notice hello data has been synchronized between hello database and hello local store and contains hello items you added while your app was disconnected.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4dd97-175">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4dd97-175">Additional resources</span></span>
* <span data-ttu-id="4dd97-176">[Offline synchronizací dat v Azure Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="4dd97-176">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="4dd97-177">[Visual Studio Tools for Apache Cordova]</span><span class="sxs-lookup"><span data-stu-id="4dd97-177">[Visual Studio Tools for Apache Cordova]</span></span>

## <a name="next-steps"></a><span data-ttu-id="4dd97-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4dd97-178">Next steps</span></span>
* <span data-ttu-id="4dd97-179">Kontrola více rozšířené funkce offline synchronizace, jako je řešení konfliktů v hello [offline synchronizace ukázka]</span><span class="sxs-lookup"><span data-stu-id="4dd97-179">Review more advanced offline sync features such as conflict resolution in hello [offline sync sample]</span></span>
* <span data-ttu-id="4dd97-180">Zkontrolujte hello offline synchronizace referenční dokumentace rozhraní API v hello [dokumentaci k rozhraní API](https://azure.github.io/azure-mobile-apps-js-client).</span><span class="sxs-lookup"><span data-stu-id="4dd97-180">Review hello offline sync API reference in hello [API documentation](https://azure.github.io/azure-mobile-apps-js-client).</span></span>

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
