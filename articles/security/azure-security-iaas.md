---
title: "aaaSecurity osvědčených postupů pro IaaS úlohy v Azure | Microsoft Docs"
description: " migrace Hello úlohy tooAzure IaaS přináší příležitosti tooreevaluate naše návrhy "
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: barclayn
ms.openlocfilehash: 9cee1ca6effe9561e51dc8b945e7388ffea169b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Osvědčené postupy zabezpečení pro úlohy IaaS v Azure

Protože jste spustili přemýšlení o přesunutí úloh tooAzure infrastruktury jako služby (IaaS), pravděpodobně realizován, že některé aspekty obeznámeni. Prostředí zabezpečení virtuální prostředí již můžete mít. Když přesouváte tooAzure IaaS, můžete použít k zabezpečení virtuální prostředí svoje znalosti a používat novou sadu možností toohelp zabezpečené vaše prostředky.

Začněme tím informacemi o tom, že by neměl Očekáváme, že toobring místních prostředků jako tooAzure 1: 1. Další výzvy Hello a nové možnosti hello převést stávající tooreevaluate možnost deigns, nástroje a procesy.

Vaše odpovědnosti pro zabezpečení je založený na typu hello cloudové služby. Hello následující graf shrnuje hello rovnováhu mezi počtem odpovědnost za společnosti Microsoft a můžete:


![Oblasti odpovědnosti](./media/azure-security-iaas/sec-cloudstack-new.png)


Probereme některé hello možnosti dostupné v Azure, které vám pomohou splnit požadavky na zabezpečení vaší organizace. Mějte na paměti, že požadavky na zabezpečení se může lišit pro různé typy úloh. Mezi tyto doporučené postupy můžete samostatně zabezpečit vaše systémy. Nic jiného v zabezpečení, jako je mít toochoose hello požadované možnosti a v tématu Jak hello řešení můžete vzájemně doplňují vyplněním mezery.

## <a name="use-privileged-access-workstations"></a>Použít pracovní stanice privilegovaný přístup.

Organizace často spadají zneužívají toocyberattacks protože správci provádět akce při používání účtů se zvýšenými oprávněními. Obvykle není to neoprávněnému ale protože existující konfiguraci a procesy povolit. Ve většině těmto uživatelům pochopit hello riziko z hlediska koncepční tyto akce, ale stále zvolte toodo je.

Provádění akcí, jako je kontrola e-mailu a procházení hello Internet pravděpodobně dostatečně nevinnosti. Ale se mohou být vystaveny toocompromise zvýšenými účty pomocí nebezpečného actors, kteří můžou využívat procházení aktivity, speciálního e-mailů nebo jiné techniky toogain přístup tooyour enterprise. Důrazně doporučujeme použití hello pracovní stanice pro správu zabezpečení pro všechny úlohy správy Azure, a tak snížit ohrožení tooaccidental ohrožení provádění.

Pracovní stanice privilegovaného přístupu (PAWs) zadejte pro citlivé úkoly – jeden, který je chráněný před útoky Internet a z nečekaných směrů vyhrazené operačního systému. Oddělení tyto úkoly citlivé a účty z hello denně použití pracovních stanic a zařízení poskytuje silnou ochranu před útoky phishing, aplikace a ohrožení zabezpečení operačního systému, různé zosobnění útokům a útokům krádeže přihlašovacích údajů například stisknutí klávesy protokolování, Pass-the-Hash a Pass-the-Ticket.

Hello TLAPA přístup je rozšířením hello zavedené a doporučené postupem toouse jednotlivě přiřazené účtu správce, která je oddělená od standardní uživatelský účet. TLAPA poskytuje trustworthy pracovní stanice pro tyto citlivé účty.

Další informace a implementaci pokyny najdete v tématu [pracovní stanice privilegovaného přístupu](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Použití vícefaktorového ověřování

V posledních hello byla vaší hraniční sítě používané toocontrol přístup toocorporate data. První cloudu, mobile první světě, identita je rovině řízení hello: použití služeb tooIaaS toocontrol přístup z jakéhokoli zařízení. Můžete ji použít i tooget viditelnost a přehled o tom, jak a kde se používá vaše data. Ochrana hello digitální identitu Azure uživatelů je kamenem hello ochranu vašich předplatných z krádežím identity a dalších cybercrimes.

Jeden z kroků nejvíce výhodné hello můžete podniknout toosecure účet je tooenable dvoufaktorové ověřování. Dvoufaktorové ověřování je způsob ověřování s použitím něco v přidání tooa heslo. Pomáhá zmírnit riziko hello přístup někdo, kdo spravuje tooget heslo jiného uživatele.

[Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) pomáhá zabezpečit přístup toodata a aplikace při splnění požadavků uživatelů pro jednoduchý proces přihlášení. Zajišťuje silné ověřování s celou řadu možností snadno ověření – telefonní hovor, textová zpráva nebo oznámení mobilní aplikace. Uživatelé zvolit hello metoda, která dávají přednost.

Nejjednodušší způsob, jak toouse Hello služby Multi-Factor Authentication je Microsoft Authenticator hello mobilní se aplikace, které lze použít v mobilních zařízení se systémem Windows, iOS a Android. S hello nejnovější verzi Windows 10 a hello integraci místní služby Active Directory s Azure Active Directory (Azure AD) [Windows Hello pro firmy](../active-directory/active-directory-azureadjoin-passport-deployment.md) lze použít pro bezproblémové jeden přihlašování tooAzure prostředky. V takovém případě zařízení hello Windows 10 slouží jako druhý faktor hello k ověřování.

Pro účty, které spravují předplatné Azure a účty, které se můžete přihlásit toovirtual počítače pomocí služby Multi-Factor Authentication vám dává mnohem vyšší úroveň zabezpečení než použití pouze heslo. Jiné formy dvoufaktorové ověřování může fungovat stejně dobře, ale jejich nasazení může být složité, pokud nejsou již v produkčním prostředí.

Hello následující snímek obrazovky ukazuje některé z možností hello k dispozici pro Azure Multi-Factor Authentication:

![Možnosti vícefaktorového ověřování](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Omezení a omezit přístup pro správu

Zabezpečení hello účty, které můžete spravovat vaše předplatné Azure je velmi důležité. Hello ohrožení některý z těchto účtů Neguje hello hodnotu všechny hello další kroky, může trvat tooensure hello utajení a integritu dat. Jako nedávno návodu hello [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) úniku utajených informací, interní útoky představovat obrovské threat toohello celkové zabezpečení každé organizace.

Pomocí následujících kritérií podobné toothese vyhodnoťte jednotlivce pro práva pro správu:

- Jsou jejich provádění úloh, které vyžadují oprávnění správce?
- Jak často jsou prováděny hello úlohy?
- Je k dispozici konkrétní důvod, proč nelze provést úlohy hello jiným správcem jejich jménem?

Zdokumentujte všechny ostatní známé Alternativní přístupy toogranting hello oprávnění a proč každý není přijatelný.

použití Hello správy za běhu zabraňuje hello nepotřebné existenci účty se zvýšenými oprávněními během období, kdy nejsou potřebné tato práva. Účty mít zvýšená práva a po omezenou dobu, tak, aby správci mohou dělat svou práci. Tato práva jsou odebráni na konci hello shift nebo po dokončení úlohy.

Můžete použít [Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md) toomanage, monitorování a řízení přístupu ve vaší organizaci. Pomáhá informován o hello akcí, které jednotlivce provést ve vaší organizaci. Přináší také správy za běhu tooAzure AD zavedením hello konceptu oprávněné správce. Toto jsou jednotlivce, kteří mají účty s potenciální toobe hello udělena práva správce. Tyto typy uživatelů, můžete přejít prostřednictvím procesu aktivace a udělení oprávnění správce po omezenou dobu.


## <a name="use-devtest-labs"></a>Používá DevTest Labs

Pomocí Azure pro testovací prostředí a vývojových prostředí umožňuje organizacím toogain flexibilita v vývoj a testování pomocí trvá tokeny hello zpoždění, které představuje nákup hardwaru. Bohužel chybějících znalost Azure nebo desire toohelp urychlit přijetí může vést hello správce toobe zbytečně projektovou s přiřazení práv. Toto riziko může neúmyslně vystavit útokům toointernal hello organizace. Někteří uživatelé mohou udělit přístup mnohem víc, než by měly mít.

Hello [Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) služby používá [řízení přístupu](../active-directory/role-based-access-control-what-is.md) (RBAC). Pomocí RBAC, můžete v rámci týmu oddělit povinností do rolí, která udělují pouze hello úroveň přístupu, které jsou nezbytné pro uživatele toodo svou práci. RBAC se dodává s předdefinované role (vlastník, uživatel lab a Přispěvatel). Můžete použít i těchto rolí tooassign práva tooexternal partnerů a výrazně zjednodušit spolupráci.

Vzhledem k tomu používá DevTest Labs RBAC, je možné toocreate další, [vlastní role](../devtest-lab/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs nejen zjednodušuje správu hello oprávnění, se zjednodušuje proces hello prostředí zřízený. Pomáhá také řešit potíže s další typické výzvy týmy, které pracují na vývojových a testovacích prostředí. Vyžaduje určitou přípravu, ale hello dlouhou dobu, ji budou usnadnily pro váš tým.

Azure DevTest Labs funkce patří:

- Správní kontrolu nad toousers dostupné možnosti hello. Hello správce můžete centrálně spravovat třeba povolené velikosti virtuálních počítačů, maximální počet virtuálních počítačů a virtuálních počítačů jsou spuštění a vypnutí.
- Automatizace vytváření testovacím prostředí.
- Sledování nákladů.
- Zjednodušená distribuci virtuálních počítačů pro dočasné spolupráce práce.
- Samoobslužné služby, která umožňuje uživatelům tooprovision jejich labs pomocí šablon.
- Správa a omezování spotřeby.

![Pomocí DevTest Labs toocreate testovacího prostředí](./media/azure-security-iaas/devtestlabs.png)

Použití hello DevTest Labs přidružen bez dalších nákladů. Vytvoření Hello labs, zásady, šablony a artefaktů je zdarma. Platíte jenom hello prostředků Azure používá v prostředích, jako jsou virtuální počítače, účty úložiště a virtuální sítě.



## <a name="control-and-limit-endpoint-access"></a>Řízení a omezení přístupu koncový bod

Hostování labs nebo produkční systémy v Azure znamená, že vaše systémy toobe přístupné z Internetu hello. Ve výchozím nastavení nový virtuální počítač Windows má portu RDP hello přístupné z Internetu hello a otevřít port SSH hello má virtuální počítač s Linuxem. Provedení kroků toolimit zveřejněné koncových bodů je nutné toominimize hello riziko neoprávněného přístupu.

Technologie v Azure vám může pomoct omezit hello koncové body toothose správu přístupu. V Azure, můžete použít [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) (Nsg). Pokud používáte pro nasazení Azure Resource Manager, skupiny Nsg omezit přístup hello ze všech sítí toojust hello koncových bodů pro správu (RDP nebo SSH). Pokud se domníváte, že skupiny Nsg, vezměte v úvahu seznamy ACL směrovače. Můžete je používat tootightly řízení hello síťovou komunikaci mezi různé segmenty vaší sítě Azure. Toto je podobné toocreating sítě v hraniční sítě nebo jiné izolované sítě. Navrhují není hello přenosy, ale mohou pomoci s segmentace sítě.


V Azure, můžete nakonfigurovat [site-to-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) z vaší místní sítě. Síť site-to-site VPN rozšiřuje místní sítě toohello cloudu. To vám dává příležitost toouse jiné skupiny Nsg, protože můžete také upravit hello skupiny Nsg toonot povolí přístup odkudkoli jiné než místní sítě hello. Potom můžete vyžadovat jeho správu se provádí první připojování toohello Azure sítě prostřednictvím sítě VPN.

Hello site-to-site VPN možnost může být nejvíce atraktivní v případech, kde jsou hostování produkční systémy, které jsou úzce integrovaná s vaší místní prostředky v Azure.

Alternativně můžete použít hello [point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) možnost v situacích, kdy toomanage systémy, které nebudete potřebovat přístup k prostředkům tooon místní. Tyto systémy můžete izolovat ve své vlastní virtuální síti Azure. Správci můžou VPN do hello Azure hostované prostředí z jejich pracovních stanic.

>[!NOTE]
>Můžete použít buď hello tooreconfigure možnost VPN, seznamy ACL v hello skupiny Nsg toonot povolit koncové body toomanagement přístupu z Internetu hello.

Další možností vhodné vzhledem k tomu je [Brána vzdálené plochy](../multi-factor-authentication/multi-factor-authentication-get-started-server-rdg.md) nasazení. Můžete použít toto nasazení toosecurely plochy servery tooRemote připojení přes protokol HTTPS, při použití podrobnější řídí toothose připojení.

Funkce, které byste měli přístup k tooinclude:

- Správce možnosti toolimit toorequests připojení z konkrétní systémy.
- Ověřování čipovou kartou nebo Azure Multi-Factor Authentication.
- Řízení, přes které systémy někdo může připojit toovia hello gateway.
- Možnost ovládat přesměrování zařízení a disku.

## <a name="use-a-key-management-solution"></a>Použít řešení správy klíčů

Bezpečnou správu klíčů je důležité tooprotecting dat v cloudu hello. S [Azure Key Vault](../key-vault/key-vault-whatis.md), můžete bezpečně uložit šifrovací klíče a tajné klíče malé jako hesel v modulech hardwarového zabezpečení (HSM). Pro zvýšené bezpečí můžete klíče importovat nebo generovat v modulech HSM.

Procesy Microsoft vaše klíče v FIPS 140-2 Level 2 ověřené HSM (hardware a firmware). Sledování a audit použití klíče s Azure protokolování: přesměrováním protokoly do systému Azure nebo vaše informace o zabezpečení a událostí správy (SIEM) pro další analýzu a zjišťování hrozeb.

Každý, kdo má předplatné Azure můžete vytvořit a použít trezorů klíčů. I když Key Vault přínosný vývojářů a správců zabezpečení, můžete implementovat a spravovat správce, který je zodpovědný za správu služby Azure v organizaci.


## <a name="encrypt-virtual-disks-and-disk-storage"></a>Šifrování virtuálních disků a diskových úložišť

[Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) adresy hello hrozby odcizení nebo prozrazení před neoprávněným přístupem, které se dosáhne přesunutím disk dat. Hello disk může být připojené tooanother systému jako způsob obcházení ostatní ovládací prvky zabezpečení. Disk používá šifrování [BitLocker](https://technet.microsoft.com/library/hh831713) v systému Windows a DM-Crypt v Linux tooencrypt operačního systému a datové jednotky. Azure Disk Encryption integruje s toocontrol Key Vault a správu hello šifrovacích klíčů. Je k dispozici pro standardní virtuálních počítačů a virtuálních počítačů služby storage úrovně premium.

Další informace najdete v tématu [Azure Disk Encryption v systému Windows a virtuálních počítačů IaaS Linux](azure-security-disk-encryption.md).

[Azure šifrování služby úložiště](../storage/common/storage-service-encryption.md) pomáhá chránit vaše data v klidovém stavu. Je povolena na úrovni účtu úložiště hello. Se šifruje data, jako je zapsání v našich datacentrech a automaticky je dešifruje jako k němu přístup. Podporuje hello následující scénáře:

- Šifrování objektů BLOB bloku, doplňovací objekty BLOB, objekty BLOB stránky
- Šifrování archivovaný virtuální pevné disky a šablony brzy tooAzure z místní
- Šifrování discích operačního systému a dat pro virtuální počítače IaaS, kterou jste vytvořili pomocí virtuální pevné disky

Než přikročíte k šifrování úložiště Azure, nezapomeňte dvě omezení:

- Není k dispozici na klasické účty úložiště.
- Se šifruje jenom data, zapsaná po je povolené šifrování.

## <a name="use-a-centralized-security-management-system"></a>Použít systém správy centralizované zabezpečení

Vaše servery potřebovat toobe monitorovat opravy, konfiguraci, události a aktivity, které může považovat za z důvodů zabezpečení. tooaddress těch, které se týká, můžete použít [Security Center](https://azure.microsoft.com/services/security-center/) a [Operations Management Suite zabezpečení a dodržování předpisů](https://azure.microsoft.com/services/security-center/). Obě tyto možnosti překročila hello konfigurace operačního systému hello. Obsahují taky monitorování hello konfiguraci základní infrastruktury, jako je konfigurace sítě a virtuální zařízení použijte hello.

## <a name="manage-operating-systems"></a>Správa operačních systémů

V nasazení IaaS jsou stále odpovědní za správu hello hello systémů, které nasadíte, stejně jako libovolný server nebo pracovní stanice ve vašem prostředí. Opravy, posílení zabezpečení, přiřazení práv a dalších aktivit souvisejících s toohello údržbu systému jsou stále vaší povinností. Pro systémy, které jsou pevně integrovány s místní prostředky, můžete toouse hello stejné nástroje a postupy, které používáte místní pro věcmi, jako jsou antivirové programy, proti malwaru, opravy a zálohování.

### <a name="harden-systems"></a>Posílení zabezpečení systémů
Všechny virtuální počítače v Azure IaaS by měl být zesílené zabezpečení, tak, aby se vystavit pouze koncové body služby, které jsou požadovány pro hello aplikací, které jsou nainstalovány. Virtuální počítače s Windows, postupujte podle doporučení hello, které společnost Microsoft publikuje jako směrných plánů pro hello [Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) řešení.

Security Compliance Manager je bezplatný nástroj. Můžete ho tooquickly nakonfigurovat a spravovat stolní počítače, tradičního datacentra a privátních a veřejných cloudů pomocí zásad skupiny a System Center Configuration Manager.

Security Compliance Manager poskytuje připravené nasadit zásady a Desired Configuration Management Pack konfigurace, které jsou testovány. Tyto standardní hodnoty jsou založeny na [doprovodné materiály zabezpečení Microsoft](https://technet.microsoft.com/en-us/library/cc184906.aspx) doporučení a odvětví osvědčené postupy. Pomáhají spravovat odlišily konfigurace, požadavky na dodržování předpisů adresu, a snížit bezpečnostní hrozby.

Můžete tooimport Security Compliance Manager hello aktuální konfiguraci počítače pomocí dvou různých metod. První můžete importovat zásady skupiny služby Active Directory. Druhý, můžete importovat konfigurační hello "zlatá hlavní" referenčního počítače pomocí hello [LocalGPO nástroj](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) tooback hello zásad místní skupiny. Potom můžete importovat hello místní zásady skupiny do správce dodržování předpisů zabezpečení.

Porovnání vaše standardy tooindustry osvědčené postupy, si je přizpůsobit a vytvořte nové zásady a balíčky konfigurace Desired Configuration Management. Standardní hodnoty byly publikovány pro všechny podporované operační systémy, včetně Windows 10 Anniversary Update a Windows Server 2016.


### <a name="install-and-manage-antimalware"></a>Instalace a správa antimalwarových

Pro prostředí, které jsou hostované odděleně od provozního prostředí, můžete použít rozšíření toohelp antimalwarových chránit virtuální počítače a cloudové služby. Se integruje [Azure Security Center](../security-center/security-center-intro.md).


[Microsoft Antimalware](azure-security-antimalware.md) zahrnuje funkcí, jako je ochrana v reálném čase, naplánovanou kontrolu, malwarové nápravy, podpisu aktualizací, aktualizací stroje, ukázky vytváření sestav, shromažďování událostí vyloučení, a [podpora prostředí PowerShell](https://msdn.microsoft.com/library/dn771715.aspx).

![Azure proti malwaru](./media/azure-security-iaas/azantimalware.png)

### <a name="install-hello-latest-security-updates"></a>Nainstalujte nejnovější aktualizace zabezpečení hello
Některé hello první úloh, aby zákazníci přesunout tooAzure jsou labs a externího systémy. Pokud virtuální počítače Azure hostované hostitelem aplikace nebo služby, které je třeba toobe přístupné toohello Internet, být mimořádnou pozornost opravy. Oprava nad rámec hello operačního systému. Neopravenými chybami v aplikacích třetích stran, může také dojít tooproblems, které se můžete vyhnout, pokud je Správa dobrý oprava na místě.

### <a name="deploy-and-test-a-backup-solution"></a>Nasaďte a otestujte řešení zálohování

Stejně jako aktualizace zabezpečení, musí zálohu toobe zpracovává hello stejným způsobem, že zpracovat jiná operace. To platí pro systémy, které jsou součástí vašeho produkčního prostředí rozšíření toohello cloudu. Test a vývojářů systémy musí následovat strategii zálohování, které zajišťují, že možnosti obnovení, které jsou podobné toowhat uživatelé zvykli, o svých zkušenostech s místními prostředími.

Úlohy v produkčním prostředí přesunuty tooAzure by měl integrovat existující řešení zálohování, pokud je to možné. Nebo můžete použít [Azure Backup](../backup/backup-azure-arm-vms.md) toohelp adres požadavky zálohy.


## <a name="monitor"></a>Monitorování

[Security Center](../security-center/security-center-intro.md) poskytuje probíhající vyhodnocení hello stav zabezpečení vašich prostředků Azure tooidentify potenciální ohrožení zabezpečení. Seznam doporučení vás provede procesem konfigurace potřebných kontrol hello.

Příklady obsahují:

- Zřizování antimalwaru, toohelp identifikovat a odebrat škodlivý software.
- Konfigurace zabezpečení skupiny a pravidel toocontrol provoz toovirtual počítače sítě.
- Zřizování brány firewall webových aplikací toohelp bránit proti útokům, které cílí na vaše webové aplikace.
- Nasazení chybějících aktualizací systému.
- Adresování konfigurací operačního systému, které neodpovídají hello doporučuje směrných plánů.

Hello následující obrázek ukazuje některé hello možnosti, které se dají povolit ve službě Security Center.

![Zásad Azure Security Center](./media/azure-security-iaas/security-center-policies.png)

[Služby Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) je Microsoft cloudové IT správu řešení, které pomáhá spravovat a chránit místní a cloudové infrastruktury. Protože Operations Management Suite je implementovaný jako cloudová služba, dá se nasadit rychle a s minimálními investic do prostředků infrastruktury.

Nové funkce jsou automaticky doručit vyhnout se tak průběžnou údržbu a upgradovat náklady. Služby Operations Management Suite je integrován s nástrojem System Center Operations Manager. Obsahuje různé součásti toohelp lépe spravovat Azure úlohy, včetně [zabezpečení a dodržování předpisů](../operations-management-suite/oms-security-getting-started.md) modulu.

Funkce zabezpečení a dodržování předpisů hello můžete použít v Operations Management Suite tooview informace o vašich prostředků. Hello informace jsou rozděleny do čtyř hlavních kategorií:

- **Zabezpečení domény**: dále prozkoumat záznamy zabezpečení v čase. Přístup k posouzením malwaru, informace o aktualizacích hodnocení, informace o zabezpečení sítě, přístupu a identit a počítače s událostmi zabezpečení. Výhodou řídicí panel Azure Security Center toohello rychlý přístup.
- **Významné problémy**: rychle identifikovat hello počet aktivní problémy a hello závažnost těchto problémů.
- **Detekce (preview)**: identifikovat útok vzory vizualizací výstrahy zabezpečení nimž dochází před vašich prostředků.
- **Hrozby intelligence**: identifikovat útok vzory vizualizací hello celkový počet servery s odchozím škodlivým provozem IP hello typ zjištění ohrožení a mapu, která ukazuje, kde jsou tyto IP adresy pocházejících z.
- **Běžné dotazy zabezpečení**: najdete seznam nejčastějších zabezpečení hello dotazy, které můžete použít toomonitor prostředí. Když kliknete na jednu z těchto dotazů, hello **vyhledávání** okno otevře a zobrazí výsledky hello tento dotaz.

Hello následující snímek obrazovky ukazuje příklad hello informace, které můžete zobrazit Operations Management Suite.

![Směrné plány zabezpečení Operations Management Suite](./media/azure-security-iaas/oms-security-baseline.png)



## <a name="next-steps"></a>Další kroky


* [Blog týmu zabezpečení Azure](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Středisko Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Osvědčené postupy zabezpečení Azure a vzory](security-best-practices-and-patterns.md)
