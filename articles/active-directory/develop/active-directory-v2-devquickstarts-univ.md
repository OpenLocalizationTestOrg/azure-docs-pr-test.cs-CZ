---
title: "aaaAzure AD v2.0 univerzální aplikace pro Windows | Microsoft Docs"
description: "Jak toobuild aplikaci pro univerzální pro Windows, přihlášení uživatelů s Account Microsoft i osobní a pracovní nebo školní účty."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: d2c92b65-3c1d-46d1-81c8-88f32f6b2c4b
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.date: 02/20/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 49b26c74fa5a76664c3229256c9bd128563b830c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-windows-universal-app-using-hello-v20-endpoint"></a><span data-ttu-id="b2e3f-103">Přidat aplikaci univerzální pro Windows tooa přihlášení pomocí koncového bodu v2.0 hello</span><span class="sxs-lookup"><span data-stu-id="b2e3f-103">Add sign-in tooa Windows Universal app using hello v2.0 endpoint</span></span>
  <span data-ttu-id="b2e3f-104">Hello úvodní kurz pro univerzálních aplikací pro Windows není poměrně připraven... Vraťte se sem brzy & vyhledejte aktualizace z @AzureAD na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="b2e3f-104">hello quick-start tutorial for Windows Universal apps isn't quite ready... Check back soon & look for updates from @AzureAD on Twitter.</span></span>

> [!NOTE]
> <span data-ttu-id="b2e3f-105">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="b2e3f-105">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="b2e3f-106">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="b2e3f-106">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

    ## Get security updates for our products

<span data-ttu-id="b2e3f-107">Doporučujeme vám tooget oznámení o bezpečnostních incidentech navštivte stránky [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.</span><span class="sxs-lookup"><span data-stu-id="b2e3f-107">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

