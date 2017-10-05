---
title: "Potíže při přihlašování k místní aplikace pomocí proxy aplikace služby Azure AD | Microsoft Docs"
description: "Řešení běžných problémů potýkají Pokud nelze se přihlásit k aplikaci místně integrované s Azure AD pomocí Azure AD Application Proxy"
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
ms.openlocfilehash: 5687f789355cc9769d26b53e98486bb213c66419
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-on-premises-application-using-the-azure-ad-application-proxy"></a><span data-ttu-id="bc2de-103">Potíže při přihlašování k místní aplikace pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc2de-103">Problems signing in to an on-premises application using the Azure AD application proxy</span></span>

<span data-ttu-id="bc2de-104">Pokud máte potíže při přihlašování v aplikaci místně, můžete zkusit následujících kroků pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="bc2de-104">If you are having problems signing in an on-premises application, you can try following the steps below to resolving your problem.</span></span>

## <a name="i-can-load-my-application-but-something-on-the-page-looks-broken"></a><span data-ttu-id="bc2de-105">I může načíst Moje aplikace, ale něco na stránce vypadá porušený</span><span class="sxs-lookup"><span data-stu-id="bc2de-105">I can load my application, but something on the page looks broken</span></span>

<span data-ttu-id="bc2de-106">Následující dokumenty vám mohou pomoci při řešení některých nejběžnějších problémů v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="bc2de-106">The following documents can help you to resolve some of the most common issues in this category.</span></span>

  * [<span data-ttu-id="bc2de-107">Můžu se k aplikaci dostat, ale stránka aplikace se nezobrazuje správně</span><span class="sxs-lookup"><span data-stu-id="bc2de-107">I can get to my application, but the application page isn't displaying correctly</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-page-appearance-broken-problem/)
  * [<span data-ttu-id="bc2de-108">Můžu se k aplikaci dostat, ale načítá se příliš dlouho</span><span class="sxs-lookup"><span data-stu-id="bc2de-108">I can get to my application, but the application takes too long to load</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-page-load-speed-problem/)
  * [<span data-ttu-id="bc2de-109">Můžu se k aplikaci dostat, ale odkazy na stránce aplikace nefungují</span><span class="sxs-lookup"><span data-stu-id="bc2de-109">I can get to my application, but the links on the application page do not work</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-page-links-broken-problem/)

## <a name="im-having-a-connectivity-problem-my-application"></a><span data-ttu-id="bc2de-110">Došlo k potížím s připojením Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="bc2de-110">I'm having a connectivity problem my application</span></span>
  <span data-ttu-id="bc2de-111">Následující dokumenty vám mohou pomoci při řešení některých nejběžnějších problémů v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="bc2de-111">The following documents can help you to resolve some of the most common issues in this category.</span></span>
  * [<span data-ttu-id="bc2de-112">Nevím, které porty pro aplikaci otevřít</span><span class="sxs-lookup"><span data-stu-id="bc2de-112">I don't know what ports to open for my application</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-connectivity-ports-how-to/)
  * [<span data-ttu-id="bc2de-113">Vyskytl se problém, protože ve skupině konektorů pro aplikaci nebyl žádný funkční konektor</span><span class="sxs-lookup"><span data-stu-id="bc2de-113">I encountered a problem because there was no working connector in a connector group for my application</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-connectivity-no-working-connector/)

## <a name="im-having-a-problem-configuring-the-azure-ad-application-proxy-in-the-admin-portal"></a><span data-ttu-id="bc2de-114">Došlo k potížím konfigurace Azure AD Application Proxy v portálu pro správu</span><span class="sxs-lookup"><span data-stu-id="bc2de-114">I'm having a problem configuring the Azure AD Application Proxy in the admin portal</span></span>
  <span data-ttu-id="bc2de-115">Následující dokumenty vám mohou pomoci při řešení některých nejběžnějších problémů v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="bc2de-115">The following documents can help you to resolve some of the most common issues in this category.</span></span>
  * [<span data-ttu-id="bc2de-116">Mám potíže s konfigurací aplikace aplikačního proxy serveru</span><span class="sxs-lookup"><span data-stu-id="bc2de-116">I am having difficulty configuring an application Proxy application</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-config-how-to/)
  * [<span data-ttu-id="bc2de-117">Nevím, jak nakonfigurovat jednotné přihlašování k aplikaci aplikačního proxy serveru</span><span class="sxs-lookup"><span data-stu-id="bc2de-117">I don't know how to configure single sign-on to my application Proxy application</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-config-sso-how-to/)
  * [<span data-ttu-id="bc2de-118">Vyskytl se problém při vytváření aplikace na portálu pro správu</span><span class="sxs-lookup"><span data-stu-id="bc2de-118">I encountered a problem when creating my application in admin portal</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-config-problem/)

## <a name="im-having-a-problem-setting-up-back-end-authentication-to-my-application"></a><span data-ttu-id="bc2de-119">Došlo problému s nastavením ověřování back-end pro Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="bc2de-119">I'm having a problem setting up back-end authentication to my application</span></span>
  <span data-ttu-id="bc2de-120">Následující dokumenty vám mohou pomoci při řešení některých nejběžnějších problémů v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="bc2de-120">The following documents can help you to resolve some of the most common issues in this category.</span></span>
  * [<span data-ttu-id="bc2de-121">Nevím, jak nakonfigurovat omezené delegování Kerberos</span><span class="sxs-lookup"><span data-stu-id="bc2de-121">I don't know how to configure Kerberos Constrained Delegation</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-back-end-kerberos-constrained-delegation-how-to/)
  * [<span data-ttu-id="bc2de-122">Nevím, jak nakonfigurovat aplikaci pro používání PingAccessu</span><span class="sxs-lookup"><span data-stu-id="bc2de-122">I don't know how to configure my application with PingAccess</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-back-end-ping-access-how-to/)

## <a name="im-having-a-problem-when-signing-in-to-my-application"></a><span data-ttu-id="bc2de-123">Došlo potížím při přihlašování k Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="bc2de-123">I'm having a problem when signing in to my application</span></span>
  <span data-ttu-id="bc2de-124">Následující dokumenty vám mohou pomoci při řešení některých nejběžnějších problémů v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="bc2de-124">The following documents can help you to resolve some of the most common issues in this category.</span></span>
  * [<span data-ttu-id="bc2de-125">Zobrazuje se chyba „Není přístup k podnikové aplikaci“</span><span class="sxs-lookup"><span data-stu-id="bc2de-125">I get a "Can't Access this Corporate Application" error</span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-sign-in-bad-gateway-timeout-error/)

## <a name="im-having-a-problem-with-the-application-proxy-agent-connector"></a><span data-ttu-id="bc2de-126">Došlo k potížím s agenta konektor Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="bc2de-126">I'm having a problem with the Application Proxy Agent Connector</span></span>
  <span data-ttu-id="bc2de-127">Následující dokumenty vám mohou pomoci při řešení některých nejběžnějších problémů v této kategorii.</span><span class="sxs-lookup"><span data-stu-id="bc2de-127">The following documents can help you to resolve some of the most common issues in this category.</span></span>
  * [<span data-ttu-id="bc2de-128">Vyskytují se potíže při instalaci konektoru agenta aplikačního proxy serveru</span><span class="sxs-lookup"><span data-stu-id="bc2de-128">I am having issues installing the Application Proxy Agent Connector </span></span>](https://docs.microsoft.com/azure/active-directory/application-proxy-connector-installation-problem/)

## <a name="next-steps"></a><span data-ttu-id="bc2de-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bc2de-129">Next steps</span></span>
[<span data-ttu-id="bc2de-130">Jak poskytnout zabezpečený vzdálený přístup k místním aplikacím</span><span class="sxs-lookup"><span data-stu-id="bc2de-130">How to provide secure remote access to on-premises applications</span></span>](active-directory-application-proxy-get-started.md)
