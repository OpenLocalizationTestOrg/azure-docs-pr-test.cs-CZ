---
title: "aaaAzure Service Fabric vzory a scénáře | Microsoft Docs"
description: "Další informace osvědčených postupů a předpokládá se, opakovaně použitelné vzory toodesign vývoj a provoz vaší mikroslužeb v Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: 3811420eb53d9a49e490dd2e2e5319d8dea5629c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-patterns-and-scenarios"></a>Vzory Service Fabric a scénáře
Pokud zvažujete sestavování rozsáhlých mikroslužeb pomocí Azure Service Fabric, přečtěte si od odborníků hello, kteří navržen a sestaven Tato platforma jako služba (PaaS). Začínáme s správnou architekturu a dozvíte se, jak toooptimize prostředky pro vaši aplikaci. Hello [Service Fabric Patterns and Practices](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) kurzu odpovědi nejčastěji kladené reálného zákazníci scénáře Service Fabric a v oblastech aplikace hello otázky.
 
Zjistěte, jak toodesign, vývoj a provoz vaší mikroslužeb v Service Fabric pomocí osvědčené postupy a principy, opakovaně použitelný vzorů. Získat přehled o Service Fabric a pak podrobné informace o tématech, které se týkají optimalizace clusteru a zabezpečení, migrace starší verze aplikace, IoT ve velkém měřítku, hostování herní moduly a další. Podívejte se na nastavené průběžné doručování pro různé úlohy a dokonce získat podrobnosti o hello na podporu Linux a kontejnery. 

## <a name="introduction"></a>Úvod
Prozkoumejte osvědčené postupy a informace o výběru platforma jako služba (PaaS) přes infrastruktury jako služby (IaaS). Získáte podrobnosti o hello na následující zásady designu ověřené aplikací.

<table><tr><th>Video</th><th>Balíček aplikace PowerPoint</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Úvod tooService prostředků infrastruktury</a></td></tr>
</table>

## <a name="cluster-planning-and-management"></a>Plánování a správu clusteru
Další informace o plánování kapacity, optimalizace clusteru a zabezpečení clusteru v této pohled na Azure Service Fabric.

<table><tr><th>Video</th><th>Balíček aplikace PowerPoint</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Plánování clusteru a správy</a></td></tr>
</table>

## <a name="hyper-scale-web"></a>Webové technologie Hyper škálování
Seznamte se s koncepty kolem flexibilně škálovatelné webové, včetně dostupnost a spolehlivost, flexibilně škálovatelné a správy stavu.

<table><tr><th>Video</th><th>Balíček aplikace PowerPoint</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Webové technologie Hyper škálování</a></td></tr>
</table>

## <a name="iot"></a>IoT
Prozkoumejte hello Internet věcí (IoT) v kontextu hello Azure Service Fabric, včetně hello Azure IoT kanálu, víceklientský a IoT ve velkém měřítku.

<table><tr><th>Video</th><th>Balíček aplikace PowerPoint</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></td></tr>
</table>

## <a name="gaming"></a>Hraní her
Podívejte se na na základě zapnout hry, interaktivní hry a hostování existující herní moduly.

<table><tr><th>Video</th><th>Balíček aplikace PowerPoint</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Herní</a></td></tr>
</table>

## <a name="continuous-delivery"></a>Nepřetržité doručování
Prozkoumejte koncepty, včetně nepřetržité integrace/průběžné doručování s Visual Studio Team Services, sestavení, balení/publikování pracovního postupu, nastavení více prostředí a služby nebo sdílené složky balíčku.

<table><tr><th>Video</th><th>Balíček aplikace PowerPoint</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Nastavené průběžné doručování</a></td></tr>
</table>

## <a name="migration"></a>Migrace
Další informace o migraci z cloudové služby, kromě toomigration starší verze aplikací.

<table><tr><th>Video</th><th>Balíček aplikace PowerPoint</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Migrace</a></td></tr>
</table>

## <a name="containers-and-linux-support"></a>Kontejnery a podpora Linuxu
Získat hello odpovědí toohello otázku, "Proč kontejnery?" Další informace o hello preview pro kontejnery Windows, Linux podporuje a orchestraci kontejnerů Linux. Zjistit plus, jak tooLinux aplikace toomigrate .NET Core.

<table><tr><th>Video</th><th>Balíček aplikace PowerPoint</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Kontejnery a podpora Linuxu</a></td></tr>
</table>

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili o vzory Service Fabric a scénáře, další informace o tom, jak příliš[vytváření a správě clusterů](service-fabric-deploy-anywhere.md), [migrovat tooService aplikace cloudové služby Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [nastavení nastavené průběžné doručování](service-fabric-set-up-continuous-integration.md), a [nasazení kontejnerů](service-fabric-containers-overview.md).
