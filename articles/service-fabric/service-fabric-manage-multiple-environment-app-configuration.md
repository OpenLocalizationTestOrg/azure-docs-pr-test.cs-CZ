---
title: "aaaManage prostředí s více v Service Fabric | Microsoft Docs"
description: "Service Fabric aplikace můžete spustit na clustery, které rozsahu velikost z jednoho počítače toothousands počítačů. V některých případech můžete tooconfigure aplikace pro tato rozmanitých prostředí. Tento článek popisuje jak toodefine jinou aplikaci parametry podle prostředí."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 2b3327e0e1a3bbd35a50835e720619f308b1b501
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a>Spravovat aplikace parametry pro prostředí s více
Můžete vytvořit clustery Azure Service Fabric pomocí kdekoli z jednoho toomany tisíc počítačů. Při binární soubory aplikace můžete spustit bez úprav přes tento široké spektrum prostředí, často chcete tooconfigure hello aplikace odlišně, v závislosti na hello počtu počítačů, které nasazujete na.

Jako jednoduchý příklad, zvažte `InstanceCount` bezstavové služby. Když aplikace běží v Azure, obvykle chcete tooset tento parametr toohello speciální hodnotu-1. Tato konfigurace zajistí, že vaše služba běží na všech uzlech v clusteru hello (nebo každý uzel v uzlu typu hello Pokud jste nastavili omezení umístění). Tato konfigurace však není vhodný pro cluster jednoho počítače, protože nemůže mít více procesy naslouchání na hello stejný koncový bod na jednom počítači. Místo toho je obvykle nastavit `InstanceCount` příliš "1".

## <a name="specifying-environment-specific-parameters"></a>Zadání parametrů specifické pro prostředí
problém s konfigurací toothis řešení Hello je sada parametrizované výchozích služeb a soubory parametrů aplikace, které zadejte tyto hodnoty parametrů pro dané prostředí. Výchozí služby a aplikace parametry jsou nakonfigurovaní v hello aplikace a služby manifesty. Hello definice schématu hello ServiceManifest.xml a ApplicationManifest.xml souborů se instaluje s hello Service Fabric SDK a nástrojů pro příliš*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Výchozí služby
Aplikace Service Fabric se skládají z kolekce instancí služby. Když je možné, toocreate prázdnou aplikaci a pak vytvořte všechny instance služby dynamicky, většina aplikací mít sadu základní služby, které mají být vytvořeny vždy při vytváření instance aplikace hello. Jedná se o odkazované tooas "výchozí služby". Jsou uvedené v manifestu aplikace hello se zástupnými symboly pro konfiguraci za prostředí, které jsou součástí hranaté závorky:

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

Každý hello pojmenované parametry musí být definován v rámci elementu parametry hello manifest aplikace hello:

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

DefaultValue – atribut Hello určuje toobe hodnota hello používá hello neexistence parametr informace specifické pro dané prostředí.

> [!NOTE]
> Všechny parametry instance služby jsou vhodné pro konfiguraci podle prostředí. V předchozím příkladu hello hello LowKey a HighKey hodnoty pro schéma rozdělení oddílů hello služby jsou explicitně definovány pro všechny instance služby hello vzhledem k tomu, že rozsah oddílu hello je funkce hello data domény, ne hello prostředí.
> 
> 

### <a name="per-environment-service-configuration-settings"></a>Nastavení konfigurace služby za prostředí
Hello [model aplikace Service Fabric](service-fabric-application-model.md) umožňuje služby tooinclude konfigurace balíčky, které obsahují vlastní páry klíč hodnota, které jsou v době běhu čitelné. Hello hodnoty těchto nastavení můžete také rozlišené pomocí prostředí tak, že zadáte `ConfigOverride` v manifestu aplikace hello.

Předpokládejme, že máte následující nastavení v souboru Config\Settings.xml hello hello hello `Stateful1` služby:

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
vytvoření této hodnoty pro dvojici konkrétní aplikaci nebo prostředí toooverride `ConfigOverride` při importu hello service manifest v manifestu aplikace hello.

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
Tento parametr můžete pak nakonfigurovat prostředí, jak je uvedeno výše. To provedete tak, že deklarace v části Parametry hello hello manifest aplikace a zadání hodnoty v závislosti na prostředí v soubory parametrů aplikace hello.

> [!NOTE]
> V případě hello nastavení konfigurace služby jsou tři místa, kde můžete nastavit hello hodnoty klíče: hello balíček konfigurace služby, manifest aplikace hello a soubor parametrů aplikace hello. Service Fabric se vždycky zvolte ze souboru parametrů aplikace hello nejprve (Pokud je zadána), pak hello manifest aplikace a nakonec hello konfigurační balíček.
> 
> 

### <a name="setting-and-using-environment-variables"></a>Nastavení a použití proměnných prostředí 
Můžete zadat a nastavení proměnných prostředí v souboru ServiceManifest.xml hello a pak přepsat hello ApplicationManifest.xml do souboru na základě za instance.
Hello níže uvedený příklad dvou proměnných prostředí, jeden s hodnotou nastavte a hello jiných přepsána. Parametry aplikačního proměnné prostředí tooset hodnoty v hello stejný způsobem, že tyto se používaly k přepsání konfigurace můžete použít.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```
proměnné prostředí hello toooverride v hello ApplicationManifest.xml, odkaz na balíček kódu hello v hello ServiceManifest s hello `EnvironmentOverrides` elementu.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 Po vytvoření hello s názvem instance služby je přístup k proměnné prostředí hello z kódu. například v C# můžete provést následující hello

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a>Proměnné prostředí Service Fabric
Service Fabric má vestavěné proměnné prostředí, nastavte pro každou instanci služby. Hello úplný seznam proměnných prostředí je níže, kde hello těch, které jsou v tučné jsou hello šablony, které budete používat ve službě, hello jiné se používá modulu runtime Service Fabric. 

* Fabric_ApplicationHostId
* Fabric_ApplicationHostType
* Fabric_ApplicationId
* **Fabric_ApplicationName**
* Fabric_CodePackageInstanceId
* **Fabric_CodePackageName**
* **TypeEndpoint Fabric_Endpoint_ [YourServiceName]**
* **Fabric_Folder_App_Log**
* **Fabric_Folder_App_Temp**
* **Fabric_Folder_App_Work**
* **Fabric_Folder_Application**
* Fabric_NodeId
* **Fabric_NodeIPOrFQDN**
* **Fabric_NodeName**
* Fabric_RuntimeConnectionAddress
* Fabric_ServicePackageInstanceId
* Fabric_ServicePackageName
* Fabric_ServicePackageVersionInstance
* FabricPackageFileName

belows Hello kód ukazuje, jak toolist hello proměnné prostředí Service Fabric
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
Hello Následují příklady proměnných prostředí pro typ aplikace volá `GuestExe.Application` volat s typem služby `FrontEndService` při spuštění v místním vývojářském počítači.

* **Fabric_ApplicationName = fabric:/GuestExe.Application**
* **Fabric_CodePackageName = kód**
* **Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**
* **Fabric_NodeIPOrFQDN = localhost**
* **Fabric_NodeName = to uzel _Node_2**

### <a name="application-parameter-files"></a>Soubory parametrů aplikace
projekt aplikace Hello Service Fabric může obsahovat jeden nebo více soubory parametrů aplikace. Každý z nich definuje hello konkrétní hodnoty pro parametry hello, které jsou definovány v manifestu aplikace hello:

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
Ve výchozím nastavení zahrnuje tři soubory parametrů aplikace s názvem Local.1Node.xml, Local.5Node.xml a Cloud.xml nové aplikace:

![Soubory parametrů aplikace v Průzkumníku řešení][app-parameters-solution-explorer]

toocreate soubor parametru jednoduše zkopírujte a vložte stávající a dejte mu nový název.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identifikace konkrétní prostředí parametrů během nasazování
Při nasazení je nutné toochoose hello odpovídající parametr souboru tooapply s vaší aplikací. Můžete to provést prostřednictvím dialogu hello publikovat v sadě Visual Studio nebo pomocí prostředí PowerShell.

### <a name="deploy-from-visual-studio"></a>Nasazení z Visual Studia
Můžete zvolit z hello seznam souborů k dispozici parametr při publikování aplikace v sadě Visual Studio.

![Vyberte soubor s parametry v dialogovém okně Publikovat hello][publishdialog]

### <a name="deploy-from-powershell"></a>Nasazení z prostředí PowerShell
Hello `Deploy-FabricApplication.ps1` skript prostředí PowerShell, které jsou součástí šablony projektu aplikace hello přijme profil publikování jako parametr a hello PublishProfile obsahuje soubor odkaz na aplikaci toohello parametry.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Další kroky
toolearn Další informace o některých hello základní koncepty, které jsou popsané v tomto tématu najdete v části hello [Service Fabric technický přehled](service-fabric-technical-overview.md). Informace o dalším funkcím správy aplikace, které jsou k dispozici v sadě Visual Studio najdete v tématu [spravovat aplikace Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
