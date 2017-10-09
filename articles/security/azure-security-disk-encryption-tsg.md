---
title: "řešení potíží s šifrování disku aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: 2ecb8df1fb869e3bf8f3be4be4494e6485e75695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Průvodce odstraňováním potíží Azure Disk Encryption

Tato příručka je (IT) profesionálům, informace analytikům zabezpečení a problémy související s můžou správci cloudové jejichž organizace používají šifrování disku Azure a potřebovat pokyny tootroubleshoot-šifrování disku.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Řešení potíží s šifrování disku operačního systému Linux

Šifrování disku operačního systému Linux musí odpojit předchozí toorunning jednotky hello operačního systému přes proces šifrování celého disku hello.   Pokud ne, chybová zpráva z "se nezdařilo toounmount po..." chybová zpráva je pravděpodobně toooccur.

Toto je nejpravděpodobnější, při pokusu o šifrování disku operačního systému na cílové prostředí virtuálního počítače, který byl změněn nebo se změnila z jeho podporovaných uložených Galerie image.  Příklady odchylky od hello podporované bitovou kopii, kterou může narušovat hello rozšíření možnost toounmount hello OS jednotky:
- Přizpůsobené bitové kopie, které už odpovídají v podporovaném systému souborů nebo schéma rozdělení oddílů.
- Přizpůsobené bitové kopie s aplikací, jako jsou antivirové Docker, SAP, MongoDB nebo Cassandra Apache spuštěné v předchozí tooencryption hello operačního systému.  Tyto aplikace se obtížně tooterminate a při otevření souboru popisovače toohello OS jednotky zachovávají, hello jednotka nemůže nepřipojené způsobující selhání.
- Vlastní skripty, které běží v zavřete čas blízkosti toohello šifrování krok může docházet ke konfliktům a způsobit, že tuto chybu. Tomu může dojít, když šablony Resource Manageru současně definuje tooexecute více rozšíření, nebo když rozšíření vlastních skriptů nebo jiným běží současně toodisk šifrování.   Hello problém může vyřešit serializaci a izolování tyto kroky.
- Když SELinux nebylo zakázáno předchozí tooenabling šifrování, odpojte hello krok nezdaří.  Po dokončení šifrování, může být SELinux znovu zapnout.
- Pokud je disk hello operačního systému pomocí příslušné schéma LVM (i když je k dispozici omezená podpora disku data LVM, disk operačního systému LVM není)
- Pokud nejsou splněné požadavky na minimální množství paměti (7 GB je navržený pro šifrování disku operačního systému)
- Když datové jednotky byly rekurzivně připojeny v adresáři /mnt/ nebo navzájem (například /mnt/data1 /mnt/data2, /data3 + /data3/data4, atd.)
- Při další Azure Disk Encryption [požadavky](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) nejsou splněny pro Linux

## <a name="unable-tooencrypt"></a>Nelze tooencrypt

V některých případech se zobrazí šifrování disku Linux hello toobe zablokované ve "Spuštění šifrování disku operačního systému." a SSH je zakázaná. Tento proces může trvat mezi toocomplete 3-16 hodin na bitovou kopii uložených galerie.  Pokud přidáte více TB velikost datových disků, hello proces může trvat dnů. Hello pořadí šifrování disku operačního systému Linux dočasně odpojí jednotky hello operačního systému a provádí šifrování blok po bloku hello celý disk operačního systému, než ho opakovanému připojení v jeho šifrovaného stavu.   Na rozdíl od Azure Disk Encryption v systému Windows Linux šifrování disku neumožňuje souběžné používání hello virtuálních počítačů, když probíhá šifrování hello.  Hello výkonnostních charakteristik hello virtuálních počítačů, včetně velikosti hello hello disku a zda je účet úložiště hello zajištěna úložiště standard nebo premium (SSD), můžete výrazně ovlivnit hello čas potřebný toocomplete šifrování.

Stav toocheck hello ProgressMessage pole vrácených hello [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) testují příkaz.   Při šifrované jednotky hello operačního systému, hello virtuální počítač dostane do stavu údržby a SSH je také zakázané tooprevent jakýkoli proces probíhající toohello přerušení.  Při šifrování probíhá, a potom několik hodin později s VMRestartPending zpráva s výzvou toorestart hello virtuálních počítačů, se pro většinu hello hello doby ohlásí EncryptionInProgress.  Například:


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
ProgressMessage            : OS disk successfully encrypted, please reboot hello VM
```

Po zobrazení výzvy tooreboot hello virtuálních počítačů a po restartování hello virtuálních počítačů a to se 2 – 3 minuty, pro restartování hello a provést na cíli hello toobe závěrečné kroky, označí hello stavovou zprávu, že šifrování nakonec byla dokončena.   Jakmile tato zpráva je k dispozici, hello šifrované jednotky operačního systému je očekávané toobe připravené pro použití a hello virtuálních počítačů toobe použít znovu.

V případech, kdy toto pořadí nebyla zobrazena nebo pokud informace o spuštění, zprávu o průběhu nebo další indikátory, chyba ohlásit že OS šifrování selhala uprostřed hello tohoto procesu (například pokud vidíte chyby hello "se nezdařilo toounmount" popsané v tomto průvodci) Doporučujeme toorestore hello virtuální počítač zpět toohello snímek nebo zálohy okamžitě předchozí tooencryption.  Další pokus předchozí toohello, je navržené toore-vyhodnocení hello charakteristika hello virtuální počítač a ujistěte se, že jsou splněny všechny požadavky.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Řešení potíží s Azure Disk Encryption za bránou Firewall
Při připojení je omezené na základě schopnost hello rozšíření tooperform hello brány firewall, proxy požadavek nebo nastavení zabezpečení skupiny (NSG) sítě, potřeby můžou být přerušeny úlohy.   To může způsobit stavové zprávy, jako je například "Stav rozšíření není k dispozici v hello virtuální počítač" a v očekávané scénáře selhání toofinish.  Hello oddíly, které následují má některé běžné problémy brány firewall, které může prozkoumat.

### <a name="network-security-groups"></a>Skupiny zabezpečení sítě
Všechna nastavení skupiny zabezpečení sítě použít musí umožňovat stále konfigurace sítě hello koncový bod toomeet hello popsané [požadavky](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) šifrování disku.

### <a name="azure-keyvault-behind-firewall"></a>Azure Keyvault za bránou firewall
Hello virtuálního počítače musí být schopný tooaccess trezoru klíčů. Odkazovat tooguidance na selhání tookey přístup přes bránu firewall, který je spravován hello [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) týmu.

### <a name="linux-package-management-behind-firewall"></a>Linux správy balíčků za bránou firewall
V době běhu Azure Disk Encryption pro Linux spoléhá na předchozí tooenabling šifrování požadované součásti systému tooinstall potřeby hello cílový distribuční balíček správy.  Pokud nastavení brány firewall, aby hello virtuálního počítače nebylo možné toodownload a tyto součásti nainstalovat, jsou očekávané následné selhání.    Hello tooconfigure kroky, to se může lišit podle distribuce.  Na Red Hat zajistíte, že odběr manager a yum jsou správně nastavena je pokud proxy server je nutné, důležité.  V tématu [to](https://access.redhat.com/solutions/189533) článek podpory Red Hat v tomto tématu.  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Řešení potíží s jádra serveru systému Windows Server 2016

Na jádru serveru Windows Server 2016 hello bdehdcfg součást není k dispozici ve výchozím nastavení. Tato součást vyžaduje Azure Disk Encryption.

tooworkaround tento problém, kopie hello následující 4 soubory ze složky c:\windows\system32 toohello virtuálního počítače Windows serveru 2016 Data Center hello jádra serveru bitové kopie:

```
bdehdcfg.exe
bdehdcfglib.dll
bdehdcfglib.dll.mui
bdehdcfg.exe.mui
```

Potom spusťte následující příkaz hello:

```
bdehdcfg.exe -target default
```

Tím se vytvoří systémový oddíl 550MB a pak po restartování systému, můžete pomocí nástroje Diskpart toocheck hello svazky a pokračovat.  

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
V tomto dokumentu jste zjistili, informace o některé běžné problémy v Azure disk encryption a jak tootroubleshoot. Další informace o této služby a jeho schopnosti najdete v tématu:

- [Použít šifrování disku v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Šifrování virtuálního počítače Azure](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Šifrování na Rest Azure dat](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
