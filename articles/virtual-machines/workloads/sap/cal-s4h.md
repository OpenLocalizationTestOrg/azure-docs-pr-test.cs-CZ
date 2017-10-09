---
title: "aaaDeploy SAP S nebo 4HANA nebo BW/4HANA ve virtuálním počítači Azure | Microsoft Docs"
description: "Nasazení SAP S nebo 4HANA nebo BW/4HANA ve virtuálním počítači Azure"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 7e57f7daa7667b7c7dbcb86f6892a54e4295b74c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a><span data-ttu-id="454f0-103">Nasazení SAP S nebo 4HANA nebo BW/4HANA v Azure</span><span class="sxs-lookup"><span data-stu-id="454f0-103">Deploy SAP S/4HANA or BW/4HANA on Azure</span></span>
<span data-ttu-id="454f0-104">Tento článek popisuje, jak toodeploy S nebo 4HANA v Azure pomocí hello SAP cloudu zařízení knihoven (SAP CAL) 3.0.</span><span class="sxs-lookup"><span data-stu-id="454f0-104">This article describes how toodeploy S/4HANA on Azure by using hello SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="454f0-105">toodeploy jiných řešení na základě SAP HANA, jako je například BW/4HANA, postupujte podle hello stejný postup.</span><span class="sxs-lookup"><span data-stu-id="454f0-105">toodeploy other SAP HANA-based solutions, such as BW/4HANA, follow hello same steps.</span></span>

> [!NOTE]
<span data-ttu-id="454f0-106">Další informace o hello SAP CAL, přejděte toohello [knihovny zařízení cloudu SAP](https://cal.sap.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="454f0-106">For more information about hello SAP CAL, go toohello [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="454f0-107">SAP má také blog o hello [SAP cloudu zařízení knihovna 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="454f0-107">SAP also has a blog about hello [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span>

> [!NOTE]
<span data-ttu-id="454f0-108">Jako 29. květen 2017, můžete pomocí modelu nasazení Azure Resource Manager hello kromě model nasazení classic méně než upřednostňovaný toohello toodeploy hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-108">As of May 29, 2017, you can use hello Azure Resource Manager deployment model in addition toohello less-preferred classic deployment model toodeploy hello SAP CAL.</span></span> <span data-ttu-id="454f0-109">Doporučujeme použít hello nového modelu nasazení Resource Manager a modelu nasazení classic hello ignorovat.</span><span class="sxs-lookup"><span data-stu-id="454f0-109">We recommend that you use hello new Resource Manager deployment model and disregard hello classic deployment model.</span></span>

## <a name="step-by-step-process-toodeploy-hello-solution"></a><span data-ttu-id="454f0-110">Podrobný postup toodeploy hello řešení</span><span class="sxs-lookup"><span data-stu-id="454f0-110">Step-by-step process toodeploy hello solution</span></span>

<span data-ttu-id="454f0-111">Hello následující pořadí snímky obrazovky ukazuje, jak toodeploy S nebo 4HANA v Azure pomocí hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-111">hello following sequence of screenshots shows you how toodeploy S/4HANA on Azure by using hello SAP CAL.</span></span> <span data-ttu-id="454f0-112">proces Hello funguje hello stejným způsobem jako u jiných řešení, jako je například BW/4HANA.</span><span class="sxs-lookup"><span data-stu-id="454f0-112">hello process works hello same way for other solutions, such as BW/4HANA.</span></span>

<span data-ttu-id="454f0-113">Hello **řešení** stránky jsou uvedeny některé hello řešení založená na SAP CAL HANA dostupné v Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-113">hello **Solutions** page shows some of hello SAP CAL HANA-based solutions available on Azure.</span></span> <span data-ttu-id="454f0-114">**SAP S nebo 4HANA 1610 FPS01, Fully-Activated zařízení** se v prostředním řádku hello:</span><span class="sxs-lookup"><span data-stu-id="454f0-114">**SAP S/4HANA 1610 FPS01, Fully-Activated Appliance** is in hello middle row:</span></span>

![Řešení CAL SAP](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-hello-sap-cal"></a><span data-ttu-id="454f0-116">Vytvoření účtu na hello SAP CAL</span><span class="sxs-lookup"><span data-stu-id="454f0-116">Create an account in hello SAP CAL</span></span>
1. <span data-ttu-id="454f0-117">toosign v toohello SAP CAL pro hello poprvé, použijte S SAP-uživatele nebo jiný uživatel zaregistrována SAP.</span><span class="sxs-lookup"><span data-stu-id="454f0-117">toosign in toohello SAP CAL for hello first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="454f0-118">Poté definujte SAP CAL účtu, který je používán hello SAP CAL toodeploy zařízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-118">Then define an SAP CAL account that is used by hello SAP CAL toodeploy appliances on Azure.</span></span> <span data-ttu-id="454f0-119">V definici hello účet budete muset:</span><span class="sxs-lookup"><span data-stu-id="454f0-119">In hello account definition, you need to:</span></span>

    <span data-ttu-id="454f0-120">a.</span><span class="sxs-lookup"><span data-stu-id="454f0-120">a.</span></span> <span data-ttu-id="454f0-121">Vyberte model nasazení hello v Azure (Resource Manager nebo classic).</span><span class="sxs-lookup"><span data-stu-id="454f0-121">Select hello deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="454f0-122">b.</span><span class="sxs-lookup"><span data-stu-id="454f0-122">b.</span></span> <span data-ttu-id="454f0-123">Zadejte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-123">Enter your Azure subscription.</span></span> <span data-ttu-id="454f0-124">Účet SAP CAL lze přiřadit pouze tooone předplatné.</span><span class="sxs-lookup"><span data-stu-id="454f0-124">An SAP CAL account can be assigned tooone subscription only.</span></span> <span data-ttu-id="454f0-125">Pokud potřebujete více než jedno předplatné, je třeba toocreate jiný účet SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-125">If you need more than one subscription, you need toocreate another SAP CAL account.</span></span>

    <span data-ttu-id="454f0-126">c.</span><span class="sxs-lookup"><span data-stu-id="454f0-126">c.</span></span> <span data-ttu-id="454f0-127">Dejte hello SAP CAL oprávnění toodeploy do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-127">Give hello SAP CAL permission toodeploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="454f0-128">Hello následující kroky ukazují, jak toocreate je SAP CAL účet pro nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="454f0-128">hello next steps show how toocreate an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="454f0-129">Pokud již máte účet SAP CAL, který je propojené toohello modelu nasazení classic, můžete *potřebovat* toofollow tyto kroky toocreate nový účet SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-129">If you already have an SAP CAL account that is linked toohello classic deployment model, you *need* toofollow these steps toocreate a new SAP CAL account.</span></span> <span data-ttu-id="454f0-130">nový účet SAP CAL Hello musí toodeploy v modelu Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="454f0-130">hello new SAP CAL account needs toodeploy in hello Resource Manager model.</span></span>

2. <span data-ttu-id="454f0-131">Vytvořte nový účet SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-131">Create a new SAP CAL account.</span></span> <span data-ttu-id="454f0-132">Hello **účty** stránka zobrazuje tři možnosti pro Azure:</span><span class="sxs-lookup"><span data-stu-id="454f0-132">hello **Accounts** page shows three choices for Azure:</span></span> 

    <span data-ttu-id="454f0-133">a.</span><span class="sxs-lookup"><span data-stu-id="454f0-133">a.</span></span> <span data-ttu-id="454f0-134">**Microsoft Azure (klasický)** je hello modelu nasazení classic a již není upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="454f0-134">**Microsoft Azure (classic)** is hello classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="454f0-135">b.</span><span class="sxs-lookup"><span data-stu-id="454f0-135">b.</span></span> <span data-ttu-id="454f0-136">**Microsoft Azure** je hello nového modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="454f0-136">**Microsoft Azure** is hello new Resource Manager deployment model.</span></span>

    <span data-ttu-id="454f0-137">c.</span><span class="sxs-lookup"><span data-stu-id="454f0-137">c.</span></span> <span data-ttu-id="454f0-138">**Windows Azure je provozována společností 21Vianet** je možnost v Číně, který používá model nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="454f0-138">**Windows Azure operated by 21Vianet** is an option in China that uses hello classic deployment model.</span></span>

    <span data-ttu-id="454f0-139">Vyberte toodeploy v modelu Resource Manager hello **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="454f0-139">toodeploy in hello Resource Manager model, select **Microsoft Azure**.</span></span>

    ![Podrobnosti účtu CAL SAP](./media/cal-s4h/s4h-pic-2a.png)

3. <span data-ttu-id="454f0-141">Zadejte hello Azure **ID předplatného** , naleznete na portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="454f0-141">Enter hello Azure **Subscription ID** that can be found on hello Azure portal.</span></span>

   ![Účty CAL SAP](./media/cal-s4h/s4h-pic3c.png)

4. <span data-ttu-id="454f0-143">definované tooauthorize hello SAP CAL toodeploy do hello předplatné Azure, klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="454f0-143">tooauthorize hello SAP CAL toodeploy into hello Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="454f0-144">hello záložce prohlížeče se zobrazí následující stránka Hello:</span><span class="sxs-lookup"><span data-stu-id="454f0-144">hello following page appears in hello browser tab:</span></span>

   ![Internet Explorer cloudových služeb přihlášení](./media/cal-s4h/s4h-pic4c.png)

5. <span data-ttu-id="454f0-146">Pokud je uveden více než jeden uživatel, zvolte účet Microsoft hello, který je propojený toobe hello spolusprávce hello předplatné Azure, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="454f0-146">If more than one user is listed, choose hello Microsoft account that is linked toobe hello coadministrator of hello Azure subscription you selected.</span></span> <span data-ttu-id="454f0-147">hello záložce prohlížeče se zobrazí následující stránka Hello:</span><span class="sxs-lookup"><span data-stu-id="454f0-147">hello following page appears in hello browser tab:</span></span>

   ![Internet Explorer cloudové služby potvrzení](./media/cal-s4h/s4h-pic5a.png)

6. <span data-ttu-id="454f0-149">Klikněte na tlačítko **přijmout**.</span><span class="sxs-lookup"><span data-stu-id="454f0-149">Click **Accept**.</span></span> <span data-ttu-id="454f0-150">Pokud autorizace hello je úspěšné, hello Definice SAP CAL účtu se zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="454f0-150">If hello authorization is successful, hello SAP CAL account definition displays again.</span></span> <span data-ttu-id="454f0-151">Po krátkou dobu zprávu potvrdí, že proces autorizace hello bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="454f0-151">After a short time, a message confirms that hello authorization process was successful.</span></span>

7. <span data-ttu-id="454f0-152">tooassign hello nově vytvořený uživatel tooyour účet SAP CAL, zadejte vaše **ID uživatele** v hello textového pole na hello správné a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="454f0-152">tooassign hello newly created SAP CAL account tooyour user, enter your **User ID** in hello text box on hello right and click **Add**.</span></span>

   ![Přidružení toouser účtu](./media/cal-s4h/s4h-pic8a.png)

8. <span data-ttu-id="454f0-154">Klikněte na tlačítko tooassociate vašeho účtu pomocí hello uživatele, že používáte toosign toohello SAP CAL, **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="454f0-154">tooassociate your account with hello user that you use toosign in toohello SAP CAL, click **Review**.</span></span> 
 
9. <span data-ttu-id="454f0-155">Klikněte na tlačítko toocreate hello přidružení mezi uživateli a hello nově vytvořený účet SAP CAL, **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="454f0-155">toocreate hello association between your user and hello newly created SAP CAL account, click **Create**.</span></span>

   ![Přidružení účtu uživatele tooSAP Kalendáře](./media/cal-s4h/s4h-pic9b.png)

<span data-ttu-id="454f0-157">Úspěšně jste vytvořili účet SAP CAL, aby bylo možné:</span><span class="sxs-lookup"><span data-stu-id="454f0-157">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="454f0-158">Pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="454f0-158">Use hello Resource Manager deployment model.</span></span>
- <span data-ttu-id="454f0-159">Nasazení systémů SAP do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-159">Deploy SAP systems into your Azure subscription.</span></span>

<span data-ttu-id="454f0-160">Nyní můžete začít S nebo 4HANA toodeploy do předplatného uživatele v Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-160">Now you can start toodeploy S/4HANA into your user subscription in Azure.</span></span>

> [!NOTE]
<span data-ttu-id="454f0-161">Než budete pokračovat, zjistit, zda máte Azure základní kvóty pro virtuální počítače Azure H-Series.</span><span class="sxs-lookup"><span data-stu-id="454f0-161">Before you continue, determine whether you have Azure core quotas for Azure H-Series VMs.</span></span> <span data-ttu-id="454f0-162">V okamžiku hello hello SAP CAL používá toodeploy H-Series virtuální počítače Azure některé řešení založená na SAP HANA hello.</span><span class="sxs-lookup"><span data-stu-id="454f0-162">At hello moment, hello SAP CAL uses H-Series VMs of Azure toodeploy some of hello SAP HANA-based solutions.</span></span> <span data-ttu-id="454f0-163">Vaše předplatné Azure nemusí mít žádné H-Series základní kvóty pro H-Series.</span><span class="sxs-lookup"><span data-stu-id="454f0-163">Your Azure subscription might not have any H-Series core quotas for H-Series.</span></span> <span data-ttu-id="454f0-164">Pokud ano, bude pravděpodobně nutné toocontact podporu Azure tooget kvótu alespoň 16 jader H-Series.</span><span class="sxs-lookup"><span data-stu-id="454f0-164">If so, you might need toocontact Azure support tooget a quota of at least 16 H-Series cores.</span></span>

> [!NOTE]
<span data-ttu-id="454f0-165">Když nasadíte řešení v Azure v hello SAP CAL, můžete zjistit, které můžete zvolit pouze jedna oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-165">When you deploy a solution on Azure in hello SAP CAL, you might find that you can choose only one Azure region.</span></span> <span data-ttu-id="454f0-166">toodeploy do Azure oblastí, než hello jeden navrhovaná službou hello SAP CAL, musíte toopurchase odběru Kalendáře ze SAP.</span><span class="sxs-lookup"><span data-stu-id="454f0-166">toodeploy into Azure regions other than hello one suggested by hello SAP CAL, you need toopurchase a CAL subscription from SAP.</span></span> <span data-ttu-id="454f0-167">Také může musíte tooopen zprávu s SAP toohave vašeho účtu povoleno toodeliver CAL do Azure oblastí, než ty, které jsou původně navržený hello.</span><span class="sxs-lookup"><span data-stu-id="454f0-167">You also might need tooopen a message with SAP toohave your CAL account enabled toodeliver into Azure regions other than hello ones initially suggested.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="454f0-168">Nasazení řešení</span><span class="sxs-lookup"><span data-stu-id="454f0-168">Deploy a solution</span></span>

<span data-ttu-id="454f0-169">Umožňuje nasadit řešení z hello **řešení** stránku hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-169">Let's deploy a solution from hello **Solutions** page of hello SAP CAL.</span></span> <span data-ttu-id="454f0-170">Hello SAP CAL má dva toodeploy pořadí:</span><span class="sxs-lookup"><span data-stu-id="454f0-170">hello SAP CAL has two sequences toodeploy:</span></span>

- <span data-ttu-id="454f0-171">Základní pořadí, které používá jednu stránku toodefine hello systému toobe nasazení</span><span class="sxs-lookup"><span data-stu-id="454f0-171">A basic sequence that uses one page toodefine hello system toobe deployed</span></span>
- <span data-ttu-id="454f0-172">Pokročilé pořadí, který vám dává některé možnosti na velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="454f0-172">An advanced sequence that gives you certain choices on VM sizes</span></span> 

<span data-ttu-id="454f0-173">Ukážeme hello základní cesta toodeployment sem.</span><span class="sxs-lookup"><span data-stu-id="454f0-173">We demonstrate hello basic path toodeployment here.</span></span>

1. <span data-ttu-id="454f0-174">Na hello **Podrobnosti účtu** stránky, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="454f0-174">On hello **Account Details** page, you need to:</span></span>

    <span data-ttu-id="454f0-175">a.</span><span class="sxs-lookup"><span data-stu-id="454f0-175">a.</span></span> <span data-ttu-id="454f0-176">Vyberte účet SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-176">Select an SAP CAL account.</span></span> <span data-ttu-id="454f0-177">(Použít účet, který je přidružený toodeploy pomocí modelu nasazení Resource Manager hello.)</span><span class="sxs-lookup"><span data-stu-id="454f0-177">(Use an account that is associated toodeploy with hello Resource Manager deployment model.)</span></span>

    <span data-ttu-id="454f0-178">b.</span><span class="sxs-lookup"><span data-stu-id="454f0-178">b.</span></span> <span data-ttu-id="454f0-179">Zadejte instance **název**.</span><span class="sxs-lookup"><span data-stu-id="454f0-179">Enter an instance **Name**.</span></span>

    <span data-ttu-id="454f0-180">c.</span><span class="sxs-lookup"><span data-stu-id="454f0-180">c.</span></span> <span data-ttu-id="454f0-181">Vyberte Azure **oblast**.</span><span class="sxs-lookup"><span data-stu-id="454f0-181">Select an Azure **Region**.</span></span> <span data-ttu-id="454f0-182">Hello SAP CAL navrhuje oblast.</span><span class="sxs-lookup"><span data-stu-id="454f0-182">hello SAP CAL suggests a region.</span></span> <span data-ttu-id="454f0-183">Pokud potřebujete jinou oblast Azure a nemáte předplatné SAP CAL, potřebujete předplatné tooorder licence CAL s SAP.</span><span class="sxs-lookup"><span data-stu-id="454f0-183">If you need another Azure region and you don't have an SAP CAL subscription, you need tooorder a CAL subscription with SAP.</span></span>

    <span data-ttu-id="454f0-184">d.</span><span class="sxs-lookup"><span data-stu-id="454f0-184">d.</span></span> <span data-ttu-id="454f0-185">Zadejte hlavní **heslo** pro řešení hello osm nebo devět znaků.</span><span class="sxs-lookup"><span data-stu-id="454f0-185">Enter a master **Password** for hello solution of eight or nine characters.</span></span> <span data-ttu-id="454f0-186">Hello heslo se používá pro hello správci hello různé součásti.</span><span class="sxs-lookup"><span data-stu-id="454f0-186">hello password is used for hello administrators of hello different components.</span></span>

   ![SAP CAL Basic režim: Vytvoření Instance](./media/cal-s4h/s4h-pic10a.png)

2. <span data-ttu-id="454f0-188">Klikněte na tlačítko **vytvořit**a v hello se zprávou, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="454f0-188">Click **Create**, and in hello message box that appears, click **OK**.</span></span>

   ![SAP CAL podporované velikosti virtuálních počítačů](./media/cal-s4h/s4h-pic10b.png)

3. <span data-ttu-id="454f0-190">V hello **privátní klíč** dialogové okno, klikněte na tlačítko **úložiště** toostore hello privátní klíč v hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-190">In hello **Private Key** dialog box, click **Store** toostore hello private key in hello SAP CAL.</span></span> <span data-ttu-id="454f0-191">Klikněte na tlačítko toouse ochrana heslem pro hello privátní klíč, **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="454f0-191">toouse password protection for hello private key, click **Download**.</span></span> 

   ![SAP CAL privátní klíč](./media/cal-s4h/s4h-pic10c.png)

4. <span data-ttu-id="454f0-193">Čtení hello SAP CAL **upozornění** zpráva a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="454f0-193">Read hello SAP CAL **Warning** message, and click **OK**.</span></span>

   ![Upozornění CAL SAP](./media/cal-s4h/s4h-pic10d.png)

    <span data-ttu-id="454f0-195">Nyní hello nasazení probíhá.</span><span class="sxs-lookup"><span data-stu-id="454f0-195">Now hello deployment takes place.</span></span> <span data-ttu-id="454f0-196">Po určité době, v závislosti na složitosti hello řešení (hello, SAP CAL poskytuje odhad) a velikost hello hello stav se zobrazuje jako aktivní a připravena k použití.</span><span class="sxs-lookup"><span data-stu-id="454f0-196">After some time, depending on hello size and complexity of hello solution (hello SAP CAL provides an estimate), hello status is shown as active and ready for use.</span></span>

5. <span data-ttu-id="454f0-197">toofind hello virtuální počítače shromážděné pomocí hello další související prostředky v jedné skupině prostředků, přejděte toohello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="454f0-197">toofind hello virtual machines collected with hello other associated resources in one resource group, go toohello Azure portal:</span></span> 

   ![Objekty SAP CAL nasazené v hello nového portálu.](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. <span data-ttu-id="454f0-199">Na portálu hello SAP CAL, hello stav se zobrazí jako **Active**.</span><span class="sxs-lookup"><span data-stu-id="454f0-199">On hello SAP CAL portal, hello status appears as **Active**.</span></span> <span data-ttu-id="454f0-200">tooconnect toohello řešení, klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="454f0-200">tooconnect toohello solution, click **Connect**.</span></span> <span data-ttu-id="454f0-201">Různé možnosti tooconnect toohello různé součásti jsou nasazeny v rámci tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="454f0-201">Different options tooconnect toohello different components are deployed within this solution.</span></span>

   ![Instance SAP Kalendáře](./media/cal-s4h/active_solution.png)

7. <span data-ttu-id="454f0-203">Než budete moct použít jeden z hello možnosti tooconnect toohello nasazené systémy, klikněte na tlačítko **– Příručka Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="454f0-203">Before you can use one of hello options tooconnect toohello deployed systems, click **Getting Started Guide**.</span></span> 

   ![Připojit toohello Instance](./media/cal-s4h/connect_to_solution.png)

    <span data-ttu-id="454f0-205">Hello dokumentace názvy hello uživatelů pro každou z metod hello připojení.</span><span class="sxs-lookup"><span data-stu-id="454f0-205">hello documentation names hello users for each of hello connectivity methods.</span></span> <span data-ttu-id="454f0-206">Hello hesla pro uživatele, se nastavují toohello hlavní heslo, které jste definovali od začátku hello hello procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="454f0-206">hello passwords for those users are set toohello master password you defined at hello beginning of hello deployment process.</span></span> <span data-ttu-id="454f0-207">Hello dokumentace, jsou uvedena jiných více funkční uživatelů s jejich hesla, které můžete použít toosign v toohello nasazené systému.</span><span class="sxs-lookup"><span data-stu-id="454f0-207">In hello documentation, other more functional users are listed with their passwords, which you can use toosign in toohello deployed system.</span></span> 

    <span data-ttu-id="454f0-208">Například pokud použijete hello SAP grafickým uživatelským rozhraním, který je předinstalován v počítači vzdálené plochy Windows hello, hello S/4 systému může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="454f0-208">For example, if you use hello SAP GUI that's preinstalled on hello Windows Remote Desktop machine, hello S/4 system might look like this:</span></span>

   ![SM50 v hello předinstalována SAP grafického uživatelského rozhraní](./media/cal-s4h/gui_sm50.png)

    <span data-ttu-id="454f0-210">Nebo pokud používáte hello DBACockpit, hello instance může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="454f0-210">Or if you use hello DBACockpit, hello instance might look like this:</span></span>

   ![SM50 v hello DBACockpit SAP grafického uživatelského rozhraní](./media/cal-s4h/dbacockpit.png)

<span data-ttu-id="454f0-212">V rámci několik hodin je v pořádku zařízení SAP S/4 nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-212">Within a few hours, a healthy SAP S/4 appliance is deployed in Azure.</span></span>

<span data-ttu-id="454f0-213">Pokud jste si zakoupili předplatné SAP CAL, SAP plně podporuje nasazení prostřednictvím hello SAP CAL na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="454f0-213">If you bought an SAP CAL subscription, SAP fully supports deployments through hello SAP CAL on Azure.</span></span> <span data-ttu-id="454f0-214">fronta podporu Hello je BC. VCM CAL.</span><span class="sxs-lookup"><span data-stu-id="454f0-214">hello support queue is BC-VCM-CAL.</span></span>







