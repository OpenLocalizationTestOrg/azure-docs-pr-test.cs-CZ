---
title: "Nastavte virtuální počítač jako server IPython Poznámkový blok | Microsoft Docs"
description: "Nastavte si virtuální počítač Azure pro použití v prostředí datového vědecké účely IPython serveru pro pokročilou analýzu."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 818617c1-048e-49c2-b941-f9a983f93998
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: xibingao;bradsev
ms.openlocfilehash: 66fd9e5573390ac6faeb82ad5b0f7ddb18d50a77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a><span data-ttu-id="5be15-103">Nastavení virtuálního počítače Azure jako serveru IPython Notebook pro pokročilé analýzy</span><span class="sxs-lookup"><span data-stu-id="5be15-103">Set up an Azure virtual machine as an IPython Notebook server for advanced analytics</span></span>
<span data-ttu-id="5be15-104">Toto téma ukazuje, jak zřídit a nakonfigurovat virtuální počítač Azure pro pokročilou analýzu, který slouží jako součást prostředí vědecké účely data.</span><span class="sxs-lookup"><span data-stu-id="5be15-104">This topic shows how to provision and configure an Azure virtual machine for advanced analytics that can be used as part of a data science environment.</span></span> <span data-ttu-id="5be15-105">Virtuální počítač Windows nakonfigurovaný s Podpora nástrojů, například Poznámkový blok IPython, Azure Storage Explorer, AzCopy, jakož i jiné nástroje, které jsou užitečné pro pokročilou analýzu projekty.</span><span class="sxs-lookup"><span data-stu-id="5be15-105">The Windows virtual machine is configured with supporting tools such as IPython Notebook, Azure Storage Explorer, AzCopy, as well as other utilities that are useful for advanced analytics projects.</span></span> <span data-ttu-id="5be15-106">Azure Storage Explorer a AzCopy, například poskytují pohodlný způsoby odesílání dat do úložiště objektů blob v Azure z místního počítače nebo se stáhne do místního počítače z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5be15-106">Azure Storage Explorer and AzCopy, for example, provide convenient ways to upload data to Azure blob storage from your local machine or to download it to your local machine from blob storage.</span></span>

## <span data-ttu-id="5be15-107"><a name="create-vm"></a>Krok 1: Vytvoření pro obecné účely virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="5be15-107"><a name="create-vm"></a>Step 1: Create a general-purpose Azure virtual machine</span></span>
<span data-ttu-id="5be15-108">Pokud už máte virtuální počítač Azure a chcete jen nastavení serveru na něm IPython Poznámkový blok, můžete tento krok přeskočit a přejít k [krok 2: Přidání koncového bodu pro poznámkové bloky IPython do existujícího virtuálního počítače](#add-endpoint).</span><span class="sxs-lookup"><span data-stu-id="5be15-108">If you already have an Azure virtual machine and just want to set up an IPython Notebook server on it, you can skip this step and proceed to [Step 2: Add an endpoint for IPython Notebooks to an existing virtual machine](#add-endpoint).</span></span>

<span data-ttu-id="5be15-109">Před zahájením procesu vytvoření virtuálního počítače na platformě Azure, budete muset určit velikost počítače, který je potřebný pro zpracování dat pro jejich projektu.</span><span class="sxs-lookup"><span data-stu-id="5be15-109">Before starting the process of creating a virtual machine on Azure, you need to determine the size of the machine that is needed to process the data for their project.</span></span> <span data-ttu-id="5be15-110">Mít menší počítače méně paměti a méně jader procesoru, než větší počítače, ale jsou také levnější.</span><span class="sxs-lookup"><span data-stu-id="5be15-110">Smaller machines have less memory and fewer CPU cores than larger machines, but they are also less expensive.</span></span> <span data-ttu-id="5be15-111">Seznam typů počítačů a ceny, najdete v článku <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">ceny služeb Virtual Machines </a> stránky</span><span class="sxs-lookup"><span data-stu-id="5be15-111">For a list of machine types and prices, see the <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtual Machines Pricing </a> page</span></span>

1. <span data-ttu-id="5be15-112">Přihlaste se k <a href="https://manage.windowsazure.com" target="_blank">portál Azure classic</a>a klikněte na tlačítko **nový** v levém dolním rohu.</span><span class="sxs-lookup"><span data-stu-id="5be15-112">Log in to <a href="https://manage.windowsazure.com" target="_blank">Azure classic portal</a>, and click **New** in the bottom left corner.</span></span> <span data-ttu-id="5be15-113">Bude překryvné okno.</span><span class="sxs-lookup"><span data-stu-id="5be15-113">A window will pop up.</span></span> <span data-ttu-id="5be15-114">Vyberte **výpočetní** -> **VIRTUÁLNÍHO počítače** -> **FROM GALERIE**.</span><span class="sxs-lookup"><span data-stu-id="5be15-114">Select **COMPUTE** -> **VIRTUAL MACHINE** -> **FROM GALLERY**.</span></span>
   
    ![Vytvořit pracovní prostor][24]
2. <span data-ttu-id="5be15-116">Vyberte jednu z následujících bitových kopií:</span><span class="sxs-lookup"><span data-stu-id="5be15-116">Choose one of the following images:</span></span>
   
   * <span data-ttu-id="5be15-117">Windows Server 2012 R2 Datacenter</span><span class="sxs-lookup"><span data-stu-id="5be15-117">Windows Server 2012 R2 Datacenter</span></span>
   * <span data-ttu-id="5be15-118">Windows Server Essentials Experience (Windows Server 2012 R2)</span><span class="sxs-lookup"><span data-stu-id="5be15-118">Windows Server Essentials Experience (Windows Server 2012 R2)</span></span>
     
     <span data-ttu-id="5be15-119">Potom klikněte na šipku vpravo v pravém dolním přejít na další stránku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5be15-119">Then, click the arrow pointing right at the lower right to go the next configuration page.</span></span>
     
     ![Vytvořit pracovní prostor][25]
3. <span data-ttu-id="5be15-121">Zadejte název pro virtuální počítač, který chcete vytvořit, vyberte velikost počítače (výchozí: A3) na základě velikost dat na počítač se bude proces a jak výkonné, který se má počítač (velikost paměti a počet výpočetních jader) , zadejte uživatelské jméno a heslo pro tento počítač.</span><span class="sxs-lookup"><span data-stu-id="5be15-121">Enter a name for the virtual machine you want to create, select the size of the machine (Default: A3) based on the size of the data the machine is going to process and how powerful you want the machine to be (memory size and the number of compute cores), enter a user name and password for the machine.</span></span> <span data-ttu-id="5be15-122">Potom klikněte na šipku vpravo na Přejít na další stránku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5be15-122">Then, click the arrow pointing right to go to the next configuration page.</span></span>
   
    ![Vytvořit pracovní prostor][26]
4. <span data-ttu-id="5be15-124">Vyberte **oblasti nebo vztahů skupiny nebo virtuální síť** obsahující **účet úložiště** , že máte v úmyslu použít pro tento virtuální počítač a potom vyberte daný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="5be15-124">Select the **REGION/AFFINITY GROUP/VIRTUAL NETWORK** that contains the **STORAGE ACCOUNT** that you are planning to use for this virtual machine, and then select that storage account.</span></span> <span data-ttu-id="5be15-125">Přidání koncového bodu v dolní části v **koncové body** pole tak, že zadáte název koncového bodu ("IPython" sem).</span><span class="sxs-lookup"><span data-stu-id="5be15-125">Add an endpoint at the bottom in the **ENDPOINTS**  field by entering the name of the endpoint ("IPython" here).</span></span> <span data-ttu-id="5be15-126">Můžete vybrat libovolný řetězec, jako **název** koncový bod a jakékoliv celé číslo mezi 0 a 65536, který je **k dispozici** jako **veřejný PORT**.</span><span class="sxs-lookup"><span data-stu-id="5be15-126">You can choose any string as the **NAME** of the end point, and any integer between 0 and 65536 that is **available** as the **PUBLIC PORT**.</span></span> <span data-ttu-id="5be15-127">**PRIVÁTNÍ PORT** musí být **9999**.</span><span class="sxs-lookup"><span data-stu-id="5be15-127">The **PRIVATE PORT** has to be **9999**.</span></span> <span data-ttu-id="5be15-128">Měli byste **vyhnout** pomocí veřejného porty, které již byly přiřazeny pro internetové služby.</span><span class="sxs-lookup"><span data-stu-id="5be15-128">You should **avoid** using public ports that have already been assigned for internet services.</span></span> <span data-ttu-id="5be15-129"><a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Porty pro internetové služby</a> poskytuje seznam portů, které byly přiřazeny a je nutno.</span><span class="sxs-lookup"><span data-stu-id="5be15-129"><a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Ports for Internet Services</a> provides a list of ports that have been assigned and should be avoided.</span></span>
   
    ![Vytvořit pracovní prostor][27]
5. <span data-ttu-id="5be15-131">Kliknutím na značku zaškrtnutí zahájíte proces zřizování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5be15-131">Click the check mark to start the virtual machine provisioning process.</span></span>
   
    ![Vytvořit pracovní prostor][28]

<span data-ttu-id="5be15-133">Může trvat 15-25 minut na dokončení procesu zřizování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5be15-133">It may take 15-25 minutes to complete the virtual machine provisioning process.</span></span> <span data-ttu-id="5be15-134">Po vytvoření virtuálního počítače, jako by měl zobrazit stav tohoto počítače **systémem**.</span><span class="sxs-lookup"><span data-stu-id="5be15-134">After the virtual machine has been created, the status of this machine should show as **Running**.</span></span>

![Vytvořit pracovní prostor][29]

## <span data-ttu-id="5be15-136"><a name="add-endpoint"></a>Krok 2: Přidání koncového bodu pro poznámkové bloky IPython do existujícího virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="5be15-136"><a name="add-endpoint"></a>Step 2: Add an endpoint for IPython Notebooks to an existing virtual machine</span></span>
<span data-ttu-id="5be15-137">Pokud jste vytvořili virtuální počítač podle pokynů v kroku 1, již byl přidán koncový bod pro IPython poznámkového bloku a můžete tento krok přeskočen.</span><span class="sxs-lookup"><span data-stu-id="5be15-137">If you created the virtual machine by following the instructions in Step 1, then the endpoint for IPython Notebook has already been added and this step can be skipped.</span></span>

<span data-ttu-id="5be15-138">Pokud virtuální počítač již existuje a je nutné přidat koncový bod pro IPython Poznámkový blok, který bude instalovat v kroku 3 pod prvním přihlášení k portálu Azure classic, vyberte virtuální počítač a přidání koncového bodu pro server IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="5be15-138">If the virtual machine already exists, and you need to add an endpoint for IPython Notebook that you will install in Step 3 below, first login to Azure classic portal, select the virtual machine, and add the endpoint for IPython Notebook server.</span></span> <span data-ttu-id="5be15-139">Následující obrázek obsahuje snímek obrazovky portálu po koncový bod pro IPython poznámkového bloku byl přidán do virtuálního počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="5be15-139">The following figure contains a screen shot of the portal after the endpoint for IPython Notebook has been added to a Windows virtual machine.</span></span>

![Vytvořit pracovní prostor][17]

## <span data-ttu-id="5be15-141"><a name="run-commands"></a>Krok 3: Instalace IPython Poznámkový blok a další podpůrné nástroje</span><span class="sxs-lookup"><span data-stu-id="5be15-141"><a name="run-commands"></a>Step 3: Install IPython Notebook and other supporting tools</span></span>
<span data-ttu-id="5be15-142">Po vytvoření virtuálního počítače pomocí protokolu RDP (Remote Desktop) pro přihlášení k systému Windows virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5be15-142">After the virtual machine is created, use Remote Desktop Protocol (RDP) to log on to the Windows virtual machine.</span></span> <span data-ttu-id="5be15-143">Pokyny najdete v tématu [postup Přihlaste se k Windows serveru spuštěný virtuální počítač](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5be15-143">For instructions, see [How to Log on to a Virtual Machine Running Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="5be15-144">Otevřete **příkazového řádku** (**není příkazové okno prostředí Powershell**) jako **správce** a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="5be15-144">Open the **Command Prompt** (**Not the Powershell command window**) as an **Administrator** and run the following command.</span></span>

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

<span data-ttu-id="5be15-145">Po dokončení instalace serveru IPython poznámkového bloku se spouští automaticky v *C:\\uživatelé\\\<uživatelské jméno\>\\dokumenty\\IPython poznámkových bloků*  adresáře.</span><span class="sxs-lookup"><span data-stu-id="5be15-145">When the installation completes, the IPython Notebook server is launched automatically in the *C:\\Users\\\<user name\>\\Documents\\IPython Notebooks* directory.</span></span>

<span data-ttu-id="5be15-146">Po zobrazení výzvy zadejte heslo pro IPython poznámkového bloku a heslo správce počítače.</span><span class="sxs-lookup"><span data-stu-id="5be15-146">When prompted, enter a password for the IPython Notebook and the password of the machine administrator.</span></span> <span data-ttu-id="5be15-147">To umožňuje IPython poznámkového bloku ke spuštění jako služba na počítači.</span><span class="sxs-lookup"><span data-stu-id="5be15-147">This enables the IPython Notebook to run as a service on the machine.</span></span>

## <span data-ttu-id="5be15-148"><a name="access"></a>Krok 4: Poznámkových bloků IPython přístup z webového prohlížeče</span><span class="sxs-lookup"><span data-stu-id="5be15-148"><a name="access"></a>Step 4: Access IPython Notebooks from a web browser</span></span>
<span data-ttu-id="5be15-149">Pro přístup k serveru IPython poznámkového bloku, otevřete webového prohlížeče a vstup *https://&#60;virtual název DNS počítače >: & č. 60; veřejný port číslo >* do textového pole adresy URL.</span><span class="sxs-lookup"><span data-stu-id="5be15-149">To access the IPython Notebook server, open a web browser, and input *https://&#60;virtual machine DNS name>:&#60;public port number>* in the URL text box.</span></span> <span data-ttu-id="5be15-150">Zde *& č. 60; veřejný port číslo >* by měla být číslo portu, který jste zadali, pokud byl přidán koncový bod IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="5be15-150">Here, the *&#60;public port number>* should  be the port number you specified when the IPython Notebook endpoint was added.</span></span>

<span data-ttu-id="5be15-151">*& Č. 60; název DNS virtuálního počítače >* najdete na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="5be15-151">The *&#60;virtual machine DNS name>* can be found at the Azure classic portal.</span></span> <span data-ttu-id="5be15-152">Po přihlášení k portálu classic, klikněte na tlačítko **virtuální počítače**, vyberte počítač, který jste vytvořili a potom vyberte **řídicí panel**, název DNS se zobrazí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5be15-152">After logging in to the classic portal, click **VIRTUAL MACHINES**, select the machine you created, and then select **DASHBOARD**, the DNS name will be shown as follows:</span></span>

![Vytvořit pracovní prostor][19]

<span data-ttu-id="5be15-154">Upozornění oznamující, že se setkají *došlo k potížím s certifikátem zabezpečení tohoto webu* (Internet Explorer) nebo *připojení není privátní* (Chrome), jak je znázorněno v následující obrázky .</span><span class="sxs-lookup"><span data-stu-id="5be15-154">You will encounter a warning stating that *There is a problem with this website's security certificate* (Internet Explorer) or *Your connection is not private* (Chrome), as shown in the following figures.</span></span> <span data-ttu-id="5be15-155">Klikněte na tlačítko **pokračovat na tento web (nedoporučujeme)** (Internet Explorer) nebo **Upřesnit** a potom  **pokračovat na & č. 60;* Název DNS*> (unsafe) ** (Chrome) Chcete-li pokračovat.</span><span class="sxs-lookup"><span data-stu-id="5be15-155">Click **Continue to this website (not recommended)** (Internet Explorer) or **Advanced** and then **Proceed to &#60;*DNS Name*> (unsafe)** (Chrome) to continue.</span></span> <span data-ttu-id="5be15-156">Pak zadejte heslo, které jste dříve zadaný pro přístup k IPython poznámkových bloků.</span><span class="sxs-lookup"><span data-stu-id="5be15-156">Then input the password you specified earlier to access the IPython Notebooks.</span></span>

<span data-ttu-id="5be15-157">**Internet Explorer:**
![vytvořit pracovní prostor][20]</span><span class="sxs-lookup"><span data-stu-id="5be15-157">**Internet Explorer:**
![Create workspace][20]</span></span>

<span data-ttu-id="5be15-158">**Chrome:**
![vytvořit pracovní prostor][21]</span><span class="sxs-lookup"><span data-stu-id="5be15-158">**Chrome:**
![Create workspace][21]</span></span>

<span data-ttu-id="5be15-159">Po přihlášení k IPython poznámkového bloku, adresář *DataScienceSamples* se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="5be15-159">After you log on to the IPython Notebook, a directory *DataScienceSamples* will show on the browser.</span></span> <span data-ttu-id="5be15-160">Tento adresář obsahuje ukázkové poznámkových bloků IPython, které Microsoftu pomáhají uživatelům provedení úlohy vědecké účely dat sdílí.</span><span class="sxs-lookup"><span data-stu-id="5be15-160">This directory contains sample IPython Notebooks that are shared by Microsoft to help users conduct data science tasks.</span></span> <span data-ttu-id="5be15-161">Poznámkové bloky tyto ukázkové IPython jsou rezervovány z [ **úložiště GitHub** ](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) na virtuální počítače během procesu instalace serveru IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="5be15-161">These sample IPython Notebooks are checked out from [**GitHub repository**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) to the virtual machines during the IPython Notebook server setup process.</span></span> <span data-ttu-id="5be15-162">Společnost Microsoft udržuje a často aktualizuje toto úložiště.</span><span class="sxs-lookup"><span data-stu-id="5be15-162">Microsoft maintains and updates this repository frequently.</span></span> <span data-ttu-id="5be15-163">Uživatelé můžou navštívit úložišti GitHub, abyste si nedávno aktualizovaného notebooky IPython ukázka.</span><span class="sxs-lookup"><span data-stu-id="5be15-163">Users may visit the GitHub repository to get the most recently updated sample IPython Notebooks.</span></span>
<span data-ttu-id="5be15-164">![Vytvořit pracovní prostor][18]</span><span class="sxs-lookup"><span data-stu-id="5be15-164">![Create workspace][18]</span></span>

## <span data-ttu-id="5be15-165"><a name="upload"></a>Krok 5: Nahrajte existující IPython poznámkového bloku z místního počítače k serveru IPython Poznámkový blok</span><span class="sxs-lookup"><span data-stu-id="5be15-165"><a name="upload"></a>Step 5: Upload an existing IPython Notebook from a local machine to the IPython Notebook server</span></span>
<span data-ttu-id="5be15-166">Poznámkových bloků IPython poskytují snadný způsob pro uživatelům odesílat existujícího poznámkového bloku IPython na své místní počítače k serveru IPython poznámkového bloku na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="5be15-166">IPython Notebooks provide an easy way for users to upload an existing IPython Notebook on their local machines to the IPython Notebook server on the virtual machines.</span></span> <span data-ttu-id="5be15-167">Po přihlášení do poznámkového bloku IPython ve webovém prohlížeči, klikněte na tlačítko do **directory** který poznámkového bloku IPython bude nahrán do.</span><span class="sxs-lookup"><span data-stu-id="5be15-167">After you log on to the IPython Notebook in a web browser, click into the **directory** that the IPython Notebook will be uploaded to.</span></span> <span data-ttu-id="5be15-168">Potom vyberte soubor poznámkového bloku IPython .ipynb nahrát z místního počítače v **Průzkumníka souborů**a přetáhněte ji do poznámkového bloku IPython adresáře na webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="5be15-168">Then, select an IPython Notebook .ipynb file to upload from the local machine in the **File Explorer**, and drag and drop it to the IPython Notebook directory on the web browser.</span></span> <span data-ttu-id="5be15-169">Klikněte **nahrát** tlačítko Nahrát soubor .ipynb k serveru IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="5be15-169">Click the **Upload** button, to upload the .ipynb file to the IPython Notebook server.</span></span> <span data-ttu-id="5be15-170">Ostatní uživatelé pak jej začít používat v z webových prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="5be15-170">Other users can then start using it in from their web browsers.</span></span>

![Vytvořit pracovní prostor][22]

![Vytvořit pracovní prostor][23]

## <span data-ttu-id="5be15-173"><a name="shutdown"></a>Vypnout a zrušit přidělení virtuálního počítače, když není používán</span><span class="sxs-lookup"><span data-stu-id="5be15-173"><a name="shutdown"></a>Shut down and de-allocate virtual machine when not in use</span></span>
<span data-ttu-id="5be15-174">Virtuální počítače Azure jsou cenově jako **platíte jenom pro co používáte**.</span><span class="sxs-lookup"><span data-stu-id="5be15-174">Azure Virtual Machines are priced as **pay only for what you use**.</span></span> <span data-ttu-id="5be15-175">Aby se zajistilo, že nejsou se fakturuje Pokud nepoužíváte virtuální počítač, musí být v **zastaveno (Deallocated)** stavu, když není používán.</span><span class="sxs-lookup"><span data-stu-id="5be15-175">To ensure that you are not being billed when not using your virtual machine, it has to be in the **Stopped (Deallocated)** state when not in use.</span></span>

> [!NOTE]
> <span data-ttu-id="5be15-176">Pokud vypnout virtuální počítač z ve virtuálním počítači (s použitím možnosti napájení systému Windows), virtuální počítač je zastavena, ale zůstává přidělené.</span><span class="sxs-lookup"><span data-stu-id="5be15-176">If you shut down the virtual machine from inside the VM (using Windows power options), the VM is stopped but remains allocated.</span></span> <span data-ttu-id="5be15-177">Aby se zajistilo, není nadále účtovány poplatky, vždy zastaví virtuální počítače z [portál Azure classic](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="5be15-177">To ensure you do not continue to be billed, always stop virtual machines from the [Azure classic portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="5be15-178">Můžete také zastavit virtuální počítač pomocí prostředí Powershell voláním **ShutdownRoleOperation** s "PostShutdownAction" rovno "StoppedDeallocated".</span><span class="sxs-lookup"><span data-stu-id="5be15-178">You can also stop the VM through Powershell by calling **ShutdownRoleOperation** with "PostShutdownAction" equal to "StoppedDeallocated".</span></span>
> 
> 

<span data-ttu-id="5be15-179">Vypnutí a zrušit přidělení virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="5be15-179">To shut down and deallocate the virtual machine:</span></span>

1. <span data-ttu-id="5be15-180">Přihlaste se k [portál Azure classic](http://manage.windowsazure.com/) pomocí svého účtu.</span><span class="sxs-lookup"><span data-stu-id="5be15-180">Log in to the [Azure classic portal](http://manage.windowsazure.com/) using your account.</span></span>  
2. <span data-ttu-id="5be15-181">Vyberte **virtuální počítače** levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="5be15-181">Select **VIRTUAL MACHINES** from the left navigation bar.</span></span>
3. <span data-ttu-id="5be15-182">V seznamu virtuálních počítačů, klikněte na název virtuálního počítače potom přejděte na stránku **řídicí panel** stránky.</span><span class="sxs-lookup"><span data-stu-id="5be15-182">In the list of virtual machines, click on the name of your virtual machine then go to the **DASHBOARD** page.</span></span>
4. <span data-ttu-id="5be15-183">V dolní části stránky klikněte na tlačítko **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="5be15-183">At the bottom of the page, click **SHUTDOWN**.</span></span>

![Vypnutí virtuálního počítače][15]

<span data-ttu-id="5be15-185">Virtuální počítač bude zrušeno, ale nebyl odstraněn.</span><span class="sxs-lookup"><span data-stu-id="5be15-185">The virtual machine will be deallocated but not deleted.</span></span> <span data-ttu-id="5be15-186">Kdykoli z portálu Azure classic můžete restartovat virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5be15-186">You may restart your virtual machine at any time from the Azure classic portal.</span></span>

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a><span data-ttu-id="5be15-187">Virtuální počítač Azure je připravené k použití: co je další?</span><span class="sxs-lookup"><span data-stu-id="5be15-187">Your Azure VM is ready to use: what's next?</span></span>
<span data-ttu-id="5be15-188">Virtuální počítač je nyní připraven k použití v cvičení vědecké účely vaše data.</span><span class="sxs-lookup"><span data-stu-id="5be15-188">Your virtual machine is now ready to use in your data science exercises.</span></span> <span data-ttu-id="5be15-189">Virtuální počítač je taky připravené pro použití jako server IPython Poznámkový blok pro zkoumání a zpracování dat a dalších úloh ve spojení s Azure Machine Learning a vědecké účely procesu dat Team.</span><span class="sxs-lookup"><span data-stu-id="5be15-189">The virtual machine is also ready for use as an IPython Notebook server for the exploration and processing of data, and other tasks in conjunction with Azure Machine Learning and the Team Data Science Process.</span></span>

<span data-ttu-id="5be15-190">Další kroky v procesu Team dat. vědecké účely, jsou namapované v [učení cesta](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesun dat do HDInsight, proces a ukázkové ho existuje v rámci přípravy learning z dat pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5be15-190">The next steps in the Team Data Science Process are mapped in the [Learning Path](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) and may include steps that move data into HDInsight, process and sample it there in preparation for learning from the data with Azure Machine Learning.</span></span>

[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png