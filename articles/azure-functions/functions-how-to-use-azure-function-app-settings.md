---
title: "aaaConfigure nastavení aplikace funkce Azure | Microsoft Docs"
description: "Zjistěte, jak funkce tooconfigure Azure nastavení aplikace."
services: 
documentationcenter: .net
author: rachelappel
manager: erikre
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: glenga
ms.openlocfilehash: 539e203ac449061ef3ceae5e93df3bdbb326e43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-a-function-app-in-hello-azure-portal"></a><span data-ttu-id="66ba1-103">Jak toomanage funkce aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="66ba1-103">How toomanage a function app in hello Azure portal</span></span> 

<span data-ttu-id="66ba1-104">V Azure Functions poskytuje funkce aplikace hello kontext spuštění pro jednotlivé funkce.</span><span class="sxs-lookup"><span data-stu-id="66ba1-104">In Azure Functions, a function app provides hello execution context for your individual functions.</span></span> <span data-ttu-id="66ba1-105">Chování funkce aplikaci použít funkce tooall hostované danou funkci aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba1-105">Function app behaviors apply tooall functions hosted by a given function app.</span></span> <span data-ttu-id="66ba1-106">Toto téma popisuje, jak tooconfigure a správě aplikací funkce v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="66ba1-106">This topic describes how tooconfigure and manage your function apps in hello Azure portal.</span></span>

<span data-ttu-id="66ba1-107">toobegin, přejděte toohello [portál Azure](http://portal.azure.com) a přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="66ba1-107">toobegin, go toohello [Azure portal](http://portal.azure.com) and sign in tooyour Azure account.</span></span> <span data-ttu-id="66ba1-108">V panelu vyhledávání hello hello horní části portálu hello zadejte název hello funkce aplikace a vyberte ji ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="66ba1-108">In hello search bar at hello top of hello portal, type hello name of your function app and select it from hello list.</span></span> <span data-ttu-id="66ba1-109">Po výběru funkce aplikace, najdete v části hello následující stránky:</span><span class="sxs-lookup"><span data-stu-id="66ba1-109">After selecting your function app, you see hello following page:</span></span>

![Přehled funkce aplikace v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <span data-ttu-id="66ba1-111"><a name="manage-app-service-settings"></a>Karta nastavení aplikace – funkce</span><span class="sxs-lookup"><span data-stu-id="66ba1-111"><a name="manage-app-service-settings"></a>Function app settings tab</span></span>

![Přehled funkce aplikace v hello portálu Azure.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

<span data-ttu-id="66ba1-113">Hello **nastavení** karta je, kde můžete aktualizovat verzi modulu runtime funkce hello používá funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba1-113">hello **Settings** tab is where you can update hello Functions runtime version used by your function app.</span></span> <span data-ttu-id="66ba1-114">Je také kde budete spravovat hello hostitele klíčů používaných toorestrict HTTP přístup tooall funkce hostované hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba1-114">It is also where you manage hello host keys used toorestrict HTTP access tooall functions hosted by hello function app.</span></span>

<span data-ttu-id="66ba1-115">Funkce podporuje spotřeba hostování a hostování plány služby App Service.</span><span class="sxs-lookup"><span data-stu-id="66ba1-115">Functions supports both Consumption hosting and App Service hosting plans.</span></span> <span data-ttu-id="66ba1-116">Další informace najdete v tématu [zvolte hello správné služby plán pro Azure Functions](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="66ba1-116">For more information, see [Choose hello correct service plan for Azure Functions](functions-scale.md).</span></span> <span data-ttu-id="66ba1-117">Pro lepší předvídatelnost v plánu spotřeby hello funkce vám umožní omezit využití platformy a to nastavením denní kvóty využití, v GB sekundách.</span><span class="sxs-lookup"><span data-stu-id="66ba1-117">For better predictability in hello Consumption plan, Functions lets you limit platform usage by setting a daily usage quota, in gigabytes-seconds.</span></span> <span data-ttu-id="66ba1-118">Jakmile bude dosaženo kvóty využití denní hello, hello funkce aplikace je zastavena.</span><span class="sxs-lookup"><span data-stu-id="66ba1-118">Once hello daily usage quota is reached, hello function app is stopped.</span></span> <span data-ttu-id="66ba1-119">Aplikaci funkce zastavit v důsledku dosažení hello výdaje kvóty může být znovu zapnout z hello stejné oblasti jako zřízení hello denně výdaje kvóty.</span><span class="sxs-lookup"><span data-stu-id="66ba1-119">A function app stopped as a result of reaching hello spending quota can be re-enabled from hello same context as establishing hello daily spending quota.</span></span> <span data-ttu-id="66ba1-120">V tématu hello [Azure Functions stránce s cenami](http://azure.microsoft.com/pricing/details/functions/) podrobnosti o fakturaci.</span><span class="sxs-lookup"><span data-stu-id="66ba1-120">See hello [Azure Functions pricing page](http://azure.microsoft.com/pricing/details/functions/) for details on billing.</span></span>   

## <a name="platform-features-tab"></a><span data-ttu-id="66ba1-121">Karta funkcí platformy</span><span class="sxs-lookup"><span data-stu-id="66ba1-121">Platform features tab</span></span>

![Karta funkcí platformy funkce aplikace.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

<span data-ttu-id="66ba1-123">Funkce aplikace spustit a udržované pomocí hello platformě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="66ba1-123">Function apps run in, and are maintained, by hello Azure App Service platform.</span></span> <span data-ttu-id="66ba1-124">Funkce aplikací mít jako takový přístup toomost hello funkce systému Azure základní webového hostingu platformy.</span><span class="sxs-lookup"><span data-stu-id="66ba1-124">As such, your function apps have access toomost of hello features of Azure's core web hosting platform.</span></span> <span data-ttu-id="66ba1-125">Hello **funkce** karta je, kde přístup hello mnoho funkcí hello platformě App Service, můžete použít ve svých aplikacích funkce.</span><span class="sxs-lookup"><span data-stu-id="66ba1-125">hello **Platform features** tab is where you access hello many features of hello App Service platform that you can use in your function apps.</span></span> 

> [!NOTE]
> <span data-ttu-id="66ba1-126">Ne všechny funkce služby App Service jsou k dispozici při spuštění aplikace funkce na hostování plánu spotřeby hello.</span><span class="sxs-lookup"><span data-stu-id="66ba1-126">Not all App Service features are available when a function app runs on hello Consumption hosting plan.</span></span>

<span data-ttu-id="66ba1-127">Hello zbývající část tohoto tématu se zaměřuje na hello následující funkce služby App Service v hello portálu Azure, které jsou užitečné pro funkce:</span><span class="sxs-lookup"><span data-stu-id="66ba1-127">hello rest of this topic focuses on hello following App Service features in hello Azure portal that are useful for Functions:</span></span>

+ [<span data-ttu-id="66ba1-128">Editor služby App Service</span><span class="sxs-lookup"><span data-stu-id="66ba1-128">App Service editor</span></span>](#editor)
+ [<span data-ttu-id="66ba1-129">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="66ba1-129">Application settings</span></span>](#settings) 
+ [<span data-ttu-id="66ba1-130">Console</span><span class="sxs-lookup"><span data-stu-id="66ba1-130">Console</span></span>](#console)
+ [<span data-ttu-id="66ba1-131">Pokročilé nástroje (Kudu)</span><span class="sxs-lookup"><span data-stu-id="66ba1-131">Advanced tools (Kudu)</span></span>](#kudu)
+ [<span data-ttu-id="66ba1-132">Možnosti nasazení</span><span class="sxs-lookup"><span data-stu-id="66ba1-132">Deployment options</span></span>](#deployment)
+ [<span data-ttu-id="66ba1-133">CORS</span><span class="sxs-lookup"><span data-stu-id="66ba1-133">CORS</span></span>](#cors)
+ [<span data-ttu-id="66ba1-134">Ověřování</span><span class="sxs-lookup"><span data-stu-id="66ba1-134">Authentication</span></span>](#auth)
+ [<span data-ttu-id="66ba1-135">Definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="66ba1-135">API definition</span></span>](#swagger)

<span data-ttu-id="66ba1-136">Další informace o tom, najdete v části toowork s nastavením služby App Service, [nakonfigurovat nastavení aplikace služby Azure](../app-service-web/web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="66ba1-136">For more information about how toowork with App Service settings, see [Configure Azure App Service Settings](../app-service-web/web-sites-configure.md).</span></span>

### <span data-ttu-id="66ba1-137"><a name="editor"></a>Editor služby aplikace</span><span class="sxs-lookup"><span data-stu-id="66ba1-137"><a name="editor"></a>App Service Editor</span></span>

| | |
|-|-|
| ![Funkce aplikace služby App Service editor.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | <span data-ttu-id="66ba1-139">Hello editor služby App Service je editoru pokročilé v portálu, můžete použít toomodify JSON konfigurační soubory a soubory kódu agentem.</span><span class="sxs-lookup"><span data-stu-id="66ba1-139">hello App Service editor is an advanced in-portal editor that you can use toomodify JSON configuration files and code files alike.</span></span> <span data-ttu-id="66ba1-140">Výběrem této možnosti se spustí na záložce prohlížeče samostatné s základní editor.</span><span class="sxs-lookup"><span data-stu-id="66ba1-140">Choosing this option launches a separate browser tab with a basic editor.</span></span> <span data-ttu-id="66ba1-141">To vám umožní toointegrate s hello Git úložiště, spuštění a ladění kódu a upravit nastavení funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba1-141">This enables you toointegrate with hello Git repository, run and debug code, and modify function app settings.</span></span> <span data-ttu-id="66ba1-142">Tohoto editoru poskytuje vylepšené vývojářské prostředí pro funkce ve srovnání s okně hello výchozí funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba1-142">This editor provides an enhanced development environment for your functions compared with hello default function app blade.</span></span>    |

![Hello editor služby App Service](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <span data-ttu-id="66ba1-144"><a name="settings"></a>Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="66ba1-144"><a name="settings"></a>Application settings</span></span>

| | |
|-|-|
| ![Nastavení aplikace funkce aplikace.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | <span data-ttu-id="66ba1-146">Hello služby App Service **nastavení aplikace** okno je, kde můžete konfigurovat a spravovat framework verze, vzdálené ladění, nastavení aplikace a připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="66ba1-146">hello App Service **Application settings** blade is where you configure and manage framework versions, remote debugging, app settings, and connection strings.</span></span> <span data-ttu-id="66ba1-147">Při integraci s jinými Azure a služby třetích stran, funkce aplikace, můžete upravit tato nastavení zde.</span><span class="sxs-lookup"><span data-stu-id="66ba1-147">When you integrate your function app with other Azure and third-party services, you can modify those settings here.</span></span> |

![Konfigurace nastavení aplikace](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <span data-ttu-id="66ba1-149"><a name="console"></a>Konzola</span><span class="sxs-lookup"><span data-stu-id="66ba1-149"><a name="console"></a>Console</span></span>

| | |
|-|-|
| ![Funkce aplikace konzoly v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | <span data-ttu-id="66ba1-151">Hello v portálu konzoly je ideální vývojáře nástroj Pokud dáváte přednost toointeract s vaší aplikací funkce z příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="66ba1-151">hello in-portal console is an ideal developer tool when you prefer toointeract with your function app from hello command line.</span></span> <span data-ttu-id="66ba1-152">Běžné příkazy zahrnují adresáře a vytváření souborů a navigace, a také provádění dávkové soubory a skripty.</span><span class="sxs-lookup"><span data-stu-id="66ba1-152">Common commands include directory and file creation and navigation, as well as executing batch files and scripts.</span></span> |

![Funkce aplikace konzoly](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <span data-ttu-id="66ba1-154"><a name="kudu"></a>Pokročilé nástroje (Kudu)</span><span class="sxs-lookup"><span data-stu-id="66ba1-154"><a name="kudu"></a>Advanced tools (Kudu)</span></span>

| | |
|-|-|
| ![Funkce aplikace Kudu v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | <span data-ttu-id="66ba1-156">Hello Rozšířené nástroje pro službu App Service (také označované jako Kudu) poskytovat přístup k funkcím pro správu tooadvanced funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="66ba1-156">hello advanced tools for App Service (also known as Kudu) provide access tooadvanced administrative features of your function app.</span></span> <span data-ttu-id="66ba1-157">Z modulu Kudu spravovat informace o systému, nastavení aplikace, proměnné prostředí, rozšíření lokality, hlaviček protokolu HTTP a proměnných serveru.</span><span class="sxs-lookup"><span data-stu-id="66ba1-157">From Kudu, you manage system information, app settings, environment variables, site extensions, HTTP headers, and server variables.</span></span> <span data-ttu-id="66ba1-158">Můžete také spustit **Kudu** procházením toohello SCM koncový bod pro funkce aplikace, jako je třeba`https://<myfunctionapp>.scm.azurewebsites.net/`</span><span class="sxs-lookup"><span data-stu-id="66ba1-158">You can also launch **Kudu** by browsing toohello SCM endpoint for your function app, like `https://<myfunctionapp>.scm.azurewebsites.net/`</span></span> |

![Konfigurace modulu Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><span data-ttu-id="66ba1-160"><a name="deployment">Možnosti nasazení</span><span class="sxs-lookup"><span data-stu-id="66ba1-160"><a name="deployment">Deployment options</span></span>

| | |
|-|-|
| ![Možnosti nasazení funkce aplikace v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | <span data-ttu-id="66ba1-162">Funkce vám umožní vyvíjet funkce kódu na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="66ba1-162">Functions lets you develop your function code on your local machine.</span></span> <span data-ttu-id="66ba1-163">Potom můžete nahrát tooAzure projektu aplikace vaše místní funkce.</span><span class="sxs-lookup"><span data-stu-id="66ba1-163">You can then upload your local function app project tooAzure.</span></span> <span data-ttu-id="66ba1-164">Kromě toho tootraditional odeslání na server FTP, funkce umožňuje nasadit funkce aplikace pomocí Oblíbené průběžnou integraci řešení, jako jsou Githubu, služby VSTS, Dropbox, Bitbucket a dalších.</span><span class="sxs-lookup"><span data-stu-id="66ba1-164">In addition tootraditional FTP upload, Functions lets you deploy your function app using popular continuous integration solutions, like GitHub, VSTS, Dropbox, Bitbucket, and others.</span></span> <span data-ttu-id="66ba1-165">Další informace najdete v tématu [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="66ba1-165">For more information, see [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span> <span data-ttu-id="66ba1-166">tooupload ručně pomocí protokol FTP nebo místní Git, musíte také [nakonfigurovat přihlašovací údaje nasazení](functions-continuous-deployment.md#credentials).</span><span class="sxs-lookup"><span data-stu-id="66ba1-166">tooupload manually using FTP or local Git, you also must [configure your deployment credentials](functions-continuous-deployment.md#credentials).</span></span> |


### <span data-ttu-id="66ba1-167"><a name="cors"></a>CORS</span><span class="sxs-lookup"><span data-stu-id="66ba1-167"><a name="cors"></a>CORS</span></span>

| | |
|-|-|
| ![Funkce aplikace CORS v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | <span data-ttu-id="66ba1-169">spuštění škodlivého kódu tooprevent v službách, služby App Service bloky volá tooyour funkce aplikací z externích zdrojů.</span><span class="sxs-lookup"><span data-stu-id="66ba1-169">tooprevent malicious code execution in your services, App Service blocks calls tooyour function apps from external sources.</span></span> <span data-ttu-id="66ba1-170">Funkce podporuje toolet (CORS), které definujete "seznamem povolných" povolené zdroje, ze kterých může přijmout funkce vzdálené žádosti pro sdílení prostředků různého původu.</span><span class="sxs-lookup"><span data-stu-id="66ba1-170">Functions supports cross-origin resource sharing (CORS) toolet you define a "whitelist" of allowed origins from which your functions can accept remote requests.</span></span>  |

![Konfigurace funkce aplikace CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <span data-ttu-id="66ba1-172"><a name="auth"></a>Ověřování</span><span class="sxs-lookup"><span data-stu-id="66ba1-172"><a name="auth"></a>Authentication</span></span>

| | |
|-|-|
| ![Funkce ověřování aplikace v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | <span data-ttu-id="66ba1-174">Když funkce používají aktivační procedury protokolu HTTP, můžete vyžadovat volání toofirst ověřit.</span><span class="sxs-lookup"><span data-stu-id="66ba1-174">When functions use an HTTP trigger, you can require calls toofirst be authenticated.</span></span> <span data-ttu-id="66ba1-175">App Service podporuje ověřování Azure Active Directory a přihlaste se pomocí sociálních sítí, jako je Facebook, Microsoft a Twitter.</span><span class="sxs-lookup"><span data-stu-id="66ba1-175">App Service supports Azure Active Directory authentication and sign in with social providers, such as Facebook, Microsoft, and Twitter.</span></span> <span data-ttu-id="66ba1-176">Podrobnosti o konfiguraci zprostředkovatele konkrétní ověřování najdete v tématu [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="66ba1-176">For details on configuring specific authentication providers, see [Azure App Service authentication overview](../app-service/app-service-authentication-overview.md).</span></span> |

![Konfigurovat ověřování pro aplikaci funkce](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <span data-ttu-id="66ba1-178"><a name="swagger"></a>Definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="66ba1-178"><a name="swagger"></a>API definition</span></span>

| | |
|-|-|
| ![Funkce aplikace API swagger definice v hello portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | <span data-ttu-id="66ba1-180">Funkce podporuje Swagger tooallow klienti toomore snadno využívat funkce aktivované protokolem HTTP.</span><span class="sxs-lookup"><span data-stu-id="66ba1-180">Functions supports Swagger tooallow clients toomore easily consume your HTTP-triggered functions.</span></span> <span data-ttu-id="66ba1-181">Další informace týkající se vytváření se Swagger definice rozhraní API najdete v článku [Začínáme s API Apps, ASP.NET a Swaggerem v Azure](../app-service-api/app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="66ba1-181">For more information on creating API definitions with Swagger, visit [Get Started with API Apps, ASP.NET, and Swagger in Azure](../app-service-api/app-service-api-dotnet-get-started.md).</span></span> <span data-ttu-id="66ba1-182">Funkce proxy toodefine jedné plochy API můžete použít také pro víc funkcí.</span><span class="sxs-lookup"><span data-stu-id="66ba1-182">You can also use Functions Proxies toodefine a single API surface for multiple functions.</span></span> <span data-ttu-id="66ba1-183">Další informace najdete v tématu [práce s Azure funkce proxy](functions-proxies.md).</span><span class="sxs-lookup"><span data-stu-id="66ba1-183">For more information, see [Working with Azure Functions Proxies](functions-proxies.md).</span></span> |

![Konfigurace funkce aplikace API](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a><span data-ttu-id="66ba1-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66ba1-185">Next steps</span></span>

+ [<span data-ttu-id="66ba1-186">Konfigurace nastavení služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="66ba1-186">Configure Azure App Service Settings</span></span>](../app-service-web/web-sites-configure.md)
+ [<span data-ttu-id="66ba1-187">Průběžné nasazování se službou Azure Functions</span><span class="sxs-lookup"><span data-stu-id="66ba1-187">Continuous deployment for Azure Functions</span></span>](functions-continuous-deployment.md)



