---
title: "aaaTroubleshoot nasazení problémy virtuální počítač Windows v Azure | Microsoft Docs"
description: "Řešení potíží nasazení systému Windows virtuálního počítače v modelu nasazení Azurethe Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: genlin
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
ms.openlocfilehash: 30de25827c050cc266761cfc14548bcc64237dac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>Problémy s nasazení systému Windows virtuálního počítače v Azure

problémy při nasazení virtuálního počítače (VM) tootroubleshoot v Azure, zkontrolujte hello [top problémy](#top-issues) pro běžné chyby a řešení.

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.

## <a name="top-issues"></a>Nejdůležitější problémy
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

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

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a>Jak můžete používat a nasazení bitové kopie klienta systému windows do Azure?

Můžete Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování Pokud máte předplatné příslušné sadě Visual Studio (dříve MSDN). To [článku](client-images.md) jsou podrobněji popsány dále hello způsobilosti požadavky pro spuštění klienta systému Windows v Azure a používá hello Image Azure Gallery.

## <a name="how-can-i-deploy-a-virtual-machine-using-hello-hybrid-use-benefit-hub"></a>Nasazení virtuálního počítače pomocí hello hybridní použití zvýhodnění (ROZBOČOVAČ)

Existují dvě různé způsoby toodeploy Windows virtuálních počítačů s hello výhody použití hybridní Azure.

Předplatné Enterprise Agreement:

• Nasazení virtuálních počítačů z konkrétní Marketplace bitové kopie, které jsou předem nakonfigurovaným rozhraním výhody použití hybridní Azure.

Pro smlouvy Enterprise:

• Nahrát vlastní virtuální počítač a nasadit pomocí šablony Resource Manageru nebo Azure PowerShell.

Další informace najdete v tématu hello následující prostředky:

 - [Přehled hybridní použití výhody služby Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [Nejčastější dotazy ke stažení](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - [Výhody použití Azure hybridní pro Windows Server a klienta Windows](hybrid-use-benefit-licensing.md).

 - [Jak můžete použít hello výhody použití hybridní v Azure](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Jak lze aktivovat moje měsíční kredit pro sadu Visual studio Enterprise (BizSpark)

tooactivate vaše měsíční úvěrové, najdete [článku](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="how-tooadd-enterprise-devtest-toomy-enterprise-agreement-ea-tooget-access-toowindow-client-images"></a>Jak tooadd Enterprise pro vývoj/testování toomy Enterprise Agreement (EA) tooget přistupovat k tooWindow obrazy klientů?

Hello možnost toocreate odběry podle hello Enterprise pro vývoj/testování nabízejí je omezená tooAccount vlastníky, kteří mají oprávnění toodo Ano pomocí Správce podnikové sítě. Hello vlastníka účtu vytvoří předplatné prostřednictvím hello portálu účtů Azure a poté by měl přidejte active odběratele Visual Studio jako spolusprávce. Tak, aby můžou spravovat a používat hello prostředky potřebné pro vývoj a testování. Další informace najdete v tématu [Enterprise pro vývoj/testování](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a>Moje ovladače chybí pro virtuální počítač Windows N-Series

Ovladače pro virtuální počítače na bázi Windows, které jsou umístěné [zde](n-series-driver-setup.md).

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>Nelze najít instanci GPU v rámci virtuální počítač N-Series

tootake využívat funkce GPU hello Azure N-series virtuální počítače se systémem Windows Server 2016 nebo Windows Server 2012 R2, je nutné nainstalovat NVIDIA grafické ovladače pro každý virtuální počítač po nasazení. Informace o instalaci ovladačů je k dispozici pro [virtuálních počítačů Windows](n-series-driver-setup.md) a [virtuální počítače s Linuxem](../linux/n-series-driver-setup.md).

## <a name="are-client-images-supported-for-n-series"></a>Jsou podporované Image klienta pro N-Series?

Azure v současné době podporuje pouze N-Series na virtuálních počítačích s operačními systémy Windows Server a Linux.

## <a name="is-n-series-vms-available-in-my-region"></a>Je k dispozici N-Series virtuálních počítačů v mojí oblasti?

Můžete zkontrolovat dostupnost hello z hello [produkty, které jsou dostupné podle oblasti tabulku](https://azure.microsoft.com/regions/services)a ceny [zde](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-tooi-get-them"></a>Jaké obrazy klientů lze použít a nasadit v Azure a jak je tooI získat?

Můžete použít systém Windows 7, Windows 8 nebo Windows 10 v Azure pro scénáře vývoje/testování za předpokladu, že máte příslušné předplatné sady Visual Studio (dříve MSDN). 

- Bitové kopie systému Windows 10 jsou dostupné z Galerie Azure hello v rámci [nabízí vhodné pro vývoj/testování](client-images.md#eligible-offers). 
- Visual Studio odběratele v rámci libovolného typu nabídky můžete také [adekvátní Příprava a vytvoření](prepare-for-upload-vhd-image.md) bitovou kopii 64bitová verze Windows 7, Windows 8 nebo Windows 10 a pak [nahrát tooAzure](upload-generalized-managed.md). použití Hello zůstane omezené toodev a testovací odběratelům active Visual Studio.

To [článku](client-images.md) jsou podrobněji popsány dále hello způsobilosti požadavky pro spuštění klienta systému Windows Azure a použít Dobrý den Image Azure Gallery.

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a>Nejsem řady možné toosee velikost virtuálního počítače, který chci, aby při změně velikosti virtuální počítač.

Když je spuštěný virtuální počítač, je nasazené tooa fyzický server. Hello fyzické servery v oblastech Azure jsou seskupené v clusterech běžné fyzický hardware. Změna velikosti virtuálního počítače, který vyžaduje hello virtuální počítač přesunout toobe toodifferent hardwaru clustery se liší v závislosti na modelu nasazení bylo použité toodeploy hello virtuálních počítačů.

- Virtuální počítače nasazené v modelu nasazení Classic, nasazení hello cloudové služby musí být odebrány a znovu nasadit toochange hello virtuální počítače tooa velikost v jiné rodiny velikost.

- Virtuální počítače nasazené v modelu nasazení Resource Manager, je potřeba zastavit všechny virtuální počítače ve skupině před změnou velikosti hello žádné virtuální počítače v sadě dostupnosti hello dostupnosti hello.

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Hello uvedené velikost virtuálního počítače není podporován při nasazení ve skupině dostupnosti.

Zvolte velikost, která je podporována v nastavení dostupnosti hello clusteru. Při vytváření doporučujeme, že sadu dostupnosti toochoose hello největší velikost virtuálního počítače, které se domníváte, že je třeba, a mít které bude vaše první toohello nasazení, které skupiny dostupnosti.

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a>Můžete přidat stávající sadu dostupnosti tooan Classic virtuálního počítače?

Ano. Můžete přidat existující klasické virtuální počítač tooa nové nebo stávající sadu dostupnosti. Další informace najdete v části [přidat stávající sadu dostupnosti virtuálního počítače tooan](classic/configure-availability.md#addmachine).


## <a name="next-steps"></a>Další kroky
Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/).

Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.
