---
title: "Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service, která používá Traffic Manager pro vyrovnávání zatížení."
description: "Vlastní název domény pro použití webové aplikace ve službě Azure App Service, která zahrnuje Traffic Manager pro vyrovnávání zatížení."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 5f099201d9018a6f8577cb3daf127d09560fb94b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a><span data-ttu-id="c48ea-103">Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service pomocí Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="c48ea-103">Configuring a custom domain name for a web app in Azure App Service using Traffic Manager</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

<span data-ttu-id="c48ea-104">Tento článek obsahuje obecné pokyny pro používání vlastního názvu domény službou Azure App Service, které pomocí služby Traffic Manager pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c48ea-104">This article provides generic instructions for using a custom domain name with Azure App Service that use Traffic Manager for load balancing.</span></span>

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="c48ea-105">Principy záznamy DNS</span><span class="sxs-lookup"><span data-stu-id="c48ea-105">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a><span data-ttu-id="c48ea-106">Konfigurace webových aplikacích pro standardní režim</span><span class="sxs-lookup"><span data-stu-id="c48ea-106">Configure your web apps for standard mode</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="c48ea-107">Přidejte záznam DNS pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="c48ea-107">Add a DNS record for your custom domain</span></span>
> [!NOTE]
> <span data-ttu-id="c48ea-108">Pokud jste zakoupili domény prostřednictvím Azure App Service Web Apps přeskočit následující kroky a odkazovat na posledním kroku [koupit domény pro webové aplikace](custom-dns-web-site-buydomains-web-app.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c48ea-108">If you have purchased domain through Azure App Service Web Apps then skip following steps and refer to the final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md) article.</span></span>
> 
> 

<span data-ttu-id="c48ea-109">Pokud chcete přiřadit vlastní domény webové aplikace ve službě Azure App Service, musí přidáte nový záznam v tabulce DNS pro vaši vlastní doménu pomocí nástrojů poskytovaných registrátora domény, které jste zakoupili název domény z.</span><span class="sxs-lookup"><span data-stu-id="c48ea-109">To associate your custom domain with a web app in Azure App Service, you must add a new entry in the DNS table for your custom domain by using tools provided by the domain registrar that you purchased your domain name from.</span></span> <span data-ttu-id="c48ea-110">Použijte následující postup pro vyhledání a pomocí nástrojů DNS.</span><span class="sxs-lookup"><span data-stu-id="c48ea-110">Use the following steps to locate and use the DNS tools.</span></span>

1. <span data-ttu-id="c48ea-111">Přihlaste se ke svému účtu u registrátora domény a vyhledejte stránku pro správu záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="c48ea-111">Sign in to your account at your domain registrar, and look for a page for managing DNS records.</span></span> <span data-ttu-id="c48ea-112">Vyhledejte odkazy nebo oblasti lokality označený jako **název domény**, **DNS**, nebo **název serveru správy**.</span><span class="sxs-lookup"><span data-stu-id="c48ea-112">Look for links or areas of the site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="c48ea-113">Často můžete najít odkaz na tuto stránku zobrazení informací o vašem účtu a podívat se na odkaz, jako **mé domény**.</span><span class="sxs-lookup"><span data-stu-id="c48ea-113">Often a link to this page can be found be viewing your account information, and then looking for a link such as **My domains**.</span></span>
2. <span data-ttu-id="c48ea-114">Jakmile naleznete na stránce Správa pro název domény, vyhledejte odkaz, který slouží k úpravě záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="c48ea-114">Once you have found the management page for your domain name, look for a link that allows you to edit the DNS records.</span></span> <span data-ttu-id="c48ea-115">To může být uveden jako **souboru zóny**, **záznamy DNS**, nebo jako **Upřesnit** odkazu na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c48ea-115">This might be listed as a **Zone file**, **DNS Records**, or as an **Advanced** configuration link.</span></span>
   
   * <span data-ttu-id="c48ea-116">Stránky bude mít pravděpodobně několik záznamů, které jsou již vytvořeny, jako je například přidružení vstupního '**@**'nebo'\*' 'domény parkovací' stránky.</span><span class="sxs-lookup"><span data-stu-id="c48ea-116">The page will most likely have a few records already created, such as an entry associating '**@**' or '\*' with a 'domain parking' page.</span></span> <span data-ttu-id="c48ea-117">Záznamy pro běžné dílčím doménám domény může obsahovat také jako **www**.</span><span class="sxs-lookup"><span data-stu-id="c48ea-117">It may also contain records for common sub-domains such as **www**.</span></span>
   * <span data-ttu-id="c48ea-118">Bude zmínili stránce **záznamy CNAME**, nebo zadejte rozevíracího seznamu vyberte typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="c48ea-118">The page will mention **CNAME records**, or provide a drop-down to select a record type.</span></span> <span data-ttu-id="c48ea-119">Například je může další záznamy také zmínili **záznamy A** a **záznamů MX**.</span><span class="sxs-lookup"><span data-stu-id="c48ea-119">It may also mention other records such as **A records** and **MX records**.</span></span> <span data-ttu-id="c48ea-120">V některých případech záznamy CNAME bude volat jiné názvy, jako **záznam aliasu**.</span><span class="sxs-lookup"><span data-stu-id="c48ea-120">In some cases, CNAME records will be called by other names such as an **Alias Record**.</span></span>
   * <span data-ttu-id="c48ea-121">Stránky bude mít i pole, které vám umožní **mapy** z **název hostitele** nebo **název domény** na jiný název domény.</span><span class="sxs-lookup"><span data-stu-id="c48ea-121">The page will also have fields that allow you to **map** from a **Host name** or **Domain name** to another domain name.</span></span>
3. <span data-ttu-id="c48ea-122">Když jsou specifikace každý Registrátor liší, obecně namapujete *z* vlastního názvu domény (například **contoso.com**,) *k* název domény Traffic Manageru (**contoso.trafficmanager.net**) používané pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c48ea-122">While the specifics of each registrar vary, in general you map *from* your custom domain name (such as **contoso.com**,) *to* the Traffic Manager domain name (**contoso.trafficmanager.net**) that is used for your web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c48ea-123">Pokud záznam se už používá a je nutné ho preventivně svázat aplikace k němu, případně můžete vytvořit další záznam CNAME.</span><span class="sxs-lookup"><span data-stu-id="c48ea-123">Alternatively, if a record is already in use and you need to preemptively bind your apps to it, you can create an additional CNAME record.</span></span> <span data-ttu-id="c48ea-124">Například ho preventivně vazby **www.contoso.com** do webové aplikace, vytvořte záznam CNAME z **awverify.www** k **contoso.trafficmanager.net**.</span><span class="sxs-lookup"><span data-stu-id="c48ea-124">For example, to preemptively bind **www.contoso.com** to your web app, create a CNAME record from **awverify.www** to **contoso.trafficmanager.net**.</span></span> <span data-ttu-id="c48ea-125">Potom můžete přidat "www.contoso.com" do vaší webové aplikace beze změny záznam CNAME "www".</span><span class="sxs-lookup"><span data-stu-id="c48ea-125">You can then add "www.contoso.com" to your Web App without changing the "www" CNAME record.</span></span> <span data-ttu-id="c48ea-126">Další informace najdete v tématu [záznamy DNS vytvořit pro webové aplikace ve vlastní doménu][CREATEDNS].</span><span class="sxs-lookup"><span data-stu-id="c48ea-126">For more information, see [Create DNS records for a web app in a custom domain][CREATEDNS].</span></span>
   > 
   > 
4. <span data-ttu-id="c48ea-127">Po dokončení přidávání nebo úpravě záznamů DNS u registrátora uložte změny.</span><span class="sxs-lookup"><span data-stu-id="c48ea-127">Once you have finished adding or modifying DNS records at your registrar, save the changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a><span data-ttu-id="c48ea-128">Povolit správce provozu</span><span class="sxs-lookup"><span data-stu-id="c48ea-128">Enable Traffic Manager</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="c48ea-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c48ea-129">Next steps</span></span>
<span data-ttu-id="c48ea-130">Další informace najdete ve [Středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="c48ea-130">For more information, see the [Node.js Developer Center](/develop/nodejs/).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
