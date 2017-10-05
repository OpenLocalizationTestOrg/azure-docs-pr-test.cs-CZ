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
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Z příkazového řádku Azure CLI spusťte následující příkaz:

      Azure VM extension list

Tento příkaz vrátí název vydavatele, název rozšíření a verze jako následující:

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

Chcete-li podíváte na ukázkové konfigurace pro rozšíření Linux, klikněte na tlačítko dokumentaci najdete v tématu [Linux eExtensions ukázky](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Naleznete následující příkaz pro získání plně dokončení šablony pomocí rozšíření virtuálního počítače.

[Rozšíření vlastních skriptů na virtuální počítač s Linuxem](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Po vytvoření šablony, můžete nasadit pomocí rozhraní příkazového řádku Azure.

