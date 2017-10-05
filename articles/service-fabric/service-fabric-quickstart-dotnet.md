---
title: "Vytvoření aplikace .NET Service Fabric v Azure | Microsoft Docs"
description: "Vytvořte aplikaci .NET pro Azure pomocí úvodní ukázka Service Fabric."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: d11b9af982112db8ba94b62110c18be843f1abb1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-net-service-fabric-application-in-azure"></a><span data-ttu-id="a9448-103">Vytvoření aplikace .NET Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="a9448-103">Create a .NET Service Fabric application in Azure</span></span>
<span data-ttu-id="a9448-104">Azure Service Fabric je platforma distribuovaných systémů pro nasazování a správu škálovatelných a spolehlivých mikroslužeb a kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="a9448-104">Azure Service Fabric is a distributed systems platform for deploying and managing scalable and reliable microservices and containers.</span></span> 

<span data-ttu-id="a9448-105">Tento rychlý start ukazuje, jak nasadit vaší první aplikace .NET do Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a9448-105">This quickstart shows how to deploy your first .NET application to Service Fabric.</span></span> <span data-ttu-id="a9448-106">Jakmile budete hotovi, máte hlasovací aplikaci s ASP.NET Core web, který je front-end, který uloží výsledků hlasování ve stavové služby back-end v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a9448-106">When you're finished, you have a voting application with an ASP.NET Core web front-end that saves voting results in a stateful back-end service in the cluster.</span></span>

![Snímek obrazovky aplikace](./media/service-fabric-quickstart-dotnet/application-screenshot.png)

<span data-ttu-id="a9448-108">Pomocí této aplikace se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="a9448-108">Using this application you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="a9448-109">Vytvoření aplikace pomocí rozhraní .NET a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a9448-109">Create an application using .NET and Service Fabric</span></span>
> * <span data-ttu-id="a9448-110">Pomocí ASP.NET core jako webového front-endu</span><span class="sxs-lookup"><span data-stu-id="a9448-110">Use ASP.NET core as a web front-end</span></span>
> * <span data-ttu-id="a9448-111">Ukládání dat aplikací ve stavové služby</span><span class="sxs-lookup"><span data-stu-id="a9448-111">Store application data in a stateful service</span></span>
> * <span data-ttu-id="a9448-112">Ladění aplikace místně</span><span class="sxs-lookup"><span data-stu-id="a9448-112">Debug your application locally</span></span>
> * <span data-ttu-id="a9448-113">Nasaďte aplikaci do clusteru s podporou v Azure</span><span class="sxs-lookup"><span data-stu-id="a9448-113">Deploy the application to a cluster in Azure</span></span>
> * <span data-ttu-id="a9448-114">Škálování aplikací mezi několika uzly</span><span class="sxs-lookup"><span data-stu-id="a9448-114">Scale-out the application across multiple nodes</span></span>
> * <span data-ttu-id="a9448-115">Provedení postupného upgradu aplikace</span><span class="sxs-lookup"><span data-stu-id="a9448-115">Perform a rolling application upgrade</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9448-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a9448-116">Prerequisites</span></span>
<span data-ttu-id="a9448-117">K provedení kroků v tomto kurzu Rychlý start je potřeba:</span><span class="sxs-lookup"><span data-stu-id="a9448-117">To complete this quickstart:</span></span>
1. <span data-ttu-id="a9448-118">[Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) s **Azure development** a **ASP.NET a webové vývoj** úlohy.</span><span class="sxs-lookup"><span data-stu-id="a9448-118">[Install Visual Studio 2017](https://www.visualstudio.com/) with the **Azure development** and **ASP.NET and web development** workloads.</span></span>
2. <span data-ttu-id="a9448-119">[Nainstalovat Git](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="a9448-119">[Install Git](https://git-scm.com/)</span></span>
3. [<span data-ttu-id="a9448-120">Nainstalovat Microsoft Azure Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="a9448-120">Install the Microsoft Azure Service Fabric SDK</span></span>](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK)
4. <span data-ttu-id="a9448-121">Spusťte následující příkaz, který povolit Visual Studio k nasazení na místní cluster Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="a9448-121">Run the following command to enable Visual Studio to deploy to the local Service Fabric cluster:</span></span>
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
    ```

## <a name="download-the-sample"></a><span data-ttu-id="a9448-122">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="a9448-122">Download the sample</span></span>
<span data-ttu-id="a9448-123">V příkazovém okně spusťte následující příkaz, který klonovat úložiště ukázkové aplikace do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="a9448-123">In a command window, run the following command to clone the sample app repository to your local machine.</span></span>
```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="run-the-application-locally"></a><span data-ttu-id="a9448-124">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="a9448-124">Run the application locally</span></span>
<span data-ttu-id="a9448-125">Klikněte pravým tlačítkem na ikonu sady Visual Studio v nabídce Start a vyberte **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="a9448-125">Right-click the Visual Studio icon in the Start Menu and choose **Run as administrator**.</span></span> <span data-ttu-id="a9448-126">Aby bylo možné připojit ladicí program k vašim službám, budete muset spustit sadu Visual Studio jako správce.</span><span class="sxs-lookup"><span data-stu-id="a9448-126">In order to attach the debugger to your services, you need to run Visual Studio as administrator.</span></span>

<span data-ttu-id="a9448-127">Otevřete **Voting.sln** řešení sady Visual Studio z úložiště, které jste naklonovali.</span><span class="sxs-lookup"><span data-stu-id="a9448-127">Open the **Voting.sln** Visual Studio solution from the repository you cloned.</span></span>

<span data-ttu-id="a9448-128">Chcete-li nasadit aplikaci, stiskněte **F5**.</span><span class="sxs-lookup"><span data-stu-id="a9448-128">To deploy the application, press **F5**.</span></span>

> [!NOTE]
> <span data-ttu-id="a9448-129">Při prvním spuštění a nasazení aplikace, Visual Studio vytvoří místní cluster pro ladění.</span><span class="sxs-lookup"><span data-stu-id="a9448-129">The first time you run and deploy the application, Visual Studio creates a local cluster for debugging.</span></span> <span data-ttu-id="a9448-130">Tato operace může chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="a9448-130">This operation may take some time.</span></span> <span data-ttu-id="a9448-131">Stav vytváření clusteru se zobrazí v okně výstupu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9448-131">The cluster creation status is displayed in the Visual Studio output window.</span></span>

<span data-ttu-id="a9448-132">Po dokončení nasazení se spustí prohlížeč a otevřete tuto stránku: `http://localhost:8080` -webové aplikace front-endu.</span><span class="sxs-lookup"><span data-stu-id="a9448-132">When the deployment is complete, launch a browser and open this page: `http://localhost:8080` - the web front-end of the application.</span></span>

![Aplikace front-endu](./media/service-fabric-quickstart-dotnet/application-screenshot-new.png)

<span data-ttu-id="a9448-134">Teď můžete přidat sadu hlasovací tlačítka a spuštění trvá hlasy.</span><span class="sxs-lookup"><span data-stu-id="a9448-134">You can now add a set of voting options, and start taking votes.</span></span> <span data-ttu-id="a9448-135">Aplikace běží a ukládá všechna data v clusteru Service Fabric, bez nutnosti samostatné databáze.</span><span class="sxs-lookup"><span data-stu-id="a9448-135">The application runs and stores all data in your Service Fabric cluster, without the need for a separate database.</span></span>

## <a name="walk-through-the-voting-sample-application"></a><span data-ttu-id="a9448-136">Provede hlasujících ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="a9448-136">Walk through the voting sample application</span></span>
<span data-ttu-id="a9448-137">Hlasovací aplikaci se skládá ze dvou služeb:</span><span class="sxs-lookup"><span data-stu-id="a9448-137">The voting application consists of two services:</span></span>
- <span data-ttu-id="a9448-138">Webová služba front-endu (VotingWeb) – ASP.NET Core webových front-endové služby, která obsluhuje webové stránky a zpřístupňuje rozhraní API pro komunikaci s back-end službu.</span><span class="sxs-lookup"><span data-stu-id="a9448-138">Web front-end service (VotingWeb)- An ASP.NET Core web front-end service, which serves the web page and exposes web APIs to communicate with the backend service.</span></span>
- <span data-ttu-id="a9448-139">Služba back endu (VotingData)-ASP.NET Core webová služba, která zpřístupňuje rozhraní API můžete ukládat výsledky hlas ve slovníku spolehlivé trvalé na disku.</span><span class="sxs-lookup"><span data-stu-id="a9448-139">Back-end service (VotingData)- An ASP.NET Core web service, which exposes an API to store the vote results in a reliable dictionary persisted on disk.</span></span>

![Diagram aplikace](./media/service-fabric-quickstart-dotnet/application-diagram.png)

<span data-ttu-id="a9448-141">Když hlasovat v aplikaci dojít k následujícím událostem:</span><span class="sxs-lookup"><span data-stu-id="a9448-141">When you vote in the application the following events occur:</span></span>
1. <span data-ttu-id="a9448-142">JavaScript odešle žádost hlas webovému rozhraní API ve front-endové webové službě jako požadavek HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="a9448-142">A JavaScript sends the vote request to the web API in the web front-end service as an HTTP PUT request.</span></span>

2. <span data-ttu-id="a9448-143">Webovou službu front-endu používá proxy server pro vyhledání a předávat požadavek HTTP PUT ve službě back-end.</span><span class="sxs-lookup"><span data-stu-id="a9448-143">The web front-end service uses a proxy to locate and forward an HTTP PUT request to the back-end service.</span></span>

3. <span data-ttu-id="a9448-144">Back endové službě přijímá příchozí požadavky a ukládá aktualizované výsledek v spolehlivé slovník, který získá replikují do několika uzly v clusteru a trvalé na disku.</span><span class="sxs-lookup"><span data-stu-id="a9448-144">The back-end service takes the incoming request, and stores the updated result in a reliable dictionary, which gets replicated to multiple nodes within the cluster and persisted on disk.</span></span> <span data-ttu-id="a9448-145">Všechny aplikační data se ukládají v clusteru, takže se žádná databáze.</span><span class="sxs-lookup"><span data-stu-id="a9448-145">All the application's data is stored in the cluster, so no database is needed.</span></span>

## <a name="debug-in-visual-studio"></a><span data-ttu-id="a9448-146">Ladění v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9448-146">Debug in Visual Studio</span></span>
<span data-ttu-id="a9448-147">Při ladění aplikace v sadě Visual Studio, kterou používáte místní cluster Service Fabric vývoj.</span><span class="sxs-lookup"><span data-stu-id="a9448-147">When debugging application in Visual Studio, you are using a local Service Fabric development cluster.</span></span> <span data-ttu-id="a9448-148">Máte možnost upravit prostředí ladění pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="a9448-148">You have the option to adjust your debugging experience to your scenario.</span></span> <span data-ttu-id="a9448-149">V této aplikaci uložíme data v naší službě back-end pomocí slovník spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="a9448-149">In this application, we store data in our back-end service, using a reliable dictionary.</span></span> <span data-ttu-id="a9448-150">Visual Studio odebere aplikaci za výchozí při zastavení ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="a9448-150">Visual Studio removes the application per default when you stop the debugger.</span></span> <span data-ttu-id="a9448-151">Odebrání aplikace způsobí, že data ve službě back-end taky odeberou.</span><span class="sxs-lookup"><span data-stu-id="a9448-151">Removing the application causes the data in the back-end service to also be removed.</span></span> <span data-ttu-id="a9448-152">Chcete-li zachovat data mezi relace ladění, můžete změnit **režim ladění aplikací** jako vlastnost na **Voting** projektu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9448-152">To persist the data between debugging sessions, you can change the **Application Debug Mode** as a property on the **Voting** project in Visual Studio.</span></span>

<span data-ttu-id="a9448-153">Podívat se na co se stane, že v kódu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a9448-153">To look at what happens in the code, complete the following steps:</span></span>
1. <span data-ttu-id="a9448-154">Otevřete **VotesController.cs** souboru a nastavit zarážky ve webové rozhraní API **Put** – metoda (řádku 47) – můžete vyhledat soubor v Průzkumníku řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9448-154">Open the **VotesController.cs** file and set a breakpoint in the web API's **Put** method (line 47) - You can search for the file in the Solution Explorer in Visual Studio.</span></span>

2. <span data-ttu-id="a9448-155">Otevřete **VoteDataController.cs** souboru a nastavit zarážky v tomto rozhraní web API **Put** – metoda (řádku 50).</span><span class="sxs-lookup"><span data-stu-id="a9448-155">Open the **VoteDataController.cs** file and set a breakpoint in this web API's **Put** method (line 50).</span></span>

3. <span data-ttu-id="a9448-156">Přejděte zpět do prohlížeče a klikněte na hlasování možnost nebo přidat novou možnost hlasování.</span><span class="sxs-lookup"><span data-stu-id="a9448-156">Go back to the browser and click a voting option or add a new voting option.</span></span> <span data-ttu-id="a9448-157">Kliknutí na první zarážky do kontroleru webového přední konci na rozhraní api.</span><span class="sxs-lookup"><span data-stu-id="a9448-157">You hit the first breakpoint in the web front-end's api controller.</span></span>
    - <span data-ttu-id="a9448-158">Toto je, kde JavaScript v prohlížeči odešle požadavek na kontroler API web ve službě front-endu.</span><span class="sxs-lookup"><span data-stu-id="a9448-158">This is where the JavaScript in the browser sends a request to the web API controller in the front-end service.</span></span>
    
    ![Přidejte Front-End služby hlas](./media/service-fabric-quickstart-dotnet/addvote-frontend.png)

    - <span data-ttu-id="a9448-160">Nejprve jsme vytvořit adresu URL ReverseProxy pro naši službu back-end **(1)**.</span><span class="sxs-lookup"><span data-stu-id="a9448-160">First we construct the URL to the ReverseProxy for our back-end service **(1)**.</span></span>
    - <span data-ttu-id="a9448-161">Potom jsme poslat PUT požadavek HTTP ReverseProxy **(2)**.</span><span class="sxs-lookup"><span data-stu-id="a9448-161">Then we send the HTTP PUT Request to the ReverseProxy **(2)**.</span></span>
    - <span data-ttu-id="a9448-162">Nakonec vrátíme odpověď z back-end službu do klienta **(3)**.</span><span class="sxs-lookup"><span data-stu-id="a9448-162">Finally the we return the response from the back-end service to the client **(3)**.</span></span>

4. <span data-ttu-id="a9448-163">Stiskněte klávesu **F5** pokračovat</span><span class="sxs-lookup"><span data-stu-id="a9448-163">Press **F5** to continue</span></span>
    - <span data-ttu-id="a9448-164">Nyní jste na zarážce ve službě back-end.</span><span class="sxs-lookup"><span data-stu-id="a9448-164">You are now at the break point in the back-end service.</span></span>
    
    ![Přidat hlas Back-End službu](./media/service-fabric-quickstart-dotnet/addvote-backend.png)

    - <span data-ttu-id="a9448-166">Na prvním řádku v metodě **(1)** používáme `StateManager` nebo přidat spolehlivé slovník nazývaný `counts`.</span><span class="sxs-lookup"><span data-stu-id="a9448-166">In the first line in the method **(1)** we are using the `StateManager` to get or add a reliable dictionary called `counts`.</span></span>
    - <span data-ttu-id="a9448-167">Všechny interakce s hodnotami ve slovníku spolehlivé vyžadují transakci, této konfigurace pomocí příkazu **(2)** vytvoří této transakce.</span><span class="sxs-lookup"><span data-stu-id="a9448-167">All interactions with values in a reliable dictionary require a transaction, this using statement **(2)** creates that transaction.</span></span>
    - <span data-ttu-id="a9448-168">V transakci, jsme potom aktualizujte hodnotu relevantní klíče pro možnost hlasování a provede operaci **(3)**.</span><span class="sxs-lookup"><span data-stu-id="a9448-168">In the transaction, we then update the value of the relevant key for the voting option and commits the operation **(3)**.</span></span> <span data-ttu-id="a9448-169">Po potvrzení metoda vrátí, data aktualizovat ve slovníku a replikovat do jiných uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a9448-169">Once the commit method returns, the data is updated in the dictionary and replicated to other nodes in the cluster.</span></span> <span data-ttu-id="a9448-170">Data jsou teď bezpečně uložená v clusteru, a může převzít back-end službu do dalších uzlů, i nadále s dostupná data.</span><span class="sxs-lookup"><span data-stu-id="a9448-170">The data is now safely stored in the cluster, and the back-end service can fail over to other nodes, still having the data available.</span></span>
5. <span data-ttu-id="a9448-171">Stiskněte klávesu **F5** pokračovat</span><span class="sxs-lookup"><span data-stu-id="a9448-171">Press **F5** to continue</span></span>

<span data-ttu-id="a9448-172">Chcete-li ukončit relaci ladění, stiskněte **Shift + F5**.</span><span class="sxs-lookup"><span data-stu-id="a9448-172">To stop the debugging session, press **Shift+F5**.</span></span>

## <a name="deploy-the-application-to-azure"></a><span data-ttu-id="a9448-173">Nasazení aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="a9448-173">Deploy the application to Azure</span></span>
<span data-ttu-id="a9448-174">Pokud chcete nasadit aplikace do clusteru s podporou v Azure, buď můžete vytvořit vlastní cluster nebo používat Cluster s podporou strany.</span><span class="sxs-lookup"><span data-stu-id="a9448-174">To deploy the application to a cluster in Azure, you can either choose to create your own cluster, or use a Party Cluster.</span></span>

<span data-ttu-id="a9448-175">Party clustery jsou bezplatné, časově omezené clustery Service Fabric hostované v Azure a provozované týmem Service Fabric, na kterých může kdokoli nasazovat aplikace a seznamovat se s platformou.</span><span class="sxs-lookup"><span data-stu-id="a9448-175">Party clusters are free, limited-time Service Fabric clusters hosted on Azure and run by the Service Fabric team where anyone can deploy applications and learn about the platform.</span></span> <span data-ttu-id="a9448-176">Pokud chcete získat přístup k Party Clusteru, [postupujte podle těchto pokynů](http://aka.ms/tryservicefabric).</span><span class="sxs-lookup"><span data-stu-id="a9448-176">To get access to a Party Cluster, [follow the instructions](http://aka.ms/tryservicefabric).</span></span> 

<span data-ttu-id="a9448-177">Informace o vytvoření vlastního clusteru najdete v tématu [Vytvoření vašeho prvního clusteru Service Fabric v Azure](service-fabric-get-started-azure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="a9448-177">For information about creating your own cluster, see [Create your first Service Fabric cluster on Azure](service-fabric-get-started-azure-cluster.md).</span></span>

> [!Note]
> <span data-ttu-id="a9448-178">Front-endu webové služby je nakonfigurován pro naslouchání na portu 8080 pro příchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="a9448-178">The web front-end service is configured to listen on port 8080 for incoming traffic.</span></span> <span data-ttu-id="a9448-179">Ujistěte se, že je ve vašem clusteru tento port otevřený.</span><span class="sxs-lookup"><span data-stu-id="a9448-179">Make sure that port is open in your cluster.</span></span> <span data-ttu-id="a9448-180">Pokud používáte Cluster strany, tento port je otevřený.</span><span class="sxs-lookup"><span data-stu-id="a9448-180">If you are using the Party Cluster, this port is open.</span></span>
>

### <a name="deploy-the-application-using-visual-studio"></a><span data-ttu-id="a9448-181">Nasazení aplikace pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9448-181">Deploy the application using Visual Studio</span></span>
<span data-ttu-id="a9448-182">Aplikace je teď připravená a přímo ze sady Visual Studio ji můžete nasadit do clusteru.</span><span class="sxs-lookup"><span data-stu-id="a9448-182">Now that the application is ready, you can deploy it to a cluster directly from Visual Studio.</span></span>

1. <span data-ttu-id="a9448-183">Klikněte pravým tlačítkem na **Voting** v Průzkumníku řešení a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="a9448-183">Right-click **Voting** in the Solution Explorer and choose **Publish**.</span></span> <span data-ttu-id="a9448-184">Zobrazí se dialogové okno Publikovat.</span><span class="sxs-lookup"><span data-stu-id="a9448-184">The Publish dialog appears.</span></span>

    ![Dialogové okno Publikovat](./media/service-fabric-quickstart-dotnet/publish-app.png)

2. <span data-ttu-id="a9448-186">Do pole **Koncový bod připojení** zadejte koncový bod připojení clusteru a klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="a9448-186">Type in the Connection Endpoint of the cluster in the **Connection Endpoint** field and click **Publish**.</span></span> <span data-ttu-id="a9448-187">Při registraci ke clusteru strany, koncový bod připojení je k dispozici v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a9448-187">When signing up for the Party Cluster, the Connection Endpoint is provided in the browser.</span></span> <span data-ttu-id="a9448-188">-například `winh1x87d1d.westus.cloudapp.azure.com:19000`.</span><span class="sxs-lookup"><span data-stu-id="a9448-188">- for example, `winh1x87d1d.westus.cloudapp.azure.com:19000`.</span></span>

3. <span data-ttu-id="a9448-189">Otevřete prohlížeč a zadejte adresu clusteru – například `http://winh1x87d1d.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="a9448-189">Open a browser and type in the cluster address - for example, `http://winh1x87d1d.westus.cloudapp.azure.com`.</span></span> <span data-ttu-id="a9448-190">Teď byste měli vidět aplikace běžící v clusteru v Azure.</span><span class="sxs-lookup"><span data-stu-id="a9448-190">You should now see the application running in the cluster in Azure.</span></span>

![Aplikace front-endu](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)

## <a name="scale-applications-and-services-in-a-cluster"></a><span data-ttu-id="a9448-192">Škálování aplikací a služeb v clusteru</span><span class="sxs-lookup"><span data-stu-id="a9448-192">Scale applications and services in a cluster</span></span>
<span data-ttu-id="a9448-193">Service Fabric služby je možné snadno rozšířit mezi clustery pro přizpůsobení pro změnu zatížení v rámci služeb.</span><span class="sxs-lookup"><span data-stu-id="a9448-193">Service Fabric services can easily be scaled across a cluster to accommodate for a change in the load on the services.</span></span> <span data-ttu-id="a9448-194">Služby se škálují změnou počtu instancí spuštěných v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a9448-194">You scale a service by changing the number of instances running in the cluster.</span></span> <span data-ttu-id="a9448-195">Máte více způsobů škálování vašim službám, můžete použít skripty nebo příkazy z prostředí PowerShell nebo Service Fabric rozhraní příkazového řádku (sfctl).</span><span class="sxs-lookup"><span data-stu-id="a9448-195">You have multiple ways of scaling your services, you can use scripts or commands from PowerShell or Service Fabric CLI (sfctl).</span></span> <span data-ttu-id="a9448-196">V tomto příkladu používáme Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="a9448-196">In this example, we are using Service Fabric Explorer.</span></span>

<span data-ttu-id="a9448-197">Service Fabric Explorer běží v na všech clusterech Service Fabric a je přístupný z prohlížeče, procházením port pro správu clusterů HTTP (19080), například `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span><span class="sxs-lookup"><span data-stu-id="a9448-197">Service Fabric Explorer runs in all Service Fabric clusters and can be accessed from a browser, by browsing to the clusters HTTP management port (19080), for example, `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span></span>

<span data-ttu-id="a9448-198">Pokud chcete škálovat webovou front-end službu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a9448-198">To scale the web front-end service, do the following steps:</span></span>

1. <span data-ttu-id="a9448-199">Otevřete ve vašem clusteru Service Fabric Explorer – například `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span><span class="sxs-lookup"><span data-stu-id="a9448-199">Open Service Fabric Explorer in your cluster - for example,`http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span></span>
2. <span data-ttu-id="a9448-200">Klikněte na znak výpustky (tři tečky) vedle položky **fabric: / Voting/VotingWeb** uzlu ve stromovém zobrazení a zvolte **škálování služby**.</span><span class="sxs-lookup"><span data-stu-id="a9448-200">Click on the ellipsis (three dots) next to the **fabric:/Voting/VotingWeb** node in the treeview and choose **Scale Service**.</span></span>

    ![Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scale.png)

    <span data-ttu-id="a9448-202">Nyní můžete škálovat počet instancí webové front-end služby.</span><span class="sxs-lookup"><span data-stu-id="a9448-202">You can now choose to scale the number of instances of the web front-end service.</span></span>

3. <span data-ttu-id="a9448-203">Změňte počet na **2** a klikněte na **Škálovat službu**.</span><span class="sxs-lookup"><span data-stu-id="a9448-203">Change the number to **2** and click **Scale Service**.</span></span>
4. <span data-ttu-id="a9448-204">Klikněte na **fabric: / Voting/VotingWeb** uzlu ve stromovém zobrazení a rozbalte uzel oddílu (představované identifikátor GUID).</span><span class="sxs-lookup"><span data-stu-id="a9448-204">Click on the **fabric:/Voting/VotingWeb** node in the tree-view and expand the partition node (represented by a GUID).</span></span>

    ![Service Fabric Explorer škálování Service](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scaled-service.png)

    <span data-ttu-id="a9448-206">Nyní můžete vidět, že služba má dvě instance, a ve stromovém zobrazení uzlů, které instance spustili.</span><span class="sxs-lookup"><span data-stu-id="a9448-206">You can now see that the service has two instances, and in the tree view you see which nodes the instances run on.</span></span>

<span data-ttu-id="a9448-207">Touto jednoduchou úlohou správy jsme zdvojnásobili prostředky, které má naše front-end služba k dispozici pro zpracování uživatelské zátěže.</span><span class="sxs-lookup"><span data-stu-id="a9448-207">By this simple management task, we doubled the resources available for our front-end service to process user load.</span></span> <span data-ttu-id="a9448-208">Je důležité si uvědomit, že pro spolehlivý provoz služby nepotřebujete více jejích instancí.</span><span class="sxs-lookup"><span data-stu-id="a9448-208">It's important to understand that you do not need multiple instances of a service to have it run reliably.</span></span> <span data-ttu-id="a9448-209">Pokud služba selže, Service Fabric zajistí v clusteru spuštění nové instance služby.</span><span class="sxs-lookup"><span data-stu-id="a9448-209">If a service fails, Service Fabric makes sure a new service instance runs in the cluster.</span></span>

## <a name="perform-a-rolling-application-upgrade"></a><span data-ttu-id="a9448-210">Provedení postupného upgradu aplikace</span><span class="sxs-lookup"><span data-stu-id="a9448-210">Perform a rolling application upgrade</span></span>
<span data-ttu-id="a9448-211">Při nasazování nové aktualizace do vaší aplikace, Service Fabric zavede aktualizace bezpečným způsobem.</span><span class="sxs-lookup"><span data-stu-id="a9448-211">When deploying new updates to your application, Service Fabric rolls out the update in a safe way.</span></span> <span data-ttu-id="a9448-212">Vrácení upgradu vám dává žádné výpadky při upgradu a také automatické vrácení zpět, musí dojít k chybám.</span><span class="sxs-lookup"><span data-stu-id="a9448-212">Rolling upgrades gives you no downtime while upgrading as well as automated rollback should errors occur.</span></span>

<span data-ttu-id="a9448-213">Pokud chcete upgradovat aplikaci, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a9448-213">To upgrade the application, do the following:</span></span>

1. <span data-ttu-id="a9448-214">Otevřete **Index.cshtml** souborů v sadě Visual Studio – můžete vyhledat soubor v Průzkumníku řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9448-214">Open the **Index.cshtml** file in Visual Studio - You can search for the file in the Solution Explorer in Visual Studio.</span></span>
2. <span data-ttu-id="a9448-215">Nadpis na stránce změníte tak, že přidáte některé text – například.</span><span class="sxs-lookup"><span data-stu-id="a9448-215">Change the heading on the page by adding some text - for example.</span></span>
    ```html
        <div class="col-xs-8 col-xs-offset-2 text-center">
            <h2>Service Fabric Voting Sample v2</h2>
        </div>
    ```
3. <span data-ttu-id="a9448-216">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="a9448-216">Save the file.</span></span>
4. <span data-ttu-id="a9448-217">Klikněte pravým tlačítkem na **Voting** v Průzkumníku řešení a zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="a9448-217">Right-click **Voting** in the Solution Explorer and choose **Publish**.</span></span> <span data-ttu-id="a9448-218">Zobrazí se dialogové okno Publikovat.</span><span class="sxs-lookup"><span data-stu-id="a9448-218">The Publish dialog appears.</span></span>
5. <span data-ttu-id="a9448-219">Klikněte **Manifest verze** tlačítko Změna verze služby a aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9448-219">Click the **Manifest Version** button to change the version of the service and application.</span></span>
6. <span data-ttu-id="a9448-220">Změna verze **kód** prvek v rámci **VotingWebPkg** na "2.0.0", například a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a9448-220">Change the version of the **Code** element under **VotingWebPkg** to "2.0.0", for example, and click **Save**.</span></span>

    ![Dialogové okno Změnit verzi](./media/service-fabric-quickstart-dotnet/change-version.png)
7. <span data-ttu-id="a9448-222">V **publikovat aplikace Service Fabric** dialogové okno, zkontrolujte upgradu aplikace políčko a klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="a9448-222">In the **Publish Service Fabric Application** dialog, check the Upgrade the Application checkbox, and click **Publish**.</span></span>

    ![Dialogové okno publikování upgradovat nastavení](./media/service-fabric-quickstart-dotnet/upgrade-app.png)
8. <span data-ttu-id="a9448-224">Otevřete prohlížeč a přejděte na adresu clusteru na portu 19080 – například `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span><span class="sxs-lookup"><span data-stu-id="a9448-224">Open your browser and browse to the cluster address on port 19080 - for example, `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.</span></span>
9. <span data-ttu-id="a9448-225">Klikněte na **aplikace** uzlu ve stromovém zobrazení a potom **upgrady v průběhu** v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="a9448-225">Click on the **Applications** node in the tree view, and then **Upgrades in Progress** in the right-hand pane.</span></span> <span data-ttu-id="a9448-226">Zobrazí způsob upgradu zahrnuje prostřednictvím upgradovacích domén v clusteru, ujistěte se, že každá doména, je v pořádku, než budete pokračovat na další.</span><span class="sxs-lookup"><span data-stu-id="a9448-226">You see how the upgrade rolls through the upgrade domains in your cluster, making sure each domain is healthy before proceeding to the next.</span></span>
    <span data-ttu-id="a9448-227">![Upgrade zobrazení v Service Fabric Exploreru](./media/service-fabric-quickstart-dotnet/upgrading.png)</span><span class="sxs-lookup"><span data-stu-id="a9448-227">![Upgrade View in Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/upgrading.png)</span></span>

    <span data-ttu-id="a9448-228">Service Fabric lze upgrady bezpečné tím, že dvě minuty po upgradu služby v každém uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a9448-228">Service Fabric makes upgrades safe by waiting two minutes after upgrading the service on each node in the cluster.</span></span> <span data-ttu-id="a9448-229">Očekávejte celou aktualizace trvat přibližně osm minut.</span><span class="sxs-lookup"><span data-stu-id="a9448-229">Expect the entire update to take approximately eight minutes.</span></span>

10. <span data-ttu-id="a9448-230">Při upgradu, můžete dál používat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a9448-230">While the upgrade is running, you can still use the application.</span></span> <span data-ttu-id="a9448-231">Vzhledem k tomu, že máte dvě instance služby spuštěné v clusteru, některé z vašich žádosti o může získat upgradované verzi aplikace, zatímco ostatní může stále dojít k předchozí verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9448-231">Because you have two instances of the service running in the cluster, some of your requests may get an upgraded version of the application, while others may still get the old version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9448-232">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a9448-232">Next steps</span></span>
<span data-ttu-id="a9448-233">V tomto rychlém startu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="a9448-233">In this quickstart, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a9448-234">Vytvoření aplikace pomocí rozhraní .NET a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a9448-234">Create an application using .NET and Service Fabric</span></span>
> * <span data-ttu-id="a9448-235">Pomocí ASP.NET core jako webového front-endu</span><span class="sxs-lookup"><span data-stu-id="a9448-235">Use ASP.NET core as a web front-end</span></span>
> * <span data-ttu-id="a9448-236">Ukládání dat aplikací ve stavové služby</span><span class="sxs-lookup"><span data-stu-id="a9448-236">Store application data in a stateful service</span></span>
> * <span data-ttu-id="a9448-237">Ladění aplikace místně</span><span class="sxs-lookup"><span data-stu-id="a9448-237">Debug your application locally</span></span>
> * <span data-ttu-id="a9448-238">Nasaďte aplikaci do clusteru s podporou v Azure</span><span class="sxs-lookup"><span data-stu-id="a9448-238">Deploy the application to a cluster in Azure</span></span>
> * <span data-ttu-id="a9448-239">Škálování aplikací mezi několika uzly</span><span class="sxs-lookup"><span data-stu-id="a9448-239">Scale-out the application across multiple nodes</span></span>
> * <span data-ttu-id="a9448-240">Provedení postupného upgradu aplikace</span><span class="sxs-lookup"><span data-stu-id="a9448-240">Perform a rolling application upgrade</span></span>

<span data-ttu-id="a9448-241">Další informace o Service Fabric a .NET, podívejte se na tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="a9448-241">To learn more about Service Fabric and .NET, take a look at this tutorial:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a9448-242">Aplikace .NET v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a9448-242">.NET application on Service Fabric</span></span>](service-fabric-tutorial-create-dotnet-app.md)