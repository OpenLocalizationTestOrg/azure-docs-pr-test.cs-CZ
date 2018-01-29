---
title: "Řešení potíží s Azure Backup selhání: hosta stavu agenta není k dispozici | Microsoft Docs"
description: "Příznaky, příčiny a řešení Azure Backup chyb související s agenta, rozšíření, disky"
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
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli;markgal;sogup;
ms.openlocfilehash: 0be2391268e11593802cb0f455e8c4553f0d4731
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="troubleshoot-azure-backup-failure-issues-with-agent-andor-extension"></a>Řešení potíží s Azure Backup selhání: problémy s agenta nebo rozšíření

Tento článek obsahuje pro řešení potíží k řešení zálohování chyby týkající se problémů při komunikaci s agentem virtuálních počítačů a rozšíření.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>Nelze navázat komunikaci s Azure Backup Agent virtuálního počítače

> [!NOTE]
> Pokud spuštění zálohování virtuálního počítače s Linuxem Azure selhání s touto chybou na nebo po 4. ledna 2018, spusťte následující příkaz v ovlivněných virtuální počítače a opakujte zálohování

    sudo rm -f /var/lib/waagent/*.[0-9]*.xml

Po registraci a naplánovat virtuálního počítače pro službu Azure Backup, zálohování spustí úlohu komunikaci s agentem virtuálních počítačů k pořízení snímku v daném okamžiku. Některé z následujících podmínek může zabránit snímek z se aktivuje, což následně může vést k selhání zálohování. Postupujte podle níže řešení potíží s kroky v uvedeném pořadí a opakujte operaci.

##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 1: [virtuální počítač nemá žádný přístup k Internetu](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Příčina 2: [agenta je nainstalovaná ve virtuálním počítači, ale je reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 3: [agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Příčina 4: [nelze načíst stav snímku ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>Příčina 5: [rozšíření zálohování se nezdaří aktualizace nebo zatížení](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-6-azure-classic-vms-may-require-additional-step-to-complete-registrationazure-classic-vms-may-require-additional-step-to-complete-registration"></a>Příčina 6: [Azure klasické virtuální počítače může vyžadovat další krok k dokončení registrace](#azure-classic-vms-may-require-additional-step-to-complete-registration)

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Snímek operace se nezdařila z důvodu žádné připojení k síti na virtuálním počítači
Po registraci a naplánovat virtuálního počítače pro službu Azure zálohování, zálohování spustí úlohu komunikaci s rozšíření zálohování virtuálních počítačů k pořízení snímku v daném okamžiku. Některé z následujících podmínek může zabránit snímek z se aktivuje, což následně může vést k selhání zálohování. Postupujte podle níže řešení potíží s kroky v uvedeném pořadí a opakujte operaci.
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 1: [virtuální počítač nemá žádný přístup k Internetu](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Příčina 2: [nelze načíst stav snímku ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-3-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>Příčina 3: [rozšíření zálohování se nezdaří aktualizace nebo zatížení](#the-backup-extension-fails-to-update-or-load)

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot rozšíření operace se nezdařila.

Po registraci a naplánovat virtuálního počítače pro službu Azure zálohování, zálohování spustí úlohu komunikaci s rozšíření zálohování virtuálních počítačů k pořízení snímku v daném okamžiku. Některé z následujících podmínek může zabránit snímek z se aktivuje, což následně může vést k selhání zálohování. Postupujte podle níže řešení potíží s kroky v uvedeném pořadí a opakujte operaci.
##### <a name="cause-1-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Příčina 1: [nelze načíst stav snímku ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-2-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>Příčina 2: [rozšíření zálohování se nezdaří aktualizace nebo zatížení](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-3-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 3: [virtuální počítač nemá žádný přístup k Internetu](#the-vm-has-no-internet-access)
##### <a name="cause-4-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Příčina 4: [agenta je nainstalovaná ve virtuálním počítači, ale je reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-5-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 5: [agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)

## <a name="unable-to-perform-the-operation-as-the-vm-agent-is-not-responsive"></a>Nelze provést operaci, protože agenta virtuálního počítače není reaguje

Po registraci a naplánovat virtuálního počítače pro službu Azure zálohování, zálohování spustí úlohu komunikaci s rozšíření zálohování virtuálních počítačů k pořízení snímku v daném okamžiku. Některé z následujících podmínek může zabránit snímek z se aktivuje, což následně může vést k selhání zálohování. Postupujte podle níže řešení potíží s kroky v uvedeném pořadí a opakujte operaci.
##### <a name="cause-1-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Příčina 1: [agenta je nainstalovaná ve virtuálním počítači, ale je reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-2-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 2: [agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-3-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 3: [virtuální počítač nemá žádný přístup k Internetu](#the-vm-has-no-internet-access)

## <a name="backup-failed-with-an-internal-error---please-retry-the-operation-in-a-few-minutes"></a>Zálohování došlo k vnitřní chybě – opakujte operaci za několik minut

Po registraci a naplánovat virtuálního počítače pro službu Azure zálohování, zálohování spustí úlohu komunikaci s rozšíření zálohování virtuálních počítačů k pořízení snímku v daném okamžiku. Některé z následujících podmínek může zabránit snímek z se aktivuje, což následně může vést k selhání zálohování. Postupujte podle níže řešení potíží s kroky v uvedeném pořadí a opakujte operaci.
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>Příčina 1: [virtuální počítač nemá žádný přístup k Internetu](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-agent-installed-in-the-vm-but-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Příčina 2: [agent nainstalovaný ve virtuálním počítači, ale reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 3: [agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Příčina 4: [nelze načíst stav snímku ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>Příčina 5: [rozšíření zálohování se nezdaří aktualizace nebo zatížení](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-6-backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lockbackup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>Příčina 6: [zálohování služby nemá oprávnění k odstranění staré body obnovení z důvodu zámku skupiny prostředků](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)

## <a name="the-specified-disk-configuration-is-not-supported"></a>Zadaná konfigurace disku nejsou podporovány

> [!NOTE]
> Máme privátní Preview verzi pro podporu zálohování pro virtuální počítače s > disky 1TB. Podrobnosti najdete na [privátní Preview verzi pro podporu zálohování velkých disků virtuálních počítačů](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

V současné době zálohování Azure nepodporuje velikosti disků [větší než 1023GB](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm). 
- Pokud máte disky větší než 1 TB, [připojte nové disky](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal), které jsou menší než 1 TB. <br>
- Potom zkopírujte data z disku většího než 1 TB na nově vytvořené disky s velikostí menší než 1 TB. <br>
- Zkontrolujte, že se všechna data zkopírovala, a potom disky větší než 1 TB odeberte.
- Spustit zálohování

## <a name="causes-and-solutions"></a>Příčiny a řešení

### <a name="the-vm-has-no-internet-access"></a>Virtuální počítač nemá žádný přístup k Internetu
Na požadavcích nasazení virtuálního počítače nemá oprávnění k Internetu, nebo má omezení na místě, která umožňují přístup na infrastrukturu Azure.

Správné fungování rozšíření zálohování vyžaduje připojení k Azure veřejné IP adresy. Rozšíření odesílá příkazy koncový bod Azure Storage (HTTP URL) ke správě snímky virtuálního počítače. Pokud rozšíření nemá přístup do veřejného Internetu, zálohování se nakonec nezdaří.

####  <a name="solution"></a>Řešení
Chcete-li problém vyřešit, zkuste jednu z metod, které jsou zde uvedeny.
##### <a name="allow-access-to-the-azure-storage-corresponding-to-the-region"></a>Povolit přístup k úložišti Azure odpovídající oblast

Připojení k úložišti konkrétní oblasti můžete povolit pomocí [služby značky](../virtual-network/security-overview.md#service-tags). Ujistěte se, že pravidlo, které umožňuje přístup k účtu úložiště má vyšší prioritu než pravidlo, které blokuje přístup k Internetu. 

![Skupina NSG s značky úložiště pro oblast](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Značky služby úložiště jsou k dispozici pouze v určitých oblastí a jsou ve verzi preview. Seznam oblastí naleznete v tématu [služby značky pro úložiště](../virtual-network/security-overview.md#service-tags).

##### <a name="create-a-path-for-http-traffic-to-flow"></a>Vytvoření cesty pro tok provozu HTTP

1. Pokud máte omezení síťového na místě (například skupinu zabezpečení sítě), nasazení proxy server HTTP směrovat provoz.
2. Povolit přístup k Internetu prostřednictvím serveru proxy protokolu HTTP, přidejte pravidla na skupinu zabezpečení sítě, pokud nemáte.

Další informace o nastavení proxy serveru HTTP pro zálohování virtuálních počítačů naleznete v tématu [Příprava prostředí pro zálohování virtuálních počítačů Azure](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

V případě, že používáte spravované disků, může být nutné další port (8443) otevírání na bránu firewall.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Agent nainstalovaný ve virtuálním počítači, ale reagovat (pro virtuální počítače Windows)

#### <a name="solution"></a>Řešení
Agent virtuálního počítače může dojít k poškození nebo služba může byla zastavena. Opětovné instalaci agenta virtuálního počítače by pomáhají získat nejnovější verzi a restartujte komunikace.

1. Ověřte, zda Služba agenta hosta Windows spuštěna v služeb (services.msc) virtuálního počítače. Zkuste restartovat službu systému Windows agenta hosta a zahájit zálohování<br>
2. Pokud není zobrazená v služby, ověřte programy a funkce, zda je nainstalována Služba agenta hosta systému Windows.
4. Pokud budete moci zobrazit na panelu Programy a funkce odinstalace agenta hosta systému Windows.
5. Stáhněte a nainstalujte [nejnovější verzi MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). K dokončení instalace je potřeba oprávnění správce.
6. Pak by měl být schopní zobrazit služby agenta hosta Windows ve službě
7. Zkuste spustit zálohu na vyžádání nebo ad hoc kliknutím na "zálohování" na portálu.

Rovněž ověřte váš virtuální počítač má  **[.NET 4.5 v systému nainstalovány](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)**. Je potřeba agenta virtuálního počítače ke komunikaci se službou

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)

#### <a name="solution"></a>Řešení
Většina související s agenta nebo rozšíření selhání pro virtuální počítače s Linuxem jsou způsobeny problémy, které ovlivňují zastaralé agenta virtuálního počítače. Chcete-li vyřešit tento problém, postupujte podle následujících obecných pokynů:

1. Postupujte podle pokynů pro [aktualizace agenta virtuálního počítače s Linuxem](../virtual-machines/linux/update-agent.md).

 >[!NOTE]
 >Jsme *důrazně doporučujeme* aktualizovat agenta pouze prostřednictvím distribuční úložiště. Nedoporučujeme stáhne kód agenta přímo z Githubu a jeho aktualizaci. Pokud není k dispozici pro distribuční nejnovější verzi agenta, obraťte se na podporu distribuční pokyny o tom, jak ji nainstalovat. Chcete-li vyhledat nejnovější agenta, přejděte na [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) stránky v úložišti GitHub.

2. Ujistěte se, že Azure agent běží ve virtuálním počítači tak, že spustíte následující příkaz:`ps -e`

 Pokud proces není spuštěná, restartujte ji pomocí následujících příkazů:

 * Pro Ubuntu:`service walinuxagent start`
 * Pro ostatní distribuce:`service waagent start`

3. [Konfigurace automatického restartování agenta](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Spusťte nové zálohování testu. Je-li chyba přetrvávat, shromážděte tyto protokoly z virtuálního počítače zákazníka:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * / var/protokolu/azure / *

Pokud jsme vyžadovat podrobné protokolování pro příkaz waagent, postupujte podle těchto kroků:

1. V souboru /etc/waagent.conf vyhledejte následující řádek: **zapnout podrobné protokolování (y | n)**
2. Změna **Logs.Verbose** z hodnoty  *n*  k *y*.
3. Změnu uložíte a znovu spusťte příkaz waagent podle předchozích pokynů v této části.

### <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Nelze načíst stav snímku ani snímku nelze provést.
Zálohování virtuálních počítačů spoléhá na vydání snímku příkazu k základní účtu úložiště. Zálohování může selhat, protože ho nemá přístup k účtu úložiště nebo provádění úlohy snímku je zpožděno.

#### <a name="solution"></a>Řešení
Selhání úlohy snímku mohou způsobovat následující podmínky:

| Příčina | Řešení |
| --- | --- |
| Virtuální počítač má nakonfigurované zálohování serveru SQL Server. | Ve výchozím nastavení zálohování virtuálních počítačů běží VSS úplného zálohování na virtuální počítače Windows. Na virtuální počítače, které jsou spuštěné na serveru SQL servery a na které systém SQL Server zálohování je nakonfigurované, může docházet k prodlevám provádění snímku.<br><br>Pokud dochází k selhání zálohování kvůli potížím snímek, nastavte následující klíč registru:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| Stav virtuálního počítače je nesprávně uvést, protože virtuální počítač je vypnutý v protokolu RDP. | Pokud se vypnout virtuální počítač v protokolu RDP (Remote Desktop), zkontrolujte portálu k určení, zda je správný stav virtuálního počítače. Pokud není správný, vypněte virtuální počítač na portálu pomocí **vypnutí** možnost na řídicím panelu virtuálních počítačů. |
| Virtuální počítač nemůže získat adresu hostitele nebo prostředků infrastruktury z DHCP. | DHCP musí být povolen v hosta pro zálohování virtuálních počítačů IaaS pracovat.  Pokud virtuální počítač nemůže získat adresu hostitele nebo prostředků infrastruktury z odpovědi DHCP 245, nemůže stáhnout nebo spustit libovolné rozšíření. Pokud potřebujete statickou privátní IP adresu, byste ho měli nakonfigurovat prostřednictvím platformy. Možnost DHCP ve virtuálním počítači by měly být vlevo povoleny. Další informace najdete v tématu [nastavení statickou privátní IP interní](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>Rozšíření zálohování se nezdaří aktualizace nebo zatížení
Pokud rozšíření nelze načíst, zálohování se nezdaří, protože snímek nemůže být provedeny.

#### <a name="solution"></a>Řešení

**Pro hosty Windows:** ověřte, zda je povoleno iaasvmprovider služby a má typ spuštění *automatické*. Pokud služba není nakonfigurována tímto způsobem, povolte ji k určení, zda je úspěšné další zálohování.

**Pro hosty Linux:** ověřte nejnovější verzi VMSnapshot pro Linux (rozšíření používané zálohování) je 1.0.91.0.<br>


Pokud se aktualizace nebo zatížení stále rozšíření zálohování nezdaří, můžete vynutit rozšíření VMSnapshot nahrávat znovu odinstalací rozšíření. Další pokus o zálohování dojde k opětovnému načtení rozšíření.

Pokud chcete odinstalovat rozšíření, postupujte takto:

1. Přejděte na [portál Azure](https://portal.azure.com/).
2. Najděte virtuálního počítače, který má potíže s zálohování.
3. Klikněte na tlačítko **nastavení**.
4. Klikněte na tlačítko **rozšíření**.
5. Klikněte na tlačítko **Vmsnapshot rozšíření**.
6. Klikněte na tlačítko **odinstalovat**.

Tento postup způsobí, že rozšíření nutné přeinstalovat během příští zálohování.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>Zálohování služby nemá oprávnění k odstranění staré body obnovení z důvodu zámku skupiny prostředků
Tento problém je specifická pro spravovaných virtuálních počítačů, kde uživatel uzamkne skupině prostředků a služba zálohování není možné odstranit starší body obnovení. Z důvodu to nových záloh spustit selhání, jako je omezení maximální 18 body obnovení uložené z back-end.

#### <a name="solution"></a>Řešení

K vyřešení problému, použijte následující kroky k odebrání kolekce bodu obnovení: <br>
 
1. Odebrat skupinu prostředků uzamknout, ve kterém je umístěn virtuální počítač 
     
2. Instalace pomocí Chocolatey ARMClient <br>
   https://github.com/projectkudu/ARMClient
     
3. Přihlášení k ARMClient <br>
             `.\armclient.exe login`
         
4. Bod obnovení Get kolekce odpovídající virtuální počítač <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Příklad:`.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
             
5. Odstranit kolekci bodu obnovení <br>
            `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
 
6. Další naplánované zálohování automaticky vytvoří kolekci bod obnovení a nové body obnovení 
 
7. Problém se znovu zobrazí, pokud zamknete skupiny prostředků jako znovu existuje pouze omezení 18 bodů obnovení, po které zálohování začít selhávat 

