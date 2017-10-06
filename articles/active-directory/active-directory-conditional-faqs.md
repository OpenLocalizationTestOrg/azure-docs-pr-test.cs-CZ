---
title: "aaaAzure podmíněného přístupu služby Active Directory nejčastější dotazy | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se podmíněného přístupu v Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d23acbb01217d7e9717d1a43de1b46a929404118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Active Directory podmíněného přístupu nejčastější dotazy

## <a name="which-applications-work-with-conditional-access-policies"></a>Aplikace, které podporují zásady podmíněného přístupu?

Informace o aplikacích, které pracují se zásadami podmíněného přístupu najdete v tématu [aplikací a prohlížeče, které používají pravidla podmíněného přístupu v Azure Active Directory](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Prosazují zásady podmíněného přístupu pro spolupráci B2B a uživatele typu Host?

Zásady se vynucují pro business-to-business (B2B) spolupráce uživatele. V některých případech ale uživatel nemusí být možné toosatisfy požadavky zásad hello. Například uživatel typu Host organizace nemusí podporovat službu Multi-Factor authentication. 



## <a name="does-a-sharepoint-online-policy-also-apply-tooonedrive-for-business"></a>Zásady Sharepointu Online také platí tooOneDrive pro firmy?

Ano. Zásady Sharepointu Online platí také tooOneDrive pro firmy.


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Proč nelze nastavit zásadu pro klientské aplikace, jako je Word nebo Outlook?

Požadavky pro přístup k službě nastavuje zásady podmíněného přístupu. Prosazuje se při ověřování toothat služby. přímo na klientskou aplikaci nejsou nastavené zásady Hello. Místo toho se použije, když klient zavolá službu. Například zásadu nastavit u SharePoint platí tooclients volání služby SharePoint. Zásadu nastavit u Exchange platí tooOutlook.

## <a name="does-a-conditional-access-policy-apply-tooservice-accounts"></a>Platí zásady podmíněného přístupu tooservice účty?

Zásady podmíněného přístupu použije tooall uživatelské účty. To zahrnuje uživatelské účty, které se používají jako účty služeb. Účet služby, který spouští bezobslužné často nelze vyhovět požadavkům hello zásady podmíněného přístupu. Služba Multi-Factor authentication může být například vyžaduje. Účty služby můžete vyloučit ze zásady pomocí nastavení zásad řízení podmíněného přístupu. 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a>Rozhraní Graph API jsou k dispozici ke konfiguraci zásad podmíněného přístupu?

V současné době není. 

## <a name="what-is-hello-default-exclusion-policy-for-unsupported-device-platforms"></a>Co je hello výchozí vyloučení zásady pro platformy nepodporované zařízení?

V současné době se na uživatele se systémy iOS a zařízení se systémem Android selektivně vynucují zásady podmíněného přístupu. Aplikace na jiných platformách zařízení se ve výchozím nastavení, není vliv hello zásady podmíněného přístupu pro zařízení iOS a Android. Správce klienta můžete zvolit toooverride hello globální zásady toodisallow přístup toousers na platformách, které nejsou podporovány.


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Jak fungují zásady podmíněného přístupu pro Teams společnosti Microsoft?  

Microsoft Teams velice spoléhá na Exchange Online a SharePoint Online pro základní produktivitu scénáře, jako je schůzek, kalendáře a sdílení souborů. Zásady podmíněného přístupu, které jsou nastavené na těchto cloudových aplikací použít tooMicrosoft týmy, když se uživatel přihlásí.

Teams společnosti Microsoft je podporováno také samostatně jako cloudové aplikace v Azure Active Directory zásady podmíněného přístupu. Zásady certifikátu autority, které jsou nastavené pro cloudové aplikace použít tooMicrosoft týmy, když se uživatel přihlásí.

Klienti plocha Teams společnosti Microsoft pro systém Windows a Mac podporují moderní ověřování. Moderní ověřování poskytuje přihlašování založené na hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office klientské aplikace napříč platformami. 
