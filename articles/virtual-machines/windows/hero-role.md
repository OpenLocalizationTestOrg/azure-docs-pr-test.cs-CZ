---
title: "aaaInstall IIS na vaše první virtuální počítač s Windows | Microsoft Docs"
description: "Experimentujte s první virtuální počítač s Windows pomocí instalace služby IIS a otevření portu 80 pomocí hello portálu Azure."
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6334ea45-db6b-4e08-abb5-1f98927ebc34
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: cynthn
ms.openlocfilehash: 7cfed6197df058c4569d111ee88961da7c6fe0b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Experimentujte s instalaci role na vašem virtuálním počítači Windows
Až budete mít první virtuální počítač (VM) nahoru a spuštěna, můžete přesunout na tooinstalling softwaru a služeb. V tomto kurzu budeme toouse správce serveru na virtuální počítač Windows serveru tooinstall hello služby IIS. Poté vytvoříme skupinu zabezpečení sítě (NSG) pomocí hello Azure portálu tooopen port 80 tooIIS provoz. 

Pokud jste ještě nevytvořili vaše první virtuální počítač, přejděte zpět příliš[vytvořit svůj první virtuální počítač Windows v hello portál Azure](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před pokračováním v tomto kurzu.

## <a name="make-sure-hello-vm-is-running"></a>Ujistěte se, zda text hello, který je spuštěný virtuální počítač
1. Otevřete hello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello, klikněte na tlačítko **virtuální počítače**. Vyberte virtuální počítač hello hello seznamu.
3. Pokud je stav hello **zastaveno (Deallocated)**, klikněte na tlačítko hello **spustit** na hello tlačítko **Essentials** okno hello virtuálních počítačů. Pokud je stav hello **systémem**, můžete přesunout na další krok toohello.

## <a name="connect-toohello-virtual-machine-and-sign-in"></a>Připojit toohello virtuálního počítače a přihlášení
1. V nabídce centra hello, klikněte na tlačítko **virtuální počítače**. Vyberte virtuální počítač hello hello seznamu.
2. V okně hello hello virtuálního počítače, klikněte na tlačítko **Connect**. Tím se vytvoří a stáhne soubor Remote Desktop Protocol (soubor .rdp), který je třeba na počítači tooyour tooconnect zástupce. Můžete chtít toosave hello souboru tooyour desktop pro snadný přístup. **Otevřete** tooyour tooconnect tento soubor virtuálního počítače.
   
    ![Snímek obrazovky portálu Azure znázorňující hello jak tooconnect tooyour virtuálních počítačů](./media/hero-role/connect.png)
3. Se zobrazí upozornění, že hello RDP je od neznámého vydavatele. To je normální. V okně hello vzdálené plochy, klikněte na tlačítko **připojit** toocontinue.
   
    ![Snímek obrazovky s upozorněním na neznámého vydavatele](./media/hero-role/rdp-warn.png)
4. V okně zabezpečení systému Windows hello hello typ hello uživatelské jméno a heslo pro hello místní účet, který jste vytvořili při vytváření virtuálního počítače. uživatelské jméno Hello je zadán jako *vmname*&#92; *uživatelské jméno*, pak klikněte na tlačítko **OK**.
   
    ![Snímek obrazovky zadání hello název virtuálního počítače, uživatelské jméno a heslo](./media/hero-role/credentials.png)
5. Zobrazí se upozornění certifikátu hello nelze ověřit. To je normální. Klikněte na tlačítko **Ano** tooverify hello identity hello virtuální počítač a dokončete přihlášení.
   
   ![Snímek obrazovky zobrazující zprávu o ověření identity hello hello virtuálních počítačů](./media/hero-role/cert-warning.png)

Pokud spustíte tootrouble při pokusu o tooconnect, najdete v části [tooa připojení řešení Vzdálená plocha systému Windows virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="install-iis-on-your-vm"></a>Instalace služby IIS na vašem virtuálním počítači
Teď, když jste přihlášení toohello virtuálních počítačů, se nainstaluje role serveru, aby můžete experimentovat více.

1. Otevřete **Správce serveru**, pokud ještě není otevřený. Klikněte na tlačítko hello **spustit** nabídce a pak klikněte na tlačítko **správce serveru**.
2. V **správce serveru**, vyberte **místní Server** v levém podokně hello. 
3. V nabídce hello vyberte **spravovat** > **přidat role a funkce**.
4. V hello přidat role a funkce Průvodce hello **typ instalace** vyberte **instalace na základě rolí nebo na základě funkcí**a potom klikněte na **Další**.
   
    ![Snímek obrazovky znázorňující hello Průvodce přidáním rolí a funkcí kartu pro typ instalace](./media/hero-role/role-wizard.png)
5. Vyberte hello virtuálních počítačů z fondu serverů hello a klikněte na **Další**.
6. Na hello **role serveru** vyberte **webového serveru (IIS)**.
   
    ![Snímek obrazovky znázorňující hello Průvodce přidáním rolí a funkcí kartu pro role serveru](./media/hero-role/add-iis.png)
7. V hello automaticky otevírané okno o přidávání funkcí, které jsou potřebné pro službu IIS, ujistěte se, že **zahrnout nástroje pro správu** je vybrána a potom klikněte na **přidat funkce**. Po hello automaticky otevírané okno se zavře a klikněte na tlačítko **Další** v Průvodci hello.
   
    ![Snímek obrazovky ukazující, automaticky otevírané okno tooconfirm přidání hello IIS role](./media/hero-role/confirm-add-feature.png)
8. Na stránce funkce hello, klikněte na **Další**.
9. Na hello **Role webového serveru (IIS)** klikněte na tlačítko **Další**. 
10. Na hello **služby rolí** klikněte na tlačítko **Další**. 
11. Na hello **potvrzení** klikněte na tlačítko **nainstalovat**. 
12. Po dokončení instalace hello klikněte na tlačítko **Zavřít** v Průvodci hello.

## <a name="open-port-80"></a>Otevření portu 80
V pořadí pro váš počítač tooaccept příchozí provoz přes port 80, je třeba tooadd skupinu zabezpečení sítě toohello příchozí pravidlo. 

1. Otevřete hello [portál Azure](https://portal.azure.com).
2. V **virtuální počítače** hello vyberte virtuální počítač, který jste vytvořili.
3. V nastavení hello virtuální počítače, vyberte **síťových rozhraní** a pak vyberte hello existujícího síťového rozhraní.
   
    ![Snímek obrazovky zobrazující hello nastavení virtuálního počítače pro hello síťová rozhraní](./media/hero-role/network-interface.png)
4. V **Essentials** hello síťového rozhraní, klikněte na hello **skupinu zabezpečení sítě**.
   
    ![Snímek obrazovky zobrazující hello Essentials části pro síťové rozhraní hello](./media/hero-role/select-nsg.png)
5. V hello **Essentials** okně hello NSG, měli byste mít jeden existující výchozí pravidlo pro příchozí **výchozí povolit rdp** který vám umožní toolog v toohello virtuálních počítačů. Přidejte jiný příchozí pravidlo tooallow IIS provoz. Klikněte na **Příchozí pravidla zabezpečení**.
   
    ![Snímek obrazovky zobrazující hello Essentials části hello NSG](./media/hero-role/inbound.png)
6. V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**.
   
    ![Snímek obrazovky zobrazující hello tlačítko tooadd pravidlo zabezpečení](./media/hero-role/add-rule.png)
7. V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**. Typ **80** v hello rozsah portů a zajistěte, aby **povolit** je vybrána. Až to budete mít, klikněte na **OK**.
   
    ![Snímek obrazovky zobrazující hello tlačítko tooadd pravidlo zabezpečení](./media/hero-role/port-80.png)

Další informace o skupinách Nsg, příchozí a odchozí pravidla, najdete v části [povolit externí přístup tooyour virtuální počítač pomocí hello portálu Azure](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="connect-toohello-default-iis-website"></a>Připojit toohello výchozí web služby IIS
1. V hello portálu Azure, klikněte na **virtuální počítače** a potom vyberte virtuální počítač.
2. V hello **Essentials** zkopírujte vaše **veřejnou IP adresu**.
   
    ![Snímek obrazovky ukazující, kde toofind hello veřejnou IP adresu vašeho virtuálního počítače](./media/hero-role/ipaddress.png)
3. Otevřete prohlížeč a v panelu Adresa hello, zadejte svoji veřejnou IP adresu takto: http://<publicIPaddress> a klikněte na tlačítko **Enter** toogo toothat adresu.
4. Váš prohlížeč by měla otevřít hello výchozí IIS webové stránky. Vypadá přibližně takto:
   
    ![Snímek obrazovky se stránkou jaké hello výchozí IIS vypadá v prohlížeči](./media/hero-role/iis-default.png)

## <a name="next-steps"></a>Další kroky
* Také můžete experimentovat s [připojením datového disku](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtuálního počítače. Pomocí datových disků si můžete rozšířit úložiště pro virtuální počítač.

