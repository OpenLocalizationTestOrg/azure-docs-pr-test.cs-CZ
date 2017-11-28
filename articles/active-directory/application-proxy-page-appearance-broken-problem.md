---
title: "stránka aaaApplication nezobrazuje správně pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Pokyny při stránku hello není správně zobrazení v aplikaci Proxy aplikací mít integrované s Azure AD"
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
ms.openlocfilehash: f4abaa4e94c512868f2085affe59cac443784a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a><span data-ttu-id="65926-103">Stránka aplikace se nezobrazuje správně pro aplikaci Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="65926-103">Application page does not display correctly for an Application Proxy application</span></span>

<span data-ttu-id="65926-104">Tento článek vám pomohou tootroubleshoot problémy s aplikacemi Azure Proxy aplikace služby Active Directory když přejdete na stránku toohello, ale něco na stránce hello nevypadá správné.</span><span class="sxs-lookup"><span data-stu-id="65926-104">This article help you tootroubleshoot issues with Azure Active Directory Application Proxy applications when you navigate toohello page, but something on hello page doesn't look correct.</span></span>

## <a name="overview"></a><span data-ttu-id="65926-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="65926-105">Overview</span></span>
<span data-ttu-id="65926-106">Když publikujete aplikaci Proxy aplikace, jsou přístupné pouze stránky v rámci kořenového adresáře, při přístupu k aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="65926-106">When you publish an Application Proxy app, only pages under your root are accessible when accessing hello application.</span></span> <span data-ttu-id="65926-107">Pokud stránku hello nezobrazuje správně, hello kořenové interní adresa URL použitá pro aplikace hello mohou chybět některé prostředky stránky.</span><span class="sxs-lookup"><span data-stu-id="65926-107">If hello page isn’t displaying correctly, hello root internal URL used for hello application may be missing some page resources.</span></span> <span data-ttu-id="65926-108">tooresolve, zajistěte, aby jste publikovali *všechny* hello prostředky pro stránku hello v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="65926-108">tooresolve, make sure you have published *all* hello resources for hello page as part of your application.</span></span>

<span data-ttu-id="65926-109">Můžete ověřit jedná o problém hello otevřením sledovací modul vaší sítě (například aplikaci Fiddler nebo F12 nástroje v Internet Explorer nebo Edge), načítání stránky hello a hledá chyb 404.</span><span class="sxs-lookup"><span data-stu-id="65926-109">You can verify this is hello issue by opening your network tracker (such as Fiddler, or F12 tools in Internet Explorer/Edge), loading hello page, and looking for 404 errors.</span></span> <span data-ttu-id="65926-110">Který označuje hello stránek, které v současné době nelze nalézt a může být stále nutné toobe publikována.</span><span class="sxs-lookup"><span data-stu-id="65926-110">That indicates hello pages that currently cannot be found and may still need toobe published.</span></span>

<span data-ttu-id="65926-111">Jako příklad tento případ, předpokládá jste publikovali aplikaci výdaje pomocí interní URL <http://myapps/expenses>, ale aplikace hello používá šablony stylů hello <http://myapps/style.css>.</span><span class="sxs-lookup"><span data-stu-id="65926-111">As an example of this case, assume you have published an expenses application using an internal URL of <http://myapps/expenses>, but hello app uses hello stylesheet <http://myapps/style.css>.</span></span> <span data-ttu-id="65926-112">V takovém případě šablony stylů hello není publikována ve vaší aplikaci, tak načítání aplikace hello výdaje throw 404 Chyba při pokusu o tooload style.css.</span><span class="sxs-lookup"><span data-stu-id="65926-112">In this case, hello stylesheet is not published in your application, so loading hello expenses app throw a 404 error while trying tooload style.css.</span></span> <span data-ttu-id="65926-113">V tomto příkladu by hello problém vyřešit tím, že publikujete aplikace hello s interní URL <http://myapp/> místo.</span><span class="sxs-lookup"><span data-stu-id="65926-113">In this example, hello problem would be resolved by publishing hello application with an internal URL of <http://myapp/> instead.</span></span>

## <a name="problems-with-publishing-as-one-application"></a><span data-ttu-id="65926-114">Problémy s publikování jako jednu aplikaci</span><span class="sxs-lookup"><span data-stu-id="65926-114">Problems with publishing as one application</span></span>

<span data-ttu-id="65926-115">Pokud není možné toopublish všechny prostředky v rámci hello stejnou aplikaci, můžete potřebovat toopublish více aplikací a povolit propojení mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="65926-115">If it is not possible toopublish all resources within hello same application, you need toopublish multiple applications and enable links between them.</span></span>

<span data-ttu-id="65926-116">toodo Ano, doporučujeme používat hello [vlastní domény](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) řešení.</span><span class="sxs-lookup"><span data-stu-id="65926-116">toodo so, we recommend using hello [custom domains](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) solution.</span></span> <span data-ttu-id="65926-117">Toto řešení však třeba vlastní hello certifikát pro vaši doménu a aplikace pomocí plně kvalifikované názvy domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="65926-117">However, this solution requires that you own hello certificate for your domain and your applications use fully qualified domain names (FQDNs).</span></span> <span data-ttu-id="65926-118">Další možnosti najdete v tématu hello [řešení potíží s poškozených odkazů dokumentaci](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span><span class="sxs-lookup"><span data-stu-id="65926-118">For other options, see hello [troubleshoot broken links documentation](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span></span>

## <a name="next-steps"></a><span data-ttu-id="65926-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65926-119">Next steps</span></span>
[<span data-ttu-id="65926-120">Publikování aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="65926-120">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
