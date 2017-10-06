---
title: "vytváření aplikací Proxy aplikace aaaProblem | Microsoft Docs"
description: "Jak tootroubleshoot problémy vytváření aplikace Proxy aplikace v portálu pro správu Azure AD hello"
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
ms.openlocfilehash: 24fab83c38a49a9e5754854acf2f9711e374e559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-creating-an-application-proxy-application"></a><span data-ttu-id="fdb42-103">Problém s vytvořením aplikace Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="fdb42-103">Problem creating an Application Proxy application</span></span> 

<span data-ttu-id="fdb42-104">Níže jsou uvedeny některé běžné problémy, hello uživatelé setkávají při vytváření nové aplikace proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="fdb42-104">Below are some of hello common issues people face when creating a new application proxy application.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="fdb42-105">Doporučené dokumenty</span><span class="sxs-lookup"><span data-stu-id="fdb42-105">Recommended documents</span></span> 

<span data-ttu-id="fdb42-106">toolearn Další informace o vytváření aplikací Proxy aplikací prostřednictvím hello portál pro správu, najdete v části [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="fdb42-106">toolearn more about creating an Application Proxy application through hello Admin Portal, see [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="fdb42-107">Pokud jsou následující hello kroky v tomto dokumentu a dochází k výskytu k chybě při vytváření aplikace hello, podívejte se na podrobnosti o chybě hello informace a návrhy, jak toofix hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="fdb42-107">If you are following hello steps in that document and are getting an error creating hello application, see hello error details for information and suggestions for how toofix hello application.</span></span> <span data-ttu-id="fdb42-108">Většina chybové zprávy, obsahuje navrhované opravu.</span><span class="sxs-lookup"><span data-stu-id="fdb42-108">Most error messages include a suggested fix.</span></span> 

## <a name="specific-things-toocheck"></a><span data-ttu-id="fdb42-109">Toocheck specifické položky</span><span class="sxs-lookup"><span data-stu-id="fdb42-109">Specific things toocheck</span></span>

<span data-ttu-id="fdb42-110">tooavoid běžné chyby, zkontrolujte:</span><span class="sxs-lookup"><span data-stu-id="fdb42-110">tooavoid common errors, verify:</span></span>

-   <span data-ttu-id="fdb42-111">Jste přihlášeni jako správce s toocreate oprávnění aplikace Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="fdb42-111">You are an administrator with permission toocreate an Application Proxy application</span></span>

-   <span data-ttu-id="fdb42-112">Interní adresa URL Hello je jedinečný</span><span class="sxs-lookup"><span data-stu-id="fdb42-112">hello internal URL is unique</span></span>

-   <span data-ttu-id="fdb42-113">externí adresu URL Hello je jedinečný</span><span class="sxs-lookup"><span data-stu-id="fdb42-113">hello external URL is unique</span></span>

-   <span data-ttu-id="fdb42-114">Hello vycházejte adresy URL protokolu http nebo https a končit "/"</span><span class="sxs-lookup"><span data-stu-id="fdb42-114">hello URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="fdb42-115">Adresa URL Hello by měl být název domény a ne IP adresy</span><span class="sxs-lookup"><span data-stu-id="fdb42-115">hello URL should be a domain name and not an IP address</span></span>

<span data-ttu-id="fdb42-116">Hello chybová zpráva by měla zobrazit v pravém horním rohu hello při vytvoření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fdb42-116">hello error message should display in hello top right corner when you create hello application.</span></span> <span data-ttu-id="fdb42-117">Můžete také vybrat hello oznámení ikonu toosee hello chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="fdb42-117">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Oznámení řádku](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a><span data-ttu-id="fdb42-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fdb42-119">Next steps</span></span>
[<span data-ttu-id="fdb42-120">Povolení Proxy aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fdb42-120">Enable Application Proxy in hello Azure portal</span></span>](active-directory-application-proxy-enable.md)
