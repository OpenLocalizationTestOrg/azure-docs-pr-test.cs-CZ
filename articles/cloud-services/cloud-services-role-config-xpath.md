---
title: "aaaCloud roli služby konfigurace XPath tahák | Microsoft Docs"
description: "Hello různá nastavení XPath, které můžete použít v tooexpose konfigurační nastavení pro hello cloudové služby role jako proměnné prostředí."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 27f98f956a1c790c9bb30f9fefe1ab1736b2b150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Vystavit nastavení konfigurace role v proměnné prostředí, jejichž výraz XPath
V souboru definice služby role webový nebo hello cloudové služby pracovního procesu můžete vystavit hodnoty konfigurace modulu runtime v proměnné prostředí. Hello následující XPath hodnoty jsou podporovány (která odpovídají tooAPI hodnoty).

Tyto hodnoty XPath jsou také k dispozici prostřednictvím hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) knihovny. 

## <a name="app-running-in-emulator"></a>Aplikaci spuštěnou v emulátoru
Označuje, že tuto aplikaci hello běží v emulátoru hello.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/RoleEnvironment/Deployment/@emulated" |
| Kód |var x = RoleEnvironment.IsEmulated; |

## <a name="deployment-id"></a>ID nasazení
Načte ID hello nasazení pro instanci hello.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/RoleEnvironment/Deployment/@id" |
| Kód |var deploymentId = RoleEnvironment.DeploymentId; |

## <a name="role-id"></a>Role ID
Načte hello aktuální ID role pro instanci hello.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/RoleEnvironment/CurrentInstance/@id" |
| Kód |var id = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>Aktualizace domény
Načte hello aktualizace domény hello instance.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kód |var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |

## <a name="fault-domain"></a>Doména selhání
Načte doména selhání hello hello instance.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kód |var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |

## <a name="role-name"></a>Název role
Načte název role hello hello instancí.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/RoleEnvironment/CurrentInstance/@roleName" |
| Kód |var rname = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Nastavení konfigurace
Načte hodnotu hello hello zadat nastavení konfigurace.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/ RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value" |
| Kód |var nastavení = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |

## <a name="local-storage-path"></a>Cesta k místnímu úložišti
Načte cestu hello místní úložiště pro instanci hello.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path" |
| Kód |var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |

## <a name="local-storage-size"></a>Velikost místní úložiště
Načte hello velikost hello místní úložiště pro instanci hello.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/ RoleEnvironment/CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB" |
| Kód |var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Koncový bod protokolu
Načte hello koncový bod protokolu pro instanci hello.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@protocol" |
| Kód |var ochranu = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protokol; |

## <a name="endpoint-ip"></a>Koncový bod IP
Získá hello zadaný pro koncový bod IP adresu.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@address" |
| Kód |var adresu = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>koncový port
Načte hello koncový port pro instanci hello.

| Typ | Příklad |
| --- | --- |
| Výraz XPath |výraz XPath = "/ RoleEnvironment nebo CurrentInstance nebo koncových bodů nebo koncového bodu [@name= 'koncovém bodě 1']/@port" |
| Kód |var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |

## <a name="example"></a>Příklad
Tady je příklad role pracovního procesu, který vytváří úloha spuštění s proměnnou prostředí s názvem `TestIsEmulated` nastavit toohello [ @emulated hodnotu xpath](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Další kroky
Další informace o hello [souboru ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) souboru.

Vytvoření [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) balíčku.

Povolit [vzdálené plochy](cloud-services-role-enable-remote-desktop.md) pro roli.

