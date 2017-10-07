---
title: aaaAzure kontejneru registru webhooky | Microsoft Docs
description: Kontejner Azure registru webhooky
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: Docker, kontejnery, ACR
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2017
ms.author: nepeters
ms.openlocfilehash: adc2afec486007e2d54cd689e6f7ef8b1098db06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-container-registry-webhooks---azure-portal"></a>Pomocí webhooků registru kontejner Azure – portál Azure

Azure kontejneru registru uchovává a spravuje privátní Docker kontejner imagí, podobné toohello způsob úložiště Docker Hub ukládá veřejné imagí Dockeru. Použijete webhooky tootrigger událostí v případě, že některé akce provést v jednom z vašeho úložiště registru. Webhooky může reagovat tooevents na úrovni registru hello nebo může být obor dolů značky tooa konkrétní úložiště. 

Pozadí a koncepty, najdete v části hello [přehled](./container-registry-intro.md).

## <a name="prerequisites"></a>Požadavky 

- Kontejner Azure spravované registru - vytvořit spravované kontejneru registru ve vašem předplatném Azure. Například použijte hello portál Azure nebo Azure CLI 2.0 hello. 
- Docker rozhraní příkazového řádku - tooset do místního počítače jako Docker hostitele a přístup hello příkazového řádku Dockeru příkazy, nainstalujte modul Docker. 

## <a name="create-webhook-azure-portal"></a>Vytvoření webhooku portálu Azure

1. Přihlaste se toohello portál Azure a přejděte toohello registru, ve kterém chcete toocreate webhooky. 

2. V okně kontejner hello vyberte kartu "Webhooky" hello. 

3. Vyberte z panelu nástrojů okna webhooku hello "Přidat". 

4. Dokončení hello *vytvoření Webhooku* formulář hello následující informace:

| Hodnota | Popis |
|---|---|
| Name (Název) | Název Hello chcete toogive toohello webhooku. Může obsahovat pouze malá písmena a čísla a 5 – 50 znaků. |
| URI služby | Hello identifikátor URI, kde by měli hello webhooku poslat oznámení POST. |
| Vlastní hlavičky | Hlavičky chcete toopass společně s hello požadavek POST. Musí být v "klíč: hodnota" formátu. |
| Akce aktivace | Akce, které aktivují webhooku hello. Aktuálně webhooky se aktivuje nabízené nebo odstranit image tooan akce. |
| Status | Stav Hello hello webhooku po jejím vytvoření. Ve výchozím nastavení je povolená. |
| Rozsah | Hello rozsahu, na které hello webhooku funguje. Ve výchozím nastavení je hello obor pro všechny události v registru hello. Může se zadat pro úložiště nebo značku ve formátu hello "úložiště: značka". |

Příklad webhooku formuláře:

![ORCHESTRÁTORU DC/OS UŽIVATELSKÉHO ROZHRANÍ](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Vytvoření webhooku rozhraní příkazového řádku Azure

hello toocreate webhooku pomocí rozhraní příkazového řádku Azure, použijte hello [vytvoření webhooku acr az](/cli/azure/acr/webhook#create) příkaz.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Test webhooku

### <a name="azure-portal"></a>portál Azure

Předchozí toousing hello webhooku na kontejneru bitovou kopii nabízené a odstranění akcí, může být testována pomocí hello **Ping** tlačítko. Pokud se použije, odešle hello Ping že toohello žádost o obecný post zadaný koncový bod a protokoly hello odpovědi. To je užitečné tooverify, který hello webhooku byla nastavena správně.

1. Vyberte, chcete tootest webhooku hello. 
2. V horním panelu nástrojů hello vybráním akce "Příkazem ping otestovat" hello. 
3. Zkontrolujte hello žádostí a odpovědí.

### <a name="azure-cli"></a>Azure CLI

tootest webhook ACR s hello rozhraní příkazového řádku Azure, použijte hello [az acr webhooku ping](/cli/azure/acr/webhook#ping) příkaz.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

výsledky hello toosee, použijte hello [az acr webhooku seznam události](/cli/azure/acr/webhook#list-events) příkaz. 

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Odstranit webhook

### <a name="azure-portal"></a>portál Azure

Každý webhooku se dá odstranit výběrem hello webhooku a potom na tlačítko Odstranit hello na hello portálu Azure.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```