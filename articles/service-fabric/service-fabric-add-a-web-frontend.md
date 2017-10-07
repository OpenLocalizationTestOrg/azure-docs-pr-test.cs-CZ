---
title: "aaaCreate front-end webové aplikace Azure Service Fabric pomocí ASP.NET Core | Microsoft Docs"
description: "Vystavení webového toohello aplikace Service Fabric pomocí služby projektu ASP.NET Core a komunikace mezi službami prostřednictvím vzdálené komunikace služby."
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
ms.openlocfilehash: 0c4454d6cac4c2e343bd6e93e56d3d2f0ebfc4ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a><span data-ttu-id="12148-103">Vytvoření služby front-end webové aplikace pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12148-103">Build a web service front end for your application using ASP.NET Core</span></span>
<span data-ttu-id="12148-104">Ve výchozím nastavení neposkytují služby Azure Service Fabric webové toohello veřejné rozhraní.</span><span class="sxs-lookup"><span data-stu-id="12148-104">By default, Azure Service Fabric services do not provide a public interface toohello web.</span></span> <span data-ttu-id="12148-105">tooexpose klienti tooHTTP funkce aplikace, máte toocreate webového projektu tooact jako vstupní bod a poté komunikaci ze zde tooyour jednotlivých služeb.</span><span class="sxs-lookup"><span data-stu-id="12148-105">tooexpose your application's functionality tooHTTP clients, you have toocreate a web project tooact as an entry point and then communicate from there tooyour individual services.</span></span>

<span data-ttu-id="12148-106">V tomto kurzu budeme pokračovat tam kde jsme skončil v hello [vytvoření vaší první aplikace v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) kurzu a přidání webové služby ASP.NET Core před hello čítač stavové služby.</span><span class="sxs-lookup"><span data-stu-id="12148-106">In this tutorial, we pick up where we left off in hello [Creating your first application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) tutorial and add an ASP.NET Core web service in front of hello stateful counter service.</span></span> <span data-ttu-id="12148-107">Pokud jste tak již neučinili, měli přejděte zpět a první krok prostřednictvím tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="12148-107">If you have not already done so, you should go back and step through that tutorial first.</span></span>

## <a name="set-up-your-environment-for-aspnet-core"></a><span data-ttu-id="12148-108">Nastavení prostředí pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12148-108">Set up your environment for ASP.NET Core</span></span>

<span data-ttu-id="12148-109">Budete potřebovat Visual Studio 2017 toofollow spolu se v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="12148-109">You need Visual Studio 2017 toofollow along with this tutorial.</span></span> <span data-ttu-id="12148-110">Všechny edice provede.</span><span class="sxs-lookup"><span data-stu-id="12148-110">Any edition will do.</span></span> 

 - [<span data-ttu-id="12148-111">Nainstalovat Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="12148-111">Install Visual Studio 2017</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="12148-112">aplikace ASP.NET Core Service Fabric toodevelop, byste měli mít hello úlohy, které jsou nainstalované následující:</span><span class="sxs-lookup"><span data-stu-id="12148-112">toodevelop ASP.NET Core Service Fabric applications, you should have hello following workloads installed:</span></span>
 - <span data-ttu-id="12148-113">**Vývoj pro Azure** (v části *Web a Cloud*)</span><span class="sxs-lookup"><span data-stu-id="12148-113">**Azure development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="12148-114">**Vývoj pro ASP.NET a webové** (v části *Web a Cloud*)</span><span class="sxs-lookup"><span data-stu-id="12148-114">**ASP.NET and web development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="12148-115">**Vývoj pro různé platformy .NET core** (v části *ostatní modulové*)</span><span class="sxs-lookup"><span data-stu-id="12148-115">**.NET Core cross-platform development** (under *Other Toolsets*)</span></span>

>[!Note] 
><span data-ttu-id="12148-116">Hello .NET Core nástrojů pro Visual Studio 2015 se už aktualizují, ale pokud stále používáte Visual Studio 2015, je třeba toohave [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="12148-116">hello .NET Core tools for Visual Studio 2015 are no longer being updated, however if you are still using Visual Studio 2015, you need toohave [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) installed.</span></span>

## <a name="add-an-aspnet-core-service-tooyour-application"></a><span data-ttu-id="12148-117">Přidání aplikace ASP.NET Core služby tooyour</span><span class="sxs-lookup"><span data-stu-id="12148-117">Add an ASP.NET Core service tooyour application</span></span>
<span data-ttu-id="12148-118">ASP.NET Core je rozhraní vývoj webové lightweight, a platformy, můžete použít toocreate moderní webového uživatelského rozhraní a webovým rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="12148-118">ASP.NET Core is a lightweight, cross-platform web development framework that you can use toocreate modern web UI and web APIs.</span></span> 

<span data-ttu-id="12148-119">Umožňuje přidat existující aplikaci tooour projekt webového rozhraní API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="12148-119">Let's add an ASP.NET Web API project tooour existing application.</span></span>

1. <span data-ttu-id="12148-120">V Průzkumníku řešení klikněte pravým tlačítkem na **služby** uvnitř hello projekt aplikace a zvolte **Přidat > Nový Service Fabric Service**.</span><span class="sxs-lookup"><span data-stu-id="12148-120">In Solution Explorer, right-click **Services** within hello application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Přidání nové služby tooan existující aplikace][vs-add-new-service]
2. <span data-ttu-id="12148-122">Na hello **vytvoření služby** vyberte **ASP.NET Core** a pojmenujte ho.</span><span class="sxs-lookup"><span data-stu-id="12148-122">On hello **Create a Service** page, choose **ASP.NET Core** and give it a name.</span></span>
   
    ![Výběr webové služby ASP.NET v hello dialogové okno Nová služba][vs-new-service-dialog]

3. <span data-ttu-id="12148-124">Další stránku Hello poskytuje sadu ASP.NET Core šablony projektů.</span><span class="sxs-lookup"><span data-stu-id="12148-124">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="12148-125">Všimněte si, že tyto jsou hello stejné možnosti, které by vidíte, pokud jste vytvořili projekt ASP.NET Core mimo aplikace Service Fabric pomocí malé množství další kód tooregister hello služby pomocí modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="12148-125">Note that these are hello same choices that you would see if you created an ASP.NET Core project outside of a Service Fabric application, with a small amount of additional code tooregister hello service with hello Service Fabric runtime.</span></span> <span data-ttu-id="12148-126">V tomto kurzu zvolte **webového rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="12148-126">For this tutorial, choose **Web API**.</span></span> <span data-ttu-id="12148-127">Můžete však použít hello stejné koncepty toobuilding úplné webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="12148-127">However, you can apply hello same concepts toobuilding a full web application.</span></span>
   
    ![Výběr typu projektu ASP.NET][vs-new-aspnet-project-dialog]
   
    <span data-ttu-id="12148-129">Po vytvoření projektu webového rozhraní API byste měli mít dvě služby ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="12148-129">Once your Web API project is created, you should have two services in your application.</span></span> <span data-ttu-id="12148-130">Při dalším toobuild vaší aplikace, můžete přidat další služby v přesně hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="12148-130">As you continue toobuild your application, you can add more services in exactly hello same way.</span></span> <span data-ttu-id="12148-131">Každý může být nezávisle verzí a upgradovaný.</span><span class="sxs-lookup"><span data-stu-id="12148-131">Each can be independently versioned and upgraded.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="12148-132">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="12148-132">Run hello application</span></span>
<span data-ttu-id="12148-133">tooget představu o co jsme krok, můžeme nasadit hello novou aplikaci a proveďte poskytuje pohled na hello výchozí chování, které hello webového rozhraní API ASP.NET Core šablony.</span><span class="sxs-lookup"><span data-stu-id="12148-133">tooget a sense of what we've done, let's deploy hello new application and take a look at hello default behavior that hello ASP.NET Core Web API template provides.</span></span>

1. <span data-ttu-id="12148-134">V sadě Visual Studio toodebug hello aplikaci stisknutím klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="12148-134">Press F5 in Visual Studio toodebug hello app.</span></span>
2. <span data-ttu-id="12148-135">Po dokončení nasazení sady Visual Studio spustí prohlížeče toohello kořenovém hello služby ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="12148-135">When deployment is complete, Visual Studio launches a browser toohello root of hello ASP.NET Web API service.</span></span> <span data-ttu-id="12148-136">Šablona webového rozhraní API ASP.NET Core Hello neposkytuje výchozí chování pro kořenový adresář hello, proto byste měli vidět chybu 404 v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="12148-136">hello ASP.NET Core Web API template doesn't provide default behavior for hello root, so you should see a 404 error in hello browser.</span></span>
3. <span data-ttu-id="12148-137">Přidat `/api/values` toohello umístění v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="12148-137">Add `/api/values` toohello location in hello browser.</span></span> <span data-ttu-id="12148-138">Tím se spustí hello `Get` metodu hello ValuesController v šabloně hello webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="12148-138">This invokes hello `Get` method on hello ValuesController in hello Web API template.</span></span> <span data-ttu-id="12148-139">Vrátí hello výchozí odpovědi, které poskytuje šablony hello – pole JSON, který obsahuje dva řetězce:</span><span class="sxs-lookup"><span data-stu-id="12148-139">It returns hello default response that is provided by hello template--a JSON array that contains two strings:</span></span>
   
    ![Výchozí hodnot vrácených z webového rozhraní API ASP.NET Core šablony][browser-aspnet-template-values]
   
    <span data-ttu-id="12148-141">Hello konec hello kurzu zobrazí tato stránka hello nejnovější hodnotu čítače z našich stavové služby místo hello výchozí řetězce.</span><span class="sxs-lookup"><span data-stu-id="12148-141">By hello end of hello tutorial, this page will show hello most recent counter value from our stateful service instead of hello default strings.</span></span>

## <a name="connect-hello-services"></a><span data-ttu-id="12148-142">Připojení služby hello</span><span class="sxs-lookup"><span data-stu-id="12148-142">Connect hello services</span></span>
<span data-ttu-id="12148-143">Service Fabric nabízí flexibilitu v tom, jak komunikovat se službami reliable services.</span><span class="sxs-lookup"><span data-stu-id="12148-143">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="12148-144">V rámci jedné aplikace můžete mít služby, které jsou přístupné přes TCP, jiných služeb, které jsou přístupné přes rozhraní HTTP REST API a stále jiných služeb, které jsou přístupné přes webové sokety.</span><span class="sxs-lookup"><span data-stu-id="12148-144">Within a single application, you might have services that are accessible via TCP, other services that are accessible via an HTTP REST API, and still other services that are accessible via web sockets.</span></span> <span data-ttu-id="12148-145">Pro informace o dostupných možnostech hello a hello kompromisy související se situací, viz [komunikaci se službou](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="12148-145">For background on hello options available and hello tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span> <span data-ttu-id="12148-146">V tomto kurzu používáme [vzdálené komunikace služby Service Fabric](service-fabric-reliable-services-communication-remoting.md), zadaný v hello SDK.</span><span class="sxs-lookup"><span data-stu-id="12148-146">In this tutorial, we use [Service Fabric Service Remoting](service-fabric-reliable-services-communication-remoting.md), provided in hello SDK.</span></span>

<span data-ttu-id="12148-147">V hello (modelován na vzdálených volání procedur nebo vzdálených volání procedur) přístup vzdálené komunikace služby definujete rozhraní tooact jako hello veřejného kontraktu služby hello.</span><span class="sxs-lookup"><span data-stu-id="12148-147">In hello Service Remoting approach (modeled on remote procedure calls or RPCs), you define an interface tooact as hello public contract for hello service.</span></span> <span data-ttu-id="12148-148">Potom pomocí tohoto rozhraní toogenerate třídu proxy pro interakci s hello služby.</span><span class="sxs-lookup"><span data-stu-id="12148-148">Then, you use that interface toogenerate a proxy class for interacting with hello service.</span></span>

### <a name="create-hello-remoting-interface"></a><span data-ttu-id="12148-149">Vytvoření hello vzdálené komunikace rozhraní</span><span class="sxs-lookup"><span data-stu-id="12148-149">Create hello remoting interface</span></span>
<span data-ttu-id="12148-150">Začněme vytvořením hello rozhraní tooact jako hello smlouva mezi hello stavové služby a další služby v této případu hello webového projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12148-150">Let's start by creating hello interface tooact as hello contract between hello stateful service and other services, in this case hello ASP.NET Core web project.</span></span> <span data-ttu-id="12148-151">Všechny služby, které ho, vytvoříme ji v jeho vlastní projektu knihovny tříd používají toomake volání RPC, musí být sdílená toto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="12148-151">This interface must be shared by all services that use it toomake RPC calls, so we'll create it in its own Class Library project.</span></span>

1. <span data-ttu-id="12148-152">V Průzkumníku řešení klikněte pravým tlačítkem na řešení a zvolte **přidat** > **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="12148-152">In Solution Explorer, right-click your solution and choose **Add** > **New Project**.</span></span>

2. <span data-ttu-id="12148-153">Zvolte hello **Visual C#** položku v levé navigační podokně a pak vyberte hello hello **knihovny tříd** šablony.</span><span class="sxs-lookup"><span data-stu-id="12148-153">Choose hello **Visual C#** entry in hello left navigation pane and then select hello **Class Library** template.</span></span> <span data-ttu-id="12148-154">Ujistěte se, že verze rozhraní .NET Framework hello je nastaven příliš**4.5.2**.</span><span class="sxs-lookup"><span data-stu-id="12148-154">Ensure that hello .NET Framework version is set too**4.5.2**.</span></span>
   
    ![Vytvoření projektu rozhraní pro stavové služby][vs-add-class-library-project]

3. <span data-ttu-id="12148-156">Nainstalujte hello **Microsoft.ServiceFabric.Services.Remoting** balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="12148-156">Install hello **Microsoft.ServiceFabric.Services.Remoting** NuGet package.</span></span> <span data-ttu-id="12148-157">Vyhledejte **Microsoft.ServiceFabric.Services.Remoting** v hello NuGet balíček správce a nainstalujte ho pro všechny projekty v řešení hello, které používají vzdálené komunikace služby, včetně:</span><span class="sxs-lookup"><span data-stu-id="12148-157">Search for  **Microsoft.ServiceFabric.Services.Remoting** in hello NuGet package manager and install it for all projects in hello solution that use Service Remoting, including:</span></span>
   - <span data-ttu-id="12148-158">Hello knihovny tříd projekt, který obsahuje rozhraní služby hello</span><span class="sxs-lookup"><span data-stu-id="12148-158">hello Class Library project that contains hello service interface</span></span>
   - <span data-ttu-id="12148-159">Projekt stavové služby Hello</span><span class="sxs-lookup"><span data-stu-id="12148-159">hello Stateful Service project</span></span>
   - <span data-ttu-id="12148-160">Hello projektu webové služby ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12148-160">hello ASP.NET Core web service project</span></span>
   
    ![Přidává se balíček NuGet služby hello][vs-services-nuget-package]

4. <span data-ttu-id="12148-162">V knihovně tříd hello, vytvořit rozhraní s jedinou metodu `GetCountAsync`, a rozšířit hello rozhraní `Microsoft.ServiceFabric.Services.Remoting.IService`.</span><span class="sxs-lookup"><span data-stu-id="12148-162">In hello class library, create an interface with a single method, `GetCountAsync`, and extend hello interface from `Microsoft.ServiceFabric.Services.Remoting.IService`.</span></span> <span data-ttu-id="12148-163">Hello vzdálené komunikace rozhraní musí být odvozeny od tooindicate toto rozhraní je rozhraní, které je Vzdálená komunikace služby.</span><span class="sxs-lookup"><span data-stu-id="12148-163">hello remoting interface must derive from this interface tooindicate that it is a Service Remoting interface.</span></span>
   
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

### <a name="implement-hello-interface-in-your-stateful-service"></a><span data-ttu-id="12148-164">Implementovat rozhraní hello stavové služby</span><span class="sxs-lookup"><span data-stu-id="12148-164">Implement hello interface in your stateful service</span></span>
<span data-ttu-id="12148-165">Teď, když jsme definovali hello rozhraní, potřebujeme tooimplement v hello stavové služby.</span><span class="sxs-lookup"><span data-stu-id="12148-165">Now that we have defined hello interface, we need tooimplement it in hello stateful service.</span></span>

1. <span data-ttu-id="12148-166">Stavové služby přidejte projekt knihovny třída toohello odkaz, který obsahuje rozhraní hello.</span><span class="sxs-lookup"><span data-stu-id="12148-166">In your stateful service, add a reference toohello class library project that contains hello interface.</span></span>
   
    ![Přidání projektu knihovny tříd toohello odkaz v hello stavové služby][vs-add-class-library-reference]
2. <span data-ttu-id="12148-168">Vyhledejte hello třídu, která dědí z `StatefulService`, jako například `MyStatefulService`a rozšířit ji tooimplement hello `ICounter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="12148-168">Locate hello class that inherits from `StatefulService`, such as `MyStatefulService`, and extend it tooimplement hello `ICounter` interface.</span></span>
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. <span data-ttu-id="12148-169">Nyní implementovat hello jedné metody, která je definována v hello `ICounter` rozhraní `GetCountAsync`.</span><span class="sxs-lookup"><span data-stu-id="12148-169">Now implement hello single method that is defined in hello `ICounter` interface, `GetCountAsync`.</span></span>
   
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

### <a name="expose-hello-stateful-service-using-a-service-remoting-listener"></a><span data-ttu-id="12148-170">Vystavení hello stavové služby pomocí služby vzdálené komunikace naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="12148-170">Expose hello stateful service using a service remoting listener</span></span>
<span data-ttu-id="12148-171">S hello `ICounter` implementace rozhraní hello posledním krokem je tooopen hello vzdálené komunikace služby komunikační kanál.</span><span class="sxs-lookup"><span data-stu-id="12148-171">With hello `ICounter` interface implemented, hello final step is tooopen hello Service Remoting communication channel.</span></span> <span data-ttu-id="12148-172">Pro stavové služby, Service Fabric nabízí přepisovatelné metodu s názvem `CreateServiceReplicaListeners`.</span><span class="sxs-lookup"><span data-stu-id="12148-172">For stateful services, Service Fabric provides an overridable method called `CreateServiceReplicaListeners`.</span></span> <span data-ttu-id="12148-173">Pomocí této metody můžete určit jeden nebo více naslouchací procesy komunikace, na základě typu hello komunikace, že chcete tooenable pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="12148-173">With this method, you can specify one or more communication listeners, based on hello type of communication that you want tooenable for your service.</span></span>

> [!NOTE]
> <span data-ttu-id="12148-174">volání metody ekvivalentní Hello pro otevření komunikační kanál toostateless služby `CreateServiceInstanceListeners`.</span><span class="sxs-lookup"><span data-stu-id="12148-174">hello equivalent method for opening a communication channel toostateless services is called `CreateServiceInstanceListeners`.</span></span>

<span data-ttu-id="12148-175">V takovém případě jsme nahradit stávající hello `CreateServiceReplicaListeners` metoda a zadejte instanci `ServiceRemotingListener`, která vytvoří koncový bod protokolu RPC, lze volat z klientů prostřednictvím `ServiceProxy`.</span><span class="sxs-lookup"><span data-stu-id="12148-175">In this case, we replace hello existing `CreateServiceReplicaListeners` method and provide an instance of `ServiceRemotingListener`, which creates an RPC endpoint that is callable from clients through `ServiceProxy`.</span></span>  

<span data-ttu-id="12148-176">Hello `CreateServiceRemotingListener` rozšiřující metody na hello `IService` rozhraní vám umožní tooeasily vytvořit `ServiceRemotingListener` s všechna výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="12148-176">hello `CreateServiceRemotingListener` extension method on hello `IService` interface allows you tooeasily create a `ServiceRemotingListener` with all default settings.</span></span> <span data-ttu-id="12148-177">toouse této metody rozšíření, zajistěte, abyste měli hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` importovat obor názvů.</span><span class="sxs-lookup"><span data-stu-id="12148-177">toouse this extension method, ensure you have hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace imported.</span></span> 

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


### <a name="use-hello-serviceproxy-class-toointeract-with-hello-service"></a><span data-ttu-id="12148-178">Používat hello ServiceProxy třída toointeract službou hello</span><span class="sxs-lookup"><span data-stu-id="12148-178">Use hello ServiceProxy class toointeract with hello service</span></span>
<span data-ttu-id="12148-179">Naše stavové služby je nyní připraven tooreceive provoz z jiných služeb prostřednictvím protokolu RPC.</span><span class="sxs-lookup"><span data-stu-id="12148-179">Our stateful service is now ready tooreceive traffic from other services over RPC.</span></span> <span data-ttu-id="12148-180">Takže zůstane je přidání hello kód toocommunicate s ním z hello webovou službu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="12148-180">So all that remains is adding hello code toocommunicate with it from hello ASP.NET web service.</span></span>

1. <span data-ttu-id="12148-181">V projektu ASP.NET přidejte reference knihovny tříd toohello obsahující hello `ICounter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="12148-181">In your ASP.NET project, add a reference toohello class library that contains hello `ICounter` interface.</span></span>

2. <span data-ttu-id="12148-182">Dříve jste přidali hello **Microsoft.ServiceFabric.Services.Remoting** projekt ASP.NET toohello balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="12148-182">Earlier you added hello **Microsoft.ServiceFabric.Services.Remoting** NuGet package toohello ASP.NET project.</span></span> <span data-ttu-id="12148-183">Tento balíček poskytuje hello `ServiceProxy` třída, která používáte toomake RPC volá toohello stavové služby.</span><span class="sxs-lookup"><span data-stu-id="12148-183">This package provides hello `ServiceProxy` class which you use toomake RPC calls toohello stateful service.</span></span> <span data-ttu-id="12148-184">Zkontrolujte, zda že tento balíček NuGet je nainstalován v hello projektu webové služby ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12148-184">Ensure this NuGet package is installed in hello ASP.NET Core web service project.</span></span>

4. <span data-ttu-id="12148-185">V hello **řadiče** složku, otevřete hello `ValuesController` třídy.</span><span class="sxs-lookup"><span data-stu-id="12148-185">In hello **Controllers** folder, open hello `ValuesController` class.</span></span> <span data-ttu-id="12148-186">Všimněte si, že hello `Get` metoda aktuálně právě vrátí pole pevně řetězce "hodnota1" a "hodnota2"--který by odpovídal co jsme viděli dříve v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="12148-186">Note that hello `Get` method currently just returns a hard-coded string array of "value1" and "value2"--which matches what we saw earlier in hello browser.</span></span> <span data-ttu-id="12148-187">Tato implementace nahraďte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="12148-187">Replace this implementation with hello following code:</span></span>
   
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
   
    <span data-ttu-id="12148-188">první řádek kódu Hello je klíč hello jeden.</span><span class="sxs-lookup"><span data-stu-id="12148-188">hello first line of code is hello key one.</span></span> <span data-ttu-id="12148-189">toocreate hello ICounter proxy toohello stavové služby, budete potřebovat dva kusy tooprovide informace: název oddílu ID a hello hello služby.</span><span class="sxs-lookup"><span data-stu-id="12148-189">toocreate hello ICounter proxy toohello stateful service, you need tooprovide two pieces of information: a partition ID and hello name of hello service.</span></span>
   
    <span data-ttu-id="12148-190">Rozdělení tooscale stavové služby můžete použít porušením jejich stav do různých intervalů, na základě klíče, které definujete, jako je ID zákazníka nebo PSČ.</span><span class="sxs-lookup"><span data-stu-id="12148-190">You can use partitioning tooscale stateful services by breaking up their state into different buckets, based on a key that you define, such as a customer ID or postal code.</span></span> <span data-ttu-id="12148-191">V naší aplikaci trivial má hello stavové služby jenom jeden oddíl, takže hello klíč není důležité.</span><span class="sxs-lookup"><span data-stu-id="12148-191">In our trivial application, hello stateful service only has one partition, so hello key doesn't matter.</span></span> <span data-ttu-id="12148-192">Všechny klíče, který zadáte povede toohello stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="12148-192">Any key that you provide will lead toohello same partition.</span></span> <span data-ttu-id="12148-193">toolearn Další informace o vytváření oddílů služby, najdete v části [jak toopartition spolehlivé služby Service Fabric](service-fabric-concepts-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="12148-193">toolearn more about partitioning services, see [How toopartition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).</span></span>
   
    <span data-ttu-id="12148-194">název služby Hello je identifikátor URI hello formuláře fabric: /&lt;název_aplikace&gt;/&lt;service_name&gt;.</span><span class="sxs-lookup"><span data-stu-id="12148-194">hello service name is a URI of hello form fabric:/&lt;application_name&gt;/&lt;service_name&gt;.</span></span>
   
    <span data-ttu-id="12148-195">Pomocí těchto dvou položek příslušné Service Fabric jednoznačně identifikovat hello počítač, který by měl být odeslány požadavky.</span><span class="sxs-lookup"><span data-stu-id="12148-195">With these two pieces of information, Service Fabric can uniquely identify hello machine that requests should be sent to.</span></span> <span data-ttu-id="12148-196">Hello `ServiceProxy` třída také bezproblémově zpracovává hello případ, kdy selže hello počítač, který je hostitelem oddílu hello stavové služby a další počítač musí být propagovaných tootake jeho místo.</span><span class="sxs-lookup"><span data-stu-id="12148-196">hello `ServiceProxy` class also seamlessly handles hello case where hello machine that hosts hello stateful service partition fails and another machine must be promoted tootake its place.</span></span> <span data-ttu-id="12148-197">Tato abstrakce provede zápis hello toodeal kód klienta s jinými službami výrazně jednodušší.</span><span class="sxs-lookup"><span data-stu-id="12148-197">This abstraction makes writing hello client code toodeal with other services significantly simpler.</span></span>
   
    <span data-ttu-id="12148-198">Jakmile jsme hello proxy, můžeme jednoduše vyvolání hello `GetCountAsync` metodu a vrátí její výsledek.</span><span class="sxs-lookup"><span data-stu-id="12148-198">Once we have hello proxy, we simply invoke hello `GetCountAsync` method and return its result.</span></span>

5. <span data-ttu-id="12148-199">Stisknutím klávesy F5 znovu toorun hello upravit aplikace.</span><span class="sxs-lookup"><span data-stu-id="12148-199">Press F5 again toorun hello modified application.</span></span> <span data-ttu-id="12148-200">Jako dříve, Visual Studio se automaticky spustí hello prohlížeče toohello kořenovém hello webového projektu.</span><span class="sxs-lookup"><span data-stu-id="12148-200">As before, Visual Studio automatically launches hello browser toohello root of hello web project.</span></span> <span data-ttu-id="12148-201">Přidejte cestu "rozhraní api nebo hodnoty" hello, a měli byste vidět hello aktuální vrátila hodnotu čítače.</span><span class="sxs-lookup"><span data-stu-id="12148-201">Add hello "api/values" path, and you should see hello current counter value returned.</span></span>
   
    ![Hello stavová čítač hodnoty zobrazené v prohlížeči hello][browser-aspnet-counter-value]
   
    <span data-ttu-id="12148-203">Aktualizujte hello prohlížeče pravidelně toosee hello čítače hodnota aktualizace.</span><span class="sxs-lookup"><span data-stu-id="12148-203">Refresh hello browser periodically toosee hello counter value update.</span></span>

## <a name="kestrel-and-weblistener"></a><span data-ttu-id="12148-204">Kestrel a WebListener</span><span class="sxs-lookup"><span data-stu-id="12148-204">Kestrel and WebListener</span></span>

<span data-ttu-id="12148-205">Hello výchozí ASP.NET Core webový server, označuje jako Kestrel, je [není podporován pro zpracování přímé přenosy z Internetu](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="12148-205">hello default ASP.NET Core web server, known as Kestrel, is [not currently supported for handling direct internet traffic](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span></span> <span data-ttu-id="12148-206">V důsledku toho hello ASP.NET Core bezstavové služby šablona pro Service Fabric používá [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="12148-206">As a result, hello ASP.NET Core stateless service template for Service Fabric uses [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) by default.</span></span> 

<span data-ttu-id="12148-207">toolearn informace o Kestrel a WebListener v Service Fabric služeb, naleznete příliš[ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="12148-207">toolearn more about Kestrel and WebListener in Service Fabric services, please refer too[ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

## <a name="connecting-tooa-reliable-actor-service"></a><span data-ttu-id="12148-208">Připojení služby tooa spolehlivé objektu Actor</span><span class="sxs-lookup"><span data-stu-id="12148-208">Connecting tooa Reliable Actor service</span></span>
<span data-ttu-id="12148-209">Tento kurz se zaměřuje na přidání webový front-end, který oznamovat se stavovou službou.</span><span class="sxs-lookup"><span data-stu-id="12148-209">This tutorial focused on adding a web front end that communicated with a stateful service.</span></span> <span data-ttu-id="12148-210">Můžete však provést velmi podobné tooactors tootalk modelu.</span><span class="sxs-lookup"><span data-stu-id="12148-210">However, you can follow a very similar model tootalk tooactors.</span></span> <span data-ttu-id="12148-211">Při vytváření spolehlivé objektu Actor projektu sady Visual Studio automaticky generuje projektu rozhraní za vás.</span><span class="sxs-lookup"><span data-stu-id="12148-211">When you create a Reliable Actor project, Visual Studio automatically generates an interface project for you.</span></span> <span data-ttu-id="12148-212">Můžete použít tento rozhraní toogenerate proxy služby objektu actor v hello webového projektu toocommunicate s objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="12148-212">You can use that interface toogenerate an actor proxy in hello web project toocommunicate with hello actor.</span></span> <span data-ttu-id="12148-213">Hello komunikační kanál je zadán automaticky.</span><span class="sxs-lookup"><span data-stu-id="12148-213">hello communication channel is provided automatically.</span></span> <span data-ttu-id="12148-214">Proto není nutné toodo cokoli, který je ekvivalentní tooestablishing `ServiceRemotingListener` stejně, jako jste pro stavové služby hello v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="12148-214">So you do not need toodo anything that is equivalent tooestablishing a `ServiceRemotingListener` like you did for hello stateful service in this tutorial.</span></span>

## <a name="how-web-services-work-on-your-local-cluster"></a><span data-ttu-id="12148-215">Jak fungují webové služby v místním clusteru</span><span class="sxs-lookup"><span data-stu-id="12148-215">How web services work on your local cluster</span></span>
<span data-ttu-id="12148-216">Obecně platí můžete nasadit přesně hello stejné Service Fabric aplikace tooa více počítače, které nasazení clusteru v místním clusteru a být vysoce jistý, že funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="12148-216">In general, you can deploy exactly hello same Service Fabric application tooa multi-machine cluster that you deployed on your local cluster and be highly confident that it works as you expect.</span></span> <span data-ttu-id="12148-217">Je to proto, že místní cluster je jednoduše konfiguraci pěti uzly, který je sbalené tooa jeden počítač.</span><span class="sxs-lookup"><span data-stu-id="12148-217">This is because your local cluster is simply a five-node configuration that is collapsed tooa single machine.</span></span>

<span data-ttu-id="12148-218">Pokud jde tooweb služby, je však jeden nuance klíče.</span><span class="sxs-lookup"><span data-stu-id="12148-218">When it comes tooweb services, however, there is one key nuance.</span></span> <span data-ttu-id="12148-219">Při clusteru se nachází za službou Vyrovnávání zatížení, jako je tomu v Azure, musíte zajistit, aby webové služby jsou nasazeny na každý počítač od nástroj pro vyrovnávání zatížení hello jednoduše kruhové dotazování provoz mezi počítači hello.</span><span class="sxs-lookup"><span data-stu-id="12148-219">When your cluster sits behind a load balancer, as it does in Azure, you must ensure that your web services are deployed on every machine since hello load balancer simply round-robins traffic across hello machines.</span></span> <span data-ttu-id="12148-220">To provedete pomocí nastavení hello `InstanceCount` pro hello služby toohello speciální hodnotu-1.</span><span class="sxs-lookup"><span data-stu-id="12148-220">You can do this by setting hello `InstanceCount` for hello service toohello special value of "-1".</span></span>

<span data-ttu-id="12148-221">Naopak při spuštění webové služby místně, je nutné tooensure této pouze jednu instanci služby hello běží.</span><span class="sxs-lookup"><span data-stu-id="12148-221">By contrast, when you run a web service locally, you need tooensure that only one instance of hello service is running.</span></span> <span data-ttu-id="12148-222">V opačném spustíte do konfliktu z více procesů, které naslouchají na hello stejný port a cesta.</span><span class="sxs-lookup"><span data-stu-id="12148-222">Otherwise, you run into conflicts from multiple processes that are listening on hello same path and port.</span></span> <span data-ttu-id="12148-223">V důsledku toho počet instancí služby webové hello by mělo být nastavené příliš "1" pro místní nasazení.</span><span class="sxs-lookup"><span data-stu-id="12148-223">As a result, hello web service instance count should be set too"1" for local deployments.</span></span>

<span data-ttu-id="12148-224">jak tooconfigure různé hodnoty pro jiného prostředí, najdete v části toolearn [Správa parametry aplikace pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="12148-224">toolearn how tooconfigure different values for different environment, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="12148-225">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12148-225">Next steps</span></span>
<span data-ttu-id="12148-226">Teď, když máte webové přední skončili sadu pro vaši aplikaci s ASP.NET Core, další informace o [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md) pro podrobné pochopení fungování ASP.NET Core s Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="12148-226">Now that you have a web front end set up for your application with ASP.NET Core, learn more about [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) for an in-depth understanding of how ASP.NET Core works with Service Fabric.</span></span>

<span data-ttu-id="12148-227">Dále [Další informace o komunikaci se službou](service-fabric-connect-and-communicate-with-services.md) obecně tooget a dokončení obrázek o tom, jak služba funguje komunikace v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="12148-227">Next, [learn more about communicating with services](service-fabric-connect-and-communicate-with-services.md) in general tooget a complete picture of how service communication works in Service Fabric.</span></span>

<span data-ttu-id="12148-228">Až budete mít dostatečné povědomí o tom, jak funguje komunikace služby, [vytvořte cluster v Azure a nasadit své cloudové aplikace toohello](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="12148-228">Once you have a good understanding of how service communication works, [create a cluster in Azure and deploy your application toohello cloud](service-fabric-cluster-creation-via-portal.md).</span></span>

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
