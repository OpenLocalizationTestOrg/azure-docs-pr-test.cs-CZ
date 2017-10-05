---
title: "Pojmenování pokyny - Linux infrastrukturu Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro pojmenování ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ee4fb1-8297-49a1-8d3f-097880be67c7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1b086f0972c02d569a7219820a3d596960b6014b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a>Pokyny pro pojmenování pro virtuální počítače s Linuxem infrastrukturu Azure 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení postupy, dosahují zásady vytváření názvů pro všechny různých prostředků Azure k vytvoření logické a snadno identifikovat sadu prostředků ve vašem prostředí.

## <a name="implementation-guidelines-for-naming-conventions"></a>Postup implementace pro zásady vytváření názvů
Rozhodnutí:

* Jaké jsou vaše zásady vytváření názvů pro prostředky Azure?

Úlohy:

* Definujte přípony k zachování konzistence použít ve vašich prostředků.
* Zadejte názvy účtů úložiště zadaný požadavek mohly být globálně jedinečný.
* Zdokumentujte zásady vytváření názvů, který chcete použít a distribuovat do všech k zachování konzistence napříč nasazení zúčastněných stran.

## <a name="naming-conventions"></a>Zásady vytváření názvů
Dobrý zásady vytváření názvů v místě byste měli mít před vytvořením nic v Azure. Zásady vytváření názvů zajistí, že všechny prostředky předvídatelný název, což pomáhá snížit administrativní zátěž související se správou těchto prostředků.

Můžete zvolit podle konkrétní sadu zásady vytváření názvů definované pro celou organizaci nebo pro konkrétní předplatného Azure nebo účtu. I když je snadné pro jednotlivce v rámci organizace vytvořit implicitní pravidla při práci s prostředky Azure, budete muset být škálovat pro týmy společně v Azure.

Odsouhlasení sadu předem zásady vytváření názvů. Existují některé aspekty týkající se zásadami vytváření názvů které vyjmout napříč nastaví pravidel.

## <a name="affixes"></a>Přípony
Při pohledu definovat zásady vytváření názvů, je jeden rozhodnutí, jestli předpona je na:

* Na začátek názvu (předpona)
* Konci názvu (přípona)

Například tady jsou dvě možné názvy pro skupinu prostředků pomocí `rg` opatří:

* Rg-WebApp (předpona)
* WebApp-Rg (přípona)

Přípony najdete různé aspekty, které popisují konkrétní prostředky. Následující tabulka uvádí některé příklady obvykle používá.

| Aspekt | Příklady | Poznámky |
|:--- |:--- |:--- |
| Prostředí |vývoj, stg, produkčního |V závislosti na účelu a název každé prostředí. |
| Umístění |usw (západní USA), použijte (východní USA 2) |V závislosti na oblasti datovém centru nebo oblasti organizace. |
| Komponenta Azure, služby nebo produktu |Rg pro skupinu prostředků, virtuální sítě pro virtuální síť |V závislosti na produkt, pro kterou prostředek podporuje. |
| Role |DB, aplikace, webová aplikace |V závislosti na roli virtuálního počítače. |
| Instance |01, 02, 03, atd. |Pro prostředky, které mají více než jednu instanci. Například s vyrovnáváním zatížení se webové servery v cloudové službě. |

Při vytváření zásady vytváření názvů, ujistěte se, že vyžadují, jasně uvádějí přípony, které chcete použít pro každý typ prostředku a které pozice (přípona vs předponu).

## <a name="dates"></a>kalendářní data
Často je důležité určit datum vytvoření z názvu prostředku. Doporučujeme, abyste RRRRMMDD formát data. Tento formát zajišťuje, ne jenom celý se zaznamená datum, ale také že dva prostředky, jejichž názvy se liší pouze na data jsou seřazeny podle abecedy a časovém pořadí.

## <a name="naming-resources"></a>Názvy prostředků
Definovat každý typ prostředku v zásady vytváření názvů, které by měl mít pravidla, která definují přiřazení názvy pro každý prostředek, který je vytvořen. Tato pravidla by se měly používat pro všechny typy prostředků, například:

* Předplatná
* Účty
* Účty úložiště
* Virtuální sítě
* Podsítě
* Skupiny dostupnosti
* Skupiny prostředků
* Virtuální počítače
* Koncové body
* Skupiny zabezpečení sítě
* Role

Abyste ověřili, že název poskytuje dostatek informací k určení, který prostředek se odkazuje, používejte popisné názvy.

## <a name="computer-names"></a>Názvy počítačů
Při vytváření virtuálního počítače (VM) Azure vyžaduje název virtuálního počítače až 64 znaků, který se používá pro název prostředku. Azure používá stejný název pro operační systém nainstalovaný ve virtuálním počítači. Ale tyto názvy nemusí být vždy stejné.

Pokud virtuální počítač je vytvořen ze souboru bitové kopie VHD, který již obsahuje operační systém, název virtuálního počítače v Azure se může lišit od názvu počítače operační systém Virtuálního počítače. Tuto situaci můžete přidat určitý stupeň potíže ke správě virtuálních počítačů, které proto nedoporučujeme. Přiřaďte prostředků Azure virtuálního počítače stejný název jako název počítače, který přiřadíte operačního systému tohoto virtuálního počítače.

Doporučujeme vám, že název virtuálního počítače Azure je stejný jako název počítače základní operační systém.

## <a name="storage-account-names"></a>Názvy účtů úložiště
Tato část se nevztahuje na [Azure spravované disky](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), jak vytvářet účet samostatného úložiště. Pro nespravovaná disky účty úložiště mají zvláštní pravidla, kterými se řídí jejich názvy. Můžete použít pouze malá písmena a číslice. Další informace najdete v tématu [vytvořit účet úložiště](../../storage/storage-create-storage-account.md#create-a-storage-account). Název účtu úložiště, s core.windows.net, kromě toho musí být globálně platný jedinečný název DNS. Například pokud účet úložiště, se nazývá můj_účet_úložiště, následující výsledné názvy DNS musí být jedinečné:

* mystorageaccount.BLOB.Core.Windows.NET
* mystorageaccount.Table.Core.Windows.NET
* mystorageaccount.Queue.Core.Windows.NET

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

