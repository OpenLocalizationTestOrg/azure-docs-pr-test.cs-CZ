---
title: "Azure Active Directory B2C:Language přizpůsobení v vlastní zásady | Microsoft Docs"
description: "Naučte se používat obsah vlastních zásad pro víc jazyků pro lokalizaci"
services: active-directory-b2c
documentationcenter: 
author: sammak
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 11/13/2017
ms.author: sama
ms.openlocfilehash: 4ed9791d6590e3982a1bc79b96f8592995bc315c
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
#<a name="language-customization-in-custom-policies"></a><span data-ttu-id="14c8a-103">Vlastní nastavení jazyka v vlastní zásady</span><span class="sxs-lookup"><span data-stu-id="14c8a-103">Language customization in custom policies</span></span>

> [!NOTE]
> <span data-ttu-id="14c8a-104">Tato funkce je ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="14c8a-104">This feature is in public preview.</span></span>
> 

<span data-ttu-id="14c8a-105">V vlastní zásady funguje jazyk přizpůsobení stejné jako integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="14c8a-105">In custom policies, language customization works the same as in built-in policies.</span></span>  <span data-ttu-id="14c8a-106">V tématu integrované [dokumentace](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-language-customization) jejich chování v tom, jak jazyk je zvolen v závislosti na parametry a nastavení prohlížeče, který popisuje.</span><span class="sxs-lookup"><span data-stu-id="14c8a-106">See the built-in [documentation](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-language-customization) that describes the behavior in how a language is chosen based on the parameters and browser settings.</span></span>

##<a name="enable-supported-languages"></a><span data-ttu-id="14c8a-107">Povolit podporované jazyky</span><span class="sxs-lookup"><span data-stu-id="14c8a-107">Enable supported languages</span></span>
<span data-ttu-id="14c8a-108">Pokud nebyl zadán uživatelského rozhraní – národní prostředí a prohlížeče uživatele požádá o jednu z těchto jazyků, podporovaných jazyků se zobrazí uživateli.</span><span class="sxs-lookup"><span data-stu-id="14c8a-108">If ui-locales was not specified and the user's browser asks for one of these languages, supported languages are shown to the user.</span></span>  

<span data-ttu-id="14c8a-109">Podporované jazyky jsou definovány v `<BuildingBlocks>` v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="14c8a-109">Supported languages are defined in `<BuildingBlocks>` in the following format:</span></span>

```XML
<BuildingBlocks>
  <Localization>
    <SupportedLanguages DefaultLanguage="en">
      <SupportedLanguage>qps-ploc</SupportedLanguage>
      <SupportedLanguage>en</SupportedLanguage>
    </SupportedLanguages>
  </Localization>
</BuildingBlocks>
```

<span data-ttu-id="14c8a-110">Výchozí jazyk a podporovaných jazyků, chovají stejným způsobem jako ve integrovaných zásad.</span><span class="sxs-lookup"><span data-stu-id="14c8a-110">Default language and supported languages behave in the same way as they do in built-in policies.</span></span>

##<a name="enable-custom-language-strings"></a><span data-ttu-id="14c8a-111">Povolit vlastní jazyk řetězce</span><span class="sxs-lookup"><span data-stu-id="14c8a-111">Enable custom language strings</span></span>

<span data-ttu-id="14c8a-112">Vytváření vlastní jazyk řetězců vyžaduje dva kroky:</span><span class="sxs-lookup"><span data-stu-id="14c8a-112">Creating custom language strings requires two steps:</span></span>
1. <span data-ttu-id="14c8a-113">Upravit `<ContentDefinition>` pro stránku a zadejte ID prostředku pro požadované jazyky</span><span class="sxs-lookup"><span data-stu-id="14c8a-113">Edit the `<ContentDefinition>` for the page to specify a resource ID for the desired languages</span></span>
2. <span data-ttu-id="14c8a-114">Vytvořte `<LocalizedResources>` s odpovídající ID vašeho `<BuildingBlocks>`</span><span class="sxs-lookup"><span data-stu-id="14c8a-114">Create the `<LocalizedResources>` with corresponding IDs in your `<BuildingBlocks>`</span></span>

<span data-ttu-id="14c8a-115">Mějte na paměti, který můžete umístit `<ContentDefinition>` a `<BuildingBlock>` v souboru rozšíření nebo předávající soubor zásad, který v závislosti na tom, zda chcete změny být ve všech dědičných zásad, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="14c8a-115">Keep in mind that you can put a `<ContentDefinition>` and `<BuildingBlock>` in both your extension file or the relying policy file depending on whether you want the changes to be in all your inheriting policies or not.</span></span>

###<a name="edit-the-contentdefinition-for-the-page"></a><span data-ttu-id="14c8a-116">Upravit ContentDefinition pro stránku.</span><span class="sxs-lookup"><span data-stu-id="14c8a-116">Edit the ContentDefinition for the page</span></span>

<span data-ttu-id="14c8a-117">Pro každou stránku lokalizovat, můžete zadat v `<ContentDefinition>` prostředky jazyk má být vyhledán každý kód jazyka.</span><span class="sxs-lookup"><span data-stu-id="14c8a-117">For each page you want to localize, you can specify in the `<ContentDefinition>` what language resources to look for each language code.</span></span>

```XML
<ContentDefinition Id="api.signuporsignin">
      <LocalizedResourcesReferences>
        <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="fr" />
        <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="en" />
      </LocalizedResourcesReferences>
</ContentDefinition>
```

<span data-ttu-id="14c8a-118">V této ukázce francouzština (fr) a vlastní řetězce Angličtina (en) se přidají na stránku Unified registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="14c8a-118">In this sample, French (fr) and English (en) custom strings are added to the Unified sign-up or sign-in page.</span></span>  <span data-ttu-id="14c8a-119">`LocalizedResourcesReferenceId` Pro každou `LocalizedResourcesReference` je stejný jako jejich národního prostředí, ale můžete použít libovolný řetězec jako ID.</span><span class="sxs-lookup"><span data-stu-id="14c8a-119">The `LocalizedResourcesReferenceId` for each `LocalizedResourcesReference` is the same as their locale, but you could use any string as the ID.</span></span>  <span data-ttu-id="14c8a-120">Pro každou kombinaci jazyka a stránky, budete muset vytvořit odpovídající `<LocalizedResources>` vidět v následujícím.</span><span class="sxs-lookup"><span data-stu-id="14c8a-120">For each language and page combination, you have to create a corresponding `<LocalizedResources>` shown in the following.</span></span>


###<a name="create-the-localizedresources"></a><span data-ttu-id="14c8a-121">Vytvořte LocalizedResources</span><span class="sxs-lookup"><span data-stu-id="14c8a-121">Create the LocalizedResources</span></span>

<span data-ttu-id="14c8a-122">Vaše přepsání jsou obsaženy v vaše `<BuildingBlocks>` a je `<LocalizedResources>` pro každou stránku a jazyk jste zadali v `<ContentDefinition>` pro jednotlivé stránky.</span><span class="sxs-lookup"><span data-stu-id="14c8a-122">Your overrides are contained in your `<BuildingBlocks>` and there is a `<LocalizedResources>` for each page and language you have specified in the `<ContentDefinition>` for each page.</span></span>  <span data-ttu-id="14c8a-123">Každý přepsání je zadán jako `<LocalizedString>` například následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="14c8a-123">Each override is specified as a `<LocalizedString>` such as in the following sample:</span></span>

```XML
<BuildingBlocks>
  <Localization>
    <LocalizedResources Id="en">
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimsProvider" StringId="SignInWithLogonNameExchange">Local Account Sign-in</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="DisplayName">Username</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="UserHelpText">Username used for signing in.</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="UserId" StringId="PatternHelpText">The username you provided is not valid.</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_signin">Sign In Now</LocalizedString>
        <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidPassword">Your password is incorrect.</LocalizedString>
      </LocalizedStrings>
    </LocalizedResources>
  </Localization>
</BuildingBlocks>
```

<span data-ttu-id="14c8a-124">Existují čtyři typy řetězec elementy na stránce:</span><span class="sxs-lookup"><span data-stu-id="14c8a-124">There are four types of string elements on the page:</span></span>

<span data-ttu-id="14c8a-125">**ClaimsProvider** -štítky pro vaši zprostředkovatelé identity (Facebook, Google, Azure AD atd.) **Typ ClaimType** -štítky pro vaši atributy a jejich odpovídající text nápovědy nebo pole chyby ověření **UxElement** – jiné řetězce elementy na stránce, které jsou ve výchozím nastavení, jako jsou tlačítka, odkazy nebo text **ErrorMessage** -formuláři chybových zpráv ověření</span><span class="sxs-lookup"><span data-stu-id="14c8a-125">**ClaimsProvider** - Labels for your identity providers (Facebook, Google, Azure AD etc.) **ClaimType** - Labels for your attributes and their corresponding help text, or field validation errors **UxElement** - Other string elements on the page that are there by default, such as buttons, links, or text **ErrorMessage** - Form validation error messages</span></span>

<span data-ttu-id="14c8a-126">Ujistěte se, že `StringId`s odpovídat pro stránku, že používáte tato přepsání, jinak je blokována ověření zásad na odeslání.</span><span class="sxs-lookup"><span data-stu-id="14c8a-126">Ensure that the `StringId`s match for the page that you are using these overrides otherwise it is blocked by policy validation on upload.</span></span>  