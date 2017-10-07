---
title: "aaaClient a server pro správu verzí sady SDK v Mobile Apps a Mobile Services | Microsoft Docs"
description: "Seznam klientskou sadu SDK a kompatibilitu s verzí serveru SDK pro Mobile Services a Azure Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5874b7455ea407ca8c77fb1bd03d97d0767ebb47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a><span data-ttu-id="7de25-103">Správa verzí klientských a serverových v Mobile Apps a Mobile Services</span><span class="sxs-lookup"><span data-stu-id="7de25-103">Client and server versioning in Mobile Apps and Mobile Services</span></span>
<span data-ttu-id="7de25-104">nejnovější verzi Azure Mobile Services Hello je hello **Mobile Apps** funkce Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7de25-104">hello latest version of Azure Mobile Services is hello **Mobile Apps** feature of Azure App Service.</span></span>

<span data-ttu-id="7de25-105">Hello mobilní aplikace klienta a serveru SDK jsou původně založené na těch, které v Mobile Services, ale jsou *není* vzájemně kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="7de25-105">hello Mobile Apps client and server SDKs are originally based on those in Mobile Services, but they are *not* compatible with each other.</span></span>
<span data-ttu-id="7de25-106">To znamená, že musí použít *Mobile Apps* klienta SDK s *Mobile Apps* serveru SDK a podobně pro *Mobile Services*.</span><span class="sxs-lookup"><span data-stu-id="7de25-106">That is, you must use a *Mobile Apps* client SDK with a *Mobile Apps* server SDK and similarly for *Mobile Services*.</span></span> <span data-ttu-id="7de25-107">Tato smlouva se vynucuje prostřednictvím hodnotu speciální hlavičky používané hello klientských a serverových sad SDK, `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="7de25-107">This contract is enforced through a special header value used by hello client and server SDKs, `ZUMO-API-VERSION`.</span></span>

<span data-ttu-id="7de25-108">Poznámka: když tento dokument odkazuje tooa *Mobile Services* back-end, nemusí nutně toobe hostované na Mobile Services.</span><span class="sxs-lookup"><span data-stu-id="7de25-108">Note: whenever this document refers tooa *Mobile Services* backend, it does not necessarily need toobe hosted on Mobile Services.</span></span> <span data-ttu-id="7de25-109">Je teď možné toomigrate toorun mobilní službu v App Service beze změn kódu, ale stále používat služby hello *Mobile Services* verze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="7de25-109">It is now possible toomigrate a mobile service toorun on App Service without any code changes, but hello service would still be using *Mobile Services*  SDK versions.</span></span>

<span data-ttu-id="7de25-110">Další informace o toolearn migrace tooApp služby beze změn kódu, najdete v článku hello [migrovat tooAzure služby mobilní služby App Service].</span><span class="sxs-lookup"><span data-stu-id="7de25-110">toolearn more about migrating tooApp Service without any code changes, see hello article [Migrate a Mobile Service tooAzure App Service].</span></span>

## <a name="header-specification"></a><span data-ttu-id="7de25-111">Specifikace záhlaví</span><span class="sxs-lookup"><span data-stu-id="7de25-111">Header specification</span></span>
<span data-ttu-id="7de25-112">klíč Hello `ZUMO-API-VERSION` je možné zadat hello HTTP, hlavičku nebo řetězec dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="7de25-112">hello key `ZUMO-API-VERSION` may be specified in either hello HTTP header or hello query string.</span></span> <span data-ttu-id="7de25-113">Hello hodnota je řetězec verze ve formuláři hello **x.y.z**.</span><span class="sxs-lookup"><span data-stu-id="7de25-113">hello value is a version string in hello form **x.y.z**.</span></span>

<span data-ttu-id="7de25-114">Například:</span><span class="sxs-lookup"><span data-stu-id="7de25-114">For example:</span></span>

<span data-ttu-id="7de25-115">ZÍSKAT https://service.azurewebsites.net/tables/TodoItem</span><span class="sxs-lookup"><span data-stu-id="7de25-115">GET https://service.azurewebsites.net/tables/TodoItem</span></span>

<span data-ttu-id="7de25-116">HLAVIČKY: ZÁHLAVÍ ZUMO-API-VERSION: 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-116">HEADERS: ZUMO-API-VERSION: 2.0.0</span></span>

<span data-ttu-id="7de25-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span></span>

## <a name="opting-out-of-version-checking"></a><span data-ttu-id="7de25-118">Zrušení kontrolu verzí</span><span class="sxs-lookup"><span data-stu-id="7de25-118">Opting out of version checking</span></span>
<span data-ttu-id="7de25-119">Můžete vyjádření výslovného nesouhlasu kontroly nastavením hodnotu verze **true** pro nastavení aplikace hello **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="7de25-119">You can opt out of version checking by setting a value of **true** for hello app setting **MS_SkipVersionCheck**.</span></span> <span data-ttu-id="7de25-120">Tuto verzi uveďte v souboru web.config nebo v hello část nastavení aplikace hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7de25-120">Specify this either in your web.config or in hello Application Settings section of hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="7de25-121">Existuje několik změn v chování mezi Mobile Services a mobilní aplikace, zejména v oblasti hello ověřování, offline synchronizace a nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="7de25-121">There are a number of behavior changes between Mobile Services and Mobile Apps, particularly in hello areas of offline sync, authentication, and push notifications.</span></span> <span data-ttu-id="7de25-122">Má jenom vyjádření výslovného nesouhlasu s verze kontrola po dokončení testování tooensure tyto změny chování není rozdělit funkce vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7de25-122">You should only opt out of version checking after complete testing tooensure that these behavioral changes do not break your app's functionality.</span></span>
>
>

## <a name="summary-of-compatibility-for-all-versions"></a><span data-ttu-id="7de25-123">Souhrn kompatibility pro všechny verze</span><span class="sxs-lookup"><span data-stu-id="7de25-123">Summary of compatibility for all versions</span></span>
<span data-ttu-id="7de25-124">Graf Hello níže znázorňuje hello kompatibilitu mezi všechny typy klienta a serveru.</span><span class="sxs-lookup"><span data-stu-id="7de25-124">hello chart below shows hello compatibility between all client and server types.</span></span> <span data-ttu-id="7de25-125">Back-end je jsou klasifikovány jako buď Mobile **služby** nebo Mobile **aplikace** založené na serveru hello SDK, která používá.</span><span class="sxs-lookup"><span data-stu-id="7de25-125">A backend is classified as either Mobile **Services** or Mobile **Apps** based on hello server SDK that it uses.</span></span>

|  | <span data-ttu-id="7de25-126">**Mobilní služby** Node.js, nebo .NET</span><span class="sxs-lookup"><span data-stu-id="7de25-126">**Mobile Services** Node.js or .NET</span></span> | <span data-ttu-id="7de25-127">**Mobilní aplikace** Node.js, nebo .NET</span><span class="sxs-lookup"><span data-stu-id="7de25-127">**Mobile Apps** Node.js or .NET</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7de25-128">[Klienti Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="7de25-128">[Mobile Services clients]</span></span> |<span data-ttu-id="7de25-129">Ok</span><span class="sxs-lookup"><span data-stu-id="7de25-129">Ok</span></span> |<span data-ttu-id="7de25-130">Chyba\*</span><span class="sxs-lookup"><span data-stu-id="7de25-130">Error\*</span></span> |
| <span data-ttu-id="7de25-131">[Klienti Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="7de25-131">[Mobile Apps clients]</span></span> |<span data-ttu-id="7de25-132">Chyba\*</span><span class="sxs-lookup"><span data-stu-id="7de25-132">Error\*</span></span> |<span data-ttu-id="7de25-133">Ok</span><span class="sxs-lookup"><span data-stu-id="7de25-133">Ok</span></span> |

<span data-ttu-id="7de25-134">\*To se dá nastavit podle určení **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="7de25-134">\*This can be controlled by specifying **MS_SkipVersionCheck**.</span></span>

<!-- IMPORTANT!  hello anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: hello fwlink toothis document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <span data-ttu-id="7de25-135"><a name="1.0.0"></a>Klientem Mobile Services a serveru</span><span class="sxs-lookup"><span data-stu-id="7de25-135"><a name="1.0.0"></a>Mobile Services client and server</span></span>
<span data-ttu-id="7de25-136">Hello klienta sady SDK v tabulce hello jsou kompatibilní s **Mobile Services**.</span><span class="sxs-lookup"><span data-stu-id="7de25-136">hello client SDKs in hello table below are compatible with **Mobile Services**.</span></span>

<span data-ttu-id="7de25-137">Poznámka: hello klientské sady SDK Mobile Services *nepodporují* odeslání hodnotu hlavičky pro `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="7de25-137">Note: hello Mobile Services client SDKs *do not* send a header value for `ZUMO-API-VERSION`.</span></span> <span data-ttu-id="7de25-138">Pokud služba hello obdrží tato záhlaví nebo hodnotu řetězce dotazu, bude vrácena chyba, pokud jste explicitně zvolili odhlašování, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="7de25-138">If hello service receives this header or query string value, an error will be returned, unless you have explicitly opted out as described above.</span></span>

### <span data-ttu-id="7de25-139"><a name="MobileServicesClients"></a>Mobilní *služby* klientskou sadu SDK</span><span class="sxs-lookup"><span data-stu-id="7de25-139"><a name="MobileServicesClients"></a> Mobile *Services* client SDKs</span></span>
| <span data-ttu-id="7de25-140">Klientské platformy</span><span class="sxs-lookup"><span data-stu-id="7de25-140">Client platform</span></span> | <span data-ttu-id="7de25-141">Verze</span><span class="sxs-lookup"><span data-stu-id="7de25-141">Version</span></span> | <span data-ttu-id="7de25-142">Hodnota záhlaví verze</span><span class="sxs-lookup"><span data-stu-id="7de25-142">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7de25-143">Spravované klientské (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="7de25-143">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="7de25-144">1.3.2</span><span class="sxs-lookup"><span data-stu-id="7de25-144">1.3.2</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |<span data-ttu-id="7de25-145">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7de25-145">n/a</span></span> |
| <span data-ttu-id="7de25-146">iOS</span><span class="sxs-lookup"><span data-stu-id="7de25-146">iOS</span></span> |[<span data-ttu-id="7de25-147">2.2.2</span><span class="sxs-lookup"><span data-stu-id="7de25-147">2.2.2</span></span>](http://aka.ms/gc6fex) |<span data-ttu-id="7de25-148">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7de25-148">n/a</span></span> |
| <span data-ttu-id="7de25-149">Android</span><span class="sxs-lookup"><span data-stu-id="7de25-149">Android</span></span> |[<span data-ttu-id="7de25-150">2.0.3</span><span class="sxs-lookup"><span data-stu-id="7de25-150">2.0.3</span></span>](https://go.microsoft.com/fwLink/?LinkID=280126) |<span data-ttu-id="7de25-151">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7de25-151">n/a</span></span> |
| <span data-ttu-id="7de25-152">HTML</span><span class="sxs-lookup"><span data-stu-id="7de25-152">HTML</span></span> |[<span data-ttu-id="7de25-153">1.2.7</span><span class="sxs-lookup"><span data-stu-id="7de25-153">1.2.7</span></span>](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |<span data-ttu-id="7de25-154">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7de25-154">n/a</span></span> |

### <a name="mobile-services-server-sdks"></a><span data-ttu-id="7de25-155">Mobilní *služby* server sady SDK</span><span class="sxs-lookup"><span data-stu-id="7de25-155">Mobile *Services* server SDKs</span></span>
| <span data-ttu-id="7de25-156">Platforma serveru</span><span class="sxs-lookup"><span data-stu-id="7de25-156">Server platform</span></span> | <span data-ttu-id="7de25-157">Verze</span><span class="sxs-lookup"><span data-stu-id="7de25-157">Version</span></span> | <span data-ttu-id="7de25-158">Přijatá verze hlavičky</span><span class="sxs-lookup"><span data-stu-id="7de25-158">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7de25-159">.NET</span><span class="sxs-lookup"><span data-stu-id="7de25-159">.NET</span></span> |[<span data-ttu-id="7de25-160">Verze WindowsAzure.MobileServices.Backend.* 1.0.x</span><span class="sxs-lookup"><span data-stu-id="7de25-160">WindowsAzure.MobileServices.Backend.* Version 1.0.x</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |<span data-ttu-id="7de25-161">** Žádné záhlaví verze **</span><span class="sxs-lookup"><span data-stu-id="7de25-161">**No version header **</span></span> |
| <span data-ttu-id="7de25-162">Node.js</span><span class="sxs-lookup"><span data-stu-id="7de25-162">Node.js</span></span> |<span data-ttu-id="7de25-163">(už brzy)</span><span class="sxs-lookup"><span data-stu-id="7de25-163">(coming soon)</span></span> |<span data-ttu-id="7de25-164">**Žádná hlavička verze**</span><span class="sxs-lookup"><span data-stu-id="7de25-164">**No version header**</span></span> |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a><span data-ttu-id="7de25-165">Chování back-EndY mobilní služby</span><span class="sxs-lookup"><span data-stu-id="7de25-165">Behavior of Mobile Services backends</span></span>
| <span data-ttu-id="7de25-166">ZÁHLAVÍ ZUMO-API-VERSION</span><span class="sxs-lookup"><span data-stu-id="7de25-166">ZUMO-API-VERSION</span></span> | <span data-ttu-id="7de25-167">Hodnota MS_SkipVersionCheck</span><span class="sxs-lookup"><span data-stu-id="7de25-167">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="7de25-168">Odpověď</span><span class="sxs-lookup"><span data-stu-id="7de25-168">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7de25-169">Není zadaná.</span><span class="sxs-lookup"><span data-stu-id="7de25-169">Not specified</span></span> |<span data-ttu-id="7de25-170">Všechny</span><span class="sxs-lookup"><span data-stu-id="7de25-170">Any</span></span> |<span data-ttu-id="7de25-171">200 – OK</span><span class="sxs-lookup"><span data-stu-id="7de25-171">200 - OK</span></span> |
| <span data-ttu-id="7de25-172">Libovolná hodnota</span><span class="sxs-lookup"><span data-stu-id="7de25-172">Any value</span></span> |<span data-ttu-id="7de25-173">True</span><span class="sxs-lookup"><span data-stu-id="7de25-173">True</span></span> |<span data-ttu-id="7de25-174">200 – OK</span><span class="sxs-lookup"><span data-stu-id="7de25-174">200 - OK</span></span> |
| <span data-ttu-id="7de25-175">Libovolná hodnota</span><span class="sxs-lookup"><span data-stu-id="7de25-175">Any value</span></span> |<span data-ttu-id="7de25-176">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7de25-176">False/Not Specified</span></span> |<span data-ttu-id="7de25-177">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="7de25-177">400 - Bad Request</span></span> |

## <span data-ttu-id="7de25-178"><a name="2.0.0"></a>Azure Mobile Apps klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="7de25-178"><a name="2.0.0"></a>Azure Mobile Apps client and server</span></span>
### <span data-ttu-id="7de25-179"><a name="MobileAppsClients"></a>Mobilní *aplikace* klientskou sadu SDK</span><span class="sxs-lookup"><span data-stu-id="7de25-179"><a name="MobileAppsClients"></a> Mobile *Apps* client SDKs</span></span>
<span data-ttu-id="7de25-180">Kontrola verze byla zavedena počínaje hello následující verzí hello klienta SDK pro **Azure Mobile Apps**:</span><span class="sxs-lookup"><span data-stu-id="7de25-180">Version checking was introduced starting with hello following versions of hello client SDK for **Azure Mobile Apps**:</span></span>

| <span data-ttu-id="7de25-181">Klientské platformy</span><span class="sxs-lookup"><span data-stu-id="7de25-181">Client platform</span></span> | <span data-ttu-id="7de25-182">Verze</span><span class="sxs-lookup"><span data-stu-id="7de25-182">Version</span></span> | <span data-ttu-id="7de25-183">Hodnota záhlaví verze</span><span class="sxs-lookup"><span data-stu-id="7de25-183">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7de25-184">Spravované klientské (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="7de25-184">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="7de25-185">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-185">2.0.0</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |<span data-ttu-id="7de25-186">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-186">2.0.0</span></span> |
| <span data-ttu-id="7de25-187">iOS</span><span class="sxs-lookup"><span data-stu-id="7de25-187">iOS</span></span> |[<span data-ttu-id="7de25-188">3.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-188">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=529823) |<span data-ttu-id="7de25-189">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-189">2.0.0</span></span> |
| <span data-ttu-id="7de25-190">Android</span><span class="sxs-lookup"><span data-stu-id="7de25-190">Android</span></span> |[<span data-ttu-id="7de25-191">3.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-191">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |<span data-ttu-id="7de25-192">3.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-192">3.0.0</span></span> |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a><span data-ttu-id="7de25-193">Mobilní *aplikace* server sady SDK</span><span class="sxs-lookup"><span data-stu-id="7de25-193">Mobile *Apps* server SDKs</span></span>
<span data-ttu-id="7de25-194">Kontrola verze je součástí následující verze sady SDK serveru:</span><span class="sxs-lookup"><span data-stu-id="7de25-194">Version checking is included in following server SDK versions:</span></span>

| <span data-ttu-id="7de25-195">Platforma serveru</span><span class="sxs-lookup"><span data-stu-id="7de25-195">Server platform</span></span> | <span data-ttu-id="7de25-196">Sada SDK</span><span class="sxs-lookup"><span data-stu-id="7de25-196">SDK</span></span> | <span data-ttu-id="7de25-197">Přijatá verze hlavičky</span><span class="sxs-lookup"><span data-stu-id="7de25-197">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7de25-198">.NET</span><span class="sxs-lookup"><span data-stu-id="7de25-198">.NET</span></span> |[<span data-ttu-id="7de25-199">Microsoft.Azure.Mobile.Server</span><span class="sxs-lookup"><span data-stu-id="7de25-199">Microsoft.Azure.Mobile.Server</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |<span data-ttu-id="7de25-200">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-200">2.0.0</span></span> |
| <span data-ttu-id="7de25-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="7de25-201">Node.js</span></span> |[<span data-ttu-id="7de25-202">Azure mobile apps)</span><span class="sxs-lookup"><span data-stu-id="7de25-202">azure-mobile-apps)</span></span>](https://www.npmjs.com/package/azure-mobile-apps) |<span data-ttu-id="7de25-203">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7de25-203">2.0.0</span></span> |

### <a name="behavior-of-mobile-apps-backends"></a><span data-ttu-id="7de25-204">Chování back-EndY mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="7de25-204">Behavior of Mobile Apps backends</span></span>
| <span data-ttu-id="7de25-205">ZÁHLAVÍ ZUMO-API-VERSION</span><span class="sxs-lookup"><span data-stu-id="7de25-205">ZUMO-API-VERSION</span></span> | <span data-ttu-id="7de25-206">Hodnota MS_SkipVersionCheck</span><span class="sxs-lookup"><span data-stu-id="7de25-206">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="7de25-207">Odpověď</span><span class="sxs-lookup"><span data-stu-id="7de25-207">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7de25-208">x.y.z nebo hodnota Null.</span><span class="sxs-lookup"><span data-stu-id="7de25-208">x.y.z or Null</span></span> |<span data-ttu-id="7de25-209">True</span><span class="sxs-lookup"><span data-stu-id="7de25-209">True</span></span> |<span data-ttu-id="7de25-210">200 – OK</span><span class="sxs-lookup"><span data-stu-id="7de25-210">200 - OK</span></span> |
| <span data-ttu-id="7de25-211">Hodnotu Null</span><span class="sxs-lookup"><span data-stu-id="7de25-211">Null</span></span> |<span data-ttu-id="7de25-212">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7de25-212">False/Not Specified</span></span> |<span data-ttu-id="7de25-213">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="7de25-213">400 - Bad Request</span></span> |
| <span data-ttu-id="7de25-214">1.x.y</span><span class="sxs-lookup"><span data-stu-id="7de25-214">1.x.y</span></span> |<span data-ttu-id="7de25-215">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7de25-215">False/Not Specified</span></span> |<span data-ttu-id="7de25-216">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="7de25-216">400 - Bad Request</span></span> |
| <span data-ttu-id="7de25-217">2.0.0-2.x.y</span><span class="sxs-lookup"><span data-stu-id="7de25-217">2.0.0-2.x.y</span></span> |<span data-ttu-id="7de25-218">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7de25-218">False/Not Specified</span></span> |<span data-ttu-id="7de25-219">200 – OK</span><span class="sxs-lookup"><span data-stu-id="7de25-219">200 - OK</span></span> |
| <span data-ttu-id="7de25-220">3.0.0-3.x.y</span><span class="sxs-lookup"><span data-stu-id="7de25-220">3.0.0-3.x.y</span></span> |<span data-ttu-id="7de25-221">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7de25-221">False/Not Specified</span></span> |<span data-ttu-id="7de25-222">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="7de25-222">400 - Bad Request</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7de25-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7de25-223">Next Steps</span></span>
* <span data-ttu-id="7de25-224">[migrovat tooAzure služby mobilní služby App Service]</span><span class="sxs-lookup"><span data-stu-id="7de25-224">[Migrate a Mobile Service tooAzure App Service]</span></span>

[Klienti Mobile Services]: #MobileServicesClients
[Klienti Mobile Apps]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[migrovat tooAzure služby mobilní služby App Service]: app-service-mobile-migrating-from-mobile-services.md
