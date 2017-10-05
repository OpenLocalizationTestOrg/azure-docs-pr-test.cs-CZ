---
title: "Přizpůsobení pravidel brány firewall webových aplikací v Azure Application Gateway - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek obsahuje informace o tom, jak přizpůsobit pravidla brány firewall webových aplikací v Application Gateway pomocí Azure CLI 2.0."
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
ms.openlocfilehash: 456be048dc2d82cd50d145b71f17a84a7189ea96
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="customize-web-application-firewall-rules-through-the-azure-cli-20"></a><span data-ttu-id="4d36f-103">Přizpůsobení pravidla brány firewall webových aplikací pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4d36f-103">Customize web application firewall rules through the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4d36f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4d36f-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="4d36f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d36f-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="4d36f-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4d36f-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="4d36f-107">Azure Application Gateway brány firewall webových aplikací (firewall webových aplikací) poskytuje ochranu pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4d36f-107">The Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="4d36f-108">Tyto ochrany jsou poskytovány pomocí aplikace otevřete webový projekt zabezpečení (OWASP) základní pravidlo nastavit CRS ().</span><span class="sxs-lookup"><span data-stu-id="4d36f-108">These protections are provided by the Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="4d36f-109">Některá pravidla můžete způsobit falešně pozitivních zjištění a blokování skutečné provozu.</span><span class="sxs-lookup"><span data-stu-id="4d36f-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="4d36f-110">Z tohoto důvodu Application Gateway poskytuje schopnost přizpůsobit skupiny pravidla a pravidla.</span><span class="sxs-lookup"><span data-stu-id="4d36f-110">For this reason, Application Gateway provides the capability to customize rule groups and rules.</span></span> <span data-ttu-id="4d36f-111">Další informace o konkrétní pravidlo skupiny a pravidel najdete v tématu [seznam webových aplikací brány firewall řádku pravidlo skupiny a pravidel](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="4d36f-111">For more information on the specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="4d36f-112">Zobrazit skupiny pravidla a pravidla</span><span class="sxs-lookup"><span data-stu-id="4d36f-112">View rule groups and rules</span></span>

<span data-ttu-id="4d36f-113">Následující příklady kódu ukazují, jak zobrazit pravidla a pravidla skupiny, které se dají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="4d36f-113">The following code examples show how to view rules and rule groups that are configurable.</span></span>

### <a name="view-rule-groups"></a><span data-ttu-id="4d36f-114">Zobrazení pravidla skupiny</span><span class="sxs-lookup"><span data-stu-id="4d36f-114">View rule groups</span></span>

<span data-ttu-id="4d36f-115">Následující příklad ukazuje, jak chcete-li zobrazit skupiny pravidel:</span><span class="sxs-lookup"><span data-stu-id="4d36f-115">The following example shows how to view the rule groups:</span></span>

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

<span data-ttu-id="4d36f-116">Tento výstup je oříznuta odpovědí z předchozího příkladu:</span><span class="sxs-lookup"><span data-stu-id="4d36f-116">The following output is a truncated response from the preceding example:</span></span>

```
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  },
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_2.2.9",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
   "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "crs_20_protocol_violations",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "2.2.9",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

### <a name="view-rules-in-a-rule-group"></a><span data-ttu-id="4d36f-117">Zobrazení pravidel skupiny pravidla</span><span class="sxs-lookup"><span data-stu-id="4d36f-117">View rules in a rule group</span></span>

<span data-ttu-id="4d36f-118">Následující příklad ukazuje, jak chcete zobrazit pravidla ve skupině zadaným pravidlem:</span><span class="sxs-lookup"><span data-stu-id="4d36f-118">The following example shows how to view rules in a specified rule group:</span></span>

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

<span data-ttu-id="4d36f-119">Tento výstup je oříznuta odpovědí z předchozího příkladu:</span><span class="sxs-lookup"><span data-stu-id="4d36f-119">The following output is a truncated response from the preceding example:</span></span>

```
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": [
          {
            "description": "Rule 910011",
            "ruleId": 910011
          },
          ...
        ]
      }
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

## <a name="disable-rules"></a><span data-ttu-id="4d36f-120">Zakázání pravidla</span><span class="sxs-lookup"><span data-stu-id="4d36f-120">Disable rules</span></span>

<span data-ttu-id="4d36f-121">Následující příklad zakazuje pravidla `910018` a `910017` na aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="4d36f-121">The following example disables rules `910018` and `910017` on an application gateway:</span></span>

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a><span data-ttu-id="4d36f-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d36f-122">Next steps</span></span>

<span data-ttu-id="4d36f-123">Po dokončení konfigurace zakázaná pravidla, můžete naučit k zobrazení protokolů firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="4d36f-123">After you configure your disabled rules, you can learn how to view your WAF logs.</span></span> <span data-ttu-id="4d36f-124">Další informace najdete v tématu [diagnostics Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="4d36f-124">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
