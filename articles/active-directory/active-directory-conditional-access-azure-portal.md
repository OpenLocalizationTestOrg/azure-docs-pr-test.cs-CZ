---
title: "Podmíněný přístup pro Azure Active Directory | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu ve službě Azure Active Directory zkontrolujte za určitých podmínek při ověřování pro přístup k aplikacím."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 20572ecbde79bc2722f3a25f297c92d8e722a3e8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Podmíněný přístup v Azure Active Directory

První mobilní, cloudové první světě Azure Active Directory umožňuje jednotné přihlašování k zařízení, aplikacím a službám odkudkoli. S jak narůstá počet zařízení (včetně BYOD), použít vypnout podnikových sítích a aplikace SaaS 3. stran, odborníci v oblasti IT potýkají s dva dosáhnout cíle:

- Umožnit koncovým uživatelům k dosažení produktivity. kdykoli a kdekoli
- Ochrana podnikových prostředků kdykoli

Pokud chcete zvýšit produktivitu, Azure Active Directory poskytuje uživatelům s širokou škálu možností pro přístup k vaší podnikové prostředky. Azure Active Directory se správou aplikací přístup, umožňuje pouze zkontrolujte *příslušní lidé* můžete přístup k vaší aplikace. Co dělat, pokud chcete mít větší kontrolu nad jak příslušní lidé přistupují k prostředkům za určitých podmínek? Co když i máte podmínky, za kterých chcete zablokovat přístup k určitým aplikacím i pro *pravým osoby*? Například když může to být OK můžete příslušní lidé přistupují k určitým aplikacím z důvěryhodné sítě; však nemusí chcete, aby tyto aplikace přistupovat ze sítě, kterým nedůvěřujete. Tyto otázky pomocí podmíněného přístupu můžete vyřešit.

Podmíněný přístup je funkce služby Azure Active Directory, která umožňuje vynutit ovládacích prvků na přístup k aplikacím ve vašem prostředí na základě určitých podmínek. S ovládacími prvky můžete buď tie další požadavky na přístup, nebo můžete ho blokovat. Implementace podmíněného přístupu je založená na zásadách. O přístupu na základě zásad usnadňuje prostředí konfigurace, protože postupuje způsob, jakým si myslíte o požadavků na přístup.  

Obvykle můžete definovat požadavků na přístup pomocí příkazů, které jsou založeny na vzoru následující:

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/10.png)

Když nahradíte dva výskyty "*to*" reálného informace, máte příklad prohlášení o zásadách, pravděpodobně bude vypadat snadno dokážete:

*Když dodavatelů se pokoušíte získat přístup ze sítě, které nejsou důvěryhodné naší cloudové aplikace, pak Blokujte přístup.*

Prohlášení o zásadách výše označuje power podmíněného přístupu. Když povolíte dodavatelů v podstatě přístup cloudových aplikací (**kdo**), s podmíněným přístupem můžete také definovat podmínky, za kterých je možné přístup (**jak**).

V rámci Azure Active Directory podmíněný přístup,

- "**v takovém případě**" se nazývá **podmínky – příkaz**
- "**Udělejte to**" se nazývá **ovládací prvky**

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/11.png)

Kombinace příkaz podmínky s ovládacími prvky představuje zásady podmíněného přístupu.

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Ovládací prvky

V zásadách podmíněného přístupu ovládací prvky definovat, co je, že, musí dojít v případě, že příkaz podmínky má byly splněny.  
S ovládacími prvky můžete blokovat přístup nebo povolit přístup s další požadavky.
Pokud budete konfigurovat zásadu, která umožňuje přístup, je nutné vybrat alespoň jeden požadavek.   

### <a name="grant-controls"></a>Udělení ovládací prvky
Aktuální implementace služby Azure Active Directory umožňuje nakonfigurovat následující požadavky řízení grant:

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/05.png)

- **Služba Multi-Factor Authentication** -může vyžadovat silné ověřování pomocí služby Multi-Factor authentication. Jako zprostředkovatel můžete použít Azure Multi-Factor nebo poskytovatele služby Multi-Factor authentication na místě v kombinaci s Active Directory Federation Services (AD FS). Pomocí služby Multi-Factor authentication pomáhá chránit prostředky z přistupuje neoprávněný uživatel, který může mít získal přístup k přihlašovacím údajům platného uživatele.

- **Zařízení kompatibilní s** – můžete nastavit zásady podmíněného přístupu na úrovni zařízení. Může nastavit zásady Povolit pouze počítače, které jsou kompatibilní, nebo mobilní zařízení, která jsou zaregistrovaná pomocí správy mobilních zařízení pro přístup k prostředkům vaší organizace. Můžete například Intune používat ke kontrole dodržování předpisů pro zařízení a pak zprávy do služby Azure AD pro vynucení když se uživatel pokusí o přístup k aplikaci. Podrobné pokyny o tom, jak Intune používat k ochraně dat a aplikací najdete v tématu [ochranu dat a aplikací pomocí Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune). Intune taky můžete použít k vynucení ochrana dat pro ztracené nebo odcizené zařízení. Další informace najdete v tématu [Chraňte svá data s využitím plného nebo selektivního vymazání pomocí Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

- **Zařízení připojených k doméně** – můžete vyžadovat, zařízení, které jste použili pro připojení k Azure Active Directory k doméně, do místní služby Active Directory (AD). Tato zásada platí pro stolní počítače, přenosné počítače a tablety enterprise systému Windows. 

Pokud máte více ovládacích prvků vybraná, můžete také nakonfigurovat, jestli všechny z nich jsou povinné, při zpracování vaší zásady.

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/06.png)

### <a name="session-controls"></a>Ovládací prvky relace
Ovládací prvky relace povolit omezení prostředí v rámci cloudové aplikace. Ovládací prvky relace vynucuje cloudových aplikací a spoléhá na další informace, které poskytuje Azure AD do aplikace o relaci.

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/session-control-pic.png)

#### <a name="use-app-enforced-restrictions"></a>Pomocí omezení aplikace, které vynucují
Tento ovládací prvek můžete použít tak, aby vyžadovala Azure AD k předání informací o zařízení ke cloudové aplikaci. To pomáhá cloudové aplikaci vědět, pokud uživatel pochází ze zařízení připojených k doméně nebo kompatibilní zařízení. Tento ovládací prvek je aktuálně podporuje jenom s SharePoint jako cloudové aplikace. SharePoint používá informace o zařízení a poskytuje uživatelům úplné nebo omezené prostředí v závislosti na stavu zařízení.
Další informace o tom, jak vyžadovat omezený přístup se službou SharePoint, přejděte [zde](https://aka.ms/spolimitedaccessdocs).

## <a name="condition-statement"></a>Příkaz podmínky

V předchozí části obsahuje zavedla podporované možnosti blokovat nebo omezení přístupu k prostředkům v podobě ovládacích prvků. V zásadách podmíněného přístupu zadejte kritéria, která musí být splněny pro vaše ovládací prvky, které se má použít v podobě příkaz podmínky.  

Do vaší příkaz podmínky může zahrnovat následující přiřazení:

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/07.png)


- **Kdo** – v mnoha případech chcete, aby vaše ovládací prvky má být použita pro konkrétní skupinu uživatelů. V příkazu podmínku můžete definovat této sady výběrem uživatelé a skupiny, které vaše zásada se vztahuje na. V případě potřeby můžete také výslovně vyloučit sadu uživatelů ze zásady výjimky je.  
Pokud vyberete uživatelé a skupiny, definujte rozsah uživatele, které vaše zásada se vztahuje na.    

    ![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/08.png)



- **Co** – obvykle existují určité aplikace, které běží ve vašem prostředí z hlediska ochrany vyžaduje další pozornost než jiné. Tímto je ovlivněn, například aplikace, které mají přístup k citlivým datům.
Pokud vyberete cloudové aplikace, definujte rozsah cloudové aplikace, které vaše zásada se vztahuje na. V případě potřeby můžete také výslovně vyloučili sadu aplikací z vaší zásady.

    ![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/09.png)


- **Jak** – tak dlouho, dokud se provádí přístup k aplikacím v podmínkách můžete řídit, že může pro nastavení na další ovládací prvky, nemusí být jak cloudové aplikace přistupují uživatelé. Věcí však může vypadat jinak, pokud se provádí přístup k vaší cloudové aplikace, například v ze sítě, které nejsou důvěryhodné nebo zařízení, která nejsou kompatibilní. V příkazu podmínku můžete definovat určité podmínky přístupu, které mají další požadavky na tom, jak se provádí přístup k aplikacím.

    ![Podmínky](./media/active-directory-conditional-access-azure-portal/21.png)


## <a name="conditions"></a>Podmínky

V aktuální implementace služby Azure Active Directory můžete definovat podmínky v následujících oblastech:

- Riziko přihlášení
- Platformy zařízení
- Umístění
- Klientské aplikace

![Podmínky](./media/active-directory-conditional-access-azure-portal/21.png)

### <a name="sign-in-risk"></a>Riziko přihlášení

Riziko přihlášení je objekt, který se používá služba Azure Active Directory pro trasování pravděpodobnost, že u přihlášení pokus nebyla provedena legitimní vlastníkem uživatelského účtu. V tomto objektu pravděpodobnost (vysoká, střední nebo nízká) je uložen v podobě atributu s názvem [úroveň rizika přihlašovací](active-directory-reporting-risk-events.md#risk-level). Tento objekt je generován během přihlášení uživatele, pokud byly zjištěny rizika přihlášení pomocí služby Azure Active Directory. Další podrobnosti najdete v tématu [Riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins).  
Úroveň rizika počítané přihlášení můžete použít jako podmínku v zásadách podmíněného přístupu. 

![Podmínky](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Platformy zařízení

Operační systém, který běží na vašem zařízení je charakterizovaná platformu zařízení:

- Android
- iOS
- telefon se systémem Windows
- Windows
- systému macOS (preview). 

![Podmínky](./media/active-directory-conditional-access-azure-portal/02.png)

Můžete definovat platformy zařízení, které jsou zahrnuty i platformy zařízení, které jsou vyloučené ze zásad.  
Pokud chcete použít v zásadách platformy zařízení, nejprve změňte konfigurace přepínačů **Ano**a vyberte všechny nebo platformy jednotlivých zařízení se zásady vztahují. Pokud vyberete platformy jednotlivých zařízení, zásady má vliv pouze na těchto platformách. V tomto případě nejsou zásady dopad na přihlášení na jiných podporovaných platforem.


### <a name="locations"></a>Umístění

Umístění je identifikována IP adresu klienta, které jste použili pro připojení k Azure Active Directory. Tato podmínka vyžaduje, abyste se seznamte s **s názvem umístění** a **MFA důvěryhodné IP adresy**.  

**S názvem umístění** je funkce služby Azure Active Directory, která umožňuje označovat důvěryhodné rozsahy IP adres ve vaší organizace. Ve vašem prostředí, můžete použít s názvem umístění v kontextu detekce [rizik události](active-directory-reporting-risk-events.md) a také podmíněný přístup. Další podrobnosti týkající se konfigurace s názvem umístění v Azure Active Directory najdete v tématu [s názvem umístění v Azure Active Directory](active-directory-named-locations.md).

Počet umístění, které můžete konfigurovat, je omezené velikost je související objekt ve službě Azure AD. Můžete nakonfigurovat:
 
 - Jedno umístění s názvem s maximálně 500 rozsahy IP adres
 - Nesmí být delší než 60 s názvem umístění (preview) s jeden rozsah IP adres přiřazené ke každému z nich 


**MFA důvěryhodné IP adresy** je funkce služby Multi-Factor authentication, která umožňuje definovat důvěryhodné rozsahy IP adres reprezentující místní intranet vaší organizace. Když nakonfigurujete podmínky umístění, důvěryhodných IP adres umožňuje rozlišovat připojení z vaší podnikové síti a všech jiných umístění. Další podrobnosti najdete v tématu [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  



Můžete buď zahrnout všechny umístění nebo všechny důvěryhodné IP adresy a můžete vyloučit všechny důvěryhodné IP adresy.

![Podmínky](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-app"></a>Klientské aplikace

Klientská aplikace může být buď na obecné úrovni aplikace (webový prohlížeč, mobilní aplikace, klient plocha), jste použili pro připojení k Azure Active Directory nebo můžete konkrétně vybrat protokolu Exchange Active Sync.  
Starší verze ověřování odkazuje na klienty, kteří používají základní ověřování, jako je starší klienty Office, které nepoužívají moderní ověřování. Pomocí starší verze ověřování není aktuálně podporuje podmíněný přístup.

![Podmínky](./media/active-directory-conditional-access-azure-portal/04.png)


## <a name="common-scenarios"></a>Obvyklé scénáře

### <a name="requiring-multi-factor-authentication-for-apps"></a>Vyžadování vícefaktorového ověřování pro aplikace

Mnoho prostředí mít aplikace, které vyžadují vyšší úroveň ochrany než jiné.
To platí, například pro aplikace, které mají přístup k citlivým datům.
Pokud chcete přidat další vrstvu ochrany do těchto aplikací, můžete nakonfigurovat zásady podmíněného přístupu, který vyžaduje službu Multi-Factor authentication, když uživatelé přistupují těchto aplikací.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Vyžadování vícefaktorového ověřování pro přístup ze sítě, které nejsou důvěryhodné

Tento scénář je podobný předchozímu scénáři, protože ho přidá požadavek pro službu Multi-Factor authentication.
Hlavní rozdíl je však podmínky pro tento požadavek.  
Během fokus předchozím scénáři je pro aplikace s přístupem k datům sensitve, je aktivní tohoto scénáře důvěryhodného umístění.  
Jinými slovy může mít požadavek pro službu Multi-Factor authentication, pokud uživatel ze sítě, kterým nedůvěřujete přístupu k aplikaci.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Jenom důvěryhodné zařízení mají přístup ke službám Office 365

Pokud ve svém prostředí používáte Intune, můžete okamžitě začít používat rozhraní zásad podmíněného přístupu v konzole Azure.

Mnoho zákazníků Intune použití podmíněného přístupu k zajištění, že pouze důvěryhodné zařízení mají přístup ke službám Office 365. To znamená, že mobilní zařízení jsou zaregistrovaná v Intune a splňovat požadavky zásad dodržování předpisů, a že jsou počítače s Windows připojený k doméně místní. Klíče zlepšování je, že není nutné nastavit stejné zásady pro každou služeb Office 365.  Když vytvoříte novou zásadu, nakonfigurujte cloudových aplikací na každý ze O365 aplikace, které chcete chránit pomocí s podmíněným přístupem.

## <a name="next-steps"></a>Další kroky

Pokud chcete vědět, jak konfigurovat zásadu podmíněného přístupu, najdete v článku [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Pokud jste připraveni ke konfiguraci zásad podmíněného přístupu pro prostředí, najdete v článku [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md). 