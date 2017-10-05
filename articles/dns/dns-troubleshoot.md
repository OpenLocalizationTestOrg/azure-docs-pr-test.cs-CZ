---
title: "Průvodci odstraňováním potíží Azure DNS | Microsoft Docs"
description: "Postup řešení běžných problémů s Azure DNS"
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: 95b01dc3-ee69-4575-a259-4227131e4f9c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/20/2017
ms.author: jonatul
ms.openlocfilehash: 1d9bb681a864bdc3e5a2f9c9a531d9566b16ada4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-dns-troubleshooting-guide"></a><span data-ttu-id="58382-103">Azure DNS Průvodci odstraňováním potíží</span><span class="sxs-lookup"><span data-stu-id="58382-103">Azure DNS troubleshooting guide</span></span>

<span data-ttu-id="58382-104">Tato stránka obsahuje informace o odstraňování potíží pro běžné otázky Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="58382-104">This page provides troubleshooting information for common Azure DNS questions.</span></span>

<span data-ttu-id="58382-105">Pokud tyto kroky problém nevyřeší, můžete také vyhledat nebo zveřejnit svůj problém na našem [Fórum komunity podpory na webu MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span><span class="sxs-lookup"><span data-stu-id="58382-105">If these steps don't resolve your issue, you can also search for or post your issue on our [community support forum on MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span></span> <span data-ttu-id="58382-106">Alternativně můžete otevřete žádost o podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="58382-106">Alternatively, open an Azure support request.</span></span>


## <a name="i-cant-create-a-dns-zone"></a><span data-ttu-id="58382-107">Nelze vytvořit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="58382-107">I can't create a DNS zone</span></span>

<span data-ttu-id="58382-108">Při řešení běžných problémů zkuste použít jeden nebo několik následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="58382-108">To resolve common issues, try one or more of the following steps:</span></span>

1.  <span data-ttu-id="58382-109">Prohlédněte si protokoly auditu Azure DNS a určete důvod selhání.</span><span class="sxs-lookup"><span data-stu-id="58382-109">Review the Azure DNS audit logs to determine the failure reason.</span></span>
2.  <span data-ttu-id="58382-110">Název každé zóny DNS musí být v rámci dané skupiny prostředků jedinečný.</span><span class="sxs-lookup"><span data-stu-id="58382-110">Each DNS zone name must be unique within its resource group.</span></span> <span data-ttu-id="58382-111">To znamená, že dvě zóny DNS se stejným názvem nemůžou sdílet stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="58382-111">That is, two DNS zones with the same name cannot share a resource group.</span></span> <span data-ttu-id="58382-112">Zkuste použít jiný název zóny nebo jinou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="58382-112">Try using a different zone name, or a different resource group.</span></span>
3.  <span data-ttu-id="58382-113">Může se zobrazit chyba typu Dosáhli nebo přesáhli jste maximální počet zón v předplatném {id předplatného}.</span><span class="sxs-lookup"><span data-stu-id="58382-113">You may see an error "You have reached or exceeded the maximum number of zones in subscription {subscription id}."</span></span> <span data-ttu-id="58382-114">Použijte jiné předplatné Azure, odstraňte některé zóny nebo kontaktujte podporu Azure se žádostí o zvýšení limitu vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="58382-114">Either use a different Azure subscription, delete some zones, or contact Azure Support to raise your subscription limit.</span></span>
4.  <span data-ttu-id="58382-115">Může se zobrazit chyba typu Zóna {název zóny} není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="58382-115">You may see an error "The zone '{zone name}' is not available."</span></span> <span data-ttu-id="58382-116">Tato chyba znamená, že Azure DNS se pro tuto zónu DNS nepodařilo přidělit názvové servery.</span><span class="sxs-lookup"><span data-stu-id="58382-116">This error means that Azure DNS was unable to allocate name servers for this DNS zone.</span></span> <span data-ttu-id="58382-117">Zkuste použít jiný název zóny.</span><span class="sxs-lookup"><span data-stu-id="58382-117">Try using a different zone name.</span></span> <span data-ttu-id="58382-118">Případně pokud jste vlastníkem názvu domény, kontaktujte podporu Azure, která může přidělit názvové servery za vás.</span><span class="sxs-lookup"><span data-stu-id="58382-118">Alternatively, if you are the domain name owner, contact Azure support, who can allocate name servers for you.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="58382-119">**Doporučené dokumenty**</span><span class="sxs-lookup"><span data-stu-id="58382-119">**Recommended documents**</span></span>

<span data-ttu-id="58382-120">[Záznamy a zóny DNS](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="58382-120">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="58382-121">
[Vytvoření zóny DNS](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="58382-121">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>

## <a name="i-cant-create-a-dns-record"></a><span data-ttu-id="58382-122">Nejde mi vytvořit záznam DNS</span><span class="sxs-lookup"><span data-stu-id="58382-122">I can't create a DNS record</span></span>

<span data-ttu-id="58382-123">Při řešení běžných problémů zkuste použít jeden nebo několik následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="58382-123">To resolve common issues, try one or more of the following steps:</span></span>

1.  <span data-ttu-id="58382-124">Prohlédněte si protokoly auditu Azure DNS a určete důvod selhání.</span><span class="sxs-lookup"><span data-stu-id="58382-124">Review the Azure DNS audit logs to determine the failure reason.</span></span>
2.  <span data-ttu-id="58382-125">Už tato sada záznamů existuje?</span><span class="sxs-lookup"><span data-stu-id="58382-125">Does the record set exist already?</span></span>  <span data-ttu-id="58382-126">Azure DNS spravuje záznamy pomocí *sad* záznamů. To jsou kolekce záznamů se stejným názvem a typem.</span><span class="sxs-lookup"><span data-stu-id="58382-126">Azure DNS manages records using record *sets*, which are the collection of records of the same name and the same type.</span></span> <span data-ttu-id="58382-127">Pokud záznam se stejným názvem a typem už existuje a chcete přidat další takové záznam, měli byste existující sadu záznamů upravit.</span><span class="sxs-lookup"><span data-stu-id="58382-127">If a record with the same name and type already exists, then to add another such record you should edit the existing record set.</span></span>
3.  <span data-ttu-id="58382-128">Pokoušíte se vytvořit záznam ve vrcholu zóny DNS („kořenu“ zóny)?</span><span class="sxs-lookup"><span data-stu-id="58382-128">Are you trying to create a record at the DNS zone apex (the ‘root’ of the zone)?</span></span> <span data-ttu-id="58382-129">Pokud ano, v rámci konvence DNS se jako název tohoto záznamu používá znak ‘@’.</span><span class="sxs-lookup"><span data-stu-id="58382-129">If so, the DNS convention is to use the ‘@’ character as the record name.</span></span> <span data-ttu-id="58382-130">Všimněte si také, že standardy DNS ve vrcholu zóny nepovolují záznamy CNAME.</span><span class="sxs-lookup"><span data-stu-id="58382-130">Also note that the DNS standards do not permit CNAME records at the zone apex.</span></span>
4.  <span data-ttu-id="58382-131">Vznikl konflikt CNAME?</span><span class="sxs-lookup"><span data-stu-id="58382-131">Do you have a CNAME conflict?</span></span>  <span data-ttu-id="58382-132">Standardy DNS nepovolují záznamy CNAME se stejným názvem jako záznam jakéhokoli jiného typu.</span><span class="sxs-lookup"><span data-stu-id="58382-132">The DNS standards do not allow a CNAME record with the same name as a record of any other type.</span></span> <span data-ttu-id="58382-133">Pokud už máte CNAME, vytvoření záznamu jiného typu se stejným názvem se nepovede.</span><span class="sxs-lookup"><span data-stu-id="58382-133">If you have an existing CNAME, creating a record with the same name of a different type fails.</span></span>  <span data-ttu-id="58382-134">Podobně se vytvoření CNAME nepovede, pokud jeho název odpovídá existujícímu záznamu jiného typu.</span><span class="sxs-lookup"><span data-stu-id="58382-134">Likewise, creating a CNAME fails if the name matches an existing record of a different type.</span></span> <span data-ttu-id="58382-135">Konflikt odstraníte odebráním druhého záznamu nebo volbou jiného názvu záznamu.</span><span class="sxs-lookup"><span data-stu-id="58382-135">Remove the conflict by removing the other record or choosing a different record name.</span></span>
5.  <span data-ttu-id="58382-136">Dosáhli jste limitu povoleného počtu sad záznamů v zóně DNS?</span><span class="sxs-lookup"><span data-stu-id="58382-136">Have you reached the limit on the number of record sets permitted in a DNS zone?</span></span> <span data-ttu-id="58382-137">Aktuální počet sad záznamů a maximální počet sad záznamů se zobrazuje na webu Azure Portal ve vlastnostech zóny.</span><span class="sxs-lookup"><span data-stu-id="58382-137">The current number of record sets and the maximum number of record sets are shown in the Azure portal, under the 'Properties' for the zone.</span></span> <span data-ttu-id="58382-138">Pokud jste dosáhli tohoto limitu, buď odstraňte některé sady záznamů nebo kontaktujte podporu Azure, požádejte o zvýšení limitu sad záznamů pro tuto zónu a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="58382-138">If you have reached this limit, then either delete some record sets or contact Azure Support to raise your record set limit for this zone, then try again.</span></span> 


### <a name="recommended-documents"></a><span data-ttu-id="58382-139">**Doporučené dokumenty**</span><span class="sxs-lookup"><span data-stu-id="58382-139">**Recommended documents**</span></span>

<span data-ttu-id="58382-140">[Záznamy a zóny DNS](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="58382-140">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="58382-141">
[Vytvoření zóny DNS](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="58382-141">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>



## <a name="i-cant-resolve-my-dns-record"></a><span data-ttu-id="58382-142">Nejde mi přeložit záznam DNS</span><span class="sxs-lookup"><span data-stu-id="58382-142">I can't resolve my DNS record</span></span>

<span data-ttu-id="58382-143">Překlad názvů DNS je vícefázový proces, který může selhat z mnoha důvodů.</span><span class="sxs-lookup"><span data-stu-id="58382-143">DNS name resolution is a multi-step process, which can fail for many reasons.</span></span> <span data-ttu-id="58382-144">Následující kroky vám pomůžou zjistit, proč se pro záznam DNS v zóně hostované v Azure DNS nedaří překlad DNS.</span><span class="sxs-lookup"><span data-stu-id="58382-144">The following steps help you investigate why DNS resolution is failing for a DNS record in a zone hosted in Azure DNS.</span></span>

1.  <span data-ttu-id="58382-145">Potvrďte, že záznamy DNS jsou v Azure DNS správně nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="58382-145">Confirm that the DNS records have been configured correctly in Azure DNS.</span></span> <span data-ttu-id="58382-146">Zkontrolujte záznamy DNS na webu Azure Portal a ověřte, jestli název zóny, název záznamu a typ záznamu jsou správné.</span><span class="sxs-lookup"><span data-stu-id="58382-146">Review the DNS records in the Azure portal, checking that the zone name, record name, and record type are correct.</span></span>
2.  <span data-ttu-id="58382-147">Zkontrolujte, že se záznamy DNS správně překládají na názvových serverech Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="58382-147">Confirm that the DNS records resolve correctly on the Azure DNS name servers.</span></span>
    - <span data-ttu-id="58382-148">Pokud dotazy DNS zpracováváte z vašeho místního počítače, může se stát, že se zobrazují výsledky uložené v mezipaměti, které neodrážejí aktuální stav serverů.</span><span class="sxs-lookup"><span data-stu-id="58382-148">If you make DNS queries from your local PC, you may see cached results that don’t reflect the current state of the name servers.</span></span>  <span data-ttu-id="58382-149">Podnikové sítě navíc často používají proxy servery DNS, které zabraňují směrování dotazů DNS na konkrétní názvové servery.</span><span class="sxs-lookup"><span data-stu-id="58382-149">Also, corporate networks often use DNS proxy servers, which prevent DNS queries from being directed to specific name servers.</span></span>  <span data-ttu-id="58382-150">Pokud se chcete těmto problémům vyhnout, použijte webovou službu překladu názvů, jako je třeba [digwebinterface](http://digwebinterface.com).</span><span class="sxs-lookup"><span data-stu-id="58382-150">To avoid these problems, use a web-based name resolution service such as [digwebinterface](http://digwebinterface.com).</span></span>
    - <span data-ttu-id="58382-151">Nezapomeňte zadat správné názvové servery pro zónu DNS (jsou uvedené na webu Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="58382-151">Be sure to specify the correct name servers for your DNS zone, as shown in the Azure portal.</span></span>
    - <span data-ttu-id="58382-152">Zkontrolujte správnost názvu DNS (musíte zadat plně kvalifikovaný název včetně názvu zóny) a typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="58382-152">Check that the DNS name is correct (you have to specify the fully qualified name, including the zone name) and the record type is correct</span></span>
3.  <span data-ttu-id="58382-153">Potvrďte, že název domény DNS byl správně [delegovaný na názvové servery Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="58382-153">Confirm that the DNS domain name has been correctly [delegated to the Azure DNS name servers](dns-domain-delegation.md).</span></span> <span data-ttu-id="58382-154">Existuje [celá řada webů třetích stran, které poskytují ověření delegování DNS](https://www.bing.com/search?q=dns+check+tool).</span><span class="sxs-lookup"><span data-stu-id="58382-154">There are a [many 3rd-party web sites that offer DNS delegation validation](https://www.bing.com/search?q=dns+check+tool).</span></span> <span data-ttu-id="58382-155">Jde o test delegování *zóny*, takže byste měli zadat název zóny DNS, a ne plně kvalifikovaný název záznamu.</span><span class="sxs-lookup"><span data-stu-id="58382-155">This test is a *zone* delegation test, so you should only enter the DNS zone name and not the fully qualified record name.</span></span>
4.  <span data-ttu-id="58382-156">Po dokončení těchto kroků by se záznam DNS už měl překládat správně.</span><span class="sxs-lookup"><span data-stu-id="58382-156">Having completed the above, your DNS record should now resolve correctly.</span></span> <span data-ttu-id="58382-157">K ověření můžete znovu využít [digwebinterface](http://digwebinterface.com) a tentokrát použít výchozí nastavení názvového serveru.</span><span class="sxs-lookup"><span data-stu-id="58382-157">To verify, you can again use [digwebinterface](http://digwebinterface.com), this time using the default name server settings.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="58382-158">**Doporučené dokumenty**</span><span class="sxs-lookup"><span data-stu-id="58382-158">**Recommended documents**</span></span>

[<span data-ttu-id="58382-159">Delegování domény do Azure DNS</span><span class="sxs-lookup"><span data-stu-id="58382-159">Delegate a domain to Azure DNS</span></span>](dns-domain-delegation.md)



## <a name="how-do-i-specify-the-service-and-protocol-for-an-srv-record"></a><span data-ttu-id="58382-160">Jak zadat službu a protokol pro záznam SRV?</span><span class="sxs-lookup"><span data-stu-id="58382-160">How do I specify the ‘service’ and ‘protocol’ for an SRV record?</span></span>

<span data-ttu-id="58382-161">Azure DNS spravuje záznamy DNS jako sady záznamů. To jsou kolekce záznamů se stejným názvem a typem.</span><span class="sxs-lookup"><span data-stu-id="58382-161">Azure DNS manages DNS records as record sets—the collection of records with the same name and the same type.</span></span> <span data-ttu-id="58382-162">U sady záznamů SRV se služba i protokol musí zadat jako součást názvu sady.</span><span class="sxs-lookup"><span data-stu-id="58382-162">For an SRV record set, the 'service' and 'protocol' need to be specified as part of the record set name.</span></span> <span data-ttu-id="58382-163">Ostatní parametry SRV (priorita, váha, port a cíl) se pro jednotlivé záznamy v sadě záznamů zadávají samostatně.</span><span class="sxs-lookup"><span data-stu-id="58382-163">The other SRV parameters ('priority', 'weight', 'port' and 'target') are specified separately for each record in the record set.</span></span>

<span data-ttu-id="58382-164">Ukázkové názvy záznamů SRV (služba = sip, protokol = tcp):</span><span class="sxs-lookup"><span data-stu-id="58382-164">Example SRV record names (service name 'sip', protocol 'tcp'):</span></span>

- <span data-ttu-id="58382-165">\_sip.\_tcp (vytvoří sadu záznamů ve vrcholu zóny)</span><span class="sxs-lookup"><span data-stu-id="58382-165">\_sip.\_tcp (creates a record set at the zone apex)</span></span>
- <span data-ttu-id="58382-166">\_sip.\_tcp.sipservice (vytvoří sadu záznamů s názvem sipservice)</span><span class="sxs-lookup"><span data-stu-id="58382-166">\_sip.\_tcp.sipservice (creates a record set named 'sipservice')</span></span>

### <a name="recommended-documents"></a><span data-ttu-id="58382-167">**Doporučené dokumenty**</span><span class="sxs-lookup"><span data-stu-id="58382-167">**Recommended documents**</span></span>

<span data-ttu-id="58382-168">[Záznamy a zóny DNS](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="58382-168">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="58382-169">
[Vytváření sad záznamů a záznamů DNS pomocí webu Azure Portal](dns-getstarted-create-recordset-portal.md)
</span><span class="sxs-lookup"><span data-stu-id="58382-169">
[Create DNS record sets and records by using the Azure portal](dns-getstarted-create-recordset-portal.md)
</span></span><br><span data-ttu-id="58382-170">
[Záznamy typu SRV (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span><span class="sxs-lookup"><span data-stu-id="58382-170">
[SRV record type (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span></span>


## <a name="next-steps"></a><span data-ttu-id="58382-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="58382-171">Next steps</span></span>

* <span data-ttu-id="58382-172">Další informace o [Azure DNS zóny a záznamy](dns-zones-records.md)</span><span class="sxs-lookup"><span data-stu-id="58382-172">Learn about [Azure DNS zones and records](dns-zones-records.md)</span></span>
* <span data-ttu-id="58382-173">Chcete-li začít používat Azure DNS, zjistěte další postup [vytvořit zónu DNS](dns-getstarted-create-dnszone-portal.md) a [vytvořit záznamy DNS](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="58382-173">To start using Azure DNS, learn how to [create a DNS zone](dns-getstarted-create-dnszone-portal.md) and [create DNS records](dns-getstarted-create-recordset-portal.md).</span></span>
* <span data-ttu-id="58382-174">Pokud chcete migrovat existující zónu DNS, zjistěte, jak [import a export souboru zóny DNS](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="58382-174">To migrate an existing DNS zone, learn how to [import and export a DNS zone file](dns-import-export.md).</span></span>

