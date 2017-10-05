---
title: "Zapnutí offline synchronizace pro mobilní aplikace Azure (Android)"
description: "Naučte se používat App Service Mobile Apps do mezipaměti a synchronizaci dat offline vaší aplikace Android"
documentationcenter: android
author: ggailey777
manager: syntaxc4
services: app-service\mobile
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 304323c1816302e8c1f68f36a029aee55e02c54e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a><span data-ttu-id="59fb4-103">Zapnutí offline synchronizace pro mobilní aplikaci s Androidem</span><span class="sxs-lookup"><span data-stu-id="59fb4-103">Enable offline sync for your Android mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="59fb4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="59fb4-104">Overview</span></span>
<span data-ttu-id="59fb4-105">Tento kurz se zaměřuje offline synchronizace funkci Azure Mobile Apps pro Android.</span><span class="sxs-lookup"><span data-stu-id="59fb4-105">This tutorial covers the offline sync feature of Azure Mobile Apps for Android.</span></span> <span data-ttu-id="59fb4-106">Offline synchronizace umožňuje koncovým uživatelům pracovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="59fb4-106">Offline sync allows end users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="59fb4-107">Změny se ukládají do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="59fb4-107">Changes are stored in a local database.</span></span> <span data-ttu-id="59fb4-108">Jakmile zařízení do režimu online, tyto změny se synchronizují s vzdálené back-end.</span><span class="sxs-lookup"><span data-stu-id="59fb4-108">Once the device is back online, these changes are synced with the remote backend.</span></span>

<span data-ttu-id="59fb4-109">Pokud je vaše první zkušenosti s Azure Mobile Apps, musí nejprve dokončit kurz [vytvoření aplikace pro Android].</span><span class="sxs-lookup"><span data-stu-id="59fb4-109">If this is your first experience with Azure Mobile Apps, you should first complete the tutorial [Create an Android App].</span></span> <span data-ttu-id="59fb4-110">Pokud použijete serverový projekt stažené rychlý start, je nutné přidat data přístup rozšiřující balíčky do projektu.</span><span class="sxs-lookup"><span data-stu-id="59fb4-110">If you do not use the downloaded quick start server project, you must add the data access extension packages to your project.</span></span> <span data-ttu-id="59fb4-111">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="59fb4-111">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="59fb4-112">Další informace o funkci offline synchronizace, naleznete v tématu [Offline synchronizací dat v Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="59fb4-112">To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="update-the-app-to-support-offline-sync"></a><span data-ttu-id="59fb4-113">Aktualizace aplikace pro podporu offline synchronizace</span><span class="sxs-lookup"><span data-stu-id="59fb4-113">Update the app to support offline sync</span></span>
<span data-ttu-id="59fb4-114">S offline synchronizací pro čtení a zápisu z *synchronizace tabulky* (pomocí *IMobileServiceSyncTable* rozhraní), které je součástí **SQLite** databáze na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="59fb4-114">With offline sync, you read to and write from a *sync table* (using the *IMobileServiceSyncTable* interface), which is part of a **SQLite** database on your device.</span></span>

<span data-ttu-id="59fb4-115">Pokud chcete push a změny mezi zařízením a Azure Mobile Services pro vyžádání obsahu, použijte *kontext synchronizace* (*MobileServiceClient.SyncContext*), který inicializace s místní databázi k ukládání data v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="59fb4-115">To push and pull changes between the device and Azure Mobile Services, you use a *synchronization context* (*MobileServiceClient.SyncContext*), which you initialize with the local database to store data locally.</span></span>

1. <span data-ttu-id="59fb4-116">V `TodoActivity.java`, komentář existující definice `mToDoTable` a zrušte komentář u tabulky verze synchronizace:</span><span class="sxs-lookup"><span data-stu-id="59fb4-116">In `TodoActivity.java`, comment out the existing definition of `mToDoTable` and uncomment the sync table version:</span></span>
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. <span data-ttu-id="59fb4-117">V `onCreate` metoda komentář existující inicializace `mToDoTable` a zrušte komentář u této definici:</span><span class="sxs-lookup"><span data-stu-id="59fb4-117">In the `onCreate` method, comment out the existing initialization of `mToDoTable` and uncomment this definition:</span></span>
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. <span data-ttu-id="59fb4-118">V `refreshItemsFromTable` komentář definice `results` a zrušte komentář u této definici:</span><span class="sxs-lookup"><span data-stu-id="59fb4-118">In `refreshItemsFromTable` comment out the definition of `results` and uncomment this definition:</span></span>
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. <span data-ttu-id="59fb4-119">Komentář definice `refreshItemsFromMobileServiceTable`.</span><span class="sxs-lookup"><span data-stu-id="59fb4-119">Comment out the definition of `refreshItemsFromMobileServiceTable`.</span></span>
5. <span data-ttu-id="59fb4-120">Zrušením komentáře u definici `refreshItemsFromMobileServiceTableSyncTable`:</span><span class="sxs-lookup"><span data-stu-id="59fb4-120">Uncomment the definition of `refreshItemsFromMobileServiceTableSyncTable`:</span></span>
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. <span data-ttu-id="59fb4-121">Zrušením komentáře u definici `sync`:</span><span class="sxs-lookup"><span data-stu-id="59fb4-121">Uncomment the definition of `sync`:</span></span>
   
        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a><span data-ttu-id="59fb4-122">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="59fb4-122">Test the app</span></span>
<span data-ttu-id="59fb4-123">V této části na test chování s Wi-Fi a pak vypněte Wi-Fi k vytvoření offline scénář.</span><span class="sxs-lookup"><span data-stu-id="59fb4-123">In this section, you test the behavior with WiFi on, and then turn off WiFi to create an offline scenario.</span></span>

<span data-ttu-id="59fb4-124">Když přidáte datových položek, jsou uchovávat v místní úložiště SQLite, ale není synchronizovat do mobilní služby, dokud stisknete **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="59fb4-124">When you add data items, they are held in the local SQLite store, but not synced to the mobile service until you press the **Refresh** button.</span></span> <span data-ttu-id="59fb4-125">Ostatní aplikace může mít jiné požadavky týkající se při data musí být synchronizovány, ale pro účely ukázky v tomto kurzu má uživatel explicitní žádost o.</span><span class="sxs-lookup"><span data-stu-id="59fb4-125">Other apps may have different requirements regarding when data needs to be synchronized, but for demo purposes this tutorial has the user explicitly request it.</span></span>

<span data-ttu-id="59fb4-126">Stisknutím tohoto tlačítka spustí novou úlohu pozadí.</span><span class="sxs-lookup"><span data-stu-id="59fb4-126">When you press that button, a new background task starts.</span></span> <span data-ttu-id="59fb4-127">Vynutí nejprve všechny změny provedené v místním úložišti pomocí kontextu synchronizace a potom si změnit dat z Azure do místní tabulky.</span><span class="sxs-lookup"><span data-stu-id="59fb4-127">It first pushes all changes made to the local store using synchronization context, then pulls all changed data from Azure to the local table.</span></span>

### <a name="offline-testing"></a><span data-ttu-id="59fb4-128">Offline testování</span><span class="sxs-lookup"><span data-stu-id="59fb4-128">Offline testing</span></span>
1. <span data-ttu-id="59fb4-129">Umístit zařízení nebo v simulátoru *režim v letadle*.</span><span class="sxs-lookup"><span data-stu-id="59fb4-129">Place the device or simulator in *Airplane Mode*.</span></span> <span data-ttu-id="59fb4-130">Tím se vytvoří offline scénář.</span><span class="sxs-lookup"><span data-stu-id="59fb4-130">This creates an offline scenario.</span></span>
2. <span data-ttu-id="59fb4-131">Přidejte nějaké *ToDo* položky nebo označit některé položky jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="59fb4-131">Add some *ToDo* items, or mark some items as complete.</span></span> <span data-ttu-id="59fb4-132">Ukončení zařízení ani simulátor (nebo vynuceně zavřít aplikaci) a znovu spusťte.</span><span class="sxs-lookup"><span data-stu-id="59fb4-132">Quit the device or simulator (or forcibly close the app) and restart.</span></span> <span data-ttu-id="59fb4-133">Ověřte, že změny jsou držena formou na zařízení vzhledem k tomu, že jsou uložené v místní úložiště SQLite.</span><span class="sxs-lookup"><span data-stu-id="59fb4-133">Verify that your changes have been persisted on the device because they are held in the local SQLite store.</span></span>
3. <span data-ttu-id="59fb4-134">Zobrazení obsahu Azure *TodoItem* tabulky buď pomocí nástroje SQL, jako *SQL Server Management Studio*, nebo REST klienta *Fiddler* nebo  *Postman*.</span><span class="sxs-lookup"><span data-stu-id="59fb4-134">View the contents of the Azure *TodoItem* table either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span> <span data-ttu-id="59fb4-135">Ověřte, že nové položky *není* byly synchronizované do serveru</span><span class="sxs-lookup"><span data-stu-id="59fb4-135">Verify that the new items have *not* been synced to the server</span></span>
   
       + <span data-ttu-id="59fb4-136">Back-end Node.js, najdete [portál Azure](https://portal.azure.com/), a v mobilní aplikace klikněte na back-end **snadno tabulky** > **TodoItem** Chcete-li zobrazit obsah `TodoItem`tabulky.</span><span class="sxs-lookup"><span data-stu-id="59fb4-136">For a Node.js backend, go to the [Azure portal](https://portal.azure.com/), and in your Mobile App backend click **Easy Tables** > **TodoItem** to view the contents of the `TodoItem` table.</span></span>
       + <span data-ttu-id="59fb4-137">Pro rozhraní .NET back-end, zobrazit obsah tabulky buď pomocí SQL nástroje, jako *SQL Server Management Studio*, nebo REST klienta *Fiddler* nebo *Postman*.</span><span class="sxs-lookup"><span data-stu-id="59fb4-137">For a .NET backend, view the table contents either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span>
4. <span data-ttu-id="59fb4-138">Zapněte Wi-Fi v zařízení ani simulátor.</span><span class="sxs-lookup"><span data-stu-id="59fb4-138">Turn on WiFi in the device or simulator.</span></span> <span data-ttu-id="59fb4-139">Stiskněte klávesu **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="59fb4-139">Next, press the **Refresh** button.</span></span>
5. <span data-ttu-id="59fb4-140">Zobrazte TodoItem data znovu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="59fb4-140">View the TodoItem data again in the Azure portal.</span></span> <span data-ttu-id="59fb4-141">Nové a změněné TodoItems by se měla zobrazit.</span><span class="sxs-lookup"><span data-stu-id="59fb4-141">The new and changed TodoItems should now appear.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59fb4-142">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="59fb4-142">Additional Resources</span></span>
* <span data-ttu-id="59fb4-143">[Offline synchronizací dat v Azure Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="59fb4-143">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="59fb4-144">[Obálka cloudu: Offline synchronizace v Azure Mobile Services] \(Poznámka: je v Mobile Services, ale offline synchronizace funguje podobným způsobem jako v Azure Mobile Apps\)</span><span class="sxs-lookup"><span data-stu-id="59fb4-144">[Cloud Cover: Offline Sync in Azure Mobile Services] \(note: the video is on Mobile Services, but offline sync works in a similar way in Azure Mobile Apps\)</span></span>

<!-- URLs. -->

<span data-ttu-id="59fb4-145">[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="59fb4-145">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>

<span data-ttu-id="59fb4-146">[vytvoření aplikace pro Android]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="59fb4-146">[Create an Android App]: app-service-mobile-android-get-started.md</span></span>

<span data-ttu-id="59fb4-147">[Obálka cloudu: Offline synchronizace v Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span><span class="sxs-lookup"><span data-stu-id="59fb4-147">[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri</span></span>
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

