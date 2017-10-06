---
title: "řešení potíží Site Recovery aaaAzure problémů s replikací Azure do Azure a chyby | Microsoft Docs"
description: "Řešení potíží s chyb a problémů při replikaci virtuálních počítačů Azure pro zotavení po havárii"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/10/2017
ms.author: sujayt
ms.openlocfilehash: bca957dd0f40e6b16e68913caf522f3431c55bd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Řešení problémů replikace virtuálního počítače Azure do Azure

Tento článek popisuje hello běžné problémy při Azure Site Recovery při replikaci a obnovení virtuálních počítačů Azure z jedné oblasti tooanother oblasti a vysvětluje, jak tootroubleshoot je. Další informace o podporovaných konfiguracích najdete v tématu hello [matici podpory pro replikaci virtuálních počítačů Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problémy kvótu prostředků Azure (kód chyby 150097)
Vaše předplatné musí být povoleno toocreate virtuálních počítačů Azure ve hello cílová oblast, abyste naplánovali toouse jako svou oblast pro obnovení po havárii. Kromě toho musí mít vaše předplatné dostatečná kvóta toocreate virtuální počítače určité velikosti. Ve výchozím nastavení Site Recovery vyskladnění hello stejnou velikost pro cíl hello virtuálního počítače jako hello zdrojového virtuálního počítače. Pokud hello odpovídající velikost není k dispozici, je automaticky vybrána nejbližší možné velikost hello. Pokud není k dispozici žádné odpovídající velikost, která podporuje konfiguraci zdrojového virtuálního počítače, zobrazí se tato chybová zpráva:

**Kód chyby** | **Možné příčiny** | **Doporučení**
--- | --- | ---
150097<br></br>**Zpráva**: nebylo možné povolit replikaci pro virtuální počítač hello VmName. | -Vaše předplatné, které nemusí být ID povoleno toocreate všechny virtuální počítače v hello cílové oblasti umístění.</br></br>-Svoje ID předplatného nemusí být povoleno nebo nemá dostatečnou kvótu toocreate určité velikosti virtuálních počítačů v hello cílové oblasti umístění.</br></br>-Žádnou vhodnou cílovou velikost virtuálního počítače, který odpovídá hello zdrojového virtuálního počítače síťový adaptér počet (2) nebyl nalezen pro ID předplatného hello v hello cílové oblasti umístění.| Obraťte se na [Azure podporu fakturace](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) tooenable vytvoření virtuálních počítačů pro hello požadované velikosti virtuálních počítačů v cílovém umístění hello pro vaše předplatné. Když je tato funkce povolená, hello opakování operace se nezdařila.

### <a name="fix-hello-problem"></a>Opravte problém hello
Obraťte se na [Azure podporu fakturace](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) tooenable vaše předplatné toocreate virtuální počítače v cílovém umístění hello požadované velikosti.

Pokud hello cílové umístění obsahuje omezení kapacity, zakažte replikaci a povolte ho tooa jiné umístění, kde vaše předplatné má dostatečnou kvótu toocreate virtuální počítače hello požadované velikosti.

## <a name="trusted-root-certificates-error-code-151066"></a>Důvěryhodné kořenové certifikáty (kód chyby 151066)

Pokud nejsou k dispozici na hello virtuálního počítače všechny hello nejnovější důvěryhodných kořenových certifikátů, může selhat úlohu "povolit replikaci". Bez hello certifikáty, hello ověřování a autorizace služby Site Recovery nezdaří volání z hello virtuálních počítačů. Zobrazí se chybová zpráva Hello úlohy Site Recovery "povolit replikaci" hello se nezdařilo:

**Kód chyby** | **Možná příčina** | **Recommendations** (Doporučení)
--- | --- | ---
151066<br></br>**Zpráva**: Site Recovery konfigurace se nezdařila. | Hello požadované důvěryhodných kořenových certifikátů použít k ověřování a ověřování nejsou k dispozici na počítač hello. | -Pro virtuální počítač s operačním systémem Windows hello Ujistěte se, že hello důvěryhodné kořenové certifikáty jsou uloženy v počítači hello. Informace najdete v tématu [konfigurovat Důvěryhodné kořeny a zakázané certifikáty](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Pro virtuální počítač s operačním systémem Linux hello postupujte podle pokynů hello pro důvěryhodných kořenových certifikátů, které zveřejnil distributora verze operačního systému Linux hello.

### <a name="fix-hello-problem"></a>Opravte problém hello
**Windows**

Nainstalujte všechny aktualizace nejnovější Windows hello na hello virtuálních počítačů tak, aby všechny hello důvěryhodné kořenové certifikáty jsou uloženy v počítači hello. Pokud jste v odpojeném prostředí, postupujte podle hello standardní Windows update proces ve vaší organizaci tooget hello certifikáty. Pokud hello požadované certifikáty nejsou k dispozici na hello virtuálních počítačů, nezdaří se hello volání toohello služba Site Recovery z bezpečnostních důvodů.

Správa aktualizací na typické Windows hello nebo certifikát proces správy aktualizací ve vaší organizaci tooget všechny hello nejnovější kořenové certifikáty a odvolání certifikátů hello aktualizovat seznam podle hello virtuálních počítačů.

tooverify, který hello problém vyřešen, přejděte toologin.microsoftonline.com v virtuálního počítače z prohlížeče.

**Linux**

Postupujte podle pokynů hello poskytované Linux distributora tooget hello nejnovější důvěryhodných kořenových certifikátů a hello nejnovější seznam odvolaných certifikátů na hello virtuálních počítačů.

Protože SuSE Linux používá symbolických odkazů toomaintain seznam certifikátů, postupujte takto:

1.  Přihlaste se jako kořenové uživatele.

2.  Spusťte tento příkaz:

      ``# cd /etc/ssl/certs``

3.  toosee Pokud certifikát kořenové certifikační Autority hello Symantec není přítomen nebo není, spusťte tento příkaz:

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4.  Pokud hello soubor nebyl nalezen, spusťte tyto příkazy:

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

      ``# c_rehash``

5.  toocreate symlink s b204d74a.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem, spusťte tento příkaz:

      ``# ln -s  VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

6.  Zkontrolujte toosee, pokud tento příkaz má následující výstup hello. Pokud ne, máte toocreate symlink:

      ``# ls -l | grep Baltimore
      -rw-r--r-- 1 root root   1303 Apr  7  2016 Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 04:47 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 05:01 653b494a.0 -> Baltimore_CyberTrust_Root.pem``

7. Pokud není přítomen symlink 653b494a.0, použijte tento příkaz toocreate symlink:

      ``# ln -s Baltimore_CyberTrust_Root.pem 653b494a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Odchozí připojení pro rozsahy adres URL obnovení lokality nebo IP adresu (kód chyby 151037 nebo 151072)

U replikace toowork Site Recovery se vyžaduje z hello virtuálních počítačů toospecific odchozí připojení, které rozsahy adres URL nebo IP adresu. Pokud virtuální počítač je za bránou firewall nebo používá síť zabezpečení skupiny (NSG) pravidla toocontrol odchozí připojení, může se zobrazit některá z těchto chybových zpráv:

**Kódy chyb** | **Možné příčiny** | **Recommendations** (Doporučení)
--- | --- | ---
151037<br></br>**Zpráva**: tooregister virtuální počítač Azure s Site Recovery se nezdařilo. | -Používáte NSG toocontrol odchozí přístup na hello virtuálních počítačů a hello požadované IP rozsahy nejsou povolené pro odchozí přístup.</br></br>-Používáte nástroje brány firewall třetích stran a hello požadované rozsahy IP/adresy URL nejsou povolené.</br>| – Pokud používáte bránu firewall proxy toocontrol odchozí síťové připojení na hello virtuálních počítačů, ujistěte se, že hello požadovaných součástí adresy URL nebo rozsahy IP datacenter jsou seznam povolených adres. Informace najdete v tématu [brány firewall proxy pokyny](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>– Pokud používáte NSG pravidla toocontrol odchozího síťového připojení na hello virtuálních počítačů, zajistěte, aby rozsahy IP požadovaných datacenter hello seznam povolených adres. Informace najdete v tématu [sítě doprovodné materiály zabezpečení skupiny](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**Zpráva**: Site Recovery konfigurace se nezdařila. | Připojení nelze zavedených tooSite koncové body služby obnovení. | – Pokud používáte bránu firewall proxy toocontrol odchozí síťové připojení na hello virtuálních počítačů, ujistěte se, že hello požadovaných součástí adresy URL nebo rozsahy IP datacenter jsou seznam povolených adres. Informace najdete v tématu [brány firewall proxy pokyny](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>– Pokud používáte NSG pravidla toocontrol odchozího síťového připojení na hello virtuálních počítačů, zajistěte, aby rozsahy IP požadovaných datacenter hello seznam povolených adres. Informace najdete v tématu [sítě doprovodné materiály zabezpečení skupiny](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-hello-problem"></a>Opravte problém hello
toowhitelist [hello požadované adresy URL](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-urls) nebo hello [požadované rozsahy IP adres](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-ip-ranges), postupujte podle kroků hello v hello [sítě pokyny dokumentu](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-hello-machine-error-code-150039"></a>Nebyl nalezen v počítači hello (kód chyby 150039)

Nový disk připojit toohello, které musí být inicializován virtuálních počítačů.

**Kód chyby** | **Možné příčiny** | **Recommendations** (Doporučení)
--- | --- | ---
150039<br></br>**Zpráva**: Azure datový disk (DiskName) (DiskURI) se logická jednotka (LUN) (LUNValue) nebyla namapované tooa odpovídající disku v rámci hello virtuální počítač, který má hello nahlásila stejnou hodnotu logické jednotky. | -Nový disk data byla připojené toohello virtuálních počítačů, ale nebyl inicializován.</br></br>-hello datový disk uvnitř hello virtuální počítač není správně reporting hello logické jednotky hodnotu, na které hello byl disk připojené toohello virtuálních počítačů.| Zajistěte, aby se inicializují hello datových disků a zopakujte operaci hello.</br></br>Pro Windows: [připojení a inicializovat nový disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).</br></br>Pro Linux: [inicializovat nový datový disk v systému Linux](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux).

### <a name="fix-hello-problem"></a>Opravte problém hello
Zajistěte, aby byly inicializovány hello datových disků a poté opakujte operaci hello:

- Pro Windows: [připojení a inicializovat nový disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).
- Pro Linux: [inicializovat nový datový disk v systému Linux](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux).

Pokud hello potíže potrvají, obraťte se na podporu.


## <a name="unable-toosee-hello-azure-vm-for-selection-in-enable-replication"></a>Nelze toosee hello virtuálního počítače Azure pro výběr v "povolení replikace"

Nemusíte to vidět svého virtuálního počítače Azure pro výběr v [povolení replikace: Krok 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines). Tento problém může být z důvodu konfigurace Site Recovery toostale na hello virtuálního počítače Azure vlevo. Hello stálou konfiguraci může být ponecháno na virtuální počítač Azure v následujících případech hello:

- Povolit replikaci pro virtuální počítač Azure hello pomocí Site Recovery a pak odstranit trezor Site Recovery hello bez výslovně zakazuje replikace na hello virtuálních počítačů.
- Povolit replikaci pro virtuální počítač Azure hello pomocí Site Recovery a pak odstranit skupinu prostředků hello obsahující trezoru Site Recovery hello bez výslovně zakazuje replikace na hello virtuálních počítačů.

### <a name="fix-hello-problem"></a>Opravte problém hello

Můžete použít [odebrat zastaralé konfigurační skript automatické obnovení systému](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) a odeberte hello stálou konfiguraci Site Recovery na hello virtuálního počítače Azure. Měli byste vidět hello virtuálního počítače v [povolení replikace: Krok 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines) po odebrání hello stálou konfiguraci.


## <a name="next-steps"></a>Další kroky
[Replikace virtuálních počítačů Azure](site-recovery-replicate-azure-to-azure.md)
