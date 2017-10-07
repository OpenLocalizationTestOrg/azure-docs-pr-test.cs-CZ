---
title: "aaaAzure AD Connect a federační | Microsoft Docs"
description: "Tato stránka je hello centrálního umístění pro veškerá dokumentace týkající se provozu služby AD FS, které používají Azure AD Connect."
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: anandy
ms.openlocfilehash: dc70206eee2296c2320712ef2ade48ccebcc912d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect a federace
Azure Active Directory (Azure AD) připojit umožňuje konfiguraci federace pomocí místní služby Active Directory Federation Services (AD FS) a Azure AD. S federační přihlášení můžete povolit toosign uživatelé v tooAzure služby AD s místními hesla – a, když v podnikové síti hello, bez nutnosti tooenter hesla znovu. Při použití možnosti hello federační službou AD FS můžete nasadit nové instalace služby AD FS, nebo můžete zadat existující instalace ve farmě Windows serveru 2012 R2.

Toto téma je hello domácí informace o souvisejících s federační funkce Azure AD Connect. Vypíše odkazy tooall Příbuzná témata. Pro odkazy tooAzure AD Connect, najdete v části [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect: témata federace
| Téma | Co pokrývá a kdy tooread ho |
|:--- |:--- |
| **Azure AD Connect uživatelské možnosti přihlášení** | |
| [Pochopení možností přihlášení uživatele](active-directory-aadconnect-user-signin.md) |Další informace o různých uživatel přihlašovací možností a jak ovlivňují činnost koncového uživatele hello Azure přihlášení. |
| **Instalace služby AD FS pomocí Azure AD Connect** | |
| [Požadavky](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |Viz hello požadavky pro úspěšnou instalaci služby AD FS pomocí Azure AD Connect. |
| [Konfigurovat farmu služby AD FS](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |Nainstalujte novou farmu služby AD FS pomocí Azure AD Connect. |
| [Vytvořit federaci s Azure AD pomocí alternativního přihlašovacího ID](active-directory-aadconnect-federation-management.md#alternateid) | Konfigurace federace pomocí alternativního přihlašovacího ID  |
| **Upravte konfiguraci hello AD FS** | |
| [Oprava hello důvěryhodnosti](active-directory-aadconnect-federation-management.md#repairthetrust) |Oprava hello aktuální důvěryhodnosti mezi místní služby AD FS a Office 365 nebo Azure. |
| [Přidejte nový server služby AD FS](active-directory-aadconnect-federation-management.md#addadfsserver) |Rozbalte farmu služby AD FS s dalším serverem služby AD FS po počáteční instalaci. |
| [Přidejte nový server AD FS WAP](active-directory-aadconnect-federation-management.md#addwapserver) |Rozbalte farmu služby AD FS s dalším serverem Proxy webových aplikací (WAP) po počáteční instalaci. |
| [Přidání nové federované domény](active-directory-aadconnect-federation-management.md#addfeddomain) |Přidání další domény toobe sdružených se službou Azure AD. |
| [Aktualizovat certifikát SSL hello](active-directory-aadconnectfed-ssl-update.md)| Aktualizujte certifikát SSL hello pro farmu služby AD FS. |
| **Další konfigurace federace** | |
| [Federování několika instancí Azure AD s jednou instancí AD FS](active-directory-aadconnectfed-single-adfs-multitenant-federation.md) | Vytvořit federaci několika Azure AD s jedné farmy služby AD FS| 
| [Přidat vlastní firemní logo/obrázku](active-directory-aadconnect-federation-management.md#customlogo) |Upravte hello přihlašování uživatelů zadáním hello vlastní logo, které se zobrazí na stránce přihlášení hello AD FS. |
| [Přidejte popis přihlášení](active-directory-aadconnect-federation-management.md#addsignindescription) |Změnit hello přihlášení popis na přihlašovací stránku hello služby AD FS. |
| [Upravit pravidla deklarace identity služby AD FS](active-directory-aadconnect-federation-management.md#modclaims) |Úprava nebo přidání pravidla deklarace identity ve službě AD FS, která odpovídají konfigurace tooAzure AD Connect sync. |


## <a name="additional-resources"></a>Další zdroje
* [Federaci dvě Azure AD s jedné služby AD FS](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)
* [Nasazení služby AD FS v Azure](active-directory-aadconnect-azure-adfs.md)
* [Nasazení vysoké dostupnosti geografické mezi AD FS v Azure s Azure Traffic Manager](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
