---
title: "Funkce operačního systému v Azure App Service"
description: "Další informace o funkci operačního systému k dispozici pro webové aplikace, back-EndY mobilních aplikací a aplikace API v Azure App Service"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 18ff5c81d0aa5e8a28ed8a11dad19811d2fa1d2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="operating-system-functionality-on-azure-app-service"></a><span data-ttu-id="db274-103">Funkce operačního systému v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="db274-103">Operating system functionality on Azure App Service</span></span>
<span data-ttu-id="db274-104">Tento článek popisuje běžné funkce operačního systému standardních hodnot, které jsou k dispozici pro všechny aplikace běžící na [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="db274-104">This article describes the common baseline operating system functionality that is available to all apps running on [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="db274-105">Tato funkce zahrnuje soubor, sítě a přístup k registru a diagnostické protokoly a události.</span><span class="sxs-lookup"><span data-stu-id="db274-105">This functionality includes file, network, and registry access, and diagnostics logs and events.</span></span> 

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a><span data-ttu-id="db274-106">Úrovně plánu služby App Service</span><span class="sxs-lookup"><span data-stu-id="db274-106">App Service plan tiers</span></span>
<span data-ttu-id="db274-107">Služby App Service zákazníka aplikace běží v hostitelských prostředí s více klienty.</span><span class="sxs-lookup"><span data-stu-id="db274-107">App Service runs customer apps in a multi-tenant hosting environment.</span></span> <span data-ttu-id="db274-108">Aplikace nasazené v **volné** a **sdílené** vrstev spustit v pracovních procesů na sdílených virtuálních počítačích, zatímco aplikace nasazené v **standardní** a **Premium** vrstev spustit na virtuální počítače vyhrazené speciálně pro aplikace přidružené k jednoho zákazníka.</span><span class="sxs-lookup"><span data-stu-id="db274-108">Apps deployed in the **Free** and **Shared** tiers run in worker processes on shared virtual machines, while apps deployed in the **Standard** and **Premium** tiers run on virtual machine(s) dedicated specifically for the apps associated with a single customer.</span></span>

<span data-ttu-id="db274-109">Protože služby App Service podporuje bezproblémové škálování prostředí mezi různých vrstev, konfigurace zabezpečení pro aplikace služby App Service vynucený zůstává stejná.</span><span class="sxs-lookup"><span data-stu-id="db274-109">Because App Service supports a seamless scaling experience between different tiers, the security configuration enforced for App Service apps remains the same.</span></span> <span data-ttu-id="db274-110">Tím se zajistí, že aplikace není chovat najednou jinak, selhání neočekávané způsoby, když se plán služby App Service přepne z jedné vrstvy do jiného.</span><span class="sxs-lookup"><span data-stu-id="db274-110">This ensures that apps don't suddenly behave differently, failing in unexpected ways, when App Service plan switches from one tier to another.</span></span>

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a><span data-ttu-id="db274-111">Architektury pro vývoj</span><span class="sxs-lookup"><span data-stu-id="db274-111">Development frameworks</span></span>
<span data-ttu-id="db274-112">Cenové úrovně služby App Service řízení množství výpočetních prostředků (procesor, diskové úložiště, paměti a odchozí síťový) k dispozici pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="db274-112">App Service pricing tiers control the amount of compute resources (CPU, disk storage, memory, and network egress) available to apps.</span></span> <span data-ttu-id="db274-113">Však šířky framework funkce je k dispozici pro aplikace zůstává stejný bez ohledu na škálování vrstvy.</span><span class="sxs-lookup"><span data-stu-id="db274-113">However, the breadth of framework functionality available to apps remains the same regardless of the scaling tiers.</span></span>

<span data-ttu-id="db274-114">App Service podporuje celou řadu architektury pro vývoj, včetně ASP.NET, klasické ASP, node.js, PHP a Python - všechny spouští jako rozšíření v rámci služby IIS.</span><span class="sxs-lookup"><span data-stu-id="db274-114">App Service supports a variety of development frameworks, including ASP.NET, classic ASP, node.js, PHP and Python - all of which run as extensions within IIS.</span></span> <span data-ttu-id="db274-115">Aby bylo možné zjednodušit a normalizaci konfigurace zabezpečení, aplikacemi App Service obvykle běží různé architektury pro vývoj ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="db274-115">In order to simplify and normalize security configuration, App Service apps typically run the various development frameworks with their default settings.</span></span> <span data-ttu-id="db274-116">Jeden ze způsobů konfigurace aplikací může se k útoku na rozhraní API a funkce pro každé rozhraní pro jednotlivé vývoj přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="db274-116">One approach to configuring apps could have been to customize the API surface area and functionality for each individual development framework.</span></span> <span data-ttu-id="db274-117">Služby App Service místo trvá více obecné přístup povolením běžné základní funkce operačního systému bez ohledu na to rozhraní pro vývoj aplikace.</span><span class="sxs-lookup"><span data-stu-id="db274-117">App Service instead takes a more generic approach by enabling a common baseline of operating system functionality regardless of an app's development framework.</span></span>

<span data-ttu-id="db274-118">Následující části shrnují obecné druhy funkce operačního systému k dispozici pro aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="db274-118">The following sections summarize the general kinds of operating system functionality available to App Service apps.</span></span>

<a id="FileAccess"></a>

## <a name="file-access"></a><span data-ttu-id="db274-119">Přístup k souborům</span><span class="sxs-lookup"><span data-stu-id="db274-119">File access</span></span>
<span data-ttu-id="db274-120">Existují různé jednotky v App Service, včetně místní jednotky a síťové jednotky.</span><span class="sxs-lookup"><span data-stu-id="db274-120">Various drives exist within App Service, including local drives and network drives.</span></span>

<a id="LocalDrives"></a>

### <a name="local-drives"></a><span data-ttu-id="db274-121">Místní jednotky</span><span class="sxs-lookup"><span data-stu-id="db274-121">Local drives</span></span>
<span data-ttu-id="db274-122">Jádro aplikace App Service je služby spuštěné na infrastruktuře Azure PaaS (platforma jako služba).</span><span class="sxs-lookup"><span data-stu-id="db274-122">At its core, App Service is a service running on top of the Azure PaaS (platform as a service) infrastructure.</span></span> <span data-ttu-id="db274-123">Místní disky, které jsou "připojené" k virtuálnímu počítači v důsledku toho jsou stejné typy jednotku, která je k dispozici pro všechny role pracovního procesu, který běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="db274-123">As a result, the local drives that are "attached" to a virtual machine are the same drive types available to any worker role running in Azure.</span></span> <span data-ttu-id="db274-124">To zahrnuje jednotce operačního systému (jednotku D:\), aplikaci jednotku, která obsahuje balíček Azure cspkg soubory používané výhradně službou App Service (a dostupná pro zákazníky) a na jednotku "user" (jednotka C:\), jejichž velikost se liší v závislosti na velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db274-124">This includes an operating system drive (the D:\ drive), an application drive that contains Azure Package cspkg files used exclusively by App Service (and inaccessible to customers), and a "user" drive (the C:\ drive), whose size varies depending on the size of the VM.</span></span>

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a><span data-ttu-id="db274-125">Síťové jednotky (neboli UNC sdílené složky)</span><span class="sxs-lookup"><span data-stu-id="db274-125">Network drives (aka UNC shares)</span></span>
<span data-ttu-id="db274-126">Jedním z jedinečné aspekty aplikační služba, která usnadňuje nasazení aplikace a údržby přehledné je, že je všechny uživatele obsah uložený na sadu sdílené složky UNC.</span><span class="sxs-lookup"><span data-stu-id="db274-126">One of the unique aspects of App Service that makes app deployment and maintenance straightforward is that all user content is stored on a set of UNC shares.</span></span> <span data-ttu-id="db274-127">Tento model mapuje velmi vhodně běžný vzor úložiště obsahu, které používá místní webových hostitelských prostředích, která mají víc serverů s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="db274-127">This model maps very nicely to the common pattern of content storage used by on-premises web hosting environments that have multiple load-balanced servers.</span></span> 

<span data-ttu-id="db274-128">V rámci služby App Service se počet UNC sdílené složky vytvořené v každém datovém centru.</span><span class="sxs-lookup"><span data-stu-id="db274-128">Within App Service, there are number of UNC shares created in each data center.</span></span> <span data-ttu-id="db274-129">Procento obsah uživatele pro všechny zákazníky v každé datového centra je přidělen pro každé sdílené složky UNC.</span><span class="sxs-lookup"><span data-stu-id="db274-129">A percentage of the user content for all customers in each data center is allocated to each UNC share.</span></span> <span data-ttu-id="db274-130">Kromě toho celého obsahu, pro jednoho zákazníka předplatného je vždy umístit na stejnou cestu UNC souboru sdílet.</span><span class="sxs-lookup"><span data-stu-id="db274-130">Furthermore, all of the file content for a single customer's subscription is always placed on the same UNC share.</span></span> 

<span data-ttu-id="db274-131">Všimněte si, že se z důvodu jak cloudových služeb pracovní, zodpovědná za sdílené složky UNC hostování konkrétní virtuální počítač se změní v čase.</span><span class="sxs-lookup"><span data-stu-id="db274-131">Note that due to how cloud services work, the specific virtual machine responsible for hosting a UNC share will change over time.</span></span> <span data-ttu-id="db274-132">Je zaručeno, že sdílené složky UNC se připojí jiné virtuální počítače, jako přechodem při běžném průběhu operací cloudu nahoru a dolů.</span><span class="sxs-lookup"><span data-stu-id="db274-132">It is guaranteed that UNC shares will be mounted by different virtual machines as they are brought up and down during the normal course of cloud operations.</span></span> <span data-ttu-id="db274-133">Z tohoto důvodu by měla aplikace umožňují nikdy pevně předpoklady, informace o počítači v cestě k souboru UNC zůstane stabilní v čase.</span><span class="sxs-lookup"><span data-stu-id="db274-133">For this reason, apps should never make hard-coded assumptions that the machine information in a UNC file path will remain stable over time.</span></span> <span data-ttu-id="db274-134">Místo toho měli používat vhodného *umělé* absolutní cestu **D:\home\site** poskytující služby App Service.</span><span class="sxs-lookup"><span data-stu-id="db274-134">Instead, they should use the convenient *faux* absolute path **D:\home\site** that App Service provides.</span></span> <span data-ttu-id="db274-135">Tato umělé absolutní cesta poskytuje přenosné, aplikace a uživatele – bez ohledu na metodu pro odkaz na aplikaci vlastního.</span><span class="sxs-lookup"><span data-stu-id="db274-135">This faux absolute path provides a portable, app-and-user-agnostic method for referring to one's own app.</span></span> <span data-ttu-id="db274-136">Pomocí **D:\home\site**, jeden můžou přenášet sdílených souborů z aplikace do aplikace bez nutnosti konfigurovat novou absolutní cestu pro každý přenos.</span><span class="sxs-lookup"><span data-stu-id="db274-136">By using **D:\home\site**, one can transfer shared files from app to app without having to configure a new absolute path for each transfer.</span></span>

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-to-an-app"></a><span data-ttu-id="db274-137">Typy souboru udělení přístupu k aplikaci</span><span class="sxs-lookup"><span data-stu-id="db274-137">Types of file access granted to an app</span></span>
<span data-ttu-id="db274-138">Každý zákazník předplatné má vyhrazené adresářovou strukturu na sdílenou jednotku UNC konkrétní v rámci datového centra.</span><span class="sxs-lookup"><span data-stu-id="db274-138">Each customer's subscription has a reserved directory structure on a specific UNC share within a data center.</span></span> <span data-ttu-id="db274-139">Zákazník může mít víc aplikací v rámci konkrétní datové středisko, vytvořili, všechny adresáře, které patří do jednoho zákazníka předplatného se vytvářejí na stejnou cestu UNC sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="db274-139">A customer may have multiple apps created within a specific data center, so all of the directories belonging to a single customer subscription are created on the same UNC share.</span></span> <span data-ttu-id="db274-140">Sdílené složce může zahrnovat adresáře jsou pro obsah, chyby a diagnostické protokoly a dřívějších verzích aplikace vytvořené zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="db274-140">The share may include directories such as those for content, error and diagnostic logs, and earlier versions of the app created by source control.</span></span> <span data-ttu-id="db274-141">Podle očekávání, adresáře aplikace zákazníka jsou k dispozici pro oprávnění ke čtení a zápisu v době běhu aplikace kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="db274-141">As expected, a customer's app directories are available for read and write access at runtime by the app's application code.</span></span>

<span data-ttu-id="db274-142">Na místní diskové jednotky připojené k virtuálnímu počítači, který spouští aplikaci služby App Service si vyhrazuje bloku místa na jednotce C:\ pro konkrétní aplikaci dočasné místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="db274-142">On the local drives attached to the virtual machine that runs an app, App Service reserves a chunk of space on the C:\ drive for app-specific temporary local storage.</span></span> <span data-ttu-id="db274-143">I když aplikace má čtení/zápisu přístup k vlastní dočasné místní úložiště, že úložiště skutečně není určena pro použití přímo v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="db274-143">Although an app has full read/write access to its own temporary local storage, that storage really isn't intended to be used directly by the application code.</span></span> <span data-ttu-id="db274-144">Místo toho je záměr zajistit ukládání dočasných souborů pro službu IIS a webové aplikace rozhraní.</span><span class="sxs-lookup"><span data-stu-id="db274-144">Rather, the intent is to provide temporary file storage for IIS and web application frameworks.</span></span> <span data-ttu-id="db274-145">Velikost dočasné místní úložiště, které jsou k dispozici pro každou aplikaci, aby zabránila jednotlivými aplikacemi nespotřeboval nadměrné množství úložiště místního souboru je také omezení služby App Service.</span><span class="sxs-lookup"><span data-stu-id="db274-145">App Service also limits the amount of temporary local storage available to each app to prevent individual apps from consuming excessive amounts of local file storage.</span></span>

<span data-ttu-id="db274-146">Jsou dva příklady, jak služba aplikace používá dočasné úložiště místní adresář pro dočasné soubory ASP.NET a komprimované soubory v adresáři služby IIS.</span><span class="sxs-lookup"><span data-stu-id="db274-146">Two examples of how App Service uses temporary local storage are the directory for temporary ASP.NET files and the directory for IIS compressed files.</span></span> <span data-ttu-id="db274-147">Systém kompilace technologie ASP.NET používá jako umístění mezipaměti dočasné kompilace adresáře "Temporary ASP.NET Files".</span><span class="sxs-lookup"><span data-stu-id="db274-147">The ASP.NET compilation system uses the "Temporary ASP.NET Files" directory as a temporary compilation cache location.</span></span> <span data-ttu-id="db274-148">Služba IIS používá adresáři "IIS dočasné komprimované soubory" pro ukládání výstupu zkomprimovanou odpověď.</span><span class="sxs-lookup"><span data-stu-id="db274-148">IIS uses the "IIS Temporary Compressed Files" directory to store compressed response output.</span></span> <span data-ttu-id="db274-149">Oba tyto typy souborů využití (stejně jako ostatní) jsou mapovány na dočasné místní úložiště pro aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="db274-149">Both of these types of file usage (as well as others) are remapped in App Service to per-app temporary local storage.</span></span> <span data-ttu-id="db274-150">Přemapování zajistí, že funkce pokračuje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="db274-150">This remapping ensures that functionality continues as expected.</span></span>

<span data-ttu-id="db274-151">Každá aplikace ve službě App Service spouští jako identitu náhodných jedinečný nízkými oprávněními pracovního procesu názvem "identita fondu aplikací", zde popsané dál: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities).</span><span class="sxs-lookup"><span data-stu-id="db274-151">Each app in App Service runs as a random unique low-privileged worker process identity called the "application pool identity", described further here: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities).</span></span> <span data-ttu-id="db274-152">Kód aplikace použije tuto identitu pro základní přístup jen pro čtení k jednotce operačního systému (jednotku D:\).</span><span class="sxs-lookup"><span data-stu-id="db274-152">Application code uses this identity for basic read-only access to the operating system drive (the D:\ drive).</span></span> <span data-ttu-id="db274-153">To znamená, že kód aplikace, můžete seznam běžných struktur adresářů a číst společné soubory na jednotce operačního systému.</span><span class="sxs-lookup"><span data-stu-id="db274-153">This means application code can list common directory structures and read common files on operating system drive.</span></span> <span data-ttu-id="db274-154">I když to může vypadat jako poněkud obecné úrovni přístupu, stejné adresářů a souborů jsou dostupné při zřizování roli pracovního procesu v Azure hostovaná služba a číst obsah jednotky.</span><span class="sxs-lookup"><span data-stu-id="db274-154">Although this might appear to be a somewhat broad level of access, the same directories and files are accessible when you provision a worker role in an Azure hosted service and read the drive contents.</span></span> 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a><span data-ttu-id="db274-155">Přístup k souborům ve více instancích</span><span class="sxs-lookup"><span data-stu-id="db274-155">File access across multiple instances</span></span>
<span data-ttu-id="db274-156">Domovský adresář je obsah aplikace a kód aplikace může do něj zapisovat.</span><span class="sxs-lookup"><span data-stu-id="db274-156">The home directory contains an app's content, and application code can write to it.</span></span> <span data-ttu-id="db274-157">Pokud aplikace běží na několik instancí, domovský adresář je sdílena mezi všechny instance, tak, aby všechny instance, najdete v článku do stejného adresáře.</span><span class="sxs-lookup"><span data-stu-id="db274-157">If an app runs on multiple instances, the home directory is shared among all instances so that all instances see the same directory.</span></span> <span data-ttu-id="db274-158">Ano, například pokud aplikace uloží odeslané soubory na domovský adresář, tyto soubory jsou okamžitě k dispozici pro všechny instance.</span><span class="sxs-lookup"><span data-stu-id="db274-158">So, for example, if an app saves uploaded files to the home directory, those files are immediately available to all instances.</span></span> 

<a id="NetworkAccess"></a>

## <a name="network-access"></a><span data-ttu-id="db274-159">Přístup k síti</span><span class="sxs-lookup"><span data-stu-id="db274-159">Network access</span></span>
<span data-ttu-id="db274-160">Kód aplikace můžete použít protokol TCP/IP a UDP, na základě protokolů, aby odchozí síťová připojení přístupné koncových bodů Internet, které zveřejňují externích služeb.</span><span class="sxs-lookup"><span data-stu-id="db274-160">Application code can use TCP/IP and UDP based protocols to make outbound network connections to Internet accessible endpoints that expose external services.</span></span> <span data-ttu-id="db274-161">Aplikace můžete použít tyto stejné protokoly pro připojení ke službám v rámci Azure &#151; například při navazování připojení prostřednictvím protokolu HTTPS k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="db274-161">Apps can use these same protocols to connect to services within Azure&#151;for example, by establishing HTTPS connections to SQL Database.</span></span>

<span data-ttu-id="db274-162">Je také omezené funkce pro aplikace k připojení jeden místní smyčky a aplikace naslouchání že soketem místní smyčky.</span><span class="sxs-lookup"><span data-stu-id="db274-162">There is also a limited capability for apps to establish one local loopback connection, and have an app listen on that local loopback socket.</span></span> <span data-ttu-id="db274-163">Tato funkce existuje především k aplikacím, které čekají na místní smyčky sockets jako součást jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="db274-163">This feature exists primarily to enable apps that listen on local loopback sockets as part of their functionality.</span></span> <span data-ttu-id="db274-164">Všimněte si, že každá aplikace uvidí připojení "privátní" zpětné smyčky; aplikace "A" nemůže naslouchat na soket místní smyčky navázat aplikací "B".</span><span class="sxs-lookup"><span data-stu-id="db274-164">Note that each app sees a "private" loopback connection; app "A" cannot listen to a local loopback socket established by app "B".</span></span>

<span data-ttu-id="db274-165">Pojmenované kanály jsou podporovány také jako mechanismus meziprocesová komunikace (IPC) mezi různé procesy, které se souhrnně spustí aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db274-165">Named pipes are also supported as an inter-process communication (IPC) mechanism between different processes that collectively run an app.</span></span> <span data-ttu-id="db274-166">Například modul FastCGI služby IIS využívá pojmenované kanály pro koordinaci jednotlivých procesů, které spouštějí stránky PHP.</span><span class="sxs-lookup"><span data-stu-id="db274-166">For example, the IIS FastCGI module relies on named pipes to coordinate the individual processes that run PHP pages.</span></span>

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a><span data-ttu-id="db274-167">Provádění kódu, procesy a paměti</span><span class="sxs-lookup"><span data-stu-id="db274-167">Code execution, processes and memory</span></span>
<span data-ttu-id="db274-168">Jak již bylo uvedeno dříve, aplikace běžet uvnitř nízkými oprávněními pracovních procesů pomocí identita fondu náhodných aplikací.</span><span class="sxs-lookup"><span data-stu-id="db274-168">As noted earlier, apps run inside of low-privileged worker processes using a random application pool identity.</span></span> <span data-ttu-id="db274-169">Kód aplikace má přístup do paměti prostoru přidružené pracovní proces, jakož žádné podřízené procesy, které může být vytvořený procesy CGI nebo jiné aplikace.</span><span class="sxs-lookup"><span data-stu-id="db274-169">Application code has access to the memory space associated with the worker process, as well as any child processes that may be spawned by CGI processes or other applications.</span></span> <span data-ttu-id="db274-170">Ale jednu aplikaci nelze přístup k paměti nebo data z jiné aplikace i v případě, že je na jednom virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="db274-170">However, one app cannot access the memory or data of another app even if it is on the same virtual machine.</span></span>

<span data-ttu-id="db274-171">Aplikace můžete spustit skripty nebo napsané pomocí architektury pro vývoj podporované webové stránky.</span><span class="sxs-lookup"><span data-stu-id="db274-171">Apps can run scripts or pages written with supported web development frameworks.</span></span> <span data-ttu-id="db274-172">Služby App Service není nakonfigurujte všechna nastavení webové framework na omezenější režimy.</span><span class="sxs-lookup"><span data-stu-id="db274-172">App Service doesn't configure any web framework settings to more restricted modes.</span></span> <span data-ttu-id="db274-173">Například spusťte ASP.NET aplikace běžící na App Service v "úplná" důvěryhodnosti oproti omezenější režim vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="db274-173">For example, ASP.NET apps running on App Service run in "full" trust as opposed to a more restricted trust mode.</span></span> <span data-ttu-id="db274-174">Webové rozhraní, včetně classic ASP a ASP.NET, může zavolat komponenty modelu COM v procesu (ale ne mimo proces COM – součásti) jako ADO (ActiveX Data Objects), která jsou zaregistrovaná ve výchozím nastavení operačního systému Windows.</span><span class="sxs-lookup"><span data-stu-id="db274-174">Web frameworks, including both classic ASP and ASP.NET, can call in-process COM components (but not out of process COM components) like ADO (ActiveX Data Objects) that are registered by default on the Windows operating system.</span></span>

<span data-ttu-id="db274-175">Aplikace můžete vytvořit a spustit libovolný kód.</span><span class="sxs-lookup"><span data-stu-id="db274-175">Apps can spawn and run arbitrary code.</span></span> <span data-ttu-id="db274-176">Je povolený pro aplikaci, provádět akce, jako je spawn příkazové prostředí nebo spustit skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db274-176">It is allowable for an app to do things like spawn a command shell or run a PowerShell script.</span></span> <span data-ttu-id="db274-177">I když libovolný kód a procesů může být vytvořený z aplikace, spustitelné programy a skripty jsou však stále omezen na udělena k fondu aplikací nadřazené oprávnění.</span><span class="sxs-lookup"><span data-stu-id="db274-177">However, even though arbitrary code and processes can be spawned from an app, executable programs and scripts are still restricted to the privileges granted to the parent application pool.</span></span> <span data-ttu-id="db274-178">Například může aplikace spawn spustitelné soubory, které umožňuje odchozí volání protokolu HTTP, ale, že stejný spustitelný soubor nelze pokus o odpojení IP adresu virtuálního počítače z jeho síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="db274-178">For example, an app can spawn an executable that makes an outbound HTTP call, but that same executable cannot attempt to unbind the IP address of a virtual machine from its NIC.</span></span> <span data-ttu-id="db274-179">Vytvořit volání odchozí sítě je povolený kódu nízkou úrovní oprávnění, ale pokusu změnit konfiguraci nastavení sítě na virtuálním počítači vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="db274-179">Making an outbound network call is allowed to low-privileged code, but attempting to reconfigure network settings on a virtual machine requires administrative privileges.</span></span>

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a><span data-ttu-id="db274-180">Diagnostické protokoly a události</span><span class="sxs-lookup"><span data-stu-id="db274-180">Diagnostics logs and events</span></span>
<span data-ttu-id="db274-181">Informace v protokolu je jinou sadu dat, která některé aplikace pokusí o přístup.</span><span class="sxs-lookup"><span data-stu-id="db274-181">Log information is another set of data that some apps attempt to access.</span></span> <span data-ttu-id="db274-182">Typy protokolu informací, které jsou k dispozici pro kód spuštěný ve službě App Service zahrnuje diagnostiky a protokolovat informace generované aplikaci, která je také snadno dostupná k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db274-182">The types of log information available to code running in App Service includes diagnostic and log information generated by an app that is also easily accessible to the app.</span></span> 

<span data-ttu-id="db274-183">Například protokoly W3C HTTP vygenerovaný active aplikací jsou k dispozici, buď v adresáři protokolu v umístění síťové sdílené složky vytvořené pro aplikaci nebo k dispozici v úložišti objektů blob, pokud zákazník nastavil protokolování W3C do úložiště.</span><span class="sxs-lookup"><span data-stu-id="db274-183">For example, W3C HTTP logs generated by an active app are available either on a log directory in the network share location created for the app, or available in blob storage if a customer has set up W3C logging to storage.</span></span> <span data-ttu-id="db274-184">Druhou možnost umožňuje velké objemy protokolů, které chcete shromáždit bez riziko překračuje limit úložiště souborů přidružené k síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="db274-184">The latter option enables large quantities of logs to be gathered without the risk of exceeding the file storage limits associated with a network share.</span></span>

<span data-ttu-id="db274-185">V souvislosti podobnou v reálném čase diagnostické informace z aplikací .NET můžete taky zaznamená .NET trasování a diagnostiku infrastrukturu, pomocí možnosti zapsat informace trasování do síťové složky, buď aplikace, případně k umístění úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="db274-185">In a similar vein, real-time diagnostics information from .NET apps can also be logged using the .NET tracing and diagnostics infrastructure, with options to write the trace information to either the app's network share, or alternatively to a blob storage location.</span></span>

<span data-ttu-id="db274-186">Oblasti diagnostiky protokolování a trasování, která nejsou k dispozici pro aplikace, jsou události Windows ETW a běžné protokoly událostí systému Windows (například systému, aplikace a zabezpečení protokoly událostí).</span><span class="sxs-lookup"><span data-stu-id="db274-186">Areas of diagnostics logging and tracing that aren't available to apps are Windows ETW events and common Windows event logs (e.g. System, Application and Security event logs).</span></span> <span data-ttu-id="db274-187">Vzhledem k tomu, že informace o trasování ETW potenciálně lze zobrazit celého systému (s právem seznamy ACL), jsou zablokovány oprávnění ke čtení a zápis na události trasování událostí pro Windows.</span><span class="sxs-lookup"><span data-stu-id="db274-187">Since ETW trace information can potentially be viewable machine-wide (with the right ACLs), read and write access to ETW events are blocked.</span></span> <span data-ttu-id="db274-188">Vývojáři mohou Všimněte si, že volání rozhraní API pro čtení a zápis trasování událostí a běžné protokoly událostí systému Windows pravděpodobně fungovat, ale je to způsobeno služby App Service je "faking" volání, aby se zobrazily proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="db274-188">Developers might notice that API calls to read and write ETW events and common Windows event logs appear to work, but that is because App Service is "faking" the calls so that they appear to succeed.</span></span> <span data-ttu-id="db274-189">Ve skutečnosti kódu aplikace nemá přístup k těmto datům událostí.</span><span class="sxs-lookup"><span data-stu-id="db274-189">In reality, the application code has no access to this event data.</span></span>

<a id="RegistryAccess"></a>

## <a name="registry-access"></a><span data-ttu-id="db274-190">Přístup k registru</span><span class="sxs-lookup"><span data-stu-id="db274-190">Registry access</span></span>
<span data-ttu-id="db274-191">Aplikace mají přístup jen pro čtení k mnohem (ale ne všechny) virtuálního počítače, že jsou spuštěny v registru.</span><span class="sxs-lookup"><span data-stu-id="db274-191">Apps have read-only access to much (though not all) of the registry of the virtual machine they are running on.</span></span> <span data-ttu-id="db274-192">V praxi to znamená, že jsou přístupné pro aplikace klíče registru, které povolí přístup jen pro čtení do místní skupiny uživatelů.</span><span class="sxs-lookup"><span data-stu-id="db274-192">In practice, this means registry keys that allow read-only access to the local Users group are accessible by apps.</span></span> <span data-ttu-id="db274-193">Jednou z oblastí registru, která není aktuálně podporována pro čtení nebo zápis je nastavení HKEY\_aktuální\_uživatele hive.</span><span class="sxs-lookup"><span data-stu-id="db274-193">One area of the registry that is currently not supported for either read or write access is the HKEY\_CURRENT\_USER hive.</span></span>

<span data-ttu-id="db274-194">Zápis do registru je blokovaný, včetně přístupu k žádné klíče registru na uživatele.</span><span class="sxs-lookup"><span data-stu-id="db274-194">Write-access to the registry is blocked, including access to any per-user registry keys.</span></span> <span data-ttu-id="db274-195">Z hlediska aplikace přístup k zápisu do registru by nikdy spoléhat v prostředí Azure vzhledem k tomu, že aplikace se můžou (a provést) získat proběhne migrace na jiné virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="db274-195">From the app's perspective, write access to the registry should never be relied upon in the Azure environment since apps can (and do) get migrated across different virtual machines.</span></span> <span data-ttu-id="db274-196">Pouze trvalé zapisovatelné úložiště, které může být závisí na aplikaci je struktura adresář s obsahem pro aplikaci, uložené ve sdílené složky UNC služby aplikace.</span><span class="sxs-lookup"><span data-stu-id="db274-196">The only persistent writeable storage that can be depended on by an app is the per-app content directory structure stored on the App Service UNC shares.</span></span> 

## <a name="more-information"></a><span data-ttu-id="db274-197">Další informace</span><span class="sxs-lookup"><span data-stu-id="db274-197">More information</span></span>

<span data-ttu-id="db274-198">[Azure izolovaného prostoru webové aplikace](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) – nejnovější informace o prostředí pro spuštění služby App Service.</span><span class="sxs-lookup"><span data-stu-id="db274-198">[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) - The most up-to-date information about the execution environment of App Service.</span></span> <span data-ttu-id="db274-199">Tato stránka se spravuje přímo pomocí vývojový tým služby App Service.</span><span class="sxs-lookup"><span data-stu-id="db274-199">This page is maintained directly by the App Service development team.</span></span>

> [!NOTE]
> <span data-ttu-id="db274-200">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db274-200">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="db274-201">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="db274-201">No credit cards required; no commitments.</span></span>
> 
> 

