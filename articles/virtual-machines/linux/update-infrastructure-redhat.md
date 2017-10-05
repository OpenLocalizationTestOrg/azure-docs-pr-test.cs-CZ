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
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat aktualizace infrastruktury (RHUI) pro na vyžádání Red Hat Enterprise Linux virtuálních počítačů v Azure
Pro přístup Red Hat aktualizace infrastruktury (RHUI) nasazené v Azure jsou registrované virtuálních počítačích vytvořených z na vyžádání Red Hat Enterprise Linux (RHEL) bitové kopie, které v Azure Marketplace.  Instance RHEL na vyžádání mají přístup do úložiště místní yum a může přijímat přírůstkové aktualizace.

Seznamu yum úložiště, který se spravuje nástrojem RHUI, je v instanci RHEL nakonfigurovali během zřizování. Nemusíte provádět žádnou další konfiguraci - spustit `yum update` po vaší instance RHEL je připraven k získat nejnovější aktualizace.

> [!NOTE]
> V září 2016 jsme nasadili aktualizované RHUI Azure a v ledna 2017 spuštění postupné vypnutí starší RHUI Azure. Pokud používáte Image RHEL (nebo jejich snímky) od září 2016 nebo novější – pravděpodobně není třeba žádné akce. Pokud však máte starší snímky nebo virtuální počítače, je třeba aktualizovat pro bez přerušení přístup k Azure RHUI jejich konfigurace.
> 

## <a name="rhui-azure-infrastructure-update"></a>Aktualizace RHUI infrastrukturu Azure
Od září 2016 Azure má novou sadu serverů Red Hat aktualizace infrastruktury (RHUI). Tyto servery se nasadí s [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) tak, aby jeden koncový bod (rhui 1.microsoft.cz) mohou být využívána žádné virtuální počítače bez ohledu na oblast. Nový Image průběžné platby srážek daně (ze RHEL MZDY) v Azure Marketplace (verze ze září 2016 nebo novější) přejděte na nové servery Azure RHUI a nevyžadují žádné další akce.

### <a name="determine-if-action-is-required"></a>Určit, zda je požadovaná akce
Pokud máte potíže s připojením k Azure RHUI ze svého virtuálního počítače Azure RHEL srážek daně ze MZDY, postupujte podle těchto kroků
1. Zkontrolujte konfiguraci virtuálních počítačů pro koncový bod Azure RHUI

    Zkontrolujte, zda `/etc/yum.repos.d/rh-cloud.repo` soubor obsahuje odkaz na `rhui-[1-3].microsoft.com` v baseurl z `[rhui-microsoft-azure-rhel*]` část souboru. Pokud se jedná - používáte novou RHUI Azure.

    Pokud ho odkazující na umístění s následující vzor `mirrorlist.*cds[1-4].cloudapp.net` -je vyžadována aktualizace konfigurace.

    Pokud používáte novou konfiguraci a pořád nemůžete připojit k Azure RHUI - souboru případu podpory společnosti Microsoft nebo Red Hat.

    > [!NOTE]
    > Přístup k Azure hostovaná RHUI je omezený na virtuálních počítačích v rámci [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).
    > 

2. Pokud původní RHUI Azure je stále k dispozici, když provedete tuto kontrolu a chcete automaticky aktualizovat konfiguraci, spusťte následující příkaz:

    `sudo yum update RHEL6`nebo `sudo yum update RHEL7` v závislosti na verzi rodiny RHEL.

3. Pokud už můžete připojit k původní RHUI Azure, postupujte podle ruční kroků popsaných v další části.

4. Zajistěte, aby se aktualizovat konfiguraci na zdroj bitové kopie/snímku vliv na zřizování virtuálních počítačů z.

### <a name="phased-shutdown-of-the-old-azure-rhui"></a>Postupné vypnutí staré RHUI Azure
Při vypnutí staré RHUI Azure jsme omezit přístup k němu následujícím způsobem:

1. Další omezení přístupu (ACL) k nastavení IP adres, které jsou již k němu připojuje. Možné vedlejší účinky: Pokud budete pokračovat, pomocí staré RHUI Azure – nové virtuální počítače nemusí být možné se připojit k němu. RHEL virtuálních počítačů pomocí dynamické IP adresy, které projít vypnutí nebo navrácení nebo počáteční sekvence obdržet nových IP a proto může spustit i nedaří připojit k původní RHUI Azure

2. Vypnutí zrcadlení doručování obsahu serverů. Možné vedlejší účinky: Jak jsme vypnout další CDSes se může zobrazit již `yum update` obsluhy čas, další časové limity až do bodu, pokud už můžete připojit k původní RHUI Azure.

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Jsou IP adresy pro nové servery RHUI doručování obsahu
Pokud používáte konfiguraci sítě a dál omezit přístup z virtuálních počítačů systému RHEL srážek daně ze MZDY, ujistěte se, že následující adresy IP jsou povoleny pro `yum update` pro práci v závislosti na prostředí, které jsou v. 

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

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Postup ruční aktualizace pomocí nových serverů Azure RHUI
Stažení (prostřednictvím curl) podpis veřejného klíče

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Ověřte stažené klíč

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Zkontrolujte výstup, ověřte `keyid` a `user ID packet`:

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

Nainstalujte veřejný klíč

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

Zkontrolujte, že podpisu balíčku ve výstupu je v pořádku.

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Nainstalujte ot. / min

```bash
sudo rpm -U azureclient.rpm
```

Po dokončení ověřte, že vám přístup Azure RHUI formuláře virtuálního počítače

### <a name="all-in-one-script-for-automating-the-preceding-task"></a>Vše v jednom skriptu pro automatizaci předchozí úlohy
Pomocí následujícího skriptu pro automatizaci úloh aktualizace ovlivněné virtuální počítače na nové servery Azure RHUI podle potřeby.

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
[Red Hat aktualizace infrastruktury](https://access.redhat.com/products/red-hat-update-infrastructure) nabízí vysoce škálovatelné řešení pro správu obsahu yum úložiště pro Red Hat Enterprise Linux cloudové instance, které hostují poskytovatelé certifikované Red Hat cloudů. Na základě nadřazeného projektu buničiny si RHUI poskytovatelům cloudových místně zrcadlení hostované Red Hat úložiště obsahu, vytvořte vlastní úložiště s vlastní obsah a zpřístupnit tyto úložiště velkou skupinu koncovým uživatelům prostřednictvím Vyrovnávání zatížení systém doručování obsahu.

## <a name="regions-where-rhui-is-available"></a>Oblasti, kde je k dispozici RHUI
RHUI je k dispozici ve všech oblastech, kde jsou k dispozici na vyžádání RHEL bitové kopie. Aktuálně zahrnuje všechny veřejné oblasti uvedené na [řídicí panel Azure stav](https://azure.microsoft.com/status/) stránky, oblasti Azure US Government a Azure v Německu. RHUI přístup pro virtuální počítače zřízené z bitové kopie na vyžádání RHEL je součástí jejich cena. Dodatečné cloudové místní/national dostupnosti bude aktualizovat, protože jsme rozbalte RHEL na vyžádání dostupnosti v budoucnu.

> [!NOTE]
> Přístup k Azure hostovaná RHUI je omezený na virtuálních počítačích v rámci [rozsahy IP Datacentra Azure Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

## <a name="get-updates-from-another-update-repository"></a>Získat aktualizace z jiného úložiště aktualizací
Pokud potřebujete získat aktualizace z různých aktualizace úložiště (namísto Azure hostovaná RHUI), je třeba nejprve zrušit registraci z RHUI vaše instance. Pak budete muset zaregistrovat je znovu s infrastruktury požadované aktualizace (například Red Hat satelitní nebo Red Hat zákazníka portál CDN). Bude nutné vhodné Red Hat odběry pro tyto služby a registraci [Red Hat přístup do cloudu v Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

Zrušení registrace RHUI a znovu zaregistrujte k infrastruktuře aktualizace postupujte takto:

1. Upravit /etc/yum.repos.d/rh-cloud.repo a změňte všechny `enabled=1` k `enabled=0`. Například:
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. Upravte /etc/yum/pluginconf.d/rhnplugin.conf a změňte `enabled=0` k `enabled=1`.
3. Potom proveďte registraci s požadovanou infrastruktury, například zákaznický portál Red Hat. Postupujte podle Red Hat řešení Průvodce na [způsob registrace a přihlášení k odběru systému na zákaznický portál Red Hat](https://access.redhat.com/solutions/253273).

> [!NOTE]
> Přístup k Azure hostovaná RHUI je součástí ceny image průběžné platby srážek daně (ze RHEL MZDY). Zrušení registrace virtuálních počítačů systému RHEL srážek daně ze MZDY z Azure hostovaná RHUI není převodem virtuálního počítače do typu Bring-Your-vlastní-licenci (BYOL) virtuálních počítačů. Když si zaregistrujete stejného virtuálního počítače s jiný zdroj aktualizací, vám může být by docházelo k dvojité poplatky: nejdřív čas pro Azure RHEL softwaru poplatek a podruhé pro odběry Red Hat. 
> 
> Pokud potřebujete konzistentně jiné než použití aktualizace infrastruktury Azure hostovaná RHUI, zvažte vytvoření a nasazení vlastních bitových kopií (BYOL-type), jak je popsáno v [vytvořit a nahrát Red Hat virtuálním počítačem Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) článku.
> 

## <a name="next-steps"></a>Další kroky
Chcete-li vytvořit virtuální počítač Red Hat Enterprise Linux z Azure Marketplace s průběžnými platbami image a využívání Azure hostovaná RHUI přejít na [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). Bude moct používat `yum update` v instanci RHEL bez jakékoli další nastavení.

