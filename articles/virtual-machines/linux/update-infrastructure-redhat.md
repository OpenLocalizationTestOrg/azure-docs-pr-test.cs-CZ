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
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat aktualizace infrastruktury (RHUI) pro na vyžádání Red Hat Enterprise Linux virtuálních počítačů v Azure
Virtuální počítače vytvořené z hello na vyžádání Red Hat Enterprise Linux (RHEL) bitové kopie k dispozici v Azure Marketplace jsou registrované tooaccess hello Red Hat aktualizace infrastruktury (RHUI) nasazené v Azure.  instance RHEL Hello na vyžádání, mají přístup tooa regionální yum úložiště a možnost tooreceive přírůstkové aktualizace.

Hello yum úložiště seznam, který spravuje RHUI, je v instanci RHEL nakonfigurovali během zřizování. Nepotřebujete toodo žádné další konfigurace – spustit `yum update` po vaší instance RHEL je připraven tooget hello nejnovější aktualizace.

> [!NOTE]
> V září 2016 jsme nasadili aktualizované RHUI Azure a v ledna 2017 spuštění postupné vypnutí hello starší RHUI Azure. Pokud používáte Image RHEL hello (nebo jejich snímky) od září 2016 nebo novější – pravděpodobně není třeba žádné akce. Pokud však máte starší snímky nebo virtuální počítače, je jejich konfiguraci toobe aktualizované bez přerušení přístup toohello Azure RHUI.
> 

## <a name="rhui-azure-infrastructure-update"></a>Aktualizace RHUI infrastrukturu Azure
Od září 2016 Azure má novou sadu serverů Red Hat aktualizace infrastruktury (RHUI). Tyto servery se nasadí s [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) tak, aby jeden koncový bod (rhui 1.microsoft.cz) mohou být využívána žádné virtuální počítače bez ohledu na oblast. Hello nové obrázků průběžné platby srážek daně (ze RHEL MZDY) v Azure Marketplace (verze ze září 2016 nebo novější) hello bodu toohello nové Azure RHUI servery a nevyžadují žádné další akce.

### <a name="determine-if-action-is-required"></a>Určit, zda je požadovaná akce
Pokud máte potíže s připojením tooAzure RHUI ze svého virtuálního počítače Azure RHEL srážek daně ze MZDY, postupujte podle těchto kroků
1. Zkontrolujte konfiguraci virtuálních počítačů pro koncový bod Azure RHUI

    Zkontrolujte, zda `/etc/yum.repos.d/rh-cloud.repo` soubor obsahuje odkaz na příliš`rhui-[1-3].microsoft.com` v baseurl z `[rhui-microsoft-azure-rhel*]` hello souboru. Pokud je – používáte hello nové RHUI Azure.

    Pokud ho odkazující tooa umístění s hello následující vzor `mirrorlist.*cds[1-4].cloudapp.net` -je požadována aktualizace konfigurace hello.

    Pokud používáte novou konfiguraci hello a pořád nemůžete připojit tooAzure RHUI - souboru případu podpory společnosti Microsoft nebo Red Hat.

    > [!NOTE]
    > Hostované tooAzure RHUI přístup je omezená toohello virtuálních počítačů v rámci [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).
    > 

2. Pokud hello staré RHUI Azure je stále k dispozici při provedení této kontroly a chcete tooautomatically aktualizace hello konfigurace, spouštění hello následující příkaz:

    `sudo yum update RHEL6`nebo `sudo yum update RHEL7` v závislosti na verzi rodiny RHEL hello.

3. Pokud už se můžete připojit toohello staré RHUI Azure, postupujte podle hello ruční kroky popsané v další části hello.

4. Zajistěte, aby vliv na konfiguraci hello tooupdate na hello zdroj bitové kopie/snímku virtuálního počítače byla zajištěného z.

### <a name="phased-shutdown-of-hello-old-azure-rhui"></a>Postupné vypnutí hello staré RHUI Azure
Při vypnutí hello hello staré Azure RHUI jsme omezit přístup tooit následujícím způsobem:

1. Další omezení přístupu (ACL) tooset IP adres, které jsou již připojení tooit. Možné vedlejší účinky: Pokud budete pokračovat, pomocí hello staré RHUI Azure – nové virtuální počítače nemusí být možné tooconnect tooit. RHEL virtuálních počítačů pomocí dynamické IP adresy, které projít vypnutí nebo navrácení nebo počáteční sekvence obdržet nových IP a proto může spustit také selháním tooconnect toohello staré RHUI Azure

2. Vypnutí zrcadlení doručování obsahu serverů. Možné vedlejší účinky: Jak jsme vypnout další CDSes se může zobrazit již `yum update` obsluhy čas další časové limity až po hello bodu, když už se můžete připojit toohello staré RHUI Azure.

### <a name="hello-ips-for-hello-new-rhui-content-delivery-servers-are"></a>jsou Hello IP adresy pro hello nových serverů RHUI doručování obsahu
Pokud používáte síťové konfigurace toofurther omezit přístup z virtuálních počítačů systému RHEL srážek daně ze MZDY, zajistěte, aby hello následující IP adresy jsou povoleny pro `yum update` toowork v závislosti na prostředí hello v. 

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

### <a name="manual-update-procedure-toouse-hello-new-azure-rhui-servers"></a>Ruční aktualizace postupu toouse hello nové Azure RHUI servery
Stažení (prostřednictvím curl) hello veřejný klíč podpisu

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Ověřte hello stáhli klíč

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Zkontrolujte výstup hello, ověřte `keyid` a `user ID packet`:

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

Nainstalujte hello veřejný klíč

```bash
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Stáhnout, ověření a instalace klienta ot. / min

Stáhnout: Pro RHEL 6

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Pro RHEL 7

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Ověřte:

```bash
rpm -Kv azureclient.rpm
```

Ve výstupu zkontrolujte, že podpisu hello balíčku je OK

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Nainstalujte hello ot. / min

```bash
sudo rpm -U azureclient.rpm
```

Po dokončení ověřte, že vám přístup Azure RHUI formuláře hello virtuálních počítačů

### <a name="all-in-one-script-for-automating-hello-preceding-task"></a>Vše v jednom skriptu pro automatizaci hello předchozích úloh
Použijte následující skript jako úloha hello potřebné tooautomate aktualizace ovlivněné virtuální počítače toohello nové Azure RHUI servery hello.

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

## <a name="rhui-overview"></a>Přehled RHUI
[Red Hat aktualizace infrastruktury](https://access.redhat.com/products/red-hat-update-infrastructure) nabízí vysoce škálovatelné řešení toomanage yum úložiště obsahu pro Red Hat Enterprise Linux cloudové instance, které hostují poskytovatelé certifikované Red Hat cloudů. Na základě hello nadřazeného buničiny projektu, RHUI poskytovatelům cloudových toolocally zrcadlení hostované Red Hat úložiště obsahu, vytvořte vlastní úložiště s vlastní obsah a nastavte tyto úložiště k dispozici tooa velkou skupinu koncovým uživatelům prostřednictvím vyrovnáváním zatížení systém doručování obsahu.

## <a name="regions-where-rhui-is-available"></a>Oblasti, kde je k dispozici RHUI
RHUI je k dispozici ve všech oblastech, kde jsou k dispozici na vyžádání RHEL bitové kopie. Aktuálně zahrnuje všechny veřejné oblasti uvedené na hello [řídicí panel Azure stav](https://azure.microsoft.com/status/) stránky, oblasti Azure US Government a Azure v Německu. RHUI přístup pro virtuální počítače zřízené z bitové kopie na vyžádání RHEL je součástí jejich cena. Dodatečné cloudové místní/national dostupnosti bude aktualizovat, protože jsme rozbalte RHEL dostupnosti na vyžádání v budoucnu hello.

> [!NOTE]
> Hostované tooAzure RHUI přístup je omezená toohello virtuálních počítačů v rámci [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

## <a name="get-updates-from-another-update-repository"></a>Získat aktualizace z jiného úložiště aktualizací
Pokud potřebujete tooget aktualizace z různých aktualizace úložiště (namísto Azure hostovaná RHUI), je třeba nejprve toounregister z RHUI vaše instance. Pak je nutné zaregistrovat toore je hello požadované aktualizace infrastruktury (třeba Red Hat satelitní nebo Red Hat zákazníka portál CDN). Bude nutné vhodné Red Hat odběry pro tyto služby a registraci [Red Hat přístup do cloudu v Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

toounregister RHUI a znovu zaregistrujte tooyour aktualizace infrastruktury pomocí těchto kroků:

1. Upravit /etc/yum.repos.d/rh-cloud.repo a změňte všechny `enabled=1` příliš`enabled=0`. Například:
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. Upravte /etc/yum/pluginconf.d/rhnplugin.conf a změňte `enabled=0` příliš`enabled=1`.
3. Potom proveďte registraci s požadovanou hello infrastruktury, například zákaznický portál Red Hat. Postupujte podle Red Hat řešení Průvodce na [jak tooregister a přihlásit se k systému toohello zákaznický portál Red Hat](https://access.redhat.com/solutions/253273).

> [!NOTE]
> Přístup toohello Azure hostovaná RHUI je součástí ceny image hello průběžné platby srážek daně (ze RHEL MZDY). Zrušení registrace virtuálních počítačů systému RHEL srážek daně ze MZDY z hello Azure hostovaná RHUI nepřevede hello virtuálního počítače do typu Bring-Your-vlastní-licenci (BYOL) virtuálních počítačů. Když si zaregistrujete hello stejného virtuálního počítače s jiný zdroj aktualizací, může být nabíhání poplatků za dvojité: nejdřív čas pro Azure RHEL softwaru poplatek a hello znovu pro Red Hat odběry. 
> 
> Pokud potřebujete konzistentně toouse aktualizace infrastruktury než Azure hostovaná RHUI, zvažte vytvoření a nasazení vlastních bitových kopií (BYOL-type), jak je popsáno v [vytvořit a nahrát Red Hat virtuálním počítačem Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) článku.
> 

## <a name="next-steps"></a>Další kroky
toocreate Red Hat Enterprise virtuálního počítače s Linuxem z Azure Marketplace s průběžnými platbami image a využívání Azure hostovaná RHUI přejděte příliš[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). Bude moct toouse `yum update` v instanci RHEL bez jakékoli další nastavení.

