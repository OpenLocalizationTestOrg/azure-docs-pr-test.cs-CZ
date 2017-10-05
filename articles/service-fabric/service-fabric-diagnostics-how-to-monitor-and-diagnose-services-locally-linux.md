---
title: "Ladění Azure mikroslužeb v systému Linux | Microsoft Docs"
description: "Zjistěte, jak sledovat a diagnostikovat vaše služby vytvořené pomocí Microsoft Azure Service Fabric na místním vývojovém počítači."
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
ms.openlocfilehash: 4bc73f581f4855ebc724df19dd56fab8bf103854
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a><span data-ttu-id="1b736-103">Monitorování a Diagnostika služby v instalačním programu pro vývoj místním počítači</span><span class="sxs-lookup"><span data-stu-id="1b736-103">Monitor and diagnose services in a local machine development setup</span></span>


> [!div class="op_single_selector"]
> * [<span data-ttu-id="1b736-104">Windows</span><span class="sxs-lookup"><span data-stu-id="1b736-104">Windows</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [<span data-ttu-id="1b736-105">Linux</span><span class="sxs-lookup"><span data-stu-id="1b736-105">Linux</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

<span data-ttu-id="1b736-106">Monitorování, zjišťování, Diagnostika a řešení potíží s povolit pro služby, chcete-li pokračovat s minimálním dopadem na činnost koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1b736-106">Monitoring, detecting, diagnosing, and troubleshooting allow for services to continue with minimal disruption to the user experience.</span></span> <span data-ttu-id="1b736-107">Monitorovací a diagnostické jsou kritické v skutečné nasazené produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="1b736-107">Monitoring and diagnostics are critical in an actual deployed production environment.</span></span> <span data-ttu-id="1b736-108">Přijetí model podobně jako při vývoji služeb zajišťuje, že diagnostické kanálu funguje, když přesunete do provozního prostředí.</span><span class="sxs-lookup"><span data-stu-id="1b736-108">Adopting a similar model during development of services ensures that the diagnostic pipeline works when you move to a production environment.</span></span> <span data-ttu-id="1b736-109">Service Fabric usnadňuje vývojářům implementovat diagnostiky, které může bezproblémově fungovat v jednom počítači místní vývoj nastavení a nastavení clusteru skutečné produkční služby.</span><span class="sxs-lookup"><span data-stu-id="1b736-109">Service Fabric makes it easy for service developers to implement diagnostics that can seamlessly work across both single-machine local development setups and real-world production cluster setups.</span></span>


## <a name="debugging-service-fabric-java-applications"></a><span data-ttu-id="1b736-110">Ladění aplikací Service Fabric Java</span><span class="sxs-lookup"><span data-stu-id="1b736-110">Debugging Service Fabric Java applications</span></span>

<span data-ttu-id="1b736-111">Pro aplikace Java [více rozhraní protokolování](http://en.wikipedia.org/wiki/Java_logging_framework) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="1b736-111">For Java applications, [multiple logging frameworks](http://en.wikipedia.org/wiki/Java_logging_framework) are available.</span></span> <span data-ttu-id="1b736-112">Vzhledem k tomu `java.util.logging` je výchozí možností s prostředí JRE, používá se také pro [příklady v githubu kódu](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1b736-112">Since `java.util.logging` is the default option with the JRE, it is also used for the [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  <span data-ttu-id="1b736-113">Následující diskusi vysvětluje postup konfigurace `java.util.logging` framework.</span><span class="sxs-lookup"><span data-stu-id="1b736-113">The following discussion explains how to configure the `java.util.logging` framework.</span></span>

<span data-ttu-id="1b736-114">Pomocí java.util.logging je paměti, výstupní datové proudy, soubory konzoly nebo sockets přesměrovat protokolů aplikace.</span><span class="sxs-lookup"><span data-stu-id="1b736-114">Using java.util.logging you can redirect your application logs to memory, output streams, console files, or sockets.</span></span> <span data-ttu-id="1b736-115">Pro každou z těchto možností existují výchozí obslužné rutiny už zadané v rozhraní framework.</span><span class="sxs-lookup"><span data-stu-id="1b736-115">For each of these options, there are default handlers already provided in the framework.</span></span> <span data-ttu-id="1b736-116">Můžete vytvořit `app.properties` soubor pro konfiguraci obslužné rutiny souborů pro vaši aplikaci pro přesměrování všechny protokoly do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="1b736-116">You can create a `app.properties` file to configure the file handler for your application to redirect all logs to a local file.</span></span>

<span data-ttu-id="1b736-117">Následující fragment kódu obsahuje příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="1b736-117">The following code snippet contains an example configuration:</span></span>

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

<span data-ttu-id="1b736-118">Složka na kterou odkazuje `app.properties` soubor musí existovat.</span><span class="sxs-lookup"><span data-stu-id="1b736-118">The folder pointed to by the `app.properties` file must exist.</span></span> <span data-ttu-id="1b736-119">Po `app.properties` soubor je vytvořen, musíte také upravit skript vstupního bodu, `entrypoint.sh` v `<applicationfolder>/<servicePkg>/Code/` složky se nastavit vlastnost `java.util.logging.config.file` k `app.propertes` souboru.</span><span class="sxs-lookup"><span data-stu-id="1b736-119">After the `app.properties` file is created, you need to also modify your entry point script, `entrypoint.sh` in the `<applicationfolder>/<servicePkg>/Code/` folder to set the property `java.util.logging.config.file` to `app.propertes` file.</span></span> <span data-ttu-id="1b736-120">Položka by měl vypadat podobně jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="1b736-120">The entry should look like the following snippet:</span></span>

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


<span data-ttu-id="1b736-121">Tato konfigurace, které jsou výsledkem protokoly shromažďovaných rotační způsobem v `/tmp/servicefabric/logs/`.</span><span class="sxs-lookup"><span data-stu-id="1b736-121">This configuration results in logs being collected in a rotating fashion at `/tmp/servicefabric/logs/`.</span></span> <span data-ttu-id="1b736-122">Soubor protokolu v tomto případě je názvem mysfapp%u.%g.log kde:</span><span class="sxs-lookup"><span data-stu-id="1b736-122">The log file in this case is named mysfapp%u.%g.log where:</span></span>
* <span data-ttu-id="1b736-123">**%u** je jedinečné číslo a vyřešte konflikty mezi současně spuštěných procesů Java.</span><span class="sxs-lookup"><span data-stu-id="1b736-123">**%u** is a unique number to resolve conflicts between simultaneous Java processes.</span></span>
* <span data-ttu-id="1b736-124">**%g** je číslo generace k rozlišení mezi otáčení protokoly.</span><span class="sxs-lookup"><span data-stu-id="1b736-124">**%g** is the generation number to distinguish between rotating logs.</span></span>

<span data-ttu-id="1b736-125">Ve výchozím nastavení je pokud žádná obslužná rutina je explicitně nakonfigurován, zaregistrován obslužná rutina konzoly.</span><span class="sxs-lookup"><span data-stu-id="1b736-125">By default if no handler is explicitly configured, the console handler is registered.</span></span> <span data-ttu-id="1b736-126">V procesu syslog pod /var/log/syslog jeden můžete zobrazit protokoly.</span><span class="sxs-lookup"><span data-stu-id="1b736-126">One can view the logs in syslog under /var/log/syslog.</span></span>

<span data-ttu-id="1b736-127">Další informace najdete v tématu [příklady v githubu kódu](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="1b736-127">For more information, see the [code examples in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>  


## <a name="debugging-service-fabric-c-applications"></a><span data-ttu-id="1b736-128">Ladění aplikací Service Fabric C#</span><span class="sxs-lookup"><span data-stu-id="1b736-128">Debugging Service Fabric C# applications</span></span>


<span data-ttu-id="1b736-129">Více rozhraní jsou k dispozici pro trasování CoreCLR aplikace v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="1b736-129">Multiple frameworks are available for tracing CoreCLR applications on Linux.</span></span> <span data-ttu-id="1b736-130">Další informace najdete v tématu [Githubu: protokolování](http:/github.com/aspnet/logging).</span><span class="sxs-lookup"><span data-stu-id="1b736-130">For more information, see [GitHub: logging](http:/github.com/aspnet/logging).</span></span>  <span data-ttu-id="1b736-131">Vzhledem k tomu, že je dobře známé pro C# vývojáře, EventSource se tento článek používá EventSource pro trasování v CoreCLR ukázky v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="1b736-131">Since EventSource is familiar to C# developers,\`this article uses EventSource for tracing in CoreCLR samples on Linux.</span></span>

<span data-ttu-id="1b736-132">Prvním krokem je na System.Diagnostics.Tracing, aby vaše protokoly může zapsat do paměti, výstupní datové proudy nebo soubory konzoly.</span><span class="sxs-lookup"><span data-stu-id="1b736-132">The first step is to include System.Diagnostics.Tracing so that you can write your logs to memory, output streams, or console files.</span></span>  <span data-ttu-id="1b736-133">Pro protokolování použití EventSource, přidejte do souboru project.json následující projektu:</span><span class="sxs-lookup"><span data-stu-id="1b736-133">For logging using EventSource, add the following project to your project.json:</span></span>

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

<span data-ttu-id="1b736-134">Vlastní EventListener můžete použít k naslouchání událostí služby a pak je správně přesměrovat na trasovací soubory.</span><span class="sxs-lookup"><span data-stu-id="1b736-134">You can use a custom EventListener to listen for the service event and then appropriately redirect them to trace files.</span></span> <span data-ttu-id="1b736-135">Následující fragment kódu ukazuje implementaci Ukázka protokolování pomocí EventSource a vlastní EventListener:</span><span class="sxs-lookup"><span data-stu-id="1b736-135">The following code snippet shows a sample implementation of logging using EventSource and a custom EventListener:</span></span>


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

        // TBD: Need to add method for sample event.

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


<span data-ttu-id="1b736-136">Výstupy protokoly do souboru v předchozím fragmentu `/tmp/MyServiceLog.txt`.</span><span class="sxs-lookup"><span data-stu-id="1b736-136">The preceding snippet outputs the logs to a file in `/tmp/MyServiceLog.txt`.</span></span> <span data-ttu-id="1b736-137">Tento název souboru musí být správně aktualizován.</span><span class="sxs-lookup"><span data-stu-id="1b736-137">This file name needs to be appropriately updated.</span></span> <span data-ttu-id="1b736-138">V případě, že chcete přesměrovat protokoly do konzoly, použijte následující fragment kódu v třídě přizpůsobené EventListener:</span><span class="sxs-lookup"><span data-stu-id="1b736-138">In case you want to redirect the logs to console, use the following snippet in your customized EventListener class:</span></span>

```csharp
public static TextWriter Out = Console.Out;
```

<span data-ttu-id="1b736-139">Ukázky v [C# – ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) pomocí EventSource a vlastní EventListener protokolovat události do souboru.</span><span class="sxs-lookup"><span data-stu-id="1b736-139">The samples at [C# Samples](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) use EventSource and a custom EventListener to log events to a file.</span></span>



## <a name="next-steps"></a><span data-ttu-id="1b736-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b736-140">Next steps</span></span>
<span data-ttu-id="1b736-141">Stejný kód trasování přidané do vaší aplikace funguje taky s diagnostikou vaší aplikace v clusteru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="1b736-141">The same tracing code added to your application also works with the diagnostics of your application on an Azure cluster.</span></span> <span data-ttu-id="1b736-142">Podívejte se na tyto články, které popisují různé možnosti pro nástroje a popisují, jak je nastavit.</span><span class="sxs-lookup"><span data-stu-id="1b736-142">Check out these articles that discuss the different options for the tools and describe how to set them up.</span></span>
* [<span data-ttu-id="1b736-143">Postup shromažďování protokolů pomocí Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="1b736-143">How to collect logs with Azure Diagnostics</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
