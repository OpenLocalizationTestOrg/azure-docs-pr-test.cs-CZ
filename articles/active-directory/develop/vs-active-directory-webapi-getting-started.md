---
title: "Začínáme s Azure AD v projektech Visual Studio WebApi | Microsoft Docs"
description: "Jak začít používat Azure Active Directory v projektech WebApi po připojení k nebo vytváření Azure AD pomocí sady Visual Studio připojené služby"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: a756316054dd3bb63f3b0a9f59621bb1345bc693
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a><span data-ttu-id="88ad5-103">Get Začínáme s Azure Active Directory a služby Visual Studio připojené (WebApi projekty)</span><span class="sxs-lookup"><span data-stu-id="88ad5-103">Get Started with Azure Active Directory and Visual Studio connected services (WebApi projects)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="88ad5-104">Začínáme</span><span class="sxs-lookup"><span data-stu-id="88ad5-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="88ad5-105">Co se přihodilo</span><span class="sxs-lookup"><span data-stu-id="88ad5-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a><span data-ttu-id="88ad5-106">Vyžádání ověření řadičům přístup</span><span class="sxs-lookup"><span data-stu-id="88ad5-106">Requiring authentication to access controllers</span></span>
<span data-ttu-id="88ad5-107">Všechny řadiče ve vašem projektu byly ozdobené s **Autorizovat** atribut.</span><span class="sxs-lookup"><span data-stu-id="88ad5-107">All controllers in your project were adorned with the **Authorize** attribute.</span></span> <span data-ttu-id="88ad5-108">Tento atribut vyžaduje, aby uživatel k ověření před přístupem k rozhraní API definované tyto řadiče.</span><span class="sxs-lookup"><span data-stu-id="88ad5-108">This attribute requires the user to be authenticated before accessing the APIs defined by these controllers.</span></span> <span data-ttu-id="88ad5-109">Chcete-li umožňují řadiči získat anonymní přístup, odeberte tento atribut z řadiče.</span><span class="sxs-lookup"><span data-stu-id="88ad5-109">To allow the controller to be accessed anonymously, remove this attribute from the controller.</span></span> <span data-ttu-id="88ad5-110">Pokud chcete nastavit oprávnění na podrobnější úrovni, použijte atribut pro každou metodu, která vyžaduje ověření, namísto aplikace do třídy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="88ad5-110">If you want to set the permissions at a more granular level, apply the attribute to each method that requires authorization instead of applying it to the controller class.</span></span>

## <a name="next-steps"></a><span data-ttu-id="88ad5-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88ad5-111">Next steps</span></span>
[<span data-ttu-id="88ad5-112">Další informace o službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="88ad5-112">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

