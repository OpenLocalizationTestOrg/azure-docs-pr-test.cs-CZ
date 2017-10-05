---
title: "Microsoft Azure StorSimple a poskytovatele cloudových řešení programu přehled | Microsoft Docs"
description: "Přehled o StorSimple a zprostředkovatele kryptografických služeb pro partnery služby StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/08/2017
ms.author: alkohli
ms.openlocfilehash: c8cb51093142146fc7d43b51a62d949f6cc38988
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a><span data-ttu-id="b653f-103">Nasazení programu poskytovatele cloudových řešení pro pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="b653f-103">Deploy StorSimple Virtual Array for Cloud Solution Provider Program</span></span>

## <a name="overview"></a><span data-ttu-id="b653f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b653f-104">Overview</span></span>

<span data-ttu-id="b653f-105">Pole virtuální zařízení StorSimple se dá nasadit partnery Cloud Solution Provider (CSP) pro své zákazníky.</span><span class="sxs-lookup"><span data-stu-id="b653f-105">StorSimple Virtual Array can be deployed by the Cloud Solution Provider (CSP) partners for their customers.</span></span> <span data-ttu-id="b653f-106">Zprostředkovatel kryptografických služeb partnera můžete vytvořit služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="b653f-106">A CSP partner can create a StorSimple Device Manager service.</span></span> <span data-ttu-id="b653f-107">Tato služba pak slouží k nasazení a správě pole virtuální zařízení StorSimple a přidružené sdílených složek, svazků a zálohování.</span><span class="sxs-lookup"><span data-stu-id="b653f-107">This service can then be used to deploy and manage StorSimple Virtual Array and the associated shares, volumes, and backups.</span></span>

<span data-ttu-id="b653f-108">Tento článek popisuje, jak přidat do stávající zákazník zákazníka nebo nové předplatné a pak vytvořit službu pro nasazení StorSimple virtuální pole v CSP partnera CSP.</span><span class="sxs-lookup"><span data-stu-id="b653f-108">This article describes how a CSP partner can add a customer or a new subscription to an existing customer and then create a service to deploy a StorSimple Virtual Array in CSP.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b653f-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b653f-109">Prerequisites</span></span>

<span data-ttu-id="b653f-110">Než začnete, zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="b653f-110">Before you begin, ensure that:</span></span>

- <span data-ttu-id="b653f-111">Jste zaregistrováni v rámci programu CSP.</span><span class="sxs-lookup"><span data-stu-id="b653f-111">You are enrolled under the CSP program.</span></span>
- <span data-ttu-id="b653f-112">Máte platný [Partnerské centrum](http://partnercenter.microsoft.com/) přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="b653f-112">You have valid [Partner Center](http://partnercenter.microsoft.com/) login credentials.</span></span> <span data-ttu-id="b653f-113">Přihlašovací údaje umožní se přihlásit k portálu pro partnery přidávat nové zákazníky, vyhledejte zákazníků nebo přejděte na účet zákazníka na řídicím panelu partnera.</span><span class="sxs-lookup"><span data-stu-id="b653f-113">The credentials enable you to sign in to the Partner portal to add new customers, search for customers, or navigate to a customer account from the Partner dashboard.</span></span> <span data-ttu-id="b653f-114">Zprostředkovatel kryptografických služeb, můžou fungovat jako správce StorSimple jménem zákazníka na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b653f-114">The CSP can function as a StorSimple administrator on behalf of the customer in the Azure portal.</span></span>
                             
## <a name="add-a-customer"></a><span data-ttu-id="b653f-115">Přidejte zákazníka</span><span class="sxs-lookup"><span data-stu-id="b653f-115">Add a customer</span></span>

<span data-ttu-id="b653f-116">Pokud přidáte zákazníka, se automaticky vytvoří předplatné.</span><span class="sxs-lookup"><span data-stu-id="b653f-116">If you add a customer, a subscription is automatically created.</span></span> <span data-ttu-id="b653f-117">Přidejte zákazníka (a automaticky vytvořit odběr), proveďte následující kroky v portálu pro partnery.</span><span class="sxs-lookup"><span data-stu-id="b653f-117">To add a customer (and automatically create a subscription), perform the following steps in the Partner portal.</span></span>

1. <span data-ttu-id="b653f-118">Přejděte na [Partnerské centrum](http://partnercenter.microsoft.com/) a přihlaste se pomocí svých přihlašovacích údajů CSP.</span><span class="sxs-lookup"><span data-stu-id="b653f-118">Go to the [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="b653f-119">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="b653f-119">Click **Dashboard**.</span></span>

     ![Řídicí panel v partnerské Centrum](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="b653f-121">V levém podokně klikněte na **zákazníci**.</span><span class="sxs-lookup"><span data-stu-id="b653f-121">In the left-pane, click **Customers**.</span></span> <span data-ttu-id="b653f-122">V pravém podokně klikněte na **přidejte zákazníky**.</span><span class="sxs-lookup"><span data-stu-id="b653f-122">In the right-pane, click **Add customers**.</span></span> <span data-ttu-id="b653f-123">Zadejte podrobnosti o zákazníka.</span><span class="sxs-lookup"><span data-stu-id="b653f-123">Enter the details of the customer.</span></span> <span data-ttu-id="b653f-124">Klikněte na tlačítko **Další: odběry** vytvořit odběr zákazníka.</span><span class="sxs-lookup"><span data-stu-id="b653f-124">Click **Next: Subscriptions** to create a customer subscription.</span></span>

    ![Přidat zákazníka](./media/storsimple-partner-csp-deploy/image2.png)

3.  <span data-ttu-id="b653f-126">Vyberte **Microsoft Azure** nabízejí.</span><span class="sxs-lookup"><span data-stu-id="b653f-126">Select **Microsoft Azure** offer.</span></span> <span data-ttu-id="b653f-127">Přejděte do dolní části stránky a klikněte na tlačítko **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="b653f-127">Scroll to the bottom of the page and click **Review**.</span></span>

    ![Přečtěte si informace o předplatném](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. <span data-ttu-id="b653f-129">Zkontrolujte zadané informace a klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="b653f-129">Review the information and click **Submit**.</span></span>

    ![Odeslání předplatného](./media/storsimple-partner-csp-deploy/image4.png)

5. <span data-ttu-id="b653f-131">Uložte informace o potvrzení pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="b653f-131">Save the confirmation information for future reference.</span></span>

    ![Uložit potvrzení](./media/storsimple-partner-csp-deploy/image5.png)

6. <span data-ttu-id="b653f-133">Najít nebo přejděte na Zákazník jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="b653f-133">Find or navigate to the customer you just added.</span></span> <span data-ttu-id="b653f-134">Klikněte **název společnosti** můžete rozbalit podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="b653f-134">Click the **Company name** to drill down into the details.</span></span>

    ![Hledat zákazníka](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="b653f-136">V levém podokně vyberte **Správa služeb**.</span><span class="sxs-lookup"><span data-stu-id="b653f-136">In the left-pane, select **Service management**.</span></span> <span data-ttu-id="b653f-137">V pravém podokně v části **správě služeb**, klikněte na tlačítko **Microsoft Azure Management Portal** se přihlásit jako správce Azure pro vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="b653f-137">In the right-pane, under **Administer services**, click **Microsoft Azure Management Portal** to sign in as an Azure administrator for your customer.</span></span>

    ![Přihlaste se k portálu Azure](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="b653f-139">Chcete-li vytvořit Správce zařízení StorSimple, klikněte na tlačítko **+ nový** a vyhledejte nebo přejděte na **virtuální zařízení řady StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="b653f-139">To create a StorSimple Device Manager, click **+ New** and search for or navigate to **StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="b653f-140">Další informace, přejděte na [nasazení služby StorSimple Manager zařízení](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="b653f-140">For more information, go to [Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![Vytvořit službu StorSimple Manager zařízení](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a><span data-ttu-id="b653f-142">Přidat předplatné</span><span class="sxs-lookup"><span data-stu-id="b653f-142">Add a subscription</span></span>

<span data-ttu-id="b653f-143">V některých případech může mít stávající zákazník a je nutné přidat předplatné.</span><span class="sxs-lookup"><span data-stu-id="b653f-143">In some instances, you may have an existing customer, and you need to add a subscription.</span></span> <span data-ttu-id="b653f-144">Pokud chcete přidat odběr do stávající zákazník, proveďte následující kroky v portálu pro partnery.</span><span class="sxs-lookup"><span data-stu-id="b653f-144">To add a subscription to an existing customer, perform the following steps in the Partner portal.</span></span>

1. <span data-ttu-id="b653f-145">Přejděte na [Partnerské centrum](http://partnercenter.microsoft.com/) a přihlaste se pomocí svých přihlašovacích údajů CSP.</span><span class="sxs-lookup"><span data-stu-id="b653f-145">Go to the [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="b653f-146">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="b653f-146">Click **Dashboard**.</span></span>

     ![Řídicí panel v partnerské Centrum](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="b653f-148">V levém podokně klikněte na **zákazníci**.</span><span class="sxs-lookup"><span data-stu-id="b653f-148">In the left-pane, click **Customers**.</span></span> <span data-ttu-id="b653f-149">Najít nebo přejděte zákazníkovi chcete přidat odběr.</span><span class="sxs-lookup"><span data-stu-id="b653f-149">Find or navigate to the customer you want to add a subscription to.</span></span> <span data-ttu-id="b653f-150">Klikněte ![ikona zaškrtnutí rozbalte](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) ikonu rozbalte řádek pro název společnosti pro vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="b653f-150">Click the ![Expand check icon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) icon to expand the row for the company name for your customer.</span></span> <span data-ttu-id="b653f-151">V podrobnostech, klikněte na tlačítko **přidání předplatného**.</span><span class="sxs-lookup"><span data-stu-id="b653f-151">In the details, click **Add subscriptions**.</span></span>

    ![Zákazníci](./media/storsimple-partner-csp-deploy/image10.png)

3. <span data-ttu-id="b653f-153">Zkontrolujte **Microsoft Azure** pro **Top nabízí** předplatného a klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="b653f-153">Check **Microsoft Azure** for the **Top offers** in the subscription and click **Submit**.</span></span> <span data-ttu-id="b653f-154">Tím se vytvoří nový odběr.</span><span class="sxs-lookup"><span data-stu-id="b653f-154">This creates a new subscription.</span></span>

    ![Přidat nové předplatné](./media/storsimple-partner-csp-deploy/image11.png)

6. <span data-ttu-id="b653f-156">Po vytvoření nové předplatné, klikněte na tlačítko **<--zákazníkům** v levém podokně se vraťte do **zákazníci** stránky.</span><span class="sxs-lookup"><span data-stu-id="b653f-156">After a new subscription is created, click **<-- Customers** in the left-pane to return to the **Customers** page.</span></span> <span data-ttu-id="b653f-157">Vyhledat zákazníka, pro kterého jste právě vytvořili předplatné.</span><span class="sxs-lookup"><span data-stu-id="b653f-157">Search for the customer for whom you just created a subscription.</span></span> <span data-ttu-id="b653f-158">Klikněte **název společnosti** můžete rozbalit podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="b653f-158">Click the **Company name** to drill down into the details.</span></span>

    ![Hledat zákazníka](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="b653f-160">V levém podokně vyberte **Správa služeb**.</span><span class="sxs-lookup"><span data-stu-id="b653f-160">In the left-pane, select **Service management**.</span></span> <span data-ttu-id="b653f-161">V pravém podokně v části **správě služeb**, klikněte na tlačítko **Microsoft Azure Management Portal** se přihlásit jako správce Azure pro vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="b653f-161">In the right-pane, under **Administer services**, click **Microsoft Azure Management Portal** to sign in as an Azure administrator for your customer.</span></span>

    ![Přihlaste se k portálu Azure](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="b653f-163">Chcete-li vytvořit Správce zařízení StorSimple, klikněte na tlačítko **+ nový** a vyhledejte nebo přejděte na **virtuální zařízení řady StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="b653f-163">To create a StorSimple Device Manager, click **+ New** and search for or navigate to **StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="b653f-164">Další informace, přejděte na [nasazení služby StorSimple Manager zařízení](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="b653f-164">For more information, go to [Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![Vytvořit službu StorSimple Manager zařízení](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a><span data-ttu-id="b653f-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b653f-166">Next steps</span></span>

- <span data-ttu-id="b653f-167">Pokud máte další otázky ohledně StorSimple v CSP, přejděte na [StorSimple v CSP: Nejčastější dotazy](storsimple-partner-csp-faq.md).</span><span class="sxs-lookup"><span data-stu-id="b653f-167">If you have more questions regarding the StorSimple in CSP, go to [StorSimple in CSP: Frequently asked questions](storsimple-partner-csp-faq.md).</span></span>
- <span data-ttu-id="b653f-168">Pokud jste připravení nasadit vaše zařízení StorSimple, přejděte na [nasazení vaše zařízení StorSimple v CSP](storsimple-partner-csp-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b653f-168">If you are ready to deploy your StorSimple, go to [Deploy your StorSimple in CSP](storsimple-partner-csp-deploy.md).</span></span>
