---
title: "aaaManage aplikace v sadě Visual Studio | Microsoft Docs"
description: "Pomocí sady Visual Studio toocreate, vývoj, balíčku, nasazení a ladění aplikací Service Fabric a služeb."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: b2d5803d85e4f9645dcbece33a2208bc0955498d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-toosimplify-writing-and-managing-your-service-fabric-applications"></a>Pomocí sady Visual Studio toosimplify zápis a správu aplikací Service Fabric
Můžete spravovat vaše aplikace Azure Service Fabric a služby pomocí sady Visual Studio. Jakmile jste [nastavení vývojového prostředí](service-fabric-get-started.md), můžete používat aplikace Service Fabric toocreate Visual Studio, přidejte služby nebo balíček, registrace a nasazení aplikací v místní vývojový cluster.

## <a name="deploy-your-service-fabric-application"></a>Nasazení aplikace Service Fabric
Ve výchozím nastavení nasazení aplikace kombinuje hello následující kroky do jednoho jednoduché operace:

1. Vytvoření balíčku aplikace hello
2. Odesílání úložiště bitových kopií toohello balíčku aplikace hello
3. Registrace typu aplikace hello
4. Odebrat všechny spuštěné instance aplikace
5. Vytvoření instance aplikace

Ve Visual Studiu stisknutím **F5** nasadí aplikaci a připojte instancemi aplikace tooall hello ladicí program. Můžete použít **Ctrl + F5** toodeploy aplikace bez ladění, nebo můžete publikovat tooa místního nebo vzdáleného clusteru pomocí hello profilu publikování. Další informace najdete v tématu [publikování vzdáleného clusteru služby aplikace tooa pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Režim ladění aplikací
Visual Studio poskytují vlastnost s názvem **režim ladění aplikací**, který určuje, jakým způsobem chcete nasazení aplikace Visual Studia toohandle jako součást ladění.

#### <a name="tooset-hello-application-debug-mode-property"></a>hello tooset vlastnost režim ladění aplikací
1. Na hello Service Fabric aplikace projektu (*.sfproj) místní nabídky zvolte **vlastnosti** (nebo stiskněte klávesu hello **F4** klíč).
2. V hello **vlastnosti** okno, sada hello **režim ladění aplikací** vlastnost.

![Nastavte vlastnost režimu ladění aplikace][debugmodeproperty]

#### <a name="application-debug-modes"></a>Režim ladění aplikace

1. **Aktualizovat aplikaci** tento režim můžete změnit tooquickly a ladění kódu a podporuje úpravy statických webových souborů při ladění. Tento režim funguje jenom v případě místního vývojového clusteru je v [režim 1 uzel](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).
2. **Odebrání aplikace** příčiny hello toobe aplikace odebrat, pokud ukončení relace ladění hello.
3. **Automaticky Upgrade** aplikace hello stále toorun při ukončení relace ladění hello. Hello další relaci ladění bude považovat za hello nasazení upgradu. proces upgradu Hello se zachovají všechna data, která jste zadali v předchozí relaci ladění.
4. **Zachovat aplikace** udržuje hello aplikace spuštěné v clusteru hello při hello ladění neukončí relace. Na hello začátku hello další relaci ladění, se odeberou aplikace hello.

Pro **automatického upgradu** uchování dat s použitím možnosti upgradu hello aplikací Service Fabric. Další informace o upgrade aplikace a jak můžete provést upgrade v prostředí skutečné najdete v tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).

## <a name="add-a-service-tooyour-service-fabric-application"></a>Přidání služby tooyour aplikace Service Fabric
Můžete přidat nové služby tooyour aplikace tooextend její funkce.  tooensure hello služby je součástí balíčku aplikace, přidejte hello služby prostřednictvím hello **nové infrastruktury služby...**  položku nabídky.

![Přidejte novou službu Service Fabric][newservice]

Vyberte aplikace Service Fabric projektu typu tooadd tooyour a zadejte název pro službu hello.  V tématu [výběr rozhraní služby](service-fabric-choose-framework.md) toohelp rozhodnete služby, která zadejte toouse.

![Vyberte aplikace Service Fabric služby projektu typu tooadd tooyour][addserviceproject]

Přidání nové služby Hello tooyour řešení a stávající balíček aplikace. odkazy na službu Hello a výchozí instance služby se přidané toohello manifest aplikace, což způsobilo toobe hello služby vytvořit a spustit hello příštím nasazení aplikace hello.

![Přidání nové služby Hello tooyour manifest aplikace][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Balíček aplikace Service Fabric
aplikace hello toodeploy a jeho služeb tooa cluster, je nutné toocreate balíček aplikace.  balíček Hello organizuje manifest aplikace hello manifesty služby a jiné potřebné soubory v konkrétní rozložení.  Visual Studio nastavuje a spravuje hello balíček v projektu aplikace hello složky v adresáři 'pkg' hello.  Kliknutím na tlačítko **balíček** z hello **aplikace** vytvoří místní nabídky nebo aktualizace hello balíčku aplikace.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Odebrat aplikace a typy aplikací pomocí Průzkumníku cloudu
Můžete provádět operace správy základní cluster z v sadě Visual Studio pomocí Průzkumníku cloudu, který můžete spustit z hello **zobrazení** nabídky. Můžete například odstranit aplikace a zrušit zřízení typy aplikací v clusterech místního nebo vzdáleného.

![Odebrání aplikace][removeapplication]

> [!TIP]
> Funkce správy bohatší clusteru, najdete v části [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).
>
>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
* [Model aplikace Service Fabric](service-fabric-application-model.md)
* [Nasazení aplikace Service Fabric](service-fabric-deploy-remove-applications.md)
* [Správa aplikací parametry pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md)
* [Ladění aplikace Service Fabric](service-fabric-debugging-your-application.md)
* [Vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png