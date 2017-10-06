---
title: "aaaDeploy aplikace Node.js, která používá MongoDB | Microsoft Docs"
description: "Návod, jak toopackage více hosta spustitelné soubory toodeploy tooan Azure Service Fabric clusteru"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: b76bb756-c1ba-49f9-9666-e9807cf8f92f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell;mikhegn
ms.openlocfilehash: 2775080f0d9d42d6ba15cca911e23067106be26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-multiple-guest-executables"></a><span data-ttu-id="1b73f-103">Nasazení několika hostujících spustitelných souborů</span><span class="sxs-lookup"><span data-stu-id="1b73f-103">Deploy multiple guest executables</span></span>
<span data-ttu-id="1b73f-104">Tento článek ukazuje, jak toopackage a nasadit více hosta spustitelné soubory tooAzure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1b73f-104">This article shows how toopackage and deploy multiple guest executables tooAzure Service Fabric.</span></span> <span data-ttu-id="1b73f-105">Pro vytváření a nasazování jeden balíček Service Fabric číst jak příliš[nasazení spustitelné tooService hosta Fabric](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="1b73f-105">For building and deploying a single Service Fabric package read how too[deploy a guest executable tooService Fabric](service-fabric-deploy-existing-app.md).</span></span>

<span data-ttu-id="1b73f-106">Když tento návod ukazuje, jak toodeploy aplikace s Node.js front-end, který jako úložiště dat hello používá MongoDB, můžete použít hello kroky tooany aplikace, který má závislosti na jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1b73f-106">While this walkthrough shows how toodeploy an application with a Node.js front end that uses MongoDB as hello data store, you can apply hello steps tooany application that has dependencies on another application.</span></span>   

<span data-ttu-id="1b73f-107">Můžete použít Visual Studio tooproduce hello aplikace balíček, který obsahuje více spustitelné soubory hosta.</span><span class="sxs-lookup"><span data-stu-id="1b73f-107">You can use Visual Studio tooproduce hello application package that contains multiple guest executables.</span></span> <span data-ttu-id="1b73f-108">V tématu [pomocí sady Visual Studio toopackage existující aplikaci](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="1b73f-108">See [Using Visual Studio toopackage an existing application](service-fabric-deploy-existing-app.md).</span></span> <span data-ttu-id="1b73f-109">Po přidání hello první hosta spustitelný soubor, klikněte pravým tlačítkem na projekt aplikace hello a vyberte hello **Přidat -> Nový Service Fabric service** tooadd hello druhý hosta spustitelný projekt toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="1b73f-109">After you have added hello first guest executable, right click on hello application project and select hello **Add->New Service Fabric service** tooadd hello second guest executable project toohello solution.</span></span> <span data-ttu-id="1b73f-110">Poznámka: Pokud se rozhodnete, že zdroj hello toolink v hello projektu sady Visual Studio, vytváření hello řešení sady Visual Studio, se ujistěte se, že balíčku aplikace zapnutý toodate se změnami ve zdroji hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-110">Note: If you choose toolink hello source in hello Visual Studio project, building hello Visual Studio solution, will make sure that your application package is up toodate with changes in hello source.</span></span> 

## <a name="samples"></a><span data-ttu-id="1b73f-111">Ukázky</span><span class="sxs-lookup"><span data-stu-id="1b73f-111">Samples</span></span>
* [<span data-ttu-id="1b73f-112">Ukázka pro balení a nasazení spustitelný soubor hosta</span><span class="sxs-lookup"><span data-stu-id="1b73f-112">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="1b73f-113">Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes hello pojmenování službu pomocí REST</span><span class="sxs-lookup"><span data-stu-id="1b73f-113">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="manually-package-hello-multiple-guest-executable-application"></a><span data-ttu-id="1b73f-114">Ručně balíček hello více hosta spustitelná aplikace</span><span class="sxs-lookup"><span data-stu-id="1b73f-114">Manually package hello multiple guest executable application</span></span>
<span data-ttu-id="1b73f-115">Případně můžete ručně balíček hello hosta spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="1b73f-115">Alternatively you can manually package hello guest executable.</span></span> <span data-ttu-id="1b73f-116">Pro ruční balení hello, tento článek používá hello Service Fabric balení nástroj, který je k dispozici na [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span><span class="sxs-lookup"><span data-stu-id="1b73f-116">For hello manual packaging, this article uses hello Service Fabric packaging tool, which is available at [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span></span>

### <a name="packaging-hello-nodejs-application"></a><span data-ttu-id="1b73f-117">Balení hello aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="1b73f-117">Packaging hello Node.js application</span></span>
<span data-ttu-id="1b73f-118">Tento článek předpokládá, že není nainstalována Node.js hello uzlů v clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-118">This article assumes that Node.js is not installed on hello nodes in hello Service Fabric cluster.</span></span> <span data-ttu-id="1b73f-119">V důsledku toho je nutné tooadd Node.exe toohello kořenový adresář aplikace uzlu před balení.</span><span class="sxs-lookup"><span data-stu-id="1b73f-119">As a consequence, you need tooadd Node.exe toohello root directory of your node application before packaging.</span></span> <span data-ttu-id="1b73f-120">struktura adresářů Hello aplikace Node.js hello (s použitím expresního webová architektura a modulu Jade šablon) by měl vypadat podobně jako toohello jeden níže:</span><span class="sxs-lookup"><span data-stu-id="1b73f-120">hello directory structure of hello Node.js application (using Express web framework and Jade template engine) should look similar toohello one below:</span></span>

```
|-- NodeApplication
    |-- bin
        |-- www
    |-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
    |-- public
        |-- images
        |-- etc.
    |-- routes
        |-- index.js
        |-- users.js
    |-- views
        |-- index.jade
        |-- etc.
    |-- app.js
    |-- package.json
    |-- node.exe
```

<span data-ttu-id="1b73f-121">Jako další krok vytvoříte balíček aplikace pro aplikace Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-121">As a next step, you create an application package for hello Node.js application.</span></span> <span data-ttu-id="1b73f-122">Následující kód Hello vytvoří balíček aplikace Service Fabric, která obsahuje aplikaci Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-122">hello code below creates a Service Fabric application package that contains hello Node.js application.</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

<span data-ttu-id="1b73f-123">Níže je uveden popis hello parametry, které jsou používány:</span><span class="sxs-lookup"><span data-stu-id="1b73f-123">Below is a description of hello parameters that are being used:</span></span>

* <span data-ttu-id="1b73f-124">**/ source** body toohello adresáře hello aplikace, který by měl být zabalen.</span><span class="sxs-lookup"><span data-stu-id="1b73f-124">**/source** points toohello directory of hello application that should be packaged.</span></span>
* <span data-ttu-id="1b73f-125">**/ target** definuje hello adresář, ve které hello by měl být balíček vytvořen.</span><span class="sxs-lookup"><span data-stu-id="1b73f-125">**/target** defines hello directory in which hello package should be created.</span></span> <span data-ttu-id="1b73f-126">Tento adresář má toobe liší od hello zdrojový adresář.</span><span class="sxs-lookup"><span data-stu-id="1b73f-126">This directory has toobe different from hello source directory.</span></span>
* <span data-ttu-id="1b73f-127">**příkazy/appname** definuje název aplikace hello hello existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b73f-127">**/appname** defines hello application name of hello existing application.</span></span> <span data-ttu-id="1b73f-128">Je důležité toounderstand, to znamená, že název služby toohello v manifestu hello a není toohello název aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1b73f-128">It's important toounderstand that this translates toohello service name in hello manifest, and not toohello Service Fabric application name.</span></span>
* <span data-ttu-id="1b73f-129">**/exe** definuje hello spustitelný soubor, že Service Fabric je v tomto případě by měl toolaunch, `node.exe`.</span><span class="sxs-lookup"><span data-stu-id="1b73f-129">**/exe** defines hello executable that Service Fabric is supposed toolaunch, in this case `node.exe`.</span></span>
* <span data-ttu-id="1b73f-130">**/Ma** definuje hello argument, který se použité toolaunch hello spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="1b73f-130">**/ma** defines hello argument that is being used toolaunch hello executable.</span></span> <span data-ttu-id="1b73f-131">Jako Node.js není nainstalovaná, Service Fabric musí toolaunch hello Node.js webový server tak, že spustíte `node.exe bin/www`.</span><span class="sxs-lookup"><span data-stu-id="1b73f-131">As Node.js is not installed, Service Fabric needs toolaunch hello Node.js web server by executing `node.exe bin/www`.</span></span>  <span data-ttu-id="1b73f-132">`/ma:'bin/www'`informuje hello balení nástroj toouse `bin/ma` jako hello argument node.exe.</span><span class="sxs-lookup"><span data-stu-id="1b73f-132">`/ma:'bin/www'` tells hello packaging tool toouse `bin/ma` as hello argument for node.exe.</span></span>
* <span data-ttu-id="1b73f-133">**Nebo typ aplikace** definuje název typu aplikace Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-133">**/AppType** defines hello Service Fabric application type name.</span></span>

<span data-ttu-id="1b73f-134">Pokud toohello adresář, který byl zadaný v parametru/Target hello, můžete zjistit, že tento nástroj hello vytvořil plně funkční balíček Service Fabric, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="1b73f-134">If you browse toohello directory that was specified in hello /target parameter, you can see that hello tool has created a fully functioning Service Fabric package as shown below:</span></span>

```
|--[yourtargetdirectory]
    |-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
<span data-ttu-id="1b73f-135">Hello generovaného ServiceManifest.xml teď má oddíl, který popisuje, jak hello Node.js webového serveru musí být spuštěna, jak je znázorněno v následující fragment kódu hello:</span><span class="sxs-lookup"><span data-stu-id="1b73f-135">hello generated ServiceManifest.xml now has a section that describes how hello Node.js web server should be launched, as shown in hello code snippet below:</span></span>

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
<span data-ttu-id="1b73f-136">V této ukázce hello Node.js webový server naslouchá tooport 3000, takže budete muset tooupdate hello koncový bod, informace v souboru ServiceManifest.xml hello, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="1b73f-136">In this sample, hello Node.js web server listens tooport 3000, so you need tooupdate hello endpoint information in hello ServiceManifest.xml file as shown below.</span></span>   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-hello-mongodb-application"></a><span data-ttu-id="1b73f-137">Balení hello MongoDB aplikace</span><span class="sxs-lookup"><span data-stu-id="1b73f-137">Packaging hello MongoDB application</span></span>
<span data-ttu-id="1b73f-138">Teď, když máte zabalené aplikace Node.js hello, můžete přejít k tématu a balíček MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1b73f-138">Now that you have packaged hello Node.js application, you can go ahead and package MongoDB.</span></span> <span data-ttu-id="1b73f-139">Jak je uvedeno nahoře, nejsou hello kroky, které teď projít konkrétní tooNode.js a MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1b73f-139">As mentioned before, hello steps that you go through now are not specific tooNode.js and MongoDB.</span></span> <span data-ttu-id="1b73f-140">Vztahují se ve skutečnosti tooall aplikace, které jsou určené toobe zabalené společně jako jednu aplikaci Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1b73f-140">In fact, they apply tooall applications that are meant toobe packaged together as one Service Fabric application.</span></span>  

<span data-ttu-id="1b73f-141">toopackage MongoDB, budete chtít toomake se, že balíček Mongod.exe a Mongo.exe.</span><span class="sxs-lookup"><span data-stu-id="1b73f-141">toopackage MongoDB, you want toomake sure you package Mongod.exe and Mongo.exe.</span></span> <span data-ttu-id="1b73f-142">Obě binární soubory jsou umístěny v hello `bin` adresář adresáře instalace MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1b73f-142">Both binaries are located in hello `bin` directory of your MongoDB installation directory.</span></span> <span data-ttu-id="1b73f-143">struktura adresářů Hello vypadá podobně jako toohello jeden níže.</span><span class="sxs-lookup"><span data-stu-id="1b73f-143">hello directory structure looks similar toohello one below.</span></span>

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
<span data-ttu-id="1b73f-144">Service Fabric potřebuje toostart MongoDB s příkaz podobné toohello, jeden níže, takže musíte toouse hello `/ma` parametr při balení MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1b73f-144">Service Fabric needs toostart MongoDB with a command similar toohello one below, so you need toouse hello `/ma` parameter when packaging MongoDB.</span></span>

```
mongod.exe --dbpath [path toodata]
```
> [!NOTE]
> <span data-ttu-id="1b73f-145">Hello data není právě zachována hello v případě selhání uzlu, když vložíte hello MongoDB data adresáře v místním adresáři hello hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="1b73f-145">hello data is not being preserved in hello case of a node failure if you put hello MongoDB data directory on hello local directory of hello node.</span></span> <span data-ttu-id="1b73f-146">Buď použijte odolná úložiště nebo implementaci sady dojít ke ztrátě dat. pořadí tooprevent replik MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1b73f-146">You should either use durable storage or implement a MongoDB replica set in order tooprevent data loss.</span></span>  
>
>

<span data-ttu-id="1b73f-147">V příkazovém prostředí PowerShell nebo hello jsme hello balení nástroj spustit s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="1b73f-147">In PowerShell or hello command shell, we run hello packaging tool with hello following parameters:</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path toodata]' /AppType:NodeAppType
```

<span data-ttu-id="1b73f-148">V pořadí tooadd MongoDB tooyour Service Fabric balíčku aplikace, musíte se, že parametr/target hello odkazují toohello toomake stejný adresář, který již obsahuje manifest aplikace hello spolu s aplikací Node.js hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-148">In order tooadd MongoDB tooyour Service Fabric application package, you need toomake sure that hello /target parameter points toohello same directory that already contains hello application manifest along with hello Node.js application.</span></span> <span data-ttu-id="1b73f-149">Musíte taky toomake jistotu, že používáte stejný název ApplicationType hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-149">You also need toomake sure that you are using hello same ApplicationType name.</span></span>

<span data-ttu-id="1b73f-150">Umožňuje procházet adresář toohello a prozkoumat co hello nástroj vytvořil.</span><span class="sxs-lookup"><span data-stu-id="1b73f-150">Let's browse toohello directory and examine what hello tool has created.</span></span>

```
|--[yourtargetdirectory]
    |-- MyNodeApplication
    |-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
<span data-ttu-id="1b73f-151">Jak vidíte, nástroj hello přidat nový adresář toohello složky, MongoDB, který obsahuje binární soubory MongoDB hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-151">As you can see, hello tool added a new folder, MongoDB, toohello directory that contains hello MongoDB binaries.</span></span> <span data-ttu-id="1b73f-152">Pokud otevřete hello `ApplicationManifest.xml` souboru, zobrazí se tento balíček hello nyní obsahuje aplikace Node.js hello a MongoDB.</span><span class="sxs-lookup"><span data-stu-id="1b73f-152">If you open hello `ApplicationManifest.xml` file, you can see that hello package now contains both hello Node.js application and MongoDB.</span></span> <span data-ttu-id="1b73f-153">Následující kód Hello ukazuje hello obsah hello manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b73f-153">hello code below shows hello content of hello application manifest.</span></span>

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-hello-application"></a><span data-ttu-id="1b73f-154">Publikování aplikace hello</span><span class="sxs-lookup"><span data-stu-id="1b73f-154">Publishing hello application</span></span>
<span data-ttu-id="1b73f-155">posledním krokem Hello je toopublish hello aplikace toohello místní cluster Service Fabric pomocí skriptů prostředí PowerShell hello níže:</span><span class="sxs-lookup"><span data-stu-id="1b73f-155">hello last step is toopublish hello application toohello local Service Fabric cluster by using hello PowerShell scripts below:</span></span>

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

<span data-ttu-id="1b73f-156">Jakmile aplikace hello úspěšně publikovaných toohello místní cluster, můžete přistupovat aplikace Node.js hello na hello portu, které jsme zadali v hello service manifest aplikace Node.js hello – například http://localhost: 3000.</span><span class="sxs-lookup"><span data-stu-id="1b73f-156">Once hello application is successfully published toohello local cluster, you can access hello Node.js application on hello port that we have entered in hello service manifest of hello Node.js application--for example http://localhost:3000.</span></span>

<span data-ttu-id="1b73f-157">V tomto kurzu jste viděli, jak tooeasily balíček dvě existující aplikace jako jednu aplikaci Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1b73f-157">In this tutorial, you have seen how tooeasily package two existing applications as one Service Fabric application.</span></span> <span data-ttu-id="1b73f-158">Naučili jste se jak toodeploy ho tooService prostředků infrastruktury, které se mohou těžit z výhod některé hello Service Fabric funkce, jako je vysoká dostupnost a stavu systému integrace.</span><span class="sxs-lookup"><span data-stu-id="1b73f-158">You have also learned how toodeploy it tooService Fabric so that it can benefit from some of hello Service Fabric features, such as high availability and health system integration.</span></span>


## <a name="adding-more-guest-executables-tooan-existing-application-using-yeoman-on-linux"></a><span data-ttu-id="1b73f-159">Přidání další hosta spustitelné soubory tooan existující aplikace pomocí Yeoman v systému Linux</span><span class="sxs-lookup"><span data-stu-id="1b73f-159">Adding more guest executables tooan existing application using Yeoman on Linux</span></span>

<span data-ttu-id="1b73f-160">tooadd jiná tooan aplikace služby již vytvořené pomocí `yo`, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1b73f-160">tooadd another service tooan application already created using `yo`, perform hello following steps:</span></span> 
1. <span data-ttu-id="1b73f-161">Změnit kořenový adresář toohello hello existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b73f-161">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="1b73f-162">Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je vytvořený Yeoman aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-162">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="1b73f-163">Spustit `yo azuresfguest:AddService` a zadejte potřebné detaily hello.</span><span class="sxs-lookup"><span data-stu-id="1b73f-163">Run `yo azuresfguest:AddService` and provide hello necessary details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b73f-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b73f-164">Next steps</span></span>
* <span data-ttu-id="1b73f-165">Další informace o nasazení kontejnery s [Service Fabric a kontejnery – přehled](service-fabric-containers-overview.md)</span><span class="sxs-lookup"><span data-stu-id="1b73f-165">Learn about deploying containers with [Service Fabric and containers overview](service-fabric-containers-overview.md)</span></span>
* [<span data-ttu-id="1b73f-166">Ukázka pro balení a nasazení spustitelný soubor hosta</span><span class="sxs-lookup"><span data-stu-id="1b73f-166">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="1b73f-167">Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes hello pojmenování službu pomocí REST</span><span class="sxs-lookup"><span data-stu-id="1b73f-167">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
