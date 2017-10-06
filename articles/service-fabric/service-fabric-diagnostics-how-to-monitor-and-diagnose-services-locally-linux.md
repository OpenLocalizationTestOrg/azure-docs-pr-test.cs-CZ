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
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Monitorování a Diagnostika služby v instalačním programu pro vývoj místním počítači


> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

Monitorování, zjišťování, Diagnostika a řešení potíží s umožňují toocontinue služby s minimálním dopadem toohello uživatelské prostředí. Monitorovací a diagnostické jsou kritické v skutečné nasazené produkční prostředí. Přijetí model podobně jako při vývoji služeb zajistí, že tento kanál diagnostiky hello funguje, když přesunete tooa produkčního prostředí. Service Fabric usnadňuje diagnostiku tooimplement vývojáři služby, který může bezproblémově pracovat v jednom počítači místní vývoj nastavení a nastavení clusteru skutečné produkční.


## <a name="debugging-service-fabric-java-applications"></a>Ladění aplikací Service Fabric Java

Pro aplikace Java [více rozhraní protokolování](http://en.wikipedia.org/wiki/Java_logging_framework) jsou k dispozici. Vzhledem k tomu `java.util.logging` je výchozí možnost hello s hello prostředí JRE, používá se také pro hello [příklady v githubu kódu](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Vysvětluje, Hello následující diskusi jak tooconfigure hello `java.util.logging` framework.

Pomocí java.util.logging můžete přesměrovat aplikace zaznamená toomemory výstupní datové proudy, soubory konzoly nebo sokety. Pro každou z těchto možností existují výchozí obslužné rutiny už zadané v rámci hello. Můžete vytvořit `app.properties` souboru tooconfigure hello obslužná rutina soubor pro vaše aplikace tooredirect všechny protokoly tooa místního souboru.

Hello následující fragment kódu obsahuje příklad konfigurace:

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Hello složky odkazováno tooby hello `app.properties` soubor musí existovat. Po hello `app.properties` soubor je vytvořen, musíte tooalso upravit skript vstupního bodu, `entrypoint.sh` v hello `<applicationfolder>/<servicePkg>/Code/` složky tooset hello vlastnost `java.util.logging.config.file` příliš`app.propertes` souboru. Položka Hello by měl vypadat podobně jako hello následující fragment kódu:

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path tooapp.properties> -jar <service name>.jar
```


Tato konfigurace, které jsou výsledkem protokoly shromažďovaných rotační způsobem v `/tmp/servicefabric/logs/`. soubor protokolu Hello v tomto případě je názvem mysfapp%u.%g.log kde:
* **%u** je jedinečné číslo tooresolve konflikty mezi současně spuštěných procesů Java.
* **%g** je hello generování číslo toodistinguish mezi otáčení protokoly.

Ve výchozím nastavení Pokud žádná obslužná rutina je explicitně nakonfigurován, hello konzoly popisovač je zaregistrován. Protokoly hello jeden můžete zobrazit v procesu syslog pod /var/log/syslog.

Další informace najdete v tématu hello [příklady v githubu kódu](http://github.com/Azure-Samples/service-fabric-java-getting-started).  


## <a name="debugging-service-fabric-c-applications"></a>Ladění aplikací Service Fabric C#


Více rozhraní jsou k dispozici pro trasování CoreCLR aplikace v systému Linux. Další informace najdete v tématu [Githubu: protokolování](http:/github.com/aspnet/logging).  Vzhledem k tomu, že EventSource je známé tooC # vývojáře, se tento článek používá EventSource pro trasování v CoreCLR ukázky v systému Linux.

prvním krokem Hello je tooinclude System.Diagnostics.Tracing tak, aby bylo možné napsat vaše protokoly toomemory, výstupní datové proudy nebo soubory konzoly.  Pro protokolování použití EventSource, přidejte následující project.json tooyour projektu hello:

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

Můžete použít vlastní toolisten EventListener pro událost hello služby a potom správně přesměruje je tootrace soubory. Hello následující fragment kódu ukazuje příklad implementace protokolování událostí EventSource a vlastní EventListener:


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


Hello předchozím fragmentu výstupy hello protokoly tooa souboru v `/tmp/MyServiceLog.txt`. Tento název souboru musí toobe správně aktualizován. V případě, že chcete tooredirect hello protokoly tooconsole, použijte následující fragment kódu v třídě přizpůsobené EventListener hello:

```csharp
public static TextWriter Out = Console.Out;
```

Hello ukázky v [C# – ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) použít EventSource a vlastního souboru tooa EventListener toolog události.



## <a name="next-steps"></a>Další kroky
Hello stejný kód trasování přidat tooyour aplikace funguje taky s diagnostikou hello vaší aplikace v clusteru služby Azure. Podívejte se na tyto články, které popisují různé možnosti hello hello nástroje a popisují, jak tooset je nahoru.
* [Jak toocollect protokoly s Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md)
