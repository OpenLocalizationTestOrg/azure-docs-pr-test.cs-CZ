---
title: "aaaCustomize webové aplikace pravidla brány firewall v Azure Application Gateway - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek obsahuje informace o tom, jak pravidla brány firewall webových aplikací toocustomize v aplikační brány s hello 2.0 rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: b83ffb9f6a7e0d0c8c970885d2bcb3b63d32581c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-hello-azure-cli-20"></a><span data-ttu-id="cceac-103">Přizpůsobení pravidel brány firewall webových aplikací prostřednictvím hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="cceac-103">Customize web application firewall rules through hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="cceac-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cceac-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="cceac-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cceac-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="cceac-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="cceac-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="cceac-107">brány firewall webových aplikací Hello Azure Application Gateway (firewall webových aplikací) poskytuje ochranu pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cceac-107">hello Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="cceac-108">Tyto ochrany jsou poskytovány hello aplikace otevřete webový projekt zabezpečení (OWASP) nastavit pravidlo jádra (řádku).</span><span class="sxs-lookup"><span data-stu-id="cceac-108">These protections are provided by hello Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="cceac-109">Některá pravidla můžete způsobit falešně pozitivních zjištění a blokování skutečné provozu.</span><span class="sxs-lookup"><span data-stu-id="cceac-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="cceac-110">Z tohoto důvodu Application Gateway poskytuje hello schopností toocustomize pravidlo skupiny a pravidel.</span><span class="sxs-lookup"><span data-stu-id="cceac-110">For this reason, Application Gateway provides hello capability toocustomize rule groups and rules.</span></span> <span data-ttu-id="cceac-111">Další informace o hello konkrétní pravidlo skupiny a pravidel najdete v tématu [seznam webových aplikací brány firewall řádku pravidlo skupiny a pravidel](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="cceac-111">For more information on hello specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="cceac-112">Zobrazit skupiny pravidla a pravidla</span><span class="sxs-lookup"><span data-stu-id="cceac-112">View rule groups and rules</span></span>

<span data-ttu-id="cceac-113">Hello následující příklady kódu ukazují, jak tooview pravidla a pravidla skupiny, které se dají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="cceac-113">hello following code examples show how tooview rules and rule groups that are configurable.</span></span>

### <a name="view-rule-groups"></a><span data-ttu-id="cceac-114">Zobrazení pravidla skupiny</span><span class="sxs-lookup"><span data-stu-id="cceac-114">View rule groups</span></span>

<span data-ttu-id="cceac-115">Hello následující příklad ukazuje, jak tooview hello skupiny pravidel:</span><span class="sxs-lookup"><span data-stu-id="cceac-115">hello following example shows how tooview hello rule groups:</span></span>

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

<span data-ttu-id="cceac-116">Následující výstup Hello je oříznuta odpovědí z hello předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="cceac-116">hello following output is a truncated response from hello preceding example:</span></span>

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

### <a name="view-rules-in-a-rule-group"></a><span data-ttu-id="cceac-117">Zobrazení pravidel skupiny pravidla</span><span class="sxs-lookup"><span data-stu-id="cceac-117">View rules in a rule group</span></span>

<span data-ttu-id="cceac-118">Hello následující příklad ukazuje, jak tooview pravidel ve skupině zadaným pravidlem:</span><span class="sxs-lookup"><span data-stu-id="cceac-118">hello following example shows how tooview rules in a specified rule group:</span></span>

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

<span data-ttu-id="cceac-119">Následující výstup Hello je oříznuta odpovědí z hello předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="cceac-119">hello following output is a truncated response from hello preceding example:</span></span>

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

## <a name="disable-rules"></a><span data-ttu-id="cceac-120">Zakázání pravidla</span><span class="sxs-lookup"><span data-stu-id="cceac-120">Disable rules</span></span>

<span data-ttu-id="cceac-121">Hello následující příklad zakazuje pravidla `910018` a `910017` na aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="cceac-121">hello following example disables rules `910018` and `910017` on an application gateway:</span></span>

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a><span data-ttu-id="cceac-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cceac-122">Next steps</span></span>

<span data-ttu-id="cceac-123">Po dokončení konfigurace zakázaná pravidla, dozvíte, jak tooview protokolů firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="cceac-123">After you configure your disabled rules, you can learn how tooview your WAF logs.</span></span> <span data-ttu-id="cceac-124">Další informace najdete v tématu [diagnostics Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="cceac-124">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
