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
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-hello-azure-cli-10"></a><span data-ttu-id="5ae11-103">Nastavení pro virtuální počítače ve službě Správce prostředků Azure s hello 1.0 rozhraní příkazového řádku Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5ae11-103">Set up Key Vault for virtual machines in Azure Resource Manager with hello Azure CLI 1.0</span></span>
<span data-ttu-id="5ae11-104">V zásobníku hello Azure Resource Manager jsou tajné klíče nebo certifikáty modelován jako prostředky, které jsou k dispozici zprostředkovatelem hello prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5ae11-104">In hello Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by hello resource provider of Key Vault.</span></span> <span data-ttu-id="5ae11-105">toolearn Další informace o Azure Key Vault najdete v části [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="5ae11-105">toolearn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="5ae11-106">Aby mohl používat s virtuálními počítači Azure Resource Manager toobe Key Vault, hello *EnabledForDeployment* musí být nastavena vlastnost v Key Vault tootrue.</span><span class="sxs-lookup"><span data-stu-id="5ae11-106">In order for Key Vault toobe used with Azure Resource Manager virtual machines, hello *EnabledForDeployment* property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="5ae11-107">To provedete v různých klientech.</span><span class="sxs-lookup"><span data-stu-id="5ae11-107">You can do this in various clients.</span></span> <span data-ttu-id="5ae11-108">Tento článek ukazuje, jak tooset až Key Vault pro použití s virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="5ae11-108">This article shows you how tooset up Key Vault for use with Azure Virtual Machines.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="5ae11-109">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5ae11-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="5ae11-110">Dokončení úkolů hello pomocí jedné z hello následující verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="5ae11-110">You can complete hello task using one of hello following CLI versions</span></span>

- <span data-ttu-id="5ae11-111">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="5ae11-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="5ae11-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="5ae11-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="use-cli-10-tooset-up-key-vault"></a><span data-ttu-id="5ae11-113">Pomocí rozhraní příkazového řádku 1.0 tooset až Key Vault</span><span class="sxs-lookup"><span data-stu-id="5ae11-113">Use CLI 1.0 tooset up Key Vault</span></span>
<span data-ttu-id="5ae11-114">toocreate trezoru klíčů pomocí hello rozhraní příkazového řádku (CLI), najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="5ae11-114">toocreate a key vault by using hello command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="5ae11-115">Pro rozhraní příkazového řádku 1.0 máte trezoru klíčů hello toocreate před přiřazením hello nasazení zásad.</span><span class="sxs-lookup"><span data-stu-id="5ae11-115">For CLI 1.0, you have toocreate hello key vault before you assign hello deployment policy.</span></span> <span data-ttu-id="5ae11-116">Pak můžete přiřadit zásady hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5ae11-116">You can then assign hello policy by using hello following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="5ae11-117">Použití šablony tooset až Key Vault</span><span class="sxs-lookup"><span data-stu-id="5ae11-117">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="5ae11-118">Pokud použijete šablonu, budete potřebovat tooset hello `enabledForDeployment` vlastnost příliš`true` pro hello prostředku Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5ae11-118">When you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource.</span></span>

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

<span data-ttu-id="5ae11-119">Jiné možnosti, které můžete konfigurovat při vytvoření trezoru klíčů pomocí šablon najdete v tématu [vytvoření trezoru klíčů](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="5ae11-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
