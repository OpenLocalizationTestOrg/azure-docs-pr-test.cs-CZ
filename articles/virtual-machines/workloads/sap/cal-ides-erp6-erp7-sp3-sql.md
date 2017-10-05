---
title: "Nasazení SAP integrovaného vývojového prostředí EHP7 SP3 pro SAP ERP 6.0 v Azure | Microsoft Docs"
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
ms.openlocfilehash: 91eed294077ff72d0760018b10c98f32db88f3be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a><span data-ttu-id="5be03-103">Nasazení SAP integrovaného vývojového prostředí EHP7 SP3 pro SAP ERP 6.0 na Azure</span><span class="sxs-lookup"><span data-stu-id="5be03-103">Deploy SAP IDES EHP7 SP3 for SAP ERP 6.0 on Azure</span></span>
<span data-ttu-id="5be03-104">Tento článek popisuje postup nasazení SAP integrovaného vývojového prostředí systému spuštěné s SQL serverem a operačním systému Windows v Azure prostřednictvím SAP cloudu zařízení knihoven (SAP CAL) 3.0.</span><span class="sxs-lookup"><span data-stu-id="5be03-104">This article describes how to deploy an SAP IDES system running with SQL Server and the Windows operating system on Azure via the SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="5be03-105">Na snímcích obrazovky zobrazit podrobný postup.</span><span class="sxs-lookup"><span data-stu-id="5be03-105">The screenshots show the step-by-step process.</span></span> <span data-ttu-id="5be03-106">Pokud chcete nasadit jiné řešení, použijte stejný postup.</span><span class="sxs-lookup"><span data-stu-id="5be03-106">To deploy a different solution, follow the same steps.</span></span>

<span data-ttu-id="5be03-107">Chcete-li začít s SAP CAL, přejděte na [SAP cloudu zařízení knihovny](https://cal.sap.com/) webu.</span><span class="sxs-lookup"><span data-stu-id="5be03-107">To start with the SAP CAL, go to the [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="5be03-108">SAP má také blog o nové [SAP cloudu zařízení knihovna 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="5be03-108">SAP also has a blog about the new [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span> 

> [!NOTE]
<span data-ttu-id="5be03-109">Od 29. květen 2017 můžete v modelu nasazení Azure Resource Manager kromě modelu nasazení classic méně než upřednostňovaný nasazení SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="5be03-109">As of May 29, 2017, you can use the Azure Resource Manager deployment model in addition to the less-preferred classic deployment model to deploy the SAP CAL.</span></span> <span data-ttu-id="5be03-110">Doporučujeme použít nový model nasazení Resource Manager a modelu nasazení classic ignorovat.</span><span class="sxs-lookup"><span data-stu-id="5be03-110">We recommend that you use the new Resource Manager deployment model and disregard the classic deployment model.</span></span>

<span data-ttu-id="5be03-111">Pokud jste již vytvořili SAP CAL účet, který používá v případě klasického modelu *musíte vytvořit jiný účet SAP CAL*.</span><span class="sxs-lookup"><span data-stu-id="5be03-111">If you already created an SAP CAL account that uses the classic model, *you need to create another SAP CAL account*.</span></span> <span data-ttu-id="5be03-112">Tento účet musí výhradně nasadit do Azure pomocí modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5be03-112">This account needs to exclusively deploy into Azure by using the Resource Manager model.</span></span>

<span data-ttu-id="5be03-113">Po přihlášení k prostředí SAP CAL na první stránku obvykle vede k **řešení** stránky.</span><span class="sxs-lookup"><span data-stu-id="5be03-113">After you sign in to the SAP CAL, the first page usually leads you to the **Solutions** page.</span></span> <span data-ttu-id="5be03-114">Řešení nabízí na SAP CAL jsou vytrvale zvýšení, takže možná budete muset posunout odlišují najít řešení, které chcete.</span><span class="sxs-lookup"><span data-stu-id="5be03-114">The solutions offered on the SAP CAL are steadily increasing, so you might need to scroll quite a bit to find the solution you want.</span></span> <span data-ttu-id="5be03-115">Zvýrazněných integrovaného vývojového prostředí systému Windows SAP řešení, které je k dispozici výhradně na Azure znázorňuje proces nasazení:</span><span class="sxs-lookup"><span data-stu-id="5be03-115">The highlighted Windows-based SAP IDES solution that is available exclusively on Azure demonstrates the deployment process:</span></span>

![Řešení CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-the-sap-cal"></a><span data-ttu-id="5be03-117">Vytvoření účtu na SAP CAL</span><span class="sxs-lookup"><span data-stu-id="5be03-117">Create an account in the SAP CAL</span></span>
1. <span data-ttu-id="5be03-118">Přihlaste se k SAP CAL poprvé, použijte S SAP-uživatele nebo jiný uživatel zaregistrován u služby SAP.</span><span class="sxs-lookup"><span data-stu-id="5be03-118">To sign in to the SAP CAL for the first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="5be03-119">Poté definujte SAP CAL účtu, který je používán SAP CAL nasazení zařízení v Azure.</span><span class="sxs-lookup"><span data-stu-id="5be03-119">Then define an SAP CAL account that is used by the SAP CAL to deploy appliances on Azure.</span></span> <span data-ttu-id="5be03-120">V definici účtu budete muset:</span><span class="sxs-lookup"><span data-stu-id="5be03-120">In the account definition, you need to:</span></span>

    <span data-ttu-id="5be03-121">a.</span><span class="sxs-lookup"><span data-stu-id="5be03-121">a.</span></span> <span data-ttu-id="5be03-122">Vyberte model nasazení v Azure (Resource Manager nebo classic).</span><span class="sxs-lookup"><span data-stu-id="5be03-122">Select the deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="5be03-123">b.</span><span class="sxs-lookup"><span data-stu-id="5be03-123">b.</span></span> <span data-ttu-id="5be03-124">Zadejte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5be03-124">Enter your Azure subscription.</span></span> <span data-ttu-id="5be03-125">Účet SAP CAL lze přiřadit pouze jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="5be03-125">An SAP CAL account can be assigned to one subscription only.</span></span> <span data-ttu-id="5be03-126">Pokud potřebujete více než jedno předplatné, musíte vytvořit jiný účet SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="5be03-126">If you need more than one subscription, you need to create another SAP CAL account.</span></span>
    
    <span data-ttu-id="5be03-127">c.</span><span class="sxs-lookup"><span data-stu-id="5be03-127">c.</span></span> <span data-ttu-id="5be03-128">Udělte oprávnění SAP CAL k nasazení do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5be03-128">Give the SAP CAL permission to deploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="5be03-129">Další kroky ukazují, jak vytvořit účet SAP CAL pro nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5be03-129">The next steps show how to create an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="5be03-130">Pokud již máte účet SAP CAL, který je propojený s modelem nasazení classic budete *potřebovat* postupovat podle těchto kroků můžete vytvořit nový účet SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="5be03-130">If you already have an SAP CAL account that is linked to the classic deployment model, you *need* to follow these steps to create a new SAP CAL account.</span></span> <span data-ttu-id="5be03-131">Nový účet SAP CAL je potřeba nasadit v modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5be03-131">The new SAP CAL account needs to deploy in the Resource Manager model.</span></span>

2. <span data-ttu-id="5be03-132">Chcete-li vytvořit nový účet SAP CAL, **účty** stránky zobrazí dvě možnosti pro Azure:</span><span class="sxs-lookup"><span data-stu-id="5be03-132">To create a new SAP CAL account, the **Accounts** page shows two choices for Azure:</span></span> 

    <span data-ttu-id="5be03-133">a.</span><span class="sxs-lookup"><span data-stu-id="5be03-133">a.</span></span> <span data-ttu-id="5be03-134">**Microsoft Azure (klasický)** je modelu nasazení classic a již není upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="5be03-134">**Microsoft Azure (classic)** is the classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="5be03-135">b.</span><span class="sxs-lookup"><span data-stu-id="5be03-135">b.</span></span> <span data-ttu-id="5be03-136">**Microsoft Azure** je nový model nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5be03-136">**Microsoft Azure** is the new Resource Manager deployment model.</span></span>

    ![Účty CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    <span data-ttu-id="5be03-138">Chcete-li nasadit v modelu Resource Manager, vyberte **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="5be03-138">To deploy in the Resource Manager model, select **Microsoft Azure**.</span></span>

    ![Účty CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

3. <span data-ttu-id="5be03-140">Zadejte Azure **ID předplatného** , naleznete na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5be03-140">Enter the Azure **Subscription ID** that can be found on the Azure portal.</span></span> 

    ![ID předplatného CAL SAP](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

4. <span data-ttu-id="5be03-142">K autorizaci CAL SAP k nasazení do předplatné Azure, které jste definovali, klikněte na tlačítko **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="5be03-142">To authorize the SAP CAL to deploy into the Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="5be03-143">Na následující stránce se zobrazí na záložce prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="5be03-143">The following page appears in the browser tab:</span></span>

    ![Internet Explorer cloudových služeb přihlášení](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

5. <span data-ttu-id="5be03-145">Pokud je uveden více než jeden uživatel, zvolte účet Microsoft, který je propojený jako spolusprávce předplatného Azure, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="5be03-145">If more than one user is listed, choose the Microsoft account that is linked to be the coadministrator of the Azure subscription you selected.</span></span> <span data-ttu-id="5be03-146">Na následující stránce se zobrazí na záložce prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="5be03-146">The following page appears in the browser tab:</span></span>

    ![Internet Explorer cloudové služby potvrzení](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

6. <span data-ttu-id="5be03-148">Klikněte na tlačítko **přijmout**.</span><span class="sxs-lookup"><span data-stu-id="5be03-148">Click **Accept**.</span></span> <span data-ttu-id="5be03-149">Pokud ověřování úspěšné, zobrazí definici účtu SAP CAL znovu.</span><span class="sxs-lookup"><span data-stu-id="5be03-149">If the authorization is successful, the SAP CAL account definition displays again.</span></span> <span data-ttu-id="5be03-150">Po krátkou dobu zprávu potvrdí, že proces autorizace bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="5be03-150">After a short time, a message confirms that the authorization process was successful.</span></span>

7. <span data-ttu-id="5be03-151">Chcete-li přiřadit nově vytvořený účet SAP CAL pro vaše uživatele, zadejte vaše **ID uživatele** do textového pole na doprava a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5be03-151">To assign the newly created SAP CAL account to your user, enter your **User ID** in the text box on the right and click **Add**.</span></span> 

    ![Účet k přidružení uživatele](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

8. <span data-ttu-id="5be03-153">Chcete-li přidružit uživatele, který používáte pro přihlášení k prostředí SAP CAL váš účet, klikněte na tlačítko **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="5be03-153">To associate your account with the user that you use to sign in to the SAP CAL, click **Review**.</span></span> 

9. <span data-ttu-id="5be03-154">Chcete-li vytvořit přidružení mezi uživateli a nově vytvořený účet SAP CAL, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5be03-154">To create the association between your user and the newly created SAP CAL account, click **Create**.</span></span>

    ![Uživatele k přidružení účtu](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

<span data-ttu-id="5be03-156">Úspěšně jste vytvořili účet SAP CAL, aby bylo možné:</span><span class="sxs-lookup"><span data-stu-id="5be03-156">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="5be03-157">Pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5be03-157">Use the Resource Manager deployment model.</span></span>
- <span data-ttu-id="5be03-158">Nasazení systémů SAP do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5be03-158">Deploy SAP systems into your Azure subscription.</span></span>

> [!NOTE]
<span data-ttu-id="5be03-159">Před nasazením řešení SAP integrovaného vývojového prostředí na základě systému Windows a SQL Server, možná budete muset zaregistrujte si předplatné SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="5be03-159">Before you can deploy the SAP IDES solution based on Windows and SQL Server, you might need to sign up for an SAP CAL subscription.</span></span> <span data-ttu-id="5be03-160">Jinak se možná řešení nezobrazí jako **uzamčen** na stránce Přehled.</span><span class="sxs-lookup"><span data-stu-id="5be03-160">Otherwise, the solution might show up as **Locked** on the overview page.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="5be03-161">Nasazení řešení</span><span class="sxs-lookup"><span data-stu-id="5be03-161">Deploy a solution</span></span>
1. <span data-ttu-id="5be03-162">Když nastavíte účet SAP CAL, vyberte **řešení SAP integrovaného vývojového prostředí v systémech Windows a SQL Server** řešení.</span><span class="sxs-lookup"><span data-stu-id="5be03-162">After you set up an SAP CAL account, select **The SAP IDES solution on Windows and SQL Server** solution.</span></span> <span data-ttu-id="5be03-163">Klikněte na tlačítko **vytvořit instanci**a potvrďte podmínky použití a podmínky.</span><span class="sxs-lookup"><span data-stu-id="5be03-163">Click **Create Instance**, and confirm the usage and terms conditions.</span></span> 

2. <span data-ttu-id="5be03-164">Na **základní režim: vytvoření Instance** stránky, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="5be03-164">On the **Basic Mode: Create Instance** page, you need to:</span></span>

    <span data-ttu-id="5be03-165">a.</span><span class="sxs-lookup"><span data-stu-id="5be03-165">a.</span></span> <span data-ttu-id="5be03-166">Zadejte instance **název**.</span><span class="sxs-lookup"><span data-stu-id="5be03-166">Enter an instance **Name**.</span></span>

    <span data-ttu-id="5be03-167">b.</span><span class="sxs-lookup"><span data-stu-id="5be03-167">b.</span></span> <span data-ttu-id="5be03-168">Vyberte Azure **oblast**.</span><span class="sxs-lookup"><span data-stu-id="5be03-168">Select an Azure **Region**.</span></span> <span data-ttu-id="5be03-169">Může být nutné předplatné SAP CAL získat několika oblastmi Azure nabízí.</span><span class="sxs-lookup"><span data-stu-id="5be03-169">You might need an SAP CAL subscription to get multiple Azure regions offered.</span></span>

    <span data-ttu-id="5be03-170">c.</span><span class="sxs-lookup"><span data-stu-id="5be03-170">c.</span></span>  <span data-ttu-id="5be03-171">Zadejte hlavní **heslo** pro řešení, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="5be03-171">Enter the master **Password** for the solution, as shown:</span></span>

    ![SAP CAL Basic režim: Vytvoření Instance](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

3. <span data-ttu-id="5be03-173">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5be03-173">Click **Create**.</span></span> <span data-ttu-id="5be03-174">Po určité době, v závislosti na velikost a složitost řešení (SAP CAL poskytuje odhad), je stav zobrazen jako aktivní a připravena k použití:</span><span class="sxs-lookup"><span data-stu-id="5be03-174">After some time, depending on the size and complexity of the solution (the SAP CAL provides an estimate), the status is shown as active and ready for use:</span></span> 

    ![Instance SAP Kalendáře](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

4. <span data-ttu-id="5be03-176">Chcete-li najít skupinu prostředků a všechny její objekty, které byly vytvořeny SAP CAL, přejděte na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5be03-176">To find the resource group and all its objects that were created by the SAP CAL, go to the Azure portal.</span></span> <span data-ttu-id="5be03-177">Virtuální počítač můžete najít spuštěním se stejným názvem instance, který byl zadán v SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="5be03-177">The virtual machine can be found starting with the same instance name that was given in the SAP CAL.</span></span>

    ![Objekty skupiny prostředků](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

5. <span data-ttu-id="5be03-179">Na portálu SAP CAL, přejděte do nasazené instancí a klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="5be03-179">On the SAP CAL portal, go to the deployed instances and click **Connect**.</span></span> <span data-ttu-id="5be03-180">Zobrazí se následující automaticky otevírané okno okno:</span><span class="sxs-lookup"><span data-stu-id="5be03-180">The following pop-up window appears:</span></span> 

    ![Připojte se k instanci](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

6. <span data-ttu-id="5be03-182">Než použijete jednu z možností pro připojení k nasazené systémy, klikněte na tlačítko **– Příručka Začínáme**.</span><span class="sxs-lookup"><span data-stu-id="5be03-182">Before you can use one of the options to connect to the deployed systems, click **Getting Started Guide**.</span></span> <span data-ttu-id="5be03-183">V dokumentaci názvy uživatelů pro každou metodu připojení.</span><span class="sxs-lookup"><span data-stu-id="5be03-183">The documentation names the users for each of the connectivity methods.</span></span> <span data-ttu-id="5be03-184">Hesla pro tyto uživatele jsou nastaveny na hlavní heslo, které jste definovali na začátku procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="5be03-184">The passwords for those users are set to the master password you defined at the beginning of the deployment process.</span></span> <span data-ttu-id="5be03-185">V dokumentaci jsou uvedeny další více funkční uživatelé s svá hesla, které můžete použít k přihlášení k systému nasazené.</span><span class="sxs-lookup"><span data-stu-id="5be03-185">In the documentation, other more functional users are listed with their passwords, which you can use to sign in to the deployed system.</span></span>

    ![Vítejte v dokumentaci k SAP](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

<span data-ttu-id="5be03-187">V rámci několik hodin je v pořádku systému integrovaného vývojového prostředí SAP nasazené v Azure.</span><span class="sxs-lookup"><span data-stu-id="5be03-187">Within a few hours, a healthy SAP IDES system is deployed in Azure.</span></span>

<span data-ttu-id="5be03-188">Pokud jste si zakoupili předplatné SAP CAL, SAP plně podporuje nasazení SAP CAL na Azure.</span><span class="sxs-lookup"><span data-stu-id="5be03-188">If you bought an SAP CAL subscription, SAP fully supports deployments through the SAP CAL on Azure.</span></span> <span data-ttu-id="5be03-189">Podpora fronta je BC. VCM CAL.</span><span class="sxs-lookup"><span data-stu-id="5be03-189">The support queue is BC-VCM-CAL.</span></span>

