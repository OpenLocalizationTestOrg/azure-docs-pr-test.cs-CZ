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
# <a name="azure-cosmos-db-documentdb-api-getting-started-tutorial"></a><span data-ttu-id="3c9b9-104">Azure Cosmos DB: Úvodní kurz k rozhraní DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="3c9b9-104">Azure Cosmos DB: DocumentDB API getting started tutorial</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c9b9-105">.NET</span><span class="sxs-lookup"><span data-stu-id="3c9b9-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="3c9b9-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c9b9-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="3c9b9-107">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="3c9b9-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="3c9b9-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="3c9b9-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="3c9b9-109">Java</span><span class="sxs-lookup"><span data-stu-id="3c9b9-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="3c9b9-110">C++</span><span class="sxs-lookup"><span data-stu-id="3c9b9-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="3c9b9-111">Vítejte toohello rozhraní API služby Azure Cosmos databáze DocumentDB kurz Začínáme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-111">Welcome toohello Azure Cosmos DB DocumentDB API getting started tutorial!</span></span> <span data-ttu-id="3c9b9-112">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="3c9b9-113">Budeme se zabývat těmito tématy:</span><span class="sxs-lookup"><span data-stu-id="3c9b9-113">We'll cover:</span></span>

* <span data-ttu-id="3c9b9-114">Vytvoření a připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="3c9b9-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="3c9b9-115">Konfigurace řešení v nástroji Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c9b9-115">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="3c9b9-116">Vytvoření online databáze</span><span class="sxs-lookup"><span data-stu-id="3c9b9-116">Creating an online database</span></span>
* <span data-ttu-id="3c9b9-117">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="3c9b9-117">Creating a collection</span></span>
* <span data-ttu-id="3c9b9-118">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="3c9b9-118">Creating JSON documents</span></span>
* <span data-ttu-id="3c9b9-119">Dotazování na kolekci hello</span><span class="sxs-lookup"><span data-stu-id="3c9b9-119">Querying hello collection</span></span>
* <span data-ttu-id="3c9b9-120">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="3c9b9-120">Replacing a document</span></span>
* <span data-ttu-id="3c9b9-121">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="3c9b9-121">Deleting a document</span></span>
* <span data-ttu-id="3c9b9-122">Odstraňování databáze aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3c9b9-122">Deleting hello database</span></span>

<span data-ttu-id="3c9b9-123">Nemáte čas?</span><span class="sxs-lookup"><span data-stu-id="3c9b9-123">Don't have time?</span></span> <span data-ttu-id="3c9b9-124">Nevadí!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-124">Don't worry!</span></span> <span data-ttu-id="3c9b9-125">Hello úplné řešení je k dispozici na [Githubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-125">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> <span data-ttu-id="3c9b9-126">Jump toohello [načtení hello dokončení NoSQL řešení kurzu oddílu](#GetSolution) pro rychlé pokyny.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-126">Jump toohello [Get hello complete NoSQL tutorial solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="3c9b9-127">Později, prosím použijte hello hlasovací tlačítka v hello horní nebo dolní části této stránky toogive nám zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-127">Afterwards, please use hello voting buttons at hello top or bottom of this page toogive us feedback.</span></span> <span data-ttu-id="3c9b9-128">Pokud byste nám chtěli toocontact přímo, cítíte volné tooinclude e-mailovou adresou v komentářích.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="3c9b9-129">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-129">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c9b9-130">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c9b9-130">Prerequisites</span></span>
<span data-ttu-id="3c9b9-131">Přesvědčte se, že máte následující hello:</span><span class="sxs-lookup"><span data-stu-id="3c9b9-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="3c9b9-132">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-132">An active Azure account.</span></span> <span data-ttu-id="3c9b9-133">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-133">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="3c9b9-134">Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-134">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="3c9b9-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="3c9b9-136">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3c9b9-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="3c9b9-137">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="3c9b9-138">Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-138">If you already have an account you want toouse, you can skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="3c9b9-139">Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-139">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="3c9b9-140"><a id="SetupVS"></a>Krok 2: Nastavení řešení v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c9b9-140"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="3c9b9-141">Otevřete v počítači **Visual Studio 2017**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-141">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="3c9b9-142">Na hello **soubor** nabídce vyberte možnost **nový**a potom zvolte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-142">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="3c9b9-143">V hello **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolové aplikace**, název projekt a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-143">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console Application**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="3c9b9-144">![Snímek obrazovky okna Nový projekt hello](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="3c9b9-144">![Screen shot of hello New Project window](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span></span>
4. <span data-ttu-id="3c9b9-145">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na novou konzolovou aplikaci, která je v části řešení sady Visual Studio, a pak klikněte na tlačítko **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="3c9b9-145">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Snímek obrazovky znázorňující hello právo místní nabídky pro hello projektu](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="3c9b9-147">V hello **Nuget** , klikněte na **Procházet**a typ **azure documentdb** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-147">In hello **Nuget** tab, click **Browse**, and type **azure documentdb** in hello search box.</span></span>
6. <span data-ttu-id="3c9b9-148">V rámci hello výsledky najít **Microsoft.Azure.DocumentDB** a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-148">Within hello results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="3c9b9-149">je třeba ID balíčku Hello pro hello klientské knihovny DocumentDB rozhraní API Azure Cosmos DB [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-149">hello package ID for hello Azure Cosmos DB DocumentDB API Client Library is [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span></span>
   <span data-ttu-id="3c9b9-150">![Snímek obrazovky hello nabídky Nuget pro vyhledání Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="3c9b9-150">![Screen shot of hello Nuget Menu for finding Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="3c9b9-151">Pokud se zobrazí zprávy o Kontrola řešení toohello změny, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-151">If you get a messages about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="3c9b9-152">Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-152">If you get a message about license acceptance, click **I accept**.</span></span>

<span data-ttu-id="3c9b9-153">Výborně!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-153">Great!</span></span> <span data-ttu-id="3c9b9-154">Teď když jsme dokončili hello instalace, napišme nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-154">Now that we finished hello setup, let's start writing some code.</span></span> <span data-ttu-id="3c9b9-155">Úplný projekt s kódem pro tento kurz najdete na [GitHubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-155">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span></span>

## <span data-ttu-id="3c9b9-156"><a id="Connect"></a>Krok 3: Připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="3c9b9-156"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="3c9b9-157">Nejprve přidejte tyto odkazuje toohello začátek aplikace C#, v souboru Program.cs hello:</span><span class="sxs-lookup"><span data-stu-id="3c9b9-157">First, add these references toohello beginning of your C# application, in hello Program.cs file:</span></span>

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART tooYOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> <span data-ttu-id="3c9b9-158">V kurzu hello toocomplete pořadí zkontrolujte, zda že přidat hello závislostí uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-158">In order toocomplete hello tutorial, make sure you add hello dependencies above.</span></span>
> 
> 

<span data-ttu-id="3c9b9-159">Nyní přidejte tyto dvě konstanty a proměnnou *client* pod veřejnou třídu *Program*.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-159">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

    public class Program
    {
        // ADD THIS PART tooYOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

<span data-ttu-id="3c9b9-160">V dalším kroku head zpět toohello [portálu Azure](https://portal.azure.com) tooretrieve adresu URL koncového bodu a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-160">Next, head back toohello [Azure Portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="3c9b9-161">Adresa URL koncového bodu Hello a primární klíč jsou nutné pro vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-161">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="3c9b9-162">V hello portálu Azure, přejděte tooyour Azure Cosmos DB účtu a pak klikněte na tlačítko **klíče**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-162">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="3c9b9-163">Zkopírujte z portálu hello hello URI a vložte ji do `<your endpoint URL>` v souboru program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-163">Copy hello URI from hello portal and paste it into `<your endpoint URL>` in hello program.cs file.</span></span> <span data-ttu-id="3c9b9-164">Kopírování hello primární klíč z portálu hello a vložte ji do `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-164">Then copy hello PRIMARY KEY from hello portal and paste it into `<your primary key>`.</span></span>

![Snímek obrazovky hello portálu Azure používá hello toocreate kurzu NoSQL konzolovou aplikaci C#.][keys]

<span data-ttu-id="3c9b9-167">V dalším kroku začneme hello aplikace tak, že vytvoříte novou instanci třídy hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-167">Next, we'll start hello application by creating a new instance of hello **DocumentClient**.</span></span>

<span data-ttu-id="3c9b9-168">Níže hello **hlavní** metoda, přidejte tento nový asynchronní úkol pojmenovaný **GetStartedDemo**, který vytvoří instanci našeho nového **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-168">Below hello **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

    static void Main(string[] args)
    {
    }

    // ADD THIS PART tooYOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

<span data-ttu-id="3c9b9-169">Přidejte následující hello kód toorun asynchronní úkol z vaší **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-169">Add hello following code toorun your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="3c9b9-170">Hello **hlavní** metoda zachytí výjimky a jejich zápis toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-170">hello **Main** method will catch exceptions and write them toohello console.</span></span>

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

<span data-ttu-id="3c9b9-171">Stiskněte klávesu **F5** toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-171">Press **F5** toorun your application.</span></span> <span data-ttu-id="3c9b9-172">výstup okna konzoly Hello zobrazí zprávu hello `End of demo, press any key tooexit.` potvrzení, že bylo vytvořeno připojení hello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-172">hello console window output displays hello message `End of demo, press any key tooexit.` confirming that hello connection was made.</span></span>  <span data-ttu-id="3c9b9-173">Zavřete okno konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-173">You can then close hello console window.</span></span> 

<span data-ttu-id="3c9b9-174">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-174">Congratulations!</span></span> <span data-ttu-id="3c9b9-175">Úspěšně jste se připojili účet Azure Cosmos DB tooan, teď Podívejme se na práci s prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-175">You have successfully connected tooan Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="3c9b9-176">Krok 4: Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="3c9b9-176">Step 4: Create a database</span></span>
<span data-ttu-id="3c9b9-177">Než přidáte hello kód pro vytvoření databáze, přidejte pomocnou metodu pro výpis toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-177">Before you add hello code for creating a database, add a helper method for writing toohello console.</span></span>

<span data-ttu-id="3c9b9-178">Zkopírujte a vložte hello **WriteToConsoleAndPromptToContinue** metoda po hello **GetStartedDemo** metoda.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-178">Copy and paste hello **WriteToConsoleAndPromptToContinue** method after hello **GetStartedDemo** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key toocontinue ...");
            Console.ReadKey();
    }

<span data-ttu-id="3c9b9-179">Vaše Azure DB Cosmos [databáze](documentdb-resources.md#databases) lze vytvořit pomocí hello [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-179">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="3c9b9-180">Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi hello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-180">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="3c9b9-181">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po vytvoření klienta hello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-181">Copy and paste hello following code tooyour **GetStartedDemo** method after hello client creation.</span></span> <span data-ttu-id="3c9b9-182">Tím se vytvoří databáze s názvem *FamilyDB*.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-182">This will create a database named *FamilyDB*.</span></span>

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART tooYOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

<span data-ttu-id="3c9b9-183">Stiskněte klávesu **F5** toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-183">Press **F5** toorun your application.</span></span>

<span data-ttu-id="3c9b9-184">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-184">Congratulations!</span></span> <span data-ttu-id="3c9b9-185">Úspěšně jste vytvořili databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-185">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="3c9b9-186"><a id="CreateColl"></a>Krok 5: Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="3c9b9-186"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="3c9b9-187">**CreateDocumentCollectionIfNotExistsAsync** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-187">**CreateDocumentCollectionIfNotExistsAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="3c9b9-188">Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-188">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="3c9b9-189">A [kolekce](documentdb-resources.md#collections) lze vytvořit pomocí hello [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-189">A [collection](documentdb-resources.md#collections) can be created by using hello [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="3c9b9-190">Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-190">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="3c9b9-191">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po vytvoření databáze hello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-191">Copy and paste hello following code tooyour **GetStartedDemo** method after hello database creation.</span></span> <span data-ttu-id="3c9b9-192">Tím se vytvoří kolekce dokumentů s názvem *FamilyCollection*.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-192">This will create a document collection named *FamilyCollection*.</span></span>

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART tooYOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

<span data-ttu-id="3c9b9-193">Stiskněte klávesu **F5** toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-193">Press **F5** toorun your application.</span></span>

<span data-ttu-id="3c9b9-194">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-194">Congratulations!</span></span> <span data-ttu-id="3c9b9-195">Úspěšně jste vytvořili kolekci dokumentů Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-195">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="3c9b9-196"><a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="3c9b9-196"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="3c9b9-197">A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-197">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="3c9b9-198">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-198">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="3c9b9-199">Nyní můžete vložit jeden nebo více dokumentů.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-199">We can now insert one or more documents.</span></span> <span data-ttu-id="3c9b9-200">Pokud již máte data, které byste chtěli toostore v databázi, můžete použít hello Azure Cosmos DB [nástroj pro migraci dat](import-data.md) tooimport hello data do databáze.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-200">If you already have data you'd like toostore in your database, you can use hello Azure Cosmos DB [Data Migration tool](import-data.md) tooimport hello data into a database.</span></span>

<span data-ttu-id="3c9b9-201">Nejdřív potřebujeme toocreate **rodiny** třídu, která bude představovat objekty uložené v Azure Cosmos DB v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-201">First, we need toocreate a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="3c9b9-202">Kromě toho vytvoříme i podtřídy **Parent**, **Child**, **Pet** a **Address**, které se použijí v rámci **Family**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-202">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="3c9b9-203">Povšimněte si, že dokumenty musí mít vlastnost **Id** serializovanou jako **id** ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-203">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="3c9b9-204">Vytvořte tyto třídy tak, že přidáte následující vnitřní podtřídy po hello hello **GetStartedDemo** metoda.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-204">Create these classes by adding hello following internal sub-classes after hello **GetStartedDemo** method.</span></span>

<span data-ttu-id="3c9b9-205">Zkopírujte a vložte hello **rodiny**, **nadřazené**, **podřízené**, **Pet**, a **adresu** třídy po hello **WriteToConsoleAndPromptToContinue** metoda.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-205">Copy and paste hello **Family**, **Parent**, **Child**, **Pet**, and **Address** classes after hello **WriteToConsoleAndPromptToContinue** method.</span></span>

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

<span data-ttu-id="3c9b9-206">Zkopírujte a vložte hello **CreateFamilyDocumentIfNotExists** pod vaší **adresu** třídy.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-206">Copy and paste hello **CreateFamilyDocumentIfNotExists** method underneath your **Address** class.</span></span>

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

<span data-ttu-id="3c9b9-207">A vložte dva dokumenty, jeden pro rodinu hello a hello rodinu Wakefieldů.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-207">And insert two documents, one each for hello Andersen Family and hello Wakefield Family.</span></span>

<span data-ttu-id="3c9b9-208">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po vytvoření kolekce dokumentů hello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-208">Copy and paste hello following code tooyour **GetStartedDemo** method after hello document collection creation.</span></span>

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

<span data-ttu-id="3c9b9-209">Stiskněte klávesu **F5** toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-209">Press **F5** toorun your application.</span></span>

<span data-ttu-id="3c9b9-210">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-210">Congratulations!</span></span> <span data-ttu-id="3c9b9-211">Úspěšně jste vytvořili dva dokumenty Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-211">You have successfully created two Azure Cosmos DB documents.</span></span>  

![Diagram ilustrující hierarchický vztah hello mezi hello účet, hello online databáze, kolekce hello a hello dokumenty používanými v kurzu NoSQL toocreate konzolovou aplikaci C# hello](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="3c9b9-213"><a id="Query"></a>Krok 7: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3c9b9-213"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="3c9b9-214">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-214">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="3c9b9-215">Hello následující vzorový kód ukazuje různé dotazy – používání obou Azure SQL DB Cosmos syntaxe, jakož i LINQ – které jsme dají spouštět i proti hello dokumenty, které jsme v předchozím kroku hello vložit.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-215">hello following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against hello documents we inserted in hello previous step.</span></span>

<span data-ttu-id="3c9b9-216">Zkopírujte a vložte hello **ExecuteSimpleQuery** metoda po vaší **CreateFamilyDocumentIfNotExists** metoda.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-216">Copy and paste hello **ExecuteSimpleQuery** method after your **CreateFamilyDocumentIfNotExists** method.</span></span>

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

<span data-ttu-id="3c9b9-217">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po vytvoření druhého dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-217">Copy and paste hello following code tooyour **GetStartedDemo** method after hello second document creation.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART tooYOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="3c9b9-218">Stiskněte klávesu **F5** toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-218">Press **F5** toorun your application.</span></span>

<span data-ttu-id="3c9b9-219">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-219">Congratulations!</span></span> <span data-ttu-id="3c9b9-220">Úspěšně jste provedli dotaz na kolekci Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-220">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="3c9b9-221">Hello následující diagram ilustruje, jak se hello Azure Cosmos DB SQL dotazu, že se volá syntaxe proti kolekci hello jste vytvořili, a hello stejná logika platí také dotaz LINQ toohello.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-221">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created, and hello same logic applies toohello LINQ query as well.</span></span>

![Diagram ilustrující hello obor a význam dotazu hello používá toocreate kurzu NoSQL hello konzolovou aplikaci C#](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="3c9b9-223">Hello [FROM](documentdb-sql-query.md#FromClause) – klíčové slovo je volitelný hello dotazu, protože Azure Cosmos DB dotazy jsou již vymezená tooa jedinou kolekci.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-223">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="3c9b9-224">Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-224">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="3c9b9-225">Azure Cosmos DB odvodí, že rodiny, root nebo název proměnné hello jste vybrali, odkaz na aktuální kolekci hello ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-225">Azure Cosmos DB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

## <span data-ttu-id="3c9b9-226"><a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="3c9b9-226"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="3c9b9-227">Azure Cosmos DB podporuje nahrazování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-227">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="3c9b9-228">Zkopírujte a vložte hello **ReplaceFamilyDocument** metoda po vaší **ExecuteSimpleQuery** metoda.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-228">Copy and paste hello **ReplaceFamilyDocument** method after your **ExecuteSimpleQuery** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

<span data-ttu-id="3c9b9-229">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po spuštění dotazu hello, na konci hello hello metody.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-229">Copy and paste hello following code tooyour **GetStartedDemo** method after hello query execution, at hello end of hello method.</span></span> <span data-ttu-id="3c9b9-230">Po nahrazení dokumentu hello, tím se spustí hello stejný dotaz znovu tooview hello změnit dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-230">After replacing hello document, this will run hello same query again tooview hello changed document.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART tooYOUR CODE
    // Update hello Grade of hello Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="3c9b9-231">Stiskněte klávesu **F5** toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-231">Press **F5** toorun your application.</span></span>

<span data-ttu-id="3c9b9-232">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-232">Congratulations!</span></span> <span data-ttu-id="3c9b9-233">Úspěšně jste nahradili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-233">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="3c9b9-234"><a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON</span><span class="sxs-lookup"><span data-stu-id="3c9b9-234"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="3c9b9-235">Azure Cosmos DB podporuje odstraňování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-235">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="3c9b9-236">Zkopírujte a vložte hello **DeleteFamilyDocument** metoda po vaší **ReplaceFamilyDocument** metoda.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-236">Copy and paste hello **DeleteFamilyDocument** method after your **ReplaceFamilyDocument** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

<span data-ttu-id="3c9b9-237">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po hello spuštění druhého dotazu, na konci hello hello metody.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-237">Copy and paste hello following code tooyour **GetStartedDemo** method after hello second query execution, at hello end of hello method.</span></span>

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART tooCODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

<span data-ttu-id="3c9b9-238">Stiskněte klávesu **F5** toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-238">Press **F5** toorun your application.</span></span>

<span data-ttu-id="3c9b9-239">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-239">Congratulations!</span></span> <span data-ttu-id="3c9b9-240">Úspěšně jste odstranili dokument Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-240">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="3c9b9-241"><a id="DeleteDatabase"></a>Krok 10: Odstranění databáze hello</span><span class="sxs-lookup"><span data-stu-id="3c9b9-241"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="3c9b9-242">Odstraňování hello vytvořené databáze dojde k odebrání hello databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-242">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="3c9b9-243">Kopírování a vložení hello následující kód tooyour **GetStartedDemo** metoda po hello dokumentu odstranit toodelete hello celou databázi a její podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-243">Copy and paste hello following code tooyour **GetStartedDemo** method after hello document delete toodelete hello entire database and all children resources.</span></span>

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART tooCODE
    // Clean up/delete hello database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

<span data-ttu-id="3c9b9-244">Stiskněte klávesu **F5** toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-244">Press **F5** toorun your application.</span></span>

<span data-ttu-id="3c9b9-245">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-245">Congratulations!</span></span> <span data-ttu-id="3c9b9-246">Úspěšně jste odstranili databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-246">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="3c9b9-247"><a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#</span><span class="sxs-lookup"><span data-stu-id="3c9b9-247"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="3c9b9-248">Stiskněte F5 v sadě Visual Studio toobuild hello aplikace v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-248">Hit F5 in Visual Studio toobuild hello application in debug mode.</span></span>

<span data-ttu-id="3c9b9-249">Měli byste vidět hello výstup počáteční aplikace v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-249">You should see hello output of your get started app in a console window.</span></span> <span data-ttu-id="3c9b9-250">Hello výstup bude zobrazovat výsledky hello hello dotazy jsme přidali a by měl odpovídat ukázkovému textu hello níže.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-250">hello output will show hello results of hello queries we added and should match hello example text below.</span></span>

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

<span data-ttu-id="3c9b9-251">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-251">Congratulations!</span></span> <span data-ttu-id="3c9b9-252">Po dokončení kurzu hello a mít práci konzolovou aplikaci C#!</span><span class="sxs-lookup"><span data-stu-id="3c9b9-252">You've completed hello tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="3c9b9-253"><a id="GetSolution"></a>Získání úplného řešení kurzu hello</span><span class="sxs-lookup"><span data-stu-id="3c9b9-253"><a id="GetSolution"></a> Get hello complete tutorial solution</span></span>
<span data-ttu-id="3c9b9-254">Pokud nebyly čas toocomplete hello kroky v tomto kurzu, nebo jenom chcete toodownload hello ukázky kódu, můžete získat z [Githubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-254">If you didn't have time toocomplete hello steps in this tutorial, or just want toodownload hello code samples, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> 

<span data-ttu-id="3c9b9-255">řešení GetStarted hello toobuild, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="3c9b9-255">toobuild hello GetStarted solution, you will need hello following:</span></span>

* <span data-ttu-id="3c9b9-256">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-256">An active Azure account.</span></span> <span data-ttu-id="3c9b9-257">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-257">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="3c9b9-258">[Účet Azure Cosmos DB][cosmos-db-create-account].</span><span class="sxs-lookup"><span data-stu-id="3c9b9-258">An [Azure Cosmos DB account][cosmos-db-create-account].</span></span>
* <span data-ttu-id="3c9b9-259">Hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) řešení, které jsou dostupné na Githubu.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-259">hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="3c9b9-260">toorestore hello odkazy toohello Cosmos DB .NET SDK služby Azure v sadě Visual Studio, klikněte pravým tlačítkem na hello **GetStarted** řešení v Průzkumníku řešení a pak klikněte na tlačítko **povolit obnovení balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-260">toorestore hello references toohello Azure Cosmos DB .NET SDK in Visual Studio, right-click hello **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="3c9b9-261">Potom v souboru App.config hello aktualizujte hodnoty EndpointUrl a authorizationkey tak hello jak je popsáno v [připojení účtu Azure Cosmos DB tooan](#Connect).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-261">Next, in hello App.config file, update hello EndpointUrl and AuthorizationKey values as described in [Connect tooan Azure Cosmos DB account](#Connect).</span></span>

<span data-ttu-id="3c9b9-262">A to je vše, stačí sestavit a máte hotovo.</span><span class="sxs-lookup"><span data-stu-id="3c9b9-262">That's it, build it and you're on your way!</span></span>


## <a name="next-steps"></a><span data-ttu-id="3c9b9-263">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c9b9-263">Next steps</span></span>
* <span data-ttu-id="3c9b9-264">Chcete komplexnější kurz pro ASP.NET MVC?</span><span class="sxs-lookup"><span data-stu-id="3c9b9-264">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="3c9b9-265">V tématu [kurz k ASP.NET MVC: vývoj webových aplikací s Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-265">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="3c9b9-266">Chcete tooperform škálování a výkon testování pomocí Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="3c9b9-266">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="3c9b9-267">V tématu [výkonu a možností škálování testování pomocí Azure Cosmos DB](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="3c9b9-267">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="3c9b9-268">Zjistěte, jak příliš[sledování požadavků, využití a úložiště Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-268">Learn how too[monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="3c9b9-269">Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-269">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="3c9b9-270">toolearn Další informace o databázi Cosmos Azure, najdete v části [Vítejte Cosmos DB tooAzure](https://docs.microsoft.com/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="3c9b9-270">toolearn more about Azure Cosmos DB, see [Welcome tooAzure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-documentdb-dotnet.md#create-account
