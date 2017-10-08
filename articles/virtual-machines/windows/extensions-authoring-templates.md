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
# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Windows
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Z prostředí Azure PowerShell spusťte následující rutiny Azure Powershellu hello:

      Get-AzureVMAvailableExtension


Tato rutina vrací hello název vydavatele, název rozšíření a verzi následujícím způsobem:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Tyto tři vlastnosti mapy příliš "vydavatel", "type" a "typeHandlerVersion" v uvedeném pořadí v hello výše fragment šablony.

> [!NOTE]
> Vždycky je doporučené toouse hello nejnovější rozšíření verze tooget hello nejvíce aktualizované funkce.
> 
> 

## <a name="identifying-hello-schema-for-hello-extension-configuration-parameters"></a>Identifikace hello schéma pro konfigurační parametry rozšíření hello
dalším krokem Hello s vytváření šablonu rozšíření je tooidentify hello formát pro zajištění parametry konfigurace. Každé rozšíření podporuje vlastní sadu parametrů.

toolook v ukázkové konfigurace pro rozšíření Windows, najdete v části [ukázky rozšíření Windows](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Naleznete toohello následující tooget plně úplnou šablonu s rozšíření virtuálního počítače.

[Rozšíření vlastních skriptů na systému Windows virtuálního počítače](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

Po vytvoření šablony hello, můžete nasadit pomocí prostředí Azure PowerShell.

