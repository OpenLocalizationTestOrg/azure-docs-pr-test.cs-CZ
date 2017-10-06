---
title: "aaaDebug aplikace Azure Service Fabric v prostředí Eclipse | Microsoft Docs"
description: "Vylepšit hello spolehlivost a výkon vašich služeb vývoji a ladění je v prostředí Eclipse v místní vývojový cluster."
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
ms.date: 02/10/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: ab86254a5c312db40fd631746c89aab0bbb9d1a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Ladění aplikace Java Service Fabric pomocí Eclipse
> [!div class="op_single_selector"]
> * [Visual Studio nebo CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse nebo Java](service-fabric-debugging-your-application-java.md)
> 

1. Spuštění místního vývojového clusteru podle následujících kroků hello v [nastavení vývojového prostředí Service Fabric](service-fabric-get-started-linux.md).

2. Aktualizovat entryPoint.sh služby hello chcete toodebug, tak, aby spouštěl procesu java hello s parametry vzdáleného ladění. Tento soubor se nachází v hello následující umístění: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. Port 8001 nastaven pro ladění v tomto příkladu.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Aktualizovat hello Application Manifest nastavením počet instancí hello nebo hello počet replik pro hello službu, která je ladit too1. Toto nastavení se vyhnete konfliktům pro hello port, který se používá pro ladění. Například pro bezstavové služby, nastavte ``InstanceCount="1"`` a pro stavové služby sady hello cíle a minimální sady replik velikosti too1 následujícím způsobem: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

4. Nasazení aplikace hello.

5. V Eclipse IDE hello, vyberte **spustit -> Konfigurace ladění -> vzdálené aplikace Java a zadejte vlastnosti připojení** a nastavte vlastnosti hello následujícím způsobem:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Nastavte zarážky v požadované body a ladění aplikace hello.

Pokud aplikace hello selhává, můžete také tooenable coredumps. Spuštění ``ulimit -c`` v prostředí a pokud vrátí hodnotu 0, pak coredumps nejsou povoleny. tooenable neomezená coredumps provést hello následující příkaz: ``ulimit -c unlimited``. Můžete si taky ověřit stav hello pomocí příkazu hello ``ulimit -a``.  Pokud jste chtěli tooupdate hello coredump generování cestu, spouštění ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``. 

### <a name="next-steps"></a>Další kroky

* [Shromažďování protokolů pomocí Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).
* [Monitorování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
