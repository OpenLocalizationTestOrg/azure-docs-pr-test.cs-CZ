---
title: "aaaWhat je na základě skupin licencování v Azure Active Directory? | Dokumentace Microsoftu"
description: "Popis služby Azure Active Directory na základě skupiny licencí, jak to funguje a doporučené postupy"
services: active-directory
keywords: "Licencování Azure AD"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/29/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 11647de6b76022cd2393751fcafc67ce671aeba6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="group-based-licensing-basics-in-azure-active-directory"></a>Na základě skupiny licencování základy v Azure Active Directory

Použití Microsoft placené cloudové služby, jako je například Office 365, Enterprise Mobility + Security, Dynamics CRM a další podobné produkty, vyžaduje licence. Tyto licence jsou přiřazeny tooeach uživatele, který potřebuje přístup toothese služby. licence toomanage správci pomocí jedné z portály pro správu hello (Office nebo Azure) a rutin prostředí PowerShell. Azure Active Directory (Azure AD) je hello základní infrastruktura, která podporuje správu identit pro všechny cloudové služby Microsoftu. Azure AD ukládá informace o stavu přiřazení licence pro uživatele.

Dosud licence lze přiřadit pouze na úrovni jednotlivých uživatelských hello, což mohou ztížit správu ve velkém měřítku. Například tooadd nebo odeberte uživatelských licencí podle změn v organizaci, jako jsou uživatelé propojení nebo bez hello organizace nebo oddělení, správce často musíte napsat skript prostředí PowerShell komplexní. Tento skript volá jednotlivých toohello cloudové služby.

tooaddress ty vyzve, Azure AD nyní zahrnuje, na základě skupin licencí. Můžete přiřadit jednu nebo více skupině tooa licencí produktu. Azure AD zajistí, že licence hello přiřazené tooall členy skupiny hello. Všechny nové členy, kteří připojit hello skupině jsou přiřazeni hello příslušné licence. Po skončení hello skupiny, tyto licence budou odebrány. Tím se eliminuje potřeba hello pro automatizaci správy licencí pomocí prostředí PowerShell tooreflect změny v organizaci hello a oddělení strukturu na jednotlivé uživatele.

## <a name="features"></a>Funkce

Zde jsou hlavní funkce hello na základě skupiny licencí:

- Licence můžete přiřadit tooany skupiny zabezpečení ve službě Azure AD. Skupiny zabezpečení, může být synchronizovaná místně pomocí Azure AD Connect. Skupiny zabezpečení můžete taky vytvořit přímo ve službě Azure AD (také nazývané skupiny jenom pro cloud) nebo automaticky pomocí funkce Dynamická skupina hello Azure AD.

- Je-li licenci produktu přiřazen tooa skupiny, můžete zakázat hello správce jeden nebo více plánů služby v produktu hello. Obvykle to se provádí při hello organizace ještě není připravená toostart pomocí služby zahrnuté v produktu. Například hello správce může přiřadit oddělení tooa Office 365, ale dočasně zakázat službu Yammer hello.

- Jsou podporovány všem cloudovým službám Microsoftu, které vyžadují individuální licencování. To zahrnuje všechny produkty, Enterprise Mobility + Security a Dynamics CRM v Office 365.

- Na základě skupiny licencí je aktuálně k dispozici pouze prostřednictvím [hello portál Azure](https://portal.azure.com). Pokud používáte jiné portály pro správu především pro uživatele a skupiny správy, jako je například portál hello Office 365, můžete tak dál toodo. Ale byste měli používat licence Azure portálu toomanage hello na úrovni skupiny.

- Azure AD automaticky spravuje úpravy licencí, které jsou výsledkem změn členství ve skupinách. Změny licencí jsou obvykle efektivní minut změny členství.

- Uživatel může být členem více skupin se zásadami licencí zadaná. Uživatel může mít také některé licence, které byly přímo přiřazeny, mimo žádné skupiny. Hello výsledná stav uživatele je kombinací všech přiřazené produktu a licence služby.

- V některých případech nelze licence přiřadit tooa uživatele. Například v klientovi hello nemusí být dostatek dostupné licence nebo konfliktní služby může byly přiřazeny na hello stejný čas. Správci mají přístup tooinformation o uživatelích, které Azure AD nebylo možné plně zpracovat skupiny licencí. Poté můžete začít opravné akce na základě těchto informací.

- Během verzi public preview v správy na základě skupiny licencí toouse klienta hello je vyžadován zkušební nebo placené předplatné pro edice Azure AD Basic nebo Premium.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o scénáře pro správu licencí prostřednictvím na základě skupiny licencí, najdete v části:

* [Začínáme s licencí v Azure Active Directory](active-directory-licensing-get-started-azure-portal.md)
* [Přiřazení licencí tooa skupiny ve službě Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Identifikace a řešení potíží s licencí pro skupinu v Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Jak toomigrate jednotlivé licencované uživatele toogroup základě licencování v Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory na základě skupin licencí další scénáře](active-directory-licensing-group-advanced.md)
