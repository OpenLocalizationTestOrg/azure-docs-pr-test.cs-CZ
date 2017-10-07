---
title: "aaaManage DNS záznam sady a záznamy s Azure DNS | Microsoft Docs"
description: "Azure DNS poskytuje záznam DNS toomanage schopností hello nastaví a zaznamenává při hostování vaší domény."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ed44a1-7bfe-454f-964e-922ad978264a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 2e62d017341589eaf8d1f8df2fe5db4b973381d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-hello-azure-portal"></a>Spravovat záznamy a sadami záznamů DNS pomocí hello portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Tento článek ukazuje, jak hello toomanage sady záznamů a záznamů zóny DNS pomocí portálu Azure.

Je důležité toounderstand hello rozdíl mezi sady záznamů DNS a jednotlivé záznamy DNS. Sada záznamů je kolekce záznamů v zóně, které mají hello stejný název a jsou hello stejný typ. Další informace najdete v tématu [DNS vytvoření sady záznamů a záznamy pomocí portálu Azure hello](dns-getstarted-create-recordset-portal.md).

## <a name="create-a-new-record-set-and-record"></a>Vytvoření nové sady záznamů a záznamů

toocreate záznamů v hello portál Azure, najdete v části [hello záznamy DNS vytvořit pomocí portálu Azure](dns-getstarted-create-recordset-portal.md).

## <a name="view-a-record-set"></a>Zobrazení sady záznamů

1. V hello portálu Azure, přejděte toohello **zónu DNS** okno.
2. Hledání pro sadu záznamů hello a vyberte jej. Otevře vlastnosti hello sady záznamů.

    ![Vyhledejte sady záznamů](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-tooa-record-set"></a>Přidat novou sadu záznamů tooa záznamů

Můžete přidat do sady záznamů tooany too20 záznamy. Sada záznamů nesmí obsahovat dva identické záznamy. Prázdný sady záznamů (s nulový počet záznamů) mohou být vytvořeny, ale nejsou zobrazeny na názvových serverů Azure DNS hello. Sady záznamů typu CNAME může obsahovat maximálně jeden záznam.

1. Na hello **vlastnosti sady záznamů** okno pro vaši zónu DNS, klikněte na tlačítko záznam hello nastavit, že chcete tooadd záznam o.

    ![Vyberte sady záznamů](./media/dns-operations-recordsets-portal/selectset500.png)

2. Zadejte vlastnosti sady záznamů hello vyplněním hello polí.

    ![Přidání záznamu](./media/dns-operations-recordsets-portal/addrecord500.png)

3. Klikněte na tlačítko **Uložit** v hello horní části okna toosave hello nastavení. Zavřete okno hello.
4. V horním hello uvidíte, zda je ukládání hello záznamu.

    ![Ukládání sady záznamů](./media/dns-operations-recordsets-portal/saving150.png)

Po uložení hello záznamu, hello hodnot hello **zónu DNS** okně se projeví hello nový záznam.

## <a name="update-a-record"></a>Aktualizovat záznam

Při aktualizaci záznamu v existující sady záznamů hello pole, která můžete aktualizovat závisí na hello typu záznamu, kterými pracujete.

1. Na hello **vlastnosti sady záznamů** okna pro vaše sadu záznamů, vyhledejte záznam hello.
2. Upravte záznamu hello. Když upravíte záznam, můžete změnit hello dostupná nastavení pro záznam hello. V následujícím příkladu hello, hello **IP adresu** vybrat pole a hello IP adresa je v procesu hello upravována.

    ![Upravte záznam](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. Klikněte na tlačítko **Uložit** v hello horní části okna toosave hello nastavení. V horním pravém rohu hello uvidíte hello oznámení, že záznam hello se uložila.

    ![Uložit záznamovou sadu](./media/dns-operations-recordsets-portal/saved150.png)

Po uložení hello záznamu, hello hodnoty pro záznam hello nastavené na hello **zónu DNS** okně se projeví hello aktualizovat záznam.

## <a name="remove-a-record-from-a-record-set"></a>Odebrat záznam ze sady záznamů

Můžete použít hello Azure portálu tooremove záznamy ze sady záznamů. Všimněte si, že odstraněním hello poslední záznam ze záznamů sady neodstraní hello sady záznamů.

1. Na hello **vlastnosti sady záznamů** okna pro vaše sadu záznamů, vyhledejte záznam hello.
2. Klikněte na tlačítko, které chcete tooremove záznam hello. Potom vyberte **odebrat**.

    ![Odebrat záznam](./media/dns-operations-recordsets-portal/removerecord500.png)

3. Klikněte na tlačítko **Uložit** v hello horní části okna toosave hello nastavení.
4. Po odebrání záznamu hello hello hodnoty pro záznam hello na hello **zónu DNS** okně se projeví odebrání hello.

## <a name="delete"></a>Odstranit sadu záznamů

1. Na hello **vlastnosti sady záznamů** pro vaše sadu záznamů, klikněte na **odstranit**.

    ![Odstranit sadu záznamů](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. Zobrazí dotaz, pokud chcete, aby sada záznamů toodelete hello.
3. Ověřte, zda text hello název odpovídá hello záznam nastavena má toodelete a pak klikněte na tlačítko **Ano**.
4. Na hello **zónu DNS** okno, ověřte, zda sada záznamů hello je už nebude viditelné.

## <a name="work-with-ns-and-soa-records"></a>Práce se záznamy NS a SOA

Záznamy NS a SOA, které se automaticky vytvoří spravuje odlišně od ostatních typů záznamu.

### <a name="modify-soa-records"></a>Upravit záznamy SOA

Nelze přidat nebo odebrat záznamy z hello automaticky vytvořil záznam SOA nastavit na vrcholu zóny hello (název = "@"). Ale můžete upravit některý z parametrů hello v rámci hello záznam SOA (s výjimkou "hostitel") a záznam hello nastavit hodnotu TTL.

### <a name="modify-ns-records-at-hello-zone-apex"></a>Upravit NS záznamy na vrcholu zóny hello

Nastavte na vrcholu zóny hello záznam NS Hello se vytvoří automaticky s každou zónou DNS. Obsahuje názvy hello hello Azure DNS název servery přiřazené toohello zóny.

Můžete přidat další název servery toothis NS sady záznamů, toosupport společně s více než jednoho poskytovatele DNS hostování domény. Můžete také upravit hello TTL a metadat pro tuto sadu záznamů. Však nelze odebrat ani změnit hello předem vyplněná názvových serverů Azure DNS.

Všimněte si, že platí pouze toohello NS sady záznamů na vrcholu zóny hello. Další NS sady záznamů ve vaší zóně (jako podřízené zóny použité toodelegate) můžete změnit bez omezení.

### <a name="delete-soa-or-ns-record-sets"></a>Odstranění sady záznamů SOA nebo NS

Nelze odstranit hello SOA a NS sady záznamů na vrcholu zóny hello (název = "@"), automaticky vytvoří při vytvoření zóny hello. Jsou automaticky odstraněny při odstranění zóny hello.

## <a name="next-steps"></a>Další kroky

* Další informace o službě Azure DNS najdete v tématu hello [přehled Azure DNS](dns-overview.md).
* Další informace o automatizaci DNS najdete v tématu [vytvoření DNS zóny a sad záznamů pomocí .NET SDK hello](dns-sdk.md).
* Další informace o zpětné záznamy DNS, najdete v části [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).
