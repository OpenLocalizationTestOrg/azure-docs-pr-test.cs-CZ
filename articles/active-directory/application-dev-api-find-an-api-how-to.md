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
# <a name="how-toofind-a-specific-api-needed-for-a-custom-developed-application"></a><span data-ttu-id="02214-103">Jak potřeby toofind konkrétní rozhraní API pro aplikace vyvinuté vlastní</span><span class="sxs-lookup"><span data-stu-id="02214-103">How toofind a specific API needed for a custom-developed application</span></span>

<span data-ttu-id="02214-104">Přístup k tooAPIs vyžadují konfiguraci oborů přístupu a rolí.</span><span class="sxs-lookup"><span data-stu-id="02214-104">Access tooAPIs require configuration of access scopes and roles.</span></span> <span data-ttu-id="02214-105">Pokud chcete tooexpose prostředků aplikace webových rozhraní API tooclient aplikací, musíte tooconfigure obory přístupu a rolí pro hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="02214-105">If you want tooexpose your resource application web APIs tooclient applications, you need tooconfigure access scopes and roles for hello API.</span></span> <span data-ttu-id="02214-106">Pokud chcete klientské aplikace tooaccess webového rozhraní API, musíte tooconfigure oprávnění tooaccess hello API v registraci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02214-106">If you want a client application tooaccess a web API, you need tooconfigure permissions tooaccess hello API in hello app registration.</span></span>

## <a name="configuring-a-resource-application-tooexpose-web-apis"></a><span data-ttu-id="02214-107">Konfigurace prostředků aplikace tooexpose webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="02214-107">Configuring a resource application tooexpose web APIs</span></span>

<span data-ttu-id="02214-108">Při vystavení webového rozhraní API, zobrazit hello rozhraní API v hello **vybrat rozhraní API** seznamu při přidávání registrace aplikací tooan oprávnění.</span><span class="sxs-lookup"><span data-stu-id="02214-108">When you expose your web API, hello API be displayed in hello **Select an API** list when adding permissions tooan app registration.</span></span> <span data-ttu-id="02214-109">obory přístupu tooadd, postupujte podle kroků hello uvedených v [přidání přístup rozsahy tooyour prostředků aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span><span class="sxs-lookup"><span data-stu-id="02214-109">tooadd access scopes, follow hello steps outlined in [adding access scopes tooyour resource application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span></span>

## <a name="configuring-a-client-application-tooaccess-web-apis"></a><span data-ttu-id="02214-110">Konfigurace klienta aplikace tooaccess webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="02214-110">Configuring a client application tooaccess web APIs</span></span>

<span data-ttu-id="02214-111">Když přidáte registrace aplikací tooyour oprávnění, můžete **přidat přístup pomocí rozhraní API** tooexposed webových rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="02214-111">When you add permissions tooyour app registration, you can **add API access** tooexposed web APIs.</span></span> <span data-ttu-id="02214-112">tooaccess webovým rozhraním API, postupujte podle kroků hello uvedených v [přidat přihlašovací údaje nebo oprávnění tooaccess webovým rozhraním API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span><span class="sxs-lookup"><span data-stu-id="02214-112">tooaccess web APIs, follow hello steps outlined in [add credentials or permissions tooaccess web APIs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span></span>

## <a name="next-steps"></a><span data-ttu-id="02214-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02214-113">Next steps</span></span>

-   [<span data-ttu-id="02214-114">Integrace aplikací se službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="02214-114">Integrating applications with Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [<span data-ttu-id="02214-115">Principy hello manifest aplikace Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="02214-115">Understanding hello Azure Active Directory application manifest</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


