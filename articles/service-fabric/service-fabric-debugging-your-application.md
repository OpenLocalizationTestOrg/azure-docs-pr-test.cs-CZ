---
title: "aaaDebug aplikace v sadě Visual Studio | Microsoft Docs"
description: "Vylepšit hello spolehlivost a výkon vašich služeb vývoji a ladění je v sadě Visual Studio na místní vývojový cluster."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: 8d08689e82195d09f57b462631ad04fd237bc4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Ladění aplikace Service Fabric pomocí sady Visual Studio
> [!div class="op_single_selector"]
> * [Visual Studio nebo CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse nebo Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Ladění místní aplikace Service Fabric
Nasazení a ladění aplikace Azure Service Fabric v místním počítači vývoj clusteru můžete ušetřit čas a peníze. Visual Studio 2017 nebo Visual Studio 2015 můžete nasadit hello aplikace toohello místní cluster a automaticky se připojí hello ladicí program tooall instancí aplikace.

1. Spuštění místního vývojového clusteru podle následujících kroků hello v [nastavení vývojového prostředí Service Fabric](service-fabric-get-started.md).
2. Stiskněte klávesu **F5** nebo klikněte na tlačítko **ladění** > **spustit ladění**.
   
    ![Spuštění ladění aplikace][startdebugging]
3. Nastavte zarážky v kódu a krok prostřednictvím aplikace hello klepnutím na příkazy v hello **ladění** nabídky.
   
   > [!NOTE]
   > Visual Studio připojí tooall instancí aplikace. Když jste krokování kódu, může získat zarážky více procesy, které jsou výsledkem souběžných relací. Zkuste, zákaz hello zarážky poté, že máte, tím, že každé zarážky podmíněného na ID vlákna hello nebo pomocí diagnostických událostí.
   > 
   > 
4. Hello **diagnostických událostí** okno se automaticky otevře, takže si můžete prohlédnout diagnostických událostí v reálném čase.
   
    ![Zobrazení diagnostických událostí v reálném čase][diagnosticevents]
5. Můžete také otevřít hello **diagnostických událostí** okno v Průzkumníku cloudu.  V části **Service Fabric**, klikněte pravým tlačítkem na libovolný uzel a vyberte možnost **zobrazení streamování trasování**.
   
    ![Okno otevřít hello diagnostických událostí][viewdiagnosticevents]
   
    Pokud chcete toofilter trasování tooa konkrétní službu nebo aplikaci, jednoduše povolte streamování trasování na této konkrétní služby nebo aplikace.
6. Hello diagnostických událostí se zobrazí v automaticky generovány hello **ServiceEventSource.cs** souborů a jsou volat z kódu aplikace.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. Hello **diagnostických událostí** okno podporuje filtrování, pozastavení a kontrole událostí v reálném čase.  Filtr Hello je hledání jednoduchého řetězce hello událostí zprávy, včetně jeho obsah.
   
    ![Filtrovat, pozastavení a obnovení nebo zkontrolujte události v reálném čase][diagnosticeventsactions]
8. Ladění služeb je jako ladění žádnou jinou aplikaci. Bude obvykle nastavit zarážky pomocí sady Visual Studio pro snadné ladění. I když spolehlivé kolekce replikují mezi několika uzly, se stále implementovat rozhraní IEnumerable. To znamená, že můžete použít hello výsledky zobrazení v sadě Visual Studio při ladění toosee jste uložený v. Jednoduše zarážku kdekoli v kódu.
   
    ![Spuštění ladění aplikace][breakpoint]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Ladění vzdáleného aplikace Service Fabric
Pokud vaše aplikace Service Fabric běží na clusteru Service Fabric v Azure, máte možnost tooremotely ladění tyto přímo ze sady Visual Studio.

> [!NOTE]
> Funkce Hello vyžaduje [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) a [Azure SDK pro .NET 2.9](https://azure.microsoft.com/downloads/).    
> 
> 

<!-- -->
> [!WARNING]
> Vzdálené ladění je určená pro scénáře vývoje/testování a není toobe použít v produkčním prostředí kvůli hello dopad na hello spuštěných aplikací.
> 
> 

1. Přejděte tooyour clusteru v **Průzkumník cloudu**, klikněte pravým tlačítkem a vyberte možnost **povolit ladění**
   
    ![Povolit vzdálené ladění][enableremotedebugging]
   
    To bude ji hello proces povolení hello vzdálené ladění rozšíření na uzly clusteru, a také vyžaduje konfiguraci sítě.
2. Klikněte pravým tlačítkem na uzel clusteru hello v **Průzkumník cloudu**a zvolte **připojit ladicí program**
   
    ![Připojit ladicí program][attachdebugger]
3. V hello **připojit tooprocess** dialogovém okně, vyberte hello proces toodebug a klikněte na **připojit**
   
    ![Vyberte proces][chooseprocess]
   
    Název Hello hello procesu, který má tooattach k, rovná hello název název sestavení projektu služby.
   
    ladicí program Hello připojí uzly tooall spuštěn proces hello.
   
   * Všechny instance služby hello na všech uzlech v případě hello, kde jsou ladění bezstavové služby, jsou součástí relace ladění hello.
   * Pokud ladíte stavové služby, bude pouze hello primární repliky libovolného oddílu aktivní a proto zachycení pomocí ladicího programu hello. Pokud se primární repliky hello přesune během relace ladění hello, zůstanou hello zpracování této repliky součástí relace ladění hello.
   * V pořadí tooonly catch příslušné oddíly nebo instance dané služby můžete použít podmíněné zarážky tooonly přerušení na konkrétní oddíl nebo instance.
     
     ![Podmíněné zarážky][conditionalbreakpoint]
     
     > [!NOTE]
     > Aktuálně nepodporujeme ladění clusteru Service Fabric s více instancemi hello stejný název spustitelného souboru služby.
     > 
     > 
4. Jakmile dokončíte ladění aplikace, můžete zakázat hello vzdáleného ladění rozšíření kliknutím pravým tlačítkem na cluster hello v **Průzkumník cloudu** a zvolte **zakázat ladění**
   
    ![Zakázání vzdálené ladění][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Streamování trasování z uzlu vzdáleného clusteru
Můžete je taky možné toostream trasování přímo v uzlu tooVisual vzdálený cluster Studio. Tato funkce umožňuje události trasování ETW toostream, vytváří na uzlu clusteru Service Fabric.

> [!NOTE]
> Tato funkce vyžaduje [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) a [Azure SDK pro .NET 2.9](https://azure.microsoft.com/downloads/).
> Tato funkce podporuje pouze clustery spuštěná v Azure.
> 
> 

<!-- -->
> [!WARNING]
> Streamování trasování je určená pro scénáře vývoje/testování a není toobe použít v produkčním prostředí kvůli hello dopad na hello spuštěných aplikací.
> V produkční scénář měli byste tedy spoléhat na předávání událostí pomocí diagnostiky Azure.
> 
> 

1. Přejděte tooyour clusteru v **Průzkumník cloudu**, klikněte pravým tlačítkem a vyberte možnost **povolit trasování streamování**
   
    ![Povolit vzdálené streamování trasování][enablestreamingtraces]
   
    To bude ji hello proces povolení hello streamování rozšíření trasování na uzly clusteru, a také vyžaduje konfiguraci sítě.
2. Rozbalte hello **uzly** element v **Průzkumník cloudu**, klikněte pravým tlačítkem na hello uzel má toostream trasování z a zvolte **zobrazení streamování trasování**
   
    ![Zobrazení vzdáleného streamování trasování][viewremotestreamingtraces]
   
    Opakujte krok 2 pro libovolný počet uzlů tak, jak chcete toosee trasování z. Každý datový proud uzly se zobrazí v okně vyhrazené.
   
    Nyní je možné toosee hello trasování vysílaných Service Fabric a služeb. Pokud chcete zobrazit tooonly události hello toofilter konkrétní aplikaci, jednoduše zadejte název hello aplikace hello ve filtru hello.
   
    ![Zobrazení, streamování trasování][viewingstreamingtraces]
3. Jakmile dokončíte streamování trasování z clusteru, můžete zakázat vzdálené streamování trasování, kliknutím pravým tlačítkem na cluster hello v **Průzkumník cloudu** a zvolte **zakázat streamování trasování**
   
    ![Zakázání vzdálené streamování trasování][disablestreamingtraces]

## <a name="next-steps"></a>Další kroky
* [Testování služby Service Fabric](service-fabric-testability-overview.md).
* [Spravujte svoje aplikace Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
