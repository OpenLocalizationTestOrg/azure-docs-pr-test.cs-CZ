---
title: "Azure Active Directory B2C: REST API deklarací výměnu jako krok orchestration | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady, které se integrují s rozhraní API"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: 90a495029f48d70232ef3f99de4ea4d351395aa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a><span data-ttu-id="9ca9d-103">Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako krok orchestration</span><span class="sxs-lookup"><span data-stu-id="9ca9d-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>

<span data-ttu-id="9ca9d-104">Hello Identity rozhraní Framework (IEF) podkladovou Azure Active Directory B2C (Azure AD B2C) umožňuje hello identity vývojáře toointegrate interakci s rozhraní RESTful API cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-104">hello Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables hello identity developer toointegrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="9ca9d-105">Na konci hello tohoto návodu bude možné toocreate cesty uživatele Azure AD B2C, která interaguje s služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-105">At hello end of this walkthrough, you will be able toocreate an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="9ca9d-106">Hello IEF odesílá data v deklaracích identity a přijímá data zpět v deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-106">hello IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="9ca9d-107">Hello exchange deklarace identity rozhraní REST API:</span><span class="sxs-lookup"><span data-stu-id="9ca9d-107">hello REST API claims exchange:</span></span>

- <span data-ttu-id="9ca9d-108">Můžete třeba navrhnout jako krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-108">Can be designed as an orchestration step.</span></span>
- <span data-ttu-id="9ca9d-109">Můžete aktivovat externí akce.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-109">Can trigger an external action.</span></span> <span data-ttu-id="9ca9d-110">Například se můžete přihlásit jako událost v externí databáze.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-110">For instance, it can log an event in an external database.</span></span>
- <span data-ttu-id="9ca9d-111">Můžou být použité toofetch hodnota a uloží jej v uživatelské databázi hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-111">Can be used toofetch a value and then store it in hello user database.</span></span>

<span data-ttu-id="9ca9d-112">Můžete použít deklarace identity hello přijatých novější toku toochange hello spouštění.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-112">You can use hello received claims later toochange hello flow of execution.</span></span>

<span data-ttu-id="9ca9d-113">Můžete taky navrhnout hello interakce jako profil ověření.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-113">You can also design hello interaction as a validation profile.</span></span> <span data-ttu-id="9ca9d-114">Další informace najdete v tématu [návod: integrace rozhraní API REST deklarací výměn v vám dobře slouží Azure AD B2C uživatele jako ověření na vstup uživatele](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="9ca9d-114">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>

<span data-ttu-id="9ca9d-115">scénář Hello je, že když uživatel provede úpravy profilu, chceme:</span><span class="sxs-lookup"><span data-stu-id="9ca9d-115">hello scenario is that when a user performs a profile edit, we want to:</span></span>

1. <span data-ttu-id="9ca9d-116">Vyhledání uživatele hello v externím systému.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-116">Look up hello user in an external system.</span></span>
2. <span data-ttu-id="9ca9d-117">Získáte hello Město, kde je tento uživatel zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-117">Get hello city where that user is registered.</span></span>
3. <span data-ttu-id="9ca9d-118">Vrátí tuto aplikaci toohello atribut jako deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-118">Return that attribute toohello application as a claim.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ca9d-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ca9d-119">Prerequisites</span></span>

- <span data-ttu-id="9ca9d-120">Toocomplete nakonfigurovat klienta Azure AD B2C místní účet registrace-množství nebo přihlášení, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="9ca9d-120">An Azure AD B2C tenant configured toocomplete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="9ca9d-121">Toointeract koncový bod REST API s.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-121">A REST API endpoint toointeract with.</span></span> <span data-ttu-id="9ca9d-122">Tento návod používá jako příklad webhook, jehož jednoduché Azure funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-122">This walkthrough uses a simple Azure function app webhook as an example.</span></span>
- <span data-ttu-id="9ca9d-123">*Doporučená*: dokončení hello [REST API deklarací exchange návod jako krok ověření](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="9ca9d-123">*Recommended*: Complete hello [REST API claims exchange walkthrough as a validation step](active-directory-b2c-rest-api-validation-custom.md).</span></span>

## <a name="step-1-prepare-hello-rest-api-function"></a><span data-ttu-id="9ca9d-124">Krok 1: Příprava funkce rozhraní API REST hello</span><span class="sxs-lookup"><span data-stu-id="9ca9d-124">Step 1: Prepare hello REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="9ca9d-125">Instalace funkce rozhraní API REST je mimo rámec tohoto článku hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-125">Setup of REST API functions is outside hello scope of this article.</span></span> <span data-ttu-id="9ca9d-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) poskytuje vynikající toolkit toocreate RESTful služby v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit toocreate RESTful services in hello cloud.</span></span>

<span data-ttu-id="9ca9d-127">Nastavíme Azure funkce, která obdrží deklarace identity názvem `email`, a potom vrátí hello deklarace identity `city` s hodnotou hello přiřazené `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-127">We have set up an Azure function that receives a claim called `email`, and then returns hello claim `city` with hello assigned value of `Redmond`.</span></span> <span data-ttu-id="9ca9d-128">Ukázka Hello Azure funkce je na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="9ca9d-128">hello sample Azure function is on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

<span data-ttu-id="9ca9d-129">Hello `userMessage` deklaraci identity, kterou vrátí funkce Azure hello je volitelná v tomto kontextu a hello IEF ho budou ignorovat.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-129">hello `userMessage` claim that hello Azure function returns is optional in this context, and hello IEF will ignore it.</span></span> <span data-ttu-id="9ca9d-130">Potenciálně můžete ho jako zprávu předán toohello aplikace a později uvedené toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-130">You can potentially use it as a message passed toohello application and presented toohello user later.</span></span>

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

<span data-ttu-id="9ca9d-131">Aplikace Azure funkce umožňuje snadno tooget hello funkce URL, která obsahuje identifikátor hello hello konkrétní funkce.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-131">An Azure function app makes it easy tooget hello function URL, which includes hello identifier of hello specific function.</span></span> <span data-ttu-id="9ca9d-132">V takovém případě je adresa URL hello: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-132">In this case, hello URL is: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span></span> <span data-ttu-id="9ca9d-133">Můžete ho použít pro testování.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-133">You can use it for testing.</span></span>

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a><span data-ttu-id="9ca9d-134">Krok 2: Konfigurace exchange deklarace identity rozhraní RESTful API hello jako technické profil v souboru TrustFrameworExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="9ca9d-134">Step 2: Configure hello RESTful API claims exchange as a technical profile in your TrustFrameworExtensions.xml file</span></span>

<span data-ttu-id="9ca9d-135">Technické profil je hello úplná konfigurace systému exchange hello požadovaného s hello služba RESTful.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-135">A technical profile is hello full configuration of hello exchange desired with hello RESTful service.</span></span> <span data-ttu-id="9ca9d-136">Otevřete soubor TrustFrameworkExtensions.xml hello a přidejte následující fragment kódu XML uvnitř hello hello `<ClaimsProvider>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-136">Open hello TrustFrameworkExtensions.xml file and add hello following XML snippet inside hello `<ClaimsProvider>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="9ca9d-137">V následující XML, RESTful zprostředkovatele hello `Version=1.0.0.0` označen jako protokol hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-137">In hello following XML, RESTful provider `Version=1.0.0.0` is described as hello protocol.</span></span> <span data-ttu-id="9ca9d-138">Považuje za jako hello funkce, která bude komunikovat s externí služba hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-138">Consider it as hello function that will interact with hello external service.</span></span> <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

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

<span data-ttu-id="9ca9d-139">Hello `<InputClaims>` element definuje hello deklarace identity, které budou odesílané z hello IEF toohello REST služby.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-139">hello `<InputClaims>` element defines hello claims that will be sent from hello IEF toohello REST service.</span></span> <span data-ttu-id="9ca9d-140">V tomto příkladu hello obsah deklarace identity hello `givenName` se budou odesílat služby REST toohello jako deklarace identity hello `email`.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-140">In this example, hello contents of hello claim `givenName` will be sent toohello REST service as hello claim `email`.</span></span>  

<span data-ttu-id="9ca9d-141">Hello `<OutputClaims>` element definuje hello deklarace identity tohoto hello IEF bude od služby REST hello očekávat.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-141">hello `<OutputClaims>` element defines hello claims that hello IEF will expect from hello REST service.</span></span> <span data-ttu-id="9ca9d-142">Bez ohledu na číslo hello deklarací identity, které jsou přijaty hello IEF použije pouze ty, které jsou identifikovány sem.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-142">Regardless of hello number of claims that are received, hello IEF will use only those identified here.</span></span> <span data-ttu-id="9ca9d-143">V tomto příkladu se deklarace identity jako přijata `city` bude volána deklarací identity mapovat tooan IEF `city`.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-143">In this example, a claim received as `city` will be mapped tooan IEF claim called `city`.</span></span>

## <a name="step-3-add-hello-new-claim-city-toohello-schema-of-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="9ca9d-144">Krok 3: Přidejte novou deklaraci hello `city` toohello schématu TrustFrameworkExtensions.xml souboru</span><span class="sxs-lookup"><span data-stu-id="9ca9d-144">Step 3: Add hello new claim `city` toohello schema of your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="9ca9d-145">deklarace identity Hello `city` není kdekoli definován zatím v našem schématu.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-145">hello claim `city` is not yet defined anywhere in our schema.</span></span> <span data-ttu-id="9ca9d-146">Ano, přidat definici uvnitř elementu hello `<BuildingBlocks>`.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-146">So, add a definition inside hello element `<BuildingBlocks>`.</span></span> <span data-ttu-id="9ca9d-147">Tento element na začátku hello hello TrustFrameworkExtensions.xml soubor můžete najít.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-147">You can find this element at hello beginning of hello TrustFrameworkExtensions.xml file.</span></span>

```XML
<BuildingBlocks>
    <!--hello claimtype city must be added toohello TrustFrameworkPolicy-->
    <!-- You can add new claims in hello BASE file Section III, or in hello extensions file-->
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

## <a name="step-4-include-hello-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a><span data-ttu-id="9ca9d-148">Krok 4: Zahrnují výměny deklarací identity služby REST hello jako krok orchestration ve vašem profilu upravit uživatele cesty ve TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="9ca9d-148">Step 4: Include hello REST service claims exchange as an orchestration step in your profile edit user journey in TrustFrameworkExtensions.xml</span></span>

<span data-ttu-id="9ca9d-149">Přidejte krok toohello profil upravit uživatele cesty po hello uživatel ověřen (postup orchestration 1 – 4 v následující XML hello) a hello uživatel zadal hello aktualizovat informace o profilu (krok 5).</span><span class="sxs-lookup"><span data-stu-id="9ca9d-149">Add a step toohello profile edit user journey, after hello user has been authenticated (orchestration steps 1-4 in hello following XML) and hello user has provided hello updated profile information (step 5).</span></span>

> [!NOTE]
> <span data-ttu-id="9ca9d-150">Existuje mnoho případy použití, kde hello volání rozhraní API REST slouží jako krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-150">There are many use cases where hello REST API call can be used as an orchestration step.</span></span> <span data-ttu-id="9ca9d-151">Krok orchestration ho lze použít jako externí systém tooan aktualizace po uživatele byla úspěšně dokončena úloha jako první registrace, nebo jako profil aktualizovat tookeep informace synchronizované.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-151">As an orchestration step, it can be used as an update tooan external system after a user has successfully completed a task like first-time registration, or as a profile update tookeep information synchronized.</span></span> <span data-ttu-id="9ca9d-152">V takovém případě je použité tooaugment hello informací toohello aplikace po upravit profil hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-152">In this case, it's used tooaugment hello information provided toohello application after hello profile edit.</span></span>

<span data-ttu-id="9ca9d-153">Kopírování hello profil upravit kód XML cesty uživatele ze souboru TrustFrameworkExtensions.xml hello TrustFrameworkBase.xml souboru tooyour uvnitř hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-153">Copy hello profile edit user journey XML code from hello TrustFrameworkBase.xml file tooyour TrustFrameworkExtensions.xml file inside hello `<UserJourneys>` element.</span></span> <span data-ttu-id="9ca9d-154">Proveďte úpravy hello v kroku 6.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-154">Then make hello modification under step 6.</span></span>

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> <span data-ttu-id="9ca9d-155">Pokud pořadí hello se neshoduje s vaší verzí, ujistěte se, vložit hello kód jako hello krok před hello `ClaimsExchange` typu `SendClaims`.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-155">If hello order does not match your version, make sure that you insert hello code as hello step before hello `ClaimsExchange` type `SendClaims`.</span></span>

<span data-ttu-id="9ca9d-156">Hello konečné XML pro cestu hello uživatel by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="9ca9d-156">hello final XML for hello user journey should look like this:</span></span>

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
        <!-- Add a step 6 toohello user journey before hello JWT token is created-->
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

## <a name="step-5-add-hello-claim-city-tooyour-relying-party-policy-file-so-hello-claim-is-sent-tooyour-application"></a><span data-ttu-id="9ca9d-157">Krok 5: Přidejte hello deklarace `city` tooyour předávající strany zásad souborů, deklarace identity hello se odesílají tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="9ca9d-157">Step 5: Add hello claim `city` tooyour relying party policy file so hello claim is sent tooyour application</span></span>

<span data-ttu-id="9ca9d-158">Váš soubor ProfileEdit.xml předávající stranu a upravte hello `<TechnicalProfile Id="PolicyProfile">` element tooadd hello následující: `<OutputClaim ClaimTypeReferenceId="city" />`.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-158">Edit your ProfileEdit.xml relying party (RP) file and modify hello `<TechnicalProfile Id="PolicyProfile">` element tooadd hello following: `<OutputClaim ClaimTypeReferenceId="city" />`.</span></span>

<span data-ttu-id="9ca9d-159">Po přidání novou deklaraci hello technické profil hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9ca9d-159">After you add hello new claim, hello technical profile looks like this:</span></span>

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

## <a name="step-6-upload-your-changes-and-test"></a><span data-ttu-id="9ca9d-160">Krok 6: Odešlete své změny a testování</span><span class="sxs-lookup"><span data-stu-id="9ca9d-160">Step 6: Upload your changes and test</span></span>

<span data-ttu-id="9ca9d-161">Přepište existující verze hello hello zásad.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-161">Overwrite hello existing versions of hello policy.</span></span>

1.  <span data-ttu-id="9ca9d-162">(Volitelné:) Uložte hello stávající verzi (podle stahování) souboru rozšíření předtím, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-162">(Optional:) Save hello existing version (by downloading) of your extensions file before you proceed.</span></span> <span data-ttu-id="9ca9d-163">Doporučujeme tookeep hello počáteční složitost nízkou nenahrávejte více verzí souboru rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-163">tookeep hello initial complexity low, we recommend that you do not upload multiple versions of hello extensions file.</span></span>
2.  <span data-ttu-id="9ca9d-164">(Volitelné:) Přejmenujte hello novou verzi hello ID zásad pro soubor úpravy zásad hello změnou `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-164">(Optional:) Rename hello new version of hello policy ID for hello policy edit file by changing   `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span></span>
3.  <span data-ttu-id="9ca9d-165">Nahrajte soubor rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-165">Upload hello extensions file.</span></span>
4.  <span data-ttu-id="9ca9d-166">Nahrajte soubor RP úpravy zásad hello.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-166">Upload hello policy edit RP file.</span></span>
5.  <span data-ttu-id="9ca9d-167">Použití **spustit nyní** tootest hello zásad.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-167">Use **Run Now** tootest hello policy.</span></span> <span data-ttu-id="9ca9d-168">Zkontrolujte hello token, který hello IEF vrátí toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-168">Review hello token that hello IEF returns toohello application.</span></span>

<span data-ttu-id="9ca9d-169">Pokud všechno, co je správně nastavena, hello token bude obsahovat nová deklarace identity hello `city`, s hodnotou hello `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="9ca9d-169">If everything is set up correctly, hello token will include hello new claim `city`, with hello value `Redmond`.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="9ca9d-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ca9d-170">Next steps</span></span>

[<span data-ttu-id="9ca9d-171">Pomocí rozhraní API REST jako krok ověření</span><span class="sxs-lookup"><span data-stu-id="9ca9d-171">Use a REST API as a validation step</span></span>](active-directory-b2c-rest-api-validation-custom.md)

[<span data-ttu-id="9ca9d-172">Upravte hello profil upravit toogather Další informace od uživatelů</span><span class="sxs-lookup"><span data-stu-id="9ca9d-172">Modify hello profile edit toogather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
