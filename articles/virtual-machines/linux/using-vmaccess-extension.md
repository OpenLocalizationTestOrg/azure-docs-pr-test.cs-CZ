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
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-20"></a><span data-ttu-id="1f709-103">Spravovat uživatele, SSH a zkontrolujte nebo opravit disky na virtuální počítače s Linuxem pomocí rozšíření VMAccess hello s hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="1f709-103">Manage users, SSH, and check or repair disks on Linux VMs using hello VMAccess Extension with hello Azure CLI 2.0</span></span>
<span data-ttu-id="1f709-104">Hello disk ve virtuálním počítačům s Linuxem se zobrazuje chyby.</span><span class="sxs-lookup"><span data-stu-id="1f709-104">hello disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="1f709-105">Nějakým způsobem resetovat hello kořenové heslo pro virtuální počítač Linux nebo omylem odstraněné svůj privátní klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="1f709-105">You somehow reset hello root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="1f709-106">V takovém případě zpět ve dnech hello hello datacenter by třeba toodrive existuje a pak otevřete hello KVM tooget v konzole serveru hello.</span><span class="sxs-lookup"><span data-stu-id="1f709-106">If that happened back in hello days of hello datacenter, you would need toodrive there and then open hello KVM tooget at hello server console.</span></span> <span data-ttu-id="1f709-107">Hello rozšíření Azure VMAccess si můžete představit jako tento KVM přepínač, který umožňuje definovat můžete tooaccess hello konzoly tooreset přístup tooLinux nebo provést údržbu na úrovni disku.</span><span class="sxs-lookup"><span data-stu-id="1f709-107">Think of hello Azure VMAccess extension as that KVM switch that allows you tooaccess hello console tooreset access tooLinux or perform disk level maintenance.</span></span>

<span data-ttu-id="1f709-108">Tento článek ukazuje, jak toouse hello toocheck rozšíření VMAccess Azure nebo opravit disk, resetovat přístup uživatelů, Správa uživatelských účtů nebo obnovte konfiguraci SSH hello v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="1f709-108">This article shows you how toouse hello Azure VMAccess Extension toocheck or repair a disk, reset user access, manage user accounts, or reset hello SSH configuration on Linux.</span></span> <span data-ttu-id="1f709-109">Můžete také provést tyto kroky hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1f709-109">You can also perform these steps with hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="ways-toouse-hello-vmaccess-extension"></a><span data-ttu-id="1f709-110">Způsoby toouse hello rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="1f709-110">Ways toouse hello VMAccess Extension</span></span>
<span data-ttu-id="1f709-111">Existují dva způsoby, abyste používali hello rozšíření VMAccess na virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="1f709-111">There are two ways that you can use hello VMAccess Extension on your Linux VMs:</span></span>

* <span data-ttu-id="1f709-112">Použijte hello Azure CLI 2.0 a hello požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="1f709-112">Use hello Azure CLI 2.0 and hello required parameters.</span></span>
* <span data-ttu-id="1f709-113">[Nezpracovaném formátu JSON pomocí souborů této hello proces rozšíření VMAccess](#use-json-files-and-the-vmaccess-extension) a potom na.</span><span class="sxs-lookup"><span data-stu-id="1f709-113">[Use raw JSON files that hello VMAccess Extension process](#use-json-files-and-the-vmaccess-extension) and then act on.</span></span>

<span data-ttu-id="1f709-114">Následující příklady použití Hello [uživatele virtuálního počítače az](/cli/azure/vm/user) příkazy.</span><span class="sxs-lookup"><span data-stu-id="1f709-114">hello following examples use [az vm user](/cli/azure/vm/user) commands.</span></span> <span data-ttu-id="1f709-115">Tyto kroky tooperform, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="1f709-115">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="reset-ssh-key"></a><span data-ttu-id="1f709-116">Resetovat klíč SSH</span><span class="sxs-lookup"><span data-stu-id="1f709-116">Reset SSH key</span></span>
<span data-ttu-id="1f709-117">Hello následující příklad resetuje hello klíč SSH pro uživatele hello `azureuser` na hello virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="1f709-117">hello following example resets hello SSH key for hello user `azureuser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a><span data-ttu-id="1f709-118">Resetování hesla</span><span class="sxs-lookup"><span data-stu-id="1f709-118">Reset password</span></span>
<span data-ttu-id="1f709-119">Hello následující příklad resetuje hello heslo pro uživatele hello `azureuser` na hello virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="1f709-119">hello following example resets hello password for hello user `azureuser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a><span data-ttu-id="1f709-120">Restartujte SSH</span><span class="sxs-lookup"><span data-stu-id="1f709-120">Restart SSH</span></span>
<span data-ttu-id="1f709-121">Hello následující příklad restartuje démon procesu SSH hello a resetování hello hodnoty toodefault konfigurace SSH na virtuálním počítači s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="1f709-121">hello following example restarts hello SSH daemon and resets hello SSH configuration toodefault values on a VM named `myVM`:</span></span>

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a><span data-ttu-id="1f709-122">Vytvoření uživatele</span><span class="sxs-lookup"><span data-stu-id="1f709-122">Create a user</span></span>
<span data-ttu-id="1f709-123">Hello následující příklad vytvoří uživatele s názvem `myNewUser` pomocí klíče SSH pro ověřování na hello virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="1f709-123">hello following example creates a user named `myNewUser` using an SSH key for authentication on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a><span data-ttu-id="1f709-124">Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="1f709-124">Delete a user</span></span>
<span data-ttu-id="1f709-125">Hello následující příklad odstraní uživatele s názvem `myNewUser` na hello virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="1f709-125">hello following example deletes a user named `myNewUser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-hello-vmaccess-extension"></a><span data-ttu-id="1f709-126">Použijte soubory JSON a hello rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="1f709-126">Use JSON files and hello VMAccess Extension</span></span>
<span data-ttu-id="1f709-127">Následující příklady Hello použijte nezpracované soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="1f709-127">hello following examples use raw JSON files.</span></span> <span data-ttu-id="1f709-128">Použití [nastavení rozšíření virtuálního az](/cli/azure/vm/extension#set) toothen volání souborů JSON.</span><span class="sxs-lookup"><span data-stu-id="1f709-128">Use [az vm extension set](/cli/azure/vm/extension#set) toothen call your JSON files.</span></span> <span data-ttu-id="1f709-129">Tyto soubory JSON je možné také volat z šablony Azure.</span><span class="sxs-lookup"><span data-stu-id="1f709-129">These JSON files can also be called from Azure templates.</span></span> 

### <a name="reset-user-access"></a><span data-ttu-id="1f709-130">Obnovte uživatele přístup</span><span class="sxs-lookup"><span data-stu-id="1f709-130">Reset user access</span></span>
<span data-ttu-id="1f709-131">Pokud jste ztratili přístup tooroot na virtuálním počítačům s Linuxem, můžete spustit skript tooreset VMAccess klíč SSH uživatele nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="1f709-131">If you have lost access tooroot on your Linux VM, you can launch a VMAccess script tooreset a user's SSH key or password.</span></span>

<span data-ttu-id="1f709-132">veřejný klíč SSH tooreset hello uživatele, vytvořte soubor s názvem `reset_ssh_key.json` a přidat nastavení v hello formátu.</span><span class="sxs-lookup"><span data-stu-id="1f709-132">tooreset hello SSH public key of a user, create a file named `reset_ssh_key.json` and add settings in hello following format.</span></span> <span data-ttu-id="1f709-133">Nahraďte vlastními hodnotami pro hello `username` a `ssh_key` parametry:</span><span class="sxs-lookup"><span data-stu-id="1f709-133">Substitute your own values for hello `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

<span data-ttu-id="1f709-134">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="1f709-134">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

<span data-ttu-id="1f709-135">tooreset heslo uživatele, vytvořte soubor s názvem `reset_user_password.json` a přidat nastavení v hello formátu.</span><span class="sxs-lookup"><span data-stu-id="1f709-135">tooreset a user password, create a file named `reset_user_password.json` and add settings in hello following format.</span></span> <span data-ttu-id="1f709-136">Nahraďte vlastními hodnotami pro hello `username` a `password` parametry:</span><span class="sxs-lookup"><span data-stu-id="1f709-136">Substitute your own values for hello `username` and `password` parameters:</span></span>

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

<span data-ttu-id="1f709-137">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="1f709-137">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a><span data-ttu-id="1f709-138">Restartujte SSH</span><span class="sxs-lookup"><span data-stu-id="1f709-138">Restart SSH</span></span>
<span data-ttu-id="1f709-139">toorestart hello proces démon programu SSH a resetovat hodnoty toodefault konfigurace SSH hello, vytvořte soubor s názvem `reset_sshd.json`.</span><span class="sxs-lookup"><span data-stu-id="1f709-139">toorestart hello SSH daemon and reset hello SSH configuration toodefault values, create a file named `reset_sshd.json`.</span></span> <span data-ttu-id="1f709-140">Přidejte hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="1f709-140">Add hello following content:</span></span>

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="1f709-141">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="1f709-141">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a><span data-ttu-id="1f709-142">Správa uživatelů</span><span class="sxs-lookup"><span data-stu-id="1f709-142">Manage users</span></span>

<span data-ttu-id="1f709-143">toocreate uživatel, který používá klíč SSH pro ověřování, vytvořte soubor s názvem `create_new_user.json` a přidat nastavení v hello formátu.</span><span class="sxs-lookup"><span data-stu-id="1f709-143">toocreate a user that uses an SSH key for authentication, create a file named `create_new_user.json` and add settings in hello following format.</span></span> <span data-ttu-id="1f709-144">Nahraďte vlastními hodnotami pro hello `username` a `ssh_key` parametry:</span><span class="sxs-lookup"><span data-stu-id="1f709-144">Substitute your own values for hello `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="1f709-145">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="1f709-145">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

<span data-ttu-id="1f709-146">toodelete uživatele, vytvořte soubor s názvem `delete_user.json` a přidejte hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="1f709-146">toodelete a user, create a file named `delete_user.json` and add hello following content.</span></span> <span data-ttu-id="1f709-147">Nahraďte vlastní hodnoty hello `remove_user` parametr:</span><span class="sxs-lookup"><span data-stu-id="1f709-147">Substitute your own value for hello `remove_user` parameter:</span></span>

```json
{
  "remove_user":"myNewUser"
}
```

<span data-ttu-id="1f709-148">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="1f709-148">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-hello-disk"></a><span data-ttu-id="1f709-149">Zkontrolujte nebo opravte hello disku</span><span class="sxs-lookup"><span data-stu-id="1f709-149">Check or repair hello disk</span></span>
<span data-ttu-id="1f709-150">Použitím VMAccess můžete také zkontrolujte a opravte disk, zda jste přidali toohello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="1f709-150">Using VMAccess you can also check and repair a disk that you added toohello Linux VM.</span></span>

<span data-ttu-id="1f709-151">toocheck a potom opravit hello disk, vytvořte soubor s názvem `disk_check_repair.json` a přidat nastavení v hello formátu.</span><span class="sxs-lookup"><span data-stu-id="1f709-151">toocheck and then repair hello disk, create a file named `disk_check_repair.json` and add settings in hello following format.</span></span> <span data-ttu-id="1f709-152">Nahraďte vlastní hodnotu pro název hello `repair_disk`:</span><span class="sxs-lookup"><span data-stu-id="1f709-152">Substitute your own value for hello name of `repair_disk`:</span></span>

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

<span data-ttu-id="1f709-153">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="1f709-153">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a><span data-ttu-id="1f709-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1f709-154">Next steps</span></span>
<span data-ttu-id="1f709-155">Aktualizace Linux pomocí hello rozšíření VMAccess Azure je jedna metoda toomake změny na spuštěného virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="1f709-155">Updating Linux using hello Azure VMAccess Extension is one method toomake changes on a running Linux VM.</span></span> <span data-ttu-id="1f709-156">Můžete také použít nástroje, například cloudové init a toomodify šablony Azure Resource Manager virtuálním počítačům s Linuxem na spouštěcí.</span><span class="sxs-lookup"><span data-stu-id="1f709-156">You can also use tools like cloud-init and Azure Resource Manager templates toomodify your Linux VM on boot.</span></span>

[<span data-ttu-id="1f709-157">Rozšíření virtuálního počítače a funkce pro Linux</span><span class="sxs-lookup"><span data-stu-id="1f709-157">Virtual machine extensions and features for Linux</span></span>](extensions-features.md)

[<span data-ttu-id="1f709-158">Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="1f709-158">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="1f709-159">Během vytváření pomocí toocustomize init cloudu virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="1f709-159">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md)

