---
title: "aaaAzure WCF předávání hybridní lokální/Cloudová aplikace (.NET) | Microsoft Docs"
description: "Zjistěte, jak toocreate .NET lokální/Cloudová hybridní aplikace pomocí Azure WCF předávání."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: sethm
ms.openlocfilehash: aab8b1dbdc85c4edf7b0ccef0921b69524b2d306
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a>Hybridní místní/cloudová aplikace .NET využívající Azure WCF Relay
## <a name="introduction"></a>Úvod

Tento článek ukazuje, jak toobuild hybridní cloudové aplikace s Microsoft Azure a Visual Studio. Hello kurz předpokládá, že nemáte žádné předchozí zkušenosti s používáním Azure. Za méně než 30 minut budete mít aplikaci, která se používá několik prostředků Azure a spouštění v cloudu hello.

Co se dozvíte:

* Jak toocreate nebo přizpůsobit existující webovou službu pro spotřebu webovým řešením.
* Jak toouse hello Azure WCF předávací služby tooshare dat mezi aplikací Azure a webovou službu hostovanou jinde.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a>Jak Azure Relay pomáhá s hybridními řešeními

Podniková řešení se obvykle skládají z kombinace vlastního kódu napsaného tootackle nových a jedinečných obchodních požadavků a stávajících funkcí poskytovaných řešeními a systémy, které jsou již v místní.

Řešení architekty začínáte toouse hello cloud pro snazší zpracování požadavků škálování a nižších provozních nákladů. Při tom najdou, že existující prostředky služeb mu tooleverage jako stavební bloky pro svá řešení jsou podnikovým firewallem hello a mimo snadno spojit pro přístup k hello cloudové řešení. Spousta interních služeb nejsou vytvořené nebo hostovaná tak, aby se dala snadno vystavit na hranici podnikové sítě hello.

[Předávání přes Azure](https://azure.microsoft.com/services/service-bus/) je určená pro hello případ použití vzít existující webové služby Windows Communication Foundation (WCF) a bezpečně přístupné toosolutions nacházející se mimo firemní zónu hello bez nutnosti publikování těchto služeb. nežádoucí změny toohello podnikové síťové infrastruktury. Takové služby předávání přes se stále hostují uvnitř existujícího prostředí, ale delegují čekání příchozí relace a požadavky toohello služby předávání hostovaných v cloudu. Azure Relay taky takové služby chrání před neoprávněným přístupem pomocí ověření [Sdíleným přístupovým podpisem](../service-bus-messaging/service-bus-sas.md) (SAS).

## <a name="solution-scenario"></a>Scénář řešení
V tomto kurzu vytvoříte webu ASP.NET, která umožňuje toosee seznam produktů na stránce inventáře produktů hello.

![][0]

Hello kurz předpokládá, že máte produktové informace dostupné v existující místní systém a používá předávání přes Azure tooreach do takového systému. To se simuluje jako webová služba, která spustí jednoduchou konzolovou aplikaci a využívá sadu produktů uloženou v paměti. Bude se mít toorun této konzolové aplikace ve vašem počítači a nasazení hello webové role do Azure. Díky tomu se zobrazí jak hello webová role běžící v hello datovém centru Azure skutečně zavolá do vašeho počítače, i když váš počítač skoro určitě nachází za nejméně jedním firewallem a vrstvou sítě adres překlad (NAT).

## <a name="set-up-hello-development-environment"></a>Nastavit hello vývojového prostředí

Před zahájením vývojem aplikací pro Azure, stáhnout hello nástroje a nastavení vývojového prostředí:

1. Nainstalujte hello Azure SDK pro .NET ze hello SDK [položky ke stažení](https://azure.microsoft.com/downloads/).
2. V hello **.NET** sloupce, klikněte na tlačítko hello verzi [Visual Studio](http://www.visualstudio.com) používáte. Hello kroky v tomto kurzu Visual Studio 2015, ale také pracovat s Visual Studio 2017.
3. Po zobrazení výzvy toorun nebo uložení instalačního programu hello, klikněte na tlačítko **spustit**.
4. V hello **instalačního programu webové platformy**, klikněte na tlačítko **nainstalovat** a pokračovat v instalaci hello.
5. Po dokončení instalace hello budete mít všechno potřebné toostart toodevelop hello aplikace. Hello SDK obsahuje nástroje, které vám umožní snadno vyvíjet aplikace Azure v sadě Visual Studio.

## <a name="create-a-namespace"></a>Vytvoření oboru názvů

pomocí toobegin hello funkce předávání v Azure, musíte nejdřív vytvořit obor názvů služby. Obor názvů poskytuje kontejner oboru pro adresování prostředků Azure v rámci vaší aplikace. Postupujte podle hello [pokynů tady](relay-create-namespace-portal.md) toocreate předávání názvů.

## <a name="create-an-on-premises-server"></a>Vytvoření lokálního serveru

Nejdřív vytvoříte lokální (testovací) systém katalogu produktů. Bude vcelku jednoduchý; Zobrazí se to jako představující skutečný lokální systém katalogu produktu s kompletní rovinou služeb pokoušíme toointegrate.

Tento projekt je konzolová aplikace z Visual Studia a pomocí hello [balíček Azure Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello konfiguračních nastavení a knihovny Service Bus.

### <a name="create-hello-project"></a>Vytvoření projektu hello

1. Spusťte Visual Studio s právy správce. Ano, klikněte pravým tlačítkem na ikonu programu sady Visual Studio hello toodo a pak klikněte na **spustit jako správce**.
2. V sadě Visual Studio na hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.
3. V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Konzolová aplikace (.NET Framework)**. V hello **název** pole, název typu hello **ProductsServer**:

   ![][11]
4. Klikněte na tlačítko **OK** toocreate hello **ProductsServer** projektu.
5. Pokud jste již nainstalovali hello Správce balíčků NuGet pro Visual Studio, toohello další krok přeskočte. Pokud ne, přejděte na [NuGet][NuGet] a klikněte na [Nainstalovat NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Postupujte podle Správce balíčků NuGet hello tooinstall hello výzvy a potom znovu spusťte Visual Studio.
6. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsServer** projektu a pak klikněte na **spravovat balíčky NuGet**.
7. Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Vyberte hello **WindowsAzure.ServiceBus** balíčku.
8. Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.

   ![][13]

   Všimněte si, že hello vyžaduje, že teď jsou reference na sestavení klienta.
8. Přidejte novou třídu pro váš produktový kontrakt. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsServer** projektu a klikněte na tlačítko **přidat**a potom klikněte na **třída**.
9. V hello **název** pole, název typu hello **ProductsContract.cs**. Pak klikněte na **Přidat**.
10. V **ProductsContract.cs**, nahraďte definici oboru názvů hello hello následující kód, který definuje kontrakt hello služby hello.

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define hello data contract for hello service
        [DataContract]
        // Declare hello serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define hello service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```
11. V souboru Program.cs místo definice oboru názvů hello hello následující kód, který přidá službu profilu hello a hello hosta pro ni.

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement hello IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in hello service console application
            // when hello list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define hello Main() function in hello service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER tooclose");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. V Průzkumníku řešení klikněte dvakrát na hello **App.config** souboru tooopen ji v editoru Visual Studio hello. Na konci hello hello `<system.ServiceModel>` – element (ale stále ještě v `<system.ServiceModel>`), přidejte následující kód XML hello. Být jisti tooreplace *yourServiceNamespace* s hello název vašeho oboru názvů, a *yourKey* s hello SAS klíč, který jste předtím získali z portálu hello:

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
13. Stále v souboru App.config v hello `<appSettings>` elementu, nahraďte hello připojení řetězcovou hodnotu s jste předtím získali z portálu hello hello připojovací řetězec.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. Stiskněte klávesu **Ctrl + Shift + B** nebo z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** toobuild hello aplikace a ověřte hello přesnost své dosavadní práce.

## <a name="create-an-aspnet-application"></a>Vytvoření aplikace ASP.NET

V této části sestavíte jednoduchou aplikaci ASP.NET, která zobrazí data načtená z vaší produktové služby.

### <a name="create-hello-project"></a>Vytvoření projektu hello

1. Zkontrolkujte, že je Visual Studio spuštěné s právy správce.
2. V sadě Visual Studio na hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.
3. V **Nainstalovaných šablonách** v části **Visual C#**, klikněte na **Webová aplikace ASP.NET (.NET Framework)**. Název projektu hello **ProductsPortal**. Pak klikněte na **OK**.

   ![][15]

4. Z hello **šablony ASP.NET** seznamu v hello **nové webové aplikace ASP.NET** dialogové okno, klikněte na tlačítko **MVC**.

   ![][16]

6. Klikněte na tlačítko hello **změna ověřování** tlačítko. V hello **změna ověřování** dialogové okno pole, ujistěte se, že **bez ověřování** je vybrána a potom klikněte na **OK**. V tomto kurzu nasazujete aplikace, která nepotřebuje přihlášení uživatele.

    ![][18]

7. Zpět v hello **nové webové aplikace ASP.NET** dialogové okno, klikněte na tlačítko **OK** aplikace MVC toocreate hello.
8. Teď musíte nakonfigurovat prostředky Azure pro novou webovou aplikaci. Postupujte podle kroků hello v hello [publikování tooAzure části tohoto článku](../app-service-web/app-service-web-get-started-dotnet.md). Potom vraťte toothis kurzu a pokračujte dalším krokem toohello.
10. V Průzkumníkovi řešení klikněte pravým tlačítkem na **Modely**, pak levým na **Přidat** a pak na **Třída**. V hello **název** pole, název typu hello **Product.cs**. Pak klikněte na **Přidat**.

    ![][17]

### <a name="modify-hello-web-application"></a>Úprava hello webové aplikace

1. V souboru Product.cs hello v sadě Visual Studio nahraďte existující definici oboru názvů hello hello následující kód.

   ```csharp
    // Declare properties for hello products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```
2. V Průzkumníku řešení rozbalte hello **řadiče** složky, dvakrát hello **HomeController.cs** souboru tooopen ho v sadě Visual Studio.
3. V **HomeController.cs**, nahraďte existující definici oboru názvů hello hello následující kód.

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of hello products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. V Průzkumníku řešení rozbalte složku Views\Shared hello, pak poklikejte na **_Layout.cshtml** tooopen ji v editoru Visual Studio hello.
5. Změňte všechny výskyty **Moje aplikace technologie ASP.NET** příliš**LITWARE's Products**.
6. Odebrat hello **Domů**, **o**, a **kontaktujte** odkazy. V následujícím příkladu hello odstraňte hello zvýrazněná kód.

    ![][41]

7. V Průzkumníku řešení rozbalte složku Views\Home hello, pak poklikejte na **Index.cshtml** tooopen ji v editoru Visual Studio hello. Nahraďte hello celý obsah souboru hello hello následující kód.

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```
8. tooverify hello přesnost své dosavadní práce, stiskněte **Ctrl + Shift + B** toobuild hello projektu.

### <a name="run-hello-app-locally"></a>Místní spuštění aplikace hello

Spusťte tooverify hello aplikace, která funguje.

1. Ujistěte se, že **ProductsPortal** je hello aktivního projektu. Název projektu hello v Průzkumníku řešení klikněte pravým tlačítkem a vyberte **nastavit jako spouštěný projekt**.
2. V sadě Visual Studio stiskněte **F5**.
3. Měla by se objevit vaše aplikace, jak běží v prohlížeči.

   ![][21]

## <a name="put-hello-pieces-together"></a>Připravili částí hello

dalším krokem Hello je toohook hello místní produkty server s hello aplikace ASP.NET.

1. Pokud již není otevřený, v sadě Visual Studio znovu otevřete hello **ProductsPortal** projektu, které jste vytvořili v hello [vytvořit aplikaci ASP.NET](#create-an-aspnet-application) části.
2. Podobně jako toohello krok v části "Vytvoření místnímu serveru" hello, přidání odkazů projektu toohello balíček NuGet hello. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** projektu a pak klikněte na **spravovat balíčky NuGet**.
3. Vyhledejte "Service Bus" a vyberte hello **WindowsAzure.ServiceBus** položky. Potom dokončete hello instalace a zavřete toto dialogové okno.
4. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** projektu a pak klikněte na **přidat**, pak **existující položka**.
5. Přejděte toohello **ProductsContract.cs** soubor z hello **ProductsServer** projekt konzoly. Klikněte na tlačítko toohighlight ProductsContract.cs. Klikněte na tlačítko hello šipka dolů vedle příliš**přidat**, pak klikněte na tlačítko **přidat jako odkaz**.

   ![][24]

6. Nyní otevřete hello **HomeController.cs** souboru v editoru Visual Studio hello a nahraďte definici oboru názvů hello hello následující kód. Být jisti tooreplace *yourServiceNamespace* s hello názvem vašeho oboru názvů služby a *yourKey* váš SAS klíč. Tato akce povolí hello klienta toocall hello místní službu a vrátit výsledek volání hello hello.

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare hello channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of hello products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** řešení (zkontrolujte že tooright kliknutím hello řešení, není hello projektu). Klikněte na **Přidat** a pak klikněte na **Existující projekt**.
8. Přejděte toohello **ProductsServer** projekt, pak poklikejte na hello **ProductsServer.csproj** tooadd soubor řešení ho.
9. **ProductsServer** musí být spuštěn v pořadí toodisplay hello data v **ProductsPortal**. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** řešení a klikněte na tlačítko **vlastnosti**. Hello **stránky vlastností** se zobrazí dialogové okno.
10. Na levé straně hello, klikněte na tlačítko **spouštěný projekt**. Na pravé straně hello, klikněte na tlačítko **více projektů po spuštění**. Ujistěte se, že **ProductsServer** a **ProductsPortal** zobrazí v tomto pořadí, s **spustit** nastavit jako hello akci pro oba.

      ![][25]

11. Stále v hello **vlastnosti** dialogové okno, klikněte na tlačítko **závislosti projektu** na levé straně hello.
12. V hello **projekty** seznamu, klikněte na tlačítko **ProductsServer**. Zkontrolujte, že **ProductsPortal** není vybraný.
13. V hello **projekty** seznamu, klikněte na tlačítko **ProductsPortal**. Zkontrolujte, že je **ProductsServer** vybraný.

    ![][26]

14. Klikněte na tlačítko **OK** v hello **stránky vlastností** dialogové okno.

## <a name="run-hello-project-locally"></a>Místní spuštění projektu hello

tootest hello místně, aplikace v sadě Visual Studio klávesu **F5**. místní server Hello (**ProductsServer**) musí nejdřív spustit a pak hello **ProductsPortal** v okně prohlížeče měla spustit aplikace. Tentokrát uvidíte, že tento hello inventář produktů zobrazí seznam dat načtených z hello produktu služby v místním systému.

![][10]

Stiskněte klávesu **aktualizovat** na hello **ProductsPortal** stránky. Pokaždé, když obnovíte stránku hello, zobrazí se aplikace server hello zobrazí zpráva při `GetProducts()` z **ProductsServer** je volána.

Zavřete obě aplikace před pokračováním toohello další krok.

## <a name="deploy-hello-productsportal-project-tooan-azure-web-app"></a>Nasazení hello ProductsPortal projektu tooan Azure webové aplikace

dalším krokem Hello je toorepublish hello Azure webové aplikace **ProductsPortal** front-endu. Hello následující:

1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** projektu a klikněte na tlačítko **publikovat**. Potom klikněte na **publikovat** na hello **publikovat** stránky.

  > [!NOTE]
  > Mohou se zobrazit chybová zpráva v okně prohlížeče hello při hello **ProductsPortal** automatickém spuštění webového projektu po nasazení hello. To je v pořádku a dochází hello **ProductsServer** aplikace ještě neběží.
>
>

2. Kopírování hello URL hello nasadit webové aplikace, budete potřebovat adresu URL hello v dalším kroku hello. Tuto adresu URL můžete získat i z okna aktivita služby Azure App Service hello v sadě Visual Studio:

  ![][9]

3. Zavřete hello prohlížeče okno toostop hello spuštění aplikace.

### <a name="set-productsportal-as-web-app"></a>Nastavení ProductsPortal jako webové aplikace

Před běžící hello aplikaci v cloudu hello, musíte zajistit, aby **ProductsPortal** spustí z Visual Studia jak webová aplikace.

1. V sadě Visual Studio, klikněte pravým tlačítkem na hello **ProductsPortal** projektu a pak klikněte na **vlastnosti**.
2. V levém sloupci hello, klikněte na **webové**.
3. V hello **spustit akci** klikněte na tlačítko hello **spustit adresu URL** tlačítko a hello textového pole zadejte hello URL dříve nasazené webové aplikace, například `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

4. Z hello **soubor** nabídky v sadě Visual Studio, klikněte na tlačítko **Uložit vše**.
5. V nabídce hello sestavení v sadě Visual Studio, klikněte na tlačítko **znovu sestavit řešení**.

## <a name="run-hello-application"></a>Spuštění aplikace hello

1. Stiskněte klávesu F5 toobuild a spuštění aplikace hello. místní server Hello (hello **ProductsServer** Konzolová aplikace) musí nejdřív spustit a pak hello **ProductsPortal** v okně prohlížeče měla spustit aplikace, jak je znázorněno v následující obrazovce hello snímek. Všimněte si znovu, že hello inventář produktů zobrazí seznam dat načtených z hello produktu služby v místním systému a tato data zobrazí ve webové aplikaci hello. Zkontrolujte toomake URL hello se, který **ProductsPortal** běží v cloudu hello jako webové aplikace Azure.

   ![][1]

   > [!IMPORTANT]
   > Hello **ProductsServer** konzolové aplikace a musí být spuštěná schopná tooserve hello data toohello **ProductsPortal** aplikace. Pokud se hello prohlížeč zobrazí chybu, počkejte několik dalších sekund **ProductsServer** tooload a zobrazení hello následující zpráva. Stiskněte klávesu **aktualizovat** v prohlížeči hello.
   >
   >

   ![][37]
2. Zpět v prohlížeči hello, stiskněte klávesu **aktualizovat** na hello **ProductsPortal** stránky. Pokaždé, když obnovíte stránku hello, zobrazí se aplikace server hello zobrazí zpráva při `GetProducts()` z **ProductsServer** je volána.

    ![][38]

## <a name="next-steps"></a>Další kroky

toolearn Další informace o předávání přes Azure, najdete v části hello následující prostředky:  

* [Co je Azure Relay?](relay-what-is-it.md)  
* [Jak toouse předávání](service-bus-dotnet-how-to-use-relay.md)  

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[NuGet]: http://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png
