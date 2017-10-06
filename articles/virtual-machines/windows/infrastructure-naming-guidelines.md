---
title: "pokyny – Windows pro pojmenování infrastruktury aaaAzure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pravidla pro vytváření názvů v služby infrastruktury Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 660765fa-4d42-49cb-a9c6-8c596d26d221
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9b4a16ce99cf1cac5804c77675e24590ac2e2b33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-windows-vms"></a>Pokyny pro pojmenování pro virtuální počítače Windows Azure infrastruktury

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

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

Můžete zvolit, toofollow konkrétní sadu zásady vytváření názvů definované pro celou organizaci nebo pro konkrétní předplatného Azure nebo účtu. I když je snadné pro jednotlivce v rámci organizace tooestablish implicitní pravidla při práci s prostředky Azure, když tým potřebuje toowork na projektu v Azure, že model nedá se jednoduše škálovat.

Odsouhlasení sadu předem zásady vytváření názvů. Existují některé aspekty týkající se zásadami vytváření názvů vyjmout napříč této sady pravidel.

## <a name="affixes"></a>Přípony
Při pohledu toodefine zásady vytváření názvů, jeden rozhodnutí obsahuje informaci, jestli hello připojí na:

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
| Role |SQL, ora, sp, služby iis |V závislosti na roli hello hello virtuálního počítače. |
| Instance |01, 02, 03, atd. |Pro prostředky, které mají více než jednu instanci. Například s vyrovnáváním zatížení se webové servery v cloudové službě. |

Při vytváření zásady vytváření názvů, ujistěte se, že v vyžadují, jasně uvádějí který opatří toouse pro každý typ prostředku a které pozice (přípona vs předponu).

## <a name="dates"></a>kalendářní data
Často je důležité toodetermine hello datum vytvoření z hello název prostředku. Doporučujeme formát data RRRRMMDD hello. Tento formát zajišťuje, že pouze úplné datum hello zaznamená, ale také že dva prostředky, jejichž názvy se liší pouze na datum hello je seřazen abecedně a časovém pořadí v hello stejnou dobu.

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
Když vytvoříte virtuální počítač (VM), Microsoft Azure vyžaduje název virtuálního počítače z až too15 znaků, který se používá pro název prostředku hello. Azure používá hello pro hello operační systém nainstalovaný v hello virtuálního počítače stejný název. Ale tyto názvy nemusí vždy být hello stejné.

V případě, že virtuální počítač je vytvořen ze souboru bitové kopie VHD, který již obsahuje operační systém, hello název virtuálního počítače v Azure se může lišit od hello název počítače operační systém Virtuálního počítače. Tuto situaci můžete přidat určitý stupeň potíže tooVM správu, který proto nedoporučujeme. Přiřaďte hello hello prostředků virtuálního počítače Azure stejný název jako název počítače hello přiřadit toohello operačního systému tohoto virtuálního počítače.

Doporučujeme vám, že název virtuálního počítače Azure hello je hello stejné jako hello základní název operačního systému počítače.

## <a name="storage-account-names"></a>Názvy účtů úložiště
Tato část se nevztahuje příliš[Azure spravované disky](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), jak vytvářet účet samostatného úložiště. Pro nespravovaná disky účty úložiště mají zvláštní pravidla, kterými se řídí jejich názvy. Můžete použít pouze malá písmena a číslice. Další informace najdete v tématu [vytvořit účet úložiště](../../storage/storage-create-storage-account.md#create-a-storage-account). Kromě toho hello název účtu úložiště, společně s core.windows.net, musí být globálně platný jedinečný název DNS. Například pokud účet úložiště hello nazývá můj_účet_úložiště, hello následující výsledné názvy DNS musí být jedinečné:

* mystorageaccount.BLOB.Core.Windows.NET
* mystorageaccount.Table.Core.Windows.NET
* mystorageaccount.Queue.Core.Windows.NET

## <a name="next-steps"></a>Další kroky
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

