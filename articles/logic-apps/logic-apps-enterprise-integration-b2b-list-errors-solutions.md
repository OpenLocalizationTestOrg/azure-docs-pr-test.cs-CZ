---
title: "Seznam B2B aplikace logiky chyby a řešení: Azure App Service | Microsoft Docs"
description: "Seznam B2B aplikace logiky chyby a řešení"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 0340e2979f1972ba631354e206c93969e55946e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a><span data-ttu-id="a99ad-103">Seznam B2B aplikace logiky chyby a řešení</span><span class="sxs-lookup"><span data-stu-id="a99ad-103">Logic Apps B2B list of errors and solutions</span></span>  
<span data-ttu-id="a99ad-104">Tento článek vám pomůže vyřešit chyby, které by se mohlo stát v scénáře B2B aplikace logiky a navrhne příslušné akce při opravách tyto chyby.</span><span class="sxs-lookup"><span data-stu-id="a99ad-104">This article helps you troubleshoot errors that might happen in Logic Apps B2B scenarios and suggests appropriate actions for correcting those errors.</span></span>


## <a name="agreement-resolution"></a><span data-ttu-id="a99ad-105">Smlouva řešení</span><span class="sxs-lookup"><span data-stu-id="a99ad-105">Agreement Resolution</span></span>

### <a name="no-agreement-found"></a><span data-ttu-id="a99ad-106">* Byla nalezena žádná smlouva.</span><span class="sxs-lookup"><span data-stu-id="a99ad-106">*No agreement found</span></span> 

|   |   |  
|---|---|
| <span data-ttu-id="a99ad-107">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-107">Error description</span></span> | <span data-ttu-id="a99ad-108">Žádné smlouvy nalezen s parametry řešení smlouvy</span><span class="sxs-lookup"><span data-stu-id="a99ad-108">No agreement found with Agreement Resolution Parameters</span></span>|    
| <span data-ttu-id="a99ad-109">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-109">User action</span></span> | <span data-ttu-id="a99ad-110">Hello smlouvy musí být přidaní toohello účet integrace s identitami odsouhlaseného firmy.</span><span class="sxs-lookup"><span data-stu-id="a99ad-110">hello agreement should be added toohello integration account with agreed business identities.</span></span></br> <span data-ttu-id="a99ad-111">Hello firmy identity shodovat s identifikátory toohello vstupní zpráv</span><span class="sxs-lookup"><span data-stu-id="a99ad-111">hello business identities should match toohello input message ids</span></span>|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a><span data-ttu-id="a99ad-112">* Žádná smlouva se identity nalezena.</span><span class="sxs-lookup"><span data-stu-id="a99ad-112">* No agreement found with identities</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="a99ad-113">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-113">Error description</span></span> | <span data-ttu-id="a99ad-114">Žádná nalezena s identitami smlouva: 'AS2Identity':: 'Partner1' a 'AS2Identity':: 'Partner3.</span><span class="sxs-lookup"><span data-stu-id="a99ad-114">No agreement found with identities: 'AS2Identity'::'Partner1' and'AS2Identity'::'Partner3'</span></span>| 
| <span data-ttu-id="a99ad-115">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-115">User action</span></span> | <span data-ttu-id="a99ad-116">Neplatný AS2-z nebo AS2 tooconfigured pro smlouvu.</span><span class="sxs-lookup"><span data-stu-id="a99ad-116">Invalid AS2-From or AS2-tooconfigured for agreement.</span></span> </br> <span data-ttu-id="a99ad-117">Správné AS2 zpráva AS2-z nebo AS2 tooheaders nebo smlouvu toomatch AS2 ID v AS2 zprávy záhlaví s konfigurací smlouvy</span><span class="sxs-lookup"><span data-stu-id="a99ad-117">Correct AS2 message AS2-From or AS2-tooheaders or agreement toomatch AS2 ids in AS2 message headers with agreement configurations</span></span> |
|   |   |     

## <a name="as2"></a><span data-ttu-id="a99ad-118">AS2</span><span class="sxs-lookup"><span data-stu-id="a99ad-118">AS2</span></span>

### <a name="-missing-as2-message-headers"></a><span data-ttu-id="a99ad-119">* Chybí záhlaví AS2 zpráv</span><span class="sxs-lookup"><span data-stu-id="a99ad-119">* Missing AS2 message headers</span></span>  

|   |   |  
|---|---|
| <span data-ttu-id="a99ad-120">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-120">Error description</span></span>| <span data-ttu-id="a99ad-121">Neplatný AS2 hlavičky.</span><span class="sxs-lookup"><span data-stu-id="a99ad-121">Invalid AS2 headers.</span></span> <span data-ttu-id="a99ad-122">Jeden z ' AS2-k ' nebo ' AS2-z, hlavičky jsou prázdné</span><span class="sxs-lookup"><span data-stu-id="a99ad-122">One of 'AS2-To' or 'AS2-From' headers are empty</span></span>| 
| <span data-ttu-id="a99ad-123">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-123">User action</span></span> | <span data-ttu-id="a99ad-124">Byla přijata zpráva AS2, který neobsahoval hello AS2-z nebo AS2 tooor obou hlavičky.</span><span class="sxs-lookup"><span data-stu-id="a99ad-124">An AS2 message was received that did not contain hello AS2-From or AS2-tooor both headers.</span></span> </br> <span data-ttu-id="a99ad-125">Zkontrolujte zprávy a AS2 AS2-z a AS2 tooheaders a opravte je na základě smlouvy konfigurace</span><span class="sxs-lookup"><span data-stu-id="a99ad-125">Check AS2 message AS2-From and AS2-tooheaders and correct them based on agreement configuration</span></span> |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a><span data-ttu-id="a99ad-126">* Chybí tělo zprávy AS2 a hlavičky</span><span class="sxs-lookup"><span data-stu-id="a99ad-126">* Missing AS2 message body and headers</span></span>    

|   |   |  
|---|---|
| <span data-ttu-id="a99ad-127">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-127">Error description</span></span>| <span data-ttu-id="a99ad-128">obsah žádosti Hello je null nebo prázdná</span><span class="sxs-lookup"><span data-stu-id="a99ad-128">hello request content is null or empty</span></span> | 
| <span data-ttu-id="a99ad-129">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-129">User action</span></span> | <span data-ttu-id="a99ad-130">Byla přijata zpráva AS2, který neobsahoval tělo zprávy hello</span><span class="sxs-lookup"><span data-stu-id="a99ad-130">An AS2 message was received that did not contain hello message body</span></span> |
|  |  | 

### <a name="-as2-message-decryption-failure"></a><span data-ttu-id="a99ad-131">* Chyba při dešifrování zprávy AS2</span><span class="sxs-lookup"><span data-stu-id="a99ad-131">* AS2 message decryption failure</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="a99ad-132">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-132">Error description</span></span> |  <span data-ttu-id="a99ad-133">[zpracování chybou: Neúspěšné dešifrování]</span><span class="sxs-lookup"><span data-stu-id="a99ad-133">[processed/Error: decryption-failed]</span></span> | 
| <span data-ttu-id="a99ad-134">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-134">User action</span></span> | <span data-ttu-id="a99ad-135">Přidat @base64ToBinary tooAS2Message před odesláním toopartner</span><span class="sxs-lookup"><span data-stu-id="a99ad-135">Add @base64ToBinary tooAS2Message before sending toopartner</span></span> 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a><span data-ttu-id="a99ad-136">* Chyba při dešifrování MDN</span><span class="sxs-lookup"><span data-stu-id="a99ad-136">* MDN decryption failure</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="a99ad-137">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-137">Error description</span></span> |  <span data-ttu-id="a99ad-138">[zpracování chybou: Neúspěšné dešifrování]</span><span class="sxs-lookup"><span data-stu-id="a99ad-138">[processed/Error: decryption-failed]</span></span> | 
| <span data-ttu-id="a99ad-139">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-139">User action</span></span> | <span data-ttu-id="a99ad-140">Přidat @base64ToBinary tooMDN před odesláním toopartner</span><span class="sxs-lookup"><span data-stu-id="a99ad-140">Add @base64ToBinary tooMDN before sending toopartner</span></span> 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a><span data-ttu-id="a99ad-141">* Chybí podpisový certifikát</span><span class="sxs-lookup"><span data-stu-id="a99ad-141">* Missing signing certificate</span></span>

|   |   |  
|---|---|
| <span data-ttu-id="a99ad-142">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-142">Error description</span></span>| <span data-ttu-id="a99ad-143">Certifikát pro podpis Hello nebyla nakonfigurována pro stranu AS2.</span><span class="sxs-lookup"><span data-stu-id="a99ad-143">hello Signing Certificate has not been configured for AS2 party.</span></span> </br> <span data-ttu-id="a99ad-144">AS2-z: partner1 AS2-k: partner2</span><span class="sxs-lookup"><span data-stu-id="a99ad-144">AS2-From: partner1 AS2-To: partner2</span></span> | 
| <span data-ttu-id="a99ad-145">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-145">User action</span></span> | <span data-ttu-id="a99ad-146">Konfigurace nastavení smlouvy AS2 s správný certifikát pro podpis</span><span class="sxs-lookup"><span data-stu-id="a99ad-146">Configure AS2 agreement settings with correct certificate for signature</span></span> |
|  |  | 

## <a name="x12-and-edifact"></a><span data-ttu-id="a99ad-147">X12 a EDIFACT</span><span class="sxs-lookup"><span data-stu-id="a99ad-147">X12 and EDIFACT</span></span>

### <a name="-leading-or-trailing-space-found"></a><span data-ttu-id="a99ad-148">* Počáteční nebo koncové mezery nalezené</span><span class="sxs-lookup"><span data-stu-id="a99ad-148">* Leading or trailing space found</span></span>    
    
|   |   | 
|---|---|
| <span data-ttu-id="a99ad-149">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-149">Error description</span></span> | <span data-ttu-id="a99ad-150">Během analýzy došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="a99ad-150">Error encountered during parsing.</span></span> <span data-ttu-id="a99ad-151">Dobrý den Edifact transakce s id ' 123456 'obsažená v výměnu (bez skupina) s id ' 987654', s id odesílatele 'Partner1', id příjemce 'Partner2' bylo pozastaveno s následujícími chybami: úvodní koncové oddělovače nalezen</span><span class="sxs-lookup"><span data-stu-id="a99ad-151">hello Edifact transaction set with id '123456' contained in interchange (without group) with id '987654', with sender id 'Partner1', receiver id 'Partner2' is being suspended with following errors: Leading Trailing separator found</span></span> |
| <span data-ttu-id="a99ad-152">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-152">User action</span></span> | <span data-ttu-id="a99ad-153">Hello smlouvy nastavení toobe nakonfigurovat tooallow počátečních a koncových znaků.</span><span class="sxs-lookup"><span data-stu-id="a99ad-153">hello agreement settings toobe configured tooallow leading and trailing space.</span></span> </br> <span data-ttu-id="a99ad-154">Upravit nastavení tooallow smlouvy počátečních a koncových znaků</span><span class="sxs-lookup"><span data-stu-id="a99ad-154">Edit agreement settings tooallow leading and trailing space</span></span> |
|   |   |

![Povolit místa](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-hello-agreement"></a><span data-ttu-id="a99ad-156">* Duplicitní kontrola povolil hello smlouvy</span><span class="sxs-lookup"><span data-stu-id="a99ad-156">* Duplicate check has enabled in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="a99ad-157">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-157">Error description</span></span> | <span data-ttu-id="a99ad-158">Duplicitní číslo ovládacího prvku</span><span class="sxs-lookup"><span data-stu-id="a99ad-158">Duplicate Control Number</span></span> |
| <span data-ttu-id="a99ad-159">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-159">User action</span></span> | <span data-ttu-id="a99ad-160">Tato chyba znamená, že hello přijata zpráva má duplicitní řízení čísla.</span><span class="sxs-lookup"><span data-stu-id="a99ad-160">This error indicates hello received message has duplicate control numbers.</span></span> </br> <span data-ttu-id="a99ad-161">Opravte hello řízení číslo a opakujte odeslání uvítací zprávu</span><span class="sxs-lookup"><span data-stu-id="a99ad-161">Correct hello control number and resend hello message</span></span> |
|   |   |

### <a name="-missing-schema-in-hello-agreement"></a><span data-ttu-id="a99ad-162">* Chybějící schématu hello smlouvy</span><span class="sxs-lookup"><span data-stu-id="a99ad-162">* Missing schema in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="a99ad-163">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-163">Error description</span></span> | <span data-ttu-id="a99ad-164">Během analýzy došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="a99ad-164">Error encountered during parsing.</span></span> <span data-ttu-id="a99ad-165">Sada Hello X12 transakce s id '564220001' obsažená v funkční skupiny s id '56422', v výměnu s id "000056422" s id odesílatele ' 12345678', id příjemce ' 87654321' bylo pozastaveno s následujícími chybami "hello zpráva má ty neznámé dokumentu PE a nebylo možné přeložit tooany hello existující schémata nakonfigurované smlouvy hello "</span><span class="sxs-lookup"><span data-stu-id="a99ad-165">hello X12 transaction set with id '564220001' contained in functional group with id '56422', in interchange with id '000056422', with sender id '12345678       ', receiver id '87654321       ' is being suspended with following errors "hello message has an unknown document type and did not resolve tooany of hello existing schemas configured in hello agreement"</span></span> |
| <span data-ttu-id="a99ad-166">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-166">User action</span></span> | <span data-ttu-id="a99ad-167">Nakonfigurujte schématu smlouvy hello</span><span class="sxs-lookup"><span data-stu-id="a99ad-167">Configure schema in hello agreement settings</span></span>  |
|   |   |

### <a name="-incorrect-schema-in-hello-agreement"></a><span data-ttu-id="a99ad-168">* Nesprávné schéma hello smlouvy</span><span class="sxs-lookup"><span data-stu-id="a99ad-168">* Incorrect schema in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="a99ad-169">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-169">Error description</span></span> | <span data-ttu-id="a99ad-170">uvítací zprávu má typ Neznámý dokumentu a nebylo možné přeložit tooany hello existující schémata nakonfigurované hello smlouvy.</span><span class="sxs-lookup"><span data-stu-id="a99ad-170">hello message has an unknown document type and did not resolve tooany of hello existing schemas configured in hello agreement.</span></span> |
| <span data-ttu-id="a99ad-171">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-171">User action</span></span> | <span data-ttu-id="a99ad-172">Nakonfigurujte správné schéma smlouvy hello</span><span class="sxs-lookup"><span data-stu-id="a99ad-172">Configure correct schema in hello agreement settings</span></span>  |
|   |   |

## <a name="flat-file"></a><span data-ttu-id="a99ad-173">Plochý soubor</span><span class="sxs-lookup"><span data-stu-id="a99ad-173">Flat file</span></span>

### <a name="-input-message-with-no-body"></a><span data-ttu-id="a99ad-174">* Vstupní zprávu s žádný text</span><span class="sxs-lookup"><span data-stu-id="a99ad-174">* Input message with no body</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="a99ad-175">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="a99ad-175">Error description</span></span> | <span data-ttu-id="a99ad-176">InvalidTemplate.</span><span class="sxs-lookup"><span data-stu-id="a99ad-176">InvalidTemplate.</span></span> <span data-ttu-id="a99ad-177">Výrazy jazyka šablony nelze tooprocess v vstupy 'Flat_File_Decoding' akce v řádku '1' a ve sloupci '1902': ' požadované vlastnosti 'content' očekává hodnotu ale byla null.</span><span class="sxs-lookup"><span data-stu-id="a99ad-177">Unable tooprocess template language expressions in action 'Flat_File_Decoding' inputs at line '1' and column '1902': 'Required property 'content' expects a value but got null.</span></span> <span data-ttu-id="a99ad-178">Cesta k ".".</span><span class="sxs-lookup"><span data-stu-id="a99ad-178">Path ''.'.</span></span> |
| <span data-ttu-id="a99ad-179">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="a99ad-179">User action</span></span> | <span data-ttu-id="a99ad-180">Tato chyba označuje, že vstupní uvítací zprávu neobsahuje text</span><span class="sxs-lookup"><span data-stu-id="a99ad-180">This error indicates hello input message does not contain a body</span></span> |
|   |   | 

## <a name="learn-more"></a><span data-ttu-id="a99ad-181">Další informace</span><span class="sxs-lookup"><span data-stu-id="a99ad-181">Learn more</span></span>
[<span data-ttu-id="a99ad-182">Další informace o hello Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="a99ad-182">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
