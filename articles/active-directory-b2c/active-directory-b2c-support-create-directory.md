---
title: "Azure Active Directory B2C: Řešení potíží s vytváření klienty | Microsoft Docs"
description: "Problémy a řešení pro vytvoření klienta služby Azure Active Directory nebo Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: 7ba4c6b2-161b-45b5-b3bd-ccb662f5d7a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: cfbcb9ad561320da8836086b1b8bff39f3fefb37
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a><span data-ttu-id="5a576-103">Řešení potíží s vytváření klienta služby Azure Active Directory nebo Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="5a576-103">Troubleshoot creating an Azure Active Directory or Azure Active Directory B2C tenant</span></span> 

## <a name="create-an-azure-ad-tenant"></a><span data-ttu-id="5a576-104">Vytvoření klienta Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a576-104">Create an Azure AD tenant</span></span>
<span data-ttu-id="5a576-105">Pokud při prvním pokusu nelze vytvořit klienta služby Azure Active Directory (Azure AD), zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="5a576-105">If you can't create an Azure Active Directory (Azure AD) tenant on the first attempt, try again.</span></span> <span data-ttu-id="5a576-106">Pokud potíže potrvají, kontaktujte podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="5a576-106">If the problem persists, contact Azure Support.</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="5a576-107">Vytvoření tenanta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="5a576-107">Create an Azure AD B2C tenant</span></span>
<span data-ttu-id="5a576-108">Pokud narazíte na potíže při jste [vytvoření Azure Active Directory B2C klienta (Azure AD B2C)](active-directory-b2c-get-started.md), vyzkoušejte následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="5a576-108">If you encounter issues when you [create an Azure Active Directory B2C (Azure AD B2C) tenant](active-directory-b2c-get-started.md), try the following options:</span></span>

* <span data-ttu-id="5a576-109">Pokud klienta Azure AD B2C není zobrazena v seznamu klienty, zkuste to znovu vytvořit klienta.</span><span class="sxs-lookup"><span data-stu-id="5a576-109">If the Azure AD B2C tenant doesn't show up in your list of tenants, try again to create the tenant.</span></span>
* <span data-ttu-id="5a576-110">Pokud klienta Azure AD B2C objeví v seznamu klientů a zobrazí se následující chybová zpráva, odstraňte klienta a vytvořit znovu:</span><span class="sxs-lookup"><span data-stu-id="5a576-110">If the Azure AD B2C tenant does show up in your list of tenants and you see the following  error message, delete the tenant and create it again:</span></span>

    <span data-ttu-id="5a576-111">"Nelze dokončit vytvoření klienta B2C 'contosob2c'.</span><span class="sxs-lookup"><span data-stu-id="5a576-111">"Could not complete the creation of the B2C tenant 'contosob2c'.</span></span> <span data-ttu-id="5a576-112">Navštivte toto [odkaz](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) další pokyny. "</span><span class="sxs-lookup"><span data-stu-id="5a576-112">Please visit this [link](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) for more guidance."</span></span>
* <span data-ttu-id="5a576-113">Existují známé problémy při odstranění existujícího klienta Azure AD B2C a znovu vytvořit pomocí stejného názvu domény.</span><span class="sxs-lookup"><span data-stu-id="5a576-113">There are known issues when you delete an existing Azure AD B2C tenant and re-create it by using the same domain name.</span></span> <span data-ttu-id="5a576-114">Při vytváření nového klienta Azure AD B2C, musíte použít jiný název domény.</span><span class="sxs-lookup"><span data-stu-id="5a576-114">When you create a new Azure AD B2C tenant, you must use a different domain name.</span></span>
* <span data-ttu-id="5a576-115">Pokud tato řešení nefungují, kontaktujte podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="5a576-115">If these resolutions don't work, contact Azure Support.</span></span> <span data-ttu-id="5a576-116">Další informace najdete v tématu [soubor žádosti o podporu pro Azure AD B2C](active-directory-b2c-support.md).</span><span class="sxs-lookup"><span data-stu-id="5a576-116">For more information, see [File support requests for Azure AD B2C](active-directory-b2c-support.md).</span></span>

