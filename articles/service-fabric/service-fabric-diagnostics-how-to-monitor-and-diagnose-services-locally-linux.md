---
title: "aaaDebug Azure mikroslužeb v systému Linux | Microsoft Docs"
description: "Zjistěte, jak toomonitor a diagnostikovat vaše služby vytvořené pomocí Microsoft Azure Service Fabric na místním vývojovém počítači."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 4eebe937-ab42-4429-93db-f35c26424321
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: bee47bbabcf6b84ff2da14079e026529e36a198b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a><span data-ttu-id="a700c-103">Monitorování a Diagnostika služby v instalačním programu pro vývoj místním počítači</span><span class="sxs-lookup"><span data-stu-id="a700c-103">Monitor and diagnose services in a local machine development setup</span></span>


> [!div class="op_single_selector"]
> * [<span data-ttu-id="a700c-104">Windows</span><span class="sxs-lookup"><span data-stu-id="a700c-104">Windows</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [<span data-ttu-id="a700c-105">Linux</span><span class="sxs-lookup"><span data-stu-id="a700c-105">Linux</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

<span data-ttu-id="a700c-106">Monitorování, zjišťování, Diagnostika a řešení potíží s umožňují toocontinue služby s minimálním dopadem toohello uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="a700c-106">Monitoring, detecting, diagnosing, and troubleshooting allow for services toocontinue with minimal disruption toohello user experience.</span></span> <span data-ttu-id="a700c-107">Monitorovací a diagnostické jsou kritické v skutečné nasazené produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="a700c-107">Monitoring and diagnostics are critical in an actual deployed production environment.</span></span> <span data-ttu-id="a700c-108">Přijetí model podobně jako při vývoji služeb zajistí, že tento kanál diagnostiky hello funguje, když přesunete tooa produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="a700c-108">Adopting a similar model during development of services ensures that hello diagnostic pipeline works when you move tooa production environment.</span></span> <span data-ttu-id="a700c-109">Service Fabric usnadňuje diagnostiku tooimplement vývojáři služby, který může bezproblémově pracovat v jednom počítači místní vývoj nastavení a nastavení clusteru skutečné produkční.</span><span class="sxs-lookup"><span data-stu-id="a700c-109">Service Fabric makes it easy for service developers tooimplement diagnostics that can seamlessly work across both single-machine local development setups and real-world production cluster setups.</span></span>


## <a name="debugging-service-fabric-java-applications"></a><span data-ttu-id="a700c-110">Ladění aplikací Service Fabric Java</span><span class="sxs-lookup"><span data-stu-id="a700c-110">Debugging Service Fabric Java applications</span></span>

<span data-ttu-id="a700c-111">Pro aplikace Java [více rozhraní protokolování](http://en.wikipedia.org/wiki/Java_logging_framework) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="a700c-111">For Java applications, [multiple logging frameworks](http://en.wikipedia.org/wiki/Java_logging_framework) are available.</span></span> <span data-ttu-id="a700c-112">Vzhledem k tomu `java.util.logging` je výchozí možnost hello s hello prostředí JRE, používá se také pro hello [příklady v githubu kódu](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="a700c-112">Since `java.util.logging` is hello default option with hello JRE, it is also used for hello [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  <span data-ttu-id="a700c-113">Vysvětluje, Hello následující diskusi jak tooconfigure hello `java.util.logging` framework.</span><span class="sxs-lookup"><span data-stu-id="a700c-113">hello following discussion explains how tooconfigure hello `java.util.logging` framework.</span></span>

<span data-ttu-id="a700c-114">Pomocí java.util.logging můžete přesměrovat aplikace zaznamená toomemory výstupní datové proudy, soubory konzoly nebo sokety.</span><span class="sxs-lookup"><span data-stu-id="a700c-114">Using java.util.logging you can redirect your application logs toomemory, output streams, console files, or sockets.</span></span> <span data-ttu-id="a700c-115">Pro každou z těchto možností existují výchozí obslužné rutiny už zadané v rámci hello.</span><span class="sxs-lookup"><span data-stu-id="a700c-115">For each of these options, there are default handlers already provided in hello framework.</span></span> <span data-ttu-id="a700c-116">Můžete vytvořit `app.properties` souboru tooconfigure hello obslužná rutina soubor pro vaše aplikace tooredirect všechny protokoly tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="a700c-116">You can create a `app.properties` file tooconfigure hello file handler for your application tooredirect all logs tooa local file.</span></span>

<span data-ttu-id="a700c-117">Hello následující fragment kódu obsahuje příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="a700c-117">hello following code snippet contains an example configuration:</span></span>

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

<span data-ttu-id="a700c-118">Hello složky odkazováno tooby hello `app.properties` soubor musí existovat.</span><span class="sxs-lookup"><span data-stu-id="a700c-118">hello folder pointed tooby hello `app.properties` file must exist.</span></span> <span data-ttu-id="a700c-119">Po hello `app.properties` soubor je vytvořen, musíte tooalso upravit skript vstupního bodu, `entrypoint.sh` v hello `<applicationfolder>/<servicePkg>/Code/` složky tooset hello vlastnost `java.util.logging.config.file` příliš`app.propertes` souboru.</span><span class="sxs-lookup"><span data-stu-id="a700c-119">After hello `app.properties` file is created, you need tooalso modify your entry point script, `entrypoint.sh` in hello `<applicationfolder>/<servicePkg>/Code/` folder tooset hello property `java.util.logging.config.file` too`app.propertes` file.</span></span> <span data-ttu-id="a700c-120">Položka Hello by měl vypadat podobně jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="a700c-120">hello entry should look like hello following snippet:</span></span>

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path tooapp.properties> -jar <service name>.jar
```


<span data-ttu-id="a700c-121">Tato konfigurace, které jsou výsledkem protokoly shromažďovaných rotační způsobem v `/tmp/servicefabric/logs/`.</span><span class="sxs-lookup"><span data-stu-id="a700c-121">This configuration results in logs being collected in a rotating fashion at `/tmp/servicefabric/logs/`.</span></span> <span data-ttu-id="a700c-122">soubor protokolu Hello v tomto případě je názvem mysfapp%u.%g.log kde:</span><span class="sxs-lookup"><span data-stu-id="a700c-122">hello log file in this case is named mysfapp%u.%g.log where:</span></span>
* <span data-ttu-id="a700c-123">**%u** je jedinečné číslo tooresolve konflikty mezi současně spuštěných procesů Java.</span><span class="sxs-lookup"><span data-stu-id="a700c-123">**%u** is a unique number tooresolve conflicts between simultaneous Java processes.</span></span>
* <span data-ttu-id="a700c-124">**%g** je hello generování číslo toodistinguish mezi otáčení protokoly.</span><span class="sxs-lookup"><span data-stu-id="a700c-124">**%g** is hello generation number toodistinguish between rotating logs.</span></span>

<span data-ttu-id="a700c-125">Ve výchozím nastavení Pokud žádná obslužná rutina je explicitně nakonfigurován, hello konzoly popisovač je zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="a700c-125">By default if no handler is explicitly configured, hello console handler is registered.</span></span> <span data-ttu-id="a700c-126">Protokoly hello jeden můžete zobrazit v procesu syslog pod /var/log/syslog.</span><span class="sxs-lookup"><span data-stu-id="a700c-126">One can view hello logs in syslog under /var/log/syslog.</span></span>

<span data-ttu-id="a700c-127">Další informace najdete v tématu hello [příklady v githubu kódu](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="a700c-127">For more information, see hello [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  


## <a name="debugging-service-fabric-c-applications"></a><span data-ttu-id="a700c-128">Ladění aplikací Service Fabric C#</span><span class="sxs-lookup"><span data-stu-id="a700c-128">Debugging Service Fabric C# applications</span></span>


<span data-ttu-id="a700c-129">Více rozhraní jsou k dispozici pro trasování CoreCLR aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a700c-129">Multiple frameworks are available for tracing CoreCLR applications on Linux.</span></span> <span data-ttu-id="a700c-130">Další informace najdete v tématu [Githubu: protokolování](http:/github.com/aspnet/logging).</span><span class="sxs-lookup"><span data-stu-id="a700c-130">For more information, see [GitHub: logging](http:/github.com/aspnet/logging).</span></span>  <span data-ttu-id="a700c-131">Vzhledem k tomu, že EventSource je známé tooC # vývojáře, se tento článek používá EventSource pro trasování v CoreCLR ukázky v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a700c-131">Since EventSource is familiar tooC# developers,\`this article uses EventSource for tracing in CoreCLR samples on Linux.</span></span>

<span data-ttu-id="a700c-132">prvním krokem Hello je tooinclude System.Diagnostics.Tracing tak, aby bylo možné napsat vaše protokoly toomemory, výstupní datové proudy nebo soubory konzoly.</span><span class="sxs-lookup"><span data-stu-id="a700c-132">hello first step is tooinclude System.Diagnostics.Tracing so that you can write your logs toomemory, output streams, or console files.</span></span>  <span data-ttu-id="a700c-133">Pro protokolování použití EventSource, přidejte následující project.json tooyour projektu hello:</span><span class="sxs-lookup"><span data-stu-id="a700c-133">For logging using EventSource, add hello following project tooyour project.json:</span></span>

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

<span data-ttu-id="a700c-134">Můžete použít vlastní toolisten EventListener pro událost hello služby a potom správně přesměruje je tootrace soubory.</span><span class="sxs-lookup"><span data-stu-id="a700c-134">You can use a custom EventListener toolisten for hello service event and then appropriately redirect them tootrace files.</span></span> <span data-ttu-id="a700c-135">Hello následující fragment kódu ukazuje příklad implementace protokolování událostí EventSource a vlastní EventListener:</span><span class="sxs-lookup"><span data-stu-id="a700c-135">hello following code snippet shows a sample implementation of logging using EventSource and a custom EventListener:</span></span>


```csharp

 public class ServiceEventSource : EventSource
 {
        public static ServiceEventSource Current = new ServiceEventSource();

        [NonEvent]
        public void Message(string message, params object[] args)
        {
            if (this.IsEnabled())
            {
                var finalMessage = string.Format(message, args);
                this.Message(finalMessage);
            }
        }

        // TBD: Need tooadd method for sample event.

}

```


```csharp
   internal class ServiceEventListener : EventListener
   {

        protected override void OnEventSourceCreated(EventSource eventSource)
        {
            EnableEvents(eventSource, EventLevel.LogAlways, EventKeywords.All);
        }
        protected override void OnEventWritten(EventWrittenEventArgs eventData)
        {
            using (StreamWriter Out = new StreamWriter( new FileStream("/tmp/MyServiceLog.txt", FileMode.Append)))           
        { 
                 // report all event information               
         Out.Write(" {0} ",  Write(eventData.Task.ToString(), eventData.EventName, eventData.EventId.ToString(), eventData.Level,""));
                if (eventData.Message != null)              
            Out.WriteLine(eventData.Message, eventData.Payload.ToArray());              
            else             
        { 
                    string[] sargs = eventData.Payload != null ? eventData.Payload.Select(o => o.ToString()).ToArray() : null; 
                    Out.WriteLine("({0}).", sargs != null ? string.Join(", ", sargs) : "");             
        }
           }
        }
    }
```


<span data-ttu-id="a700c-136">Hello předchozím fragmentu výstupy hello protokoly tooa souboru v `/tmp/MyServiceLog.txt`.</span><span class="sxs-lookup"><span data-stu-id="a700c-136">hello preceding snippet outputs hello logs tooa file in `/tmp/MyServiceLog.txt`.</span></span> <span data-ttu-id="a700c-137">Tento název souboru musí toobe správně aktualizován.</span><span class="sxs-lookup"><span data-stu-id="a700c-137">This file name needs toobe appropriately updated.</span></span> <span data-ttu-id="a700c-138">V případě, že chcete tooredirect hello protokoly tooconsole, použijte následující fragment kódu v třídě přizpůsobené EventListener hello:</span><span class="sxs-lookup"><span data-stu-id="a700c-138">In case you want tooredirect hello logs tooconsole, use hello following snippet in your customized EventListener class:</span></span>

```csharp
public static TextWriter Out = Console.Out;
```

<span data-ttu-id="a700c-139">Hello ukázky v [C# – ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) použít EventSource a vlastního souboru tooa EventListener toolog události.</span><span class="sxs-lookup"><span data-stu-id="a700c-139">hello samples at [C# Samples](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) use EventSource and a custom EventListener toolog events tooa file.</span></span>



## <a name="next-steps"></a><span data-ttu-id="a700c-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a700c-140">Next steps</span></span>
<span data-ttu-id="a700c-141">Hello stejný kód trasování přidat tooyour aplikace funguje taky s diagnostikou hello vaší aplikace v clusteru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="a700c-141">hello same tracing code added tooyour application also works with hello diagnostics of your application on an Azure cluster.</span></span> <span data-ttu-id="a700c-142">Podívejte se na tyto články, které popisují různé možnosti hello hello nástroje a popisují, jak tooset je nahoru.</span><span class="sxs-lookup"><span data-stu-id="a700c-142">Check out these articles that discuss hello different options for hello tools and describe how tooset them up.</span></span>
* [<span data-ttu-id="a700c-143">Jak toocollect protokoly s Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="a700c-143">How toocollect logs with Azure Diagnostics</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
