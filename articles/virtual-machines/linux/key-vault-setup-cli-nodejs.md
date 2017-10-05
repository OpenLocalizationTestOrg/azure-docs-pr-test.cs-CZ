---
title: "Nastavení pro virtuální počítače s Linuxem se 1.0 rozhraní příkazového řádku Azure Key Vault | Microsoft Docs"
description: "Jak nastavit Key Vault pro použití s virtuálním počítačem správce prostředků Azure pomocí Azure CLI 1.0."
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
ms.openlocfilehash: fed612a354d45f34619f2a66bd40d78740c43ac7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-the-azure-cli-10"></a><span data-ttu-id="2b69b-103">Nastavení pro virtuální počítače ve Správci prostředků pomocí Azure CLI 1.0 Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2b69b-103">Set up Key Vault for virtual machines in Azure Resource Manager with the Azure CLI 1.0</span></span>
<span data-ttu-id="2b69b-104">V zásobníku Azure Resource Manager tajných klíčů nebo certifikáty jsou modelovat jako prostředky, které jsou poskytovány zprostředkovatele prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2b69b-104">In the Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by the resource provider of Key Vault.</span></span> <span data-ttu-id="2b69b-105">Další informace o Azure Key Vault najdete v tématu [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="2b69b-105">To learn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="2b69b-106">V pořadí pro Key Vault pro použití s Azure Resource Manager virtuální počítače *EnabledForDeployment* vlastnost v Key Vault musí být nastavena na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="2b69b-106">In order for Key Vault to be used with Azure Resource Manager virtual machines, the *EnabledForDeployment* property on Key Vault must be set to true.</span></span> <span data-ttu-id="2b69b-107">To provedete v různých klientech.</span><span class="sxs-lookup"><span data-stu-id="2b69b-107">You can do this in various clients.</span></span> <span data-ttu-id="2b69b-108">Tento článek ukazuje, jak nastavit Key Vault pro použití s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="2b69b-108">This article shows you how to set up Key Vault for use with Azure Virtual Machines.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="2b69b-109">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="2b69b-109">CLI versions to complete the task</span></span>
<span data-ttu-id="2b69b-110">Můžete dokončit úlohu pomocí jedné z následujících verzí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="2b69b-110">You can complete the task using one of the following CLI versions</span></span>

- <span data-ttu-id="2b69b-111">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="2b69b-111">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="2b69b-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="2b69b-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="use-cli-10-to-set-up-key-vault"></a><span data-ttu-id="2b69b-113">Nastavit Key Vault pomocí rozhraní příkazového řádku 1.0</span><span class="sxs-lookup"><span data-stu-id="2b69b-113">Use CLI 1.0 to set up Key Vault</span></span>
<span data-ttu-id="2b69b-114">Vytvoření trezoru klíčů pomocí rozhraní příkazového řádku (CLI), naleznete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="2b69b-114">To create a key vault by using the command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="2b69b-115">Pro rozhraní příkazového řádku 1.0 budete muset vytvořit trezor klíčů, před přiřazením zásady nasazení.</span><span class="sxs-lookup"><span data-stu-id="2b69b-115">For CLI 1.0, you have to create the key vault before you assign the deployment policy.</span></span> <span data-ttu-id="2b69b-116">Pak můžete přiřadit zásady pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2b69b-116">You can then assign the policy by using the following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a><span data-ttu-id="2b69b-117">Použití šablon nastavit Key Vault</span><span class="sxs-lookup"><span data-stu-id="2b69b-117">Use templates to set up Key Vault</span></span>
<span data-ttu-id="2b69b-118">Pokud použijete šablonu, je nutné nastavit `enabledForDeployment` vlastnost `true` prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2b69b-118">When you use a template, you need to set the `enabledForDeployment` property to `true` for the Key Vault resource.</span></span>

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

<span data-ttu-id="2b69b-119">Jiné možnosti, které můžete konfigurovat při vytvoření trezoru klíčů pomocí šablon najdete v tématu [vytvoření trezoru klíčů](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="2b69b-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
