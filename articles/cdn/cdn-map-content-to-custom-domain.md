---
title: "vlastní domény obsahu tooa aaaMap Azure CDN | Microsoft Docs"
description: "Zjistěte, jak toomap Azure CDN obsahu tooa vlastní doménu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d3ee77297f1dd7dbf31a9391191cc2910fbd2cee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="map-azure-cdn-content-tooa-custom-domain"></a>Namapovat vlastní doménu Azure CDN obsahu tooa
Můžete namapovat vlastní domény tooa koncový bod CDN v pořadí toouse svůj vlastní název domény v toocached adresy URL obsahu místo pomocí subdoména azureedge.net.

Existují dva způsoby toomap koncový bod CDN tooa vlastní domény:

1. [Vytvořte záznam CNAME u vašeho registrátora domény a mapování vlastních domén a subdomén toohello koncový bod CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint).
   
    Záznam CNAME je funkce DNS, která mapuje zdrojové domény, jako je třeba `www.contosocdn.com` nebo `cdn.contoso.com`, tooa cílové domény. V takovém případě je hello zdrojovou doménu vlastních domén a subdomén (subdoména, například **www** nebo **cdn** vždy je nutné). Cílová doména Hello je koncový bod CDN.  
   
    Hello proces mapování koncový bod CDN tooyour vlastní domény může, ale způsobit na krátkou dobu výpadku pro doménu hello registrace hello domény v hello portálu Azure.
2. [Přidat na krok zprostředkující registrace s **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)
   
    Pokud vaše vlastní doména je aktuálně podporuje aplikace s smlouvu úrovně služeb (SLA), který vyžaduje, aby existuje bez výpadku, pak můžete použít hello Azure **cdnverify** tooprovide subdomény zprostředkující registrace krok tak, aby uživatelé budou moct tooaccess doménu při hello DNS mapování proběhne.  

Po dokončení registrace vaši vlastní doménu pomocí jedné z hello výše postupy, budete chtít příliš[ověřte, že vlastní subdomény hello odkazuje na koncový bod CDN](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [!NOTE]
> Vytvořte záznam CNAME s vaší toomap registrátora domény koncový bod CDN toohello domény. Záznamy CNAME mapování specifické subdomény, jako `www.contoso.com` nebo `cdn.contoso.com`. Není možné toomap kořenovou doménu tooa záznamů CNAME, jako například `contoso.com`.
> 
> Subdoména lze přidružit pouze jeden koncový bod CDN. Hello záznam CNAME, který vytvoříte bude směrovat veškerý provoz řešit toohello subdomény toohello zadaný koncový bod.  Například Pokud přidružíte `www.contoso.com` s koncový bod CDN, pak nelze přidružíte k jiné Azure koncových bodů, jako je koncový bod účtu úložiště nebo koncového bodu služby cloud. Můžete však použít jiný subdomény z hello stejné domény pro koncové body jinou službu. Můžete také mapovat různé subdomény toohello stejný koncový bod CDN.
> 
> Pro **Azure CDN společnosti Verizon** (Standard a Premium) koncových bodů, Všimněte si, že zabírají příliš**90 minut** u vlastní domény změní toopropagate tooCDN edge uzly.
> 
> 

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Zaregistrovat vlastní doménu pro koncový bod Azure CDN
1. Přihlaste se k hello [portálu Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **Procházet**, pak **profilů CDN**, pak hello profil CDN s koncovým bodem hello chcete toomap tooa vlastní doménu.  
3. V hello **profil CDN** okně klikněte na koncový bod CDN hello se kterým se má tooassociate hello subdomény.
4. Hello horní části okna hello koncový bod, klikněte na tlačítko hello **přidat vlastní doménu** tlačítko.  V hello **přidat vlastní doménu** okně uvidíte hello názvu hostitele koncového bodu, odvozené od koncový bod CDN, toouse při vytváření nového záznamu CNAME. Hello formát adresy název hostitele hello se zobrazí jako  **&lt;EndpointName >. azureedge.net**.  Vytvořit záznam CNAME hello můžete zkopírovat tato toouse název hostitele.  
5. Navigace webu tooyour doménového registrátora a vyhledejte oddíl hello pro vytvoření záznamů DNS. Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.
6. Najít oddíl hello správy záznamů CNAME. Může mít toogo tooan Upřesnit nastavení stránky a vyhledejte slova hello CNAME, Alias nebo subdomény.
7. Vytvořit nový záznam CNAME, který mapuje vaši zvolenou subdomény (například **www** nebo **cdn**) název hostitele toohello součástí hello **přidat vlastní doménu** okno. 
8. Vrátí toohello **přidat vlastní doménu** okně a zadejte vlastní domény, včetně hello subdomény, v dialogovém okně hello. Například zadejte název domény hello ve formátu hello `www.contoso.com` nebo `cdn.contoso.com`.   
   
   Azure bude ověřte, zda existuje záznam CNAME hello pro hello název domény, který jste zadali. Pokud hello CNAME je správná, je ověřit vaši vlastní doménu.  Pro **Azure CDN společnosti Verizon** koncových bodů (Standard a Premium), může trvat až minut too90 pro vlastní doménu nastavení toopropagate tooall CDN edge uzly, ale.  
   
   Všimněte si, že v některých případech se může trvat dobu pro hello CNAME záznamů toopropagate tooname servery na hello Internetu. Pokud vaše doména není ověřený okamžitě, a budete mít dojem, že je správný hello záznam CNAME, počkejte několik minut a zkuste to znovu.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-hello-intermediary-cdnverify-subdomain"></a>Zaregistrovat vlastní doménu pro koncový bod Azure CDN pomocí subdomény zprostředkující cdnverify hello
1. Přihlaste se k hello [portálu Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **Procházet**, pak **profilů CDN**, pak hello profil CDN s koncovým bodem hello chcete toomap tooa vlastní doménu.  
3. V hello **profil CDN** okně klikněte na koncový bod CDN hello se kterým se má tooassociate hello subdomény.
4. Hello horní části okna hello koncový bod, klikněte na tlačítko hello **přidat vlastní doménu** tlačítko.  V hello **přidat vlastní doménu** okně uvidíte hello názvu hostitele koncového bodu, odvozené od koncový bod CDN, toouse při vytváření nového záznamu CNAME. Hello formát adresy název hostitele hello se zobrazí jako  **&lt;EndpointName >. azureedge.net**.  Vytvořit záznam CNAME hello můžete zkopírovat tato toouse název hostitele.
5. Navigace webu tooyour doménového registrátora a vyhledejte oddíl hello pro vytvoření záznamů DNS. Může to být v části, jako je **Název domény**, **DNS** nebo **Správa názvového serveru**.
6. Najít oddíl hello správy záznamů CNAME. Můžete mít toogo tooan upřesňující nastavení stránku a vyhledejte slova hello **CNAME**, **Alias**, nebo **subdomény**.
7. Vytvořit nový záznam CNAME a zadejte alias subdomény, která zahrnuje hello **cdnverify** subdomény. Například hello subdomény, který zadáte, bude ve formátu hello **cdnverify.www** nebo **cdnverify.cdn**. Potom zadejte název hostitele hello, což je koncový bod CDN, ve formátu hello **cdnverify.&lt; EndpointName >. azureedge.net**. Vaše mapování DNS by měl vypadat podobně jako:`cdnverify.www.consoto.com   CNAME   cdnverify.consoto.azureedge.net`  
8. Vrátí toohello **přidat vlastní doménu** okně a zadejte vlastní domény, včetně hello subdomény, v dialogovém okně hello. Například zadejte název domény hello ve formátu hello `www.contoso.com` nebo `cdn.contoso.com`. Všimněte si, že v tomto kroku není nutné toopreface hello subdomény s **cdnverify**.  
   
    Azure bude ověřte, zda existuje záznam CNAME hello pro název domény cdnverify hello, které jste zadali.
9. V tomto okamžiku se neověří vaše vlastní doména Azure, ale provoz tooyour domény není ještě probíhá směrované tooyour koncový bod CDN. Po uplynutí dostatečně dlouhé tooallow hello vlastní domény nastavení toopropagate toohello CDN okraj uzly (90 minut **Azure CDN společnosti Verizon**, 1 – 2 minuty pro **Azure CDN společnosti Akamai**), vrátí tooyour DNS webu registrátora a vytvořit další záznam CNAME, který mapuje koncový bod CDN tooyour subdomény. Můžete například zadat hello subdomény jako **www** nebo **cdn**, a název hostitele jako hello  **&lt;EndpointName >. azureedge.net**. Pro tento krok je dokončena registrace hello vaši vlastní doménu.
10. Nakonec můžete odstranit záznam CNAME hello jste vytvořili pomocí **cdnverify**, stejně jako tomu bylo nezbytné pouze jako zprostředkující krok.  

## <a name="verify-that-hello-custom-subdomain-references-your-cdn-endpoint"></a>Ověřte, zda tento vlastní subdomény hello odkazuje na koncový bod CDN
* Po dokončení registrace hello vaši vlastní doménu, můžete mít přístup k obsahu, který je uložený v mezipaměti v koncový bod CDN pomocí vlastní domény hello.
  Nejprve ověřit, že máte veřejný obsah, který je uloží do mezipaměti na koncový bod hello. Například pokud je váš koncový bod CDN přidružený účet úložiště, hello CDN ukládá obsah do mezipaměti v kontejnerech veřejného objektu blob. tootest hello vlastní doménu, ujistěte se, že vaše kontejneru je nastavená tooallow veřejný přístup a obsahuje alespoň jeden objekt blob.
* V prohlížeči přejděte na adresu toohello objektu blob hello používání vlastní domény hello. Například, pokud je vaše vlastní doména `cdn.contoso.com`, budou uložené v mezipaměti objektů blob hello URL tooa podobné toohello následující adresu URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Viz také
[Jak tooEnable hello Content Delivery Network (CDN) pro Azure.](cdn-create-new-endpoint.md)  

