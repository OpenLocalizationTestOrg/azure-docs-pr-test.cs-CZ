---
title: "aaaAuthoring šablony s rozšířeními virtuálního počítače s Windows | Microsoft Docs"
description: "Další informace o vytváření šablon Azure Resource Manager s rozšířeními pro virtuální počítače Windows"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 418dd1f7-ded8-45ab-9a5a-a59d245e2555
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: c5156cb3859f7ff86bebda942150d268e57d6486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a><span data-ttu-id="0c61e-103">Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="0c61e-103">Authoring Azure Resource Manager templates with Windows VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="0c61e-104">Z prostředí Azure PowerShell spusťte následující rutiny Azure Powershellu hello:</span><span class="sxs-lookup"><span data-stu-id="0c61e-104">From Azure PowerShell, run hello following Azure PowerShell cmdlet:</span></span>

      Get-AzureVMAvailableExtension


<span data-ttu-id="0c61e-105">Tato rutina vrací hello název vydavatele, název rozšíření a verzi následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0c61e-105">This cmdlet returns hello publisher name, extension name, and version as follows:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="0c61e-106">Tyto tři vlastnosti mapy příliš "vydavatel", "type" a "typeHandlerVersion" v uvedeném pořadí v hello výše fragment šablony.</span><span class="sxs-lookup"><span data-stu-id="0c61e-106">These three properties map too"publisher", "type", and "typeHandlerVersion" respectively in hello above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="0c61e-107">Vždycky je doporučené toouse hello nejnovější rozšíření verze tooget hello nejvíce aktualizované funkce.</span><span class="sxs-lookup"><span data-stu-id="0c61e-107">It's always recommended toouse hello latest extension version tooget hello most updated functionality.</span></span>
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a><span data-ttu-id="0c61e-108">Identifikace hello schéma pro konfigurační parametry rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="0c61e-108">Identifying hello schema for hello extension configuration parameters</span></span>
<span data-ttu-id="0c61e-109">dalším krokem Hello s vytváření šablonu rozšíření je tooidentify hello formát pro zajištění parametry konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0c61e-109">hello next step with authoring an extension template is tooidentify hello format for providing configuration parameters.</span></span> <span data-ttu-id="0c61e-110">Každé rozšíření podporuje vlastní sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="0c61e-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="0c61e-111">toolook v ukázkové konfigurace pro rozšíření Windows, najdete v části [ukázky rozšíření Windows](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0c61e-111">toolook at sample configurations for Windows extensions, see [Windows extensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="0c61e-112">Naleznete toohello následující tooget plně úplnou šablonu s rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0c61e-112">Please refer toohello following tooget a fully complete template with VM extensions.</span></span>

[<span data-ttu-id="0c61e-113">Rozšíření vlastních skriptů na systému Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0c61e-113">Custom Script Extension on a Windows VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

<span data-ttu-id="0c61e-114">Po vytvoření šablony hello, můžete nasadit pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0c61e-114">After authoring hello template, you can deploy it using Azure PowerShell.</span></span>

