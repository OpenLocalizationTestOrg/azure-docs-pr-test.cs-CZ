---
title: "Schémata sledování AS2 pro monitorování B2B - Azure Logic Apps | Microsoft Docs"
description: "Ke sledování zpráv B2B z transakcí v účtu Azure integrace použijte schémata sledování AS2."
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 31bd296dc5ed5ac6998a6c05ee80fd38b12d662c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="start-or-enable-tracking-of-as2-messages-and-mdns-to-monitor-success-errors-and-message-properties"></a><span data-ttu-id="ce8da-103">Spuštění nebo povolení sledování AS2 zpráv a MDNs k úspěchu monitorování, chyb a vlastnosti zprávy</span><span class="sxs-lookup"><span data-stu-id="ce8da-103">Start or enable tracking of AS2 messages and MDNs to monitor success, errors, and message properties</span></span>
<span data-ttu-id="ce8da-104">Tato schémata sledování AS2 můžete ve vašem účtu integrace se službou Azure vám pomohou monitorovat business-to-business transakce (B2B):</span><span class="sxs-lookup"><span data-stu-id="ce8da-104">You can use these AS2 tracking schemas in your Azure integration account to help you monitor business-to-business (B2B) transactions:</span></span>

* <span data-ttu-id="ce8da-105">Schéma sledování AS2 zpráv</span><span class="sxs-lookup"><span data-stu-id="ce8da-105">AS2 message tracking schema</span></span>
* <span data-ttu-id="ce8da-106">Schéma sledování AS2 MDN</span><span class="sxs-lookup"><span data-stu-id="ce8da-106">AS2 MDN tracking schema</span></span>

## <a name="as2-message-tracking-schema"></a><span data-ttu-id="ce8da-107">Schéma sledování AS2 zpráv</span><span class="sxs-lookup"><span data-stu-id="ce8da-107">AS2 message tracking schema</span></span>
````java

    {
       "agreementProperties": {  
            "senderPartnerName": "",  
            "receiverPartnerName": "",  
            "as2To": "",  
            "as2From": "",  
            "agreementName": ""  
        },  
        "messageProperties": {
            "direction": "",
            "messageId": "",
            "dispositionType": "",
            "fileName": "",
            "isMessageFailed": "",
            "isMessageSigned": "",
            "isMessageEncrypted": "",
            "isMessageCompressed": "",
            "correlationMessageId": "",
            "incomingHeaders": {
            },
            "outgoingHeaders": {
            },
        "isNrrEnabled": "",
        "isMdnExpected": "",
        "mdnType": ""
        }
    }
````

| <span data-ttu-id="ce8da-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ce8da-108">Property</span></span> | <span data-ttu-id="ce8da-109">Typ</span><span class="sxs-lookup"><span data-stu-id="ce8da-109">Type</span></span> | <span data-ttu-id="ce8da-110">Popis</span><span class="sxs-lookup"><span data-stu-id="ce8da-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce8da-111">senderPartnerName</span><span class="sxs-lookup"><span data-stu-id="ce8da-111">senderPartnerName</span></span> | <span data-ttu-id="ce8da-112">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-112">String</span></span> | <span data-ttu-id="ce8da-113">Název odesílatele zprávy AS2 partnera.</span><span class="sxs-lookup"><span data-stu-id="ce8da-113">AS2 message sender's partner name.</span></span> <span data-ttu-id="ce8da-114">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-114">(Optional)</span></span> |
| <span data-ttu-id="ce8da-115">receiverPartnerName</span><span class="sxs-lookup"><span data-stu-id="ce8da-115">receiverPartnerName</span></span> | <span data-ttu-id="ce8da-116">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-116">String</span></span> | <span data-ttu-id="ce8da-117">Příjemce zprávu AS2 název partnera.</span><span class="sxs-lookup"><span data-stu-id="ce8da-117">AS2 message receiver's partner name.</span></span> <span data-ttu-id="ce8da-118">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-118">(Optional)</span></span> |
| <span data-ttu-id="ce8da-119">as2To</span><span class="sxs-lookup"><span data-stu-id="ce8da-119">as2To</span></span> | <span data-ttu-id="ce8da-120">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-120">String</span></span> | <span data-ttu-id="ce8da-121">Příjemce zprávu AS2 název ze záhlaví zprávy AS2.</span><span class="sxs-lookup"><span data-stu-id="ce8da-121">AS2 message receiver’s name, from the headers of the AS2 message.</span></span> <span data-ttu-id="ce8da-122">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-122">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-123">as2From</span><span class="sxs-lookup"><span data-stu-id="ce8da-123">as2From</span></span> | <span data-ttu-id="ce8da-124">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-124">String</span></span> | <span data-ttu-id="ce8da-125">Název odesílatele zprávy AS2 z hlavičky AS2 zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-125">AS2 message sender’s name, from the headers of the AS2 message.</span></span> <span data-ttu-id="ce8da-126">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-126">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-127">agreementName</span><span class="sxs-lookup"><span data-stu-id="ce8da-127">agreementName</span></span> | <span data-ttu-id="ce8da-128">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-128">String</span></span> | <span data-ttu-id="ce8da-129">Název smlouvy AS2, ke kterému jsou vyřešeny zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-129">Name of the AS2 agreement to which the messages are resolved.</span></span> <span data-ttu-id="ce8da-130">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-130">(Optional)</span></span> |
| <span data-ttu-id="ce8da-131">Směr</span><span class="sxs-lookup"><span data-stu-id="ce8da-131">direction</span></span> | <span data-ttu-id="ce8da-132">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-132">String</span></span> | <span data-ttu-id="ce8da-133">Směr toku zpráv přijímat nebo odesílat.</span><span class="sxs-lookup"><span data-stu-id="ce8da-133">Direction of the message flow, receive or send.</span></span> <span data-ttu-id="ce8da-134">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-134">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-135">messageId</span><span class="sxs-lookup"><span data-stu-id="ce8da-135">messageId</span></span> | <span data-ttu-id="ce8da-136">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-136">String</span></span> | <span data-ttu-id="ce8da-137">ID zprávy AS2, z hlavičky AS2 zprávy (volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-137">AS2 message ID, from the headers of the AS2 message (Optional)</span></span> |
| <span data-ttu-id="ce8da-138">dispositionType</span><span class="sxs-lookup"><span data-stu-id="ce8da-138">dispositionType</span></span> |<span data-ttu-id="ce8da-139">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-139">String</span></span> | <span data-ttu-id="ce8da-140">Hodnota typu dispozice zpráva dispozice oznámení (MDN).</span><span class="sxs-lookup"><span data-stu-id="ce8da-140">Message Disposition Notification (MDN) disposition type value.</span></span> <span data-ttu-id="ce8da-141">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-141">(Optional)</span></span> |
| <span data-ttu-id="ce8da-142">fileName</span><span class="sxs-lookup"><span data-stu-id="ce8da-142">fileName</span></span> | <span data-ttu-id="ce8da-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-143">String</span></span> | <span data-ttu-id="ce8da-144">Název souboru z hlavičky AS2 zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-144">File name, from the header of the AS2 message.</span></span> <span data-ttu-id="ce8da-145">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-145">(Optional)</span></span> |
| <span data-ttu-id="ce8da-146">isMessageFailed</span><span class="sxs-lookup"><span data-stu-id="ce8da-146">isMessageFailed</span></span> |<span data-ttu-id="ce8da-147">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-147">Boolean</span></span> | <span data-ttu-id="ce8da-148">Jestli AS2 se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="ce8da-148">Whether the AS2 message failed.</span></span> <span data-ttu-id="ce8da-149">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-149">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-150">isMessageSigned</span><span class="sxs-lookup"><span data-stu-id="ce8da-150">isMessageSigned</span></span> | <span data-ttu-id="ce8da-151">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-151">Boolean</span></span> | <span data-ttu-id="ce8da-152">Jestli byla podepsána zpráva AS2.</span><span class="sxs-lookup"><span data-stu-id="ce8da-152">Whether the AS2 message was signed.</span></span> <span data-ttu-id="ce8da-153">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-153">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-154">isMessageEncrypted</span><span class="sxs-lookup"><span data-stu-id="ce8da-154">isMessageEncrypted</span></span> | <span data-ttu-id="ce8da-155">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-155">Boolean</span></span> | <span data-ttu-id="ce8da-156">Zda byla zpráva AS2 šifrována.</span><span class="sxs-lookup"><span data-stu-id="ce8da-156">Whether the AS2 message was encrypted.</span></span> <span data-ttu-id="ce8da-157">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-157">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-158">isMessageCompressed</span><span class="sxs-lookup"><span data-stu-id="ce8da-158">isMessageCompressed</span></span> |<span data-ttu-id="ce8da-159">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-159">Boolean</span></span> | <span data-ttu-id="ce8da-160">Jestli byla komprimována zpráva AS2.</span><span class="sxs-lookup"><span data-stu-id="ce8da-160">Whether the AS2 message was compressed.</span></span> <span data-ttu-id="ce8da-161">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-161">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-162">correlationMessageId</span><span class="sxs-lookup"><span data-stu-id="ce8da-162">correlationMessageId</span></span> | <span data-ttu-id="ce8da-163">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-163">String</span></span> | <span data-ttu-id="ce8da-164">ID zprávy AS2 ke korelaci zprávy s MDNs.</span><span class="sxs-lookup"><span data-stu-id="ce8da-164">AS2 message ID, to correlate messages with MDNs.</span></span> <span data-ttu-id="ce8da-165">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-165">(Optional)</span></span> |
| <span data-ttu-id="ce8da-166">incomingHeaders</span><span class="sxs-lookup"><span data-stu-id="ce8da-166">incomingHeaders</span></span> |<span data-ttu-id="ce8da-167">Slovník JToken</span><span class="sxs-lookup"><span data-stu-id="ce8da-167">Dictionary of JToken</span></span> | <span data-ttu-id="ce8da-168">Podrobnosti záhlaví zprávy příchozí AS2.</span><span class="sxs-lookup"><span data-stu-id="ce8da-168">Incoming AS2 message header details.</span></span> <span data-ttu-id="ce8da-169">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-169">(Optional)</span></span> |
| <span data-ttu-id="ce8da-170">outgoingHeaders</span><span class="sxs-lookup"><span data-stu-id="ce8da-170">outgoingHeaders</span></span> |<span data-ttu-id="ce8da-171">Slovník JToken</span><span class="sxs-lookup"><span data-stu-id="ce8da-171">Dictionary of JToken</span></span> | <span data-ttu-id="ce8da-172">Odchozí AS2 podrobnosti záhlaví zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-172">Outgoing AS2 message header details.</span></span> <span data-ttu-id="ce8da-173">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-173">(Optional)</span></span> |
| <span data-ttu-id="ce8da-174">isNrrEnabled</span><span class="sxs-lookup"><span data-stu-id="ce8da-174">isNrrEnabled</span></span> | <span data-ttu-id="ce8da-175">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-175">Boolean</span></span> | <span data-ttu-id="ce8da-176">Použijte výchozí hodnotu, pokud hodnota není znám.</span><span class="sxs-lookup"><span data-stu-id="ce8da-176">Use default value if the value is not known.</span></span> <span data-ttu-id="ce8da-177">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-177">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-178">isMdnExpected</span><span class="sxs-lookup"><span data-stu-id="ce8da-178">isMdnExpected</span></span> | <span data-ttu-id="ce8da-179">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-179">Boolean</span></span> | <span data-ttu-id="ce8da-180">Použijte výchozí hodnotu, pokud hodnota není znám.</span><span class="sxs-lookup"><span data-stu-id="ce8da-180">Use default value if the value is not known.</span></span> <span data-ttu-id="ce8da-181">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-181">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-182">mdnType</span><span class="sxs-lookup"><span data-stu-id="ce8da-182">mdnType</span></span> | <span data-ttu-id="ce8da-183">výčet</span><span class="sxs-lookup"><span data-stu-id="ce8da-183">Enum</span></span> | <span data-ttu-id="ce8da-184">Povolené hodnoty jsou **NotConfigured**, **synchronizace**, a **asynchronní**.</span><span class="sxs-lookup"><span data-stu-id="ce8da-184">Allowed values are **NotConfigured**, **Sync**, and **Async**.</span></span> <span data-ttu-id="ce8da-185">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-185">(Mandatory)</span></span> |

## <a name="as2-mdn-tracking-schema"></a><span data-ttu-id="ce8da-186">Schéma sledování AS2 MDN</span><span class="sxs-lookup"><span data-stu-id="ce8da-186">AS2 MDN tracking schema</span></span>
````java

    {
        "agreementProperties": {
                "senderPartnerName": "",
                "receiverPartnerName": "",
                "as2To": "",
                "as2From": "",
                "agreementName": "g"
            },
            "messageProperties": {
                "direction": "",
                "messageId": "",
                "originalMessageId": "",
                "dispositionType": "",
                "isMessageFailed": "",
                "isMessageSigned": "",
                "isNrrEnabled": "",
                "statusCode": "",
                "micVerificationStatus": "",
                "correlationMessageId": "",
                "incomingHeaders": {
                },
                "outgoingHeaders": {
                }
            }
    }
````

| <span data-ttu-id="ce8da-187">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ce8da-187">Property</span></span> | <span data-ttu-id="ce8da-188">Typ</span><span class="sxs-lookup"><span data-stu-id="ce8da-188">Type</span></span> | <span data-ttu-id="ce8da-189">Popis</span><span class="sxs-lookup"><span data-stu-id="ce8da-189">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce8da-190">senderPartnerName</span><span class="sxs-lookup"><span data-stu-id="ce8da-190">senderPartnerName</span></span> | <span data-ttu-id="ce8da-191">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-191">String</span></span> | <span data-ttu-id="ce8da-192">Název odesílatele zprávy AS2 partnera.</span><span class="sxs-lookup"><span data-stu-id="ce8da-192">AS2 message sender's partner name.</span></span> <span data-ttu-id="ce8da-193">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-193">(Optional)</span></span> |
| <span data-ttu-id="ce8da-194">receiverPartnerName</span><span class="sxs-lookup"><span data-stu-id="ce8da-194">receiverPartnerName</span></span> | <span data-ttu-id="ce8da-195">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-195">String</span></span> | <span data-ttu-id="ce8da-196">Příjemce zprávu AS2 název partnera.</span><span class="sxs-lookup"><span data-stu-id="ce8da-196">AS2 message receiver's partner name.</span></span> <span data-ttu-id="ce8da-197">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-197">(Optional)</span></span> |
| <span data-ttu-id="ce8da-198">as2To</span><span class="sxs-lookup"><span data-stu-id="ce8da-198">as2To</span></span> | <span data-ttu-id="ce8da-199">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-199">String</span></span> | <span data-ttu-id="ce8da-200">Název partnera, který obdrží zprávu AS2.</span><span class="sxs-lookup"><span data-stu-id="ce8da-200">Partner name who receives the AS2 message.</span></span> <span data-ttu-id="ce8da-201">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-201">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-202">as2From</span><span class="sxs-lookup"><span data-stu-id="ce8da-202">as2From</span></span> | <span data-ttu-id="ce8da-203">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-203">String</span></span> | <span data-ttu-id="ce8da-204">Název partnera, který odešle zprávu AS2.</span><span class="sxs-lookup"><span data-stu-id="ce8da-204">Partner name who sends the AS2 message.</span></span> <span data-ttu-id="ce8da-205">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-205">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-206">agreementName</span><span class="sxs-lookup"><span data-stu-id="ce8da-206">agreementName</span></span> | <span data-ttu-id="ce8da-207">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-207">String</span></span> | <span data-ttu-id="ce8da-208">Název smlouvy AS2, ke kterému jsou vyřešeny zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-208">Name of the AS2 agreement to which the messages are resolved.</span></span> <span data-ttu-id="ce8da-209">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-209">(Optional)</span></span> |
| <span data-ttu-id="ce8da-210">Směr</span><span class="sxs-lookup"><span data-stu-id="ce8da-210">direction</span></span> |<span data-ttu-id="ce8da-211">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-211">String</span></span> | <span data-ttu-id="ce8da-212">Směr toku zpráv přijímat nebo odesílat.</span><span class="sxs-lookup"><span data-stu-id="ce8da-212">Direction of the message flow, receive or send.</span></span> <span data-ttu-id="ce8da-213">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-213">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-214">messageId</span><span class="sxs-lookup"><span data-stu-id="ce8da-214">messageId</span></span> | <span data-ttu-id="ce8da-215">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-215">String</span></span> | <span data-ttu-id="ce8da-216">ID AS2 zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-216">AS2 message ID.</span></span> <span data-ttu-id="ce8da-217">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-217">(Optional)</span></span> |
| <span data-ttu-id="ce8da-218">OriginalMessageId</span><span class="sxs-lookup"><span data-stu-id="ce8da-218">originalMessageId</span></span> |<span data-ttu-id="ce8da-219">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-219">String</span></span> | <span data-ttu-id="ce8da-220">ID AS2 původní zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-220">AS2 original message ID.</span></span> <span data-ttu-id="ce8da-221">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-221">(Optional)</span></span> |
| <span data-ttu-id="ce8da-222">dispositionType</span><span class="sxs-lookup"><span data-stu-id="ce8da-222">dispositionType</span></span> | <span data-ttu-id="ce8da-223">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-223">String</span></span> | <span data-ttu-id="ce8da-224">Hodnota typu MDN dispozice.</span><span class="sxs-lookup"><span data-stu-id="ce8da-224">MDN disposition type value.</span></span> <span data-ttu-id="ce8da-225">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-225">(Optional)</span></span> |
| <span data-ttu-id="ce8da-226">isMessageFailed</span><span class="sxs-lookup"><span data-stu-id="ce8da-226">isMessageFailed</span></span> |<span data-ttu-id="ce8da-227">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-227">Boolean</span></span> | <span data-ttu-id="ce8da-228">Jestli AS2 se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="ce8da-228">Whether the AS2 message failed.</span></span> <span data-ttu-id="ce8da-229">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-229">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-230">isMessageSigned</span><span class="sxs-lookup"><span data-stu-id="ce8da-230">isMessageSigned</span></span> |<span data-ttu-id="ce8da-231">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-231">Boolean</span></span> | <span data-ttu-id="ce8da-232">Jestli byla podepsána zpráva AS2.</span><span class="sxs-lookup"><span data-stu-id="ce8da-232">Whether the AS2 message was signed.</span></span> <span data-ttu-id="ce8da-233">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-233">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-234">isNrrEnabled</span><span class="sxs-lookup"><span data-stu-id="ce8da-234">isNrrEnabled</span></span> | <span data-ttu-id="ce8da-235">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="ce8da-235">Boolean</span></span> | <span data-ttu-id="ce8da-236">Použijte výchozí hodnotu, pokud hodnota není znám.</span><span class="sxs-lookup"><span data-stu-id="ce8da-236">Use default value if the value is not known.</span></span> <span data-ttu-id="ce8da-237">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-237">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-238">statusCode</span><span class="sxs-lookup"><span data-stu-id="ce8da-238">statusCode</span></span> | <span data-ttu-id="ce8da-239">výčet</span><span class="sxs-lookup"><span data-stu-id="ce8da-239">Enum</span></span> | <span data-ttu-id="ce8da-240">Povolené hodnoty jsou **platných**, **zamítnutý**, a **AcceptedWithErrors**.</span><span class="sxs-lookup"><span data-stu-id="ce8da-240">Allowed values are **Accepted**, **Rejected**, and **AcceptedWithErrors**.</span></span> <span data-ttu-id="ce8da-241">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-241">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-242">micVerificationStatus</span><span class="sxs-lookup"><span data-stu-id="ce8da-242">micVerificationStatus</span></span> | <span data-ttu-id="ce8da-243">výčet</span><span class="sxs-lookup"><span data-stu-id="ce8da-243">Enum</span></span> | <span data-ttu-id="ce8da-244">Povolené hodnoty jsou **NotApplicable**, **úspěšné**, a **se nezdařilo**.</span><span class="sxs-lookup"><span data-stu-id="ce8da-244">Allowed values are **NotApplicable**, **Succeeded**, and **Failed**.</span></span> <span data-ttu-id="ce8da-245">(Povinné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-245">(Mandatory)</span></span> |
| <span data-ttu-id="ce8da-246">correlationMessageId</span><span class="sxs-lookup"><span data-stu-id="ce8da-246">correlationMessageId</span></span> | <span data-ttu-id="ce8da-247">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ce8da-247">String</span></span> | <span data-ttu-id="ce8da-248">ID korelace.</span><span class="sxs-lookup"><span data-stu-id="ce8da-248">Correlation ID.</span></span> <span data-ttu-id="ce8da-249">Původní messaged ID (ID zprávy zprávy, pro který je nakonfigurovaný MDN).</span><span class="sxs-lookup"><span data-stu-id="ce8da-249">The original messaged ID (the message ID of the message for which MDN is configured).</span></span> <span data-ttu-id="ce8da-250">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-250">(Optional)</span></span> |
| <span data-ttu-id="ce8da-251">incomingHeaders</span><span class="sxs-lookup"><span data-stu-id="ce8da-251">incomingHeaders</span></span> | <span data-ttu-id="ce8da-252">Slovník JToken</span><span class="sxs-lookup"><span data-stu-id="ce8da-252">Dictionary of JToken</span></span> | <span data-ttu-id="ce8da-253">Určuje podrobnosti záhlaví příchozí zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-253">Indicates incoming message header details.</span></span> <span data-ttu-id="ce8da-254">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-254">(Optional)</span></span> |
| <span data-ttu-id="ce8da-255">outgoingHeaders</span><span class="sxs-lookup"><span data-stu-id="ce8da-255">outgoingHeaders</span></span> |<span data-ttu-id="ce8da-256">Slovník JToken</span><span class="sxs-lookup"><span data-stu-id="ce8da-256">Dictionary of JToken</span></span> | <span data-ttu-id="ce8da-257">Určuje odchozí podrobnosti záhlaví zprávy.</span><span class="sxs-lookup"><span data-stu-id="ce8da-257">Indicates outgoing message header details.</span></span> <span data-ttu-id="ce8da-258">(Volitelné)</span><span class="sxs-lookup"><span data-stu-id="ce8da-258">(Optional)</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ce8da-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce8da-259">Next steps</span></span>
* <span data-ttu-id="ce8da-260">Další informace o [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ce8da-260">Learn more about the [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span>    
* <span data-ttu-id="ce8da-261">Další informace o [sledování zpráv B2B](logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="ce8da-261">Learn more about [monitoring B2B messages](logic-apps-monitor-b2b-message.md).</span></span>   
* <span data-ttu-id="ce8da-262">Další informace o [B2B vlastní sledování schémata](logic-apps-track-integration-account-custom-tracking-schema.md).</span><span class="sxs-lookup"><span data-stu-id="ce8da-262">Learn more about [B2B custom tracking schemas](logic-apps-track-integration-account-custom-tracking-schema.md).</span></span>   
* <span data-ttu-id="ce8da-263">Další informace o [X12 sledování schémata](logic-apps-track-integration-account-x12-tracking-schema.md).</span><span class="sxs-lookup"><span data-stu-id="ce8da-263">Learn more about [X12 tracking schemas](logic-apps-track-integration-account-x12-tracking-schema.md).</span></span>   
* <span data-ttu-id="ce8da-264">Další informace o [sledování zpráv B2B na portálu služby Operations Management Suite](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="ce8da-264">Learn about [tracking B2B messages in the Operations Management Suite portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>
