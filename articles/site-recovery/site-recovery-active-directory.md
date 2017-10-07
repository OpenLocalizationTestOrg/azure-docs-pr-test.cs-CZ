---
title: "aaaProtect služby Active Directory a DNS s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooimplement řešení zotavení po havárii pro službu Active Directory přes Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 7/20/2017
ms.author: pratshar
ms.openlocfilehash: 49903e54f6d6e1839b0571b7a852c6d7517f0aa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Ochrana služby Active Directory a DNS s Azure Site Recovery
Podnikové aplikace, například SharePoint, Dynamics AX a SAP závisí na službě Active Directory a toofunction infrastruktura DNS správně. Když vytvoříte řešení zotavení po havárii pro aplikace, je důležité tooremember potřebovat tooprotect a obnovení služby Active Directory a DNS před hello další součásti aplikace, tooensure, který věcí fungovat správně, pokud dojde k havárii.

Site Recovery je služba Azure, která poskytuje zotavení po havárii tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů. Site Recovery podporuje různé scénáře replikace tooconsistently chránit a bezproblémově převzetí služeb při selhání virtuálního počítače a aplikace tooprivate, public nebo hostitel cloudy.

Pomocí Site Recovery, můžete vytvořit plán obnovení po havárii dokončení automatizované pro službu Active Directory. Pokud dojde k přerušení, můžete zahájit převzetí služeb při selhání během několika sekund z libovolného místa a zprovoznění služby Active Directory za pár minut. Pokud jste nasadili služby Active Directory pro více aplikací, jako je například Sharepointu a SAP v primární lokalitě a chcete toofail přes hello dokončení lokality, můžete převzít služby Active Directory první pomocí Site Recovery a pak převzetí služeb při selhání hello jiné aplikace pomocí plánů obnovení specifické pro aplikaci.

Tento článek vysvětluje, jak toocreate řešení zotavení po havárii pro službu Active Directory, jak tooperform plánované, neplánované a testovací převzetí služeb při selhání pomocí plán obnovení jedním kliknutím, hello podporované konfigurace a požadavky.  Byste měli být obeznámeni s Active Directory a Azure Site Recovery, před zahájením.

## <a name="replicating-domain-controller"></a>Replikace řadiče domény

Je třeba toosetup [Site Recovery replikaci](#enable-protection-using-site-recovery) na alespoň jeden virtuální počítač hostující řadič domény a DNS. Pokud máte [několika řadičů domény](#environment-with-multiple-domain-controllers) ve vašem prostředí, kromě tooreplicating hello virtuálního počítače řadiče domény pomocí Site Recovery tooset byste také měli až [další řadič domény](#protect-active-directory-with-active-directory-replication) v cílové lokalitě hello (Azure nebo do sekundárního místního datacentra). 

### <a name="single-domain-controller-environment"></a>Prostředí řadičů jedné domény.
Pokud máte několik aplikací a jenom jeden řadič domény a chcete, aby toofail přes celý web hello společně, pak doporučujeme použít řadič domény hello tooreplicate Site Recovery toohello sekundární lokality (jestli jste selhání tooAzure nebo tooa sekundární lokalita). Hello stejné replikované domény, řadiče nebo DNS virtuálního počítače lze použít pro [testovací převzetí služeb při selhání](#test-failover-considerations) také.

### <a name="environment-with-multiple-domain-controllers"></a>Prostředí s více řadiči domény
Pokud máte mnoho aplikací a existuje více než jeden řadič domény v prostředí hello nebo pokud máte v plánu toofail přes několik aplikací najednou, doporučujeme, abyste to kromě řadič domény tooreplicating hello virtuální počítač pomocí Site Recovery můžete také nastavení [další řadič domény](#protect-active-directory-with-active-directory-replication) na cílovou lokalitu hello (Azure nebo do sekundárního místního datacentra). Pro [testovací převzetí služeb při selhání](#test-failover-considerations), použít řadič domény replikovaný pomocí Site Recovery a převzetí služeb při selhání, hello další řadič domény na cílový webový server hello. 


Hello následující oddíly popisují, jak tooenable ochrany pro řadič domény ve službě Site Recovery a jak tooset do řadiče domény v Azure.

## <a name="prerequisites"></a>Požadavky
* Místní nasazení služby Active Directory a DNS serveru.
* Trezoru služby služeb Azure Site Recovery v rámci předplatného Microsoft Azure.
* Pokud replikujete tooAzure, spusťte nástroj hodnocení připravenosti virtuálního počítače Azure hello na tooensure virtuální počítače jsou kompatibilní s virtuálními počítači Azure a služeb Azure Site Recovery.

## <a name="enable-protection-using-site-recovery"></a>Povolit ochranu pomocí Site Recovery
### <a name="protect-hello-virtual-machine"></a>Chránit hello virtuálního počítače
Povolení ochrany hello domény řadiče a DNS virtuálního počítače ve službě Site Recovery. Nakonfigurujte nastavení obnovení lokality podle typu virtuálního počítače hello (Hyper-V nebo VMware). řadič domény Hello replikovat pomocí Site Recovery se používá pro [testovací převzetí služeb při selhání](#test-failover-considerations). Ujistěte se, že splňuje hello následující požadavky:

1. je řadič domény Hello server globálního katalogu
2. řadič domény Hello by měl být hello vlastníka role FSMO pro role, které budou potřebovat během testovací převzetí služeb (jinak tyto role se musí toobe [převzaty](http://aka.ms/ad_seize_fsmo) po převzetí služeb při selhání hello)

### <a name="configure-virtual-machine-network-settings"></a>Konfigurace nastavení sítě virtuálního počítače
Pro virtuální počítač hello domény řadiče a DNS konfigurace nastavení sítě v Site Recovery, tak, aby hello virtuální počítač bude správného síťového připojené toohello po převzetí služeb při selhání. 

![Nastavení sítě VM](./media/site-recovery-active-directory/DNS-Target-IP.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Ochrana služby Active Directory s replikací služby Active Directory
### <a name="site-to-site-protection"></a>Site-to-site ochrany
Vytvoření řadiče domény na sekundární lokalitě hello. Při povýšení serveru hello, tooa role řadiče domény, zadejte název hello hello stejné domény, který se používá v primární lokalitě hello. Můžete použít hello **Active Directory Sites and Services** modul snap-in tooconfigure nastavení na propojení lokalit hello objektu toowhich hello je přidáno. Konfigurace nastavení na propojení lokalit, můžete řídit, když se replikují mezi dva nebo více lokalit a jak často. Další informace najdete v tématu [plánování replikace mezi lokalitami](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Ochrana serveru Azure
Postupujte podle pokynů hello příliš[vytvořit řadič domény v virtuální sítě Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Při povyšování role řadiče domény tooa server hello zadejte hello stejný název domény, který se používá v primární lokalitě hello.

Potom [překonfigurovat hello server DNS pro virtuální síť hello](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), server DNS hello toouse v Azure.

![Síť Azure](./media/site-recovery-active-directory/azure-network.png)

**Služba DNS v produkční sítě Azure**

## <a name="test-failover-considerations"></a>Aspekty testovací převzetí služeb při selhání
Testovací převzetí služeb při selhání dojde v síti, která bude izolovaná od produkční sítě tak, aby v produkčním prostředí není žádný vliv.

Většina aplikací také vyžaduje přítomnost hello řadič domény a toofunction serveru DNS. Předtím, než aplikace hello při selhání, řadič domény proto musí toobe vytvořené v toobe hello izolované sítě používá pro testovací převzetí služeb při selhání. Hello toodo nejjednodušší způsob, jak jde tooreplicate řadiče a DNS domény virtuálního počítače pomocí Site Recovery. Před spuštěním testovací převzetí služeb při selhání plánu obnovení hello aplikace hello spusťte testovací převzetí služeb hello domény Řadič virtuálního počítače. Zde je, jak můžete udělat:

1. [Replikovat](site-recovery-replicate-vmware-to-azure.md) hello domény řadiče a DNS virtuálního počítače pomocí Site Recovery.
1. Vytvořte izolovanou síť. Všechny virtuální sítě vytvořené v Azure ve výchozím nastavení je izolovaná od jiných sítích. Doporučujeme vám, že je stejná jako u sítě hello rozsah IP adres pro tuto síť. Nepovolíte připojení site-to-site v této síti.
1. Zadejte adresu IP serveru DNS v síti hello vytvořit, protože hello IP adresy, které očekáváte, že tooget virtuální počítač DNS hello. Pokud replikujete tooAzure, pak zadejte hello IP adresu pro virtuální počítač, který se používá na převzetí služeb při selhání v hello **cílová IP adresa** nastavení v **výpočty a síť** nastavení. 

    ![Cílové IP](./media/site-recovery-active-directory/DNS-Target-IP.png) **cílové IP**

    ![Azure testovací síti](./media/site-recovery-active-directory/azure-test-network.png)

    **Služba DNS v síti Azure testu**

> [!TIP]
> Site Recovery pokusí toocreate testovací virtuální počítače v podsíti stejným názvem a pomocí hello stejnou IP Adresou, který je součástí **výpočty a síť** nastavení hello virtuálního počítače. Pokud podsíť stejný název není k dispozici v hello virtuální síť Azure zadaný pro testovací převzetí služeb při selhání, testovací virtuální počítač se vytvoří v první podsíť hello abecedy. Pokud cílová IP adresa hello je součástí hello vybrali podsíť, Site Recovery pokusí toocreate hello testovací převzetí služeb při selhání virtuálního počítače pomocí hello cílová IP adresa. Pokud cílová IP adresa hello není součástí hello vybrali podsíť, pak testovacího převzetí služeb při selhání virtuálního počítače získá vytvoří pomocí dostupnou IP adresu v hello vybraná podsíť. 
>
>


1. Pokud replikujete tooanother na místní lokalitu a používáte DHCP, postupujte podle pokynů hello příliš[nastavení DNS a DHCP pro testovací převzetí služeb při selhání](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
1. Proveďte testovací převzetí služeb hello domény Řadič virtuálního počítače, spusťte v izolované síti hello. Použijte nejnovější dostupné **aplikace konzistentní** bod obnovení hello domény Řadič virtuálního počítače toodo hello testovací převzetí služeb při selhání. 
1. Spusťte testovací převzetí služeb při selhání pro plán obnovení hello, který obsahuje virtuální počítače, aplikace hello. 
1. Po dokončení testování **vyčistit testovací převzetí služeb při selhání** hello domény Řadič virtuálního počítače. Tento krok odstraní hello řadič domény, který byl vytvořen pro testovací převzetí služeb při selhání.


### <a name="removing-reference-tooother-domain-controllers"></a>Odebrání řadiče domény tooother odkaz
Při provádění testovací převzetí služeb, nemusíte přepnutím do všech řadičů domény hello v testovací síti hello. odkaz hello tooremove jiných řadičů domény, které existují v provozním prostředí, může být nutné příliš[převzetí rolí FSMO služby Active Directory](http://aka.ms/ad_seize_fsmo) a [čištění metadat](https://technet.microsoft.com/library/cc816907.aspx) pro chybí doména řadiče. 



> [!IMPORTANT]
> Některé hello konfigurace popsané v následující části hello nejsou konfigurace řadiče domény standard nebo výchozího hello. Pokud nechcete, aby řadič domény produkční tooa tyto změny a potom můžete vytvořit toobe vyhrazené řadič domény používá toomake Site Recovery testovací převzetí služeb při selhání a proveďte tyto změny toothat.  
>
>

### <a name="issues-because-of-virtualization-safeguards"></a>Problémy, kvůli ochrana virtualizace 

Od verze Windows Server 2012, [další bezpečnostní opatření se mají sestavily do služby Active Directory Domain Services](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Tato bezpečnostní opatření pomoc při ochraně virtualizovaných řadičů domény na vrácení hodnoty USN zpět, tak dlouho, dokud hello základní platformu hypervisoru VM-GenerationID podporuje. Azure podporuje VM-GenerationID, což znamená, že řadiče domény se systémem Windows Server 2012 nebo novější na Azure virtuální počítače mají další bezpečnostní opatření hello. 


Při obnovení hello VM-GenerationID hello invocationID databáze hello AD DS je také obnovit, budou zahozeny hello fond identifikátorů RID a adresáře SYSVOL je označena jako neautoritativní. Další informace najdete v tématu [tooActive Úvod virtualizace Directory Domain Services](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) a [bezpečná virtualizace DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)

Při přechodu tooAzure může způsobit, že resetování VM-GenerationID a který se spustí v další bezpečnostní opatření hello při hello domény Řadič virtuální počítač spustí v Azure. Může dojít **významné zpoždění** uživatel se může toologin toohello domény Řadič virtuálního počítače. Vzhledem k tomu, že tento řadič domény se použije pouze v testovací převzetí služeb, nejsou nezbytná bezpečnostní opatření virtualizace. tooensure, který VM-GenerationID pro počítač virtuální řadič domény hello nemění, můžete změnit hodnotu hello následující too4 typu DWORD v řadiči domény místní hello.

        
        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start
 

#### <a name="symptoms-of-virtualization-safeguards"></a>Příznaky ochrana virtualizace
 
Pokud se ochrana virtualizace mít spuštěna po testovací převzetí služeb, mohou se zobrazit nejméně jeden z následujících příznaků:  

Změna ID generování

![Změna ID generování](./media/site-recovery-active-directory/Event2170.png)

Změna ID vyvolání

![Změna ID vyvolání](./media/site-recovery-active-directory/Event1109.png)

Sdílené složky SYSVOL a Netlogon nejsou k dispozici

![Sdílená složka SYSVOL](./media/site-recovery-active-directory/sysvolshare.png)

![NTFRS Sysvol](./media/site-recovery-active-directory/Event13565.png)

Všechny databáze DFSR jsou odstraněny.

![Odstranění databáze DFSR](./media/site-recovery-active-directory/Event2208.png)


> [!IMPORTANT]
> Některé hello konfigurace popsané v následující části hello nejsou konfigurace řadiče domény standard nebo výchozího hello. Pokud nechcete, aby řadič domény produkční tooa tyto změny a potom můžete vytvořit toobe vyhrazené řadič domény používá toomake Site Recovery testovací převzetí služeb při selhání a proveďte tyto změny toothat.  
>
>


### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Řešení problémů řadič domény během testovacího převzetí služeb při selhání


Na příkazovém řádku spusťte následující příkaz toocheck, zda jsou sdílené složky SYSVOL a NETLOGON hello:

    NET SHARE

Na příkazovém řádku hello spuštění hello následující tooensure příkaz, který hello řadič domény fungovat správně.

    dcdiag /v > dcdiag.txt

V protokolu hello výstupu podívejte se na následující tooconfirm text, který hello řadič domény funguje dobře. 

* "předaný test připojení"
* "předaný testovací inzerování"
* "předaný test MachineAccount"

Pokud hello výše uvedených podmínek jsou splněny, je pravděpodobné, že dobře funguje hello řadiče domény. Pokud ne, zkuste následující kroky.


* Proveďte autoritativního obnovení řadiče domény hello.
    * I když je [nedoporučuje toouse FRS replikace](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), ale pokud jste ji stále používá pak postupujte podle zobrazených pokynů hello [sem](https://support.microsoft.com/kb/290762) toodo autoritativního obnovení. Můžete si přečíst další informace o Burflags věnovala v předchozí odkaz hello [zde](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Pokud používáte DFSR replikace, potom postupujte podle kroků hello k dispozici [sem](https://support.microsoft.com/kb/2218556) toodo autoritativního obnovení. Můžete také použít funkce prostředí Powershell k dispozici v tomto [odkaz](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/) pro tento účel. 
    
* Nastavením následujících too0 klíče registru v řadiči domény místní hello nepoužívat počáteční synchronizaci. Pokud tato DWORD neexistuje, pak můžete vytvořit ji v uzlu "Parametry". Další informace o něm [sem](https://support.microsoft.com/kb/2001093)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Zakažte hello požadavek, aby server globálního katalogu je přihlášení uživatele k dispozici toovalidate nastavením následujících too1 klíče registru v řadiči domény místní hello. Pokud tato DWORD neexistuje, můžete ho vytvořit v uzlu 'Lsa'. Další informace o něm [sem](http://support.microsoft.com/kb/241789)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>DNS a řadič domény na různé počítače
Pokud DNS není na hello stejný virtuální počítač jako řadič domény hello, potřebujete toocreate virtuální počítač DNS hello testovací převzetí služeb při selhání. Pokud jsou na hello stejného virtuálního počítače, můžete tuto část přeskočit.

Můžete použít nový server DNS a vytvořit všechny požadované hello zóny. Například pokud vaše doména služby Active Directory je contoso.com, můžete vytvořit zónu DNS s názvem contoso.com hello. Hello položky odpovídající tooActive adresář musí aktualizovat ve službě DNS, následujícím způsobem:

1. Zajistěte, aby že tato nastavení jsou na místě před žádným jiným virtuálním počítačem v plánu obnovení hello se zobrazí:
   
   * Po název kořenové domény struktury hello musí mít název zóny Hello.
   * Hello zóny musí být záložních souborů.
   * Hello zóny musí být povoleno pro zabezpečení a nezabezpečené aktualizace.
   * překladač Hello hello domény Řadič virtuálního počítače by měla odkazovat toohello IP adresu hello DNS virtuálního počítače.
2. Spusťte následující příkaz na virtuálním počítači řadiče domény hello:
   
    `nltest /dsregdns`
3. Přidání zóny na serveru DNS hello, Povolit nezabezpečené aktualizace a přidejte záznam pro něj tooDNS:
   
        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Další kroky
Čtení [jaké úlohy mohu ochránit?](site-recovery-workload.md) toolearn informace o ochraně podnikové úlohy s Azure Site Recovery.

