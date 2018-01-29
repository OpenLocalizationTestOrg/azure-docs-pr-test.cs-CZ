---
title: "Chyby registrace zprostředkovatele prostředků Azure | Microsoft Docs"
description: "Popisuje, jak vyřešit chyby registrace zprostředkovatele prostředků Azure."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: d6a99917e732a3439a31cafa5608348694014054
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Vyřešte chyby pro registrace zprostředkovatele prostředků

Tento článek popisuje chyby, ke kterým může dojít při použití zprostředkovatele prostředků, který jste dříve nepoužili ve vašem předplatném.

## <a name="symptom"></a>Příznaky

Při nasazování prostředků, může se zobrazit následující kód chyby a zpráva:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Nebo se může zobrazit podobné zpráva, že:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

## <a name="cause"></a>Příčina

Zobrazí tyto chyby pro jednu ze tří důvodů:

1. Zprostředkovatel prostředků není zaregistrován pro vaše předplatné
1. Verze rozhraní API není podporována pro typ prostředku
1. Umístění není podporováno pro typ prostředku

## <a name="solution"></a>Řešení

Chybová zpráva by měla dát návrhy na podporovaném umístění a verze rozhraní API. Šablony můžete změnit na jednu z Tyhle navrhované hodnoty. Většina poskytovatelé jsou registrované automaticky pomocí portálu Azure nebo rozhraní příkazového řádku, který používáte, ale ne všechny. Pokud jste nepoužili poskytovatele určitému prostředku před, musíte k registraci tohoto zprostředkovatele. Může zjišťovat informace o poskytovatelích prostředků prostřednictvím PowerShell nebo rozhraní příkazového řádku Azure.

### <a name="solution-1"></a>Řešení 1

Pro prostředí PowerShell, použijte **Get-AzureRmResourceProvider** zobrazíte stav registrace.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Chcete-li zaregistrovat zprostředkovatele, použijte **Register-AzureRmResourceProvider** a zadejte název zprostředkovatele prostředků, které chcete zaregistrovat.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

Chcete-li získat podporovaná umístění pro konkrétní typ prostředku, použijte:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Podporované verze rozhraní API pro konkrétní typ prostředku, použijte:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

### <a name="solution-2"></a>Řešení 2

**Azure CLI**

Chcete-li zjistit, zda zprostředkovatel registroval, použijte `az provider list` příkaz.

```azurecli-interactive
az provider list
```

Registrace poskytovatele prostředků, použijte `az provider register` příkaz a zadejte *obor názvů* k registraci.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Pokud chcete zobrazit podporovaná umístění a verze rozhraní API pro typ prostředku, použijte:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

### <a name="solution-3"></a>Řešení 3

Můžete zobrazit stav registrace a zaregistrovat obor názvů zprostředkovatele prostředků prostřednictvím portálu.

1. Pro vaše předplatné, vyberte **zprostředkovatelé prostředků**.

   ![Vyberte poskytovatele prostředků](./media/resource-manager-register-provider-errors/select-resource-provider.png)

1. Podívejte se na seznam poskytovatelů prostředků a v případě potřeby vyberte **zaregistrovat** odkaz k registraci poskytovatele prostředků typu, který se snažíte nasadit.

   ![Zobrazit seznam poskytovatelů prostředků](./media/resource-manager-register-provider-errors/list-resource-providers.png)
