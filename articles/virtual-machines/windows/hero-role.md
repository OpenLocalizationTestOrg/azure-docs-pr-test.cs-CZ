---
title: "Instalace služby IIS na vaše první virtuální počítač s Windows | Microsoft Docs"
description: "Experimentujte s prvním virtuálním počítači Windows pomocí instalace služby IIS a otevírání portu 80 pomocí portálu Azure."
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
ms.openlocfilehash: b11ce1eab0c26a802c31bc418cdf725cbc4fba30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Experimentujte s instalaci role na vašem virtuálním počítači Windows
Až budete mít první virtuální počítač (VM) fungovaly, můžete přesunout instalaci softwaru a služeb. V tomto kurzu přidáme nainstalovat službu IIS pomocí Správce serverů na virtuální počítač Windows serveru. Poté vytvoříme skupinu zabezpečení sítě (NSG) pomocí portálu Azure otevřete port 80 k provozu služby IIS. 

Pokud jste ještě nevytvořili vaše první virtuální počítač, by měl přejdete zpět do [vytvořit svůj první virtuální počítač Windows v portálu Azure](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) před pokračováním v tomto kurzu.

## <a name="make-sure-the-vm-is-running"></a>Ujistěte se, že je virtuální počítač spuštěný.
1. Otevřete web [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na **Virtual Machines**. Ze seznamu vyberte virtuální počítač.
3. Pokud je stav **zastaveno (Deallocated)**, klikněte na tlačítko **spustit** tlačítko **Essentials** okno virtuálního počítače. Pokud je stav **systémem**, můžete přesunout další krok.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Připojit k virtuálnímu počítači a přihlášení
1. V nabídce centra klikněte na **Virtual Machines**. Ze seznamu vyberte virtuální počítač.
2. V okně pro virtuální počítač klikněte na **Připojit**. Tím se vytvoří a stáhne soubor .rdp (Remote Desktop Protocol), který představuje zástupce připojení k vašemu počítači. Můžete si ho uložit na plochu, abyste k němu měli snadno přístup. **Otevřením** tohoto souboru se připojíte ke svému virtuálnímu počítači.
   
    ![Snímek obrazovky webu Azure Portal znázorňující způsob připojení k virtuálnímu počítači](./media/hero-role/connect.png)
3. Zobrazí se upozornění, že soubor .rdp je od neznámého vydavatele. To je normální. Pokračujte kliknutím na **Připojit** v okně Připojení ke vzdálené ploše.
   
    ![Snímek obrazovky s upozorněním na neznámého vydavatele](./media/hero-role/rdp-warn.png)
4. V okně zabezpečení systému Windows zadejte uživatelské jméno a heslo pro místní účet, který jste vytvořili při vytváření virtuálního počítače. Zadejte uživatelské jméno v této podobě: *název_virtuálního_počítače*&#92;*uživatelské_jméno* a pak klikněte na **OK**.
   
    ![Snímek obrazovky zadání názvu virtuálního počítače, uživatelské jméno a heslo](./media/hero-role/credentials.png)
5. Zobrazí se upozornění, že certifikát nelze ověřit. To je normální. Kliknutím na **Ano** ověřte identitu virtuálního počítače a dokončete přihlášení.
   
   ![Snímek obrazovky zobrazující zprávu o ověření identity virtuálního počítače](./media/hero-role/cert-warning.png)

Pokud budete mít s připojením problémy, projděte si téma [Poradce při potížích s připojením k virtuálnímu počítači s Windows v Azure pomocí Vzdálené plochy](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="install-iis-on-your-vm"></a>Instalace služby IIS na vašem virtuálním počítači
Teď jste přihlášení k virtuálnímu počítači a můžeme nainstalovat role serveru, abyste mohli více experimentovat.

1. Otevřete **Správce serveru**, pokud ještě není otevřený. Klikněte na tlačítko **spustit** nabídce a pak klikněte na tlačítko **správce serveru**.
2. V okně **Správce serveru** vyberte v levém podokně **Místní server**. 
3. V nabídce vyberte **Spravovat** > **Přidat role a funkce**.
4. Přidat role a funkce Průvodce na **typ instalace** vyberte **instalace na základě rolí nebo na základě funkcí**a potom klikněte na **Další**.
   
    ![Snímek obrazovky ukazující na kartě Průvodce přidáním rolí a funkcí pro typ instalace](./media/hero-role/role-wizard.png)
5. Vyberte virtuální počítač ve fondu serverů a klikněte na **Další**.
6. Na stránce **Role serveru** vyberte **Webový server (IIS)**.
   
    ![Snímek obrazovky ukazující na kartě Průvodce přidáním rolí a funkcí pro role serveru](./media/hero-role/add-iis.png)
7. V místním okně pro přidávání funkcí potřebných pro službu IIS ověřte, že je zaškrtnuté **Zahrnout nástroje pro správu** , a potom klikněte na **Přidat funkce**. Po zavření místního okna klikněte v průvodci na **Další**.
   
    ![Snímek obrazovky ukazující, automaticky otevírané okno pro potvrzení přidání role služby IIS](./media/hero-role/confirm-add-feature.png)
8. Na stránce funkcí klikněte na **Další**.
9. Na stránce **Web Server Role (IIS)** klikněte na **Další**. 
10. Na stránce **Služby rolí** klikněte na **Další**. 
11. Na stránce **Potvrzení** klikněte na **Instalovat**. 
12. Po dokončení instalace klikněte v průvodci na **Zavřít**.

## <a name="open-port-80"></a>Otevření portu 80
Aby váš virtuální počítač přijímal příchozí přenos přes port 80, musíte do skupiny zabezpečení sítě přidat příchozí pravidlo. 

1. Otevřete web [Azure Portal](https://portal.azure.com).
2. V **virtuální počítače** vyberte virtuální počítač, který jste vytvořili.
3. V nastavení virtuálního počítače, vyberte **síťových rozhraní** a pak vyberte do existujícího síťového rozhraní.
   
    ![Snímek obrazovky nastavení virtuálního počítače pro rozhraní sítě](./media/hero-role/network-interface.png)
4. V **Essentials** síťového rozhraní, klikněte na **skupinu zabezpečení sítě**.
   
    ![Snímek obrazovky zobrazující části základní údaje pro síťové rozhraní](./media/hero-role/select-nsg.png)
5. V okně **Základní údaje** pro NSG by mělo být jedno výchozí příchozí rule pro **default-allow-rdp**, které umožňuje přihlásit se k virtuálnímu počítači. Teď přidáte další pravidlo, které povolí provoz IIS. Klikněte na **Příchozí pravidla zabezpečení**.
   
    ![Snímek obrazovky zobrazující části základní údaje skupiny nsg](./media/hero-role/inbound.png)
6. V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**.
   
    ![Snímek obrazovky tlačítka Přidat pravidlo zabezpečení](./media/hero-role/add-rule.png)
7. V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**. Do pole pro rozsah portů zadejte **80** a zkontrolujte, že **Povolit** je zaškrtnuté. Až to budete mít, klikněte na **OK**.
   
    ![Snímek obrazovky tlačítka Přidat pravidlo zabezpečení](./media/hero-role/port-80.png)

Další informace o skupinách NSG a příchozích a odchozích pravidlech najdete v tématu [Povolení externího přístupu k virtuálnímu počítači pomocí webu Azure Portal](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="connect-to-the-default-iis-website"></a>Připojení k výchozímu webu IIS
1. Na webu Azure Portal klikněte na **Virtuální počítače** a vyberte svůj virtuální počítač.
2. V okně **Základní údaje** zkopírujte **veřejnou IP adresu**.
   
    ![Snímek obrazovky ukazující, kde najít veřejnou IP adresu vašeho virtuálního počítače](./media/hero-role/ipaddress.png)
3. Otevřete prohlížeč, do adresního řádku zadejte svou veřejnou IP adresu, která vypadá takto: http://<publicIPaddress>, a kliknutím na **Enter** přejděte na tuto adresu.
4. Váš prohlížeč by měla otevřít výchozí webové stránky služby IIS. Vypadá přibližně takto:
   
    ![Snímek obrazovky ukazující, jak výchozí stránka služby IIS vypadá v prohlížeči](./media/hero-role/iis-default.png)

## <a name="next-steps"></a>Další kroky
* Také můžete experimentovat s [připojením datového disku](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) k vašemu virtuálnímu počítači. Pomocí datových disků si můžete rozšířit úložiště pro virtuální počítač.

