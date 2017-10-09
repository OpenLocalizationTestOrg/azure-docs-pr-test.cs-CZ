---
title: "aaaGet začít s Azure WCF předávání přes předávací službu v rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak toouse WCF předávání přes Azure předává tooconnect dvě aplikace hostované v různých umístěních."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5493281a-c2e5-49f2-87ee-9d3ffb782c75
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: a652617fc2e9b7c8d62d39fa914f77df6e3a1771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-relay-wcf-relays-with-net"></a>Jak předává toouse WCF předávání přes Azure pomocí rozhraní .NET
Tento článek popisuje, jak toouse hello předávání přes Azure service. Hello ukázky jsou napsané v C# a používají API Windows Communication Foundation (WCF) hello s rozšíření obsažená v hello sestavení Service Bus. Další informace o předávání přes Azure najdete v tématu hello [předávání přes Azure přehled](relay-what-is-it.md).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a>Co je předávání WCF?

Hello Azure [ *WCF předávání* ](relay-what-is-it.md) služba vám umožní toobuild hybridní aplikace, které poběží v datovém centru Azure i vlastní místní podnikovém prostředí. Hello předávací služba docílí tak, že jste toosecurely vystavit služby Windows Communication Foundation (WCF), které se nacházejí v podnikové síti toohello veřejného cloudu, bez nutnosti tooopen spojení ve firewallu nebo nutnosti nežádoucí změny tooa podnikové síťové infrastruktury.

![Koncepty předávání WCF](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Předávání přes Azure umožňuje toohost služby WCF ve vaší existující podnikové síti. Potom můžete delegovat čekání na příchozí relace a požadavky toothese služby toohello předávací služby WCF běžící v Azure. To umožňuje tooexpose můžete tyto služby tooapplication kód spuštěný v Azure, nebo toomobile pracovníkům nebo partnerským prostředím v extranetu. Předávání umožňuje toosecurely řízení, kdo má přístup k těmto službám, na velice přesné úrovni. Poskytuje výkonný a bezpečný způsob tooexpose aplikaci funkce a data z existujících podnikových řešení a využít ji z cloudu hello.

Tento článek popisuje, jak předávání přes Azure toocreate toouse webové služby WCF vystavené pomocí vazeb kanálů TCP, které implementují zabezpečenou konverzaci mezi dvěma účastníky.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-hello-service-bus-nuget-package"></a>Načtení balíčku pro Service Bus NuGet hello
Hello [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je hello nejjednodušší způsob, jak tooget hello rozhraní API služby Service Bus a tooconfigure aplikaci se všemi závislostmi služby Service Bus hello služby. balíček NuGet hello tooinstall ve vašem projektu hello následující:

1. V Průzkumníku řešení klikněte pravým tlačítkem na **Reference**, a pak klikněte na **Správa balíčků NuGet**.
2. Vyhledejte "Service Bus" a vyberte hello **Microsoft Azure Service Bus** položky. Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete následující dialogové okno hello:
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a>Vystavení a spotřebování webové služby SOAP pomocí TCP
tooexpose existující webovou službu WCF SOAP pro externí spotřebu, musíte nastavit adresách a vazbách služby toohello změny. To může vyžadovat změny tooyour konfigurační soubor nebo jej může vyžadovat změny kódu, v závislosti na tom, jak jste nastavili a nakonfigurovali služby WCF. Všimněte si, že WCF umožňuje toohave několik koncových bodů sítě přes hello stejnou službu, abyste mohli zachovat stávající hello vnitřních koncových bodů při přidávání předávání koncové body pro externí přístup na hello stejný čas.

V této úloze sestavit jednoduchou službu WCF a přidejte tooit naslouchací proces předávání. Toto cvičení předpokládá některé znalost Visual Studio a proto vás neprovede úplně všechny podrobnosti hello vytvoření projektu. Místo toho zaměřuje se na kód hello.

Před zahájením těchto kroků, proveďte následující postup tooset prostředí hello:

1. Ve Visual Studiu Vytvořte konzolovou aplikaci, která obsahuje dva projekty – "Client" a "Služba" v rámci řešení hello.
2. Přidejte projekty tooboth balíček Service Bus NuGet hello. Tento balíček přidá všechny projekty tooyour odkazy hello nezbytné sestavení.

### <a name="how-toocreate-hello-service"></a>Jak toocreate hello služby
Nejprve vytvořte samotnou službu hello. Každá služba WCF se skládá nejméně ze tří různých částí:

* Definice kontraktu, která popisuje, jaké zprávy se vyměňují a jaké operace jsou toobe vyvolat.
* Implementace tohoto kontraktu.
* Hostitele, který hostuje službu WCF hello a zpřístupňuje několik koncových bodů.

Příklady kódu Hello v této části se vztahují na všechny tyto součásti.

Hello kontrakt definuje jednu operaci `AddNumbers`, která sečte dvě čísla a vrátí výsledek hello. Hello `IProblemSolverChannel` rozhraní umožňuje hello klienta toomore snadno spravovat hello doby platnosti proxy. Vytvoření takového rozhraní se obvykle považuje za vhodné řešení. Je vhodné tooput této smlouvy definic do samostatného souboru, aby tento soubor můžete odkazovat z projektů "Client" i "Service", ale hello kódu můžete také zkopírovat do obou projektů.

```csharp
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Implementace hello s hello kontrakt hotový, vypadá takto:

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Programová konfigurace hostitele služby
S hello kontrakt a implementaci na místě můžete hostovat službu hello. Hostování se provádí v [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) objektu, který se stará o správu instancí služby hello a hostitelé hello koncové body, které čekají na zprávy. Hello následující kód konfiguruje hello službu s běžným místním koncovým bodem a předávání přes koncový bod tooillustrate hello vzhled, vedle sebe, interních a externích koncových bodů. Nahraďte řetězec hello *obor názvů* názvem vašeho oboru názvů a *yourKey* hello SAS klíč, který jste získali v předchozím kroku hello.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();

sh.Close();
```

V příkladu hello vytvoříte dva koncové body, které jsou na hello stejné smlouvy implementace. Jeden je místní a druhý je promítnutý přes předávání přes Azure. Hello hlavní rozdíly mezi nimi jsou vazby hello; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) pro hello místní a [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) hello předávání koncového bodu a hello adresy. místní koncový bod Hello má místní síťovou adresu s vlastním portem. Hello předávání přes koncový bod má adresu koncového bodu složenou z řetězce hello `sb`, název oboru názvů a hello cesty "solver". Výsledkem je hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifikace hello koncový bod služby jako koncový bod Service Bus (relé) TCP s plně kvalifikovaným názvem externí DNS. Pokud umístíte hello kódu nahraďte zástupné symboly hello do hello `Main` funkce hello **služby** aplikace, budete mít funkční službu. Pokud chcete, aby vaše služba toolisten výhradně na předávání hello, odeberte deklaraci místního koncového bodu hello.

### <a name="configure-a-service-host-in-hello-appconfig-file"></a>Konfigurace hostitele služby v souboru App.config hello
Můžete také nakonfigurovat hello hostitele pomocí souboru App.config hello. Hello služby hostování kódu v tomto případě se zobrazí v dalším příkladu hello.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();
sh.Close();
```

definice koncových bodů Hello přesunout do souboru App.config hello. balíček NuGet Hello již přidán rozsah souboru App.config toohello definice, které jsou hello požadované konfigurace rozšíření pro předávání přes Azure. Následující příklad, který je hello přesný text Hello ekvivalentní hello předchozí kód, měl by být přímo pod hello **system.serviceModel** elementu. Tento příklad kódu předpokládá, že se obor názvů C# vašeho projektu jmenuje **Service**.
Nahraďte zástupné symboly hello předávání název oboru názvů a klíče SAS.

```xml
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://<namespaceName>.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Po provedení těchto změn hello služba spustí jako předtím, ale s dva živé koncové body: jeden místní a jeden v cloudu hello.

### <a name="create-hello-client"></a>Vytvoření klienta hello
#### <a name="configure-a-client-programmatically"></a>Programová konfigurace klienta
tooconsume hello služby, můžete vytvořit pomocí klienta WCF [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objektu. Service Bus používá tokenový model zabezpečení, implementovaný pomocí SAS. Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) třída představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření vracející některé známé poskytovatele tokenů. Hello následující příklad používá hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metoda toohandle hello získání příslušného tokenu SAS hello. Hello název a klíč se získá z portálu hello, jak je popsáno v předchozí části hello.

První, odkaz nebo kopírování hello `IProblemSolver` kód ze služby hello kontraktu do vašeho klientského projektu.

Pak nahraďte kód hello v hello `Main` metoda hello klienta, znovu nahraďte zástupný text hello předávání názvů a klíče SAS.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "<namespaceName>", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Teď můžete sestavit klienta hello a hello službu, spustit je (nejprve spustit službu hello), a hello klient zavolá službu hello a vytiskne **9**. Hello klienta a serveru můžete spustit na různých počítačích, i v rámci sítě a hello komunikace bude stále fungovat. Kód klienta Hello taky můžete spustit v hello cloudu nebo místně.

#### <a name="configure-a-client-in-hello-appconfig-file"></a>Konfigurace klienta v souboru App.config hello
Hello následující kód ukazuje, jak hello tooconfigure hello klienta pomocí souboru App.config.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

definice koncových bodů Hello přesunout do souboru App.config hello. Hello následující příklad, který je hello stejný jako kód hello uvedených výše, měl by být přímo pod hello `<system.serviceModel>` elementu. Zde jako předtím, je potřeba nahradit zástupné symboly hello předávání názvů a klíče SAS.

```xml
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://<namespaceName>.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello předávání přes Azure, použijte tyto odkazy toolearn Další.

* [Co je Azure Relay?](relay-what-is-it.md)
* [Přehled architektury služby Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* Stáhněte si ukázky pro Service Bus z [ukázek Azure] [ Azure samples] nebo najdete hello [přehled ukázek pro Service Bus][overview of Service Bus samples].

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
