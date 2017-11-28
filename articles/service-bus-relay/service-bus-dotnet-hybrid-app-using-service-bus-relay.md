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
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a><span data-ttu-id="c8dd2-103">Hybridní místní/cloudová aplikace .NET využívající Azure WCF Relay</span><span class="sxs-lookup"><span data-stu-id="c8dd2-103">.NET on-premises/cloud hybrid application using Azure WCF Relay</span></span>
## <a name="introduction"></a><span data-ttu-id="c8dd2-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="c8dd2-104">Introduction</span></span>

<span data-ttu-id="c8dd2-105">Tento článek ukazuje, jak toobuild hybridní cloudové aplikace s Microsoft Azure a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-105">This article shows how toobuild a hybrid cloud application with Microsoft Azure and Visual Studio.</span></span> <span data-ttu-id="c8dd2-106">Hello kurz předpokládá, že nemáte žádné předchozí zkušenosti s používáním Azure.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-106">hello tutorial assumes you have no prior experience using Azure.</span></span> <span data-ttu-id="c8dd2-107">Za méně než 30 minut budete mít aplikaci, která se používá několik prostředků Azure a spouštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-107">In less than 30 minutes, you will have an application that uses multiple Azure resources up and running in hello cloud.</span></span>

<span data-ttu-id="c8dd2-108">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="c8dd2-108">You will learn:</span></span>

* <span data-ttu-id="c8dd2-109">Jak toocreate nebo přizpůsobit existující webovou službu pro spotřebu webovým řešením.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-109">How toocreate or adapt an existing web service for consumption by a web solution.</span></span>
* <span data-ttu-id="c8dd2-110">Jak toouse hello Azure WCF předávací služby tooshare dat mezi aplikací Azure a webovou službu hostovanou jinde.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-110">How toouse hello Azure WCF Relay service tooshare data between an Azure application and a web service hosted elsewhere.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a><span data-ttu-id="c8dd2-111">Jak Azure Relay pomáhá s hybridními řešeními</span><span class="sxs-lookup"><span data-stu-id="c8dd2-111">How Azure Relay helps with hybrid solutions</span></span>

<span data-ttu-id="c8dd2-112">Podniková řešení se obvykle skládají z kombinace vlastního kódu napsaného tootackle nových a jedinečných obchodních požadavků a stávajících funkcí poskytovaných řešeními a systémy, které jsou již v místní.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-112">Business solutions are typically composed of a combination of custom code written tootackle new and unique business requirements and existing functionality provided by solutions and systems that are already in place.</span></span>

<span data-ttu-id="c8dd2-113">Řešení architekty začínáte toouse hello cloud pro snazší zpracování požadavků škálování a nižších provozních nákladů.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-113">Solution architects are starting toouse hello cloud for easier handling of scale requirements and lower operational costs.</span></span> <span data-ttu-id="c8dd2-114">Při tom najdou, že existující prostředky služeb mu tooleverage jako stavební bloky pro svá řešení jsou podnikovým firewallem hello a mimo snadno spojit pro přístup k hello cloudové řešení.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-114">In doing so, they find that existing service assets they'd like tooleverage as building blocks for their solutions are inside hello corporate firewall and out of easy reach for access by hello cloud solution.</span></span> <span data-ttu-id="c8dd2-115">Spousta interních služeb nejsou vytvořené nebo hostovaná tak, aby se dala snadno vystavit na hranici podnikové sítě hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-115">Many internal services are not built or hosted in a way that they can be easily exposed at hello corporate network edge.</span></span>

<span data-ttu-id="c8dd2-116">[Předávání přes Azure](https://azure.microsoft.com/services/service-bus/) je určená pro hello případ použití vzít existující webové služby Windows Communication Foundation (WCF) a bezpečně přístupné toosolutions nacházející se mimo firemní zónu hello bez nutnosti publikování těchto služeb. nežádoucí změny toohello podnikové síťové infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-116">[Azure Relay](https://azure.microsoft.com/services/service-bus/) is designed for hello use-case of taking existing Windows Communication Foundation (WCF) web services and making those services securely accessible toosolutions that reside outside hello corporate perimeter without requiring intrusive changes toohello corporate network infrastructure.</span></span> <span data-ttu-id="c8dd2-117">Takové služby předávání přes se stále hostují uvnitř existujícího prostředí, ale delegují čekání příchozí relace a požadavky toohello služby předávání hostovaných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-117">Such relay services are still hosted inside their existing environment, but they delegate listening for incoming sessions and requests toohello cloud-hosted relay service.</span></span> <span data-ttu-id="c8dd2-118">Azure Relay taky takové služby chrání před neoprávněným přístupem pomocí ověření [Sdíleným přístupovým podpisem](../service-bus-messaging/service-bus-sas.md) (SAS).</span><span class="sxs-lookup"><span data-stu-id="c8dd2-118">Azure Relay also protects those services from unauthorized access by using [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) authentication.</span></span>

## <a name="solution-scenario"></a><span data-ttu-id="c8dd2-119">Scénář řešení</span><span class="sxs-lookup"><span data-stu-id="c8dd2-119">Solution scenario</span></span>
<span data-ttu-id="c8dd2-120">V tomto kurzu vytvoříte webu ASP.NET, která umožňuje toosee seznam produktů na stránce inventáře produktů hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-120">In this tutorial, you will create an ASP.NET website that enables you toosee a list of products on hello product inventory page.</span></span>

![][0]

<span data-ttu-id="c8dd2-121">Hello kurz předpokládá, že máte produktové informace dostupné v existující místní systém a používá předávání přes Azure tooreach do takového systému.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-121">hello tutorial assumes that you have product information in an existing on-premises system, and uses Azure Relay tooreach into that system.</span></span> <span data-ttu-id="c8dd2-122">To se simuluje jako webová služba, která spustí jednoduchou konzolovou aplikaci a využívá sadu produktů uloženou v paměti.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-122">This is simulated by a web service that runs in a simple console application and is backed by an in-memory set of products.</span></span> <span data-ttu-id="c8dd2-123">Bude se mít toorun této konzolové aplikace ve vašem počítači a nasazení hello webové role do Azure.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-123">You will be able toorun this console application on your own computer and deploy hello web role into Azure.</span></span> <span data-ttu-id="c8dd2-124">Díky tomu se zobrazí jak hello webová role běžící v hello datovém centru Azure skutečně zavolá do vašeho počítače, i když váš počítač skoro určitě nachází za nejméně jedním firewallem a vrstvou sítě adres překlad (NAT).</span><span class="sxs-lookup"><span data-stu-id="c8dd2-124">By doing so, you will see how hello web role running in hello Azure datacenter will indeed call into your computer, even though your computer will almost certainly reside behind at least one firewall and a network address translation (NAT) layer.</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="c8dd2-125">Nastavit hello vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="c8dd2-125">Set up hello development environment</span></span>

<span data-ttu-id="c8dd2-126">Před zahájením vývojem aplikací pro Azure, stáhnout hello nástroje a nastavení vývojového prostředí:</span><span class="sxs-lookup"><span data-stu-id="c8dd2-126">Before you can begin developing Azure applications, download hello tools and set up your development environment:</span></span>

1. <span data-ttu-id="c8dd2-127">Nainstalujte hello Azure SDK pro .NET ze hello SDK [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c8dd2-127">Install hello Azure SDK for .NET from hello SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="c8dd2-128">V hello **.NET** sloupce, klikněte na tlačítko hello verzi [Visual Studio](http://www.visualstudio.com) používáte.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-128">In hello **.NET** column, click hello version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="c8dd2-129">Hello kroky v tomto kurzu Visual Studio 2015, ale také pracovat s Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-129">hello steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="c8dd2-130">Po zobrazení výzvy toorun nebo uložení instalačního programu hello, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-130">When prompted toorun or save hello installer, click **Run**.</span></span>
4. <span data-ttu-id="c8dd2-131">V hello **instalačního programu webové platformy**, klikněte na tlačítko **nainstalovat** a pokračovat v instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-131">In hello **Web Platform Installer**, click **Install** and proceed with hello installation.</span></span>
5. <span data-ttu-id="c8dd2-132">Po dokončení instalace hello budete mít všechno potřebné toostart toodevelop hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-132">Once hello installation is complete, you will have everything necessary toostart toodevelop hello app.</span></span> <span data-ttu-id="c8dd2-133">Hello SDK obsahuje nástroje, které vám umožní snadno vyvíjet aplikace Azure v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-133">hello SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="c8dd2-134">Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="c8dd2-134">Create a namespace</span></span>

<span data-ttu-id="c8dd2-135">pomocí toobegin hello funkce předávání v Azure, musíte nejdřív vytvořit obor názvů služby.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-135">toobegin using hello relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="c8dd2-136">Obor názvů poskytuje kontejner oboru pro adresování prostředků Azure v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-136">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="c8dd2-137">Postupujte podle hello [pokynů tady](relay-create-namespace-portal.md) toocreate předávání názvů.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-137">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="create-an-on-premises-server"></a><span data-ttu-id="c8dd2-138">Vytvoření lokálního serveru</span><span class="sxs-lookup"><span data-stu-id="c8dd2-138">Create an on-premises server</span></span>

<span data-ttu-id="c8dd2-139">Nejdřív vytvoříte lokální (testovací) systém katalogu produktů.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-139">First, you will build a (mock) on-premises product catalog system.</span></span> <span data-ttu-id="c8dd2-140">Bude vcelku jednoduchý; Zobrazí se to jako představující skutečný lokální systém katalogu produktu s kompletní rovinou služeb pokoušíme toointegrate.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-140">It will be fairly simple; you can see this as representing an actual on-premises product catalog system with a complete service surface that we're trying toointegrate.</span></span>

<span data-ttu-id="c8dd2-141">Tento projekt je konzolová aplikace z Visual Studia a pomocí hello [balíček Azure Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello konfiguračních nastavení a knihovny Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-141">This project is a Visual Studio console application, and uses hello [Azure Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello Service Bus libraries and configuration settings.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="c8dd2-142">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="c8dd2-142">Create hello project</span></span>

1. <span data-ttu-id="c8dd2-143">Spusťte Visual Studio s právy správce.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-143">Using administrator privileges, start Microsoft Visual Studio.</span></span> <span data-ttu-id="c8dd2-144">Ano, klikněte pravým tlačítkem na ikonu programu sady Visual Studio hello toodo a pak klikněte na **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-144">toodo so, right-click hello Visual Studio program icon, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="c8dd2-145">V sadě Visual Studio na hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-145">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="c8dd2-146">V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Konzolová aplikace (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-146">From **Installed Templates**, under **Visual C#**, click **Console App (.NET Framework)**.</span></span> <span data-ttu-id="c8dd2-147">V hello **název** pole, název typu hello **ProductsServer**:</span><span class="sxs-lookup"><span data-stu-id="c8dd2-147">In hello **Name** box, type hello name **ProductsServer**:</span></span>

   ![][11]
4. <span data-ttu-id="c8dd2-148">Klikněte na tlačítko **OK** toocreate hello **ProductsServer** projektu.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-148">Click **OK** toocreate hello **ProductsServer** project.</span></span>
5. <span data-ttu-id="c8dd2-149">Pokud jste již nainstalovali hello Správce balíčků NuGet pro Visual Studio, toohello další krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-149">If you have already installed hello NuGet package manager for Visual Studio, skip toohello next step.</span></span> <span data-ttu-id="c8dd2-150">Pokud ne, přejděte na [NuGet][NuGet] a klikněte na [Nainstalovat NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span><span class="sxs-lookup"><span data-stu-id="c8dd2-150">Otherwise, visit [NuGet][NuGet] and click [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span></span> <span data-ttu-id="c8dd2-151">Postupujte podle Správce balíčků NuGet hello tooinstall hello výzvy a potom znovu spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-151">Follow hello prompts tooinstall hello NuGet package manager, then re-start Visual Studio.</span></span>
6. <span data-ttu-id="c8dd2-152">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsServer** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-152">In Solution Explorer, right-click hello **ProductsServer** project, then click **Manage NuGet Packages**.</span></span>
7. <span data-ttu-id="c8dd2-153">Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-153">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="c8dd2-154">Vyberte hello **WindowsAzure.ServiceBus** balíčku.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-154">Select hello **WindowsAzure.ServiceBus** package.</span></span>
8. <span data-ttu-id="c8dd2-155">Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-155">Click **Install**, and accept hello terms of use.</span></span>

   ![][13]

   <span data-ttu-id="c8dd2-156">Všimněte si, že hello vyžaduje, že teď jsou reference na sestavení klienta.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-156">Note that hello required client assemblies are now referenced.</span></span>
8. <span data-ttu-id="c8dd2-157">Přidejte novou třídu pro váš produktový kontrakt.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-157">Add a new class for your product contract.</span></span> <span data-ttu-id="c8dd2-158">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsServer** projektu a klikněte na tlačítko **přidat**a potom klikněte na **třída**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-158">In Solution Explorer, right-click hello **ProductsServer** project and click **Add**, and then click **Class**.</span></span>
9. <span data-ttu-id="c8dd2-159">V hello **název** pole, název typu hello **ProductsContract.cs**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-159">In hello **Name** box, type hello name **ProductsContract.cs**.</span></span> <span data-ttu-id="c8dd2-160">Pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-160">Then click **Add**.</span></span>
10. <span data-ttu-id="c8dd2-161">V **ProductsContract.cs**, nahraďte definici oboru názvů hello hello následující kód, který definuje kontrakt hello služby hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-161">In **ProductsContract.cs**, replace hello namespace definition with hello following code, which defines hello contract for hello service.</span></span>

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
11. <span data-ttu-id="c8dd2-162">V souboru Program.cs místo definice oboru názvů hello hello následující kód, který přidá službu profilu hello a hello hosta pro ni.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-162">In Program.cs, replace hello namespace definition with hello following code, which adds hello profile service and hello host for it.</span></span>

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
12. <span data-ttu-id="c8dd2-163">V Průzkumníku řešení klikněte dvakrát na hello **App.config** souboru tooopen ji v editoru Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-163">In Solution Explorer, double-click hello **App.config** file tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="c8dd2-164">Na konci hello hello `<system.ServiceModel>` – element (ale stále ještě v `<system.ServiceModel>`), přidejte následující kód XML hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-164">At hello bottom of hello `<system.ServiceModel>` element (but still within `<system.ServiceModel>`), add hello following XML code.</span></span> <span data-ttu-id="c8dd2-165">Být jisti tooreplace *yourServiceNamespace* s hello název vašeho oboru názvů, a *yourKey* s hello SAS klíč, který jste předtím získali z portálu hello:</span><span class="sxs-lookup"><span data-stu-id="c8dd2-165">Be sure tooreplace *yourServiceNamespace* with hello name of your namespace, and *yourKey* with hello SAS key you retrieved earlier from hello portal:</span></span>

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
13. <span data-ttu-id="c8dd2-166">Stále v souboru App.config v hello `<appSettings>` elementu, nahraďte hello připojení řetězcovou hodnotu s jste předtím získali z portálu hello hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-166">Still in App.config, in hello `<appSettings>` element, replace hello connection string value with hello connection string you previously obtained from hello portal.</span></span>

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. <span data-ttu-id="c8dd2-167">Stiskněte klávesu **Ctrl + Shift + B** nebo z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** toobuild hello aplikace a ověřte hello přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-167">Press **Ctrl+Shift+B** or from hello **Build** menu, click **Build Solution** toobuild hello application and verify hello accuracy of your work so far.</span></span>

## <a name="create-an-aspnet-application"></a><span data-ttu-id="c8dd2-168">Vytvoření aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c8dd2-168">Create an ASP.NET application</span></span>

<span data-ttu-id="c8dd2-169">V této části sestavíte jednoduchou aplikaci ASP.NET, která zobrazí data načtená z vaší produktové služby.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-169">In this section you will build a simple ASP.NET application that displays data retrieved from your product service.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="c8dd2-170">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="c8dd2-170">Create hello project</span></span>

1. <span data-ttu-id="c8dd2-171">Zkontrolkujte, že je Visual Studio spuštěné s právy správce.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-171">Ensure that Visual Studio is running with administrator privileges.</span></span>
2. <span data-ttu-id="c8dd2-172">V sadě Visual Studio na hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-172">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="c8dd2-173">V **Nainstalovaných šablonách** v části **Visual C#**, klikněte na **Webová aplikace ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-173">From **Installed Templates**, under **Visual C#**, click **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="c8dd2-174">Název projektu hello **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-174">Name hello project **ProductsPortal**.</span></span> <span data-ttu-id="c8dd2-175">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-175">Then click **OK**.</span></span>

   ![][15]

4. <span data-ttu-id="c8dd2-176">Z hello **šablony ASP.NET** seznamu v hello **nové webové aplikace ASP.NET** dialogové okno, klikněte na tlačítko **MVC**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-176">From hello **ASP.NET Templates** list in hello **New ASP.NET Web Application** dialog, click **MVC**.</span></span>

   ![][16]

6. <span data-ttu-id="c8dd2-177">Klikněte na tlačítko hello **změna ověřování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-177">Click hello **Change Authentication** button.</span></span> <span data-ttu-id="c8dd2-178">V hello **změna ověřování** dialogové okno pole, ujistěte se, že **bez ověřování** je vybrána a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-178">In hello **Change Authentication** dialog box, ensure that **No Authentication** is selected, and then click **OK**.</span></span> <span data-ttu-id="c8dd2-179">V tomto kurzu nasazujete aplikace, která nepotřebuje přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-179">For this tutorial, you're deploying an app that does not need a user login.</span></span>

    ![][18]

7. <span data-ttu-id="c8dd2-180">Zpět v hello **nové webové aplikace ASP.NET** dialogové okno, klikněte na tlačítko **OK** aplikace MVC toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-180">Back in hello **New ASP.NET Web Application** dialog, click **OK** toocreate hello MVC app.</span></span>
8. <span data-ttu-id="c8dd2-181">Teď musíte nakonfigurovat prostředky Azure pro novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-181">Now you must configure Azure resources for a new web app.</span></span> <span data-ttu-id="c8dd2-182">Postupujte podle kroků hello v hello [publikování tooAzure části tohoto článku](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="c8dd2-182">Follow hello steps in hello [Publish tooAzure section of this article](../app-service-web/app-service-web-get-started-dotnet.md).</span></span> <span data-ttu-id="c8dd2-183">Potom vraťte toothis kurzu a pokračujte dalším krokem toohello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-183">Then, return toothis tutorial and proceed toohello next step.</span></span>
10. <span data-ttu-id="c8dd2-184">V Průzkumníkovi řešení klikněte pravým tlačítkem na **Modely**, pak levým na **Přidat** a pak na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-184">In Solution Explorer, right-click **Models** and then click **Add**, then click **Class**.</span></span> <span data-ttu-id="c8dd2-185">V hello **název** pole, název typu hello **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-185">In hello **Name** box, type hello name **Product.cs**.</span></span> <span data-ttu-id="c8dd2-186">Pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-186">Then click **Add**.</span></span>

    ![][17]

### <a name="modify-hello-web-application"></a><span data-ttu-id="c8dd2-187">Úprava hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c8dd2-187">Modify hello web application</span></span>

1. <span data-ttu-id="c8dd2-188">V souboru Product.cs hello v sadě Visual Studio nahraďte existující definici oboru názvů hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-188">In hello Product.cs file in Visual Studio, replace hello existing namespace definition with hello following code.</span></span>

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
2. <span data-ttu-id="c8dd2-189">V Průzkumníku řešení rozbalte hello **řadiče** složky, dvakrát hello **HomeController.cs** souboru tooopen ho v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-189">In Solution Explorer, expand hello **Controllers** folder, then double-click hello **HomeController.cs** file tooopen it in Visual Studio.</span></span>
3. <span data-ttu-id="c8dd2-190">V **HomeController.cs**, nahraďte existující definici oboru názvů hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-190">In **HomeController.cs**, replace hello existing namespace definition with hello following code.</span></span>

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
4. <span data-ttu-id="c8dd2-191">V Průzkumníku řešení rozbalte složku Views\Shared hello, pak poklikejte na **_Layout.cshtml** tooopen ji v editoru Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-191">In Solution Explorer, expand hello Views\Shared folder, then double-click **_Layout.cshtml** tooopen it in hello Visual Studio editor.</span></span>
5. <span data-ttu-id="c8dd2-192">Změňte všechny výskyty **Moje aplikace technologie ASP.NET** příliš**LITWARE's Products**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-192">Change all occurrences of **My ASP.NET Application** too**LITWARE's Products**.</span></span>
6. <span data-ttu-id="c8dd2-193">Odebrat hello **Domů**, **o**, a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-193">Remove hello **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="c8dd2-194">V následujícím příkladu hello odstraňte hello zvýrazněná kód.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-194">In hello following example, delete hello highlighted code.</span></span>

    ![][41]

7. <span data-ttu-id="c8dd2-195">V Průzkumníku řešení rozbalte složku Views\Home hello, pak poklikejte na **Index.cshtml** tooopen ji v editoru Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-195">In Solution Explorer, expand hello Views\Home folder, then double-click **Index.cshtml** tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="c8dd2-196">Nahraďte hello celý obsah souboru hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-196">Replace hello entire contents of hello file with hello following code.</span></span>

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
8. <span data-ttu-id="c8dd2-197">tooverify hello přesnost své dosavadní práce, stiskněte **Ctrl + Shift + B** toobuild hello projektu.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-197">tooverify hello accuracy of your work so far, you can press **Ctrl+Shift+B** toobuild hello project.</span></span>

### <a name="run-hello-app-locally"></a><span data-ttu-id="c8dd2-198">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c8dd2-198">Run hello app locally</span></span>

<span data-ttu-id="c8dd2-199">Spusťte tooverify hello aplikace, která funguje.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-199">Run hello application tooverify that it works.</span></span>

1. <span data-ttu-id="c8dd2-200">Ujistěte se, že **ProductsPortal** je hello aktivního projektu.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-200">Ensure that **ProductsPortal** is hello active project.</span></span> <span data-ttu-id="c8dd2-201">Název projektu hello v Průzkumníku řešení klikněte pravým tlačítkem a vyberte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-201">Right-click hello project name in Solution Explorer and select **Set As Startup Project**.</span></span>
2. <span data-ttu-id="c8dd2-202">V sadě Visual Studio stiskněte **F5**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-202">In Visual Studio, press **F5**.</span></span>
3. <span data-ttu-id="c8dd2-203">Měla by se objevit vaše aplikace, jak běží v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-203">Your application should appear, running in a browser.</span></span>

   ![][21]

## <a name="put-hello-pieces-together"></a><span data-ttu-id="c8dd2-204">Připravili částí hello</span><span class="sxs-lookup"><span data-stu-id="c8dd2-204">Put hello pieces together</span></span>

<span data-ttu-id="c8dd2-205">dalším krokem Hello je toohook hello místní produkty server s hello aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-205">hello next step is toohook up hello on-premises products server with hello ASP.NET application.</span></span>

1. <span data-ttu-id="c8dd2-206">Pokud již není otevřený, v sadě Visual Studio znovu otevřete hello **ProductsPortal** projektu, které jste vytvořili v hello [vytvořit aplikaci ASP.NET](#create-an-aspnet-application) části.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-206">If it is not already open, in Visual Studio re-open hello **ProductsPortal** project you created in hello [Create an ASP.NET application](#create-an-aspnet-application) section.</span></span>
2. <span data-ttu-id="c8dd2-207">Podobně jako toohello krok v části "Vytvoření místnímu serveru" hello, přidání odkazů projektu toohello balíček NuGet hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-207">Similar toohello step in hello "Create an On-Premises Server" section, add hello NuGet package toohello project references.</span></span> <span data-ttu-id="c8dd2-208">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-208">In Solution Explorer, right-click hello **ProductsPortal** project, then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="c8dd2-209">Vyhledejte "Service Bus" a vyberte hello **WindowsAzure.ServiceBus** položky.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-209">Search for "Service Bus" and select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="c8dd2-210">Potom dokončete hello instalace a zavřete toto dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-210">Then complete hello installation and close this dialog box.</span></span>
4. <span data-ttu-id="c8dd2-211">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** projektu a pak klikněte na **přidat**, pak **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-211">In Solution Explorer, right-click hello **ProductsPortal** project, then click **Add**, then **Existing Item**.</span></span>
5. <span data-ttu-id="c8dd2-212">Přejděte toohello **ProductsContract.cs** soubor z hello **ProductsServer** projekt konzoly.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-212">Navigate toohello **ProductsContract.cs** file from hello **ProductsServer** console project.</span></span> <span data-ttu-id="c8dd2-213">Klikněte na tlačítko toohighlight ProductsContract.cs.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-213">Click toohighlight ProductsContract.cs.</span></span> <span data-ttu-id="c8dd2-214">Klikněte na tlačítko hello šipka dolů vedle příliš**přidat**, pak klikněte na tlačítko **přidat jako odkaz**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-214">Click hello down arrow next too**Add**, then click **Add as Link**.</span></span>

   ![][24]

6. <span data-ttu-id="c8dd2-215">Nyní otevřete hello **HomeController.cs** souboru v editoru Visual Studio hello a nahraďte definici oboru názvů hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-215">Now open hello **HomeController.cs** file in hello Visual Studio editor and replace hello namespace definition with hello following code.</span></span> <span data-ttu-id="c8dd2-216">Být jisti tooreplace *yourServiceNamespace* s hello názvem vašeho oboru názvů služby a *yourKey* váš SAS klíč.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-216">Be sure tooreplace *yourServiceNamespace* with hello name of your service namespace, and *yourKey* with your SAS key.</span></span> <span data-ttu-id="c8dd2-217">Tato akce povolí hello klienta toocall hello místní službu a vrátit výsledek volání hello hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-217">This will enable hello client toocall hello on-premises service, returning hello result of hello call.</span></span>

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
7. <span data-ttu-id="c8dd2-218">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** řešení (zkontrolujte že tooright kliknutím hello řešení, není hello projektu).</span><span class="sxs-lookup"><span data-stu-id="c8dd2-218">In Solution Explorer, right-click hello **ProductsPortal** solution (make sure tooright-click hello solution, not hello project).</span></span> <span data-ttu-id="c8dd2-219">Klikněte na **Přidat** a pak klikněte na **Existující projekt**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-219">Click **Add**, then click **Existing Project**.</span></span>
8. <span data-ttu-id="c8dd2-220">Přejděte toohello **ProductsServer** projekt, pak poklikejte na hello **ProductsServer.csproj** tooadd soubor řešení ho.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-220">Navigate toohello **ProductsServer** project, then double-click hello **ProductsServer.csproj** solution file tooadd it.</span></span>
9. <span data-ttu-id="c8dd2-221">**ProductsServer** musí být spuštěn v pořadí toodisplay hello data v **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-221">**ProductsServer** must be running in order toodisplay hello data on **ProductsPortal**.</span></span> <span data-ttu-id="c8dd2-222">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** řešení a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-222">In Solution Explorer, right-click hello **ProductsPortal** solution and click **Properties**.</span></span> <span data-ttu-id="c8dd2-223">Hello **stránky vlastností** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-223">hello **Property Pages** dialog box is displayed.</span></span>
10. <span data-ttu-id="c8dd2-224">Na levé straně hello, klikněte na tlačítko **spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-224">On hello left side, click **Startup Project**.</span></span> <span data-ttu-id="c8dd2-225">Na pravé straně hello, klikněte na tlačítko **více projektů po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-225">On hello right side, click **Multiple startup projects**.</span></span> <span data-ttu-id="c8dd2-226">Ujistěte se, že **ProductsServer** a **ProductsPortal** zobrazí v tomto pořadí, s **spustit** nastavit jako hello akci pro oba.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-226">Ensure that **ProductsServer** and **ProductsPortal** appear, in that order, with **Start** set as hello action for both.</span></span>

      ![][25]

11. <span data-ttu-id="c8dd2-227">Stále v hello **vlastnosti** dialogové okno, klikněte na tlačítko **závislosti projektu** na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-227">Still in hello **Properties** dialog box, click **Project Dependencies** on hello left side.</span></span>
12. <span data-ttu-id="c8dd2-228">V hello **projekty** seznamu, klikněte na tlačítko **ProductsServer**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-228">In hello **Projects** list, click **ProductsServer**.</span></span> <span data-ttu-id="c8dd2-229">Zkontrolujte, že **ProductsPortal** není vybraný.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-229">Ensure that **ProductsPortal** is not selected.</span></span>
13. <span data-ttu-id="c8dd2-230">V hello **projekty** seznamu, klikněte na tlačítko **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-230">In hello **Projects** list, click **ProductsPortal**.</span></span> <span data-ttu-id="c8dd2-231">Zkontrolujte, že je **ProductsServer** vybraný.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-231">Ensure that **ProductsServer** is selected.</span></span>

    ![][26]

14. <span data-ttu-id="c8dd2-232">Klikněte na tlačítko **OK** v hello **stránky vlastností** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-232">Click **OK** in hello **Property Pages** dialog box.</span></span>

## <a name="run-hello-project-locally"></a><span data-ttu-id="c8dd2-233">Místní spuštění projektu hello</span><span class="sxs-lookup"><span data-stu-id="c8dd2-233">Run hello project locally</span></span>

<span data-ttu-id="c8dd2-234">tootest hello místně, aplikace v sadě Visual Studio klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-234">tootest hello application locally, in Visual Studio press **F5**.</span></span> <span data-ttu-id="c8dd2-235">místní server Hello (**ProductsServer**) musí nejdřív spustit a pak hello **ProductsPortal** v okně prohlížeče měla spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-235">hello on-premises server (**ProductsServer**) should start first, then hello **ProductsPortal** application should start in a browser window.</span></span> <span data-ttu-id="c8dd2-236">Tentokrát uvidíte, že tento hello inventář produktů zobrazí seznam dat načtených z hello produktu služby v místním systému.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-236">This time, you will see that hello product inventory lists data retrieved from hello product service on-premises system.</span></span>

![][10]

<span data-ttu-id="c8dd2-237">Stiskněte klávesu **aktualizovat** na hello **ProductsPortal** stránky.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-237">Press **Refresh** on hello **ProductsPortal** page.</span></span> <span data-ttu-id="c8dd2-238">Pokaždé, když obnovíte stránku hello, zobrazí se aplikace server hello zobrazí zpráva při `GetProducts()` z **ProductsServer** je volána.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-238">Each time you refresh hello page, you'll see hello server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

<span data-ttu-id="c8dd2-239">Zavřete obě aplikace před pokračováním toohello další krok.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-239">Close both applications before proceeding toohello next step.</span></span>

## <a name="deploy-hello-productsportal-project-tooan-azure-web-app"></a><span data-ttu-id="c8dd2-240">Nasazení hello ProductsPortal projektu tooan Azure webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c8dd2-240">Deploy hello ProductsPortal project tooan Azure web app</span></span>

<span data-ttu-id="c8dd2-241">dalším krokem Hello je toorepublish hello Azure webové aplikace **ProductsPortal** front-endu.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-241">hello next step is toorepublish hello Azure Web app **ProductsPortal** frontend.</span></span> <span data-ttu-id="c8dd2-242">Hello následující:</span><span class="sxs-lookup"><span data-stu-id="c8dd2-242">Do hello following:</span></span>

1. <span data-ttu-id="c8dd2-243">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ProductsPortal** projektu a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-243">In Solution Explorer, right-click hello **ProductsPortal** project, and click **Publish**.</span></span> <span data-ttu-id="c8dd2-244">Potom klikněte na **publikovat** na hello **publikovat** stránky.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-244">Then, click **Publish** on hello **Publish** page.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c8dd2-245">Mohou se zobrazit chybová zpráva v okně prohlížeče hello při hello **ProductsPortal** automatickém spuštění webového projektu po nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-245">You may see an error message in hello browser window when hello **ProductsPortal** web project is automatically launched after hello deployment.</span></span> <span data-ttu-id="c8dd2-246">To je v pořádku a dochází hello **ProductsServer** aplikace ještě neběží.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-246">This is expected, and occurs because hello **ProductsServer** application isn't running yet.</span></span>
>
>

2. <span data-ttu-id="c8dd2-247">Kopírování hello URL hello nasadit webové aplikace, budete potřebovat adresu URL hello v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-247">Copy hello URL of hello deployed web app, as you will need hello URL in hello next step.</span></span> <span data-ttu-id="c8dd2-248">Tuto adresu URL můžete získat i z okna aktivita služby Azure App Service hello v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c8dd2-248">You can also obtain this URL from hello Azure App Service Activity window in Visual Studio:</span></span>

  ![][9]

3. <span data-ttu-id="c8dd2-249">Zavřete hello prohlížeče okno toostop hello spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-249">Close hello browser window toostop hello running application.</span></span>

### <a name="set-productsportal-as-web-app"></a><span data-ttu-id="c8dd2-250">Nastavení ProductsPortal jako webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c8dd2-250">Set ProductsPortal as web app</span></span>

<span data-ttu-id="c8dd2-251">Před běžící hello aplikaci v cloudu hello, musíte zajistit, aby **ProductsPortal** spustí z Visual Studia jak webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-251">Before running hello application in hello cloud, you must ensure that **ProductsPortal** is launched from within Visual Studio as a web app.</span></span>

1. <span data-ttu-id="c8dd2-252">V sadě Visual Studio, klikněte pravým tlačítkem na hello **ProductsPortal** projektu a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-252">In Visual Studio, right-click hello **ProductsPortal** project and then click **Properties**.</span></span>
2. <span data-ttu-id="c8dd2-253">V levém sloupci hello, klikněte na **webové**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-253">In hello left-hand column, click **Web**.</span></span>
3. <span data-ttu-id="c8dd2-254">V hello **spustit akci** klikněte na tlačítko hello **spustit adresu URL** tlačítko a hello textového pole zadejte hello URL dříve nasazené webové aplikace, například `http://productsportal1234567890.azurewebsites.net/`.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-254">In hello **Start Action** section, click hello **Start URL** button, and in hello text box enter hello URL for your previously deployed web app; for example, `http://productsportal1234567890.azurewebsites.net/`.</span></span>

    ![][27]

4. <span data-ttu-id="c8dd2-255">Z hello **soubor** nabídky v sadě Visual Studio, klikněte na tlačítko **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-255">From hello **File** menu in Visual Studio, click **Save All**.</span></span>
5. <span data-ttu-id="c8dd2-256">V nabídce hello sestavení v sadě Visual Studio, klikněte na tlačítko **znovu sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-256">From hello Build menu in Visual Studio, click **Rebuild Solution**.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="c8dd2-257">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c8dd2-257">Run hello application</span></span>

1. <span data-ttu-id="c8dd2-258">Stiskněte klávesu F5 toobuild a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-258">Press F5 toobuild and run hello application.</span></span> <span data-ttu-id="c8dd2-259">místní server Hello (hello **ProductsServer** Konzolová aplikace) musí nejdřív spustit a pak hello **ProductsPortal** v okně prohlížeče měla spustit aplikace, jak je znázorněno v následující obrazovce hello snímek.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-259">hello on-premises server (hello **ProductsServer** console application) should start first, then hello **ProductsPortal** application should start in a browser window, as shown in hello following screen shot.</span></span> <span data-ttu-id="c8dd2-260">Všimněte si znovu, že hello inventář produktů zobrazí seznam dat načtených z hello produktu služby v místním systému a tato data zobrazí ve webové aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-260">Notice again that hello product inventory lists data retrieved from hello product service on-premises system, and displays that data in hello web app.</span></span> <span data-ttu-id="c8dd2-261">Zkontrolujte toomake URL hello se, který **ProductsPortal** běží v cloudu hello jako webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-261">Check hello URL toomake sure that **ProductsPortal** is running in hello cloud, as an Azure web app.</span></span>

   ![][1]

   > [!IMPORTANT]
   > <span data-ttu-id="c8dd2-262">Hello **ProductsServer** konzolové aplikace a musí být spuštěná schopná tooserve hello data toohello **ProductsPortal** aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-262">hello **ProductsServer** console application must be running and able tooserve hello data toohello **ProductsPortal** application.</span></span> <span data-ttu-id="c8dd2-263">Pokud se hello prohlížeč zobrazí chybu, počkejte několik dalších sekund **ProductsServer** tooload a zobrazení hello následující zpráva.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-263">If hello browser displays an error, wait a few more seconds for **ProductsServer** tooload and display hello following message.</span></span> <span data-ttu-id="c8dd2-264">Stiskněte klávesu **aktualizovat** v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-264">Then press **Refresh** in hello browser.</span></span>
   >
   >

   ![][37]
2. <span data-ttu-id="c8dd2-265">Zpět v prohlížeči hello, stiskněte klávesu **aktualizovat** na hello **ProductsPortal** stránky.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-265">Back in hello browser, press **Refresh** on hello **ProductsPortal** page.</span></span> <span data-ttu-id="c8dd2-266">Pokaždé, když obnovíte stránku hello, zobrazí se aplikace server hello zobrazí zpráva při `GetProducts()` z **ProductsServer** je volána.</span><span class="sxs-lookup"><span data-stu-id="c8dd2-266">Each time you refresh hello page, you'll see hello server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

    ![][38]

## <a name="next-steps"></a><span data-ttu-id="c8dd2-267">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8dd2-267">Next steps</span></span>

<span data-ttu-id="c8dd2-268">toolearn Další informace o předávání přes Azure, najdete v části hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="c8dd2-268">toolearn more about Azure Relay, see hello following resources:</span></span>  

* [<span data-ttu-id="c8dd2-269">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="c8dd2-269">What is Azure Relay?</span></span>](relay-what-is-it.md)  
* [<span data-ttu-id="c8dd2-270">Jak toouse předávání</span><span class="sxs-lookup"><span data-stu-id="c8dd2-270">How toouse Relay</span></span>](service-bus-dotnet-how-to-use-relay.md)  

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
