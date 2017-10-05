---
title: "Konfigurace vlastního názvu domény v cloudové služby | Microsoft Docs"
description: "Zjistěte, jak vystavit aplikaci Azure nebo dat k Internetu na vlastní domény tak, že nakonfigurujete nastavení DNS.  Tyto příklady použití portálu Azure."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: cf43d86dddc3a68573e1ba1b09118c54f0b16bc5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a><span data-ttu-id="7fda2-104">Konfigurace vlastního názvu domény pro cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="7fda2-104">Configuring a custom domain name for an Azure cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7fda2-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7fda2-105">Azure portal</span></span>](cloud-services-custom-domain-name-portal.md)
> * [<span data-ttu-id="7fda2-106">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="7fda2-106">Azure classic portal</span></span>](cloud-services-custom-domain-name.md)
> 
> 

<span data-ttu-id="7fda2-107">Při vytváření cloudové služby Azure, přiřadí ji k subdoména **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="7fda2-107">When you create a Cloud Service, Azure assigns it to a subdomain of **cloudapp.net**.</span></span> <span data-ttu-id="7fda2-108">Například pokud cloudové služby má název "contoso", uživatelé budou mít přístup k aplikaci na adresu URL podobnou http://contoso.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="7fda2-108">For example, if your Cloud Service is named "contoso", your users will be able to access your application on a URL like http://contoso.cloudapp.net.</span></span> <span data-ttu-id="7fda2-109">Azure také přiřadí virtuální IP adresu.</span><span class="sxs-lookup"><span data-stu-id="7fda2-109">Azure also assigns a virtual IP address.</span></span>

<span data-ttu-id="7fda2-110">Ale můžete také vystavit vaší aplikace na svůj vlastní název domény, jako například **contoso.com**. Tento článek vysvětluje, jak chcete rezervovat nebo konfigurovat vlastní název domény pro cloudové služby webové role.</span><span class="sxs-lookup"><span data-stu-id="7fda2-110">However, you can also expose your application on your own domain name, such as **contoso.com**. This article explains how to reserve or configure a custom domain name for Cloud Service web roles.</span></span>

<span data-ttu-id="7fda2-111">Již rozumíte co CNAME záznamy a A jsou?</span><span class="sxs-lookup"><span data-stu-id="7fda2-111">Do you already understand what CNAME and A records are?</span></span> <span data-ttu-id="7fda2-112">[Přechod za vysvětlení](#add-a-cname-record-for-your-custom-domain).</span><span class="sxs-lookup"><span data-stu-id="7fda2-112">[Jump past the explanation](#add-a-cname-record-for-your-custom-domain).</span></span>

> [!NOTE]
> <span data-ttu-id="7fda2-113">Postupy v této úloze platí pro Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="7fda2-113">The procedures in this task apply to Azure Cloud Services.</span></span> <span data-ttu-id="7fda2-114">Aplikační služby, najdete v části [to](../app-service-web/web-sites-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="7fda2-114">For App Services, see [this](../app-service-web/web-sites-custom-domain-name.md).</span></span> <span data-ttu-id="7fda2-115">Účty úložiště, najdete v části [to](../storage/blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="7fda2-115">For storage accounts, see [this](../storage/blobs/storage-custom-domain-name.md).</span></span>
> 
> 

<p/>

> [!TIP]
> <span data-ttu-id="7fda2-116">Rychlejší – začít používat nový Azure [návod na základě](http://support.microsoft.com/kb/2990804)!</span><span class="sxs-lookup"><span data-stu-id="7fda2-116">Get going faster--use the NEW Azure [guided walkthrough](http://support.microsoft.com/kb/2990804)!</span></span>  <span data-ttu-id="7fda2-117">S Azure Cloud Services nebo weby Azure moduly snap umožňuje přiřazení vlastního názvu domény a zabezpečení komunikace (SSL).</span><span class="sxs-lookup"><span data-stu-id="7fda2-117">It makes associating a custom domain name AND securing communication (SSL) with Azure Cloud Services or Azure Websites a snap.</span></span>
> 
> 

## <a name="understand-cname-and-a-records"></a><span data-ttu-id="7fda2-118">Pochopení záznamy CNAME a A</span><span class="sxs-lookup"><span data-stu-id="7fda2-118">Understand CNAME and A records</span></span>
<span data-ttu-id="7fda2-119">CNAME (nebo alias záznamů) a záznamy oba umožňují přidružit název domény pro konkrétní server (nebo v tomto případě služby) ale fungují jinak.</span><span class="sxs-lookup"><span data-stu-id="7fda2-119">CNAME (or alias records) and A records both allow you to associate a domain name with a specific server (or service in this case,) however they work differently.</span></span> <span data-ttu-id="7fda2-120">Existují zde také některé specifické aspekty při použití záznamy A službou Azure Cloud services, které byste měli zvážit, než se rozhodnete, který se použije.</span><span class="sxs-lookup"><span data-stu-id="7fda2-120">There are also some specific considerations when using A records with Azure Cloud services that you should consider before deciding which to use.</span></span>

### <a name="cname-or-alias-record"></a><span data-ttu-id="7fda2-121">Záznam CNAME nebo Alias</span><span class="sxs-lookup"><span data-stu-id="7fda2-121">CNAME or Alias record</span></span>
<span data-ttu-id="7fda2-122">Mapuje záznam CNAME *konkrétní* domény, například **contoso.com** nebo **www.contoso.com**, na název domény kanonickém tvaru.</span><span class="sxs-lookup"><span data-stu-id="7fda2-122">A CNAME record maps a *specific* domain, such as **contoso.com** or **www.contoso.com**, to a canonical domain name.</span></span> <span data-ttu-id="7fda2-123">V takovém případě je název domény kanonický **[myapp] .cloudapp .net** název domény vaší Azure hostované aplikace.</span><span class="sxs-lookup"><span data-stu-id="7fda2-123">In this case, the canonical domain name is the **[myapp].cloudapp.net** domain name of your Azure hosted application.</span></span> <span data-ttu-id="7fda2-124">Po vytvoření CNAME tento alias **[myapp] .cloudapp .net**.</span><span class="sxs-lookup"><span data-stu-id="7fda2-124">Once created, the CNAME creates an alias for the **[myapp].cloudapp.net**.</span></span> <span data-ttu-id="7fda2-125">Záznam CNAME se přeložit na IP adresu vašeho **[myapp] .cloudapp .net** služby automaticky, takže pokud se změní IP adresu cloudové služby, není nutné provádět žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="7fda2-125">The CNAME entry will resolve to the IP address of your **[myapp].cloudapp.net** service automatically, so if the IP address of the cloud service changes, you do not have to take any action.</span></span>

> [!NOTE]
> <span data-ttu-id="7fda2-126">Některé domény registrátorů Povolit jenom vám umožní mapovat subdomény při použití záznam CNAME, třeba www.contoso.com a není kořenové názvy, například contoso.com. Další informace o záznamy CNAME, naleznete v dokumentaci vaším registrátorem [položku Wikipedia na záznam CNAME](http://en.wikipedia.org/wiki/CNAME_record), nebo [IETF názvy domén – implementace a specifikace](http://tools.ietf.org/html/rfc1035) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7fda2-126">Some domain registrars only allow you to map subdomains when using a CNAME record, such as www.contoso.com, and not root names, such as contoso.com. For more information on CNAME records, see the documentation provided by your registrar, [the Wikipedia entry on CNAME record](http://en.wikipedia.org/wiki/CNAME_record), or the [IETF Domain Names - Implementation and Specification](http://tools.ietf.org/html/rfc1035) document.</span></span>
> 
> 

### <a name="a-record"></a><span data-ttu-id="7fda2-127">Záznam</span><span class="sxs-lookup"><span data-stu-id="7fda2-127">A record</span></span>
<span data-ttu-id="7fda2-128">*A* záznam mapuje domény, jako například **contoso.com** nebo **www.contoso.com**, *nebo zástupné domény* například  **\*. contoso.com**, IP adresu.</span><span class="sxs-lookup"><span data-stu-id="7fda2-128">An *A* record maps a domain, such as **contoso.com** or **www.contoso.com**, *or a wildcard domain* such as **\*.contoso.com**, to an IP address.</span></span> <span data-ttu-id="7fda2-129">V případě služby Azure, cloudové virtuální IP adresy služby.</span><span class="sxs-lookup"><span data-stu-id="7fda2-129">In the case of an Azure Cloud Service, the virtual IP of the service.</span></span> <span data-ttu-id="7fda2-130">Hlavní výhodou záznam přes záznam CNAME je, že můžete mít jednu položku, která používá zástupný znak, jako například \* **. contoso.com**, který by například zpracovávat požadavky pro více subdomén  **mail.contoso.com**, **login.contoso.com**, nebo **www.contso.com**.</span><span class="sxs-lookup"><span data-stu-id="7fda2-130">So the main benefit of an A record over a CNAME record is that you can have one entry that uses a wildcard, such as \***.contoso.com**, which would handle requests for multiple sub-domains such as **mail.contoso.com**, **login.contoso.com**, or **www.contso.com**.</span></span>

> [!NOTE]
> <span data-ttu-id="7fda2-131">Vzhledem k tomu, že záznam A je namapovaná na statickou IP adresu, nedokáže automaticky vyřešit změny na IP adresu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7fda2-131">Since an A record is mapped to a static IP address, it cannot automatically resolve changes to the IP address of your Cloud Service.</span></span> <span data-ttu-id="7fda2-132">IP adresu používanou službou cloudové služby je přidělen poprvé, kterého nasadíte do prázdné přihrádky (produkční nebo pracovní.) Pokud odstraníte nasazení pro přihrádky, IP adresa se neuvolní Azure a všechna budoucí nasazení do přihrádky udělit novou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="7fda2-132">The IP address used by your Cloud Service is allocated the first time you deploy to an empty slot (either production or staging.) If you delete the deployment for the slot, the IP address is released by Azure and any future deployments to the slot may be given a new IP address.</span></span>
> 
> <span data-ttu-id="7fda2-133">Pohodlně IP adresu z daného nasazovací slot (produkční nebo pracovní) je nastavené jako trvalé při odkládací mezi pracovním a nasazení v produkčním prostředí nebo provedení místního upgradu existujícího nasazení.</span><span class="sxs-lookup"><span data-stu-id="7fda2-133">Conveniently, the IP address of a given deployment slot (production or staging) is persisted when swapping between staging and production deployments or performing an in-place upgrade of an existing deployment.</span></span> <span data-ttu-id="7fda2-134">Další informace o provedení těchto akcí naleznete v tématu [Správa cloudových služeb](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="7fda2-134">For more information on performing these actions, see [How to manage cloud services](cloud-services-how-to-manage.md).</span></span>
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="7fda2-135">Přidejte záznam CNAME pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="7fda2-135">Add a CNAME record for your custom domain</span></span>
<span data-ttu-id="7fda2-136">Pokud chcete vytvořit záznam CNAME, musí přidáte nový záznam v tabulce DNS pro vaši vlastní doménu pomocí nástroje poskytované subsystémem registrátora.</span><span class="sxs-lookup"><span data-stu-id="7fda2-136">To create a CNAME record, you must add a new entry in the DNS table for your custom domain by using the tools provided by your registrar.</span></span> <span data-ttu-id="7fda2-137">Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam CNAME, ale koncepty jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="7fda2-137">Each registrar has a similar but slightly different method of specifying a CNAME record, but the concepts are the same.</span></span>

1. <span data-ttu-id="7fda2-138">Použijte jednu z těchto metod najít **. cloudapp.net** název domény, které jsou přiřazené ke cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="7fda2-138">Use one of these methods to find the **.cloudapp.net** domain name assigned to your cloud service.</span></span>
   
   * <span data-ttu-id="7fda2-139">Přihlášení k [portál Azure], vyberte cloudovou službu, podívejte se na **Essentials** části a pak vyhledejte **adresa URL webu** položku.</span><span class="sxs-lookup"><span data-stu-id="7fda2-139">Login to the [Azure portal], select your cloud service, look at the **Essentials** section and then find the **Site URL** entry.</span></span>
     
       ![část rychlého přehledu zobrazující adresa URL webu][csurl]
     
       <span data-ttu-id="7fda2-141">**OR**</span><span class="sxs-lookup"><span data-stu-id="7fda2-141">**OR**</span></span>
   * <span data-ttu-id="7fda2-142">Instalace a konfigurace [prostředí Azure Powershell](/powershell/azure/overview)a potom použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fda2-142">Install and configure [Azure Powershell](/powershell/azure/overview), and then use the following command:</span></span>
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     <span data-ttu-id="7fda2-143">Uložte název domény používaný v adrese URL vrácené metody, jak budete ho potřebovat při vytváření záznamu CNAME.</span><span class="sxs-lookup"><span data-stu-id="7fda2-143">Save the domain name used in the URL returned by either method, as you will need it when creating a CNAME record.</span></span>
2. <span data-ttu-id="7fda2-144">Přihlaste se k webu registrátora DNS a přejděte na stránku pro správu DNS.</span><span class="sxs-lookup"><span data-stu-id="7fda2-144">Log on to your DNS registrar's website and go to the page for managing DNS.</span></span> <span data-ttu-id="7fda2-145">Vyhledejte odkazy nebo oblasti lokality označený jako **název domény**, **DNS**, nebo **název serveru správy**.</span><span class="sxs-lookup"><span data-stu-id="7fda2-145">Look for links or areas of the site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="7fda2-146">Nyní kde můžete vyhledejte vyberte nebo zadejte na CNAME.</span><span class="sxs-lookup"><span data-stu-id="7fda2-146">Now find where you can select or enter CNAME's.</span></span> <span data-ttu-id="7fda2-147">Možná budete muset vybrat typ záznamu rozevíracím dolů, nebo přejděte na stránku Upřesnit nastavení.</span><span class="sxs-lookup"><span data-stu-id="7fda2-147">You may have to select the record type from a drop down, or go to an advanced settings page.</span></span> <span data-ttu-id="7fda2-148">Je vhodné vyhledat slova **CNAME**, **Alias**, nebo **subdomény**.</span><span class="sxs-lookup"><span data-stu-id="7fda2-148">You should look for the words **CNAME**, **Alias**, or **Subdomains**.</span></span>
4. <span data-ttu-id="7fda2-149">Je rovněž nutné poskytnout doménu nebo subdomény alias CNAME, jako například **www** Pokud chcete vytvořit alias **www.customdomain.com**. Pokud chcete vytvořit alias pro kořenovou doménu, nemusí být zobrazeny jako '**@**' symbol v nástrojích DNS vašeho registrátora.</span><span class="sxs-lookup"><span data-stu-id="7fda2-149">You must also provide the domain or subdomain alias for the CNAME, such as **www** if you want to create an alias for **www.customdomain.com**. If you want to create an alias for the root domain, it may be listed as the '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="7fda2-150">Potom je nutné zadat název kanonický hostitele, který je vaše aplikace **cloudapp.net** domény v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="7fda2-150">Then, you must provide a canonical host name, which is your application's **cloudapp.net** domain in this case.</span></span>

<span data-ttu-id="7fda2-151">Například následující záznam CNAME předává veškerý provoz z **www.contoso.com** k **contoso.cloudapp.net**, vlastní název domény nasazené aplikace:</span><span class="sxs-lookup"><span data-stu-id="7fda2-151">For example, the following CNAME record forwards all traffic from **www.contoso.com** to **contoso.cloudapp.net**, the custom domain name of your deployed application:</span></span>

| <span data-ttu-id="7fda2-152">Název aliasu/hostitele nebo subdomény</span><span class="sxs-lookup"><span data-stu-id="7fda2-152">Alias/Host name/Subdomain</span></span> | <span data-ttu-id="7fda2-153">Kanonický domény</span><span class="sxs-lookup"><span data-stu-id="7fda2-153">Canonical domain</span></span> |
| --- | --- |
| <span data-ttu-id="7fda2-154">www</span><span class="sxs-lookup"><span data-stu-id="7fda2-154">www</span></span> |<span data-ttu-id="7fda2-155">contoso.cloudapp.NET</span><span class="sxs-lookup"><span data-stu-id="7fda2-155">contoso.cloudapp.net</span></span> |

> [!NOTE]
> <span data-ttu-id="7fda2-156">Návštěvník z **www.contoso.com** nikdy zobrazit true hostitele (contoso.cloudapp.net), tak, aby procesu předávání neviditelná pro koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="7fda2-156">A visitor of **www.contoso.com** will never see the true host (contoso.cloudapp.net), so the forwarding process is invisible to the end user.</span></span>
> 
> <span data-ttu-id="7fda2-157">V předchozím příkladu platí pouze pro provoz na **www** subdomény.</span><span class="sxs-lookup"><span data-stu-id="7fda2-157">The example above only applies to traffic at the **www** subdomain.</span></span> <span data-ttu-id="7fda2-158">Vzhledem k tomu, že se záznamy CNAME nelze použít zástupné znaky, je třeba vytvořit jednu CNAME pro každou doménu subdomény.</span><span class="sxs-lookup"><span data-stu-id="7fda2-158">Since you cannot use wildcards with CNAME records, you must create one CNAME for each domain/subdomain.</span></span> <span data-ttu-id="7fda2-159">Pokud chcete směrovat přenosy z subdomény, například *. contoso.com, na vaši adresu cloudapp.net můžete nakonfigurovat **adresy URL přesměrování** nebo **dál adresy URL** položku v nastavení DNS, nebo vytvořit záznam.</span><span class="sxs-lookup"><span data-stu-id="7fda2-159">If you want to direct  traffic from subdomains, such as *.contoso.com, to your cloudapp.net address, you can configure a **URL Redirect** or **URL Forward** entry in your DNS settings, or create an A record.</span></span>
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a><span data-ttu-id="7fda2-160">Přidání záznamu A pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="7fda2-160">Add an A record for your custom domain</span></span>
<span data-ttu-id="7fda2-161">Pokud chcete vytvořit záznam A, je nutné nejprve vyhledat virtuální IP adresy cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7fda2-161">To create an A record, you must first find the virtual IP address of your cloud service.</span></span> <span data-ttu-id="7fda2-162">Potom přidejte nový záznam v tabulce DNS pro vaši vlastní doménu pomocí nástroje poskytované subsystémem registrátora.</span><span class="sxs-lookup"><span data-stu-id="7fda2-162">Then add a new entry in the DNS table for your custom domain by using the tools provided by your registrar.</span></span> <span data-ttu-id="7fda2-163">Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam A, ale koncepty jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="7fda2-163">Each registrar has a similar but slightly different method of specifying an A record, but the concepts are the same.</span></span>

1. <span data-ttu-id="7fda2-164">Použijte jednu z následujících metod k získání IP adresy cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7fda2-164">Use one of the following methods to get the IP address of your cloud service.</span></span>
   
   * <span data-ttu-id="7fda2-165">Přihlášení k [portál Azure], vyberte cloudovou službu, podívejte se na **Essentials** části a pak vyhledejte **veřejné IP adresy** položku.</span><span class="sxs-lookup"><span data-stu-id="7fda2-165">Login to the [Azure portal], select your cloud service, look at the **Essentials** section and then find the **Public IP addresses** entry.</span></span>
     
       ![Zobrazuje virtuální IP adresu části rychlého přehledu.][vip]
     
       <span data-ttu-id="7fda2-167">**OR**</span><span class="sxs-lookup"><span data-stu-id="7fda2-167">**OR**</span></span>
   * <span data-ttu-id="7fda2-168">Instalace a konfigurace [prostředí Azure Powershell](/powershell/azure/overview)a potom použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7fda2-168">Install and configure [Azure Powershell](/powershell/azure/overview), and then use the following command:</span></span>
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     <span data-ttu-id="7fda2-169">IP adresa, uložte, protože jej budete potřebovat při vytváření záznam.</span><span class="sxs-lookup"><span data-stu-id="7fda2-169">Save the IP address, as you will need it when creating an A record.</span></span>
2. <span data-ttu-id="7fda2-170">Přihlaste se k webu registrátora DNS a přejděte na stránku pro správu DNS.</span><span class="sxs-lookup"><span data-stu-id="7fda2-170">Log on to your DNS registrar's website and go to the page for managing DNS.</span></span> <span data-ttu-id="7fda2-171">Vyhledejte odkazy nebo oblasti lokality označený jako **název domény**, **DNS**, nebo **název serveru správy**.</span><span class="sxs-lookup"><span data-stu-id="7fda2-171">Look for links or areas of the site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="7fda2-172">Nyní najdete, kde můžete vyberte nebo zadejte záznamu.</span><span class="sxs-lookup"><span data-stu-id="7fda2-172">Now find where you can select or enter A record's.</span></span> <span data-ttu-id="7fda2-173">Možná budete muset vybrat typ záznamu rozevíracím dolů, nebo přejděte na stránku Upřesnit nastavení.</span><span class="sxs-lookup"><span data-stu-id="7fda2-173">You may have to select the record type from a drop down, or go to an advanced settings page.</span></span>
4. <span data-ttu-id="7fda2-174">Vyberte nebo zadejte doménu nebo subdomény, které budou používat tento záznam A.</span><span class="sxs-lookup"><span data-stu-id="7fda2-174">Select or enter the domain or subdomain that will use this A record.</span></span> <span data-ttu-id="7fda2-175">Vyberte například **www** Pokud chcete vytvořit alias **www.customdomain.com**. Pokud chcete vytvořit záznam zástupných znaků pro všechny subdomény, zadejte "***'.</span><span class="sxs-lookup"><span data-stu-id="7fda2-175">For example, select **www** if you want to create an alias for **www.customdomain.com**. If you want to create a wildcard entry for all subdomains, enter '*****'.</span></span> <span data-ttu-id="7fda2-176">Ten přináší všem dílčím doménám domény, jako **mail.customdomain.com**, **login.customdomain.com**, a **www.customdomain.com**.</span><span class="sxs-lookup"><span data-stu-id="7fda2-176">This will cover all sub-domains such as **mail.customdomain.com**, **login.customdomain.com**, and **www.customdomain.com**.</span></span>
   
    <span data-ttu-id="7fda2-177">Pokud chcete vytvořit záznam pro kořenovou doménu, nemusí být zobrazeny jako '**@**' symbol v nástrojích DNS vašeho registrátora.</span><span class="sxs-lookup"><span data-stu-id="7fda2-177">If you want to create an A record for the root domain, it may be listed as the '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="7fda2-178">Zadejte IP adresu cloudové služby v zadané pole.</span><span class="sxs-lookup"><span data-stu-id="7fda2-178">Enter the IP address of your cloud service in the provided field.</span></span> <span data-ttu-id="7fda2-179">To přiřadí položku domény použít v záznam A s IP adresou nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7fda2-179">This associates the domain entry used in the A record with the IP address of your cloud service deployment.</span></span>

<span data-ttu-id="7fda2-180">Například následující záznam předává veškerý provoz z **contoso.com** k **137.135.70.239**, IP adresu nasazené aplikace:</span><span class="sxs-lookup"><span data-stu-id="7fda2-180">For example, the following A record forwards all traffic from **contoso.com** to **137.135.70.239**, the IP address of your deployed application:</span></span>

| <span data-ttu-id="7fda2-181">Název hostitele nebo subdomény</span><span class="sxs-lookup"><span data-stu-id="7fda2-181">Host name/Subdomain</span></span> | <span data-ttu-id="7fda2-182">IP adresa</span><span class="sxs-lookup"><span data-stu-id="7fda2-182">IP address</span></span> |
| --- | --- |
| @ |<span data-ttu-id="7fda2-183">137.135.70.239</span><span class="sxs-lookup"><span data-stu-id="7fda2-183">137.135.70.239</span></span> |

<span data-ttu-id="7fda2-184">Tento příklad ukazuje vytvoření záznamu A pro kořenovou doménu.</span><span class="sxs-lookup"><span data-stu-id="7fda2-184">This example demonstrates creating an A record for the root domain.</span></span> <span data-ttu-id="7fda2-185">Pokud chcete vytvořit záznam zástupný znak nepokrývají všechny subdomény, zadali byste ' ***' jako subdoméně.</span><span class="sxs-lookup"><span data-stu-id="7fda2-185">If you wish to create a wildcard entry to cover all subdomains, you would enter '*****' as the subdomain.</span></span>

> [!WARNING]
> <span data-ttu-id="7fda2-186">IP adresy v Azure jsou ve výchozím nastavení dynamické.</span><span class="sxs-lookup"><span data-stu-id="7fda2-186">IP addresses in Azure are dynamic by default.</span></span> <span data-ttu-id="7fda2-187">Budete pravděpodobně chtít použít [vyhrazená IP adresa](../virtual-network/virtual-networks-reserved-public-ip.md) zajistit, že IP adresa se nemění.</span><span class="sxs-lookup"><span data-stu-id="7fda2-187">You will probably want to use a [reserved IP address](../virtual-network/virtual-networks-reserved-public-ip.md) to ensure that your IP address does not change.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="7fda2-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7fda2-188">Next steps</span></span>
* [<span data-ttu-id="7fda2-189">Jak spravovat Cloud Services</span><span class="sxs-lookup"><span data-stu-id="7fda2-189">How to Manage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="7fda2-190">Postup mapování obsahu CDN do vlastní domény</span><span class="sxs-lookup"><span data-stu-id="7fda2-190">How to Map CDN Content to a Custom Domain</span></span>](../cdn/cdn-map-content-to-custom-domain.md)
* <span data-ttu-id="7fda2-191">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7fda2-191">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="7fda2-192">Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7fda2-192">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="7fda2-193">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7fda2-193">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[portál Azure]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
