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
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Windows
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Z prostředí Azure PowerShell spusťte následující rutiny Azure Powershellu:

      Get-AzureVMAvailableExtension


Tato rutina vrací název vydavatele, název rozšíření a verzi následujícím způsobem:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Tyto tři vlastnosti mapovat "vydavatel", "typ" a "typeHandlerVersion" v uvedeném pořadí ve výše uvedeném fragmentu šablony.

> [!NOTE]
> Vždy se doporučuje použít nejnovější verzi rozšíření získat nejaktuálnější funkce.
> 
> 

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Identifikace schéma pro konfigurační parametry rozšíření
Dalším krokem se vytváření šablonu rozšíření je pro identifikaci formát pro zajištění parametry konfigurace. Každé rozšíření podporuje vlastní sadu parametrů.

Podívejte se na ukázkové konfigurace pro rozšíření Windows, najdete v tématu [ukázky rozšíření Windows](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Naleznete následující příkaz pro získání plně dokončení šablony pomocí rozšíření virtuálního počítače.

[Rozšíření vlastních skriptů na systému Windows virtuálního počítače](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

Po vytvoření šablony, můžete nasadit pomocí prostředí Azure PowerShell.

