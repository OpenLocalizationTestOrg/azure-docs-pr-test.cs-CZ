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
# <a name="manage-dns-records-and-record-sets-by-using-hello-azure-portal"></a><span data-ttu-id="59dd2-103">Spravovat záznamy a sadami záznamů DNS pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="59dd2-103">Manage DNS records and record sets by using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="59dd2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="59dd2-104">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="59dd2-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="59dd2-105">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="59dd2-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="59dd2-106">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="59dd2-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="59dd2-107">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="59dd2-108">Tento článek ukazuje, jak hello toomanage sady záznamů a záznamů zóny DNS pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="59dd2-108">This article shows you how toomanage record sets and records for your DNS zone by using hello Azure portal.</span></span>

<span data-ttu-id="59dd2-109">Je důležité toounderstand hello rozdíl mezi sady záznamů DNS a jednotlivé záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="59dd2-109">It's important toounderstand hello difference between DNS record sets and individual DNS records.</span></span> <span data-ttu-id="59dd2-110">Sada záznamů je kolekce záznamů v zóně, které mají hello stejný název a jsou hello stejný typ.</span><span class="sxs-lookup"><span data-stu-id="59dd2-110">A record set is a collection of records in a zone that have hello same name and are hello same type.</span></span> <span data-ttu-id="59dd2-111">Další informace najdete v tématu [DNS vytvoření sady záznamů a záznamy pomocí portálu Azure hello](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="59dd2-111">For more information, see [Create DNS record sets and records by using hello Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="create-a-new-record-set-and-record"></a><span data-ttu-id="59dd2-112">Vytvoření nové sady záznamů a záznamů</span><span class="sxs-lookup"><span data-stu-id="59dd2-112">Create a new record set and record</span></span>

<span data-ttu-id="59dd2-113">toocreate záznamů v hello portál Azure, najdete v části [hello záznamy DNS vytvořit pomocí portálu Azure](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="59dd2-113">toocreate a record set in hello Azure portal, see [Create DNS records by using hello Azure portal](dns-getstarted-create-recordset-portal.md).</span></span>

## <a name="view-a-record-set"></a><span data-ttu-id="59dd2-114">Zobrazení sady záznamů</span><span class="sxs-lookup"><span data-stu-id="59dd2-114">View a record set</span></span>

1. <span data-ttu-id="59dd2-115">V hello portálu Azure, přejděte toohello **zónu DNS** okno.</span><span class="sxs-lookup"><span data-stu-id="59dd2-115">In hello Azure portal, go toohello **DNS zone** blade.</span></span>
2. <span data-ttu-id="59dd2-116">Hledání pro sadu záznamů hello a vyberte jej.</span><span class="sxs-lookup"><span data-stu-id="59dd2-116">Search for hello record set and select it.</span></span> <span data-ttu-id="59dd2-117">Otevře vlastnosti hello sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="59dd2-117">This opens hello record set properties.</span></span>

    ![Vyhledejte sady záznamů](./media/dns-operations-recordsets-portal/searchset500.png)

## <a name="add-a-new-record-tooa-record-set"></a><span data-ttu-id="59dd2-119">Přidat novou sadu záznamů tooa záznamů</span><span class="sxs-lookup"><span data-stu-id="59dd2-119">Add a new record tooa record set</span></span>

<span data-ttu-id="59dd2-120">Můžete přidat do sady záznamů tooany too20 záznamy.</span><span class="sxs-lookup"><span data-stu-id="59dd2-120">You can add up too20 records tooany record set.</span></span> <span data-ttu-id="59dd2-121">Sada záznamů nesmí obsahovat dva identické záznamy.</span><span class="sxs-lookup"><span data-stu-id="59dd2-121">A record set cannot contain two identical records.</span></span> <span data-ttu-id="59dd2-122">Prázdný sady záznamů (s nulový počet záznamů) mohou být vytvořeny, ale nejsou zobrazeny na názvových serverů Azure DNS hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-122">Empty record sets (with zero records) can be created, but do not appear on hello Azure DNS name servers.</span></span> <span data-ttu-id="59dd2-123">Sady záznamů typu CNAME může obsahovat maximálně jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="59dd2-123">Record sets of type CNAME can contain one record at most.</span></span>

1. <span data-ttu-id="59dd2-124">Na hello **vlastnosti sady záznamů** okno pro vaši zónu DNS, klikněte na tlačítko záznam hello nastavit, že chcete tooadd záznam o.</span><span class="sxs-lookup"><span data-stu-id="59dd2-124">On hello **Record set properties** blade for your DNS zone, click hello record set that you want tooadd a record to.</span></span>

    ![Vyberte sady záznamů](./media/dns-operations-recordsets-portal/selectset500.png)

2. <span data-ttu-id="59dd2-126">Zadejte vlastnosti sady záznamů hello vyplněním hello polí.</span><span class="sxs-lookup"><span data-stu-id="59dd2-126">Specify hello record set properties by filling in hello fields.</span></span>

    ![Přidání záznamu](./media/dns-operations-recordsets-portal/addrecord500.png)

3. <span data-ttu-id="59dd2-128">Klikněte na tlačítko **Uložit** v hello horní části okna toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="59dd2-128">Click **Save** at hello top of hello blade toosave your settings.</span></span> <span data-ttu-id="59dd2-129">Zavřete okno hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-129">Then close hello blade.</span></span>
4. <span data-ttu-id="59dd2-130">V horním hello uvidíte, zda je ukládání hello záznamu.</span><span class="sxs-lookup"><span data-stu-id="59dd2-130">In hello corner, you will see that hello record is saving.</span></span>

    ![Ukládání sady záznamů](./media/dns-operations-recordsets-portal/saving150.png)

<span data-ttu-id="59dd2-132">Po uložení hello záznamu, hello hodnot hello **zónu DNS** okně se projeví hello nový záznam.</span><span class="sxs-lookup"><span data-stu-id="59dd2-132">After hello record has been saved, hello values on hello **DNS zone** blade will reflect hello new record.</span></span>

## <a name="update-a-record"></a><span data-ttu-id="59dd2-133">Aktualizovat záznam</span><span class="sxs-lookup"><span data-stu-id="59dd2-133">Update a record</span></span>

<span data-ttu-id="59dd2-134">Při aktualizaci záznamu v existující sady záznamů hello pole, která můžete aktualizovat závisí na hello typu záznamu, kterými pracujete.</span><span class="sxs-lookup"><span data-stu-id="59dd2-134">When you update a record in an existing record set, hello fields you can update depend on hello type of record you're working with.</span></span>

1. <span data-ttu-id="59dd2-135">Na hello **vlastnosti sady záznamů** okna pro vaše sadu záznamů, vyhledejte záznam hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-135">On hello **Record set properties** blade for your record set, search for hello record.</span></span>
2. <span data-ttu-id="59dd2-136">Upravte záznamu hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-136">Modify hello record.</span></span> <span data-ttu-id="59dd2-137">Když upravíte záznam, můžete změnit hello dostupná nastavení pro záznam hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-137">When you modify a record, you can change hello available settings for hello record.</span></span> <span data-ttu-id="59dd2-138">V následujícím příkladu hello, hello **IP adresu** vybrat pole a hello IP adresa je v procesu hello upravována.</span><span class="sxs-lookup"><span data-stu-id="59dd2-138">In hello following example, hello **IP address** field is selected, and hello IP address is in hello process of being modified.</span></span>

    ![Upravte záznam](./media/dns-operations-recordsets-portal/modifyrecord500.png)

3. <span data-ttu-id="59dd2-140">Klikněte na tlačítko **Uložit** v hello horní části okna toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="59dd2-140">Click **Save** at hello top of hello blade toosave your settings.</span></span> <span data-ttu-id="59dd2-141">V horním pravém rohu hello uvidíte hello oznámení, že záznam hello se uložila.</span><span class="sxs-lookup"><span data-stu-id="59dd2-141">In hello upper right corner, you'll see hello notification that hello record has been saved.</span></span>

    ![Uložit záznamovou sadu](./media/dns-operations-recordsets-portal/saved150.png)

<span data-ttu-id="59dd2-143">Po uložení hello záznamu, hello hodnoty pro záznam hello nastavené na hello **zónu DNS** okně se projeví hello aktualizovat záznam.</span><span class="sxs-lookup"><span data-stu-id="59dd2-143">After hello record has been saved, hello values for hello record set on hello **DNS zone** blade will reflect hello updated record.</span></span>

## <a name="remove-a-record-from-a-record-set"></a><span data-ttu-id="59dd2-144">Odebrat záznam ze sady záznamů</span><span class="sxs-lookup"><span data-stu-id="59dd2-144">Remove a record from a record set</span></span>

<span data-ttu-id="59dd2-145">Můžete použít hello Azure portálu tooremove záznamy ze sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="59dd2-145">You can use hello Azure portal tooremove records from a record set.</span></span> <span data-ttu-id="59dd2-146">Všimněte si, že odstraněním hello poslední záznam ze záznamů sady neodstraní hello sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="59dd2-146">Note that removing hello last record from a record set does not delete hello record set.</span></span>

1. <span data-ttu-id="59dd2-147">Na hello **vlastnosti sady záznamů** okna pro vaše sadu záznamů, vyhledejte záznam hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-147">On hello **Record set properties** blade for your record set, search for hello record.</span></span>
2. <span data-ttu-id="59dd2-148">Klikněte na tlačítko, které chcete tooremove záznam hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-148">Click hello record that you want tooremove.</span></span> <span data-ttu-id="59dd2-149">Potom vyberte **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="59dd2-149">Then select **Remove**.</span></span>

    ![Odebrat záznam](./media/dns-operations-recordsets-portal/removerecord500.png)

3. <span data-ttu-id="59dd2-151">Klikněte na tlačítko **Uložit** v hello horní části okna toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="59dd2-151">Click **Save** at hello top of hello blade toosave your settings.</span></span>
4. <span data-ttu-id="59dd2-152">Po odebrání záznamu hello hello hodnoty pro záznam hello na hello **zónu DNS** okně se projeví odebrání hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-152">After hello record has been removed, hello values for hello record on hello **DNS zone** blade will reflect hello removal.</span></span>

## <span data-ttu-id="59dd2-153"><a name="delete"></a>Odstranit sadu záznamů</span><span class="sxs-lookup"><span data-stu-id="59dd2-153"><a name="delete"></a>Delete a record set</span></span>

1. <span data-ttu-id="59dd2-154">Na hello **vlastnosti sady záznamů** pro vaše sadu záznamů, klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="59dd2-154">On hello **Record set properties** blade for your record set, click **Delete**.</span></span>

    ![Odstranit sadu záznamů](./media/dns-operations-recordsets-portal/deleterecordset500.png)

2. <span data-ttu-id="59dd2-156">Zobrazí dotaz, pokud chcete, aby sada záznamů toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-156">A message appears asking if you want toodelete hello record set.</span></span>
3. <span data-ttu-id="59dd2-157">Ověřte, zda text hello název odpovídá hello záznam nastavena má toodelete a pak klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="59dd2-157">Verify that hello name matches hello record set that you want toodelete, and then click **Yes**.</span></span>
4. <span data-ttu-id="59dd2-158">Na hello **zónu DNS** okno, ověřte, zda sada záznamů hello je už nebude viditelné.</span><span class="sxs-lookup"><span data-stu-id="59dd2-158">On hello **DNS zone** blade, verify that hello record set is no longer visible.</span></span>

## <a name="work-with-ns-and-soa-records"></a><span data-ttu-id="59dd2-159">Práce se záznamy NS a SOA</span><span class="sxs-lookup"><span data-stu-id="59dd2-159">Work with NS and SOA records</span></span>

<span data-ttu-id="59dd2-160">Záznamy NS a SOA, které se automaticky vytvoří spravuje odlišně od ostatních typů záznamu.</span><span class="sxs-lookup"><span data-stu-id="59dd2-160">NS and SOA records that are automatically created are managed differently from other record types.</span></span>

### <a name="modify-soa-records"></a><span data-ttu-id="59dd2-161">Upravit záznamy SOA</span><span class="sxs-lookup"><span data-stu-id="59dd2-161">Modify SOA records</span></span>

<span data-ttu-id="59dd2-162">Nelze přidat nebo odebrat záznamy z hello automaticky vytvořil záznam SOA nastavit na vrcholu zóny hello (název = "@").</span><span class="sxs-lookup"><span data-stu-id="59dd2-162">You cannot add or remove records from hello automatically created SOA record set at hello zone apex (name = "@").</span></span> <span data-ttu-id="59dd2-163">Ale můžete upravit některý z parametrů hello v rámci hello záznam SOA (s výjimkou "hostitel") a záznam hello nastavit hodnotu TTL.</span><span class="sxs-lookup"><span data-stu-id="59dd2-163">However, you can modify any of hello parameters within hello SOA record (except "Host") and hello record set TTL.</span></span>

### <a name="modify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="59dd2-164">Upravit NS záznamy na vrcholu zóny hello</span><span class="sxs-lookup"><span data-stu-id="59dd2-164">Modify NS records at hello zone apex</span></span>

<span data-ttu-id="59dd2-165">Nastavte na vrcholu zóny hello záznam NS Hello se vytvoří automaticky s každou zónou DNS.</span><span class="sxs-lookup"><span data-stu-id="59dd2-165">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="59dd2-166">Obsahuje názvy hello hello Azure DNS název servery přiřazené toohello zóny.</span><span class="sxs-lookup"><span data-stu-id="59dd2-166">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="59dd2-167">Můžete přidat další název servery toothis NS sady záznamů, toosupport společně s více než jednoho poskytovatele DNS hostování domény.</span><span class="sxs-lookup"><span data-stu-id="59dd2-167">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="59dd2-168">Můžete také upravit hello TTL a metadat pro tuto sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="59dd2-168">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="59dd2-169">Však nelze odebrat ani změnit hello předem vyplněná názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="59dd2-169">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="59dd2-170">Všimněte si, že platí pouze toohello NS sady záznamů na vrcholu zóny hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-170">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="59dd2-171">Další NS sady záznamů ve vaší zóně (jako podřízené zóny použité toodelegate) můžete změnit bez omezení.</span><span class="sxs-lookup"><span data-stu-id="59dd2-171">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

### <a name="delete-soa-or-ns-record-sets"></a><span data-ttu-id="59dd2-172">Odstranění sady záznamů SOA nebo NS</span><span class="sxs-lookup"><span data-stu-id="59dd2-172">Delete SOA or NS record sets</span></span>

<span data-ttu-id="59dd2-173">Nelze odstranit hello SOA a NS sady záznamů na vrcholu zóny hello (název = "@"), automaticky vytvoří při vytvoření zóny hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-173">You cannot delete hello SOA and NS record sets at hello zone apex (name = "@") that are created automatically when hello zone is created.</span></span> <span data-ttu-id="59dd2-174">Jsou automaticky odstraněny při odstranění zóny hello.</span><span class="sxs-lookup"><span data-stu-id="59dd2-174">They are deleted automatically when you delete hello zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59dd2-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="59dd2-175">Next steps</span></span>

* <span data-ttu-id="59dd2-176">Další informace o službě Azure DNS najdete v tématu hello [přehled Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59dd2-176">For more information about Azure DNS, see hello [Azure DNS overview](dns-overview.md).</span></span>
* <span data-ttu-id="59dd2-177">Další informace o automatizaci DNS najdete v tématu [vytvoření DNS zóny a sad záznamů pomocí .NET SDK hello](dns-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="59dd2-177">For more information about automating DNS, see [Creating DNS zones and record sets using hello .NET SDK](dns-sdk.md).</span></span>
* <span data-ttu-id="59dd2-178">Další informace o zpětné záznamy DNS, najdete v části [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59dd2-178">For more information about reverse DNS records, see [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>
