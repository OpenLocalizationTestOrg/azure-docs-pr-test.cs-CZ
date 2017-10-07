---
title: "aaaFind se kdy konkrétního uživatele budou mít tooaccess aplikace | Microsoft Docs"
description: "Jak toofind se při zásadní význam uživatel být schopný tooaccess aplikaci jste nakonfigurovali pro zřizování uživatelů s Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: bb9520499dcc8bbbe6fae05c5238c8852815ea0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-tooaccess-an-application"></a>Zjistit, kdy konkrétní uživatel nebude moct tooaccess aplikace
Při použití zřizování automatické uživatelů s aplikací, Azure AD automaticky zřizovat a aktualizace uživatelské účty v aplikaci na základě třeba [přiřazení uživatelů a skupin](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) v pravidelném plánovaném časovém intervalu, obvykle každých 10 minut.

## <a name="how-long-does-it-take"></a>Jak dlouho to trvá?

Hello doba potřebná pro daného uživatele toobe, zřízený závisí hlavně na tom, jestli počáteční synchronizaci "úplná" již došlo k.

Hello první synchronizace mezi službou Azure AD a aplikace, může trvat od 20 minut tooseveral hodin, v závislosti na velikosti hello directory hello Azure AD a hello počet uživatelů v oboru pro zřizování. 

Následné synchronizace po počáteční synchronizaci hello být rychlejší (např. do 10 minut), jako hello zřizování služby ukládá vodoznaky, které představují hello stav obou systémů po počáteční synchronizaci hello, zvýšení výkonu následné synchronizace.

## <a name="how-toocheck-hello-status-of-a-user"></a>Jak toocheck hello stav uživatele

Stav zřizování toosee hello pro vybraného uživatele, poraďte se se hello protokoly auditu ve službě Azure AD.

Hello zřizování protokoly auditu je přístupná v hello portál Azure, v hello **Azure Active Directory &gt; podnikové aplikace &gt; \[název aplikace\] &gt; protokolech auditování**kartě. Filtr hello přihlásí hello **zřizování účtu** kategorie tooonly najdete v části hello zřizování události pro tuto aplikaci. Můžete vyhledat uživatele podle hello "odpovídající ID" nakonfigurovaný pro ně v mapování atributů hello. 

Například, pokud jste nakonfigurovali hello "hlavní název uživatele" nebo "e-mailovou adresu" jako hello odpovídající atribut na straně hello Azure AD, a tím zřizování uživatelů hello má hodnotu "audrey@contoso.com", pak protokoly auditu hello vyhledávání pro"audrey@contoso.com" a pak zkontrolujte Vrátí položky.

Hello zřizování auditu protokoly záznam všech hello operace provedené hello zřizování služby, včetně:

* Dotazování Azure AD pro přiřazené uživatele, které jsou v oboru pro zřizování
* Dotazování hello cílové aplikace hello existence tyto uživatele
* Porovnávání objektů uživatele hello mezi hello systému
* Přidání, aktualizace nebo deaktivace hello uživatelský účet v cílovém systému hello na základě porovnání hello

## <a name="next-steps"></a>Další kroky
[Automatizace zřizování uživatelů a jeho rušení tooSaaS aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning):
