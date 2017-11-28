---
title: "aaaCapture data ze služby Event Hubs do Azure Data Lake Store | Microsoft Docs"
description: "Použití Azure Data Lake Store toocapture data ze služby Event Hubs"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 09b17bd0b47043bd2c83dba72c01a8064f206a0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-store-toocapture-data-from-event-hubs"></a><span data-ttu-id="6c68c-103">Použití Azure Data Lake Store toocapture data ze služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="6c68c-103">Use Azure Data Lake Store toocapture data from Event Hubs</span></span>

<span data-ttu-id="6c68c-104">Zjistěte, jak toouse Azure Data Lake Store toocapture dat přijatých Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="6c68c-104">Learn how toouse Azure Data Lake Store toocapture data received by Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c68c-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6c68c-105">Prerequisites</span></span>

* <span data-ttu-id="6c68c-106">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-106">**An Azure subscription**.</span></span> <span data-ttu-id="6c68c-107">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6c68c-107">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="6c68c-108">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-108">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="6c68c-109">Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6c68c-109">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md).</span></span>

*  <span data-ttu-id="6c68c-110">**Obor názvů Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-110">**An Event Hubs namespace**.</span></span> <span data-ttu-id="6c68c-111">Pokyny najdete v tématu [vytvoření oboru názvů Event Hubs](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace).</span><span class="sxs-lookup"><span data-stu-id="6c68c-111">For instructions, see [Create an Event Hubs namespace](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace).</span></span> <span data-ttu-id="6c68c-112">Zkontrolujte, zda jsou v hello hello účtu Data Lake Store a obor názvů služby Event Hubs hello stejného předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="6c68c-112">Make sure hello Data Lake Store account and hello Event Hubs namespace are in hello same Azure subscription.</span></span>


## <a name="assign-permissions-tooevent-hubs"></a><span data-ttu-id="6c68c-113">Přiřazení oprávnění tooEvent rozbočovače</span><span class="sxs-lookup"><span data-stu-id="6c68c-113">Assign permissions tooEvent Hubs</span></span>

<span data-ttu-id="6c68c-114">V této části vytvoříte v rámci účtu hello do složky, kam chcete toocapture hello data ze služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="6c68c-114">In this section, you create a folder within hello account where you want toocapture hello data from Event Hubs.</span></span> <span data-ttu-id="6c68c-115">Můžete také přiřadit oprávnění tooEvent centra tak, aby ho můžete zapsat data do účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6c68c-115">You also assign permissions tooEvent Hubs so that it can write data into a Data Lake Store account.</span></span> 

1. <span data-ttu-id="6c68c-116">Otevřete účet Data Lake Store hello, kam chcete toocapture data ze služby Event Hubs a potom klikněte na **Průzkumníku dat**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-116">Open hello Data Lake Store account where you want toocapture data from Event Hubs and then click on **Data Explorer**.</span></span>

    <span data-ttu-id="6c68c-117">![Data Lake Store Průzkumníku dat](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Průzkumníku dat v Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-117">![Data Lake Store data explorer](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Store data explorer")</span></span>

2.  <span data-ttu-id="6c68c-118">Klikněte na tlačítko **novou složku** a pak zadejte název složky, kam chcete toocapture hello data.</span><span class="sxs-lookup"><span data-stu-id="6c68c-118">Click **New Folder** and then enter a name for folder where you want toocapture hello data.</span></span>

    <span data-ttu-id="6c68c-119">![Vytvořte novou složku v Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "vytvořte novou složku v Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-119">![Create a new folder in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Create a new folder in Data Lake Store")</span></span>

3. <span data-ttu-id="6c68c-120">Přiřadíte oprávnění v kořenové hello hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6c68c-120">Assign permissions at hello root of hello Data Lake Store.</span></span> 

    <span data-ttu-id="6c68c-121">a.</span><span class="sxs-lookup"><span data-stu-id="6c68c-121">a.</span></span> <span data-ttu-id="6c68c-122">Klikněte na tlačítko **Průzkumníku dat**, vyberte hello kořenovém hello účtu Data Lake Store a pak klikněte na tlačítko **přístup**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-122">Click **Data Explorer**, select hello root of hello Data Lake Store account, and then click **Access**.</span></span>

    <span data-ttu-id="6c68c-123">![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-123">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Assign permissions for Data Lake Store root")</span></span>

    <span data-ttu-id="6c68c-124">b.</span><span class="sxs-lookup"><span data-stu-id="6c68c-124">b.</span></span> <span data-ttu-id="6c68c-125">V části **přístup**, klikněte na tlačítko **přidat**, klikněte na tlačítko **vybrat uživatele nebo skupinu**a poté vyhledejte `Microsoft.EventHubs`.</span><span class="sxs-lookup"><span data-stu-id="6c68c-125">Under **Access**, click **Add**, click **Select User or Group**, and then search for `Microsoft.EventHubs`.</span></span> 

    <span data-ttu-id="6c68c-126">![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-126">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Assign permissions for Data Lake Store root")</span></span>
    
    <span data-ttu-id="6c68c-127">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-127">Click **Select**.</span></span>

    <span data-ttu-id="6c68c-128">c.</span><span class="sxs-lookup"><span data-stu-id="6c68c-128">c.</span></span> <span data-ttu-id="6c68c-129">V části **přiřadit oprávnění**, klikněte na tlačítko **vyberte oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-129">Under **Assign Permissions**, click **Select Permissions**.</span></span> <span data-ttu-id="6c68c-130">Nastavit **oprávnění** příliš**Execute**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-130">Set **Permissions** too**Execute**.</span></span> <span data-ttu-id="6c68c-131">Nastavit **přidat do** příliš**tuto složku a všechny podřízené objekty**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-131">Set **Add to** too**This folder and all children**.</span></span> <span data-ttu-id="6c68c-132">Nastavit **přidat jako** příliš**položka oprávnění k přístupu a výchozí položku oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-132">Set **Add as** too**An access permission entry and a default permission entry**.</span></span>

    <span data-ttu-id="6c68c-133">![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-133">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Assign permissions for Data Lake Store root")</span></span>

    <span data-ttu-id="6c68c-134">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-134">Click **OK**.</span></span>

4. <span data-ttu-id="6c68c-135">Přiřazení oprávnění pro složku hello v rámci účtu Data Lake Store, které chcete toocapture data.</span><span class="sxs-lookup"><span data-stu-id="6c68c-135">Assign permissions for hello folder under Data Lake Store account where you want toocapture data.</span></span>

    <span data-ttu-id="6c68c-136">a.</span><span class="sxs-lookup"><span data-stu-id="6c68c-136">a.</span></span> <span data-ttu-id="6c68c-137">Klikněte na tlačítko **Průzkumníku dat**, vyberte složku hello v hello účtu Data Lake Store a pak klikněte na tlačítko **přístup**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-137">Click **Data Explorer**, select hello folder in hello Data Lake Store account, and then click **Access**.</span></span>

    <span data-ttu-id="6c68c-138">![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "přiřadit oprávnění pro složku Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-138">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Assign permissions for Data Lake Store folder")</span></span>

    <span data-ttu-id="6c68c-139">b.</span><span class="sxs-lookup"><span data-stu-id="6c68c-139">b.</span></span> <span data-ttu-id="6c68c-140">V části **přístup**, klikněte na tlačítko **přidat**, klikněte na tlačítko **vybrat uživatele nebo skupinu**a poté vyhledejte `Microsoft.EventHubs`.</span><span class="sxs-lookup"><span data-stu-id="6c68c-140">Under **Access**, click **Add**, click **Select User or Group**, and then search for `Microsoft.EventHubs`.</span></span> 

    <span data-ttu-id="6c68c-141">![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "přiřadit oprávnění pro složku Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-141">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Assign permissions for Data Lake Store folder")</span></span>
    
    <span data-ttu-id="6c68c-142">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-142">Click **Select**.</span></span>

    <span data-ttu-id="6c68c-143">c.</span><span class="sxs-lookup"><span data-stu-id="6c68c-143">c.</span></span> <span data-ttu-id="6c68c-144">V části **přiřadit oprávnění**, klikněte na tlačítko **vyberte oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-144">Under **Assign Permissions**, click **Select Permissions**.</span></span> <span data-ttu-id="6c68c-145">Nastavit **oprávnění** příliš**číst, zapisovat,** a **Execute**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-145">Set **Permissions** too**Read, Write,** and **Execute**.</span></span> <span data-ttu-id="6c68c-146">Nastavit **přidat do** příliš**tuto složku a všechny podřízené objekty**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-146">Set **Add to** too**This folder and all children**.</span></span> <span data-ttu-id="6c68c-147">Nakonec nastavte **přidat jako** příliš**položka oprávnění k přístupu a výchozí položku oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-147">Finally, set **Add as** too**An access permission entry and a default permission entry**.</span></span>

    <span data-ttu-id="6c68c-148">![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "přiřadit oprávnění pro složku Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-148">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Assign permissions for Data Lake Store folder")</span></span>
    
    <span data-ttu-id="6c68c-149">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-149">Click **OK**.</span></span> 

## <a name="configure-event-hubs-toocapture-data-toodata-lake-store"></a><span data-ttu-id="6c68c-150">Konfigurace služby Event Hubs toocapture data tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="6c68c-150">Configure Event Hubs toocapture data tooData Lake Store</span></span>

<span data-ttu-id="6c68c-151">V této části vytvoříte Centrum událostí v rámci oboru názvů Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="6c68c-151">In this section, you create an Event Hub within an Event Hubs namespace.</span></span> <span data-ttu-id="6c68c-152">Můžete také nakonfigurovat hello centra událostí toocapture data tooan účtu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6c68c-152">You also configure hello Event Hub toocapture data tooan Azure Data Lake Store account.</span></span> <span data-ttu-id="6c68c-153">V této části se předpokládá, že jste již vytvořili na obor názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="6c68c-153">This section assumes that you have already created an Event Hubs namespace.</span></span>

2. <span data-ttu-id="6c68c-154">Z hello **přehled** podokně hello Event Hubs oboru názvů, klikněte na tlačítko **+ centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-154">From hello **Overview** pane of hello Event Hubs namespace, click **+ Event Hub**.</span></span>

    <span data-ttu-id="6c68c-155">![Vytvoření centra událostí](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "vytvoření centra událostí")</span><span class="sxs-lookup"><span data-stu-id="6c68c-155">![Create Event Hub](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Create Event Hub")</span></span>

3. <span data-ttu-id="6c68c-156">Poskytují následující hello hodnoty tooconfigure Event Hubs toocapture data tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6c68c-156">Provide hello following values tooconfigure Event Hubs toocapture data tooData Lake Store.</span></span>

    <span data-ttu-id="6c68c-157">![Vytvoření centra událostí](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "vytvoření centra událostí")</span><span class="sxs-lookup"><span data-stu-id="6c68c-157">![Create Event Hub](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Create Event Hub")</span></span>

    <span data-ttu-id="6c68c-158">a.</span><span class="sxs-lookup"><span data-stu-id="6c68c-158">a.</span></span> <span data-ttu-id="6c68c-159">Zadejte název pro hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="6c68c-159">Provide a name for hello Event Hub.</span></span>
    
    <span data-ttu-id="6c68c-160">b.</span><span class="sxs-lookup"><span data-stu-id="6c68c-160">b.</span></span> <span data-ttu-id="6c68c-161">V tomto kurzu nastavit **počet oddílů** a **uchování zpráv** toohello výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6c68c-161">For this tutorial, set **Partition Count** and **Message Retention** toohello default values.</span></span>
    
    <span data-ttu-id="6c68c-162">c.</span><span class="sxs-lookup"><span data-stu-id="6c68c-162">c.</span></span> <span data-ttu-id="6c68c-163">Nastavit **zaznamenat** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-163">Set **Capture** too**On**.</span></span> <span data-ttu-id="6c68c-164">Sada hello **časové okno** (jak často toocapture) a **velikost okna** (toocapture velikost dat).</span><span class="sxs-lookup"><span data-stu-id="6c68c-164">Set hello **Time Window** (how frequently toocapture) and **Size Window** (data size toocapture).</span></span> 
    
    <span data-ttu-id="6c68c-165">d.</span><span class="sxs-lookup"><span data-stu-id="6c68c-165">d.</span></span> <span data-ttu-id="6c68c-166">Pro **zaznamenání zprostředkovatele**, vyberte **Azure Data Lake Store** a hello vyberte hello Data Lake Store jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="6c68c-166">For **Capture Provider**, select **Azure Data Lake Store** and hello select hello Data Lake Store you created earlier.</span></span> <span data-ttu-id="6c68c-167">Pro **Data Lake cesta**, zadejte název hello hello složky, které jste vytvořili v hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6c68c-167">For **Data Lake Path**, enter hello name of hello folder you created in hello Data Lake Store account.</span></span> <span data-ttu-id="6c68c-168">Potřebujete jenom tooprovide hello relativní cesta toohello složky.</span><span class="sxs-lookup"><span data-stu-id="6c68c-168">You only need tooprovide hello relative path toohello folder.</span></span>

    <span data-ttu-id="6c68c-169">e.</span><span class="sxs-lookup"><span data-stu-id="6c68c-169">e.</span></span> <span data-ttu-id="6c68c-170">Nechte hello **ukázka zachycení formátů název** toohello výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6c68c-170">Leave hello **Sample capture file name formats** toohello default value.</span></span> <span data-ttu-id="6c68c-171">Tato možnost řídí hello struktura složek, který se vytvoří ve složce zachycení hello.</span><span class="sxs-lookup"><span data-stu-id="6c68c-171">This option governs hello folder structure that is created under hello capture folder.</span></span>

    <span data-ttu-id="6c68c-172">f.</span><span class="sxs-lookup"><span data-stu-id="6c68c-172">f.</span></span> <span data-ttu-id="6c68c-173">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6c68c-173">Click **Create**.</span></span>

## <a name="test-hello-setup"></a><span data-ttu-id="6c68c-174">Nastavení testu hello</span><span class="sxs-lookup"><span data-stu-id="6c68c-174">Test hello setup</span></span>

<span data-ttu-id="6c68c-175">Nyní můžete otestovat hello řešení odesláním toohello datového centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="6c68c-175">You can now test hello solution by sending data toohello Azure Event Hub.</span></span> <span data-ttu-id="6c68c-176">Postupujte podle pokynů hello [odesílat události tooAzure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).</span><span class="sxs-lookup"><span data-stu-id="6c68c-176">Follow hello instructions at [Send events tooAzure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).</span></span> <span data-ttu-id="6c68c-177">Po spuštění odeslání dat hello zobrazí hello dat v Data Lake Store pomocí hello struktura složek, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="6c68c-177">Once you start sending hello data, you see hello data reflected in Data Lake Store using hello folder structure you specified.</span></span> <span data-ttu-id="6c68c-178">Například struktura složek, uvidíte, jak je znázorněno v následujícím snímku obrazovky v Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="6c68c-178">For example, you see a folder structure, as shown in hello following screenshot, in your Data Lake Store.</span></span>

<span data-ttu-id="6c68c-179">![Ukázka dat pro EventHub v Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "EventHub ukázkových dat v Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="6c68c-179">![Sample EventHub data in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Sample EventHub data in Data Lake Store")</span></span>

> [!NOTE]
> <span data-ttu-id="6c68c-180">I když nemáte zprávy přicházející do centra událostí, Event Hubs prázdné soubory s právě hello hlavičky zapisuje do hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6c68c-180">Even if you do not have messages coming into Event Hubs, Event Hubs writes empty files with just hello headers into hello Data Lake Store account.</span></span> <span data-ttu-id="6c68c-181">Hello soubory jsou zapsány v hello stejné časový interval, který jste zadali při vytváření centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="6c68c-181">hello files are written at hello same time interval that you provided while creating hello Event Hubs.</span></span>
> 
>

## <a name="analyze-data-in-data-lake-store"></a><span data-ttu-id="6c68c-182">Analýza dat v Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6c68c-182">Analyze data in Data Lake Store</span></span>

<span data-ttu-id="6c68c-183">Jakmile hello dat v Data Lake Store, můžete spustit analytické úlohy tooprocess a zpracujte data hello.</span><span class="sxs-lookup"><span data-stu-id="6c68c-183">Once hello data is in Data Lake Store, you can run analytical jobs tooprocess and crunch hello data.</span></span> <span data-ttu-id="6c68c-184">V tématu [USQL Avro příklad](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) o toodo tento pomocí Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="6c68c-184">See [USQL Avro Example](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) on how toodo this using Azure Data Lake Analytics.</span></span>
  

## <a name="see-also"></a><span data-ttu-id="6c68c-185">Viz také</span><span class="sxs-lookup"><span data-stu-id="6c68c-185">See also</span></span>
* [<span data-ttu-id="6c68c-186">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6c68c-186">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="6c68c-187">Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="6c68c-187">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
