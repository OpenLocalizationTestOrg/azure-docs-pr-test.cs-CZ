---
title: "Konfigurace vlastního názvu domény v Azure App Service (GoDaddy)"
description: "Naučte se používat název domény z GoDaddy s Azure Web Apps"
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
ms.openlocfilehash: 158c5dc06f83e16633d3c2fbb4eb27d3e8af030c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a><span data-ttu-id="8dd28-103">Konfigurace vlastní domény ve službě Azure App Service (zakoupené přímo od GoDaddy)</span><span class="sxs-lookup"><span data-stu-id="8dd28-103">Configure a custom domain name in Azure App Service (Purchased directly from GoDaddy)</span></span>
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

<span data-ttu-id="8dd28-104">Pokud jste zakoupili prostřednictvím Azure App Service Web Apps domény pak odkazovat na posledním kroku [koupit domény pro webové aplikace](custom-dns-web-site-buydomains-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="8dd28-104">If you have purchased domain through Azure App Service Web Apps then refer to the final step of [Buy Domain for Web Apps](custom-dns-web-site-buydomains-web-app.md).</span></span>

<span data-ttu-id="8dd28-105">Tento článek obsahuje pokyny k používání vlastního názvu domény, který byl zakoupen přímo z [GoDaddy](https://godaddy.com) s [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="8dd28-105">This article provides instructions on using a custom domain name that was purchased directly from [GoDaddy](https://godaddy.com) with [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a><span data-ttu-id="8dd28-106">Principy záznamy DNS</span><span class="sxs-lookup"><span data-stu-id="8dd28-106">Understanding DNS records</span></span>
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a><span data-ttu-id="8dd28-107">Přidejte záznam DNS pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="8dd28-107">Add a DNS record for your custom domain</span></span>
<span data-ttu-id="8dd28-108">Pokud chcete přiřadit vlastní domény webové aplikace ve službě App Service, musí přidáte nový záznam v tabulce DNS pro vaši vlastní doménu pomocí nástrojů poskytovaných GoDaddy.</span><span class="sxs-lookup"><span data-stu-id="8dd28-108">To associate your custom domain with a web app in App Service, you must add a new entry in the DNS table for your custom domain by using tools provided by GoDaddy.</span></span> <span data-ttu-id="8dd28-109">Použijte následující postup k vyhledání DNS nástroje pro GoDaddy.com</span><span class="sxs-lookup"><span data-stu-id="8dd28-109">Use the following steps to locate the DNS tools for GoDaddy.com</span></span>

1. <span data-ttu-id="8dd28-110">Přihlaste se k účtu s GoDaddy.com a vyberte **Můj účet** a potom **Spravovat mé domény**.</span><span class="sxs-lookup"><span data-stu-id="8dd28-110">Log on to your account with GoDaddy.com, and select **My Account** and then **Manage my domains**.</span></span> <span data-ttu-id="8dd28-111">Vyberte rozevírací nabídku pro název domény, který chcete používat s vaší webové aplikace Azure a vyberte **spravovat DNS**.</span><span class="sxs-lookup"><span data-stu-id="8dd28-111">Select the drop-down menu for the domain name that you wish to use with your Azure web app and select **Manage DNS**.</span></span>
   
    ![stránka vlastní doménu pro GoDaddy](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. <span data-ttu-id="8dd28-113">Z **podrobnosti doméně** stránky, přejděte k položce **souboru zóny DNS** kartě.</span><span class="sxs-lookup"><span data-stu-id="8dd28-113">From the **Domain details** page, scroll to the **DNS Zone File** tab.</span></span> <span data-ttu-id="8dd28-114">Toto je část používat pro přidávání a úprava záznamů DNS pro název domény.</span><span class="sxs-lookup"><span data-stu-id="8dd28-114">This is the section used for adding and modifying DNS records for your domain name.</span></span>
   
    ![Karta souboru zóny DNS](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    <span data-ttu-id="8dd28-116">Vyberte **přidat záznam** přidání existujícího záznamu.</span><span class="sxs-lookup"><span data-stu-id="8dd28-116">Select **Add Record** to add an existing record.</span></span>
   
    <span data-ttu-id="8dd28-117">K **upravit** stávajícího záznamu, vyberte pera & papír Ikona vedle záznamu.</span><span class="sxs-lookup"><span data-stu-id="8dd28-117">To **edit** an existing record, select the pen & paper icon beside the record.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8dd28-118">Před přidáním nové záznamy, Všimněte si, že GoDaddy má již vytvořené záznamy DNS pro oblíbené subdomén (nazývá **hostitele** v editoru), jako **e-mailu**, **soubory**, **e-mailu**a další.</span><span class="sxs-lookup"><span data-stu-id="8dd28-118">Before adding new records, note that GoDaddy has already created DNS records for popular sub-domains (called **Host** in editor,) such as **email**, **files**, **mail**, and others.</span></span> <span data-ttu-id="8dd28-119">Pokud název, který chcete použít, již existuje, upravte existující záznamu místo vytvoření nové.</span><span class="sxs-lookup"><span data-stu-id="8dd28-119">If the name you wish to use already exists, modify the existing record instead of creating a new one.</span></span>
   > 
   > 
3. <span data-ttu-id="8dd28-120">Při přidávání záznam, musíte nejdřív vybrat typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="8dd28-120">When adding a record, you must first select the record type.</span></span>
   
    ![Vyberte typ záznamu](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    <span data-ttu-id="8dd28-122">Dále je nutné zadat **hostitele** (vlastní doménu nebo subdomény) a co správci it **odkazuje na**.</span><span class="sxs-lookup"><span data-stu-id="8dd28-122">Next, you must provide the **Host** (the custom domain or sub-domain) and what it **Points to**.</span></span>
   
    ![Přidejte záznam zóny](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * <span data-ttu-id="8dd28-124">Při přidávání **záznam (hostitel)** -je nutné nastavit **hostitele** pole buď  **@**  (reprezentuje název kořenové domény, jako například **contoso.com** ,) * (zástupný znak pro párování více subdomén) nebo subdomény, které chcete použít (například **www**.) Je nutné nastavit **odkazuje na** pole IP adresu vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="8dd28-124">When adding an **A (host) record** - you must set the **Host** field to either **@** (this represents root domain name, such as **contoso.com**,) * (a wildcard for matching multiple sub-domains,) or the sub-domain you wish to use (for example, **www**.) You must set the **Points to** field to the IP address of your Azure web app.</span></span>
   * <span data-ttu-id="8dd28-125">Při přidávání **záznam CNAME (alias)** -je nutné nastavit **hostitele** do subdomény, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="8dd28-125">When adding a **CNAME (alias) record** - you must set the **Host** field to the sub-domain you wish to use.</span></span> <span data-ttu-id="8dd28-126">Například **www**.</span><span class="sxs-lookup"><span data-stu-id="8dd28-126">For example, **www**.</span></span> <span data-ttu-id="8dd28-127">Je nutné nastavit **odkazuje na** do **. azurewebsites.net** název domény vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="8dd28-127">You must set the **Points to** field to the **.azurewebsites.net** domain name of your Azure web app.</span></span> <span data-ttu-id="8dd28-128">Například **contoso.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="8dd28-128">For example, **contoso.azurewebsites.net**.</span></span>
4. <span data-ttu-id="8dd28-129">Klikněte na tlačítko **přidat další**.</span><span class="sxs-lookup"><span data-stu-id="8dd28-129">Click **Add Another**.</span></span>
5. <span data-ttu-id="8dd28-130">Vyberte **TXT** jako typ záznamu, zadejte **hostitele** hodnotu  **@**  a **odkazuje na** hodnotu  **&lt;yourwebappname&gt;. azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="8dd28-130">Select **TXT** as the record type, then specify a **Host** value of **@** and a **Points to** value of **&lt;yourwebappname&gt;.azurewebsites.net**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8dd28-131">Tento záznam TXT se používají v Azure k ověření vlastnictví domény popsaného záznam A nebo na první záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="8dd28-131">This TXT record is used by Azure to validate that you own the domain described by the A record or the first TXT record.</span></span> <span data-ttu-id="8dd28-132">Jakmile doména byla mapována na webovou aplikaci na portálu Azure, lze odebrat tuto položku záznamu TXT.</span><span class="sxs-lookup"><span data-stu-id="8dd28-132">Once the domain has been mapped to the web app in the Azure Portal, this TXT record entry can be removed.</span></span>
   > 
   > 
6. <span data-ttu-id="8dd28-133">Při přidání nebo úprava záznamů, klikněte na tlačítko **Dokončit** uložte změny.</span><span class="sxs-lookup"><span data-stu-id="8dd28-133">When you have finished adding or modifying records, click **Finish** to save changes.</span></span>

<a name="enabledomain"></a>

## <a name="enable-the-domain-name-on-your-web-app"></a><span data-ttu-id="8dd28-134">Povolit názvů domén ve vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="8dd28-134">Enable the domain name on your web app</span></span>
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> <span data-ttu-id="8dd28-135">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8dd28-135">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="8dd28-136">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="8dd28-136">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="8dd28-137">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="8dd28-137">What's changed</span></span>
* <span data-ttu-id="8dd28-138">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="8dd28-138">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

