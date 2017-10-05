---
title: Jak funguje aplikace souhlasu | Microsoft Docs
description: "Další informace o tom, jak funguje rozhraní Azure AD souhlasu zobrazíte, jak ji můžete využít při vývoji aplikace na Azure AD"
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
ms.openlocfilehash: 5abddf3a8698e3eb39f118f54eeac62ce68fed39
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-application-consent-works"></a><span data-ttu-id="61673-103">Jak aplikace souhlas funguje</span><span class="sxs-lookup"><span data-stu-id="61673-103">How application consent works</span></span>

<span data-ttu-id="61673-104">Tento článek je můžete získat další informace o fungování souhlasu rozhraní Azure AD, můžete vyvíjet aplikace efektivněji.</span><span class="sxs-lookup"><span data-stu-id="61673-104">This article is to help you learn more about how the Azure AD consent framework works so you can develop applications more effectively.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="61673-105">Doporučené dokumenty</span><span class="sxs-lookup"><span data-stu-id="61673-105">Recommended documents</span></span>

- <span data-ttu-id="61673-106">Získat obecné znalosti o [jak souhlasu umožňuje vlastníkovi prostředku aplikace přístup k prostředkům řídí](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#consent).</span><span class="sxs-lookup"><span data-stu-id="61673-106">Get a general understanding of [how consent allows a resource owner to govern an application's access to resources](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#consent).</span></span>
- <span data-ttu-id="61673-107">Získat podrobný přehled o [jak rozhraní Azure AD souhlasu implementuje souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework).</span><span class="sxs-lookup"><span data-stu-id="61673-107">Get a step-by-step overview of [how the Azure AD consent framework implements consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework).</span></span>
- <span data-ttu-id="61673-108">Pro další hloubka Další [jak víceklientské aplikace můžete použít rozhraní souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) implementovat "user" a "admin" souhlasu, podpora více advanced vzory vícevrstvé aplikace.</span><span class="sxs-lookup"><span data-stu-id="61673-108">For more depth, learn [how a multi-tenant application can use the consent framework](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) to implement "user" and "admin" consent, supporting more advanced multi-tier application patterns.</span></span>
- <span data-ttu-id="61673-109">Pro další hloubka Další [jak se souhlasu podporovány ve vrstvě protokolu OAuth 2.0, během tok udělení autorizačního kódu.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)</span><span class="sxs-lookup"><span data-stu-id="61673-109">For more depth, learn [how consent is supported at the OAuth 2.0 protocol layer during the authorization code grant flow.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)</span></span>

## <a name="next-steps"></a><span data-ttu-id="61673-110">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61673-110">Next steps</span></span>
[<span data-ttu-id="61673-111">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="61673-111">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
