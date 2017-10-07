---
title: aplikace aaaReplicate (Azure tooAzure) | Microsoft Docs
description: "Tento článek popisuje, jak tooset replikaci virtuálního počítače, spuštěná v jedné oblasti Azure příliš jiné oblasti v Azure."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: fb190dac14419f892a1c6b45a3d991d8005e4bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-virtual-machines-tooanother-azure-region"></a>Replikovat virtuální počítače Azure tooanother oblast Azure



>[!NOTE]
>
> Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.

Tento článek popisuje, jak tooset replikaci virtuálního počítače, spuštěná v jedné oblasti Azure tooanother oblast Azure.

## <a name="prerequisites"></a>Požadavky

* Hello článek předpokládá, že již víte o Site Recovery a trezoru služeb zotavení. Je nutné toohave po předem trezoru služeb zotavení vytvořili.

    >[!NOTE]
    >
    > Doporučuje se vytvořit hello trezoru služeb zotavení v hello umístění, kam má tooreplicate vaše virtuální počítače. Například pokud cílové umístění, střed USA", vytvořte trezor v, střed USA".

* Pokud používáte pravidel skupiny zabezpečení sítě (NSG) nebo brány firewall proxy toocontrol přístup toooutbound připojení k Internetu na hello virtuálních počítačích Azure, zajistěte, že je seznam povolených adres hello požadované adresy URL nebo IP adresy. Odkazovat příliš[sítě pokyny dokumentu](./site-recovery-azure-to-azure-networking-guidance.md) další podrobnosti.

* Pokud máte ExpressRoute nebo VPN připojení mezi místními a hello zdrojové umístění v Azure, postupujte podle [aspekty obnovení lokality pro místní tooon Azure ExpressRoute nebo konfiguraci sítě VPN](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentu.

* Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci virtuálního počítače Azure.

* Vaše předplatné Azure musí být povoleno toocreate virtuální počítače v cílovém umístění hello chcete toouse jako oblast zotavení po Havárii. Obraťte se na podporu tooenable hello požadované kvóty.

## <a name="enable-replication-from-azure-site-recovery-vault"></a>Povolit replikaci z trezoru Azure Site Recovery
Pro tento obrázek jsme bude replikovat virtuální počítače spuštěné v toohello umístění Azure, východní Asie, hello, jihovýchodní Asie, umístění. Hello kroky jsou následující:

 Klikněte na tlačítko **+ replikovat** v hello trezoru tooenable replikace pro virtuální počítače hello.

1. **Zdroj:** odkazuje toohello bod počátku hello počítače, který v tomto případě je **Azure**.

2. **Umístění zdroje:** je hello oblast Azure, ze kterého má tooprotect virtuálních počítačů. Pro tento obrázek umístění zdroje hello budou, východní Asie.

3. **Model nasazení:** odkazuje model nasazení Azure toohello hello zdrojového počítače. Můžete vybrat buď classic nebo resource manager a patřící toohello konkrétní model počítače budou uvedené pro ochranu v dalším kroku hello.

      >[!NOTE]
      >
      > Můžete replikovat klasické virtuální počítač a obnovit jako virtuální počítač s classic. Nelze obnovit jako virtuální počítač Resource Manager.

4. **Skupina prostředků:** ho je toowhich skupiny prostředků hello patří zdroj virtuálních počítačů. Pro ochranu v dalším kroku hello se objeví hello všechny virtuální počítače v části hello vybranou skupinu prostředků.

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

V **virtuální počítače > vyberte virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na OK.
    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)


V části nastavení můžete nakonfigurovat vlastnosti cílové lokality

1. **Cílové umístění:** jde hello umístění, kde bude replikován zdrojová data virtuálního počítače. V závislosti na vaší umístění vybrané počítače se Site Recovery zajistí, že hello seznam vhodnou cílovou oblastí.

    > [!TIP]
    > Je doporučeno tookeep cílové umístění stejné od vaší obnovení služeb trezoru.

2. **Cílová skupina prostředků:** je toowhich skupiny prostředků hello bude patřit všechny replikované virtuální počítače. Ve výchozím nastavení automatické obnovení systému vytvoří novou skupinu prostředků v hello cílová oblast s názvem, který má příponu "Automatické obnovení systému". V případě, že skupiny prostředků vytvořené automatické obnovení systému již existuje, bude znovu. Můžete také toocustomize ho jak je znázorněno v následující části hello.    
3. **Cílová virtuální síť:** ve výchozím nastavení, automatické obnovení systému vytvoří nové virtuální sítě v hello cílová oblast s názvem, který má příponu "Automatické obnovení systému". To bude namapované tooyour zdrojové síti a bude se používat pro všechny budoucí ochranu.

    > [!NOTE]
    > [Sítě v podrobnostech](site-recovery-network-mapping-azure-to-azure.md) tooknow více informací o mapování sítě.

4. **Cílové úložiště účtů:** ve výchozím nastavení, automatické obnovení systému vytvoří hello nový cílový účet úložiště mimicking konfiguraci zdrojového virtuálního počítače úložiště. V případě, že účet úložiště, které jsou vytvořené automatické obnovení systému již existuje, bude znovu.

5. **Účty úložiště do mezipaměti:** automatické obnovení systému potřebuje účet úložiště s názvem úložiště mezipaměti v oblasti zdroj hello. Všechny hello změny děje na hello zdrojové virtuální počítače jsou sledovány a odesílat účet úložiště toocache před replikace těchto toohello cílové umístění.

6. **Skupina dostupnosti:** ve výchozím nastavení, vytvoří nástroj Automatické obnovení systému novou skupinou dostupnosti ve hello cílová oblast s názvem, který má příponu "Automatické obnovení systému". V případě, že skupina dostupnosti vytvořené automatické obnovení systému již existuje, bude znovu.

7.  **Zásady replikace:** definuje hello nastavení pro obnovení bodu uchování historie a aplikace snímky konzistentní s četnost. Ve výchozím nastavení bude automatické obnovení systému vytvořte novou zásadu replikace s výchozím nastavením: 24 hodin se pro uchování bodu obnovení a ' 60 minut frekvence snímky konzistentní s aplikací.

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Přizpůsobení cílové prostředky

V případě, že chcete toochange hello výchozí hodnoty používané automatické obnovení systému, můžete změnit nastavení hello na základě potřeb.

1. **Přizpůsobení:** klikněte na něj toochange hello výchozí hodnoty používané automatické obnovení systému.

2. **Cílová skupina prostředků:** můžete vybrat skupinu prostředků hello z hello seznam všech skupin prostředků hello existující v cílovém umístění hello v rámci předplatného hello.

3. **Cílová virtuální síť:** hello seznam všech hello virtuální sítě můžete najít v cílovém umístění hello.

4. **Skupina dostupnosti:** lze přidat pouze dostupnost sady nastavení toohello virtuální počítače, které jsou součástí dostupnosti v oblasti zdroje.

5. **Cílem účtů úložiště:**

![Povolit replikaci](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klikněte na **vytvořit cílový prostředek** a zapnout replikaci


Jakmile jsou virtuální počítače chráněné můžete zkontrolovat stav hello stavu virtuální počítače v části **replikované položky**

>[!NOTE]
>Během hello může době počáteční replikace existuje možnost, že stav trvá toorefresh čas a nevidíte průběh nějakou dobu. Můžete klikněte na tlačítko Aktualizovat hello hello nahoře hello okno tooget hello nejnovější stavu.
>

![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Další kroky
- [Další informace](site-recovery-test-failover-to-azure.md) o spuštění převzetí služeb při selhání.
- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.
- Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md) tooreduce RTO.
- Další informace o [opětovnou ochranu virtuálních počítačů Azure](site-recovery-how-to-reprotect.md) po převzetí služeb při selhání.
