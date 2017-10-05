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
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a><span data-ttu-id="40764-103">Chyba RequestDisallowedByPolicy zásadám prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="40764-103">RequestDisallowedByPolicy error with Azure resource policy</span></span>

<span data-ttu-id="40764-104">Tento článek popisuje příčinou chyby RequestDisallowedByPolicy, poskytuje také řešení této chyby.</span><span class="sxs-lookup"><span data-stu-id="40764-104">This article describes the cause of the RequestDisallowedByPolicy error, it also provides solution for this error.</span></span>

## <a name="symptom"></a><span data-ttu-id="40764-105">Příznaky</span><span class="sxs-lookup"><span data-stu-id="40764-105">Symptom</span></span>

<span data-ttu-id="40764-106">Když se pokusíte provést akci při nasazení, může dojít **RequestDisallowedByPolicy** chybu a nemůže tuto akci provést.</span><span class="sxs-lookup"><span data-stu-id="40764-106">When you try to do an action during deployment, you might receive a **RequestDisallowedByPolicy** error that prevents the action be performed.</span></span> <span data-ttu-id="40764-107">Zde je ukázka chyby:</span><span class="sxs-lookup"><span data-stu-id="40764-107">The following is a sample of the error:</span></span>

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a><span data-ttu-id="40764-108">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="40764-108">Troubleshooting</span></span>

<span data-ttu-id="40764-109">Načíst podrobnosti o zásady, které blokované vaše nasazení, použijte následující metody:</span><span class="sxs-lookup"><span data-stu-id="40764-109">To retrieve details about the policy that blocked your deployment, use the following one of the methods:</span></span>

### <a name="method-1"></a><span data-ttu-id="40764-110">Metoda 1</span><span class="sxs-lookup"><span data-stu-id="40764-110">Method 1</span></span>

<span data-ttu-id="40764-111">V prostředí PowerShell, zadejte tento identifikátor zásady, jako **Id** parametr načíst podrobnosti o zásady, které blokované vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="40764-111">In PowerShell, provide that policy identifier as the **Id** parameter to retrieve details about the policy that blocked your deployment.</span></span>

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a><span data-ttu-id="40764-112">Metoda 2</span><span class="sxs-lookup"><span data-stu-id="40764-112">Method 2</span></span> 

<span data-ttu-id="40764-113">V Azure CLI 2.0 zadejte název definice zásady:</span><span class="sxs-lookup"><span data-stu-id="40764-113">In Azure CLI 2.0, provide the name of the policy definition:</span></span> 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a><span data-ttu-id="40764-114">Řešení</span><span class="sxs-lookup"><span data-stu-id="40764-114">Solution</span></span>

<span data-ttu-id="40764-115">Pro zabezpečení nebo dodržování předpisů může oddělení IT vynutit zásady prostředků, které brání vytvoření veřejné IP adresy, skupiny zabezpečení sítě, trasy definované uživatelem nebo směrovací tabulky.</span><span class="sxs-lookup"><span data-stu-id="40764-115">For security or compliance, your IT department might enforce a resource policy that prohibits creating Public IP addresses, Network Security Groups, User-Defined Routes, or route tables.</span></span> <span data-ttu-id="40764-116">V ukázce chybovou zprávu, která je popsaná v části "Příznaky" zásady jmenuje **regionPolicyDefinition**, ale může být odlišné.</span><span class="sxs-lookup"><span data-stu-id="40764-116">In the sample of the error message that is described in the "Symptoms" section, the policy is named **regionPolicyDefinition**, but it could be different.</span></span>
<span data-ttu-id="40764-117">Chcete-li vyřešit tento problém, pracovat vaše IT oddělení zkontrolovat zásady prostředků a určit, jak se provést požadovanou akci souladu s těmito zásadami.</span><span class="sxs-lookup"><span data-stu-id="40764-117">To resolve this problem, work with your IT department to review the resource policies, and determine how to perform the requested action in compliance with those policies.</span></span>


<span data-ttu-id="40764-118">Další informace najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="40764-118">For more information, see the following articles:</span></span>

- [<span data-ttu-id="40764-119">Přehled zásad prostředků</span><span class="sxs-lookup"><span data-stu-id="40764-119">Resource policy overview</span></span>](resource-manager-policy.md)
- [<span data-ttu-id="40764-120">Běžné chyby RequestDisallowedByPolicy nasazení</span><span class="sxs-lookup"><span data-stu-id="40764-120">Common deployment errors-RequestDisallowedByPolicy</span></span>](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


