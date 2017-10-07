---
title: "hello aaaCreate sadu škálování virtuálního počítače pomocí portálu Azure | Microsoft Docs"
description: "Nasazení sad škálování pomocí portálu Azure."
keywords: "Sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23c88f4b1ba99994a38f8886f60735da74e5c17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-hello-azure-portal"></a>Jak hello toocreate a sadu škálování virtuálního počítače pomocí portálu Azure
Tento kurz ukazuje, jak je snadné toocreate sadu škálování virtuálního počítače v několika málo minut pomocí portálu Azure hello. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="choose-hello-vm-image-from-hello-marketplace"></a>Zvolte hello image virtuálního počítače z hello marketplace
Z portálu hello můžete snadno nasadit škálování s CentOS, CoreOS, Debian, otevřete Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server nebo bitové kopie systému Windows Server.

Nejprve přejděte toohello [portál Azure](https://portal.azure.com) ve webovém prohlížeči. Klikněte na tlačítko `New`, vyhledejte `scale set`a potom vyberte hello `Virtual machine scale set` položku:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-hello-scale-set"></a>Vytvořit sadu škálování hello
Teď můžete použít výchozí nastavení hello a rychle vytvořit hello škálovací sadu.

* Na hello `Basics` okno, zadejte název pro sadu škálování hello. Tento název bude hello základní hello plně kvalifikovaný název domény nástroje pro vyrovnávání zatížení hello před sadě škálování hello, takže přesvědčte, že jste název hello je jedinečné ve všech Azure.
* Vyberte požadovanou operačním systému zadejte, zadejte své požadované uživatelské jméno a vyberte typ ověřování, které dávají přednost. Pokud si zvolíte heslo, musí být nejméně 12 znaků dlouhé a splňovat tři mimo hello čtyři následující požadavky na složitost: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak. Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm). Pokud se rozhodnete `SSH public key`, že tooonly vložení v veřejný klíč, ne privátní klíč:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Zvolte, zda byste jako sady škálování hello toolimit tooa umístění jedné skupiny, nebo zda pokrývat více skupin pro umístění. Povolení škálování hello nastavit toospan umístění skupiny umožňuje pro sadách škálování virtuálních počítačů více než 100 kapacity (až too1, 000) s některými omezeními. Další informace najdete v tématu [této dokumentace](./virtual-machine-scale-sets-placement-groups.md).
* Zadejte název skupiny požadovaných prostředků a umístění a pak klikněte na tlačítko `OK`.
* Na hello `Virtual machine scale set service settings` okno: Zadejte vaše Popisek názvu domény požadované (hello base hello plně kvalifikovaný název domény pro službu Vyrovnávání zatížení hello před sadě škálování hello). Tento popisek musí být jedinečný v rámci všech Azure.
* Zvolte si image disku požadovaný operační systém, počet instancí a velikost počítače.
* Vyberte typ požadovaný disk: spravované nebo nespravované. Další informace najdete v tématu [této dokumentace](./virtual-machine-scale-sets-managed-disks.md). Pokud jste zvolili toohave hello škálovací sadu rozmístěny v několika skupinách pro umístění, tato možnost nebude dostupná, protože spravovaných disků na je vyžadován pro škálování sady toospan umístění skupiny.
* Povolit nebo zakázat automatické škálování a nakonfigurujte, pokud je povoleno:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Na hello `Summary` okno, pokud se provádí ověřování, klikněte na tlačítko `OK` toostart hello měřítko nastavit nasazení.


## <a name="connect-tooa-vm-in-hello-scale-set"></a>Připojit tooa virtuálních počítačů v sadě škálování hello
Pokud jste zvolili toolimit vaše škálování nastavte tooa jeden umístění skupiny a pak sadě škálování hello se nasazuje s toolet nakonfigurovaná pravidla NAT připojit snadno sad škálování toohello (Pokud ne, tooconnect toohello virtuální počítače ve škálovací hello nastavena, je pravděpodobně třeba toocreate jumpbox v hello stejné virtuální síti jako sadě škálování hello). toosee, přejděte toohello `Inbound NAT Rules` kartě hello Vyrovnávání zatížení pro sadu škálování hello:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Tooeach sady virtuálního počítače ve škálovací hello pomocí těchto pravidel NAT se můžete připojit. Například pro sadu škálování systému Windows, pokud je pravidlo NAT na příchozí port 50000, můžete se připojit toothat počítač prostřednictvím protokolu RDP na `<load-balancer-ip-address>:50000`. Pro sadu škálování Linux byste připojili pomocí příkazu hello `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Další kroky
Dokumentaci na tom, jak sadách škálování toodeploy z hello rozhraní příkazového řádku najdete v tématu [této dokumentace](virtual-machine-scale-sets-cli-quick-create.md).

Dokumentaci na tom, jak sadách škálování toodeploy z prostředí PowerShell najdete v tématu [této dokumentace](virtual-machine-scale-sets-windows-create.md).

Dokumentaci na tom, jak sadách škálování toodeploy ze sady Visual Studio najdete v tématu [této dokumentace](virtual-machine-scale-sets-vs-create.md).

Pro obecnou dokumentací, podívejte se na hello [stránky přehled dokumentace pro sady škálování](virtual-machine-scale-sets-overview.md).

Obecné informace, podívejte se na hello [hlavní cílová stránka pro sady škálování](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

