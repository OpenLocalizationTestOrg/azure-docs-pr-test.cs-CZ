---
title: "kurz předávání přes Service Bus WCF aaaAzure | Microsoft Docs"
description: "Sestavení Service Bus klient aplikace a služby WCF předáváním přes."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: sethm
ms.openlocfilehash: 78cd52ef51e9fcfcda2f13ec54bde3af50d76476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-tutorial"></a>Kurz pro Azure předávání WCF

Tento kurz popisuje, jak toobuild jednoduché WCF předávání klientská aplikace a služby pomocí předávání přes Azure. Podobný kurz, který používá [zasílání zpráv Service Bus](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), najdete v části [začít pracovat s fronty Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

Absolvování tohoto kurzu pochopíte hello kroků, které jsou požadované toocreate WCF přenosový klient a služba aplikace. Stejně jako jejich původní protějšky WCF je služba konstrukce, která vystavuje jeden nebo více koncových bodů, každý z nich vystavuje jednu nebo víc operací služeb. Hello koncový bod služby specifikuje adresu, kde se dá hello služba najít, vazbu, která obsahuje hello informace, které klient musí komunikovat s hello service a kontrakt, který definuje hello funkce poskytované službou hello služby tooits klientů. Hello hlavní rozdíl mezi WCF a předávací WCF je, že tento koncový bod hello vystavený v cloudu hello místo místně na vašem počítači.

Po absolvování hello řady témat v tomto kurzu budete mít funkční službu a klienta, který může vyvolat operace hello služby hello. Hello první téma popisuje, jak tooset si účet. Hello další kroky popisují, jak toodefine služby používající kontrakt, jak tooimplement hello služby a jak tooconfigure hello služby v kódu. Najdete zde také popis jak toohost a spusťte službu hello. Hello vytvořená služba se hostuje sama a klient hello a služba běží na hello stejný počítač. Hello služby můžete nakonfigurovat pomocí kódu nebo konfiguračního souboru.

Hello poslední tři kroky popisují, jak toocreate klientskou aplikaci, konfiguraci hello klientskou aplikaci a vytvořit a použít klienta, který můžete přístup k funkcím hello hello hostitele.

## <a name="prerequisites"></a>Požadavky

toocomplete tohoto kurzu budete potřebovat hello následující:

* [Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com). Tento kurz používá Visual Studio 2017.
* Aktivní účet Azure. Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).

## <a name="create-a-service-namespace"></a>Vytvoření oboru názvů služby

prvním krokem Hello je toocreate obor názvů a tooobtain [sdíleného přístupového podpisu (SAS)](../service-bus-messaging/service-bus-sas.md) klíč. Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu předávání přes hello. Klíč SAS je automaticky generován hello systému při vytvoření oboru názvů služby. Hello kombinace oboru názvů služby a klíče SAS poskytuje pověření hello Azure tooauthenticate přístup tooan aplikace. Postupujte podle hello [pokynů tady](relay-create-namespace-portal.md) toocreate předávání názvů.

## <a name="define-a-wcf-service-contract"></a>Definování kontraktu služby WCF

Hello kontrakt služby specifikuje, jaké operace (hello termín webových služeb pro metody nebo funkce) hello služba podporuje. Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic. Každá metoda v rozhraní hello odpovídá tooa konkrétní operaci služby. Každé rozhraní musí mít hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) tooit použit atribut a každou operaci musí mít hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) tooit atribut použitý. Pokud metoda v rozhraní, které má hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atribut nemá hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) atribut, taková metoda se nevystaví. Kód Hello k těmto úlohám najdete v příkladu hello hello postupem. Diskuzi o kontraktech a službách, najdete v části [návrh a implementace služeb](https://msdn.microsoft.com/library/ms729746.aspx) v hello dokumentaci WCF.

### <a name="create-a-relay-contract-with-an-interface"></a>Vytvoření kontraktu předávání s rozhraním

1. Otevřete Visual Studio jako správce tak, že kliknete pravým tlačítkem na programu hello v hello **spustit** nabídky a výběrem **spustit jako správce**.
2. Vytvořte nový projekt konzolové aplikace. Klikněte na tlačítko hello **soubor** nabídku a vyberte **nový**, pak klikněte na tlačítko **projektu**. V hello **nový projekt** dialogové okno, klikněte na tlačítko **Visual C#** (Pokud **Visual C#** nezobrazí, podívejte se do části **jiné jazyky**). Klikněte na tlačítko hello **konzolovou aplikaci (rozhraní .NET Framework)** šablony a pojmenujte ji **EchoService**. Klikněte na tlačítko **OK** toocreate hello projektu.

    ![][2]

3. Nainstalujte balíček Service Bus NuGet hello. Tento balíček automaticky přidá Reference knihovny Service Bus toohello, jakož i hello WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je hello obor názvů, který vám umožní tooprogrammatically přístup hello základním funkcím WCF. Service Bus používá mnoho objektů hello a atributy kontrakty toodefine služeb WCF.

    V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello a pak klikněte na tlačítko **spravovat balíčky NuGet...** . Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Zkontrolujte, že název projektu hello je vybraný v hello **verze** pole. Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.

    ![][3]
4. V Průzkumníku řešení klikněte dvakrát na tooopen souboru Program.cs hello ji v editoru hello, pokud ještě není otevřete.
5. Přidejte následující hello pomocí příkazů v horní části hello hello souboru:

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. Změna hello název oboru názvů z výchozího názvu **EchoService** příliš**Microsoft.ServiceBus.Samples**.

   > [!IMPORTANT]
   > Tento kurz používá obor názvů hello C# **Microsoft.ServiceBus.Samples**, které je obor názvů hello hello na základě smlouvy spravovaný typ, který se používá v konfiguračním souboru hello v hello [konfigurace klienta WCF hello](#configure-the-wcf-client) krok. Můžete zadat obor názvů, které chcete při sestavování této ukázky; kurz hello však nebude fungovat nezměníte pak hello názvů hello kontraktu a služby podle toho, v konfiguračním souboru aplikace hello. Hello obor názvů specifikovaný v hello musí být soubor App.config hello stejné jako hello obor názvů zadaný ve vašich souborech C#.
   >
   >
7. Přímo po hello `Microsoft.ServiceBus.Samples` deklaraci oboru názvů, ale v rámci hello názvů definujte nové rozhraní s názvem `IEchoContract` a použít hello `ServiceContractAttribute` rozhraní toohello atributu s hodnotou oboru názvů `http://samples.microsoft.com/ServiceModel/Relay/`. Hodnota oboru názvů Hello se liší od hello obor názvů, který používáte v rámci oboru hello kódu. Místo toho hello hodnotu oboru názvů se používá jako jedinečný identifikátor pro tento kontrakt. Zadání oboru názvů hello explicitně brání přidání názvu kontraktu toohello hello výchozí hodnoty oboru názvů. Vložte následující kód po deklaraci oboru názvů hello hello:

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > Hello obor názvů kontraktu služby obvykle obsahuje schéma pojmenování s informacemi o verzi. Hlavní změny tooisolate služeb včetně informací o verzi v oboru názvů kontraktu služby hello umožňuje definováním nové kontrakt služby s nový obor názvů a která bude vystavená na nový koncový bod. Tímto způsobem můžou klienti pokračovat toouse hello starého kontraktu služby bez nutnosti toobe aktualizovat. Informace o verzi může mít podobu data nebo čísla sestavení. Další informace najdete v článku o [Správa verzí služeb](http://go.microsoft.com/fwlink/?LinkID=180498). Pro účely tohoto kurzu hello hello pojmenování schématu oboru názvů kontraktu služby hello neobsahuje informace o verzi.
   >
   >
8. V rámci hello `IEchoContract` rozhraní, deklarujte metodu pro jednu operaci hello hello `IEchoContract` kontrakt zpřístupňuje v hello rozhraní a použít hello `OperationContractAttribute` způsob toohello atribut, který má tooexpose jako součást hello veřejné předávání WCF smluvním vztahu, následujícím způsobem:

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. Přímo po hello `IEchoContract` definici rozhraní, deklarujte kanál, který zdědí vlastnosti z obou `IEchoContract` a také toohello `IClientChannel` rozhraní, jak je vidět tady:

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanál je objekt WCF hello hello hostitele a klienty průchodu, která tooeach informace o dalších. Později napíšete kód proti hello kanál tooecho informace mezi dvěma aplikacemi hello.
10. Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** nebo stiskněte klávesu **Ctrl + Shift + B** tooconfirm hello přesnost své dosavadní práce.

### <a name="example"></a>Příklad

Hello následující kód ukazuje základní rozhraní, která definuje kontraktu WCF předávání.

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Je teď vytvořená hello rozhraní, můžete implementovat rozhraní hello.

## <a name="implement-hello-wcf-contract"></a>Implementujte kontrakt WFG hello

Vytvoření Azure předávání vyžaduje, abyste nejdřív vytvořili kontrakt hello, který se definuje pomocí rozhraní. Další informace o vytváření rozhraní hello najdete v předchozím kroku hello. dalším krokem Hello je tooimplement hello rozhraní. To zahrnuje vytvoření třídy s názvem `EchoService` hello definovaný uživatelem, která implementuje `IEchoContract` rozhraní. Po implementaci rozhraní hello nakonfigurujete hello rozhraní pomocí souboru App.config. Hello konfigurační soubor obsahuje informace nutné k hello aplikaci, například název hello hello služby, název hello hello kontraktu a typ hello protokol, který je použité toocommunicate službou předávání přes hello. Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem. Obecnější diskuzi o způsobu tooimplement služby smlouvy, najdete v části [implementace kontraktů služby](https://msdn.microsoft.com/library/ms733764.aspx) v hello dokumentaci WCF.

1. Vytvořte novou třídu s názvem `EchoService` přímo po definici hello hello `IEchoContract` rozhraní. Hello `EchoService` třída implementuje hello `IEchoContract` rozhraní.

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    Podobné implementace rozhraní tooother, definice hello můžete implementovat v jiném souboru. V tomto kurzu však hello implementace je umístěn ve stejné souboru jako definice rozhraní hello a hello hello `Main` metoda.
2. Použít hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) atribut toohello `IEchoContract` rozhraní. Určuje atribut Hello hello název služby a obor názvů. Až to uděláte, hello `EchoService` třída vypadat takto:

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. Implementace hello `Echo` metoda definované v hello `IEchoContract` rozhraní v hello `EchoService` třídy.

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. Klikněte na tlačítko **sestavení**, pak klikněte na tlačítko **sestavit řešení** tooconfirm hello přesnost své práce.

### <a name="define-hello-configuration-for-hello-service-host"></a>Definice hello konfigurace pro hostitele služby hello

1. Hello konfigurační soubor je velmi podobný konfiguračnímu souboru WCF tooa. Obsahuje hello název služby, koncový bod (tedy umístění hello, který předávání přes Azure vystaví pro klienty a hostiteli toocommunicate mezi sebou) a vazbu (typ hello protokolu, který je použité toocommunicate) hello. Hello hlavní rozdíl je, že tento nakonfigurovaný koncový bod služby odkazuje tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) vazby, který není součástí hello rozhraní .NET Framework. [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) je jedním z hello vazeb definovaných prostředím hello service.
2. V **Průzkumníku**, dvakrát klikněte na tooopen soubor App.config hello ji v editoru Visual Studio hello.
3. V hello `<appSettings>` elementu, nahraďte zástupné symboly hello hello názvem vašeho oboru názvů služby a hello SAS klíč, který jste zkopírovali v předchozím kroku.
4. V rámci hello `<system.serviceModel>` přidat značky, `<services>` elementu. V jednom konfiguračním souboru můžete definovat více aplikacích s předáváním. V tomto kurzu se ale definuje jen jedna.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. V rámci hello `<services>` elementu, přidejte `<service>` element toodefine hello název služby hello.

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. V rámci hello `<service>` elementu, definujte hello umístění hello koncového bodu kontraktu a také hello typ vazby pro koncový bod hello.

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    koncový bod Hello definuje, kde bude hello klient hledat hostitelskou aplikaci hello. Kurz hello později, používá tento krok toocreate identifikátor URI, která plně vystavuje hostitele hello prostřednictvím předávání přes Azure. Hello vazba deklaruje, že používáme TCP jako hello protokol toocommunicate službou předávání přes hello.
7. Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** tooconfirm hello přesnost své práce.

### <a name="example"></a>Příklad

Hello následující kód ukazuje implementaci kontraktu služby hello hello.

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Hello následující kód ukazuje základní formát souboru App.config hello přidružený k hostiteli služby hello hello.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-tooregister-with-hello-relay-service"></a>Hostování a spuštění tooregister základní webové služby s hello předávací služba

Tento krok popisuje, jak toorun Azure předávání služby.

### <a name="create-hello-relay-credentials"></a>Vytvoření hello předávání pověření

1. V `Main()`, vytvořte dvě proměnné v oboru názvů které toostore hello a hello SAS klíč, který se načítají z okna konzoly hello.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    klíč SAS Hello bude použít novější tooaccess projektu. obor názvů Hello se předá jako parametr příliš`CreateServiceUri` toocreate URI služby.
2. Použití [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) objektu, deklarovat, že budete používat klíč SAS jako typ přihlašovacích údajů hello. Přidejte následující kód přímo po kódu hello přidali v posledním kroku hello hello.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-hello-service"></a>Vytvoření bázové adresy pro službu hello

Po hello kódu, které jste přidali v posledním kroku hello, vytvořte `Uri` pro základní adresu hello instanci služby hello. Toto URI specifikuje schéma Service Bus hello hello obor názvů a hello cestu rozhraní služby hello.

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

"sb" je zkratka schématu Service Bus hello a určuje, že používáme protokol TCP jako protokol pro hello. To jsme předtím indikovali v konfiguračním souboru hello, pokud [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) byl zadán jako hello vazby.

V tomto kurzu hello identifikátor URI je `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="create-and-configure-hello-service-host"></a>Vytvoření a konfigurace hostitele služby hello

1. Nastavte režim připojení hello příliš`AutoDetect`.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Hello režim připojení popisuje hello protokol hello služby používá toocommunicate předávací službou hello; pomocí protokolu HTTP nebo TCP. Pomocí výchozího nastavení hello `AutoDetect`, hello služby pokusí tooconnect tooAzure předávání přes TCP, pokud je k dispozici a HTTP, pokud TCP není k dispozici. Všimněte si, že to se liší od služby hello protokolu hello specifikuje pro komunikaci klienta. Tento protokol se určuje podle hello vazby použít. Službu můžete například použít hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) vazba, která určuje, že její koncový bod s klienty komunikuje přes HTTP. Že stejné služba by mohla specifikovat **ConnectivityMode.AutoDetect** tak, aby služba hello komunikuje s Azure předávání přes protokol TCP.
2. Vytvořte hostitele služby hello, pomocí hello URI vytvořený dříve v této části.

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Hostitel služby Hello je objekt WCF hello, který vytvoří instanci služby hello. Tady jí předáte typ hello služby chcete toocreate ( `EchoService` typu) a také toohello adresu, na které má služba tooexpose hello.
3. Hello horní části souboru Program.cs hello, přidejte odkazy na příliš[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) a [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. Zpět v `Main()`, nakonfigurujte hello koncový bod tooenable veřejný přístup.

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Tento krok informuje hello předávací služba, že vaše aplikace dá veřejně najít tak, že prověří hello informačního kanálu ATOM pro váš projekt. Pokud nastavíte **DiscoveryType** příliš**privátní**, klient bude stále možné tooaccess hello služby. Ale hello by se při vyhledávání hello předávání názvů. Hello klient místo toho by měla mít cesta ke koncovému bodu hello tooknow předem.
5. Použít pověření služby hello toohello koncové body služby definované v souboru App.config hello:

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Jak jsme uvedli v předchozím kroku hello, deklarovat vám může několik služeb a koncové body v konfiguračním souboru hello. Pokud jste měli, tento kód by prošel konfigurační soubor hello a vyhledávání pro každý koncový bod toowhich platit vaše přihlašovací údaje. V tomto kurzu má hello konfigurační soubor pouze jeden koncový bod.

### <a name="open-hello-service-host"></a>Hostitel služby otevřete hello

1. Otevřete službu hello.

    ```csharp
    host.Open();
    ```
2. Informujte hello uživatele, který hello služby používá a popisují, jak tooshut dolů hello služby.

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. Po dokončení zavřete hostitele služby hello.

    ```csharp
    host.Close();
    ```
4. Stiskněte klávesu **Ctrl + Shift + B** toobuild hello projektu.

### <a name="example"></a>Příklad

Kódu dokončené služby by měl vypadat takto. Hello kód obsahuje hello kontrakt a implementaci služby z předchozích kroků v kurzu hello a hostitelé hello službu v konzolové aplikaci.

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create hello credentials object for hello endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create hello service URI based on hello service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create hello service host reading hello configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create hello ServiceRegistrySettings behavior for hello endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add hello Relay credentials tooall endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open hello service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            // Close hello service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-hello-service-contract"></a>Vytvoření klienta WCF pro kontrakt služby hello

dalším krokem Hello je toocreate klientské aplikace a definovat hello smlouvu, kterou implementujete v pozdějších krocích. Upozorňujeme, že hodně těchto kroků vypadat hello kroky použít toocreate služby: definování kontraktu, upravení App.config souboru, pomocí přihlašovacích údajů tooconnect toohello předávací služby a tak dále. Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.

1. Vytvořte nový projekt v hello aktuální řešení sady Visual Studio pro klienta hello díky hello následující:

   1. V Průzkumníku řešení v hello stejném řešení, které obsahuje službu hello, klikněte pravým tlačítkem na hello aktuální řešení (ne hello projekt) a klikněte na tlačítko **přidat**. Pak klikněte na **Nový projekt**.
   2. V hello **přidat nový projekt** dialogové okno, klikněte na tlačítko **Visual C#** (Pokud **Visual C#** nezobrazí, podívejte se do části **jiné jazyky**), vyberte hello **Konzolovou aplikaci (rozhraní .NET Framework)** šablony a pojmenujte ji **EchoClient**.
   3. Klikněte na **OK**.
      <br />
2. V Průzkumníku řešení poklikejte na soubor Program.cs hello v hello **EchoClient** projektu tooopen ji v editoru hello, pokud ještě není otevřete.
3. Změna hello název oboru názvů z výchozího názvu `EchoClient` příliš`Microsoft.ServiceBus.Samples`.
4. Nainstalujte hello [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus): v Průzkumníku řešení klikněte pravým tlačítkem na hello **EchoClient** projektu a pak klikněte na **spravovat balíčky NuGet**. Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.

    ![][3]
5. Přidat `using` příkaz pro hello [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) oboru názvů v souboru Program.cs hello.

    ```csharp
    using System.ServiceModel;
    ```
6. Přidejte hello Definice toohello obor názvů kontraktu služby, jak ukazuje následující příklad hello. Upozorňujeme, že je tato definice identické toohello definice použitá v hello **služby** projektu. Měli byste přidat tento kód hello horní části hello `Microsoft.ServiceBus.Samples` oboru názvů.

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. Stiskněte klávesu **Ctrl + Shift + B** toobuild hello klienta.

### <a name="example"></a>Příklad

Hello následující kód ukazuje aktuální stav souboru Program.cs hello hello v hello **EchoClient** projektu.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-hello-wcf-client"></a>Konfigurace klienta WCF hello

V tomto kroku vytvoříte soubor App.config pro základní klientskou aplikaci, který přistupuje k službě hello vytvořili dříve v tomto kurzu. Tento soubor App.config definuje hello kontrakt, vazbu a název koncového bodu hello. Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.

1. V Průzkumníku řešení v hello **EchoClient** projektu, klikněte dvakrát na **App.config** tooopen hello souboru v editoru Visual Studio hello.
2. V hello `<appSettings>` elementu, nahraďte zástupné symboly hello hello názvem vašeho oboru názvů služby a hello SAS klíč, který jste zkopírovali v předchozím kroku.
3. V rámci elementu system.serviceModel hello, přidejte `<client>` elementu.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Tento krok deklaruje, že definujete klientskou aplikaci ve stylu WCF.
4. V rámci hello `client` elementu, definujte hello název, kontrakt a typ vazby pro koncový bod hello.

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Tento krok definuje název hello hello koncového bodu, hello kontrakt definovaný ve hello služby a hello fakt, že klientská aplikace hello používá TCP toocommunicate s předávání přes Azure. Hello název koncového bodu se používá v hello dalším krokem toolink této konfigurace koncového bodu s URI služby hello.
5. Klikněte na tlačítko **soubor**, pak klikněte na tlačítko **Uložit vše**.

## <a name="example"></a>Příklad

Hello následující kód ukazuje soubor App.config hello pro klienta Echo hello.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-hello-wcf-client"></a>Implementace klienta WCF hello
V tomto kroku implementujete základní klientskou aplikaci, který přistupuje k hello službu, kterou jste vytvořili dříve v tomto kurzu. Podobně jako toohello služby hello klient provádí spoustu hello stejné operace tooaccess předávání přes Azure:

1. Nastaví režim připojení hello.
2. Vytvoří hello identifikátor URI, který vyhledá hello hostitele služby.
3. Definuje hello zabezpečovací pověření.
4. Platí hello pověření toohello připojení.
5. Otevře připojení hello.
6. Provádí úlohy specifické pro aplikaci hello.
7. Zavře připojení hello.

Jedním z hlavních rozdílů hello je však, že klientská aplikace hello používá službu předávání přes kanál tooconnect toohello, zatímco hello služba používá volání příliš**ServiceHost**. Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.

### <a name="implement-a-client-application"></a>Implementace klientské aplikace
1. Nastavte režim připojení hello příliš**AutoDetect**. Přidejte následující kód do hello hello `Main()` metoda hello **EchoClient** aplikace.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. Definujte proměnné toohold hello hodnoty pro hello oboru názvů služby a klíče SAS načtené z konzoly hello.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. Vytvořte hello identifikátor URI, který definuje umístění hello hello hostitele ve vašem projektu předávání.

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. Vytvoření objektu hello přihlašovací údaje pro svůj koncový bod služby oboru názvů.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. Vytváření hello kanál, který načítá hello konfigurace popsané v souboru App.config hello.

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Postup kanálu je objekt WCF, který vytvoří kanál, který komunikuje hello služby a klientské aplikace.
6. Použijte pověření hello.

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. Vytvořte a otevřete hello kanálu toohello služby.

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. Zápis hello základní uživatelské rozhraní a funkci pro hello echo.

    ```csharp
    Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Všimněte si, že kód hello používá hello instanci objektu kanálu hello jako proxy pro službu hello.
9. Zavřete kanál hello a objektu pro vytváření zavřít hello.

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a>Příklad

Dokončený kód by měly vypadat následovně, znázorňující, jak toocreate klientskou aplikaci, jak toocall hello operace služby hello a jak tooclose hello klienta po operaci hello volání je dokončena.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-hello-applications"></a>Spouštění aplikací hello

1. Stiskněte klávesu **Ctrl + Shift + B** toobuild hello řešení. Toto sestavení projektu klienta hello i hello projekt služby, který jste vytvořili v předchozích krocích hello.
2. Před spuštěné hello klientskou aplikaci musí Ujistěte se, zda je spuštěna aplikace služby hello. V Průzkumníku řešení v sadě Visual Studio, klikněte pravým tlačítkem na hello **EchoService** řešení, pak klikněte na tlačítko **vlastnosti**.
3. V hello řešení dialogové okno Vlastnosti, klikněte na **spouštěný projekt**, pak klikněte na tlačítko hello **více projektů po spuštění** tlačítko. Zajistěte, aby **EchoService** objeví jako první v seznamu hello.
4. Sada hello **akce** pole pro obě hello **EchoService** a **EchoClient** projekty příliš**spustit**.

    ![][5]
5. Klikněte na **Závislosti projektu**. V hello **projekty** vyberte **EchoClient**. V hello **závisí na** zkontrolujte, zda **EchoService** je zaškrtnuté.

    ![][6]
6. Klikněte na tlačítko **OK** toodismiss hello **vlastnosti** dialogové okno.
7. Stiskněte klávesu **F5** toorun obou projektů.
8. Obě okna konzoly otevřete a vyzvat vás k názvu oboru názvů hello. Hello služby musíte nejprve spustit, tak v hello **EchoService** okně konzoly, zadejte obor názvů hello a potom stiskněte klávesu **Enter**.
9. Pak se zobrazí výzva k zadání klíče SAS. Zadejte klíč SAS hello a stiskněte klávesu ENTER.

    Tady je příklad výstupu z okna konzoly hello. Všimněte si, že zadané hodnoty hello tady jsou například pouze pro účely.

    `Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`

    aplikace služby Hello vytiskne toohello konzoly okno hello adresu, na kterém naslouchá, jak je vidět v hello následující ukázka.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`
10. V hello **EchoClient** okně konzoly, zadejte text hello stejné informace, které jste zadali dříve pro aplikaci služby hello. Postupujte podle hello předchozí kroky tooenter hello stejný obor názvů služby a SAS klíč hodnoty pro klientské aplikace hello.
11. Po zadání těchto hodnot, hello klient otevře kanál toohello služby a vyzve jste tooenter část textu, jak je vidět v následujícím příkladu výstupu konzoly hello.

    `Enter text tooecho (or [Enter] tooexit):`

    Zadejte některé aplikace služby toohello toosend text a stiskněte klávesu Enter. Tento text je odeslán toohello služby prostřednictvím hello operace služby Echo a zobrazí se v okně konzoly služby hello jako následující příklad výstupu hello.

    `Echoing: My sample text`

    Hello klientská aplikace obdrží hello návratová hodnota hello `Echo` operaci, která je původní text hello který se vypíše tooits okně konzoly. Hello následuje příklad výstupu z okna konzoly klienta hello.

    `Server echoed: My sample text`
12. Můžete pokračovat v odesílání zpráv ze služby toohello klienta hello tímto způsobem. Jakmile budete hotovi, stiskněte klávesu Enter v hello klientem a službou konzoly windows tooend obě aplikace.

## <a name="next-steps"></a>Další kroky

Tento kurz vám ukázal, jak toobuild klienta aplikace Azure předávání a služby pomocí hello možnosti WCF předávání přes Service Bus. Podobný kurz, který používá [zasílání zpráv Service Bus](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), najdete v části [začít pracovat s fronty Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

toolearn Další informace o předávání přes Azure, najdete v následujících tématech hello.

* [Přehled architektury služby Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [Přehled služby Azure Relay](relay-what-is-it.md)
* [Jak toouse hello WCF předávání služby pomocí rozhraní .NET](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
