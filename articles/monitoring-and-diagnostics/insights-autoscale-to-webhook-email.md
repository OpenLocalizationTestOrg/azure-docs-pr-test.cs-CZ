---
title: "aaaUse škálování akce toosend e-mailu a webhooku oznámení výstrah. | Dokumentace Microsoftu"
description: "V tématu Jak toocall akce škálování toouse webové adresy URL nebo poslat e-mailová oznámení v nástroji Sledování Azure. "
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
ms.openlocfilehash: f611a18f5a808412fbdd0c89e3addb36437064c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-autoscale-actions-toosend-email-and-webhook-alert-notifications-in-azure-monitor"></a><span data-ttu-id="51f4e-104">Použití automatického škálování akce toosend e-mailu a webhooku oznámení výstrah v monitorování Azure</span><span class="sxs-lookup"><span data-stu-id="51f4e-104">Use autoscale actions toosend email and webhook alert notifications in Azure Monitor</span></span>
<span data-ttu-id="51f4e-105">Tento článek ukazuje, jak nastavíte aktivační události tak, aby můžete volat konkrétní webové adresy URL nebo poslat e-mailů podle akce škálování v Azure.</span><span class="sxs-lookup"><span data-stu-id="51f4e-105">This article shows you how set up triggers so that you can call specific web URLs or send emails based on autoscale actions in Azure.</span></span>  

## <a name="webhooks"></a><span data-ttu-id="51f4e-106">Webhooky</span><span class="sxs-lookup"><span data-stu-id="51f4e-106">Webhooks</span></span>
<span data-ttu-id="51f4e-107">Webhooky povolit tooroute hello Azure oznámení o výstrahách tooother systémy pro následné zpracování nebo vlastních oznámení.</span><span class="sxs-lookup"><span data-stu-id="51f4e-107">Webhooks allow you tooroute hello Azure alert notifications tooother systems for post-processing or custom notifications.</span></span> <span data-ttu-id="51f4e-108">Například směrování výstrah tooservices hello, které může zpracovat příchozí webové žádosti toosend SMS, protokolu chyb, upozornění týmu pomocí chatu nebo služby zasílání zpráv, atd. hello webhooku identifikátor URI musí být platný koncový bod HTTP nebo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="51f4e-108">For example, routing hello alert tooservices that can handle an incoming web request toosend SMS, log bugs, notify a team using chat or messaging services, etc. hello webhook URI must be a valid HTTP or HTTPS endpoint.</span></span>

## <a name="email"></a><span data-ttu-id="51f4e-109">E-mail</span><span class="sxs-lookup"><span data-stu-id="51f4e-109">Email</span></span>
<span data-ttu-id="51f4e-110">Tooany platné e-mailová adresa může být odeslán e-mail.</span><span class="sxs-lookup"><span data-stu-id="51f4e-110">Email can be sent tooany valid email address.</span></span> <span data-ttu-id="51f4e-111">Také upozorňování správci a spolusprávci předplatného hello, kde je spuštěno pravidlo hello.</span><span class="sxs-lookup"><span data-stu-id="51f4e-111">Administrators and co-administrators of hello subscription where hello rule is running will also be notified.</span></span>

## <a name="cloud-services-and-web-apps"></a><span data-ttu-id="51f4e-112">Cloudové služby a webové aplikace</span><span class="sxs-lookup"><span data-stu-id="51f4e-112">Cloud Services and Web Apps</span></span>
<span data-ttu-id="51f4e-113">Vám může výslovný souhlas z hello portál Azure pro cloudové služby a serverové farmy (webové aplikace).</span><span class="sxs-lookup"><span data-stu-id="51f4e-113">You can opt-in from hello Azure portal for Cloud Services and Server Farms (Web Apps).</span></span>

* <span data-ttu-id="51f4e-114">Zvolte hello **škálovat podle** metriku.</span><span class="sxs-lookup"><span data-stu-id="51f4e-114">Choose hello **scale by** metric.</span></span>

![škálování podle](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a><span data-ttu-id="51f4e-116">Sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="51f4e-116">Virtual Machine scale sets</span></span>
<span data-ttu-id="51f4e-117">Pro novější virtuální počítače vytvořené pomocí Resource Manageru (sady škálování virtuálního počítače) to můžete nakonfigurovat pomocí rozhraní REST API, šablony Resource Manageru, prostředí PowerShell a rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="51f4e-117">For newer Virtual Machines created with Resource Manager (Virtual Machine scale sets), you can configure this using REST API, Resource Manager templates, PowerShell, and CLI.</span></span> <span data-ttu-id="51f4e-118">Portál rozhraní dosud nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="51f4e-118">A portal interface is not yet available.</span></span>
<span data-ttu-id="51f4e-119">Pokud používáte hello REST API nebo šablony Resource Manageru, zahrňte hello oznámení prvek s hello následující možnosti.</span><span class="sxs-lookup"><span data-stu-id="51f4e-119">When using hello REST API or Resource Manager template, include hello notifications element with hello following options.</span></span>

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
| <span data-ttu-id="51f4e-120">Pole</span><span class="sxs-lookup"><span data-stu-id="51f4e-120">Field</span></span> | <span data-ttu-id="51f4e-121">Povinné?</span><span class="sxs-lookup"><span data-stu-id="51f4e-121">Mandatory?</span></span> | <span data-ttu-id="51f4e-122">Popis</span><span class="sxs-lookup"><span data-stu-id="51f4e-122">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="51f4e-123">operace</span><span class="sxs-lookup"><span data-stu-id="51f4e-123">operation</span></span> |<span data-ttu-id="51f4e-124">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-124">yes</span></span> |<span data-ttu-id="51f4e-125">Hodnota musí být "Škálování"</span><span class="sxs-lookup"><span data-stu-id="51f4e-125">value must be "Scale"</span></span> |
| <span data-ttu-id="51f4e-126">sendToSubscriptionAdministrator</span><span class="sxs-lookup"><span data-stu-id="51f4e-126">sendToSubscriptionAdministrator</span></span> |<span data-ttu-id="51f4e-127">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-127">yes</span></span> |<span data-ttu-id="51f4e-128">Hodnota musí být "true" nebo "Nepravda"</span><span class="sxs-lookup"><span data-stu-id="51f4e-128">value must be "true" or "false"</span></span> |
| <span data-ttu-id="51f4e-129">sendToSubscriptionCoAdministrators</span><span class="sxs-lookup"><span data-stu-id="51f4e-129">sendToSubscriptionCoAdministrators</span></span> |<span data-ttu-id="51f4e-130">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-130">yes</span></span> |<span data-ttu-id="51f4e-131">Hodnota musí být "true" nebo "Nepravda"</span><span class="sxs-lookup"><span data-stu-id="51f4e-131">value must be "true" or "false"</span></span> |
| <span data-ttu-id="51f4e-132">customEmails</span><span class="sxs-lookup"><span data-stu-id="51f4e-132">customEmails</span></span> |<span data-ttu-id="51f4e-133">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-133">yes</span></span> |<span data-ttu-id="51f4e-134">Hodnota může být null [] nebo pole řetězců e-mailů</span><span class="sxs-lookup"><span data-stu-id="51f4e-134">value can be null [] or string array of emails</span></span> |
| <span data-ttu-id="51f4e-135">Webhooky</span><span class="sxs-lookup"><span data-stu-id="51f4e-135">webhooks</span></span> |<span data-ttu-id="51f4e-136">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-136">yes</span></span> |<span data-ttu-id="51f4e-137">Hodnota může být null nebo platný identifikátor Uri</span><span class="sxs-lookup"><span data-stu-id="51f4e-137">value can be null or valid Uri</span></span> |
| <span data-ttu-id="51f4e-138">serviceUri</span><span class="sxs-lookup"><span data-stu-id="51f4e-138">serviceUri</span></span> |<span data-ttu-id="51f4e-139">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-139">yes</span></span> |<span data-ttu-id="51f4e-140">platný https identifikátor Uri</span><span class="sxs-lookup"><span data-stu-id="51f4e-140">a valid https Uri</span></span> |
| <span data-ttu-id="51f4e-141">properties</span><span class="sxs-lookup"><span data-stu-id="51f4e-141">properties</span></span> |<span data-ttu-id="51f4e-142">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-142">yes</span></span> |<span data-ttu-id="51f4e-143">Hodnota musí být prázdný {} nebo může obsahovat páry klíč hodnota</span><span class="sxs-lookup"><span data-stu-id="51f4e-143">value must be empty {} or can contain key-value pairs</span></span> |

## <a name="authentication-in-webhooks"></a><span data-ttu-id="51f4e-144">Ověřování v webhooky</span><span class="sxs-lookup"><span data-stu-id="51f4e-144">Authentication in webhooks</span></span>
<span data-ttu-id="51f4e-145">Hello webhooku můžete ověřit pomocí ověřování založeného na token, kam jste uložili hello webhooku identifikátor URI s tokenu ID jako parametr dotazu.</span><span class="sxs-lookup"><span data-stu-id="51f4e-145">hello webhook can authenticate using token-based authentication, where you save hello webhook URI with a token ID as a query parameter.</span></span> <span data-ttu-id="51f4e-146">Například https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue</span><span class="sxs-lookup"><span data-stu-id="51f4e-146">For example, https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue</span></span>

## <a name="autoscale-notification-webhook-payload-schema"></a><span data-ttu-id="51f4e-147">Škálování oznámení webhooku datové části schématu</span><span class="sxs-lookup"><span data-stu-id="51f4e-147">Autoscale notification webhook payload schema</span></span>
<span data-ttu-id="51f4e-148">Generování oznámení škálování hello hello následující metadata je zahrnuta v datové části webhooku hello:</span><span class="sxs-lookup"><span data-stu-id="51f4e-148">When hello autoscale notification is generated, hello following metadata is included in hello webhook payload:</span></span>

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' toocapacity '2'",
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


| <span data-ttu-id="51f4e-149">Pole</span><span class="sxs-lookup"><span data-stu-id="51f4e-149">Field</span></span> | <span data-ttu-id="51f4e-150">Povinné?</span><span class="sxs-lookup"><span data-stu-id="51f4e-150">Mandatory?</span></span> | <span data-ttu-id="51f4e-151">Popis</span><span class="sxs-lookup"><span data-stu-id="51f4e-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="51f4e-152">status</span><span class="sxs-lookup"><span data-stu-id="51f4e-152">status</span></span> |<span data-ttu-id="51f4e-153">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-153">yes</span></span> |<span data-ttu-id="51f4e-154">Hello stav, který označuje, že bylo vygenerováno akce škálování</span><span class="sxs-lookup"><span data-stu-id="51f4e-154">hello status that indicates that an autoscale action was generated</span></span> |
| <span data-ttu-id="51f4e-155">operace</span><span class="sxs-lookup"><span data-stu-id="51f4e-155">operation</span></span> |<span data-ttu-id="51f4e-156">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-156">yes</span></span> |<span data-ttu-id="51f4e-157">Pro zvýšení instancí bude "Horizontální navýšení kapacity" a pro snížení instancí, bude "Škálování v"</span><span class="sxs-lookup"><span data-stu-id="51f4e-157">For an increase of instances, it will be "Scale Out" and for a decrease in instances, it will be "Scale In"</span></span> |
| <span data-ttu-id="51f4e-158">Kontext</span><span class="sxs-lookup"><span data-stu-id="51f4e-158">context</span></span> |<span data-ttu-id="51f4e-159">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-159">yes</span></span> |<span data-ttu-id="51f4e-160">kontext akce škálování Hello</span><span class="sxs-lookup"><span data-stu-id="51f4e-160">hello autoscale action context</span></span> |
| <span data-ttu-id="51f4e-161">časové razítko</span><span class="sxs-lookup"><span data-stu-id="51f4e-161">timestamp</span></span> |<span data-ttu-id="51f4e-162">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-162">yes</span></span> |<span data-ttu-id="51f4e-163">Časové razítko, když byla spuštěna akce škálování hello</span><span class="sxs-lookup"><span data-stu-id="51f4e-163">Time stamp when hello autoscale action was triggered</span></span> |
| <span data-ttu-id="51f4e-164">id</span><span class="sxs-lookup"><span data-stu-id="51f4e-164">id</span></span> |<span data-ttu-id="51f4e-165">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-165">Yes</span></span> |<span data-ttu-id="51f4e-166">Správce prostředků ID nastavení automatického škálování hello</span><span class="sxs-lookup"><span data-stu-id="51f4e-166">Resource Manager ID of hello autoscale setting</span></span> |
| <span data-ttu-id="51f4e-167">jméno</span><span class="sxs-lookup"><span data-stu-id="51f4e-167">name</span></span> |<span data-ttu-id="51f4e-168">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-168">Yes</span></span> |<span data-ttu-id="51f4e-169">Název nastavení automatického škálování hello Hello</span><span class="sxs-lookup"><span data-stu-id="51f4e-169">hello name of hello autoscale setting</span></span> |
| <span data-ttu-id="51f4e-170">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="51f4e-170">details</span></span> |<span data-ttu-id="51f4e-171">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-171">Yes</span></span> |<span data-ttu-id="51f4e-172">Vysvětlení hello akci trvalo hello škálování služby a hello změnit v počet instancí hello</span><span class="sxs-lookup"><span data-stu-id="51f4e-172">Explanation of hello action that hello autoscale service took and hello change in hello instance count</span></span> |
| <span data-ttu-id="51f4e-173">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="51f4e-173">subscriptionId</span></span> |<span data-ttu-id="51f4e-174">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-174">Yes</span></span> |<span data-ttu-id="51f4e-175">ID odběru hello cílový prostředek, který bude změněno měřítko</span><span class="sxs-lookup"><span data-stu-id="51f4e-175">Subscription ID of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="51f4e-176">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="51f4e-176">resourceGroupName</span></span> |<span data-ttu-id="51f4e-177">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-177">Yes</span></span> |<span data-ttu-id="51f4e-178">Název skupiny prostředků hello cílový prostředek, který bude změněno měřítko</span><span class="sxs-lookup"><span data-stu-id="51f4e-178">Resource Group name of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="51f4e-179">resourceName</span><span class="sxs-lookup"><span data-stu-id="51f4e-179">resourceName</span></span> |<span data-ttu-id="51f4e-180">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-180">Yes</span></span> |<span data-ttu-id="51f4e-181">Název hello cílový prostředek, který bude změněno měřítko</span><span class="sxs-lookup"><span data-stu-id="51f4e-181">Name of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="51f4e-182">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="51f4e-182">resourceType</span></span> |<span data-ttu-id="51f4e-183">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-183">Yes</span></span> |<span data-ttu-id="51f4e-184">Hello tři podporované hodnoty: "microsoft.classiccompute/domainnames/slots/roles" – cloudové služby rolí, "microsoft.compute/virtualmachinescalesets" - sady škálování virtuálního počítače a "Microsoft.Web/serverfarms" - webové aplikace</span><span class="sxs-lookup"><span data-stu-id="51f4e-184">hello three supported values: "microsoft.classiccompute/domainnames/slots/roles" - Cloud Service roles, "microsoft.compute/virtualmachinescalesets" - Virtual Machine Scale Sets,  and "Microsoft.Web/serverfarms" - Web App</span></span> |
| <span data-ttu-id="51f4e-185">resourceId</span><span class="sxs-lookup"><span data-stu-id="51f4e-185">resourceId</span></span> |<span data-ttu-id="51f4e-186">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-186">Yes</span></span> |<span data-ttu-id="51f4e-187">Správce prostředků ID hello cílový prostředek, který bude změněno měřítko</span><span class="sxs-lookup"><span data-stu-id="51f4e-187">Resource Manager ID of hello target resource that is being scaled</span></span> |
| <span data-ttu-id="51f4e-188">portalLink</span><span class="sxs-lookup"><span data-stu-id="51f4e-188">portalLink</span></span> |<span data-ttu-id="51f4e-189">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-189">Yes</span></span> |<span data-ttu-id="51f4e-190">Portál Azure odkaz toohello souhrnná stránka hello cílový prostředek</span><span class="sxs-lookup"><span data-stu-id="51f4e-190">Azure portal link toohello summary page of hello target resource</span></span> |
| <span data-ttu-id="51f4e-191">oldCapacity</span><span class="sxs-lookup"><span data-stu-id="51f4e-191">oldCapacity</span></span> |<span data-ttu-id="51f4e-192">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-192">Yes</span></span> |<span data-ttu-id="51f4e-193">Hello aktuální (starý) počet instancí při škálování trvalo akce škálování</span><span class="sxs-lookup"><span data-stu-id="51f4e-193">hello current (old) instance count when Autoscale took a scale action</span></span> |
| <span data-ttu-id="51f4e-194">newCapacity</span><span class="sxs-lookup"><span data-stu-id="51f4e-194">newCapacity</span></span> |<span data-ttu-id="51f4e-195">Ano</span><span class="sxs-lookup"><span data-stu-id="51f4e-195">Yes</span></span> |<span data-ttu-id="51f4e-196">nový počet instancí Hello škálování příliš škálovat prostředek hello</span><span class="sxs-lookup"><span data-stu-id="51f4e-196">hello new instance count that Autoscale scaled hello resource too</span></span>|
| <span data-ttu-id="51f4e-197">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="51f4e-197">Properties</span></span> |<span data-ttu-id="51f4e-198">Ne</span><span class="sxs-lookup"><span data-stu-id="51f4e-198">No</span></span> |<span data-ttu-id="51f4e-199">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="51f4e-199">Optional.</span></span> <span data-ttu-id="51f4e-200">Sada < klíč a hodnotu > páry (například slovník < String, String >).</span><span class="sxs-lookup"><span data-stu-id="51f4e-200">Set of <Key, Value> pairs (for example,  Dictionary <String, String>).</span></span> <span data-ttu-id="51f4e-201">Hello vlastnosti pole je nepovinné.</span><span class="sxs-lookup"><span data-stu-id="51f4e-201">hello properties field is optional.</span></span> <span data-ttu-id="51f4e-202">Do vlastní uživatelské rozhraní nebo aplikací na základě logiky pracovního postupu můžete zadat klíče a hodnoty, které lze předat pomocí hello datové části.</span><span class="sxs-lookup"><span data-stu-id="51f4e-202">In a custom user interface  or Logic app based workflow, you can enter keys and values that can be passed using hello payload.</span></span> <span data-ttu-id="51f4e-203">Alternativní způsob vlastní vlastnosti toopass zpět toohello odchozí webhooku volání je toouse hello webhooku URI samotné (jako parametry dotazu)</span><span class="sxs-lookup"><span data-stu-id="51f4e-203">An alternate way toopass custom properties back toohello outgoing webhook call is toouse hello webhook URI itself (as query parameters)</span></span> |
