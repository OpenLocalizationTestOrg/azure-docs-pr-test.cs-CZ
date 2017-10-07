---
title: "aaaConfigure vlastního názvu domény v cloudové služby | Microsoft Docs"
description: "Zjistěte, jak tooexpose Azure aplikace nebo data toohello internet na vlastní domény tak, že nakonfigurujete nastavení DNS.  Tyto příklady použití hello portálu Azure."
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
ms.openlocfilehash: a0f3186b6022fbc4570ef1ce4b921426842bde76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurace vlastního názvu domény pro cloudové služby Azure
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-custom-domain-name-portal.md)
> * [Portál Azure Classic](cloud-services-custom-domain-name.md)
> 
> 

Při vytváření cloudové služby Azure přiřadí, je subdoménou tooa **cloudapp.net**. Například pokud cloudové služby má název "contoso", uživatelé budou se moct tooaccess vaší aplikace na adresu URL podobnou http://contoso.cloudapp.net. Azure také přiřadí virtuální IP adresu.

Ale můžete také vystavit vaší aplikace na svůj vlastní název domény, jako například **contoso.com**. Tento článek vysvětluje, jak tooreserve nebo nakonfigurovat vlastní název domény pro cloudové služby webové role.

Již rozumíte co CNAME záznamy a A jsou? [Přechod za hello vysvětlení](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> Hello postupy v této úloze platí tooAzure cloudové služby. Aplikační služby, najdete v části [to](../app-service-web/web-sites-custom-domain-name.md). Účty úložiště, najdete v části [to](../storage/blobs/storage-custom-domain-name.md).
> 
> 

<p/>

> [!TIP]
> Zprovoznění rychlejší – použijte hello nové Azure [návod na základě](http://support.microsoft.com/kb/2990804)!  S Azure Cloud Services nebo weby Azure moduly snap umožňuje přiřazení vlastního názvu domény a zabezpečení komunikace (SSL).
> 
> 

## <a name="understand-cname-and-a-records"></a>Pochopení záznamy CNAME a A
CNAME (nebo alias záznamů) a záznamy A umožňují tooassociate názvu domény s konkrétní server (i v tomto případě služby) ale fungují jinak. Existují zde také některé specifické aspekty při použití záznamy A službou Azure Cloud services, které byste měli zvážit, než se rozhodnete, které toouse.

### <a name="cname-or-alias-record"></a>Záznam CNAME nebo Alias
Mapuje záznam CNAME *konkrétní* domény, například **contoso.com** nebo **www.contoso.com**, název tooa kanonický domény. V takovém případě je název kanonický domény hello hello **[myapp] .cloudapp .net** název domény vaší Azure hostované aplikace. Po vytvoření hello CNAME tento alias hello **[myapp] .cloudapp .net**. Hello záznam CNAME se přeložit IP adresu toohello vaše **[myapp] .cloudapp .net** služby automaticky, takže pokud se změní IP adresu hello hello cloudové služby, není nutné tootake žádnou akci.

> [!NOTE]
> Některé domény registrátorů umožňují pouze toomap subdomény při použití záznam CNAME, třeba www.contoso.com a není kořenové názvy, například contoso.com. Další informace o záznamy CNAME, naleznete v dokumentaci k hello poskytované vašeho registrátora [hello Wikipedia položku na záznam CNAME](http://en.wikipedia.org/wiki/CNAME_record), nebo hello [IETF názvy domén – implementace a specifikace](http://tools.ietf.org/html/rfc1035) dokument.
> 
> 

### <a name="a-record"></a>Záznam
*A* záznam mapuje domény, jako například **contoso.com** nebo **www.contoso.com**, *nebo zástupné domény* například  **\*. contoso.com**, tooan IP adresu. V případě hello cloudové služby Azure hello virtuální IP adresy služby hello. Takže hello hlavní výhodou záznam přes záznam CNAME je, že můžete mít jednu položku, která používá zástupný znak, jako například \* **. contoso.com**, který by například zpracovávat požadavky pro více subdomén  **mail.contoso.com**, **login.contoso.com**, nebo **www.contso.com**.

> [!NOTE]
> Vzhledem k tomu, že je namapovaný záznam tooa statickou IP adresu nelze vyřešit automaticky změny toohello IP adresu cloudové služby. Hello IP adresu používanou službou cloudové služby je přidělen hello prvním nasazení tooan prázdné přihrádky (produkční nebo pracovní.) Pokud odstraníte nasazení hello pro hello slot, hello IP adresa se neuvolní Azure a slotu toohello všechny budoucí nasazení může být poskytnut novou IP adresu.
> 
> Pohodlně hello IP adresu z daného nasazovací slot (produkční nebo pracovní) je nastavené jako trvalé při odkládací mezi pracovním a nasazení v produkčním prostředí nebo provedení místního upgradu existujícího nasazení. Další informace o provedení těchto akcí naleznete v tématu [jak toomanage cloudových služeb](cloud-services-how-to-manage.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Přidejte záznam CNAME pro vaši vlastní doménu.
toocreate záznam CNAME, je musí přidat nový záznam v tabulce hello DNS pro vaši vlastní doménu s použitím hello nástroje poskytované subsystémem registrátora. Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam CNAME, ale hello hello koncepty jsou stejné.

1. Použijte jednu z těchto metod toofind hello **. cloudapp.net** název domény, které jsou přiřazené tooyour cloudové služby.
   
   * Přihlášení toohello [portál Azure], vyberte cloudovou službu, podívejte se na hello **Essentials** části a pak vyhledejte hello **adresa URL webu** položku.
     
       ![Zobrazuje adresu URL webu hello části rychlého přehledu.][csurl]
     
       **OR**
   * Instalace a konfigurace [prostředí Azure Powershell](/powershell/azure/overview), a pak hello použijte následující příkaz:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Uložte hello název domény používaný v URL ADRESE hello vráceném buď metodou, jak budete ho potřebovat při vytváření záznamu CNAME.
2. Přihlaste se na webu registrátora DNS tooyour a přejděte toohello stránku pro správu DNS. Vyhledejte odkazy nebo oblasti lokality hello označený jako **název domény**, **DNS**, nebo **název serveru správy**.
3. Nyní kde můžete vyhledejte vyberte nebo zadejte na CNAME. Můžete mít tooselect hello záznam typu z rozevíracího seznamu, nebo přejděte tooan Upřesnit nastavení stránky. Je vhodné vyhledat hello slova **CNAME**, **Alias**, nebo **subdomény**.
4. Je rovněž nutné poskytnout hello domény nebo subdomény alias pro hello CNAME, jako například **www** Pokud chcete toocreate alias **www.customdomain.com**. Pokud chcete toocreate alias pro hello kořenovou doménu, nemusí být zobrazeny jako hello '**@**' symbol v nástrojích DNS vašeho registrátora.
5. Potom je nutné zadat název kanonický hostitele, který je vaše aplikace **cloudapp.net** domény v tomto případě.

Například následující záznam CNAME hello předává veškerý provoz z **www.contoso.com** příliš**contoso.cloudapp.net**, hello vlastní název domény nasazené aplikace:

| Název aliasu/hostitele nebo subdomény | Kanonický domény |
| --- | --- |
| www |contoso.cloudapp.NET |

> [!NOTE]
> Návštěvník z **www.contoso.com** nikdy zobrazit hello true hostitele (contoso.cloudapp.net), tak, aby hello předávání proces neviditelná toothe koncového uživatele.
> 
> Hello výše uvedeném příkladu vztahuje pouze na hello tootraffic **www** subdomény. Vzhledem k tomu, že se záznamy CNAME nelze použít zástupné znaky, je třeba vytvořit jednu CNAME pro každou doménu subdomény. Pokud například chcete toodirect provoz z subdomény, *. contoso.com, adresa cloudapp.net tooyour, můžete nakonfigurovat **adresa URL přesměrování** nebo **dál adresy URL** položku v nastavení DNS, nebo vytvořte záznam.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Přidání záznamu A pro vaši vlastní doménu.
toocreate A záznam, je nutné nejprve vyhledat hello virtuální IP adresy cloudové služby. Potom přidejte nový záznam v tabulce hello DNS pro vaši vlastní doménu pomocí hello nástroje poskytované subsystémem registrátora. Každý Registrátor má podobné, ale poněkud liší metodu zadávání záznam A, ale hello hello koncepty jsou stejné.

1. Použijte jeden z hello následující metody tooget hello IP adresu cloudové služby.
   
   * Přihlášení toohello [portál Azure], vyberte cloudovou službu, podívejte se na hello **Essentials** části a pak vyhledejte hello **veřejné IP adresy** položku.
     
       ![Zobrazuje hello VIP části rychlého přehledu.][vip]
     
       **OR**
   * Instalace a konfigurace [prostředí Azure Powershell](/powershell/azure/overview), a pak hello použijte následující příkaz:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Hello IP adresu, uložte, protože jej budete potřebovat při vytváření záznam.
2. Přihlaste se na webu registrátora DNS tooyour a přejděte toohello stránku pro správu DNS. Vyhledejte odkazy nebo oblasti lokality hello označený jako **název domény**, **DNS**, nebo **název serveru správy**.
3. Nyní najdete, kde můžete vyberte nebo zadejte záznamu. Můžete mít tooselect hello záznam typu z rozevíracího seznamu, nebo přejděte tooan Upřesnit nastavení stránky.
4. Vyberte nebo zadejte doménu hello nebo subdomény, které budou používat tento záznam A. Vyberte například **www** Pokud chcete toocreate alias **www.customdomain.com**. Pokud chcete toocreate položku zástupných znaků pro všechny subdomény, zadejte "***'. Ten přináší všem dílčím doménám domény, jako **mail.customdomain.com**, **login.customdomain.com**, a **www.customdomain.com**.
   
    Pokud chcete toocreate A záznam pro hello kořenovou doménu, nemusí být zobrazeny jako hello '**@**' symbol v nástrojích DNS vašeho registrátora.
5. Zadejte IP adresu hello cloudové služby v hello zadaná pole. To přiřadí zadaná doména hello používá v hello záznam A s IP adresou hello nasazení cloudové služby.

Například následující záznam hello předává veškerý provoz z **contoso.com** příliš**137.135.70.239**, hello IP adresu nasazené aplikace:

| Název hostitele nebo subdomény | IP adresa |
| --- | --- |
| @ |137.135.70.239 |

Tento příklad ukazuje vytvoření záznamu A pro hello kořenové domény. Pokud chcete toocreate zástupný znak položka toocover všechny subdomény, zadali byste ' ***' jako hello subdomény.

> [!WARNING]
> IP adresy v Azure jsou ve výchozím nastavení dynamické. Budete asi chtít toouse [vyhrazená IP adresa](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure, který se nemění IP adresu.
> 
> 

## <a name="next-steps"></a>Další kroky
* [Jak tooManage cloudových služeb](cloud-services-how-to-manage.md)
* [Jak tooMap obsahu CDN tooa vlastní domény](../cdn/cdn-map-content-to-custom-domain.md)
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).
* Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates hello subdomain with hello storage account]: #create-cname
[portál Azure]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
