---
title: "aaaDeploy SAP SP3 EHP7 integrovaného vývojového prostředí pro SAP ERP 6.0 v Azure | Microsoft Docs"
description: "Nasazení SAP integrovaného vývojového prostředí EHP7 SP3 pro SAP ERP 6.0 na Azure"
services: virtual-machines-windows
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 26d88c7b48a91d35602464c4f89ca7a30502c4b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a><span data-ttu-id="25cf5-103">Nasazení SAP integrovaného vývojového prostředí EHP7 SP3 pro SAP ERP 6.0 na Azure</span><span class="sxs-lookup"><span data-stu-id="25cf5-103">Deploy SAP IDES EHP7 SP3 for SAP ERP 6.0 on Azure</span></span>
<span data-ttu-id="25cf5-104">Tento článek popisuje, jak toodeploy SAP integrovaného vývojového prostředí systému SQL Server a operační systém Windows hello na Azure prostřednictvím hello SAP cloudu zařízení knihoven (SAP CAL) 3.0.</span><span class="sxs-lookup"><span data-stu-id="25cf5-104">This article describes how toodeploy an SAP IDES system running with SQL Server and hello Windows operating system on Azure via hello SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="25cf5-105">snímky obrazovky Hello zobrazit podrobný postup hello.</span><span class="sxs-lookup"><span data-stu-id="25cf5-105">hello screenshots show hello step-by-step process.</span></span> <span data-ttu-id="25cf5-106">toodeploy jiné řešení, postupujte podle stejných kroků hello.</span><span class="sxs-lookup"><span data-stu-id="25cf5-106">toodeploy a different solution, follow hello same steps.</span></span>

<span data-ttu-id="25cf5-107">toostart s hello CAL SAP, přejděte toohello [knihovny zařízení cloudu SAP](https://cal.sap.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="25cf5-107">toostart with hello SAP CAL, go toohello [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="25cf5-108">SAP má také blog o hello nové [SAP cloudu zařízení knihovna 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="25cf5-108">SAP also has a blog about hello new [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span> 

> [!NOTE]
<span data-ttu-id="25cf5-109">Jako 29. květen 2017, můžete pomocí modelu nasazení Azure Resource Manager hello kromě model nasazení classic méně než upřednostňovaný toohello toodeploy hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="25cf5-109">As of May 29, 2017, you can use hello Azure Resource Manager deployment model in addition toohello less-preferred classic deployment model toodeploy hello SAP CAL.</span></span> <span data-ttu-id="25cf5-110">Doporučujeme použít hello nového modelu nasazení Resource Manager a modelu nasazení classic hello ignorovat.</span><span class="sxs-lookup"><span data-stu-id="25cf5-110">We recommend that you use hello new Resource Manager deployment model and disregard hello classic deployment model.</span></span>

<span data-ttu-id="25cf5-111">Pokud jste již vytvořili SAP CAL účet, který používá hello klasického modelu, *potřebujete toocreate jiný účet SAP CAL*.</span><span class="sxs-lookup"><span data-stu-id="25cf5-111">If you already created an SAP CAL account that uses hello classic model, *you need toocreate another SAP CAL account*.</span></span> <span data-ttu-id="25cf5-112">Tento účet musí tooexclusively nasadit do Azure pomocí modelu Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="25cf5-112">This account needs tooexclusively deploy into Azure by using hello Resource Manager model.</span></span>

<span data-ttu-id="25cf5-113">Po přihlášení toohello SAP CAL první stránku hello obvykle vás toohello **řešení** stránky.</span><span class="sxs-lookup"><span data-stu-id="25cf5-113">After you sign in toohello SAP CAL, hello first page usually leads you toohello **Solutions** page.</span></span> <span data-ttu-id="25cf5-114">řešení Hello nabízené na hello SAP CAL jsou vytrvale zvýšení, tak může být nutné tooscroll odlišují hello řešení toofind, které chcete.</span><span class="sxs-lookup"><span data-stu-id="25cf5-114">hello solutions offered on hello SAP CAL are steadily increasing, so you might need tooscroll quite a bit toofind hello solution you want.</span></span> <span data-ttu-id="25cf5-115">Hello zvýrazněná integrovaného vývojového prostředí systému Windows SAP řešení, které je k dispozici výhradně na Azure ukazuje procesu nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="25cf5-115">hello highlighted Windows-based SAP IDES solution that is available exclusively on Azure demonstrates hello deployment process:</span></span>

![Řešení CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-hello-sap-cal"></a><span data-ttu-id="25cf5-117">Vytvoření účtu na hello SAP CAL</span><span class="sxs-lookup"><span data-stu-id="25cf5-117">Create an account in hello SAP CAL</span></span>
1. <span data-ttu-id="25cf5-118">toosign v toohello SAP CAL pro hello poprvé, použijte S SAP-uživatele nebo jiný uživatel zaregistrována SAP.</span><span class="sxs-lookup"><span data-stu-id="25cf5-118">toosign in toohello SAP CAL for hello first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="25cf5-119">Poté definujte SAP CAL účtu, který je používán hello SAP CAL toodeploy zařízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="25cf5-119">Then define an SAP CAL account that is used by hello SAP CAL toodeploy appliances on Azure.</span></span> <span data-ttu-id="25cf5-120">V definici hello účet budete muset:</span><span class="sxs-lookup"><span data-stu-id="25cf5-120">In hello account definition, you need to:</span></span>

    <span data-ttu-id="25cf5-121">a.</span><span class="sxs-lookup"><span data-stu-id="25cf5-121">a.</span></span> <span data-ttu-id="25cf5-122">Vyberte model nasazení hello v Azure (Resource Manager nebo classic).</span><span class="sxs-lookup"><span data-stu-id="25cf5-122">Select hello deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="25cf5-123">b.</span><span class="sxs-lookup"><span data-stu-id="25cf5-123">b.</span></span> <span data-ttu-id="25cf5-124">Zadejte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="25cf5-124">Enter your Azure subscription.</span></span> <span data-ttu-id="25cf5-125">Účet SAP CAL lze přiřadit pouze tooone předplatné.</span><span class="sxs-lookup"><span data-stu-id="25cf5-125">An SAP CAL account can be assigned tooone subscription only.</span></span> <span data-ttu-id="25cf5-126">Pokud potřebujete více než jedno předplatné, je třeba toocreate jiný účet SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="25cf5-126">If you need more than one subscription, you need toocreate another SAP CAL account.</span></span>
    
    <span data-ttu-id="25cf5-127">c.</span><span class="sxs-lookup"><span data-stu-id="25cf5-127">c.</span></span> <span data-ttu-id="25cf5-128">Dejte hello SAP CAL oprávnění toodeploy do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="25cf5-128">Give hello SAP CAL permission toodeploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="25cf5-129">Hello následující kroky ukazují, jak toocreate je SAP CAL účet pro nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="25cf5-129">hello next steps show how toocreate an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="25cf5-130">Pokud již máte účet SAP CAL, který je propojené toohello modelu nasazení classic, můžete *potřebovat* toofollow tyto kroky toocreate nový účet SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="25cf5-130">If you already have an SAP CAL account that is linked toohello classic deployment model, you *need* toofollow these steps toocreate a new SAP CAL account.</span></span> <span data-ttu-id="25cf5-131">nový účet SAP CAL Hello musí toodeploy v modelu Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="25cf5-131">hello new SAP CAL account needs toodeploy in hello Resource Manager model.</span></span>

2. <span data-ttu-id="25cf5-132">toocreate nové CAL SAP účet, hello **účty** stránky zobrazí dvě možnosti pro Azure:</span><span class="sxs-lookup"><span data-stu-id="25cf5-132">toocreate a new SAP CAL account, hello **Accounts** page shows two choices for Azure:</span></span> 

    <span data-ttu-id="25cf5-133">a.</span><span class="sxs-lookup"><span data-stu-id="25cf5-133">a.</span></span> <span data-ttu-id="25cf5-134">**Microsoft Azure (klasický)** je hello modelu nasazení classic a již není upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="25cf5-134">**Microsoft Azure (classic)** is hello classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="25cf5-135">b.</span><span class="sxs-lookup"><span data-stu-id="25cf5-135">b.</span></span> <span data-ttu-id="25cf5-136">**Microsoft Azure** je hello nového modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="25cf5-136">**Microsoft Azure** is hello new Resource Manager deployment model.</span></span>

    ![Účty CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    <span data-ttu-id="25cf5-138">Vyberte toodeploy v modelu Resource Manager hello **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-138">toodeploy in hello Resource Manager model, select **Microsoft Azure**.</span></span>

    ![Účty CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

3. <span data-ttu-id="25cf5-140">Zadejte hello Azure **ID předplatného** , naleznete na portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="25cf5-140">Enter hello Azure **Subscription ID** that can be found on hello Azure portal.</span></span> 

    ![ID předplatného CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

4. <span data-ttu-id="25cf5-142">definované tooauthorize hello SAP CAL toodeploy do hello předplatné Azure, klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-142">tooauthorize hello SAP CAL toodeploy into hello Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="25cf5-143">hello záložce prohlížeče se zobrazí následující stránka Hello:</span><span class="sxs-lookup"><span data-stu-id="25cf5-143">hello following page appears in hello browser tab:</span></span>

    ![Internet Explorer cloudových služeb přihlášení](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

5. <span data-ttu-id="25cf5-145">Pokud je uveden více než jeden uživatel, zvolte účet Microsoft hello, který je propojený toobe hello spolusprávce hello předplatné Azure, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="25cf5-145">If more than one user is listed, choose hello Microsoft account that is linked toobe hello coadministrator of hello Azure subscription you selected.</span></span> <span data-ttu-id="25cf5-146">hello záložce prohlížeče se zobrazí následující stránka Hello:</span><span class="sxs-lookup"><span data-stu-id="25cf5-146">hello following page appears in hello browser tab:</span></span>

    ![Internet Explorer cloudové služby potvrzení](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

6. <span data-ttu-id="25cf5-148">Klikněte na tlačítko **přijmout**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-148">Click **Accept**.</span></span> <span data-ttu-id="25cf5-149">Pokud autorizace hello je úspěšné, hello Definice SAP CAL účtu se zobrazí znovu.</span><span class="sxs-lookup"><span data-stu-id="25cf5-149">If hello authorization is successful, hello SAP CAL account definition displays again.</span></span> <span data-ttu-id="25cf5-150">Po krátkou dobu zprávu potvrdí, že proces autorizace hello bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="25cf5-150">After a short time, a message confirms that hello authorization process was successful.</span></span>

7. <span data-ttu-id="25cf5-151">tooassign hello nově vytvořený uživatel tooyour účet SAP CAL, zadejte vaše **ID uživatele** v hello textového pole na hello správné a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-151">tooassign hello newly created SAP CAL account tooyour user, enter your **User ID** in hello text box on hello right and click **Add**.</span></span> 

    ![Přidružení toouser účtu](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

8. <span data-ttu-id="25cf5-153">Klikněte na tlačítko tooassociate vašeho účtu pomocí hello uživatele, že používáte toosign toohello SAP CAL, **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-153">tooassociate your account with hello user that you use toosign in toohello SAP CAL, click **Review**.</span></span> 

9. <span data-ttu-id="25cf5-154">Klikněte na tlačítko toocreate hello přidružení mezi uživateli a hello nově vytvořený účet SAP CAL, **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-154">toocreate hello association between your user and hello newly created SAP CAL account, click **Create**.</span></span>

    ![Tooaccount přidružení uživatele](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

<span data-ttu-id="25cf5-156">Úspěšně jste vytvořili účet SAP CAL, aby bylo možné:</span><span class="sxs-lookup"><span data-stu-id="25cf5-156">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="25cf5-157">Pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="25cf5-157">Use hello Resource Manager deployment model.</span></span>
- <span data-ttu-id="25cf5-158">Nasazení systémů SAP do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="25cf5-158">Deploy SAP systems into your Azure subscription.</span></span>

> [!NOTE]
<span data-ttu-id="25cf5-159">Před nasazením řešení integrovaného vývojového prostředí SAP hello založené na systému Windows a SQL Server, bude pravděpodobně nutné toosign pro předplatné SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="25cf5-159">Before you can deploy hello SAP IDES solution based on Windows and SQL Server, you might need toosign up for an SAP CAL subscription.</span></span> <span data-ttu-id="25cf5-160">Jinak se může hello řešení nezobrazí jako **uzamčen** na stránce Přehled hello.</span><span class="sxs-lookup"><span data-stu-id="25cf5-160">Otherwise, hello solution might show up as **Locked** on hello overview page.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="25cf5-161">Nasazení řešení</span><span class="sxs-lookup"><span data-stu-id="25cf5-161">Deploy a solution</span></span>
1. <span data-ttu-id="25cf5-162">Když nastavíte účet SAP CAL, vyberte **hello řešení SAP integrovaného vývojového prostředí v systémech Windows a SQL Server** řešení.</span><span class="sxs-lookup"><span data-stu-id="25cf5-162">After you set up an SAP CAL account, select **hello SAP IDES solution on Windows and SQL Server** solution.</span></span> <span data-ttu-id="25cf5-163">Klikněte na tlačítko **vytvořit instanci**a potvrďte podmínky použití a podmínky hello.</span><span class="sxs-lookup"><span data-stu-id="25cf5-163">Click **Create Instance**, and confirm hello usage and terms conditions.</span></span> 

2. <span data-ttu-id="25cf5-164">Na hello **základní režim: Vytvořte instanci** stránky, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="25cf5-164">On hello **Basic Mode: Create Instance** page, you need to:</span></span>

    <span data-ttu-id="25cf5-165">a.</span><span class="sxs-lookup"><span data-stu-id="25cf5-165">a.</span></span> <span data-ttu-id="25cf5-166">Zadejte instance **název**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-166">Enter an instance **Name**.</span></span>

    <span data-ttu-id="25cf5-167">b.</span><span class="sxs-lookup"><span data-stu-id="25cf5-167">b.</span></span> <span data-ttu-id="25cf5-168">Vyberte Azure **oblast**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-168">Select an Azure **Region**.</span></span> <span data-ttu-id="25cf5-169">Můžete potřebovat SAP CAL tooget předplatného nabízí několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="25cf5-169">You might need an SAP CAL subscription tooget multiple Azure regions offered.</span></span>

    <span data-ttu-id="25cf5-170">c.</span><span class="sxs-lookup"><span data-stu-id="25cf5-170">c.</span></span>  <span data-ttu-id="25cf5-171">Zadejte hlavní hello **heslo** hello řešení, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="25cf5-171">Enter hello master **Password** for hello solution, as shown:</span></span>

    ![SAP CAL Basic režim: Vytvoření Instance](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

3. <span data-ttu-id="25cf5-173">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-173">Click **Create**.</span></span> <span data-ttu-id="25cf5-174">Po určité době, v závislosti na složitosti hello řešení (hello, SAP CAL poskytuje odhad) a velikost hello hello stav se zobrazuje jako aktivní a připravena k použití:</span><span class="sxs-lookup"><span data-stu-id="25cf5-174">After some time, depending on hello size and complexity of hello solution (hello SAP CAL provides an estimate), hello status is shown as active and ready for use:</span></span> 

    ![Instance SAP Kalendáře](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

4. <span data-ttu-id="25cf5-176">Skupina prostředků hello toofind a všechny její objekty, které byly vytvořeny hello SAP CAL, přejděte toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="25cf5-176">toofind hello resource group and all its objects that were created by hello SAP CAL, go toohello Azure portal.</span></span> <span data-ttu-id="25cf5-177">virtuální počítač Hello naleznete počínaje hello stejný název, který byl zadán v hello SAP CAL instance.</span><span class="sxs-lookup"><span data-stu-id="25cf5-177">hello virtual machine can be found starting with hello same instance name that was given in hello SAP CAL.</span></span>

    ![Objekty skupiny prostředků](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

5. <span data-ttu-id="25cf5-179">Na portálu SAP CAL hello, přejděte instancí toohello nasazení a klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-179">On hello SAP CAL portal, go toohello deployed instances and click **Connect**.</span></span> <span data-ttu-id="25cf5-180">Hello následující automaticky otevírané okno se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="25cf5-180">hello following pop-up window appears:</span></span> 

    ![Připojit toohello Instance](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

6. <span data-ttu-id="25cf5-182">Než budete moct použít jeden z hello možnosti tooconnect toohello nasazené systémy, klikněte na tlačítko **– Příručka Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="25cf5-182">Before you can use one of hello options tooconnect toohello deployed systems, click **Getting Started Guide**.</span></span> <span data-ttu-id="25cf5-183">Hello dokumentace názvy hello uživatelů pro každou z metod hello připojení.</span><span class="sxs-lookup"><span data-stu-id="25cf5-183">hello documentation names hello users for each of hello connectivity methods.</span></span> <span data-ttu-id="25cf5-184">Hello hesla pro uživatele, se nastavují toohello hlavní heslo, které jste definovali od začátku hello hello procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="25cf5-184">hello passwords for those users are set toohello master password you defined at hello beginning of hello deployment process.</span></span> <span data-ttu-id="25cf5-185">Hello dokumentace, jsou uvedena jiných více funkční uživatelů s jejich hesla, které můžete použít toosign v toohello nasazené systému.</span><span class="sxs-lookup"><span data-stu-id="25cf5-185">In hello documentation, other more functional users are listed with their passwords, which you can use toosign in toohello deployed system.</span></span>

    ![Vítejte v dokumentaci k SAP](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

<span data-ttu-id="25cf5-187">V rámci několik hodin je v pořádku systému integrovaného vývojového prostředí SAP nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="25cf5-187">Within a few hours, a healthy SAP IDES system is deployed in Azure.</span></span>

<span data-ttu-id="25cf5-188">Pokud jste si zakoupili předplatné SAP CAL, SAP plně podporuje nasazení prostřednictvím hello SAP CAL na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="25cf5-188">If you bought an SAP CAL subscription, SAP fully supports deployments through hello SAP CAL on Azure.</span></span> <span data-ttu-id="25cf5-189">fronta podporu Hello je BC. VCM CAL.</span><span class="sxs-lookup"><span data-stu-id="25cf5-189">hello support queue is BC-VCM-CAL.</span></span>

