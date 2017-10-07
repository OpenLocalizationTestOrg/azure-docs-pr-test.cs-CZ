---
title: "aaaMonitor a řešení potíží s ochrany pro virtuální počítače a fyzické servery | Microsoft Docs"
description: "Azure Site Recovery koordinuje hello replikace, převzetí služeb při selhání a obnovení virtuálních počítačů, které jsou umístěné na tooAzure místní servery nebo do sekundárního datacentra. Použijte tento článek toomonitor a řešení potíží s ochrana webu Virtual Machine Manager nebo technologie Hyper-V."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 0fc8e368-0c0e-4bb1-9d50-cffd5ad0853f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: d790375db5f30e98f009c8d8272558188c51934c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Monitorování a řešení ochrany pro virtuální počítače a fyzické servery
Toto monitorování a řešení potíží pomáhá průvodce se dozvíte, jak stav replikace tootrack a řešení potíží s techniky Azure Site Recovery.

## <a name="understand-hello-components"></a>Pochopení hello součásti
### <a name="vmware-virtual-machine-or-physical-server-site-deployment-for-replication-between-on-premises-and-azure"></a>Virtuální počítač VMware nebo fyzických serverů lokality nasazení pro replikaci mezi místními a Azure
tooset až obnovení databáze mezi místními VMware virtuálního počítače nebo fyzického serveru a Azure, musíte tooset hello konfigurační server, hlavní cílový server a součásti serveru proces na virtuální počítač nebo server. Pokud povolíte ochranu pro zdrojový server hello, nainstaluje Azure Site Recovery hello funkce Mobile Apps služby Microsoft Azure App Service. Po výpadku místní a po selhání serveru zdroj hello přes tooAzure nutné zákazníci tooset procesní server v Azure a místní toorebuild hello zdrojový server na místní hlavní cílový server.

![Nasazení VMware nebo fyzický lokality pro replikaci mezi místními a Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-sites"></a>Nasazení lokality nástroje Virtual Machine Manager pro replikaci mezi místními servery
tooset až obnovení databáze mezi dvěma místními umístěními, potřebujete zprostředkovatele Azure Site Recovery hello toodownload a nainstalujte ji na server nástroje Virtual Machine Manager hello. tooensure připojení k Internetu, aby všechny hello operace, které se spouštějí z hello portál Azure získat přeložený tooon místní operace, musí zprostředkovatel Hello.

![Nasazení lokality nástroje Virtual Machine Manager pro replikaci mezi místními servery](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Nasazení lokality nástroje Virtual Machine Manager pro replikaci mezi místními umístěními a Azure
Při nastavování obnovení databáze mezi místními umístěními a Azure, potřebujete zprostředkovatele Azure Site Recovery hello toodownload a nainstalujte ji na server nástroje Virtual Machine Manager hello. Budete také potřebovat tooinstall hello agenta služeb zotavení Azure, který se musí toobe nainstalován na každém hostiteli technologie Hyper-V. [Další informace](site-recovery-hyper-v-azure-architecture.md) Další informace.

![Nasazení lokality nástroje Virtual Machine Manager pro replikaci mezi místními umístěními a Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Nasazení technologie Hyper-V lokality pro replikaci mezi místními umístěními a Azure
Tento proces je podobný nasazení tooVirtual Machine Manager. Hello pouze rozdílem je, že hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure získat nainstalovaná na hostiteli technologie Hyper-V hello, sám sebe. [Další informace](site-recovery-hyper-v-azure-architecture.md). .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Sledování operací konfigurace, ochrany a obnovení
Všechny operace v Azure Site Recovery je auditovat a sledovat pod hello **úlohy** kartě. Pro všechny konfigurace, ochrany nebo obnovení chyba přejděte toohello **úlohy** kartě a vyhledejte chyby.

![filtru se nezdařilo Hello na kartě úlohy hello v](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Pokud narazíte na selhání v rámci hello **úlohy** kartě, klikněte na úlohu hello a klikněte na tlačítko **podrobnosti o CHYBĚ** pro tuto úlohu.

![Podrobnosti o CHYBĚ tlačítko Hello](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Podrobnosti o chybě Hello vám pomůže identifikovat možná příčina a doporučení pro hello problém.

![Okno, které zobrazuje podrobnosti o chybě pro určité úlohy](media/site-recovery-monitoring-and-troubleshooting/image5.png)

V předchozím příkladu hello jiná operace, která je v průběhu zdá se, že toobe způsobuje toofail Konfigurace ochrany hello. Vyřešte problém hello na základě doporučení hello a pak klikněte na tlačítko **RESART** tooinitiate hello operaci opakujte.

![Hello RESTARTOVÁNÍ tlačítka na kartě úlohy hello](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Hello **RESTARTUJTE** možnost není k dispozici pro všechny operace. Pokud operace nemá hello **RESTARTUJTE** možnost, přejděte zpět objekt toohello a znovu proveďte operaci hello znovu. Můžete zrušit všechny úlohy, který je v průběhu pomocí hello **zrušit** tlačítko.

![tlačítko Zrušit Hello](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machines"></a>Monitorování stavu replikace pro virtuální počítače
Zprostředkovatele Azure Site Recovery monitorování Azure portálu tooremotely hello můžete použít pro každý chráněný hello entit. Klikněte na tlačítko **chráněné položky**a potom klikněte na **CLOUDECH VMM** nebo **skupin ochrany**. Hello **CLOUDECH VMM** karta je dostupná jenom pro nasazení, které jsou založeny na nástroje Virtual Machine Manager. Pro ostatní scénáře hello chráněné entity se pod hello **skupin ochrany** kartě.

![Možnosti Hello Cloudech VMM a skupin ochrany](media/site-recovery-monitoring-and-troubleshooting/image8.png)

V části hello příslušného cloudu nebo ochranu skupiny toosee, které v dolním podokně hello jsou uvedeny všechny dostupné operace, klikněte na tlačítko chráněná entita.

![Dostupné možnosti pro vybrané chráněné entity](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Jak je znázorněno v předchozím snímku obrazovky hello, stav virtuálního počítače hello je **kritický**. Můžete kliknout na hello **podrobnosti o CHYBĚ** tlačítko hello dolní toosee hello chyba. Podle hello **možné příčiny** a **doporučení**, hello problém vyřešit.

![Podrobnosti o chybě tlačítko Hello](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Chyby a doporučení v dialogovém okně Podrobnosti o chybě hello](media/site-recovery-monitoring-and-troubleshooting/image11.png)

> [!NOTE]
> Pokud žádné aktivní operace probíhá nebo se nezdařilo, přejděte toohello **úlohy** zobrazení jako uvedených dřívější chybě tooview hello pro konkrétní úlohu.
>
>

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Řešení problémů místní technologie Hyper-V
Připojení konzoly Správce technologie Hyper-V místní toohello, vyberte hello virtuálního počítače a zobrazit stav replikace hello.

![Možnost tooview stav replikace v konzole Správce technologie Hyper-V hello](media/site-recovery-monitoring-and-troubleshooting/image12.png)

V takovém případě **stav replikace** je **kritický**. Klikněte pravým tlačítkem na hello virtuálního počítače a pak klikněte na **replikace** > **zobrazit stav replikace** toosee hello podrobnosti.

![Stav replikace pro konkrétní virtuální počítač](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Pokud replikace se pozastavila hello virtuálního počítače, klikněte pravým tlačítkem hello virtuálního počítače a pak klikněte na tlačítko **replikace** > **obnovit replikaci**.

![Možnost tooresume replikace v konzole Správce technologie Hyper-V hello](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Pokud nové hostitele technologie Hyper-V, který je v rámci clusteru hello nebo samostatný počítač migruje virtuální počítač a hello hostitele Hyper-V je nakonfigurovaný pomocí Azure Site Recovery, nebude to mít dopad replikace pro virtuální počítač hello. Zkontrolujte, které hostují hello nové technologie Hyper-V splňuje všechny požadavky hello a je nakonfigurováno pomocí Azure Site Recovery.

### <a name="event-log"></a>V protokolu událostí
| Zdroje událostí | Podrobnosti |
| --- |:--- |
| **Aplikace a služby protokoly/Microsoft/VirtualMachineManager/Server/Admin** (serveru Virtual Machine Manager) |Poskytuje užitečné protokolování tootroubleshoot řadu různých problémů nástroje Virtual Machine Manager. |
| **Aplikace a služby protokoly/MicrosoftAzureRecoveryServices/replikace** (hostitele Hyper-V) |Poskytuje užitečné protokolování tootroubleshoot mnohé problémy s agenta služeb zotavení Microsoft Azure. <br/> ![Umístění zdroje událostí replikace pro hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Aplikace a služby Logs/Microsoft/Azure lokality obnovení/poskytovatele/Operational** (hostitele Hyper-V) |Poskytuje užitečné protokolování tootroubleshoot mnohé problémy s službu Microsoft Azure Site Recovery. <br/> ![Umístění zdroje provozních událostí pro hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Aplikace a služby protokoly/Microsoft/Windows/Hyper-V-VMMS/Admin** (hostitele Hyper-V) |Poskytuje užitečné protokolování tootroubleshoot mnoho potíží se správou virtuálních počítačů technologie Hyper-V. <br/> ![Umístění zdroje událostí nástroje Virtual Machine Manager pro hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |

### <a name="hyper-v-replication-logging-options"></a>Možnosti protokolování replikaci technologie Hyper-V
Všechny události, které se týkají replikace tooHyper V přihlášeni hello technologie Hyper-V-VMMS\\správce protokol umístěný v části protokoly aplikací a služeb\\Microsoft\\Windows. Kromě toho můžete povolit protokol analytické pro hello služba Správa virtuálních počítačů technologie Hyper-V. tooenable to přihlašovat, nejprve zkontrolujte hello analytické a ladicí protokoly lze zobrazit v prohlížeči událostí hello. Otevřete Prohlížeč událostí a pak klikněte na **zobrazení** > **zobrazit protokoly pro ladění a**.

![Zobrazit protokoly pro ladění a možnost Hello](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Analytické protokolu je viditelný v **technologie Hyper-V-VMMS**.

![Hello analytické protokolu v prohlížeči událostí stromu hello](media/site-recovery-monitoring-and-troubleshooting/image15.png)

V hello **akce** podokně klikněte na tlačítko **povolit protokol**. Když je tato funkce povolená, zobrazí se v **sledování výkonu** jako **relaci trasování událostí** přidávaném **sady kolekcí dat.**

![Relace trasování událostí ve stromu monitorování výkonu hello](media/site-recovery-monitoring-and-troubleshooting/image16.png)

tooview hello shromážděných informací, nejprve ukončit relaci trasování hello zakázáním hello protokolu. Uložit protokol hello a znovu ho otevřete v prohlížeči událostí nebo použít jiné nástroje tooconvert jej podle potřeby.

## <a name="reach-out-for-microsoft-support"></a>Oslovení pro systém Microsoft Support
### <a name="log-collection"></a>Shromáždění protokolů
Ochrana webu nástroje Virtual Machine Manager, najdete v části příliš[shromáždění protokolů Azure Site Recovery pomocí nástroje podpory diagnostiky Platform (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) toocollect hello požadované protokoly.

Ochrana webu technologie Hyper-V, stáhněte si [nástroj](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) a spustit na protokoly hello toocollect hostitele hello technologie Hyper-V.

Scénáře VMware nebo fyzický server, najdete v části příliš[shromažďování protokolů Azure Site Recovery u VMware a fyzické lokalitě ochrany](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) toocollect hello požadované protokoly.

Nástroj Hello shromažďuje protokoly hello místně v podsložce s náhodným názvem ve složce % LocalAppData%\ElevatedDiagnostics.

![Ukázkové postupy z ochrany lokality Hyper-V.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="open-a-support-ticket"></a>Otevřete lístek podpory
tooraise lístek podpory pro Azure Site Recovery oslovení tooAzure podporu pomocí adresy URL v <http://aka.ms/getazuresupport>.

## <a name="kb-articles"></a>Články znalostní BÁZE
* [Jak toopreserve hello písmeno jednotky pro chráněné virtuální počítače, které jsou převzetí služeb při selhání nebo migrovat tooAzure](http://support.microsoft.com/kb/3031135)
* [Jak toomanage místní využití šířky pásma sítě tooAzure ochrany](https://support.microsoft.com/kb/3056159)
* [Azure Site Recovery: "hello clusteru prostředek nebyl nalezen" Chyba při pokusu o tooenable ochranu pro virtuální počítač](http://support.microsoft.com/kb/3010979)
* [Rady pro pochopení a Průvodce replikace řešení potíží s Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/21948.hyper-v-replica-troubleshooting-guide.aspx)

## <a name="common-azure-site-recovery-errors-and-their-resolutions"></a>Běžné chyby Azure Site Recovery a jejich řešení
Níže jsou uvedeny běžné chyby a jejich řešení. Jednotlivé chyby jsou uvedené na stránce samostatné wiki.

### <a name="general"></a>Obecné
* <span style="color:green;">NOVÉ</span> [úlohy selhání s chybou "operace probíhá." Chyba 505, 514, 532.](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
* <span style="color:green;">NOVÉ</span> [úlohy selhání s chybou "Server není připojený toohello Internetu". Došlo k chybě 25018.](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Nastavení
* [server Virtual Machine Manager Hello nelze registrovat z důvodu vnitřní chyby tooan. Další podrobnosti o chybě hello naleznete v zobrazení úloh toohello na portálu Site Recovery hello. Spusťte instalační program znovu tooregister hello serveru.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
* [Připojení nelze zavedených toohello správce obnovení technologie Hyper-V trezoru. Ověřte nastavení proxy serveru hello a zkuste to znovu později.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfigurace
* [Nelze toocreate skupiny ochrany: došlo k chybě při načítání hello seznam serverů.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
* [Cluster hostitele technologie Hyper-V obsahuje minimálně jeden statický síťový adaptér nebo připojené adaptéry nejsou nakonfigurované toouse DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
* [Nástroj Virtual Machine Manager nemá oprávnění toocomplete akce.](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
* [Nelze vybrat hello účet úložiště v rámci předplatného hello při konfiguraci ochrany.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Ochrana
* <span style="color:green;">NOVÉ</span> [povolení ochrany selhání s chybou "Nepodařilo se nakonfigurovat ochranu pro virtuální počítač hello". Chyba 60007, 40003.](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
* <span style="color:green;">NOVÉ</span> [povolení ochrany selhání s chybou "Hello virtuálního počítače nelze povolit ochranu." Došlo k chybě 70094.](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
* <span style="color:green;">NOVÉ</span> [migrace za provozu chyba 23848 - hello virtuální počítač bude toobe přesune pomocí typu živé. To by mohlo způsobit narušení stavu ochrany pro obnovení hello hello virtuálního počítače.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx)
* [Povolení ochrany se nezdařila, protože Agent není nainstalovaný na hostitelském počítači.](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
* [Vhodný hostitel pro virtuální počítač repliky hello nebyl nalezen - kvůli toolow výpočetní prostředky.](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
* [Vhodný hostitel pro virtuální počítač repliky hello nebyl nalezen - kvůli toono logické síti připojit.](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
* [Nelze se připojit počítač hostitele repliky toohello – nelze navázat připojení.](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)

### <a name="recovery"></a>Obnovení
* Nástroj Virtual Machine Manager nemůže dokončit hostitelskou operaci hello:
  * [Převzetí služeb při selhání toohello vybraný bod obnovení pro virtuální počítač: Obecná chyba odepření přístupu.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
  * [Vybraný bod obnovení pro virtuální počítač technologie Hyper-V se nezdařilo toofail přes toohello: operace byla přerušena.  Zkuste nejnovější bod obnovení. (0x80004004).](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
  * Připojení k serveru hello nelze zavedených (0x00002EFD).
    * [Technologie Hyper-V se nezdařilo tooenable reverzní replikaci pro virtuální počítač.](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
    * [Technologie Hyper-V se nezdařilo tooenable replikace pro virtuální počítač virtuální počítač.](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
  * [Nelze předat převzetí služeb při selhání pro virtuální počítač.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
* [Hello plán obnovení obsahuje virtuální počítače, které nejsou připraveny pro plánované převzetí služeb při selhání.](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
* [Hello virtuální počítač není připraven pro plánované převzetí služeb při selhání.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* [Virtuální počítač není spuštěna a není vypnout.](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
* [Operace vzdálené proběhlo na virtuálním počítači a potvrzení převzetí služeb při selhání se nezdařilo.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* Testovací převzetí služeb při selhání
  * [Převzetí služeb při selhání nebylo možné zahájit, protože probíhá test převzetí služeb při selhání.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
* <span style="color:green;">NOVÉ</span> převzetí služeb při selhání časového limitu 'PreFailoverWorkflow úlohy WaitForScriptExecutionTaskTimeout' z důvodu nastavení konfigurace toohello na skupinu zabezpečení sítě spojená s hello virtuálního počítače nebo počítače hello toowhich podsítěmi hello hello patří. Odkazovat příliš['PreFailoverWorkflow úloh WaitForScriptExecutionTaskTimeout'](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) podrobnosti.

### <a name="configuration-server-process-server-master-target"></a>Konfigurační server, server procesu, hlavní cíl
* [které hello PS/CS je hostitelem jako virtuální počítač hostitele ESXi Hello selže s fialové obrazovka smrti.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Řešení potíží s po převzetí služeb při selhání vzdálené plochy
* Mnoho zákazníků mít potýkají toohello tooconnect problémy při selhání virtuálního počítače v Azure. [Řešení potíží s tooRDP dokumentu do virtuálního počítače hello hello použití](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Přidání veřejnou IP adresu ve virtuálním počítači resource manager
Pokud hello **Connect** tlačítko portálu hello aktivní a nejsou připojené tooAzure prostřednictvím připojení Express Route nebo Site-to-Site VPN, potřebujete toocreate a před použitím vzdáleného přiřadit veřejnou IP adresu virtuálního počítače Plocha/sdílené prostředí. Poté můžete přidat veřejnou IP adresu v síťovém rozhraní hello hello virtuálního počítače.  

![Přidání veřejnou IP adresu na hello síťové rozhraní služeb při selhání virtuálního počítače](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)
