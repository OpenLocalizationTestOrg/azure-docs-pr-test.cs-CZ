---
title: "aaaDeploy prostředků pomocí rozhraní API REST a šablony | Microsoft Docs"
description: "Pomocí Azure Resource Manageru a REST API Resource Manageru toodeploy tooAzure prostředky. Hello prostředky jsou definovány v šabloně Resource Manager."
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
ms.openlocfilehash: 347ad3bdb604429e7291297d448688204af69b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Nasazení prostředků pomocí šablon Resource Manageru a jeho rozhraní REST API
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Azure Portal](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

Tento článek vysvětluje, jak toouse hello REST API Resource Manager pomocí Správce prostředků šablony toodeploy tooAzure vaše prostředky.  

> [!TIP]
> Pomoc s laděním chybu během nasazení najdete v tématu:
> 
> * [Zobrazit operace nasazení](resource-manager-deployment-operations.md) toolearn o získání informací, která vám pomůže vyřešit vaše chybu
> * [Odstraňování běžných chyb při nasazování tooAzure prostředků pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md) toolearn jak tooresolve běžné chyby nasazení
> 
> 

Šablony může být místní soubor nebo externí soubor, který je k dispozici prostřednictvím identifikátoru URI. Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup toohello šablony a během nasazení zadat token sdílený přístupový podpis (SAS).

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-hello-rest-api"></a>Nasazení s hello REST API
1. Nastavit [společné parametry a hlavičky](https://docs.microsoft.com/rest/api/index), včetně ověřování tokenů.
2. Pokud nemáte existující skupinu prostředků, vytvořte skupinu prostředků. Zadejte ID předplatného, název hello hello novou skupinu prostředků a umístění, které potřebujete pro vaše řešení. Další informace najdete v tématu [vytvořte skupinu prostředků](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. Ověření nasazení před provedením spuštěním hello [ověření nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operaci. Při testování hello nasazení, zadejte parametry přesně stejně jako při provádění nasazení hello (zobrazené v dalším kroku hello).
4. Vytvořte nasazení. Zadejte svoje ID předplatného, hello název skupiny prostředků hello, název hello hello nasazení a šablonu tooyour odkaz. Informace o souboru šablony hello najdete v tématu [soubor parametrů](#parameter-file). Další informace o rozhraní REST API toocreate hello skupinu prostředků najdete v tématu [vytvořit šablonu nasazení](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). Všimněte si hello **režimu** je nastaven příliš**přírůstkové**. toorun dokončení nasazení, nastavte **režimu** příliš**Complete**. Dávejte pozor při použití režimu dokončení hello jako nechtěně může odstranit prostředky, které nejsou ve vaší šabloně.
   
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
   
      Pokud chcete obsah odpovědi toolog, obsah žádosti nebo obojí, zahrnují **debugSetting** v žádosti o hello.
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      Můžete nastavit vaše toouse účet úložiště token sdílený přístupový podpis (SAS). Další informace najdete v tématu [delegování přístupu k pomocí sdíleného přístupového podpisu](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).
5. Získáte stav hello hello šablonu nasazení. Další informace najdete v tématu [získat informace o nasazení šablony](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a>Soubor parametrů

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Další kroky
* toolearn o zpracování asynchronní operace REST, najdete v části [sledovat asynchronní operace Azure](resource-manager-async-operations.md).
* Příklad nasazení prostředků prostřednictvím klientské knihovny hello .NET, naleznete v části [nasazení prostředků s použitím knihovny .NET a šablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* toodefine parametry v šabloně, najdete v části [vytváření šablon](resource-group-authoring-templates.md#parameters).
* Pokyny k nasazení vašeho prostředí toodifferent řešení najdete v tématu [vývojová a testovací prostředí v Microsoft Azure](solution-dev-test-environments.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

