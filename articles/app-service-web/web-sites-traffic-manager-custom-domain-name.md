---
title: "aaaConfigure vlastní název domény pro webovou aplikaci v Azure App Service, která používá Traffic Manager pro vyrovnávání zatížení."
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
ms.openlocfilehash: dfde5fc6b445b30b10e03dcb03e8d072130d9377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a><span data-ttu-id="3dfde-103">Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service pomocí Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="3dfde-103">Configuring a custom domain name for a web app in Azure App Service using Traffic Manager</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

<span data-ttu-id="3dfde-104">Tento článek obsahuje obecné pokyny pro používání vlastního názvu domény službou Azure App Service, které pomocí služby Traffic Manager pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3dfde-104">This article provides generic instructions for using a custom domain name with Azure App Service that use Traffic Manager for load balancing.</span></span>

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="3dfde-105">Principy záznamy DNS</span><span class="sxs-lookup"><span data-stu-id="3dfde-105">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a><span data-ttu-id="3dfde-106">Konfigurace webových aplikacích pro standardní režim</span><span class="sxs-lookup"><span data-stu-id="3dfde-106">Configure your web apps for standard mode</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="3dfde-107">Přidejte záznam DNS pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="3dfde-107">Add a DNS record for your custom domain</span></span>
> [!NOTE]
> <span data-ttu-id="3dfde-108">Pokud jste zakoupili domény prostřednictvím Azure App Service Web Apps přeskočit následující kroky a najdete v posledním kroku toohello [koupit domény pro webové aplikace](custom-dns-web-site-buydomains-web-app.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3dfde-108">If you have purchased domain through Azure App Service Web Apps then skip following steps and refer toohello final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md) article.</span></span>
> 
> 

<span data-ttu-id="3dfde-109">tooassociate vaše vlastní doména se webové aplikace ve službě Azure App Service musí přidáte nový záznam v tabulce hello DNS pro vaši vlastní doménu pomocí nástrojů poskytovaných hello doménového registrátora, které jste zakoupili název domény z.</span><span class="sxs-lookup"><span data-stu-id="3dfde-109">tooassociate your custom domain with a web app in Azure App Service, you must add a new entry in hello DNS table for your custom domain by using tools provided by hello domain registrar that you purchased your domain name from.</span></span> <span data-ttu-id="3dfde-110">Použijte následující kroky toolocate hello a pomocí nástrojů DNS hello.</span><span class="sxs-lookup"><span data-stu-id="3dfde-110">Use hello following steps toolocate and use hello DNS tools.</span></span>

1. <span data-ttu-id="3dfde-111">Přihlašovací účet tooyour u doménového registrátora a vyhledejte stránku pro správu záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="3dfde-111">Sign in tooyour account at your domain registrar, and look for a page for managing DNS records.</span></span> <span data-ttu-id="3dfde-112">Vyhledejte odkazy nebo oblasti lokality hello označený jako **název domény**, **DNS**, nebo **název serveru správy**.</span><span class="sxs-lookup"><span data-stu-id="3dfde-112">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="3dfde-113">Často se odkaz toothis stránku můžete najít zobrazení informací o vašem účtu a pak hledá odkaz, jako **mé domény**.</span><span class="sxs-lookup"><span data-stu-id="3dfde-113">Often a link toothis page can be found be viewing your account information, and then looking for a link such as **My domains**.</span></span>
2. <span data-ttu-id="3dfde-114">Jakmile naleznete stránku hello správy pro název domény, vyhledejte odkaz, který vám umožní záznamy DNS tooedit hello.</span><span class="sxs-lookup"><span data-stu-id="3dfde-114">Once you have found hello management page for your domain name, look for a link that allows you tooedit hello DNS records.</span></span> <span data-ttu-id="3dfde-115">To může být uveden jako **souboru zóny**, **záznamy DNS**, nebo jako **Upřesnit** odkazu na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="3dfde-115">This might be listed as a **Zone file**, **DNS Records**, or as an **Advanced** configuration link.</span></span>
   
   * <span data-ttu-id="3dfde-116">stránku Hello bude mít pravděpodobně několik záznamů, které jsou již vytvořeny, jako je například přidružení vstupního '**@**'nebo'\*' 'domény parkovací' stránky.</span><span class="sxs-lookup"><span data-stu-id="3dfde-116">hello page will most likely have a few records already created, such as an entry associating '**@**' or '\*' with a 'domain parking' page.</span></span> <span data-ttu-id="3dfde-117">Záznamy pro běžné dílčím doménám domény může obsahovat také jako **www**.</span><span class="sxs-lookup"><span data-stu-id="3dfde-117">It may also contain records for common sub-domains such as **www**.</span></span>
   * <span data-ttu-id="3dfde-118">bude zmínili stránku Hello **záznamy CNAME**, nebo zadejte rozevíracího seznamu tooselect typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="3dfde-118">hello page will mention **CNAME records**, or provide a drop-down tooselect a record type.</span></span> <span data-ttu-id="3dfde-119">Například je může další záznamy také zmínili **záznamy A** a **záznamů MX**.</span><span class="sxs-lookup"><span data-stu-id="3dfde-119">It may also mention other records such as **A records** and **MX records**.</span></span> <span data-ttu-id="3dfde-120">V některých případech záznamy CNAME bude volat jiné názvy, jako **záznam aliasu**.</span><span class="sxs-lookup"><span data-stu-id="3dfde-120">In some cases, CNAME records will be called by other names such as an **Alias Record**.</span></span>
   * <span data-ttu-id="3dfde-121">stránku Hello bude mít i pole, které vám umožňují příliš**mapy** z **název hostitele** nebo **název domény** tooanother název domény.</span><span class="sxs-lookup"><span data-stu-id="3dfde-121">hello page will also have fields that allow you too**map** from a **Host name** or **Domain name** tooanother domain name.</span></span>
3. <span data-ttu-id="3dfde-122">Při hello specifikace každý Registrátor liší, obecně namapujete *z* vlastního názvu domény (například **contoso.com**,) *k* název domény Traffic Manageru hello (**contoso.trafficmanager.net**) používané pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3dfde-122">While hello specifics of each registrar vary, in general you map *from* your custom domain name (such as **contoso.com**,) *to* hello Traffic Manager domain name (**contoso.trafficmanager.net**) that is used for your web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3dfde-123">Případně pokud záznam se už používá a je nutné toopreemptively vazby tooit vaší aplikace, můžete vytvořit další záznam CNAME.</span><span class="sxs-lookup"><span data-stu-id="3dfde-123">Alternatively, if a record is already in use and you need toopreemptively bind your apps tooit, you can create an additional CNAME record.</span></span> <span data-ttu-id="3dfde-124">Například toopreemptively vazby **www.contoso.com** tooyour webové aplikace, vytvořte záznam CNAME z **awverify.www** příliš**contoso.trafficmanager.net**.</span><span class="sxs-lookup"><span data-stu-id="3dfde-124">For example, toopreemptively bind **www.contoso.com** tooyour web app, create a CNAME record from **awverify.www** too**contoso.trafficmanager.net**.</span></span> <span data-ttu-id="3dfde-125">Poté můžete přidat "www.contoso.com" tooyour webové aplikace beze změny záznam CNAME hello "www".</span><span class="sxs-lookup"><span data-stu-id="3dfde-125">You can then add "www.contoso.com" tooyour Web App without changing hello "www" CNAME record.</span></span> <span data-ttu-id="3dfde-126">Další informace najdete v tématu [záznamy DNS vytvořit pro webové aplikace ve vlastní doménu][CREATEDNS].</span><span class="sxs-lookup"><span data-stu-id="3dfde-126">For more information, see [Create DNS records for a web app in a custom domain][CREATEDNS].</span></span>
   > 
   > 
4. <span data-ttu-id="3dfde-127">Po dokončení přidávání nebo úpravě záznamů DNS u registrátora uložte změny hello.</span><span class="sxs-lookup"><span data-stu-id="3dfde-127">Once you have finished adding or modifying DNS records at your registrar, save hello changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a><span data-ttu-id="3dfde-128">Povolit správce provozu</span><span class="sxs-lookup"><span data-stu-id="3dfde-128">Enable Traffic Manager</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="3dfde-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3dfde-129">Next steps</span></span>
<span data-ttu-id="3dfde-130">Další informace najdete v tématu hello [středisku pro vývojáře Node.js](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="3dfde-130">For more information, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
