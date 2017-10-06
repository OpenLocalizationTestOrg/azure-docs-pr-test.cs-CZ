---
title: "aaaAzure zásady podmíněného přístupu zařízení služby Active Directory pro služby Office 365 | Microsoft Docs"
description: "Informace o tom, jak tooprovision podmíněného přístupu zařízení zásady toohelp zabezpečit podnikové prostředky, při zachování tooservices dodržování předpisů a přístup uživatele."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8664c0bb-bba1-4012-b321-e9c8363080a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 38229a8d9766efbf0e6b17875b3018011c6b4ea3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="active-directory-conditional-access-device-policies-for-office-365-services"></a>Active Directory podmíněného přístupu zařízení zásady pro služby Office 365

Podmíněný přístup vyžaduje více částí toowork. Zahrnuje službu Multi-Factor ověřeného uživatele, ověřené zařízení a kompatibilní zařízení, mezi dalších faktorů. V tomto článku především zaměříme na zařízení na základě podmínek, vaší organizace můžete řídit přístup ke službě 365 tooOffice toohelp. 

Podnikoví uživatelé mají služby tooaccess Office 365, jako je Exchange a SharePoint Online v práci nebo ve škole ze svých osobních zařízení. Chcete hello přístup toobe zabezpečené. Můžete zřídit podmíněného přístupu zařízení zásady toohelp zkontrolujte podnikovým prostředkům bezpečnější při udělování přístupu tooservices pro uživatele, kteří používají kompatibilní zařízení. TooOffice zásady podmíněného přístupu 365 můžete nastavit v portálu podmíněného přístupu hello Microsoft Intune.

Azure Active Directory (Azure AD) vynucuje podmíněného přístupu zásady toohelp zabezpečený přístup tooOffice 365 služby. Můžete vytvořit zásady podmíněného přístupu, které blokuje uživatele, který používá nekompatibilních zařízení z přístup ke službě Office 365. Hello uživatele musí odpovídat zásady toohello společnosti zařízení před udělením přístupu toohello služby. Alternativně můžete vytvořit zásady, které vyžadují uživatelé tooenroll jejich zařízení toogain přístup tooan služby Office 365. Zásady můžou být použité tooall uživatelé v organizaci, nebo jsou omezena tooa několik cílových skupin. Můžete přidat další zásady tooa cílové skupiny v čase.

Předpokladem pro vynucování zásad zařízení je, že uživatelé musí registrovat svá zařízení se službou registrace zařízení hello Azure AD. Můžete se taky rozhodnout tooturn na vícefaktorového ověřování pro zařízení, která zaregistrovat hello služby registrace zařízení služby Azure AD. Služba Multi-Factor authentication se doporučuje pro hello služby Azure Active Directory device registration service. Po zapnutí ověřování Multi-Factor authentication, uživatelé, kteří registrovat svá zařízení s hello služby registrace zařízení služby Azure AD jsou postiženy pro dvoufaktorové ověřování.

## <a name="how-does-a-conditional-access-policy-work"></a>Jak funguje zásady podmíněného přístupu?

Když si uživatel vyžádá z platformy podporované zařízení přístup tooan služby Office 365, Azure AD ověřuje hello uživatelů a zařízení hello. Službu Azure AD uděluje přístup toohello pouze v případě, že uživatel hello vyhovuje toohello zásady nastavené pro službu hello. Uživatelé na zařízeních, která nejsou zaregistrovaná mít pokyny, jak tooenroll a označují jako vyhovující tooaccess podnikové služby Office 365. Uživatelé na zařízení iOS a Android jsou požadované tooenroll svá zařízení pomocí aplikace portál společnosti Intune hello. Když se uživatel registruje zařízení, hello zařízení zaregistrované v Azure AD a že je zaregistrováno pro správu zařízení a dodržování předpisů. Musíte použít službu device registration service hello Azure AD s Microsoft Intune pro správu mobilních zařízení pro služby Office 365. Registrace zařízení je vyžadovaná pro tooaccess uživatelů služeb Office 365, když se vynucují zásady zařízení.

Pokud uživatel úspěšně zaregistruje zařízení, hello zařízení stane důvěryhodné. Azure AD poskytuje hello ověřený uživatel přístup přihlášení toocompany aplikace. Azure AD vynucuje podmíněného přístupu zásad toogrant přístup tooa služby nejen hello hello nový uživatel požaduje přístup, ale každý uživatel hello čas obnoví žádost o přístup. Hello uživateli odepřen přístup tooservices, když se změní přihlašovací údaje, dojde ke ztrátě nebo odcizení zařízení hello nebo v době hello požadavku pro obnovení nejsou splněny podmínky hello hello zásady.

## <a name="deployment-considerations"></a>Aspekty nasazování

Je nutné použít zařízení tooregister služby registrace zařízení pro Azure AD hello.

Když jsou uživatelé místní o toobe ověření služby Active Directory Federation Services (AD FS) (verze 1.0 a novějších verzí) se vyžaduje. Víceúrovňové ověřování pro připojení k pracovní ploše selže, když zprostředkovatele identity hello nedovede služby Multi-Factor authentication. Nelze například používání služby Multi-Factor authentication s AD FS 2.0. Ujistěte se, že hello místní služby AD FS funguje s vícefaktorovým ověřováním, a že metodu platný služby Multi-Factor authentication je na místě před zapnutím služby Multi-Factor authentication pro službu registrace zařízení služby Azure AD hello. Například služby AD FS v systému Windows Server 2012 R2 má možnosti služby Multi-Factor authentication. Je třeba také nastavit metodu dodatečného ověřování platné (vícefaktorového ověřování) na serveru služby AD FS hello před zapnutím služby Multi-Factor authentication pro službu registrace zařízení hello Azure AD. Další informace o metodách podporované vícefaktorového ověřování ve službě AD FS najdete v tématu [konfigurace dalších metod ověřování pro službu AD FS](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs).

## <a name="next-steps"></a>Další kroky

*   Otázky toocommon odpovědi, najdete v části [podmíněného přístupu nejčastější dotazy k Azure Active Directory](active-directory-conditional-faqs.md).
