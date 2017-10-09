---
title: "připojení SSH aaaTroubleshoot problémy tooan virtuálního počítače Azure | Microsoft Docs"
description: "Jak tootroubleshoot problémy například \"Připojení SSH se nezdařilo\" nebo \"odmítl připojení SSH' pro virtuální počítač Azure s Linuxem."
keywords: "SSH připojení odmítnuto, ssh chyby, azure ssh, připojení SSH se nezdařilo"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: dfb4e75e571c8306edf5f300c4e0f07a5fe7750a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-ssh-connections-tooan-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Řešení potíží s tooan připojení SSH Linux virtuálního počítače Azure, který selže, chyby, nebo bylo odmítnuto
Existují různé příčiny, že dojde k chybám Secure Shell (SSH), selhání připojení SSH, nebo SSH bylo odmítnuto, když zkusíte tooconnect tooa Linuxový virtuální počítač (VM). Tento článek vám pomůže najít a správné hello problémy. Můžete použít hello portál Azure, rozhraní příkazového řádku Azure nebo rozšíření pro přístup virtuálních počítačů pro Linux tootroubleshoot a vyřešit potíže s připojením.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](http://azure.microsoft.com/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](http://azure.microsoft.com/support/options/) a vyberte **získat podporu**. Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](http://azure.microsoft.com/support/faq/).

## <a name="quick-troubleshooting-steps"></a>Rychlé řešení potíží
Po dokončení každého kroku řešení potíží pokuste o připojení toohello virtuálních počítačů.

1. Obnovte konfiguraci SSH hello.
2. Resetovat hello pověření pro uživatele hello.
3. Ověřte hello [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) pravidla povolit provoz protokolu SSH.
   * Zkontrolujte, zda pravidlo skupinu zabezpečení sítě existuje provoz toopermit SSH (ve výchozím nastavení je to TCP port 22).
   * Nemůžete použít přesměrování portu / mapování bez použití pro vyrovnávání zatížení Azure.
4. Zkontrolujte hello [stavu prostředků virtuálních počítačů](../../resource-health/resource-health-overview.md). 
   * Ujistěte se, že hello virtuálních počítačů sestavy, že je v pořádku.
   * Pokud máte povolené Diagnostika spouštění, ověřte, zda hello virtuálního počítače není reporting spouštěcí chyby v protokolech hello.
5. Restartujte hello virtuálních počítačů.
6. Znovu nasaďte hello virtuálních počítačů.

Pokračujte ve čtení pro podrobnější pro řešení potíží a vysvětlení.

## <a name="available-methods-tootroubleshoot-ssh-connection-issues"></a>Dostupné metody potíží s připojeními SSH tootroubleshoot
Můžete resetovat konfiguraci SSH pomocí jedné z následujících metod hello nebo přihlašovací údaje:

* [Portál Azure](#use-the-azure-portal) – výborné Pokud potřebujete tooquickly resetovat konfiguraci SSH hello nebo klíč SSH a nebudete mít hello nainstalovány nástroje pro Azure.
* [Azure CLI 2.0](#use-the-azure-cli-20) – Pokud jste už na příkazovém řádku hello, rychle resetování konfigurace SSH hello nebo přihlašovací údaje. Můžete taky hello [1.0 rozhraní příkazového řádku Azure](#use-the-azure-cli-10)
* [Azure rozšíření VMAccessForLinux](#use-the-vmaccess-extension) – vytvoření a opakovaně používat json definice soubory tooreset hello SSH konfigurace nebo přihlašovací údaje uživatele.

Po dokončení každého kroku řešení potíží zkuste se znovu připojit tooyour virtuálních počítačů. Pokud se pořád nemůžete připojit, zkuste hello další krok.

## <a name="use-hello-azure-portal"></a>Hello použití portálu Azure
Hello portál Azure poskytuje hello tooreset rychlým způsobem SSH konfigurace nebo přihlašovací údaje uživatele bez instalace žádné nástroje na místním počítači.

Vyberte virtuální počítač v hello portálu Azure. Projděte dolů toohello **podporu + Poradce při potížích s** a vyberte **resetovat heslo** jako hello následující ukázka:

![Resetování konfigurace SSH nebo přihlašovací údaje v hello portálu Azure](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-hello-ssh-configuration"></a>Obnovte konfiguraci SSH hello
Jako první krok, vyberte `Reset configuration only` z hello **režimu** rozevírací nabídky jako v předchozím snímku obrazovky hello klikněte hello **resetovat** tlačítko. Po dokončení této akce opakujte tooaccess virtuálního počítače.

### <a name="reset-ssh-credentials-for-a-user"></a>Resetovat SSH přihlašovací údaje pro uživatele
tooreset hello přihlašovací údaje stávajícího uživatele, vyberte buď `Reset SSH public key` nebo `Reset password` z hello **režimu** rozevírací nabídky jako hello předchozím snímku obrazovky. Zadejte uživatelské jméno hello a klíč SSH nebo nové heslo a pak klikněte na hello **resetovat** tlačítko.

Můžete také vytvořit uživatele s oprávněními sudo v hello virtuálního počítače z této nabídky. Zadejte nové uživatelské jméno a přiřazené heslo nebo klíč SSH a pak klikněte na tlačítko hello **resetovat** tlačítko.

## <a name="use-hello-azure-cli-20"></a>Hello použití Azure CLI 2.0
Pokud jste to ještě neudělali, nainstalujte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Pokud jste vytvořili a nahrát vlastní image disku Linux, ujistěte se, zda text hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verze 2.0.5 nebo novější je nainstalována. Pro virtuální počítače vytvořené pomocí Galerie obrázků toto rozšíření přístup k již instalovaných a konfigurace.

### <a name="reset-ssh-configuration"></a>Obnovte konfiguraci SSH
Můžete Nejdřív zkuste resetuje hello SSH toodefault hodnoty konfigurace a restartování serveru hello SSH na hello virtuálních počítačů. Všimněte si, že to nezmění hello název uživatelského účtu, hesla nebo klíče SSH.
Hello následující příklad používá [az virtuálního počítače uživatele resetování-ssh](/cli/azure/vm/user#reset-ssh) tooreset hello konfiguraci SSH na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>Resetovat SSH přihlašovací údaje pro uživatele
Hello následující příklad používá [aktualizace uživatele virtuálního počítače az](/cli/azure/vm/user#update) tooreset hello přihlašovací údaje pro `myUsername` toohello hodnota zadaná v `myPassword`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Pokud používáte ověření pomocí klíče SSH, můžete resetovat hello klíč SSH pro daného uživatele. Hello následující příklad používá **az virtuální počítač přístup k set-linux-user** klíč SSH hello tooupdate uložené v `~/.ssh/id_rsa.pub` pro hello uživatele s názvem `myUsername`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-hello-vmaccess-extension"></a>Použití rozšíření VMAccess hello
v souboru json, který definuje akce toocarry si přečte Hello rozšíření pro přístup virtuálních počítačů pro Linux. Mezi ně patří resetování SSHD, resetování klíče SSH nebo přidání uživatele. Dál používat hello rozhraní příkazového řádku Azure toocall hello rozšíření VMAccess, ale můžete opakovaně použít soubory json hello napříč více virtuálními počítači v případě potřeby. Tento přístup umožňuje toocreate úložiště json souborů, které může být volána pro daný scénáře.

### <a name="reset-sshd"></a>Resetování SSHD
Vytvořte soubor s názvem `settings.json` s hello následující obsah:

```json
{  
    "reset_ssh":"True"
}
```

Pomocí hello rozhraní příkazového řádku Azure, potom zavolejte hello `VMAccessForLinux` tooreset rozšíření připojení SSHD zadáním souboru json. Hello následující příklad používá [nastavení rozšíření virtuálního az](/cli/azure/vm/extension#set) tooreset SSHD na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Resetovat SSH přihlašovací údaje pro uživatele
Pokud se SSHD zobrazí toofunction správně, můžete resetovat hello pověření pro uživatele třetím osobám. tooreset hello heslo pro uživatele, vytvořte soubor s názvem `settings.json`. Hello následující příklad resetuje hello přihlašovací údaje pro `myUsername` toohello hodnota zadaná v `myPassword`. Zadejte hello následující řádky do vaší `settings.json` souboru pomocí vlastní hodnoty:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Nebo tooreset hello klíč SSH pro uživatele, nejprve vytvořte soubor s názvem `settings.json`. Hello následující příklad resetuje hello přihlašovací údaje pro `myUsername` toohello hodnota zadaná v `myPassword`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`. Zadejte hello následující řádky do vaší `settings.json` souboru pomocí vlastní hodnoty:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Po vytvoření souboru json, použít hello rozhraní příkazového řádku Azure toocall hello `VMAccessForLinux` rozšíření tooreset přihlašovací údaje uživatele SSH zadáním souboru json. Hello následující příklad resetuje přihlašovací údaje u hello virtuálního počítače s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-hello-azure-cli-10"></a>Hello použití Azure CLI 1.0
Pokud jste to ještě neudělali, [nainstalovat hello Azure CLI 1.0 a připojte tooyour předplatné](../../cli-install-nodejs.md). Ujistěte se, že režimu Resource Manager používáte následujícím způsobem:

```azurecli
azure config mode arm
```

Pokud jste vytvořili a nahrát vlastní image disku Linux, ujistěte se, zda text hello [Microsoft Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verze 2.0.5 nebo novější je nainstalována. Pro virtuální počítače vytvořené pomocí Galerie obrázků toto rozšíření přístup k již instalovaných a konfigurace.

### <a name="reset-ssh-configuration"></a>Obnovte konfiguraci SSH
může být nesprávné konfigurace SSHD Hello, sám sebe nebo hello služby došlo k chybě. Můžete resetovat SSHD toomake se, že je platný hello konfiguraci SSH, sám sebe. Resetování SSHD by měl být hello první krok odstraňování problémů, které můžete provést.

Hello následující příklad resetuje SSHD na virtuálním počítači s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`. Používejte vlastní názvy virtuálních počítačů a prostředků skupiny takto:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Resetovat SSH přihlašovací údaje pro uživatele
Pokud se SSHD zobrazí toofunction správně, můžete resetovat hello heslo pro uživatele třetím osobám. Hello následující příklad resetuje hello přihlašovací údaje pro `myUsername` toohello hodnota zadaná v `myPassword`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

Pokud používáte ověření pomocí klíče SSH, můžete resetovat hello klíč SSH pro daného uživatele. Následující příklad aktualizace Hello hello klíč SSH, které jsou uložené v `~/.ssh/id_rsa.pub` pro hello uživatele s názvem `myUsername`, na hello virtuálního počítače s názvem `myVM` v `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a>Restartování virtuálního počítače
Pokud máte resetovat hello SSH konfigurace a uživatelské přihlašovací údaje, nebo došlo k chybě při tom, můžete zkusit restartovat tooaddress hello virtuálního počítače základní výpočetní problémy.

### <a name="azure-portal"></a>portál Azure
virtuální počítač pomocí toorestart hello portálu Azure vyberte hello váš počítač a klikněte na tlačítko **restartujte** tlačítko jako hello následující ukázka:

![Restartování virtuálního počítače v hello portálu Azure](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
Následující příklad restartování Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Hello následující příklad používá [restartování virtuálního počítače az](/cli/azure/vm#restart) toorestart hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Znovunasazení virtuálního počítače
Můžete znovu nasadit uzlu tooanother virtuálních počítačů v rámci Azure, která může vyřešit problémy s základní sítě. Informace o opětovného nasazení virtuálního počítače najdete v tématu [znovu nasadit virtuální počítač toonew Azure uzlu](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Po dokončení této operace dočasné disku data budou ztracena a dynamické IP adresy, které jsou přidruženy hello virtuálního počítače budou aktualizovány.
> 
> 

### <a name="azure-portal"></a>portál Azure
virtuální počítač pomocí tooredeploy hello portálu Azure vyberte virtuální počítač a přejděte dolů toohello **podporu + Poradce při potížích s** části. Klikněte na tlačítko hello **znovu nasaďte** tlačítko jako hello následující ukázka:

![Opětovné nasazení virtuálního počítače v hello portálu Azure](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli-10"></a>Azure CLI 1.0
Následující příklad opětovně nasadí Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Následující příklad použití Hello [az virtuálního počítače znovu ho zaveďte](/cli/azure/vm#redeploy) tooredeploy hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`. Použijte vlastní hodnoty takto:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-hello-classic-deployment-model"></a>Virtuální počítače vytvořené pomocí modelu nasazení Classic hello
Opakujte tyto kroky tooresolve hello nejběžnější SSH selhání připojení pro virtuální počítače, které byly vytvořeny pomocí modelu nasazení classic hello. Po dokončení každého kroku pokuste o připojení toohello virtuálních počítačů.

* Obnovte vzdálený přístup z hello [portál Azure](https://portal.azure.com). Na hello portálu Azure, vyberte virtuální počítač a klikněte na tlačítko hello **resetovat vzdálený...**  tlačítko.
* Restartujte hello virtuálních počítačů. Na hello [portál Azure](https://portal.azure.com), vyberte virtuální počítač a klikněte na tlačítko hello **restartujte** tlačítko.
    
* Znovu nasaďte hello virtuálních počítačů tooa nového Azure uzlu. Informace o tom najdete v části tooredeploy virtuální počítač, [znovu nasadit virtuální počítač toonew Azure uzlu](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
    Po dokončení této operace dočasné disku data budou ztracena a dynamické IP adresy, které jsou přidruženy hello virtuálního počítače budou aktualizovány.
* Postupujte podle pokynů hello v [jak tooreset heslo nebo SSH pro virtuální počítače se systémem Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) na:
  
  * Resetovat hello heslo nebo klíč SSH.
  * Vytvoření *sudo* uživatelský účet.
  * Obnovte konfiguraci SSH hello.
* Zkontrolujte stav prostředku hello Virtuálního počítače pro všechny platformy problémy.<br>
     Vyberte virtuální počítač a přejděte dolů **nastavení** > **Kontrola stavu**.

## <a name="additional-resources"></a>Další zdroje
* Pokud jste stále se nedaří tooSSH tooyour virtuálních počítačů po následující hello po provedení kroků, najdete v části [podrobnější pokyny k odstraňování potíží](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooreview další kroky tooresolve problém.
* Další informace o odstraňování potíží s přístup k aplikaci najdete v tématu [Poradce při potížích přístup tooan aplikace běžící na virtuálním počítači Azure](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Další informace o odstraňování potíží s virtuálních počítačů, které byly vytvořeny pomocí modelu nasazení classic hello najdete v tématu [jak tooreset heslo nebo SSH pro virtuální počítače se systémem Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

