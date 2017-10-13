---
title: "Začínáme s Azure předávání WCF předává v rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak používat předávací službu WCF předávání přes Azure pro připojení dvě aplikace, které jsou hostované v různých umístěních."
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
ms.openlocfilehash: 1af1ac78398d65e6a87f0d24d6198f3dfbc82ffd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-relay-wcf-relays-with-net"></a>Jak používat Azure předávání WCF předává s rozhraním .NET
Tento článek popisuje, jak používat službu předávání přes Azure. Ukázky jsou napsané v C# a používají API Windows Communication Foundation (WCF) s rozšířeními, které jsou součástí sestavení Service Bus. Další informace o předávání přes Azure najdete v tématu [předávání přes Azure přehled](relay-what-is-it.md).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a>Co je předávání WCF?

Azure [ *WCF předávání* ](relay-what-is-it.md) služba vám umožní sestavovat hybridní aplikace, které poběží v datovém centru Azure i vlastní místní podnikovém prostředí. Předávací službou docílí tak, že vám umožní bezpečně vystavit služby Windows Communication Foundation (WCF), které se nacházejí v podnikové síti, veřejnému cloudu, bez nutnosti otevřít spojení ve firewallu nebo vyžadování nežádoucí změny v infrastruktuře podnikové sítě.

![Koncepty předávání WCF](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Předávání přes Azure vám umožňuje hostovat služby WCF ve vaší existující podnikové síti. Potom můžete delegovat čekání na příchozí relace a požadavky na tyto služby WCF do předávací služby běžící v Azure. To vám umožní vystavit tyto služby aplikačnímu kódu běžícímu v Azure nebo mobilním pracovníkům nebo partnerským prostředím v extranetu. Předávání vám umožňuje bezpečně řídit, kdo má přístup k těmto službám, na velice přesné úrovni. Nabízí výkonný a bezpečný způsob, jak vystavit data a funkce aplikací z existujících podnikových řešení a využívat jejich výhod z cloudu.

Tento článek popisuje, jak používat předávání přes Azure k vytvoření webové služby WCF vystavené pomocí kanálu TCP vazby, které implementují zabezpečenou konverzaci mezi dvěma účastníky.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Získání balíčku Service Bus NuGet
Nejsnadnějším způsobem, jak odkazovat na rozhraní API služby Service Bus a nakonfigurovat svoji aplikaci se všemi závislostmi služby Service Bus, je balíček [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Balíček NuGet můžete do svého projektu nainstalovat takto:

1. V Průzkumníku řešení klikněte pravým tlačítkem na **Reference**, a pak klikněte na **Správa balíčků NuGet**.
2. Vyhledejte „Service Bus“ a vyberte položku **Microsoft Azure Service Bus**. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete následující dialogové okno:
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a>Vystavení a spotřebování webové služby SOAP pomocí TCP
Pokud chcete existující webovou službu WCF SOAP vystavit pro externí spotřebu, musíte udělat několik změn v adresách a vazbách služby. K tomu může být potřeba změnit konfigurační soubor nebo kód, podle toho, jak máte služby WCF vytvořené a nastavené. Všimněte si, že WCF umožňuje mít několik koncových bodů sítě v jedné službě, abyste mohli zachovat stávající vnitřních koncových bodů při přidávání předávání koncové body pro externí přístup ve stejnou dobu.

V této úloze sestavit jednoduchou službu WCF a přidejte do ní naslouchací proces předávání. Toto cvičení předpokládá, že umíte do jisté míry pracovat s Visual Studiem, a proto vás neprovede úplně všemi podrobnými kroky pro vytvoření projektu. Naopak se zaměřuje na kód.

Než s těmito kroky začnete, dokončete následující postup a nastavte prostředí:

1. Ve Visual Studiu vytvořte konzolovou aplikaci, která v řešení bude obsahovat dva projekty – „Client“ a „Service“.
2. Do obou projektů přidejte balíček Service Bus NuGet. Projekty díky němu budou mít všechny potřebné reference k sestavení.

### <a name="how-to-create-the-service"></a>Jak vytvořit službu
Nejdřív vytvořte službu samotnou. Každá služba WCF se skládá nejméně ze tří různých částí:

* Definice kontraktu, která popisuje, které zprávy se vyměňují a jaké operace se mají vyvolávat.
* Implementace tohoto kontraktu.
* Hostitel, který hostuje službu WCF a zveřejňuje několik koncových bodů.

Příklady kódu v této části se vztahují na všechny části.

Kontrakt definuje jednu operaci `AddNumbers`, která sečte dvě čísla a vrátí výsledek. Rozhraní `IProblemSolverChannel` umožní klientovi snadnější správu doby platnosti proxy. Vytvoření takového rozhraní se obvykle považuje za vhodné řešení. Definici kontraktu se taky doporučuje dát do samostatného souboru, aby se mohla používat jako reference v projektech „Client“ i „Service“, ale kód můžete taky jednoduše zkopírovat do obou projektů.

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

Implementace s je kontrakt hotový, vypadá takto:

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
Když je kontrakt hotový a implementovaný, můžete hostovat službu. Hostování se provádí v objektu [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx), který se stará o správu instancí služby a hostuje koncové bodu, které čekají na zprávy. Následující kód konfiguruje službu s běžným místním koncovým bodem a koncový bod předávání pro ilustraci vzhled vedle sebe, interních a externích koncových bodů. Řetězec *namespace* nahraďte názvem vašeho oboru názvů a *yourKey* klíčem SAS, které jste získali v předchozím kroku.

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

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

V příkladu vytvoříte dva koncové body, které jsou na stejné implementaci kontraktu. Jeden je místní a druhý je promítnutý přes předávání přes Azure. Hlavní rozdíly mezi nimi jsou vazby: [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) pro místní a [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) pro koncový bod předávání a adresy. Místní koncový bod má místní síťovou adresu s vlastním portem. Koncový bod předávání má adresu koncového bodu složenou z řetězce `sb`, název vašeho oboru názvů a cesty "solver". Výsledkem je identifikátor URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifikace koncový bod služby jako koncový bod Service Bus (relé) TCP s plně kvalifikovaným názvem externí DNS. Pokud do kódu vložíte skutečné názvy místo zástupných názvů a vložíte ho do funkce `Main` aplikace **Service**, budete mít funkční službu. Pokud chcete, aby vaše služba naslouchala jen na předávací službu, odstraňte deklaraci místního koncového bodu.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Konfigurace hostitele služby v souboru App.config
Hostitele taky můžete konfigurovat pomocí souboru App.config. V tomto případě bude kód pro hostování vypadat jako kód v následujícím příkladu.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Definice koncových bodů se přesunou do souboru App.config. Balíček NuGet už přidal velké množství definic do souboru App.config, které jsou potřeba pro rozšíření konfigurace pro předávání přes Azure. Následující příklad je naprosto stejný jako předchozí kód a měl by být hned pod elementem **system.serviceModel**. Tento příklad kódu předpokládá, že se obor názvů C# vašeho projektu jmenuje **Service**.
Nahraďte zástupné symboly předávání název oboru názvů a klíče SAS.

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

Když provedete změny, služba se spustí jako předtím, ale tentokrát budou dva živé koncové body: jeden místní a jeden v cloudu.

### <a name="create-the-client"></a>Vytvoření klienta
#### <a name="configure-a-client-programmatically"></a>Programová konfigurace klienta
Aby se služba mohla spotřebovávat, musíte postavit klienta WCF pomocí objektu [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx). Service Bus používá tokenový model zabezpečení, implementovaný pomocí SAS. Třída [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření, které vracení některé známé poskytovatele tokenů. Následující příklad používá metodu [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) ke zpracování získání příslušného tokenu SAS. Název a klíč se získá z portálu způsobem popsaným v předchozí části.

Nejdřív zkopírujte nebo odkažte na kód kontraktu `IProblemSolver` ze služby do vašeho klientského projektu.

Pak nahraďte kód v `Main` metoda klienta, znovu nahraďte zástupný text předávání názvů a klíče SAS.

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

Teď můžete sestavit klienta a službu, spustit je (službu jako první) a klient zavolá službu a vypíše **9**. Klient a server můžete spustit na různých počítačích, dokonce i v jiných sítích, a komunikace bude stále fungovat. Kód klienta se taky může spustit v cloudu nebo lokálně.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Konfigurace klienta v souboru App.config
Následující kód ukazuje, jak nakonfigurovat klienta pomocí souboru App.config.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Definice koncových bodů se přesunou do souboru App.config. Následující příklad, což je stejný jako kód uvedených výše, měl by být přímo pod `<system.serviceModel>` elementu. Zde jako předtím, je potřeba nahradit zástupné symboly předávání názvů a klíče SAS.

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
Teď, když jste se naučili základy používání služby předávání přes Azure, postupujte podle následujících odkazech na další informace.

* [Co je Azure Relay?](relay-what-is-it.md)
* [Přehled architektury služby Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* Stáhněte si ukázky pro Service Bus z [ukázek Azure] [ Azure samples] nebo se podívejte [přehled ukázek pro Service Bus][overview of Service Bus samples].

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
