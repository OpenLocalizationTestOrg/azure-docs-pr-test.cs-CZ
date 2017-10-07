---
title: "aaaCommunication rolí v cloudových služeb | Microsoft Docs"
description: "Instance role v cloudové služby může mít koncové body (http, https, tcp, udp) definované pro ně komunikující s hello mimo nebo mezi dalších instancí rolí."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7008a083-acbe-4fb8-ae60-b837ef971ca1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: 1fb39215ceb8a3f0381ef5e108c1149de115ff8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-communication-for-role-instances-in-azure"></a>Povolit komunikaci pro instance rolí v azure
Role cloudové služby komunikují prostřednictvím interní a externí připojení. Externí připojení se nazývají **vstupní koncové body** při se označují jako vnitřní připojení **vnitřních koncových bodů**. Toto téma popisuje, jak toomodify hello [služby definice](cloud-services-model-and-package.md#csdef) toocreate koncové body.

## <a name="input-endpoint"></a>Vstupní koncový bod
Hello vstupní koncový bod se používá, když chcete tooexpose port toohello mimo. Určete typ hello protokolu a portu hello hello koncového bodu, který pak platí pro obě hello externí i interní porty pro koncový bod hello. Pokud chcete, můžete zadat jiný interní port pro koncový bod hello s hello [Místní_port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) atribut.

vstupní koncový bod Hello můžete použít následující protokoly hello: **http, https, tcp, udp**.

toocreate vstupní koncový bod, přidejte hello **InputEndpoint** podřízený element toohello **koncové body** element role web nebo worker.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Vstupní koncový bod instance
Vstupní koncových bodů instance jsou podobné tooinput koncové body, ale můžete namapovat konkrétní veřejné porty pro každou jednotlivou roli instanci pomocí přesměrování portu na Vyrovnávání zatížení hello. Můžete zadat jediný port veřejné, nebo rozsah portů.

lze použít pouze vstupní koncový bod instance Hello **tcp** nebo **udp** jako protokol pro hello.

toocreate vstupní koncový bod instance, přidejte hello **InstanceInputEndpoint** podřízený element toohello **koncové body** element role web nebo worker.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Vnitřní koncový bod
Vnitřních koncových bodů jsou k dispozici pro komunikaci instance instance. Hello port je volitelný a pokud tento parametr vynechán, je dynamický port přiřazen toohello koncový bod. Rozsah portů lze použít. Existuje limit pět vnitřních koncových bodů podle příslušné role.

vnitřní koncový bod Hello můžete použít následující protokoly hello: **http, tcp, udp, všechny**.

toocreate interní vstupní koncový bod, přidejte hello **InternalEndpoint** podřízený element toohello **koncové body** element role web nebo worker.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Můžete také použít rozsah portů.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Vs role pracovního procesu. Webové role
Při práci s worker a webové role je jeden dílčí rozdíl s koncovými body. Hello webové role musí mít alespoň jeden vstupní koncový bod pomocí hello **HTTP** protokolu.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after hello first InputEndPoint -->
</Endpoints>
```

## <a name="using-hello-net-sdk-tooaccess-an-endpoint"></a>Pomocí sady .NET SDK tooaccess hello koncový bod
Hello spravované knihovny Azure poskytuje metody pro role instance toocommunicate za běhu. Z kódu spuštěné v instanci role můžete načíst informace o existenci hello další instance rolí a jejich koncových bodů, a také informace o aktuální instance role hello.

> [!NOTE]
> Pouze můžete načíst informace o instancích role běží v cloudové služby, které definují aspoň jeden vnitřní koncový bod. Nelze získat data o instancích role spuštěné v jinou službu.
> 
> 

Můžete použít hello [instance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) vlastnost tooretrieve instancí role. Prvním použití hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn odkaz na aktuální role toohello instance a potom pomocí hello [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) vlastnost tooreturn roli toohello odkaz sám sebe.

Jakmile se připojíte přes hello .NET SDK tooa role instance prostřednictvím kódu programu, je informace koncového bodu je poměrně snadné tooaccess hello. Například po již připojení tooa určité role prostředí, můžete získat hello port konkrétní koncový bod s tímto kódem:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Hello **instance** vlastnost vrátí kolekci **RoleInstance** objekty. Tato kolekce vždy obsahuje hello aktuální instance. Pokud hello role nedefinuje vnitřní koncový bod, zahrnuje kolekce hello hello aktuální instance, ale žádné jiné instance. Hello počet instancí role v kolekci hello bude vždy 1 v případě hello kterých byla definována žádná vnitřní koncový bod pro roli hello. Pokud hello role definuje vnitřní koncový bod, její instance jsou zjistitelný za běhu a hello počet instancí v kolekci hello bude odpovídat toohello počet instancí zadaný pro roli hello v konfiguračním souboru služby hello.

> [!NOTE]
> Hello spravované knihovny Azure neposkytuje prostředek k určování stavu hello další instance rolí, ale pokud vaše služba musí tuto funkci budete moct implementovat sami takové vyhodnocování stavu. Můžete použít [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain informace o spuštění instancí role.
> 
> 

číslo portu hello toodetermine pro vnitřní koncový bod na instanci role, můžete použít hello [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) tooreturn vlastnost objekt slovník, který obsahuje názvy koncových bodů a jejich odpovídající IP adresy a porty. Hello [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) vlastnost vrací hello IP adresu a port pro zadaný koncový bod. Hello **PublicIPEndpoint** vlastnost vrací hello port pro koncový bod Vyrovnávání zatížení. část adresy IP Hello hello **PublicIPEndpoint** vlastnost nepoužívá.

Tady je příklad, který iteruje instancí rolí.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Tady je příklad role pracovního procesu, který získá hello koncový bod vystavený prostřednictvím definice služby hello a začne naslouchat pro připojení.

> [!WARNING]
> Tento kód bude fungovat pouze pro nasazené služby. Při spuštění v hello výpočetní emulátor Azure, služby konfigurační prvky, které vytvoření přímé port koncových bodů (**InstanceInputEndpoint** elementy) jsou ignorovány.
> 
> 

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;

        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;

        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

        // Bind socket listener toointernal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block hello thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set hello maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see hello MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-toocontrol-role-communication"></a>Síťová komunikace pravidla toocontrol role
Po definování vnitřních koncových bodů, můžete přidat síťový provoz pravidla (podle hello koncové body, které jste vytvořili) toocontrol jak může instance rolí vzájemně komunikovat. Hello následující diagram ukazuje některé běžné scénáře pro řízení komunikace role:

![Síťový provoz pravidla scénáře](./media/cloud-services-enable-communication-role-instances/scenarios.png "síťový provoz pravidla scénáře")

Hello následující příklad kódu ukazuje definice rolí pro role hello uvedené v předchozí diagram hello. Každý definice role obsahuje alespoň jeden interní koncového bodu definovaného:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [!NOTE]
> Omezení komunikace mezi rolemi mohou nastat u vnitřních koncových bodů i fixed a automaticky přiřadí porty.
> 
> 

Ve výchozím nastavení po definování vnitřní koncový bod může komunikace obtékat ze všech rolí toohello vnitřní koncový bod role bez jakýchkoli omezení. toorestrict komunikaci, je nutné přidat **NetworkTrafficRules** element toohello **ServiceDefinition** element v souboru definice služby hello.

### <a name="scenario-1"></a>Scénář 1
Povolit pouze provoz sítě z **WebRole1** příliš**WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Scénář 2
Umožňuje použití jenom provoz sítě z **WebRole1** příliš**WorkerRole1** a **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Scénář 3
Umožňuje použití jenom provoz sítě z **WebRole1** příliš**WorkerRole1**, a **WorkerRole1** příliš**WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Scénář 4
Umožňuje použití jenom provoz sítě z **WebRole1** příliš**WorkerRole1**, **WebRole1** příliš**WorkerRole2**, a  **WorkerRole1** příliš**WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Odkaz na schéma XML pro hello prvky používané výše naleznete [zde](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Další kroky
Další informace o hello Cloudová služba [modelu](cloud-services-model-and-package.md).

