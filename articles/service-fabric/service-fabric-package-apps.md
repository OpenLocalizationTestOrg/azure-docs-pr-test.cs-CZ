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
# <a name="package-an-application"></a>Balíček aplikace
Tento článek popisuje, jak toopackage aplikace Service Fabric a nastavit jej jako připravené na nasazení.

## <a name="package-layout"></a>Balíček rozložení
manifest aplikace Hello, jeden nebo více manifesty služby a další potřebný balíček, které soubory musí být uspořádány v konkrétní rozložení pro nasazení do clusteru Service Fabric. manifesty Hello příklad v tomto článku potřebovat toobe uspořádány do hello následující adresářovou strukturu:

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

Hello složky jsou pojmenované toomatch hello **název** atributy každý odpovídající element. Například pokud hello service manifest obsažené dvě kód balíčků s názvy hello **MyCodeA** a **MyCodeB**, pak dvě složky s hello by obsahovat stejné názvy hello nezbytné binární soubory pro každý kód balíček.

## <a name="use-setupentrypoint"></a>Použití SetupEntryPoint
Typické scénáře použití **SetupEntryPoint** jsou když potřebujete toorun spustitelný soubor před spuštěním služby hello nebo potřebujete tooperform operace se zvýšenými oprávněními. Například:

* Nastavení a inicializace proměnné prostředí, které hello potřebám spustitelný soubor služby. Není omezený tooonly spustitelné soubory zapisovat prostřednictvím programovací modely hello Service Fabric. Například npm.exe musí některé proměnné prostředí nakonfigurované pro nasazení aplikace node.js.
* Nastavení řízení přístupu nainstalováním certifikáty zabezpečení.

Další informace o tom, tooconfigure hello **SetupEntryPoint**, najdete v části [konfigurace hello zásady pro bod služby instalační položka](service-fabric-application-runas-security.md)

<a id="Package-App"></a>
## <a name="configure"></a>Konfigurace
### <a name="build-a-package-by-using-visual-studio"></a>Vytvoření balíčku pomocí sady Visual Studio
Pokud používáte Visual Studio 2015 toocreate vaší aplikace, můžete hello balíček příkaz tooautomatically vytvořit balíček, který odpovídá hello rozložení popsané výše.

toocreate balíčku, klikněte pravým tlačítkem na projekt aplikace hello v Průzkumníku řešení a vyberte příkaz hello balíčku, jak je uvedeno níže:

![Zabalení aplikace pomocí sady Visual Studio][vs-package-command]

Po dokončení balení hello umístění balíčku hello můžete najít v hello **výstup** okno. Krok balení Hello probíhá automaticky při nasazení nebo ladění aplikace v sadě Visual Studio.

### <a name="build-a-package-by-command-line"></a>Vytvoření balíčku pomocí příkazového řádku
Je také možné tooprogrammatically balíčku do vaší aplikace pomocí `msbuild.exe`. Pod pokličkou hello Visual Studio je spuštěna ho tak, aby výstup hello stejné.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-hello-package"></a>Balíček hello
Struktura balíček hello místně pomocí prostředí PowerShell můžete ověřit pomocí hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) příkaz.
Tento příkaz kontroluje manifest analýza problémů a ověřte všechny odkazy. Příkaz pouze ověří správnost strukturální hello hello adresářů a souborů v balíčku hello.
Není ověřte všechny hello kód nebo data obsah balíčku nad rámec kontrola, zda jsou přítomné všechny potřebné soubory.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : hello EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Tato chyba ukazuje, že hello *MySetup.bat* souboru, kterou se odkazuje v hello service manifest **SetupEntryPoint** chybí balíček kódu hello. Po přidání souboru chybí hello předá ověření aplikace hello:

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

Pokud má vaše aplikace [parametry aplikace](service-fabric-manage-multiple-environment-app-configuration.md) definovat, můžete předat je do [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) pro správné ověření.

Pokud znáte hello clusteru, kde bude aplikace hello nasazená, se doporučuje, kterou předáte hello `ImageStoreConnectionString` parametr. V takovém případě hello balíček také ověřovat na předchozích verzích hello aplikace, které jsou již spuštěny v clusteru hello. Například může zjistit hello ověření, zda balíček s hello byl již nasazen stejné verze, ale jiný obsah.  

Jakmile aplikace hello zabalený správně a ověřením úspěšně projde, vyhodnocení na základě hello velikost a hello počet souborů, pokud je potřeba komprese.

## <a name="compress-a-package"></a>Komprimovat balíčku
Pokud se balíček je velký nebo má mnoho souborů, můžete je Komprimovat pro rychlejší nasazení. Komprese snižuje počet hello souborů a velikost balíčku hello.
Pro komprimovaný aplikace balíček [balíčku aplikace hello odesílání](service-fabric-deploy-remove-applications.md#upload-the-application-package) může trvat déle porovnání toouploading hello nekomprimované balíčku (speciálně. Pokud se započítá komprese čas), ale [registrace](service-fabric-deploy-remove-applications.md#register-the-application-package) a [zrušením registrace typu aplikace hello](service-fabric-deploy-remove-applications.md#unregister-an-application-type) je rychlejší balíčku komprimované aplikace.

mechanismus pro nasazení Hello je stejný pro komprimované a nekomprimované balíčky. Pokud balíček hello je komprimován, uloží se jako takový do úložiště bitových kopií clusteru hello a je nekomprimovaným na hello uzlu před spuštěním aplikace hello.
komprese Hello nahradí platný balíček Service Fabric hello hello komprimované verze. Složka Hello musí umožňovat oprávnění k zápisu. Komprese systémem již zkomprimovaného balíčku vypočítá žádné změny.

Balíček lze komprimovat spuštěním příkazu prostředí Powershell hello [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) s `CompressPackage` přepínače. Hello můžete dekomprimovat balíček s hello stejný příkaz `UncompressPackage` přepínače.

Hello následující příkaz komprimaci hello balíček bez kopírování toohello úložiště bitových kopií. Podle potřeby můžete zkopírovat tooone zkomprimovaného balíčku nebo více clusterů Service Fabric, [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) bez hello `SkipCopy` příznak.
balíček Hello teď obsahuje komprimované soubory pro hello `code`, `config`, a `data` balíčky. manifest aplikace Hello a hello služby manifesty nejsou ZIP, protože jsou potřebné pro mnoho interních operací (např. balíček sdílení extrakci název a verze typu aplikace pro určité ověření).
Pomocí formátu ZIP manifesty hello by proveďte tyto operace neefektivní.

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

Alternativně můžete zkomprimovat a zkopírujte balíček hello s [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) v jednom kroku.
Pokud je balíček hello velký, zadejte tooallow dostatečně vysoký časový limit dobu hello balíček komprese a hello nahrávání toohello clusteru.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Interně Service Fabric vypočítá kontrolní součty pro hello balíčky aplikací pro ověření. Při použití komprese, kontrolní součty hello se vypočítávají v hello ZIP verze jednotlivých balíčků.
Pokud jste zkopírovali nekomprimované verzi balíčku aplikace a chcete toouse komprese hello stejného balíčku, musíte změnit hello verzích hello `code`, `config`, a `data` neshoda kontrolního součtu tooavoid balíčky. Pokud se nezměnila, místo změníte verzi hello hello balíčky, můžete použít [rozdílové zřizování](service-fabric-application-upgrade-advanced.md). Pomocí této možnosti nezahrnují beze změny balíček hello místo něj odkazovat z manifestu služby hello.

Podobně pokud jste nahráli komprimovanou verzi balíčku hello a chcete toouse dekomprimaci balíčku, musíte aktualizovat hello verze tooavoid hello kontrolního součtu neshoda.

Hello balíček je nyní zabalené správně, ověřit a komprimované (v případě potřeby), tak, aby byl připraven na [nasazení](service-fabric-deploy-remove-applications.md) tooone nebo další Service Fabric clusterů.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Komprimovat balíčky při nasazení pomocí sady Visual Studio
Můžete určit, aby balíčky toocompress Visual Studio na nasazení, přidáním hello `CopyPackageParameters` element tooyour profil publikování a nastavte hello `CompressPackage` atribut příliš`true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="next-steps"></a>Další kroky
[Nasazení a odebírat aplikace] [ 10] popisuje, jak instancemi aplikace toomanage toouse prostředí PowerShell

[Správa aplikací parametry pro prostředí s více] [ 11] popisuje, jak tooconfigure parametrů a proměnných prostředí pro instance jinou aplikaci.

[Konfigurovat zásady zabezpečení pro vaši aplikaci] [ 12] popisuje, jak toorun služeb v rámci přístup toorestrict zásady zabezpečení.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
