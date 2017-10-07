---
title: "aaaBest postupy pro podniky přesunutí tooAzure | Microsoft Docs"
description: "Popisuje zobrazení vygenerovaného uživatelského rozhraní, podniky, můžete použít tooensure zabezpečeného a spravovat prostředí."
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: 8692f37e-4d33-4100-b472-a8da37ce628f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: d1402cf21d0cf740e44c03fc345ecd39a6e1680c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatného
Podniky jsou stále přijetí hello veřejného cloudu pro jeho její agilnost a flexibilitu. Jsou jsou využitím hello cloudu síly toogenerate výnosy nebo optimalizovat prostředky pro hello firmy. Microsoft Azure poskytuje různé služby, aby podniky můžete sestavit jako stavební bloky tooaddress širokou škálu úlohy a aplikace. 

Ale zároveň budete vědět, kde je toobegin často složité. Jakmile se rozhodnete toouse Azure, běžně nastat několik otázek:

* "Jak mám splňují naše právní požadavky pro suverenity dat v některých zemích?"
* "Jak zjistím, že někdo nezmění nechtěně důležitých systémových?"
* "Jak poznám, co každý prostředků se nepodporuje, a proto I můžete účet pro ho a faktury přesně zpátky?"

potenciálních zákazníků Hello prázdný předplatného se žádná které ochrana je jednoduché. Tento prázdný prostor může zabránit spuštění vaší tooAzure move.

Tento článek obsahuje výchozí bod pro technické odborníky tooaddress hello požadavky zásad správného řízení a vyrovnávat její hello nutnost flexibility. Zavádí koncepci hello enterprise vygenerované uživatelské rozhraní, která provede organizace v implementaci a správu svých předplatných Azure. 

## <a name="need-for-governance"></a>Nutnost zásad správného řízení
Při přesunu tooAzure, musíte vyřešit, tématu hello zásad správného řízení časná tooensure hello úspěšné použití hello cloudu v rámci podniku hello. Bohužel hello čas a byrokracie vytváření komplexní zásad správného řízení systému znamená, že některé obchodní skupiny přejděte přímo toovendors bez zásahu podnikovém IT. Tento přístup můžete ponechat otevřené toovulnerabilities enterprise hello, pokud hello prostředky nespravuje správně. Hello vlastností veřejného cloudu hello - flexibility, flexibilitu a na základě spotřeby ceny - jsou důležité toobusiness skupin, které je třeba tooquickly splnily požadavky hello zákazníků (interních i externích). Ale organizace IT musí tooensure efektivně chráněných dat a systémy.

Ve skutečnosti generování uživatelského rozhraní je základem hello použité toocreate struktury hello. Hello vygenerované uživatelské rozhraní provede hello obecný postup a poskytuje ukotvení body pro více trvalých toobe systémy připojené. Vygenerované uživatelské rozhraní enterprise je hello stejné: Sada flexibilní ovládací prvky a možnosti Azure, které poskytují prostředí toohello struktura a kotvy pro služby založený na hello veřejného cloudu. Poskytuje hello počítačů (IT a obchodních skupin) foundation toocreate a připojte nové služby.

Hello vygenerované uživatelské rozhraní je založena na postupy, které jsme shromáždili z mnoha závazky s klienty různých velikostí. Tyto klienty v rozsahu od malých organizací vývoj řešení v hello cloudu tooFortune 500 podniky a nezávislé dodavatele softwaru, kteří jsou migrace a vývoji řešení v cloudu hello. Hello enterprise vygenerované uživatelské rozhraní je flexibilní toosupport "vytvořeného pro tento účel" toobe tradičních IT úlohy a agilní zatížení; například vývojáři, kteří vytvářejí aplikace software jako služba (SaaS) podle možnosti Azure.

Hello enterprise vygenerované uživatelské rozhraní je určený toobe hello základ pro každý nový odběr v rámci Azure. Umožňuje správci tooensure úlohy plnění hello minimální zásad správného řízení požadavky organizace bez brání rychle splňuje vlastní cíle obchodních skupin a vývojářů.

> [!IMPORTANT]
> Zásady správného řízení je zásadní toohello úspěch Azure. Tento článek cíle hello technickou implementaci vygenerované uživatelské rozhraní enterprise, ale pouze dotykem na širší proces hello a vztahy mezi součástmi hello. Zásady správného řízení zásad toků hello shora dolů a je určen podle jaké hello firmy chce tooachieve. Samozřejmě hello vytvoření modelu zásad správného řízení pro Azure zahrnuje zástupci oddělení IT, ale ještě důležitější musí mít silné reprezentace z žebříčky obchodní skupiny a zabezpečení a řízení rizik. V hello end vygenerované uživatelské rozhraní enterprise je zmírňování rizika toofacilitate obchodní zvláště a cíle organizace.
> 
> 

Následující obrázek Hello popisuje hello součástí hello vygenerované uživatelské rozhraní. Hello foundation spoléhá na plnou plán pro oddělení, účty a odběry. Hello pilíře obsahovat zásady Resource Manager a silné standardy pro vytváření názvů. Hello zbytek vygenerované uživatelské rozhraní hello pochází z základní možnosti Azure a že povolení funkce zabezpečení a spravovat prostředí.

![součásti vygenerované uživatelské rozhraní](./media/resource-manager-subscription-governance/components.png)

> [!NOTE]
> Azure se zvětšil rychle od jeho uvedení v 2008. Tento nárůst požadované Microsoft inženýrství týmy toorethink jejich přístup pro správu a nasazení služeb. modelu Azure Resource Manager Hello byla zavedena v 2014 a nahradí hello modelu nasazení classic. Resource Manager umožňuje organizacím toomore můžete snadno nasadit, uspořádání a řízení prostředků Azure. Správce prostředků obsahuje paralelizace při vytváření prostředků pro rychlejší nasazení řešení komplexní, vzájemně souvisí. Zahrnuje také řízení granulární přístupu a hello možnost tootag prostředky s metadaty. Společnost Microsoft doporučuje, abyste vytvořili všechny prostředky pomocí modelu Resource Manager hello. Hello enterprise vygenerované uživatelské rozhraní je explicitně určená pro hello modelu Resource Manager.
> 
> 

## <a name="define-your-hierarchy"></a>Definovat hierarchii
Hello základ pro vygenerované uživatelské rozhraní hello je hello Azure podnikového zápisu (a hello podnikový portál). Hello podnikového zápisu definuje tvar hello a využívají služby Azure v rámci společnosti a struktura zásad správného řízení základní hello. V rámci smlouvy enterprise hello, zákazníci mohou toofurther rozdělit hello prostředí do oddělení, účty a nakonec odběry. Předplatné Azure je základní jednotkou hello, kde jsou všechny prostředky obsažené. Definuje také několik omezení v rámci Azure, například na počtu jader, zdroje atd.

![Hierarchie](./media/resource-manager-subscription-governance/agreement.png)

Každý enterprise se liší a hello hierarchie v předchozí obrázek hello umožňuje významné flexibilitu v uspořádání Azure v rámci společnosti hello. Před implementací hello pokyny obsažené v tomto dokumentu, by měl modelu vaší hierarchii a pochopit hello dopad na fakturace, přístup k prostředkům a složitost.

jsou tři obecné vzory pro registrace Azure Hello:

* Hello **funkční** vzor
  
    ![funkční](./media/resource-manager-subscription-governance/functional.png)
* Hello **organizační jednotky** vzor 
  
    ![nejdůležitější.](./media/resource-manager-subscription-governance/business.png)
* Hello **geografické** vzor
  
    ![zeměpisná](./media/resource-manager-subscription-governance/geographic.png)

Můžete použít hello vygenerované uživatelské rozhraní v hello předplatné úrovně tooextend hello požadavky zásad správného řízení hello podniku do předplatného hello.

## <a name="naming-standards"></a>Standardy pro vytváření názvů
první pilíře Hello z hello vygenerované uživatelské rozhraní je pojmenování standardů. Dobře navrženou standardy pro vytváření názvů umožňují tooidentify prostředky hello portálu, faktury a v rámci skriptů. S největší pravděpodobností už máte pojmenování standardy pro místní infrastrukturu. Při přidávání Azure tooyour prostředí, by měl rozšířit ty pojmenování standardy tooyour prostředků Azure. Pojmenování standard usnadnění efektivnější správy hello prostředí na všech úrovních.

> [!TIP]
> Pro zásady vytváření názvů:
> * Zkontrolujte a přijmout, kde je to možné hello [Patterns and Practices pokyny](../guidance/guidance-naming-conventions.md). Tyto pokyny vám pomůže zjistit, na smysluplný standardní pojmenování.
> * Použijte camelCasing pro názvy prostředků (například myResourceGroup a vnetNetworkName). Poznámka: Existují některé prostředky, například účty úložiště, kde je jedinou možností hello toouse malá písmena (a žádné speciální znaky).
> * Zvažte použití tooenforce zásady (popsané v další části hello) Azure Resource Manager pojmenování standardů.
> 
> Hello předchozí tipy vám pomůžou při implementaci konzistentní zásady vytváření názvů.

## <a name="policies-and-auditing"></a>Zásady a auditování
druhý pilíře Hello z vygenerované uživatelské rozhraní hello zahrnuje vytvoření [zásady Azure Resource Manager](resource-manager-policy.md) a [auditování protokol aktivit hello](resource-group-audit.md). Správce prostředků zásady vám poskytují hello možnost toomanage rizika v Azure. Můžete definovat zásady, které zajišťují suverenity dat. omezení, vynucování nebo auditování určité akce. 

* Zásady je výchozí **povolit** systému. Akce řídit definice a přiřazení zásad tooresources, který odepřít nebo auditovat akce na prostředky.
* Zásady jsou popsané podle definice zásady v jazyce definice zásady (Pokud potom podmínky).
* Můžete vytvořit zásady formátované soubory s JSON (Javascript Object Notation). Po definování zásad, můžete je přiřadit konkrétní rozsah tooa: předplatné, skupinu prostředků nebo prostředek.

Zásady mají různé akce, které umožní použít scénáře tooyour podrobných přístup. jsou akce Hello:

* **Odepřít**: žádost o prostředku hello bloky
* **Audit**: umožňuje hello požadavek ale přidá protokol aktivit toohello řádku, (může to být použité tooprovide výstrahy nebo sady runbook tootrigger)
* **Připojit**: Přidá zadaný prostředek toohello informace. Například pokud na prostředku není značku "CostCenter", přidejte tuto značku výchozí hodnotu.

### <a name="common-uses-of-resource-manager-policies"></a>Běžná použití služby Správce prostředků zásady
Azure Resource Manager zásady jsou výkonný nástroj v hello Azure toolkit. Umožňují vám tooavoid neočekávané náklady, tooidentify center s náklady pro prostředky prostřednictvím označování a tooensure že dodržování předpisů požadavků. Zásady jsou v kombinaci s integrované funkce auditování hello, můžete dotazování komplexní a flexibilní řešení. Zásady umožňují společnosti tooprovide ovládací prvky pro úlohy "Tradičních IT" a "Agile" zatížení; například vývoj aplikací zákazníka. Hello většiny běžných vzorů, které vidíte pro zásady jsou:

* **Geograficky nebo data dodržování předpisů suverenity** -Azure poskytuje oblasti v rámci hello, world. Podniky chcete často toocontrol, kde jsou prostředky vytvořeny (jestli suverenity dat. tooensure nebo právě tooensure prostředky jsou vytvořeny zavřít toohello end spotřebiteli hello prostředků).
* **Správa nákladů** -předplatné služby Azure může obsahovat prostředky mnoho typů a škálování. Společnosti často chcete tooensure tento standard odběry nepoužívejte zbytečně velké prostředků, které může náklady stovky dolarů za měsíc nebo více.
* **Výchozí zásady správného řízení prostřednictvím požadované značky** -vyžadování značek je jedním nejběžnější a vysoce požadované funkce hello. Pomocí Azure Resource Manager zásad podniky jsou možné tooensure, že je prostředek správně označené. Hello nejběžnější značky jsou: oddělení, vlastník prostředku a prostředí typu (například - provozní, testovací, vývoj)

**Příklady**

"Tradičních IT" předplatné pro-obchodní aplikace

* Vynuťte oddělení a vlastník značek u všech zdrojů
* Omezení prostředků vytvoření toohello oblast Severní Americe
* Omezit možnost toocreate hello G-Series virtuálních počítačů a clusterů HDInsight

"Agile" prostředí pro organizační jednotku vytváření cloudových aplikací

* požadavky na toomeet dat suverenity, povolit hello vytváření prostředků pouze v určité oblasti.
* Vynuťte značky prostředí na všechny prostředky. Pokud prostředek je vytvořen bez značku, připojit hello **prostředí: Neznámý** značky toohello prostředků.
* Audit při prostředky se vytvoří mimo Severní Ameriku, ale nebudou bránit.
* Audit při vytvoření vysoce náklady na prostředky.

> [!TIP]
> Hello se nejčastěji používá správce prostředků zásad v organizacích toocontrol *kde* je možné prostředky vytvořit a *co* lze vytvořit typy prostředků. Kromě toho řídí tooproviding na *kde* a *co*, mnoho podniků použití zásady tooensure prostředky mít hello příslušná metadata toobill zpět pro používání. Doporučujeme uplatňovat zásady na úrovni předplatného hello pro:
> 
> * Suverenity geograficky nebo data dodržování předpisů
> * Správa nákladů
> * Požadované značky (ROZHODNUTY podle potřeby podniku, jako je například BillTo, majitel aplikace)
> 
> Můžete použít další zásady na nižších úrovních oboru.
> 
> 

### <a name="audit---what-happened"></a>Audit - co se stalo?
Jak funguje prostředí tooview, musíte tooaudit aktivity uživatelů. Většina typů prostředků v rámci Azure vytvořit diagnostických protokolů, které můžete analyzovat pomocí některého nástroje, protokolu nebo v Azure Operations Management Suite. Můžete shromáždit protokoly aktivity napříč více předplatných tooprovide oddělení nebo enterprise zobrazení. Záznamy auditu jsou důležité diagnostický nástroj a události zásadní mechanismus tootrigger v hello prostředí Azure.

Protokoly aktivity z nasazení Resource Manager umožňují toodetermine hello **operations** která trvala místní a kdo je provedl. Protokoly aktivity se můžou shromažďovat a agregovat pomocí nástroje, například analýzy protokolů.

## <a name="resource-tags"></a>Značky prostředku
Jak uživatelé ve vaší organizaci přidat prostředky toohello předplatné, bude stále důležité tooassociate prostředků s hello příslušné oddělení, zákazníků a prostředí. Je možné připojit tooresources metadat prostřednictvím [značky](resource-group-using-tags.md). Použijete značky tooprovide informace o prostředku hello nebo vlastníka hello. Značky umožňují toonot jenom agregační a skupiny prostředků různými způsoby, ale tato data použít pro účely hello vrácení peněz. Můžete označit prostředky s až too15 páry klíč: hodnota. 

Značky prostředku jsou flexibilní a musí být připojené toomost prostředky. Příklady běžných značky prostředku jsou:

* BillTo
* Oddělení (nebo organizační jednotka)
* Prostředí (vývoj pro produkční fázi)
* Úroveň (webová vrstva, aplikační vrstvě)
* Majitel aplikace
* Název projektu

![tags](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Další příklady značek najdete v tématu [doporučená zásady vytváření názvů pro prostředky Azure](../guidance/guidance-naming-conventions.md).

> [!TIP]
> Mějte na paměti, což zásadu, která vyžaduje označování pro:
> 
> * Skupiny prostředků
> * Úložiště
> * Virtuální počítače
> * Aplikační služby prostředí nebo webové servery
> 
> Tato strategie označování identifikuje ve vašich předplatných, jaká metadata jsou potřebné pro hello firmy, finance, zabezpečení, řízení rizik a celkové správy hello prostředí. 

## <a name="resource-group"></a>Skupina prostředků
Resource Manager umožňuje tooput prostředky do smysluplný skupin pro správu, fakturace nebo přírodní spřažení. Jak už bylo zmíněno dříve, Azure má dva modely nasazení. Starší klasického modelu, hello základní jednotkou správy v hello byla hello předplatného. Bylo obtížné toobreak ukončí prostředky v rámci předplatného, která vedla toohello vytvoření velkého počtu předplatných. Pomocí modelu Resource Manager hello jsme viděli hello Úvod skupin prostředků. Skupiny prostředků jsou kontejnery prostředky, které mají společné životního cyklu nebo sdílet atribut jako je například "všechny SQL servery" nebo "Aplikaci A".

Skupiny prostředků, nemůžou být obsažené v sobě navzájem a prostředky může patřit tooone skupinu prostředků. Některé akce můžete použít na všechny prostředky ve skupině prostředků. Odstranění skupiny prostředků, například odebere všechny prostředky v rámci skupiny prostředků hello. Obvykle umístíte celé aplikace nebo související systémová v hello stejnou skupinu prostředků. Například by obsahovat tři vrstvy aplikaci s názvem Contoso webové aplikace hello webový server, aplikační server a SQL server v hello stejnou skupinu prostředků.

> [!TIP]
> Jak uspořádat vaší skupiny prostředků může lišit "Tradičních IT" úlohy příliš "Agilní IT" úlohy:
> 
> * "Tradičních IT" úlohy jsou nejčastěji seskupené podle položky v rámci hello stejný životní cyklus, jako je například aplikace. Umožňuje seskupení aplikací pro správu jednotlivých aplikací.
> * "Agilní IT" úlohy jsou obvykle toofocus na externí zákazníkem cloudové aplikace. skupiny prostředků Hello odrážet hello vrstev nasazení (například webovou vrstvu, vrstvy aplikace) a správu.
> 
> Principy vaše úlohy usnadňuje vývoj strategie pro skupinu prostředků.

## <a name="role-based-access-control"></a>Řízení přístupu na základě role
Budete pravděpodobně žádáme sami "kdo má mít přístup tooresources?" a "jak lze řídit tento přístup?" Povolení nebo zákaz toohello přístup k portálu Azure a řízení přístupu tooresources portálu hello je zásadní. 

Pokud Azure byla původně, měla přístup ovládací prvky tooa předplatné základní: správce nebo spolusprávce. Přístup k předplatnému tooa hello Klasický model implicitní přístup tooall hello prostředky hello portálu. Kvůli chybějící jemně odstupňovanou kontrolu vedla toohello, jak narůstá počet předplatných tooprovide úroveň řízení přiměřené přístupu pro zápis služby Azure.

Tento, jak narůstá počet odběrů již není potřeba. Pomocí řízení přístupu na základě rolí můžete přiřadit uživatele toostandard role (například common "čtečky" a "zapisovače" typy rolí). Můžete také definovat vlastní role.

> [!TIP]
> tooimplement řízení přístupu na základě rolí:
> * Připojit vaše podnikové identitě úložiště (nejčastěji Active Directory) tooAzure služby Active Directory pomocí hello nástroji AD Connect.
> * Ovládací prvek hello správce nebo Spolusprávce předplatného pomocí spravovaného identity. **Nemáte** přiřadit správce nebo spolusprávce tooa nový vlastník předplatného. Místo toho použijte tooprovide role RBAC **vlastníka** práva tooa skupinu nebo jednotlivé.
> * Přidejte skupinu tooa Azure uživatele (například X vlastníci aplikace) ve službě Active Directory. Použijte hello synchronizoval skupiny tooprovide skupiny členy hello příslušná práva toomanage hello skupinu prostředků obsahující aplikaci hello.
> * Postupujte podle hello Princip přidělování hello **nejnižší oprávnění** požadované toodo hello očekává pracovní. Například:
>   * Nasazení skupiny: Skupina, která je jenom možné toodeploy prostředky.
>   * : Virtuální počítač A skupina pro správu Tedy možné toorestart virtuální počítače (pro operace)
> 
> Tyto tipy můžete spravovat přístup uživatelů v rámci vašeho předplatného.

## <a name="azure-resource-locks"></a>Zámky prostředků Azure.
Jak vaše organizace přidá jádra služby toohello předplatné, bude stále důležité tooensure, že tyto služby jsou k dispozici tooavoid obchodní přerušení. [Uzamčení prostředků](resource-group-lock-resources.md) umožňují toorestrict operací s vysokou hodnotou prostředky, jejichž změně nebo odstranění by mohlo mít významný dopad na vaší aplikace nebo cloudové infrastruktury. Můžete použít zámky tooa předplatné, skupinu prostředků nebo prostředek. Obvykle použijete zámky toofoundational prostředkům, například virtuální sítě, bran a účty úložiště. 

Uzamčení prostředků v současné době podporují dvou hodnot: CanNotDelete a jen pro čtení. CanNotDelete znamená, že uživatelé (s odpovídající práva hello) můžete stále číst nebo upravit prostředek, ale nelze ho proto odstranit. Jen pro čtení znamená, že oprávnění uživatelé nejde odstranit ani změnit prostředku.

zámky správy toocreate nebo odstranit, musíte mít přístup příliš`Microsoft.Authorization/*` nebo `Microsoft.Authorization/locks/*` akce.
Hello předdefinovaných rolí pouze vlastník a správce přístupu uživatelů mají tyto akce.

> [!TIP]
> Základní možnosti sítě by měly být chráněné s zámky. Nechtěnému odstranění bránu, site-to-site VPN by katastrofální tooan předplatného Azure. Azure vám neumožňuje toodelete virtuální síť, která se používá, ale použijí další omezení, která je užitečné opatření. 
> 
> * Virtuální sítě: CanNotDelete
> * Skupina zabezpečení sítě: CanNotDelete
> * Zásady: CanNotDelete
> 
> Zásady jsou také velmi důležitý toohello údržby odpovídající ovládacích prvků. Doporučujeme, abyste použili **CanNotDelete** zamknout toopolices, které jsou používány.

## <a name="core-networking-resources"></a>Základní síťové prostředky
Tooresources přístup může být (v rámci sítě hello corporation) interních nebo externích (prostřednictvím hello Internetu). Uživatelé ve vaší organizaci tooinadvertently put prostředky v pozici nesprávný hello snadno a potenciálně jejich otevření toomalicious přístup. Stejně jako u místní zařízení, musíte přidat podniky tooensure příslušných ovládacích prvků, že Azure uživatelé rozhodnutí hello správné. Pro řízení předplatné jsme identifikovat core prostředky, které poskytují základní řízení přístupu. Hello core prostředky obsahovat:

* **Virtuální sítě** jsou objekty kontejneru pro podsítě. Když je to nezbytně nutné, často se používá při připojování aplikace toointernal podnikovým prostředkům.
* **Skupin zabezpečení sítě** jsou podobné tooa brány firewall a poskytují pravidla pro jak prostředku může "kontaktovat" hello síti. Budou poskytovat podrobnou kontrolu nad jak / pokud podsíť (nebo virtuálního počítače) se můžete připojit toohello Internet nebo jiných podsítí ve hello stejné virtuální síti.

![Základní síťové služby](./media/resource-manager-subscription-governance/core-network.png)

> [!TIP]
> Pro sítě:
> * Vytvoření virtuální sítě vyhrazené tooexternal přístupem úlohy a úlohy interní přístupem. Tento přístup snižuje riziko hello nechtěně umístění virtuálních počítačů, které jsou určené pro interní úlohy v externí přístupných prostoru.
> * Nakonfigurujte přístup toolimit skupiny zabezpečení sítě. Minimálně blokovat toohello přístup k Internetu z interní virtuální sítě a blokování přístupu toohello podnikové síti z externí virtuální sítě.
> 
> Tyto tipy vám pomůžou při implementaci zabezpečení síťových prostředků.

### <a name="automation"></a>Automation
Správa prostředků jednotlivě není obě časově náročná a potenciálně chyby, které jsou náchylné k chybám pro určité operace. Azure poskytuje různé možnosti automatizace včetně Azure Automation, Logic Apps a Azure Functions. [Služby Azure Automation](../automation/automation-intro.md) umožňuje správci toocreate a definovat sady runbook toohandle běžné úkoly při správě prostředků. Vytváření runbooků pomocí editoru kódu prostředí PowerShell nebo grafický editor. Můžete vytvořit komplexní fáze více pracovních postupů. Služby Azure Automation je často používané toohandle běžné úkoly jako vypíná nepoužité zdroje nebo vytváření prostředků v odpovědi tooa konkrétní aktivace bez nutnosti lidského zásahu.

> [!TIP]
> Pro automatizaci:
> * Vytvoření účtu Azure Automation a zkontrolujte hello dostupné sady runbook (grafickém uživatelském rozhraní nebo příkaz řádku) k dispozici v hello [Galerie Runbooků](../automation/automation-runbook-gallery.md).
> * Import a přizpůsobit klíče sady runbook pro vlastní použití.
> 
> Obvyklým scénářem je hello možnost tooStart nebo vypnutí virtuálního počítače podle plánu. Existují příklad sady runbook, které jsou k dispozici v hello galerie, jak u tohoto scénáře a jak můžete naučit tooexpand ho.
> 
> 

## <a name="azure-security-center"></a>Azure Security Center
Možná jedním z hello největších blokování toocloud přijetí je už hello obavy nad zabezpečením. Správci IT riziko a zabezpečení oddělení potřebují tooensure, aby byly zabezpečené prostředky v Azure. 

Hello [Azure Security Center](../security-center/security-center-intro.md) poskytuje centrální zobrazení hello stav zabezpečení prostředků v hello předplatných a poskytuje doporučení, které pomáhají zabránit ohroženými prostředky. Podrobnější zásady (například použití zásady toospecific skupiny prostředků, které povolí hello enterprise tootailor jejich riziko toohello postojů, které budou adresování) ji můžete povolit. Nakonec Azure Security Center je otevřená platforma umožňující partnery společnosti Microsoft a nezávislé softwaru dodavatelé toocreate software, který připojuje do Azure Security Center tooenhance jeho možnosti. 

> [!TIP]
> Azure Security Center je povolená ve výchozím nastavení v každém předplatném. Však musí povolit shromažďování dat z virtuálních počítačů tooallow Azure Security Center tooinstall jeho agenta a spustíte shromažďování dat.
> 
> ![Shromažďování dat](./media/resource-manager-subscription-governance/data-collection.png)
> 
> 

## <a name="next-steps"></a>Další kroky
* Teď, když jste se naučili o zásad správného řízení předplatné, je čas toosee tato doporučení v praxi. V tématu [příklady implementace zásad správného řízení předplatného Azure](resource-manager-subscription-examples.md).

