---
title: "Zabezpečení aplikace ve službě Azure App Service"
description: "Zjistěte, jak zabezpečit webové aplikace, back-end mobilní aplikace nebo aplikace API v Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 5ce560b4-42d7-4b20-935c-0445fd539e39
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: b70d74441f3d6d9793ae516b3f04e36e786a9a8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-an-app-in-azure-app-service"></a><span data-ttu-id="8284c-103">Zabezpečení aplikace ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8284c-103">Secure an app in Azure App Service</span></span>
<span data-ttu-id="8284c-104">Tento článek vám pomůže začít pracovat na zabezpečení vaší webové aplikace, back-end mobilní aplikace nebo aplikace API v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="8284c-104">This article helps you get started on securing your web app, mobile app backend, or API app in Azure App Service.</span></span> 

<span data-ttu-id="8284c-105">Zabezpečení v Azure App Service má dvě úrovně:</span><span class="sxs-lookup"><span data-stu-id="8284c-105">Security in Azure App Service has two levels:</span></span> 

* <span data-ttu-id="8284c-106">**Zabezpečení infrastruktury a platformy** -důvěřujete Azure tak, aby měl služby budete muset ve skutečnosti spuštěna věcí bezpečně v cloudu.</span><span class="sxs-lookup"><span data-stu-id="8284c-106">**Infrastructure and platform security** - You trust Azure to have the services you need to actually run things securely in the cloud.</span></span>
* <span data-ttu-id="8284c-107">**Zabezpečení aplikací** -potřebujete bezpečně návrhu aplikace.</span><span class="sxs-lookup"><span data-stu-id="8284c-107">**Application security** - You need to design the app itself securely.</span></span> <span data-ttu-id="8284c-108">To zahrnuje jak integrovat se službou Azure Active Directory, jak spravovat certifikáty a jak byste si ověřit, že může bezpečně kontaktovat u různých služeb.</span><span class="sxs-lookup"><span data-stu-id="8284c-108">This includes how you integrate with Azure Active Directory, how you manage certificates, and how you make sure that you can securely talk to different services.</span></span> 

#### <a name="infrastructure-and-platform-security"></a><span data-ttu-id="8284c-109">Zabezpečení infrastruktury a platformy</span><span class="sxs-lookup"><span data-stu-id="8284c-109">Infrastructure and platform security</span></span>
<span data-ttu-id="8284c-110">Protože udržuje virtuálních počítačích Azure, úložiště, připojení k síti, webové rozhraní, správy a funkcí pro integraci a mnohé další služby App Service je aktivně zabezpečené a zesílené zabezpečení projde intenzivního dodržování předpisů a kontroluje pravidelné spouštění zajistit který:</span><span class="sxs-lookup"><span data-stu-id="8284c-110">Because App Service maintains the Azure VMs, storage, network connections, web frameworks, management and integration features and much more, it is actively secured and hardened and goes through vigorous compliance and checks on a continuous basis to make sure that:</span></span>

* <span data-ttu-id="8284c-111">Aplikace služby App Service jsou izolovány z obou Internet a z dalších zákazníků prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="8284c-111">Your App Service apps are isolated from both the Internet and from the other customers' Azure resources.</span></span>
* <span data-ttu-id="8284c-112">Komunikace mezi aplikaci aplikační služby a další prostředky Azure (např. SQL Database) ve skupině prostředků tajné klíče (například připojovací řetězce) zůstává v rámci Azure a nemá křížové žádné hranice sítě.</span><span class="sxs-lookup"><span data-stu-id="8284c-112">Communication of secrets (e.g. connection strings) between your App Service app and other Azure resources (e.g. SQL Database) in a resource group stays within Azure and doesn't cross any network boundaries.</span></span> <span data-ttu-id="8284c-113">Tajné klíče jsou vždy šifrována.</span><span class="sxs-lookup"><span data-stu-id="8284c-113">Secrets are always encrypted.</span></span>
* <span data-ttu-id="8284c-114">Veškerá komunikace mezi aplikace App Service a externím prostředkům, jako je správa prostředí PowerShell, rozhraní příkazového řádku, sadami SDK služby Azure, rozhraní REST API a hybridní připojení, jsou správně šifrována.</span><span class="sxs-lookup"><span data-stu-id="8284c-114">All communication between your App Service app and external resources, such as PowerShell management, command-line interface, Azure SDKs, REST APIs, and hybrid connections, are properly encrypted.</span></span>
* <span data-ttu-id="8284c-115">24 hodin threat management chrání prostředky služby App Service před malwarem, distributed denial of service (DDoS), man-in-the-middle typu MITM () a dalšími hrozbami.</span><span class="sxs-lookup"><span data-stu-id="8284c-115">24-hour threat management protects App Service resources from malware, distributed denial-of-service (DDoS), man-in-the-middle (MITM), and other threats.</span></span> 

<span data-ttu-id="8284c-116">Další informace o infrastruktury a platformy zabezpečení v Azure najdete v tématu [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/security/).</span><span class="sxs-lookup"><span data-stu-id="8284c-116">For more information on infrastructure and platform security in Azure, see [Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).</span></span>

#### <a name="application-security"></a><span data-ttu-id="8284c-117">Zabezpečení aplikací</span><span class="sxs-lookup"><span data-stu-id="8284c-117">Application security</span></span>
<span data-ttu-id="8284c-118">Sice zodpovídají za zabezpečení infrastruktury a platformy, které vaše aplikace běží v Azure, je vaší povinností zabezpečit vaše vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="8284c-118">While Azure is responsible for securing the infrastructure and platform that your application runs on, it is your responsibility to secure your application itself.</span></span> <span data-ttu-id="8284c-119">Jinými slovy musíte k vývoji, nasadit a spravovat kódu aplikace a obsah zabezpečené způsobem.</span><span class="sxs-lookup"><span data-stu-id="8284c-119">In other words, you need to develop, deploy, and manage your application code and content in a secure way.</span></span> <span data-ttu-id="8284c-120">Bez tohoto kódu aplikace nebo obsah může být zranitelný vůči hrozbám, jako:</span><span class="sxs-lookup"><span data-stu-id="8284c-120">Without this, your application code or content can still be vulnerable to threats such as:</span></span>

* <span data-ttu-id="8284c-121">Injektáž SQL</span><span class="sxs-lookup"><span data-stu-id="8284c-121">SQL Injection</span></span>
* <span data-ttu-id="8284c-122">Zneužití relace</span><span class="sxs-lookup"><span data-stu-id="8284c-122">Session hijacking</span></span>
* <span data-ttu-id="8284c-123">Křížové lokality skriptování</span><span class="sxs-lookup"><span data-stu-id="8284c-123">Cross-site-scripting</span></span>
* <span data-ttu-id="8284c-124">MITM úrovni aplikace</span><span class="sxs-lookup"><span data-stu-id="8284c-124">Application-level MITM</span></span>
* <span data-ttu-id="8284c-125">DDoS úrovni aplikace</span><span class="sxs-lookup"><span data-stu-id="8284c-125">Application-level DDoS</span></span>

<span data-ttu-id="8284c-126">Úplnou diskusi o aspekty zabezpečení webových aplikací je nad rámec tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8284c-126">A full discussion of security considerations for web-based applications is beyond the scope of this document.</span></span> <span data-ttu-id="8284c-127">Jako výchozí bod pro další informace o zabezpečení vaší aplikace, najdete v článku [aplikace otevřete webový projekt zabezpečení (OWASP)](https://www.owasp.org/index.php/Main_Page), konkrétně [prvních 10 projektu.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), které jsou uvedené aktuální top 10 kritické webové aplikace zabezpečení nedostatky, počítáno od OWASP členy.</span><span class="sxs-lookup"><span data-stu-id="8284c-127">As a starting point for further guidance on securing your application, see the [Open Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Main_Page), specifically the [top 10 project.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), which lists the current top 10 critical web application security flaws, as determined by OWASP members.</span></span>

## <a name="perform-penetration-testing-on-your-app"></a><span data-ttu-id="8284c-128">Provedení průnikům testování v aplikaci</span><span class="sxs-lookup"><span data-stu-id="8284c-128">Perform penetration testing on your app</span></span>
<span data-ttu-id="8284c-129">Jedním z nejjednodušších způsobů, jak začít pracovat s testování pro chyby zabezpečení v aplikaci aplikační služby je potřeba použít [integrace s Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) k ohrožení zabezpečení jedním kliknutím kontrolu ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8284c-129">One of the easiest ways to get started with testing for vulnerabilities on your App Service app is to use the [integration with Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) to perform one-click vulnerability scanning on your app.</span></span> <span data-ttu-id="8284c-130">Můžete zobrazit výsledky testů ve zprávě o snadno pochopit a zjistěte, jak opravit jednotlivé chyby zabezpečení s podrobnými pokyny.</span><span class="sxs-lookup"><span data-stu-id="8284c-130">You can view the test results in an easy-to-understand report, and learn how to fix each vulnerability with step-by-step instructions.</span></span>

<span data-ttu-id="8284c-131">Pokud dáváte přednost proveďte vlastní testy průnikům nebo chcete použít jiný skener suite nebo poskytovatele, postupujte podle [Azure průnikům testování procesu schvalování](https://security-forms.azure.com/penetration-testing/terms) a získat předchozí schválení k provedení požadované průnikům testů.</span><span class="sxs-lookup"><span data-stu-id="8284c-131">If you prefer to perform your own penetration tests or want to use another scanner suite or provider, you must follow the [Azure penetration testing approval process](https://security-forms.azure.com/penetration-testing/terms) and obtain prior approval to perform the desired penetration tests.</span></span>

## <span data-ttu-id="8284c-132"><a name="https"></a>Zabezpečené komunikace se zákazníky</span><span class="sxs-lookup"><span data-stu-id="8284c-132"><a name="https"></a> Secure communication with customers</span></span>
<span data-ttu-id="8284c-133">Pokud použijete  **\*. azurewebsites.net** název domény, které jsou vytvořené pro aplikaci aplikační služby, můžete okamžitě pomocí protokolu HTTPS, který získáte certifikát SSL pro všechny  **\*. azurewebsites.net** názvy domén.</span><span class="sxs-lookup"><span data-stu-id="8284c-133">If you use the **\*.azurewebsites.net** domain name created for your App Service app, you can immediately use HTTPS, as an SSL certificate is provided for all **\*.azurewebsites.net** domain names.</span></span> <span data-ttu-id="8284c-134">Pokud váš web používá [vlastní název domény](app-service-web-tutorial-custom-domain.md), můžete nahrát certifikát SSL k [povolit protokol HTTPS](app-service-web-tutorial-custom-ssl.md) pro vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="8284c-134">If your site uses a [custom domain name](app-service-web-tutorial-custom-domain.md), you can upload an SSL certificate to [enable HTTPS](app-service-web-tutorial-custom-ssl.md) for the custom domain.</span></span>

<span data-ttu-id="8284c-135">Povolení [HTTPS](https://en.wikipedia.org/wiki/HTTPS) může pomoct chránit před útoky MITM na komunikace mezi vaší aplikace a její uživatele.</span><span class="sxs-lookup"><span data-stu-id="8284c-135">Enabling [HTTPS](https://en.wikipedia.org/wiki/HTTPS) can help protect against MITM attacks on the communication between your app and its users.</span></span>

## <a name="secure-data-tier"></a><span data-ttu-id="8284c-136">Zabezpečení datové vrstvy</span><span class="sxs-lookup"><span data-stu-id="8284c-136">Secure data tier</span></span>
<span data-ttu-id="8284c-137">Služby App Service vysoce se integruje se službou SQL Database, tak, aby všechny připojovací řetězce jsou šifrované v panelu a jsou dešifrovat jenom na virtuálním počítači, na kterém aplikace běží na *a* pouze, při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="8284c-137">App Service highly integrates with SQL Database, such that all the connection strings are encrypted across the board and are only decrypted on the VM that the app runs on *and* only when the app runs.</span></span> <span data-ttu-id="8284c-138">Kromě toho Azure SQL Database zahrnuje mnoho funkcí zabezpečení a pomáhá vám zabezpečit vaše data aplikací z internetových hrozeb, včetně [šifrování na rest](https://msdn.microsoft.com/library/dn948096.aspx), [vždy šifrována](https://msdn.microsoft.com/library/mt163865.aspx), [ Dynamická Data maskování](../sql-database/sql-database-dynamic-data-masking-get-started.md), a [detekce hrozby](../sql-database/sql-database-threat-detection.md).</span><span class="sxs-lookup"><span data-stu-id="8284c-138">In addition, Azure SQL Database includes many security features to help you secure your application data from cyber threats, including [at-rest encryption](https://msdn.microsoft.com/library/dn948096.aspx), [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx), [Dynamic Data Masking](../sql-database/sql-database-dynamic-data-masking-get-started.md), and [Threat Detection](../sql-database/sql-database-threat-detection.md).</span></span> <span data-ttu-id="8284c-139">Pokud máte citlivá data nebo požadavky na dodržování předpisů, přečtěte si téma [zabezpečení databáze SQL](../sql-database/sql-database-security-overview.md) Další informace o tom, jak zabezpečit vaše data.</span><span class="sxs-lookup"><span data-stu-id="8284c-139">If you have sensitive data or compliance requirements, see [Securing your SQL Database](../sql-database/sql-database-security-overview.md) for more information on how to secure your data.</span></span>

<span data-ttu-id="8284c-140">Pokud používáte poskytovatele databáze třetích stran, jako je například ClearDB, byste se měli obrátit se zprostředkovatele dokumentace přímo na osvědčené postupy zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8284c-140">If you use a third-party database provider, such as ClearDB, you should consult with the provider's documentation directly on security best practices.</span></span>  

## <span data-ttu-id="8284c-141"><a name="develop"></a>Zabezpečený vývoj a nasazení</span><span class="sxs-lookup"><span data-stu-id="8284c-141"><a name="develop"></a> Secure development and deployment</span></span>
### <a name="publishing-profiles-and-publish-settings"></a><span data-ttu-id="8284c-142">Publikování profily a nastavení publikování</span><span class="sxs-lookup"><span data-stu-id="8284c-142">Publishing profiles and publish settings</span></span>
<span data-ttu-id="8284c-143">Při vývoji aplikací, provádění úloh správy, nebo automatizaci úloh pomocí nástrojů, jako třeba **Visual Studio**, **Web Matrix**, **prostředí Azure PowerShell** nebo  **Rozhraní příkazového řádku Azure (Azure CLI)**, můžete použít buď *nastavení publikování* souboru nebo *profil publikování*.</span><span class="sxs-lookup"><span data-stu-id="8284c-143">When developing applications, performing management tasks, or automating tasks using utilities such as **Visual Studio**, **Web Matrix**, **Azure PowerShell** or the **Azure Command-Line Interface (Azure CLI)**, you can use either a *publish settings* file or a *publishing profile*.</span></span> <span data-ttu-id="8284c-144">Oba typy souborů je ověření pomocí Azure a by měly být zabezpečeny před neoprávněným přístupem.</span><span class="sxs-lookup"><span data-stu-id="8284c-144">Both file types authenticate you with Azure, and should be secured to prevent unauthorized access.</span></span>

* <span data-ttu-id="8284c-145">A **nastavení publikování** soubor obsahuje</span><span class="sxs-lookup"><span data-stu-id="8284c-145">A **publish settings** file contains</span></span>
  
  * <span data-ttu-id="8284c-146">Svoje ID předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="8284c-146">Your Azure subscription ID</span></span>
  * <span data-ttu-id="8284c-147">Certifikát správy, který umožňuje provádět úlohy správy pro vaše předplatné *bez nutnosti poskytnout názvu účtu nebo hesla*.</span><span class="sxs-lookup"><span data-stu-id="8284c-147">A management certificate that allows you to perform management tasks for your subscription *without having to provide an account name or password*.</span></span>
* <span data-ttu-id="8284c-148">A **profil publikování** soubor obsahuje</span><span class="sxs-lookup"><span data-stu-id="8284c-148">A **publishing profile** file contains</span></span>
  
  * <span data-ttu-id="8284c-149">Informace pro publikování do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="8284c-149">Information for publishing to your app</span></span>

<span data-ttu-id="8284c-150">Pokud používáte nástroj, který používá soubor nastavení publikování nebo soubor profil publikování, importovat soubor obsahující nastavení publikování nebo profilu do nástroje a potom **odstranit** soubor.</span><span class="sxs-lookup"><span data-stu-id="8284c-150">If you use a utility that uses a publish settings file or publish profile file, import the file containing the publish settings or profile into the utility and then **delete** the file.</span></span> <span data-ttu-id="8284c-151">Pokud je nutné zachovat soubor sdílet s ostatními uživateli práce na projekt například uložit na bezpečném místě, jako *šifrované* adresáři s omezenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="8284c-151">If you must keep the file, to share with others working on the project for example, store it in a secure location such as an *encrypted* directory with restricted permissions.</span></span>

<span data-ttu-id="8284c-152">Kromě toho můžete Ujistěte se, že jsou zabezpečené importované přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="8284c-152">Additionally, you should make sure the imported credentials are secured.</span></span> <span data-ttu-id="8284c-153">Například **prostředí Azure PowerShell** a **rozhraní příkazového řádku Azure (Azure CLI)** obě ukládání importované informace ve vaší **domovský adresář** ( *~*  v systémech Linux nebo OS X a */users/uživatelské_jméno* v systémech Windows.) Vyšší bezpečnost, budete pravděpodobně chtít **šifrování** těchto umístění pomocí šifrování nástrojů, které jsou k dispozici pro operační systém.</span><span class="sxs-lookup"><span data-stu-id="8284c-153">For example, **Azure PowerShell** and the **Azure Command-Line Interface (Azure CLI)** both store imported information in your **home directory** (*~* on Linux or OS X systems and */users/yourusername* on Windows systems.) For extra security, you may wish to **encrypt** these locations using encryption tools available for your operating system.</span></span>

### <a name="configuration-settings-and-connection-strings"></a><span data-ttu-id="8284c-154">Nastavení konfigurace a připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="8284c-154">Configuration settings, and connection strings</span></span>
<span data-ttu-id="8284c-155">Je obvyklé, že k uložení připojovací řetězce, pověření pro ověření a jinými důvěrnými informacemi v konfiguračních souborech.</span><span class="sxs-lookup"><span data-stu-id="8284c-155">It's common practice to store connection strings, authentication credentials, and other sensitive information in configuration files.</span></span> <span data-ttu-id="8284c-156">Tyto soubory mohou být bohužel zveřejněné na webu, nebo zaškrtnutí do veřejného úložiště, vystavení tyto informace.</span><span class="sxs-lookup"><span data-stu-id="8284c-156">Unfortunately, these files may be exposed on your website, or checked into a public repository, exposing this information.</span></span> <span data-ttu-id="8284c-157">Jednoduché hledání na [Githubu](https://github.com), může například odkrýt obrovském množství konfigurační soubory s zveřejněné tajných klíčů v veřejného úložiště.</span><span class="sxs-lookup"><span data-stu-id="8284c-157">A simple search on [GitHub](https://github.com), for example, can uncover countless configuration files with exposed secrets in the public repositories.</span></span>

<span data-ttu-id="8284c-158">Osvědčeným postupem je udržovat tyto informace z vaší aplikace konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="8284c-158">The best practice is to keep this information out of your app's configuration files.</span></span> <span data-ttu-id="8284c-159">App Service umožňuje ukládat informace o konfiguraci jako součást běhové prostředí jako **nastavení aplikace** a **připojovací řetězce**.</span><span class="sxs-lookup"><span data-stu-id="8284c-159">App Service lets you store configuration information as part of the runtime environment as **app settings** and **connection strings**.</span></span> <span data-ttu-id="8284c-160">Hodnoty jsou umístěny do vaší aplikace za běhu prostřednictvím *proměnné prostředí* pro většinu programovacích jazyků.</span><span class="sxs-lookup"><span data-stu-id="8284c-160">The values are exposed to your application at runtime through *environment variables* for most programming languages.</span></span> <span data-ttu-id="8284c-161">Pro aplikace .NET jsou tyto hodnoty vsunout do vaší konfigurace .NET za běhu.</span><span class="sxs-lookup"><span data-stu-id="8284c-161">For .NET applications, these values are injected into your .NET configuration at runtime.</span></span> <span data-ttu-id="8284c-162">Kromě těchto situacích se zůstanou šifrovaná tato nastavení konfigurace není-li zobrazit nebo nakonfigurovat je pomocí [portálu Azure](https://portal.azure.com) nebo nástroje, jako je například PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8284c-162">Apart from these situations, these configuration settings will remain encrypted unless you view or configure them using the [Azure Portal](https://portal.azure.com) or utilities such as PowerShell or the Azure CLI.</span></span> 

<span data-ttu-id="8284c-163">Ukládání informací o konfiguraci ve službě App Service umožňuje pro aplikace Správce zamknout citlivé informace pro aplikace pro produkční.</span><span class="sxs-lookup"><span data-stu-id="8284c-163">Storing configuration information in App Service makes it possible for the app's administrator to lock down sensitive information for the production apps.</span></span> <span data-ttu-id="8284c-164">Vývojáři můžou pro vývoj aplikací používat samostatnou sadu nastavení konfigurace a nastavení může být automaticky nahrazována nastavení ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="8284c-164">Developers can use a separate set of configuration settings for app development and the settings can be automatically superseded by the settings configured in App Service.</span></span> <span data-ttu-id="8284c-165">Ani vývojáři potřebovat znát tajné klíče, nakonfigurované pro produkční aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8284c-165">Not even the developers need to know the secrets configured for the production app.</span></span> <span data-ttu-id="8284c-166">Další informace o konfiguraci nastavení aplikace a připojovací řetězce ve službě App Service najdete v tématu [konfigurace webové aplikace](web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="8284c-166">For more information on configuring app settings and connection strings in App Service, see [Configuring web apps](web-sites-configure.md).</span></span>

### <a name="ftps"></a><span data-ttu-id="8284c-167">FTPS</span><span class="sxs-lookup"><span data-stu-id="8284c-167">FTPS</span></span>
<span data-ttu-id="8284c-168">Aplikační služba Azure poskytuje zabezpečený FTP přístup k systému souborů pro vaši aplikaci prostřednictvím **FTPS**.</span><span class="sxs-lookup"><span data-stu-id="8284c-168">Azure App Service provides secure FTP access to the file system for your app through **FTPS**.</span></span> <span data-ttu-id="8284c-169">To umožňuje bezpečný přístup k kód aplikace na webovou aplikaci, jakož i diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="8284c-169">This allows you to securely access the application code on the web app as well as diagnostics logs.</span></span> <span data-ttu-id="8284c-170">Doporučuje se vždycky používat FTPS místo FTP.</span><span class="sxs-lookup"><span data-stu-id="8284c-170">It is recommended that you always use FTPS instead of FTP.</span></span> 

<span data-ttu-id="8284c-171">Odkaz FTPS pro vaši aplikaci najdete v následujících krocích:</span><span class="sxs-lookup"><span data-stu-id="8284c-171">The FTPS link for your app can be found with the following steps:</span></span>

1. <span data-ttu-id="8284c-172">Otevřete [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8284c-172">Open the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8284c-173">Vyberte **procházet všechny**.</span><span class="sxs-lookup"><span data-stu-id="8284c-173">Select **Browse All**.</span></span>
3. <span data-ttu-id="8284c-174">Z **Procházet** vyberte **App Services**.</span><span class="sxs-lookup"><span data-stu-id="8284c-174">From the **Browse** blade, select **App Services**.</span></span>
4. <span data-ttu-id="8284c-175">Z **App Services** okně, vyberte požadované aplikace.</span><span class="sxs-lookup"><span data-stu-id="8284c-175">From the **App Services** blade, Select the desired app.</span></span>
5. <span data-ttu-id="8284c-176">V okně aplikace, vyberte **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="8284c-176">From the app's blade, select **All settings**.</span></span>
6. <span data-ttu-id="8284c-177">Z **nastavení** vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="8284c-177">From the **Settings** blade, select **Properties**.</span></span>
7. <span data-ttu-id="8284c-178">FTP a FTPS odkazy jsou k dispozici na **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="8284c-178">The FTP and FTPS links are provided on the **Settings** blade.</span></span> 

<span data-ttu-id="8284c-179">Další informace o FTPS najdete v tématu [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).</span><span class="sxs-lookup"><span data-stu-id="8284c-179">For more information on FTPS, see [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8284c-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8284c-180">Next steps</span></span>
<span data-ttu-id="8284c-181">Další informace o zabezpečení Azure platformy, informace o vytváření sestav **zneužití nebo bezpečnostního incidentu**, nebo k informování společnosti Microsoft, který budete provádět **průnikům testování** vašeho webu najdete v části zabezpečení [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).</span><span class="sxs-lookup"><span data-stu-id="8284c-181">For more information on the security of the Azure platform, information on reporting a **security incident or abuse**, or to inform Microsoft that you will be performing **penetration testing** of your site, see the security section of the [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).</span></span>

<span data-ttu-id="8284c-182">Další informace o **web.config** nebo **applicationhost.config** souborů v aplikacích App Service naleznete v [možnosti konfigurace odemkne ve službě Azure App Service web apps](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).</span><span class="sxs-lookup"><span data-stu-id="8284c-182">For more information on **web.config** or **applicationhost.config** files in App Service apps, see [Configuration options unlocked in Azure App Service web apps](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).</span></span>

<span data-ttu-id="8284c-183">Informace o protokolování informace pro aplikace služby App Service, která mohou být užitečné při rozpoznání útoků, najdete v části [povolit protokolování diagnostiky](web-sites-enable-diagnostic-log.md).</span><span class="sxs-lookup"><span data-stu-id="8284c-183">For information on logging information for App Service apps, which may be useful in detecting attacks, see [Enable diagnostic logging](web-sites-enable-diagnostic-log.md).</span></span>

> [!NOTE]
> <span data-ttu-id="8284c-184">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="8284c-184">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter app in App Service.</span></span> <span data-ttu-id="8284c-185">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="8284c-185">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="8284c-186">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="8284c-186">What's changed</span></span>
* <span data-ttu-id="8284c-187">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="8284c-187">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>
