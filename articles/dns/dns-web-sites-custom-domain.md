---
title: "aaaCreate vlastní záznamy DNS pro webovou aplikaci | Microsoft Docs"
description: "Jak záznamy toocreate vlastní domény DNS pro webovou aplikaci pomocí Azure DNS."
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
ms.openlocfilehash: 070c808a55bab922eb624d99ae5c275d8eaa5aaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a><span data-ttu-id="00fa2-103">Vytvořit záznamy DNS pro webovou aplikaci ve vlastní domény</span><span class="sxs-lookup"><span data-stu-id="00fa2-103">Create DNS records for a web app in a custom domain</span></span>

<span data-ttu-id="00fa2-104">Azure DNS toohost vlastní doménu můžete použít pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="00fa2-104">You can use Azure DNS toohost a custom domain for your web apps.</span></span> <span data-ttu-id="00fa2-105">Například při vytváření webové aplikace Azure a chcete, aby vaši uživatelé tooaccess ho a to buď pomocí contoso.com nebo www.contoso.com jako plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="00fa2-105">For example, you are creating an Azure web app and you want your users tooaccess it by either using contoso.com, or www.contoso.com as an FQDN.</span></span>

<span data-ttu-id="00fa2-106">toodo, máte dva záznamy toocreate:</span><span class="sxs-lookup"><span data-stu-id="00fa2-106">toodo this, you have toocreate two records:</span></span>

* <span data-ttu-id="00fa2-107">Kořenový "A" záznamu polohovací toocontoso.com</span><span class="sxs-lookup"><span data-stu-id="00fa2-107">A root "A" record pointing toocontoso.com</span></span>
* <span data-ttu-id="00fa2-108">"Záznam CNAME" záznam pro hello www název, který ukazuje toohello záznam</span><span class="sxs-lookup"><span data-stu-id="00fa2-108">A "CNAME" record for hello www name that points toohello A record</span></span>

<span data-ttu-id="00fa2-109">Uvědomte si, pokud vytvoříte záznam pro webovou aplikaci v Azure, hello záznam je potřeba aktualizovat ručně pokud hello Zdrojová IP adresa pro změny hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="00fa2-109">Keep in mind that if you create an A record for a web app in Azure, hello A record must be manually updated if hello underlying IP address for hello web app changes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="00fa2-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="00fa2-110">Before you begin</span></span>

<span data-ttu-id="00fa2-111">Než začnete, musíte nejprve vytvořit zónu DNS v Azure DNS a delegovat hello zóny v tooAzure vašeho registrátora DNS.</span><span class="sxs-lookup"><span data-stu-id="00fa2-111">Before you begin, you must first create a DNS zone in Azure DNS, and delegate hello zone in your registrar tooAzure DNS.</span></span>

1. <span data-ttu-id="00fa2-112">toocreate zóny DNS, postupujte podle kroků hello v [vytvořit zónu DNS](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="00fa2-112">toocreate a DNS zone, follow hello steps in [Create a DNS zone](dns-getstarted-create-dnszone.md).</span></span>
2. <span data-ttu-id="00fa2-113">toodelegate vaše DNS tooAzure DNS, postupujte podle kroků hello v [delegování DNS domény](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="00fa2-113">toodelegate your DNS tooAzure DNS, follow hello steps in [DNS domain delegation](dns-domain-delegation.md).</span></span>

<span data-ttu-id="00fa2-114">Po vytvoření zóny a delegování ho tooAzure DNS, potom můžete vytvořit záznamy pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="00fa2-114">After creating a zone and delegating it tooAzure DNS, you can then create records for your custom domain.</span></span>

## <a name="1-create-an-a-record-for-your-custom-domain"></a><span data-ttu-id="00fa2-115">1. Vytvoření záznamu A pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="00fa2-115">1. Create an A record for your custom domain</span></span>

<span data-ttu-id="00fa2-116">Záznam je použité toomap na název tooits IP adresu.</span><span class="sxs-lookup"><span data-stu-id="00fa2-116">An A record is used toomap a name tooits IP address.</span></span> <span data-ttu-id="00fa2-117">V následující ukázka hello jsme se přiřadit jako tooan záznam A adresu IPv4:</span><span class="sxs-lookup"><span data-stu-id="00fa2-117">In hello following example we will assign @ as an A record tooan IPv4 address:</span></span>

### <a name="step-1"></a><span data-ttu-id="00fa2-118">Krok 1</span><span class="sxs-lookup"><span data-stu-id="00fa2-118">Step 1</span></span>

<span data-ttu-id="00fa2-119">Vytvořte záznam a přiřaďte tooa proměnné $rs</span><span class="sxs-lookup"><span data-stu-id="00fa2-119">Create an A record and assign tooa variable $rs</span></span>

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a><span data-ttu-id="00fa2-120">Krok 2</span><span class="sxs-lookup"><span data-stu-id="00fa2-120">Step 2</span></span>

<span data-ttu-id="00fa2-121">Přidat hello IPv4 hodnota toohello dříve vytvořili sadu záznamů "@" použití hello $rs proměnné přiřazené.</span><span class="sxs-lookup"><span data-stu-id="00fa2-121">Add hello IPv4 value toohello previously created record set "@" using hello $rs variable assigned.</span></span> <span data-ttu-id="00fa2-122">Hello IPv4 přiřazené bude hello IP adresu pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00fa2-122">hello IPv4 value assigned will be hello IP address for your web app.</span></span>

<span data-ttu-id="00fa2-123">toofind hello IP adres pro webovou aplikaci, postupujte podle kroků hello v [konfigurace vlastního názvu domény v Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="00fa2-123">toofind hello IP address for a web app, follow hello steps in [Configure a custom domain name in Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a><span data-ttu-id="00fa2-124">Krok 3</span><span class="sxs-lookup"><span data-stu-id="00fa2-124">Step 3</span></span>

<span data-ttu-id="00fa2-125">Potvrďte hello změny toohello sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="00fa2-125">Commit hello changes toohello record set.</span></span> <span data-ttu-id="00fa2-126">Použití `Set-AzureRMDnsRecordSet` tooupload hello změny tooAzure toohello sada záznamů DNS:</span><span class="sxs-lookup"><span data-stu-id="00fa2-126">Use `Set-AzureRMDnsRecordSet` tooupload hello changes toohello record set tooAzure DNS:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="00fa2-127">2. Vytvořte záznam CNAME pro vaši vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="00fa2-127">2. Create a CNAME record for your custom domain</span></span>

<span data-ttu-id="00fa2-128">Pokud vaše doména je již spravován nástrojem Azure DNS (viz [delegování DNS domény](dns-domain-delegation.md), můžete použít následující příklad toocreate hello záznam CNAME pro contoso.azurewebsites.net hello.</span><span class="sxs-lookup"><span data-stu-id="00fa2-128">If your domain is already managed by Azure DNS (see [DNS domain delegation](dns-domain-delegation.md), you can use hello following hello example toocreate a CNAME record for contoso.azurewebsites.net.</span></span>

### <a name="step-1"></a><span data-ttu-id="00fa2-129">Krok 1</span><span class="sxs-lookup"><span data-stu-id="00fa2-129">Step 1</span></span>

<span data-ttu-id="00fa2-130">Otevřete prostředí PowerShell a vytvořte novou sadu záznamů CNAME a přiřaďte tooa proměnné $rs.</span><span class="sxs-lookup"><span data-stu-id="00fa2-130">Open PowerShell and create a new CNAME record set and assign tooa variable $rs.</span></span> <span data-ttu-id="00fa2-131">Tento příklad vytvoří sadu záznamů typu CNAME s "časem toolive" 600 sekund v zóně DNS s názvem "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="00fa2-131">This example will create a record set type CNAME with a "time toolive" of 600 seconds in DNS zone named "contoso.com".</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="00fa2-132">Následující ukázka Hello je hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="00fa2-132">hello following example is hello response.</span></span>

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

### <a name="step-2"></a><span data-ttu-id="00fa2-133">Krok 2</span><span class="sxs-lookup"><span data-stu-id="00fa2-133">Step 2</span></span>

<span data-ttu-id="00fa2-134">Po vytvoření sady záznamů CNAME hello musíte toocreate hodnotou alias, který bude odkazovat toohello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="00fa2-134">Once hello CNAME record set is created, you need toocreate an alias value which will point toohello web app.</span></span>

<span data-ttu-id="00fa2-135">Hello dřív přiřazené proměnná "$rs" pomocí příkazu prostředí PowerShell hello níže toocreate hello alias pro hello webové aplikace contoso.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="00fa2-135">Using hello previously assigned variable "$rs" you can use hello PowerShell command below toocreate hello alias for hello web app contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

<span data-ttu-id="00fa2-136">Následující ukázka Hello je hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="00fa2-136">hello following example is hello response.</span></span>

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

### <a name="step-3"></a><span data-ttu-id="00fa2-137">Krok 3</span><span class="sxs-lookup"><span data-stu-id="00fa2-137">Step 3</span></span>

<span data-ttu-id="00fa2-138">Potvrzení změn hello pomocí hello `Set-AzureRMDnsRecordSet` rutiny:</span><span class="sxs-lookup"><span data-stu-id="00fa2-138">Commit hello changes using hello `Set-AzureRMDnsRecordSet` cmdlet:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="00fa2-139">Můžete ověřit hello záznam vytvořil správně dotazováním hello "www.contoso.com" pomocí nástroje nslookup, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="00fa2-139">You can validate hello record was created correctly by querying hello "www.contoso.com" using nslookup, as shown below:</span></span>

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

## <a name="create-an-awverify-record-for-web-apps"></a><span data-ttu-id="00fa2-140">Vytvoření záznamů "awverify" pro webové aplikace</span><span class="sxs-lookup"><span data-stu-id="00fa2-140">Create an "awverify" record for web apps</span></span>

<span data-ttu-id="00fa2-141">Pokud se rozhodnete toouse záznam pro vaši webovou aplikaci, musíte přejít do tooensure ověření procesu můžete vlastní hello vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="00fa2-141">If you decide toouse an A record for your web app, you must go through a verification process tooensure you own hello custom domain.</span></span> <span data-ttu-id="00fa2-142">Tento krok ověření se provádí tak, že vytvoříte speciální záznam CNAME s názvem "awverify".</span><span class="sxs-lookup"><span data-stu-id="00fa2-142">This verification step is done by creating a special CNAME record named "awverify".</span></span> <span data-ttu-id="00fa2-143">Tato část se týká pouze tooA záznamy.</span><span class="sxs-lookup"><span data-stu-id="00fa2-143">This section applies tooA records only.</span></span>

### <a name="step-1"></a><span data-ttu-id="00fa2-144">Krok 1</span><span class="sxs-lookup"><span data-stu-id="00fa2-144">Step 1</span></span>

<span data-ttu-id="00fa2-145">Vytvořte záznam "awverify" hello.</span><span class="sxs-lookup"><span data-stu-id="00fa2-145">Create hello "awverify" record.</span></span> <span data-ttu-id="00fa2-146">V příkladu hello dole vytvoříme hello "aweverify" záznam pro contoso.com tooverify vlastnictví pro vlastní doménu, hello.</span><span class="sxs-lookup"><span data-stu-id="00fa2-146">In hello example below, we will create hello "aweverify" record for contoso.com tooverify ownership for hello custom domain.</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="00fa2-147">Následující ukázka Hello je hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="00fa2-147">hello following example is hello response.</span></span>

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

### <a name="step-2"></a><span data-ttu-id="00fa2-148">Krok 2</span><span class="sxs-lookup"><span data-stu-id="00fa2-148">Step 2</span></span>

<span data-ttu-id="00fa2-149">Po vytvoření sady záznamů hello "awverify" přiřadíte alias sadu záznamů CNAME hello.</span><span class="sxs-lookup"><span data-stu-id="00fa2-149">Once hello record set "awverify" is created, assign hello CNAME record set alias.</span></span> <span data-ttu-id="00fa2-150">V příkladu hello níže jsme přiřadí tooawverify.contoso.azurewebsites.net alias sady záznamů CNAMe hello.</span><span class="sxs-lookup"><span data-stu-id="00fa2-150">In hello example below, we will assign hello CNAMe record set alias tooawverify.contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

<span data-ttu-id="00fa2-151">Následující ukázka Hello je hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="00fa2-151">hello following example is hello response.</span></span>

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

### <a name="step-3"></a><span data-ttu-id="00fa2-152">Krok 3</span><span class="sxs-lookup"><span data-stu-id="00fa2-152">Step 3</span></span>

<span data-ttu-id="00fa2-153">Potvrzení změn hello pomocí hello `Set-AzureRMDnsRecordSet cmdlet`, jak ukazuje následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="00fa2-153">Commit hello changes using hello `Set-AzureRMDnsRecordSet cmdlet`, as shown in hello command below.</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a><span data-ttu-id="00fa2-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="00fa2-154">Next steps</span></span>

<span data-ttu-id="00fa2-155">Postupujte podle kroků hello v [konfigurace vlastního názvu domény pro službu App Service](../app-service-web/web-sites-custom-domain-name.md) tooconfigure vaší webové aplikace toouse vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="00fa2-155">Follow hello steps in [Configuring a custom domain name for App Service](../app-service-web/web-sites-custom-domain-name.md) tooconfigure your web app toouse a custom domain.</span></span>
