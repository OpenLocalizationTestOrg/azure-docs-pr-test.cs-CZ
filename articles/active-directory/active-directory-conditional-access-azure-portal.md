---
title: "aaaAzure podmíněného přístupu služby Active Directory | Microsoft Docs"
description: "Použití podmíněného řízení přístupu v Azure Active Directory toocheck za určitých podmínek při ověřování pro přístup k tooapplications."
services: active-directory
keywords: "podmíněný přístup tooapps, podmíněného přístupu s Azure AD, zabezpečení přístupu k prostředkům toocompany, zásady podmíněného přístupu"
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
ms.openlocfilehash: 9fa8a5c3e514c032fbe3aa56f33d759485a018c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Podmíněný přístup v Azure Active Directory

První mobilní, cloudové první světě Azure Active Directory umožňuje toodevices přihlašování, aplikacím a službám odkudkoli. S hello, jak narůstá počet zařízení (včetně BYOD), použít vypnout podnikových sítích a aplikace SaaS 3. stran, odborníci v oblasti IT potýkají s dva dosáhnout cíle:

- Posílení produktivity toobe koncoví uživatelé hello kdykoli a kdekoli
- Ochrana podnikových prostředků hello kdykoli

tooimprove produktivitu, Azure Active Directory poskytuje uživatelům s širokou škálu možností tooaccess vaše podnikové prostředky. Se správou aplikací přístup, Azure Active Directory umožňuje pouze tooensure *hello příslušní lidé* můžete přístup k vaší aplikace. Co dělat, když chcete toohave větší kontrolu nad jak příslušní lidé hello přistupují k prostředkům za určitých podmínek? Co když i máte podmínek, za kterých tooblock přístup toocertain aplikace i pro hello *pravým osoby*? Například když může to být OK můžete hello příslušní lidé přistupují k určitým aplikacím z důvěryhodné sítě; ale nechcete jim tooaccess tyto aplikace ze sítě, kterým nedůvěřujete. Tyto otázky pomocí podmíněného přístupu můžete vyřešit.

Podmíněný přístup je funkce služby Azure Active Directory a umožní vám tooenforce ovládacích prvků na hello přístup tooapps ve vašem prostředí na základě určitých podmínek. Ovládací prvky můžete buď tie další požadavky toohello přístupu nebo můžete ho blokovat. Hello implementace podmíněného přístupu je založená na zásadách. O přístupu na základě zásad usnadňuje prostředí konfigurace, protože postupuje hello způsobem, který si myslíte o požadavků na přístup.  

Obvykle je definovat pomocí příkazů, které jsou založeny na hello následující vzor požadavků na přístup:

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/10.png)

Když nahradíte dva výskyty k hello "*to*" reálného informace, máte příklad prohlášení o zásadách, pravděpodobně bude vypadat známé tooyou:

*Když dodavatelů se snažíte tooaccess naší cloudové aplikace ze sítě, které nejsou důvěryhodné, pak Blokujte přístup.*

prohlášení o zásadách Hello výše označuje hello power podmíněného přístupu. Zatímco můžete povolit dodavatelů toobasically přístup cloudových aplikací (**kdo**), s podmíněným přístupem můžete také definovat podmínky v rámci které hello přístup je možný (**jak**).

V kontextu hello podmíněného přístupu Azure Active Directory,

- "**v takovém případě**" se nazývá **podmínky – příkaz**
- "**Udělejte to**" se nazývá **ovládací prvky**

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/11.png)

kombinace Hello příkaz podmínky s ovládacími prvky představuje zásady podmíněného přístupu.

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Ovládací prvky

V zásadách podmíněného přístupu ovládací prvky definovat, co je, že, musí dojít v případě, že příkaz podmínky má byly splněny.  
S ovládacími prvky můžete blokovat přístup nebo povolit přístup s další požadavky.
Pokud budete konfigurovat zásadu, která umožňuje přístup, je nutné tooselect alespoň jeden požadavek.   

### <a name="grant-controls"></a>Udělení ovládací prvky
Hello aktuální implementace služby Azure Active Directory umožňuje tooconfigure hello požadavky řízení udělit následující:

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/05.png)

- **Služba Multi-Factor Authentication** -může vyžadovat silné ověřování pomocí služby Multi-Factor authentication. Jako zprostředkovatel můžete použít Azure Multi-Factor nebo poskytovatele služby Multi-Factor authentication na místě v kombinaci s Active Directory Federation Services (AD FS). Pomocí služby Multi-Factor authentication pomáhá chránit prostředky z přistupuje neoprávněný uživatel, který může mít získal přístup toohello přihlašovací údaje platného uživatele.

- **Zařízení kompatibilní s** – můžete nastavit zásady podmíněného přístupu na úrovni zařízení hello. Můžete nastavit počítače povolit tooonly zásady, které jsou kompatibilní, nebo mobilní zařízení, která jsou zaregistrovaná v tooaccess správu mobilních zařízení prostředkům vaší organizace. Můžete například použít dodržování předpisů Intune toocheck zařízení a potom nahlásit ho tooAzure AD pro vynucení při pokusu uživatele hello tooaccess aplikace. Podrobné informace o tom, jak toouse Intune tooprotect aplikace a data, najdete v části [ochranu dat a aplikací pomocí Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune). Ochrana dat tooenforce Intune můžete použít také pro ztracené nebo odcizené zařízení. Další informace najdete v tématu [Chraňte svá data s využitím plného nebo selektivního vymazání pomocí Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

- **Zařízení připojených k doméně** – může vyžadovat hello zařízení jste už použili tooconnect tooAzure služby Active Directory toobe připojený k doméně tooyour místní služby Active Directory (AD). Tato zásada platí tooWindows stolních počítačů, laptopů a tablety enterprise. 

Pokud máte více ovládacích prvků vybraná, můžete také nakonfigurovat, jestli všechny z nich jsou povinné, při zpracování vaší zásady.

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/06.png)

### <a name="session-controls"></a>Ovládací prvky relace
Ovládací prvky relace povolit omezení prostředí v rámci cloudové aplikace. ovládací prvky relace Hello vynucuje cloudových aplikací a spoléhají na další informace, které poskytuje aplikaci Azure AD toohello o relaci hello.

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/session-control-pic.png)

#### <a name="use-app-enforced-restrictions"></a>Pomocí omezení aplikace, které vynucují
Můžete použít tento ovládací prvek toorequire Azure AD toopass hello zařízení informace toohello cloudové aplikace. To pomáhá cloudové aplikace hello vědět, pokud uživatel hello pochází ze zařízení připojených k doméně nebo kompatibilní zařízení. Tento ovládací prvek je aktuálně podporuje jenom s SharePoint jako hello cloudové aplikace. SharePoint používá hello zařízení informace tooprovide uživatelé úplné nebo omezené možnosti v závislosti na stavu zařízení hello.
toolearn Další informace o tom, jak toorequire omezený přístup se službou SharePoint, přejděte [zde](https://aka.ms/spolimitedaccessdocs).

## <a name="condition-statement"></a>Příkaz podmínky

předchozí části Hello obsahuje zavedla toosupported možnosti tooblock nebo omezit přístup k prostředkům tooyour ve formuláři ovládací prvky. V zásadách podmíněného přístupu definujete hello kritéria, které je třeba splnit toobe pro vaše ovládací prvky toobe použije ve formuláři příkaz podmínky.  

Hello následující přiřazení do vaší příkaz podmínky může zahrnovat:

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/07.png)


- **Kdo** – v mnoha případech chcete, aby vaše ovládací prvky toobe použít tooa konkrétní skupinu uživatelů. V příkazu podmínku můžete definovat této sady výběrem hello uživatelé a skupiny, které vaše zásada se vztahuje na. V případě potřeby můžete také výslovně vyloučit sadu uživatelů ze zásady výjimky je.  
Pokud vyberete uživatelé a skupiny, definujete hello oboru uživatele, které vaše zásada se vztahuje na.    

    ![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/08.png)



- **Co** – obvykle existují určité aplikace, které běží ve vašem prostředí z hlediska ochrany vyžaduje další pozornost než jiné. Tímto je ovlivněn, například aplikace, které mají přístup k datům toosensitive.
Výběrem cloudových aplikací definovat rozsah hello cloudové aplikace, které zásady platí pro. V případě potřeby můžete také výslovně vyloučili sadu aplikací z vaší zásady.

    ![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/09.png)


- **Jak** – tak dlouho, dokud přístup tooyour aplikací se provádí v podmínkách můžete řídit, že může pro nastavení na další ovládací prvky, nemusí být jak cloudové aplikace přistupují uživatelé. Věcí však může vypadat jinak, pokud se provádí přístup tooyour cloudové aplikace, například v ze sítě, které nejsou důvěryhodné nebo zařízení, která nejsou kompatibilní. V příkazu podmínku můžete definovat určité podmínky přístupu, které mají další požadavky na tom, jak se provádí přístup tooyour aplikace.

    ![Podmínky](./media/active-directory-conditional-access-azure-portal/21.png)


## <a name="conditions"></a>Podmínky

V aktuální implementace hello služby Azure Active Directory můžete definovat podmínky pro hello následující oblasti:

- Riziko přihlášení
- Platformy zařízení
- Umístění
- Klientské aplikace

![Podmínky](./media/active-directory-conditional-access-azure-portal/21.png)

### <a name="sign-in-risk"></a>Riziko přihlášení

Riziko přihlášení je objekt, který se používá Azure Active Directory tootrack hello pravděpodobnost, že pokus o přihlášení nebyla provedena legitimní vlastníkem hello uživatelského účtu. V tomto objektu pravděpodobnost hello (vysoká, střední nebo nízká) je uložen v podobě atributu s názvem [úroveň rizika přihlašovací](active-directory-reporting-risk-events.md#risk-level). Tento objekt je generován během přihlášení uživatele, pokud byly zjištěny rizika přihlášení pomocí služby Azure Active Directory. Další podrobnosti najdete v tématu [Riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins).  
Úroveň rizika přihlašovací hello vypočítat můžete použít jako podmínku v zásadách podmíněného přístupu. 

![Podmínky](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Platformy zařízení

Platforma Hello je charakterizovaná hello operační systém, který běží na vašem zařízení:

- Android
- iOS
- telefon se systémem Windows
- Windows
- systému macOS (preview). 

![Podmínky](./media/active-directory-conditional-access-azure-portal/02.png)

Můžete definovat hello platformy zařízení, které jsou zahrnuty i platformy zařízení, které jsou vyloučené ze zásad.  
toouse v zásadách hello platformy zařízení, první hello změn konfigurace přepínačů příliš**Ano**a pak vyberte všechny nebo jednotlivých zařízení platformy hello zásada se vztahuje na. Pokud vyberete zařízení pro jednotlivé platformy, hello zásada má vliv pouze na těchto platformách. V tomto případě nejsou zásadami hello dopad na přihlášení tooother podporované platformy.


### <a name="locations"></a>Umístění

Hello umístění je identifikován pomocí hello IP adresy klienta hello jste použili tooconnect tooAzure služby Active Directory. Tato podmínka vyžaduje, abyste toobe obeznámeni s **s názvem umístění** a **MFA důvěryhodné IP adresy**.  

**S názvem umístění** je funkce služby Azure Active Directory, která vám umožní toolabel důvěryhodné rozsahy IP adres ve vaší organizace. Ve vašem prostředí, můžete použít s názvem umístění v kontextu hello hello zjišťování [rizik události](active-directory-reporting-risk-events.md) a také podmíněný přístup. Další podrobnosti týkající se konfigurace s názvem umístění v Azure Active Directory najdete v tématu [s názvem umístění v Azure Active Directory](active-directory-named-locations.md).

Hello počet umístění, které můžete konfigurovat, je omezené hello velikost hello související objekt ve službě Azure AD. Můžete nakonfigurovat:
 
 - Jedno umístění s názvem s až too500 rozsahy IP adres
 - Nesmí být delší než 60 s názvem umístění (preview) s jeden rozsah IP adres přiřazené tooeach z nich 


**MFA důvěryhodné IP adresy** je funkce služby Multi-Factor authentication, která vám umožní rozsahy IP adres toodefine důvěryhodné reprezentující místní intranet vaší organizace. Když nakonfigurujete podmínky umístění, umožní vám důvěryhodné IP adresy toodistinguish mezi připojení z vaší podnikové síti a všech jiných umístění. Další podrobnosti najdete v tématu [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  



Můžete buď zahrnout všechny umístění nebo všechny důvěryhodné IP adresy a můžete vyloučit všechny důvěryhodné IP adresy.

![Podmínky](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-app"></a>Klientské aplikace

Hello klientská aplikace může být buď na obecné úrovni hello aplikace (webového prohlížeče, mobilní aplikace, klient plocha) použili jste tooconnect tooAzure služby Active Directory nebo můžete konkrétně vybrat protokolu Exchange Active Sync.  
Starší verze ověřování odkazuje tooclients pomocí základní ověřování, jako je starší klienty Office, které nepoužívají moderní ověřování. Pomocí starší verze ověřování není aktuálně podporuje podmíněný přístup.

![Podmínky](./media/active-directory-conditional-access-azure-portal/04.png)


## <a name="common-scenarios"></a>Obvyklé scénáře

### <a name="requiring-multi-factor-authentication-for-apps"></a>Vyžadování vícefaktorového ověřování pro aplikace

Mnoho prostředí mít aplikace, které vyžadují vyšší úroveň ochrany než hello, ostatní.
To je třeba hello případ aplikací, které mají přístup k datům toosensitive.
Pokud chcete tooadd další vrstvu ochrany toothese aplikací, můžete nakonfigurovat zásady podmíněného přístupu, který vyžaduje službu Multi-Factor authentication, když uživatelé přistupují těchto aplikací.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Vyžadování vícefaktorového ověřování pro přístup ze sítě, které nejsou důvěryhodné

Tento scénář je podobný předchozímu scénáři toohello, protože přidá požadavek pro službu Multi-Factor authentication.
Hlavní rozdíl hello je však hello podmínky pro tento požadavek.  
Během hello fokus hello předchozí situaci je pro aplikace s daty toosensitve přístup, je aktivní hello tohoto scénáře důvěryhodného umístění.  
Jinými slovy může mít požadavek pro službu Multi-Factor authentication, pokud uživatel ze sítě, kterým nedůvěřujete přístupu k aplikaci.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Jenom důvěryhodné zařízení mají přístup ke službám Office 365

Pokud ve svém prostředí používáte Intune, můžete okamžitě začít používat rozhraní zásad podmíněného přístupu hello v hello konzoly Azure.

Mnoho zákazníků Intune používáte tooensure podmíněného přístupu, jenom důvěryhodné zařízení mají přístup ke službám Office 365. To znamená, že mobilní zařízení jsou zaregistrovaná v Intune a splňovat požadavky zásad dodržování předpisů, a že jsou počítače s Windows tooan připojené k místní doméně. Klíče zlepšování je, že nemáte tooset hello stejné zásady pro jednotlivé služby hello Office 365.  Když vytvoříte novou zásadu, nakonfigurujte hello cloudové aplikace tooinclude každý hello O365 aplikací, které chcete tooprotect s pomocí podmíněného přístupu.

## <a name="next-steps"></a>Další kroky

Pokud chcete, aby tooknow jak tooconfigure zásady podmíněného přístupu, najdete v části [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Pokud jste připravené tooconfigure zásady podmíněného přístupu pro vaše prostředí, najdete v části hello [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md). 
