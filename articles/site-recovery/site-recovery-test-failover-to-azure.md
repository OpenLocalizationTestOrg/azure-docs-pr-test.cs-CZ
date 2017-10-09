---
title: "aaaTest tooAzure převzetí služeb při selhání ve službě Site Recovery | Microsoft Docs"
description: "Další informace o spuštění testovací převzetí služeb z místní tooAzure"
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: pratshar
ms.openlocfilehash: fa0a93f409cc9f2c2c06c2d91c65971dc90c507f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test--failover-tooazure-in-site-recovery"></a>Testovací převzetí služeb při selhání tooAzure ve službě Site Recovery



Tento článek obsahuje informace a pokyny pro provádění převzetí služeb při selhání nebo zotavení po Havárii procházení virtuálních počítačů a fyzických serverů, které jsou chráněné službou Site Recovery pomocí Azure jako hello obnovení lokality.

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Testovací převzetí služeb při selhání je spouštět toovalidate strategie replikace nebo postupu zotavení po havárii bez ztráty dat nebo výpadek. Provádění testovací převzetí služeb nemá žádný vliv na probíhající replikace hello nebo produkční prostředí. Testovací převzetí služeb při selhání můžete udělat buď na virtuální počítač nebo [plán obnovení](site-recovery-create-recovery-plans.md). Při spouštění testovací převzetí služeb, je nutné toospecify hello sítě toowhich testů, které by virtuální počítače připojit k. Jakmile se aktivuje testovací převzetí služeb, můžete sledovat průběh v hello **úlohy** stránky.  


## <a name="supported-scenarios"></a>Podporované scénáře
Testovací převzetí služeb při selhání se podporuje ve všech scénářích nasazení jiné než [starší verze VMware lokality tooAzure](site-recovery-vmware-to-azure-classic-legacy.md). Testovací převzetí služeb při selhání není podporováno také když virtuální počítač se po tooAzure selhání.  


## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
Tento postup popisuje, jak toorun testovací převzetí služeb při selhání pro obnovení plánu. Případně můžete také spustit test převzetí služeb při selhání pro jeden počítač s použitím hello odpovídající možnost na něm.

![Testovací převzetí služeb při selhání](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Vyberte **plány obnovení** > *recoveryplan_name*. Klikněte na tlačítko **testovací převzetí služeb při selhání**.
1. Vyberte **bod obnovení** toofailover k. Můžete použít jednu z hello následující možnosti:
    1.  **Nejnovější zpracované**: tuto možnost převezme všechny virtuální počítače v hello obnovení plán toohello nejnovější bod obnovení, který již byl zpracován službou Site Recovery. Při provádění převzetí služeb při selhání virtuálního počítače, je také zobrazit časového razítka posledního bodu obnovení zpracování hello. Při provádění převzetí služeb při selhání plánu obnovení, můžete se vrátit tooindividual virtuálního počítače a podívejte se na **body obnovení nejnovější** dlaždici tooget tyto informace. Jak je žádný čas strávený tooprocess hello nezpracovaných dat, tato možnost nabízí možnost převzetí služeb při selhání, nízkou RTO (plánovanou dobu obnovení).
    1.  **Nejnovější aplikace konzistentní**: tuto možnost převezme všechny virtuální počítače v hello obnovení plán toohello nejnovější aplikace konzistentní bod obnovení, který již byl zpracován službou Site Recovery. Při provádění převzetí služeb při selhání virtuálního počítače, je také zobrazit časové razítko poslední bod obnovení konzistentních s aplikací hello. Při provádění převzetí služeb při selhání plánu obnovení, můžete se vrátit tooindividual virtuálního počítače a podívejte se na **body obnovení nejnovější** dlaždici tooget tyto informace.
    1.  **Nejnovější**: tuto možnost napřed zpracuje všechny hello data, která byla odeslaná tooSite obnovení služby toocreate bod obnovení pro každý virtuální počítač, než selhání tooit. Tato možnost poskytuje hello nejnižší plánovaný bod obnovení (plánovaného bodu obnovení) jako hello virtuálnímu počítači vytvořenému po převzetí služeb při selhání budou mít všechna data hello které bylo replikované služby zotavení tooSite, kdy byla aktivována hello převzetí služeb při selhání.
    1.  **Nejnovější více virtuálních počítačů zpracovat**: Tato možnost je dostupná jenom pro plány obnovení, které mají alespoň jeden virtuální počítač s konzistencí pro víc virtuálních počítačů na. Virtuální počítače, které jsou součástí replikační skupiny převzetí služeb při selhání toohello nejnovější běžné více virtuálních počítačů konzistentní obnovení bodu. Ostatní virtuální počítače převzetí služeb při selhání tootheir nejnovější zpracované bod obnovení.  
    1.  **Nejnovější více virtuálních počítačů konzistentní**: Tato možnost je dostupná jenom pro plány obnovení, které mají alespoň jeden virtuální počítač s ON konzistence více virtuálních počítačů. Virtuální počítače, které jsou součástí replikační skupiny převzetí služeb při selhání toohello nejnovější společný více virtuálních počítačů konzistentní s aplikací bod obnovení. Ostatní virtuální počítače převzetí služeb při selhání tootheir nejnovější konzistentní s aplikací bodu obnovení. 
    1.  **Vlastní**: Pokud byste testovací převzetí služeb při selhání virtuálního počítače, pak můžete použít tento bod obnovení konkrétní tooa toofailover možnost.
1. Vyberte **virtuální síť Azure**: Zadejte Azure virtuální síť, kde by se vytvořily hello testovací virtuální počítače. Site Recovery pokusí toocreate testovací virtuální počítače v podsíti stejným názvem a pomocí hello stejnou IP Adresou, který je součástí **výpočty a síť** nastavení hello virtuálního počítače. Pokud podsíť stejný název není k dispozici v hello virtuální síť Azure zadaný pro testovací převzetí služeb při selhání, testovací virtuální počítač se vytvoří v první podsíť hello abecedy. Pokud není dostupná v podsíti hello stejnou IP Adresou, virtuální počítač získá další dostupná v podsíti hello IP adresu. Tato část pro [další podrobnosti](#creating-a-network-for-test-failover)
1. Pokud jste selhání tooAzure a je-li povoleno šifrování dat, v **šifrovací klíč** hello vyberte certifikát, který byl vydán, pokud povolíte šifrování dat během instalace zprostředkovatele. Pokud jste nepovolili šifrování hello virtuálního počítače, můžete tento krok ignorovat.
1. Sledovat průběh převzetí služeb při selhání na hello **úlohy** kartě. Musí být schopný toosee hello testovací počítač repliky v hello portálu Azure.
1. připojení ke vzdálené ploše na virtuálním počítači hello tooinitiate, budete potřebovat příliš[přidejte veřejnou IP adresu](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) na hello síťové rozhraní hello převzít služby při selhání virtuálního počítače. Pokud jsou selhání tooa klasické virtuální počítač, pak musíte příliš[přidání koncového bodu](../virtual-machines/windows/classic/setup-endpoints.md) na portu 3389
1. Jakmile jste hotovi, klikněte na možnost **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello. V **poznámky** zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. To odstraní hello virtuálních počítačů, které byly vytvořeny během testovacího převzetí služeb při selhání.


> [!TIP]
> Site Recovery pokusí toocreate testovací virtuální počítače v podsíti stejným názvem a pomocí hello stejnou IP Adresou, který je součástí **výpočty a síť** nastavení hello virtuálního počítače. Pokud podsíť stejný název není k dispozici v hello virtuální síť Azure zadaný pro testovací převzetí služeb při selhání, testovací virtuální počítač se vytvoří v první podsíť hello abecedy. Pokud cílová IP adresa hello je součástí hello vybrali podsíť, obnovení lokality pokusí toocreate hello testovací převzetí služeb při selhání virtuálního počítače pomocí hello cílová IP adresa. Pokud cílová IP adresa hello není součástí hello vybrali podsíť pak testovacího převzetí služeb při selhání virtuálního počítače získá vytvoří pomocí dostupnou IP adresu v hello vybraná podsíť.
>
>

## <a name="test-failover-job"></a>Testovací převzetí služeb při selhání úlohy

![Testovací převzetí služeb při selhání](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Když se aktivuje testovací převzetí služeb zahrnuje následující kroky:

1. Kontrola předpokladů: Tento krok zajistí, že jsou splněny všechny podmínky požadované pro převzetí služeb při selhání
1. Převzetí služeb při selhání: Tento krok zpracovává hello data a umožňuje připraven, aby virtuální počítač Azure můžete vytvořit mimo ho. Pokud jste vybrali **nejnovější** bodu obnovení, tento krok vytvoří bod obnovení z hello data, která byla odeslána toohello služby.
1. Začátek: Tento krok vytvoří virtuální počítač Azure pomocí zpracování v předchozím kroku hello dat hello.

## <a name="time-taken-for-failover"></a>Čas potřebný pro převzetí služeb při selhání

Převzetí služeb při selhání virtuálních počítačů v určitých případech vyžaduje velmi přechodný krok, který obvykle trvá přibližně 8 toocomplete too10 minut. Těchto případech jsou jako následující:

* Virtuální počítače VMware používáte verzi služby Mobility, které jsou starší než 9.8 hello
* Fyzické servery
* Virtuální počítače s Linuxem VMware
* Technologie Hyper-V virtuální počítače chráněné jako fyzických serverů
* Virtuální počítače VMware, kde nejsou zadány jako spouštěcí ovladače následující ovladače
    * miniport storvsc
    * VMBus
    * storflt
    * Intelide
    * ATAPI
* Virtuální počítače VMware, které nemají služba DHCP povolena bez ohledu na to, jestli jsou pomocí protokolu DHCP nebo statické IP adresy

V hello všech ostatních případech tento zprostředkující krok není povinný a je výrazně nižší hello doba hello převzetí služeb při selhání.


## <a name="creating-a-network-for-test-failover"></a>Vytvoření sítě pro testovací převzetí služeb při selhání
Doporučuje se, že při provádění testovací převzetí služeb zvolíte síť, která je izolovaná od produkční sítě site recovery který jste zadali v **výpočty a síť** nastavení hello virtuálního počítače. Ve výchozím nastavení při vytváření virtuální sítě Azure, je izolovaná od jiných sítích. Tato síť by měl napodobovat produkční sítě:

1. Testovací síť by měla mít stejný počet podsítí, který v produkční sítě a hello stejný název jako hello podsítě v produkční sítě.
1. Testovací síť by měla používat hello stejného rozsahu IP jako produkční sítě.
1. Aktualizace hello DNS hello testovací síti jako hello IP adresa, která jste zadali jako cíle pro virtuální počítač DNS hello pod IP **výpočty a síť** nastavení. Projděte [testovací převzetí služeb při selhání důležité informace týkající se služby active directory](site-recovery-active-directory.md#test-failover-considerations) další podrobnosti.


## <a name="test-failover-tooa-production-network-on-recovery-site"></a>Testovací převzetí služeb při selhání tooa produkční sítě v recovery site
Doporučuje se, že při provádění testovací převzetí služeb zvolíte síť, která se liší od síti produkční obnovení lokality, který jste zadali v **výpočty a síť** nastavení hello virtuálního počítače. Ale pokud Opravdu chcete toovalidate end tooend připojení k síti ve přes virtuální počítač, Všimněte si, hello následující body:

1. Zajistěte, aby že tento hello primárního virtuálního počítače je vypnutí při provádění hello testovací převzetí služeb při selhání. Pokud to neuděláte, bude mít dva virtuální počítače s hello stejnou identitu spuštěné v hello stejné sítě v hello stejnou dobu a které může způsobit tooundesired důsledky.
1. By být ztraceny všechny změny, které provedete do hello testovací převzetí služeb při selhání virtuálního počítače, když jste čištění hello testovací převzetí služeb při selhání virtuálního počítače. Tyto změny nebudou replikované back toohello primárního virtuálního počítače.
1. Tento způsob plnění testování vede tooa výpadek produkční aplikace. Uživatelům aplikace hello by měl být vyzváni, že probíhá toonot použití hello aplikace při přejít k podrobnostem hello zotavení po Havárii.  



## <a name="prepare-active-directory-and-dns"></a>Příprava služby Active Directory a DNS
toorun testovací převzetí služeb při selhání pro testování aplikace, musíte kopii prostředí služby Active Directory produkční hello v testovacím prostředí. Projděte [testovací převzetí služeb při selhání důležité informace týkající se služby active directory](site-recovery-active-directory.md#test-failover-considerations) další podrobnosti.

## <a name="prepare-tooconnect-tooazure-vms-after-failover"></a>Příprava tooconnect tooAzure virtuální počítače po převzetí služeb při selhání

Pokud chcete, aby tooconnect tooAzure virtuálních počítačů pomocí protokolu RDP po převzetí služeb při selhání, zajistěte, aby hello akce souhrnu v tabulce hello.

**Převzetí služeb při selhání** | **Umístění** | **Akce**
--- | --- | ---
**Virtuální počítač Azure s Windows** | Na místním počítači před převzetí služeb při selhání | tooaccess hello virtuálního počítače Azure přes hello internet, povolení protokolu RDP, ujistěte se, že TCP a UDP pravidla jsou přidány k hello **veřejné**, a zda je povolen protokol RDP v **brány Windows Firewall** > **povoleno Aplikace**, pro všechny profily.<br/><br/> Povolení protokolu RDP na počítači hello tooaccess přes připojení site-to-site a zajistit, že protokol RDP je povolené v hello **brány Windows Firewall** -> **povolené aplikace a funkce** pro  **Doména a privátní** sítě.<br/><br/>  Zajistěte, aby zásada SAN hello operační systém je nastaven příliš**OnlineAll**. [Další informace](https://support.microsoft.com/kb/3031135).<br/><br/> Ujistěte se, že nejsou dostupné žádné aktualizace Windows čekající na virtuálním počítači hello při aktivaci převzetí služeb při selhání. Služby Windows update může spustit, když převzetí služeb při selhání a vy nebudete moct toologin toohello virtuálního počítače až po dokončení aktualizace hello. <br/><br/>
**Virtuální počítač Azure s Windows** | Na virtuálním počítači Azure po převzetí služeb při selhání | Pro klasické virtuální počítač [přidejte veřejný koncový bod](../virtual-machines/windows/classic/setup-endpoints.md) pro hello protokol RDP (portu 3389)<br/><br/>  Pro virtuální počítač Resource Manager [přidejte veřejnou IP adresu](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) na něm.<br/><br/> Hello pravidel skupiny zabezpečení sítě na hello při selhání virtuálního počítače a hello toowhich podsíti Azure, který je připojen, třeba tooallow příchozí připojení toohello RDP port.<br/><br/> Pro virtuální počítač Resource Manager, můžete zkontrolovat **spouštění diagnostiky** toolook na snímek hello virtuálního počítače<br/><br/> Pokud se nemůžete připojit, zkontrolujte, že hello je spuštěný virtuální počítač a podívejte se na tyto [tipy pro odstraňování potíží](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).<br/><br/>
**Virtuální počítač Azure s Linuxem** | Na místním počítači před převzetí služeb při selhání | Zkontrolujte, jestli že tento hello Služba Secure Shell na hello virtuálního počítače Azure nastavený toostart automaticky při spuštění systému.<br/><br/> Zkontrolujte, jestli pravidla brány firewall umožňují tooit připojení SSH.
**Virtuální počítač Azure s Linuxem** | Virtuální počítač Azure po převzetí služeb při selhání | Hello pravidel skupiny zabezpečení sítě na hello při selhání virtuálního počítače a hello toowhich podsíti Azure, který je připojen, třeba tooallow příchozí připojení toohello SSH port.<br/><br/> Pro klasické virtuální počítač [přidejte veřejný koncový bod](../virtual-machines/windows/classic/setup-endpoints.md) by měl být vytvořen, tooallow příchozí připojení na hello portu SSH (port TCP 22 ve výchozím nastavení).<br/><br/> Pro virtuální počítač Resource Manager [přidejte veřejnou IP adresu](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) na něm.<br/><br/> Pro virtuální počítač Resource Manager, můžete zkontrolovat **spouštění diagnostiky** toolook na snímek hello virtuálního počítače<br/><br/>



## <a name="next-steps"></a>Další kroky
Jakmile se pokusili jste se úspěšně testovací převzetí služeb můžete zkusit to [převzetí služeb při selhání](site-recovery-failover.md).
