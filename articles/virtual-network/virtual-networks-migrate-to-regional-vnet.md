---
title: "aaaMigrate virtuální sítě Azure (klasické) z oblast tooa skupiny vztahů | Microsoft Docs"
description: "Zjistěte, jak toomigrate virtuální sítě (klasické) z spřažení Skupina oblast tooa."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: e3a5c0f21e748912e29e2e8d03f4295720e63637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-tooa-region"></a>Migrovat virtuální sítě (klasické) z oblast tooa skupiny vztahů

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model nasazení Resource Manager hello.

Skupiny vztahů zajistěte, aby prostředky vytvořené v rámci hello stejnou skupinu vztahů, které jsou fyzicky hostované servery, které jsou blízko sebe, povolení tyto prostředky toocommunicate rychlejší. V posledních hello skupiny vztahů byly požadavek pro vytvoření virtuální sítě (klasické). V té době hello síťové Správce služby, který spravovat virtuální sítě (klasické) může fungovat pouze v rámci sady fyzických serverech nebo jednotky škálování. Architektury vylepšení mají vyšší hello oboru oblasti tooa správy sítě.

V důsledku těchto vylepšení architektury jsou skupiny vztahů už doporučené, nebo požadované pro virtuální sítě (klasické). použití Hello skupin vztahů pro virtuální sítě (klasické) je nahrazena oblasti. Virtuální sítě (klasické), které jsou spojeny s oblastí se nazývají regionálních virtuálních sítí.

Doporučujeme, že skupiny vztahů nepoužíváte obecně. Kromě zajištění dostatečného hello požadavek virtuální sítě skupiny vztahů byly také důležité toouse tooensure prostředky, například výpočetní (klasické) a úložiště (klasické), byly umístěny vedle sebe. Však s hello aktuální Azure síťovou architekturu, tyto požadavky na umístění již nejsou potřebné.

> [!IMPORTANT]
> Přestože je technicky možné toocreate virtuální sítě, který je přidružený skupině vztahů, neexistuje žádný poutavé toodo důvod tak. Mnoho funkcí virtuální sítě, jako jsou skupiny zabezpečení sítě, jsou k dispozici pouze při použití regionální virtuální síť a nejsou k dispozici pro virtuální sítě, které jsou přidružené skupiny vztahů.
> 
> 

## <a name="edit-hello-network-configuration-file"></a>Upravovat soubor konfigurace sítě hello

1. Exportujte hello sítě konfigurační soubor. najdete v části toolearn jak tooexport konfiguraci sítě souboru pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure (CLI) 1.0, hello [konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md#export).
2. Upravte soubor konfigurace sítě hello nahrazení **AffinityGroup** s **umístění**. Zadejte Azure [oblast](https://azure.microsoft.com/regions) pro **umístění**.
   
   > [!NOTE]
   > Hello **umístění** hello oblast, která jste zadali pro skupiny vztahů hello, který je přidružený virtuální sítě (klasické). Pokud virtuální sítě (klasické) souvisí s skupinu vztahů, který je umístěný v západní USA, když provádíte migraci, například vaše **umístění** musí odkazovat tooWest USA. 
   > 
   > 
   
    Úpravy hello následující řádky v konfiguračním souboru sítě, nahraďte vlastními hodnotami hello: 
   
    **Původní hodnota:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 
   
    **Nová hodnota:** \<VirtualNetworkSitename = "VNetUSWest" umístění = "Západní USA"\>
3. Uložte změny a [importovat](virtual-networks-using-network-configuration-file.md#import) hello tooAzure konfigurace sítě.

> [!NOTE]
> Tato migrace nezpůsobí žádné výpadky tooyour služby.
> 
> 

## <a name="what-toodo-if-you-have-a-vm-classic-in-an-affinity-group"></a>Jaké toodo, pokud máte virtuální počítač (klasický) ve skupině vztahů
Virtuální počítače (klasické) jsou aktuálně ve skupině vztahů nemusí toobe odebrat ze skupiny vztahů hello. Po nasazení virtuálního počítače je jednotka nasazené tooa jeden škálování. Skupiny vztahů můžete omezit hello sadu dostupných velikostí virtuálních počítačů pro nové nasazení virtuálního počítače, ale existující virtuální počítač, který je nasazen s omezeným přístupem již k dispozici v jednotce škálování hello, ve které hello virtuální počítač nasazen velikosti toohello sadu virtuálních počítačů. Protože hello virtuální počítač je již nasazen jednotky škálování tooa, odebrání virtuálního počítače ze skupiny vztahů nemá žádný vliv na hello virtuálních počítačů.
