---
title: "Balíček služby Azure Fabric app | Microsoft Docs"
description: "Jak balíčku aplikace Service Fabric před nasazením do clusteru."
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
ms.openlocfilehash: 486a27d7ca576c8fe1552c02eb24ece6b8bb2ba8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="package-an-application"></a><span data-ttu-id="74388-103">Balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="74388-103">Package an application</span></span>
<span data-ttu-id="74388-104">Tento článek popisuje, jak připravit pro nasazení a balíček aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="74388-104">This article describes how to package a Service Fabric application and make it ready for deployment.</span></span>

## <a name="package-layout"></a><span data-ttu-id="74388-105">Balíček rozložení</span><span class="sxs-lookup"><span data-stu-id="74388-105">Package layout</span></span>
<span data-ttu-id="74388-106">Manifest aplikace, jeden nebo více manifesty služby a další potřebný balíček soubory musí být uspořádány v konkrétní rozložení pro nasazení do clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="74388-106">The application manifest, one or more service manifests, and other necessary package files must be organized in a specific layout for deployment into a Service Fabric cluster.</span></span> <span data-ttu-id="74388-107">Manifesty příklad v tomto článku by musela být uspořádány do následující adresářovou strukturu:</span><span class="sxs-lookup"><span data-stu-id="74388-107">The example manifests in this article would need to be organized in the following directory structure:</span></span>

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

<span data-ttu-id="74388-108">Složky jsou pojmenované tak, aby odpovídala **název** atributy každý odpovídající element.</span><span class="sxs-lookup"><span data-stu-id="74388-108">The folders are named to match the **Name** attributes of each corresponding element.</span></span> <span data-ttu-id="74388-109">Například, pokud service manifest obsažené dvě kód balíčků s názvy **MyCodeA** a **MyCodeB**, pak dvě složky se stejnými názvy by obsahovat potřebné binární soubory pro každý balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="74388-109">For example, if the service manifest contained two code packages with the names **MyCodeA** and **MyCodeB**, then two folders with the same names would contain the necessary binaries for each code package.</span></span>

## <a name="use-setupentrypoint"></a><span data-ttu-id="74388-110">Použití SetupEntryPoint</span><span class="sxs-lookup"><span data-stu-id="74388-110">Use SetupEntryPoint</span></span>
<span data-ttu-id="74388-111">Typické scénáře použití **SetupEntryPoint** když budete muset spustit spustitelný soubor před spuštěním služby nebo je třeba provést operace s vyššími oprávněními.</span><span class="sxs-lookup"><span data-stu-id="74388-111">Typical scenarios for using **SetupEntryPoint** are when you need to run an executable before the service starts or you need to perform an operation with elevated privileges.</span></span> <span data-ttu-id="74388-112">Například:</span><span class="sxs-lookup"><span data-stu-id="74388-112">For example:</span></span>

* <span data-ttu-id="74388-113">Nastavení a inicializace proměnných prostředí, které potřebuje spustitelný soubor služby.</span><span class="sxs-lookup"><span data-stu-id="74388-113">Setting up and initializing environment variables that the service executable needs.</span></span> <span data-ttu-id="74388-114">Není omezeno na napsané pomocí Service Fabric programovací modely pouze spustitelné soubory.</span><span class="sxs-lookup"><span data-stu-id="74388-114">It is not limited to only executables written via the Service Fabric programming models.</span></span> <span data-ttu-id="74388-115">Například npm.exe musí některé proměnné prostředí nakonfigurované pro nasazení aplikace node.js.</span><span class="sxs-lookup"><span data-stu-id="74388-115">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="74388-116">Nastavení řízení přístupu nainstalováním certifikáty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="74388-116">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="74388-117">Další informace o tom, jak nakonfigurovat **SetupEntryPoint**, najdete v části [nakonfigurovat zásady pro bod služby instalační položka](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="74388-117">For more information on how to configure the **SetupEntryPoint**, see [Configure the policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<a id="Package-App"></a>
## <a name="configure"></a><span data-ttu-id="74388-118">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="74388-118">Configure</span></span>
### <a name="build-a-package-by-using-visual-studio"></a><span data-ttu-id="74388-119">Vytvoření balíčku pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74388-119">Build a package by using Visual Studio</span></span>
<span data-ttu-id="74388-120">Pokud chcete vytvořit aplikaci používáte Visual Studio 2015, můžete příkaz balíček automaticky vytvořit balíček, který odpovídá rozložení popsané výše.</span><span class="sxs-lookup"><span data-stu-id="74388-120">If you use Visual Studio 2015 to create your application, you can use the Package command to automatically create a package that matches the layout described above.</span></span>

<span data-ttu-id="74388-121">Chcete-li vytvořit balíček, klikněte pravým tlačítkem na projekt aplikace v Průzkumníku řešení a vyberte příkaz balíček, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="74388-121">To create a package, right-click the application project in Solution Explorer and choose the Package command, as shown below:</span></span>

![Zabalení aplikace pomocí sady Visual Studio][vs-package-command]

<span data-ttu-id="74388-123">Po dokončení balení můžete najít umístění balíčku **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="74388-123">When packaging is complete, you can find the location of the package in the **Output** window.</span></span> <span data-ttu-id="74388-124">Krok balení probíhá automaticky při nasazení nebo ladění aplikace v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74388-124">The packaging step occurs automatically when you deploy or debug your application in Visual Studio.</span></span>

### <a name="build-a-package-by-command-line"></a><span data-ttu-id="74388-125">Vytvoření balíčku pomocí příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="74388-125">Build a package by command line</span></span>
<span data-ttu-id="74388-126">Je také možné programově zabalit aplikace pomocí `msbuild.exe`.</span><span class="sxs-lookup"><span data-stu-id="74388-126">It is also possible to programmatically package up your application using `msbuild.exe`.</span></span> <span data-ttu-id="74388-127">Pod pokličkou Visual Studio je spuštěna ho tak, aby výstup stejné.</span><span class="sxs-lookup"><span data-stu-id="74388-127">Under the hood, Visual Studio is running it so the output is same.</span></span>

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a><span data-ttu-id="74388-128">Testování balíčku</span><span class="sxs-lookup"><span data-stu-id="74388-128">Test the package</span></span>
<span data-ttu-id="74388-129">Struktura balíček místně pomocí prostředí PowerShell můžete ověřit pomocí [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) příkaz.</span><span class="sxs-lookup"><span data-stu-id="74388-129">You can verify the package structure locally through PowerShell by using the [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) command.</span></span>
<span data-ttu-id="74388-130">Tento příkaz kontroluje manifest analýza problémů a ověřte všechny odkazy.</span><span class="sxs-lookup"><span data-stu-id="74388-130">This command checks for manifest parsing issues and verify all references.</span></span> <span data-ttu-id="74388-131">Příkaz pouze ověří správnost strukturální adresářů a souborů v balíčku.</span><span class="sxs-lookup"><span data-stu-id="74388-131">This command only verifies the structural correctness of the directories and files in the package.</span></span>
<span data-ttu-id="74388-132">Není ověřte žádný obsah balíčku kódu nebo data mimo kontrola, zda jsou přítomné všechny potřebné soubory.</span><span class="sxs-lookup"><span data-stu-id="74388-132">It doesn't verify any of the code or data package contents beyond checking that all necessary files are present.</span></span>

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

<span data-ttu-id="74388-133">Tato chyba ukazuje, že *MySetup.bat* souboru, kterou se odkazuje v service manifest **SetupEntryPoint** chybí balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="74388-133">This error shows that the *MySetup.bat* file referenced in the service manifest **SetupEntryPoint** is missing from the code package.</span></span> <span data-ttu-id="74388-134">Po přidání souboru chybí předá ověření aplikace:</span><span class="sxs-lookup"><span data-stu-id="74388-134">After the missing file is added, the application verification passes:</span></span>

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

<span data-ttu-id="74388-135">Pokud má vaše aplikace [parametry aplikace](service-fabric-manage-multiple-environment-app-configuration.md) definovat, můžete předat je do [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) pro správné ověření.</span><span class="sxs-lookup"><span data-stu-id="74388-135">If your application has [application parameters](service-fabric-manage-multiple-environment-app-configuration.md) defined, you can pass them in [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) for proper validation.</span></span>

<span data-ttu-id="74388-136">Pokud znáte clusteru, kde bude aplikace nasazena, je vhodné předáte v `ImageStoreConnectionString` parametr.</span><span class="sxs-lookup"><span data-stu-id="74388-136">If you know the cluster where the application will be deployed, it is recommended you pass in the `ImageStoreConnectionString` parameter.</span></span> <span data-ttu-id="74388-137">V takovém případě balíček také ověřovat na předchozí verze této aplikace, které jsou již spuštěny v clusteru.</span><span class="sxs-lookup"><span data-stu-id="74388-137">In this case, the package is also validated against previous versions of the application that are already running in the cluster.</span></span> <span data-ttu-id="74388-138">Například ověření může zjistit, zda balíček se stejnou verzí, ale už nasazená jiný obsah.</span><span class="sxs-lookup"><span data-stu-id="74388-138">For example, the validation can detect whether a package with the same version but different content was already deployed.</span></span>  

<span data-ttu-id="74388-139">Jakmile se aplikace správně zabalený a ověřením úspěšně projde, vyhodnocení na základě velikosti a počtu souborů, které je podle potřeby komprese.</span><span class="sxs-lookup"><span data-stu-id="74388-139">Once the application is packaged correctly and passes validation, evaluate based on the size and the number of files if compression is needed.</span></span>

## <a name="compress-a-package"></a><span data-ttu-id="74388-140">Komprimovat balíčku</span><span class="sxs-lookup"><span data-stu-id="74388-140">Compress a package</span></span>
<span data-ttu-id="74388-141">Pokud se balíček je velký nebo má mnoho souborů, můžete je Komprimovat pro rychlejší nasazení.</span><span class="sxs-lookup"><span data-stu-id="74388-141">When a package is large or has many files, you can compress it for faster deployment.</span></span> <span data-ttu-id="74388-142">Komprese snižuje počet souborů a velikost balíčku.</span><span class="sxs-lookup"><span data-stu-id="74388-142">Compression reduces the number of files and the package size.</span></span>
<span data-ttu-id="74388-143">Pro komprimovaný aplikace balíček [nahrávání balíčku aplikace](service-fabric-deploy-remove-applications.md#upload-the-application-package) trvat delší dobu, ve srovnání s odesílání dekomprimaci balíčku (speciálně. Pokud se započítá komprese čas), ale [registrace](service-fabric-deploy-remove-applications.md#register-the-application-package) a [zrušením registrace typu aplikace](service-fabric-deploy-remove-applications.md#unregister-an-application-type) je rychlejší balíčku komprimované aplikace.</span><span class="sxs-lookup"><span data-stu-id="74388-143">For a compressed application package, [Uploading the application package](service-fabric-deploy-remove-applications.md#upload-the-application-package) may take longer compared to uploading the uncompressed package (specially if compression time is factored in), but [registering](service-fabric-deploy-remove-applications.md#register-the-application-package) and [un-registering the application type](service-fabric-deploy-remove-applications.md#unregister-an-application-type) are faster for a compressed application package.</span></span>

<span data-ttu-id="74388-144">Tento mechanismus nasazení je stejný pro komprimované a nekomprimované balíčky.</span><span class="sxs-lookup"><span data-stu-id="74388-144">The deployment mechanism is same for compressed and uncompressed packages.</span></span> <span data-ttu-id="74388-145">Pokud je komprimovaný balíček, je jako takový uložený v úložišti clusteru bitové kopie a je nekomprimovaným na uzlu před spuštěním aplikace.</span><span class="sxs-lookup"><span data-stu-id="74388-145">If the package is compressed, it is stored as such in the cluster image store and it's uncompressed on the node before the application is run.</span></span>
<span data-ttu-id="74388-146">Komprese nahradí platný balíček Service Fabric komprimované verze.</span><span class="sxs-lookup"><span data-stu-id="74388-146">The compression replaces the valid Service Fabric package with the compressed version.</span></span> <span data-ttu-id="74388-147">Složku musíte povolit oprávnění k zápisu.</span><span class="sxs-lookup"><span data-stu-id="74388-147">The folder must allow write permissions.</span></span> <span data-ttu-id="74388-148">Komprese systémem již zkomprimovaného balíčku vypočítá žádné změny.</span><span class="sxs-lookup"><span data-stu-id="74388-148">Running compression on an already compressed package yields no changes.</span></span>

<span data-ttu-id="74388-149">Balíček lze komprimovat spuštěním příkazu prostředí Powershell [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) s `CompressPackage` přepínače.</span><span class="sxs-lookup"><span data-stu-id="74388-149">You can compress a package by running the Powershell command [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) with `CompressPackage` switch.</span></span> <span data-ttu-id="74388-150">Můžete dekomprimovat balíček se stejným příkaz `UncompressPackage` přepínače.</span><span class="sxs-lookup"><span data-stu-id="74388-150">You can uncompress the package with the same command, using `UncompressPackage` switch.</span></span>

<span data-ttu-id="74388-151">Následující příkaz komprimaci balíček bez kopírování do úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="74388-151">The following command compresses the package without copying it to the image store.</span></span> <span data-ttu-id="74388-152">Můžete zkopírovat zkomprimovaného balíčku na jeden nebo více clusterů Service Fabric, podle potřeby, pomocí [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) bez `SkipCopy` příznak.</span><span class="sxs-lookup"><span data-stu-id="74388-152">You can copy a compressed package to one or more Service Fabric clusters, as needed, using [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) without the `SkipCopy` flag.</span></span>
<span data-ttu-id="74388-153">Nyní zahrnuje komprimované soubory pro balíček `code`, `config`, a `data` balíčky.</span><span class="sxs-lookup"><span data-stu-id="74388-153">The package now includes zipped files for the `code`, `config`, and `data` packages.</span></span> <span data-ttu-id="74388-154">Manifest aplikace a služby manifestech nejsou ZIP, protože jsou potřebné pro mnoho interní operace (jako je balíček sdílení, aplikace zadejte název a verze extrakce pro určité ověření).</span><span class="sxs-lookup"><span data-stu-id="74388-154">The application manifest and the service manifests are not zipped, because they are needed for many internal operations (like package sharing, application type name and version extraction for certain validations).</span></span>
<span data-ttu-id="74388-155">Pomocí formátu ZIP manifesty by proveďte, tyto operace neefektivní.</span><span class="sxs-lookup"><span data-stu-id="74388-155">Zipping the manifests would make these operations inefficient.</span></span>

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

<span data-ttu-id="74388-156">Alternativně můžete zkomprimovat a zkopírujte balíček s [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) v jednom kroku.</span><span class="sxs-lookup"><span data-stu-id="74388-156">Alternatively, you can compress and copy the package with [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) in one step.</span></span>
<span data-ttu-id="74388-157">Pokud balíček je rozsáhlý, zadejte dostatečně vysoký vypršení časového limitu umožňující čas pro balíček komprese a nahrávání do clusteru.</span><span class="sxs-lookup"><span data-stu-id="74388-157">If the package is large, provide a high enough timeout to allow time for both the package compression and the upload to the cluster.</span></span>
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

<span data-ttu-id="74388-158">Interně Service Fabric vypočítá kontrolní součty pro balíčky aplikací pro ověření.</span><span class="sxs-lookup"><span data-stu-id="74388-158">Internally, Service Fabric computes checksums for the application packages for validation.</span></span> <span data-ttu-id="74388-159">Při použití komprese, kontrolní součty se vypočítávají v komprimované verze jednotlivých balíčků.</span><span class="sxs-lookup"><span data-stu-id="74388-159">When using compression, the checksums are computed on the zipped versions of each package.</span></span>
<span data-ttu-id="74388-160">Pokud jste zkopírovali nekomprimované verzi balíčku aplikace a chcete používat kompresi u stejného balíčku, musíte změnit verze `code`, `config`, a `data` balíčků, aby se zabránilo neshoda kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="74388-160">If you copied an uncompressed version of your application package, and you want to use compression for the same package, you must change the versions of the `code`, `config`, and `data` packages to avoid checksum mismatch.</span></span> <span data-ttu-id="74388-161">Pokud jsou balíčky beze změny, místo změníte verzi, můžete použít [rozdílové zřizování](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="74388-161">If the packages are unchanged, instead of changing the version, you can use [diff provisioning](service-fabric-application-upgrade-advanced.md).</span></span> <span data-ttu-id="74388-162">Tato možnost, nebudou obsahovat beze změny balíček místo něj odkazovat z manifestu služby.</span><span class="sxs-lookup"><span data-stu-id="74388-162">With this option, do not include the unchanged package instead reference it from the service manifest.</span></span>

<span data-ttu-id="74388-163">Podobně pokud jste nahráli komprimované verze balíčku a chcete použít dekomprimaci balíčku, musíte aktualizovat verze, aby se zabránilo neshoda kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="74388-163">Similarly, if you uploaded a compressed version of the package and you want to use an uncompressed package, you must update the versions to avoid the checksum mismatch.</span></span>

<span data-ttu-id="74388-164">Balíček je nyní zabalené správně, ověřit a komprimované (v případě potřeby), tak, aby byl připraven na [nasazení](service-fabric-deploy-remove-applications.md) na jeden nebo více clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="74388-164">The package is now packaged correctly, validated, and compressed (if needed), so it is ready for [deployment](service-fabric-deploy-remove-applications.md) to one or more Service Fabric clusters.</span></span>

### <a name="compress-packages-when-deploying-using-visual-studio"></a><span data-ttu-id="74388-165">Komprimovat balíčky při nasazení pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74388-165">Compress packages when deploying using Visual Studio</span></span>
<span data-ttu-id="74388-166">Můžete určit, aby chcete komprimovat balíčků na nasazení, přidáním Visual Studio `CopyPackageParameters` elementu, který chcete profil publikování a nastavte `CompressPackage` atribut `true`.</span><span class="sxs-lookup"><span data-stu-id="74388-166">You can instruct Visual Studio to compress packages on deployment, by adding the `CopyPackageParameters` element to your publish profile, and set the `CompressPackage` attribute to `true`.</span></span>

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a><span data-ttu-id="74388-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74388-167">Next steps</span></span>
<span data-ttu-id="74388-168">[Nasazení a odebírat aplikace] [ 10] popisuje, jak pomocí prostředí PowerShell ke správě instance aplikace</span><span class="sxs-lookup"><span data-stu-id="74388-168">[Deploy and remove applications][10] describes how to use PowerShell to manage application instances</span></span>

<span data-ttu-id="74388-169">[Správa aplikací parametry pro prostředí s více] [ 11] popisuje postup konfigurace parametrů a proměnných prostředí pro instance jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74388-169">[Managing application parameters for multiple environments][11] describes how to configure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="74388-170">[Konfigurovat zásady zabezpečení pro vaši aplikaci] [ 12] popisuje, jak ke spouštění služeb v rámci zásad zabezpečení, které omezují přístup.</span><span class="sxs-lookup"><span data-stu-id="74388-170">[Configure security policies for your application][12] describes how to run services under security policies to restrict access.</span></span>

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
