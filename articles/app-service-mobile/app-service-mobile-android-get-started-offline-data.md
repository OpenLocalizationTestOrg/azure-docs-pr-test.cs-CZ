---
title: "offline synchronizace aaaEnable mobilní aplikace Azure (Android)"
description: "Zjistěte, jak toouse App Service Mobile Apps toocache a synchronizaci dat offline v aplikaci pro Android"
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
ms.openlocfilehash: 34508c7394610cf9127e1753637940826b8fd06a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a><span data-ttu-id="862a5-103">Zapnutí offline synchronizace pro mobilní aplikaci s Androidem</span><span class="sxs-lookup"><span data-stu-id="862a5-103">Enable offline sync for your Android mobile app</span></span>
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a><span data-ttu-id="862a5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="862a5-104">Overview</span></span>
<span data-ttu-id="862a5-105">Tento kurz se zaměřuje hello offline synchronizace funkci Azure Mobile Apps pro Android.</span><span class="sxs-lookup"><span data-stu-id="862a5-105">This tutorial covers hello offline sync feature of Azure Mobile Apps for Android.</span></span> <span data-ttu-id="862a5-106">Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="862a5-106">Offline sync allows end users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span> <span data-ttu-id="862a5-107">Změny se ukládají do místní databáze.</span><span class="sxs-lookup"><span data-stu-id="862a5-107">Changes are stored in a local database.</span></span> <span data-ttu-id="862a5-108">Jakmile je zařízení hello zpět do režimu online, tyto změny synchronizované s hello vzdálené back-end.</span><span class="sxs-lookup"><span data-stu-id="862a5-108">Once hello device is back online, these changes are synced with hello remote backend.</span></span>

<span data-ttu-id="862a5-109">Pokud je vaše první zkušenosti s Azure Mobile Apps, byste měli nejprve dokončit kurz hello [vytvoření aplikace pro Android].</span><span class="sxs-lookup"><span data-stu-id="862a5-109">If this is your first experience with Azure Mobile Apps, you should first complete hello tutorial [Create an Android App].</span></span> <span data-ttu-id="862a5-110">Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello data přístup rozšíření balíčky tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="862a5-110">If you do not use hello downloaded quick start server project, you must add hello data access extension packages tooyour project.</span></span> <span data-ttu-id="862a5-111">Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="862a5-111">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

<span data-ttu-id="862a5-112">toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="862a5-112">toolearn more about hello offline sync feature, see hello topic [Offline Data Sync in Azure Mobile Apps].</span></span>

## <a name="update-hello-app-toosupport-offline-sync"></a><span data-ttu-id="862a5-113">Aktualizace offline synchronizace toosupport hello aplikace</span><span class="sxs-lookup"><span data-stu-id="862a5-113">Update hello app toosupport offline sync</span></span>
<span data-ttu-id="862a5-114">S offline synchronizací číst tooand zápisu z *synchronizace tabulky* (pomocí hello *IMobileServiceSyncTable* rozhraní), který je součástí **SQLite** databáze na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="862a5-114">With offline sync, you read tooand write from a *sync table* (using hello *IMobileServiceSyncTable* interface), which is part of a **SQLite** database on your device.</span></span>

<span data-ttu-id="862a5-115">toopush a vyžadování změny mezi hello zařízení a mobilní služby Azure, můžete použít *kontext synchronizace* (*MobileServiceClient.SyncContext*), který inicializaci s místní databází hello toostore data v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="862a5-115">toopush and pull changes between hello device and Azure Mobile Services, you use a *synchronization context* (*MobileServiceClient.SyncContext*), which you initialize with hello local database toostore data locally.</span></span>

1. <span data-ttu-id="862a5-116">V `TodoActivity.java`, komentář hello existující definice `mToDoTable` a zrušte komentář u verze tabulky hello synchronizace:</span><span class="sxs-lookup"><span data-stu-id="862a5-116">In `TodoActivity.java`, comment out hello existing definition of `mToDoTable` and uncomment hello sync table version:</span></span>
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. <span data-ttu-id="862a5-117">V hello `onCreate` metoda komentář hello inicializace existující `mToDoTable` a zrušte komentář u této definici:</span><span class="sxs-lookup"><span data-stu-id="862a5-117">In hello `onCreate` method, comment out hello existing initialization of `mToDoTable` and uncomment this definition:</span></span>
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. <span data-ttu-id="862a5-118">V `refreshItemsFromTable` komentář hello definice `results` a zrušte komentář u této definici:</span><span class="sxs-lookup"><span data-stu-id="862a5-118">In `refreshItemsFromTable` comment out hello definition of `results` and uncomment this definition:</span></span>
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. <span data-ttu-id="862a5-119">Komentář hello definice `refreshItemsFromMobileServiceTable`.</span><span class="sxs-lookup"><span data-stu-id="862a5-119">Comment out hello definition of `refreshItemsFromMobileServiceTable`.</span></span>
5. <span data-ttu-id="862a5-120">Zrušením komentáře u hello definice `refreshItemsFromMobileServiceTableSyncTable`:</span><span class="sxs-lookup"><span data-stu-id="862a5-120">Uncomment hello definition of `refreshItemsFromMobileServiceTableSyncTable`:</span></span>
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync hello data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. <span data-ttu-id="862a5-121">Zrušením komentáře u hello definice `sync`:</span><span class="sxs-lookup"><span data-stu-id="862a5-121">Uncomment hello definition of `sync`:</span></span>
   
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

## <a name="test-hello-app"></a><span data-ttu-id="862a5-122">Testování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="862a5-122">Test hello app</span></span>
<span data-ttu-id="862a5-123">V této části na test chování hello s Wi-Fi a pak vypněte toocreate Wi-Fi offline scénář.</span><span class="sxs-lookup"><span data-stu-id="862a5-123">In this section, you test hello behavior with WiFi on, and then turn off WiFi toocreate an offline scenario.</span></span>

<span data-ttu-id="862a5-124">Když přidáte datových položek, dokud stiskněte hello, jejich jsou uložené v místní úložiště SQLite hello, ale není synchronizovaná toohello mobilní služby **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="862a5-124">When you add data items, they are held in hello local SQLite store, but not synced toohello mobile service until you press hello **Refresh** button.</span></span> <span data-ttu-id="862a5-125">Ostatní aplikace může mít jiné požadavky týkající se když data potřebuje toobe synchronizovány, ale pro účely ukázky v tomto kurzu má explicitní žádost o uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="862a5-125">Other apps may have different requirements regarding when data needs toobe synchronized, but for demo purposes this tutorial has hello user explicitly request it.</span></span>

<span data-ttu-id="862a5-126">Stisknutím tohoto tlačítka spustí novou úlohu pozadí.</span><span class="sxs-lookup"><span data-stu-id="862a5-126">When you press that button, a new background task starts.</span></span> <span data-ttu-id="862a5-127">Vynutí nejprve všechny změny provedené toohello místní úložiště pomocí kontextu synchronizace a potom si změnit data z Azure toohello místní tabulky.</span><span class="sxs-lookup"><span data-stu-id="862a5-127">It first pushes all changes made toohello local store using synchronization context, then pulls all changed data from Azure toohello local table.</span></span>

### <a name="offline-testing"></a><span data-ttu-id="862a5-128">Offline testování</span><span class="sxs-lookup"><span data-stu-id="862a5-128">Offline testing</span></span>
1. <span data-ttu-id="862a5-129">Místní hello zařízení ani simulátor v *režim v letadle*.</span><span class="sxs-lookup"><span data-stu-id="862a5-129">Place hello device or simulator in *Airplane Mode*.</span></span> <span data-ttu-id="862a5-130">Tím se vytvoří offline scénář.</span><span class="sxs-lookup"><span data-stu-id="862a5-130">This creates an offline scenario.</span></span>
2. <span data-ttu-id="862a5-131">Přidejte nějaké *ToDo* položky nebo označit některé položky jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="862a5-131">Add some *ToDo* items, or mark some items as complete.</span></span> <span data-ttu-id="862a5-132">Ukončení hello zařízení nebo simulátoru (nebo vynuceně zavřít hello aplikace) a restartování.</span><span class="sxs-lookup"><span data-stu-id="862a5-132">Quit hello device or simulator (or forcibly close hello app) and restart.</span></span> <span data-ttu-id="862a5-133">Ověřte, že změny jsou držena formou na zařízení hello vzhledem k tomu, že jsou uložené v hello místní úložiště SQLite.</span><span class="sxs-lookup"><span data-stu-id="862a5-133">Verify that your changes have been persisted on hello device because they are held in hello local SQLite store.</span></span>
3. <span data-ttu-id="862a5-134">Zobrazit obsah hello hello Azure *TodoItem* tabulky buď pomocí nástroje SQL, jako *SQL Server Management Studio*, nebo REST klienta *Fiddler* nebo  *Postman*.</span><span class="sxs-lookup"><span data-stu-id="862a5-134">View hello contents of hello Azure *TodoItem* table either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span> <span data-ttu-id="862a5-135">Ověřte, že nové položky hello *není* byly synchronizované toohello serveru</span><span class="sxs-lookup"><span data-stu-id="862a5-135">Verify that hello new items have *not* been synced toohello server</span></span>
   
       + <span data-ttu-id="862a5-136">Back-end Node.js najdete toohello [portál Azure](https://portal.azure.com/), a klikněte na mobilní aplikace back-end **snadno tabulky** > **TodoItem** tooview hello obsah hello `TodoItem` tabulky.</span><span class="sxs-lookup"><span data-stu-id="862a5-136">For a Node.js backend, go toohello [Azure portal](https://portal.azure.com/), and in your Mobile App backend click **Easy Tables** > **TodoItem** tooview hello contents of hello `TodoItem` table.</span></span>
       + <span data-ttu-id="862a5-137">Pro rozhraní .NET back-end, zobrazení hello tabulky obsah buď pomocí nástroje SQL, jako *SQL Server Management Studio*, nebo REST klienta *Fiddler* nebo *Postman*.</span><span class="sxs-lookup"><span data-stu-id="862a5-137">For a .NET backend, view hello table contents either with a SQL tool such as *SQL Server Management Studio*, or a REST client such as *Fiddler* or *Postman*.</span></span>
4. <span data-ttu-id="862a5-138">Zapněte Wi-Fi v zařízení hello ani simulátor.</span><span class="sxs-lookup"><span data-stu-id="862a5-138">Turn on WiFi in hello device or simulator.</span></span> <span data-ttu-id="862a5-139">Stiskněte klávesu hello **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="862a5-139">Next, press hello **Refresh** button.</span></span>
5. <span data-ttu-id="862a5-140">Zobrazte hello TodoItem data znovu v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="862a5-140">View hello TodoItem data again in hello Azure portal.</span></span> <span data-ttu-id="862a5-141">Dobrý den, které by se měla objevit nové a změněné TodoItems.</span><span class="sxs-lookup"><span data-stu-id="862a5-141">hello new and changed TodoItems should now appear.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="862a5-142">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="862a5-142">Additional Resources</span></span>
* <span data-ttu-id="862a5-143">[Offline synchronizací dat v Azure Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="862a5-143">[Offline Data Sync in Azure Mobile Apps]</span></span>
* <span data-ttu-id="862a5-144">[Obálka cloudu: Offline synchronizace v Azure Mobile Services] \(Poznámka: je hello video v Mobile Services, ale offline synchronizace funguje podobným způsobem jako v Azure Mobile Apps\)</span><span class="sxs-lookup"><span data-stu-id="862a5-144">[Cloud Cover: Offline Sync in Azure Mobile Services] \(note: hello video is on Mobile Services, but offline sync works in a similar way in Azure Mobile Apps\)</span></span>

<!-- URLs. -->

[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md

[vytvoření aplikace pro Android]: app-service-mobile-android-get-started.md

[Obálka cloudu: Offline synchronizace v Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

