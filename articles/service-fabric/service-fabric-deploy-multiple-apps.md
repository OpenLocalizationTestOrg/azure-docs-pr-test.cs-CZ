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
# <a name="deploy-multiple-guest-executables"></a>Nasazení několika hostujících spustitelných souborů
Tento článek ukazuje, jak toopackage a nasadit více hosta spustitelné soubory tooAzure Service Fabric. Pro vytváření a nasazování jeden balíček Service Fabric číst jak příliš[nasazení spustitelné tooService hosta Fabric](service-fabric-deploy-existing-app.md).

Když tento návod ukazuje, jak toodeploy aplikace s Node.js front-end, který jako úložiště dat hello používá MongoDB, můžete použít hello kroky tooany aplikace, který má závislosti na jinou aplikaci.   

Můžete použít Visual Studio tooproduce hello aplikace balíček, který obsahuje více spustitelné soubory hosta. V tématu [pomocí sady Visual Studio toopackage existující aplikaci](service-fabric-deploy-existing-app.md). Po přidání hello první hosta spustitelný soubor, klikněte pravým tlačítkem na projekt aplikace hello a vyberte hello **Přidat -> Nový Service Fabric service** tooadd hello druhý hosta spustitelný projekt toohello řešení. Poznámka: Pokud se rozhodnete, že zdroj hello toolink v hello projektu sady Visual Studio, vytváření hello řešení sady Visual Studio, se ujistěte se, že balíčku aplikace zapnutý toodate se změnami ve zdroji hello. 

## <a name="samples"></a>Ukázky
* [Ukázka pro balení a nasazení spustitelný soubor hosta](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes hello pojmenování službu pomocí REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="manually-package-hello-multiple-guest-executable-application"></a>Ručně balíček hello více hosta spustitelná aplikace
Případně můžete ručně balíček hello hosta spustitelný soubor. Pro ruční balení hello, tento článek používá hello Service Fabric balení nástroj, který je k dispozici na [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-hello-nodejs-application"></a>Balení hello aplikace Node.js
Tento článek předpokládá, že není nainstalována Node.js hello uzlů v clusteru Service Fabric hello. V důsledku toho je nutné tooadd Node.exe toohello kořenový adresář aplikace uzlu před balení. struktura adresářů Hello aplikace Node.js hello (s použitím expresního webová architektura a modulu Jade šablon) by měl vypadat podobně jako toohello jeden níže:

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

Jako další krok vytvoříte balíček aplikace pro aplikace Node.js hello. Následující kód Hello vytvoří balíček aplikace Service Fabric, která obsahuje aplikaci Node.js hello.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Níže je uveden popis hello parametry, které jsou používány:

* **/ source** body toohello adresáře hello aplikace, který by měl být zabalen.
* **/ target** definuje hello adresář, ve které hello by měl být balíček vytvořen. Tento adresář má toobe liší od hello zdrojový adresář.
* **příkazy/appname** definuje název aplikace hello hello existující aplikace. Je důležité toounderstand, to znamená, že název služby toohello v manifestu hello a není toohello název aplikace Service Fabric.
* **/exe** definuje hello spustitelný soubor, že Service Fabric je v tomto případě by měl toolaunch, `node.exe`.
* **/Ma** definuje hello argument, který se použité toolaunch hello spustitelný soubor. Jako Node.js není nainstalovaná, Service Fabric musí toolaunch hello Node.js webový server tak, že spustíte `node.exe bin/www`.  `/ma:'bin/www'`informuje hello balení nástroj toouse `bin/ma` jako hello argument node.exe.
* **Nebo typ aplikace** definuje název typu aplikace Service Fabric hello.

Pokud toohello adresář, který byl zadaný v parametru/Target hello, můžete zjistit, že tento nástroj hello vytvořil plně funkční balíček Service Fabric, jak je uvedeno níže:

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
Hello generovaného ServiceManifest.xml teď má oddíl, který popisuje, jak hello Node.js webového serveru musí být spuštěna, jak je znázorněno v následující fragment kódu hello:

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
V této ukázce hello Node.js webový server naslouchá tooport 3000, takže budete muset tooupdate hello koncový bod, informace v souboru ServiceManifest.xml hello, jak je uvedeno níže.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-hello-mongodb-application"></a>Balení hello MongoDB aplikace
Teď, když máte zabalené aplikace Node.js hello, můžete přejít k tématu a balíček MongoDB. Jak je uvedeno nahoře, nejsou hello kroky, které teď projít konkrétní tooNode.js a MongoDB. Vztahují se ve skutečnosti tooall aplikace, které jsou určené toobe zabalené společně jako jednu aplikaci Service Fabric.  

toopackage MongoDB, budete chtít toomake se, že balíček Mongod.exe a Mongo.exe. Obě binární soubory jsou umístěny v hello `bin` adresář adresáře instalace MongoDB. struktura adresářů Hello vypadá podobně jako toohello jeden níže.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric potřebuje toostart MongoDB s příkaz podobné toohello, jeden níže, takže musíte toouse hello `/ma` parametr při balení MongoDB.

```
mongod.exe --dbpath [path toodata]
```
> [!NOTE]
> Hello data není právě zachována hello v případě selhání uzlu, když vložíte hello MongoDB data adresáře v místním adresáři hello hello uzlu. Buď použijte odolná úložiště nebo implementaci sady dojít ke ztrátě dat. pořadí tooprevent replik MongoDB.  
>
>

V příkazovém prostředí PowerShell nebo hello jsme hello balení nástroj spustit s hello následující parametry:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path toodata]' /AppType:NodeAppType
```

V pořadí tooadd MongoDB tooyour Service Fabric balíčku aplikace, musíte se, že parametr/target hello odkazují toohello toomake stejný adresář, který již obsahuje manifest aplikace hello spolu s aplikací Node.js hello. Musíte taky toomake jistotu, že používáte stejný název ApplicationType hello.

Umožňuje procházet adresář toohello a prozkoumat co hello nástroj vytvořil.

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
Jak vidíte, nástroj hello přidat nový adresář toohello složky, MongoDB, který obsahuje binární soubory MongoDB hello. Pokud otevřete hello `ApplicationManifest.xml` souboru, zobrazí se tento balíček hello nyní obsahuje aplikace Node.js hello a MongoDB. Následující kód Hello ukazuje hello obsah hello manifest aplikace.

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

### <a name="publishing-hello-application"></a>Publikování aplikace hello
posledním krokem Hello je toopublish hello aplikace toohello místní cluster Service Fabric pomocí skriptů prostředí PowerShell hello níže:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Jakmile aplikace hello úspěšně publikovaných toohello místní cluster, můžete přistupovat aplikace Node.js hello na hello portu, které jsme zadali v hello service manifest aplikace Node.js hello – například http://localhost: 3000.

V tomto kurzu jste viděli, jak tooeasily balíček dvě existující aplikace jako jednu aplikaci Service Fabric. Naučili jste se jak toodeploy ho tooService prostředků infrastruktury, které se mohou těžit z výhod některé hello Service Fabric funkce, jako je vysoká dostupnost a stavu systému integrace.


## <a name="adding-more-guest-executables-tooan-existing-application-using-yeoman-on-linux"></a>Přidání další hosta spustitelné soubory tooan existující aplikace pomocí Yeoman v systému Linux

tooadd jiná tooan aplikace služby již vytvořené pomocí `yo`, proveďte následující kroky hello: 
1. Změnit kořenový adresář toohello hello existující aplikace.  Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je vytvořený Yeoman aplikace hello.
2. Spustit `yo azuresfguest:AddService` a zadejte potřebné detaily hello.

## <a name="next-steps"></a>Další kroky
* Další informace o nasazení kontejnery s [Service Fabric a kontejnery – přehled](service-fabric-containers-overview.md)
* [Ukázka pro balení a nasazení spustitelný soubor hosta](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes hello pojmenování službu pomocí REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
