---
title: "aaaCreate front-end webové aplikace Azure Service Fabric pomocí ASP.NET Core | Microsoft Docs"
description: "Vystavení webového toohello aplikace Service Fabric pomocí služby projektu ASP.NET Core a komunikace mezi službami prostřednictvím vzdálené komunikace služby."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: 0c4454d6cac4c2e343bd6e93e56d3d2f0ebfc4ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Vytvoření služby front-end webové aplikace pomocí ASP.NET Core
Ve výchozím nastavení neposkytují služby Azure Service Fabric webové toohello veřejné rozhraní. tooexpose klienti tooHTTP funkce aplikace, máte toocreate webového projektu tooact jako vstupní bod a poté komunikaci ze zde tooyour jednotlivých služeb.

V tomto kurzu budeme pokračovat tam kde jsme skončil v hello [vytvoření vaší první aplikace v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) kurzu a přidání webové služby ASP.NET Core před hello čítač stavové služby. Pokud jste tak již neučinili, měli přejděte zpět a první krok prostřednictvím tohoto kurzu.

## <a name="set-up-your-environment-for-aspnet-core"></a>Nastavení prostředí pro ASP.NET Core

Budete potřebovat Visual Studio 2017 toofollow spolu se v tomto kurzu. Všechny edice provede. 

 - [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/)

aplikace ASP.NET Core Service Fabric toodevelop, byste měli mít hello úlohy, které jsou nainstalované následující:
 - **Vývoj pro Azure** (v části *Web a Cloud*)
 - **Vývoj pro ASP.NET a webové** (v části *Web a Cloud*)
 - **Vývoj pro různé platformy .NET core** (v části *ostatní modulové*)

>[!Note] 
>Hello .NET Core nástrojů pro Visual Studio 2015 se už aktualizují, ale pokud stále používáte Visual Studio 2015, je třeba toohave [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) nainstalována.

## <a name="add-an-aspnet-core-service-tooyour-application"></a>Přidání aplikace ASP.NET Core služby tooyour
ASP.NET Core je rozhraní vývoj webové lightweight, a platformy, můžete použít toocreate moderní webového uživatelského rozhraní a webovým rozhraním API. 

Umožňuje přidat existující aplikaci tooour projekt webového rozhraní API ASP.NET.

1. V Průzkumníku řešení klikněte pravým tlačítkem na **služby** uvnitř hello projekt aplikace a zvolte **Přidat > Nový Service Fabric Service**.
   
    ![Přidání nové služby tooan existující aplikace][vs-add-new-service]
2. Na hello **vytvoření služby** vyberte **ASP.NET Core** a pojmenujte ho.
   
    ![Výběr webové služby ASP.NET v hello dialogové okno Nová služba][vs-new-service-dialog]

3. Další stránku Hello poskytuje sadu ASP.NET Core šablony projektů. Všimněte si, že tyto jsou hello stejné možnosti, které by vidíte, pokud jste vytvořili projekt ASP.NET Core mimo aplikace Service Fabric pomocí malé množství další kód tooregister hello služby pomocí modulu runtime Service Fabric hello. V tomto kurzu zvolte **webového rozhraní API**. Můžete však použít hello stejné koncepty toobuilding úplné webové aplikace.
   
    ![Výběr typu projektu ASP.NET][vs-new-aspnet-project-dialog]
   
    Po vytvoření projektu webového rozhraní API byste měli mít dvě služby ve vaší aplikaci. Při dalším toobuild vaší aplikace, můžete přidat další služby v přesně hello stejným způsobem. Každý může být nezávisle verzí a upgradovaný.

## <a name="run-hello-application"></a>Spuštění aplikace hello
tooget představu o co jsme krok, můžeme nasadit hello novou aplikaci a proveďte poskytuje pohled na hello výchozí chování, které hello webového rozhraní API ASP.NET Core šablony.

1. V sadě Visual Studio toodebug hello aplikaci stisknutím klávesy F5.
2. Po dokončení nasazení sady Visual Studio spustí prohlížeče toohello kořenovém hello služby ASP.NET Web API. Šablona webového rozhraní API ASP.NET Core Hello neposkytuje výchozí chování pro kořenový adresář hello, proto byste měli vidět chybu 404 v prohlížeči hello.
3. Přidat `/api/values` toohello umístění v prohlížeči hello. Tím se spustí hello `Get` metodu hello ValuesController v šabloně hello webového rozhraní API. Vrátí hello výchozí odpovědi, které poskytuje šablony hello – pole JSON, který obsahuje dva řetězce:
   
    ![Výchozí hodnot vrácených z webového rozhraní API ASP.NET Core šablony][browser-aspnet-template-values]
   
    Hello konec hello kurzu zobrazí tato stránka hello nejnovější hodnotu čítače z našich stavové služby místo hello výchozí řetězce.

## <a name="connect-hello-services"></a>Připojení služby hello
Service Fabric nabízí flexibilitu v tom, jak komunikovat se službami reliable services. V rámci jedné aplikace můžete mít služby, které jsou přístupné přes TCP, jiných služeb, které jsou přístupné přes rozhraní HTTP REST API a stále jiných služeb, které jsou přístupné přes webové sokety. Pro informace o dostupných možnostech hello a hello kompromisy související se situací, viz [komunikaci se službou](service-fabric-connect-and-communicate-with-services.md). V tomto kurzu používáme [vzdálené komunikace služby Service Fabric](service-fabric-reliable-services-communication-remoting.md), zadaný v hello SDK.

V hello (modelován na vzdálených volání procedur nebo vzdálených volání procedur) přístup vzdálené komunikace služby definujete rozhraní tooact jako hello veřejného kontraktu služby hello. Potom pomocí tohoto rozhraní toogenerate třídu proxy pro interakci s hello služby.

### <a name="create-hello-remoting-interface"></a>Vytvoření hello vzdálené komunikace rozhraní
Začněme vytvořením hello rozhraní tooact jako hello smlouva mezi hello stavové služby a další služby v této případu hello webového projektu ASP.NET Core. Všechny služby, které ho, vytvoříme ji v jeho vlastní projektu knihovny tříd používají toomake volání RPC, musí být sdílená toto rozhraní.

1. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a zvolte **přidat** > **nový projekt**.

2. Zvolte hello **Visual C#** položku v levé navigační podokně a pak vyberte hello hello **knihovny tříd** šablony. Ujistěte se, že verze rozhraní .NET Framework hello je nastaven příliš**4.5.2**.
   
    ![Vytvoření projektu rozhraní pro stavové služby][vs-add-class-library-project]

3. Nainstalujte hello **Microsoft.ServiceFabric.Services.Remoting** balíček NuGet. Vyhledejte **Microsoft.ServiceFabric.Services.Remoting** v hello NuGet balíček správce a nainstalujte ho pro všechny projekty v řešení hello, které používají vzdálené komunikace služby, včetně:
   - Hello knihovny tříd projekt, který obsahuje rozhraní služby hello
   - Projekt stavové služby Hello
   - Hello projektu webové služby ASP.NET Core
   
    ![Přidává se balíček NuGet služby hello][vs-services-nuget-package]

4. V knihovně tříd hello, vytvořit rozhraní s jedinou metodu `GetCountAsync`, a rozšířit hello rozhraní `Microsoft.ServiceFabric.Services.Remoting.IService`. Hello vzdálené komunikace rozhraní musí být odvozeny od tooindicate toto rozhraní je rozhraní, které je Vzdálená komunikace služby.
   
    ```c#
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-hello-interface-in-your-stateful-service"></a>Implementovat rozhraní hello stavové služby
Teď, když jsme definovali hello rozhraní, potřebujeme tooimplement v hello stavové služby.

1. Stavové služby přidejte projekt knihovny třída toohello odkaz, který obsahuje rozhraní hello.
   
    ![Přidání projektu knihovny tříd toohello odkaz v hello stavové služby][vs-add-class-library-reference]
2. Vyhledejte hello třídu, která dědí z `StatefulService`, jako například `MyStatefulService`a rozšířit ji tooimplement hello `ICounter` rozhraní.
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. Nyní implementovat hello jedné metody, která je definována v hello `ICounter` rozhraní `GetCountAsync`.
   
    ```c#
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-hello-stateful-service-using-a-service-remoting-listener"></a>Vystavení hello stavové služby pomocí služby vzdálené komunikace naslouchací proces
S hello `ICounter` implementace rozhraní hello posledním krokem je tooopen hello vzdálené komunikace služby komunikační kanál. Pro stavové služby, Service Fabric nabízí přepisovatelné metodu s názvem `CreateServiceReplicaListeners`. Pomocí této metody můžete určit jeden nebo více naslouchací procesy komunikace, na základě typu hello komunikace, že chcete tooenable pro vaši službu.

> [!NOTE]
> volání metody ekvivalentní Hello pro otevření komunikační kanál toostateless služby `CreateServiceInstanceListeners`.

V takovém případě jsme nahradit stávající hello `CreateServiceReplicaListeners` metoda a zadejte instanci `ServiceRemotingListener`, která vytvoří koncový bod protokolu RPC, lze volat z klientů prostřednictvím `ServiceProxy`.  

Hello `CreateServiceRemotingListener` rozšiřující metody na hello `IService` rozhraní vám umožní tooeasily vytvořit `ServiceRemotingListener` s všechna výchozí nastavení. toouse této metody rozšíření, zajistěte, abyste měli hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` importovat obor názvů. 

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-hello-serviceproxy-class-toointeract-with-hello-service"></a>Používat hello ServiceProxy třída toointeract službou hello
Naše stavové služby je nyní připraven tooreceive provoz z jiných služeb prostřednictvím protokolu RPC. Takže zůstane je přidání hello kód toocommunicate s ním z hello webovou službu ASP.NET.

1. V projektu ASP.NET přidejte reference knihovny tříd toohello obsahující hello `ICounter` rozhraní.

2. Dříve jste přidali hello **Microsoft.ServiceFabric.Services.Remoting** projekt ASP.NET toohello balíček NuGet. Tento balíček poskytuje hello `ServiceProxy` třída, která používáte toomake RPC volá toohello stavové služby. Zkontrolujte, zda že tento balíček NuGet je nainstalován v hello projektu webové služby ASP.NET Core.

4. V hello **řadiče** složku, otevřete hello `ValuesController` třídy. Všimněte si, že hello `Get` metoda aktuálně právě vrátí pole pevně řetězce "hodnota1" a "hodnota2"--který by odpovídal co jsme viděli dříve v prohlížeči hello. Tato implementace nahraďte hello následující kód:
   
    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    první řádek kódu Hello je klíč hello jeden. toocreate hello ICounter proxy toohello stavové služby, budete potřebovat dva kusy tooprovide informace: název oddílu ID a hello hello služby.
   
    Rozdělení tooscale stavové služby můžete použít porušením jejich stav do různých intervalů, na základě klíče, které definujete, jako je ID zákazníka nebo PSČ. V naší aplikaci trivial má hello stavové služby jenom jeden oddíl, takže hello klíč není důležité. Všechny klíče, který zadáte povede toohello stejného oddílu. toolearn Další informace o vytváření oddílů služby, najdete v části [jak toopartition spolehlivé služby Service Fabric](service-fabric-concepts-partitioning.md).
   
    název služby Hello je identifikátor URI hello formuláře fabric: /&lt;název_aplikace&gt;/&lt;service_name&gt;.
   
    Pomocí těchto dvou položek příslušné Service Fabric jednoznačně identifikovat hello počítač, který by měl být odeslány požadavky. Hello `ServiceProxy` třída také bezproblémově zpracovává hello případ, kdy selže hello počítač, který je hostitelem oddílu hello stavové služby a další počítač musí být propagovaných tootake jeho místo. Tato abstrakce provede zápis hello toodeal kód klienta s jinými službami výrazně jednodušší.
   
    Jakmile jsme hello proxy, můžeme jednoduše vyvolání hello `GetCountAsync` metodu a vrátí její výsledek.

5. Stisknutím klávesy F5 znovu toorun hello upravit aplikace. Jako dříve, Visual Studio se automaticky spustí hello prohlížeče toohello kořenovém hello webového projektu. Přidejte cestu "rozhraní api nebo hodnoty" hello, a měli byste vidět hello aktuální vrátila hodnotu čítače.
   
    ![Hello stavová čítač hodnoty zobrazené v prohlížeči hello][browser-aspnet-counter-value]
   
    Aktualizujte hello prohlížeče pravidelně toosee hello čítače hodnota aktualizace.

## <a name="kestrel-and-weblistener"></a>Kestrel a WebListener

Hello výchozí ASP.NET Core webový server, označuje jako Kestrel, je [není podporován pro zpracování přímé přenosy z Internetu](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel). V důsledku toho hello ASP.NET Core bezstavové služby šablona pro Service Fabric používá [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) ve výchozím nastavení. 

toolearn informace o Kestrel a WebListener v Service Fabric služeb, naleznete příliš[ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="connecting-tooa-reliable-actor-service"></a>Připojení služby tooa spolehlivé objektu Actor
Tento kurz se zaměřuje na přidání webový front-end, který oznamovat se stavovou službou. Můžete však provést velmi podobné tooactors tootalk modelu. Při vytváření spolehlivé objektu Actor projektu sady Visual Studio automaticky generuje projektu rozhraní za vás. Můžete použít tento rozhraní toogenerate proxy služby objektu actor v hello webového projektu toocommunicate s objektu actor hello. Hello komunikační kanál je zadán automaticky. Proto není nutné toodo cokoli, který je ekvivalentní tooestablishing `ServiceRemotingListener` stejně, jako jste pro stavové služby hello v tomto kurzu.

## <a name="how-web-services-work-on-your-local-cluster"></a>Jak fungují webové služby v místním clusteru
Obecně platí můžete nasadit přesně hello stejné Service Fabric aplikace tooa více počítače, které nasazení clusteru v místním clusteru a být vysoce jistý, že funguje podle očekávání. Je to proto, že místní cluster je jednoduše konfiguraci pěti uzly, který je sbalené tooa jeden počítač.

Pokud jde tooweb služby, je však jeden nuance klíče. Při clusteru se nachází za službou Vyrovnávání zatížení, jako je tomu v Azure, musíte zajistit, aby webové služby jsou nasazeny na každý počítač od nástroj pro vyrovnávání zatížení hello jednoduše kruhové dotazování provoz mezi počítači hello. To provedete pomocí nastavení hello `InstanceCount` pro hello služby toohello speciální hodnotu-1.

Naopak při spuštění webové služby místně, je nutné tooensure této pouze jednu instanci služby hello běží. V opačném spustíte do konfliktu z více procesů, které naslouchají na hello stejný port a cesta. V důsledku toho počet instancí služby webové hello by mělo být nastavené příliš "1" pro místní nasazení.

jak tooconfigure různé hodnoty pro jiného prostředí, najdete v části toolearn [Správa parametry aplikace pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Další kroky
Teď, když máte webové přední skončili sadu pro vaši aplikaci s ASP.NET Core, další informace o [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md) pro podrobné pochopení fungování ASP.NET Core s Service Fabric.

Dále [Další informace o komunikaci se službou](service-fabric-connect-and-communicate-with-services.md) obecně tooget a dokončení obrázek o tom, jak služba funguje komunikace v Service Fabric.

Až budete mít dostatečné povědomí o tom, jak funguje komunikace služby, [vytvořte cluster v Azure a nasadit své cloudové aplikace toohello](service-fabric-cluster-creation-via-portal.md).

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
