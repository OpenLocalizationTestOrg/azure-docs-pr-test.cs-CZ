---
title: "Služba Service Fabric DNS aaaAzure | Microsoft Docs"
description: "Použít službu dns Service Fabric pro zjišťování mikroslužeb z uvnitř hello clusteru."
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
ms.openlocfilehash: fa536f0e41f52c4942702d0a1bdcd3ed7d418d6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dns-service-in-azure-service-fabric"></a>Služba DNS v Azure Service Fabric
Hello služba DNS je služba volitelné systému, kterou můžete povolit v vašeho clusteru toodiscover jiných služeb pomocí protokolu DNS hello.

Mnoho služeb, zejména kontejnerizované služeb, může mít název existující adresu URL a je možné tooresolve je pomocí standardní protokol DNS hello (nikoli hello služby DNS protocol) je žádoucí, zejména ve scénářích "navýšení a posunutí". Hello služba DNS umožňuje jste toomap DNS názvy tooa název služby a proto přeložit koncový bod IP adresy. 

Hello služba DNS mapuje názvy tooservice názvy DNS, které se pak vyřeší hello služby DNS tooreturn hello koncový bod služby. v době vytvoření hello je zadaný název DNS Hello služby hello. 

![Koncové body služby][0]

## <a name="enabling-hello-dns-service"></a>Povolení služby DNS hello
Nejdřív je potřeba služba DNS hello tooenable v clusteru. Získáte hello šablony pro hello clusteru, které chcete toodeploy. Můžete buď použít hello [ukázkových šablon](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) nebo vytvořit šablonu Resource Manager. Služba DNS hello můžete povolit pomocí hello následující kroky:

1. Zkontrolujte, že hello `apiversion` je nastaven příliš`2017-07-01-preview` pro hello `Microsoft.ServiceFabric/clusters` prostředků a pokud ne, aktualizovat, je znázorněné v hello následující fragment kódu:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Teď povolit služba DNS hello přidáním následující hello `addonFeatures` po provedení hello `fabricSettings` části, jak je znázorněno v následujícím fragmentu kódu hello: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Jakmile clusteru šablony aktualizovaly s hello předcházející změny, je použít a nechat hello upgrade byl dokončen. Po dokončení hello služba DNS systému spuštění v clusteru, který se nazývá `fabric:/System/DnsService` části systému služby v Service Fabric Exploreru hello. 

Alternativně můžete povolit hello služba DNS prostřednictvím portálu hello v době vytváření clusteru hello. Hello služba DNS lze povolit zaškrtnutím políčka hello `Include DNS service` v hello `Cluster configuration` nabídky, jak ukazuje následující snímek obrazovky hello:

![Povolení služby DNS prostřednictvím portálu hello][2]


## <a name="setting-hello-dns-name-for-your-service"></a>Nastavení hello název DNS služby
Jakmile hello služba DNS běží v clusteru, můžete nastavit název DNS pro vaše služby buď deklarativně pro výchozí služby v hello `ApplicationManifest.xml` nebo pomocí příkazů prostředí Powershell.

### <a name="setting-hello-dns-name-for-a-default-service-in-hello-applicationmanifestxml"></a>Název DNS hello služby výchozí nastavení v hello ApplicationManifest.xml
Otevřete projekt v sadě Visual Studio nebo svém oblíbeném editoru a otevřete hello `ApplicationManifest.xml` souboru. Přejděte toohello výchozí služby části a pro každé služby přidat hello `ServiceDnsName` atribut. Hello následující příklad ukazuje, jak tooset příliš hello název DNS služby hello`service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Po nasazení aplikace hello hello instance služby v Service Fabric explorer hello hello název DNS pro tuto instanci, ukazuje, jak ukazuje následující obrázek hello: 

![Koncové body služby][1]

### <a name="setting-hello-dns-name-for-a-service-using-powershell"></a>Nastavení hello název DNS pro službu pomocí prostředí Powershell
Hello název DNS pro službu, můžete nastavit při vytváření pomocí hello `New-ServiceFabricService` prostředí Powershell. Hello následující příklad vytvoří nový bezstavové služby s názvem DNS hello`service1.application1`

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

## <a name="using-dns-in-your-services"></a>Pomocí DNS v službách
Pokud nasazujete více než jedna služba, můžete najít hello koncové body toocommunicate jiných služeb s pomocí názvu DNS. Hello služba DNS je pouze použít toostateless služby, protože hello protokol DNS nemůže komunikovat s stavové služby. Pro stavové služby můžete použít předdefinované reverzní proxy server hello pro http volání toocall oddíl konkrétní službu.

Hello následující kód ukazuje, jak toocall jiná služba, která je jednoduše regulární http volat kde zadáte hello port a všechny cesty jako součást hello URL.

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

## <a name="next-steps"></a>Další kroky
Další informace o komunikaci služby v rámci clusteru hello s [připojení a komunikovat se službami](service-fabric-connect-and-communicate-with-services.md)

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
