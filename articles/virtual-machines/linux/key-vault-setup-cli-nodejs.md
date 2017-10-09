---
title: "aaaSet až Key Vault pro virtuální počítače Linux s hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak hello tooset až Key Vault pro použití s se virtuální počítač Azure Resource Manager pomocí Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 275022e4e7e26d7363784c289dd7512047c07bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-hello-azure-cli-10"></a>Nastavení pro virtuální počítače ve službě Správce prostředků Azure s hello 1.0 rozhraní příkazového řádku Azure Key Vault
V zásobníku hello Azure Resource Manager jsou tajné klíče nebo certifikáty modelován jako prostředky, které jsou k dispozici zprostředkovatelem hello prostředku Key Vault. toolearn Další informace o Azure Key Vault najdete v části [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md) Aby mohl používat s virtuálními počítači Azure Resource Manager toobe Key Vault, hello *EnabledForDeployment* musí být nastavena vlastnost v Key Vault tootrue. To provedete v různých klientech. Tento článek ukazuje, jak tooset až Key Vault pro použití s virtuálními počítači Azure.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Dokončení úkolů hello pomocí jedné z hello následující verze rozhraní příkazového řádku

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="use-cli-10-tooset-up-key-vault"></a>Pomocí rozhraní příkazového řádku 1.0 tooset až Key Vault
toocreate trezoru klíčů pomocí hello rozhraní příkazového řádku (CLI), najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Pro rozhraní příkazového řádku 1.0 máte trezoru klíčů hello toocreate před přiřazením hello nasazení zásad. Pak můžete přiřadit zásady hello pomocí hello následující příkaz:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a>Použití šablony tooset až Key Vault
Pokud použijete šablonu, budete potřebovat tooset hello `enabledForDeployment` vlastnost příliš`true` pro hello prostředku Key Vault.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Jiné možnosti, které můžete konfigurovat při vytvoření trezoru klíčů pomocí šablon najdete v tématu [vytvoření trezoru klíčů](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
