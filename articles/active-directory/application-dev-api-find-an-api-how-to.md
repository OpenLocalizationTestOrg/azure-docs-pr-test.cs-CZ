---
title: "aaaHow toofind konkrétní API potřebné pro aplikaci zákaznických | Microsoft Docs"
description: "Jak vyvinuté oprávnění hello tooconfigure potřebujete tooaccess dané rozhraní API ve vaší vlastní aplikaci Azure AD"
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
ms.openlocfilehash: 7331129204d8b34b4ef9671749bd702f893768ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-a-specific-api-needed-for-a-custom-developed-application"></a>Jak potřeby toofind konkrétní rozhraní API pro aplikace vyvinuté vlastní

Přístup k tooAPIs vyžadují konfiguraci oborů přístupu a rolí. Pokud chcete tooexpose prostředků aplikace webových rozhraní API tooclient aplikací, musíte tooconfigure obory přístupu a rolí pro hello rozhraní API. Pokud chcete klientské aplikace tooaccess webového rozhraní API, musíte tooconfigure oprávnění tooaccess hello API v registraci aplikace hello.

## <a name="configuring-a-resource-application-tooexpose-web-apis"></a>Konfigurace prostředků aplikace tooexpose webového rozhraní API

Při vystavení webového rozhraní API, zobrazit hello rozhraní API v hello **vybrat rozhraní API** seznamu při přidávání registrace aplikací tooan oprávnění. obory přístupu tooadd, postupujte podle kroků hello uvedených v [přidání přístup rozsahy tooyour prostředků aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-tooaccess-web-apis"></a>Konfigurace klienta aplikace tooaccess webového rozhraní API

Když přidáte registrace aplikací tooyour oprávnění, můžete **přidat přístup pomocí rozhraní API** tooexposed webových rozhraní API. tooaccess webovým rozhraním API, postupujte podle kroků hello uvedených v [přidat přihlašovací údaje nebo oprávnění tooaccess webovým rozhraním API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Další kroky

-   [Integrace aplikací se službou Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Principy hello manifest aplikace Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


