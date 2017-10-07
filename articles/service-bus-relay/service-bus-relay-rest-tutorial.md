---
title: "kurz REST sběrnice aaaService pomocí předávání přes Azure | Microsoft Docs"
description: "Vytvořit jednoduchou aplikaci Azure Service Bus relay hostitele, která vystavuje rozhraní založené na REST."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2017
ms.author: sethm
ms.openlocfilehash: b68650993a0390e7cef891ccb4236095cd86d4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a>Kurz pro Azure předávání přes REST WCF

Tento kurz popisuje, jak toobuild jednoduché předávání přes Azure hostování aplikace, která vystavuje rozhraní založené na REST. Umožňuje REST webovému klientovi, například webový prohlížeč, hello tooaccess požadavky API pro Service Bus pomocí protokolu HTTP.

kurz Hello používá hello Windows Communication Foundation (WCF) REST programovací model tooconstruct služby v Service Bus. Další informace najdete v tématu [programovací Model REST WCF](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) a [návrh a implementace služeb](/dotnet/framework/wcf/designing-and-implementing-services) v hello dokumentaci WCF.

## <a name="step-1-create-a-namespace"></a>Krok 1: Vytvoření oboru názvů

pomocí toobegin hello funkce předávání v Azure, musíte nejdřív vytvořit obor názvů služby. Obor názvů poskytuje kontejner oboru pro adresování prostředků Azure v rámci vaší aplikace. Postupujte podle hello [pokynů tady](relay-create-namespace-portal.md) toocreate předávání názvů.

## <a name="step-2-define-a-rest-based-wcf-service-contract-toouse-with-azure-relay"></a>Krok 2: Definování WCF na bázi REST služby kontrakt toouse s předávání přes Azure

Při vytváření služby WCF stylu REST, je nutné definovat kontrakt hello. Hello kontrakt Určuje, jakým operacím hello hostitel podporuje. Operaci služby se můžeme představit jako metodu webové služby. Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic. Každá metoda v rozhraní hello odpovídá tooa konkrétní operaci služby. Hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atributu musí být použité tooeach rozhraní a hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) atribut musí být použité tooeach operaci. Pokud metoda v rozhraní, které má hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nemá hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), taková metoda se nevystaví. Hello kód použitý pro tyto úlohy se zobrazí v příkladu hello hello postupem.

Hello základní rozdíl mezi kontraktu WCF a kontraktu ve stylu REST je přidání hello vlastnost toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Tato vlastnost umožňuje toomap metoda v rozhraní tooa metodu na hello druhé straně rozhraní hello. V tomto případě použijeme [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink tooHTTP metoda GET. To umožňuje Service Bus tooaccurately získat a interpretovat příkazy odeslané toohello rozhraní.

### <a name="toocreate-a-contract-with-an-interface"></a>toocreate kontraktu s rozhraním

1. Otevřete Visual Studio jako správce: programu hello klikněte pravým tlačítkem v hello **spustit** nabídce a pak klikněte na tlačítko **spustit jako správce**.
2. Vytvořte nový projekt konzolové aplikace. Klikněte na tlačítko hello **soubor** nabídku a vyberte **nový**, pak vyberte **projektu**. V hello **nový projekt** dialogové okno, klikněte na tlačítko **Visual C#**, vyberte hello **konzolové aplikace** šablony a pojmenujte ji **ImageListener**. Použít výchozí hello **umístění**. Klikněte na tlačítko **OK** toocreate hello projektu.
3. Visual Studio pro projekt C# vytvoří soubor `Program.cs`. Tato třída obsahuje prázdnou `Main()` metoda, vyžaduje se pro toobuild projektu na konzole aplikace správně.
4. Přidání odkazů tooService sběrnice a **System.ServiceModel.dll** toohello projektu instalace balíčku Service Bus NuGet hello. Tento balíček automaticky přidá Reference knihovny Service Bus toohello, jakož i hello WCF **System.ServiceModel**. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ImageListener** projektu a pak klikněte na **spravovat balíčky NuGet**. Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.
5. Musíte explicitně přidat odkaz na příliš**System.ServiceModel.Web.dll** toohello projektu:
   
    a. V Průzkumníku řešení klikněte pravým tlačítkem na hello **odkazy** ve složce složky hello projektu a pak klikněte na tlačítko **přidat odkaz na**.
   
    b. V hello **přidat odkaz na** dialogovém okně klikněte na hello **Framework** karty na levé straně hello a v hello **vyhledávání** zadejte **System.ServiceModel.Web** . Vyberte hello **System.ServiceModel.Web** zaškrtněte políčko a potom klikněte na **OK**.
6. Přidejte následující hello `using` příkazy hello horní části souboru Program.cs hello.
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je hello obor názvů, který umožňuje programový přístup toobasic funkcím WCF. Předávání WCF používá řadu hello objekty a atributy kontrakty toodefine služeb WCF. Tento obor názvů budete používat ve většině aplikací předávání. Podobně [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) pomáhá definovat kanál hello, což je hello objekt, přes který komunikujete s webovým prohlížečem, předávání přes Azure a hello klienta. Nakonec [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) obsahuje hello typy, které umožňují toocreate webových aplikací.
7. Přejmenujte hello `ImageListener` obor názvů příliš**Microsoft.ServiceBus.Samples**.
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. Přímo po otevření složené závorky deklarace oboru názvů hello hello, definujte nové rozhraní s názvem **IImageContract** a použít hello **ServiceContractAttribute** atribut toohello rozhraní s Hodnota `http://samples.microsoft.com/ServiceModel/Relay/`. Hodnota oboru názvů Hello se liší od hello obor názvů, který používáte v rámci oboru hello kódu. Hodnota oboru názvů Hello se používá jako jedinečný identifikátor pro tento kontrakt a musí mít informace o verzi. Další informace najdete v článku o [Správa verzí služeb](http://go.microsoft.com/fwlink/?LinkID=180498). Zadání oboru názvů hello explicitně brání přidání názvu kontraktu toohello hello výchozí hodnoty oboru názvů.
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. V rámci hello `IImageContract` rozhraní, deklarujte metodu pro jednu operaci hello hello `IImageContract` kontrakt zpřístupňuje v hello rozhraní a použít hello `OperationContractAttribute` atribut toohello metodu chcete tooexpose jako součást hello veřejné Service Bus kontrakt.
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. V hello **OperationContract** atribut, přidejte hello **WebGet** hodnotu.
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    V tom umožňuje hello tooroute služby předávání přes požadavky HTTP GET příliš`GetImage`a vrátit hodnoty hello tootranslate `GetImage` do odpovědi HTTP GETRESPONSE. Později v kurzu hello použijete webové prohlížeče tooaccess tato metoda a toodisplay hello image v prohlížeči hello.
11. Přímo po hello `IImageContract` definice deklarujte kanál, který dědí z obou hello `IImageContract` a `IClientChannel` rozhraní.
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    Kanál je objekt WCF hello klienta a služby hello průchodu, která tooeach informace o dalších. Později vytvoříte kanál hello ve své hostitelské aplikaci. Předávání přes Azure pak použije tento kanál toopass hello požadavky HTTP GET z prohlížeče tooyour hello **GetImage** implementace. předávání Hello používá také hello kanál tootake hello **GetImage** vrátit hodnotu a přeloží ji do HTTP GETRESPONSE pro hello prohlížeče klienta.
12. Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** tooconfirm hello přesnost své dosavadní práce.

### <a name="example"></a>Příklad
Hello následující kód ukazuje základní rozhraní, která definuje kontraktu WCF předávání.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-toouse-service-bus"></a>Krok 3: Implementace toouse kontraktu služby WCF na bázi REST Service Bus
Vytváření stylu REST WCF předávací služba vyžaduje, abyste nejdřív vytvořili kontrakt hello, který se definuje pomocí rozhraní. dalším krokem Hello je tooimplement hello rozhraní. To zahrnuje vytvoření třídy s názvem **ImageService** hello definovaný uživatelem, která implementuje **IImageContract** rozhraní. Po implementaci kontraktu hello nakonfigurujete hello rozhraní pomocí souboru App.config. Hello konfigurační soubor obsahuje informace nutné k hello aplikaci, například název hello hello služby, název hello hello kontraktu a typ hello protokol, který je použité toocommunicate službou předávání přes hello. Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.

Stejně jako u předchozí kroky hello, je velmi malý rozdíl mezi implementací kontraktu ve stylu REST a kontraktu WCF předávání.

### <a name="tooimplement-a-rest-style-service-bus-contract"></a>kontraktu Service Bus tooimplement stylu REST
1. Vytvořte novou třídu s názvem **ImageService** přímo po definici hello hello **IImageContract** rozhraní. Hello **ImageService** třída implementuje hello **IImageContract** rozhraní.
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    Podobné implementace rozhraní tooother, definice hello můžete implementovat v jiném souboru. Ale pro účely tohoto kurzu hello implementace objeví ve hello stejný soubor jako definice rozhraní hello a `Main()` metoda.
2. Použít hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) atribut toohello **IImageService** tooindicate třída, která hello třída je implementace kontraktu WCF.
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    Jak jsme už zmínili, tento obor názvů není obvyklý obor názvů. Místo toho je součástí architektury WCF, která identifikuje kontrakt hello hello. Další informace najdete v tématu hello [názvy datových kontraktů](https://msdn.microsoft.com/library/ms731045.aspx) téma v hello dokumentaci WCF.
3. Přidáte projekt tooyour .jpg bitové kopie.  
   
    To je obrázek, který hello služba zobrazí ve hello přijetí prohlížeče. Klikněte pravým tlačítkem na projekt a klikněte na **Přidat**. Pak klikněte na **Existující položka**. Použití hello **přidat existující položku** dialogové okno pole toobrowse tooan vhodné .jpg a pak klikněte na **přidat**.
   
    Když přidáváte soubor hello, ujistěte se, že **všechny soubory** je vybraný v hello rozevíracího seznamu další toohello **název souboru:** pole. Hello zbytek tohoto kurzu se předpokládá, že hello název obrázku hello je "image.jpg". Pokud máte jiný soubor, bude toorename hello image obsahují, nebo změňte toocompensate vašeho kódu.
4. toomake se, že hello službou můžete najít v hello soubor obrázku, **Průzkumníku řešení** klikněte pravým tlačítkem na soubor bitové kopie hello a pak klikněte na **vlastnosti**. V hello **vlastnosti** podokně nastavit **zkopírujte tooOutput Directory** příliš**kopírovat, pokud je novější**.
5. Přidat odkaz na toohello **System.Drawing.dll** toohello sestavení projektu a také přidat následující hello přidružené `using` příkazy.  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. V hello **ImageService** třídy, přidejte text hello následující konstruktor, zatížení hello rastrového obrázku a připraví toosend ho toohello prohlížeče klienta.
   
    ```csharp
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```
7. Přímo za předchozí kód hello přidejte následující hello **GetImage** metoda v hello **ImageService** třídy tooreturn HTTP zprávu, která obsahuje bitovou kopii hello.
   
    ```csharp
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```
   
    Tato implementace používá **MemoryStream** tooretrieve hello obrázku a jeho přípravu pro streamování toohello prohlížeče. Spustí pozici přenosu hello na nule, deklaruje hello obsah přenosu jako jpeg a datové proudy hello informace.
8. Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení**.

### <a name="toodefine-hello-configuration-for-running-hello-web-service-on-service-bus"></a>Konfigurace hello toodefine pro spuštění hello webové služby v Service Bus
1. V **Průzkumníku řešení**, dvakrát klikněte na **App.config** tooopen ji v editoru Visual Studio hello.
   
    Hello **App.config** soubor obsahuje hello název služby, koncový bod (tj. umístění hello předávání přes Azure vystaví pro klienty a hostiteli toocommunicate mezi sebou) a vazbu (typ hello protokolu, který je použité toocommunicate). Hello hlavní rozdíl je tento koncový bod služby hello nakonfigurované odkazuje tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) vazby.
2. Hello `<system.serviceModel>` – element XML je element WCF, který definuje jednu nebo více služeb. Zde je název služby používané toodefine hello a koncového bodu. Na konci hello hello `<system.serviceModel>` element (ale stále ještě v `<system.serviceModel>`), přidejte `<bindings>` element, který má hello následující obsah. Definuje hello vazbu použitou v aplikaci hello. Můžete definovat víc vazeb, ale v tomto kurzu se definuje jen jedna.
   
    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```
   
    Hello předchozí kód definuje WCF předávání [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) s **relayClientAuthenticationType** nastavit příliš**žádné**. Toto nastavení naznačuje, že koncový bod, který používá tuto vazbu, nepotřebuje pověření klienta.
3. Po hello `<bindings>` elementu, přidejte `<services>` elementu. Podobné toohello vazby v jednom konfiguračním souboru můžete definovat více služeb. V tomto kurzu ale definujete jen jednu.
   
    ```xml
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```
   
    Tento krok konfiguruje službu, která používá výchozí hello předtím definovaný **webHttpRelayBinding**. Taky používá výchozí hello **sbTokenProvider**, která je definována v dalším kroku hello.
4. Po hello `<services>` elementu, vytvoření `<behaviors>` element s hello následující obsah, nahraďte "SAS_KEY" hello *sdíleného přístupového podpisu* klíč (SAS), které jste předtím získali z hello [portálu Azure ][Azure portal].
   
    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```
5. Stále v souboru App.config v hello `<appSettings>` elementu, nahraďte hello celé připojení řetězcovou hodnotu s jste předtím získali z portálu hello hello připojovací řetězec. 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** toobuild hello celé řešení.

### <a name="example"></a>Příklad
Hello následující kód ukazuje implementaci kontraktu a služby hello pro nějakou službu založenou na REST, která běží na Service Bus pomocí hello **WebHttpRelayBinding** vazby.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Hello následující příklad ukazuje soubor App.config hello související se službou hello.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove hello ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey="YOUR_SAS_KEY"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-hello-rest-based-wcf-service-toouse-azure-relay"></a>Krok 4: Hostování toouse služby WCF na bázi REST hello předávání přes Azure
Tento krok popisuje, jak toorun webové služby WCF předávání pomocí konzolové aplikace. Úplný seznam všech hello kód napsaný v tomto kroku najdete v příkladu hello hello postupem.

### <a name="toocreate-a-base-address-for-hello-service"></a>toocreate základní adresa služby hello
1. V hello `Main()` deklaraci funkce, vytvořit obor názvů hello proměnné toostore projektu. Ujistěte se, že tooreplace `yourNamespace` s hello název oboru názvů předávání hello, které jste předtím vytvořili.
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    Service Bus používá hello název vašeho oboru názvů toocreate jedinečný identifikátor URI.
2. Vytvoření `Uri` instance pro základní adresu hello hello služby, který je založen na obor názvů hello.
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="toocreate-and-configure-hello-web-service-host"></a>toocreate a konfigurace hostitele webové služby hello
* Vytvořte hello hostitele webové služby pomocí adresy URI hello vytvořili dříve v této části.
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Hostitel služby Hello je objekt WCF hello, který instancuje hostitelskou aplikaci hello. Tento příklad jí předá typ hello hostitele chcete toocreate ( **ImageService**), a také hello adresu, na které chcete tooexpose hello hostitelskou aplikaci.

### <a name="toorun-hello-web-service-host"></a>hostitele toorun hello webové služby
1. Otevřete službu hello.
   
    ```csharp
    host.Open();
    ```
    Hello služba je nyní spuštěna.
2. Zobrazí se zpráva s oznámením, že je spuštěna služba hello a jak toostop hello služby.
   
    ```csharp
    Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. Po dokončení zavřete hostitele služby hello.
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a>Příklad
Následující ukázka Hello zahrnuje hello kontrakt a implementaci služby z předchozích kroků v hello kurzu a hostitele služby hello v konzolové aplikaci. Zkompilujte následující kód do spustitelného souboru s názvem ImageListener.exe hello.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-hello-code"></a>Kompilování kódu hello
Po sestavení řešení hello, hello následující toorun hello aplikace:

1. Stiskněte klávesu **F5**, nebo vyhledejte umístění toohello spustitelného souboru (ImageListener\bin\Debug\ImageListener.exe), služba toorun hello. Zachovat hello aplikace spuštěna, protože je to požadováno, dalším krokem tooperform hello.
2. Zkopírujte a vložte adresu hello z příkazového řádku hello Image hello toosee prohlížeče.
3. Když skončíte, stiskněte **Enter** v aplikace hello tooclose v okně příkazového řádku hello.

## <a name="next-steps"></a>Další kroky
Teď, když jste sestavili aplikaci, která používá předávací službu Service Bus hello, najdete v části hello následující další články toolearn o předávání přes Azure:

* [Přehled architektury služby Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [Přehled služby Azure Relay](relay-what-is-it.md)
* [Jak toouse hello WCF předávání služby pomocí rozhraní .NET](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
