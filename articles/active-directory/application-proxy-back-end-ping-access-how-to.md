---
title: aaaHow tooconfigure toouse aplikace Proxy aplikace PingAccess | Microsoft Docs
description: "Zjistěte, jak toouse PingAccess tooextend hello výhod tooapplications Proxy aplikací pomocí ověřování založeného na záhlaví"
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
ms.openlocfilehash: fa4c9747b7bf5a135425be270e4f7eadf95248fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application-toouse-pingaccess"></a><span data-ttu-id="e456b-103">Jak tooconfigure toouse aplikace Proxy aplikace PingAccess</span><span class="sxs-lookup"><span data-stu-id="e456b-103">How tooconfigure an Application Proxy application toouse PingAccess</span></span>

<span data-ttu-id="e456b-104">Naše spolupráce s PingAccess teď umožňuje tooextend hello výhod tooapplications Proxy aplikací pomocí ověřování založeného na záhlaví.</span><span class="sxs-lookup"><span data-stu-id="e456b-104">Our collaboration with PingAccess now allows you tooextend hello benefits of Application Proxy tooapplications using header-based authentication.</span></span> <span data-ttu-id="e456b-105">Pokud vaše aplikace nepoužívají hlavičky, přečtěte si naše [jednotné přihlašování v dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) podrobné informace o dalších možnostech.</span><span class="sxs-lookup"><span data-stu-id="e456b-105">If your applications do not use headers, see our [Single Sign-On documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) for details on other options.</span></span>

## <a name="overview-of-steps-and-recommended-documents"></a><span data-ttu-id="e456b-106">Přehled kroků a doporučené dokumenty</span><span class="sxs-lookup"><span data-stu-id="e456b-106">Overview of steps and recommended documents</span></span>

<span data-ttu-id="e456b-107">tooconfigure aplikaci s PingAccess, existují čtyři kroky:</span><span class="sxs-lookup"><span data-stu-id="e456b-107">tooconfigure an application with PingAccess, there are four steps:</span></span>

1.  <span data-ttu-id="e456b-108">Nakonfigurujte konektory Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="e456b-108">Configure Application Proxy Connectors</span></span>

2.  <span data-ttu-id="e456b-109">Vytvoření aplikace Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="e456b-109">Create an Azure AD Application Proxy Application</span></span>

3.  <span data-ttu-id="e456b-110">& Stáhnout konfigurace PingAccess</span><span class="sxs-lookup"><span data-stu-id="e456b-110">Download & Configure PingAccess</span></span>

4.  <span data-ttu-id="e456b-111">Konfigurace aplikací v PingAccess</span><span class="sxs-lookup"><span data-stu-id="e456b-111">Configure Applications in PingAccess</span></span>

<span data-ttu-id="e456b-112">Podrobnosti o jednotlivých kroků v tématu naše [jednotné přihlašování s hlavičky dokumentaci](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).</span><span class="sxs-lookup"><span data-stu-id="e456b-112">For details on each of these steps, see our [Single Sign-On with Headers documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).</span></span>
