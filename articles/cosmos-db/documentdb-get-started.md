---
title: "Azure Cosmos DB: Úvodní kurz k rozhraní DocumentDB API | Dokumentace Microsoftu"
description: "Kurz, který vytváří online databáze a Konzolová aplikace C# pomocí hello DocumentDB rozhraní API."
keywords: "kurz nosql, online databáze konzolová aplikace jazyka c#"
services: cosmos-db
documentationcenter: .net
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2017
ms.author: anhoh
ms.openlocfilehash: 65a181f715a670987492ad7815ef2ec94498e84d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-api-getting-started-tutorial"></a>Azure Cosmos DB: Úvodní kurz k rozhraní DocumentDB API
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js pro MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Vítejte toohello rozhraní API služby Azure Cosmos databáze DocumentDB kurz Začínáme! Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.

Budeme se zabývat těmito tématy:

* Vytvoření a připojení účtu Azure Cosmos DB tooan
* Konfigurace řešení v nástroji Visual Studio
* Vytvoření online databáze
* Vytvoření kolekce
* Vytvoření dokumentů JSON
* Dotazování na kolekci hello
* Nahrazení dokumentu
* Odstranění dokumentu
* Odstraňování databáze aplikace hello

Nemáte čas? Nevadí! Hello úplné řešení je k dispozici na [Githubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). Jump toohello [načtení hello dokončení NoSQL řešení kurzu oddílu](#GetSolution) pro rychlé pokyny.

Později, prosím použijte hello hlasovací tlačítka v hello horní nebo dolní části této stránky toogive nám zpětnou vazbu. Pokud byste nám chtěli toocontact přímo, cítíte volné tooinclude e-mailovou adresou v komentářích.

Můžeme začít!

## <a name="prerequisites"></a>Požadavky
Přesvědčte se, že máte následující hello:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/). 
    * Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu.
* [Visual Studio Community 2017](http://www.visualstudio.com/).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Krok 1: Vytvoření účtu služby Azure Cosmos DB
Vytvořme účet služby Azure Cosmos DB. Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS). Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Krok 2: Nastavení řešení v sadě Visual Studio
1. Otevřete v počítači **Visual Studio 2017**.
2. Na hello **soubor** nabídce vyberte možnost **nový**a potom zvolte **projektu**.
3. V hello **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolové aplikace**, název projekt a pak klikněte na tlačítko **OK**.
   ![Snímek obrazovky okna Nový projekt hello](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)
4. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na novou konzolovou aplikaci, která je v části řešení sady Visual Studio, a pak klikněte na tlačítko **spravovat balíčky NuGet...**
    
    ![Snímek obrazovky znázorňující hello právo místní nabídky pro hello projektu](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. V hello **Nuget** , klikněte na **Procházet**a typ **azure documentdb** hello vyhledávacího pole.
6. V rámci hello výsledky najít **Microsoft.Azure.DocumentDB** a klikněte na tlačítko **nainstalovat**.
   je třeba ID balíčku Hello pro hello klientské knihovny DocumentDB rozhraní API Azure Cosmos DB [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).
   ![Snímek obrazovky hello nabídky Nuget pro vyhledání Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

    Pokud se zobrazí zprávy o Kontrola řešení toohello změny, klikněte na tlačítko **OK**. Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.

Výborně! Teď když jsme dokončili hello instalace, napišme nějaký kód. Úplný projekt s kódem pro tento kurz najdete na [GitHubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).

## <a id="Connect"></a>Krok 3: Připojení účtu Azure Cosmos DB tooan
Nejprve přidejte tyto odkazuje toohello začátek aplikace C#, v souboru Program.cs hello:

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART tooYOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> V kurzu hello toocomplete pořadí zkontrolujte, zda že přidat hello závislostí uvedených výše.
> 
> 

Nyní přidejte tyto dvě konstanty a proměnnou *client* pod veřejnou třídu *Program*.

    public class Program
    {
        // ADD THIS PART tooYOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

V dalším kroku head zpět toohello [portálu Azure](https://portal.azure.com) tooretrieve adresu URL koncového bodu a primární klíč. Adresa URL koncového bodu Hello a primární klíč jsou nutné pro vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace.

V hello portálu Azure, přejděte tooyour Azure Cosmos DB účtu a pak klikněte na tlačítko **klíče**.

Zkopírujte z portálu hello hello URI a vložte ji do `<your endpoint URL>` v souboru program.cs hello. Kopírování hello primární klíč z portálu hello a vložte ji do `<your primary key>`.

![Snímek obrazovky hello portálu Azure používá hello toocreate kurzu NoSQL konzolovou aplikaci C#. Zobrazuje Azure DB Cosmos účet s AKTIVNÍM centrem hello zvýrazní, hello tlačítkem klíče v okně účtu Azure Cosmos DB hello a hodnotami URI, primární klíč a sekundární klíč hello zvýrazněným hello okna klíče][keys]

V dalším kroku začneme hello aplikace tak, že vytvoříte novou instanci třídy hello **DocumentClient**.

Níže hello **hlavní** metoda, přidejte tento nový asynchronní úkol pojmenovaný **GetStartedDemo**, který vytvoří instanci našeho nového **DocumentClient**.

    static void Main(string[] args)
    {
    }

    // ADD THIS PART tooYOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

Přidejte následující hello kód toorun asynchronní úkol z vaší **hlavní** metoda. Hello **hlavní** metoda zachytí výjimky a jejich zápis toohello konzoly.

    static void Main(string[] args)
    {
            // ADD THIS PART tooYOUR CODE
            try
            {
                    Program p = new Program();
                    p.GetStartedDemo().Wait();
            }
            catch (DocumentClientException de)
            {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
            }
            catch (Exception e)
            {
                    Exception baseException = e.GetBaseException();
                    Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
            }
            finally
            {
                    Console.WriteLine("End of demo, press any key tooexit.");
                    Console.ReadKey();
            }

Stiskněte klávesu **F5** toorun vaší aplikace. výstup okna konzoly Hello zobrazí zprávu hello `End of demo, press any key tooexit.` potvrzení, že bylo vytvořeno připojení hello.  Zavřete okno konzoly hello. 

Blahopřejeme! Úspěšně jste se připojili účet Azure Cosmos DB tooan, teď Podívejme se na práci s prostředky Azure Cosmos DB.  

## <a name="step-4-create-a-database"></a>Krok 4: Vytvoření databáze
Než přidáte hello kód pro vytvoření databáze, přidejte pomocnou metodu pro výpis toohello konzoly.

Zkopírujte a vložte hello **WriteToConsoleAndPromptToContinue** metoda po hello **GetStartedDemo** metoda.

    // ADD THIS PART tooYOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key toocontinue ...");
            Console.ReadKey();
    }

Vaše Azure DB Cosmos [databáze](documentdb-resources.md#databases) lze vytvořit pomocí hello [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metoda hello **DocumentClient** třídy. Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi hello.

Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po vytvoření klienta hello. Tím se vytvoří databáze s názvem *FamilyDB*.

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART tooYOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

Stiskněte klávesu **F5** toorun vaší aplikace.

Blahopřejeme! Úspěšně jste vytvořili databázi Azure Cosmos DB.  

## <a id="CreateColl"></a>Krok 5: Vytvoření kolekce
> [!WARNING]
> **CreateDocumentCollectionIfNotExistsAsync** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky. Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

A [kolekce](documentdb-resources.md#collections) lze vytvořit pomocí hello [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metoda hello **DocumentClient** třídy. Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.

Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po vytvoření databáze hello. Tím se vytvoří kolekce dokumentů s názvem *FamilyCollection*.

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART tooYOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

Stiskněte klávesu **F5** toorun vaší aplikace.

Blahopřejeme! Úspěšně jste vytvořili kolekci dokumentů Azure Cosmos DB.  

## <a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON
A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metoda hello **DocumentClient** třídy. Dokumenty představují uživatelem definovaný (libovolný) obsah JSON. Nyní můžete vložit jeden nebo více dokumentů. Pokud již máte data, které byste chtěli toostore v databázi, můžete použít hello Azure Cosmos DB [nástroj pro migraci dat](import-data.md) tooimport hello data do databáze.

Nejdřív potřebujeme toocreate **rodiny** třídu, která bude představovat objekty uložené v Azure Cosmos DB v této ukázce. Kromě toho vytvoříme i podtřídy **Parent**, **Child**, **Pet** a **Address**, které se použijí v rámci **Family**. Povšimněte si, že dokumenty musí mít vlastnost **Id** serializovanou jako **id** ve formátu JSON. Vytvořte tyto třídy tak, že přidáte následující vnitřní podtřídy po hello hello **GetStartedDemo** metoda.

Zkopírujte a vložte hello **rodiny**, **nadřazené**, **podřízené**, **Pet**, a **adresu** třídy po hello **WriteToConsoleAndPromptToContinue** metoda.

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
    }

    // ADD THIS PART tooYOUR CODE
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }

Zkopírujte a vložte hello **CreateFamilyDocumentIfNotExists** pod vaší **adresu** třídy.

    // ADD THIS PART tooYOUR CODE
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
            }
            else
            {
                throw;
            }
        }
    }

A vložte dva dokumenty, jeden pro rodinu hello a hello rodinu Wakefieldů.

Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po vytvoření kolekce dokumentů hello.

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


    // ADD THIS PART tooYOUR CODE
    Family andersenFamily = new Family
    {
            Id = "Andersen.1",
            LastName = "Andersen",
            Parents = new Parent[]
            {
                    new Parent { FirstName = "Thomas" },
                    new Parent { FirstName = "Mary Kay" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FirstName = "Henriette Thaulow",
                            Gender = "female",
                            Grade = 5,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Fluffy" }
                            }
                    }
            },
            Address = new Address { State = "WA", County = "King", City = "Seattle" },
            IsRegistered = true
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

    Family wakefieldFamily = new Family
    {
            Id = "Wakefield.7",
            LastName = "Wakefield",
            Parents = new Parent[]
            {
                    new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                    new Parent { FamilyName = "Miller", FirstName = "Ben" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FamilyName = "Merriam",
                            FirstName = "Jesse",
                            Gender = "female",
                            Grade = 8,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Goofy" },
                                    new Pet { GivenName = "Shadow" }
                            }
                    },
                    new Child
                    {
                            FamilyName = "Miller",
                            FirstName = "Lisa",
                            Gender = "female",
                            Grade = 1
                    }
            },
            Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
            IsRegistered = false
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

Stiskněte klávesu **F5** toorun vaší aplikace.

Blahopřejeme! Úspěšně jste vytvořili dva dokumenty Azure Cosmos DB.  

![Diagram ilustrující hierarchický vztah hello mezi hello účet, hello online databáze, kolekce hello a hello dokumenty používanými v kurzu NoSQL toocreate konzolovou aplikaci C# hello](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Krok 7: Dotazování prostředků Azure Cosmos DB
Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.  Hello následující vzorový kód ukazuje různé dotazy – používání obou Azure SQL DB Cosmos syntaxe, jakož i LINQ – které jsme dají spouštět i proti hello dokumenty, které jsme v předchozím kroku hello vložit.

Zkopírujte a vložte hello **ExecuteSimpleQuery** metoda po vaší **CreateFamilyDocumentIfNotExists** metoda.

    // ADD THIS PART tooYOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find hello Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // hello query is executed synchronously here, but can also be executed asynchronously via hello IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute hello same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key toocontinue ...");
            Console.ReadKey();
    }

Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po vytvoření druhého dokumentu hello.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART tooYOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Stiskněte klávesu **F5** toorun vaší aplikace.

Blahopřejeme! Úspěšně jste provedli dotaz na kolekci Azure Cosmos DB.

Hello následující diagram ilustruje, jak se hello Azure Cosmos DB SQL dotazu, že se volá syntaxe proti kolekci hello jste vytvořili, a hello stejná logika platí také dotaz LINQ toohello.

![Diagram ilustrující hello obor a význam dotazu hello používá toocreate kurzu NoSQL hello konzolovou aplikaci C#](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

Hello [FROM](documentdb-sql-query.md#FromClause) – klíčové slovo je volitelný hello dotazu, protože Azure Cosmos DB dotazy jsou již vymezená tooa jedinou kolekci. Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte. Azure Cosmos DB odvodí, že rodiny, root nebo název proměnné hello jste vybrali, odkaz na aktuální kolekci hello ve výchozím nastavení.

## <a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON
Azure Cosmos DB podporuje nahrazování dokumentů JSON.  

Zkopírujte a vložte hello **ReplaceFamilyDocument** metoda po vaší **ExecuteSimpleQuery** metoda.

    // ADD THIS PART tooYOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po spuštění dotazu hello, na konci hello hello metody. Po nahrazení dokumentu hello, tím se spustí hello stejný dotaz znovu tooview hello změnit dokumentu.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART tooYOUR CODE
    // Update hello Grade of hello Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Stiskněte klávesu **F5** toorun vaší aplikace.

Blahopřejeme! Úspěšně jste nahradili dokument Azure Cosmos DB.

## <a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON
Azure Cosmos DB podporuje odstraňování dokumentů JSON.  

Zkopírujte a vložte hello **DeleteFamilyDocument** metoda po vaší **ReplaceFamilyDocument** metoda.

    // ADD THIS PART tooYOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po hello spuštění druhého dotazu, na konci hello hello metody.

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART tooCODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

Stiskněte klávesu **F5** toorun vaší aplikace.

Blahopřejeme! Úspěšně jste odstranili dokument Azure Cosmos DB.

## <a id="DeleteDatabase"></a>Krok 10: Odstranění databáze hello
Odstraňování hello vytvořené databáze dojde k odebrání hello databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).

Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po hello dokumentu odstranit toodelete hello celou databázi a její podřízené prostředky.

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART tooCODE
    // Clean up/delete hello database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

Stiskněte klávesu **F5** toorun vaší aplikace.

Blahopřejeme! Úspěšně jste odstranili databázi Azure Cosmos DB.

## <a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#
Stiskněte F5 v sadě Visual Studio toobuild hello aplikace v režimu ladění.

Měli byste vidět hello výstup počáteční aplikace v okně konzoly. Hello výstup bude zobrazovat výsledky hello hello dotazy jsme přidali a by měl odpovídat ukázkovému textu hello níže.

    Created FamilyDB
    Press any key toocontinue ...
    Created FamilyCollection
    Press any key toocontinue ...
    Created Family Andersen.1
    Press any key toocontinue ...
    Created Family Wakefield.7
    Press any key toocontinue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key toocontinue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key tooexit.

Blahopřejeme! Po dokončení kurzu hello a mít práci konzolovou aplikaci C#!

## <a id="GetSolution"></a>Získání úplného řešení kurzu hello
Pokud nebyly čas toocomplete hello kroky v tomto kurzu, nebo jenom chcete toodownload hello ukázky kódu, můžete získat z [Githubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). 

řešení GetStarted hello toobuild, budete potřebovat následující hello:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).
* [Účet Azure Cosmos DB][cosmos-db-create-account].
* Hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) řešení, které jsou dostupné na Githubu.

toorestore hello odkazy toohello Cosmos DB .NET SDK služby Azure v sadě Visual Studio, klikněte pravým tlačítkem na hello **GetStarted** řešení v Průzkumníku řešení a pak klikněte na tlačítko **povolit obnovení balíčků NuGet**. Potom v souboru App.config hello aktualizujte hodnoty EndpointUrl a authorizationkey tak hello jak je popsáno v [připojení účtu Azure Cosmos DB tooan](#Connect).

A to je vše, stačí sestavit a máte hotovo.


## <a name="next-steps"></a>Další kroky
* Chcete komplexnější kurz pro ASP.NET MVC? V tématu [kurz k ASP.NET MVC: vývoj webových aplikací s Azure Cosmos DB](documentdb-dotnet-application.md).
* Chcete tooperform škálování a výkon testování pomocí Azure Cosmos DB? V tématu [výkonu a možností škálování testování pomocí Azure Cosmos DB](performance-testing.md)
* Zjistěte, jak příliš[sledování požadavků, využití a úložiště Azure Cosmos DB](monitor-accounts.md).
* Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).
* toolearn Další informace o databázi Cosmos Azure, najdete v části [Vítejte Cosmos DB tooAzure](https://docs.microsoft.com/azure/cosmos-db/introduction).

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-documentdb-dotnet.md#create-account
