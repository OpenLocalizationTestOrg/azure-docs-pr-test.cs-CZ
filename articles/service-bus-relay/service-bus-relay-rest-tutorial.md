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
# <a name="azure-wcf-relay-rest-tutorial"></a><span data-ttu-id="5f4c5-103">Kurz pro Azure předávání přes REST WCF</span><span class="sxs-lookup"><span data-stu-id="5f4c5-103">Azure WCF Relay REST tutorial</span></span>

<span data-ttu-id="5f4c5-104">Tento kurz popisuje, jak toobuild jednoduché předávání přes Azure hostování aplikace, která vystavuje rozhraní založené na REST.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-104">This tutorial describes how toobuild a simple Azure Relay host application that exposes a REST-based interface.</span></span> <span data-ttu-id="5f4c5-105">Umožňuje REST webovému klientovi, například webový prohlížeč, hello tooaccess požadavky API pro Service Bus pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-105">REST enables a web client, such as a web browser, tooaccess hello Service Bus APIs through HTTP requests.</span></span>

<span data-ttu-id="5f4c5-106">kurz Hello používá hello Windows Communication Foundation (WCF) REST programovací model tooconstruct služby v Service Bus.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-106">hello tutorial uses hello Windows Communication Foundation (WCF) REST programming model tooconstruct a REST service on Service Bus.</span></span> <span data-ttu-id="5f4c5-107">Další informace najdete v tématu [programovací Model REST WCF](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) a [návrh a implementace služeb](/dotnet/framework/wcf/designing-and-implementing-services) v hello dokumentaci WCF.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-107">For more information, see [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) and [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) in hello WCF documentation.</span></span>

## <a name="step-1-create-a-namespace"></a><span data-ttu-id="5f4c5-108">Krok 1: Vytvoření oboru názvů</span><span class="sxs-lookup"><span data-stu-id="5f4c5-108">Step 1: Create a namespace</span></span>

<span data-ttu-id="5f4c5-109">pomocí toobegin hello funkce předávání v Azure, musíte nejdřív vytvořit obor názvů služby.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-109">toobegin using hello relay features in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="5f4c5-110">Obor názvů poskytuje kontejner oboru pro adresování prostředků Azure v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-110">A namespace provides a scoping container for addressing Azure resources within your application.</span></span> <span data-ttu-id="5f4c5-111">Postupujte podle hello [pokynů tady](relay-create-namespace-portal.md) toocreate předávání názvů.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-111">Follow hello [instructions here](relay-create-namespace-portal.md) toocreate a Relay namespace.</span></span>

## <a name="step-2-define-a-rest-based-wcf-service-contract-toouse-with-azure-relay"></a><span data-ttu-id="5f4c5-112">Krok 2: Definování WCF na bázi REST služby kontrakt toouse s předávání přes Azure</span><span class="sxs-lookup"><span data-stu-id="5f4c5-112">Step 2: Define a REST-based WCF service contract toouse with Azure Relay</span></span>

<span data-ttu-id="5f4c5-113">Při vytváření služby WCF stylu REST, je nutné definovat kontrakt hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-113">When you create a WCF REST-style service, you must define hello contract.</span></span> <span data-ttu-id="5f4c5-114">Hello kontrakt Určuje, jakým operacím hello hostitel podporuje.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-114">hello contract specifies what operations hello host supports.</span></span> <span data-ttu-id="5f4c5-115">Operaci služby se můžeme představit jako metodu webové služby.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-115">A service operation can be thought of as a web service method.</span></span> <span data-ttu-id="5f4c5-116">Kontrakty se vytvoří definováním základního rozhraní C++, C# nebo Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-116">Contracts are created by defining a C++, C#, or Visual Basic interface.</span></span> <span data-ttu-id="5f4c5-117">Každá metoda v rozhraní hello odpovídá tooa konkrétní operaci služby.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-117">Each method in hello interface corresponds tooa specific service operation.</span></span> <span data-ttu-id="5f4c5-118">Hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) atributu musí být použité tooeach rozhraní a hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) atribut musí být použité tooeach operaci.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-118">hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribute must be applied tooeach interface, and hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribute must be applied tooeach operation.</span></span> <span data-ttu-id="5f4c5-119">Pokud metoda v rozhraní, které má hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nemá hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), taková metoda se nevystaví.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-119">If a method in an interface that has hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) does not have hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), that method is not exposed.</span></span> <span data-ttu-id="5f4c5-120">Hello kód použitý pro tyto úlohy se zobrazí v příkladu hello hello postupem.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-120">hello code used for these tasks is shown in hello example following hello procedure.</span></span>

<span data-ttu-id="5f4c5-121">Hello základní rozdíl mezi kontraktu WCF a kontraktu ve stylu REST je přidání hello vlastnost toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f4c5-121">hello primary difference between a WCF contract and a REST-style contract is hello addition of a property toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx).</span></span> <span data-ttu-id="5f4c5-122">Tato vlastnost umožňuje toomap metoda v rozhraní tooa metodu na hello druhé straně rozhraní hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-122">This property enables you toomap a method in your interface tooa method on hello other side of hello interface.</span></span> <span data-ttu-id="5f4c5-123">V tomto případě použijeme [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink tooHTTP metoda GET.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-123">In this case, we will use [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink a method tooHTTP GET.</span></span> <span data-ttu-id="5f4c5-124">To umožňuje Service Bus tooaccurately získat a interpretovat příkazy odeslané toohello rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-124">This allows Service Bus tooaccurately retrieve and interpret commands sent toohello interface.</span></span>

### <a name="toocreate-a-contract-with-an-interface"></a><span data-ttu-id="5f4c5-125">toocreate kontraktu s rozhraním</span><span class="sxs-lookup"><span data-stu-id="5f4c5-125">toocreate a contract with an interface</span></span>

1. <span data-ttu-id="5f4c5-126">Otevřete Visual Studio jako správce: programu hello klikněte pravým tlačítkem v hello **spustit** nabídce a pak klikněte na tlačítko **spustit jako správce**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-126">Open Visual Studio as an administrator: right-click hello program in hello **Start** menu, and then click **Run as administrator**.</span></span>
2. <span data-ttu-id="5f4c5-127">Vytvořte nový projekt konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-127">Create a new console application project.</span></span> <span data-ttu-id="5f4c5-128">Klikněte na tlačítko hello **soubor** nabídku a vyberte **nový**, pak vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-128">Click hello **File** menu and select **New**, then select **Project**.</span></span> <span data-ttu-id="5f4c5-129">V hello **nový projekt** dialogové okno, klikněte na tlačítko **Visual C#**, vyberte hello **konzolové aplikace** šablony a pojmenujte ji **ImageListener**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-129">In hello **New Project** dialog box, click **Visual C#**, select hello **Console Application** template, and name it **ImageListener**.</span></span> <span data-ttu-id="5f4c5-130">Použít výchozí hello **umístění**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-130">Use hello default **Location**.</span></span> <span data-ttu-id="5f4c5-131">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-131">Click **OK** toocreate hello project.</span></span>
3. <span data-ttu-id="5f4c5-132">Visual Studio pro projekt C# vytvoří soubor `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-132">For a C# project, Visual Studio creates a `Program.cs` file.</span></span> <span data-ttu-id="5f4c5-133">Tato třída obsahuje prázdnou `Main()` metoda, vyžaduje se pro toobuild projektu na konzole aplikace správně.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-133">This class contains an empty `Main()` method, required for a console application project toobuild correctly.</span></span>
4. <span data-ttu-id="5f4c5-134">Přidání odkazů tooService sběrnice a **System.ServiceModel.dll** toohello projektu instalace balíčku Service Bus NuGet hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-134">Add references tooService Bus and **System.ServiceModel.dll** toohello project by installing hello Service Bus NuGet package.</span></span> <span data-ttu-id="5f4c5-135">Tento balíček automaticky přidá Reference knihovny Service Bus toohello, jakož i hello WCF **System.ServiceModel**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-135">This package automatically adds references toohello Service Bus libraries, as well as hello WCF **System.ServiceModel**.</span></span> <span data-ttu-id="5f4c5-136">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ImageListener** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-136">In Solution Explorer, right-click hello **ImageListener** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="5f4c5-137">Klikněte na tlačítko hello **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-137">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="5f4c5-138">Klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-138">Click **Install**, and accept hello terms of use.</span></span>
5. <span data-ttu-id="5f4c5-139">Musíte explicitně přidat odkaz na příliš**System.ServiceModel.Web.dll** toohello projektu:</span><span class="sxs-lookup"><span data-stu-id="5f4c5-139">You must explicitly add a reference too**System.ServiceModel.Web.dll** toohello project:</span></span>
   
    <span data-ttu-id="5f4c5-140">a.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-140">a.</span></span> <span data-ttu-id="5f4c5-141">V Průzkumníku řešení klikněte pravým tlačítkem na hello **odkazy** ve složce složky hello projektu a pak klikněte na tlačítko **přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-141">In Solution Explorer, right-click hello **References** folder under hello project folder and then click **Add Reference**.</span></span>
   
    <span data-ttu-id="5f4c5-142">b.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-142">b.</span></span> <span data-ttu-id="5f4c5-143">V hello **přidat odkaz na** dialogovém okně klikněte na hello **Framework** karty na levé straně hello a v hello **vyhledávání** zadejte **System.ServiceModel.Web** .</span><span class="sxs-lookup"><span data-stu-id="5f4c5-143">In hello **Add Reference** dialog box, click hello **Framework** tab on hello left-hand side and in hello **Search** box, type **System.ServiceModel.Web**.</span></span> <span data-ttu-id="5f4c5-144">Vyberte hello **System.ServiceModel.Web** zaškrtněte políčko a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-144">Select hello **System.ServiceModel.Web** check box, then click **OK**.</span></span>
6. <span data-ttu-id="5f4c5-145">Přidejte následující hello `using` příkazy hello horní části souboru Program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-145">Add hello following `using` statements at hello top of hello Program.cs file.</span></span>
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    <span data-ttu-id="5f4c5-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) je hello obor názvů, který umožňuje programový přístup toobasic funkcím WCF.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-146">[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is hello namespace that enables programmatic access toobasic features of WCF.</span></span> <span data-ttu-id="5f4c5-147">Předávání WCF používá řadu hello objekty a atributy kontrakty toodefine služeb WCF.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-147">WCF Relay uses many of hello objects and attributes of WCF toodefine service contracts.</span></span> <span data-ttu-id="5f4c5-148">Tento obor názvů budete používat ve většině aplikací předávání.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-148">You will use this namespace in most of your relay applications.</span></span> <span data-ttu-id="5f4c5-149">Podobně [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) pomáhá definovat kanál hello, což je hello objekt, přes který komunikujete s webovým prohlížečem, předávání přes Azure a hello klienta.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-149">Similarly, [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) helps define hello channel, which is hello object through which you communicate with Azure Relay and hello client web browser.</span></span> <span data-ttu-id="5f4c5-150">Nakonec [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) obsahuje hello typy, které umožňují toocreate webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-150">Finally, [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) contains hello types that enable you toocreate web-based applications.</span></span>
7. <span data-ttu-id="5f4c5-151">Přejmenujte hello `ImageListener` obor názvů příliš**Microsoft.ServiceBus.Samples**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-151">Rename hello `ImageListener` namespace too**Microsoft.ServiceBus.Samples**.</span></span>
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. <span data-ttu-id="5f4c5-152">Přímo po otevření složené závorky deklarace oboru názvů hello hello, definujte nové rozhraní s názvem **IImageContract** a použít hello **ServiceContractAttribute** atribut toohello rozhraní s Hodnota `http://samples.microsoft.com/ServiceModel/Relay/`.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-152">Directly after hello opening curly brace of hello namespace declaration, define a new interface named **IImageContract** and apply hello **ServiceContractAttribute** attribute toohello interface with a value of `http://samples.microsoft.com/ServiceModel/Relay/`.</span></span> <span data-ttu-id="5f4c5-153">Hodnota oboru názvů Hello se liší od hello obor názvů, který používáte v rámci oboru hello kódu.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-153">hello namespace value differs from hello namespace that you use throughout hello scope of your code.</span></span> <span data-ttu-id="5f4c5-154">Hodnota oboru názvů Hello se používá jako jedinečný identifikátor pro tento kontrakt a musí mít informace o verzi.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-154">hello namespace value is used as a unique identifier for this contract, and should have version information.</span></span> <span data-ttu-id="5f4c5-155">Další informace najdete v článku o [Správa verzí služeb](http://go.microsoft.com/fwlink/?LinkID=180498).</span><span class="sxs-lookup"><span data-stu-id="5f4c5-155">For more information, see [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498).</span></span> <span data-ttu-id="5f4c5-156">Zadání oboru názvů hello explicitně brání přidání názvu kontraktu toohello hello výchozí hodnoty oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-156">Specifying hello namespace explicitly prevents hello default namespace value from being added toohello contract name.</span></span>
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. <span data-ttu-id="5f4c5-157">V rámci hello `IImageContract` rozhraní, deklarujte metodu pro jednu operaci hello hello `IImageContract` kontrakt zpřístupňuje v hello rozhraní a použít hello `OperationContractAttribute` atribut toohello metodu chcete tooexpose jako součást hello veřejné Service Bus kontrakt.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-157">Within hello `IImageContract` interface, declare a method for hello single operation hello `IImageContract` contract exposes in hello interface and apply hello `OperationContractAttribute` attribute toohello method that you want tooexpose as part of hello public Service Bus contract.</span></span>
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. <span data-ttu-id="5f4c5-158">V hello **OperationContract** atribut, přidejte hello **WebGet** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-158">In hello **OperationContract** attribute, add hello **WebGet** value.</span></span>
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    <span data-ttu-id="5f4c5-159">V tom umožňuje hello tooroute služby předávání přes požadavky HTTP GET příliš`GetImage`a vrátit hodnoty hello tootranslate `GetImage` do odpovědi HTTP GETRESPONSE.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-159">Doing so enables hello relay service tooroute HTTP GET requests too`GetImage`, and tootranslate hello return values of `GetImage` into an HTTP GETRESPONSE reply.</span></span> <span data-ttu-id="5f4c5-160">Později v kurzu hello použijete webové prohlížeče tooaccess tato metoda a toodisplay hello image v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-160">Later in hello tutorial, you will use a web browser tooaccess this method, and toodisplay hello image in hello browser.</span></span>
11. <span data-ttu-id="5f4c5-161">Přímo po hello `IImageContract` definice deklarujte kanál, který dědí z obou hello `IImageContract` a `IClientChannel` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-161">Directly after hello `IImageContract` definition, declare a channel that inherits from both hello `IImageContract` and `IClientChannel` interfaces.</span></span>
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    <span data-ttu-id="5f4c5-162">Kanál je objekt WCF hello klienta a služby hello průchodu, která tooeach informace o dalších.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-162">A channel is hello WCF object through which hello service and client pass information tooeach other.</span></span> <span data-ttu-id="5f4c5-163">Později vytvoříte kanál hello ve své hostitelské aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-163">Later, you will create hello channel in your host application.</span></span> <span data-ttu-id="5f4c5-164">Předávání přes Azure pak použije tento kanál toopass hello požadavky HTTP GET z prohlížeče tooyour hello **GetImage** implementace.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-164">Azure Relay then uses this channel toopass hello HTTP GET requests from hello browser tooyour **GetImage** implementation.</span></span> <span data-ttu-id="5f4c5-165">předávání Hello používá také hello kanál tootake hello **GetImage** vrátit hodnotu a přeloží ji do HTTP GETRESPONSE pro hello prohlížeče klienta.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-165">hello relay also uses hello channel tootake hello **GetImage** return value and translate it into an HTTP GETRESPONSE for hello client browser.</span></span>
12. <span data-ttu-id="5f4c5-166">Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** tooconfirm hello přesnost své dosavadní práce.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-166">From hello **Build** menu, click **Build Solution** tooconfirm hello accuracy of your work so far.</span></span>

### <a name="example"></a><span data-ttu-id="5f4c5-167">Příklad</span><span class="sxs-lookup"><span data-stu-id="5f4c5-167">Example</span></span>
<span data-ttu-id="5f4c5-168">Hello následující kód ukazuje základní rozhraní, která definuje kontraktu WCF předávání.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-168">hello following code shows a basic interface that defines a WCF Relay contract.</span></span>

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

## <a name="step-3-implement-a-rest-based-wcf-service-contract-toouse-service-bus"></a><span data-ttu-id="5f4c5-169">Krok 3: Implementace toouse kontraktu služby WCF na bázi REST Service Bus</span><span class="sxs-lookup"><span data-stu-id="5f4c5-169">Step 3: Implement a REST-based WCF service contract toouse Service Bus</span></span>
<span data-ttu-id="5f4c5-170">Vytváření stylu REST WCF předávací služba vyžaduje, abyste nejdřív vytvořili kontrakt hello, který se definuje pomocí rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-170">Creating a REST-style WCF Relay service requires that you first create hello contract, which is defined by using an interface.</span></span> <span data-ttu-id="5f4c5-171">dalším krokem Hello je tooimplement hello rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-171">hello next step is tooimplement hello interface.</span></span> <span data-ttu-id="5f4c5-172">To zahrnuje vytvoření třídy s názvem **ImageService** hello definovaný uživatelem, která implementuje **IImageContract** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-172">This involves creating a class named **ImageService** that implements hello user-defined **IImageContract** interface.</span></span> <span data-ttu-id="5f4c5-173">Po implementaci kontraktu hello nakonfigurujete hello rozhraní pomocí souboru App.config.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-173">After you implement hello contract, you then configure hello interface using an App.config file.</span></span> <span data-ttu-id="5f4c5-174">Hello konfigurační soubor obsahuje informace nutné k hello aplikaci, například název hello hello služby, název hello hello kontraktu a typ hello protokol, který je použité toocommunicate službou předávání přes hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-174">hello configuration file contains necessary information for hello application, such as hello name of hello service, hello name of hello contract, and hello type of protocol that is used toocommunicate with hello relay service.</span></span> <span data-ttu-id="5f4c5-175">Hello kód použitý k těmto úlohám najdete v příkladu hello hello postupem.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-175">hello code used for these tasks is provided in hello example following hello procedure.</span></span>

<span data-ttu-id="5f4c5-176">Stejně jako u předchozí kroky hello, je velmi malý rozdíl mezi implementací kontraktu ve stylu REST a kontraktu WCF předávání.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-176">As with hello previous steps, there is very little difference between implementing a REST-style contract and a WCF Relay contract.</span></span>

### <a name="tooimplement-a-rest-style-service-bus-contract"></a><span data-ttu-id="5f4c5-177">kontraktu Service Bus tooimplement stylu REST</span><span class="sxs-lookup"><span data-stu-id="5f4c5-177">tooimplement a REST-style Service Bus contract</span></span>
1. <span data-ttu-id="5f4c5-178">Vytvořte novou třídu s názvem **ImageService** přímo po definici hello hello **IImageContract** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-178">Create a new class named **ImageService** directly after hello definition of hello **IImageContract** interface.</span></span> <span data-ttu-id="5f4c5-179">Hello **ImageService** třída implementuje hello **IImageContract** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-179">hello **ImageService** class implements hello **IImageContract** interface.</span></span>
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    <span data-ttu-id="5f4c5-180">Podobné implementace rozhraní tooother, definice hello můžete implementovat v jiném souboru.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-180">Similar tooother interface implementations, you can implement hello definition in a different file.</span></span> <span data-ttu-id="5f4c5-181">Ale pro účely tohoto kurzu hello implementace objeví ve hello stejný soubor jako definice rozhraní hello a `Main()` metoda.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-181">However, for this tutorial, hello implementation appears in hello same file as hello interface definition and `Main()` method.</span></span>
2. <span data-ttu-id="5f4c5-182">Použít hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) atribut toohello **IImageService** tooindicate třída, která hello třída je implementace kontraktu WCF.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-182">Apply hello [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribute toohello **IImageService** class tooindicate that hello class is an implementation of a WCF contract.</span></span>
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    <span data-ttu-id="5f4c5-183">Jak jsme už zmínili, tento obor názvů není obvyklý obor názvů.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-183">As mentioned previously, this namespace is not a traditional namespace.</span></span> <span data-ttu-id="5f4c5-184">Místo toho je součástí architektury WCF, která identifikuje kontrakt hello hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-184">Instead, it is part of hello WCF architecture that identifies hello contract.</span></span> <span data-ttu-id="5f4c5-185">Další informace najdete v tématu hello [názvy datových kontraktů](https://msdn.microsoft.com/library/ms731045.aspx) téma v hello dokumentaci WCF.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-185">For more information, see hello [Data Contract Names](https://msdn.microsoft.com/library/ms731045.aspx) topic in hello WCF documentation.</span></span>
3. <span data-ttu-id="5f4c5-186">Přidáte projekt tooyour .jpg bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-186">Add a .jpg image tooyour project.</span></span>  
   
    <span data-ttu-id="5f4c5-187">To je obrázek, který hello služba zobrazí ve hello přijetí prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-187">This is a picture that hello service displays in hello receiving browser.</span></span> <span data-ttu-id="5f4c5-188">Klikněte pravým tlačítkem na projekt a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-188">Right-click your project, then click **Add**.</span></span> <span data-ttu-id="5f4c5-189">Pak klikněte na **Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-189">Then click **Existing Item**.</span></span> <span data-ttu-id="5f4c5-190">Použití hello **přidat existující položku** dialogové okno pole toobrowse tooan vhodné .jpg a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-190">Use hello **Add Existing Item** dialog box toobrowse tooan appropriate .jpg, and then click **Add**.</span></span>
   
    <span data-ttu-id="5f4c5-191">Když přidáváte soubor hello, ujistěte se, že **všechny soubory** je vybraný v hello rozevíracího seznamu další toohello **název souboru:** pole.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-191">When adding hello file, make sure that **All Files** is selected in hello drop-down list next toohello **File name:** field.</span></span> <span data-ttu-id="5f4c5-192">Hello zbytek tohoto kurzu se předpokládá, že hello název obrázku hello je "image.jpg".</span><span class="sxs-lookup"><span data-stu-id="5f4c5-192">hello rest of this tutorial assumes that hello name of hello image is "image.jpg".</span></span> <span data-ttu-id="5f4c5-193">Pokud máte jiný soubor, bude toorename hello image obsahují, nebo změňte toocompensate vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-193">If you have a different file, you will have toorename hello image, or change your code toocompensate.</span></span>
4. <span data-ttu-id="5f4c5-194">toomake se, že hello službou můžete najít v hello soubor obrázku, **Průzkumníku řešení** klikněte pravým tlačítkem na soubor bitové kopie hello a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-194">toomake sure that hello running service can find hello image file, in **Solution Explorer** right-click hello image file, then click **Properties**.</span></span> <span data-ttu-id="5f4c5-195">V hello **vlastnosti** podokně nastavit **zkopírujte tooOutput Directory** příliš**kopírovat, pokud je novější**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-195">In hello **Properties** pane, set **Copy tooOutput Directory** too**Copy if newer**.</span></span>
5. <span data-ttu-id="5f4c5-196">Přidat odkaz na toohello **System.Drawing.dll** toohello sestavení projektu a také přidat následující hello přidružené `using` příkazy.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-196">Add a reference toohello **System.Drawing.dll** assembly toohello project, and also add hello following associated `using` statements.</span></span>  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. <span data-ttu-id="5f4c5-197">V hello **ImageService** třídy, přidejte text hello následující konstruktor, zatížení hello rastrového obrázku a připraví toosend ho toohello prohlížeče klienta.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-197">In hello **ImageService** class, add hello following constructor that loads hello bitmap and prepares toosend it toohello client browser.</span></span>
   
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
7. <span data-ttu-id="5f4c5-198">Přímo za předchozí kód hello přidejte následující hello **GetImage** metoda v hello **ImageService** třídy tooreturn HTTP zprávu, která obsahuje bitovou kopii hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-198">Directly after hello previous code, add hello following **GetImage** method in hello **ImageService** class tooreturn an HTTP message that contains hello image.</span></span>
   
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
   
    <span data-ttu-id="5f4c5-199">Tato implementace používá **MemoryStream** tooretrieve hello obrázku a jeho přípravu pro streamování toohello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-199">This implementation uses **MemoryStream** tooretrieve hello image and prepare it for streaming toohello browser.</span></span> <span data-ttu-id="5f4c5-200">Spustí pozici přenosu hello na nule, deklaruje hello obsah přenosu jako jpeg a datové proudy hello informace.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-200">It starts hello stream position at zero, declares hello stream content as a jpeg, and streams hello information.</span></span>
8. <span data-ttu-id="5f4c5-201">Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-201">From hello **Build** menu, click **Build Solution**.</span></span>

### <a name="toodefine-hello-configuration-for-running-hello-web-service-on-service-bus"></a><span data-ttu-id="5f4c5-202">Konfigurace hello toodefine pro spuštění hello webové služby v Service Bus</span><span class="sxs-lookup"><span data-stu-id="5f4c5-202">toodefine hello configuration for running hello web service on Service Bus</span></span>
1. <span data-ttu-id="5f4c5-203">V **Průzkumníku řešení**, dvakrát klikněte na **App.config** tooopen ji v editoru Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-203">In **Solution Explorer**, double-click **App.config** tooopen it in hello Visual Studio editor.</span></span>
   
    <span data-ttu-id="5f4c5-204">Hello **App.config** soubor obsahuje hello název služby, koncový bod (tj. umístění hello předávání přes Azure vystaví pro klienty a hostiteli toocommunicate mezi sebou) a vazbu (typ hello protokolu, který je použité toocommunicate).</span><span class="sxs-lookup"><span data-stu-id="5f4c5-204">hello **App.config** file includes hello service name, endpoint (that is, hello location Azure Relay exposes for clients and hosts toocommunicate with each other), and binding (hello type of protocol that is used toocommunicate).</span></span> <span data-ttu-id="5f4c5-205">Hello hlavní rozdíl je tento koncový bod služby hello nakonfigurované odkazuje tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) vazby.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-205">hello main difference here is that hello configured service endpoint refers tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding.</span></span>
2. <span data-ttu-id="5f4c5-206">Hello `<system.serviceModel>` – element XML je element WCF, který definuje jednu nebo více služeb.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-206">hello `<system.serviceModel>` XML element is a WCF element that defines one or more services.</span></span> <span data-ttu-id="5f4c5-207">Zde je název služby používané toodefine hello a koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-207">Here, it is used toodefine hello service name and endpoint.</span></span> <span data-ttu-id="5f4c5-208">Na konci hello hello `<system.serviceModel>` element (ale stále ještě v `<system.serviceModel>`), přidejte `<bindings>` element, který má hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-208">At hello bottom of hello `<system.serviceModel>` element (but still within `<system.serviceModel>`), add a `<bindings>` element that has hello following content.</span></span> <span data-ttu-id="5f4c5-209">Definuje hello vazbu použitou v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-209">This defines hello bindings used in hello application.</span></span> <span data-ttu-id="5f4c5-210">Můžete definovat víc vazeb, ale v tomto kurzu se definuje jen jedna.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-210">You can define multiple bindings, but for this tutorial you are defining only one.</span></span>
   
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
   
    <span data-ttu-id="5f4c5-211">Hello předchozí kód definuje WCF předávání [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) s **relayClientAuthenticationType** nastavit příliš**žádné**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-211">hello previous code defines a WCF Relay [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding with **relayClientAuthenticationType** set too**None**.</span></span> <span data-ttu-id="5f4c5-212">Toto nastavení naznačuje, že koncový bod, který používá tuto vazbu, nepotřebuje pověření klienta.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-212">This setting indicates that an endpoint using this binding does not require a client credential.</span></span>
3. <span data-ttu-id="5f4c5-213">Po hello `<bindings>` elementu, přidejte `<services>` elementu.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-213">After hello `<bindings>` element, add a `<services>` element.</span></span> <span data-ttu-id="5f4c5-214">Podobné toohello vazby v jednom konfiguračním souboru můžete definovat více služeb.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-214">Similar toohello bindings, you can define multiple services in a single configuration file.</span></span> <span data-ttu-id="5f4c5-215">V tomto kurzu ale definujete jen jednu.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-215">However, for this tutorial, you define only one.</span></span>
   
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
   
    <span data-ttu-id="5f4c5-216">Tento krok konfiguruje službu, která používá výchozí hello předtím definovaný **webHttpRelayBinding**.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-216">This step configures a service that uses hello previously defined default **webHttpRelayBinding**.</span></span> <span data-ttu-id="5f4c5-217">Taky používá výchozí hello **sbTokenProvider**, která je definována v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-217">It also uses hello default **sbTokenProvider**, which is defined in hello next step.</span></span>
4. <span data-ttu-id="5f4c5-218">Po hello `<services>` elementu, vytvoření `<behaviors>` element s hello následující obsah, nahraďte "SAS_KEY" hello *sdíleného přístupového podpisu* klíč (SAS), které jste předtím získali z hello [portálu Azure ][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="5f4c5-218">After hello `<services>` element, create a `<behaviors>` element with hello following content, replacing "SAS_KEY" with hello *Shared Access Signature* (SAS) key you previously obtained from hello [Azure portal][Azure portal].</span></span>
   
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
5. <span data-ttu-id="5f4c5-219">Stále v souboru App.config v hello `<appSettings>` elementu, nahraďte hello celé připojení řetězcovou hodnotu s jste předtím získali z portálu hello hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-219">Still in App.config, in hello `<appSettings>` element, replace hello entire connection string value with hello connection string you previously obtained from hello portal.</span></span> 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. <span data-ttu-id="5f4c5-220">Z hello **sestavení** nabídky, klikněte na tlačítko **sestavit řešení** toobuild hello celé řešení.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-220">From hello **Build** menu, click **Build Solution** toobuild hello entire solution.</span></span>

### <a name="example"></a><span data-ttu-id="5f4c5-221">Příklad</span><span class="sxs-lookup"><span data-stu-id="5f4c5-221">Example</span></span>
<span data-ttu-id="5f4c5-222">Hello následující kód ukazuje implementaci kontraktu a služby hello pro nějakou službu založenou na REST, která běží na Service Bus pomocí hello **WebHttpRelayBinding** vazby.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-222">hello following code shows hello contract and service implementation for a REST-based service that is running on  Service Bus using hello **WebHttpRelayBinding** binding.</span></span>

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

<span data-ttu-id="5f4c5-223">Hello následující příklad ukazuje soubor App.config hello související se službou hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-223">hello following example shows hello App.config file associated with hello service.</span></span>

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

## <a name="step-4-host-hello-rest-based-wcf-service-toouse-azure-relay"></a><span data-ttu-id="5f4c5-224">Krok 4: Hostování toouse služby WCF na bázi REST hello předávání přes Azure</span><span class="sxs-lookup"><span data-stu-id="5f4c5-224">Step 4: Host hello REST-based WCF service toouse Azure Relay</span></span>
<span data-ttu-id="5f4c5-225">Tento krok popisuje, jak toorun webové služby WCF předávání pomocí konzolové aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-225">This step describes how toorun a web service using a console application with WCF Relay.</span></span> <span data-ttu-id="5f4c5-226">Úplný seznam všech hello kód napsaný v tomto kroku najdete v příkladu hello hello postupem.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-226">A complete listing of hello code written in this step is provided in hello example following hello procedure.</span></span>

### <a name="toocreate-a-base-address-for-hello-service"></a><span data-ttu-id="5f4c5-227">toocreate základní adresa služby hello</span><span class="sxs-lookup"><span data-stu-id="5f4c5-227">toocreate a base address for hello service</span></span>
1. <span data-ttu-id="5f4c5-228">V hello `Main()` deklaraci funkce, vytvořit obor názvů hello proměnné toostore projektu.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-228">In hello `Main()` function declaration, create a variable toostore hello namespace of your project.</span></span> <span data-ttu-id="5f4c5-229">Ujistěte se, že tooreplace `yourNamespace` s hello název oboru názvů předávání hello, které jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-229">Make sure tooreplace `yourNamespace` with hello name of hello Relay namespace you created previously.</span></span>
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    <span data-ttu-id="5f4c5-230">Service Bus používá hello název vašeho oboru názvů toocreate jedinečný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-230">Service Bus uses hello name of your namespace toocreate a unique URI.</span></span>
2. <span data-ttu-id="5f4c5-231">Vytvoření `Uri` instance pro základní adresu hello hello služby, který je založen na obor názvů hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-231">Create a `Uri` instance for hello base address of hello service that is based on hello namespace.</span></span>
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="toocreate-and-configure-hello-web-service-host"></a><span data-ttu-id="5f4c5-232">toocreate a konfigurace hostitele webové služby hello</span><span class="sxs-lookup"><span data-stu-id="5f4c5-232">toocreate and configure hello web service host</span></span>
* <span data-ttu-id="5f4c5-233">Vytvořte hello hostitele webové služby pomocí adresy URI hello vytvořili dříve v této části.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-233">Create hello web service host, using hello URI address created earlier in this section.</span></span>
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    <span data-ttu-id="5f4c5-234">Hostitel služby Hello je objekt WCF hello, který instancuje hostitelskou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-234">hello service host is hello WCF object that instantiates hello host application.</span></span> <span data-ttu-id="5f4c5-235">Tento příklad jí předá typ hello hostitele chcete toocreate ( **ImageService**), a také hello adresu, na které chcete tooexpose hello hostitelskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-235">This example passes it hello type of host you want toocreate (an **ImageService**), and also hello address at which you want tooexpose hello host application.</span></span>

### <a name="toorun-hello-web-service-host"></a><span data-ttu-id="5f4c5-236">hostitele toorun hello webové služby</span><span class="sxs-lookup"><span data-stu-id="5f4c5-236">toorun hello web service host</span></span>
1. <span data-ttu-id="5f4c5-237">Otevřete službu hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-237">Open hello service.</span></span>
   
    ```csharp
    host.Open();
    ```
    <span data-ttu-id="5f4c5-238">Hello služba je nyní spuštěna.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-238">hello service is now running.</span></span>
2. <span data-ttu-id="5f4c5-239">Zobrazí se zpráva s oznámením, že je spuštěna služba hello a jak toostop hello služby.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-239">Display a message indicating that hello service is running, and how toostop hello service.</span></span>
   
    ```csharp
    Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. <span data-ttu-id="5f4c5-240">Po dokončení zavřete hostitele služby hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-240">When finished, close hello service host.</span></span>
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a><span data-ttu-id="5f4c5-241">Příklad</span><span class="sxs-lookup"><span data-stu-id="5f4c5-241">Example</span></span>
<span data-ttu-id="5f4c5-242">Následující ukázka Hello zahrnuje hello kontrakt a implementaci služby z předchozích kroků v hello kurzu a hostitele služby hello v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-242">hello following example includes hello service contract and implementation from previous steps in hello tutorial and hosts hello service in a console application.</span></span> <span data-ttu-id="5f4c5-243">Zkompilujte následující kód do spustitelného souboru s názvem ImageListener.exe hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-243">Compile hello following code into an executable named ImageListener.exe.</span></span>

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

### <a name="compiling-hello-code"></a><span data-ttu-id="5f4c5-244">Kompilování kódu hello</span><span class="sxs-lookup"><span data-stu-id="5f4c5-244">Compiling hello code</span></span>
<span data-ttu-id="5f4c5-245">Po sestavení řešení hello, hello následující toorun hello aplikace:</span><span class="sxs-lookup"><span data-stu-id="5f4c5-245">After building hello solution, do hello following toorun hello application:</span></span>

1. <span data-ttu-id="5f4c5-246">Stiskněte klávesu **F5**, nebo vyhledejte umístění toohello spustitelného souboru (ImageListener\bin\Debug\ImageListener.exe), služba toorun hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-246">Press **F5**, or browse toohello executable file location (ImageListener\bin\Debug\ImageListener.exe), toorun hello service.</span></span> <span data-ttu-id="5f4c5-247">Zachovat hello aplikace spuštěna, protože je to požadováno, dalším krokem tooperform hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-247">Keep hello app running, as it's required tooperform hello next step.</span></span>
2. <span data-ttu-id="5f4c5-248">Zkopírujte a vložte adresu hello z příkazového řádku hello Image hello toosee prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-248">Copy and paste hello address from hello command prompt into a browser toosee hello image.</span></span>
3. <span data-ttu-id="5f4c5-249">Když skončíte, stiskněte **Enter** v aplikace hello tooclose v okně příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="5f4c5-249">When you are finished, press **Enter** in hello command prompt window tooclose hello app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f4c5-250">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f4c5-250">Next steps</span></span>
<span data-ttu-id="5f4c5-251">Teď, když jste sestavili aplikaci, která používá předávací službu Service Bus hello, najdete v části hello následující další články toolearn o předávání přes Azure:</span><span class="sxs-lookup"><span data-stu-id="5f4c5-251">Now that you've built an application that uses hello Service Bus relay service, see hello following articles toolearn more about Azure Relay:</span></span>

* [<span data-ttu-id="5f4c5-252">Přehled architektury služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="5f4c5-252">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [<span data-ttu-id="5f4c5-253">Přehled služby Azure Relay</span><span class="sxs-lookup"><span data-stu-id="5f4c5-253">Azure Relay overview</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="5f4c5-254">Jak toouse hello WCF předávání služby pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="5f4c5-254">How toouse hello WCF relay service with .NET</span></span>](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
