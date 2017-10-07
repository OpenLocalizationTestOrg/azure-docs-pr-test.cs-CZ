---
title: "aaaCharacteristics služby Azure Active Directory klienta intercaction | Microsoft Docs"
description: "Seznámení s klienty jako plně nezávislý prostředky spravovat klienty klienta Azure Active"
services: active-tenant
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-tenant
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/27/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 57b677665c7cb4aee63f518c39d26754fe71a999
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Pochopit, jak více klientů služby Azure Active Directory pracovat

V Azure Active Directory (Azure AD), každý tenanta je plně nezávislý prostředek: sdílené, které je logicky nezávislý hello ostatních klientů, které spravujete. Není žádný vztah nadřazený podřízený mezi klienty. Tato nezávislost mezi klienty zahrnuje nezávislost prostředků, nezávislost správy a nezávislost synchronizace.

## <a name="resource-independence"></a>Nezávislost prostředků
* Pokud vytvoříte nebo odstranění prostředku v jednoho klienta, nemá žádný vliv na prostředky v jiného klienta, s výjimkou hello částečné externí uživatele. 
* Pokud použijete jeden názvů domén pomocí jednoho klienta, nelze použít s žádným jiným klientem.

## <a name="administrative-independence"></a>Nezávislost správy
Pokud uživatele bez oprávnění správce klienta "Contoso" vytvoří testovacím klientem "Test", pak:

* Ve výchozím nastavení je hello uživatel, který vytvoří klienta přidat jako externí uživatel v nového klienta a roli globálního správce přiřazené hello v něm.
* Správci Hello klienta "Contoso" nemají žádná přímá oprávnění správce tootenant "Test", pokud správce "Test" explicitně neudělí těchto oprávnění. Však správci "Contoso" můžou řídit přístup tootenant "Test" Pokud se řídit hello uživatelský účet vytvořený Test.
* Pokud můžete přidat nebo odebrat roli správce u uživatele v jednoho klienta, změna hello nemá ovlivňují role správce hello této hello má uživatel v jiného klienta.

## <a name="synchronization-independence"></a>Nezávislost synchronizace
Můžete nakonfigurovat každý Azure AD klienta nezávisle na nástroji tooget data synchronizovala z jediné instance buď:

* nástroji Hello Azure AD Connect, toosynchronize dat s jednou doménovou strukturou AD.
* Hello klienta Azure Active konektor pro nástroj Forefront Identity Manager toosynchronize dat s jednou nebo více místními doménovými strukturami a/nebo zdroje dat mimo Azure AD.

## <a name="add-an-azure-ad-tenant"></a>Přidat klienta Azure AD
tooadd klient služby Azure AD v hello portál Azure, přihlaste se příliš[hello portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce Azure AD a na levé straně hello, vyberte **nový**.

> [!NOTE]
> Na rozdíl od jiných prostředků Azure klienti nejsou podřízené prostředky předplatného Azure. Pokud vaše předplatné Azure je zrušená nebo vypršela platnost, můžete pořád přístup k datům klienta pomocí prostředí Azure PowerShell, hello Azure Graph API nebo Centrum pro správu hello Office 365. Také můžete přidružit jiné předplatné s klientem hello.
>

## <a name="next-steps"></a>Další kroky
Komplexní přehled o problémy s licencí Azure AD a osvědčené postupy, najdete v části [co je Azure Active klienta licencování?](active-directory-licensing-whatis-azure-portal.md)
