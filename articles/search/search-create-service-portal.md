---
title: "aaaCreate služby Azure Search na portálu hello | Microsoft Docs"
description: "Zřídit služby Azure Search na portálu hello."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: f1c7197a1467e32bd4b360b165c9059e6bb0e496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-service-in-hello-portal"></a>Vytvoření služby Azure Search na portálu hello

Tento článek vysvětluje, jak toocreate nebo zřízení Azure Search, které se služby portálu hello. Prostředí PowerShell pokyny najdete v tématu [spravovat Azure Search pomocí prostředí PowerShell](search-manage-powershell.md).

## <a name="subscribe-free-or-paid"></a>Přihlášení k odběru (volné nebo placené)

[Otevřít bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) a používat bezplatné kredity tootry na placené služby Azure. Až kredity vyčerpáte, ponechte hello účet a pokračovat toouse bezplatné služby Azure, třeba weby. Platební karty se nikdy účtovat Pokud explicitně změnit nastavení a požádejte toobe účtovat.

Alternativně [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure. 

## <a name="find-azure-search"></a>Najít vyhledávání systému Azure
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V horní části hello levého horního rohu klikněte na tlačítko hello plus přihlašovací ("+").
3. Vyberte **Web + mobilní** > **služba Azure Search**.

![](./media/search-create-service-portal/find-search2.png)

## <a name="name-hello-service-and-url-endpoint"></a>Název hello služby a adresu URL koncového bodu

Název služby je součástí hello URL koncového bodu, vůči které jsou vydány volání rozhraní API. Zadejte název vaší služby hello **URL** pole. 

Požadavky na název služby:
   * 2 až 60 znaků.
   * malá písmena, číslice nebo spojovníky ("-")
   * žádné pomlčka ("-") jako hello nejprve 2 znaky nebo poslední jeden znak
   * žádné po sobě jdoucí pomlčky ("--")

## <a name="select-a-subscription"></a>Vyberte předplatné.
Pokud máte více než jedno předplatné, vyberte ten, který má také služby úložiště dat nebo souboru. Vyhledávání systému Azure můžete automaticky rozpoznat úložiště Azure Table a objektů Blob, databáze SQL a Azure Cosmos DB pro indexování prostřednictvím *indexery*, ale pouze pro služby v hello stejného předplatného.

## <a name="select-a-resource-group"></a>Vybrat skupinu prostředků
Skupina prostředků je kolekce služeb Azure a prostředky, použít společně. Například pokud používáte Azure Search tooindex databázi SQL, pak obě služby by měl být součástí hello stejnou skupinu prostředků.

> [!TIP]
> Odstranění skupiny prostředků, odstraní se také hello služby v něm. Pro projekty prototypu využívá více služeb, všechny z nich uvedení v hello stejnou skupinu prostředků usnadňuje čištění po hello projektu. 

## <a name="select-a-hosting-location"></a>Vyberte umístění pro hostování 
Jako služba Azure může být hostovaný Azure Search v datových centrech kolem hello, world. Všimněte si, že [ceny se může lišit](https://azure.microsoft.com/pricing/details/search/) zeměpisných údajů.

## <a name="select-a-pricing-tier-sku"></a>Vyberte cenovou úroveň (SKU)
[Služba Azure Search je aktuálně k dispozici v několika cenových úrovní](https://azure.microsoft.com/pricing/details/search/): volné, Basic nebo Standard. Každá úroveň má svou vlastní [kapacity a omezení](search-limits-quotas-capacity.md). V tématu [zvolte cenovou úroveň nebo SKU](search-sku-tier.md) pokyny.

V tomto návodu jsme zvolili hello úrovně Standard pro naši službu.

## <a name="create-your-service"></a>Vytvoření služby

Mějte na paměti toopin řídicího panelu služby toohello pro snadný přístup při každém přihlášení.

![](./media/search-create-service-portal/new-service2.png)

## <a name="scale-your-service"></a>Škálování služby
Může trvat několik minut toocreate služby (15 minut nebo déle v závislosti na úrovni hello). Po zřízení služby, je možné škálovat ho toomeet vašim potřebám. Protože jste zvolili hello úrovně Standard pro služby Azure Search, můžete škálovat služby v dvěma rozměry: repliky a oddíly. Jste nastavili hello základní vrstvy, můžete použít pouze repliky. Pokud jste zřídili hello bezplatná služba, škálování není k dispozici.

***Oddíly*** povolit služby toostore a vyhledávání pomocí více dokumentů.

***Repliky*** povolit vaší služby toohandle vyšší zatížení vyhledávací dotazy.

> [!Important]
> Služba musí mít [2 repliky smlouva SLA jen pro čtení a 3 repliky pro čtení/zápisu SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Přejděte okno služby vyhledávání tooyour v hello portálu Azure.
2. V levém navigačním podokně hello vyberte **nastavení** > **škálování**.
3. Použijte hello slidebar tooadd repliky nebo oddíly.

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> Každá úroveň má jiný [omezení](search-limits-quotas-capacity.md) na celkový počet jednotek vyhledávání povolené v jedné službě hello (repliky * oddíly = celkový počet jednotek vyhledávání).

## <a name="when-tooadd-a-second-service"></a>Když tooadd druhý služby

Většina zákazníků použít pouze jednu službu zřídí v vrstvy, která poskytuje hello [pravým vyrovnávání prostředků](search-sku-tier.md). Jedna služba může být hostitelem více indexů, předmět toohello [maximální limit hello vrstvy vyberete](search-capacity-planning.md), s každou indexem izolované z jiné. Ve službě Azure Search, můžete požadavky jen bude směrovanou tooone index, minimalizovat riziko hello načtení náhodnému nebo záměrnému dat z jiných indexy v hello stejné služby.

I když většina zákazníků používat jenom jedna služba, může být nutné v případě provozních požadavků zahrnují následující hello redundance služby:

+ Zotavení po havárii (data center výpadek). Služba Azure Search neposkytuje rychlých převzetí služeb při selhání v případě hello výpadku. Doporučení a pokyny najdete v tématu [služby správy](search-manage.md).
+ Vaše šetření víceklientský modelování má zjistíte, že další služby hello optimální návrhu. Další informace najdete v tématu [návrhu víceklientský](search-modeling-multitenant-saas-applications.md).
+ Pro globální nasazené aplikace může vyžadovat instanci Azure Search v několika latence oblasti toominimize mezinárodní provoz vaší aplikace.

> [!NOTE]
> Ve službě Azure Search nelze oddělit indexování a dotazování zatížení; Proto by nikdy vytvořit více služeb pro oddělenou úlohy. Index je vždy dotazován na hello služby byl vytvořen ve kterém it (nelze vytvořit index v jedné službě a zkopírujte jej tooanother).
>

Druhý služby není nutné pro zajištění vysoké dostupnosti. Vysoká dostupnost pro dotazy se dosáhne, když používáte 2 nebo více replik v hello stejnou službu. Aktualizace repliky jsou sekvenční, což znamená, že je alespoň jeden funkční při aktualizaci služby se nasazuje. Další informace o provozu najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Další kroky
Po zřízení služby Azure Search, jste připraveni příliš[definujte index](search-what-is-an-index.md) vám umožní nahrát a hledání vaše data.

tooaccess hello služby z kódu nebo skriptu, zadejte adresu URL hello (*název služby*. search.windows.net) a klíč. Klíče správce poskytují úplný přístup; klíče dotazu udělují přístup jen pro čtení. V tématu [jak toouse Azure hledání v rozhraní .NET](search-howto-dotnet-sdk.md) tooget spuštěna.

V tématu [sestavení a dotazování indexu první](search-get-started-portal.md) najdete rychlý kurz založené na portálu.

