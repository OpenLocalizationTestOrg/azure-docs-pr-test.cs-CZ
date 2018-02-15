---
title: "Složitost hesla v vlastní zásady – Azure AD B2C | Microsoft Docs"
description: "Jak konfigurovat požadavky na složitost hesel v vlastní zásady"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: eb187a120399089f1c3c145a06fbe993f50fb92b
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="configure-password-complexity-in-custom-policies"></a><span data-ttu-id="11853-103">Nakonfigurujte složitost hesel v vlastní zásady</span><span class="sxs-lookup"><span data-stu-id="11853-103">Configure password complexity in custom policies</span></span>

> [!NOTE]
> <span data-ttu-id="11853-104">Tato funkce je ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="11853-104">**This feature is in preview.**</span></span>  <span data-ttu-id="11853-105">Obraťte se na [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com) tak, aby měl váš klient testovací povolit tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="11853-105">Contact [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com) to have your test tenant enabled with this feature.</span></span>  <span data-ttu-id="11853-106">Neprovádějte testování to na produkční klienty.</span><span class="sxs-lookup"><span data-stu-id="11853-106">Do not test this on production tenants.</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="11853-107">Tento článek k pokročilé popis toho, jak funguje složitost hesla a je povolit pomocí vlastních zásad Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="11853-107">This article is an advanced description of how password complexity works and is enabled using Azure AD B2C custom policies.</span></span>

## <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a><span data-ttu-id="11853-108">Azure AD B2C: Konfigurujte požadavky na složitost hesel</span><span class="sxs-lookup"><span data-stu-id="11853-108">Azure AD B2C: Configure complexity requirements for passwords</span></span>

<span data-ttu-id="11853-109">Azure Active Directory B2C (Azure AD B2C) podporuje změna požadavky na složitost hesla poskytl koncového uživatele při vytváření účtu.</span><span class="sxs-lookup"><span data-stu-id="11853-109">Azure Active Directory B2C (Azure AD B2C) supports changing the complexity requirements for passwords supplied by an end user when creating an account.</span></span>  <span data-ttu-id="11853-110">Ve výchozím nastavení používá Azure AD B2C **silným** hesla.</span><span class="sxs-lookup"><span data-stu-id="11853-110">By default, Azure AD B2C uses **Strong** passwords.</span></span>  <span data-ttu-id="11853-111">Azure AD B2C podporuje také konfigurační možnosti pro řízení složitosti hesla, které zákazníci mohou používat.</span><span class="sxs-lookup"><span data-stu-id="11853-111">Azure AD B2C also supports configuration options to control the complexity of passwords that customers can use.</span></span>  <span data-ttu-id="11853-112">V tomto článku bude zmíněn konfiguraci složitosti hesla v vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="11853-112">This article talks about how to do configure password complexity in custom policies.</span></span>  <span data-ttu-id="11853-113">Je také možné použít [nakonfigurujte složitost hesel v integrovaných zásad](active-directory-b2c-reference-password-complexity.md).</span><span class="sxs-lookup"><span data-stu-id="11853-113">It is also possible to use [configure password complexity in built-in policies](active-directory-b2c-reference-password-complexity.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11853-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="11853-114">Prerequisites</span></span>

<span data-ttu-id="11853-115">Klient služby Azure AD B2C, nakonfigurované k dokončení registrace-množství nebo přihlášení, místní účet, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="11853-115">An Azure AD B2C tenant configured to complete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="how-to-configure-password-complexity-in-custom-policy"></a><span data-ttu-id="11853-116">Postup konfigurace složitost hesla ve vlastních zásadách</span><span class="sxs-lookup"><span data-stu-id="11853-116">How to configure password complexity in custom policy</span></span>

<span data-ttu-id="11853-117">Nakonfigurujte složitost hesla ve vlastních zásadách, musí obsahovat celková struktura vlastní zásady `ClaimsSchema`, `Predicates`, a `InputValidations` prvku v `BuildingBlocks`.</span><span class="sxs-lookup"><span data-stu-id="11853-117">To configure password complexity in custom policy, the overall structure of your custom policy must include a `ClaimsSchema`, `Predicates`, and `InputValidations` element inside `BuildingBlocks`.</span></span>

```XML
  <BuildingBlocks>
    <ClaimsSchema>...</ClaimsSchema>
    <Predicates>...</Predicates>
    <InputValidations>...</InputValidations>
  </BuildingBlocks>
```

<span data-ttu-id="11853-118">Účelem těchto prvků vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="11853-118">The purpose of these elements is as follows:</span></span>

- <span data-ttu-id="11853-119">Každý `Predicate` element definuje kontrolu ověření základní řetězec, který vrací hodnotu true nebo false.</span><span class="sxs-lookup"><span data-stu-id="11853-119">Each `Predicate` element defines a basic string validation check that returns true or false.</span></span>
- <span data-ttu-id="11853-120">`InputValidations` Element má jeden nebo více `InputValidation` elementy.</span><span class="sxs-lookup"><span data-stu-id="11853-120">The `InputValidations` element has one or more `InputValidation` elements.</span></span>  <span data-ttu-id="11853-121">Každý `InputValidation` je vytvořený pomocí řady `Predicate` elementy.</span><span class="sxs-lookup"><span data-stu-id="11853-121">Each `InputValidation` is constructed by using a series of `Predicate` elements.</span></span> <span data-ttu-id="11853-122">Tento element umožňuje provádět boolean agregace (podobně jako `and` a `or`).</span><span class="sxs-lookup"><span data-stu-id="11853-122">This element allows you to perform boolean aggregations (similar to `and` and `or`).</span></span>
- <span data-ttu-id="11853-123">`ClaimsSchema` Definuje, které deklarace identity je ověřován.</span><span class="sxs-lookup"><span data-stu-id="11853-123">The `ClaimsSchema` defines which claim is being validated.</span></span>  <span data-ttu-id="11853-124">Potom definuje, které `InputValidation` pravidlo se používá k ověření, že deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="11853-124">It then defines which `InputValidation` rule is used to validate that claim.</span></span>

### <a name="defining-a-predicate-element"></a><span data-ttu-id="11853-125">Definování predikátu element</span><span class="sxs-lookup"><span data-stu-id="11853-125">Defining a predicate element</span></span>

<span data-ttu-id="11853-126">Predikáty má dva typy metoda: IsLengthRange nebo MatchesRegex.</span><span class="sxs-lookup"><span data-stu-id="11853-126">Predicates have two method types: IsLengthRange or MatchesRegex.</span></span> <span data-ttu-id="11853-127">Pojďme si každý příkladem.</span><span class="sxs-lookup"><span data-stu-id="11853-127">Let's review an example of each.</span></span>  <span data-ttu-id="11853-128">Nejprve máme příkladem MatchesRegex, který se používá k odpovídají regulárnímu výrazu.</span><span class="sxs-lookup"><span data-stu-id="11853-128">First we have an example of MatchesRegex, which is used to match a regular expression.</span></span>  <span data-ttu-id="11853-129">V tomto příkladu odpovídá řetězec, který obsahuje čísla.</span><span class="sxs-lookup"><span data-stu-id="11853-129">In this example, it matches string that contains numbers.</span></span>

```XML
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
```

<span data-ttu-id="11853-130">Další pojďme si příklad IsLengthRange.</span><span class="sxs-lookup"><span data-stu-id="11853-130">Next let's review an example of IsLengthRange.</span></span>  <span data-ttu-id="11853-131">Tato metoda přebírá řetězec minimální a maximální délku.</span><span class="sxs-lookup"><span data-stu-id="11853-131">This method takes a minimum and maximum string length.</span></span>

```XML
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
```

<span data-ttu-id="11853-132">Použití `HelpText` atribut poskytněte chybovou zprávu pro koncové uživatele, pokud kontrola selže.</span><span class="sxs-lookup"><span data-stu-id="11853-132">Use the `HelpText` attribute to provide an error message for end users if the check fails.</span></span>  <span data-ttu-id="11853-133">Tento řetězec je možné lokalizovat pomocí [jazyk přizpůsobení funkce](active-directory-b2c-reference-language-customization.md).</span><span class="sxs-lookup"><span data-stu-id="11853-133">This string can be localized using the [language customization feature](active-directory-b2c-reference-language-customization.md).</span></span>

### <a name="defining-an-inputvalidation-element"></a><span data-ttu-id="11853-134">Definování InputValidation element</span><span class="sxs-lookup"><span data-stu-id="11853-134">Defining an InputValidation element</span></span>

<span data-ttu-id="11853-135">`InputValidation` Je agregaci `PredicateReferences`.</span><span class="sxs-lookup"><span data-stu-id="11853-135">An `InputValidation` is an aggregation of `PredicateReferences`.</span></span> <span data-ttu-id="11853-136">Každý `PredicateReferences` musí být splněné, aby `InputValidation` proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="11853-136">Each `PredicateReferences` must be true in order for the `InputValidation` to succeed.</span></span>  <span data-ttu-id="11853-137">Ale uvnitř `PredicateReferences` element použít atribut `MatchAtLeast` k určení kolik `PredicateReference` kontroly true musí vracet.</span><span class="sxs-lookup"><span data-stu-id="11853-137">However, inside the `PredicateReferences` element use an attribute called `MatchAtLeast` to specify how many `PredicateReference` checks must return true.</span></span>  <span data-ttu-id="11853-138">Volitelně můžete definovat `HelpText` definován atribut přepsat chybovou zprávu v `Predicate` elementy, které odkazuje.</span><span class="sxs-lookup"><span data-stu-id="11853-138">Optionally, define a `HelpText` attribute to override the error message defined in the `Predicate` elements that it references.</span></span>

```XML
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
```

### <a name="defining-a-claimsschema-element"></a><span data-ttu-id="11853-139">Definování ClaimsSchema element</span><span class="sxs-lookup"><span data-stu-id="11853-139">Defining a ClaimsSchema element</span></span>

<span data-ttu-id="11853-140">Typy deklarací identity `newPassword` a `reenterPassword` jsou považovány za zvláštní, takže názvy beze změny.</span><span class="sxs-lookup"><span data-stu-id="11853-140">The claim types `newPassword` and `reenterPassword` are considered special, so do not change the names.</span></span>  <span data-ttu-id="11853-141">Uživatelské rozhraní ověří uživatele správně znovu zadat hesla během vytváření účtu založené na těchto `ClaimType` elementy.</span><span class="sxs-lookup"><span data-stu-id="11853-141">The UI validates the user correctly reentered their password during account creation based on these `ClaimType` elements.</span></span>  <span data-ttu-id="11853-142">K vyhledání stejné `ClaimType` elementy, vyhledejte v TrustFrameworkBase.xml v svůj balíček starter.</span><span class="sxs-lookup"><span data-stu-id="11853-142">To find the same `ClaimType` elements, look in the TrustFrameworkBase.xml in your starter pack.</span></span>  <span data-ttu-id="11853-143">Co je nového v tomto příkladu je potlačuje tyto prvky můžete definovat `InputValidationReference`.</span><span class="sxs-lookup"><span data-stu-id="11853-143">What's new in this example is that we are overriding these elements to define a `InputValidationReference`.</span></span> <span data-ttu-id="11853-144">`ID` Atribut tohoto nového elementu odkazuje `InputValidation` element, který jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="11853-144">The `ID` attribute of this new element is pointing to the `InputValidation` element that we defined.</span></span>

```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
```

### <a name="putting-it-all-together"></a><span data-ttu-id="11853-145">Třeba umisťovat všechny společně</span><span class="sxs-lookup"><span data-stu-id="11853-145">Putting it all together</span></span>

<span data-ttu-id="11853-146">Tento příklad ukazuje, jak všechny části zapadají k vytvoření pracovních zásad.</span><span class="sxs-lookup"><span data-stu-id="11853-146">This example shows how all the pieces fit together to form a working policy.</span></span>  <span data-ttu-id="11853-147">Použití tohoto příkladu:</span><span class="sxs-lookup"><span data-stu-id="11853-147">To use this example:</span></span>

1. <span data-ttu-id="11853-148">Postupujte podle pokynů v nezbytné [Začínáme](active-directory-b2c-get-started-custom.md) Pokud chcete stáhnout, konfigurace a nahrajte TrustFrameworkBase.xml a TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="11853-148">Follow the instructions in the pre-requisite [Getting started](active-directory-b2c-get-started-custom.md) to download, configure, and upload TrustFrameworkBase.xml and TrustFrameworkExtensions.xml</span></span>
1. <span data-ttu-id="11853-149">Vytvořte soubor SignUporSignIn.xml pomocí příklad obsah v této části.</span><span class="sxs-lookup"><span data-stu-id="11853-149">Create a SignUporSignIn.xml file using the example content in this section.</span></span>
1. <span data-ttu-id="11853-150">Aktualizovat nahrazení SignUporSignIn.xml `yourtenant` s název vašeho klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="11853-150">Update SignUporSignIn.xml replacing `yourtenant` with your Azure AD B2C tenant name.</span></span>
1. <span data-ttu-id="11853-151">Nahrajte soubor zásad SignUporSignIn.xml poslední.</span><span class="sxs-lookup"><span data-stu-id="11853-151">Upload the SignUporSignIn.xml policy file last.</span></span>

<span data-ttu-id="11853-152">Tento příklad obsahuje ověření pro hesla PIN kód a jeden pro silná hesla:</span><span class="sxs-lookup"><span data-stu-id="11853-152">This example contains a validation for pin passwords and one for strong passwords:</span></span>

- <span data-ttu-id="11853-153">Vyhledejte `PINpassword`.</span><span class="sxs-lookup"><span data-stu-id="11853-153">Look for `PINpassword`.</span></span> <span data-ttu-id="11853-154">To `InputValidation` element ověří PIN kód o libovolné délce.</span><span class="sxs-lookup"><span data-stu-id="11853-154">This `InputValidation` element validates a pin of any length.</span></span>  <span data-ttu-id="11853-155">Není použit v okamžiku, protože není odkazovaný ve `InputValidationReference` prvku v `ClaimType`.</span><span class="sxs-lookup"><span data-stu-id="11853-155">It is not used at the moment, because it is not referenced in the `InputValidationReference` element inside `ClaimType`.</span></span> 
- <span data-ttu-id="11853-156">Vyhledejte `PasswordValidation`.</span><span class="sxs-lookup"><span data-stu-id="11853-156">Look for `PasswordValidation`.</span></span> <span data-ttu-id="11853-157">To `InputValidation` element ověří hesla je 8 až 16 znaků a obsahuje hodnotu 3 4 velkých a malých písmen, čísel nebo symboly.</span><span class="sxs-lookup"><span data-stu-id="11853-157">This `InputValidation` element validates a password is 8 to 16 characters and contains 3 of 4 of numbers, uppercase, lowercase, or symbols.</span></span>  <span data-ttu-id="11853-158">Se odkazuje v `ClaimType`.</span><span class="sxs-lookup"><span data-stu-id="11853-158">It is referenced in `ClaimType`.</span></span>  <span data-ttu-id="11853-159">Toto pravidlo je proto vynucován v této zásadě.</span><span class="sxs-lookup"><span data-stu-id="11853-159">Therefore, this rule is being enforced in this policy.</span></span>

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="yourtenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin">
 <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    <Predicates>
      <Predicate Id="Lowercase" Method="MatchesRegex" HelpText="a lowercase">
        <Parameters>
          <Parameter Id="RegularExpression">^[a-z]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="MatchesRegex" HelpText="an uppercase">
        <Parameters>
          <Parameter Id="RegularExpression">^[A-Z]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="MatchesRegex" HelpText="a number">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="MatchesRegex" HelpText="a symbol">
        <Parameters>
          <Parameter Id="RegularExpression">^[!@#$%^*()]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    <InputValidations>
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
      <InputValidation Id="PINpassword">
        <PredicateReferences Id="PINGroup">
          <PredicateReference Id="PIN" />
        </PredicateReferences>
      </InputValidation>
    </InputValidations>
  </BuildingBlocks>
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
</TrustFrameworkPolicy>
```
