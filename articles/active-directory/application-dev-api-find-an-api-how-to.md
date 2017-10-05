---
title: "Postup nalezení konkrétní API potřebné pro aplikaci zákaznických | Microsoft Docs"
description: "Jak nakonfigurovat oprávnění, potřebujete získat přístup k dané rozhraní API v vaše vlastní vyvinuté aplikaci Azure AD"
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
ms.openlocfilehash: e0c07fd030339d025894520500d2cd948d31af45
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a><span data-ttu-id="3af09-103">Postup nalezení konkrétní API potřebné pro aplikaci zákaznických</span><span class="sxs-lookup"><span data-stu-id="3af09-103">How to find a specific API needed for a custom-developed application</span></span>

<span data-ttu-id="3af09-104">Přístup k rozhraním API vyžadují konfiguraci oborů přístupu a rolí.</span><span class="sxs-lookup"><span data-stu-id="3af09-104">Access to APIs require configuration of access scopes and roles.</span></span> <span data-ttu-id="3af09-105">Pokud chcete vystavit prostředků aplikace webového rozhraní API pro klientské aplikace, budete muset nakonfigurovat obory přístupu a rolí pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3af09-105">If you want to expose your resource application web APIs to client applications, you need to configure access scopes and roles for the API.</span></span> <span data-ttu-id="3af09-106">Pokud chcete klientskou aplikaci pro přístup k webové rozhraní API, musíte nakonfigurovat oprávnění pro přístup k rozhraní API v registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="3af09-106">If you want a client application to access a web API, you need to configure permissions to access the API in the app registration.</span></span>

## <a name="configuring-a-resource-application-to-expose-web-apis"></a><span data-ttu-id="3af09-107">Konfigurace prostředků aplikace ke zveřejnění webových rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3af09-107">Configuring a resource application to expose web APIs</span></span>

<span data-ttu-id="3af09-108">Při vystavení webového rozhraní API, rozhraní API se zobrazí v **vybrat rozhraní API** při přidání oprávnění k registraci aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3af09-108">When you expose your web API, the API be displayed in the **Select an API** list when adding permissions to an app registration.</span></span> <span data-ttu-id="3af09-109">Chcete-li přidat oborů přístupu, postupujte podle kroků uvedených v [přidání oborů přístupu k vaší aplikaci prostředků](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span><span class="sxs-lookup"><span data-stu-id="3af09-109">To add access scopes, follow the steps outlined in [adding access scopes to your resource application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span></span>

## <a name="configuring-a-client-application-to-access-web-apis"></a><span data-ttu-id="3af09-110">Konfigurace klientskou aplikaci pro přístup k webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3af09-110">Configuring a client application to access web APIs</span></span>

<span data-ttu-id="3af09-111">Když přidáte oprávnění registrace aplikace, můžete **přidat přístup pomocí rozhraní API** zveřejněné webovému rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3af09-111">When you add permissions to your app registration, you can **add API access** to exposed web APIs.</span></span> <span data-ttu-id="3af09-112">Pro přístup k webové rozhraní API, postupujte podle kroků uvedených v [přidat přihlašovací údaje nebo oprávnění pro přístup k webové rozhraní API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span><span class="sxs-lookup"><span data-stu-id="3af09-112">To access web APIs, follow the steps outlined in [add credentials or permissions to access web APIs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3af09-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3af09-113">Next steps</span></span>

-   [<span data-ttu-id="3af09-114">Integrace aplikací se službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3af09-114">Integrating applications with Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [<span data-ttu-id="3af09-115">Principy manifest aplikace Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3af09-115">Understanding the Azure Active Directory application manifest</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


