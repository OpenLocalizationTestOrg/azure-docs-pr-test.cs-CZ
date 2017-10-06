---
title: "aaaSecurity výstrahy podle typu v Azure Security Center | Microsoft Docs"
description: "Tento článek popisuje hello různé druhy výstrah zabezpečení v Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.openlocfilehash: ee69cb9035c35f5bc2ed51f9b9d6f29486b4caf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Principy výstrah zabezpečení ve službě Azure Security Center
Tento článek vám pomůže toounderstand hello různé typy výstrah zabezpečení a související přehledy, které jsou k dispozici v Azure Security Center. Další informace o tom, jak toomanage výstrah a incidentů, najdete v části [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> tooset až pokročilé detekce upgradu tooAzure Standard Center zabezpečení. K dispozici je bezplatná 60denní zkušební verze. tooupgrade, vyberte **cenová úroveň** v hello [zásady zabezpečení](security-center-policies.md). toolearn více, viz hello [stránce s cenami](https://azure.microsoft.com/pricing/details/security-center/).
>

## <a name="what-type-of-alerts-are-available"></a>Jaké typy výstrah jsou k dispozici?
Azure Security Center používá celou řadu [možností detekce](security-center-detection-capabilities.md) tooalert zákazníci toopotential útoky cílení na jejich prostředí. Tyto výstrahy obsahovat cenné informace o hello co aktivuje hello výstrahy hello prostředky cílit a hello zdroj útoku hello. Hello informace obsažené v výstrahy se liší podle typu hello analytics používá toodetect hello ohrožení. Incidenty mohou obsahovat také další kontextové informace, které mohou být užitečné při dalším zkoumání hrozeb.  Tento článek obsahuje informace o hello následující typy výstrah:

* Analýza chování virtuálního počítače (VMBA)
* Analýza sítě
* Analýza prostředků
* Kontextové informace

## <a name="virtual-machine-behavioral-analysis"></a>Analýza chování virtuálního počítače
Azure Security Center může používat prostředky pro vypracování analýzy chování tooidentify ohrožení zabezpečení na základě analýzy protokolů událostí virtuálního počítače. Například události vytváření procesů a události přihlášení. Kromě toho je korelace s další toocheck signály pro podporu důkaz rozšířeným kampaně.

> [!NOTE]
> Další informace o tom, jak detekce služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).
>

### <a name="crash-analysis"></a>Analýza stavu systému
Analýza havárií výpisu paměti je metoda použitá toodetect sofistikované malwaru, který je možné tooevade tradiční zabezpečení řešení. Různé formy malwaru, zkuste tooreduce hello pravděpodobnost, že se nikdy napsáním toodisk nebo šifrování softwarové komponenty napsané toodisk detekovaných aplikací antivirové produkty. Díky tomu obtížné toodetect hello malwaru pomocí tradičních antimalwarových přístupy. Tento typ malwaru, můžete však detekovat pomocí nástroje Analýza paměti, protože malwaru musí zůstat trasování v paměti toofunction pořadí.

Když dojde k chybě softwaru, stav systému zaznamená část paměti v době hello hello havárie. Hello havárií může být způsobeno malwaru, obecné aplikace nebo systému problémy. Analýzou hello paměti v hello stav systému, Security Center zjišťovat techniky použitou tooexploit ohrožení zabezpečení v softwaru, přístup k důvěrných dat a tajně zachovat v rámci ohroženého počítače. To lze provést s minimální výkonu dopad toohosts jako hello analysis provádí hello ukončení Security Center zpět.

Hello následující pole jsou běžné toohello havárií výpisu výstrahy příklady, které se objeví dál v tomto článku:

* Soubor VÝPISU: Název souboru se stavem systému hello.
* Název_procesu: Název hello chybám procesu.
* PROCESSVERSION: Verze hello chybám procesu.

### <a name="shellcode-discovered"></a>Zjištěn skrytý spustitelný kód
Shellcode je hello datové části, která se spouští po malwaru zneužití ohrožení zabezpečení softwaru. Tato výstraha znamená, že při analýze výpisu stavu systému byl nalezen spustitelný kód, který vykazuje chování typické pro škodlivý software. Někdy se může takovým způsobem chovat i software bez zlých úmyslů, pro běžný vývoj softwaru ale toto chování není typické.

Příklad oznámení Shellcode Hello poskytuje hello následující další pole:

* Adresa: hello umístění v paměti hello shellcode.

Příklad tohoto typu výstrahy:

![Výstraha na skrytý spustitelný kód](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Zjištěno napadení modulu
Windows používá dynamické knihovny (DLL) tooallow softwaru tooutilize běžné Windows funkci systému. Knihovny DLL weby nastane, když malwaru změní hello DLL zatížení pořadí tooload škodlivý datových částí do paměti, kde mohou být provedeny libovolný kód. Tato výstraha udává, že hello havárií výpisu analysis zjistil modul s podobným názvem, který je načten z dvě různé cesty. Jedna z cest hello načíst pochází z běžné umístění binární systému Windows.

Vývojáři softwaru legitimní občas změnit pořadí načtení knihovny DLL hello řádných z důvodů, například instrumentace, rozšíření hello operačního systému Windows nebo rozšíření aplikace systému Windows. toohelp rozlišit mezi toohello škodlivý a potenciálně neškodné změny pořadí načtení knihovny DLL, Azure Security Center zkontroluje, zda modul načíst vyhovuje tooa podezřelé profilu. Hello výsledku této kontroly je indikován pole "Podpis" hello hello výstrahy a se odrazí v hello závažnost výstrahy hello, popis výstrahy a výstrah opravná opatření. tooresearch, zda modul hello je legitimní nebo škodlivý, analýza hello na disku kopii hello weby modulu. Například můžete ověřit digitální podpis souboru hello nebo spustit antivirové kontroly.

Kromě polí běžné toohello popsané v části starší "Shellcode zjištěné" hello, tuto výstrahu poskytuje hello následující pole:

* PODPIS: Určuje, pokud hello weby modulu vyhovuje profil tooa podezřelého chování.
* HIJACKEDMODULE: název hello hello zjistil modul systému Windows.
* HIJACKEDMODULEPATH: cesta hello hello zjistil modul systému Windows.
* HIJACKINGMODULEPATH: cesta hello hello zneužití modulu.

Příklad tohoto typu výstrahy:

![Výstraha na napadení modulu](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Zjištěno maskování za modul systému Windows
Malware může používat běžné názvy binární soubory systému Windows (například SVCHOST. Soubor EXE), nebo moduly (například NTDLL. Knihovny DLL) příliš*blendu* a skrývat hello povaha hello škodlivého softwaru z správce systému. Tato výstraha udává, že hello havárií výpisu analysis zjistí, že tento hello soubor se stavem systému obsahuje moduly, které používat názvy modulů systému Windows, ale nesplňují jiných kritérií, které jsou typické pro moduly prostředí Windows. Analýza hello na disku kopii podvržený modulu hello poskytnout další informace o povaze legitimní nebo škodlivý hello tohoto modulu. Součástí analýzy může být:

* Potvrďte, že tento soubor hello dotyčném je dodávána jako součást balíčku legitimní softwaru.
* Ověřte digitální podpis souboru hello.
* Spusťte antivirovým na soubor hello.

Kromě toohello běžné pole dříve popsané v části "Shellcode zjištěné" hello, tuto výstrahu poskytuje hello následující další pole:

* Podrobnosti: Popisuje, zda je metadata modulu hello platný a zda hello modul byl načten z na cestu v systému.
* Název: název hello podvržený modulu Windows hello.
* CESTA: hello cesta toohello podvržený modulu Windows.

Tato výstraha také rozbalí a zobrazí určitá pole ze záhlaví PE hello modulu, jako je například "Kontrolního SOUČTU" a "Časové razítko." Tato pole se zobrazí, pouze pokud jsou součástí modulu hello hello pole. V tématu hello [Microsoft PE a specifikace COFF](https://msdn.microsoft.com/windows/hardware/gg463119.aspx) podrobnosti o těchto polí.

Příklad tohoto typu výstrahy:

![Výstraha na maskování za modul systému Windows](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Zjištěna úprava binárního systémového souboru
Malware změnit binární soubory jádra systému v pořadí toocovertly přístup k datům nebo tajně zachovat ohrožení zabezpečení systému. Tato výstraha udává, že hello havárií výpisu analysis zjistil, že binární soubory jádra operačního systému Windows byly změněny v paměti nebo na disku.

Vývojáři legitimního softwaru někdy upravují systémové soubory v paměti bez zlých úmyslů, například soubory balíčku Detours pro zajištění kompatibility aplikací. toohelp rozlišit mezi škodlivý a potenciálně legitimní moduly, Azure Security Center zkontroluje, zda modul upravené hello vyhovuje tooa podezřelé profilu. Hello výsledku této kontroly je indikován hello závažnost výstrahy hello, popis výstrahy a výstrah opravná opatření.

Kromě toohello běžné pole dříve popsané v části "Shellcode zjištěné" hello, tuto výstrahu poskytuje hello následující další pole:

* Název modulu: Název hello upravit binárním souboru systému.
* Verze modulu: Verzi hello upravit binárním souboru systému.

Příklad tohoto typu výstrahy:

![Výstraha na upravený binární systémový soubor](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Spuštění podezřelého procesu
Security Center identifikuje podezřelé proces, který běží na hello cílový virtuální počítač a potom aktivuje výstrahu. detekce Hello nevypadá pro konkrétní název hello, ale vyhledejte parametr hello spustitelný soubor. Proto i v případě, že útočník hello přejmenuje hello spustitelný soubor, Security Center dokáže rozpoznat podezřelé proces hello.

Příklad tohoto typu výstrahy:

![Výstraha na spuštění podezřelého procesu](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domain-accounts-queried"></a>Dotazování více doménových účtů
Security Center může rozpoznat více pokusů o tooquery účtů domény služby Active Directory, která se obvykle provádí útočníci během rekognoskace sítě. Útočníci můžete využít toto technika tooquery hello domény tooidentify hello uživatelé, identifikovat hello účtů správce domény, identifikovat hello počítače, které jsou řadiče domény a také identifikovat hello potenciální domény důvěryhodný vztah s ostatními doménami.

Příklad tohoto typu výstrahy:

![Výstraha na dotazování více doménových účtů](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

### <a name="local-administrators-group-members-were-enumerated"></a>Výpis členů místní skupiny Administrators

Security Center bude tootrigger výstrahu, když je aktivovat událostí zabezpečení hello 4798, v systému Windows Server 2016 a Windows 10. To se stane, když dojde k výpisu členů skupin místních správců, což obvykle provádí útočníci během rekognoskace sítě. Útočníci mohou využívat tato technika tooquery hello identity uživatelů s oprávněním správce.

Příklad tohoto typu výstrahy:

![Místní správce](./media/security-center-alerts-type/security-center-alerts-type-fig14-new.png)

### <a name="anomalous-mix-of-upper-and-lower-case-characters"></a>Neobvyklá kombinace velkých a malých písmen

Security Center spustí výstrahu, když zjistí hello použití kombinaci velkých a malých písmen v příkazovém řádku hello. Některé útočníci mohou používat tato technika toohide z malých a velkých písmen nebo hodnoty hash na základě pravidlo počítače.

Příklad tohoto typu výstrahy:

![Neobvyklá kombinace](./media/security-center-alerts-type/security-center-alerts-type-fig15-new.png)

### <a name="suspected-kerberos-golden-ticket-attack"></a>Podezření na útok Kerberos Golden Ticket

Ohroženými [krbtgt](https://technet.microsoft.com/library/dn745899.aspx) klíč slouží útočník toocreate protokolu Kerberos "Zlatá lístky", umožňuje hello útočník tooimpersonate každý uživatel si přejí. Security Center bude tootrigger výstrahu, když zjistí tento typ aktivity.

> [!NOTE] 
> Další informace o zlatém lístku protokolu Kerberos najdete v [průvodci zmírněním následků krádeže přihlašovacích údajů Windows 10](http://download.microsoft.com/download/C/1/4/C14579CA-E564-4743-8B51-61C0882662AC/Windows%2010%20credential%20theft%20mitigation%20guide.docx).

Příklad tohoto typu výstrahy:

![Zlatý lístek](./media/security-center-alerts-type/security-center-alerts-type-fig16-new.png)

### <a name="suspicious-account-created"></a>Vytvoření podezřelého účtu

Security Center aktivuje výstrahu při vytvoření účtu, který se nápadně podobá existujícímu integrovanému účtu s oprávněním správce. Tento postup slouží toocreate útočníky podvodný účet tooavoid se zaznamenali podle lidského ověření.
 
Příklad tohoto typu výstrahy:

![Podezřelý účet](./media/security-center-alerts-type/security-center-alerts-type-fig17-new.png)

### <a name="suspicious-firewall-rule-created"></a>Vytvoření podezřelého pravidla brány firewall

Útočníci mohou zkuste toocircumvent hostitele zabezpečení tak, že vytvoříte vlastní bránu firewall pravidla tooallow škodlivé aplikace toocommunicate s příkazy a ovládání nebo toolaunch útoků prostřednictvím sítě hello prostřednictvím hello ohrožení zabezpečení hostitele. Security Center aktivuje výstrahu, když zjistí, že bylo vytvořeno nové pravidlo brány firewall ze spustitelného souboru v podezřelém umístění.
 
Příklad tohoto typu výstrahy:

![Pravidlo brány firewall](./media/security-center-alerts-type/security-center-alerts-type-fig18-new.png)

### <a name="suspicious-combination-of-hta-and-powershell"></a>Podezřelá kombinace HTA a PowerShellu

Security Center aktivuje výstrahu, když zjistí, že Microsoft HTML Application Host (HTA) spouští příkazy PowerShellu. Jde o techniku používaných útočníky toolaunch škodlivých skriptů prostředí PowerShell.
 
Příklad tohoto typu výstrahy:

![HTA a PS](./media/security-center-alerts-type/security-center-alerts-type-fig19-new.png)


## <a name="network-analysis"></a>Analýza sítě
Detekce síťových hrozeb ve službě Security Center funguje tak, že se automaticky shromažďují informace o zabezpečení z přenosu Azure IPFIX (Internet Protocol Flow Information Export). Analyzuje tyto informace, často korelace informace z více zdrojů, tooidentify hrozeb.

### <a name="suspicious-outgoing-traffic-detected"></a>Zjištěn podezřelý odchozí provoz
Síťová zařízení lze zjistit a profilovaným v mnohem hello stejným způsobem jako jiné typy systémů. Útočníci obvykle začínají skenováním portů (port scanning) nebo hledáním konkrétního otevřeného portu (port sweeping). V dalším příkladu hello máte podezřelé Secure Shell (SSH) provoz z virtuálního počítače. V tomto scénáři je možné, že došlo k útoku hrubou silou na SSH nebo útoku hledáním konkrétního otevřeného portu proti externímu prostředku.

![Výstraha na podezřelý odchozí provoz](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Tato výstraha poskytuje informace, které můžete použít tooidentify hello prostředek, který byl použité tooinitiate tento útok. Tato výstraha také obsahuje informace, že počítač, čas detekce hello, plus hello protokol a port, který byl použit k ohrožení tooidentify hello. Toto okno vám také seznam oprav kroky, které můžou být použité toomitigate tento problém.

### <a name="network-communication-with-a-malicious-machine"></a>Síťová komunikace se škodlivým počítačem
Azure Security Center může pomocí informačních kanálů analýzy hrozeb Microsoft rozpoznat ohrožené počítače, které komunikují se škodlivými IP adresami. V mnoha případech hello škodlivý adresa je příkazy a ovládání center. V takovém případě Security Center zjistila, že komunikace hello bylo provedeno pomocí malwaru budou hrát zavaděč (také označované jako [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![výstraha na síťovou komunikaci](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Tato výstraha poskytuje informace, které vám umožní tooidentify hello prostředek, který byl použité tooinitiate tento útok, hello napadení prostředek, hello postižené IP, hello útočník IP a čas detekce hello.

> [!NOTE]
> V zájmu ochrany osobních údajů jsme ze snímku obrazovky odebrali skutečné IP adresy.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Zjištění možného odchozího útoku DoS
Nestandardní síťový provoz, který pochází z jednoho virtuálního počítače může způsobit Security Center tootrigger potenciální odmítnutí služby typ útoku.

Příklad tohoto typu výstrahy:

![Odchozí útok DoS](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Analýza prostředků
Analýza prostředků Security Center se zaměřuje na platforma jako služba (PaaS) služby, jako je hello integrace s hello [detekce hrozeb Azure SQL Database](../sql-database/sql-database-threat-detection.md) funkce. Na základě analýzy hello výsledků z těchto oblastí Security Center aktivuje upozornění na týkající se prostředků.

### <a name="potential-sql-injection"></a>Potenciální útok prostřednictvím injektáže SQL
Injektáž SQL je útok, kde je škodlivý kód vložena do řetězce, které se předávají později tooan instance systému SQL Server pro analýzy a spouštění. SQL Server spouští všechny syntakticky platné dotazy, které obdrží. Všechny procedury, které sestavují SQL příkazy, by tedy měly být chráněny proti tomuto typu útoku. Detekce hrozeb SQL využívá machine learning, analýzy chování a anomálií detekce toodetermine podezřelé události, které pravděpodobně probíhá v databázích Azure SQL. Například:

* Pokus o přístup k databázi bývalým zaměstnancem
* Útoky prostřednictvím injektáže SQL
* Neobvyklé přístup tooa produkční databázi z domácí uživatele

![Výstraha na potenciální útok prostřednictvím injektáže SQL](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

Hello informace v této výstrahy může být použité tooidentify hello napadení prostředků, čas detekce hello a stavu hello hello útoku. Poskytuje také odkaz toofurther šetření kroky.

### <a name="vulnerability-toosql-injection"></a>Ohrožení zabezpečení tooSQL vkládání
Tato výstraha se aktivuje v případě zjištění chyby aplikace v databázi. Tato výstraha může znamenat, že před útoky vkládání tooSQL možných ohrožení zabezpečení.

![Výstraha na potenciální útok prostřednictvím injektáže SQL](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Neobvyklý přístup z neznámého umístění
Toto upozornění se spustí, když na server hello, který nebyla v hello poslední období byla zjištěna událost přístup z neznámého IP adresy.

![Výstraha na neobvyklý přístup](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="contextual-information"></a>Kontextové informace
Během šetření, analytici vyžadovat další kontext tooreach mínění programu o hello povaha hello hrozeb a jak toomitigate ho.  Například anomálií sítě byla zjištěna, ale bez znalosti co se děje v síti hello nebo s ohledem toohello cílené prostředkem ho je každý pevný toounderstand jaké akce tootake Další. tooaid s třídou incidentu zabezpečení mohou zahrnovat artefakty, související události a informace, které mohou pomoci dílčího zkoušení hello. Hello dostupnost Další informace budou lišit v závislosti na typu byla zjištěna hrozba hello hello konfiguraci vašeho prostředí a nebudete mít k dispozici pro všechny incidenty zabezpečení.

Pokud Další informace jsou k dispozici, zobrazí se v hello incidentu zabezpečení níže hello seznam výstrah. Může se jednat o informace tohoto typu:

- Události vymazání protokolu
- Zařízení PNP zapojená z neznámého zařízení
- Výstrahy, na které není možné reagovat 

![Výstraha na neobvyklý přístup](./media/security-center-alerts-type/security-center-alerts-type-fig20.png) 


## <a name="see-also"></a>Viz také
V tomto článku jste se dozvěděli o různých typech výstrah zabezpečení v Centru zabezpečení hello. toolearn Další informace o Security Center, najdete v části hello následující:

* [Řešení bezpečnostních incidentů v Azure Security Center](security-center-incident.md)
* [Možnosti detekce v Azure Security Center](security-center-detection-capabilities.md)
* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md): Přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
