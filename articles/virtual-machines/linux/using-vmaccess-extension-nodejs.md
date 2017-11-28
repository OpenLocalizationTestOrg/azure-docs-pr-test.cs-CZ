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
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-10"></a><span data-ttu-id="e0874-103">Spravovat uživatele, SSH a zkontrolujte nebo opravit disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess hello s hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e0874-103">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension with hello Azure CLI 1.0</span></span>
<span data-ttu-id="e0874-104">Tento článek ukazuje, jak toouse hello rozšíření Azure VMAcesss toocheck opravit disk, obnovte přístup uživatele, Správa uživatelských účtů nebo resetujte konfiguraci SSHD hello v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="e0874-104">This article shows you how toouse hello Azure VMAcesss Extension toocheck or repair a disk, reset user access, manage user accounts, or reset hello SSHD configuration on Linux.</span></span> <span data-ttu-id="e0874-105">článek Hello vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="e0874-105">hello article requires:</span></span>

* <span data-ttu-id="e0874-106">účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))</span><span class="sxs-lookup"><span data-stu-id="e0874-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="e0874-107">Hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) přihlášení `azure login`.</span><span class="sxs-lookup"><span data-stu-id="e0874-107">hello [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="e0874-108">Hello rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="e0874-108">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="e0874-109">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e0874-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="e0874-110">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="e0874-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="e0874-111">[Azure CLI 1.0](#quick-commands)– naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="e0874-111">[Azure CLI 1.0](#quick-commands)– our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="e0874-112">[Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="e0874-112">[Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="e0874-113">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="e0874-113">Quick commands</span></span>
<span data-ttu-id="e0874-114">Existují dva způsoby toouse VMAccess na virtuální počítače Linux:</span><span class="sxs-lookup"><span data-stu-id="e0874-114">There are two ways toouse VMAccess on your Linux VMs:</span></span>

* <span data-ttu-id="e0874-115">Pomocí Azure CLI 1.0 hello a hello požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="e0874-115">Using hello Azure CLI 1.0 and hello required parameters.</span></span>
* <span data-ttu-id="e0874-116">Pomocí nezpracované soubory JSON, které VMAccess procesy a potom na.</span><span class="sxs-lookup"><span data-stu-id="e0874-116">Using raw JSON files that VMAccess processes and then act on.</span></span>

<span data-ttu-id="e0874-117">Pro oddíl rychlý příkaz hello, přidáme toouse hello Azure CLI 1.0 `azure vm reset-access` metoda.</span><span class="sxs-lookup"><span data-stu-id="e0874-117">For hello quick command section, we are going toouse hello Azure CLI 1.0 `azure vm reset-access` method.</span></span> <span data-ttu-id="e0874-118">Následující příklady příkazů v hello místo hello hodnoty, které obsahují "příkladu" hello hodnotami ze svého vlastního prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0874-118">In hello following command examples, replace hello values that contain "example" with hello values from your own environment.</span></span>

## <a name="create-a-resource-group-and-linux-vm"></a><span data-ttu-id="e0874-119">Vytvoření skupiny prostředků a virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e0874-119">Create a Resource Group and Linux VM</span></span>
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a><span data-ttu-id="e0874-120">Vytvoření Debian virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e0874-120">Create a Debian VM</span></span>
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

## <a name="reset-root-password"></a><span data-ttu-id="e0874-121">Resetování hesla kořenového</span><span class="sxs-lookup"><span data-stu-id="e0874-121">Reset root password</span></span>
<span data-ttu-id="e0874-122">tooreset hello kořenové heslo:</span><span class="sxs-lookup"><span data-stu-id="e0874-122">tooreset hello root password:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a><span data-ttu-id="e0874-123">Obnovení klíče SSH</span><span class="sxs-lookup"><span data-stu-id="e0874-123">SSH key reset</span></span>
<span data-ttu-id="e0874-124">klíč SSH tooreset hello nekořenovými uživatele:</span><span class="sxs-lookup"><span data-stu-id="e0874-124">tooreset hello SSH key of a non-root user:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a><span data-ttu-id="e0874-125">Vytvoření uživatele</span><span class="sxs-lookup"><span data-stu-id="e0874-125">Create a user</span></span>
<span data-ttu-id="e0874-126">toocreate uživatele:</span><span class="sxs-lookup"><span data-stu-id="e0874-126">toocreate a user:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a><span data-ttu-id="e0874-127">Odebrání uživatele</span><span class="sxs-lookup"><span data-stu-id="e0874-127">Remove a user</span></span>
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a><span data-ttu-id="e0874-128">Resetování SSHD</span><span class="sxs-lookup"><span data-stu-id="e0874-128">Reset SSHD</span></span>
<span data-ttu-id="e0874-129">tooreset hello SSHD konfigurace:</span><span class="sxs-lookup"><span data-stu-id="e0874-129">tooreset hello SSHD configuration:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a><span data-ttu-id="e0874-130">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="e0874-130">Detailed walkthrough</span></span>
### <a name="vmaccess-defined"></a><span data-ttu-id="e0874-131">VMAccess definované:</span><span class="sxs-lookup"><span data-stu-id="e0874-131">VMAccess defined:</span></span>
<span data-ttu-id="e0874-132">Hello disk ve virtuálním počítačům s Linuxem se zobrazuje chyby.</span><span class="sxs-lookup"><span data-stu-id="e0874-132">hello disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="e0874-133">Nějakým způsobem resetovat hello kořenové heslo pro virtuální počítač Linux nebo omylem odstraněné svůj privátní klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="e0874-133">You somehow reset hello root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="e0874-134">V takovém případě zpět ve dnech hello hello datacenter by třeba toodrive existuje a pak otevřete hello KVM tooget v konzole serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e0874-134">If that happened back in hello days of hello datacenter, you would need toodrive there and then open hello KVM tooget at hello server console.</span></span> <span data-ttu-id="e0874-135">Hello rozšíření Azure VMAccess si můžete představit jako tento KVM přepínač, který umožňuje definovat můžete tooaccess hello konzoly tooreset přístup tooLinux nebo provést údržbu na úrovni disku.</span><span class="sxs-lookup"><span data-stu-id="e0874-135">Think of hello Azure VMAccess extension as that KVM switch that allows you tooaccess hello console tooreset access tooLinux or perform disk level maintenance.</span></span>

<span data-ttu-id="e0874-136">Pro hello podrobný návod, přidáme toouse hello dlouhých úseků VMAccess, který používá nezpracované soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="e0874-136">For hello detailed walkthrough, we are going toouse hello long form of VMAccess, which uses raw JSON files.</span></span>  <span data-ttu-id="e0874-137">Tyto soubory VMAccess JSON je možné také volat z šablony Azure.</span><span class="sxs-lookup"><span data-stu-id="e0874-137">These VMAccess JSON files can also be called from Azure templates.</span></span>

### <a name="using-vmaccess-toocheck-or-repair-hello-disk-of-a-linux-vm"></a><span data-ttu-id="e0874-138">Použitím VMAccess toocheck nebo oprava hello disku virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e0874-138">Using VMAccess toocheck or repair hello disk of a Linux VM</span></span>
<span data-ttu-id="e0874-139">Pomocí VMAccess můžete provést fsck spustit na hello disku pod virtuálním počítačům s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e0874-139">Using VMAccess you can do a fsck run on hello disk under your Linux VM.</span></span>  <span data-ttu-id="e0874-140">Můžete také provést kontrolu disku a disku opravit pomocí VMAccess.</span><span class="sxs-lookup"><span data-stu-id="e0874-140">You can also do a disk check and a disk repair using a VMAccess.</span></span>

<span data-ttu-id="e0874-141">Tento skript VMAccess použít toocheck a potom disk hello opravy:</span><span class="sxs-lookup"><span data-stu-id="e0874-141">toocheck, and then repair hello disk use this VMAccess script:</span></span>

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

<span data-ttu-id="e0874-142">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="e0874-142">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-tooreset-user-access-toolinux"></a><span data-ttu-id="e0874-143">Použitím VMAccess tooreset uživatel přístup tooLinux</span><span class="sxs-lookup"><span data-stu-id="e0874-143">Using VMAccess tooreset user access tooLinux</span></span>
<span data-ttu-id="e0874-144">Pokud jste ztratili přístup tooroot na virtuálním počítačům s Linuxem, můžete spustit skript VMAccess tooreset hello kořenové heslo.</span><span class="sxs-lookup"><span data-stu-id="e0874-144">If you have lost access tooroot on your Linux VM, you can launch a VMAccess script tooreset hello root password.</span></span>

<span data-ttu-id="e0874-145">tooreset hello kořenové heslo, použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="e0874-145">tooreset hello root password, use this VMAccess script:</span></span>

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

<span data-ttu-id="e0874-146">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="e0874-146">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

<span data-ttu-id="e0874-147">klíč SSH hello tooreset nekořenovými uživatele, použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="e0874-147">tooreset hello SSH key of a non-root user, use this VMAccess script:</span></span>

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

<span data-ttu-id="e0874-148">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="e0874-148">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-toomanage-user-accounts-on-linux"></a><span data-ttu-id="e0874-149">Použitím VMAccess toomanage uživatelské účty v systému Linux</span><span class="sxs-lookup"><span data-stu-id="e0874-149">Using VMAccess toomanage user accounts on Linux</span></span>
<span data-ttu-id="e0874-150">VMAccess je skript jazyka Python, který lze použít toomanage uživatele na virtuálním počítačům s Linuxem bez přihlášení a pomocí příkazu "sudo" nebo hello kořenového účtu.</span><span class="sxs-lookup"><span data-stu-id="e0874-150">VMAccess is a Python script that can be used toomanage users on your Linux VM without logging in and using sudo or hello root account.</span></span>

<span data-ttu-id="e0874-151">toocreate uživatele, použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="e0874-151">toocreate a user, use this VMAccess script:</span></span>

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="e0874-152">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="e0874-152">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

<span data-ttu-id="e0874-153">toodelete uživatele, použijte tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="e0874-153">toodelete a user, use this VMAccess script:</span></span>

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

<span data-ttu-id="e0874-154">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="e0874-154">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-tooreset-hello-sshd-configuration"></a><span data-ttu-id="e0874-155">Pomocí VMAccess tooreset hello SSHD konfigurace</span><span class="sxs-lookup"><span data-stu-id="e0874-155">Using VMAccess tooreset hello SSHD configuration</span></span>
<span data-ttu-id="e0874-156">Pokud provedete konfiguraci SSHD virtuální počítače Linux toohello změny a zavřít hello připojení SSH před ověřováním hello změny, může být zakázaný z SSH'ing zpět v.</span><span class="sxs-lookup"><span data-stu-id="e0874-156">If you make changes toohello Linux VMs SSHD configuration and close hello SSH connection before verifying hello changes, you may be prevented from SSH'ing back in.</span></span>  <span data-ttu-id="e0874-157">VMAccess lze použít tooreset hello SSHD konfigurace back tooa známé platné konfigurace bez právě přihlášen prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="e0874-157">VMAccess can be used tooreset hello SSHD configuration back tooa known good configuration without being logged in over SSH.</span></span>

<span data-ttu-id="e0874-158">tooreset hello SSHD konfiguraci můžete použít tento skript VMAccess:</span><span class="sxs-lookup"><span data-stu-id="e0874-158">tooreset hello SSHD configuration use this VMAccess script:</span></span>

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="e0874-159">Spusťte skript VMAccess hello se:</span><span class="sxs-lookup"><span data-stu-id="e0874-159">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a><span data-ttu-id="e0874-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0874-160">Next steps</span></span>
<span data-ttu-id="e0874-161">Aktualizace Linux používá rozšíření VMAccess Azure je jedna metoda toomake změny na spuštěného virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e0874-161">Updating Linux using Azure VMAccess Extensions is one method toomake changes on a running Linux VM.</span></span>  <span data-ttu-id="e0874-162">Můžete také použít nástroje, například cloudové init a šablon Azure toomodify virtuálním počítačům s Linuxem na spouštěcí.</span><span class="sxs-lookup"><span data-stu-id="e0874-162">You can also use tools like cloud-init and Azure Templates toomodify your Linux VM on boot.</span></span>

[<span data-ttu-id="e0874-163">O rozšíření virtuálního počítače a funkce</span><span class="sxs-lookup"><span data-stu-id="e0874-163">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="e0874-164">Vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e0874-164">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="e0874-165">Během vytváření pomocí toocustomize init cloudu virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e0874-165">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

