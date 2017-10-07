---
title: "Chyba aaaRequestDisallowedByPolicy zásadám prostředků Azure | Microsoft Docs"
description: "Popisuje hello příčinu hello RequestDisallowedByPolicy chyby."
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
ms.openlocfilehash: 7870e40205cf433ccb4ba02376b5fe809f20d0df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Chyba RequestDisallowedByPolicy zásadám prostředků Azure.

Tento článek popisuje hello příčinu chyby RequestDisallowedByPolicy hello, poskytuje také řešení této chyby.

## <a name="symptom"></a>Příznaky

Při pokusu o toodo akce během nasazení, může dojít **RequestDisallowedByPolicy** provést, chyba, která zabraňuje hello akce. Hello Následuje ukázka hello chyby:

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Řešení potíží

tooretrieve podrobnosti o hello zásad, který blokovaný vaše nasazení, použijte jednu z metod hello následující hello:

### <a name="method-1"></a>Metoda 1

V prostředí PowerShell, zadejte identifikátor této zásady jako hello **Id** parametr tooretrieve podrobnosti o hello zásady, které blokované vaše nasazení.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>Metoda 2 

V Azure CLI 2.0 zadejte název definice zásady hello hello: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Řešení

Pro zabezpečení nebo dodržování předpisů může oddělení IT vynutit zásady prostředků, které brání vytvoření veřejné IP adresy, skupiny zabezpečení sítě, trasy definované uživatelem nebo směrovací tabulky. V ukázce hello hello chybová zpráva, která je popsána v části "Příznaky" hello, hello zásad jmenuje **regionPolicyDefinition**, ale může být odlišné.
tooresolve tento problém pracovat zásady prostředků tooreview hello vaše IT oddělení a určit, jak tooperform hello požadované akce v souladu s těmito zásadami.


Další informace najdete v tématu hello následující články:

- [Přehled zásad prostředků](resource-manager-policy.md)
- [Běžné chyby RequestDisallowedByPolicy nasazení](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


