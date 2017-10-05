---
title: "Nastavit klíč trezoru pro virtuální počítače Windows ve službě Správce prostředků Azure | Microsoft Docs"
description: "Jak nastavit Key Vault pro použití s se virtuální počítač Azure Resource Manager."
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
ms.openlocfilehash: a5083a5216efbfd76fd912ec48c2f0ec3b30c4a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="db06a-103">Nastavení pro virtuální počítače ve službě Správce prostředků Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="db06a-103">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="db06a-104">V zásobníku Azure Resource Manager tajných klíčů nebo certifikáty jsou modelovat jako prostředky, které jsou poskytovány zprostředkovatele prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="db06a-104">In Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by the resource provider of Key Vault.</span></span> <span data-ttu-id="db06a-105">Další informace o Key Vault najdete v tématu [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="db06a-105">To learn more about Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span>

> [!NOTE]
> 1. <span data-ttu-id="db06a-106">V pořadí pro Key Vault pro použití s Azure Resource Manager virtuální počítače **EnabledForDeployment** vlastnost v Key Vault musí být nastavena na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="db06a-106">In order for Key Vault to be used with Azure Resource Manager virtual machines, the **EnabledForDeployment** property on Key Vault must be set to true.</span></span> <span data-ttu-id="db06a-107">To provedete v různých klientech.</span><span class="sxs-lookup"><span data-stu-id="db06a-107">You can do this in various clients.</span></span>
> 2. <span data-ttu-id="db06a-108">Key Vault je potřeba vytvořit ve stejném předplatném a umístění jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="db06a-108">The Key Vault needs to be created in the same subscription and location as the Virtual Machine.</span></span>
>
>

## <a name="use-powershell-to-set-up-key-vault"></a><span data-ttu-id="db06a-109">Nastavit Key Vault pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="db06a-109">Use PowerShell to set up Key Vault</span></span>
<span data-ttu-id="db06a-110">Vytvoření trezoru klíčů pomocí prostředí PowerShell naleznete v tématu [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span><span class="sxs-lookup"><span data-stu-id="db06a-110">To create a key vault by using PowerShell, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span></span>

<span data-ttu-id="db06a-111">Pro nové trezorů klíčů můžete použít tuto rutinu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="db06a-111">For new key vaults, you can use this PowerShell cmdlet:</span></span>

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

<span data-ttu-id="db06a-112">Pro existující trezorů klíčů můžete použít tuto rutinu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="db06a-112">For existing key vaults, you can use this PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a><span data-ttu-id="db06a-113">Nám rozhraní příkazového řádku pro nastavení Key Vault</span><span class="sxs-lookup"><span data-stu-id="db06a-113">Us CLI to set up Key Vault</span></span>
<span data-ttu-id="db06a-114">Vytvoření trezoru klíčů pomocí rozhraní příkazového řádku (CLI), naleznete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="db06a-114">To create a key vault by using the command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="db06a-115">Pro rozhraní příkazového řádku musíte vytvořit trezor klíčů, před přiřazením zásady nasazení.</span><span class="sxs-lookup"><span data-stu-id="db06a-115">For CLI, you have to create the key vault before you assign the deployment policy.</span></span> <span data-ttu-id="db06a-116">Můžete to provést pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="db06a-116">You can do this by using the following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a><span data-ttu-id="db06a-117">Použití šablon nastavit Key Vault</span><span class="sxs-lookup"><span data-stu-id="db06a-117">Use templates to set up Key Vault</span></span>
<span data-ttu-id="db06a-118">Když použijete šablonu, je nutné nastavit `enabledForDeployment` vlastnost `true` prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="db06a-118">While you use a template, you need to set the `enabledForDeployment` property to `true` for the Key Vault resource.</span></span>

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

<span data-ttu-id="db06a-119">Jiné možnosti, které můžete konfigurovat při vytvoření trezoru klíčů pomocí šablon najdete v tématu [vytvoření trezoru klíčů](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="db06a-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
