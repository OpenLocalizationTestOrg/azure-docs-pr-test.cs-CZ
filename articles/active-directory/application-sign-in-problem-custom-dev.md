---
title: "Potíže při přihlašování k aplikaci zákaznických | Microsoft Docs"
description: "Běžné rrors, které by mohlo být příčinou vám nebudou moci přihlásit k aplikaci jste si vytvořili s Azure AD"
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
ms.openlocfilehash: b0df23e040a73d18968f547eef7347f14cc577c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a><span data-ttu-id="1a0ac-103">Přihlášení k aplikaci zákaznických problémů</span><span class="sxs-lookup"><span data-stu-id="1a0ac-103">Problems signing in to an custom-developed application</span></span>

<span data-ttu-id="1a0ac-104">Existuje několik chyb, které by mohlo být příčinou vám nebudou moci přihlásit do aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a0ac-104">There are several errors that could be causing you to not be able to sign into an app.</span></span> <span data-ttu-id="1a0ac-105">Největších důvod uživatelé setkávají tento problém je špatně nakonfigurovaný. aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a0ac-105">The biggest reason people encounter this problem is misconfigured apps.</span></span>

## <a name="errors-related-to--misconfigured-apps"></a><span data-ttu-id="1a0ac-106">Chyby související s nesprávně nakonfigurované aplikace</span><span class="sxs-lookup"><span data-stu-id="1a0ac-106">Errors related to  misconfigured apps</span></span>

* <span data-ttu-id="1a0ac-107">Ověřte, že konfigurace na portálu shodovat, co máte ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1a0ac-107">Verify both the configurations in the portal match what you have in your app.</span></span> <span data-ttu-id="1a0ac-108">Konkrétně porovnejte ID klienta nebo aplikace, adresy URL odpovědí, tajné klíče nebo klíče klienta a identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a0ac-108">Specifically, compare Client/Application ID, Reply URLs, Client Secrets/Keys, and App ID URI.</span></span>

* <span data-ttu-id="1a0ac-109">Porovnání prostředků, kterou žádáte o přístup k v kódu s nakonfigurovanou oprávnění v **požadované prostředky** a zajistěte, aby vám pouze požadavky na prostředky, které jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="1a0ac-109">Compare the resource you’re requesting access to in code with the configured permissions in the **Required Resources** tab to make sure you only request resources you’ve configured.</span></span>

* <span data-ttu-id="1a0ac-110">V tématu [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) všechny podobné chyby nebo problémy.</span><span class="sxs-lookup"><span data-stu-id="1a0ac-110">See [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) for any similar errors or problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a0ac-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a0ac-111">Next steps</span></span>

[<span data-ttu-id="1a0ac-112">Příručka vývojáře pro službu Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a0ac-112">Azure AD Developer Guide</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[<span data-ttu-id="1a0ac-113">Souhlasu a integrace aplikací do Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a0ac-113">Consent and Integrating Apps to Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[<span data-ttu-id="1a0ac-114">Souhlasu a systému oprávnění rolích pro Azure AD v2.0 konvergované aplikace</span><span class="sxs-lookup"><span data-stu-id="1a0ac-114">Consent and Permissioning for Azure AD v2.0 converged Apps</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="1a0ac-115">Azure AD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="1a0ac-115">Azure AD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory>)
