---
title: aaaHow aplikace souhlasu funguje | Microsoft Docs
description: "Další informace o fungování toosee hello Azure AD souhlasu framework jak ji můžete využít při vývoji aplikace na Azure AD"
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
ms.openlocfilehash: 1b090c0c8d49320a012d8a06b7e9d35134a3ab43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-application-consent-works"></a><span data-ttu-id="a02cc-103">Jak aplikace souhlas funguje</span><span class="sxs-lookup"><span data-stu-id="a02cc-103">How application consent works</span></span>

<span data-ttu-id="a02cc-104">Tento článek je toohelp Další informace o fungování framework souhlasu hello Azure AD, můžete vyvíjet aplikace efektivněji.</span><span class="sxs-lookup"><span data-stu-id="a02cc-104">This article is toohelp you learn more about how hello Azure AD consent framework works so you can develop applications more effectively.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="a02cc-105">Doporučené dokumenty</span><span class="sxs-lookup"><span data-stu-id="a02cc-105">Recommended documents</span></span>

- <span data-ttu-id="a02cc-106">Získat obecné znalosti o [jak souhlasu umožňuje toogovern vlastníka prostředku tooresources přístup aplikaci](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#consent).</span><span class="sxs-lookup"><span data-stu-id="a02cc-106">Get a general understanding of [how consent allows a resource owner toogovern an application's access tooresources](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#consent).</span></span>
- <span data-ttu-id="a02cc-107">Získat podrobný přehled o [jak framework souhlasu hello Azure AD implementuje souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework).</span><span class="sxs-lookup"><span data-stu-id="a02cc-107">Get a step-by-step overview of [how hello Azure AD consent framework implements consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework).</span></span>
- <span data-ttu-id="a02cc-108">Pro další hloubka Další [jak víceklientské aplikace můžete použít architekturu souhlasu hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) tooimplement "user" a "admin" souhlasu, podpora pokročilejší vzory vícevrstvé aplikace.</span><span class="sxs-lookup"><span data-stu-id="a02cc-108">For more depth, learn [how a multi-tenant application can use hello consent framework](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) tooimplement "user" and "admin" consent, supporting more advanced multi-tier application patterns.</span></span>
- <span data-ttu-id="a02cc-109">Pro další hloubka Další [jak souhlasu je podporovaná v vrstvy protokolu OAuth 2.0 hello během tok udělení autorizačního kódu hello.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)</span><span class="sxs-lookup"><span data-stu-id="a02cc-109">For more depth, learn [how consent is supported at hello OAuth 2.0 protocol layer during hello authorization code grant flow.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)</span></span>

## <a name="next-steps"></a><span data-ttu-id="a02cc-110">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a02cc-110">Next steps</span></span>
[<span data-ttu-id="a02cc-111">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="a02cc-111">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
