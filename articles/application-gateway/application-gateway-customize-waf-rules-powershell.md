---
title: "Přizpůsobení pravidel brány firewall webových aplikací v Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Tento článek obsahuje informace o tom, jak přizpůsobit pravidla brány firewall webových aplikací v Application Gateway pomocí prostředí PowerShell."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 4595864a7bc624375ba2ff6ace09ebae5b0f843a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2018
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>Přizpůsobení pravidel brány firewall webových aplikací pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> * [portál Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

Azure Application Gateway brány firewall webových aplikací (firewall webových aplikací) poskytuje ochranu pro webové aplikace. Tyto ochrany jsou poskytovány pomocí aplikace otevřete webový projekt zabezpečení (OWASP) základní pravidlo nastavit CRS (). Některá pravidla můžete způsobit falešně pozitivních zjištění a blokování skutečné provozu. Z tohoto důvodu Application Gateway poskytuje schopnost přizpůsobit skupiny pravidla a pravidla. Další informace o konkrétní pravidlo skupiny a pravidel najdete v tématu [seznam skupin řádku pravidlo brány firewall webových aplikací a pravidla](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Zobrazit skupiny pravidla a pravidla

Následující příklady kódu ukazují, jak zobrazit pravidla a pravidla skupiny, které se dají konfigurovat na bránu firewall webových aplikací s podporou aplikace.

### <a name="view-rule-groups"></a>Zobrazení pravidla skupiny

Následující příklad ukazuje, jak chcete-li zobrazit skupiny pravidel:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

Tento výstup je oříznuta odpovědí z předchozího příkladu:

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

## <a name="disable-rules"></a>Zakázání pravidla

Následující příklad zakazuje pravidla `910018` a `910017` na aplikační brány:

```powershell
$disabledrules=New-AzureRmApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-910-IP-REPUTATION -Rules 910018,910017
Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
```

## <a name="next-steps"></a>Další postup

Po dokončení konfigurace zakázaná pravidla, můžete naučit k zobrazení protokolů firewall webových aplikací. Další informace najdete v tématu [diagnostiku brány aplikace](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
