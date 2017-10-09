---
title: "Řešení potíží s Azure Backup selhání: hosta stavu agenta není k dispozici | Microsoft Docs"
description: "Příznaky, příčiny a řešení souvisejících tooerror selhání zálohování Azure: nešlo komunikovat s hello agenta virtuálního počítače"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
keywords: "Zálohování Azure; Agent virtuálního počítače; Připojení k síti;"
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: genli;markgal;
ms.openlocfilehash: 724c61ba80d0a9ef91a5f8543ae72bb86968881b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-backup-failure-issues-with-agent-andor-extension"></a>Řešení potíží s Azure Backup selhání: problémy s agenta nebo rozšíření

Tento článek obsahuje řešení problémů s kroky toohelp vyřešit selhání zálohování související tooproblems v komunikaci s agenta virtuálního počítače a rozšíření.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-toocommunicate-with-azure-backup"></a>Agent virtuálního počítače nelze toocommunicate s Azure Backup
Po registraci a naplánovat virtuálního počítače pro hello služby zálohování Azure se inicializuje zálohování hello úlohu navázat komunikaci s hello tootake agenta virtuálního počítače v okamžiku snímek. Některé z následujících podmínek hello může zabránit hello snímek z se aktivuje, který pak může způsobit selhání tooBackup. Postupujte podle níže v zadané pořadí hello řešení potíží a opakujte operaci.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 1: [hello virtuálního počítače nemá oprávnění k Internetu](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Příčina 2: [hello agenta je nainstalována v hello virtuálních počítačů, ale neodpovídá (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 3: [hello agent nainstalovaný v hello virtuálního počítače je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Příčina 4: [hello snímek stavu nelze načíst ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>Příčina 5: [hello rozšíření zálohování nezdaří tooupdate nebo zatížení](#the-backup-extension-fails-to-update-or-load)

## <a name="snapshot-operation-failed-due-toono-network-connectivity-on-hello-virtual-machine"></a>Operace snímku se nezdařilo z důvodu toono připojení k síti na virtuálním počítači hello
Po registraci a naplánovat virtuálního počítače pro hello služba Azure Backup, zálohování spustí úlohu hello komunikaci s snímku hello virtuálního počítače rozšíření zálohování tootake v daném okamžiku. Některé z následujících podmínek hello může zabránit hello snímek z se aktivuje, který pak může způsobit selhání tooBackup. Postupujte podle níže v zadané pořadí hello řešení potíží a opakujte operaci.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 1: [hello virtuálního počítače nemá oprávnění k Internetu](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Příčina 2: [hello snímek stavu nelze načíst ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-3-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>Příčina 3: [hello rozšíření zálohování nezdaří tooupdate nebo zatížení](#the-backup-extension-fails-to-update-or-load)

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot rozšíření operace se nezdařila.

Po registraci a naplánovat virtuálního počítače pro hello služba Azure Backup, zálohování spustí úlohu hello komunikaci s snímku hello virtuálního počítače rozšíření zálohování tootake v daném okamžiku. Některé z následujících podmínek hello může zabránit hello snímek z se aktivuje, který pak může způsobit selhání tooBackup. Postupujte podle níže v zadané pořadí hello řešení potíží a opakujte operaci.
##### <a name="cause-1-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Příčina 1: [hello snímek stavu nelze načíst ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-2-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>Příčina 2: [hello rozšíření zálohování nezdaří tooupdate nebo zatížení](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-3-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 3: [hello virtuálního počítače nemá oprávnění k Internetu](#the-vm-has-no-internet-access)
##### <a name="cause-4-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Příčina 4: [hello agenta je nainstalována v hello virtuálních počítačů, ale neodpovídá (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-5-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 5: [hello agent nainstalovaný v hello virtuálního počítače je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)

## <a name="unable-tooperform-hello-operation-as-hello-vm-agent-is-not-responsive"></a>Operaci nelze tooperform hello jako hello agenta virtuálního počítače není reakce

Po registraci a naplánovat virtuálního počítače pro hello služba Azure Backup, zálohování spustí úlohu hello komunikaci s snímku hello virtuálního počítače rozšíření zálohování tootake v daném okamžiku. Některé z následujících podmínek hello může zabránit hello snímek z se aktivuje, který pak může způsobit selhání tooBackup. Postupujte podle níže v zadané pořadí hello řešení potíží a opakujte operaci.
##### <a name="cause-1-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Příčina 1: [hello agenta je nainstalována v hello virtuálních počítačů, ale neodpovídá (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-2-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 2: [hello agent nainstalovaný v hello virtuálního počítače je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-3-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 3: [hello virtuálního počítače nemá oprávnění k Internetu](#the-vm-has-no-internet-access)

## <a name="backup-failed-with-an-internal-error---please-retry-hello-operation-in-a-few-minutes"></a>Zálohování došlo k vnitřní chybě – zkuste to prosím znovu hello operaci za několik minut

Po registraci a naplánovat virtuálního počítače pro hello služba Azure Backup, zálohování spustí úlohu hello komunikaci s snímku hello virtuálního počítače rozšíření zálohování tootake v daném okamžiku. Některé z následujících podmínek hello může zabránit hello snímek z se aktivuje, který pak může způsobit selhání tooBackup. Postupujte podle níže v zadané pořadí hello řešení potíží a opakujte operaci.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 1: [hello virtuálního počítače nemá oprávnění k Internetu](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-agent-installed-in-hello-vm-but-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Příčina 2: [hello agent nainstalovaný v hello virtuálních počítačů, ale reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 3: [hello agent nainstalovaný v hello virtuálního počítače je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Příčina 4: [hello snímek stavu nelze načíst ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>Příčina 5: [hello rozšíření zálohování nezdaří tooupdate nebo zatížení](#the-backup-extension-fails-to-update-or-load)


## <a name="causes-and-solutions"></a>Příčiny a řešení

### <a name="hello-vm-has-no-internet-access"></a>Hello virtuálního počítače nemá oprávnění k Internetu
Na požadavcích nasazení hello, hello virtuálních počítačů má žádný přístup k Internetu, nebo ji omezení na místě, které brání přístupu toohello infrastruktury Azure.

toofunction správně, vyžaduje rozšíření zálohování hello toohello připojení k Azure veřejné IP adresy. rozšíření Hello odešle příkazy tooan Azure Storage koncový bod (adresa URL protokolu HTTP) toomanage hello snímky hello virtuálních počítačů. Pokud hello rozšíření nemá žádné toohello přístupu, které veřejný Internet, nakonec zálohování se nezdaří.

####  <a name="solution"></a>Řešení
tooresolve hello problém, použijte jeden z metody hello tady.
##### <a name="allow-access-toohello-azure-datacenter-ip-ranges"></a>Povolit přístup rozsahy IP adres toohello datové centrum Azure

1. Získat hello [seznam IP adres Azure datacenter](https://www.microsoft.com/download/details.aspx?id=41653) tooallow přístup k.
2. Odblokování hello IP adresy spuštěním hello **New-NetRoute** rutiny v hello virtuálního počítače Azure v okně Powershellu se zvýšenými oprávněními. Spusťte rutinu hello jako správce.
3. tooallow přístup toohello IP adresy, přidejte skupinu zabezpečení sítě toohello pravidla, pokud nemáte.

##### <a name="create-a-path-for-http-traffic-tooflow"></a>Vytvoření cesty pro tooflow provoz protokolu HTTP

1. Pokud máte omezení síťového na místě (například skupinu zabezpečení sítě), nasaďte provozu hello tooroute HTTP proxy serveru.
2. tooallow toohello přístup k Internetu z hello HTTP proxy serveru, přidejte skupinu zabezpečení sítě toohello pravidla, pokud nemáte.

toolearn jak tooset až proxy serveru HTTP pro zálohování virtuálních počítačů, najdete v části [připravit vaše prostředí tooback zálohu virtuálních počítačích Azure](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

V případě, že používáte spravované disků, může být nutné další port (8443) otevírání na bránách firewall hello.

### <a name="hello-agent-installed-in-hello-vm-but-unresponsive-for-windows-vms"></a>Hello agent nainstalovaný v hello virtuálních počítačů, ale reagovat (pro virtuální počítače Windows)

#### <a name="solution"></a>Řešení
Hello agenta virtuálního počítače může dojít k poškození nebo hello služby může byla zastavena. Opětovné instalaci agenta virtuálního počítače hello by pomáhají získat nejnovější verzi hello a restartujte hello komunikace.

1. Ověřte, zda Služba agenta hosta Windows spuštěna v služeb (services.msc) hello virtuálního počítače. Zkuste restartovat službu agenta hosta Windows hello a zahájit hello zálohování<br>
2. Pokud není zobrazená v služby, ověřte programy a funkce, zda je nainstalována Služba agenta hosta systému Windows.
4. Pokud jste možnost tooview v hello odinstalovat programy a funkce systému Windows agenta hosta.
5. Stáhněte a nainstalujte hello [nejnovější verzi MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Potřebujete instalaci hello toocomplete oprávnění správce.
6. Měla by být možné tooview agenta hosta Windows služby ve službě
7. Zkuste spustit zálohu na vyžádání nebo ad hoc kliknutím na "zálohování" hello portálu.

Rovněž ověřte váš virtuální počítač má  **[.NET 4.5 nainstalované v systému hello](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)**. Je potřeba toocommunicate agenta virtuálního počítače hello službou hello

### <a name="hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vms"></a>Hello agent nainstalovaný v hello virtuálního počítače je zastaralý (pro virtuální počítače s Linuxem)

#### <a name="solution"></a>Řešení
Většina související s agenta nebo rozšíření selhání pro virtuální počítače s Linuxem jsou způsobeny problémy, které ovlivňují zastaralé agenta virtuálního počítače. tootroubleshoot-li tento problém, postupujte podle následujících obecných pokynů:

1. Postupujte podle pokynů hello pro [aktualizace hello agenta virtuálního počítače s Linuxem](../virtual-machines/linux/update-agent.md).

 >[!NOTE]
 >Jsme *důrazně doporučujeme* aktualizovat agenta hello pouze prostřednictvím distribuční úložiště. Nedoporučujeme stahování hello agenta kód přímo z Githubu a jeho aktualizaci. Pokud není k dispozici pro distribuční hello nejnovější verze agenta, požádejte o pokyny, jak distribuční podporu tooinstall ho. toocheck pro hello nejnovější agenta, přejděte toohello [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) stránky v úložišti GitHub hello.

2. Ujistěte se, zda že tento hello Azure agent běží na hello virtuálních počítačů tak, že spustíte následující příkaz hello:`ps -e`

 Pokud proces hello není spuštěná, restartujte ji pomocí hello následující příkazy:

 * Pro Ubuntu:`service walinuxagent start`
 * Pro ostatní distribuce:`service waagent start`

3. [Konfigurace hello automatické restartování agenta](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Spusťte nové zálohování testu. Pokud hello chyba přetrvává, shromážděte hello následujících protokolů z virtuálních počítačů hello zákazníka:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * / var/protokolu/azure / *

Pokud jsme vyžadovat podrobné protokolování pro příkaz waagent, postupujte podle těchto kroků:

1. V souboru /etc/waagent.conf hello vyhledejte hello následující řádek: **zapnout podrobné protokolování (y | n)**
2. Změna hello **Logs.Verbose** z hodnoty  *n*  příliš*y*.
3. Uložte změnu hello a znovu spusťte příkaz waagent podle hello předchozí kroky v této části.

### <a name="hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Nelze načíst stav snímku Hello nebo snímek nelze provést.
zálohování virtuálních počítačů Hello spoléhá na vydání snímku příkaz toohello základní účet úložiště. Zálohování může selhat, protože nemá žádný účet úložiště toohello přístupu nebo protože hello provádění úlohy snímku hello je zpožděno.

#### <a name="solution"></a>Řešení
selhání úlohy snímku může způsobit Hello následující podmínky:

| Příčina | Řešení |
| --- | --- |
| Hello virtuální počítač má nakonfigurované zálohování serveru SQL Server. | Ve výchozím nastavení spustí zálohování virtuálních počítačů hello úplné zálohování VSS na virtuálních počítačích systému Windows. Na virtuální počítače, které jsou spuštěné na serveru SQL servery a na které systém SQL Server zálohování je nakonfigurované, může docházet k prodlevám provádění snímku.<br><br>Pokud dochází k selhání zálohování kvůli potížím snímek, nastavte hello následující klíč registru:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| Dobrý stav virtuálního počítače je nesprávně uvést, protože je vypnutý hello virtuálních počítačů v protokolu RDP. | Pokud vypnete hello virtuálních počítačů v protokolu RDP (Remote Desktop), zkontrolujte správnost hello stav virtuálního počítače portálu toodetermine hello. Pokud není správný, vypněte hello virtuálních počítačů hello portálu pomocí hello **vypnutí** možnost na řídicím panelu hello virtuálních počítačů. |
| Mnoho virtuálních počítačů z hello stejné cloudové služby jsou nakonfigurované tooback v hello stejnou dobu. | Je nejlepší postup toospread out hello plánů zálohování pro virtuální počítače z hello stejné cloudové služby. |
| Hello virtuální počítač běží na vysoké využití procesoru nebo paměti. | Pokud hello virtuální počítač běží na vysoké využití procesoru (více než 90 procent) nebo velké množství paměti, je hello snímku úloh zařazených do fronty a odložené a nakonec časového limitu. V takovém případě zkuste zálohu na vyžádání. |
| Hello virtuálního počítače nemůže získat hello hostitele nebo fabric adresu z DHCP. | DHCP musí být povolena uvnitř hello hosta pro hello toowork zálohování virtuálních počítačů IaaS.  Pokud hello virtuálního počítače nemůže získat hello hostitele nebo fabric adresu z DHCP odpovědi 245, nemůže stáhnout nebo spustit libovolné rozšíření. Pokud potřebujete statickou privátní IP adresu, byste ho měli nakonfigurovat prostřednictvím platformy hello. Hello možnost DHCP uvnitř hello virtuální počítač by měl být ponecháno povolena. Další informace najdete v tématu [nastavení statickou privátní IP interní](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="hello-backup-extension-fails-tooupdate-or-load"></a>Rozšíření zálohování Hello selže tooupdate nebo zatížení
Pokud rozšíření nelze načíst, zálohování se nezdaří, protože snímek nemůže být provedeny.

#### <a name="solution"></a>Řešení

**Pro hosty Windows:** ověřte, zda text hello iaasvmprovider služby je povolená a má typ spuštění *automatické*. Pokud hello služba není nakonfigurována tímto způsobem, povolte ji toodetermine zda hello další zálohování se podaří.

**Pro hosty Linux:** ověřte, zda text hello nejnovější verze VMSnapshot pro Linux (hello rozšíření používané zálohování) je 1.0.91.0.<br>


Pokud rozšíření zálohování hello stále nedaří tooupdate nebo zatížení, můžete vynutit hello VMSnapshot rozšíření toobe znovu načíst odinstalací hello rozšíření. Další zálohování pokus Hello dojde k opětovnému načtení rozšíření hello.

toouninstall hello rozšíření, hello následující:

1. Přejděte toohello [portál Azure](https://portal.azure.com/).
2. Vyhledejte hello virtuální počítač, který má potíže s zálohování.
3. Klikněte na tlačítko **nastavení**.
4. Klikněte na tlačítko **rozšíření**.
5. Klikněte na tlačítko **Vmsnapshot rozšíření**.
6. Klikněte na tlačítko **odinstalovat**.

Tento postup způsobí, že toobe rozšíření hello přeinstalovat během příští zálohování hello.

