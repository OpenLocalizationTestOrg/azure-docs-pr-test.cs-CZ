---
title: "aaaApplications a prohlížeče, které používají pravidla podmíněného přístupu v Azure Active Directory | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu Azure Active Directory zjišťuje konkrétní podmínky během ověřování uživatele hello a tooallow přístup k aplikaci."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ca20853bb9f4b22d0b88ddd2f051d658e0d88cf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="applications-and-browsers-that-use-conditional-access-rules-in-azure-active-directory"></a>Aplikace a prohlížeče, které používají pravidla podmíněného přístupu v Azure Active Directory

Pravidla podmíněného přístupu jsou podporovány v Azure Active Directory (Azure AD)-připojený aplikace, předem integrovaných federované software jako služba (SaaS) aplikace, aplikace, které používají heslo jednotné přihlašování (SSO),-obchodní aplikace a aplikace, které používají Azure AD Application Proxy. Podrobný seznam aplikací, které můžete použít podmíněný přístup, najdete v části [povolit služby s podmíněným přístupem](active-directory-conditional-access-technical-reference.md). Podmíněný přístup funguje i u mobilních a desktopových aplikací, které používají moderní ověřování. V tomto článku nabídneme jak podmíněný přístup funguje v mobilních a desktopových aplikací.

V aplikacích, které používají moderní ověřování můžete použít Azure AD přihlašovací stránky. S přihlašovací stránkou uživatel vyzván k služby Multi-Factor authentication. Zpráva se zobrazí, pokud je blokován přístup hello uživatele. Moderní ověřování je vyžadována pro hello tooauthenticate zařízení s Azure AD, tak, aby se vyhodnocují zásady podmíněného přístupu podle zařízení.

Je důležité tooknow aplikace, které můžete použít pravidla podmíněného přístupu a hello kroky, které můžete potřebovat tootake toosecure další vstupní body aplikace.

## <a name="applications-that-use-modern-authentication"></a>Aplikace, které používají moderní ověřování
> [!NOTE]
> Pokud máte zásadu podmíněného přístupu ve službě Azure AD, který má ekvivalentní v Office 365, nakonfigurujte obě zásady podmíněného přístupu. To platí, například tooconditional zásady přístupu pro Exchange Online nebo SharePoint Online.
>
>

Hello následující aplikace podporují podmíněný přístup pro Office 365 a další služby Azure AD připojené aplikace:


| Cílová služba| Platforma| Aplikace |
| --- | --- | --- |
| Všechny služby app service pro Moje aplikace| Android a iOS| MFA a umístění zásady pro aplikace. Zásady zařízení na základě nejsou podporovány.|
| Služba vzdálené aplikace Azure| Windows 10, Windows 8.1, Windows 7, iOS, Android a Mac OS X| Vzdálené aplikace Azure|
| Dynamics CRM| Windows 10, Windows 8.1, Windows 7, iOS a Android| Aplikaci Dynamics CRM|
| Microsoft Teams| Windows 10, Windows 8.1, Windows 7, iOS nebo Android a MAC OSX| Microsoft týmy služby – tato volba určuje všechny služby, které podporují Teams společnosti Microsoft a její klientské aplikace – Windows Desktop, MAC OS X, iOS, Android, webové části a webového klienta|
| Office 365 Exchange Online| Windows 10| E-mailu, kalendáři nebo osoby aplikace Outlook 2016, aplikace Outlook 2013 (s moderní ověřování), Skype pro firmy (s moderní ověřování)|
| Office 365 Exchange Online| Windows 8.1, Windows 7| Outlook 2016, aplikace Outlook 2013 (s moderní ověřování), Skype pro firmy (s moderní ověřování)|
| Office 365 Exchange Online| iOS| Mobilní aplikace Outlook|
| Office 365 Exchange Online| Mac OS X| Outlook 2016 (Office pro systému macOS)|
| Office 365 Sharepointu Online| Windows 10| Aplikace Office 2016, Office univerzální aplikace, Office 2013 (s moderní ověřování), OneDrive synchronizace klienta (v tématu [poznámky](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), podporu skupiny Office plánované pro budoucí hello plánované podpora aplikací služby SharePoint pro budoucí hello|
| Office 365 Sharepointu Online| Windows 8.1, Windows 7| Aplikace Office 2016, Office 2013 (s moderní ověřování), Onedrivu synchronizovat klienta (viz [poznámky](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|
| Office 365 Sharepointu Online| iOS, Android| Mobilní aplikace Office|
| Office 365 Sharepointu Online| Mac OS X| Office 2016 pro systému macOS (Word, Excel, PowerPoint, OneNote pouze). OneDrive pro firmy podporu plánované pro budoucí hello|
| Yammer Office 365| Windows 10, iOS, Android| Aplikace Yammer Office|
| Služba PowerBI| Windows 10, Windows 8.1, Windows 7 a iOS| Aplikaci PowerBI. Hello Power BI aplikace pro Android v současné době nepodporuje podmíněného přístupu podle zařízení.|
| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS a Android| Visual Studio Team Services aplikace|








## <a name="applications-that-do-not-use-modern-authentication"></a>Aplikace, které nepoužívají moderní ověřování
V současné době je nutné použít jiné metody tooblock přístup tooapps, který nepoužívejte moderní ověřování. Pravidla přístupu pro aplikace, které nepoužívají moderní ověřování vynucovat podmíněný přístup. Toto je primárně zabývat pro přístup k systému Exchange a SharePoint. Většina starších verzí aplikací používají starší protokoly řízení přístupu.

### <a name="control-access-in-office-365-sharepoint-online"></a>Řízení přístupu v Office 365 Sharepointu Online
Zastaralé protokoly pro přístup k Sharepointu můžete zakázat pomocí rutiny Set-SPOTenant hello. Pomocí této rutiny tooprevent Office klienty, kteří používají jiných než moderních ověřovacích protokolů v přístupu k prostředkům služby SharePoint Online.

**Příklad**:`Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Řízení přístupu v Office 365 Exchange Online
Exchange nabízí dvou hlavních kategorií protokolů. Zkontrolujte hello následující možnosti a pak vyberte hello zásad, který je pro organizaci správné.

* **Exchange ActiveSync**. Ve výchozím nastavení vynucovat zásady podmíněného přístupu pro službu Multi-Factor authentication a umístění pro Exchange ActiveSync. Je nutné tooprotect přístup toothese služby konfigurace přímo zásad protokolu Exchange ActiveSync, nebo zasláním blokování protokolu Exchange ActiveSync pomocí pravidel Active Directory Federation Services (AD FS).
* **Zastaralé protokoly**. Můžete blokovat starších protokolů se službou AD FS. To blokuje přístup tooolder Office klienty, například Office 2013 bez moderní ověřování povoleno a dřívějších verzí sady Office.

### <a name="use-ad-fs-tooblock-legacy-protocol"></a>Použití služby AD FS tooblock starší verze protokolu
Můžete použít následující příklad vystavování autorizační pravidla tooblock starší verze protokolu přístupu na úrovni hello služby AD FS hello. Vyberte z dvě běžné konfigurace.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-hello-intranet"></a>Možnost 1: Povolení protokolu Exchange ActiveSync a povolit starší verze aplikace, ale jenom na intranetu hello
Použitím hello následující tři pravidla toohello vztah důvěryhodnosti předávající strany služby AD FS pro Microsoft Office 365 Identity Platform, provoz protokolu Exchange ActiveSync, a prohlížeče a přenosy dat moderní ověřování, máte přístup. Starší verze aplikace jsou blokovaná z extranetu hello.

##### <a name="rule-1"></a>Pravidlo 1
    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Pravidlo 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Pravidlo 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Možnost 2: Povolit Exchange ActiveSync a blokovat starší verze aplikace
Použitím hello následující tři pravidla toohello vztah důvěryhodnosti předávající strany služby AD FS pro Microsoft Office 365 Identity Platform, provoz protokolu Exchange ActiveSync, a prohlížeče a přenosy dat moderní ověřování, máte přístup. Starší verze aplikace jsou blokovaná z libovolného místa.

##### <a name="rule-1"></a>Pravidlo 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Pravidlo 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Pravidlo 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


## <a name="supported-browsers-for-device-based-policies"></a>Podporované prohlížeče pro zařízení na základě zásad 

Pokud Azure AD můžete identifikovat a ověřovat hello zařízení, může získat pouze přístup pro zařízení na základě zásady, které kontrolují pro dodržování předpisů pro zařízení a připojení k doméně. Při většina kontroly, jako je umístění a pracovní MFA na většině zařízení a prohlížeče zásady zařízení vyžadují verze hello operačního systému a prohlížeče, které jsou uvedeny níže. Pro uživatele na nepodporované prohlížeče nebo hello operačních systémů je blokován přístup po zavedené zásady zařízení. 

| Operační systém                     | Prohlížeče                 | Podpora     |
| :--                    | :--                      | :-:         |
| Windows 10                 | IE Edge                 | ![Zaškrtnout][1] |
| Windows 10                 | Chrome                   | Preview     |
| Win 8 / 8.1            | IE Chrome               | ![Zaškrtnout][1] |
| Windows 7                  | IE Chrome               | ![Zaškrtnout][1] |
| iOS                    | Safari                   | ![Zaškrtnout][1] |
| Android                | Chrome                   | ![Zaškrtnout][1] |
| telefon se systémem Windows          | IE Edge                 | ![Zaškrtnout][1] |
| Windows Server 2016    | IE Edge                 | ![Zaškrtnout][1] |
| Windows Server 2016    | Chrome                   | Již brzy |
| Windows Server 2012 R2 | IE Chrome               | ![Zaškrtnout][1] |
| Windows Server 2008 R2 | IE Chrome               | ![Zaškrtnout][1] |
| Mac OS                 | Safari                   | ![Zaškrtnout][1] |
| Mac OS                 | Chrome                   | Již brzy |

> [!NOTE]
> Podpora Chrome, používejte Windows 10 Creators Update a instalace rozšíření hello nalezeno [zde](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).
>
>

## <a name="next-steps"></a>Další kroky

Další podrobnosti najdete v tématu [podmíněný přístup v Azure Active Directory](active-directory-conditional-access.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-supported-apps/ic195031.png


