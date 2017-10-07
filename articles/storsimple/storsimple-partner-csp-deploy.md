---
title: "aaaMicrosoft Azure StorSimple a přehled programu poskytovatele cloudových řešení | Microsoft Docs"
description: "Přehled o hello StorSimple a zprostředkovatele kryptografických služeb pro partnery služby StorSimple."
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
ms.openlocfilehash: b5d999f2fbb9a27e7404ff454957b29dbef56af6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a><span data-ttu-id="40ee6-103">Nasazení programu poskytovatele cloudových řešení pro pole virtuální zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="40ee6-103">Deploy StorSimple Virtual Array for Cloud Solution Provider Program</span></span>

## <a name="overview"></a><span data-ttu-id="40ee6-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="40ee6-104">Overview</span></span>

<span data-ttu-id="40ee6-105">Pole virtuální zařízení StorSimple se dá nasadit hello Cloud Solution Provider (CSP) partnery pro své zákazníky.</span><span class="sxs-lookup"><span data-stu-id="40ee6-105">StorSimple Virtual Array can be deployed by hello Cloud Solution Provider (CSP) partners for their customers.</span></span> <span data-ttu-id="40ee6-106">Zprostředkovatel kryptografických služeb partnera můžete vytvořit služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="40ee6-106">A CSP partner can create a StorSimple Device Manager service.</span></span> <span data-ttu-id="40ee6-107">Tato služba může být použité toodeploy a spravovat pole virtuální zařízení StorSimple a hello související sdílených složek, svazků a zálohování.</span><span class="sxs-lookup"><span data-stu-id="40ee6-107">This service can then be used toodeploy and manage StorSimple Virtual Array and hello associated shares, volumes, and backups.</span></span>

<span data-ttu-id="40ee6-108">Tento článek popisuje, jak můžete partnera CSP přidat zákazníka nebo nové předplatné tooan stávající zákazník a potom vytvořit toodeploy služby pole virtuální zařízení StorSimple v CSP.</span><span class="sxs-lookup"><span data-stu-id="40ee6-108">This article describes how a CSP partner can add a customer or a new subscription tooan existing customer and then create a service toodeploy a StorSimple Virtual Array in CSP.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40ee6-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="40ee6-109">Prerequisites</span></span>

<span data-ttu-id="40ee6-110">Než začnete, zajistěte, aby:</span><span class="sxs-lookup"><span data-stu-id="40ee6-110">Before you begin, ensure that:</span></span>

- <span data-ttu-id="40ee6-111">Jste zaregistrováni v rámci programu hello CSP.</span><span class="sxs-lookup"><span data-stu-id="40ee6-111">You are enrolled under hello CSP program.</span></span>
- <span data-ttu-id="40ee6-112">Máte platný [Partnerské centrum](http://partnercenter.microsoft.com/) přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="40ee6-112">You have valid [Partner Center](http://partnercenter.microsoft.com/) login credentials.</span></span> <span data-ttu-id="40ee6-113">Hello pověření umožňují toosign v toohello partnera portálu tooadd nové zákazníky, vyhledejte zákazníků nebo přejděte účtu zákazníka tooa z panelu Partner hello.</span><span class="sxs-lookup"><span data-stu-id="40ee6-113">hello credentials enable you toosign in toohello Partner portal tooadd new customers, search for customers, or navigate tooa customer account from hello Partner dashboard.</span></span> <span data-ttu-id="40ee6-114">Hello CSP, můžou fungovat jako správce StorSimple jménem zákazníka hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="40ee6-114">hello CSP can function as a StorSimple administrator on behalf of hello customer in hello Azure portal.</span></span>
                             
## <a name="add-a-customer"></a><span data-ttu-id="40ee6-115">Přidejte zákazníka</span><span class="sxs-lookup"><span data-stu-id="40ee6-115">Add a customer</span></span>

<span data-ttu-id="40ee6-116">Pokud přidáte zákazníka, se automaticky vytvoří předplatné.</span><span class="sxs-lookup"><span data-stu-id="40ee6-116">If you add a customer, a subscription is automatically created.</span></span> <span data-ttu-id="40ee6-117">tooadd zákazník (a automaticky vytvořit odběr), proveďte následující kroky v portálu pro partnery hello hello.</span><span class="sxs-lookup"><span data-stu-id="40ee6-117">tooadd a customer (and automatically create a subscription), perform hello following steps in hello Partner portal.</span></span>

1. <span data-ttu-id="40ee6-118">Přejděte toohello [Partnerské centrum](http://partnercenter.microsoft.com/) a přihlaste se pomocí svých přihlašovacích údajů CSP.</span><span class="sxs-lookup"><span data-stu-id="40ee6-118">Go toohello [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="40ee6-119">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-119">Click **Dashboard**.</span></span>

     ![Řídicí panel v partnerské Centrum](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="40ee6-121">V hello levém podokně, klikněte na **zákazníci**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-121">In hello left-pane, click **Customers**.</span></span> <span data-ttu-id="40ee6-122">V hello pravém podokně klikněte na **přidejte zákazníky**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-122">In hello right-pane, click **Add customers**.</span></span> <span data-ttu-id="40ee6-123">Zadejte podrobnosti o hello hello zákazníka.</span><span class="sxs-lookup"><span data-stu-id="40ee6-123">Enter hello details of hello customer.</span></span> <span data-ttu-id="40ee6-124">Klikněte na tlačítko **Další: odběry** toocreate předplatné zákazníka.</span><span class="sxs-lookup"><span data-stu-id="40ee6-124">Click **Next: Subscriptions** toocreate a customer subscription.</span></span>

    ![Přidat zákazníka](./media/storsimple-partner-csp-deploy/image2.png)

3.  <span data-ttu-id="40ee6-126">Vyberte **Microsoft Azure** nabízejí.</span><span class="sxs-lookup"><span data-stu-id="40ee6-126">Select **Microsoft Azure** offer.</span></span> <span data-ttu-id="40ee6-127">Posuv toohello dolní části stránky hello a klikněte na tlačítko **zkontrolujte**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-127">Scroll toohello bottom of hello page and click **Review**.</span></span>

    ![Přečtěte si informace o předplatném](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. <span data-ttu-id="40ee6-129">Zkontrolujte hello informace a klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-129">Review hello information and click **Submit**.</span></span>

    ![Odeslání předplatného](./media/storsimple-partner-csp-deploy/image4.png)

5. <span data-ttu-id="40ee6-131">Uložte informace o potvrzení hello pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="40ee6-131">Save hello confirmation information for future reference.</span></span>

    ![Uložit potvrzení](./media/storsimple-partner-csp-deploy/image5.png)

6. <span data-ttu-id="40ee6-133">Najít, nebo přejděte toohello zákazníka, které jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="40ee6-133">Find or navigate toohello customer you just added.</span></span> <span data-ttu-id="40ee6-134">Klikněte na tlačítko hello **název společnosti** toodrill dolů do hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="40ee6-134">Click hello **Company name** toodrill down into hello details.</span></span>

    ![Hledat zákazníka hello](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="40ee6-136">V hello levém podokně, vyberte **Správa služeb**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-136">In hello left-pane, select **Service management**.</span></span> <span data-ttu-id="40ee6-137">V hello pravém podokně v části **správě služeb**, klikněte na tlačítko **Microsoft Azure Management Portal** toosign v jako správce Azure pro vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="40ee6-137">In hello right-pane, under **Administer services**, click **Microsoft Azure Management Portal** toosign in as an Azure administrator for your customer.</span></span>

    ![Přihlaste se tooAzure portálu](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="40ee6-139">toocreate Správce zařízení StorSimple, klikněte na tlačítko **+ nový** a vyhledat nebo přejděte příliš**virtuální zařízení řady StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-139">toocreate a StorSimple Device Manager, click **+ New** and search for or navigate too**StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="40ee6-140">Další informace, přejděte příliš[nasazení služby StorSimple Manager zařízení](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="40ee6-140">For more information, go too[Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![Vytvořit službu StorSimple Manager zařízení](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a><span data-ttu-id="40ee6-142">Přidat předplatné</span><span class="sxs-lookup"><span data-stu-id="40ee6-142">Add a subscription</span></span>

<span data-ttu-id="40ee6-143">V některých případech může mít stávající zákazník a potřebujete tooadd předplatné.</span><span class="sxs-lookup"><span data-stu-id="40ee6-143">In some instances, you may have an existing customer, and you need tooadd a subscription.</span></span> <span data-ttu-id="40ee6-144">tooadd tooan předplatné stávající zákazník, proveďte následující kroky v portálu pro partnery hello hello.</span><span class="sxs-lookup"><span data-stu-id="40ee6-144">tooadd a subscription tooan existing customer, perform hello following steps in hello Partner portal.</span></span>

1. <span data-ttu-id="40ee6-145">Přejděte toohello [Partnerské centrum](http://partnercenter.microsoft.com/) a přihlaste se pomocí svých přihlašovacích údajů CSP.</span><span class="sxs-lookup"><span data-stu-id="40ee6-145">Go toohello [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="40ee6-146">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-146">Click **Dashboard**.</span></span>

     ![Řídicí panel v partnerské Centrum](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="40ee6-148">V hello levém podokně, klikněte na **zákazníci**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-148">In hello left-pane, click **Customers**.</span></span> <span data-ttu-id="40ee6-149">Najít, nebo přejděte toohello zákazníka, kterému chcete tooadd předplatné.</span><span class="sxs-lookup"><span data-stu-id="40ee6-149">Find or navigate toohello customer you want tooadd a subscription to.</span></span> <span data-ttu-id="40ee6-150">Klikněte na tlačítko hello ![ikona zaškrtnutí rozbalte](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) ikonu tooexpand hello řádek pro hello název společnosti pro vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="40ee6-150">Click hello ![Expand check icon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) icon tooexpand hello row for hello company name for your customer.</span></span> <span data-ttu-id="40ee6-151">V podrobnostech hello, klikněte na tlačítko **přidání předplatného**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-151">In hello details, click **Add subscriptions**.</span></span>

    ![Zákazníci](./media/storsimple-partner-csp-deploy/image10.png)

3. <span data-ttu-id="40ee6-153">Zkontrolujte **Microsoft Azure** pro hello **Top nabízí** hello předplatného a klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-153">Check **Microsoft Azure** for hello **Top offers** in hello subscription and click **Submit**.</span></span> <span data-ttu-id="40ee6-154">Tím se vytvoří nový odběr.</span><span class="sxs-lookup"><span data-stu-id="40ee6-154">This creates a new subscription.</span></span>

    ![Přidat nové předplatné](./media/storsimple-partner-csp-deploy/image11.png)

6. <span data-ttu-id="40ee6-156">Po vytvoření nové předplatné, klikněte na tlačítko **<--zákazníkům** v levém podokně tooreturn toohello hello **zákazníci** stránky.</span><span class="sxs-lookup"><span data-stu-id="40ee6-156">After a new subscription is created, click **<-- Customers** in hello left-pane tooreturn toohello **Customers** page.</span></span> <span data-ttu-id="40ee6-157">Vyhledejte hello zákazníka, pro kterého jste právě vytvořili předplatné.</span><span class="sxs-lookup"><span data-stu-id="40ee6-157">Search for hello customer for whom you just created a subscription.</span></span> <span data-ttu-id="40ee6-158">Klikněte na tlačítko hello **název společnosti** toodrill dolů do hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="40ee6-158">Click hello **Company name** toodrill down into hello details.</span></span>

    ![Hledat zákazníka hello](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="40ee6-160">V hello levém podokně, vyberte **Správa služeb**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-160">In hello left-pane, select **Service management**.</span></span> <span data-ttu-id="40ee6-161">V hello pravém podokně v části **správě služeb**, klikněte na tlačítko **Microsoft Azure Management Portal** toosign v jako správce Azure pro vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="40ee6-161">In hello right-pane, under **Administer services**, click **Microsoft Azure Management Portal** toosign in as an Azure administrator for your customer.</span></span>

    ![Přihlaste se tooAzure portálu](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="40ee6-163">toocreate Správce zařízení StorSimple, klikněte na tlačítko **+ nový** a vyhledat nebo přejděte příliš**virtuální zařízení řady StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="40ee6-163">toocreate a StorSimple Device Manager, click **+ New** and search for or navigate too**StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="40ee6-164">Další informace, přejděte příliš[nasazení služby StorSimple Manager zařízení](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="40ee6-164">For more information, go too[Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![Vytvořit službu StorSimple Manager zařízení](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a><span data-ttu-id="40ee6-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40ee6-166">Next steps</span></span>

- <span data-ttu-id="40ee6-167">Pokud máte další otázky ohledně hello StorSimple v CSP, přejděte příliš[StorSimple v CSP: Nejčastější dotazy](storsimple-partner-csp-faq.md).</span><span class="sxs-lookup"><span data-stu-id="40ee6-167">If you have more questions regarding hello StorSimple in CSP, go too[StorSimple in CSP: Frequently asked questions](storsimple-partner-csp-faq.md).</span></span>
- <span data-ttu-id="40ee6-168">Pokud jste připraveni toodeploy StorSimple, přejděte příliš[nasazení vaše zařízení StorSimple v CSP](storsimple-partner-csp-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="40ee6-168">If you are ready toodeploy your StorSimple, go too[Deploy your StorSimple in CSP](storsimple-partner-csp-deploy.md).</span></span>
