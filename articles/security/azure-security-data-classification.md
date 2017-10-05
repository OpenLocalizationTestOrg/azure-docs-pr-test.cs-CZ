---
title: Klasifikace dat pro Azure. | Microsoft Docs
description: "Tento článek obsahuje úvod do základní informace o klasifikaci dat a zvýrazňuje svou hodnotu, konkrétně v rámci cloud computing a pomocí Microsoft Azure"
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
ms.openlocfilehash: e5d8841c47f91b27131fcf5066bfd3805b5670f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="data-classification-for-azure"></a>Klasifikace dat pro Azure.
Tento článek obsahuje úvod do základní informace o klasifikaci dat a zvýrazňuje svou hodnotu, konkrétně v rámci cloud computing a pomocí Microsoft Azure. 

## <a name="data-classification-fundamentals"></a>Základy klasifikace dat
Klasifikace úspěšné dat v organizaci, vyžaduje široký povědomí o potřebám vaší organizace a důkladné znalosti týkající se kde jsou uloženy vaše datové prostředky.  

Data v jednom ze tří stavů základní existuje: 

* V klidovém stavu 
* V procesu 
* Při přenosu 

Všechny tři stavy vyžadovat jedinečné technických řešení pro klasifikaci dat, ale použité zásady klasifikace dat musí být stejná pro všechny. Data, která je klasifikován jako důvěrný musí zůstat důvěrné při v klidovém stavu, v procesu a v provozu. 

Data mohou být také strukturovaných nebo nestrukturovaných. Procesy typické klasifikace pro strukturovaná data v databází a tabulek jsou méně složitý a zdlouhavý ke správě než ty, které pro Nestrukturovaná data, jako jsou dokumenty, zdrojový kód a e-mailu. 

> [!TIP]
> Další informace týkající se možnosti Azure a osvědčené postupy pro šifrování dat [osvědčené postupy pro šifrování dat Azure](azure-security-data-encryption-best-practices.md)
> 
> 

Obecně platí, organizace bude mít více než strukturovaných dat Nestrukturovaná data. Bez ohledu na to, zda jsou data strukturovaných nebo nestrukturovaných je důležité pro správu dat velkých a malých písmen. Když je implementovaná správně, klasifikace dat pomáhá zajistit že citlivá nebo důvěrná data, prostředky se spravují pomocí dohledu větší než datové prostředky, které jsou považovány za veřejné nebo volně distribuovat. 

### <a name="controlling-access-to-data"></a>Řízení přístupu k datům
Ověřování a autorizace často matoucí mezi sebou a nesprávně pochopeny jejich rolí. Ve skutečnosti jsou výrazně lišit, jak je znázorněno na následujícím obrázku.  

![Přístup k datům a řízení](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Authentication
Ověřování se obvykle skládá z nejméně dvou částí: uživatelské jméno nebo uživatelské ID pro identifikaci uživatele a token, jako například heslo, potvrďte, že je platné přihlašovací údaje uživatelského jména. Proces neposkytuje ověřeného uživatele s přístupem k všech položek nebo služeb; ověří, zda uživatel je kdo říká se, že jsou.   

> [!TIP]
> [Azure Active Directory](../active-directory/active-directory-whatis.md) poskytuje služby cloudových identit, které umožňují ověřování a autorizaci uživatelů. 
> 
> 

### <a name="authorization"></a>Autorizace
Autorizace je proces poskytnutí ověřeného uživatele umožňuje přístup k aplikaci, datové sady, datový soubor nebo některé další objekt. Přiřazení ověřené uživatele práva k používání, upravit nebo odstranit položky, které získají přístup k vyžaduje pozornost klasifikace dat. 

Úspěšné autorizace vyžaduje implementace mechanismus k ověření jednotlivých uživatelů požadavky na přístup k souborům a informacím založená na kombinaci role, zásady zabezpečení a rizika zásady aspekty. Například data z konkrétní-obchodní (LOB) aplikace nemusí mít přístup všichni zaměstnanci a pouze malou podmnožinu zaměstnanci pravděpodobně potřebovat přístup k souborům lidských zdrojů (HR). Ale pro organizace na řízení, kdo má přístup dat, třeba i jako kdy a jak, musí být účinný systém pro ověřování uživatelů na místě. 

> [!TIP]
> ve službě Microsoft Azure nezapomeňte využít řízení přístupu řízení (RBAC) Chcete-li poskytnout pouze takovou úroveň přístupu, aby uživatelé potřebovat k provádění svých úloh. Čtení [použití přiřazení rolí ke správě přístupu k prostředkům Azure Active Directory](../active-directory/role-based-access-control-configure.md) Další informace. 
> 
> 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Role a zodpovědnosti v cloud computing
I když poskytovatelů cloudu vám může pomoci řídit rizika, zákazníci potřeba zajistit, že Správa klasifikace dat a vynucení je implementovaná správně zajistit odpovídající úroveň služby pro data.  

Odpovědnosti klasifikace dat budou lišit v závislosti na modelu služby cloud je na místě, jak je znázorněno na následujícím obrázku. Jsou tři modely služeb primárního cloudu infrastruktura jako služba (IaaS), platforma jako služba (PaaS) a software jako služba (SaaS). Implementace mechanismy pro klasifikaci dat bude také lišit v závislosti na spoléhat na a očekávání poskytovateli cloudu. 

![Role](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

I když jste zodpovědní za klasifikace dat, měli poskytovatele cloudových napsané závazky o tom, jak bude zabezpečení a zachovat ochranu osobních údajů zákaznická data uložená v jejich cloudu.  

* **Zprostředkovatelé IaaS** požadavky jsou omezeny na zajistíte, že virtuální prostředí zvládne funkcemi klasifikace dat a požadavky na dodržování předpisů zákazníka. Zprostředkovatelé IaaS mají menší role v klasifikaci dat, protože stačí Ujistěte se, že data zákazníků řeší požadavky na dodržování předpisů. Však zprostředkovatelé stále Ujistěte se, že jejich virtuální prostředí adres požadavkům na klasifikaci dat kromě zabezpečení svých datových center.
* **Zprostředkovatelé PaaS** odpovědnosti může být ve smíšeném, protože platformu může v rámci vrstveného přístupu k zajištění zabezpečení pro nástroj klasifikace. Zprostředkovatelé PaaS může být zodpovědná za ověřování a které by mohly mít některé autorizační pravidla a musí poskytnout zabezpečení a funkcemi klasifikace dat k jejich aplikační vrstvu. Jako IaaS poskytovatelů, mnohem PaaS zprostředkovatelé nutné zajistit, že jejich platformy odpovídá požadavkům na klasifikaci všechny relevantní data.
* **Poskytovatelů SaaS** bude často by se zařadit jako součást řetězce autorizace a bude potřeba zajistit, že data uložená v aplikaci SaaS se dá nastavit podle typu klasifikace. Aplikace SaaS můžete použít pro obchodní aplikace a jejich podstaty potřebují prostředek k ověřování a autorizaci data, která se používá a uloží. 

## <a name="classification-process"></a>Proces klasifikace
Mnoho organizací, které pochopit potřebu klasifikace dat a chcete ho implementovat čelí základní výzvě: kde začít?

Efektivní a jednoduchá implementace klasifikace dat je možné použít plán, proveďte, ZKONTROLUJTE, fungovat model z [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx). Následující obrázek grafy úloh, které jsou nutné k implementaci úspěšně klasifikace dat v tomto modelu.  

1. **PLÁNOVÁNÍ**. Identifikujte datových prostředků, správce dat pro nasazení programu klasifikace a vyvíjet ochrany profily. 
2. **PROVEĎTE**. Po typové zásady klasifikace dat, nasaďte program a implementovat technologie vynucení podle potřeby pro důvěrné údaje.  
3. **ZKONTROLUJTE**. Zkontrolujte a ověřit sestavy zajistit, že nástroje a metody, které používá jsou efektivně adresování zásady klasifikace. 
4. **AKCE**. Zkontrolujte stav přístupu k datům a zkontrolujte soubory a data, která vyžadují revizi pomocí metodika přeřazení a revize přijmout změny a nová rizika adresu.  

![Plánování,, zkontrolujte, fungují](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)

### <a name="select-a-terminology-model-that-addresses-your-needs"></a>Vyberte model terminologie, která řeší potřeb
Existuje několik typů procesů pro data, včetně manuální procesy, na základě polohy procesy, které klasifikaci dat podle umístění uživatele nebo systému, procesy založené na aplikaci, například klasifikace specifické pro databáze a automatickou klasifikaci procesy používané modulem různých technologií, z nichž některé jsou popsány v části "Ochrany důvěrných dat." později v tomto článku.  

Tento článek představuje dva modely zobecněný terminologie, které jsou založeny na dobře použité a dodrženy odvětví modely. Tyto terminologie modely, které poskytují tři úrovně klasifikace velkých a malých písmen, jsou uvedené v následující tabulce.  

> [!NOTE]
> při klasifikaci souboru či prostředku, který kombinuje data, která by obvykle rozdělit na různé úrovně, nejvyšší úroveň klasifikace přítomen zavede celkové klasifikace. Například by se měly klasifikovat soubor obsahující citlivé a s omezeným přístupem data jako s omezeným přístupem.  
> 
> 

| **Velkých a malých písmen** | **Terminologie modelu 1** | **Terminologie modelu 2** |
| --- | --- | --- |
| Vysoký |Důvěrné informace |Omezený |
| Střednědobé používání |Pouze pro interní použití |Citlivé |
| Nízký |Veřejné |Bez omezení |

#### <a name="confidential-restricted"></a>Důvěrné (zakázáno)
Informace, které je jsou klasifikovány jako důvěrné nebo s omezeným přístupem obsahuje data, která může být závažné na jeden nebo více osob nebo organizace, pokud bezpečnostním rizikům, nebo ztraceny. Takové informace jsou často poskytovány na základě "potřebujete vědět" a může zahrnovat: 

* Osobní data, včetně identifikovatelné osobní údaje, například sociálního zabezpečení nebo státní identifikační čísla, passport čísla, čísla platebních karet, ovladače čísla multilicenčních, lékařské záznamy a zdravotním pojištění čísla ID zásad.  
* Finanční záznamy, včetně finančních účet například kontrola nebo čísla investice. 
* Obchodních informací, například dokumenty nebo data, která je jedinečný nebo konkrétní duševní vlastnictví.  
* Právní data, včetně potenciální materiálu moci úrovní oprávnění. 
* Ověřovací data, včetně šifrování privátního klíče, páry heslo uživatelské jméno nebo jiné identifikační posloupnosti například biometrické soubory privátních klíčů. 

Data, která je klasifikován jako důvěrný často má regulačních a požadavky na dodržování předpisů pro manipulaci s daty. 

#### <a name="for-internal-use-only-sensitive"></a>Pro interní použití pouze (velká a malá písmena)
Informace, které je klasifikován jako středně velkých a malých písmen obsahuje soubory a data, která nebude mít vážný dopad na jednotlivcům nebo organizace, v případě ztráty nebo zničena. Tyto informace mohou zahrnovat: 

* E-mailu, většina z nich je možné odstranit nebo distribuované aniž by docházelo krizové (s výjimkou poštovních schránek nebo e-mailu z jednotlivcům, kteří se budou identifikovat důvěrné klasifikace).  
* Dokumenty a soubory, které neobsahují důvěrné údaje.

Obecně platí tato klasifikace obsahuje všechno, co není důvěrné. Tato klasifikace může zahrnovat většinu firemních dat, protože většina souborů, které budou spravovat nebo používat každodenní můžou být klasifikované jako velká a malá písmena. S výjimkou data, která je zveřejněna nebo je důvěrný všechna data v rámci organizace firmy můžou být klasifikované jako citlivé ve výchozím nastavení. 

#### <a name="public-unrestricted"></a>Public (bez omezení)
Informace, které jsou klasifikovány jako veřejné zahrnuje data a soubory, které nejsou důležité pro obchodní potřeby nebo operace. Tato klasifikace může také obsahovat data, která byla vydána úmyslně veřejnosti k jejich použití, jako je například marketingové materiály nebo stiskněte klávesu oznámení. Kromě toho tato klasifikace může obsahovat data, jako jsou e-mailové zprávy nevyžádané pošty uložených v e-mailové služby. 

### <a name="define-data-ownership"></a>Definování vlastnictví dat.
Je důležité vytvořit řetěz zrušte odnětí vlastnictví pro všechny datové prostředky. Následující tabulka obsahuje různé datové vlastnictví rolí v úsilí klasifikace dat a jejich odpovídající práva.  

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

**Vlastník majetku data** je původní tvůrce dat, který můžete delegovat vlastnictví a přiřadit správce. Když soubor je vytvořen, vlastníkem byste měli mít přiřadit klasifikaci, což znamená, že mají odpovědnost pochopit, co musí být klasifikovány jako důvěrné informace, na základě zásad organizace. Všechna data vlastník majetku dat může být klasifikované automaticky jako pro interní použití pouze (velká a malá písmena) dokud je zodpovědná za vlastnící nebo vytváření důvěrné (s omezením) datové typy. Často bude po klasifikaci dat změnit vlastníka role. Vlastník může například vytvořit databázi utajených informací a opustit jejich oprávnění do Správce dat.  

> [!NOTE]
> data asset vlastníky často používají směs služby, zařízení a média, z nichž některé jsou osobní a některé z nich patří organizaci. Zrušte organizační zásady může pomoci zajistit, že využití zařízení, jako jsou přenosné počítače a inteligentní zařízení je v souladu s pokyny pro klasifikaci dat.  
> 
> 

**Data asset správce** je přiřazen vlastník majetku (nebo jejich delegáta) ke správě asset podle smluv s vlastník majetku nebo v souladu s požadavky příslušných zásad. V ideálním případě roli správce, můžou se implementovat v automatizovaný systém. Správce asset zajistí, že ovládací prvky potřebná přístupová oprávnění jsou k dispozici a je odpovědná za správu a ochranu prostředků delegovat na jejich pozornost. Odpovědnosti správce asset můžou zahrnovat:  

* Ochrana majetek v souladu s vlastník majetku směr nebo souladu s podmínkami vlastník majetku 
* Zajištění dodržování zásad klasifikace 
* Informuje o asset vlastníci všechny změny na dohodnuté ovládací prvky nebo postupy ochrany před tyto změny se neprojeví 
* Zprávy o změny nebo odebrání odpovědnosti správce asset vlastník majetku 
* **Správce** představuje uživatele, který je zodpovědný za zajištění, že se zachová integrita, ale nejsou vlastník majetku dat, správce nebo uživatel. Ve skutečnosti mnoho role správce poskytují služby pro kontejner dat bez nutnosti přístupu k datům. Role správce zahrnuje zálohování a obnovení dat, zachování záznamů prostředků a výběr, získávání a provoz zařízení a úložiště domu prostředky. 
* Uživatel asset obsahuje každý, kdo je udělen přístup k souboru nebo data. Přiřazení přístupu je často delegovaní vlastníkovi správce asset.  

### <a name="implementation"></a>Implementace
Aspekty správy platí pro všechny metody požadavků klasifikace. Těchto aspektů musí obsahovat podrobnosti o tom, kdo, co, kde, kdy a proč datový prostředek by se používá, získat přístup, změnit nebo odstranit. Všechny správu prostředků je třeba provést se seznámíte s jak organizace zobrazení jeho rizika, ale jednoduché metodika lze použít, jak jsou definovány v procesu klasifikace dat. Mezi další aspekty ke klasifikaci dat patří zavedení nových aplikací a nástrojů a správa změn po implementace metodu klasifikace.  

### <a name="reclassification"></a>Přeřazení
Přeřazení nebo změna stavu klasifikace datový prostředek je potřeba provést, pokud uživatel nebo systém, určuje, že se změnila profil důležitost nebo riziko datový prostředek. Tato úsilí je důležité zajistit, že stav klasifikace je stále aktuální a platné. Většinu obsahu, který není klasifikovaný ručně může být klasifikované automaticky nebo na základě využití dat správce nebo vlastníka dat. 

### <a name="manual-data-reclassification"></a>Ruční data přeřazení
V ideálním případě by tato úsilí zajistí zachycení a auditovat podrobnosti o změnu. Nejpravděpodobnější důvod pro ruční přeřazení by být z důvodů velkých a malých písmen, nebo pro zachovány v dokumentu formátu, nebo požadavek na data, která byla původně chybně klasifikované zkontrolujte záznamy. Vzhledem k tomu, že tento dokument zvažuje klasifikace dat a přesunutí dat do cloudu, ruční přeřazení úsilí by vyžadují pozornost případ od případu a kontrolu riziko správy by ideální pro požadavky na klasifikaci adresu. Obecně by takové snaze zvažte zásady organizace, o co musí být klasifikované, výchozí stav klasifikace (všechna data a soubory se citlivé, ale není důvěrné) a proveďte výjimky pro data s vysokým rizikem. 

### <a name="automatic-data-reclassification"></a>Přeřazení dat
Přeřazení dat používá stejné obecným pravidlem jako ruční klasifikace. Výjimkou je, automatizovaná řešení můžete zajistit, že jsou pravidla a potom a použít podle potřeby. Klasifikace dat lze provést v rámci vynucení zásady klasifikace dat, které můžete vynutit, pokud jsou data uložena, používá a v přenosu pomocí technologie autorizace.

* Založené na aplikaci. Pomocí některých aplikací ve výchozím nastavení se nastaví úroveň klasifikace. Data z software (CRM) pro řízení vztahů se zákazníky, HR a nástroje správy záznamů stavu je například důvěrné ve výchozím nastavení. 
* Na základě polohy. Umístění dat může pomoci identifikovat data velkých a malých písmen. Například data uložená HR nebo finanční oddělení je pravděpodobnější důvěrnou ve své podstatě.  

### <a name="data-retention-recovery-and-disposal"></a>Uchovávání dat, obnovení a uvolnění
Obnovení dat a uvolnění jako přeřazení dat je zásadní aspekt správy datových prostředků. Zásady pro obnovení dat a uvolnění by určené zásady uchovávání informací data a vynucuje stejným způsobem jako data přeřazení; takové snaze by se provedla role správce a správce jako úlohu spolupráce.  

Selhání tak, aby měl zásad uchovávání dat může znamenat ztrátě dat nebo nedodržení zjišťování regulačních a právní požadavky. Většina organizací, které nemají zásady uchovávání informací jasně definované datové často používají výchozí "zachovat všechno" zásady uchovávání informací. Zásady uchovávání informací má však další rizika ve scénářích cloudové služby. 

Například data zásady uchovávání informací pro poskytovatele cloudových služeb lze považovat za jako "doba trvání předplatného" (Pokud je služba poskytuje za data se uchovávají). Zásady uchovávání informací podnikové nebo regulačních nemusí adres a platím pro uchování smlouvy. Definování zásad pro důvěrné údaje můžete zajistit, že data jsou uložené a odebrat podle osvědčených postupů. Kromě toho zásady archivace vytvořením k formalizovat představu o jaká data by měla být uvolněna z a kdy. 

Zásady uchovávání dat je potřeba vyřešit požadované regulačních a požadavky na dodržování předpisů, jakož i požadavky na podnikové povinností uchování. Klasifikovaný dat může vyvolat otázky týkající se doba uchování a výjimky pro data, která byla uložena s poskytovatelem; tyto otázky budou s větší pravděpodobností pro data, která ještě nejsou klasifikovaná správně. 

> [!TIP]
> Další informace o zásady uchovávání dat Azure a další načtením [Microsoft Online Subscription Agreement](https://azure.microsoft.com/support/legal/subscription-agreement/)
> 
> 

## <a name="protecting-confidential-data"></a>Ochrany důvěrných dat.
Po klasifikaci dat hledání a implementace způsoby ochrany důvěrných dat se stane nedílnou součástí strategie nasazení všech dat ochrany. Ochrana důvěrných dat vyžaduje další pozornost jak se data ukládají a přenášených v konvenční architektury stejně jako v cloudu. 

Tato část obsahuje základní informace o některé technologie, které můžete automatizovat vynucení ve snaze o lepší ochranu dat, který byl klasifikován jako důvěrné. 

Jak ukazuje následující obrázek, můžete nasadit tyto technologie jako místní nebo cloudové řešení, nebo způsobem, hybridní prostředí, pomocí některé z je nasadit místně a některé v cloudu. (Některé technologie, jako je například šifrování a správy práv, také rozšířit zařízení uživatelů.)  

![Technologie](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Software pro správu práv
Jedno řešení pro prevenci ztráty dat je software, rights management. Na rozdíl od přístupů, které se pokoušejí o přerušení toku informací na ukončovací body v organizaci software pro správu práv funguje v rámci technologie ukládání dat na úrovni hloubkové. Dokumenty jsou zašifrované a řízení přístupu, které jsou definovány v řešením pro řízení ověřování, jako je adresářová služba využívá kontrolu nad jejich dešifrování.  

> [!TIP]
> Azure Rights Management (Azure RMS) jako řešení ochrany informací vám pomůže chránit data v různých scénářích. Čtení [co je Azure Rights Management?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms) Další informace o tomto řešení Azure.
> 
> 

Mezi výhody software pro správu práv, patří: 

* Zabezpečeném citlivé informace. Uživatelé můžou chránit svá data přímo pomocí aplikací podporujících správu práva. Nejsou potřeba žádné další kroky – vytváření dokumentů a e-mailem a publikování dat nabízejí v konzistentní data protection rozhraní. 
* Ochrany přenáší s daty. Zákazníci zůstat v ovládacím prvku uživatelů, kteří mají přístup k jejich datům, zda v cloudu, stávající infrastrukturu IT, nebo na ploše uživatele. Organizace můžou rozhodnout pro šifrování dat a omezit přístup na základě svých podnikových požadavků. 
* Výchozí zásady ochrany informací. Správci a uživatelé můžou používat standardní zásady pro mnoho běžné obchodní scénáře, jako je například "Jenom pro důvěrné – čtení společnosti" a "Nepředávat." Širokou škálu využití, které jsou podporovány práva, jako je čtení, kopírování, tisku, uložit, úpravy a předat dál umožňuje flexibilitu při definování vlastní užívací práva. 

> [!TIP]
> můžete chránit data ve službě Azure Storage pomocí [šifrování služby úložiště Azure](../storage/storage-service-encryption.md) pro neaktivní uložená Data. Můžete také použít [Azure Disk Encryption](azure-security-disk-encryption.md) k ochraně dat na virtuální disky použité pro virtuální počítače Azure.
> 
> 

### <a name="encryption-gateways"></a>Brány šifrování
Brány šifrování fungují ve své vlastní vrstvy k poskytování služeb šifrování pomocí přesměrování spojnic všechny přístup k datům cloudu. Tento přístup Nezaměňovat s, virtuální privátní sítě (VPN). Brány šifrování jsou navrženy pro poskytnout transparentní vrstvu na cloudové řešení.   

Brány šifrování může poskytnout způsob, jak spravovat a zabezpečit data byly klasifikovány jako důvěrné šifrování přenášených dat i dat v klidovém stavu.  

Brány šifrování se umístí do tok dat mezi zařízeními uživatelů a data aplikací centrech k poskytování služeb šifrování a dešifrování. Tato řešení, jako jsou sítě VPN, jsou převážně místními řešeními. Jsou navrženy poskytnout kontrolu nad šifrovací klíče, která pomáhá snížit riziko umístění správy dat i klíč pomocí jednoho poskytovatele třetích stran. Taková řešení jsou navržené tak, podobně jako šifrování, funguje bez problémů a transparentně mezi uživateli a služby. 

> [!TIP]
> Azure ExpressRoute můžete použít k rozšíření místní sítě do cloudu Microsoftu přes vyhrazené soukromé připojení. Čtení [technický přehled ExpressRoute](../expressroute/expressroute-introduction.md) Další informace o této funkci. Další možnosti pro mezi místní připojení mezi místní sítí a [Azure je síť site-to-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).
> 
> 

### <a name="data-loss-prevention"></a>Zabránění ztrátě dat
Ztráta dat (někdy označované jako úniku dat) je důležitý faktor a prevence ztráty dat externí prostřednictvím škodlivý a náhodných insider je prvořadá pro mnoho společností.  

Technologie ochrany před únikem informací ztráty dat může pomoci zajistit, aby řešení například e-mailové služby není přenášet data, která je klasifikovaný jako důvěrné. Organizace mohou využít výhod funkcí ochrany před únikem informací v existující produkty pomáhá zabránit ztrátě dat. Tyto funkce používají zásady, které lze snadno vytvořit od nuly nebo pomocí šablony dodávaných poskytovatelem softwaru.  

Technologie ochrany před únikem informací můžete provést hloubkovou analýzu obsahu prostřednictvím porovnání klíčových slov, slovníků, vyhodnocení regulárních výrazů a dalších způsobů zkoumání obsahu, ke zjištění obsah, který je v rozporu organizační zásady ochrany před únikem informací. Například ochrany před únikem informací můžou pomoct zabránit ztrátě následující typy dat: 

* Sociálního zabezpečení a státní identifikační čísla 
* Bankovních údajů 
* Čísla platebních karet  
* IP adresy 

Některé technologie ochrany před únikem informací také poskytují možnost přepsat konfiguraci ochrany před únikem informací (například pokud organizace potřebuje zajistit přenášet informace číslo sociálního pojištění mzdy procesoru). Kromě toho je možné nakonfigurovat ochrany před únikem informací tak, aby se uživatelům oznamuje před pokoušejí i posílat citlivé informace, které by neměly být odeslány. 

> [!TIP]
> Možnosti ochrany před únikem informací Office 365 můžete použít k ochraně dokumentů. Čtení [ovládací prvky dodržování předpisů Office 365: ochranu před únikem](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/) Další informace.
> 
> 

## <a name="see-also"></a>Viz také
* [Osvědčené postupy pro šifrování dat Azure](azure-security-data-encryption-best-practices.md)
* [Osvědčené postupy zabezpečení řízení Azure správu identit a přístupu](azure-security-identity-management-best-practices.md)
* [Blog týmu zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/)
* [Středisko Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

