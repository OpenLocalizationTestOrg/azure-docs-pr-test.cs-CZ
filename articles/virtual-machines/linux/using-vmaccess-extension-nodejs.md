---
title: "Obnovte přístup na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess | Microsoft Docs"
description: "Obnovte přístup na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess."
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
ms.openlocfilehash: 278bf1785aac71068ab94cf9916af69a204c44be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-10"></a><span data-ttu-id="faae6-103">Spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuálních počítačích Azure Linux rozšíření VMAccess pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="faae6-103">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension with the Azure CLI 1.0</span></span>
<span data-ttu-id="faae6-104">Tento článek ukazuje, jak používat rozšíření VMAcesss Azure zkontrolujte nebo opravit disk, obnovte přístup uživatele, Správa uživatelských účtů nebo obnovit konfiguraci SSHD v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="faae6-104">This article shows you how to use the Azure VMAcesss Extension to check or repair a disk, reset user access, manage user accounts, or reset the SSHD configuration on Linux.</span></span> <span data-ttu-id="faae6-105">Tento článek vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="faae6-105">The article requires:</span></span>

* <span data-ttu-id="faae6-106">účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))</span><span class="sxs-lookup"><span data-stu-id="faae6-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="faae6-107">[rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) s přihlášením `azure login`.</span><span class="sxs-lookup"><span data-stu-id="faae6-107">the [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="faae6-108">Rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager`azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="faae6-108">the Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="faae6-109">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="faae6-109">CLI versions to complete the task</span></span>
<span data-ttu-id="faae6-110">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="faae6-110">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="faae6-111">[Azure CLI 1.0](#quick-commands)– naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="faae6-111">[Azure CLI 1.0](#quick-commands)– our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="faae6-112">[Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="faae6-112">[Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="faae6-113">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="faae6-113">Quick commands</span></span>
<span data-ttu-id="faae6-114">Existují dva způsoby, jak používat VMAccess na virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="faae6-114">There are two ways to use VMAccess on your Linux VMs:</span></span>

* <span data-ttu-id="faae6-115">Pomocí Azure CLI 1.0 a požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="faae6-115">Using the Azure CLI 1.0 and the required parameters.</span></span>
* <span data-ttu-id="faae6-116">Pomocí nezpracované soubory JSON, které VMAccess procesy a potom na.</span><span class="sxs-lookup"><span data-stu-id="faae6-116">Using raw JSON files that VMAccess processes and then act on.</span></span>

<span data-ttu-id="faae6-117">Pro oddíl rychlý příkaz přidáme 1.0 rozhraní příkazového řádku Azure používat `azure vm reset-access` metoda.</span><span class="sxs-lookup"><span data-stu-id="faae6-117">For the quick command section, we are going to use the Azure CLI 1.0 `azure vm reset-access` method.</span></span> <span data-ttu-id="faae6-118">V následujících příkladech nahraďte hodnoty, které obsahují "Příklad" s hodnotami ze svého vlastního prostředí.</span><span class="sxs-lookup"><span data-stu-id="faae6-118">In the following command examples, replace the values that contain "example" with the values from your own environment.</span></span>

## <a name="create-a-resource-group-and-linux-vm"></a><span data-ttu-id="faae6-119">Vytvoření skupiny prostředků a virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="faae6-119">Create a Resource Group and Linux VM</span></span>
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a><span data-ttu-id="faae6-120">Vytvoření Debian virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="faae6-120">Create a Debian VM</span></span>
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

## <a name="reset-root-password"></a><span data-ttu-id="faae6-121">Resetování hesla kořenového</span><span class="sxs-lookup"><span data-stu-id="faae6-121">Reset root password</span></span>
<span data-ttu-id="faae6-122">Resetování hesla kořenového:</span><span class="sxs-lookup"><span data-stu-id="faae6-122">To reset the root password:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a><span data-ttu-id="faae6-123">Obnovení klíče SSH</span><span class="sxs-lookup"><span data-stu-id="faae6-123">SSH key reset</span></span>
<span data-ttu-id="faae6-124">Chcete-li obnovit klíč SSH nekořenovými uživatele:</span><span class="sxs-lookup"><span data-stu-id="faae6-124">To reset the SSH key of a non-root user:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a><span data-ttu-id="faae6-125">Vytvoření uživatele</span><span class="sxs-lookup"><span data-stu-id="faae6-125">Create a user</span></span>
<span data-ttu-id="faae6-126">Chcete-li vytvořit uživateli:</span><span class="sxs-lookup"><span data-stu-id="faae6-126">To create a user:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a><span data-ttu-id="faae6-127">Odebrání uživatele</span><span class="sxs-lookup"><span data-stu-id="faae6-127">Remove a user</span></span>
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a><span data-ttu-id="faae6-128">Resetování SSHD</span><span class="sxs-lookup"><span data-stu-id="faae6-128">Reset SSHD</span></span>
<span data-ttu-id="faae6-129">Chcete-li obnovit konfiguraci SSHD:</span><span class="sxs-lookup"><span data-stu-id="faae6-129">To reset the SSHD configuration:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a><span data-ttu-id="faae6-130">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="faae6-130">Detailed walkthrough</span></span>
### <a name="vmaccess-defined"></a><span data-ttu-id="faae6-131">VMAccess definované:</span><span class="sxs-lookup"><span data-stu-id="faae6-131">VMAccess defined:</span></span>
<span data-ttu-id="faae6-132">Disk ve virtuálním počítačům s Linuxem se zobrazuje chyby.</span><span class="sxs-lookup"><span data-stu-id="faae6-132">The disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="faae6-133">Nějakým způsobem resetování hesla kořenového virtuálním počítačům s Linuxem nebo omylem odstraněné svůj privátní klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="faae6-133">You somehow reset the root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="faae6-134">V takovém případě zpět v dny v datovém centru potřebovali byste existuje jednotky a pak otevřete KVM získat z konzoly serveru.</span><span class="sxs-lookup"><span data-stu-id="faae6-134">If that happened back in the days of the datacenter, you would need to drive there and then open the KVM to get at the server console.</span></span> <span data-ttu-id="faae6-135">Rozšíření Azure VMAccess si můžete představit jako že KVM přepínačů, která umožňuje přístup ke konzole resetovat přístup do systému Linux nebo provést údržbu na úrovni disku.</span><span class="sxs-lookup"><span data-stu-id="faae6-135">Think of the Azure VMAccess extension as that KVM switch that allows you to access the console to reset access to Linux or perform disk level maintenance.</span></span>

<span data-ttu-id="faae6-136">Podrobný postup přidáme dlouho tvar VMAccess, který používá nezpracované soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="faae6-136">For the detailed walkthrough, we are going to use the long form of VMAccess, which uses raw JSON files.</span></span>  <span data-ttu-id="faae6-137">Tyto soubory VMAccess JSON je možné také volat z šablony Azure.</span><span class="sxs-lookup"><span data-stu-id="faae6-137">These VMAccess JSON files can also be called from Azure templates.</span></span>

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a><span data-ttu-id="faae6-138">Použitím VMAccess zkontrolujte nebo opravte disku virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="faae6-138">Using VMAccess to check or repair the disk of a Linux VM</span></span>
<span data-ttu-id="faae6-139">Pomocí VMAccess můžete provést fsck spustit na disku v rámci vašeho virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="faae6-139">Using VMAccess you can do a fsck run on the disk under your Linux VM.</span></span>  <span data-ttu-id="faae6-140">Můžete také provést kontrolu disku a disku opravit pomocí VMAccess.</span><span class="sxs-lookup"><span data-stu-id="faae6-140">You can also do a disk check and a disk repair using a VMAccess.</span></span>

<span data-ttu-id="faae6-141">Kontrolovat, a pak opravte disku použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="faae6-141">To check, and then repair the disk use this VMAccess script:</span></span>

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

<span data-ttu-id="faae6-142">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="faae6-142">Execute the VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a><span data-ttu-id="faae6-143">Použitím VMAccess k resetování uživatelský přístup do systému Linux</span><span class="sxs-lookup"><span data-stu-id="faae6-143">Using VMAccess to reset user access to Linux</span></span>
<span data-ttu-id="faae6-144">Pokud jste ztratili přístup ke kořenové na virtuálním počítačům s Linuxem, můžete spustit skript VMAccess k resetování hesla root.</span><span class="sxs-lookup"><span data-stu-id="faae6-144">If you have lost access to root on your Linux VM, you can launch a VMAccess script to reset the root password.</span></span>

<span data-ttu-id="faae6-145">Pokud chcete resetovat hesla kořenového, použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="faae6-145">To reset the root password, use this VMAccess script:</span></span>

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

<span data-ttu-id="faae6-146">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="faae6-146">Execute the VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

<span data-ttu-id="faae6-147">Pokud chcete resetovat klíč SSH nekořenovými uživatele, použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="faae6-147">To reset the SSH key of a non-root user, use this VMAccess script:</span></span>

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

<span data-ttu-id="faae6-148">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="faae6-148">Execute the VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a><span data-ttu-id="faae6-149">Použití VMAccess ke správě uživatelských účtů v systému Linux</span><span class="sxs-lookup"><span data-stu-id="faae6-149">Using VMAccess to manage user accounts on Linux</span></span>
<span data-ttu-id="faae6-150">VMAccess je skript jazyka Python, který lze použít ke správě uživatelů na virtuálním počítačům s Linuxem bez přihlášení a pomocí příkazu "sudo" nebo kořenového účtu.</span><span class="sxs-lookup"><span data-stu-id="faae6-150">VMAccess is a Python script that can be used to manage users on your Linux VM without logging in and using sudo or the root account.</span></span>

<span data-ttu-id="faae6-151">Chcete-li vytvořit uživatele, použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="faae6-151">To create a user, use this VMAccess script:</span></span>

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="faae6-152">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="faae6-152">Execute the VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

<span data-ttu-id="faae6-153">Chcete-li odstranit uživatele, použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="faae6-153">To delete a user, use this VMAccess script:</span></span>

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

<span data-ttu-id="faae6-154">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="faae6-154">Execute the VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a><span data-ttu-id="faae6-155">Použití VMAccess k resetování konfigurace SSHD</span><span class="sxs-lookup"><span data-stu-id="faae6-155">Using VMAccess to reset the SSHD configuration</span></span>
<span data-ttu-id="faae6-156">Pokud provádět změny konfigurace SSHD virtuální počítače Linux a zavřete připojení SSH před ověřováním změny, může být zakázaný z SSH'ing zpět v.</span><span class="sxs-lookup"><span data-stu-id="faae6-156">If you make changes to the Linux VMs SSHD configuration and close the SSH connection before verifying the changes, you may be prevented from SSH'ing back in.</span></span>  <span data-ttu-id="faae6-157">VMAccess umožňuje obnovit konfiguraci SSHD známé platné konfigurace bez právě přihlášen prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="faae6-157">VMAccess can be used to reset the SSHD configuration back to a known good configuration without being logged in over SSH.</span></span>

<span data-ttu-id="faae6-158">Chcete-li obnovit konfiguraci SSHD můžete použít tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="faae6-158">To reset the SSHD configuration use this VMAccess script:</span></span>

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="faae6-159">Spuštění skriptu VMAccess pomocí:</span><span class="sxs-lookup"><span data-stu-id="faae6-159">Execute the VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a><span data-ttu-id="faae6-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="faae6-160">Next steps</span></span>
<span data-ttu-id="faae6-161">Aktualizace Linux používá rozšíření VMAccess Azure je jedna z metod k provádění změn v spuštěného virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="faae6-161">Updating Linux using Azure VMAccess Extensions is one method to make changes on a running Linux VM.</span></span>  <span data-ttu-id="faae6-162">Nástroje, například cloudové init a šablon Azure můžete taky upravit virtuálním počítačům s Linuxem na spuštění.</span><span class="sxs-lookup"><span data-stu-id="faae6-162">You can also use tools like cloud-init and Azure Templates to modify your Linux VM on boot.</span></span>

[<span data-ttu-id="faae6-163">O rozšíření virtuálního počítače a funkce</span><span class="sxs-lookup"><span data-stu-id="faae6-163">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="faae6-164">Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="faae6-164">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="faae6-165">Přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí init cloudu</span><span class="sxs-lookup"><span data-stu-id="faae6-165">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

