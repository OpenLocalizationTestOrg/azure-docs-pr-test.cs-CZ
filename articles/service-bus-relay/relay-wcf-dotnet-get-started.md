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
# <a name="how-toouse-azure-relay-wcf-relays-with-net"></a><span data-ttu-id="8a9f7-103">Jak předává toouse WCF předávání přes Azure pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="8a9f7-103">How toouse Azure Relay WCF relays with .NET</span></span>
<span data-ttu-id="8a9f7-104">Tento článek popisuje, jak toouse hello předávání přes Azure service.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-104">This article describes how toouse hello Azure Relay service.</span></span> <span data-ttu-id="8a9f7-105">Hello ukázky jsou napsané v C# a používají API Windows Communication Foundation (WCF) hello s rozšíření obsažená v hello sestavení Service Bus.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-105">hello samples are written in C# and use hello Windows Communication Foundation (WCF) API with extensions contained in hello Service Bus assembly.</span></span> <span data-ttu-id="8a9f7-106">Další informace o předávání přes Azure najdete v tématu hello [předávání přes Azure přehled](relay-what-is-it.md).</span><span class="sxs-lookup"><span data-stu-id="8a9f7-106">For more information about Azure relay, see hello [Azure Relay overview](relay-what-is-it.md).</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a><span data-ttu-id="8a9f7-107">Co je předávání WCF?</span><span class="sxs-lookup"><span data-stu-id="8a9f7-107">What is WCF Relay?</span></span>

<span data-ttu-id="8a9f7-108">Hello Azure [ *WCF předávání* ](relay-what-is-it.md) služba vám umožní toobuild hybridní aplikace, které poběží v datovém centru Azure i vlastní místní podnikovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-108">hello Azure [*WCF Relay*](relay-what-is-it.md) service enables you toobuild hybrid applications that run in both an Azure datacenter and your own on-premises enterprise environment.</span></span> <span data-ttu-id="8a9f7-109">Hello předávací služba docílí tak, že jste toosecurely vystavit služby Windows Communication Foundation (WCF), které se nacházejí v podnikové síti toohello veřejného cloudu, bez nutnosti tooopen spojení ve firewallu nebo nutnosti nežádoucí změny tooa podnikové síťové infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-109">hello relay service facilitates this by enabling you toosecurely expose Windows Communication Foundation (WCF) services that reside within a corporate enterprise network toohello public cloud, without having tooopen a firewall connection, or requiring intrusive changes tooa corporate network infrastructure.</span></span>

![Koncepty předávání WCF](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

<span data-ttu-id="8a9f7-111">Předávání přes Azure umožňuje toohost služby WCF ve vaší existující podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-111">Azure Relay enables you toohost WCF services within your existing enterprise environment.</span></span> <span data-ttu-id="8a9f7-112">Potom můžete delegovat čekání na příchozí relace a požadavky toothese služby toohello předávací služby WCF běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-112">You can then delegate listening for incoming sessions and requests toothese WCF services toohello relay service running within Azure.</span></span> <span data-ttu-id="8a9f7-113">To umožňuje tooexpose můžete tyto služby tooapplication kód spuštěný v Azure, nebo toomobile pracovníkům nebo partnerským prostředím v extranetu.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-113">This enables you tooexpose these services tooapplication code running in Azure, or toomobile workers or extranet partner environments.</span></span> <span data-ttu-id="8a9f7-114">Předávání umožňuje toosecurely řízení, kdo má přístup k těmto službám, na velice přesné úrovni.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-114">Relay enables you toosecurely control who can access these services at a fine-grained level.</span></span> <span data-ttu-id="8a9f7-115">Poskytuje výkonný a bezpečný způsob tooexpose aplikaci funkce a data z existujících podnikových řešení a využít ji z cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-115">It provides a powerful and secure way tooexpose application functionality and data from your existing enterprise solutions and take advantage of it from hello cloud.</span></span>

<span data-ttu-id="8a9f7-116">Tento článek popisuje, jak předávání přes Azure toocreate toouse webové služby WCF vystavené pomocí vazeb kanálů TCP, které implementují zabezpečenou konverzaci mezi dvěma účastníky.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-116">This article discusses how toouse Azure Relay toocreate a WCF web service, exposed using a TCP channel binding, that implements a secure conversation between two parties.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-hello-service-bus-nuget-package"></a><span data-ttu-id="8a9f7-117">Načtení balíčku pro Service Bus NuGet hello</span><span class="sxs-lookup"><span data-stu-id="8a9f7-117">Get hello Service Bus NuGet package</span></span>
<span data-ttu-id="8a9f7-118">Hello [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) je hello nejjednodušší způsob, jak tooget hello rozhraní API služby Service Bus a tooconfigure aplikaci se všemi závislostmi služby Service Bus hello služby.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-118">hello [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is hello easiest way tooget hello Service Bus API and tooconfigure your application with all of hello Service Bus dependencies.</span></span> <span data-ttu-id="8a9f7-119">balíček NuGet hello tooinstall ve vašem projektu hello následující:</span><span class="sxs-lookup"><span data-stu-id="8a9f7-119">tooinstall hello NuGet package in your project, do hello following:</span></span>

1. <span data-ttu-id="8a9f7-120">V Průzkumníku řešení klikněte pravým tlačítkem na **Reference**, a pak klikněte na **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-120">In Solution Explorer, right-click **References**, then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="8a9f7-121">Vyhledejte "Service Bus" a vyberte hello **Microsoft Azure Service Bus** položky.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-121">Search for "Service Bus" and select hello **Microsoft Azure Service Bus** item.</span></span> <span data-ttu-id="8a9f7-122">Klikněte na tlačítko **nainstalovat** toocomplete hello instalace a pak zavřete následující dialogové okno hello:</span><span class="sxs-lookup"><span data-stu-id="8a9f7-122">Click **Install** toocomplete hello installation, then close hello following dialog box:</span></span>
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a><span data-ttu-id="8a9f7-123">Vystavení a spotřebování webové služby SOAP pomocí TCP</span><span class="sxs-lookup"><span data-stu-id="8a9f7-123">Expose and consume a SOAP web service with TCP</span></span>
<span data-ttu-id="8a9f7-124">tooexpose existující webovou službu WCF SOAP pro externí spotřebu, musíte nastavit adresách a vazbách služby toohello změny.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-124">tooexpose an existing WCF SOAP web service for external consumption, you must make changes toohello service bindings and addresses.</span></span> <span data-ttu-id="8a9f7-125">To může vyžadovat změny tooyour konfigurační soubor nebo jej může vyžadovat změny kódu, v závislosti na tom, jak jste nastavili a nakonfigurovali služby WCF.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-125">This may require changes tooyour configuration file or it could require code changes, depending on how you have set up and configured your WCF services.</span></span> <span data-ttu-id="8a9f7-126">Všimněte si, že WCF umožňuje toohave několik koncových bodů sítě přes hello stejnou službu, abyste mohli zachovat stávající hello vnitřních koncových bodů při přidávání předávání koncové body pro externí přístup na hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-126">Note that WCF allows you toohave multiple network endpoints over hello same service, so you can retain hello existing internal endpoints while adding relay endpoints for external access at hello same time.</span></span>

<span data-ttu-id="8a9f7-127">V této úloze sestavit jednoduchou službu WCF a přidejte tooit naslouchací proces předávání.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-127">In this task, you build a simple WCF service and add a relay listener tooit.</span></span> <span data-ttu-id="8a9f7-128">Toto cvičení předpokládá některé znalost Visual Studio a proto vás neprovede úplně všechny podrobnosti hello vytvoření projektu.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-128">This exercise assumes some familiarity with Visual Studio, and therefore does not walk through all hello details of creating a project.</span></span> <span data-ttu-id="8a9f7-129">Místo toho zaměřuje se na kód hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-129">Instead, it focuses on hello code.</span></span>

<span data-ttu-id="8a9f7-130">Před zahájením těchto kroků, proveďte následující postup tooset prostředí hello:</span><span class="sxs-lookup"><span data-stu-id="8a9f7-130">Before starting these steps, complete hello following procedure tooset up your environment:</span></span>

1. <span data-ttu-id="8a9f7-131">Ve Visual Studiu Vytvořte konzolovou aplikaci, která obsahuje dva projekty – "Client" a "Služba" v rámci řešení hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-131">Within Visual Studio, create a console application that contains two projects, "Client" and "Service", within hello solution.</span></span>
2. <span data-ttu-id="8a9f7-132">Přidejte projekty tooboth balíček Service Bus NuGet hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-132">Add hello Service Bus NuGet package tooboth projects.</span></span> <span data-ttu-id="8a9f7-133">Tento balíček přidá všechny projekty tooyour odkazy hello nezbytné sestavení.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-133">This package adds all hello necessary assembly references tooyour projects.</span></span>

### <a name="how-toocreate-hello-service"></a><span data-ttu-id="8a9f7-134">Jak toocreate hello služby</span><span class="sxs-lookup"><span data-stu-id="8a9f7-134">How toocreate hello service</span></span>
<span data-ttu-id="8a9f7-135">Nejprve vytvořte samotnou službu hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-135">First, create hello service itself.</span></span> <span data-ttu-id="8a9f7-136">Každá služba WCF se skládá nejméně ze tří různých částí:</span><span class="sxs-lookup"><span data-stu-id="8a9f7-136">Any WCF service consists of at least three distinct parts:</span></span>

* <span data-ttu-id="8a9f7-137">Definice kontraktu, která popisuje, jaké zprávy se vyměňují a jaké operace jsou toobe vyvolat.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-137">Definition of a contract that describes what messages are exchanged and what operations are toobe invoked.</span></span>
* <span data-ttu-id="8a9f7-138">Implementace tohoto kontraktu.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-138">Implementation of that contract.</span></span>
* <span data-ttu-id="8a9f7-139">Hostitele, který hostuje službu WCF hello a zpřístupňuje několik koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-139">Host that hosts hello WCF service and exposes several endpoints.</span></span>

<span data-ttu-id="8a9f7-140">Příklady kódu Hello v této části se vztahují na všechny tyto součásti.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-140">hello code examples in this section address each of these components.</span></span>

<span data-ttu-id="8a9f7-141">Hello kontrakt definuje jednu operaci `AddNumbers`, která sečte dvě čísla a vrátí výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-141">hello contract defines a single operation, `AddNumbers`, that adds two numbers and returns hello result.</span></span> <span data-ttu-id="8a9f7-142">Hello `IProblemSolverChannel` rozhraní umožňuje hello klienta toomore snadno spravovat hello doby platnosti proxy.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-142">hello `IProblemSolverChannel` interface enables hello client toomore easily manage hello proxy lifetime.</span></span> <span data-ttu-id="8a9f7-143">Vytvoření takového rozhraní se obvykle považuje za vhodné řešení.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-143">Creating such an interface is considered a best practice.</span></span> <span data-ttu-id="8a9f7-144">Je vhodné tooput této smlouvy definic do samostatného souboru, aby tento soubor můžete odkazovat z projektů "Client" i "Service", ale hello kódu můžete také zkopírovat do obou projektů.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-144">It's a good idea tooput this contract definition into a separate file so that you can reference that file from both your "Client" and "Service" projects, but you can also copy hello code into both projects.</span></span>

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

<span data-ttu-id="8a9f7-145">Implementace hello s hello kontrakt hotový, vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8a9f7-145">With hello contract in place, hello implementation is as follows:</span></span>

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a><span data-ttu-id="8a9f7-146">Programová konfigurace hostitele služby</span><span class="sxs-lookup"><span data-stu-id="8a9f7-146">Configure a service host programmatically</span></span>
<span data-ttu-id="8a9f7-147">S hello kontrakt a implementaci na místě můžete hostovat službu hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-147">With hello contract and implementation in place, you can now host hello service.</span></span> <span data-ttu-id="8a9f7-148">Hostování se provádí v [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) objektu, který se stará o správu instancí služby hello a hostitelé hello koncové body, které čekají na zprávy.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-148">Hosting occurs inside a [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) object, which takes care of managing instances of hello service and hosts hello endpoints that listen for messages.</span></span> <span data-ttu-id="8a9f7-149">Hello následující kód konfiguruje hello službu s běžným místním koncovým bodem a předávání přes koncový bod tooillustrate hello vzhled, vedle sebe, interních a externích koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-149">hello following code configures hello service with both a regular local endpoint and a relay endpoint tooillustrate hello appearance, side by side, of internal and external endpoints.</span></span> <span data-ttu-id="8a9f7-150">Nahraďte řetězec hello *obor názvů* názvem vašeho oboru názvů a *yourKey* hello SAS klíč, který jste získali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-150">Replace hello string *namespace* with your namespace name and *yourKey* with hello SAS key that you obtained in hello previous setup step.</span></span>

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

<span data-ttu-id="8a9f7-151">V příkladu hello vytvoříte dva koncové body, které jsou na hello stejné smlouvy implementace.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-151">In hello example, you create two endpoints that are on hello same contract implementation.</span></span> <span data-ttu-id="8a9f7-152">Jeden je místní a druhý je promítnutý přes předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-152">One is local and one is projected through Azure Relay.</span></span> <span data-ttu-id="8a9f7-153">Hello hlavní rozdíly mezi nimi jsou vazby hello; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) pro hello místní a [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) hello předávání koncového bodu a hello adresy.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-153">hello key differences between them are hello bindings; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) for hello local one and [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) for hello relay endpoint and hello addresses.</span></span> <span data-ttu-id="8a9f7-154">místní koncový bod Hello má místní síťovou adresu s vlastním portem.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-154">hello local endpoint has a local network address with a distinct port.</span></span> <span data-ttu-id="8a9f7-155">Hello předávání přes koncový bod má adresu koncového bodu složenou z řetězce hello `sb`, název oboru názvů a hello cesty "solver".</span><span class="sxs-lookup"><span data-stu-id="8a9f7-155">hello relay endpoint has an endpoint address composed of hello string `sb`, your namespace name, and hello path "solver."</span></span> <span data-ttu-id="8a9f7-156">Výsledkem je hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifikace hello koncový bod služby jako koncový bod Service Bus (relé) TCP s plně kvalifikovaným názvem externí DNS.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-156">This results in hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifying hello service endpoint as a Service Bus (relay) TCP endpoint with a fully qualified external DNS name.</span></span> <span data-ttu-id="8a9f7-157">Pokud umístíte hello kódu nahraďte zástupné symboly hello do hello `Main` funkce hello **služby** aplikace, budete mít funkční službu.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-157">If you place hello code replacing hello placeholders into hello `Main` function of hello **Service** application, you will have a functional service.</span></span> <span data-ttu-id="8a9f7-158">Pokud chcete, aby vaše služba toolisten výhradně na předávání hello, odeberte deklaraci místního koncového bodu hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-158">If you want your service toolisten exclusively on hello relay, remove hello local endpoint declaration.</span></span>

### <a name="configure-a-service-host-in-hello-appconfig-file"></a><span data-ttu-id="8a9f7-159">Konfigurace hostitele služby v souboru App.config hello</span><span class="sxs-lookup"><span data-stu-id="8a9f7-159">Configure a service host in hello App.config file</span></span>
<span data-ttu-id="8a9f7-160">Můžete také nakonfigurovat hello hostitele pomocí souboru App.config hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-160">You can also configure hello host using hello App.config file.</span></span> <span data-ttu-id="8a9f7-161">Hello služby hostování kódu v tomto případě se zobrazí v dalším příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-161">hello service hosting code in this case appears in hello next example.</span></span>

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();
sh.Close();
```

<span data-ttu-id="8a9f7-162">definice koncových bodů Hello přesunout do souboru App.config hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-162">hello endpoint definitions move into hello App.config file.</span></span> <span data-ttu-id="8a9f7-163">balíček NuGet Hello již přidán rozsah souboru App.config toohello definice, které jsou hello požadované konfigurace rozšíření pro předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-163">hello NuGet package has already added a range of definitions toohello App.config file, which are hello required configuration extensions for Azure Relay.</span></span> <span data-ttu-id="8a9f7-164">Následující příklad, který je hello přesný text Hello ekvivalentní hello předchozí kód, měl by být přímo pod hello **system.serviceModel** elementu.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-164">hello following example, which is hello exact equivalent of hello previous code, should appear directly beneath hello **system.serviceModel** element.</span></span> <span data-ttu-id="8a9f7-165">Tento příklad kódu předpokládá, že se obor názvů C# vašeho projektu jmenuje **Service**.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-165">This code example assumes that your project C# namespace is named **Service**.</span></span>
<span data-ttu-id="8a9f7-166">Nahraďte zástupné symboly hello předávání název oboru názvů a klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-166">Replace hello placeholders with your relay namespace name and SAS key.</span></span>

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

<span data-ttu-id="8a9f7-167">Po provedení těchto změn hello služba spustí jako předtím, ale s dva živé koncové body: jeden místní a jeden v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-167">After you make these changes, hello service starts as it did before, but with two live endpoints: one local and one listening in hello cloud.</span></span>

### <a name="create-hello-client"></a><span data-ttu-id="8a9f7-168">Vytvoření klienta hello</span><span class="sxs-lookup"><span data-stu-id="8a9f7-168">Create hello client</span></span>
#### <a name="configure-a-client-programmatically"></a><span data-ttu-id="8a9f7-169">Programová konfigurace klienta</span><span class="sxs-lookup"><span data-stu-id="8a9f7-169">Configure a client programmatically</span></span>
<span data-ttu-id="8a9f7-170">tooconsume hello služby, můžete vytvořit pomocí klienta WCF [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objektu.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-170">tooconsume hello service, you can construct a WCF client using a [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) object.</span></span> <span data-ttu-id="8a9f7-171">Service Bus používá tokenový model zabezpečení, implementovaný pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-171">Service Bus uses a token-based security model implemented using SAS.</span></span> <span data-ttu-id="8a9f7-172">Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) třída představuje poskytovatele tokenu zabezpečení se zabudovanými metodami pro vytváření vracející některé známé poskytovatele tokenů.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-172">hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) class represents a security token provider with built-in factory methods that return some well-known token providers.</span></span> <span data-ttu-id="8a9f7-173">Hello následující příklad používá hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metoda toohandle hello získání příslušného tokenu SAS hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-173">hello following example uses hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) method toohandle hello acquisition of hello appropriate SAS token.</span></span> <span data-ttu-id="8a9f7-174">Hello název a klíč se získá z portálu hello, jak je popsáno v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-174">hello name and key are those obtained from hello portal as described in hello previous section.</span></span>

<span data-ttu-id="8a9f7-175">První, odkaz nebo kopírování hello `IProblemSolver` kód ze služby hello kontraktu do vašeho klientského projektu.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-175">First, reference or copy hello `IProblemSolver` contract code from hello service into your client project.</span></span>

<span data-ttu-id="8a9f7-176">Pak nahraďte kód hello v hello `Main` metoda hello klienta, znovu nahraďte zástupný text hello předávání názvů a klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-176">Then, replace hello code in hello `Main` method of hello client, again replacing hello placeholder text with your relay namespace and SAS key.</span></span>

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

<span data-ttu-id="8a9f7-177">Teď můžete sestavit klienta hello a hello službu, spustit je (nejprve spustit službu hello), a hello klient zavolá službu hello a vytiskne **9**.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-177">You can now build hello client and hello service, run them (run hello service first), and hello client calls hello service and prints **9**.</span></span> <span data-ttu-id="8a9f7-178">Hello klienta a serveru můžete spustit na různých počítačích, i v rámci sítě a hello komunikace bude stále fungovat.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-178">You can run hello client and server on different machines, even across networks, and hello communication will still work.</span></span> <span data-ttu-id="8a9f7-179">Kód klienta Hello taky můžete spustit v hello cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-179">hello client code can also run in hello cloud or locally.</span></span>

#### <a name="configure-a-client-in-hello-appconfig-file"></a><span data-ttu-id="8a9f7-180">Konfigurace klienta v souboru App.config hello</span><span class="sxs-lookup"><span data-stu-id="8a9f7-180">Configure a client in hello App.config file</span></span>
<span data-ttu-id="8a9f7-181">Hello následující kód ukazuje, jak hello tooconfigure hello klienta pomocí souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-181">hello following code shows how tooconfigure hello client using hello App.config file.</span></span>

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

<span data-ttu-id="8a9f7-182">definice koncových bodů Hello přesunout do souboru App.config hello.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-182">hello endpoint definitions move into hello App.config file.</span></span> <span data-ttu-id="8a9f7-183">Hello následující příklad, který je hello stejný jako kód hello uvedených výše, měl by být přímo pod hello `<system.serviceModel>` elementu.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-183">hello following example, which is hello same as hello code listed previously, should appear directly beneath hello `<system.serviceModel>` element.</span></span> <span data-ttu-id="8a9f7-184">Zde jako předtím, je potřeba nahradit zástupné symboly hello předávání názvů a klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-184">Here, as before, you must replace hello placeholders with your relay namespace and SAS key.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8a9f7-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a9f7-185">Next steps</span></span>
<span data-ttu-id="8a9f7-186">Teď, když jste se naučili základy hello předávání přes Azure, použijte tyto odkazy toolearn Další.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-186">Now that you've learned hello basics of Azure Relay, follow these links toolearn more.</span></span>

* [<span data-ttu-id="8a9f7-187">Co je Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="8a9f7-187">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="8a9f7-188">Přehled architektury služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="8a9f7-188">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* <span data-ttu-id="8a9f7-189">Stáhněte si ukázky pro Service Bus z [ukázek Azure] [ Azure samples] nebo najdete hello [přehled ukázek pro Service Bus][overview of Service Bus samples].</span><span class="sxs-lookup"><span data-stu-id="8a9f7-189">Download Service Bus samples from [Azure samples][Azure samples] or see hello [overview of Service Bus samples][overview of Service Bus samples].</span></span>

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
