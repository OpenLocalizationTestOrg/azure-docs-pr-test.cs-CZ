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
# <span data-ttu-id="28431-105"><a name="_Toc395809351"></a>Kurz k ASP.NET MVC: Vývoj webové aplikace s použitím služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="28431-105"><a name="_Toc395809351"></a>ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="28431-106">.NET</span><span class="sxs-lookup"><span data-stu-id="28431-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="28431-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="28431-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="28431-108">Java</span><span class="sxs-lookup"><span data-stu-id="28431-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="28431-109">Python</span><span class="sxs-lookup"><span data-stu-id="28431-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="28431-110">toohighlight způsobu můžete efektivně využívat Azure Cosmos DB toostore a dotazování dokumentů JSON, tento článek obsahuje začátku do konce návod ukazuje, jak toobuild todo aplikace pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="28431-110">toohighlight how you can efficiently leverage Azure Cosmos DB toostore and query JSON documents, this article provides an end-to-end walk-through showing you how toobuild a todo app using Azure Cosmos DB.</span></span> <span data-ttu-id="28431-111">Hello úkoly se budou ukládat jako dokumenty JSON v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="28431-111">hello tasks will be stored as JSON documents in Azure Cosmos DB.</span></span>

![Snímek obrazovky seznamu úkolů hello webové aplikace MVC vytvořené v tomto kurzu – podrobný kurz Asp.net MVC krok za krokem](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image01.png)

<span data-ttu-id="28431-113">Tento návod ukazuje, jak toouse hello data toostore a přístup služby Azure Cosmos DB z webové aplikace ASP.NET MVC hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="28431-113">This walk-through shows you how toouse hello Azure Cosmos DB service toostore and access data from an ASP.NET MVC web application hosted on Azure.</span></span> <span data-ttu-id="28431-114">Pokud hledáte kurz, který se zaměřuje jenom na databázi Cosmos Azure a není hello komponenty ASP.NET MVC najdete v části [sestavit aplikaci konzoly Azure Cosmos DB jazyka C#](documentdb-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="28431-114">If you're looking for a tutorial that focuses only on Azure Cosmos DB, and not hello ASP.NET MVC components, see [Build an Azure Cosmos DB C# console application](documentdb-get-started.md).</span></span>

> [!TIP]
> <span data-ttu-id="28431-115">V tomto kurzu se předpokládá, že již máte zkušenosti s používáním ASP.NET MVC a Webů Azure.</span><span class="sxs-lookup"><span data-stu-id="28431-115">This tutorial assumes that you have prior experience using ASP.NET MVC and Azure Websites.</span></span> <span data-ttu-id="28431-116">Pokud jste nový tooASP.NET nebo hello [požadovaných nástrojů](#_Toc395637760), doporučujeme stáhnout úplný ukázkový projekt hello z [Githubu] [ GitHub] a postupujte podle pokynů hello Tato ukázka.</span><span class="sxs-lookup"><span data-stu-id="28431-116">If you are new tooASP.NET or hello [prerequisite tools](#_Toc395637760), we recommend downloading hello complete sample project from [GitHub][GitHub] and following hello instructions in this sample.</span></span> <span data-ttu-id="28431-117">Po jejím vytvořené, můžete zkontrolovat Tento článek toogain porozuměli hello kódu v kontextu hello hello projektu.</span><span class="sxs-lookup"><span data-stu-id="28431-117">Once you have it built, you can review this article toogain insight on hello code in hello context of hello project.</span></span>
> 
> 

## <span data-ttu-id="28431-118"><a name="_Toc395637760"></a>Předpoklady pro tento databázový kurz</span><span class="sxs-lookup"><span data-stu-id="28431-118"><a name="_Toc395637760"></a>Prerequisites for this database tutorial</span></span>
<span data-ttu-id="28431-119">Před následující hello pokyny v tomto článku, se ujistěte, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="28431-119">Before following hello instructions in this article, you should ensure that you have hello following:</span></span>

* <span data-ttu-id="28431-120">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="28431-120">An active Azure account.</span></span> <span data-ttu-id="28431-121">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="28431-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="28431-122">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="28431-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

    <span data-ttu-id="28431-123">NEBO</span><span class="sxs-lookup"><span data-stu-id="28431-123">OR</span></span>

    <span data-ttu-id="28431-124">Místní instalace hello [emulátoru DB Cosmos Azure](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="28431-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="28431-125">[Visual Studio 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="28431-125">[Visual Studio 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="28431-126">Microsoft Azure SDK pro .NET pro Visual Studio 2017, k dispozici prostřednictvím hello instalační program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="28431-126">Microsoft Azure SDK for .NET for Visual Studio 2017, available through hello Visual Studio Installer.</span></span>

<span data-ttu-id="28431-127">Všechny snímky obrazovky hello v tomto článku byly pořízeny pomocí nástroje Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="28431-127">All hello screen shots in this article have been taken using Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="28431-128">Pokud je systém konfigurován s jinou verzí je možné, že vaše obrazovky a možnosti budou mírně lišit, ale pokud splníte hello výše požadavky tohoto řešení by mělo fungovat.</span><span class="sxs-lookup"><span data-stu-id="28431-128">If your system is configured with a different version it is possible that your screens and options won't match entirely, but if you meet hello above prerequisites this solution should work.</span></span>

## <span data-ttu-id="28431-129"><a name="_Toc395637761"></a>Krok 1: Vytvoření účtu databáze Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="28431-129"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="28431-130">Začněme vytvořením účtu služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="28431-130">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="28431-131">Pokud už máte účet SQL (DocumentDB) pro Azure Cosmos DB nebo pokud používáte hello emulátoru DB Cosmos Azure pro účely tohoto kurzu, můžete přeskočit příliš[vytvoření nové aplikace ASP.NET MVC](#_Toc395637762).</span><span class="sxs-lookup"><span data-stu-id="28431-131">If you already have a SQL (DocumentDB) account for Azure Cosmos DB or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Create a new ASP.NET MVC application](#_Toc395637762).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
<span data-ttu-id="28431-132">Nyní vám ukážeme jak toocreate nové aplikace ASP.NET MVC z hello základů.</span><span class="sxs-lookup"><span data-stu-id="28431-132">We will now walk through how toocreate a new ASP.NET MVC application from hello ground-up.</span></span> 

## <span data-ttu-id="28431-133"><a name="_Toc395637762"></a>Krok 2: Vytvoření nové aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="28431-133"><a name="_Toc395637762"></a>Step 2: Create a new ASP.NET MVC application</span></span>

1. <span data-ttu-id="28431-134">V sadě Visual Studio na hello **soubor** nabídce bodu příliš**nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="28431-134">In Visual Studio, on hello **File** menu, point too**New**, and then click **Project**.</span></span> <span data-ttu-id="28431-135">Hello **nový projekt** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="28431-135">hello **New Project** dialog box appears.</span></span>

2. <span data-ttu-id="28431-136">V hello **typy projektů** podokně rozbalte **šablony**, **Visual C#**, **webové**a potom vyberte **webové aplikace ASP.NET** .</span><span class="sxs-lookup"><span data-stu-id="28431-136">In hello **Project types** pane, expand **Templates**, **Visual C#**, **Web**, and then select **ASP.NET Web Application**.</span></span>

      ![Snímek obrazovky znázorňující dialogové okno Nový projekt hello se zvýrazněným typem projektu webová aplikace ASP.NET hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. <span data-ttu-id="28431-138">V hello **název** pole, název typu hello hello projektu.</span><span class="sxs-lookup"><span data-stu-id="28431-138">In hello **Name** box, type hello name of hello project.</span></span> <span data-ttu-id="28431-139">Tento kurz používá název hello "úkolů".</span><span class="sxs-lookup"><span data-stu-id="28431-139">This tutorial uses hello name "todo".</span></span> <span data-ttu-id="28431-140">Pokud si zvolíte toouse něco jiného, bez ohledu na tomto kurzu bude zmíněn obor názvů todo hello, budete potřebovat tooadjust hello zadat kód ukázky toouse vámi zvolený název aplikace.</span><span class="sxs-lookup"><span data-stu-id="28431-140">If you choose toouse something other than this, then wherever this tutorial talks about hello todo namespace, you need tooadjust hello provided code samples toouse whatever you named your application.</span></span> 
4. <span data-ttu-id="28431-141">Klikněte na tlačítko **Procházet** toonavigate toohello složku, kde jako toocreate hello projekt a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="28431-141">Click **Browse** toonavigate toohello folder where you would like toocreate hello project, and then click **OK**.</span></span>
   
      <span data-ttu-id="28431-142">Hello **nové webové aplikace ASP.NET** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="28431-142">hello **New ASP.NET Web Application** dialog box appears.</span></span>
   
    ![Snímek obrazovky dialogového okna novou webovou aplikaci ASP.NET hello se zvýrazněnou šablonou aplikace MVC hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. <span data-ttu-id="28431-144">V podokně hello šablony, vyberte **MVC**.</span><span class="sxs-lookup"><span data-stu-id="28431-144">In hello templates pane, select **MVC**.</span></span>

6. <span data-ttu-id="28431-145">Klikněte na tlačítko **OK** a nechejte Visual Studio odvést svou práci generování uživatelského rozhraní hello prázdné šablony ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="28431-145">Click **OK** and let Visual Studio do its thing around scaffolding hello empty ASP.NET MVC template.</span></span> 

          
7. <span data-ttu-id="28431-146">Až Visual Studio dokončí vytváření aplikace MVC standardní hello budete mít prázdnou aplikaci ASP.NET, který můžete spustit místně.</span><span class="sxs-lookup"><span data-stu-id="28431-146">Once Visual Studio has finished creating hello boilerplate MVC application you have an empty ASP.NET application that you can run locally.</span></span>
   
    <span data-ttu-id="28431-147">Lokální spuštěné projektu hello místně, protože určitě jsme všechny zaznamenané hello ASP.NET "Hello, World" aplikace.</span><span class="sxs-lookup"><span data-stu-id="28431-147">We'll skip running hello project locally because I'm sure we've all seen hello ASP.NET "Hello World" application.</span></span> <span data-ttu-id="28431-148">Přejděme rovnou tooadding Azure Cosmos DB toothis projektu a sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="28431-148">Let's go straight tooadding Azure Cosmos DB toothis project and building our application.</span></span>

## <span data-ttu-id="28431-149"><a name="_Toc395637767"></a>Krok 3: Přidání projektu webové aplikace v Azure Cosmos DB tooyour MVC</span><span class="sxs-lookup"><span data-stu-id="28431-149"><a name="_Toc395637767"></a>Step 3: Add Azure Cosmos DB tooyour MVC web application project</span></span>
<span data-ttu-id="28431-150">Teď, když máme většinu vložení hello ASP.NET MVC, které potřebujeme pro toto řešení, Pojďme toohello skutečnému účelu tohoto kurzu, přidání webové aplikace MVC tooour Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="28431-150">Now that we have most of hello ASP.NET MVC plumbing that we need for this solution, let's get toohello real purpose of this tutorial, adding Azure Cosmos DB tooour MVC web application.</span></span>

1. <span data-ttu-id="28431-151">Hello .NET SDK služby Azure Cosmos DB je zabalené a distribuovat jako balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="28431-151">hello Azure Cosmos DB .NET SDK is packaged and distributed as a NuGet package.</span></span> <span data-ttu-id="28431-152">tooget hello balíček NuGet v sadě Visual Studio, použijte hello Správce balíčků NuGet v sadě Visual Studio, kliknutím pravým tlačítkem na projekt hello v **Průzkumníku řešení** a pak levým na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="28431-152">tooget hello NuGet package in Visual Studio, use hello NuGet package manager in Visual Studio by right-clicking on hello project in **Solution Explorer** and then clicking **Manage NuGet Packages**.</span></span>
   
    ![Snímek obrazovky hello klikněte pravým tlačítkem na možnosti pro projekt webové aplikace v Průzkumníkovi řešení se spravovat balíčky NuGet zvýrazněná hello.](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    <span data-ttu-id="28431-154">Hello **spravovat balíčky NuGet** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="28431-154">hello **Manage NuGet Packages** dialog box appears.</span></span>
2. <span data-ttu-id="28431-155">V hello NuGet **Procházet** zadejte ***Azure DocumentDB***.</span><span class="sxs-lookup"><span data-stu-id="28431-155">In hello NuGet **Browse** box, type ***Azure DocumentDB***.</span></span> <span data-ttu-id="28431-156">(název balíčku hello nebyla aktualizované tooAzure Cosmos DB.)</span><span class="sxs-lookup"><span data-stu-id="28431-156">(hello package name has not been updated tooAzure Cosmos DB.)</span></span>
   
    <span data-ttu-id="28431-157">Z výsledků hello nainstalovat hello **Microsoft.Azure.DocumentDB Microsoft** balíčku.</span><span class="sxs-lookup"><span data-stu-id="28431-157">From hello results, install hello **Microsoft.Azure.DocumentDB by Microsoft** package.</span></span> <span data-ttu-id="28431-158">Tím stáhnout a nainstalovat balíček Azure Cosmos DB hello a také všechny závislosti, jako je například Newtonsoft.Json.</span><span class="sxs-lookup"><span data-stu-id="28431-158">This will download and install hello Azure Cosmos DB package as well as all dependencies, such as Newtonsoft.Json.</span></span> <span data-ttu-id="28431-159">Klikněte na tlačítko **OK** v hello **Preview** okně a **souhlasím** v hello **přijetí licence** okno toocomplete hello instalace.</span><span class="sxs-lookup"><span data-stu-id="28431-159">Click **OK** in hello **Preview** window, and **I Accept** in hello **License Acceptance** window toocomplete hello install.</span></span>
   
    ![Snímek obrazovky okna Správa balíčků NuGet hello, s hello Microsoft Azure DocumentDB Client Library zvýrazněná](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      <span data-ttu-id="28431-161">Případně můžete také použít hello balíček hello tooinstall Konzola správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="28431-161">Alternatively you can use hello Package Manager Console tooinstall hello package.</span></span> <span data-ttu-id="28431-162">toodo Ano, na hello **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet**a potom klikněte na **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="28431-162">toodo so, on hello **Tools** menu, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span> <span data-ttu-id="28431-163">Hello řádku zadejte následující hello.</span><span class="sxs-lookup"><span data-stu-id="28431-163">At hello prompt, type hello following.</span></span>
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. <span data-ttu-id="28431-164">Po instalaci balíčku hello řešení sady Visual Studio by měla vypadat přibližně následující hello se dvěma přidanými referencemi, Microsoft.Azure.Documents.Client a Newtonsoft.Json.</span><span class="sxs-lookup"><span data-stu-id="28431-164">Once hello package is installed, your Visual Studio solution should resemble hello following with two new references added, Microsoft.Azure.Documents.Client and Newtonsoft.Json.</span></span>
   
    ![Snímek obrazovky hello dva odkazy přidat datový projekt JSON toohello v Průzkumníku řešení](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <span data-ttu-id="28431-166"><a name="_Toc395637763"></a>Krok 4: Nastavení hello aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="28431-166"><a name="_Toc395637763"></a>Step 4: Set up hello ASP.NET MVC application</span></span>
<span data-ttu-id="28431-167">Nyní Pojďme přidat aplikace MVC toothis hello pro modely, zobrazení a kontrolery:</span><span class="sxs-lookup"><span data-stu-id="28431-167">Now let's add hello models, views, and controllers toothis MVC application:</span></span>

* <span data-ttu-id="28431-168">[Přidání modelu](#_Toc395637764)</span><span class="sxs-lookup"><span data-stu-id="28431-168">[Add a model](#_Toc395637764).</span></span>
* <span data-ttu-id="28431-169">[Přidání kontroleru](#_Toc395637765)</span><span class="sxs-lookup"><span data-stu-id="28431-169">[Add a controller](#_Toc395637765).</span></span>
* <span data-ttu-id="28431-170">[Přidání zobrazení](#_Toc395637766)</span><span class="sxs-lookup"><span data-stu-id="28431-170">[Add views](#_Toc395637766).</span></span>

### <span data-ttu-id="28431-171"><a name="_Toc395637764"></a>Přidání datového modelu JSON</span><span class="sxs-lookup"><span data-stu-id="28431-171"><a name="_Toc395637764"></a>Add a JSON data model</span></span>
<span data-ttu-id="28431-172">Začněme vytvořením hello **M** v MVC, hello modelu.</span><span class="sxs-lookup"><span data-stu-id="28431-172">Let's begin by creating hello **M** in MVC, hello model.</span></span> 

1. <span data-ttu-id="28431-173">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **modely** složku, klikněte na tlačítko **přidat**a potom klikněte na **– třída**.</span><span class="sxs-lookup"><span data-stu-id="28431-173">In **Solution Explorer**, right-click hello **Models** folder, click **Add**, and then click **Class**.</span></span>
   
      <span data-ttu-id="28431-174">Hello **přidat novou položku** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="28431-174">hello **Add New Item** dialog box appears.</span></span>
2. <span data-ttu-id="28431-175">Pojmenujte svou novou třídu **Item.cs** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="28431-175">Name your new class **Item.cs** and click **Add**.</span></span> 
3. <span data-ttu-id="28431-176">Do tohoto nového **Item.cs** soubor, přidejte následující hello po hello poslední *pomocí příkazu*.</span><span class="sxs-lookup"><span data-stu-id="28431-176">In this new **Item.cs** file, add hello following after hello last *using statement*.</span></span>
   
        using Newtonsoft.Json;
4. <span data-ttu-id="28431-177">Nyní nahraďte tento kód</span><span class="sxs-lookup"><span data-stu-id="28431-177">Now replace this code</span></span> 
   
        public class Item
        {
        }
   
    <span data-ttu-id="28431-178">s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="28431-178">with hello following code.</span></span>
   
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
   
    <span data-ttu-id="28431-179">Všechna data v Azure Cosmos DB je předán přes přenosu hello a uloží jako dokumenty JSON.</span><span class="sxs-lookup"><span data-stu-id="28431-179">All data in Azure Cosmos DB is passed over hello wire and stored as JSON.</span></span> <span data-ttu-id="28431-180">způsob hello toocontrol jsou vaše objekty serializují a deserializují technologií JSON.NET, můžete použít hello **JsonProperty** atributu, jak je předvedeno v hello **položky** kterou jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="28431-180">toocontrol hello way your objects are serialized/deserialized by JSON.NET you can use hello **JsonProperty** attribute as demonstrated in hello **Item** class we just created.</span></span> <span data-ttu-id="28431-181">Ne **mít** toodo to ale chcete tooensure, že naše vlastnosti dodržují hello JSON camelCase konvence vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="28431-181">You don't **have** toodo this but I want tooensure that my properties follow hello JSON camelCase naming conventions.</span></span> 
   
    <span data-ttu-id="28431-182">Ne jenom můžete určovat formát názvu vlastnosti hello hello když přejde do formátu JSON, ale můžete zcela přejmenovat vlastnosti .NET, jako jsme to udělali s hello **popis** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="28431-182">Not only can you control hello format of hello property name when it goes into JSON, but you can entirely rename your .NET properties like I did with hello **Description** property.</span></span> 

### <span data-ttu-id="28431-183"><a name="_Toc395637765"></a>Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="28431-183"><a name="_Toc395637765"></a>Add a controller</span></span>
<span data-ttu-id="28431-184">Který má na starosti hello **M**, teď Vytvořme hello **C** v MVC, třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="28431-184">That takes care of hello **M**, now let's create hello **C** in MVC, a controller class.</span></span>

1. <span data-ttu-id="28431-185">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **řadiče** složku, klikněte na tlačítko **přidat**a potom klikněte na **řadiče**.</span><span class="sxs-lookup"><span data-stu-id="28431-185">In **Solution Explorer**, right-click hello **Controllers** folder, click **Add**, and then click **Controller**.</span></span>
   
    <span data-ttu-id="28431-186">Hello **přidat vygenerované uživatelské rozhraní** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="28431-186">hello **Add Scaffold** dialog box appears.</span></span>
2. <span data-ttu-id="28431-187">Vyberte **Kontroler MVC 5 – prázdný** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="28431-187">Select **MVC 5 Controller - Empty** and then click **Add**.</span></span>
   
    ![Snímek obrazovky znázorňující dialogové okno Přidat vygenerované uživatelské rozhraní hello s hello kontroler MVC 5 – prázdný zvýrazněnou možností](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. <span data-ttu-id="28431-189">Pojmenujte nový kontroler, **ItemController**.</span><span class="sxs-lookup"><span data-stu-id="28431-189">Name your new Controller, **ItemController.**</span></span>
   
    ![Snímek obrazovky dialogového okna Přidat kontroler hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    <span data-ttu-id="28431-191">Po vytvoření souboru hello řešení sady Visual Studio by měla vypadat přibližně následující hello s hello novým souborem ItemController.cs v **Průzkumníku řešení**.</span><span class="sxs-lookup"><span data-stu-id="28431-191">Once hello file is created, your Visual Studio solution should resemble hello following with hello new ItemController.cs file in **Solution Explorer**.</span></span> <span data-ttu-id="28431-192">také se zobrazí nový soubor Item.cs Hello vytvořený dříve.</span><span class="sxs-lookup"><span data-stu-id="28431-192">hello new Item.cs file created earlier is also shown.</span></span>
   
    ![Snímek obrazovky znázorňující hello řešení Visual Studio – Průzkumník řešení se hello novým souborem ItemController.cs a Item.cs](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    <span data-ttu-id="28431-194">Soubor ItemController.cs můžete zavřít, vrátíme tooit později.</span><span class="sxs-lookup"><span data-stu-id="28431-194">You can close ItemController.cs, we'll come back tooit later.</span></span> 

### <span data-ttu-id="28431-195"><a name="_Toc395637766"></a>Přidání zobrazení</span><span class="sxs-lookup"><span data-stu-id="28431-195"><a name="_Toc395637766"></a>Add views</span></span>
<span data-ttu-id="28431-196">Nyní vytvoříme hello **V** v MVC, hello zobrazení:</span><span class="sxs-lookup"><span data-stu-id="28431-196">Now, let's create hello **V** in MVC, hello views:</span></span>

* <span data-ttu-id="28431-197">[Přidání zobrazení Index položky](#AddItemIndexView)</span><span class="sxs-lookup"><span data-stu-id="28431-197">[Add an Item Index view](#AddItemIndexView).</span></span>
* <span data-ttu-id="28431-198">[Přidání zobrazení Nová položka](#AddNewIndexView)</span><span class="sxs-lookup"><span data-stu-id="28431-198">[Add a New Item view](#AddNewIndexView).</span></span>
* <span data-ttu-id="28431-199">[Přidání zobrazení Upravit položku](#_Toc395888515)</span><span class="sxs-lookup"><span data-stu-id="28431-199">[Add an Edit Item view](#_Toc395888515).</span></span>

#### <span data-ttu-id="28431-200"><a name="AddItemIndexView"></a>Přidání zobrazení Index položky</span><span class="sxs-lookup"><span data-stu-id="28431-200"><a name="AddItemIndexView"></a>Add an Item Index view</span></span>
1. <span data-ttu-id="28431-201">V **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku, klikněte pravým tlačítkem na hello prázdný **položky** složce vytvořené sadě Visual Studio pro vás při přidání hello  **ItemController** dříve, klikněte na tlačítko **přidat**a potom klikněte na **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="28431-201">In **Solution Explorer**, expand hello **Views**  folder, right-click hello empty **Item** folder that Visual Studio created for you when you added hello **ItemController** earlier, click **Add**, and then click **View**.</span></span>
   
    ![Snímek obrazovky Průzkumník řešení zobrazující hello položky složky, vytvořené v sadě Visual Studio se zvýrazněnými příkazy přidat zobrazení hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. <span data-ttu-id="28431-203">V hello **přidat zobrazení** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="28431-203">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="28431-204">V hello **název zobrazení** zadejte ***Index***.</span><span class="sxs-lookup"><span data-stu-id="28431-204">In hello **View name** box, type ***Index***.</span></span>
   * <span data-ttu-id="28431-205">V hello **šablony** vyberte ***seznamu***.</span><span class="sxs-lookup"><span data-stu-id="28431-205">In hello **Template** box, select ***List***.</span></span>
   * <span data-ttu-id="28431-206">V hello **třída modelu** vyberte ***položky (todo. Modely)***.</span><span class="sxs-lookup"><span data-stu-id="28431-206">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="28431-207">Zadejte pole stránky rozložení hello ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="28431-207">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
     
   ![Dialogové okno hello přidat zobrazení snímek obrazovky zobrazující](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. <span data-ttu-id="28431-209">Jakmile jsou všechny tyto hodnoty nastaveny, klikněte na **Přidat** a nechejte Visual Studio vytvořit novou šablonu zobrazení.</span><span class="sxs-lookup"><span data-stu-id="28431-209">Once all these values are set, click **Add** and let Visual Studio create a new template view.</span></span> <span data-ttu-id="28431-210">Až se to dokončí, otevře se hello soubor cshtml, který byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="28431-210">Once it is done, it will open hello cshtml file  that was created.</span></span> <span data-ttu-id="28431-211">Tento soubor v sadě Visual Studio jsme můžete zavřít, protože jsme se vraťte tooit později.</span><span class="sxs-lookup"><span data-stu-id="28431-211">We can close that file in Visual Studio as we will come back tooit later.</span></span>

#### <span data-ttu-id="28431-212"><a name="AddNewIndexView"></a>Přidání zobrazení Nová položka</span><span class="sxs-lookup"><span data-stu-id="28431-212"><a name="AddNewIndexView"></a>Add a New Item view</span></span>
<span data-ttu-id="28431-213">Podobně jako toohow jsme vytvořili **Index položky** zobrazení, nyní vytvoříme nové zobrazení pro vytváření nových **položky**.</span><span class="sxs-lookup"><span data-stu-id="28431-213">Similar toohow we created an **Item Index** view, we will now create a new view for creating new **Items**.</span></span>

1. <span data-ttu-id="28431-214">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **položky** znovu, klikněte na složku **přidat**a potom klikněte na **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="28431-214">In **Solution Explorer**, right-click hello **Item** folder again, click **Add**, and then click **View**.</span></span>
2. <span data-ttu-id="28431-215">V hello **přidat zobrazení** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="28431-215">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="28431-216">V hello **název zobrazení** zadejte ***vytvořit***.</span><span class="sxs-lookup"><span data-stu-id="28431-216">In hello **View name** box, type ***Create***.</span></span>
   * <span data-ttu-id="28431-217">V hello **šablony** vyberte ***vytvořit***.</span><span class="sxs-lookup"><span data-stu-id="28431-217">In hello **Template** box, select ***Create***.</span></span>
   * <span data-ttu-id="28431-218">V hello **třída modelu** vyberte ***položky (todo. Modely)***.</span><span class="sxs-lookup"><span data-stu-id="28431-218">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="28431-219">Zadejte pole stránky rozložení hello ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="28431-219">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
   * <span data-ttu-id="28431-220">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="28431-220">Click **Add**.</span></span>
   
#### <span data-ttu-id="28431-221"><a name="_Toc395888515"></a>Přidání zobrazení Upravit položku</span><span class="sxs-lookup"><span data-stu-id="28431-221"><a name="_Toc395888515"></a>Add an Edit Item view</span></span>
<span data-ttu-id="28431-222">A nakonec přidejte jedno poslední zobrazení pro úpravy **položky** v hello stejným způsobem jako předtím.</span><span class="sxs-lookup"><span data-stu-id="28431-222">And finally, add one last view for editing an **Item** in hello same way as before.</span></span>

1. <span data-ttu-id="28431-223">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **položky** znovu, klikněte na složku **přidat**a potom klikněte na **zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="28431-223">In **Solution Explorer**, right-click hello **Item** folder again, click **Add**, and then click **View**.</span></span>
2. <span data-ttu-id="28431-224">V hello **přidat zobrazení** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="28431-224">In hello **Add View** dialog box, do hello following:</span></span>
   
   * <span data-ttu-id="28431-225">V hello **název zobrazení** zadejte ***upravit***.</span><span class="sxs-lookup"><span data-stu-id="28431-225">In hello **View name** box, type ***Edit***.</span></span>
   * <span data-ttu-id="28431-226">V hello **šablony** vyberte ***upravit***.</span><span class="sxs-lookup"><span data-stu-id="28431-226">In hello **Template** box, select ***Edit***.</span></span>
   * <span data-ttu-id="28431-227">V hello **třída modelu** vyberte ***položky (todo. Modely)***.</span><span class="sxs-lookup"><span data-stu-id="28431-227">In hello **Model class** box, select ***Item (todo.Models)***.</span></span>
   * <span data-ttu-id="28431-228">Zadejte pole stránky rozložení hello ***~/Views/Shared/_Layout.cshtml***.</span><span class="sxs-lookup"><span data-stu-id="28431-228">In hello layout page box, type ***~/Views/Shared/_Layout.cshtml***.</span></span>
   * <span data-ttu-id="28431-229">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="28431-229">Click **Add**.</span></span>

<span data-ttu-id="28431-230">Až bude vše Hotovo, zavřete všechny dokumenty cshtml hello v sadě Visual Studio, jako jsme vrátí toothese zobrazení později.</span><span class="sxs-lookup"><span data-stu-id="28431-230">Once this is done, close all hello cshtml documents in Visual Studio as we will return toothese views later.</span></span>

## <span data-ttu-id="28431-231"><a name="_Toc395637769"></a>Krok 5: Připojení služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="28431-231"><a name="_Toc395637769"></a>Step 5: Wiring up Azure Cosmos DB</span></span>
<span data-ttu-id="28431-232">Teď, když hello standardní MVC věcem se stará, můžeme zaměřit tooadding hello kód pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="28431-232">Now that hello standard MVC stuff is taken care of, let's turn tooadding hello code for Azure Cosmos DB.</span></span> 

<span data-ttu-id="28431-233">V této části přidáme kód toohandle hello následující:</span><span class="sxs-lookup"><span data-stu-id="28431-233">In this section, we'll add code toohandle hello following:</span></span>

* <span data-ttu-id="28431-234">[Výpis neúplných položek](#_Toc395637770)</span><span class="sxs-lookup"><span data-stu-id="28431-234">[Listing incomplete Items](#_Toc395637770).</span></span>
* <span data-ttu-id="28431-235">[Přidávání položek](#_Toc395637771)</span><span class="sxs-lookup"><span data-stu-id="28431-235">[Adding Items](#_Toc395637771).</span></span>
* <span data-ttu-id="28431-236">[Úprava položek](#_Toc395637772)</span><span class="sxs-lookup"><span data-stu-id="28431-236">[Editing Items](#_Toc395637772).</span></span>

### <span data-ttu-id="28431-237"><a name="_Toc395637770"></a>Výpis neúplných položek ve webové aplikaci MVC</span><span class="sxs-lookup"><span data-stu-id="28431-237"><a name="_Toc395637770"></a>Listing incomplete Items in your MVC web application</span></span>
<span data-ttu-id="28431-238">Hello první věc toodo tady je přidat třídu, která obsahuje všechny hello logiku tooconnect tooand použití Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="28431-238">hello first thing toodo here is add a class that contains all hello logic tooconnect tooand use Azure Cosmos DB.</span></span> <span data-ttu-id="28431-239">Pro účely tohoto kurzu zapouzdříme všechnu tuto logiku tooa třídy úložiště nazvané DocumentDBRepository.</span><span class="sxs-lookup"><span data-stu-id="28431-239">For this tutorial we'll encapsulate all this logic in tooa repository class called DocumentDBRepository.</span></span> 

1. <span data-ttu-id="28431-240">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello, klikněte na **přidat**a potom klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="28431-240">In **Solution Explorer**, right-click on hello project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="28431-241">Pojmenujte novou třídu hello **DocumentDBRepository** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="28431-241">Name hello new class **DocumentDBRepository** and click **Add**.</span></span>
2. <span data-ttu-id="28431-242">V nově vytvořený hello **DocumentDBRepository** třídu a přidejte následující hello *pomocí příkazů* výše hello *obor názvů* deklarace</span><span class="sxs-lookup"><span data-stu-id="28431-242">In hello newly created **DocumentDBRepository** class and add hello following *using statements* above hello *namespace* declaration</span></span>
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net
        
    <span data-ttu-id="28431-243">Nyní nahraďte tento kód</span><span class="sxs-lookup"><span data-stu-id="28431-243">Now replace this code</span></span> 
   
        public class DocumentDBRepository
        {
        }
   
    <span data-ttu-id="28431-244">s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="28431-244">with hello following code.</span></span>
   
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
   
    
3. <span data-ttu-id="28431-245">Čteme několik hodnot z konfigurace, takže otevřete hello **Web.config** vaší aplikace a přidejte následující řádky pod hello hello `<AppSettings>` části.</span><span class="sxs-lookup"><span data-stu-id="28431-245">We're reading some values from configuration, so open hello **Web.config** file of your application and add hello following lines under hello `<AppSettings>` section.</span></span>
   
        <add key="endpoint" value="enter hello URI from hello Keys blade of hello Azure Portal"/>
        <add key="authKey" value="enter hello PRIMARY KEY, or hello SECONDARY KEY, from hello Keys blade of hello Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. <span data-ttu-id="28431-246">Nyní, aktualizovat hello hodnoty pro *koncový bod* a *authKey* pomocí okna klíče hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="28431-246">Now, update hello values for *endpoint* and *authKey* using hello Keys blade of hello Azure Portal.</span></span> <span data-ttu-id="28431-247">Použít hello **URI** z okna klíče hello jako hodnota hello hello koncový bod nastavení a použití hello **primární klíč**, nebo **sekundární klíč** z okna klíče hello jako hodnota hello hello nastavení authKey.</span><span class="sxs-lookup"><span data-stu-id="28431-247">Use hello **URI** from hello Keys blade as hello value of hello endpoint setting, and use hello **PRIMARY KEY**, or **SECONDARY KEY** from hello Keys blade as hello value of hello authKey setting.</span></span>

    <span data-ttu-id="28431-248">Vyřešeno připojení do úložiště Azure Cosmos DB hello, nyní Pojďme přidat logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="28431-248">That takes care of wiring up hello Azure Cosmos DB repository, now let's add our application logic.</span></span>

1. <span data-ttu-id="28431-249">Hello nejprve thing Chceme mít toodo toobe s aplikaci seznamu úkolů je toodisplay hello neúplné položky.</span><span class="sxs-lookup"><span data-stu-id="28431-249">hello first thing we want toobe able toodo with a todo list application is toodisplay hello incomplete items.</span></span>  <span data-ttu-id="28431-250">Zkopírujte a vložte následující fragment kódu kamkoli v hello hello **DocumentDBRepository** třídy.</span><span class="sxs-lookup"><span data-stu-id="28431-250">Copy and paste hello following code snippet anywhere within hello **DocumentDBRepository** class.</span></span>
   
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
2. <span data-ttu-id="28431-251">Otevřete hello **ItemController** jsme přidali dříve a přidejte následující hello *pomocí příkazů* nad deklaraci oboru názvů hello.</span><span class="sxs-lookup"><span data-stu-id="28431-251">Open hello **ItemController** we added earlier and add hello following *using statements* above hello namespace declaration.</span></span>
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    <span data-ttu-id="28431-252">Pokud se váš projekt nejmenuje "todo", budete potřebovat tooupdate pomocí "úkolů. Modely"; tooreflect hello název projektu.</span><span class="sxs-lookup"><span data-stu-id="28431-252">If your project is not named "todo", then you need tooupdate using "todo.Models"; tooreflect hello name of your project.</span></span>
   
    <span data-ttu-id="28431-253">Nyní nahraďte tento kód</span><span class="sxs-lookup"><span data-stu-id="28431-253">Now replace this code</span></span>
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    <span data-ttu-id="28431-254">s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="28431-254">with hello following code.</span></span>
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. <span data-ttu-id="28431-255">Otevřete **Global.asax.cs** a přidejte následující řádek toohello hello **Application_Start** – metoda</span><span class="sxs-lookup"><span data-stu-id="28431-255">Open **Global.asax.cs** and add hello following line toohello **Application_Start** method</span></span> 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

<span data-ttu-id="28431-256">Řešení v tomto okamžiku by měla být schopný toobuild bez chyb.</span><span class="sxs-lookup"><span data-stu-id="28431-256">At this point your solution should be able toobuild without any errors.</span></span>

<span data-ttu-id="28431-257">Pokud jste spustili hello aplikace hned, by se dostala toohello **HomeController** a hello **Index** tohoto kontroleru.</span><span class="sxs-lookup"><span data-stu-id="28431-257">If you ran hello application now, you would go toohello **HomeController** and hello **Index** view of that controller.</span></span> <span data-ttu-id="28431-258">Toto je výchozí chování projektu šablony MVC hello, který jsme zvolili při spuštění hello hello ale nechceme!</span><span class="sxs-lookup"><span data-stu-id="28431-258">This is hello default behavior for hello MVC template project we chose at hello start but we don't want that!</span></span> <span data-ttu-id="28431-259">Nyní změníme hello směrování na tento tooalter aplikace MVC toto chování.</span><span class="sxs-lookup"><span data-stu-id="28431-259">Let's change hello routing on this MVC application tooalter this behavior.</span></span>

<span data-ttu-id="28431-260">Otevřete ***aplikace\_Start\RouteConfig.cs*** a vyhledejte řádek hello začíná "výchozí hodnoty:" a změňte ji tooresemble hello následující.</span><span class="sxs-lookup"><span data-stu-id="28431-260">Open ***App\_Start\RouteConfig.cs*** and locate hello line starting with "defaults:" and change it tooresemble hello following.</span></span>

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

<span data-ttu-id="28431-261">Tímto bude aplikaci ASP.NET MVC, pokud jste nezadali hodnotu v hello URL toocontrol hello směrování chování místo oznámeno **Domů**, použijte **položky** jako řadič hello a uživatel **Index** jako hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="28431-261">This now tells ASP.NET MVC that if you have not specified a value in hello URL toocontrol hello routing behavior that instead of **Home**, use **Item** as hello controller and user **Index** as hello view.</span></span>

<span data-ttu-id="28431-262">Teď Pokud hello aplikace spustíte, zavolá do vašeho **ItemController** které zavolá v třídě úložiště toohello a použít všechny neúplné položky toohello hello tooreturn metodu getitems, pomocí hello **zobrazení** \\ **Položky**\\**Index** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="28431-262">Now if you run hello application, it will call into your **ItemController** which will call in toohello repository class and use hello GetItems method tooreturn all hello incomplete items toohello **Views**\\**Item**\\**Index** view.</span></span> 

<span data-ttu-id="28431-263">Pokud teď projekt sestavíte a spustíte, mělo by se zobrazit něco přibližně takového.</span><span class="sxs-lookup"><span data-stu-id="28431-263">If you build and run this project now, you should now see something that looks this.</span></span>    

![Snímek obrazovky hello webové aplikace vytvořené v tomto kurzu databáze](./media/documentdb-dotnet-application/build-and-run-the-project-now.png)

### <span data-ttu-id="28431-265"><a name="_Toc395637771"></a>Přidávání položek</span><span class="sxs-lookup"><span data-stu-id="28431-265"><a name="_Toc395637771"></a>Adding Items</span></span>
<span data-ttu-id="28431-266">Umožňuje uvést některé položky do databáze hotový, abychom měli něco víc než prázdnou mřížkou toolook v.</span><span class="sxs-lookup"><span data-stu-id="28431-266">Let's put some items into our database so we have something more than an empty grid toolook at.</span></span>

<span data-ttu-id="28431-267">Přidejme nějaký kód příliš Azure Cosmos DBRepository a ItemController toopersist hello záznamu v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="28431-267">Let's add some code too Azure Cosmos DBRepository and ItemController toopersist hello record in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="28431-268">Přidejte následující metodu tooyour hello **DocumentDBRepository** třídy.</span><span class="sxs-lookup"><span data-stu-id="28431-268">Add hello following method tooyour **DocumentDBRepository** class.</span></span>
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   <span data-ttu-id="28431-269">Tato metoda jednoduše přijme předaný tooit objekt a zachová jej v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="28431-269">This method simply takes an object passed tooit and persists it in Azure Cosmos DB.</span></span>
2. <span data-ttu-id="28431-270">Otevřete soubor ItemController.cs hello a přidejte následující fragment kódu v rámci třídy hello hello.</span><span class="sxs-lookup"><span data-stu-id="28431-270">Open hello ItemController.cs file and add hello following code snippet within hello class.</span></span> <span data-ttu-id="28431-271">Jedná se jak ASP.NET MVC ví, co toodo pro hello **vytvořit** akce.</span><span class="sxs-lookup"><span data-stu-id="28431-271">This is how ASP.NET MVC knows what toodo for hello **Create** action.</span></span> <span data-ttu-id="28431-272">V takovém případě jenom vykreslení hello přidružené zobrazení Create.cshtml vytvořené dříve.</span><span class="sxs-lookup"><span data-stu-id="28431-272">In this case just render hello associated Create.cshtml view created earlier.</span></span>
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    <span data-ttu-id="28431-273">Nyní potřebujeme některé další kód v tomto kontroleru, který bude přijímat odeslání hello z hello **vytvořit** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="28431-273">We now need some more code in this controller that will accept hello submission from hello **Create** view.</span></span>
3. <span data-ttu-id="28431-274">Přidejte hello další blok kódu toohello třídy ItemController.cs, který technologii ASP.NET MVC řekne, co toodo s operací POST formuláře pro tento kontroler.</span><span class="sxs-lookup"><span data-stu-id="28431-274">Add hello next block of code toohello ItemController.cs class that tells ASP.NET MVC what toodo with a form POST for this controller.</span></span>
   
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
   
    <span data-ttu-id="28431-275">Tento kód zavolá toohello DocumentDBRepository a použije hello CreateItemAsync metoda toopersist hello nové todo položky toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="28431-275">This code calls in toohello DocumentDBRepository and uses hello CreateItemAsync method toopersist hello new todo item toohello database.</span></span> 
   
    <span data-ttu-id="28431-276">**Poznámka k zabezpečení**: hello **ValidateAntiForgeryToken** je použit atribut zde toohelp zabezpečit tuto aplikace před útoky na padělání požadavku posílaného mezi weby.</span><span class="sxs-lookup"><span data-stu-id="28431-276">**Security Note**: hello **ValidateAntiForgeryToken** attribute is used here toohelp protect this application against cross-site request forgery attacks.</span></span> <span data-ttu-id="28431-277">Existuje více tooit jen přidat tento atribut, vaše zobrazení musí toowork s tímto tokenem proti padělání také.</span><span class="sxs-lookup"><span data-stu-id="28431-277">There is more tooit than just adding this attribute, your views need toowork with this anti-forgery token as well.</span></span> <span data-ttu-id="28431-278">Další informace o subjektu hello a příklady, jak tooimplement to správně, najdete v tématu [brání padělání požadavku webů][Preventing Cross-Site Request Forgery].</span><span class="sxs-lookup"><span data-stu-id="28431-278">For more on hello subject, and examples of how tooimplement this correctly, please see [Preventing Cross-Site Request Forgery][Preventing Cross-Site Request Forgery].</span></span> <span data-ttu-id="28431-279">Hello zdrojový kód dostupný na [Githubu] [ GitHub] má hello úplnou implementaci na místě.</span><span class="sxs-lookup"><span data-stu-id="28431-279">hello source code provided on [GitHub][GitHub] has hello full implementation in place.</span></span>
   
    <span data-ttu-id="28431-280">**Poznámka k zabezpečení**: hello používáme **vazby** atribut hello metoda parametr toohelp chránit před útoky typu overpost.</span><span class="sxs-lookup"><span data-stu-id="28431-280">**Security Note**: We also use hello **Bind** attribute on hello method parameter toohelp protect against over-posting attacks.</span></span> <span data-ttu-id="28431-281">Další informace najdete v tématu [Základní operace CRUD v ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].</span><span class="sxs-lookup"><span data-stu-id="28431-281">For more details please see [Basic CRUD Operations in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].</span></span>

<span data-ttu-id="28431-282">Tím končí hello vyžaduje kód tooadd nové položky tooour databáze.</span><span class="sxs-lookup"><span data-stu-id="28431-282">This concludes hello code required tooadd new Items tooour database.</span></span>

### <span data-ttu-id="28431-283"><a name="_Toc395637772"></a>Úprava položek</span><span class="sxs-lookup"><span data-stu-id="28431-283"><a name="_Toc395637772"></a>Editing Items</span></span>
<span data-ttu-id="28431-284">Je pro nás jednu věc toodo a který je tooadd hello možnost tooedit **položky** v databázi hello a toomark je jako dokončit.</span><span class="sxs-lookup"><span data-stu-id="28431-284">There is one last thing for us toodo, and that is tooadd hello ability tooedit **Items** in hello database and toomark them as complete.</span></span> <span data-ttu-id="28431-285">Hello zobrazení pro úpravy již byl přidán toohello projektu, takže potřebujeme tooadd některé kódu tooour řadiče a toohello **DocumentDBRepository** třídy znovu.</span><span class="sxs-lookup"><span data-stu-id="28431-285">hello view for editing was already added toohello project, so we just need tooadd some code tooour controller and toohello **DocumentDBRepository** class again.</span></span>

1. <span data-ttu-id="28431-286">Přidejte následující toohello hello **DocumentDBRepository** třídy.</span><span class="sxs-lookup"><span data-stu-id="28431-286">Add hello following toohello **DocumentDBRepository** class.</span></span>
   
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
   
    <span data-ttu-id="28431-287">první z těchto metod Hello **GetItem** načítá položku z databáze Cosmos Azure, která se předá zpět toohello **ItemController** a pak na toohello **upravit** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="28431-287">hello first of these methods, **GetItem** fetches an Item from Azure Cosmos DB which is passed back toohello **ItemController** and then on toohello **Edit** view.</span></span>
   
    <span data-ttu-id="28431-288">Hello sekundu metod hello jsme právě přidali nahradí hello **dokumentu** v Azure DB Cosmos hello verzi hello **dokumentu** předané z hello **ItemController**.</span><span class="sxs-lookup"><span data-stu-id="28431-288">hello second of hello methods we just added replaces hello **Document** in Azure Cosmos DB with hello version of hello **Document** passed in from hello **ItemController**.</span></span>
2. <span data-ttu-id="28431-289">Přidejte následující toohello hello **ItemController** třídy.</span><span class="sxs-lookup"><span data-stu-id="28431-289">Add hello following toohello **ItemController** class.</span></span>
   
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
   
    <span data-ttu-id="28431-290">Hello první metoda zpracovává hello GET protokolu Http, který se stane, když hello uživatel klikne na hello **upravit** odkaz z hello **Index** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="28431-290">hello first method handles hello Http GET that happens when hello user clicks on hello **Edit** link from hello **Index** view.</span></span> <span data-ttu-id="28431-291">Tato metoda načítá [ **dokumentu** ](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) z Azure Cosmos databáze a předává je toohello **upravit** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="28431-291">This method fetches a [**Document**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) from Azure Cosmos DB and passes it toohello **Edit** view.</span></span>
   
    <span data-ttu-id="28431-292">Hello **upravit** zobrazení pak provede Http POST toohello **Indexcontrolleru**.</span><span class="sxs-lookup"><span data-stu-id="28431-292">hello **Edit** view will then do an Http POST toohello **IndexController**.</span></span> 
   
    <span data-ttu-id="28431-293">druhý metoda Hello jsme přidali, zpracovává předávání hello aktualizovat objekt tooAzure Cosmos DB toobe jako trvalý v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="28431-293">hello second method we added handles passing hello updated object tooAzure Cosmos DB toobe persisted in hello database.</span></span>

<span data-ttu-id="28431-294">Je to, který je všechno, co potřebujeme toorun naše aplikace, vypsání neúplné **položky**, přidat nové **položky**a upravte **položky**.</span><span class="sxs-lookup"><span data-stu-id="28431-294">That's it, that is everything we need toorun our application, list incomplete **Items**, add new **Items**, and edit **Items**.</span></span>

## <span data-ttu-id="28431-295"><a name="_Toc395637773"></a>Krok 6: Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="28431-295"><a name="_Toc395637773"></a>Step 6: Run hello application locally</span></span>
<span data-ttu-id="28431-296">aplikace hello tootest na místním počítači, hello následující:</span><span class="sxs-lookup"><span data-stu-id="28431-296">tootest hello application on your local machine, do hello following:</span></span>

1. <span data-ttu-id="28431-297">Stiskněte F5 v sadě Visual Studio toobuild hello aplikace v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="28431-297">Hit F5 in Visual Studio toobuild hello application in debug mode.</span></span> <span data-ttu-id="28431-298">Měl by sestavení aplikace hello a spustí prohlížeč s stránku hello prázdnou mřížkou, kterou jsme viděli dříve:</span><span class="sxs-lookup"><span data-stu-id="28431-298">It should build hello application and launch a browser with hello empty grid page we saw before:</span></span>
   
    ![Snímek obrazovky hello webové aplikace vytvořené v tomto kurzu databáze](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. <span data-ttu-id="28431-300">Klikněte na tlačítko hello **vytvořit nový** propojení a přidejte hodnoty toohello **název** a **popis** pole.</span><span class="sxs-lookup"><span data-stu-id="28431-300">Click hello **Create New** link and add values toohello **Name** and **Description** fields.</span></span> <span data-ttu-id="28431-301">Nechte hello **dokončeno** zaškrtnutí políčka zrušit jinak hello nové **položky** přidána ve stavu dokončení a nezobrazí se na počáteční seznam hello.</span><span class="sxs-lookup"><span data-stu-id="28431-301">Leave hello **Completed** check box unselected otherwise hello new **Item** will be added in a completed state and will not appear on hello initial list.</span></span>
   
    ![Snímek obrazovky zobrazení pro vytváření hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. <span data-ttu-id="28431-303">Klikněte na tlačítko **vytvořit** a jsou přesměrovaného back toohello **Index** zobrazení a **položky** se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="28431-303">Click **Create** and you are redirected back toohello **Index** view and your **Item** appears in hello list.</span></span>
   
    ![Snímek obrazovky zobrazení Index hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    <span data-ttu-id="28431-305">Působí volné tooadd několik dalších **položky** tooyour seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="28431-305">Feel free tooadd a few more **Items** tooyour todo list.</span></span>
    
4. <span data-ttu-id="28431-306">Klikněte na tlačítko **upravit** další tooan **položky** na seznamu hello a jsou přesměrováni toohello **upravit** zobrazení, kde můžete aktualizovat jakoukoli vlastnost objektu, včetně hello  **Dokončit** příznak.</span><span class="sxs-lookup"><span data-stu-id="28431-306">Click **Edit** next tooan **Item** on hello list and you are taken toohello **Edit** view where you can update any property of your object, including hello **Completed** flag.</span></span> <span data-ttu-id="28431-307">Pokud označíte hello **Complete** příznak a klikněte na tlačítko **Uložit**, hello **položky** se odebral ze seznamu hello neúplných úkolů.</span><span class="sxs-lookup"><span data-stu-id="28431-307">If you mark hello **Complete** flag and click **Save**, hello **Item** is removed from hello list of incomplete tasks.</span></span>
   
    ![Snímek obrazovky zobrazení Index se zaškrtnutým políčkem dokončeno hello hello](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. <span data-ttu-id="28431-309">Jakmile otestujete aplikace hello, stiskněte Ctrl + F5 toostop ladění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="28431-309">Once you've tested hello app, press Ctrl+F5 toostop debugging hello app.</span></span> <span data-ttu-id="28431-310">Jste připravené toodeploy!</span><span class="sxs-lookup"><span data-stu-id="28431-310">You're ready toodeploy!</span></span>

## <span data-ttu-id="28431-311"><a name="_Toc395637774"></a>Krok 7: Nasazení tooAzure hello aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="28431-311"><a name="_Toc395637774"></a>Step 7: Deploy hello application tooAzure App Service</span></span> 
<span data-ttu-id="28431-312">Teď, když máte hotové aplikace hello správně funguje s Azure Cosmos DB vytvoříme toodeploy tooAzure této webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="28431-312">Now that you have hello complete application working correctly with Azure Cosmos DB we're going toodeploy this web app tooAzure App Service.</span></span>  

1. <span data-ttu-id="28431-313">Klikněte pravým tlačítkem na projekt hello v toopublish potřebujete toodo všechny této aplikace je **Průzkumníku řešení** a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="28431-313">toopublish this application all you need toodo is right-click on hello project in **Solution Explorer** and click **Publish**.</span></span>
   
    ![Snímek obrazovky hello možností publikovat v Průzkumníkovi řešení](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. <span data-ttu-id="28431-315">V hello **publikovat** dialogové okno, klikněte na tlačítko **Microsoft Azure App Service**, pak vyberte **vytvořit nový** toocreate aplikační službu profil, nebo klikněte na tlačítko **vyberte Existující** toouse stávající profil.</span><span class="sxs-lookup"><span data-stu-id="28431-315">In hello **Publish** dialog box, click **Microsoft Azure App Service**, then select **Create New** toocreate an App Service profile, or click **Select Existing** toouse an existing profile.</span></span>

    ![Dialogové okno publikování v sadě Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. <span data-ttu-id="28431-317">Pokud máte existující profil Azure App Service, zadejte název vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="28431-317">If you have an existing Azure App Service profile, enter your subscription name.</span></span> <span data-ttu-id="28431-318">Použití hello **zobrazení** filtrovat toosort skupinu prostředků nebo typ prostředku a potom vyberte Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="28431-318">Use hello **View** filter toosort by resource group or resource type, then select your Azure App Service.</span></span> 
   
    ![Dialogové okno služby App Service v sadě Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. <span data-ttu-id="28431-320">Klikněte na tlačítko toocreate nový profil Azure App Service, **vytvořit nový** v hello **publikovat** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="28431-320">toocreate a new Azure App Service profile, click **Create New** in hello **Publish** dialog box.</span></span> <span data-ttu-id="28431-321">V hello **vytvořit službu App Service** dialogové okno, zadejte název webové aplikace a příslušné předplatné, skupinu prostředků a plán služby App Service, a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="28431-321">In hello **Create App Service** dialog, enter your Web App name and appropriate subscription, resource group, and App Service plan, then click **Create**.</span></span>

    ![Služby App Service dialogové okno vytvořit v sadě Visual Studio](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

<span data-ttu-id="28431-323">Během pár sekund bude Visual Studio dokončí publikování webové aplikace a spustí prohlížeč, kde uvidíte vaše handiwork běžící v Azure!</span><span class="sxs-lookup"><span data-stu-id="28431-323">In a few seconds, Visual Studio will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>



## <span data-ttu-id="28431-324"><a name="_Toc395637775"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="28431-324"><a name="_Toc395637775"></a>Next steps</span></span>
<span data-ttu-id="28431-325">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="28431-325">Congratulations!</span></span> <span data-ttu-id="28431-326">Právě vytvořené webové aplikace pomocí Azure Cosmos DB první rozhraní ASP.NET MVC a publikovali jste ji tooAzure.</span><span class="sxs-lookup"><span data-stu-id="28431-326">You just built your first ASP.NET MVC web application using Azure Cosmos DB and published it tooAzure.</span></span> <span data-ttu-id="28431-327">zdrojový kód pro hello hotové aplikace včetně podrobností hello Hello a odstranit funkce, které nebyly zahrnuty v tomto kurzu lze stáhnout nebo naklonovat z [Githubu][GitHub].</span><span class="sxs-lookup"><span data-stu-id="28431-327">hello source code for hello complete application, including hello detail and delete functionality that were not included in this tutorial can be downloaded or cloned from [GitHub][GitHub].</span></span> <span data-ttu-id="28431-328">Takže pokud vás zajímá přidání tooyour aplikaci, získat kód hello a přidejte ho toothis aplikace.</span><span class="sxs-lookup"><span data-stu-id="28431-328">So if you're interested in adding that tooyour app, grab hello code and add it toothis app.</span></span>

<span data-ttu-id="28431-329">tooadd další funkce tooyour aplikaci, zkontrolujte hello rozhraní API dostupná v hello [knihovny .NET DB Cosmos Azure](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) a myslíte, že volné toocontribute toohello knihovny .NET DB Cosmos Azure na [Githubu] [GitHub].</span><span class="sxs-lookup"><span data-stu-id="28431-329">tooadd additional functionality tooyour application, review hello APIs available in hello [Azure Cosmos DB .NET Library](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) and feel free toocontribute toohello Azure Cosmos DB .NET Library on [GitHub][GitHub].</span></span> 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
