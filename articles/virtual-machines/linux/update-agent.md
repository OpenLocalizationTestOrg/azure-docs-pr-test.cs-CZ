---
title: aaaUpdate hello Azure Linux Agent z webu GitHub | Microsoft Docs
description: "Zjistěte, jak tooupdate Azure Linux Agent pro váš virtuální počítač s Linuxem v Azure toohello nejnovější verzi z webu GitHub"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: mingzhan
ms.openlocfilehash: 4ce7c56efc1e6563e6415f7687573f9fb9e7b4c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-hello-azure-linux-agent-on-a-vm"></a><span data-ttu-id="e0eb6-103">Jak tooupdate hello Azure Linux Agent na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="e0eb6-103">How tooupdate hello Azure Linux Agent on a VM</span></span>

<span data-ttu-id="e0eb6-104">tooupdate vaše [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) na virtuální počítač s Linuxem v Azure, musíte už mít:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-104">tooupdate your [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) on a Linux VM in Azure, you must already have:</span></span>

- <span data-ttu-id="e0eb6-105">Spuštěného virtuálního počítače s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-105">A running Linux VM in Azure.</span></span>
- <span data-ttu-id="e0eb6-106">Toothat připojení virtuálního počítače s Linuxem pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-106">A connection toothat Linux VM using SSH.</span></span>

<span data-ttu-id="e0eb6-107">Vždy zkontrolujte pro balíček v úložišti distro Linux hello nejdřív.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-107">You should always check for a package in hello Linux distro repository first.</span></span> <span data-ttu-id="e0eb6-108">Je možné hello balíčku k dispozici nemusí být hello nejnovější verzi, ale povolení automatických aktualizací zajistí hello agenta systému Linux bude vždycky získat nejnovější aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-108">It is possible hello package available may not be hello latest version, however, enabling autoupdate will ensure hello Linux Agent will always get hello latest update.</span></span> <span data-ttu-id="e0eb6-109">Budete mít problémy instalace z hello balíček správci, byste se měli obrátit podporu od dodavatele distro hello.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-109">Should you have issues installing from hello package managers, you should seek support from hello distro vendor.</span></span>

## <a name="updating-hello-azure-linux-agent"></a><span data-ttu-id="e0eb6-110">Aktualizace hello Azure Linux Agent</span><span class="sxs-lookup"><span data-stu-id="e0eb6-110">Updating hello Azure Linux Agent</span></span>

## <a name="ubuntu"></a><span data-ttu-id="e0eb6-111">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e0eb6-111">Ubuntu</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="e0eb6-112">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-112">Check your current package version</span></span>

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="e0eb6-113">Aktualizace mezipaměti balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-113">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="e0eb6-114">Nainstalujte nejnovější verzi balíčku hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-114">Install hello latest package version</span></span>

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="e0eb6-115">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-115">Ensure auto update is enabled</span></span>

<span data-ttu-id="e0eb6-116">Nejdřív zkontrolujte toosee, je-li aktivní:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-116">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="e0eb6-117">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-117">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="e0eb6-118">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-118">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="e0eb6-119">tooenable spustit:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-119">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="e0eb6-120">Restartujte službu příkaz waagent hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-120">Restart hello waagent service</span></span>

#### <a name="restart-agent-for-1404"></a><span data-ttu-id="e0eb6-121">Restartovat agenta 14.04</span><span class="sxs-lookup"><span data-stu-id="e0eb6-121">Restart agent for 14.04</span></span>

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a><span data-ttu-id="e0eb6-122">Restartovat agenta 16.04 / 17.04</span><span class="sxs-lookup"><span data-stu-id="e0eb6-122">Restart agent for 16.04 / 17.04</span></span>

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a><span data-ttu-id="e0eb6-123">Debian</span><span class="sxs-lookup"><span data-stu-id="e0eb6-123">Debian</span></span>

### <a name="debian-7-wheezy"></a><span data-ttu-id="e0eb6-124">Debian 7 "Wheezy"</span><span class="sxs-lookup"><span data-stu-id="e0eb6-124">Debian 7 “Wheezy”</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="e0eb6-125">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-125">Check your current package version</span></span>

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="e0eb6-126">Aktualizace mezipaměti balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-126">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="e0eb6-127">Nainstalujte nejnovější verzi balíčku hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-127">Install hello latest package version</span></span>

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a><span data-ttu-id="e0eb6-128">Povolit automatické aktualizace agenta</span><span class="sxs-lookup"><span data-stu-id="e0eb6-128">Enable agent auto update</span></span>
<span data-ttu-id="e0eb6-129">Tato verze Debian nemá na verzi > = 2.0.16, proto není k dispozici pro něj automatických aktualizací.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-129">This version of Debian does not have a version >= 2.0.16, therefore AutoUpdate is not available for it.</span></span> <span data-ttu-id="e0eb6-130">výstup Hello hello výše příkazu se zobrazí, pokud balíček hello je aktuální.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-130">hello output from hello above command will show you if hello package is up-to-date.</span></span>

### <a name="debian-8-jessie--debian-9-stretch"></a><span data-ttu-id="e0eb6-131">Debian 8 "Klára" / Debian 9 "Stretch"</span><span class="sxs-lookup"><span data-stu-id="e0eb6-131">Debian 8 “Jessie” / Debian 9 “Stretch”</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="e0eb6-132">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-132">Check your current package version</span></span>

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="e0eb6-133">Aktualizace mezipaměti balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-133">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="e0eb6-134">Nainstalujte nejnovější verzi balíčku hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-134">Install hello latest package version</span></span>

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="e0eb6-135">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-135">Ensure auto update is enabled</span></span> 

<span data-ttu-id="e0eb6-136">Nejdřív zkontrolujte toosee, je-li aktivní:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-136">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="e0eb6-137">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-137">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="e0eb6-138">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-138">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="e0eb6-139">tooenable spustit:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-139">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="e0eb6-140">Restartujte službu příkaz waagent hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-140">Restart hello waagent service</span></span>

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a><span data-ttu-id="e0eb6-141">Red Hat nebo CentOS</span><span class="sxs-lookup"><span data-stu-id="e0eb6-141">Redhat / CentOS</span></span>

### <a name="rhelcentos-6"></a><span data-ttu-id="e0eb6-142">RHEL nebo CentOS 6</span><span class="sxs-lookup"><span data-stu-id="e0eb6-142">RHEL/CentOS 6</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="e0eb6-143">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-143">Check your current package version</span></span>

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a><span data-ttu-id="e0eb6-144">Zkontrolujte dostupné aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-144">Check available updates</span></span>

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="e0eb6-145">Nainstalujte nejnovější verzi balíčku hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-145">Install hello latest package version</span></span>

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="e0eb6-146">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-146">Ensure auto update is enabled</span></span> 

<span data-ttu-id="e0eb6-147">Nejdřív zkontrolujte toosee, je-li aktivní:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-147">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="e0eb6-148">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-148">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="e0eb6-149">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-149">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="e0eb6-150">tooenable spustit:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-150">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="e0eb6-151">Restartujte službu příkaz waagent hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-151">Restart hello waagent service</span></span>

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a><span data-ttu-id="e0eb6-152">RHEL nebo CentOS 7</span><span class="sxs-lookup"><span data-stu-id="e0eb6-152">RHEL/CentOS 7</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="e0eb6-153">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-153">Check your current package version</span></span>

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a><span data-ttu-id="e0eb6-154">Zkontrolujte dostupné aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-154">Check available updates</span></span>

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="e0eb6-155">Nainstalujte nejnovější verzi balíčku hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-155">Install hello latest package version</span></span>

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="e0eb6-156">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-156">Ensure auto update is enabled</span></span> 

<span data-ttu-id="e0eb6-157">Nejdřív zkontrolujte toosee, je-li aktivní:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-157">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="e0eb6-158">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-158">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="e0eb6-159">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-159">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="e0eb6-160">tooenable spustit:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-160">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="e0eb6-161">Restartujte službu příkaz waagent hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-161">Restart hello waagent service</span></span>

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a><span data-ttu-id="e0eb6-162">SUSE SLES</span><span class="sxs-lookup"><span data-stu-id="e0eb6-162">SUSE SLES</span></span>

### <a name="suse-sles-11-sp4"></a><span data-ttu-id="e0eb6-163">SUSE SLES 11 SP4</span><span class="sxs-lookup"><span data-stu-id="e0eb6-163">SUSE SLES 11 SP4</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="e0eb6-164">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-164">Check your current package version</span></span>

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a><span data-ttu-id="e0eb6-165">Zkontrolujte dostupné aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-165">Check available updates</span></span>

<span data-ttu-id="e0eb6-166">Hello výše výstupu se zobrazí, pokud je balíček hello až toodate.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-166">hello above output will show you if hello package is up toodate.</span></span>

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="e0eb6-167">Nainstalujte nejnovější verzi balíčku hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-167">Install hello latest package version</span></span>

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="e0eb6-168">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-168">Ensure auto update is enabled</span></span> 

<span data-ttu-id="e0eb6-169">Nejdřív zkontrolujte toosee, je-li aktivní:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-169">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="e0eb6-170">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-170">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="e0eb6-171">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-171">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="e0eb6-172">tooenable spustit:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-172">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="e0eb6-173">Restartujte službu příkaz waagent hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-173">Restart hello waagent service</span></span>

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a><span data-ttu-id="e0eb6-174">SUSE SLES 12 SP2</span><span class="sxs-lookup"><span data-stu-id="e0eb6-174">SUSE SLES 12 SP2</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="e0eb6-175">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="e0eb6-175">Check your current package version</span></span>

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a><span data-ttu-id="e0eb6-176">Zkontrolujte dostupné aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-176">Check available updates</span></span>

<span data-ttu-id="e0eb6-177">Ve výstupu hello z výše uvedených hello to vám ukáže, pokud je balíček hello maximálně datum.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-177">In hello output from hello above, this will show you if hello package is upto date.</span></span>

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="e0eb6-178">Nainstalujte nejnovější verzi balíčku hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-178">Install hello latest package version</span></span>

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="e0eb6-179">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-179">Ensure auto update is enabled</span></span> 

<span data-ttu-id="e0eb6-180">Nejdřív zkontrolujte toosee, je-li aktivní:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-180">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="e0eb6-181">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-181">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="e0eb6-182">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-182">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="e0eb6-183">tooenable spustit:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-183">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="e0eb6-184">Restartujte službu příkaz waagent hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-184">Restart hello waagent service</span></span>

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a><span data-ttu-id="e0eb6-185">Oracle 6 a 7</span><span class="sxs-lookup"><span data-stu-id="e0eb6-185">Oracle 6 and 7</span></span>

<span data-ttu-id="e0eb6-186">Pro Oracle Linux, ujistěte se, že hello `Addons` je povoleno úložiště.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-186">For Oracle Linux, make sure that hello `Addons` repository is enabled.</span></span> <span data-ttu-id="e0eb6-187">Vybrat soubor hello tooedit `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux) a změňte řádek hello `enabled=0` příliš`enabled=1` pod **[ol6_addons]** nebo **[ol7_addons]** v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-187">Choose tooedit hello file `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), and change hello line `enabled=0` too`enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

<span data-ttu-id="e0eb6-188">Potom tooinstall hello nejnovější verzi hello Azure Linux Agent, typ:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-188">Then, tooinstall hello latest version of hello Azure Linux Agent, type:</span></span>

```bash
sudo yum install WALinuxAgent
```

<span data-ttu-id="e0eb6-189">Pokud nenajdete hello rozšíření úložiště můžete jednoduše přidat tyto řádky na konci hello .repo souboru podle verze tooyour Oracle Linux:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-189">If you don't find hello add-on repository you can simply add these lines at hello end of your .repo file according tooyour Oracle Linux release:</span></span>

<span data-ttu-id="e0eb6-190">Pro virtuální počítače, Oracle Linux 6:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-190">For Oracle Linux 6 virtual machines:</span></span>

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

<span data-ttu-id="e0eb6-191">Pro virtuální počítače, Oracle Linux 7:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-191">For Oracle Linux 7 virtual machines:</span></span>

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```

<span data-ttu-id="e0eb6-192">Poté zadejte:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-192">Then type:</span></span>

```bash
sudo yum update WALinuxAgent
```

<span data-ttu-id="e0eb6-193">Obvykle to je všechno, co potřebujete, ale pokud z nějakého důvodu že potřebujete tooinstall z https://github.com přímo, hello použijte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-193">Typically this is all you need, but if for some reason you need tooinstall it from https://github.com directly, use hello following steps.</span></span>


## <a name="update-hello-linux-agent-when-no-agent-package-exists-for-distribution"></a><span data-ttu-id="e0eb6-194">Hello aktualizace agenta systému Linux při žádný balíček agenta existuje pro distribuci</span><span class="sxs-lookup"><span data-stu-id="e0eb6-194">Update hello Linux Agent when no agent package exists for distribution</span></span>

<span data-ttu-id="e0eb6-195">Nainstalujte wget (existují zde některých distribucích, který nemusíte instalovat ve výchozím nastavení, jako je například Redhat a CentOS, Oracle Linux verze 6.4 a 6.5) tak, že zadáte `sudo yum install wget` na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-195">Install wget (there are some distros that don't install it by default, such as Redhat, CentOS, and Oracle Linux versions 6.4 and 6.5) by typing `sudo yum install wget` on hello command line.</span></span>

### <a name="1-download-hello-latest-version"></a><span data-ttu-id="e0eb6-196">1. Stažení nejnovější verze hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-196">1. Download hello latest version</span></span>
<span data-ttu-id="e0eb6-197">Otevřete [hello verzi Azure Linux Agent v Githubu](https://github.com/Azure/WALinuxAgent/releases) ve webové stránky a zjistěte hello nejnovější číslo verze.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-197">Open [hello release of Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) in a web page, and find out hello latest version number.</span></span> <span data-ttu-id="e0eb6-198">(Vaší aktuální verzí můžete vyhledat zadáním `waagent --version`.)</span><span class="sxs-lookup"><span data-stu-id="e0eb6-198">(You can locate your current version by typing `waagent --version`.)</span></span>

#### <a name="for-version-22x-or-later-type"></a><span data-ttu-id="e0eb6-199">Pro verzi 2.2.x nebo novější, zadejte:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-199">For version 2.2.x or later, type:</span></span>
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip.zip
cd WALinuxAgent-2.2.x
```

<span data-ttu-id="e0eb6-200">Hello následující řádek používá verzi 2.2.0 jako příklad:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-200">hello following line uses version 2.2.0 as an example:</span></span>

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-hello-azure-linux-agent"></a><span data-ttu-id="e0eb6-201">2. Nainstalujte hello Azure Linux Agent</span><span class="sxs-lookup"><span data-stu-id="e0eb6-201">2. Install hello Azure Linux Agent</span></span>

#### <a name="for-version-22x-use"></a><span data-ttu-id="e0eb6-202">Pro verzi 2.2.x, použijte:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-202">For version 2.2.x, use:</span></span>
<span data-ttu-id="e0eb6-203">Může být nutné balíček hello tooinstall `setuptools` nejprve – Viz [zde](https://pypi.python.org/pypi/setuptools).</span><span class="sxs-lookup"><span data-stu-id="e0eb6-203">You may need tooinstall hello package `setuptools` first--see [here](https://pypi.python.org/pypi/setuptools).</span></span> <span data-ttu-id="e0eb6-204">Potom spusťte:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-204">Then run:</span></span>

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="e0eb6-205">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="e0eb6-205">Ensure auto update is enabled</span></span>

<span data-ttu-id="e0eb6-206">Nejdřív zkontrolujte toosee, je-li aktivní:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-206">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="e0eb6-207">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-207">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="e0eb6-208">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-208">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="e0eb6-209">tooenable spustit:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-209">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-hello-waagent-service"></a><span data-ttu-id="e0eb6-210">3. Restartujte službu příkaz waagent hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-210">3. Restart hello waagent service</span></span>
<span data-ttu-id="e0eb6-211">Pro většinu distribucích systému Linux:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-211">For most of Linux distros:</span></span>

```bash
sudo service waagent restart
```

<span data-ttu-id="e0eb6-212">Ubuntu použijte:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-212">For Ubuntu, use:</span></span>

```bash
sudo service walinuxagent restart
```

<span data-ttu-id="e0eb6-213">Pro jádro operačního systému použijte:</span><span class="sxs-lookup"><span data-stu-id="e0eb6-213">For CoreOS, use:</span></span>

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-hello-azure-linux-agent-version"></a><span data-ttu-id="e0eb6-214">4. Ověřit verzi Azure Linux Agent hello</span><span class="sxs-lookup"><span data-stu-id="e0eb6-214">4. Confirm hello Azure Linux Agent version</span></span>
    
```bash
waagent -version
```

<span data-ttu-id="e0eb6-215">Pro jádro operačního systému nemusí fungovat hello výše příkaz.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-215">For CoreOS, hello above command may not work.</span></span>

<span data-ttu-id="e0eb6-216">Uvidíte, že tento hello Azure Linux Agent byla aktualizována toohello novou verzi.</span><span class="sxs-lookup"><span data-stu-id="e0eb6-216">You will see that hello Azure Linux Agent version has been updated toohello new version.</span></span>

<span data-ttu-id="e0eb6-217">Další informace týkající se hello Azure Linux Agent najdete v tématu [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).</span><span class="sxs-lookup"><span data-stu-id="e0eb6-217">For more information regarding hello Azure Linux Agent, see [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).</span></span>
