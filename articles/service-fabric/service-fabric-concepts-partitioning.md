---
title: "služby Service Fabric aaaPartitioning | Microsoft Docs"
description: "Popisuje, jak toopartition Service Fabric stavové služby. Oddíly umožňuje ukládání dat na místních počítačích hello tak data a výpočetní je možné rozšířit společně."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 6ead48716c08f4212535202ee69d169067d5c6d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="partition-service-fabric-reliable-services"></a>Spolehlivé služby oddílu Service Fabric
Tento článek obsahuje úvod toohello základní koncepty oddíly spolehlivé služby Azure Service Fabric. Hello používá se v článku hello zdrojový kód je k dispozici také na [Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="partitioning"></a>Dělení
Dělení na oddíly není jedinečný tooService prostředků infrastruktury. Ve skutečnosti je základní vzor vytváření škálovatelných služeb. V širším významu jsme vezměte v úvahu dělení jako koncept dělení stavu (data) a výpočetní do menší přístupné jednotky tooimprove škálovatelnost a výkon. Dobře známé formulář oddíly je [vytvoření oddílů dat][wikipartition], označované také jako horizontálního dělení.

### <a name="partition-service-fabric-stateless-services"></a>Bezstavové služby oddílu Service Fabric
Pro bezstavové služby si myslíte o oddílu se logická jednotka, která obsahuje jeden nebo více instancí služby. Obrázek 1 zobrazuje bezstavové služby s pět instancí, které jsou rozmístěny v clusteru pomocí jeden oddíl.

![Bezstavové služby](./media/service-fabric-concepts-partitioning/statelessinstances.png)

Skutečně existují dva typy řešení bezstavové služby. Hello nejdřív jednu je služba, která přetrvává stav externě, například v Azure SQL database (jako je web, který ukládá hello relace informace a data). Hello druhá je jen výpočetní služby (např. kalkulačky nebo bitové kopie vytváření miniatur), které nespravujete žádné trvalý stav.

Buď v případech dělení bezstavové služby je velmi výjimečných scénář--škálovatelnost a dostupnosti jsou obvykle dosáhnete přidáním více instancí. Hello pouze, když chcete tooconsider více oddílů pro bezstavové služby instance je, když potřebujete toomeet speciální směrování požadavků.

Jako příklad Představte si případ, kdy uživatelé s identifikátory v určitý rozsah má být zpracován pouze instance konkrétní službu. Další příklad, kdy může oddílu bezstavové služby je, když máte skutečně oddílů back-end (např. horizontálně dělené databáze SQL) a chcete, aby toocontrol měli zapsat horizontálního oddílu databáze toohello – nebo provádět jinou práci přípravy v rámci které instanci služby Hello bezstavové služby, která vyžaduje hello stejné rozdělení do oddílů informace, jak se používá v back-end hello. Tyto typy scénáře lze vyřešit různými způsoby a nevyžadují nutně rozdělení do oddílů služby.

Hello zbývající část tohoto návodu se zaměřuje na stavové služby.

### <a name="partition-service-fabric-stateful-services"></a>Oddíl Service Fabric stavové služby
Service Fabric umožňuje snadno toodevelop škálovatelné stavové služby prostřednictvím nabídky prvotřídní způsob toopartition stavu (data). Koncepčně, si můžete představit o oddílu stavové služby jako jednotku škálování, která je vysoce spolehlivé prostřednictvím [repliky](service-fabric-availability-services.md) které jsou distribuované a rovnoměrně rozdělen mezi hello uzlů v clusteru.

Vytváření oddílů v kontextu hello stavové služby Service Fabric odkazuje toohello proces pro určení toho, že oddíl konkrétní službu je zodpovědná za část hello dokončení stavu služby hello. (Jak je uvedeno nahoře, oddíl je sada [repliky](service-fabric-availability-services.md)). Skvělé věc o Service Fabric se umístí hello oddíly v různých uzlech. To jim umožňuje limitu prostředků toogrow tooa uzlu. Jako hello data potřebuje růst, oddíly růst a Service Fabric oddíly znovu vytvoří rovnováhu mezi uzly. Tím se zajistí, že hello dál efektivní využití hardwarových prostředků.

toogive příklad sdělení je začněte s 5 uzly clusteru a službu, která je nakonfigurovaná toohave 10 oddílů a cílem tři repliky. V takovém případě by vyvážit a distribuovat hello repliky do hello clusteru Service Fabric a skončili byste s dva primární [repliky](service-fabric-availability-services.md) na jeden uzel.
Pokud potřebujete teď tooscale out hello too10 uzly, Service Fabric by znovu vyvážit hello primární [repliky](service-fabric-availability-services.md) pro všechny uzly 10. Podobně při změně měřítka back too5 uzly Service Fabric by znovu vyvážit všechny repliky hello mezi uzly hello 5.  

Obrázek 2 ukazuje hello distribuce 10 oddílů před a po škálování clusteru hello.

![Stavové služby](./media/service-fabric-concepts-partitioning/partitions.png)

Hello Škálováním na více systémů se v důsledku toho dosáhne vzhledem k tomu, že požadavky od klientů, které jsou rozmístěny v počítačích, celkový výkon aplikace hello je vyšší, a tím se snižuje kolize na přístup toochunks data.

## <a name="plan-for-partitioning"></a>Plán pro dělení
Před implementací služby, byste měli zvážit vždy hello dělení strategie, která je požadovaná tooscale out. Existují různé způsoby, ale všechny z nich soustředit na jaké aplikace hello musí tooachieve. Pro hello kontext tohoto článku zvažte některé hello důležité aspekty.

Dobrým přístupem je toothink o struktuře hello hello stavu, který potřebuje toobe rozdělena na oddíly, jako první krok hello.

Podívejme jednoduchý příklad. Pokud byste byli toobuild služby pro countywide dotazování, můžete vytvořit oddíl pro každé město v okres hello. Potom může ukládat hello hlasy pro každou osobu v hello městě hello oddílu, který odpovídá toothat města. Obrázek 3 znázorňuje sadu osoby a hello města, ve kterém se nacházejí.

![Jednoduché oddílu](./media/service-fabric-concepts-partitioning/cities.png)

Jako hello naplnění města se výrazně liší, můžete skončit s některé oddíly, které obsahují velké množství dat (např. Praha) a ostatní oddíly s velmi malé stavu (např. Kirkland). A co je hello dopad s oddíly s nerovnoměrné objemy stavu?

Pokud přemýšlíte o příklad hello znovu, snadno uvidíte, že hello oddílu, který obsahuje, zda text hello hlasy pro Seattle získáte více požadavků než hello Kirkland jeden. Ve výchozím nastavení, Service Fabric zajišťuje, že je o hello stejný počet primární a sekundární repliky na každém uzlu. Proto můžete skončit s uzly, které mají replik, které poskytovat další provoz a ostatní, které slouží menší provoz. By pokud možno chcete tooavoid aktivní a cold body, jako to v clusteru.

V pořadí tooavoid, to, měli byste udělat dvě věci, z hlediska rozdělení oddílů:

* Zkuste toopartition hello stavu tak, aby je rovnoměrně rozdělené mezi všechny oddíly.
* Načtení sestav z každou repliku hello služby hello. (Informace o tom, projděte si tento článek na [metriky a zatížení](service-fabric-cluster-resource-manager-metrics.md)). Service Fabric nabízí hello schopností tooreport zatížení spotřebovávají služby, jako je množství paměti nebo počet záznamů. Na základě hello metriky hlášené, Service Fabric zjistí, že některé oddíly slouží větší objemy než jiné a znovu vytvoří rovnováhu hello clusteru přesunutí repliky toomore vhodný uzly, tak, aby celkový je přetížena žádný uzel.

V některých případech nelze vědět, kolik dat bude v daném oddílu. Obecné doporučení je toodo obou – nejprve přijetím strategie dělení který se šíří hello data rovnoměrně mezi oddílů hello a druhý, pomocí sestav zatížení.  První metoda Hello zabraňuje případů popsaných v hello hlasování příklad, zatímco hello druhý pomáhá vyhlazení dočasné rozdíly v přístupu nebo zatížení v čase.

Další aspekt plánování oddílu je toochoose hello správný počet oddílů toobegin s.
Z hlediska Service Fabric není nic, které zabraňují začínáte s vyšší počet oddílů, než se očekává pro váš scénář.
Za předpokladu, že hello maximální počet oddílů ve skutečnosti je platný přístup.

Ve výjimečných případech můžete dospět nutnosti více oddílů, než jste původně vybrali. Jako počet oddílů hello nelze změnit po hello fakt, potřebovali byste tooapply přístupy některé pokročilé oddílu, jako je vytvoření nové instance služby hello stejný typ služby. Také potřebujete tooimplement některé klientské logiky, která směruje hello požadavky instance správné služby toohello, založené na straně klienta znalostních bází, které musí zachovat váš klientský kód.

Je potřeba vzít v úvahu pro dělení plánování hello dostupný počítač prostředky. Stav hello stačit toobe přístup a uložené, jste vázané toofollow:

* Omezení šířky pásma sítě
* Omezení paměti systému
* Limity úložiště disku

Co se tak stane, pokud k omezení prostředků v clusteru s podporou spuštěné? odpověď Hello je, že budete jednoduše škálovat hello clusteru tooaccommodate hello nové požadavky.

[Příručka pro plánování kapacity Hello](service-fabric-capacity-planning.md) nabízí pokyny, jak toodetermine kolik uzly, které cluster potřebuje.

## <a name="get-started-with-partitioning"></a>Začínáme s dělení
Tato část popisuje, jak se tooget pracovat s vytváření oddílů služby.

Service Fabric nabízí výběr ze tří schématy oddílu:

* Pohyboval, vytváření oddílů (označováno také jako UniformInt64Partition).
* S názvem rozdělení do oddílů. Aplikace, které používají tento model obvykle mají data, která může být bucketed, v rámci ohraničené sady. Několik běžných příkladů datová pole, které jsou použity jako klíče s názvem oddílu by oblasti, PSČ, zákazníka skupiny nebo jiné obchodní hranice.
* Vytváření oddílů singleton. Singleton oddíly jsou obvykle používány, když služba hello nevyžaduje žádné další směrování. Například bezstavové služby použijte toto schéma rozdělení oddílů ve výchozím nastavení.

S názvem a schémata rozdělení oddílů Singleton jsou speciální formy pohyboval oddíly. Ve výchozím nastavení hello šablony sady Visual Studio pro použití Service Fabric pohyboval oddílů, protože je hello jedním nejběžnějších a užitečné. Hello zbývající část tohoto článku se zaměřuje na schéma rozdělení oddílů pohyboval hello.

### <a name="ranged-partitioning-scheme"></a>Pohyboval schéma rozdělení oddílů
Toto je použité toospecify celé číslo v rozsahu (identifikovaný dolní klíč a vysoká hodnota klíče) a počet oddílů (ne). Vytvoří n oddílů, každý zodpovědná za Podoblast sady nepřekrývají hello celkový rozsah klíče oddílu. Například ranged schéma rozdělení oddílů s nízkou klíč 0, vysoká hodnota klíče 99 a počet 4 by vytvořit čtyři oddíly, jak je uvedeno níže.

![Vytváření oddílů v rozsahu](./media/service-fabric-concepts-partitioning/range-partitioning.png)

Běžný postup je toocreate hodnotu hash na základě jedinečné klíče v rámci sady dat hello. Několik běžných příkladů klíčů by vehicle identifikační číslo (VIN), ID zaměstnance nebo do jedinečného řetězce. Pomocí této jedinečný klíč by pak generovat kód hash, numerického zbytku hello klíče rozsahu, toouse jako klíč. Můžete zadat hello horní a dolní meze hello povolený rozsah klíče.

### <a name="select-a-hash-algorithm"></a>Vyberte algoritmus hash
Důležitou součástí generování hodnoty hash je výběr vaší algoritmus hash. Aspekt spočívá v tom, zda text hello cílem je toogroup podobné klíče vedle sebe (polohu citlivé algoritmu hash)--nebo pokud aktivity by měl být široce distribuovány na všechny oddíly (distribuční algoritmu hash), což je dnes běžné.

Hello vlastnosti algoritmu hash správné distribuční jsou je snadno toocompute, má několik kolize a rovnoměrně distribuuje hello klíče. Dobrým příkladem algoritmu hash efektivní je hello [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) algoritmus hash.

Dobrý prostředku obecné hash kód algoritmus možnosti, které je hello [Wikipedia stránky na funkce hash](http://en.wikipedia.org/wiki/Hash_function).

## <a name="build-a-stateful-service-with-multiple-partitions"></a>Sestavení stavové služby s více oddílů
Umožňuje vytvořit vaše první spolehlivé stavové služby s více oddílů. V tomto příkladu vytvoříte velmi jednoduchou aplikaci, kam chcete toostore všechny poslední názvy, které začínají hello stejné písmeno v hello stejného oddílu.

Než napíšete kód, je nutné toothink o hello oddílů a klíčů oddílů. Potřebujete 26 oddíly (jeden pro každou písmeno v hello abecedy), ale co o hello nízkou a vysokou klíčů?
Jak chceme oznámena toohave jeden oddíl na písmeno, můžeme použít 0 jako hello dolní klíč a 25 jako hello vysoká hodnota klíče, jako je každý písmeno svůj vlastní klíč.

> [!NOTE]
> Toto je zjednodušený scénář, ve skutečnosti by byla nevyrovnaná distribuce hello. Příjmení začíná hello písmena "S" nebo "M" jsou častější než ty, které jsou hello začíná "X" nebo "Y".
> 
> 

1. Otevřete **sady Visual Studio** > **soubor** > **nové** > **projektu**.
2. V hello **nový projekt** dialogovém okně vyberte aplikace Service Fabric hello.
3. Volání projektu hello "AlphabetPartitions".
4. V hello **vytvoření služby** dialogovém okně vyberte **Stateful** služby a pojmenujte ji "Alphabet.Processing", jak ukazuje následující obrázek hello.
       ![Dialogové okno Nová služba v sadě Visual Studio][1]

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. Nastavit hello počet oddílů. Otevřete hello Applicationmanifest.xml soubor umístěný ve hello ApplicationPackageRoot složky hello AlphabetPartitions projekt a aktualizace hello parametru Processing_PartitionCount too26, jak je uvedeno níže.
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    Musíte taky tooupdate hello LowKey a HighKey vlastnosti elementu StatefulService hello hello ApplicationManifest.xml, jak je uvedeno níže.
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. Pro služby toobe hello přístupný otevře koncový bod na portu přidáním hello koncový bod elementu ServiceManifest.xml (umístěný ve složce PackageRoot hello) pro hello Alphabet.Processing služby, jak je uvedeno níže:
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    Nyní je služba hello nakonfigurované toolisten tooan vnitřní koncový bod s 26 oddíly.
7. Dále musíte toooverride hello `CreateServiceReplicaListeners()` metoda hello zpracování třídy.
   
   > [!NOTE]
   > Tato ukázka předpokládáme, že používáte jednoduché HttpCommunicationListener. Další informace o komunikace spolehlivé služby najdete v tématu [hello spolehlivá služba komunikační model](service-fabric-reliable-services-communication.md).
   > 
   > 
8. Doporučené vzor hello adresu URL, která replika naslouchá na je hello následující formát: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.
    Proto je vhodné tooconfigure vaší komunikace toolisten naslouchací proces na správné koncové body hello a s tohoto vzoru.
   
    Víc replik tato služba může být hostitelem hello stejný počítač, takže tato adresa musí toobe jedinečný toohello repliky. Z tohoto důvodu ID oddílu + ID repliky jsou v adrese URL hello. HttpListener můžete naslouchat na více adres na stejný port, dokud je jedinečnou předponu adresy URL hello hello.
   
    Dobrý den, který je navíc GUID k pokročilé případu, kdy sekundární repliky také naslouchání požadavkům jen pro čtení. Po hello případ, budete chtít toomake jistotu, že se při přechodu z primární toosecondary tooforce klienti toore vyřešit hello adresy slouží novou jedinečnou adresu. '+' se používá jako hello adresy tady tak, aby hello replika naslouchá na všech dostupných hostitelů (IP, FQDM, localhost atd.) hello následující kód ukazuje příklad.
   
    ```CSharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    Je také vhodné poznamenat, že hello publikovat adresu URL se mírně liší od hello naslouchání předponu adresy URL.
    Adresa URL naslouchání Hello dostane tooHttpListener. Hello hello adresu URL, která je publikovaná toohello služby prostředků infrastruktury služby DNS, který se používá pro zjišťování služby je publikovaná adresa URL. Klienti požádá pro tuto adresu prostřednictvím služby zjišťování. Adresa Hello klientům získat potřebám toohave hello skutečné IP adresu nebo plně kvalifikovaný název domény uzlu hello v tooconnect pořadí. Proto musíte tooreplace '+' s hello uzlu IP adresu nebo plně kvalifikovaný název domény jako uvedené výše.
9. posledním krokem Hello je tooadd hello zpracování logiky toohello služby, jak je uvedeno níže.
   
    ```CSharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "sucessfully added" : "already exists");
        }
    }
    ```
   
    `ProcessInternalRequest`čtení hello hodnoty hello dotazu řetězec parametr použitý toocall hello oddílu a volání `AddUserAsync` tooadd hello lastname toohello spolehlivé slovník `dictionary`.
10. Umožňuje přidat bezstavové služby toohello projektu toosee jak můžete volat na konkrétní oddíl.
    
    Tato služba slouží jako v jednoduchém webovém rozhraní, která přijímá hello lastname jako parametr řetězce dotazu, Určuje klíč oddílu hello a odešle ji toohello Alphabet.Processing služby pro zpracování.
11. V hello **vytvoření služby** dialogovém okně vyberte **Stateless** služby a pojmenujte ji "Alphabet.Web", jak je uvedeno níže.
    
    ![Snímek obrazovky bezstavové služby](./media/service-fabric-concepts-partitioning/createnewstateless.png).
12. Aktualizujte informace hello koncového bodu v hello ServiceManifest.xml hello Alphabet.WebApi služby tooopen až portu, jak je uvedeno níže.
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. Je nutné tooreturn kolekce ServiceInstanceListeners ve třídě hello Web. Znovu můžete zvolit tooimplement jednoduché HttpCommunicationListener.
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is hello node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. Teď musíte tooimplement hello zpracování logiky. Hello HttpCommunicationListener volání `ProcessInputRequest` když přijde žádost. Proto přejdeme dopředu a přidejte následující kód hello.
    
    ```CSharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from hello first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added tooPartition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    Podívejme se krok za krokem. Hello kód čte hello první písmeno parametr řetězce dotazu hello `lastname` do char. Poté, určí hello klíč oddílu pro toto písmeno odečtením hello šestnáctkové hodnoty `A` z hello šestnáctkové hodnoty hello příjmení první písmeno.
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    Pamatujte si, že v tomto příkladu že používáme 26 oddíly s klíčem jeden oddíl na oddíl.
    V dalším kroku získáme oddílu služby hello `partition` pro tento klíč pomocí hello `ResolveAsync` metodu hello `servicePartitionResolver` objektu. `servicePartitionResolver`je definován jako
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    Hello `ResolveAsync` URI metoda trvá hello služby, klíč oddílu hello a token zrušení jako parametry. Hello URI služby pro služby zpracování hello je `fabric:/AlphabetPartitions/Processing`. V dalším kroku se nám získat koncový bod hello hello oddílu.
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    Nakonec sestavit adresu URL koncového bodu hello plus hello řetězce dotazu a volání hello služby zpracování.
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    Po dokončení zpracování hello výstup hello jsme zapsat zpět.
15. posledním krokem Hello je tootest hello služby. Parametry aplikace Visual Studio používá pro místní a cloudové nasazení. Služba hello tootest s 26 oddíly místně, je nutné tooupdate hello `Local.xml` souborů ve složce ApplicationParameters hello hello AlphabetPartitions projektu, jak je uvedeno níže:
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. Po dokončení nasazení můžete zkontrolovat hello služby a všechny její oddíly v hello Service Fabric Exploreru.
    
    ![Snímek obrazovky Service Fabric Exploreru](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. V prohlížeči, můžete otestovat hello dělení logiku zadáním `http://localhost:8081/?lastname=somename`. Zobrazí se, že každý příjmení začne s hello stejné písmeno ukládají do hello stejného oddílu.
    
    ![Snímek obrazovky prohlížeče](./media/service-fabric-concepts-partitioning/samplerunning.png)

Ukázka hello Hello celý zdrojový kód je k dispozici na [Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="next-steps"></a>Další kroky
Informace o konceptech Service Fabric najdete v tématu hello následující:

* [Dostupnost služeb Service Fabric](service-fabric-availability-services.md)
* [Škálovatelnost služby Service Fabric](service-fabric-concepts-scalability.md)
* [Plánování kapacity pro aplikace Service Fabric](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png