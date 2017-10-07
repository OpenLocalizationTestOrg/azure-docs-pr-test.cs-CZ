---
title: "aaaReset přístup tooan virtuální počítač Azure s Linuxem | Microsoft Docs"
description: "Jak uživatelé toomanage a resetování přístup na virtuální počítače s Linuxem pomocí rozšíření VMAccess hello a hello 2.0 rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/04/2017
ms.author: danlep
ms.openlocfilehash: 2f8db01b9fac20bf547d8b1926e5c0b3c5d18280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-20"></a>Spravovat uživatele, SSH a zkontrolujte nebo opravit disky na virtuální počítače s Linuxem pomocí rozšíření VMAccess hello s hello 2.0 rozhraní příkazového řádku Azure
Hello disk ve virtuálním počítačům s Linuxem se zobrazuje chyby. Nějakým způsobem resetovat hello kořenové heslo pro virtuální počítač Linux nebo omylem odstraněné svůj privátní klíč SSH. V takovém případě zpět ve dnech hello hello datacenter by třeba toodrive existuje a pak otevřete hello KVM tooget v konzole serveru hello. Hello rozšíření Azure VMAccess si můžete představit jako tento KVM přepínač, který umožňuje definovat můžete tooaccess hello konzoly tooreset přístup tooLinux nebo provést údržbu na úrovni disku.

Tento článek ukazuje, jak toouse hello toocheck rozšíření VMAccess Azure nebo opravit disk, resetovat přístup uživatelů, Správa uživatelských účtů nebo obnovte konfiguraci SSH hello v systému Linux. Můžete také provést tyto kroky hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="ways-toouse-hello-vmaccess-extension"></a>Způsoby toouse hello rozšíření VMAccess
Existují dva způsoby, abyste používali hello rozšíření VMAccess na virtuální počítače Linux:

* Použijte hello Azure CLI 2.0 a hello požadované parametry.
* [Nezpracovaném formátu JSON pomocí souborů této hello proces rozšíření VMAccess](#use-json-files-and-the-vmaccess-extension) a potom na.

Následující příklady použití Hello [uživatele virtuálního počítače az](/cli/azure/vm/user) příkazy. Tyto kroky tooperform, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

## <a name="reset-ssh-key"></a>Resetovat klíč SSH
Hello následující příklad resetuje hello klíč SSH pro uživatele hello `azureuser` na hello virtuálního počítače s názvem `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a>Resetování hesla
Hello následující příklad resetuje hello heslo pro uživatele hello `azureuser` na hello virtuálního počítače s názvem `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>Restartujte SSH
Hello následující příklad restartuje démon procesu SSH hello a resetování hello hodnoty toodefault konfigurace SSH na virtuálním počítači s názvem `myVM`:

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a>Vytvoření uživatele
Hello následující příklad vytvoří uživatele s názvem `myNewUser` pomocí klíče SSH pro ověřování na hello virtuálního počítače s názvem `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a>Odstranit uživatele
Hello následující příklad odstraní uživatele s názvem `myNewUser` na hello virtuálního počítače s názvem `myVM`:

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-hello-vmaccess-extension"></a>Použijte soubory JSON a hello rozšíření VMAccess
Následující příklady Hello použijte nezpracované soubory JSON. Použití [nastavení rozšíření virtuálního az](/cli/azure/vm/extension#set) toothen volání souborů JSON. Tyto soubory JSON je možné také volat z šablony Azure. 

### <a name="reset-user-access"></a>Obnovte uživatele přístup
Pokud jste ztratili přístup tooroot na virtuálním počítačům s Linuxem, můžete spustit skript tooreset VMAccess klíč SSH uživatele nebo heslo.

veřejný klíč SSH tooreset hello uživatele, vytvořte soubor s názvem `reset_ssh_key.json` a přidat nastavení v hello formátu. Nahraďte vlastními hodnotami pro hello `username` a `ssh_key` parametry:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

Spusťte skript VMAccess hello se:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

tooreset heslo uživatele, vytvořte soubor s názvem `reset_user_password.json` a přidat nastavení v hello formátu. Nahraďte vlastními hodnotami pro hello `username` a `password` parametry:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

Spusťte skript VMAccess hello se:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>Restartujte SSH
toorestart hello proces démon programu SSH a resetovat hodnoty toodefault konfigurace SSH hello, vytvořte soubor s názvem `reset_sshd.json`. Přidejte hello následující obsah:

```json
{
  "reset_ssh": true
}
```

Spusťte skript VMAccess hello se:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a>Správa uživatelů

toocreate uživatel, který používá klíč SSH pro ověřování, vytvořte soubor s názvem `create_new_user.json` a přidat nastavení v hello formátu. Nahraďte vlastními hodnotami pro hello `username` a `ssh_key` parametry:

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Spusťte skript VMAccess hello se:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

toodelete uživatele, vytvořte soubor s názvem `delete_user.json` a přidejte hello následující obsah. Nahraďte vlastní hodnoty hello `remove_user` parametr:

```json
{
  "remove_user":"myNewUser"
}
```

Spusťte skript VMAccess hello se:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-hello-disk"></a>Zkontrolujte nebo opravte hello disku
Použitím VMAccess můžete také zkontrolujte a opravte disk, zda jste přidali toohello virtuálního počítače s Linuxem.

toocheck a potom opravit hello disk, vytvořte soubor s názvem `disk_check_repair.json` a přidat nastavení v hello formátu. Nahraďte vlastní hodnotu pro název hello `repair_disk`:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

Spusťte skript VMAccess hello se:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a>Další kroky
Aktualizace Linux pomocí hello rozšíření VMAccess Azure je jedna metoda toomake změny na spuštěného virtuálního počítače s Linuxem. Můžete také použít nástroje, například cloudové init a toomodify šablony Azure Resource Manager virtuálním počítačům s Linuxem na spouštěcí.

[Rozšíření virtuálního počítače a funkce pro Linux](extensions-features.md)

[Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Během vytváření pomocí toocustomize init cloudu virtuálního počítače s Linuxem](using-cloud-init.md)

