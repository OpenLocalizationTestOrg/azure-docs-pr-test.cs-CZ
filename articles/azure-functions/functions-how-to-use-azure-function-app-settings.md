---
title: "Konfigurovat nastavení aplikace Azure funkce | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat nastavení aplikace Azure funkce."
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
ms.openlocfilehash: 3b23011f66592151d517d61bf806da8743f38e03
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a><span data-ttu-id="f5b03-103">Správa funkce aplikace na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f5b03-103">How to manage a function app in the Azure portal</span></span> 

<span data-ttu-id="f5b03-104">Funkce aplikace v Azure Functions poskytuje kontext spuštění pro jednotlivých funkcí.</span><span class="sxs-lookup"><span data-stu-id="f5b03-104">In Azure Functions, a function app provides the execution context for your individual functions.</span></span> <span data-ttu-id="f5b03-105">Chování funkce aplikace platí pro všechny funkce, které jsou hostované aplikací danou funkci.</span><span class="sxs-lookup"><span data-stu-id="f5b03-105">Function app behaviors apply to all functions hosted by a given function app.</span></span> <span data-ttu-id="f5b03-106">Toto téma popisuje, jak konfigurovat a spravovat aplikace pro funkce na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b03-106">This topic describes how to configure and manage your function apps in the Azure portal.</span></span>

<span data-ttu-id="f5b03-107">Chcete-li začít, přejděte na [portál Azure](http://portal.azure.com) a přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b03-107">To begin, go to the [Azure portal](http://portal.azure.com) and sign in to your Azure account.</span></span> <span data-ttu-id="f5b03-108">Na panelu hledání v horní části portálu zadejte název vaší aplikace Function App a vyberte ji ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="f5b03-108">In the search bar at the top of the portal, type the name of your function app and select it from the list.</span></span> <span data-ttu-id="f5b03-109">Po výběru funkce aplikace, se zobrazí následující stránka:</span><span class="sxs-lookup"><span data-stu-id="f5b03-109">After selecting your function app, you see the following page:</span></span>

![Přehled funkce aplikace na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <span data-ttu-id="f5b03-111"><a name="manage-app-service-settings"></a>Karta nastavení aplikace – funkce</span><span class="sxs-lookup"><span data-stu-id="f5b03-111"><a name="manage-app-service-settings"></a>Function app settings tab</span></span>

![Přehled funkce aplikace na portálu Azure.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

<span data-ttu-id="f5b03-113">**Nastavení** karta je, kde můžete aktualizovat verzi modulu runtime funkce, kterou vaše aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="f5b03-113">The **Settings** tab is where you can update the Functions runtime version used by your function app.</span></span> <span data-ttu-id="f5b03-114">Je také kde budete spravovat hostitele klíčů používaných omezit přístup protokolu HTTP na všechny funkce, které jsou hostované aplikací funkce.</span><span class="sxs-lookup"><span data-stu-id="f5b03-114">It is also where you manage the host keys used to restrict HTTP access to all functions hosted by the function app.</span></span>

<span data-ttu-id="f5b03-115">Funkce podporuje spotřeba hostování a hostování plány služby App Service.</span><span class="sxs-lookup"><span data-stu-id="f5b03-115">Functions supports both Consumption hosting and App Service hosting plans.</span></span> <span data-ttu-id="f5b03-116">Další informace najdete v tématu [vyberte plán správné služby pro Azure Functions](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="f5b03-116">For more information, see [Choose the correct service plan for Azure Functions](functions-scale.md).</span></span> <span data-ttu-id="f5b03-117">Pro lepší předvídatelnost v plánu spotřeby funkce vám umožní omezit využití platformy a to nastavením denní kvóty využití, v GB sekundách.</span><span class="sxs-lookup"><span data-stu-id="f5b03-117">For better predictability in the Consumption plan, Functions lets you limit platform usage by setting a daily usage quota, in gigabytes-seconds.</span></span> <span data-ttu-id="f5b03-118">Jakmile bude dosaženo kvóty denní využití, funkce aplikace je zastavena.</span><span class="sxs-lookup"><span data-stu-id="f5b03-118">Once the daily usage quota is reached, the function app is stopped.</span></span> <span data-ttu-id="f5b03-119">Aplikaci funkce zastavit v důsledku dosažení útraty kvótu může být ze stejné oblasti jako zřízení denně výdaje kvóty znovu zapnout.</span><span class="sxs-lookup"><span data-stu-id="f5b03-119">A function app stopped as a result of reaching the spending quota can be re-enabled from the same context as establishing the daily spending quota.</span></span> <span data-ttu-id="f5b03-120">Najdete v článku [Azure Functions stránce s cenami](http://azure.microsoft.com/pricing/details/functions/) podrobnosti o fakturaci.</span><span class="sxs-lookup"><span data-stu-id="f5b03-120">See the [Azure Functions pricing page](http://azure.microsoft.com/pricing/details/functions/) for details on billing.</span></span>   

## <a name="platform-features-tab"></a><span data-ttu-id="f5b03-121">Karta funkcí platformy</span><span class="sxs-lookup"><span data-stu-id="f5b03-121">Platform features tab</span></span>

![Karta funkcí platformy funkce aplikace.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

<span data-ttu-id="f5b03-123">Funkce aplikace spouštět ve a udržované platformou Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f5b03-123">Function apps run in, and are maintained, by the Azure App Service platform.</span></span> <span data-ttu-id="f5b03-124">Jako takový funkce aplikací mít přístup k většinu funkcí Azure základní webového hostingu platformy.</span><span class="sxs-lookup"><span data-stu-id="f5b03-124">As such, your function apps have access to most of the features of Azure's core web hosting platform.</span></span> <span data-ttu-id="f5b03-125">**Funkce** karta je, odkud si řadu funkcí služby App Service platformy, na které můžete použít ve svých aplikacích funkce.</span><span class="sxs-lookup"><span data-stu-id="f5b03-125">The **Platform features** tab is where you access the many features of the App Service platform that you can use in your function apps.</span></span> 

> [!NOTE]
> <span data-ttu-id="f5b03-126">Ne všechny funkce služby App Service jsou k dispozici při spuštění aplikace funkce na spotřebu hostování plánu.</span><span class="sxs-lookup"><span data-stu-id="f5b03-126">Not all App Service features are available when a function app runs on the Consumption hosting plan.</span></span>

<span data-ttu-id="f5b03-127">Zbývající část tohoto tématu se zaměřuje na těchto funkcích služby App Service na portálu Azure, které jsou užitečné pro funkce:</span><span class="sxs-lookup"><span data-stu-id="f5b03-127">The rest of this topic focuses on the following App Service features in the Azure portal that are useful for Functions:</span></span>

+ [<span data-ttu-id="f5b03-128">Editor služby App Service</span><span class="sxs-lookup"><span data-stu-id="f5b03-128">App Service editor</span></span>](#editor)
+ [<span data-ttu-id="f5b03-129">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="f5b03-129">Application settings</span></span>](#settings) 
+ [<span data-ttu-id="f5b03-130">Console</span><span class="sxs-lookup"><span data-stu-id="f5b03-130">Console</span></span>](#console)
+ [<span data-ttu-id="f5b03-131">Pokročilé nástroje (Kudu)</span><span class="sxs-lookup"><span data-stu-id="f5b03-131">Advanced tools (Kudu)</span></span>](#kudu)
+ [<span data-ttu-id="f5b03-132">Možnosti nasazení</span><span class="sxs-lookup"><span data-stu-id="f5b03-132">Deployment options</span></span>](#deployment)
+ [<span data-ttu-id="f5b03-133">CORS</span><span class="sxs-lookup"><span data-stu-id="f5b03-133">CORS</span></span>](#cors)
+ [<span data-ttu-id="f5b03-134">Ověřování</span><span class="sxs-lookup"><span data-stu-id="f5b03-134">Authentication</span></span>](#auth)
+ [<span data-ttu-id="f5b03-135">Definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f5b03-135">API definition</span></span>](#swagger)

<span data-ttu-id="f5b03-136">Další informace o tom, jak pracovat s nastavením služby App Service najdete v tématu [nakonfigurovat nastavení aplikace služby Azure](../app-service-web/web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="f5b03-136">For more information about how to work with App Service settings, see [Configure Azure App Service Settings](../app-service-web/web-sites-configure.md).</span></span>

### <span data-ttu-id="f5b03-137"><a name="editor"></a>Editor služby aplikace</span><span class="sxs-lookup"><span data-stu-id="f5b03-137"><a name="editor"></a>App Service Editor</span></span>

| | |
|-|-|
| ![Funkce aplikace služby App Service editor.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | <span data-ttu-id="f5b03-139">Editor služby App Service je editor pokročilé v portálu, který můžete použít k úpravě JSON konfigurační soubory a soubory kódu agentem.</span><span class="sxs-lookup"><span data-stu-id="f5b03-139">The App Service editor is an advanced in-portal editor that you can use to modify JSON configuration files and code files alike.</span></span> <span data-ttu-id="f5b03-140">Výběrem této možnosti se spustí na záložce prohlížeče samostatné s základní editor.</span><span class="sxs-lookup"><span data-stu-id="f5b03-140">Choosing this option launches a separate browser tab with a basic editor.</span></span> <span data-ttu-id="f5b03-141">To umožňuje integraci se službou úložiště Git, spuštění a ladění kódu a upravit nastavení funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b03-141">This enables you to integrate with the Git repository, run and debug code, and modify function app settings.</span></span> <span data-ttu-id="f5b03-142">Tohoto editoru poskytuje vylepšené vývojářské prostředí pro funkce v porovnání s výchozím okně funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b03-142">This editor provides an enhanced development environment for your functions compared with the default function app blade.</span></span>    |

![Editor služby App Service](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <span data-ttu-id="f5b03-144"><a name="settings"></a>Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="f5b03-144"><a name="settings"></a>Application settings</span></span>

| | |
|-|-|
| ![Nastavení aplikace funkce aplikace.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | <span data-ttu-id="f5b03-146">App Service **nastavení aplikace** okno je, kde můžete konfigurovat a spravovat framework verze, vzdálené ladění, nastavení aplikace a připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="f5b03-146">The App Service **Application settings** blade is where you configure and manage framework versions, remote debugging, app settings, and connection strings.</span></span> <span data-ttu-id="f5b03-147">Při integraci s jinými Azure a služby třetích stran, funkce aplikace, můžete upravit tato nastavení zde.</span><span class="sxs-lookup"><span data-stu-id="f5b03-147">When you integrate your function app with other Azure and third-party services, you can modify those settings here.</span></span> |

![Konfigurace nastavení aplikace](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <span data-ttu-id="f5b03-149"><a name="console"></a>Konzola</span><span class="sxs-lookup"><span data-stu-id="f5b03-149"><a name="console"></a>Console</span></span>

| | |
|-|-|
| ![Funkce aplikace konzoly na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | <span data-ttu-id="f5b03-151">Konzole v portálu je nástroj pro ideální developer, když chcete spolupracovat s vaší aplikací funkce z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f5b03-151">The in-portal console is an ideal developer tool when you prefer to interact with your function app from the command line.</span></span> <span data-ttu-id="f5b03-152">Běžné příkazy zahrnují adresáře a vytváření souborů a navigace, a také provádění dávkové soubory a skripty.</span><span class="sxs-lookup"><span data-stu-id="f5b03-152">Common commands include directory and file creation and navigation, as well as executing batch files and scripts.</span></span> |

![Funkce aplikace konzoly](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <span data-ttu-id="f5b03-154"><a name="kudu"></a>Pokročilé nástroje (Kudu)</span><span class="sxs-lookup"><span data-stu-id="f5b03-154"><a name="kudu"></a>Advanced tools (Kudu)</span></span>

| | |
|-|-|
| ![Funkce aplikace Kudu na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | <span data-ttu-id="f5b03-156">Rozšířené nástroje pro službu App Service (také označované jako Kudu) poskytují přístup k funkcím pro správu pokročilé funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5b03-156">The advanced tools for App Service (also known as Kudu) provide access to advanced administrative features of your function app.</span></span> <span data-ttu-id="f5b03-157">Z modulu Kudu spravovat informace o systému, nastavení aplikace, proměnné prostředí, rozšíření lokality, hlaviček protokolu HTTP a proměnných serveru.</span><span class="sxs-lookup"><span data-stu-id="f5b03-157">From Kudu, you manage system information, app settings, environment variables, site extensions, HTTP headers, and server variables.</span></span> <span data-ttu-id="f5b03-158">Můžete také spustit **Kudu** procházením SCM koncový bod pro funkce aplikace, jako je třeba`https://<myfunctionapp>.scm.azurewebsites.net/`</span><span class="sxs-lookup"><span data-stu-id="f5b03-158">You can also launch **Kudu** by browsing to the SCM endpoint for your function app, like `https://<myfunctionapp>.scm.azurewebsites.net/`</span></span> |

![Konfigurace modulu Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><span data-ttu-id="f5b03-160"><a name="deployment">Možnosti nasazení</span><span class="sxs-lookup"><span data-stu-id="f5b03-160"><a name="deployment">Deployment options</span></span>

| | |
|-|-|
| ![Funkce Možnosti nasazení aplikace na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | <span data-ttu-id="f5b03-162">Funkce vám umožní vyvíjet funkce kódu na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="f5b03-162">Functions lets you develop your function code on your local machine.</span></span> <span data-ttu-id="f5b03-163">Potom můžete nahrát projektu místní funkce aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b03-163">You can then upload your local function app project to Azure.</span></span> <span data-ttu-id="f5b03-164">Kromě tradičních odeslání na server FTP funkce umožňuje nasadit funkce aplikace pomocí Oblíbené průběžnou integraci řešení, jako jsou Githubu, služby VSTS, Dropbox, Bitbucket a dalších.</span><span class="sxs-lookup"><span data-stu-id="f5b03-164">In addition to traditional FTP upload, Functions lets you deploy your function app using popular continuous integration solutions, like GitHub, VSTS, Dropbox, Bitbucket, and others.</span></span> <span data-ttu-id="f5b03-165">Další informace najdete v tématu [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f5b03-165">For more information, see [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span> <span data-ttu-id="f5b03-166">Pokud chcete nahrát ručně pomocí protokol FTP nebo místní Git, je také nutné [nakonfigurovat přihlašovací údaje nasazení](functions-continuous-deployment.md#credentials).</span><span class="sxs-lookup"><span data-stu-id="f5b03-166">To upload manually using FTP or local Git, you also must [configure your deployment credentials](functions-continuous-deployment.md#credentials).</span></span> |


### <span data-ttu-id="f5b03-167"><a name="cors"></a>CORS</span><span class="sxs-lookup"><span data-stu-id="f5b03-167"><a name="cors"></a>CORS</span></span>

| | |
|-|-|
| ![Funkce aplikace CORS na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | <span data-ttu-id="f5b03-169">Pokud chcete zabránit spuštění škodlivého kódu v službách, služby App Service blokuje volání funkce aplikací z externích zdrojů.</span><span class="sxs-lookup"><span data-stu-id="f5b03-169">To prevent malicious code execution in your services, App Service blocks calls to your function apps from external sources.</span></span> <span data-ttu-id="f5b03-170">Funkce podporuje prostředků mezi zdroji (CORS) a umožňují definovat "povolených" povolené zdroje, ze kterých může přijmout funkce vzdálené žádosti pro sdílení.</span><span class="sxs-lookup"><span data-stu-id="f5b03-170">Functions supports cross-origin resource sharing (CORS) to let you define a "whitelist" of allowed origins from which your functions can accept remote requests.</span></span>  |

![Konfigurace funkce aplikace CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <span data-ttu-id="f5b03-172"><a name="auth"></a>Ověřování</span><span class="sxs-lookup"><span data-stu-id="f5b03-172"><a name="auth"></a>Authentication</span></span>

| | |
|-|-|
| ![Funkce ověřování aplikací na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | <span data-ttu-id="f5b03-174">Když funkce používají aktivační procedury protokolu HTTP, můžete vyžadovat volání nejdřív ověřit.</span><span class="sxs-lookup"><span data-stu-id="f5b03-174">When functions use an HTTP trigger, you can require calls to first be authenticated.</span></span> <span data-ttu-id="f5b03-175">App Service podporuje ověřování Azure Active Directory a přihlaste se pomocí sociálních sítí, jako je Facebook, Microsoft a Twitter.</span><span class="sxs-lookup"><span data-stu-id="f5b03-175">App Service supports Azure Active Directory authentication and sign in with social providers, such as Facebook, Microsoft, and Twitter.</span></span> <span data-ttu-id="f5b03-176">Podrobnosti o konfiguraci zprostředkovatele konkrétní ověřování najdete v tématu [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5b03-176">For details on configuring specific authentication providers, see [Azure App Service authentication overview](../app-service/app-service-authentication-overview.md).</span></span> |

![Konfigurovat ověřování pro aplikaci funkce](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <span data-ttu-id="f5b03-178"><a name="swagger"></a>Definice rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f5b03-178"><a name="swagger"></a>API definition</span></span>

| | |
|-|-|
| ![Funkce aplikace API swagger definice na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | <span data-ttu-id="f5b03-180">Funkce podporuje Swagger tak, aby klienti snadno využívat funkce aktivované protokolem HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5b03-180">Functions supports Swagger to allow clients to more easily consume your HTTP-triggered functions.</span></span> <span data-ttu-id="f5b03-181">Další informace týkající se vytváření se Swagger definice rozhraní API najdete v článku [Začínáme s API Apps, ASP.NET a Swaggerem v Azure](../app-service-api/app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f5b03-181">For more information on creating API definitions with Swagger, visit [Get Started with API Apps, ASP.NET, and Swagger in Azure](../app-service-api/app-service-api-dotnet-get-started.md).</span></span> <span data-ttu-id="f5b03-182">Funkce proxy lze také definovat jeden prostor rozhraní API pro víc funkcí.</span><span class="sxs-lookup"><span data-stu-id="f5b03-182">You can also use Functions Proxies to define a single API surface for multiple functions.</span></span> <span data-ttu-id="f5b03-183">Další informace najdete v tématu [práce s Azure funkce proxy](functions-proxies.md).</span><span class="sxs-lookup"><span data-stu-id="f5b03-183">For more information, see [Working with Azure Functions Proxies](functions-proxies.md).</span></span> |

![Konfigurace funkce aplikace API](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a><span data-ttu-id="f5b03-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5b03-185">Next steps</span></span>

+ [<span data-ttu-id="f5b03-186">Konfigurace nastavení služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f5b03-186">Configure Azure App Service Settings</span></span>](../app-service-web/web-sites-configure.md)
+ [<span data-ttu-id="f5b03-187">Průběžné nasazování se službou Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f5b03-187">Continuous deployment for Azure Functions</span></span>](functions-continuous-deployment.md)



