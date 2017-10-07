---
title: "aaaHow toocontainerize vaše Azure Service Fabric mikroslužeb (preview)"
description: "Azure Service Fabric přidal nové funkce toocontainerize vaše mikroslužeb Service Fabric. Tato funkce je aktuálně ve verzi Preview."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: anmolah
editor: anmolah
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/04/2017
ms.author: anmola
ms.openlocfilehash: 6edaff73c0828707c7fa736669ba8084663d31ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocontainerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a>Jak toocontainerize vaší služby Fabric spolehlivé Services a Reliable Actors (Preview)

Service Fabric podporuje containerizing mikroslužeb Service Fabric (spolehlivé služeb a služeb na základě spolehlivého Actor). Další informace najdete v tématu [služby fabric kontejnery](service-fabric-containers-overview.md).


 Tato funkce je ve verzi preview a tento článek obsahuje hello různé kroky tooget služby běží uvnitř kontejneru.  

> [!NOTE]
> Tato funkce je ve verzi preview a není podporována v produkčním prostředí. Tato funkce v současné době funkční pro systém Windows.

## <a name="steps-toocontainerize-your-service-fabric-application"></a>Kroky toocontainerize aplikace Service Fabric

1. Otevřete aplikaci Service Fabric v sadě Visual Studio.

2. Přidání třídy [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour projektu. Hello kód v této třídě je pomocné rutiny toocorrectly zatížení hello Service Fabric binární soubory modulu runtime v aplikaci při spuštění v rámci kontejneru.

3. Pro každý balíček kódu chcete toocontainerize, inicializovat hello zavaděč v hello program Vstupní bod. Přidání statického konstruktoru hello uvedené v následující kód fragment kódu tooyour program Vstupní bod soubor hello.

  ```csharp
  namespace MyApplication
  {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is hello entry point of hello service host process.
          /// </summary>
          private static void Main()
          {
  ```

4. Sestavení a [balíček](service-fabric-package-apps.md#Package-App) projektu. toobuild a vytvořit balíček, klikněte pravým tlačítkem na projekt aplikace hello v Průzkumníku řešení a zvolte hello **balíček** příkaz.

5. Pro každý balíček kódu potřebujete toocontainerize hello spustit skript prostředí PowerShell [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1). využití Hello vypadá takto:
  ```powershell
    $codePackagePath = 'Path toohello code package toocontainerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for hello generated docker folder.'
    $applicationExeName = 'Name of hello ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  skript Hello vytvoří složku s artefakty Docker v $dockerPackageOutputDirectoryPath. Upravte soubor Docker tooexpose hello generované žádné porty, spusťte instalační program skripty atd. na základě potřeb.

6. Dále je třeba příliš[sestavení](service-fabric-get-started-containers.md#Build-Containers) a [nabízené](service-fabric-get-started-containers.md#Push-Containers) tooyour úložiště Docker kontejneru balíčku.

7. Upravte hello ApplicationManifest.xml a ServiceManifest.xml tooadd kontejneru bitové kopie, informace o úložišti, registru ověřování a port hostitele mapování. Úprava hello manifesty, najdete v části [vytvoření kontejneru aplikace Azure Service Fabric](service-fabric-get-started-containers.md). Hello definice balíčku kódu v hello služby manifestu potřebám toobe nahradí bitovou kopii odpovídající kontejneru. Ujistěte se, že toochange hello EntryPoint tooa ContainerHost typu.

  ```xml
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers tooService Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
  </EntryPoint>
  <!-- Pass environment variables tooyour container: -->    
</CodePackage>
  ```

8. Přidáte mapování portů hostitel hello Replikátor a koncový bod služby. Vzhledem k tomu, že oba tyto porty jsou přiřazeny za běhu pomocí Service Fabric, hello ContainerPort nastavena hello toouse toozero přiřazené port pro mapování.

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. tootest tuto aplikaci, musíte toodeploy ho tooa clusteru, který používá verzi 5.7 nebo vyšší. Kromě toho musí tooedit a aktualizujte hello clusteru nastavení tooenable této funkce ve verzi preview. Postupujte podle kroků hello v tomto [článku](service-fabric-cluster-fabric-settings.md) tooadd hello nastavení zobrazí další.
```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
```
10. Další [nasazení](service-fabric-deploy-remove-applications.md) hello upravit clusteru toothis balíčku aplikace.

Teď byste měli mít kontejnerizované aplikace Service Fabric spuštění clusteru.

## <a name="next-steps"></a>Další kroky
* Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-get-started-containers.md).
* Další informace o hello Service Fabric [životního cyklu aplikace](service-fabric-application-lifecycle.md).
