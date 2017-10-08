---
title: "aaaSet si klíč trezoru pro virtuální počítače Windows ve službě Správce prostředků Azure | Microsoft Docs"
description: "Jak tooset až Key Vault pro použití s se virtuální počítač Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: 53bbe90708202ecfdcf754822d04bf2469631f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Nastavení pro virtuální počítače ve službě Správce prostředků Azure Key Vault

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

V zásobníku Azure Resource Manager jsou tajné klíče nebo certifikáty modelován jako prostředky, které jsou k dispozici zprostředkovatelem hello prostředku Key Vault. toolearn Další informace o Key Vault najdete v části [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. Aby mohl používat s virtuálními počítači Azure Resource Manager toobe Key Vault, hello **EnabledForDeployment** musí být nastavena vlastnost v Key Vault tootrue. To provedete v různých klientech.
> 2. Hello Key Vault potřeb, které toobe vytvořené v hello stejném předplatném a umístění jako hello virtuálního počítače.
>
>

## <a name="use-powershell-tooset-up-key-vault"></a>Použití prostředí PowerShell tooset až Key Vault
toocreate trezoru klíčů pomocí prostředí PowerShell, najdete v části [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).

Pro nové trezorů klíčů můžete použít tuto rutinu prostředí PowerShell:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Pro existující trezorů klíčů můžete použít tuto rutinu prostředí PowerShell:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-tooset-up-key-vault"></a>Nám rozhraní příkazového řádku tooset až Key Vault
toocreate trezoru klíčů pomocí hello rozhraní příkazového řádku (CLI), najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Rozhraní příkazového řádku je nutné trezoru klíčů hello toocreate před přiřazením hello nasazení zásad. Můžete to provést pomocí hello následující příkaz:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a>Použití šablony tooset až Key Vault
Když použijete šablonu, budete potřebovat tooset hello `enabledForDeployment` vlastnost příliš`true` pro hello prostředku Key Vault.

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
