---
title: "Zachycení dat ze služby Event Hubs do Azure Data Lake Store | Microsoft Docs"
description: "Použití Azure Data Lake Store k zaznamenání dat ze služby Event Hubs"
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
ms.openlocfilehash: a9e69576958ae96d22a4eb03d0df429f0b307298
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-data-lake-store-to-capture-data-from-event-hubs"></a><span data-ttu-id="a04aa-103">Použití Azure Data Lake Store k zaznamenání dat ze služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a04aa-103">Use Azure Data Lake Store to capture data from Event Hubs</span></span>

<span data-ttu-id="a04aa-104">Naučte se používat Azure Data Lake Store k zaznamenání dat přijatých Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="a04aa-104">Learn how to use Azure Data Lake Store to capture data received by Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a04aa-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a04aa-105">Prerequisites</span></span>

* <span data-ttu-id="a04aa-106">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-106">**An Azure subscription**.</span></span> <span data-ttu-id="a04aa-107">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a04aa-107">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="a04aa-108">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-108">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="a04aa-109">Pokyny o tom, jak vytvořit najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a04aa-109">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md).</span></span>

*  <span data-ttu-id="a04aa-110">**Obor názvů Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-110">**An Event Hubs namespace**.</span></span> <span data-ttu-id="a04aa-111">Pokyny najdete v tématu [vytvoření oboru názvů Event Hubs](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace).</span><span class="sxs-lookup"><span data-stu-id="a04aa-111">For instructions, see [Create an Event Hubs namespace](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace).</span></span> <span data-ttu-id="a04aa-112">Ujistěte se, že účet Data Lake Store a obor názvů služby Event Hubs jsou ve stejném předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="a04aa-112">Make sure the Data Lake Store account and the Event Hubs namespace are in the same Azure subscription.</span></span>


## <a name="assign-permissions-to-event-hubs"></a><span data-ttu-id="a04aa-113">Přiřazení oprávnění do centra událostí</span><span class="sxs-lookup"><span data-stu-id="a04aa-113">Assign permissions to Event Hubs</span></span>

<span data-ttu-id="a04aa-114">V této části vytvoříte složku v rámci účtu, ve které chcete zaznamenat data ze služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="a04aa-114">In this section, you create a folder within the account where you want to capture the data from Event Hubs.</span></span> <span data-ttu-id="a04aa-115">Můžete také přiřadit oprávnění do centra událostí tak, aby ho můžete zapsat data do účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a04aa-115">You also assign permissions to Event Hubs so that it can write data into a Data Lake Store account.</span></span> 

1. <span data-ttu-id="a04aa-116">Otevřete účet Data Lake Store, místo zachycení dat ze služby Event Hubs a potom klikněte na **Průzkumníku dat**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-116">Open the Data Lake Store account where you want to capture data from Event Hubs and then click on **Data Explorer**.</span></span>

    <span data-ttu-id="a04aa-117">![Data Lake Store Průzkumníku dat](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Průzkumníku dat v Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-117">![Data Lake Store data explorer](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Store data explorer")</span></span>

2.  <span data-ttu-id="a04aa-118">Klikněte na tlačítko **novou složku** a pak zadejte název složky, kam chcete zaznamenat data.</span><span class="sxs-lookup"><span data-stu-id="a04aa-118">Click **New Folder** and then enter a name for folder where you want to capture the data.</span></span>

    <span data-ttu-id="a04aa-119">![Vytvořte novou složku v Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "vytvořte novou složku v Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-119">![Create a new folder in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Create a new folder in Data Lake Store")</span></span>

3. <span data-ttu-id="a04aa-120">Přiřazení oprávnění v kořenovém adresáři Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a04aa-120">Assign permissions at the root of the Data Lake Store.</span></span> 

    <span data-ttu-id="a04aa-121">a.</span><span class="sxs-lookup"><span data-stu-id="a04aa-121">a.</span></span> <span data-ttu-id="a04aa-122">Klikněte na tlačítko **Průzkumníku dat**, vyberte kořenového účtu Data Lake Store a pak klikněte na tlačítko **přístup**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-122">Click **Data Explorer**, select the root of the Data Lake Store account, and then click **Access**.</span></span>

    <span data-ttu-id="a04aa-123">![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-123">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Assign permissions for Data Lake Store root")</span></span>

    <span data-ttu-id="a04aa-124">b.</span><span class="sxs-lookup"><span data-stu-id="a04aa-124">b.</span></span> <span data-ttu-id="a04aa-125">V části **přístup**, klikněte na tlačítko **přidat**, klikněte na tlačítko **vybrat uživatele nebo skupinu**a poté vyhledejte `Microsoft.EventHubs`.</span><span class="sxs-lookup"><span data-stu-id="a04aa-125">Under **Access**, click **Add**, click **Select User or Group**, and then search for `Microsoft.EventHubs`.</span></span> 

    <span data-ttu-id="a04aa-126">![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-126">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Assign permissions for Data Lake Store root")</span></span>
    
    <span data-ttu-id="a04aa-127">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-127">Click **Select**.</span></span>

    <span data-ttu-id="a04aa-128">c.</span><span class="sxs-lookup"><span data-stu-id="a04aa-128">c.</span></span> <span data-ttu-id="a04aa-129">V části **přiřadit oprávnění**, klikněte na tlačítko **vyberte oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-129">Under **Assign Permissions**, click **Select Permissions**.</span></span> <span data-ttu-id="a04aa-130">Nastavit **oprávnění** k **provést**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-130">Set **Permissions** to **Execute**.</span></span> <span data-ttu-id="a04aa-131">Nastavit **přidat do** k **tuto složku a všechny podřízené objekty**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-131">Set **Add to** to **This folder and all children**.</span></span> <span data-ttu-id="a04aa-132">Nastavit **přidat jako** k **položka oprávnění k přístupu a výchozí položku oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-132">Set **Add as** to **An access permission entry and a default permission entry**.</span></span>

    <span data-ttu-id="a04aa-133">![Přiřazení oprávnění pro Data Lake Store kořenové](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "přiřadit oprávnění pro kořenový adresář Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-133">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Assign permissions for Data Lake Store root")</span></span>

    <span data-ttu-id="a04aa-134">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-134">Click **OK**.</span></span>

4. <span data-ttu-id="a04aa-135">Přiřazení oprávnění pro dílčí složku složky účtu Data Lake Store, ve které chcete zaznamenat data.</span><span class="sxs-lookup"><span data-stu-id="a04aa-135">Assign permissions for the folder under Data Lake Store account where you want to capture data.</span></span>

    <span data-ttu-id="a04aa-136">a.</span><span class="sxs-lookup"><span data-stu-id="a04aa-136">a.</span></span> <span data-ttu-id="a04aa-137">Klikněte na tlačítko **Průzkumníku dat**, vyberte složku, v účtu Data Lake Store a pak klikněte na tlačítko **přístup**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-137">Click **Data Explorer**, select the folder in the Data Lake Store account, and then click **Access**.</span></span>

    <span data-ttu-id="a04aa-138">![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "přiřadit oprávnění pro složku Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-138">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Assign permissions for Data Lake Store folder")</span></span>

    <span data-ttu-id="a04aa-139">b.</span><span class="sxs-lookup"><span data-stu-id="a04aa-139">b.</span></span> <span data-ttu-id="a04aa-140">V části **přístup**, klikněte na tlačítko **přidat**, klikněte na tlačítko **vybrat uživatele nebo skupinu**a poté vyhledejte `Microsoft.EventHubs`.</span><span class="sxs-lookup"><span data-stu-id="a04aa-140">Under **Access**, click **Add**, click **Select User or Group**, and then search for `Microsoft.EventHubs`.</span></span> 

    <span data-ttu-id="a04aa-141">![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "přiřadit oprávnění pro složku Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-141">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Assign permissions for Data Lake Store folder")</span></span>
    
    <span data-ttu-id="a04aa-142">Klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-142">Click **Select**.</span></span>

    <span data-ttu-id="a04aa-143">c.</span><span class="sxs-lookup"><span data-stu-id="a04aa-143">c.</span></span> <span data-ttu-id="a04aa-144">V části **přiřadit oprávnění**, klikněte na tlačítko **vyberte oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-144">Under **Assign Permissions**, click **Select Permissions**.</span></span> <span data-ttu-id="a04aa-145">Nastavit **oprávnění** k **číst, zapisovat,** a **provést**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-145">Set **Permissions** to **Read, Write,** and **Execute**.</span></span> <span data-ttu-id="a04aa-146">Nastavit **přidat do** k **tuto složku a všechny podřízené objekty**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-146">Set **Add to** to **This folder and all children**.</span></span> <span data-ttu-id="a04aa-147">Nakonec nastavte **přidat jako** k **položka oprávnění k přístupu a výchozí položku oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-147">Finally, set **Add as** to **An access permission entry and a default permission entry**.</span></span>

    <span data-ttu-id="a04aa-148">![Přiřazení oprávnění pro Data Lake Store složku](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "přiřadit oprávnění pro složku Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-148">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Assign permissions for Data Lake Store folder")</span></span>
    
    <span data-ttu-id="a04aa-149">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-149">Click **OK**.</span></span> 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-store"></a><span data-ttu-id="a04aa-150">Konfigurace služby Event Hubs k zaznamenání dat do Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a04aa-150">Configure Event Hubs to capture data to Data Lake Store</span></span>

<span data-ttu-id="a04aa-151">V této části vytvoříte Centrum událostí v rámci oboru názvů Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="a04aa-151">In this section, you create an Event Hub within an Event Hubs namespace.</span></span> <span data-ttu-id="a04aa-152">Můžete také nakonfigurovat centra událostí k zaznamenání dat k účtu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a04aa-152">You also configure the Event Hub to capture data to an Azure Data Lake Store account.</span></span> <span data-ttu-id="a04aa-153">V této části se předpokládá, že jste již vytvořili na obor názvů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="a04aa-153">This section assumes that you have already created an Event Hubs namespace.</span></span>

2. <span data-ttu-id="a04aa-154">Z **přehled** podokně oboru názvů služby Event Hubs, klikněte na tlačítko **+ centra událostí**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-154">From the **Overview** pane of the Event Hubs namespace, click **+ Event Hub**.</span></span>

    <span data-ttu-id="a04aa-155">![Vytvoření centra událostí](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "vytvoření centra událostí")</span><span class="sxs-lookup"><span data-stu-id="a04aa-155">![Create Event Hub](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Create Event Hub")</span></span>

3. <span data-ttu-id="a04aa-156">Zadejte následující hodnoty pro konfiguraci služby Event Hubs k zaznamenání dat do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a04aa-156">Provide the following values to configure Event Hubs to capture data to Data Lake Store.</span></span>

    <span data-ttu-id="a04aa-157">![Vytvoření centra událostí](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "vytvoření centra událostí")</span><span class="sxs-lookup"><span data-stu-id="a04aa-157">![Create Event Hub](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Create Event Hub")</span></span>

    <span data-ttu-id="a04aa-158">a.</span><span class="sxs-lookup"><span data-stu-id="a04aa-158">a.</span></span> <span data-ttu-id="a04aa-159">Zadejte název centra událostí.</span><span class="sxs-lookup"><span data-stu-id="a04aa-159">Provide a name for the Event Hub.</span></span>
    
    <span data-ttu-id="a04aa-160">b.</span><span class="sxs-lookup"><span data-stu-id="a04aa-160">b.</span></span> <span data-ttu-id="a04aa-161">V tomto kurzu nastavit **počet oddílů** a **uchování zpráv** na výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a04aa-161">For this tutorial, set **Partition Count** and **Message Retention** to the default values.</span></span>
    
    <span data-ttu-id="a04aa-162">c.</span><span class="sxs-lookup"><span data-stu-id="a04aa-162">c.</span></span> <span data-ttu-id="a04aa-163">Nastavit **zaznamenat** k **na**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-163">Set **Capture** to **On**.</span></span> <span data-ttu-id="a04aa-164">Nastavte **časové okno** (jak často se zaznamenat) a **velikost okna** (velikost dat k zachycení).</span><span class="sxs-lookup"><span data-stu-id="a04aa-164">Set the **Time Window** (how frequently to capture) and **Size Window** (data size to capture).</span></span> 
    
    <span data-ttu-id="a04aa-165">d.</span><span class="sxs-lookup"><span data-stu-id="a04aa-165">d.</span></span> <span data-ttu-id="a04aa-166">Pro **zaznamenání zprostředkovatele**, vyberte **Azure Data Lake Store** a vyberte Data Lake Store jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="a04aa-166">For **Capture Provider**, select **Azure Data Lake Store** and the select the Data Lake Store you created earlier.</span></span> <span data-ttu-id="a04aa-167">Pro **Data Lake cesta**, zadejte název složky, kterou jste vytvořili v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a04aa-167">For **Data Lake Path**, enter the name of the folder you created in the Data Lake Store account.</span></span> <span data-ttu-id="a04aa-168">Stačí zadat relativní cestu ke složce.</span><span class="sxs-lookup"><span data-stu-id="a04aa-168">You only need to provide the relative path to the folder.</span></span>

    <span data-ttu-id="a04aa-169">e.</span><span class="sxs-lookup"><span data-stu-id="a04aa-169">e.</span></span> <span data-ttu-id="a04aa-170">Ponechte **ukázka zachycení formátů název** na výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a04aa-170">Leave the **Sample capture file name formats** to the default value.</span></span> <span data-ttu-id="a04aa-171">Tato možnost řídí strukturu složek, který se vytvoří ve složce zachycení.</span><span class="sxs-lookup"><span data-stu-id="a04aa-171">This option governs the folder structure that is created under the capture folder.</span></span>

    <span data-ttu-id="a04aa-172">f.</span><span class="sxs-lookup"><span data-stu-id="a04aa-172">f.</span></span> <span data-ttu-id="a04aa-173">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a04aa-173">Click **Create**.</span></span>

## <a name="test-the-setup"></a><span data-ttu-id="a04aa-174">Test nastavení</span><span class="sxs-lookup"><span data-stu-id="a04aa-174">Test the setup</span></span>

<span data-ttu-id="a04aa-175">Nyní můžete otestovat řešení odesílá data do centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="a04aa-175">You can now test the solution by sending data to the Azure Event Hub.</span></span> <span data-ttu-id="a04aa-176">Postupujte podle pokynů v [odesílat události do centra událostí Azure](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).</span><span class="sxs-lookup"><span data-stu-id="a04aa-176">Follow the instructions at [Send events to Azure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).</span></span> <span data-ttu-id="a04aa-177">Po spuštění zasílání dat se zobrazí data zobrazená v Data Lake Store pomocí struktura složek, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="a04aa-177">Once you start sending the data, you see the data reflected in Data Lake Store using the folder structure you specified.</span></span> <span data-ttu-id="a04aa-178">Například struktura složek, uvidíte, jak je znázorněno na následujícím snímku obrazovky v Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a04aa-178">For example, you see a folder structure, as shown in the following screenshot, in your Data Lake Store.</span></span>

<span data-ttu-id="a04aa-179">![Ukázka dat pro EventHub v Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "EventHub ukázkových dat v Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="a04aa-179">![Sample EventHub data in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Sample EventHub data in Data Lake Store")</span></span>

> [!NOTE]
> <span data-ttu-id="a04aa-180">I když nemáte zprávy přicházející do centra událostí, Event Hubs prázdné soubory s pouze záhlaví zapisuje do účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a04aa-180">Even if you do not have messages coming into Event Hubs, Event Hubs writes empty files with just the headers into the Data Lake Store account.</span></span> <span data-ttu-id="a04aa-181">Soubory jsou zapsány ve stejném časovém intervalu, zda jste zadali při vytváření centra událostí.</span><span class="sxs-lookup"><span data-stu-id="a04aa-181">The files are written at the same time interval that you provided while creating the Event Hubs.</span></span>
> 
>

## <a name="analyze-data-in-data-lake-store"></a><span data-ttu-id="a04aa-182">Analýza dat v Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a04aa-182">Analyze data in Data Lake Store</span></span>

<span data-ttu-id="a04aa-183">Jakmile jsou data v Data Lake Store, můžete spustit analytické úlohy a proces, zpracujte data.</span><span class="sxs-lookup"><span data-stu-id="a04aa-183">Once the data is in Data Lake Store, you can run analytical jobs to process and crunch the data.</span></span> <span data-ttu-id="a04aa-184">V tématu [USQL Avro příklad](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) o tom, jak to provést pomocí Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="a04aa-184">See [USQL Avro Example](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) on how to do this using Azure Data Lake Analytics.</span></span>
  

## <a name="see-also"></a><span data-ttu-id="a04aa-185">Viz také</span><span class="sxs-lookup"><span data-stu-id="a04aa-185">See also</span></span>
* [<span data-ttu-id="a04aa-186">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a04aa-186">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="a04aa-187">Kopírování dat z úložiště objektů BLOB Azure do Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a04aa-187">Copy data from Azure Storage Blobs to Data Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
