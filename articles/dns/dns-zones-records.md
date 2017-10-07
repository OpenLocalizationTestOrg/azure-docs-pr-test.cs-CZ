---
title: "aaaDNS zóny a zaznamenává přehled – Azure DNS | Microsoft Docs"
description: "Přehled podpory pro hostování zón DNS a záznamy v Microsoft Azure DNS."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: jonatul
ms.openlocfilehash: f214c3e2e810a80a000281820acd35f0aaf5a7e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-dns-zones-and-records"></a>Přehled zóny DNS a záznamů

Tato stránka vysvětluje klíčové koncepty hello domén, zóny DNS a záznamy DNS a sady záznamů a jak jsou podporovány v Azure DNS.

## <a name="domain-names"></a>Názvy domén

Hello Domain Name System je hierarchie domén. Hello hierarchie začíná od domény "kořenový" hello, jejíž název je jednoduše "**.**'.  Následují domény nejvyšší úrovně, jako jsou „com“, „net“, „org“, „uk“ nebo „jp“.  Následují domény druhé úrovně, jako jsou „org.uk“ nebo „co.jp“. Hello domén v hierarchii DNS hello jsou globálně distribuované a hostované názvovými servery DNS po hello, world.

Registrátorem názvu domény je v organizaci, která vám umožní toopurchase název domény, například "contoso.com".  Zakoupení hello správné toocontrol hello hierarchii DNS pod názvem, například povolení toodirect hello název "www.contoso.com" tooyour společnosti webový server poskytuje název domény. Hello registrátora může být hostitelem hello domény v jeho vlastní názvové servery vaším jménem nebo umožňují toospecify alternativní názvové servery.

Azure DNS poskytuje infrastrukturu globálně distribuované, vysokou dostupnost názvu serveru, který můžete použít toohost vaší domény. Hostování domény do Azure DNS, můžete spravovat svoje záznamy DNS s hello stejné přihlašovací údaje, rozhraní API, nástroje, fakturaci a podporu jako jinými službami Azure.

Azure DNS aktuálně nepodporuje nákup názvů domén. Pokud chcete, aby toopurchase název domény, je třeba toouse doménového registrátora názvu domény třetí strany. Hello Registrátor obvykle účtuje malý roční poplatek. pro správu záznamů DNS, může být Hello domény pak hostovaný v Azure DNS. V tématu [delegovat tooAzure domény DNS](dns-domain-delegation.md) podrobnosti.

## <a name="dns-zones"></a>Zóny DNS

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>Záznamy DNS

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Time to live

Hello čas toolive nebo TTL, určuje, jak dlouho každý záznam se uloží do mezipaměti klienty, než se znovu dotazován. V hello výše příklad je hello TTL 3 600 sekund nebo 1 hodina.

V Azure DNS, hello TTL se zadává pro sadu záznamů hello, ne pro jednotlivé záznamy, takže hello stejnou hodnotu se používá pro všechny záznamy v rámci záznamů nastavit.  Můžete zadat libovolnou hodnotu TTL mezi 1 a 2 147 483 647 sekundami.

### <a name="wildcard-records"></a>Záznamy se zástupným znakem

Azure DNS podporuje [záznamy se zástupným znakem](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Záznamy se zástupným znakem se vrátí v odpovědi tooany dotazu s odpovídajícím názvem (pokud nebude nalezena bližší shoda v sadě záznamů bez zástupných znaků). Azure DNS podporuje zástupné sad záznamů u všech typů záznamů s výjimkou NS a SOA.

toocreate záznam zástupný znak nastavení, použijte název sady záznamů hello '\*'. Alternativně můžete také použít název s '\*'jako jeho nejvíce vlevo štítek, například"\*.foo".

### <a name="cname-records"></a>Záznamy CNAME

Sady záznamů CNAME nemůže existovat společně s další sady záznamů s hello stejný název. Například nelze vytvořit záznam CNAME s relativním názvem "www" hello a záznam A s hello relativní název "www" v hello stejný čas.

Protože hello vrcholu zóny (název = ' @') vždy obsahuje záznam NS a SOA hello sad, které byly vytvořeny při vytvoření zóny hello, nelze vytvořit záznam CNAME, který je nastaven na vrcholu zóny hello.

Těchto omezení vyplývat z norem DNS hello a nejsou omezení Azure DNS.

### <a name="ns-records"></a>Záznamy NS

sady záznamů Hello NS na vrcholu zóny hello (název ' @') se vytvoří automaticky s každou zónou DNS a je automaticky odstraněna při odstranění zóny hello (nelze jej odstranit samostatně).

Tato sada záznamů obsahuje názvy hello hello Azure DNS název servery přiřazené toohello zóny. Můžete přidat další název servery toothis NS sady záznamů, toosupport společně s více než jednoho poskytovatele DNS hostování domény. Můžete také upravit hello TTL a metadat pro tuto sadu záznamů. Však nelze odebrat ani změnit hello předem vyplněná názvových serverů Azure DNS. 

Všimněte si, že platí pouze toohello NS sady záznamů na vrcholu zóny hello. Další NS sady záznamů ve vaší zóně (jako podřízené zóny použité toodelegate) se dají vytvořit, upravit a odstraněn bez omezení.

### <a name="soa-records"></a>Záznamy SOA

Sady záznamů SOA se vytvoří automaticky na vrcholu hello každé zóny (název = ' @') a automaticky odstraněna při odstranění zóny hello.  Záznamy SOA nelze vytvořit nebo odstranit samostatně.

Můžete upravit všechny vlastnosti záznamu SOA hello s výjimkou vlastností "hostitele" hello, která je předem nakonfigurovaný toorefer toohello název primárního serveru název poskytuje Azure DNS.

### <a name="spf-records"></a>Záznamy SPF

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>Záznamy SRV

[Záznamy SRV](https://en.wikipedia.org/wiki/SRV_record) používají různé umístění serveru toospecify služby. Při zadávání záznam SRV v Azure DNS:

* Hello *služby* a *protokol* musí být zadaný jako součást hello název sady záznamů, předponu podtržítka.  Například '\_sip.\_ TCP.Name'.  Pro záznam na vrcholu zóny hello je bez nutnosti toospecify ' @' v názvu záznamu hello, jednoduše použijte hello služba a protokol, například "\_sip.\_ TCP'.
* Hello *s prioritou*, *váhy*, *port*, a *cíl* jsou určené jako parametry všechny záznamy v sadě záznamů hello.

### <a name="txt-records"></a>Záznamů TXT

TXT záznamy jsou použité toomap domény názvy tooarbitrary textové řetězce. Jsou použity v více aplikací, zejména související tooemail konfiguraci, například hello [odesílatele zásad Framework (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) a [DomainKeys identifikovat e-mailu (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

standardech DNS Hello povolit jeden toocontain záznamu TXT vícenásobných řetězců, z nichž každá může být až too254 znaků. Pokud se používají více řetězců, jsou zřetězených klienty a považován za jeden řetězec.

Při volání hello REST API služby Azure DNS, je nutné toospecify každý řetězec TXT samostatně.  Pokud používáte hello portálu Azure, PowerShell nebo rozhraní příkazového řádku rozhraní musíte zadat jeden řetězec na záznam, který je automaticky rozdělené do 254 znaků segmenty, v případě potřeby.

Hello vícenásobných řetězců v záznamu DNS Nezaměňovat s hello více záznamů TXT v sadě záznamů TXT.  Sady záznamů TXT může obsahovat několik záznamů, *každý z nich* může obsahovat více řetězců.  Azure DNS podporuje celkový řetězec délky až too1024 znaků v každý záznam TXT nastavit (v rámci všech záznamů kombinaci).

## <a name="tags-and-metadata"></a>Značky a metadata

### <a name="tags"></a>Značky

Značky jsou seznam dvojic název hodnota a jsou používány toolabel prostředky Azure Resource Manager.  Azure Resource Manager používá značky tooenable filtrované zobrazení faktury Azure a umožní vám tooset zásady, na které značky se vyžadují. Další informace o značkách najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](../azure-resource-manager/resource-group-using-tags.md).

Azure DNS podporuje pomocí Azure Resource Manager značek na prostředky zóny DNS.  Nepodporuje značky na sady záznamů DNS, i když jako alternativu, metadata, je podporovaný na sady záznamů DNS popsané níže.

### <a name="metadata"></a>Metadata

Jako alternativní toorecord nastavit značky, Azure DNS podporuje zadávání poznámek k sad záznamů pomocí 'metadat'.  Podobné tootags metadata umožňují vám tooassociate dvojice název hodnota s každou sadu záznamů.  To může být užitečné, například nastavit toorecord hello účel každý záznam.  Na rozdíl od značky metadata nelze použít tooprovide filtrované zobrazení faktury Azure a nelze zadat v zásadách Azure Resource Manager.

## <a name="etags"></a>Značky etag binárním rozsáhlým

Předpokládejme, že dvě osoby nebo dva procesy zkuste toomodify DNS záznam o na hello stejný čas. Které z nich wins? A hello Vítěz vědět, že jste se přepsat změny vytvořený jiným uživatelem?

Azure DNS používá značky etag binárním rozsáhlým toohandle souběžných změny toohello stejného zdroje bezpečně. Značky etag binárním rozsáhlým jsou oddělené od [Azure Resource Manager, značky'](#tags). Všechny prostředky DNS (zóny nebo sady záznamů) má Etag s ním spojená. Vždy, když se načte prostředek, je také načíst jeho Značka Etag. Při aktualizaci prostředku, je možné, že toopass zpět hello Značka Etag, Azure DNS můžete ověřit, že hello Etag na server odpovídá hello. Vzhledem k tomu, že každý zdroj aktualizace tooa výsledkem hello Značka Etag se znovu vygeneruje, neshoda značek Etag označuje, že došlo ke změně souběžných. Značky etag binárním rozsáhlým mohou sloužit také při vytváření nové tooensure prostředku, který prostředek hello již neexistuje.

Ve výchozím nastavení prostředí PowerShell Azure DNS používá značky etag binárním rozsáhlým tooblock souběžných změny toozones a sady záznamů. Hello volitelné *-přepsat* přepínač může být použité toosuppress Značka Etag kontroly, v takovém případě všechny souběžných budou přepsána změny, které mají došlo k chybě.

Na úrovni hello hello REST API služby Azure DNS jsou značky etag binárním rozsáhlým zadán pomocí hlavičky protokolu HTTP.  Jejich chování je uveden v hello následující tabulka:

| Záhlaví | Chování |
| --- | --- |
| Žádný |PUT vždy úspěšné (žádná značka Etag kontrola) |
| If-match<etag> |PUT pouze úspěšná, pokud prostředek existuje a Značka Etag odpovídá |
| If-match * |PUT pouze úspěšná, pokud existuje prostředek |
| If-none-match * |PUT pouze úspěšná, pokud prostředek neexistuje. |


## <a name="limits"></a>Omezení

Hello následující výchozí omezení platí při použití Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Další kroky

* toostart pomocí Azure DNS, zjistěte, jak příliš[vytvořit zónu DNS](dns-getstarted-create-dnszone-portal.md) a [vytvořit záznamy DNS](dns-getstarted-create-recordset-portal.md).
* toomigrate stávající zónu DNS, zjistěte, jak příliš[import a export souboru zóny DNS](dns-import-export.md).
