---
title: "Vytváření šablon s rozšířeními virtuálního počítače s Linuxem | Microsoft Docs"
description: "Další informace o vytváření šablon Azure Resource Manager s rozšířeními pro virtuální počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 322f8f0b-6697-4acb-b5f3-b3f58d28358b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 8b017306474670bf8dde1440128e16ec35146f24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a><span data-ttu-id="c4b58-103">Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c4b58-103">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="c4b58-104">Z příkazového řádku Azure CLI spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c4b58-104">From Azure CLI, run the following commnad:</span></span>

      Azure VM extension list

<span data-ttu-id="c4b58-105">Tento příkaz vrátí název vydavatele, název rozšíření a verze jako následující:</span><span class="sxs-lookup"><span data-stu-id="c4b58-105">This command returns the publisher name, extension name and version as following:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="c4b58-106">Tyto tři vlastnosti mapovat "vydavatel", "typ" a "typeHandlerVersion" v uvedeném pořadí ve výše uvedeném fragmentu šablony.</span><span class="sxs-lookup"><span data-stu-id="c4b58-106">These three properties map to "publisher", "type", and "typeHandlerVersion" respectively in the above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="c4b58-107">Vždy se doporučuje použít nejnovější verzi rozšíření získat nejaktuálnější funkce.</span><span class="sxs-lookup"><span data-stu-id="c4b58-107">It's always recommended to use the latest extension version to get the most updated functionality.</span></span>
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a><span data-ttu-id="c4b58-108">Identifikace schéma pro konfigurační parametry rozšíření</span><span class="sxs-lookup"><span data-stu-id="c4b58-108">Identifying the schema for the extension configuration parameters</span></span>
<span data-ttu-id="c4b58-109">Dalším krokem se vytváření šablonu rozšíření je pro identifikaci formát pro zajištění parametry konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4b58-109">The next step with authoring an extension template is to identify the format for providing configuration parameters.</span></span> <span data-ttu-id="c4b58-110">Každé rozšíření podporuje vlastní sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="c4b58-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="c4b58-111">Chcete-li podíváte na ukázkové konfigurace pro rozšíření Linux, klikněte na tlačítko dokumentaci najdete v tématu [Linux eExtensions ukázky](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c4b58-111">To look at sample configurations for Linux extensions, click the documentation for see [Linux eExtensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="c4b58-112">Naleznete následující příkaz pro získání plně dokončení šablony pomocí rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c4b58-112">Please refer to the following to get a fully complete template with VM Extensions.</span></span>

[<span data-ttu-id="c4b58-113">Rozšíření vlastních skriptů na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c4b58-113">Custom script extension on a Linux VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

<span data-ttu-id="c4b58-114">Po vytvoření šablony, můžete nasadit pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c4b58-114">After authoring the template, you can deploy it using the Azure CLI.</span></span>

