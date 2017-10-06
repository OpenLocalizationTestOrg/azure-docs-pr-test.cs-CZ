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
# <a name="azure-wcf-relay-tutorial"></a><span data-ttu-id="9db39-103">Kurz pro Azure předávání WCF</span><span class="sxs-lookup"><span data-stu-id="9db39-103">Azure WCF Relay tutorial</span></span>

<span data-ttu-id="9db39-104">Tento kurz popisuje, jak toobuild jednoduché WCF předávání klientská aplikace a služby pomocí předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="9db39-104">This tutorial describes how toobuild a simple WCF Relay client application and service using Azure Relay.</span></span> <span data-ttu-id="9db39-105">Podobný kurz, který používá [zasílání zpráv Service Bus](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), najdete v části [začít pracovat s fronty Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="9db39-105">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="9db39-106">Absolvování tohoto kurzu pochopíte hello kroků, které jsou požadované toocreate WCF přenosový klient a služba aplikace.</span><span class="sxs-lookup"><span data-stu-id="9db39-106">Working through this tutorial gives you an understanding of hello steps that are required toocreate a WCF Relay client and service application.</span></span> <span data-ttu-id="9db39-107">Stejně jako jejich původní protějšky WCF je služba konstrukce, která vystavuje jeden nebo více koncových bodů, každý z nich vystavuje jednu nebo víc operací služeb.</span><span class="sxs-lookup"><span data-stu-id="9db39-107">Like their original WCF counterparts, a service is a construct that exposes one or more endpoints, each of which exposes one or more service operations.</span></span> <span data-ttu-id="9db39-108">Hello koncový bod služby specifikuje adresu, kde se dá hello služba najít, vazbu, která obsahuje hello informace, které klient musí komunikovat s hello service a kontrakt, který definuje hello funkce poskytované službou hello služby tooits klientů.</span><span class="sxs-lookup"><span data-stu-id="9db39-108">hello endpoint of a service specifies an address where hello service can be found, a binding that contains hello information that a client must communicate with hello service, and a contract that defines hello functionality provided by hello service tooits clients.</span></span> <span data-ttu-id="9db39-109">Hello hlavní rozdíl mezi WCF a předávací WCF je, že tento koncový bod hello vystavený v cloudu hello místo místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9db39-109">hello main difference between WCF and WCF Relay is that hello endpoint is exposed in hello cloud instead of locally on your computer.</span></span>

<span data-ttu-id="9db39-110">Po absolvování hello řady témat v tomto kurzu budete mít funkční službu a klienta, který může vyvolat operace hello služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-110">After you work through hello sequence of topics in this tutorial, you will have a running service, and a client that can invoke hello operations of hello service.</span></span> <span data-ttu-id="9db39-111">Hello první téma popisuje, jak tooset si účet.</span><span class="sxs-lookup"><span data-stu-id="9db39-111">hello first topic describes how tooset up an account.</span></span> <span data-ttu-id="9db39-112">Hello další kroky popisují, jak toodefine služby používající kontrakt, jak tooimplement hello služby a jak tooconfigure hello služby v kódu.</span><span class="sxs-lookup"><span data-stu-id="9db39-112">hello next steps describe how toodefine a service that uses a contract, how tooimplement hello service, and how tooconfigure hello service in code.</span></span> <span data-ttu-id="9db39-113">Najdete zde také popis jak toohost a spusťte službu hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-113">They also describe how toohost and run hello service.</span></span> <span data-ttu-id="9db39-114">Hello vytvořená služba se hostuje sama a klient hello a služba běží na hello stejný počítač.</span><span class="sxs-lookup"><span data-stu-id="9db39-114">hello service that is created is self-hosted and hello client and service run on hello same computer.</span></span> <span data-ttu-id="9db39-115">Hello služby můžete nakonfigurovat pomocí kódu nebo konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="9db39-115">You can configure hello service by using either code or a configuration file.</span></span>

<span data-ttu-id="9db39-116">Hello poslední tři kroky popisují, jak toocreate klientskou aplikaci, konfiguraci hello klientskou aplikaci a vytvořit a použít klienta, který můžete přístup k funkcím hello hello hostitele.</span><span class="sxs-lookup"><span data-stu-id="9db39-116">hello final three steps describe how toocreate a client application, configure hello client application, and create and use a client that can access hello functionality of hello host.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9db39-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9db39-117">Prerequisites</span></span>

<span data-ttu-id="9db39-118">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="9db39-118">toocomplete this tutorial, you'll need hello following:</span></span>

* <span data-ttu-id="9db39-119">[Microsoft Visual Studio 2015 nebo vyšší](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="9db39-119">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="9db39-120">Tento kurz používá Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="9db39-120">This tutorial uses Visual Studio 2017.</span></span>
* <span data-ttu-id="9db39-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="9db39-121">An active Azure account.</span></span> <span data-ttu-id="9db39-122">Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut.</span><span class="sxs-lookup"><span data-stu-id="9db39-122">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="9db39-123">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="9db39-123">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="9db39-124">Vytvoření oboru názvů služby</span><span class="sxs-lookup"><span data-stu-id="9db39-124">Create a service namespace</span></span>

<span data-ttu-id="9db39-125">prvním krokem Hello je toocreate obor názvů a tooobtain [sdíleného přístupového podpisu (SAS)](../service-bus-messaging/service-bus-sas.md) klíč.</span><span class="sxs-lookup"><span data-stu-id="9db39-125">hello first step is toocreate a namespace, and tooobtain a [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) key.</span></span> <span data-ttu-id="9db39-126">Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu předávání přes hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-126">A namespace provides an application boundary for each application exposed through hello relay service.</span></span> <span data-ttu-id="9db39-127">Klíč SAS je automaticky generován hello systému při vytvoření oboru názvů služby.</span><span class="sxs-lookup"><span data-stu-id="9db39-127">A SAS key is automatically generated by hello system when a service namespace is created.</span></span> <span data-ttu-id="9db39-128">Hello kombinace oboru názvů služby a klíče SAS poskytuje pověření hello Azure tooauthenticate přístup tooan aplikace.</span><span class="sxs-lookup"><span data-stu-id="9db39-128">hello combination of service namespace and SAS key provides hello credentials for Azure tooauthenticate access tooan application.</span></span> <span data-ttu-id="9db39-129">Postupujte podle hello [pokynů tady](relay-create-namespace-portal.md) toocreate předávání názvů.</span><span class="sxs-lookup"><span data-stu-id="9db39-129">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="define-a-wcf-service-contract"></a><span data-ttu-id="9db39-130">Definování kontraktu služby WCF</span><span class="sxs-lookup"><span data-stu-id="9db39-130">Define a WCF service contract</span></span>

<span data-ttu-id="9db39-131">Hello kontrakt služby specifikuje, jaké operace (hello termín webových služeb pro metody nebo funkce) hello služba podporuje.</span><span class="sxs-lookup"><span data-stu-id="9db39-131">hello service contract specifies what operations (hello web service terminology for methods or functions) hello service supports.</span></span> <span data-ttu-id="9db39-132">Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="9db39-132">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="9db39-133">Každá metoda v rozhraní hello odpovídá tooa konkrétní operaci služby.</span><span class="sxs-lookup"><span data-stu-id="9db39-133">Each method in hello interface corresponds tooa specific service operation.</span></span> <span data-ttu-id="9db39-134">Každé rozhraní musí mít hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) tooit použit atribut a každou operaci musí mít hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) tooit atribut použitý.</span><span class="sxs-lookup"><span data-stu-id="9db39-134">Each interface must have hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute applied tooit, and each operation must have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute applied tooit.</span></span> <span data-ttu-id="9db39-135">Pokud metoda v rozhraní, které má hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atribut nemá hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) atribut, taková metoda se nevystaví.</span><span class="sxs-lookup"><span data-stu-id="9db39-135">If a method in an interface that has hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute does not have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute, that method is not exposed.</span></span> <span data-ttu-id="9db39-136">Kód Hello k těmto úlohám najdete v příkladu hello hello postupem.</span><span class="sxs-lookup"><span data-stu-id="9db39-136">hello code for these tasks is provided in hello example following hello procedure.</span></span> <span data-ttu-id="9db39-137">Diskuzi o kontraktech a službách, najdete v části [návrh a implementace služeb](https://msdn.microsoft.com/library/ms729746.aspx) v hello dokumentaci WCF.</span><span class="sxs-lookup"><span data-stu-id="9db39-137">For a larger discussion of contracts and services, see [Designing and Implementing Services](https://msdn.microsoft.com/library/ms729746.aspx) in hello WCF documentation.</span></span>

### <a name="create-a-relay-contract-with-an-interface"></a><span data-ttu-id="9db39-138">Vytvoření kontraktu předávání s rozhraním</span><span class="sxs-lookup"><span data-stu-id="9db39-138">Create a relay contract with an interface</span></span>

1. <span data-ttu-id="9db39-139">Otevřete Visual Studio jako správce tak, že kliknete pravým tlačítkem na programu hello v hello **spustit** nabídky a výběrem **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="9db39-139">Open Visual Studio as an administrator by right-clicking hello program in hello **Start** menu and selecting **Run as administrator**.</span></span>
2. <span data-ttu-id="9db39-140">Vytvořte nový projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9db39-140">Create a new console application project.</span></span> <span data-ttu-id="9db39-141">Klikněte na tlačítko hello **soubor** nabídku a vyberte **nový**, pak klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="9db39-141">Click hello **File** menu and select **New**, then click **Project**.</span></span> <span data-ttu-id="9db39-142">V hello **nový projekt** dialogové okno, klikněte na tlačítko **Visual C#** (Pokud **Visual C#** nezobrazí, podívejte se do části **jiné jazyky**).</span><span class="sxs-lookup"><span data-stu-id="9db39-142">In hello **New Project** dialog, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**).</span></span> <span data-ttu-id="9db39-143">Klikněte na tlačítko hello **konzolovou aplikaci (rozhraní .NET Framework)** šablony a pojmenujte ji **EchoService**.</span><span class="sxs-lookup"><span data-stu-id="9db39-143">Click hello **Console App (.NET Framework)** template, and name it **EchoService**.</span></span> <span data-ttu-id="9db39-144">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="9db39-144">Click **OK** toocreate hello project.</span></span>

    ![][2]

3. <span data-ttu-id="9db39-145">Nainstalujte balíček Service Bus NuGet hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-145">Install hello Service Bus NuGet package.</span></span> <span data-ttu-id="9db39-146">Tento balíček automaticky přidá Reference knihovny Service Bus toohello, jakož i hello WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="9db39-146">This package automatically adds references toohello Service Bus libraries, as well as hello WCF **System.ServiceModel**.</span></span> <span data-ttu-id="9db39-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je hello obor názvů, který vám umožní tooprogrammatically přístup hello základním funkcím WCF.</span><span class="sxs-lookup"><span data-stu-id="9db39-147">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is hello namespace that enables you tooprogrammatically access hello basic features of WCF.</span></span> <span data-ttu-id="9db39-148">Service Bus používá mnoho objektů hello a atributy kontrakty toodefine služeb WCF.</span><span class="sxs-lookup"><span data-stu-id="9db39-148">Service Bus uses many of hello objects and attributes of WCF toodefine service contracts.</span></span>

    <span data-ttu-id="9db39-149">V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello a pak klikněte na tlačítko **spravovat balíčky NuGet...** . Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="9db39-149">In Solution Explorer, right-click hello project, and then click **Manage NuGet Packages...**. Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="9db39-150">Zkontrolujte, že název projektu hello je vybraný v hello **verze** pole.</span><span class="sxs-lookup"><span data-stu-id="9db39-150">Ensure that hello project name is selected in hello **Version(s)** box.</span></span> <span data-ttu-id="9db39-151">Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-151">Click **Install**, and accept hello terms of use.</span></span>

    ![][3]
4. <span data-ttu-id="9db39-152">V Průzkumníku řešení klikněte dvakrát na tooopen souboru Program.cs hello ji v editoru hello, pokud ještě není otevřete.</span><span class="sxs-lookup"><span data-stu-id="9db39-152">In Solution Explorer, double-click hello Program.cs file tooopen it in hello editor, if it is not already open.</span></span>
5. <span data-ttu-id="9db39-153">Přidejte následující hello pomocí příkazů v horní části hello hello souboru:</span><span class="sxs-lookup"><span data-stu-id="9db39-153">Add hello following using statements at hello top of hello file:</span></span>

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. <span data-ttu-id="9db39-154">Změna hello název oboru názvů z výchozího názvu **EchoService** příliš**Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="9db39-154">Change hello namespace name from its default name of **EchoService** too**Microsoft.ServiceBus.Samples**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="9db39-155">Tento kurz používá obor názvů hello C# **Microsoft.ServiceBus.Samples**, které je obor názvů hello hello na základě smlouvy spravovaný typ, který se používá v konfiguračním souboru hello v hello [konfigurace klienta WCF hello](#configure-the-wcf-client) krok.</span><span class="sxs-lookup"><span data-stu-id="9db39-155">This tutorial uses hello C# namespace **Microsoft.ServiceBus.Samples**, which is hello namespace of hello contract-based managed type that is used in hello configuration file in hello [Configure hello WCF client](#configure-the-wcf-client) step.</span></span> <span data-ttu-id="9db39-156">Můžete zadat obor názvů, které chcete při sestavování této ukázky; kurz hello však nebude fungovat nezměníte pak hello názvů hello kontraktu a služby podle toho, v konfiguračním souboru aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-156">You can specify any namespace you want when you build this sample; however, hello tutorial will not work unless you then modify hello namespaces of hello contract and service accordingly, in hello application configuration file.</span></span> <span data-ttu-id="9db39-157">Hello obor názvů specifikovaný v hello musí být soubor App.config hello stejné jako hello obor názvů zadaný ve vašich souborech C#.</span><span class="sxs-lookup"><span data-stu-id="9db39-157">hello namespace specified in hello App.config file must be hello same as hello namespace specified in your C# files.</span></span>
   >
   >
7. <span data-ttu-id="9db39-158">Přímo po hello `Microsoft.ServiceBus.Samples` deklaraci oboru názvů, ale v rámci hello názvů definujte nové rozhraní s názvem `IEchoContract` a použít hello `ServiceContractAttribute` rozhraní toohello atributu s hodnotou oboru názvů `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="9db39-158">Directly after hello `Microsoft.ServiceBus.Samples` namespace declaration, but within hello namespace, define a new interface named `IEchoContract` and apply hello `ServiceContractAttribute` attribute toohello interface with a namespace value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="9db39-159">Hodnota oboru názvů Hello se liší od hello obor názvů, který používáte v rámci oboru hello kódu.</span><span class="sxs-lookup"><span data-stu-id="9db39-159">hello namespace value differs from hello namespace that you use throughout hello scope of your code.</span></span> <span data-ttu-id="9db39-160">Místo toho hello hodnotu oboru názvů se používá jako jedinečný identifikátor pro tento kontrakt.</span><span class="sxs-lookup"><span data-stu-id="9db39-160">Instead, hello namespace value is used as a unique identifier for this contract.</span></span> <span data-ttu-id="9db39-161">Zadání oboru názvů hello explicitně brání přidání názvu kontraktu toohello hello výchozí hodnoty oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="9db39-161">Specifying hello namespace explicitly prevents hello default namespace value from being added toohello contract name.</span></span> <span data-ttu-id="9db39-162">Vložte následující kód po deklaraci oboru názvů hello hello:</span><span class="sxs-lookup"><span data-stu-id="9db39-162">Paste hello following code after hello namespace declaration:</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="9db39-163">Hello obor názvů kontraktu služby obvykle obsahuje schéma pojmenování s informacemi o verzi.</span><span class="sxs-lookup"><span data-stu-id="9db39-163">Typically, hello service contract namespace contains a naming scheme that includes version information.</span></span> <span data-ttu-id="9db39-164">Hlavní změny tooisolate služeb včetně informací o verzi v oboru názvů kontraktu služby hello umožňuje definováním nové kontrakt služby s nový obor názvů a která bude vystavená na nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9db39-164">Including version information in hello service contract namespace enables services tooisolate major changes by defining a new service contract with a new namespace and exposing it on a new endpoint.</span></span> <span data-ttu-id="9db39-165">Tímto způsobem můžou klienti pokračovat toouse hello starého kontraktu služby bez nutnosti toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="9db39-165">In this manner, clients can continue toouse hello old service contract without having toobe updated.</span></span> <span data-ttu-id="9db39-166">Informace o verzi může mít podobu data nebo čísla sestavení.</span><span class="sxs-lookup"><span data-stu-id="9db39-166">Version information can consist of a date or a build number.</span></span> <span data-ttu-id="9db39-167">Další informace najdete v článku o [Správa verzí služeb](http://go.microsoft.com/fwlink/?LinkID=180498).</span><span class="sxs-lookup"><span data-stu-id="9db39-167">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="9db39-168">Pro účely tohoto kurzu hello hello pojmenování schématu oboru názvů kontraktu služby hello neobsahuje informace o verzi.</span><span class="sxs-lookup"><span data-stu-id="9db39-168">For hello purposes of this tutorial, hello naming scheme of hello service contract namespace does not contain version information.</span></span>
   >
   >
8. <span data-ttu-id="9db39-169">V rámci hello `IEchoContract` rozhraní, deklarujte metodu pro jednu operaci hello hello `IEchoContract` kontrakt zpřístupňuje v hello rozhraní a použít hello `OperationContractAttribute` způsob toohello atribut, který má tooexpose jako součást hello veřejné předávání WCF smluvním vztahu, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9db39-169">Within hello `IEchoContract` interface, declare a method for hello single operation hello `IEchoContract` contract exposes in hello interface and apply hello `OperationContractAttribute` attribute toohello method that you want tooexpose as part of hello public WCF Relay contract, as follows:</span></span>

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. <span data-ttu-id="9db39-170">Přímo po hello `IEchoContract` definici rozhraní, deklarujte kanál, který zdědí vlastnosti z obou `IEchoContract` a také toohello `IClientChannel` rozhraní, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="9db39-170">Directly after hello `IEchoContract` interface definition, declare a channel that inherits from both `IEchoContract` and also toohello `IClientChannel` interface, as shown here:</span></span>

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    <span data-ttu-id="9db39-171">Kanál je objekt WCF hello hello hostitele a klienty průchodu, která tooeach informace o dalších.</span><span class="sxs-lookup"><span data-stu-id="9db39-171">A channel is hello WCF object through which hello host and client pass information tooeach other.</span></span> <span data-ttu-id="9db39-172">Později napíšete kód proti hello kanál tooecho informace mezi dvěma aplikacemi hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-172">Later, you will write code against hello channel tooecho information between hello two applications.</span></span>
10. <span data-ttu-id="9db39-173">Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** nebo stiskněte klávesu **Ctrl + Shift + B** tooconfirm hello přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="9db39-173">From hello **Build** menu, click **Build Solution** or press **Ctrl+Shift+B** tooconfirm hello accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="9db39-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="9db39-174">Example</span></span>

<span data-ttu-id="9db39-175">Hello následující kód ukazuje základní rozhraní, která definuje kontraktu WCF předávání.</span><span class="sxs-lookup"><span data-stu-id="9db39-175">hello following code shows a basic interface that defines a WCF Relay contract.</span></span>

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

<span data-ttu-id="9db39-176">Je teď vytvořená hello rozhraní, můžete implementovat rozhraní hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-176">Now that hello interface is created, you can implement hello interface.</span></span>

## <a name="implement-hello-wcf-contract"></a><span data-ttu-id="9db39-177">Implementujte kontrakt WFG hello</span><span class="sxs-lookup"><span data-stu-id="9db39-177">Implement hello WCF contract</span></span>

<span data-ttu-id="9db39-178">Vytvoření Azure předávání vyžaduje, abyste nejdřív vytvořili kontrakt hello, který se definuje pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9db39-178">Creating an Azure relay requires that you first create hello contract, which is defined by using an interface.</span></span> <span data-ttu-id="9db39-179">Další informace o vytváření rozhraní hello najdete v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-179">For more information about creating hello interface, see hello previous step.</span></span> <span data-ttu-id="9db39-180">dalším krokem Hello je tooimplement hello rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9db39-180">hello next step is tooimplement hello interface.</span></span> <span data-ttu-id="9db39-181">To zahrnuje vytvoření třídy s názvem `EchoService` hello definovaný uživatelem, která implementuje `IEchoContract` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9db39-181">This involves creating a class named `EchoService` that implements hello user-defined `IEchoContract` interface.</span></span> <span data-ttu-id="9db39-182">Po implementaci rozhraní hello nakonfigurujete hello rozhraní pomocí souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="9db39-182">After you implement hello interface, you then configure hello interface using an App.config configuration file.</span></span> <span data-ttu-id="9db39-183">Hello konfigurační soubor obsahuje informace nutné k hello aplikaci, například název hello hello služby, název hello hello kontraktu a typ hello protokol, který je použité toocommunicate službou předávání přes hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-183">hello configuration file contains necessary information for hello application, such as hello name of hello service, hello name of hello contract, and hello type of protocol that is used toocommunicate with hello relay service.</span></span> <span data-ttu-id="9db39-184">Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.</span><span class="sxs-lookup"><span data-stu-id="9db39-184">hello code used for these tasks is provided in hello example following hello procedure.</span></span> <span data-ttu-id="9db39-185">Obecnější diskuzi o způsobu tooimplement služby smlouvy, najdete v části [implementace kontraktů služby](https://msdn.microsoft.com/library/ms733764.aspx) v hello dokumentaci WCF.</span><span class="sxs-lookup"><span data-stu-id="9db39-185">For a more general discussion about how tooimplement a service contract, see [Implementing Service Contracts](https://msdn.microsoft.com/library/ms733764.aspx) in hello WCF documentation.</span></span>

1. <span data-ttu-id="9db39-186">Vytvořte novou třídu s názvem `EchoService` přímo po definici hello hello `IEchoContract` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9db39-186">Create a new class named `EchoService` directly after hello definition of hello `IEchoContract` interface.</span></span> <span data-ttu-id="9db39-187">Hello `EchoService` třída implementuje hello `IEchoContract` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9db39-187">hello `EchoService` class implements hello `IEchoContract` interface.</span></span>

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    <span data-ttu-id="9db39-188">Podobné implementace rozhraní tooother, definice hello můžete implementovat v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="9db39-188">Similar tooother interface implementations, you can implement hello definition in a different file.</span></span> <span data-ttu-id="9db39-189">V tomto kurzu však hello implementace je umístěn ve stejné souboru jako definice rozhraní hello a hello hello `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="9db39-189">However, for this tutorial, hello implementation is located in hello same file as hello interface definition and hello `Main` method.</span></span>
2. <span data-ttu-id="9db39-190">Použít hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) atribut toohello `IEchoContract` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9db39-190">Apply hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute toohello `IEchoContract` interface.</span></span> <span data-ttu-id="9db39-191">Určuje atribut Hello hello název služby a obor názvů.</span><span class="sxs-lookup"><span data-stu-id="9db39-191">hello attribute specifies hello service name and namespace.</span></span> <span data-ttu-id="9db39-192">Až to uděláte, hello `EchoService` třída vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="9db39-192">After doing so, hello `EchoService` class appears as follows:</span></span>

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. <span data-ttu-id="9db39-193">Implementace hello `Echo` metoda definované v hello `IEchoContract` rozhraní v hello `EchoService` třídy.</span><span class="sxs-lookup"><span data-stu-id="9db39-193">Implement hello `Echo` method defined in hello `IEchoContract` interface in hello `EchoService` class.</span></span>

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. <span data-ttu-id="9db39-194">Klikněte na tlačítko **sestavení**, pak klikněte na tlačítko **sestavit řešení** tooconfirm hello přesnost své práce.</span><span class="sxs-lookup"><span data-stu-id="9db39-194">Click **Build**, then click **Build Solution** tooconfirm hello accuracy of your work.</span></span>

### <a name="define-hello-configuration-for-hello-service-host"></a><span data-ttu-id="9db39-195">Definice hello konfigurace pro hostitele služby hello</span><span class="sxs-lookup"><span data-stu-id="9db39-195">Define hello configuration for hello service host</span></span>

1. <span data-ttu-id="9db39-196">Hello konfigurační soubor je velmi podobný konfiguračnímu souboru WCF tooa.</span><span class="sxs-lookup"><span data-stu-id="9db39-196">hello configuration file is very similar tooa WCF configuration file.</span></span> <span data-ttu-id="9db39-197">Obsahuje hello název služby, koncový bod (tedy umístění hello, který předávání přes Azure vystaví pro klienty a hostiteli toocommunicate mezi sebou) a vazbu (typ hello protokolu, který je použité toocommunicate) hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-197">It includes hello service name, endpoint (that is, hello location that Azure Relay exposes for clients and hosts toocommunicate with each other), and hello binding (hello type of protocol that is used toocommunicate).</span></span> <span data-ttu-id="9db39-198">Hello hlavní rozdíl je, že tento nakonfigurovaný koncový bod služby odkazuje tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) vazby, který není součástí hello rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9db39-198">hello main difference is that this configured service endpoint refers tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) binding, which is not part of hello .NET Framework.</span></span> <span data-ttu-id="9db39-199">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) je jedním z hello vazeb definovaných prostředím hello service.</span><span class="sxs-lookup"><span data-stu-id="9db39-199">[NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) is one of hello bindings defined by hello service.</span></span>
2. <span data-ttu-id="9db39-200">V **Průzkumníku**, dvakrát klikněte na tooopen soubor App.config hello ji v editoru Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-200">In **Solution Explorer**, double-click hello App.config file tooopen it in hello Visual Studio editor.</span></span>
3. <span data-ttu-id="9db39-201">V hello `<appSettings>` elementu, nahraďte zástupné symboly hello hello názvem vašeho oboru názvů služby a hello SAS klíč, který jste zkopírovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="9db39-201">In hello `<appSettings>` element, replace hello placeholders with hello name of your service namespace, and hello SAS key that you copied in an earlier step.</span></span>
4. <span data-ttu-id="9db39-202">V rámci hello `<system.serviceModel>` přidat značky, `<services>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9db39-202">Within hello `<system.serviceModel>` tags, add a `<services>` element.</span></span> <span data-ttu-id="9db39-203">V jednom konfiguračním souboru můžete definovat více aplikacích s předáváním.</span><span class="sxs-lookup"><span data-stu-id="9db39-203">You can define multiple relay applications in a single configuration file.</span></span> <span data-ttu-id="9db39-204">V tomto kurzu se ale definuje jen jedna.</span><span class="sxs-lookup"><span data-stu-id="9db39-204">However, this tutorial defines only one.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. <span data-ttu-id="9db39-205">V rámci hello `<services>` elementu, přidejte `<service>` element toodefine hello název služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-205">Within hello `<services>` element, add a `<service>` element toodefine hello name of hello service.</span></span>

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. <span data-ttu-id="9db39-206">V rámci hello `<service>` elementu, definujte hello umístění hello koncového bodu kontraktu a také hello typ vazby pro koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-206">Within hello `<service>` element, define hello location of hello endpoint contract, and also hello type of binding for hello endpoint.</span></span>

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="9db39-207">koncový bod Hello definuje, kde bude hello klient hledat hostitelskou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-207">hello endpoint defines where hello client will look for hello host application.</span></span> <span data-ttu-id="9db39-208">Kurz hello později, používá tento krok toocreate identifikátor URI, která plně vystavuje hostitele hello prostřednictvím předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="9db39-208">Later, hello tutorial uses this step toocreate a URI that fully exposes hello host through Azure Relay.</span></span> <span data-ttu-id="9db39-209">Hello vazba deklaruje, že používáme TCP jako hello protokol toocommunicate službou předávání přes hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-209">hello binding declares that we are using TCP as hello protocol toocommunicate with hello relay service.</span></span>
7. <span data-ttu-id="9db39-210">Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** tooconfirm hello přesnost své práce.</span><span class="sxs-lookup"><span data-stu-id="9db39-210">From hello **Build** menu, click **Build Solution** tooconfirm hello accuracy of your work.</span></span>

### <a name="example"></a><span data-ttu-id="9db39-211">Příklad</span><span class="sxs-lookup"><span data-stu-id="9db39-211">Example</span></span>

<span data-ttu-id="9db39-212">Hello následující kód ukazuje implementaci kontraktu služby hello hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-212">hello following code shows hello implementation of hello service contract.</span></span>

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

<span data-ttu-id="9db39-213">Hello následující kód ukazuje základní formát souboru App.config hello přidružený k hostiteli služby hello hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-213">hello following code shows hello basic format of hello App.config file associated with hello service host.</span></span>

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

## <a name="host-and-run-a-basic-web-service-tooregister-with-hello-relay-service"></a><span data-ttu-id="9db39-214">Hostování a spuštění tooregister základní webové služby s hello předávací služba</span><span class="sxs-lookup"><span data-stu-id="9db39-214">Host and run a basic web service tooregister with hello relay service</span></span>

<span data-ttu-id="9db39-215">Tento krok popisuje, jak toorun Azure předávání služby.</span><span class="sxs-lookup"><span data-stu-id="9db39-215">This step describes how toorun an Azure Relay service.</span></span>

### <a name="create-hello-relay-credentials"></a><span data-ttu-id="9db39-216">Vytvoření hello předávání pověření</span><span class="sxs-lookup"><span data-stu-id="9db39-216">Create hello relay credentials</span></span>

1. <span data-ttu-id="9db39-217">V `Main()`, vytvořte dvě proměnné v oboru názvů které toostore hello a hello SAS klíč, který se načítají z okna konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-217">In `Main()`, create two variables in which toostore hello namespace and hello SAS key that are read from hello console window.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    <span data-ttu-id="9db39-218">klíč SAS Hello bude použít novější tooaccess projektu.</span><span class="sxs-lookup"><span data-stu-id="9db39-218">hello SAS key will be used later tooaccess your project.</span></span> <span data-ttu-id="9db39-219">obor názvů Hello se předá jako parametr příliš`CreateServiceUri` toocreate URI služby.</span><span class="sxs-lookup"><span data-stu-id="9db39-219">hello namespace is passed as a parameter too`CreateServiceUri` toocreate a service URI.</span></span>
2. <span data-ttu-id="9db39-220">Použití [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) objektu, deklarovat, že budete používat klíč SAS jako typ přihlašovacích údajů hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-220">Using a [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) object, declare that you will be using a SAS key as hello credential type.</span></span> <span data-ttu-id="9db39-221">Přidejte následující kód přímo po kódu hello přidali v posledním kroku hello hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-221">Add hello following code directly after hello code added in hello last step.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-hello-service"></a><span data-ttu-id="9db39-222">Vytvoření bázové adresy pro službu hello</span><span class="sxs-lookup"><span data-stu-id="9db39-222">Create a base address for hello service</span></span>

<span data-ttu-id="9db39-223">Po hello kódu, které jste přidali v posledním kroku hello, vytvořte `Uri` pro základní adresu hello instanci služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-223">After hello code you added in hello last step, create a `Uri` instance for hello base address of hello service.</span></span> <span data-ttu-id="9db39-224">Toto URI specifikuje schéma Service Bus hello hello obor názvů a hello cestu rozhraní služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-224">This URI specifies hello Service Bus scheme, hello namespace, and hello path of hello service interface.</span></span>

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

<span data-ttu-id="9db39-225">"sb" je zkratka schématu Service Bus hello a určuje, že používáme protokol TCP jako protokol pro hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-225">"sb" is an abbreviation for hello Service Bus scheme, and indicates that we are using TCP as hello protocol.</span></span> <span data-ttu-id="9db39-226">To jsme předtím indikovali v konfiguračním souboru hello, pokud [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) byl zadán jako hello vazby.</span><span class="sxs-lookup"><span data-stu-id="9db39-226">This was also previously indicated in hello configuration file, when [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) was specified as hello binding.</span></span>

<span data-ttu-id="9db39-227">V tomto kurzu hello identifikátor URI je `sb://putServiceNamespaceHere.windows.net/EchoService`.</span><span class="sxs-lookup"><span data-stu-id="9db39-227">For this tutorial, hello URI is `sb://putServiceNamespaceHere.windows.net/EchoService`.</span></span>

### <a name="create-and-configure-hello-service-host"></a><span data-ttu-id="9db39-228">Vytvoření a konfigurace hostitele služby hello</span><span class="sxs-lookup"><span data-stu-id="9db39-228">Create and configure hello service host</span></span>

1. <span data-ttu-id="9db39-229">Nastavte režim připojení hello příliš`AutoDetect`.</span><span class="sxs-lookup"><span data-stu-id="9db39-229">Set hello connectivity mode too`AutoDetect`.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    <span data-ttu-id="9db39-230">Hello režim připojení popisuje hello protokol hello služby používá toocommunicate předávací službou hello; pomocí protokolu HTTP nebo TCP.</span><span class="sxs-lookup"><span data-stu-id="9db39-230">hello connectivity mode describes hello protocol hello service uses toocommunicate with hello relay service; either HTTP or TCP.</span></span> <span data-ttu-id="9db39-231">Pomocí výchozího nastavení hello `AutoDetect`, hello služby pokusí tooconnect tooAzure předávání přes TCP, pokud je k dispozici a HTTP, pokud TCP není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9db39-231">Using hello default setting `AutoDetect`, hello service attempts tooconnect tooAzure Relay over TCP if it is available, and HTTP if TCP is not available.</span></span> <span data-ttu-id="9db39-232">Všimněte si, že to se liší od služby hello protokolu hello specifikuje pro komunikaci klienta.</span><span class="sxs-lookup"><span data-stu-id="9db39-232">Note that this differs from hello protocol hello service specifies for client communication.</span></span> <span data-ttu-id="9db39-233">Tento protokol se určuje podle hello vazby použít.</span><span class="sxs-lookup"><span data-stu-id="9db39-233">That protocol is determined by hello binding used.</span></span> <span data-ttu-id="9db39-234">Službu můžete například použít hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) vazba, která určuje, že její koncový bod s klienty komunikuje přes HTTP.</span><span class="sxs-lookup"><span data-stu-id="9db39-234">For example, a service can use hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) binding, which specifies that its endpoint communicates with clients over HTTP.</span></span> <span data-ttu-id="9db39-235">Že stejné služba by mohla specifikovat **ConnectivityMode.AutoDetect** tak, aby služba hello komunikuje s Azure předávání přes protokol TCP.</span><span class="sxs-lookup"><span data-stu-id="9db39-235">That same service could specify **ConnectivityMode.AutoDetect** so that hello service communicates with Azure Relay over TCP.</span></span>
2. <span data-ttu-id="9db39-236">Vytvořte hostitele služby hello, pomocí hello URI vytvořený dříve v této části.</span><span class="sxs-lookup"><span data-stu-id="9db39-236">Create hello service host, using hello URI created earlier in this section.</span></span>

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    <span data-ttu-id="9db39-237">Hostitel služby Hello je objekt WCF hello, který vytvoří instanci služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-237">hello service host is hello WCF object that instantiates hello service.</span></span> <span data-ttu-id="9db39-238">Tady jí předáte typ hello služby chcete toocreate ( `EchoService` typu) a také toohello adresu, na které má služba tooexpose hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-238">Here, you pass it hello type of service you want toocreate (an `EchoService` type), and also toohello address at which you want tooexpose hello service.</span></span>
3. <span data-ttu-id="9db39-239">Hello horní části souboru Program.cs hello, přidejte odkazy na příliš[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) a [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span><span class="sxs-lookup"><span data-stu-id="9db39-239">At hello top of hello Program.cs file, add references too[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) and [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).</span></span>

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. <span data-ttu-id="9db39-240">Zpět v `Main()`, nakonfigurujte hello koncový bod tooenable veřejný přístup.</span><span class="sxs-lookup"><span data-stu-id="9db39-240">Back in `Main()`, configure hello endpoint tooenable public access.</span></span>

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    <span data-ttu-id="9db39-241">Tento krok informuje hello předávací služba, že vaše aplikace dá veřejně najít tak, že prověří hello informačního kanálu ATOM pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="9db39-241">This step informs hello relay service that your application can be found publicly by examining hello ATOM feed for your project.</span></span> <span data-ttu-id="9db39-242">Pokud nastavíte **DiscoveryType** příliš**privátní**, klient bude stále možné tooaccess hello služby.</span><span class="sxs-lookup"><span data-stu-id="9db39-242">If you set **DiscoveryType** too**private**, a client would still be able tooaccess hello service.</span></span> <span data-ttu-id="9db39-243">Ale hello by se při vyhledávání hello předávání názvů.</span><span class="sxs-lookup"><span data-stu-id="9db39-243">However, hello service would not appear when it searches hello Relay namespace.</span></span> <span data-ttu-id="9db39-244">Hello klient místo toho by měla mít cesta ke koncovému bodu hello tooknow předem.</span><span class="sxs-lookup"><span data-stu-id="9db39-244">Instead, hello client would have tooknow hello endpoint path beforehand.</span></span>
5. <span data-ttu-id="9db39-245">Použít pověření služby hello toohello koncové body služby definované v souboru App.config hello:</span><span class="sxs-lookup"><span data-stu-id="9db39-245">Apply hello service credentials toohello service endpoints defined in hello App.config file:</span></span>

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    <span data-ttu-id="9db39-246">Jak jsme uvedli v předchozím kroku hello, deklarovat vám může několik služeb a koncové body v konfiguračním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-246">As stated in hello previous step, you could have declared multiple services and endpoints in hello configuration file.</span></span> <span data-ttu-id="9db39-247">Pokud jste měli, tento kód by prošel konfigurační soubor hello a vyhledávání pro každý koncový bod toowhich platit vaše přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9db39-247">If you had, this code would traverse hello configuration file and search for every endpoint toowhich it should apply your credentials.</span></span> <span data-ttu-id="9db39-248">V tomto kurzu má hello konfigurační soubor pouze jeden koncový bod.</span><span class="sxs-lookup"><span data-stu-id="9db39-248">However, for this tutorial, hello configuration file has only one endpoint.</span></span>

### <a name="open-hello-service-host"></a><span data-ttu-id="9db39-249">Hostitel služby otevřete hello</span><span class="sxs-lookup"><span data-stu-id="9db39-249">Open hello service host</span></span>

1. <span data-ttu-id="9db39-250">Otevřete službu hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-250">Open hello service.</span></span>

    ```csharp
    host.Open();
    ```
2. <span data-ttu-id="9db39-251">Informujte hello uživatele, který hello služby používá a popisují, jak tooshut dolů hello služby.</span><span class="sxs-lookup"><span data-stu-id="9db39-251">Inform hello user that hello service is running, and explain how tooshut down hello service.</span></span>

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="9db39-252">Po dokončení zavřete hostitele služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-252">When finished, close hello service host.</span></span>

    ```csharp
    host.Close();
    ```
4. <span data-ttu-id="9db39-253">Stiskněte klávesu **Ctrl + Shift + B** toobuild hello projektu.</span><span class="sxs-lookup"><span data-stu-id="9db39-253">Press **Ctrl+Shift+B** toobuild hello project.</span></span>

### <a name="example"></a><span data-ttu-id="9db39-254">Příklad</span><span class="sxs-lookup"><span data-stu-id="9db39-254">Example</span></span>

<span data-ttu-id="9db39-255">Kódu dokončené služby by měl vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="9db39-255">Your completed service code should appear as follows.</span></span> <span data-ttu-id="9db39-256">Hello kód obsahuje hello kontrakt a implementaci služby z předchozích kroků v kurzu hello a hostitelé hello službu v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9db39-256">hello code includes hello service contract and implementation from previous steps in hello tutorial, and hosts hello service in a console application.</span></span>

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

## <a name="create-a-wcf-client-for-hello-service-contract"></a><span data-ttu-id="9db39-257">Vytvoření klienta WCF pro kontrakt služby hello</span><span class="sxs-lookup"><span data-stu-id="9db39-257">Create a WCF client for hello service contract</span></span>

<span data-ttu-id="9db39-258">dalším krokem Hello je toocreate klientské aplikace a definovat hello smlouvu, kterou implementujete v pozdějších krocích.</span><span class="sxs-lookup"><span data-stu-id="9db39-258">hello next step is toocreate a client application and define hello service contract you will implement in later steps.</span></span> <span data-ttu-id="9db39-259">Upozorňujeme, že hodně těchto kroků vypadat hello kroky použít toocreate služby: definování kontraktu, upravení App.config souboru, pomocí přihlašovacích údajů tooconnect toohello předávací služby a tak dále.</span><span class="sxs-lookup"><span data-stu-id="9db39-259">Note that many of these steps resemble hello steps used toocreate a service: defining a contract, editing an App.config file, using credentials tooconnect toohello relay service, and so on.</span></span> <span data-ttu-id="9db39-260">Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.</span><span class="sxs-lookup"><span data-stu-id="9db39-260">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

1. <span data-ttu-id="9db39-261">Vytvořte nový projekt v hello aktuální řešení sady Visual Studio pro klienta hello díky hello následující:</span><span class="sxs-lookup"><span data-stu-id="9db39-261">Create a new project in hello current Visual Studio solution for hello client by doing hello following:</span></span>

   1. <span data-ttu-id="9db39-262">V Průzkumníku řešení v hello stejném řešení, které obsahuje službu hello, klikněte pravým tlačítkem na hello aktuální řešení (ne hello projekt) a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9db39-262">In Solution Explorer, in hello same solution that contains hello service, right-click hello current solution (not hello project), and click **Add**.</span></span> <span data-ttu-id="9db39-263">Pak klikněte na **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="9db39-263">Then click **New Project**.</span></span>
   2. <span data-ttu-id="9db39-264">V hello **přidat nový projekt** dialogové okno, klikněte na tlačítko **Visual C#** (Pokud **Visual C#** nezobrazí, podívejte se do části **jiné jazyky**), vyberte hello **Konzolovou aplikaci (rozhraní .NET Framework)** šablony a pojmenujte ji **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="9db39-264">In hello **Add New Project** dialog box, click **Visual C#** (if **Visual C#** does not appear, look under **Other Languages**), select hello **Console App (.NET Framework)** template, and name it **EchoClient**.</span></span>
   3. <span data-ttu-id="9db39-265">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9db39-265">Click **OK**.</span></span>
      <br />
2. <span data-ttu-id="9db39-266">V Průzkumníku řešení poklikejte na soubor Program.cs hello v hello **EchoClient** projektu tooopen ji v editoru hello, pokud ještě není otevřete.</span><span class="sxs-lookup"><span data-stu-id="9db39-266">In Solution Explorer, double-click hello Program.cs file in hello **EchoClient** project tooopen it in hello editor, if it is not already open.</span></span>
3. <span data-ttu-id="9db39-267">Změna hello název oboru názvů z výchozího názvu `EchoClient` příliš`Microsoft.ServiceBus.Samples`.</span><span class="sxs-lookup"><span data-stu-id="9db39-267">Change hello namespace name from its default name of `EchoClient` too`Microsoft.ServiceBus.Samples`.</span></span>
4. <span data-ttu-id="9db39-268">Nainstalujte hello [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus): v Průzkumníku řešení klikněte pravým tlačítkem na hello **EchoClient** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9db39-268">Install hello [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus): in Solution Explorer, right-click hello **EchoClient** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="9db39-269">Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="9db39-269">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="9db39-270">Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-270">Click **Install**, and accept hello terms of use.</span></span>

    ![][3]
5. <span data-ttu-id="9db39-271">Přidat `using` příkaz pro hello [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) oboru názvů v souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-271">Add a `using` statement for hello [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) namespace in hello Program.cs file.</span></span>

    ```csharp
    using System.ServiceModel;
    ```
6. <span data-ttu-id="9db39-272">Přidejte hello Definice toohello obor názvů kontraktu služby, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-272">Add hello service contract definition toohello namespace, as shown in hello following example.</span></span> <span data-ttu-id="9db39-273">Upozorňujeme, že je tato definice identické toohello definice použitá v hello **služby** projektu.</span><span class="sxs-lookup"><span data-stu-id="9db39-273">Note that this definition is identical toohello definition used in hello **Service** project.</span></span> <span data-ttu-id="9db39-274">Měli byste přidat tento kód hello horní části hello `Microsoft.ServiceBus.Samples` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="9db39-274">You should add this code at hello top of hello `Microsoft.ServiceBus.Samples` namespace.</span></span>

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. <span data-ttu-id="9db39-275">Stiskněte klávesu **Ctrl + Shift + B** toobuild hello klienta.</span><span class="sxs-lookup"><span data-stu-id="9db39-275">Press **Ctrl+Shift+B** toobuild hello client.</span></span>

### <a name="example"></a><span data-ttu-id="9db39-276">Příklad</span><span class="sxs-lookup"><span data-stu-id="9db39-276">Example</span></span>

<span data-ttu-id="9db39-277">Hello následující kód ukazuje aktuální stav souboru Program.cs hello hello v hello **EchoClient** projektu.</span><span class="sxs-lookup"><span data-stu-id="9db39-277">hello following code shows hello current status of hello Program.cs file in hello **EchoClient** project.</span></span>

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

## <a name="configure-hello-wcf-client"></a><span data-ttu-id="9db39-278">Konfigurace klienta WCF hello</span><span class="sxs-lookup"><span data-stu-id="9db39-278">Configure hello WCF client</span></span>

<span data-ttu-id="9db39-279">V tomto kroku vytvoříte soubor App.config pro základní klientskou aplikaci, který přistupuje k službě hello vytvořili dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9db39-279">In this step, you create an App.config file for a basic client application that accesses hello service created previously in this tutorial.</span></span> <span data-ttu-id="9db39-280">Tento soubor App.config definuje hello kontrakt, vazbu a název koncového bodu hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-280">This App.config file defines hello contract, binding, and name of hello endpoint.</span></span> <span data-ttu-id="9db39-281">Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.</span><span class="sxs-lookup"><span data-stu-id="9db39-281">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

1. <span data-ttu-id="9db39-282">V Průzkumníku řešení v hello **EchoClient** projektu, klikněte dvakrát na **App.config** tooopen hello souboru v editoru Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-282">In Solution Explorer, in hello **EchoClient** project, double-click **App.config** tooopen hello file in hello Visual Studio editor.</span></span>
2. <span data-ttu-id="9db39-283">V hello `<appSettings>` elementu, nahraďte zástupné symboly hello hello názvem vašeho oboru názvů služby a hello SAS klíč, který jste zkopírovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="9db39-283">In hello `<appSettings>` element, replace hello placeholders with hello name of your service namespace, and hello SAS key that you copied in an earlier step.</span></span>
3. <span data-ttu-id="9db39-284">V rámci elementu system.serviceModel hello, přidejte `<client>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9db39-284">Within hello system.serviceModel element, add a `<client>` element.</span></span>

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    <span data-ttu-id="9db39-285">Tento krok deklaruje, že definujete klientskou aplikaci ve stylu WCF.</span><span class="sxs-lookup"><span data-stu-id="9db39-285">This step declares that you are defining a WCF-style client application.</span></span>
4. <span data-ttu-id="9db39-286">V rámci hello `client` elementu, definujte hello název, kontrakt a typ vazby pro koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-286">Within hello `client` element, define hello name, contract, and binding type for hello endpoint.</span></span>

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    <span data-ttu-id="9db39-287">Tento krok definuje název hello hello koncového bodu, hello kontrakt definovaný ve hello služby a hello fakt, že klientská aplikace hello používá TCP toocommunicate s předávání přes Azure.</span><span class="sxs-lookup"><span data-stu-id="9db39-287">This step defines hello name of hello endpoint, hello contract defined in hello service, and hello fact that hello client application uses TCP toocommunicate with Azure Relay.</span></span> <span data-ttu-id="9db39-288">Hello název koncového bodu se používá v hello dalším krokem toolink této konfigurace koncového bodu s URI služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-288">hello endpoint name is used in hello next step toolink this endpoint configuration with hello service URI.</span></span>
5. <span data-ttu-id="9db39-289">Klikněte na tlačítko **soubor**, pak klikněte na tlačítko **Uložit vše**.</span><span class="sxs-lookup"><span data-stu-id="9db39-289">Click **File**, then click **Save All**.</span></span>

## <a name="example"></a><span data-ttu-id="9db39-290">Příklad</span><span class="sxs-lookup"><span data-stu-id="9db39-290">Example</span></span>

<span data-ttu-id="9db39-291">Hello následující kód ukazuje soubor App.config hello pro klienta Echo hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-291">hello following code shows hello App.config file for hello Echo client.</span></span>

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

## <a name="implement-hello-wcf-client"></a><span data-ttu-id="9db39-292">Implementace klienta WCF hello</span><span class="sxs-lookup"><span data-stu-id="9db39-292">Implement hello WCF client</span></span>
<span data-ttu-id="9db39-293">V tomto kroku implementujete základní klientskou aplikaci, který přistupuje k hello službu, kterou jste vytvořili dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9db39-293">In this step, you implement a basic client application that accesses hello service you created previously in this tutorial.</span></span> <span data-ttu-id="9db39-294">Podobně jako toohello služby hello klient provádí spoustu hello stejné operace tooaccess předávání přes Azure:</span><span class="sxs-lookup"><span data-stu-id="9db39-294">Similar toohello service, hello client performs many of hello same operations tooaccess Azure Relay:</span></span>

1. <span data-ttu-id="9db39-295">Nastaví režim připojení hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-295">Sets hello connectivity mode.</span></span>
2. <span data-ttu-id="9db39-296">Vytvoří hello identifikátor URI, který vyhledá hello hostitele služby.</span><span class="sxs-lookup"><span data-stu-id="9db39-296">Creates hello URI that locates hello host service.</span></span>
3. <span data-ttu-id="9db39-297">Definuje hello zabezpečovací pověření.</span><span class="sxs-lookup"><span data-stu-id="9db39-297">Defines hello security credentials.</span></span>
4. <span data-ttu-id="9db39-298">Platí hello pověření toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="9db39-298">Applies hello credentials toohello connection.</span></span>
5. <span data-ttu-id="9db39-299">Otevře připojení hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-299">Opens hello connection.</span></span>
6. <span data-ttu-id="9db39-300">Provádí úlohy specifické pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-300">Performs hello application-specific tasks.</span></span>
7. <span data-ttu-id="9db39-301">Zavře připojení hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-301">Closes hello connection.</span></span>

<span data-ttu-id="9db39-302">Jedním z hlavních rozdílů hello je však, že klientská aplikace hello používá službu předávání přes kanál tooconnect toohello, zatímco hello služba používá volání příliš**ServiceHost**.</span><span class="sxs-lookup"><span data-stu-id="9db39-302">However, one of hello main differences is that hello client application uses a channel tooconnect toohello relay service, whereas hello service uses a call too**ServiceHost**.</span></span> <span data-ttu-id="9db39-303">Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.</span><span class="sxs-lookup"><span data-stu-id="9db39-303">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

### <a name="implement-a-client-application"></a><span data-ttu-id="9db39-304">Implementace klientské aplikace</span><span class="sxs-lookup"><span data-stu-id="9db39-304">Implement a client application</span></span>
1. <span data-ttu-id="9db39-305">Nastavte režim připojení hello příliš**AutoDetect**.</span><span class="sxs-lookup"><span data-stu-id="9db39-305">Set hello connectivity mode too**AutoDetect**.</span></span> <span data-ttu-id="9db39-306">Přidejte následující kód do hello hello `Main()` metoda hello **EchoClient** aplikace.</span><span class="sxs-lookup"><span data-stu-id="9db39-306">Add hello following code inside hello `Main()` method of hello **EchoClient** application.</span></span>

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. <span data-ttu-id="9db39-307">Definujte proměnné toohold hello hodnoty pro hello oboru názvů služby a klíče SAS načtené z konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-307">Define variables toohold hello values for hello service namespace, and SAS key that are read from hello console.</span></span>

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. <span data-ttu-id="9db39-308">Vytvořte hello identifikátor URI, který definuje umístění hello hello hostitele ve vašem projektu předávání.</span><span class="sxs-lookup"><span data-stu-id="9db39-308">Create hello URI that defines hello location of hello host in your Relay project.</span></span>

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. <span data-ttu-id="9db39-309">Vytvoření objektu hello přihlašovací údaje pro svůj koncový bod služby oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="9db39-309">Create hello credential object for your service namespace endpoint.</span></span>

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. <span data-ttu-id="9db39-310">Vytváření hello kanál, který načítá hello konfigurace popsané v souboru App.config hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-310">Create hello channel factory that loads hello configuration described in hello App.config file.</span></span>

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    <span data-ttu-id="9db39-311">Postup kanálu je objekt WCF, který vytvoří kanál, který komunikuje hello služby a klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="9db39-311">A channel factory is a WCF object that creates a channel through which hello service and client applications communicate.</span></span>
6. <span data-ttu-id="9db39-312">Použijte pověření hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-312">Apply hello credentials.</span></span>

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. <span data-ttu-id="9db39-313">Vytvořte a otevřete hello kanálu toohello služby.</span><span class="sxs-lookup"><span data-stu-id="9db39-313">Create and open hello channel toohello service.</span></span>

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. <span data-ttu-id="9db39-314">Zápis hello základní uživatelské rozhraní a funkci pro hello echo.</span><span class="sxs-lookup"><span data-stu-id="9db39-314">Write hello basic user interface and functionality for hello echo.</span></span>

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

    <span data-ttu-id="9db39-315">Všimněte si, že kód hello používá hello instanci objektu kanálu hello jako proxy pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-315">Note that hello code uses hello instance of hello channel object as a proxy for hello service.</span></span>
9. <span data-ttu-id="9db39-316">Zavřete kanál hello a objektu pro vytváření zavřít hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-316">Close hello channel, and close hello factory.</span></span>

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a><span data-ttu-id="9db39-317">Příklad</span><span class="sxs-lookup"><span data-stu-id="9db39-317">Example</span></span>

<span data-ttu-id="9db39-318">Dokončený kód by měly vypadat následovně, znázorňující, jak toocreate klientskou aplikaci, jak toocall hello operace služby hello a jak tooclose hello klienta po operaci hello volání je dokončena.</span><span class="sxs-lookup"><span data-stu-id="9db39-318">Your completed code should appear as follows, showing how toocreate a client application, how toocall hello operations of hello service, and how tooclose hello client after hello operation call is finished.</span></span>

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

## <a name="run-hello-applications"></a><span data-ttu-id="9db39-319">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="9db39-319">Run hello applications</span></span>

1. <span data-ttu-id="9db39-320">Stiskněte klávesu **Ctrl + Shift + B** toobuild hello řešení.</span><span class="sxs-lookup"><span data-stu-id="9db39-320">Press **Ctrl+Shift+B** toobuild hello solution.</span></span> <span data-ttu-id="9db39-321">Toto sestavení projektu klienta hello i hello projekt služby, který jste vytvořili v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-321">This builds both hello client project and hello service project that you created in hello previous steps.</span></span>
2. <span data-ttu-id="9db39-322">Před spuštěné hello klientskou aplikaci musí Ujistěte se, zda je spuštěna aplikace služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-322">Before running hello client application, you must make sure that hello service application is running.</span></span> <span data-ttu-id="9db39-323">V Průzkumníku řešení v sadě Visual Studio, klikněte pravým tlačítkem na hello **EchoService** řešení, pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="9db39-323">In Solution Explorer in Visual Studio, right-click hello **EchoService** solution, then click **Properties**.</span></span>
3. <span data-ttu-id="9db39-324">V hello řešení dialogové okno Vlastnosti, klikněte na **spouštěný projekt**, pak klikněte na tlačítko hello **více projektů po spuštění** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9db39-324">In hello solution properties dialog box, click **Startup Project**, then click hello **Multiple startup projects** button.</span></span> <span data-ttu-id="9db39-325">Zajistěte, aby **EchoService** objeví jako první v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-325">Make sure **EchoService** appears first in hello list.</span></span>
4. <span data-ttu-id="9db39-326">Sada hello **akce** pole pro obě hello **EchoService** a **EchoClient** projekty příliš**spustit**.</span><span class="sxs-lookup"><span data-stu-id="9db39-326">Set hello **Action** box for both hello **EchoService** and **EchoClient** projects too**Start**.</span></span>

    ![][5]
5. <span data-ttu-id="9db39-327">Klikněte na **Závislosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="9db39-327">Click **Project Dependencies**.</span></span> <span data-ttu-id="9db39-328">V hello **projekty** vyberte **EchoClient**.</span><span class="sxs-lookup"><span data-stu-id="9db39-328">In hello **Projects** box, select **EchoClient**.</span></span> <span data-ttu-id="9db39-329">V hello **závisí na** zkontrolujte, zda **EchoService** je zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="9db39-329">In hello **Depends on** box, make sure **EchoService** is checked.</span></span>

    ![][6]
6. <span data-ttu-id="9db39-330">Klikněte na tlačítko **OK** toodismiss hello **vlastnosti** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9db39-330">Click **OK** toodismiss hello **Properties** dialog.</span></span>
7. <span data-ttu-id="9db39-331">Stiskněte klávesu **F5** toorun obou projektů.</span><span class="sxs-lookup"><span data-stu-id="9db39-331">Press **F5** toorun both projects.</span></span>
8. <span data-ttu-id="9db39-332">Obě okna konzoly otevřete a vyzvat vás k názvu oboru názvů hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-332">Both console windows open and prompt you for hello namespace name.</span></span> <span data-ttu-id="9db39-333">Hello služby musíte nejprve spustit, tak v hello **EchoService** okně konzoly, zadejte obor názvů hello a potom stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="9db39-333">hello service must run first, so in hello **EchoService** console window, enter hello namespace and then press **Enter**.</span></span>
9. <span data-ttu-id="9db39-334">Pak se zobrazí výzva k zadání klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="9db39-334">Next, you are prompted for your SAS key.</span></span> <span data-ttu-id="9db39-335">Zadejte klíč SAS hello a stiskněte klávesu ENTER.</span><span class="sxs-lookup"><span data-stu-id="9db39-335">Enter hello SAS key and press ENTER.</span></span>

    <span data-ttu-id="9db39-336">Tady je příklad výstupu z okna konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-336">Here is example output from hello console window.</span></span> <span data-ttu-id="9db39-337">Všimněte si, že zadané hodnoty hello tady jsou například pouze pro účely.</span><span class="sxs-lookup"><span data-stu-id="9db39-337">Note that hello values provided here are for example purposes only.</span></span>

    <span data-ttu-id="9db39-338">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span><span class="sxs-lookup"><span data-stu-id="9db39-338">`Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`</span></span>

    <span data-ttu-id="9db39-339">aplikace služby Hello vytiskne toohello konzoly okno hello adresu, na kterém naslouchá, jak je vidět v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="9db39-339">hello service application prints toohello console window hello address on which it's listening, as seen in hello following example.</span></span>

    <span data-ttu-id="9db39-340">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`</span><span class="sxs-lookup"><span data-stu-id="9db39-340">`Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`</span></span>
10. <span data-ttu-id="9db39-341">V hello **EchoClient** okně konzoly, zadejte text hello stejné informace, které jste zadali dříve pro aplikaci služby hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-341">In hello **EchoClient** console window, enter hello same information that you entered previously for hello service application.</span></span> <span data-ttu-id="9db39-342">Postupujte podle hello předchozí kroky tooenter hello stejný obor názvů služby a SAS klíč hodnoty pro klientské aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-342">Follow hello previous steps tooenter hello same service namespace and SAS key values for hello client application.</span></span>
11. <span data-ttu-id="9db39-343">Po zadání těchto hodnot, hello klient otevře kanál toohello služby a vyzve jste tooenter část textu, jak je vidět v následujícím příkladu výstupu konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-343">After entering these values, hello client opens a channel toohello service and prompts you tooenter some text as seen in hello following console output example.</span></span>

    `Enter text tooecho (or [Enter] tooexit):`

    <span data-ttu-id="9db39-344">Zadejte některé aplikace služby toohello toosend text a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="9db39-344">Enter some text toosend toohello service application and press Enter.</span></span> <span data-ttu-id="9db39-345">Tento text je odeslán toohello služby prostřednictvím hello operace služby Echo a zobrazí se v okně konzoly služby hello jako následující příklad výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-345">This text is sent toohello service through hello Echo service operation and appears in hello service console window as in hello following example output.</span></span>

    `Echoing: My sample text`

    <span data-ttu-id="9db39-346">Hello klientská aplikace obdrží hello návratová hodnota hello `Echo` operaci, která je původní text hello který se vypíše tooits okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="9db39-346">hello client application receives hello return value of hello `Echo` operation, which is hello original text, and prints it tooits console window.</span></span> <span data-ttu-id="9db39-347">Hello následuje příklad výstupu z okna konzoly klienta hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-347">hello following is example output from hello client console window.</span></span>

    `Server echoed: My sample text`
12. <span data-ttu-id="9db39-348">Můžete pokračovat v odesílání zpráv ze služby toohello klienta hello tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="9db39-348">You can continue sending text messages from hello client toohello service in this manner.</span></span> <span data-ttu-id="9db39-349">Jakmile budete hotovi, stiskněte klávesu Enter v hello klientem a službou konzoly windows tooend obě aplikace.</span><span class="sxs-lookup"><span data-stu-id="9db39-349">When you are finished, press Enter in hello client and service console windows tooend both applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9db39-350">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9db39-350">Next steps</span></span>

<span data-ttu-id="9db39-351">Tento kurz vám ukázal, jak toobuild klienta aplikace Azure předávání a služby pomocí hello možnosti WCF předávání přes Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9db39-351">This tutorial showed how toobuild an Azure Relay client application and service using hello WCF Relay capabilities of Service Bus.</span></span> <span data-ttu-id="9db39-352">Podobný kurz, který používá [zasílání zpráv Service Bus](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), najdete v části [začít pracovat s fronty Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="9db39-352">For a similar tutorial that uses [Service Bus Messaging](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), see [Get started with Service Bus queues](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span>

<span data-ttu-id="9db39-353">toolearn Další informace o předávání přes Azure, najdete v následujících tématech hello.</span><span class="sxs-lookup"><span data-stu-id="9db39-353">toolearn more about Azure Relay, see hello following topics.</span></span>

* [<span data-ttu-id="9db39-354">Přehled architektury služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="9db39-354">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [<span data-ttu-id="9db39-355">Přehled služby Azure Relay</span><span class="sxs-lookup"><span data-stu-id="9db39-355">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="9db39-356">Jak toouse hello WCF předávání služby pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9db39-356">How toouse hello WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
