---
title: "Kurz k ASP.NET MVC pro službu Azure Cosmos DB: Vývoj webové aplikace | Dokumentace Microsoftu"
description: "ASP.NET MVC kurz toocreate webová aplikace MVC pomocí Azure Cosmos DB. Budete ukládat JSON a přístupová data z aplikace seznamu úkolů hostované na Webech Azure – podrobný kurz ASP.NET MVC."
keywords: "kurz asp.net mvc, vývoj webových aplikací, aplikace mvc web, kurz asp net mvc krok za krokem"
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 52532d89-a40e-4fdf-9b38-aadb3a4cccbc
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: mimig
ms.openlocfilehash: dac2a9599b395524533e6fe14983789ff095331f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="_Toc395809351"></a>Kurz k ASP.NET MVC: Vývoj webové aplikace s použitím služby Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

toohighlight způsobu můžete efektivně využívat Azure Cosmos DB toostore a dotazování dokumentů JSON, tento článek obsahuje začátku do konce návod ukazuje, jak toobuild todo aplikace pomocí Azure Cosmos DB. Hello úkoly se budou ukládat jako dokumenty JSON v Azure Cosmos DB.

![Snímek obrazovky seznamu úkolů hello webové aplikace MVC vytvořené v tomto kurzu – podrobný kurz Asp.net MVC krok za krokem](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image01.png)

Tento návod ukazuje, jak toouse hello data toostore a přístup služby Azure Cosmos DB z webové aplikace ASP.NET MVC hostované v Azure. Pokud hledáte kurz, který se zaměřuje jenom na databázi Cosmos Azure a není hello komponenty ASP.NET MVC najdete v části [sestavit aplikaci konzoly Azure Cosmos DB jazyka C#](documentdb-get-started.md).

> [!TIP]
> V tomto kurzu se předpokládá, že již máte zkušenosti s používáním ASP.NET MVC a Webů Azure. Pokud jste nový tooASP.NET nebo hello [požadovaných nástrojů](#_Toc395637760), doporučujeme stáhnout úplný ukázkový projekt hello z [Githubu] [ GitHub] a postupujte podle pokynů hello Tato ukázka. Po jejím vytvořené, můžete zkontrolovat Tento článek toogain porozuměli hello kódu v kontextu hello hello projektu.
> 
> 

## <a name="_Toc395637760"></a>Předpoklady pro tento databázový kurz
Před následující hello pokyny v tomto článku, se ujistěte, že máte hello následující:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 

    NEBO

    Místní instalace hello [emulátoru DB Cosmos Azure](local-emulator.md).
* [Visual Studio 2017](http://www.visualstudio.com/).  
* Microsoft Azure SDK pro .NET pro Visual Studio 2017, k dispozici prostřednictvím hello instalační program Visual Studio.

Všechny snímky obrazovky hello v tomto článku byly pořízeny pomocí nástroje Microsoft Visual Studio Community 2017. Pokud je systém konfigurován s jinou verzí je možné, že vaše obrazovky a možnosti budou mírně lišit, ale pokud splníte hello výše požadavky tohoto řešení by mělo fungovat.

## <a name="_Toc395637761"></a>Krok 1: Vytvoření účtu databáze Azure Cosmos DB
Začněme vytvořením účtu služby Azure Cosmos DB. Pokud už máte účet SQL (DocumentDB) pro Azure Cosmos DB nebo pokud používáte hello emulátoru DB Cosmos Azure pro účely tohoto kurzu, můžete přeskočit příliš[vytvoření nové aplikace ASP.NET MVC](#_Toc395637762).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
Nyní vám ukážeme jak toocreate nové aplikace ASP.NET MVC z hello základů. 

## <a name="_Toc395637762"></a>Krok 2: Vytvoření nové aplikace ASP.NET MVC

1. V sadě Visual Studio na hello **soubor** nabídce bodu příliš**nový**a potom klikněte na **projektu**. Hello **nový projekt** zobrazí se dialogové okno.

2. V hello **typy projektů** podokně rozbalte **šablony**, **Visual C#**, **webové**a potom vyberte **webové aplikace ASP.NET** .

      ![Snímek obrazovky znázorňující dialogové okno Nový projekt hello se zvýrazněným typem projektu webová aplikace ASP.NET hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. V hello **název** pole, název typu hello hello projektu. Tento kurz používá název hello "úkolů". Pokud si zvolíte toouse něco jiného, bez ohledu na tomto kurzu bude zmíněn obor názvů todo hello, budete potřebovat tooadjust hello zadat kód ukázky toouse vámi zvolený název aplikace. 
4. Klikněte na tlačítko **Procházet** toonavigate toohello složku, kde jako toocreate hello projekt a potom klikněte na **OK**.
   
      Hello **nové webové aplikace ASP.NET** zobrazí se dialogové okno.
   
    ![Snímek obrazovky dialogového okna novou webovou aplikaci ASP.NET hello se zvýrazněnou šablonou aplikace MVC hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. V podokně hello šablony, vyberte **MVC**.

6. Klikněte na tlačítko **OK** a nechejte Visual Studio odvést svou práci generování uživatelského rozhraní hello prázdné šablony ASP.NET MVC. 

          
7. Až Visual Studio dokončí vytváření aplikace MVC standardní hello budete mít prázdnou aplikaci ASP.NET, který můžete spustit místně.
   
    Lokální spuštěné projektu hello místně, protože určitě jsme všechny zaznamenané hello ASP.NET "Hello, World" aplikace. Přejděme rovnou tooadding Azure Cosmos DB toothis projektu a sestavení aplikace.

## <a name="_Toc395637767"></a>Krok 3: Přidání projektu webové aplikace v Azure Cosmos DB tooyour MVC
Teď, když máme většinu vložení hello ASP.NET MVC, které potřebujeme pro toto řešení, Pojďme toohello skutečnému účelu tohoto kurzu, přidání webové aplikace MVC tooour Azure Cosmos DB.

1. Hello .NET SDK služby Azure Cosmos DB je zabalené a distribuovat jako balíčku NuGet. tooget hello balíček NuGet v sadě Visual Studio, použijte hello Správce balíčků NuGet v sadě Visual Studio, kliknutím pravým tlačítkem na projekt hello v **Průzkumníku řešení** a pak levým na **spravovat balíčky NuGet**.
   
    ![Snímek obrazovky hello klikněte pravým tlačítkem na možnosti pro projekt webové aplikace v Průzkumníkovi řešení se spravovat balíčky NuGet zvýrazněná hello.](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    Hello **spravovat balíčky NuGet** zobrazí se dialogové okno.
2. V hello NuGet **Procházet** zadejte ***Azure DocumentDB***. (název balíčku hello nebyla aktualizované tooAzure Cosmos DB.)
   
    Z výsledků hello nainstalovat hello **Microsoft.Azure.DocumentDB Microsoft** balíčku. Tím stáhnout a nainstalovat balíček Azure Cosmos DB hello a také všechny závislosti, jako je například Newtonsoft.Json. Klikněte na tlačítko **OK** v hello **Preview** okně a **souhlasím** v hello **přijetí licence** okno toocomplete hello instalace.
   
    ![Snímek obrazovky okna Správa balíčků NuGet hello, s hello Microsoft Azure DocumentDB Client Library zvýrazněná](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      Případně můžete také použít hello balíček hello tooinstall Konzola správce balíčků. toodo Ano, na hello **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet**a potom klikněte na **Konzola správce balíčků**. Hello řádku zadejte následující hello.
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. Po instalaci balíčku hello řešení sady Visual Studio by měla vypadat přibližně následující hello se dvěma přidanými referencemi, Microsoft.Azure.Documents.Client a Newtonsoft.Json.
   
    ![Snímek obrazovky hello dva odkazy přidat datový projekt JSON toohello v Průzkumníku řešení](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <a name="_Toc395637763"></a>Krok 4: Nastavení hello aplikace ASP.NET MVC
Nyní Pojďme přidat aplikace MVC toothis hello pro modely, zobrazení a kontrolery:

* [Přidání modelu](#_Toc395637764)
* [Přidání kontroleru](#_Toc395637765)
* [Přidání zobrazení](#_Toc395637766)

### <a name="_Toc395637764"></a>Přidání datového modelu JSON
Začněme vytvořením hello **M** v MVC, hello modelu. 

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **modely** složku, klikněte na tlačítko **přidat**a potom klikněte na **– třída**.
   
      Hello **přidat novou položku** zobrazí se dialogové okno.
2. Pojmenujte svou novou třídu **Item.cs** a klikněte na **Přidat**. 
3. Do tohoto nového **Item.cs** soubor, přidejte následující hello po hello poslední *pomocí příkazu*.
   
        using Newtonsoft.Json;
4. Nyní nahraďte tento kód 
   
        public class Item
        {
        }
   
    s hello následující kód.
   
        public class Item
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
   
            [JsonProperty(PropertyName = "name")]
            public string Name { get; set; }
   
            [JsonProperty(PropertyName = "description")]
            public string Description { get; set; }
   
            [JsonProperty(PropertyName = "isComplete")]
            public bool Completed { get; set; }
        }
   
    Všechna data v Azure Cosmos DB je předán přes přenosu hello a uloží jako dokumenty JSON. způsob hello toocontrol jsou vaše objekty serializují a deserializují technologií JSON.NET, můžete použít hello **JsonProperty** atributu, jak je předvedeno v hello **položky** kterou jsme právě vytvořili. Ne **mít** toodo to ale chcete tooensure, že naše vlastnosti dodržují hello JSON camelCase konvence vytváření názvů. 
   
    Ne jenom můžete určovat formát názvu vlastnosti hello hello když přejde do formátu JSON, ale můžete zcela přejmenovat vlastnosti .NET, jako jsme to udělali s hello **popis** vlastnost. 

### <a name="_Toc395637765"></a>Přidání kontroleru
Který má na starosti hello **M**, teď Vytvořme hello **C** v MVC, třídy kontroleru.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **řadiče** složku, klikněte na tlačítko **přidat**a potom klikněte na **řadiče**.
   
    Hello **přidat vygenerované uživatelské rozhraní** zobrazí se dialogové okno.
2. Vyberte **Kontroler MVC 5 – prázdný** a klikněte na **Přidat**.
   
    ![Snímek obrazovky znázorňující dialogové okno Přidat vygenerované uživatelské rozhraní hello s hello kontroler MVC 5 – prázdný zvýrazněnou možností](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. Pojmenujte nový kontroler, **ItemController**.
   
    ![Snímek obrazovky dialogového okna Přidat kontroler hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    Po vytvoření souboru hello řešení sady Visual Studio by měla vypadat přibližně následující hello s hello novým souborem ItemController.cs v **Průzkumníku řešení**. také se zobrazí nový soubor Item.cs Hello vytvořený dříve.
   
    ![Snímek obrazovky znázorňující hello řešení Visual Studio – Průzkumník řešení se hello novým souborem ItemController.cs a Item.cs](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    Soubor ItemController.cs můžete zavřít, vrátíme tooit později. 

### <a name="_Toc395637766"></a>Přidání zobrazení
Nyní vytvoříme hello **V** v MVC, hello zobrazení:

* [Přidání zobrazení Index položky](#AddItemIndexView)
* [Přidání zobrazení Nová položka](#AddNewIndexView)
* [Přidání zobrazení Upravit položku](#_Toc395888515)

#### <a name="AddItemIndexView"></a>Přidání zobrazení Index položky
1. V **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na hello prázdný **položky** složce vytvořené sadě Visual Studio pro vás při přidání hello  **ItemController** dříve, klikněte na tlačítko **přidat**a potom klikněte na **zobrazení**.
   
    ![Snímek obrazovky Průzkumník řešení zobrazující hello položky složky, vytvořené v sadě Visual Studio se zvýrazněnými příkazy přidat zobrazení hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. V hello **přidat zobrazení** dialogové okno pole, hello následující:
   
   * V hello **název zobrazení** zadejte ***Index***.
   * V hello **šablony** vyberte ***seznamu***.
   * V hello **třída modelu** vyberte ***položky (todo. Modely)***.
   * Zadejte pole stránky rozložení hello ***~/Views/Shared/_Layout.cshtml***.
     
   ![Dialogové okno hello přidat zobrazení snímek obrazovky zobrazující](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. Jakmile jsou všechny tyto hodnoty nastaveny, klikněte na **Přidat** a nechejte Visual Studio vytvořit novou šablonu zobrazení. Až se to dokončí, otevře se hello soubor cshtml, který byl vytvořen. Tento soubor v sadě Visual Studio jsme můžete zavřít, protože jsme se vraťte tooit později.

#### <a name="AddNewIndexView"></a>Přidání zobrazení Nová položka
Podobně jako toohow jsme vytvořili **Index položky** zobrazení, nyní vytvoříme nové zobrazení pro vytváření nových **položky**.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **položky** znovu, klikněte na složku **přidat**a potom klikněte na **zobrazení**.
2. V hello **přidat zobrazení** dialogové okno pole, hello následující:
   
   * V hello **název zobrazení** zadejte ***vytvořit***.
   * V hello **šablony** vyberte ***vytvořit***.
   * V hello **třída modelu** vyberte ***položky (todo. Modely)***.
   * Zadejte pole stránky rozložení hello ***~/Views/Shared/_Layout.cshtml***.
   * Klikněte na tlačítko **Přidat**.
   
#### <a name="_Toc395888515"></a>Přidání zobrazení Upravit položku
A nakonec přidejte jedno poslední zobrazení pro úpravy **položky** v hello stejným způsobem jako předtím.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **položky** znovu, klikněte na složku **přidat**a potom klikněte na **zobrazení**.
2. V hello **přidat zobrazení** dialogové okno pole, hello následující:
   
   * V hello **název zobrazení** zadejte ***upravit***.
   * V hello **šablony** vyberte ***upravit***.
   * V hello **třída modelu** vyberte ***položky (todo. Modely)***.
   * Zadejte pole stránky rozložení hello ***~/Views/Shared/_Layout.cshtml***.
   * Klikněte na tlačítko **Přidat**.

Až bude vše Hotovo, zavřete všechny dokumenty cshtml hello v sadě Visual Studio, jako jsme vrátí toothese zobrazení později.

## <a name="_Toc395637769"></a>Krok 5: Připojení služby Azure Cosmos DB
Teď, když hello standardní MVC věcem se stará, můžeme zaměřit tooadding hello kód pro Azure Cosmos DB. 

V této části přidáme kód toohandle hello následující:

* [Výpis neúplných položek](#_Toc395637770)
* [Přidávání položek](#_Toc395637771)
* [Úprava položek](#_Toc395637772)

### <a name="_Toc395637770"></a>Výpis neúplných položek ve webové aplikaci MVC
Hello první věc toodo tady je přidat třídu, která obsahuje všechny hello logiku tooconnect tooand použití Azure Cosmos DB. Pro účely tohoto kurzu zapouzdříme všechnu tuto logiku tooa třídy úložiště nazvané DocumentDBRepository. 

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello, klikněte na **přidat**a potom klikněte na **třída**. Pojmenujte novou třídu hello **DocumentDBRepository** a klikněte na tlačítko **přidat**.
2. V nově vytvořený hello **DocumentDBRepository** třídu a přidejte následující hello *pomocí příkazů* výše hello *obor názvů* deklarace
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net
        
    Nyní nahraďte tento kód 
   
        public class DocumentDBRepository
        {
        }
   
    s hello následující kód.
   
        public static class DocumentDBRepository<T> where T : class
        {
            private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
            private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
            private static DocumentClient client;
   
            public static void Initialize()
            {
                client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
                CreateDatabaseIfNotExistsAsync().Wait();
                CreateCollectionIfNotExistsAsync().Wait();
            }
   
            private static async Task CreateDatabaseIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(DatabaseId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
   
            private static async Task CreateCollectionIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDocumentCollectionAsync(
                            UriFactory.CreateDatabaseUri(DatabaseId),
                            new DocumentCollection { Id = CollectionId },
                            new RequestOptions { OfferThroughput = 1000 });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
        }
   
    
3. Čteme několik hodnot z konfigurace, takže otevřete hello **Web.config** vaší aplikace a přidejte následující řádky pod hello hello `<AppSettings>` části.
   
        <add key="endpoint" value="enter hello URI from hello Keys blade of hello Azure Portal"/>
        <add key="authKey" value="enter hello PRIMARY KEY, or hello SECONDARY KEY, from hello Keys blade of hello Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. Nyní, aktualizovat hello hodnoty pro *koncový bod* a *authKey* pomocí okna klíče hello hello portálu Azure. Použít hello **URI** z okna klíče hello jako hodnota hello hello koncový bod nastavení a použití hello **primární klíč**, nebo **sekundární klíč** z okna klíče hello jako hodnota hello hello nastavení authKey.

    Vyřešeno připojení do úložiště Azure Cosmos DB hello, nyní Pojďme přidat logiku aplikace.

1. Hello nejprve thing Chceme mít toodo toobe s aplikaci seznamu úkolů je toodisplay hello neúplné položky.  Zkopírujte a vložte následující fragment kódu kamkoli v hello hello **DocumentDBRepository** třídy.
   
        public static async Task<IEnumerable<T>> GetItemsAsync(Expression<Func<T, bool>> predicate)
        {
            IDocumentQuery<T> query = client.CreateDocumentQuery<T>(
                UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId))
                .Where(predicate)
                .AsDocumentQuery();
   
            List<T> results = new List<T>();
            while (query.HasMoreResults)
            {
                results.AddRange(await query.ExecuteNextAsync<T>());
            }
   
            return results;
        }
2. Otevřete hello **ItemController** jsme přidali dříve a přidejte následující hello *pomocí příkazů* nad deklaraci oboru názvů hello.
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    Pokud se váš projekt nejmenuje "todo", budete potřebovat tooupdate pomocí "úkolů. Modely"; tooreflect hello název projektu.
   
    Nyní nahraďte tento kód
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    s hello následující kód.
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. Otevřete **Global.asax.cs** a přidejte následující řádek toohello hello **Application_Start** – metoda 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

Řešení v tomto okamžiku by měla být schopný toobuild bez chyb.

Pokud jste spustili hello aplikace hned, by se dostala toohello **HomeController** a hello **Index** tohoto kontroleru. Toto je výchozí chování projektu šablony MVC hello, který jsme zvolili při spuštění hello hello ale nechceme! Nyní změníme hello směrování na tento tooalter aplikace MVC toto chování.

Otevřete ***aplikace\_Start\RouteConfig.cs*** a vyhledejte řádek hello začíná "výchozí hodnoty:" a změňte ji tooresemble hello následující.

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

Tímto bude aplikaci ASP.NET MVC, pokud jste nezadali hodnotu v hello URL toocontrol hello směrování chování místo oznámeno **Domů**, použijte **položky** jako řadič hello a uživatel **Index** jako hello zobrazení.

Teď Pokud hello aplikace spustíte, zavolá do vašeho **ItemController** které zavolá v třídě úložiště toohello a použít všechny neúplné položky toohello hello tooreturn metodu getitems, pomocí hello **zobrazení** \\ **Položky**\\**Index** zobrazení. 

Pokud teď projekt sestavíte a spustíte, mělo by se zobrazit něco přibližně takového.    

![Snímek obrazovky hello webové aplikace vytvořené v tomto kurzu databáze](./media/documentdb-dotnet-application/build-and-run-the-project-now.png)

### <a name="_Toc395637771"></a>Přidávání položek
Umožňuje uvést některé položky do databáze hotový, abychom měli něco víc než prázdnou mřížkou toolook v.

Přidejme nějaký kód příliš Azure Cosmos DBRepository a ItemController toopersist hello záznamu v Azure Cosmos DB.

1. Přidejte následující metodu tooyour hello **DocumentDBRepository** třídy.
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   Tato metoda jednoduše přijme předaný tooit objekt a zachová jej v Azure Cosmos DB.
2. Otevřete soubor ItemController.cs hello a přidejte následující fragment kódu v rámci třídy hello hello. Jedná se jak ASP.NET MVC ví, co toodo pro hello **vytvořit** akce. V takovém případě jenom vykreslení hello přidružené zobrazení Create.cshtml vytvořené dříve.
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    Nyní potřebujeme některé další kód v tomto kontroleru, který bude přijímat odeslání hello z hello **vytvořit** zobrazení.
3. Přidejte hello další blok kódu toohello třídy ItemController.cs, který technologii ASP.NET MVC řekne, co toodo s operací POST formuláře pro tento kontroler.
   
        [HttpPost]
        [ActionName("Create")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> CreateAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
    Tento kód zavolá toohello DocumentDBRepository a použije hello CreateItemAsync metoda toopersist hello nové todo položky toohello databáze. 
   
    **Poznámka k zabezpečení**: hello **ValidateAntiForgeryToken** je použit atribut zde toohelp zabezpečit tuto aplikace před útoky na padělání požadavku posílaného mezi weby. Existuje více tooit jen přidat tento atribut, vaše zobrazení musí toowork s tímto tokenem proti padělání také. Další informace o subjektu hello a příklady, jak tooimplement to správně, najdete v tématu [brání padělání požadavku webů][Preventing Cross-Site Request Forgery]. Hello zdrojový kód dostupný na [Githubu] [ GitHub] má hello úplnou implementaci na místě.
   
    **Poznámka k zabezpečení**: hello používáme **vazby** atribut hello metoda parametr toohelp chránit před útoky typu overpost. Další informace najdete v tématu [Základní operace CRUD v ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].

Tím končí hello vyžaduje kód tooadd nové položky tooour databáze.

### <a name="_Toc395637772"></a>Úprava položek
Je pro nás jednu věc toodo a který je tooadd hello možnost tooedit **položky** v databázi hello a toomark je jako dokončit. Hello zobrazení pro úpravy již byl přidán toohello projektu, takže potřebujeme tooadd některé kódu tooour řadiče a toohello **DocumentDBRepository** třídy znovu.

1. Přidejte následující toohello hello **DocumentDBRepository** třídy.
   
        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            return await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id), item);
        }
   
        public static async Task<T> GetItemAsync(string id)
        {
            try
            {
                Document document = await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id));
                return (T)(dynamic)document;
            }
            catch (DocumentClientException e)
            {
                if (e.StatusCode == HttpStatusCode.NotFound)
                {
                    return null;
                }
                else
                {
                    throw;
                }
            }
        }
   
    první z těchto metod Hello **GetItem** načítá položku z databáze Cosmos Azure, která se předá zpět toohello **ItemController** a pak na toohello **upravit** zobrazení.
   
    Hello sekundu metod hello jsme právě přidali nahradí hello **dokumentu** v Azure DB Cosmos hello verzi hello **dokumentu** předané z hello **ItemController**.
2. Přidejte následující toohello hello **ItemController** třídy.
   
        [HttpPost]
        [ActionName("Edit")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> EditAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
        [ActionName("Edit")]
        public async Task<ActionResult> EditAsync(string id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
   
            Item item = await DocumentDBRepository<Item>.GetItemAsync(id);
            if (item == null)
            {
                return HttpNotFound();
            }
   
            return View(item);
        }
   
    Hello první metoda zpracovává hello GET protokolu Http, který se stane, když hello uživatel klikne na hello **upravit** odkaz z hello **Index** zobrazení. Tato metoda načítá [ **dokumentu** ](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) z Azure Cosmos databáze a předává je toohello **upravit** zobrazení.
   
    Hello **upravit** zobrazení pak provede Http POST toohello **Indexcontrolleru**. 
   
    druhý metoda Hello jsme přidali, zpracovává předávání hello aktualizovat objekt tooAzure Cosmos DB toobe jako trvalý v databázi hello.

Je to, který je všechno, co potřebujeme toorun naše aplikace, vypsání neúplné **položky**, přidat nové **položky**a upravte **položky**.

## <a name="_Toc395637773"></a>Krok 6: Místní spuštění aplikace hello
aplikace hello tootest na místním počítači, hello následující:

1. Stiskněte F5 v sadě Visual Studio toobuild hello aplikace v režimu ladění. Měl by sestavení aplikace hello a spustí prohlížeč s stránku hello prázdnou mřížkou, kterou jsme viděli dříve:
   
    ![Snímek obrazovky hello webové aplikace vytvořené v tomto kurzu databáze](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. Klikněte na tlačítko hello **vytvořit nový** propojení a přidejte hodnoty toohello **název** a **popis** pole. Nechte hello **dokončeno** zaškrtnutí políčka zrušit jinak hello nové **položky** přidána ve stavu dokončení a nezobrazí se na počáteční seznam hello.
   
    ![Snímek obrazovky zobrazení pro vytváření hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. Klikněte na tlačítko **vytvořit** a jsou přesměrovaného back toohello **Index** zobrazení a **položky** se zobrazí v seznamu hello.
   
    ![Snímek obrazovky zobrazení Index hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    Působí volné tooadd několik dalších **položky** tooyour seznamu úkolů.
    
4. Klikněte na tlačítko **upravit** další tooan **položky** na seznamu hello a jsou přesměrováni toohello **upravit** zobrazení, kde můžete aktualizovat jakoukoli vlastnost objektu, včetně hello  **Dokončit** příznak. Pokud označíte hello **Complete** příznak a klikněte na tlačítko **Uložit**, hello **položky** se odebral ze seznamu hello neúplných úkolů.
   
    ![Snímek obrazovky zobrazení Index se zaškrtnutým políčkem dokončeno hello hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. Jakmile otestujete aplikace hello, stiskněte Ctrl + F5 toostop ladění aplikace hello. Jste připravené toodeploy!

## <a name="_Toc395637774"></a>Krok 7: Nasazení tooAzure hello aplikace služby App Service 
Teď, když máte hotové aplikace hello správně funguje s Azure Cosmos DB vytvoříme toodeploy tooAzure této webové aplikace služby App Service.  

1. Klikněte pravým tlačítkem na projekt hello v toopublish potřebujete toodo všechny této aplikace je **Průzkumníku řešení** a klikněte na tlačítko **publikovat**.
   
    ![Snímek obrazovky hello možností publikovat v Průzkumníkovi řešení](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. V hello **publikovat** dialogové okno, klikněte na tlačítko **Microsoft Azure App Service**, pak vyberte **vytvořit nový** toocreate aplikační službu profil, nebo klikněte na tlačítko **vyberte Existující** toouse stávající profil.

    ![Dialogové okno publikování v sadě Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. Pokud máte existující profil Azure App Service, zadejte název vašeho předplatného. Použití hello **zobrazení** filtrovat toosort skupinu prostředků nebo typ prostředku a potom vyberte Azure App Service. 
   
    ![Dialogové okno služby App Service v sadě Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. Klikněte na tlačítko toocreate nový profil Azure App Service, **vytvořit nový** v hello **publikovat** dialogové okno. V hello **vytvořit službu App Service** dialogové okno, zadejte název webové aplikace a příslušné předplatné, skupinu prostředků a plán služby App Service, a pak klikněte na tlačítko **vytvořit**.

    ![Služby App Service dialogové okno vytvořit v sadě Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

Během pár sekund bude Visual Studio dokončí publikování webové aplikace a spustí prohlížeč, kde uvidíte vaše handiwork běžící v Azure!



## <a name="_Toc395637775"></a>Další kroky
Blahopřejeme! Právě vytvořené webové aplikace pomocí Azure Cosmos DB první rozhraní ASP.NET MVC a publikovali jste ji tooAzure. zdrojový kód pro hello hotové aplikace včetně podrobností hello Hello a odstranit funkce, které nebyly zahrnuty v tomto kurzu lze stáhnout nebo naklonovat z [Githubu][GitHub]. Takže pokud vás zajímá přidání tooyour aplikaci, získat kód hello a přidejte ho toothis aplikace.

tooadd další funkce tooyour aplikaci, zkontrolujte hello rozhraní API dostupná v hello [knihovny .NET DB Cosmos Azure](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) a myslíte, že volné toocontribute toohello knihovny .NET DB Cosmos Azure na [Githubu] [GitHub]. 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
