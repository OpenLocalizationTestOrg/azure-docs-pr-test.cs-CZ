---
title: "Technické informace o Azure Active Directory podmíněného přístupu | Microsoft Docs"
description: "Azure Active Directory pomocí podmíněného řízení přístupu, zkontroluje konkrétní podmínky, kterou vyberete při ověřování uživatele a před povolením přístupu k aplikaci. Po splnění těchto podmínek je uživatel ověřený a přistupovat k aplikaci."
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
ms.openlocfilehash: ca16a5399f94fd1ab267e0798cade3fd83f75b13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Technické informace o Azure Active Directory podmíněného přístupu

## <a name="services-enabled-with-conditional-access"></a>Služba povolena s podmíněným přístupem

Pravidla podmíněného přístupu jsou podporovány v rámci různých typů aplikací Azure AD. Tento seznam obsahuje:


* Aplikace zaregistrované pomocí Proxy aplikace Azure
* Vzdálené aplikace Azure
* Vyvinuté řádek obchodní a víceklientské aplikace zaregistrované s Azure AD
* Dynamics CRM
* Federované aplikace v galerii aplikací Azure AD
* Aplikace Microsoft Office 365 Yammer
* Aplikace Microsoft Office 365 Exchange Online
* Aplikace Microsoft Office 365 SharePoint Online (zahrnuje Onedrivu pro firmy)
* Microsoft Power BI 
* Heslo aplikace jednotného přihlašování z galerii aplikací Azure AD
* Visual Studio Team Services
* Microsoft Teams









## <a name="enable-access-rules"></a>Povolení pravidla přístupu
Každé pravidlo můžete povolit nebo zakázat v na základů aplikace. Pokud jsou pravidla **ON** bude možné povolit a vynutit pro uživatele, kteří používají aplikaci. Když jsou **OFF** se nebude používat a nebude mít vliv na přihlašování uživatelů.

## <a name="applying-rules-to-specific-users"></a>Použití pravidel pro konkrétní uživatele
Pravidla lze použít pro konkrétní skupiny uživatelů podle skupiny zabezpečení nastavením **použít na**. **Použít na** může být nastaven na **všichni uživatelé** nebo **skupiny**. Pokud nastavíte hodnotu **všichni uživatelé** pravidla budou platit pro všechny uživatele s přístupem k aplikaci. **Skupiny** možnost umožňuje konkrétní zabezpečení a distribučních skupin být vybrána, pravidla se vynucují jenom pro tyto skupiny.

Pokud nasazujete pravidlo, je běžné nejprve provést omezené uživatelů, které jsou členy skupin pilotní. Po dokončení pravidlo můžete použít k **všichni uživatelé**. To způsobí, že je pravidlo, která budou vynucena pro všechny uživatele v organizaci.

Vyberte skupiny může být také vyloučené z pomocí zásad **s výjimkou** možnost. Všechny členy těchto skupin bude vyloučené i v případě, že se objeví ve skupině součástí.

## <a name="at-work-networks"></a>Sítě "v práci"
Pravidla podmíněného přístupu, které používají síť "v práci" spoléhat na důvěryhodný rozsahy IP adres, které byly konfigurovány ve službě Azure AD, nebo použijte deklarace "uvnitř corpnet" ze služby AD FS. Tato pravidla zahrnují:

* Vyžadovat vícefaktorové ověřování, když není v práci
* Blokování přístupu, když není v práci

Možnosti pro zadání sítě "v práci"

1. Konfigurovat důvěryhodné rozsahy IP adres ve [stránku konfigurace služby Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Zásadu podmíněného přístupu použije k vyhodnocení pravidel nastavených rozsahů na každé žádosti o ověření a vystavování tokenů. 
2. Nakonfigurovat použití uvnitř corpnet deklarace identity, tuto možnost lze použít s federované adresáře pomocí služby AD FS. Další informace o uvnitř corpnet deklarace identity, najdete v části [IP adresy Tusted](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="rules-based-on-application-sensitivity"></a>Pravidla založená na aplikace velkých a malých písmen
Pravidla se konfigurují na aplikaci povolení služby vysoké hodnoty zabezpečený bez dopadu na přístup k jiným službám. Pravidla podmíněného přístupu můžete nakonfigurovat na **konfigurace** aplikace. 

Pravidla aktuálně nabízí:

* **Vyžadovat vícefaktorové ověřování**
  
  * Všichni uživatelé, které se aplikují tyto zásady budou muset ověřit pomocí služby Multi-Factor authentication alespoň jednou.
* **Vyžadovat vícefaktorové ověřování, když není v práci**
  
  * Pokud je tato zásada použitá, všichni uživatelé budou muset mít provést služby Multi-Factor authentication alespoň jednou, pokud jejich přístup ke službě ze vzdáleného umístění není funkční. Pokud se přesouvají z pracovní do vzdáleného umístění, budou muset provést vícefaktorového ověřování při přístupu ke službě.
* **Blokování přístupu, když není v práci** 
  
  * Když se uživatelé přesunou do vzdáleného umístění v práci, že budou Blokovaní, pokud k nim jsou použité zásady "Zablokovat přístup, když není v práci".  Je znovu dovoleno přístup v pracovního umístění.

## <a name="related-topics"></a>Související témata
* [Zabezpečení přístupu k Office 365 a jiným aplikacím připojeným ke službě Azure Active Directory](active-directory-conditional-access.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

