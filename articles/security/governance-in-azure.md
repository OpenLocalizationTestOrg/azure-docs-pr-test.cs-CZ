---
title: aaaGovernance v Azure | Microsoft Docs
description: "Další informace o cloudové výpočetní služby, které zahrnují široký výběr výpočetních instancích & služby, které je možné škálovat nahoru a dolů automaticky toomeet hello potřebám vaší aplikace nebo enterprise."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/01/2017
ms.author: TomSh
ms.openlocfilehash: 956e82e92f4232c24069bdb79fed5315f1d6486f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="governance-in-azure"></a>Zásady správného řízení v Azure

Víme, že je zabezpečení úlohy, jeden v cloudu hello a jak důležité je, že zjistíte přesné a aktuální informace o zabezpečení Azure. Jedním z hello nejlepší toouse důvodů Azure pro vaše aplikace a služby je tootake výhod jeho širokou škálu zabezpečení nástroje a možnosti. Tyto nástroje a možnosti pomáhat při jeho možné toocreate zabezpečené řešení na platformu Azure zabezpečené hello.

toohelp, které lépe porozumíte hello pole ovládacích prvků zásad správného řízení implementována v rámci Microsoft Azure z obou hello zákazníka a Microsoft operations perspektivy, v tomto článku "Zásad správného řízení v Azure", je zapsán, která poskytuje komplexní pohled na hello Zásady správného řízení funkcí dostupných v Microsoft Azure.

## <a name="azure-platform"></a>Platforma Azure

Azure je platforma služby veřejného cloudu, která podporuje široký výběr operačních systémů, programovací jazyky, rozhraní, nástroje, databází a zařízení. Může probíhat Linux kontejnery s integrací Dockers; vývoj aplikací pomocí jazyka JavaScript, Python, .NET, PHP, Java a Node.js; sestavení back EndY pro iOS, Android a Windows zařízení. Služby veřejného cloudu Azure podporovat hello stejné technologie miliony vývojářů a IT profesionály už spoléhají na a vztah důvěryhodnosti.

Pokud sestavení nebo IT prostředky se mají migrovat, poskytovatele služeb veřejného cloudu se spoléhat na dané organizace dalo tooprotect vaší aplikace a data se službami hello a ovládací prvky hello poskytují toomanage hello zabezpečení vašeho cloudu prostředky.

Infrastruktury Azure a je určená z hello tooapplications zařízení pro hostování miliony zákazníků současně, a poskytuje trustworthy foundation, na kterém může podnikům splňovat požadavky jejich zabezpečení. Kromě toho Azure poskytuje mnoho možností zabezpečení a toocontrol možnost hello je tak, aby si můžete přizpůsobit toomeet hello jedinečné požadavky na zabezpečení vaší organizace nasazení.

Tento dokument vám pomůže pochopit, jak možnosti zásad správného řízení Azure vám mohou pomoci splnit tyto požadavky.

## <a name="abstract"></a>Abstraktní

Zásady správného řízení cloudu Microsoft Azure poskytuje integrované auditu a konzultační přístup pro kontrolu a poradenství organizace na jejich použití hello platformy Azure. Microsoft Azure cloud zásad správného řízení odkazuje toohello rozhodovacích procesů, kritéria a zásady zahrnutých v hello plánování, architektury, získávání, nasazení, operace a správy cloudu computing.

toocreate plán pro Microsoft Azure cloud zásad správného řízení, budete muset tootake hlubší pohled na hello osoby, procesy a technologie aktuálně a pak sestavení rozhraní, které usnadňují IT tooconsistently podporu obchodních potřeb při současném poskytování end Uživatelé se hello flexibilitu toouse hello výkonné funkce Microsoft Azure.

Tento dokument popisuje, jak můžete dosáhnout zvýšené úrovni zásad správného řízení prostředků vaší IT v Microsoft Azure. Tento dokument vám může pomoct pochopit funkce zabezpečení a zásad správného řízení hello součástí tooMicrosoft Azure.

Hello následují hlavní hello zásad správného řízení problémy popsané v tomto dokumentu:

- Implementace zásady, procesy a postupy podle cíle organizace.

- Zabezpečení a průběžné dodržování standardů organizace

- Monitorování a výstrahy

## <a name="implementation-of-policies-processes-and-procedures"></a>Implementace zásady, procesy a postupy 

Správa navázal rolích a zodpovědnostech toooversee implementací hello zásady zabezpečení informací a provozní kontinuitu v Azure. Správa Microsoft Azure zodpovídá za dohled nad zabezpečením a postupy kontinuity v rámci svých příslušných týmů (včetně třetích stran) a usnadnit dodržování zásad zabezpečení, procesy a standardy.

Zde jsou hello faktory vyvinuly:

- Zřizování účtů

- Ovládací prvky předplatného

- Řízení přístupu na základě role

- Správa prostředků

- Sledování prostředku

- Ovládací prvek kritické prostředku

- Přístup pomocí rozhraní API tooBilling informace

- Ovládací prvky sítě

## <a name="account-provisioning"></a>Zřizování účtů

Definování účet hierarchie je hlavní krok toouse a struktura služby Azure v rámci společnosti a struktura zásad správného řízení základní hello. V případě zákazníků s hello smlouvy enterprise můžete rozdělit zákazníkům další hello prostředí do oddělení, účty a nakonec odběry.

![Zřizování účtů](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Pokud nemáte smlouvu enterprise agreement, zvažte použití [Azure značky](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) na předplatné toodefine úrovni hierarchie. Předplatné Azure je základní jednotkou hello, kde jsou všechny prostředky obsažené. Definuje také několik omezení v rámci Azure, například na počtu jader, zdroje atd. Může obsahovat odběry [skupiny prostředků](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), která může obsahovat prostředky. [RBAC](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control) zásady se vztahují na tyto tři úrovně.

Každý enterprise se liší a hello hierarchii pomocí značek Azure v případě jiných podnikových zákazníků umožňuje významnou flexibilitu uspořádání Azure v rámci společnosti hello. Před nasazením prostředky v Microsoft Azure, by měl modelu hierarchie a pochopit hello dopad na fakturace, přístup k prostředkům a složitost.

## <a name="subscription-controls"></a>Ovládací prvky předplatného

Předplatné Určuje, jak hlášené a účtují využití prostředků. Odběry, může být instalační program pro samostatné fakturace a platby. Jako uvedených starší v rámci jednoho účtu Azure jsme může mít několik odběrů. Odběry se dá použít toodetermine hello využití prostředků Azure více oddělení v rámci podniku.

Například, pokud má společnosti oddělení IT oddělení lidských zdrojů a uvádění na trh a tyto oddělení mají různé projekty systémem. Na základě hello využití prostředků Azure stejně jako virtuální počítače každé oddělení, se můžou účtovat odpovídajícím způsobem. To jsme můžete řídit finance hello jednotlivých oddělení.

Předplatná Azure vytvořit tři parametry:

- ID jedinečný odběratele

- fakturace umístění

- Sadu dostupných prostředků

Pro jednotlivá, která bude zahrnovat jeden ID pro účet Microsoft, platební karty číslo a hello úplná sada Azure services – i když Microsoft vynucuje omezení spotřeby, v závislosti na typu předplatného hello.

Azure registrace hierarchií definovat, jak jsou strukturovaná služby v rámci smlouvy Enterprise. Hello podnikový portál umožňuje zákazníkům toodivide přístup tooAzure prostředky přidružené k smlouvu Enterprise Agreement, na základě potřeb organizace přizpůsobitelné tooan flexibilní hierarchií jedinečný. vzor hierarchie Hello by měl odpovídat správy a geografické struktura organizace tak, aby hello přidruženého fakturace a přístupu k prostředkům můžou být přesně pozornost.

Hello tři vzory vysoké úrovně jsou funkční, obchodní jednotky a geografické, pomocí oddělení jako pro správu konstrukce pro účet seskupení. V rámci každé oddělení můžete účty přiřazené odběry, které vytvoření sila pro fakturaci a několik klíčů omezení v Azure (například počet virtuální počítače, účty služby storage atd.).

![Ovládací prvky předplatného](./media/governance-in-azure/security-governance-in-azure-fig2.png)


Pro organizace s smlouvu Enterprise Agreement předplatná Azure, postupujte podle čtyři úrovně hierarchie:

- registrace podnikového správce.

- Správce oddělení

- vlastníka účtu

- Správce služeb

Tato hierarchie řídí hello následující:

- Vztah fakturace

- Účet správy

- Tooartifacts role řízení přístupu na základě (RBAC)

- Hranice nebo omezení

- Hranice

  - Využití a fakturace (sazebník podle čísla nabídka)

  - Omezení

  - Virtual Network

- Připojit too1 AAD (1 AAD být přidružen mnoho odběrů)

- Účet přidružený tooan podnikové registrace

## <a name="role-based-access-controls"></a>Řízení přístupu na základě rolí

Pokud Azure byla původně, měla přístup ovládací prvky tooa předplatné základní: správce nebo spolusprávce. Přístup k předplatnému tooa hello Klasický model implicitní přístup tooall hello prostředky hello portálu. Kvůli chybějící jemně odstupňovanou kontrolu vedla toohello, jak narůstá počet předplatných tooprovide úroveň řízení přiměřené přístupu pro zápis služby Azure.

![Řízení přístupu na základě rolí](./media/governance-in-azure/security-governance-in-azure-fig3.png)

Tento, jak narůstá počet odběrů již není potřeba. Pomocí řízení přístupu na základě rolí můžete přiřadit uživatele toostandard role (například common "čtečky" a "zapisovače" typy rolí). Můžete také definovat vlastní role.

[Azure na základě rolí řízení přístupu (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) umožňuje vyladění správy přístupu pro Azure. Pomocí RBAC, můžete udělit pouze hello množství přístup tito uživatelé si musí tooperform svou práci. Zaměřené na zabezpečení společnosti by měla soustředit na poskytnutí zaměstnanci hello přesný oprávnění, které potřebují. Příliš mnoho oprávnění vystavit tooattackers účtu. Příliš málo oprávnění znamená, že zaměstnanci nelze práci efektivně. Azure na základě rolí řízení přístupu (RBAC) pomáhá vyřešit tento problém tak, že nabídka vyladění správy přístupu pro Azure. RBAC vám pomůže toosegregate povinností v rámci vašeho týmu a udělit pouze hello množství toousers přístup, potřebují tooperform svou práci. Namísto udělení každý uživatel neomezený oprávnění v vašeho předplatného Azure nebo prostředky, můžete povolit jenom určité akce.

Například použijte RBAC toolet jednoho zaměstnance spravovat virtuální počítače v předplatném, zatímco jiné můžete spravovat SQL databáze, v rámci hello stejné předplatné.

Azure RBAC má tři základní rolí, které se vztahují tooall typy prostředků:

- **Vlastník** má úplný přístup k prostředkům tooall včetně tooothers přístup správné toodelegate hello.

- **Přispěvatel** můžete vytvářet a spravovat všechny typy prostředků Azure, ale nelze udělit přístup tooothers.

- **Čtečka** můžete zobrazit stávající prostředky Azure.

Hello zbytek hello RBAC role v Azure povolit správu konkrétních prostředků Azure. Například hello role Přispěvatel virtuálních počítačů umožňuje toocreate hello uživatele a spravovat virtuální počítače. Nedává je přístup toohello virtuální sítě nebo hello podsítě, která hello virtuální počítač připojí k.

[Předdefinované role RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) seznamy hello role v Azure k dispozici. Určuje hello operace a že každý předdefinovaná role uděluje toousers oboru.

Udělit přístup přiřazením toousers hello odpovídající RBAC role, skupiny a aplikace v určité oboru. předplatné, skupinu prostředků nebo jediný zdroj, může být Hello rozsah přiřazení role. Role přiřazené v nadřazeném oboru také uděluje přístup toohello podřízené objekty jsou v něm obsažena.

Například uživatel s skupiny prostředků tooa přístupu můžete spravovat všechny hello prostředky, které obsahuje, jako jsou weby, virtuální počítače a podsítě.

Azure RBAC jen podporuje management operace hello prostředky služby Azure v hello portál Azure a rozhraní API Správce Azure Resource Manager. Všechny operace úrovně dat pro prostředky Azure se nejde autorizovat. Například může autorizovat někdo nelze toomanage účty úložiště, ale není toohello objektů BLOB nebo tabulky v rámci účtu úložiště. Podobně databáze SQL je možné spravovat, ale není hello tabulek v něm.

Další informace o tom, jak vám řízení přístupu na základě role v Azure pomůže spravovat přístup uživatelů najdete v článku [Co je řízení přístupu na základě role](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is).

Můžete také [vytvořit vlastní roli](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) v řízení řízení přístupu (RBAC), pokud žádná z předdefinovaných rolí hello splňují konkrétní přístup potřebuje. Můžete vytvořit vlastní role pomocí [prostředí Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), [rozhraní příkazového řádku Azure (CLI)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli)a hello [REST API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Stejně jako předdefinovaných rolí vlastní role přiřadit toousers, skupinám a aplikacím na předplatné, skupinu prostředků a prostředků obory.

V rámci každé předplatné můžete udělit too2000 přiřazení rolí.

## <a name="resource-management"></a>Správa prostředků

Azure původně zadat pouze hello modelu nasazení classic. V tomto modelu všechny prostředky existovaly nezávisle; nebyl způsob toogroup související prostředky společně. Místo toho musíte toomanually sledovat prostředky, ke kterým skládá řešení nebo aplikace a zapamatovat si toomanage je v koordinovaný přístup.

toodeploy řešení, bylo tooeither vytvořit každého prostředku jednotlivě prostřednictvím portálu classic hello nebo skript, který nasadit všechny prostředky hello ve správném pořadí hello. toodelete řešení, bylo toodelete každého prostředku jednotlivě. Není snadno můžete použít a aktualizovat zásady řízení přístupu pro související prostředky. Nakonec nelze aplikovat značky tooresources toolabel je s podmínkami, které vám pomůžou sledovat vaše prostředky a spravovat fakturace.

V roce 2014 si uvedla Azure Resource Manager, která přidá hello konceptu skupinu prostředků. Skupina prostředků je kontejner pro prostředky, které sdílejí společné životního cyklu. Hello modelu nasazení Resource Manager poskytuje několik výhod:

- Můžete nasadit, spravovat a monitorovat všechny hello služby pro vaše řešení jako skupina, nikoli samostatně zpracování těchto služeb.

- Můžete opakovaně nasadit řešení v průběhu životního cyklu a mít přitom jistotu, že vaše prostředky jsou nasazeny v konzistentním stavu.

- Přístup k řízení tooall prostředkům můžete použít ve vaší skupině prostředků a tyto zásady budou automaticky použita při přidávání nových prostředků toohello skupinu prostředků.

- Můžete použít značky tooresources toologically uspořádat všechny prostředky hello ve vašem předplatném.

- JavaScript Object Notation (JSON) toodefine hello infrastruktury můžete použít pro vaše řešení. soubor JSON Hello se označuje jako šablony Resource Manageru.

- Můžete definovat hello závislosti mezi prostředky, takže se nasadí ve správném pořadí hello.

![Správa prostředků](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Resource Manager umožňuje tooput prostředky do smysluplný skupin pro správu, fakturace nebo přírodní spřažení. Jak už bylo zmíněno dříve, Azure má dva modely nasazení. V dříve hello [modelu Classic](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model), hello základní jednotkou správy byla hello předplatného. Bylo obtížné toobreak ukončí prostředky v rámci předplatného, která vedla toohello vytvoření velkého počtu předplatných. Pomocí modelu Resource Manager hello jsme viděli hello Úvod skupin prostředků.

Skupina prostředků je kontejner, který obsahuje související prostředky pro řešení s Azure. [Skupina prostředků Hello](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) může zahrnovat všechny hello prostředky pro řešení hello nebo jenom prostředky, které chcete toomanage jako skupina. Můžete určit, jak mají prostředky tooallocate tooresource skupin podle díky hello nejvhodnější pro vaši organizaci.

Další doporučení k šablonám najdete v tématu [Osvědčené postupy pro vytváření šablon Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager analyzuje závislosti, které tooensure prostředky vytvoří ve správném pořadí hello. Pokud jeden prostředek závisí na hodnotě z jiného prostředku (například virtuální počítač potřebuje účet úložiště pro disky), nastavíte závislost.

>[!Note]
>Další informace najdete v tématu [Definování závislostí v šablonách Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies).

Hello šablonu můžete použít také pro aktualizace toohello infrastrukturu. Můžete například přidat řešení tooyour prostředek a konfigurační pravidla pro hello prostředky, které jsou už nasazené. Pokud hello Šablona specifikuje vytvoření prostředku, ale tento prostředek již existuje, Azure Resource Manager provede aktualizaci místo vytvoření nového prostředku. Azure Resource Manager aktualizace hello existující asset toohello stejné stavu, protože by byl nový.

Pokud potřebujete další operace, například při instalaci softwaru, který není zahrnutý v instalačním programu hello Resource Manager poskytuje rozšíření pro scénáře.

## <a name="resource-tracking"></a>Sledování prostředku

Jak uživatelé ve vaší organizaci přidat prostředky toohello předplatné, bude stále důležité tooassociate prostředků s hello příslušné oddělení, zákazníků a prostředí. Můžete připojit tooresources metadat prostřednictvím značky. Používáte [značky](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) tooprovide informace o prostředku hello nebo vlastníka hello. Značky umožňují toonot jenom agregační a skupiny prostředků několika způsoby, ale tato data použít pro účely hello vrácení peněz.

Použití značek, když máte komplexní kolekci prostředků a jejich skupin a potřebujete toovisualize tyto prostředky hello takovým způsobem, který umožňuje hello většina tooyou smysl. Můžete například označit prostředky, které ve vaší organizaci poskytovat podobnou roli nebo patří toohello stejného oddělení.

Bez značky, uživatelé ve vaší organizaci můžete vytvořit různé prostředky, které může být obtížné toolater identifikovat a spravovat. Například můžete toodelete všechny hello prostředky pro projekt. Pokud tyto prostředky nejsou označeny pro hello projekt, musíte je ručně vyhledat. Označení může také hrát důležitou roli můžete tooreduce zbytečných nákladů ve vašem předplatném.

Prostředky nemusí tooreside v hello stejné tooshare skupiny prostředků značku. Můžete vytvořit vlastní značky taxonomii tooensure, že všichni uživatelé ve vaší organizaci používat společné značky spíše než neúmyslně zavádět mírně odlišné značky (třeba odd"místo"oddělení").

Zásady prostředků povolit toocreate standardní pravidla pro vaši organizaci. Můžete vytvořit zásady, které zajišťují, že prostředky jsou označené hello příslušné hodnoty.

> [!Note]
> Další informace najdete v tématu [zásad prostředků pro značky](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags).

Můžete také zobrazit s příznakem prostředkům prostřednictvím hello portálu Azure.

Hello [sestav využití](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) pro vaše předplatné zahrnuje značka názvy a hodnoty, což vám umožní toobreak out náklady podle značky.

> [!Note]
> Další informace o značkách najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags).

Hello následující omezení platí tootags:

- Každý prostředek nebo skupina prostředků může mít maximálně 15 páry klíč – hodnota značky. Toto omezení se vztahuje pouze skupinu prostředků toohello tootags použité přímo nebo prostředek. Skupiny prostředků může obsahovat mnoho prostředků nichž každá má 15 páry klíč – hodnota značky.

- název značky Hello je omezená too512 znaků.

- Hodnota značky Hello je omezená too256 znaků.

- Skupina prostředků toohello značky použít nejsou zdědí hello prostředky v příslušné skupině prostředků.

Pokud máte více než 15 hodnoty, je nutné, aby tooassociate s prostředku, použijte pro hodnotu značky hello řetězec formátu JSON. Hello řetězce formátu JSON může obsahovat mnoho hodnoty, které jsou použité tooa jedinou značku klíč.

### <a name="tags-and-billing"></a>Značky a fakturace

Značky povolit jste toogroup fakturační údaje. Například pokud používáte víc virtuálních počítačů pro jiné organizace, použijte hello značky toogroup využití podle nákladové středisko. Můžete použít také náklady toocategorize značky prostředí runtime; například hello fakturace využití pro virtuální počítače spuštěné v provozním prostředí.

Můžete načíst informace o značkách prostřednictvím hello [využití prostředků Azure a rozhraní API RateCard](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) nebo soubor hodnot oddělených čárkami (CSV) využití hello. Stáhnout soubor využití hello z hello [portál účtů Azure](https://account.windowsazure.com/) nebo [EA portál](https://ea.azure.com/).

>[!Note]
> Další informace o programový přístup toobilling informace najdete v tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). Operace REST API, najdete v části [referenční dokumentace rozhraní API Azure fakturace REST](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Když si stáhnete hello použití sdíleného svazku clusteru pro služby, které podporují značky s fakturace, značek hello se zobrazí ve sloupci hello značky.

## <a name="critical-resource-controls"></a>Ovládací prvky kritické prostředků

Jak vaše organizace přidá jádra služby toohello předplatné, bude stále důležité tooensure, že tyto služby jsou k dispozici tooavoid obchodní přerušení. [Uzamčení prostředků](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) umožňují toorestrict operací s vysokou hodnotou prostředky, jejichž změně nebo odstranění by mohlo mít významný dopad na vaší aplikace nebo cloudové infrastruktury. Můžete použít zámky tooa předplatné, skupinu prostředků nebo prostředek. Obvykle použijete zámky toofoundational prostředkům, například virtuální sítě, bran a účty úložiště.

Uzamčení prostředků v současné době podporují dvou hodnot: CanNotDelete a jen pro čtení. CanNotDelete znamená, že uživatelé (s odpovídající práva hello) můžete stále číst nebo upravit prostředek, ale nelze ho proto odstranit. Jen pro čtení znamená, že oprávnění uživatelé nejde odstranit ani změnit prostředku.

Správce prostředků zámky použít pouze toooperations, který dojít v rovině, správu hello, která se skládá z operace odeslané příliš<https://management.azure.com>. hello zámky neomezují jak prostředky provádět vlastní funkce. Změny prostředku jsou s omezeným přístupem, ale operace prostředků nejsou s omezeným přístupem. Například jen pro čtení zámku v databázi SQL brání odstranění nebo úprava hello databáze, ale nezabrání vytváření, aktualizaci nebo odstranění dat v databázi hello.

Použití **jen pro čtení** může způsobit toounexpected výsledky, protože některé operace, které vypadají podobně jako pro čtení, operace vyžadují další akce. Například umístění **jen pro čtení** výpis klíčů hello všem uživatelům zabrání zámku na účet úložiště. Zobrazí seznam Hello operace klíče se zpracovává prostřednictvím požadavek POST, protože hello vrátit klíče jsou k dispozici pro operace zápisu.

![Ovládací prvky kritické prostředků](./media/governance-in-azure/security-governance-in-azure-fig5.png)

Další příklad uvedení zámek jen pro čtení na prostředek aplikace služby zabrání Průzkumníka serveru Visual Studia zobrazení souborů pro prostředek hello vzhledem k tomu, že interakce vyžaduje oprávnění k zápisu.

Na rozdíl od řízení přístupu na základě rolí použijte správu zámky tooapply omezení ve všech uživatelů a rolí. toolearn o nastavení oprávnění pro uživatele a rolí, najdete v části [řízení přístupu na základě Role v Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

Když použijete zámku v nadřazeném oboru, všechny prostředky v rámci tohoto oboru dědit hello stejné zámku. I prostředky, které přidáte později dědí z nadřazené hello hello zámku. nejvíc omezující zámku Hello v dědičnosti hello přednost.

zámky správy toocreate nebo odstranit, musíte mít přístup tooMicrosoft.Authorization/ _nebo Microsoft.Authorization/locks/_ akce. Hello předdefinovaných rolí, pouze **vlastníka** a **správce přístupu uživatelů** mají tyto akce.

## <a name="api-access-toobilling-information"></a>Informace o rozhraní API access toobilling

Pomocí rozhraní API Správce Azure fakturace toopull využití a prostředků dat do vaší nástrojů pro analýzu upřednostňované data. Hello využití prostředků Azure a rozhraní API RateCard vám může pomoct přesně předpovědět a náklady na správu. Hello rozhraní API jsou implementovány jako poskytovatel prostředků a součástí rodiny hello rozhraní API vystavené hello Azure Resource Manager.

### <a name="azure-resource-usage-api-preview"></a>Rozhraní API (Preview) pro využití prostředků Azure.

Použití hello Azure [API využití prostředků](https://msdn.microsoft.com/library/azure/mt219003) tooget data Odhadované využití platformy Azure. Hello rozhraní API obsahuje:

- **Řízení přístupu Azure na základě rolí** -konfigurace přístupu zásady na hello [portál Azure](https://portal.azure.com/) nebo pomocí [rutin prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) toospecify, které uživatele nebo aplikace můžete získat přístup data o využití toohello předplatné. Volající musí používat standardní tokeny služby Azure Active Directory pro ověřování. Přidejte hello volající tooeither hello fakturace čtečky čtečky, vlastníka nebo přispěvatele role tooget toohello data o využití přístupu pro konkrétní předplatné Azure.

- **Hodinové nebo denní agregace** – volající můžete určit, jestli chtějí jejich Azure využití dat každou hodinu intervalů nebo denních intervalů. Výchozí hodnota Hello je denně.

- **Instance metadat (zahrnuje značky prostředku)** – získání podrobností na úrovni instance jako hello identifikátor uri prostředku plně kvalifikovaný (/subscriptions/ {id předplatného} /..), hello informace o skupině prostředků a značky prostředku. Tato metadata vám pomůže deterministicky a prostřednictvím kódu programu přidělit využití podle hello značky pro případy použití jako mezi poplatků.

- **Metadata prostředků** – prostředek podrobnosti, například hello měření název, kategorie měření, měření dílčí kategorie, jednotky a oblast poskytnout hello volajícího lépe porozumět tomu, co se spotřebovala. Také pracujeme tooalign prostředků metadata terminologie napříč hello portálu Azure, Azure použití sdíleného svazku clusteru, EA fakturace sdíleného svazku clusteru a dalších činnostech veřejné toolet vazbu mezi data v prostředí.

- **Použití všech nabízet typy** – data o využití je k dispozici pro všechny typy jako průběžné platby, MSDN, peněžních závazků, peněžního kreditu, který a EA nabízejí.

**Prostředek Azure API RateCard (Preview)**

Použití hello rozhraní API služby Azure prostředků RateCard tooget hello seznam dostupných prostředků Azure a odhadované ceny informace pro každý. Hello rozhraní API obsahuje:

- **Řízení přístupu Azure na základě rolí** – konfigurace zásad přístupu na hello portál Azure nebo prostřednictvím toospecify rutin prostředí Azure PowerShell, což uživatelům a aplikacím můžete získat přístup k datům RateCard toohello. Volající musí používat standardní tokeny služby Azure Active Directory pro ověřování. Přidejte hello volající tooeither hello čtečky, vlastníka nebo přispěvatele role tooget toohello data o využití přístupu pro konkrétní předplatné Azure.

- **Podpora pro průběžné platby, MSDN, peněžních závazků a peněžního kreditu, který nabízí (EA není podporován)** – tato rozhraní API poskytuje Azure míra nabídka úroveň informace. Hello volající toto rozhraní API musí předat hello nabídka informace tooget prostředků podrobnosti a sazby. Nyní nelze tooprovide EA sazby nám, protože nabízí EA přizpůsobili sazby za registraci. Tady jsou některé hello scénáře, které lze vytvořit pomocí kombinace hello hello využití a hello RateCard rozhraní API:

- **Azure tráví v měsíci hello** -použití hello kombinace hello využití a rozhraní API RateCard tooget lepší přehled o vašem cloudu tráví v měsíci hello. Můžete analyzovat hello každou hodinu a odhadne denních intervalů využití a poplatků.

- **Nastavení výstrah** – pomocí hello využití a hello rozhraní API RateCard tooget odhadované využívání cloud a poplatky a nastavení na základě prostředků nebo peněžní na základě výstrah.

- **Předpovídat faktury** – Get odhadované spotřeby a cloud tráví a použít strojového učení toopredict algoritmy, jaké hello faktury bude na konci hello hello fakturační cyklus.

- **Předběžné spotřeba analýza nákladů** – použít toopredict RateCard API hello kolik vaše faktura by byl očekávané využití při přesunutí tooAzure vaše úlohy. Pokud máte existující úlohy v ostatních cloudů nebo privátní cloudy, můžete také mapovat vašeho využití s tooget Azure sazby hello tráví lepší odhad Azure. Tento odhad poskytuje hello možnost toopivot na nabídku a porovnání mezi typy různých nabídka hello nad rámec průběžné platby, jako peněžních závazků a peněžního kreditu. také poskytuje Hello API hello možnost toosee náklady rozdíly podle oblasti a umožňuje vám toodo toohelp analýzy citlivostních náklady provedete rozhodnutí o nasazení.

- **Analýz** -můžete určit, zda je víc úloh nákladově efektivní toorun v jiné oblasti nebo na jinou konfiguraci hello prostředků Azure. Prostředků Azure, které náklady se můžou lišit podle hello oblast Azure, kterou používáte.

- Můžete také určit, pokud jiný typ nabídky Azure poskytuje lepší rychlost na prostředek služby Azure.

## <a name="networking-controls"></a>Ovládací prvky sítě

Tooresources přístup může být (v rámci sítě hello corporation) interních nebo externích (prostřednictvím hello Internetu). Uživatelé ve vaší organizaci tooinadvertently put prostředky v pozici nesprávný hello snadno a potenciálně jejich otevření toomalicious přístup. Stejně jako u místní / zařízení, musíte přidat podniky tooensure příslušných ovládacích prvků, že Azure uživatelé rozhodnutí hello správné.

![Ovládací prvky sítě](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Pro řízení předplatné jsme identifikovat core prostředky, které poskytují základní řízení přístupu. Hello core prostředky obsahovat:

### <a name="network-connectivity"></a>Připojení k síti

[Virtuální sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) jsou objekty kontejneru pro podsítě. Když je to nezbytně nutné, často se používá při připojování aplikace toointernal podnikovým prostředkům. Hello Azure Virtual Network service umožňuje toosecurely můžete připojit prostředky Azure tooeach jiné s virtuálními sítěmi (virtuální sítě).

Virtuální síť je reprezentace vlastní sítě v cloudu hello. Virtuální síť je to logická izolace cloudu Azure vyhrazeného tooyour předplatné hello. Můžete také připojit virtuální sítě tooyour do místní sítě.

Následují možnosti pro virtuální sítě Azure:

- **Izolace**: virtuální sítě jsou izolované od sebe navzájem. Můžete vytvořit samostatné virtuální sítě pro vývoj, testování a produkci této hello použijte stejné bloky adres CIDR. Naopak můžete vytvořit více virtuálních sítí, použít jiné bloky adres CIDR a společně připojení sítě. Virtuální síť můžete rozdělit do několika podsítí. Azure poskytuje interní překlad adres pro virtuální počítače a instance rolí cloudové služby připojený tooa virtuální sítě. Volitelně můžete nakonfigurovat virtuální síť toouse vlastní servery DNS, místo použití Azure interní překlad adres.

- **Připojení k Internetu**: cloudových služeb pro všechny virtuální počítače Azure (VM) a instancí rolí tooa připojené virtuální sítě mají přístup k Internetu, toohello ve výchozím nastavení. Můžete také povolit příchozí přístup k prostředkům toospecific, podle potřeby.

- **Připojení prostředků Azure**: prostředky Azure, jako je například cloudových služeb a virtuálních počítačů může být připojené toohello stejnou virtuální síť. Hello prostředky můžete připojit tooeach jiných použití privátních IP adres, i když jsou v různých podsítích. Azure poskytuje výchozí směrování mezi podsítěmi, virtuálními sítěmi a místními sítěmi, takže nemáte tooconfigure a spravovat trasy.

- **Připojení k virtuální síti**: virtuální sítě může být připojené tooeach jiné, povolení prostředky připojené virtuální sítě toocommunicate tooany s jakýmikoli prostředky na ostatní virtuální sítě.

- **Místní připojení**: virtuální sítě můžou být připojené tooon místní sítě prostřednictvím privátní sítě připojení mezi vaší sítí a Azure, nebo připojení site-to-site VPN přes hello Internet.

- **Filtrování provozu**: virtuálního počítače a cloudové služby role instance síťového provozu je možné filtrovat příchozí a odchozí zdrojové IP adresy a portu, cílové IP adresy a portu a protokolu.

- **Směrování**: Volitelně můžete přepsat výchozí Azure směrování konfiguraci vlastních tras, nebo pomocí trasy protokolu BGP prostřednictvím brány sítě.

## <a name="network-access-controls"></a>Ovládací prvky pro přístup k síti

[Skupin zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) jsou například Brána firewall a poskytují pravidla pro jak prostředku může "kontaktovat" hello síti. Budou poskytovat podrobnou kontrolu nad jak / pokud podsíť (nebo virtuálního počítače) se můžete připojit toohello Internet nebo jiných podsítí ve hello stejné virtuální síti.

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, které povolí nebo zakáže přenášení tooresources provoz sítě připojené tooAzure virtuálních sítí (VNet). Skupiny Nsg můžou být přidružené toosubnets, jednotlivé virtuální počítače (klasické), nebo jednotlivých síťových rozhraní (NIC) připojených tooVMs (Resource Manager).

Pokud skupinu NSG přidružená tooa podsíť, hello pravidla použít tooall prostředky připojené toohello podsítě. Provoz se dá dál omezit tím, že také přidružíte NSG tooa virtuálního počítače nebo síťový adaptér.

## <a name="security-and-continuous-compliance-with-organizational-standards"></a>Zabezpečení a průběžné dodržování standardů organizace

Každý firmy má různé požadavky a každou obchodní bude vytěžit maximum odlišné výhody z cloudové řešení. Zákazníci nejrůznějších mít dál, hello stejné základní obavy o přesunutí toohello cloudu. Chtějí tooretain řízení svých dat a chtějí mít tento toobe data uchovávat zabezpečení a privátní, všechny při zachování průhlednost a dodržování předpisů.

Zákazníci chcete od poskytovatelů cloudu je:

- **Zabezpečení našich dat** při to v úvahu, že může poskytovat hello cloudu vyšší zabezpečení dat a administrativní řízení, IT žebříčky jsou stále problémem, migrace toohello cloudu ponechá je zranitelnější toohackers než jeho aktuální interní řešení.

- **Ponechat naše data** soukromé cloudové služby vyvolat výzvy jedinečný ochrany osobních údajů pro firmy. Když společnosti vypadal toohello cloudu toosave náklady na infrastrukturu a zvýšit jejich flexibilitě, budou také obávat ztráty řízení toho, kde mají uložená data, který je k ní přistupují a jak získá používají.

- **Sdělte nám řízení** to i v případě jejich využít výhod hello cloudu toodeploy další inovativní řešení, jsou velmi zajímá neztratili kontrolu nad svá data společnosti. poslední zveřejňování Hello vládních agentur přístup k datům zákazníka prostřednictvím znamená, právních i velmi právní, zkontrolujte některé ředitelé informačních technologií opatrní ukládání svých dat v cloudu hello.

- **Zvýšit úroveň průhlednosti** zabezpečení, ochrany osobních údajů a řízení jsou důležité toobusiness rozhodnutí ve firmě, chtějí hello možnost tooindependently ověřte jak jejich data ukládají, přístupu a zabezpečené.

- **Udržovat kompatibilitu** společností rozšířit jejich používání cloudových technologií, hello složitost a obor standardů a nařízení dál tooevolve. Společnosti potřebují tooknow, který budou splněny jejich standardů dodržování předpisů, a že dodržování předpisů bude momentální jako předpisy změn v průběhu času.

## <a name="security-configuration-monitoring-and-alerting"></a>Konfigurace zabezpečení, monitorování a výstrahy

Předplatitelé služby Azure mohou svoje cloudová prostředí spravovat z více zařízení. Můžou k tomu využívat pracovní stanice, počítače vývojářů a dokonce i privilegovaná zařízení koncových uživatelů, která mají oprávnění ke konkrétním úlohám. V některých případech se funkce správy provádějí prostřednictvím webových konzol, například hello portálu Azure. V jiných případech může být tooAzure přímé připojení z místních systémů prostřednictvím virtuálních privátních sítí (VPN), terminálových služeb, protokolů klientských aplikací nebo (prostřednictvím kódu programu) hello rozhraní API pro správu služby Azure (SMAPI). Kromě toho můžou být koncové body klienta buď připojené k doménám nebo izolované a nespravované, jako například tablety nebo smartphony.

I když více možností přístup a správu nabízejí bohatou sadu možností, může tato variabilita zvýšit významné riziko tooa cloudu nasazení. Může být obtížné toomanage, sledování a audit akcí správy. Tato variabilita může také zvýšit ohrožení bezpečnosti prostřednictvím neregulovaného přístupu koncových bodů tooclient, které se používají pro správu cloudových služeb. Používání obecných nebo osobních pracovních stanic k vývoji a správě infrastruktury otevírá možnosti útoků z nečekaných směrů, například prohlížení webu (např. útok typu watering hole) nebo e-mailu (např. sociální inženýrství a phishing).

Sledování, protokolování a auditování poskytují základ pro sledování a pochopení aktivit správy, ale nemusí být vždy proveditelné tooaudit podrobností kvůli toohello objemu vygenerovaných dat dokončení všech akcí v. Auditování účinnosti zásad správy hello hello je nejlepším postupem je ale.

Zabezpečení Azure zásad správného řízení z toocontrol GPO v AD DS Windows všichni správci hello rozhraní, například sdílení souborů. Zahrňte pracovní stanice pro správu do procesů auditování, sledování a protokolování. Sledujte všechny přístupy a chování správců a vývojářů.

### <a name="azure-security-center"></a>Azure security center

Hello [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) poskytuje centrální zobrazení hello stav zabezpečení prostředků v hello předplatných a poskytuje doporučení, které pomáhají zabránit ohroženými prostředky. Podrobnější zásady (například použití zásady toospecific skupiny prostředků, které povolí hello enterprise tootailor jejich riziko toohello postojů, které budou adresování) ji můžete povolit.

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Security Center poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, které byste jinak nevšimli a spolupracuje s řadou řešení zabezpečení. Po povolení [zásady zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-policies) pro prostředky předplatného, Security Center analyzuje hello zabezpečení vaše prostředky tooidentify potenciální ohrožení zabezpečení. Informace o konfiguraci vaší sítě jsou k dispozici okamžitě.

Azure Security Center představuje kombinaci osvědčených postupů analýzy a zabezpečení správy zásad pro všechny prostředky v rámci předplatného Azure. Tento nástroj efektivní a snadno toouse umožňuje týmy zabezpečení a rizika tooprevent osob, zjistit a reagovat toosecurity hrozby, jak automaticky shromažďuje a analyzuje data zabezpečení z vaše prostředky Azure, hello sítě a řešení partnerů, jako antimalwarové programy a brány firewall.

Kromě toho Azure Security Center používá pokročilou analýzu, včetně machine learningu a analýzy chování při využívání globální analýzou hrozeb z Microsoft produktů a služeb společnosti Microsoft digitální činů jednotky (DCU), hello hello Microsoft Security Response Center (MSRC) a externích informačních kanálů. [Zásady správného řízení zabezpečení](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) lze použít široce na úrovni předplatného hello nebo co nejlépe určen toospecific, podrobné požadavky použít tooindividual prostředků prostřednictvím definic zásad.

Nakonec Azure Security Center analyzuje stav zabezpečení prostředků na základě těchto zásad a používá tento tooprovide pronikavého řídicí panely a výstrahy pro události, například zjištění malwaru nebo škodlivý připojení IP pokusy.

>[!Note]
> Další informace o tom, tooapply doporučení, přečtěte si [implementace doporučení zabezpečení v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Security Center shromažďuje data z vaší virtuální počítače tooassess jejich stavu zabezpečení, zadejte doporučení zabezpečení a výstrah toothreats. Pokud nejprve přístup k Security Center, shromažďování dat je povolené na všechny virtuální počítače ve vašem předplatném. Doporučuje se shromažďování dat ale můžete můžete odhlásit pomocí [zakázání shromažďování dat](https://docs.microsoft.com/azure/security-center/security-center-faq) v hello zásad Security Center.

Nakonec Azure Security Center je otevřená platforma umožňující partnery společnosti Microsoft a nezávislé softwaru dodavatelé toocreate software, který připojuje do Azure Security Center tooenhance jeho možnosti.

Azure Security Center monitoruje hello následující prostředky Azure:

- Virtuální počítače (VM) (včetně cloudové služby)

- Virtuální sítě Azure

- Služba Azure SQL

- Partnerských řešení integrovaných ve vašem předplatném Azure například brány firewall webových aplikací na virtuálních počítačích a na [App Service Environment](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme).

### <a name="operations-management-suite"></a>Operations Management Suite

Hello OMS vývoj softwaru a služeb zabezpečení informací týmu a [zásad správného řízení programu](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) podporuje požadavky na jeho firmy a dodržuje toolaws a zákonům o, jak je popsáno v [Microsoft Azure Trust Center ](https://azure.microsoft.com/support/trust-center/) a [dodržování předpisů Centrum zabezpečení Microsoft](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Jak zřídit požadavky na zabezpečení OMS, identifikuje ovládací prvky zabezpečení, spravuje a monitoruje rizika jsou také popsány existuje. Ročně, jsme zkontrolujte zásady, standardy, postupy a pokyny.

Každý člen týmu vývoj OMS obdrží formální aplikace bezpečnostního školení. Interně používáme systém správy verzí pro vývoj softwaru. Každý projekt softwaru je chráněn hello systém správy verzí.

Společnost Microsoft nemá tým zabezpečení a dodržování předpisů, který dohlíží a vyhodnocuje všechny služby ve službě Microsoft. Informace o zabezpečení osob tvoří tým hello a nejsou přidruženi s hello technici oddělení, které vyvíjet OMS. Hello zabezpečení osoby mít vlastní řetězec správy a provedení nezávislé posuzování produktů a služeb tooensure zabezpečení a dodržování předpisů.

Služby Operations Management Suite (OMS) je kolekce služeb pro správu, které byly navrženy v cloudu hello od začátku hello. Namísto nasazení a správa na místní prostředky, jsou součástí OMS zcela hostované v Azure. Konfigurace je minimální a během několika minut můžete začít pracovat.

![Sada Operations Manager](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Právě vzhledem k tomu OMS služby běží cloudu hello neznamená, že nelze spravují efektivně v místním prostředí.

Uvedené agenta na všechny Windows nebo počítač se systémem Linux v datovém centru a odešle data tooLog analýzy, kde lze analyzovat spolu se všemi ostatními daty získané z cloudu nebo na místní služby. Pro zálohování a vysokou dostupnost pro místní prostředky pomocí Azure Backup a Azure Site Recovery tooleverage hello cloudu.

Sady Runbook v cloudu hello nelze obvykle přístup k prostředkům místně, ale můžete nainstalovat agenta na jeden nebo více počítačů příliš, který bude hostitelem sady runbook ve vašem datovém centru. Při spuštění sady runbook, jednoduše zadejte, zda se má toorun v hello cloudu nebo na místní pracovní.

Hello základní funkce služby OMS poskytuje sadu služby, které běží v Azure. Každá služba poskytuje funkce správy specifických a můžete kombinovat scénářů tooachieve různých správy služeb.

![Sada Operations Manager](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Operace systému Azure manager rozšiřuje jeho funkce tím, že poskytuje řešení pro správu. [Řešení pro správu](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) jsou hotových sady logiky, které implementují scénáři správy využívá jednu nebo více služeb OMS.

![Spravovat operace systému Azure](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Různá řešení jsou dostupné od Microsoftu a partnerů, můžete snadno přidat tooyour předplatného Azure tooincrease hello hodnotu investice v OMS.

Jako partner můžete vytvořit vlastní řešení toosupport vašim aplikacím a službám a poskytněte toousers prostřednictvím hello Azure Marketplace nebo šablony rychlý Start.

## <a name="performance-alerting-and-monitoring"></a>Monitorování a výstrahy výkonu

### <a name="alerting"></a>Zobrazení výstrah

Metoda monitorování metriky prostředků Azure, události nebo protokoly se výstrahy a oznámení při zadávání podmínku splníte.

**Výstrahy v různých služeb Azure**

Výstrahy jsou k dispozici mezi různé služby, včetně:

- Application Insights: Umožňuje test webu a metriky výstrahy.

>[!Note]
> V tématu [nastavit výstrahy ve službě Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) a [sledování dostupnosti a odezvy žádné webu](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- Analýzy protokolů (Operations Management Suite): Umožňuje hello směrování tooLog aktivity a diagnostické protokoly analýzy. Služby Operations Management Suite umožňuje metrika, log a ostatní typy výstrah.

>[!Note]
> Další informace najdete v tématu výstrahy v [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- Azure monitorování: Umožňuje výstrahy na základě hodnoty metriky a aktivity protokolu události. Můžete použít hello [REST API služby Azure monitorování](https://msdn.microsoft.com/library/dn931943.aspx) toomanage výstrahy.

>[!Note]
> Další informace najdete v tématu [pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku toocreate výstrahy](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Monitorování

Problémy s výkonem v cloudové aplikace může mít vliv na vaši firmu. S více vzájemně propojena součástmi a často verzích může dojít, degradations kdykoli. A pokud vyvíjíte aplikace, uživatelé obvykle zjistit problémy, které nebyl nalezen v testování. Měli vědět o tyto problémy okamžitě a mít nástroje pro diagnostiku a řešení problémů hello. Microsoft Azure obsahuje řadu nástrojů pro identifikaci těchto problémů.

**Jak se monitorování Moje Azure cloudových aplikací?**

Není řadu nástrojů pro monitorování aplikací Azure a služby. Některé z jejich funkcí překrývat. Toto je částečně historických důvodů a částečně kvůli toohello stírá mezi vývojovým týmem a operace aplikace.

Zde jsou hlavní nástroje hello:

- **Azure monitorování** je základní nástroj pro monitorování služby spuštěné v Azure. Nabízí data na úrovni infrastruktury o hello propustnost služby a které obaluje prostředí hello. Pokud spravujete své aplikace v Azure, rozhodování, zda tooscale nahoru nebo dolů prostředky, pak monitorování Azure vám dává můžete použít toostart.

- **Application Insights** lze použít pro vývoj a jako výrobní řešení monitorování. Funguje tak, že instalaci balíčku do vaší aplikace a proto nabízí více interní zobrazení co se děje. Jeho data zahrnují odezvy závislostí, výjimek trasování, ladění snímky, profily spuštění. Poskytuje výkonné inteligentní nástroje pro analýzu tuto telemetrii obou toohelp ladění toohelp chápete, co uživatelé dělají s ním a aplikace. Můžete zjistit, zda je kvůli Špička při odezvy toosomething v aplikaci nebo některé externí resourcing problém. Pokud používáte Visual Studio a aplikace hello je při selhání, můžete děláte správné toohello problém řádky kódu, můžete ji opravit.

- **Analýza protokolu** je pro uživatele, kteří potřebují tootune výkon a plán údržby na aplikace běžící v produkčním prostředí. Pracuje v Azure. Shromažďuje a agreguje data z mnoha zdrojů, když se zpožděním 10 minut too15. Poskytuje komplexní řešení pro správu IT pro Azure, místní a cloudové infrastruktuře jiných výrobců (například Amazon Web Services). Poskytuje tooanalyze data širší nástroje napříč více zdrojů, umožňuje komplexní dotazy napříč všechny protokoly a může aktivně upozornit na zadaných podmínek. Můžete dokonce shromáždění vlastních dat do své centrální úložiště, takže se můžete dotazovat a vizualizovat ho.

- **System Center Operations Manager (SCOM)** je pro správu a monitorování instalace velké cloudu. Je již obeznámeni s ním jako nástroj pro správu pro místní server systému Windows a na základě technologie Hyper-V-cloudy, ale můžete také integrovat a spravovat aplikace Azure. Kromě jiných věcí ho můžete nainstalovat na existující živé aplikace Application Insights. Pokud aplikace přestane fungovat, zde zjistíte v sekundách.


## <a name="next-steps"></a>Další kroky

- [Osvědčené postupy pro vytváření šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

- [Příklady implementace zásad správného řízení předplatného Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-examples).

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/).
