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
# <a name="enable-communication-for-role-instances-in-azure"></a><span data-ttu-id="c669b-103">Povolit komunikaci pro instance rolí v azure</span><span class="sxs-lookup"><span data-stu-id="c669b-103">Enable communication for role instances in azure</span></span>
<span data-ttu-id="c669b-104">Role cloudové služby komunikují prostřednictvím interní a externí připojení.</span><span class="sxs-lookup"><span data-stu-id="c669b-104">Cloud service roles communicate through internal and external connections.</span></span> <span data-ttu-id="c669b-105">Externí připojení se nazývají **vstupní koncové body** při se označují jako vnitřní připojení **vnitřních koncových bodů**.</span><span class="sxs-lookup"><span data-stu-id="c669b-105">External connections are called **input endpoints** while internal connections are called **internal endpoints**.</span></span> <span data-ttu-id="c669b-106">Toto téma popisuje, jak toomodify hello [služby definice](cloud-services-model-and-package.md#csdef) toocreate koncové body.</span><span class="sxs-lookup"><span data-stu-id="c669b-106">This topic describes how toomodify hello [service definition](cloud-services-model-and-package.md#csdef) toocreate endpoints.</span></span>

## <a name="input-endpoint"></a><span data-ttu-id="c669b-107">Vstupní koncový bod</span><span class="sxs-lookup"><span data-stu-id="c669b-107">Input endpoint</span></span>
<span data-ttu-id="c669b-108">Hello vstupní koncový bod se používá, když chcete tooexpose port toohello mimo.</span><span class="sxs-lookup"><span data-stu-id="c669b-108">hello input endpoint is used when you want tooexpose a port toohello outside.</span></span> <span data-ttu-id="c669b-109">Určete typ hello protokolu a portu hello hello koncového bodu, který pak platí pro obě hello externí i interní porty pro koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-109">You specify hello protocol type and hello port of hello endpoint which then applies for both hello external and internal ports for hello endpoint.</span></span> <span data-ttu-id="c669b-110">Pokud chcete, můžete zadat jiný interní port pro koncový bod hello s hello [Místní_port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) atribut.</span><span class="sxs-lookup"><span data-stu-id="c669b-110">If you want, you can specify a different internal port for hello endpoint with hello [localPort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribute.</span></span>

<span data-ttu-id="c669b-111">vstupní koncový bod Hello můžete použít následující protokoly hello: **http, https, tcp, udp**.</span><span class="sxs-lookup"><span data-stu-id="c669b-111">hello input endpoint can use hello following protocols: **http, https, tcp, udp**.</span></span>

<span data-ttu-id="c669b-112">toocreate vstupní koncový bod, přidejte hello **InputEndpoint** podřízený element toohello **koncové body** element role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="c669b-112">toocreate an input endpoint, add hello **InputEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a><span data-ttu-id="c669b-113">Vstupní koncový bod instance</span><span class="sxs-lookup"><span data-stu-id="c669b-113">Instance input endpoint</span></span>
<span data-ttu-id="c669b-114">Vstupní koncových bodů instance jsou podobné tooinput koncové body, ale můžete namapovat konkrétní veřejné porty pro každou jednotlivou roli instanci pomocí přesměrování portu na Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-114">Instance input endpoints are similar tooinput endpoints but allows you map specific public-facing ports for each individual role instance by using port forwarding on hello load balancer.</span></span> <span data-ttu-id="c669b-115">Můžete zadat jediný port veřejné, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="c669b-115">You can specify a single public-facing port, or a range of ports.</span></span>

<span data-ttu-id="c669b-116">lze použít pouze vstupní koncový bod instance Hello **tcp** nebo **udp** jako protokol pro hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-116">hello instance input endpoint can only use **tcp** or **udp** as hello protocol.</span></span>

<span data-ttu-id="c669b-117">toocreate vstupní koncový bod instance, přidejte hello **InstanceInputEndpoint** podřízený element toohello **koncové body** element role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="c669b-117">toocreate an instance input endpoint, add hello **InstanceInputEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a><span data-ttu-id="c669b-118">Vnitřní koncový bod</span><span class="sxs-lookup"><span data-stu-id="c669b-118">Internal endpoint</span></span>
<span data-ttu-id="c669b-119">Vnitřních koncových bodů jsou k dispozici pro komunikaci instance instance.</span><span class="sxs-lookup"><span data-stu-id="c669b-119">Internal endpoints are available for instance-to-instance communication.</span></span> <span data-ttu-id="c669b-120">Hello port je volitelný a pokud tento parametr vynechán, je dynamický port přiřazen toohello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="c669b-120">hello port is optional and if omitted, a dynamic port is assigned toohello endpoint.</span></span> <span data-ttu-id="c669b-121">Rozsah portů lze použít.</span><span class="sxs-lookup"><span data-stu-id="c669b-121">A port range can be used.</span></span> <span data-ttu-id="c669b-122">Existuje limit pět vnitřních koncových bodů podle příslušné role.</span><span class="sxs-lookup"><span data-stu-id="c669b-122">There is a limit of five internal endpoints per role.</span></span>

<span data-ttu-id="c669b-123">vnitřní koncový bod Hello můžete použít následující protokoly hello: **http, tcp, udp, všechny**.</span><span class="sxs-lookup"><span data-stu-id="c669b-123">hello internal endpoint can use hello following protocols: **http, tcp, udp, any**.</span></span>

<span data-ttu-id="c669b-124">toocreate interní vstupní koncový bod, přidejte hello **InternalEndpoint** podřízený element toohello **koncové body** element role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="c669b-124">toocreate an internal input endpoint, add hello **InternalEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

<span data-ttu-id="c669b-125">Můžete také použít rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="c669b-125">You can also use a port range.</span></span>

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a><span data-ttu-id="c669b-126">Vs role pracovního procesu. Webové role</span><span class="sxs-lookup"><span data-stu-id="c669b-126">Worker roles vs. Web roles</span></span>
<span data-ttu-id="c669b-127">Při práci s worker a webové role je jeden dílčí rozdíl s koncovými body.</span><span class="sxs-lookup"><span data-stu-id="c669b-127">There is one minor difference with endpoints when working with both worker and web roles.</span></span> <span data-ttu-id="c669b-128">Hello webové role musí mít alespoň jeden vstupní koncový bod pomocí hello **HTTP** protokolu.</span><span class="sxs-lookup"><span data-stu-id="c669b-128">hello web role must have at minimum a single input endpoint using hello **HTTP** protocol.</span></span>

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after hello first InputEndPoint -->
</Endpoints>
```

## <a name="using-hello-net-sdk-tooaccess-an-endpoint"></a><span data-ttu-id="c669b-129">Pomocí sady .NET SDK tooaccess hello koncový bod</span><span class="sxs-lookup"><span data-stu-id="c669b-129">Using hello .NET SDK tooaccess an endpoint</span></span>
<span data-ttu-id="c669b-130">Hello spravované knihovny Azure poskytuje metody pro role instance toocommunicate za běhu.</span><span class="sxs-lookup"><span data-stu-id="c669b-130">hello Azure Managed Library provides methods for role instances toocommunicate at runtime.</span></span> <span data-ttu-id="c669b-131">Z kódu spuštěné v instanci role můžete načíst informace o existenci hello další instance rolí a jejich koncových bodů, a také informace o aktuální instance role hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-131">From code running within a role instance, you can retrieve information about hello existence of other role instances and their endpoints, as well as information about hello current role instance.</span></span>

> [!NOTE]
> <span data-ttu-id="c669b-132">Pouze můžete načíst informace o instancích role běží v cloudové služby, které definují aspoň jeden vnitřní koncový bod.</span><span class="sxs-lookup"><span data-stu-id="c669b-132">You can only retrieve information about role instances that are running in your cloud service and that define at least one internal endpoint.</span></span> <span data-ttu-id="c669b-133">Nelze získat data o instancích role spuštěné v jinou službu.</span><span class="sxs-lookup"><span data-stu-id="c669b-133">You cannot obtain data about role instances running in a different service.</span></span>
> 
> 

<span data-ttu-id="c669b-134">Můžete použít hello [instance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) vlastnost tooretrieve instancí role.</span><span class="sxs-lookup"><span data-stu-id="c669b-134">You can use hello [Instances](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) property tooretrieve instances of a role.</span></span> <span data-ttu-id="c669b-135">Prvním použití hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn odkaz na aktuální role toohello instance a potom pomocí hello [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) vlastnost tooreturn roli toohello odkaz sám sebe.</span><span class="sxs-lookup"><span data-stu-id="c669b-135">First use hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn a reference toohello current role instance, and then use hello [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) property tooreturn a reference toohello role itself.</span></span>

<span data-ttu-id="c669b-136">Jakmile se připojíte přes hello .NET SDK tooa role instance prostřednictvím kódu programu, je informace koncového bodu je poměrně snadné tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-136">When you connect tooa role instance programmatically through hello .NET SDK, it's relatively easy tooaccess hello endpoint information.</span></span> <span data-ttu-id="c669b-137">Například po již připojení tooa určité role prostředí, můžete získat hello port konkrétní koncový bod s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="c669b-137">For example, after you've already connected tooa specific role environment, you can get hello port of a specific endpoint with this code:</span></span>

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

<span data-ttu-id="c669b-138">Hello **instance** vlastnost vrátí kolekci **RoleInstance** objekty.</span><span class="sxs-lookup"><span data-stu-id="c669b-138">hello **Instances** property returns a collection of **RoleInstance** objects.</span></span> <span data-ttu-id="c669b-139">Tato kolekce vždy obsahuje hello aktuální instance.</span><span class="sxs-lookup"><span data-stu-id="c669b-139">This collection always contains hello current instance.</span></span> <span data-ttu-id="c669b-140">Pokud hello role nedefinuje vnitřní koncový bod, zahrnuje kolekce hello hello aktuální instance, ale žádné jiné instance.</span><span class="sxs-lookup"><span data-stu-id="c669b-140">If hello role does not define an internal endpoint, hello collection includes hello current instance but no other instances.</span></span> <span data-ttu-id="c669b-141">Hello počet instancí role v kolekci hello bude vždy 1 v případě hello kterých byla definována žádná vnitřní koncový bod pro roli hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-141">hello number of role instances in hello collection will always be 1 in hello case where no internal endpoint is defined for hello role.</span></span> <span data-ttu-id="c669b-142">Pokud hello role definuje vnitřní koncový bod, její instance jsou zjistitelný za běhu a hello počet instancí v kolekci hello bude odpovídat toohello počet instancí zadaný pro roli hello v konfiguračním souboru služby hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-142">If hello role defines an internal endpoint, its instances are discoverable at runtime, and hello number of instances in hello collection will correspond toohello number of instances specified for hello role in hello service configuration file.</span></span>

> [!NOTE]
> <span data-ttu-id="c669b-143">Hello spravované knihovny Azure neposkytuje prostředek k určování stavu hello další instance rolí, ale pokud vaše služba musí tuto funkci budete moct implementovat sami takové vyhodnocování stavu.</span><span class="sxs-lookup"><span data-stu-id="c669b-143">hello Azure Managed Library does not provide a means of determining hello health of other role instances, but you can implement such health assessments yourself if your service needs this functionality.</span></span> <span data-ttu-id="c669b-144">Můžete použít [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain informace o spuštění instancí role.</span><span class="sxs-lookup"><span data-stu-id="c669b-144">You can use [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain information about running role instances.</span></span>
> 
> 

<span data-ttu-id="c669b-145">číslo portu hello toodetermine pro vnitřní koncový bod na instanci role, můžete použít hello [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) tooreturn vlastnost objekt slovník, který obsahuje názvy koncových bodů a jejich odpovídající IP adresy a porty.</span><span class="sxs-lookup"><span data-stu-id="c669b-145">toodetermine hello port number for an internal endpoint on a role instance, you can use hello [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) property tooreturn a Dictionary object that contains endpoint names and their corresponding IP addresses and ports.</span></span> <span data-ttu-id="c669b-146">Hello [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) vlastnost vrací hello IP adresu a port pro zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="c669b-146">hello [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) property returns hello IP address and port for a specified endpoint.</span></span> <span data-ttu-id="c669b-147">Hello **PublicIPEndpoint** vlastnost vrací hello port pro koncový bod Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c669b-147">hello **PublicIPEndpoint** property returns hello port for a load balanced endpoint.</span></span> <span data-ttu-id="c669b-148">část adresy IP Hello hello **PublicIPEndpoint** vlastnost nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="c669b-148">hello IP address portion of hello **PublicIPEndpoint** property is not used.</span></span>

<span data-ttu-id="c669b-149">Tady je příklad, který iteruje instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="c669b-149">Here is an example that iterates role instances.</span></span>

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

<span data-ttu-id="c669b-150">Tady je příklad role pracovního procesu, který získá hello koncový bod vystavený prostřednictvím definice služby hello a začne naslouchat pro připojení.</span><span class="sxs-lookup"><span data-stu-id="c669b-150">Here is an example of a worker role that gets hello endpoint exposed through hello service definition and starts listening for connections.</span></span>

> [!WARNING]
> <span data-ttu-id="c669b-151">Tento kód bude fungovat pouze pro nasazené služby.</span><span class="sxs-lookup"><span data-stu-id="c669b-151">This code will only work for a deployed service.</span></span> <span data-ttu-id="c669b-152">Při spuštění v hello výpočetní emulátor Azure, služby konfigurační prvky, které vytvoření přímé port koncových bodů (**InstanceInputEndpoint** elementy) jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="c669b-152">When running in hello Azure Compute Emulator, service configuration elements that create direct port endpoints (**InstanceInputEndpoint** elements) are ignored.</span></span>
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

## <a name="network-traffic-rules-toocontrol-role-communication"></a><span data-ttu-id="c669b-153">Síťová komunikace pravidla toocontrol role</span><span class="sxs-lookup"><span data-stu-id="c669b-153">Network traffic rules toocontrol role communication</span></span>
<span data-ttu-id="c669b-154">Po definování vnitřních koncových bodů, můžete přidat síťový provoz pravidla (podle hello koncové body, které jste vytvořili) toocontrol jak může instance rolí vzájemně komunikovat.</span><span class="sxs-lookup"><span data-stu-id="c669b-154">After you define internal endpoints, you can add network traffic rules (based on hello endpoints that you created) toocontrol how role instances can communicate with each other.</span></span> <span data-ttu-id="c669b-155">Hello následující diagram ukazuje některé běžné scénáře pro řízení komunikace role:</span><span class="sxs-lookup"><span data-stu-id="c669b-155">hello following diagram shows some common scenarios for controlling role communication:</span></span>

<span data-ttu-id="c669b-156">![Síťový provoz pravidla scénáře](./media/cloud-services-enable-communication-role-instances/scenarios.png "síťový provoz pravidla scénáře")</span><span class="sxs-lookup"><span data-stu-id="c669b-156">![Network Traffic Rules Scenarios](./media/cloud-services-enable-communication-role-instances/scenarios.png "Network Traffic Rules Scenarios")</span></span>

<span data-ttu-id="c669b-157">Hello následující příklad kódu ukazuje definice rolí pro role hello uvedené v předchozí diagram hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-157">hello following code example shows role definitions for hello roles shown in hello previous diagram.</span></span> <span data-ttu-id="c669b-158">Každý definice role obsahuje alespoň jeden interní koncového bodu definovaného:</span><span class="sxs-lookup"><span data-stu-id="c669b-158">Each role definition includes at least one internal endpoint defined:</span></span>

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
> <span data-ttu-id="c669b-159">Omezení komunikace mezi rolemi mohou nastat u vnitřních koncových bodů i fixed a automaticky přiřadí porty.</span><span class="sxs-lookup"><span data-stu-id="c669b-159">Restriction of communication between roles can occur with internal endpoints of both fixed and automatically assigned ports.</span></span>
> 
> 

<span data-ttu-id="c669b-160">Ve výchozím nastavení po definování vnitřní koncový bod může komunikace obtékat ze všech rolí toohello vnitřní koncový bod role bez jakýchkoli omezení.</span><span class="sxs-lookup"><span data-stu-id="c669b-160">By default, after an internal endpoint is defined, communication can flow from any role toohello internal endpoint of a role without any restrictions.</span></span> <span data-ttu-id="c669b-161">toorestrict komunikaci, je nutné přidat **NetworkTrafficRules** element toohello **ServiceDefinition** element v souboru definice služby hello.</span><span class="sxs-lookup"><span data-stu-id="c669b-161">toorestrict communication, you must add a **NetworkTrafficRules** element toohello **ServiceDefinition** element in hello service definition file.</span></span>

### <a name="scenario-1"></a><span data-ttu-id="c669b-162">Scénář 1</span><span class="sxs-lookup"><span data-stu-id="c669b-162">Scenario 1</span></span>
<span data-ttu-id="c669b-163">Povolit pouze provoz sítě z **WebRole1** příliš**WorkerRole1**.</span><span class="sxs-lookup"><span data-stu-id="c669b-163">Only allow network traffic from **WebRole1** too**WorkerRole1**.</span></span>

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

### <a name="scenario-2"></a><span data-ttu-id="c669b-164">Scénář 2</span><span class="sxs-lookup"><span data-stu-id="c669b-164">Scenario 2</span></span>
<span data-ttu-id="c669b-165">Umožňuje použití jenom provoz sítě z **WebRole1** příliš**WorkerRole1** a **WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="c669b-165">Only allows network traffic from **WebRole1** too**WorkerRole1** and **WorkerRole2**.</span></span>

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

### <a name="scenario-3"></a><span data-ttu-id="c669b-166">Scénář 3</span><span class="sxs-lookup"><span data-stu-id="c669b-166">Scenario 3</span></span>
<span data-ttu-id="c669b-167">Umožňuje použití jenom provoz sítě z **WebRole1** příliš**WorkerRole1**, a **WorkerRole1** příliš**WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="c669b-167">Only allows network traffic from **WebRole1** too**WorkerRole1**, and **WorkerRole1** too**WorkerRole2**.</span></span>

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

### <a name="scenario-4"></a><span data-ttu-id="c669b-168">Scénář 4</span><span class="sxs-lookup"><span data-stu-id="c669b-168">Scenario 4</span></span>
<span data-ttu-id="c669b-169">Umožňuje použití jenom provoz sítě z **WebRole1** příliš**WorkerRole1**, **WebRole1** příliš**WorkerRole2**, a  **WorkerRole1** příliš**WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="c669b-169">Only allows network traffic from **WebRole1** too**WorkerRole1**, **WebRole1** too**WorkerRole2**, and **WorkerRole1** too**WorkerRole2**.</span></span>

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

<span data-ttu-id="c669b-170">Odkaz na schéma XML pro hello prvky používané výše naleznete [zde](https://msdn.microsoft.com/library/azure/gg557551.aspx).</span><span class="sxs-lookup"><span data-stu-id="c669b-170">An XML schema reference for hello elements used above can be found [here](https://msdn.microsoft.com/library/azure/gg557551.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c669b-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c669b-171">Next steps</span></span>
<span data-ttu-id="c669b-172">Další informace o hello Cloudová služba [modelu](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="c669b-172">Read more about hello Cloud Service [model](cloud-services-model-and-package.md).</span></span>

