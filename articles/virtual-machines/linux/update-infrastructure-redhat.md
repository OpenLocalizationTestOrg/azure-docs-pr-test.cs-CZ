---
title: Red Hat aktualizace infrastruktury (RHUI) | Microsoft Docs
description: "Další informace o Red Hat aktualizace infrastruktury (RHUI) pro instance Red Hat Enterprise Linux na vyžádání v Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: BorisB2015
manager: timlt
editor: 
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: borisb
ms.openlocfilehash: 07815d691ffe57f0349f7a90ced4a2fcc1ab834f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a><span data-ttu-id="45d22-103">Red Hat aktualizace infrastruktury (RHUI) pro na vyžádání Red Hat Enterprise Linux virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="45d22-103">Red Hat Update Infrastructure (RHUI) for on-demand Red Hat Enterprise Linux VMs in Azure</span></span>
<span data-ttu-id="45d22-104">Pro přístup Red Hat aktualizace infrastruktury (RHUI) nasazené v Azure jsou registrované virtuálních počítačích vytvořených z na vyžádání Red Hat Enterprise Linux (RHEL) bitové kopie, které v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="45d22-104">Virtual machines created from the on-demand Red Hat Enterprise Linux (RHEL) images available in Azure Marketplace are registered to access the Red Hat Update Infrastructure (RHUI) deployed in Azure.</span></span>  <span data-ttu-id="45d22-105">Instance RHEL na vyžádání mají přístup do úložiště místní yum a může přijímat přírůstkové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="45d22-105">The on-demand RHEL instances have access to a regional yum repository and able to receive incremental updates.</span></span>

<span data-ttu-id="45d22-106">Seznamu yum úložiště, který se spravuje nástrojem RHUI, je v instanci RHEL nakonfigurovali během zřizování.</span><span class="sxs-lookup"><span data-stu-id="45d22-106">The yum repository list, which is managed by RHUI, is configured in your RHEL instance during provisioning.</span></span> <span data-ttu-id="45d22-107">Nemusíte provádět žádnou další konfiguraci - spustit `yum update` po vaší instance RHEL je připraven k získat nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="45d22-107">You don't need to do any additional configuration - run `yum update` after your RHEL instance is ready to get the latest updates.</span></span>

> [!NOTE]
> <span data-ttu-id="45d22-108">V září 2016 jsme nasadili aktualizované RHUI Azure a v ledna 2017 spuštění postupné vypnutí starší RHUI Azure.</span><span class="sxs-lookup"><span data-stu-id="45d22-108">In September 2016 we deployed an updated Azure RHUI and in January 2017 we started phased shutdown of the older Azure RHUI.</span></span> <span data-ttu-id="45d22-109">Pokud používáte Image RHEL (nebo jejich snímky) od září 2016 nebo novější – pravděpodobně není třeba žádné akce.</span><span class="sxs-lookup"><span data-stu-id="45d22-109">If you have been using the RHEL images (or their snapshots) from September 2016 or later - likely no action is required.</span></span> <span data-ttu-id="45d22-110">Pokud však máte starší snímky nebo virtuální počítače, je třeba aktualizovat pro bez přerušení přístup k Azure RHUI jejich konfigurace.</span><span class="sxs-lookup"><span data-stu-id="45d22-110">If, however, you have older snapshots/VMs, their configuration needs to be updated for uninterrupted access to the Azure RHUI.</span></span>
> 

## <a name="rhui-azure-infrastructure-update"></a><span data-ttu-id="45d22-111">Aktualizace RHUI infrastrukturu Azure</span><span class="sxs-lookup"><span data-stu-id="45d22-111">RHUI Azure Infrastructure Update</span></span>
<span data-ttu-id="45d22-112">Od září 2016 Azure má novou sadu serverů Red Hat aktualizace infrastruktury (RHUI).</span><span class="sxs-lookup"><span data-stu-id="45d22-112">As of September 2016, Azure has a new set of Red Hat Update Infrastructure (RHUI) servers.</span></span> <span data-ttu-id="45d22-113">Tyto servery se nasadí s [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) tak, aby jeden koncový bod (rhui 1.microsoft.cz) mohou být využívána žádné virtuální počítače bez ohledu na oblast.</span><span class="sxs-lookup"><span data-stu-id="45d22-113">These servers are deployed with [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) so that a single endpoint (rhui-1.microsoft.com) can be used by any VM regardless of region.</span></span> <span data-ttu-id="45d22-114">Nový Image průběžné platby srážek daně (ze RHEL MZDY) v Azure Marketplace (verze ze září 2016 nebo novější) přejděte na nové servery Azure RHUI a nevyžadují žádné další akce.</span><span class="sxs-lookup"><span data-stu-id="45d22-114">The new RHEL Pay-As-You-Go (PAYG) images in the Azure Marketplace (versions dated September 2016 or later) point to the new Azure RHUI servers and do not require any additional action.</span></span>

### <a name="determine-if-action-is-required"></a><span data-ttu-id="45d22-115">Určit, zda je požadovaná akce</span><span class="sxs-lookup"><span data-stu-id="45d22-115">Determine if action is required</span></span>
<span data-ttu-id="45d22-116">Pokud máte potíže s připojením k Azure RHUI ze svého virtuálního počítače Azure RHEL srážek daně ze MZDY, postupujte podle těchto kroků</span><span class="sxs-lookup"><span data-stu-id="45d22-116">If you are experiencing problems connecting to Azure RHUI from your Azure RHEL PAYG VM, follow these steps</span></span>
1. <span data-ttu-id="45d22-117">Zkontrolujte konfiguraci virtuálních počítačů pro koncový bod Azure RHUI</span><span class="sxs-lookup"><span data-stu-id="45d22-117">Inspect VM configuration for Azure RHUI endpoint</span></span>

    <span data-ttu-id="45d22-118">Zkontrolujte, zda `/etc/yum.repos.d/rh-cloud.repo` soubor obsahuje odkaz na `rhui-[1-3].microsoft.com` v baseurl z `[rhui-microsoft-azure-rhel*]` část souboru.</span><span class="sxs-lookup"><span data-stu-id="45d22-118">Check if `/etc/yum.repos.d/rh-cloud.repo` file contains reference to `rhui-[1-3].microsoft.com` in baseurl of `[rhui-microsoft-azure-rhel*]` section of the file.</span></span> <span data-ttu-id="45d22-119">Pokud se jedná - používáte novou RHUI Azure.</span><span class="sxs-lookup"><span data-stu-id="45d22-119">If it is - you are using the new Azure RHUI.</span></span>

    <span data-ttu-id="45d22-120">Pokud ho odkazující na umístění s následující vzor `mirrorlist.*cds[1-4].cloudapp.net` -je vyžadována aktualizace konfigurace.</span><span class="sxs-lookup"><span data-stu-id="45d22-120">If it pointing to a location with the following pattern `mirrorlist.*cds[1-4].cloudapp.net` - the configuration update is required.</span></span>

    <span data-ttu-id="45d22-121">Pokud používáte novou konfiguraci a pořád nemůžete připojit k Azure RHUI - souboru případu podpory společnosti Microsoft nebo Red Hat.</span><span class="sxs-lookup"><span data-stu-id="45d22-121">If you are using the new configuration and still cannot connect to Azure RHUI - file a support case with Microsoft or Red Hat.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45d22-122">Přístup k Azure hostovaná RHUI je omezený na virtuálních počítačích v rámci [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="45d22-122">Access to Azure-hosted RHUI is limited to the VMs within [Microsoft Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
    > 

2. <span data-ttu-id="45d22-123">Pokud původní RHUI Azure je stále k dispozici, když provedete tuto kontrolu a chcete automaticky aktualizovat konfiguraci, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="45d22-123">If the old Azure RHUI is still available when you do this check and you would like to automatically update the configuration, execute the following command:</span></span>

    <span data-ttu-id="45d22-124">`sudo yum update RHEL6`nebo `sudo yum update RHEL7` v závislosti na verzi rodiny RHEL.</span><span class="sxs-lookup"><span data-stu-id="45d22-124">`sudo yum update RHEL6` or `sudo yum update RHEL7` depending on the RHEL family version.</span></span>

3. <span data-ttu-id="45d22-125">Pokud už můžete připojit k původní RHUI Azure, postupujte podle ruční kroků popsaných v další části.</span><span class="sxs-lookup"><span data-stu-id="45d22-125">If you can no longer connect to the old Azure RHUI, follow the manual steps described in the next section.</span></span>

4. <span data-ttu-id="45d22-126">Zajistěte, aby se aktualizovat konfiguraci na zdroj bitové kopie/snímku vliv na zřizování virtuálních počítačů z.</span><span class="sxs-lookup"><span data-stu-id="45d22-126">Make sure to update the configuration on the source image/snapshot affected VM was provisioned from.</span></span>

### <a name="phased-shutdown-of-the-old-azure-rhui"></a><span data-ttu-id="45d22-127">Postupné vypnutí staré RHUI Azure</span><span class="sxs-lookup"><span data-stu-id="45d22-127">Phased shutdown of the old Azure RHUI</span></span>
<span data-ttu-id="45d22-128">Při vypnutí staré RHUI Azure jsme omezit přístup k němu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="45d22-128">During the shutdown of the old Azure RHUI we restrict access to it as follows:</span></span>

1. <span data-ttu-id="45d22-129">Další omezení přístupu (ACL) k nastavení IP adres, které jsou již k němu připojuje.</span><span class="sxs-lookup"><span data-stu-id="45d22-129">Further restrict access (ACL) to set of IP addresses that are already connecting to it.</span></span> <span data-ttu-id="45d22-130">Možné vedlejší účinky: Pokud budete pokračovat, pomocí staré RHUI Azure – nové virtuální počítače nemusí být možné se připojit k němu.</span><span class="sxs-lookup"><span data-stu-id="45d22-130">Possible side-effects: if you continue using the old Azure RHUI - your new VMs may not be able to connect to it.</span></span> <span data-ttu-id="45d22-131">RHEL virtuálních počítačů pomocí dynamické IP adresy, které projít vypnutí nebo navrácení nebo počáteční sekvence obdržet nových IP a proto může spustit i nedaří připojit k původní RHUI Azure</span><span class="sxs-lookup"><span data-stu-id="45d22-131">RHEL VMs with dynamic IPs that go through shutdown/deallocate/start sequence may receive new IP and hence also could start failing to connect to the old Azure RHUI</span></span>

2. <span data-ttu-id="45d22-132">Vypnutí zrcadlení doručování obsahu serverů.</span><span class="sxs-lookup"><span data-stu-id="45d22-132">Shutdown of mirror content delivery servers.</span></span> <span data-ttu-id="45d22-133">Možné vedlejší účinky: Jak jsme vypnout další CDSes se může zobrazit již `yum update` obsluhy čas, další časové limity až do bodu, pokud už můžete připojit k původní RHUI Azure.</span><span class="sxs-lookup"><span data-stu-id="45d22-133">Possible side-effects: as we shut down more CDSes you may see longer `yum update` servicing time, more timeouts up until the point when you can no longer connect to the old Azure RHUI.</span></span>

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a><span data-ttu-id="45d22-134">Jsou IP adresy pro nové servery RHUI doručování obsahu</span><span class="sxs-lookup"><span data-stu-id="45d22-134">The IPs for the new RHUI content delivery servers are</span></span>
<span data-ttu-id="45d22-135">Pokud používáte konfiguraci sítě a dál omezit přístup z virtuálních počítačů systému RHEL srážek daně ze MZDY, ujistěte se, že následující adresy IP jsou povoleny pro `yum update` pro práci v závislosti na prostředí, které jsou v.</span><span class="sxs-lookup"><span data-stu-id="45d22-135">If you are using network configuration to further restrict access from RHEL PAYG VMs, make sure the following IPs are allowed for `yum update` to work depending on the environment you are in.</span></span> 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a><span data-ttu-id="45d22-136">Postup ruční aktualizace pomocí nových serverů Azure RHUI</span><span class="sxs-lookup"><span data-stu-id="45d22-136">Manual update procedure to use the new Azure RHUI servers</span></span>
<span data-ttu-id="45d22-137">Stažení (prostřednictvím curl) podpis veřejného klíče</span><span class="sxs-lookup"><span data-stu-id="45d22-137">Download (via curl) the public key signature</span></span>

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

<span data-ttu-id="45d22-138">Ověřte stažené klíč</span><span class="sxs-lookup"><span data-stu-id="45d22-138">Verify the downloaded key</span></span>

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

<span data-ttu-id="45d22-139">Zkontrolujte výstup, ověřte `keyid` a `user ID packet`:</span><span class="sxs-lookup"><span data-stu-id="45d22-139">Check the output, verify `keyid` and `user ID packet`:</span></span>

```bash
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

<span data-ttu-id="45d22-140">Nainstalujte veřejný klíč</span><span class="sxs-lookup"><span data-stu-id="45d22-140">Install the public key</span></span>

```bash
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

<span data-ttu-id="45d22-141">Stáhnout, ověření a instalace klienta ot. / min</span><span class="sxs-lookup"><span data-stu-id="45d22-141">Download, Verify, and Install Client RPM</span></span>

<span data-ttu-id="45d22-142">Stáhnout: Pro RHEL 6</span><span class="sxs-lookup"><span data-stu-id="45d22-142">Download: For RHEL 6</span></span>

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

<span data-ttu-id="45d22-143">Pro RHEL 7</span><span class="sxs-lookup"><span data-stu-id="45d22-143">For RHEL 7</span></span>

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

<span data-ttu-id="45d22-144">Ověřte:</span><span class="sxs-lookup"><span data-stu-id="45d22-144">Verify:</span></span>

```bash
rpm -Kv azureclient.rpm
```

<span data-ttu-id="45d22-145">Zkontrolujte, že podpisu balíčku ve výstupu je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="45d22-145">Check in output that signature of the package is OK</span></span>

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

<span data-ttu-id="45d22-146">Nainstalujte ot. / min</span><span class="sxs-lookup"><span data-stu-id="45d22-146">Install the RPM</span></span>

```bash
sudo rpm -U azureclient.rpm
```

<span data-ttu-id="45d22-147">Po dokončení ověřte, že vám přístup Azure RHUI formuláře virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="45d22-147">Upon completion, verify that you can access Azure RHUI form the VM</span></span>

### <a name="all-in-one-script-for-automating-the-preceding-task"></a><span data-ttu-id="45d22-148">Vše v jednom skriptu pro automatizaci předchozí úlohy</span><span class="sxs-lookup"><span data-stu-id="45d22-148">All-in-one script for automating the preceding task</span></span>
<span data-ttu-id="45d22-149">Pomocí následujícího skriptu pro automatizaci úloh aktualizace ovlivněné virtuální počítače na nové servery Azure RHUI podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="45d22-149">Use the following script as needed to automate the task of updating affected VMs to the new Azure RHUI servers.</span></span>

```sh
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a><span data-ttu-id="45d22-150">Přehled RHUI</span><span class="sxs-lookup"><span data-stu-id="45d22-150">RHUI overview</span></span>
<span data-ttu-id="45d22-151">[Red Hat aktualizace infrastruktury](https://access.redhat.com/products/red-hat-update-infrastructure) nabízí vysoce škálovatelné řešení pro správu obsahu yum úložiště pro Red Hat Enterprise Linux cloudové instance, které hostují poskytovatelé certifikované Red Hat cloudů.</span><span class="sxs-lookup"><span data-stu-id="45d22-151">[Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) offers a highly scalable solution to manage yum repository content for Red Hat Enterprise Linux cloud instances that are hosted by Red Hat-certified cloud providers.</span></span> <span data-ttu-id="45d22-152">Na základě nadřazeného projektu buničiny si RHUI poskytovatelům cloudových místně zrcadlení hostované Red Hat úložiště obsahu, vytvořte vlastní úložiště s vlastní obsah a zpřístupnit tyto úložiště velkou skupinu koncovým uživatelům prostřednictvím Vyrovnávání zatížení systém doručování obsahu.</span><span class="sxs-lookup"><span data-stu-id="45d22-152">Based on the upstream Pulp project, RHUI allows cloud providers to locally mirror Red Hat-hosted repository content, create custom repositories with their own content, and make those repositories available to a large group of end users through a load-balanced content delivery system.</span></span>

## <a name="regions-where-rhui-is-available"></a><span data-ttu-id="45d22-153">Oblasti, kde je k dispozici RHUI</span><span class="sxs-lookup"><span data-stu-id="45d22-153">Regions where RHUI is available</span></span>
<span data-ttu-id="45d22-154">RHUI je k dispozici ve všech oblastech, kde jsou k dispozici na vyžádání RHEL bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="45d22-154">RHUI is available in all regions where RHEL on-demand images are available.</span></span> <span data-ttu-id="45d22-155">Aktuálně zahrnuje všechny veřejné oblasti uvedené na [řídicí panel Azure stav](https://azure.microsoft.com/status/) stránky, oblasti Azure US Government a Azure v Německu.</span><span class="sxs-lookup"><span data-stu-id="45d22-155">It currently includes all public regions listed on the [Azure status dashboard](https://azure.microsoft.com/status/) page, Azure US Government and Azure Germany regions.</span></span> <span data-ttu-id="45d22-156">RHUI přístup pro virtuální počítače zřízené z bitové kopie na vyžádání RHEL je součástí jejich cena.</span><span class="sxs-lookup"><span data-stu-id="45d22-156">RHUI access for VMs provisioned from RHEL on-demand images is included in their price.</span></span> <span data-ttu-id="45d22-157">Dodatečné cloudové místní/national dostupnosti bude aktualizovat, protože jsme rozbalte RHEL na vyžádání dostupnosti v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="45d22-157">Additional regional/national cloud availability will be updated as we expand RHEL on-demand availability in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="45d22-158">Přístup k Azure hostovaná RHUI je omezený na virtuálních počítačích v rámci [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="45d22-158">Access to Azure-hosted RHUI is limited to the VMs within [Microsoft Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
> 
> 

## <a name="get-updates-from-another-update-repository"></a><span data-ttu-id="45d22-159">Získat aktualizace z jiného úložiště aktualizací</span><span class="sxs-lookup"><span data-stu-id="45d22-159">Get updates from another update repository</span></span>
<span data-ttu-id="45d22-160">Pokud potřebujete získat aktualizace z různých aktualizace úložiště (namísto Azure hostovaná RHUI), je třeba nejprve zrušit registraci z RHUI vaše instance.</span><span class="sxs-lookup"><span data-stu-id="45d22-160">If you need to get updates from a different update repository (instead of Azure-hosted RHUI), first you need to unregister your instances from RHUI.</span></span> <span data-ttu-id="45d22-161">Pak budete muset zaregistrovat je znovu s infrastruktury požadované aktualizace (například Red Hat satelitní nebo Red Hat zákazníka portál CDN).</span><span class="sxs-lookup"><span data-stu-id="45d22-161">Then you need to re-register them with the desired update infrastructure (such as Red Hat Satellite or Red Hat Customer Portal CDN).</span></span> <span data-ttu-id="45d22-162">Bude nutné vhodné Red Hat odběry pro tyto služby a registraci [Red Hat přístup do cloudu v Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).</span><span class="sxs-lookup"><span data-stu-id="45d22-162">You will need appropriate Red Hat subscriptions for these services and registration for [Red Hat Cloud Access in Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).</span></span>

<span data-ttu-id="45d22-163">Zrušení registrace RHUI a znovu zaregistrujte k infrastruktuře aktualizace postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="45d22-163">To unregister RHUI and reregister to your update infrastructure follow these steps:</span></span>

1. <span data-ttu-id="45d22-164">Upravit /etc/yum.repos.d/rh-cloud.repo a změňte všechny `enabled=1` k `enabled=0`.</span><span class="sxs-lookup"><span data-stu-id="45d22-164">Edit /etc/yum.repos.d/rh-cloud.repo and change all `enabled=1` to `enabled=0`.</span></span> <span data-ttu-id="45d22-165">Například:</span><span class="sxs-lookup"><span data-stu-id="45d22-165">For example:</span></span>
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. <span data-ttu-id="45d22-166">Upravte /etc/yum/pluginconf.d/rhnplugin.conf a změňte `enabled=0` k `enabled=1`.</span><span class="sxs-lookup"><span data-stu-id="45d22-166">Edit /etc/yum/pluginconf.d/rhnplugin.conf and change `enabled=0` to `enabled=1`.</span></span>
3. <span data-ttu-id="45d22-167">Potom proveďte registraci s požadovanou infrastruktury, například zákaznický portál Red Hat.</span><span class="sxs-lookup"><span data-stu-id="45d22-167">Then register with the desired infrastructure, such as Red Hat Customer Portal.</span></span> <span data-ttu-id="45d22-168">Postupujte podle Red Hat řešení Průvodce na [způsob registrace a přihlášení k odběru systému na zákaznický portál Red Hat](https://access.redhat.com/solutions/253273).</span><span class="sxs-lookup"><span data-stu-id="45d22-168">Follow Red Hat solution guide on [how to register and subscribe a system to the Red Hat Customer Portal](https://access.redhat.com/solutions/253273).</span></span>

> [!NOTE]
> <span data-ttu-id="45d22-169">Přístup k Azure hostovaná RHUI je součástí ceny image průběžné platby srážek daně (ze RHEL MZDY).</span><span class="sxs-lookup"><span data-stu-id="45d22-169">Access to the Azure-hosted RHUI is included in the RHEL Pay-As-You-Go (PAYG) image price.</span></span> <span data-ttu-id="45d22-170">Zrušení registrace virtuálních počítačů systému RHEL srážek daně ze MZDY z Azure hostovaná RHUI není převodem virtuálního počítače do typu Bring-Your-vlastní-licenci (BYOL) virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="45d22-170">Unregistering a PAYG RHEL VM from the Azure-hosted RHUI does not convert the virtual machine into Bring-Your-Own-License (BYOL) type VM.</span></span> <span data-ttu-id="45d22-171">Když si zaregistrujete stejného virtuálního počítače s jiný zdroj aktualizací, vám může být by docházelo k dvojité poplatky: nejdřív čas pro Azure RHEL softwaru poplatek a podruhé pro odběry Red Hat.</span><span class="sxs-lookup"><span data-stu-id="45d22-171">If you register the same VM with another source of updates you may be incurring double charges: first time for Azure RHEL software fee, and the second time for Red Hat subscription(s).</span></span> 
> 
> <span data-ttu-id="45d22-172">Pokud potřebujete konzistentně jiné než použití aktualizace infrastruktury Azure hostovaná RHUI, zvažte vytvoření a nasazení vlastních bitových kopií (BYOL-type), jak je popsáno v [vytvořit a nahrát Red Hat virtuálním počítačem Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) článku.</span><span class="sxs-lookup"><span data-stu-id="45d22-172">If you consistently need to use an update infrastructure other than Azure-hosted RHUI consider creating and deploying your own (BYOL-type) images as described in [Create and Upload Red Hat-based virtual machine for Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) article.</span></span>
> 

## <a name="next-steps"></a><span data-ttu-id="45d22-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45d22-173">Next steps</span></span>
<span data-ttu-id="45d22-174">Chcete-li vytvořit virtuální počítač Red Hat Enterprise Linux z Azure Marketplace s průběžnými platbami image a využívání Azure hostovaná RHUI přejít na [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).</span><span class="sxs-lookup"><span data-stu-id="45d22-174">To create a Red Hat Enterprise Linux VM from Azure Marketplace Pay-As-You-Go image and leverage Azure-hosted RHUI go to [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).</span></span> <span data-ttu-id="45d22-175">Bude moct používat `yum update` v instanci RHEL bez jakékoli další nastavení.</span><span class="sxs-lookup"><span data-stu-id="45d22-175">You will be able to use `yum update` in your RHEL instance without any additional setup.</span></span>

