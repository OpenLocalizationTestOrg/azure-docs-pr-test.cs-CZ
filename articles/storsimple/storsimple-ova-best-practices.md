---
title: "aaaBest postupy pro pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje hello osvědčené postupy pro nasazení a správě hello pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 57ac6eeb-c47c-442d-a5f4-b360d81a76a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2017
ms.author: alkohli
ms.openlocfilehash: f242364365e07f86b78e2f9bb2acfb3aa98e83e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-best-practices"></a>Osvědčené postupy pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled
Microsoft Azure StorSimple virtuální pole je řešení integrované úložiště, který spravuje úlohy úložiště mezi virtuální zařízení s místně spuštěnou v hypervisoru a cloudového úložiště Microsoft Azure. Pole virtuální zařízení StorSimple je efektivní a nákladově efektivní alternativní toohello 8000 fyzické pole řady. pole virtuálním Hello můžete spustit na vaší stávající infrastruktury hypervisoru, podporuje hello iSCSI a protokoly SMB hello a pro vzdálené office/pobočkách vhodné. Další informace o řešení StorSimple hello přejděte příliš[přehled Microsoft Azure StorSimple](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Tento článek popisuje osvědčené postupy hello implementována během počáteční instalace hello, nasazení a správu hello pole virtuální zařízení StorSimple. Tyto doporučené postupy pro poskytují ověřené pokyny pro nastavení hello a správu virtuálních pole. Tento článek je určeno směrem hello IT správce, kteří nasadit a spravovat hello virtuální pole ve svých datacentrech.

Doporučujeme, abyste pravidelných kontrol hello osvědčené postupy toohelp Ujistěte se, že zařízení je stále v dodržování předpisů při změně toohello instalační nebo provozní toku. Měli jste dojde k potížím při implementaci těchto osvědčených postupů na vaše virtuální pole [kontaktovat Microsoft Support](storsimple-virtual-array-log-support-ticket.md) o pomoc.

## <a name="configuration-best-practices"></a>Osvědčenými postupy konfigurace
Tyto doporučené postupy zahrnují hello pokyny, které je třeba toobe a potom během počáteční instalace hello a nasazení virtuálních polí hello. Tyto doporučené postupy zahrnují tyto související toohello zřizování hello virtuálního počítače, nastavení zásad skupiny, změna velikosti, nastavení hello sítě, konfiguraci účtů úložiště a vytvoření sdílených složek a svazků pro hello virtuální pole. 

### <a name="provisioning"></a>Zřizování
Pole virtuální zařízení StorSimple je virtuální počítač (VM) zřízený hello hypervisoru (Hyper-V nebo VMware) hostitelského serveru. Při zřizování hello virtuálního počítače, ujistěte se, že váš hostitel je schopný toodedicate dostatek prostředků. Další informace, přejděte příliš[minimální požadavky na zdroje](storsimple-virtual-array-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements) tooprovision pole.

Implementace hello následující osvědčené postupy při zřizování virtuální pole hello:

|  | Hyper-V | VMware |
| --- | --- | --- |
| **Typ virtuálního počítače** |**2. generace** virtuálního počítače pro použití se systémem Windows Server 2012 nebo novější a *.vhdx* bitové kopie. <br></br> **Generace 1** virtuálního počítače pro použití se systémem Windows Server 2008 nebo novějším a *VHD* bitové kopie. |Při použití použít virtuální počítač verze 8-11 *vmdk* bitové kopie. |
| **Typ paměti** |Konfigurace jako **statická paměť**. <br></br> Nepoužívejte hello **dynamické paměti** možnost. | |
| **Datový typ disku** |Zřízení jako **dynamicky se zvětšující**.<br></br> **Pevná velikost** trvá příliš dlouho. <br></br> Nepoužívejte hello **rozdílových** možnost. |Použití hello **dynamického zřizování** možnost. |
| **Úprava dat disku** |Rozšíření nebo zmenšení není povoleno. Pokusu o toodo proto výsledkem hello ke ztrátě všech hello místní data na zařízení. |Rozšíření nebo zmenšení není povoleno. Pokusu o toodo proto výsledkem hello ke ztrátě všech hello místní data na zařízení. |

### <a name="sizing"></a>Nastavení velikosti
Když pro změnu velikosti pole virtuální zařízení StorSimple, zvažte hello následující faktory:

* Místní rezervaci pro svazky nebo sdílené složky. Přibližně 12 % prostoru hello je rezervovaná na hello místní vrstvy pro každou zřízené vrstvený svazek nebo sdílenou složku. Také je přibližně 10 % místa hello vyhrazený pro místně vázaný svazek pro systém souborů.
* Režie snímku. Přibližně 15 % místa na místní úrovni hello je vyhrazená pro snímky.
* Třeba pro obnovení. Pokud to obnovit jako novou operaci, nastavení velikosti by měl účet pro prostor hello potřebná pro obnovení. Obnovení se provádí tooa sdílení nebo svazku hello stejnou velikost.
* Některé vyrovnávací paměti by měla být přidělená pro všechny neočekávané růstu.

Podle předchozích faktorech hello, hello dimenzování požadavky může být reprezentovaný hello následující rovnice:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`

Hello následující příklady znázorňují, jak můžete velikost virtuálního pole podle svých požadavků.

#### <a name="example-1"></a>Příklad 1:
Na vaše virtuální pole chcete toobe moct

* zřídit 2 TB vrstvený svazek nebo sdílenou složku.
* zřídit 1 TB vrstvený svazek nebo sdílenou složku.
* zřídit 300 GB místně vázaný svazek nebo sdílenou složkou.

Pro hello předchozí svazky nebo sdílené složky, dejte nám vypočítat požadavky na místo hello na místní úrovni hello.

Pro každý vrstvený svazek nebo sdílenou složku, nejdřív místní rezervace by rovna too12 % velikosti hello sdílení nebo svazku. Pro hello místně vázaný svazek nebo sdílenou složku místní rezervace je 10 % hello místně připnutý velikost svazku nebo sdílené složky (kromě toohello zřízený velikost). V tomto příkladu potřebujete

* Místní rezervace 240 GB (2 TB víceúrovňová sdílení nebo svazku)
* Místní rezervace 120 GB (1 TB víceúrovňová sdílení nebo svazku)
* 330 GB pro místně vázaný svazek nebo sdílenou složku (přidání 10 % velikosti 300 GB zřízený toohello místní rezervace)

Hello celkový obsahuje požadované místo na místní úrovni hello, pokud je: 240 GB + 120 GB + 330 GB = 690 GB.

Druhý potřebujeme alespoň tolik místa na místní úrovni hello jako největší jeden rezervace hello. Toto množství navíc se používá v případě, že potřebujete toorestore z cloudový snímek. V tomto příkladu je největší místní rezervace hello 330 GB (včetně rezervace pro systém souborů), měli byste přidat tento toohello 690 GB: 690 GB + 330 GB = 1020 GB.
Pokud jsme provedli následující další obnovení, jsme vždy uvolněte místo hello z předchozí operace obnovení hello.

Třetí, potřebujeme 15 % na celkový místní místo dosavadní toostore místní snímky, aby pouze 85 % je k dispozici. V tomto příkladu, který byl kolem 1020 GB = 0.85&ast;zřízené data na disku TB. Ano, bude hello zřízené datový disk (1020&ast;(1/0.85)) = 1 200 GB = 1,20 TB ~ 1,25 TB (zaokrouhlení QUARTIL toonearest)

Řešení v neočekávané růstu a nové obnovení, by měl zřídit místní disk kolem 1,25-1,5 TB.

> [!NOTE]
> Doporučujeme také, že je tence zřízený tento hello místní disk. Toto doporučení se totiž hello obnovení prostor je potřebný jenom, když chcete, aby toorestore data, která je starší než pět dní. Obnovení na úrovni položek vám umožní toorestore data pro hello posledních pět dnů bez nutnosti hello místo navíc je pro obnovení.


#### <a name="example-2"></a>Příklad 2:
Na vaše virtuální pole chcete toobe moct

* zřídit 2 TB vrstvené svazku
* 300 GB místně vázaný svazek zřídit

Podle % 12 rezervace volné místo pro vrstvené svazky nebo sdílené složky a 10 % místně vázaných svazků nebo sdílených složek, potřebujeme

* Místní rezervace 240 GB (2 TB víceúrovňová sdílení nebo svazku)
* 330 GB pro místně vázaný svazek nebo sdílenou složku (přidání 10 % místní rezervace toohello zřízený 300 GB místa)

Celkové požadované místo na místní vrstvy hello je: 240 GB + 330 GB = 570 GB

minimální volné místo Hello potřebná pro obnovení je 330 GB.

15 % celkové disku je použité toostore snímky tak, aby pouze 0.85 je k dispozici. Ano, je velikost disku hello (900&ast;(1/0.85)) = 1.06 TB ~ 1,25 TB (zaokrouhlení QUARTIL toonearest)

Řešení v jakékoli neočekávané růst, můžete zřídit na místní disk 1,25-1,5 TB.

### <a name="group-policy"></a>Zásady skupiny
Zásady skupiny jsou infrastruktura, která vám umožní tooimplement konkrétní konfigurace pro uživatele a počítače. Nastavení zásad skupiny jsou obsažené v objektech zásad skupiny (GPO), které jsou propojené toohello následující kontejnery služby Active Directory Domain Services (AD DS): weby, domény nebo organizační jednotky (OU). 

Pokud vaše virtuální pole je připojený k doméně, může být objekty zásad skupiny použité tooit. Tyto objekty zásad skupiny můžete nainstalovat aplikace, jako je antivirový software, který může nepříznivě ovlivnit hello operace hello pole virtuální zařízení StorSimple.

Proto doporučujeme, aby vám:

* Zajistěte, aby byl virtuální pole ve vlastní organizační jednotku (OU) pro službu Active Directory.
* Ujistěte se, že žádné objekty zásad skupiny (GPO) jsou použité tooyour virtuální pole. Můžete zablokovat dědičnost tooensure, který hello virtuální pole (podřízený uzel) z nadřazené hello automaticky nedědí žádnými objekty zásad skupiny. Další informace, přejděte příliš[zablokovat dědičnost](https://technet.microsoft.com/library/cc731076.aspx).

### <a name="networking"></a>Sítě
Hello konfiguraci sítě pro vaše virtuální pole se provádí prostřednictvím hello místního webového uživatelského rozhraní. Rozhraní virtuální sítě je povolená díky hello hypervisoru, ve kterém je zřízený virtuální pole hello. Použití hello [nastavení sítě](storsimple-virtual-array-deploy3-fs-setup.md) stránka tooconfigure hello virtuální sítě rozhraní IP adresu podsítě a bránu.  Můžete také konfigurovat hello primární a sekundární server DNS, nastavení času a volitelná nastavení proxy serveru pro vaše zařízení. Většinu konfiguraci sítě hello je jednorázové instalační program. Zkontrolujte hello [StorSimple sítě požadavky](storsimple-ova-system-requirements.md#networking-requirements) předchozí toodeploying hello virtuální pole.

Pokud nasazujete virtuální pole, doporučujeme dodržovat tyto osvědčené postupy:

* Zajistěte, aby byl tuto hello síť, ve které hello virtuální pole je nasazena vždy hello kapacity toodedicate 5 šířky pásma Internetu MB/s (nebo více).
  
  * Třeba šířky pásma Internetu se liší v závislosti na charakteristiky zatížení a hello rychlosti změny dat.
  * Hello změny dat, která může být zpracována je přímo úměrná tooyour šířky pásma Internetu. Například při zálohování 5 MB/s šířky pásma zvládne přibližně 18 GB změny dat v 8 hodin. Čtyřikrát větší šířka pásma (20 MB/s) může zpracovávat čtyřikrát další změny dat (72 GB).
* Zajistěte, aby toohello připojení k Internetu je vždy k dispozici. Výskyt občasný, nebo nespolehlivé internetové připojení toohello zařízení může mít za následek ztrátu přístupu toodata v cloudu hello a může mít za následek nepodporované konfigurace.
* Pokud máte v plánu toodeploy zařízení jako iSCSI server:
  
  * Doporučujeme zakázat hello **získat IP adresu automaticky** možnost (DHCP).
  * Konfigurace statické IP adresy. Je nutné nakonfigurovat primární a sekundární server DNS.
  * Pokud na vaše virtuální pole definování více síťových rozhraní, hello pouze první síťové rozhraní (ve výchozím nastavení, toto rozhraní je **Ethernet**) dosáhnout hello cloudu. Typ hello toocontrol přenosů dat, můžete vytvořit více rozhraní virtuální sítě na vaše virtuální pole (nakonfigurovaný jako server se službou iSCSI) a připojení těchto podsítí toodifferent rozhraní.
* toothrottle hello cloudu šířky pásma pouze (používá pole virtuálním hello), konfigurace omezení na hello směrovače nebo brány firewall hello. Pokud definujete omezení ve vašem hypervisoru, bude ho omezení všechny protokoly hello včetně iSCSI a SMB místo právě šířky pásma cloudu hello.
* Ujistěte se, že synchronizaci času pro hypervisory je povoleno. Pokud pomocí technologie Hyper-V, vybrat virtuální pole v hello Správce technologie Hyper-V, přejděte příliš**nastavení &gt; integrační služby**a ujistěte se, že hello **synchronizace času** je zaškrtnuté.

### <a name="storage-accounts"></a>Účty úložiště
Pole virtuální zařízení StorSimple může být přidružena k účtu jedno úložiště. Tento účet úložiště může být účet úložiště automaticky generovaného účtu v hello stejnému předplatnému jako hello služby nebo účet úložiště související tooanother předplatné. Další informace najdete v tématu Jak příliš[Správa účtů úložiště pro vaše virtuální pole](storsimple-virtual-array-manage-storage-accounts.md).

Následující doporučení pro účty úložiště přidružené virtuální pole použití hello.

* Při propojování více virtuální pole s účtem jedno úložiště, dvoufaktorového hello maximální kapacity (64 TB) pro virtuální pole a maximální velikost hello (500 TB) pro účet úložiště. Toto nastavení omezuje počet hello v plné velikosti virtuální pole, které může být přidružen účet tooabout tohoto úložiště 7.
* Při vytváření nového účtu úložiště
  
  * Doporučujeme vytvořit hello oblast nejbližší toohello vzdálené office/pobočce kde pole virtuální zařízení StorSimple je nasazené toominimize latenci.
  * Berte v úvahu, že nemůžete přesunout účet úložiště v různých oblastech. Službu nelze také přesouvat mezi odběry.
  * Použijte účet úložiště, který implementuje redundance mezi datovými centry hello. GEO-Redundant Storage (GRS), zóny redundantní úložiště (ZRS) a místně redundantní úložiště (LRS) jsou všechny podporované pro použití s virtuální pole. Další informace o hello různé typy účtů úložiště, přejděte příliš[replikace Azure storage](../storage/common/storage-redundancy.md).

### <a name="shares-and-volumes"></a>Sdílené složky a svazky
Pro pole virtuální zařízení StorSimple můžete zřídit sdílené složky, když je nakonfigurovaný jako souborový server a svazky, když je nakonfigurovaný jako server se službou iSCSI. Typ velikosti a hello toohello nakonfigurovat jsou příslušných Hello osvědčené postupy pro vytváření sdílených složek a svazky.

#### <a name="volumeshare-size"></a>Velikost svazku nebo sdílené složky
Na virtuální pole můžete zřídit sdílené složky, když je nakonfigurovaný jako souborový server a svazky, když je nakonfigurovaný jako server se službou iSCSI. Hello osvědčené postupy pro vytváření sdílených složek a svazky se týkají toohello velikost a typ konfigurovaný hello. 

Mějte na paměti hello následující osvědčené postupy při zřizování sdílené složky nebo svazky na virtuální zařízení.

* Hello velikosti relativní toohello zřízený velikost souboru vrstvené sdílená složka může ovlivnit výkon vrstvení hello. Práce s velkými soubory by mohla způsobit pomalé vrstvy. Při práci s velkých souborů, doporučujeme, že soubor největší hello je menší než 3 % hello velikost sdílené složky.
* V poli virtuálním hello lze vytvořit maximálně 16 svazky nebo sdílené složky. Omezení velikosti hello hello místně připnutý a vrstvené svazky nebo sdílené složky, vždy naleznete toohello [omezuje pole virtuální zařízení StorSimple](storsimple-ova-limits.md).
* Při vytváření svazku, faktorem spotřeby dat hello očekávání, jakož i budoucímu růstu. Hello svazek nelze rozšířit později.
* Po vytvoření svazku hello nelze zmenšit velikost hello hello svazek v zařízení StorSimple.
* Při zápisu tooa vrstvené svazek v zařízení StorSimple, když data hello svazek dosáhne určitým prahem (relativní toohello místní místa vyhrazeného pro svazek hello), je omezen hello vstupně-výstupní operace. Pokračováním toowrite toothis svazku zpomaluje hello vstupně-výstupní operace výrazně. I když můžete napsat tooa vrstvené svazku nad rámec jeho zřízená kapacita (jsme nedojde k zastavení aktivně hello uživatele z zápis nad hello zřídit kapacitu), najdete v části vliv toohello oznámení výstrah, které mají oversubscribed. Jakmile se zobrazí výstraha hello, je nutné podniknout nápravné opatření, například odstranit data svazku hello (svazku rozšíření není aktuálně podporována.).
* Pro případy použití pro zotavení po havárii jako je hello počet povolených sdílené složky nebo svazky 16 a hello maximální počet sdílené složky nebo svazky, které lze zpracovat paralelní je také 16, hello počtu sdílených složek nebo svazků nemá vliv na plánovaný bod obnovení a RTO.

#### <a name="volumeshare-type"></a>Typ svazku nebo sdílené složky
StorSimple podporuje dva typy sdílení nebo svazku na základě využití hello: místně ukotvena a vrstvené. Místně vázaných svazků nebo sdílených složek je tlustě zřízený, zatímco hello vrstvené svazky nebo sdílené složky jsou dynamicky zajišťované. Nelze převést tootiered místně vázaný svazek nebo sdílenou složku nebo *naopak* po vytvoření.

Doporučujeme vám, že můžete implementovat hello následující osvědčené postupy při konfiguraci StorSimple svazky nebo sdílené složky:

* Určete typ svazku hello podle hello úlohy, který chcete toodeploy, před vytvořením svazku. Použijte pro úlohy, které vyžadují místní záruky dat (i během výpadku cloud) a které vyžadují, nízkou cloudu latence místně vázaných svazků. Jakmile vytvoříte svazek na vaše virtuální pole, nelze změnit typ svazku hello z místně vázaný tootiered nebo *naopak*. Jako příklad vytváření místně vázaných svazků při nasazování SQL úlohy nebo úlohy hostování virtuálních počítačů (VM); použijte pro úlohy sdílené složky souboru vrstvené svazky.
* Zkontrolujte hello možnost pro archivní data s méně často používaných při plánování práce s velikostí velkých souborů. Větší velikost bloku dat odstranění duplicit 512 kB se používá, pokud je tato možnost povolena příchozí přenos dat hello tooexpedite toohello cloudu.

#### <a name="volume-format"></a>Formát svazku
Po vytvoření svazky zařízení StorSimple na vašem serveru iSCSI, budete potřebovat tooinitialize, připojení a formát hello svazky. Tato operace provádí na zařízení StorSimple tooyour hostování připojené hello. Doporučujeme následující osvědčené postupy při připojení a formátování svazků zařízení StorSimple hostiteli hello.

* Proveďte rychlé formátování na všechny svazky zařízení StorSimple.
* Při formátování svazek StorSimple, použijte velikost alokační jednotky (Austrálie) 64 kB (výchozí hodnota je 4 KB). Hello 64 KB Austrálie je založena na testování provádí interně pro běžné úlohy StorSimple a další úlohy.
* Při použití hello pole virtuální zařízení StorSimple nakonfigurovaný jako server se službou iSCSI, nepoužívejte rozložené svazky nebo dynamické disky jako tyto svazky nebo disky nejsou podporovány StorSimple.

#### <a name="share-access"></a>Přístup ke sdílené složce
Při vytváření sdílené složky na souborovém serveru virtuální pole, postupujte podle těchto pokynů:

* Když vytváříte sdílenou složku, přiřadíte skupinu uživatelů jako správce a sdílené složky namísto jednoho uživatele.
* Po vytvoření sdílené složky hello úpravou hello sdílených složek pomocí Průzkumníka Windows, můžete spravovat oprávnění NTFS hello.

#### <a name="volume-access"></a>Přístup ke svazku
Při konfiguraci hello iSCSI svazky na pole virtuální zařízení StorSimple, je důležité toocontrol přístup, kdykoli je to nutné. toodetermine, která je hostitelem serverů můžete získat přístup ke svazkům, vytvořit a přidružit svazky zařízení StorSimple záznamy řízení přístupu (ACRs).

Použijte následující osvědčené postupy při konfiguraci ACRs pro svazky zařízení StorSimple hello:

* Alespoň jeden ACR vždy přidružte svazku.

* Při přiřazování více než jeden svazek tooa ACR, ujistěte se, že hello svazek není vystavení způsobem, kde můžete současně přistupovat ve více než jeden neclusterovaného hostitele. Pokud jste přiřadili více ACRs tooa svazku, upozornění se zobrazí pro tooreview můžete konfiguraci.

### <a name="data-security-and-encryption"></a>Zabezpečení dat a šifrování
Pole virtuální zařízení StorSimple je funkce zabezpečení a šifrování dat, které zajištění důvěrnosti hello a integritu dat. Při používání těchto funkcí, doporučujeme dodržovat tyto osvědčené postupy: 

* Před odesláním dat hello z vašeho cloudu toohello virtuální pole, definujte toogenerate klíče AES 256 šifrování úložiště cloudu. Tento klíč se nevyžaduje, pokud vaše data jsou šifrovaná toobegin s. Hello klíč může generovat a uchovávat bezpečné pomocí systémem správy klíčů, jako třeba [Azure trezoru klíčů](../key-vault/key-vault-whatis.md).
* Při konfiguraci účtu úložiště hello prostřednictvím hello služby StorSimple Manager, ujistěte se, abyste povolili toocreate režimu SSL hello zabezpečený kanál pro síťovou komunikaci mezi zařízení StorSimple a hello cloudu.
* Opětovné vytváření hello klíče pro účty úložiště (podle přístup ke službě Azure Storage hello) pravidelně tooaccount pro jakékoli změny tooaccess podle hello změnit seznamu správců.
* Data na vaše virtuální pole jsou komprimované a odstranění duplicit před odesláním tooAzure. Nedoporučujeme používat službu role odstranění duplicitních dat hello na hostiteli s Windows Server.

## <a name="operational-best-practices"></a>Aplikovatelné nejlepší postupy
Hello aplikovatelné nejlepší postupy jsou pokyny, které musí být sledována během operace hello virtuální pole nebo hello každodenní správu. Tyto postupy zahrnují určité úlohy správy například umožňoval vytvářet zálohy, obnovení ze zálohovacího skladu, provádění převzetí služeb při selhání, deaktivace a odstraňování hello pole, monitorování využití systémové a stavu, a spouští virů kontrolu na vaše virtuální pole.

### <a name="backups"></a>Zálohování
Hello data na vaše virtuální pole je zálohována toohello cloudu dvěma způsoby, výchozí automatizované denní zálohování hello zařízení spouštění ve 22:30 nebo prostřednictvím ručního zálohování na vyžádání. Ve výchozím nastavení hello zařízení automaticky vytvoří denní cloudové snímky dat hello umístěný na něm. Další informace, přejděte příliš[zálohování pole virtuální zařízení StorSimple](storsimple-virtual-array-backup.md).

Hello četnost a uchování přidružené hello výchozí zálohování nelze změnit, ale můžete konfigurovat hello dobu, na které hello denní zálohy se spouští každý den. Při konfiguraci hello počáteční čas pro hello automaticky zálohování, doporučujeme:

* Plán zálohování pro hodiny mimo špičku. Pokud čas spuštění zálohování se neshoduje se množství hostitele vstupně-výstupní operace.
* Při plánování tooperform zahájit převzetí služeb při selhání zařízení nebo předchozí toohello časové období údržby, tooprotect hello dat na vaše virtuální pole ruční zálohy na vyžádání.

### <a name="restore"></a>Obnovení
Můžete obnovit ze zálohy nastavit dvěma způsoby: Obnovte tooanother svazek nebo sdílenou složku nebo provést obnovení na úrovni položek (k dispozici pouze u virtuálních pole nakonfigurovaný jako souborový server). Obnovení na úrovni položek umožňuje toodo podrobné obnovení souborů a složek ze záloh cloudu všechny hello sdílené složky v zařízení StorSimple hello. Další informace, přejděte příliš[obnovit ze zálohy](storsimple-virtual-array-clone.md).

Při provádění obnovení, Udržovat hello pokyny na paměti následující:

* Pole virtuální zařízení StorSimple nepodporuje obnovení na místě. To může ale být snadno dosáhnout dvoustupňový proces: Uvolněte na virtuálním hello pole a potom obnovte tooanother svazek nebo sdílenou složku.
* Při obnovování na místním svazku, mějte na paměti hello obnovení bude operace probíhající dlouhou dobu. I když hello svazku může rychle režimu online, pokračuje hello data toobe HYDRATOVANÝ hello pozadí.
* Hello svazku typu zůstane stejný hello během procesu obnovení hello. Vrstvený svazek je obnoven tooanother vrstvené svazky tooanother místně vázaný místně vázaný svazek a.
* Při pokusu o toorestore svazek nebo sdílenou složku ze zálohovacího skladu, pokud se nezdaří úlohy obnovení hello, cílový svazek nebo sdílenou složku stále možné vytvářet hello portálu. Je důležité odstranit tohoto nepoužívané cílového svazku nebo sdílené složky v portálu toominimize hello budoucí potíží vzniklých tohoto elementu.

### <a name="failover-and-disaster-recovery"></a>Převzetí služeb při selhání a zotavení po havárii
Selhání zařízení vám umožní toomigrate svá data z *zdroj* zařízení v datovém centru tooanother hello *cíl* zařízení nachází v hello stejné nebo jiné zeměpisné umístění. Hello zařízení převzetí služeb při selhání je pro zařízení hello. Data v cloudu hello hello zdrojového zařízení během převzetí služeb při selhání, změní vlastnictví toothat hello cílové zařízení.

Pro pole virtuální zařízení StorSimple, můžete pouze převzetí služeb při selhání virtuální pole tooanother spravuje hello stejné služby StorSimple Manager. Zařízení řady tooan 8000 převzetí služeb při selhání nebo pole spravovány jinou službu StorSimple Manager (než hello jednu pro hello zdrojového zařízení) není povoleno. toolearn Další informace o hello aspekty převzetí služeb při selhání, přejděte příliš[požadavky pro převzetí služeb při selhání hello zařízení](storsimple-virtual-array-failover-dr.md).

Při provádění selhání přes pro vaše virtuální pole, mějte hello následující skutečnosti:

* Plánované převzetí služeb je doporučené osvědčené praxi tootake všechny hello svazky nebo sdílené složky offline předchozí tooinitiating hello převzetí služeb při selhání. Postupujte podle pokynů specifické pro operační systém hello tootake hello svazky nebo sdílené složky offline na hello nejprve hostitele a pak proveďte ty offline na virtuální zařízení.
* Souboru serveru zotavení po havárii (DR) doporučujeme hello cílové zařízení toohello automaticky vyřešeny stejné domény jako zdroj hello, který hello oprávnění ke sdílení připojení k. Pouze hello převzetí služeb při selhání tooa cílové zařízení v hello stejné domény je podporovaná v této verzi.
* Jakmile hello zotavení po Havárii je úspěšně dokončen, hello zdrojového zařízení se automaticky odstraní. I když hello zařízení již není k dispozici, je hello virtuálního počítače, kterou jste zřídili v systému hostitele hello nadále spotřebovávají prostředky. Doporučujeme vám, že odstraníte tento virtuální počítač z vaší tooprevent systému hostitele ze nabíhání poplatků.
* Všimněte si, že i když je úspěšné, převzetí služeb při selhání hello **hello dat je bezpečné vždy v cloudu hello**. Vezměte v úvahu hello následující tři scénáře, ve které hello převzetí služeb při selhání nebyla úspěšně dokončena:
  
  * Došlo k chybě v hello počáteční fáze hello převzetí služeb při selhání například když se provádí předběžné kontroly hello zotavení po Havárii. V takovém případě je stále možné použít cílové zařízení. Můžete zkusit hello převzetí služeb při selhání na hello stejné cílové zařízení.
  * Došlo k chybě během procesu hello skutečné převzetí služeb při selhání. V takovém případě je označena hello cílové zařízení nepoužitelný. Musíte zřídit a nakonfigurovat jiné cílové virtuální pole a použijte k převzetí služeb při selhání.
  * Hello převzetí služeb při selhání je úplná následující, které zařízení hello zdroj byl odstraněn, ale hello cílové zařízení má problémy a nemůžou přistupovat žádná data. Hello data se stále bezpečné v cloudu hello a dá snadno načíst vytvořením jiný virtuální pole a pak ho pomocí jako cílové zařízení pro zotavení po Havárii hello.

### <a name="deactivate"></a>Deaktivace
Pokud deaktivujete o pole virtuální zařízení StorSimple, můžete server hello připojení mezi hello zařízení a služby StorSimple Manager odpovídající hello. Deaktivace je **trvalé** operace a nelze ji vrátit zpět. Deaktivované zařízení nemůže být zaregistrován u hello služby StorSimple Manager znovu. Další informace, přejděte příliš[deaktivovat a odstranit pole virtuální zařízení StorSimple](storsimple-virtual-array-deactivate-and-delete-device.md).

Udržovat hello následující osvědčené postupy v paměti, když deaktivace virtuální pole:

* Pořízení snímku cloudu všechny hello data předchozí toodeactivating virtuálního zařízení. Když deaktivujete virtuální pole, dojde ke ztrátě všech dat hello místní zařízení. Pořízení snímku cloudu vám umožní toorecover data v pozdější fázi.
* Před deaktivací o pole virtuální zařízení StorSimple, ujistěte se, že toostop nebo odstranit klienty a hostitele, které jsou závislé na tomto zařízení.
* Odstraňte deaktivované zařízení, pokud už používáte, takže ho nebude nabíhat poplatky.

### <a name="monitoring"></a>Monitorování
tooensure, který je pole virtuální zařízení StorSimple v průběžné stavu v pořádku, je nutné toomonitor hello pole a ujistěte se, zda přijímají informace ze systému hello včetně výstrahy. toomonitor hello celkového stavu hello virtuální pole, implementujte hello následující osvědčené postupy:

* Konfigurace monitorování využití disku hello tootrack vaše virtuální pole datový disk, stejně jako disk s operačním systémem hello. Pokud s technologií Hyper-V, můžete použít kombinaci System Center Virtual Machine Manager (SCVMM) a System Center Operations Manager (SCOM) toomonitor virtualizační hostitele.
* Konfigurace e-mailová oznámení na vaše virtuální pole toosend výstrahy na určité úrovni využití.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Index vyhledávání a antivirové kontroly aplikací
Pole virtuální zařízení StorSimple můžete automaticky vrstvy data z hello místní vrstvy toohello cloudu Microsoft Azure. Pokud aplikace například prohledání indexu nebo antivirovou kontrolu je použité tooscan hello dat uložených na zařízení StorSimple, je potřeba tootake pozor, že data v cloudu hello nepodporuje získat přístup a vyžádat zpět toohello místní vrstvy.

Doporučujeme vám, že implementujete hello následující osvědčené postupy při konfiguraci hello indexu vyhledávání nebo antivirové kontroly na vaše virtuální pole:

* Zakažte všechny operace automaticky konfigurované úplnou kontrolu.
* Pro vrstvené svazky nakonfigurujte hello indexu vyhledávání nebo antivirové kontroly aplikací tooperform přírůstkové skenování. To by kontrolovat jenom hello nové dat pravděpodobně umístěný na místní vrstvy hello. během operace přírůstkové není přístup Hello data, která je vrstvené toohello cloudu.
* Zkontrolujte hello hledání správné filtry a nastavení jsou nakonfigurovaná tak, aby získat zkontrolovat pouze hello určené typy souborů. Například bitovou kopii souborů (JPEG, GIF a TIFF) a technické výkresy nesmí zkontrolován během opětovné sestavení indexu úplné nebo přírůstkové hello.

Pokud používáte Windows indexování procesu, postupujte podle následujících pokynů:

* Nepoužívejte Windows Indexer hello pro vrstvené svazky, protože vyhledává velké objemy dat (TBs) z cloudu hello, pokud potřebuje toobe často znovu sestavit hello index. Znovu sestavit hello index by načíst všechny typy tooindex soubor jejich obsah.
* Použijte Windows hello indexování proces místně vázaných svazků, jako to by pouze přístup k datům v hello místních vrstvách toobuild hello indexu (hello cloud, nebude mít přístup dat).

### <a name="byte-range-locking"></a>Zamykání rozsahu bajtů
Aplikace můžete uzamknout zadaný rozsah bajtů v rámci hello soubory. Pokud na hello aplikace, které jsou zápis tooyour StorSimple je povolený zámek rozsah bajtů, pak vrstvení nefunguje na vaše virtuální pole. Pro vrstvení toowork hello musí být všechny oblasti hello souborů přístup odemknout. Zamykání rozsah bajtů není podporována u vrstvené svazky na vaše virtuální pole.

Doporučujeme míry zamykání rozsah bajtů tooalleviate zahrnují:

* Vypněte rozsah bajtů zamykání logiky aplikace.
* Použití místně připnutý svazky (místo víceúrovňová) pro hello data přidružená k této aplikaci. Místně vázaných svazků není vrstvy do cloudu hello.
* Při použití místně vázaný svazky s povoleno uzamčení rozsah bajtů, můžou mít svazek hello online před dokončením hello obnovení. V těchto případech je nutné počkat hello obnovení toobe dokončení.

## <a name="multiple-arrays"></a>Několik polí
Několik polí virtuální může být nutné nasadit toobe tooaccount pro rostoucí pracovní sadu dat, která by mohla distribuována do cloudu hello tak ovlivnit výkon hello hello zařízení. V těchto případech je nejlepší tooscale zařízení s růstem hello pracovní sady. To vyžaduje jeden nebo více zařízení toobe přidali v hello místního datového centra. Při přidávání hello zařízení, může:

* Aktuální rozdělení hello sada data.
* Nasaďte nové úlohy toonew zařízení.
* Pokud nasazení několik virtuálních polí, doporučujeme, aby ze služby Vyrovnávání zatížení perspektivy, distribuci hello pole přes hostitele jiný hypervisoru.
* Několik virtuálních polí (Pokud je nakonfigurovaný jako souborový server nebo server se službou iSCSI) se dá nasadit v distribuované Namespace systému souborů. Podrobný postup je uveden příliš[distribuované soubor řešení systému Namespace s hybridní cloudové úložiště Deployment Guide](https://www.microsoft.com/download/details.aspx?id=45507). Replikaci distribuovaného systému souborů se aktuálně nedoporučuje používat s polem virtuálním hello. 

## <a name="see-also"></a>Viz také
Zjistěte, jak příliš[spravovat vaše pole virtuální zařízení StorSimple](storsimple-virtual-array-manager-service-administration.md) prostřednictvím hello služby StorSimple Manager.

