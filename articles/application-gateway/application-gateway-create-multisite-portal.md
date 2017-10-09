---
title: "aaaHost víc lokalit s Azure Application Gateway | Microsoft Docs"
description: "Tato stránka poskytuje pokyny tooconfigure existující bránu aplikací Azure pro hostování několika webových aplikací na hello stejnou bránu s hello portálu Azure."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 2172aa2c80720f6f1ab7dd91745b44654bcaee00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfigurace existující aplikační brány pro hostování několika webových aplikací

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

Hostování více lokality můžete toodeploy více než jednu webovou aplikaci na hello stejné aplikační brány. Přitom spoléhá na přítomnost Hlavička hostitele v hello příchozí požadavek HTTP, který naslouchací proces by přijímat přenosy toodetermine. naslouchací proces Hello potom směrovat přenosy tooappropriate back-end fondu podle konfigurace v definici pravidla hello hello brány. V protokolu SSL povoleno webových aplikací Aplikační brána spoléhá na hello indikace názvu serveru (SNI) rozšíření toochoose hello správné naslouchací proces pro hello webový provoz. Běžně používá pro více hostování lokality je tooload vyrovnávat požadavky pro fondy jiných webových domén toodifferent back-end serverů. Podobně jako více subdomény hello stejné kořenové domény může být také hostovaná v hello stejné aplikační brány.

## <a name="scenario"></a>Scénář

V následujícím příkladu hello, aplikační brána obsluhuje přenosy dat pro contoso.com a fabrikam.com s dvěma fondy back-end serverů: contoso fondu serverů a fond serverů fabrikam. Podobně jako instalační program může být subdomény používané toohost jako app.contoso.com a blog.contoso.com.

![scénář nasazení ve více lokalitách][multisite]

## <a name="before-you-begin"></a>Než začnete

Tento scénář přidá podporu více lokalit tooan existující aplikační brány. toocomplete tento scénář existující bránu aplikace potřebuje k dispozici tooconfigure toobe. Navštivte [vytvoření služby application gateway pomocí portálu hello](application-gateway-create-gateway-portal.md) toolearn jak toocreate základní aplikační brána hello portálu.

Následující Hello se, že kroky hello tooupdate hello aplikační brány:

1. Vytvořte toouse back endové fondy pro každou lokalitu.
2. Vytvořte naslouchací proces pro každou lokalitu Aplikační brána podporuje.
3. Vytvoření pravidla toomap každý naslouchací proces s hello odpovídající back-end.

## <a name="requirements"></a>Požadavky

* **Fond back-end serverů:** hello seznam IP adres hello back-end serverů. uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP. Můžete také použít plně kvalifikovaný název domény.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.
* **Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány. Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL). Pro aplikace s povolenou více servery brány název hostitele a indikátory SNI jsou také přidat.
* **Pravidlo:** hello pravidlo váže naslouchací proces hello, hello fondu back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu. Pravidla se zpracovávají v pořadí hello, které jsou uvedeny a provoz se přesměruje prostřednictvím hello první pravidlo, které odpovídá bez ohledu na specifické podobě. Například pokud máte pravidlo pomocí základní naslouchací proces a pravidla pomocí více lokalit naslouchací proces i na stejný port, hello pravidlo s hello naslouchací proces hello více lokalit musí být uvedený před hello pravidlo základní naslouchací proces hello-li pravidlo více lokalit toofunction hello jako byl očekáván. 
* **Certifikáty:** každý naslouchací proces vyžaduje jedinečný certifikát, v tomto příkladu jsou vytvořeny 2 naslouchací procesy pro více lokalit. Dva certifikáty PFX a hesla hello u nich třeba toobe vytvořili.

## <a name="create-back-end-pools-for-each-site"></a>Vytvoření fondu back-end pro každou lokalitu

Back-end fondu pro každou lokalitu, že je potřeba brána podporuje aplikace, v takovém případě 2 se vytvářejí, jeden pro contoso11.com a jeden pro fabrikam11.com.

### <a name="step-1"></a>Krok 1

Přejděte tooan existující aplikační brána v hello portálu Azure (https://portal.azure.com). Vyberte **back-endové fondy** a klikněte na tlačítko **přidat**

![Přidat back-endové fondy][7]

### <a name="step-2"></a>Krok 2

Vyplnit hello informace pro fond back-end hello **pool1**, přidání hello ip adresy nebo plně kvalifikované názvy domén pro hello back-end serverů a klikněte na tlačítko **OK**

![nastavení pool1 fondu back-end][8]

### <a name="step-3"></a>Krok 3

V okně back-endové fondy hello klikněte na tlačítko **přidat** tooadd další back-end fondu **pool2**, přidání hello ip adresy nebo plně kvalifikované názvy DOMÉN pro hello back-end serverů a klikněte na tlačítko **OK**

![nastavení pool2 fondu back-end][9]

## <a name="create-listeners-for-each-back-end"></a>Vytvořte naslouchací procesy pro každý back-end

Aplikační brána spoléhá na HTTP 1.1 toohost hlavičky hostitele více než jeden web na hello stejnou veřejnou IP adresu a port. Hello základní naslouchací proces vytvoření portálu hello tuto vlastnost neobsahuje.

### <a name="step-1"></a>Krok 1

Klikněte na tlačítko **naslouchací procesy** hello existující aplikační brány a klikněte na tlačítko **Multi-Site** první naslouchací proces tooadd hello.

![okno Přehled – moduly naslouchání][1]

### <a name="step-2"></a>Krok 2

Vyplňte hello informace pro naslouchací proces hello. V tomto příkladu SSL je nakonfigurované ukončení, vytvořte nový port front-endu. Nahrajte toobe certifikátu .pfx hello používá pro ukončení protokolu SSL. jediným rozdílem Hello v tomto okně okno porovnání toohello standardní základní naslouchací proces je název hostitele hello.

![naslouchací proces vlastnosti okna][2]

### <a name="step-3"></a>Krok 3

Klikněte na tlačítko **Multi-Site** a vytvořit naslouchací proces jiný, jak je popsáno v předchozím kroku hello pro druhou lokalitu hello. Ujistěte se, že toouse jiný certifikát pro druhý naslouchací proces hello. jediným rozdílem Hello v tomto okně okno porovnání toohello standardní základní naslouchací proces je název hostitele hello. Vyplnění hello informací pro naslouchací proces hello a klikněte na tlačítko **OK**.

![naslouchací proces vlastnosti okna][3]

> [!NOTE]
> Vytvoření naslouchacího procesu v hello portál Azure pro službu application gateway je dlouhotrvající úlohy, může trvat některé hello toocreate čas dva naslouchací procesy v tomto scénáři. Při dokončení hello naslouchací procesy zobrazit hello portálu, jak je vidět v hello následující bitové kopie:

![Přehled naslouchací proces][4]

## <a name="create-rules-toomap-listeners-toobackend-pools"></a>Vytvoření pravidel fondy toobackend toomap – moduly naslouchání

### <a name="step-1"></a>Krok 1

Přejděte tooan existující aplikační brána v hello portálu Azure (https://portal.azure.com). Vyberte **pravidla** a zvolte existující výchozí pravidlo hello **rule1 New** a klikněte na tlačítko **upravit**.

### <a name="step-2"></a>Krok 2

Vyplňte okno hello pravidla, jak je vidět v hello následující obrázek. Výběr hello první naslouchací proces a první fondu a kliknutím na **Uložit** při dokončení.

![upravit existující pravidlo][6]

### <a name="step-3"></a>Krok 3

Klikněte na tlačítko **základní pravidlo** toocreate hello druhé pravidlo. Vyplňte formulář hello hello druhý naslouchací proces a sekundu fond back-end a klikněte na tlačítko **OK** toosave.

![přidat základní pravidlo okno][10]

Tento scénář se dokončí konfigurace aplikační brány s podporou více lokalit prostřednictvím hello portálu Azure.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooprotect své weby s [Application Gateway - brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
