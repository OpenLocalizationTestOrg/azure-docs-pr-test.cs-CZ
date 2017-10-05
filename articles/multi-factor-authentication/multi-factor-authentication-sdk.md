---
title: "MFA software development kit vlastních aplikací | Microsoft Docs"
description: "Tento článek ukazuje, jak stáhnout a použít Azure MFA SDK k povolení dvoustupňové ověřování pro vaše vlastní aplikace."
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
ms.openlocfilehash: 281f9c61a30a20027f69808600373aa272255ef6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a><span data-ttu-id="ff971-103">Vytváření služby Multi-Factor Authentication do vlastní aplikace (SDK)</span><span class="sxs-lookup"><span data-stu-id="ff971-103">Building Multi-Factor Authentication into Custom Apps (SDK)</span></span>

<span data-ttu-id="ff971-104">Azure Multi-Factor Authentication Software Development Kit (SDK) umožňují vytvářet dvoustupňové ověření přímo do procesy přihlášení nebo transakci aplikací v klientovi služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff971-104">The Azure Multi-Factor Authentication Software Development Kit (SDK) lets you build two-step verification directly into the sign-in or transaction processes of applications in your Azure AD tenant.</span></span>

<span data-ttu-id="ff971-105">Sada SDK služby Multi-Factor Authentication je k dispozici pro C#, Visual Basic (.NET), Java, Perl, PHP a Ruby.</span><span class="sxs-lookup"><span data-stu-id="ff971-105">The Multi-Factor Authentication SDK is available for C#, Visual Basic (.NET), Java, Perl, PHP, and Ruby.</span></span> <span data-ttu-id="ff971-106">Sada SDK poskytuje dynamické obálku kolem dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff971-106">The SDK provides a thin wrapper around two-step verification.</span></span> <span data-ttu-id="ff971-107">Obsahuje všechno, co potřebujete k zápisu kódu, včetně soubory komentáři zdrojového kódu, například soubory a podrobné souboru ReadMe.</span><span class="sxs-lookup"><span data-stu-id="ff971-107">It includes everything you need to write your code, including commented source code files, example files, and a detailed ReadMe file.</span></span> <span data-ttu-id="ff971-108">Každý SDK zahrnuje také certifikát a soukromý klíč pro šifrování transakce, které jsou jedinečné pro vaše zprostředkovatel vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff971-108">Each SDK also includes a certificate and private key for encrypting transactions that are unique to your Multi-Factor Authentication Provider.</span></span> <span data-ttu-id="ff971-109">Tak dlouho, dokud máte poskytovatele, si můžete stáhnout sadu SDK v jako v mnoha jazycích a formátů podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="ff971-109">As long as you have a provider, you can download the SDK in as many languages and formats as you need.</span></span>

<span data-ttu-id="ff971-110">Struktura rozhraní API v službu Multi-Factor Authentication SDK je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="ff971-110">The structure of the APIs in the Multi-Factor Authentication SDK is simple.</span></span> <span data-ttu-id="ff971-111">Ujistěte se, jedné funkce volání do rozhraní API s parametry Multi-Factor možnost (jako jsou ověřování režimu) a dat uživatele (například telefonní číslo pro volání nebo číslo PIN kódu k ověření).</span><span class="sxs-lookup"><span data-stu-id="ff971-111">Make a single function call to an API with the multi-factor option parameters (like verification mode) and user data (like the telephone number to call or the PIN number to validate).</span></span> <span data-ttu-id="ff971-112">Rozhraní API převede volání funkce do webových požadavků služby založené na cloudu Azure Multi-Factor Authentication Service.</span><span class="sxs-lookup"><span data-stu-id="ff971-112">The APIs translate the function call into web services requests to the cloud-based Azure Multi-Factor Authentication Service.</span></span> <span data-ttu-id="ff971-113">Všechna volání musí obsahovat odkaz na privátní certifikát, který je zahrnuta v každé sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="ff971-113">All calls must include a reference to the private certificate that is included in every SDK.</span></span>

<span data-ttu-id="ff971-114">Protože rozhraní API nemají přístup k uživatelům zaregistrovat ve službě Azure Active Directory, je třeba zadat informace o uživateli v souboru nebo databáze.</span><span class="sxs-lookup"><span data-stu-id="ff971-114">Because the APIs do not have access to users registered in Azure Active Directory, you must provide user information in a file or database.</span></span> <span data-ttu-id="ff971-115">Rozhraní API také neposkytují funkce správy zápisu nebo uživatele, proto musíte vytvořit tyto procesy do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff971-115">Also, the APIs do not provide enrollment or user management features, so you need to build these processes into your application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff971-116">Pokud si chcete stáhnout sadu SDK, je třeba vytvořit poskytovatele Azure Multi-Factor Auth i v případě, že máte licence Azure MFA, AAD Premium nebo EMS.</span><span class="sxs-lookup"><span data-stu-id="ff971-116">To download the SDK, you need to create an Azure Multi-Factor Auth Provider even if you have Azure MFA, AAD Premium, or EMS licenses.</span></span> <span data-ttu-id="ff971-117">Pokud pro tento účel vytvořit poskytovatele Azure Multi-Factor Auth a už máte licence, nezapomeňte si vytvořit zprostředkovatele s **za povoleného uživatele** modelu.</span><span class="sxs-lookup"><span data-stu-id="ff971-117">If you create an Azure Multi-Factor Auth Provider for this purpose and already have licenses, make sure to create the Provider with the **Per Enabled User** model.</span></span> <span data-ttu-id="ff971-118">Potom propojte poskytovatele s adresářem, ve kterém jsou uložené licence Azure MFA, Azure AD Premium nebo EMS.</span><span class="sxs-lookup"><span data-stu-id="ff971-118">Then, link the Provider to the directory that contains the Azure MFA, Azure AD Premium, or EMS licenses.</span></span> <span data-ttu-id="ff971-119">Tato konfigurace zajistí, že se pouze účtují Pokud máte více jedinečných uživatelů, kteří pomocí sady SDK než počet licencí, které vlastníte.</span><span class="sxs-lookup"><span data-stu-id="ff971-119">This configuration ensures that you are only billed if you have more unique users using the SDK than the number of licenses you own.</span></span>


## <a name="download-the-sdk"></a><span data-ttu-id="ff971-120">Stažení sady SDK</span><span class="sxs-lookup"><span data-stu-id="ff971-120">Download the SDK</span></span>
<span data-ttu-id="ff971-121">Stažení sady SDK Azure Multi-Factor vyžaduje [zprostředkovatel vícefaktorového ověřování Azure](multi-factor-authentication-get-started-auth-provider.md).</span><span class="sxs-lookup"><span data-stu-id="ff971-121">Downloading the Azure Multi-Factor SDK requires an [Azure Multi-Factor Auth Provider](multi-factor-authentication-get-started-auth-provider.md).</span></span>  <span data-ttu-id="ff971-122">To vyžaduje úplné předplatné, i když jsou ve vlastnictví licence Azure MFA, Azure AD Premium nebo Enterprise Mobility Suite.</span><span class="sxs-lookup"><span data-stu-id="ff971-122">This requires a full Azure subscription, even if Azure MFA, Azure AD Premium, or Enterprise Mobility Suite licenses are owned.</span></span>  <span data-ttu-id="ff971-123">Chcete-li stáhnout sadu SDK, přejděte na portálu pro správu Multi-Factor.</span><span class="sxs-lookup"><span data-stu-id="ff971-123">To download the SDK, navigate to the Multi-Factor Management Portal.</span></span> <span data-ttu-id="ff971-124">Můžete dostat na portál, pomocí správy poskytovatele služby Multi-Factor Auth přímo, nebo kliknutím **"Přejděte na portál"** odkaz na stránce nastavení služby MFA.</span><span class="sxs-lookup"><span data-stu-id="ff971-124">You can reach the portal either by managing the Multi-Factor Auth Provider directly, or by clicking the **"Go to the portal"** link on the MFA service settings page.</span></span>

### <a name="download-from-the-azure-classic-portal"></a><span data-ttu-id="ff971-125">Stáhnout z portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="ff971-125">Download from the Azure classic portal</span></span>
1. <span data-ttu-id="ff971-126">Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ff971-126">Sign in to the [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="ff971-127">Vlevo vyberte možnost **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ff971-127">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="ff971-128">Na stránce služby Active Directory, v nejvyšší vyberte **zprostředkovatelé vícefaktorového ověřování**</span><span class="sxs-lookup"><span data-stu-id="ff971-128">On the Active Directory page, at the top select **Multi-Factor Auth Providers**</span></span>
4. <span data-ttu-id="ff971-129">V dolní části vyberte **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="ff971-129">At the bottom select **Manage**.</span></span> <span data-ttu-id="ff971-130">Otevře se nová stránka.</span><span class="sxs-lookup"><span data-stu-id="ff971-130">A new page opens.</span></span>
5. <span data-ttu-id="ff971-131">Na levé straně, v dolní části, klikněte na tlačítko **SDK**.</span><span class="sxs-lookup"><span data-stu-id="ff971-131">On the left, at the bottom, click **SDK**.</span></span>
   <span data-ttu-id="ff971-132"><center>![Stahování](./media/multi-factor-authentication-sdk/download.png)</center></span><span class="sxs-lookup"><span data-stu-id="ff971-132"><center>![Download](./media/multi-factor-authentication-sdk/download.png)</center></span></span>
6. <span data-ttu-id="ff971-133">Vyberte jazyk a klikněte na jednu odkazů přidružené ke stažení.</span><span class="sxs-lookup"><span data-stu-id="ff971-133">Select the language you want and click one the associated download links.</span></span>
7. <span data-ttu-id="ff971-134">Uložte stažený soubor.</span><span class="sxs-lookup"><span data-stu-id="ff971-134">Save the download.</span></span>

### <a name="download-from-the-service-settings"></a><span data-ttu-id="ff971-135">Stáhnout z nastavení služby</span><span class="sxs-lookup"><span data-stu-id="ff971-135">Download from the service settings</span></span>
1. <span data-ttu-id="ff971-136">Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ff971-136">Sign in to the [Azure classic portal](https://manage.windowsazure.com) as an Administrator.</span></span>
2. <span data-ttu-id="ff971-137">Vlevo vyberte možnost **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ff971-137">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="ff971-138">Dvakrát klikněte na svoji instanci služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff971-138">Double-click your instance of Azure AD.</span></span>
4. <span data-ttu-id="ff971-139">Nahoře klikněte na **Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="ff971-139">At the top click **Configure**</span></span>
5. <span data-ttu-id="ff971-140">V části ověřování Multi-Factor authentication, vyberte **spravovat nastavení služby**
   ![stáhnout](./media/multi-factor-authentication-sdk/download2.png)</span><span class="sxs-lookup"><span data-stu-id="ff971-140">Under multi-factor authentication, select **Manage service settings**
![Download](./media/multi-factor-authentication-sdk/download2.png)</span></span>
6. <span data-ttu-id="ff971-141">Dole na stránce s nastavením služby klikněte na **Přejít na portál**.</span><span class="sxs-lookup"><span data-stu-id="ff971-141">On the services settings page, at the bottom of the screen click **Go to the portal**.</span></span> <span data-ttu-id="ff971-142">Otevře se nová stránka.</span><span class="sxs-lookup"><span data-stu-id="ff971-142">A new page opens.</span></span>
   <span data-ttu-id="ff971-143">![Stáhnout](./media/multi-factor-authentication-sdk/download3a.png)</span><span class="sxs-lookup"><span data-stu-id="ff971-143">![Download](./media/multi-factor-authentication-sdk/download3a.png)</span></span>
7. <span data-ttu-id="ff971-144">Na levé straně, v dolní části, klikněte na tlačítko **SDK**.</span><span class="sxs-lookup"><span data-stu-id="ff971-144">On the left, at the bottom, click **SDK**.</span></span>
8. <span data-ttu-id="ff971-145">Vyberte jazyk a klikněte na jednu odkazů přidružené ke stažení.</span><span class="sxs-lookup"><span data-stu-id="ff971-145">Select the language you want and click one the associated download links.</span></span>
9. <span data-ttu-id="ff971-146">Uložte stažený soubor.</span><span class="sxs-lookup"><span data-stu-id="ff971-146">Save the download.</span></span>

## <a name="whats-in-the-sdk"></a><span data-ttu-id="ff971-147">Co je v sadě SDK</span><span class="sxs-lookup"><span data-stu-id="ff971-147">What's in the SDK</span></span>
<span data-ttu-id="ff971-148">Sada SDK zahrnuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="ff971-148">The SDK includes the following items:</span></span>

* <span data-ttu-id="ff971-149">**SOUBOR README**.</span><span class="sxs-lookup"><span data-stu-id="ff971-149">**README**.</span></span> <span data-ttu-id="ff971-150">Vysvětluje, jak používat rozhraní API služby Multi-Factor Authentication v nové nebo existující aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ff971-150">Explains how to use the Multi-Factor Authentication APIs in a new or existing application.</span></span>
* <span data-ttu-id="ff971-151">**Zdrojové soubory** pro službu Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="ff971-151">**Source files** for Multi-Factor Authentication</span></span>
* <span data-ttu-id="ff971-152">**Klientský certifikát** používaný ke komunikaci se službou Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="ff971-152">**Client certificate** that you use to communicate with the Multi-Factor Authentication service</span></span>
* <span data-ttu-id="ff971-153">**Privátní klíč** certifikátu</span><span class="sxs-lookup"><span data-stu-id="ff971-153">**Private key** for the certificate</span></span>
* <span data-ttu-id="ff971-154">**Výsledky volání.**</span><span class="sxs-lookup"><span data-stu-id="ff971-154">**Call results.**</span></span> <span data-ttu-id="ff971-155">Seznam kódy výsledků volání.</span><span class="sxs-lookup"><span data-stu-id="ff971-155">A list of call result codes.</span></span> <span data-ttu-id="ff971-156">K otevření tohoto souboru, použijte aplikaci s formátování textu, například WordPad.</span><span class="sxs-lookup"><span data-stu-id="ff971-156">To open this file, use an application with text formatting, such as WordPad.</span></span> <span data-ttu-id="ff971-157">Kódy výsledků volání používejte pro testování a řešení potíží s implementací vícefaktorového ověřování ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ff971-157">Use the call result codes to test and troubleshoot the implementation of Multi-Factor Authentication in your application.</span></span> <span data-ttu-id="ff971-158">Nejsou ověřování stavové kódy.</span><span class="sxs-lookup"><span data-stu-id="ff971-158">They are not authentication status codes.</span></span>
* <span data-ttu-id="ff971-159">**Příklady.**</span><span class="sxs-lookup"><span data-stu-id="ff971-159">**Examples.**</span></span> <span data-ttu-id="ff971-160">Ukázkový kód pro základní pracovní implementaci služby Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="ff971-160">Sample code for a basic working implementation of Multi-Factor Authentication.</span></span>

> [!WARNING]
> <span data-ttu-id="ff971-161">Klientský certifikát je jedinečný privátní certifikát, který byl vygenerován speciálně pro vás.</span><span class="sxs-lookup"><span data-stu-id="ff971-161">The client certificate is a unique private certificate that was generated especially for you.</span></span> <span data-ttu-id="ff971-162">Sdílené složky nebo ztratit tento soubor.</span><span class="sxs-lookup"><span data-stu-id="ff971-162">Do not share or lose this file.</span></span> <span data-ttu-id="ff971-163">Je váš klíč k zajištění zabezpečení komunikace se službou Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="ff971-163">It’s your key to ensuring the security of your communications with the Multi-Factor Authentication service.</span></span>

## <a name="code-sample"></a><span data-ttu-id="ff971-164">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="ff971-164">Code sample</span></span>
<span data-ttu-id="ff971-165">Tento příklad ukazuje, jak přidat standardní režim hlasového hovoru ověření do vaší aplikace pomocí rozhraní API v sadě SDK Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="ff971-165">This code sample shows you how to use the APIs in the Azure Multi-Factor Authentication SDK to add standard mode voice call verification to your application.</span></span> <span data-ttu-id="ff971-166">Je standardní režim telefonního hovoru, který uživatel odpoví na stisknutím klávesy #.</span><span class="sxs-lookup"><span data-stu-id="ff971-166">Standard mode is a telephone call that the user responds to by pressing the # key.</span></span>

<span data-ttu-id="ff971-167">Tento příklad používá C# .NET 2.0 Multi-Factor Authentication SDK v základní aplikace ASP.NET pomocí jazyka C# logiku na straně serveru, ale proces je podobný v dalších jazycích.</span><span class="sxs-lookup"><span data-stu-id="ff971-167">This example uses the C# .NET 2.0 Multi-Factor Authentication SDK in a basic ASP.NET application with C# server-side logic, but the process is similar in other languages.</span></span> <span data-ttu-id="ff971-168">Vzhledem k tomu, že sada SDK obsahuje zdrojové soubory, není spustitelné soubory, lze vytvořit soubory a odkazujte na ně nebo je přímo do aplikace zahrnout.</span><span class="sxs-lookup"><span data-stu-id="ff971-168">Because the SDK includes source files, not executable files, you can build the files and reference them or include them directly in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="ff971-169">Při implementaci vícefaktorového ověřování, použijte další metody (telefonního hovoru nebo textové zprávy) jako sekundární nebo terciární ověření pro doplnění vaší primární metoda ověřování (uživatelské jméno a heslo).</span><span class="sxs-lookup"><span data-stu-id="ff971-169">When implementing Multi-Factor Authentication, use the additional methods (phone call or text message) as secondary or tertiary verification to supplement your primary authentication method (username and password).</span></span> <span data-ttu-id="ff971-170">Tyto metody se nedají jako primární metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff971-170">These methods are not designed as primary authentication methods.</span></span>

### <a name="code-sample-overview"></a><span data-ttu-id="ff971-171">Přehled ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="ff971-171">Code Sample Overview</span></span>
<span data-ttu-id="ff971-172">Tento ukázkový kód pro jednoduchou webovou aplikaci ukázku používá telefonního hovoru s klíče odpověď # ověřit uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff971-172">This sample code for a simple web demo application uses a telephone call with a # key response to verify the user's authentication.</span></span> <span data-ttu-id="ff971-173">Tento faktor telefonní hovor je ověřování službou Multi-Factor Authentication označuje jako standardní režim.</span><span class="sxs-lookup"><span data-stu-id="ff971-173">This telephone call factor is known in Multi-Factor Authentication as standard mode.</span></span>

<span data-ttu-id="ff971-174">Kód klienta neobsahuje žádné elementy specifické pro službu Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="ff971-174">The client-side code does not include any Multi-Factor Authentication-specific elements.</span></span> <span data-ttu-id="ff971-175">Protože dodatečných ověřovacích faktorů jsou nezávislé na primární ověřování, můžete je přidat beze změny existujícího rozhraní přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ff971-175">Because the additional authentication factors are independent of the primary authentication, you can add them without changing the existing sign-on interface.</span></span> <span data-ttu-id="ff971-176">Rozhraní API v sadě SDK Multi-Factor umožňují přizpůsobit činnost koncového uživatele, ale možná nebudete muset nic nezmění vůbec.</span><span class="sxs-lookup"><span data-stu-id="ff971-176">The APIs in the Multi-Factor SDK let you customize the user experience, but you might not need to change anything at all.</span></span>

<span data-ttu-id="ff971-177">Serverový kód přidá standardní režim ověřování v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="ff971-177">The server-side code adds standard-mode authentication in Step 2.</span></span> <span data-ttu-id="ff971-178">Vytvoří objekt PfAuthParams s parametry, které jsou požadovány pro standardní režim ověřování: uživatelské jméno, telefonní číslo a režim a cestu k certifikátu klienta (CertFilePath), který je požadován při každém volání.</span><span class="sxs-lookup"><span data-stu-id="ff971-178">It creates a PfAuthParams object with the parameters that are required for standard-mode verification: username, telephone number, and mode, and the path to the client certificate (CertFilePath), which is required in each call.</span></span> <span data-ttu-id="ff971-179">Ukázka všech parametrů v PfAuthParams naleznete v souboru příklad v sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="ff971-179">For a demonstration of all parameters in PfAuthParams, see the Example file in the SDK.</span></span>

<span data-ttu-id="ff971-180">V dalším kroku kód předá objekt PfAuthParams pf_authenticate() funkce.</span><span class="sxs-lookup"><span data-stu-id="ff971-180">Next, the code passes the PfAuthParams object to the pf_authenticate() function.</span></span> <span data-ttu-id="ff971-181">Návratová hodnota označuje úspěch nebo selhání ověřování.</span><span class="sxs-lookup"><span data-stu-id="ff971-181">The return value indicates the success or failure of the authentication.</span></span> <span data-ttu-id="ff971-182">Parametry, callStatus a ID chyby, obsahují další volání výsledek informace.</span><span class="sxs-lookup"><span data-stu-id="ff971-182">The out parameters, callStatus, and errorID, contain additional call result information.</span></span> <span data-ttu-id="ff971-183">Kódy výsledků volání jsou popsané v souboru výsledků volání v sadě SDK.</span><span class="sxs-lookup"><span data-stu-id="ff971-183">The call result codes are documented in the call results file in the SDK.</span></span>

<span data-ttu-id="ff971-184">Tato minimální implementace může být napsán v pár řádků.</span><span class="sxs-lookup"><span data-stu-id="ff971-184">This minimal implementation can be written in a few lines.</span></span> <span data-ttu-id="ff971-185">V produkčním kódu, bude však zahrnovat sofistikovanější zpracování chyb, kód další databáze a vylepšené uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff971-185">However, in production code, you would include more sophisticated error handling, additional database code, and an enhanced user experience.</span></span>

### <a name="web-client-code"></a><span data-ttu-id="ff971-186">Kódu klienta webové</span><span class="sxs-lookup"><span data-stu-id="ff971-186">Web Client Code</span></span>
<span data-ttu-id="ff971-187">Zde je kódu klienta webové stránky ukázku.</span><span class="sxs-lookup"><span data-stu-id="ff971-187">The following is web client code for a demo page.</span></span>

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


### <a name="server-side-code"></a><span data-ttu-id="ff971-188">Kód na straně serveru</span><span class="sxs-lookup"><span data-stu-id="ff971-188">Server-Side Code</span></span>
<span data-ttu-id="ff971-189">V následujícím kódu na straně serveru Multi-Factor Authentication nakonfigurován a spusťte v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="ff971-189">In the following server-side code, Multi-Factor Authentication is configured and run in Step 2.</span></span> <span data-ttu-id="ff971-190">Standardní režim (MODE_STANDARD) je telefonní hovor, na kterou uživatel odpovídá stisknutím klávesy #.</span><span class="sxs-lookup"><span data-stu-id="ff971-190">Standard mode (MODE_STANDARD) is a telephone call to which the user responds by pressing the # key.</span></span>

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
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
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
