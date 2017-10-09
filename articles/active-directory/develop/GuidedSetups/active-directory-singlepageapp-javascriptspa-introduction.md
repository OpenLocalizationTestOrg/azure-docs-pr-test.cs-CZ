---
title: "aaaAzure AD v2 JS SPA na základě nastavení – Úvod | Microsoft Docs"
description: "Jak může aplikace JavaScript SPA volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 7e4250ccca837a17489a99603da167009cc2e3d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Volání hello Microsoft Graph API z JavaScript jedné stránky aplikace (SPA)

Tato příručka ukazuje, jak můžete JavaScript aplikace pro jednu stránku (SPA) přihlásit osobní pracovní a školní účty, získání přístupového tokenu a volání hello Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z hello koncový bod v2 Azure Active Directory.

### <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje hello ukázkové aplikace generované tímto průvodcem](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Další informace

Hello ukázkové aplikace vytvořené v této příručce umožňuje JavaScript SPA tooquery hello Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory. V tomto scénáři poté, co uživatel přihlásí, je token přístupu tooHTTP požadovaný a přidání požadavků prostřednictvím hello autorizační hlavičky. Získání tokenu a obnovení, jsou zpracovávány hello Microsoft ověřování knihovny (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Knihovny

Tato příručka používá hello následující knihovny:

|Knihovna|Popis|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Knihovna Microsoft ověřování pro JavaScript Preview|

> [!NOTE]
> *msal.js* cíle hello *koncový bod služby Azure Active Directory v2* – což umožňuje toosign osobní, školní i pracovní účty v a získávat tokeny. Hello *koncový bod služby Azure Active Directory v2* má [určitá omezení](..\active-directory-v2-limitations.md). Pokud vás zajímá jenom pracovní a školní účty, použijte *adal.js* a hello *koncový bod V1*. toounderstand rozdíly mezi koncovými body v1 a v2 hello číst hello [v1-v2 porovnání](..\active-directory-v2-compare.md).

<!--end-collapse-->
