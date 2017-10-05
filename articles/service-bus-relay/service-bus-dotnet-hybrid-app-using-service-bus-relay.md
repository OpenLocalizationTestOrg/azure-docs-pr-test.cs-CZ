---
title: "Hybridní místní/cloudová aplikace (.NET) Azure WCF Relay | Dokumentace Microsoftu"
description: "Naučte se vytvořit hybridní místní/cloudovou aplikaci .NET využívající Azure WCF Relay."
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
ms.openlocfilehash: 366922a083b9d18ef50e04eb8b459d2725315e1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a><span data-ttu-id="56023-103">Hybridní místní/cloudová aplikace .NET využívající Azure WCF Relay</span><span class="sxs-lookup"><span data-stu-id="56023-103">.NET on-premises/cloud hybrid application using Azure WCF Relay</span></span>
## <a name="introduction"></a><span data-ttu-id="56023-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="56023-104">Introduction</span></span>

<span data-ttu-id="56023-105">Tento článek popisuje, jak vytvořit hybridní cloudovou aplikaci pomocí Microsoft Azure a Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="56023-105">This article shows how to build a hybrid cloud application with Microsoft Azure and Visual Studio.</span></span> <span data-ttu-id="56023-106">Tento kurz předpokládá, že nemáte žádné předchozí zkušenosti s používáním Azure.</span><span class="sxs-lookup"><span data-stu-id="56023-106">The tutorial assumes you have no prior experience using Azure.</span></span> <span data-ttu-id="56023-107">Za méně než 30 minut budete mít aplikaci, která používá několik různých prostředků Azure a běží v cloudu.</span><span class="sxs-lookup"><span data-stu-id="56023-107">In less than 30 minutes, you will have an application that uses multiple Azure resources up and running in the cloud.</span></span>

<span data-ttu-id="56023-108">Co se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="56023-108">You will learn:</span></span>

* <span data-ttu-id="56023-109">Jak vytvořit nebo přizpůsobit existující webovou službu pro spotřebu webovým řešením.</span><span class="sxs-lookup"><span data-stu-id="56023-109">How to create or adapt an existing web service for consumption by a web solution.</span></span>
* <span data-ttu-id="56023-110">Jak používat službu Azure WCF Relay ke sdílení dat mezi aplikací Azure a webovou službou hostovanou jinde.</span><span class="sxs-lookup"><span data-stu-id="56023-110">How to use the Azure WCF Relay service to share data between an Azure application and a web service hosted elsewhere.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a><span data-ttu-id="56023-111">Jak Azure Relay pomáhá s hybridními řešeními</span><span class="sxs-lookup"><span data-stu-id="56023-111">How Azure Relay helps with hybrid solutions</span></span>

<span data-ttu-id="56023-112">Podniková řešení se obvykle skládají z kombinace vlastního kódu napsaného pro řešení nových a jedinečných podnikových řešení a stávajících funkcí poskytovaných řešeními a systémy, které již existují.</span><span class="sxs-lookup"><span data-stu-id="56023-112">Business solutions are typically composed of a combination of custom code written to tackle new and unique business requirements and existing functionality provided by solutions and systems that are already in place.</span></span>

<span data-ttu-id="56023-113">Architekti řešení začínají používat cloud, protože jim to umožňuje snadněji zvládat nároky na škálování a snížit provozní náklady.</span><span class="sxs-lookup"><span data-stu-id="56023-113">Solution architects are starting to use the cloud for easier handling of scale requirements and lower operational costs.</span></span> <span data-ttu-id="56023-114">Přitom zjišťují, že existující prostředky služeb, které by chtěli využívat jako stavební prvky pro svá řešení, jsou za firemním firewallem a cloudové řešení k nim nemá snadný přístup.</span><span class="sxs-lookup"><span data-stu-id="56023-114">In doing so, they find that existing service assets they'd like to leverage as building blocks for their solutions are inside the corporate firewall and out of easy reach for access by the cloud solution.</span></span> <span data-ttu-id="56023-115">Spousta interních služeb není postavená nebo hostovaná tak, aby se dala snadno vystavit na rozhraní firemní sítě.</span><span class="sxs-lookup"><span data-stu-id="56023-115">Many internal services are not built or hosted in a way that they can be easily exposed at the corporate network edge.</span></span>

<span data-ttu-id="56023-116">[Azure Relay](https://azure.microsoft.com/services/service-bus/) je navržené pro situace, kdy je potřeba vzít existující webové služby WCF (Windows Communication Foundation) a bezpečně je zpřístupnit pro řešení, která jsou mimo firemní zónu, a to bez nutnosti provádět nežádoucí změny infrastruktury podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="56023-116">[Azure Relay](https://azure.microsoft.com/services/service-bus/) is designed for the use-case of taking existing Windows Communication Foundation (WCF) web services and making those services securely accessible to solutions that reside outside the corporate perimeter without requiring intrusive changes to the corporate network infrastructure.</span></span> <span data-ttu-id="56023-117">Takové přenosové služby se stále hostují uvnitř existujícího prostředí, ale delegují čekání na příchozí spojení a požadavky na přenosovou službu hostovanou v cloudu.</span><span class="sxs-lookup"><span data-stu-id="56023-117">Such relay services are still hosted inside their existing environment, but they delegate listening for incoming sessions and requests to the cloud-hosted relay service.</span></span> <span data-ttu-id="56023-118">Azure Relay taky takové služby chrání před neoprávněným přístupem pomocí ověření [Sdíleným přístupovým podpisem](../service-bus-messaging/service-bus-sas.md) (SAS).</span><span class="sxs-lookup"><span data-stu-id="56023-118">Azure Relay also protects those services from unauthorized access by using [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) authentication.</span></span>

## <a name="solution-scenario"></a><span data-ttu-id="56023-119">Scénář řešení</span><span class="sxs-lookup"><span data-stu-id="56023-119">Solution scenario</span></span>
<span data-ttu-id="56023-120">V tomto kurzu vytvoříte webovou stránku ASP.NET, která vám umožní zobrazit seznam produktů na stránce inventáře produktů.</span><span class="sxs-lookup"><span data-stu-id="56023-120">In this tutorial, you will create an ASP.NET website that enables you to see a list of products on the product inventory page.</span></span>

![][0]

<span data-ttu-id="56023-121">Kurz předpokládá, že máte produktové informace dostupné v existujícím místním systému, a používá Azure Relay k získání přístupu do takového systému.</span><span class="sxs-lookup"><span data-stu-id="56023-121">The tutorial assumes that you have product information in an existing on-premises system, and uses Azure Relay to reach into that system.</span></span> <span data-ttu-id="56023-122">To se simuluje jako webová služba, která spustí jednoduchou konzolovou aplikaci a využívá sadu produktů uloženou v paměti.</span><span class="sxs-lookup"><span data-stu-id="56023-122">This is simulated by a web service that runs in a simple console application and is backed by an in-memory set of products.</span></span> <span data-ttu-id="56023-123">Taky si vyzkoušíte spuštění této konzolové aplikace na svém vlastním počítači a nasazení webové role do Azure.</span><span class="sxs-lookup"><span data-stu-id="56023-123">You will be able to run this console application on your own computer and deploy the web role into Azure.</span></span> <span data-ttu-id="56023-124">Uvidíte tak, jak webová role běžící v datovém centru Azure skutečně zavolá do vašeho počítače, i když se váš počítač skoro určitě nachází za nejméně jedním firewallem a vrstvou překládání adres (NAT).</span><span class="sxs-lookup"><span data-stu-id="56023-124">By doing so, you will see how the web role running in the Azure datacenter will indeed call into your computer, even though your computer will almost certainly reside behind at least one firewall and a network address translation (NAT) layer.</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="56023-125">Nastavení vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="56023-125">Set up the development environment</span></span>

<span data-ttu-id="56023-126">Než začnete s vývojem aplikací pro Azure, stáhněte si nástroje a nastavte si vývojové prostředí:</span><span class="sxs-lookup"><span data-stu-id="56023-126">Before you can begin developing Azure applications, download the tools and set up your development environment:</span></span>

1. <span data-ttu-id="56023-127">Nainstalujte sadu Azure SDK pro .NET ze [stránky pro stažení SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="56023-127">Install the Azure SDK for .NET from the SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="56023-128">Ve sloupci **.NET** klikněte na verzi sady [Visual Studio](http://www.visualstudio.com), kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="56023-128">In the **.NET** column, click the version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="56023-129">Kroky v tomto kurzu používají sadu Visual Studio 2015, ale také pracují se sadou Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="56023-129">The steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="56023-130">Když se zobrazí dialog pro spuštění nebo uložení instalačního programu, klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="56023-130">When prompted to run or save the installer, click **Run**.</span></span>
4. <span data-ttu-id="56023-131">V **Instalačním programu webové platformy** klikněte na **Instalovat** a pokračujte v instalaci.</span><span class="sxs-lookup"><span data-stu-id="56023-131">In the **Web Platform Installer**, click **Install** and proceed with the installation.</span></span>
5. <span data-ttu-id="56023-132">Po dokončení instalace budete mít všechno, co je potřeba k vývoji aplikace.</span><span class="sxs-lookup"><span data-stu-id="56023-132">Once the installation is complete, you will have everything necessary to start to develop the app.</span></span> <span data-ttu-id="56023-133">Sada SDK obsahuje nástroje, které vám umožní snadno vyvíjet aplikace pro Azure ve Visual Studiu.</span><span class="sxs-lookup"><span data-stu-id="56023-133">The SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="56023-134">Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="56023-134">Create a namespace</span></span>

<span data-ttu-id="56023-135">Pokud chcete začít používat přenosové funkce v Azure, musíte nejdříve vytvořit obor názvů služby.</span><span class="sxs-lookup"><span data-stu-id="56023-135">To begin using the relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="56023-136">Obor názvů poskytuje kontejner oboru pro adresování prostředků Azure v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="56023-136">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="56023-137">Pokud chcete vytvořit obor názvů Relay, postupujte podle [těchto pokynů](relay-create-namespace-portal.md).</span><span class="sxs-lookup"><span data-stu-id="56023-137">Follow the [instructions here](relay-create-namespace-portal.md) to create a Relay namespace.</span></span>

## <a name="create-an-on-premises-server"></a><span data-ttu-id="56023-138">Vytvoření lokálního serveru</span><span class="sxs-lookup"><span data-stu-id="56023-138">Create an on-premises server</span></span>

<span data-ttu-id="56023-139">Nejdřív vytvoříte lokální (testovací) systém katalogu produktů.</span><span class="sxs-lookup"><span data-stu-id="56023-139">First, you will build a (mock) on-premises product catalog system.</span></span> <span data-ttu-id="56023-140">Bude vcelku jednoduchý a nahradí skutečný lokální systém katalogu produktů, i s kompletní rovinou služeb, kterou chceme integrovat.</span><span class="sxs-lookup"><span data-stu-id="56023-140">It will be fairly simple; you can see this as representing an actual on-premises product catalog system with a complete service surface that we're trying to integrate.</span></span>

<span data-ttu-id="56023-141">Tento projekt je konzolová aplikace z Visual Studia a pomocí [balíčku NuGet pro Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) zahrnuje konfiguračních nastavení a knihovny Service Bus.</span><span class="sxs-lookup"><span data-stu-id="56023-141">This project is a Visual Studio console application, and uses the [Azure Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) to include the Service Bus libraries and configuration settings.</span></span>

### <a name="create-the-project"></a><span data-ttu-id="56023-142">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="56023-142">Create the project</span></span>

1. <span data-ttu-id="56023-143">Spusťte Visual Studio s právy správce.</span><span class="sxs-lookup"><span data-stu-id="56023-143">Using administrator privileges, start Microsoft Visual Studio.</span></span> <span data-ttu-id="56023-144">Pokud tak chcete učinit, klikněte pravým tlačítkem na ikonu programu Visual Studio a potom klikněte na **Spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="56023-144">To do so, right-click the Visual Studio program icon, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="56023-145">Ve Visual Studiu v nabídce **Soubor** klikněte na **Nový** a pak na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="56023-145">In Visual Studio, on the **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="56023-146">V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Konzolová aplikace (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="56023-146">From **Installed Templates**, under **Visual C#**, click **Console App (.NET Framework)**.</span></span> <span data-ttu-id="56023-147">Do pole **Název** zadejte název **ProductsServer**:</span><span class="sxs-lookup"><span data-stu-id="56023-147">In the **Name** box, type the name **ProductsServer**:</span></span>

   ![][11]
4. <span data-ttu-id="56023-148">Kliknutím na tlačítko **OK** vytvořte projekt **ProductsServer**.</span><span class="sxs-lookup"><span data-stu-id="56023-148">Click **OK** to create the **ProductsServer** project.</span></span>
5. <span data-ttu-id="56023-149">Pokud jste už nainstalovali správce balíčků NuGet pro Visual Studio, přejděte na další krok.</span><span class="sxs-lookup"><span data-stu-id="56023-149">If you have already installed the NuGet package manager for Visual Studio, skip to the next step.</span></span> <span data-ttu-id="56023-150">Pokud ne, přejděte na [NuGet][NuGet] a klikněte na [Nainstalovat NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span><span class="sxs-lookup"><span data-stu-id="56023-150">Otherwise, visit [NuGet][NuGet] and click [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span></span> <span data-ttu-id="56023-151">Podle pokynů nainstalujte správce balíčků NuGet, a pak znovu spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56023-151">Follow the prompts to install the NuGet package manager, then re-start Visual Studio.</span></span>
6. <span data-ttu-id="56023-152">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ProductsServer**, pak klikněte na **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="56023-152">In Solution Explorer, right-click the **ProductsServer** project, then click **Manage NuGet Packages**.</span></span>
7. <span data-ttu-id="56023-153">Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="56023-153">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="56023-154">Vyberte balíček **WindowsAzure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="56023-154">Select the **WindowsAzure.ServiceBus** package.</span></span>
8. <span data-ttu-id="56023-155">Klikněte na **Instalovat** a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="56023-155">Click **Install**, and accept the terms of use.</span></span>

   ![][13]

   <span data-ttu-id="56023-156">Poznámka: Teď jsou vytvořené reference na sestavení klienta.</span><span class="sxs-lookup"><span data-stu-id="56023-156">Note that the required client assemblies are now referenced.</span></span>
8. <span data-ttu-id="56023-157">Přidejte novou třídu pro váš produktový kontrakt.</span><span class="sxs-lookup"><span data-stu-id="56023-157">Add a new class for your product contract.</span></span> <span data-ttu-id="56023-158">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ProductsServer**, pak klikněte na **Přidat** a pak na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="56023-158">In Solution Explorer, right-click the **ProductsServer** project and click **Add**, and then click **Class**.</span></span>
9. <span data-ttu-id="56023-159">Do pole **Název** zadejte název **ProductsContract.cs**.</span><span class="sxs-lookup"><span data-stu-id="56023-159">In the **Name** box, type the name **ProductsContract.cs**.</span></span> <span data-ttu-id="56023-160">Pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="56023-160">Then click **Add**.</span></span>
10. <span data-ttu-id="56023-161">V **ProductsContract.cs** místo definice oboru názvů zadejte následující kód, který definuje kontrakt služby.</span><span class="sxs-lookup"><span data-stu-id="56023-161">In **ProductsContract.cs**, replace the namespace definition with the following code, which defines the contract for the service.</span></span>

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define the service contract.
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
11. <span data-ttu-id="56023-162">V Program.cs místo definice oboru názvů zadejte následující kód, který přidá službu profilu a hosta pro ni.</span><span class="sxs-lookup"><span data-stu-id="56023-162">In Program.cs, replace the namespace definition with the following code, which adds the profile service and the host for it.</span></span>

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement the IProducts interface.
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

            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. <span data-ttu-id="56023-163">V Průzkumníku řešení poklikejte na soubor **App.config**.cs a otevře se v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56023-163">In Solution Explorer, double-click the **App.config** file to open it in the Visual Studio editor.</span></span> <span data-ttu-id="56023-164">Na konci elementu `<system.ServiceModel>` (ale pořád ještě uvnitř `<system.ServiceModel>`) přidejte následující kód XML.</span><span class="sxs-lookup"><span data-stu-id="56023-164">At the bottom of the `<system.ServiceModel>` element (but still within `<system.ServiceModel>`), add the following XML code.</span></span> <span data-ttu-id="56023-165">Nezapomeňte místo *yourServiceNamespace* zadat název vašeho oboru názvů a místo *yourKey* váš SAS klíč, který jste předtím získali z portálu:</span><span class="sxs-lookup"><span data-stu-id="56023-165">Be sure to replace *yourServiceNamespace* with the name of your namespace, and *yourKey* with the SAS key you retrieved earlier from the portal:</span></span>

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
13. <span data-ttu-id="56023-166">Ještě v souboru App.config v elementu `<appSettings>` nahraďte hodnotu připojovacího řetězce připojovacím řetězcem, který jste předtím získali z portálu.</span><span class="sxs-lookup"><span data-stu-id="56023-166">Still in App.config, in the `<appSettings>` element, replace the connection string value with the connection string you previously obtained from the portal.</span></span>

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. <span data-ttu-id="56023-167">Stiskněte **Ctrl+Shift+B** nebo v nabídce **Sestavení** klikněte na **Sestavit řešení** a tím sestavte aplikaci a potvrďte přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="56023-167">Press **Ctrl+Shift+B** or from the **Build** menu, click **Build Solution** to build the application and verify the accuracy of your work so far.</span></span>

## <a name="create-an-aspnet-application"></a><span data-ttu-id="56023-168">Vytvoření aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="56023-168">Create an ASP.NET application</span></span>

<span data-ttu-id="56023-169">V této části sestavíte jednoduchou aplikaci ASP.NET, která zobrazí data načtená z vaší produktové služby.</span><span class="sxs-lookup"><span data-stu-id="56023-169">In this section you will build a simple ASP.NET application that displays data retrieved from your product service.</span></span>

### <a name="create-the-project"></a><span data-ttu-id="56023-170">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="56023-170">Create the project</span></span>

1. <span data-ttu-id="56023-171">Zkontrolkujte, že je Visual Studio spuštěné s právy správce.</span><span class="sxs-lookup"><span data-stu-id="56023-171">Ensure that Visual Studio is running with administrator privileges.</span></span>
2. <span data-ttu-id="56023-172">Ve Visual Studiu v nabídce **Soubor** klikněte na **Nový** a pak na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="56023-172">In Visual Studio, on the **File** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="56023-173">V **Nainstalovaných šablonách** v části **Visual C#**, klikněte na **Webová aplikace ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="56023-173">From **Installed Templates**, under **Visual C#**, click **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="56023-174">Jako název projektu zadejte **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="56023-174">Name the project **ProductsPortal**.</span></span> <span data-ttu-id="56023-175">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="56023-175">Then click **OK**.</span></span>

   ![][15]

4. <span data-ttu-id="56023-176">V seznamu **Šablony ASP.NET** v dialogovém okně **Nová webová aplikace ASP.NET** klikněte na **MVC**.</span><span class="sxs-lookup"><span data-stu-id="56023-176">From the **ASP.NET Templates** list in the **New ASP.NET Web Application** dialog, click **MVC**.</span></span>

   ![][16]

6. <span data-ttu-id="56023-177">Klikněte na tlačítko **Změna ověřování**.</span><span class="sxs-lookup"><span data-stu-id="56023-177">Click the **Change Authentication** button.</span></span> <span data-ttu-id="56023-178">V dialogovém okně **Změnit ověřování** zkontrolujte, že je vybraná možnost **Bez ověřování**, a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="56023-178">In the **Change Authentication** dialog box, ensure that **No Authentication** is selected, and then click **OK**.</span></span> <span data-ttu-id="56023-179">V tomto kurzu nasazujete aplikace, která nepotřebuje přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="56023-179">For this tutorial, you're deploying an app that does not need a user login.</span></span>

    ![][18]

7. <span data-ttu-id="56023-180">Vraťte se do dialogového okna **Nová webová aplikace ASP.NET** a kliknutím na **OK** vytvořte aplikaci MVC.</span><span class="sxs-lookup"><span data-stu-id="56023-180">Back in the **New ASP.NET Web Application** dialog, click **OK** to create the MVC app.</span></span>
8. <span data-ttu-id="56023-181">Teď musíte nakonfigurovat prostředky Azure pro novou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56023-181">Now you must configure Azure resources for a new web app.</span></span> <span data-ttu-id="56023-182">Postupujte podle pokynů v [části Publikování v Azure v tomto článku](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="56023-182">Follow the steps in the [Publish to Azure section of this article](../app-service-web/app-service-web-get-started-dotnet.md).</span></span> <span data-ttu-id="56023-183">Potom se vraťte do tohoto kurzu a pokračujte dalším krokem.</span><span class="sxs-lookup"><span data-stu-id="56023-183">Then, return to this tutorial and proceed to the next step.</span></span>
10. <span data-ttu-id="56023-184">V Průzkumníkovi řešení klikněte pravým tlačítkem na **Modely**, pak levým na **Přidat** a pak na **Třída**.</span><span class="sxs-lookup"><span data-stu-id="56023-184">In Solution Explorer, right-click **Models** and then click **Add**, then click **Class**.</span></span> <span data-ttu-id="56023-185">Do pole **Název** zadejte název **Product.cs**:</span><span class="sxs-lookup"><span data-stu-id="56023-185">In the **Name** box, type the name **Product.cs**.</span></span> <span data-ttu-id="56023-186">Pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="56023-186">Then click **Add**.</span></span>

    ![][17]

### <a name="modify-the-web-application"></a><span data-ttu-id="56023-187">Úprava webové aplikace</span><span class="sxs-lookup"><span data-stu-id="56023-187">Modify the web application</span></span>

1. <span data-ttu-id="56023-188">V souboru Product.cs ve Visual Studiu nahraďte existující definici oboru názvů následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="56023-188">In the Product.cs file in Visual Studio, replace the existing namespace definition with the following code.</span></span>

   ```csharp
    // Declare properties for the products inventory.
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
2. <span data-ttu-id="56023-189">V Průzkumníku řešení rozbalte složku **Kontrolery**, pak poklikejte na soubor **HomeController.cs** a ten se otevře ve Visual Studiu.</span><span class="sxs-lookup"><span data-stu-id="56023-189">In Solution Explorer, expand the **Controllers** folder, then double-click the **HomeController.cs** file to open it in Visual Studio.</span></span>
3. <span data-ttu-id="56023-190">V souboru **HomeController.cs** nahraďte existující definici oboru názvů následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="56023-190">In **HomeController.cs**, replace the existing namespace definition with the following code.</span></span>

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. <span data-ttu-id="56023-191">V Průzkumníku řešení rozbalte složku Views\Shared, pak poklikejte na soubor **_Layout.cshtml** a ten se otevře v editoru Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="56023-191">In Solution Explorer, expand the Views\Shared folder, then double-click **_Layout.cshtml** to open it in the Visual Studio editor.</span></span>
5. <span data-ttu-id="56023-192">Všechny výskyty **My ASP.NET Application** změňte na **LITWARE's Products**.</span><span class="sxs-lookup"><span data-stu-id="56023-192">Change all occurrences of **My ASP.NET Application** to **LITWARE's Products**.</span></span>
6. <span data-ttu-id="56023-193">Odstraňte odkazy **Home**, **About** a **Contact**.</span><span class="sxs-lookup"><span data-stu-id="56023-193">Remove the **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="56023-194">V následujícím příkladu odstraňte zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="56023-194">In the following example, delete the highlighted code.</span></span>

    ![][41]

7. <span data-ttu-id="56023-195">V Průzkumníku řešení rozbalte složku Views\Home, pak poklikejte na soubor **Index.cshtml** a ten se otevře v editoru Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="56023-195">In Solution Explorer, expand the Views\Home folder, then double-click **Index.cshtml** to open it in the Visual Studio editor.</span></span> <span data-ttu-id="56023-196">Celý obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="56023-196">Replace the entire contents of the file with the following code.</span></span>

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
8. <span data-ttu-id="56023-197">Pokud chcete zkontrolovat přesnost svojí dosavadní práce, můžete stisknout **Ctrl+Shift+B** a tím projekt sestavit.</span><span class="sxs-lookup"><span data-stu-id="56023-197">To verify the accuracy of your work so far, you can press **Ctrl+Shift+B** to build the project.</span></span>

### <a name="run-the-app-locally"></a><span data-ttu-id="56023-198">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="56023-198">Run the app locally</span></span>

<span data-ttu-id="56023-199">Spusťte aplikace pro kontrolu, že funguje.</span><span class="sxs-lookup"><span data-stu-id="56023-199">Run the application to verify that it works.</span></span>

1. <span data-ttu-id="56023-200">Zkontrolujte, že aktivní projekt je **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="56023-200">Ensure that **ProductsPortal** is the active project.</span></span> <span data-ttu-id="56023-201">V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a klikněte na možnost **Nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="56023-201">Right-click the project name in Solution Explorer and select **Set As Startup Project**.</span></span>
2. <span data-ttu-id="56023-202">V sadě Visual Studio stiskněte **F5**.</span><span class="sxs-lookup"><span data-stu-id="56023-202">In Visual Studio, press **F5**.</span></span>
3. <span data-ttu-id="56023-203">Měla by se objevit vaše aplikace, jak běží v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="56023-203">Your application should appear, running in a browser.</span></span>

   ![][21]

## <a name="put-the-pieces-together"></a><span data-ttu-id="56023-204">Složení částí do jednoho celku</span><span class="sxs-lookup"><span data-stu-id="56023-204">Put the pieces together</span></span>

<span data-ttu-id="56023-205">Dalším krokem je spojit lokální produktový server s aplikací ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="56023-205">The next step is to hook up the on-premises products server with the ASP.NET application.</span></span>

1. <span data-ttu-id="56023-206">Pokud v aplikaci Visual Studio není otevřený projekt **ProductsPortal**, který jste vytvořili v části [Vytvoření aplikace ASP.NET](#create-an-aspnet-application), znovu ho otevřete.</span><span class="sxs-lookup"><span data-stu-id="56023-206">If it is not already open, in Visual Studio re-open the **ProductsPortal** project you created in the [Create an ASP.NET application](#create-an-aspnet-application) section.</span></span>
2. <span data-ttu-id="56023-207">Podobně jako v části Vytvoření lokálního serveru přidejte do referencí projektu balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="56023-207">Similar to the step in the "Create an On-Premises Server" section, add the NuGet package to the project references.</span></span> <span data-ttu-id="56023-208">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ProductsPortal**, pak klikněte na **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="56023-208">In Solution Explorer, right-click the **ProductsPortal** project, then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="56023-209">Vyhledejte „Service Bus“ a vyberte položku **WindowsAzure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="56023-209">Search for "Service Bus" and select the **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="56023-210">Potom zavřete dialogové okno, tím dokončíte instalaci.</span><span class="sxs-lookup"><span data-stu-id="56023-210">Then complete the installation and close this dialog box.</span></span>
4. <span data-ttu-id="56023-211">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ProductsPortal**, pak klikněte na **Přidat** a pak na **Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="56023-211">In Solution Explorer, right-click the **ProductsPortal** project, then click **Add**, then **Existing Item**.</span></span>
5. <span data-ttu-id="56023-212">Přejděte na soubor **ProductsContract.cs** v konzolovém projektu **ProductsServer**.</span><span class="sxs-lookup"><span data-stu-id="56023-212">Navigate to the **ProductsContract.cs** file from the **ProductsServer** console project.</span></span> <span data-ttu-id="56023-213">Kliknutím zvýrazněte ProductsContract.cs.</span><span class="sxs-lookup"><span data-stu-id="56023-213">Click to highlight ProductsContract.cs.</span></span> <span data-ttu-id="56023-214">Klikněte na šipku dolů vedle tlačítka **Přidat**, pak klikněte na **Přidat jako odkaz**.</span><span class="sxs-lookup"><span data-stu-id="56023-214">Click the down arrow next to **Add**, then click **Add as Link**.</span></span>

   ![][24]

6. <span data-ttu-id="56023-215">Teď ve Visual Studiu otevřete soubor **HomeController.cs** a nahraďte definici oboru názvů následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="56023-215">Now open the **HomeController.cs** file in the Visual Studio editor and replace the namespace definition with the following code.</span></span> <span data-ttu-id="56023-216">Nezapomeňte místo *yourServiceNamespace* zadat název vašeho oboru názvů služby a místo *yourKey* váš SAS klíč.</span><span class="sxs-lookup"><span data-stu-id="56023-216">Be sure to replace *yourServiceNamespace* with the name of your service namespace, and *yourKey* with your SAS key.</span></span> <span data-ttu-id="56023-217">To klientovi umožní zavolat lokální službu a vrátit výsledek volání.</span><span class="sxs-lookup"><span data-stu-id="56023-217">This will enable the client to call the on-premises service, returning the result of the call.</span></span>

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
           // Declare the channel factory.
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
                   // Return a view of the products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. <span data-ttu-id="56023-218">V Průzkumníku řešení klikněte pravým tlačítkem na řešení **ProductsPortal** (klikněte skutečně na řešení, ne na projekt).</span><span class="sxs-lookup"><span data-stu-id="56023-218">In Solution Explorer, right-click the **ProductsPortal** solution (make sure to right-click the solution, not the project).</span></span> <span data-ttu-id="56023-219">Klikněte na **Přidat** a pak klikněte na **Existující projekt**.</span><span class="sxs-lookup"><span data-stu-id="56023-219">Click **Add**, then click **Existing Project**.</span></span>
8. <span data-ttu-id="56023-220">Přejděte do projektu **ProductsServer**, pak poklikejte na řešení **ProductsServer.csproj** a tím ho přidejte.</span><span class="sxs-lookup"><span data-stu-id="56023-220">Navigate to the **ProductsServer** project, then double-click the **ProductsServer.csproj** solution file to add it.</span></span>
9. <span data-ttu-id="56023-221">Aby se data mohla zobrazit na **ProductsPortal**, musí běžet **ProductsServer**.</span><span class="sxs-lookup"><span data-stu-id="56023-221">**ProductsServer** must be running in order to display the data on **ProductsPortal**.</span></span> <span data-ttu-id="56023-222">V Průzkumníku řešení klikněte pravým tlačítkem na řešení **ProductsPortal** a pak na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="56023-222">In Solution Explorer, right-click the **ProductsPortal** solution and click **Properties**.</span></span> <span data-ttu-id="56023-223">Zobrazí se dialogové okno **Stránky vlastností**.</span><span class="sxs-lookup"><span data-stu-id="56023-223">The **Property Pages** dialog box is displayed.</span></span>
10. <span data-ttu-id="56023-224">Na levé straně klikněte na **Spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="56023-224">On the left side, click **Startup Project**.</span></span> <span data-ttu-id="56023-225">Na pravé straně klikněte na **Více projektů po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="56023-225">On the right side, click **Multiple startup projects**.</span></span> <span data-ttu-id="56023-226">Zkontrolujte, že se objeví **ProductsServer** a **ProductsPortal** v tomto pořadí, a že je pro obojí nastavená akce **Start**.</span><span class="sxs-lookup"><span data-stu-id="56023-226">Ensure that **ProductsServer** and **ProductsPortal** appear, in that order, with **Start** set as the action for both.</span></span>

      ![][25]

11. <span data-ttu-id="56023-227">Pořád ještě v dialogovém okně **Vlastnosti** na levé straně klikněte na **Závislosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="56023-227">Still in the **Properties** dialog box, click **Project Dependencies** on the left side.</span></span>
12. <span data-ttu-id="56023-228">V seznamu **Projekty** klikněte na **ProductsServer**.</span><span class="sxs-lookup"><span data-stu-id="56023-228">In the **Projects** list, click **ProductsServer**.</span></span> <span data-ttu-id="56023-229">Zkontrolujte, že **ProductsPortal** není vybraný.</span><span class="sxs-lookup"><span data-stu-id="56023-229">Ensure that **ProductsPortal** is not selected.</span></span>
13. <span data-ttu-id="56023-230">V seznamu **Projekty** klikněte na **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="56023-230">In the **Projects** list, click **ProductsPortal**.</span></span> <span data-ttu-id="56023-231">Zkontrolujte, že je **ProductsServer** vybraný.</span><span class="sxs-lookup"><span data-stu-id="56023-231">Ensure that **ProductsServer** is selected.</span></span>

    ![][26]

14. <span data-ttu-id="56023-232">Kliknutím na **OK** zavřete se dialogové okno **Stránky vlastností**.</span><span class="sxs-lookup"><span data-stu-id="56023-232">Click **OK** in the **Property Pages** dialog box.</span></span>

## <a name="run-the-project-locally"></a><span data-ttu-id="56023-233">Spusťte projekt lokálně.</span><span class="sxs-lookup"><span data-stu-id="56023-233">Run the project locally</span></span>

<span data-ttu-id="56023-234">Aplikaci můžete lokálně spustit a otestovat ve Visual Studiu stisknutím klávesy **F5**.</span><span class="sxs-lookup"><span data-stu-id="56023-234">To test the application locally, in Visual Studio press **F5**.</span></span> <span data-ttu-id="56023-235">Nejdřív by se měl spustit lokální server (**ProductsServer**), potom by se v okně prohlížeče měla spustit aplikace **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="56023-235">The on-premises server (**ProductsServer**) should start first, then the **ProductsPortal** application should start in a browser window.</span></span> <span data-ttu-id="56023-236">Tentokrát uvidíte, že inventář produktů zobrazí seznam dat načtených z lokálního systému služby.</span><span class="sxs-lookup"><span data-stu-id="56023-236">This time, you will see that the product inventory lists data retrieved from the product service on-premises system.</span></span>

![][10]

<span data-ttu-id="56023-237">Na stránce **ProductsPortal** stiskněte **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="56023-237">Press **Refresh** on the **ProductsPortal** page.</span></span> <span data-ttu-id="56023-238">Pokaždé, když obnovíte stránku, měla by aplikace serveru při volání `GetProducts()` z **ProductsServer** zobrazit zprávu.</span><span class="sxs-lookup"><span data-stu-id="56023-238">Each time you refresh the page, you'll see the server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

<span data-ttu-id="56023-239">Před dalším krokem zavřete obě aplikace.</span><span class="sxs-lookup"><span data-stu-id="56023-239">Close both applications before proceeding to the next step.</span></span>

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a><span data-ttu-id="56023-240">Nasazení projektu ProductsPortal do webové aplikace Azure</span><span class="sxs-lookup"><span data-stu-id="56023-240">Deploy the ProductsPortal project to an Azure web app</span></span>

<span data-ttu-id="56023-241">Dalším krokem je nové publikování frontendu **ProductsPortal** webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="56023-241">The next step is to republish the Azure Web app **ProductsPortal** frontend.</span></span> <span data-ttu-id="56023-242">Udělejte toto:</span><span class="sxs-lookup"><span data-stu-id="56023-242">Do the following:</span></span>

1. <span data-ttu-id="56023-243">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **ProductsPortal** a klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="56023-243">In Solution Explorer, right-click the **ProductsPortal** project, and click **Publish**.</span></span> <span data-ttu-id="56023-244">Potom klikněte na **Publikovat** na stránce **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="56023-244">Then, click **Publish** on the **Publish** page.</span></span>

  > [!NOTE]
  > <span data-ttu-id="56023-245">V okně prohlížeče se při automatickém spuštění webového projektu **ProductsPortal** po nasazení může objevit chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="56023-245">You may see an error message in the browser window when the **ProductsPortal** web project is automatically launched after the deployment.</span></span> <span data-ttu-id="56023-246">To je v pořádku a děje se to, protože aplikace **ProductsServer** ještě neběží.</span><span class="sxs-lookup"><span data-stu-id="56023-246">This is expected, and occurs because the **ProductsServer** application isn't running yet.</span></span>
>
>

2. <span data-ttu-id="56023-247">Zkopírujte adresu URL nasazené aplikace protože ji budete potřebovat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="56023-247">Copy the URL of the deployed web app, as you will need the URL in the next step.</span></span> <span data-ttu-id="56023-248">Tuto adresu URL můžete taky získat z okna Aktivita služby Azure App Service ve Visual Studiu:</span><span class="sxs-lookup"><span data-stu-id="56023-248">You can also obtain this URL from the Azure App Service Activity window in Visual Studio:</span></span>

  ![][9]

3. <span data-ttu-id="56023-249">Zavřením okna prohlížeče zastavte spuštěnou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56023-249">Close the browser window to stop the running application.</span></span>

### <a name="set-productsportal-as-web-app"></a><span data-ttu-id="56023-250">Nastavení ProductsPortal jako webové aplikace</span><span class="sxs-lookup"><span data-stu-id="56023-250">Set ProductsPortal as web app</span></span>

<span data-ttu-id="56023-251">Než spustíte aplikaci v cloudu, musíte zkontrolovat, že se **ProductsPortal** spustí z Visual Studia jak webová aplikace.</span><span class="sxs-lookup"><span data-stu-id="56023-251">Before running the application in the cloud, you must ensure that **ProductsPortal** is launched from within Visual Studio as a web app.</span></span>

1. <span data-ttu-id="56023-252">V sadě Visual Studio klikněte pravým tlačítkem myši na projekt **ProductsPortal** a potom klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="56023-252">In Visual Studio, right-click the **ProductsPortal** project and then click **Properties**.</span></span>
2. <span data-ttu-id="56023-253">V levém sloupci klikněte na **Web**.</span><span class="sxs-lookup"><span data-stu-id="56023-253">In the left-hand column, click **Web**.</span></span>
3. <span data-ttu-id="56023-254">V části **Spustit akci** klikněte na tlačítko **Otevřít adresu URL** a do textového pole zadejte URL webové aplikace nasazené předtím, například `http://productsportal1234567890.azurewebsites.net/`.</span><span class="sxs-lookup"><span data-stu-id="56023-254">In the **Start Action** section, click the **Start URL** button, and in the text box enter the URL for your previously deployed web app; for example, `http://productsportal1234567890.azurewebsites.net/`.</span></span>

    ![][27]

4. <span data-ttu-id="56023-255">Ve Visual Studiu zvolte v nabídce **Soubor** možnost **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="56023-255">From the **File** menu in Visual Studio, click **Save All**.</span></span>
5. <span data-ttu-id="56023-256">Ve Visual Studiu zvolte v nabídce Sestavení možnost **Znovu sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="56023-256">From the Build menu in Visual Studio, click **Rebuild Solution**.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="56023-257">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="56023-257">Run the application</span></span>

1. <span data-ttu-id="56023-258">Stisknutím klávesy F5 aplikaci sestavíte a spustíte.</span><span class="sxs-lookup"><span data-stu-id="56023-258">Press F5 to build and run the application.</span></span> <span data-ttu-id="56023-259">Nejdřív by se měl spustit lokální server (konzolová aplikace **ProductsServer**), potom by se v okně prohlížeče měla spustit aplikace **ProductsPortal**, jak je vidět na tomto snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="56023-259">The on-premises server (the **ProductsServer** console application) should start first, then the **ProductsPortal** application should start in a browser window, as shown in the following screen shot.</span></span> <span data-ttu-id="56023-260">Znovu si všimněte, že inventář produktů zobrazí seznam dat načtených z lokálního systému služby a tato data zobrazí ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="56023-260">Notice again that the product inventory lists data retrieved from the product service on-premises system, and displays that data in the web app.</span></span> <span data-ttu-id="56023-261">Zkontrolujte adresu URL a ujistěte se, že **ProductsPortal** běží v cloudu jako webová aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="56023-261">Check the URL to make sure that **ProductsPortal** is running in the cloud, as an Azure web app.</span></span>

   ![][1]

   > [!IMPORTANT]
   > <span data-ttu-id="56023-262">Konzolová aplikace **ProductsServer** musí běžet a musí být schopná odesílat data do aplikace **ProductsPortal**.</span><span class="sxs-lookup"><span data-stu-id="56023-262">The **ProductsServer** console application must be running and able to serve the data to the **ProductsPortal** application.</span></span> <span data-ttu-id="56023-263">pokud prohlížeč zobrazí chybu, počkejte několik dalších sekund, než se načte **ProductsServer** a zobrazí se následující zpráva.</span><span class="sxs-lookup"><span data-stu-id="56023-263">If the browser displays an error, wait a few more seconds for **ProductsServer** to load and display the following message.</span></span> <span data-ttu-id="56023-264">Potom v prohlížeči stiskněte **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="56023-264">Then press **Refresh** in the browser.</span></span>
   >
   >

   ![][37]
2. <span data-ttu-id="56023-265">Zpátky v prohlížeči na stránce **ProductsPortal** stiskněte **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="56023-265">Back in the browser, press **Refresh** on the **ProductsPortal** page.</span></span> <span data-ttu-id="56023-266">Pokaždé, když obnovíte stránku, měla by aplikace serveru při volání `GetProducts()` z **ProductsServer** zobrazit zprávu.</span><span class="sxs-lookup"><span data-stu-id="56023-266">Each time you refresh the page, you'll see the server app display a message when `GetProducts()` from **ProductsServer** is called.</span></span>

    ![][38]

## <a name="next-steps"></a><span data-ttu-id="56023-267">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56023-267">Next steps</span></span>

<span data-ttu-id="56023-268">Pokud se o Azure Relay chcete dozvědět víc, pročtěte si následující zdroje:</span><span class="sxs-lookup"><span data-stu-id="56023-268">To learn more about Azure Relay, see the following resources:</span></span>  

* [<span data-ttu-id="56023-269">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="56023-269">What is Azure Relay?</span></span>](relay-what-is-it.md)  
* [<span data-ttu-id="56023-270">Jak používat Relay</span><span class="sxs-lookup"><span data-stu-id="56023-270">How to use Relay</span></span>](service-bus-dotnet-how-to-use-relay.md)  

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
