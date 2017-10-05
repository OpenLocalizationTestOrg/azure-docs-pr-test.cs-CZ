---
title: "Spravovat sady záznamů DNS a záznamy s Azure DNS | Microsoft Docs"
description: "Azure DNS poskytuje možnost spravovat sady záznamů DNS a záznamy při hostování vaší domény."
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
ms.openlocfilehash: 001b80ccba43beab44f6a598f820df65a85a345f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-record-sets-by-using-the-azure-portal"></a><span data-ttu-id="6dc5b-103">Záznamy DNS spravovat a sady záznamů pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6dc5b-103">Manage DNS records and record sets by using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6dc5b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6dc5b-104">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="6dc5b-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="6dc5b-105">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="6dc5b-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6dc5b-106">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="6dc5b-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6dc5b-107">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="6dc5b-108">Tento článek ukazuje, jak spravovat sady záznamů a záznamů zóny DNS pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-108">This article shows you how to manage record sets and records for your DNS zone by using the Azure portal.</span></span>

<span data-ttu-id="6dc5b-109">Je důležité si uvědomit rozdíl mezi sady záznamů DNS a jednotlivé záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-109">It's important to understand the difference between DNS record sets and individual DNS records.</span></span> <span data-ttu-id="6dc5b-110">Sada záznamů je kolekce záznamů v zóně, které mají stejný název a jsou stejného typu.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-110">A record set is a collection of records in a zone that have the same name and are the same type.</span></span> <span data-ttu-id="6dc5b-111">Další informace najdete v tématu [DNS vytvoření sady záznamů a záznamy pomocí portálu Azure](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6dc5b-111">For more information, see [Create DNS record sets and records by using the Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="create-a-new-record-set-and-record"></a><span data-ttu-id="6dc5b-112">Vytvoření nové sady záznamů a záznamů</span><span class="sxs-lookup"><span data-stu-id="6dc5b-112">Create a new record set and record</span></span>

<span data-ttu-id="6dc5b-113">Vytvoření záznamů na portálu Azure, naleznete v části [záznamy DNS vytvořit pomocí portálu Azure](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6dc5b-113">To create a record set in the Azure portal, see [Create DNS records by using the Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="view-a-record-set"></a><span data-ttu-id="6dc5b-114">Zobrazení sady záznamů</span><span class="sxs-lookup"><span data-stu-id="6dc5b-114">View a record set</span></span>

1. <span data-ttu-id="6dc5b-115">V portálu Azure přejděte do **zónu DNS** okno.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-115">In the Azure portal, go to the **DNS zone** blade.</span></span>
2. <span data-ttu-id="6dc5b-116">Hledání pro sadu záznamů a vyberte jej.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-116">Search for the record set and select it.</span></span> <span data-ttu-id="6dc5b-117">Otevře vlastnosti sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-117">This opens the record set properties.</span></span>

    ![Vyhledejte sady záznamů](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-to-a-record-set"></a><span data-ttu-id="6dc5b-119">Přidejte nový záznam do sady záznamů</span><span class="sxs-lookup"><span data-stu-id="6dc5b-119">Add a new record to a record set</span></span>

<span data-ttu-id="6dc5b-120">Můžete přidat až 20 záznamů pro všechny sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-120">You can add up to 20 records to any record set.</span></span> <span data-ttu-id="6dc5b-121">Sada záznamů nesmí obsahovat dva identické záznamy.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-121">A record set cannot contain two identical records.</span></span> <span data-ttu-id="6dc5b-122">Prázdný sady záznamů (s nulový počet záznamů) mohou být vytvořeny, ale nejsou zobrazeny na názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-122">Empty record sets (with zero records) can be created, but do not appear on the Azure DNS name servers.</span></span> <span data-ttu-id="6dc5b-123">Sady záznamů typu CNAME může obsahovat maximálně jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-123">Record sets of type CNAME can contain one record at most.</span></span>

1. <span data-ttu-id="6dc5b-124">Na **záznam nastavit vlastnosti** pro zónu DNS, klikněte na sadu záznamů, které chcete přidat záznam do.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-124">On the **Record set properties** blade for your DNS zone, click the record set that you want to add a record to.</span></span>

    ![Vyberte sady záznamů](./media/dns-operations-recordsets-portal/selectset500.png)

2. <span data-ttu-id="6dc5b-126">Zadejte, že záznam nastaven vlastnosti vyplnění polí.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-126">Specify the record set properties by filling in the fields.</span></span>

    ![Přidání záznamu](./media/dns-operations-recordsets-portal/addrecord500.png)

3. <span data-ttu-id="6dc5b-128">Klikněte na tlačítko **Uložit** v horní části okna nastavení uložte.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-128">Click **Save** at the top of the blade to save your settings.</span></span> <span data-ttu-id="6dc5b-129">Zavřete okno.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-129">Then close the blade.</span></span>
4. <span data-ttu-id="6dc5b-130">V horním uvidíte, zda je ukládání záznamu.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-130">In the corner, you will see that the record is saving.</span></span>

    ![Ukládání sady záznamů](./media/dns-operations-recordsets-portal/saving150.png)

<span data-ttu-id="6dc5b-132">Záznam po uložení, hodnoty na **zóny DNS** okně se projeví nový záznam.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-132">After the record has been saved, the values on the **DNS zone** blade will reflect the new record.</span></span>

## <a name="update-a-record"></a><span data-ttu-id="6dc5b-133">Aktualizovat záznam</span><span class="sxs-lookup"><span data-stu-id="6dc5b-133">Update a record</span></span>

<span data-ttu-id="6dc5b-134">Při aktualizaci záznamu v existující sady záznamů pole, která můžete aktualizovat závisí na vybraném typu záznamu, kterými pracujete.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-134">When you update a record in an existing record set, the fields you can update depend on the type of record you're working with.</span></span>

1. <span data-ttu-id="6dc5b-135">Na **vlastnosti sady záznamů** okna pro vaše sadu záznamů, vyhledejte záznam.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-135">On the **Record set properties** blade for your record set, search for the record.</span></span>
2. <span data-ttu-id="6dc5b-136">Upravte záznamu.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-136">Modify the record.</span></span> <span data-ttu-id="6dc5b-137">Když upravíte záznam, můžete změnit nastavení k dispozici pro tento záznam.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-137">When you modify a record, you can change the available settings for the record.</span></span> <span data-ttu-id="6dc5b-138">V následujícím příkladu **IP adresu** pole je vybrána, a IP adresu právě upravována.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-138">In the following example, the **IP address** field is selected, and the IP address is in the process of being modified.</span></span>

    ![Upravte záznam](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. <span data-ttu-id="6dc5b-140">Klikněte na tlačítko **Uložit** v horní části okna nastavení uložte.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-140">Click **Save** at the top of the blade to save your settings.</span></span> <span data-ttu-id="6dc5b-141">V pravém horním rohu uvidíte oznámení, které byly uloženy v záznamu.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-141">In the upper right corner, you'll see the notification that the record has been saved.</span></span>

    ![Uložit záznamovou sadu](./media/dns-operations-recordsets-portal/saved150.png)

<span data-ttu-id="6dc5b-143">Po uložení záznamu, nastavte hodnoty pro záznam na **zónu DNS** okně se projeví aktualizovaný záznam.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-143">After the record has been saved, the values for the record set on the **DNS zone** blade will reflect the updated record.</span></span>

## <a name="remove-a-record-from-a-record-set"></a><span data-ttu-id="6dc5b-144">Odebrat záznam ze sady záznamů</span><span class="sxs-lookup"><span data-stu-id="6dc5b-144">Remove a record from a record set</span></span>

<span data-ttu-id="6dc5b-145">Na portálu Azure můžete odebrat záznamy ze sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-145">You can use the Azure portal to remove records from a record set.</span></span> <span data-ttu-id="6dc5b-146">Všimněte si, že odstraněním poslední záznam ze záznamů sady nedojde k odstranění sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-146">Note that removing the last record from a record set does not delete the record set.</span></span>

1. <span data-ttu-id="6dc5b-147">Na **vlastnosti sady záznamů** okna pro vaše sadu záznamů, vyhledejte záznam.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-147">On the **Record set properties** blade for your record set, search for the record.</span></span>
2. <span data-ttu-id="6dc5b-148">Klikněte na záznam, který chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-148">Click the record that you want to remove.</span></span> <span data-ttu-id="6dc5b-149">Potom vyberte **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-149">Then select **Remove**.</span></span>

    ![Odebrat záznam](./media/dns-operations-recordsets-portal/removerecord500.png)

3. <span data-ttu-id="6dc5b-151">Klikněte na tlačítko **Uložit** v horní části okna nastavení uložte.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-151">Click **Save** at the top of the blade to save your settings.</span></span>
4. <span data-ttu-id="6dc5b-152">Po odebrání záznamu, hodnoty pro záznam na **zónu DNS** okně se projeví odebrání.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-152">After the record has been removed, the values for the record on the **DNS zone** blade will reflect the removal.</span></span>

## <span data-ttu-id="6dc5b-153"><a name="delete"></a>Odstranit sadu záznamů</span><span class="sxs-lookup"><span data-stu-id="6dc5b-153"><a name="delete"></a>Delete a record set</span></span>

1. <span data-ttu-id="6dc5b-154">Na **vlastnosti sady záznamů** pro vaše sadu záznamů, klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-154">On the **Record set properties** blade for your record set, click **Delete**.</span></span>

    ![Odstranit sadu záznamů](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. <span data-ttu-id="6dc5b-156">Zobrazí se zpráva s dotazem, pokud chcete odstranit záznamovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-156">A message appears asking if you want to delete the record set.</span></span>
3. <span data-ttu-id="6dc5b-157">Zkontrolujte, zda název odpovídá sady záznamů, který chcete odstranit a potom klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-157">Verify that the name matches the record set that you want to delete, and then click **Yes**.</span></span>
4. <span data-ttu-id="6dc5b-158">Na **zóny DNS** okno, ověřte, zda sada záznamů je již nebudou viditelné.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-158">On the **DNS zone** blade, verify that the record set is no longer visible.</span></span>

## <a name="work-with-ns-and-soa-records"></a><span data-ttu-id="6dc5b-159">Práce se záznamy NS a SOA</span><span class="sxs-lookup"><span data-stu-id="6dc5b-159">Work with NS and SOA records</span></span>

<span data-ttu-id="6dc5b-160">Záznamy NS a SOA, které se automaticky vytvoří spravuje odlišně od ostatních typů záznamu.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-160">NS and SOA records that are automatically created are managed differently from other record types.</span></span>

### <a name="modify-soa-records"></a><span data-ttu-id="6dc5b-161">Upravit záznamy SOA</span><span class="sxs-lookup"><span data-stu-id="6dc5b-161">Modify SOA records</span></span>

<span data-ttu-id="6dc5b-162">Nelze přidat nebo odebrat záznamy z automaticky vytvořený záznam SOA nastavit ve vrcholu zóny (název = "@").</span><span class="sxs-lookup"><span data-stu-id="6dc5b-162">You cannot add or remove records from the automatically created SOA record set at the zone apex (name = "@").</span></span> <span data-ttu-id="6dc5b-163">Ale můžete upravit některý z parametrů v rámci záznamu SOA (s výjimkou "hostitel") a hodnota TTL sady záznamu.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-163">However, you can modify any of the parameters within the SOA record (except "Host") and the record set TTL.</span></span>

### <a name="modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="6dc5b-164">Upravit NS záznamů ve vrcholu zóny</span><span class="sxs-lookup"><span data-stu-id="6dc5b-164">Modify NS records at the zone apex</span></span>

<span data-ttu-id="6dc5b-165">S každou zónou DNS se automaticky vytvoří záznam NS nastavit ve vrcholu zóny.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-165">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="6dc5b-166">Obsahuje názvy názvových serverů Azure DNS přiřadit do zóny.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-166">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="6dc5b-167">Můžete přidat další název nastavit servery na tento záznam NS, podporuje společné hosting domén s více než jednoho poskytovatele DNS.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-167">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="6dc5b-168">Můžete také upravit TTL a metadat pro tuto sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-168">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="6dc5b-169">Však nelze odebrat ani změnit předem vyplněná názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-169">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="6dc5b-170">Všimněte si, že vztahuje se pouze na vrcholu zóny sady záznamů NS.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-170">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="6dc5b-171">Jiné sady záznamů NS v pásmu (jak je používá delegovat podřízených zónách) můžete změnit bez omezení.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-171">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

### <a name="delete-soa-or-ns-record-sets"></a><span data-ttu-id="6dc5b-172">Odstranění sady záznamů SOA nebo NS</span><span class="sxs-lookup"><span data-stu-id="6dc5b-172">Delete SOA or NS record sets</span></span>

<span data-ttu-id="6dc5b-173">Nelze odstranit, SOA a sady záznamů NS ve vrcholu zóny (název = "@"), jsou vytvořeny automaticky při vytváření zóny.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-173">You cannot delete the SOA and NS record sets at the zone apex (name = "@") that are created automatically when the zone is created.</span></span> <span data-ttu-id="6dc5b-174">Jsou automaticky odstraněny při odstranění zóny.</span><span class="sxs-lookup"><span data-stu-id="6dc5b-174">They are deleted automatically when you delete the zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6dc5b-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6dc5b-175">Next steps</span></span>

* <span data-ttu-id="6dc5b-176">Další informace o službě Azure DNS najdete v tématu [přehled Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6dc5b-176">For more information about Azure DNS, see the [Azure DNS overview](dns-overview.md).</span></span>
* <span data-ttu-id="6dc5b-177">Další informace o automatizaci DNS najdete v tématu [vytvoření DNS zóny a sad záznamů pomocí sady .NET SDK](dns-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="6dc5b-177">For more information about automating DNS, see [Creating DNS zones and record sets using the .NET SDK](dns-sdk.md).</span></span>
* <span data-ttu-id="6dc5b-178">Další informace o zpětné záznamy DNS, najdete v části [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6dc5b-178">For more information about reverse DNS records, see [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>
