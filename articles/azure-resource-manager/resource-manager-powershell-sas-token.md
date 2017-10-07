---
title: "aaaDeploy šablony Azure s tokenu SAS a prostředí PowerShell | Microsoft Docs"
description: "Azure Resource Manageru a prostředí Azure PowerShell toodeploy tooAzure prostředky ze šablony, která je chráněný pomocí tokenu SAS."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: b95e096591d6213f8ef79235c8cd85705c4b79ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a>Nasazení privátní šablony Resource Manageru pomocí tokenu SAS a prostředí Azure PowerShell

Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup toohello šablony a během nasazení zadat token sdílený přístupový podpis (SAS). Toto téma vysvětluje, jak toouse prostředí Azure PowerShell s Resource Manager šablony tooprovide token SAS během nasazení. 

## <a name="add-private-template-toostorage-account"></a>Přidat účet toostorage privátní šablony

Můžete přidat váš účet úložiště tooa šablony a propojit toothem během nasazení s tokenem SAS.

> [!IMPORTANT]
> Podle následujících kroků hello hello blob obsahující hello šablony je přístupný tooonly vlastníka účtu hello. Při vytváření tokenu SAS pro objekt blob hello je hello blob však přístupné tooanyone s Tento identifikátor URI. Pokud jiný uživatel zabrání hello identifikátor URI, je tento uživatel moct tooaccess hello šablony. Použití SAS token je vhodný způsob omezení přístupu tooyour šablony, ale nesmí obsahovat citlivá data, jako jsou hesla přímo v šabloně hello.
> 
> 

Hello následující příklad nastaví kontejner privátní úložiště účet a odesílá šablonu:
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a>Během nasazení zadat tokenu SAS
toodeploy šablonu privátní v účtu úložiště, vygenerování tokenu SAS a její zahrnutí do hello identifikátor URI pro šablonu hello. Nastavte tooallow čas vypršení platnosti hello dostatek času toocomplete hello nasazení.
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get hello URI with hello SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

Příklad použití tokenu SAS s propojených šablon naleznete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).


## <a name="next-steps"></a>Další kroky
* Úvod toodeploying šablony, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md).
* Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript šablony nasazení Resource Manager](resource-manager-samples-powershell-deploy.md)
* toodefine parametry v šabloně, najdete v části [vytváření šablon](resource-group-authoring-templates.md#parameters).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

