---
title: "Nasazení prostředků pomocí rozhraní API REST a šablony | Microsoft Docs"
description: "Nasazení prostředky do Azure pomocí Azure Resource Manageru a REST API Resource Manageru. Prostředky jsou definovány v šabloně Resource Manageru."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: tomfitz
ms.openlocfilehash: 46856a25fb57bb2c5a3c1aeae13c11655e1a58a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Nasazení prostředků pomocí šablon Resource Manageru a jeho rozhraní REST API
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Azure Portal](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

Tento článek vysvětluje, jak používat rozhraní REST API služby Správce prostředků pomocí šablony Resource Manageru k nasazení vašich prostředků Azure.  

> [!TIP]
> Pomoc s laděním chybu během nasazení najdete v tématu:
> 
> * [Zobrazit operace nasazení](resource-manager-deployment-operations.md) Další informace o získání informací, která pomáhá odstranění vaší chyby
> * [Odstraňování běžných chyb při nasazování prostředků do Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md) Další informace o řešení běžných chyb při nasazení
> 
> 

Šablony může být místní soubor nebo externí soubor, který je k dispozici prostřednictvím identifikátoru URI. Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup k šabloně a během nasazení zadat token sdílený přístupový podpis (SAS).

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Nasazení pomocí rozhraní REST API
1. Nastavit [společné parametry a hlavičky](https://docs.microsoft.com/rest/api/index), včetně ověřování tokenů.
2. Pokud nemáte existující skupinu prostředků, vytvořte skupinu prostředků. Zadejte svoje ID předplatného, název nové skupiny prostředků a umístění, které potřebujete pro vaše řešení. Další informace najdete v tématu [vytvořte skupinu prostředků](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. Ověření nasazení před provedením spuštěním [ověření nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operaci. Při testování nasazení, zadejte parametry přesně stejně jako při provádění nasazení (zobrazené v dalším kroku).
4. Vytvořte nasazení. Zadejte svoje ID předplatného, název skupiny prostředků, název nasazení a odkaz na šablonu. Informace o souboru šablony najdete v tématu [soubor parametrů](#parameter-file). Další informace o rozhraní REST API pro vytvoření skupiny prostředků najdete v tématu [vytvořit šablonu nasazení](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). Upozornění **režimu** je nastaven na **přírůstkové**. Chcete-li spustit kompletní nasazení, nastavte **režimu** k **Complete**. Buďte opatrní při používání dokončení režimu jako nechtěně může odstranit prostředky, které nejsou ve vaší šabloně.
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0"
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0"
              }
            }
          }
   
      Pokud chcete protokolovat obsah odpovědi, obsah žádosti nebo obojí, zahrnout **debugSetting** v požadavku.
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      Můžete nastavit účtu úložiště používat token sdílený přístupový podpis (SAS). Další informace najdete v tématu [delegování přístupu k pomocí sdíleného přístupového podpisu](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).
5. Získáte stav nasazení šablony. Další informace najdete v tématu [získat informace o nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a>Soubor parametrů

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Další kroky
* Další informace o zpracování asynchronní operace REST najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).
* Příklad nasazení prostředků prostřednictvím klientské knihovny .NET, naleznete v části [nasazení prostředků s použitím knihovny .NET a šablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Chcete-li definovat parametry v šabloně, přečtěte si téma [vytváření šablon](resource-group-authoring-templates.md#parameters).
* Pokyny pro nasazení řešení do různých prostředí najdete v článku věnovaném [testovacím a vývojovým prostředím v Microsoft Azure](solution-dev-test-environments.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

