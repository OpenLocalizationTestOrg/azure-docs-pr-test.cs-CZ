---
title: "aaaMFA software development kit vlastních aplikací | Microsoft Docs"
description: "Tento článek ukazuje, jak toodownload a používání hello Azure MFA SDK tooenable dvoustupňové ověřování pro vaše vlastní aplikace."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 1c152f67-be02-42a5-a0c7-246fb6b34377
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 10e02e844bf3928575bfca79dbc34717a31a08b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a><span data-ttu-id="6ffa7-103">Vytváření služby Multi-Factor Authentication do vlastní aplikace (SDK)</span><span class="sxs-lookup"><span data-stu-id="6ffa7-103">Building Multi-Factor Authentication into Custom Apps (SDK)</span></span>

<span data-ttu-id="6ffa7-104">transakce procesy aplikací v klientovi služby Azure AD nebo vám Hello Azure Multi-Factor Authentication Software Development Kit (SDK) umožňuje vytvářet dvoustupňové ověření přímo do hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-104">hello Azure Multi-Factor Authentication Software Development Kit (SDK) lets you build two-step verification directly into hello sign-in or transaction processes of applications in your Azure AD tenant.</span></span>

<span data-ttu-id="6ffa7-105">Hello SDK služby Multi-Factor Authentication je k dispozici pro C#, Visual Basic (.NET), Java, Perl, PHP a Ruby.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-105">hello Multi-Factor Authentication SDK is available for C#, Visual Basic (.NET), Java, Perl, PHP, and Ruby.</span></span> <span data-ttu-id="6ffa7-106">Hello SDK poskytuje dynamické obálku kolem dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-106">hello SDK provides a thin wrapper around two-step verification.</span></span> <span data-ttu-id="6ffa7-107">Obsahuje všechno, co že potřebujete toowrite kódu, včetně soubory komentáři zdrojového kódu, například soubory a podrobné souboru ReadMe.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-107">It includes everything you need toowrite your code, including commented source code files, example files, and a detailed ReadMe file.</span></span> <span data-ttu-id="6ffa7-108">Každý SDK zahrnuje také certifikát a soukromý klíč pro šifrování transakce, které jsou jedinečné tooyour zprostředkovatel vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-108">Each SDK also includes a certificate and private key for encrypting transactions that are unique tooyour Multi-Factor Authentication Provider.</span></span> <span data-ttu-id="6ffa7-109">Tak dlouho, dokud máte poskytovatele, si můžete stáhnout hello SDK v jako v mnoha jazycích a formáty podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-109">As long as you have a provider, you can download hello SDK in as many languages and formats as you need.</span></span>

<span data-ttu-id="6ffa7-110">Struktura Hello hello rozhraní API v hello Multi-Factor Authentication SDK je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-110">hello structure of hello APIs in hello Multi-Factor Authentication SDK is simple.</span></span> <span data-ttu-id="6ffa7-111">Ujistěte se, jedné funkce volání rozhraní API tooan s parametry Multi-Factor možnost hello (jako jsou ověřování režimu) a dat uživatele (například hello telefonní číslo toocall nebo číslo toovalidate hello kód PIN).</span><span class="sxs-lookup"><span data-stu-id="6ffa7-111">Make a single function call tooan API with hello multi-factor option parameters (like verification mode) and user data (like hello telephone number toocall or hello PIN number toovalidate).</span></span> <span data-ttu-id="6ffa7-112">Hello rozhraní API převede volání funkce hello do toohello žádosti webové služby založené na cloudu Azure Multi-Factor Authentication Service.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-112">hello APIs translate hello function call into web services requests toohello cloud-based Azure Multi-Factor Authentication Service.</span></span> <span data-ttu-id="6ffa7-113">Všechna volání musí obsahovat odkaz toohello privátní certifikát, který je zahrnuta v každé sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-113">All calls must include a reference toohello private certificate that is included in every SDK.</span></span>

<span data-ttu-id="6ffa7-114">Protože hello rozhraní API není k dispozici přístup toousers zaregistrované v Azure Active Directory, je třeba zadat informace o uživateli v souboru nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-114">Because hello APIs do not have access toousers registered in Azure Active Directory, you must provide user information in a file or database.</span></span> <span data-ttu-id="6ffa7-115">Navíc hello rozhraní API neposkytují funkce správy zápisu nebo uživatele, proto musíte toobuild tyto procesy do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-115">Also, hello APIs do not provide enrollment or user management features, so you need toobuild these processes into your application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ffa7-116">toodownload hello SDK, je nutné toocreate poskytovatele Azure Multi-Factor Auth i v případě, že máte licence Azure MFA, AAD Premium nebo EMS.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-116">toodownload hello SDK, you need toocreate an Azure Multi-Factor Auth Provider even if you have Azure MFA, AAD Premium, or EMS licenses.</span></span> <span data-ttu-id="6ffa7-117">Pokud jste pro tento účel vytvořit poskytovatele Azure Multi-Factor Auth a už máte licence, ujistěte se, zda toocreate hello zprostředkovatele s hello **za povoleného uživatele** modelu.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-117">If you create an Azure Multi-Factor Auth Provider for this purpose and already have licenses, make sure toocreate hello Provider with hello **Per Enabled User** model.</span></span> <span data-ttu-id="6ffa7-118">Pak propojte hello zprostředkovatele toohello adresář, který obsahuje hello licence Azure MFA, Azure AD Premium nebo EMS.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-118">Then, link hello Provider toohello directory that contains hello Azure MFA, Azure AD Premium, or EMS licenses.</span></span> <span data-ttu-id="6ffa7-119">Tato konfigurace zajistí, že se pouze účtují Pokud máte více jedinečných uživatelů, kteří pomocí hello SDK než hello počet licencí, které vlastníte.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-119">This configuration ensures that you are only billed if you have more unique users using hello SDK than hello number of licenses you own.</span></span>


## <a name="download-hello-sdk"></a><span data-ttu-id="6ffa7-120">Stáhnout hello SDK</span><span class="sxs-lookup"><span data-stu-id="6ffa7-120">Download hello SDK</span></span>
<span data-ttu-id="6ffa7-121">Stahování hello SDK Azure Multi-Factor vyžaduje [zprostředkovatel vícefaktorového ověřování Azure](multi-factor-authentication-get-started-auth-provider.md).</span><span class="sxs-lookup"><span data-stu-id="6ffa7-121">Downloading hello Azure Multi-Factor SDK requires an [Azure Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md).</span></span>  <span data-ttu-id="6ffa7-122">To vyžaduje úplné předplatné, i když jsou ve vlastnictví licence Azure MFA, Azure AD Premium nebo Enterprise Mobility Suite.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-122">This requires a full Azure subscription, even if Azure MFA, Azure AD Premium, or Enterprise Mobility Suite licenses are owned.</span></span>  <span data-ttu-id="6ffa7-123">hello toodownload SDK, přejděte toohello Multi-Factor Management Portal.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-123">toodownload hello SDK, navigate toohello Multi-Factor Management Portal.</span></span> <span data-ttu-id="6ffa7-124">Můžete uživatele oslovit hello portál Správa hello zprostředkovatel vícefaktorového ověřování přímo, nebo kliknutím hello **"Přejděte toohello portál"** odkaz na stránce nastavení služby hello vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-124">You can reach hello portal either by managing hello Multi-Factor Auth Provider directly, or by clicking hello **"Go toohello portal"** link on hello MFA service settings page.</span></span>

### <a name="download-from-hello-azure-classic-portal"></a><span data-ttu-id="6ffa7-125">Stáhnout z portálu Azure classic hello</span><span class="sxs-lookup"><span data-stu-id="6ffa7-125">Download from hello Azure classic portal</span></span>
1. <span data-ttu-id="6ffa7-126">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako správce.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-126">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="6ffa7-127">Na levé straně hello vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-127">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="6ffa7-128">Na stránce služby Active Directory hello v horní vyberte hello **zprostředkovatelé vícefaktorového ověřování**</span><span class="sxs-lookup"><span data-stu-id="6ffa7-128">On hello Active Directory page, at hello top select **Multi-Factor Auth Providers**</span></span>
4. <span data-ttu-id="6ffa7-129">V dolní části hello vyberte **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-129">At hello bottom select **Manage**.</span></span> <span data-ttu-id="6ffa7-130">Otevře se nová stránka.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-130">A new page opens.</span></span>
5. <span data-ttu-id="6ffa7-131">Na hello ponechány na dolním hello, klikněte na tlačítko **SDK**.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-131">On hello left, at hello bottom, click **SDK**.</span></span>
   <span data-ttu-id="6ffa7-132"><center>![Stahování](./media/multi-factor-authentication-sdk/download.png)</center></span><span class="sxs-lookup"><span data-stu-id="6ffa7-132"><center>![Download](./media/multi-factor-authentication-sdk/download.png)</center></span></span>
6. <span data-ttu-id="6ffa7-133">Vyberte hello jazyk a klikněte na jednu hello související odkazy na stažení.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-133">Select hello language you want and click one hello associated download links.</span></span>
7. <span data-ttu-id="6ffa7-134">Uložte hello stahování.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-134">Save hello download.</span></span>

### <a name="download-from-hello-service-settings"></a><span data-ttu-id="6ffa7-135">Stáhnout z nastavení služby hello</span><span class="sxs-lookup"><span data-stu-id="6ffa7-135">Download from hello service settings</span></span>
1. <span data-ttu-id="6ffa7-136">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako správce.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-136">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="6ffa7-137">Na levé straně hello vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-137">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="6ffa7-138">Dvakrát klikněte na svoji instanci služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-138">Double-click your instance of Azure AD.</span></span>
4. <span data-ttu-id="6ffa7-139">V nahoře klikněte na hello **konfigurace**</span><span class="sxs-lookup"><span data-stu-id="6ffa7-139">At hello top click **Configure**</span></span>
5. <span data-ttu-id="6ffa7-140">V části ověřování Multi-Factor authentication, vyberte **spravovat nastavení služby**
   ![stáhnout](./media/multi-factor-authentication-sdk/download2.png)</span><span class="sxs-lookup"><span data-stu-id="6ffa7-140">Under multi-factor authentication, select **Manage service settings**
![Download](./media/multi-factor-authentication-sdk/download2.png)</span></span>
6. <span data-ttu-id="6ffa7-141">Na stránce nastavení služby hello klikněte v hello dolní části obrazovky hello **přejděte toohello portál**.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-141">On hello services settings page, at hello bottom of hello screen click **Go toohello portal**.</span></span> <span data-ttu-id="6ffa7-142">Otevře se nová stránka.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-142">A new page opens.</span></span>
   <span data-ttu-id="6ffa7-143">![Stáhnout](./media/multi-factor-authentication-sdk/download3a.png)</span><span class="sxs-lookup"><span data-stu-id="6ffa7-143">![Download](./media/multi-factor-authentication-sdk/download3a.png)</span></span>
7. <span data-ttu-id="6ffa7-144">Na hello ponechány na dolním hello, klikněte na tlačítko **SDK**.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-144">On hello left, at hello bottom, click **SDK**.</span></span>
8. <span data-ttu-id="6ffa7-145">Vyberte hello jazyk a klikněte na jednu hello související odkazy na stažení.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-145">Select hello language you want and click one hello associated download links.</span></span>
9. <span data-ttu-id="6ffa7-146">Uložte hello stahování.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-146">Save hello download.</span></span>

## <a name="whats-in-hello-sdk"></a><span data-ttu-id="6ffa7-147">Co je v hello SDK</span><span class="sxs-lookup"><span data-stu-id="6ffa7-147">What's in hello SDK</span></span>
<span data-ttu-id="6ffa7-148">Hello SDK zahrnuje hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6ffa7-148">hello SDK includes hello following items:</span></span>

* <span data-ttu-id="6ffa7-149">**SOUBOR README**.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-149">**README**.</span></span> <span data-ttu-id="6ffa7-150">Vysvětluje, jak toouse hello rozhraní API vícefaktorového ověřování v nové nebo existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-150">Explains how toouse hello Multi-Factor Authentication APIs in a new or existing application.</span></span>
* <span data-ttu-id="6ffa7-151">**Zdrojové soubory** pro službu Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="6ffa7-151">**Source files** for Multi-Factor Authentication</span></span>
* <span data-ttu-id="6ffa7-152">**Klientský certifikát** použijte toocommunicate s hello službou Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="6ffa7-152">**Client certificate** that you use toocommunicate with hello Multi-Factor Authentication service</span></span>
* <span data-ttu-id="6ffa7-153">**Privátní klíč** hello certifikátu</span><span class="sxs-lookup"><span data-stu-id="6ffa7-153">**Private key** for hello certificate</span></span>
* <span data-ttu-id="6ffa7-154">**Výsledky volání.**</span><span class="sxs-lookup"><span data-stu-id="6ffa7-154">**Call results.**</span></span> <span data-ttu-id="6ffa7-155">Seznam kódy výsledků volání.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-155">A list of call result codes.</span></span> <span data-ttu-id="6ffa7-156">tooopen tento soubor, použijte aplikaci s formátování textu, například WordPad.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-156">tooopen this file, use an application with text formatting, such as WordPad.</span></span> <span data-ttu-id="6ffa7-157">Použití hello tootest kódy výsledků volání a řešení potíží s hello implementaci vícefaktorového ověřování ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-157">Use hello call result codes tootest and troubleshoot hello implementation of Multi-Factor Authentication in your application.</span></span> <span data-ttu-id="6ffa7-158">Nejsou ověřování stavové kódy.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-158">They are not authentication status codes.</span></span>
* <span data-ttu-id="6ffa7-159">**Příklady.**</span><span class="sxs-lookup"><span data-stu-id="6ffa7-159">**Examples.**</span></span> <span data-ttu-id="6ffa7-160">Ukázkový kód pro základní pracovní implementaci služby Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-160">Sample code for a basic working implementation of Multi-Factor Authentication.</span></span>

> [!WARNING]
> <span data-ttu-id="6ffa7-161">Hello klientský certifikát je jedinečný privátní certifikát, který byl vygenerován speciálně pro vás.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-161">hello client certificate is a unique private certificate that was generated especially for you.</span></span> <span data-ttu-id="6ffa7-162">Sdílené složky nebo ztratit tento soubor.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-162">Do not share or lose this file.</span></span> <span data-ttu-id="6ffa7-163">Je vaše klíče tooensuring hello zabezpečení komunikace se službou Multi-Factor Authentication hello.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-163">It’s your key tooensuring hello security of your communications with hello Multi-Factor Authentication service.</span></span>

## <a name="code-sample"></a><span data-ttu-id="6ffa7-164">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="6ffa7-164">Code sample</span></span>
<span data-ttu-id="6ffa7-165">Tento příklad ukazuje, jak toouse hello rozhraní API v hello sady SDK Azure Multi-Factor Authentication tooadd standardní režim hlasového volat ověření tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-165">This code sample shows you how toouse hello APIs in hello Azure Multi-Factor Authentication SDK tooadd standard mode voice call verification tooyour application.</span></span> <span data-ttu-id="6ffa7-166">Je standardní režim telefonního hovoru, který uživatel odpoví tooby stisknutím klávesy # hello hello.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-166">Standard mode is a telephone call that hello user responds tooby pressing hello # key.</span></span>

<span data-ttu-id="6ffa7-167">Tento příklad používá hello C# .NET 2.0 Multi-Factor Authentication SDK v základní aplikace ASP.NET pomocí jazyka C# logiku na straně serveru, ale hello proces je podobný v dalších jazycích.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-167">This example uses hello C# .NET 2.0 Multi-Factor Authentication SDK in a basic ASP.NET application with C# server-side logic, but hello process is similar in other languages.</span></span> <span data-ttu-id="6ffa7-168">Protože hello SDK obsahuje zdrojové soubory, není spustitelné soubory, lze vytvořit hello soubory a odkazujte na ně nebo je přímo do aplikace zahrnout.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-168">Because hello SDK includes source files, not executable files, you can build hello files and reference them or include them directly in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="6ffa7-169">Při implementaci vícefaktorového ověřování, použijte další metody hello (telefonního hovoru nebo textové zprávy) jako sekundární nebo terciární ověření toosupplement vaší primární metoda ověřování (uživatelské jméno a heslo).</span><span class="sxs-lookup"><span data-stu-id="6ffa7-169">When implementing Multi-Factor Authentication, use hello additional methods (phone call or text message) as secondary or tertiary verification toosupplement your primary authentication method (username and password).</span></span> <span data-ttu-id="6ffa7-170">Tyto metody se nedají jako primární metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-170">These methods are not designed as primary authentication methods.</span></span>

### <a name="code-sample-overview"></a><span data-ttu-id="6ffa7-171">Přehled ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="6ffa7-171">Code Sample Overview</span></span>
<span data-ttu-id="6ffa7-172">Tento ukázkový kód pro jednoduchou webovou aplikaci ukázku používá telefonního hovoru s ověřováním uživatele # klíče odpovědi tooverify hello.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-172">This sample code for a simple web demo application uses a telephone call with a # key response tooverify hello user's authentication.</span></span> <span data-ttu-id="6ffa7-173">Tento faktor telefonní hovor je ověřování službou Multi-Factor Authentication označuje jako standardní režim.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-173">This telephone call factor is known in Multi-Factor Authentication as standard mode.</span></span>

<span data-ttu-id="6ffa7-174">kódu na straně klienta Hello neobsahuje žádné elementy specifické pro službu Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-174">hello client-side code does not include any Multi-Factor Authentication-specific elements.</span></span> <span data-ttu-id="6ffa7-175">Protože hello další faktory ověřování jsou nezávislé na hello primární ověřování, můžete je přidat beze změny hello existujícího přihlašování rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-175">Because hello additional authentication factors are independent of hello primary authentication, you can add them without changing hello existing sign-on interface.</span></span> <span data-ttu-id="6ffa7-176">Hello rozhraní API v hello SDK Multi-Factor umožňují přizpůsobit hello uživatelské prostředí, ale pravděpodobně nebudete potřebovat toochange nic vůbec.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-176">hello APIs in hello Multi-Factor SDK let you customize hello user experience, but you might not need toochange anything at all.</span></span>

<span data-ttu-id="6ffa7-177">kódu na straně serveru Hello přidá standardní režim ověřování v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-177">hello server-side code adds standard-mode authentication in Step 2.</span></span> <span data-ttu-id="6ffa7-178">Vytvoří objekt PfAuthParams s hello parametry, které jsou požadovány pro standardní režim ověřování: uživatelské jméno, telefonní číslo a režim a hello cesta toohello klientský certifikát (CertFilePath), který je požadován při každém volání.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-178">It creates a PfAuthParams object with hello parameters that are required for standard-mode verification: username, telephone number, and mode, and hello path toohello client certificate (CertFilePath), which is required in each call.</span></span> <span data-ttu-id="6ffa7-179">Ukázka všech parametrů v PfAuthParams, najdete v části hello příklad souboru v hello SDK.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-179">For a demonstration of all parameters in PfAuthParams, see hello Example file in hello SDK.</span></span>

<span data-ttu-id="6ffa7-180">V dalším kroku hello kód předá hello PfAuthParams objekt toohello pf_authenticate() funkce.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-180">Next, hello code passes hello PfAuthParams object toohello pf_authenticate() function.</span></span> <span data-ttu-id="6ffa7-181">Návratová hodnota Hello označuje hello úspěšné nebo neúspěšné ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-181">hello return value indicates hello success or failure of hello authentication.</span></span> <span data-ttu-id="6ffa7-182">Hello parametry, callStatus a ID chyby, obsahuje informace o další volání výsledek.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-182">hello out parameters, callStatus, and errorID, contain additional call result information.</span></span> <span data-ttu-id="6ffa7-183">kódy výsledků volání Hello jsou popsané v souboru výsledků volání hello v hello SDK.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-183">hello call result codes are documented in hello call results file in hello SDK.</span></span>

<span data-ttu-id="6ffa7-184">Tato minimální implementace může být napsán v pár řádků.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-184">This minimal implementation can be written in a few lines.</span></span> <span data-ttu-id="6ffa7-185">V produkčním kódu, bude však zahrnovat sofistikovanější zpracování chyb, kód další databáze a vylepšené uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-185">However, in production code, you would include more sophisticated error handling, additional database code, and an enhanced user experience.</span></span>

### <a name="web-client-code"></a><span data-ttu-id="6ffa7-186">Kódu klienta webové</span><span class="sxs-lookup"><span data-stu-id="6ffa7-186">Web Client Code</span></span>
<span data-ttu-id="6ffa7-187">Hello následuje kódu klienta webové stránky ukázku.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-187">hello following is web client code for a demo page.</span></span>

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a><span data-ttu-id="6ffa7-188">Kód na straně serveru</span><span class="sxs-lookup"><span data-stu-id="6ffa7-188">Server-Side Code</span></span>
<span data-ttu-id="6ffa7-189">V hello následující kódu na straně serveru je Multi-Factor Authentication nakonfigurovat a spustit v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-189">In hello following server-side code, Multi-Factor Authentication is configured and run in Step 2.</span></span> <span data-ttu-id="6ffa7-190">Standardní režim (MODE_STANDARD) je, že uživatel hello toowhich telefonní hovor odpoví stisknutím klávesy # hello.</span><span class="sxs-lookup"><span data-stu-id="6ffa7-190">Standard mode (MODE_STANDARD) is a telephone call toowhich hello user responds by pressing hello # key.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate hello username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from hello user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains hello private key for hello client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }
