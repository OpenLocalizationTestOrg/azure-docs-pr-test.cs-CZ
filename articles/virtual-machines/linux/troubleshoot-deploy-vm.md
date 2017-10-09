---
title: "aaaTroubleshoot nasazení problémy Linux virtuálního počítače v Azure | Microsoft Docs"
description: "Řešení potíží nasazení Linux virtuálního počítače v modelu nasazení Azurethe Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: genli
ms.openlocfilehash: d1092ca3d9d51af7510b19c8c9a79e6dda588697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>Problémy s nasazení Linux virtuálního počítače v Azure

problémy při nasazení virtuálního počítače (VM) tootroubleshoot v Azure, zkontrolujte hello [top problémy](#top-issues) pro běžné chyby a řešení.

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.

## <a name="top-issues"></a>Nejdůležitější problémy
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a>Hello clusteru nepodporuje hello požadovaná velikost virtuálního počítače
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Opakujte žádost hello pomocí menší velikost virtuálního počítače.
- Pokud hello požadovaná velikost hello že virtuálních počítačů nelze změnit:
    - Zastavte všechny virtuální počítače hello v sadě dostupnosti hello. Klikněte na tlačítko **skupiny prostředků** > vaší skupiny prostředků > **prostředky** > vaše skupina dostupnosti > **virtuální počítače** > virtuálního počítače > **Zastavit**.
    - Po všech hello zastavit virtuální počítače, vytvořte hello virtuálních počítačů v hello potřeby velikost.
    - Spusťte nejprve hello nový virtuální počítač a potom vyberete jednotlivé hello zastaví virtuální počítače a klikněte na příkaz spustit.


## <a name="hello-cluster-does-not-have-free-resources"></a>Hello cluster nebude mít uvolnění prostředků
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Opakujte žádost hello později.
- Pokud můžete technologie hello nový virtuální počítač součástí jiné dostupnosti nastavit
    - Vytvoření virtuálního počítače v sadě dostupnosti jinou (v hello stejné oblasti).
    - Přidat nový virtuální počítač toohello hello stejné virtuální síti.

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Jak lze aktivovat moje měsíční kredit pro sadu Visual studio Enterprise (BizSpark)

tooactivate vaše měsíční úvěrové, najdete [článku](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="why-can-i-not-install-hello-gpu-driver-for-an-ubuntu-nv-vm"></a>Proč nelze nainstalovat ovladač GPU hello pro virtuálního počítače s Ubuntu vs?

V současné době Linux GPU podpora je k dispozici pouze na virtuálních počítačích Azure NC systémem Ubuntu Server 16.04 LTS. Další informace najdete v tématu [nastavení GPU ovladače pro N-series virtuální počítače se systémem Linux](n-series-driver-setup.md).

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>Moje ovladače chybí pro virtuální počítač Linux N-Series

Ovladače pro virtuální počítače na bázi systému Linux jsou umístěné [zde](n-series-driver-setup.md). 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>Nelze najít instanci GPU v rámci virtuální počítač N-Series

tootake využívat funkce GPU hello Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, je nutné nainstalovat NVIDIA grafické ovladače pro každý virtuální počítač po nasazení. Informace o instalaci ovladačů je k dispozici pro [virtuálních počítačů Windows](../windows/n-series-driver-setup.md) a [virtuální počítače s Linuxem](n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>Je k dispozici N-Series virtuálních počítačů v mojí oblasti?

Můžete zkontrolovat dostupnost hello z hello [produkty, které jsou dostupné podle oblasti tabulku](https://azure.microsoft.com/regions/services)a ceny [zde](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a>Nejsem řady možné toosee velikost virtuálního počítače, který chci, aby při změně velikosti virtuální počítač.

Když je spuštěný virtuální počítač, je nasazené tooa fyzický server. Hello fyzické servery v oblastech Azure jsou seskupené v clusterech běžné fyzický hardware. Změna velikosti virtuálního počítače, který vyžaduje hello virtuální počítač přesunout toobe toodifferent hardwaru clustery se liší v závislosti na modelu nasazení bylo použité toodeploy hello virtuálních počítačů.

- Virtuální počítače nasazené v modelu nasazení Classic, nasazení hello cloudové služby musí být odebrány a znovu nasadit toochange hello virtuální počítače tooa velikost v jiné rodiny velikost.

- Virtuální počítače nasazené v modelu nasazení Resource Manager, je potřeba zastavit všechny virtuální počítače ve skupině před změnou velikosti hello žádné virtuální počítače v sadě dostupnosti hello dostupnosti hello.

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Hello uvedené velikost virtuálního počítače není podporován při nasazení ve skupině dostupnosti.

Zvolte velikost, která je podporována v nastavení dostupnosti hello clusteru. Při vytváření doporučujeme, že sadu dostupnosti toochoose hello největší velikost virtuálního počítače, které se domníváte, že je třeba, a mít které bude vaše první toohello nasazení, které skupiny dostupnosti.

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Jaké distribucí Linux nebo verze jsou podporovány v Azure?

Seznam hello v Linux najdete na [Azure-Endorsed distribuce](endorsed-distros.md).

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a>Můžete přidat stávající sadu dostupnosti tooan Classic virtuálního počítače?

Ano. Můžete přidat existující klasické virtuální počítač tooa nové nebo stávající sadu dostupnosti. Další informace najdete v části [přidat stávající sadu dostupnosti virtuálního počítače tooan](../windows/classic/configure-availability.md#addmachine).


## <a name="next-steps"></a>Další kroky
Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).

Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.
