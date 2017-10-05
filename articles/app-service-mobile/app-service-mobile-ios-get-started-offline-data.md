---
title: "Povolit offline synchronizace u mobilních aplikací pro iOS | Microsoft Docs"
description: "Naučte se používat Azure App Service mobilní aplikace do mezipaměti a synchronizaci dat offline v aplikacích pro iOS."
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
ms.openlocfilehash: 44c0d26b2d7d28322d436d4bda319d728c31a635
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a><span data-ttu-id="ae626-103">Povolit offline synchronizace u mobilních aplikací pro iOS</span><span class="sxs-lookup"><span data-stu-id="ae626-103">Enable offline syncing with iOS mobile apps</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="ae626-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="ae626-104">Overview</span></span>
<span data-ttu-id="ae626-105">Tento kurz se zaměřuje na offline synchronizace se funkce Mobile Apps služby Azure App Service pro iOS.</span><span class="sxs-lookup"><span data-stu-id="ae626-105">This tutorial covers offline syncing with the Mobile Apps feature of Azure App Service for iOS.</span></span> <span data-ttu-id="ae626-106">S offline synchronizaci koncoví uživatelé mohou komunikovat s mobilní aplikací lze zobrazit, přidat nebo upravit data, i když mají žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="ae626-106">With offline syncing end-users can interact with a mobile app to view, add, or modify data, even when they have no network connection.</span></span> <span data-ttu-id="ae626-107">Změny se ukládají do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="ae626-107">Changes are stored in a local database.</span></span> <span data-ttu-id="ae626-108">Jakmile zařízení přejde do režimu online, budou změny synchronizovány s vzdálené back-end.</span><span class="sxs-lookup"><span data-stu-id="ae626-108">After the device is back online, the changes are synced with the remote back end.</span></span>

<span data-ttu-id="ae626-109">Pokud je vaše první zkušenosti s Mobile Apps, musí nejprve dokončit kurz [vytvořit aplikaci pro iOS].</span><span class="sxs-lookup"><span data-stu-id="ae626-109">If this is your first experience with Mobile Apps, you should first complete the tutorial [Create an iOS App].</span></span> <span data-ttu-id="ae626-110">Pokud nepoužijete stažené úvodní serverový projekt, je nutné přidat do projektu balíčky rozšíření přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="ae626-110">If you do not use the downloaded quick-start server project, you must add the data-access extension packages to your project.</span></span> <span data-ttu-id="ae626-111">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="ae626-111">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="ae626-112">Další informace o funkci offline synchronizace najdete v tématu [Offline synchronizací dat v Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="ae626-112">To learn more about the offline sync feature, see [Offline Data Sync in Mobile Apps].</span></span>

## <span data-ttu-id="ae626-113"><a name="review-sync"></a>Zkontrolujte kód synchronizace klienta</span><span class="sxs-lookup"><span data-stu-id="ae626-113"><a name="review-sync"></a>Review the client sync code</span></span>
<span data-ttu-id="ae626-114">Klientského projektu, který jste stáhli pro [vytvořit aplikaci pro iOS] kurzu již obsahuje kód, který podporuje offline synchronizace pomocí místní databáze založené na datech jádra.</span><span class="sxs-lookup"><span data-stu-id="ae626-114">The client project that you downloaded for the [Create an iOS App] tutorial already contains code that supports offline synchronization using a local Core Data-based database.</span></span> <span data-ttu-id="ae626-115">Tento oddíl shrnuje, co je již zahrnut v kurzu kódu.</span><span class="sxs-lookup"><span data-stu-id="ae626-115">This section summarizes what is already included in the tutorial code.</span></span> <span data-ttu-id="ae626-116">Koncepční přehled funkce, najdete v části [Offline synchronizací dat v Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="ae626-116">For a conceptual overview of the feature, see [Offline Data Sync in Mobile Apps].</span></span>

<span data-ttu-id="ae626-117">Pomocí funkce offline synchronizací dat z mobilní aplikace, mohou koncoví uživatelé komunikovat s místní databáze, i v případě, že síť je nedostupná.</span><span class="sxs-lookup"><span data-stu-id="ae626-117">Using the offline data-sync feature of Mobile Apps, end-users can interact with a local database even when the network is inaccessible.</span></span> <span data-ttu-id="ae626-118">V aplikaci použít tyto funkce, inicializovat kontext synchronizace `MSClient` a odkazovat na místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="ae626-118">To use these features in your app, you initialize the sync context of `MSClient` and reference a local store.</span></span> <span data-ttu-id="ae626-119">Pak referenční tabulku prostřednictvím **MSSyncTable** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ae626-119">Then you reference your table through the **MSSyncTable** interface.</span></span>

<span data-ttu-id="ae626-120">V **QSTodoService.m** (Objective-C) nebo **ToDoTableViewController.swift** (Swift), Všimněte si, že typ člena **syncTable** je **MSSyncTable** .</span><span class="sxs-lookup"><span data-stu-id="ae626-120">In **QSTodoService.m** (Objective-C) or **ToDoTableViewController.swift** (Swift), notice that the type of the member **syncTable** is **MSSyncTable**.</span></span> <span data-ttu-id="ae626-121">Offline synchronizace používá toto rozhraní tabulky synchronizace místo **MSTable**.</span><span class="sxs-lookup"><span data-stu-id="ae626-121">Offline sync uses this sync table interface instead of **MSTable**.</span></span> <span data-ttu-id="ae626-122">Při synchronizaci tabulky se používá, všechny operace přejděte do místního úložiště a jsou synchronizovány pouze s vzdálené back-end s explicitní nabízení a vyžadování operace.</span><span class="sxs-lookup"><span data-stu-id="ae626-122">When a sync table is used, all operations go to the local store and are synchronized only with the remote back end with explicit push and pull operations.</span></span>

 <span data-ttu-id="ae626-123">Pokud chcete získat odkaz na tabulku synchronizace, použijte **syncTableWithName** metodu `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="ae626-123">To get a reference to a sync table, use the **syncTableWithName** method on `MSClient`.</span></span> <span data-ttu-id="ae626-124">Odebrat funkci offline synchronizace, použijte **tableWithName** místo.</span><span class="sxs-lookup"><span data-stu-id="ae626-124">To remove offline sync functionality, use **tableWithName** instead.</span></span>

<span data-ttu-id="ae626-125">Před provedením jakékoli operace s tabulkou, musí být inicializován místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="ae626-125">Before any table operations can be performed, the local store must be initialized.</span></span> <span data-ttu-id="ae626-126">Tady je relevantní kód:</span><span class="sxs-lookup"><span data-stu-id="ae626-126">Here is the relevant code:</span></span>

* <span data-ttu-id="ae626-127">**Jazyka Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="ae626-127">**Objective-C**.</span></span> <span data-ttu-id="ae626-128">V **QSTodoService.init** metoda:</span><span class="sxs-lookup"><span data-stu-id="ae626-128">In the **QSTodoService.init** method:</span></span>

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* <span data-ttu-id="ae626-129">**Kód SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="ae626-129">**Swift**.</span></span> <span data-ttu-id="ae626-130">V **ToDoTableViewController.viewDidLoad** metoda:</span><span class="sxs-lookup"><span data-stu-id="ae626-130">In the **ToDoTableViewController.viewDidLoad** method:</span></span>

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   <span data-ttu-id="ae626-131">Tato metoda vytvoří místní úložiště pomocí `MSCoreDataStore` rozhraní, která poskytuje sada SDK mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae626-131">This method creates a local store by using the `MSCoreDataStore` interface, which the Mobile Apps SDK provides.</span></span> <span data-ttu-id="ae626-132">Alternativně můžete zadat různé ukládají v místním úložišti implementací `MSSyncContextDataSource` protokolu.</span><span class="sxs-lookup"><span data-stu-id="ae626-132">Alternatively, you can provide a different local store by implementing the `MSSyncContextDataSource` protocol.</span></span> <span data-ttu-id="ae626-133">Navíc první parametr **MSSyncContext** slouží k určení obslužnou rutinu konflikt.</span><span class="sxs-lookup"><span data-stu-id="ae626-133">Also, the first parameter of **MSSyncContext** is used to specify a conflict handler.</span></span> <span data-ttu-id="ae626-134">Protože jsme uplynulo `nil`, se nám získat obslužná rutina konflikt výchozí selže ve všech konflikt.</span><span class="sxs-lookup"><span data-stu-id="ae626-134">Because we have passed `nil`, we get the default conflict handler, which fails on any conflict.</span></span>

<span data-ttu-id="ae626-135">Nyní můžeme provést operaci skutečné synchronizace a získat data z vzdálené back-end:</span><span class="sxs-lookup"><span data-stu-id="ae626-135">Now, let's perform the actual sync operation, and get data from the remote back end:</span></span>

* <span data-ttu-id="ae626-136">**Jazyka Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="ae626-136">**Objective-C**.</span></span> <span data-ttu-id="ae626-137">`syncData`nejprve nabízených oznámení nové změny a pak zavolá **pullData** k získání dat z vzdálené back-end.</span><span class="sxs-lookup"><span data-stu-id="ae626-137">`syncData` first pushes new changes and then calls **pullData** to get data from the remote back end.</span></span> <span data-ttu-id="ae626-138">Pak **pullData** metoda získá nová data, která odpovídá dotazu:</span><span class="sxs-lookup"><span data-stu-id="ae626-138">In turn, the **pullData** method gets new data that matches a query:</span></span>

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in the sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from the remote server into the local table.
       // We're pulling all items and filtering in the view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets the caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* <span data-ttu-id="ae626-139">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="ae626-139">**Swift**:</span></span>
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc via the MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep the server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation to item \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting to server's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

<span data-ttu-id="ae626-140">Ve verzi jazyka Objective-C v `syncData`, nejprve říkáme **pushWithCompletion** v kontextu synchronizace.</span><span class="sxs-lookup"><span data-stu-id="ae626-140">In the Objective-C version, in `syncData`, we first call **pushWithCompletion** on the sync context.</span></span> <span data-ttu-id="ae626-141">Tato metoda je členem skupiny `MSSyncContext` (a ne samotné tabulky synchronizace) protože vynutí změny mezi všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="ae626-141">This method is a member of `MSSyncContext` (and not the sync table itself) because it pushes changes across all tables.</span></span> <span data-ttu-id="ae626-142">Na server se odesílají pouze záznamy, které byly upraveny nějakým způsobem místně (prostřednictvím operace vytvoření).</span><span class="sxs-lookup"><span data-stu-id="ae626-142">Only records that have been modified in some way locally (through CUD operations) are sent to the server.</span></span> <span data-ttu-id="ae626-143">Potom pomocné rutiny **pullData** je volána, který volá **MSSyncTable.pullWithQuery** načíst vzdálená data a ukládá ho do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="ae626-143">Then the helper **pullData** is called, which calls **MSSyncTable.pullWithQuery** to retrieve remote data and store it in the local database.</span></span>

<span data-ttu-id="ae626-144">V Swift verze, protože není nezbytně nutné, pomocí operace push je bez volání **pushWithCompletion**.</span><span class="sxs-lookup"><span data-stu-id="ae626-144">In the Swift version, because the push operation was not strictly necessary, there is no call to **pushWithCompletion**.</span></span> <span data-ttu-id="ae626-145">Pokud jsou všechny změny čekající na vyřízení ve kontext synchronizace pro tabulky, která provádí operace push, pull vždy problémy push nejdřív.</span><span class="sxs-lookup"><span data-stu-id="ae626-145">If there are any changes pending in the sync context for the table that is doing a push operation, pull always issues a push first.</span></span> <span data-ttu-id="ae626-146">Pokud máte více než jedné tabulky synchronizace, je však vhodné explicitně volání nabízené zajistit, že všechno, co je konzistentní napříč související tabulky.</span><span class="sxs-lookup"><span data-stu-id="ae626-146">However, if you have more than one sync table, it is best to explicitly call push to ensure that everything is consistent across related tables.</span></span>

<span data-ttu-id="ae626-147">V Objective-C i Swift verze, můžete použít **pullWithQuery** metoda k zadání dotazu pro filtrování záznamů, můžete obnovit.</span><span class="sxs-lookup"><span data-stu-id="ae626-147">In both the Objective-C and Swift versions, you can use the **pullWithQuery** method to specify a query to filter the records you want to retrieve.</span></span> <span data-ttu-id="ae626-148">V tomto příkladu dotaz načte všechny záznamy v vzdálených `TodoItem` tabulky.</span><span class="sxs-lookup"><span data-stu-id="ae626-148">In this example, the query retrieves all records in the remote `TodoItem` table.</span></span>

<span data-ttu-id="ae626-149">Druhý parametr **pullWithQuery** je ID dotazu, který se používá pro *přírůstkové synchronizace*.</span><span class="sxs-lookup"><span data-stu-id="ae626-149">The second parameter of **pullWithQuery** is a query ID that is used for *incremental sync*.</span></span> <span data-ttu-id="ae626-150">Přírůstkové synchronizace vyhledá všechny záznamy, která byla změněna od poslední synchronizace, pomocí záznamu `UpdatedAt` časové razítko (nazývá `updatedAt` v místním úložišti.) ID dotazu by měla být popisný řetězec, který je jedinečný pro každý logický dotaz ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae626-150">Incremental sync retrieves only records that were modified since the last sync, using the record's `UpdatedAt` time stamp (called `updatedAt` in the local store.) The query ID should be a descriptive string that is unique for each logical query in your app.</span></span> <span data-ttu-id="ae626-151">Pro vyjádření výslovného nesouhlasu přírůstkové synchronizace, předat `nil` jako ID dotazu.</span><span class="sxs-lookup"><span data-stu-id="ae626-151">To opt out of incremental sync, pass `nil` as the query ID.</span></span> <span data-ttu-id="ae626-152">Tento přístup může být potenciálně neefektivní, protože ho načte všechny záznamy na každou vyžádanou operaci.</span><span class="sxs-lookup"><span data-stu-id="ae626-152">This approach can be potentially inefficient, because it retrieves all records on each pull operation.</span></span>

<span data-ttu-id="ae626-153">Aplikace jazyka Objective-C synchronizuje se při úpravě nebo přidání dat, když uživatel provede gesto aktualizace a při spuštění.</span><span class="sxs-lookup"><span data-stu-id="ae626-153">The Objective-C app syncs when you modify or add data, when a user performs the refresh gesture, and on launch.</span></span>

<span data-ttu-id="ae626-154">Aplikace Swift synchronizuje, když uživatel provádí gesto aktualizace a při spuštění.</span><span class="sxs-lookup"><span data-stu-id="ae626-154">The Swift app syncs when the user performs the refresh gesture and on launch.</span></span>

<span data-ttu-id="ae626-155">Důvodu synchronizace aplikace vždy, když je dat (Objective-C) nebo při každém spuštění aplikace (Objective-C a Swift), aplikace se předpokládá, že uživatel je online.</span><span class="sxs-lookup"><span data-stu-id="ae626-155">Because the app syncs whenever data is modified (Objective-C) or whenever the app starts (Objective-C and Swift), the app assumes that the user is online.</span></span> <span data-ttu-id="ae626-156">V další části bude aktualizovat aplikaci tak, aby uživatelé mohou upravovat, i když se nachází v režimu offline.</span><span class="sxs-lookup"><span data-stu-id="ae626-156">In a later section, you will update the app so that users can edit even when they are offline.</span></span>

## <span data-ttu-id="ae626-157"><a name="review-core-data"></a>Přečtěte si základní datový model</span><span class="sxs-lookup"><span data-stu-id="ae626-157"><a name="review-core-data"></a>Review the Core Data model</span></span>
<span data-ttu-id="ae626-158">Pokud používáte offline úložiště dat jádra, je nutné zadat konkrétní tabulky a pole v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="ae626-158">When you use the Core Data offline store, you must define particular tables and fields in your data model.</span></span> <span data-ttu-id="ae626-159">Ukázková aplikace již obsahuje datový model se správném formátu.</span><span class="sxs-lookup"><span data-stu-id="ae626-159">The sample app already includes a data model with the right format.</span></span> <span data-ttu-id="ae626-160">V této části jsme provede následující tabulky, chcete-li zobrazit, jak se používají.</span><span class="sxs-lookup"><span data-stu-id="ae626-160">In this section, we walk through these tables to show how they are used.</span></span>

<span data-ttu-id="ae626-161">Otevřete **QSDataModel.xcdatamodeld**.</span><span class="sxs-lookup"><span data-stu-id="ae626-161">Open **QSDataModel.xcdatamodeld**.</span></span> <span data-ttu-id="ae626-162">Čtyři tabulky jsou definovány--tři, které používají sadu SDK a jeden, který se používá pro úkol, položky sami:</span><span class="sxs-lookup"><span data-stu-id="ae626-162">Four tables are defined--three that are used by the SDK and one that's used for the to-do items themselves:</span></span>
  * <span data-ttu-id="ae626-163">MS_TableOperations: Sleduje položky, které je nutné synchronizovat se serverem.</span><span class="sxs-lookup"><span data-stu-id="ae626-163">MS_TableOperations: Tracks the items that need to be synchronized with the server.</span></span>
  * <span data-ttu-id="ae626-164">MS_TableOperationErrors: Sleduje všechny chyby, které dojít během offline synchronizace.</span><span class="sxs-lookup"><span data-stu-id="ae626-164">MS_TableOperationErrors: Tracks any errors that happen during offline synchronization.</span></span>
  * <span data-ttu-id="ae626-165">MS_TableConfig: Sleduje poslední aktualizace čas poslední operace synchronizace pro všechny operace vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="ae626-165">MS_TableConfig: Tracks the last updated time for the last sync operation for all pull operations.</span></span>
  * <span data-ttu-id="ae626-166">TodoItem: Ukládá položky úkolů.</span><span class="sxs-lookup"><span data-stu-id="ae626-166">TodoItem: Stores the to-do items.</span></span> <span data-ttu-id="ae626-167">Systémové sloupce **createdAt**, **updatedAt**, a **verze** jsou vlastnosti volitelné systému.</span><span class="sxs-lookup"><span data-stu-id="ae626-167">The system columns **createdAt**, **updatedAt**, and **version** are optional system properties.</span></span>

> [!NOTE]
> <span data-ttu-id="ae626-168">Mobile Apps SDK rezervuje názvy sloupců, které začínají "**``**".</span><span class="sxs-lookup"><span data-stu-id="ae626-168">The Mobile Apps SDK reserves column names that begin with "**``**".</span></span> <span data-ttu-id="ae626-169">Nepoužívejte tuto předponu s jakoukoli jinou hodnotu než systémové sloupce.</span><span class="sxs-lookup"><span data-stu-id="ae626-169">Do not use this prefix with anything other than system columns.</span></span> <span data-ttu-id="ae626-170">Názvy sloupců jsou, jinak hodnota upravovat, když používáte vzdálené back-end.</span><span class="sxs-lookup"><span data-stu-id="ae626-170">Otherwise, your column names are modified when you use the remote back end.</span></span>
>
>

<span data-ttu-id="ae626-171">Pokud používáte funkci offline synchronizace, definujte tabulky dat a tři systémové tabulky.</span><span class="sxs-lookup"><span data-stu-id="ae626-171">When you use the offline sync feature, define the three system tables and the data table.</span></span>

### <a name="system-tables"></a><span data-ttu-id="ae626-172">Systémové tabulky</span><span class="sxs-lookup"><span data-stu-id="ae626-172">System tables</span></span>

<span data-ttu-id="ae626-173">**MS_TableOperations**</span><span class="sxs-lookup"><span data-stu-id="ae626-173">**MS_TableOperations**</span></span>  

![Atributy MS_TableOperations tabulky][defining-core-data-tableoperations-entity]

| <span data-ttu-id="ae626-175">Atribut</span><span class="sxs-lookup"><span data-stu-id="ae626-175">Attribute</span></span> | <span data-ttu-id="ae626-176">Typ</span><span class="sxs-lookup"><span data-stu-id="ae626-176">Type</span></span> |
| --- | --- |
| <span data-ttu-id="ae626-177">id</span><span class="sxs-lookup"><span data-stu-id="ae626-177">id</span></span> | <span data-ttu-id="ae626-178">Celé číslo 64</span><span class="sxs-lookup"><span data-stu-id="ae626-178">Integer 64</span></span> |
| <span data-ttu-id="ae626-179">itemId</span><span class="sxs-lookup"><span data-stu-id="ae626-179">itemId</span></span> | <span data-ttu-id="ae626-180">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-180">String</span></span> |
| <span data-ttu-id="ae626-181">properties</span><span class="sxs-lookup"><span data-stu-id="ae626-181">properties</span></span> | <span data-ttu-id="ae626-182">Binární Data</span><span class="sxs-lookup"><span data-stu-id="ae626-182">Binary Data</span></span> |
| <span data-ttu-id="ae626-183">Tabulka</span><span class="sxs-lookup"><span data-stu-id="ae626-183">table</span></span> | <span data-ttu-id="ae626-184">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-184">String</span></span> |
| <span data-ttu-id="ae626-185">tableKind</span><span class="sxs-lookup"><span data-stu-id="ae626-185">tableKind</span></span> | <span data-ttu-id="ae626-186">Celé číslo 16</span><span class="sxs-lookup"><span data-stu-id="ae626-186">Integer 16</span></span> |


<span data-ttu-id="ae626-187">**MS_TableOperationErrors**</span><span class="sxs-lookup"><span data-stu-id="ae626-187">**MS_TableOperationErrors**</span></span>

 ![Atributy MS_TableOperationErrors tabulky][defining-core-data-tableoperationerrors-entity]

| <span data-ttu-id="ae626-189">Atribut</span><span class="sxs-lookup"><span data-stu-id="ae626-189">Attribute</span></span> | <span data-ttu-id="ae626-190">Typ</span><span class="sxs-lookup"><span data-stu-id="ae626-190">Type</span></span> |
| --- | --- |
| <span data-ttu-id="ae626-191">id</span><span class="sxs-lookup"><span data-stu-id="ae626-191">id</span></span> |<span data-ttu-id="ae626-192">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-192">String</span></span> |
| <span data-ttu-id="ae626-193">operationId</span><span class="sxs-lookup"><span data-stu-id="ae626-193">operationId</span></span> |<span data-ttu-id="ae626-194">Celé číslo 64</span><span class="sxs-lookup"><span data-stu-id="ae626-194">Integer 64</span></span> |
| <span data-ttu-id="ae626-195">properties</span><span class="sxs-lookup"><span data-stu-id="ae626-195">properties</span></span> |<span data-ttu-id="ae626-196">Binární Data</span><span class="sxs-lookup"><span data-stu-id="ae626-196">Binary Data</span></span> |
| <span data-ttu-id="ae626-197">tableKind</span><span class="sxs-lookup"><span data-stu-id="ae626-197">tableKind</span></span> |<span data-ttu-id="ae626-198">Celé číslo 16</span><span class="sxs-lookup"><span data-stu-id="ae626-198">Integer 16</span></span> |

 <span data-ttu-id="ae626-199">**MS_TableConfig**</span><span class="sxs-lookup"><span data-stu-id="ae626-199">**MS_TableConfig**</span></span>

 ![][defining-core-data-tableconfig-entity]

| <span data-ttu-id="ae626-200">Atribut</span><span class="sxs-lookup"><span data-stu-id="ae626-200">Attribute</span></span> | <span data-ttu-id="ae626-201">Typ</span><span class="sxs-lookup"><span data-stu-id="ae626-201">Type</span></span> |
| --- | --- |
| <span data-ttu-id="ae626-202">id</span><span class="sxs-lookup"><span data-stu-id="ae626-202">id</span></span> |<span data-ttu-id="ae626-203">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-203">String</span></span> |
| <span data-ttu-id="ae626-204">key</span><span class="sxs-lookup"><span data-stu-id="ae626-204">key</span></span> |<span data-ttu-id="ae626-205">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-205">String</span></span> |
| <span data-ttu-id="ae626-206">Typ_klíče.</span><span class="sxs-lookup"><span data-stu-id="ae626-206">keyType</span></span> |<span data-ttu-id="ae626-207">Celé číslo 64</span><span class="sxs-lookup"><span data-stu-id="ae626-207">Integer 64</span></span> |
| <span data-ttu-id="ae626-208">Tabulka</span><span class="sxs-lookup"><span data-stu-id="ae626-208">table</span></span> |<span data-ttu-id="ae626-209">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-209">String</span></span> |
| <span data-ttu-id="ae626-210">hodnota</span><span class="sxs-lookup"><span data-stu-id="ae626-210">value</span></span> |<span data-ttu-id="ae626-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-211">String</span></span> |

### <a name="data-table"></a><span data-ttu-id="ae626-212">Datová tabulka</span><span class="sxs-lookup"><span data-stu-id="ae626-212">Data table</span></span>

<span data-ttu-id="ae626-213">**TodoItem**</span><span class="sxs-lookup"><span data-stu-id="ae626-213">**TodoItem**</span></span>

| <span data-ttu-id="ae626-214">Atribut</span><span class="sxs-lookup"><span data-stu-id="ae626-214">Attribute</span></span> | <span data-ttu-id="ae626-215">Typ</span><span class="sxs-lookup"><span data-stu-id="ae626-215">Type</span></span> | <span data-ttu-id="ae626-216">Poznámka</span><span class="sxs-lookup"><span data-stu-id="ae626-216">Note</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ae626-217">id</span><span class="sxs-lookup"><span data-stu-id="ae626-217">id</span></span> | <span data-ttu-id="ae626-218">Řetězec, označen jako požadovaný</span><span class="sxs-lookup"><span data-stu-id="ae626-218">String, marked required</span></span> |<span data-ttu-id="ae626-219">primární klíč v vzdáleného úložiště</span><span class="sxs-lookup"><span data-stu-id="ae626-219">Primary key in remote store</span></span> |
| <span data-ttu-id="ae626-220">Dokončení</span><span class="sxs-lookup"><span data-stu-id="ae626-220">complete</span></span> | <span data-ttu-id="ae626-221">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ae626-221">Boolean</span></span> | <span data-ttu-id="ae626-222">Pole položky seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="ae626-222">To-do item field</span></span> |
| <span data-ttu-id="ae626-223">Text</span><span class="sxs-lookup"><span data-stu-id="ae626-223">text</span></span> |<span data-ttu-id="ae626-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-224">String</span></span> |<span data-ttu-id="ae626-225">Pole položky seznamu úkolů</span><span class="sxs-lookup"><span data-stu-id="ae626-225">To-do item field</span></span> |
| <span data-ttu-id="ae626-226">CreatedAt</span><span class="sxs-lookup"><span data-stu-id="ae626-226">createdAt</span></span> | <span data-ttu-id="ae626-227">Datum</span><span class="sxs-lookup"><span data-stu-id="ae626-227">Date</span></span> | <span data-ttu-id="ae626-228">(volitelné) Se mapuje na **createdAt** vlastnost systému</span><span class="sxs-lookup"><span data-stu-id="ae626-228">(optional) Maps to **createdAt** system property</span></span> |
| <span data-ttu-id="ae626-229">updatedAt</span><span class="sxs-lookup"><span data-stu-id="ae626-229">updatedAt</span></span> | <span data-ttu-id="ae626-230">Datum</span><span class="sxs-lookup"><span data-stu-id="ae626-230">Date</span></span> | <span data-ttu-id="ae626-231">(volitelné) Se mapuje na **updatedAt** vlastnost systému</span><span class="sxs-lookup"><span data-stu-id="ae626-231">(optional) Maps to **updatedAt** system property</span></span> |
| <span data-ttu-id="ae626-232">Verze</span><span class="sxs-lookup"><span data-stu-id="ae626-232">version</span></span> | <span data-ttu-id="ae626-233">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ae626-233">String</span></span> | <span data-ttu-id="ae626-234">(volitelné) Používá ke zjišťování konfliktů, map k verzi</span><span class="sxs-lookup"><span data-stu-id="ae626-234">(optional) Used to detect conflicts, maps to version</span></span> |

## <span data-ttu-id="ae626-235"><a name="setup-sync"></a>Změna chování aplikace při synchronizaci</span><span class="sxs-lookup"><span data-stu-id="ae626-235"><a name="setup-sync"></a>Change the sync behavior of the app</span></span>
<span data-ttu-id="ae626-236">V této části upravíte aplikace tak, aby ho na spuštění aplikace nebo když vložení a aktualizace položky není synchronizovaná.</span><span class="sxs-lookup"><span data-stu-id="ae626-236">In this section, you modify the app so that it does not sync on app start or when you insert and update items.</span></span> <span data-ttu-id="ae626-237">Synchronizuje se jenom v případě, že se provádí gesto tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ae626-237">It syncs only when the refresh gesture button is performed.</span></span>

<span data-ttu-id="ae626-238">**Jazyka Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="ae626-238">**Objective-C**:</span></span>

1. <span data-ttu-id="ae626-239">V **QSTodoListViewController.m**, změnit **viewDidLoad** metoda odebrat volání `[self refresh]` na konci metody.</span><span class="sxs-lookup"><span data-stu-id="ae626-239">In **QSTodoListViewController.m**, change the **viewDidLoad** method to remove the call to `[self refresh]` at the end of the method.</span></span> <span data-ttu-id="ae626-240">Data se teď není synchronizovaný se serverem při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae626-240">Now the data is not synced with the server on app start.</span></span> <span data-ttu-id="ae626-241">Místo toho je synchronizovaný s obsahem místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="ae626-241">Instead, it's synced with the contents of the local store.</span></span>
2. <span data-ttu-id="ae626-242">V **QSTodoService.m**, upravte definici `addItem` tak, aby se nebude synchronizovat po vložení položky.</span><span class="sxs-lookup"><span data-stu-id="ae626-242">In **QSTodoService.m**, modify the definition of `addItem` so that it doesn't sync after the item is inserted.</span></span> <span data-ttu-id="ae626-243">Odeberte `self syncData` blokovat a nahraďte ji následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae626-243">Remove the `self syncData` block and replace it with the following:</span></span>

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. <span data-ttu-id="ae626-244">Upravit definici `completeItem` jak je uvedeno nahoře.</span><span class="sxs-lookup"><span data-stu-id="ae626-244">Modify the definition of `completeItem` as mentioned previously.</span></span> <span data-ttu-id="ae626-245">Odeberte blok pro `self syncData` a nahraďte ji následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae626-245">Remove the block for `self syncData` and replace it with the following:</span></span>
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

<span data-ttu-id="ae626-246">**Kód SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="ae626-246">**Swift**:</span></span>

<span data-ttu-id="ae626-247">V `viewDidLoad`v **ToDoTableViewController.swift**, komentář dva řádky, které jsou zde uvedeny zastavit synchronizuje se při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae626-247">In `viewDidLoad`, in **ToDoTableViewController.swift**, comment out the two lines shown here, to stop syncing on app start.</span></span> <span data-ttu-id="ae626-248">V době psaní tohoto textu aplikace Swift Todo nelze aktualizovat službu, když někdo přidá nebo dokončení položku.</span><span class="sxs-lookup"><span data-stu-id="ae626-248">At the time of this writing, the Swift Todo app does not update the service when someone adds or completes an item.</span></span> <span data-ttu-id="ae626-249">Aktualizuje službu pouze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae626-249">It updates the service only on app start.</span></span>

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <span data-ttu-id="ae626-250"><a name="test-app"></a>Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="ae626-250"><a name="test-app"></a>Test the app</span></span>
<span data-ttu-id="ae626-251">V této části se můžete připojit k neplatná adresa URL k simulaci offline scénář.</span><span class="sxs-lookup"><span data-stu-id="ae626-251">In this section, you connect to an invalid URL to simulate an offline scenario.</span></span> <span data-ttu-id="ae626-252">Když přidáte datových položek, jste uchovávat v místní základní datové úložiště, ale není synchronizovaná s back-end mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ae626-252">When you add data items, they're held in the local Core Data store, but they're not synced with the mobile-app back end.</span></span>

1. <span data-ttu-id="ae626-253">Změňte adresu URL mobilní aplikace v **QSTodoService.m** na neplatná adresa URL a znovu spusťte aplikaci:</span><span class="sxs-lookup"><span data-stu-id="ae626-253">Change the mobile-app URL in **QSTodoService.m** to an invalid URL, and run the app again:</span></span>

   <span data-ttu-id="ae626-254">**Jazyka Objective-C**.</span><span class="sxs-lookup"><span data-stu-id="ae626-254">**Objective-C**.</span></span> <span data-ttu-id="ae626-255">V QSTodoService.m:</span><span class="sxs-lookup"><span data-stu-id="ae626-255">In QSTodoService.m:</span></span>
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   <span data-ttu-id="ae626-256">**Kód SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="ae626-256">**Swift**.</span></span> <span data-ttu-id="ae626-257">V ToDoTableViewController.swift:</span><span class="sxs-lookup"><span data-stu-id="ae626-257">In ToDoTableViewController.swift:</span></span>
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. <span data-ttu-id="ae626-258">Přidejte některé položky seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="ae626-258">Add some to-do items.</span></span> <span data-ttu-id="ae626-259">Ukončení simulátoru (nebo vynuceně zavřít aplikaci) a pak ji znovu spusťte.</span><span class="sxs-lookup"><span data-stu-id="ae626-259">Quit the simulator (or forcibly close the app), and then restart it.</span></span> <span data-ttu-id="ae626-260">Ověřte, že vaše změny se zachovají.</span><span class="sxs-lookup"><span data-stu-id="ae626-260">Verify that your changes persist.</span></span>

3. <span data-ttu-id="ae626-261">Zobrazit obsah vzdáleného **TodoItem** tabulky:</span><span class="sxs-lookup"><span data-stu-id="ae626-261">View the contents of the remote **TodoItem** table:</span></span>
   * <span data-ttu-id="ae626-262">Pro Node.js back-end, přejděte na [portál Azure](https://portal.azure.com/) a ve vaší mobilní aplikace back-end, klikněte na tlačítko **snadno tabulky** > **TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="ae626-262">For a Node.js back end, go to the [Azure portal](https://portal.azure.com/) and, in your mobile-app back end, click **Easy Tables** > **TodoItem**.</span></span>  
   * <span data-ttu-id="ae626-263">Pro .NET zpět ukončení, použijte nástroj SQL, jako je například SQL Server Management Studio, nebo klienta REST, například aplikaci Fiddler nebo Postman.</span><span class="sxs-lookup"><span data-stu-id="ae626-263">For a .NET back end, use either a SQL tool, such as SQL Server Management Studio, or a REST client, such as Fiddler or Postman.</span></span>  

4. <span data-ttu-id="ae626-264">Ověřte, že nové položky *není* byla synchronizovat se serverem.</span><span class="sxs-lookup"><span data-stu-id="ae626-264">Verify that the new items have *not* been synced with the server.</span></span>

5. <span data-ttu-id="ae626-265">Změňte adresu URL zpět na tu správnou v **QSTodoService.m**a znovu spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae626-265">Change the URL back to the correct one in **QSTodoService.m**, and rerun the app.</span></span>

6. <span data-ttu-id="ae626-266">Proveďte aktualizaci gesto přidáváním dolů seznam položek.</span><span class="sxs-lookup"><span data-stu-id="ae626-266">Perform the refresh gesture by pulling down the list of items.</span></span>  
<span data-ttu-id="ae626-267">Zobrazí se průběh číselník.</span><span class="sxs-lookup"><span data-stu-id="ae626-267">A progress spinner is displayed.</span></span>

7. <span data-ttu-id="ae626-268">Zobrazení **TodoItem** data znovu.</span><span class="sxs-lookup"><span data-stu-id="ae626-268">View the **TodoItem** data again.</span></span> <span data-ttu-id="ae626-269">Úkolů nové a změněné položky by teď mělo být zobrazené.</span><span class="sxs-lookup"><span data-stu-id="ae626-269">The new and changed to-do items should now be displayed.</span></span>

## <a name="summary"></a><span data-ttu-id="ae626-270">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ae626-270">Summary</span></span>
<span data-ttu-id="ae626-271">K podpoře funkce offline synchronizace, jsme použili `MSSyncTable` rozhraní a inicializovat `MSClient.syncContext` s místním úložištěm.</span><span class="sxs-lookup"><span data-stu-id="ae626-271">To support the offline sync feature, we used the `MSSyncTable` interface and initialized `MSClient.syncContext` with a local store.</span></span> <span data-ttu-id="ae626-272">V takovém případě se místní úložiště databáze založené na datech jádra.</span><span class="sxs-lookup"><span data-stu-id="ae626-272">In this case, the local store was a Core Data-based database.</span></span>

<span data-ttu-id="ae626-273">Pokud používáte místní úložiště dat jádra, je nutné zadat několik tabulek s [opravte vlastnosti systému](#review-core-data).</span><span class="sxs-lookup"><span data-stu-id="ae626-273">When you use a Core Data local store, you must define several tables with the [correct system properties](#review-core-data).</span></span>

<span data-ttu-id="ae626-274">Normální vytvořit, číst, aktualizovat a odstranit operace pro mobilní aplikace pracují, jako kdyby aplikace stále připojený, ale všechny operace nastat proti místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="ae626-274">The normal create, read, update, and delete (CRUD) operations for mobile apps work as if the app is still connected, but all the operations occur against the local store.</span></span>

<span data-ttu-id="ae626-275">Když jsme místní úložiště synchronizovány se serverem, jsme použili **MSSyncTable.pullWithQuery** metoda.</span><span class="sxs-lookup"><span data-stu-id="ae626-275">When we synchronized the local store with the server, we used the **MSSyncTable.pullWithQuery** method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ae626-276">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ae626-276">Additional resources</span></span>
* <span data-ttu-id="ae626-277">[Offline synchronizací dat v Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="ae626-277">[Offline Data Sync in Mobile Apps]</span></span>
* <span data-ttu-id="ae626-278">[Obálka cloudu: Offline synchronizace v Azure Mobile Services] \(je o Mobile Services, ale Mobile Apps offline synchronizace funguje podobným způsobem.\)</span><span class="sxs-lookup"><span data-stu-id="ae626-278">[Cloud Cover: Offline Sync in Azure Mobile Services] \(The video is about Mobile Services, but Mobile Apps offline sync works in a similar way.\)</span></span>

<!-- URLs. -->


<span data-ttu-id="ae626-279">[vytvořit aplikaci pro iOS]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="ae626-279">[Create an iOS App]: app-service-mobile-ios-get-started.md</span></span>
<span data-ttu-id="ae626-280">[Offline synchronizací dat v Mobile Apps]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="ae626-280">[Offline Data Sync in Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

<span data-ttu-id="ae626-281">[Obálka cloudu: Offline synchronizace v Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span><span class="sxs-lookup"><span data-stu-id="ae626-281">[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span></span>
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
