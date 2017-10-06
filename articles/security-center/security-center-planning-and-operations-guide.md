---
title: "Průvodce aaaSecurity Center plánováním a provozem | Microsoft Docs"
description: "Tento dokument vám pomůže tooplan před přijetím řešení Azure Security Center a vyřešením aspektů každodenního provozu."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.openlocfilehash: b0a0a6f5fd56fbd46f7736928c99e3bcd0b1e140
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-planning-and-operations-guide"></a>Průvodce plánováním a provozem služby Azure Security Center
Tento průvodce je pro odborníky informačních technologií (IT) informace, IT architektům, analytikům zabezpečení informací a můžou správci cloudové, jejichž společnosti hodlají toouse Azure Security Center.

>[!NOTE] 
>Počínaje časná 2017 června, Security Center použije hello agenta Microsoft Monitoring Agent toocollect a ukládat data. V tématu [Azure Security Center platformy migrace](security-center-platform-migration.md) toolearn Další. Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>

## <a name="planning-guide"></a>Průvodce plánováním
Tato příručka popisuje sadu kroků a úloh, které vám pomůžou toooptimize použití služby Security Center v závislosti na požadavcích zabezpečení vaší organizace a modelu správy cloudu. tootake plně využívat Security Center, je důležité toounderstand jak budou různí jednotlivci nebo týmy ve vaší organizaci použijte hello služby toomeet zabezpečený vývoj a provoz, sledování, řízení a reakce na incidenty. jsou klíčové oblasti tooconsider Hello při plánování toouse Security Center:

* Role zabezpečení a řízení přístupu
* Zásady a doporučení zabezpečení
* Shromažďování dat a úložiště
* Průběžné sledování zabezpečení
* Reakce na incidenty

V další části hello se dozvíte, jak tooplan pro každou z těchto oblastí a tato doporučení uplatnit podle svých požadavků.

> [!NOTE]
> Čtení [Azure Security Center nejčastější dotazy (FAQ)](security-center-faq.md) seznam běžných dotazů, které může být užitečné také při hello fáze plánování a návrhu.
> 

## <a name="security-roles-and-access-controls"></a>Role zabezpečení a řízení přístupu
V závislosti na hello velikosti a struktuře vaší organizace může použít více jednotlivci a týmy a Security Center tooperform různé související se zabezpečením úlohy. V následujícím diagramu hello máte příklad zahrnující fiktivní osoby a jejich příslušné role a povinnosti v oblasti zabezpečení:

![Role](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Security Center umožňuje tyto jednotlivce toomeet tyto různé odpovědnosti. Například:

**Jeff (vlastník úloh v cloudu)**

* Spravuje úlohy v cloudu a související prostředky
* Odpovídá za implementace a správu bezpečnostních mechanismů v souladu se zásadami zabezpečení společnosti

**Ellen (ředitelka zabezpečení informací / ředitelka IT)**

* Zodpovědná za všechny aspekty zabezpečení společnosti hello
* Chce postavení zabezpečení společnosti hello toounderstand napříč Cloudová pracovní prostředí
* Potřebuje toobe informováni o hlavní útoky a rizika

**David (zabezpečení IT)**

* Nastaví firemní zásady zabezpečení, které se zavede ochrana odpovídající tooensure hello
* Dohlíží na dodržování zásad
* Generuje sestavy pro vedení a auditory

**Judy (pracovnice oddělení zabezpečení)**

* Monitoruje a odpoví toosecurity výstrahy 24 hodin denně 7
* Eskaluje tooCloud vlastník úloh nebo analytik zabezpečení IT

**Sam (analytik zabezpečení)**

* Zkoumá útoky
* Práce s nápravy tooapply vlastník úloh v cloudu 

Security Center používá [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md), který poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md) , lze přiřadit toousers, skupiny a služby v Azure. Když uživatel otevře Security Center, uvidí jenom informace související s tooresources, ke kterým mají přístup. Což znamená, že uživatel hello je přiřazenou roli hello vlastník, Přispěvatel nebo Čtenář toohello předplatné nebo prostředek skupiny, která daný prostředek patří. Přidání toothese role existují dvě konkrétní role Security Center:

- **Zabezpečení čtečky**: uživatel, který patří toothis role je být schopný tooview práva tooSecurity Center, která obsahuje doporučení, výstrahy, zásady a stav, ale nebude moct toomake změny.
- **Správce zabezpečení**: stejná jako čtečky zabezpečení, ale můžete také aktualizovat hello zásady zabezpečení, Zavřít doporučení a výstrahy.

role Security Center Hello popsané výše nemají přístup tooother služby oblasti Azure, jako je například úložiště, webové a mobilní telefon nebo Internet věcí.  

> [!NOTE]
> Uživatel potřebuje toobe alespoň předplatné, vlastník skupiny prostředků nebo Přispěvatel toobe možné toosee v Azure Security Center. 
> 
> 

Hello osoby pomocí popsaných v předchozím diagramu hello hello, které by potřeba tyto role RBAC:

**Jeff (vlastník úloh v cloudu)**

* Vlastník/spolupracovník skupiny prostředků

**David (zabezpečení IT)**

* Vlastník/spolupracovník předplatného nebo správce zabezpečení

**Judy (pracovnice oddělení zabezpečení)**

* Čtenář předplatného s oprávněním nebo tooview výstrahy čtenářům zabezpečení
* Vlastník/spolupracovník předplatného nebo správce zabezpečení vyžaduje toodismiss výstrahy

**Sam (analytik zabezpečení)**

* Výstrahy tooview čtečky předplatného
* Vlastník/spolupracovník předplatného požadované toodismiss výstrahy
* Může být nutný přístup toohello prostoru

Některé jiné tooconsider důležité informace:

* Zásady zabezpečení můžou upravovat jenom vlastníci/přispěvatelé předplatného a správci zabezpečení.
* Doporučení zabezpečení pro určitý prostředek můžou uplatňovat jenom vlastníci a přispěvatelé předplatného a skupiny prostředků.

Při plánování řízení přístupu pomocí RBAC pro Security Center, být jisti toounderstand, které ve vaší organizaci budou používat Security Center. Ke správné konfiguraci RBAC je také důležité, jaké typy úloh budou uživatelé provádět.

> [!NOTE]
> Doporučujeme, abyste přiřadili hello omezenou roli potřebné pro uživatele toocomplete úkoly. Například uživatelé, kteří stačí tooview informace o hello stav zabezpečení prostředků, ale nemusí provádět žádné akce, třeba uplatňovat doporučení nebo upravovat zásady, musí být přiřazena role čtečky hello.
> 
> 

## <a name="security-policies-and-recommendations"></a>Zásady a doporučení zabezpečení
Zásady zabezpečení definuje hello sadu ovládacích prvků, které se doporučují pro prostředky v rámci hello zadaný odběr. V Security Center určíte zásady podle tooyour požadavky na zabezpečení a společnosti hello typu aplikací nebo citlivosti dat hello.

Zásady, které jsou povoleny v úrovni předplatného hello automaticky rozšíří tooall skupin prostředků v rámci předplatného hello, jak je znázorněno v následujícím diagramu hello:

![Zásady zabezpečení](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-newUI.png)

> [!NOTE]
> Pokud potřebujete tooreview, které zásady jste změnily, můžete použít [protokoly auditu Azure](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). V protokolech auditu jsou zaznamenané všechny změny zásad.
> 
> 

### <a name="security-recommendations"></a>Doporučení zabezpečení
Před konfigurací zásad zabezpečení, prohlédněte si všechny hello [doporučení zabezpečení](security-center-recommendations.md)a určit, jestli tyto zásady jsou vhodné pro vaše předplatné a skupiny prostředků. Je také důležité toounderstand jakou akci by měla být provedena tooaddress [doporučení zabezpečení](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations) a kdo ve vaší organizaci je zodpovědná za monitorování pro nová doporučení a hello pořízení potřebné kroky.

Security Center vám doporučí, abyste ve svém předplatném Azure uvedli podrobnosti o kontaktu zabezpečení. Tyto informace použije Microsoft toocontact vám, zda text hello Microsoft Security Response Center (MSRC) zjistí, že zákaznických údajů měl strana nezákonné nebo neoprávněný přístup k. Čtení [zadejte kontaktní údaje zabezpečení v Azure Security Center](security-center-provide-security-contact-details.md) pro další informace o tooenable tohoto doporučení.

## <a name="data-collection-and-storage"></a>Shromažďování dat a úložiště
Azure Security Center používá hello agenta Microsoft Monitoring Agent – to je hello používá stejné agenta hello Operations Management Suite a analýzy protokolů služba – toocollect data zabezpečení z virtuálních počítačů. Data shromážděná z tohoto agenta se budou ukládat v pracovních prostorech Log Analytics.

### <a name="agent"></a>Agent

Po povolení shromažďování dat v zásadách zabezpečení hello hello agenta Microsoft Monitoring Agent (pro [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) nebo [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)) je nainstalována na všech podporovaných virtuálních počítačích Azure a všechny nové, které jsou vytvořeny.  Pokud hello hello Microsoft Monitoring Agent nainstalován, již má virtuální počítač Azure Security Center budou využívat hello aktuální nainstalovat agenta. proces Hello agenta je neinvazivní navrženou toobe a mít velmi minimální dopad na výkon virtuálního počítače.

Microsoft Monitoring agenta pro Windows Hello vyžaduje používá port TCP 443. V tématu hello [článku Poradce při potížích s](security-center-troubleshooting-guide.md) další podrobnosti.

Pokud v určitém okamžiku chcete toodisable shromažďování dat, můžete ho vypnout v zásadách zabezpečení hello. Ale protože hello agenta Microsoft Monitoring Agent může být používán jiných Azure správu a monitorování služeb, hello agent nebude odinstalován automaticky po vypnutí shromažďování dat ve službě Security Center. V případě potřeby můžete ručně odinstalovat agenta hello.

> [!NOTE]
> seznam podporovaných virtuálních počítačů, přečtěte si hello toofind [Azure Security Center nejčastější dotazy (FAQ)](security-center-faq.md).
> 

### <a name="workspace"></a>Pracovní prostor

Data shromážděná z hello agenta Microsoft Monitoring Agent (jménem Azure Security Center) se budou ukládat do buď existující pracovních prostorů analýzy protokolů spojených s vašeho předplatného Azure nebo nové pracovních prostorů, s ohledem na účet hello geograficky hello virtuálních počítačů. 

V hello portálu Azure můžete procházet toosee seznam vašich analýzy protokolů pracovních prostorů, včetně všech vytvořených pomocí služby Azure Security Center. Pro nové pracovní prostory se vytvoří související skupina prostředků. Budou se řídit těmito zásadami vytváření názvů: 

* Pracovní prostor: *DefaultWorkspace-[ID_předplatného]-[zeměpisné umístění]*
* Skupina prostředků: *DefaultResouceGroup-[zeměpisné_umístění]*

U pracovních prostorů vytvořených službou Azure Security Center se data uchovávají po dobu 30 dnů. Pro ukončení pracovních prostorů, uchování vychází z pracovního prostoru hello cenová úroveň.

> [!NOTE]
> Microsoft zkontrolujte silné závazky tooprotect hello ochrany osobních údajů a zabezpečení tato data. Microsoft dodržuje pravidla dodržování předpisů a zabezpečení toostrict – z kódování toooperating služby. Další informace o zpracování dat a ochraně osobních údajů najdete v článku [Zabezpečení dat ve službě Azure Security Center](security-center-data-security.md).
> 

## <a name="ongoing-security-monitoring"></a>Průběžné sledování zabezpečení
Po počáteční konfiguraci a uplatnění doporučení služby Security Center hello dalším krokem zvážení provozních procesů Security Center.

tooaccess Security Center z portálu Azure, můžete kliknout na hello **Procházet** a typ **Security Center** v hello **filtru** pole. Hello zobrazení, že uživatel získá hello jsou podle toothese použít filtry, hello níže uvedený příklad prostředí s mnoha toobe potíže řešit:

![řídicí panel](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig6.png)

> [!NOTE]
> Security Center nebude ovlivňovat vaše běžné provozní postupy, bude jenom pasivně sledovat vaše nasazení a poskytovat doporučení na základě zásad hello zabezpečení, které jste povolili.

Když jste nejdřív vyjádřit výslovný souhlas s toouse Security Center svého stávajícího prostředí Azure, ujistěte se, že si projít všechna doporučení, které lze provést v hello **doporučení** dlaždici nebo u jednotlivých prostředků (**výpočetní** **Sítě**, **& úložiště dat**, **aplikace**).

Až vyřešíte všechna doporučení, hello **prevence** části by měly být pro všechny prostředky, které byly odstraněny. Průběžné monitorování v tomto okamžiku jednodušší, protože je budete provádět kroky jenom na základě změn v hello prostředků zabezpečení stavu a dlaždic doporučení.

Hello **detekce** části je víc reakcí, obsahuje totiž výstrahy týkající se problémů, které jsou buď dochází nyní, nebo došlo k chybě v posledních hello a kdy je zjistily ovládací prvky Security Center a systémy 3. stran. dlaždice výstrahy zabezpečení Hello se zobrazí pruhové grafy znázorňující hello počet výstrah o zjištěných hrozbách, které nebyly nalezeny v každý den a jejich distribuci mezi hello různých kategorií závažnosti (nízká, střední, vysoká). Další informace o výstrahách zabezpečení najdete v tématu [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> Můžete také využít toovisualize Microsoft Power BI data Security Center. Přečtěte si článek [Přehledné znázornění dat z Azure Security Center v řešení Power BI](security-center-powerbi.md).
> 
> 

### <a name="monitoring-for-new-or-changed-resources"></a>Sledování nových nebo změněných prostředků
Většina prostředí Azure je dynamická a pravidelně v nich probíhá přidávání a odebírání prostředků, konfigurace a další změny. Security Center pomáhá zajistit, že máte přehled o stavu zabezpečení hello těchto nových prostředků.

Při přidání nové prostředky (virtuální počítače, databáze SQL) tooyour prostředí Azure Security Center automaticky zjišťovat tyto prostředky a začít toomonitor jejich zabezpečení. To zahrnuje také webové role a role pracovního procesu PaaS. Pokud je povoleno shromažďování dat v hello [zásady zabezpečení](security-center-policies.md), další možnosti monitorování se automaticky povolí pro virtuální počítače.

![Klíčové oblasti](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. Pro virtuální počítače klikněte na **Compute** v části **Prevence**. Případné problémy s povolováním dat nebo související doporučení se zobrazí v hello **přehled** kartě a **monitorování doporučení** části.
2. Zobrazení hello **doporučení** toosee, případná bezpečnostní rizika byly zjištěny pro nový prostředek hello.
3. Je velmi běžné, že když tooyour prostředí se přidají nové virtuální počítače, jenom hello je nainstalován operační systém původně. vlastník prostředku Hello může být nutné některé čas toodeploy jiné aplikace, které se použijí tyto virtuální počítače.  V ideálním případě byste měli znát konečný záměr této úlohy hello. Bude se toobe aplikační Server? Jaký to na základě nová úloha je toobe probíhající, můžete povolit příslušné hello **zásady zabezpečení**, což je hello třetí krok v tomto pracovním postupu.
4. Přidávání nových prostředků tooyour prostředí Azure, je možné, že se v hello objeví nové výstrahy **výstrahy zabezpečení** dlaždici. Vždy ověřte, pokud na této dlaždici nejsou nové výstrahy a provádět akce podle doporučení tooSecurity Center.

Také můžete tooregularly hello stavu monitorování existující změn konfigurace tooidentify prostředky, které vyvolaly bezpečnostní rizika, odlišily z doporučených standardních hodnot a výstrahy zabezpečení. Spusťte na řídicím panelu Security Center hello. Zde máte tři hlavní oblasti tooreview pravidelně kontrolovat.

![Operace](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. Hello **prevence** části panel poskytuje rychlý přístup tooyour klíčovým prostředkům. Použijte tuto možnost toomonitor výpočty, sítě, úložiště a datům a aplikacím.
2. Hello **doporučení** panel vám umožní tooreview doporučení služby Security Center. Při průběžném sledování můžete zjistit, že nemáte doporučení na každý den, což je normální, protože jste všechna doporučení na hello počáteční nastavení Security Center. Z tohoto důvodu nesmí mít nové informace v této části každý den a bude jednoduše tooaccess jej podle potřeby.
3. Hello **detekce** části může měnit velmi často i velmi zřídka základ. Vždy zkontrolujte výstrahy zabezpečení a proveďte akce na základě doporučení služby Security Center.

## <a name="incident-response"></a>Reakce na incidenty
Security Center zjišťuje a upozorní vás toothreats, kdy k nim dojde. Organizace by měl monitorování pro nové výstrahy zabezpečení a provést akci jako další potřebné tooinvestigate nebo napravit hello útoku. Další informace o tom, jak detekce hrozeb služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).

Při tomto článku nemá záměrné tooassist hello vám s vytvořením plánu reakcí na incidenty, přidáme toouse odpověď zabezpečení společnosti Microsoft Azure v průběhu životního cyklu cloudu hello jako hello foundation pro fáze reakcí na incidenty. Následující diagram hello se zobrazuje Hello fázích:

![Podezřelá aktivita](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Můžete použít hello Národního institutu standardů a technologie (NIST) [počítače Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) jako tooassist odkaz můžete vytvářet vlastní.
> 

Výstrahy zabezpečení Center můžete použít během hello následujících fází:

* **Detekce**: Identifikace podezřelé aktivity v jednom nebo několika prostředcích. 
* **Vyhodnocení**: provedení počáteční assessment tooobtain hello Další informace o hello podezřelou aktivitu.
* **Diagnostika**: použijte hello nápravy kroky tooconduct hello technické postup tooaddress hello problém.

Každá výstraha zabezpečení poskytuje informace, které můžete použít toobetter pochopit hello povaha hello útoku a navrhněte možné způsoby zmírnění rizik. Některé výstrahy také poskytnout odkazy tooeither Další informace nebo tooother zdroje informací v rámci Azure. Hello informace k dispozici pro další zdroje informací a zmírnění toobegin můžete použít, a můžete také prohledat související se zabezpečením dat, která je uložená v pracovním prostoru.

Hello následující příklad ukazuje probíhající podezřelé aktivity protokolu RDP:

![Podezřelá aktivita](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Jak vidíte, toto okno zobrazuje podrobnosti týkající se hello čas proběhla hello útoku, hello název hostitele zdrojového, hello cílovém virtuálním počítači a obsahuje také doporučené kroky. V některých případech hello informace o zdroji útoku hello může být prázdná. Další informace o tomto typu chování najdete v článku [Chybějící informace o zdroji ve výstrahách služby Azure Security Center](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/).

V hello [jak tooLeverage hello Azure Security Center & Microsoft Operations Management Suite pro odpověď Incident](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) video se zobrazí některé ukázky, které vám mohou pomoci toounderstand použití Security Center v každé jeden z těchto fází.

> [!NOTE]
> Čtení [Leveraging Azure Security Center pro reakcí na incidenty](security-center-incident-response.md) pro další informace o tom, jak se toouse Security Center možnosti tooassist jste během reakcí na incidenty procesu. 
> 
> 

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili jak tooplan pro přijetí Security Center. toolearn Další informace o Security Center, najdete v části hello následující:

* [Správa a zpracování výstrah toosecurity v Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

