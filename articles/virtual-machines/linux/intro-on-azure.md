---
title: aaaIntroduction tooLinux v Azure | Microsoft Docs
description: "Další informace o použití virtuální počítače s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: python
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: b13bf305-87bf-4df3-815e-e8f6337aa6ea
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: szark
ms.openlocfilehash: 3a931447ee23ce7000174ca314c3e10abc6b8e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toolinux-on-azure"></a>Úvod tooLinux v Azure
Toto téma obsahuje přehled některých aspektů pomocí virtuální počítače s Linuxem v hello cloudu Azure. Virtuální počítač s Linuxem nasazení je jednoduchý proces pomocí bitovou kopii z Galerie hello.

## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Ověřování: Uživatelská jména, hesla a klíče SSH
Při vytváření virtuální počítač s Linuxem pomocí hello portál Azure, se zobrazí výzva tooprovide buď uživatelské jméno a heslo nebo veřejný klíč SSH. Hello volba uživatelského jména pro nasazení virtuální počítač s Linuxem v Azure je subjektu toohello následující omezení: názvy účtů systému (UID < 100) již existuje v hello virtuálního počítače nejsou povoleny, "kořenový" třeba.

* V tématu [vytvoření virtuálního počítače se systémem Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* V tématu [jak tooUse SSH s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="obtaining-superuser-privileges-using-sudo"></a>Získání oprávnění superuživatele pomocí`sudo`
Hello uživatelský účet, který je zadán během nasazení instance virtuálního počítače na platformě Azure je privilegovaný účet. Tento účet je nakonfiguroval hello Azure Linux Agent toobe možné tooelevate oprávnění tooroot (superuživatel účtu) pomocí hello `sudo` nástroj. Po přihlášení pomocí tohoto uživatelského účtu, bude možné toorun příkazy jako kořenová pomocí syntaxe příkazu hello:

    # sudo <COMMAND>

Volitelně můžete získat pomocí prostředí kořenové **sudo -s**.

* V tématu [pomocí oprávnění kořenového na virtuální počítače s Linuxem v Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="firewall-configuration"></a>Konfigurace brány firewall
Azure poskytuje filtr příchozího paketu, který omezuje tooports připojení zadaný v hello portálu Azure. Ve výchozím nastavení, hello pouze povolené port je SSH. Může dojít k otevření až přístupové porty tooadditional na virtuální počítač Linux nakonfigurováním koncové body v hello portálu Azure:

* Přejděte na téma: [jak tooSet až koncové body tooa virtuálního počítače](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Hello Linux obrázků v Azure Gallery hello nepovolujte hello *iptables* brány firewall ve výchozím nastavení. V případě potřeby lze nakonfigurovat bránu firewall hello tooprovide další filtrování.

## <a name="hostname-changes"></a>Název hostitele změny
Když nasadíte původně instanci bitovou kopii systému Linux, jste požadované tooprovide název hostitele pro virtuální počítač hello. Po hello virtuální počítač spuštěn, je tento název hostitele serverů DNS publikované toohello platformy, tak, aby více virtuálních počítačů připojený tooeach jiné můžete provádět vyhledávání IP adresy pomocí názvy hostitelů.

Pokud název hostitele změny se požaduje po nasazení virtuálního počítače, použijte příkaz hello

    # sudo hostname <newname>

Hello Azure Linux Agent zahrnuje funkci tooautomatically, zjištění této změny názvu a odpovídajícím způsobem konfigurace toopersist hello virtuální počítač tato změna a publikování serverů DNS platformy toohello tuto změnu.

* [Uživatelská příručka k Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a>Init cloudu
**Ubuntu** a **CoreOS** bitové kopie využívat cloudové init na Azure, které poskytuje další funkce pro zavádění virtuálního počítače.

* [Jak tooInject vlastní Data](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Vlastní Data a inicializací cloudu na platformě Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [Vytvoření Azure Swap oddíly používající Init cloudu](https://wiki.ubuntu.com/AzureSwapPartitions)
* [Jak tooUse CoreOS v Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a>Zachycení Image virtuálního počítače
Azure poskytuje možnost hello toocapture hello stav existující virtuální počítač do bitové kopie, kterou lze následně instance používané toodeploy dalších virtuálních počítačů. Hello Azure Linux Agent může být použité toorollback některé hello přizpůsobení, která byla provedena během procesu zřizování hello. Hello kroků toocapture virtuální počítač může postupujte jako obrázek:

1. Spustit **příkaz waagent-deprovision** tooundo zřizování přizpůsobení. Nebo **příkaz waagent-deprovision + uživatele** toooptionally odstranit hello uživatelský účet zadaný během zřizování a všechna přidružená data.
2. Vypnout nižší nebo vypnutí hello virtuálního počítače.
3. Klikněte na tlačítko **zaznamenat** v hello Azure hello portálu nebo pomocí prostředí PowerShell nebo rozhraní příkazového řádku nástroje toocapture hello virtuálního počítače jako obrázek.
   
   * Přejděte na téma: [jak tooCapture tooUse Linuxový virtuální počítač jako šablonu](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="attaching-disks"></a>Připojení disků
Každý virtuální počítač má dočasného, místní *prostředků disku* připojen. Protože data na prostředku disku nesmí být trvanlivý po restartování počítače, se často používá aplikace a procesů spuštěných ve virtuálním počítači hello přechodná a **dočasné** úložiště dat. Je také hello použité toostore stránky nebo swap soubory hello operačního systému.

V systému Linux, je obvykle spravuje hello Azure Linux Agent a automaticky připojit příliš hello prostředků disku**/mnt nebo prostředků** (nebo **/mnt** Ubuntu Image).

> [!NOTE]
> Všimněte si, je tento disk hello prostředků **dočasné** na disku a může se odstraní a naformátována při hello virtuálních počítačů po restartu.
> 
> 

V systému Linux hello datový disk může být s názvem podle hello jádra jako `/dev/sdc`, a uživatelé budou potřebovat toopartition, formátování a připojte tento prostředek. To je zahrnutý v kurzu hello krok za krokem: [jak tooAttach tooa datový Disk virtuálního počítače](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

* **Viz také:** [konfigurace softwaru diskového pole RAID v systému Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [konfigurace LVM na virtuální počítač s Linuxem v Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

