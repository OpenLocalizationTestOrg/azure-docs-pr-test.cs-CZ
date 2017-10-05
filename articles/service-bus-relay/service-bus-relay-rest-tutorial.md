---
title: "Kurz REST pro Service Bus pomocí předávání přes Azure | Microsoft Docs"
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
ms.openlocfilehash: 0db9dbd2d2743907e3f0b259228201d4f5d0c3c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a><span data-ttu-id="154ef-103">Kurz pro Azure předávání přes REST WCF</span><span class="sxs-lookup"><span data-stu-id="154ef-103">Azure WCF Relay REST tutorial</span></span>

<span data-ttu-id="154ef-104">Tento kurz popisuje, jak vytvořit jednoduchou hostitelskou aplikaci předávání přes Azure, která vystavuje rozhraní založené na REST.</span><span class="sxs-lookup"><span data-stu-id="154ef-104">This tutorial describes how to build a simple Azure Relay host application that exposes a REST-based interface.</span></span> <span data-ttu-id="154ef-105">REST webovému klientovi, jako je třeba webový prohlížeč, umožňuje přístup k API pro Service Bus přes požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="154ef-105">REST enables a web client, such as a web browser, to access the Service Bus APIs through HTTP requests.</span></span>

<span data-ttu-id="154ef-106">Tento kurz používá programovací model REST Windows Communication Foundation (WCF) k vytvoření služby v Service Bus.</span><span class="sxs-lookup"><span data-stu-id="154ef-106">The tutorial uses the Windows Communication Foundation (WCF) REST programming model to construct a REST service on Service Bus.</span></span> <span data-ttu-id="154ef-107">Další informace najdete v dokumentaci [Programovací model Rest WCF](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) a [Návrh a implementace služeb](/dotnet/framework/wcf/designing-and-implementing-services).</span><span class="sxs-lookup"><span data-stu-id="154ef-107">For more information, see [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) and [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) in the WCF documentation.</span></span>

## <a name="step-1-create-a-namespace"></a><span data-ttu-id="154ef-108">Krok 1: Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="154ef-108">Step 1: Create a namespace</span></span>

<span data-ttu-id="154ef-109">Pokud chcete začít používat přenosové funkce v Azure, musíte nejdříve vytvořit obor názvů služby.</span><span class="sxs-lookup"><span data-stu-id="154ef-109">To begin using the relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="154ef-110">Obor názvů poskytuje kontejner oboru pro adresování prostředků Azure v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="154ef-110">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="154ef-111">Pokud chcete vytvořit obor názvů Relay, postupujte podle [těchto pokynů](relay-create-namespace-portal.md).</span><span class="sxs-lookup"><span data-stu-id="154ef-111">Follow the [instructions here](relay-create-namespace-portal.md) to create a Relay namespace.</span></span>

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-azure-relay"></a><span data-ttu-id="154ef-112">Krok 2: Definování kontraktu služby WCF na bázi REST pro použití s Azure předávání</span><span class="sxs-lookup"><span data-stu-id="154ef-112">Step 2: Define a REST-based WCF service contract to use with Azure Relay</span></span>

<span data-ttu-id="154ef-113">Při vytváření služby WCF stylu REST, je nutné definovat kontrakt.</span><span class="sxs-lookup"><span data-stu-id="154ef-113">When you create a WCF REST-style service, you must define the contract.</span></span> <span data-ttu-id="154ef-114">Kontrakt určuje, které operace hostitel podporuje.</span><span class="sxs-lookup"><span data-stu-id="154ef-114">The contract specifies what operations the host supports.</span></span> <span data-ttu-id="154ef-115">Operaci služby se můžeme představit jako metodu webové služby.</span><span class="sxs-lookup"><span data-stu-id="154ef-115">A service operation can be thought of as a web service method.</span></span> <span data-ttu-id="154ef-116">Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="154ef-116">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="154ef-117">Každá metoda v rozhraní odpovídá konkrétní operaci služby.</span><span class="sxs-lookup"><span data-stu-id="154ef-117">Each method in the interface corresponds to a specific service operation.</span></span> <span data-ttu-id="154ef-118">Atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) se musí použít na každé rozhraní a atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) se musí použít na každou operaci.</span><span class="sxs-lookup"><span data-stu-id="154ef-118">The [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute must be applied to each interface, and the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute must be applied to each operation.</span></span> <span data-ttu-id="154ef-119">Pokud metoda v rozhraní, které má atribut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx), nemá atribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), taková metoda se nevystaví.</span><span class="sxs-lookup"><span data-stu-id="154ef-119">If a method in an interface that has the [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) does not have the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), that method is not exposed.</span></span> <span data-ttu-id="154ef-120">Kód použitý pro tyto úlohy je v následujícím příkladu za postupem.</span><span class="sxs-lookup"><span data-stu-id="154ef-120">The code used for these tasks is shown in the example following the procedure.</span></span>

<span data-ttu-id="154ef-121">Hlavní rozdíl mezi kontraktu WCF a kontraktu ve stylu REST je přidání vlastnosti do [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="154ef-121">The primary difference between a WCF contract and a REST-style contract is the addition of a property to the [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span></span> <span data-ttu-id="154ef-122">Tato vlastnost vám umožní mapovat metodu ve svém rozhraní k metodě na druhé straně rozhraní.</span><span class="sxs-lookup"><span data-stu-id="154ef-122">This property enables you to map a method in your interface to a method on the other side of the interface.</span></span> <span data-ttu-id="154ef-123">V tomto případě použijeme [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) k propojení metody na HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="154ef-123">In this case, we will use [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) to link a method to HTTP GET.</span></span> <span data-ttu-id="154ef-124">Službě Service Bus tak umožníme přesně získat a interpretovat příkazy odeslané na rozhraní.</span><span class="sxs-lookup"><span data-stu-id="154ef-124">This allows Service Bus to accurately retrieve and interpret commands sent to the interface.</span></span>

### <a name="to-create-a-contract-with-an-interface"></a><span data-ttu-id="154ef-125">K vytvoření smlouvy s rozhraním</span><span class="sxs-lookup"><span data-stu-id="154ef-125">To create a contract with an interface</span></span>

1. <span data-ttu-id="154ef-126">Spusťte Visual Studio jako správce: v nabídce **Start** klikněte na program pravým tlačítkem a vyberte možnost **Spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="154ef-126">Open Visual Studio as an administrator: right-click the program in the **Start** menu, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="154ef-127">Vytvořte nový projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="154ef-127">Create a new console application project.</span></span> <span data-ttu-id="154ef-128">Klikněte na nabídku **Soubor** a vyberte možnost **Nový**, a pak klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="154ef-128">Click the **File** menu and select **New**, then select **Project**.</span></span> <span data-ttu-id="154ef-129">V dialogovém okně **Nový projekt** klikněte na **Visual C#**, vyberte šablonu **Konzolová aplikace** ta zadejte jí název **ImageListener**.</span><span class="sxs-lookup"><span data-stu-id="154ef-129">In the **New Project** dialog box, click **Visual C#**, select the **Console Application** template, and name it **ImageListener**.</span></span> <span data-ttu-id="154ef-130">Použijte výchozí **Umístění**.</span><span class="sxs-lookup"><span data-stu-id="154ef-130">Use the default **Location**.</span></span> <span data-ttu-id="154ef-131">Klikněte na **OK**, tím vytvoříte projekt.</span><span class="sxs-lookup"><span data-stu-id="154ef-131">Click **OK** to create the project.</span></span>
3. <span data-ttu-id="154ef-132">Visual Studio pro projekt C# vytvoří soubor `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="154ef-132">For a C# project, Visual Studio creates a `Program.cs` file.</span></span> <span data-ttu-id="154ef-133">Tato třída obsahuje prázdnou metodu `Main()` potřebnou ke správnému sestavení projektu konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="154ef-133">This class contains an empty `Main()` method, required for a console application project to build correctly.</span></span>
4. <span data-ttu-id="154ef-134">Nainstalujte balíček Service Bus NuGet, tím do projektu přidáte odkazy na Service Bus a **System.ServiceModel.dll**.</span><span class="sxs-lookup"><span data-stu-id="154ef-134">Add references to Service Bus and **System.ServiceModel.dll** to the project by installing the Service Bus NuGet package.</span></span> <span data-ttu-id="154ef-135">Tento balíček automaticky přidá reference na knihovny Service Bus a WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="154ef-135">This package automatically adds references to the Service Bus libraries, as well as the WCF **System.ServiceModel**.</span></span> <span data-ttu-id="154ef-136">V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ImageListener** a pak klikněte na **Správa balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="154ef-136">In Solution Explorer, right-click the **ImageListener** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="154ef-137">Klikněte na kartu **Procházení** a potom najděte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="154ef-137">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="154ef-138">Klikněte na **Instalovat** a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="154ef-138">Click **Install**, and accept the terms of use.</span></span>
5. <span data-ttu-id="154ef-139">Do projektu musíte explicitně přidat odkaz na **System.ServiceModel.Web.dll**:</span><span class="sxs-lookup"><span data-stu-id="154ef-139">You must explicitly add a reference to **System.ServiceModel.Web.dll** to the project:</span></span>
   
    <span data-ttu-id="154ef-140">a.</span><span class="sxs-lookup"><span data-stu-id="154ef-140">a.</span></span> <span data-ttu-id="154ef-141">V Průzkumníku řešení klikněte ve složce projektu pravým tlačítkem na složku **References**, pak klikněte na **Přidat odkaz**.</span><span class="sxs-lookup"><span data-stu-id="154ef-141">In Solution Explorer, right-click the **References** folder under the project folder and then click **Add Reference**.</span></span>
   
    <span data-ttu-id="154ef-142">b.</span><span class="sxs-lookup"><span data-stu-id="154ef-142">b.</span></span> <span data-ttu-id="154ef-143">V dialogovém okně **Přidat odkaz** klikněte vlevo na kartu **Architektura** a do pole **Hledat** zadejte **System.ServiceModel.Web**.</span><span class="sxs-lookup"><span data-stu-id="154ef-143">In the **Add Reference** dialog box, click the **Framework** tab on the left-hand side and in the **Search** box, type **System.ServiceModel.Web**.</span></span> <span data-ttu-id="154ef-144">Označte zatržítko **System.ServiceModel.Web** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="154ef-144">Select the **System.ServiceModel.Web** check box, then click **OK**.</span></span>
6. <span data-ttu-id="154ef-145">Na začátek souboru Program.cs přidejte následující příkazy `using`.</span><span class="sxs-lookup"><span data-stu-id="154ef-145">Add the following `using` statements at the top of the Program.cs file.</span></span>
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    <span data-ttu-id="154ef-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je obor názvů, který umožňuje programový přístup k základním funkcím WCF.</span><span class="sxs-lookup"><span data-stu-id="154ef-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is the namespace that enables programmatic access to basic features of WCF.</span></span> <span data-ttu-id="154ef-147">Předávání WCF používá mnoho objektů a atributů WCF k definování kontraktů služby.</span><span class="sxs-lookup"><span data-stu-id="154ef-147">WCF Relay uses many of the objects and attributes of WCF to define service contracts.</span></span> <span data-ttu-id="154ef-148">Tento obor názvů budete používat ve většině aplikací předávání.</span><span class="sxs-lookup"><span data-stu-id="154ef-148">You will use this namespace in most of your relay applications.</span></span> <span data-ttu-id="154ef-149">Podobně [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) pomáhá definovat kanál, který je objekt, přes který komunikujete se předávání přes Azure a klientským webovým prohlížečem.</span><span class="sxs-lookup"><span data-stu-id="154ef-149">Similarly, [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) helps define the channel, which is the object through which you communicate with Azure Relay and the client web browser.</span></span> <span data-ttu-id="154ef-150">Nakonec [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) obsahuje typy, které vám umožní vytvořit webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="154ef-150">Finally, [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) contains the types that enable you to create web-based applications.</span></span>
7. <span data-ttu-id="154ef-151">Přejmenujte obor názvů `ImageListener` na **Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="154ef-151">Rename the `ImageListener` namespace to **Microsoft.ServiceBus.Samples**.</span></span>
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. <span data-ttu-id="154ef-152">Přímo po otevření složené závorky deklarace oboru názvů definujte nové rozhraní s názvem **IImageContract** a aplikujte atribut **ServiceContractAttribute** na rozhraní s hodnotou `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="154ef-152">Directly after the opening curly brace of the namespace declaration, define a new interface named **IImageContract** and apply the **ServiceContractAttribute** attribute to the interface with a value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="154ef-153">Hodnota oboru názvů se liší od oboru názvů, které používáte v celém svém kódu.</span><span class="sxs-lookup"><span data-stu-id="154ef-153">The namespace value differs from the namespace that you use throughout the scope of your code.</span></span> <span data-ttu-id="154ef-154">Hodnota oboru názvů se používá jako jedinečný identifikátor pro tento kontrakt a měla by mít informaci o verzi.</span><span class="sxs-lookup"><span data-stu-id="154ef-154">The namespace value is used as a unique identifier for this contract, and should have version information.</span></span> <span data-ttu-id="154ef-155">Další informace najdete v článku o [Správa verzí služeb](http://go.microsoft.com/fwlink/?LinkID=180498).</span><span class="sxs-lookup"><span data-stu-id="154ef-155">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="154ef-156">Když explicitně zadáte obor názvů, zabráníte tím přidání výchozí hodnoty oboru názvů do názvu kontraktu.</span><span class="sxs-lookup"><span data-stu-id="154ef-156">Specifying the namespace explicitly prevents the default namespace value from being added to the contract name.</span></span>
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. <span data-ttu-id="154ef-157">V rozhraní `IImageContract` deklarujte metodu pro jednu operaci, kterou kontrakt `IImageContract` vystaví v rozhraní, a aplikujte atribut `OperationContractAttribute` na metodu, kterou chcete vystavit v rámci veřejného kontraktu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="154ef-157">Within the `IImageContract` interface, declare a method for the single operation the `IImageContract` contract exposes in the interface and apply the `OperationContractAttribute` attribute to the method that you want to expose as part of the public Service Bus contract.</span></span>
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. <span data-ttu-id="154ef-158">V atributu **OperationContract** přidejte hodnotu **WebGet**.</span><span class="sxs-lookup"><span data-stu-id="154ef-158">In the **OperationContract** attribute, add the **WebGet** value.</span></span>
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    <span data-ttu-id="154ef-159">To umožňuje Ano předávací službu, kterou chcete směrovat požadavky HTTP GET na `GetImage`a překládat vrácené hodnoty `GetImage` do odpovědi HTTP GETRESPONSE.</span><span class="sxs-lookup"><span data-stu-id="154ef-159">Doing so enables the relay service to route HTTP GET requests to `GetImage`, and to translate the return values of `GetImage` into an HTTP GETRESPONSE reply.</span></span> <span data-ttu-id="154ef-160">Později v tomto kurzu pomocí webového prohlížeče získáte přístup k této metodě a zobrazíte v prohlížeči obrázek.</span><span class="sxs-lookup"><span data-stu-id="154ef-160">Later in the tutorial, you will use a web browser to access this method, and to display the image in the browser.</span></span>
11. <span data-ttu-id="154ef-161">Přímo po definici `IImageContract` deklarujte kanál, který zdědí vlastnosti z rozhraní `IImageContract` i `IClientChannel`.</span><span class="sxs-lookup"><span data-stu-id="154ef-161">Directly after the `IImageContract` definition, declare a channel that inherits from both the `IImageContract` and `IClientChannel` interfaces.</span></span>
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    <span data-ttu-id="154ef-162">Kanál je objekt WCF, kterým si služba a klient navzájem posílají informace.</span><span class="sxs-lookup"><span data-stu-id="154ef-162">A channel is the WCF object through which the service and client pass information to each other.</span></span> <span data-ttu-id="154ef-163">Později vytvoříte kanál ve své hostitelské aplikaci.</span><span class="sxs-lookup"><span data-stu-id="154ef-163">Later, you will create the channel in your host application.</span></span> <span data-ttu-id="154ef-164">Předávání přes Azure pak pomocí tohoto kanálu předat požadavky HTTP GET z prohlížeče do vaší **GetImage** implementace.</span><span class="sxs-lookup"><span data-stu-id="154ef-164">Azure Relay then uses this channel to pass the HTTP GET requests from the browser to your **GetImage** implementation.</span></span> <span data-ttu-id="154ef-165">Předávací službu také pomocí tohoto kanálu trvat **GetImage** vrátit hodnotu a přeloží ji do HTTP GETRESPONSE pro prohlížeče klienta.</span><span class="sxs-lookup"><span data-stu-id="154ef-165">The relay also uses the channel to take the **GetImage** return value and translate it into an HTTP GETRESPONSE for the client browser.</span></span>
12. <span data-ttu-id="154ef-166">V nabídce **Sestavení** klikněte na **Sestavit řešení** a zkontrolujte přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="154ef-166">From the **Build** menu, click **Build Solution** to confirm the accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="154ef-167">Příklad</span><span class="sxs-lookup"><span data-stu-id="154ef-167">Example</span></span>
<span data-ttu-id="154ef-168">Následující kód ukazuje základní rozhraní, které definuje kontrakt předávání WCF.</span><span class="sxs-lookup"><span data-stu-id="154ef-168">The following code shows a basic interface that defines a WCF Relay contract.</span></span>

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

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a><span data-ttu-id="154ef-169">Krok 3: Implementace kontraktu služby WCF na bázi REST pro použití služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="154ef-169">Step 3: Implement a REST-based WCF service contract to use Service Bus</span></span>
<span data-ttu-id="154ef-170">Vytváření stylu REST WCF předávací služba vyžaduje, abyste nejdřív vytvořili kontrakt, který se definuje pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="154ef-170">Creating a REST-style WCF Relay service requires that you first create the contract, which is defined by using an interface.</span></span> <span data-ttu-id="154ef-171">Dalším krokem je implementace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="154ef-171">The next step is to implement the interface.</span></span> <span data-ttu-id="154ef-172">K tomu patří vytvoření třídy s názvem **ImageService**, která implementuje uživatelsky definované rozhraní **IImageContract**.</span><span class="sxs-lookup"><span data-stu-id="154ef-172">This involves creating a class named **ImageService** that implements the user-defined **IImageContract** interface.</span></span> <span data-ttu-id="154ef-173">Po implementaci kontraktu nakonfigurujete rozhraní pomocí souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="154ef-173">After you implement the contract, you then configure the interface using an App.config file.</span></span> <span data-ttu-id="154ef-174">Konfigurační soubor obsahuje nezbytné informace pro aplikaci, například název služby, název kontraktu a typ protokolu, který se používá ke komunikaci se službou předávání.</span><span class="sxs-lookup"><span data-stu-id="154ef-174">The configuration file contains necessary information for the application, such as the name of the service, the name of the contract, and the type of protocol that is used to communicate with the relay service.</span></span> <span data-ttu-id="154ef-175">Kód použitý k těmto úlohám najdete v příkladu za postupem.</span><span class="sxs-lookup"><span data-stu-id="154ef-175">The code used for these tasks is provided in the example following the procedure.</span></span>

<span data-ttu-id="154ef-176">Stejně jako u v předchozích krocích je jen malý rozdíl mezi implementací kontraktu ve stylu REST a kontraktu WCF předávání.</span><span class="sxs-lookup"><span data-stu-id="154ef-176">As with the previous steps, there is very little difference between implementing a REST-style contract and a WCF Relay contract.</span></span>

### <a name="to-implement-a-rest-style-service-bus-contract"></a><span data-ttu-id="154ef-177">Implementace kontraktu Service Bus ve stylu REST</span><span class="sxs-lookup"><span data-stu-id="154ef-177">To implement a REST-style Service Bus contract</span></span>
1. <span data-ttu-id="154ef-178">Vytvořte novou třídu s názvem **ImageService** přímo po definování rozhraní **IImageContract**.</span><span class="sxs-lookup"><span data-stu-id="154ef-178">Create a new class named **ImageService** directly after the definition of the **IImageContract** interface.</span></span> <span data-ttu-id="154ef-179">Třída **ImageService** implementuje rozhraní **IImageContract**.</span><span class="sxs-lookup"><span data-stu-id="154ef-179">The **ImageService** class implements the **IImageContract** interface.</span></span>
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    <span data-ttu-id="154ef-180">Podobně jako u implementace jiných rozhraní můžete definici implementovat v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="154ef-180">Similar to other interface implementations, you can implement the definition in a different file.</span></span> <span data-ttu-id="154ef-181">V tomto kurzu se ale implementace objeví ve stejném souboru jako definice rozhraní a metoda `Main()`.</span><span class="sxs-lookup"><span data-stu-id="154ef-181">However, for this tutorial, the implementation appears in the same file as the interface definition and `Main()` method.</span></span>
2. <span data-ttu-id="154ef-182">Aplikujte atribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) na třídu **IImageService** na znamení, že třída je implementace kontraktu WCF.</span><span class="sxs-lookup"><span data-stu-id="154ef-182">Apply the [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute to the **IImageService** class to indicate that the class is an implementation of a WCF contract.</span></span>
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    <span data-ttu-id="154ef-183">Jak jsme už zmínili, tento obor názvů není obvyklý obor názvů.</span><span class="sxs-lookup"><span data-stu-id="154ef-183">As mentioned previously, this namespace is not a traditional namespace.</span></span> <span data-ttu-id="154ef-184">Místo toho je součástí architektury WCF, která identifikuje kontrakt.</span><span class="sxs-lookup"><span data-stu-id="154ef-184">Instead, it is part of the WCF architecture that identifies the contract.</span></span> <span data-ttu-id="154ef-185">Další informace najdete v tématu [Názvy datových kontraktů](https://msdn.microsoft.com/library/ms731045.aspx) v dokumentaci WCF.</span><span class="sxs-lookup"><span data-stu-id="154ef-185">For more information, see the [Data Contract Names](https://msdn.microsoft.com/library/ms731045.aspx) topic in the WCF documentation.</span></span>
3. <span data-ttu-id="154ef-186">Přidejte do svého projekt obrázek ve formátu .jpg.</span><span class="sxs-lookup"><span data-stu-id="154ef-186">Add a .jpg image to your project.</span></span>  
   
    <span data-ttu-id="154ef-187">To je obrázek, který služba zobrazí ve webovém prohlížeči příjemce.</span><span class="sxs-lookup"><span data-stu-id="154ef-187">This is a picture that the service displays in the receiving browser.</span></span> <span data-ttu-id="154ef-188">Klikněte pravým tlačítkem na projekt a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="154ef-188">Right-click your project, then click **Add**.</span></span> <span data-ttu-id="154ef-189">Pak klikněte na **Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="154ef-189">Then click **Existing Item**.</span></span> <span data-ttu-id="154ef-190">V dialogovém okně **Přidat existující položku** vyberte vhodný soubor .jpg a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="154ef-190">Use the **Add Existing Item** dialog box to browse to an appropriate .jpg, and then click **Add**.</span></span>
   
    <span data-ttu-id="154ef-191">Když přidáváte soubor, zkontrolujte, že je v rozbalovacím seznamu vedle pole**Název souboru:** vybraná možnost **Všechny soubory**.</span><span class="sxs-lookup"><span data-stu-id="154ef-191">When adding the file, make sure that **All Files** is selected in the drop-down list next to the **File name:** field.</span></span> <span data-ttu-id="154ef-192">Zbytek tohoto kurzu předpokládá, že je název obrázku „image.jpg“.</span><span class="sxs-lookup"><span data-stu-id="154ef-192">The rest of this tutorial assumes that the name of the image is "image.jpg".</span></span> <span data-ttu-id="154ef-193">Pokud máte jiný soubor, budete ho muset přejmenovat nebo odpovídajícím způsobem upravit svůj kód.</span><span class="sxs-lookup"><span data-stu-id="154ef-193">If you have a different file, you will have to rename the image, or change your code to compensate.</span></span>
4. <span data-ttu-id="154ef-194">Pokud chcete zkontrolovat, že běžící služba dokáže najít soubor obrázku, v **Průzkumníku řešení** klikněte pravým tlačítkem na soubor obrázku, pak klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="154ef-194">To make sure that the running service can find the image file, in **Solution Explorer** right-click the image file, then click **Properties**.</span></span> <span data-ttu-id="154ef-195">V podokně **Vlastnosti** nastavte **Kopírovat do výstupního adresáře** na **Kopírovat, pokud je novější**.</span><span class="sxs-lookup"><span data-stu-id="154ef-195">In the **Properties** pane, set **Copy to Output Directory** to **Copy if newer**.</span></span>
5. <span data-ttu-id="154ef-196">Přidejte do projektu odkaz na sestavení **System.Drawing.dll** a taky přidejte následující přidružené příkazy `using`.</span><span class="sxs-lookup"><span data-stu-id="154ef-196">Add a reference to the **System.Drawing.dll** assembly to the project, and also add the following associated `using` statements.</span></span>  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. <span data-ttu-id="154ef-197">Ve třídě **ImageService** přidejte následující konstruktor, který načte bitovou mapu a odešle ji do klientského prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="154ef-197">In the **ImageService** class, add the following constructor that loads the bitmap and prepares to send it to the client browser.</span></span>
   
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
7. <span data-ttu-id="154ef-198">Přímo za předchozí kód přidejte následující metodu **GetImage** ve třídě **ImageService**, aby se vrátila zpráva HTTP,která bude obsahovat obrázek.</span><span class="sxs-lookup"><span data-stu-id="154ef-198">Directly after the previous code, add the following **GetImage** method in the **ImageService** class to return an HTTP message that contains the image.</span></span>
   
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
   
    <span data-ttu-id="154ef-199">Tato implementace používá **MemoryStream** k získání obrázku a jeho přípravu pro streamování do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="154ef-199">This implementation uses **MemoryStream** to retrieve the image and prepare it for streaming to the browser.</span></span> <span data-ttu-id="154ef-200">Spustí pozici přenosu na nule, deklaruje obsah přenosu jako jpeg a přenese informaci.</span><span class="sxs-lookup"><span data-stu-id="154ef-200">It starts the stream position at zero, declares the stream content as a jpeg, and streams the information.</span></span>
8. <span data-ttu-id="154ef-201">V nabídce **Sestavení** klikněte na **Sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="154ef-201">From the **Build** menu, click **Build Solution**.</span></span>

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a><span data-ttu-id="154ef-202">Definování konfigurace pro spuštění webové služby v Service Bus</span><span class="sxs-lookup"><span data-stu-id="154ef-202">To define the configuration for running the web service on Service Bus</span></span>
1. <span data-ttu-id="154ef-203">V **Průzkumníku řešení** poklikejte na **App.config** a otevře se v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="154ef-203">In **Solution Explorer**, double-click **App.config** to open it in the Visual Studio editor.</span></span>
   
    <span data-ttu-id="154ef-204">**App.config** soubor obsahuje název služby, koncový bod (tj. umístění předávání přes Azure vystaví pro klienty a hostiteli pro komunikaci mezi sebou) a vazbu (typ protokolu používaný pro komunikaci).</span><span class="sxs-lookup"><span data-stu-id="154ef-204">The **App.config** file includes the service name, endpoint (that is, the location Azure Relay exposes for clients and hosts to communicate with each other), and binding (the type of protocol that is used to communicate).</span></span> <span data-ttu-id="154ef-205">Hlavní rozdíl je, že nakonfigurovaný koncový bod služby odkazuje na [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) vazby.</span><span class="sxs-lookup"><span data-stu-id="154ef-205">The main difference here is that the configured service endpoint refers to a [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding.</span></span>
2. <span data-ttu-id="154ef-206">XML element `<system.serviceModel>` je element WCF, který definuje jednu nebo víc služeb.</span><span class="sxs-lookup"><span data-stu-id="154ef-206">The `<system.serviceModel>` XML element is a WCF element that defines one or more services.</span></span> <span data-ttu-id="154ef-207">Tady se používá k definování názvu služby a koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="154ef-207">Here, it is used to define the service name and endpoint.</span></span> <span data-ttu-id="154ef-208">Na konci elementu `<system.serviceModel>` (ale pořád ještě uvnitř `<system.serviceModel>`) přidejte element `<bindings>`, který má následující obsah.</span><span class="sxs-lookup"><span data-stu-id="154ef-208">At the bottom of the `<system.serviceModel>` element (but still within `<system.serviceModel>`), add a `<bindings>` element that has the following content.</span></span> <span data-ttu-id="154ef-209">To definuje vazbu použitou v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="154ef-209">This defines the bindings used in the application.</span></span> <span data-ttu-id="154ef-210">Můžete definovat víc vazeb, ale v tomto kurzu se definuje jen jedna.</span><span class="sxs-lookup"><span data-stu-id="154ef-210">You can define multiple bindings, but for this tutorial you are defining only one.</span></span>
   
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
   
    <span data-ttu-id="154ef-211">Předchozí kód definuje WCF předávání [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) s **relayClientAuthenticationType** nastavena na **žádné**.</span><span class="sxs-lookup"><span data-stu-id="154ef-211">The previous code defines a WCF Relay [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding with **relayClientAuthenticationType** set to **None**.</span></span> <span data-ttu-id="154ef-212">Toto nastavení naznačuje, že koncový bod, který používá tuto vazbu, nepotřebuje pověření klienta.</span><span class="sxs-lookup"><span data-stu-id="154ef-212">This setting indicates that an endpoint using this binding does not require a client credential.</span></span>
3. <span data-ttu-id="154ef-213">Za element `<bindings>` přidejte element `<services>`.</span><span class="sxs-lookup"><span data-stu-id="154ef-213">After the `<bindings>` element, add a `<services>` element.</span></span> <span data-ttu-id="154ef-214">Podobně jako u vazeb můžete v jednom konfiguračním souboru definovat několik služeb.</span><span class="sxs-lookup"><span data-stu-id="154ef-214">Similar to the bindings, you can define multiple services in a single configuration file.</span></span> <span data-ttu-id="154ef-215">V tomto kurzu ale definujete jen jednu.</span><span class="sxs-lookup"><span data-stu-id="154ef-215">However, for this tutorial, you define only one.</span></span>
   
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
   
    <span data-ttu-id="154ef-216">Tento krok konfiguruje službu, která použije předtím nastavenou výchozí vazbu **webHttpRelayBinding**.</span><span class="sxs-lookup"><span data-stu-id="154ef-216">This step configures a service that uses the previously defined default **webHttpRelayBinding**.</span></span> <span data-ttu-id="154ef-217">Taky používá výchozí ho poskytovatele **sbTokenProvider**, který se definuje v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="154ef-217">It also uses the default **sbTokenProvider**, which is defined in the next step.</span></span>
4. <span data-ttu-id="154ef-218">Po `<services>` elementu, vytvoření `<behaviors>` element s následujícím obsahem, který nahradí "SAS_KEY" s *sdíleného přístupového podpisu* klíč (SAS), jste dříve získali z [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="154ef-218">After the `<services>` element, create a `<behaviors>` element with the following content, replacing "SAS_KEY" with the *Shared Access Signature* (SAS) key you previously obtained from the [Azure portal][Azure portal].</span></span>
   
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
5. <span data-ttu-id="154ef-219">Ještě v souboru App.config v elementu `<appSettings>` nahraďte celou hodnotu připojovacího řetězce element připojovacím řetězcem, který jste předtím získali z portálu.</span><span class="sxs-lookup"><span data-stu-id="154ef-219">Still in App.config, in the `<appSettings>` element, replace the entire connection string value with the connection string you previously obtained from the portal.</span></span> 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. <span data-ttu-id="154ef-220">V nabídce **Sestavení** klikněte na **Sestavit řešení** a sestavte celé řešení.</span><span class="sxs-lookup"><span data-stu-id="154ef-220">From the **Build** menu, click **Build Solution** to build the entire solution.</span></span>

### <a name="example"></a><span data-ttu-id="154ef-221">Příklad</span><span class="sxs-lookup"><span data-stu-id="154ef-221">Example</span></span>
<span data-ttu-id="154ef-222">Následující kód ukazuje implementaci kontraktu a služby pro službu založenou na REST, která běží ve službě Service Bus pomocí vazby **WebHttpRelayBinding**.</span><span class="sxs-lookup"><span data-stu-id="154ef-222">The following code shows the contract and service implementation for a REST-based service that is running on  Service Bus using the **WebHttpRelayBinding** binding.</span></span>

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

<span data-ttu-id="154ef-223">Následující příklad ukazuje soubor App.config přidružený ke službě.</span><span class="sxs-lookup"><span data-stu-id="154ef-223">The following example shows the App.config file associated with the service.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
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

## <a name="step-4-host-the-rest-based-wcf-service-to-use-azure-relay"></a><span data-ttu-id="154ef-224">Krok 4: Hostování služby WCF na bázi REST pro použití předávání přes Azure</span><span class="sxs-lookup"><span data-stu-id="154ef-224">Step 4: Host the REST-based WCF service to use Azure Relay</span></span>
<span data-ttu-id="154ef-225">Tento krok popisuje, jak spustit webovou službu pomocí konzolové aplikace s WCF předávání.</span><span class="sxs-lookup"><span data-stu-id="154ef-225">This step describes how to run a web service using a console application with WCF Relay.</span></span> <span data-ttu-id="154ef-226">Úplný kód napsaný v tomto kroku najdete v příkladu za postupem.</span><span class="sxs-lookup"><span data-stu-id="154ef-226">A complete listing of the code written in this step is provided in the example following the procedure.</span></span>

### <a name="to-create-a-base-address-for-the-service"></a><span data-ttu-id="154ef-227">Vytvoření bázové adresy pro tuto službu</span><span class="sxs-lookup"><span data-stu-id="154ef-227">To create a base address for the service</span></span>
1. <span data-ttu-id="154ef-228">V `Main()` deklaraci funkce, vytvořte proměnnou pro uložení oboru názvů vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="154ef-228">In the `Main()` function declaration, create a variable to store the namespace of your project.</span></span> <span data-ttu-id="154ef-229">Nezapomeňte nahradit `yourNamespace` s názvem oboru názvů předávání jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="154ef-229">Make sure to replace `yourNamespace` with the name of the Relay namespace you created previously.</span></span>
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    <span data-ttu-id="154ef-230">Service Bus použije název vašeho oboru názvů k vytvoření jedinečné URI.</span><span class="sxs-lookup"><span data-stu-id="154ef-230">Service Bus uses the name of your namespace to create a unique URI.</span></span>
2. <span data-ttu-id="154ef-231">Vytvořte instanci `Uri` pro bázovou adresu služby, která je založená na tomto oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="154ef-231">Create a `Uri` instance for the base address of the service that is based on the namespace.</span></span>
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a><span data-ttu-id="154ef-232">Vytvoření a konfigurace hostitele webové služby</span><span class="sxs-lookup"><span data-stu-id="154ef-232">To create and configure the web service host</span></span>
* <span data-ttu-id="154ef-233">Vytvořte hostitele webové služby pomocí adresy URI, kterou jste předtím vytvořili v této části.</span><span class="sxs-lookup"><span data-stu-id="154ef-233">Create the web service host, using the URI address created earlier in this section.</span></span>
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    <span data-ttu-id="154ef-234">Hostitel služby je objekt WCF, který instancuje hostitelskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="154ef-234">The service host is the WCF object that instantiates the host application.</span></span> <span data-ttu-id="154ef-235">Tento příklad jí předá typ hostitele, kterého chcete vytvořit (**ImageService**) a taky adresu, na které chcete hostitelskou aplikaci vystavit.</span><span class="sxs-lookup"><span data-stu-id="154ef-235">This example passes it the type of host you want to create (an **ImageService**), and also the address at which you want to expose the host application.</span></span>

### <a name="to-run-the-web-service-host"></a><span data-ttu-id="154ef-236">Spuštění hostitele webové služby</span><span class="sxs-lookup"><span data-stu-id="154ef-236">To run the web service host</span></span>
1. <span data-ttu-id="154ef-237">Otevřete službu.</span><span class="sxs-lookup"><span data-stu-id="154ef-237">Open the service.</span></span>
   
    ```csharp
    host.Open();
    ```
    <span data-ttu-id="154ef-238">Služba teď běží.</span><span class="sxs-lookup"><span data-stu-id="154ef-238">The service is now running.</span></span>
2. <span data-ttu-id="154ef-239">Zobrazte zprávu oznamující, že zpráva běží, a jak službu zastavit.</span><span class="sxs-lookup"><span data-stu-id="154ef-239">Display a message indicating that the service is running, and how to stop the service.</span></span>
   
    ```csharp
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="154ef-240">Po dokončení zavřete hostitele služby.</span><span class="sxs-lookup"><span data-stu-id="154ef-240">When finished, close the service host.</span></span>
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a><span data-ttu-id="154ef-241">Příklad</span><span class="sxs-lookup"><span data-stu-id="154ef-241">Example</span></span>
<span data-ttu-id="154ef-242">Následující příklad obsahuje kontrakt a implementaci služby z předchozích kroků tohoto kurzu a hostuje službu v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="154ef-242">The following example includes the service contract and implementation from previous steps in the tutorial and hosts the service in a console application.</span></span> <span data-ttu-id="154ef-243">Zkompilujte následující kód do spustitelného souboru s názvem ImageListener.exe.</span><span class="sxs-lookup"><span data-stu-id="154ef-243">Compile the following code into an executable named ImageListener.exe.</span></span>

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

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a><span data-ttu-id="154ef-244">Zkompilování kódu</span><span class="sxs-lookup"><span data-stu-id="154ef-244">Compiling the code</span></span>
<span data-ttu-id="154ef-245">Po sestavení řešení proveďte následující kroky pro spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="154ef-245">After building the solution, do the following to run the application:</span></span>

1. <span data-ttu-id="154ef-246">Spusťte službu stisknutím klávesy**F5** nebo přejděte k umístění spustitelného souboru (ImageListener\bin\Debug\ImageListener.exe) a spusťte ho.</span><span class="sxs-lookup"><span data-stu-id="154ef-246">Press **F5**, or browse to the executable file location (ImageListener\bin\Debug\ImageListener.exe), to run the service.</span></span> <span data-ttu-id="154ef-247">Nechte aplikaci spuštěnou, protože je potřeba při dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="154ef-247">Keep the app running, as it's required to perform the next step.</span></span>
2. <span data-ttu-id="154ef-248">Zkopírujte a vložte adresu z příkazového řádku do prohlížeče, zobrazí se obrázek.</span><span class="sxs-lookup"><span data-stu-id="154ef-248">Copy and paste the address from the command prompt into a browser to see the image.</span></span>
3. <span data-ttu-id="154ef-249">Když skončíte, v okně příkazovém řádku stiskněte **Enter** a aplikace se zavře.</span><span class="sxs-lookup"><span data-stu-id="154ef-249">When you are finished, press **Enter** in the command prompt window to close the app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="154ef-250">Další kroky</span><span class="sxs-lookup"><span data-stu-id="154ef-250">Next steps</span></span>
<span data-ttu-id="154ef-251">Teď, když jste sestavili aplikaci, která používá předávací službu Service Bus, najdete další informace o předávání přes Azure v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="154ef-251">Now that you've built an application that uses the Service Bus relay service, see the following articles to learn more about Azure Relay:</span></span>

* [<span data-ttu-id="154ef-252">Přehled architektury služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="154ef-252">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [<span data-ttu-id="154ef-253">Přehled služby Azure Relay</span><span class="sxs-lookup"><span data-stu-id="154ef-253">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="154ef-254">Jak používat předávání služby WCF s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="154ef-254">How to use the WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
