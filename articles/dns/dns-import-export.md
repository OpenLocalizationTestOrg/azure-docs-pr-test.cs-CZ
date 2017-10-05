---
title: "Importovat a exportovat soubor zóny domény do Azure DNS pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak importovat a exportovat souboru zóny DNS do Azure DNS pomocí Azure CLI 1.0"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: d6d3fa7aa0e8b2462b3a6b4b66d3d87ab5535314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli-10"></a><span data-ttu-id="d4d22-103">Import a export souboru zóny DNS pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d4d22-103">Import and export a DNS zone file using the Azure CLI 1.0</span></span> 

<span data-ttu-id="d4d22-104">Tento článek vás provede jak importovat a exportovat soubory zóny DNS pro Azure DNS pomocí Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="d4d22-104">This article walks you through how to import and export DNS zone files for Azure DNS using the Azure CLI 1.0.</span></span>

## <a name="introduction-to-dns-zone-migration"></a><span data-ttu-id="d4d22-105">Úvod do migrace zóny DNS</span><span class="sxs-lookup"><span data-stu-id="d4d22-105">Introduction to DNS zone migration</span></span>

<span data-ttu-id="d4d22-106">Soubor zóny DNS je textový soubor, který obsahuje podrobnosti o každé systému DNS (Domain Name) záznamu v zóně.</span><span class="sxs-lookup"><span data-stu-id="d4d22-106">A DNS zone file is a text file that contains details of every Domain Name System (DNS) record in the zone.</span></span> <span data-ttu-id="d4d22-107">Postupuje standardní formát, takže je vhodné pro přenos mezi systémy DNS záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-107">It follows a standard format, making it suitable for transferring DNS records between DNS systems.</span></span> <span data-ttu-id="d4d22-108">Použití souboru zóny je rychlé, spolehlivé a pohodlný způsob pro přenos zóny DNS do nebo z Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-108">Using a zone file is a quick, reliable, and convenient way to transfer a DNS zone into or out of Azure DNS.</span></span>

<span data-ttu-id="d4d22-109">Azure DNS podporuje import a export souborů zóny pomocí rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="d4d22-109">Azure DNS supports importing and exporting zone files by using the Azure command-line interface (CLI).</span></span> <span data-ttu-id="d4d22-110">Import souboru zóny je **není** aktuálně podporované pomocí prostředí Azure PowerShell nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4d22-110">Zone file import is **not** currently supported via Azure PowerShell or the Azure portal.</span></span>

<span data-ttu-id="d4d22-111">Azure CLI 1.0 je nástroj příkazového řádku a platformy, použít pro správu služby Azure.</span><span class="sxs-lookup"><span data-stu-id="d4d22-111">The Azure CLI 1.0 is a cross-platform command-line tool used for managing Azure services.</span></span> <span data-ttu-id="d4d22-112">Je k dispozici pro platformy Windows, Mac a Linux z [stránky Azure stahování](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="d4d22-112">It is available for the Windows, Mac, and Linux platforms from the [Azure downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="d4d22-113">Podpora více platforem je důležité pro import a export souborů zóny, protože nejběžnější název serverového softwaru [vazby](https://www.isc.org/downloads/bind/), obvykle běží na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="d4d22-113">Cross-platform support is important for importing and exporting zone files, because the most common name server software, [BIND](https://www.isc.org/downloads/bind/), typically runs on Linux.</span></span>

> [!NOTE]
> <span data-ttu-id="d4d22-114">Aktuálně neexistují dvě verze rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="d4d22-114">There are currently two versions of the Azure CLI.</span></span> <span data-ttu-id="d4d22-115">CLI1.0 je založena na Node.js a má příkazy počínaje "azure".</span><span class="sxs-lookup"><span data-stu-id="d4d22-115">CLI1.0 is based on Node.js, and has commands beginning with "azure".</span></span>
> <span data-ttu-id="d4d22-116">CLI2.0 je založena na Python a má příkazy počínaje "az".</span><span class="sxs-lookup"><span data-stu-id="d4d22-116">CLI2.0 is based on Python and has commands beginning with "az".</span></span> <span data-ttu-id="d4d22-117">Při importu souboru zóny je podporovaná v obou verzích, doporučujeme používat příkazy CLI1.0, jak je popsáno v této stránce.</span><span class="sxs-lookup"><span data-stu-id="d4d22-117">While zone file import is supported in both versions, we recommend using the CLI1.0 commands, as described in this page.</span></span>

## <a name="obtain-your-existing-dns-zone-file"></a><span data-ttu-id="d4d22-118">Získat existující soubor zóny DNS</span><span class="sxs-lookup"><span data-stu-id="d4d22-118">Obtain your existing DNS zone file</span></span>

<span data-ttu-id="d4d22-119">Před importem souboru zóny DNS do Azure DNS, musíte získat kopii souboru zóny.</span><span class="sxs-lookup"><span data-stu-id="d4d22-119">Before you import a DNS zone file into Azure DNS, you need to obtain a copy of the zone file.</span></span> <span data-ttu-id="d4d22-120">Zdroj tohoto souboru závisí na je aktuálně hostitelem zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-120">The source of this file depends on where the DNS zone is currently hosted.</span></span>

* <span data-ttu-id="d4d22-121">Pokud zónu DNS je hostitelem služby partnera (například doménového registrátora, vyhrazené poskytovatele hostingu DNS nebo poskytovatele alternativní cloudu), měl by poskytnout služby možnost stažení souboru zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-121">If your DNS zone is hosted by a partner service (such as a domain registrar, dedicated DNS hosting provider, or alternative cloud provider), that service should provide the ability to download the DNS zone file.</span></span>
* <span data-ttu-id="d4d22-122">Pokud DNS systému Windows je hostitelem zóny DNS, je výchozí složku pro soubory zóny **%systemroot%\system32\dns**.</span><span class="sxs-lookup"><span data-stu-id="d4d22-122">If your DNS zone is hosted on Windows DNS, the default folder for the zone files is **%systemroot%\system32\dns**.</span></span> <span data-ttu-id="d4d22-123">Úplná cesta k souboru každé zóny také ukazuje na **Obecné** kartě konzolu DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-123">The full path to each zone file also shows on the **General** tab of the DNS console.</span></span>
* <span data-ttu-id="d4d22-124">Pokud zónu DNS je hostitelem pomocí vazby, je umístění souboru zóny pro každou zónu zadaný v konfiguračním souboru vazby **named.conf**.</span><span class="sxs-lookup"><span data-stu-id="d4d22-124">If your DNS zone is hosted by using BIND, the location of the zone file for each zone is specified in the BIND configuration file **named.conf**.</span></span>

> [!NOTE]
> <span data-ttu-id="d4d22-125">Zóny soubory stahované z GoDaddy mají mírně nestandardní formát.</span><span class="sxs-lookup"><span data-stu-id="d4d22-125">Zone files downloaded from GoDaddy have a slightly nonstandard format.</span></span> <span data-ttu-id="d4d22-126">Budete muset opravu před importem tyto soubory zóny do Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-126">You need to correct this before you import these zone files into Azure DNS.</span></span>
>
> <span data-ttu-id="d4d22-127">Názvy DNS v RDATA každého záznamu DNS jsou určené jako plně kvalifikované názvy, ale nemají ukončující "." To znamená, že se interpretují jinými systémy DNS jako relativních názvů.</span><span class="sxs-lookup"><span data-stu-id="d4d22-127">DNS names in the RDATA of each DNS record are specified as fully qualified names, but they don't have a terminating "." This means they are interpreted by other DNS systems as relative names.</span></span> <span data-ttu-id="d4d22-128">Je nutné upravit soubor zóny připojit ukončení "." na jejich názvy před importem do Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-128">You need to edit the zone file to append the terminating "." to their names before you import them into Azure DNS.</span></span>
>
> <span data-ttu-id="d4d22-129">Například by mělo být změněno záznam CNAME "v doméně contoso.com CNAME www 3600" na "www 3600 v doméně contoso.com CNAME."</span><span class="sxs-lookup"><span data-stu-id="d4d22-129">For example, the CNAME record "www 3600 IN CNAME contoso.com" should be changed to "www 3600 IN CNAME contoso.com."</span></span>
> <span data-ttu-id="d4d22-130">(s ukončující ".").</span><span class="sxs-lookup"><span data-stu-id="d4d22-130">(with a terminating ".").</span></span>

## <a name="import-a-dns-zone-file-into-azure-dns"></a><span data-ttu-id="d4d22-131">Import souboru zóny DNS do Azure DNS</span><span class="sxs-lookup"><span data-stu-id="d4d22-131">Import a DNS zone file into Azure DNS</span></span>

<span data-ttu-id="d4d22-132">Import souboru zóny vytvoří nové zóny v Azure DNS, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d4d22-132">Importing a zone file creates a new zone in Azure DNS if one does not already exist.</span></span> <span data-ttu-id="d4d22-133">Pokud zóna již existuje, je potřeba sloučit sady záznamů v souboru zóny s existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="d4d22-133">If the zone already exists, the record sets in the zone file must be merged with the existing record sets.</span></span>

### <a name="merge-behavior"></a><span data-ttu-id="d4d22-134">Sloučení chování</span><span class="sxs-lookup"><span data-stu-id="d4d22-134">Merge behavior</span></span>

* <span data-ttu-id="d4d22-135">Ve výchozím nastavení jsou sloučeny stávající a nové sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="d4d22-135">By default, existing and new record sets are merged.</span></span> <span data-ttu-id="d4d22-136">Jsou identické záznamy v sadě záznamů sloučené zrušte duplicitní.</span><span class="sxs-lookup"><span data-stu-id="d4d22-136">Identical records within a merged record set are de-duplicated.</span></span>
* <span data-ttu-id="d4d22-137">Můžete také zadáním `--force` možnost, nahradí proces importu, který se stávajícím záznamem, nastaví se nové sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="d4d22-137">Alternatively, by specifying the `--force` option, the import process replaces existing record sets with new record sets.</span></span> <span data-ttu-id="d4d22-138">Existující sady záznamů, které nemají odpovídající záznam v souboru importované zóny se neodeberou.</span><span class="sxs-lookup"><span data-stu-id="d4d22-138">Existing record sets that do not have a corresponding record set in the imported zone file are not be removed.</span></span>
* <span data-ttu-id="d4d22-139">Když sady záznamů jsou sloučeny, použije se čas live (TTL) z dříve existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="d4d22-139">When record sets are merged, the time to live (TTL) of preexisting record sets is used.</span></span> <span data-ttu-id="d4d22-140">Když `--force` se používá, se používá hodnota TTL nové sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="d4d22-140">When `--force` is used, the TTL of the new record set is used.</span></span>
* <span data-ttu-id="d4d22-141">Začátek parametry záznam Authority (SOA) (s výjimkou `host`) jsou vždy převzat ze souboru importované zóny, bez ohledu na to, jestli se `--force` se používá.</span><span class="sxs-lookup"><span data-stu-id="d4d22-141">Start of Authority (SOA) parameters (except `host`) are always taken from the imported zone file, regardless of whether `--force` is used.</span></span> <span data-ttu-id="d4d22-142">Pro záznam názvového serveru nastavit ve vrcholu zóny, se podobně interval TTL, ZÍSKÁ bodů vždy převzat ze souboru importované zóny.</span><span class="sxs-lookup"><span data-stu-id="d4d22-142">Similarly, for the name server record set at the zone apex, the TTL is always taken from the imported zone file.</span></span>
* <span data-ttu-id="d4d22-143">Importovaný záznam CNAME nenahrazuje existující záznam CNAME se stejným názvem, pokud `--force` je zadán parametr.</span><span class="sxs-lookup"><span data-stu-id="d4d22-143">An imported CNAME record does not replace an existing CNAME record with the same name unless the `--force` parameter is specified.</span></span>
* <span data-ttu-id="d4d22-144">Když dojde ke konfliktu mezi záznam CNAME a jiný záznam stejným názvem, ale jiný typ (bez ohledu na to, který je existující nebo nové), se zachová stávající záznam.</span><span class="sxs-lookup"><span data-stu-id="d4d22-144">When a conflict arises between a CNAME record and another record of the same name but different type (regardless of which is existing or new), the existing record is retained.</span></span> <span data-ttu-id="d4d22-145">Je nezávislé na použití `--force`.</span><span class="sxs-lookup"><span data-stu-id="d4d22-145">This is independent of the use of `--force`.</span></span>

### <a name="additional-information-about-importing"></a><span data-ttu-id="d4d22-146">Další informace o importu</span><span class="sxs-lookup"><span data-stu-id="d4d22-146">Additional information about importing</span></span>

<span data-ttu-id="d4d22-147">Následující poznámky k poskytují další technické podrobnosti o zóny importu.</span><span class="sxs-lookup"><span data-stu-id="d4d22-147">The following notes provide additional technical details about the zone import process.</span></span>

* <span data-ttu-id="d4d22-148">`$TTL` – Direktiva je volitelný a je podporované.</span><span class="sxs-lookup"><span data-stu-id="d4d22-148">The `$TTL` directive is optional, and it is supported.</span></span> <span data-ttu-id="d4d22-149">Pokud ne `$TTL` – direktiva je zadána, jsou záznamy bez explicitního TTL importované nastaven na výchozí hodnotu TTL 3 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="d4d22-149">When no `$TTL` directive is given, records without an explicit TTL are imported set to a default TTL of 3600 seconds.</span></span> <span data-ttu-id="d4d22-150">Když dva záznamy v sadě záznamů stejnou zadat jiný TTLs, použije se nižší hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d4d22-150">When two records in the same record set specify different TTLs, the lower value is used.</span></span>
* <span data-ttu-id="d4d22-151">`$ORIGIN` – Direktiva je volitelný a je podporované.</span><span class="sxs-lookup"><span data-stu-id="d4d22-151">The `$ORIGIN` directive is optional, and it is supported.</span></span> <span data-ttu-id="d4d22-152">Pokud ne `$ORIGIN` není nastaven, je výchozí hodnota používaná pro název zóny jako zadaného na příkazovém řádku (plus ukončení ".").</span><span class="sxs-lookup"><span data-stu-id="d4d22-152">When no `$ORIGIN` is set, the default value used is the zone name as specified on the command line (plus the terminating ".").</span></span>
* <span data-ttu-id="d4d22-153">`$INCLUDE` a `$GENERATE` direktivy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="d4d22-153">The `$INCLUDE` and `$GENERATE` directives are not supported.</span></span>
* <span data-ttu-id="d4d22-154">Jsou podporovány těchto typů záznamů: A, AAAA, CNAME, MX, NS, SOA, SRV a TXT.</span><span class="sxs-lookup"><span data-stu-id="d4d22-154">These record types are supported: A, AAAA, CNAME, MX, NS, SOA, SRV, and TXT.</span></span>
* <span data-ttu-id="d4d22-155">Záznam SOA je vytvářena automaticky nástrojem Azure DNS, když je vytvořena zóna.</span><span class="sxs-lookup"><span data-stu-id="d4d22-155">The SOA record is created automatically by Azure DNS when a zone is created.</span></span> <span data-ttu-id="d4d22-156">Když importujete soubor zóny, jsou všechny parametry SOA převzat ze souboru zóny *s výjimkou* `host` parametr.</span><span class="sxs-lookup"><span data-stu-id="d4d22-156">When you import a zone file, all SOA parameters are taken from the zone file *except* the `host` parameter.</span></span> <span data-ttu-id="d4d22-157">Tento parametr používá hodnotu poskytovanou infrastrukturou Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-157">This parameter uses the value provided by Azure DNS.</span></span> <span data-ttu-id="d4d22-158">Je to proto, že tento parametr musí odkazovat na název primárního serveru poskytuje Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-158">This is because this parameter must refer to the primary name server provided by Azure DNS.</span></span>
* <span data-ttu-id="d4d22-159">Záznam názvového serveru nastavit ve vrcholu zóny se také automaticky vytvoří Azure DNS při vytváření zóny.</span><span class="sxs-lookup"><span data-stu-id="d4d22-159">The name server record set at the zone apex is also created automatically by Azure DNS when the zone is created.</span></span> <span data-ttu-id="d4d22-160">Je importovat pouze hodnotu TTL této sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="d4d22-160">Only the TTL of this record set is imported.</span></span> <span data-ttu-id="d4d22-161">Tyto záznamy obsahovat názvy názvových serverů poskytuje Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-161">These records contain the name server names provided by Azure DNS.</span></span> <span data-ttu-id="d4d22-162">Záznam dat není přepsány hodnoty obsažené v souboru importované zóny.</span><span class="sxs-lookup"><span data-stu-id="d4d22-162">The record data is not overwritten by the values contained in the imported zone file.</span></span>
* <span data-ttu-id="d4d22-163">Během verzi Public Preview Azure DNS podporuje pouze jeden řetězec záznamů TXT.</span><span class="sxs-lookup"><span data-stu-id="d4d22-163">During Public Preview, Azure DNS supports only single-string TXT records.</span></span> <span data-ttu-id="d4d22-164">Jsou nahrazován záznamů TXT zřetězených a zkrácen na 255 znaků.</span><span class="sxs-lookup"><span data-stu-id="d4d22-164">Multistring TXT records are be concatenated and truncated to 255 characters.</span></span>

### <a name="cli-format-and-values"></a><span data-ttu-id="d4d22-165">Formát rozhraní příkazového řádku a hodnoty</span><span class="sxs-lookup"><span data-stu-id="d4d22-165">CLI format and values</span></span>

<span data-ttu-id="d4d22-166">Formát příkazu příkazového řádku Azure k importu zóny DNS je:</span><span class="sxs-lookup"><span data-stu-id="d4d22-166">The format of the Azure CLI command to import a DNS zone is:</span></span>

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="d4d22-167">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d4d22-167">Values:</span></span>

* <span data-ttu-id="d4d22-168">`<resource group>`je název skupiny prostředků pro zónu v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-168">`<resource group>` is the name of the resource group for the zone in Azure DNS.</span></span>
* <span data-ttu-id="d4d22-169">`<zone name>`je název zóny.</span><span class="sxs-lookup"><span data-stu-id="d4d22-169">`<zone name>` is the name of the zone.</span></span>
* <span data-ttu-id="d4d22-170">`<zone file name>`je cesta nebo název souboru zóny určených k importu.</span><span class="sxs-lookup"><span data-stu-id="d4d22-170">`<zone file name>` is the path/name of the zone file to be imported.</span></span>

<span data-ttu-id="d4d22-171">Pokud zóna s tímto názvem neexistuje ve skupině prostředků, vytvoří se pro vás.</span><span class="sxs-lookup"><span data-stu-id="d4d22-171">If a zone with this name does not exist in the resource group, it is created for you.</span></span> <span data-ttu-id="d4d22-172">Pokud zóna již existuje, importované sady záznamů jsou sloučeny s existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="d4d22-172">If the zone already exists, the imported record sets are merged with existing record sets.</span></span> <span data-ttu-id="d4d22-173">Pokud chcete přepsat existující sady záznamů, použijte `--force` možnost.</span><span class="sxs-lookup"><span data-stu-id="d4d22-173">To overwrite the existing record sets, use the `--force` option.</span></span>

<span data-ttu-id="d4d22-174">K ověření formát souboru zóny bez provedení ve skutečnosti importu, použijte `--parse-only` možnost.</span><span class="sxs-lookup"><span data-stu-id="d4d22-174">To verify the format of a zone file without actually importing it, use the `--parse-only` option.</span></span>

### <a name="step-1-import-a-zone-file"></a><span data-ttu-id="d4d22-175">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="d4d22-175">Step 1.</span></span> <span data-ttu-id="d4d22-176">Importovat soubor zóny</span><span class="sxs-lookup"><span data-stu-id="d4d22-176">Import a zone file</span></span>

<span data-ttu-id="d4d22-177">Chcete-li importovat soubor zóny pro zónu **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="d4d22-177">To import a zone file for the zone **contoso.com**.</span></span>

1. <span data-ttu-id="d4d22-178">Přihlaste se k předplatnému Azure pomocí Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="d4d22-178">Sign in to your Azure subscription by using the Azure CLI 1.0.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="d4d22-179">Vyberte předplatné, kde chcete vytvořit novou zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-179">Select the subscription where you want to create your new DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="d4d22-180">Azure DNS je jen správce prostředků služby Azure, takže Azure CLI musí přepnout do režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4d22-180">Azure DNS is an Azure Resource Manager-only service, so the Azure CLI must be switched to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="d4d22-181">Než použijete službu Azure DNS, je nutné zaregistrovat vaše předplatné používalo poskytovatel prostředků Microsoft.Network.</span><span class="sxs-lookup"><span data-stu-id="d4d22-181">Before you use the Azure DNS service, you must register your subscription to use the Microsoft.Network resource provider.</span></span> <span data-ttu-id="d4d22-182">(Jde o jednorázovou operaci u každého předplatného.)</span><span class="sxs-lookup"><span data-stu-id="d4d22-182">(This is a one-time operation for each subscription.)</span></span>

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. <span data-ttu-id="d4d22-183">Pokud nemáte již, musíte taky vytvořit skupinu prostředků Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4d22-183">If you don't have one already, you also need to create a Resource Manager resource group.</span></span>

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. <span data-ttu-id="d4d22-184">Chcete-li importovat zóny **contoso.com** ze souboru **contoso.com.txt** do nové zóny DNS ve skupině prostředků **myresourcegroup**, spusťte příkaz `azure network dns zone import`.</span><span class="sxs-lookup"><span data-stu-id="d4d22-184">To import the zone **contoso.com** from the file **contoso.com.txt** into a new DNS zone in the resource group **myresourcegroup**, run the command `azure network dns zone import`.</span></span><BR><span data-ttu-id="d4d22-185">Tento příkaz načte soubor zóny a analyzovat ho.</span><span class="sxs-lookup"><span data-stu-id="d4d22-185">This command loads the zone file and parse it.</span></span> <span data-ttu-id="d4d22-186">Příkaz spustí řadu příkazů ve službě Azure DNS k vytvoření zóny a sady všech záznamů v zóně.</span><span class="sxs-lookup"><span data-stu-id="d4d22-186">The command executes a series of commands on the Azure DNS service to create the zone and all the record sets in the zone.</span></span> <span data-ttu-id="d4d22-187">Příkaz sestavy průběhu v okně konzoly společně s žádné chyby nebo upozornění.</span><span class="sxs-lookup"><span data-stu-id="d4d22-187">The command reports progress in the console window, along with any errors or warnings.</span></span> <span data-ttu-id="d4d22-188">Protože sady záznamů jsou vytvářeny v řadě, může trvat několik minut pro import souboru velké zóny.</span><span class="sxs-lookup"><span data-stu-id="d4d22-188">Because record sets are created in series, it may take a few minutes to import a large zone file.</span></span>

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a><span data-ttu-id="d4d22-189">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="d4d22-189">Step 2.</span></span> <span data-ttu-id="d4d22-190">Ověřte zóny</span><span class="sxs-lookup"><span data-stu-id="d4d22-190">Verify the zone</span></span>

<span data-ttu-id="d4d22-191">Ověření zónu DNS po importu souboru, můžete použít jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="d4d22-191">To verify the DNS zone after you import the file, you can use any one of the following methods:</span></span>

* <span data-ttu-id="d4d22-192">Pomocí následujícího příkazu příkazového řádku Azure můžete vytvořit seznam záznamů:</span><span class="sxs-lookup"><span data-stu-id="d4d22-192">You can list the records by using the following Azure CLI command:</span></span>

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* <span data-ttu-id="d4d22-193">Můžete vytvořit seznam záznamů pomocí rutiny prostředí PowerShell `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="d4d22-193">You can list the records by using the PowerShell cmdlet `Get-AzureRmDnsRecordSet`.</span></span>
* <span data-ttu-id="d4d22-194">Můžete použít `nslookup` ověření překlad názvů pro záznamy.</span><span class="sxs-lookup"><span data-stu-id="d4d22-194">You can use `nslookup` to verify name resolution for the records.</span></span> <span data-ttu-id="d4d22-195">Protože ještě není přidělena zóny, budete muset explicitně zadat správné názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-195">Because the zone isn't delegated yet, you need to specify the correct Azure DNS name servers explicitly.</span></span> <span data-ttu-id="d4d22-196">Následující příklad ukazuje, jak načíst názvy názvových serverů, které jsou přiřazeny k zóně.</span><span class="sxs-lookup"><span data-stu-id="d4d22-196">The following sample shows how to retrieve the name server names assigned to the zone.</span></span> <span data-ttu-id="d4d22-197">IT také ukazuje postup dotazování záznamů "www" pomocí `nslookup`.</span><span class="sxs-lookup"><span data-stu-id="d4d22-197">IT also shows how to query the "www" record by using `nslookup`.</span></span>

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/.../resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a><span data-ttu-id="d4d22-198">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="d4d22-198">Step 3.</span></span> <span data-ttu-id="d4d22-199">Aktualizovat delegování DNS</span><span class="sxs-lookup"><span data-stu-id="d4d22-199">Update DNS delegation</span></span>

<span data-ttu-id="d4d22-200">Po ověření, že správně naimportované zóny, potřebujete aktualizovat delegování DNS tak, aby odkazoval na názvové servery Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-200">After you have verified that the zone has been imported correctly, you need to update the DNS delegation to point to the Azure DNS name servers.</span></span> <span data-ttu-id="d4d22-201">Další informace najdete v článku [aktualizovat delegování DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="d4d22-201">For more information, see the article [Update the DNS delegation](dns-domain-delegation.md).</span></span>

## <a name="export-a-dns-zone-file-from-azure-dns"></a><span data-ttu-id="d4d22-202">Exportovat souboru zóny DNS z Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-202">Export a DNS zone file from Azure DNS</span></span>

<span data-ttu-id="d4d22-203">Formát příkazu příkazového řádku Azure k importu zóny DNS je:</span><span class="sxs-lookup"><span data-stu-id="d4d22-203">The format of the Azure CLI command to import a DNS zone is:</span></span>

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="d4d22-204">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d4d22-204">Values:</span></span>

* <span data-ttu-id="d4d22-205">`<resource group>`je název skupiny prostředků pro zónu v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-205">`<resource group>` is the name of the resource group for the zone in Azure DNS.</span></span>
* <span data-ttu-id="d4d22-206">`<zone name>`je název zóny.</span><span class="sxs-lookup"><span data-stu-id="d4d22-206">`<zone name>` is the name of the zone.</span></span>
* <span data-ttu-id="d4d22-207">`<zone file name>`je cesta nebo název souboru zóny export.</span><span class="sxs-lookup"><span data-stu-id="d4d22-207">`<zone file name>` is the path/name of the zone file to be exported.</span></span>

<span data-ttu-id="d4d22-208">Jako v zóně importu můžete nejprve nutné se přihlásit, zvolte vaše předplatné a konfigurace rozhraní příkazového řádku Azure pro použití režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4d22-208">As with the zone import, you first need to sign in, choose your subscription, and configure the Azure CLI to use Resource Manager mode.</span></span>

### <a name="to-export-a-zone-file"></a><span data-ttu-id="d4d22-209">Chcete-li exportovat soubor zóny</span><span class="sxs-lookup"><span data-stu-id="d4d22-209">To export a zone file</span></span>

1. <span data-ttu-id="d4d22-210">Přihlaste se k předplatnému Azure pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="d4d22-210">Sign in to your Azure subscription by using the Azure CLI.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="d4d22-211">Vyberte předplatné, kde chcete vytvořit zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="d4d22-211">Select the subscription where you want to create your DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="d4d22-212">Azure DNS je služba Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4d22-212">Azure DNS is an Azure Resource Manager-only service.</span></span> <span data-ttu-id="d4d22-213">Azure CLI musí přepnout do režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4d22-213">The Azure CLI must be switched to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="d4d22-214">Chcete-li exportovat stávající zóny DNS **contoso.com** ve skupině prostředků **myresourcegroup** do souboru **contoso.com.txt** (v aktuální složce), spusťte `azure network dns zone export`.</span><span class="sxs-lookup"><span data-stu-id="d4d22-214">To export the existing Azure DNS zone **contoso.com** in resource group **myresourcegroup** to the file **contoso.com.txt** (in the current folder), run `azure network dns zone export`.</span></span> <span data-ttu-id="d4d22-215">Tento příkaz volá službu Azure DNS vytvořit výčet sady záznamů v zóně a exportovat výsledky do souboru zóny vazby kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="d4d22-215">This command  calls the Azure DNS service to enumerate record sets in the zone and export the results to a BIND-compatible zone file.</span></span>

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
