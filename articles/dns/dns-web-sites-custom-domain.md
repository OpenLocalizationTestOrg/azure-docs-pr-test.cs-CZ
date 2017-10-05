---
title: "Vytvořit vlastní záznamy DNS pro webovou aplikaci | Microsoft Docs"
description: "Jak vytvořit vlastní doménu. záznamy DNS pro webovou aplikaci pomocí Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: b054a41ecd69ee1c802d8403fe4b25128f016e3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a><span data-ttu-id="177da-103">Vytvořit záznamy DNS pro webovou aplikaci ve vlastní domény</span><span class="sxs-lookup"><span data-stu-id="177da-103">Create DNS records for a web app in a custom domain</span></span>

<span data-ttu-id="177da-104">Azure DNS můžete použít k hostování vlastní doménu pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="177da-104">You can use Azure DNS to host a custom domain for your web apps.</span></span> <span data-ttu-id="177da-105">Například při vytváření webové aplikace Azure a mají vaši uživatelé k němu přístup, a to buď pomocí contoso.com nebo www.contoso.com jako plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="177da-105">For example, you are creating an Azure web app and you want your users to access it by either using contoso.com, or www.contoso.com as an FQDN.</span></span>

<span data-ttu-id="177da-106">Chcete-li to provést, je nutné vytvořit dva záznamy:</span><span class="sxs-lookup"><span data-stu-id="177da-106">To do this, you have to create two records:</span></span>

* <span data-ttu-id="177da-107">Kořenový "A" záznam odkazující na contoso.com</span><span class="sxs-lookup"><span data-stu-id="177da-107">A root "A" record pointing to contoso.com</span></span>
* <span data-ttu-id="177da-108">Záznam "CNAME" www název, který odkazuje na záznam a.</span><span class="sxs-lookup"><span data-stu-id="177da-108">A "CNAME" record for the www name that points to the A record</span></span>

<span data-ttu-id="177da-109">Mějte na paměti, že pokud vytváříte záznam pro webovou aplikaci v Azure, záznam A musí být ručně aktualizovat, pokud zdrojová IP adresa pro změny webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="177da-109">Keep in mind that if you create an A record for a web app in Azure, the A record must be manually updated if the underlying IP address for the web app changes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="177da-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="177da-110">Before you begin</span></span>

<span data-ttu-id="177da-111">Než začnete, musíte nejprve vytvořit zónu DNS v Azure DNS a delegovat zóny v registrátora do Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="177da-111">Before you begin, you must first create a DNS zone in Azure DNS, and delegate the zone in your registrar to Azure DNS.</span></span>

1. <span data-ttu-id="177da-112">Pokud chcete vytvořit zónu DNS, postupujte podle kroků v [vytvořit zónu DNS](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="177da-112">To create a DNS zone, follow the steps in [Create a DNS zone](dns-getstarted-create-dnszone.md).</span></span>
2. <span data-ttu-id="177da-113">Delegování DNS do Azure DNS, postupujte podle kroků v [delegování DNS domény](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="177da-113">To delegate your DNS to Azure DNS, follow the steps in [DNS domain delegation](dns-domain-delegation.md).</span></span>

<span data-ttu-id="177da-114">Po vytvoření zóny a delegování do Azure DNS, potom můžete vytvořit záznamy pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="177da-114">After creating a zone and delegating it to Azure DNS, you can then create records for your custom domain.</span></span>

## <a name="1-create-an-a-record-for-your-custom-domain"></a><span data-ttu-id="177da-115">1. Vytvoření záznamu A pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="177da-115">1. Create an A record for your custom domain</span></span>

<span data-ttu-id="177da-116">Záznam A slouží k mapování názvů na IP adresu.</span><span class="sxs-lookup"><span data-stu-id="177da-116">An A record is used to map a name to its IP address.</span></span> <span data-ttu-id="177da-117">V následujícím příkladu jsme se přiřadit jako záznam na adresu IPv4:</span><span class="sxs-lookup"><span data-stu-id="177da-117">In the following example we will assign @ as an A record to an IPv4 address:</span></span>

### <a name="step-1"></a><span data-ttu-id="177da-118">Krok 1</span><span class="sxs-lookup"><span data-stu-id="177da-118">Step 1</span></span>

<span data-ttu-id="177da-119">Vytvořte záznam a přiřaďte proměnnou $rs</span><span class="sxs-lookup"><span data-stu-id="177da-119">Create an A record and assign to a variable $rs</span></span>

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a><span data-ttu-id="177da-120">Krok 2</span><span class="sxs-lookup"><span data-stu-id="177da-120">Step 2</span></span>

<span data-ttu-id="177da-121">Přidejte hodnotu IPv4 do sady dříve vytvořenou záznamu "@" použití $rs proměnné přiřazené.</span><span class="sxs-lookup"><span data-stu-id="177da-121">Add the IPv4 value to the previously created record set "@" using the $rs variable assigned.</span></span> <span data-ttu-id="177da-122">IPv4 hodnotu přiřazenou bude IP adresa pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="177da-122">The IPv4 value assigned will be the IP address for your web app.</span></span>

<span data-ttu-id="177da-123">Najít IP adresu pro webovou aplikaci, postupujte podle kroků v [konfigurace vlastního názvu domény v Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="177da-123">To find the IP address for a web app, follow the steps in [Configure a custom domain name in Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a><span data-ttu-id="177da-124">Krok 3</span><span class="sxs-lookup"><span data-stu-id="177da-124">Step 3</span></span>

<span data-ttu-id="177da-125">Potvrďte změny provedené v sadě záznamů.</span><span class="sxs-lookup"><span data-stu-id="177da-125">Commit the changes to the record set.</span></span> <span data-ttu-id="177da-126">Použití `Set-AzureRMDnsRecordSet` odeslání změn na záznam nastavit do Azure DNS:</span><span class="sxs-lookup"><span data-stu-id="177da-126">Use `Set-AzureRMDnsRecordSet` to upload the changes to the record set to Azure DNS:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="177da-127">2. Vytvořte záznam CNAME pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="177da-127">2. Create a CNAME record for your custom domain</span></span>

<span data-ttu-id="177da-128">Pokud vaše doména je již spravován nástrojem Azure DNS (v tématu [delegování DNS domény](dns-domain-delegation.md), můžete použít následující příklad vytvoří záznam CNAME pro contoso.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="177da-128">If your domain is already managed by Azure DNS (see [DNS domain delegation](dns-domain-delegation.md), you can use the following the example to create a CNAME record for contoso.azurewebsites.net.</span></span>

### <a name="step-1"></a><span data-ttu-id="177da-129">Krok 1</span><span class="sxs-lookup"><span data-stu-id="177da-129">Step 1</span></span>

<span data-ttu-id="177da-130">Otevřete prostředí PowerShell a vytvořit novou sadu záznamů CNAME a proměnné $rs přiřadit.</span><span class="sxs-lookup"><span data-stu-id="177da-130">Open PowerShell and create a new CNAME record set and assign to a variable $rs.</span></span> <span data-ttu-id="177da-131">Tento příklad vytvoří sadu záznamů typu CNAME s "time to live" 600 sekund v zóně DNS s názvem "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="177da-131">This example will create a record set type CNAME with a "time to live" of 600 seconds in DNS zone named "contoso.com".</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="177da-132">Dalším příkladem je tato odpověď.</span><span class="sxs-lookup"><span data-stu-id="177da-132">The following example is the response.</span></span>

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a><span data-ttu-id="177da-133">Krok 2</span><span class="sxs-lookup"><span data-stu-id="177da-133">Step 2</span></span>

<span data-ttu-id="177da-134">Po vytvoření sady záznamů CNAME, budete muset vytvořit hodnotu alias, který bude odkazovat na webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="177da-134">Once the CNAME record set is created, you need to create an alias value which will point to the web app.</span></span>

<span data-ttu-id="177da-135">Pomocí dříve přiřazenou proměnnou "$rs" můžete použít následující příkaz prostředí PowerShell pro vytvoření aliasu pro contoso.azurewebsites.net aplikace webové.</span><span class="sxs-lookup"><span data-stu-id="177da-135">Using the previously assigned variable "$rs" you can use the PowerShell command below to create the alias for the web app contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

<span data-ttu-id="177da-136">Dalším příkladem je tato odpověď.</span><span class="sxs-lookup"><span data-stu-id="177da-136">The following example is the response.</span></span>

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a><span data-ttu-id="177da-137">Krok 3</span><span class="sxs-lookup"><span data-stu-id="177da-137">Step 3</span></span>

<span data-ttu-id="177da-138">Potvrzení změn pomocí `Set-AzureRMDnsRecordSet` rutiny:</span><span class="sxs-lookup"><span data-stu-id="177da-138">Commit the changes using the `Set-AzureRMDnsRecordSet` cmdlet:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="177da-139">Můžete ověřit záznam byl vytvořen správně dotazováním "www.contoso.com" pomocí nástroje nslookup, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="177da-139">You can validate the record was created correctly by querying the "www.contoso.com" using nslookup, as shown below:</span></span>

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a><span data-ttu-id="177da-140">Vytvoření záznamů "awverify" pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="177da-140">Create an "awverify" record for web apps</span></span>

<span data-ttu-id="177da-141">Pokud se rozhodnete použít záznam pro vaši webovou aplikaci, musí projít procesem ověření Ujistěte se, že vlastníte vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="177da-141">If you decide to use an A record for your web app, you must go through a verification process to ensure you own the custom domain.</span></span> <span data-ttu-id="177da-142">Tento krok ověření se provádí tak, že vytvoříte speciální záznam CNAME s názvem "awverify".</span><span class="sxs-lookup"><span data-stu-id="177da-142">This verification step is done by creating a special CNAME record named "awverify".</span></span> <span data-ttu-id="177da-143">Tato část se týká pouze záznamy.</span><span class="sxs-lookup"><span data-stu-id="177da-143">This section applies to A records only.</span></span>

### <a name="step-1"></a><span data-ttu-id="177da-144">Krok 1</span><span class="sxs-lookup"><span data-stu-id="177da-144">Step 1</span></span>

<span data-ttu-id="177da-145">Vytvořte záznam "awverify".</span><span class="sxs-lookup"><span data-stu-id="177da-145">Create the "awverify" record.</span></span> <span data-ttu-id="177da-146">V následujícím příkladu vytvoříme záznam "aweverify" pro doménu contoso.com ověřit vlastnictví pro vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="177da-146">In the example below, we will create the "aweverify" record for contoso.com to verify ownership for the custom domain.</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="177da-147">Dalším příkladem je tato odpověď.</span><span class="sxs-lookup"><span data-stu-id="177da-147">The following example is the response.</span></span>

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a><span data-ttu-id="177da-148">Krok 2</span><span class="sxs-lookup"><span data-stu-id="177da-148">Step 2</span></span>

<span data-ttu-id="177da-149">Po vytvoření sady záznamů "awverify" přiřadíte sadu alias záznamů CNAME.</span><span class="sxs-lookup"><span data-stu-id="177da-149">Once the record set "awverify" is created, assign the CNAME record set alias.</span></span> <span data-ttu-id="177da-150">V následujícím příkladu jsme přiřadí nastavte alias na awverify.contoso.azurewebsites.net záznam CNAMe.</span><span class="sxs-lookup"><span data-stu-id="177da-150">In the example below, we will assign the CNAMe record set alias to awverify.contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

<span data-ttu-id="177da-151">Dalším příkladem je tato odpověď.</span><span class="sxs-lookup"><span data-stu-id="177da-151">The following example is the response.</span></span>

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a><span data-ttu-id="177da-152">Krok 3</span><span class="sxs-lookup"><span data-stu-id="177da-152">Step 3</span></span>

<span data-ttu-id="177da-153">Potvrzení změn pomocí `Set-AzureRMDnsRecordSet cmdlet`, jak ukazuje následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="177da-153">Commit the changes using the `Set-AzureRMDnsRecordSet cmdlet`, as shown in the command below.</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a><span data-ttu-id="177da-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="177da-154">Next steps</span></span>

<span data-ttu-id="177da-155">Postupujte podle kroků v [konfigurace vlastního názvu domény pro službu App Service](../app-service-web/web-sites-custom-domain-name.md) ke konfiguraci vaší webové aplikace na používat vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="177da-155">Follow the steps in [Configuring a custom domain name for App Service](../app-service-web/web-sites-custom-domain-name.md) to configure your web app to use a custom domain.</span></span>
