---
title: "aaaDebug Azure mikroslužeb v systému Windows | Microsoft Docs"
description: "Zjistěte, jak toomonitor a diagnostikovat vaše služby vytvořené pomocí Microsoft Azure Service Fabric na místním vývojovém počítači."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2017
ms.author: dekapur
ms.openlocfilehash: 24868aa194b8a28fa3e6de95c1de5506d912a544
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Monitorování a Diagnostika služby v instalačním programu pro vývoj místním počítači
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

Monitorování, zjišťování, Diagnostika a řešení potíží s umožňují toocontinue služby s minimálním dopadem toohello uživatelské prostředí. Při monitorování a Diagnostika jsou kritické v prostředí skutečné produkční nasazené, efektivitu hello bude záviset na přijetí model podobně jako při vývoji tooensure služby, kterou pracují při přesunutí instalace reálného tooa. Service Fabric usnadňuje diagnostiku tooimplement vývojáři služby, který může bezproblémově pracovat v jednom počítači místní vývoj nastavení a nastavení clusteru skutečné produkční.

## <a name="event-tracing-for-windows"></a>Trasování událostí pro Windows
[Trasování událostí pro systém Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) je hello doporučená technologie pro trasování zpráv v Service Fabric. Jsou některé výhody použití trasování událostí pro Windows:

* **Trasování událostí pro Windows je rychlá.** Byla vytvořena jako trasování technologie, která má minimální dopad na dobu provádění kódu.
* **Trasování událostí pro Windows funguje bezproblémově mezi místní vývojové prostředí a taky nastavení reálného clusteru.** To znamená, že nemáte toorewrite vaše trasování code po připravené toodeploy skutečné clusteru tooa kódu.
* **Service Fabric systému kód také používá trasování událostí pro Windows pro interní trasování.** To vám umožní tooview vaše trasování aplikací prokládaný. pomocí trasování systému Service Fabric. Pomáhá také toomore snadno porozumíte hello pořadí a vzájemné vztahy mezi kódu aplikace a události v základním systému hello.
* **Události trasování událostí pro Windows tooview není integrovanou podporu v nástroji Service Fabric Visual Studio.** Události trasování událostí se zobrazí v hello zobrazení diagnostických událostí sady Visual Studio, jakmile sady Visual Studio je správně nakonfigurován s Service Fabric. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>Zobrazit události systému Service Fabric v sadě Visual Studio
Service Fabric vysílá události trasování událostí pro vývojáře aplikací toohelp pochopit, co se děje v platformě hello. Pokud jste tak již neučinili, pokračujte a postupujte podle kroků hello v [vytvoření vaší první aplikace v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md). Tyto informace vám pomohou vám zprovoznění aplikace s hello diagnostiky Prohlížeč událostí zobrazuje hello trasování zpráv.

1. Pokud hello diagnostiky události okno nezobrazí automaticky, přejděte toohello **zobrazení** v sadě Visual Studio, zvolte **ostatní okna** a potom **prohlížeče diagnostických událostí**.
2. Každá událost obsahuje informace o standardní metadata, dozvíte se, že hello uzlu, služby a aplikace hello událost pochází z. Můžete také filtrovat hello seznam událostí pomocí hello **filtrování událostí** pole hello horní části okna události hello. Například můžete filtrovat podle **název uzlu** nebo **název služby.** A když se díváte na podrobnosti události, je také možné pozastavit pomocí hello **pozastavit** tlačítko hello horní části okna hello události a pokračovat později, aniž by došlo ke ztrátě událostí.
   
   ![Prohlížeč událostí diagnostiky Visual Studio](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-toohello-application-code"></a>Přidání vlastního kódu aplikace toohello vlastní trasování
šablony projektů Service Fabric Visual Studio Hello obsahovat ukázkový kód. Hello kód ukazuje, jak kód vlastní aplikace tooadd trasování událostí pro Windows trasování, zobrazující se v prohlížeči Visual Studio trasování událostí pro Windows hello společně systému trasování z Service Fabric. Hello Výhodou této metody je, že metadata se automaticky přidá tootraces a hello Visual Studio diagnostiky Prohlížeč událostí je již nakonfigurován toodisplay je.

Pro projekty vytvořené z hello **šablony služby** (bezstavové nebo stateful) jenom hledat hello `RunAsync` implementace:

1. Hello volání příliš`ServiceEventSource.Current.ServiceMessage` v hello `RunAsync` metoda ukazuje příklad vlastního trasování událostí pro Windows trasování z kódu aplikace hello.
2. V hello **ServiceEventSource.cs** souboru, můžete použít přetížení pro hello `ServiceEventSource.ServiceMessage` metoda, která má být použit pro vysoká frekvence událostí z důvodu tooperformance důvodů.

Pro projekty vytvořené z hello **šablony objektu actor** (bezstavové nebo stateful):

1. Otevřete hello **"ProjectName".cs** souboru kde *ProjectName* je název hello jste zvolili pro svůj projekt sady Visual Studio.  
2. Najít kód hello `ActorEventSource.Current.ActorMessage(this, "Doing Work");` v hello *DoWorkAsync* metoda.  Toto je příklad vlastního trasování ETW zapsána z kódu aplikace.  
3. V souboru **ActorEventSource.cs**, můžete použít přetížení pro hello `ActorEventSource.ActorMessage` metoda, která má být použit pro vysoká frekvence událostí z důvodu tooperformance důvodů.

Po přidání vlastního trasování událostí pro Windows trasování tooyour kódu služby, můžete sestavit, nasazení a spuštění aplikace hello znovu toosee vaše událostí v hello prohlížeče diagnostických událostí. Pokud při ladění aplikace hello s **F5**, hello se automaticky otevře prohlížeč diagnostických událostí.

## <a name="next-steps"></a>Další kroky
Hello stejný kód trasování, které jste přidali aplikaci tooyour výše pro místní diagnostiky bude pracovat s nástroji, které můžete použít tooview tyto události při spuštění aplikace v clusteru služby Azure. Podívejte se na tyto články, které popisují různé možnosti hello hello nástroje a popisují, jak je můžete nastavit tak.

* [Jak toocollect protokoly s Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md)
* [Seskupení událostí a kolekce pomocí EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)

