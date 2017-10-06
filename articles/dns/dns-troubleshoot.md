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
# <a name="azure-dns-troubleshooting-guide"></a>Azure DNS Průvodci odstraňováním potíží

Tato stránka obsahuje informace o odstraňování potíží pro běžné otázky Azure DNS.

Pokud tyto kroky problém nevyřeší, můžete také vyhledat nebo zveřejnit svůj problém na našem [Fórum komunity podpory na webu MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork). Alternativně můžete otevřete žádost o podporu Azure.


## <a name="i-cant-create-a-dns-zone"></a>Nelze vytvořit zónu DNS

tooresolve běžné problémy, zkuste jeden nebo více hello následující kroky:

1.  Zkontrolujte hello auditu Azure DNS zaznamená důvod selhání toodetermine hello.
2.  Název každé zóny DNS musí být v rámci dané skupiny prostředků jedinečný. To znamená, dvě DNS zóny s hello stejný název nelze sdílet skupinu prostředků. Zkuste použít jiný název zóny nebo jinou skupinu prostředků.
3.  Mohou se zobrazit chyba "Můžete mít dosáhli nebo Přesáhli jste maximální počet zón v předplatném {id předplatného} hello." Použijte jiné předplatné Azure, odstraňte některé zóny nebo požádejte podporu Azure tooraise svého limitu předplatného.
4.  Zobrazí chyba "hello zóny {zóny name} není k dispozici." Tato chyba znamená, že Azure DNS bylo nelze tooallocate názvové servery pro tuto zónu DNS. Zkuste použít jiný název zóny. Případně pokud jste vlastník název domény hello, požádejte podporu Azure, který můžete přidělit názvové servery, které pro vás.


### <a name="recommended-documents"></a>**Doporučené dokumenty**

[Záznamy a zóny DNS](dns-zones-records.md)
<br>
[Vytvoření zóny DNS](dns-getstarted-create-dnszone-portal.md)

## <a name="i-cant-create-a-dns-record"></a>Nejde mi vytvořit záznam DNS

tooresolve běžné problémy, zkuste jeden nebo více hello následující kroky:

1.  Zkontrolujte hello auditu Azure DNS zaznamená důvod selhání toodetermine hello.
2.  Existuje sada záznamů hello již?  Azure DNS spravuje záznamy pomocí záznamu *nastaví*, což jsou kolekce hello záznamů hello stejný název a text hello, stejný typ. Pokud záznam s hello stejný název a typ již existuje, pak tooadd jiné takové byste neměli upravovat stávající záznam hello sady záznamů.
3.  Pokoušíte toocreate záznamů na vrcholu zóny DNS hello (hello kořenové zóně hello)? Pokud ano, hello konvenci DNS je toouse hello ' @' znak jako název záznamu hello. Všimněte si také, že standardech DNS hello nepovoluje záznamy CNAME na vrcholu zóny hello.
4.  Vznikl konflikt CNAME?  Hello standardech DNS záznam CNAME se hello stejný název jako záznamy o jiný typ nepovolují. Pokud máte existující CNAME, vytvoření záznamu s hello, v případě selhání stejný název jiného typu.  Podobně vytváření záznam CNAME se nezdaří, pokud název hello odpovídá existující záznam jiného typu. Odeberte hello konflikt odebráním hello jiný záznam nebo zvolte jiný záznam název.
5.  Dosáhli jste limitu hello hello počtu sady záznamů v zóně DNS povolené? Hello aktuální počet sad záznamů a hello maximální počet sad záznamů jsou zobrazeny v portálu Azure, v části hello 'vlastnosti' pro zónu hello hello. Pokud bylo dosaženo tohoto limitu, pak buď odstranit některé sady záznamů nebo kontaktujte podporu Azure tooraise záznamů nastaveného limitu pro tuto zónu, a zkuste to znovu. 


### <a name="recommended-documents"></a>**Doporučené dokumenty**

[Záznamy a zóny DNS](dns-zones-records.md)
<br>
[Vytvoření zóny DNS](dns-getstarted-create-dnszone-portal.md)



## <a name="i-cant-resolve-my-dns-record"></a>Nejde mi přeložit záznam DNS

Překlad názvů DNS je vícefázový proces, který může selhat z mnoha důvodů. Hello následující postup vám pomůže prozkoumat, proč se nedaří překlad názvů DNS pro záznam DNS v zóně hostované v Azure DNS.

1.  Potvrďte, že záznamy DNS hello byly nakonfigurovány správně v Azure DNS. Zkontrolujte záznamy DNS hello v hello portál Azure, kontrola, zda jsou správné hello název zóny, název záznamu a typ záznamu.
2.  Potvrďte, že záznamy DNS hello správně přeložit na názvových serverů Azure DNS hello.
    - Pokud provedete dotazy DNS z vašeho místního počítače, může se zobrazit výsledky uložené v mezipaměti, které neodpovídají hello aktuální stav hello názvové servery.  Kromě toho podnikové sítě často používají proxy servery DNS, který zabrání se dotazy DNS přesměruje toospecific názvové servery.  tooavoid tyto problémy používat webová služba pro překlad názvů, jako [digwebinterface](http://digwebinterface.com).
    - Zda toospecify být hello správné názvové servery pro zónu DNS, jak je znázorněno v hello portálu Azure.
    - Zkontrolujte, zda je název DNS hello správný, (máte toospecify hello plně kvalifikovaný název, včetně názvu zóny hello) a typ záznamu hello je správný
3.  Zkontrolujte název domény DNS hello je správně [delegovaný názvových serverů Azure DNS toohello](dns-domain-delegation.md). Existuje [celá řada webů třetích stran, které poskytují ověření delegování DNS](https://www.bing.com/search?q=dns+check+tool). Tento test je *zóny* delegování test, takže byste měli jenom zadat název zóny DNS hello a není hello plně kvalifikovaný název záznamu.
4.  Po dokončení hello výše, vaše záznam DNS by měl nyní vyřešit správně. tooverify, můžete znovu použít [digwebinterface](http://digwebinterface.com), tentokrát pomocí nastavení hello výchozí název serveru.


### <a name="recommended-documents"></a>**Doporučené dokumenty**

[Delegát tooAzure domény DNS](dns-domain-delegation.md)



## <a name="how-do-i-specify-hello-service-and-protocol-for-an-srv-record"></a>Jak určit hello "služba" a "protokol" pro záznam SRV?

Azure DNS spravuje záznamy DNS jako sady záznamů – hello kolekce záznamů s hello stejný název a text hello, stejný typ. Pro sadu záznamů SRV služby"hello" a "protokol" potřebovat toobe zadaný jako součást hello název sady záznamů. Hello další parametry SRV ('prioritu', 'weight', 'port' a "target") se zadávají samostatně pro jednotlivé záznamy v sadě záznamů hello.

Ukázkové názvy záznamů SRV (služba = sip, protokol = tcp):

- \_SIP. \_tcp (vytvoří záznamů na vrcholu zóny hello)
- \_sip.\_tcp.sipservice (vytvoří sadu záznamů s názvem sipservice)

### <a name="recommended-documents"></a>**Doporučené dokumenty**

[Záznamy a zóny DNS](dns-zones-records.md)
<br>
[Vytvoření sady záznamů DNS a záznamy pomocí hello portálu Azure](dns-getstarted-create-recordset-portal.md)
<br>
[Záznamy typu SRV (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)


## <a name="next-steps"></a>Další kroky

* Další informace o [Azure DNS zóny a záznamy](dns-zones-records.md)
* toostart pomocí Azure DNS, zjistěte, jak příliš[vytvořit zónu DNS](dns-getstarted-create-dnszone-portal.md) a [vytvořit záznamy DNS](dns-getstarted-create-recordset-portal.md).
* toomigrate stávající zónu DNS, zjistěte, jak příliš[import a export souboru zóny DNS](dns-import-export.md).

