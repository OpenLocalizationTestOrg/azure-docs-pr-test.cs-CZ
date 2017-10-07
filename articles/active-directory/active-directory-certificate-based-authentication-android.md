---
title: "ověřování pomocí certifikátů aaaAzure služby Active Directory v systému Android | Microsoft Docs"
description: "Další informace o hello Podporované scénáře a hello požadavky pro konfiguraci ověřování na základě certifikátů v řešení se zařízeními s Androidem"
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 148275fa3da610530c278fcd57e02e907f735d9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Azure Active Directory ověřování prostřednictvím certifikátu v systému Android


Ověřování pomocí certifikátů (CBA) vám umožní toobe službou Azure Active Directory se ověřit klientský certifikát na zařízení s Windows, Android nebo iOS při připojování váš účet systému Exchange online: 

* Mobilní aplikace Office, jako je například Microsoft Outlook a Microsoft Word   
* Klienti Exchange ActiveSync (EAS) 

Konfigurace tato funkce eliminuje hello nutné tooenter uživatelské jméno a heslo kombinaci do určité e-mailu a aplikace Microsoft Office na vašem mobilním zařízení. 

Toto téma poskytuje hello Podporované scénáře a požadavky na hello CBA konfigurace pro zařízení s iOS(Android) pro uživatele klientů v Office 365 Enterprise, Business, Education, US Government, Čína, a plány Německu.



Tato funkce je dostupná ve verzi preview v Office 365 US Government obrany a federální plány.


## <a name="office-mobile-applications-support"></a>Podpora mobilních aplikacích Office
| Aplikace | Podpora |
| --- | --- |
| Azure Information Protection aplikace |![Zaškrtnout][1] |
| Microsoft Teams |![Zaškrtnout][1] |
| OneNote |![Zaškrtnout][1] |
| OneDrive |![Zaškrtnout][1] |
| Outlook |![Zaškrtnout][1] |
| Mobilních aplikací Power BI |![Zaškrtnout][1] |
| Skype pro firmy |![Zaškrtnout][1] |
| Aplikace Word / Excel / PowerPoint |![Zaškrtnout][1] |
| Yammer |![Zaškrtnout][1] |


### <a name="implementation-requirements"></a>Požadavky na implementaci

Hello verze operačního systému zařízení musí být Android 5.0 (typu Lupa) a vyšší. 

Federační server musí být nakonfigurované.  

Pro Azure Active Directory toorevoke klientský certifikát musí mít tokenu služby AD FS hello hello následující deklarace identity:  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  (hello sériové číslo certifikátu klienta hello) 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  (hello řetězec hello vystavitele certifikátu klienta hello) 

Azure Active Directory přidá tyto deklarace identity toohello obnovovací token, pokud jsou k dispozici v tokenu služby AD FS hello (nebo jiné tokenu SAML). Pokud token obnovení hello potřebuje toobe ověřit, tyto informace jsou použité toocheck hello odvolání. 

Jako osvědčený postup, by měl aktualizovat hello služby AD FS chybové stránky s pokyny, jak tooget certifikát uživatele.  
Další podrobnosti najdete v tématu [přizpůsobení stránek přihlášení AD FS hello](https://technet.microsoft.com/library/dn280950.aspx).  

Některé aplikace Office (s povolené moderní ověřování) odeslat '*řádku = přihlášení*' tooAzure AD v jejich požadavku. Ve výchozím nastavení, Azure AD to převede v požadavku tooADFS hello příliš '*wauth = usernamepassworduri*' (požádá ověřování U/P toodo služby AD FS) a "*wfresh = 0*' (požádá stavu jednotného přihlašování k tooignore služby AD FS a provést novou ověřování) . Pokud chcete, aby tooenable ověřování pomocí certifikátů pro tyto aplikace, je třeba hello toomodify se výchozí chování Azure AD. Právě sadu hello '*PromptLoginBehavior*' v nastaveních federovanou doménu příliš '*zakázané*'. Můžete použít hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) tooperform rutiny tento úkol:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`



## <a name="exchange-activesync-clients-support"></a>Podporu klientů protokolu Exchange ActiveSync
Jsou podporovány některých aplikací Exchange ActiveSync v systému Android 5.0 (typu Lupa) nebo novější. toodetermine Pokud e-mailové aplikace podporuje tuto funkci, kontaktujte prosím vašeho vývojáře aplikace. 


## <a name="next-steps"></a>Další kroky

Pokud chcete ověřování pomocí certifikátů tooconfigure ve vašem prostředí, najdete v části [Začínáme s ověřováním na základě certifikátu v systému Android](active-directory-certificate-based-authentication-get-started.md) pokyny.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
