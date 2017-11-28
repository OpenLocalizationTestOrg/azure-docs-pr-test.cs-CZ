---
title: "aaaConfigure vlastní název domény ve službě Azure App Service (GoDaddy)"
description: "Zjistěte, jak název toouse domény z GoDaddy s Azure Web Apps"
services: app-service
documentationcenter: 
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 33233e30-5846-488f-83f3-b32e5c114564
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 48158ee752f9833249bbf85adf80f572d1c68486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a><span data-ttu-id="1ac40-103">Konfigurace vlastní domény ve službě Azure App Service (zakoupené přímo od GoDaddy)</span><span class="sxs-lookup"><span data-stu-id="1ac40-103">Configure a custom domain name in Azure App Service (Purchased directly from GoDaddy)</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

<span data-ttu-id="1ac40-104">Pokud jste zakoupili domény prostřednictvím Azure App Service Web Apps naleznete v posledním kroku toohello [koupit domény pro webové aplikace](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="1ac40-104">If you have purchased domain through Azure App Service Web Apps then refer toohello final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md).</span></span>

<span data-ttu-id="1ac40-105">Tento článek obsahuje pokyny k používání vlastního názvu domény, který byl zakoupen přímo z [GoDaddy](https://godaddy.com) s [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="1ac40-105">This article provides instructions on using a custom domain name that was purchased directly from [GoDaddy](https://godaddy.com) with [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="1ac40-106">Principy záznamy DNS</span><span class="sxs-lookup"><span data-stu-id="1ac40-106">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="1ac40-107">Přidejte záznam DNS pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="1ac40-107">Add a DNS record for your custom domain</span></span>
<span data-ttu-id="1ac40-108">tooassociate vlastní domény s webovou aplikaci ve službě App Service, musí přidáte nový záznam v tabulce hello DNS pro vaši vlastní doménu pomocí nástrojů poskytovaných GoDaddy.</span><span class="sxs-lookup"><span data-stu-id="1ac40-108">tooassociate your custom domain with a web app in App Service, you must add a new entry in hello DNS table for your custom domain by using tools provided by GoDaddy.</span></span> <span data-ttu-id="1ac40-109">Použijte následující postup toolocate hello DNS nástroje pro GoDaddy.com hello</span><span class="sxs-lookup"><span data-stu-id="1ac40-109">Use hello following steps toolocate hello DNS tools for GoDaddy.com</span></span>

1. <span data-ttu-id="1ac40-110">Přihlaste se tooyour účet s GoDaddy.com a vyberte **Můj účet** a potom **Spravovat mé domény**.</span><span class="sxs-lookup"><span data-stu-id="1ac40-110">Log on tooyour account with GoDaddy.com, and select **My Account** and then **Manage my domains**.</span></span> <span data-ttu-id="1ac40-111">Rozevírací nabídky vyberte hello pro název domény hello chcete toouse s Azure webové aplikace a vyberte **spravovat DNS**.</span><span class="sxs-lookup"><span data-stu-id="1ac40-111">Select hello drop-down menu for hello domain name that you wish toouse with your Azure web app and select **Manage DNS**.</span></span>
   
    ![stránka vlastní doménu pro GoDaddy](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. <span data-ttu-id="1ac40-113">Z hello **podrobnosti doméně** přejděte toohello **souboru zóny DNS** kartě. Toto je část hello používá pro přidávání a úprava záznamů DNS pro název domény.</span><span class="sxs-lookup"><span data-stu-id="1ac40-113">From hello **Domain details** page, scroll toohello **DNS Zone File** tab. This is hello section used for adding and modifying DNS records for your domain name.</span></span>
   
    ![Karta souboru zóny DNS](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    <span data-ttu-id="1ac40-115">Vyberte **přidat záznam** tooadd existujícího záznamu.</span><span class="sxs-lookup"><span data-stu-id="1ac40-115">Select **Add Record** tooadd an existing record.</span></span>
   
    <span data-ttu-id="1ac40-116">příliš**upravit** existujícím záznamem, vyberte hello pera a papír Ikona vedle záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="1ac40-116">too**edit** an existing record, select hello pen & paper icon beside hello record.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1ac40-117">Před přidáním nové záznamy, Všimněte si, že GoDaddy má již vytvořené záznamy DNS pro oblíbené subdomén (nazývá **hostitele** v editoru), jako **e-mailu**, **soubory**, **e-mailu**a další.</span><span class="sxs-lookup"><span data-stu-id="1ac40-117">Before adding new records, note that GoDaddy has already created DNS records for popular sub-domains (called **Host** in editor,) such as **email**, **files**, **mail**, and others.</span></span> <span data-ttu-id="1ac40-118">Pokud název hello chcete toouse již existuje, upravte existující záznamu hello místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="1ac40-118">If hello name you wish toouse already exists, modify hello existing record instead of creating a new one.</span></span>
   > 
   > 
3. <span data-ttu-id="1ac40-119">Při přidávání záznam, musíte nejdřív vybrat typ záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="1ac40-119">When adding a record, you must first select hello record type.</span></span>
   
    ![Vyberte typ záznamu](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    <span data-ttu-id="1ac40-121">Dále je nutné zadat hello **hostitele** (hello vlastní doménu nebo subdomény) a co správci it **odkazuje na**.</span><span class="sxs-lookup"><span data-stu-id="1ac40-121">Next, you must provide hello **Host** (hello custom domain or sub-domain) and what it **Points to**.</span></span>
   
    ![Přidejte záznam zóny](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * <span data-ttu-id="1ac40-123">Při přidávání **záznam (hostitel)** -je nutné nastavit hello **hostitele** pole tooeither  **@**  (reprezentuje název kořenové domény, jako například  **contoso.com**,) * (zástupný znak pro párování více subdomén) nebo subdomény hello chcete toouse (například **www**.) Je nutné nastavit hello **odkazuje na** pole toohello IP adresa vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac40-123">When adding an **A (host) record** - you must set hello **Host** field tooeither **@** (this represents root domain name, such as **contoso.com**,) * (a wildcard for matching multiple sub-domains,) or hello sub-domain you wish toouse (for example, **www**.) You must set hello **Points to** field toohello IP address of your Azure web app.</span></span>
   * <span data-ttu-id="1ac40-124">Při přidávání **záznam CNAME (alias)** -je nutné nastavit hello **hostitele** pole toohello subdomény chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="1ac40-124">When adding a **CNAME (alias) record** - you must set hello **Host** field toohello sub-domain you wish toouse.</span></span> <span data-ttu-id="1ac40-125">Například **www**.</span><span class="sxs-lookup"><span data-stu-id="1ac40-125">For example, **www**.</span></span> <span data-ttu-id="1ac40-126">Je nutné nastavit hello **odkazuje na** pole toohello **. azurewebsites.net** název domény vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1ac40-126">You must set hello **Points to** field toohello **.azurewebsites.net** domain name of your Azure web app.</span></span> <span data-ttu-id="1ac40-127">Například **contoso.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="1ac40-127">For example, **contoso.azurewebsites.net**.</span></span>
4. <span data-ttu-id="1ac40-128">Klikněte na tlačítko **přidat další**.</span><span class="sxs-lookup"><span data-stu-id="1ac40-128">Click **Add Another**.</span></span>
5. <span data-ttu-id="1ac40-129">Vyberte **TXT** jako typ záznamu hello, zadejte **hostitele** hodnotu  **@**  a **odkazuje na** hodnotu  **&lt;yourwebappname&gt;. azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="1ac40-129">Select **TXT** as hello record type, then specify a **Host** value of **@** and a **Points to** value of **&lt;yourwebappname&gt;.azurewebsites.net**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1ac40-130">Tento záznam TXT používá Azure toovalidate, že jste vlastníkem, že hello domény popsaného hello záznam nebo hello první záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="1ac40-130">This TXT record is used by Azure toovalidate that you own hello domain described by hello A record or hello first TXT record.</span></span> <span data-ttu-id="1ac40-131">Jakmile hello domény už namapované toohello webové aplikace ve hello portálu Azure, můžete odebrat tuto položku záznamu TXT.</span><span class="sxs-lookup"><span data-stu-id="1ac40-131">Once hello domain has been mapped toohello web app in hello Azure Portal, this TXT record entry can be removed.</span></span>
   > 
   > 
6. <span data-ttu-id="1ac40-132">Při přidání nebo úprava záznamů, klikněte na tlačítko **Dokončit** toosave změny.</span><span class="sxs-lookup"><span data-stu-id="1ac40-132">When you have finished adding or modifying records, click **Finish** toosave changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-hello-domain-name-on-your-web-app"></a><span data-ttu-id="1ac40-133">Povolit hello názvů domén ve vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="1ac40-133">Enable hello domain name on your web app</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> <span data-ttu-id="1ac40-134">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="1ac40-134">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="1ac40-135">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="1ac40-135">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="1ac40-136">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="1ac40-136">What's changed</span></span>
* <span data-ttu-id="1ac40-137">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="1ac40-137">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

