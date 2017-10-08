---
title: "aaaDesign důležité informace týkající se sady škálování virtuálního počítače Azure | Microsoft Docs"
description: "Další informace o aspektech návrhu pro vaše sady škálování virtuálního počítače Azure"
keywords: "virtuální počítač Linux, sadách škálování virtuálních počítačů"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: c27c6a59-a0ab-4117-a01b-42b049464ca1
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: f8644d36fe5903bd4b74df26dca5dc3211ee3516
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-considerations-for-scale-sets"></a>Aspekty návrhu pro sady škálování
Toto téma popisuje aspekty návrhu pro sady škálování virtuálního počítače. Informace o tom, co jsou sady škálování virtuálního počítače, najdete v části příliš[Přehled sady škálování virtuálních počítačů](virtual-machine-scale-sets-overview.md).

## <a name="when-toouse-scale-sets-instead-of-virtual-machines"></a>Když sadách škálování toouse místo virtuální počítače?
Obecně platí škálovatelné sady jsou užitečné pro nasazení infrastruktury s vysokou dostupností kde sada počítačů mají podobné konfigurace. Ale některé funkce jsou k dispozici pouze v sadách škálování při další funkce jsou dostupné jen ve virtuálních počítačích. V pořadí toomake informované rozhodnutí o tom, kdy toouse každou technologii jsme měli nejdřív podívejte se na některé z nejčastěji používaných hello funkce, které jsou k dispozici v sady škálování, ale není virtuální počítače:

### <a name="scale-set-specific-features"></a>Funkce specifické pro sadu škálování

- Jakmile zadáte konfigurace sady škálování hello, můžete jednoduše aktualizovat hello "kapacity" vlastnost toodeploy více virtuálních počítačů současně. Toto je mnohem jednodušší než zápisu skriptu tooorchestrate, nasazení hodně jednotlivých virtuálních počítačů současně.
- Můžete [použití automatického škálování Azure tooautomatically škálování škálovací sadu](./virtual-machine-scale-sets-autoscale-overview.md) , ale ne jednotlivé virtuální počítače.
- Můžete [obnovení z Image škálovací sady virtuálních počítačů](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-a-vm) ale [není jednotlivé virtuální počítače](https://docs.microsoft.com/rest/api/compute/virtualmachines).
- Můžete [overprovision](./virtual-machine-scale-sets-design-overview.md) sady škálování virtuálních počítačů pro větší spolehlivost a rychlejší nasazení časy. Provést nelze s jednotlivé virtuální počítače Pokud můžete psát vlastní kód toodo to.
- Můžete zadat [zásad upgradu](./virtual-machine-scale-sets-upgrade-scale-set.md) toomake ho snadno tooroll se upgrady mezi virtuální počítače ve škálovací sadě. S jednotlivé virtuální počítače které musí orchestraci aktualizací sami.

### <a name="vm-specific-features"></a>Funkce specifické pro virtuální počítač

Na hello druhé straně, některé funkce jsou dostupné jen ve virtuálních počítačích (alespoň pro hello čas probíhá):

- Můžete připojit datové disky toospecific jednotlivé virtuální počítače, ale připojené datové disky jsou nakonfigurovány pro všechny virtuální počítače ve škálovací sadě.
- Můžete připojit tooindividual neprázdný datové disky virtuálních počítačů, ale není virtuální počítače ve škálovací sadě.
- Můžete snímek konkrétního virtuálního počítače, ale ne virtuální počítač ve škálovací sadě.
- Můžete zaznamenat bitovou kopii z konkrétního virtuálního počítače, ale ne z virtuálního počítače ve škálovací sadě.
- Konkrétního virtuálního počítače můžete migrovat z disků toomanaged nativních discích, ale provést nelze pro virtuální počítače ve škálovací sadě.
- Můžete přiřadit IPv6 veřejné IP adresy virtuálních počítačů tooindividual síťové adaptéry, ale nelze učinit pro virtuální počítače ve škálovací sadě. Všimněte si, že IPv6 veřejné IP adresy můžete přiřadit vyrovnávání tooload před buď jednotlivé virtuální počítače nebo sady škálování virtuálních počítačů.

## <a name="storage"></a>Úložiště

### <a name="scale-sets-with-azure-managed-disks"></a>Sady škálování s Azure spravované disky
Sady škálování může být vytvořen pomocí [Azure spravované disky](../virtual-machines/windows/managed-disks-overview.md) místo účty tradiční úložiště Azure. Spravované disky poskytují hello následující výhody:
- Nemáte toopre-vytvořit sadu účtů úložiště Azure pro hello škálovací sady virtuálních počítačů.
- Můžete definovat [připojené datových disků](virtual-machine-scale-sets-attached-disks.md) hello virtuální počítače ve vaší škálování nastavit.
- Sady škálování lze nakonfigurovat příliš[podporovat až too1 000 virtuálních počítačů v sadě](virtual-machine-scale-sets-placement-groups.md). 

Pokud máte existující šablony, můžete také [aktualizovat hello šablony toouse spravované disky](virtual-machine-scale-sets-convert-template-to-md.md).

### <a name="user-managed-storage"></a>Spravovaná uživatelem úložiště
Sada škálování, které není definovaná s Azure spravované disky spoléhá na disky OS uživatelem vytvořené úložiště účtů toostore hello hello virtuálních počítačů v sadě hello. Poměr 20 virtuálních počítačů na účet úložiště nebo méně se doporučuje maximální vstupně-výstupní operace tooachieve a také využívá výhod _předimenzování_ (viz níže). Dále je doporučeno, rozkládá hello začátku znaků názvy účtů úložiště hello mezi hello abecedy. V tom pomáhá šíří zatížení napříč různými systémy interní. 


## <a name="overprovisioning"></a>Předimenzování
Škálování nastaví aktuálně výchozí příliš "předimenzování" virtuální počítače. S předimenzování zapnutý, hello škálování nastavit monitoru je ve skutečnosti nestabilní až více virtuálních počítačů, než jste žádali, a pak odstraní hello, které jsou úspěšně zřízené navíc za virtuální počítače po hello požadovaný počet virtuálních počítačů. Předimenzování zvyšuje úspěšnost zřizování a snižuje čas nasazení. Fakturuje nejsou pro hello navíc za virtuální počítače a jejich nepočítá vaší kvóty.

Předimenzování zlepšit úspěšnost zřizování, může to způsobit matoucí chování pro aplikaci, která není navrženou toohandle zobrazování a pak jindy mizí další virtuální počítače. tooturn předimenzování vypnutý, ujistěte se, máte následující řetězec v šabloně hello: `"overprovision": "false"`. Další podrobnosti naleznete v hello [dokumentace k REST API nastavit škálování](/rest/api/virtualmachinescalesets/create-or-update-a-set).

Pokud škálovací sadu používá uživatele spravovat úložiště a vypnete předimenzování, může mít více než 20 virtuálních počítačů na účet úložiště, ale není doporučeno toogo výše 40 z důvodů výkonu vstupně-výstupní operace. 

## <a name="limits"></a>Omezení
Sada škálování založený na image pořízenou prostřednictvím Marketplace (také označované jako image platformy) a nakonfigurovat toouse disky spravované Azure podporuje kapacitou až too1 000 virtuálních počítačů. Pokud nakonfigurujete vaší toosupport sady škálování virtuálních počítačů více než 100, ne všechny scénáře pracovní stejné hello (pro příklad Vyrovnávání zatížení). Další informace najdete v tématu [práce s škálovací sady virtuálních počítačů velké](virtual-machine-scale-sets-placement-groups.md). 

Nastavit nakonfigurovaný s účty úložiště uživatele, spravovat škálování je aktuálně omezená too100 virtuální počítače (a 5 účty úložiště se doporučují pro tohoto měřítka).

Sada škálování založený na vlastní image (jedna je sestavena) může mít kapacitou až too100 virtuálních počítačů při konfiguraci s Azure spravované disky. Pokud sadě škálování hello je nakonfigurovaný s účty úložiště spravované uživatele, musíte vytvořit všechny VHD disku operačního systému v rámci jednoho účtu úložiště. V důsledku toho hello maximální počet virtuálních počítačů ve škálovací sadě založený na vlastní image a doporučené spravovaná uživatelem úložiště je 20. Pokud vypnete předimenzování, můžete přejít do too40.

Pro další virtuální počítače než povolit tyto limity, je nutné toodeploy více škálování nastaví, jak je znázorněno v [této šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).

