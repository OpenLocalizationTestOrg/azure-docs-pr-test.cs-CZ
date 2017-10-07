---
title: "aaaConsume Azure spravované aplikace | Microsoft Docs"
description: "Popisuje, jak zákazník vytvoří Azure spravované aplikace z publikovaných souborů."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b8510086eb05304c0e351a391b7e0cf34a467568
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-internal-managed-application"></a>Využívat interní spravované aplikace

Můžete využívat Azure [spravované aplikace](managed-application-overview.md) , jsou určené pro členy vaší organizace. Spravované aplikace můžete například vybrat od vašeho IT oddělení, které zajišťují dodržování standardů organizace. Tyto spravované aplikace jsou k dispozici prostřednictvím hello katalogu služeb, hello Azure Marketplace.

Než budete pokračovat v tomto článku, musíte mít k dispozici v katalogu služeb hello spravované aplikace pro vaše předplatné. Pokud někdo ve vaší organizaci dosud vytvořena spravované aplikace, najdete v části [publikování spravované aplikace pro interní používání](managed-application-publishing.md).

V současné době můžete použít rozhraní příkazového řádku Azure nebo hello Azure portálu tooconsume spravované aplikace.

## <a name="create-hello-managed-application-by-using-hello-portal"></a>Vytvoření aplikace hello spravované pomocí portálu hello

toodeploy spravované aplikace prostřednictvím portálu hello, postupujte takto:

1. Přejděte toohello portálu Azure. Vyhledejte **katalogu služeb spravované aplikace**.

   ![Katalog služeb spravované aplikace](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. Vyberte hello spravované aplikace, které chcete toocreate z hello seznam dostupných řešení. Vyberte **Vytvořit**.

   ![Výběr spravované aplikace](./media/managed-application-consumption/select-offer.png)

1. Zadejte hodnoty pro parametry hello, které jsou prostředky požadované tooprovision hello. Vyberte **– Západ střední USA** pro umístění. Vyberte **OK**.

   ![Parametry spravované aplikace](./media/managed-application-consumption/input-parameters.png)

1. Šablona Hello ověří hello hodnoty, které jste zadali. V případě úspěšného ověření vyberte **OK** toostart hello nasazení.

   ![Ověřování spravovaných aplikací](./media/managed-application-consumption/validation.png)

Po dokončení nasazení hello hello definovaná v šabloně hello odpovídající prostředky jsou zřízené v hello spravovaných prostředků skupinu, kterou jste zadali.

## <a name="create-hello-managed-application-by-using-azure-cli"></a>Vytvoření aplikace hello spravované pomocí rozhraní příkazového řádku Azure

Existují dva způsoby toocreate spravované aplikace pomocí rozhraní příkazového řádku Azure:

* Pro vytvoření spravované aplikace můžete použijte příkaz hello.
* Příkaz hello regulární šablonu nasazení.

### <a name="use-hello-template-deployment-command"></a>Použijte příkaz nasazení šablony hello

Nasaďte soubor applianceMainTemplate.json hello hello dodavatele vytvořen.

Pak vytvořte dvě skupiny prostředků. Hello první skupina prostředků je kde hello spravované aplikace je vytvořen prostředek: Microsoft.Solutions/appliances. Skupina prostředků druhý Hello obsahuje všechny prostředky hello definované v mainTemplate.json. Tato skupina prostředků je spravovaný hello ISV.

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Použití `westcentralus` jako hello umístění skupiny prostředků hello.
>

toodeploy applianceMainTemplate.json v mainResourceGroup hello použijte následující příkaz:

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

Po hello předcházející spustí šablony budete vyzváni k hello hodnoty hello parametrů, které jsou definované v šabloně hello. Kromě toohello parametry, které jsou potřeba tooprovision prostředky v šabloně, budete potřebovat dvě hodnoty klíčů parametr:

- **managedResourceGroupId**: hello ID prostředků hello prostředků skupiny obsahující hello definované v applianceMainTemplate.json. Hello ID má podobu hello `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. V předchozím příkladu hello, to je hello ID `managedResourceGroup`.
- **applianceDefinitionId**: hello ID hello spravované aplikace definice prostředků. Tato hodnota je poskytovaný hello ISV.

> [!NOTE]
> Vydavatel Hello musí udělit přístup toohello prostředků skupiny, která obsahuje definice hello spravované aplikace. Hello definice prostředků se vytvoří v předplatném hello vydavatele. Proto uživatele, skupiny uživatelů nebo aplikací v klientovi zákazníka hello musí prostředků toothis přístup pro čtení.

Po nasazení hello skončí úspěšně, zobrazí hello spravované aplikace je vytvořena v mainResourceGroup. Hello storageAccount prostředek vytvoří v managedResourceGroup.

### <a name="use-hello-create-command"></a>Použití hello vytvoření příkazu

Můžete použít hello `az managedapp create` příkaz toocreate spravované aplikace z hello spravované definice aplikace.

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* **Id definice zařízení**: ID prostředku hello hello spravované definice aplikace vytvořené v předchozím kroku hello. tooobtain toto ID, spusťte následující příkaz hello:

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  Tento příkaz vrátí definice hello spravované aplikace. Je nutné hello hodnotu vlastnosti ID hello.

* **spravované id rg**: název hello hello prostředků obsahující hello prostředků skupiny definované v applianceMainTemplate.json. Tato skupina prostředků je skupina hello spravovaných prostředků. Je spravovaný hello vydavatele. Pokud neexistuje, vytvoří se pro vás.
* **Skupina prostředků**: Po vytvoření skupiny prostředků hello kde hello spravovaných prostředků aplikace. Hello Microsoft.Solutions/appliance prostředků je umístěn v této skupině prostředků.
* **Parametry**: hello parametry, které jsou potřebné pro prostředky hello definované v applianceMainTemplate.json.

## <a name="known-issues"></a>Známé problémy

Tato verze preview zahrnuje hello následující problémy:

* Během vytváření hello hello spravované aplikace se zobrazí 500 vnitřní chybu serveru. Pokud narazíte na potíže, je pravděpodobně toobe přerušované. Opakujte operaci hello.
* Novou skupinu prostředků je potřeba pro skupinu hello spravovaných prostředků. Pokud použijete existující skupinu prostředků, hello nasazení se nezdaří.
* Hello skupinu prostředků, která obsahuje hello Microsoft.Solutions/appliances prostředek musí být vytvořen v hello **westcentralus** umístění.

## <a name="next-steps"></a>Další kroky

* Úvod toomanaged aplikace naleznete v [spravované aplikace přehled](managed-application-overview.md).
* Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).
* Informace o publikování toohello spravovaných aplikací Azure Marketplace najdete v tématu [Azure spravované aplikace v hello Marketplace](managed-application-author-marketplace.md).
* Informace o použití spravovaných aplikací z hello Marketplace najdete v tématu [využívat Azure spravované aplikace v hello Marketplace](managed-application-consume-marketplace.md).
