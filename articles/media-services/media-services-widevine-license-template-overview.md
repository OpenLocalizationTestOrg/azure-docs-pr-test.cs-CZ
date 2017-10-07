---
title: "Přehled šablonu licence aaaWidevine | Microsoft Docs"
description: "Toto téma poskytuje přehled o šablonu licence Widevine, který používá licence na Widevine tooconfigure."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 67a6ae38cf3d3c21e1b7282aef15f79b21776414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="widevine-license-template-overview"></a><span data-ttu-id="7c01c-103">Přehled šablonu licence Widevine</span><span class="sxs-lookup"><span data-stu-id="7c01c-103">Widevine license template overview</span></span>
## <a name="overview"></a><span data-ttu-id="7c01c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7c01c-104">Overview</span></span>
<span data-ttu-id="7c01c-105">Azure Media Services nyní umožňuje licence na Widevine tooconfigure a požadavek.</span><span class="sxs-lookup"><span data-stu-id="7c01c-105">Azure Media Services now enables you tooconfigure and request Widevine licenses.</span></span> <span data-ttu-id="7c01c-106">Když player hello koncový uživatel pokusí tooplay Widevine chráněný obsah, žádost je odeslané toohello licence doručení služby tooobtain licenci.</span><span class="sxs-lookup"><span data-stu-id="7c01c-106">When hello end user player tries tooplay your Widevine protected content, a request is sent toohello license delivery service tooobtain a license.</span></span> <span data-ttu-id="7c01c-107">Pokud hello licenční služby schválí požadavek hello, vystavuje hello licenci, která je klient odeslané toohello a může být použité toodecrypt a play hello zadaný obsah.</span><span class="sxs-lookup"><span data-stu-id="7c01c-107">If hello license service approves hello request, it issues hello license which is sent toohello client and can be used toodecrypt and play hello specified content.</span></span>

<span data-ttu-id="7c01c-108">Požadavek na licenční Widevine je naformátován jako zprávu JSON.</span><span class="sxs-lookup"><span data-stu-id="7c01c-108">Widevine license request is formatted as a JSON message.</span></span>  

>[!NOTE]
> <span data-ttu-id="7c01c-109">Můžete vybrat toocreate zprávu prázdný s žádné hodnoty jenom "{}" a vytvoří se šablona licence s všechny výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7c01c-109">You can choose toocreate an empty message with no values just "{}" and a license template will be created with all defaults.</span></span> <span data-ttu-id="7c01c-110">Výchozí Hello funguje pro většině případů.</span><span class="sxs-lookup"><span data-stu-id="7c01c-110">hello default works for most cases.</span></span> <span data-ttu-id="7c01c-111">Například pro MS na základě scénáře doručení licence, které by měl vždycky být výchozí.</span><span class="sxs-lookup"><span data-stu-id="7c01c-111">For example, for MS based license delivery scenarios that should always be default.</span></span> <span data-ttu-id="7c01c-112">Pokud potřebujete tooset hello "zprostředkovatele" a "content_id" hodnoty, se musí shodovat zprostředkovatele Google Widevine pověření.</span><span class="sxs-lookup"><span data-stu-id="7c01c-112">If you do need tooset hello "provider" and "content_id" values, a provider must match Google's Widevine credentials.</span></span>

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a><span data-ttu-id="7c01c-113">Zpráva JSON</span><span class="sxs-lookup"><span data-stu-id="7c01c-113">JSON message</span></span>
| <span data-ttu-id="7c01c-114">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="7c01c-114">Name</span></span> | <span data-ttu-id="7c01c-115">Hodnota</span><span class="sxs-lookup"><span data-stu-id="7c01c-115">Value</span></span> | <span data-ttu-id="7c01c-116">Popis</span><span class="sxs-lookup"><span data-stu-id="7c01c-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7c01c-117">datová část</span><span class="sxs-lookup"><span data-stu-id="7c01c-117">payload</span></span> |<span data-ttu-id="7c01c-118">Řetězec s kódováním base64</span><span class="sxs-lookup"><span data-stu-id="7c01c-118">Base64 encoded string</span></span> |<span data-ttu-id="7c01c-119">požadavek na licenční Hello odeslány klientem.</span><span class="sxs-lookup"><span data-stu-id="7c01c-119">hello license request sent by a client.</span></span> |
| <span data-ttu-id="7c01c-120">content_id</span><span class="sxs-lookup"><span data-stu-id="7c01c-120">content_id</span></span> |<span data-ttu-id="7c01c-121">Řetězec s kódováním base64</span><span class="sxs-lookup"><span data-stu-id="7c01c-121">Base64 encoded string</span></span> |<span data-ttu-id="7c01c-122">Identifikátor používá tooderive KeyId(s) a obsahu klíče pro každý content_key_specs.track_type.</span><span class="sxs-lookup"><span data-stu-id="7c01c-122">Identifier used tooderive KeyId(s) and Content Key(s) for each content_key_specs.track_type.</span></span> |
| <span data-ttu-id="7c01c-123">Zprostředkovatel</span><span class="sxs-lookup"><span data-stu-id="7c01c-123">provider</span></span> |<span data-ttu-id="7c01c-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7c01c-124">string</span></span> |<span data-ttu-id="7c01c-125">Použít toolook klíčů k obsahu a zásady.</span><span class="sxs-lookup"><span data-stu-id="7c01c-125">Used toolook up content keys and policies.</span></span> <span data-ttu-id="7c01c-126">Pokud MS doručení klíče se používá pro doručování licence Widevine, tento parametr je ignorován.</span><span class="sxs-lookup"><span data-stu-id="7c01c-126">If MS key delivery is used for Widevine license delivery, this parameter is ignored.</span></span> |
| <span data-ttu-id="7c01c-127">název_zásad</span><span class="sxs-lookup"><span data-stu-id="7c01c-127">policy_name</span></span> |<span data-ttu-id="7c01c-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7c01c-128">string</span></span> |<span data-ttu-id="7c01c-129">Název zásady dříve zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="7c01c-129">Name of a previously registered policy.</span></span> <span data-ttu-id="7c01c-130">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7c01c-130">Optional</span></span> |
| <span data-ttu-id="7c01c-131">allowed_track_types</span><span class="sxs-lookup"><span data-stu-id="7c01c-131">allowed_track_types</span></span> |<span data-ttu-id="7c01c-132">výčet</span><span class="sxs-lookup"><span data-stu-id="7c01c-132">enum</span></span> |<span data-ttu-id="7c01c-133">SD_ONLY nebo SD_HD.</span><span class="sxs-lookup"><span data-stu-id="7c01c-133">SD_ONLY or SD_HD.</span></span> <span data-ttu-id="7c01c-134">Ovládací prvky, které obsahu klíče by měl být součástí licenci</span><span class="sxs-lookup"><span data-stu-id="7c01c-134">Controls which content keys should be included in a license</span></span> |
| <span data-ttu-id="7c01c-135">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="7c01c-135">content_key_specs</span></span> |<span data-ttu-id="7c01c-136">pole JSON struktur, najdete v části **obsahu specifikace klíče** níže</span><span class="sxs-lookup"><span data-stu-id="7c01c-136">array of JSON structures, see **Content Key Specs** below</span></span> |<span data-ttu-id="7c01c-137">Přesnější možnosti řízení na který obsah klíče tooreturn.</span><span class="sxs-lookup"><span data-stu-id="7c01c-137">A finer grained control on what content keys tooreturn.</span></span> <span data-ttu-id="7c01c-138">Podrobnosti najdete v obsahu specifikace klíče níže.</span><span class="sxs-lookup"><span data-stu-id="7c01c-138">See Content Key Spec below for details.</span></span>  <span data-ttu-id="7c01c-139">Lze zadat pouze jeden z allowed_track_types a content_key_specs.</span><span class="sxs-lookup"><span data-stu-id="7c01c-139">Only one of allowed_track_types and content_key_specs can be specified.</span></span> |
| <span data-ttu-id="7c01c-140">use_policy_overrides_exclusively</span><span class="sxs-lookup"><span data-stu-id="7c01c-140">use_policy_overrides_exclusively</span></span> |<span data-ttu-id="7c01c-141">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="7c01c-141">boolean.</span></span> <span data-ttu-id="7c01c-142">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="7c01c-142">true or false</span></span> |<span data-ttu-id="7c01c-143">Pomocí určeného policy_overrides atributy zásad a vynechejte všechny dříve uložené zásad.</span><span class="sxs-lookup"><span data-stu-id="7c01c-143">Use policy attributes specified by policy_overrides and omit all previously stored policy.</span></span> |
| <span data-ttu-id="7c01c-144">policy_overrides</span><span class="sxs-lookup"><span data-stu-id="7c01c-144">policy_overrides</span></span> |<span data-ttu-id="7c01c-145">Struktura JSON, najdete v části **zásada přepíše** níže</span><span class="sxs-lookup"><span data-stu-id="7c01c-145">JSON structure, see **Policy Overrides** below</span></span> |<span data-ttu-id="7c01c-146">Nastavení zásad pro tuto licenci.</span><span class="sxs-lookup"><span data-stu-id="7c01c-146">Policy settings for this license.</span></span>  <span data-ttu-id="7c01c-147">V případě hello tento prostředek má předem definované zásady, budou použity tyto zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7c01c-147">In hello event this asset has a pre-defined policy, these specified values will be used.</span></span> |
| <span data-ttu-id="7c01c-148">session_init</span><span class="sxs-lookup"><span data-stu-id="7c01c-148">session_init</span></span> |<span data-ttu-id="7c01c-149">Struktura JSON, najdete v části **inicializace relace** níže</span><span class="sxs-lookup"><span data-stu-id="7c01c-149">JSON structure, see **Session Initialization** below</span></span> |<span data-ttu-id="7c01c-150">Volitelná data předána toolicense.</span><span class="sxs-lookup"><span data-stu-id="7c01c-150">Optional data passed toolicense.</span></span> |
| <span data-ttu-id="7c01c-151">parse_only</span><span class="sxs-lookup"><span data-stu-id="7c01c-151">parse_only</span></span> |<span data-ttu-id="7c01c-152">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="7c01c-152">boolean.</span></span> <span data-ttu-id="7c01c-153">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="7c01c-153">true or false</span></span> |<span data-ttu-id="7c01c-154">požadavek na licenční Hello je analyzována, ale je vydán žádná licence.</span><span class="sxs-lookup"><span data-stu-id="7c01c-154">hello license request is parsed but no license is issued.</span></span> <span data-ttu-id="7c01c-155">Požadavek na licenční hello hodnot formuláře jsou však vráceny v odpovědi hello.</span><span class="sxs-lookup"><span data-stu-id="7c01c-155">However, values form hello license request are returned in hello response.</span></span> |

## <a name="content-key-specs"></a><span data-ttu-id="7c01c-156">Obsahu specifikace klíče</span><span class="sxs-lookup"><span data-stu-id="7c01c-156">Content Key Specs</span></span>
<span data-ttu-id="7c01c-157">Pokud existují předem existující zásady, neexistuje žádný toospecify nutné žádné hello hodnoty v hello obsahu specifikace klíče.  Hello existující zásady přidružené k tomuto obsahu bude ochrany výstup hello použité toodetermine například HDCP a CGMS.</span><span class="sxs-lookup"><span data-stu-id="7c01c-157">If a pre-existing policy exist, there is no need toospecify any of hello values in hello Content Key Spec.  hello pre-existing policy associated with this content will be used toodetermine hello output protection such as HDCP and CGMS.</span></span>  <span data-ttu-id="7c01c-158">Pokud už existující zásady není zaregistrována hello Widevine licenční Server, můžete poskytovateli obsahu hello vložit hello hodnoty do požadavek na licenční hello.</span><span class="sxs-lookup"><span data-stu-id="7c01c-158">If a pre-existing policy is not registered with hello Widevine License Server, hello content provider can inject hello values into hello license request.</span></span>   

<span data-ttu-id="7c01c-159">Každý content_key_specs je třeba zadat pro všechny sleduje, bez ohledu na to možnost use_policy_overrides_exclusively hello.</span><span class="sxs-lookup"><span data-stu-id="7c01c-159">Each content_key_specs must be specified for all tracks, regardless of hello option use_policy_overrides_exclusively.</span></span> 

| <span data-ttu-id="7c01c-160">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="7c01c-160">Name</span></span> | <span data-ttu-id="7c01c-161">Hodnota</span><span class="sxs-lookup"><span data-stu-id="7c01c-161">Value</span></span> | <span data-ttu-id="7c01c-162">Popis</span><span class="sxs-lookup"><span data-stu-id="7c01c-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7c01c-163">content_key_specs.</span><span class="sxs-lookup"><span data-stu-id="7c01c-163">content_key_specs.</span></span> <span data-ttu-id="7c01c-164">track_type</span><span class="sxs-lookup"><span data-stu-id="7c01c-164">track_type</span></span> |<span data-ttu-id="7c01c-165">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7c01c-165">string</span></span> |<span data-ttu-id="7c01c-166">Název typu sledování.</span><span class="sxs-lookup"><span data-stu-id="7c01c-166">A track type name.</span></span> <span data-ttu-id="7c01c-167">Pokud content_key_specs je určený v požadavku hello licence, ujistěte se, že toospecify, které sledují všechny typy explicitně.</span><span class="sxs-lookup"><span data-stu-id="7c01c-167">If content_key_specs is specified in hello license request, make sure toospecify all track types explicitly.</span></span> <span data-ttu-id="7c01c-168">Selhání toodo proto způsobí selhání tooplayback posledních 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="7c01c-168">Failure toodo so will result in failure tooplayback past 10 seconds.</span></span> |
| <span data-ttu-id="7c01c-169">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="7c01c-169">content_key_specs</span></span>  <br/> <span data-ttu-id="7c01c-170">security_level</span><span class="sxs-lookup"><span data-stu-id="7c01c-170">security_level</span></span> |<span data-ttu-id="7c01c-171">UInt32</span><span class="sxs-lookup"><span data-stu-id="7c01c-171">uint32</span></span> |<span data-ttu-id="7c01c-172">Definuje požadavky na klienta odolnosti pro přehrávání.</span><span class="sxs-lookup"><span data-stu-id="7c01c-172">Defines client robustness requirements for playback.</span></span> <br/> <span data-ttu-id="7c01c-173">1 - softwarový whitebox kryptografických je povinný.</span><span class="sxs-lookup"><span data-stu-id="7c01c-173">1 - Software-based whitebox crypto is required.</span></span> <br/> <span data-ttu-id="7c01c-174">2 – software kryptografických a zakódovaná dekodér je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="7c01c-174">2 - Software crypto and an obfuscated decoder is required.</span></span> <br/> <span data-ttu-id="7c01c-175">3 - hello materiálech a kryptografické operace s klíčem, je nutné provést v prostředí zálohovány důvěryhodné spouštěcí hardwaru.</span><span class="sxs-lookup"><span data-stu-id="7c01c-175">3 - hello key material and crypto operations must be performed within a hardware backed trusted execution environment.</span></span> <br/> <span data-ttu-id="7c01c-176">4 - hello kryptografických a dekódování obsahu, je nutné provést v prostředí zálohovány důvěryhodné spouštěcí hardwaru.</span><span class="sxs-lookup"><span data-stu-id="7c01c-176">4 - hello crypto and decoding of content must be performed within a hardware backed trusted execution environment.</span></span>  <br/> <span data-ttu-id="7c01c-177">5 - hello kryptografických, dekódování a všechny zpracování hello média (komprimovaným a nekomprimovaným) musí být zpracován zálohovány důvěryhodné spouštěcí prostředí hardwaru.</span><span class="sxs-lookup"><span data-stu-id="7c01c-177">5 - hello crypto, decoding and all handling of hello media (compressed and uncompressed) must be handled within a hardware backed trusted execution environment.</span></span> |
| <span data-ttu-id="7c01c-178">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="7c01c-178">content_key_specs</span></span> <br/> <span data-ttu-id="7c01c-179">required_output_protection.hDC</span><span class="sxs-lookup"><span data-stu-id="7c01c-179">required_output_protection.hdc</span></span> |<span data-ttu-id="7c01c-180">String – jeden z: HDCP_V2 HDCP_NONE, HDCP_V1,</span><span class="sxs-lookup"><span data-stu-id="7c01c-180">string - one of: HDCP_NONE, HDCP_V1, HDCP_V2</span></span> |<span data-ttu-id="7c01c-181">Označuje, zda je vyžadují HDCP</span><span class="sxs-lookup"><span data-stu-id="7c01c-181">Indicates whether HDCP is require</span></span> |
| <span data-ttu-id="7c01c-182">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="7c01c-182">content_key_specs</span></span> <br/><span data-ttu-id="7c01c-183">key</span><span class="sxs-lookup"><span data-stu-id="7c01c-183">key</span></span> |<span data-ttu-id="7c01c-184">formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="7c01c-184">Base64</span></span> <br/><span data-ttu-id="7c01c-185">řetězec s kódováním</span><span class="sxs-lookup"><span data-stu-id="7c01c-185">encoded string</span></span> |<span data-ttu-id="7c01c-186">Obsahu toouse klíče pro toto sledování. -Li zadána, hello track_type nebo key_id je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="7c01c-186">Content key toouse for this track. If specified, hello track_type or key_id is required.</span></span>  <span data-ttu-id="7c01c-187">Tato možnost umožňuje poskytovateli obsahu hello tooinject klíč obsahu hello tento stopy místo Widevine licenční server generovat nebo vyhledat klíč.</span><span class="sxs-lookup"><span data-stu-id="7c01c-187">This option allows hello content provider tooinject hello content key for this track instead of letting Widevine license server generate or lookup a key.</span></span> |
| <span data-ttu-id="7c01c-188">content_key_specs.key_id</span><span class="sxs-lookup"><span data-stu-id="7c01c-188">content_key_specs.key_id</span></span> |<span data-ttu-id="7c01c-189">Kódováním base64, pomocí řetězce binární, 16 bajtů</span><span class="sxs-lookup"><span data-stu-id="7c01c-189">Base64 encoded string  binary, 16 bytes</span></span> |<span data-ttu-id="7c01c-190">Jedinečný identifikátor pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="7c01c-190">Unique identifier for hello key.</span></span> |

## <a name="policy-overrides"></a><span data-ttu-id="7c01c-191">Přepsání zásad</span><span class="sxs-lookup"><span data-stu-id="7c01c-191">Policy Overrides</span></span>
| <span data-ttu-id="7c01c-192">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="7c01c-192">Name</span></span> | <span data-ttu-id="7c01c-193">Hodnota</span><span class="sxs-lookup"><span data-stu-id="7c01c-193">Value</span></span> | <span data-ttu-id="7c01c-194">Popis</span><span class="sxs-lookup"><span data-stu-id="7c01c-194">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7c01c-195">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-195">policy_overrides.</span></span> <span data-ttu-id="7c01c-196">can_play</span><span class="sxs-lookup"><span data-stu-id="7c01c-196">can_play</span></span> |<span data-ttu-id="7c01c-197">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="7c01c-197">boolean.</span></span> <span data-ttu-id="7c01c-198">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="7c01c-198">true or false</span></span> |<span data-ttu-id="7c01c-199">Určuje, že přehrávání obsahu hello je povolený.</span><span class="sxs-lookup"><span data-stu-id="7c01c-199">Indicates that playback of hello content is allowed.</span></span> <span data-ttu-id="7c01c-200">Výchozí hodnota je false.</span><span class="sxs-lookup"><span data-stu-id="7c01c-200">Default is false.</span></span> |
| <span data-ttu-id="7c01c-201">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-201">policy_overrides.</span></span> <span data-ttu-id="7c01c-202">can_persist</span><span class="sxs-lookup"><span data-stu-id="7c01c-202">can_persist</span></span> |<span data-ttu-id="7c01c-203">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="7c01c-203">boolean.</span></span> <span data-ttu-id="7c01c-204">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="7c01c-204">true or false</span></span> |<span data-ttu-id="7c01c-205">Označuje, že licence hello může být trvalý toonon volatile úložiště pro použití v offline režimu.</span><span class="sxs-lookup"><span data-stu-id="7c01c-205">Indicates that hello license may be persisted toonon-volatile storage for offline use.</span></span> <span data-ttu-id="7c01c-206">Výchozí hodnota je false.</span><span class="sxs-lookup"><span data-stu-id="7c01c-206">Default is false.</span></span> |
| <span data-ttu-id="7c01c-207">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-207">policy_overrides.</span></span> <span data-ttu-id="7c01c-208">can_renew</span><span class="sxs-lookup"><span data-stu-id="7c01c-208">can_renew</span></span> |<span data-ttu-id="7c01c-209">Logická hodnota PRAVDA nebo NEPRAVDA</span><span class="sxs-lookup"><span data-stu-id="7c01c-209">boolean true or false</span></span> |<span data-ttu-id="7c01c-210">Označuje, zda je povoleno obnovení tuto licenci.</span><span class="sxs-lookup"><span data-stu-id="7c01c-210">Indicates that renewal of this license is allowed.</span></span> <span data-ttu-id="7c01c-211">V případě hodnoty true hello období platnosti licence hello můžete rozšířit pomocí prezenčního signálu.</span><span class="sxs-lookup"><span data-stu-id="7c01c-211">If true, hello duration of hello license can be extended by heartbeat.</span></span> <span data-ttu-id="7c01c-212">Výchozí hodnota je false.</span><span class="sxs-lookup"><span data-stu-id="7c01c-212">Default is false.</span></span> |
| <span data-ttu-id="7c01c-213">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-213">policy_overrides.</span></span> <span data-ttu-id="7c01c-214">license_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="7c01c-214">license_duration_seconds</span></span> |<span data-ttu-id="7c01c-215">Int64</span><span class="sxs-lookup"><span data-stu-id="7c01c-215">int64</span></span> |<span data-ttu-id="7c01c-216">Označuje hello časový interval pro tuto konkrétní licenci.</span><span class="sxs-lookup"><span data-stu-id="7c01c-216">Indicates hello time window for this specific license.</span></span> <span data-ttu-id="7c01c-217">Hodnota 0 značí, že neexistuje žádné omezení toohello trvání.</span><span class="sxs-lookup"><span data-stu-id="7c01c-217">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="7c01c-218">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="7c01c-218">Default is 0.</span></span> |
| <span data-ttu-id="7c01c-219">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-219">policy_overrides.</span></span> <span data-ttu-id="7c01c-220">rental_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="7c01c-220">rental_duration_seconds</span></span> |<span data-ttu-id="7c01c-221">Int64</span><span class="sxs-lookup"><span data-stu-id="7c01c-221">int64</span></span> |<span data-ttu-id="7c01c-222">Označuje hello časové okno při přehrávání je povoleno.</span><span class="sxs-lookup"><span data-stu-id="7c01c-222">Indicates hello time window while playback is permitted.</span></span> <span data-ttu-id="7c01c-223">Hodnota 0 značí, že neexistuje žádné omezení toohello trvání.</span><span class="sxs-lookup"><span data-stu-id="7c01c-223">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="7c01c-224">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="7c01c-224">Default is 0.</span></span> |
| <span data-ttu-id="7c01c-225">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-225">policy_overrides.</span></span> <span data-ttu-id="7c01c-226">playback_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="7c01c-226">playback_duration_seconds</span></span> |<span data-ttu-id="7c01c-227">Int64</span><span class="sxs-lookup"><span data-stu-id="7c01c-227">int64</span></span> |<span data-ttu-id="7c01c-228">zobrazení okna času po zahájení v rámci licence trvání hello Hello.</span><span class="sxs-lookup"><span data-stu-id="7c01c-228">hello viewing window of time once playback starts within hello license duration.</span></span> <span data-ttu-id="7c01c-229">Hodnota 0 značí, že neexistuje žádné omezení toohello trvání.</span><span class="sxs-lookup"><span data-stu-id="7c01c-229">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="7c01c-230">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="7c01c-230">Default is 0.</span></span> |
| <span data-ttu-id="7c01c-231">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-231">policy_overrides.</span></span> <span data-ttu-id="7c01c-232">renewal_server_url</span><span class="sxs-lookup"><span data-stu-id="7c01c-232">renewal_server_url</span></span> |<span data-ttu-id="7c01c-233">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7c01c-233">string</span></span> |<span data-ttu-id="7c01c-234">Všechny požadavky prezenčního signálu (obnovení) pro tuto licenci musí směřovat toohello zadaná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="7c01c-234">All heartbeat (renewal) requests for this license shall be directed toohello specified URL.</span></span> <span data-ttu-id="7c01c-235">Toto pole je použít jen v případě can_renew má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7c01c-235">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="7c01c-236">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-236">policy_overrides.</span></span> <span data-ttu-id="7c01c-237">renewal_delay_seconds</span><span class="sxs-lookup"><span data-stu-id="7c01c-237">renewal_delay_seconds</span></span> |<span data-ttu-id="7c01c-238">Int64</span><span class="sxs-lookup"><span data-stu-id="7c01c-238">int64</span></span> |<span data-ttu-id="7c01c-239">Kolik sekund po license_start_time, než je první pokus o obnovení.</span><span class="sxs-lookup"><span data-stu-id="7c01c-239">How many seconds after license_start_time, before renewal is first attempted.</span></span> <span data-ttu-id="7c01c-240">Toto pole je použít jen v případě can_renew má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7c01c-240">This field is only used if can_renew is true.</span></span> <span data-ttu-id="7c01c-241">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="7c01c-241">Default is 0</span></span> |
| <span data-ttu-id="7c01c-242">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-242">policy_overrides.</span></span> <span data-ttu-id="7c01c-243">renewal_retry_interval_seconds</span><span class="sxs-lookup"><span data-stu-id="7c01c-243">renewal_retry_interval_seconds</span></span> |<span data-ttu-id="7c01c-244">Int64</span><span class="sxs-lookup"><span data-stu-id="7c01c-244">int64</span></span> |<span data-ttu-id="7c01c-245">Určuje hello zpoždění v sekundách mezi požadavky na obnovení certifikátu další licence, v případě selhání.</span><span class="sxs-lookup"><span data-stu-id="7c01c-245">Specifies hello delay in seconds between subsequent license renewal requests, in case of failure.</span></span> <span data-ttu-id="7c01c-246">Toto pole je použít jen v případě can_renew má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7c01c-246">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="7c01c-247">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-247">policy_overrides.</span></span> <span data-ttu-id="7c01c-248">renewal_recovery_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="7c01c-248">renewal_recovery_duration_seconds</span></span> |<span data-ttu-id="7c01c-249">Int64</span><span class="sxs-lookup"><span data-stu-id="7c01c-249">int64</span></span> |<span data-ttu-id="7c01c-250">okno Hello dobu, ve kterém je povolen přehrávání toocontinue při obnovení je pokus o, ještě neúspěšné kvůli toobackend problémy s hello licenční server.</span><span class="sxs-lookup"><span data-stu-id="7c01c-250">hello window of time, in which playback is allowed toocontinue while renewal is attempted, yet unsuccessful due toobackend problems with hello license server.</span></span> <span data-ttu-id="7c01c-251">Hodnota 0 značí, že neexistuje žádné omezení toohello trvání.</span><span class="sxs-lookup"><span data-stu-id="7c01c-251">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="7c01c-252">Toto pole je použít jen v případě can_renew má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7c01c-252">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="7c01c-253">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="7c01c-253">policy_overrides.</span></span> <span data-ttu-id="7c01c-254">renew_with_usage</span><span class="sxs-lookup"><span data-stu-id="7c01c-254">renew_with_usage</span></span> |<span data-ttu-id="7c01c-255">Logická hodnota PRAVDA nebo NEPRAVDA</span><span class="sxs-lookup"><span data-stu-id="7c01c-255">boolean true or false</span></span> |<span data-ttu-id="7c01c-256">Označuje, že se zasílá hello licencích pro obnovení, při spuštění využití.</span><span class="sxs-lookup"><span data-stu-id="7c01c-256">Indicates that hello license shall be sent for renewal when usage is started.</span></span> <span data-ttu-id="7c01c-257">Toto pole je použít jen v případě can_renew má hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7c01c-257">This field is only used if can_renew is true.</span></span> |

## <a name="session-initialization"></a><span data-ttu-id="7c01c-258">Inicializace relace</span><span class="sxs-lookup"><span data-stu-id="7c01c-258">Session Initialization</span></span>
| <span data-ttu-id="7c01c-259">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="7c01c-259">Name</span></span> | <span data-ttu-id="7c01c-260">Hodnota</span><span class="sxs-lookup"><span data-stu-id="7c01c-260">Value</span></span> | <span data-ttu-id="7c01c-261">Popis</span><span class="sxs-lookup"><span data-stu-id="7c01c-261">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7c01c-262">provider_session_token</span><span class="sxs-lookup"><span data-stu-id="7c01c-262">provider_session_token</span></span> |<span data-ttu-id="7c01c-263">Řetězec s kódováním base64</span><span class="sxs-lookup"><span data-stu-id="7c01c-263">Base64 encoded string</span></span> |<span data-ttu-id="7c01c-264">Tento token relace je předán zpět v hello licenci a bude existovat v dalších prodloužení.</span><span class="sxs-lookup"><span data-stu-id="7c01c-264">This session token is passed back in hello license and will exist in subsequent renewals.</span></span>  <span data-ttu-id="7c01c-265">Hello relace tokenu neuchovává kromě relací.</span><span class="sxs-lookup"><span data-stu-id="7c01c-265">hello session token will not persist beyond sessions.</span></span> |
| <span data-ttu-id="7c01c-266">provider_client_token</span><span class="sxs-lookup"><span data-stu-id="7c01c-266">provider_client_token</span></span> |<span data-ttu-id="7c01c-267">Řetězec s kódováním base64</span><span class="sxs-lookup"><span data-stu-id="7c01c-267">Base64 encoded string</span></span> |<span data-ttu-id="7c01c-268">Klient tokenu toosend zpět v odpovědi licence hello.</span><span class="sxs-lookup"><span data-stu-id="7c01c-268">Client token toosend back in hello license response.</span></span>  <span data-ttu-id="7c01c-269">Pokud požadavek na licenční hello obsahuje token klienta, je tato hodnota ignorována.</span><span class="sxs-lookup"><span data-stu-id="7c01c-269">If hello license request contains a client token, this value is ignored.</span></span> <span data-ttu-id="7c01c-270">token klienta Hello se uchová nad rámec relace licence.</span><span class="sxs-lookup"><span data-stu-id="7c01c-270">hello client token will persist beyond license sessions.</span></span> |
| <span data-ttu-id="7c01c-271">override_provider_client_token</span><span class="sxs-lookup"><span data-stu-id="7c01c-271">override_provider_client_token</span></span> |<span data-ttu-id="7c01c-272">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="7c01c-272">boolean.</span></span> <span data-ttu-id="7c01c-273">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="7c01c-273">true or false</span></span> |<span data-ttu-id="7c01c-274">Pokud požadavek na licenční hodnotu false a hello obsahuje token klienta, použijte hello tokenu z požadavku hello i v případě, že token klienta byla zadána v této struktuře.</span><span class="sxs-lookup"><span data-stu-id="7c01c-274">If false and hello license request contains a client token, use hello token from hello request even if a client token was specified in this structure.</span></span>  <span data-ttu-id="7c01c-275">V případě hodnoty true vždy používejte token hello zadané v této struktuře.</span><span class="sxs-lookup"><span data-stu-id="7c01c-275">If true, always use hello token specified in this structure.</span></span> |

## <a name="configure-your-widevine-licenses-using-net-types"></a><span data-ttu-id="7c01c-276">Nakonfigurujte své licence Widevine pomocí typy .NET</span><span class="sxs-lookup"><span data-stu-id="7c01c-276">Configure your Widevine licenses using .NET types</span></span>
<span data-ttu-id="7c01c-277">Služba Media Services poskytuje rozhraní API .NET, která umožňují nakonfigurujte své licence Widevine.</span><span class="sxs-lookup"><span data-stu-id="7c01c-277">Media Services provides .NET APIs that let you configure your Widevine licenses.</span></span> 

### <a name="classes-as-defined-in-hello-media-services-net-sdk"></a><span data-ttu-id="7c01c-278">Třídy definované v hello sady Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="7c01c-278">Classes as defined in hello Media Services .NET SDK</span></span>
<span data-ttu-id="7c01c-279">Hello následují definice hello z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="7c01c-279">hello following are hello definitions of these types.</span></span>

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a><span data-ttu-id="7c01c-280">Příklad</span><span class="sxs-lookup"><span data-stu-id="7c01c-280">Example</span></span>
<span data-ttu-id="7c01c-281">Následující příklad ukazuje, jak Hello toouse rozhraní API technologie .NET tooconfigure jednoduché licenci Widevine.</span><span class="sxs-lookup"><span data-stu-id="7c01c-281">hello following example shows how toouse .NET APIs tooconfigure  a simple Widevine license.</span></span>

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="7c01c-282">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="7c01c-282">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7c01c-283">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="7c01c-283">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="7c01c-284">Viz také</span><span class="sxs-lookup"><span data-stu-id="7c01c-284">See also</span></span>
[<span data-ttu-id="7c01c-285">Použití běžného šifrování PlayReady nebo Widevine dynamický</span><span class="sxs-lookup"><span data-stu-id="7c01c-285">Using PlayReady and/or Widevine Dynamic Common Encryption</span></span>](media-services-protect-with-drm.md)

