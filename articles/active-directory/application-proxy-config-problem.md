---
title: "Problém s vytvořením aplikace Proxy aplikace | Microsoft Docs"
description: "Řešení potíží při vytváření aplikací pro Proxy aplikace v portálu pro správu Azure AD"
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
ms.openlocfilehash: fe56f56162ba7186f1b660a5b44fcef38f1dee9d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-creating-an-application-proxy-application"></a><span data-ttu-id="f2dae-103">Problém s vytvořením aplikace Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="f2dae-103">Problem creating an Application Proxy application</span></span> 

<span data-ttu-id="f2dae-104">V následující tabulce jsou některé běžné problémy, uživatelé setkávají při vytváření nové aplikace proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2dae-104">Below are some of the common issues people face when creating a new application proxy application.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="f2dae-105">Doporučené dokumenty</span><span class="sxs-lookup"><span data-stu-id="f2dae-105">Recommended documents</span></span> 

<span data-ttu-id="f2dae-106">Další informace o vytváření aplikací Proxy aplikace prostřednictvím portálu pro správu, najdete v části [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="f2dae-106">To learn more about creating an Application Proxy application through the Admin Portal, see [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="f2dae-107">Pokud postupujete podle kroků v tomto dokumentu a dochází k výskytu k chybě při vytváření aplikace, najdete v části Podrobnosti o chybě informace a návrhy, jak opravit aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f2dae-107">If you are following the steps in that document and are getting an error creating the application, see the error details for information and suggestions for how to fix the application.</span></span> <span data-ttu-id="f2dae-108">Většina chybové zprávy, obsahuje navrhované opravu.</span><span class="sxs-lookup"><span data-stu-id="f2dae-108">Most error messages include a suggested fix.</span></span> 

## <a name="specific-things-to-check"></a><span data-ttu-id="f2dae-109">Konkrétní co je potřeba zkontrolovat</span><span class="sxs-lookup"><span data-stu-id="f2dae-109">Specific things to check</span></span>

<span data-ttu-id="f2dae-110">Aby se zabránilo běžné chyby, ověřte:</span><span class="sxs-lookup"><span data-stu-id="f2dae-110">To avoid common errors, verify:</span></span>

-   <span data-ttu-id="f2dae-111">Správci s oprávněním k vytvoření aplikace Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="f2dae-111">You are an administrator with permission to create an Application Proxy application</span></span>

-   <span data-ttu-id="f2dae-112">Interní adresa URL je jedinečný</span><span class="sxs-lookup"><span data-stu-id="f2dae-112">The internal URL is unique</span></span>

-   <span data-ttu-id="f2dae-113">Externí adresa URL je jedinečný</span><span class="sxs-lookup"><span data-stu-id="f2dae-113">The external URL is unique</span></span>

-   <span data-ttu-id="f2dae-114">Adresy URL začínat protokolu http nebo https a končit "/"</span><span class="sxs-lookup"><span data-stu-id="f2dae-114">The URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="f2dae-115">Adresa URL by měla být název domény a ne IP adresy</span><span class="sxs-lookup"><span data-stu-id="f2dae-115">The URL should be a domain name and not an IP address</span></span>

<span data-ttu-id="f2dae-116">Chybová zpráva by měla zobrazit v pravém horním rohu při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="f2dae-116">The error message should display in the top right corner when you create the application.</span></span> <span data-ttu-id="f2dae-117">Můžete také vybrat na ikonu oznámení naleznete v chybových zprávách.</span><span class="sxs-lookup"><span data-stu-id="f2dae-117">You can also select the notification icon to see the error messages.</span></span>

   ![Oznámení řádku](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a><span data-ttu-id="f2dae-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f2dae-119">Next steps</span></span>
[<span data-ttu-id="f2dae-120">Povolení Proxy aplikace na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f2dae-120">Enable Application Proxy in the Azure portal</span></span>](active-directory-application-proxy-enable.md)
