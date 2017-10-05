---
title: "Vytváření šablon s rozšířeními virtuálního počítače s Windows | Microsoft Docs"
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
ms.openlocfilehash: 338668df33783d8ef62c6710f4d96ce805296fe5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a><span data-ttu-id="f5647-103">Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="f5647-103">Authoring Azure Resource Manager templates with Windows VM extensions</span></span>
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

<span data-ttu-id="f5647-104">Z prostředí Azure PowerShell spusťte následující rutiny Azure Powershellu:</span><span class="sxs-lookup"><span data-stu-id="f5647-104">From Azure PowerShell, run the following Azure PowerShell cmdlet:</span></span>

      Get-AzureVMAvailableExtension


<span data-ttu-id="f5647-105">Tato rutina vrací název vydavatele, název rozšíření a verzi následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f5647-105">This cmdlet returns the publisher name, extension name, and version as follows:</span></span>

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

<span data-ttu-id="f5647-106">Tyto tři vlastnosti mapovat "vydavatel", "typ" a "typeHandlerVersion" v uvedeném pořadí ve výše uvedeném fragmentu šablony.</span><span class="sxs-lookup"><span data-stu-id="f5647-106">These three properties map to "publisher", "type", and "typeHandlerVersion" respectively in the above template snippet.</span></span>

> [!NOTE]
> <span data-ttu-id="f5647-107">Vždy se doporučuje použít nejnovější verzi rozšíření získat nejaktuálnější funkce.</span><span class="sxs-lookup"><span data-stu-id="f5647-107">It's always recommended to use the latest extension version to get the most updated functionality.</span></span>
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a><span data-ttu-id="f5647-108">Identifikace schéma pro konfigurační parametry rozšíření</span><span class="sxs-lookup"><span data-stu-id="f5647-108">Identifying the schema for the extension configuration parameters</span></span>
<span data-ttu-id="f5647-109">Dalším krokem se vytváření šablonu rozšíření je pro identifikaci formát pro zajištění parametry konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f5647-109">The next step with authoring an extension template is to identify the format for providing configuration parameters.</span></span> <span data-ttu-id="f5647-110">Každé rozšíření podporuje vlastní sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="f5647-110">Each extension supports its own set of parameters.</span></span>

<span data-ttu-id="f5647-111">Podívejte se na ukázkové konfigurace pro rozšíření Windows, najdete v tématu [ukázky rozšíření Windows](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f5647-111">To look at sample configurations for Windows extensions, see [Windows extensions samples](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="f5647-112">Naleznete následující příkaz pro získání plně dokončení šablony pomocí rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f5647-112">Please refer to the following to get a fully complete template with VM extensions.</span></span>

[<span data-ttu-id="f5647-113">Rozšíření vlastních skriptů na systému Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f5647-113">Custom Script Extension on a Windows VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

<span data-ttu-id="f5647-114">Po vytvoření šablony, můžete nasadit pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f5647-114">After authoring the template, you can deploy it using Azure PowerShell.</span></span>

