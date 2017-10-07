---
title: "aaaCreate spolehlivé služby Azure Service Fabric pomocí C#"
description: "Vytvoření, nasazení a ladění aplikace spolehlivé služby postavené na Azure Service Fabric pomocí sady Visual Studio."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 740c866da6e639219b529fe92ed63cbeaa702a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Vytvoření první aplikace spolehlivé stavové služby Service Fabric v jazyce C#

Zjistěte, jak toodeploy vaší první aplikace Service Fabric pro .NET v systému Windows za několik minut. Jakmile budete hotovi, budete mít funkční místní cluster s aplikací spolehlivé služby.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md). To zahrnuje, instalací hello Service Fabric SDK a Visual Studio 2017 nebo 2015.

## <a name="create-hello-application"></a>Vytvoření aplikace hello

Spusťte sadu Visual Studio jako **správce**.

Vytvořte projekt pomocí klávesové zkratky `CTRL`+`SHIFT`+`N`.

V hello **nový projekt** dialogovém okně, vyberte **Cloud > aplikace Service Fabric**.

Název aplikace hello **Moje_aplikace** a stiskněte klávesu **OK**.

   
![Dialogové okno Nový projekt ve Visual Studiu][1]

Můžete vytvořit jakýkoli typ aplikace Service Fabric z dialogového okna Další hello. Pro účely tohoto Rychlého startu zvolte **Stavová služba**.

Název služby hello **MyStatefulService** a stiskněte klávesu **OK**.

![Dialogové okno Nová služba ve Visual Studiu][2]


Visual Studio vytvoří projekt aplikace hello a projekt stavové služby hello a zobrazí je v Průzkumníku řešení.

![Průzkumník řešení po vytvoření aplikace se stavovou službou][3]

projekt aplikace Hello (**Moje_aplikace**) přímo neobsahuje žádný kód. Odkazuje ale na sadu projektů služeb. Kromě toho obsahuje další tři typy obsahu:

* **Profily publikování**  
Profily pro nasazení toodifferent prostředí.

* **Skripty**  
Skript PowerShellu pro nasazení/upgrade aplikace.

* **Definice aplikace**  
Zahrnuje hello ApplicationManifest.xml souboru pod *ApplicationPackageRoot* který popisuje složení vaší aplikace. Soubory parametrů přidružené aplikace jsou v části *ApplicationParameters*, které lze použít toospecify parametry konkrétní prostředí. Visual Studio vybere parametr souboru aplikace, který je uveden v hello přidružené profil publikování se během nasazení tooa konkrétní prostředí.
    
Přehled hello obsah hello projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-hello-application"></a>Nasazení a ladění aplikace hello

Teď, když máte aplikaci, ji spusťte.

V sadě Visual Studio, stiskněte klávesu `F5` aplikace hello toodeploy pro ladění.

>[!NOTE]
>Hello poprvé spustíte a nasadit hello aplikaci místně, Visual Studio vytvoří místní cluster pro ladění. To může nějakou dobu trvat. Stav vytváření clusteru Hello se zobrazí v okně výstupu sady Visual Studio hello.

Když hello clusteru je připraven, zobrazí oznámení z hello místní cluster systému panelu Správce aplikace součástí hello SDK.
   
![Upozornění na hlavním panelu systému místního clusteru][4]

Jednou hello aplikace spustí, Visual Studio automaticky vyvolá hello **prohlížeč diagnostických událostí**, kde uvidíte výstup trasování z vašich služeb.
   
![Prohlížeč diagnostických událostí][5]

Hello šablony stavové služby jsme použili jednoduše zobrazuje zvyšování hodnoty čítačů v hello `RunAsync` metodu **souboru MyStatefulService.cs**.

Rozbalte jednu hello události toosee další podrobnosti, včetně hello uzlu, kde je spuštěn hello kód. V tomto případě je to uzel \_Node\_2, to se ale může na vašem počítači lišit.
   
![Podrobnosti v prohlížeči diagnostických událostí][6]

Hello místní cluster obsahuje pět uzlů hostovaných na jednom počítači. V produkčním prostředí je každý uzel hostovaný na odlišném fyzickém nebo virtuálním počítači. toosimulate hello ztrátě počítače při výkonu hello Visual Studio ladicího programu na hello stejný čas, Zkusme teď jeden z uzlů hello na místní cluster hello.

V hello **Průzkumníku řešení** okně Otevřít **souboru MyStatefulService.cs**. 

Najde hello `RunAsync` metoda a sadu zarážku na první řádek metody hello hello.

![Zarážka v metodě RunAsync stavové služby][7]

Spusťte hello **Service Fabric Explorer** nástroj kliknutím pravým tlačítkem na hello **Local Cluster Manager** aplikace na hlavním panelu systému a zvolte **spravovat místní Cluster**.

![Spusťte Service Fabric Explorer z hello Local Cluster Manager][systray-launch-sfx]

[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) nabízí vizuální znázornění clusteru. Obsahuje sadu hello aplikace nasazené tooit a hello sady fyzických uzlů, které ho tvoří.

V levém podokně hello rozbalte **Cluster > uzly** a najít hello uzlu, kde běží váš kód.

Klikněte na tlačítko **akce > Deaktivovat (restartovat)** toosimulate restartování počítače.

![Zastavení uzlu v Service Fabric Exploreru][sfx-stop-node]

Na okamžik, měli byste vidět vaší zarážce dosáhl v sadě Visual Studio jako výpočetní hello jste dělali v jednom uzlu plynule převezme tooanother.


V dalším kroku vrátit toohello prohlížeče diagnostických událostí a sledovat hello zprávy. Hello čítače pořád roste, i když hello události ve skutečnosti přicházejí z jiného uzlu.

![Prohlížeč diagnostických událostí po převzetí služeb při selhání][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-hello-local-cluster-optional"></a>Čištění hello místní cluster (volitelné)

Pamatujte, že tento místní cluster je skutečný. Zastavení ladicího programu hello odebere vaší instanci aplikace a zrušení registrace typu aplikace hello. Hello cluster však stále toorun hello pozadí. Jakmile budete místní cluster připravený toostop hello, existuje několik možností.

### <a name="keep-application-and-trace-data"></a>Zachování dat aplikace a trasování

Vypnout hello clusteru kliknutím pravým tlačítkem na hello **Local Cluster Manager** aplikace na hlavním panelu systému a potom zvolte **Stop Local Cluster**.

### <a name="delete-hello-cluster-and-all-data"></a>Odstranění clusteru hello a všechna data

Odebrat hello cluster kliknutím pravým tlačítkem na hello **Local Cluster Manager** aplikace na hlavním panelu systému a potom zvolte **odebrat místní Cluster**. 

Pokud zvolíte tuto možnost, Visual Studio bude znovu nasaďte hello clusteru hello příštím vaše práce hello aplikace. Tuto možnost zvolte, pokud toouse hello místní cluster nechystáte nějakou dobu, nebo pokud potřebujete tooreclaim prostředky.

## <a name="next-steps"></a>Další kroky
Další informace o [spolehlivých službách](service-fabric-reliable-services-introduction.md).
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
