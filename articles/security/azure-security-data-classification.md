---
title: "aaaData klasifikaci pro službu Azure | Microsoft Docs"
description: "Tento článek poskytuje základní informace o klasifikaci dat toohello úvod a zvýrazňuje svou hodnotu, konkrétně v kontextu hello technologie cloud computing a pomocí Microsoft Azure"
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ROBOTS: NOINDEX
redirect_url: https://aka.ms/data-classification-cloud
ms.assetid: 91d4afed-b80f-4a26-b526-b52b9c858cfb
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/18/2017
ms.author: yurid
ms.openlocfilehash: 726da2482beab3bf7b0ac33510f2b523d5074df8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-classification-for-azure"></a>Klasifikace dat pro Azure.
Tento článek obsahuje základní informace o klasifikaci dat toohello úvod a zvýrazňuje svou hodnotu, konkrétně v kontextu hello technologie cloud computing a pomocí Microsoft Azure. 

## <a name="data-classification-fundamentals"></a>Základy klasifikace dat
Klasifikace úspěšné dat v organizaci, vyžaduje široký povědomí o potřebám vaší organizace a důkladné znalosti týkající se kde jsou uloženy vaše datové prostředky.  

Data v jednom ze tří stavů základní existuje: 

* V klidovém stavu 
* V procesu 
* Při přenosu 

Všechny tři stavy vyžadovat jedinečné technických řešení pro klasifikaci dat, ale hello použité zásady klasifikace dat by měl být hello stejné pro všechny. Data, která je klasifikován jako důvěrný musí toostay důvěrné při v klidovém stavu, v procesu a v provozu. 

Data mohou být také strukturovaných nebo nestrukturovaných. Procesy typické klasifikace pro hello strukturovaná data uvedená v databází a tabulek jsou méně složitý a zdlouhavý toomanage než ty, které pro Nestrukturovaná data, jako jsou dokumenty, zdrojový kód a e-mailu. 

> [!TIP]
> Další informace týkající se možnosti Azure a osvědčené postupy pro šifrování dat [osvědčené postupy pro šifrování dat Azure](azure-security-data-encryption-best-practices.md)
> 
> 

Obecně platí, organizace bude mít více než strukturovaných dat Nestrukturovaná data. Bez ohledu na to, zda jsou data strukturovaných nebo nestrukturovaných je důležité pro jste toomanage data velkých a malých písmen. Když je implementovaná správně, klasifikace dat pomáhá zajistit že citlivá nebo důvěrná data, prostředky se spravují pomocí dohledu větší než datové prostředky, které jsou považovány za veřejné nebo volné toodistribute. 

### <a name="controlling-access-toodata"></a>Řízení přístupu toodata
Ověřování a autorizace často matoucí mezi sebou a nesprávně pochopeny jejich rolí. Ve skutečnosti jsou výrazně lišit, jak ukazuje následující obrázek hello.  

![Přístup k datům a řízení](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Authentication
Ověřování se obvykle skládá z nejméně dvou částí: tooidentify uživatelské jméno nebo uživatelské ID, uživatele a token, jako například heslo, tooconfirm, který hello pověření uživatelské jméno je platný. proces Hello neposkytuje hello ověřeného uživatele pro přístup k položkám tooany nebo služby; ověří, že je tento uživatel hello, kdo říká se, že jsou.   

> [!TIP]
> [Azure Active Directory](../active-directory/active-directory-whatis.md) poskytuje služby cloudových identit, které umožňují tooauthenticate a autorizaci uživatelů. 
> 
> 

### <a name="authorization"></a>Autorizace
Autorizace je proces hello poskytnout tooaccess možnost hello ověřenému uživateli. aplikace, datové sady, datový soubor nebo některé další objekt. Přiřazení toouse práva hello ověřené uživatele, upravit nebo odstranit položky, které získají přístup k vyžaduje pozornost toodata klasifikace. 

Úspěšné autorizace vyžaduje tooaccess soubory a informace, které jsou založené na kombinaci role, zásady zabezpečení a důležité informace o riziko zásad je potřeba při implementaci mechanismus toovalidate jednotlivých uživatelů. Například data z konkrétní-obchodní (LOB) aplikace nemusí potřebovat toobe získat přístup všichni zaměstnanci a pouze malou podmnožinu zaměstnanci bude pravděpodobně nutné souborům toohuman zdrojů. Ale pro organizace toocontrol kdo přístup k datům, jako a taky kdy a jak, musí být účinný systém pro ověřování uživatelů na místě. 

> [!TIP]
> ve službě Microsoft Azure zkontrolujte že tooleverage řízení řízení přístupu (RBAC) toogrant pouze hello úroveň přístupu, tito uživatelé si musí tooperform svou práci. Čtení [použít roli přiřazení toomanage prostředků Azure Active Directory přístup tooyour](../active-directory/role-based-access-control-configure.md) Další informace. 
> 
> 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Role a zodpovědnosti v cloud computing
I když poskytovatelů cloudu může pomoct spravovat rizika, musí uživatelé tooensure této Správa klasifikace dat a je implementovaná správně vynucení tooprovide hello odpovídající úroveň služby pro data.  

Klasifikace dat, které odpovědnosti se bude lišit podle toho, které model cloudové služby na místě, jak ukazuje následující obrázek hello. tři modely služeb primárního cloudu Hello jsou infrastruktura jako služba (IaaS), platforma jako služba (PaaS) a software jako služba (SaaS). Implementace mechanismy pro klasifikaci dat bude také lišit v závislosti na hello spoléhat na a očekávání poskytovatele cloudové hello. 

![Role](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

I když jste zodpovědní za klasifikace dat, měli poskytovatele cloudových napsané závazky o tom, jak bude zabezpečení a udržují ochranu osobních hello hello zákaznická data uložená v jejich cloudu.  

* **Zprostředkovatelé IaaS** požadavky jsou omezené tooensuring, který hello virtuální prostředí zvládne funkcemi klasifikace dat a požadavky na dodržování předpisů zákazníka. Poskytovatelům IaaS mít menší roli v klasifikaci dat, protože stačí tooensure, že data zákazníků řeší požadavky na dodržování předpisů. Však zprostředkovatelé musí stále zajistit adres jejich virtuální prostředí požadavkům na klasifikaci dat v přidání toosecuring svých datových center.
* **Zprostředkovatelé PaaS** odpovědnosti může být ve smíšeném, protože hello platformy by mohly být použity v zabezpečení tooprovide vrstveného přístupu nástroj klasifikace. Zprostředkovatelé PaaS může být zodpovědná za ověřování a které by mohly mít některé autorizační pravidla a musí poskytnout zabezpečení a data klasifikace možnosti tootheir aplikační vrstvu. Mnohem jako IaaS poskytovatelů, třeba zprostředkovatelé PaaS tooensure, který splňuje jejich platformy s požadavky na klasifikaci všechny relevantní data.
* **Poskytovatelů SaaS** bude často by se zařadit jako součást řetězce autorizace a bude nutné tooensure, který hello data uložená v aplikaci SaaS hello se dá nastavit podle typu klasifikace. Aplikace SaaS lze použít pro aplikace LOB a ze své podstaty potřebovat tooprovide hello znamená tooauthenticate a autorizovat data, která se používá a uloží. 

## <a name="classification-process"></a>Proces klasifikace
Mnoho organizací, které pochopit hello potřebovat pro klasifikaci dat a chcete ho čelí základní výzvě tooimplement: kde toobegin?

Jeden efektivní a jednoduchý způsob tooimplement klasifikace dat je toouse hello plán, provést, ZKONTROLUJTE, ACT modelu z [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx). Hello následující obrázek grafy hello úlohy, které jsou požadované toosuccessfully implementace klasifikace dat v tomto modelu.  

1. **PLÁNOVÁNÍ**. Identifikujte datových prostředků, data správce toodeploy hello klasifikace program a vytvořte profily ochrany. 
2. **PROVEĎTE**. Po typové zásady klasifikace dat, nasaďte programu hello a implementovat technologie vynucení podle potřeby pro důvěrné údaje.  
3. **ZKONTROLUJTE**. Zkontrolujte a ověřit sestavy tooensure, který hello nástrojů a metod, které používá efektivně řeší hello zásady klasifikace. 
4. **AKCE**. Zkontrolujte stav hello přístup k datům a zkontrolujte soubory a data, která vyžadují revizi pomocí přeřazení a revize metodika tooadopt změn a nová rizika tooaddress.  

![Plánování,, zkontrolujte, fungují](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)

### <a name="select-a-terminology-model-that-addresses-your-needs"></a>Vyberte model terminologie, která řeší potřeb
Existuje několik typů procesů pro data, včetně manuální procesy, na základě polohy procesy, které klasifikaci dat podle umístění uživatele nebo systému, procesy založené na aplikaci, například klasifikace specifické pro databáze a automatickou klasifikaci procesy používané modulem různých technologií, z nichž některé jsou popsány v části "Ochrany důvěrných dat." hello později v tomto článku.  

Tento článek představuje dva modely zobecněný terminologie, které jsou založeny na dobře použité a dodrženy odvětví modely. Tyto terminologie modely, které poskytují tři úrovně klasifikace velkých a malých písmen, jsou uvedené v následující tabulka hello.  

> [!NOTE]
> při klasifikaci souboru či prostředku, že kombinuje, které by měl zřídit data, která by obvykle rozdělit na různé úrovně, hello nejvyšší úrovně klasifikace přítomen hello celkové klasifikace. Například by se měly klasifikovat soubor obsahující citlivé a s omezeným přístupem data jako s omezeným přístupem.  
> 
> 

| **Velkých a malých písmen** | **Terminologie modelu 1** | **Terminologie modelu 2** |
| --- | --- | --- |
| Vysoký |Důvěrné informace |Omezený |
| Střednědobé používání |Pouze pro interní použití |Citlivé |
| Nízký |Veřejné |Bez omezení |

#### <a name="confidential-restricted"></a>Důvěrné (zakázáno)
Informace, které je jsou klasifikovány jako důvěrné nebo s omezeným přístupem obsahuje data, která může být závažné tooone nebo více osob nebo organizace, pokud dojde k ohrožení nebo ke ztrátě. Tyto informace je často poskytnuté na základě "nutné tooknow" a může zahrnovat: 

* Osobní data, včetně identifikovatelné osobní údaje, například sociálního zabezpečení nebo státní identifikační čísla, passport čísla, čísla platebních karet, ovladače čísla multilicenčních, lékařské záznamy a zdravotním pojištění čísla ID zásad.  
* Finanční záznamy, včetně finančních účet například kontrola nebo čísla investice. 
* Obchodních informací, například dokumenty nebo data, která je jedinečný nebo konkrétní duševní vlastnictví.  
* Právní data, včetně potenciální materiálu moci úrovní oprávnění. 
* Ověřovací data, včetně šifrování privátního klíče, páry heslo uživatelské jméno nebo jiné identifikační posloupnosti například biometrické soubory privátních klíčů. 

Data, která je klasifikován jako důvěrný často má regulačních a požadavky na dodržování předpisů pro manipulaci s daty. 

#### <a name="for-internal-use-only-sensitive"></a>Pro interní použití pouze (velká a malá písmena)
Informace, které je klasifikován jako středně velkých a malých písmen obsahuje soubory a data, která nebude mít vážný dopad na jednotlivcům nebo organizace, v případě ztráty nebo zničena. Tyto informace mohou zahrnovat: 

* E-mailu, většina z nich je možné odstranit nebo distribuované aniž by docházelo krizové (s výjimkou poštovních schránek nebo e-mailu z jednotlivcům, kteří se budou identifikovat důvěrné klasifikace hello).  
* Dokumenty a soubory, které neobsahují důvěrné údaje.

Obecně platí tato klasifikace obsahuje všechno, co není důvěrné. Tato klasifikace může zahrnovat většinu firemních dat, protože většina souborů, které budou spravovat nebo používat každodenní můžou být klasifikované jako velká a malá písmena. S výjimkou hello data, která je zveřejněna nebo je důvěrný všechna data v rámci organizace firmy můžou být klasifikované jako citlivé ve výchozím nastavení. 

#### <a name="public-unrestricted"></a>Public (bez omezení)
Informace, které jsou klasifikovány jako veřejné zahrnuje data a soubory, které nejsou důležité toobusiness potřebám nebo operace. Tato klasifikace může obsahovat také data, která záměrně byla vydaná toohello veřejné k jejich použití, jako je například marketingové materiály nebo klikněte na tlačítko oznámení. Kromě toho tato klasifikace může obsahovat data, jako jsou e-mailové zprávy nevyžádané pošty uložených v e-mailové služby. 

### <a name="define-data-ownership"></a>Definování vlastnictví dat.
Je důležité tooestablish řetěz zrušte odnětí vlastnictví pro všechny datové prostředky. Hello následující tabulka obsahuje různé datové vlastnictví rolí v úsilí klasifikace dat a jejich odpovídající oprávnění.  

| **Role** | **Vytvoření** | **Úprava nebo odstranění** | **Delegát** | **Čtení** | **Archivace a obnovení** |
| --- | --- | --- | --- | --- | --- |
| Vlastník |X |X |X |X |X |
| Správce | | |X | | |
| Správce | | | | |X |
| Uživatel\* | |X | |X | |

**Uživatelé poskytnout další oprávnění, jako upravit a odstranit tak, že správce* 

> [!NOTE]
> Tato tabulka neobsahuje vyčerpávající seznam rolí a oprávnění, ale pouze reprezentativní vzorek. 
> 
> 

Hello **vlastník majetku data** je původní tvůrce hello hello dat, který můžete delegovat vlastnictví a přiřadit správce. Při vytvoření souboru vlastníka hello by měl být schopný tooassign klasifikaci, což znamená, že mají odpovědnost toounderstand toho, co je toobe jsou klasifikovány jako důvěrné v závislosti na zásadách organizace. Všechna data vlastník majetku dat může být klasifikované automaticky jako pro interní použití pouze (velká a malá písmena) dokud je zodpovědná za vlastnící nebo vytváření důvěrné (s omezením) datové typy. Často bude po klasifikaci dat hello změnit roli vlastníka hello. Vlastník hello může například vytvořit databázi utajených informací a opustit jejich práva toohello data správce.  

> [!NOTE]
> data asset vlastníky často používají směs služby, zařízení a média, z nichž některé jsou osobní a některé z nich patří toohello organizace. Zrušte organizační zásady může pomoci zajistit, že využití zařízení, jako jsou přenosné počítače a inteligentní zařízení je v souladu s pokyny pro klasifikaci dat.  
> 
> 

Hello **data asset správce** je přiřazen vlastník majetku hello (nebo jejich delegáta) toomanage hello asset podle tooagreements s vlastník majetku hello nebo v souladu s požadavky příslušných zásad. V ideálním případě hello role správce, můžou se implementovat v automatizovaný systém. Správce asset zajistí potřebná přístupová oprávnění ovládací prvky jsou k dispozici a je zodpovědný za správu a ochranu prostředků delegovaný tootheir pozornost. Hello odpovědnosti správce asset hello můžou zahrnovat:  

* Ochrana hello majetek v souladu s směr vlastník majetku hello nebo souladu s podmínkami hello vlastník majetku 
* Zajištění dodržování zásad klasifikace 
* Informuje o asset vlastníci žádné změny tooagreed-při ovládací prvky nebo změny předchozí toothose postupy ochrany neprojeví 
* Generování sestav vlastník majetku toohello o odebrání tooor změny správce asset hello odpovědnosti 
* **Správce** představuje uživatele, který je zodpovědný za zajištění, že se zachová integrita, ale nejsou vlastník majetku dat, správce nebo uživatel. Ve skutečnosti mnoho role správce poskytují služby pro kontejner dat bez nutnosti přístupu toohello data. role správce Hello zahrnuje zálohování a obnovení dat hello zachování záznamů hello prostředků, a výběr, získávání a provoz hello zařízení a úložiště této úklidové hello prostředky. 
* Hello asset uživatele obsahuje každý, kdo je udělen přístup toodata nebo souboru. Přiřazení přístupu je často delegovaní hello vlastníka toohello asset správce.  

### <a name="implementation"></a>Implementace
Aspekty správy použít metody tooall klasifikace požadavků. Tyto aspekty potřebovat tooinclude podrobnosti o tom, kdo, co, kde, kdy a proč datový prostředek by se používá, získat přístup, změnit nebo odstranit. Všechny správu prostředků je třeba provést se seznámíte s jak organizace zobrazení jeho rizika, ale jednoduché metodika lze použít, jak jsou definovány v procesu klasifikace dat hello. Mezi další aspekty ke klasifikaci dat patří hello Úvod nové aplikace a nástrojů a správa změn po implementace metodu klasifikace.  

### <a name="reclassification"></a>Přeřazení
Toobe provést, pokud uživatel nebo systém, určuje tento datový prostředek hello důležitosti nebo došlo ke změně profil rizika musí přeřazení nebo změna stavu klasifikace hello datový prostředek. Tato úsilí je důležité zajistit, že stav klasifikace hello pokračuje toobe aktuální a platné. Většinu obsahu, který není klasifikovaný ručně může být klasifikované automaticky nebo na základě využití dat správce nebo vlastníka dat. 

### <a name="manual-data-reclassification"></a>Ruční data přeřazení
V ideálním případě by tato úsilí zajistí zachycení a auditovat hello podrobnosti o změnu. Hello nejpravděpodobnější důvod pro ruční přeřazení by být z důvodů velkých a malých písmen, nebo pro záznamy zachovány v dokumentu formátu, nebo data tooreview požadavek, který byl původně chybně klasifikované. Vzhledem k tomu, že tento dokument zvažuje klasifikace dat a toohello přesunutí dat v cloudu, ruční přeřazení úsilí by vyžadují pozornost případ od případu a kontrolu riziko správy by ideální tooaddress požadavkům na klasifikaci. Obecně platí takové snaze by zvažte zásad organizace hello o toho, co je toobe klasifikované, hello výchozí klasifikace stav (všechna data a soubory se citlivé, ale není důvěrné) a proveďte výjimky pro data s vysokým rizikem. 

### <a name="automatic-data-reclassification"></a>Přeřazení dat
Přeřazení dat používá stejné obecné pravidlo hello jako ruční klasifikace. Hello výjimkou je, automatizovaná řešení můžete zajistit, že jsou pravidla a potom a použít podle potřeby. Klasifikace dat lze provést v rámci vynucení zásady klasifikace dat, které můžete vynutit, pokud jsou data uložena, používá a v přenosu pomocí technologie autorizace.

* Založené na aplikaci. Pomocí některých aplikací ve výchozím nastavení se nastaví úroveň klasifikace. Data z software (CRM) pro řízení vztahů se zákazníky, HR a nástroje správy záznamů stavu je například důvěrné ve výchozím nastavení. 
* Na základě polohy. Umístění dat může pomoci identifikovat data velkých a malých písmen. Například data uložená HR nebo finanční oddělení je pravděpodobnější toobe důvěrné povahy.  

### <a name="data-retention-recovery-and-disposal"></a>Uchovávání dat, obnovení a uvolnění
Obnovení dat a uvolnění jako přeřazení dat je zásadní aspekt správy datových prostředků. Hello zásady pro obnovení dat a uvolnění by být určené zásad uchovávání dat a vynucení hello stejný způsobem jako data přeřazení; takové snaze by provést pomocí role správce a správce hello jako úlohu spolupráce.  

Selhání toohave zásad uchovávání dat by mohly znamenat dat výpadku nebo selhání toocomply s zjišťování regulačních a právní požadavky. Většina organizací, které nemají jasně definované datové zásady uchovávání informací jsou obvykle toouse výchozí "zachovat všechno" zásady uchovávání informací. Zásady uchovávání informací má však další rizika ve scénářích cloudové služby. 

Například data zásady uchovávání informací pro poskytovatele cloudových služeb lze považovat za jako "hello doba trvání předplatného hello" (Pokud je služba hello je placené pro mají být uchována hello data). Zásady uchovávání informací podnikové nebo regulačních nemusí adres a platím pro uchování smlouvy. Definování zásad pro důvěrné údaje můžete zajistit, že data jsou uložené a odebrat podle osvědčených postupů. Kromě toho zásady archivace lze vytvořit tooformalize představu o datech, která by měla být uvolněna z a kdy. 

Zásady uchovávání dat je potřeba vyřešit hello požadované regulačních a požadavky na dodržování předpisů, jakož i požadavky na podnikové povinností uchování. Klasifikovaný dat může vyvolat otázky týkající se doba uchování a výjimky pro data, která byla uložena s poskytovatelem; tyto otázky budou s větší pravděpodobností pro data, která ještě nejsou klasifikovaná správně. 

> [!TIP]
> Další informace o zásady uchovávání dat Azure a další načtením hello [Microsoft Online Subscription Agreement](https://azure.microsoft.com/support/legal/subscription-agreement/)
> 
> 

## <a name="protecting-confidential-data"></a>Ochrany důvěrných dat.
Po klasifikaci dat hledání a implementace způsoby tooprotect důvěrných dat se stane nedílnou součástí strategie nasazení všech dat ochrany. Ochrana důvěrných dat vyžaduje další pozornost toohow data ukládají a přenášených v konvenční architektury také jako hello cloudu. 

Tato část obsahuje základní informace o některé technologie, které můžete automatizovat vynucení úsilí, že toohelp ochranu dat, které byly klasifikovány jako důvěrné. 

Jako hello následující obrázek ukazuje, mohou tyto technologie nasazeny jako místní nebo cloudové řešení, nebo způsobem, hybridní prostředí, pomocí některé z je nasadit místně a některé v cloudu hello. (Některé technologie, jako je například šifrování a správy práv, také rozšířit toouser zařízení.)  

![Technologie](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Software pro správu práv
Jedno řešení pro prevenci ztráty dat je software, rights management. Na rozdíl od přístupů, které se pokoušejí toointerrupt hello toku informací na ukončovací body v organizaci software pro správu práv funguje v rámci technologie ukládání dat na úrovni hloubkové. Dokumenty jsou zašifrované a řízení přístupu, které jsou definovány v řešením pro řízení ověřování, jako je adresářová služba využívá kontrolu nad jejich dešifrování.  

> [!TIP]
> Azure Rights Management (Azure RMS) můžete použít jako hello tooprotect dat řešení ochrany informací v různých scénářích. Čtení [co je Azure Rights Management?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms) Další informace o tomto řešení Azure.
> 
> 

Mezi výhody hello software pro správu práv, patří: 

* Zabezpečeném citlivé informace. Uživatelé můžou chránit svá data přímo pomocí aplikací podporujících správu práva. Nejsou potřeba žádné další kroky – vytváření dokumentů a e-mailem a publikování dat nabízejí v konzistentní data protection rozhraní. 
* Ochrany přenáší daty hello. Zákazníci zůstat v ovládacím prvku uživatelů, kteří mají přístup k datům tootheir, zda v cloudu hello existující infrastrukturu IT, nebo na ploše uživatele hello. Organizace můžete zvolit tooencrypt svá data a omezení přístupu podle tootheir podnikové požadavky. 
* Výchozí zásady ochrany informací. Správci a uživatelé můžou používat standardní zásady pro mnoho běžné obchodní scénáře, jako je například "Jenom pro důvěrné – čtení společnosti" a "Nepředávat." Bohatá sada práv na používání jsou podporovány jako pro čtení, kopírování, tisku, uložit, úpravy a předat dál tooallow flexibilitu při definování vlastní užívací práva. 

> [!TIP]
> můžete chránit data ve službě Azure Storage pomocí [šifrování služby úložiště Azure](../storage/storage-service-encryption.md) pro neaktivní uložená Data. Můžete také použít [Azure Disk Encryption](azure-security-disk-encryption.md) toohelp chránit data uložená na virtuální disky použité pro virtuální počítače Azure.
> 
> 

### <a name="encryption-gateways"></a>Brány šifrování
Brány šifrování fungují ve své vlastní vrstvy tooprovide šifrování služby pomocí přesměrování spojnic všechna data na základě toocloud přístup. Tento přístup Nezaměňovat s, virtuální privátní sítě (VPN). Brány šifrování jsou navrženou tooprovide transparentní vrstvy na základě toocloud řešení.   

Brány šifrování poskytnout znamená toomanage a zabezpečených dat, které byly klasifikovány jako důvěrné šifrování přenášených dat hello i dat v klidovém stavu.  

Brány šifrování se umístí do hello tok dat mezi zařízeními uživatelů a aplikací datová centra služby tooprovide šifrování a dešifrování. Tato řešení, jako jsou sítě VPN, jsou převážně místními řešeními. Jsou to navrženou tooprovide třetích stran s kontrolu nad šifrovací klíče, která pomáhá snížit riziko hello umístění hello dat a správu klíčů s jednoho poskytovatele. Taková řešení jsou navržené tak, podobně jako šifrování, toowork bez problémů a transparentně mezi uživateli a hello služby. 

> [!TIP]
> můžete použít Azure ExpressRoute tooextend místní sítě do cloudu Microsoft hello přes vyhrazené soukromé připojení. Čtení [technický přehled ExpressRoute](../expressroute/expressroute-introduction.md) Další informace o této funkci. Další možnosti pro mezi místní připojení mezi místní sítí a [Azure je síť site-to-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).
> 
> 

### <a name="data-loss-prevention"></a>Zabránění ztrátě dat
Ztráty dat (někdy označují tooas úniku) je důležitý faktor a hello prevence ztráty dat externí prostřednictvím škodlivý a náhodných insider je prvořadá pro mnoho společností.  

Technologie ochrany před únikem informací ztráty dat může pomoci zajistit, aby řešení například e-mailové služby není přenášet data, která je klasifikovaný jako důvěrné. Organizace, můžete využít výhod funkcí ochrany před únikem informací v existující produkty toohelp nedošlo ke ztrátě dat.. Tyto funkce používají zásady, které lze snadno vytvořit od nuly nebo pomocí šablony poskytl hello software zprostředkovatele.  

Technologie ochrany před únikem informací můžete provést hloubkovou analýzu obsahu prostřednictvím porovnání klíčových slov, slovníků, vyhodnocení regulárních výrazů a dalších způsobů zkoumání obsahu toodetect obsah, který je v rozporu organizační zásady ochrany před únikem informací. Například ochrany před únikem informací, pomáhá zabránit ztrátě hello hello následující typy dat: 

* Sociálního zabezpečení a státní identifikační čísla 
* Bankovních údajů 
* Čísla platebních karet  
* IP adresy 

Některé technologie ochrany před únikem informací také zadat hello možnost toooverride hello ochrany před únikem informací konfiguraci (například pokud organizace potřebuje tootransmit číslo sociálního pojištění informace tooa mzdy procesoru). Kromě toho je možné tooconfigure ochrany před únikem informací, aby se uživatelům oznamuje před i pokoušejí toosend citlivé informace, které by neměly být odeslány. 

> [!TIP]
> tooprotect možnosti ochrany před únikem informací Office 365 můžete použít k dokumentům. Čtení [ovládací prvky dodržování předpisů Office 365: ochranu před únikem](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/) Další informace.
> 
> 

## <a name="see-also"></a>Viz také
* [Osvědčené postupy pro šifrování dat Azure](azure-security-data-encryption-best-practices.md)
* [Osvědčené postupy zabezpečení řízení Azure správu identit a přístupu](azure-security-identity-management-best-practices.md)
* [Blog týmu zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/)
* [Středisko Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

