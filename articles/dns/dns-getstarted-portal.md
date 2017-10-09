---
title: "aaaGet začít s Azure DNS pomocí hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate DNS zóna a záznam v Azure DNS. Toto je podrobný průvodce toocreate a spravovat první zónu DNS a záznam pomocí hello portálu Azure."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 5cea01d840d794001cccac64defed8b329d948db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-hello-azure-portal"></a>Začínáme s Azure DNS pomocí hello portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Tento článek vás provede kroky toocreate hello první zónu DNS a záznam pomocí hello portálu Azure. Můžete také provést tyto kroky, pomocí Azure PowerShell nebo hello rozhraní příkazového řádku Azure napříč platformami.

Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény. toostart hostovat svoji doménu v Azure DNS, je nutné toocreate zóny DNS pro název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS. Nakonec toopublish DNS zóny toohello Internet, musíte tooconfigure hello názvové servery pro doménu hello. Každý z těchto kroků je popsaná v hello následující kroky.

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlaste se toohello portálu Azure
2. V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový > sítě >** a pak klikněte na **zónu DNS** tooopen hello vytvoření DNS zóny okno.

    ![Zóna DNS](./media/dns-getstarted-portal/openzone650.png)

4. Na hello **zóny DNS vytvořte** okno Zadejte hello následující hodnoty a pak klikněte na **vytvořit**:


   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|contoso.com|Název zóny DNS hello Hello|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné zóny DNS hello toocreate v.|
   |**Skupina prostředků**|**Vytvořit novou:** contosoDNSRG|Vytvořte skupinu prostředků. Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali. Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) článek s přehledem.|
   |**Umístění**|Západní USA||

> [!NOTE]
> Skupina prostředků Hello odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na zónu DNS hello. umístění zóny DNS Hello je vždy "globální" a není zobrazen.

## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

Hello následující příklad vás provede procesem hello vytváření nových "A" záznamu. Pro jiné typy záznamů a toomodify existující záznamy, najdete v části [záznamy DNS spravovat a sad záznamů pomocí portálu Azure hello](dns-operations-recordsets-portal.md). 

1. S hello vytvoření zóny DNS v hello portál Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello **contoso.com** zónu DNS v hello okno všechny prostředky. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **contoso.com** v hello **filtrovat podle názvu...** pole zóny DNS hello tooeasily přístup.

1. Hello horní části hello **zónu DNS** vyberte **+ sady záznamů** tooopen hello **přidat sadu záznamů** okno.

1. Na hello **přidat sadu záznamů** okno, zadejte následující hodnoty hello a klikněte na **OK**. V tomto příkladu vytváříte záznam A.

   |**Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|www|Název záznamu hello|
   |**Typ**|A| Typ toocreate záznamů DNS, přípustné hodnoty jsou A, AAAA, CNAME, MX, NS, SRV, TXT a PTR.  Další informace o typech záznamů najdete v tématu [Přehled záznamů a zón DNS](dns-zones-records.md).|
   |**Hodnota TTL**|1|Time-to-live hello DNS požadavku.|
   |**Jednotka hodnoty TTL**|Hodiny|Měřené jednotky času pro hodnotu TTL.|
   |**IP adresa**|ipAddressValue| Tato hodnota je hello IP adresu, která řeší záznam DNS hello.|

## <a name="view-records"></a>Zobrazení záznamů

V dolní části hello hello DNS zóny okna uvidíte hello záznamy pro zónu DNS hello. Měli byste vidět hello výchozí DNS a SOA záznamů, které jsou vytvořené v každé zóny, a všechny nové záznamy, které jste vytvořili.

![zóna](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Aktualizace názvových serverů

Jakmile jste uspokojit, že zóna DNS a záznamy byly nastaveny správně, musíte tooconfigure váš název domény toouse názvových serverů Azure DNS hello. To umožňuje jiných uživatelů v Internetu toofind hello svoje záznamy DNS.

Hello názvové servery zóny jsou uvedeny v hello portálu Azure:

![zóna](./media/dns-getstarted-portal/viewzonens500.png)

Tyto názvové servery musí být nakonfigurovaný u registrátora názvu domény hello (kde jste zakoupili hello název domény). Registrátora nabízí možnost tooset hello až hello názvové servery pro doménu hello. Další informace najdete v tématu [delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Odstranění všech prostředků

toodelete vytvořit všechny prostředky v tomto článku, dokončení hello následující kroky:

1. V portálu Azure hello **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello **MyResourceGroup** skupina prostředků v hello okno všechny prostředky. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **MyResourceGroup** v hello **filtrovat podle názvu...** pole tooeasily přístup hello prostředků skupiny.
1. V hello **MyResourceGroup** okně klikněte na tlačítko hello **odstranit** tlačítko.
1. Hello portál vyžaduje tootype hello název hello prostředků skupiny tooconfirm, které chcete toodelete ho. Klikněte na tlačítko **odstranit**, typ *MyResourceGroup* hello název skupiny prostředků, klikněte **odstranit**. Odstranění skupiny prostředků se odstraní všechny prostředky v rámci skupiny prostředků hello, takže vždy být zda tooconfirm hello obsah skupinu prostředků. před odstraněním. Odstraní všechny prostředky obsažené v rámci skupiny prostředků hello Hello portálu, a pak odstraní samotná skupina prostředků hello. Tento proces trvá několik minut.


## <a name="next-steps"></a>Další kroky

toolearn Další informace o Azure DNS, najdete v části [přehled Azure DNS](dns-overview.md).

toolearn Další informace o správě záznamů DNS v Azure DNS, najdete v části [záznamy DNS spravovat a sad záznamů pomocí portálu Azure hello](dns-operations-recordsets-portal.md).

