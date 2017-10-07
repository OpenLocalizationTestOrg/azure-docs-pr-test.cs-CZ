---
title: "aspekty návrhu rozšíření aaaAzure služby Active Directory hybridní identity - určit hybridní identity životního cyklu přijetí strategii | Microsoft Docs"
description: "Pomáhá definovat hello hybridní identity úlohy správy podle toohello možnosti dostupné pro jednotlivé fáze životního cyklu."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 420b6046-bd9b-4fce-83b0-72625878ae71
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 86ec0a9896f069bc93e49e06006954848f8e4d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Určení strategii přijetí životního cyklu hybridní identity
V této úloze definujete hello strategie správy identit pro hybridní identity řešení toomeet hello podnikové požadavky definované v [určit úlohy správy hybridní identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).

toodefine hello hybridní identity úlohy správy podle toohello identity začátku do konce cyklu uvedené výše v tomto kroku, budete mít tooconsider hello možnosti dostupné pro jednotlivé fáze životního cyklu.

## <a name="access-management-and-provisioning"></a>Správa přístupu a zřizování
Řešení pro správu přístupu k funkčním účet vaší organizace můžete sledovat přesněji kdo má přístup k informacím toowhat v organizaci hello.

Řízení přístupu je důležité funkce systému zřizování centralizovaný, jeden bod. Kromě chránit citlivé informace, řízení přístupu vystavit existující účty, které mají neschválených oprávnění nebo se již nebude nutné. zastaralé účty toocontrol hello systému odkazy společně účet informace o zřizování s autoritativní informace o hello uživatelé, kteří vlastní účty hello. Informace o identitě autoritativní uživatele se obvykle udržuje v hello databáze a adresáře lidských zdrojů.

Účty v sofistikované podniky zahrnují stovky parametry, které definují hello autority a tyto podrobnosti můžete ovlivnit pomocí systému zřizování. Noví uživatelé je možné zjistit, které poskytujete z autoritativní zdroj hello daty hello. Možnosti schvalování žádosti o přístup Hello zahájí hello procesy, které schválit (nebo odmítnout) zřizování pro jejich prostředků.

| Fáze životního cyklu správy | Místní | Cloud | Hybridní |
| --- | --- | --- | --- |
| Správa účtů a zřizování |Pomocí role serveru hello Active Directory® Domain Services (AD DS) můžete vytvářet škálovatelnou, zabezpečenou a zvladatelnou infrastrukturu pro správu uživatelů a prostředků a poskytovat podporu pro práci s adresáři aplikace, jako je Microsoft® Exchange Server. <br><br> [Můžete zřídit skupiny ve službě AD DS prostřednictvím Identity manager](https://technet.microsoft.com/library/ff686261.aspx) <br>[Můžete zřídit uživatelů ve službě AD DS](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Správci mohou pomocí tooshared přístup k prostředkům pro přístup k řízení toomanage uživatele z bezpečnostních důvodů. Ve službě Active Directory, je Správa řízení přístupu na úrovni objektu hello podle nastavení různé úrovně přístupu nebo oprávnění, tooobjects, jako je například Úplné řízení, zápisu, čtení nebo žádný přístup. Jak budou různí uživatelé definuje řízení přístupu ve službě Active Directory můžete použít objekty služby Active Directory. Ve výchozím nastavení jsou nastavena toohello nejbezpečnější nastavení oprávnění u objektů ve službě Active Directory. |Toocreate máte účet pro každého uživatele, kteří budou přistupovat ke cloudové službě Microsoftu. Můžete také změnit uživatelské účty nebo je odstranit, když jste už nepotřebují. Ve výchozím nastavení uživatelé nebudou mít oprávnění správce, ale můžete volitelně přiřadit. Další informace najdete v tématu [Správa uživatelů ve službě Azure AD](active-directory-create-users.md). <br><br> V rámci Azure Active Directory je jednou z hlavních funkcí hello hello možnost toomanage přístup tooresources. Tyto prostředky můžou být součástí hello adresář, jako v případě hello oprávnění toomanage objektů pomocí rolí v adresáři hello nebo prostředky, které jsou externí toohello adresáři, například aplikace SaaS, služby Azure a weby Sharepointu nebo místní prostředky. <br><br> V hello je řešení pro správu přístupu center z Azure Active Directory je skupina zabezpečení hello. vlastník prostředku Hello (nebo hello správce adresáře hello) můžete přiřadit skupiny tooprovide určitých přístup správné toohello prostředků, které vlastní. Hello členy skupiny hello bude poskytnuta hello přístup a vlastník prostředku hello můžete delegovat hello správné toomanage hello seznamu členy skupiny toosomeone jiný – například Správce oddělení nebo technickou podporu správce<br> <br> Správa skupin Hello v tématu Azure AD poskytuje další informace o správě přístupu pomocí skupin. |Rozšíření do cloudu hello prostřednictvím synchronizace a federaci identit služby Active Directory |

## <a name="role-based-access-control"></a>Řízení přístupu na základě role
Řízení přístupu na základě role (RBAC) používá role a zřizování zásad tooevaluate, testování a vynucovat podnikové procesy a pravidla pro poskytování přístupu toousers. Klíče správci vytvářet zásady zřizování a přiřadit uživatelům tooroles a které definují sady tooresources oprávnění pro tyto role. RBAC rozšiřuje hello identity řešení toouse softwaru na základě procesů správy a snížit ruční zásah uživatele v hello procesu zřizování.
Azure AD RBAC umožňuje hello společnosti toorestrict hello množství operací, které konkrétního můžete provést po má tooAzure přístup k portálu pro správu. Pomocí RBAC toocontrol přístup toohello portál blíží správci IT certifikační autority delegovat přístup pomocí hello následující správu přístupu:

* **Přiřazení role na základě skupiny**: můžete přiřadit přístup tooAzure AD skupin, které mohou být synchronizovány z místní služby Active Directory. To vám umožní tooleverage hello investovali, které vaše organizace má provést v nástroji a procesů pro správu skupin. Můžete také použít funkce správy delegované skupiny hello Azure AD Premium.
* **Využívání, které jsou součástí role v Azure**: tři role je možné použít – tooensure vlastník, Přispěvatel a čtečky, aby uživatelé a skupiny mají oprávnění toodo pouze hello úlohy toodo potřebují svou práci.
* **Podrobné přístup tooresources**: můžete přiřadit toousers rolí a skupin pro určitý odběr, skupinu prostředků nebo jednotlivých prostředků Azure, jako je například na webu nebo v databázi. Tímto způsobem zajistíte, že uživatelé mají přístup k prostředkům hello tooall, které potřebují a žádné tooresources přístup, že toomanage není nutné.

## <a name="provisioning-and-other-customization-options"></a>Zřizování a další možnosti přizpůsobení
Váš tým, můžete použít obchodní požadavky a plány toodecide kolik toocustomize hello identity řešení. Velký podnik může například vyžadovat postupné zavádění plán pro pracovní postupy a vlastní adaptéry, které je založené na časovou osu pro přírůstkově zřizování aplikací široce používaných v zeměpisných. Pro dva nebo více aplikací toobe zřízené v rámci celé organizace, po úspěšném otestování můžete získat z jiného vlastního nastavení plánu. Interakce uživatele aplikace se dají přizpůsobit a postupy pro zřizování prostředků může být změněné tooaccommodate automatické zřizování.

Můžete zrušení zřízení tooremove služba nebo komponenta. Zrušení zřízení účtu například znamená, že hello účet je odstraněný z prostředku.

model hybridní Hello zřizování prostředků kombinuje požadavku a na základě rolí přístupů, které jsou podporovány službou Azure AD. Pro podmnožiny zaměstnanci nebo spravované systémy může být vhodné firma tooautomate přístup s přiřazením na základě rolí. Firma může také zpracovat všechny požadavky na přístup nebo výjimky prostřednictvím modelu na základě požadavku. Některé podniky mohou začínat ruční přiřazení a momentální směrem k hybridní model, se záměr nasazení plně na základě rolí v budoucnosti.

Jiných společností, může být nepraktické pro obchodní důvody tooachieve dokončení na základě rolí zřizování a cíl přístup hybridní jako požadovaného cíle. Stále jiných společností nemusí být splněna pouze založené na požadavku zřizování a chcete tooinvest další úsilí toodefine a spravovat zásady na základě rolí, automatické zřizování.

## <a name="license-management"></a>Správa licencí
Správa licencí na základě skupiny ve službě Azure AD umožňuje správci přiřadit skupiny zabezpečení users tooa a Azure AD automaticky přiřadí licence tooall hello členy skupiny hello. Pokud uživatel je následně přidány do nebo odebrat ze skupiny hello, licence bude automaticky přiřazen nebo odebrat podle potřeby.

Můžete použít skupiny, kterou synchronizujete z místní AD nebo spravovat ve službě Azure AD. To párování s Azure AD premium Samoobslužná správa skupiny můžete snadno delegovat licence přiřazení toohello odpovídající vedoucím pracovníkům. Můžete si být jistí, že jsou problémy, jako je v konfliktu licencí a chybějící data o umístění automaticky seřazené.

## <a name="self-regulating-user-administration"></a>Samoobslužné regulační Správa uživatele
Při spuštění tooprovision prostředky v organizacích všechny interní vaší organizaci byste implementovat funkce správy uživatelů samoobslužné regulační hello. Výhody hello a výhody zřizování uživatelů můžou realizovat přes její hranice. V tomto prostředí je změna stav uživatele, odrazí v přístupová práva přes hranice organizace a zeměpisných automaticky. Můžete snížit náklady na zřizování a zefektivnit hello přístup a schválení procesy. implementace Hello rozpoznává hello úplné potenciální implementaci řízení přístupu na základě rolí pro správu přístupu začátku do konce ve vaší organizaci. Můžete snížit náklady na správu prostřednictvím automatizované postupy pro řídících zřizování uživatelů. Můžete zlepšit zabezpečení tím, že automatizuje vynucení zásad zabezpečení a zjednodušit a centralizovat správu životního cyklu uživatele a zřizování prostředků pro plnění velké uživatele.

> [!NOTE]
> Další informace najdete v tématu Nastavení služby Azure AD pro samoobslužnou správu přístupu aplikace
> 
> 

Na základě licencí (nárok na základě) Azure AD služby pracovní podle aktivaci předplatného v klientovi directory nebo služby Azure AD. Jakmile hello předplatné je aktivní možnosti služby hello můžete spravují správci nástroje directory nebo služby a používá licencovaní uživatelé. Další informace najdete v tématu Jak funguje licencování pracovní Azure AD?
Integrace s poskytovatelé další 3.

Azure Active Directory poskytuje jednotné přihlašování a rozšířené toothousands zabezpečení přístup k aplikaci SaaS aplikací a místní webové aplikace. Podrobný seznam podporovaných aplikací SaaS v galerii aplikací Azure Active Directory najdete v tématu seznam kompatibility federace Azure Active Directory: poskytovatelů identit třetích stran, které se dají použít tooimplement jednotného přihlašování

## <a name="define-synchronization-management"></a>Definování správy synchronizace
Integrace místních adresářů se službou Azure AD zvyšuje produktivitu uživatelů tím, že jim poskytuje společnou identitu pro přístup ke cloudovým i místním prostředkům. Díky této integraci uživatelům a organizacím můžete využít výhod hello následující:

* Organizace můžou poskytovat uživatelům jednotnou identitu hybridní přes místní nebo cloudové služby využívají Windows Server Active Directory a následným připojením tooAzure služby Active Directory.
* Správci můžou poskytovat podmíněný přístup na základě prostředků aplikace, zařízení a identity uživatele, umístění v síti a vícefaktorového ověřování.
* Uživatelé mohou využívat jejich společnou identitu prostřednictvím účty v Azure AD tooOffice 365, Intune, SaaS aplikace a aplikace třetích stran.
* Vývojáři mohou vytvářet aplikace, které využívají hello společný model identity, integrace aplikací do služby Active Directory v místě nebo Azure pro cloudové aplikace

Hello následující obrázek je příkladem široký přehled o procesu synchronizace identit.

![](./media/hybrid-id-design-considerations/identitysync.png)

Proces synchronizace identit

Projděte si následující tabulky toocompare hello synchronizace možnosti hello:

| Možnost správy synchronizace | Výhody | Nevýhody |
| --- | --- | --- |
| Na základě synchronizace (prostřednictvím DirSync nebo službu AADConnect.) |Uživatelé a skupiny synchronizované z místní a cloudové <br>  **Řízení zásad**: zásady účtů lze nastavit pomocí služby Active Directory, která umožňuje hello správce hello možnost toomanage zásady hesel, pracovní stanice, omezení, se na více systémů zámek ovládací prvky a další, bez nutnosti tooperform další úlohy v cloudu hello.  <br>  **Řízení přístupu**: můžete omezit přístup toohello cloudové služby tak, aby hello služby je možné přistupovat prostřednictvím hello podnikovém prostředí, prostřednictvím online servery nebo oba. <br>  Snížená volání podpory: Pokud uživatelé používají méně tooremember hesla, jsou méně pravděpodobné, že tooforget je. <br>  Zabezpečení: Identity uživatelů a informace jsou chráněny, protože všechny servery hello a služby jsou používány v jednotné přihlašování, řídí se hlavním a řídí místně. <br>  Podpora pro silné ověřování: silného ověřování (také nazývané dvoufaktorové ověřování) můžete použít s cloudovou službou hello. Ale pokud používáte silné ověřování, musíte použít jednotné přihlašování. | |
| Na základě Federation (prostřednictvím služby AD FS) |Povolit pomocí služby tokenů zabezpečení (STS). Při konfiguraci služby tokenů zabezpečení tooprovide jednoho přihlášení přístup s cloudovou službou Microsoft, bude vytvoření federovaného vztahu důvěryhodnosti mezi vaší místní služby tokenů zabezpečení a hello federovanou doménu, kterou jste určili v klientovi služby Azure AD. <br> Koncovým uživatelům umožňuje toouse hello stejné nastavit přihlašovací údaje tooobtain přístup toomultiple prostředků <br>koncoví uživatelé nemají toomaintain více sad přihlašovací údaje. Ještě hello uživatelé mají tooprovide jejich přihlašovací údaje tooeach jeden hello zúčastněných prostředků., scénáře B2B a B2C podporována. |Vyžaduje specializované pracovníky pro nasazení a údržby vyhrazené místní servery služby AD FS. Existují omezení použití hello silné ověřování, pokud máte v plánu toouse služby AD FS pro vaši službu tokenů zabezpečení. Další informace najdete v tématu [konfigurace rozšířené možnosti pro službu AD FS 2.0](http://go.microsoft.com/fwlink/?linkid=235649). |

> [!NOTE]
> Další informace najdete v tématu [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
> 
> 

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

