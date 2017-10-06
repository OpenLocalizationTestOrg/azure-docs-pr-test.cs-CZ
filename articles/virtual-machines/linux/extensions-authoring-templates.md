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
# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem
[!INCLUDE [virtual-machines-common-extensions-authoring-templates](../../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Z příkazového řádku Azure CLI spusťte následující příkaz hello:

      Azure VM extension list

Tento příkaz vrátí hello vydavatele název, název rozšíření a verze jako následující:

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

toolook v ukázkové konfigurace pro rozšíření Linux, klikněte na tlačítko hello dokumentaci najdete v tématu [Linux eExtensions ukázky](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Naleznete toohello následující tooget plně úplnou šablonu s rozšíření virtuálního počítače.

[Rozšíření vlastních skriptů na virtuální počítač s Linuxem](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Po vytvoření šablony hello, můžete nasadit pomocí hello rozhraní příkazového řádku Azure.

