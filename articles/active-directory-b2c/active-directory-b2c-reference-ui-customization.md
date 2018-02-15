---
title: "Přizpůsobení uživatelského rozhraní (UI) – Azure AD B2C | Microsoft Docs"
description: "Téma o funkce přizpůsobení uživatelského rozhraní (UI) v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 99f5a391-5328-471d-a15c-a2fafafe233d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: be3fe7199308606aaab002290319df9c82149433
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a><span data-ttu-id="4af25-103">Azure Active Directory B2C: Přizpůsobení uživatelského rozhraní (UI) Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="4af25-103">Azure Active Directory B2C: Customize the Azure AD B2C user interface (UI)</span></span>

<span data-ttu-id="4af25-104">Činnost koncového uživatele je prvořadá v zákazníků, kterým čelí aplikace.</span><span class="sxs-lookup"><span data-stu-id="4af25-104">User experience is paramount in a customer facing application.</span></span>  <span data-ttu-id="4af25-105">Růst zákazníkovi základní tím, že vytvoří koncových uživatelů s vzhledu a chování vaší značkou.</span><span class="sxs-lookup"><span data-stu-id="4af25-105">Grow your customer base by crafting user experiences with the look and feel of your brand.</span></span> <span data-ttu-id="4af25-106">Azure Active Directory B2C (Azure AD B2C) umožňuje přizpůsobit profil registrace, přihlášení, úpravy a resetování hesla stránky s ovládacím prvkem dokonalou pixelů.</span><span class="sxs-lookup"><span data-stu-id="4af25-106">Azure Active Directory B2C (Azure AD B2C) lets you customize sign-up, sign-in, profile editing, and password reset pages with pixel-perfect control.</span></span>

> [!NOTE]
> <span data-ttu-id="4af25-107">Funkce přizpůsobení uživatelského rozhraní stránky popsané v tomto článku se nevztahuje pouze zásad přihlašování, jeho doprovodné stránku pro reset hesla a ověření e-mailů.</span><span class="sxs-lookup"><span data-stu-id="4af25-107">The page UI customization feature described in this article does not apply to the sign-in only policy, its accompanying password reset page, and verification emails.</span></span>  <span data-ttu-id="4af25-108">Tyto funkce používají [firemního brandingu funkce](../active-directory/customize-branding.md) místo.</span><span class="sxs-lookup"><span data-stu-id="4af25-108">These features use the [company branding feature](../active-directory/customize-branding.md) instead.</span></span>
>
> <span data-ttu-id="4af25-109">Podobně pokud uživatel intiates zásadu upravit profil *před* přihlášení, uživatel bude přesměrován na stránku, kterou lze přizpůsobit pomocí [firemního brandingu funkce](../active-directory/customize-branding.md).</span><span class="sxs-lookup"><span data-stu-id="4af25-109">Similarly, if a user intiates an edit profile policy *before* signing in, the user will be redirected to a page that can be customized using the [company branding feature](../active-directory/customize-branding.md).</span></span>

<span data-ttu-id="4af25-110">Tento článek obsahuje následující témata:</span><span class="sxs-lookup"><span data-stu-id="4af25-110">This article covers the following topics:</span></span>

* <span data-ttu-id="4af25-111">Funkce pro přizpůsobení uživatelského rozhraní stránky.</span><span class="sxs-lookup"><span data-stu-id="4af25-111">The page UI customization feature.</span></span>
* <span data-ttu-id="4af25-112">Nástroj pro nahrávání obsahu HTML do úložiště objektů Blob Azure pro použití s přizpůsobení funkcí uživatelského rozhraní stránky.</span><span class="sxs-lookup"><span data-stu-id="4af25-112">A tool for uploading HTML content to Azure Blob Storage for use with the page UI customization feature.</span></span>
* <span data-ttu-id="4af25-113">Prvky uživatelského rozhraní používá Azure AD B2C, kterou si můžete přizpůsobit pomocí stylů CSS (Cascading Style).</span><span class="sxs-lookup"><span data-stu-id="4af25-113">The UI elements used by Azure AD B2C that you can customize using Cascading Style Sheets (CSS).</span></span>
* <span data-ttu-id="4af25-114">Osvědčené postupy při výkonu této funkce.</span><span class="sxs-lookup"><span data-stu-id="4af25-114">Best practices when exercising this feature.</span></span>

## <a name="the-page-ui-customization-feature"></a><span data-ttu-id="4af25-115">Přizpůsobení funkce uživatelského rozhraní stránky</span><span class="sxs-lookup"><span data-stu-id="4af25-115">The page UI customization feature</span></span>

<span data-ttu-id="4af25-116">Můžete přizpůsobit vzhled a chování registrace a přihlášení se heslo zákazníka resetování a úpravy profilu stránky (nakonfigurováním [zásady](active-directory-b2c-reference-policies.md)).</span><span class="sxs-lookup"><span data-stu-id="4af25-116">You can customize the look and feel of customer sign-up, sign-in, password reset, and profile-editing pages (by configuring [policies](active-directory-b2c-reference-policies.md)).</span></span> <span data-ttu-id="4af25-117">Vaši zákazníci získat integrované prostředí při přechodu mezi vaší aplikace a stránky obsluhuje Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="4af25-117">Your customers get a seamless experience when navigating between your application and pages served by Azure AD B2C.</span></span>

<span data-ttu-id="4af25-118">Na rozdíl od jiných služeb, kde možnosti uživatelského rozhraní, Azure AD B2C používá jednoduché a moderní přístupu k přizpůsobení uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4af25-118">Unlike other services where UI options, Azure AD B2C uses a simple and modern approach to UI customization.</span></span>

<span data-ttu-id="4af25-119">Zde je, jak to funguje: Azure AD B2C spuštěním kódu v prohlížeči vašeho zákazníka a používá moderní přístup názvem [sdílení prostředků různých původů (CORS)](http://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="4af25-119">Here's how it works: Azure AD B2C runs code in your customer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span></span>  <span data-ttu-id="4af25-120">Při spuštění je obsah načten z adresy URL, který určíte v zásadách.</span><span class="sxs-lookup"><span data-stu-id="4af25-120">At run-time, content is loaded from a URL that you specify in a policy.</span></span> <span data-ttu-id="4af25-121">Můžete zadat různé adresy URL pro různé stránky.</span><span class="sxs-lookup"><span data-stu-id="4af25-121">You can specify different URLs for different pages.</span></span> <span data-ttu-id="4af25-122">Po obsah načíst z vaše adresa URL je sloučen s fragment HTML, který je vložen z Azure AD B2C, stránka se zobrazí zákazníkovi.</span><span class="sxs-lookup"><span data-stu-id="4af25-122">After content loaded from your URL is merged with an HTML fragment inserted from Azure AD B2C, the page is displayed to your customer.</span></span> <span data-ttu-id="4af25-123">Je potřeba udělat je:</span><span class="sxs-lookup"><span data-stu-id="4af25-123">All you need to do is:</span></span>

1. <span data-ttu-id="4af25-124">Vytváření obsahu s prázdnou ve správném formátu HTML5 `<div id="api"></div>` element nachází někde v `<body>`.</span><span class="sxs-lookup"><span data-stu-id="4af25-124">Create well-formed HTML5 content with an empty `<div id="api"></div>` element located somewhere in the `<body>`.</span></span> <span data-ttu-id="4af25-125">Tento element značky, které je vložen obsahu Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="4af25-125">This element marks where the Azure AD B2C content is inserted.</span></span>
1. <span data-ttu-id="4af25-126">Hostovat obsah na koncový bod HTTPS (s CORS povoleny).</span><span class="sxs-lookup"><span data-stu-id="4af25-126">Host your content on an HTTPS endpoint (with CORS allowed).</span></span> <span data-ttu-id="4af25-127">Všimněte si, jak získat a možnosti požadavek metody musí být povolena při konfiguraci CORS.</span><span class="sxs-lookup"><span data-stu-id="4af25-127">Note both GET and OPTIONS request methods must be enabled when configuring CORS.</span></span>
1. <span data-ttu-id="4af25-128">K úpravě stylu prvky uživatelského rozhraní, které Azure AD B2C vloží pomocí šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="4af25-128">Use CSS to style the UI elements that Azure AD B2C inserts.</span></span>

### <a name="a-basic-example-of-customized-html"></a><span data-ttu-id="4af25-129">Základní příklad přizpůsobené HTML</span><span class="sxs-lookup"><span data-stu-id="4af25-129">A basic example of customized HTML</span></span>

<span data-ttu-id="4af25-130">V následujícím příkladu je nejzákladnější obsah HTML, který můžete použít k testování této funkci.</span><span class="sxs-lookup"><span data-stu-id="4af25-130">The following example is the most basic HTML content that you can use to test this capability.</span></span> <span data-ttu-id="4af25-131">Použití [pomocným nástrojem pro](active-directory-b2c-reference-ui-customization-helper-tool.md) nahrání a konfigurace tohoto obsahu ve službě Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="4af25-131">Use the [helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) to upload and configure this content on your Azure Blob storage.</span></span> <span data-ttu-id="4af25-132">Následně můžete ověřit, že základní, stylizované tlačítka & polí formuláře na každé stránce jsou zobrazené a funkční.</span><span class="sxs-lookup"><span data-stu-id="4af25-132">You can then verify that the basic, non-stylized buttons & form fields on each page are displayed and functional.</span></span>

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
    </body>
</html>
```

## <a name="test-out-the-ui-customization-feature"></a><span data-ttu-id="4af25-133">Otestování funkce přizpůsobení uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="4af25-133">Test out the UI customization feature</span></span>

<span data-ttu-id="4af25-134">Chcete vyzkoušet funkce přizpůsobení uživatelského rozhraní pomocí naše ukázka HTML a CSS obsahu?</span><span class="sxs-lookup"><span data-stu-id="4af25-134">Want to try out the UI customization feature by using our sample HTML and CSS content?</span></span>  <span data-ttu-id="4af25-135">Nabízíme vám [pomocný nástroj](active-directory-b2c-reference-ui-customization-helper-tool.md) , odešle a nakonfiguruje ukázkový obsah v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="4af25-135">We've provided you [a helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) that uploads and configures sample content on your Azure Blob storage.</span></span>

> [!NOTE]
> <span data-ttu-id="4af25-136">Je možné hostovat kdekoli obsah uživatelského rozhraní: na webových serverech, sítím CDN, AWS S3, sdílení systémy souborů atd. Tak dlouho, dokud je obsah uložený na veřejně dostupný koncový bod HTTPS s povolením CORS, můžete se pustit do práce.</span><span class="sxs-lookup"><span data-stu-id="4af25-136">You can host your UI content anywhere: on web servers, CDNs, AWS S3, file sharing systems, etc. As long as the content is hosted on a publicly available HTTPS endpoint with CORS enabled, you are good to go.</span></span> <span data-ttu-id="4af25-137">Úložiště objektů Blob v Azure se používá pouze pro ilustraci.</span><span class="sxs-lookup"><span data-stu-id="4af25-137">We are using Azure Blob storage for illustrative purposes only.</span></span>
>

## <a name="the-ui-fragments-embedded-by-azure-ad-b2c"></a><span data-ttu-id="4af25-138">Fragmenty uživatelského rozhraní vložených pomocí Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="4af25-138">The UI fragments embedded by Azure AD B2C</span></span>

<span data-ttu-id="4af25-139">Následující části uvádějí fragmentů jazyka HTML5, které Azure AD B2C sloučí `<div id="api"></div>` element umístěné v obsahu.</span><span class="sxs-lookup"><span data-stu-id="4af25-139">The following sections list the HTML5 fragments that Azure AD B2C merges into the `<div id="api"></div>` element located in your content.</span></span> <span data-ttu-id="4af25-140">**Nevkládejte tyto Fragmenty HTML 5 obsah.**</span><span class="sxs-lookup"><span data-stu-id="4af25-140">**Do not insert these fragments in your HTML 5 content.**</span></span> <span data-ttu-id="4af25-141">Služba Azure AD B2C vloží je do za běhu.</span><span class="sxs-lookup"><span data-stu-id="4af25-141">The Azure AD B2C service inserts them in at run-time.</span></span> <span data-ttu-id="4af25-142">Použijte tyto fragmenty jako pomůcku při navrhování vlastních stylů CSS (Cascading Style).</span><span class="sxs-lookup"><span data-stu-id="4af25-142">Use these fragments as a reference when designing your own Cascading Style Sheets (CSS).</span></span>

### <a name="fragment-inserted-into-the-identity-provider-selection-page"></a><span data-ttu-id="4af25-143">Fragment vloženy do "zprostředkovatele výběr stránky identit"</span><span class="sxs-lookup"><span data-stu-id="4af25-143">Fragment inserted into the "Identity provider selection page"</span></span>

<span data-ttu-id="4af25-144">Tato stránka obsahuje seznam poskytovatelů identity, které může uživatel vybírat během registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4af25-144">This page contains a list of identity providers that the user can choose from during sign-up or sign-in.</span></span> <span data-ttu-id="4af25-145">Tato tlačítka zahrnují zprostředkovatelů identity sociálních třeba Facebook a Google + nebo místní účty (podle e-mailové adresy nebo uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="4af25-145">These buttons include social identity providers such as Facebook and Google+, or local accounts (based on email address or user name).</span></span>

```HTML
<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-local-account-sign-up-page"></a><span data-ttu-id="4af25-146">Fragment vloženy do "místní účet stránku pro přihlášení"</span><span class="sxs-lookup"><span data-stu-id="4af25-146">Fragment inserted into the "Local account sign-up page"</span></span>

<span data-ttu-id="4af25-147">Tato stránka obsahuje formulář založený na e-mailovou adresu nebo uživatelské jméno pro místní účet pro zápis.</span><span class="sxs-lookup"><span data-stu-id="4af25-147">This page contains a form for local account sign-up based on an email address or a user name.</span></span> <span data-ttu-id="4af25-148">Formulář může obsahovat různé vstupní ovládací prvky jako vstupní textové pole, pole pro zadání hesla, přepínač, polí rozevíracího seznamu vyberte jeden a více vyberte zaškrtávací políčka.</span><span class="sxs-lookup"><span data-stu-id="4af25-148">The form can contain different input controls such as text input box, password entry box, radio button, single-select drop-down boxes, and multi-select check boxes.</span></span>

```HTML
<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-social-account-sign-up-page"></a><span data-ttu-id="4af25-149">Fragment vloženy do "" sociálních účet stránku pro přihlášení"</span><span class="sxs-lookup"><span data-stu-id="4af25-149">Fragment inserted into the ""Social account sign-up page"</span></span>

<span data-ttu-id="4af25-150">Tato stránka se mohou objevit při registraci pomocí existujícího účtu od poskytovatele identity sociálních třeba Facebook nebo Google +.</span><span class="sxs-lookup"><span data-stu-id="4af25-150">This page may appear when signing up using an existing account from a social identity provider such as Facebook or Google+.</span></span>  <span data-ttu-id="4af25-151">Používá se při dalších informace musí být shromažďovány z koncového uživatele pomocí registračního formuláře.</span><span class="sxs-lookup"><span data-stu-id="4af25-151">It is used when additional information must be collected from the end user using a sign-up form.</span></span> <span data-ttu-id="4af25-152">Tato stránka je podobný na místní účet stránku pro přihlášení (zobrazené v předchozí části) s výjimkou pole pro zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="4af25-152">This page is similar to the local account sign-up page (shown in the previous section) with the exception of the password entry fields.</span></span>

### <a name="fragment-inserted-into-the-unified-sign-up-or-sign-in-page"></a><span data-ttu-id="4af25-153">Fragment vloženy do "Unified registrace nebo přihlášení stránku"</span><span class="sxs-lookup"><span data-stu-id="4af25-153">Fragment inserted into the "Unified sign-up or sign-in page"</span></span>

<span data-ttu-id="4af25-154">Tato stránka zpracovává jak registrace a přihlášení zákazníků, kteří můžou využívat zprostředkovatelů identity sociálních třeba Facebook nebo Google + nebo místní účty.</span><span class="sxs-lookup"><span data-stu-id="4af25-154">This page handles both sign-up & sign-in of customers, who can use social identity providers such as Facebook or Google+, or local accounts.</span></span>

```HTML
<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>
```

### <a name="fragment-inserted-into-the-multi-factor-authentication-page"></a><span data-ttu-id="4af25-155">Fragment vloženy do "stránka vícefaktorového ověřování"</span><span class="sxs-lookup"><span data-stu-id="4af25-155">Fragment inserted into the "Multi-factor authentication page"</span></span>

<span data-ttu-id="4af25-156">Na této stránce si uživatelé mohli ověřit jejich telefonních čísel (pomocí textové nebo hlasové) během registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4af25-156">On this page, users can verify their phone numbers (using text or voice) during sign-up or sign-in.</span></span>

```HTML
<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-error-page"></a><span data-ttu-id="4af25-157">Fragment vloženy do "" chybové stránky"</span><span class="sxs-lookup"><span data-stu-id="4af25-157">Fragment inserted into the ""Error page"</span></span>

```HTML
<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>
```

## <a name="localizing-your-html-content"></a><span data-ttu-id="4af25-158">Lokalizace obsah HTML</span><span class="sxs-lookup"><span data-stu-id="4af25-158">Localizing your HTML content</span></span>

<span data-ttu-id="4af25-159">Je možné lokalizovat obsah HTML zapnutím ['Jazyk přizpůsobení'](active-directory-b2c-reference-language-customization.md).</span><span class="sxs-lookup"><span data-stu-id="4af25-159">You can localize your HTML content by turning on ['Language customization'](active-directory-b2c-reference-language-customization.md).</span></span>  <span data-ttu-id="4af25-160">Povolení této funkce umožňuje Azure AD B2C předávat parametr Open ID Connect `ui-locales`, na váš koncový bod.</span><span class="sxs-lookup"><span data-stu-id="4af25-160">Enabling this feature allows Azure AD B2C to forward the Open ID Connect parameter, `ui-locales`, to your endpoint.</span></span>  <span data-ttu-id="4af25-161">Vaše servery obsahu můžete zajistit vlastní stránky HTML, které jsou specifické pro jazyk použít tento parametr.</span><span class="sxs-lookup"><span data-stu-id="4af25-161">Your content server can use this parameter to provide customized HTML pages that are language-specific.</span></span>

## <a name="things-to-remember-when-building-your-own-content"></a><span data-ttu-id="4af25-162">Pamatujte při vytváření vlastní obsah</span><span class="sxs-lookup"><span data-stu-id="4af25-162">Things to remember when building your own content</span></span>

<span data-ttu-id="4af25-163">Pokud máte v úmyslu používat funkci přizpůsobení uživatelského rozhraní stránky, projděte si následující osvědčené postupy:</span><span class="sxs-lookup"><span data-stu-id="4af25-163">If you are planning to use the page UI customization feature, review the following best practices:</span></span>

* <span data-ttu-id="4af25-164">Nemáte kopírovat obsah výchozí Azure AD B2C a pokuste se upravit.</span><span class="sxs-lookup"><span data-stu-id="4af25-164">Don't copy the Azure AD B2C's default content and attempt to modify it.</span></span> <span data-ttu-id="4af25-165">Je nejvhodnější k sestavení obsahu HTML5 od začátku a používat výchozí obsah jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="4af25-165">It is best to build your HTML5 content from scratch and to use default content as reference.</span></span>
* <span data-ttu-id="4af25-166">Z bezpečnostních důvodů jsme nemáte umožňují zahrnout všechny JavaScript do vašeho obsahu.</span><span class="sxs-lookup"><span data-stu-id="4af25-166">For security reasons, we don't allow you to include any JavaScript in your content.</span></span> <span data-ttu-id="4af25-167">Většina co potřebujete, by měly být dostupné z pole.</span><span class="sxs-lookup"><span data-stu-id="4af25-167">Most of what you need should be available out of the box.</span></span> <span data-ttu-id="4af25-168">Pokud ne, použít [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) požádat o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="4af25-168">If not, use [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) to request new functionality.</span></span>
* <span data-ttu-id="4af25-169">Podporované verze prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="4af25-169">Supported browser versions:</span></span>
  * <span data-ttu-id="4af25-170">Internet Explorer 11, 10, Edge</span><span class="sxs-lookup"><span data-stu-id="4af25-170">Internet Explorer 11, 10, Edge</span></span>
  * <span data-ttu-id="4af25-171">Omezená podpora pro Internet Explorer 9, 8</span><span class="sxs-lookup"><span data-stu-id="4af25-171">Limited support for Internet Explorer 9, 8</span></span>
  * <span data-ttu-id="4af25-172">Google Chrome 42.0 a vyšší</span><span class="sxs-lookup"><span data-stu-id="4af25-172">Google Chrome 42.0 and above</span></span>
  * <span data-ttu-id="4af25-173">Mozilla Firefox 38.0 a vyšší</span><span class="sxs-lookup"><span data-stu-id="4af25-173">Mozilla Firefox 38.0 and above</span></span>
