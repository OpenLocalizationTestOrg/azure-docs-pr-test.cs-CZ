---
title: "Akce škálování použijte k odesílání e-mailu a webhooku oznámení výstrah. | Dokumentace Microsoftu"
description: "Zjistit, jak používat akce škálování adres URL webových zavolat nebo poslat e-mailová oznámení v nástroji Sledování Azure. "
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: eb9a4c98-0894-488c-8ee8-5df0065d094f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: ancav
ms.openlocfilehash: 16caf14028494800e9259f0296c292b606d0210a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a><span data-ttu-id="bfc91-104">Akce škálování používat k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure</span><span class="sxs-lookup"><span data-stu-id="bfc91-104">Use autoscale actions to send email and webhook alert notifications in Azure Monitor</span></span>
<span data-ttu-id="bfc91-105">Tento článek ukazuje, jak nastavíte aktivační události tak, aby můžete volat konkrétní webové adresy URL nebo poslat e-mailů podle akce škálování v Azure.</span><span class="sxs-lookup"><span data-stu-id="bfc91-105">This article shows you how set up triggers so that you can call specific web URLs or send emails based on autoscale actions in Azure.</span></span>  

## <a name="webhooks"></a><span data-ttu-id="bfc91-106">Webhooky</span><span class="sxs-lookup"><span data-stu-id="bfc91-106">Webhooks</span></span>
<span data-ttu-id="bfc91-107">Webhooky umožňují směrovat Azure oznámení výstrah do jiných systémů pro následné zpracování nebo vlastních oznámení.</span><span class="sxs-lookup"><span data-stu-id="bfc91-107">Webhooks allow you to route the Azure alert notifications to other systems for post-processing or custom notifications.</span></span> <span data-ttu-id="bfc91-108">Například směrování upozornění do služby, které může zpracovávat příchozí webové žádosti pro odeslání že SMS, protokolu chyb, upozornění, že tým pomocí chatu nebo zasílání zpráv služby atd. Webhook identifikátor URI musí být platný koncový bod HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bfc91-108">For example, routing the alert to services that can handle an incoming web request to send SMS, log bugs, notify a team using chat or messaging services, etc. The webhook URI must be a valid HTTP or HTTPS endpoint.</span></span>

## <a name="email"></a><span data-ttu-id="bfc91-109">E-mail</span><span class="sxs-lookup"><span data-stu-id="bfc91-109">Email</span></span>
<span data-ttu-id="bfc91-110">Všechny platné e-mailovou adresu nelze odesílat e-mailu.</span><span class="sxs-lookup"><span data-stu-id="bfc91-110">Email can be sent to any valid email address.</span></span> <span data-ttu-id="bfc91-111">Správci a spolusprávci předplatného, kde je spuštěno pravidlo bude také upozorněni.</span><span class="sxs-lookup"><span data-stu-id="bfc91-111">Administrators and co-administrators of the subscription where the rule is running will also be notified.</span></span>

## <a name="cloud-services-and-web-apps"></a><span data-ttu-id="bfc91-112">Cloudové služby a webové aplikace</span><span class="sxs-lookup"><span data-stu-id="bfc91-112">Cloud Services and Web Apps</span></span>
<span data-ttu-id="bfc91-113">Vám může výslovný souhlas z portálu Azure pro cloudové služby a serverové farmy (webové aplikace).</span><span class="sxs-lookup"><span data-stu-id="bfc91-113">You can opt-in from the Azure portal for Cloud Services and Server Farms (Web Apps).</span></span>

* <span data-ttu-id="bfc91-114">Vyberte **škálovat podle** metriku.</span><span class="sxs-lookup"><span data-stu-id="bfc91-114">Choose the **scale by** metric.</span></span>

![škálování podle](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a><span data-ttu-id="bfc91-116">Sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bfc91-116">Virtual Machine scale sets</span></span>
<span data-ttu-id="bfc91-117">Pro novější virtuální počítače vytvořené pomocí Resource Manageru (sady škálování virtuálního počítače) to můžete nakonfigurovat pomocí rozhraní REST API, šablony Resource Manageru, prostředí PowerShell a rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="bfc91-117">For newer Virtual Machines created with Resource Manager (Virtual Machine scale sets), you can configure this using REST API, Resource Manager templates, PowerShell, and CLI.</span></span> <span data-ttu-id="bfc91-118">Portál rozhraní dosud nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bfc91-118">A portal interface is not yet available.</span></span>
<span data-ttu-id="bfc91-119">Při použití šablony REST API nebo Resource Manager, obsahovat element oznámení s následujícími parametry.</span><span class="sxs-lookup"><span data-stu-id="bfc91-119">When using the REST API or Resource Manager template, include the notifications element with the following options.</span></span>

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
| <span data-ttu-id="bfc91-120">Pole</span><span class="sxs-lookup"><span data-stu-id="bfc91-120">Field</span></span> | <span data-ttu-id="bfc91-121">Povinné?</span><span class="sxs-lookup"><span data-stu-id="bfc91-121">Mandatory?</span></span> | <span data-ttu-id="bfc91-122">Popis</span><span class="sxs-lookup"><span data-stu-id="bfc91-122">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bfc91-123">operace</span><span class="sxs-lookup"><span data-stu-id="bfc91-123">operation</span></span> |<span data-ttu-id="bfc91-124">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-124">yes</span></span> |<span data-ttu-id="bfc91-125">Hodnota musí být "Škálování"</span><span class="sxs-lookup"><span data-stu-id="bfc91-125">value must be "Scale"</span></span> |
| <span data-ttu-id="bfc91-126">sendToSubscriptionAdministrator</span><span class="sxs-lookup"><span data-stu-id="bfc91-126">sendToSubscriptionAdministrator</span></span> |<span data-ttu-id="bfc91-127">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-127">yes</span></span> |<span data-ttu-id="bfc91-128">Hodnota musí být "true" nebo "Nepravda"</span><span class="sxs-lookup"><span data-stu-id="bfc91-128">value must be "true" or "false"</span></span> |
| <span data-ttu-id="bfc91-129">sendToSubscriptionCoAdministrators</span><span class="sxs-lookup"><span data-stu-id="bfc91-129">sendToSubscriptionCoAdministrators</span></span> |<span data-ttu-id="bfc91-130">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-130">yes</span></span> |<span data-ttu-id="bfc91-131">Hodnota musí být "true" nebo "Nepravda"</span><span class="sxs-lookup"><span data-stu-id="bfc91-131">value must be "true" or "false"</span></span> |
| <span data-ttu-id="bfc91-132">customEmails</span><span class="sxs-lookup"><span data-stu-id="bfc91-132">customEmails</span></span> |<span data-ttu-id="bfc91-133">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-133">yes</span></span> |<span data-ttu-id="bfc91-134">Hodnota může být null [] nebo pole řetězců e-mailů</span><span class="sxs-lookup"><span data-stu-id="bfc91-134">value can be null [] or string array of emails</span></span> |
| <span data-ttu-id="bfc91-135">Webhooky</span><span class="sxs-lookup"><span data-stu-id="bfc91-135">webhooks</span></span> |<span data-ttu-id="bfc91-136">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-136">yes</span></span> |<span data-ttu-id="bfc91-137">Hodnota může být null nebo platný identifikátor Uri</span><span class="sxs-lookup"><span data-stu-id="bfc91-137">value can be null or valid Uri</span></span> |
| <span data-ttu-id="bfc91-138">serviceUri</span><span class="sxs-lookup"><span data-stu-id="bfc91-138">serviceUri</span></span> |<span data-ttu-id="bfc91-139">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-139">yes</span></span> |<span data-ttu-id="bfc91-140">platný https identifikátor Uri</span><span class="sxs-lookup"><span data-stu-id="bfc91-140">a valid https Uri</span></span> |
| <span data-ttu-id="bfc91-141">properties</span><span class="sxs-lookup"><span data-stu-id="bfc91-141">properties</span></span> |<span data-ttu-id="bfc91-142">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-142">yes</span></span> |<span data-ttu-id="bfc91-143">Hodnota musí být prázdný {} nebo může obsahovat páry klíč hodnota</span><span class="sxs-lookup"><span data-stu-id="bfc91-143">value must be empty {} or can contain key-value pairs</span></span> |

## <a name="authentication-in-webhooks"></a><span data-ttu-id="bfc91-144">Ověřování v webhooky</span><span class="sxs-lookup"><span data-stu-id="bfc91-144">Authentication in webhooks</span></span>
<span data-ttu-id="bfc91-145">Webhooku můžete ověřit pomocí ověřování založeného na token, kam jste uložili webhooku identifikátor URI s tokenu ID jako parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="bfc91-145">The webhook can authenticate using token-based authentication, where you save the webhook URI with a token ID as a query parameter.</span></span> <span data-ttu-id="bfc91-146">Například https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue</span><span class="sxs-lookup"><span data-stu-id="bfc91-146">For example, https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue</span></span>

## <a name="autoscale-notification-webhook-payload-schema"></a><span data-ttu-id="bfc91-147">Škálování oznámení webhooku datové části schématu</span><span class="sxs-lookup"><span data-stu-id="bfc91-147">Autoscale notification webhook payload schema</span></span>
<span data-ttu-id="bfc91-148">Když se vygeneruje oznámení škálování, je následující metadata zahrnuty do datové části webhooku:</span><span class="sxs-lookup"><span data-stu-id="bfc91-148">When the autoscale notification is generated, the following metadata is included in the webhook payload:</span></span>

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| <span data-ttu-id="bfc91-149">Pole</span><span class="sxs-lookup"><span data-stu-id="bfc91-149">Field</span></span> | <span data-ttu-id="bfc91-150">Povinné?</span><span class="sxs-lookup"><span data-stu-id="bfc91-150">Mandatory?</span></span> | <span data-ttu-id="bfc91-151">Popis</span><span class="sxs-lookup"><span data-stu-id="bfc91-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bfc91-152">status</span><span class="sxs-lookup"><span data-stu-id="bfc91-152">status</span></span> |<span data-ttu-id="bfc91-153">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-153">yes</span></span> |<span data-ttu-id="bfc91-154">Stav, který označuje, že bylo vygenerováno akce škálování</span><span class="sxs-lookup"><span data-stu-id="bfc91-154">The status that indicates that an autoscale action was generated</span></span> |
| <span data-ttu-id="bfc91-155">operace</span><span class="sxs-lookup"><span data-stu-id="bfc91-155">operation</span></span> |<span data-ttu-id="bfc91-156">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-156">yes</span></span> |<span data-ttu-id="bfc91-157">Pro zvýšení instancí bude "Horizontální navýšení kapacity" a pro snížení instancí, bude "Škálování v"</span><span class="sxs-lookup"><span data-stu-id="bfc91-157">For an increase of instances, it will be "Scale Out" and for a decrease in instances, it will be "Scale In"</span></span> |
| <span data-ttu-id="bfc91-158">Kontext</span><span class="sxs-lookup"><span data-stu-id="bfc91-158">context</span></span> |<span data-ttu-id="bfc91-159">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-159">yes</span></span> |<span data-ttu-id="bfc91-160">Kontext akce škálování</span><span class="sxs-lookup"><span data-stu-id="bfc91-160">The autoscale action context</span></span> |
| <span data-ttu-id="bfc91-161">časové razítko</span><span class="sxs-lookup"><span data-stu-id="bfc91-161">timestamp</span></span> |<span data-ttu-id="bfc91-162">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-162">yes</span></span> |<span data-ttu-id="bfc91-163">Časové razítko, když byla spuštěna akce škálování</span><span class="sxs-lookup"><span data-stu-id="bfc91-163">Time stamp when the autoscale action was triggered</span></span> |
| <span data-ttu-id="bfc91-164">id</span><span class="sxs-lookup"><span data-stu-id="bfc91-164">id</span></span> |<span data-ttu-id="bfc91-165">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-165">Yes</span></span> |<span data-ttu-id="bfc91-166">ID správce prostředků nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="bfc91-166">Resource Manager ID of the autoscale setting</span></span> |
| <span data-ttu-id="bfc91-167">jméno</span><span class="sxs-lookup"><span data-stu-id="bfc91-167">name</span></span> |<span data-ttu-id="bfc91-168">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-168">Yes</span></span> |<span data-ttu-id="bfc91-169">Název nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="bfc91-169">The name of the autoscale setting</span></span> |
| <span data-ttu-id="bfc91-170">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="bfc91-170">details</span></span> |<span data-ttu-id="bfc91-171">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-171">Yes</span></span> |<span data-ttu-id="bfc91-172">Vysvětlení akci, kterou trvalo službu automatické škálování a změna v počet instancí</span><span class="sxs-lookup"><span data-stu-id="bfc91-172">Explanation of the action that the autoscale service took and the change in the instance count</span></span> |
| <span data-ttu-id="bfc91-173">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="bfc91-173">subscriptionId</span></span> |<span data-ttu-id="bfc91-174">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-174">Yes</span></span> |<span data-ttu-id="bfc91-175">ID předplatného cílového prostředku, který bude změněno měřítko</span><span class="sxs-lookup"><span data-stu-id="bfc91-175">Subscription ID of the target resource that is being scaled</span></span> |
| <span data-ttu-id="bfc91-176">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="bfc91-176">resourceGroupName</span></span> |<span data-ttu-id="bfc91-177">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-177">Yes</span></span> |<span data-ttu-id="bfc91-178">Název skupiny prostředků cílového prostředku, který bude změněno měřítko</span><span class="sxs-lookup"><span data-stu-id="bfc91-178">Resource Group name of the target resource that is being scaled</span></span> |
| <span data-ttu-id="bfc91-179">resourceName</span><span class="sxs-lookup"><span data-stu-id="bfc91-179">resourceName</span></span> |<span data-ttu-id="bfc91-180">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-180">Yes</span></span> |<span data-ttu-id="bfc91-181">Název cílového prostředku, který bude změněno měřítko</span><span class="sxs-lookup"><span data-stu-id="bfc91-181">Name of the target resource that is being scaled</span></span> |
| <span data-ttu-id="bfc91-182">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="bfc91-182">resourceType</span></span> |<span data-ttu-id="bfc91-183">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-183">Yes</span></span> |<span data-ttu-id="bfc91-184">Tři podporované hodnoty: "microsoft.classiccompute/domainnames/slots/roles" – cloudové služby rolí, "microsoft.compute/virtualmachinescalesets" - sady škálování virtuálního počítače a "Microsoft.Web/serverfarms" - webové aplikace</span><span class="sxs-lookup"><span data-stu-id="bfc91-184">The three supported values: "microsoft.classiccompute/domainnames/slots/roles" - Cloud Service roles, "microsoft.compute/virtualmachinescalesets" - Virtual Machine Scale Sets,  and "Microsoft.Web/serverfarms" - Web App</span></span> |
| <span data-ttu-id="bfc91-185">resourceId</span><span class="sxs-lookup"><span data-stu-id="bfc91-185">resourceId</span></span> |<span data-ttu-id="bfc91-186">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-186">Yes</span></span> |<span data-ttu-id="bfc91-187">Správce prostředků ID cílového prostředku, který bude změněno měřítko</span><span class="sxs-lookup"><span data-stu-id="bfc91-187">Resource Manager ID of the target resource that is being scaled</span></span> |
| <span data-ttu-id="bfc91-188">portalLink</span><span class="sxs-lookup"><span data-stu-id="bfc91-188">portalLink</span></span> |<span data-ttu-id="bfc91-189">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-189">Yes</span></span> |<span data-ttu-id="bfc91-190">Azure portálu odkaz na stránce Souhrn cílového prostředku</span><span class="sxs-lookup"><span data-stu-id="bfc91-190">Azure portal link to the summary page of the target resource</span></span> |
| <span data-ttu-id="bfc91-191">oldCapacity</span><span class="sxs-lookup"><span data-stu-id="bfc91-191">oldCapacity</span></span> |<span data-ttu-id="bfc91-192">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-192">Yes</span></span> |<span data-ttu-id="bfc91-193">Aktuální (starý) počet instancí při škálování trvalo akce škálování</span><span class="sxs-lookup"><span data-stu-id="bfc91-193">The current (old) instance count when Autoscale took a scale action</span></span> |
| <span data-ttu-id="bfc91-194">newCapacity</span><span class="sxs-lookup"><span data-stu-id="bfc91-194">newCapacity</span></span> |<span data-ttu-id="bfc91-195">Ano</span><span class="sxs-lookup"><span data-stu-id="bfc91-195">Yes</span></span> |<span data-ttu-id="bfc91-196">Nový počet instancí, který škálování škálovat prostředek, který</span><span class="sxs-lookup"><span data-stu-id="bfc91-196">The new instance count that Autoscale scaled the resource to</span></span> |
| <span data-ttu-id="bfc91-197">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bfc91-197">Properties</span></span> |<span data-ttu-id="bfc91-198">Ne</span><span class="sxs-lookup"><span data-stu-id="bfc91-198">No</span></span> |<span data-ttu-id="bfc91-199">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="bfc91-199">Optional.</span></span> <span data-ttu-id="bfc91-200">Sada < klíč a hodnotu > páry (například slovník < String, String >).</span><span class="sxs-lookup"><span data-stu-id="bfc91-200">Set of <Key, Value> pairs (for example,  Dictionary <String, String>).</span></span> <span data-ttu-id="bfc91-201">Vlastnosti pole je nepovinné.</span><span class="sxs-lookup"><span data-stu-id="bfc91-201">The properties field is optional.</span></span> <span data-ttu-id="bfc91-202">Do vlastní uživatelské rozhraní nebo aplikací na základě logiky pracovního postupu můžete zadat klíče a hodnoty, které lze předat pomocí datové části.</span><span class="sxs-lookup"><span data-stu-id="bfc91-202">In a custom user interface  or Logic app based workflow, you can enter keys and values that can be passed using the payload.</span></span> <span data-ttu-id="bfc91-203">Jiný způsob, jak předat vlastní vlastnosti odchozí voláním webhooku se má používat webhooku URI samotné (jako parametry dotazu)</span><span class="sxs-lookup"><span data-stu-id="bfc91-203">An alternate way to pass custom properties back to the outgoing webhook call is to use the webhook URI itself (as query parameters)</span></span> |
