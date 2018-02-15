---
title: "Správa relací jednotného přihlašování pomocí vlastních zásad – Azure AD B2C | Microsoft Docs"
description: "Naučte se spravovat relace jednotného přihlašování pomocí vlastních zásad v Azure AD B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 809f6000-2e52-43e4-995d-089d85747e1f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: parja
ms.openlocfilehash: 676b277ae3fbf4554838eee70c5d3e2d8e12c33d
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-ad-b2c-single-sign-on-sso-session-management"></a><span data-ttu-id="6a02c-103">Azure AD B2C: Jednotné přihlašování (SSO) relace správy</span><span class="sxs-lookup"><span data-stu-id="6a02c-103">Azure AD B2C: Single sign-on (SSO) session management</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="6a02c-104">Azure AD B2C umožňuje správci řídit, jak Azure AD B2C komunikuje s uživatelem po má již byl uživatel ověřen.</span><span class="sxs-lookup"><span data-stu-id="6a02c-104">Azure AD B2C allows an administrator to control how Azure AD B2C interacts with a user after the user has already authenticated.</span></span> <span data-ttu-id="6a02c-105">To se provádí přes správu relace jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a02c-105">This is done through SSO session management.</span></span> <span data-ttu-id="6a02c-106">Správce může například řídit, jestli se zobrazí výběr zprostředkovatelů identity, nebo zda je nutné znovu zadat podrobnosti o místní účet.</span><span class="sxs-lookup"><span data-stu-id="6a02c-106">For example, the administrator can control whether the selection of identity providers is displayed, or whether local account details need to be entered again.</span></span> <span data-ttu-id="6a02c-107">Tento článek popisuje postup konfigurace nastavení jednotného přihlašování pro Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="6a02c-107">This article describes how to configure the SSO settings for Azure AD B2C.</span></span>

## <a name="overview"></a><span data-ttu-id="6a02c-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="6a02c-108">Overview</span></span>

<span data-ttu-id="6a02c-109">Správa relací jednotné přihlašování má dvě části.</span><span class="sxs-lookup"><span data-stu-id="6a02c-109">SSO session management has two parts.</span></span> <span data-ttu-id="6a02c-110">První se zabývá interakcí přímo s Azure AD B2C a jiné obchody s interakce uživatele s externími uživateli, jako je Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a02c-110">The first deals with the user's interactions directly with Azure AD B2C and the other deals with the user's interactions with external parties such as Facebook.</span></span> <span data-ttu-id="6a02c-111">Azure AD B2C nepřepisuje ani vynechat relace jednotného přihlašování, které může uchovávat externího stranami.</span><span class="sxs-lookup"><span data-stu-id="6a02c-111">Azure AD B2C does not override or bypass SSO sessions that might be held by external parties.</span></span> <span data-ttu-id="6a02c-112">Místo směrovat přes Azure AD B2C, abyste se dostali na externí strany je "zapamatovaných", nebudete muset reprompt uživateli vybrat jejich sociálního nebo enterprise zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="6a02c-112">Rather the route through Azure AD B2C to get to the external party is “remembered”, avoiding the need to reprompt the user to select their social or enterprise identity provider.</span></span> <span data-ttu-id="6a02c-113">Ultimate jednotného přihlašování k rozhodnutí zůstává s externí strany.</span><span class="sxs-lookup"><span data-stu-id="6a02c-113">The ultimate SSO decision remains with the external party.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="6a02c-114">Jak to funguje?</span><span class="sxs-lookup"><span data-stu-id="6a02c-114">How does it work?</span></span>

<span data-ttu-id="6a02c-115">Správa relace jednotného přihlašování k využívá stejnou sémantiku jako další technické profil ve vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="6a02c-115">SSO session management uses the same semantics as any other technical profile in custom policies.</span></span> <span data-ttu-id="6a02c-116">Když se spustí na krok orchestration, technické profil spojený s krokem je dotazován na `UseTechnicalProfileForSessionManagement` odkaz.</span><span class="sxs-lookup"><span data-stu-id="6a02c-116">When an orchestration step is executed, the technical profile associated with the step is queried for a `UseTechnicalProfileForSessionManagement` reference.</span></span> <span data-ttu-id="6a02c-117">Pokud existuje, odkazovaná zprostředkovatele relace jednotného přihlašování k kontroluje zda uživatel je účastníkem relace.</span><span class="sxs-lookup"><span data-stu-id="6a02c-117">If one exists, the referenced SSO session provider is then checked to see if the user is a session participant.</span></span> <span data-ttu-id="6a02c-118">Pokud tak zprostředkovatele relace jednotného přihlašování se používá k znovu vytvořit relace.</span><span class="sxs-lookup"><span data-stu-id="6a02c-118">If so the SSO session provider is used to repopulate the session.</span></span> <span data-ttu-id="6a02c-119">Podobně po dokončení provádění na krok orchestration zprostředkovatele se používá k ukládání informací v relaci, pokud byla zadána poskytovatele relace jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a02c-119">Similarly, when the execution of an orchestration step is complete, the provider is used to store information in the session if an SSO session provider has been specified.</span></span>

<span data-ttu-id="6a02c-120">Azure AD B2C je definovaný několik poskytovatelů relace jednotného přihlašování, které lze použít:</span><span class="sxs-lookup"><span data-stu-id="6a02c-120">Azure AD B2C has defined a number of SSO session providers that can be used:</span></span>

* <span data-ttu-id="6a02c-121">NoopSSOSessionProvider</span><span class="sxs-lookup"><span data-stu-id="6a02c-121">NoopSSOSessionProvider</span></span>
* <span data-ttu-id="6a02c-122">DefaultSSOSessionProvider</span><span class="sxs-lookup"><span data-stu-id="6a02c-122">DefaultSSOSessionProvider</span></span>
* <span data-ttu-id="6a02c-123">ExternalLoginSSOSessionProvider</span><span class="sxs-lookup"><span data-stu-id="6a02c-123">ExternalLoginSSOSessionProvider</span></span>
* <span data-ttu-id="6a02c-124">SamlSSOSessionProvider</span><span class="sxs-lookup"><span data-stu-id="6a02c-124">SamlSSOSessionProvider</span></span>

<span data-ttu-id="6a02c-125">Jednotné přihlašování třídy pro správu jsou určeny pomocí `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` element technické profilu.</span><span class="sxs-lookup"><span data-stu-id="6a02c-125">SSO management classes are specified using the `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` element of a technical profile.</span></span>

### <a name="noopssosessionprovider"></a><span data-ttu-id="6a02c-126">NoopSSOSessionProvider</span><span class="sxs-lookup"><span data-stu-id="6a02c-126">NoopSSOSessionProvider</span></span>

<span data-ttu-id="6a02c-127">Jako název stanoví, tohoto zprostředkovatele neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="6a02c-127">As the name dictates, this provider does nothing.</span></span> <span data-ttu-id="6a02c-128">Tento zprostředkovatel slouží pro potlačení chování jednotného přihlašování pro konkrétní technické profil.</span><span class="sxs-lookup"><span data-stu-id="6a02c-128">This provider can be used for suppressing SSO behavior for a specific technical profile.</span></span>

### <a name="defaultssosessionprovider"></a><span data-ttu-id="6a02c-129">DefaultSSOSessionProvider</span><span class="sxs-lookup"><span data-stu-id="6a02c-129">DefaultSSOSessionProvider</span></span>

<span data-ttu-id="6a02c-130">Tento zprostředkovatel mohou být použity k uložení deklarace identity v relaci.</span><span class="sxs-lookup"><span data-stu-id="6a02c-130">This provider can be used for storing claims in a session.</span></span> <span data-ttu-id="6a02c-131">Tento zprostředkovatel se obvykle odkazuje v profilu technické používaným ke správě místních účtů.</span><span class="sxs-lookup"><span data-stu-id="6a02c-131">This provider is typically referenced in a technical profile used for managing local accounts.</span></span> 

```XML
<TechnicalProfile Id="SM-AAD">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="newUser" />
        <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
    </PersistedClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true" />
    </OutputClaims>
</TechnicalProfile>
```

<span data-ttu-id="6a02c-132">K přidávání deklarací identit v relaci, použijte `<PersistedClaims>` element technické profilu.</span><span class="sxs-lookup"><span data-stu-id="6a02c-132">To add claims in the session, use the `<PersistedClaims>` element of the technical profile.</span></span> <span data-ttu-id="6a02c-133">Když se zprostředkovatel používá k znovu vytvořit relaci, trvalou deklarace identity se nepřidají do kontejneru deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="6a02c-133">When the provider is used to repopulate the session, the persisted claims are added to the claims bag.</span></span> <span data-ttu-id="6a02c-134">`<OutputClaims>` slouží k načítání deklarace identity z relace.</span><span class="sxs-lookup"><span data-stu-id="6a02c-134">`<OutputClaims>` is used for retrieving claims from the session.</span></span>

### <a name="externalloginssosessionprovider"></a><span data-ttu-id="6a02c-135">ExternalLoginSSOSessionProvider</span><span class="sxs-lookup"><span data-stu-id="6a02c-135">ExternalLoginSSOSessionProvider</span></span>

<span data-ttu-id="6a02c-136">Tento zprostředkovatel se používá k potlačení obrazovce "Vyberte zprostředkovatele identity".</span><span class="sxs-lookup"><span data-stu-id="6a02c-136">This provider is used to suppress the “choose identity provider” screen.</span></span> <span data-ttu-id="6a02c-137">Obvykle se odkazuje v profilu technické nakonfigurovaný pro zprostředkovatele externí identity, jako je Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a02c-137">It is typically referenced in a technical profile configured for an external identity provider, such as Facebook.</span></span> 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a><span data-ttu-id="6a02c-138">SamlSSOSessionProvider</span><span class="sxs-lookup"><span data-stu-id="6a02c-138">SamlSSOSessionProvider</span></span>

<span data-ttu-id="6a02c-139">Tento zprostředkovatel se používá pro správu Azure AD B2C SAML relací mezi aplikací, jakož i externí zprostředkovatele identity SAML.</span><span class="sxs-lookup"><span data-stu-id="6a02c-139">This provider is used for managing the Azure AD B2C SAML sessions between apps as well as external SAML identity providers.</span></span>

```XML
<TechnicalProfile Id="SM-Reflector-SAML">
    <DisplayName>Session Management Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="IncludeSessionIndex">false</Item>
        <Item Key="RegisterServiceProviders">false</Item>
    </Metadata>
</TechnicalProfile>
```

<span data-ttu-id="6a02c-140">Existují dvě položky metadat v technické profilu:</span><span class="sxs-lookup"><span data-stu-id="6a02c-140">There are two metadata items in the technical profile:</span></span>

| <span data-ttu-id="6a02c-141">Položka</span><span class="sxs-lookup"><span data-stu-id="6a02c-141">Item</span></span> | <span data-ttu-id="6a02c-142">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="6a02c-142">Default Value</span></span> | <span data-ttu-id="6a02c-143">Možné hodnoty</span><span class="sxs-lookup"><span data-stu-id="6a02c-143">Possible Values</span></span> | <span data-ttu-id="6a02c-144">Popis</span><span class="sxs-lookup"><span data-stu-id="6a02c-144">Description</span></span>
| --- | --- | --- | --- |
| <span data-ttu-id="6a02c-145">IncludeSessionIndex</span><span class="sxs-lookup"><span data-stu-id="6a02c-145">IncludeSessionIndex</span></span> | <span data-ttu-id="6a02c-146">true (pravda)</span><span class="sxs-lookup"><span data-stu-id="6a02c-146">true</span></span> | <span data-ttu-id="6a02c-147">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="6a02c-147">true/false</span></span> | <span data-ttu-id="6a02c-148">K poskytovateli označuje, že by měly být uložené relace index.</span><span class="sxs-lookup"><span data-stu-id="6a02c-148">Indicates to the provider that the session index should be stored.</span></span> |
| <span data-ttu-id="6a02c-149">RegisterServiceProviders</span><span class="sxs-lookup"><span data-stu-id="6a02c-149">RegisterServiceProviders</span></span> | <span data-ttu-id="6a02c-150">true (pravda)</span><span class="sxs-lookup"><span data-stu-id="6a02c-150">true</span></span> | <span data-ttu-id="6a02c-151">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="6a02c-151">true/false</span></span> | <span data-ttu-id="6a02c-152">Označuje, že zprostředkovatel měli zaregistrovat všechny poskytovatele služby SAML, které byly vydané kontrolní výrazy.</span><span class="sxs-lookup"><span data-stu-id="6a02c-152">Indicates that the provider should register all SAML service providers that have been issued an assertion.</span></span> |

<span data-ttu-id="6a02c-153">Při použití zprostředkovatele pro ukládání relace poskytovatele identity SAML, uvedených položek musí mít oba hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="6a02c-153">When using the provider for storing a SAML identity provider session, the items above should both be false.</span></span> <span data-ttu-id="6a02c-154">Při použití zprostředkovatele pro ukládání relace B2C SAML, uvedených položek musí být true nebo není uveden jako výchozí hodnoty jsou true.</span><span class="sxs-lookup"><span data-stu-id="6a02c-154">When using the provider for storing the B2C SAML session, the items above should be true or omitted as the defaults are true.</span></span>

>[!NOTE]
> <span data-ttu-id="6a02c-155">Odhlášení relace SAML vyžaduje `SessionIndex` a `NameID` k dokončení.</span><span class="sxs-lookup"><span data-stu-id="6a02c-155">SAML session logout requires the `SessionIndex` and `NameID` to complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a02c-156">Další postup</span><span class="sxs-lookup"><span data-stu-id="6a02c-156">Next steps</span></span>

<span data-ttu-id="6a02c-157">Jsme rádi, názory a návrhy!</span><span class="sxs-lookup"><span data-stu-id="6a02c-157">We love feedback and suggestions!</span></span> <span data-ttu-id="6a02c-158">Pokud máte jakékoli problémy s tímto tématem, můžete zveřejnit na Stack Overflow pomocí značky [, azure ad b2c,](https://stackoverflow.com/questions/tagged/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="6a02c-158">If you have any difficulties with this topic, post on Stack Overflow using the tag ['azure-ad-b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c).</span></span> <span data-ttu-id="6a02c-159">Pro žádosti o funkce, hlasování u nich naše [fóru pro zpětnou vazbu](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="6a02c-159">For feature requests, vote for them in our [feedback forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>

