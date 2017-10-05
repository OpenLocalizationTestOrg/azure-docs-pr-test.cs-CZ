---
title: "Přizpůsobení pravidel brány firewall webových aplikací v Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Tento článek obsahuje informace o tom, jak přizpůsobit pravidla brány firewall webových aplikací v Application Gateway pomocí prostředí PowerShell."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: 681625e40035b05c593c6161236cb80b7db576b9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a><span data-ttu-id="27be5-103">Přizpůsobení pravidel brány firewall webových aplikací pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="27be5-103">Customize web application firewall rules through PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="27be5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="27be5-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="27be5-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27be5-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="27be5-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="27be5-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="27be5-107">Azure Application Gateway brány firewall webových aplikací (firewall webových aplikací) poskytuje ochranu pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="27be5-107">The Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="27be5-108">Tyto ochrany jsou poskytovány pomocí aplikace otevřete webový projekt zabezpečení (OWASP) základní pravidlo nastavit CRS ().</span><span class="sxs-lookup"><span data-stu-id="27be5-108">These protections are provided by the Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="27be5-109">Některá pravidla můžete způsobit falešně pozitivních zjištění a blokování skutečné provozu.</span><span class="sxs-lookup"><span data-stu-id="27be5-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="27be5-110">Z tohoto důvodu Application Gateway poskytuje schopnost přizpůsobit skupiny pravidla a pravidla.</span><span class="sxs-lookup"><span data-stu-id="27be5-110">For this reason, Application Gateway provides the capability to customize rule groups and rules.</span></span> <span data-ttu-id="27be5-111">Další informace o konkrétní pravidlo skupiny a pravidel najdete v tématu [seznam skupin řádku pravidlo brány firewall webových aplikací a pravidla](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="27be5-111">For more information on the specific rule groups and rules, see [List of web application firewall CRS Rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="27be5-112">Zobrazit skupiny pravidla a pravidla</span><span class="sxs-lookup"><span data-stu-id="27be5-112">View rule groups and rules</span></span>

<span data-ttu-id="27be5-113">Následující příklady kódu ukazují, jak zobrazit pravidla a pravidla skupiny, které se dají konfigurovat na bránu firewall webových aplikací s podporou aplikace.</span><span class="sxs-lookup"><span data-stu-id="27be5-113">The following code examples show how to view rules and rule groups that are configurable on a WAF-enabled application gateway.</span></span>

### <a name="view-rule-groups"></a><span data-ttu-id="27be5-114">Zobrazení pravidla skupiny</span><span class="sxs-lookup"><span data-stu-id="27be5-114">View rule groups</span></span>

<span data-ttu-id="27be5-115">Následující příklad ukazuje, jak chcete-li zobrazit skupiny pravidel:</span><span class="sxs-lookup"><span data-stu-id="27be5-115">The following example shows how to view rule groups:</span></span>

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

<span data-ttu-id="27be5-116">Tento výstup je oříznuta odpovědí z předchozího příkladu:</span><span class="sxs-lookup"><span data-stu-id="27be5-116">The following output is a truncated response from the preceding example:</span></span>

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a><span data-ttu-id="27be5-117">Zakázání pravidla</span><span class="sxs-lookup"><span data-stu-id="27be5-117">Disable rules</span></span>

<span data-ttu-id="27be5-118">Následující příklad zakazuje pravidla `910018` a `910017` na aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="27be5-118">The following example disables rules `910018` and `910017` on an application gateway:</span></span>

```azurecli
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a><span data-ttu-id="27be5-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27be5-119">Next steps</span></span>

<span data-ttu-id="27be5-120">Po dokončení konfigurace zakázaná pravidla, můžete naučit k zobrazení protokolů firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="27be5-120">After you configure your disabled rules, you can learn how to view your WAF logs.</span></span> <span data-ttu-id="27be5-121">Další informace najdete v tématu [diagnostiku brány aplikace](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="27be5-121">For more information, see [Application Gateway Diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
