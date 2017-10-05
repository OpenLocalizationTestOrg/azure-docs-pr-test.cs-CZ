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
ms.openlocfilehash: eb44a0d2234c9ee3801d8b3a1655d877aa2f4fef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a><span data-ttu-id="781b9-103">Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako ověření na vstup uživatele</span><span class="sxs-lookup"><span data-stu-id="781b9-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input</span></span>

<span data-ttu-id="781b9-104">Framework prostředí Identity (IEF) podkladovou Azure Active Directory B2C (Azure AD B2C) umožňuje vývojáři identity integrovat interakci s rozhraní RESTful API cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="781b9-104">The Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables the identity developer to integrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="781b9-105">Na konci tohoto průvodce bude možné vytvořit cestu uživatele Azure AD B2C, která komunikuje s služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="781b9-105">At the end of this walkthrough, you will be able to create an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="781b9-106">IEF odesílá data v deklaracích identity a přijímá data zpět v deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="781b9-106">The IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="781b9-107">Interakce s rozhraním API:</span><span class="sxs-lookup"><span data-stu-id="781b9-107">The interaction with the API:</span></span>

- <span data-ttu-id="781b9-108">Můžete třeba navrhnout jako deklarace identity systému exchange REST API nebo jako profil ověření, který se stane uvnitř na krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="781b9-108">Can be designed as a REST API claims exchange or as a validation profile, which happens inside an orchestration step.</span></span>
- <span data-ttu-id="781b9-109">Obvykle ověří vstup od uživatele.</span><span class="sxs-lookup"><span data-stu-id="781b9-109">Typically validates input from the user.</span></span> <span data-ttu-id="781b9-110">Pokud se odmítne hodnota od uživatele, uživatel můžete zkusit znovu zadejte platnou hodnotu s možnost vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="781b9-110">If the value from the user is rejected, the user can try again to enter a valid value with the opportunity to return an error message.</span></span>

<span data-ttu-id="781b9-111">Můžete taky navrhnout interakce jako krok orchestration.</span><span class="sxs-lookup"><span data-stu-id="781b9-111">You can also design the interaction as an orchestration step.</span></span> <span data-ttu-id="781b9-112">Další informace najdete v tématu [návod: integrace rozhraní API REST deklarací výměn v vám dobře slouží uživatele Azure AD B2C jako krok orchestration](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="781b9-112">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step](active-directory-b2c-rest-api-step-custom.md).</span></span>

<span data-ttu-id="781b9-113">Pro ověření profil příkladu použijeme cesty profil upravit uživatele v souboru pack starter ProfileEdit.xml.</span><span class="sxs-lookup"><span data-stu-id="781b9-113">For the validation profile example, we will use the profile edit user journey in the starter pack file ProfileEdit.xml.</span></span>

<span data-ttu-id="781b9-114">Jsme můžete ověřit, že zadaný uživatelem v profilu upravit název není součástí seznam vyloučení.</span><span class="sxs-lookup"><span data-stu-id="781b9-114">We can verify that the name provided by the user in the profile edit is not part of an exclusion list.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="781b9-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="781b9-115">Prerequisites</span></span>

- <span data-ttu-id="781b9-116">Klient služby Azure AD B2C, nakonfigurované k dokončení registrace-množství nebo přihlášení, místní účet, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="781b9-116">An Azure AD B2C tenant configured to complete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="781b9-117">Koncový bod REST API pro interakci s.</span><span class="sxs-lookup"><span data-stu-id="781b9-117">A REST API endpoint to interact with.</span></span> <span data-ttu-id="781b9-118">V tomto návodu, jsme zřídili ukázkový web s názvem [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) službou REST API.</span><span class="sxs-lookup"><span data-stu-id="781b9-118">For this walkthrough, we've set up a demo site called [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) with a REST API service.</span></span>

## <a name="step-1-prepare-the-rest-api-function"></a><span data-ttu-id="781b9-119">Krok 1: Příprava funkce rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="781b9-119">Step 1: Prepare the REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="781b9-120">Instalace funkce rozhraní API REST je mimo rámec tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="781b9-120">Setup of REST API functions is outside the scope of this article.</span></span> <span data-ttu-id="781b9-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) poskytuje vynikající sada nástrojů pro vytváření služeb RESTful v cloudu.</span><span class="sxs-lookup"><span data-stu-id="781b9-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit to create RESTful services in the cloud.</span></span>

<span data-ttu-id="781b9-122">Vytvořili jsme Azure funkce, která obdrží deklarace identity, která se očekává, že jako `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="781b9-122">We have created an Azure function that receives a claim that it expects as `playerTag`.</span></span> <span data-ttu-id="781b9-123">Funkce ověřuje, zda existuje tuto deklaraci.</span><span class="sxs-lookup"><span data-stu-id="781b9-123">The function validates whether this claim exists.</span></span> <span data-ttu-id="781b9-124">Dostanete kód dokončení funkce Azure v [Githubu](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="781b9-124">You can access the complete Azure function code in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

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
      userMessage = $"The player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

<span data-ttu-id="781b9-125">Očekává IEF `userMessage` deklarace identity, který vrací funkci Azure.</span><span class="sxs-lookup"><span data-stu-id="781b9-125">The IEF expects the `userMessage` claim that the Azure function returns.</span></span> <span data-ttu-id="781b9-126">Tuto deklaraci bude zobrazovat jako řetězec uživatelům, pokud se ověření nezdaří, například když se vrátí stav 409 – konflikt v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="781b9-126">This claim will be presented as a string to the user if the validation fails, such as when a 409 conflict status is returned in the preceding example.</span></span>

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="781b9-127">Krok 2: Konfigurace exchange deklarace identity rozhraní RESTful API jako technické profil v souboru TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="781b9-127">Step 2: Configure the RESTful API claims exchange as a technical profile in your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="781b9-128">Technické profil je úplná konfigurace exchange potřeby u služby RESTful.</span><span class="sxs-lookup"><span data-stu-id="781b9-128">A technical profile is the full configuration of the exchange desired with the RESTful service.</span></span> <span data-ttu-id="781b9-129">Otevřete soubor TrustFrameworkExtensions.xml a přidejte následující fragment kódu XML uvnitř `<ClaimsProviders>` elementu.</span><span class="sxs-lookup"><span data-stu-id="781b9-129">Open the TrustFrameworkExtensions.xml file and add the following XML snippet inside the `<ClaimsProviders>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="781b9-130">V následující soubor XML, RESTful zprostředkovatele `Version=1.0.0.0` označen jako protokol.</span><span class="sxs-lookup"><span data-stu-id="781b9-130">In the following XML, RESTful provider `Version=1.0.0.0` is described as the protocol.</span></span> <span data-ttu-id="781b9-131">Považuje za jako funkce, která bude komunikovat s externí služby.</span><span class="sxs-lookup"><span data-stu-id="781b9-131">Consider it as the function that will interact with the external service.</span></span> <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

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

<span data-ttu-id="781b9-132">`InputClaims` Element definuje deklarace identity, které budou odesílané z IEF ke službě REST.</span><span class="sxs-lookup"><span data-stu-id="781b9-132">The `InputClaims` element defines the claims that will be sent from the IEF to the REST service.</span></span> <span data-ttu-id="781b9-133">V tomto příkladu obsah deklarace `givenName` zašle službě REST jako `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="781b9-133">In this example, the contents of the claim `givenName` will be sent to the REST service as `playerTag`.</span></span> <span data-ttu-id="781b9-134">V tomto příkladu IEF neočekává zpět deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="781b9-134">In this example, the IEF does not expect claims back.</span></span> <span data-ttu-id="781b9-135">Místo toho čeká na odpověď od služby REST a jednání založené na stavové kódy, které obdrží.</span><span class="sxs-lookup"><span data-stu-id="781b9-135">Instead, it waits for a response from the REST service and acts based on the status codes that it receives.</span></span>

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a><span data-ttu-id="781b9-136">Krok 3: Zahrnout exchange deklarace identity služba RESTful samoobslužné uplatňovaná technické profil, ve které chcete ověření vstupu uživatele</span><span class="sxs-lookup"><span data-stu-id="781b9-136">Step 3: Include the RESTful service claims exchange in self-asserted technical profile where you want to validate the user input</span></span>

<span data-ttu-id="781b9-137">Krok ověření slouží nejčastěji v interakci s uživatelem.</span><span class="sxs-lookup"><span data-stu-id="781b9-137">The most common use of the validation step is in the interaction with a user.</span></span> <span data-ttu-id="781b9-138">Všechny interakce, kde se očekává poskytování vstup uživatele jsou *samoobslužné prohlašovanou technické profily*.</span><span class="sxs-lookup"><span data-stu-id="781b9-138">All interactions where the user is expected to provide input are *self-asserted technical profiles*.</span></span> <span data-ttu-id="781b9-139">V tomto příkladu přidáme technické profil samoobslužných Asserted ProfileUpdate ověření.</span><span class="sxs-lookup"><span data-stu-id="781b9-139">For this example, we will add the validation to the Self-Asserted-ProfileUpdate technical profile.</span></span> <span data-ttu-id="781b9-140">Toto je technická profilu, který soubor zásad předávající stranu `Profile Edit` používá.</span><span class="sxs-lookup"><span data-stu-id="781b9-140">This is the technical profile that the relying party (RP) policy file `Profile Edit` uses.</span></span>

<span data-ttu-id="781b9-141">Přidání deklarace identity exchange do samoobslužné uplatňovaná technické profil:</span><span class="sxs-lookup"><span data-stu-id="781b9-141">To add the claims exchange to the self-asserted technical profile:</span></span>

1. <span data-ttu-id="781b9-142">Otevřete soubor TrustFrameworkBase.xml a vyhledejte `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span><span class="sxs-lookup"><span data-stu-id="781b9-142">Open the TrustFrameworkBase.xml file and search for `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span></span>
2. <span data-ttu-id="781b9-143">Zkontrolujte konfiguraci tohoto technické profilu.</span><span class="sxs-lookup"><span data-stu-id="781b9-143">Review the configuration of this technical profile.</span></span> <span data-ttu-id="781b9-144">Sledujte, jak je exchange s uživatelem definovaný jako deklarace identity, které se dotaz uživatele (vstupních deklarací identity) a deklarací identity, které se budou zpět ze samoobslužných uplatňovaná zprostředkovatele (výstup deklarace identity).</span><span class="sxs-lookup"><span data-stu-id="781b9-144">Observe how the exchange with the user is defined as claims that will be asked of the user (input claims) and claims that will be expected back from the self-asserted provider (output claims).</span></span>
3. <span data-ttu-id="781b9-145">Vyhledejte `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`a Všimněte si, že tento profil je vyvolána jako orchestration krok 6 `<UserJourney Id="ProfileEdit">`.</span><span class="sxs-lookup"><span data-stu-id="781b9-145">Search for `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, and notice that this profile is invoked as orchestration step 6 of `<UserJourney Id="ProfileEdit">`.</span></span>

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a><span data-ttu-id="781b9-146">Krok 4: Nahrání a testovací soubor zásad RP úpravy profilu</span><span class="sxs-lookup"><span data-stu-id="781b9-146">Step 4: Upload and test the profile edit RP policy file</span></span>

1. <span data-ttu-id="781b9-147">Nahrajte novou verzi souboru TrustFrameworkExtensions.xml.</span><span class="sxs-lookup"><span data-stu-id="781b9-147">Upload the new version of the TrustFrameworkExtensions.xml file.</span></span>
2. <span data-ttu-id="781b9-148">Použití **spustit nyní** k testování soubor zásad RP úpravy profilu.</span><span class="sxs-lookup"><span data-stu-id="781b9-148">Use **Run now** to test the profile edit RP policy file.</span></span>
3. <span data-ttu-id="781b9-149">Test ověření některým z existující názvy (například mcvinny) v **křestní jméno** pole.</span><span class="sxs-lookup"><span data-stu-id="781b9-149">Test the validation by providing one of the existing names (for example, mcvinny) in the **Given Name** field.</span></span> <span data-ttu-id="781b9-150">Pokud všechno, co je správně nastavena, měli byste obdržet zprávu, která upozorní uživatele, že player značky se již používá.</span><span class="sxs-lookup"><span data-stu-id="781b9-150">If everything is set up correctly, you should receive a message that notifies the user that the player tag is already used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="781b9-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="781b9-151">Next steps</span></span>

[<span data-ttu-id="781b9-152">Upravit profil registrace upravit a uživatelů sbírat dodatečné informace od uživatelů</span><span class="sxs-lookup"><span data-stu-id="781b9-152">Modify the profile edit and user registration to gather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[<span data-ttu-id="781b9-153">Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako krok orchestration</span><span class="sxs-lookup"><span data-stu-id="781b9-153">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>](active-directory-b2c-rest-api-step-custom.md)
