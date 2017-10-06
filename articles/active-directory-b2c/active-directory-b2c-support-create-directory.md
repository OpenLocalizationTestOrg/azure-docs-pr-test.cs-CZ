---
title: "Azure Active Directory B2C: Řešení potíží s vytváření klienty | Microsoft Docs"
description: "Problémy a řešení pro vytvoření klienta služby Azure Active Directory nebo Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 7ba4c6b2-161b-45b5-b3bd-ccb662f5d7a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 4bb7ec6ec67d3368a0e36c098f290f582510714a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Řešení potíží s vytváření klienta služby Azure Active Directory nebo Azure Active Directory B2C 

## <a name="create-an-azure-ad-tenant"></a>Vytvoření klienta Azure AD
Pokud na první pokus o hello nelze vytvořit klienta služby Azure Active Directory (Azure AD), zkuste to znovu. Pokud hello potíže potrvají, kontaktujte podporu Azure.

## <a name="create-an-azure-ad-b2c-tenant"></a>Vytvoření tenanta Azure AD B2C
Pokud narazíte na potíže při jste [vytvoření Azure Active Directory B2C klienta (Azure AD B2C)](active-directory-b2c-get-started.md), zkuste hello následující možnosti:

* Pokud hello Azure AD B2C klienta není zobrazena v seznamu klienty, zkuste to znovu toocreate hello klienta.
* Pokud klienta hello Azure AD B2C objeví v seznamu klientů a zobrazí následující chybová zpráva hello, odstraňte hello klienta a vytvořit znovu:

    "Nelze dokončit vytvoření hello klienta B2C hello 'contosob2c'. Navštivte toto [odkaz](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) další pokyny. "
* Existují známé problémy při odstranění existující Azure AD B2C klienta a znovu vytvořit pomocí hello stejným názvem domény. Při vytváření nového klienta Azure AD B2C, musíte použít jiný název domény.
* Pokud tato řešení nefungují, kontaktujte podporu Azure. Další informace najdete v tématu [soubor žádosti o podporu pro Azure AD B2C](active-directory-b2c-support.md).

