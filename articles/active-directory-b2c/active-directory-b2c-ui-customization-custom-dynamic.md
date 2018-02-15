---
title: "Azure Active Directory B2C: Přizpůsobení uživatelského rozhraní (UI) Azure AD B2C dynamicky pomocí vlastních zásad"
description: "Podpora více značky prostředí s obsahem HTML5 nebo šablon stylů CSS, který změní dynamicky za běhu."
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 09/20/2017
ms.author: yoelh
ms.openlocfilehash: 3a2310ae6266709df6677c55f11b15239c0425a2
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-configure-the-ui-with-dynamic-content-by-using-custom-policies"></a><span data-ttu-id="44035-103">Azure Active Directory B2C: Konfigurace uživatelského rozhraní s dynamickým obsahem pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="44035-103">Azure Active Directory B2C: Configure the UI with dynamic content by using custom policies</span></span>
<span data-ttu-id="44035-104">Pomocí Azure Active Directory B2C (Azure AD B2C) vlastní zásady, můžete odeslat parametr v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="44035-104">By using Azure Active Directory B2C (Azure AD B2C) custom policies, you can send a parameter in a query string.</span></span> <span data-ttu-id="44035-105">Pomocí předání parametru váš koncový bod HTML, můžete dynamicky měnit obsah stránky.</span><span class="sxs-lookup"><span data-stu-id="44035-105">By passing the parameter to your HTML endpoint, you can dynamically change the page content.</span></span> <span data-ttu-id="44035-106">Například můžete změnit obrázek pozadí na Azure AD B2C registrace nebo přihlášení stránky, na základě parametr, který můžete předat z webu nebo mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="44035-106">For example, you can change the background image on the Azure AD B2C sign-up or sign-in page, based on a parameter that you pass from your web or mobile application.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="44035-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="44035-107">Prerequisites</span></span>
<span data-ttu-id="44035-108">Tento článek zaměřuje na tom, jak přizpůsobit uživatelské rozhraní Azure AD B2C s *dynamický obsah* pomocí vlastních zásad.</span><span class="sxs-lookup"><span data-stu-id="44035-108">This article focuses on how to customize the Azure AD B2C user interface with *dynamic content* by using custom policies.</span></span> <span data-ttu-id="44035-109">Abyste mohli začít, najdete v části [přizpůsobení uživatelského rozhraní ve vlastních zásadách pro](active-directory-b2c-ui-customization-custom.md).</span><span class="sxs-lookup"><span data-stu-id="44035-109">To get started, see [UI customization in a custom policy](active-directory-b2c-ui-customization-custom.md).</span></span> 

>[!NOTE]
><span data-ttu-id="44035-110">V článku Azure AD B2C [přizpůsobení konfigurace uživatelského rozhraní ve vlastních zásadách pro](active-directory-b2c-ui-customization-custom.md), popisuje základní následující informace:</span><span class="sxs-lookup"><span data-stu-id="44035-110">The Azure AD B2C article, [Configure UI customization in a custom policy](active-directory-b2c-ui-customization-custom.md), discusses the following fundamentals:</span></span>
> * <span data-ttu-id="44035-111">Funkce přizpůsobení stránce uživatelské rozhraní (UI).</span><span class="sxs-lookup"><span data-stu-id="44035-111">The page user interface (UI) customization feature.</span></span>
> * <span data-ttu-id="44035-112">Základní nástroje pro testování funkce přizpůsobení uživatelského rozhraní stránky pomocí našich ukázkový obsah.</span><span class="sxs-lookup"><span data-stu-id="44035-112">Essential tools for testing the page UI customization feature by using our sample content.</span></span>
> * <span data-ttu-id="44035-113">Zadejte základní prvky uživatelského rozhraní každé stránky.</span><span class="sxs-lookup"><span data-stu-id="44035-113">The core UI elements of each page type.</span></span>
> * <span data-ttu-id="44035-114">Osvědčené postupy pro použití této funkce.</span><span class="sxs-lookup"><span data-stu-id="44035-114">Best practices for applying this feature.</span></span>

## <a name="add-a-link-to-html5css-templates-to-your-user-journey"></a><span data-ttu-id="44035-115">Přidat odkaz na HTML5 nebo šablon stylů CSS šablony vám dobře slouží uživatele</span><span class="sxs-lookup"><span data-stu-id="44035-115">Add a link to HTML5/CSS templates to your user journey</span></span>

<span data-ttu-id="44035-116">Ve vlastních zásadách definuje definici obsahu stránce HTML5 identifikátor URI, který slouží ke konkrétnímu kroku uživatelského rozhraní (například stránky přihlášení nebo registraci).</span><span class="sxs-lookup"><span data-stu-id="44035-116">In a custom policy, a content definition defines the HTML5 page URI that is used for a specified UI step (for example, the sign-in or sign-up pages).</span></span> <span data-ttu-id="44035-117">Základní zásada definuje výchozí vzhledu a chování tak, že odkazuje na URI HTML5 souborů (v CSS).</span><span class="sxs-lookup"><span data-stu-id="44035-117">The base policy defines the default look and feel by pointing to a URI of HTML5 files (in the CSS).</span></span> <span data-ttu-id="44035-118">V případě rozšíření zásad můžete upravit vzhled a chování přepsáním LoadUri souboru HTML5.</span><span class="sxs-lookup"><span data-stu-id="44035-118">In the extension policy, you can modify the look and feel by overriding the LoadUri for the HTML5 file.</span></span> <span data-ttu-id="44035-119">Obsahu definice obsahovat adresy URL na externí obsah, který je definován tím, že vytvoří soubory HTML5 nebo šablon stylů CSS, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="44035-119">Content definitions contain URLs to external content that is defined by crafting HTML5/CSS files, as appropriate.</span></span> 

<span data-ttu-id="44035-120">`ContentDefinitions` Část obsahuje řadu `ContentDefinition` elementů XML.</span><span class="sxs-lookup"><span data-stu-id="44035-120">The `ContentDefinitions` section contains a series of `ContentDefinition` XML elements.</span></span> <span data-ttu-id="44035-121">Atribut ID `ContentDefinition` element určuje typ stránky, která má vztah k definici obsahu.</span><span class="sxs-lookup"><span data-stu-id="44035-121">The ID attribute of the `ContentDefinition` element specifies the type of page that relates to the content definition.</span></span> <span data-ttu-id="44035-122">To znamená že element definují kontext, který se bude použít vlastní šablonu HTML5 nebo šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="44035-122">That is, the element defines the context that a custom HTML5/CSS template is going to apply.</span></span> <span data-ttu-id="44035-123">Následující tabulka popisuje sadu obsahu definice ID, které jsou rozpoznány modul IEF a typy stránek, které se vztahují k nim.</span><span class="sxs-lookup"><span data-stu-id="44035-123">The following table describes the set of content definition IDs that are recognized by the IEF engine, and the page types that relate to them.</span></span>

| <span data-ttu-id="44035-124">ID obsahu definice</span><span class="sxs-lookup"><span data-stu-id="44035-124">Content definition ID</span></span> | <span data-ttu-id="44035-125">Výchozí HTML5 šablony</span><span class="sxs-lookup"><span data-stu-id="44035-125">Default HTML5 template</span></span>| <span data-ttu-id="44035-126">Popis</span><span class="sxs-lookup"><span data-stu-id="44035-126">Description</span></span> | 
|-----------------------|--------|-------------|
| <span data-ttu-id="44035-127">*api.error*</span><span class="sxs-lookup"><span data-stu-id="44035-127">*api.error*</span></span> | [<span data-ttu-id="44035-128">exception.cshtml</span><span class="sxs-lookup"><span data-stu-id="44035-128">exception.cshtml</span></span>](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | <span data-ttu-id="44035-129">**Chybové stránky**.</span><span class="sxs-lookup"><span data-stu-id="44035-129">**Error page**.</span></span> <span data-ttu-id="44035-130">Tato stránka se zobrazí, když je došlo k výjimce nebo došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="44035-130">This page is displayed when an exception or an error is encountered.</span></span> |
| <span data-ttu-id="44035-131">*api.idpselections*</span><span class="sxs-lookup"><span data-stu-id="44035-131">*api.idpselections*</span></span> | [<span data-ttu-id="44035-132">idpSelector.cshtml</span><span class="sxs-lookup"><span data-stu-id="44035-132">idpSelector.cshtml</span></span>](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | <span data-ttu-id="44035-133">**Stránka Výběr zprostředkovatele identity**.</span><span class="sxs-lookup"><span data-stu-id="44035-133">**Identity provider selection page**.</span></span> <span data-ttu-id="44035-134">Tato stránka obsahuje seznam zprostředkovatelů identity, které uživatelé mohou vybírat z během přihlášení.</span><span class="sxs-lookup"><span data-stu-id="44035-134">This page lists identity providers that users can choose from during sign-in.</span></span> <span data-ttu-id="44035-135">Možnosti jsou obvykle poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google + nebo místní účty.</span><span class="sxs-lookup"><span data-stu-id="44035-135">The options are usually enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="44035-136">*api.idpselections.signup*</span><span class="sxs-lookup"><span data-stu-id="44035-136">*api.idpselections.signup*</span></span> | [<span data-ttu-id="44035-137">idpSelector.cshtml</span><span class="sxs-lookup"><span data-stu-id="44035-137">idpSelector.cshtml</span></span>](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | <span data-ttu-id="44035-138">**Výběr zprostředkovatele identity pro registraci**.</span><span class="sxs-lookup"><span data-stu-id="44035-138">**Identity provider selection for sign-up**.</span></span> <span data-ttu-id="44035-139">Tato stránka obsahuje seznam zprostředkovatelů identity, které uživatelé mohou vybírat z během registrace.</span><span class="sxs-lookup"><span data-stu-id="44035-139">This page lists identity providers that users can choose from during sign-up.</span></span> <span data-ttu-id="44035-140">Možnosti jsou poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook a Google + nebo místním účtům.</span><span class="sxs-lookup"><span data-stu-id="44035-140">The options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="44035-141">*api.localaccountpasswordreset*</span><span class="sxs-lookup"><span data-stu-id="44035-141">*api.localaccountpasswordreset*</span></span> | [<span data-ttu-id="44035-142">selfasserted.html</span><span class="sxs-lookup"><span data-stu-id="44035-142">selfasserted.html</span></span>](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | <span data-ttu-id="44035-143">**Zapomněli jste heslo**.</span><span class="sxs-lookup"><span data-stu-id="44035-143">**Forgot password page**.</span></span> <span data-ttu-id="44035-144">Tato stránka obsahuje formulář, který uživatelé musí dokončit zahájíte resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="44035-144">This page contains a form that users must complete to initiate a password reset.</span></span>  |
| <span data-ttu-id="44035-145">*api.localaccountsignin*</span><span class="sxs-lookup"><span data-stu-id="44035-145">*api.localaccountsignin*</span></span> | [<span data-ttu-id="44035-146">selfasserted.html</span><span class="sxs-lookup"><span data-stu-id="44035-146">selfasserted.html</span></span>](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | <span data-ttu-id="44035-147">**Přihlašovací stránka místní účet**.</span><span class="sxs-lookup"><span data-stu-id="44035-147">**Local account sign-in page**.</span></span> <span data-ttu-id="44035-148">Tato stránka obsahuje formulář pro přihlašování pomocí místního účtu, který je založený na e-mailovou adresu nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="44035-148">This page contains a form for signing in with a local account that's based on an email address or a user name.</span></span> <span data-ttu-id="44035-149">Formulář může obsahovat vstupní textové pole a pole pro zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="44035-149">The form can contain a text input box and password entry box.</span></span> |
| <span data-ttu-id="44035-150">*api.localaccountsignup*</span><span class="sxs-lookup"><span data-stu-id="44035-150">*api.localaccountsignup*</span></span> | [<span data-ttu-id="44035-151">selfasserted.html</span><span class="sxs-lookup"><span data-stu-id="44035-151">selfasserted.html</span></span>](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | <span data-ttu-id="44035-152">**Místní účet registrační stránku**.</span><span class="sxs-lookup"><span data-stu-id="44035-152">**Local account sign up page**.</span></span> <span data-ttu-id="44035-153">Tato stránka obsahuje formulář pro registraci pro místní účet, který je založený na e-mailovou adresu nebo uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="44035-153">This page contains a form for signing up for a local account that's based on an email address or a user name.</span></span> <span data-ttu-id="44035-154">Formulář může obsahovat různé vstupní ovládací prvky, jako například: textový vstupní pole, pole pro zadání hesla, přepínače, polí rozevíracího seznamu vyberte jeden a více zaškrtněte políčka.</span><span class="sxs-lookup"><span data-stu-id="44035-154">The form can contain various input controls, such as: a text input box, a password entry box, a radio button, single-select drop-down boxes, and multi-select check boxes.</span></span> |
| <span data-ttu-id="44035-155">*api.phonefactor*</span><span class="sxs-lookup"><span data-stu-id="44035-155">*api.phonefactor*</span></span> | [<span data-ttu-id="44035-156">multifactor-1.0.0.cshtml</span><span class="sxs-lookup"><span data-stu-id="44035-156">multifactor-1.0.0.cshtml</span></span>](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | <span data-ttu-id="44035-157">**Stránka služby Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="44035-157">**Multi-factor authentication page**.</span></span> <span data-ttu-id="44035-158">Na této stránce uživatelé mohli ověřit jejich telefonní čísla (pomocí textové nebo hlasové) během registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="44035-158">On this page, users can verify their phone numbers (by using text or voice) during sign-up or sign-in.</span></span> |
| <span data-ttu-id="44035-159">*api.selfasserted*</span><span class="sxs-lookup"><span data-stu-id="44035-159">*api.selfasserted*</span></span> | [<span data-ttu-id="44035-160">selfasserted.html</span><span class="sxs-lookup"><span data-stu-id="44035-160">selfasserted.html</span></span>](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | <span data-ttu-id="44035-161">**Stránku pro přihlášení sociálních účet**.</span><span class="sxs-lookup"><span data-stu-id="44035-161">**Social account sign-up page**.</span></span> <span data-ttu-id="44035-162">Tato stránka obsahuje formulář, který uživatelé musí dokončit při registraci pomocí stávající účet ze sociálních identity zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="44035-162">This page contains a form that users must complete when they sign up by using an existing account from a social identity provider.</span></span> <span data-ttu-id="44035-163">Tato stránka je podobný na předchozí sociálních registrační stránku účtu, s výjimkou pole pro zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="44035-163">This page is similar to the preceding social account sign-up page, except for the password entry fields.</span></span> |
| <span data-ttu-id="44035-164">*api.selfasserted.profileupdate*</span><span class="sxs-lookup"><span data-stu-id="44035-164">*api.selfasserted.profileupdate*</span></span> | [<span data-ttu-id="44035-165">updateprofile.html</span><span class="sxs-lookup"><span data-stu-id="44035-165">updateprofile.html</span></span>](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | <span data-ttu-id="44035-166">**Stránka pro aktualizaci profilu**.</span><span class="sxs-lookup"><span data-stu-id="44035-166">**Profile update page**.</span></span> <span data-ttu-id="44035-167">Tato stránka obsahuje formulář, který mohou uživatelé aktualizovat svůj profil.</span><span class="sxs-lookup"><span data-stu-id="44035-167">This page contains a form that users can access to update their profile.</span></span> <span data-ttu-id="44035-168">Tato stránka je podobná registrační stránku sociálních účtu, s výjimkou pole pro zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="44035-168">This page is similar to the social account sign-up page, except for the password entry fields.</span></span> |
| <span data-ttu-id="44035-169">*api.signuporsignin*</span><span class="sxs-lookup"><span data-stu-id="44035-169">*api.signuporsignin*</span></span> | [<span data-ttu-id="44035-170">unified.html</span><span class="sxs-lookup"><span data-stu-id="44035-170">unified.html</span></span>](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | <span data-ttu-id="44035-171">**Jednotná stránku registrace nebo přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="44035-171">**Unified sign-up or sign-in page**.</span></span> <span data-ttu-id="44035-172">Tato stránka zpracovává proces registrace a přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="44035-172">This page handles the user sign-up and sign-in process.</span></span> <span data-ttu-id="44035-173">Uživatelé mohou pomocí poskytovatelů identit enterprise, poskytovatelů identit sociálních třeba Facebook nebo Google + nebo místní účty.</span><span class="sxs-lookup"><span data-stu-id="44035-173">Users can use enterprise identity providers, social identity providers such as Facebook or Google+, or local accounts.</span></span>  |

## <a name="serving-dynamic-content"></a><span data-ttu-id="44035-174">Poskytování dynamického obsahu</span><span class="sxs-lookup"><span data-stu-id="44035-174">Serving dynamic content</span></span>
<span data-ttu-id="44035-175">V [přizpůsobení konfigurace uživatelského rozhraní ve vlastních zásadách pro](active-directory-b2c-ui-customization-custom.md) článek, odešle HTML5 soubory do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="44035-175">In the [Configure UI customization in a custom policy](active-directory-b2c-ui-customization-custom.md) article, you upload HTML5 files to Azure Blob storage.</span></span> <span data-ttu-id="44035-176">Tyto soubory HTML5 statické a vykreslení stejné HTML obsahu pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="44035-176">Those HTML5 files are static and render the same HTML content for each request.</span></span> 

<span data-ttu-id="44035-177">V tomto článku použijte webové aplikace ASP.NET, který může přijmout parametrů řetězce dotazu a reagují odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="44035-177">In this article, you use an ASP.NET web app, which can accept query string parameters and respond accordingly.</span></span> 

<span data-ttu-id="44035-178">V tomto návodu můžete:</span><span class="sxs-lookup"><span data-stu-id="44035-178">In this walkthrough, you:</span></span>
* <span data-ttu-id="44035-179">Vytvořte webovou aplikaci ASP.NET Core, který je hostitelem vaší šablony HTML5.</span><span class="sxs-lookup"><span data-stu-id="44035-179">Create an ASP.NET Core web application that hosts your HTML5 templates.</span></span> 
* <span data-ttu-id="44035-180">Přidat vlastní šablonu HTML5 _unified.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="44035-180">Add a custom HTML5 template, _unified.cshtml_.</span></span> 
* <span data-ttu-id="44035-181">Publikování webové aplikace do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="44035-181">Publish your web app to Azure App Service.</span></span> 
* <span data-ttu-id="44035-182">Nastavit prostředků mezi zdroji (CORS) pro webovou aplikaci pro sdílení.</span><span class="sxs-lookup"><span data-stu-id="44035-182">Set cross-origin resource sharing (CORS) for your web app.</span></span>
* <span data-ttu-id="44035-183">Přepsání `LoadUri` elementy tak, aby odkazovaly do souboru HTML5.</span><span class="sxs-lookup"><span data-stu-id="44035-183">Override the `LoadUri` elements to point to your HTML5 file.</span></span>

## <a name="step-1-create-an-aspnet-web-app"></a><span data-ttu-id="44035-184">Krok 1: Vytvoření webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="44035-184">Step 1: Create an ASP.NET web app</span></span>

1. <span data-ttu-id="44035-185">V sadě Visual Studio vytvořte projekt tak, že vyberete **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="44035-185">In Visual Studio, create a project by selecting **File** > **New** > **Project**.</span></span>

2. <span data-ttu-id="44035-186">V **nový projekt** vyberte **Visual C#** > **webové** > **webové aplikace ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="44035-186">In the **New Project** window, select **Visual C#** > **Web** > **ASP.NET Core Web Application (.NET Core)**.</span></span>

3. <span data-ttu-id="44035-187">Název aplikace (například *Contoso.AADB2C.UI*) a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="44035-187">Name the application (for example, *Contoso.AADB2C.UI*), and then select **OK**.</span></span>

    ![Vytvoření nového projektu sady Visual Studio](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project1.png)

4. <span data-ttu-id="44035-189">Vyberte **webové aplikace** šablony.</span><span class="sxs-lookup"><span data-stu-id="44035-189">Select the **Web Application** template.</span></span>

5. <span data-ttu-id="44035-190">Nastavení ověřování **bez ověřování**.</span><span class="sxs-lookup"><span data-stu-id="44035-190">Set the authentication to **No Authentication**.</span></span>

    ![Vyberte šablonu webové aplikace](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project2.png)

6. <span data-ttu-id="44035-192">Vyberte **OK** a vytvořte tak projekt.</span><span class="sxs-lookup"><span data-stu-id="44035-192">Select **OK** to create the project.</span></span>

## <a name="step-2-create-mvc-view"></a><span data-ttu-id="44035-193">Krok 2: Vytvoření zobrazení MVC</span><span class="sxs-lookup"><span data-stu-id="44035-193">Step 2: Create MVC view</span></span>
### <a name="step-21-download-the-b2c-built-in-html5-template"></a><span data-ttu-id="44035-194">Krok 2.1: Stáhněte si předdefinované šablony HTML5 B2C</span><span class="sxs-lookup"><span data-stu-id="44035-194">Step 2.1: Download the B2C built-in HTML5 template</span></span>
<span data-ttu-id="44035-195">Vlastní šablony HTML5 je založený na šabloně předdefinované HTML5 Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="44035-195">Your custom HTML5 template is based on the Azure AD B2C built-in HTML5 template.</span></span> <span data-ttu-id="44035-196">Si můžete stáhnout [unified.html soubor](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) nebo stáhnout šablonu z [starter pack](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates/wingtip).</span><span class="sxs-lookup"><span data-stu-id="44035-196">You can download the [unified.html file](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) or download the template from [starter pack](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates/wingtip).</span></span> <span data-ttu-id="44035-197">Tento soubor HTML5 použijete k vytvoření jednotné stránku registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="44035-197">You use this HTML5 file to create a unified sign-up or sign-in page.</span></span>

### <a name="step-22-add-the-mvc-view"></a><span data-ttu-id="44035-198">Krok 2.2: Přidejte zobrazení MVC</span><span class="sxs-lookup"><span data-stu-id="44035-198">Step 2.2: Add the MVC view</span></span>
1. <span data-ttu-id="44035-199">Klikněte pravým tlačítkem na zobrazení/domovskou složku a potom **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="44035-199">Right-click the Views/Home folder, and then **Add** > **New Item**.</span></span>

    ![Přidat novou položku MVC](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view1.png)

2. <span data-ttu-id="44035-201">V **přidat novou položku – Contoso.AADB2C.UI** vyberte **Web > ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="44035-201">In the **Add New Item - Contoso.AADB2C.UI** window, select **Web > ASP.NET**.</span></span>

3. <span data-ttu-id="44035-202">Vyberte **MVC zobrazení stránky**.</span><span class="sxs-lookup"><span data-stu-id="44035-202">Select **MVC View Page**.</span></span>

4. <span data-ttu-id="44035-203">V **název** pole, změňte název **unified.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="44035-203">In the **Name** box, change the name to **unified.cshtml**.</span></span>

5. <span data-ttu-id="44035-204">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="44035-204">Select **Add**.</span></span>

    ![Přidání zobrazení MVC](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view2.png)

6. <span data-ttu-id="44035-206">Pokud *unified.cshtml* soubor již není otevřený, poklikejte na soubor a otevře se a pak vymazat obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="44035-206">If the *unified.cshtml* file is not open already, double-click the file to open it, and then clear the file contents.</span></span>

7. <span data-ttu-id="44035-207">V tomto návodu budeme odeberte odkaz na stránce rozložení.</span><span class="sxs-lookup"><span data-stu-id="44035-207">For this walkthrough, we remove the reference to layout-page.</span></span> <span data-ttu-id="44035-208">Přidejte následující fragment kódu do _unified.cshtml_:</span><span class="sxs-lookup"><span data-stu-id="44035-208">Add the following code snippet to _unified.cshtml_:</span></span>

    ```csharp
    @{
        Layout = null;
    }
    ```

8. <span data-ttu-id="44035-209">Otevřete _unified.cshtml_ soubor stažený z Azure AD B2C předdefinované šablony HTML5.</span><span class="sxs-lookup"><span data-stu-id="44035-209">Open the _unified.cshtml_ file you downloaded from Azure AD B2C built-in HTML5 template.</span></span>

9. <span data-ttu-id="44035-210">Nahraďte jeden znak (@) s dvojitou znak @@.</span><span class="sxs-lookup"><span data-stu-id="44035-210">Replace the single at signs (@) with double at signs (@@).</span></span>

10. <span data-ttu-id="44035-211">Obsah souboru zkopírujte a vložte ji pod definici rozložení.</span><span class="sxs-lookup"><span data-stu-id="44035-211">Copy the content of the file and paste it below the Layout definition.</span></span> <span data-ttu-id="44035-212">Váš kód by měl vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="44035-212">Your code should look like:</span></span>

    ![Po přidání HTML5 Unified.cshtml soubor](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-edit-view1.png)

### <a name="step-23-change-the-background-image"></a><span data-ttu-id="44035-214">Krok 2.3: Změna rozložení obrázku na pozadí</span><span class="sxs-lookup"><span data-stu-id="44035-214">Step 2.3: Change the background image</span></span>

<span data-ttu-id="44035-215">Vyhledejte `<img>` elementu, který obsahuje `ID` hodnotu *background_background_image*a potom `src` hodnotu s **https://kbdevstorage1.blob.core.windows.net/ Asset – objekty BLOB nebo 19889_en_1** nebo jiného obrázku pozadí, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="44035-215">Locate the `<img>` element that contains the `ID` value *background_background_image*, and then replace the `src` value with **https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1** or any other background image you want to use.</span></span>

![Změní pozadí stránky](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-static-background.png)

### <a name="step-24-add-your-view-to-the-mvc-controller"></a><span data-ttu-id="44035-217">Krok 2.4: Přidáte zobrazení do řadiče MVC</span><span class="sxs-lookup"><span data-stu-id="44035-217">Step 2.4: Add your view to the MVC controller</span></span>

1. <span data-ttu-id="44035-218">Otevřete **Controllers\HomeController.cs**a přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="44035-218">Open **Controllers\HomeController.cs**, and add following method:</span></span> 

    ```C
    public IActionResult unified()
    {
        return View();
    }
    ```
    <span data-ttu-id="44035-219">Tento kód určuje, že by měl používat metodu *zobrazení* soubor šablony k vykreslení odpovědi do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="44035-219">This code specifies that the method should use a *View* template file to render a response to the browser.</span></span> <span data-ttu-id="44035-220">Protože jsme explicitně nezadávejte název *zobrazení* soubor šablony MVC používá ve výchozím nastavení _unified.cshtml_ zobrazení souboru v *nebo zobrazení, domácí* složky.</span><span class="sxs-lookup"><span data-stu-id="44035-220">Because we don't explicitly specify the name of the *View* template file, MVC defaults to using the _unified.cshtml_ View file in the */Views/Home* folder.</span></span>
    
    <span data-ttu-id="44035-221">Po přidání _jednotná_ metoda, váš kód by měl vypadat jako:</span><span class="sxs-lookup"><span data-stu-id="44035-221">After you add the _unified_ method, your code should look like:</span></span>
    
    ![Změna řadiče k vykreslení zobrazení](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-controller-view.png)

2. <span data-ttu-id="44035-223">Ladění webové aplikace a ujistěte se, že _jednotná_ stránka je přístupná (například `http://localhost:<Port number>/Home/unified`).</span><span class="sxs-lookup"><span data-stu-id="44035-223">Debug your web app, and make sure that the _unified_ page is accessible (for example, `http://localhost:<Port number>/Home/unified`).</span></span>

### <a name="step-25-publish-to-azure"></a><span data-ttu-id="44035-224">Krok 2.5: Publikování aplikace do Azure</span><span class="sxs-lookup"><span data-stu-id="44035-224">Step 2.5: Publish to Azure</span></span>
1. <span data-ttu-id="44035-225">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **Contoso.AADB2C.UI** projektu a potom vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="44035-225">In **Solution Explorer**, right-click the **Contoso.AADB2C.UI** project, and then select **Publish**.</span></span>

    ![Publikování do služby Microsoft Azure App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish1.png)

2. <span data-ttu-id="44035-227">Vyberte **Microsoft Azure App Service** dlaždici a potom vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="44035-227">Select the **Microsoft Azure App Service** tile, and then select **Publish**.</span></span>

    ![Vytvořit nový Microsoft Azure App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish2.png)

    <span data-ttu-id="44035-229">**Vytvořit službu App Service** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="44035-229">The **Create App Service** window opens.</span></span> <span data-ttu-id="44035-230">V něm můžete začít vytvářet všechny potřebné prostředky Azure ke spouštění webové aplikace ASP.NET v Azure.</span><span class="sxs-lookup"><span data-stu-id="44035-230">In it you can begin to create all the necessary Azure resources to run the ASP.NET web app in Azure.</span></span>

    > [!NOTE]
    > <span data-ttu-id="44035-231">Další informace o publikování najdete v tématu [vytvoření webové aplikace ASP.NET v Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).</span><span class="sxs-lookup"><span data-stu-id="44035-231">For more information about publishing, see [Create an ASP.NET web app in Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).</span></span>

3. <span data-ttu-id="44035-232">V **název webové aplikace** zadejte jedinečným názvem aplikace (platné znaky jsou a – z, A-Z, 0 – 9 a pomlčku (-).</span><span class="sxs-lookup"><span data-stu-id="44035-232">In the **Web App Name** box, type a unique app name (valid characters are a-z, A-Z, 0-9, and the hyphen (-).</span></span> <span data-ttu-id="44035-233">Adresa URL webové aplikace je `http://<app_name>.azurewebsites.NET`, kde `<app_name>` je název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="44035-233">The URL of the web app is `http://<app_name>.azurewebsites.NET`, where `<app_name>` is your web app name.</span></span> <span data-ttu-id="44035-234">Můžete přijmout automaticky vygenerovaný název, který je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="44035-234">You can accept the automatically generated name, which is unique.</span></span>

4. <span data-ttu-id="44035-235">Výběrem možnosti **Vytvořit** spustíte vytváření prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="44035-235">Select **Create** to start creating the Azure resources.</span></span>

    ![Zadejte vlastnosti služby App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish3.png)

    <span data-ttu-id="44035-237">Po dokončení procesu vytváření průvodce publikuje webové aplikace ASP.NET ve službě Azure a pak spustí aplikaci ve výchozím prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="44035-237">After the creation process is complete, the wizard publishes the ASP.NET web app to Azure and then launches the app in the default browser.</span></span>

5. <span data-ttu-id="44035-238">Zkopírujte adresu URL _jednotná_ stránky (například _https://<app_name>.azurewebsites.net/home/unified_).</span><span class="sxs-lookup"><span data-stu-id="44035-238">Copy the URL of the _unified_ page (for example, _https://<app_name>.azurewebsites.net/home/unified_).</span></span>

## <a name="step-3-configure-cors-in-azure-app-service"></a><span data-ttu-id="44035-239">Krok 3: Konfigurace CORS v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="44035-239">Step 3: Configure CORS in Azure App Service</span></span>
1. <span data-ttu-id="44035-240">V [portál Azure](https://portal.azure.com/), vyberte **App Services**a pak vyberte název aplikace API.</span><span class="sxs-lookup"><span data-stu-id="44035-240">In the [Azure portal](https://portal.azure.com/), Select **App Services**, and then select the name of your API app.</span></span>

    ![Výběr aplikace API na portálu Azure](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS1.png)

2. <span data-ttu-id="44035-242">V **nastavení** v oblasti **rozhraní API** vyberte **CORS**.</span><span class="sxs-lookup"><span data-stu-id="44035-242">In the **Settings** section, under **API** section, select **CORS**.</span></span>

    ![Vyberte nastavení CORS](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS2.png)

3. <span data-ttu-id="44035-244">V **CORS** okno v **povolené zdroje** pole, proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="44035-244">In the **CORS** window, in the **Allowed Origins** box, do either of the following:</span></span>

    * <span data-ttu-id="44035-245">Zadejte adresu nebo adresy URL, které chcete povolit volání JavaScriptu z.</span><span class="sxs-lookup"><span data-stu-id="44035-245">Enter the URL or URLs that you want to allow JavaScript calls to come from.</span></span>
    * <span data-ttu-id="44035-246">Zadejte hvězdičku (\*) Chcete-li povolit všechny zdrojové domény.</span><span class="sxs-lookup"><span data-stu-id="44035-246">Enter an asterisk (\*) to specify that all origin domains are accepted.</span></span>

4. <span data-ttu-id="44035-247">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="44035-247">Select **Save**.</span></span>

    ![Okno CORS](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS3.png)

    <span data-ttu-id="44035-249">Po výběru **Uložit**, aplikace API přijímat volání JavaScriptu ze zadaných adres URL.</span><span class="sxs-lookup"><span data-stu-id="44035-249">After you select **Save**, the API app accepts JavaScript calls from the specified URLs.</span></span> 

## <a name="step-4-html5-template-validation"></a><span data-ttu-id="44035-250">Krok 4: Ověření šablony HTML5</span><span class="sxs-lookup"><span data-stu-id="44035-250">Step 4: HTML5 template validation</span></span>
<span data-ttu-id="44035-251">Šablona HTML5 je připravený k použití.</span><span class="sxs-lookup"><span data-stu-id="44035-251">Your HTML5 template is ready to use.</span></span> <span data-ttu-id="44035-252">Však není k dispozici v `ContentDefinition` kódu.</span><span class="sxs-lookup"><span data-stu-id="44035-252">However, it is not available in the `ContentDefinition` code.</span></span> <span data-ttu-id="44035-253">Chcete-li přidat `ContentDefinition` vlastní zásady, ověřte, že:</span><span class="sxs-lookup"><span data-stu-id="44035-253">Before you can add `ContentDefinition` to your custom policy, ensure that:</span></span>
* <span data-ttu-id="44035-254">Obsah je HTML5 kompatibilní a přístupné.</span><span class="sxs-lookup"><span data-stu-id="44035-254">Your content is HTML5 compliant and accessible.</span></span>
* <span data-ttu-id="44035-255">Vaše servery obsahu je povolený pro CORS.</span><span class="sxs-lookup"><span data-stu-id="44035-255">Your content server is enabled for CORS.</span></span>

    >[!NOTE]
    ><span data-ttu-id="44035-256">Ověřte, zda má povolenou CORS lokality, kde jste hostování obsahu a můžete otestovat požadavků CORS, přejděte na [test cors.org](http://test-cors.org/) webu.</span><span class="sxs-lookup"><span data-stu-id="44035-256">To verify that the site where you're hosting your content has enabled CORS and can test CORS requests, go to the [test-cors.org](http://test-cors.org/) website.</span></span> 

* <span data-ttu-id="44035-257">Obsloužit obsah je zabezpečené přes **HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="44035-257">Your served content is secure over **HTTPS**.</span></span>
* <span data-ttu-id="44035-258">Používáte *absolutní adresy URL*, jako například *https://yourdomain/content*, pro všechny odkazy, obsah šablon stylů CSS a obrázků.</span><span class="sxs-lookup"><span data-stu-id="44035-258">You are using *absolute URLS*, such as *https://yourdomain/content*, for all links, CSS content, and images.</span></span>

## <a name="step-5-configure-your-content-definition"></a><span data-ttu-id="44035-259">Krok 5: Konfigurace vašeho obsahu definice</span><span class="sxs-lookup"><span data-stu-id="44035-259">Step 5: Configure your content definition</span></span>
<span data-ttu-id="44035-260">Ke konfiguraci `ContentDefinition`, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="44035-260">To configure `ContentDefinition`, do the following:</span></span>
1. <span data-ttu-id="44035-261">Otevřete soubor základní zásad (například *TrustFrameworkBase.xml*).</span><span class="sxs-lookup"><span data-stu-id="44035-261">Open the base file of your policy (for example, *TrustFrameworkBase.xml*).</span></span>

2. <span data-ttu-id="44035-262">Vyhledejte `<ContentDefinitions>` elementu a poté zkopírujte celý obsah `<ContentDefinitions>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="44035-262">Search for the `<ContentDefinitions>` element, and then copy the entire contents of the `<ContentDefinitions>` node.</span></span>

3. <span data-ttu-id="44035-263">Otevřete soubor rozšíření (například *TrustFrameworkExtensions.xml*) a poté vyhledejte `<BuildingBlocks>` elementu.</span><span class="sxs-lookup"><span data-stu-id="44035-263">Open the extension file (for example, *TrustFrameworkExtensions.xml*) and then search for the `<BuildingBlocks>` element.</span></span> <span data-ttu-id="44035-264">Pokud element neexistuje, přidejte ji.</span><span class="sxs-lookup"><span data-stu-id="44035-264">If the element doesn't exist, add it.</span></span>

4. <span data-ttu-id="44035-265">Vložte celý obsah `<ContentDefinitions>` uzlu, který jste zkopírovali jako podřízenou `<BuildingBlocks>` elementu.</span><span class="sxs-lookup"><span data-stu-id="44035-265">Paste the entire contents of the `<ContentDefinitions>` node that you copied as a child of the `<BuildingBlocks>` element.</span></span> 

5. <span data-ttu-id="44035-266">Vyhledejte `<ContentDefinition>` uzlu, který obsahuje `Id="api.signuporsignin"` v souboru XML, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="44035-266">Search for the `<ContentDefinition>` node that contains `Id="api.signuporsignin"` in the XML that you copied.</span></span>

6. <span data-ttu-id="44035-267">Změňte hodnotu `LoadUri` z _~/tenant/default/unified_ k _https://<app_name>.azurewebsites.net/home/unified_.</span><span class="sxs-lookup"><span data-stu-id="44035-267">Change the value of `LoadUri` from _~/tenant/default/unified_ to _https://<app_name>.azurewebsites.net/home/unified_.</span></span>  
    <span data-ttu-id="44035-268">Vaše vlastní zásada by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="44035-268">Your custom policy should look like the following:</span></span>
    
    ![Vaše obsahu definice](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-content-definition.png)

## <a name="step-6-upload-the-policy-to-your-tenant"></a><span data-ttu-id="44035-270">Krok 6: Nahrajte zásady klienta</span><span class="sxs-lookup"><span data-stu-id="44035-270">Step 6: Upload the policy to your tenant</span></span>
1. <span data-ttu-id="44035-271">V [portál Azure](https://portal.azure.com), přepnout [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a potom vyberte **Azure AD B2C**.</span><span class="sxs-lookup"><span data-stu-id="44035-271">In the [Azure portal](https://portal.azure.com), switch to the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then select **Azure AD B2C**.</span></span>

2. <span data-ttu-id="44035-272">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="44035-272">Select **Identity Experience Framework**.</span></span>

3. <span data-ttu-id="44035-273">Vyberte **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="44035-273">Select **All Policies**.</span></span>

4. <span data-ttu-id="44035-274">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="44035-274">Select **Upload Policy**.</span></span>

5. <span data-ttu-id="44035-275">Vyberte **přepsat zásady, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="44035-275">Select the **Overwrite the policy if it exists** check box.</span></span>

6. <span data-ttu-id="44035-276">Nahrát *TrustFrameworkExtensions.xml* souboru a ujistěte se, že předává ověření.</span><span class="sxs-lookup"><span data-stu-id="44035-276">Upload the *TrustFrameworkExtensions.xml* file, and ensure that it passes validation.</span></span>

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="44035-277">Krok 7: Testování spustit pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="44035-277">Step 7: Test the custom policy by using Run Now</span></span>
1. <span data-ttu-id="44035-278">Vyberte **nastavení Azure AD B2C**a potom vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="44035-278">Select **Azure AD B2C Settings**, and then select **Identity Experience Framework**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="44035-279">Spustit nyní vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="44035-279">Run Now requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="44035-280">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="44035-280">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>

2. <span data-ttu-id="44035-281">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, které jste nahráli a potom vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="44035-281">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded, and then select **Run now**.</span></span>  
    <span data-ttu-id="44035-282">Nyní byste měli mít v tématu vaše vlastní HTML5 s na pozadí, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="44035-282">You should be able to see your custom HTML5 with the background that you created earlier.</span></span>

    ![Vaše zásady registrace nebo přihlášení](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo1.png)

## <a name="step-8-add-dynamic-content"></a><span data-ttu-id="44035-284">Krok 8: Přidejte dynamický obsah</span><span class="sxs-lookup"><span data-stu-id="44035-284">Step 8: Add dynamic content</span></span>
<span data-ttu-id="44035-285">Změní pozadí podle parametru řetězce dotazu s názvem _campaignId_.</span><span class="sxs-lookup"><span data-stu-id="44035-285">Change the background based on query string parameter named _campaignId_.</span></span> <span data-ttu-id="44035-286">Vaše aplikace RP (webových a mobilních aplikací) odešle parametr do Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="44035-286">Your RP application (web and mobile apps) sends the parameter to Azure AD B2C.</span></span> <span data-ttu-id="44035-287">Vaše zásady čte parametr a jeho hodnotu do šablony HTML5.</span><span class="sxs-lookup"><span data-stu-id="44035-287">Your policy reads the parameter and sends its value to your HTML5 template.</span></span> 

### <a name="step-81-add-a-content-definition-parameter"></a><span data-ttu-id="44035-288">Krok 8.1: Parametr obsahu definice přidat</span><span class="sxs-lookup"><span data-stu-id="44035-288">Step 8.1: Add a content definition parameter</span></span>

<span data-ttu-id="44035-289">Přidat `ContentDefinitionParameters` element provedením následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="44035-289">Add the `ContentDefinitionParameters` element by doing the following:</span></span>
1. <span data-ttu-id="44035-290">Otevřete *SignUpOrSignin* soubor zásad (například *SignUpOrSignin.xml*).</span><span class="sxs-lookup"><span data-stu-id="44035-290">Open the *SignUpOrSignin* file of your policy (for example, *SignUpOrSignin.xml*).</span></span>

2. <span data-ttu-id="44035-291">Vyhledejte `<DefaultUserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="44035-291">Search for the `<DefaultUserJourney>` node.</span></span> 

3. <span data-ttu-id="44035-292">V `<DefaultUserJourney>` uzlu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="44035-292">In the `<DefaultUserJourney>` node, add the following XML snippet:</span></span>  

    ```XML
    <UserJourneyBehaviors>
        <ContentDefinitionParameters>
            <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    ```

### <a name="step-82-change-your-code-to-accept-a-query-string-parameter-and-replace-the-background-image"></a><span data-ttu-id="44035-293">Krok 8.2: Změnit kód tak, aby přijímal parametr řetězce dotazu a nahraďte obrázku pozadí</span><span class="sxs-lookup"><span data-stu-id="44035-293">Step 8.2: Change your code to accept a query string parameter, and replace the background image</span></span>
<span data-ttu-id="44035-294">Změnit HomeController `unified` metoda tak, aby přijímal parametr campaignId.</span><span class="sxs-lookup"><span data-stu-id="44035-294">Modify the HomeController `unified` method to accept the campaignId parameter.</span></span> <span data-ttu-id="44035-295">Parametr poté zkontroluje tato metoda je hodnota a nastaví `ViewData["background"]` proměnná odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="44035-295">The method then checks the parameter's value and sets the `ViewData["background"]` variable accordingly.</span></span>

1. <span data-ttu-id="44035-296">Otevřete *Controllers\HomeController.cs* souboru a poté změňte `unified` metoda přidáním následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="44035-296">Open the *Controllers\HomeController.cs* file, and then change the `unified` method by adding the following code snippet:</span></span>

    ```csharp
    public IActionResult unified(string campaignId)
    {
        // If campaign ID is Hawaii, show Hawaii background
        if (campaignId != null && campaignId.ToLower() == "hawaii")
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1";
        }
        // If campaign ID is Tokyo, show Tokyo background
        else if (campaignId != null && campaignId.ToLower() == "tokyo")
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/19666_en_1";
        }
        // Default background
        else
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/18983_en_1";
        }

        return View();
    }

    ```

2. <span data-ttu-id="44035-297">Vyhledejte `<img>` element s ID `background_background_image`a nahraďte `src` hodnotu s `@ViewData["background"]`.</span><span class="sxs-lookup"><span data-stu-id="44035-297">Locate the `<img>` element with ID `background_background_image`, and replace the `src` value with `@ViewData["background"]`.</span></span>

    ![Změní pozadí stránky](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-dynamic-background.png)

### <a name="83-upload-the-changes-and-publish-your-policy"></a><span data-ttu-id="44035-299">8.3: odešlete změny a publikování zásady</span><span class="sxs-lookup"><span data-stu-id="44035-299">8.3: Upload the changes and publish your policy</span></span>
1. <span data-ttu-id="44035-300">Publikování projektu sady Visual Studio do služby Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="44035-300">Publish your Visual Studio project to Azure App Service.</span></span>

2. <span data-ttu-id="44035-301">Nahrát *SignUpOrSignin.xml* zásad do Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="44035-301">Upload the *SignUpOrSignin.xml* policy to Azure AD B2C.</span></span>

3. <span data-ttu-id="44035-302">Otevřete **B2C_1A_signup_signin**, RP vlastních zásad, které jste nahráli a potom vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="44035-302">Open **B2C_1A_signup_signin**, the RP custom policy that you uploaded, and then select **Run now**.</span></span>  
    <span data-ttu-id="44035-303">Měli byste vidět stejné obrázku pozadí, která byla dříve zobrazena.</span><span class="sxs-lookup"><span data-stu-id="44035-303">You should see the same background image that was previously displayed.</span></span>

4. <span data-ttu-id="44035-304">Zkopírujte adresu URL z panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="44035-304">Copy the URL from the browser's address bar.</span></span>

5. <span data-ttu-id="44035-305">Přidat _campaignId_ parametr řetězce k identifikátoru URI dotazu.</span><span class="sxs-lookup"><span data-stu-id="44035-305">Add the _campaignId_ query string parameter to the URI.</span></span> <span data-ttu-id="44035-306">Například přidejte `&campaignId=hawaii`, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="44035-306">For example, add `&campaignId=hawaii`, as shown in following image:</span></span>

    ![Změní pozadí stránky](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-campaignId-param.png)

6. <span data-ttu-id="44035-308">Vyberte **Enter** zobrazíte Havaj obrázku pozadí.</span><span class="sxs-lookup"><span data-stu-id="44035-308">Select **Enter** to display the Hawaii background image.</span></span>

    ![Změní pozadí stránky](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo2.png)

7. <span data-ttu-id="44035-310">Změňte hodnotu na *Tokio*a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="44035-310">Change the value to *Tokyo*, and then select **Enter**.</span></span>  
    <span data-ttu-id="44035-311">Prohlížeč zobrazí Tokio pozadí.</span><span class="sxs-lookup"><span data-stu-id="44035-311">The browser displays the Tokyo background.</span></span>

    ![Změní pozadí stránky](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo3.png)

## <a name="step-9-change-the-rest-of-the-user-journey"></a><span data-ttu-id="44035-313">Krok 9: Změňte zbytek cesty uživatele</span><span class="sxs-lookup"><span data-stu-id="44035-313">Step 9: Change the rest of the user journey</span></span>
<span data-ttu-id="44035-314">Pokud vyberete **nyní** odkaz na přihlašovací stránce v prohlížeči zobrazí výchozí obrázek pozadí, definované není obrázek.</span><span class="sxs-lookup"><span data-stu-id="44035-314">If you select the **Sign up now** link on the sign-in page, the browser displays the default background image, not the image you defined.</span></span> <span data-ttu-id="44035-315">Toto chování nastane, protože jste změnili pouze stránku registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="44035-315">This behavior arises because you've changed only the sign-up or sign-in page.</span></span> <span data-ttu-id="44035-316">Chcete-li změnit zbytek samoobslužné Assert obsahu definice:</span><span class="sxs-lookup"><span data-stu-id="44035-316">To change the rest of the Self-Assert content definitions:</span></span>
1. <span data-ttu-id="44035-317">Přejděte zpět na krok 2 a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="44035-317">Go back to "Step 2," and do the following:</span></span>

    <span data-ttu-id="44035-318">a.</span><span class="sxs-lookup"><span data-stu-id="44035-318">a.</span></span> <span data-ttu-id="44035-319">Stažení *selfasserted* souboru.</span><span class="sxs-lookup"><span data-stu-id="44035-319">Download the *selfasserted* file.</span></span>

    <span data-ttu-id="44035-320">b.</span><span class="sxs-lookup"><span data-stu-id="44035-320">b.</span></span> <span data-ttu-id="44035-321">Zkopírujte obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="44035-321">Copy the file content.</span></span>

    <span data-ttu-id="44035-322">c.</span><span class="sxs-lookup"><span data-stu-id="44035-322">c.</span></span> <span data-ttu-id="44035-323">Vytvořit nové zobrazení, *selfasserted*.</span><span class="sxs-lookup"><span data-stu-id="44035-323">Create a new view, *selfasserted*.</span></span>

    <span data-ttu-id="44035-324">d.</span><span class="sxs-lookup"><span data-stu-id="44035-324">d.</span></span> <span data-ttu-id="44035-325">Přidat *selfasserted* k **Domů** řadiče.</span><span class="sxs-lookup"><span data-stu-id="44035-325">Add *selfasserted* to the **Home** controller.</span></span>

2. <span data-ttu-id="44035-326">Vraťte se zpátky a "Krok 4" a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="44035-326">Go back to "Step 4," and do the following:</span></span> 

    <span data-ttu-id="44035-327">a.</span><span class="sxs-lookup"><span data-stu-id="44035-327">a.</span></span> <span data-ttu-id="44035-328">V zásadách rozšíření najít `<ContentDefinition>` uzlu, který obsahuje `Id="api.selfasserted"`, `Id="api.localaccountsignup"`, a `Id="api.localaccountpasswordreset"`.</span><span class="sxs-lookup"><span data-stu-id="44035-328">In your extension policy, find the `<ContentDefinition>` node that contains `Id="api.selfasserted"`, `Id="api.localaccountsignup"`, and `Id="api.localaccountpasswordreset"`.</span></span>

    <span data-ttu-id="44035-329">b.</span><span class="sxs-lookup"><span data-stu-id="44035-329">b.</span></span> <span data-ttu-id="44035-330">Nastavte `LoadUri` atribut vaše *selfasserted* identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="44035-330">Set the `LoadUri` attribute to your *selfasserted* URI.</span></span>

3. <span data-ttu-id="44035-331">Vraťte se zpátky a "Krok 8.2" a změnit kód tak, aby přijímal parametrů řetězce dotazu, ale této doby *selfasserted* funkce.</span><span class="sxs-lookup"><span data-stu-id="44035-331">Go back to "Step 8.2," and change your code to accept query string parameters, but this time to the *selfasserted* function.</span></span> 

4. <span data-ttu-id="44035-332">Nahrát *TrustFrameworkExtensions.xml* zásady a ujistěte se, že předává ověření.</span><span class="sxs-lookup"><span data-stu-id="44035-332">Upload the *TrustFrameworkExtensions.xml* policy, and ensure that it passes validation.</span></span>

5. <span data-ttu-id="44035-333">Spuštění testu zásady a pak vyberte **nyní** zobrazíte výsledek.</span><span class="sxs-lookup"><span data-stu-id="44035-333">Run the policy test, and then select **Sign up now** to see the result.</span></span>

## <a name="optional-download-the-complete-policy-files-and-code"></a><span data-ttu-id="44035-334">(Volitelné) Stáhnout soubory dokončení zásad a kódu</span><span class="sxs-lookup"><span data-stu-id="44035-334">(Optional) Download the complete policy files and code</span></span>
* <span data-ttu-id="44035-335">Po dokončení [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) návod, doporučujeme vám vytvořit váš scénář pomocí vlastních zásad pro soubory.</span><span class="sxs-lookup"><span data-stu-id="44035-335">After you complete the [Get started with custom policies](active-directory-b2c-get-started-custom.md) walkthrough, we recommend that you build your scenario by using your own custom policy files.</span></span> <span data-ttu-id="44035-336">Pro vaši informaci uvádíme [ukázkové soubory zásad](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="44035-336">For your reference, we have provided [Sample policy files](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).</span></span>
* <span data-ttu-id="44035-337">Si můžete stáhnout kompletní kód z [řešení sady Visual Studio ukázkový pro referenci](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="44035-337">You can download the complete code from [Sample Visual Studio solution for reference](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).</span></span>




