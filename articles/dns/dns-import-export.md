---
title: "aaaImport a export zónu domény souboru tooAzure DNS pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak tooimport a export DNS zóny souboru tooAzure DNS pomocí Azure CLI 1.0"
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
ms.openlocfilehash: 4c3163395e151e9934c730349b828c612491016f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-a-dns-zone-file-using-hello-azure-cli-10"></a><span data-ttu-id="f69ac-103">Import a export souboru zóny DNS pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f69ac-103">Import and export a DNS zone file using hello Azure CLI 1.0</span></span> 

<span data-ttu-id="f69ac-104">Tento článek vás provede jak hello tooimport a export souborů zóny DNS pro Azure DNS pomocí Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="f69ac-104">This article walks you through how tooimport and export DNS zone files for Azure DNS using hello Azure CLI 1.0.</span></span>

## <a name="introduction-toodns-zone-migration"></a><span data-ttu-id="f69ac-105">Úvod tooDNS zóny migrace</span><span class="sxs-lookup"><span data-stu-id="f69ac-105">Introduction tooDNS zone migration</span></span>

<span data-ttu-id="f69ac-106">Soubor zóny DNS je textový soubor, který obsahuje podrobnosti o každé systému DNS (Domain Name) záznamu v zóně hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-106">A DNS zone file is a text file that contains details of every Domain Name System (DNS) record in hello zone.</span></span> <span data-ttu-id="f69ac-107">Postupuje standardní formát, takže je vhodné pro přenos mezi systémy DNS záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-107">It follows a standard format, making it suitable for transferring DNS records between DNS systems.</span></span> <span data-ttu-id="f69ac-108">Pomocí souboru zóny je rychlé, spolehlivé a pohodlný způsob tootransfer zóny DNS do nebo z Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-108">Using a zone file is a quick, reliable, and convenient way tootransfer a DNS zone into or out of Azure DNS.</span></span>

<span data-ttu-id="f69ac-109">Azure DNS podporuje import a export souborů zóny pomocí hello rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="f69ac-109">Azure DNS supports importing and exporting zone files by using hello Azure command-line interface (CLI).</span></span> <span data-ttu-id="f69ac-110">Import souboru zóny je **není** prostřednictvím prostředí Azure PowerShell nebo hello portál Azure momentálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="f69ac-110">Zone file import is **not** currently supported via Azure PowerShell or hello Azure portal.</span></span>

<span data-ttu-id="f69ac-111">Hello Azure CLI 1.0 je nástroj příkazového řádku a platformy pro správu služby Azure.</span><span class="sxs-lookup"><span data-stu-id="f69ac-111">hello Azure CLI 1.0 is a cross-platform command-line tool used for managing Azure services.</span></span> <span data-ttu-id="f69ac-112">Je k dispozici pro platformy Windows, Mac a Linux hello z hello [stránky Azure stahování](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f69ac-112">It is available for hello Windows, Mac, and Linux platforms from hello [Azure downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="f69ac-113">Podpora více platforem je důležité pro import a export souborů zóny, protože hello nejběžnější název serverového softwaru [vazby](https://www.isc.org/downloads/bind/), obvykle běží na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="f69ac-113">Cross-platform support is important for importing and exporting zone files, because hello most common name server software, [BIND](https://www.isc.org/downloads/bind/), typically runs on Linux.</span></span>

> [!NOTE]
> <span data-ttu-id="f69ac-114">Aktuálně neexistují dvě verze hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f69ac-114">There are currently two versions of hello Azure CLI.</span></span> <span data-ttu-id="f69ac-115">CLI1.0 je založena na Node.js a má příkazy počínaje "azure".</span><span class="sxs-lookup"><span data-stu-id="f69ac-115">CLI1.0 is based on Node.js, and has commands beginning with "azure".</span></span>
> <span data-ttu-id="f69ac-116">CLI2.0 je založena na Python a má příkazy počínaje "az".</span><span class="sxs-lookup"><span data-stu-id="f69ac-116">CLI2.0 is based on Python and has commands beginning with "az".</span></span> <span data-ttu-id="f69ac-117">Při importu souboru zóny je podporovaná v obou verzích, doporučujeme používat příkazy CLI1.0 hello, jak je popsáno v této stránce.</span><span class="sxs-lookup"><span data-stu-id="f69ac-117">While zone file import is supported in both versions, we recommend using hello CLI1.0 commands, as described in this page.</span></span>

## <a name="obtain-your-existing-dns-zone-file"></a><span data-ttu-id="f69ac-118">Získat existující soubor zóny DNS</span><span class="sxs-lookup"><span data-stu-id="f69ac-118">Obtain your existing DNS zone file</span></span>

<span data-ttu-id="f69ac-119">Před importem souboru zóny DNS do Azure DNS, musíte tooobtain kopii souboru zóny hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-119">Before you import a DNS zone file into Azure DNS, you need tooobtain a copy of hello zone file.</span></span> <span data-ttu-id="f69ac-120">Hello zdroj tohoto souboru závisí na je aktuálně hostitelem zóny DNS hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-120">hello source of this file depends on where hello DNS zone is currently hosted.</span></span>

* <span data-ttu-id="f69ac-121">Pokud zónu DNS je hostitelem služby partnera (například doménového registrátora, vyhrazené poskytovatele hostingu DNS nebo poskytovatele alternativní cloudu), měl by poskytnout služby hello možnost toodownload souboru zóny DNS hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-121">If your DNS zone is hosted by a partner service (such as a domain registrar, dedicated DNS hosting provider, or alternative cloud provider), that service should provide hello ability toodownload hello DNS zone file.</span></span>
* <span data-ttu-id="f69ac-122">Pokud DNS systému Windows je hostitelem zóny DNS, je hello výchozí složku pro soubory zóny hello **%systemroot%\system32\dns**.</span><span class="sxs-lookup"><span data-stu-id="f69ac-122">If your DNS zone is hosted on Windows DNS, hello default folder for hello zone files is **%systemroot%\system32\dns**.</span></span> <span data-ttu-id="f69ac-123">soubor zóny tooeach úplná cesta Hello také ukazuje na hello **Obecné** karty hello DNS konzoly.</span><span class="sxs-lookup"><span data-stu-id="f69ac-123">hello full path tooeach zone file also shows on hello **General** tab of hello DNS console.</span></span>
* <span data-ttu-id="f69ac-124">Pokud je hostovaná zóny DNS pomocí vazby, hello umístění souboru hello zóny pro každou zónu jsou uvedeny v souboru konfigurace vazby hello **named.conf**.</span><span class="sxs-lookup"><span data-stu-id="f69ac-124">If your DNS zone is hosted by using BIND, hello location of hello zone file for each zone is specified in hello BIND configuration file **named.conf**.</span></span>

> [!NOTE]
> <span data-ttu-id="f69ac-125">Zóny soubory stahované z GoDaddy mají mírně nestandardní formát.</span><span class="sxs-lookup"><span data-stu-id="f69ac-125">Zone files downloaded from GoDaddy have a slightly nonstandard format.</span></span> <span data-ttu-id="f69ac-126">Tuto funkci potřebujete toocorrect před importem tyto soubory zóny do Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-126">You need toocorrect this before you import these zone files into Azure DNS.</span></span>
>
> <span data-ttu-id="f69ac-127">Názvy DNS v hello RDATA každý záznam DNS jsou určené jako plně kvalifikované názvy, ale nemají ukončující "." To znamená, že se interpretují jinými systémy DNS jako relativních názvů.</span><span class="sxs-lookup"><span data-stu-id="f69ac-127">DNS names in hello RDATA of each DNS record are specified as fully qualified names, but they don't have a terminating "." This means they are interpreted by other DNS systems as relative names.</span></span> <span data-ttu-id="f69ac-128">Je třeba tooedit hello zóny souboru tooappend hello ukončení "." tootheir názvy před importem do Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-128">You need tooedit hello zone file tooappend hello terminating "." tootheir names before you import them into Azure DNS.</span></span>
>
> <span data-ttu-id="f69ac-129">Například hello záznam CNAME "v doméně contoso.com CNAME www 3600" by mělo být změněno příliš "contoso.com IN CNAME www 3600."</span><span class="sxs-lookup"><span data-stu-id="f69ac-129">For example, hello CNAME record "www 3600 IN CNAME contoso.com" should be changed too"www 3600 IN CNAME contoso.com."</span></span>
> <span data-ttu-id="f69ac-130">(s ukončující ".").</span><span class="sxs-lookup"><span data-stu-id="f69ac-130">(with a terminating ".").</span></span>

## <a name="import-a-dns-zone-file-into-azure-dns"></a><span data-ttu-id="f69ac-131">Import souboru zóny DNS do Azure DNS</span><span class="sxs-lookup"><span data-stu-id="f69ac-131">Import a DNS zone file into Azure DNS</span></span>

<span data-ttu-id="f69ac-132">Import souboru zóny vytvoří nové zóny v Azure DNS, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="f69ac-132">Importing a zone file creates a new zone in Azure DNS if one does not already exist.</span></span> <span data-ttu-id="f69ac-133">Pokud zóna hello již existuje, hello sad záznamů v souboru zóny hello je potřeba sloučit s hello existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="f69ac-133">If hello zone already exists, hello record sets in hello zone file must be merged with hello existing record sets.</span></span>

### <a name="merge-behavior"></a><span data-ttu-id="f69ac-134">Sloučení chování</span><span class="sxs-lookup"><span data-stu-id="f69ac-134">Merge behavior</span></span>

* <span data-ttu-id="f69ac-135">Ve výchozím nastavení jsou sloučeny stávající a nové sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="f69ac-135">By default, existing and new record sets are merged.</span></span> <span data-ttu-id="f69ac-136">Jsou identické záznamy v sadě záznamů sloučené zrušte duplicitní.</span><span class="sxs-lookup"><span data-stu-id="f69ac-136">Identical records within a merged record set are de-duplicated.</span></span>
* <span data-ttu-id="f69ac-137">Můžete také zadáním hello `--force` možnost, hello nahradí proces importu stávajícího záznamu, nastaví se nové sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="f69ac-137">Alternatively, by specifying hello `--force` option, hello import process replaces existing record sets with new record sets.</span></span> <span data-ttu-id="f69ac-138">Existující sady záznamů, které nemají odpovídající záznam v souboru importované zóny hello se neodeberou.</span><span class="sxs-lookup"><span data-stu-id="f69ac-138">Existing record sets that do not have a corresponding record set in hello imported zone file are not be removed.</span></span>
* <span data-ttu-id="f69ac-139">Když jsou sloučeny sady záznamů, hello čas toolive (TTL) dříve existující sady záznamů se používá.</span><span class="sxs-lookup"><span data-stu-id="f69ac-139">When record sets are merged, hello time toolive (TTL) of preexisting record sets is used.</span></span> <span data-ttu-id="f69ac-140">Když `--force` se používá, se používá hello TTL hello nové sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="f69ac-140">When `--force` is used, hello TTL of hello new record set is used.</span></span>
* <span data-ttu-id="f69ac-141">Začátek parametry záznam Authority (SOA) (s výjimkou `host`) jsou vždy převzat ze souboru hello importované zóny, bez ohledu na to, jestli se `--force` se používá.</span><span class="sxs-lookup"><span data-stu-id="f69ac-141">Start of Authority (SOA) parameters (except `host`) are always taken from hello imported zone file, regardless of whether `--force` is used.</span></span> <span data-ttu-id="f69ac-142">Podobně pro hello název serveru sady záznamů na vrcholu zóny hello, hello TTL je vždy převzat ze souboru importované zóny hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-142">Similarly, for hello name server record set at hello zone apex, hello TTL is always taken from hello imported zone file.</span></span>
* <span data-ttu-id="f69ac-143">Importovaný záznam CNAME nenahrazuje existující CNAME záznam s hello stejný název, pokud hello `--force` je zadán parametr.</span><span class="sxs-lookup"><span data-stu-id="f69ac-143">An imported CNAME record does not replace an existing CNAME record with hello same name unless hello `--force` parameter is specified.</span></span>
* <span data-ttu-id="f69ac-144">Když dojde ke konfliktu mezi záznam CNAME a jiný záznam hello stejný název, ale jiné zadejte (bez ohledu na to, který je existující nebo nové), se uchovávají hello stávajícího záznamu.</span><span class="sxs-lookup"><span data-stu-id="f69ac-144">When a conflict arises between a CNAME record and another record of hello same name but different type (regardless of which is existing or new), hello existing record is retained.</span></span> <span data-ttu-id="f69ac-145">Je nezávislé na použití hello `--force`.</span><span class="sxs-lookup"><span data-stu-id="f69ac-145">This is independent of hello use of `--force`.</span></span>

### <a name="additional-information-about-importing"></a><span data-ttu-id="f69ac-146">Další informace o importu</span><span class="sxs-lookup"><span data-stu-id="f69ac-146">Additional information about importing</span></span>

<span data-ttu-id="f69ac-147">Hello následující poznámky k poskytují další technické podrobnosti o hello zóny importu.</span><span class="sxs-lookup"><span data-stu-id="f69ac-147">hello following notes provide additional technical details about hello zone import process.</span></span>

* <span data-ttu-id="f69ac-148">Hello `$TTL` – direktiva je volitelný a je podporované.</span><span class="sxs-lookup"><span data-stu-id="f69ac-148">hello `$TTL` directive is optional, and it is supported.</span></span> <span data-ttu-id="f69ac-149">Pokud ne `$TTL` – direktiva je zadána, jsou importovány záznamy bez explicitního TTL nastavit výchozí tooa TTL 3 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="f69ac-149">When no `$TTL` directive is given, records without an explicit TTL are imported set tooa default TTL of 3600 seconds.</span></span> <span data-ttu-id="f69ac-150">Když dva zaznamenává v hello stejné sady záznamů zadejte jiný TTLs, hello nižší hodnota se používá.</span><span class="sxs-lookup"><span data-stu-id="f69ac-150">When two records in hello same record set specify different TTLs, hello lower value is used.</span></span>
* <span data-ttu-id="f69ac-151">Hello `$ORIGIN` – direktiva je volitelný a je podporované.</span><span class="sxs-lookup"><span data-stu-id="f69ac-151">hello `$ORIGIN` directive is optional, and it is supported.</span></span> <span data-ttu-id="f69ac-152">Pokud ne `$ORIGIN` nastavena výchozí hello používá, je název zóny hello jako zadaného na příkazovém řádku hello (plus ukončení hello ".").</span><span class="sxs-lookup"><span data-stu-id="f69ac-152">When no `$ORIGIN` is set, hello default value used is hello zone name as specified on hello command line (plus hello terminating ".").</span></span>
* <span data-ttu-id="f69ac-153">Hello `$INCLUDE` a `$GENERATE` direktivy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="f69ac-153">hello `$INCLUDE` and `$GENERATE` directives are not supported.</span></span>
* <span data-ttu-id="f69ac-154">Jsou podporovány těchto typů záznamů: A, AAAA, CNAME, MX, NS, SOA, SRV a TXT.</span><span class="sxs-lookup"><span data-stu-id="f69ac-154">These record types are supported: A, AAAA, CNAME, MX, NS, SOA, SRV, and TXT.</span></span>
* <span data-ttu-id="f69ac-155">Hello záznam SOA je vytvářena automaticky nástrojem Azure DNS, když je vytvořena zóna.</span><span class="sxs-lookup"><span data-stu-id="f69ac-155">hello SOA record is created automatically by Azure DNS when a zone is created.</span></span> <span data-ttu-id="f69ac-156">Když importujete soubor zóny, jsou všechny parametry SOA převzat ze souboru zóny hello *s výjimkou* hello `host` parametr.</span><span class="sxs-lookup"><span data-stu-id="f69ac-156">When you import a zone file, all SOA parameters are taken from hello zone file *except* hello `host` parameter.</span></span> <span data-ttu-id="f69ac-157">Tento parametr používá hello hodnotu poskytovanou infrastrukturou Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-157">This parameter uses hello value provided by Azure DNS.</span></span> <span data-ttu-id="f69ac-158">Je to proto, že tento parametr musí odkazovat toohello název primárního serveru poskytuje Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-158">This is because this parameter must refer toohello primary name server provided by Azure DNS.</span></span>
* <span data-ttu-id="f69ac-159">záznam názvového serveru Hello nastavit na vrcholu zóny hello se také automaticky vytvoří Azure DNS při vytvoření zóny hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-159">hello name server record set at hello zone apex is also created automatically by Azure DNS when hello zone is created.</span></span> <span data-ttu-id="f69ac-160">Pouze hello TTL této sady záznamů je naimportována.</span><span class="sxs-lookup"><span data-stu-id="f69ac-160">Only hello TTL of this record set is imported.</span></span> <span data-ttu-id="f69ac-161">Tyto záznamy obsahovat názvy názvových serverů hello poskytuje Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-161">These records contain hello name server names provided by Azure DNS.</span></span> <span data-ttu-id="f69ac-162">data záznamů Hello není přepsány hello hodnoty obsažené v souboru importované zóny hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-162">hello record data is not overwritten by hello values contained in hello imported zone file.</span></span>
* <span data-ttu-id="f69ac-163">Během verzi Public Preview Azure DNS podporuje pouze jeden řetězec záznamů TXT.</span><span class="sxs-lookup"><span data-stu-id="f69ac-163">During Public Preview, Azure DNS supports only single-string TXT records.</span></span> <span data-ttu-id="f69ac-164">Nahrazován záznamů TXT jsou být zřetězených a zkrácený too255 znaků.</span><span class="sxs-lookup"><span data-stu-id="f69ac-164">Multistring TXT records are be concatenated and truncated too255 characters.</span></span>

### <a name="cli-format-and-values"></a><span data-ttu-id="f69ac-165">Formát rozhraní příkazového řádku a hodnoty</span><span class="sxs-lookup"><span data-stu-id="f69ac-165">CLI format and values</span></span>

<span data-ttu-id="f69ac-166">Formát Hello hello rozhraní příkazového řádku Azure příkaz tooimport zónu DNS je:</span><span class="sxs-lookup"><span data-stu-id="f69ac-166">hello format of hello Azure CLI command tooimport a DNS zone is:</span></span>

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="f69ac-167">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f69ac-167">Values:</span></span>

* <span data-ttu-id="f69ac-168">`<resource group>`je název skupiny prostředků hello hello hello zóny v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-168">`<resource group>` is hello name of hello resource group for hello zone in Azure DNS.</span></span>
* <span data-ttu-id="f69ac-169">`<zone name>`je název hello hello zóny.</span><span class="sxs-lookup"><span data-stu-id="f69ac-169">`<zone name>` is hello name of hello zone.</span></span>
* <span data-ttu-id="f69ac-170">`<zone file name>`je hello cesta a název toobe soubor zóny hello importovat.</span><span class="sxs-lookup"><span data-stu-id="f69ac-170">`<zone file name>` is hello path/name of hello zone file toobe imported.</span></span>

<span data-ttu-id="f69ac-171">Pokud zóna s tímto názvem neexistuje ve skupině prostředků hello, vytvoří se pro vás.</span><span class="sxs-lookup"><span data-stu-id="f69ac-171">If a zone with this name does not exist in hello resource group, it is created for you.</span></span> <span data-ttu-id="f69ac-172">Pokud hello zóně už existuje, hello sady importovaných záznamů jsou sloučeny s existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="f69ac-172">If hello zone already exists, hello imported record sets are merged with existing record sets.</span></span> <span data-ttu-id="f69ac-173">toooverwrite hello existující sady záznamů, použijte hello `--force` možnost.</span><span class="sxs-lookup"><span data-stu-id="f69ac-173">toooverwrite hello existing record sets, use hello `--force` option.</span></span>

<span data-ttu-id="f69ac-174">tooverify hello formát souboru zóny bez jeho použití hello ve skutečnosti import `--parse-only` možnost.</span><span class="sxs-lookup"><span data-stu-id="f69ac-174">tooverify hello format of a zone file without actually importing it, use hello `--parse-only` option.</span></span>

### <a name="step-1-import-a-zone-file"></a><span data-ttu-id="f69ac-175">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="f69ac-175">Step 1.</span></span> <span data-ttu-id="f69ac-176">Importovat soubor zóny</span><span class="sxs-lookup"><span data-stu-id="f69ac-176">Import a zone file</span></span>

<span data-ttu-id="f69ac-177">tooimport souboru zóny pro zónu hello **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="f69ac-177">tooimport a zone file for hello zone **contoso.com**.</span></span>

1. <span data-ttu-id="f69ac-178">Přihlaste se tooyour předplatného Azure pomocí Azure CLI 1.0 hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-178">Sign in tooyour Azure subscription by using hello Azure CLI 1.0.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="f69ac-179">Vyberte předplatné hello místo toocreate nové zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-179">Select hello subscription where you want toocreate your new DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="f69ac-180">Azure DNS je jen správce prostředků služby Azure, takže hello rozhraní příkazového řádku Azure musí být vypnuté tooResource režimu správce.</span><span class="sxs-lookup"><span data-stu-id="f69ac-180">Azure DNS is an Azure Resource Manager-only service, so hello Azure CLI must be switched tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="f69ac-181">Než použijete hello služba Azure DNS, je nutné zaregistrovat poskytovatel prostředků Microsoft.Network vaše předplatné toouse hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-181">Before you use hello Azure DNS service, you must register your subscription toouse hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="f69ac-182">(Jde o jednorázovou operaci u každého předplatného.)</span><span class="sxs-lookup"><span data-stu-id="f69ac-182">(This is a one-time operation for each subscription.)</span></span>

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. <span data-ttu-id="f69ac-183">Pokud nemáte již, musíte taky toocreate skupinu prostředků Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f69ac-183">If you don't have one already, you also need toocreate a Resource Manager resource group.</span></span>

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. <span data-ttu-id="f69ac-184">zóny hello tooimport **contoso.com** ze souboru hello **contoso.com.txt** do nové zóny DNS ve skupině prostředků hello **myresourcegroup**, spusťte příkaz hello `azure network dns zone import`.</span><span class="sxs-lookup"><span data-stu-id="f69ac-184">tooimport hello zone **contoso.com** from hello file **contoso.com.txt** into a new DNS zone in hello resource group **myresourcegroup**, run hello command `azure network dns zone import`.</span></span><BR><span data-ttu-id="f69ac-185">Tento příkaz načte soubor zóny hello a analyzovat ho.</span><span class="sxs-lookup"><span data-stu-id="f69ac-185">This command loads hello zone file and parse it.</span></span> <span data-ttu-id="f69ac-186">příkaz Hello provede řadu příkazů hello Azure DNS služby toocreate hello zóny a nastaví všechny hello záznamu v zóně hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-186">hello command executes a series of commands on hello Azure DNS service toocreate hello zone and all hello record sets in hello zone.</span></span> <span data-ttu-id="f69ac-187">příkaz Hello sestavy průběhu v okně konzoly hello společně s žádné chyby nebo upozornění.</span><span class="sxs-lookup"><span data-stu-id="f69ac-187">hello command reports progress in hello console window, along with any errors or warnings.</span></span> <span data-ttu-id="f69ac-188">Protože sady záznamů jsou vytvářeny v řadě, může trvat několik minut tooimport souboru velké zóny.</span><span class="sxs-lookup"><span data-stu-id="f69ac-188">Because record sets are created in series, it may take a few minutes tooimport a large zone file.</span></span>

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-hello-zone"></a><span data-ttu-id="f69ac-189">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="f69ac-189">Step 2.</span></span> <span data-ttu-id="f69ac-190">Ověřte hello zóny</span><span class="sxs-lookup"><span data-stu-id="f69ac-190">Verify hello zone</span></span>

<span data-ttu-id="f69ac-191">tooverify hello zóny DNS po importu souboru hello, můžete použít kterékoli z hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="f69ac-191">tooverify hello DNS zone after you import hello file, you can use any one of hello following methods:</span></span>

* <span data-ttu-id="f69ac-192">Pomocí následujícího příkazu příkazového řádku Azure CLI hello můžete vytvořit seznam záznamů hello:</span><span class="sxs-lookup"><span data-stu-id="f69ac-192">You can list hello records by using hello following Azure CLI command:</span></span>

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* <span data-ttu-id="f69ac-193">Můžete vytvořit seznam záznamů hello pomocí rutiny prostředí PowerShell hello `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="f69ac-193">You can list hello records by using hello PowerShell cmdlet `Get-AzureRmDnsRecordSet`.</span></span>
* <span data-ttu-id="f69ac-194">Můžete použít `nslookup` tooverify překlad názvů pro záznamy hello.</span><span class="sxs-lookup"><span data-stu-id="f69ac-194">You can use `nslookup` tooverify name resolution for hello records.</span></span> <span data-ttu-id="f69ac-195">Protože ještě není přidělena hello zóny, toospecify hello správné názvových serverů Azure DNS je třeba explicitně.</span><span class="sxs-lookup"><span data-stu-id="f69ac-195">Because hello zone isn't delegated yet, you need toospecify hello correct Azure DNS name servers explicitly.</span></span> <span data-ttu-id="f69ac-196">Hello následující příklad ukazuje, jak přiřazovat názvy názvových serverů tooretrieve hello toohello zóny.</span><span class="sxs-lookup"><span data-stu-id="f69ac-196">hello following sample shows how tooretrieve hello name server names assigned toohello zone.</span></span> <span data-ttu-id="f69ac-197">IT také ukazuje, jak tooquery hello "www" zaznamenání pomocí `nslookup`.</span><span class="sxs-lookup"><span data-stu-id="f69ac-197">IT also shows how tooquery hello "www" record by using `nslookup`.</span></span>

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up hello DNS Record Set "@" of type "NS"
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

### <a name="step-3-update-dns-delegation"></a><span data-ttu-id="f69ac-198">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="f69ac-198">Step 3.</span></span> <span data-ttu-id="f69ac-199">Aktualizovat delegování DNS</span><span class="sxs-lookup"><span data-stu-id="f69ac-199">Update DNS delegation</span></span>

<span data-ttu-id="f69ac-200">Po ověření, že zóna hello importu správně, musíte tooupdate hello DNS delegování toopoint toohello názvové servery Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-200">After you have verified that hello zone has been imported correctly, you need tooupdate hello DNS delegation toopoint toohello Azure DNS name servers.</span></span> <span data-ttu-id="f69ac-201">Další informace najdete v článku hello [aktualizovat delegování DNS hello](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="f69ac-201">For more information, see hello article [Update hello DNS delegation](dns-domain-delegation.md).</span></span>

## <a name="export-a-dns-zone-file-from-azure-dns"></a><span data-ttu-id="f69ac-202">Exportovat souboru zóny DNS z Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-202">Export a DNS zone file from Azure DNS</span></span>

<span data-ttu-id="f69ac-203">Formát Hello hello rozhraní příkazového řádku Azure příkaz tooimport zónu DNS je:</span><span class="sxs-lookup"><span data-stu-id="f69ac-203">hello format of hello Azure CLI command tooimport a DNS zone is:</span></span>

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="f69ac-204">Hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f69ac-204">Values:</span></span>

* <span data-ttu-id="f69ac-205">`<resource group>`je název skupiny prostředků hello hello hello zóny v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-205">`<resource group>` is hello name of hello resource group for hello zone in Azure DNS.</span></span>
* <span data-ttu-id="f69ac-206">`<zone name>`je název hello hello zóny.</span><span class="sxs-lookup"><span data-stu-id="f69ac-206">`<zone name>` is hello name of hello zone.</span></span>
* <span data-ttu-id="f69ac-207">`<zone file name>`je hello cesta a název toobe soubor zóny hello exportovali.</span><span class="sxs-lookup"><span data-stu-id="f69ac-207">`<zone file name>` is hello path/name of hello zone file toobe exported.</span></span>

<span data-ttu-id="f69ac-208">Jako s importem hello zóny, musíte nejdřív toosign v, vyberte předplatné a nakonfigurujte režimu Resource Manager toouse hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f69ac-208">As with hello zone import, you first need toosign in, choose your subscription, and configure hello Azure CLI toouse Resource Manager mode.</span></span>

### <a name="tooexport-a-zone-file"></a><span data-ttu-id="f69ac-209">tooexport soubor zóny</span><span class="sxs-lookup"><span data-stu-id="f69ac-209">tooexport a zone file</span></span>

1. <span data-ttu-id="f69ac-210">Přihlaste se tooyour předplatného Azure pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="f69ac-210">Sign in tooyour Azure subscription by using hello Azure CLI.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="f69ac-211">Vyberte předplatné hello místo toocreate zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="f69ac-211">Select hello subscription where you want toocreate your DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="f69ac-212">Azure DNS je služba Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f69ac-212">Azure DNS is an Azure Resource Manager-only service.</span></span> <span data-ttu-id="f69ac-213">Hello rozhraní příkazového řádku Azure musí být vypnuté tooResource režimu správce.</span><span class="sxs-lookup"><span data-stu-id="f69ac-213">hello Azure CLI must be switched tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="f69ac-214">tooexport hello existující zóny DNS **contoso.com** ve skupině prostředků **myresourcegroup** toohello soubor **contoso.com.txt** (ve složce hello aktuální), spusťte `azure network dns zone export`.</span><span class="sxs-lookup"><span data-stu-id="f69ac-214">tooexport hello existing Azure DNS zone **contoso.com** in resource group **myresourcegroup** toohello file **contoso.com.txt** (in hello current folder), run `azure network dns zone export`.</span></span> <span data-ttu-id="f69ac-215">Tento příkaz, že volání hello tooenumerate služby Azure DNS sady záznamů v zóně hello a exportovat soubor zóny hello výsledky tooa vazby kompatibilní.</span><span class="sxs-lookup"><span data-stu-id="f69ac-215">This command  calls hello Azure DNS service tooenumerate record sets in hello zone and export hello results tooa BIND-compatible zone file.</span></span>

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
