---
title: "Přehled zásad SSL pro Azure Application Gateway | Microsoft Docs"
description: "Další informace o Azure Application Gateway umožňuje konfigurovat zásady protokolu SSL"
services: application gateway
documentationcenter: na
author: amsriva
manager: 
editor: 
tags: azure resource manager
ms.service: application gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure services
ms.date: 08/03/2017
ms.author: amsriva
ms.openlocfilehash: 938591bc2422f3137492e86fc60f7d5529596424
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="application-gateway-ssl-policy-overview"></a><span data-ttu-id="5c04a-103">Přehled zásad brány SSL aplikace</span><span class="sxs-lookup"><span data-stu-id="5c04a-103">Application Gateway SSL policy overview</span></span>

<span data-ttu-id="5c04a-104">Aplikační brána vám umožní centralizovat správu certifikátu SSL a snížit režijní náklady na šifrování a dešifrování z back-end serverové farmy.</span><span class="sxs-lookup"><span data-stu-id="5c04a-104">Application Gateway allows you to centralize SSL certificate management and reduce encryption and decryption overhead from a backend server farm.</span></span> <span data-ttu-id="5c04a-105">Tento centrální zpracování SSL můžete také zadat centrální zásady protokolu SSL vhodné pro vaše požadavky na zabezpečení organizace.</span><span class="sxs-lookup"><span data-stu-id="5c04a-105">This centralized SSL handling also allows you to specify a central SSL policy suited to your organizational security requirements.</span></span> <span data-ttu-id="5c04a-106">To pomáhá splnit požadavky na dodržování předpisů jak pokyny pro zabezpečení a doporučené postupy.</span><span class="sxs-lookup"><span data-stu-id="5c04a-106">This helps you meet both compliance requirements as well as security guidelines and recommended practices.</span></span>

<span data-ttu-id="5c04a-107">Zásady protokolu SSL zahrnuje kontrolu nad verzi protokolu SSL a také šifrovacích sad a pořadí, ve kterém šifry se používají během metody handshake pro protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="5c04a-107">SSL policy includes control of both SSL protocol version as well as cipher suites and the order ciphers are used during an SSL handshake.</span></span> <span data-ttu-id="5c04a-108">Směrem tuto bránu aplikace nabízí dva mechanismy umožňující zákazníkům řídit zásady protokolu SSL, i předdefinovanou nebo vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="5c04a-108">Towards this Application Gateway offers two mechanisms to allow customers to control SSL policy, a predefined or a custom policy.</span></span>

## <a name="predefined-ssl-policy"></a><span data-ttu-id="5c04a-109">Předdefinované zásady protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="5c04a-109">Predefined SSL policy</span></span>

<span data-ttu-id="5c04a-110">Aplikační brána obsahuje tři předdefinované zásady zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5c04a-110">Application Gateway has three predefined security policies.</span></span> <span data-ttu-id="5c04a-111">Bránu můžete nakonfigurovat pomocí některé z těchto zásad získat odpovídající úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5c04a-111">You can configure your gateway with any of these policies to get the appropriate level of security.</span></span> <span data-ttu-id="5c04a-112">Názvy zásad jsou opatřeny poznámkami ve za rok a měsíc, ve kterém byly nakonfigurovány.</span><span class="sxs-lookup"><span data-stu-id="5c04a-112">The policy names are annotated by the year and month in which they were configured.</span></span> <span data-ttu-id="5c04a-113">Každé zásady nabízí různé SSL protocol verze a šifrovací sady.</span><span class="sxs-lookup"><span data-stu-id="5c04a-113">Each policy offers different SSL protocol versions and cipher suites.</span></span> <span data-ttu-id="5c04a-114">Doporučujeme použít nejnovější zásady protokolu SSL zajistit nejlepší zabezpečení SSL.</span><span class="sxs-lookup"><span data-stu-id="5c04a-114">It is recommended to use the newest SSL policies to ensure the best SSL security.</span></span> <span data-ttu-id="5c04a-115">Aplikační brána ještě dnes umožňuje uživatelům zvolit jednu z těchto předdefinovaných.</span><span class="sxs-lookup"><span data-stu-id="5c04a-115">Application Gateway today allows for users to choose from one of the following predefined.</span></span>

### <a name="appgwsslpolicy20150501"></a><span data-ttu-id="5c04a-116">AppGwSslPolicy20150501</span><span class="sxs-lookup"><span data-stu-id="5c04a-116">AppGwSslPolicy20150501</span></span>

|<span data-ttu-id="5c04a-117">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5c04a-117">Property</span></span>  |<span data-ttu-id="5c04a-118">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5c04a-118">Value</span></span>  |
|---|---|
|<span data-ttu-id="5c04a-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5c04a-119">Name</span></span>     | <span data-ttu-id="5c04a-120">AppGwSslPolicy20150501</span><span class="sxs-lookup"><span data-stu-id="5c04a-120">AppGwSslPolicy20150501</span></span>        |
|<span data-ttu-id="5c04a-121">MinProtocolVersion</span><span class="sxs-lookup"><span data-stu-id="5c04a-121">MinProtocolVersion</span></span>     | <span data-ttu-id="5c04a-122">TLSv1_0</span><span class="sxs-lookup"><span data-stu-id="5c04a-122">TLSv1_0</span></span>        |
|<span data-ttu-id="5c04a-123">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5c04a-123">Default</span></span>| <span data-ttu-id="5c04a-124">Hodnota TRUE, (Pokud je zadán žádné předdefinované zásady)</span><span class="sxs-lookup"><span data-stu-id="5c04a-124">True (if no predefined policy is specified)</span></span> |
|<span data-ttu-id="5c04a-125">AES</span><span class="sxs-lookup"><span data-stu-id="5c04a-125">CipherSuites</span></span>     |<span data-ttu-id="5c04a-126">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-126">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-127">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-127">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-128">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-128">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span></span><br><span data-ttu-id="5c04a-129">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-129">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-130">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-130">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-131">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-131">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span><br><span data-ttu-id="5c04a-132">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-132">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-133">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-133">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-134">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-134">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-135">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-135">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span></span><br><span data-ttu-id="5c04a-136">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-136">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-137">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-137">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-138">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-138">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-139">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-139">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-140">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-140">TLS_RSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-141">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-141">TLS_RSA_WITH_AES_128_CBC_SHA</span></span><br><span data-ttu-id="5c04a-142">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-142">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-143">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-143">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-144">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-144">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span><br><span data-ttu-id="5c04a-145">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-145">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-146">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-146">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-147">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-147">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span><br><span data-ttu-id="5c04a-148">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-148">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-149">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-149">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-150">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-150">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-151">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-151">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span></span><br><span data-ttu-id="5c04a-152">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-152">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span></span><br><span data-ttu-id="5c04a-153">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-153">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span></span> |
  
  ### <a name="appgwsslpolicy20170401"></a><span data-ttu-id="5c04a-154">AppGwSslPolicy20170401</span><span class="sxs-lookup"><span data-stu-id="5c04a-154">AppGwSslPolicy20170401</span></span>
  
|<span data-ttu-id="5c04a-155">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5c04a-155">Property</span></span>  |<span data-ttu-id="5c04a-156">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5c04a-156">Value</span></span>  |
|   ---      |  ---       |
|<span data-ttu-id="5c04a-157">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5c04a-157">Name</span></span>     | <span data-ttu-id="5c04a-158">AppGwSslPolicy20170401</span><span class="sxs-lookup"><span data-stu-id="5c04a-158">AppGwSslPolicy20170401</span></span>        |
|<span data-ttu-id="5c04a-159">MinProtocolVersion</span><span class="sxs-lookup"><span data-stu-id="5c04a-159">MinProtocolVersion</span></span>     | <span data-ttu-id="5c04a-160">TLSv1_0</span><span class="sxs-lookup"><span data-stu-id="5c04a-160">TLSv1_0</span></span>        |
|<span data-ttu-id="5c04a-161">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5c04a-161">Default</span></span>| <span data-ttu-id="5c04a-162">False</span><span class="sxs-lookup"><span data-stu-id="5c04a-162">False</span></span> |
|<span data-ttu-id="5c04a-163">AES</span><span class="sxs-lookup"><span data-stu-id="5c04a-163">CipherSuites</span></span>     |<span data-ttu-id="5c04a-164">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-164">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-165">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-165">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-166">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-166">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span><br><span data-ttu-id="5c04a-167">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-167">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-168">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-168">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-169">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-169">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span><br><span data-ttu-id="5c04a-170">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-170">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-171">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-171">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-172">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-172">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span><br><span data-ttu-id="5c04a-173">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-173">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-174">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-174">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-175">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-175">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-176">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-176">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-177">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-177">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-178">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-178">TLS_RSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-179">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-179">TLS_RSA_WITH_AES_128_CBC_SHA</span></span> |
  
### <a name="appgwsslpolicy20170401s"></a><span data-ttu-id="5c04a-180">AppGwSslPolicy20170401S</span><span class="sxs-lookup"><span data-stu-id="5c04a-180">AppGwSslPolicy20170401S</span></span>

|<span data-ttu-id="5c04a-181">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5c04a-181">Property</span></span>  |<span data-ttu-id="5c04a-182">Hodnota</span><span class="sxs-lookup"><span data-stu-id="5c04a-182">Value</span></span>  |
|---|---|
|<span data-ttu-id="5c04a-183">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5c04a-183">Name</span></span>     | <span data-ttu-id="5c04a-184">AppGwSslPolicy20170401</span><span class="sxs-lookup"><span data-stu-id="5c04a-184">AppGwSslPolicy20170401</span></span>        |
|<span data-ttu-id="5c04a-185">MinProtocolVersion</span><span class="sxs-lookup"><span data-stu-id="5c04a-185">MinProtocolVersion</span></span>     | <span data-ttu-id="5c04a-186">TLSv1_2</span><span class="sxs-lookup"><span data-stu-id="5c04a-186">TLSv1_2</span></span>        |
|<span data-ttu-id="5c04a-187">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5c04a-187">Default</span></span>| <span data-ttu-id="5c04a-188">False</span><span class="sxs-lookup"><span data-stu-id="5c04a-188">False</span></span> |
|<span data-ttu-id="5c04a-189">AES</span><span class="sxs-lookup"><span data-stu-id="5c04a-189">CipherSuites</span></span>     |<span data-ttu-id="5c04a-190">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-190">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span> <br>    <span data-ttu-id="5c04a-191">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-191">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span> <br>    <span data-ttu-id="5c04a-192">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-192">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span> <br><span data-ttu-id="5c04a-193">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-193">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span> <br><span data-ttu-id="5c04a-194">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-194">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-195">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-195">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span><br><span data-ttu-id="5c04a-196">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-196">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-197">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-197">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-198">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-198">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span><br><span data-ttu-id="5c04a-199">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-199">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-200">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-200">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span><br><span data-ttu-id="5c04a-201">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-201">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span><br><span data-ttu-id="5c04a-202">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-202">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-203">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-203">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span><br><span data-ttu-id="5c04a-204">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-204">TLS_RSA_WITH_AES_256_CBC_SHA</span></span><br><span data-ttu-id="5c04a-205">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-205">TLS_RSA_WITH_AES_128_CBC_SHA</span></span><br> |

## <a name="custom-ssl-policy"></a><span data-ttu-id="5c04a-206">Vlastní zásady protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="5c04a-206">Custom SSL policy</span></span>

<span data-ttu-id="5c04a-207">Pokud předdefinované zásady protokolu SSL je potřeba nakonfigurovat pro vaše požadavky, je třeba musí definovat vlastní vlastní zásady protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="5c04a-207">If a predefined SSL policy needs to be configured for your requirements, you have must define your own custom SSL policy.</span></span> <span data-ttu-id="5c04a-208">V takovém případě máte plnou kontrolu nad minimální verzi protokolu SSL pro podporu a také podporovaných sad šifer a jejich pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="5c04a-208">In this case, you have complete control over the minimum SSL protocol version to support as well as supported cipher suites and their priority order.</span></span>
 
<span data-ttu-id="5c04a-209">Verze protokolu SSL:</span><span class="sxs-lookup"><span data-stu-id="5c04a-209">SSL protocol versions:</span></span>

* <span data-ttu-id="5c04a-210">Protokoly SSL 2.0 a 3.0 jsou ve výchozím nastavení zakázané pro všechny brány Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="5c04a-210">SSL 2.0 and 3.0 disabled by default for all Application Gateways.</span></span> <span data-ttu-id="5c04a-211">Tyto verze protokolu se nedají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="5c04a-211">These protocol versions are not configurable.</span></span>
* <span data-ttu-id="5c04a-212">Vlastní SSL zásady definice poskytuje možnost vyberte jednu z následujících tří protokoly jako minimální verzi protokolu SSL pro bránu TLSv1_0, TLSv1_1, TLSv1_2.</span><span class="sxs-lookup"><span data-stu-id="5c04a-212">Custom SSL policy definition gives you option to select any one of the following three protocols as the minimum SSL protocol version for your gateway   TLSv1_0, TLSv1_1, TLSv1_2.</span></span>
* <span data-ttu-id="5c04a-213">Pokud není nadefinovaná žádná zásada SSL všechny tři (TLSv1_0, TLSv1_1, TLSv1_2) jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="5c04a-213">If no SSL policy is defined all three (TLSv1_0, TLSv1_1, TLSv1_2) are enabled.</span></span>

<span data-ttu-id="5c04a-214">Šifrovací sada:</span><span class="sxs-lookup"><span data-stu-id="5c04a-214">Cipher Suite:</span></span>

<span data-ttu-id="5c04a-215">Aplikační brána podporuje následující šifrovací sady, ze kterých je možné vybrat vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="5c04a-215">Application Gateway supports the following cipher suites from which your custom policy can be chosen.</span></span> <span data-ttu-id="5c04a-216">Při vyjednávání SSL řazení šifrovací sada určuje pořadí podle priority.</span><span class="sxs-lookup"><span data-stu-id="5c04a-216">The ordering of the cipher suites determines the priority order during SSL negotiation.</span></span>

<span data-ttu-id="5c04a-217">K dispozici šifrovací sady:</span><span class="sxs-lookup"><span data-stu-id="5c04a-217">Available Cipher Suites:</span></span>

- <span data-ttu-id="5c04a-218">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-218">TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="5c04a-219">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-219">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="5c04a-220">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-220">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="5c04a-221">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-221">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="5c04a-222">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-222">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-223">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-223">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-224">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-224">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="5c04a-225">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-225">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="5c04a-226">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-226">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-227">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-227">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-228">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-228">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="5c04a-229">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-229">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="5c04a-230">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-230">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="5c04a-231">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-231">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="5c04a-232">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-232">TLS_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-233">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-233">TLS_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-234">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-234">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="5c04a-235">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-235">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="5c04a-236">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="5c04a-236">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="5c04a-237">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-237">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="5c04a-238">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-238">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-239">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-239">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-240">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-240">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="5c04a-241">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="5c04a-241">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="5c04a-242">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-242">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-243">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-243">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-244">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-244">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span></span>
- <span data-ttu-id="5c04a-245">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="5c04a-245">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c04a-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c04a-246">Next steps</span></span>

[<span data-ttu-id="5c04a-247">Konfigurace zásady protokolu SSL na aplikační brány</span><span class="sxs-lookup"><span data-stu-id="5c04a-247">Configure SSL policy on an application gateway</span></span>](application-gateway-configure-ssl-policy-powershell.md)