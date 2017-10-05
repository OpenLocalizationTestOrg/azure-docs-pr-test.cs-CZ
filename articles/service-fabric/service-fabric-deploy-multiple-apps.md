---
title: "Nasazení aplikace Node.js, která používá MongoDB | Microsoft Docs"
description: "Návod o tom, jak balíček více spustitelné soubory hosta k nasazení clusteru služby Azure Service Fabric"
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
ms.openlocfilehash: b71723034e5f663986c49481072bfd6779d3d57b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-multiple-guest-executables"></a><span data-ttu-id="b6bc1-103">Nasazení několika hostujících spustitelných souborů</span><span class="sxs-lookup"><span data-stu-id="b6bc1-103">Deploy multiple guest executables</span></span>
<span data-ttu-id="b6bc1-104">Tento článek ukazuje, jak pro balíček a nasazení více spustitelné soubory hosta pro Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-104">This article shows how to package and deploy multiple guest executables to Azure Service Fabric.</span></span> <span data-ttu-id="b6bc1-105">Vytváření a nasazování jeden balíček Service Fabric najdete v návodu k [nasadit do Service Fabric Host spustitelný soubor](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="b6bc1-105">For building and deploying a single Service Fabric package read how to [deploy a guest executable to Service Fabric](service-fabric-deploy-existing-app.md).</span></span>

<span data-ttu-id="b6bc1-106">Když tento návod ukazuje, jak nasadit aplikaci s Node.js front-end, který jako úložiště dat používá MongoDB, můžete použít kroky pro každou aplikaci, která má závislosti na jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-106">While this walkthrough shows how to deploy an application with a Node.js front end that uses MongoDB as the data store, you can apply the steps to any application that has dependencies on another application.</span></span>   

<span data-ttu-id="b6bc1-107">Visual Studio můžete použít k vytvoření balíček aplikace, který obsahuje více spustitelné soubory hosta.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-107">You can use Visual Studio to produce the application package that contains multiple guest executables.</span></span> <span data-ttu-id="b6bc1-108">V tématu [pomocí sady Visual Studio balíčku existující aplikaci](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="b6bc1-108">See [Using Visual Studio to package an existing application](service-fabric-deploy-existing-app.md).</span></span> <span data-ttu-id="b6bc1-109">Po přidání první hosta spustitelný soubor, klikněte pravým tlačítkem na projekt aplikace a vyberte **Přidat -> Nový Service Fabric service** přidat druhý hosta spustitelný projekt do řešení.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-109">After you have added the first guest executable, right click on the application project and select the **Add->New Service Fabric service** to add the second guest executable project to the solution.</span></span> <span data-ttu-id="b6bc1-110">Poznámka: Pokud zvolíte možnost propojit zdroje v projektu sady Visual Studio, vytváření řešení sady Visual Studio, budou se, že je aktuální pomocí změn ve zdroji balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-110">Note: If you choose to link the source in the Visual Studio project, building the Visual Studio solution, will make sure that your application package is up to date with changes in the source.</span></span> 

## <a name="samples"></a><span data-ttu-id="b6bc1-111">Ukázky</span><span class="sxs-lookup"><span data-stu-id="b6bc1-111">Samples</span></span>
* [<span data-ttu-id="b6bc1-112">Ukázka pro balení a nasazení spustitelný soubor hosta</span><span class="sxs-lookup"><span data-stu-id="b6bc1-112">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="b6bc1-113">Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes službu Naming pomocí REST</span><span class="sxs-lookup"><span data-stu-id="b6bc1-113">Sample of two guest executables (C# and nodejs) communicating via the Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="manually-package-the-multiple-guest-executable-application"></a><span data-ttu-id="b6bc1-114">Ručně balíček více hosta spustitelná aplikace</span><span class="sxs-lookup"><span data-stu-id="b6bc1-114">Manually package the multiple guest executable application</span></span>
<span data-ttu-id="b6bc1-115">Případně můžete ručně balíček Host spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-115">Alternatively you can manually package the guest executable.</span></span> <span data-ttu-id="b6bc1-116">Pro ruční vytváření balíčků, tento článek používá nástroj balení Service Fabric, která je k dispozici v [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span><span class="sxs-lookup"><span data-stu-id="b6bc1-116">For the manual packaging, this article uses the Service Fabric packaging tool, which is available at [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).</span></span>

### <a name="packaging-the-nodejs-application"></a><span data-ttu-id="b6bc1-117">Zabalení aplikace Node.js</span><span class="sxs-lookup"><span data-stu-id="b6bc1-117">Packaging the Node.js application</span></span>
<span data-ttu-id="b6bc1-118">Tento článek předpokládá, že Node.js není nainstalována na uzly v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-118">This article assumes that Node.js is not installed on the nodes in the Service Fabric cluster.</span></span> <span data-ttu-id="b6bc1-119">V důsledku toho je nutné přidat do kořenového adresáře aplikace uzlu před balení Node.exe.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-119">As a consequence, you need to add Node.exe to the root directory of your node application before packaging.</span></span> <span data-ttu-id="b6bc1-120">Struktura adresářů aplikace Node.js (s použitím expresního webová architektura a modulu Jade šablon) by měl vypadat podobná té následující:</span><span class="sxs-lookup"><span data-stu-id="b6bc1-120">The directory structure of the Node.js application (using Express web framework and Jade template engine) should look similar to the one below:</span></span>

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

<span data-ttu-id="b6bc1-121">Jako další krok vytvoříte balíček aplikace pro aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-121">As a next step, you create an application package for the Node.js application.</span></span> <span data-ttu-id="b6bc1-122">Následující kód vytvoří balíček aplikace Service Fabric, která obsahuje aplikaci Node.js.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-122">The code below creates a Service Fabric application package that contains the Node.js application.</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

<span data-ttu-id="b6bc1-123">Níže je uveden popis parametry, které jsou používány:</span><span class="sxs-lookup"><span data-stu-id="b6bc1-123">Below is a description of the parameters that are being used:</span></span>

* <span data-ttu-id="b6bc1-124">**/ source** odkazuje na adresář aplikace, která by měla být zabalena.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-124">**/source** points to the directory of the application that should be packaged.</span></span>
* <span data-ttu-id="b6bc1-125">**/ target** definuje adresář, ve kterém by měl být balíček vytvořen.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-125">**/target** defines the directory in which the package should be created.</span></span> <span data-ttu-id="b6bc1-126">Tento adresář musí být odlišný od zdrojového adresáře.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-126">This directory has to be different from the source directory.</span></span>
* <span data-ttu-id="b6bc1-127">**příkazy/appname** definuje název aplikace existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-127">**/appname** defines the application name of the existing application.</span></span> <span data-ttu-id="b6bc1-128">Je důležité si uvědomit, že to znamená, že k názvu služby v manifestu a nikoliv k názvu aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-128">It's important to understand that this translates to the service name in the manifest, and not to the Service Fabric application name.</span></span>
* <span data-ttu-id="b6bc1-129">**/exe** definuje spustitelný soubor, který by měl Service Fabric spustíte v tomto případě `node.exe`.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-129">**/exe** defines the executable that Service Fabric is supposed to launch, in this case `node.exe`.</span></span>
* <span data-ttu-id="b6bc1-130">**/Ma** definuje argument, který slouží ke spuštění spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-130">**/ma** defines the argument that is being used to launch the executable.</span></span> <span data-ttu-id="b6bc1-131">Jako Node.js není nainstalovaná, Service Fabric musí spusťte webový server Node.js spuštěním `node.exe bin/www`.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-131">As Node.js is not installed, Service Fabric needs to launch the Node.js web server by executing `node.exe bin/www`.</span></span>  <span data-ttu-id="b6bc1-132">`/ma:'bin/www'`informuje nástroj balení používat `bin/ma` jako argument pro node.exe.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-132">`/ma:'bin/www'` tells the packaging tool to use `bin/ma` as the argument for node.exe.</span></span>
* <span data-ttu-id="b6bc1-133">**Nebo typ aplikace** definuje název typu aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-133">**/AppType** defines the Service Fabric application type name.</span></span>

<span data-ttu-id="b6bc1-134">Pokud přejdete do adresáře, která byla zadaná v parametru/Target, uvidíte, že nástroj vytvořil plně funkční balíček Service Fabric, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="b6bc1-134">If you browse to the directory that was specified in the /target parameter, you can see that the tool has created a fully functioning Service Fabric package as shown below:</span></span>

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
<span data-ttu-id="b6bc1-135">Vygenerovaný ServiceManifest.xml teď má oddíl, který popisuje, jak Node.js webového serveru musí být spuštěna, jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="b6bc1-135">The generated ServiceManifest.xml now has a section that describes how the Node.js web server should be launched, as shown in the code snippet below:</span></span>

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
<span data-ttu-id="b6bc1-136">V této ukázce Node.js webový server naslouchá na port 3000, proto musíte aktualizovat informace o koncový bod v souboru ServiceManifest.xml, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-136">In this sample, the Node.js web server listens to port 3000, so you need to update the endpoint information in the ServiceManifest.xml file as shown below.</span></span>   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a><span data-ttu-id="b6bc1-137">Balení aplikací MongoDB</span><span class="sxs-lookup"><span data-stu-id="b6bc1-137">Packaging the MongoDB application</span></span>
<span data-ttu-id="b6bc1-138">Teď, když máte zabalené aplikace Node.js, můžete přejít k tématu a balíček MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-138">Now that you have packaged the Node.js application, you can go ahead and package MongoDB.</span></span> <span data-ttu-id="b6bc1-139">Jak je uvedeno nahoře, kroky, které teď projít nejsou specifické pro Node.js a MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-139">As mentioned before, the steps that you go through now are not specific to Node.js and MongoDB.</span></span> <span data-ttu-id="b6bc1-140">Ve skutečnosti se vztahují na všechny aplikace, které jsou určené pro zabalené společně jako jednu aplikaci Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-140">In fact, they apply to all applications that are meant to be packaged together as one Service Fabric application.</span></span>  

<span data-ttu-id="b6bc1-141">Chcete-li balíček MongoDB, budete chtít Ujistěte se, že balíček Mongod.exe a Mongo.exe.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-141">To package MongoDB, you want to make sure you package Mongod.exe and Mongo.exe.</span></span> <span data-ttu-id="b6bc1-142">Obě binární soubory jsou umístěny ve `bin` adresář adresáře instalace MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-142">Both binaries are located in the `bin` directory of your MongoDB installation directory.</span></span> <span data-ttu-id="b6bc1-143">Vypadá podobná té následující adresářovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-143">The directory structure looks similar to the one below.</span></span>

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
<span data-ttu-id="b6bc1-144">Service Fabric musí začínat MongoDB podobné tomu příkaz níže, takže budete muset použít `/ma` parametr při balení MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-144">Service Fabric needs to start MongoDB with a command similar to the one below, so you need to use the `/ma` parameter when packaging MongoDB.</span></span>

```
mongod.exe --dbpath [path to data]
```
> [!NOTE]
> <span data-ttu-id="b6bc1-145">Data není se zachovají v případě selhání uzlu, když vložíte do adresáře dat MongoDB v místním adresáři uzlu.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-145">The data is not being preserved in the case of a node failure if you put the MongoDB data directory on the local directory of the node.</span></span> <span data-ttu-id="b6bc1-146">Buď použijte odolná úložiště nebo implementovat prevence ztráty dat sady replik MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-146">You should either use durable storage or implement a MongoDB replica set in order to prevent data loss.</span></span>  
>
>

<span data-ttu-id="b6bc1-147">V prostředí PowerShell nebo příkazové okno jsme spustit nástroj balení s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="b6bc1-147">In PowerShell or the command shell, we run the packaging tool with the following parameters:</span></span>

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

<span data-ttu-id="b6bc1-148">Aby bylo možné přidat do balíčku aplikace Service Fabric MongoDB, budete muset Ujistěte se, že odkazuje parametr/target na stejný adresář, který již obsahuje aplikaci manifestu spolu s aplikací Node.js.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-148">In order to add MongoDB to your Service Fabric application package, you need to make sure that the /target parameter points to the same directory that already contains the application manifest along with the Node.js application.</span></span> <span data-ttu-id="b6bc1-149">Budete také muset Ujistěte se, že používáte stejný název ApplicationType.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-149">You also need to make sure that you are using the same ApplicationType name.</span></span>

<span data-ttu-id="b6bc1-150">Pojďme vyhledejte adresář a zkontrolujte, co nástroj vytvořil.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-150">Let's browse to the directory and examine what the tool has created.</span></span>

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
<span data-ttu-id="b6bc1-151">Jak vidíte, nástroj Přidat novou složku, MongoDB, k adresáři, který obsahuje binární soubory MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-151">As you can see, the tool added a new folder, MongoDB, to the directory that contains the MongoDB binaries.</span></span> <span data-ttu-id="b6bc1-152">Pokud otevřete `ApplicationManifest.xml` souboru, zobrazí se balíček teď obsahuje aplikace Node.js i MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-152">If you open the `ApplicationManifest.xml` file, you can see that the package now contains both the Node.js application and MongoDB.</span></span> <span data-ttu-id="b6bc1-153">Následující kód ukazuje obsah manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-153">The code below shows the content of the application manifest.</span></span>

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

### <a name="publishing-the-application"></a><span data-ttu-id="b6bc1-154">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="b6bc1-154">Publishing the application</span></span>
<span data-ttu-id="b6bc1-155">Posledním krokem je k publikování aplikace pro místní cluster Service Fabric pomocí skriptů prostředí PowerShell, níže:</span><span class="sxs-lookup"><span data-stu-id="b6bc1-155">The last step is to publish the application to the local Service Fabric cluster by using the PowerShell scripts below:</span></span>

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

<span data-ttu-id="b6bc1-156">Jakmile se k místnímu clusteru se daná aplikace úspěšně publikuje, dostanete aplikace Node.js na portu, který jsme zadali v service manifest aplikace Node.js – například http://localhost: 3000.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-156">Once the application is successfully published to the local cluster, you can access the Node.js application on the port that we have entered in the service manifest of the Node.js application--for example http://localhost:3000.</span></span>

<span data-ttu-id="b6bc1-157">V tomto kurzu jste viděli, jak snadno balíček dvě existující aplikace jako jednu aplikaci Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-157">In this tutorial, you have seen how to easily package two existing applications as one Service Fabric application.</span></span> <span data-ttu-id="b6bc1-158">Naučili jste postup nasazení na Service Fabric, takže můžete využít některé funkce Service Fabric, jako například vysokou dostupnost a stavu systému integrace.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-158">You have also learned how to deploy it to Service Fabric so that it can benefit from some of the Service Fabric features, such as high availability and health system integration.</span></span>


## <a name="adding-more-guest-executables-to-an-existing-application-using-yeoman-on-linux"></a><span data-ttu-id="b6bc1-159">Přidání další spustitelné soubory hosta do existující aplikace pomocí Yeoman v systému Linux</span><span class="sxs-lookup"><span data-stu-id="b6bc1-159">Adding more guest executables to an existing application using Yeoman on Linux</span></span>

<span data-ttu-id="b6bc1-160">Pokud chcete přidat další službu do aplikace již vytvořené pomocí `yo`, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b6bc1-160">To add another service to an application already created using `yo`, perform the following steps:</span></span> 
1. <span data-ttu-id="b6bc1-161">Změňte adresář na kořenovou složku stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-161">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="b6bc1-162">Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je aplikace vytvořená pomocí Yeomanu.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-162">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="b6bc1-163">Spustit `yo azuresfguest:AddService` a zadejte potřebné detaily.</span><span class="sxs-lookup"><span data-stu-id="b6bc1-163">Run `yo azuresfguest:AddService` and provide the necessary details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6bc1-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6bc1-164">Next steps</span></span>
* <span data-ttu-id="b6bc1-165">Další informace o nasazení kontejnery s [Service Fabric a kontejnery – přehled](service-fabric-containers-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b6bc1-165">Learn about deploying containers with [Service Fabric and containers overview](service-fabric-containers-overview.md)</span></span>
* [<span data-ttu-id="b6bc1-166">Ukázka pro balení a nasazení spustitelný soubor hosta</span><span class="sxs-lookup"><span data-stu-id="b6bc1-166">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="b6bc1-167">Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes službu Naming pomocí REST</span><span class="sxs-lookup"><span data-stu-id="b6bc1-167">Sample of two guest executables (C# and nodejs) communicating via the Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
