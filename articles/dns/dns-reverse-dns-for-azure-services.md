---
title: "aaaReverse DNS pro služby Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure zpětné vyhledávání DNS pro služby hostované v Azure"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: c6fe1d80232f124be86dd7fc57abc20699be7eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Konfigurace zpětné DNS pro služby hostované v Azure

Tento článek vysvětluje, jak tooconfigure zpětné vyhledávání DNS pro služby hostované v Azure.

Služby v Azure pomocí IP adresy přiřazené službou Azure a vlastnictví společnosti Microsoft. Tyto zpětné záznamy DNS (záznam PTR) musí být vytvořen v hello odpovídající ve vlastnictví společnosti Microsoft zpětné vyhledávání zóny DNS. Tento článek vysvětluje, jak toodo to.

Tento scénář by se neměly zaměňovat s hello možnost příliš[hostitele hello zpětné vyhledávání zóny DNS pro vaše přiřazené rozsahy IP v Azure DNS](dns-reverse-dns-hosting.md). V takovém případě rozsahy IP hello reprezentována zóny zpětného vyhledávání hello musí být přiřazen tooyour organizace, obvykle podle vašeho poskytovatele internetových služeb.

Před přečtení tohoto článku, měli byste se seznámit s tím [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).

Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).
* V modelu nasazení Resource Manager hello výpočetní prostředky (třeba virtuální počítače sady škálování virtuálního počítače nebo clusterů Service Fabric) jsou zveřejňovány prostřednictvím PublicIpAddress prostředků. Zpětné vyhledávání DNS se konfiguruje pomocí vlastnosti 'ReverseFqdn' hello hello PublicIpAddress.
* V modelu nasazení Classic hello se zveřejňují výpočetní prostředky, cloudové služby. Zpětné vyhledávání DNS se konfiguruje pomocí vlastnosti 'ReverseDnsFqdn' hello hello cloudové služby.

Zpětné DNS není aktuálně podporován pro hello Azure App Service.

## <a name="validation-of-reverse-dns-records"></a>Ověření záznamy zpětného vyhledávání DNS

Třetí strany by neměl být schopný toocreate zpětné záznamy DNS pro jejich službu Azure mapování tooyour DNS domény. tooprevent, Azure umožňuje pouze hello vytvoření zpětné záznam DNS, kde je název domény určený v záznamu DNS zpětného hello hello stejný jako, nebo překládá, hello název DNS nebo IP adresu PublicIpAddress nebo cloudové služby v hello stejné předplatné Azure.

Toto ověření se provádí pouze při nastavit nebo změnit hello zpětné záznamu DNS. Pravidelně opakované ověření není provedena.

Příklad: Předpokládejme, že hello PublicIpAddress prostředků má contosoapp1.northus.cloudapp.azure.com název hello DNS a IP adresu 23.96.52.53. ReverseFqdn Hello hello PublicIpAddress lze zadat jako:
* název DNS Hello hello PublicIpAddress, contosoapp1.northus.cloudapp.azure.com
* Hello na názvy DNS pro různé PublicIpAddress v hello stejné předplatné, jako například contosoapp2.westus.cloudapp.azure.com
* Jednoduché DNS název, jako je například app1.contoso.com, tak dlouho, dokud tento název se *první* nakonfigurované jako CNAME toocontosoapp1.northus.cloudapp.azure.com nebo tooa různých PublicIpAddress v hello stejného předplatného.
* Jednoduché DNS název, jako je například app1.contoso.com, tak dlouho, dokud tento název se *první* nakonfigurovaný jako IP adresu A záznamů toohello 23.96.52.53 nebo toohello IP adresu na jinou PublicIpAddress v hello stejného předplatného.

Hello stejné omezení platí tooreverse DNS pro cloudové služby.


## <a name="reverse-dns-for-publicipaddress-resources"></a>Reverse DNS pro prostředky PublicIpAddress

Tato část obsahuje podrobné pokyny, jak tooconfigure reverse DNS pro prostředky PublicIpAddress v modelu nasazení Resource Manager hello, pomocí prostředí Azure PowerShell, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure. Konfigurace zpětné DNS pro prostředky PublicIpAddress není aktuálně podporován prostřednictvím hello portálu Azure.

Azure aktuálně podporuje reverse DNS pouze pro prostředky IPv4 PublicIpAddress. Není podporováno pro protokol IPv6.

### <a name="add-reverse-dns-tooan-existing-publicipaddresses"></a>Přidat existující publicipaddresses, na které zpětné DNS tooan

#### <a name="powershell"></a>PowerShell

tooadd reverse existující PublicIpAddress tooan DNS:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

tooadd zpětně DNS tooan existující PublicIpAddress, který ještě nemá název DNS, musíte taky zadat název DNS:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

tooadd reverse existující PublicIpAddress tooan DNS:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

tooadd zpětně DNS tooan existující PublicIpAddress, který ještě nemá název DNS, musíte taky zadat název DNS:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

tooadd reverse existující PublicIpAddress tooan DNS:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

tooadd zpětně DNS tooan existující PublicIpAddress, který ještě nemá název DNS, musíte taky zadat název DNS:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Vytvoření veřejné IP adresy s zpětné DNS

toocreate nové PublicIpAddress s hello zpětné již zadanou vlastnost DNS:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Zobrazení zpětného DNS pro existující PublicIpAddress

pro existující PublicIpAddress tooview hello nakonfigurovat hodnotu:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Odeberte zpětné DNS z existující veřejné IP adresy

tooremove zpětné DNS vlastnosti z existující PublicIpAddress:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Konfigurace zpětné DNS pro cloudové služby

Tato část obsahuje podrobné pokyny, jak tooconfigure reverse DNS pro cloudové služby v modelu nasazení Classic hello, pomocí Azure PowerShell. Konfigurace zpětné DNS pro cloudové služby není podporována prostřednictvím hello portál Azure, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure.

### <a name="add-reverse-dns-tooexisting-cloud-services"></a>Přidat zpětné DNS tooexisting cloudové služby

tooadd tooan zpětné záznamů DNS existující cloudové služby:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Vytvoření cloudové služby se zpětné DNS

toocreate novou Cloudovou službu se hello zpětné již zadanou vlastností DNS:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Zobrazení zpětného DNS pro existující cloudové služby

tooview hello reverse vlastnosti DNS pro stávající Cloudovou službu:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Odeberte zpětné DNS z existující cloudové služby

tooremove zpětné vlastnosti DNS z existující cloudové služby:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>Nejčastější dotazy

### <a name="how-much-do-reverse-dns-records-cost"></a>Kolik reverse náklady na záznamy DNS?

Jsou zdarma!  Není k dispozici bez dalších nákladů pro zpětné záznamy DNS nebo dotazy.

### <a name="will-my-reverse-dns-records-resolve-from-hello-internet"></a>Bude Moje zpětné vyřešte záznamy DNS z hello Internetu?

Ano. Jakmile jednou nastavíte vlastnost DNS zpětného hello služby Azure, Azure spravuje všechny hello delegování DNS a zón DNS požadované přeloží tooensure, který reverse záznam DNS pro všechny uživatele na Internetu.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Vytvářejí výchozí záznamy zpětného vyhledávání DNS pro Moje služby Azure?

Ne. Zpětné DNS je funkce přihlášení. Zpětné záznamy DNS jsou vytvořen, pokud si zvolíte tooconfigure není žádné výchozí je.

### <a name="what-is-hello-format-for-hello-fully-qualified-domain-name-fqdn"></a>Co je hello formát pro hello plně kvalifikovaný název domény (FQDN)?

Plně kvalifikované názvy domény jsou zadány popořadě a musí být ukončen tečkou (například "app1.contoso.com.").

### <a name="what-happens-if-hello-validation-check-for-hello-reverse-dns-ive-specified-fails"></a>Co se stane, pokud ověření zkontrolujte hello hello reverse DNS I jste určili nezdaří?

Kde hello zpětné DNS kontrolu ověření selže, selže hello operaci tooconfigure hello zpětné záznam DNS. Správné hello zpětné hodnotu DNS podle potřeby a zkuste to znovu.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Můžete nakonfigurovat zpětné DNS pro službu Azure App Service?

Ne. Zpětné DNS není podporována pro hello Azure App Service.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Můžete nakonfigurovat více zpětné záznamy DNS pro Moje služba Azure?

Ne. Azure podporuje jeden zpětné záznam DNS pro každé cloudové služby Azure nebo PublicIpAddress.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>Můžete nakonfigurovat zpětné DNS pro prostředky IPv6 PublicIpAddress?

Ne. Azure aktuálně podporuje reverse DNS pouze pro prostředky IPv4 PublicIpAddress a cloudové služby.

### <a name="can-i-send-emails-tooexternal-domains-from-my-azure-compute-services"></a>Můžete odesílat e-mailů tooexternal domén z mé Azure výpočetní služby?

Ne. [Azure výpočetní služby nepodporují odesílání e-mailů tooexternal domén](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a>Další kroky

Další informace o zpětné DNS najdete v tématu [zpětného vyhledávání DNS na webu Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Zjistěte, jak příliš[zóny zpětného vyhledávání hello hostitele pro rozsah vašeho poskytovatele internetových služeb přiřadit IP v Azure DNS](dns-reverse-dns-for-azure-services.md).

