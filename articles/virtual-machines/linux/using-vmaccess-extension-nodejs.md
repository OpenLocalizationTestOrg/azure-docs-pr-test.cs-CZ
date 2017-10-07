---
title: "hello aaaReset přístup na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess | Microsoft Docs"
description: "Obnovte přístup na virtuálních počítačích Azure Linux pomocí hello rozšíření VMAccess."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: 2636655f3f7d14ba30e1dc62c319e4e278521ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-10"></a>Spravovat uživatele, SSH a zkontrolujte nebo opravit disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess hello s hello Azure CLI 1.0
Tento článek ukazuje, jak toouse hello rozšíření Azure VMAcesss toocheck opravit disk, obnovte přístup uživatele, Správa uživatelských účtů nebo resetujte konfiguraci SSHD hello v systému Linux. článek Hello vyžaduje:

* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))
* Hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) přihlášení `azure login`.
* Hello rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager `azure config mode arm`.


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands)– naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="quick-commands"></a>Rychlé příkazy
Existují dva způsoby toouse VMAccess na virtuální počítače Linux:

* Pomocí Azure CLI 1.0 hello a hello požadované parametry.
* Pomocí nezpracované soubory JSON, které VMAccess procesy a potom na.

Pro oddíl rychlý příkaz hello, přidáme toouse hello Azure CLI 1.0 `azure vm reset-access` metoda. Následující příklady příkazů v hello místo hello hodnoty, které obsahují "příkladu" hello hodnotami ze svého vlastního prostředí.

## <a name="create-a-resource-group-and-linux-vm"></a>Vytvoření skupiny prostředků a virtuální počítač s Linuxem
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Vytvoření Debian virtuálního počítače
```azurecli
azure vm quick-create \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -g myResourceGroup \
  -l westus \
  -y Linux \
  -n myVM \
  -Q Debian
```

## <a name="reset-root-password"></a>Resetování hesla kořenového
tooreset hello kořenové heslo:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a>Obnovení klíče SSH
klíč SSH tooreset hello nekořenovými uživatele:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Vytvoření uživatele
toocreate uživatele:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a>Odebrání uživatele
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a>Resetování SSHD
tooreset hello SSHD konfigurace:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a>Podrobný postup
### <a name="vmaccess-defined"></a>VMAccess definované:
Hello disk ve virtuálním počítačům s Linuxem se zobrazuje chyby. Nějakým způsobem resetovat hello kořenové heslo pro virtuální počítač Linux nebo omylem odstraněné svůj privátní klíč SSH. V takovém případě zpět ve dnech hello hello datacenter by třeba toodrive existuje a pak otevřete hello KVM tooget v konzole serveru hello. Hello rozšíření Azure VMAccess si můžete představit jako tento KVM přepínač, který umožňuje definovat můžete tooaccess hello konzoly tooreset přístup tooLinux nebo provést údržbu na úrovni disku.

Pro hello podrobný návod, přidáme toouse hello dlouhých úseků VMAccess, který používá nezpracované soubory JSON.  Tyto soubory VMAccess JSON je možné také volat z šablony Azure.

### <a name="using-vmaccess-toocheck-or-repair-hello-disk-of-a-linux-vm"></a>Použitím VMAccess toocheck nebo oprava hello disku virtuálního počítače s Linuxem
Pomocí VMAccess můžete provést fsck spustit na hello disku pod virtuálním počítačům s Linuxem.  Můžete také provést kontrolu disku a disku opravit pomocí VMAccess.

Tento skript VMAccess použít toocheck a potom disk hello opravy:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Spusťte skript VMAccess hello se:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-tooreset-user-access-toolinux"></a>Použitím VMAccess tooreset uživatel přístup tooLinux
Pokud jste ztratili přístup tooroot na virtuálním počítačům s Linuxem, můžete spustit skript VMAccess tooreset hello kořenové heslo.

tooreset hello kořenové heslo, použijte tento skript VMAccess:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

Spusťte skript VMAccess hello se:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

klíč SSH hello tooreset nekořenovými uživatele, použijte tento skript VMAccess:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

Spusťte skript VMAccess hello se:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-toomanage-user-accounts-on-linux"></a>Použitím VMAccess toomanage uživatelské účty v systému Linux
VMAccess je skript jazyka Python, který lze použít toomanage uživatele na virtuálním počítačům s Linuxem bez přihlášení a pomocí příkazu "sudo" nebo hello kořenového účtu.

toocreate uživatele, použijte tento skript VMAccess:

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Spusťte skript VMAccess hello se:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

toodelete uživatele, použijte tento skript VMAccess:

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

Spusťte skript VMAccess hello se:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-tooreset-hello-sshd-configuration"></a>Pomocí VMAccess tooreset hello SSHD konfigurace
Pokud provedete konfiguraci SSHD virtuální počítače Linux toohello změny a zavřít hello připojení SSH před ověřováním hello změny, může být zakázaný z SSH'ing zpět v.  VMAccess lze použít tooreset hello SSHD konfigurace back tooa známé platné konfigurace bez právě přihlášen prostřednictvím SSH.

tooreset hello SSHD konfiguraci můžete použít tento skript VMAccess:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Spusťte skript VMAccess hello se:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Další kroky
Aktualizace Linux používá rozšíření VMAccess Azure je jedna metoda toomake změny na spuštěného virtuálního počítače s Linuxem.  Můžete také použít nástroje, například cloudové init a šablon Azure toomodify virtuálním počítačům s Linuxem na spouštěcí.

[O rozšíření virtuálního počítače a funkce](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Během vytváření pomocí toocustomize init cloudu virtuálního počítače s Linuxem](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

