---
title: "Ovládací prvky v Azure Active Directory podmíněného přístupu | Microsoft Docs"
description: "Zjistěte, jak fungují ovládacích prvků v Azure Active Directory podmíněného přístupu."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4cb225266a45808e5fda271e901749bf03c636e2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="controls-in-azure-active-directory-conditional-access"></a>Ovládací prvky v Azure Active Directory podmíněného přístupu 

S [podmíněného přístupu Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), jak oprávněným uživatelům přístup můžete řídit cloudových aplikací. V zásadách podmíněného přístupu definujete odpovědi ("to") na určitý stav ("v takovém případě"). V kontextu podmíněného přístupu 

- "**v takovém případě**" se nazývá **podmínky – příkaz**

- "**Udělejte to**" se nazývá **ovládací prvky**

![Ovládací prvek](./media/active-directory-conditional-access-controls/11.png)

Kombinace příkaz podmínky s ovládacími prvky představuje zásady podmíněného přístupu.

![Ovládací prvek](./media/active-directory-conditional-access-controls/12.png)

Každý ovládací prvek je buď požadavek, který musí být splněny osoba nebo systému přihlášení nebo omezení na to, co uživatel provést po přihlášení. 

Existují dva typy ovládacích prvků: 

- **Ovládací prvky grant** – Pokud chcete přístup brány

- **Ovládací prvky relace** – Pokud chcete omezit přístup v rámci relace

Toto téma vysvětluje různé ovládacích prvků, které jsou k dispozici v Azure AD podmíněného přístupu. 

## <a name="grant-controls"></a>Udělení ovládací prvky

Udělení kontrolní prvky můžete buď úplně blokovat přístup nebo povolit přístup s další požadavky výběrem požadované ovládací prvky. Pro více ovládacích prvků můžete požadovat:

- Všechny vybrané ovládací prvky, musí být splněny (*a*) 
- Vybrán jeden ovládací prvek musí být splněny (*nebo*)

![Ovládací prvek](./media/active-directory-conditional-access-controls/17.png)



### <a name="multi-factor-authentication"></a>Ověřování pomocí služby Multi-Factor Authentication

Tento ovládací prvek můžete požadovat použití vícefaktorového ověřování pro přístup k zadané cloudové aplikace. Tento ovládací prvek podporuje následující zprostředkovatelé vícefaktorového: 

- Azure Multi-Factor Authentication 

- Poskytovatele služby Multi-Factor authentication na místě v kombinaci s Active Directory Federation Services (AD FS).
 
Pomocí služby Multi-Factor authentication pomáhá chránit prostředky z přistupuje neoprávněný uživatel, který může mít získal přístup k primární pověření platného uživatele.



### <a name="compliant-device"></a>Odpovídající zařízení

Můžete nakonfigurovat zásady podmíněného přístupu, které jsou založené na zařízení. Cílem zásad podmíněného přístupu na základě zařízení je k udělení přístupu k prostředkům nakonfigurované pouze z důvěryhodných zařízení. Vyžadování vyhovující zařízení je jednou z možností je nutné definovat je důvěryhodné zařízení. Další informace najdete v tématu [nastavit zásady podmíněného přístupu na základě zařízení služby Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).

### <a name="domain-joined-device"></a>Zařízení připojených k doméně

Vyžadování, že je zařízení připojené k doméně jinou možnost, budete muset nakonfigurovat zásady podmíněného přístupu podle zařízení. Tento požadavek se odkazuje na stolních počítačů, laptopů a tablety enterprise, které jsou připojeny k místní službě Active Directory systému Windows. Další informace najdete v tématu [nastavit zásady podmíněného přístupu na základě zařízení služby Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md).





### <a name="approved-client-app"></a>Schválené klientské aplikace

Vzhledem k tomu, že vaši zaměstnanci používají mobilní zařízení i osobní a pracovní úkoly, můžete chtít mít možnost chránit firemní data při přístupu pomocí zařízení i v případě, kde nejsou spravovaná vámi.
Můžete použít [zásady ochrany aplikací Intune](https://docs.microsoft.com/intune/app-protection-policy) k ochraně dat vaší společnosti, nezávisle na řešení správy mobilních zařízení (MDM).


Schválené klientských aplikací, můžete vyžadovat klientskou aplikaci, která se pokusí o přístup k vaší cloudové aplikace pro podporu [zásady ochrany aplikací Intune](https://docs.microsoft.com/intune/app-protection-policy). Například můžete omezit přístup na Exchange Online k aplikaci Outlook. Zásady podmíněného přístupu, která vyžaduje schválené klientské aplikace je také označován jako [zásady podmíněného přístupu na základě aplikace](active-directory-conditional-access-mam.md). Seznam podporovaných schválené klientských aplikací, naleznete v části [schválení klienta aplikace požadavek](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).


### <a name="terms-of-use"></a>Podmínky použití

Uživatel může vyžadovat ve vašem klientovi o souhlas s podmínkami použití před udělením přístupu k prostředku. Jako správce můžete nakonfigurovat a upravit podmínky použití tím, že nahrajete dokument PDF. Pokud uživatel spadá do rozsahu toto řízení přístupu k aplikaci je udělit pouze tehdy, pokud byl souhlas s podmínkami použití. 


### <a name="custom-controls"></a>Vlastní ovládací prvky 

Můžete vytvořit vlastní ovládací prvky v podmíněného přístupu, který přesměruje uživatele na kompatibilní služby splňovat další požadavky mimo Azure Active Directory. To umožňuje použití určitých externí služby Multi-Factor authentication a zprostředkovatelů ověření vynutit pravidla podmíněného přístupu, nebo vytvořit vlastní služby. Prohlížeč vyhovět tento ovládací prvek, je přesměrován na externí služba, provede požadované ověřování nebo ověřování aktivity a je pak přesměrován zpět do Azure Active Directory. Pokud uživatel byl úspěšně ověřen nebo ověřit, uživatel bude pokračovat v toku podmíněného přístupu. 

## <a name="custom-controls"></a>Vlastní ovládací prvky

Vlastní ovládací prvky jsou funkce Azure Active Directory Premium P2 edice. Pokud používáte vlastní ovládací prvky, uživatelé jsou přesměrovány do kompatibilní služby splňovat další požadavky mimo Azure Active Directory. Prohlížeč vyhovět tento ovládací prvek, je přesměrován na externí služba, provede požadované ověřování nebo ověřování aktivity a je pak přesměrován zpět do Azure Active Directory. Azure Active Directory zkontroluje odpověď, a pokud uživatel byl úspěšně ověřen nebo ověřit, uživatel bude pokračovat v toku podmíněného přístupu.

Tyto ovládací prvky umožňují použití určité externí nebo vlastní služby jako řízení podmíněného přístupu a obecně rozšiřují možnosti podmíněného přístupu.

Zprostředkovatelé aktuálně nabízející kompatibilní službu patří:

- Duo zabezpečení

- RSA

- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Další informace o těchto služeb obraťte se na zprostředkovatele přímo.

### <a name="creating-custom-controls"></a>Vytváření vlastních ovládacích prvků

Pokud chcete vytvořit vlastní ovládací prvek, je měli kontaktovat zprostředkovatele, který chcete využívat. Každý poskytovatel jiných společností než Microsoft má svou vlastní proces a požadavky registrace, přihlášení k odběru nebo v opačném případě se stanou součástí služby a k označení, které chcete integrovat s podmíněným přístupem. V tomto bodě zprostředkovatele vám poskytne bloku dat ve formátu JSON. Tato data umožňuje zprostředkovatele a podmíněného přístupu, aby spolu spolupracovaly pro vašeho klienta, vytvoří nový ovládací prvek a definuje, jak podmíněný přístup zjistíte, pokud vaši uživatelé provedli úspěšně ověřit pomocí zprostředkovatele.

Zkopírujte JSON data a pak ji vložit do textového pole související. Neprovádějte změny do formátu JSON, pokud nerozumíte explicitně změny, které děláte. Provedení jakékoli změny může zrušit propojení mezi poskytovatelem a Microsoft a potenciálně zamknout vás a uživatele mimo vaši účty.

Možnost vytvořit vlastní ovládací prvek je v **spravovat** části **podmíněného přístupu** stránky.

![Ovládací prvek](./media/active-directory-conditional-access-controls/82.png)

Kliknutím na tlačítko **nové vlastní ovládací prvek**, otevře se okno s textové pole pro data JSON ovládacího prvku.  


![Ovládací prvek](./media/active-directory-conditional-access-controls/81.png)


### <a name="deleting-custom-controls"></a>Odstranění vlastní ovládací prvky

Chcete-li odstranit vlastní ovládací prvek, třeba nejprve zajistit, že se již nepoužívá v žádné zásady podmíněného přístupu. Po dokončení:

1. Přejděte do vlastní ovládací prvky seznamu

2. Klikněte na tlačítko...  

3. Vyberte **Odstranit**.

### <a name="editing-custom-controls"></a>Vlastní ovládací prvky úprav

Upravit vlastní ovládací prvek, musí odstranit aktuální ovládací prvek a vytvořte nový ovládací prvek s aktualizované informace.




## <a name="session-controls"></a>Ovládací prvky relace

Ovládací prvky relace povolit omezený prostředí v rámci cloudové aplikace. Ovládací prvky relace vynucuje cloudových aplikací a spoléhá na další informace, které poskytuje Azure AD do aplikace o relaci.

![Ovládací prvek](./media/active-directory-conditional-access-controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Pomocí omezení aplikace, které vynucují

Tento ovládací prvek můžete použít tak, aby vyžadovala Azure AD k předání informací o zařízení ke cloudové aplikaci. To pomáhá cloudové aplikaci vědět, pokud uživatel pochází ze zařízení připojených k doméně nebo kompatibilní zařízení. Tento ovládací prvek je aktuálně podporuje jenom s SharePoint jako cloudové aplikace. SharePoint používá informace o zařízení a poskytuje uživatelům úplné nebo omezené prostředí v závislosti na stavu zařízení.
Další informace o tom, jak vyžadovat omezený přístup se službou SharePoint naleznete v tématu [řízení přístupu z nespravovaných zařízení](https://aka.ms/spolimitedaccessdocs).



## <a name="next-steps"></a>Další kroky

- Pokud chcete vědět, jak konfigurovat zásadu podmíněného přístupu, najdete v článku [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Pokud jste připraveni ke konfiguraci zásad podmíněného přístupu pro prostředí, najdete v článku [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md). 
