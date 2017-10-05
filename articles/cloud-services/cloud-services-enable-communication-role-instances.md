---
title: "Komunikace pro role v cloudové služby | Microsoft Docs"
description: "Instance role v cloudové služby může mít koncové body (http, https, tcp, udp) definované pro ně komunikující s vnější nebo mezi dalších instancí rolí."
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
ms.openlocfilehash: 8e171d56bb67c971337fa383014988074ec828b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-communication-for-role-instances-in-azure"></a><span data-ttu-id="abc59-103">Povolit komunikaci pro instance rolí v azure</span><span class="sxs-lookup"><span data-stu-id="abc59-103">Enable communication for role instances in azure</span></span>
<span data-ttu-id="abc59-104">Role cloudové služby komunikují prostřednictvím interní a externí připojení.</span><span class="sxs-lookup"><span data-stu-id="abc59-104">Cloud service roles communicate through internal and external connections.</span></span> <span data-ttu-id="abc59-105">Externí připojení se nazývají **vstupní koncové body** při se označují jako vnitřní připojení **vnitřních koncových bodů**.</span><span class="sxs-lookup"><span data-stu-id="abc59-105">External connections are called **input endpoints** while internal connections are called **internal endpoints**.</span></span> <span data-ttu-id="abc59-106">Toto téma popisuje postup úpravy [služby definice](cloud-services-model-and-package.md#csdef) k vytvoření koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="abc59-106">This topic describes how to modify the [service definition](cloud-services-model-and-package.md#csdef) to create endpoints.</span></span>

## <a name="input-endpoint"></a><span data-ttu-id="abc59-107">Vstupní koncový bod</span><span class="sxs-lookup"><span data-stu-id="abc59-107">Input endpoint</span></span>
<span data-ttu-id="abc59-108">Vstupní koncový bod se používá, když chcete vystavit port ven.</span><span class="sxs-lookup"><span data-stu-id="abc59-108">The input endpoint is used when you want to expose a port to the outside.</span></span> <span data-ttu-id="abc59-109">Určete typ protokol a port koncového bodu, který pak platí pro externí i interní porty pro koncový bod.</span><span class="sxs-lookup"><span data-stu-id="abc59-109">You specify the protocol type and the port of the endpoint which then applies for both the external and internal ports for the endpoint.</span></span> <span data-ttu-id="abc59-110">Pokud chcete, můžete zadat jiný interní port koncového bodu se [Místní_port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) atribut.</span><span class="sxs-lookup"><span data-stu-id="abc59-110">If you want, you can specify a different internal port for the endpoint with the [localPort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribute.</span></span>

<span data-ttu-id="abc59-111">Vstupní koncový bod můžete použít následující protokoly: **http, https, tcp, udp**.</span><span class="sxs-lookup"><span data-stu-id="abc59-111">The input endpoint can use the following protocols: **http, https, tcp, udp**.</span></span>

<span data-ttu-id="abc59-112">Chcete-li vytvořit vstupní koncový bod, přidejte **InputEndpoint** podřízený element pro **koncové body** element role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="abc59-112">To create an input endpoint, add the **InputEndpoint** child element to the **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a><span data-ttu-id="abc59-113">Vstupní koncový bod instance</span><span class="sxs-lookup"><span data-stu-id="abc59-113">Instance input endpoint</span></span>
<span data-ttu-id="abc59-114">Instance vstupní koncové body jsou podobná vstupních koncových bodů ale můžete namapovat konkrétní veřejné porty pro každou jednotlivou roli instanci pomocí přesměrování portu v nástroji pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="abc59-114">Instance input endpoints are similar to input endpoints but allows you map specific public-facing ports for each individual role instance by using port forwarding on the load balancer.</span></span> <span data-ttu-id="abc59-115">Můžete zadat jediný port veřejné, nebo rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="abc59-115">You can specify a single public-facing port, or a range of ports.</span></span>

<span data-ttu-id="abc59-116">Vstupní koncový bod instanci lze použít pouze **tcp** nebo **udp** jako protokol.</span><span class="sxs-lookup"><span data-stu-id="abc59-116">The instance input endpoint can only use **tcp** or **udp** as the protocol.</span></span>

<span data-ttu-id="abc59-117">Chcete-li vytvořit instanci vstupní koncový bod, přidejte **InstanceInputEndpoint** podřízený element pro **koncové body** element role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="abc59-117">To create an instance input endpoint, add the **InstanceInputEndpoint** child element to the **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a><span data-ttu-id="abc59-118">Vnitřní koncový bod</span><span class="sxs-lookup"><span data-stu-id="abc59-118">Internal endpoint</span></span>
<span data-ttu-id="abc59-119">Vnitřních koncových bodů jsou k dispozici pro komunikaci instance instance.</span><span class="sxs-lookup"><span data-stu-id="abc59-119">Internal endpoints are available for instance-to-instance communication.</span></span> <span data-ttu-id="abc59-120">Port je volitelný a pokud tento parametr vynechán, je dynamický port přiřazené ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="abc59-120">The port is optional and if omitted, a dynamic port is assigned to the endpoint.</span></span> <span data-ttu-id="abc59-121">Rozsah portů lze použít.</span><span class="sxs-lookup"><span data-stu-id="abc59-121">A port range can be used.</span></span> <span data-ttu-id="abc59-122">Existuje limit pět vnitřních koncových bodů podle příslušné role.</span><span class="sxs-lookup"><span data-stu-id="abc59-122">There is a limit of five internal endpoints per role.</span></span>

<span data-ttu-id="abc59-123">Vnitřní koncový bod můžete použít následující protokoly: **http, tcp, udp, všechny**.</span><span class="sxs-lookup"><span data-stu-id="abc59-123">The internal endpoint can use the following protocols: **http, tcp, udp, any**.</span></span>

<span data-ttu-id="abc59-124">Chcete-li vytvořit interní vstupní koncový bod, přidejte **InternalEndpoint** podřízený element pro **koncové body** element role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="abc59-124">To create an internal input endpoint, add the **InternalEndpoint** child element to the **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

<span data-ttu-id="abc59-125">Můžete také použít rozsah portů.</span><span class="sxs-lookup"><span data-stu-id="abc59-125">You can also use a port range.</span></span>

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a><span data-ttu-id="abc59-126">Vs role pracovního procesu. Webové role</span><span class="sxs-lookup"><span data-stu-id="abc59-126">Worker roles vs. Web roles</span></span>
<span data-ttu-id="abc59-127">Při práci s worker a webové role je jeden dílčí rozdíl s koncovými body.</span><span class="sxs-lookup"><span data-stu-id="abc59-127">There is one minor difference with endpoints when working with both worker and web roles.</span></span> <span data-ttu-id="abc59-128">Webové role musí mít alespoň jeden vstupní koncový bod pomocí **HTTP** protokolu.</span><span class="sxs-lookup"><span data-stu-id="abc59-128">The web role must have at minimum a single input endpoint using the **HTTP** protocol.</span></span>

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a><span data-ttu-id="abc59-129">Pomocí sady .NET SDK pro přístup k koncový bod</span><span class="sxs-lookup"><span data-stu-id="abc59-129">Using the .NET SDK to access an endpoint</span></span>
<span data-ttu-id="abc59-130">Spravované knihovny Azure poskytuje metody pro instance rolí pro komunikaci za běhu.</span><span class="sxs-lookup"><span data-stu-id="abc59-130">The Azure Managed Library provides methods for role instances to communicate at runtime.</span></span> <span data-ttu-id="abc59-131">Z kódu spuštěné v instanci role můžete načíst informace o existenci další instance rolí a jejich koncových bodů, a také informace o aktuální instance role.</span><span class="sxs-lookup"><span data-stu-id="abc59-131">From code running within a role instance, you can retrieve information about the existence of other role instances and their endpoints, as well as information about the current role instance.</span></span>

> [!NOTE]
> <span data-ttu-id="abc59-132">Pouze můžete načíst informace o instancích role běží v cloudové služby, které definují aspoň jeden vnitřní koncový bod.</span><span class="sxs-lookup"><span data-stu-id="abc59-132">You can only retrieve information about role instances that are running in your cloud service and that define at least one internal endpoint.</span></span> <span data-ttu-id="abc59-133">Nelze získat data o instancích role spuštěné v jinou službu.</span><span class="sxs-lookup"><span data-stu-id="abc59-133">You cannot obtain data about role instances running in a different service.</span></span>
> 
> 

<span data-ttu-id="abc59-134">Můžete použít [instance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) vlastnost načtení instancí role.</span><span class="sxs-lookup"><span data-stu-id="abc59-134">You can use the [Instances](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) property to retrieve instances of a role.</span></span> <span data-ttu-id="abc59-135">Prvním použití [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) vrátíte odkaz na aktuální instanci role, a potom pomocí [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) vlastnost vrací odkaz k roli sám sebe.</span><span class="sxs-lookup"><span data-stu-id="abc59-135">First use the [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) to return a reference to the current role instance, and then use the [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) property to return a reference to the role itself.</span></span>

<span data-ttu-id="abc59-136">Při připojení k instanci role programově pomocí .NET SDK je poměrně snadné pro přístup k informacím koncový bod.</span><span class="sxs-lookup"><span data-stu-id="abc59-136">When you connect to a role instance programmatically through the .NET SDK, it's relatively easy to access the endpoint information.</span></span> <span data-ttu-id="abc59-137">Například po již připojení k prostředí konkrétní roli, můžete získat port konkrétní koncový bod s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="abc59-137">For example, after you've already connected to a specific role environment, you can get the port of a specific endpoint with this code:</span></span>

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

<span data-ttu-id="abc59-138">**Instance** vlastnost vrátí kolekci **RoleInstance** objekty.</span><span class="sxs-lookup"><span data-stu-id="abc59-138">The **Instances** property returns a collection of **RoleInstance** objects.</span></span> <span data-ttu-id="abc59-139">Tato kolekce vždy obsahuje aktuální instance.</span><span class="sxs-lookup"><span data-stu-id="abc59-139">This collection always contains the current instance.</span></span> <span data-ttu-id="abc59-140">Pokud roli nedefinuje vnitřní koncový bod, kolekce zahrnuje aktuální instance, ale žádné jiné instance.</span><span class="sxs-lookup"><span data-stu-id="abc59-140">If the role does not define an internal endpoint, the collection includes the current instance but no other instances.</span></span> <span data-ttu-id="abc59-141">Počet instancí role v kolekci bude vždy 1 v případě, kde je definován žádný vnitřní koncový bod pro roli.</span><span class="sxs-lookup"><span data-stu-id="abc59-141">The number of role instances in the collection will always be 1 in the case where no internal endpoint is defined for the role.</span></span> <span data-ttu-id="abc59-142">Pokud role definuje vnitřní koncový bod, její instance jsou zjistitelný za běhu, a počet instancí v kolekci odpovídá počtu instancí zadaný pro roli v konfiguračním souboru služby.</span><span class="sxs-lookup"><span data-stu-id="abc59-142">If the role defines an internal endpoint, its instances are discoverable at runtime, and the number of instances in the collection will correspond to the number of instances specified for the role in the service configuration file.</span></span>

> [!NOTE]
> <span data-ttu-id="abc59-143">Spravované knihovny Azure neposkytuje způsob určování stavu další instance rolí, ale pokud vaše služba musí tuto funkci budete moct implementovat sami takové vyhodnocování stavu.</span><span class="sxs-lookup"><span data-stu-id="abc59-143">The Azure Managed Library does not provide a means of determining the health of other role instances, but you can implement such health assessments yourself if your service needs this functionality.</span></span> <span data-ttu-id="abc59-144">Můžete použít [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) získat informace o spuštění instancí role.</span><span class="sxs-lookup"><span data-stu-id="abc59-144">You can use [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) to obtain information about running role instances.</span></span>
> 
> 

<span data-ttu-id="abc59-145">Chcete-li zjistit číslo portu pro vnitřní koncový bod na instanci role, můžete použít [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) vlastnost vrátit objekt slovník, který obsahuje názvy koncových bodů a jejich odpovídající IP adresy a porty.</span><span class="sxs-lookup"><span data-stu-id="abc59-145">To determine the port number for an internal endpoint on a role instance, you can use the [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) property to return a Dictionary object that contains endpoint names and their corresponding IP addresses and ports.</span></span> <span data-ttu-id="abc59-146">[IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) vlastnost vrací IP adresu a port pro zadaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="abc59-146">The [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) property returns the IP address and port for a specified endpoint.</span></span> <span data-ttu-id="abc59-147">**PublicIPEndpoint** vlastnost vrací port pro koncový bod Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="abc59-147">The **PublicIPEndpoint** property returns the port for a load balanced endpoint.</span></span> <span data-ttu-id="abc59-148">Část adresy IP **PublicIPEndpoint** vlastnost nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="abc59-148">The IP address portion of the **PublicIPEndpoint** property is not used.</span></span>

<span data-ttu-id="abc59-149">Tady je příklad, který iteruje instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="abc59-149">Here is an example that iterates role instances.</span></span>

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

<span data-ttu-id="abc59-150">Tady je příklad role pracovního procesu, který získá koncový bod vystavený prostřednictvím definice služby a začne naslouchat pro připojení.</span><span class="sxs-lookup"><span data-stu-id="abc59-150">Here is an example of a worker role that gets the endpoint exposed through the service definition and starts listening for connections.</span></span>

> [!WARNING]
> <span data-ttu-id="abc59-151">Tento kód bude fungovat pouze pro nasazené služby.</span><span class="sxs-lookup"><span data-stu-id="abc59-151">This code will only work for a deployed service.</span></span> <span data-ttu-id="abc59-152">Při spouštění v emulátoru výpočetní Azure, služby konfigurační prvky, které vytvoření přímé port koncových bodů (**InstanceInputEndpoint** elementy) jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="abc59-152">When running in the Azure Compute Emulator, service configuration elements that create direct port endpoints (**InstanceInputEndpoint** elements) are ignored.</span></span>
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

        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
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
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a><span data-ttu-id="abc59-153">Pravidla pro provoz sítě pro řízení komunikace role</span><span class="sxs-lookup"><span data-stu-id="abc59-153">Network traffic rules to control role communication</span></span>
<span data-ttu-id="abc59-154">Po definování vnitřních koncových bodů, můžete přidat pravidla pro provoz sítě (založené na koncových bodů, které jste vytvořili) řídit, jak můžete instance rolí vzájemně komunikovat.</span><span class="sxs-lookup"><span data-stu-id="abc59-154">After you define internal endpoints, you can add network traffic rules (based on the endpoints that you created) to control how role instances can communicate with each other.</span></span> <span data-ttu-id="abc59-155">Následující diagram znázorňuje některé běžné scénáře pro řízení komunikace role:</span><span class="sxs-lookup"><span data-stu-id="abc59-155">The following diagram shows some common scenarios for controlling role communication:</span></span>

<span data-ttu-id="abc59-156">![Síťový provoz pravidla scénáře](./media/cloud-services-enable-communication-role-instances/scenarios.png "síťový provoz pravidla scénáře")</span><span class="sxs-lookup"><span data-stu-id="abc59-156">![Network Traffic Rules Scenarios](./media/cloud-services-enable-communication-role-instances/scenarios.png "Network Traffic Rules Scenarios")</span></span>

<span data-ttu-id="abc59-157">Následující příklad kódu ukazuje definice rolí u rolí vidět na předchozím obrázku.</span><span class="sxs-lookup"><span data-stu-id="abc59-157">The following code example shows role definitions for the roles shown in the previous diagram.</span></span> <span data-ttu-id="abc59-158">Každý definice role obsahuje alespoň jeden interní koncového bodu definovaného:</span><span class="sxs-lookup"><span data-stu-id="abc59-158">Each role definition includes at least one internal endpoint defined:</span></span>

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
> <span data-ttu-id="abc59-159">Omezení komunikace mezi rolemi mohou nastat u vnitřních koncových bodů i fixed a automaticky přiřadí porty.</span><span class="sxs-lookup"><span data-stu-id="abc59-159">Restriction of communication between roles can occur with internal endpoints of both fixed and automatically assigned ports.</span></span>
> 
> 

<span data-ttu-id="abc59-160">Ve výchozím nastavení po definování vnitřní koncový bod komunikace můžete procházet z žádné role, do vnitřní koncový bod role bez jakýchkoli omezení.</span><span class="sxs-lookup"><span data-stu-id="abc59-160">By default, after an internal endpoint is defined, communication can flow from any role to the internal endpoint of a role without any restrictions.</span></span> <span data-ttu-id="abc59-161">Pokud chcete omezit komunikaci, je nutné přidat **NetworkTrafficRules** elementu, který chcete **ServiceDefinition** element v souboru definice služby.</span><span class="sxs-lookup"><span data-stu-id="abc59-161">To restrict communication, you must add a **NetworkTrafficRules** element to the **ServiceDefinition** element in the service definition file.</span></span>

### <a name="scenario-1"></a><span data-ttu-id="abc59-162">Scénář 1</span><span class="sxs-lookup"><span data-stu-id="abc59-162">Scenario 1</span></span>
<span data-ttu-id="abc59-163">Povolit pouze provoz sítě z **WebRole1** k **WorkerRole1**.</span><span class="sxs-lookup"><span data-stu-id="abc59-163">Only allow network traffic from **WebRole1** to **WorkerRole1**.</span></span>

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

### <a name="scenario-2"></a><span data-ttu-id="abc59-164">Scénář 2</span><span class="sxs-lookup"><span data-stu-id="abc59-164">Scenario 2</span></span>
<span data-ttu-id="abc59-165">Umožňuje použití jenom provoz sítě z **WebRole1** k **WorkerRole1** a **WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="abc59-165">Only allows network traffic from **WebRole1** to **WorkerRole1** and **WorkerRole2**.</span></span>

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

### <a name="scenario-3"></a><span data-ttu-id="abc59-166">Scénář 3</span><span class="sxs-lookup"><span data-stu-id="abc59-166">Scenario 3</span></span>
<span data-ttu-id="abc59-167">Umožňuje použití jenom provoz sítě z **WebRole1** k **WorkerRole1**, a **WorkerRole1** k **WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="abc59-167">Only allows network traffic from **WebRole1** to **WorkerRole1**, and **WorkerRole1** to **WorkerRole2**.</span></span>

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

### <a name="scenario-4"></a><span data-ttu-id="abc59-168">Scénář 4</span><span class="sxs-lookup"><span data-stu-id="abc59-168">Scenario 4</span></span>
<span data-ttu-id="abc59-169">Umožňuje použití jenom provoz sítě z **WebRole1** k **WorkerRole1**, **WebRole1** k **WorkerRole2**, a **WorkerRole1** k **WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="abc59-169">Only allows network traffic from **WebRole1** to **WorkerRole1**, **WebRole1** to **WorkerRole2**, and **WorkerRole1** to **WorkerRole2**.</span></span>

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

<span data-ttu-id="abc59-170">Odkaz na schéma XML pro prvky používané výše naleznete [zde](https://msdn.microsoft.com/library/azure/gg557551.aspx).</span><span class="sxs-lookup"><span data-stu-id="abc59-170">An XML schema reference for the elements used above can be found [here](https://msdn.microsoft.com/library/azure/gg557551.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="abc59-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abc59-171">Next steps</span></span>
<span data-ttu-id="abc59-172">Další informace o cloudové službě [modelu](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="abc59-172">Read more about the Cloud Service [model](cloud-services-model-and-package.md).</span></span>

