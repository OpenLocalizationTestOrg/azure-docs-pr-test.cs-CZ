---
title: "Azure Disk Encryption řešení potíží s | Microsoft Docs"
description: "Tento článek obsahuje tipy k řešení potíží pro Microsoft Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: ce0e23bd-07eb-43af-a56c-aa1a73bdb747
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: devtiw
ms.openlocfilehash: 5f482a92b8fcd71a1b767fcc5741bc57605997ea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Průvodce odstraňováním potíží Azure Disk Encryption

Tato příručka je (IT) profesionálům, informace analytikům zabezpečení a problémy související s můžou správci cloudové jejichž organizace používají šifrování disku Azure a potřebovat pokyny k řešení potíží s šifrování disku.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Řešení potíží s šifrování disku operačního systému Linux

Šifrování disku operačního systému Linux musí odpojit disk operačního systému před spuštěním procesu šifrování celého disku.   Pokud ne, chybová zpráva z "se nepodařilo odpojit po..." chybová zpráva se může vyskytnout.

Toto je nejpravděpodobnější, při pokusu o šifrování disku operačního systému na cílové prostředí virtuálního počítače, který byl změněn nebo se změnila z jeho podporovaných uložených Galerie image.  Příklady odchylky od podporované bitovou kopii, kterou může narušovat rozšíření možnost Odpojit disk operačního systému:
- Přizpůsobené bitové kopie, které už odpovídají v podporovaném systému souborů nebo schéma rozdělení oddílů.
- Přizpůsobené bitové kopie s aplikací, jako jsou antivirové Docker, SAP, MongoDB nebo Cassandra Apache spuštěn v operačním systému před šifrování.  Tyto aplikace obtížně se ukončí a při současném zachování otevřených popisovačů souborů na jednotce operačního systému, jednotka nemůže nepřipojené způsobující selhání.
- Vlastní skripty, které běží v zavřete čas blízkosti krok šifrování můžete konfliktu a způsobit, že tuto chybu. Tomu může dojít, když šablonu Resource Manager definuje několik rozšíření provést současně, nebo když rozšíření vlastních skriptů nebo jiným běží současně pro šifrování disku.   Tento problém může vyřešit serializaci a izolace tyto kroky.
- Při SELinux nebylo zrušeno před povolením šifrování, selže, krok odpojení.  Po dokončení šifrování, může být SELinux znovu zapnout.
- Pokud je disk operačního systému pomocí příslušné schéma LVM (i když je k dispozici omezená podpora disku data LVM, disk operačního systému LVM není)
- Pokud nejsou splněné požadavky na minimální množství paměti (7 GB je navržený pro šifrování disku operačního systému)
- Když datové jednotky byly rekurzivně připojeny v adresáři /mnt/ nebo navzájem (například /mnt/data1 /mnt/data2, /data3 + /data3/data4, atd.)
- Při další Azure Disk Encryption [požadavky](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) nejsou splněny pro Linux

## <a name="unable-to-encrypt"></a>Nelze zašifrovat

V některých případech je zakázána Linux šifrování disku se zdá být zablokované ve "Spuštění šifrování disku operačního systému" a SSH. Tento proces může trvat mezi 3-16 hodin, na bitovou kopii uložených galerie.  Pokud přidáte více TB velikost datových disků, může trvat dnů. Pořadí šifrování disku operačního systému Linux dočasně ji operačního systému, odpojíte a provádí blok po bloku šifrování celého disku operačního systému před jeho opakovanému připojení v jeho šifrovaného stavu.   Na rozdíl od Azure Disk Encryption v systému Windows Linux šifrování disku neumožňuje souběžné používání virtuálního počítače, když probíhá šifrování.  Výkonové charakteristiky virtuálního počítače, včetně velikosti disku a zda je zálohovaný účet úložiště standard nebo premium (SSD) úložiště může výrazně ovlivnit čas potřebný k dokončení šifrování.

Pokud chcete zkontrolovat stav, pole ProgressMessage vrácená z [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) testují příkaz.   Během šifrované jednotky operačního systému, virtuální počítač dostane do stavu údržby a SSH se vypne taky aby se zabránilo přerušení na probíhající proces.  EncryptionInProgress se ohlásí pro většinu času při šifrování probíhá, a potom několik hodin později s VMRestartPending zprávy, výzvy k restartování virtuálního počítače.  Například:


```
PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Po zobrazení výzvy k restartování virtuálního počítače a po restartování virtuálního počítače a poskytnutí 2 – 3 minuty, pro restartování a poslední postup provést v cílovém, označí stavovou zprávu, že šifrování nakonec byla dokončena.   Jakmile se tato zpráva je k dispozici, na šifrované jednotce operačního systému musí být připravené pro použití a pro virtuální počítač. Chcete-li být znovu použitelné.

V případech, kdy toto pořadí nebyla zobrazena nebo pokud informace o spuštění, zprávu o průběhu nebo další indikátory, chyba ohlásit že OS šifrování se nezdařila uprostřed tento proces (například pokud se zobrazuje chyba "se nepodařilo odpojit" popsané v tomto průvodci), je Doporučujeme obnovit virtuální počítač zpět do snímku nebo zálohy bezprostředně před šifrování.  Před dalším pokusu o je doporučeno vytvořit znovu vyhodnotit vlastnosti virtuálního počítače a ujistěte se, že jsou splněny všechny požadavky.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Řešení potíží s Azure Disk Encryption za bránou Firewall
Při připojení je omezené na základě brána firewall, proxy požadavek nebo nastavení zabezpečení skupiny (NSG) sítě, může být přerušeny možnost rozšíření potřebné úkoly.   To může mít za následek stavové zprávy, jako je například "Stav rozšíření ve virtuálním počítači není k dispozici" a v očekávané scénáře nedaří dokončit.  V následujících má některé běžné problémy brány firewall, které může prozkoumat.

### <a name="network-security-groups"></a>Skupiny zabezpečení sítě
Nastavení skupiny zabezpečení sítě aplikovaná musí umožňovat stále koncový bod podle zdokumentovaných síťovou konfiguraci [požadavky](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) šifrování disku.

### <a name="azure-keyvault-behind-firewall"></a>Azure Keyvault za bránou firewall
Virtuální počítač musí být schopen získat přístup k trezoru klíčů. Najdete pokyny k přístupu do klíče selhání za bránou firewall, který je spravován [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) týmu.

### <a name="linux-package-management-behind-firewall"></a>Linux správy balíčků za bránou firewall
V době běhu Azure Disk Encryption pro Linux spoléhá na systém správy cílového distribučního balíčku pro instalaci potřebné součásti, které před povolením šifrování.  Pokud nastavení brány firewall zabránit virtuální počítač mohli ke stažení a instalaci těchto součástí, se očekává následné selhání.    Postup konfigurace to se může lišit distribucí.  Na Red Hat zajistíte, že odběr manager a yum jsou správně nastavena je pokud proxy server je nutné, důležité.  V tématu [to](https://access.redhat.com/solutions/189533) článek podpory Red Hat v tomto tématu.  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Řešení potíží s jádra serveru systému Windows Server 2016

Součást bdehdcfg není k dispozici ve výchozím nastavení, na jádru serveru Windows Server 2016. Tato součást vyžaduje Azure Disk Encryption.

Chcete-li vyřešit tento problém, kopírovat 4 následující soubory z virtuálního počítače Windows serveru 2016 Data Center do složky c:\windows\system32 bitovou kopii jádra serveru:

```
bdehdcfg.exe
bdehdcfglib.dll
bdehdcfglib.dll.mui
bdehdcfg.exe.mui
```

Pak spusťte následující příkaz:

```
bdehdcfg.exe -target default
```

Tím se vytvoří systémový oddíl 550MB a po restartování systému, můžete pak pomocí nástroje Diskpart, zkontrolujte příslušné svazky a pokračovat.  

Například:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, informace o některé běžné problémy při šifrování disku Azure a jejich řešení. Další informace o této služby a jeho schopnosti najdete v tématu:

- [Použít šifrování disku v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Šifrování virtuálního počítače Azure](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Šifrování na Rest Azure dat](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
