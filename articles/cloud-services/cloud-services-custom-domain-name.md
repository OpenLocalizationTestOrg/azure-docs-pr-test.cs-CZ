---
title: "aaaConfigure vlastního názvu domény v cloudové služby | Microsoft Docs"
description: "Zjistěte, jak tooexpose aplikaci Azure nebo data na vlastní domény tak, že nakonfigurujete nastavení DNS."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 6a62c2b7-ea47-4cce-9d6a-0cca38357f42
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 71e553a73b40a8d0512b4d40173500561841772c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a><span data-ttu-id="8db5f-103">Konfigurace vlastního názvu domény pro cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="8db5f-103">Configuring a custom domain name for an Azure cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8db5f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8db5f-104">Azure portal</span></span>](cloud-services-custom-domain-name-portal.md)
> * [<span data-ttu-id="8db5f-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="8db5f-105">Azure classic portal</span></span>](cloud-services-custom-domain-name.md)
> 
> 

<span data-ttu-id="8db5f-106">Při vytváření cloudové služby Azure přiřadí ji tooa subdoména cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="8db5f-106">When you create a Cloud Service, Azure assigns it tooa subdomain of cloudapp.net.</span></span> <span data-ttu-id="8db5f-107">Například pokud cloudové služby má název "contoso", uživatelé budou se moct tooaccess vaší aplikace na adresu URL podobnou http://contoso.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="8db5f-107">For example, if your Cloud Service is named "contoso", your users will be able tooaccess your application on a URL like http://contoso.cloudapp.net.</span></span> <span data-ttu-id="8db5f-108">Azure také přiřadí virtuální IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8db5f-108">Azure also assigns a virtual IP address.</span></span>

<span data-ttu-id="8db5f-109">Můžete však také vystavit vaší aplikace na svůj vlastní název domény, například contoso.com. Tento článek vysvětluje, jak tooreserve nebo nakonfigurovat vlastní název domény pro cloudové služby webové role.</span><span class="sxs-lookup"><span data-stu-id="8db5f-109">However, you can also expose your application on your own domain name, such as contoso.com. This article explains how tooreserve or configure a custom domain name for Cloud Service web roles.</span></span>

<span data-ttu-id="8db5f-110">Již rozumíte co CNAME záznamy a A jsou?</span><span class="sxs-lookup"><span data-stu-id="8db5f-110">Do you already understand what CNAME and A records are?</span></span> <span data-ttu-id="8db5f-111">[Přechod za hello vysvětlení](#add-a-cname-record-for-your-custom-domain).</span><span class="sxs-lookup"><span data-stu-id="8db5f-111">[Jump past hello explanation](#add-a-cname-record-for-your-custom-domain).</span></span>

> [!NOTE]
> <span data-ttu-id="8db5f-112">Získání budete rychlejší!</span><span class="sxs-lookup"><span data-stu-id="8db5f-112">Get going faster!</span></span> <span data-ttu-id="8db5f-113">Použití hello Azure [návod na základě](http://support.microsoft.com/kb/2990804).</span><span class="sxs-lookup"><span data-stu-id="8db5f-113">Use hello Azure [guided walkthrough](http://support.microsoft.com/kb/2990804).</span></span> <span data-ttu-id="8db5f-114">Umožňuje přiřazení vlastního názvu domény a zabezpečení komunikace (SSL) s Azure Cloud Services nebo weby Azure moduly snap.</span><span class="sxs-lookup"><span data-stu-id="8db5f-114">It makes associating a custom domain name and securing communication (SSL) with Azure Cloud Services or Azure Websites a snap.</span></span>
> 
> 

<p/>

> [!NOTE]
> <span data-ttu-id="8db5f-115">Hello postupy v této úloze platí tooAzure cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8db5f-115">hello procedures in this task apply tooAzure Cloud Services.</span></span> <span data-ttu-id="8db5f-116">Aplikační služby, najdete v části [to](../app-service-web/web-sites-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="8db5f-116">For App Services, see [this](../app-service-web/web-sites-custom-domain-name.md).</span></span> <span data-ttu-id="8db5f-117">Účty úložiště, najdete v části [to](../storage/blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="8db5f-117">For storage accounts, see [this](../storage/blobs/storage-custom-domain-name.md).</span></span>
> 
> 

## <a name="understand-cname-and-a-records"></a><span data-ttu-id="8db5f-118">Pochopení záznamy CNAME a A</span><span class="sxs-lookup"><span data-stu-id="8db5f-118">Understand CNAME and A records</span></span>
<span data-ttu-id="8db5f-119">CNAME (nebo alias záznamů) a záznamy A umožňují tooassociate názvu domény s konkrétní server (i v tomto případě služby) ale fungují jinak.</span><span class="sxs-lookup"><span data-stu-id="8db5f-119">CNAME (or alias records) and A records both allow you tooassociate a domain name with a specific server (or service in this case,) however they work differently.</span></span> <span data-ttu-id="8db5f-120">Existují zde také některé specifické aspekty při použití záznamy A službou Azure Cloud services, které byste měli zvážit, než se rozhodnete, které toouse.</span><span class="sxs-lookup"><span data-stu-id="8db5f-120">There are also some specific considerations when using A records with Azure Cloud services that you should consider before deciding which toouse.</span></span>

### <a name="cname-or-alias-record"></a><span data-ttu-id="8db5f-121">Záznam CNAME nebo Alias</span><span class="sxs-lookup"><span data-stu-id="8db5f-121">CNAME or Alias record</span></span>
<span data-ttu-id="8db5f-122">Mapuje záznam CNAME *konkrétní* domény, například **contoso.com** nebo **www.contoso.com**, název tooa kanonický domény.</span><span class="sxs-lookup"><span data-stu-id="8db5f-122">A CNAME record maps a *specific* domain, such as **contoso.com** or **www.contoso.com**, tooa canonical domain name.</span></span> <span data-ttu-id="8db5f-123">V takovém případě je název kanonický domény hello hello **[myapp] .cloudapp .net** název domény vaší Azure hostované aplikace.</span><span class="sxs-lookup"><span data-stu-id="8db5f-123">In this case, hello canonical domain name is hello **[myapp].cloudapp.net** domain name of your Azure hosted application.</span></span> <span data-ttu-id="8db5f-124">Po vytvoření hello CNAME tento alias hello **[myapp] .cloudapp .net**.</span><span class="sxs-lookup"><span data-stu-id="8db5f-124">Once created, hello CNAME creates an alias for hello **[myapp].cloudapp.net**.</span></span> <span data-ttu-id="8db5f-125">Hello záznam CNAME se přeložit IP adresu toohello vaše **[myapp] .cloudapp .net** služby automaticky, takže pokud se změní IP adresu hello hello cloudové služby, není nutné tootake žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="8db5f-125">hello CNAME entry will resolve toohello IP address of your **[myapp].cloudapp.net** service automatically, so if hello IP address of hello cloud service changes, you do not have tootake any action.</span></span>

> [!NOTE]
> <span data-ttu-id="8db5f-126">Některé domény registrátorů umožňují pouze toomap subdomény při použití záznam CNAME, třeba www.contoso.com a není kořenové názvy, například contoso.com. Další informace o záznamy CNAME, naleznete v dokumentaci k hello poskytované vašeho registrátora [hello Wikipedia položku na záznam CNAME](http://en.wikipedia.org/wiki/CNAME_record), nebo hello [IETF názvy domén – implementace a specifikace](http://tools.ietf.org/html/rfc1035) dokument.</span><span class="sxs-lookup"><span data-stu-id="8db5f-126">Some domain registrars only allow you toomap subdomains when using a CNAME record, such as www.contoso.com, and not root names, such as contoso.com. For more information on CNAME records, see hello documentation provided by your registrar, [hello Wikipedia entry on CNAME record](http://en.wikipedia.org/wiki/CNAME_record), or hello [IETF Domain Names - Implementation and Specification](http://tools.ietf.org/html/rfc1035) document.</span></span>
> 
> 

### <a name="a-record"></a><span data-ttu-id="8db5f-127">Záznam</span><span class="sxs-lookup"><span data-stu-id="8db5f-127">A record</span></span>
<span data-ttu-id="8db5f-128">Záznam A mapuje domény, jako například **contoso.com** nebo **www.contoso.com**, *nebo zástupné domény* například  **\*. contoso.com**, tooan IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8db5f-128">An A record maps a domain, such as **contoso.com** or **www.contoso.com**, *or a wildcard domain* such as **\*.contoso.com**, tooan IP address.</span></span> <span data-ttu-id="8db5f-129">V případě hello cloudové služby Azure hello virtuální IP adresy služby hello.</span><span class="sxs-lookup"><span data-stu-id="8db5f-129">In hello case of an Azure Cloud Service, hello virtual IP of hello service.</span></span> <span data-ttu-id="8db5f-130">Takže hello hlavní výhodou záznam přes záznam CNAME je, že můžete mít jednu položku, která používá zástupný znak, jako například \* **. contoso.com**, který by například zpracovávat požadavky pro více subdomén  **mail.contoso.com**, **login.contoso.com**, nebo **www.contso.com**.</span><span class="sxs-lookup"><span data-stu-id="8db5f-130">So hello main benefit of an A record over a CNAME record is that you can have one entry that uses a wildcard, such as \***.contoso.com**, which would handle requests for multiple sub-domains such as **mail.contoso.com**, **login.contoso.com**, or **www.contso.com**.</span></span>

> [!NOTE]
> <span data-ttu-id="8db5f-131">Vzhledem k tomu, že je namapovaný záznam tooa statickou IP adresu nelze vyřešit automaticky změny toohello IP adresu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8db5f-131">Since an A record is mapped tooa static IP address, it cannot automatically resolve changes toohello IP address of your Cloud Service.</span></span> <span data-ttu-id="8db5f-132">Hello IP adresu používanou službou cloudové služby je přidělen hello prvním nasazení tooan prázdné přihrádky (produkční nebo pracovní.) Pokud odstraníte nasazení hello pro hello slot, hello IP adresa se neuvolní Azure a slotu toohello všechny budoucí nasazení může být poskytnut novou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8db5f-132">hello IP address used by your Cloud Service is allocated hello first time you deploy tooan empty slot (either production or staging.) If you delete hello deployment for hello slot, hello IP address is released by Azure and any future deployments toohello slot may be given a new IP address.</span></span>
> 
> <span data-ttu-id="8db5f-133">Pohodlně hello IP adresu z daného nasazovací slot (produkční nebo pracovní) je nastavené jako trvalé při odkládací mezi pracovním a nasazení v produkčním prostředí nebo provedení místního upgradu existujícího nasazení.</span><span class="sxs-lookup"><span data-stu-id="8db5f-133">Conveniently, hello IP address of a given deployment slot (production or staging) is persisted when swapping between staging and production deployments or performing an in-place upgrade of an existing deployment.</span></span> <span data-ttu-id="8db5f-134">Další informace o provedení těchto akcí naleznete v tématu [jak toomanage cloudových služeb](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="8db5f-134">For more information on performing these actions, see [How toomanage cloud services](cloud-services-how-to-manage.md).</span></span>
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="8db5f-135">Přidejte záznam CNAME pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="8db5f-135">Add a CNAME record for your custom domain</span></span>
<span data-ttu-id="8db5f-136">toocreate záznam CNAME, je musí přidat nový záznam v tabulce hello DNS pro vaši vlastní doménu s použitím hello nástroje poskytované subsystémem registrátora.</span><span class="sxs-lookup"><span data-stu-id="8db5f-136">toocreate a CNAME record, you must add a new entry in hello DNS table for your custom domain by using hello tools provided by your registrar.</span></span> <span data-ttu-id="8db5f-137">Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam CNAME, ale hello hello koncepty jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="8db5f-137">Each registrar has a similar but slightly different method of specifying a CNAME record, but hello concepts are hello same.</span></span>

1. <span data-ttu-id="8db5f-138">Použijte jednu z těchto metod toofind hello **. cloudapp.net** název domény, které jsou přiřazené tooyour cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8db5f-138">Use one of these methods toofind hello **.cloudapp.net** domain name assigned tooyour cloud service.</span></span>
   
   * <span data-ttu-id="8db5f-139">Přihlášení toohello [portál Azure classic]vyberte cloudové služby, vyberte **řídicí panel**a pak vyhledejte hello **adresa URL webu** položku v hello **rychlého přehledu.**  části.</span><span class="sxs-lookup"><span data-stu-id="8db5f-139">Login toohello [Azure classic portal], select your cloud service, select **Dashboard**, and then find hello **Site URL** entry in hello **quick glance** section.</span></span>
     
       ![Zobrazuje adresu URL webu hello části rychlého přehledu.][csurl]
     
       <span data-ttu-id="8db5f-141">**OR**</span><span class="sxs-lookup"><span data-stu-id="8db5f-141">**OR**</span></span>  
   * <span data-ttu-id="8db5f-142">Instalace a konfigurace [prostředí Azure Powershell](/powershell/azure/overview), a pak hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8db5f-142">Install and configure [Azure Powershell](/powershell/azure/overview), and then use hello following command:</span></span>
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     <span data-ttu-id="8db5f-143">Uložte hello název domény používaný v URL ADRESE hello vráceném buď metodou, jak budete ho potřebovat při vytváření záznamu CNAME.</span><span class="sxs-lookup"><span data-stu-id="8db5f-143">Save hello domain name used in hello URL returned by either method, as you will need it when creating a CNAME record.</span></span>
2. <span data-ttu-id="8db5f-144">Přihlaste se na webu registrátora DNS tooyour a přejděte toohello stránku pro správu DNS.</span><span class="sxs-lookup"><span data-stu-id="8db5f-144">Log on tooyour DNS registrar's website and go toohello page for managing DNS.</span></span> <span data-ttu-id="8db5f-145">Vyhledejte odkazy nebo oblasti lokality hello označený jako **název domény**, **DNS**, nebo **název serveru správy**.</span><span class="sxs-lookup"><span data-stu-id="8db5f-145">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="8db5f-146">Nyní kde můžete vyhledejte vyberte nebo zadejte na CNAME.</span><span class="sxs-lookup"><span data-stu-id="8db5f-146">Now find where you can select or enter CNAME's.</span></span> <span data-ttu-id="8db5f-147">Můžete mít tooselect hello záznam typu z rozevíracího seznamu, nebo přejděte tooan Upřesnit nastavení stránky.</span><span class="sxs-lookup"><span data-stu-id="8db5f-147">You may have tooselect hello record type from a drop down, or go tooan advanced settings page.</span></span> <span data-ttu-id="8db5f-148">Je vhodné vyhledat hello slova **CNAME**, **Alias**, nebo **subdomény**.</span><span class="sxs-lookup"><span data-stu-id="8db5f-148">You should look for hello words **CNAME**, **Alias**, or **Subdomains**.</span></span>
4. <span data-ttu-id="8db5f-149">Je rovněž nutné poskytnout hello domény nebo subdomény alias pro hello CNAME, jako například **www** Pokud chcete toocreate alias **www.customdomain.com**. Pokud chcete toocreate alias pro hello kořenovou doménu, nemusí být zobrazeny jako hello '**@**' symbol v nástrojích DNS vašeho registrátora.</span><span class="sxs-lookup"><span data-stu-id="8db5f-149">You must also provide hello domain or subdomain alias for hello CNAME, such as **www** if you want toocreate an alias for **www.customdomain.com**. If you want toocreate an alias for hello root domain, it may be listed as hello '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="8db5f-150">Potom je nutné zadat název kanonický hostitele, který je vaše aplikace **cloudapp.net** domény v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="8db5f-150">Then, you must provide a canonical host name, which is your application's **cloudapp.net** domain in this case.</span></span>

<span data-ttu-id="8db5f-151">Například následující záznam CNAME hello předává veškerý provoz z **www.contoso.com** příliš**contoso.cloudapp.net**, hello vlastní název domény nasazené aplikace:</span><span class="sxs-lookup"><span data-stu-id="8db5f-151">For example, hello following CNAME record forwards all traffic from **www.contoso.com** too**contoso.cloudapp.net**, hello custom domain name of your deployed application:</span></span>

| <span data-ttu-id="8db5f-152">Název aliasu/hostitele nebo subdomény</span><span class="sxs-lookup"><span data-stu-id="8db5f-152">Alias/Host name/Subdomain</span></span> | <span data-ttu-id="8db5f-153">Kanonický domény</span><span class="sxs-lookup"><span data-stu-id="8db5f-153">Canonical domain</span></span> |
| --- | --- |
| <span data-ttu-id="8db5f-154">www</span><span class="sxs-lookup"><span data-stu-id="8db5f-154">www</span></span> |<span data-ttu-id="8db5f-155">contoso.cloudapp.NET</span><span class="sxs-lookup"><span data-stu-id="8db5f-155">contoso.cloudapp.net</span></span> |

<span data-ttu-id="8db5f-156">Návštěvník z **www.contoso.com** nikdy zobrazit hello true hostitele (contoso.cloudapp.net), tak, aby hello předávání proces neviditelná toothe koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="8db5f-156">A visitor of **www.contoso.com** will never see hello true host (contoso.cloudapp.net), so hello forwarding process is invisible toothe end user.</span></span>

> [!NOTE]
> <span data-ttu-id="8db5f-157">Hello výše uvedeném příkladu vztahuje pouze na hello tootraffic **www** subdomény.</span><span class="sxs-lookup"><span data-stu-id="8db5f-157">hello example above only applies tootraffic at hello **www** subdomain.</span></span> <span data-ttu-id="8db5f-158">Vzhledem k tomu, že se záznamy CNAME nelze použít zástupné znaky, je třeba vytvořit jednu CNAME pro každou doménu subdomény.</span><span class="sxs-lookup"><span data-stu-id="8db5f-158">Since you cannot use wildcards with CNAME records, you must create one CNAME for each domain/subdomain.</span></span> <span data-ttu-id="8db5f-159">Pokud například chcete toodirect provoz z subdomény, \*. contoso.com, adresa cloudapp.net tooyour, můžete nakonfigurovat **adresa URL přesměrování** nebo **URL dál** položku v nastavení DNS, nebo Vytvořte záznam.</span><span class="sxs-lookup"><span data-stu-id="8db5f-159">If you want toodirect  traffic from subdomains, such as \*.contoso.com, tooyour cloudapp.net address, you can configure a **URL Redirect** or **URL Forward** entry in your DNS settings, or create an A record.</span></span>
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a><span data-ttu-id="8db5f-160">Přidání záznamu A pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="8db5f-160">Add an A record for your custom domain</span></span>
<span data-ttu-id="8db5f-161">toocreate A záznam, je nutné nejprve vyhledat hello virtuální IP adresy cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8db5f-161">toocreate an A record, you must first find hello virtual IP address of your cloud service.</span></span> <span data-ttu-id="8db5f-162">Potom přidejte nový záznam v tabulce hello DNS pro vaši vlastní doménu pomocí hello nástroje poskytované subsystémem registrátora.</span><span class="sxs-lookup"><span data-stu-id="8db5f-162">Then add a new entry in hello DNS table for your custom domain by using hello tools provided by your registrar.</span></span> <span data-ttu-id="8db5f-163">Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam A, ale hello hello koncepty jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="8db5f-163">Each registrar has a similar but slightly different method of specifying an A record, but hello concepts are hello same.</span></span>

1. <span data-ttu-id="8db5f-164">Použijte jeden z hello následující metody tooget hello IP adresu cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8db5f-164">Use one of hello following methods tooget hello IP address of your cloud service.</span></span>
   
   * <span data-ttu-id="8db5f-165">přihlášení toohello [portál Azure classic]vyberte cloudové služby, vyberte **řídicí panel**a pak vyhledejte hello **veřejná virtuální IP (VIP) Adresa** položku v hello **rychlého přehledu** části.</span><span class="sxs-lookup"><span data-stu-id="8db5f-165">login toohello [Azure classic portal], select your cloud service, select **Dashboard**, and then find hello **Public Virtual IP (VIP) address** entry in hello **quick glance** section.</span></span>
     
       ![Zobrazuje hello VIP části rychlého přehledu.][vip]
     
       <span data-ttu-id="8db5f-167">**OR**</span><span class="sxs-lookup"><span data-stu-id="8db5f-167">**OR**</span></span>  
   * <span data-ttu-id="8db5f-168">Instalace a konfigurace [prostředí Azure Powershell](/powershell/azure/overview), a pak hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8db5f-168">Install and configure [Azure Powershell](/powershell/azure/overview), and then use hello following command:</span></span>
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     <span data-ttu-id="8db5f-169">Pokud máte několik koncových bodů, které jsou spojené s cloudovou službou, zobrazí se více řádků obsahujících hello IP adresu, ale všechny by se měl zobrazit hello stejné adresy.</span><span class="sxs-lookup"><span data-stu-id="8db5f-169">If you have multiple endpoints associated with your cloud service, you will receive multiple lines containing hello IP address, but all should display hello same address.</span></span>
     
     <span data-ttu-id="8db5f-170">Hello IP adresu, uložte, protože jej budete potřebovat při vytváření záznam.</span><span class="sxs-lookup"><span data-stu-id="8db5f-170">Save hello IP address, as you will need it when creating an A record.</span></span>
2. <span data-ttu-id="8db5f-171">Přihlaste se na webu registrátora DNS tooyour a přejděte toohello stránku pro správu DNS.</span><span class="sxs-lookup"><span data-stu-id="8db5f-171">Log on tooyour DNS registrar's website and go toohello page for managing DNS.</span></span> <span data-ttu-id="8db5f-172">Vyhledejte odkazy nebo oblasti lokality hello označený jako **název domény**, **DNS**, nebo **název serveru správy**.</span><span class="sxs-lookup"><span data-stu-id="8db5f-172">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="8db5f-173">Nyní najdete, kde můžete vyberte nebo zadejte záznamu.</span><span class="sxs-lookup"><span data-stu-id="8db5f-173">Now find where you can select or enter A record's.</span></span> <span data-ttu-id="8db5f-174">Můžete mít tooselect hello záznam typu z rozevíracího seznamu, nebo přejděte tooan Upřesnit nastavení stránky.</span><span class="sxs-lookup"><span data-stu-id="8db5f-174">You may have tooselect hello record type from a drop down, or go tooan advanced settings page.</span></span>
4. <span data-ttu-id="8db5f-175">Vyberte nebo zadejte doménu hello nebo subdomény, které budou používat tento záznam A.</span><span class="sxs-lookup"><span data-stu-id="8db5f-175">Select or enter hello domain or subdomain that will use this A record.</span></span> <span data-ttu-id="8db5f-176">Vyberte například **www** Pokud chcete toocreate alias **www.customdomain.com**. Pokud chcete toocreate položku zástupných znaků pro všechny subdomény, zadejte "__*__'.</span><span class="sxs-lookup"><span data-stu-id="8db5f-176">For example, select **www** if you want toocreate an alias for **www.customdomain.com**. If you want toocreate a wildcard entry for all subdomains, enter '__*__'.</span></span> <span data-ttu-id="8db5f-177">Ten přináší všem dílčím doménám domény, jako **mail.customdomain.com**, **login.customdomain.com**, a **www.customdomain.com**.</span><span class="sxs-lookup"><span data-stu-id="8db5f-177">This will cover all sub-domains such as **mail.customdomain.com**, **login.customdomain.com**, and **www.customdomain.com**.</span></span>
   
    <span data-ttu-id="8db5f-178">Pokud chcete toocreate A záznam pro hello kořenovou doménu, nemusí být zobrazeny jako hello '**@**' symbol v nástrojích DNS vašeho registrátora.</span><span class="sxs-lookup"><span data-stu-id="8db5f-178">If you want toocreate an A record for hello root domain, it may be listed as hello '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="8db5f-179">Zadejte IP adresu hello cloudové služby v hello zadaná pole.</span><span class="sxs-lookup"><span data-stu-id="8db5f-179">Enter hello IP address of your cloud service in hello provided field.</span></span> <span data-ttu-id="8db5f-180">To přiřadí zadaná doména hello používá v hello záznam A s IP adresou hello nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8db5f-180">This associates hello domain entry used in hello A record with hello IP address of your cloud service deployment.</span></span>

<span data-ttu-id="8db5f-181">Například následující záznam hello předává veškerý provoz z **contoso.com** příliš**137.135.70.239**, hello IP adresu nasazené aplikace:</span><span class="sxs-lookup"><span data-stu-id="8db5f-181">For example, hello following A record forwards all traffic from **contoso.com** too**137.135.70.239**, hello IP address of your deployed application:</span></span>

| <span data-ttu-id="8db5f-182">Název hostitele nebo subdomény</span><span class="sxs-lookup"><span data-stu-id="8db5f-182">Host name/Subdomain</span></span> | <span data-ttu-id="8db5f-183">IP adresa</span><span class="sxs-lookup"><span data-stu-id="8db5f-183">IP address</span></span> |
| --- | --- |
| @ |<span data-ttu-id="8db5f-184">137.135.70.239</span><span class="sxs-lookup"><span data-stu-id="8db5f-184">137.135.70.239</span></span> |

<span data-ttu-id="8db5f-185">Tento příklad ukazuje vytvoření záznamu A pro hello kořenové domény.</span><span class="sxs-lookup"><span data-stu-id="8db5f-185">This example demonstrates creating an A record for hello root domain.</span></span> <span data-ttu-id="8db5f-186">Pokud chcete toocreate zástupný znak položka toocover všechny subdomény, zadali byste '__*__' jako hello subdomény.</span><span class="sxs-lookup"><span data-stu-id="8db5f-186">If you wish toocreate a wildcard entry toocover all subdomains, you would enter '__*__' as hello subdomain.</span></span>

> [!WARNING]
> <span data-ttu-id="8db5f-187">IP adresy v Azure jsou ve výchozím nastavení dynamické.</span><span class="sxs-lookup"><span data-stu-id="8db5f-187">IP addresses in Azure are dynamic by default.</span></span> <span data-ttu-id="8db5f-188">Budete asi chtít toouse [vyhrazená IP adresa](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure, který se nemění IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8db5f-188">You will probably want toouse a [reserved IP address](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure that your IP address does not change.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="8db5f-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8db5f-189">Next steps</span></span>
* [<span data-ttu-id="8db5f-190">Jak tooManage cloudových služeb</span><span class="sxs-lookup"><span data-stu-id="8db5f-190">How tooManage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="8db5f-191">Jak tooMap obsahu CDN tooa vlastní domény</span><span class="sxs-lookup"><span data-stu-id="8db5f-191">How tooMap CDN Content tooa Custom Domain</span></span>](../cdn/cdn-map-content-to-custom-domain.md)
* <span data-ttu-id="8db5f-192">[Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="8db5f-192">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="8db5f-193">Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="8db5f-193">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="8db5f-194">Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="8db5f-194">Configure [ssl certificates](cloud-services-configure-ssl-certificate.md).</span></span>

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates hello subdomain with hello storage account]: #create-cname
[portál Azure classic]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
