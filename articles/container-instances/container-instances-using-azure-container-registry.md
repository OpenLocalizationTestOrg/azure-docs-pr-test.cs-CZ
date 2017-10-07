---
title: "aaaDeploy tooAzure kontejner instancí z hello registru kontejner Azure | Dokumentace Azure"
description: "Nasazování tooAzure kontejner instancí z hello registru kontejner Azure"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2667f91db8ed92a9ccc9ba722a2b1f5c5ea93886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-container-instances-from-hello-azure-container-registry"></a>Nasazování tooAzure kontejner instancí z hello registru kontejner Azure

Hello registru kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie. Tento článek popisuje, jak Image kontejneru toodeploy uložené v registru kontejner Azure hello tooAzure kontejner instancí.

## <a name="using-hello-azure-cli"></a>Pomocí hello rozhraní příkazového řádku Azure

Hello rozhraní příkazového řádku Azure obsahuje příkazy pro vytváření a Správa kontejnerů v Azure kontejner instancí. Pokud zadáte privátní bitové kopie v hello `create` příkazů, můžete také určit hello image registru požadováno heslo tooauthenticate s hello kontejneru registru.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

Hello `create` příkaz také podporuje určení hello `registry-login-server` a `registry-username`. Ale hello přihlášení na server pro hello registru kontejner Azure je vždy *registryname*. azurecr.io a hello výchozí uživatelské jméno *registryname*, takže tyto hodnoty jsou odvodit z hello název bitové kopie, pokud k dispozici není explicitně.

## <a name="using-an-azure-resource-manager-template"></a>Pomocí šablony Azure Resource Manager

Můžete zadat vlastnosti hello registru kontejneru systému Azure v šablonu Azure Resource Manager včetně hello `imageRegistryCredentials` vlastnost v definici skupiny hello kontejneru:

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

tooavoid ukládání heslo registru kontejneru přímo v šabloně hello, doporučujeme uložit jako tajný klíč v [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) a odkazovat v šabloně hello pomocí hello [nativní integrace mezi službou Hello Azure Resource Manageru a Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="using-hello-azure-portal"></a>Pomocí hello portálu Azure

Pokud chcete zachovat kontejneru obrázků v hello registru kontejner Azure, můžete snadno vytvořit kontejner v Azure kontejner instancí pomocí hello portálu Azure.

1. V hello portálu Azure přejděte tooyour kontejneru registru.

2. Vyberte úložiště.

    ![nabídky Azure kontejneru registru Hello v hello portálu Azure][acr-menu]

3. Zvolte hello úložiště, který chcete toodeploy z.

4. Klikněte pravým tlačítkem na hello značky pro bitovou kopii kontejneru hello chcete toodeploy.

    ![Kontextové nabídky pro spuštění kontejner s instancemi Azure kontejneru][acr-runinstance-contextmenu]

5. Zadejte název pro kontejner hello a název skupiny prostředků hello. Pokud chcete můžete také změnit hello výchozí hodnoty.

    ![Vytvořit nabídku pro instance kontejner Azure][acr-create-deeplink]

6. Po dokončení nasazení hello, můžete přejít skupina kontejneru toohello z hello oznámení podokně toofind jeho IP adresu a další vlastnosti.

    ![Zobrazení podrobností pro skupinu kontejner instancí kontejnerů Azure][aci-detailsview]

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toobuild kontejnery, vložit je tooa privátní kontejneru registru a jejich nasazení instance kontejneru tooAzure podle [dokončení kurzu hello](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
