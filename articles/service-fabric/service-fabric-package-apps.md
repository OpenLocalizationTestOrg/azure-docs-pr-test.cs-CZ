---
title: aaaPackage aplikace Azure Service Fabric | Microsoft Docs
description: "Jak toopackage aplikace Service Fabric před nasazením tooa clusteru."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: b3918e1e25e532acdc9440855213e1fa364ea000
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="package-an-application"></a><span data-ttu-id="2e697-103">Balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="2e697-103">Package an application</span></span>
<span data-ttu-id="2e697-104">Tento článek popisuje, jak toopackage aplikace Service Fabric a nastavit jej jako připravené na nasazení.</span><span class="sxs-lookup"><span data-stu-id="2e697-104">This article describes how toopackage a Service Fabric application and make it ready for deployment.</span></span>

## <a name="package-layout"></a><span data-ttu-id="2e697-105">Balíček rozložení</span><span class="sxs-lookup"><span data-stu-id="2e697-105">Package layout</span></span>
<span data-ttu-id="2e697-106">manifest aplikace Hello, jeden nebo více manifesty služby a další potřebný balíček, které soubory musí být uspořádány v konkrétní rozložení pro nasazení do clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2e697-106">hello application manifest, one or more service manifests, and other necessary package files must be organized in a specific layout for deployment into a Service Fabric cluster.</span></span> <span data-ttu-id="2e697-107">manifesty Hello příklad v tomto článku potřebovat toobe uspořádány do hello následující adresářovou strukturu:</span><span class="sxs-lookup"><span data-stu-id="2e697-107">hello example manifests in this article would need toobe organized in hello following directory structure:</span></span>

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

<span data-ttu-id="2e697-108">Hello složky jsou pojmenované toomatch hello **název** atributy každý odpovídající element.</span><span class="sxs-lookup"><span data-stu-id="2e697-108">hello folders are named toomatch hello **Name** attributes of each corresponding element.</span></span> <span data-ttu-id="2e697-109">Například pokud hello service manifest obsažené dvě kód balíčků s názvy hello **MyCodeA** a **MyCodeB**, pak dvě složky s hello by obsahovat stejné názvy hello nezbytné binární soubory pro každý kód balíček.</span><span class="sxs-lookup"><span data-stu-id="2e697-109">For example, if hello service manifest contained two code packages with hello names **MyCodeA** and **MyCodeB**, then two folders with hello same names would contain hello necessary binaries for each code package.</span></span>

## <a name="use-setupentrypoint"></a><span data-ttu-id="2e697-110">Použití SetupEntryPoint</span><span class="sxs-lookup"><span data-stu-id="2e697-110">Use SetupEntryPoint</span></span>
<span data-ttu-id="2e697-111">Typické scénáře použití **SetupEntryPoint** jsou když potřebujete toorun spustitelný soubor před spuštěním služby hello nebo potřebujete tooperform operace se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="2e697-111">Typical scenarios for using **SetupEntryPoint** are when you need toorun an executable before hello service starts or you need tooperform an operation with elevated privileges.</span></span> <span data-ttu-id="2e697-112">Například:</span><span class="sxs-lookup"><span data-stu-id="2e697-112">For example:</span></span>

* <span data-ttu-id="2e697-113">Nastavení a inicializace proměnné prostředí, které hello potřebám spustitelný soubor služby.</span><span class="sxs-lookup"><span data-stu-id="2e697-113">Setting up and initializing environment variables that hello service executable needs.</span></span> <span data-ttu-id="2e697-114">Není omezený tooonly spustitelné soubory zapisovat prostřednictvím programovací modely hello Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2e697-114">It is not limited tooonly executables written via hello Service Fabric programming models.</span></span> <span data-ttu-id="2e697-115">Například npm.exe musí některé proměnné prostředí nakonfigurované pro nasazení aplikace node.js.</span><span class="sxs-lookup"><span data-stu-id="2e697-115">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="2e697-116">Nastavení řízení přístupu nainstalováním certifikáty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2e697-116">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="2e697-117">Další informace o tom, tooconfigure hello **SetupEntryPoint**, najdete v části [konfigurace hello zásady pro bod služby instalační položka](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="2e697-117">For more information on how tooconfigure hello **SetupEntryPoint**, see [Configure hello policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<a id="Package-App"></a>
## <a name="configure"></a><span data-ttu-id="2e697-118">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="2e697-118">Configure</span></span>
### <a name="build-a-package-by-using-visual-studio"></a><span data-ttu-id="2e697-119">Vytvoření balíčku pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e697-119">Build a package by using Visual Studio</span></span>
<span data-ttu-id="2e697-120">Pokud používáte Visual Studio 2015 toocreate vaší aplikace, můžete hello balíček příkaz tooautomatically vytvořit balíček, který odpovídá hello rozložení popsané výše.</span><span class="sxs-lookup"><span data-stu-id="2e697-120">If you use Visual Studio 2015 toocreate your application, you can use hello Package command tooautomatically create a package that matches hello layout described above.</span></span>

<span data-ttu-id="2e697-121">toocreate balíčku, klikněte pravým tlačítkem na projekt aplikace hello v Průzkumníku řešení a vyberte příkaz hello balíčku, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="2e697-121">toocreate a package, right-click hello application project in Solution Explorer and choose hello Package command, as shown below:</span></span>

![Zabalení aplikace pomocí sady Visual Studio][vs-package-command]

<span data-ttu-id="2e697-123">Po dokončení balení hello umístění balíčku hello můžete najít v hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="2e697-123">When packaging is complete, you can find hello location of hello package in hello **Output** window.</span></span> <span data-ttu-id="2e697-124">Krok balení Hello probíhá automaticky při nasazení nebo ladění aplikace v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2e697-124">hello packaging step occurs automatically when you deploy or debug your application in Visual Studio.</span></span>

### <a name="build-a-package-by-command-line"></a><span data-ttu-id="2e697-125">Vytvoření balíčku pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="2e697-125">Build a package by command line</span></span>
<span data-ttu-id="2e697-126">Je také možné tooprogrammatically balíčku do vaší aplikace pomocí `msbuild.exe`.</span><span class="sxs-lookup"><span data-stu-id="2e697-126">It is also possible tooprogrammatically package up your application using `msbuild.exe`.</span></span> <span data-ttu-id="2e697-127">Pod pokličkou hello Visual Studio je spuštěna ho tak, aby výstup hello stejné.</span><span class="sxs-lookup"><span data-stu-id="2e697-127">Under hello hood, Visual Studio is running it so hello output is same.</span></span>

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-hello-package"></a><span data-ttu-id="2e697-128">Balíček hello</span><span class="sxs-lookup"><span data-stu-id="2e697-128">Test hello package</span></span>
<span data-ttu-id="2e697-129">Struktura balíček hello místně pomocí prostředí PowerShell můžete ověřit pomocí hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) příkaz.</span><span class="sxs-lookup"><span data-stu-id="2e697-129">You can verify hello package structure locally through PowerShell by using hello [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span>
<span data-ttu-id="2e697-130">Tento příkaz kontroluje manifest analýza problémů a ověřte všechny odkazy.</span><span class="sxs-lookup"><span data-stu-id="2e697-130">This command checks for manifest parsing issues and verify all references.</span></span> <span data-ttu-id="2e697-131">Příkaz pouze ověří správnost strukturální hello hello adresářů a souborů v balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="2e697-131">This command only verifies hello structural correctness of hello directories and files in hello package.</span></span>
<span data-ttu-id="2e697-132">Není ověřte všechny hello kód nebo data obsah balíčku nad rámec kontrola, zda jsou přítomné všechny potřebné soubory.</span><span class="sxs-lookup"><span data-stu-id="2e697-132">It doesn't verify any of hello code or data package contents beyond checking that all necessary files are present.</span></span>

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : hello EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

<span data-ttu-id="2e697-133">Tato chyba ukazuje, že hello *MySetup.bat* souboru, kterou se odkazuje v hello service manifest **SetupEntryPoint** chybí balíček kódu hello.</span><span class="sxs-lookup"><span data-stu-id="2e697-133">This error shows that hello *MySetup.bat* file referenced in hello service manifest **SetupEntryPoint** is missing from hello code package.</span></span> <span data-ttu-id="2e697-134">Po přidání souboru chybí hello předá ověření aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="2e697-134">After hello missing file is added, hello application verification passes:</span></span>

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

<span data-ttu-id="2e697-135">Pokud má vaše aplikace [parametry aplikace](service-fabric-manage-multiple-environment-app-configuration.md) definovat, můžete předat je do [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) pro správné ověření.</span><span class="sxs-lookup"><span data-stu-id="2e697-135">If your application has [application parameters](service-fabric-manage-multiple-environment-app-configuration.md) defined, you can pass them in [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) for proper validation.</span></span>

<span data-ttu-id="2e697-136">Pokud znáte hello clusteru, kde bude aplikace hello nasazená, se doporučuje, kterou předáte hello `ImageStoreConnectionString` parametr.</span><span class="sxs-lookup"><span data-stu-id="2e697-136">If you know hello cluster where hello application will be deployed, it is recommended you pass in hello `ImageStoreConnectionString` parameter.</span></span> <span data-ttu-id="2e697-137">V takovém případě hello balíček také ověřovat na předchozích verzích hello aplikace, které jsou již spuštěny v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="2e697-137">In this case, hello package is also validated against previous versions of hello application that are already running in hello cluster.</span></span> <span data-ttu-id="2e697-138">Například může zjistit hello ověření, zda balíček s hello byl již nasazen stejné verze, ale jiný obsah.</span><span class="sxs-lookup"><span data-stu-id="2e697-138">For example, hello validation can detect whether a package with hello same version but different content was already deployed.</span></span>  

<span data-ttu-id="2e697-139">Jakmile aplikace hello zabalený správně a ověřením úspěšně projde, vyhodnocení na základě hello velikost a hello počet souborů, pokud je potřeba komprese.</span><span class="sxs-lookup"><span data-stu-id="2e697-139">Once hello application is packaged correctly and passes validation, evaluate based on hello size and hello number of files if compression is needed.</span></span>

## <a name="compress-a-package"></a><span data-ttu-id="2e697-140">Komprimovat balíčku</span><span class="sxs-lookup"><span data-stu-id="2e697-140">Compress a package</span></span>
<span data-ttu-id="2e697-141">Pokud se balíček je velký nebo má mnoho souborů, můžete je Komprimovat pro rychlejší nasazení.</span><span class="sxs-lookup"><span data-stu-id="2e697-141">When a package is large or has many files, you can compress it for faster deployment.</span></span> <span data-ttu-id="2e697-142">Komprese snižuje počet hello souborů a velikost balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="2e697-142">Compression reduces hello number of files and hello package size.</span></span>
<span data-ttu-id="2e697-143">Pro komprimovaný aplikace balíček [balíčku aplikace hello odesílání](service-fabric-deploy-remove-applications.md#upload-the-application-package) může trvat déle porovnání toouploading hello nekomprimované balíčku (speciálně. Pokud se započítá komprese čas), ale [registrace](service-fabric-deploy-remove-applications.md#register-the-application-package) a [zrušením registrace typu aplikace hello](service-fabric-deploy-remove-applications.md#unregister-an-application-type) je rychlejší balíčku komprimované aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e697-143">For a compressed application package, [Uploading hello application package](service-fabric-deploy-remove-applications.md#upload-the-application-package) may take longer compared toouploading hello uncompressed package (specially if compression time is factored in), but [registering](service-fabric-deploy-remove-applications.md#register-the-application-package) and [un-registering hello application type](service-fabric-deploy-remove-applications.md#unregister-an-application-type) are faster for a compressed application package.</span></span>

<span data-ttu-id="2e697-144">mechanismus pro nasazení Hello je stejný pro komprimované a nekomprimované balíčky.</span><span class="sxs-lookup"><span data-stu-id="2e697-144">hello deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="2e697-145">Pokud balíček hello je komprimován, uloží se jako takový do úložiště bitových kopií clusteru hello a je nekomprimovaným na hello uzlu před spuštěním aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2e697-145">If hello package is compressed, it is stored as such in hello cluster image store and it's uncompressed on hello node before hello application is run.</span></span>
<span data-ttu-id="2e697-146">komprese Hello nahradí platný balíček Service Fabric hello hello komprimované verze.</span><span class="sxs-lookup"><span data-stu-id="2e697-146">hello compression replaces hello valid Service Fabric package with hello compressed version.</span></span> <span data-ttu-id="2e697-147">Složka Hello musí umožňovat oprávnění k zápisu.</span><span class="sxs-lookup"><span data-stu-id="2e697-147">hello folder must allow write permissions.</span></span> <span data-ttu-id="2e697-148">Komprese systémem již zkomprimovaného balíčku vypočítá žádné změny.</span><span class="sxs-lookup"><span data-stu-id="2e697-148">Running compression on an already compressed package yields no changes.</span></span>

<span data-ttu-id="2e697-149">Balíček lze komprimovat spuštěním příkazu prostředí Powershell hello [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) s `CompressPackage` přepínače.</span><span class="sxs-lookup"><span data-stu-id="2e697-149">You can compress a package by running hello Powershell command [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) with `CompressPackage` switch.</span></span> <span data-ttu-id="2e697-150">Hello můžete dekomprimovat balíček s hello stejný příkaz `UncompressPackage` přepínače.</span><span class="sxs-lookup"><span data-stu-id="2e697-150">You can uncompress hello package with hello same command, using `UncompressPackage` switch.</span></span>

<span data-ttu-id="2e697-151">Hello následující příkaz komprimaci hello balíček bez kopírování toohello úložiště bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="2e697-151">hello following command compresses hello package without copying it toohello image store.</span></span> <span data-ttu-id="2e697-152">Podle potřeby můžete zkopírovat tooone zkomprimovaného balíčku nebo více clusterů Service Fabric, [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) bez hello `SkipCopy` příznak.</span><span class="sxs-lookup"><span data-stu-id="2e697-152">You can copy a compressed package tooone or more Service Fabric clusters, as needed, using [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) without hello `SkipCopy` flag.</span></span>
<span data-ttu-id="2e697-153">balíček Hello teď obsahuje komprimované soubory pro hello `code`, `config`, a `data` balíčky.</span><span class="sxs-lookup"><span data-stu-id="2e697-153">hello package now includes zipped files for hello `code`, `config`, and `data` packages.</span></span> <span data-ttu-id="2e697-154">manifest aplikace Hello a hello služby manifesty nejsou ZIP, protože jsou potřebné pro mnoho interních operací (např. balíček sdílení extrakci název a verze typu aplikace pro určité ověření).</span><span class="sxs-lookup"><span data-stu-id="2e697-154">hello application manifest and hello service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span>
<span data-ttu-id="2e697-155">Pomocí formátu ZIP manifesty hello by proveďte tyto operace neefektivní.</span><span class="sxs-lookup"><span data-stu-id="2e697-155">Zipping hello manifests would make these operations inefficient.</span></span>

```
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

<span data-ttu-id="2e697-156">Alternativně můžete zkomprimovat a zkopírujte balíček hello s [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) v jednom kroku.</span><span class="sxs-lookup"><span data-stu-id="2e697-156">Alternatively, you can compress and copy hello package with [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) in one step.</span></span>
<span data-ttu-id="2e697-157">Pokud je balíček hello velký, zadejte tooallow dostatečně vysoký časový limit dobu hello balíček komprese a hello nahrávání toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="2e697-157">If hello package is large, provide a high enough timeout tooallow time for both hello package compression and hello upload toohello cluster.</span></span>
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

<span data-ttu-id="2e697-158">Interně Service Fabric vypočítá kontrolní součty pro hello balíčky aplikací pro ověření.</span><span class="sxs-lookup"><span data-stu-id="2e697-158">Internally, Service Fabric computes checksums for hello application packages for validation.</span></span> <span data-ttu-id="2e697-159">Při použití komprese, kontrolní součty hello se vypočítávají v hello ZIP verze jednotlivých balíčků.</span><span class="sxs-lookup"><span data-stu-id="2e697-159">When using compression, hello checksums are computed on hello zipped versions of each package.</span></span>
<span data-ttu-id="2e697-160">Pokud jste zkopírovali nekomprimované verzi balíčku aplikace a chcete toouse komprese hello stejného balíčku, musíte změnit hello verzích hello `code`, `config`, a `data` neshoda kontrolního součtu tooavoid balíčky.</span><span class="sxs-lookup"><span data-stu-id="2e697-160">If you copied an uncompressed version of your application package, and you want toouse compression for hello same package, you must change hello versions of hello `code`, `config`, and `data` packages tooavoid checksum mismatch.</span></span> <span data-ttu-id="2e697-161">Pokud se nezměnila, místo změníte verzi hello hello balíčky, můžete použít [rozdílové zřizování](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="2e697-161">If hello packages are unchanged, instead of changing hello version, you can use [diff provisioning](service-fabric-application-upgrade-advanced.md).</span></span> <span data-ttu-id="2e697-162">Pomocí této možnosti nezahrnují beze změny balíček hello místo něj odkazovat z manifestu služby hello.</span><span class="sxs-lookup"><span data-stu-id="2e697-162">With this option, do not include hello unchanged package instead reference it from hello service manifest.</span></span>

<span data-ttu-id="2e697-163">Podobně pokud jste nahráli komprimovanou verzi balíčku hello a chcete toouse dekomprimaci balíčku, musíte aktualizovat hello verze tooavoid hello kontrolního součtu neshoda.</span><span class="sxs-lookup"><span data-stu-id="2e697-163">Similarly, if you uploaded a compressed version of hello package and you want toouse an uncompressed package, you must update hello versions tooavoid hello checksum mismatch.</span></span>

<span data-ttu-id="2e697-164">Hello balíček je nyní zabalené správně, ověřit a komprimované (v případě potřeby), tak, aby byl připraven na [nasazení](service-fabric-deploy-remove-applications.md) tooone nebo další Service Fabric clusterů.</span><span class="sxs-lookup"><span data-stu-id="2e697-164">hello package is now packaged correctly, validated, and compressed (if needed), so it is ready for [deployment](service-fabric-deploy-remove-applications.md) tooone or more Service Fabric clusters.</span></span>

### <a name="compress-packages-when-deploying-using-visual-studio"></a><span data-ttu-id="2e697-165">Komprimovat balíčky při nasazení pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e697-165">Compress packages when deploying using Visual Studio</span></span>
<span data-ttu-id="2e697-166">Můžete určit, aby balíčky toocompress Visual Studio na nasazení, přidáním hello `CopyPackageParameters` element tooyour profil publikování a nastavte hello `CompressPackage` atribut příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="2e697-166">You can instruct Visual Studio toocompress packages on deployment, by adding hello `CopyPackageParameters` element tooyour publish profile, and set hello `CompressPackage` attribute too`true`.</span></span>

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a><span data-ttu-id="2e697-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e697-167">Next steps</span></span>
<span data-ttu-id="2e697-168">[Nasazení a odebírat aplikace] [ 10] popisuje, jak instancemi aplikace toomanage toouse prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e697-168">[Deploy and remove applications][10] describes how toouse PowerShell toomanage application instances</span></span>

<span data-ttu-id="2e697-169">[Správa aplikací parametry pro prostředí s více] [ 11] popisuje, jak tooconfigure parametrů a proměnných prostředí pro instance jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2e697-169">[Managing application parameters for multiple environments][11] describes how tooconfigure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="2e697-170">[Konfigurovat zásady zabezpečení pro vaši aplikaci] [ 12] popisuje, jak toorun služeb v rámci přístup toorestrict zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2e697-170">[Configure security policies for your application][12] describes how toorun services under security policies toorestrict access.</span></span>

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
