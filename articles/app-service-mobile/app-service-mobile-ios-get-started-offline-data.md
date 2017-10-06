---
title: "aaaEnable offline synchronizace u mobilních aplikací pro iOS | Microsoft Docs"
description: "Zjistěte, jak toouse Azure App Service mobilní aplikace toocache a synchronizaci dat offline v aplikacích pro iOS."
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
services: app-service\mobile
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 570ea7cf6694ab7317c977331038929b64508ad3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a><span data-ttu-id="58b14-103">Povolit offline synchronizace u mobilních aplikací pro iOS</span><span class="sxs-lookup"><span data-stu-id="58b14-103">Enable offline syncing with iOS mobile apps</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="58b14-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="58b14-104">Overview</span></span>
<span data-ttu-id="58b14-105">Tento kurz se zaměřuje na offline synchronizace s hello funkce Azure App Service Mobile Apps pro iOS.</span><span class="sxs-lookup"><span data-stu-id="58b14-105">This tutorial covers offline syncing with hello Mobile Apps feature of Azure App Service for iOS.</span></span> <span data-ttu-id="58b14-106">S offline synchronizaci koncoví uživatelé můžete pracovat s tooview mobilní aplikace, přidat nebo upravit data, i když mají žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="58b14-106">With offline syncing end-users can interact with a mobile app tooview, add, or modify data, even when they have no network connection.</span></span> <span data-ttu-id="58b14-107">Změny se ukládají do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="58b14-107">Changes are stored in a local database.</span></span> <span data-ttu-id="58b14-108">Když hello zařízení do režimu online, změny hello synchronizované s hello vzdálené back end.</span><span class="sxs-lookup"><span data-stu-id="58b14-108">After hello device is back online, hello changes are synced with hello remote back end.</span></span>

<span data-ttu-id="58b14-109">Pokud je vaše první zkušenosti s Mobile Apps, byste měli nejprve dokončit kurz hello [vytvořit aplikaci pro iOS].</span><span class="sxs-lookup"><span data-stu-id="58b14-109">If this is your first experience with Mobile Apps, you should first complete hello tutorial [Create an iOS App].</span></span> <span data-ttu-id="58b14-110">Pokud nepoužijete hello stáhli úvodní serverový projekt, musíte přidat hello přístup k datům rozšíření balíčky tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="58b14-110">If you do not use hello downloaded quick-start server project, you must add hello data-access extension packages tooyour project.</span></span> <span data-ttu-id="58b14-111">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="58b14-111">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="58b14-112">toolearn Další informace o funkci hello offline synchronizace, najdete v části [Offline synchronizací dat v Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="58b14-112">toolearn more about hello offline sync feature, see [Offline Data Sync in Mobile Apps].</span></span>

## <span data-ttu-id="58b14-113"><a name="review-sync"></a>Zkontrolujte kód synchronizace klienta hello</span><span class="sxs-lookup"><span data-stu-id="58b14-113"><a name="review-sync"></a>Review hello client sync code</span></span>
<span data-ttu-id="58b14-114">Hello klientského projektu, který jste stáhli pro hello [vytvořit aplikaci pro iOS] kurzu již obsahuje kód, který podporuje offline synchronizace pomocí místní databáze založené na datech jádra.</span><span class="sxs-lookup"><span data-stu-id="58b14-114">hello client project that you downloaded for hello [Create an iOS App] tutorial already contains code that supports offline synchronization using a local Core Data-based database.</span></span> <span data-ttu-id="58b14-115">Tento oddíl shrnuje, co je již součástí hello kódu.</span><span class="sxs-lookup"><span data-stu-id="58b14-115">This section summarizes what is already included in hello tutorial code.</span></span> <span data-ttu-id="58b14-116">Koncepční přehled hello funkce, najdete v tématu [Offline synchronizací dat v Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="58b14-116">For a conceptual overview of hello feature, see [Offline Data Sync in Mobile Apps].</span></span>

<span data-ttu-id="58b14-117">Pomocí funkce offline synchronizací dat hello mobilních aplikací mohou koncoví uživatelé komunikovat s místní databáze, i v případě, že síť hello je nepřístupný.</span><span class="sxs-lookup"><span data-stu-id="58b14-117">Using hello offline data-sync feature of Mobile Apps, end-users can interact with a local database even when hello network is inaccessible.</span></span> <span data-ttu-id="58b14-118">toouse tyto funkce ve vaší aplikaci inicializovat kontext synchronizace hello `MSClient` a odkazovat na místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="58b14-118">toouse these features in your app, you initialize hello sync context of `MSClient` and reference a local store.</span></span> <span data-ttu-id="58b14-119">Pak referenční tabulku prostřednictvím hello **MSSyncTable** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="58b14-119">Then you reference your table through hello **MSSyncTable** interface.</span></span>

<span data-ttu-id="58b14-120">V **QSTodoService.m** (Objective-C) nebo **ToDoTableViewController.swift** (Swift), Všimněte si, že hello typ člena hello **syncTable** je  **MSSyncTable**.</span><span class="sxs-lookup"><span data-stu-id="58b14-120">In **QSTodoService.m** (Objective-C) or **ToDoTableViewController.swift** (Swift), notice that hello type of hello member **syncTable** is **MSSyncTable**.</span></span> <span data-ttu-id="58b14-121">Offline synchronizace používá toto rozhraní tabulky synchronizace místo **MSTable**.</span><span class="sxs-lookup"><span data-stu-id="58b14-121">Offline sync uses this sync table interface instead of **MSTable**.</span></span> <span data-ttu-id="58b14-122">Pokud se používá tabulku synchronizace, všechny operace přejděte toohello místního úložiště a jsou synchronizovány pouze s hello vzdálené back-end s explicitní nabízené a operací pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="58b14-122">When a sync table is used, all operations go toohello local store and are synchronized only with hello remote back end with explicit push and pull operations.</span></span>

 <span data-ttu-id="58b14-123">tooget referenční tooa synchronizace tabulku, použijte hello **syncTableWithName** metodu `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="58b14-123">tooget a reference tooa sync table, use hello **syncTableWithName** method on `MSClient`.</span></span> <span data-ttu-id="58b14-124">funkci offline synchronizace tooremove, použijte **tableWithName** místo.</span><span class="sxs-lookup"><span data-stu-id="58b14-124">tooremove offline sync functionality, use **tableWithName** instead.</span></span>

<span data-ttu-id="58b14-125">Před provedením jakékoli operace s tabulkou, musí být inicializován hello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="58b14-125">Before any table operations can be performed, hello local store must be initialized.</span></span> <span data-ttu-id="58b14-126">Tady je relevantní kód hello:</span><span class="sxs-lookup"><span data-stu-id="58b14-126">Here is hello relevant code:</span></span>

* <span data-ttu-id="58b14-127">**Jazyka Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="58b14-127">**Objective-C**.</span></span> <span data-ttu-id="58b14-128">V hello **QSTodoService.init** metoda:</span><span class="sxs-lookup"><span data-stu-id="58b14-128">In hello **QSTodoService.init** method:</span></span>

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* <span data-ttu-id="58b14-129">**Kód SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="58b14-129">**Swift**.</span></span> <span data-ttu-id="58b14-130">V hello **ToDoTableViewController.viewDidLoad** metoda:</span><span class="sxs-lookup"><span data-stu-id="58b14-130">In hello **ToDoTableViewController.viewDidLoad** method:</span></span>

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of hello Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   <span data-ttu-id="58b14-131">Tato metoda vytvoří místní úložiště pomocí hello `MSCoreDataStore` poskytuje rozhraní, což hello Mobile Apps SDK.</span><span class="sxs-lookup"><span data-stu-id="58b14-131">This method creates a local store by using hello `MSCoreDataStore` interface, which hello Mobile Apps SDK provides.</span></span> <span data-ttu-id="58b14-132">Alternativně můžete zadat různé ukládají v místním úložišti implementací hello `MSSyncContextDataSource` protokolu.</span><span class="sxs-lookup"><span data-stu-id="58b14-132">Alternatively, you can provide a different local store by implementing hello `MSSyncContextDataSource` protocol.</span></span> <span data-ttu-id="58b14-133">Navíc hello první parametr **MSSyncContext** je použité toospecify obslužnou rutinu konflikt.</span><span class="sxs-lookup"><span data-stu-id="58b14-133">Also, hello first parameter of **MSSyncContext** is used toospecify a conflict handler.</span></span> <span data-ttu-id="58b14-134">Protože jsme uplynulo `nil`, se nám získat hello výchozí konflikt rutiny, který selže ve všech konflikt.</span><span class="sxs-lookup"><span data-stu-id="58b14-134">Because we have passed `nil`, we get hello default conflict handler, which fails on any conflict.</span></span>

<span data-ttu-id="58b14-135">Nyní Pojďme hello skutečné synchronizace operaci provést a získat data z hello vzdálené back-end:</span><span class="sxs-lookup"><span data-stu-id="58b14-135">Now, let's perform hello actual sync operation, and get data from hello remote back end:</span></span>

* <span data-ttu-id="58b14-136">**Jazyka Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="58b14-136">**Objective-C**.</span></span> <span data-ttu-id="58b14-137">`syncData`nejprve nabízených oznámení nové změny a pak zavolá **pullData** tooget data ze vzdáleného back endu hello.</span><span class="sxs-lookup"><span data-stu-id="58b14-137">`syncData` first pushes new changes and then calls **pullData** tooget data from hello remote back end.</span></span> <span data-ttu-id="58b14-138">Pak hello **pullData** metoda získá nová data, která odpovídá dotazu:</span><span class="sxs-lookup"><span data-stu-id="58b14-138">In turn, hello **pullData** method gets new data that matches a query:</span></span>

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in hello sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from hello remote server into hello local table.
       // We're pulling all items and filtering in hello view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets hello caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* <span data-ttu-id="58b14-139">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="58b14-139">**Swift**:</span></span>
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc via hello MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep hello server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation tooitem \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting tooserver's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

<span data-ttu-id="58b14-140">Ve verzi hello jazyka Objective-C v `syncData`, nejprve říkáme **pushWithCompletion** v kontextu synchronizace hello.</span><span class="sxs-lookup"><span data-stu-id="58b14-140">In hello Objective-C version, in `syncData`, we first call **pushWithCompletion** on hello sync context.</span></span> <span data-ttu-id="58b14-141">Tato metoda je členem skupiny `MSSyncContext` (a ne hello synchronizace samotné tabulky) protože vynutí změny mezi všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="58b14-141">This method is a member of `MSSyncContext` (and not hello sync table itself) because it pushes changes across all tables.</span></span> <span data-ttu-id="58b14-142">Pouze záznamy, které byly upraveny nějakým způsobem místně (prostřednictvím operace vytvoření) jsou odesílány toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="58b14-142">Only records that have been modified in some way locally (through CUD operations) are sent toohello server.</span></span> <span data-ttu-id="58b14-143">Potom hello pomocná **pullData** je volána, který volá **MSSyncTable.pullWithQuery** tooretrieve Vzdálená data a ukládá je v místní databázi hello.</span><span class="sxs-lookup"><span data-stu-id="58b14-143">Then hello helper **pullData** is called, which calls **MSSyncTable.pullWithQuery** tooretrieve remote data and store it in hello local database.</span></span>

<span data-ttu-id="58b14-144">V hello Swift verze, protože operace push hello není nezbytně nutné, není žádná volání příliš**pushWithCompletion**.</span><span class="sxs-lookup"><span data-stu-id="58b14-144">In hello Swift version, because hello push operation was not strictly necessary, there is no call too**pushWithCompletion**.</span></span> <span data-ttu-id="58b14-145">Pokud jsou všechny změny čekající na vyřízení ve hello kontext synchronizace pro hello tabulku, která provádí operace push, pull vždy problémy push nejdřív.</span><span class="sxs-lookup"><span data-stu-id="58b14-145">If there are any changes pending in hello sync context for hello table that is doing a push operation, pull always issues a push first.</span></span> <span data-ttu-id="58b14-146">Pokud máte více než jedné tabulky synchronizace, je však nejlepší tooexplicitly volání nabízené tooensure že všechno, co je konzistentní napříč související tabulky.</span><span class="sxs-lookup"><span data-stu-id="58b14-146">However, if you have more than one sync table, it is best tooexplicitly call push tooensure that everything is consistent across related tables.</span></span>

<span data-ttu-id="58b14-147">V hello jazyka Objective-C a Swift verze, můžete použít hello **pullWithQuery** toospecify metoda dotazu toofilter hello záznamy, které chcete tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="58b14-147">In both hello Objective-C and Swift versions, you can use hello **pullWithQuery** method toospecify a query toofilter hello records you want tooretrieve.</span></span> <span data-ttu-id="58b14-148">V tomto příkladu hello dotaz načte všechny záznamy v hello vzdálené `TodoItem` tabulky.</span><span class="sxs-lookup"><span data-stu-id="58b14-148">In this example, hello query retrieves all records in hello remote `TodoItem` table.</span></span>

<span data-ttu-id="58b14-149">druhý parametr Hello **pullWithQuery** je ID dotazu, který se používá pro *přírůstkové synchronizace*. Přírůstkové synchronizace vyhledá všechny záznamy, která byla změněna od poslední synchronizace hello pomocí záznamu hello `UpdatedAt` časové razítko (nazývá `updatedAt` v hello místním úložišti.) hello ID dotazu by měla být popisný řetězec, který je jedinečný pro každý logický dotaz v vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="58b14-149">hello second parameter of **pullWithQuery** is a query ID that is used for *incremental sync*. Incremental sync retrieves only records that were modified since hello last sync, using hello record's `UpdatedAt` time stamp (called `updatedAt` in hello local store.) hello query ID should be a descriptive string that is unique for each logical query in your app.</span></span> <span data-ttu-id="58b14-150">tooopt mimo přírůstkové synchronizace předat `nil` jako hello ID dotazu.</span><span class="sxs-lookup"><span data-stu-id="58b14-150">tooopt out of incremental sync, pass `nil` as hello query ID.</span></span> <span data-ttu-id="58b14-151">Tento přístup může být potenciálně neefektivní, protože ho načte všechny záznamy na každou vyžádanou operaci.</span><span class="sxs-lookup"><span data-stu-id="58b14-151">This approach can be potentially inefficient, because it retrieves all records on each pull operation.</span></span>

<span data-ttu-id="58b14-152">aplikace Hello jazyka Objective-C synchronizuje se při úpravě nebo přidání dat, když uživatel provede gesto hello aktualizace a při spuštění.</span><span class="sxs-lookup"><span data-stu-id="58b14-152">hello Objective-C app syncs when you modify or add data, when a user performs hello refresh gesture, and on launch.</span></span>

<span data-ttu-id="58b14-153">Swift aplikace Hello synchronizuje hello uživatel provede gesto hello aktualizace a při spuštění.</span><span class="sxs-lookup"><span data-stu-id="58b14-153">hello Swift app syncs when hello user performs hello refresh gesture and on launch.</span></span>

<span data-ttu-id="58b14-154">Protože hello aplikace synchronizacemi vždy, když je dat změněna (Objective-C) nebo při každém spuštění aplikace hello (Objective-C a Swift), aplikace hello předpokládá, že tento uživatel hello je online.</span><span class="sxs-lookup"><span data-stu-id="58b14-154">Because hello app syncs whenever data is modified (Objective-C) or whenever hello app starts (Objective-C and Swift), hello app assumes that hello user is online.</span></span> <span data-ttu-id="58b14-155">V další části bude aktualizace hello aplikace tak, aby uživatelé mohou upravovat, i když se nachází v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="58b14-155">In a later section, you will update hello app so that users can edit even when they are offline.</span></span>

## <span data-ttu-id="58b14-156"><a name="review-core-data"></a>Zkontrolujte hello základní datový model</span><span class="sxs-lookup"><span data-stu-id="58b14-156"><a name="review-core-data"></a>Review hello Core Data model</span></span>
<span data-ttu-id="58b14-157">Pokud použijete hello základní Data offline úložiště, je nutné zadat konkrétní tabulky a pole v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="58b14-157">When you use hello Core Data offline store, you must define particular tables and fields in your data model.</span></span> <span data-ttu-id="58b14-158">Ukázková aplikace Hello již obsahuje datový model se hello správném formátu.</span><span class="sxs-lookup"><span data-stu-id="58b14-158">hello sample app already includes a data model with hello right format.</span></span> <span data-ttu-id="58b14-159">V této části jsme provede tyto tabulky tooshow jejich použití.</span><span class="sxs-lookup"><span data-stu-id="58b14-159">In this section, we walk through these tables tooshow how they are used.</span></span>

<span data-ttu-id="58b14-160">Otevřete **QSDataModel.xcdatamodeld**.</span><span class="sxs-lookup"><span data-stu-id="58b14-160">Open **QSDataModel.xcdatamodeld**.</span></span> <span data-ttu-id="58b14-161">Čtyři tabulky jsou definované--tři, které používá hello SDK a ten, který se používá pro hello úkolů sami položky:</span><span class="sxs-lookup"><span data-stu-id="58b14-161">Four tables are defined--three that are used by hello SDK and one that's used for hello to-do items themselves:</span></span>
  * <span data-ttu-id="58b14-162">MS_TableOperations: Sleduje hello položky, které je třeba toobe synchronizovány se serverem hello.</span><span class="sxs-lookup"><span data-stu-id="58b14-162">MS_TableOperations: Tracks hello items that need toobe synchronized with hello server.</span></span>
  * <span data-ttu-id="58b14-163">MS_TableOperationErrors: Sleduje všechny chyby, které dojít během offline synchronizace.</span><span class="sxs-lookup"><span data-stu-id="58b14-163">MS_TableOperationErrors: Tracks any errors that happen during offline synchronization.</span></span>
  * <span data-ttu-id="58b14-164">MS_TableConfig: Sleduje hello poslední čas poslední aktualizace pro hello poslední synchronizace pro všechny operace vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="58b14-164">MS_TableConfig: Tracks hello last updated time for hello last sync operation for all pull operations.</span></span>
  * <span data-ttu-id="58b14-165">TodoItem: Ukládá položky úkolů hello.</span><span class="sxs-lookup"><span data-stu-id="58b14-165">TodoItem: Stores hello to-do items.</span></span> <span data-ttu-id="58b14-166">Hello systémové sloupce **createdAt**, **updatedAt**, a **verze** jsou vlastnosti volitelné systému.</span><span class="sxs-lookup"><span data-stu-id="58b14-166">hello system columns **createdAt**, **updatedAt**, and **version** are optional system properties.</span></span>

> [!NOTE]
> <span data-ttu-id="58b14-167">Hello Mobile Apps SDK rezervuje názvy sloupců, které začínají "**``**".</span><span class="sxs-lookup"><span data-stu-id="58b14-167">hello Mobile Apps SDK reserves column names that begin with "**``**".</span></span> <span data-ttu-id="58b14-168">Nepoužívejte tuto předponu s jakoukoli jinou hodnotu než systémové sloupce.</span><span class="sxs-lookup"><span data-stu-id="58b14-168">Do not use this prefix with anything other than system columns.</span></span> <span data-ttu-id="58b14-169">Názvy sloupců jsou, jinak hodnota upravovat, když používáte vzdálené back end hello.</span><span class="sxs-lookup"><span data-stu-id="58b14-169">Otherwise, your column names are modified when you use hello remote back end.</span></span>
>
>

<span data-ttu-id="58b14-170">Při použití funkce offline synchronizace hello definujte hello tři systémové tabulky a hello dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="58b14-170">When you use hello offline sync feature, define hello three system tables and hello data table.</span></span>

### <a name="system-tables"></a><span data-ttu-id="58b14-171">Systémové tabulky</span><span class="sxs-lookup"><span data-stu-id="58b14-171">System tables</span></span>

<span data-ttu-id="58b14-172">**MS_TableOperations**</span><span class="sxs-lookup"><span data-stu-id="58b14-172">**MS_TableOperations**</span></span>  

![Atributy MS_TableOperations tabulky][defining-core-data-tableoperations-entity]

| <span data-ttu-id="58b14-174">Atribut</span><span class="sxs-lookup"><span data-stu-id="58b14-174">Attribute</span></span> | <span data-ttu-id="58b14-175">Typ</span><span class="sxs-lookup"><span data-stu-id="58b14-175">Type</span></span> |
| --- | --- |
| <span data-ttu-id="58b14-176">id</span><span class="sxs-lookup"><span data-stu-id="58b14-176">id</span></span> | <span data-ttu-id="58b14-177">Celé číslo 64</span><span class="sxs-lookup"><span data-stu-id="58b14-177">Integer 64</span></span> |
| <span data-ttu-id="58b14-178">itemId</span><span class="sxs-lookup"><span data-stu-id="58b14-178">itemId</span></span> | <span data-ttu-id="58b14-179">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-179">String</span></span> |
| <span data-ttu-id="58b14-180">properties</span><span class="sxs-lookup"><span data-stu-id="58b14-180">properties</span></span> | <span data-ttu-id="58b14-181">Binární Data</span><span class="sxs-lookup"><span data-stu-id="58b14-181">Binary Data</span></span> |
| <span data-ttu-id="58b14-182">Tabulka</span><span class="sxs-lookup"><span data-stu-id="58b14-182">table</span></span> | <span data-ttu-id="58b14-183">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-183">String</span></span> |
| <span data-ttu-id="58b14-184">tableKind</span><span class="sxs-lookup"><span data-stu-id="58b14-184">tableKind</span></span> | <span data-ttu-id="58b14-185">Celé číslo 16</span><span class="sxs-lookup"><span data-stu-id="58b14-185">Integer 16</span></span> |


<span data-ttu-id="58b14-186">**MS_TableOperationErrors**</span><span class="sxs-lookup"><span data-stu-id="58b14-186">**MS_TableOperationErrors**</span></span>

 ![Atributy MS_TableOperationErrors tabulky][defining-core-data-tableoperationerrors-entity]

| <span data-ttu-id="58b14-188">Atribut</span><span class="sxs-lookup"><span data-stu-id="58b14-188">Attribute</span></span> | <span data-ttu-id="58b14-189">Typ</span><span class="sxs-lookup"><span data-stu-id="58b14-189">Type</span></span> |
| --- | --- |
| <span data-ttu-id="58b14-190">id</span><span class="sxs-lookup"><span data-stu-id="58b14-190">id</span></span> |<span data-ttu-id="58b14-191">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-191">String</span></span> |
| <span data-ttu-id="58b14-192">operationId</span><span class="sxs-lookup"><span data-stu-id="58b14-192">operationId</span></span> |<span data-ttu-id="58b14-193">Celé číslo 64</span><span class="sxs-lookup"><span data-stu-id="58b14-193">Integer 64</span></span> |
| <span data-ttu-id="58b14-194">properties</span><span class="sxs-lookup"><span data-stu-id="58b14-194">properties</span></span> |<span data-ttu-id="58b14-195">Binární Data</span><span class="sxs-lookup"><span data-stu-id="58b14-195">Binary Data</span></span> |
| <span data-ttu-id="58b14-196">tableKind</span><span class="sxs-lookup"><span data-stu-id="58b14-196">tableKind</span></span> |<span data-ttu-id="58b14-197">Celé číslo 16</span><span class="sxs-lookup"><span data-stu-id="58b14-197">Integer 16</span></span> |

 <span data-ttu-id="58b14-198">**MS_TableConfig**</span><span class="sxs-lookup"><span data-stu-id="58b14-198">**MS_TableConfig**</span></span>

 ![][defining-core-data-tableconfig-entity]

| <span data-ttu-id="58b14-199">Atribut</span><span class="sxs-lookup"><span data-stu-id="58b14-199">Attribute</span></span> | <span data-ttu-id="58b14-200">Typ</span><span class="sxs-lookup"><span data-stu-id="58b14-200">Type</span></span> |
| --- | --- |
| <span data-ttu-id="58b14-201">id</span><span class="sxs-lookup"><span data-stu-id="58b14-201">id</span></span> |<span data-ttu-id="58b14-202">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-202">String</span></span> |
| <span data-ttu-id="58b14-203">key</span><span class="sxs-lookup"><span data-stu-id="58b14-203">key</span></span> |<span data-ttu-id="58b14-204">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-204">String</span></span> |
| <span data-ttu-id="58b14-205">Typ_klíče.</span><span class="sxs-lookup"><span data-stu-id="58b14-205">keyType</span></span> |<span data-ttu-id="58b14-206">Celé číslo 64</span><span class="sxs-lookup"><span data-stu-id="58b14-206">Integer 64</span></span> |
| <span data-ttu-id="58b14-207">Tabulka</span><span class="sxs-lookup"><span data-stu-id="58b14-207">table</span></span> |<span data-ttu-id="58b14-208">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-208">String</span></span> |
| <span data-ttu-id="58b14-209">hodnota</span><span class="sxs-lookup"><span data-stu-id="58b14-209">value</span></span> |<span data-ttu-id="58b14-210">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-210">String</span></span> |

### <a name="data-table"></a><span data-ttu-id="58b14-211">Datová tabulka</span><span class="sxs-lookup"><span data-stu-id="58b14-211">Data table</span></span>

<span data-ttu-id="58b14-212">**TodoItem**</span><span class="sxs-lookup"><span data-stu-id="58b14-212">**TodoItem**</span></span>

| <span data-ttu-id="58b14-213">Atribut</span><span class="sxs-lookup"><span data-stu-id="58b14-213">Attribute</span></span> | <span data-ttu-id="58b14-214">Typ</span><span class="sxs-lookup"><span data-stu-id="58b14-214">Type</span></span> | <span data-ttu-id="58b14-215">Poznámka</span><span class="sxs-lookup"><span data-stu-id="58b14-215">Note</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58b14-216">id</span><span class="sxs-lookup"><span data-stu-id="58b14-216">id</span></span> | <span data-ttu-id="58b14-217">Řetězec, označen jako požadovaný</span><span class="sxs-lookup"><span data-stu-id="58b14-217">String, marked required</span></span> |<span data-ttu-id="58b14-218">primární klíč v vzdáleného úložiště</span><span class="sxs-lookup"><span data-stu-id="58b14-218">Primary key in remote store</span></span> |
| <span data-ttu-id="58b14-219">Dokončení</span><span class="sxs-lookup"><span data-stu-id="58b14-219">complete</span></span> | <span data-ttu-id="58b14-220">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="58b14-220">Boolean</span></span> | <span data-ttu-id="58b14-221">Pole položky seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="58b14-221">To-do item field</span></span> |
| <span data-ttu-id="58b14-222">Text</span><span class="sxs-lookup"><span data-stu-id="58b14-222">text</span></span> |<span data-ttu-id="58b14-223">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-223">String</span></span> |<span data-ttu-id="58b14-224">Pole položky seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="58b14-224">To-do item field</span></span> |
| <span data-ttu-id="58b14-225">CreatedAt</span><span class="sxs-lookup"><span data-stu-id="58b14-225">createdAt</span></span> | <span data-ttu-id="58b14-226">Datum</span><span class="sxs-lookup"><span data-stu-id="58b14-226">Date</span></span> | <span data-ttu-id="58b14-227">(volitelné) Mapuje příliš**createdAt** vlastnost systému</span><span class="sxs-lookup"><span data-stu-id="58b14-227">(optional) Maps too**createdAt** system property</span></span> |
| <span data-ttu-id="58b14-228">updatedAt</span><span class="sxs-lookup"><span data-stu-id="58b14-228">updatedAt</span></span> | <span data-ttu-id="58b14-229">Datum</span><span class="sxs-lookup"><span data-stu-id="58b14-229">Date</span></span> | <span data-ttu-id="58b14-230">(volitelné) Mapuje příliš**updatedAt** vlastnost systému</span><span class="sxs-lookup"><span data-stu-id="58b14-230">(optional) Maps too**updatedAt** system property</span></span> |
| <span data-ttu-id="58b14-231">Verze</span><span class="sxs-lookup"><span data-stu-id="58b14-231">version</span></span> | <span data-ttu-id="58b14-232">Řetězec</span><span class="sxs-lookup"><span data-stu-id="58b14-232">String</span></span> | <span data-ttu-id="58b14-233">(volitelné) Použít toodetect konfliktů, tooversion mapy</span><span class="sxs-lookup"><span data-stu-id="58b14-233">(optional) Used toodetect conflicts, maps tooversion</span></span> |

## <span data-ttu-id="58b14-234"><a name="setup-sync"></a>Změnit chování synchronizace hello aplikace hello</span><span class="sxs-lookup"><span data-stu-id="58b14-234"><a name="setup-sync"></a>Change hello sync behavior of hello app</span></span>
<span data-ttu-id="58b14-235">V této části upravíte hello aplikace tak, aby ho na spuštění aplikace nebo když vložení a aktualizace položky není synchronizovaná.</span><span class="sxs-lookup"><span data-stu-id="58b14-235">In this section, you modify hello app so that it does not sync on app start or when you insert and update items.</span></span> <span data-ttu-id="58b14-236">Synchronizuje se jenom v případě, že se provádí tlačítko gesto aktualizovat hello.</span><span class="sxs-lookup"><span data-stu-id="58b14-236">It syncs only when hello refresh gesture button is performed.</span></span>

<span data-ttu-id="58b14-237">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="58b14-237">**Objective-C**:</span></span>

1. <span data-ttu-id="58b14-238">V **QSTodoListViewController.m**, změňte hello **viewDidLoad** tooremove metoda hello volání příliš`[self refresh]` na konci hello hello metody.</span><span class="sxs-lookup"><span data-stu-id="58b14-238">In **QSTodoListViewController.m**, change hello **viewDidLoad** method tooremove hello call too`[self refresh]` at hello end of hello method.</span></span> <span data-ttu-id="58b14-239">Teď hello dat není synchronizovaný s hello serveru při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="58b14-239">Now hello data is not synced with hello server on app start.</span></span> <span data-ttu-id="58b14-240">Místo toho je synchronizovaný s hello obsah hello místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="58b14-240">Instead, it's synced with hello contents of hello local store.</span></span>
2. <span data-ttu-id="58b14-241">V **QSTodoService.m**, upravte definici hello `addItem` tak, aby se nebude synchronizovat po vložení hello položky.</span><span class="sxs-lookup"><span data-stu-id="58b14-241">In **QSTodoService.m**, modify hello definition of `addItem` so that it doesn't sync after hello item is inserted.</span></span> <span data-ttu-id="58b14-242">Odebrat hello `self syncData` blokovat a nahraďte ji metodou hello následující:</span><span class="sxs-lookup"><span data-stu-id="58b14-242">Remove hello `self syncData` block and replace it with hello following:</span></span>

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. <span data-ttu-id="58b14-243">Upravit definici hello `completeItem` jak je uvedeno nahoře.</span><span class="sxs-lookup"><span data-stu-id="58b14-243">Modify hello definition of `completeItem` as mentioned previously.</span></span> <span data-ttu-id="58b14-244">Odeberte hello blok pro `self syncData` a nahraďte ji metodou hello následující:</span><span class="sxs-lookup"><span data-stu-id="58b14-244">Remove hello block for `self syncData` and replace it with hello following:</span></span>
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

<span data-ttu-id="58b14-245">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="58b14-245">**Swift**:</span></span>

<span data-ttu-id="58b14-246">V `viewDidLoad`v **ToDoTableViewController.swift**, komentář hello dva řádky zobrazeny zde, toostop synchronizuje se při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="58b14-246">In `viewDidLoad`, in **ToDoTableViewController.swift**, comment out hello two lines shown here, toostop syncing on app start.</span></span> <span data-ttu-id="58b14-247">V době psaní tohoto textu hello aplikace Swift Todo hello hello služby při aktualizaci někdo přidá nebo dokončení položku.</span><span class="sxs-lookup"><span data-stu-id="58b14-247">At hello time of this writing, hello Swift Todo app does not update hello service when someone adds or completes an item.</span></span> <span data-ttu-id="58b14-248">Aktualizuje hello služby pouze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="58b14-248">It updates hello service only on app start.</span></span>

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <span data-ttu-id="58b14-249"><a name="test-app"></a>Testování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="58b14-249"><a name="test-app"></a>Test hello app</span></span>
<span data-ttu-id="58b14-250">V této části připojit tooan neplatná adresa URL toosimulate offline scénář.</span><span class="sxs-lookup"><span data-stu-id="58b14-250">In this section, you connect tooan invalid URL toosimulate an offline scenario.</span></span> <span data-ttu-id="58b14-251">Když přidáte datových položek, jste uchovávat v hello místní základní datové úložiště, ale není synchronizovaná s back-end hello mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="58b14-251">When you add data items, they're held in hello local Core Data store, but they're not synced with hello mobile-app back end.</span></span>

1. <span data-ttu-id="58b14-252">Změnit adresu URL mobilní aplikace hello v **QSTodoService.m** tooan neplatná adresa URL a znovu spusťte hello aplikaci:</span><span class="sxs-lookup"><span data-stu-id="58b14-252">Change hello mobile-app URL in **QSTodoService.m** tooan invalid URL, and run hello app again:</span></span>

   <span data-ttu-id="58b14-253">**Jazyka Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="58b14-253">**Objective-C**.</span></span> <span data-ttu-id="58b14-254">V QSTodoService.m:</span><span class="sxs-lookup"><span data-stu-id="58b14-254">In QSTodoService.m:</span></span>
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   <span data-ttu-id="58b14-255">**Kód SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="58b14-255">**Swift**.</span></span> <span data-ttu-id="58b14-256">V ToDoTableViewController.swift:</span><span class="sxs-lookup"><span data-stu-id="58b14-256">In ToDoTableViewController.swift:</span></span>
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. <span data-ttu-id="58b14-257">Přidejte některé položky seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="58b14-257">Add some to-do items.</span></span> <span data-ttu-id="58b14-258">Ukončete simulátoru hello (nebo vynuceně zavřít hello aplikace) a pak ji znovu spusťte.</span><span class="sxs-lookup"><span data-stu-id="58b14-258">Quit hello simulator (or forcibly close hello app), and then restart it.</span></span> <span data-ttu-id="58b14-259">Ověřte, že vaše změny se zachovají.</span><span class="sxs-lookup"><span data-stu-id="58b14-259">Verify that your changes persist.</span></span>

3. <span data-ttu-id="58b14-260">Zobrazit obsah hello hello vzdálené **TodoItem** tabulky:</span><span class="sxs-lookup"><span data-stu-id="58b14-260">View hello contents of hello remote **TodoItem** table:</span></span>
   * <span data-ttu-id="58b14-261">Pro Node.js back-end, přejděte toohello [portál Azure](https://portal.azure.com/) a ve vaší mobilní aplikace back-end, klikněte na tlačítko **snadno tabulky** > **TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="58b14-261">For a Node.js back end, go toohello [Azure portal](https://portal.azure.com/) and, in your mobile-app back end, click **Easy Tables** > **TodoItem**.</span></span>  
   * <span data-ttu-id="58b14-262">Pro .NET zpět ukončení, použijte nástroj SQL, jako je například SQL Server Management Studio, nebo klienta REST, například aplikaci Fiddler nebo Postman.</span><span class="sxs-lookup"><span data-stu-id="58b14-262">For a .NET back end, use either a SQL tool, such as SQL Server Management Studio, or a REST client, such as Fiddler or Postman.</span></span>  

4. <span data-ttu-id="58b14-263">Ověřte, že nové položky hello *není* byly synchronizované s hello server.</span><span class="sxs-lookup"><span data-stu-id="58b14-263">Verify that hello new items have *not* been synced with hello server.</span></span>

5. <span data-ttu-id="58b14-264">Změna hello adresa URL back toohello opravte jednu v **QSTodoService.m**a znovu spusťte hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="58b14-264">Change hello URL back toohello correct one in **QSTodoService.m**, and rerun hello app.</span></span>

6. <span data-ttu-id="58b14-265">Proveďte aktualizaci gesto hello přidáváním dolů hello seznam položek.</span><span class="sxs-lookup"><span data-stu-id="58b14-265">Perform hello refresh gesture by pulling down hello list of items.</span></span>  
<span data-ttu-id="58b14-266">Zobrazí se průběh číselník.</span><span class="sxs-lookup"><span data-stu-id="58b14-266">A progress spinner is displayed.</span></span>

7. <span data-ttu-id="58b14-267">Zobrazení hello **TodoItem** data znovu.</span><span class="sxs-lookup"><span data-stu-id="58b14-267">View hello **TodoItem** data again.</span></span> <span data-ttu-id="58b14-268">nyní mají být zobrazeny Hello úkolů nové a změněné položky.</span><span class="sxs-lookup"><span data-stu-id="58b14-268">hello new and changed to-do items should now be displayed.</span></span>

## <a name="summary"></a><span data-ttu-id="58b14-269">Souhrn</span><span class="sxs-lookup"><span data-stu-id="58b14-269">Summary</span></span>
<span data-ttu-id="58b14-270">Funkce offline synchronizace hello toosupport, použili jsme hello `MSSyncTable` rozhraní a inicializovat `MSClient.syncContext` s místním úložištěm.</span><span class="sxs-lookup"><span data-stu-id="58b14-270">toosupport hello offline sync feature, we used hello `MSSyncTable` interface and initialized `MSClient.syncContext` with a local store.</span></span> <span data-ttu-id="58b14-271">V takovém případě hello místní úložiště se databáze založené na datech jádra.</span><span class="sxs-lookup"><span data-stu-id="58b14-271">In this case, hello local store was a Core Data-based database.</span></span>

<span data-ttu-id="58b14-272">Pokud používáte místní úložiště dat jádra, je nutné zadat několik tabulek s hello [opravte vlastnosti systému](#review-core-data).</span><span class="sxs-lookup"><span data-stu-id="58b14-272">When you use a Core Data local store, you must define several tables with hello [correct system properties](#review-core-data).</span></span>

<span data-ttu-id="58b14-273">Normální Hello vytvářet, číst, aktualizovat a odstranit operace pro mobilní aplikace pracují, jako kdyby aplikace hello je stále připojená, ale všechny operace hello nastat proti hello místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="58b14-273">hello normal create, read, update, and delete (CRUD) operations for mobile apps work as if hello app is still connected, but all hello operations occur against hello local store.</span></span>

<span data-ttu-id="58b14-274">Když jsme místního úložiště hello synchronizovány se serverem hello, použili jsme hello **MSSyncTable.pullWithQuery** metoda.</span><span class="sxs-lookup"><span data-stu-id="58b14-274">When we synchronized hello local store with hello server, we used hello **MSSyncTable.pullWithQuery** method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58b14-275">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="58b14-275">Additional resources</span></span>
* <span data-ttu-id="58b14-276">[Offline synchronizací dat v Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="58b14-276">[Offline Data Sync in Mobile Apps]</span></span>
* <span data-ttu-id="58b14-277">[Obálka cloudu: Offline synchronizace v Azure Mobile Services] \(je hello video o Mobile Services, ale Mobile Apps offline synchronizace funguje podobným způsobem.\)</span><span class="sxs-lookup"><span data-stu-id="58b14-277">[Cloud Cover: Offline Sync in Azure Mobile Services] \(hello video is about Mobile Services, but Mobile Apps offline sync works in a similar way.\)</span></span>

<!-- URLs. -->


[vytvořit aplikaci pro iOS]: app-service-mobile-ios-get-started.md
[Offline synchronizací dat v Mobile Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Obálka cloudu: Offline synchronizace v Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
