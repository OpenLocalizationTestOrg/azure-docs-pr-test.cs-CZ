---
title: "Infrastruktura aaaAzure pojmenování pokyny - Linux | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pravidla pro vytváření názvů v služby infrastruktury Azure."
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
ms.openlocfilehash: 333146e7b2071e43527a5d7dc2ec02ebfb316eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a>Pokyny pro pojmenování pro virtuální počítače s Linuxem infrastrukturu Azure 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Tento článek se zaměřuje na pochopení, jak nastavit tooapproach zásady vytváření názvů pro všechny vaše různých prostředků Azure toobuild a logické a snadno identifikovat prostředků ve vašem prostředí.

## <a name="implementation-guidelines-for-naming-conventions"></a>Postup implementace pro zásady vytváření názvů
Rozhodnutí:

* Jaké jsou vaše zásady vytváření názvů pro prostředky Azure?

Úlohy:

* Definujte hello přípony toouse napříč konzistence toomaintain vaše prostředky.
* Zadejte účet úložiště, že názvy zadané hello požadavek pro ně toobe globálně jedinečný.
* Dokument hello naming convention toobe používá a distribuovat tooall strany související se situací tooensure konzistence do nasazení.

## <a name="naming-conventions"></a>Zásady vytváření názvů
Dobrý zásady vytváření názvů v místě byste měli mít před vytvořením nic v Azure. Zásady vytváření názvů zajistí, že všechny prostředky hello předvídatelný název, který pomáhá nižší hello administrativní zátěž související se správou těchto prostředků.

Můžete zvolit, toofollow konkrétní sadu zásady vytváření názvů definované pro celou organizaci nebo pro konkrétní předplatného Azure nebo účtu. I když je snadné pro jednotlivce v rámci organizace tooestablish implicitní pravidla při práci s prostředky Azure, musíte mít tooscale toobe pro týmy společně v Azure.

Odsouhlasení sadu předem zásady vytváření názvů. Existují některé aspekty týkající se zásadami vytváření názvů které vyjmout napříč nastaví pravidel.

## <a name="affixes"></a>Přípony
Při pohledu toodefine zásady vytváření názvů, jeden rozhodnutí je jestli hello připojí na:

* začátek Hello hello názvu (předpona)
* Hello konci názvu hello (přípona)

Například tady jsou dvě možné názvy pro skupinu prostředků pomocí hello `rg` opatří:

* Rg-WebApp (předpona)
* WebApp-Rg (přípona)

Přípony najdete toodifferent aspekty, které popisují hello konkrétní prostředky. Hello následující tabulka uvádí některé příklady obvykle používá.

| Aspekt | Příklady | Poznámky |
|:--- |:--- |:--- |
| Prostředí |vývoj, stg, produkčního |V závislosti na účelu hello a název každé prostředí. |
| Umístění |usw (západní USA), použijte (východní USA 2) |V závislosti na oblast hello hello datacenter nebo oblasti hello hello organizace. |
| Komponenta Azure, služby nebo produktu |Rg pro skupinu prostředků, virtuální sítě pro virtuální síť |V závislosti na hello produktů, pro které hello prostředků poskytuje podporu. |
| Role |DB, aplikace, webová aplikace |V závislosti na roli hello hello virtuálního počítače. |
| Instance |01, 02, 03, atd. |Pro prostředky, které mají více než jednu instanci. Například s vyrovnáváním zatížení se webové servery v cloudové službě. |

Při vytváření zásady vytváření názvů, ujistěte se, že v vyžadují, jasně uvádějí který opatří toouse pro každý typ prostředku a které pozice (přípona vs předponu).

## <a name="dates"></a>kalendářní data
Často je důležité toodetermine hello datum vytvoření z hello název prostředku. Doporučujeme formát data RRRRMMDD hello. Tento formát zajišťuje, že ne jenom hello úplné datum zaznamená, ale také že dva prostředky, jejichž názvy se liší pouze na datum hello jsou seřazeny podle abecedy a časovém pořadí.

## <a name="naming-resources"></a>Názvy prostředků
Definování každý typ prostředku v hello zásady vytváření názvů, které by měl mít pravidla, která definují, jak tooassign názvy prostředků tooeach který je vytvořen. Tato pravidla by se měly používat tooall typů prostředků, například:

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

tooensure, který hello název poskytuje dostatek prostředků toowhich toodetermine informace, které se odkazuje, byste měli používat popisné názvy.

## <a name="computer-names"></a>Názvy počítačů
Při vytváření virtuálního počítače (VM) Azure vyžaduje název virtuálního počítače z až too64 znaků používaný pro název prostředku hello. Azure používá hello pro hello operační systém nainstalovaný v hello virtuálního počítače stejný název. Ale tyto názvy nemusí vždy být hello stejné.

Pokud virtuální počítač je vytvořen ze souboru bitové kopie VHD, který již obsahuje operační systém, hello název virtuálního počítače v Azure se může lišit od hello název počítače operační systém Virtuálního počítače. Tuto situaci můžete přidat určitý stupeň potíže tooVM správu, který proto nedoporučujeme. Přiřaďte hello hello prostředků virtuálního počítače Azure stejný název jako název počítače hello přiřadit toohello operačního systému tohoto virtuálního počítače.

Doporučujeme vám, že název virtuálního počítače Azure hello je hello stejné jako hello základní název operačního systému počítače.

## <a name="storage-account-names"></a>Názvy účtů úložiště
Tato část se nevztahuje příliš[Azure spravované disky](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), jak vytvářet účet samostatného úložiště. Pro nespravovaná disky účty úložiště mají zvláštní pravidla, kterými se řídí jejich názvy. Můžete použít pouze malá písmena a číslice. Další informace najdete v tématu [vytvořit účet úložiště](../../storage/storage-create-storage-account.md#create-a-storage-account). Kromě toho hello název účtu úložiště, s core.windows.net, musí být globálně platný jedinečný název DNS. Například pokud účet úložiště hello nazývá můj_účet_úložiště, hello následující výsledné názvy DNS musí být jedinečné:

* mystorageaccount.BLOB.Core.Windows.NET
* mystorageaccount.Table.Core.Windows.NET
* mystorageaccount.Queue.Core.Windows.NET

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

