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
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a><span data-ttu-id="6f796-103">Chyba RequestDisallowedByPolicy zásadám prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6f796-103">RequestDisallowedByPolicy error with Azure resource policy</span></span>

<span data-ttu-id="6f796-104">Tento článek popisuje hello příčinu chyby RequestDisallowedByPolicy hello, poskytuje také řešení této chyby.</span><span class="sxs-lookup"><span data-stu-id="6f796-104">This article describes hello cause of hello RequestDisallowedByPolicy error, it also provides solution for this error.</span></span>

## <a name="symptom"></a><span data-ttu-id="6f796-105">Příznaky</span><span class="sxs-lookup"><span data-stu-id="6f796-105">Symptom</span></span>

<span data-ttu-id="6f796-106">Při pokusu o toodo akce během nasazení, může dojít **RequestDisallowedByPolicy** provést, chyba, která zabraňuje hello akce.</span><span class="sxs-lookup"><span data-stu-id="6f796-106">When you try toodo an action during deployment, you might receive a **RequestDisallowedByPolicy** error that prevents hello action be performed.</span></span> <span data-ttu-id="6f796-107">Hello Následuje ukázka hello chyby:</span><span class="sxs-lookup"><span data-stu-id="6f796-107">hello following is a sample of hello error:</span></span>

```
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"hello resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a><span data-ttu-id="6f796-108">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6f796-108">Troubleshooting</span></span>

<span data-ttu-id="6f796-109">tooretrieve podrobnosti o hello zásad, který blokovaný vaše nasazení, použijte jednu z metod hello následující hello:</span><span class="sxs-lookup"><span data-stu-id="6f796-109">tooretrieve details about hello policy that blocked your deployment, use hello following one of hello methods:</span></span>

### <a name="method-1"></a><span data-ttu-id="6f796-110">Metoda 1</span><span class="sxs-lookup"><span data-stu-id="6f796-110">Method 1</span></span>

<span data-ttu-id="6f796-111">V prostředí PowerShell, zadejte identifikátor této zásady jako hello **Id** parametr tooretrieve podrobnosti o hello zásady, které blokované vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="6f796-111">In PowerShell, provide that policy identifier as hello **Id** parameter tooretrieve details about hello policy that blocked your deployment.</span></span>

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a><span data-ttu-id="6f796-112">Metoda 2</span><span class="sxs-lookup"><span data-stu-id="6f796-112">Method 2</span></span> 

<span data-ttu-id="6f796-113">V Azure CLI 2.0 zadejte název definice zásady hello hello:</span><span class="sxs-lookup"><span data-stu-id="6f796-113">In Azure CLI 2.0, provide hello name of hello policy definition:</span></span> 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a><span data-ttu-id="6f796-114">Řešení</span><span class="sxs-lookup"><span data-stu-id="6f796-114">Solution</span></span>

<span data-ttu-id="6f796-115">Pro zabezpečení nebo dodržování předpisů může oddělení IT vynutit zásady prostředků, které brání vytvoření veřejné IP adresy, skupiny zabezpečení sítě, trasy definované uživatelem nebo směrovací tabulky.</span><span class="sxs-lookup"><span data-stu-id="6f796-115">For security or compliance, your IT department might enforce a resource policy that prohibits creating Public IP addresses, Network Security Groups, User-Defined Routes, or route tables.</span></span> <span data-ttu-id="6f796-116">V ukázce hello hello chybová zpráva, která je popsána v části "Příznaky" hello, hello zásad jmenuje **regionPolicyDefinition**, ale může být odlišné.</span><span class="sxs-lookup"><span data-stu-id="6f796-116">In hello sample of hello error message that is described in hello "Symptoms" section, hello policy is named **regionPolicyDefinition**, but it could be different.</span></span>
<span data-ttu-id="6f796-117">tooresolve tento problém pracovat zásady prostředků tooreview hello vaše IT oddělení a určit, jak tooperform hello požadované akce v souladu s těmito zásadami.</span><span class="sxs-lookup"><span data-stu-id="6f796-117">tooresolve this problem, work with your IT department tooreview hello resource policies, and determine how tooperform hello requested action in compliance with those policies.</span></span>


<span data-ttu-id="6f796-118">Další informace najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="6f796-118">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="6f796-119">Přehled zásad prostředků</span><span class="sxs-lookup"><span data-stu-id="6f796-119">Resource policy overview</span></span>](resource-manager-policy.md)
- [<span data-ttu-id="6f796-120">Běžné chyby RequestDisallowedByPolicy nasazení</span><span class="sxs-lookup"><span data-stu-id="6f796-120">Common deployment errors-RequestDisallowedByPolicy</span></span>](resource-manager-common-deployment-errors.md#requestdisallowedbypolicy)

 


