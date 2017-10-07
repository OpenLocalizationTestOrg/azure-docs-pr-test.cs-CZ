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
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Zapnutí offline synchronizace pro mobilní aplikaci s Androidem
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Přehled
Tento kurz se zaměřuje hello offline synchronizace funkci Azure Mobile Apps pro Android. Offline synchronizace umožňuje koncovým uživatelům toointeract s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení. Změny se ukládají do místní databáze. Jakmile je zařízení hello zpět do režimu online, tyto změny synchronizované s hello vzdálené back-end.

Pokud je vaše první zkušenosti s Azure Mobile Apps, byste měli nejprve dokončit kurz hello [vytvoření aplikace pro Android]. Pokud nepoužijete hello stáhli úvodní serverový projekt, je nutné přidat hello data přístup rozšíření balíčky tooyour projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn Další informace o funkci hello offline synchronizace, najdete v tématu hello [Offline synchronizací dat v Azure Mobile Apps].

## <a name="update-hello-app-toosupport-offline-sync"></a>Aktualizace offline synchronizace toosupport hello aplikace
S offline synchronizací číst tooand zápisu z *synchronizace tabulky* (pomocí hello *IMobileServiceSyncTable* rozhraní), který je součástí **SQLite** databáze na vašem zařízení.

toopush a vyžadování změny mezi hello zařízení a mobilní služby Azure, můžete použít *kontext synchronizace* (*MobileServiceClient.SyncContext*), který inicializaci s místní databází hello toostore data v místním počítači.

1. V `TodoActivity.java`, komentář hello existující definice `mToDoTable` a zrušte komentář u verze tabulky hello synchronizace:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. V hello `onCreate` metoda komentář hello inicializace existující `mToDoTable` a zrušte komentář u této definici:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. V `refreshItemsFromTable` komentář hello definice `results` a zrušte komentář u této definici:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. Komentář hello definice `refreshItemsFromMobileServiceTable`.
5. Zrušením komentáře u hello definice `refreshItemsFromMobileServiceTableSyncTable`:
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync hello data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. Zrušením komentáře u hello definice `sync`:
   
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

## <a name="test-hello-app"></a>Testování aplikace hello
V této části na test chování hello s Wi-Fi a pak vypněte toocreate Wi-Fi offline scénář.

Když přidáte datových položek, dokud stiskněte hello, jejich jsou uložené v místní úložiště SQLite hello, ale není synchronizovaná toohello mobilní služby **aktualizovat** tlačítko. Ostatní aplikace může mít jiné požadavky týkající se když data potřebuje toobe synchronizovány, ale pro účely ukázky v tomto kurzu má explicitní žádost o uživatele hello.

Stisknutím tohoto tlačítka spustí novou úlohu pozadí. Vynutí nejprve všechny změny provedené toohello místní úložiště pomocí kontextu synchronizace a potom si změnit data z Azure toohello místní tabulky.

### <a name="offline-testing"></a>Offline testování
1. Místní hello zařízení ani simulátor v *režim v letadle*. Tím se vytvoří offline scénář.
2. Přidejte nějaké *ToDo* položky nebo označit některé položky jako dokončené. Ukončení hello zařízení nebo simulátoru (nebo vynuceně zavřít hello aplikace) a restartování. Ověřte, že změny jsou držena formou na zařízení hello vzhledem k tomu, že jsou uložené v hello místní úložiště SQLite.
3. Zobrazit obsah hello hello Azure *TodoItem* tabulky buď pomocí nástroje SQL, jako *SQL Server Management Studio*, nebo REST klienta *Fiddler* nebo  *Postman*. Ověřte, že nové položky hello *není* byly synchronizované toohello serveru
   
       + Back-end Node.js najdete toohello [portál Azure](https://portal.azure.com/), a klikněte na mobilní aplikace back-end **snadno tabulky** > **TodoItem** tooview hello obsah hello `TodoItem` tabulky.
       + Pro rozhraní .NET back-end, zobrazení hello tabulky obsah buď pomocí nástroje SQL, jako *SQL Server Management Studio*, nebo REST klienta *Fiddler* nebo *Postman*.
4. Zapněte Wi-Fi v zařízení hello ani simulátor. Stiskněte klávesu hello **aktualizovat** tlačítko.
5. Zobrazte hello TodoItem data znovu v hello portálu Azure. Dobrý den, které by se měla objevit nové a změněné TodoItems.

## <a name="additional-resources"></a>Další zdroje
* [Offline synchronizací dat v Azure Mobile Apps]
* [Obálka cloudu: Offline synchronizace v Azure Mobile Services] \(Poznámka: je hello video v Mobile Services, ale offline synchronizace funguje podobným způsobem jako v Azure Mobile Apps\)

<!-- URLs. -->

[Offline synchronizací dat v Azure Mobile Apps]: app-service-mobile-offline-data-sync.md

[vytvoření aplikace pro Android]: app-service-mobile-android-get-started.md

[Obálka cloudu: Offline synchronizace v Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

