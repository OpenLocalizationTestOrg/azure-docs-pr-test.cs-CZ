---
title: "Obnovte přístup na virtuální počítač Azure Linux | Microsoft Docs"
description: "Jak spravovat uživatele a obnovte přístup na virtuální počítače s Linuxem pomocí rozšíření VMAccess a 2.0 rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: 587c73278a9a92776276a811c5c4c8d3db773de3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-20"></a><span data-ttu-id="e3496-103">Spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuální počítače s Linuxem pomocí rozšíření VMAccess 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="e3496-103">Manage users, SSH, and check or repair disks on Linux VMs using the VMAccess Extension with the Azure CLI 2.0</span></span>
<span data-ttu-id="e3496-104">Disk ve virtuálním počítačům s Linuxem se zobrazuje chyby.</span><span class="sxs-lookup"><span data-stu-id="e3496-104">The disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="e3496-105">Nějakým způsobem resetování hesla kořenového virtuálním počítačům s Linuxem nebo omylem odstraněné svůj privátní klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="e3496-105">You somehow reset the root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="e3496-106">V takovém případě zpět v dny v datovém centru potřebovali byste existuje jednotky a pak otevřete KVM získat z konzoly serveru.</span><span class="sxs-lookup"><span data-stu-id="e3496-106">If that happened back in the days of the datacenter, you would need to drive there and then open the KVM to get at the server console.</span></span> <span data-ttu-id="e3496-107">Rozšíření Azure VMAccess si můžete představit jako že KVM přepínačů, která umožňuje přístup ke konzole resetovat přístup do systému Linux nebo provést údržbu na úrovni disku.</span><span class="sxs-lookup"><span data-stu-id="e3496-107">Think of the Azure VMAccess extension as that KVM switch that allows you to access the console to reset access to Linux or perform disk level maintenance.</span></span>

<span data-ttu-id="e3496-108">Tento článek ukazuje, jak používat Azure rozšíření VMAccess k kontrola opravit disk, obnovte přístup uživatele, Správa uživatelských účtů nebo obnovte konfiguraci SSH na Linux.</span><span class="sxs-lookup"><span data-stu-id="e3496-108">This article shows you how to use the Azure VMAccess Extension to check or repair a disk, reset user access, manage user accounts, or reset the SSH configuration on Linux.</span></span> <span data-ttu-id="e3496-109">K provedení těchto kroků můžete také využít [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e3496-109">You can also perform these steps with the [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="ways-to-use-the-vmaccess-extension"></a><span data-ttu-id="e3496-110">Způsoby použití rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="e3496-110">Ways to use the VMAccess Extension</span></span>
<span data-ttu-id="e3496-111">Existují dva způsoby, které můžete rozšíření VMAccess na virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="e3496-111">There are two ways that you can use the VMAccess Extension on your Linux VMs:</span></span>

* <span data-ttu-id="e3496-112">Použití Azure CLI 2.0 a požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="e3496-112">Use the Azure CLI 2.0 and the required parameters.</span></span>
* <span data-ttu-id="e3496-113">[Použít nezpracované soubory JSON, které zpracovávají rozšíření VMAccess](#use-json-files-and-the-vmaccess-extension) a potom na.</span><span class="sxs-lookup"><span data-stu-id="e3496-113">[Use raw JSON files that the VMAccess Extension process](#use-json-files-and-the-vmaccess-extension) and then act on.</span></span>

<span data-ttu-id="e3496-114">Následující příklady použití [uživatele virtuálního počítače az](/cli/azure/vm/user) příkazy.</span><span class="sxs-lookup"><span data-stu-id="e3496-114">The following examples use [az vm user](/cli/azure/vm/user) commands.</span></span> <span data-ttu-id="e3496-115">K provedení těchto kroků, budete potřebovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="e3496-115">To perform these steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="reset-ssh-key"></a><span data-ttu-id="e3496-116">Resetovat klíč SSH</span><span class="sxs-lookup"><span data-stu-id="e3496-116">Reset SSH key</span></span>
<span data-ttu-id="e3496-117">Následující příklad resetuje klíč SSH pro uživatele `azureuser` ve virtuálním počítači s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="e3496-117">The following example resets the SSH key for the user `azureuser` on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a><span data-ttu-id="e3496-118">Resetování hesla</span><span class="sxs-lookup"><span data-stu-id="e3496-118">Reset password</span></span>
<span data-ttu-id="e3496-119">Následující příklad resetuje heslo pro uživatele `azureuser` ve virtuálním počítači s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="e3496-119">The following example resets the password for the user `azureuser` on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a><span data-ttu-id="e3496-120">Restartujte SSH</span><span class="sxs-lookup"><span data-stu-id="e3496-120">Restart SSH</span></span>
<span data-ttu-id="e3496-121">V následujícím příkladu restartuje démon procesu SSH a konfiguraci SSH obnovíte výchozí hodnoty na virtuálním počítači s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="e3496-121">The following example restarts the SSH daemon and resets the SSH configuration to default values on a VM named `myVM`:</span></span>

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a><span data-ttu-id="e3496-122">Vytvoření uživatele</span><span class="sxs-lookup"><span data-stu-id="e3496-122">Create a user</span></span>
<span data-ttu-id="e3496-123">Následující příklad vytvoří uživatele s názvem `myNewUser` pomocí klíče SSH pro ověřování na virtuální počítač s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="e3496-123">The following example creates a user named `myNewUser` using an SSH key for authentication on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a><span data-ttu-id="e3496-124">Odstranit uživatele</span><span class="sxs-lookup"><span data-stu-id="e3496-124">Delete a user</span></span>
<span data-ttu-id="e3496-125">Následující příklad odstraní uživatele s názvem `myNewUser` ve virtuálním počítači s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="e3496-125">The following example deletes a user named `myNewUser` on the VM named `myVM`:</span></span>

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-the-vmaccess-extension"></a><span data-ttu-id="e3496-126">Použít soubory JSON a rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="e3496-126">Use JSON files and the VMAccess Extension</span></span>
<span data-ttu-id="e3496-127">Následující příklady použití nezpracované soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="e3496-127">The following examples use raw JSON files.</span></span> <span data-ttu-id="e3496-128">Použití [nastavení rozšíření virtuálního az](/cli/azure/vm/extension#set) pak volat souborů JSON.</span><span class="sxs-lookup"><span data-stu-id="e3496-128">Use [az vm extension set](/cli/azure/vm/extension#set) to then call your JSON files.</span></span> <span data-ttu-id="e3496-129">Tyto soubory JSON je možné také volat z šablony Azure.</span><span class="sxs-lookup"><span data-stu-id="e3496-129">These JSON files can also be called from Azure templates.</span></span> 

### <a name="reset-user-access"></a><span data-ttu-id="e3496-130">Obnovte uživatele přístup</span><span class="sxs-lookup"><span data-stu-id="e3496-130">Reset user access</span></span>
<span data-ttu-id="e3496-131">Pokud jste ztratili přístup ke kořenové na virtuálním počítačům s Linuxem, můžete spustit skript VMAccess k resetování klíč SSH uživatele nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="e3496-131">If you have lost access to root on your Linux VM, you can launch a VMAccess script to reset a user's SSH key or password.</span></span>

<span data-ttu-id="e3496-132">Resetovat veřejný klíč SSH uživatele, vytvořte soubor s názvem `reset_ssh_key.json` a přidat nastavení v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="e3496-132">To reset the SSH public key of a user, create a file named `reset_ssh_key.json` and add settings in the following format.</span></span> <span data-ttu-id="e3496-133">Dosaďte svoje vlastní hodnoty `username` a `ssh_key` parametry:</span><span class="sxs-lookup"><span data-stu-id="e3496-133">Substitute your own values for the `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

<span data-ttu-id="e3496-134">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="e3496-134">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

<span data-ttu-id="e3496-135">Pokud chcete resetovat heslo uživatele, vytvořte soubor s názvem `reset_user_password.json` a přidat nastavení v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="e3496-135">To reset a user password, create a file named `reset_user_password.json` and add settings in the following format.</span></span> <span data-ttu-id="e3496-136">Dosaďte svoje vlastní hodnoty `username` a `password` parametry:</span><span class="sxs-lookup"><span data-stu-id="e3496-136">Substitute your own values for the `username` and `password` parameters:</span></span>

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

<span data-ttu-id="e3496-137">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="e3496-137">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a><span data-ttu-id="e3496-138">Restartujte SSH</span><span class="sxs-lookup"><span data-stu-id="e3496-138">Restart SSH</span></span>
<span data-ttu-id="e3496-139">Pokud chcete restartovat démon procesu SSH a obnovit výchozí hodnoty v konfiguraci SSH, vytvořte soubor s názvem `reset_sshd.json`.</span><span class="sxs-lookup"><span data-stu-id="e3496-139">To restart the SSH daemon and reset the SSH configuration to default values, create a file named `reset_sshd.json`.</span></span> <span data-ttu-id="e3496-140">Přidejte do něj následující obsah:</span><span class="sxs-lookup"><span data-stu-id="e3496-140">Add the following content:</span></span>

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="e3496-141">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="e3496-141">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a><span data-ttu-id="e3496-142">Správa uživatelů</span><span class="sxs-lookup"><span data-stu-id="e3496-142">Manage users</span></span>

<span data-ttu-id="e3496-143">Chcete-li vytvořit uživatele, který používá klíč SSH pro ověřování, vytvořte soubor s názvem `create_new_user.json` a přidat nastavení v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="e3496-143">To create a user that uses an SSH key for authentication, create a file named `create_new_user.json` and add settings in the following format.</span></span> <span data-ttu-id="e3496-144">Dosaďte svoje vlastní hodnoty `username` a `ssh_key` parametry:</span><span class="sxs-lookup"><span data-stu-id="e3496-144">Substitute your own values for the `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="e3496-145">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="e3496-145">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

<span data-ttu-id="e3496-146">Pokud chcete odstranit uživatele, vytvořte soubor s názvem `delete_user.json` a přidejte následující obsah.</span><span class="sxs-lookup"><span data-stu-id="e3496-146">To delete a user, create a file named `delete_user.json` and add the following content.</span></span> <span data-ttu-id="e3496-147">Nahraďte vlastní hodnoty `remove_user` parametr:</span><span class="sxs-lookup"><span data-stu-id="e3496-147">Substitute your own value for the `remove_user` parameter:</span></span>

```json
{
  "remove_user":"myNewUser"
}
```

<span data-ttu-id="e3496-148">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="e3496-148">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a><span data-ttu-id="e3496-149">Zkontrolujte nebo opravte disku</span><span class="sxs-lookup"><span data-stu-id="e3496-149">Check or repair the disk</span></span>
<span data-ttu-id="e3496-150">Použitím VMAccess můžete také zkontrolujte a opravte disk, který jste přidali do virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e3496-150">Using VMAccess you can also check and repair a disk that you added to the Linux VM.</span></span>

<span data-ttu-id="e3496-151">Zkontrolujte a pak Opravte disk, vytvořte soubor s názvem `disk_check_repair.json` a přidat nastavení v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="e3496-151">To check and then repair the disk, create a file named `disk_check_repair.json` and add settings in the following format.</span></span> <span data-ttu-id="e3496-152">Nahraďte vlastní hodnotu pro název `repair_disk`:</span><span class="sxs-lookup"><span data-stu-id="e3496-152">Substitute your own value for the name of `repair_disk`:</span></span>

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

<span data-ttu-id="e3496-153">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="e3496-153">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a><span data-ttu-id="e3496-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3496-154">Next steps</span></span>
<span data-ttu-id="e3496-155">Aktualizace Linux pomocí rozšíření VMAccess Azure je jedna z metod k provádění změn v spuštěného virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e3496-155">Updating Linux using the Azure VMAccess Extension is one method to make changes on a running Linux VM.</span></span> <span data-ttu-id="e3496-156">Nástroje, například cloudové init a šablony Azure Resource Manager můžete také upravit virtuálním počítačům s Linuxem na spuštění.</span><span class="sxs-lookup"><span data-stu-id="e3496-156">You can also use tools like cloud-init and Azure Resource Manager templates to modify your Linux VM on boot.</span></span>

[<span data-ttu-id="e3496-157">Rozšíření virtuálního počítače a funkce pro Linux</span><span class="sxs-lookup"><span data-stu-id="e3496-157">Virtual machine extensions and features for Linux</span></span>](extensions-features.md)

[<span data-ttu-id="e3496-158">Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e3496-158">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="e3496-159">Přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí init cloudu</span><span class="sxs-lookup"><span data-stu-id="e3496-159">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md)

