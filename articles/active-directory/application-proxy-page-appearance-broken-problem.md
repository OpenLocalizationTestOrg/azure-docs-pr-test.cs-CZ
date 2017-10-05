---
title: "Stránka aplikace se nezobrazuje správně pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Pokyny při stránky není správně zobrazení v aplikaci Proxy aplikací mít integrované s Azure AD"
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
ms.openlocfilehash: cac4c333e59ef9a0f28a2f93a7afee22eeafd54e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a><span data-ttu-id="ba1f2-103">Stránka aplikace se nezobrazuje správně pro aplikaci Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="ba1f2-103">Application page does not display correctly for an Application Proxy application</span></span>

<span data-ttu-id="ba1f2-104">Tento článek pomoci při řešení problémů s Proxy aplikace služby Active Directory Azure aplikace, když přejdete na stránku, ale něco na stránce nevypadá správné.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-104">This article help you to troubleshoot issues with Azure Active Directory Application Proxy applications when you navigate to the page, but something on the page doesn't look correct.</span></span>

## <a name="overview"></a><span data-ttu-id="ba1f2-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="ba1f2-105">Overview</span></span>
<span data-ttu-id="ba1f2-106">Když publikujete aplikaci Proxy aplikace, jsou přístupné pouze stránky v rámci kořenového adresáře, při přístupu k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-106">When you publish an Application Proxy app, only pages under your root are accessible when accessing the application.</span></span> <span data-ttu-id="ba1f2-107">Pokud stránky nezobrazuje správně, interní adresy URL kořenového adresáře používá pro aplikace mohou chybět některé prostředky stránky.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-107">If the page isn’t displaying correctly, the root internal URL used for the application may be missing some page resources.</span></span> <span data-ttu-id="ba1f2-108">Chcete-li vyřešit, ujistěte se, jste publikovali *všechny* prostředky pro stránku v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-108">To resolve, make sure you have published *all* the resources for the page as part of your application.</span></span>

<span data-ttu-id="ba1f2-109">Toto je problém otevřením sledovací modul vaší sítě můžete ověřit (například aplikaci Fiddler nebo F12 nástroje v Internet Explorer nebo Edge), načítání stránky a hledá chyb 404.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-109">You can verify this is the issue by opening your network tracker (such as Fiddler, or F12 tools in Internet Explorer/Edge), loading the page, and looking for 404 errors.</span></span> <span data-ttu-id="ba1f2-110">Který označuje stránek, které v současné době nelze nalézt a může stále je třeba publikovat.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-110">That indicates the pages that currently cannot be found and may still need to be published.</span></span>

<span data-ttu-id="ba1f2-111">Jako příklad tento případ, předpokládá jste publikovali aplikaci výdaje pomocí interní URL <http://myapps/expenses>, ale aplikace používá šablony stylů <http://myapps/style.css>.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-111">As an example of this case, assume you have published an expenses application using an internal URL of <http://myapps/expenses>, but the app uses the stylesheet <http://myapps/style.css>.</span></span> <span data-ttu-id="ba1f2-112">V takovém případě šablony stylů není publikována ve vaší aplikaci, takže chybu 404 načítání aplikace výdaje výjimku při pokusu o načtení style.css.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-112">In this case, the stylesheet is not published in your application, so loading the expenses app throw a 404 error while trying to load style.css.</span></span> <span data-ttu-id="ba1f2-113">V tomto příkladu by problém vyřešit tím, že publikujete aplikaci s interní URL <http://myapp/> místo.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-113">In this example, the problem would be resolved by publishing the application with an internal URL of <http://myapp/> instead.</span></span>

## <a name="problems-with-publishing-as-one-application"></a><span data-ttu-id="ba1f2-114">Problémy s publikování jako jednu aplikaci</span><span class="sxs-lookup"><span data-stu-id="ba1f2-114">Problems with publishing as one application</span></span>

<span data-ttu-id="ba1f2-115">Pokud není možné publikovat všechny prostředky v rámci stejné aplikaci, musíte k publikování několika aplikací a povolit propojení mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-115">If it is not possible to publish all resources within the same application, you need to publish multiple applications and enable links between them.</span></span>

<span data-ttu-id="ba1f2-116">Chcete-li tak učinit, doporučujeme používat [vlastní domény](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) řešení.</span><span class="sxs-lookup"><span data-stu-id="ba1f2-116">To do so, we recommend using the [custom domains](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) solution.</span></span> <span data-ttu-id="ba1f2-117">Toto řešení je však třeba vlastní certifikát pro vaši doménu a aplikace pomocí plně kvalifikované názvy domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="ba1f2-117">However, this solution requires that you own the certificate for your domain and your applications use fully qualified domain names (FQDNs).</span></span> <span data-ttu-id="ba1f2-118">Další možnosti najdete v tématu [řešení potíží s poškozených odkazů dokumentaci](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span><span class="sxs-lookup"><span data-stu-id="ba1f2-118">For other options, see the [troubleshoot broken links documentation](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba1f2-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba1f2-119">Next steps</span></span>
[<span data-ttu-id="ba1f2-120">Publikování aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ba1f2-120">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
