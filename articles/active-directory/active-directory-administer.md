---
title: "aaaHow toouse přehled Azure Active Direcory directory klienta | Microsoft Docs"
description: "Vysvětluje, co je klient Azure AD a jak toomanage Azure pomocí Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro;oldportal
ms.openlocfilehash: ddb16d89bf06a3753ed5106bd95162130ad51b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-azure-ad-directory"></a>Správa adresáře služby Azure AD

## <a name="what-is-an-azure-ad-tenant"></a>Co je klient služby Azure AD?
V Azure Active Directory (Azure AD) je tenant vyhrazenou instancí služby Azure AD, kterou vaše organizace obdrží, když se přihlásí ke cloudové službě Microsoftu, jako je Azure nebo Office 365. Každý adresář služby Azure AD je oddělený od ostatních adresářů služby Azure AD. Stejně jako je podniková kancelářská budova je konkrétní tooonly zabezpečený prostředek vaší organizace, adresář služby Azure AD se také navrženou toobe o zabezpečený prostředek k použití výhradně vaší organizace. Architektura služby Azure AD Hello izoluje informace o zákazníkovi dat a identity tak, aby uživatelé a správci jednoho adresáře služby Azure AD už úmyslně nebo neúmyslně přístup k datům v jiném adresáři.

![Správa služby Azure Active Directory](./media/active-directory-administer/aad_portals.png)

## <a name="how-can-i-get-an-azure-ad-directory"></a>Jak získat adresář služby Azure AD?
Azure AD poskytuje hello základní adresáře a identity, možnosti správy většinu cloudových služeb společnosti Microsoft, včetně:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Adresář Azure AD získáte při registraci libovolné z těchto cloudových služeb Microsoftu. Podle potřeby můžete vytvářet další adresáře. Například můžete první adresář používat jako produkční a vytvořit další adresář pro testování nebo přípravu.

### <a name="using-hello-azure-ad-directory-that-comes-with-a-new-azure-subscription"></a>Pomocí hello adresář Azure AD, která se dodává s nové předplatné služby Azure

Doporučujeme použít účet správce hello, která jste použili pro první službě při registraci v jiných služeb společnosti Microsoft. Hello informace, které zadáte hello poprvé zaregistrovat pro službu Microsoft je použité toocreate nové Azure AD instance adresáře pro vaši organizaci. Pokud použijete tento adresář tooauthenticate pokusy o přihlášení při vytvoření odběru služby Microsoft tooother, můžou používat hello existující uživatelské účty, zásady, nastavení nebo místní integrace adresáře, které jste nakonfigurovali v výchozí adresář.

Například pokud jste přihlášení k odběru Microsoft Intune a další synchronizaci služby Active Directory v místě s adresářem Azure AD, můžete si zaregistrovat pro jiné služby společnosti Microsoft, např. Office 365 a snadno dosáhnout hello stejný adresář Výhody integrace, které jste s Microsoft Intune.

Další informace o integraci místního adresáře se službou Azure AD najdete v článku o [integraci adresáře s Azure AD Connect](active-directory-aadconnect.md).

### <a name="associate-an-existing-azure-ad-directory-with-a-new-azure-subscription"></a>Přidružení stávajícího adresáře služby Azure AD k novému předplatnému služby Azure
Nové předplatné služby Azure můžete přidružit hello stejný adresář, který ověřuje přihlášení pro stávající předplatné služeb Office 365 nebo Microsoft Intune. Další informace o tomto scénáři najdete v tématu [přenos vlastnictví tooanother účtu předplatného Azure](../billing/billing-subscription-transfer.md)

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Vytvoření adresáře služby Azure AD prostřednictvím registrace organizace ke cloudové službě Microsoftu
Pokud ještě nemáte předplatné tooa cloudové služby Microsoftu, můžete použít jednu z hello následující odkazy toosign. Při registraci k první službě se adresář služby Azure AD vytvoří automaticky.

* [Microsoft Azure](https://account.azure.com/organization)
* [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
* [Microsoft Intune](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0%20)

### <a name="how-toochange-hello-default-directory-for-a-subscription"></a>Jak toochange hello výchozí adresář pro předplatné

1. Přihlaste se toohello [centra účtů Azure](https://account.windowsazure.com/Home/Index) pomocí účtu, který je hello správce účtu hello předplatné tootransfer předplatné vlastnictví.
2. Ujistěte se, že hello uživatele, který budete chtít vlastník předplatného toobe hello je v adresáři hello cílem.
3. Klikněte na **Převod vlastnictví předplatného**.
4. Zadejte příjemce hello. příjemce Hello automaticky získá e-mail s odkazem přijetí.
5. příjemce Hello klikne na odkaz hello a postupuje hello pokynů, včetně zadáním jejich platebních údajů. Pokud je příjemce hello úspěšná, se přenáší hello předplatného. 
6. Hello výchozí adresář hello předplatného se změnilo toohello adresář, který hello uživatele je v, pokud je přenos vlastnictví předplatného hello úspěšné.

### <a name="manage-hello-default-directory-in-azure"></a>Spravovat hello výchozí adresář v Azure
Při registraci v Azure se k vašemu předplatnému přidruží výchozí adresář Azure AD. Za používání služby Azure AD se neplatí a vaše adresáře jsou bezplatným prostředkem. Existují placené služby Azure AD, které se licencují samostatně a poskytují další funkce, jako je třeba firemní branding při přihlášení nebo samoobslužné resetování hesla. Můžete také vytvořit vlastní doménu pomocí názvu DNS, který vlastníte místo výchozí hello *. onmicrosoft.com domény.

## <a name="how-can-i-manage-directory-data"></a>Jak spravovat data adresáře?
tooadminister jeden nebo více předplatných cloudové služby Microsoftu, můžete použít hello [centra pro správu Azure AD](https://aad.portal.azure.com), hello portálu účtu Microsoft Intune nebo hello [Centrum pro správu Office 365](https://portal.office.com/) toomanage vaší data adresáře organizace. Můžete také použít [rutiny Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory) toohelp spravovat data uložená ve službě Azure AD.

Pomocí kteréhokoli z těchto portálů (nebo rutin) můžete:

* vytvářet a spravovat účty uživatelů a skupin
* spravovat související cloudové služby pro předplatná vaší organizace
* nastavit místní integraci se službami ověřování a identit Azure AD

Centrum pro správu Azure AD Hello, Centrum pro správu Office 365, portálu účtů Microsoft Intune a hello rutiny služby Azure AD čtou a zápis tooa jediné sdílené instance služby Azure AD, který je přiřazen k adresáři vaší organizace. Každý z těchto nástrojů funguje jako front-endové rozhraní, které přijímá nebo mění data vašeho adresáře.

Pokud změníte data vaší organizace pomocí kteréhokoli hello portály nebo rutin a přihlášeni v kontextu hello jednu z těchto služeb, hello změny jsou také uvedené v hello jiné, že portálů hello při příštím přihlášení. Tato data je sdílet mezi hello Microsoft cloud services toowhich, které se přihlásíte.

Například pokud použijete hello Centrum pro správu Office 365 tooblock uživatele z přihlášení, které blokuje akce hello uživateli přihlašování tooany jiné služby toowhich vaše organizace předplacenou. Při zobrazení hello stejným uživatelským účtem hello portálu účtů Microsoft Intune, je také zobrazit tento uživatel hello je blokován.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Jak přidat a spravovat více adresářů?
Můžete [přidejte adresář služby Azure AD v hello portál Azure](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory). Vyplnění hello informací a vyberte **vytvořit**.

Každý adresář můžete spravovat jako plně nezávislý prostředek: každý adresář je rovnocenný, plně vybavený a logicky nezávislý na ostatních adresářích, které spravujete. Mezi adresáři neexistují žádné vztahy typu nadřazenost/podřazenost. Tato nezávislost mezi adresáři zahrnuje nezávislost prostředků, nezávislost správy a nezávislost synchronizace.

* **Nezávislost prostředků**. Pokud vytvoříte nebo odstranění prostředku v jednom adresáři nemá žádný vliv na prostředky v jiných adresářích. částečnou výjimku hello externích uživatelů. Pokud používáte vlastní doménu „contoso.com“ u jednoho adresáře, nemůžete ji použít u jiných adresářů.
* **Nezávislost správy**.  Pokud uživatel adresáře Contoso, který nemá oprávnění správce, vytvoří testovací adresář Test:
  
  * Správci adresáře "Contoso" Hello mít žádná přímá oprávnění správce toodirectory "Test", pokud správce "Test" explicitně neudělí těchto oprávnění. Správci "Contoso" můžou řídit přístup toodirectory "Test" základě ovládání hello uživatelského účtu, který vytvořili Test.
    
  * Pokud je přiřadit nebo odebrat roli správce pro uživatele v jednom adresáři, hello změna neovlivňuje žádnou roli správce tento uživatel může mít v jiném adresáři.
* **Nezávislost synchronizace**. Můžete nakonfigurovat každý Azure AD klienta nezávisle na nástroji tooget data synchronizovala z jednu instanci hello Azure AD Connect nástroj pro synchronizaci adresáře.

Na rozdíl od jiných prostředků služby Azure nejsou vaše adresáře podřízenými prostředky předplatného služby Azure. Proto pokud zrušíte nebo necháte tooexpire vaše předplatné Azure, můžete přístup datům vašeho adresáře prostřednictvím Azure AD PowerShell, hello Azure Graph API nebo dalších rozhraní, jako je například Centrum pro správu hello Office 365. Také můžete přidružit jiné předplatné s adresářem hello.

## <a name="how-tooprepare-toodelete-an-azure-ad-directory"></a>Jak tooprepare toodelete adresář služby Azure AD
Globální správce můžete z portálu hello odstranit adresář služby Azure AD. Při odstranění adresáře se také odstraní všechny prostředky, které jsou obsaženy v adresáři hello. Ověřte, že je není nutné hello directory před odstraněním.

> [!NOTE]
> Pokud hello uživatel přihlášený pomocí pracovního nebo školního účtu, hello nemůže se pokusit toodelete svého domovského adresáře. Například pokud hello uživatel je přihlášený jako joe@contoso.onmicrosoft.com, tento uživatel nemůže odstranit adresář hello, která má jako výchozí doménu contoso.onmicrosoft.com.

Azure AD vyžaduje, aby byly určité podmínky nesplnění toodelete adresář. Tím se snižuje riziko, že odstranění adresáře negativně ovlivní uživatele nebo aplikace, jako je například možnost hello toosign uživatelé v tooOffice 365 nebo přístup k prostředkům v Azure. Například pokud se někdo neúmyslně odstranil adresář pro předplatné, pak uživatelé nemají přístup k hello prostředků Azure pro toto předplatné.

jsou zaškrtnutá políčka Hello následující podmínky:

* Hello pouze uživatele v adresáři hello by měl být hello globální správce, který je toodelete hello adresář. Všichni ostatní uživatelé musí před odstraněním adresáře hello odstranit. Pokud uživatelé používají synchronizaci z místní, potom musí být vypnutý synchronizace a hello uživatelé třeba jej odstranit v hello cloudového adresáře pomocí hello portálu Azure nebo rutin prostředí Azure PowerShell. Neexistuje požadavek toodelete skupiny nebo kontakty, například kontakty přidané pomocí centra pro správu hello Office 365.
* Může existovat v adresáři hello žádné aplikace. Před odstraněním adresáře hello musí odstranit všechny aplikace.
* Žádní poskytovatelé služby Multi-Factor authentication může být propojený toohello adresář.
* Může být žádné předplatné Online služby Microsoft, jako je Microsoft Azure, Office 365 nebo Azure AD Premium přidružené hello adresáře. Například pokud byl váš výchozí adresář vytvořený v Azure, nemůžete ho odstranit, pokud stále slouží k ověřování přihlášení k předplatnému služby Azure. Podobně nelze odstranit adresář, pokud k němu má přidružené předplatné jiný uživatel. 


## <a name="next-steps"></a>Další kroky
* [Fórum služby Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
* [Fórum služby Azure Multi-Factor Authentication](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
* [Dotazy webu Stack Overflow pro Azure](http://stackoverflow.com/questions/tagged/azure)
* [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory)
* [Přiřazování rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md)
