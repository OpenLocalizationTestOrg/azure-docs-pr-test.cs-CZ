---
title: upgrade hello aaaConfigure aplikace Service Fabric | Microsoft Docs
description: "Zjistěte, jak tooconfigure hello nastavení pro upgrade aplikace Service Fabric pomocí sady Microsoft Visual Studio."
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikkelhegn
ms.openlocfilehash: 8ca50aa9d911f3c98f017490c8fe29011e8d80cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Konfigurovat hello upgrade aplikace Service Fabric v sadě Visual Studio
Nástroje sady Visual Studio pro Azure Service Fabric upgradu podporují publikování toolocal nebo vzdálené clustery. Existují tři scénáře, ve kterých má tooupgrade vaší aplikace tooa novější verzi místo nahrazení aplikace hello během testování a ladění:

* Během upgradu hello nedojde ke ztrátě dat. aplikace.
* Dostupnosti zůstane vysoké, takže nebudou existovat žádné přerušení služby během upgradu hello případné že dostatek instance služby rozloženy domén upgradu.
* Vzhledem aplikaci lze spustit testy, je během upgradu.

## <a name="parameters-needed-tooupgrade"></a>Parametry potřeby tooupgrade
Můžete vybrat z dva typy nasazení: standardním nebo upgradu. Regulární nasazení vymaže všechny předchozí informace o nasazení a data v clusteru hello při upgradu nasazení se zachová. Při upgradu aplikace Service Fabric v sadě Visual Studio, musíte parametry upgradu aplikace tooprovide a Kontrola zásad stavu. Aplikace upgradu parametry nápovědy řízení hello upgrade, zatímco zásady kontroly stavu určit, zda text hello upgradu byla úspěšná. V tématu [upgradu aplikace Service Fabric: upgrade parametry](service-fabric-application-upgrade-parameters.md) další podrobnosti.

Existují tři režimy upgradu: *monitorované*, *UnmonitoredAuto*, a *UnmonitoredManual*.

* Upgrade monitorované automatizuje hello upgradu a aplikace kontrolu stavu.
* UnmonitoredAuto upgrade automatizuje hello upgrade, ale přeskočí hello Kontrola stavu aplikace.
* Když provedete UnmonitoredManual upgrade, je nutné toomanually upgrade každé upgradované domény.

Každý režimu upgradu vyžaduje různé sady parametrů. V tématu [parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md) toolearn Další informace o dostupných možnostech upgradu hello.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Upgrade aplikace Service Fabric v sadě Visual Studio
Pokud používáte tooupgrade nástroje Visual Studio Service Fabric hello aplikace Service Fabric, můžete zadat, toobe proces publikování upgrade spíše než regulární nasazení kontrolou hello **upgradu aplikace hello** zkontrolujte pole.

### <a name="tooconfigure-hello-upgrade-parameters"></a>Parametry upgradu tooconfigure hello
1. Klikněte na tlačítko hello **nastavení** tlačítko Další toohello zaškrtávací políčko. Hello **upravit parametry upgradu** zobrazí se dialogové okno. Hello **upravit parametry upgradu** dialogové okno podporuje hello monitorované UnmonitoredAuto a UnmonitoredManual upgradu režimy.
2. Vyberte režim upgradu hello má toouse a potom vyplňte hello parametr mřížky.

    Každý parametr má výchozí hodnoty. Hello volitelný parametr *DefaultServiceTypeHealthPolicy* trvá vstupní tabulky hodnota hash. Tady je příklad hello hash tabulku formát vstupu pro *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* je jiný volitelný parametr, který přebírá vstupní tabulky hash hello následující formát:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Tady je příklad reálnými:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. Pokud vyberete UnmonitoredManual režimu upgradu, musíte ručně spustit toocontinue konzoly prostředí PowerShell a dokončení procesu upgradu hello. Odkazovat příliš[upgradu aplikace Service Fabric: advanced témata](service-fabric-application-upgrade-advanced.md) toolearn jak ruční upgrade funguje.

## <a name="upgrade-an-application-by-using-powershell"></a>Upgrade aplikace pomocí prostředí PowerShell
Můžete použít tooupgrade rutiny prostředí PowerShell aplikace Service Fabric. V tématu [kurz upgradu aplikace Service Fabric](service-fabric-application-upgrade-tutorial.md) a [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) podrobné informace.

## <a name="specify-a-health-check-policy-in-hello-application-manifest-file"></a>Zadejte zásady stavu zkontrolujte v souboru manifestu aplikace hello
Všechny služby v Service Fabric aplikace může mít svůj vlastní parametry zásad stavu, které hello výchozí hodnotu přepsat. Můžete zadat tyto hodnoty parametrů v souboru manifestu aplikace hello.

Hello následující příklad ukazuje, jak zkontrolovat tooapply jedinečný stav zásad u každé služby v manifestu aplikace hello.

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Další kroky
Další informace o nasazení aplikace naleznete v tématu [nasadit existující aplikaci v Azure Service Fabric](service-fabric-deploy-existing-app.md).