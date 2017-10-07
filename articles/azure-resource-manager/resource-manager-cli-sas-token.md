---
title: "aaaDeploy šablony Azure s tokenu SAS a rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Azure Resource Manageru a rozhraní příkazového řádku Azure toodeploy tooAzure prostředky ze šablony, která je chráněný pomocí tokenu SAS."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 59c64616d6e1f5e456d88a72854d0ed99e1bdc0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a>Nasazení privátní šablony Resource Manageru pomocí tokenu SAS a rozhraní příkazového řádku Azure

Pokud vaše šablona se nachází v účtu úložiště, můžete omezit přístup toohello šablony a během nasazení zadat token sdílený přístupový podpis (SAS). Toto téma vysvětluje, jak toouse prostředí Azure PowerShell s Resource Manager šablony tooprovide token SAS během nasazení. 

## <a name="add-private-template-toostorage-account"></a>Přidat účet toostorage privátní šablony

Můžete přidat váš účet úložiště tooa šablony a propojit toothem během nasazení s tokenem SAS.

> [!IMPORTANT]
> Podle následujících kroků hello hello blob obsahující hello šablony je přístupný tooonly vlastníka účtu hello. Při vytváření tokenu SAS pro objekt blob hello je hello blob však přístupné tooanyone s Tento identifikátor URI. Pokud jiný uživatel zabrání hello identifikátor URI, je tento uživatel moct tooaccess hello šablony. Použití SAS token je vhodný způsob omezení přístupu tooyour šablony, ale nesmí obsahovat citlivá data, jako jsou hesla přímo v šabloně hello.
> 
> 

Hello následující příklad nastaví kontejner privátní úložiště účet a odesílá šablonu:
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a>Během nasazení zadat tokenu SAS
toodeploy šablonu privátní v účtu úložiště, vygenerování tokenu SAS a její zahrnutí do hello identifikátor URI pro šablonu hello. Nastavte tooallow čas vypršení platnosti hello dostatek času toocomplete hello nasazení.
   
```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

Příklad použití tokenu SAS s propojených šablon naleznete v části [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).

## <a name="next-steps"></a>Další kroky
* Úvod toodeploying šablony, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy-cli.md).
* Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript šablony nasazení Resource Manager](resource-manager-samples-cli-deploy.md)
* toodefine parametry v šabloně, najdete v části [vytváření šablon](resource-group-authoring-templates.md#parameters).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).
