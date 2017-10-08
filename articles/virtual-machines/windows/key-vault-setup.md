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
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="9b7b9-103">Nastavení pro virtuální počítače ve službě Správce prostředků Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="9b7b9-103">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="9b7b9-104">V zásobníku Azure Resource Manager jsou tajné klíče nebo certifikáty modelován jako prostředky, které jsou k dispozici zprostředkovatelem hello prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="9b7b9-104">In Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by hello resource provider of Key Vault.</span></span> <span data-ttu-id="9b7b9-105">toolearn Další informace o Key Vault najdete v části [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="9b7b9-105">toolearn more about Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span>

> [!NOTE]
> 1. <span data-ttu-id="9b7b9-106">Aby mohl používat s virtuálními počítači Azure Resource Manager toobe Key Vault, hello **EnabledForDeployment** musí být nastavena vlastnost v Key Vault tootrue.</span><span class="sxs-lookup"><span data-stu-id="9b7b9-106">In order for Key Vault toobe used with Azure Resource Manager virtual machines, hello **EnabledForDeployment** property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="9b7b9-107">To provedete v různých klientech.</span><span class="sxs-lookup"><span data-stu-id="9b7b9-107">You can do this in various clients.</span></span>
> 2. <span data-ttu-id="9b7b9-108">Hello Key Vault potřeb, které toobe vytvořené v hello stejném předplatném a umístění jako hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9b7b9-108">hello Key Vault needs toobe created in hello same subscription and location as hello Virtual Machine.</span></span>
>
>

## <a name="use-powershell-tooset-up-key-vault"></a><span data-ttu-id="9b7b9-109">Použití prostředí PowerShell tooset až Key Vault</span><span class="sxs-lookup"><span data-stu-id="9b7b9-109">Use PowerShell tooset up Key Vault</span></span>
<span data-ttu-id="9b7b9-110">toocreate trezoru klíčů pomocí prostředí PowerShell, najdete v části [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span><span class="sxs-lookup"><span data-stu-id="9b7b9-110">toocreate a key vault by using PowerShell, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span></span>

<span data-ttu-id="9b7b9-111">Pro nové trezorů klíčů můžete použít tuto rutinu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9b7b9-111">For new key vaults, you can use this PowerShell cmdlet:</span></span>

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

<span data-ttu-id="9b7b9-112">Pro existující trezorů klíčů můžete použít tuto rutinu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9b7b9-112">For existing key vaults, you can use this PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-tooset-up-key-vault"></a><span data-ttu-id="9b7b9-113">Nám rozhraní příkazového řádku tooset až Key Vault</span><span class="sxs-lookup"><span data-stu-id="9b7b9-113">Us CLI tooset up Key Vault</span></span>
<span data-ttu-id="9b7b9-114">toocreate trezoru klíčů pomocí hello rozhraní příkazového řádku (CLI), najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="9b7b9-114">toocreate a key vault by using hello command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="9b7b9-115">Rozhraní příkazového řádku je nutné trezoru klíčů hello toocreate před přiřazením hello nasazení zásad.</span><span class="sxs-lookup"><span data-stu-id="9b7b9-115">For CLI, you have toocreate hello key vault before you assign hello deployment policy.</span></span> <span data-ttu-id="9b7b9-116">Můžete to provést pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9b7b9-116">You can do this by using hello following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="9b7b9-117">Použití šablony tooset až Key Vault</span><span class="sxs-lookup"><span data-stu-id="9b7b9-117">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="9b7b9-118">Když použijete šablonu, budete potřebovat tooset hello `enabledForDeployment` vlastnost příliš`true` pro hello prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="9b7b9-118">While you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource.</span></span>

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

<span data-ttu-id="9b7b9-119">Jiné možnosti, které můžete konfigurovat při vytvoření trezoru klíčů pomocí šablon najdete v tématu [vytvoření trezoru klíčů](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="9b7b9-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
