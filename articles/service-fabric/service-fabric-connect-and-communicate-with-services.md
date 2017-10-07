---
title: "aaaConnect a komunikovat se službami v Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak připojit tooresolve a komunikovat se službami v Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: msfussell
ms.assetid: 7d1052ec-2c9f-443d-8b99-b75c97266e6c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/9/2017
ms.author: vturecek
ms.openlocfilehash: b8b374a71d4c5d21f48a560a3a8c81b357fe418d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Připojení a komunikovat se službami v Service Fabric
V Service Fabric běží služba někde v clusteru Service Fabric, obvykle se distribuují napříč více virtuálními počítači. Nebylo možné přesouvat z jednoho místa tooanother pomocí hello vlastník služby, nebo automaticky pomocí Service Fabric. Služby nejsou staticky vázané tooa konkrétní počítač nebo adresu.

Aplikace Service Fabric se obvykle skládá z mnoha různých služeb, kde každá služba provádí úlohu specializované. Tyto služby může komunikovat s navzájem tooform dokončení funkce, jako je například vykreslování různé části webové aplikace. Existují také klientské aplikace, které se připojují tooand komunikovat se službami. Tento dokument popisuje, jak tooset až komunikaci s a mezi vaší služby v Service Fabric.

Toto video Microsoft Virtual Academy taky popisuje komunikace služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965">  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a>Přineste si vlastní protokol
Service Fabric pomáhá spravovat životní cyklus hello vašich služeb, ale neprovede rozhodnutí o co dělat, vaše služby. To zahrnuje komunikaci. Po otevření služby pomocí Service Fabric, která je vaše služba možnost tooset až koncový bod pro příchozí požadavky, ať zásobník protokolu nebo komunikaci pomocí chcete. Vaše služba bude naslouchat na normální **IP: port** adres pomocí žádné schéma adresování, jako je identifikátor URI. Hostitelský proces může sdílet více instancí služby nebo repliky, v takovém případě se bude buď potřebovat toouse jiné porty, nebo použijte mechanismus sdílení portů, jako je například jádra ovladač http.sys hello v systému Windows. V obou případech musí být jedinečně adresovat každé instance služby nebo repliky v hostitelském procesu.

![Koncové body služby][1]

## <a name="service-discovery-and-resolution"></a>Zjišťování služby a řešení
V distribuované systému může služby přesun z tooanother jeden počítač v čase. K tomu dochází z různých důvodů, včetně prostředků vyrovnávání upgrady, převzetí služeb při selhání nebo Škálováním na více systémů. To znamená, že adresy koncových bodů služby změnit, protože služba hello přesune toonodes různé IP adresy a může otevřít v jiné porty, pokud služba hello používá dynamicky vybraný port.

![Distribuce služeb][7]

Service Fabric nabízí zjišťování a řešení služba byla požádána hello služby DNS. Hello služby DNS udržuje tabulku, která mapuje toohello adresy koncových bodů, které naslouchat požadavkům na pojmenovaných instancí služby. Všechny instance s názvem služby v Service Fabric mít jedinečné názvy reprezentován jako identifikátory URI, například `"fabric:/MyApplication/MyService"`. Hello hello služby nedojde ke změně názvu průběhu životnosti hello hello služby, je pouze adresy koncových bodů hello, které můžou změnit při přesunutí služby. Toto je obdobou toowebsites, které mají konstantní adresy URL, ale kde může změnit hello IP adresu. A podobně jako tooDNS na hello webu, který se přeloží adresy tooIP adresy URL webu, Service Fabric má doménového registrátora, která mapuje názvy služby, tootheir adresa koncového bodu.

![Koncové body služby][2]

Řešení a připojení tooservices zahrnuje následující kroky na opakování hello:

* **Vyřešte**: koncový bod hello Get, který služba byla publikována z hello služby DNS.
* **Připojit**: připojení ke službám toohello přes ať protokolu používá na tohoto koncového bodu.
* **Opakujte**: Pokus o připojení mohou být neúspěšné pro z nejrůznějších důvodů, například pokud hello služby přesunul vzhledem k tomu, že adresa koncového bodu hello poslední čas hello byl vyřešen. V takovém případě hello předcházející vyřešte a připojte kroky nutné toobe opakovat a tento cyklus se opakuje, dokud nebude úspěšné připojení hello.

## <a name="connecting-tooother-services"></a>Připojení služby tooother
Služby připojení tooeach jiné v clusteru s podporou obecně můžete přímý přístup k koncové body hello jiných služeb, protože jsou na hello hello uzly v clusteru s podporou stejné místní síti. toomake je snazší tooconnect mezi službami, Service Fabric nabízí další služby, které používají hello služby DNS. Služba DNS a služby reverzní proxy server.


### <a name="dns-service"></a>Služba DNS
Vzhledem k tomu, že mnoho služeb, zejména kontejnerizované služeb, může mít název existující adresu URL, je možné tooresolve tyto pomocí hello standardní protokol DNS (nikoli hello služby DNS protocol) je velmi praktické, zejména v aplikaci "navýšení a posunutí" scénáře. Toto je přesně službou DNS, jaké hello. Ji umožňuje vám toomap DNS názvy tooa název služby a proto přeložit koncový bod IP adresy. 

Jako hello uvedené v následujícím diagramu, hello služba DNS v clusteru Service Fabric hello systému mapuje názvy tooservice názvy DNS, které se pak vyřeší hello služby DNS tooreturn hello koncový bod adresy tooconnect k. v době vytvoření hello je zadaný název DNS Hello služby hello. 

![Koncové body služby][9]

Další informace o jak zjistit, toouse hello služba DNS [služba DNS v Azure Service Fabric](service-fabric-dnsservice.md) článku.

### <a name="reverse-proxy-service"></a>Reverzní proxy server služby
reverzní proxy server Hello adresy služeb v hello clusteru, který zveřejňuje koncové body HTTP včetně HTTPS. reverzní proxy server Hello výrazně zjednodušuje volání jiných služeb a své metody tak, že konkrétní formátu identifikátoru URI a zpracovává hello vyřešit, připojení, opakujte kroky potřebné k toocommunicate jednu službu s použitím jiného hello pojmenování služby. Jinými slovy skryje hello Naming Service od vás při volání metody jiných služeb tím, že to stejně jednoduché jako volání adresu URL.

![Koncové body služby][10]

Další informace o jak toouse hello reverse službu proxy serveru najdete v tématu [reverzní proxy server v Azure Service Fabric](service-fabric-reverseproxy.md) článku.

## <a name="connections-from-external-clients"></a>Připojení klientů, externí
Služby připojení tooeach jiné v clusteru s podporou obecně můžete přímý přístup k koncové body hello jiných služeb, protože jsou na hello hello uzly v clusteru s podporou stejné místní síti. V některých prostředích však clusteru může být za nástroj pro vyrovnávání zatížení, který směruje externí příchozí provoz prostřednictvím omezenou sadu portů. V těchto případech můžete služby stále vzájemně komunikovat a vyřešit hello adresy pomocí služby DNS, ale dodatečné kroky musí být tooservices tooconnect přijatá tooallow externích klientů.

## <a name="service-fabric-in-azure"></a>Service Fabric v Azure
Cluster Service Fabric v Azure je umístěn za pro vyrovnávání zatížení Azure. Všechny externí přenosy toohello cluster musí projít prostřednictvím hello nástroj pro vyrovnávání zatížení. Hello nástroj pro vyrovnávání zatížení bude automaticky přeposílal provoz na daného portu tooa náhodných příchozí *uzlu* má hello otevřete stejný port. Hello nástroj pro vyrovnávání zatížení Azure pouze zná na hello otevřené porty *uzly*, neví o otevřené porty fyzická *služby*.

![Azure topologie pro vyrovnávání zatížení a Service Fabric][3]

Například v pořadí tooaccept externí přenosy na portu **80**, musí být nakonfigurované hello následující věci:

1. Zápis služba, která naslouchá na portu 80. Nakonfigurujte port 80 v ServiceManifest.xml hello služby a otevřete naslouchací proces ve službě service hello, například vlastním hostováním webový server.

    ```xml
    <Resources>
        <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>
    ```
    ```csharp
        class HttpCommunicationListener : ICommunicationListener
        {
            ...

            public Task<string> OpenAsync(CancellationToken cancellationToken)
            {
                EndpointResourceDescription endpoint =
                    serviceContext.CodePackageActivationContext.GetEndpoint("WebEndpoint");

                string uriPrefix = $"{endpoint.Protocol}://+:{endpoint.Port}/myapp/";

                this.httpListener = new HttpListener();
                this.httpListener.Prefixes.Add(uriPrefix);
                this.httpListener.Start();

                string publishUri = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
                return Task.FromResult(publishUri);
            }

            ...
        }

        class WebService : StatelessService
        {
            ...

            protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
            {
                return new[] { new ServiceInstanceListener(context => new HttpCommunicationListener(context))};
            }

            ...
        }
    ```
    ```java
        class HttpCommunicationlistener implements CommunicationListener {
            ...

            @Override
            public CompletableFuture<String> openAsync(CancellationToken arg0) {
                EndpointResourceDescription endpoint =
                    this.serviceContext.getCodePackageActivationContext().getEndpoint("WebEndpoint");
                try {
                    HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress(endpoint.getPort()), 0);
                    server.start();

                    String publishUri = String.format("http://%s:%d/",
                        this.serviceContext.getNodeContext().getIpAddressOrFQDN(), endpoint.getPort());
                    return CompletableFuture.completedFuture(publishUri);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

            ...
        }

        class WebService extends StatelessService {
            ...

            @Override
            protected List<ServiceInstanceListener> createServiceInstanceListeners() {
                <ServiceInstanceListener> listeners = new ArrayList<ServiceInstanceListener>();
                listeners.add(new ServiceInstanceListener((context) -> new HttpCommunicationlistener(context)));
                return listeners;       
            }

            ...
        }
    ```
2. Vytvořte Service Fabric Cluster v Azure a zadejte port **80** jako vlastní koncový port pro typ hello uzlu, který bude hostitelem služby hello. Pokud máte více než jeden typ uzlu, můžete nastavit *omezení umístění* na tooensure hello služba spuštěna pouze v hello typu uzlu, který má otevřený port hello vlastní koncový bod.

    ![Otevřete port typu uzlu][4]
3. Po vytvoření clusteru hello, nakonfigurujte v clusteru hello skupiny prostředků tooforward přenosy na portu 80 hello nástroj pro vyrovnávání zatížení Azure. Při vytváření clusteru prostřednictvím hello portálu Azure, to je nastavený automaticky pro každé vlastní koncový port, který byl nakonfigurovaný.

    ![Přenos dat v hello nástroj pro vyrovnávání zatížení Azure][5]
4. Test toodetermine jestli Hello používá Azure nástroj pro vyrovnávání zatížení nebo není toosend provozu tooa konkrétním uzlu. Hello testů pravidelně kontroluje koncový bod na každý uzel toodetermine, jestli odpovídá hello uzlu. Pokud hello test nezdaří tooreceive odpověď po nakonfigurovanou stanovený počet, nástroj pro vyrovnávání zatížení hello zastaví odesílání přenosů toothat uzlu. Při vytváření clusteru prostřednictvím hello portálu Azure, sondu se automaticky nastaví pro každý koncový bod vlastní port, který byl nakonfigurován.

    ![Přenos dat v hello nástroj pro vyrovnávání zatížení Azure][8]

Je důležité tooremember, který hello nástroj pro vyrovnávání zatížení Azure a kontroly hello pouze vědět o hello *uzly*, není hello *služby* spuštěná na uzlech hello. Hello nástroj pro vyrovnávání zatížení Azure vždycky odešlou toonodes provoz, který reagovat toohello testu, takže se musí věnovat tooensure je k dispozici v hello uzlů, které jsou možné toorespond toohello testu.

## <a name="reliable-services-built-in-communication-api-options"></a>Spolehlivé služby: Možnosti integrované komunikace rozhraní API
Spolehlivé služby framework Hello se dodává s několik předdefinovaných komunikace možností. Hello rozhodnutí o tom, které jeden bude nejlépe fungovat pro vás závisí na volbu hello hello programování modelu, hello komunikace framework a hello programovací jazyk, který vašim službám, které jsou napsané v.

* **Žádný konkrétní protokol:** Pokud nemáte konkrétní volbu komunikace framework, ale chcete tooget něco nahoru a spuštěná rychle, je hello ideální možnost pro vás [vzdálené komunikace služby](service-fabric-reliable-services-communication-remoting.md), což umožňuje silně typované vzdálené volání procedur pro Reliable Services a Reliable Actors. Toto je nejjednodušší hello a tooget nejrychlejší způsob, jak začít s komunikace služby. Vzdálená komunikace služby provádí překlad adres služby, připojení, zkuste to znovu a zpracování chyb. Toto je k dispozici pro C# a aplikací v jazyce Java.
* **HTTP**: pro jazykově nezávislého komunikaci protokolu HTTP poskytuje na standardní volbu s nástroji a servery HTTP, které jsou k dispozici v mnoha různých jazycích vše s podporou Service Fabric. Služby mohou používat jakékoli zásobník protokolu HTTP k dispozici, včetně [rozhraní ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) pro aplikací v C#. Klienti, které jsou napsané v C# můžete využít hello `ICommunicationClient` a `ServicePartitionClient` třídy, zatímco pro jazyk Java, použijte hello `CommunicationClient` a `FabricServicePartitionClient` třídy, [pro řešení služby, připojení prostřednictvím protokolu HTTP a opakování smyčky](service-fabric-reliable-services-communication.md).
* **WCF**: Pokud máte existující kód, který používá WCF jako rozhraní vaší komunikace, pak můžete použít hello `WcfCommunicationListener` na straně serveru hello a `WcfCommunicationClient` a `ServicePartitionClient` třídy pro klienta hello. To ale je k dispozici pouze pro aplikace C# na clusterech systémem Windows. Další podrobnosti najdete v tématu Tento článek [provádění WCF hello komunikačního balíku](service-fabric-reliable-services-communication-wcf.md).

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Vlastní protokoly a další komunikaci rozhraní
Služby můžete použít libovolný protokol nebo framework pro komunikaci, jestli je vlastní binární protokol přes TCP sokety nebo streamování události prostřednictvím [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) nebo [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/). Service Fabric nabízí komunikace rozhraní API, kterou můžete připojit vaše komunikačního balíku do, zatímco všechny hello práce toodiscover a připojit je abstrahované od vás. Najdete v článku o hello [spolehlivá služba komunikační model](service-fabric-reliable-services-communication.md) další podrobnosti.

## <a name="next-steps"></a>Další kroky
Další informace o konceptech hello a rozhraní API dostupná v hello [spolehlivé služby komunikační model](service-fabric-reliable-services-communication.md), pak rychle začít s [vzdálené komunikace služby](service-fabric-reliable-services-communication-remoting.md) nebo přejděte podrobný toolearn jak toowrite naslouchací proces komunikace pomocí [webového rozhraní API s OWIN hostování na vlastním](service-fabric-reliable-services-communication-webapi.md).

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
