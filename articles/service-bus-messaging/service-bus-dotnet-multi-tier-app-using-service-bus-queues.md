---
title: "aaa.NET vícevrstvé aplikace pomocí Azure Service Bus | Microsoft Docs"
description: "Kurz .NET, který vám pomůže vytvořit vícevrstvou aplikaci v Azure, která používá toocommunicate fronty Service Bus mezi vrstvami."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1b8608ca-aa5a-4700-b400-54d65b02615c
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: sethm
ms.openlocfilehash: 485910ff1d3b8b0a709ee14ede32e57cf873829a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>Vícevrstvá aplikace .NET, která používá fronty Azure Service Bus
## <a name="introduction"></a>Úvod
Vývoj pro Microsoft Azure je snadný při použití sady Visual Studio a hello bezplatné sady Azure SDK pro .NET. Tento kurz vás provede kroky toocreate hello aplikaci, která používá několik prostředků Azure běžících ve vašem místním prostředí.

Co se dozvíte hello následující:

* Jak tooenable počítače pro vývoj pro Azure s jedním stáhněte a nainstalujte.
* Jak toouse toodevelop Visual Studio pro Azure.
* Jak toocreate vícevrstvé aplikace v Azure pomocí webových a pracovních rolí.
* Jak toocommunicate mezi úrovně pomocí front Service Bus.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

V tomto kurzu budete sestavte a spusťte hello vícevrstvé aplikace v cloudové službě Azure. Webová role ASP.NET MVC je Hello front-endu a back-end hello role pracovního procesu, který používá fronty Service Bus. Můžete vytvořit stejnou vícevrstvou aplikaci s front-endu hello jako webový projekt, který je nasazený tooan webové stránky Azure namísto cloudové služby hello. Můžete také zkusit hello [hybridní lokální/Cloudová aplikace .NET](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) kurzu.

Hello následující snímek obrazovky ukazuje aplikace hello byla dokončena.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Přehled scénáře: komunikace mezi rolemi
toosubmit objednávku ke zpracování, hello front-end součást uživatelského rozhraní, spuštěné v hello webovou roli, musí spolupracovat s hello logikou střední úrovně běžící v roli pracovního procesu hello. Tento příklad používá pro hello komunikaci mezi vrstvami hello zasílání zpráv Service Bus.

Pomocí služby Service Bus zasílání zpráv mezi hello webem a prostředními úrovněmi odděluje obě části. Na rozdíl od toodirect zasílání zpráv (to znamená, TCP nebo HTTP), hello webová vrstva nepřipojuje střední vrstvy toohello přímo. Namísto toho odesílá pracovní jednotky jako zprávy do služby Service Bus, která spolehlivě uchová, dokud nebude prostřední vrstva hello je připraven tooconsume a jejich zpracování.

Service Bus poskytuje dvě entity toosupport zprostředkované zasílání zpráv na úrovni: fronty a témata. V případě front se každá zpráva odeslaná toohello fronty spotřebuje jednoho příjemce. Témata podporují hello publikování a přihlášení k odběru vzor ve kterém je každá publikovaná zpráva provedené dostupné tooa odběru registrovanému pro téma hello. Každý odběr logicky uchovává svoji vlastní frontu zpráv. Odběry můžete nakonfigurovat také pomocí pravidel filtrů, které omezují hello skupinu zpráv předávaných do toothose fronty hello předplatné, které odpovídají filtru hello. Hello následující příklad používá fronty Service Bus.

![][1]

Tento komunikační mechanizmus má několik výhod oproti přímému přenosu zpráv.

* **Časové oddělení**. S asynchronním vzorcem zasílání zpráv hello nemusí být producenti a spotřebitelé online na hello stejnou dobu. Service Bus spolehlivě uchová zprávy, dokud hello spotřebitel nebude připravený je přijmout. To umožňuje hello součástí hello distribuované aplikace toobe odpojit, například za účelem údržby nebo z důvodu tooa součást havárií, bez dopadu na systém jako celek. Kromě toho hello využívání aplikací potřebovat pouze toocome online v určitou dobu hello den.
* **Vyrovnávání zátěže**. V mnoha aplikacích zatížení systému se liší v čase, při zpracování hello čas potřebný pro jednotlivé jednotky práce je obvykle stálá. Propojovací producenti a spotřebitelé zpráv s frontou znamená, že tento hello využívání aplikací (pracovník hello) pouze potřebám toobe zřízený průměrnou zátěž tooaccommodate spíše než zátěž ve špičce. Hello hloubka fronty hello s měnící hello příchozí zátěží se mění. To znamená přímou úsporu nákladů z hlediska hello množství infrastruktury požadované tooservice hello aplikace zatížení.
* **Vyrovnávání zatížení**. Když se zátěž zvyšuje, další pracovní procesy přidáním tooread z fronty hello. Každou zprávu zpracovává jenom jeden hello pracovních procesů. Kromě toho tato Vyrovnávání zatížení založené na operaci pull umožňuje optimální využívání hello pracovních počítačů i v případě, že se pracovní počítače liší z hlediska výpočetní výkon, jak se bude načítat zprávy na svou vlastním maximální rychlostí. Toto chování se často říká hello *konkurence mezi spotřebiteli* vzor.
  
  ![][2]

Hello následující oddíly popisují hello kód, který tuto architekturu implementuje.

## <a name="set-up-hello-development-environment"></a>Nastavit hello vývojového prostředí
Před zahájením vývojem aplikací pro Azure, získání hello nástroje a nastavení vývojového prostředí.

1. Nainstalujte hello Azure SDK pro .NET ze hello SDK [položky ke stažení](https://azure.microsoft.com/downloads/).
2. V hello **.NET** sloupce, klikněte na tlačítko hello verzi [Visual Studio](http://www.visualstudio.com) používáte. Hello kroky v tomto kurzu Visual Studio 2015, ale také pracovat s Visual Studio 2017.
3. Po zobrazení výzvy toorun nebo uložení instalačního programu hello, klikněte na tlačítko **spustit**.
4. V hello **instalačního programu webové platformy**, klikněte na tlačítko **nainstalovat** a pokračovat v instalaci hello.
5. Po dokončení instalace hello budete mít všechno potřebné toostart toodevelop hello aplikace. Hello SDK obsahuje nástroje, které vám umožní snadno vyvíjet aplikace Azure v sadě Visual Studio.

## <a name="create-a-namespace"></a>Vytvoření oboru názvů
dalším krokem Hello je toocreate oboru názvů služby a získat klíč sdíleného přístupového podpisu (SAS). Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus. Hello systém vygeneruje SAS klíč při vytvoření oboru názvů. Hello kombinace oboru názvů a klíče SAS poskytuje pověření hello pro Service Bus tooauthenticate přístup tooan aplikaci.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Vytvoření webové role
V této části vytvoříte front-end hello vaší aplikace. Nejprve je třeba vytvořit hello stránky, které vaše aplikace zobrazí.
Potom přidáte kód, který odesílá položky fronty Service Bus tooa a zobrazí informace o frontě hello stavu.

### <a name="create-hello-project"></a>Vytvoření projektu hello
1. Pomocí oprávnění správce, spuštění sady Visual Studio: klikněte pravým tlačítkem na hello **Visual Studio** ikonu programu a potom klikněte na **spustit jako správce**. Hello emulátoru služby výpočty Azure, probírat později v tomto článku, vyžaduje, aby Visual Studio spuštěné s oprávněními správce.
   
   V sadě Visual Studio na hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.
2. V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Cloud** a pak na **Cloudová služba Azure**. Název projektu hello **MultiTierApp**. Pak klikněte na **OK**.
   
   ![][9]
3. Z rolí **.NET Framework 4.5** poklikáním vyberte **Webovou roli ASP.NET**.
   
   ![][10]
4. Pozastavte ukazatel myši nad **WebRole1** pod **řešení cloudové služby Azure**, klikněte na ikonu tužky hello a přejmenujte webovou roli hello příliš**FrontendWebRole**. Pak klikněte na **OK**. (Ujistěte se, že „Frontend“ zadáte s malým „e“ tzn. nikoli „FrontEnd“.)
   
   ![][11]
5. Z hello **nový projekt ASP.NET** dialogové okno, v hello **vyberte šablonu** seznamu, klikněte na tlačítko **MVC**.
   
   ![][12]
6. Stále v hello **nový projekt ASP.NET** dialogovém okně klikněte na hello **změna ověřování** tlačítko. V hello **změna ověřování** dialogové okno, klikněte na tlačítko **bez ověřování**a potom klikněte na **OK**. V tomto kurzu nasazujete aplikaci, která nepotřebuje přihlášení uživatele.
   
    ![][16]
7. Zpět v hello **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **OK** toocreate hello projektu.
8. V **Průzkumníku řešení**, v hello **FrontendWebRole** projektu, klikněte pravým tlačítkem na **odkazy**, pak klikněte na tlačítko **spravovat balíčky NuGet**.
9. Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Vyberte hello **WindowsAzure.ServiceBus** balíček, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.
   
   ![][13]
   
   Všimněte si, že hello vyžaduje teď jsou reference na sestavení klienta a přidané některé nové soubory s kódem.
10. V **Průzkumníku řešení** klikněte pravým tlačítkem na **Modely**, pak klikněte na **Přidat** a pak na **Třída**. V hello **název** pole, název typu hello **OnlineOrder.cs**. Pak klikněte na **Přidat**.

### <a name="write-hello-code-for-your-web-role"></a>Psaní kódu hello pro vaši webovou roli
V této části vytvoříte hello stránky, které vaše aplikace zobrazí.

1. V souboru OnlineOrder.cs hello v sadě Visual Studio nahraďte existující definici oboru názvů hello následující kód:
   
   ```csharp
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. V **Průzkumníku řešení** poklikejte na **Controllers\HomeController.cs**. Přidejte následující hello **pomocí** příkazy hello horní části hello souboru tooinclude hello obory názvů pro model, který jste právě vytvořili, zvolte a pro Service Bus.
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. Také v souboru HomeController.cs hello v sadě Visual Studio, nahraďte existující definici oboru názvů hello následující kód. Tento kód obsahuje metody pro zpracování odesílání hello položky toohello fronty.
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect tooSubmit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for hello submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from hello submission
           // form.
           [HttpPost]
           // Attribute toohelp prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting tooqueue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** tootest hello přesnost své dosavadní práce.
5. Teď vytvořte zobrazení hello hello `Submit()` metoda jste vytvořili dříve. Klikněte pravým tlačítkem do hello `Submit()` – metoda (hello přetížení `Submit()` které nepřijímá žádné parametry) a potom zvolte **přidat zobrazení**.
   
   ![][14]
6. Zobrazí se dialogové okno pro vytvoření zobrazení hello. V hello **šablony** vyberte **vytvořit**. V hello **třída modelu** seznamu, klikněte na tlačítko hello **OnlineOrder** třídy.
   
   ![][15]
7. Klikněte na tlačítko **Přidat**.
8. Teď změňte hello zobrazí název vaší aplikace. V **Průzkumníku řešení**, dvakrát klikněte **Views\Shared\\_Layout.cshtml** souboru tooopen ji v editoru Visual Studio hello.
9. Všechny výskyty **My ASP.NET Application** změňte na **LITWARE'S Products**.
10. Odebrat hello **Domů**, **o**, a **kontaktujte** odkazy. Odstraňte hello zvýrazněná kód:
    
    ![][28]
11. Nakonec upravte hello odeslání stránky tooinclude některé informace o frontě hello. V **Průzkumníku řešení**, dvakrát klikněte **Views\Home\Submit.cshtml** souboru tooopen ji v editoru Visual Studio hello. Přidejte následující řádek po hello `<h2>Submit</h2>`. Prozatím se hello `ViewBag.MessageCount` je prázdný. Zaplníte ji později.
    
    ```html
    <p>Current number of orders in queue waiting toobe processed: @ViewBag.MessageCount</p>
    ```
12. Teď je implementované vaše uživatelské prostředí. Stisknutím klávesy **F5** toorun aplikaci a potvrďte, že vypadá tak podle očekávání.
    
    ![][17]

### <a name="write-hello-code-for-submitting-items-tooa-service-bus-queue"></a>Zápis kódu hello pro odesílání položek fronty Service Bus tooa
Teď přidáte kód pro odesílání položek tooa fronty. Nejdřív vytvořte třídu, která obsahuje informace o připojení k vaší frontě Service Bus. Potom inicializujte připojení ze souboru Global.aspx.cs. Nakonec aktualizujte kód odeslání hello, kterou jste vytvořili dříve v HomeController.cs tooactually odeslání položky tooa fronty Service Bus.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **FrontendWebRole** (klikněte pravým tlačítkem na projekt hello, ne hello roli). Klikněte na **Přidat** a potom na **Třída**.
2. Název třídy hello **QueueConnector.cs**. Klikněte na tlačítko **přidat** toocreate hello třídy.
3. Nyní přidáte kód, který zapouzdřuje informace o připojení hello a inicializuje fronty Service Bus tooa hello připojení. Hello celý obsah souboru QueueConnector.cs nahraďte hello následující kód a zadejte hodnoty pro `your Service Bus namespace` (název vašeho oboru názvů) a `yourKey`, což je hello **primární klíč** jste předtím získali z hello Azure portál.
   
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   
   namespace FrontendWebRole
   {
       public static class QueueConnector
       {
           // Thread-safe. Recommended that you cache rather than recreating it
           // on every request.
           public static QueueClient OrdersQueueClient;
   
           // Obtain these values from hello portal.
           public const string Namespace = "your Service Bus namespace";
   
           // hello name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create hello namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http toobe friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create hello namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create hello queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client toohello queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. Teď musíte zajistit, aby se vaše metoda **Initialize** volala. V **Průzkumníku řešení** poklikejte na **Global.asax\Global.asax.cs**.
5. Přidejte následující řádek kódu na konci hello hello hello **Application_Start** metoda.
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. Nakonec aktualizujte kód webového hello jste vytvořili dříve, k odeslání položky toohello fronty. V **Průzkumníku řešení** poklikejte na **Controllers\HomeController.cs**.
7. Aktualizace hello `Submit()` – metoda (hello přetížení, které nepřijímá žádné parametry) následujícím způsobem tooget uvítací zprávu počet pro frontu hello.
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you tooperform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get hello queue, and obtain hello message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. Aktualizace hello `Submit(OnlineOrder order)` – metoda (hello přetížení, které přijímá jeden parametr) následujícím způsobem toosubmit pořadí informace toohello fronty.
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from hello order.
           var message = new BrokeredMessage(order);
   
           // Submit hello order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. Teď můžete spustit hello aplikaci znovu. Pokaždé, když odešlete odbejdnávku, počet zpráv hello se zvýší.
   
   ![][18]

## <a name="create-hello-worker-role"></a>Vytvoření role pracovního procesu hello
Teď vytvoříte roli pracovního procesu hello, který zpracovává hello objednávek. Tento příklad používá hello **Role pracovního procesu s frontou Service Bus** šablonu Visual Studia. Jste už získali z portálu hello hello vyžaduje přihlašovací údaje.

1. Ujistěte se, že jste se připojili Visual Studio tooyour účet Azure.
2. V sadě Visual Studio v **Průzkumníku řešení** klikněte pravým tlačítkem myši **role** ve složce hello **MultiTierApp** projektu.
3. Klikněte na **Přidat**, a pak klikněte na **Nový projekt role pracovního procesu**. Hello **přidat nový projekt Role** zobrazí se dialogové okno.
   
   ![][26]
4. V hello **přidat nový projekt Role** dialogové okno, klikněte na tlačítko **Role pracovního procesu s frontou Service Bus**.
   
   ![][23]
5. V hello **název** pole, název projektu hello **OrderProcessingRole**. Pak klikněte na **Přidat**.
6. Zkopírujte hello připojovací řetězec, který jste získali v kroku 9 schránky toohello části "Vytvoření oboru názvů Service Bus" hello.
7. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **OrderProcessingRole** jste vytvořili v kroku 5 (ujistěte se, že kliknete pravým tlačítkem na **OrderProcessingRole** pod **Role**, a není hello třídy). Potom klikněte na **Vlastnosti**.
8. Na hello **nastavení** kartě hello **vlastnosti** dialogové okno, klikněte do hello **hodnotu** pole pro **Microsoft.ServiceBus.ConnectionString**a potom vložte hodnotu koncového bodu hello jste zkopírovali v kroku 6.
   
   ![][25]
9. Vytvoření **OnlineOrder** třídy toorepresent hello objednávky jako při jejich zpracování z fronty hello. Můžete znovu použít třídu, kterou jste už vytvořili. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **OrderProcessingRole** – třída (klikněte pravým tlačítkem na ikonu třídy hello, ne hello roli). Klikněte na **Přidat**, pak klikněte na **Existující položka**.
10. Procházet toohello podsložky pro **FrontendWebRole\Models**a potom dvakrát klikněte na **OnlineOrder.cs** tooadd ho toothis projektu.
11. V **WorkerRole.cs**, změnit hodnotu hello hello **QueueName** proměnnou z `"ProcessingQueue"` příliš`"OrdersQueue"` jak je znázorněno v následujícím kódu hello.
    
    ```csharp
    // hello name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. Přidejte následující hello pomocí příkazu hello horní části souboru WorkerRole.cs hello.
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. V hello `Run()` funkce uvnitř hello `OnMessage()` volání, nahraďte obsah hello hello `try` klauzuli with hello následující kód.
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View hello message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. Dokončili jste aplikaci hello. Hello celou aplikaci můžete otestovat kliknutím pravým tlačítkem na projekt MultiTierApp hello v Průzkumníku řešení, výběr **nastavit jako spouštěný projekt**a pak stisknete F5. Všimněte si, že počet zpráv se nezvyšuje, protože hello role pracovního procesu zpracovává položky z fronty hello a označí je jako dokončené. Zobrazí se výstup trasování hello své role pracovního procesu zobrazením hello uživatelské prostředí emulátoru výpočtů Azure. To provedete kliknutím pravým tlačítkem myši na ikonu emulátoru hello v hello oznamovací oblasti hlavního panelu a výběrem **zobrazit uživatelské prostředí emulátoru výpočtů**.
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a>Další kroky
toolearn víc o službě Service Bus, najdete v části hello následující prostředky:  

* [Dokumentace ke službě Azure Service Bus][sbdocs]  
* [Stránka služby Service Bus][sbacom]  
* [Jak tooUse fronty služby Service Bus][sbacomqhowto]  

toolearn Další informace o víceúrovňových scénářích najdete v části:  

* [Vícevrstvá aplikace .NET, která používá tabulky, fronty a objekty blob služby Storage][mutitierstorage]  

[0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
[2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
[9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
[10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
[11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
[12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
[13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
[14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
[15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
[16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
[17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app2.png

[19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
[20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
[23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
[25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
[26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
[28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

[sbdocs]: /azure/service-bus-messaging/  
[sbacom]: https://azure.microsoft.com/services/service-bus/  
[sbacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
