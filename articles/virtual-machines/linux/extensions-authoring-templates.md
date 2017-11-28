---
title: "aaaAuthoring šablon s rozšíření virtuálního počítače s Linuxem | Microsoft Docs"
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
ms.openlocfilehash: b797e442d8706956bbc06c5be611a2b0119055d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a><span data-ttu-id="63814-103">Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="63814-103">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="63814-104">Z příkazového řádku Azure CLI spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="63814-104">From Azure CLI, run hello following commnad:</span></span>

      Azure VM extension list

<span data-ttu-id="63814-105">Tento příkaz vrátí hello vydavatele název, název rozšíření a verze jako následující:</span><span class="sxs-lookup"><span data-stu-id="63814-105">This command returns hello publisher name, extension name and version as following:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="63814-106">Tyto tři vlastnosti mapy příliš "vydavatel", "type" a "typeHandlerVersion" v uvedeném pořadí v hello výše fragment šablony.</span><span class="sxs-lookup"><span data-stu-id="63814-106">These three properties map too"publisher", "type", and "typeHandlerVersion" respectively in hello above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="63814-107">Vždycky je doporučené toouse hello nejnovější rozšíření verze tooget hello nejvíce aktualizované funkce.</span><span class="sxs-lookup"><span data-stu-id="63814-107">It's always recommended toouse hello latest extension version tooget hello most updated functionality.</span></span>
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a><span data-ttu-id="63814-108">Identifikace hello schéma pro konfigurační parametry rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="63814-108">Identifying hello schema for hello extension configuration parameters</span></span>
<span data-ttu-id="63814-109">dalším krokem Hello s vytváření šablonu rozšíření je tooidentify hello formát pro zajištění parametry konfigurace.</span><span class="sxs-lookup"><span data-stu-id="63814-109">hello next step with authoring an extension template is tooidentify hello format for providing configuration parameters.</span></span> <span data-ttu-id="63814-110">Každé rozšíření podporuje vlastní sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="63814-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="63814-111">toolook v ukázkové konfigurace pro rozšíření Linux, klikněte na tlačítko hello dokumentaci najdete v tématu [Linux eExtensions ukázky](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="63814-111">toolook at sample configurations for Linux extensions, click hello documentation for see [Linux eExtensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="63814-112">Naleznete toohello následující tooget plně úplnou šablonu s rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="63814-112">Please refer toohello following tooget a fully complete template with VM Extensions.</span></span>

[<span data-ttu-id="63814-113">Rozšíření vlastních skriptů na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="63814-113">Custom script extension on a Linux VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

<span data-ttu-id="63814-114">Po vytvoření šablony hello, můžete nasadit pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="63814-114">After authoring hello template, you can deploy it using hello Azure CLI.</span></span>

