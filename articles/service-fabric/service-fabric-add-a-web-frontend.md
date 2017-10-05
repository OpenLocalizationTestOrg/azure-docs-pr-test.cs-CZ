---
title: "Vytvořte front-end webové aplikace Azure Service Fabric pomocí ASP.NET Core | Microsoft Docs"
description: "Zveřejnit aplikaci Service Fabric na webu pomocí služby projektu ASP.NET Core a komunikace mezi službami prostřednictvím vzdálené komunikace služby."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: b19aaa652f2c15573ded632ca1348e1a6752f080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a><span data-ttu-id="c97ed-103">Vytvoření služby front-end webové aplikace pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c97ed-103">Build a web service front end for your application using ASP.NET Core</span></span>
<span data-ttu-id="c97ed-104">Ve výchozím nastavení neposkytují služby Azure Service Fabric veřejné rozhraní na webu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-104">By default, Azure Service Fabric services do not provide a public interface to the web.</span></span> <span data-ttu-id="c97ed-105">Vystavit funkcionalitu vaší aplikace do klientů protokolu HTTP, budete muset vytvořit webového projektu fungovat jako vstupní bod a potom z ní sdělit jednotlivých služeb.</span><span class="sxs-lookup"><span data-stu-id="c97ed-105">To expose your application's functionality to HTTP clients, you have to create a web project to act as an entry point and then communicate from there to your individual services.</span></span>

<span data-ttu-id="c97ed-106">V tomto kurzu budeme pokračovat tam kde jsme skončil v [vytvoření vaší první aplikace v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) kurzu a přidání webové služby ASP.NET Core před službu stavová čítače.</span><span class="sxs-lookup"><span data-stu-id="c97ed-106">In this tutorial, we pick up where we left off in the [Creating your first application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) tutorial and add an ASP.NET Core web service in front of the stateful counter service.</span></span> <span data-ttu-id="c97ed-107">Pokud jste tak již neučinili, měli přejděte zpět a první krok prostřednictvím tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-107">If you have not already done so, you should go back and step through that tutorial first.</span></span>

## <a name="set-up-your-environment-for-aspnet-core"></a><span data-ttu-id="c97ed-108">Nastavení prostředí pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c97ed-108">Set up your environment for ASP.NET Core</span></span>

<span data-ttu-id="c97ed-109">Budete potřebovat Visual Studio 2017 podle spolu se v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-109">You need Visual Studio 2017 to follow along with this tutorial.</span></span> <span data-ttu-id="c97ed-110">Všechny edice provede.</span><span class="sxs-lookup"><span data-stu-id="c97ed-110">Any edition will do.</span></span> 

 - [<span data-ttu-id="c97ed-111">Nainstalovat Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c97ed-111">Install Visual Studio 2017</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="c97ed-112">K vývoji aplikací ASP.NET Core Service Fabric, musí mít nainstalované následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c97ed-112">To develop ASP.NET Core Service Fabric applications, you should have the following workloads installed:</span></span>
 - <span data-ttu-id="c97ed-113">**Vývoj pro Azure** (v části *Web a Cloud*)</span><span class="sxs-lookup"><span data-stu-id="c97ed-113">**Azure development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="c97ed-114">**Vývoj pro ASP.NET a webové** (v části *Web a Cloud*)</span><span class="sxs-lookup"><span data-stu-id="c97ed-114">**ASP.NET and web development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="c97ed-115">**Vývoj pro různé platformy .NET core** (v části *ostatní modulové*)</span><span class="sxs-lookup"><span data-stu-id="c97ed-115">**.NET Core cross-platform development** (under *Other Toolsets*)</span></span>

>[!Note] 
><span data-ttu-id="c97ed-116">.NET Core nástrojů pro Visual Studio 2015 se už aktualizují, ale pokud stále používáte Visual Studio 2015, budete muset mít [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="c97ed-116">The .NET Core tools for Visual Studio 2015 are no longer being updated, however if you are still using Visual Studio 2015, you need to have [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) installed.</span></span>

## <a name="add-an-aspnet-core-service-to-your-application"></a><span data-ttu-id="c97ed-117">Přidání služby ASP.NET Core do aplikace</span><span class="sxs-lookup"><span data-stu-id="c97ed-117">Add an ASP.NET Core service to your application</span></span>
<span data-ttu-id="c97ed-118">ASP.NET Core je odlehčený, napříč platformami webové rozhraní vývoj, které vám pomůže vytvořit moderní webové uživatelské rozhraní a webovým rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="c97ed-118">ASP.NET Core is a lightweight, cross-platform web development framework that you can use to create modern web UI and web APIs.</span></span> 

<span data-ttu-id="c97ed-119">Umožňuje přidat projekt webového rozhraní API ASP.NET naše stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="c97ed-119">Let's add an ASP.NET Web API project to our existing application.</span></span>

1. <span data-ttu-id="c97ed-120">V Průzkumníku řešení klikněte pravým tlačítkem na **služby** v aplikaci projektu a zvolte **Přidat > Nový Service Fabric Service**.</span><span class="sxs-lookup"><span data-stu-id="c97ed-120">In Solution Explorer, right-click **Services** within the application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Přidání nové služby do existující aplikace][vs-add-new-service]
2. <span data-ttu-id="c97ed-122">Na **vytvoření služby** vyberte **ASP.NET Core** a pojmenujte ho.</span><span class="sxs-lookup"><span data-stu-id="c97ed-122">On the **Create a Service** page, choose **ASP.NET Core** and give it a name.</span></span>
   
    ![Výběr v dialogovém okně Nový služby webovou službu ASP.NET.][vs-new-service-dialog]

3. <span data-ttu-id="c97ed-124">Další stránka obsahuje sadu ASP.NET Core šablony projektů.</span><span class="sxs-lookup"><span data-stu-id="c97ed-124">The next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="c97ed-125">Všimněte si, že jsou stejné možnosti, které by vidíte, pokud jste vytvořili projekt ASP.NET Core mimo aplikace Service Fabric pomocí malé množství další kód pro registraci služby modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c97ed-125">Note that these are the same choices that you would see if you created an ASP.NET Core project outside of a Service Fabric application, with a small amount of additional code to register the service with the Service Fabric runtime.</span></span> <span data-ttu-id="c97ed-126">V tomto kurzu zvolte **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="c97ed-126">For this tutorial, choose **Web API**.</span></span> <span data-ttu-id="c97ed-127">Koncepty však můžete použít k vytvoření úplné webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c97ed-127">However, you can apply the same concepts to building a full web application.</span></span>
   
    ![Výběr typu projektu ASP.NET][vs-new-aspnet-project-dialog]
   
    <span data-ttu-id="c97ed-129">Po vytvoření projektu webového rozhraní API byste měli mít dvě služby ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c97ed-129">Once your Web API project is created, you should have two services in your application.</span></span> <span data-ttu-id="c97ed-130">Při dalším sestavit aplikaci, můžete přidat další služby stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c97ed-130">As you continue to build your application, you can add more services in exactly the same way.</span></span> <span data-ttu-id="c97ed-131">Každý může být nezávisle verzí a upgradovaný.</span><span class="sxs-lookup"><span data-stu-id="c97ed-131">Each can be independently versioned and upgraded.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="c97ed-132">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c97ed-132">Run the application</span></span>
<span data-ttu-id="c97ed-133">Chcete-li získat představu o co máme jste Hotovo, umožňuje nasazení nové aplikace a podívejte se na výchozí chování, které poskytuje webové rozhraní API ASP.NET Core šablony.</span><span class="sxs-lookup"><span data-stu-id="c97ed-133">To get a sense of what we've done, let's deploy the new application and take a look at the default behavior that the ASP.NET Core Web API template provides.</span></span>

1. <span data-ttu-id="c97ed-134">Stisknutím klávesy F5 v sadě Visual Studio k ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c97ed-134">Press F5 in Visual Studio to debug the app.</span></span>
2. <span data-ttu-id="c97ed-135">Po dokončení nasazení sady Visual Studio spustí prohlížeč na kořenový server služby webového rozhraní API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c97ed-135">When deployment is complete, Visual Studio launches a browser to the root of the ASP.NET Web API service.</span></span> <span data-ttu-id="c97ed-136">Šablona webového rozhraní API ASP.NET Core neposkytuje výchozí chování pro kořenový adresář, takže byste měli vidět chybu 404 v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c97ed-136">The ASP.NET Core Web API template doesn't provide default behavior for the root, so you should see a 404 error in the browser.</span></span>
3. <span data-ttu-id="c97ed-137">Přidat `/api/values` do umístění v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c97ed-137">Add `/api/values` to the location in the browser.</span></span> <span data-ttu-id="c97ed-138">Tím se spustí `Get` metodu ValuesController v šabloně webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c97ed-138">This invokes the `Get` method on the ValuesController in the Web API template.</span></span> <span data-ttu-id="c97ed-139">Vrátí výchozí odpovědi, které poskytuje šablony – pole JSON, který obsahuje dva řetězce:</span><span class="sxs-lookup"><span data-stu-id="c97ed-139">It returns the default response that is provided by the template--a JSON array that contains two strings:</span></span>
   
    ![Výchozí hodnot vrácených z webového rozhraní API ASP.NET Core šablony][browser-aspnet-template-values]
   
    <span data-ttu-id="c97ed-141">Na konci tohoto kurzu Tato stránka se zobrazí nejnovější hodnotu čítače z našich stavové služby namísto výchozí řetězců.</span><span class="sxs-lookup"><span data-stu-id="c97ed-141">By the end of the tutorial, this page will show the most recent counter value from our stateful service instead of the default strings.</span></span>

## <a name="connect-the-services"></a><span data-ttu-id="c97ed-142">Připojení služby</span><span class="sxs-lookup"><span data-stu-id="c97ed-142">Connect the services</span></span>
<span data-ttu-id="c97ed-143">Service Fabric nabízí flexibilitu v tom, jak komunikovat se službami reliable services.</span><span class="sxs-lookup"><span data-stu-id="c97ed-143">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="c97ed-144">V rámci jedné aplikace můžete mít služby, které jsou přístupné přes TCP, jiných služeb, které jsou přístupné přes rozhraní HTTP REST API a stále jiných služeb, které jsou přístupné přes webové sokety.</span><span class="sxs-lookup"><span data-stu-id="c97ed-144">Within a single application, you might have services that are accessible via TCP, other services that are accessible via an HTTP REST API, and still other services that are accessible via web sockets.</span></span> <span data-ttu-id="c97ed-145">Pro informace o dostupných možnostech a kompromisy související se situací, viz [komunikaci se službou](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="c97ed-145">For background on the options available and the tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span> <span data-ttu-id="c97ed-146">V tomto kurzu používáme [vzdálené komunikace služby Service Fabric](service-fabric-reliable-services-communication-remoting.md), zadaný v sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="c97ed-146">In this tutorial, we use [Service Fabric Service Remoting](service-fabric-reliable-services-communication-remoting.md), provided in the SDK.</span></span>

<span data-ttu-id="c97ed-147">V metodě vzdálené komunikace služby (modelován na vzdálených volání procedur nebo RPC) definujete rozhraní tak, aby fungoval jako veřejného kontraktu služby.</span><span class="sxs-lookup"><span data-stu-id="c97ed-147">In the Service Remoting approach (modeled on remote procedure calls or RPCs), you define an interface to act as the public contract for the service.</span></span> <span data-ttu-id="c97ed-148">Pak použijte tohoto rozhraní k vygenerování třídu proxy pro interakci se službou.</span><span class="sxs-lookup"><span data-stu-id="c97ed-148">Then, you use that interface to generate a proxy class for interacting with the service.</span></span>

### <a name="create-the-remoting-interface"></a><span data-ttu-id="c97ed-149">Vytvořit rozhraní vzdálené komunikace</span><span class="sxs-lookup"><span data-stu-id="c97ed-149">Create the remoting interface</span></span>
<span data-ttu-id="c97ed-150">Začněme vytvořením rozhraní fungovat jako kontrakt mezi stavové služby a další služby, v tomto případě ASP.NET Core webového projektu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-150">Let's start by creating the interface to act as the contract between the stateful service and other services, in this case the ASP.NET Core web project.</span></span> <span data-ttu-id="c97ed-151">Toto rozhraní musí být sdílená všech služeb, které ho používají volání RPC, takže ho vytvoříme v jeho vlastní projektu knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="c97ed-151">This interface must be shared by all services that use it to make RPC calls, so we'll create it in its own Class Library project.</span></span>

1. <span data-ttu-id="c97ed-152">V Průzkumníku řešení klikněte pravým tlačítkem na řešení a zvolte **přidat** > **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="c97ed-152">In Solution Explorer, right-click your solution and choose **Add** > **New Project**.</span></span>

2. <span data-ttu-id="c97ed-153">Vyberte **Visual C#** v levém navigačním podokně a potom vyberte položku **knihovny tříd** šablony.</span><span class="sxs-lookup"><span data-stu-id="c97ed-153">Choose the **Visual C#** entry in the left navigation pane and then select the **Class Library** template.</span></span> <span data-ttu-id="c97ed-154">Ujistěte se, že verze rozhraní .NET Framework je nastavená na **4.5.2**.</span><span class="sxs-lookup"><span data-stu-id="c97ed-154">Ensure that the .NET Framework version is set to **4.5.2**.</span></span>
   
    ![Vytvoření projektu rozhraní pro stavové služby][vs-add-class-library-project]

3. <span data-ttu-id="c97ed-156">Nainstalujte **Microsoft.ServiceFabric.Services.Remoting** balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="c97ed-156">Install the **Microsoft.ServiceFabric.Services.Remoting** NuGet package.</span></span> <span data-ttu-id="c97ed-157">Vyhledejte **Microsoft.ServiceFabric.Services.Remoting** nuget balíček správce a instalace pro všechny projekty v řešení, které používají vzdálené komunikace služby, včetně:</span><span class="sxs-lookup"><span data-stu-id="c97ed-157">Search for  **Microsoft.ServiceFabric.Services.Remoting** in the NuGet package manager and install it for all projects in the solution that use Service Remoting, including:</span></span>
   - <span data-ttu-id="c97ed-158">Knihovna tříd projekt, který obsahuje rozhraní služby</span><span class="sxs-lookup"><span data-stu-id="c97ed-158">The Class Library project that contains the service interface</span></span>
   - <span data-ttu-id="c97ed-159">Projekt stavové služby</span><span class="sxs-lookup"><span data-stu-id="c97ed-159">The Stateful Service project</span></span>
   - <span data-ttu-id="c97ed-160">Projektu ASP.NET Core webové služby</span><span class="sxs-lookup"><span data-stu-id="c97ed-160">The ASP.NET Core web service project</span></span>
   
    ![Probíhá přidávání balíčku NuGet služby][vs-services-nuget-package]

4. <span data-ttu-id="c97ed-162">V knihovně tříd vytvořit rozhraní s jedinou metodu `GetCountAsync`, a rozšířit rozhraní z `Microsoft.ServiceFabric.Services.Remoting.IService`.</span><span class="sxs-lookup"><span data-stu-id="c97ed-162">In the class library, create an interface with a single method, `GetCountAsync`, and extend the interface from `Microsoft.ServiceFabric.Services.Remoting.IService`.</span></span> <span data-ttu-id="c97ed-163">Vzdálené komunikace rozhraní musí být odvozeny od tohoto rozhraní to znamená, že je Služba vzdálené komunikace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c97ed-163">The remoting interface must derive from this interface to indicate that it is a Service Remoting interface.</span></span>
   
    ```c#
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-the-interface-in-your-stateful-service"></a><span data-ttu-id="c97ed-164">Toto rozhraní implementovat v stavové služby</span><span class="sxs-lookup"><span data-stu-id="c97ed-164">Implement the interface in your stateful service</span></span>
<span data-ttu-id="c97ed-165">Teď, když jsme definovali rozhraní, musíme implementovat v stavové služby.</span><span class="sxs-lookup"><span data-stu-id="c97ed-165">Now that we have defined the interface, we need to implement it in the stateful service.</span></span>

1. <span data-ttu-id="c97ed-166">Stavové služby přidejte odkaz na projekt knihovny tříd, který obsahuje rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c97ed-166">In your stateful service, add a reference to the class library project that contains the interface.</span></span>
   
    ![Přidat odkaz na projekt knihovny tříd v stavové služby][vs-add-class-library-reference]
2. <span data-ttu-id="c97ed-168">Najít třídu, která dědí z `StatefulService`, jako například `MyStatefulService`, a rozšířit ji k implementaci `ICounter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c97ed-168">Locate the class that inherits from `StatefulService`, such as `MyStatefulService`, and extend it to implement the `ICounter` interface.</span></span>
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. <span data-ttu-id="c97ed-169">Nyní implementovat jednu metodu, která je definována v `ICounter` rozhraní `GetCountAsync`.</span><span class="sxs-lookup"><span data-stu-id="c97ed-169">Now implement the single method that is defined in the `ICounter` interface, `GetCountAsync`.</span></span>
   
    ```c#
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a><span data-ttu-id="c97ed-170">Vystavení stavové služby pomocí služby vzdálené komunikace naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="c97ed-170">Expose the stateful service using a service remoting listener</span></span>
<span data-ttu-id="c97ed-171">Pomocí `ICounter` rozhraní implementované, posledním krokem je otevřít vzdálenou komunikaci služby komunikační kanál.</span><span class="sxs-lookup"><span data-stu-id="c97ed-171">With the `ICounter` interface implemented, the final step is to open the Service Remoting communication channel.</span></span> <span data-ttu-id="c97ed-172">Pro stavové služby, Service Fabric nabízí přepisovatelné metodu s názvem `CreateServiceReplicaListeners`.</span><span class="sxs-lookup"><span data-stu-id="c97ed-172">For stateful services, Service Fabric provides an overridable method called `CreateServiceReplicaListeners`.</span></span> <span data-ttu-id="c97ed-173">Pomocí této metody můžete určit jeden nebo více naslouchací procesy komunikace, na základě typu komunikaci, která chcete povolit pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-173">With this method, you can specify one or more communication listeners, based on the type of communication that you want to enable for your service.</span></span>

> [!NOTE]
> <span data-ttu-id="c97ed-174">Volání metody ekvivalentní pro otevření komunikační kanál pro bezstavové služby `CreateServiceInstanceListeners`.</span><span class="sxs-lookup"><span data-stu-id="c97ed-174">The equivalent method for opening a communication channel to stateless services is called `CreateServiceInstanceListeners`.</span></span>

<span data-ttu-id="c97ed-175">V takovém případě jsme nahradit existující `CreateServiceReplicaListeners` metoda a zadejte instanci `ServiceRemotingListener`, která vytvoří koncový bod protokolu RPC, lze volat z klientů prostřednictvím `ServiceProxy`.</span><span class="sxs-lookup"><span data-stu-id="c97ed-175">In this case, we replace the existing `CreateServiceReplicaListeners` method and provide an instance of `ServiceRemotingListener`, which creates an RPC endpoint that is callable from clients through `ServiceProxy`.</span></span>  

<span data-ttu-id="c97ed-176">`CreateServiceRemotingListener` Rozšiřující metody na `IService` rozhraní umožňuje snadno vytvářet `ServiceRemotingListener` s všechna výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="c97ed-176">The `CreateServiceRemotingListener` extension method on the `IService` interface allows you to easily create a `ServiceRemotingListener` with all default settings.</span></span> <span data-ttu-id="c97ed-177">Při použití této metody rozšíření, zajistěte, abyste měli `Microsoft.ServiceFabric.Services.Remoting.Runtime` importovat obor názvů.</span><span class="sxs-lookup"><span data-stu-id="c97ed-177">To use this extension method, ensure you have the `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace imported.</span></span> 

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a><span data-ttu-id="c97ed-178">Použití třídy ServiceProxy komunikovat se službou</span><span class="sxs-lookup"><span data-stu-id="c97ed-178">Use the ServiceProxy class to interact with the service</span></span>
<span data-ttu-id="c97ed-179">Naše stavové služby je nyní připravena přijímat provoz z jiných služeb prostřednictvím protokolu RPC.</span><span class="sxs-lookup"><span data-stu-id="c97ed-179">Our stateful service is now ready to receive traffic from other services over RPC.</span></span> <span data-ttu-id="c97ed-180">Takže zůstane je přidání kód ke komunikaci s ním z webovou službu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c97ed-180">So all that remains is adding the code to communicate with it from the ASP.NET web service.</span></span>

1. <span data-ttu-id="c97ed-181">V projektu ASP.NET přidejte odkaz na knihovnu tříd, který obsahuje `ICounter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c97ed-181">In your ASP.NET project, add a reference to the class library that contains the `ICounter` interface.</span></span>

2. <span data-ttu-id="c97ed-182">Dříve jste přidali **Microsoft.ServiceFabric.Services.Remoting** balíček NuGet do projektu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c97ed-182">Earlier you added the **Microsoft.ServiceFabric.Services.Remoting** NuGet package to the ASP.NET project.</span></span> <span data-ttu-id="c97ed-183">Tento balíček poskytuje `ServiceProxy` třídy, které můžete provádět RPC volá, aby se stavové služby.</span><span class="sxs-lookup"><span data-stu-id="c97ed-183">This package provides the `ServiceProxy` class which you use to make RPC calls to the stateful service.</span></span> <span data-ttu-id="c97ed-184">Zkontrolujte, jestli že tento balíček NuGet je nainstalované v projektu webové služby ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c97ed-184">Ensure this NuGet package is installed in the ASP.NET Core web service project.</span></span>

4. <span data-ttu-id="c97ed-185">V **řadiče** složku, otevřete `ValuesController` třídy.</span><span class="sxs-lookup"><span data-stu-id="c97ed-185">In the **Controllers** folder, open the `ValuesController` class.</span></span> <span data-ttu-id="c97ed-186">Všimněte si, že `Get` metoda aktuálně právě vrátí pole pevně řetězce "hodnota1" a "hodnota2"--který by odpovídal co jsme viděli dříve v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c97ed-186">Note that the `Get` method currently just returns a hard-coded string array of "value1" and "value2"--which matches what we saw earlier in the browser.</span></span> <span data-ttu-id="c97ed-187">Tato implementace nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c97ed-187">Replace this implementation with the following code:</span></span>
   
    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    <span data-ttu-id="c97ed-188">První řádek kódu je klíče.</span><span class="sxs-lookup"><span data-stu-id="c97ed-188">The first line of code is the key one.</span></span> <span data-ttu-id="c97ed-189">Vytvořit proxy ICounter pro stavové služby, musíte zadat dva kusy informace: název služby a ID oddílu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-189">To create the ICounter proxy to the stateful service, you need to provide two pieces of information: a partition ID and the name of the service.</span></span>
   
    <span data-ttu-id="c97ed-190">Pomocí rozdělení do oddílů stavové služby porušením jejich stav do různých intervalů, na základě klíče, které definujete, jako je ID zákazníka nebo PSČ.</span><span class="sxs-lookup"><span data-stu-id="c97ed-190">You can use partitioning to scale stateful services by breaking up their state into different buckets, based on a key that you define, such as a customer ID or postal code.</span></span> <span data-ttu-id="c97ed-191">V naší aplikaci trivial má stavové služby jenom jeden oddíl, tak klíč není důležité.</span><span class="sxs-lookup"><span data-stu-id="c97ed-191">In our trivial application, the stateful service only has one partition, so the key doesn't matter.</span></span> <span data-ttu-id="c97ed-192">Všechny klíče, který zadáte přejde do stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-192">Any key that you provide will lead to the same partition.</span></span> <span data-ttu-id="c97ed-193">Další informace o vytváření oddílů služby najdete v tématu [postup oddíl spolehlivé služby Service Fabric](service-fabric-concepts-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="c97ed-193">To learn more about partitioning services, see [How to partition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).</span></span>
   
    <span data-ttu-id="c97ed-194">Název služby je identifikátor URI formuláře fabric: /&lt;název_aplikace&gt;/&lt;service_name&gt;.</span><span class="sxs-lookup"><span data-stu-id="c97ed-194">The service name is a URI of the form fabric:/&lt;application_name&gt;/&lt;service_name&gt;.</span></span>
   
    <span data-ttu-id="c97ed-195">Pomocí těchto dvou položek příslušné Service Fabric jednoznačně identifikovat na počítač, který by měl být odeslány požadavky.</span><span class="sxs-lookup"><span data-stu-id="c97ed-195">With these two pieces of information, Service Fabric can uniquely identify the machine that requests should be sent to.</span></span> <span data-ttu-id="c97ed-196">`ServiceProxy` Třída také bezproblémově zpracovává případ, kdy je počítač, který je hostitelem oddílu stavové služby selže a jiný počítač musí být povýšen na jeho proběhnout.</span><span class="sxs-lookup"><span data-stu-id="c97ed-196">The `ServiceProxy` class also seamlessly handles the case where the machine that hosts the stateful service partition fails and another machine must be promoted to take its place.</span></span> <span data-ttu-id="c97ed-197">Tato abstrakce usnadňuje psaní kódu klienta, jak nakládat s jinými službami výrazně jednodušší.</span><span class="sxs-lookup"><span data-stu-id="c97ed-197">This abstraction makes writing the client code to deal with other services significantly simpler.</span></span>
   
    <span data-ttu-id="c97ed-198">Jakmile jsme proxy server, můžeme jednoduše vyvolání `GetCountAsync` metodu a vrátí její výsledek.</span><span class="sxs-lookup"><span data-stu-id="c97ed-198">Once we have the proxy, we simply invoke the `GetCountAsync` method and return its result.</span></span>

5. <span data-ttu-id="c97ed-199">Stisknutím klávesy F5 spusťte upravené aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c97ed-199">Press F5 again to run the modified application.</span></span> <span data-ttu-id="c97ed-200">Jako dříve, Visual Studio automaticky spustí prohlížeč do kořenového adresáře webového projektu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-200">As before, Visual Studio automatically launches the browser to the root of the web project.</span></span> <span data-ttu-id="c97ed-201">Přidejte cestu "rozhraní api nebo hodnoty" a měli byste vidět aktuální vrátila hodnotu čítače.</span><span class="sxs-lookup"><span data-stu-id="c97ed-201">Add the "api/values" path, and you should see the current counter value returned.</span></span>
   
    ![Stavová čítače hodnota zobrazená v prohlížeči][browser-aspnet-counter-value]
   
    <span data-ttu-id="c97ed-203">Aktualizujte stránku prohlížeče pravidelně zobrazíte hodnota čítače aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="c97ed-203">Refresh the browser periodically to see the counter value update.</span></span>

## <a name="kestrel-and-weblistener"></a><span data-ttu-id="c97ed-204">Kestrel a WebListener</span><span class="sxs-lookup"><span data-stu-id="c97ed-204">Kestrel and WebListener</span></span>

<span data-ttu-id="c97ed-205">Webový server výchozí ASP.NET Core, označuje jako Kestrel, je [není podporován pro zpracování přímé přenosy z Internetu](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c97ed-205">The default ASP.NET Core web server, known as Kestrel, is [not currently supported for handling direct internet traffic](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span></span> <span data-ttu-id="c97ed-206">V důsledku toho šablonu ASP.NET Core bezstavové služby Service Fabric používá [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c97ed-206">As a result, the ASP.NET Core stateless service template for Service Fabric uses [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) by default.</span></span> 

<span data-ttu-id="c97ed-207">Další informace o Kestrel a WebListener v Service Fabric služby, naleznete v [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="c97ed-207">To learn more about Kestrel and WebListener in Service Fabric services, please refer to [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

## <a name="connecting-to-a-reliable-actor-service"></a><span data-ttu-id="c97ed-208">Připojení ke službě spolehlivé objektu Actor</span><span class="sxs-lookup"><span data-stu-id="c97ed-208">Connecting to a Reliable Actor service</span></span>
<span data-ttu-id="c97ed-209">Tento kurz se zaměřuje na přidání webový front-end, který oznamovat se stavovou službou.</span><span class="sxs-lookup"><span data-stu-id="c97ed-209">This tutorial focused on adding a web front end that communicated with a stateful service.</span></span> <span data-ttu-id="c97ed-210">Můžete však provést velmi podobný modelu, aby komunikoval s aktéři.</span><span class="sxs-lookup"><span data-stu-id="c97ed-210">However, you can follow a very similar model to talk to actors.</span></span> <span data-ttu-id="c97ed-211">Při vytváření spolehlivé objektu Actor projektu sady Visual Studio automaticky generuje projektu rozhraní za vás.</span><span class="sxs-lookup"><span data-stu-id="c97ed-211">When you create a Reliable Actor project, Visual Studio automatically generates an interface project for you.</span></span> <span data-ttu-id="c97ed-212">Toto rozhraní můžete použít ke generování proxy služby objektu actor v webového projektu ke komunikaci s objektu actor.</span><span class="sxs-lookup"><span data-stu-id="c97ed-212">You can use that interface to generate an actor proxy in the web project to communicate with the actor.</span></span> <span data-ttu-id="c97ed-213">Komunikační kanál je zadán automaticky.</span><span class="sxs-lookup"><span data-stu-id="c97ed-213">The communication channel is provided automatically.</span></span> <span data-ttu-id="c97ed-214">Proto není potřeba dělat nic, který je ekvivalentní pro vytvoření `ServiceRemotingListener` stejně, jako jste pro stavové služby v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c97ed-214">So you do not need to do anything that is equivalent to establishing a `ServiceRemotingListener` like you did for the stateful service in this tutorial.</span></span>

## <a name="how-web-services-work-on-your-local-cluster"></a><span data-ttu-id="c97ed-215">Jak fungují webové služby v místním clusteru</span><span class="sxs-lookup"><span data-stu-id="c97ed-215">How web services work on your local cluster</span></span>
<span data-ttu-id="c97ed-216">Obecně platí můžete nasadit přesně stejnou aplikaci Service Fabric na clusteru s více počítači, který jste nasadili v místním clusteru a být vysoce jistý, že funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="c97ed-216">In general, you can deploy exactly the same Service Fabric application to a multi-machine cluster that you deployed on your local cluster and be highly confident that it works as you expect.</span></span> <span data-ttu-id="c97ed-217">Je to proto, že místní cluster je jednoduše konfigurace pěti uzly sbalena na jednom počítači.</span><span class="sxs-lookup"><span data-stu-id="c97ed-217">This is because your local cluster is simply a five-node configuration that is collapsed to a single machine.</span></span>

<span data-ttu-id="c97ed-218">Pokud jde o webové služby, je však jeden nuance klíče.</span><span class="sxs-lookup"><span data-stu-id="c97ed-218">When it comes to web services, however, there is one key nuance.</span></span> <span data-ttu-id="c97ed-219">Při clusteru se nachází za službou Vyrovnávání zatížení, jako je tomu v Azure, musíte zajistit, aby webové služby jsou nasazeny na každý počítač od nástroje pro vyrovnávání zatížení jednoduše kruhové dotazování provoz mezi počítači.</span><span class="sxs-lookup"><span data-stu-id="c97ed-219">When your cluster sits behind a load balancer, as it does in Azure, you must ensure that your web services are deployed on every machine since the load balancer simply round-robins traffic across the machines.</span></span> <span data-ttu-id="c97ed-220">To provedete nastavením `InstanceCount` pro službu na zvláštní hodnotu-1.</span><span class="sxs-lookup"><span data-stu-id="c97ed-220">You can do this by setting the `InstanceCount` for the service to the special value of "-1".</span></span>

<span data-ttu-id="c97ed-221">Naopak když jste místní spuštění webové služby, musíte zajistit, že pouze jedna instance je služba spuštěná.</span><span class="sxs-lookup"><span data-stu-id="c97ed-221">By contrast, when you run a web service locally, you need to ensure that only one instance of the service is running.</span></span> <span data-ttu-id="c97ed-222">Spustíte, jinak hodnota do konfliktu z více procesy, které jsou na stejné cesty a portu naslouchá.</span><span class="sxs-lookup"><span data-stu-id="c97ed-222">Otherwise, you run into conflicts from multiple processes that are listening on the same path and port.</span></span> <span data-ttu-id="c97ed-223">Počet instancí služby webové v důsledku toho musí být nastavená na 1, pro místní nasazení.</span><span class="sxs-lookup"><span data-stu-id="c97ed-223">As a result, the web service instance count should be set to "1" for local deployments.</span></span>

<span data-ttu-id="c97ed-224">Naučte se konfigurovat různé hodnoty pro různé prostředí, najdete v tématu [Správa parametry aplikace pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="c97ed-224">To learn how to configure different values for different environment, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c97ed-225">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c97ed-225">Next steps</span></span>
<span data-ttu-id="c97ed-226">Teď, když máte webové přední skončili sadu pro vaši aplikaci s ASP.NET Core, další informace o [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md) pro podrobné pochopení fungování ASP.NET Core s Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c97ed-226">Now that you have a web front end set up for your application with ASP.NET Core, learn more about [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) for an in-depth understanding of how ASP.NET Core works with Service Fabric.</span></span>

<span data-ttu-id="c97ed-227">Dále [Další informace o komunikaci se službou](service-fabric-connect-and-communicate-with-services.md) v obecné získat úplná obrázek o tom, jak služba funguje komunikace v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c97ed-227">Next, [learn more about communicating with services](service-fabric-connect-and-communicate-with-services.md) in general to get a complete picture of how service communication works in Service Fabric.</span></span>

<span data-ttu-id="c97ed-228">Až budete mít dostatečné povědomí o tom, jak funguje komunikace služby, [vytvořte cluster v Azure a nasazení aplikace do cloudu](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c97ed-228">Once you have a good understanding of how service communication works, [create a cluster in Azure and deploy your application to the cloud](service-fabric-cluster-creation-via-portal.md).</span></span>

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
