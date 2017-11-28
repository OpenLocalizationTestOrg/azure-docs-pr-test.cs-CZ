---
title: "Azure Active Directory B2C: Řešení potíží s vytváření klienty | Microsoft Docs"
description: "Problémy a řešení pro vytvoření klienta služby Azure Active Directory nebo Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 7ba4c6b2-161b-45b5-b3bd-ccb662f5d7a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 4bb7ec6ec67d3368a0e36c098f290f582510714a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a><span data-ttu-id="bbe29-103">Řešení potíží s vytváření klienta služby Azure Active Directory nebo Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="bbe29-103">Troubleshoot creating an Azure Active Directory or Azure Active Directory B2C tenant</span></span> 

## <a name="create-an-azure-ad-tenant"></a><span data-ttu-id="bbe29-104">Vytvoření klienta Azure AD</span><span class="sxs-lookup"><span data-stu-id="bbe29-104">Create an Azure AD tenant</span></span>
<span data-ttu-id="bbe29-105">Pokud na první pokus o hello nelze vytvořit klienta služby Azure Active Directory (Azure AD), zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="bbe29-105">If you can't create an Azure Active Directory (Azure AD) tenant on hello first attempt, try again.</span></span> <span data-ttu-id="bbe29-106">Pokud hello potíže potrvají, kontaktujte podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe29-106">If hello problem persists, contact Azure Support.</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="bbe29-107">Vytvoření tenanta Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="bbe29-107">Create an Azure AD B2C tenant</span></span>
<span data-ttu-id="bbe29-108">Pokud narazíte na potíže při jste [vytvoření Azure Active Directory B2C klienta (Azure AD B2C)](active-directory-b2c-get-started.md), zkuste hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="bbe29-108">If you encounter issues when you [create an Azure Active Directory B2C (Azure AD B2C) tenant](active-directory-b2c-get-started.md), try hello following options:</span></span>

* <span data-ttu-id="bbe29-109">Pokud hello Azure AD B2C klienta není zobrazena v seznamu klienty, zkuste to znovu toocreate hello klienta.</span><span class="sxs-lookup"><span data-stu-id="bbe29-109">If hello Azure AD B2C tenant doesn't show up in your list of tenants, try again toocreate hello tenant.</span></span>
* <span data-ttu-id="bbe29-110">Pokud klienta hello Azure AD B2C objeví v seznamu klientů a zobrazí následující chybová zpráva hello, odstraňte hello klienta a vytvořit znovu:</span><span class="sxs-lookup"><span data-stu-id="bbe29-110">If hello Azure AD B2C tenant does show up in your list of tenants and you see hello following  error message, delete hello tenant and create it again:</span></span>

    <span data-ttu-id="bbe29-111">"Nelze dokončit vytvoření hello klienta B2C hello 'contosob2c'.</span><span class="sxs-lookup"><span data-stu-id="bbe29-111">"Could not complete hello creation of hello B2C tenant 'contosob2c'.</span></span> <span data-ttu-id="bbe29-112">Navštivte toto [odkaz](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) další pokyny. "</span><span class="sxs-lookup"><span data-stu-id="bbe29-112">Please visit this [link](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) for more guidance."</span></span>
* <span data-ttu-id="bbe29-113">Existují známé problémy při odstranění existující Azure AD B2C klienta a znovu vytvořit pomocí hello stejným názvem domény.</span><span class="sxs-lookup"><span data-stu-id="bbe29-113">There are known issues when you delete an existing Azure AD B2C tenant and re-create it by using hello same domain name.</span></span> <span data-ttu-id="bbe29-114">Při vytváření nového klienta Azure AD B2C, musíte použít jiný název domény.</span><span class="sxs-lookup"><span data-stu-id="bbe29-114">When you create a new Azure AD B2C tenant, you must use a different domain name.</span></span>
* <span data-ttu-id="bbe29-115">Pokud tato řešení nefungují, kontaktujte podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe29-115">If these resolutions don't work, contact Azure Support.</span></span> <span data-ttu-id="bbe29-116">Další informace najdete v tématu [soubor žádosti o podporu pro Azure AD B2C](active-directory-b2c-support.md).</span><span class="sxs-lookup"><span data-stu-id="bbe29-116">For more information, see [File support requests for Azure AD B2C](active-directory-b2c-support.md).</span></span>

