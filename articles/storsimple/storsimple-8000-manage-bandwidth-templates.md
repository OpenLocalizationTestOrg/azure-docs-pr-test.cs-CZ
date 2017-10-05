---
title: "Správa šířky pásma šablon pro řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak spravovat šablony StorSimple šířky pásma, které umožňují řídit šířku pásma."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 50d0a920bef097013feddc828d2c37133b9057b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-bandwidth-templates"></a><span data-ttu-id="ec049-103">Použít službu StorSimple Manager zařízení ke správě šablon šířky pásma zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="ec049-103">Use the StorSimple Device Manager service to manage StorSimple bandwidth templates</span></span>

## <a name="overview"></a><span data-ttu-id="ec049-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="ec049-104">Overview</span></span>

<span data-ttu-id="ec049-105">Šablony šířky pásma vám umožňují konfigurovat využití šířky pásma sítě v rámci více plánů času dne do vrstvy data ze zařízení StorSimple do cloudu.</span><span class="sxs-lookup"><span data-stu-id="ec049-105">Bandwidth templates allow you to configure network bandwidth usage across multiple time-of-day schedules to tier the data from the StorSimple device to the cloud.</span></span>

<span data-ttu-id="ec049-106">Plány omezení šířky pásma můžete:</span><span class="sxs-lookup"><span data-stu-id="ec049-106">With bandwidth throttling schedules you can:</span></span>

* <span data-ttu-id="ec049-107">Zadejte vlastní šířky pásma plány v závislosti na použití sítě zatížení.</span><span class="sxs-lookup"><span data-stu-id="ec049-107">Specify customized bandwidth schedules depending on the workload network usages.</span></span>
* <span data-ttu-id="ec049-108">Centralizovat správu a znovu použít plány v různých zařízeních snadné a plynulé způsobem.</span><span class="sxs-lookup"><span data-stu-id="ec049-108">Centralize management and reuse the schedules across multiple devices in an easy and seamless manner.</span></span>

> [!NOTE]
> <span data-ttu-id="ec049-109">Tato funkce je k dispozici pouze pro fyzického zařízení StorSimple (modelů 8100 a 8600) a ne pro zařízení StorSimple cloudu (modelů 8010 a 8020).</span><span class="sxs-lookup"><span data-stu-id="ec049-109">This feature is available only for StorSimple physical devices (models 8100 and 8600) and not for StorSimple Cloud Appliances (models 8010 and 8020).</span></span>


## <a name="the-bandwidth-templates-blade"></a><span data-ttu-id="ec049-110">V okně Šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-110">The Bandwidth templates blade</span></span>

<span data-ttu-id="ec049-111">**Šablony šířky pásma** okno má všechny šablony šířky pásma pro vaši službu v tabulkovém formátu a obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="ec049-111">The **Bandwidth templates** blade has all the bandwidth templates for your service in a tabular format, and contains the following information:</span></span>

* <span data-ttu-id="ec049-112">**Název** – přiřazené šablony šířky pásma při vytváření jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="ec049-112">**Name** – A unique name assigned to the bandwidth template when it was created.</span></span>
* <span data-ttu-id="ec049-113">**Plán** – počet plány, které jsou součástí šablony dané šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-113">**Schedule** – The number of schedules contained in a given bandwidth template.</span></span>
* <span data-ttu-id="ec049-114">**Používá** – počet svazků pomocí šablony šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-114">**Used by** – The number of volumes using the bandwidth templates.</span></span>

<span data-ttu-id="ec049-115">Další informace vám pomohou při konfiguraci šablon šířky pásma v můžete také najít:</span><span class="sxs-lookup"><span data-stu-id="ec049-115">You can also find additional information to help configure bandwidth templates in:</span></span>

* [<span data-ttu-id="ec049-116">Otázky a odpovědi týkající se šířky pásma šablony</span><span class="sxs-lookup"><span data-stu-id="ec049-116">Questions and answers about bandwidth templates</span></span>](#questions-and-answers-about-bandwidth-templates)
* [<span data-ttu-id="ec049-117">Doporučené postupy pro šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-117">Best practices for bandwidth templates</span></span>](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a><span data-ttu-id="ec049-118">Přidání šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-118">Add a bandwidth template</span></span>

<span data-ttu-id="ec049-119">Proveďte následující kroky k vytvoření nové šablony šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-119">Perform the following steps to create a new bandwidth template.</span></span>

#### <a name="to-add-a-bandwidth-template"></a><span data-ttu-id="ec049-120">Přidání šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-120">To add a bandwidth template</span></span>

1. <span data-ttu-id="ec049-121">Přejděte do služby StorSimple Manager zařízení, klikněte na tlačítko **šablony šířky pásma** a pak klikněte na **+ šablony šířky pásma přidat**.</span><span class="sxs-lookup"><span data-stu-id="ec049-121">Go to your StorSimple Device Manager service, click **Bandwidth templates** and then click **+ Add Bandwidth template**.</span></span>

    ![Klikněte na tlačítko + přidání šablony šířky pásma](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. <span data-ttu-id="ec049-123">V **přidat šablonu šířky pásma** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ec049-123">In the **Add bandwidth template** blade, do the following steps:</span></span>
   
    1. <span data-ttu-id="ec049-124">Zadejte jedinečný název pro šablonu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-124">Specify a unique name for your bandwidth template.</span></span>
    2. <span data-ttu-id="ec049-125">Definujte plán šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-125">Define a bandwidth schedule.</span></span> <span data-ttu-id="ec049-126">Vytvoření plánu:</span><span class="sxs-lookup"><span data-stu-id="ec049-126">To create a schedule:</span></span>
   
        1. <span data-ttu-id="ec049-127">V rozevíracím seznamu vyberte **dní** v týdnu je nakonfigurovaný plán pro.</span><span class="sxs-lookup"><span data-stu-id="ec049-127">From the drop-down list, choose the **Days** of the week the schedule is configured for.</span></span> <span data-ttu-id="ec049-128">Můžete vybrat více dní.</span><span class="sxs-lookup"><span data-stu-id="ec049-128">You can select multiple days.</span></span>        
        
        2. <span data-ttu-id="ec049-129">Zadejte **čas spuštění** v _hh: mm_ formátu.</span><span class="sxs-lookup"><span data-stu-id="ec049-129">Enter a **Start Time** in _hh:mm_ format.</span></span> <span data-ttu-id="ec049-130">To je, když bude zahájena podle plánu.</span><span class="sxs-lookup"><span data-stu-id="ec049-130">This is when the schedule will begin.</span></span>

        3. <span data-ttu-id="ec049-131">Zadejte **koncový čas** v _hh: mm_ formátu.</span><span class="sxs-lookup"><span data-stu-id="ec049-131">Enter an **End Time** in _hh:mm_ format.</span></span> <span data-ttu-id="ec049-132">To je, když se zastaví plán.</span><span class="sxs-lookup"><span data-stu-id="ec049-132">This is when the schedule will stop.</span></span>
      
           > [!NOTE]
           > <span data-ttu-id="ec049-133">Překrývající se plány nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="ec049-133">Overlapping schedules are not allowed.</span></span> <span data-ttu-id="ec049-134">Pokud počáteční a koncový čas způsobí překrývající se plán, zobrazí se chybová zpráva k tomuto účelu.</span><span class="sxs-lookup"><span data-stu-id="ec049-134">If the start and end times will result in an overlapping schedule, you will see an error message to that effect.</span></span>

        4. <span data-ttu-id="ec049-135">Zadejte **šířky pásma**.</span><span class="sxs-lookup"><span data-stu-id="ec049-135">Specify the **Bandwidth Rate**.</span></span> <span data-ttu-id="ec049-136">Toto je šířka pásma v MB za sekundu (Mbps) používané zařízení StorSimple v operací zahrnujících cloudu (nahrávání a stahování).</span><span class="sxs-lookup"><span data-stu-id="ec049-136">This is the bandwidth in Megabits per second (Mbps) used by your StorSimple device in operations involving the cloud (both uploads and downloads).</span></span> <span data-ttu-id="ec049-137">Zadejte číslo mezi 1 a 1000 u tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="ec049-137">Supply a number between 1 and 1,000 for this field.</span></span>

            ![Definujte plán šířky pásma](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            <span data-ttu-id="ec049-139">Zopakujte výše uvedené kroky k definování více plánů pro šablonu, až do dokončení.</span><span class="sxs-lookup"><span data-stu-id="ec049-139">Repeat the above steps to define multiple schedules for your template until you are done.</span></span>

        5. <span data-ttu-id="ec049-140">Klikněte na tlačítko **přidat** chcete začít vytvářet šablonu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-140">Click **Add** to start creating a bandwidth template.</span></span> <span data-ttu-id="ec049-141">Vytvořená šablona se přidá do seznamu šablon šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-141">The created template is added to the list of bandwidth templates.</span></span>
      

## <a name="edit-a-bandwidth-template"></a><span data-ttu-id="ec049-142">Upravit šablonu šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-142">Edit a bandwidth template</span></span>

<span data-ttu-id="ec049-143">Proveďte následující kroky, chcete-li upravit šablonu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-143">Perform the following steps to edit a bandwidth template.</span></span>

### <a name="to-edit-a-bandwidth-template"></a><span data-ttu-id="ec049-144">Chcete-li upravit šablonu šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-144">To edit a bandwidth template</span></span>

1. <span data-ttu-id="ec049-145">Přejděte k vaší Správce zařízení StorSimple služby a klikněte na tlačítko **šablony šířky pásma**.</span><span class="sxs-lookup"><span data-stu-id="ec049-145">Go to your StorSimple Device Manager service and click **Bandwidth templates**.</span></span>
2. <span data-ttu-id="ec049-146">V seznamu šablon šířky pásma vyberte šablonu, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="ec049-146">In the list of bandwidth templates, select the template you wish to delete.</span></span> <span data-ttu-id="ec049-147">Klikněte pravým tlačítkem myši a v místní nabídce vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="ec049-147">Right-click and from the context menu, select **Delete**.</span></span>
3. <span data-ttu-id="ec049-148">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec049-148">When prompted for confirmation, click **OK**.</span></span> <span data-ttu-id="ec049-149">To by měl odstranit šablonu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-149">This should delete the bandwidth template.</span></span> 
4. <span data-ttu-id="ec049-150">Seznam aktualizací šablony šířky pásma, aby odrážela odstranění.</span><span class="sxs-lookup"><span data-stu-id="ec049-150">The list of bandwidth templates updates to reflect the deletion.</span></span>

> [!NOTE]
> <span data-ttu-id="ec049-151">Vaše změny nelze uložit, pokud upravená plánu překrývá s existující plán v šabloně šířky pásma, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="ec049-151">You cannot save your changes if the edited schedule overlaps with an existing schedule in the bandwidth template that you are modifying.</span></span>

## <a name="delete-a-bandwidth-template"></a><span data-ttu-id="ec049-152">Odstranit šablonu šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-152">Delete a bandwidth template</span></span>

<span data-ttu-id="ec049-153">Proveďte následující kroky k odstranění šablony šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-153">Perform the following steps to delete a bandwidth template.</span></span>

#### <a name="to-delete-a-bandwidth-template"></a><span data-ttu-id="ec049-154">Chcete-li odstranit šablonu šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-154">To delete a bandwidth template</span></span>

1. <span data-ttu-id="ec049-155">Přejděte k vaší Správce zařízení StorSimple služby a klikněte na tlačítko **šablony šířky pásma**.</span><span class="sxs-lookup"><span data-stu-id="ec049-155">Go to your StorSimple Device Manager service and click **Bandwidth templates**.</span></span>
2. <span data-ttu-id="ec049-156">V seznamu šablon šířky pásma vyberte šablonu, kterou chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="ec049-156">In the list of bandwidth templates, select the template you wish to delete.</span></span> <span data-ttu-id="ec049-157">Klikněte pravým tlačítkem myši a v místní nabídce vyberte příkaz odstranit.</span><span class="sxs-lookup"><span data-stu-id="ec049-157">Right-click and from the context menu, select Delete.</span></span>
3. <span data-ttu-id="ec049-158">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec049-158">When prompted for confirmation, click **OK**.</span></span> <span data-ttu-id="ec049-159">To by měl odstranit šablonu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-159">This should delete the bandwidth template.</span></span>
4. <span data-ttu-id="ec049-160">Seznam aktualizací šablony šířky pásma, aby odrážela odstranění.</span><span class="sxs-lookup"><span data-stu-id="ec049-160">The list of bandwidth templates updates to reflect the deletion.</span></span>

<span data-ttu-id="ec049-161">Pokud šablona se používá ve všech svazcích, nebude možné jej odstranit.</span><span class="sxs-lookup"><span data-stu-id="ec049-161">If the template is in use by any volume(s), you will not be allowed to delete it.</span></span> <span data-ttu-id="ec049-162">Zobrazí se chybová zpráva oznamující, že šablona se používá.</span><span class="sxs-lookup"><span data-stu-id="ec049-162">You will see an error message indicating that the template is in use.</span></span> <span data-ttu-id="ec049-163">Dialogové okno chyby zpráva se zobrazí radí, že by měl odstranit všechny odkazy na šablony.</span><span class="sxs-lookup"><span data-stu-id="ec049-163">An error message dialog box will appear advising you that all the references to the template should be removed.</span></span>

<span data-ttu-id="ec049-164">Můžete odstranit všechny odkazy na šablony přímým přístupem **kontejnery svazků** stránky a úprava kontejnery svazků, které tuto šablonu použít tak, aby použít jinou šablonu nebo použít nastavení vlastní nebo neomezená šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-164">You can delete all the references to the template by accessing the **Volume Containers** page and modifying the volume containers that use this template so that they use another template or use a custom or unlimited bandwidth setting.</span></span> <span data-ttu-id="ec049-165">Pokud byly odebrány všechny odkazy, můžete odstranit šablonu.</span><span class="sxs-lookup"><span data-stu-id="ec049-165">When all the references have been removed, you can delete the template.</span></span>

## <a name="use-a-default-bandwidth-template"></a><span data-ttu-id="ec049-166">Použít výchozí šablonu šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-166">Use a default bandwidth template</span></span>

<span data-ttu-id="ec049-167">Výchozí šablony šířky pásma je k dispozici a je používána kontejnery svazků ve výchozím nastavení k vynucení řízení šířky pásma při přístupu k cloudu.</span><span class="sxs-lookup"><span data-stu-id="ec049-167">A default bandwidth template is provided and is used by volume containers by default to enforce bandwidth controls when accessing the cloud.</span></span> <span data-ttu-id="ec049-168">Výchozí šablony slouží také jako připravené odkaz pro uživatele, kteří vytvořit své vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="ec049-168">The default template also serves as a ready reference for users who create their own templates.</span></span> <span data-ttu-id="ec049-169">Podrobnosti o této výchozí šablony jsou:</span><span class="sxs-lookup"><span data-stu-id="ec049-169">The details of this default template are:</span></span>

* <span data-ttu-id="ec049-170">**Název** – neomezená noci a víkendů</span><span class="sxs-lookup"><span data-stu-id="ec049-170">**Name** – Unlimited nights and weekends</span></span>
* <span data-ttu-id="ec049-171">**Plán** – jeden plán od pondělí až pátek, která se vztahuje šířky pásma 1 MB/s mezi 8: 00 a 17: 00 času.</span><span class="sxs-lookup"><span data-stu-id="ec049-171">**Schedule** – A single schedule from Monday to Friday that applies a bandwidth rate of 1 Mbps between 8 AM and 5 PM device time.</span></span> <span data-ttu-id="ec049-172">Šířka pásma je nastavena na neomezený zbytek v týdnu.</span><span class="sxs-lookup"><span data-stu-id="ec049-172">The bandwidth is set to Unlimited for the remainder of the week.</span></span>

<span data-ttu-id="ec049-173">Můžete upravit výchozí šablony.</span><span class="sxs-lookup"><span data-stu-id="ec049-173">The default template can be edited.</span></span> <span data-ttu-id="ec049-174">Použití této šablony (včetně upravená verze) sledován.</span><span class="sxs-lookup"><span data-stu-id="ec049-174">The usage of this template (including edited versions) is tracked.</span></span>

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a><span data-ttu-id="ec049-175">Vytvořit šablonu celodenní šířky pásma, která se spouští v zadanou dobu</span><span class="sxs-lookup"><span data-stu-id="ec049-175">Create an all-day bandwidth template that starts at a specified time</span></span>

<span data-ttu-id="ec049-176">Tento postup slouží k vytvoření plánu, který se spustí v zadaný čas a spustí celý den.</span><span class="sxs-lookup"><span data-stu-id="ec049-176">Follow this procedure to create a schedule that starts at a specified time and runs all day.</span></span> <span data-ttu-id="ec049-177">V příkladu plán začíná na 9: 00 ráno a spouští až 9: 00 ráno Další.</span><span class="sxs-lookup"><span data-stu-id="ec049-177">In the example, the schedule starts at 9 AM in the morning and runs until 9 AM the next morning.</span></span> <span data-ttu-id="ec049-178">Je důležité si uvědomit, že počáteční a koncový čas pro dané plán musí oba být obsaženy na stejný plán 24 hodin a nesmí zahrnovat více dní.</span><span class="sxs-lookup"><span data-stu-id="ec049-178">It's important to note that the start and end times for a given schedule must both be contained on the same 24 hour schedule and cannot span multiple days.</span></span> <span data-ttu-id="ec049-179">Pokud potřebujete nastavit šablony šířky pásma, které jsou rozmístěny v několika dní, budete se muset použít více plánů (jak je znázorněno v příkladu).</span><span class="sxs-lookup"><span data-stu-id="ec049-179">If you need to set up bandwidth templates that span multiple days, you will need to use multiple schedules (as shown in the example).</span></span>

#### <a name="to-create-an-all-day-bandwidth-template"></a><span data-ttu-id="ec049-180">Chcete-li vytvořit šablonu celodenní šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-180">To create an all-day bandwidth template</span></span>

1. <span data-ttu-id="ec049-181">Vytvořte plán, který začíná v 9: 00 ráno a spouští dokud půlnoci.</span><span class="sxs-lookup"><span data-stu-id="ec049-181">Create a schedule that starts at 9 AM in the morning and runs until midnight.</span></span>
2. <span data-ttu-id="ec049-182">Přidejte jiný plán.</span><span class="sxs-lookup"><span data-stu-id="ec049-182">Add another schedule.</span></span> <span data-ttu-id="ec049-183">Nakonfigurujte druhý plán pro spouštění z půlnoc až 9: 00 ráno.</span><span class="sxs-lookup"><span data-stu-id="ec049-183">Configure the second schedule to run from midnight until 9 AM in the morning.</span></span>
3. <span data-ttu-id="ec049-184">Uložte šablonu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-184">Save the bandwidth template.</span></span>

<span data-ttu-id="ec049-185">Složený plán bude poté otevřete současně dle vašeho výběru a spusťte celodenní.</span><span class="sxs-lookup"><span data-stu-id="ec049-185">The composite schedule will then start at a time of your choosing and run all-day.</span></span>

## <a name="questions-and-answers-about-bandwidth-templates"></a><span data-ttu-id="ec049-186">Otázky a odpovědi týkající se šířky pásma šablony</span><span class="sxs-lookup"><span data-stu-id="ec049-186">Questions and answers about bandwidth templates</span></span>

<span data-ttu-id="ec049-187">**Q**.</span><span class="sxs-lookup"><span data-stu-id="ec049-187">**Q**.</span></span> <span data-ttu-id="ec049-188">Co se stane řízení šířky pásma, když jsou mezi plány?</span><span class="sxs-lookup"><span data-stu-id="ec049-188">What happens to bandwidth controls when you are in between the schedules?</span></span> <span data-ttu-id="ec049-189">(Skončila plánu a jiný se ještě nespustilo.)</span><span class="sxs-lookup"><span data-stu-id="ec049-189">(A schedule has ended and another one has not started yet.)</span></span>

<span data-ttu-id="ec049-190">**A**.</span><span class="sxs-lookup"><span data-stu-id="ec049-190">**A**.</span></span> <span data-ttu-id="ec049-191">V takových případech bude těmto nekompatibilitám bez řízení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-191">In such cases, no bandwidth controls will be employed.</span></span> <span data-ttu-id="ec049-192">To znamená, že zařízení použít neomezenou šířku pásma při vrstvení data do cloudu.</span><span class="sxs-lookup"><span data-stu-id="ec049-192">This means that the device can use unlimited bandwidth when tiering data to the cloud.</span></span>

<span data-ttu-id="ec049-193">**Q**.</span><span class="sxs-lookup"><span data-stu-id="ec049-193">**Q**.</span></span> <span data-ttu-id="ec049-194">Můžete upravit šablony šířky pásma na zařízení s offline?</span><span class="sxs-lookup"><span data-stu-id="ec049-194">Can you modify bandwidth templates on an offline device?</span></span>

<span data-ttu-id="ec049-195">**A**.</span><span class="sxs-lookup"><span data-stu-id="ec049-195">**A**.</span></span> <span data-ttu-id="ec049-196">Nebudete moct změnit šablony šířky pásma na kontejnery svazků, pokud odpovídající zařízení je offline.</span><span class="sxs-lookup"><span data-stu-id="ec049-196">You will not be able to modify bandwidth templates on volumes containers if the corresponding device is offline.</span></span>

<span data-ttu-id="ec049-197">**Q**.</span><span class="sxs-lookup"><span data-stu-id="ec049-197">**Q**.</span></span> <span data-ttu-id="ec049-198">Můžete upravit šablonu šířky pásma přidružené kontejner svazků, pokud přidružené svazky jsou offline?</span><span class="sxs-lookup"><span data-stu-id="ec049-198">Can you edit a bandwidth template associated with a volume container when the associated volumes are offline?</span></span>

<span data-ttu-id="ec049-199">**A**.</span><span class="sxs-lookup"><span data-stu-id="ec049-199">**A**.</span></span> <span data-ttu-id="ec049-200">Můžete upravit šablonu šířky pásma přidružené kontejner svazků, jejichž svazky jsou offline.</span><span class="sxs-lookup"><span data-stu-id="ec049-200">You can modify a bandwidth template associated with a volume container whose volumes are offline.</span></span> <span data-ttu-id="ec049-201">Všimněte si, že po offline svazky se žádná data budou být rozvrstvena ze zařízení do cloudu.</span><span class="sxs-lookup"><span data-stu-id="ec049-201">Note that when volumes are offline, no data will be tiered from the device to the cloud.</span></span>

<span data-ttu-id="ec049-202">**Q**.</span><span class="sxs-lookup"><span data-stu-id="ec049-202">**Q**.</span></span> <span data-ttu-id="ec049-203">Můžete odstranit výchozí šablonu?</span><span class="sxs-lookup"><span data-stu-id="ec049-203">Can you delete a default template?</span></span>

<span data-ttu-id="ec049-204">**A**.</span><span class="sxs-lookup"><span data-stu-id="ec049-204">**A**.</span></span> <span data-ttu-id="ec049-205">I když můžete odstranit výchozí šablonu, není vhodné učinit.</span><span class="sxs-lookup"><span data-stu-id="ec049-205">Although you can delete a default template, it is not a good idea to do so.</span></span> <span data-ttu-id="ec049-206">Použití výchozí šablony, včetně upravená verze sledován.</span><span class="sxs-lookup"><span data-stu-id="ec049-206">The usage of a default template, including edited versions, is tracked.</span></span> <span data-ttu-id="ec049-207">Sledování dat se analyzují a v průběhu času, které se používají ke zlepšení výchozí šablony.</span><span class="sxs-lookup"><span data-stu-id="ec049-207">The tracking data is analyzed and over the course of time, is used to improve the default template.</span></span>

<span data-ttu-id="ec049-208">**Q**.</span><span class="sxs-lookup"><span data-stu-id="ec049-208">**Q**.</span></span> <span data-ttu-id="ec049-209">Jak můžete určit, jestli vaše šablony šířky pásma, který je potřeba upravit?</span><span class="sxs-lookup"><span data-stu-id="ec049-209">How do you determine that your bandwidth templates need to be modified?</span></span>

<span data-ttu-id="ec049-210">**A**.</span><span class="sxs-lookup"><span data-stu-id="ec049-210">**A**.</span></span> <span data-ttu-id="ec049-211">Jeden z příznaků, je třeba upravit šablony šířky pásma je při spuštění zobrazuje sítě zpomalit nebo vyseknutí několikrát za den.</span><span class="sxs-lookup"><span data-stu-id="ec049-211">One of the signs that you need to modify the bandwidth templates is when you start seeing the network slow down or choke multiple times in a day.</span></span> <span data-ttu-id="ec049-212">Pokud k tomu dojde, sledujte prohlížením grafy vstupně-výstupní výkon a propustnost sítě úložiště a využití sítě.</span><span class="sxs-lookup"><span data-stu-id="ec049-212">If this happens, monitor the storage and usage network by looking at the I/O Performance and Network Throughput charts.</span></span>

<span data-ttu-id="ec049-213">Z dat propustnost sítě určete čas, den a kontejnery svazků, ve kterých dojde k přetížení sítě.</span><span class="sxs-lookup"><span data-stu-id="ec049-213">From the network throughput data, identify the time of day and the volume containers in which the network bottleneck occurs.</span></span> <span data-ttu-id="ec049-214">Pokud to se stane, když probíhá vrstveny data do cloudu (získat tyto informace z vstupně-výstupní výkon pro všechny kontejnery svazků pro zařízení do cloudu), pak budete muset upravit šablony šířky pásma přidružené k vaší kontejnery svazků.</span><span class="sxs-lookup"><span data-stu-id="ec049-214">If this happens when data is being tiered to the cloud (get this information from I/O performance for all volume containers for device to cloud), then you will need to modify the bandwidth templates associated with your volume containers.</span></span>

<span data-ttu-id="ec049-215">Po změny šablon se používají, musíte se ke sledování sítě znovu pro významné latenci.</span><span class="sxs-lookup"><span data-stu-id="ec049-215">After the modified templates are in use, you will need to monitor the network again for significant latencies.</span></span> <span data-ttu-id="ec049-216">Pokud tyto stále existují, pak bude muset pokroku šablony šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="ec049-216">If these still exist, then you will need to revisit your bandwidth templates.</span></span>

<span data-ttu-id="ec049-217">**Q**.</span><span class="sxs-lookup"><span data-stu-id="ec049-217">**Q**.</span></span> <span data-ttu-id="ec049-218">Co se stane, pokud máte více kontejnery svazků na zařízení plány této překrývají, ale jiné omezení se vztahují na všechny?</span><span class="sxs-lookup"><span data-stu-id="ec049-218">What happens if multiple volume containers on my device have schedules that overlap but different limits apply to each?</span></span>

<span data-ttu-id="ec049-219">**A**.</span><span class="sxs-lookup"><span data-stu-id="ec049-219">**A**.</span></span> <span data-ttu-id="ec049-220">Předpokládejme, že máte zařízení s 3 kontejnery svazků.</span><span class="sxs-lookup"><span data-stu-id="ec049-220">Let's assume that you have a device with 3 volume containers.</span></span> <span data-ttu-id="ec049-221">Plány přidružené k těmto kontejnerům úplně překrývat.</span><span class="sxs-lookup"><span data-stu-id="ec049-221">The schedules associated with these containers completely overlap.</span></span> <span data-ttu-id="ec049-222">Pro každý z těchto kontejnerů omezení šířky pásma použít jsou 5, 10 až 15 MB/s v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="ec049-222">For each of these containers, the bandwidth limits used are 5, 10, and 15 Mbps respectively.</span></span> <span data-ttu-id="ec049-223">Při vstupně-výstupních operací dochází na všech těchto kontejnerů ve stejnou dobu, může být použitá minimálně 3 omezení šířky pásma: v tomto případě 5 MB/s jako tyto odchozí vstupně-výstupní požadavky sdílet stejnou frontou.</span><span class="sxs-lookup"><span data-stu-id="ec049-223">When I/O are occurring on all of these containers at the same time, the minimum of the 3 bandwidth limits may be applied: in this case, 5 Mbps as these outgoing I/O requests share the same queue.</span></span>

## <a name="best-practices-for-bandwidth-templates"></a><span data-ttu-id="ec049-224">Doporučené postupy pro šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="ec049-224">Best practices for bandwidth templates</span></span>

<span data-ttu-id="ec049-225">Použijte tyto osvědčené postupy pro zařízení StorSimple:</span><span class="sxs-lookup"><span data-stu-id="ec049-225">Follow these best practices for your StorSimple device:</span></span>

* <span data-ttu-id="ec049-226">Konfigurace šablon šířky pásma na svém zařízení povolit proměnné omezení propustnosti síťových zařízení v různých časech dne.</span><span class="sxs-lookup"><span data-stu-id="ec049-226">Configure bandwidth templates on your device to enable variable throttling of the network throughput by the device at different times of the day.</span></span> <span data-ttu-id="ec049-227">Tyto šablony šířky pásma při použití s plánů zálohování můžete efektivně využívat další šířku pásma sítě pro operace cloudu mimo špičku.</span><span class="sxs-lookup"><span data-stu-id="ec049-227">These bandwidth templates when used with backup schedules can effectively leverage additional network bandwidth for cloud operations during off-peak hours.</span></span>
* <span data-ttu-id="ec049-228">Vypočítejte skutečně nezbytná šířka pásma pro konkrétní nasazení podle velikosti nasazení a plánovanou dobu požadované obnovení (RTO).</span><span class="sxs-lookup"><span data-stu-id="ec049-228">Calculate the actual bandwidth required for a particular deployment based on the size of the deployment and the required recovery time objective (RTO).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec049-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec049-229">Next steps</span></span>

<span data-ttu-id="ec049-230">Další informace o [pomocí služby StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="ec049-230">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

