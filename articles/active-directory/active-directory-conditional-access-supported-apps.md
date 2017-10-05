---
title: "Aplikace a prohlížeče, které používají pravidla podmíněného přístupu v Azure Active Directory | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu Azure Active Directory ověří za určitých podmínek, když se ověřuje uživatele a umožnit přístup k aplikaci."
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
ms.openlocfilehash: 8614660f7c98af7b4e6d50348775495c67ae1cc8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="applications-and-browsers-that-use-conditional-access-rules-in-azure-active-directory"></a>Aplikace a prohlížeče, které používají pravidla podmíněného přístupu v Azure Active Directory

Pravidla podmíněného přístupu jsou podporovány v Azure Active Directory (Azure AD)-připojený aplikace, předem integrovaných federované software jako služba (SaaS) aplikace, aplikace, které používají heslo jednotné přihlašování (SSO),-obchodní aplikace a aplikace, které používají Azure AD Application Proxy. Podrobný seznam aplikací, které můžete použít podmíněný přístup, najdete v části [povolit služby s podmíněným přístupem](active-directory-conditional-access-technical-reference.md). Podmíněný přístup funguje i u mobilních a desktopových aplikací, které používají moderní ověřování. V tomto článku nabídneme jak podmíněný přístup funguje v mobilních a desktopových aplikací.

V aplikacích, které používají moderní ověřování můžete použít Azure AD přihlašovací stránky. S přihlašovací stránkou uživatel vyzván k služby Multi-Factor authentication. Zpráva se zobrazí, pokud je blokován přístup uživatele. Moderní ověřování je požadované pro zařízení k ověřování s Azure AD, tak, aby se vyhodnocují zásady podmíněného přístupu podle zařízení.

Je důležité vědět, které aplikace můžete použít pravidla podmíněného přístupu a kroky, které možná budete muset provést k zajištění zabezpečení další vstupní body aplikace.

## <a name="applications-that-use-modern-authentication"></a>Aplikace, které používají moderní ověřování
> [!NOTE]
> Pokud máte zásadu podmíněného přístupu ve službě Azure AD, který má ekvivalentní v Office 365, nakonfigurujte obě zásady podmíněného přístupu. To platí, například zásady podmíněného přístupu pro Exchange Online nebo SharePoint Online.
>
>

Následující aplikace podporují podmíněný přístup pro Office 365 a další služby Azure AD připojené aplikace:


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
| Office 365 Sharepointu Online| Windows 10| Aplikace Office 2016, Office univerzální aplikace, Office 2013 (s moderní ověřování), OneDrive synchronizace klienta (najdete v části [poznámky](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), podporu skupiny Office plánované do budoucna plánované do budoucna podpora aplikací služby SharePoint|
| Office 365 Sharepointu Online| Windows 8.1, Windows 7| Aplikace Office 2016, Office 2013 (s moderní ověřování), Onedrivu synchronizovat klienta (viz [poznámky](https://support.office.com/en-US/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|
| Office 365 Sharepointu Online| iOS, Android| Mobilní aplikace Office|
| Office 365 Sharepointu Online| Mac OS X| Office 2016 pro systému macOS (Word, Excel, PowerPoint, OneNote pouze). OneDrive pro firmy podporu plánované v budoucnosti|
| Yammer Office 365| Windows 10, iOS, Android| Aplikace Yammer Office|
| Služba PowerBI| Windows 10, Windows 8.1, Windows 7 a iOS| Aplikaci PowerBI. Power BI aplikace pro Android v současné době nepodporuje podmíněného přístupu podle zařízení.|
| Visual Studio Team Services| Windows 10, Windows 8.1, Windows 7, iOS a Android| Visual Studio Team Services aplikace|








## <a name="applications-that-do-not-use-modern-authentication"></a>Aplikace, které nepoužívají moderní ověřování
V současné době je nutné použít jiné metody blokovat přístup k aplikacím, které nepoužívají moderní ověřování. Pravidla přístupu pro aplikace, které nepoužívají moderní ověřování vynucovat podmíněný přístup. Toto je primárně zabývat pro přístup k systému Exchange a SharePoint. Většina starších verzí aplikací používají starší protokoly řízení přístupu.

### <a name="control-access-in-office-365-sharepoint-online"></a>Řízení přístupu v Office 365 Sharepointu Online
Zastaralé protokoly pro přístup k Sharepointu můžete zakázat pomocí rutiny Set-SPOTenant. Chcete-li zabránit klientům Office, které používají jiných než moderních ověřovacích protokolů v přístupu k prostředkům služby SharePoint Online, použijte tuto rutinu.

**Příklad**:`Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Řízení přístupu v Office 365 Exchange Online
Exchange nabízí dvou hlavních kategorií protokolů. Projděte si následující možnosti a pak vyberte zásadu, která je pro organizaci správné.

* **Exchange ActiveSync**. Ve výchozím nastavení vynucovat zásady podmíněného přístupu pro službu Multi-Factor authentication a umístění pro Exchange ActiveSync. Je třeba chránit přístup k těmto službám, buď přímo konfigurací zásad protokolu Exchange ActiveSync, nebo pomocí blokování protokolu Exchange ActiveSync pomocí pravidel Active Directory Federation Services (AD FS).
* **Zastaralé protokoly**. Můžete blokovat starších protokolů se službou AD FS. To blokuje přístup na starší klienty Office, jako je například Office 2013 bez moderní ověřování povoleno a dřívějších verzí sady Office.

### <a name="use-ad-fs-to-block-legacy-protocol"></a>Blokovat starší verze protokolu pomocí služby AD FS
Následující příklad autorizační pravidla vystavování můžete použít k blokování přístupu starší verze protokolu na úrovni služby AD FS. Vyberte z dvě běžné konfigurace.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Možnost 1: Povolit Exchange ActiveSync a povolit starší verze aplikace, ale jenom na intranetu
Při použití následující tři pravidla pro vztah důvěryhodnosti předávající strany pro aplikaci Microsoft Office 365 Identity Platform, provoz protokolu Exchange ActiveSync a prohlížeče a přenosy moderní ověřování služby AD FS, máte přístup. Starší verze aplikace jsou blokovaná z extranetu.

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
Při použití následující tři pravidla pro vztah důvěryhodnosti předávající strany pro aplikaci Microsoft Office 365 Identity Platform, provoz protokolu Exchange ActiveSync a prohlížeče a přenosy moderní ověřování služby AD FS, máte přístup. Starší verze aplikace jsou blokovaná z libovolného místa.

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

Pokud Azure AD můžete identifikovat a ověřovat zařízení, může získat pouze pro zařízení na základě zásady, které kontrolují přístup pro dodržování předpisů pro zařízení a připojení k doméně. Při většina kontroly, jako je umístění a pracovní MFA na většině zařízení a prohlížeče zásady zařízení vyžadují verze operačního systému a prohlížeče, které jsou uvedeny níže. Pro uživatele na nepodporované prohlížeče nebo operační systémy je blokován přístup, když se zásada zařízení na místě. 

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
> Podpora Chrome, které musí být pomocí Windows 10 Creators Update a nainstalovat nalezeno rozšíření [zde](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).
>
>

## <a name="next-steps"></a>Další kroky

Další podrobnosti najdete v tématu [podmíněný přístup v Azure Active Directory](active-directory-conditional-access.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-supported-apps/ic195031.png


