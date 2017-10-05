---
title: "Klient a server správy verzí sady SDK v Mobile Apps a Mobile Services | Microsoft Docs"
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
ms.openlocfilehash: f79e819b1547f81498ea213858faf3c75e374782
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a><span data-ttu-id="7e499-103">Správa verzí klientských a serverových v Mobile Apps a Mobile Services</span><span class="sxs-lookup"><span data-stu-id="7e499-103">Client and server versioning in Mobile Apps and Mobile Services</span></span>
<span data-ttu-id="7e499-104">Nejnovější verzi Azure Mobile Services je **Mobile Apps** funkce Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7e499-104">The latest version of Azure Mobile Services is the **Mobile Apps** feature of Azure App Service.</span></span>

<span data-ttu-id="7e499-105">Sady SDK pro klienta a serveru Mobile Apps jsou původně založené na těch, které v Mobile Services, ale jsou *není* vzájemně kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="7e499-105">The Mobile Apps client and server SDKs are originally based on those in Mobile Services, but they are *not* compatible with each other.</span></span>
<span data-ttu-id="7e499-106">To znamená, že musí použít *Mobile Apps* klienta SDK s *Mobile Apps* serveru SDK a podobně pro *Mobile Services*.</span><span class="sxs-lookup"><span data-stu-id="7e499-106">That is, you must use a *Mobile Apps* client SDK with a *Mobile Apps* server SDK and similarly for *Mobile Services*.</span></span> <span data-ttu-id="7e499-107">Tato smlouva se vynucuje prostřednictvím hodnotu speciální hlavičky používané klientem a serverem sady SDK, `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="7e499-107">This contract is enforced through a special header value used by the client and server SDKs, `ZUMO-API-VERSION`.</span></span>

<span data-ttu-id="7e499-108">Poznámka: když tento dokument odkazuje *Mobile Services* back-end, nemusí nutně pro hostování v Mobile Services.</span><span class="sxs-lookup"><span data-stu-id="7e499-108">Note: whenever this document refers to a *Mobile Services* backend, it does not necessarily need to be hosted on Mobile Services.</span></span> <span data-ttu-id="7e499-109">Nyní je možné migrovat mobilních služeb ke spuštění v App Service beze změn kódu, ale stále používat službu *Mobile Services* verze sady SDK.</span><span class="sxs-lookup"><span data-stu-id="7e499-109">It is now possible to migrate a mobile service to run on App Service without any code changes, but the service would still be using *Mobile Services*  SDK versions.</span></span>

<span data-ttu-id="7e499-110">Další informace o migraci do služby App Service beze změn kódu, najdete v článku [migrace mobilní služby Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="7e499-110">To learn more about migrating to App Service without any code changes, see the article [Migrate a Mobile Service to Azure App Service].</span></span>

## <a name="header-specification"></a><span data-ttu-id="7e499-111">Specifikace záhlaví</span><span class="sxs-lookup"><span data-stu-id="7e499-111">Header specification</span></span>
<span data-ttu-id="7e499-112">Klíč `ZUMO-API-VERSION` může být určen v hlavičce HTTP nebo řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="7e499-112">The key `ZUMO-API-VERSION` may be specified in either the HTTP header or the query string.</span></span> <span data-ttu-id="7e499-113">Hodnota je řetězec verze ve formě **x.y.z**.</span><span class="sxs-lookup"><span data-stu-id="7e499-113">The value is a version string in the form **x.y.z**.</span></span>

<span data-ttu-id="7e499-114">Například:</span><span class="sxs-lookup"><span data-stu-id="7e499-114">For example:</span></span>

<span data-ttu-id="7e499-115">ZÍSKAT https://service.azurewebsites.net/tables/TodoItem</span><span class="sxs-lookup"><span data-stu-id="7e499-115">GET https://service.azurewebsites.net/tables/TodoItem</span></span>

<span data-ttu-id="7e499-116">HLAVIČKY: ZÁHLAVÍ ZUMO-API-VERSION: 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-116">HEADERS: ZUMO-API-VERSION: 2.0.0</span></span>

<span data-ttu-id="7e499-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span></span>

## <a name="opting-out-of-version-checking"></a><span data-ttu-id="7e499-118">Zrušení kontrolu verzí</span><span class="sxs-lookup"><span data-stu-id="7e499-118">Opting out of version checking</span></span>
<span data-ttu-id="7e499-119">Můžete vyjádření výslovného nesouhlasu kontroly nastavením hodnotu verze **true** pro nastavení aplikace **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="7e499-119">You can opt out of version checking by setting a value of **true** for the app setting **MS_SkipVersionCheck**.</span></span> <span data-ttu-id="7e499-120">Tuto verzi uveďte buď do souboru web.config, nebo v části Nastavení aplikace z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7e499-120">Specify this either in your web.config or in the Application Settings section of the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="7e499-121">Existuje několik změn v chování mezi Mobile Services a mobilní aplikace, zejména v oblasti ověřování, offline synchronizace a nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="7e499-121">There are a number of behavior changes between Mobile Services and Mobile Apps, particularly in the areas of offline sync, authentication, and push notifications.</span></span> <span data-ttu-id="7e499-122">Má jenom vyjádření výslovného nesouhlasu verze kontrola po dokončení testování pro zajištění, že tyto změny chování není rozdělit funkce vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7e499-122">You should only opt out of version checking after complete testing to ensure that these behavioral changes do not break your app's functionality.</span></span>
>
>

## <a name="summary-of-compatibility-for-all-versions"></a><span data-ttu-id="7e499-123">Souhrn kompatibility pro všechny verze</span><span class="sxs-lookup"><span data-stu-id="7e499-123">Summary of compatibility for all versions</span></span>
<span data-ttu-id="7e499-124">Následující graf zobrazuje kompatibilitu mezi všechny typy klienta a serveru.</span><span class="sxs-lookup"><span data-stu-id="7e499-124">The chart below shows the compatibility between all client and server types.</span></span> <span data-ttu-id="7e499-125">Back-end je jsou klasifikovány jako buď Mobile **služby** nebo Mobile **aplikace** založené na serveru SDK, která používá.</span><span class="sxs-lookup"><span data-stu-id="7e499-125">A backend is classified as either Mobile **Services** or Mobile **Apps** based on the server SDK that it uses.</span></span>

|  | <span data-ttu-id="7e499-126">**Mobilní služby** Node.js, nebo .NET</span><span class="sxs-lookup"><span data-stu-id="7e499-126">**Mobile Services** Node.js or .NET</span></span> | <span data-ttu-id="7e499-127">**Mobilní aplikace** Node.js, nebo .NET</span><span class="sxs-lookup"><span data-stu-id="7e499-127">**Mobile Apps** Node.js or .NET</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7e499-128">[Klienti Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="7e499-128">[Mobile Services clients]</span></span> |<span data-ttu-id="7e499-129">Ok</span><span class="sxs-lookup"><span data-stu-id="7e499-129">Ok</span></span> |<span data-ttu-id="7e499-130">Chyba\*</span><span class="sxs-lookup"><span data-stu-id="7e499-130">Error\*</span></span> |
| <span data-ttu-id="7e499-131">[Klienti Mobile Apps]</span><span class="sxs-lookup"><span data-stu-id="7e499-131">[Mobile Apps clients]</span></span> |<span data-ttu-id="7e499-132">Chyba\*</span><span class="sxs-lookup"><span data-stu-id="7e499-132">Error\*</span></span> |<span data-ttu-id="7e499-133">Ok</span><span class="sxs-lookup"><span data-stu-id="7e499-133">Ok</span></span> |

<span data-ttu-id="7e499-134">\*To se dá nastavit podle určení **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="7e499-134">\*This can be controlled by specifying **MS_SkipVersionCheck**.</span></span>

<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <span data-ttu-id="7e499-135"><a name="1.0.0"></a>Klientem Mobile Services a serveru</span><span class="sxs-lookup"><span data-stu-id="7e499-135"><a name="1.0.0"></a>Mobile Services client and server</span></span>
<span data-ttu-id="7e499-136">Klientské sady SDK v následující tabulce jsou kompatibilní s **Mobile Services**.</span><span class="sxs-lookup"><span data-stu-id="7e499-136">The client SDKs in the table below are compatible with **Mobile Services**.</span></span>

<span data-ttu-id="7e499-137">Poznámka: Mobile Services klientskou sadu SDK *nepodporují* odeslání hodnotu hlavičky pro `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="7e499-137">Note: the Mobile Services client SDKs *do not* send a header value for `ZUMO-API-VERSION`.</span></span> <span data-ttu-id="7e499-138">Pokud služba přijme této hlavičky nebo hodnotu řetězce dotazu, bude vrácena chyba, pokud jste explicitně zvolili odhlašování, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="7e499-138">If the service receives this header or query string value, an error will be returned, unless you have explicitly opted out as described above.</span></span>

### <span data-ttu-id="7e499-139"><a name="MobileServicesClients"></a>Mobilní *služby* klientskou sadu SDK</span><span class="sxs-lookup"><span data-stu-id="7e499-139"><a name="MobileServicesClients"></a> Mobile *Services* client SDKs</span></span>
| <span data-ttu-id="7e499-140">Klientské platformy</span><span class="sxs-lookup"><span data-stu-id="7e499-140">Client platform</span></span> | <span data-ttu-id="7e499-141">Verze</span><span class="sxs-lookup"><span data-stu-id="7e499-141">Version</span></span> | <span data-ttu-id="7e499-142">Hodnota záhlaví verze</span><span class="sxs-lookup"><span data-stu-id="7e499-142">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7e499-143">Spravované klientské (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="7e499-143">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="7e499-144">1.3.2</span><span class="sxs-lookup"><span data-stu-id="7e499-144">1.3.2</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |<span data-ttu-id="7e499-145">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7e499-145">n/a</span></span> |
| <span data-ttu-id="7e499-146">iOS</span><span class="sxs-lookup"><span data-stu-id="7e499-146">iOS</span></span> |[<span data-ttu-id="7e499-147">2.2.2</span><span class="sxs-lookup"><span data-stu-id="7e499-147">2.2.2</span></span>](http://aka.ms/gc6fex) |<span data-ttu-id="7e499-148">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7e499-148">n/a</span></span> |
| <span data-ttu-id="7e499-149">Android</span><span class="sxs-lookup"><span data-stu-id="7e499-149">Android</span></span> |[<span data-ttu-id="7e499-150">2.0.3</span><span class="sxs-lookup"><span data-stu-id="7e499-150">2.0.3</span></span>](https://go.microsoft.com/fwLink/?LinkID=280126) |<span data-ttu-id="7e499-151">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7e499-151">n/a</span></span> |
| <span data-ttu-id="7e499-152">HTML</span><span class="sxs-lookup"><span data-stu-id="7e499-152">HTML</span></span> |[<span data-ttu-id="7e499-153">1.2.7</span><span class="sxs-lookup"><span data-stu-id="7e499-153">1.2.7</span></span>](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |<span data-ttu-id="7e499-154">neuvedeno</span><span class="sxs-lookup"><span data-stu-id="7e499-154">n/a</span></span> |

### <a name="mobile-services-server-sdks"></a><span data-ttu-id="7e499-155">Mobilní *služby* server sady SDK</span><span class="sxs-lookup"><span data-stu-id="7e499-155">Mobile *Services* server SDKs</span></span>
| <span data-ttu-id="7e499-156">Platforma serveru</span><span class="sxs-lookup"><span data-stu-id="7e499-156">Server platform</span></span> | <span data-ttu-id="7e499-157">Verze</span><span class="sxs-lookup"><span data-stu-id="7e499-157">Version</span></span> | <span data-ttu-id="7e499-158">Přijatá verze hlavičky</span><span class="sxs-lookup"><span data-stu-id="7e499-158">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7e499-159">.NET</span><span class="sxs-lookup"><span data-stu-id="7e499-159">.NET</span></span> |[<span data-ttu-id="7e499-160">Verze WindowsAzure.MobileServices.Backend.* 1.0.x</span><span class="sxs-lookup"><span data-stu-id="7e499-160">WindowsAzure.MobileServices.Backend.* Version 1.0.x</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |<span data-ttu-id="7e499-161">** Žádné záhlaví verze **</span><span class="sxs-lookup"><span data-stu-id="7e499-161">**No version header **</span></span> |
| <span data-ttu-id="7e499-162">Node.js</span><span class="sxs-lookup"><span data-stu-id="7e499-162">Node.js</span></span> |<span data-ttu-id="7e499-163">(už brzy)</span><span class="sxs-lookup"><span data-stu-id="7e499-163">(coming soon)</span></span> |<span data-ttu-id="7e499-164">**Žádná hlavička verze**</span><span class="sxs-lookup"><span data-stu-id="7e499-164">**No version header**</span></span> |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a><span data-ttu-id="7e499-165">Chování back-EndY mobilní služby</span><span class="sxs-lookup"><span data-stu-id="7e499-165">Behavior of Mobile Services backends</span></span>
| <span data-ttu-id="7e499-166">ZÁHLAVÍ ZUMO-API-VERSION</span><span class="sxs-lookup"><span data-stu-id="7e499-166">ZUMO-API-VERSION</span></span> | <span data-ttu-id="7e499-167">Hodnota MS_SkipVersionCheck</span><span class="sxs-lookup"><span data-stu-id="7e499-167">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="7e499-168">Odpověď</span><span class="sxs-lookup"><span data-stu-id="7e499-168">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7e499-169">Není zadaná.</span><span class="sxs-lookup"><span data-stu-id="7e499-169">Not specified</span></span> |<span data-ttu-id="7e499-170">Všechny</span><span class="sxs-lookup"><span data-stu-id="7e499-170">Any</span></span> |<span data-ttu-id="7e499-171">200 – OK</span><span class="sxs-lookup"><span data-stu-id="7e499-171">200 - OK</span></span> |
| <span data-ttu-id="7e499-172">Libovolná hodnota</span><span class="sxs-lookup"><span data-stu-id="7e499-172">Any value</span></span> |<span data-ttu-id="7e499-173">True</span><span class="sxs-lookup"><span data-stu-id="7e499-173">True</span></span> |<span data-ttu-id="7e499-174">200 – OK</span><span class="sxs-lookup"><span data-stu-id="7e499-174">200 - OK</span></span> |
| <span data-ttu-id="7e499-175">Libovolná hodnota</span><span class="sxs-lookup"><span data-stu-id="7e499-175">Any value</span></span> |<span data-ttu-id="7e499-176">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7e499-176">False/Not Specified</span></span> |<span data-ttu-id="7e499-177">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="7e499-177">400 - Bad Request</span></span> |

## <span data-ttu-id="7e499-178"><a name="2.0.0"></a>Azure Mobile Apps klienta a serveru</span><span class="sxs-lookup"><span data-stu-id="7e499-178"><a name="2.0.0"></a>Azure Mobile Apps client and server</span></span>
### <span data-ttu-id="7e499-179"><a name="MobileAppsClients"></a>Mobilní *aplikace* klientskou sadu SDK</span><span class="sxs-lookup"><span data-stu-id="7e499-179"><a name="MobileAppsClients"></a> Mobile *Apps* client SDKs</span></span>
<span data-ttu-id="7e499-180">Kontrola verze byla zavedena spuštění v následujících verzích klienta SDK pro **Azure Mobile Apps**:</span><span class="sxs-lookup"><span data-stu-id="7e499-180">Version checking was introduced starting with the following versions of the client SDK for **Azure Mobile Apps**:</span></span>

| <span data-ttu-id="7e499-181">Klientské platformy</span><span class="sxs-lookup"><span data-stu-id="7e499-181">Client platform</span></span> | <span data-ttu-id="7e499-182">Verze</span><span class="sxs-lookup"><span data-stu-id="7e499-182">Version</span></span> | <span data-ttu-id="7e499-183">Hodnota záhlaví verze</span><span class="sxs-lookup"><span data-stu-id="7e499-183">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7e499-184">Spravované klientské (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="7e499-184">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="7e499-185">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-185">2.0.0</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |<span data-ttu-id="7e499-186">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-186">2.0.0</span></span> |
| <span data-ttu-id="7e499-187">iOS</span><span class="sxs-lookup"><span data-stu-id="7e499-187">iOS</span></span> |[<span data-ttu-id="7e499-188">3.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-188">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=529823) |<span data-ttu-id="7e499-189">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-189">2.0.0</span></span> |
| <span data-ttu-id="7e499-190">Android</span><span class="sxs-lookup"><span data-stu-id="7e499-190">Android</span></span> |[<span data-ttu-id="7e499-191">3.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-191">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |<span data-ttu-id="7e499-192">3.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-192">3.0.0</span></span> |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a><span data-ttu-id="7e499-193">Mobilní *aplikace* server sady SDK</span><span class="sxs-lookup"><span data-stu-id="7e499-193">Mobile *Apps* server SDKs</span></span>
<span data-ttu-id="7e499-194">Kontrola verze je součástí následující verze sady SDK serveru:</span><span class="sxs-lookup"><span data-stu-id="7e499-194">Version checking is included in following server SDK versions:</span></span>

| <span data-ttu-id="7e499-195">Platforma serveru</span><span class="sxs-lookup"><span data-stu-id="7e499-195">Server platform</span></span> | <span data-ttu-id="7e499-196">Sada SDK</span><span class="sxs-lookup"><span data-stu-id="7e499-196">SDK</span></span> | <span data-ttu-id="7e499-197">Přijatá verze hlavičky</span><span class="sxs-lookup"><span data-stu-id="7e499-197">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7e499-198">.NET</span><span class="sxs-lookup"><span data-stu-id="7e499-198">.NET</span></span> |[<span data-ttu-id="7e499-199">Microsoft.Azure.Mobile.Server</span><span class="sxs-lookup"><span data-stu-id="7e499-199">Microsoft.Azure.Mobile.Server</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |<span data-ttu-id="7e499-200">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-200">2.0.0</span></span> |
| <span data-ttu-id="7e499-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="7e499-201">Node.js</span></span> |[<span data-ttu-id="7e499-202">Azure mobile apps)</span><span class="sxs-lookup"><span data-stu-id="7e499-202">azure-mobile-apps)</span></span>](https://www.npmjs.com/package/azure-mobile-apps) |<span data-ttu-id="7e499-203">2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e499-203">2.0.0</span></span> |

### <a name="behavior-of-mobile-apps-backends"></a><span data-ttu-id="7e499-204">Chování back-EndY mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="7e499-204">Behavior of Mobile Apps backends</span></span>
| <span data-ttu-id="7e499-205">ZÁHLAVÍ ZUMO-API-VERSION</span><span class="sxs-lookup"><span data-stu-id="7e499-205">ZUMO-API-VERSION</span></span> | <span data-ttu-id="7e499-206">Hodnota MS_SkipVersionCheck</span><span class="sxs-lookup"><span data-stu-id="7e499-206">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="7e499-207">Odpověď</span><span class="sxs-lookup"><span data-stu-id="7e499-207">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7e499-208">x.y.z nebo hodnota Null.</span><span class="sxs-lookup"><span data-stu-id="7e499-208">x.y.z or Null</span></span> |<span data-ttu-id="7e499-209">True</span><span class="sxs-lookup"><span data-stu-id="7e499-209">True</span></span> |<span data-ttu-id="7e499-210">200 – OK</span><span class="sxs-lookup"><span data-stu-id="7e499-210">200 - OK</span></span> |
| <span data-ttu-id="7e499-211">Hodnotu Null</span><span class="sxs-lookup"><span data-stu-id="7e499-211">Null</span></span> |<span data-ttu-id="7e499-212">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7e499-212">False/Not Specified</span></span> |<span data-ttu-id="7e499-213">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="7e499-213">400 - Bad Request</span></span> |
| <span data-ttu-id="7e499-214">1.x.y</span><span class="sxs-lookup"><span data-stu-id="7e499-214">1.x.y</span></span> |<span data-ttu-id="7e499-215">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7e499-215">False/Not Specified</span></span> |<span data-ttu-id="7e499-216">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="7e499-216">400 - Bad Request</span></span> |
| <span data-ttu-id="7e499-217">2.0.0-2.x.y</span><span class="sxs-lookup"><span data-stu-id="7e499-217">2.0.0-2.x.y</span></span> |<span data-ttu-id="7e499-218">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7e499-218">False/Not Specified</span></span> |<span data-ttu-id="7e499-219">200 – OK</span><span class="sxs-lookup"><span data-stu-id="7e499-219">200 - OK</span></span> |
| <span data-ttu-id="7e499-220">3.0.0-3.x.y</span><span class="sxs-lookup"><span data-stu-id="7e499-220">3.0.0-3.x.y</span></span> |<span data-ttu-id="7e499-221">False nebo nebyla zadána</span><span class="sxs-lookup"><span data-stu-id="7e499-221">False/Not Specified</span></span> |<span data-ttu-id="7e499-222">400 – Chybný požadavek</span><span class="sxs-lookup"><span data-stu-id="7e499-222">400 - Bad Request</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7e499-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e499-223">Next Steps</span></span>
* <span data-ttu-id="7e499-224">[migrace mobilní služby Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="7e499-224">[Migrate a Mobile Service to Azure App Service]</span></span>

<span data-ttu-id="7e499-225">[Klienti Mobile Services]: #MobileServicesClients</span><span class="sxs-lookup"><span data-stu-id="7e499-225">[Mobile Services clients]: #MobileServicesClients</span></span>
<span data-ttu-id="7e499-226">[Klienti Mobile Apps]: #MobileAppsClients</span><span class="sxs-lookup"><span data-stu-id="7e499-226">[Mobile Apps clients]: #MobileAppsClients</span></span>


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
<span data-ttu-id="7e499-227">[migrace mobilní služby Azure App Service]: app-service-mobile-migrating-from-mobile-services.md</span><span class="sxs-lookup"><span data-stu-id="7e499-227">[Migrate a Mobile Service to Azure App Service]: app-service-mobile-migrating-from-mobile-services.md</span></span>
