---
title: "Kurz pro Azure předávání přes Service Bus WCF | Microsoft Docs"
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
ms.openlocfilehash: 5347bf85cad32b59677369d51a1f36529aef6662
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-wcf-relay-tutorial"></a><span data-ttu-id="d70ca-103">Kurz pro Azure předávání WCF</span><span class="sxs-lookup"><span data-stu-id="d70ca-103">Azure WCF Relay tutorial</span></span>

<span data-ttu-id="d70ca-104">Tento kurz popisuje, jak sestavit jednoduchý klient WCF předávání aplikace a služby pomocí předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="d70ca-104">This tutorial describes how to build a simple WCF Relay client application and service using Azure Relay.</span></span> <span data-ttu-id="d70ca-105">Podobný kurz, který používá [zasílání zpráv Service Bus](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), najdete v části [začít pracovat s fronty Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="d70ca-105">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="d70ca-106">Absolvování tohoto kurzu pochopíte kroky, které jsou potřeba k vytvoření aplikace klienta a služby WCF předávání.</span><span class="sxs-lookup"><span data-stu-id="d70ca-106">Working through this tutorial gives you an understanding of the steps that are required to create a WCF Relay client and service application.</span></span> <span data-ttu-id="d70ca-107">Stejně jako jejich původní protějšky WCF je služba konstrukce, která vystavuje jeden nebo více koncových bodů, každý z nich vystavuje jednu nebo víc operací služeb.</span><span class="sxs-lookup"><span data-stu-id="d70ca-107">Like their original WCF counterparts, a service is a construct that exposes one or more endpoints, each of which exposes one or more service operations.</span></span> <span data-ttu-id="d70ca-108">Koncový bod služby specifikuje adresu, kde se dá služba najít, vazbu, která obsahuje informaci, že klient musí komunikovat se službou, a kontrakt, který definuje funkci, kterou služba klientovi poskytuje.</span><span class="sxs-lookup"><span data-stu-id="d70ca-108">The endpoint of a service specifies an address where the service can be found, a binding that contains the information that a client must communicate with the service, and a contract that defines the functionality provided by the service to its clients.</span></span> <span data-ttu-id="d70ca-109">Hlavní rozdíl mezi WCF a předávací WCF je, že koncový bod vystavený v cloudu, a ne místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d70ca-109">The main difference between WCF and WCF Relay is that the endpoint is exposed in the cloud instead of locally on your computer.</span></span>

<span data-ttu-id="d70ca-110">Po dokončení řady témat v tomto kurzu budete mít funkční službu a klienta, který může vyvolat operace této služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-110">After you work through the sequence of topics in this tutorial, you will have a running service, and a client that can invoke the operations of the service.</span></span> <span data-ttu-id="d70ca-111">První téma popisuje, jak vytvořit účet.</span><span class="sxs-lookup"><span data-stu-id="d70ca-111">The first topic describes how to set up an account.</span></span> <span data-ttu-id="d70ca-112">V dalších krocích se popisuje, jak definovat službu, která používá kontrakt, jak tuto službu implementovat a jak službu konfigurovat pomocí kódu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-112">The next steps describe how to define a service that uses a contract, how to implement the service, and how to configure the service in code.</span></span> <span data-ttu-id="d70ca-113">Taky se v nich popisuje, jak hostovat a spustit službu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-113">They also describe how to host and run the service.</span></span> <span data-ttu-id="d70ca-114">Vytvořená služba se hostuje sama a klient a služba běží na stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="d70ca-114">The service that is created is self-hosted and the client and service run on the same computer.</span></span> <span data-ttu-id="d70ca-115">Službu můžete konfigurovat pomocí kódu nebo konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="d70ca-115">You can configure the service by using either code or a configuration file.</span></span>

<span data-ttu-id="d70ca-116">Poslední tři kroky popisují, jak vytvořit klientskou aplikaci, nakonfigurovat klientskou aplikaci a vytvořit a použít klienta, který může přistupovat k funkcím hostitele.</span><span class="sxs-lookup"><span data-stu-id="d70ca-116">The final three steps describe how to create a client application, configure the client application, and create and use a client that can access the functionality of the host.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d70ca-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d70ca-117">Prerequisites</span></span>

<span data-ttu-id="d70ca-118">K absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="d70ca-118">To complete this tutorial, you'll need the following:</span></span>

* <span data-ttu-id="d70ca-119">[Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="d70ca-119">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="d70ca-120">Tento kurz používá Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d70ca-120">This tutorial uses Visual Studio 2017.</span></span>
* <span data-ttu-id="d70ca-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d70ca-121">An active Azure account.</span></span> <span data-ttu-id="d70ca-122">Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut.</span><span class="sxs-lookup"><span data-stu-id="d70ca-122">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="d70ca-123">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="d70ca-123">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="d70ca-124">Vytvoření oboru názvů služby</span><span class="sxs-lookup"><span data-stu-id="d70ca-124">Create a service namespace</span></span>

<span data-ttu-id="d70ca-125">Prvním krokem je vytvoření oboru názvů a získat [sdíleného přístupového podpisu (SAS)](../service-bus-messaging/service-bus-sas.md) klíč.</span><span class="sxs-lookup"><span data-stu-id="d70ca-125">The first step is to create a namespace, and to obtain a [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) key.</span></span> <span data-ttu-id="d70ca-126">Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu předávání.</span><span class="sxs-lookup"><span data-stu-id="d70ca-126">A namespace provides an application boundary for each application exposed through the relay service.</span></span> <span data-ttu-id="d70ca-127">Systém automaticky vygeneruje SAS klíč při vytvoření oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-127">A SAS key is automatically generated by the system when a service namespace is created.</span></span> <span data-ttu-id="d70ca-128">Kombinace oboru názvů služby a klíče SAS poskytuje pověření, kterým Azure k ověření přístupu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d70ca-128">The combination of service namespace and SAS key provides the credentials for Azure to authenticate access to an application.</span></span> <span data-ttu-id="d70ca-129">Pokud chcete vytvořit obor názvů Relay, postupujte podle [těchto pokynů](relay-create-namespace-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d70ca-129">Follow the [instructions here](relay-create-namespace-portal.md) to create a Relay namespace.</span></span>

## <a name="define-a-wcf-service-contract"></a><span data-ttu-id="d70ca-130">Definování kontraktu služby WCF</span><span class="sxs-lookup"><span data-stu-id="d70ca-130">Define a WCF service contract</span></span>

<span data-ttu-id="d70ca-131">Kontrakt služby specifikuje, jaké operace (termín webových služeb pro metody nebo funkce) služba podporuje.</span><span class="sxs-lookup"><span data-stu-id="d70ca-131">The service contract specifies what operations (the web service terminology for methods or functions) the service supports.</span></span> <span data-ttu-id="d70ca-132">Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d70ca-132">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="d70ca-133">Každá metoda v rozhraní odpovídá konkrétní operaci služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-133">Each method in the interface corresponds to a specific service operation.</span></span> <span data-ttu-id="d70ca-134">Na každé rozhraní musí mít aplikovaný atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) a na každou operace musí byt aplikovaný atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="d70ca-134">Each interface must have the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute applied to it, and each operation must have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute applied to it.</span></span> <span data-ttu-id="d70ca-135">Pokud metoda v rozhraní, které má atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx), nemá atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), taková metoda se nevystaví.</span><span class="sxs-lookup"><span data-stu-id="d70ca-135">If a method in an interface that has the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute does not have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute, that method is not exposed.</span></span> <span data-ttu-id="d70ca-136">Kód k těmto úlohám najdete v příkladu za postupem.</span><span class="sxs-lookup"><span data-stu-id="d70ca-136">The code for these tasks is provided in the example following the procedure.</span></span> <span data-ttu-id="d70ca-137">Podrobnější diskuzi o kontraktech a službách najdete v dokumentaci WCF v části [Návrh a implementace služeb](https://msdn.microsoft.com/library/ms729746.aspx).</span><span class="sxs-lookup"><span data-stu-id="d70ca-137">For a larger discussion of contracts and services, see [Designing and Implementing Services](https://msdn.microsoft.com/library/ms729746.aspx) in the WCF documentation.</span></span>

### <a name="create-a-relay-contract-with-an-interface"></a><span data-ttu-id="d70ca-138">Vytvoření kontraktu předávání s rozhraním</span><span class="sxs-lookup"><span data-stu-id="d70ca-138">Create a relay contract with an interface</span></span>

1. <span data-ttu-id="d70ca-139">Otevřete Visual Studio jako správce tak, že v nabídce **Start** kliknete na program pravým tlačítkem a vyberete možnost **Spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-139">Open Visual Studio as an administrator by right-clicking the program in the **Start** menu and selecting **Run as administrator**.</span></span>
2. <span data-ttu-id="d70ca-140">Vytvořte nový projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="d70ca-140">Create a new console application project.</span></span> <span data-ttu-id="d70ca-141">Klikněte na nabídku **Soubor** a vyberte možnost **Nový**, a pak klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-141">Click the **File** menu and select **New**, then click **Project**.</span></span> <span data-ttu-id="d70ca-142">V dialogu **Nový projekt** klikněte na **Visual C#** (pokud se **Visual C#** nezobrazí, podívejte se do části **Jiné jazyky**).</span><span class="sxs-lookup"><span data-stu-id="d70ca-142">In the **New Project** dialog, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**).</span></span> <span data-ttu-id="d70ca-143">Klikněte **konzolovou aplikaci (rozhraní .NET Framework)** šablony a pojmenujte ji **EchoService**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-143">Click the **Console App (.NET Framework)** template, and name it **EchoService**.</span></span> <span data-ttu-id="d70ca-144">Kliknutím na tlačítko **OK** vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="d70ca-144">Click **OK** to create the project.</span></span>

    ![][2]

3. <span data-ttu-id="d70ca-145">Nainstalujte balíček Service Bus NuGet.</span><span class="sxs-lookup"><span data-stu-id="d70ca-145">Install the Service Bus NuGet package.</span></span> <span data-ttu-id="d70ca-146">Tento balíček automaticky přidá reference na knihovny Service Bus a WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-146">This package automatically adds references to the Service Bus libraries, as well as the WCF **System.ServiceModel**.</span></span> <span data-ttu-id="d70ca-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je obor názvů, který vám umožňuje programový přístup k základním funkcím WCF.</span><span class="sxs-lookup"><span data-stu-id="d70ca-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is the namespace that enables you to programmatically access the basic features of WCF.</span></span> <span data-ttu-id="d70ca-148">Service Bus používá mnoho objektů a atributů WCF k definování kontraktů služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-148">Service Bus uses many of the objects and attributes of WCF to define service contracts.</span></span>

    <span data-ttu-id="d70ca-149">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="d70ca-149">In Solution Explorer, right-click the project, and then click **Manage NuGet Packages...**.</span></span> <span data-ttu-id="d70ca-150">Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-150">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="d70ca-151">Zkontrolujte, že je v části **Verze** označený název projektu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-151">Ensure that the project name is selected in the **Version(s)** box.</span></span> <span data-ttu-id="d70ca-152">Klikněte na **Instalovat** a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="d70ca-152">Click **Install**, and accept the terms of use.</span></span>

    ![][3]
4. <span data-ttu-id="d70ca-153">V Průzkumníku řešení poklikejte na soubor Program.cs a pokud ještě není otevřený, otevře se v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d70ca-153">In Solution Explorer, double-click the Program.cs file to open it in the editor, if it is not already open.</span></span>
5. <span data-ttu-id="d70ca-154">Na začátek souboru přidejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d70ca-154">Add the following using statements at the top of the file:</span></span>

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. <span data-ttu-id="d70ca-155">Změňte název oboru názvů z výchozího názvu **EchoService** na **Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-155">Change the namespace name from its default name of **EchoService** to **Microsoft.ServiceBus.Samples**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="d70ca-156">Tento kurz používá obor názvů C# **Microsoft.ServiceBus.Samples**, které je obor názvů kontraktu základě spravovaný typ, který se používá v konfiguračním souboru v [konfigurace klienta WCF](#configure-the-wcf-client) krok.</span><span class="sxs-lookup"><span data-stu-id="d70ca-156">This tutorial uses the C# namespace **Microsoft.ServiceBus.Samples**, which is the namespace of the contract-based managed type that is used in the configuration file in the [Configure the WCF client](#configure-the-wcf-client) step.</span></span> <span data-ttu-id="d70ca-157">Při sestavování této ukázky můžete specifikovat jakýkoli obor názvů, který chcete – tento kurz ale bude fungovat jen tehdy, když odpovídajícím způsobem v konfiguračním souboru aplikace upravíte obor názvů kontraktu a služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-157">You can specify any namespace you want when you build this sample; however, the tutorial will not work unless you then modify the namespaces of the contract and service accordingly, in the application configuration file.</span></span> <span data-ttu-id="d70ca-158">Obor názvů specifikovaný v souboru App.config musí být stejný jako obor názvů zadaný ve vašich souborech C#.</span><span class="sxs-lookup"><span data-stu-id="d70ca-158">The namespace specified in the App.config file must be the same as the namespace specified in your C# files.</span></span>
   >
   >
7. <span data-ttu-id="d70ca-159">Přímo po `Microsoft.ServiceBus.Samples` deklaraci oboru názvů, ale v rámci oboru názvů definujte nové rozhraní s názvem `IEchoContract` a použít `ServiceContractAttribute` atribut rozhraní s hodnotou oboru názvů `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-159">Directly after the `Microsoft.ServiceBus.Samples` namespace declaration, but within the namespace, define a new interface named `IEchoContract` and apply the `ServiceContractAttribute` attribute to the interface with a namespace value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="d70ca-160">Hodnota oboru názvů se liší od oboru názvů, které používáte v celém svém kódu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-160">The namespace value differs from the namespace that you use throughout the scope of your code.</span></span> <span data-ttu-id="d70ca-161">Místo toho se obor názvů používá jako jedinečný identifikátor pro tento kontrakt.</span><span class="sxs-lookup"><span data-stu-id="d70ca-161">Instead, the namespace value is used as a unique identifier for this contract.</span></span> <span data-ttu-id="d70ca-162">Když explicitně zadáte obor názvů, zabráníte tím přidání výchozí hodnoty oboru názvů do názvu kontraktu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-162">Specifying the namespace explicitly prevents the default namespace value from being added to the contract name.</span></span> <span data-ttu-id="d70ca-163">Vložte následující kód po deklaraci oboru názvů:</span><span class="sxs-lookup"><span data-stu-id="d70ca-163">Paste the following code after the namespace declaration:</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="d70ca-164">Obor názvů kontraktu služby obvykle obsahuje schéma pojmenování s informacemi o verzi.</span><span class="sxs-lookup"><span data-stu-id="d70ca-164">Typically, the service contract namespace contains a naming scheme that includes version information.</span></span> <span data-ttu-id="d70ca-165">Informace o verzi, které jsou v oboru názvů kontraktu služby, službám umožňuje službám izolovat výrazné změny pomocí definice nové služby s novým oborem názvů, která bude vystavená na novém koncovém bodu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-165">Including version information in the service contract namespace enables services to isolate major changes by defining a new service contract with a new namespace and exposing it on a new endpoint.</span></span> <span data-ttu-id="d70ca-166">Tímto způsobem můžete dál používat starého kontraktu služby bez nutnosti ho aktualizovat klienty.</span><span class="sxs-lookup"><span data-stu-id="d70ca-166">In this manner, clients can continue to use the old service contract without having to be updated.</span></span> <span data-ttu-id="d70ca-167">Informace o verzi může mít podobu data nebo čísla sestavení.</span><span class="sxs-lookup"><span data-stu-id="d70ca-167">Version information can consist of a date or a build number.</span></span> <span data-ttu-id="d70ca-168">Další informace najdete v článku o [Správa verzí služeb](http://go.microsoft.com/fwlink/?LinkID=180498).</span><span class="sxs-lookup"><span data-stu-id="d70ca-168">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="d70ca-169">Pro účely tohoto kurzu nebude mít schéma pojmenování oboru názvů kontraktu služby žádné informace o verzi.</span><span class="sxs-lookup"><span data-stu-id="d70ca-169">For the purposes of this tutorial, the naming scheme of the service contract namespace does not contain version information.</span></span>
   >
   >
8. <span data-ttu-id="d70ca-170">V rámci `IEchoContract` rozhraní, deklarujte metodu pro jednu operaci `IEchoContract` kontrakt vystaví v rozhraní a aplikujte `OperationContractAttribute` atribut metodu, kterou chcete vystavit v rámci veřejného kontraktu WCF předávání, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d70ca-170">Within the `IEchoContract` interface, declare a method for the single operation the `IEchoContract` contract exposes in the interface and apply the `OperationContractAttribute` attribute to the method that you want to expose as part of the public WCF Relay contract, as follows:</span></span>

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. <span data-ttu-id="d70ca-171">Přímo po definici rozhraní `IEchoContract` deklarujte kanál, který zdědí vlastnosti z obou `IEchoContract` a také do rozhraní `IClientChannel`, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="d70ca-171">Directly after the `IEchoContract` interface definition, declare a channel that inherits from both `IEchoContract` and also to the `IClientChannel` interface, as shown here:</span></span>

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    <span data-ttu-id="d70ca-172">Kanál je objekt WCF, kterým si hostitel a klient navzájem posílají informace.</span><span class="sxs-lookup"><span data-stu-id="d70ca-172">A channel is the WCF object through which the host and client pass information to each other.</span></span> <span data-ttu-id="d70ca-173">Později napíšete kód na kanál, aby se informace zrcadlily mezi oběma aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="d70ca-173">Later, you will write code against the channel to echo information between the two applications.</span></span>
10. <span data-ttu-id="d70ca-174">V nabídce **Sestavení** můžete kliknout na **Sestavit řešení** nebo stisknout **Ctrl+Shift+B** a potvrdit přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="d70ca-174">From the **Build** menu, click **Build Solution** or press **Ctrl+Shift+B** to confirm the accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="d70ca-175">Příklad</span><span class="sxs-lookup"><span data-stu-id="d70ca-175">Example</span></span>

<span data-ttu-id="d70ca-176">Následující kód ukazuje základní rozhraní, které definuje kontrakt předávání WCF.</span><span class="sxs-lookup"><span data-stu-id="d70ca-176">The following code shows a basic interface that defines a WCF Relay contract.</span></span>

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

<span data-ttu-id="d70ca-177">Když je teď vytvořené rozhraní, můžete ho implementovat.</span><span class="sxs-lookup"><span data-stu-id="d70ca-177">Now that the interface is created, you can implement the interface.</span></span>

## <a name="implement-the-wcf-contract"></a><span data-ttu-id="d70ca-178">Implementace kontraktu WCF</span><span class="sxs-lookup"><span data-stu-id="d70ca-178">Implement the WCF contract</span></span>

<span data-ttu-id="d70ca-179">Vytvoření Azure předávání vyžaduje, abyste nejdřív vytvořili kontrakt, který se definuje pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d70ca-179">Creating an Azure relay requires that you first create the contract, which is defined by using an interface.</span></span> <span data-ttu-id="d70ca-180">Další informace o vytváření rozhraní najdete v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d70ca-180">For more information about creating the interface, see the previous step.</span></span> <span data-ttu-id="d70ca-181">Dalším krokem je implementace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d70ca-181">The next step is to implement the interface.</span></span> <span data-ttu-id="d70ca-182">K tomu patří vytvoření třídy s názvem `EchoService`, která implementuje uživatelsky definované rozhraní `IEchoContract`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-182">This involves creating a class named `EchoService` that implements the user-defined `IEchoContract` interface.</span></span> <span data-ttu-id="d70ca-183">Po implementaci rozhraní nakonfigurujete rozhraní pomocí souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="d70ca-183">After you implement the interface, you then configure the interface using an App.config configuration file.</span></span> <span data-ttu-id="d70ca-184">Konfigurační soubor obsahuje nezbytné informace pro aplikaci, například název služby, název kontraktu a typ protokolu, který se používá ke komunikaci se službou předávání.</span><span class="sxs-lookup"><span data-stu-id="d70ca-184">The configuration file contains necessary information for the application, such as the name of the service, the name of the contract, and the type of protocol that is used to communicate with the relay service.</span></span> <span data-ttu-id="d70ca-185">Kód použitý k těmto úlohám najdete v příkladu za postupem.</span><span class="sxs-lookup"><span data-stu-id="d70ca-185">The code used for these tasks is provided in the example following the procedure.</span></span> <span data-ttu-id="d70ca-186">Obecnější diskuzi o způsobu implementace kontraktu služby najdete v dokumentaci WCF [Implementace kontraktů služby](https://msdn.microsoft.com/library/ms733764.aspx).</span><span class="sxs-lookup"><span data-stu-id="d70ca-186">For a more general discussion about how to implement a service contract, see [Implementing Service Contracts](https://msdn.microsoft.com/library/ms733764.aspx) in the WCF documentation.</span></span>

1. <span data-ttu-id="d70ca-187">Vytvořte novou třídu s názvem `EchoService` přímo po definování rozhraní `IEchoContract`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-187">Create a new class named `EchoService` directly after the definition of the `IEchoContract` interface.</span></span> <span data-ttu-id="d70ca-188">Třída `EchoService` implementuje rozhraní `IEchoContract`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-188">The `EchoService` class implements the `IEchoContract` interface.</span></span>

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    <span data-ttu-id="d70ca-189">Podobně jako u implementace jiných rozhraní můžete definici implementovat v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="d70ca-189">Similar to other interface implementations, you can implement the definition in a different file.</span></span> <span data-ttu-id="d70ca-190">V tomto kurzu je ale implementace ve stejném souboru jako definice rozhraní a metoda `Main`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-190">However, for this tutorial, the implementation is located in the same file as the interface definition and the `Main` method.</span></span>
2. <span data-ttu-id="d70ca-191">Na rozhraní `IEchoContract` aplikujte atribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="d70ca-191">Apply the [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute to the `IEchoContract` interface.</span></span> <span data-ttu-id="d70ca-192">Atribut specifikuje název služby a obor názvů.</span><span class="sxs-lookup"><span data-stu-id="d70ca-192">The attribute specifies the service name and namespace.</span></span> <span data-ttu-id="d70ca-193">Když to dokončíte, třída `EchoService` bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d70ca-193">After doing so, the `EchoService` class appears as follows:</span></span>

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. <span data-ttu-id="d70ca-194">Implementujte metodu `Echo` definovanou v rozhraní `IEchoContract` ve třídě `EchoService`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-194">Implement the `Echo` method defined in the `IEchoContract` interface in the `EchoService` class.</span></span>

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. <span data-ttu-id="d70ca-195">Klikněte na **Sestavení**, klikněte na **Sestavit řešení** a zkontrolujte přesnost své práce.</span><span class="sxs-lookup"><span data-stu-id="d70ca-195">Click **Build**, then click **Build Solution** to confirm the accuracy of your work.</span></span>

### <a name="define-the-configuration-for-the-service-host"></a><span data-ttu-id="d70ca-196">Definice konfigurace hostitele služby</span><span class="sxs-lookup"><span data-stu-id="d70ca-196">Define the configuration for the service host</span></span>

1. <span data-ttu-id="d70ca-197">Konfigurační soubor je velmi podobný konfiguračnímu souboru WCF.</span><span class="sxs-lookup"><span data-stu-id="d70ca-197">The configuration file is very similar to a WCF configuration file.</span></span> <span data-ttu-id="d70ca-198">Obsahuje název služby, koncový bod (to znamená, umístění, které Azure předávání vystaví pro klienty a hostiteli pro komunikaci mezi sebou) a vazbu (typ protokolu používaný pro komunikaci).</span><span class="sxs-lookup"><span data-stu-id="d70ca-198">It includes the service name, endpoint (that is, the location that Azure Relay exposes for clients and hosts to communicate with each other), and the binding (the type of protocol that is used to communicate).</span></span> <span data-ttu-id="d70ca-199">Hlavní rozdíl je v tom, že nakonfigurovaný koncový bod služby odkazuje na vazbu [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding), která není součástí architektury .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d70ca-199">The main difference is that this configured service endpoint refers to a [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) binding, which is not part of the .NET Framework.</span></span> <span data-ttu-id="d70ca-200">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) je jedna z vazeb definovaných službou.</span><span class="sxs-lookup"><span data-stu-id="d70ca-200">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) is one of the bindings defined by the service.</span></span>
2. <span data-ttu-id="d70ca-201">V **Průzkumníku řešení** poklikejte na soubor App.config a otevře se v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d70ca-201">In **Solution Explorer**, double-click the App.config file to open it in the Visual Studio editor.</span></span>
3. <span data-ttu-id="d70ca-202">V elementu `<appSettings>` nahraďte zástupné texty názvem svého oboru názvů a klíčem SAS, který jste zkopírovali v jednom z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="d70ca-202">In the `<appSettings>` element, replace the placeholders with the name of your service namespace, and the SAS key that you copied in an earlier step.</span></span>
4. <span data-ttu-id="d70ca-203">Ve značkách `<system.serviceModel>` přidejte element `<services>`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-203">Within the `<system.serviceModel>` tags, add a `<services>` element.</span></span> <span data-ttu-id="d70ca-204">V jednom konfiguračním souboru můžete definovat více aplikacích s předáváním.</span><span class="sxs-lookup"><span data-stu-id="d70ca-204">You can define multiple relay applications in a single configuration file.</span></span> <span data-ttu-id="d70ca-205">V tomto kurzu se ale definuje jen jedna.</span><span class="sxs-lookup"><span data-stu-id="d70ca-205">However, this tutorial defines only one.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. <span data-ttu-id="d70ca-206">V elementu `<services>` přidejte element `<service>`, který definuje název služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-206">Within the `<services>` element, add a `<service>` element to define the name of the service.</span></span>

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. <span data-ttu-id="d70ca-207">V elementu `<service>` definujte umístění koncového bodu kontraktu a také vazbu koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-207">Within the `<service>` element, define the location of the endpoint contract, and also the type of binding for the endpoint.</span></span>

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="d70ca-208">Koncový bod definuje, kde bude klient hledat hostitelskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d70ca-208">The endpoint defines where the client will look for the host application.</span></span> <span data-ttu-id="d70ca-209">Kurz později použije tento krok k vytvoření adresu URI, která plně vystavuje hostitele přes předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="d70ca-209">Later, the tutorial uses this step to create a URI that fully exposes the host through Azure Relay.</span></span> <span data-ttu-id="d70ca-210">Vazba deklaruje, že se používá TCP jako protokol pro komunikaci se službou předávání přes.</span><span class="sxs-lookup"><span data-stu-id="d70ca-210">The binding declares that we are using TCP as the protocol to communicate with the relay service.</span></span>
7. <span data-ttu-id="d70ca-211">V nabídce **Sestavení** klikněte na **Sestavit řešení** a zkontrolujte přesnost své práce.</span><span class="sxs-lookup"><span data-stu-id="d70ca-211">From the **Build** menu, click **Build Solution** to confirm the accuracy of your work.</span></span>

### <a name="example"></a><span data-ttu-id="d70ca-212">Příklad</span><span class="sxs-lookup"><span data-stu-id="d70ca-212">Example</span></span>

<span data-ttu-id="d70ca-213">Následující kód ukazuje implementaci kontraktu služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-213">The following code shows the implementation of the service contract.</span></span>

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

<span data-ttu-id="d70ca-214">Následující kód ukazuje základní formát souboru App.config přidruženého k hostiteli služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-214">The following code shows the basic format of the App.config file associated with the service host.</span></span>

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

## <a name="host-and-run-a-basic-web-service-to-register-with-the-relay-service"></a><span data-ttu-id="d70ca-215">Hostování a spuštění základní webové služby pro registraci ve službě předávání</span><span class="sxs-lookup"><span data-stu-id="d70ca-215">Host and run a basic web service to register with the relay service</span></span>

<span data-ttu-id="d70ca-216">Tento krok popisuje, jak spustit služby předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="d70ca-216">This step describes how to run an Azure Relay service.</span></span>

### <a name="create-the-relay-credentials"></a><span data-ttu-id="d70ca-217">Vytvořit přihlašovací údaje předávání</span><span class="sxs-lookup"><span data-stu-id="d70ca-217">Create the relay credentials</span></span>

1. <span data-ttu-id="d70ca-218">V `Main()` vytvořte dvě proměnné, do kterých se uloží obor názvů a klíč SAS načtené z okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="d70ca-218">In `Main()`, create two variables in which to store the namespace and the SAS key that are read from the console window.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    <span data-ttu-id="d70ca-219">Klíč SAS se později použije pro přístup k projektu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-219">The SAS key will be used later to access your project.</span></span> <span data-ttu-id="d70ca-220">Obor názvů se předá do `CreateServiceUri` jako parametr a vytvoří se URI služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-220">The namespace is passed as a parameter to `CreateServiceUri` to create a service URI.</span></span>
2. <span data-ttu-id="d70ca-221">Pomocí objektu [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) deklarujte, že jako typ pověření budete používat klíč SAS.</span><span class="sxs-lookup"><span data-stu-id="d70ca-221">Using a [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) object, declare that you will be using a SAS key as the credential type.</span></span> <span data-ttu-id="d70ca-222">Následující kód přidejte přímo za kód, který jste přidali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d70ca-222">Add the following code directly after the code added in the last step.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-the-service"></a><span data-ttu-id="d70ca-223">Vytvoření bázové adresy pro službu</span><span class="sxs-lookup"><span data-stu-id="d70ca-223">Create a base address for the service</span></span>

<span data-ttu-id="d70ca-224">Po kódu, které jste přidali v posledním kroku, vytvořte `Uri` instance pro bázovou adresu služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-224">After the code you added in the last step, create a `Uri` instance for the base address of the service.</span></span> <span data-ttu-id="d70ca-225">Toto URI specifikuje schéma Service Bus, obor názvů a cestu rozhraní služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-225">This URI specifies the Service Bus scheme, the namespace, and the path of the service interface.</span></span>

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

<span data-ttu-id="d70ca-226">„sb“ je zkratka schématu Service Bus a indikuje, že používáme protokol TCP.</span><span class="sxs-lookup"><span data-stu-id="d70ca-226">"sb" is an abbreviation for the Service Bus scheme, and indicates that we are using TCP as the protocol.</span></span> <span data-ttu-id="d70ca-227">To jsme předtím indikovali v konfiguračním souboru, když jsme specifikovali [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) jako vazbu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-227">This was also previously indicated in the configuration file, when [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) was specified as the binding.</span></span>

<span data-ttu-id="d70ca-228">V tomto kurzu je URI `sb://putServiceNamespaceHere.windows.net/EchoService`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-228">For this tutorial, the URI is `sb://putServiceNamespaceHere.windows.net/EchoService`.</span></span>

### <a name="create-and-configure-the-service-host"></a><span data-ttu-id="d70ca-229">Vytvoření a konfigurace hostitele služby</span><span class="sxs-lookup"><span data-stu-id="d70ca-229">Create and configure the service host</span></span>

1. <span data-ttu-id="d70ca-230">Nastavte režim připojení na `AutoDetect`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-230">Set the connectivity mode to `AutoDetect`.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    <span data-ttu-id="d70ca-231">Režim připojení popisuje, protokol, služba se používá ke komunikaci se službou předávání přes; pomocí protokolu HTTP nebo TCP.</span><span class="sxs-lookup"><span data-stu-id="d70ca-231">The connectivity mode describes the protocol the service uses to communicate with the relay service; either HTTP or TCP.</span></span> <span data-ttu-id="d70ca-232">Pomocí výchozího nastavení `AutoDetect`, služba se pokusí připojit k předávání přes Azure přes TCP, pokud je k dispozici a HTTP, pokud TCP není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d70ca-232">Using the default setting `AutoDetect`, the service attempts to connect to Azure Relay over TCP if it is available, and HTTP if TCP is not available.</span></span> <span data-ttu-id="d70ca-233">Všimněte si, že tu je rozdíl oproti protokolu, který služba specifikuje pro komunikaci klienta.</span><span class="sxs-lookup"><span data-stu-id="d70ca-233">Note that this differs from the protocol the service specifies for client communication.</span></span> <span data-ttu-id="d70ca-234">Jeho protokol se určuje podle požité vazby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-234">That protocol is determined by the binding used.</span></span> <span data-ttu-id="d70ca-235">Například můžete použít službu [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) vazba, která určuje, že její koncový bod s klienty komunikuje přes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d70ca-235">For example, a service can use the [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) binding, which specifies that its endpoint communicates with clients over HTTP.</span></span> <span data-ttu-id="d70ca-236">Že stejné služba by mohla specifikovat **ConnectivityMode.AutoDetect** tak, aby služba komunikuje s Azure předávání přes protokol TCP.</span><span class="sxs-lookup"><span data-stu-id="d70ca-236">That same service could specify **ConnectivityMode.AutoDetect** so that the service communicates with Azure Relay over TCP.</span></span>
2. <span data-ttu-id="d70ca-237">Vytvořte hostitele služby pomocí URI, které jste předtím vytvořili v této části.</span><span class="sxs-lookup"><span data-stu-id="d70ca-237">Create the service host, using the URI created earlier in this section.</span></span>

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    <span data-ttu-id="d70ca-238">Hostitel služby je objekt WCF, který instancuje službu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-238">The service host is the WCF object that instantiates the service.</span></span> <span data-ttu-id="d70ca-239">Tady jí předáte typ služby, kterou chcete vytvořit (typ `EchoService`) a taky adresu, na které chcete službu vystavit.</span><span class="sxs-lookup"><span data-stu-id="d70ca-239">Here, you pass it the type of service you want to create (an `EchoService` type), and also to the address at which you want to expose the service.</span></span>
3. <span data-ttu-id="d70ca-240">Na začátku souboru Program.cs přidejte odkazy na [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) a [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span><span class="sxs-lookup"><span data-stu-id="d70ca-240">At the top of the Program.cs file, add references to [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) and [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span></span>

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. <span data-ttu-id="d70ca-241">Zpátky v `Main()` nakonfigurujte koncový bod, a povolte tak veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="d70ca-241">Back in `Main()`, configure the endpoint to enable public access.</span></span>

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    <span data-ttu-id="d70ca-242">Tento krok informuje předávací služba, která vaše aplikace dá veřejně najít tak, že prověří ATOM kanálu pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="d70ca-242">This step informs the relay service that your application can be found publicly by examining the ATOM feed for your project.</span></span> <span data-ttu-id="d70ca-243">Pokud **DiscoveryType** nastavíte na **private**, služba by pro klienta pořád byla dostupná.</span><span class="sxs-lookup"><span data-stu-id="d70ca-243">If you set **DiscoveryType** to **private**, a client would still be able to access the service.</span></span> <span data-ttu-id="d70ca-244">Službu však nebude se ale při prohledávání oboru názvů předávání.</span><span class="sxs-lookup"><span data-stu-id="d70ca-244">However, the service would not appear when it searches the Relay namespace.</span></span> <span data-ttu-id="d70ca-245">Místo toho by klient musel předem znát cestu ke koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-245">Instead, the client would have to know the endpoint path beforehand.</span></span>
5. <span data-ttu-id="d70ca-246">Použijte pověření služby na koncové body služby definované v souboru App.config:</span><span class="sxs-lookup"><span data-stu-id="d70ca-246">Apply the service credentials to the service endpoints defined in the App.config file:</span></span>

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    <span data-ttu-id="d70ca-247">Jak jsme uvedli v předchozím kroku, mohli jste v konfiguračním souboru deklarovat několik služeb a koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="d70ca-247">As stated in the previous step, you could have declared multiple services and endpoints in the configuration file.</span></span> <span data-ttu-id="d70ca-248">Pokud byste to udělali, tento kód by prošel konfigurační soubor a vyhledal by všechny koncové body, na které by měl vaše pověření použít.</span><span class="sxs-lookup"><span data-stu-id="d70ca-248">If you had, this code would traverse the configuration file and search for every endpoint to which it should apply your credentials.</span></span> <span data-ttu-id="d70ca-249">V tomto kurzu má ale konfigurační soubor jen jeden koncový bod.</span><span class="sxs-lookup"><span data-stu-id="d70ca-249">However, for this tutorial, the configuration file has only one endpoint.</span></span>

### <a name="open-the-service-host"></a><span data-ttu-id="d70ca-250">Otevření hostitele služby</span><span class="sxs-lookup"><span data-stu-id="d70ca-250">Open the service host</span></span>

1. <span data-ttu-id="d70ca-251">Otevřete službu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-251">Open the service.</span></span>

    ```csharp
    host.Open();
    ```
2. <span data-ttu-id="d70ca-252">Informujte uživatele, že zpráva běží, a vysvětlete mu, jak službu ukončit.</span><span class="sxs-lookup"><span data-stu-id="d70ca-252">Inform the user that the service is running, and explain how to shut down the service.</span></span>

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="d70ca-253">Po dokončení zavřete hostitele služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-253">When finished, close the service host.</span></span>

    ```csharp
    host.Close();
    ```
4. <span data-ttu-id="d70ca-254">Stisknutím kláves **CTRL+SHIFT+B** sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="d70ca-254">Press **Ctrl+Shift+B** to build the project.</span></span>

### <a name="example"></a><span data-ttu-id="d70ca-255">Příklad</span><span class="sxs-lookup"><span data-stu-id="d70ca-255">Example</span></span>

<span data-ttu-id="d70ca-256">Kódu dokončené služby by měl vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="d70ca-256">Your completed service code should appear as follows.</span></span> <span data-ttu-id="d70ca-257">Kód obsahuje kontrakt a implementaci služby z předchozích kroků tohoto kurzu a hostuje službu v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d70ca-257">The code includes the service contract and implementation from previous steps in the tutorial, and hosts the service in a console application.</span></span>

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

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a><span data-ttu-id="d70ca-258">Vytvoření klienta WCF pro kontrakt služby</span><span class="sxs-lookup"><span data-stu-id="d70ca-258">Create a WCF client for the service contract</span></span>

<span data-ttu-id="d70ca-259">Dalším krokem je vytvoření klientskou aplikaci a definování kontraktu služby, který implementujete v pozdějších krocích.</span><span class="sxs-lookup"><span data-stu-id="d70ca-259">The next step is to create a client application and define the service contract you will implement in later steps.</span></span> <span data-ttu-id="d70ca-260">Všimněte si, že hodně těchto kroků připomíná kroky k vytvoření služby: definování kontraktu, upravení App.config souboru, pomocí přihlašovacích údajů k připojení ke službě předávání a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d70ca-260">Note that many of these steps resemble the steps used to create a service: defining a contract, editing an App.config file, using credentials to connect to the relay service, and so on.</span></span> <span data-ttu-id="d70ca-261">Kód použitý k těmto úlohám najdete v příkladu za postupem.</span><span class="sxs-lookup"><span data-stu-id="d70ca-261">The code used for these tasks is provided in the example following the procedure.</span></span>

1. <span data-ttu-id="d70ca-262">Vytvořte pro klienta nový projekt v aktuálním řešení ve Visual Studiu, a to tímto postupem:</span><span class="sxs-lookup"><span data-stu-id="d70ca-262">Create a new project in the current Visual Studio solution for the client by doing the following:</span></span>

   1. <span data-ttu-id="d70ca-263">V Průzkumníku řešení ve stejném řešení, které obsahuje službu, klikněte pravým tlačítkem na aktuální řešení (nikoli projekt) a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-263">In Solution Explorer, in the same solution that contains the service, right-click the current solution (not the project), and click **Add**.</span></span> <span data-ttu-id="d70ca-264">Pak klikněte na **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-264">Then click **New Project**.</span></span>
   2. <span data-ttu-id="d70ca-265">V **přidat nový projekt** dialogové okno, klikněte na tlačítko **Visual C#** (Pokud **Visual C#** nezobrazí, podívejte se do části **jiné jazyky**), vyberte **Konzolovou aplikaci (rozhraní .NET Framework)** šablony a pojmenujte ji **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-265">In the **Add New Project** dialog box, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**), select the **Console App (.NET Framework)** template, and name it **EchoClient**.</span></span>
   3. <span data-ttu-id="d70ca-266">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-266">Click **OK**.</span></span>
      <br />
2. <span data-ttu-id="d70ca-267">V Průzkumníku řešení poklikejte na soubor Program.cs v projektu **EchoClient** a pokud ještě není otevřený, otevře se v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d70ca-267">In Solution Explorer, double-click the Program.cs file in the **EchoClient** project to open it in the editor, if it is not already open.</span></span>
3. <span data-ttu-id="d70ca-268">Změňte název oboru názvů z výchozího názvu `EchoClient` na `Microsoft.ServiceBus.Samples`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-268">Change the namespace name from its default name of `EchoClient` to `Microsoft.ServiceBus.Samples`.</span></span>
4. <span data-ttu-id="d70ca-269">Nainstalujte [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus): v Průzkumníku řešení klikněte pravým tlačítkem myši **EchoClient** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-269">Install the [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus): in Solution Explorer, right-click the **EchoClient** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="d70ca-270">Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-270">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="d70ca-271">Klikněte na **Instalovat** a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="d70ca-271">Click **Install**, and accept the terms of use.</span></span>

    ![][3]
5. <span data-ttu-id="d70ca-272">V souboru Program.cs přidejte příkaz `using` pro obor názvů [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx).</span><span class="sxs-lookup"><span data-stu-id="d70ca-272">Add a `using` statement for the [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) namespace in the Program.cs file.</span></span>

    ```csharp
    using System.ServiceModel;
    ```
6. <span data-ttu-id="d70ca-273">Přidejte definici kontraktu služby do oboru názvů, jak je vidět v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-273">Add the service contract definition to the namespace, as shown in the following example.</span></span> <span data-ttu-id="d70ca-274">Všimněte si, že je tato definice skoro stejná jako definice použitá v projektu **Service**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-274">Note that this definition is identical to the definition used in the **Service** project.</span></span> <span data-ttu-id="d70ca-275">Tento kód byste měli přidat na začátek oboru názvů `Microsoft.ServiceBus.Samples`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-275">You should add this code at the top of the `Microsoft.ServiceBus.Samples` namespace.</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. <span data-ttu-id="d70ca-276">Stisknutím kláves **CTRL+SHIFT+B** sestavte klienta.</span><span class="sxs-lookup"><span data-stu-id="d70ca-276">Press **Ctrl+Shift+B** to build the client.</span></span>

### <a name="example"></a><span data-ttu-id="d70ca-277">Příklad</span><span class="sxs-lookup"><span data-stu-id="d70ca-277">Example</span></span>

<span data-ttu-id="d70ca-278">Následující kód ukazuje aktuální stav souboru Program.cs v **EchoClient** projektu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-278">The following code shows the current status of the Program.cs file in the **EchoClient** project.</span></span>

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

## <a name="configure-the-wcf-client"></a><span data-ttu-id="d70ca-279">Konfigurace klienta WCF</span><span class="sxs-lookup"><span data-stu-id="d70ca-279">Configure the WCF client</span></span>

<span data-ttu-id="d70ca-280">V tomto kroku vytvoříte soubor App.config pro základní klientskou aplikaci, která bude mít přístup ke službě vytvořené v jednom z předchozích kroků tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-280">In this step, you create an App.config file for a basic client application that accesses the service created previously in this tutorial.</span></span> <span data-ttu-id="d70ca-281">Tento soubor App.config definuje kontrakt, vazbu a název koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-281">This App.config file defines the contract, binding, and name of the endpoint.</span></span> <span data-ttu-id="d70ca-282">Kód použitý k těmto úlohám najdete v příkladu za postupem.</span><span class="sxs-lookup"><span data-stu-id="d70ca-282">The code used for these tasks is provided in the example following the procedure.</span></span>

1. <span data-ttu-id="d70ca-283">V Průzkumníku řešení v projektu **EchoClient** poklikejte na **App.config.cs** a otevře se v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d70ca-283">In Solution Explorer, in the **EchoClient** project, double-click **App.config** to open the file in the Visual Studio editor.</span></span>
2. <span data-ttu-id="d70ca-284">V elementu `<appSettings>` nahraďte zástupné texty názvem svého oboru názvů a klíčem SAS, který jste zkopírovali v jednom z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="d70ca-284">In the `<appSettings>` element, replace the placeholders with the name of your service namespace, and the SAS key that you copied in an earlier step.</span></span>
3. <span data-ttu-id="d70ca-285">V elementu system.serviceModel element přidejte element `<client>`.</span><span class="sxs-lookup"><span data-stu-id="d70ca-285">Within the system.serviceModel element, add a `<client>` element.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    <span data-ttu-id="d70ca-286">Tento krok deklaruje, že definujete klientskou aplikaci ve stylu WCF.</span><span class="sxs-lookup"><span data-stu-id="d70ca-286">This step declares that you are defining a WCF-style client application.</span></span>
4. <span data-ttu-id="d70ca-287">V elementu `client` definujte název, kontrakt a typ vazby koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-287">Within the `client` element, define the name, contract, and binding type for the endpoint.</span></span>

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="d70ca-288">Tento krok definuje název koncového bodu, kontrakt definovaný ve službě a fakt, že klientská aplikace používá TCP ke komunikaci s předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="d70ca-288">This step defines the name of the endpoint, the contract defined in the service, and the fact that the client application uses TCP to communicate with Azure Relay.</span></span> <span data-ttu-id="d70ca-289">Název koncového bodu se použije v následujícím kroku k propojení této konfigurace koncového bodu s URI služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-289">The endpoint name is used in the next step to link this endpoint configuration with the service URI.</span></span>
5. <span data-ttu-id="d70ca-290">Klikněte na tlačítko **soubor**, pak klikněte na tlačítko **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-290">Click **File**, then click **Save All**.</span></span>

## <a name="example"></a><span data-ttu-id="d70ca-291">Příklad</span><span class="sxs-lookup"><span data-stu-id="d70ca-291">Example</span></span>

<span data-ttu-id="d70ca-292">Následující kód ukazuje soubor App.config pro klienta Echo.</span><span class="sxs-lookup"><span data-stu-id="d70ca-292">The following code shows the App.config file for the Echo client.</span></span>

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

## <a name="implement-the-wcf-client"></a><span data-ttu-id="d70ca-293">Implementace klienta WCF</span><span class="sxs-lookup"><span data-stu-id="d70ca-293">Implement the WCF client</span></span>
<span data-ttu-id="d70ca-294">V tomto kroku implementujete základní klientskou aplikaci, která bude mít přístup ke službě, kterou jste vytvořili v jednom z předchozích kroků tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-294">In this step, you implement a basic client application that accesses the service you created previously in this tutorial.</span></span> <span data-ttu-id="d70ca-295">Podobně jako u služby, klient provádí spoustu stejných operací jako pro přístup k předávání přes Azure:</span><span class="sxs-lookup"><span data-stu-id="d70ca-295">Similar to the service, the client performs many of the same operations to access Azure Relay:</span></span>

1. <span data-ttu-id="d70ca-296">Nastaví režim připojení.</span><span class="sxs-lookup"><span data-stu-id="d70ca-296">Sets the connectivity mode.</span></span>
2. <span data-ttu-id="d70ca-297">Vytvoří URI, které vyhledá hostitelskou službu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-297">Creates the URI that locates the host service.</span></span>
3. <span data-ttu-id="d70ca-298">Definuje bezpečnostní pověření.</span><span class="sxs-lookup"><span data-stu-id="d70ca-298">Defines the security credentials.</span></span>
4. <span data-ttu-id="d70ca-299">Aplikuje pověření na připojení.</span><span class="sxs-lookup"><span data-stu-id="d70ca-299">Applies the credentials to the connection.</span></span>
5. <span data-ttu-id="d70ca-300">Otevře připojení.</span><span class="sxs-lookup"><span data-stu-id="d70ca-300">Opens the connection.</span></span>
6. <span data-ttu-id="d70ca-301">Provádí úlohy specifické pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="d70ca-301">Performs the application-specific tasks.</span></span>
7. <span data-ttu-id="d70ca-302">Ukončí připojení.</span><span class="sxs-lookup"><span data-stu-id="d70ca-302">Closes the connection.</span></span>

<span data-ttu-id="d70ca-303">Jedním z hlavních rozdílů je ale, že klientská aplikace používá kanál, který se připojit ke službě předávání, zatímco služba používá volání **ServiceHost**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-303">However, one of the main differences is that the client application uses a channel to connect to the relay service, whereas the service uses a call to **ServiceHost**.</span></span> <span data-ttu-id="d70ca-304">Kód použitý k těmto úlohám najdete v příkladu za postupem.</span><span class="sxs-lookup"><span data-stu-id="d70ca-304">The code used for these tasks is provided in the example following the procedure.</span></span>

### <a name="implement-a-client-application"></a><span data-ttu-id="d70ca-305">Implementace klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="d70ca-305">Implement a client application</span></span>
1. <span data-ttu-id="d70ca-306">Nastavte režim připojení na **AutoDetect**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-306">Set the connectivity mode to **AutoDetect**.</span></span> <span data-ttu-id="d70ca-307">Do metody `Main()` aplikace **EchoClient** přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="d70ca-307">Add the following code inside the `Main()` method of the **EchoClient** application.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. <span data-ttu-id="d70ca-308">Definujte proměnné, do kterých se uloží hodnoty obor názvů služby a klíče SAS načtené z konzoly.</span><span class="sxs-lookup"><span data-stu-id="d70ca-308">Define variables to hold the values for the service namespace, and SAS key that are read from the console.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. <span data-ttu-id="d70ca-309">Vytvořte identifikátor URI, který definuje umístění hostitele ve vašem projektu předávání.</span><span class="sxs-lookup"><span data-stu-id="d70ca-309">Create the URI that defines the location of the host in your Relay project.</span></span>

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. <span data-ttu-id="d70ca-310">Vytvořte objekt pověření pro koncový bod vašeho oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d70ca-310">Create the credential object for your service namespace endpoint.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. <span data-ttu-id="d70ca-311">Vytvořte objekt kanálu pro vytváření, který načte konfiguraci popsanou v souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="d70ca-311">Create the channel factory that loads the configuration described in the App.config file.</span></span>

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    <span data-ttu-id="d70ca-312">Objekt kanálu pro vytváření je objekt WCF, který vytvoří kanál, přes který může služba komunikovat s klientskými aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="d70ca-312">A channel factory is a WCF object that creates a channel through which the service and client applications communicate.</span></span>
6. <span data-ttu-id="d70ca-313">Použijte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="d70ca-313">Apply the credentials.</span></span>

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. <span data-ttu-id="d70ca-314">Vytvořte a otevřete kanál pro službu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-314">Create and open the channel to the service.</span></span>

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. <span data-ttu-id="d70ca-315">Napište základní uživatelské prostředí a funkci pro echo.</span><span class="sxs-lookup"><span data-stu-id="d70ca-315">Write the basic user interface and functionality for the echo.</span></span>

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
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

    <span data-ttu-id="d70ca-316">Všimněte si, že kód používá instanci objektu kanálu jako proxy pro službu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-316">Note that the code uses the instance of the channel object as a proxy for the service.</span></span>
9. <span data-ttu-id="d70ca-317">Zavřete kanál a zavřete objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="d70ca-317">Close the channel, and close the factory.</span></span>

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a><span data-ttu-id="d70ca-318">Příklad</span><span class="sxs-lookup"><span data-stu-id="d70ca-318">Example</span></span>

<span data-ttu-id="d70ca-319">Dokončený kód by měly vypadat následovně, jak vytvořit klientskou aplikaci, jak volat operace služby a jak zavřít klienta po volání operace je dokončena.</span><span class="sxs-lookup"><span data-stu-id="d70ca-319">Your completed code should appear as follows, showing how to create a client application, how to call the operations of the service, and how to close the client after the operation call is finished.</span></span>

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

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
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

## <a name="run-the-applications"></a><span data-ttu-id="d70ca-320">Spuštění aplikací</span><span class="sxs-lookup"><span data-stu-id="d70ca-320">Run the applications</span></span>

1. <span data-ttu-id="d70ca-321">Stisknutím kláves **CTRL+SHIFT+B** sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="d70ca-321">Press **Ctrl+Shift+B** to build the solution.</span></span> <span data-ttu-id="d70ca-322">Sestavíte tím projekt klienta a projekt služby, které jste vytvořili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="d70ca-322">This builds both the client project and the service project that you created in the previous steps.</span></span>
2. <span data-ttu-id="d70ca-323">Než spustíte klientskou aplikaci, musíte se ujistit, že aplikace služby běží.</span><span class="sxs-lookup"><span data-stu-id="d70ca-323">Before running the client application, you must make sure that the service application is running.</span></span> <span data-ttu-id="d70ca-324">V Průzkumníku řešení ve Visual Studiu klikněte pravým tlačítkem na řešení **EchoService**, pak klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-324">In Solution Explorer in Visual Studio, right-click the **EchoService** solution, then click **Properties**.</span></span>
3. <span data-ttu-id="d70ca-325">V dialogovém okně vlastností řešení klikněte na **Spouštěný projekt** a pak klikněte na tlačítko **Více projektů po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-325">In the solution properties dialog box, click **Startup Project**, then click the **Multiple startup projects** button.</span></span> <span data-ttu-id="d70ca-326">Ujistěte se, že se **EchoService** v seznamu objeví jako první.</span><span class="sxs-lookup"><span data-stu-id="d70ca-326">Make sure **EchoService** appears first in the list.</span></span>
4. <span data-ttu-id="d70ca-327">V poli **Akce** u projektů **EchoService** i **EchoClient** nastavte **Start**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-327">Set the **Action** box for both the **EchoService** and **EchoClient** projects to **Start**.</span></span>

    ![][5]
5. <span data-ttu-id="d70ca-328">Klikněte na **Závislosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-328">Click **Project Dependencies**.</span></span> <span data-ttu-id="d70ca-329">V poli **Projekty** klikněte na **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-329">In the **Projects** box, select **EchoClient**.</span></span> <span data-ttu-id="d70ca-330">Ujistěte se, že je v poli **Závisí na** označené **EchoService**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-330">In the **Depends on** box, make sure **EchoService** is checked.</span></span>

    ![][6]
6. <span data-ttu-id="d70ca-331">Klikněte na **OK**, a dialog **Vlastnosti** se zavře.</span><span class="sxs-lookup"><span data-stu-id="d70ca-331">Click **OK** to dismiss the **Properties** dialog.</span></span>
7. <span data-ttu-id="d70ca-332">Stiskněte klávesu **F5** a oba projekty se spustí.</span><span class="sxs-lookup"><span data-stu-id="d70ca-332">Press **F5** to run both projects.</span></span>
8. <span data-ttu-id="d70ca-333">Obě okna konzoly se otevřou a požádají vás o zadání oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d70ca-333">Both console windows open and prompt you for the namespace name.</span></span> <span data-ttu-id="d70ca-334">Nejdřív se musí spustit služba, takže v okně konzoly **EchoService** zadejte obor názvů, a pak stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="d70ca-334">The service must run first, so in the **EchoService** console window, enter the namespace and then press **Enter**.</span></span>
9. <span data-ttu-id="d70ca-335">Pak se zobrazí výzva k zadání klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="d70ca-335">Next, you are prompted for your SAS key.</span></span> <span data-ttu-id="d70ca-336">Zadejte klíč SAS a stiskněte Enter.</span><span class="sxs-lookup"><span data-stu-id="d70ca-336">Enter the SAS key and press ENTER.</span></span>

    <span data-ttu-id="d70ca-337">Tady je příklad výstupu z okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="d70ca-337">Here is example output from the console window.</span></span> <span data-ttu-id="d70ca-338">Nezapomeňte, tyto zadané hodnoty platí jen pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="d70ca-338">Note that the values provided here are for example purposes only.</span></span>

    <span data-ttu-id="d70ca-339">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span><span class="sxs-lookup"><span data-stu-id="d70ca-339">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span></span>

    <span data-ttu-id="d70ca-340">Aplikace služby do okna konzoly vypíše adresu, na které naslouchá, jak je vidět na následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-340">The service application prints to the console window the address on which it's listening, as seen in the following example.</span></span>

    <span data-ttu-id="d70ca-341">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`</span><span class="sxs-lookup"><span data-stu-id="d70ca-341">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`</span></span>
10. <span data-ttu-id="d70ca-342">V okně konzoly **EchoClient** zadejte stejný údaj, který jste zadali pro aplikaci služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-342">In the **EchoClient** console window, enter the same information that you entered previously for the service application.</span></span> <span data-ttu-id="d70ca-343">Stejným postupem jako v předchozích krocích zadejte stejný obor názvů a klíč SAS pro klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d70ca-343">Follow the previous steps to enter the same service namespace and SAS key values for the client application.</span></span>
11. <span data-ttu-id="d70ca-344">Po zadání těchto hodnot klient otevře kanál ke službě a zobrazí se výzva k zadání nějakého textu, jak je vidět v následujícím příkladu výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="d70ca-344">After entering these values, the client opens a channel to the service and prompts you to enter some text as seen in the following console output example.</span></span>

    `Enter text to echo (or [Enter] to exit):`

    <span data-ttu-id="d70ca-345">Zadejte nějaký text, který se má odeslat do aplikace služby, a stiskněte Enter.</span><span class="sxs-lookup"><span data-stu-id="d70ca-345">Enter some text to send to the service application and press Enter.</span></span> <span data-ttu-id="d70ca-346">Tento text se odešle do služby pomocí operace služby Echo a objeví se v okně konzoly služby, jak je vidět v následujícím příkladu výstupu.</span><span class="sxs-lookup"><span data-stu-id="d70ca-346">This text is sent to the service through the Echo service operation and appears in the service console window as in the following example output.</span></span>

    `Echoing: My sample text`

    <span data-ttu-id="d70ca-347">Klientská aplikace obdrží hodnotu vrácenou z operace `Echo`. Tou je původní text, který se vypíše do okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="d70ca-347">The client application receives the return value of the `Echo` operation, which is the original text, and prints it to its console window.</span></span> <span data-ttu-id="d70ca-348">Následující příklad ukazuje výstup okna konzoly klienta.</span><span class="sxs-lookup"><span data-stu-id="d70ca-348">The following is example output from the client console window.</span></span>

    `Server echoed: My sample text`
12. <span data-ttu-id="d70ca-349">Tímto způsobem můžete dál posílat textové zprávy z klienta do služby.</span><span class="sxs-lookup"><span data-stu-id="d70ca-349">You can continue sending text messages from the client to the service in this manner.</span></span> <span data-ttu-id="d70ca-350">Když skončíte, stiskněte Enter v oknech konzoly klienta a služby a obě aplikace se ukončí.</span><span class="sxs-lookup"><span data-stu-id="d70ca-350">When you are finished, press Enter in the client and service console windows to end both applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d70ca-351">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d70ca-351">Next steps</span></span>

<span data-ttu-id="d70ca-352">Tento kurz vám ukázal, jak vytvářet klientem předávání přes Azure aplikace a služby pomocí možnosti WCF předávání přes Service Bus.</span><span class="sxs-lookup"><span data-stu-id="d70ca-352">This tutorial showed how to build an Azure Relay client application and service using the WCF Relay capabilities of Service Bus.</span></span> <span data-ttu-id="d70ca-353">Podobný kurz, který používá [zasílání zpráv Service Bus](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), najdete v části [začít pracovat s fronty Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="d70ca-353">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="d70ca-354">Další informace o předávání přes Azure, naleznete v následujících tématech.</span><span class="sxs-lookup"><span data-stu-id="d70ca-354">To learn more about Azure Relay, see the following topics.</span></span>

* [<span data-ttu-id="d70ca-355">Přehled architektury služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="d70ca-355">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [<span data-ttu-id="d70ca-356">Přehled služby Azure Relay</span><span class="sxs-lookup"><span data-stu-id="d70ca-356">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="d70ca-357">Jak používat předávání služby WCF s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="d70ca-357">How to use the WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
