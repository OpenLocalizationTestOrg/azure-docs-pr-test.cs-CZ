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
ms.openlocfilehash: 1865d75f1b4c2aa18d5a3130f639572d19563b3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a><span data-ttu-id="c7238-103">Seznam B2B aplikace logiky chyby a řešení</span><span class="sxs-lookup"><span data-stu-id="c7238-103">Logic Apps B2B list of errors and solutions</span></span>  
<span data-ttu-id="c7238-104">Tento článek vám pomůže vyřešit chyby, které by se mohlo stát v scénáře B2B aplikace logiky a navrhne příslušné akce při opravách tyto chyby.</span><span class="sxs-lookup"><span data-stu-id="c7238-104">This article helps you troubleshoot errors that might happen in Logic Apps B2B scenarios and suggests appropriate actions for correcting those errors.</span></span>


## <a name="agreement-resolution"></a><span data-ttu-id="c7238-105">Smlouva řešení</span><span class="sxs-lookup"><span data-stu-id="c7238-105">Agreement Resolution</span></span>

### <a name="no-agreement-found"></a><span data-ttu-id="c7238-106">* Byla nalezena žádná smlouva.</span><span class="sxs-lookup"><span data-stu-id="c7238-106">*No agreement found</span></span> 

|   |   |  
|---|---|
| <span data-ttu-id="c7238-107">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-107">Error description</span></span> | <span data-ttu-id="c7238-108">Žádné smlouvy nalezen s parametry řešení smlouvy</span><span class="sxs-lookup"><span data-stu-id="c7238-108">No agreement found with Agreement Resolution Parameters</span></span>|    
| <span data-ttu-id="c7238-109">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-109">User action</span></span> | <span data-ttu-id="c7238-110">Smlouvy musí být přidaní do účtu integrace s identitami odsouhlaseného firmy.</span><span class="sxs-lookup"><span data-stu-id="c7238-110">The agreement should be added to the integration account with agreed business identities.</span></span></br> <span data-ttu-id="c7238-111">Obchodní identity by se měl shodovat identifikátory vstupní zpráv</span><span class="sxs-lookup"><span data-stu-id="c7238-111">The business identities should match to the input message ids</span></span>|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a><span data-ttu-id="c7238-112">* Žádná smlouva se identity nalezena.</span><span class="sxs-lookup"><span data-stu-id="c7238-112">* No agreement found with identities</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="c7238-113">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-113">Error description</span></span> | <span data-ttu-id="c7238-114">Žádná nalezena s identitami smlouva: 'AS2Identity':: 'Partner1' a 'AS2Identity':: 'Partner3.</span><span class="sxs-lookup"><span data-stu-id="c7238-114">No agreement found with identities: 'AS2Identity'::'Partner1' and'AS2Identity'::'Partner3'</span></span>| 
| <span data-ttu-id="c7238-115">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-115">User action</span></span> | <span data-ttu-id="c7238-116">Neplatný AS2-z nebo AS2-na nakonfigurované pro smlouvu.</span><span class="sxs-lookup"><span data-stu-id="c7238-116">Invalid AS2-From or AS2-To configured for agreement.</span></span> </br> <span data-ttu-id="c7238-117">Správné AS2 zpráva AS2-z nebo AS2-hlavičky nebo dohody, tak, aby odpovídaly AS2 ID v AS2 zprávy záhlaví s konfigurací smlouvy</span><span class="sxs-lookup"><span data-stu-id="c7238-117">Correct AS2 message AS2-From or AS2-To headers or agreement to match AS2 ids in AS2 message headers with agreement configurations</span></span> |
|   |   |     

## <a name="as2"></a><span data-ttu-id="c7238-118">AS2</span><span class="sxs-lookup"><span data-stu-id="c7238-118">AS2</span></span>

### <a name="-missing-as2-message-headers"></a><span data-ttu-id="c7238-119">* Chybí záhlaví AS2 zpráv</span><span class="sxs-lookup"><span data-stu-id="c7238-119">* Missing AS2 message headers</span></span>  

|   |   |  
|---|---|
| <span data-ttu-id="c7238-120">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-120">Error description</span></span>| <span data-ttu-id="c7238-121">Neplatný AS2 hlavičky.</span><span class="sxs-lookup"><span data-stu-id="c7238-121">Invalid AS2 headers.</span></span> <span data-ttu-id="c7238-122">Jeden z ' AS2-k ' nebo ' AS2-z, hlavičky jsou prázdné</span><span class="sxs-lookup"><span data-stu-id="c7238-122">One of 'AS2-To' or 'AS2-From' headers are empty</span></span>| 
| <span data-ttu-id="c7238-123">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-123">User action</span></span> | <span data-ttu-id="c7238-124">Byla přijata zpráva AS2, který neobsahoval AS2-z nebo AS2-na nebo oba hlavičky.</span><span class="sxs-lookup"><span data-stu-id="c7238-124">An AS2 message was received that did not contain the AS2-From or AS2-To or both headers.</span></span> </br> <span data-ttu-id="c7238-125">Zkontrolujte zprávy a AS2 AS2-z a AS2-hlavičky a správnou je na základě smlouvy konfigurace</span><span class="sxs-lookup"><span data-stu-id="c7238-125">Check AS2 message AS2-From and AS2-To headers and correct them based on agreement configuration</span></span> |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a><span data-ttu-id="c7238-126">* Chybí tělo zprávy AS2 a hlavičky</span><span class="sxs-lookup"><span data-stu-id="c7238-126">* Missing AS2 message body and headers</span></span>    

|   |   |  
|---|---|
| <span data-ttu-id="c7238-127">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-127">Error description</span></span>| <span data-ttu-id="c7238-128">Obsah žádosti je null nebo prázdná</span><span class="sxs-lookup"><span data-stu-id="c7238-128">The request content is null or empty</span></span> | 
| <span data-ttu-id="c7238-129">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-129">User action</span></span> | <span data-ttu-id="c7238-130">Byla přijata zpráva AS2, který neobsahoval text zprávy</span><span class="sxs-lookup"><span data-stu-id="c7238-130">An AS2 message was received that did not contain the message body</span></span> |
|  |  | 

### <a name="-as2-message-decryption-failure"></a><span data-ttu-id="c7238-131">* Chyba při dešifrování zprávy AS2</span><span class="sxs-lookup"><span data-stu-id="c7238-131">* AS2 message decryption failure</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="c7238-132">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-132">Error description</span></span> |  <span data-ttu-id="c7238-133">[zpracování chybou: Neúspěšné dešifrování]</span><span class="sxs-lookup"><span data-stu-id="c7238-133">[processed/Error: decryption-failed]</span></span> | 
| <span data-ttu-id="c7238-134">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-134">User action</span></span> | <span data-ttu-id="c7238-135">Přidat @base64ToBinary k AS2Message před odesláním partnerovi</span><span class="sxs-lookup"><span data-stu-id="c7238-135">Add @base64ToBinary to AS2Message before sending to partner</span></span> 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a><span data-ttu-id="c7238-136">* Chyba při dešifrování MDN</span><span class="sxs-lookup"><span data-stu-id="c7238-136">* MDN decryption failure</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="c7238-137">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-137">Error description</span></span> |  <span data-ttu-id="c7238-138">[zpracování chybou: Neúspěšné dešifrování]</span><span class="sxs-lookup"><span data-stu-id="c7238-138">[processed/Error: decryption-failed]</span></span> | 
| <span data-ttu-id="c7238-139">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-139">User action</span></span> | <span data-ttu-id="c7238-140">Přidat @base64ToBinary k MDN před odesláním partnerovi</span><span class="sxs-lookup"><span data-stu-id="c7238-140">Add @base64ToBinary to MDN before sending to partner</span></span> 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a><span data-ttu-id="c7238-141">* Chybí podpisový certifikát</span><span class="sxs-lookup"><span data-stu-id="c7238-141">* Missing signing certificate</span></span>

|   |   |  
|---|---|
| <span data-ttu-id="c7238-142">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-142">Error description</span></span>| <span data-ttu-id="c7238-143">Podpisový certifikát nebyl nakonfigurován pro stranu AS2.</span><span class="sxs-lookup"><span data-stu-id="c7238-143">The Signing Certificate has not been configured for AS2 party.</span></span> </br> <span data-ttu-id="c7238-144">AS2-z: partner1 AS2-k: partner2</span><span class="sxs-lookup"><span data-stu-id="c7238-144">AS2-From: partner1 AS2-To: partner2</span></span> | 
| <span data-ttu-id="c7238-145">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-145">User action</span></span> | <span data-ttu-id="c7238-146">Konfigurace nastavení smlouvy AS2 s správný certifikát pro podpis</span><span class="sxs-lookup"><span data-stu-id="c7238-146">Configure AS2 agreement settings with correct certificate for signature</span></span> |
|  |  | 

## <a name="x12-and-edifact"></a><span data-ttu-id="c7238-147">X12 a EDIFACT</span><span class="sxs-lookup"><span data-stu-id="c7238-147">X12 and EDIFACT</span></span>

### <a name="-leading-or-trailing-space-found"></a><span data-ttu-id="c7238-148">* Počáteční nebo koncové mezery nalezené</span><span class="sxs-lookup"><span data-stu-id="c7238-148">* Leading or trailing space found</span></span>    
    
|   |   | 
|---|---|
| <span data-ttu-id="c7238-149">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-149">Error description</span></span> | <span data-ttu-id="c7238-150">Během analýzy došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="c7238-150">Error encountered during parsing.</span></span> <span data-ttu-id="c7238-151">Transakce Edifact nastavit s id ' 123456 'obsažená v výměnu (bez skupina) s id ' 987654', s id odesílatele 'Partner1', id příjemce 'Partner2' bylo pozastaveno s následujícími chybami: úvodní koncové oddělovače nalezen</span><span class="sxs-lookup"><span data-stu-id="c7238-151">The Edifact transaction set with id '123456' contained in interchange (without group) with id '987654', with sender id 'Partner1', receiver id 'Partner2' is being suspended with following errors: Leading Trailing separator found</span></span> |
| <span data-ttu-id="c7238-152">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-152">User action</span></span> | <span data-ttu-id="c7238-153">Nastavení smlouvy a nakonfigurovat tak, aby počátečních a koncových znaků.</span><span class="sxs-lookup"><span data-stu-id="c7238-153">The agreement settings to be configured to allow leading and trailing space.</span></span> </br> <span data-ttu-id="c7238-154">Upravit nastavení smlouvy, které povolí úvodní a koncové místa</span><span class="sxs-lookup"><span data-stu-id="c7238-154">Edit agreement settings to allow leading and trailing space</span></span> |
|   |   |

![Povolit místa](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-the-agreement"></a><span data-ttu-id="c7238-156">* Duplicitní kontrola povolil smlouvy</span><span class="sxs-lookup"><span data-stu-id="c7238-156">* Duplicate check has enabled in the agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="c7238-157">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-157">Error description</span></span> | <span data-ttu-id="c7238-158">Duplicitní číslo ovládacího prvku</span><span class="sxs-lookup"><span data-stu-id="c7238-158">Duplicate Control Number</span></span> |
| <span data-ttu-id="c7238-159">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-159">User action</span></span> | <span data-ttu-id="c7238-160">Tato chyba znamená, že má duplicitní řízení čísla přijaté zprávy.</span><span class="sxs-lookup"><span data-stu-id="c7238-160">This error indicates the received message has duplicate control numbers.</span></span> </br> <span data-ttu-id="c7238-161">Opravte řízení číslo a opakujte odeslání zprávy</span><span class="sxs-lookup"><span data-stu-id="c7238-161">Correct the control number and resend the message</span></span> |
|   |   |

### <a name="-missing-schema-in-the-agreement"></a><span data-ttu-id="c7238-162">* Chybějící schématu smlouvy</span><span class="sxs-lookup"><span data-stu-id="c7238-162">* Missing schema in the agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="c7238-163">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-163">Error description</span></span> | <span data-ttu-id="c7238-164">Během analýzy došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="c7238-164">Error encountered during parsing.</span></span> <span data-ttu-id="c7238-165">X12 sadu transakce s id '564220001' obsažená v funkční skupiny s id '56422', v výměnu s id '000056422"s id odesílatele ' 12345678', id příjemce ' 87654321' bylo pozastaveno s následujícími chybami" zpráva má typ Neznámý dokumentu ND nebylo možné přeložit do jakéhokoli z existující schémata nakonfigurované smlouvy"</span><span class="sxs-lookup"><span data-stu-id="c7238-165">The X12 transaction set with id '564220001' contained in functional group with id '56422', in interchange with id '000056422', with sender id '12345678       ', receiver id '87654321       ' is being suspended with following errors "The message has an unknown document type and did not resolve to any of the existing schemas configured in the agreement"</span></span> |
| <span data-ttu-id="c7238-166">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-166">User action</span></span> | <span data-ttu-id="c7238-167">Konfigurace schématu v nastavení smlouvy</span><span class="sxs-lookup"><span data-stu-id="c7238-167">Configure schema in the agreement settings</span></span>  |
|   |   |

### <a name="-incorrect-schema-in-the-agreement"></a><span data-ttu-id="c7238-168">* Nesprávné schéma smlouvy</span><span class="sxs-lookup"><span data-stu-id="c7238-168">* Incorrect schema in the agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="c7238-169">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-169">Error description</span></span> | <span data-ttu-id="c7238-170">Zpráva má typ Neznámý dokumentu a nebylo možné přeložit do jakéhokoli z existující schémata nakonfigurované v této smlouvy.</span><span class="sxs-lookup"><span data-stu-id="c7238-170">The message has an unknown document type and did not resolve to any of the existing schemas configured in the agreement.</span></span> |
| <span data-ttu-id="c7238-171">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-171">User action</span></span> | <span data-ttu-id="c7238-172">Konfigurace správné schéma v nastavení smlouvy</span><span class="sxs-lookup"><span data-stu-id="c7238-172">Configure correct schema in the agreement settings</span></span>  |
|   |   |

## <a name="flat-file"></a><span data-ttu-id="c7238-173">Plochý soubor</span><span class="sxs-lookup"><span data-stu-id="c7238-173">Flat file</span></span>

### <a name="-input-message-with-no-body"></a><span data-ttu-id="c7238-174">* Vstupní zprávu s žádný text</span><span class="sxs-lookup"><span data-stu-id="c7238-174">* Input message with no body</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="c7238-175">Popis chyby</span><span class="sxs-lookup"><span data-stu-id="c7238-175">Error description</span></span> | <span data-ttu-id="c7238-176">InvalidTemplate.</span><span class="sxs-lookup"><span data-stu-id="c7238-176">InvalidTemplate.</span></span> <span data-ttu-id="c7238-177">Nelze výrazy jazyka šablony procesů v vstupy 'Flat_File_Decoding' akce v řádku '1' a ve sloupci '1902': ' požadované vlastnosti 'content' očekává hodnotu ale byla null.</span><span class="sxs-lookup"><span data-stu-id="c7238-177">Unable to process template language expressions in action 'Flat_File_Decoding' inputs at line '1' and column '1902': 'Required property 'content' expects a value but got null.</span></span> <span data-ttu-id="c7238-178">Cesta k ".".</span><span class="sxs-lookup"><span data-stu-id="c7238-178">Path ''.'.</span></span> |
| <span data-ttu-id="c7238-179">Akce uživatele</span><span class="sxs-lookup"><span data-stu-id="c7238-179">User action</span></span> | <span data-ttu-id="c7238-180">Tato chyba označuje, že vstupní zprávy neobsahuje text</span><span class="sxs-lookup"><span data-stu-id="c7238-180">This error indicates the input message does not contain a body</span></span> |
|   |   | 

## <a name="learn-more"></a><span data-ttu-id="c7238-181">Další informace</span><span class="sxs-lookup"><span data-stu-id="c7238-181">Learn more</span></span>
[<span data-ttu-id="c7238-182">Další informace o Enterprise integračního balíčku</span><span class="sxs-lookup"><span data-stu-id="c7238-182">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)