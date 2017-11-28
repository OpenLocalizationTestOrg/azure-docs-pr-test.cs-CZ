---
title: "Průvodci odstraňováním potíží DNS aaaAzure | Microsoft Docs"
description: "Jak tootroubleshoot běžné problémy s Azure DNS"
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
ms.openlocfilehash: 944aa1811c980063f739268cd2c79b647b2754a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-troubleshooting-guide"></a><span data-ttu-id="c853d-103">Azure DNS Průvodci odstraňováním potíží</span><span class="sxs-lookup"><span data-stu-id="c853d-103">Azure DNS troubleshooting guide</span></span>

<span data-ttu-id="c853d-104">Tato stránka obsahuje informace o odstraňování potíží pro běžné otázky Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="c853d-104">This page provides troubleshooting information for common Azure DNS questions.</span></span>

<span data-ttu-id="c853d-105">Pokud tyto kroky problém nevyřeší, můžete také vyhledat nebo zveřejnit svůj problém na našem [Fórum komunity podpory na webu MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span><span class="sxs-lookup"><span data-stu-id="c853d-105">If these steps don't resolve your issue, you can also search for or post your issue on our [community support forum on MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span></span> <span data-ttu-id="c853d-106">Alternativně můžete otevřete žádost o podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="c853d-106">Alternatively, open an Azure support request.</span></span>


## <a name="i-cant-create-a-dns-zone"></a><span data-ttu-id="c853d-107">Nelze vytvořit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="c853d-107">I can't create a DNS zone</span></span>

<span data-ttu-id="c853d-108">tooresolve běžné problémy, zkuste jeden nebo více hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c853d-108">tooresolve common issues, try one or more of hello following steps:</span></span>

1.  <span data-ttu-id="c853d-109">Zkontrolujte hello auditu Azure DNS zaznamená důvod selhání toodetermine hello.</span><span class="sxs-lookup"><span data-stu-id="c853d-109">Review hello Azure DNS audit logs toodetermine hello failure reason.</span></span>
2.  <span data-ttu-id="c853d-110">Název každé zóny DNS musí být v rámci dané skupiny prostředků jedinečný.</span><span class="sxs-lookup"><span data-stu-id="c853d-110">Each DNS zone name must be unique within its resource group.</span></span> <span data-ttu-id="c853d-111">To znamená, dvě DNS zóny s hello stejný název nelze sdílet skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c853d-111">That is, two DNS zones with hello same name cannot share a resource group.</span></span> <span data-ttu-id="c853d-112">Zkuste použít jiný název zóny nebo jinou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c853d-112">Try using a different zone name, or a different resource group.</span></span>
3.  <span data-ttu-id="c853d-113">Mohou se zobrazit chyba "Můžete mít dosáhli nebo Přesáhli jste maximální počet zón v předplatném {id předplatného} hello."</span><span class="sxs-lookup"><span data-stu-id="c853d-113">You may see an error "You have reached or exceeded hello maximum number of zones in subscription {subscription id}."</span></span> <span data-ttu-id="c853d-114">Použijte jiné předplatné Azure, odstraňte některé zóny nebo požádejte podporu Azure tooraise svého limitu předplatného.</span><span class="sxs-lookup"><span data-stu-id="c853d-114">Either use a different Azure subscription, delete some zones, or contact Azure Support tooraise your subscription limit.</span></span>
4.  <span data-ttu-id="c853d-115">Zobrazí chyba "hello zóny {zóny name} není k dispozici."</span><span class="sxs-lookup"><span data-stu-id="c853d-115">You may see an error "hello zone '{zone name}' is not available."</span></span> <span data-ttu-id="c853d-116">Tato chyba znamená, že Azure DNS bylo nelze tooallocate názvové servery pro tuto zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="c853d-116">This error means that Azure DNS was unable tooallocate name servers for this DNS zone.</span></span> <span data-ttu-id="c853d-117">Zkuste použít jiný název zóny.</span><span class="sxs-lookup"><span data-stu-id="c853d-117">Try using a different zone name.</span></span> <span data-ttu-id="c853d-118">Případně pokud jste vlastník název domény hello, požádejte podporu Azure, který můžete přidělit názvové servery, které pro vás.</span><span class="sxs-lookup"><span data-stu-id="c853d-118">Alternatively, if you are hello domain name owner, contact Azure support, who can allocate name servers for you.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="c853d-119">**Doporučené dokumenty**</span><span class="sxs-lookup"><span data-stu-id="c853d-119">**Recommended documents**</span></span>

<span data-ttu-id="c853d-120">[Záznamy a zóny DNS](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="c853d-120">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="c853d-121">
[Vytvoření zóny DNS](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c853d-121">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>

## <a name="i-cant-create-a-dns-record"></a><span data-ttu-id="c853d-122">Nejde mi vytvořit záznam DNS</span><span class="sxs-lookup"><span data-stu-id="c853d-122">I can't create a DNS record</span></span>

<span data-ttu-id="c853d-123">tooresolve běžné problémy, zkuste jeden nebo více hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c853d-123">tooresolve common issues, try one or more of hello following steps:</span></span>

1.  <span data-ttu-id="c853d-124">Zkontrolujte hello auditu Azure DNS zaznamená důvod selhání toodetermine hello.</span><span class="sxs-lookup"><span data-stu-id="c853d-124">Review hello Azure DNS audit logs toodetermine hello failure reason.</span></span>
2.  <span data-ttu-id="c853d-125">Existuje sada záznamů hello již?</span><span class="sxs-lookup"><span data-stu-id="c853d-125">Does hello record set exist already?</span></span>  <span data-ttu-id="c853d-126">Azure DNS spravuje záznamy pomocí záznamu *nastaví*, což jsou kolekce hello záznamů hello stejný název a text hello, stejný typ.</span><span class="sxs-lookup"><span data-stu-id="c853d-126">Azure DNS manages records using record *sets*, which are hello collection of records of hello same name and hello same type.</span></span> <span data-ttu-id="c853d-127">Pokud záznam s hello stejný název a typ již existuje, pak tooadd jiné takové byste neměli upravovat stávající záznam hello sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="c853d-127">If a record with hello same name and type already exists, then tooadd another such record you should edit hello existing record set.</span></span>
3.  <span data-ttu-id="c853d-128">Pokoušíte toocreate záznamů na vrcholu zóny DNS hello (hello kořenové zóně hello)?</span><span class="sxs-lookup"><span data-stu-id="c853d-128">Are you trying toocreate a record at hello DNS zone apex (hello ‘root’ of hello zone)?</span></span> <span data-ttu-id="c853d-129">Pokud ano, hello konvenci DNS je toouse hello ' @' znak jako název záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="c853d-129">If so, hello DNS convention is toouse hello ‘@’ character as hello record name.</span></span> <span data-ttu-id="c853d-130">Všimněte si také, že standardech DNS hello nepovoluje záznamy CNAME na vrcholu zóny hello.</span><span class="sxs-lookup"><span data-stu-id="c853d-130">Also note that hello DNS standards do not permit CNAME records at hello zone apex.</span></span>
4.  <span data-ttu-id="c853d-131">Vznikl konflikt CNAME?</span><span class="sxs-lookup"><span data-stu-id="c853d-131">Do you have a CNAME conflict?</span></span>  <span data-ttu-id="c853d-132">Hello standardech DNS záznam CNAME se hello stejný název jako záznamy o jiný typ nepovolují.</span><span class="sxs-lookup"><span data-stu-id="c853d-132">hello DNS standards do not allow a CNAME record with hello same name as a record of any other type.</span></span> <span data-ttu-id="c853d-133">Pokud máte existující CNAME, vytvoření záznamu s hello, v případě selhání stejný název jiného typu.</span><span class="sxs-lookup"><span data-stu-id="c853d-133">If you have an existing CNAME, creating a record with hello same name of a different type fails.</span></span>  <span data-ttu-id="c853d-134">Podobně vytváření záznam CNAME se nezdaří, pokud název hello odpovídá existující záznam jiného typu.</span><span class="sxs-lookup"><span data-stu-id="c853d-134">Likewise, creating a CNAME fails if hello name matches an existing record of a different type.</span></span> <span data-ttu-id="c853d-135">Odeberte hello konflikt odebráním hello jiný záznam nebo zvolte jiný záznam název.</span><span class="sxs-lookup"><span data-stu-id="c853d-135">Remove hello conflict by removing hello other record or choosing a different record name.</span></span>
5.  <span data-ttu-id="c853d-136">Dosáhli jste limitu hello hello počtu sady záznamů v zóně DNS povolené? Hello aktuální počet sad záznamů a hello maximální počet sad záznamů jsou zobrazeny v portálu Azure, v části hello 'vlastnosti' pro zónu hello hello.</span><span class="sxs-lookup"><span data-stu-id="c853d-136">Have you reached hello limit on hello number of record sets permitted in a DNS zone? hello current number of record sets and hello maximum number of record sets are shown in hello Azure portal, under hello 'Properties' for hello zone.</span></span> <span data-ttu-id="c853d-137">Pokud bylo dosaženo tohoto limitu, pak buď odstranit některé sady záznamů nebo kontaktujte podporu Azure tooraise záznamů nastaveného limitu pro tuto zónu, a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="c853d-137">If you have reached this limit, then either delete some record sets or contact Azure Support tooraise your record set limit for this zone, then try again.</span></span> 


### <a name="recommended-documents"></a><span data-ttu-id="c853d-138">**Doporučené dokumenty**</span><span class="sxs-lookup"><span data-stu-id="c853d-138">**Recommended documents**</span></span>

<span data-ttu-id="c853d-139">[Záznamy a zóny DNS](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="c853d-139">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="c853d-140">
[Vytvoření zóny DNS](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c853d-140">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>



## <a name="i-cant-resolve-my-dns-record"></a><span data-ttu-id="c853d-141">Nejde mi přeložit záznam DNS</span><span class="sxs-lookup"><span data-stu-id="c853d-141">I can't resolve my DNS record</span></span>

<span data-ttu-id="c853d-142">Překlad názvů DNS je vícefázový proces, který může selhat z mnoha důvodů.</span><span class="sxs-lookup"><span data-stu-id="c853d-142">DNS name resolution is a multi-step process, which can fail for many reasons.</span></span> <span data-ttu-id="c853d-143">Hello následující postup vám pomůže prozkoumat, proč se nedaří překlad názvů DNS pro záznam DNS v zóně hostované v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="c853d-143">hello following steps help you investigate why DNS resolution is failing for a DNS record in a zone hosted in Azure DNS.</span></span>

1.  <span data-ttu-id="c853d-144">Potvrďte, že záznamy DNS hello byly nakonfigurovány správně v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="c853d-144">Confirm that hello DNS records have been configured correctly in Azure DNS.</span></span> <span data-ttu-id="c853d-145">Zkontrolujte záznamy DNS hello v hello portál Azure, kontrola, zda jsou správné hello název zóny, název záznamu a typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="c853d-145">Review hello DNS records in hello Azure portal, checking that hello zone name, record name, and record type are correct.</span></span>
2.  <span data-ttu-id="c853d-146">Potvrďte, že záznamy DNS hello správně přeložit na názvových serverů Azure DNS hello.</span><span class="sxs-lookup"><span data-stu-id="c853d-146">Confirm that hello DNS records resolve correctly on hello Azure DNS name servers.</span></span>
    - <span data-ttu-id="c853d-147">Pokud provedete dotazy DNS z vašeho místního počítače, může se zobrazit výsledky uložené v mezipaměti, které neodpovídají hello aktuální stav hello názvové servery.</span><span class="sxs-lookup"><span data-stu-id="c853d-147">If you make DNS queries from your local PC, you may see cached results that don’t reflect hello current state of hello name servers.</span></span>  <span data-ttu-id="c853d-148">Kromě toho podnikové sítě často používají proxy servery DNS, který zabrání se dotazy DNS přesměruje toospecific názvové servery.</span><span class="sxs-lookup"><span data-stu-id="c853d-148">Also, corporate networks often use DNS proxy servers, which prevent DNS queries from being directed toospecific name servers.</span></span>  <span data-ttu-id="c853d-149">tooavoid tyto problémy používat webová služba pro překlad názvů, jako [digwebinterface](http://digwebinterface.com).</span><span class="sxs-lookup"><span data-stu-id="c853d-149">tooavoid these problems, use a web-based name resolution service such as [digwebinterface](http://digwebinterface.com).</span></span>
    - <span data-ttu-id="c853d-150">Zda toospecify být hello správné názvové servery pro zónu DNS, jak je znázorněno v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c853d-150">Be sure toospecify hello correct name servers for your DNS zone, as shown in hello Azure portal.</span></span>
    - <span data-ttu-id="c853d-151">Zkontrolujte, zda je název DNS hello správný, (máte toospecify hello plně kvalifikovaný název, včetně názvu zóny hello) a typ záznamu hello je správný</span><span class="sxs-lookup"><span data-stu-id="c853d-151">Check that hello DNS name is correct (you have toospecify hello fully qualified name, including hello zone name) and hello record type is correct</span></span>
3.  <span data-ttu-id="c853d-152">Zkontrolujte název domény DNS hello je správně [delegovaný názvových serverů Azure DNS toohello](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="c853d-152">Confirm that hello DNS domain name has been correctly [delegated toohello Azure DNS name servers](dns-domain-delegation.md).</span></span> <span data-ttu-id="c853d-153">Existuje [celá řada webů třetích stran, které poskytují ověření delegování DNS](https://www.bing.com/search?q=dns+check+tool).</span><span class="sxs-lookup"><span data-stu-id="c853d-153">There are a [many 3rd-party web sites that offer DNS delegation validation](https://www.bing.com/search?q=dns+check+tool).</span></span> <span data-ttu-id="c853d-154">Tento test je *zóny* delegování test, takže byste měli jenom zadat název zóny DNS hello a není hello plně kvalifikovaný název záznamu.</span><span class="sxs-lookup"><span data-stu-id="c853d-154">This test is a *zone* delegation test, so you should only enter hello DNS zone name and not hello fully qualified record name.</span></span>
4.  <span data-ttu-id="c853d-155">Po dokončení hello výše, vaše záznam DNS by měl nyní vyřešit správně.</span><span class="sxs-lookup"><span data-stu-id="c853d-155">Having completed hello above, your DNS record should now resolve correctly.</span></span> <span data-ttu-id="c853d-156">tooverify, můžete znovu použít [digwebinterface](http://digwebinterface.com), tentokrát pomocí nastavení hello výchozí název serveru.</span><span class="sxs-lookup"><span data-stu-id="c853d-156">tooverify, you can again use [digwebinterface](http://digwebinterface.com), this time using hello default name server settings.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="c853d-157">**Doporučené dokumenty**</span><span class="sxs-lookup"><span data-stu-id="c853d-157">**Recommended documents**</span></span>

[<span data-ttu-id="c853d-158">Delegát tooAzure domény DNS</span><span class="sxs-lookup"><span data-stu-id="c853d-158">Delegate a domain tooAzure DNS</span></span>](dns-domain-delegation.md)



## <a name="how-do-i-specify-hello-service-and-protocol-for-an-srv-record"></a><span data-ttu-id="c853d-159">Jak určit hello "služba" a "protokol" pro záznam SRV?</span><span class="sxs-lookup"><span data-stu-id="c853d-159">How do I specify hello ‘service’ and ‘protocol’ for an SRV record?</span></span>

<span data-ttu-id="c853d-160">Azure DNS spravuje záznamy DNS jako sady záznamů – hello kolekce záznamů s hello stejný název a text hello, stejný typ.</span><span class="sxs-lookup"><span data-stu-id="c853d-160">Azure DNS manages DNS records as record sets—hello collection of records with hello same name and hello same type.</span></span> <span data-ttu-id="c853d-161">Pro sadu záznamů SRV služby"hello" a "protokol" potřebovat toobe zadaný jako součást hello název sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="c853d-161">For an SRV record set, hello 'service' and 'protocol' need toobe specified as part of hello record set name.</span></span> <span data-ttu-id="c853d-162">Hello další parametry SRV ('prioritu', 'weight', 'port' a "target") se zadávají samostatně pro jednotlivé záznamy v sadě záznamů hello.</span><span class="sxs-lookup"><span data-stu-id="c853d-162">hello other SRV parameters ('priority', 'weight', 'port' and 'target') are specified separately for each record in hello record set.</span></span>

<span data-ttu-id="c853d-163">Ukázkové názvy záznamů SRV (služba = sip, protokol = tcp):</span><span class="sxs-lookup"><span data-stu-id="c853d-163">Example SRV record names (service name 'sip', protocol 'tcp'):</span></span>

- <span data-ttu-id="c853d-164">\_SIP. \_tcp (vytvoří záznamů na vrcholu zóny hello)</span><span class="sxs-lookup"><span data-stu-id="c853d-164">\_sip.\_tcp (creates a record set at hello zone apex)</span></span>
- <span data-ttu-id="c853d-165">\_sip.\_tcp.sipservice (vytvoří sadu záznamů s názvem sipservice)</span><span class="sxs-lookup"><span data-stu-id="c853d-165">\_sip.\_tcp.sipservice (creates a record set named 'sipservice')</span></span>

### <a name="recommended-documents"></a><span data-ttu-id="c853d-166">**Doporučené dokumenty**</span><span class="sxs-lookup"><span data-stu-id="c853d-166">**Recommended documents**</span></span>

<span data-ttu-id="c853d-167">[Záznamy a zóny DNS](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="c853d-167">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="c853d-168">
[Vytvoření sady záznamů DNS a záznamy pomocí hello portálu Azure](dns-getstarted-create-recordset-portal.md)
</span><span class="sxs-lookup"><span data-stu-id="c853d-168">
[Create DNS record sets and records by using hello Azure portal](dns-getstarted-create-recordset-portal.md)
</span></span><br><span data-ttu-id="c853d-169">
[Záznamy typu SRV (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span><span class="sxs-lookup"><span data-stu-id="c853d-169">
[SRV record type (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span></span>


## <a name="next-steps"></a><span data-ttu-id="c853d-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c853d-170">Next steps</span></span>

* <span data-ttu-id="c853d-171">Další informace o [Azure DNS zóny a záznamy](dns-zones-records.md)</span><span class="sxs-lookup"><span data-stu-id="c853d-171">Learn about [Azure DNS zones and records](dns-zones-records.md)</span></span>
* <span data-ttu-id="c853d-172">toostart pomocí Azure DNS, zjistěte, jak příliš[vytvořit zónu DNS](dns-getstarted-create-dnszone-portal.md) a [vytvořit záznamy DNS](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c853d-172">toostart using Azure DNS, learn how too[create a DNS zone](dns-getstarted-create-dnszone-portal.md) and [create DNS records](dns-getstarted-create-recordset-portal.md).</span></span>
* <span data-ttu-id="c853d-173">toomigrate stávající zónu DNS, zjistěte, jak příliš[import a export souboru zóny DNS](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="c853d-173">toomigrate an existing DNS zone, learn how too[import and export a DNS zone file](dns-import-export.md).</span></span>

