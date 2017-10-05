---
title: "Konfigurace vlastního názvu domény v cloudové služby | Microsoft Docs"
description: "Zjistěte, jak vystavit aplikaci Azure nebo data na vlastní domény tak, že nakonfigurujete nastavení DNS."
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
ms.openlocfilehash: 9f872fd5119042945356225a80331da18f3a6d99
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurace vlastního názvu domény pro cloudové služby Azure
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-custom-domain-name-portal.md)
> * [Portál Azure Classic](cloud-services-custom-domain-name.md)
> 
> 

Při vytváření cloudové služby Azure přiřadí ji k subdoména cloudapp.net. Například pokud cloudové služby má název "contoso", uživatelé budou mít přístup k aplikaci na adresu URL podobnou http://contoso.cloudapp.net. Azure také přiřadí virtuální IP adresu.

Můžete však také vystavit vaší aplikace na svůj vlastní název domény, například contoso.com. Tento článek vysvětluje, jak chcete rezervovat nebo konfigurovat vlastní název domény pro cloudové služby webové role.

Již rozumíte co CNAME záznamy a A jsou? [Přechod za vysvětlení](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> Získání budete rychlejší! Použití Azure [návod na základě](http://support.microsoft.com/kb/2990804). Umožňuje přiřazení vlastního názvu domény a zabezpečení komunikace (SSL) s Azure Cloud Services nebo weby Azure moduly snap.
> 
> 

<p/>

> [!NOTE]
> Postupy v této úloze platí pro Azure Cloud Services. Aplikační služby, najdete v části [to](../app-service-web/web-sites-custom-domain-name.md). Účty úložiště, najdete v části [to](../storage/blobs/storage-custom-domain-name.md).
> 
> 

## <a name="understand-cname-and-a-records"></a>Pochopení záznamy CNAME a A
CNAME (nebo alias záznamů) a záznamy oba umožňují přidružit název domény pro konkrétní server (nebo v tomto případě služby) ale fungují jinak. Existují zde také některé specifické aspekty při použití záznamy A službou Azure Cloud services, které byste měli zvážit, než se rozhodnete, který se použije.

### <a name="cname-or-alias-record"></a>Záznam CNAME nebo Alias
Mapuje záznam CNAME *konkrétní* domény, například **contoso.com** nebo **www.contoso.com**, na název domény kanonickém tvaru. V takovém případě je název domény kanonický **[myapp] .cloudapp .net** název domény vaší Azure hostované aplikace. Po vytvoření CNAME tento alias **[myapp] .cloudapp .net**. Záznam CNAME se přeložit na IP adresu vašeho **[myapp] .cloudapp .net** služby automaticky, takže pokud se změní IP adresu cloudové služby, není nutné provádět žádnou akci.

> [!NOTE]
> Některé domény registrátorů Povolit jenom vám umožní mapovat subdomény při použití záznam CNAME, třeba www.contoso.com a není kořenové názvy, například contoso.com. Další informace o záznamy CNAME, naleznete v dokumentaci vaším registrátorem [položku Wikipedia na záznam CNAME](http://en.wikipedia.org/wiki/CNAME_record), nebo [IETF názvy domén – implementace a specifikace](http://tools.ietf.org/html/rfc1035) dokumentu.
> 
> 

### <a name="a-record"></a>Záznam
Záznam A mapuje domény, jako například **contoso.com** nebo **www.contoso.com**, *nebo zástupné domény* například  **\*. contoso.com**, IP adresu. V případě služby Azure, cloudové virtuální IP adresy služby. Hlavní výhodou záznam přes záznam CNAME je, že můžete mít jednu položku, která používá zástupný znak, jako například \* **. contoso.com**, který by například zpracovávat požadavky pro více subdomén  **mail.contoso.com**, **login.contoso.com**, nebo **www.contso.com**.

> [!NOTE]
> Vzhledem k tomu, že záznam A je namapovaná na statickou IP adresu, nedokáže automaticky vyřešit změny na IP adresu cloudové služby. IP adresu používanou službou cloudové služby je přidělen poprvé, kterého nasadíte do prázdné přihrádky (produkční nebo pracovní.) Pokud odstraníte nasazení pro přihrádky, IP adresa se neuvolní Azure a všechna budoucí nasazení do přihrádky udělit novou IP adresu.
> 
> Pohodlně IP adresu z daného nasazovací slot (produkční nebo pracovní) je nastavené jako trvalé při odkládací mezi pracovním a nasazení v produkčním prostředí nebo provedení místního upgradu existujícího nasazení. Další informace o provedení těchto akcí naleznete v tématu [Správa cloudových služeb](cloud-services-how-to-manage.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Přidejte záznam CNAME pro vaši vlastní doménu.
Pokud chcete vytvořit záznam CNAME, musí přidáte nový záznam v tabulce DNS pro vaši vlastní doménu pomocí nástroje poskytované subsystémem registrátora. Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam CNAME, ale koncepty jsou stejné.

1. Použijte jednu z těchto metod najít **. cloudapp.net** název domény, které jsou přiřazené ke cloudové službě.
   
   * Přihlášení k [portál Azure classic]vyberte cloudové služby, vyberte **řídicí panel**a pak vyhledejte **adresa URL webu** položku v **rychlého přehledu**části.
     
       ![část rychlého přehledu zobrazující adresa URL webu][csurl]
     
       **OR**  
   * Instalace a konfigurace [prostředí Azure Powershell](/powershell/azure/overview)a potom použijte následující příkaz:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Uložte název domény používaný v adrese URL vrácené metody, jak budete ho potřebovat při vytváření záznamu CNAME.
2. Přihlaste se k webu registrátora DNS a přejděte na stránku pro správu DNS. Vyhledejte odkazy nebo oblasti lokality označený jako **název domény**, **DNS**, nebo **název serveru správy**.
3. Nyní kde můžete vyhledejte vyberte nebo zadejte na CNAME. Možná budete muset vybrat typ záznamu rozevíracím dolů, nebo přejděte na stránku Upřesnit nastavení. Je vhodné vyhledat slova **CNAME**, **Alias**, nebo **subdomény**.
4. Je rovněž nutné poskytnout doménu nebo subdomény alias CNAME, jako například **www** Pokud chcete vytvořit alias **www.customdomain.com**. Pokud chcete vytvořit alias pro kořenovou doménu, nemusí být zobrazeny jako '**@**' symbol v nástrojích DNS vašeho registrátora.
5. Potom je nutné zadat název kanonický hostitele, který je vaše aplikace **cloudapp.net** domény v tomto případě.

Například následující záznam CNAME předává veškerý provoz z **www.contoso.com** k **contoso.cloudapp.net**, vlastní název domény nasazené aplikace:

| Název aliasu/hostitele nebo subdomény | Kanonický domény |
| --- | --- |
| www |contoso.cloudapp.NET |

Návštěvník z **www.contoso.com** nikdy zobrazit true hostitele (contoso.cloudapp.net), tak, aby procesu předávání neviditelná pro koncového uživatele.

> [!NOTE]
> V předchozím příkladu platí pouze pro provoz na **www** subdomény. Vzhledem k tomu, že se záznamy CNAME nelze použít zástupné znaky, je třeba vytvořit jednu CNAME pro každou doménu subdomény. Pokud chcete směrovat přenosy z subdomény, jako například \*. contoso.com, na vaši adresu cloudapp.net můžete nakonfigurovat **adresa URL přesměrování** nebo **dál adresy URL** položku v nastavení DNS, nebo vytvořit záznam.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Přidání záznamu A pro vaši vlastní doménu.
Pokud chcete vytvořit záznam A, je nutné nejprve vyhledat virtuální IP adresy cloudové služby. Potom přidejte nový záznam v tabulce DNS pro vaši vlastní doménu pomocí nástroje poskytované subsystémem registrátora. Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam A, ale koncepty jsou stejné.

1. Použijte jednu z následujících metod k získání IP adresy cloudové služby.
   
   * přihlášení k [portál Azure classic]vyberte cloudové služby, vyberte **řídicí panel**a pak vyhledejte **veřejná virtuální IP (VIP) Adresa** položku v  **rychlého přehledu** části.
     
       ![Zobrazuje virtuální IP adresu části rychlého přehledu.][vip]
     
       **OR**  
   * Instalace a konfigurace [prostředí Azure Powershell](/powershell/azure/overview)a potom použijte následující příkaz:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Pokud máte několik koncových bodů, které jsou spojené s cloudovou službou, zobrazí se více řádků, který obsahuje IP adresu, ale všechny by měl zobrazit stejnou adresu.
     
     IP adresa, uložte, protože jej budete potřebovat při vytváření záznam.
2. Přihlaste se k webu registrátora DNS a přejděte na stránku pro správu DNS. Vyhledejte odkazy nebo oblasti lokality označený jako **název domény**, **DNS**, nebo **název serveru správy**.
3. Nyní najdete, kde můžete vyberte nebo zadejte záznamu. Možná budete muset vybrat typ záznamu rozevíracím dolů, nebo přejděte na stránku Upřesnit nastavení.
4. Vyberte nebo zadejte doménu nebo subdomény, které budou používat tento záznam A. Vyberte například **www** Pokud chcete vytvořit alias **www.customdomain.com**. Pokud chcete vytvořit záznam zástupných znaků pro všechny subdomény, zadejte "__*__'. Ten přináší všem dílčím doménám domény, jako **mail.customdomain.com**, **login.customdomain.com**, a **www.customdomain.com**.
   
    Pokud chcete vytvořit záznam pro kořenovou doménu, nemusí být zobrazeny jako '**@**' symbol v nástrojích DNS vašeho registrátora.
5. Zadejte IP adresu cloudové služby v zadané pole. To přiřadí položku domény použít v záznam A s IP adresou nasazení cloudové služby.

Například následující záznam předává veškerý provoz z **contoso.com** k **137.135.70.239**, IP adresu nasazené aplikace:

| Název hostitele nebo subdomény | IP adresa |
| --- | --- |
| @ |137.135.70.239 |

Tento příklad ukazuje vytvoření záznamu A pro kořenovou doménu. Pokud chcete vytvořit záznam zástupný znak nepokrývají všechny subdomény, zadejte "__*__' jako subdoméně.

> [!WARNING]
> IP adresy v Azure jsou ve výchozím nastavení dynamické. Budete pravděpodobně chtít použít [vyhrazená IP adresa](../virtual-network/virtual-networks-reserved-public-ip.md) zajistit, že IP adresa se nemění.
> 
> 

## <a name="next-steps"></a>Další kroky
* [Jak spravovat Cloud Services](cloud-services-how-to-manage.md)
* [Postup mapování obsahu CDN do vlastní domény](../cdn/cdn-map-content-to-custom-domain.md)
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[portál Azure classic]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
