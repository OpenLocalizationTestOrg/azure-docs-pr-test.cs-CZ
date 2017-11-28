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
# <a name="how-to-use-azure-relay-wcf-relays-with-net"></a><span data-ttu-id="b8e03-103">Jak používat Azure předávání WCF předává s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="b8e03-103">How to use Azure Relay WCF relays with .NET</span></span>
<span data-ttu-id="b8e03-104">Tento článek popisuje, jak používat službu předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="b8e03-104">This article describes how to use the Azure Relay service.</span></span> <span data-ttu-id="b8e03-105">Ukázky jsou napsané v C# a používají API Windows Communication Foundation (WCF) s rozšířeními, které jsou součástí sestavení Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b8e03-105">The samples are written in C# and use the Windows Communication Foundation (WCF) API with extensions contained in the Service Bus assembly.</span></span> <span data-ttu-id="b8e03-106">Další informace o předávání přes Azure najdete v tématu [předávání přes Azure přehled](relay-what-is-it.md).</span><span class="sxs-lookup"><span data-stu-id="b8e03-106">For more information about Azure relay, see the [Azure Relay overview](relay-what-is-it.md).</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a><span data-ttu-id="b8e03-107">Co je předávání WCF?</span><span class="sxs-lookup"><span data-stu-id="b8e03-107">What is WCF Relay?</span></span>

<span data-ttu-id="b8e03-108">Azure [ *WCF předávání* ](relay-what-is-it.md) služba vám umožní sestavovat hybridní aplikace, které poběží v datovém centru Azure i vlastní místní podnikovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="b8e03-108">The Azure [*WCF Relay*](relay-what-is-it.md) service enables you to build hybrid applications that run in both an Azure datacenter and your own on-premises enterprise environment.</span></span> <span data-ttu-id="b8e03-109">Předávací službou docílí tak, že vám umožní bezpečně vystavit služby Windows Communication Foundation (WCF), které se nacházejí v podnikové síti, veřejnému cloudu, bez nutnosti otevřít spojení ve firewallu nebo vyžadování nežádoucí změny v infrastruktuře podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="b8e03-109">The relay service facilitates this by enabling you to securely expose Windows Communication Foundation (WCF) services that reside within a corporate enterprise network to the public cloud, without having to open a firewall connection, or requiring intrusive changes to a corporate network infrastructure.</span></span>

![Koncepty předávání WCF](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

<span data-ttu-id="b8e03-111">Předávání přes Azure vám umožňuje hostovat služby WCF ve vaší existující podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="b8e03-111">Azure Relay enables you to host WCF services within your existing enterprise environment.</span></span> <span data-ttu-id="b8e03-112">Potom můžete delegovat čekání na příchozí relace a požadavky na tyto služby WCF do předávací služby běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="b8e03-112">You can then delegate listening for incoming sessions and requests to these WCF services to the relay service running within Azure.</span></span> <span data-ttu-id="b8e03-113">To vám umožní vystavit tyto služby aplikačnímu kódu běžícímu v Azure nebo mobilním pracovníkům nebo partnerským prostředím v extranetu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-113">This enables you to expose these services to application code running in Azure, or to mobile workers or extranet partner environments.</span></span> <span data-ttu-id="b8e03-114">Předávání vám umožňuje bezpečně řídit, kdo má přístup k těmto službám, na velice přesné úrovni.</span><span class="sxs-lookup"><span data-stu-id="b8e03-114">Relay enables you to securely control who can access these services at a fine-grained level.</span></span> <span data-ttu-id="b8e03-115">Nabízí výkonný a bezpečný způsob, jak vystavit data a funkce aplikací z existujících podnikových řešení a využívat jejich výhod z cloudu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-115">It provides a powerful and secure way to expose application functionality and data from your existing enterprise solutions and take advantage of it from the cloud.</span></span>

<span data-ttu-id="b8e03-116">Tento článek popisuje, jak používat předávání přes Azure k vytvoření webové služby WCF vystavené pomocí kanálu TCP vazby, které implementují zabezpečenou konverzaci mezi dvěma účastníky.</span><span class="sxs-lookup"><span data-stu-id="b8e03-116">This article discusses how to use Azure Relay to create a WCF web service, exposed using a TCP channel binding, that implements a secure conversation between two parties.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a><span data-ttu-id="b8e03-117">Získání balíčku Service Bus NuGet</span><span class="sxs-lookup"><span data-stu-id="b8e03-117">Get the Service Bus NuGet package</span></span>
<span data-ttu-id="b8e03-118">Nejsnadnějším způsobem, jak odkazovat na rozhraní API služby Service Bus a nakonfigurovat svoji aplikaci se všemi závislostmi služby Service Bus, je balíček [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="b8e03-118">The [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is the easiest way to get the Service Bus API and to configure your application with all of the Service Bus dependencies.</span></span> <span data-ttu-id="b8e03-119">Balíček NuGet můžete do svého projektu nainstalovat takto:</span><span class="sxs-lookup"><span data-stu-id="b8e03-119">To install the NuGet package in your project, do the following:</span></span>

1. <span data-ttu-id="b8e03-120">V Průzkumníku řešení klikněte pravým tlačítkem na **Reference**, a pak klikněte na **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b8e03-120">In Solution Explorer, right-click **References**, then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="b8e03-121">Vyhledejte „Service Bus“ a vyberte položku **Microsoft Azure Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="b8e03-121">Search for "Service Bus" and select the **Microsoft Azure Service Bus** item.</span></span> <span data-ttu-id="b8e03-122">Klikněte na **Instalovat** a dokončete instalaci, pak zavřete následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="b8e03-122">Click **Install** to complete the installation, then close the following dialog box:</span></span>
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a><span data-ttu-id="b8e03-123">Vystavení a spotřebování webové služby SOAP pomocí TCP</span><span class="sxs-lookup"><span data-stu-id="b8e03-123">Expose and consume a SOAP web service with TCP</span></span>
<span data-ttu-id="b8e03-124">Pokud chcete existující webovou službu WCF SOAP vystavit pro externí spotřebu, musíte udělat několik změn v adresách a vazbách služby.</span><span class="sxs-lookup"><span data-stu-id="b8e03-124">To expose an existing WCF SOAP web service for external consumption, you must make changes to the service bindings and addresses.</span></span> <span data-ttu-id="b8e03-125">K tomu může být potřeba změnit konfigurační soubor nebo kód, podle toho, jak máte služby WCF vytvořené a nastavené.</span><span class="sxs-lookup"><span data-stu-id="b8e03-125">This may require changes to your configuration file or it could require code changes, depending on how you have set up and configured your WCF services.</span></span> <span data-ttu-id="b8e03-126">Všimněte si, že WCF umožňuje mít několik koncových bodů sítě v jedné službě, abyste mohli zachovat stávající vnitřních koncových bodů při přidávání předávání koncové body pro externí přístup ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-126">Note that WCF allows you to have multiple network endpoints over the same service, so you can retain the existing internal endpoints while adding relay endpoints for external access at the same time.</span></span>

<span data-ttu-id="b8e03-127">V této úloze sestavit jednoduchou službu WCF a přidejte do ní naslouchací proces předávání.</span><span class="sxs-lookup"><span data-stu-id="b8e03-127">In this task, you build a simple WCF service and add a relay listener to it.</span></span> <span data-ttu-id="b8e03-128">Toto cvičení předpokládá, že umíte do jisté míry pracovat s Visual Studiem, a proto vás neprovede úplně všemi podrobnými kroky pro vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-128">This exercise assumes some familiarity with Visual Studio, and therefore does not walk through all the details of creating a project.</span></span> <span data-ttu-id="b8e03-129">Naopak se zaměřuje na kód.</span><span class="sxs-lookup"><span data-stu-id="b8e03-129">Instead, it focuses on the code.</span></span>

<span data-ttu-id="b8e03-130">Než s těmito kroky začnete, dokončete následující postup a nastavte prostředí:</span><span class="sxs-lookup"><span data-stu-id="b8e03-130">Before starting these steps, complete the following procedure to set up your environment:</span></span>

1. <span data-ttu-id="b8e03-131">Ve Visual Studiu vytvořte konzolovou aplikaci, která v řešení bude obsahovat dva projekty – „Client“ a „Service“.</span><span class="sxs-lookup"><span data-stu-id="b8e03-131">Within Visual Studio, create a console application that contains two projects, "Client" and "Service", within the solution.</span></span>
2. <span data-ttu-id="b8e03-132">Do obou projektů přidejte balíček Service Bus NuGet.</span><span class="sxs-lookup"><span data-stu-id="b8e03-132">Add the Service Bus NuGet package to both projects.</span></span> <span data-ttu-id="b8e03-133">Projekty díky němu budou mít všechny potřebné reference k sestavení.</span><span class="sxs-lookup"><span data-stu-id="b8e03-133">This package adds all the necessary assembly references to your projects.</span></span>

### <a name="how-to-create-the-service"></a><span data-ttu-id="b8e03-134">Jak vytvořit službu</span><span class="sxs-lookup"><span data-stu-id="b8e03-134">How to create the service</span></span>
<span data-ttu-id="b8e03-135">Nejdřív vytvořte službu samotnou.</span><span class="sxs-lookup"><span data-stu-id="b8e03-135">First, create the service itself.</span></span> <span data-ttu-id="b8e03-136">Každá služba WCF se skládá nejméně ze tří různých částí:</span><span class="sxs-lookup"><span data-stu-id="b8e03-136">Any WCF service consists of at least three distinct parts:</span></span>

* <span data-ttu-id="b8e03-137">Definice kontraktu, která popisuje, které zprávy se vyměňují a jaké operace se mají vyvolávat.</span><span class="sxs-lookup"><span data-stu-id="b8e03-137">Definition of a contract that describes what messages are exchanged and what operations are to be invoked.</span></span>
* <span data-ttu-id="b8e03-138">Implementace tohoto kontraktu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-138">Implementation of that contract.</span></span>
* <span data-ttu-id="b8e03-139">Hostitel, který hostuje službu WCF a zveřejňuje několik koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="b8e03-139">Host that hosts the WCF service and exposes several endpoints.</span></span>

<span data-ttu-id="b8e03-140">Příklady kódu v této části se vztahují na všechny části.</span><span class="sxs-lookup"><span data-stu-id="b8e03-140">The code examples in this section address each of these components.</span></span>

<span data-ttu-id="b8e03-141">Kontrakt definuje jednu operaci `AddNumbers`, která sečte dvě čísla a vrátí výsledek.</span><span class="sxs-lookup"><span data-stu-id="b8e03-141">The contract defines a single operation, `AddNumbers`, that adds two numbers and returns the result.</span></span> <span data-ttu-id="b8e03-142">Rozhraní `IProblemSolverChannel` umožní klientovi snadnější správu doby platnosti proxy.</span><span class="sxs-lookup"><span data-stu-id="b8e03-142">The `IProblemSolverChannel` interface enables the client to more easily manage the proxy lifetime.</span></span> <span data-ttu-id="b8e03-143">Vytvoření takového rozhraní se obvykle považuje za vhodné řešení.</span><span class="sxs-lookup"><span data-stu-id="b8e03-143">Creating such an interface is considered a best practice.</span></span> <span data-ttu-id="b8e03-144">Definici kontraktu se taky doporučuje dát do samostatného souboru, aby se mohla používat jako reference v projektech „Client“ i „Service“, ale kód můžete taky jednoduše zkopírovat do obou projektů.</span><span class="sxs-lookup"><span data-stu-id="b8e03-144">It's a good idea to put this contract definition into a separate file so that you can reference that file from both your "Client" and "Service" projects, but you can also copy the code into both projects.</span></span>

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

<span data-ttu-id="b8e03-145">Implementace s je kontrakt hotový, vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="b8e03-145">With the contract in place, the implementation is as follows:</span></span>

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a><span data-ttu-id="b8e03-146">Programová konfigurace hostitele služby</span><span class="sxs-lookup"><span data-stu-id="b8e03-146">Configure a service host programmatically</span></span>
<span data-ttu-id="b8e03-147">Když je kontrakt hotový a implementovaný, můžete hostovat službu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-147">With the contract and implementation in place, you can now host the service.</span></span> <span data-ttu-id="b8e03-148">Hostování se provádí v objektu [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx), který se stará o správu instancí služby a hostuje koncové bodu, které čekají na zprávy.</span><span class="sxs-lookup"><span data-stu-id="b8e03-148">Hosting occurs inside a [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) object, which takes care of managing instances of the service and hosts the endpoints that listen for messages.</span></span> <span data-ttu-id="b8e03-149">Následující kód konfiguruje službu s běžným místním koncovým bodem a koncový bod předávání pro ilustraci vzhled vedle sebe, interních a externích koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="b8e03-149">The following code configures the service with both a regular local endpoint and a relay endpoint to illustrate the appearance, side by side, of internal and external endpoints.</span></span> <span data-ttu-id="b8e03-150">Řetězec *namespace* nahraďte názvem vašeho oboru názvů a *yourKey* klíčem SAS, které jste získali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="b8e03-150">Replace the string *namespace* with your namespace name and *yourKey* with the SAS key that you obtained in the previous setup step.</span></span>

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

<span data-ttu-id="b8e03-151">V příkladu vytvoříte dva koncové body, které jsou na stejné implementaci kontraktu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-151">In the example, you create two endpoints that are on the same contract implementation.</span></span> <span data-ttu-id="b8e03-152">Jeden je místní a druhý je promítnutý přes předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="b8e03-152">One is local and one is projected through Azure Relay.</span></span> <span data-ttu-id="b8e03-153">Hlavní rozdíly mezi nimi jsou vazby: [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) pro místní a [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) pro koncový bod předávání a adresy.</span><span class="sxs-lookup"><span data-stu-id="b8e03-153">The key differences between them are the bindings; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) for the local one and [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) for the relay endpoint and the addresses.</span></span> <span data-ttu-id="b8e03-154">Místní koncový bod má místní síťovou adresu s vlastním portem.</span><span class="sxs-lookup"><span data-stu-id="b8e03-154">The local endpoint has a local network address with a distinct port.</span></span> <span data-ttu-id="b8e03-155">Koncový bod předávání má adresu koncového bodu složenou z řetězce `sb`, název vašeho oboru názvů a cesty "solver".</span><span class="sxs-lookup"><span data-stu-id="b8e03-155">The relay endpoint has an endpoint address composed of the string `sb`, your namespace name, and the path "solver."</span></span> <span data-ttu-id="b8e03-156">Výsledkem je identifikátor URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifikace koncový bod služby jako koncový bod Service Bus (relé) TCP s plně kvalifikovaným názvem externí DNS.</span><span class="sxs-lookup"><span data-stu-id="b8e03-156">This results in the URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifying the service endpoint as a Service Bus (relay) TCP endpoint with a fully qualified external DNS name.</span></span> <span data-ttu-id="b8e03-157">Pokud do kódu vložíte skutečné názvy místo zástupných názvů a vložíte ho do funkce `Main` aplikace **Service**, budete mít funkční službu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-157">If you place the code replacing the placeholders into the `Main` function of the **Service** application, you will have a functional service.</span></span> <span data-ttu-id="b8e03-158">Pokud chcete, aby vaše služba naslouchala jen na předávací službu, odstraňte deklaraci místního koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-158">If you want your service to listen exclusively on the relay, remove the local endpoint declaration.</span></span>

### <a name="configure-a-service-host-in-the-appconfig-file"></a><span data-ttu-id="b8e03-159">Konfigurace hostitele služby v souboru App.config</span><span class="sxs-lookup"><span data-stu-id="b8e03-159">Configure a service host in the App.config file</span></span>
<span data-ttu-id="b8e03-160">Hostitele taky můžete konfigurovat pomocí souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="b8e03-160">You can also configure the host using the App.config file.</span></span> <span data-ttu-id="b8e03-161">V tomto případě bude kód pro hostování vypadat jako kód v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-161">The service hosting code in this case appears in the next example.</span></span>

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

<span data-ttu-id="b8e03-162">Definice koncových bodů se přesunou do souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="b8e03-162">The endpoint definitions move into the App.config file.</span></span> <span data-ttu-id="b8e03-163">Balíček NuGet už přidal velké množství definic do souboru App.config, které jsou potřeba pro rozšíření konfigurace pro předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="b8e03-163">The NuGet package has already added a range of definitions to the App.config file, which are the required configuration extensions for Azure Relay.</span></span> <span data-ttu-id="b8e03-164">Následující příklad je naprosto stejný jako předchozí kód a měl by být hned pod elementem **system.serviceModel**.</span><span class="sxs-lookup"><span data-stu-id="b8e03-164">The following example, which is the exact equivalent of the previous code, should appear directly beneath the **system.serviceModel** element.</span></span> <span data-ttu-id="b8e03-165">Tento příklad kódu předpokládá, že se obor názvů C# vašeho projektu jmenuje **Service**.</span><span class="sxs-lookup"><span data-stu-id="b8e03-165">This code example assumes that your project C# namespace is named **Service**.</span></span>
<span data-ttu-id="b8e03-166">Nahraďte zástupné symboly předávání název oboru názvů a klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="b8e03-166">Replace the placeholders with your relay namespace name and SAS key.</span></span>

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

<span data-ttu-id="b8e03-167">Když provedete změny, služba se spustí jako předtím, ale tentokrát budou dva živé koncové body: jeden místní a jeden v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-167">After you make these changes, the service starts as it did before, but with two live endpoints: one local and one listening in the cloud.</span></span>

### <a name="create-the-client"></a><span data-ttu-id="b8e03-168">Vytvoření klienta</span><span class="sxs-lookup"><span data-stu-id="b8e03-168">Create the client</span></span>
#### <a name="configure-a-client-programmatically"></a><span data-ttu-id="b8e03-169">Programová konfigurace klienta</span><span class="sxs-lookup"><span data-stu-id="b8e03-169">Configure a client programmatically</span></span>
<span data-ttu-id="b8e03-170">Aby se služba mohla spotřebovávat, musíte postavit klienta WCF pomocí objektu [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8e03-170">To consume the service, you can construct a WCF client using a [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) object.</span></span> <span data-ttu-id="b8e03-171">Service Bus používá tokenový model zabezpečení, implementovaný pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="b8e03-171">Service Bus uses a token-based security model implemented using SAS.</span></span> <span data-ttu-id="b8e03-172">Třída [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření, které vracení některé známé poskytovatele tokenů.</span><span class="sxs-lookup"><span data-stu-id="b8e03-172">The [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) class represents a security token provider with built-in factory methods that return some well-known token providers.</span></span> <span data-ttu-id="b8e03-173">Následující příklad používá metodu [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) ke zpracování získání příslušného tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="b8e03-173">The following example uses the [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) method to handle the acquisition of the appropriate SAS token.</span></span> <span data-ttu-id="b8e03-174">Název a klíč se získá z portálu způsobem popsaným v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="b8e03-174">The name and key are those obtained from the portal as described in the previous section.</span></span>

<span data-ttu-id="b8e03-175">Nejdřív zkopírujte nebo odkažte na kód kontraktu `IProblemSolver` ze služby do vašeho klientského projektu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-175">First, reference or copy the `IProblemSolver` contract code from the service into your client project.</span></span>

<span data-ttu-id="b8e03-176">Pak nahraďte kód v `Main` metoda klienta, znovu nahraďte zástupný text předávání názvů a klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="b8e03-176">Then, replace the code in the `Main` method of the client, again replacing the placeholder text with your relay namespace and SAS key.</span></span>

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

<span data-ttu-id="b8e03-177">Teď můžete sestavit klienta a službu, spustit je (službu jako první) a klient zavolá službu a vypíše **9**.</span><span class="sxs-lookup"><span data-stu-id="b8e03-177">You can now build the client and the service, run them (run the service first), and the client calls the service and prints **9**.</span></span> <span data-ttu-id="b8e03-178">Klient a server můžete spustit na různých počítačích, dokonce i v jiných sítích, a komunikace bude stále fungovat.</span><span class="sxs-lookup"><span data-stu-id="b8e03-178">You can run the client and server on different machines, even across networks, and the communication will still work.</span></span> <span data-ttu-id="b8e03-179">Kód klienta se taky může spustit v cloudu nebo lokálně.</span><span class="sxs-lookup"><span data-stu-id="b8e03-179">The client code can also run in the cloud or locally.</span></span>

#### <a name="configure-a-client-in-the-appconfig-file"></a><span data-ttu-id="b8e03-180">Konfigurace klienta v souboru App.config</span><span class="sxs-lookup"><span data-stu-id="b8e03-180">Configure a client in the App.config file</span></span>
<span data-ttu-id="b8e03-181">Následující kód ukazuje, jak nakonfigurovat klienta pomocí souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="b8e03-181">The following code shows how to configure the client using the App.config file.</span></span>

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

<span data-ttu-id="b8e03-182">Definice koncových bodů se přesunou do souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="b8e03-182">The endpoint definitions move into the App.config file.</span></span> <span data-ttu-id="b8e03-183">Následující příklad, což je stejný jako kód uvedených výše, měl by být přímo pod `<system.serviceModel>` elementu.</span><span class="sxs-lookup"><span data-stu-id="b8e03-183">The following example, which is the same as the code listed previously, should appear directly beneath the `<system.serviceModel>` element.</span></span> <span data-ttu-id="b8e03-184">Zde jako předtím, je potřeba nahradit zástupné symboly předávání názvů a klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="b8e03-184">Here, as before, you must replace the placeholders with your relay namespace and SAS key.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b8e03-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8e03-185">Next steps</span></span>
<span data-ttu-id="b8e03-186">Teď, když jste se naučili základy používání služby předávání přes Azure, postupujte podle následujících odkazech na další informace.</span><span class="sxs-lookup"><span data-stu-id="b8e03-186">Now that you've learned the basics of Azure Relay, follow these links to learn more.</span></span>

* [<span data-ttu-id="b8e03-187">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="b8e03-187">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="b8e03-188">Přehled architektury služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="b8e03-188">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* <span data-ttu-id="b8e03-189">Stáhněte si ukázky pro Service Bus z [ukázek Azure] [ Azure samples] nebo se podívejte [přehled ukázek pro Service Bus][overview of Service Bus samples].</span><span class="sxs-lookup"><span data-stu-id="b8e03-189">Download Service Bus samples from [Azure samples][Azure samples] or see the [overview of Service Bus samples][overview of Service Bus samples].</span></span>

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
