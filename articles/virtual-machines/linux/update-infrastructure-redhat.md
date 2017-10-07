---
title: aaaRed Hat aktualizace infrastruktury (RHUI) | Microsoft Docs
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
ms.openlocfilehash: cc244857104b25e4e61862c518db77e915e137ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a><span data-ttu-id="f6053-103">Red Hat aktualizace infrastruktury (RHUI) pro na vyžádání Red Hat Enterprise Linux virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="f6053-103">Red Hat Update Infrastructure (RHUI) for on-demand Red Hat Enterprise Linux VMs in Azure</span></span>
<span data-ttu-id="f6053-104">Virtuální počítače vytvořené z hello na vyžádání Red Hat Enterprise Linux (RHEL) bitové kopie k dispozici v Azure Marketplace jsou registrované tooaccess hello Red Hat aktualizace infrastruktury (RHUI) nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="f6053-104">Virtual machines created from hello on-demand Red Hat Enterprise Linux (RHEL) images available in Azure Marketplace are registered tooaccess hello Red Hat Update Infrastructure (RHUI) deployed in Azure.</span></span>  <span data-ttu-id="f6053-105">instance RHEL Hello na vyžádání, mají přístup tooa regionální yum úložiště a možnost tooreceive přírůstkové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f6053-105">hello on-demand RHEL instances have access tooa regional yum repository and able tooreceive incremental updates.</span></span>

<span data-ttu-id="f6053-106">Hello yum úložiště seznam, který spravuje RHUI, je v instanci RHEL nakonfigurovali během zřizování.</span><span class="sxs-lookup"><span data-stu-id="f6053-106">hello yum repository list, which is managed by RHUI, is configured in your RHEL instance during provisioning.</span></span> <span data-ttu-id="f6053-107">Nepotřebujete toodo žádné další konfigurace – spustit `yum update` po vaší instance RHEL je připraven tooget hello nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f6053-107">You don't need toodo any additional configuration - run `yum update` after your RHEL instance is ready tooget hello latest updates.</span></span>

> [!NOTE]
> <span data-ttu-id="f6053-108">V září 2016 jsme nasadili aktualizované RHUI Azure a v ledna 2017 spuštění postupné vypnutí hello starší RHUI Azure.</span><span class="sxs-lookup"><span data-stu-id="f6053-108">In September 2016 we deployed an updated Azure RHUI and in January 2017 we started phased shutdown of hello older Azure RHUI.</span></span> <span data-ttu-id="f6053-109">Pokud používáte Image RHEL hello (nebo jejich snímky) od září 2016 nebo novější – pravděpodobně není třeba žádné akce.</span><span class="sxs-lookup"><span data-stu-id="f6053-109">If you have been using hello RHEL images (or their snapshots) from September 2016 or later - likely no action is required.</span></span> <span data-ttu-id="f6053-110">Pokud však máte starší snímky nebo virtuální počítače, je jejich konfiguraci toobe aktualizované bez přerušení přístup toohello Azure RHUI.</span><span class="sxs-lookup"><span data-stu-id="f6053-110">If, however, you have older snapshots/VMs, their configuration needs toobe updated for uninterrupted access toohello Azure RHUI.</span></span>
> 

## <a name="rhui-azure-infrastructure-update"></a><span data-ttu-id="f6053-111">Aktualizace RHUI infrastrukturu Azure</span><span class="sxs-lookup"><span data-stu-id="f6053-111">RHUI Azure Infrastructure Update</span></span>
<span data-ttu-id="f6053-112">Od září 2016 Azure má novou sadu serverů Red Hat aktualizace infrastruktury (RHUI).</span><span class="sxs-lookup"><span data-stu-id="f6053-112">As of September 2016, Azure has a new set of Red Hat Update Infrastructure (RHUI) servers.</span></span> <span data-ttu-id="f6053-113">Tyto servery se nasadí s [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) tak, aby jeden koncový bod (rhui 1.microsoft.cz) mohou být využívána žádné virtuální počítače bez ohledu na oblast.</span><span class="sxs-lookup"><span data-stu-id="f6053-113">These servers are deployed with [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) so that a single endpoint (rhui-1.microsoft.com) can be used by any VM regardless of region.</span></span> <span data-ttu-id="f6053-114">Hello nové obrázků průběžné platby srážek daně (ze RHEL MZDY) v Azure Marketplace (verze ze září 2016 nebo novější) hello bodu toohello nové Azure RHUI servery a nevyžadují žádné další akce.</span><span class="sxs-lookup"><span data-stu-id="f6053-114">hello new RHEL Pay-As-You-Go (PAYG) images in hello Azure Marketplace (versions dated September 2016 or later) point toohello new Azure RHUI servers and do not require any additional action.</span></span>

### <a name="determine-if-action-is-required"></a><span data-ttu-id="f6053-115">Určit, zda je požadovaná akce</span><span class="sxs-lookup"><span data-stu-id="f6053-115">Determine if action is required</span></span>
<span data-ttu-id="f6053-116">Pokud máte potíže s připojením tooAzure RHUI ze svého virtuálního počítače Azure RHEL srážek daně ze MZDY, postupujte podle těchto kroků</span><span class="sxs-lookup"><span data-stu-id="f6053-116">If you are experiencing problems connecting tooAzure RHUI from your Azure RHEL PAYG VM, follow these steps</span></span>
1. <span data-ttu-id="f6053-117">Zkontrolujte konfiguraci virtuálních počítačů pro koncový bod Azure RHUI</span><span class="sxs-lookup"><span data-stu-id="f6053-117">Inspect VM configuration for Azure RHUI endpoint</span></span>

    <span data-ttu-id="f6053-118">Zkontrolujte, zda `/etc/yum.repos.d/rh-cloud.repo` soubor obsahuje odkaz na příliš`rhui-[1-3].microsoft.com` v baseurl z `[rhui-microsoft-azure-rhel*]` hello souboru.</span><span class="sxs-lookup"><span data-stu-id="f6053-118">Check if `/etc/yum.repos.d/rh-cloud.repo` file contains reference too`rhui-[1-3].microsoft.com` in baseurl of `[rhui-microsoft-azure-rhel*]` section of hello file.</span></span> <span data-ttu-id="f6053-119">Pokud je – používáte hello nové RHUI Azure.</span><span class="sxs-lookup"><span data-stu-id="f6053-119">If it is - you are using hello new Azure RHUI.</span></span>

    <span data-ttu-id="f6053-120">Pokud ho odkazující tooa umístění s hello následující vzor `mirrorlist.*cds[1-4].cloudapp.net` -je požadována aktualizace konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="f6053-120">If it pointing tooa location with hello following pattern `mirrorlist.*cds[1-4].cloudapp.net` - hello configuration update is required.</span></span>

    <span data-ttu-id="f6053-121">Pokud používáte novou konfiguraci hello a pořád nemůžete připojit tooAzure RHUI - souboru případu podpory společnosti Microsoft nebo Red Hat.</span><span class="sxs-lookup"><span data-stu-id="f6053-121">If you are using hello new configuration and still cannot connect tooAzure RHUI - file a support case with Microsoft or Red Hat.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6053-122">Hostované tooAzure RHUI přístup je omezená toohello virtuálních počítačů v rámci [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="f6053-122">Access tooAzure-hosted RHUI is limited toohello VMs within [Microsoft Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
    > 

2. <span data-ttu-id="f6053-123">Pokud hello staré RHUI Azure je stále k dispozici při provedení této kontroly a chcete tooautomatically aktualizace hello konfigurace, spouštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f6053-123">If hello old Azure RHUI is still available when you do this check and you would like tooautomatically update hello configuration, execute hello following command:</span></span>

    <span data-ttu-id="f6053-124">`sudo yum update RHEL6`nebo `sudo yum update RHEL7` v závislosti na verzi rodiny RHEL hello.</span><span class="sxs-lookup"><span data-stu-id="f6053-124">`sudo yum update RHEL6` or `sudo yum update RHEL7` depending on hello RHEL family version.</span></span>

3. <span data-ttu-id="f6053-125">Pokud už se můžete připojit toohello staré RHUI Azure, postupujte podle hello ruční kroky popsané v další části hello.</span><span class="sxs-lookup"><span data-stu-id="f6053-125">If you can no longer connect toohello old Azure RHUI, follow hello manual steps described in hello next section.</span></span>

4. <span data-ttu-id="f6053-126">Zajistěte, aby vliv na konfiguraci hello tooupdate na hello zdroj bitové kopie/snímku virtuálního počítače byla zajištěného z.</span><span class="sxs-lookup"><span data-stu-id="f6053-126">Make sure tooupdate hello configuration on hello source image/snapshot affected VM was provisioned from.</span></span>

### <a name="phased-shutdown-of-hello-old-azure-rhui"></a><span data-ttu-id="f6053-127">Postupné vypnutí hello staré RHUI Azure</span><span class="sxs-lookup"><span data-stu-id="f6053-127">Phased shutdown of hello old Azure RHUI</span></span>
<span data-ttu-id="f6053-128">Při vypnutí hello hello staré Azure RHUI jsme omezit přístup tooit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f6053-128">During hello shutdown of hello old Azure RHUI we restrict access tooit as follows:</span></span>

1. <span data-ttu-id="f6053-129">Další omezení přístupu (ACL) tooset IP adres, které jsou již připojení tooit.</span><span class="sxs-lookup"><span data-stu-id="f6053-129">Further restrict access (ACL) tooset of IP addresses that are already connecting tooit.</span></span> <span data-ttu-id="f6053-130">Možné vedlejší účinky: Pokud budete pokračovat, pomocí hello staré RHUI Azure – nové virtuální počítače nemusí být možné tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="f6053-130">Possible side-effects: if you continue using hello old Azure RHUI - your new VMs may not be able tooconnect tooit.</span></span> <span data-ttu-id="f6053-131">RHEL virtuálních počítačů pomocí dynamické IP adresy, které projít vypnutí nebo navrácení nebo počáteční sekvence obdržet nových IP a proto může spustit také selháním tooconnect toohello staré RHUI Azure</span><span class="sxs-lookup"><span data-stu-id="f6053-131">RHEL VMs with dynamic IPs that go through shutdown/deallocate/start sequence may receive new IP and hence also could start failing tooconnect toohello old Azure RHUI</span></span>

2. <span data-ttu-id="f6053-132">Vypnutí zrcadlení doručování obsahu serverů.</span><span class="sxs-lookup"><span data-stu-id="f6053-132">Shutdown of mirror content delivery servers.</span></span> <span data-ttu-id="f6053-133">Možné vedlejší účinky: Jak jsme vypnout další CDSes se může zobrazit již `yum update` obsluhy čas další časové limity až po hello bodu, když už se můžete připojit toohello staré RHUI Azure.</span><span class="sxs-lookup"><span data-stu-id="f6053-133">Possible side-effects: as we shut down more CDSes you may see longer `yum update` servicing time, more timeouts up until hello point when you can no longer connect toohello old Azure RHUI.</span></span>

### <a name="hello-ips-for-hello-new-rhui-content-delivery-servers-are"></a><span data-ttu-id="f6053-134">jsou Hello IP adresy pro hello nových serverů RHUI doručování obsahu</span><span class="sxs-lookup"><span data-stu-id="f6053-134">hello IPs for hello new RHUI content delivery servers are</span></span>
<span data-ttu-id="f6053-135">Pokud používáte síťové konfigurace toofurther omezit přístup z virtuálních počítačů systému RHEL srážek daně ze MZDY, zajistěte, aby hello následující IP adresy jsou povoleny pro `yum update` toowork v závislosti na prostředí hello v.</span><span class="sxs-lookup"><span data-stu-id="f6053-135">If you are using network configuration toofurther restrict access from RHEL PAYG VMs, make sure hello following IPs are allowed for `yum update` toowork depending on hello environment you are in.</span></span> 

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

### <a name="manual-update-procedure-toouse-hello-new-azure-rhui-servers"></a><span data-ttu-id="f6053-136">Ruční aktualizace postupu toouse hello nové Azure RHUI servery</span><span class="sxs-lookup"><span data-stu-id="f6053-136">Manual update procedure toouse hello new Azure RHUI servers</span></span>
<span data-ttu-id="f6053-137">Stažení (prostřednictvím curl) hello veřejný klíč podpisu</span><span class="sxs-lookup"><span data-stu-id="f6053-137">Download (via curl) hello public key signature</span></span>

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

<span data-ttu-id="f6053-138">Ověřte hello stáhli klíč</span><span class="sxs-lookup"><span data-stu-id="f6053-138">Verify hello downloaded key</span></span>

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

<span data-ttu-id="f6053-139">Zkontrolujte výstup hello, ověřte `keyid` a `user ID packet`:</span><span class="sxs-lookup"><span data-stu-id="f6053-139">Check hello output, verify `keyid` and `user ID packet`:</span></span>

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

<span data-ttu-id="f6053-140">Nainstalujte hello veřejný klíč</span><span class="sxs-lookup"><span data-stu-id="f6053-140">Install hello public key</span></span>

```bash
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

<span data-ttu-id="f6053-141">Stáhnout, ověření a instalace klienta ot. / min</span><span class="sxs-lookup"><span data-stu-id="f6053-141">Download, Verify, and Install Client RPM</span></span>

<span data-ttu-id="f6053-142">Stáhnout: Pro RHEL 6</span><span class="sxs-lookup"><span data-stu-id="f6053-142">Download: For RHEL 6</span></span>

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

<span data-ttu-id="f6053-143">Pro RHEL 7</span><span class="sxs-lookup"><span data-stu-id="f6053-143">For RHEL 7</span></span>

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

<span data-ttu-id="f6053-144">Ověřte:</span><span class="sxs-lookup"><span data-stu-id="f6053-144">Verify:</span></span>

```bash
rpm -Kv azureclient.rpm
```

<span data-ttu-id="f6053-145">Ve výstupu zkontrolujte, že podpisu hello balíčku je OK</span><span class="sxs-lookup"><span data-stu-id="f6053-145">Check in output that signature of hello package is OK</span></span>

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

<span data-ttu-id="f6053-146">Nainstalujte hello ot. / min</span><span class="sxs-lookup"><span data-stu-id="f6053-146">Install hello RPM</span></span>

```bash
sudo rpm -U azureclient.rpm
```

<span data-ttu-id="f6053-147">Po dokončení ověřte, že vám přístup Azure RHUI formuláře hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f6053-147">Upon completion, verify that you can access Azure RHUI form hello VM</span></span>

### <a name="all-in-one-script-for-automating-hello-preceding-task"></a><span data-ttu-id="f6053-148">Vše v jednom skriptu pro automatizaci hello předchozích úloh</span><span class="sxs-lookup"><span data-stu-id="f6053-148">All-in-one script for automating hello preceding task</span></span>
<span data-ttu-id="f6053-149">Použijte následující skript jako úloha hello potřebné tooautomate aktualizace ovlivněné virtuální počítače toohello nové Azure RHUI servery hello.</span><span class="sxs-lookup"><span data-stu-id="f6053-149">Use hello following script as needed tooautomate hello task of updating affected VMs toohello new Azure RHUI servers.</span></span>

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

## <a name="rhui-overview"></a><span data-ttu-id="f6053-150">Přehled RHUI</span><span class="sxs-lookup"><span data-stu-id="f6053-150">RHUI overview</span></span>
<span data-ttu-id="f6053-151">[Red Hat aktualizace infrastruktury](https://access.redhat.com/products/red-hat-update-infrastructure) nabízí vysoce škálovatelné řešení toomanage yum úložiště obsahu pro Red Hat Enterprise Linux cloudové instance, které hostují poskytovatelé certifikované Red Hat cloudů.</span><span class="sxs-lookup"><span data-stu-id="f6053-151">[Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) offers a highly scalable solution toomanage yum repository content for Red Hat Enterprise Linux cloud instances that are hosted by Red Hat-certified cloud providers.</span></span> <span data-ttu-id="f6053-152">Na základě hello nadřazeného buničiny projektu, RHUI poskytovatelům cloudových toolocally zrcadlení hostované Red Hat úložiště obsahu, vytvořte vlastní úložiště s vlastní obsah a nastavte tyto úložiště k dispozici tooa velkou skupinu koncovým uživatelům prostřednictvím vyrovnáváním zatížení systém doručování obsahu.</span><span class="sxs-lookup"><span data-stu-id="f6053-152">Based on hello upstream Pulp project, RHUI allows cloud providers toolocally mirror Red Hat-hosted repository content, create custom repositories with their own content, and make those repositories available tooa large group of end users through a load-balanced content delivery system.</span></span>

## <a name="regions-where-rhui-is-available"></a><span data-ttu-id="f6053-153">Oblasti, kde je k dispozici RHUI</span><span class="sxs-lookup"><span data-stu-id="f6053-153">Regions where RHUI is available</span></span>
<span data-ttu-id="f6053-154">RHUI je k dispozici ve všech oblastech, kde jsou k dispozici na vyžádání RHEL bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="f6053-154">RHUI is available in all regions where RHEL on-demand images are available.</span></span> <span data-ttu-id="f6053-155">Aktuálně zahrnuje všechny veřejné oblasti uvedené na hello [řídicí panel Azure stav](https://azure.microsoft.com/status/) stránky, oblasti Azure US Government a Azure v Německu.</span><span class="sxs-lookup"><span data-stu-id="f6053-155">It currently includes all public regions listed on hello [Azure status dashboard](https://azure.microsoft.com/status/) page, Azure US Government and Azure Germany regions.</span></span> <span data-ttu-id="f6053-156">RHUI přístup pro virtuální počítače zřízené z bitové kopie na vyžádání RHEL je součástí jejich cena.</span><span class="sxs-lookup"><span data-stu-id="f6053-156">RHUI access for VMs provisioned from RHEL on-demand images is included in their price.</span></span> <span data-ttu-id="f6053-157">Dodatečné cloudové místní/national dostupnosti bude aktualizovat, protože jsme rozbalte RHEL dostupnosti na vyžádání v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="f6053-157">Additional regional/national cloud availability will be updated as we expand RHEL on-demand availability in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="f6053-158">Hostované tooAzure RHUI přístup je omezená toohello virtuálních počítačů v rámci [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="f6053-158">Access tooAzure-hosted RHUI is limited toohello VMs within [Microsoft Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
> 
> 

## <a name="get-updates-from-another-update-repository"></a><span data-ttu-id="f6053-159">Získat aktualizace z jiného úložiště aktualizací</span><span class="sxs-lookup"><span data-stu-id="f6053-159">Get updates from another update repository</span></span>
<span data-ttu-id="f6053-160">Pokud potřebujete tooget aktualizace z různých aktualizace úložiště (namísto Azure hostovaná RHUI), je třeba nejprve toounregister z RHUI vaše instance.</span><span class="sxs-lookup"><span data-stu-id="f6053-160">If you need tooget updates from a different update repository (instead of Azure-hosted RHUI), first you need toounregister your instances from RHUI.</span></span> <span data-ttu-id="f6053-161">Pak je nutné zaregistrovat toore je hello požadované aktualizace infrastruktury (třeba Red Hat satelitní nebo Red Hat zákazníka portál CDN).</span><span class="sxs-lookup"><span data-stu-id="f6053-161">Then you need toore-register them with hello desired update infrastructure (such as Red Hat Satellite or Red Hat Customer Portal CDN).</span></span> <span data-ttu-id="f6053-162">Bude nutné vhodné Red Hat odběry pro tyto služby a registraci [Red Hat přístup do cloudu v Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).</span><span class="sxs-lookup"><span data-stu-id="f6053-162">You will need appropriate Red Hat subscriptions for these services and registration for [Red Hat Cloud Access in Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).</span></span>

<span data-ttu-id="f6053-163">toounregister RHUI a znovu zaregistrujte tooyour aktualizace infrastruktury pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="f6053-163">toounregister RHUI and reregister tooyour update infrastructure follow these steps:</span></span>

1. <span data-ttu-id="f6053-164">Upravit /etc/yum.repos.d/rh-cloud.repo a změňte všechny `enabled=1` příliš`enabled=0`.</span><span class="sxs-lookup"><span data-stu-id="f6053-164">Edit /etc/yum.repos.d/rh-cloud.repo and change all `enabled=1` too`enabled=0`.</span></span> <span data-ttu-id="f6053-165">Například:</span><span class="sxs-lookup"><span data-stu-id="f6053-165">For example:</span></span>
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. <span data-ttu-id="f6053-166">Upravte /etc/yum/pluginconf.d/rhnplugin.conf a změňte `enabled=0` příliš`enabled=1`.</span><span class="sxs-lookup"><span data-stu-id="f6053-166">Edit /etc/yum/pluginconf.d/rhnplugin.conf and change `enabled=0` too`enabled=1`.</span></span>
3. <span data-ttu-id="f6053-167">Potom proveďte registraci s požadovanou hello infrastruktury, například zákaznický portál Red Hat.</span><span class="sxs-lookup"><span data-stu-id="f6053-167">Then register with hello desired infrastructure, such as Red Hat Customer Portal.</span></span> <span data-ttu-id="f6053-168">Postupujte podle Red Hat řešení Průvodce na [jak tooregister a přihlásit se k systému toohello zákaznický portál Red Hat](https://access.redhat.com/solutions/253273).</span><span class="sxs-lookup"><span data-stu-id="f6053-168">Follow Red Hat solution guide on [how tooregister and subscribe a system toohello Red Hat Customer Portal](https://access.redhat.com/solutions/253273).</span></span>

> [!NOTE]
> <span data-ttu-id="f6053-169">Přístup toohello Azure hostovaná RHUI je součástí ceny image hello průběžné platby srážek daně (ze RHEL MZDY).</span><span class="sxs-lookup"><span data-stu-id="f6053-169">Access toohello Azure-hosted RHUI is included in hello RHEL Pay-As-You-Go (PAYG) image price.</span></span> <span data-ttu-id="f6053-170">Zrušení registrace virtuálních počítačů systému RHEL srážek daně ze MZDY z hello Azure hostovaná RHUI nepřevede hello virtuálního počítače do typu Bring-Your-vlastní-licenci (BYOL) virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f6053-170">Unregistering a PAYG RHEL VM from hello Azure-hosted RHUI does not convert hello virtual machine into Bring-Your-Own-License (BYOL) type VM.</span></span> <span data-ttu-id="f6053-171">Když si zaregistrujete hello stejného virtuálního počítače s jiný zdroj aktualizací, může být nabíhání poplatků za dvojité: nejdřív čas pro Azure RHEL softwaru poplatek a hello znovu pro Red Hat odběry.</span><span class="sxs-lookup"><span data-stu-id="f6053-171">If you register hello same VM with another source of updates you may be incurring double charges: first time for Azure RHEL software fee, and hello second time for Red Hat subscription(s).</span></span> 
> 
> <span data-ttu-id="f6053-172">Pokud potřebujete konzistentně toouse aktualizace infrastruktury než Azure hostovaná RHUI, zvažte vytvoření a nasazení vlastních bitových kopií (BYOL-type), jak je popsáno v [vytvořit a nahrát Red Hat virtuálním počítačem Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) článku.</span><span class="sxs-lookup"><span data-stu-id="f6053-172">If you consistently need toouse an update infrastructure other than Azure-hosted RHUI consider creating and deploying your own (BYOL-type) images as described in [Create and Upload Red Hat-based virtual machine for Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) article.</span></span>
> 

## <a name="next-steps"></a><span data-ttu-id="f6053-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6053-173">Next steps</span></span>
<span data-ttu-id="f6053-174">toocreate Red Hat Enterprise virtuálního počítače s Linuxem z Azure Marketplace s průběžnými platbami image a využívání Azure hostovaná RHUI přejděte příliš[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).</span><span class="sxs-lookup"><span data-stu-id="f6053-174">toocreate a Red Hat Enterprise Linux VM from Azure Marketplace Pay-As-You-Go image and leverage Azure-hosted RHUI go too[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).</span></span> <span data-ttu-id="f6053-175">Bude moct toouse `yum update` v instanci RHEL bez jakékoli další nastavení.</span><span class="sxs-lookup"><span data-stu-id="f6053-175">You will be able toouse `yum update` in your RHEL instance without any additional setup.</span></span>

