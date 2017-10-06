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
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a>Povolit offline synchronizace u mobilních aplikací pro iOS
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Přehled
Tento kurz se zaměřuje na offline synchronizace s hello funkce Azure App Service Mobile Apps pro iOS. S offline synchronizaci koncoví uživatelé můžete pracovat s tooview mobilní aplikace, přidat nebo upravit data, i když mají žádné síťové připojení. Změny se ukládají do místní databáze. Když hello zařízení do režimu online, změny hello synchronizované s hello vzdálené back end.

Pokud je vaše první zkušenosti s Mobile Apps, byste měli nejprve dokončit kurz hello [vytvořit aplikaci pro iOS]. Pokud nepoužijete hello stáhli úvodní serverový projekt, musíte přidat hello přístup k datům rozšíření balíčky tooyour projektu. Další informace o balíčcích rozšíření serveru najdete v tématu [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

toolearn Další informace o funkci hello offline synchronizace, najdete v části [Offline synchronizací dat v Mobile Apps].

## <a name="review-sync"></a>Zkontrolujte kód synchronizace klienta hello
Hello klientského projektu, který jste stáhli pro hello [vytvořit aplikaci pro iOS] kurzu již obsahuje kód, který podporuje offline synchronizace pomocí místní databáze založené na datech jádra. Tento oddíl shrnuje, co je již součástí hello kódu. Koncepční přehled hello funkce, najdete v tématu [Offline synchronizací dat v Mobile Apps].

Pomocí funkce offline synchronizací dat hello mobilních aplikací mohou koncoví uživatelé komunikovat s místní databáze, i v případě, že síť hello je nepřístupný. toouse tyto funkce ve vaší aplikaci inicializovat kontext synchronizace hello `MSClient` a odkazovat na místní úložiště. Pak referenční tabulku prostřednictvím hello **MSSyncTable** rozhraní.

V **QSTodoService.m** (Objective-C) nebo **ToDoTableViewController.swift** (Swift), Všimněte si, že hello typ člena hello **syncTable** je  **MSSyncTable**. Offline synchronizace používá toto rozhraní tabulky synchronizace místo **MSTable**. Pokud se používá tabulku synchronizace, všechny operace přejděte toohello místního úložiště a jsou synchronizovány pouze s hello vzdálené back-end s explicitní nabízené a operací pro vyžádání obsahu.

 tooget referenční tooa synchronizace tabulku, použijte hello **syncTableWithName** metodu `MSClient`. funkci offline synchronizace tooremove, použijte **tableWithName** místo.

Před provedením jakékoli operace s tabulkou, musí být inicializován hello místního úložiště. Tady je relevantní kód hello:

* **Jazyka Objective-C**. V hello **QSTodoService.init** metoda:

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* **Kód SWIFT**. V hello **ToDoTableViewController.viewDidLoad** metoda:

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of hello Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   Tato metoda vytvoří místní úložiště pomocí hello `MSCoreDataStore` poskytuje rozhraní, což hello Mobile Apps SDK. Alternativně můžete zadat různé ukládají v místním úložišti implementací hello `MSSyncContextDataSource` protokolu. Navíc hello první parametr **MSSyncContext** je použité toospecify obslužnou rutinu konflikt. Protože jsme uplynulo `nil`, se nám získat hello výchozí konflikt rutiny, který selže ve všech konflikt.

Nyní Pojďme hello skutečné synchronizace operaci provést a získat data z hello vzdálené back-end:

* **Jazyka Objective-C**. `syncData`nejprve nabízených oznámení nové změny a pak zavolá **pullData** tooget data ze vzdáleného back endu hello. Pak hello **pullData** metoda získá nová data, která odpovídá dotazu:

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
* **Kód SWIFT**:
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

Ve verzi hello jazyka Objective-C v `syncData`, nejprve říkáme **pushWithCompletion** v kontextu synchronizace hello. Tato metoda je členem skupiny `MSSyncContext` (a ne hello synchronizace samotné tabulky) protože vynutí změny mezi všechny tabulky. Pouze záznamy, které byly upraveny nějakým způsobem místně (prostřednictvím operace vytvoření) jsou odesílány toohello serveru. Potom hello pomocná **pullData** je volána, který volá **MSSyncTable.pullWithQuery** tooretrieve Vzdálená data a ukládá je v místní databázi hello.

V hello Swift verze, protože operace push hello není nezbytně nutné, není žádná volání příliš**pushWithCompletion**. Pokud jsou všechny změny čekající na vyřízení ve hello kontext synchronizace pro hello tabulku, která provádí operace push, pull vždy problémy push nejdřív. Pokud máte více než jedné tabulky synchronizace, je však nejlepší tooexplicitly volání nabízené tooensure že všechno, co je konzistentní napříč související tabulky.

V hello jazyka Objective-C a Swift verze, můžete použít hello **pullWithQuery** toospecify metoda dotazu toofilter hello záznamy, které chcete tooretrieve. V tomto příkladu hello dotaz načte všechny záznamy v hello vzdálené `TodoItem` tabulky.

druhý parametr Hello **pullWithQuery** je ID dotazu, který se používá pro *přírůstkové synchronizace*. Přírůstkové synchronizace vyhledá všechny záznamy, která byla změněna od poslední synchronizace hello pomocí záznamu hello `UpdatedAt` časové razítko (nazývá `updatedAt` v hello místním úložišti.) hello ID dotazu by měla být popisný řetězec, který je jedinečný pro každý logický dotaz v vaše aplikace. tooopt mimo přírůstkové synchronizace předat `nil` jako hello ID dotazu. Tento přístup může být potenciálně neefektivní, protože ho načte všechny záznamy na každou vyžádanou operaci.

aplikace Hello jazyka Objective-C synchronizuje se při úpravě nebo přidání dat, když uživatel provede gesto hello aktualizace a při spuštění.

Swift aplikace Hello synchronizuje hello uživatel provede gesto hello aktualizace a při spuštění.

Protože hello aplikace synchronizacemi vždy, když je dat změněna (Objective-C) nebo při každém spuštění aplikace hello (Objective-C a Swift), aplikace hello předpokládá, že tento uživatel hello je online. V další části bude aktualizace hello aplikace tak, aby uživatelé mohou upravovat, i když se nachází v režimu offline.

## <a name="review-core-data"></a>Zkontrolujte hello základní datový model
Pokud použijete hello základní Data offline úložiště, je nutné zadat konkrétní tabulky a pole v datovém modelu. Ukázková aplikace Hello již obsahuje datový model se hello správném formátu. V této části jsme provede tyto tabulky tooshow jejich použití.

Otevřete **QSDataModel.xcdatamodeld**. Čtyři tabulky jsou definované--tři, které používá hello SDK a ten, který se používá pro hello úkolů sami položky:
  * MS_TableOperations: Sleduje hello položky, které je třeba toobe synchronizovány se serverem hello.
  * MS_TableOperationErrors: Sleduje všechny chyby, které dojít během offline synchronizace.
  * MS_TableConfig: Sleduje hello poslední čas poslední aktualizace pro hello poslední synchronizace pro všechny operace vyžádání obsahu.
  * TodoItem: Ukládá položky úkolů hello. Hello systémové sloupce **createdAt**, **updatedAt**, a **verze** jsou vlastnosti volitelné systému.

> [!NOTE]
> Hello Mobile Apps SDK rezervuje názvy sloupců, které začínají "**``**". Nepoužívejte tuto předponu s jakoukoli jinou hodnotu než systémové sloupce. Názvy sloupců jsou, jinak hodnota upravovat, když používáte vzdálené back end hello.
>
>

Při použití funkce offline synchronizace hello definujte hello tři systémové tabulky a hello dat tabulky.

### <a name="system-tables"></a>Systémové tabulky

**MS_TableOperations**  

![Atributy MS_TableOperations tabulky][defining-core-data-tableoperations-entity]

| Atribut | Typ |
| --- | --- |
| id | Celé číslo 64 |
| itemId | Řetězec |
| properties | Binární Data |
| Tabulka | Řetězec |
| tableKind | Celé číslo 16 |


**MS_TableOperationErrors**

 ![Atributy MS_TableOperationErrors tabulky][defining-core-data-tableoperationerrors-entity]

| Atribut | Typ |
| --- | --- |
| id |Řetězec |
| operationId |Celé číslo 64 |
| properties |Binární Data |
| tableKind |Celé číslo 16 |

 **MS_TableConfig**

 ![][defining-core-data-tableconfig-entity]

| Atribut | Typ |
| --- | --- |
| id |Řetězec |
| key |Řetězec |
| Typ_klíče. |Celé číslo 64 |
| Tabulka |Řetězec |
| hodnota |Řetězec |

### <a name="data-table"></a>Datová tabulka

**TodoItem**

| Atribut | Typ | Poznámka |
| --- | --- | --- |
| id | Řetězec, označen jako požadovaný |primární klíč v vzdáleného úložiště |
| Dokončení | Logická hodnota | Pole položky seznamu úkolů |
| Text |Řetězec |Pole položky seznamu úkolů |
| CreatedAt | Datum | (volitelné) Mapuje příliš**createdAt** vlastnost systému |
| updatedAt | Datum | (volitelné) Mapuje příliš**updatedAt** vlastnost systému |
| Verze | Řetězec | (volitelné) Použít toodetect konfliktů, tooversion mapy |

## <a name="setup-sync"></a>Změnit chování synchronizace hello aplikace hello
V této části upravíte hello aplikace tak, aby ho na spuštění aplikace nebo když vložení a aktualizace položky není synchronizovaná. Synchronizuje se jenom v případě, že se provádí tlačítko gesto aktualizovat hello.

**Jazyka Objective-C**:

1. V **QSTodoListViewController.m**, změňte hello **viewDidLoad** tooremove metoda hello volání příliš`[self refresh]` na konci hello hello metody. Teď hello dat není synchronizovaný s hello serveru při spuštění aplikace. Místo toho je synchronizovaný s hello obsah hello místní úložiště.
2. V **QSTodoService.m**, upravte definici hello `addItem` tak, aby se nebude synchronizovat po vložení hello položky. Odebrat hello `self syncData` blokovat a nahraďte ji metodou hello následující:

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. Upravit definici hello `completeItem` jak je uvedeno nahoře. Odeberte hello blok pro `self syncData` a nahraďte ji metodou hello následující:
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

**Kód SWIFT**:

V `viewDidLoad`v **ToDoTableViewController.swift**, komentář hello dva řádky zobrazeny zde, toostop synchronizuje se při spuštění aplikace. V době psaní tohoto textu hello aplikace Swift Todo hello hello služby při aktualizaci někdo přidá nebo dokončení položku. Aktualizuje hello služby pouze při spuštění aplikace.

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <a name="test-app"></a>Testování aplikace hello
V této části připojit tooan neplatná adresa URL toosimulate offline scénář. Když přidáte datových položek, jste uchovávat v hello místní základní datové úložiště, ale není synchronizovaná s back-end hello mobilní aplikace.

1. Změnit adresu URL mobilní aplikace hello v **QSTodoService.m** tooan neplatná adresa URL a znovu spusťte hello aplikaci:

   **Jazyka Objective-C**. V QSTodoService.m:
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   **Kód SWIFT**. V ToDoTableViewController.swift:
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. Přidejte některé položky seznamu úkolů. Ukončete simulátoru hello (nebo vynuceně zavřít hello aplikace) a pak ji znovu spusťte. Ověřte, že vaše změny se zachovají.

3. Zobrazit obsah hello hello vzdálené **TodoItem** tabulky:
   * Pro Node.js back-end, přejděte toohello [portál Azure](https://portal.azure.com/) a ve vaší mobilní aplikace back-end, klikněte na tlačítko **snadno tabulky** > **TodoItem**.  
   * Pro .NET zpět ukončení, použijte nástroj SQL, jako je například SQL Server Management Studio, nebo klienta REST, například aplikaci Fiddler nebo Postman.  

4. Ověřte, že nové položky hello *není* byly synchronizované s hello server.

5. Změna hello adresa URL back toohello opravte jednu v **QSTodoService.m**a znovu spusťte hello aplikace.

6. Proveďte aktualizaci gesto hello přidáváním dolů hello seznam položek.  
Zobrazí se průběh číselník.

7. Zobrazení hello **TodoItem** data znovu. nyní mají být zobrazeny Hello úkolů nové a změněné položky.

## <a name="summary"></a>Souhrn
Funkce offline synchronizace hello toosupport, použili jsme hello `MSSyncTable` rozhraní a inicializovat `MSClient.syncContext` s místním úložištěm. V takovém případě hello místní úložiště se databáze založené na datech jádra.

Pokud používáte místní úložiště dat jádra, je nutné zadat několik tabulek s hello [opravte vlastnosti systému](#review-core-data).

Normální Hello vytvářet, číst, aktualizovat a odstranit operace pro mobilní aplikace pracují, jako kdyby aplikace hello je stále připojená, ale všechny operace hello nastat proti hello místního úložiště.

Když jsme místního úložiště hello synchronizovány se serverem hello, použili jsme hello **MSSyncTable.pullWithQuery** metoda.

## <a name="additional-resources"></a>Další zdroje
* [Offline synchronizací dat v Mobile Apps]
* [Obálka cloudu: Offline synchronizace v Azure Mobile Services] \(je hello video o Mobile Services, ale Mobile Apps offline synchronizace funguje podobným způsobem.\)

<!-- URLs. -->


[vytvořit aplikaci pro iOS]: app-service-mobile-ios-get-started.md
[Offline synchronizací dat v Mobile Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Obálka cloudu: Offline synchronizace v Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
