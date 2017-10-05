---
title: "Služba Azure Service Fabric DNS | Microsoft Docs"
description: "Použijte službu dns Service Fabric pro zjišťování mikroslužeb z v clusteru."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 9871bc5aa4e74ab0faef401d67c4e9558eb5e14b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="dns-service-in-azure-service-fabric"></a><span data-ttu-id="886c9-103">Služba DNS v Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="886c9-103">DNS Service in Azure Service Fabric</span></span>
<span data-ttu-id="886c9-104">Služba DNS je volitelné systému služba, kterou můžete povolit v clusteru pro zjišťování dalších služeb pomocí protokolu DNS.</span><span class="sxs-lookup"><span data-stu-id="886c9-104">The DNS Service is an optional system service that you can enable in your cluster to discover other services using the DNS protocol.</span></span>

<span data-ttu-id="886c9-105">Mnoho služeb, zejména kontejnerizované služeb, může mít název existující adresu URL a schopnost vyřešit pomocí standardní protokol DNS (a nikoli protokol služby DNS) je žádoucí, zejména ve scénářích "navýšení a posunutí".</span><span class="sxs-lookup"><span data-stu-id="886c9-105">Many services, especially containerized services, can have an existing URL name, and being able to resolve them using the standard DNS protocol (rather than the Naming Service protocol) is desirable, particularly in "lift and shift" scenarios.</span></span> <span data-ttu-id="886c9-106">Služba DNS umožňuje mapování názvů DNS pro název služby a proto přeložit koncový bod IP adresy.</span><span class="sxs-lookup"><span data-stu-id="886c9-106">The DNS service enables you to map DNS names to a service name and hence resolve endpoint IP addresses.</span></span> 

<span data-ttu-id="886c9-107">Služba DNS mapuje názvy DNS pro názvy služby, které se pak vyřeší služby DNS k vrácení koncového bodu služby.</span><span class="sxs-lookup"><span data-stu-id="886c9-107">The DNS service maps DNS names to service names, which in turn are resolved by the Naming Service to return the service endpoint.</span></span> <span data-ttu-id="886c9-108">V době vytvoření je zadaný název DNS pro službu.</span><span class="sxs-lookup"><span data-stu-id="886c9-108">The DNS name for the service is provided at the time of creation.</span></span> 

![Koncové body služby][0]

## <a name="enabling-the-dns-service"></a><span data-ttu-id="886c9-110">Povolení služby DNS</span><span class="sxs-lookup"><span data-stu-id="886c9-110">Enabling the DNS service</span></span>
<span data-ttu-id="886c9-111">Je třeba nejprve povolit službu DNS v clusteru.</span><span class="sxs-lookup"><span data-stu-id="886c9-111">First you need to enable the DNS service in your cluster.</span></span> <span data-ttu-id="886c9-112">Získáte šablonu pro cluster, který chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="886c9-112">Get the template for the cluster that you want to deploy.</span></span> <span data-ttu-id="886c9-113">Můžete použít [ukázkových šablon](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) nebo vytvořit šablonu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="886c9-113">You can either use the [sample templates](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype)  or create a Resource Manager template.</span></span> <span data-ttu-id="886c9-114">Můžete povolit službu DNS pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="886c9-114">You can enable the DNS service with the following steps:</span></span>

1. <span data-ttu-id="886c9-115">Zkontrolujte, zda `apiversion` je nastaven na `2017-07-01-preview` pro `Microsoft.ServiceFabric/clusters` prostředků a pokud ne, aktualizovat, je znázorněné v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="886c9-115">Check that the `apiversion` is set to `2017-07-01-preview` for the `Microsoft.ServiceFabric/clusters` resource, and if not, update it as shown in the following snippet:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="886c9-116">Teď povolit službu DNS přidáním následující `addonFeatures` části po `fabricSettings` části, jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="886c9-116">Now enable the DNS service by adding the following `addonFeatures` section after the `fabricSettings` section as shown in the following snippet:</span></span> 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. <span data-ttu-id="886c9-117">Jakmile clusteru šablony aktualizovaly s předchozí změny, je použít a umožní upgradu dokončení.</span><span class="sxs-lookup"><span data-stu-id="886c9-117">Once you have updated your cluster template with the preceding changes, apply them and let the upgrade complete.</span></span> <span data-ttu-id="886c9-118">Po dokončení spuštění služby DNS systému běžící v clusteru, který se nazývá `fabric:/System/DnsService` části systému služby v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="886c9-118">Once complete, the DNS system service starts running in your cluster that is called `fabric:/System/DnsService` under system service section in the Service Fabric explorer.</span></span> 

<span data-ttu-id="886c9-119">Alternativně můžete povolit službu DNS prostřednictvím portálu v době vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="886c9-119">Alternatively, you can enable the DNS service through the portal at the time of cluster creation.</span></span> <span data-ttu-id="886c9-120">Služba DNS lze povolit zaškrtnutím zaškrtávacího políčka pro `Include DNS service` v `Cluster configuration` nabídky, jak je znázorněno na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="886c9-120">The DNS service can be enabled by checking the box for `Include DNS service` in the `Cluster configuration` menu as shown in the following screenshot:</span></span>

![Povolení služby DNS prostřednictvím portálu][2]


## <a name="setting-the-dns-name-for-your-service"></a><span data-ttu-id="886c9-122">Nastavení názvu DNS pro vaši službu</span><span class="sxs-lookup"><span data-stu-id="886c9-122">Setting the DNS name for your service</span></span>
<span data-ttu-id="886c9-123">Jakmile je spuštěna služba DNS v clusteru můžete nastavit název DNS pro vaše služby buď deklarativně pro výchozí služby v `ApplicationManifest.xml` nebo pomocí příkazů prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="886c9-123">Once the DNS service is running in your cluster, you can set a DNS name for your services either declaratively for default services in the `ApplicationManifest.xml` or through Powershell commands.</span></span>

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a><span data-ttu-id="886c9-124">Nastavení názvu DNS pro výchozí služba v ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="886c9-124">Setting the DNS name for a default service in the ApplicationManifest.xml</span></span>
<span data-ttu-id="886c9-125">Otevřete projekt v sadě Visual Studio nebo svém oblíbeném editoru a otevřete `ApplicationManifest.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="886c9-125">Open your project in Visual Studio, or your favorite editor, and open the `ApplicationManifest.xml` file.</span></span> <span data-ttu-id="886c9-126">Přejděte do části výchozí služby a pro každou službu přidat `ServiceDnsName` atribut.</span><span class="sxs-lookup"><span data-stu-id="886c9-126">Go to the default services section, and for each service add the `ServiceDnsName` attribute.</span></span> <span data-ttu-id="886c9-127">Následující příklad ukazuje, jak nastavit název DNS služby`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="886c9-127">The following example shows how to set the DNS name of the service to `service1.application1`</span></span>

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
<span data-ttu-id="886c9-128">Jakmile je aplikace nasazená, instance služby v Service Fabric explorer zobrazuje název DNS pro tuto instanci, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="886c9-128">Once the application is deployed, the service instance in the Service Fabric explorer shows the DNS name for this instance, as shown in the following figure:</span></span> 

![Koncové body služby][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a><span data-ttu-id="886c9-130">Nastavení názvu DNS pro službu pomocí prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="886c9-130">Setting the DNS name for a service using Powershell</span></span>
<span data-ttu-id="886c9-131">Název DNS pro službu, můžete nastavit při vytváření pomocí `New-ServiceFabricService` prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="886c9-131">You can set the DNS name for a service when creating it using the `New-ServiceFabricService` Powershell.</span></span> <span data-ttu-id="886c9-132">Následující příklad vytvoří nový bezstavové služby s názvem DNS`service1.application1`</span><span class="sxs-lookup"><span data-stu-id="886c9-132">The following example creates a new stateless service with the DNS name `service1.application1`</span></span>

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a><span data-ttu-id="886c9-133">Pomocí DNS v službách</span><span class="sxs-lookup"><span data-stu-id="886c9-133">Using DNS in your services</span></span>
<span data-ttu-id="886c9-134">Pokud nasazujete více než jedna služba, můžete najít koncové body jiných služeb, ke komunikaci s pomocí názvu DNS.</span><span class="sxs-lookup"><span data-stu-id="886c9-134">If you deploy more than one service, you can find the endpoints of other services to communicate with  by using a DNS name.</span></span> <span data-ttu-id="886c9-135">Služba DNS používá se pouze pro bezstavové služby, protože protokol DNS nemůže komunikovat s stavové služby.</span><span class="sxs-lookup"><span data-stu-id="886c9-135">The DNS service is only applicable to stateless services, since the DNS protocol cannot communicate with stateful services.</span></span> <span data-ttu-id="886c9-136">Pro stavové služby můžete použít předdefinované reverzní proxy server pro volání protokolu http k volání oddíl konkrétní službu.</span><span class="sxs-lookup"><span data-stu-id="886c9-136">For stateful services, you can use the built-in reverse proxy for http calls to call a particular service partition.</span></span>

<span data-ttu-id="886c9-137">Následující kód ukazuje, jak volat jiné služby, která je jednoduše volání regulární http, kde zadáte port a všechny cesty jako část adresy URL.</span><span class="sxs-lookup"><span data-stu-id="886c9-137">The following code shows how to call another service, which is simply a regular http call where you provide the port and any optional path as part of the URL.</span></span>

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="886c9-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="886c9-138">Next steps</span></span>
<span data-ttu-id="886c9-139">Další informace o komunikaci služby v rámci clusteru s [připojení a komunikovat se službami](service-fabric-connect-and-communicate-with-services.md)</span><span class="sxs-lookup"><span data-stu-id="886c9-139">Learn more about service communication within the cluster with  [connect and communicate with services](service-fabric-connect-and-communicate-with-services.md)</span></span>

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
