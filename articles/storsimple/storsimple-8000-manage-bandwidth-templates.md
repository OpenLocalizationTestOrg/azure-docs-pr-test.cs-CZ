---
title: "šablony aaaManage šířky pásma pro řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak toomanage StorSimple šířky pásma šablony, které umožňují toocontrol šířku pásma."
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
ms.openlocfilehash: ebcd1824d7bb9e4c235194c04edbfe8001a3e794
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-bandwidth-templates"></a><span data-ttu-id="b7223-103">Použití hello Správce zařízení StorSimple služby toomanage StorSimple šířky pásma šablon</span><span class="sxs-lookup"><span data-stu-id="b7223-103">Use hello StorSimple Device Manager service toomanage StorSimple bandwidth templates</span></span>

## <a name="overview"></a><span data-ttu-id="b7223-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b7223-104">Overview</span></span>

<span data-ttu-id="b7223-105">Šablony šířky pásma povolit využití šířky pásma sítě tooconfigure napříč více času dne plány tootier hello daty z hello cloudu toohello zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b7223-105">Bandwidth templates allow you tooconfigure network bandwidth usage across multiple time-of-day schedules tootier hello data from hello StorSimple device toohello cloud.</span></span>

<span data-ttu-id="b7223-106">Plány omezení šířky pásma můžete:</span><span class="sxs-lookup"><span data-stu-id="b7223-106">With bandwidth throttling schedules you can:</span></span>

* <span data-ttu-id="b7223-107">Zadejte vlastní šířky pásma plány v závislosti na použití sítě zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="b7223-107">Specify customized bandwidth schedules depending on hello workload network usages.</span></span>
* <span data-ttu-id="b7223-108">Centralizovat správu a znovu použít plány hello mezi různými zařízeními snadné a plynulé způsobem.</span><span class="sxs-lookup"><span data-stu-id="b7223-108">Centralize management and reuse hello schedules across multiple devices in an easy and seamless manner.</span></span>

> [!NOTE]
> <span data-ttu-id="b7223-109">Tato funkce je k dispozici pouze pro fyzického zařízení StorSimple (modelů 8100 a 8600) a ne pro zařízení StorSimple cloudu (modelů 8010 a 8020).</span><span class="sxs-lookup"><span data-stu-id="b7223-109">This feature is available only for StorSimple physical devices (models 8100 and 8600) and not for StorSimple Cloud Appliances (models 8010 and 8020).</span></span>


## <a name="hello-bandwidth-templates-blade"></a><span data-ttu-id="b7223-110">okno šablony šířky pásma Hello</span><span class="sxs-lookup"><span data-stu-id="b7223-110">hello Bandwidth templates blade</span></span>

<span data-ttu-id="b7223-111">Hello **šablony šířky pásma** okno má všechny hello šířky pásma šablony služby v tabulkovém formátu a obsahuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="b7223-111">hello **Bandwidth templates** blade has all hello bandwidth templates for your service in a tabular format, and contains hello following information:</span></span>

* <span data-ttu-id="b7223-112">**Název** – při vytváření šablony šířky pásma toohello přiřazen jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="b7223-112">**Name** – A unique name assigned toohello bandwidth template when it was created.</span></span>
* <span data-ttu-id="b7223-113">**Plán** – hello počet plány, které jsou součástí šablony dané šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-113">**Schedule** – hello number of schedules contained in a given bandwidth template.</span></span>
* <span data-ttu-id="b7223-114">**Používá** – hello počtu svazků pomocí šablon hello šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-114">**Used by** – hello number of volumes using hello bandwidth templates.</span></span>

<span data-ttu-id="b7223-115">Taky můžete najít další informace o toohelp nakonfigurovat šablony šířky pásma v:</span><span class="sxs-lookup"><span data-stu-id="b7223-115">You can also find additional information toohelp configure bandwidth templates in:</span></span>

* [<span data-ttu-id="b7223-116">Otázky a odpovědi týkající se šířky pásma šablony</span><span class="sxs-lookup"><span data-stu-id="b7223-116">Questions and answers about bandwidth templates</span></span>](#questions-and-answers-about-bandwidth-templates)
* [<span data-ttu-id="b7223-117">Doporučené postupy pro šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-117">Best practices for bandwidth templates</span></span>](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a><span data-ttu-id="b7223-118">Přidání šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-118">Add a bandwidth template</span></span>

<span data-ttu-id="b7223-119">Proveďte následující kroky toocreate novou šablonu šířky pásma hello.</span><span class="sxs-lookup"><span data-stu-id="b7223-119">Perform hello following steps toocreate a new bandwidth template.</span></span>

#### <a name="tooadd-a-bandwidth-template"></a><span data-ttu-id="b7223-120">tooadd šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-120">tooadd a bandwidth template</span></span>

1. <span data-ttu-id="b7223-121">Přejděte tooyour service Manager zařízení StorSimple, klikněte na tlačítko **šablony šířky pásma** a pak klikněte na **+ šablony šířky pásma přidat**.</span><span class="sxs-lookup"><span data-stu-id="b7223-121">Go tooyour StorSimple Device Manager service, click **Bandwidth templates** and then click **+ Add Bandwidth template**.</span></span>

    ![Klikněte na tlačítko + přidání šablony šířky pásma](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. <span data-ttu-id="b7223-123">V hello **přidat šablonu šířky pásma** okně hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b7223-123">In hello **Add bandwidth template** blade, do hello following steps:</span></span>
   
    1. <span data-ttu-id="b7223-124">Zadejte jedinečný název pro šablonu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-124">Specify a unique name for your bandwidth template.</span></span>
    2. <span data-ttu-id="b7223-125">Definujte plán šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-125">Define a bandwidth schedule.</span></span> <span data-ttu-id="b7223-126">toocreate plánu:</span><span class="sxs-lookup"><span data-stu-id="b7223-126">toocreate a schedule:</span></span>
   
        1. <span data-ttu-id="b7223-127">Z rozevíracího seznamu hello vyberte hello **dní** hello týden hello plán je nakonfigurovaný pro.</span><span class="sxs-lookup"><span data-stu-id="b7223-127">From hello drop-down list, choose hello **Days** of hello week hello schedule is configured for.</span></span> <span data-ttu-id="b7223-128">Můžete vybrat více dní.</span><span class="sxs-lookup"><span data-stu-id="b7223-128">You can select multiple days.</span></span>        
        
        2. <span data-ttu-id="b7223-129">Zadejte **čas spuštění** v _hh: mm_ formátu.</span><span class="sxs-lookup"><span data-stu-id="b7223-129">Enter a **Start Time** in _hh:mm_ format.</span></span> <span data-ttu-id="b7223-130">To je, když bude zahájena hello plán.</span><span class="sxs-lookup"><span data-stu-id="b7223-130">This is when hello schedule will begin.</span></span>

        3. <span data-ttu-id="b7223-131">Zadejte **koncový čas** v _hh: mm_ formátu.</span><span class="sxs-lookup"><span data-stu-id="b7223-131">Enter an **End Time** in _hh:mm_ format.</span></span> <span data-ttu-id="b7223-132">To je, když se zastaví hello plán.</span><span class="sxs-lookup"><span data-stu-id="b7223-132">This is when hello schedule will stop.</span></span>
      
           > [!NOTE]
           > <span data-ttu-id="b7223-133">Překrývající se plány nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="b7223-133">Overlapping schedules are not allowed.</span></span> <span data-ttu-id="b7223-134">Pokud hello počáteční a koncový čas způsobí překrývající se plán, zobrazí se chybová zpráva toothat vliv.</span><span class="sxs-lookup"><span data-stu-id="b7223-134">If hello start and end times will result in an overlapping schedule, you will see an error message toothat effect.</span></span>

        4. <span data-ttu-id="b7223-135">Zadejte hello **šířky pásma**.</span><span class="sxs-lookup"><span data-stu-id="b7223-135">Specify hello **Bandwidth Rate**.</span></span> <span data-ttu-id="b7223-136">Toto je hello šířky pásma v MB za sekundu (Mbps) používané zařízení StorSimple v operací zahrnujících hello cloudu (nahrávání a stahování).</span><span class="sxs-lookup"><span data-stu-id="b7223-136">This is hello bandwidth in Megabits per second (Mbps) used by your StorSimple device in operations involving hello cloud (both uploads and downloads).</span></span> <span data-ttu-id="b7223-137">Zadejte číslo mezi 1 a 1000 u tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="b7223-137">Supply a number between 1 and 1,000 for this field.</span></span>

            ![Definujte plán šířky pásma](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            <span data-ttu-id="b7223-139">Opakujte hello výše kroky toodefine více plánů pro šablony až do dokončení.</span><span class="sxs-lookup"><span data-stu-id="b7223-139">Repeat hello above steps toodefine multiple schedules for your template until you are done.</span></span>

        5. <span data-ttu-id="b7223-140">Klikněte na tlačítko **přidat** toostart vytváření šablony šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-140">Click **Add** toostart creating a bandwidth template.</span></span> <span data-ttu-id="b7223-141">Hello vytvořil šablonu se přidá toohello seznam šablon šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-141">hello created template is added toohello list of bandwidth templates.</span></span>
      

## <a name="edit-a-bandwidth-template"></a><span data-ttu-id="b7223-142">Upravit šablonu šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-142">Edit a bandwidth template</span></span>

<span data-ttu-id="b7223-143">Proveďte následující kroky tooedit šablony šířky pásma hello.</span><span class="sxs-lookup"><span data-stu-id="b7223-143">Perform hello following steps tooedit a bandwidth template.</span></span>

### <a name="tooedit-a-bandwidth-template"></a><span data-ttu-id="b7223-144">tooedit šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-144">tooedit a bandwidth template</span></span>

1. <span data-ttu-id="b7223-145">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **šablony šířky pásma**.</span><span class="sxs-lookup"><span data-stu-id="b7223-145">Go tooyour StorSimple Device Manager service and click **Bandwidth templates**.</span></span>
2. <span data-ttu-id="b7223-146">V seznamu hello šablon šířky pásma vyberte šablonu hello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="b7223-146">In hello list of bandwidth templates, select hello template you wish toodelete.</span></span> <span data-ttu-id="b7223-147">Klikněte pravým tlačítkem myši a z hello kontextové nabídky, vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b7223-147">Right-click and from hello context menu, select **Delete**.</span></span>
3. <span data-ttu-id="b7223-148">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b7223-148">When prompted for confirmation, click **OK**.</span></span> <span data-ttu-id="b7223-149">To by měl odstranit hello šablony šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-149">This should delete hello bandwidth template.</span></span> 
4. <span data-ttu-id="b7223-150">Hello seznam šablon šířky pásma aktualizuje tooreflect hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="b7223-150">hello list of bandwidth templates updates tooreflect hello deletion.</span></span>

> [!NOTE]
> <span data-ttu-id="b7223-151">Vaše změny nelze uložit, pokud hello upravit plán se překrývá s existující plán v šabloně hello šířky pásma, který chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="b7223-151">You cannot save your changes if hello edited schedule overlaps with an existing schedule in hello bandwidth template that you are modifying.</span></span>

## <a name="delete-a-bandwidth-template"></a><span data-ttu-id="b7223-152">Odstranit šablonu šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-152">Delete a bandwidth template</span></span>

<span data-ttu-id="b7223-153">Proveďte následující kroky toodelete šablony šířky pásma hello.</span><span class="sxs-lookup"><span data-stu-id="b7223-153">Perform hello following steps toodelete a bandwidth template.</span></span>

#### <a name="toodelete-a-bandwidth-template"></a><span data-ttu-id="b7223-154">toodelete šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-154">toodelete a bandwidth template</span></span>

1. <span data-ttu-id="b7223-155">Přejděte služby StorSimple Manager zařízení tooyour a klikněte na tlačítko **šablony šířky pásma**.</span><span class="sxs-lookup"><span data-stu-id="b7223-155">Go tooyour StorSimple Device Manager service and click **Bandwidth templates**.</span></span>
2. <span data-ttu-id="b7223-156">V seznamu hello šablon šířky pásma vyberte šablonu hello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="b7223-156">In hello list of bandwidth templates, select hello template you wish toodelete.</span></span> <span data-ttu-id="b7223-157">Klikněte pravým tlačítkem myši a z hello kontextové nabídky, vyberte možnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="b7223-157">Right-click and from hello context menu, select Delete.</span></span>
3. <span data-ttu-id="b7223-158">Po zobrazení výzvy k potvrzení, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b7223-158">When prompted for confirmation, click **OK**.</span></span> <span data-ttu-id="b7223-159">To by měl odstranit hello šablony šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-159">This should delete hello bandwidth template.</span></span>
4. <span data-ttu-id="b7223-160">Hello seznam šablon šířky pásma aktualizuje tooreflect hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="b7223-160">hello list of bandwidth templates updates tooreflect hello deletion.</span></span>

<span data-ttu-id="b7223-161">Když je šablona hello používá všechny svazky, nebude možné toodelete ho.</span><span class="sxs-lookup"><span data-stu-id="b7223-161">If hello template is in use by any volume(s), you will not be allowed toodelete it.</span></span> <span data-ttu-id="b7223-162">Zobrazí se chybová zpráva oznamující, že tato šablona hello používá.</span><span class="sxs-lookup"><span data-stu-id="b7223-162">You will see an error message indicating that hello template is in use.</span></span> <span data-ttu-id="b7223-163">Dialogové okno chyby zpráva se zobrazí radí, že má být odebrána všechny šablony toohello odkazy hello.</span><span class="sxs-lookup"><span data-stu-id="b7223-163">An error message dialog box will appear advising you that all hello references toohello template should be removed.</span></span>

<span data-ttu-id="b7223-164">Všechny šablony toohello hello odkazy můžete odstranit pomocí přístupu k hello **kontejnery svazků** stránky a úprava hello kontejnery svazků, které tuto šablonu použít tak, aby použít jinou šablonu nebo použít vlastní nebo neomezená šířky pásma nastavení.</span><span class="sxs-lookup"><span data-stu-id="b7223-164">You can delete all hello references toohello template by accessing hello **Volume Containers** page and modifying hello volume containers that use this template so that they use another template or use a custom or unlimited bandwidth setting.</span></span> <span data-ttu-id="b7223-165">Pokud byly odebrány všechny odkazy hello, můžete odstranit šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="b7223-165">When all hello references have been removed, you can delete hello template.</span></span>

## <a name="use-a-default-bandwidth-template"></a><span data-ttu-id="b7223-166">Použít výchozí šablonu šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-166">Use a default bandwidth template</span></span>

<span data-ttu-id="b7223-167">Výchozí šablony šířky pásma je k dispozici a je používána kontejnery svazků ve výchozím nastavení řízení šířky pásma tooenforce při přístupu k hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="b7223-167">A default bandwidth template is provided and is used by volume containers by default tooenforce bandwidth controls when accessing hello cloud.</span></span> <span data-ttu-id="b7223-168">výchozí šablony Hello slouží také jako připravené odkaz pro uživatele, kteří vytvořit své vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="b7223-168">hello default template also serves as a ready reference for users who create their own templates.</span></span> <span data-ttu-id="b7223-169">Podrobnosti o Hello této výchozí šablony jsou:</span><span class="sxs-lookup"><span data-stu-id="b7223-169">hello details of this default template are:</span></span>

* <span data-ttu-id="b7223-170">**Název** – neomezená noci a víkendů</span><span class="sxs-lookup"><span data-stu-id="b7223-170">**Name** – Unlimited nights and weekends</span></span>
* <span data-ttu-id="b7223-171">**Plán** – jeden plán z tooFriday pondělí, která se vztahuje šířky pásma 1 MB/s mezi 8: 00 a 17: 00 času.</span><span class="sxs-lookup"><span data-stu-id="b7223-171">**Schedule** – A single schedule from Monday tooFriday that applies a bandwidth rate of 1 Mbps between 8 AM and 5 PM device time.</span></span> <span data-ttu-id="b7223-172">tooUnlimited zbývající hello týdnu hello je nastavená šířka pásma Hello.</span><span class="sxs-lookup"><span data-stu-id="b7223-172">hello bandwidth is set tooUnlimited for hello remainder of hello week.</span></span>

<span data-ttu-id="b7223-173">výchozí šablony Hello se dá upravit.</span><span class="sxs-lookup"><span data-stu-id="b7223-173">hello default template can be edited.</span></span> <span data-ttu-id="b7223-174">je sledovat využití Hello této šablony (včetně upravená verze).</span><span class="sxs-lookup"><span data-stu-id="b7223-174">hello usage of this template (including edited versions) is tracked.</span></span>

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a><span data-ttu-id="b7223-175">Vytvořit šablonu celodenní šířky pásma, která se spouští v zadanou dobu</span><span class="sxs-lookup"><span data-stu-id="b7223-175">Create an all-day bandwidth template that starts at a specified time</span></span>

<span data-ttu-id="b7223-176">Použijte tento postup toocreate plánu, který se spustí v zadaný čas a spustí celý den.</span><span class="sxs-lookup"><span data-stu-id="b7223-176">Follow this procedure toocreate a schedule that starts at a specified time and runs all day.</span></span> <span data-ttu-id="b7223-177">V příkladu hello hello plán začíná na 9: 00 v hello ráno a spouští až 9: 00 hello další ráno.</span><span class="sxs-lookup"><span data-stu-id="b7223-177">In hello example, hello schedule starts at 9 AM in hello morning and runs until 9 AM hello next morning.</span></span> <span data-ttu-id="b7223-178">Je důležité toonote, který hello počáteční a koncový čas pro dané plán musí oba být obsaženy na hello stejné 24 hodin naplánovat a nemůžou zahrnovat víc dnů.</span><span class="sxs-lookup"><span data-stu-id="b7223-178">It's important toonote that hello start and end times for a given schedule must both be contained on hello same 24 hour schedule and cannot span multiple days.</span></span> <span data-ttu-id="b7223-179">Pokud potřebujete tooset šablon šířky pásma, které jsou rozmístěny v několika dní, budete potřebovat toouse více plánů (jak je znázorněno v příkladu hello).</span><span class="sxs-lookup"><span data-stu-id="b7223-179">If you need tooset up bandwidth templates that span multiple days, you will need toouse multiple schedules (as shown in hello example).</span></span>

#### <a name="toocreate-an-all-day-bandwidth-template"></a><span data-ttu-id="b7223-180">toocreate šablonu celodenní šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-180">toocreate an all-day bandwidth template</span></span>

1. <span data-ttu-id="b7223-181">Vytvořte plán, který začíná v 9: 00 v hello ráno a spouští dokud půlnoci.</span><span class="sxs-lookup"><span data-stu-id="b7223-181">Create a schedule that starts at 9 AM in hello morning and runs until midnight.</span></span>
2. <span data-ttu-id="b7223-182">Přidejte jiný plán.</span><span class="sxs-lookup"><span data-stu-id="b7223-182">Add another schedule.</span></span> <span data-ttu-id="b7223-183">Nakonfigurujte druhý toorun plán hello od půlnoci až 9: 00 v hello ráno.</span><span class="sxs-lookup"><span data-stu-id="b7223-183">Configure hello second schedule toorun from midnight until 9 AM in hello morning.</span></span>
3. <span data-ttu-id="b7223-184">Uložte šablonu šířky pásma hello.</span><span class="sxs-lookup"><span data-stu-id="b7223-184">Save hello bandwidth template.</span></span>

<span data-ttu-id="b7223-185">složený plán Hello bude poté otevřete současně dle vašeho výběru a spusťte celodenní.</span><span class="sxs-lookup"><span data-stu-id="b7223-185">hello composite schedule will then start at a time of your choosing and run all-day.</span></span>

## <a name="questions-and-answers-about-bandwidth-templates"></a><span data-ttu-id="b7223-186">Otázky a odpovědi týkající se šířky pásma šablony</span><span class="sxs-lookup"><span data-stu-id="b7223-186">Questions and answers about bandwidth templates</span></span>

<span data-ttu-id="b7223-187">**Q**.</span><span class="sxs-lookup"><span data-stu-id="b7223-187">**Q**.</span></span> <span data-ttu-id="b7223-188">Ovládací prvky toobandwidth následovat, když jsou mezi hello plány?</span><span class="sxs-lookup"><span data-stu-id="b7223-188">What happens toobandwidth controls when you are in between hello schedules?</span></span> <span data-ttu-id="b7223-189">(Skončila plánu a jiný se ještě nespustilo.)</span><span class="sxs-lookup"><span data-stu-id="b7223-189">(A schedule has ended and another one has not started yet.)</span></span>

<span data-ttu-id="b7223-190">**A**.</span><span class="sxs-lookup"><span data-stu-id="b7223-190">**A**.</span></span> <span data-ttu-id="b7223-191">V takových případech bude těmto nekompatibilitám bez řízení šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-191">In such cases, no bandwidth controls will be employed.</span></span> <span data-ttu-id="b7223-192">To znamená, že hello zařízení můžete použít neomezenou šířku pásma při vrstvení toohello dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="b7223-192">This means that hello device can use unlimited bandwidth when tiering data toohello cloud.</span></span>

<span data-ttu-id="b7223-193">**Q**.</span><span class="sxs-lookup"><span data-stu-id="b7223-193">**Q**.</span></span> <span data-ttu-id="b7223-194">Můžete upravit šablony šířky pásma na zařízení s offline?</span><span class="sxs-lookup"><span data-stu-id="b7223-194">Can you modify bandwidth templates on an offline device?</span></span>

<span data-ttu-id="b7223-195">**A**.</span><span class="sxs-lookup"><span data-stu-id="b7223-195">**A**.</span></span> <span data-ttu-id="b7223-196">Nebudete moct toomodify šablony šířky pásma na kontejnery svazků hello odpovídající zařízení je offline.</span><span class="sxs-lookup"><span data-stu-id="b7223-196">You will not be able toomodify bandwidth templates on volumes containers if hello corresponding device is offline.</span></span>

<span data-ttu-id="b7223-197">**Q**.</span><span class="sxs-lookup"><span data-stu-id="b7223-197">**Q**.</span></span> <span data-ttu-id="b7223-198">Můžete upravit šablonu šířky pásma přidružené kontejner svazků, když jsou offline hello přidružené svazky?</span><span class="sxs-lookup"><span data-stu-id="b7223-198">Can you edit a bandwidth template associated with a volume container when hello associated volumes are offline?</span></span>

<span data-ttu-id="b7223-199">**A**.</span><span class="sxs-lookup"><span data-stu-id="b7223-199">**A**.</span></span> <span data-ttu-id="b7223-200">Můžete upravit šablonu šířky pásma přidružené kontejner svazků, jejichž svazky jsou offline.</span><span class="sxs-lookup"><span data-stu-id="b7223-200">You can modify a bandwidth template associated with a volume container whose volumes are offline.</span></span> <span data-ttu-id="b7223-201">Všimněte si, že po offline svazky se žádná data vrstvené z cloudu toohello hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="b7223-201">Note that when volumes are offline, no data will be tiered from hello device toohello cloud.</span></span>

<span data-ttu-id="b7223-202">**Q**.</span><span class="sxs-lookup"><span data-stu-id="b7223-202">**Q**.</span></span> <span data-ttu-id="b7223-203">Můžete odstranit výchozí šablonu?</span><span class="sxs-lookup"><span data-stu-id="b7223-203">Can you delete a default template?</span></span>

<span data-ttu-id="b7223-204">**A**.</span><span class="sxs-lookup"><span data-stu-id="b7223-204">**A**.</span></span> <span data-ttu-id="b7223-205">I když můžete odstranit výchozí šablonu, není vhodné toodo tak.</span><span class="sxs-lookup"><span data-stu-id="b7223-205">Although you can delete a default template, it is not a good idea toodo so.</span></span> <span data-ttu-id="b7223-206">Hello použití výchozí šablony, včetně upravená verze sledován.</span><span class="sxs-lookup"><span data-stu-id="b7223-206">hello usage of a default template, including edited versions, is tracked.</span></span> <span data-ttu-id="b7223-207">Hello sledování dat se analyzují a průběhu hello času je použité tooimprove hello výchozí šablony.</span><span class="sxs-lookup"><span data-stu-id="b7223-207">hello tracking data is analyzed and over hello course of time, is used tooimprove hello default template.</span></span>

<span data-ttu-id="b7223-208">**Q**.</span><span class="sxs-lookup"><span data-stu-id="b7223-208">**Q**.</span></span> <span data-ttu-id="b7223-209">Jak byste zjistit, jestli vaše šablony šířky pásma musí být toobe změnit?</span><span class="sxs-lookup"><span data-stu-id="b7223-209">How do you determine that your bandwidth templates need toobe modified?</span></span>

<span data-ttu-id="b7223-210">**A**.</span><span class="sxs-lookup"><span data-stu-id="b7223-210">**A**.</span></span> <span data-ttu-id="b7223-211">Jeden z hello přihlásí, je nutné, aby toomodify hello šířky pásma šablony je při spuštění zobrazuje hello sítě zpomalit nebo vyseknutí několikrát za den.</span><span class="sxs-lookup"><span data-stu-id="b7223-211">One of hello signs that you need toomodify hello bandwidth templates is when you start seeing hello network slow down or choke multiple times in a day.</span></span> <span data-ttu-id="b7223-212">V takovém případě sledujte prohlížením hello vstupně-výstupní výkon a propustnost sítě grafy hello úložiště a využití sítě.</span><span class="sxs-lookup"><span data-stu-id="b7223-212">If this happens, monitor hello storage and usage network by looking at hello I/O Performance and Network Throughput charts.</span></span>

<span data-ttu-id="b7223-213">Z dat propustnosti sítě hello Identifikujte denní dobu hello a hello kontejnery svazků, ve které hello dojde k přetížení sítě.</span><span class="sxs-lookup"><span data-stu-id="b7223-213">From hello network throughput data, identify hello time of day and hello volume containers in which hello network bottleneck occurs.</span></span> <span data-ttu-id="b7223-214">Pokud to se stane, když dat se vrstvené toohello cloudu (získat tyto informace z vstupně-výstupní výkon pro všechny kontejnery svazků pro toocloud zařízení), bude nutné toomodify hello šířky pásma šablon přidružených k vaší kontejnery svazků.</span><span class="sxs-lookup"><span data-stu-id="b7223-214">If this happens when data is being tiered toohello cloud (get this information from I/O performance for all volume containers for device toocloud), then you will need toomodify hello bandwidth templates associated with your volume containers.</span></span>

<span data-ttu-id="b7223-215">Po úpravě hello šablony jsou používány, budete potřebovat síť hello toomonitor znovu pro významné latenci.</span><span class="sxs-lookup"><span data-stu-id="b7223-215">After hello modified templates are in use, you will need toomonitor hello network again for significant latencies.</span></span> <span data-ttu-id="b7223-216">Pokud tyto stále existují, budete potřebovat toorevisit šablony šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="b7223-216">If these still exist, then you will need toorevisit your bandwidth templates.</span></span>

<span data-ttu-id="b7223-217">**Q**.</span><span class="sxs-lookup"><span data-stu-id="b7223-217">**Q**.</span></span> <span data-ttu-id="b7223-218">Co se stane, pokud máte více kontejnery svazků na zařízení plány této překrývají, ale jiné limity platí tooeach?</span><span class="sxs-lookup"><span data-stu-id="b7223-218">What happens if multiple volume containers on my device have schedules that overlap but different limits apply tooeach?</span></span>

<span data-ttu-id="b7223-219">**A**.</span><span class="sxs-lookup"><span data-stu-id="b7223-219">**A**.</span></span> <span data-ttu-id="b7223-220">Předpokládejme, že máte zařízení s 3 kontejnery svazků.</span><span class="sxs-lookup"><span data-stu-id="b7223-220">Let's assume that you have a device with 3 volume containers.</span></span> <span data-ttu-id="b7223-221">Hello plány, které jsou přidružené k těmto kontejnerům úplně překrývat.</span><span class="sxs-lookup"><span data-stu-id="b7223-221">hello schedules associated with these containers completely overlap.</span></span> <span data-ttu-id="b7223-222">Pro každou z těchto kontejnerů hello šířky pásma použít je mez 5, 10 až 15 MB/s.</span><span class="sxs-lookup"><span data-stu-id="b7223-222">For each of these containers, hello bandwidth limits used are 5, 10, and 15 Mbps respectively.</span></span> <span data-ttu-id="b7223-223">Při vstupně-výstupních operací dochází na všech těchto kontejnerů v hello mohou být použity současně minimální hello limitů šířky pásma hello 3: v tomto případě 5 MB/s jako tyto odchozí sdílení vstupně-výstupní požadavky hello stejné fronty.</span><span class="sxs-lookup"><span data-stu-id="b7223-223">When I/O are occurring on all of these containers at hello same time, hello minimum of hello 3 bandwidth limits may be applied: in this case, 5 Mbps as these outgoing I/O requests share hello same queue.</span></span>

## <a name="best-practices-for-bandwidth-templates"></a><span data-ttu-id="b7223-224">Doporučené postupy pro šablony šířky pásma</span><span class="sxs-lookup"><span data-stu-id="b7223-224">Best practices for bandwidth templates</span></span>

<span data-ttu-id="b7223-225">Použijte tyto osvědčené postupy pro zařízení StorSimple:</span><span class="sxs-lookup"><span data-stu-id="b7223-225">Follow these best practices for your StorSimple device:</span></span>

* <span data-ttu-id="b7223-226">Konfigurace šablon šířky pásma na vaše zařízení tooenable proměnné omezení propustnost sítě hello hello zařízení v různých časech hello den.</span><span class="sxs-lookup"><span data-stu-id="b7223-226">Configure bandwidth templates on your device tooenable variable throttling of hello network throughput by hello device at different times of hello day.</span></span> <span data-ttu-id="b7223-227">Tyto šablony šířky pásma při použití s plánů zálohování můžete efektivně využívat další šířku pásma sítě pro operace cloudu mimo špičku.</span><span class="sxs-lookup"><span data-stu-id="b7223-227">These bandwidth templates when used with backup schedules can effectively leverage additional network bandwidth for cloud operations during off-peak hours.</span></span>
* <span data-ttu-id="b7223-228">Vypočítejte hello skutečné pásma potřebná pro konkrétní nasazení na základě velikosti hello hello nasazení a plánovanou dobu hello požadované obnovení (RTO).</span><span class="sxs-lookup"><span data-stu-id="b7223-228">Calculate hello actual bandwidth required for a particular deployment based on hello size of hello deployment and hello required recovery time objective (RTO).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7223-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b7223-229">Next steps</span></span>

<span data-ttu-id="b7223-230">Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b7223-230">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

