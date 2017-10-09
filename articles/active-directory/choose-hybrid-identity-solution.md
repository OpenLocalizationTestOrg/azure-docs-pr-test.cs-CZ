---
title: "aaaChoose Azure hybridní řešení identit | Microsoft Docs"
description: "Získejte základní povědomí hello k dispozici hybridních řešení identit a doporučení pro jste toomake hello nejlepší identity zásad správného řízení rozhodnutí pro vaši organizaci."
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/5/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 4ab8aff314f6308ab32f77e81cf0f07e1f5d7b41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-hybrid-identity-solutions"></a>Microsoft hybridních řešení identit
[Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) hybridních řešení identit umožňují toosynchronize místní adresář objekty s Azure AD při nadále spravuje uživatele místní. Hello toomake první rozhodnutí při plánování toosynchronize vaše místní Windows Server Active Directory s Azure AD se, zda chcete toouse identity synchronizovaných nebo federovaných identit. Synchronizované identity a volitelně hodnot hash hesel, mohou uživatelé toouse hello stejné heslo tooaccess místní i cloudové prostředkům organizace. Pro pokročilejší scénáře požadavky, například jednotného přihlašování (SSO) nebo místní vícefaktorové ověřování budete potřebovat identity toofederate toodeploy Active Directory Federation Services (AD FS). 

Nejsou k dispozici pro konfiguraci hybridní identita několik možností. Tento článek obsahuje toohelp informace, které zvolíte hello nejlepší jeden pro vaši organizaci podle snadné nasazení a konkrétní správu identit a přístupu musí. Jak byste zvážit, které modelu identity nejlepší vyhovuje potřebám vaší organizace, musíte také toothink o čas, stávající infrastruktury, složitost a náklady. Tyto faktory se liší pro každou organizaci a může časem změnit. Ale pokud vaše požadavky se změní, máte také hello flexibilitu tooswitch tooa jinou identitu modelu.

> [!TIP]
> Tato řešení jsou doručovány prostřednictvím [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="synchronized-identity"></a>Synchronizované identity 
Synchronizované identity je nejjednodušší způsob, jak toosynchronize hello místní objektů adresáře (uživatelé a skupiny) s Azure AD. 

![Synchronizované hybridní identita](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Synchronizované identity je nejjednodušší a nejrychlejší metodu hello, uživatelé stále potřebovat toomaintain samostatné hesla pro cloudové prostředky. tooavoid, můžete také (volitelně) [synchronizovat hodnotu hash hesla uživatele](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) directory tooyour Azure AD. Synchronizace hesla hodnoty hash umožňuje uživatelům toolog v organizační prostředků založených na toocloud s hello stejné uživatelské jméno a heslo, používají místně. Azure AD Connect pravidelně zjišťuje, zda vaše místní adresář pro změny a udržuje adresáře Azure AD synchronizovány. Atribut uživatele nebo heslo je změněné na místním Active Directory, je automaticky aktualizovat ve službě Azure AD. 

Pro většinu organizací, kteří potřebují jenom tooenable jejich uživatelé toosign tooOffice 365, aplikace SaaS a jiné na základě AD prostředky Azure se doporučuje hello výchozí heslo synchronizace možnost. Pokud to nefunguje pro vás, budete potřebovat toodecide mezi předávací ověřování a služby AD FS.

> [!TIP]
> Uživatelská hesla jsou uložené ve službě Active Directory serveru místní Windows hello tvar hodnotu hash, který představuje hello skutečného uživatelského hesla. Hodnota hash je výsledek jednosměrné matematické funkce (hello algoritmus hash). Neexistuje žádný metoda toorevert hello výsledek jednosměrné funkce toohello prostý text verze heslo. V místní síti tooyour nelze použít toosign hash a heslo. Při přihlásíte toosynchronize hesla, hodnot hash hesel Azure AD Connect extrahuje z hello místní služby Active Directory a další bezpečnostní zpracování toohello hodnoty hash hesla, než bude synchronizované tooAzure AD. Synchronizace hesel můžete také použít společně s heslo zpětný zápis tooenable samoobslužného resetování hesla ve službě Azure AD. Kromě toho můžete povolit jednotné přihlašování (SSO) pro uživatele v doméně počítače, které jsou připojené toohello podnikové síti. Pomocí jednotného přihlašování povolené uživatele stačí tooenter cloudu uživatelské jméno toosecurely přístup k prostředkům. 

## <a name="pass-through-authentication"></a>Předávací ověřování
[Azure AD předávací ověřování](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) poskytuje jednoduché heslo řešení ověřování pro Azure služeb na základě AD pomocí služby Active Directory v místě. Pokud zásady zabezpečení a dodržování předpisů pro vaši organizaci nepovoluje odeslání hesla uživatelů i ve formuláři hash a toosupport plochy jednotného přihlašování je třeba pouze pro zařízení připojená k doméně, doporučujeme vyhodnotit pomocí předávací ověřování. Předávací ověřování nevyžaduje žádné nasazení v hello DMZ, což zjednodušuje nasazení infrastruktury hello ve srovnání se službou AD FS. Když se uživatelé přihlašují pomocí služby Azure AD, ověří tato metoda ověřování hesla uživatelů přímo pro vaše místní službu Active Directory.

![Předávací ověřování](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

S předávací ověřování není nutné pro komplexní síťovou infrastrukturu a není nutné toostore místních hesel v cloudu hello. V kombinaci s jednotné přihlašování, předávací ověřování poskytuje skutečně integrované možnosti při přihlášení tooAzure AD nebo jiných cloudových služeb.

S Azure AD Connect, který používá jednoduchý místní agent, který naslouchá požadavkům ověření hesla je nakonfigurované předávací ověřování. Hello agent může být snadno nasazené toomultiple počítače tooprovide vysokou dostupnost a vyrovnávání zatížení. Vzhledem k tomu, že veškerá komunikace jsou pouze odchozí, není nutná žádná toobe hello konektor nainstalován v hraniční síti. Hello požadavky na počítač serveru pro konektor hello jsou následující:

- Windows Server 2012 R2 nebo vyšší
- Připojené k tooa domény v doménové struktuře hello, pomocí kterého se uživatelé ověřují

> [!NOTE]
> Předávací ověřování Azure AD je aktuálně ve verzi preview a je podporováno pro klienty webového prohlížeče na základě a klienti Office, které podporují moderní ověřování. Pro klienty, kteří nejsou podporovány jako je například starší verze klientů Office a Exchange ActiveSync (včetně nativní e-mailové klienty na mobilních zařízeních), je doporučené toouse hello moderní ověřování ekvivalentní. Moderní ověřování pouze umožňuje předávací ověřování, ale také umožňuje toobe zásady podmíněného přístupu použije, například služby Multi-Factor authentication. 

Předávací ověřování není aktuálně podporován při použití zařízení s Windows 10 připojené k tooAzure AD. Ale synchronizaci hodnoty hash hesla můžete použít jako automatické záložní toosupport Windows 10 a starších verzí klientů hello již bylo zmíněno dříve. Při hello preview synchronizaci hodnoty hash hesla je povolena ve výchozím nastavení při předávací ověřování je vybrán jako hello přihlášení možnost v Azure AD Connect.


## <a name="federated-identity-ad-fs"></a>Federované identity (AD FS)
Pro větší kontrolu nad přístup uživatelů k Office 365 a jiných cloudových služeb, můžete nastavit synchronizace adresáře se jednotné přihlašování (SSO) pomocí [Active Directory Federation Services (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server-2016). Přihlášení uživatele pomocí služby AD FS delegáti ověřování tooan federaci místní server, která ověřuje přihlašovací údaje uživatele. V tomto modelu jsou pověření služby Active Directory v místě nikdy předán tooAzure AD.

![Federované identity](./media/choose-hybrid-identity-solution/federated-identity.png)

Používá se také označení federování identity, tato metoda přihlašování zajišťuje, že všechny ověření uživatele je řízené místní a umožňuje správci tooimplement přísnějším úrovně řízení přístupu. Federaci identit se službou AD FS je možnost hello nejvíce komplikovanější a vyžaduje nasazení dalších serverů v místním prostředí. Federaci identit také potvrdí tooproviding 24 x 7 podporu pro infrastrukturu služby Active Directory a AD FS. Tato vysokou úroveň podpory je nutné, protože přistupují na váš místní Internet, řadič domény nebo servery služby AD FS nejsou k dispozici, nemůžete se přihlásit uživatele v toocloud služby.

> [!TIP]
> Pokud se rozhodnete toouse federační službou Active Directory Federation Services (AD FS), můžete volitelně nastavíte synchronizace hesel jako záložní případ selhání infrastruktury služby AD FS.


## <a name="common-scenarios-and-recommendations"></a>Běžné scénáře a doporučení
Tady jsou některé běžné hybridní identita a může být vhodné pro jednotlivé scénáře správy přístupu s doporučeními jako toowhich hybridní identity možnosti (nebo možnosti).

|Potřebuji:|Server PWS a jednotné přihlašování<sup>1</sup>| PTA a jednotné přihlašování<sup>2</sup> | SLUŽBA AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Nového uživatele, obraťte se na a skupinové účty, které jsou vytvořeny v mé místní služby Active Directory toohello cloudu automaticky synchronizovat.|![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)| ![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png) |![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|
|Nastavení klienta pro Office 365 hybridní scénáře|![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)| ![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png) |![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|
|Povolit toosign mé uživatelé v a přístup pomocí hesla pro místní cloudové služby|![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)| ![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png) |![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|
|Implementace jednotné přihlašování pomocí podnikové přihlašovací údaje|![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)| ![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png) |![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|
|Ujistěte se, že žádné hodnoty hash hesla se ukládají v cloudu hello| |![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|
|Povolit místní služby Multi-Factor authentication řešení| | |![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|
|Podpora ověřování pomocí čipové karty pro svoje uživatele<sup>4</sup>| | |![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|
|Zobrazovat oznámení o vypršení platnosti hesla v hello portál Office a na ploše hello Windows 10| | |![Doporučené](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> synchronizace hesel pomocí jednotného přihlašování. 

> <sup>2</sup> předávací ověřování a jednotné přihlašování. 

> <sup>3</sup> federované jednotné přihlašování se službou AD FS.

> <sup>4</sup> služby AD FS se dá integrovat s vaší podnikové infrastruktury veřejných KLÍČŮ tooallow přihlášení pomocí certifikátů. Tyto certifikáty jde konfigurace soft certifikáty nasazené přes důvěryhodné zřizování kanálů, jako je například certifikáty MDM nebo čipová karta nebo objektu zásad skupiny (včetně karet standardu PIV/CAC) nebo Hello pro firmy (cert vztahu důvěryhodnosti). Další informace o podpoře ověřování pomocí čipové karty, najdete v části [tomto blogu](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).


## <a name="next-steps"></a>Další kroky
[Další informace v prostředí Azure testování konceptu](https://aka.ms/aad-poc)

[Instalace služby Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)

[Monitorování synchronizace hybridní identity](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)

