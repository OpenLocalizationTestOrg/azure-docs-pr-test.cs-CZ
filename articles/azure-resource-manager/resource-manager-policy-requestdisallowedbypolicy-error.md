---
title: "Chyba RequestDisallowedByPolicy zásadám prostředků Azure | Microsoft Docs"
description: "Popisuje příčinou chyby RequestDisallowedByPolicy."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: genli
ms.openlocfilehash: 182a27e444c2f5db66d518a1a0c608d3e319d553
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Chyba RequestDisallowedByPolicy zásadám prostředků Azure.

Tento článek popisuje příčinou chyby RequestDisallowedByPolicy, poskytuje také řešení této chyby.

## <a name="symptom"></a>Příznaky

Když se pokusíte provést akci při nasazení, může dojít **RequestDisallowedByPolicy** chybu a nemůže tuto akci provést. Zde je ukázka chyby:

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Řešení potíží

Načíst podrobnosti o zásady, které blokované vaše nasazení, použijte následující metody:

### <a name="method-1"></a>Metoda 1

V prostředí PowerShell, zadejte tento identifikátor zásady, jako **Id** parametr načíst podrobnosti o zásady, které blokované vaše nasazení.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>Metoda 2 

V Azure CLI 2.0 zadejte název definice zásady: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Řešení

Pro zabezpečení nebo dodržování předpisů může oddělení IT vynutit zásady prostředků, které brání vytvoření veřejné IP adresy, skupiny zabezpečení sítě, trasy definované uživatelem nebo směrovací tabulky. V ukázce chybovou zprávu, která je popsaná v části "Příznaky" zásady jmenuje **regionPolicyDefinition**, ale může být odlišné.
Chcete-li vyřešit tento problém, pracovat vaše IT oddělení zkontrolovat zásady prostředků a určit, jak se provést požadovanou akci souladu s těmito zásadami.


Další informace najdete v následujících článcích:

- [Přehled zásad prostředků](resource-manager-policy.md)
- [Běžné chyby RequestDisallowedByPolicy nasazení](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


