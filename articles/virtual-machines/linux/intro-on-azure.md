---
title: "Úvod do systému Linux v Azure | Microsoft Docs"
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
ms.openlocfilehash: 7bd0c5549a2e1f592681760d5ef464b9570ca4ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-linux-on-azure"></a>Úvod do Linuxu na Azure
Toto téma obsahuje přehled některých aspektů pomocí virtuální počítače s Linuxem v cloudu Azure. Virtuální počítač s Linuxem nasazení je jednoduchý proces pomocí bitovou kopii z galerie.

## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Ověřování: Uživatelská jména, hesla a klíče SSH
Když vytváříte virtuální počítač s Linuxem pomocí portálu Azure, budete vyzváni k zadání buď uživatelské jméno a heslo nebo veřejný klíč SSH. Volba uživatelské jméno pro nasazení virtuální počítač s Linuxem v Azure se vztahují následující omezení: názvy účtů systému (UID < 100), který už ve virtuálním počítači nejsou povoleny, root, třeba.

* V tématu [vytvoření virtuálního počítače se systémem Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* V tématu [postup používání SSH s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="obtaining-superuser-privileges-using-sudo"></a>Získání oprávnění superuživatele pomocí`sudo`
Uživatelský účet, který je zadán během nasazení instance virtuálních počítačů v Azure je privilegovaný účet. Tento účet je nakonfigurovat pomocí Azure Linux Agent moct zvýšení oprávnění pomocí root (superuživatel účet) `sudo` nástroj. Po přihlášení pomocí tohoto uživatelského účtu, budete moci spustit příkazy jako kořenová pomocí syntaxe příkazu:

    # sudo <COMMAND>

Volitelně můžete získat pomocí prostředí kořenové **sudo -s**.

* V tématu [pomocí oprávnění kořenového na virtuální počítače s Linuxem v Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="firewall-configuration"></a>Konfigurace brány firewall
Azure poskytuje filtr příchozího paketu, který omezuje připojení k portům zadaný na portálu Azure. Ve výchozím nastavení je jenom povolených port SSH. Přístup k další porty na virtuální počítač Linux vám může otevře nakonfigurováním koncových bodů na portálu Azure:

* Přejděte na téma: [jak nastavit koncové body k virtuálnímu počítači](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Nepovolujte Linux obrázků v galerii Azure *iptables* brány firewall ve výchozím nastavení. V případě potřeby lze nakonfigurovat bránu firewall k poskytnutí další filtrování.

## <a name="hostname-changes"></a>Název hostitele změny
Když nasadíte původně instanci bitovou kopii systému Linux, musíte zadat název hostitele pro virtuální počítač. Jakmile je virtuální počítač spuštěn, tento název hostitele je publikována na servery DNS platformy, aby více virtuálních počítačů, které jsou vzájemně propojené můžete provádět vyhledávání IP adresy pomocí názvy hostitelů.

Pokud název hostitele změny se požaduje po nasazení virtuálního počítače, použijte příkaz

    # sudo hostname <newname>

Azure Linux Agent zahrnuje funkci automaticky zjišťovat tato změna názvu a odpovídajícím způsobem konfigurace virtuálního počítače zachovat tuto změnu a publikovat tuto změnu na servery DNS platformy.

* [Uživatelská příručka k Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a>Init cloudu
**Ubuntu** a **CoreOS** bitové kopie využívat cloudové init na Azure, které poskytuje další funkce pro zavádění virtuálního počítače.

* [Postup vložení vlastních dat](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Vlastní Data a inicializací cloudu na platformě Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [Vytvoření Azure Swap oddíly používající Init cloudu](https://wiki.ubuntu.com/AzureSwapPartitions)
* [Jak používat CoreOS v Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a>Zachycení Image virtuálního počítače
Azure umožňuje zaznamenat stav z existujícího virtuálního počítače do bitové kopie, které lze následně použít k nasazení virtuálního počítače další instance. Může být Azure Linux Agent používá k vrácení zpět některé přizpůsobení, která byla provedena během procesu zřizování. Může postupujte podle následujících kroků a zachytit virtuální počítač jako obrázek:

1. Spustit **příkaz waagent-deprovision** zrušit zřizování přizpůsobení. Nebo **příkaz waagent-deprovision + uživatele** volitelně odstranit uživatelský účet zadaný během zřizování a všechna přidružená data.
2. Vypnout nižší nebo vypnutí virtuálního počítače.
3. Klikněte na tlačítko **zaznamenat** v portálu Azure nebo pomocí prostředí PowerShell nebo rozhraní příkazového řádku nástroje zachytit virtuální počítač jako obrázek.
   
   * Přejděte na téma: [jak zachytit Linuxový virtuální počítač pro použití jako šablonu](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="attaching-disks"></a>Připojení disků
Každý virtuální počítač má dočasného, místní *prostředků disku* připojen. Protože data na prostředku disku nesmí být trvanlivý po restartování počítače, se často používá aplikace a procesů spuštěných ve virtuálním počítači pro přechodná a **dočasné** úložiště dat. Slouží také k ukládání stránky nebo Prohodit soubory operačního systému.

V systému Linux, je prostředek disku obvykle spravuje Azure Linux Agent a automaticky připojit k **/mnt nebo prostředků** (nebo **/mnt** Ubuntu Image).

> [!NOTE]
> Všimněte si, že je disk prostředků **dočasné** na disku a může se odstraní a naformátována, když je virtuální počítač restartovat.
> 
> 

V systému Linux datový disk může být s názvem jádrem jako `/dev/sdc`, a uživatelé budou potřebovat k oddílu, formátování a připojte tento prostředek. To je popsané v tomto kurzu krok za krokem: [jak připojit datový Disk k virtuálnímu počítači](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

* **Viz také:** [konfigurace softwaru diskového pole RAID v systému Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [konfigurace LVM na virtuální počítač s Linuxem v Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

