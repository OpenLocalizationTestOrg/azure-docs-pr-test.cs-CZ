---
title: "Azure Active Directory B2C: REST API deklarací výměnu jako ověření | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady"
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
ms.openlocfilehash: cec6c6e110514a8bbe0e0780f36738ff21ae2f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a><span data-ttu-id="4c86b-103">Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako ověření na vstup uživatele</span><span class="sxs-lookup"><span data-stu-id="4c86b-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input</span></span>

<span data-ttu-id="4c86b-104">Hello Identity rozhraní Framework (IEF) podkladovou Azure Active Directory B2C (Azure AD B2C) umožňuje hello identity vývojáře toointegrate interakci s rozhraní RESTful API cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="4c86b-104">hello Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables hello identity developer toointegrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="4c86b-105">Na konci hello tohoto návodu bude možné toocreate cesty uživatele Azure AD B2C, která interaguje s služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="4c86b-105">At hello end of this walkthrough, you will be able toocreate an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="4c86b-106">Hello IEF odesílá data v deklaracích identity a přijímá data zpět v deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="4c86b-106">hello IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="4c86b-107">Hello interakci s hello rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="4c86b-107">hello interaction with hello API:</span></span>

- <span data-ttu-id="4c86b-108">Můžete třeba navrhnout jako deklarace identity systému exchange REST API nebo jako profil ověření, který se stane uvnitř na krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="4c86b-108">Can be designed as a REST API claims exchange or as a validation profile, which happens inside an orchestration step.</span></span>
- <span data-ttu-id="4c86b-109">Obvykle ověří vstup od uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4c86b-109">Typically validates input from hello user.</span></span> <span data-ttu-id="4c86b-110">Pokud je hodnota hello od uživatele hello zamítnutí, hello uživatele můžete to zkusit znovu tooenter platnou hodnotu s hello možnost tooreturn chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="4c86b-110">If hello value from hello user is rejected, hello user can try again tooenter a valid value with hello opportunity tooreturn an error message.</span></span>

<span data-ttu-id="4c86b-111">Můžete taky navrhnout hello interakce jako krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="4c86b-111">You can also design hello interaction as an orchestration step.</span></span> <span data-ttu-id="4c86b-112">Další informace najdete v tématu [návod: integrace rozhraní API REST deklarací výměn v vám dobře slouží uživatele Azure AD B2C jako krok orchestration](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="4c86b-112">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step](active-directory-b2c-rest-api-step-custom.md).</span></span>

<span data-ttu-id="4c86b-113">Například profil hello ověření použijeme hello profil upravit uživatele cesty v souboru pack starter hello ProfileEdit.xml.</span><span class="sxs-lookup"><span data-stu-id="4c86b-113">For hello validation profile example, we will use hello profile edit user journey in hello starter pack file ProfileEdit.xml.</span></span>

<span data-ttu-id="4c86b-114">Zadaný uživatelem hello v profilu hello upravit není součástí seznamu vyloučení, abychom mohli ověřit tento název hello.</span><span class="sxs-lookup"><span data-stu-id="4c86b-114">We can verify that hello name provided by hello user in hello profile edit is not part of an exclusion list.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c86b-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4c86b-115">Prerequisites</span></span>

- <span data-ttu-id="4c86b-116">Toocomplete nakonfigurovat klienta Azure AD B2C místní účet registrace-množství nebo přihlášení, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="4c86b-116">An Azure AD B2C tenant configured toocomplete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="4c86b-117">Toointeract koncový bod REST API s.</span><span class="sxs-lookup"><span data-stu-id="4c86b-117">A REST API endpoint toointeract with.</span></span> <span data-ttu-id="4c86b-118">V tomto návodu, jsme zřídili ukázkový web s názvem [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) službou REST API.</span><span class="sxs-lookup"><span data-stu-id="4c86b-118">For this walkthrough, we've set up a demo site called [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) with a REST API service.</span></span>

## <a name="step-1-prepare-hello-rest-api-function"></a><span data-ttu-id="4c86b-119">Krok 1: Příprava funkce rozhraní API REST hello</span><span class="sxs-lookup"><span data-stu-id="4c86b-119">Step 1: Prepare hello REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="4c86b-120">Instalace funkce rozhraní API REST je mimo rámec tohoto článku hello.</span><span class="sxs-lookup"><span data-stu-id="4c86b-120">Setup of REST API functions is outside hello scope of this article.</span></span> <span data-ttu-id="4c86b-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) poskytuje vynikající toolkit toocreate RESTful služby v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="4c86b-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit toocreate RESTful services in hello cloud.</span></span>

<span data-ttu-id="4c86b-122">Vytvořili jsme Azure funkce, která obdrží deklarace identity, která se očekává, že jako `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="4c86b-122">We have created an Azure function that receives a claim that it expects as `playerTag`.</span></span> <span data-ttu-id="4c86b-123">Funkce Hello ověřuje, zda existuje tuto deklaraci.</span><span class="sxs-lookup"><span data-stu-id="4c86b-123">hello function validates whether this claim exists.</span></span> <span data-ttu-id="4c86b-124">Dostanete kód dokončení Azure funkce hello v [Githubu](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="4c86b-124">You can access hello complete Azure function code in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"hello player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

<span data-ttu-id="4c86b-125">Hello IEF očekává hello `userMessage` deklarací identity vrátí tuto hello Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="4c86b-125">hello IEF expects hello `userMessage` claim that hello Azure function returns.</span></span> <span data-ttu-id="4c86b-126">Toto tvrzení zobrazí jako uživatel toohello řetězec Pokud hello ověření selže, například když 409 – konflikt stavu je vrácený v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="4c86b-126">This claim will be presented as a string toohello user if hello validation fails, such as when a 409 conflict status is returned in hello preceding example.</span></span>

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="4c86b-127">Krok 2: Konfigurace exchange deklarace identity rozhraní RESTful API hello jako technické profil v souboru TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="4c86b-127">Step 2: Configure hello RESTful API claims exchange as a technical profile in your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="4c86b-128">Technické profil je hello úplná konfigurace systému exchange hello požadovaného s hello služba RESTful.</span><span class="sxs-lookup"><span data-stu-id="4c86b-128">A technical profile is hello full configuration of hello exchange desired with hello RESTful service.</span></span> <span data-ttu-id="4c86b-129">Otevřete soubor TrustFrameworkExtensions.xml hello a přidejte následující fragment kódu XML uvnitř hello hello `<ClaimsProviders>` elementu.</span><span class="sxs-lookup"><span data-stu-id="4c86b-129">Open hello TrustFrameworkExtensions.xml file and add hello following XML snippet inside hello `<ClaimsProviders>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="4c86b-130">V následující XML, RESTful zprostředkovatele hello `Version=1.0.0.0` označen jako protokol hello.</span><span class="sxs-lookup"><span data-stu-id="4c86b-130">In hello following XML, RESTful provider `Version=1.0.0.0` is described as hello protocol.</span></span> <span data-ttu-id="4c86b-131">Považuje za jako hello funkce, která bude komunikovat s externí služba hello.</span><span class="sxs-lookup"><span data-stu-id="4c86b-131">Consider it as hello function that will interact with hello external service.</span></span> <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

<span data-ttu-id="4c86b-132">Hello `InputClaims` element definuje hello deklarace identity, které budou odesílané z hello IEF toohello REST služby.</span><span class="sxs-lookup"><span data-stu-id="4c86b-132">hello `InputClaims` element defines hello claims that will be sent from hello IEF toohello REST service.</span></span> <span data-ttu-id="4c86b-133">V tomto příkladu hello obsah deklarace identity hello `givenName` odešle služba REST toohello jako `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="4c86b-133">In this example, hello contents of hello claim `givenName` will be sent toohello REST service as `playerTag`.</span></span> <span data-ttu-id="4c86b-134">V tomto příkladu hello IEF neočekává deklarací zpět.</span><span class="sxs-lookup"><span data-stu-id="4c86b-134">In this example, hello IEF does not expect claims back.</span></span> <span data-ttu-id="4c86b-135">Místo toho čeká na odpověď od služby REST hello a jednání podle hello stavové kódy, které obdrží.</span><span class="sxs-lookup"><span data-stu-id="4c86b-135">Instead, it waits for a response from hello REST service and acts based on hello status codes that it receives.</span></span>

## <a name="step-3-include-hello-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-toovalidate-hello-user-input"></a><span data-ttu-id="4c86b-136">Krok 3: Zahrnout exchange deklarace identity služba RESTful hello samoobslužné uplatňovaná technické profil místo toovalidate hello uživatelský vstup</span><span class="sxs-lookup"><span data-stu-id="4c86b-136">Step 3: Include hello RESTful service claims exchange in self-asserted technical profile where you want toovalidate hello user input</span></span>

<span data-ttu-id="4c86b-137">v hello interakce s uživatelem se nejčastěji používá Hello hello ověření kroku.</span><span class="sxs-lookup"><span data-stu-id="4c86b-137">hello most common use of hello validation step is in hello interaction with a user.</span></span> <span data-ttu-id="4c86b-138">Všechny interakce, kde uživatel hello je očekávané tooprovide vstupní jsou *samoobslužné prohlašovanou technické profily*.</span><span class="sxs-lookup"><span data-stu-id="4c86b-138">All interactions where hello user is expected tooprovide input are *self-asserted technical profiles*.</span></span> <span data-ttu-id="4c86b-139">V tomto příkladu přidáme hello ověření toohello samoobslužných Asserted ProfileUpdate technické profilu.</span><span class="sxs-lookup"><span data-stu-id="4c86b-139">For this example, we will add hello validation toohello Self-Asserted-ProfileUpdate technical profile.</span></span> <span data-ttu-id="4c86b-140">Toto je hello technické profil, který hello soubor zásad předávající stranu `Profile Edit` používá.</span><span class="sxs-lookup"><span data-stu-id="4c86b-140">This is hello technical profile that hello relying party (RP) policy file `Profile Edit` uses.</span></span>

<span data-ttu-id="4c86b-141">toohello exchange deklarace identity hello tooadd samoobslužné prohlašovanou technické profil:</span><span class="sxs-lookup"><span data-stu-id="4c86b-141">tooadd hello claims exchange toohello self-asserted technical profile:</span></span>

1. <span data-ttu-id="4c86b-142">Otevřete soubor TrustFrameworkBase.xml hello a vyhledejte `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span><span class="sxs-lookup"><span data-stu-id="4c86b-142">Open hello TrustFrameworkBase.xml file and search for `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span></span>
2. <span data-ttu-id="4c86b-143">Zkontrolujte konfiguraci hello tohoto technické profilu.</span><span class="sxs-lookup"><span data-stu-id="4c86b-143">Review hello configuration of this technical profile.</span></span> <span data-ttu-id="4c86b-144">Sledujte, jak je hello exchange s hello uživatelem definovaný jako deklarace identity, které se dotaz uživatele hello (vstupních deklarací identity) a deklarací identity, které se budou zpět od zprostředkovatele samoobslužné uplatňovaná hello (výstup deklarace identity).</span><span class="sxs-lookup"><span data-stu-id="4c86b-144">Observe how hello exchange with hello user is defined as claims that will be asked of hello user (input claims) and claims that will be expected back from hello self-asserted provider (output claims).</span></span>
3. <span data-ttu-id="4c86b-145">Vyhledejte `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`a Všimněte si, že tento profil je vyvolána jako orchestration krok 6 `<UserJourney Id="ProfileEdit">`.</span><span class="sxs-lookup"><span data-stu-id="4c86b-145">Search for `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, and notice that this profile is invoked as orchestration step 6 of `<UserJourney Id="ProfileEdit">`.</span></span>

## <a name="step-4-upload-and-test-hello-profile-edit-rp-policy-file"></a><span data-ttu-id="4c86b-146">Krok 4: Nahrání a testovací soubor zásad RP upravit profil hello</span><span class="sxs-lookup"><span data-stu-id="4c86b-146">Step 4: Upload and test hello profile edit RP policy file</span></span>

1. <span data-ttu-id="4c86b-147">Nahrajte hello nová verze souboru TrustFrameworkExtensions.xml hello.</span><span class="sxs-lookup"><span data-stu-id="4c86b-147">Upload hello new version of hello TrustFrameworkExtensions.xml file.</span></span>
2. <span data-ttu-id="4c86b-148">Použití **spustit nyní** tootest hello profil upravit soubor zásad RP.</span><span class="sxs-lookup"><span data-stu-id="4c86b-148">Use **Run now** tootest hello profile edit RP policy file.</span></span>
3. <span data-ttu-id="4c86b-149">Test ověření hello některým z existující názvy hello (například mcvinny) v hello **křestní jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="4c86b-149">Test hello validation by providing one of hello existing names (for example, mcvinny) in hello **Given Name** field.</span></span> <span data-ttu-id="4c86b-150">Pokud všechno, co je správně nastavena, měli byste obdržet zprávu, která upozorní uživatele hello že hello player značky se již používá.</span><span class="sxs-lookup"><span data-stu-id="4c86b-150">If everything is set up correctly, you should receive a message that notifies hello user that hello player tag is already used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c86b-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c86b-151">Next steps</span></span>

[<span data-ttu-id="4c86b-152">Upravit hello profil upravit a uživatel toogather Další informace o registraci od uživatelů</span><span class="sxs-lookup"><span data-stu-id="4c86b-152">Modify hello profile edit and user registration toogather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[<span data-ttu-id="4c86b-153">Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako krok orchestration</span><span class="sxs-lookup"><span data-stu-id="4c86b-153">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>](active-directory-b2c-rest-api-step-custom.md)
