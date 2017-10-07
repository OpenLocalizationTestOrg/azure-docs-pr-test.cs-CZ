---
title: "aaaAzure technické informace o službě Active Directory podmíněného přístupu | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu Azure Active Directory kontroluje hello konkrétní podmínky, kterou vyberete při ověřování uživatele hello a před povolením přístupu toohello aplikace. Po splnění těchto podmínek hello uživatel ověří a povolená přístup toohello aplikace."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ee201405d1d17f130607a95bf455b60cd222dd0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Technické informace o Azure Active Directory podmíněného přístupu

## <a name="services-enabled-with-conditional-access"></a>Služba povolena s podmíněným přístupem

Pravidla podmíněného přístupu jsou podporovány v rámci různých typů aplikací Azure AD. Tento seznam obsahuje:


* Aplikace, které jsou registrovány hello Proxy aplikace služby Azure
* Vzdálené aplikace Azure
* Vyvinuté řádek obchodní a víceklientské aplikace zaregistrované s Azure AD
* Dynamics CRM
* Federované aplikace hello galerii aplikací Azure AD
* Aplikace Microsoft Office 365 Yammer
* Aplikace Microsoft Office 365 Exchange Online
* Aplikace Microsoft Office 365 SharePoint Online (zahrnuje Onedrivu pro firmy)
* Microsoft Power BI 
* Heslo aplikace jednotného přihlašování z Galerie aplikace hello Azure AD
* Visual Studio Team Services
* Microsoft Teams









## <a name="enable-access-rules"></a>Povolení pravidla přístupu
Každé pravidlo můžete povolit nebo zakázat v na základů aplikace. Pokud jsou pravidla **ON** bude možné povolit a vynutit pro uživatele, kteří používají aplikace hello. Když jsou **OFF** nebudou použity a není dopad hello přihlášení prostředí.

## <a name="applying-rules-toospecific-users"></a>Použití pravidla toospecific uživatelů
Pravidla mohou být použité toospecific skupiny uživatelů podle skupiny zabezpečení nastavením **použít na**. **Použít na** lze nastavit příliš**všichni uživatelé** nebo **skupiny**. Pokud nastavíte příliš**všichni uživatelé** hello pravidla budou platit tooany uživatele s aplikací toohello přístup. Hello **skupiny** možnost umožňuje konkrétní zabezpečení a distribučních skupin toobe vybrali, pravidla se vynucují jenom pro tyto skupiny.

Pokud nasazujete pravidlo, je běžné toofirst použít omezenou sadu uživatelů, které jsou členy skupin pilotní. Po dokončení hello pravidlo může být příliš**všichni uživatelé**. To způsobí, že pravidlo hello toobe vynucuje pro všechny uživatele v organizaci hello.

Vyberte skupiny může být také vyloučené ze zásad pomocí hello **s výjimkou** možnost. Všechny členy těchto skupin bude vyloučené i v případě, že se objeví ve skupině součástí.

## <a name="at-work-networks"></a>Sítě "v práci"
Pravidla podmíněného přístupu, které používají síť "v práci" spoléhat na důvěryhodný rozsahy IP adres, které byly konfigurovány ve službě Azure AD, nebo použijte hello "uvnitř corpnet" deklarace identity ze služby AD FS. Tato pravidla zahrnují:

* Vyžadovat vícefaktorové ověřování, když není v práci
* Blokování přístupu, když není v práci

Možnosti pro zadání sítě "v práci"

1. Konfigurovat důvěryhodné rozsahy IP adres v hello [stránku konfigurace služby Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Zásadu podmíněného přístupu použije rozsahy hello nakonfigurované pro každý pravidla ověřování žádosti a token vystavování tooevaluate. 
2. Nakonfigurovat použití hello uvnitř corpnet deklarace identity, tuto možnost lze použít s federované adresáře pomocí služby AD FS. toolearn Další informace o hello uvnitř corpnet deklarace identity, najdete v části [IP adresy Tusted](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="rules-based-on-application-sensitivity"></a>Pravidla založená na aplikace velkých a malých písmen
Pravidla jsou konfigurována na povolení hello vysoké hodnoty služby toobe zabezpečené bez dopadu na služby tooother přístup k aplikaci. Pravidla podmíněného přístupu se dá konfigurovat na hello **konfigurace** karta aplikace hello. 

Pravidla aktuálně nabízí:

* **Vyžadovat vícefaktorové ověřování**
  
  * Všichni uživatelé, je tato zásada použitá toowill být požadované tooauthenticate prostřednictvím služby Multi-Factor authentication alespoň jednou.
* **Vyžadovat vícefaktorové ověřování, když není v práci**
  
  * Pokud je tato zásada použitá, všichni uživatelé se budou požadované toohave provést službu Multi-Factor authentication alespoň jednou, pokud budou přistupovat k hello služby ze vzdáleného umístění mimo pracovní. Pokud se přesouvají z pracovního tooremote umístění, budou požadované tooperform vícefaktorového ověřování při přístupu k hello služby.
* **Blokování přístupu, když není v práci** 
  
  * Když se uživatelé přesunou z pracovní tooa vzdáleného umístění, pokud hello "Zablokovat přístup, když není v práci" zásady použité toothem že budou Blokovaní.  Je znovu dovoleno přístup v pracovního umístění.

## <a name="related-topics"></a>Související témata
* [Zabezpečení přístupu tooOffice 365 a další aplikace připojeným tooAzure služby Active Directory](active-directory-conditional-access.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

