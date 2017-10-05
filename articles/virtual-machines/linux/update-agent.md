---
title: Aktualizovat Azure Linux Agent z webu GitHub | Microsoft Docs
description: "Zjistěte, jak aktualizovat Azure Linux Agent pro váš virtuální počítač s Linuxem v Azure na nejnovější verzi z webu GitHub"
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
ms.openlocfilehash: c79e37976a58ae5384b5856e0f7f258a773ef0fd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-update-the-azure-linux-agent-on-a-vm"></a><span data-ttu-id="56a26-103">Postup aktualizace Azure Linux Agent na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="56a26-103">How to update the Azure Linux Agent on a VM</span></span>

<span data-ttu-id="56a26-104">Chcete-li aktualizovat vaše [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) na virtuální počítač s Linuxem v Azure, musíte už mít:</span><span class="sxs-lookup"><span data-stu-id="56a26-104">To update your [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) on a Linux VM in Azure, you must already have:</span></span>

- <span data-ttu-id="56a26-105">Spuštěného virtuálního počítače s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="56a26-105">A running Linux VM in Azure.</span></span>
- <span data-ttu-id="56a26-106">Připojení k této virtuální počítač s Linuxem pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="56a26-106">A connection to that Linux VM using SSH.</span></span>

<span data-ttu-id="56a26-107">Vždy zkontrolujte pro balíček v úložišti distro Linux nejdřív.</span><span class="sxs-lookup"><span data-stu-id="56a26-107">You should always check for a package in the Linux distro repository first.</span></span> <span data-ttu-id="56a26-108">Je možné, k dispozici balíček nemusí být, že na nejnovější verzi, ale povolení automatických aktualizací se ujistěte se, že Linux Agent bude vždy získat nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="56a26-108">It is possible the package available may not be the latest version, however, enabling autoupdate will ensure the Linux Agent will always get the latest update.</span></span> <span data-ttu-id="56a26-109">Budete mít problémy instalace ze Správce balíčku, byste se měli obrátit dodavatele distro podpory.</span><span class="sxs-lookup"><span data-stu-id="56a26-109">Should you have issues installing from the package managers, you should seek support from the distro vendor.</span></span>

## <a name="updating-the-azure-linux-agent"></a><span data-ttu-id="56a26-110">Aktualizace Azure Linux Agent</span><span class="sxs-lookup"><span data-stu-id="56a26-110">Updating the Azure Linux Agent</span></span>

## <a name="ubuntu"></a><span data-ttu-id="56a26-111">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="56a26-111">Ubuntu</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="56a26-112">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-112">Check your current package version</span></span>

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="56a26-113">Aktualizace mezipaměti balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-113">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a><span data-ttu-id="56a26-114">Nainstalujte nejnovější verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-114">Install the latest package version</span></span>

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="56a26-115">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-115">Ensure auto update is enabled</span></span>

<span data-ttu-id="56a26-116">Nejdřív zkontrolujte, zda je povolena:</span><span class="sxs-lookup"><span data-stu-id="56a26-116">First, check to see if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="56a26-117">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="56a26-117">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="56a26-118">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="56a26-118">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="56a26-119">Chcete-li povolit spuštění:</span><span class="sxs-lookup"><span data-stu-id="56a26-119">To enable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a><span data-ttu-id="56a26-120">Restartujte službu příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="56a26-120">Restart the waagent service</span></span>

#### <a name="restart-agent-for-1404"></a><span data-ttu-id="56a26-121">Restartovat agenta 14.04</span><span class="sxs-lookup"><span data-stu-id="56a26-121">Restart agent for 14.04</span></span>

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a><span data-ttu-id="56a26-122">Restartovat agenta 16.04 / 17.04</span><span class="sxs-lookup"><span data-stu-id="56a26-122">Restart agent for 16.04 / 17.04</span></span>

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a><span data-ttu-id="56a26-123">Debian</span><span class="sxs-lookup"><span data-stu-id="56a26-123">Debian</span></span>

### <a name="debian-7-wheezy"></a><span data-ttu-id="56a26-124">Debian 7 "Wheezy"</span><span class="sxs-lookup"><span data-stu-id="56a26-124">Debian 7 “Wheezy”</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="56a26-125">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-125">Check your current package version</span></span>

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="56a26-126">Aktualizace mezipaměti balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-126">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a><span data-ttu-id="56a26-127">Nainstalujte nejnovější verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-127">Install the latest package version</span></span>

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a><span data-ttu-id="56a26-128">Povolit automatické aktualizace agenta</span><span class="sxs-lookup"><span data-stu-id="56a26-128">Enable agent auto update</span></span>
<span data-ttu-id="56a26-129">Tato verze Debian nemá na verzi > = 2.0.16, proto není k dispozici pro něj automatických aktualizací.</span><span class="sxs-lookup"><span data-stu-id="56a26-129">This version of Debian does not have a version >= 2.0.16, therefore AutoUpdate is not available for it.</span></span> <span data-ttu-id="56a26-130">Výstup z výše uvedeném příkazu se zobrazí, pokud tento balíček je aktuální.</span><span class="sxs-lookup"><span data-stu-id="56a26-130">The output from the above command will show you if the package is up-to-date.</span></span>

### <a name="debian-8-jessie--debian-9-stretch"></a><span data-ttu-id="56a26-131">Debian 8 "Klára" / Debian 9 "Stretch"</span><span class="sxs-lookup"><span data-stu-id="56a26-131">Debian 8 “Jessie” / Debian 9 “Stretch”</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="56a26-132">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-132">Check your current package version</span></span>

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="56a26-133">Aktualizace mezipaměti balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-133">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a><span data-ttu-id="56a26-134">Nainstalujte nejnovější verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-134">Install the latest package version</span></span>

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="56a26-135">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-135">Ensure auto update is enabled</span></span> 

<span data-ttu-id="56a26-136">Nejdřív zkontrolujte, zda je povolena:</span><span class="sxs-lookup"><span data-stu-id="56a26-136">First, check to see if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="56a26-137">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="56a26-137">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="56a26-138">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="56a26-138">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="56a26-139">Chcete-li povolit spuštění:</span><span class="sxs-lookup"><span data-stu-id="56a26-139">To enable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a><span data-ttu-id="56a26-140">Restartujte službu příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="56a26-140">Restart the waagent service</span></span>

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a><span data-ttu-id="56a26-141">Red Hat nebo CentOS</span><span class="sxs-lookup"><span data-stu-id="56a26-141">Redhat / CentOS</span></span>

### <a name="rhelcentos-6"></a><span data-ttu-id="56a26-142">RHEL nebo CentOS 6</span><span class="sxs-lookup"><span data-stu-id="56a26-142">RHEL/CentOS 6</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="56a26-143">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-143">Check your current package version</span></span>

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a><span data-ttu-id="56a26-144">Zkontrolujte dostupné aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-144">Check available updates</span></span>

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a><span data-ttu-id="56a26-145">Nainstalujte nejnovější verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-145">Install the latest package version</span></span>

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="56a26-146">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-146">Ensure auto update is enabled</span></span> 

<span data-ttu-id="56a26-147">Nejdřív zkontrolujte, zda je povolena:</span><span class="sxs-lookup"><span data-stu-id="56a26-147">First, check to see if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="56a26-148">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="56a26-148">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="56a26-149">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="56a26-149">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="56a26-150">Chcete-li povolit spuštění:</span><span class="sxs-lookup"><span data-stu-id="56a26-150">To enable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a><span data-ttu-id="56a26-151">Restartujte službu příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="56a26-151">Restart the waagent service</span></span>

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a><span data-ttu-id="56a26-152">RHEL nebo CentOS 7</span><span class="sxs-lookup"><span data-stu-id="56a26-152">RHEL/CentOS 7</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="56a26-153">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-153">Check your current package version</span></span>

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a><span data-ttu-id="56a26-154">Zkontrolujte dostupné aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-154">Check available updates</span></span>

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a><span data-ttu-id="56a26-155">Nainstalujte nejnovější verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-155">Install the latest package version</span></span>

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="56a26-156">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-156">Ensure auto update is enabled</span></span> 

<span data-ttu-id="56a26-157">Nejdřív zkontrolujte, zda je povolena:</span><span class="sxs-lookup"><span data-stu-id="56a26-157">First, check to see if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="56a26-158">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="56a26-158">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="56a26-159">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="56a26-159">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="56a26-160">Chcete-li povolit spuštění:</span><span class="sxs-lookup"><span data-stu-id="56a26-160">To enable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a><span data-ttu-id="56a26-161">Restartujte službu příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="56a26-161">Restart the waagent service</span></span>

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a><span data-ttu-id="56a26-162">SUSE SLES</span><span class="sxs-lookup"><span data-stu-id="56a26-162">SUSE SLES</span></span>

### <a name="suse-sles-11-sp4"></a><span data-ttu-id="56a26-163">SUSE SLES 11 SP4</span><span class="sxs-lookup"><span data-stu-id="56a26-163">SUSE SLES 11 SP4</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="56a26-164">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-164">Check your current package version</span></span>

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a><span data-ttu-id="56a26-165">Zkontrolujte dostupné aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-165">Check available updates</span></span>

<span data-ttu-id="56a26-166">Výše uvedené výstup se zobrazí, pokud tento balíček je aktuální.</span><span class="sxs-lookup"><span data-stu-id="56a26-166">The above output will show you if the package is up to date.</span></span>

#### <a name="install-the-latest-package-version"></a><span data-ttu-id="56a26-167">Nainstalujte nejnovější verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-167">Install the latest package version</span></span>

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="56a26-168">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-168">Ensure auto update is enabled</span></span> 

<span data-ttu-id="56a26-169">Nejdřív zkontrolujte, zda je povolena:</span><span class="sxs-lookup"><span data-stu-id="56a26-169">First, check to see if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="56a26-170">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="56a26-170">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="56a26-171">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="56a26-171">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="56a26-172">Chcete-li povolit spuštění:</span><span class="sxs-lookup"><span data-stu-id="56a26-172">To enable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a><span data-ttu-id="56a26-173">Restartujte službu příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="56a26-173">Restart the waagent service</span></span>

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a><span data-ttu-id="56a26-174">SUSE SLES 12 SP2</span><span class="sxs-lookup"><span data-stu-id="56a26-174">SUSE SLES 12 SP2</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="56a26-175">Zkontrolujte aktuální verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-175">Check your current package version</span></span>

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a><span data-ttu-id="56a26-176">Zkontrolujte dostupné aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-176">Check available updates</span></span>

<span data-ttu-id="56a26-177">Ve výstupu výše to vám ukáže, pokud je balíček maximálně datum.</span><span class="sxs-lookup"><span data-stu-id="56a26-177">In the output from the above, this will show you if the package is upto date.</span></span>

#### <a name="install-the-latest-package-version"></a><span data-ttu-id="56a26-178">Nainstalujte nejnovější verzi balíčku</span><span class="sxs-lookup"><span data-stu-id="56a26-178">Install the latest package version</span></span>

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="56a26-179">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-179">Ensure auto update is enabled</span></span> 

<span data-ttu-id="56a26-180">Nejdřív zkontrolujte, zda je povolena:</span><span class="sxs-lookup"><span data-stu-id="56a26-180">First, check to see if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="56a26-181">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="56a26-181">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="56a26-182">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="56a26-182">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="56a26-183">Chcete-li povolit spuštění:</span><span class="sxs-lookup"><span data-stu-id="56a26-183">To enable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a><span data-ttu-id="56a26-184">Restartujte službu příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="56a26-184">Restart the waagent service</span></span>

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a><span data-ttu-id="56a26-185">Oracle 6 a 7</span><span class="sxs-lookup"><span data-stu-id="56a26-185">Oracle 6 and 7</span></span>

<span data-ttu-id="56a26-186">Pro Oracle Linux, ujistěte se, že `Addons` je povoleno úložiště.</span><span class="sxs-lookup"><span data-stu-id="56a26-186">For Oracle Linux, make sure that the `Addons` repository is enabled.</span></span> <span data-ttu-id="56a26-187">Vyberte, chcete-li upravit soubor `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux) a změňte řádek `enabled=0` k `enabled=1` pod **[ol6_addons]** nebo **[ol7_addons]** v tomto soubor.</span><span class="sxs-lookup"><span data-stu-id="56a26-187">Choose to edit the file `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), and change the line `enabled=0` to `enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

<span data-ttu-id="56a26-188">Poté nainstalujte nejnovější verzi Azure Linux Agent, zadejte:</span><span class="sxs-lookup"><span data-stu-id="56a26-188">Then, to install the latest version of the Azure Linux Agent, type:</span></span>

```bash
sudo yum install WALinuxAgent
```

<span data-ttu-id="56a26-189">Pokud nenajdete rozšíření úložiště můžete jednoduše přidat tyto řádky na konci souboru .repo podle vaší verze Oracle Linux:</span><span class="sxs-lookup"><span data-stu-id="56a26-189">If you don't find the add-on repository you can simply add these lines at the end of your .repo file according to your Oracle Linux release:</span></span>

<span data-ttu-id="56a26-190">Pro virtuální počítače, Oracle Linux 6:</span><span class="sxs-lookup"><span data-stu-id="56a26-190">For Oracle Linux 6 virtual machines:</span></span>

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

<span data-ttu-id="56a26-191">Pro virtuální počítače, Oracle Linux 7:</span><span class="sxs-lookup"><span data-stu-id="56a26-191">For Oracle Linux 7 virtual machines:</span></span>

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```

<span data-ttu-id="56a26-192">Poté zadejte:</span><span class="sxs-lookup"><span data-stu-id="56a26-192">Then type:</span></span>

```bash
sudo yum update WALinuxAgent
```

<span data-ttu-id="56a26-193">Obvykle to je vše, potřebujete, ale pokud z nějakého důvodu je třeba ji nainstalovat z https://github.com přímo, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="56a26-193">Typically this is all you need, but if for some reason you need to install it from https://github.com directly, use the following steps.</span></span>


## <a name="update-the-linux-agent-when-no-agent-package-exists-for-distribution"></a><span data-ttu-id="56a26-194">Aktualizovat agenta systému Linux, pokud žádný balíček agenta existuje pro distribuci</span><span class="sxs-lookup"><span data-stu-id="56a26-194">Update the Linux Agent when no agent package exists for distribution</span></span>

<span data-ttu-id="56a26-195">Nainstalujte wget (existují zde některých distribucích, který nemusíte instalovat ve výchozím nastavení, jako je například Redhat a CentOS, Oracle Linux verze 6.4 a 6.5) tak, že zadáte `sudo yum install wget` na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="56a26-195">Install wget (there are some distros that don't install it by default, such as Redhat, CentOS, and Oracle Linux versions 6.4 and 6.5) by typing `sudo yum install wget` on the command line.</span></span>

### <a name="1-download-the-latest-version"></a><span data-ttu-id="56a26-196">1. Stáhnout nejnovější verzi</span><span class="sxs-lookup"><span data-stu-id="56a26-196">1. Download the latest version</span></span>
<span data-ttu-id="56a26-197">Otevřete [verzi Azure Linux Agent v Githubu](https://github.com/Azure/WALinuxAgent/releases) ve webové stránky a zjistěte nejnovější číslo verze.</span><span class="sxs-lookup"><span data-stu-id="56a26-197">Open [the release of Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) in a web page, and find out the latest version number.</span></span> <span data-ttu-id="56a26-198">(Vaší aktuální verzí můžete vyhledat zadáním `waagent --version`.)</span><span class="sxs-lookup"><span data-stu-id="56a26-198">(You can locate your current version by typing `waagent --version`.)</span></span>

#### <a name="for-version-22x-or-later-type"></a><span data-ttu-id="56a26-199">Pro verzi 2.2.x nebo novější, zadejte:</span><span class="sxs-lookup"><span data-stu-id="56a26-199">For version 2.2.x or later, type:</span></span>
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip.zip
cd WALinuxAgent-2.2.x
```

<span data-ttu-id="56a26-200">Následující řádek používá verzi 2.2.0 jako příklad:</span><span class="sxs-lookup"><span data-stu-id="56a26-200">The following line uses version 2.2.0 as an example:</span></span>

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-the-azure-linux-agent"></a><span data-ttu-id="56a26-201">2. Nainstalujte agenta Azure Linux</span><span class="sxs-lookup"><span data-stu-id="56a26-201">2. Install the Azure Linux Agent</span></span>

#### <a name="for-version-22x-use"></a><span data-ttu-id="56a26-202">Pro verzi 2.2.x, použijte:</span><span class="sxs-lookup"><span data-stu-id="56a26-202">For version 2.2.x, use:</span></span>
<span data-ttu-id="56a26-203">Budete muset nainstalovat balíček `setuptools` nejprve – Viz [zde](https://pypi.python.org/pypi/setuptools).</span><span class="sxs-lookup"><span data-stu-id="56a26-203">You may need to install the package `setuptools` first--see [here](https://pypi.python.org/pypi/setuptools).</span></span> <span data-ttu-id="56a26-204">Potom spusťte:</span><span class="sxs-lookup"><span data-stu-id="56a26-204">Then run:</span></span>

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="56a26-205">Ujistěte se, že je povolena automatická aktualizace</span><span class="sxs-lookup"><span data-stu-id="56a26-205">Ensure auto update is enabled</span></span>

<span data-ttu-id="56a26-206">Nejdřív zkontrolujte, zda je povolena:</span><span class="sxs-lookup"><span data-stu-id="56a26-206">First, check to see if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="56a26-207">Najít 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="56a26-207">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="56a26-208">Pokud se zobrazí tento výstup, je povoleno:</span><span class="sxs-lookup"><span data-stu-id="56a26-208">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="56a26-209">Chcete-li povolit spuštění:</span><span class="sxs-lookup"><span data-stu-id="56a26-209">To enable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-the-waagent-service"></a><span data-ttu-id="56a26-210">3. Restartujte službu příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="56a26-210">3. Restart the waagent service</span></span>
<span data-ttu-id="56a26-211">Pro většinu distribucích systému Linux:</span><span class="sxs-lookup"><span data-stu-id="56a26-211">For most of Linux distros:</span></span>

```bash
sudo service waagent restart
```

<span data-ttu-id="56a26-212">Ubuntu použijte:</span><span class="sxs-lookup"><span data-stu-id="56a26-212">For Ubuntu, use:</span></span>

```bash
sudo service walinuxagent restart
```

<span data-ttu-id="56a26-213">Pro jádro operačního systému použijte:</span><span class="sxs-lookup"><span data-stu-id="56a26-213">For CoreOS, use:</span></span>

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-the-azure-linux-agent-version"></a><span data-ttu-id="56a26-214">4. Ověřit verzi Azure Linux Agent</span><span class="sxs-lookup"><span data-stu-id="56a26-214">4. Confirm the Azure Linux Agent version</span></span>
    
```bash
waagent -version
```

<span data-ttu-id="56a26-215">Pro jádro operačního systému nemusí fungovat výše uvedeném příkazu.</span><span class="sxs-lookup"><span data-stu-id="56a26-215">For CoreOS, the above command may not work.</span></span>

<span data-ttu-id="56a26-216">Zobrazí se, že verze Azure Linux Agent je aktualizovaná na novou verzi.</span><span class="sxs-lookup"><span data-stu-id="56a26-216">You will see that the Azure Linux Agent version has been updated to the new version.</span></span>

<span data-ttu-id="56a26-217">Další informace týkající se Azure Linux Agent najdete v tématu [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).</span><span class="sxs-lookup"><span data-stu-id="56a26-217">For more information regarding the Azure Linux Agent, see [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).</span></span>