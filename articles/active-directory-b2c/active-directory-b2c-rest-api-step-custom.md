---
title: "Azure Active Directory B2C: REST API deklarací výměnu jako krok orchestration | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady, které se integrují s rozhraní API"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: 3e4f0bccf02c0332663a746d4ed8e5234c51f54e
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a><span data-ttu-id="7b174-103">Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako krok orchestration</span><span class="sxs-lookup"><span data-stu-id="7b174-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>

<span data-ttu-id="7b174-104">Framework prostředí Identity (IEF) podkladovou Azure Active Directory B2C (Azure AD B2C) umožňuje vývojáři identity integrovat interakci s rozhraní RESTful API cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="7b174-104">The Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables the identity developer to integrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="7b174-105">Na konci tohoto průvodce bude možné vytvořit cestu uživatele Azure AD B2C, která komunikuje s služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="7b174-105">At the end of this walkthrough, you will be able to create an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="7b174-106">IEF odesílá data v deklaracích identity a přijímá data zpět v deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="7b174-106">The IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="7b174-107">Rozhraní REST API deklarací exchange:</span><span class="sxs-lookup"><span data-stu-id="7b174-107">The REST API claims exchange:</span></span>

- <span data-ttu-id="7b174-108">Můžete třeba navrhnout jako krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="7b174-108">Can be designed as an orchestration step.</span></span>
- <span data-ttu-id="7b174-109">Můžete aktivovat externí akce.</span><span class="sxs-lookup"><span data-stu-id="7b174-109">Can trigger an external action.</span></span> <span data-ttu-id="7b174-110">Například se můžete přihlásit jako událost v externí databáze.</span><span class="sxs-lookup"><span data-stu-id="7b174-110">For instance, it can log an event in an external database.</span></span>
- <span data-ttu-id="7b174-111">Slouží k načtení hodnotu a uložte ho v uživatelské databázi.</span><span class="sxs-lookup"><span data-stu-id="7b174-111">Can be used to fetch a value and then store it in the user database.</span></span>

<span data-ttu-id="7b174-112">Přijaté deklarace identity můžete použít později změnit tok provádění.</span><span class="sxs-lookup"><span data-stu-id="7b174-112">You can use the received claims later to change the flow of execution.</span></span>

<span data-ttu-id="7b174-113">Můžete taky navrhnout interakce jako profil ověření.</span><span class="sxs-lookup"><span data-stu-id="7b174-113">You can also design the interaction as a validation profile.</span></span> <span data-ttu-id="7b174-114">Další informace najdete v tématu [návod: integrace rozhraní API REST deklarací výměn v vám dobře slouží Azure AD B2C uživatele jako ověření na vstup uživatele](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="7b174-114">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>

<span data-ttu-id="7b174-115">Tento scénář je, že když uživatel provede úpravy profilu, chceme:</span><span class="sxs-lookup"><span data-stu-id="7b174-115">The scenario is that when a user performs a profile edit, we want to:</span></span>

1. <span data-ttu-id="7b174-116">Vyhledání uživatele v externím systému.</span><span class="sxs-lookup"><span data-stu-id="7b174-116">Look up the user in an external system.</span></span>
2. <span data-ttu-id="7b174-117">Získáte Město, kde je tento uživatel zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="7b174-117">Get the city where that user is registered.</span></span>
3. <span data-ttu-id="7b174-118">Tento atribut vrátí do aplikace jako deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="7b174-118">Return that attribute to the application as a claim.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b174-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7b174-119">Prerequisites</span></span>

- <span data-ttu-id="7b174-120">Klient služby Azure AD B2C, nakonfigurované k dokončení registrace-množství nebo přihlášení, místní účet, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="7b174-120">An Azure AD B2C tenant configured to complete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="7b174-121">Koncový bod REST API pro interakci s.</span><span class="sxs-lookup"><span data-stu-id="7b174-121">A REST API endpoint to interact with.</span></span> <span data-ttu-id="7b174-122">Tento návod používá jako příklad webhook, jehož jednoduché Azure funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b174-122">This walkthrough uses a simple Azure function app webhook as an example.</span></span>
- <span data-ttu-id="7b174-123">*Doporučená*: dokončení [REST API deklarací exchange návod jako krok ověření](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="7b174-123">*Recommended*: Complete the [REST API claims exchange walkthrough as a validation step](active-directory-b2c-rest-api-validation-custom.md).</span></span>

## <a name="step-1-prepare-the-rest-api-function"></a><span data-ttu-id="7b174-124">Krok 1: Příprava funkce rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="7b174-124">Step 1: Prepare the REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="7b174-125">Instalace funkce rozhraní API REST je mimo rámec tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="7b174-125">Setup of REST API functions is outside the scope of this article.</span></span> <span data-ttu-id="7b174-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) poskytuje vynikající sada nástrojů pro vytváření služeb RESTful v cloudu.</span><span class="sxs-lookup"><span data-stu-id="7b174-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit to create RESTful services in the cloud.</span></span>

<span data-ttu-id="7b174-127">Nastavíme Azure funkce, která obdrží deklarace identity názvem `email`a potom se vrátí deklarace `city` s přiřazenou hodnotou `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="7b174-127">We have set up an Azure function that receives a claim called `email`, and then returns the claim `city` with the assigned value of `Redmond`.</span></span> <span data-ttu-id="7b174-128">Ukázka funkce Azure je na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="7b174-128">The sample Azure function is on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

<span data-ttu-id="7b174-129">`userMessage` Deklarace identity, který vrací funkci Azure je volitelná v tomto kontextu a IEF ho budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="7b174-129">The `userMessage` claim that the Azure function returns is optional in this context, and the IEF will ignore it.</span></span> <span data-ttu-id="7b174-130">Potenciálně ho můžete použít jako zprávu do aplikace a zobrazovat uživatelům později.</span><span class="sxs-lookup"><span data-stu-id="7b174-130">You can potentially use it as a message passed to the application and presented to the user later.</span></span>

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

<span data-ttu-id="7b174-131">Aplikace Azure funkce usnadňuje získat adresu URL funkce, která obsahuje identifikátor konkrétní funkce.</span><span class="sxs-lookup"><span data-stu-id="7b174-131">An Azure function app makes it easy to get the function URL, which includes the identifier of the specific function.</span></span> <span data-ttu-id="7b174-132">V takovém případě je adresa URL: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span><span class="sxs-lookup"><span data-stu-id="7b174-132">In this case, the URL is: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span></span> <span data-ttu-id="7b174-133">Můžete ho použít pro testování.</span><span class="sxs-lookup"><span data-stu-id="7b174-133">You can use it for testing.</span></span>

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a><span data-ttu-id="7b174-134">Krok 2: Konfigurace exchange deklarace identity rozhraní RESTful API jako technické profil v souboru TrustFrameworExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="7b174-134">Step 2: Configure the RESTful API claims exchange as a technical profile in your TrustFrameworExtensions.xml file</span></span>

<span data-ttu-id="7b174-135">Technické profil je úplná konfigurace exchange potřeby u služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="7b174-135">A technical profile is the full configuration of the exchange desired with the RESTful service.</span></span> <span data-ttu-id="7b174-136">Otevřete soubor TrustFrameworkExtensions.xml a přidejte následující fragment kódu XML uvnitř `<ClaimsProvider>` elementu.</span><span class="sxs-lookup"><span data-stu-id="7b174-136">Open the TrustFrameworkExtensions.xml file and add the following XML snippet inside the `<ClaimsProvider>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="7b174-137">V následující soubor XML, RESTful zprostředkovatele `Version=1.0.0.0` označen jako protokol.</span><span class="sxs-lookup"><span data-stu-id="7b174-137">In the following XML, RESTful provider `Version=1.0.0.0` is described as the protocol.</span></span> <span data-ttu-id="7b174-138">Považuje za jako funkce, která bude komunikovat s externí služby.</span><span class="sxs-lookup"><span data-stu-id="7b174-138">Consider it as the function that will interact with the external service.</span></span> <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
            </InputClaims>
            <OutputClaims>
                <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
            </OutputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

<span data-ttu-id="7b174-139">`<InputClaims>` Element definuje deklarace identity, které budou odesílané z IEF ke službě REST.</span><span class="sxs-lookup"><span data-stu-id="7b174-139">The `<InputClaims>` element defines the claims that will be sent from the IEF to the REST service.</span></span> <span data-ttu-id="7b174-140">V tomto příkladu obsah deklarace `givenName` zašle službě REST jako deklarace identity `email`.</span><span class="sxs-lookup"><span data-stu-id="7b174-140">In this example, the contents of the claim `givenName` will be sent to the REST service as the claim `email`.</span></span>  

<span data-ttu-id="7b174-141">`<OutputClaims>` Element definuje deklarace identity, které budou IEF očekávat od služby REST.</span><span class="sxs-lookup"><span data-stu-id="7b174-141">The `<OutputClaims>` element defines the claims that the IEF will expect from the REST service.</span></span> <span data-ttu-id="7b174-142">Bez ohledu na počet deklarace identity, které jsou přijaty IEF použije pouze ty, které jsou identifikovány sem.</span><span class="sxs-lookup"><span data-stu-id="7b174-142">Regardless of the number of claims that are received, the IEF will use only those identified here.</span></span> <span data-ttu-id="7b174-143">V tomto příkladu se deklarace identity jako přijata `city` budou mapována na IEF deklarace identity názvem `city`.</span><span class="sxs-lookup"><span data-stu-id="7b174-143">In this example, a claim received as `city` will be mapped to an IEF claim called `city`.</span></span>

## <a name="step-3-add-the-new-claim-city-to-the-schema-of-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="7b174-144">Krok 3: Přidejte novou deklaraci `city` schématu TrustFrameworkExtensions.xml souboru</span><span class="sxs-lookup"><span data-stu-id="7b174-144">Step 3: Add the new claim `city` to the schema of your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="7b174-145">Deklarace identity `city` není kdekoli definován zatím v našem schématu.</span><span class="sxs-lookup"><span data-stu-id="7b174-145">The claim `city` is not yet defined anywhere in our schema.</span></span> <span data-ttu-id="7b174-146">Ano, přidat definici uvnitř elementu `<BuildingBlocks>`.</span><span class="sxs-lookup"><span data-stu-id="7b174-146">So, add a definition inside the element `<BuildingBlocks>`.</span></span> <span data-ttu-id="7b174-147">Tento prvek na začátek souboru TrustFrameworkExtensions.xml můžete najít.</span><span class="sxs-lookup"><span data-stu-id="7b174-147">You can find this element at the beginning of the TrustFrameworkExtensions.xml file.</span></span>

```XML
<BuildingBlocks>
    <!--The claimtype city must be added to the TrustFrameworkPolicy-->
    <!-- You can add new claims in the BASE file Section III, or in the extensions file-->
    <ClaimsSchema>
        <ClaimType Id="city">
            <DisplayName>City</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your city</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-4-include-the-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a><span data-ttu-id="7b174-148">Krok 4: Zahrnují výměny deklarací identity služby REST, jak orchestration krok ve vašem profilu upravit uživatele cesty ve TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="7b174-148">Step 4: Include the REST service claims exchange as an orchestration step in your profile edit user journey in TrustFrameworkExtensions.xml</span></span>

<span data-ttu-id="7b174-149">Přidejte krok profil upravit uživatele cesty, po uživatel ověřen (postup orchestration 1 – 4 v následující soubor XML) a uživatel zadal aktualizovaný profil informací (krok 5).</span><span class="sxs-lookup"><span data-stu-id="7b174-149">Add a step to the profile edit user journey, after the user has been authenticated (orchestration steps 1-4 in the following XML) and the user has provided the updated profile information (step 5).</span></span>

> [!NOTE]
> <span data-ttu-id="7b174-150">Existuje mnoho případy použití, kde volání rozhraní REST API služby lze použít jako krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="7b174-150">There are many use cases where the REST API call can be used as an orchestration step.</span></span> <span data-ttu-id="7b174-151">Krok orchestration může sloužit jako aktualizace do externího systému po uživatele byla úspěšně dokončena úloha jako první registrace, nebo jako aktualizace profilu pro zachování informací o synchronizovány.</span><span class="sxs-lookup"><span data-stu-id="7b174-151">As an orchestration step, it can be used as an update to an external system after a user has successfully completed a task like first-time registration, or as a profile update to keep information synchronized.</span></span> <span data-ttu-id="7b174-152">V takovém případě se používá k posílení informací uvedených aplikaci po upravit profil.</span><span class="sxs-lookup"><span data-stu-id="7b174-152">In this case, it's used to augment the information provided to the application after the profile edit.</span></span>

<span data-ttu-id="7b174-153">Kopírování profil upravit uživatelský kód cesty XML ze souboru TrustFrameworkBase.xml do souboru TrustFrameworkExtensions.xml uvnitř `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="7b174-153">Copy the profile edit user journey XML code from the TrustFrameworkBase.xml file to your TrustFrameworkExtensions.xml file inside the `<UserJourneys>` element.</span></span> <span data-ttu-id="7b174-154">Proveďte změny v kroku 6.</span><span class="sxs-lookup"><span data-stu-id="7b174-154">Then make the modification under step 6.</span></span>

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> <span data-ttu-id="7b174-155">Pokud pořadí neodpovídá vaší verzí, ujistěte se, že vloží kód jako krok před `ClaimsExchange` typu `SendClaims`.</span><span class="sxs-lookup"><span data-stu-id="7b174-155">If the order does not match your version, make sure that you insert the code as the step before the `ClaimsExchange` type `SendClaims`.</span></span>

<span data-ttu-id="7b174-156">Poslední XML pro cestu uživatel by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="7b174-156">The final XML for the user journey should look like this:</span></span>

```XML
<UserJourney Id="ProfileEdit">
    <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
                <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
                <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
                <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>localAccountAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>socialIdpAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="5" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <!-- Add a step 6 to the user journey before the JWT token is created-->
        <OrchestrationStep Order="6" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5-add-the-claim-city-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a><span data-ttu-id="7b174-157">Krok 5: Přidejte do deklarací `city` k předávající straně soubor zásad, deklarace identity je odeslána do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="7b174-157">Step 5: Add the claim `city` to your relying party policy file so the claim is sent to your application</span></span>

<span data-ttu-id="7b174-158">Upravte svůj soubor ProfileEdit.xml předávající stranu a upravovat `<TechnicalProfile Id="PolicyProfile">` elementu, který chcete přidat následující: `<OutputClaim ClaimTypeReferenceId="city" />`.</span><span class="sxs-lookup"><span data-stu-id="7b174-158">Edit your ProfileEdit.xml relying party (RP) file and modify the `<TechnicalProfile Id="PolicyProfile">` element to add the following: `<OutputClaim ClaimTypeReferenceId="city" />`.</span></span>

<span data-ttu-id="7b174-159">Po přidání nových deklarací identity technické profil vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="7b174-159">After you add the new claim, the technical profile looks like this:</span></span>

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6-upload-your-changes-and-test"></a><span data-ttu-id="7b174-160">Krok 6: Odešlete své změny a testování</span><span class="sxs-lookup"><span data-stu-id="7b174-160">Step 6: Upload your changes and test</span></span>

<span data-ttu-id="7b174-161">Přepište existující verze zásad.</span><span class="sxs-lookup"><span data-stu-id="7b174-161">Overwrite the existing versions of the policy.</span></span>

1.  <span data-ttu-id="7b174-162">(Volitelné:) Uložte stávající verzi (stažením) souboru rozšíření, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="7b174-162">(Optional:) Save the existing version (by downloading) of your extensions file before you proceed.</span></span> <span data-ttu-id="7b174-163">Chcete-li zachovat počáteční složitost nízkou, doporučujeme nenahrávejte více verzí souboru rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7b174-163">To keep the initial complexity low, we recommend that you do not upload multiple versions of the extensions file.</span></span>
2.  <span data-ttu-id="7b174-164">(Volitelné:) Přejmenujte novou verzi ID zásady pro soubor zásad, který upravit tak, že změníte `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span><span class="sxs-lookup"><span data-stu-id="7b174-164">(Optional:) Rename the new version of the policy ID for the policy edit file by changing   `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span></span>
3.  <span data-ttu-id="7b174-165">Nahrajte soubor rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7b174-165">Upload the extensions file.</span></span>
4.  <span data-ttu-id="7b174-166">Nahrajte soubor zásad, který RP upravit.</span><span class="sxs-lookup"><span data-stu-id="7b174-166">Upload the policy edit RP file.</span></span>
5.  <span data-ttu-id="7b174-167">Použití **spustit nyní** k testování zásad.</span><span class="sxs-lookup"><span data-stu-id="7b174-167">Use **Run Now** to test the policy.</span></span> <span data-ttu-id="7b174-168">Zkontrolujte token, který IEF vrátí do aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b174-168">Review the token that the IEF returns to the application.</span></span>

<span data-ttu-id="7b174-169">Pokud všechno, co je správně nastavena, je token bude obsahovat nová deklarace identity `city`, s hodnotou `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="7b174-169">If everything is set up correctly, the token will include the new claim `city`, with the value `Redmond`.</span></span>

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a><span data-ttu-id="7b174-170">Další postup</span><span class="sxs-lookup"><span data-stu-id="7b174-170">Next steps</span></span>

[<span data-ttu-id="7b174-171">Pomocí rozhraní API REST jako krok ověření</span><span class="sxs-lookup"><span data-stu-id="7b174-171">Use a REST API as a validation step</span></span>](active-directory-b2c-rest-api-validation-custom.md)

[<span data-ttu-id="7b174-172">Upravit profil úpravy sbírat dodatečné informace od uživatelů</span><span class="sxs-lookup"><span data-stu-id="7b174-172">Modify the profile edit to gather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
