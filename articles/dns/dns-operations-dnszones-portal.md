---
title: "aaaManage DNS zóny v Azure DNS - portálu Azure | Microsoft Docs"
description: "Můžete spravovat pomocí portálu Azure hello zóny DNS. Tento článek popisuje, jak tooupdate, odstraňte a vytvořte zóny DNS na Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: 0d8ce302bb7126dfe8077a6f3e33418e16fcea64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-hello-azure-portal"></a>Jak hello toomanage zóny DNS v portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Tento článek ukazuje, jak toomanage DNS zóny pomocí hello portálu Azure. Můžete také spravovat zóny DNS pomocí hello napříč platformami [rozhraní příkazového řádku Azure](dns-operations-dnszones-cli.md) nebo hello Azure [prostředí PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlaste se toohello portálu Azure
2. V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový > sítě >** a pak klikněte na **zónu DNS** tooopen hello vytvoření DNS zóny okno.

    ![Zóna DNS](./media/dns-operations-dnszones-portal/openzone650.png)

4. Na hello **zóny DNS vytvořte** okno Zadejte hello následující hodnoty a pak klikněte na **vytvořit**:


   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|contoso.com|Název zóny DNS hello Hello|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné zóny DNS hello toocreate v.|
   |**Skupina prostředků**|**Vytvořit novou:** contosoDNSRG|Vytvořte skupinu prostředků. Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali. Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) článek s přehledem.|
   |**Umístění**|Západní USA||

> [!NOTE]
> Skupina prostředků Hello odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na zónu DNS hello. umístění zóny DNS Hello je vždy "globální" a není zobrazen.

## <a name="list-dns-zones"></a>Seznam zón DNS

V hello portálu Azure, přejděte příliš**další služby** > **sítě** > **zóny DNS**. Každé zóny DNS se jedná vlastní prostředek, informace, jako je počet sad záznamů a názvové servery je možné zobrazit z tohoto zobrazení. sloupec Hello **NÁZVOVÉ servery** není v hello výchozí zobrazení tooadd klikněte na tlačítko **sloupce**, vyberte **názvové servery** a klikněte na tlačítko **provádí**.

![výpis zóny DNS](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Odstranit zónu DNS

Přejděte zóny DNS tooa hello portálu. Na hello **zónu DNS** okně klikněte na tlačítko **odstranit zónu**. Jste výzvami tooconfirm jsou podmínka mimo zóny DNS toodelete hello. Odstranění zóny DNS také odstraní všechny hello záznamy, které jsou obsaženy v zóně hello.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toowork s zóny DNS a záznamy, navštivte stránky [Začínáme s Azure DNS pomocí portálu Azure hello](dns-getstarted-portal.md).
