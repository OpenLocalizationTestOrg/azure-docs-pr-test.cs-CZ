---
title: "Vědecké zpracování dat na datové vědě virtuální počítač Linux | Microsoft Docs"
description: "Jak provést několik běžných úkolů data vědecké účely pomocí virtuálního počítače s Linuxem dat vědecké účely."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev;paulsh
ms.openlocfilehash: 6da9a8e3f9f8ac851c2a8deb861ac1d0b3ec5874
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="data-science-on-the-linux-data-science-virtual-machine"></a><span data-ttu-id="74c66-103">Vědecké zkoumání dat na virtuálním počítači pro vědecké zkoumání dat s Linuxem</span><span class="sxs-lookup"><span data-stu-id="74c66-103">Data science on the Linux Data Science Virtual Machine</span></span>
<span data-ttu-id="74c66-104">Tento návod ukazuje, jak provést několik běžných úkolů data vědecké účely pomocí virtuálního počítače s Linuxem dat vědecké účely.</span><span class="sxs-lookup"><span data-stu-id="74c66-104">This walkthrough shows you how to perform several common data science tasks with the Linux Data Science VM.</span></span> <span data-ttu-id="74c66-105">Virtuální počítač pro vědecké účely Data Linux (DSVM) je bitová kopie virtuálního počítače, která je k dispozici v Azure, který je předem nainstalovaná s kolekcí nástrojů pro běžně používané k analýze dat a strojové učení.</span><span class="sxs-lookup"><span data-stu-id="74c66-105">The Linux Data Science Virtual Machine (DSVM) is a virtual machine image available on Azure that is pre-installed with a collection of tools commonly used for data analytics and machine learning.</span></span> <span data-ttu-id="74c66-106">Klíčové softwarové komponenty je uvedeno v [zřízení virtuálního počítače Linux datové vědy](machine-learning-data-science-linux-dsvm-intro.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="74c66-106">The key software components are itemized in the [Provision the Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md) topic.</span></span> <span data-ttu-id="74c66-107">Image virtuálního počítače lze snadno začít provádění vědecké zpracování dat v minutách, bez nutnosti instalace a konfigurace každého nástroje jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="74c66-107">The VM image makes it easy to get started doing data science in minutes, without having to install and configure each of the tools individually.</span></span> <span data-ttu-id="74c66-108">Můžete snadno škálování virtuálních počítačů, v případě potřeby a zastavte ji když není používán.</span><span class="sxs-lookup"><span data-stu-id="74c66-108">You can easily scale up the VM, if needed, and stop it when not in use.</span></span> <span data-ttu-id="74c66-109">Proto tento prostředek je elastické a nákladově efektivní.</span><span class="sxs-lookup"><span data-stu-id="74c66-109">So this resource is both elastic and cost-efficient.</span></span>

<span data-ttu-id="74c66-110">Data úlohy vědecké účely ukázáno v tomto návodu postupujte podle kroků uvedených v [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span><span class="sxs-lookup"><span data-stu-id="74c66-110">The data science tasks demonstrated in this walkthrough follow the steps outlined in the [Team Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span></span> <span data-ttu-id="74c66-111">Tento proces zajišťuje systematicky k vědecké zpracování dat, které týmům datových vědců efektivně spolupracovat přes životního cyklu vytváření inteligentní aplikace.</span><span class="sxs-lookup"><span data-stu-id="74c66-111">This process provides a systematic approach to data science that enables teams of data scientists to effectively collaborate over the lifecycle of building intelligent applications.</span></span> <span data-ttu-id="74c66-112">Proces vědecké účely dat také poskytuje iterativní framework vědecké zpracování dat, který může následovat jednotlivce.</span><span class="sxs-lookup"><span data-stu-id="74c66-112">The data science process also provides an iterative framework for data science that can be followed by an individual.</span></span>

<span data-ttu-id="74c66-113">Budeme analyzovat [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) datovou sadu v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="74c66-113">We analyze the [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset in this walkthrough.</span></span> <span data-ttu-id="74c66-114">Je to sada e-mailů, které jsou označeny jako nevyžádané pošty nebo šunkou (tj. nejsou nevyžádané pošty), a také obsahuje statistikami o obsah e-mailů.</span><span class="sxs-lookup"><span data-stu-id="74c66-114">This is a set of emails that are marked as either spam or ham (meaning they are not spam), and also contains some statistics on the content of the emails.</span></span> <span data-ttu-id="74c66-115">Statistiky zahrnuty jsou popsané v dalším ale jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="74c66-115">The statistics included are discussed in the next but one section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74c66-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74c66-116">Prerequisites</span></span>
<span data-ttu-id="74c66-117">Než budete moct použít virtuální počítač s Linuxem dat vědecké účely, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="74c66-117">Before you can use a Linux Data Science Virtual Machine, you must have the following:</span></span>

* <span data-ttu-id="74c66-118">**Předplatné**.</span><span class="sxs-lookup"><span data-stu-id="74c66-118">An **Azure subscription**.</span></span> <span data-ttu-id="74c66-119">Pokud není již nemáte, přečtěte si téma [vytvořit účet Azure zdarma Dnes](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="74c66-119">If you do not already have one, see [Create your free Azure account today](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="74c66-120">A [ **Linux vědecké zpracování dat virtuálních počítačů**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="74c66-120">A [**Linux data science VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span></span> <span data-ttu-id="74c66-121">Informace o zřizování tohoto virtuálního počítače najdete v tématu [zřízení virtuálního počítače Linux datové vědy](machine-learning-data-science-linux-dsvm-intro.md).</span><span class="sxs-lookup"><span data-stu-id="74c66-121">For information on provisioning this VM, see [Provision the Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md).</span></span>
* <span data-ttu-id="74c66-122">[X2Go](http://wiki.x2go.org/doku.php) v počítači nainstalován a otevřít relaci XFCE.</span><span class="sxs-lookup"><span data-stu-id="74c66-122">[X2Go](http://wiki.x2go.org/doku.php) installed on your computer and opened an XFCE session.</span></span> <span data-ttu-id="74c66-123">Informace o instalaci a konfiguraci **X2Go klienta**, najdete v části [instalace a konfigurace klienta X2Go](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span><span class="sxs-lookup"><span data-stu-id="74c66-123">For information on installing and configuring an **X2Go client**, see [Installing and configuring X2Go client](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span></span> 
* <span data-ttu-id="74c66-124">**AzureML účet**.</span><span class="sxs-lookup"><span data-stu-id="74c66-124">An **AzureML account**.</span></span> <span data-ttu-id="74c66-125">Pokud jste ještě nemáte, zaregistrujte si novým na [domovské stránky AzureML](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="74c66-125">If you don't already have one, sign up for new one at the [AzureML homepage](https://studio.azureml.net/).</span></span> <span data-ttu-id="74c66-126">Není k dispozici bezplatná využití vrstvu vám pomůže začít.</span><span class="sxs-lookup"><span data-stu-id="74c66-126">There is a free usage tier to help you get started.</span></span>

## <a name="download-the-spambase-dataset"></a><span data-ttu-id="74c66-127">Stáhněte si spambase datové sady</span><span class="sxs-lookup"><span data-stu-id="74c66-127">Download the spambase dataset</span></span>
<span data-ttu-id="74c66-128">[Spambase](https://archive.ics.uci.edu/ml/datasets/spambase) datové sady je poměrně malý sada dat, která obsahuje jenom 4601 příklady.</span><span class="sxs-lookup"><span data-stu-id="74c66-128">The [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset is a relatively small set of data that contains only 4601 examples.</span></span> <span data-ttu-id="74c66-129">Toto je vhodné velikost pro použití při demonstraci toho, že některé klíčové funkce virtuálního počítače vědecké účely Data, jak se udržuje požadavky na prostředky mírné.</span><span class="sxs-lookup"><span data-stu-id="74c66-129">This is a convenient size to use when demonstrating that some of the key features of the Data Science VM as it keeps the resource requirements modest.</span></span>

> [!NOTE]
> <span data-ttu-id="74c66-130">Tento návod byl vytvořen na D2 v2 velikost datové vědy virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="74c66-130">This walkthrough was created on a D2 v2-sized Linux Data Science Virtual Machine.</span></span> <span data-ttu-id="74c66-131">Umožňuje zpracovávat postupy v tomto návodu je této velikosti DSVM.</span><span class="sxs-lookup"><span data-stu-id="74c66-131">This size DSVM is capable of handling the procedures in this walkthrough.</span></span>
>
>

<span data-ttu-id="74c66-132">Pokud potřebujete další prostor úložiště, můžete vytvořit další disky a připojte je k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="74c66-132">If you need more storage space, you can create additional disks and attach them to your VM.</span></span> <span data-ttu-id="74c66-133">Tyto disky používají trvalé úložiště Azure, takže jejich data se zachová, i když server je znovu poskytnuto kvůli změně velikosti nebo je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="74c66-133">These disks use persistent Azure storage, so their data is preserved even when the server is reprovisioned due to resizing or is shut down.</span></span> <span data-ttu-id="74c66-134">Přidejte disk a jeho připojení k virtuálnímu počítači, postupujte podle pokynů v [přidejte disk do virtuálního počítače s Linuxem](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="74c66-134">To add a disk and attach it to your VM, follow the instructions in [Add a disk to a Linux VM](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="74c66-135">Tyto kroky použijte rozhraní příkazového řádku Azure (Azure CLI), který je již nainstalován na DSVM.</span><span class="sxs-lookup"><span data-stu-id="74c66-135">These steps use the Azure Command-Line Interface (Azure CLI), which is already installed on the DSVM.</span></span> <span data-ttu-id="74c66-136">Proto tyto postupy lze provést zcela z virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="74c66-136">So these procedures can be done entirely from the VM itself.</span></span> <span data-ttu-id="74c66-137">Chcete-li zvýšit velikost úložiště Další možností je použít [soubory Azure](../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="74c66-137">Another option to increase storage is to use [Azure files](../storage/files/storage-how-to-use-files-linux.md).</span></span>

<span data-ttu-id="74c66-138">Chcete-li stáhnout data, otevřete okno terminálu a spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="74c66-138">To download the data, open a terminal window and run this command:</span></span>

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

<span data-ttu-id="74c66-139">Stažený soubor nemá řádek záhlaví, můžeme vytvořit jiný soubor, který má hlavičku.</span><span class="sxs-lookup"><span data-stu-id="74c66-139">The downloaded file does not have a header row, so let's create another file that does have a header.</span></span> <span data-ttu-id="74c66-140">Spusťte tento příkaz k vytvoření souboru s příslušnou hlavičky:</span><span class="sxs-lookup"><span data-stu-id="74c66-140">Run this command to create a file with the appropriate headers:</span></span>

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

<span data-ttu-id="74c66-141">Potom řetězení dva soubory společně s příkaz:</span><span class="sxs-lookup"><span data-stu-id="74c66-141">Then concatenate the two files together with the command:</span></span>

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

<span data-ttu-id="74c66-142">Datová sada má několik typů statistik na každou e-mailu:</span><span class="sxs-lookup"><span data-stu-id="74c66-142">The dataset has several types of statistics on each email:</span></span>

* <span data-ttu-id="74c66-143">Sloupce jako ***word\_frekvence\_WORD*** s procentem slova v e-mailu, které odpovídají *WORD*.</span><span class="sxs-lookup"><span data-stu-id="74c66-143">Columns like ***word\_freq\_WORD*** indicate the percentage of words in the email that match *WORD*.</span></span> <span data-ttu-id="74c66-144">Například pokud *word\_frekvence\_zkontrolujte* je 1, 1 % všechna slova v e-mailu měla *zkontrolujte*.</span><span class="sxs-lookup"><span data-stu-id="74c66-144">For example, if *word\_freq\_make* is 1, then 1% of all words in the email were *make*.</span></span>
* <span data-ttu-id="74c66-145">Sloupce jako ***char\_frekvence\_CHAR*** znamenat procento všech znaků v e-mailu, které byly *CHAR*.</span><span class="sxs-lookup"><span data-stu-id="74c66-145">Columns like ***char\_freq\_CHAR*** indicate the percentage of all characters in the email that were *CHAR*.</span></span>
* <span data-ttu-id="74c66-146">***kapitálové\_spustit\_délka\_nejdelší*** je nejdelší délka posloupnost velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="74c66-146">***capital\_run\_length\_longest*** is the longest length of a sequence of capital letters.</span></span>
* <span data-ttu-id="74c66-147">***kapitálové\_spustit\_délka\_průměrná*** je průměrná délka všech pořadí velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="74c66-147">***capital\_run\_length\_average*** is the average length of all sequences of capital letters.</span></span>
* <span data-ttu-id="74c66-148">***kapitálové\_spustit\_délka\_celkový*** je celková délka všech pořadí velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="74c66-148">***capital\_run\_length\_total*** is the total length of all sequences of capital letters.</span></span>
* <span data-ttu-id="74c66-149">***nevyžádané pošty*** označuje, zda e-mailu byla považována za spam nebo ne (1 = nevyžádané pošty, 0 = není nevyžádané poště).</span><span class="sxs-lookup"><span data-stu-id="74c66-149">***spam*** indicates whether the email was considered spam or not (1 = spam, 0 = not spam).</span></span>

## <a name="explore-the-dataset-with-microsoft-r-open"></a><span data-ttu-id="74c66-150">Prozkoumejte datovou sadu s Microsoft R otevřete</span><span class="sxs-lookup"><span data-stu-id="74c66-150">Explore the dataset with Microsoft R Open</span></span>
<span data-ttu-id="74c66-151">Umožňuje kontrolovat data a provádět některé základní strojového učení s R. Virtuální počítač vědecké účely dat se dodává s [Microsoft R otevřete](https://mran.revolutionanalytics.com/open/) předinstalován.</span><span class="sxs-lookup"><span data-stu-id="74c66-151">Let's examine the data and do some basic machine learning with R. The Data Science VM comes with [Microsoft R Open](https://mran.revolutionanalytics.com/open/) pre-installed.</span></span> <span data-ttu-id="74c66-152">Knihovny math s více vlákny v této verzi R nabízí lepší výkon než různých verzí jednovláknové.</span><span class="sxs-lookup"><span data-stu-id="74c66-152">The multithreaded math libraries in this version of R offer better performance than various single-threaded versions.</span></span> <span data-ttu-id="74c66-153">Microsoft R otevřete také poskytuje reprodukovatelnosti pomocí snímku úložiště balíčků CRAN.</span><span class="sxs-lookup"><span data-stu-id="74c66-153">Microsoft R Open also provides reproducibility by using a snapshot of the CRAN package repository.</span></span>

<span data-ttu-id="74c66-154">Kopií ukázky kódu, který je použit v tomto názorném postupu získáte klonovat **Azure-Machine-Learning--vědecké zpracování dat** úložiště pomocí git, který je předem nainstalovaná ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="74c66-154">To get copies of the code samples used in this walkthrough, clone the **Azure-Machine-Learning-Data-Science** repository using git, which is pre-installed on the VM.</span></span> <span data-ttu-id="74c66-155">Z příkazového řádku git spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="74c66-155">From the git command line, run:</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="74c66-156">Otevřete okno terminálu a spusťte novou relaci R s R interaktivní konzoly.</span><span class="sxs-lookup"><span data-stu-id="74c66-156">Open a terminal window and start a new R session with the R interactive console.</span></span>

> [!NOTE]
> <span data-ttu-id="74c66-157">Následující postupy můžete použít také Rstudia.</span><span class="sxs-lookup"><span data-stu-id="74c66-157">You can also use RStudio for the following procedures.</span></span> <span data-ttu-id="74c66-158">Chcete-li nainstalovat Rstudia, spusťte tento příkaz v terminálu:`./Desktop/DSVM\ tools/installRStudio.sh`</span><span class="sxs-lookup"><span data-stu-id="74c66-158">To install RStudio, execute this command at a terminal: `./Desktop/DSVM\ tools/installRStudio.sh`</span></span>
>
>

<span data-ttu-id="74c66-159">Pokud chcete importovat data a nastavení prostředí, spusťte:</span><span class="sxs-lookup"><span data-stu-id="74c66-159">To import the data and set up the environment, run:</span></span>

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

<span data-ttu-id="74c66-160">Pokud chcete zobrazit souhrnné statistické údaje o jednotlivých sloupců:</span><span class="sxs-lookup"><span data-stu-id="74c66-160">To see summary statistics about each column:</span></span>

    summary(data)

<span data-ttu-id="74c66-161">Pro jiné zobrazení dat:</span><span class="sxs-lookup"><span data-stu-id="74c66-161">For a different view of the data:</span></span>

    str(data)

<span data-ttu-id="74c66-162">To ukazuje typ každou proměnnou a první několik hodnot v datové sadě.</span><span class="sxs-lookup"><span data-stu-id="74c66-162">This shows you the type of each variable and the first few values in the dataset.</span></span>

<span data-ttu-id="74c66-163">*Nevyžádané pošty* sloupec byl načten jako celé číslo, ale je ve skutečnosti kategorií proměnné (nebo multi-Factor).</span><span class="sxs-lookup"><span data-stu-id="74c66-163">The *spam* column was read as an integer, but it's actually a categorical variable (or factor).</span></span> <span data-ttu-id="74c66-164">Nastavte její typ:</span><span class="sxs-lookup"><span data-stu-id="74c66-164">To set its type:</span></span>

    data$spam <- as.factor(data$spam)

<span data-ttu-id="74c66-165">Chcete-li provést některé průzkumné analýzy, použijte [ggplot2](http://ggplot2.org/) balíček oblíbených grafovým knihovny pro R, který už je nainstalovaný na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="74c66-165">To do some exploratory analysis, use the [ggplot2](http://ggplot2.org/) package, a popular graphing library for R that is already installed on the VM.</span></span> <span data-ttu-id="74c66-166">Poznámka od souhrnná data zobrazit dřív, máme souhrnné statistiky o frekvenci znak vykřičník.</span><span class="sxs-lookup"><span data-stu-id="74c66-166">Note, from the summary data displayed earlier, that we have summary statistics on the frequency of the exclamation mark character.</span></span> <span data-ttu-id="74c66-167">Umožňuje vykreslení těchto frekvencí zde pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="74c66-167">Let's plot those frequencies here with the following commands:</span></span>

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="74c66-168">Vzhledem k tomu, že nulové panelu je zkosení vykreslení, budeme se jich zbavit:</span><span class="sxs-lookup"><span data-stu-id="74c66-168">Since the zero bar is skewing the plot, let's get rid of it:</span></span>

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="74c66-169">Je větší než 1, která vypadá zajímavé netriviální hustotu.</span><span class="sxs-lookup"><span data-stu-id="74c66-169">There is a non-trivial density above 1 that looks interesting.</span></span> <span data-ttu-id="74c66-170">Podívejme se na právě tato data:</span><span class="sxs-lookup"><span data-stu-id="74c66-170">Let's look at just that data:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="74c66-171">Potom ji rozdělte podle šunkou vs nevyžádané pošty:</span><span class="sxs-lookup"><span data-stu-id="74c66-171">Then split it by spam vs ham:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

<span data-ttu-id="74c66-172">Tyto příklady měli povolit vám umožní provádět podobné pozemků ostatních sloupců a prozkoumejte data obsažená v nich.</span><span class="sxs-lookup"><span data-stu-id="74c66-172">These examples should enable you to make similar plots of the other columns to explore the data contained in them.</span></span>

## <a name="train-and-test-an-ml-model"></a><span data-ttu-id="74c66-173">Natrénuje a otestuje ML model</span><span class="sxs-lookup"><span data-stu-id="74c66-173">Train and test an ML model</span></span>
<span data-ttu-id="74c66-174">Nyní Pojďme cvičení několik modelů machine learning klasifikovat e-mailů v datové sadě jako obsahující span nebo šunkou.</span><span class="sxs-lookup"><span data-stu-id="74c66-174">Now let's train a couple of machine learning models to classify the emails in the dataset as containing either span or ham.</span></span> <span data-ttu-id="74c66-175">Jsme učení rozhodovacího stromu modelu a modelu náhodných doménové struktury v této části a pak testování jejich přesnost své předpovědi.</span><span class="sxs-lookup"><span data-stu-id="74c66-175">We train a decision tree model and a random forest model in this section and then test their accuracy of their predictions.</span></span>

> [!NOTE]
> <span data-ttu-id="74c66-176">Použitý v následujícím kódu balíček rpart (rekurzivní vytváření oddílů a regresní stromy) je již nainstalován ve virtuálním počítači vědecké účely Data.</span><span class="sxs-lookup"><span data-stu-id="74c66-176">The rpart (Recursive Partitioning and Regression Trees) package used in the following code is already installed on the Data Science VM.</span></span>
>
>

<span data-ttu-id="74c66-177">První, můžeme rozdělit datové sady na školení a testovací sady:</span><span class="sxs-lookup"><span data-stu-id="74c66-177">First, let's split the dataset into training and test sets:</span></span>

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

<span data-ttu-id="74c66-178">A pak vytvořte rozhodovací strom klasifikovat e-maily.</span><span class="sxs-lookup"><span data-stu-id="74c66-178">And then create a decision tree to classify the emails.</span></span>

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

<span data-ttu-id="74c66-179">Tady je výsledek:</span><span class="sxs-lookup"><span data-stu-id="74c66-179">Here is the result:</span></span>

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

<span data-ttu-id="74c66-181">Pokud chcete zjistit, jak dobře provádí na trénovací sady, použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="74c66-181">To determine how well it performs on the training set, use the following code:</span></span>

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="74c66-182">Chcete-li zjistit, jak dobře provádí testovací sada:</span><span class="sxs-lookup"><span data-stu-id="74c66-182">To determine how well it performs on the test set:</span></span>

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="74c66-183">Zkuste taky umožňuje model náhodných doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="74c66-183">Let's also try a random forest model.</span></span> <span data-ttu-id="74c66-184">Náhodné doménovými strukturami cvičení velkého množství rozhodovací stromy a výstup třídu, která je režim klasifikace ze všech jednotlivých rozhodovací stromy.</span><span class="sxs-lookup"><span data-stu-id="74c66-184">Random forests train a multitude of decision trees and output a class that is the mode of the classifications from all of the individual decision trees.</span></span> <span data-ttu-id="74c66-185">Poskytují účinnější strojového učení a přístup podle jejich odstraňte tendence, že se stromu modelu rozhodnutí overfit školení datové sady.</span><span class="sxs-lookup"><span data-stu-id="74c66-185">They provide a more powerful machine learning approach as they correct for the tendency of a decision tree model to overfit a training dataset.</span></span>

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a><span data-ttu-id="74c66-186">Model nasazení do Azure ML</span><span class="sxs-lookup"><span data-stu-id="74c66-186">Deploy a model to Azure ML</span></span>
<span data-ttu-id="74c66-187">[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) je Cloudová služba, která umožňuje snadno vytvářet a nasazovat řešení prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="74c66-187">[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) is a cloud service that makes it easy to build and deploy predictive analytics models.</span></span> <span data-ttu-id="74c66-188">Jeden z dobrý funkce AzureML je schopnost publikovat všechny funkce R jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="74c66-188">One of the nice features of AzureML is its ability to publish any R function as a web service.</span></span> <span data-ttu-id="74c66-189">Balíček AzureML R snadné nasazení provést přímo z relace naše R na DSVM.</span><span class="sxs-lookup"><span data-stu-id="74c66-189">The AzureML R package makes deployment easy to do right from our R session on the DSVM.</span></span>

<span data-ttu-id="74c66-190">Chcete-li nasadit kód rozhodovací strom z předchozí části, přihlaste se k Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="74c66-190">To deploy the decision tree code from the previous section, you need to sign in to Azure Machine Learning Studio.</span></span> <span data-ttu-id="74c66-191">Potřebujete ID vašeho pracovního prostoru a autorizační token k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="74c66-191">You need your workspace ID and an authorization token to sign in.</span></span> <span data-ttu-id="74c66-192">K vyhledání tyto hodnoty a inicializaci AzureML proměnné s nimi:</span><span class="sxs-lookup"><span data-stu-id="74c66-192">To find these values and initialize the AzureML variables with them:</span></span>

<span data-ttu-id="74c66-193">Vyberte **nastavení** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="74c66-193">Select **Settings** on the left-hand menu.</span></span> <span data-ttu-id="74c66-194">Poznámka: vaše **ID pracovního prostoru**.</span><span class="sxs-lookup"><span data-stu-id="74c66-194">Note your **WORKSPACE ID**.</span></span> <span data-ttu-id="74c66-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span><span class="sxs-lookup"><span data-stu-id="74c66-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span></span>

<span data-ttu-id="74c66-196">Vyberte **autorizace tokeny** z nabídky režijních nákladů a Poznámka vaše **primární autorizační Token**.![ 3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span><span class="sxs-lookup"><span data-stu-id="74c66-196">Select **Authorization Tokens** from the overhead menu and note your **Primary Authorization Token**.![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span></span>

<span data-ttu-id="74c66-197">Zatížení **AzureML** balíček a potom nastavit hodnoty proměnných pomocí svého ID token a pracovního prostoru v relaci prostředí R na DSVM:</span><span class="sxs-lookup"><span data-stu-id="74c66-197">Load the **AzureML** package and then set values of the variables with your token and workspace ID in your R session on the DSVM:</span></span>

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


<span data-ttu-id="74c66-198">Umožňuje zjednodušit model usnadnění této ukázce k implementaci.</span><span class="sxs-lookup"><span data-stu-id="74c66-198">Let's simplify the model to make this demonstration easier to implement.</span></span> <span data-ttu-id="74c66-199">Vyberte tří proměnných v rozhodovacím stromu nejbližší do kořenového adresáře a vytvořit novou větev pomocí právě těchto tří proměnných:</span><span class="sxs-lookup"><span data-stu-id="74c66-199">Pick the three variables in the decision tree closest to the root and build a new tree using just those three variables:</span></span>

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

<span data-ttu-id="74c66-200">Potřebujeme předpovědi funkci, která přebírá funkce jako vstup a vrátí předpovězené hodnoty:</span><span class="sxs-lookup"><span data-stu-id="74c66-200">We need a prediction function that takes the features as an input and returns the predicted values:</span></span>

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

<span data-ttu-id="74c66-201">Publikování funkce predictSpam AzureML pomocí **publishWebService** funkce:</span><span class="sxs-lookup"><span data-stu-id="74c66-201">Publish the predictSpam function to AzureML using the **publishWebService** function:</span></span>

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

<span data-ttu-id="74c66-202">Tato funkce přebírá **predictSpam** fungovat, vytvoří webové služby s názvem **spamWebService** s definované vstupy a výstupy a vrátí informace o nový koncový bod.</span><span class="sxs-lookup"><span data-stu-id="74c66-202">This function takes the **predictSpam** function, creates a web service named **spamWebService** with defined inputs and outputs, and returns information about the new endpoint.</span></span>

<span data-ttu-id="74c66-203">Zobrazit podrobnosti o publikované webové služby, včetně jeho koncový bod rozhraní API a přístupové klíče pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="74c66-203">View details of the published web service, including its API endpoint and access keys with the command:</span></span>

    spamWebService[[2]]

<span data-ttu-id="74c66-204">Vyzkoušejte si to na prvním nastavte 10 řádků testu:</span><span class="sxs-lookup"><span data-stu-id="74c66-204">To try it out on the first 10 rows of the test set:</span></span>

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a><span data-ttu-id="74c66-205">Použít jiné nástroje, které jsou k dispozici</span><span class="sxs-lookup"><span data-stu-id="74c66-205">Use other tools available</span></span>
<span data-ttu-id="74c66-206">Zbývající části ukazují, jak používat některé z nástroje nainstalované v virtuálního počítače s Linuxem dat vědecké účely. Tady je seznam nástrojů, které jsou popsané:</span><span class="sxs-lookup"><span data-stu-id="74c66-206">The remaining sections show how to use some of the tools installed on the Linux Data Science VM.Here is the list of tools discussed:</span></span>

* <span data-ttu-id="74c66-207">XGBoost</span><span class="sxs-lookup"><span data-stu-id="74c66-207">XGBoost</span></span>
* <span data-ttu-id="74c66-208">Python</span><span class="sxs-lookup"><span data-stu-id="74c66-208">Python</span></span>
* <span data-ttu-id="74c66-209">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="74c66-209">Jupyterhub</span></span>
* <span data-ttu-id="74c66-210">Rattle</span><span class="sxs-lookup"><span data-stu-id="74c66-210">Rattle</span></span>
* <span data-ttu-id="74c66-211">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="74c66-211">PostgreSQL & Squirrel SQL</span></span>
* <span data-ttu-id="74c66-212">SQL Server datového skladu</span><span class="sxs-lookup"><span data-stu-id="74c66-212">SQL Server Data Warehouse</span></span>

## <a name="xgboost"></a><span data-ttu-id="74c66-213">XGBoost</span><span class="sxs-lookup"><span data-stu-id="74c66-213">XGBoost</span></span>
<span data-ttu-id="74c66-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) je nástroj, který poskytuje rychlé a přesné boosted stromu implementaci.</span><span class="sxs-lookup"><span data-stu-id="74c66-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) is a tool that provides a fast and accurate boosted tree implementation.</span></span>

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

<span data-ttu-id="74c66-215">XGBoost můžete také volat z pythonu nebo příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="74c66-215">XGBoost can also call from python or a command line.</span></span>

## <a name="python"></a><span data-ttu-id="74c66-216">Python</span><span class="sxs-lookup"><span data-stu-id="74c66-216">Python</span></span>
<span data-ttu-id="74c66-217">Pro vývoj pomocí Python se nainstalovaly v DSVM distribuce Anaconda Python 2.7 a 3.5.</span><span class="sxs-lookup"><span data-stu-id="74c66-217">For development using Python, the Anaconda Python distributions 2.7 and 3.5 have been installed in the DSVM.</span></span>

> [!NOTE]
> <span data-ttu-id="74c66-218">Zahrnuje distribuční Anaconda [Condas](http://conda.pydata.org/docs/index.html), který slouží k vytvoření vlastního prostředí pro jazyk Python, které mají různé verze nebo balíčky nainstalované v nich.</span><span class="sxs-lookup"><span data-stu-id="74c66-218">The Anaconda distribution includes [Condas](http://conda.pydata.org/docs/index.html), which can be used to create custom environments for Python that have different versions and/or packages installed in them.</span></span>
>
>

<span data-ttu-id="74c66-219">Umožňuje číst v některých spambase datové sady a klasifikovat e-maily s support vector počítačů v scikit-Další informace:</span><span class="sxs-lookup"><span data-stu-id="74c66-219">Let's read in some of the spambase dataset and classify the emails with support vector machines in scikit-learn:</span></span>

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="74c66-220">Chcete-li předpovědi:</span><span class="sxs-lookup"><span data-stu-id="74c66-220">To make predictions:</span></span>

    clf.predict(X.ix[0:20, :])

<span data-ttu-id="74c66-221">Pokud chcete zobrazit, jak publikovat koncového bodu AzureML, provedeme jednodušší model tří proměnných jako jsme to udělali při jsme dříve publikované R modelu.</span><span class="sxs-lookup"><span data-stu-id="74c66-221">To show how to publish an AzureML endpoint, let's make a simpler model the three variables as we did when we published the R model previously.</span></span>

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="74c66-222">Publikování modelu AzureML:</span><span class="sxs-lookup"><span data-stu-id="74c66-222">To publish the model to AzureML:</span></span>

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> <span data-ttu-id="74c66-223">To je dostupná jenom pro python 2.7 a není dosud nepodporováno u 3.5.</span><span class="sxs-lookup"><span data-stu-id="74c66-223">This is only available for python 2.7 and is not yet supported on 3.5.</span></span> <span data-ttu-id="74c66-224">Spustit s **/anaconda/bin/python2.7**.</span><span class="sxs-lookup"><span data-stu-id="74c66-224">Run with **/anaconda/bin/python2.7**.</span></span>
>
>

## <a name="jupyterhub"></a><span data-ttu-id="74c66-225">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="74c66-225">Jupyterhub</span></span>
<span data-ttu-id="74c66-226">Anaconda distribuce v DSVM se dodává s poznámkového bloku Jupyter, prostředí napříč platformami sdílet kód Python, R nebo Dita a analýzy.</span><span class="sxs-lookup"><span data-stu-id="74c66-226">The Anaconda distribution in the DSVM comes with a Jupyter notebook, a cross-platform environment to share Python, R, or Julia code and analysis.</span></span> <span data-ttu-id="74c66-227">Poznámkového bloku Jupyter přistupuje prostřednictvím JupyterHub.</span><span class="sxs-lookup"><span data-stu-id="74c66-227">The Jupyter notebook is accessed through JupyterHub.</span></span> <span data-ttu-id="74c66-228">Přihlášení pomocí místních Linux uživatelské jméno a heslo v ***https://\<název DNS virtuálního počítače nebo IP adresu\>: 8000 /***.</span><span class="sxs-lookup"><span data-stu-id="74c66-228">You sign in using your local Linux user name and password at ***https://\<VM DNS name or IP Address\>:8000/***.</span></span> <span data-ttu-id="74c66-229">Všechny konfigurační soubory pro JupyterHub se nacházejí v adresáři **/etc/jupyterhub**.</span><span class="sxs-lookup"><span data-stu-id="74c66-229">All configuration files for JupyterHub are found in directory **/etc/jupyterhub**.</span></span>

<span data-ttu-id="74c66-230">Několik ukázkových poznámkových bloků jsou již nainstalovány ve virtuálním počítači:</span><span class="sxs-lookup"><span data-stu-id="74c66-230">Several sample notebooks are already installed on the VM:</span></span>

* <span data-ttu-id="74c66-231">Najdete v článku [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) pro ukázkové Python Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="74c66-231">See the [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) for a sample Python notebook.</span></span>
* <span data-ttu-id="74c66-232">V tématu [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) pro ukázku **R** poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="74c66-232">See [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) for a sample **R** notebook.</span></span>
* <span data-ttu-id="74c66-233">Najdete v článku [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) pro jiné ukázku **Python** poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="74c66-233">See the [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) for another sample **Python** notebook.</span></span>

> [!NOTE]
> <span data-ttu-id="74c66-234">Dále je dostupný z příkazového řádku pro virtuální počítač Linux datové vědy Dita jazyk.</span><span class="sxs-lookup"><span data-stu-id="74c66-234">The Julia language is also available from the command line on the Linux Data Science VM.</span></span>
>
>

## <a name="rattle"></a><span data-ttu-id="74c66-235">Rattle</span><span class="sxs-lookup"><span data-stu-id="74c66-235">Rattle</span></span>
<span data-ttu-id="74c66-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) grafický nástroj R pro dolování dat (R Analytical nástroj pro další snadno) je.</span><span class="sxs-lookup"><span data-stu-id="74c66-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (the R Analytical Tool To Learn Easily) is a graphical R tool for data mining.</span></span> <span data-ttu-id="74c66-237">Obsahuje intuitivní rozhraní, které umožňuje snadno načíst, prozkoumat a transformovat data a vytvářet a vyhodnocení modelů.</span><span class="sxs-lookup"><span data-stu-id="74c66-237">It has an intuitive interface that makes it easy to load, explore, and transform data and build and evaluate models.</span></span>  <span data-ttu-id="74c66-238">Článek [Rattle: A Data Mining grafického uživatelského rozhraní pro R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) poskytuje návod, který ukazuje její funkce.</span><span class="sxs-lookup"><span data-stu-id="74c66-238">The article [Rattle: A Data Mining GUI for R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) provides a walkthrough that demonstrates its features.</span></span>

<span data-ttu-id="74c66-239">Instalace a spuštění Rattle pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="74c66-239">Install and start Rattle with the following commands:</span></span>

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> <span data-ttu-id="74c66-240">Na DSVM není nutné instalovat.</span><span class="sxs-lookup"><span data-stu-id="74c66-240">Installation is not required on the DSVM.</span></span> <span data-ttu-id="74c66-241">Ale Rattle můžete být vyzváni k instalaci dalších balíčků, jakmile ho načte.</span><span class="sxs-lookup"><span data-stu-id="74c66-241">But Rattle may prompt you to install additional packages when it loads.</span></span>
>
>

<span data-ttu-id="74c66-242">Rattle používá rozhraní založené na kartě.</span><span class="sxs-lookup"><span data-stu-id="74c66-242">Rattle uses a tab-based interface.</span></span> <span data-ttu-id="74c66-243">Většina karty odpovídají kroky v [proces vědecké účely dat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), jako jsou načítání dat nebo zkoumat ho.</span><span class="sxs-lookup"><span data-stu-id="74c66-243">Most of the tabs correspond to steps in the [Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), like loading data or exploring it.</span></span> <span data-ttu-id="74c66-244">Proces vědecké účely dat toky zleva doprava mezi kartami.</span><span class="sxs-lookup"><span data-stu-id="74c66-244">The data science process flows from left to right through the tabs.</span></span> <span data-ttu-id="74c66-245">Ale poslední karta obsahuje protokolu R příkazy spustit Rattle.</span><span class="sxs-lookup"><span data-stu-id="74c66-245">But the last tab contains a log of the R commands run by Rattle.</span></span>

<span data-ttu-id="74c66-246">Načíst a nakonfigurovat datové sady:</span><span class="sxs-lookup"><span data-stu-id="74c66-246">To load and configure the dataset:</span></span>

* <span data-ttu-id="74c66-247">Chcete-li načíst soubor, vyberte **Data** kartě, pak</span><span class="sxs-lookup"><span data-stu-id="74c66-247">To load the file, select the **Data** tab, then</span></span>
* <span data-ttu-id="74c66-248">Zvolte modulu pro výběr vedle **Filename** a zvolte **spambaseHeaders.data**.</span><span class="sxs-lookup"><span data-stu-id="74c66-248">Choose the selector next to **Filename** and choose **spambaseHeaders.data**.</span></span>
* <span data-ttu-id="74c66-249">Načíst soubor.</span><span class="sxs-lookup"><span data-stu-id="74c66-249">To load the file.</span></span> <span data-ttu-id="74c66-250">Vyberte **Execute** horní řádek tlačítek.</span><span class="sxs-lookup"><span data-stu-id="74c66-250">select **Execute** in the top row of buttons.</span></span> <span data-ttu-id="74c66-251">Měli byste vidět souhrn jednotlivých sloupců, včetně jeho identifikovaných datového typu, ať už se jedná o vstup, cíl nebo jiný typ proměnné a počet jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="74c66-251">You should see a summary of each column, including its identified data type, whether it's an input, a target, or other type of variable, and the number of unique values.</span></span>
* <span data-ttu-id="74c66-252">Rattle má identifikovány správně **nevyžádané pošty** sloupec jako cíl.</span><span class="sxs-lookup"><span data-stu-id="74c66-252">Rattle has correctly identified the **spam** column as the target.</span></span> <span data-ttu-id="74c66-253">Vyberte sloupec nevyžádané pošty, pak nastavte **cílový datový typ** k **Categoric**.</span><span class="sxs-lookup"><span data-stu-id="74c66-253">Select the spam column, then set the **Target Data Type** to **Categoric**.</span></span>

<span data-ttu-id="74c66-254">K prozkoumání dat:</span><span class="sxs-lookup"><span data-stu-id="74c66-254">To explore the data:</span></span>

* <span data-ttu-id="74c66-255">Vyberte **prozkoumat** kartě.</span><span class="sxs-lookup"><span data-stu-id="74c66-255">Select the **Explore** tab.</span></span>
* <span data-ttu-id="74c66-256">Klikněte na tlačítko **souhrnné**, pak **Execute**, určité informace o typy proměnných a některé souhrnné statistiky.</span><span class="sxs-lookup"><span data-stu-id="74c66-256">Click **Summary**, then **Execute**, to see some information about the variable types and some summary statistics.</span></span>
* <span data-ttu-id="74c66-257">Chcete-li zobrazit jiné typy Statistika každou proměnnou, vyberte jiné možnosti jako **popisujících** nebo **Základy**.</span><span class="sxs-lookup"><span data-stu-id="74c66-257">To view other types of statistics about each variable, select other options like **Describe** or **Basics**.</span></span>

<span data-ttu-id="74c66-258">**Prozkoumat** kartě můžete také vygenerovat mnoho pronikavého pozemků.</span><span class="sxs-lookup"><span data-stu-id="74c66-258">The **Explore** tab also allows you to generate many insightful plots.</span></span> <span data-ttu-id="74c66-259">K vykreslení histogram dat:</span><span class="sxs-lookup"><span data-stu-id="74c66-259">To plot a histogram of the data:</span></span>

* <span data-ttu-id="74c66-260">Vyberte **distribuce**.</span><span class="sxs-lookup"><span data-stu-id="74c66-260">Select **Distributions**.</span></span>
* <span data-ttu-id="74c66-261">Zkontrolujte **Histogram** pro **word_freq_remove** a **word_freq_you**.</span><span class="sxs-lookup"><span data-stu-id="74c66-261">Check **Histogram** for **word_freq_remove** and **word_freq_you**.</span></span>
* <span data-ttu-id="74c66-262">Vyberte **provést**.</span><span class="sxs-lookup"><span data-stu-id="74c66-262">Select **Execute**.</span></span> <span data-ttu-id="74c66-263">Měli byste vidět obou hustotu pozemků v okně jednoho grafu, pokud je zřejmé, že je slovo "vy" zobrazí mnohem častěji v e-mailů než "Odebrat".</span><span class="sxs-lookup"><span data-stu-id="74c66-263">You should see both density plots in a single graph window, where it is clear that the word "you" appears much more frequently in emails than "remove".</span></span>

<span data-ttu-id="74c66-264">Korelace pozemků jsou také zajímavé.</span><span class="sxs-lookup"><span data-stu-id="74c66-264">The Correlation plots are also interesting.</span></span> <span data-ttu-id="74c66-265">Chcete vytvořit:</span><span class="sxs-lookup"><span data-stu-id="74c66-265">To create one:</span></span>

* <span data-ttu-id="74c66-266">Zvolte **korelace** jako **typu**, pak</span><span class="sxs-lookup"><span data-stu-id="74c66-266">Choose **Correlation** as the **Type**, then</span></span>
* <span data-ttu-id="74c66-267">Vyberte **provést**.</span><span class="sxs-lookup"><span data-stu-id="74c66-267">Select **Execute**.</span></span>
* <span data-ttu-id="74c66-268">Rattle vás varuje, že doporučí maximálně 40 proměnné.</span><span class="sxs-lookup"><span data-stu-id="74c66-268">Rattle warns you that it recommends a maximum of 40 variables.</span></span> <span data-ttu-id="74c66-269">Vyberte **Ano** zobrazíte vykreslení.</span><span class="sxs-lookup"><span data-stu-id="74c66-269">Select **Yes** to view the plot.</span></span>

<span data-ttu-id="74c66-270">Existují některé zajímavé korelací, které se spustit: "technologie" je důrazně korelační "HP" a "labs", například.</span><span class="sxs-lookup"><span data-stu-id="74c66-270">There are some interesting correlations that come up: "technology" is strongly correlated to "HP" and "labs", for example.</span></span> <span data-ttu-id="74c66-271">Ho je také důrazně vztažen k "650", protože je 650 kód oblasti dárců datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="74c66-271">It is also strongly correlated to "650", because the area code of the dataset donors is 650.</span></span>

<span data-ttu-id="74c66-272">V okně Explore jsou k dispozici číselné hodnoty korelací mezi slovy.</span><span class="sxs-lookup"><span data-stu-id="74c66-272">The numeric values for the correlations between words are available in the Explore window.</span></span> <span data-ttu-id="74c66-273">Je zajímavé Poznámka, například, že je "technologie" negativní korelační s "vaše" a "peníze".</span><span class="sxs-lookup"><span data-stu-id="74c66-273">It is interesting to note, for example, that "technology" is negatively correlated with "your" and "money".</span></span>

<span data-ttu-id="74c66-274">Rattle můžete převést datovou sadu, která zpracovává některé běžné problémy.</span><span class="sxs-lookup"><span data-stu-id="74c66-274">Rattle can transform the dataset to handle some common issues.</span></span> <span data-ttu-id="74c66-275">Například umožňuje nastavit velikost funkce, dává chybějící hodnoty, zpracování extrémních a odebrání proměnné nebo připomínky s chybějící data.</span><span class="sxs-lookup"><span data-stu-id="74c66-275">For example, it allows you to rescale features, impute missing values, handle outliers, and remove variables or observations with missing data.</span></span> <span data-ttu-id="74c66-276">Rattle můžete také určit pravidla přidružení mezi připomínky nebo proměnné.</span><span class="sxs-lookup"><span data-stu-id="74c66-276">Rattle can also identify association rules between observations and/or variables.</span></span> <span data-ttu-id="74c66-277">Tyto karty jsou nad rámec této úvodní prohlídka.</span><span class="sxs-lookup"><span data-stu-id="74c66-277">These tabs are out of scope for this introductory walkthrough.</span></span>

<span data-ttu-id="74c66-278">Rattle můžete také provést analýzu clusteru.</span><span class="sxs-lookup"><span data-stu-id="74c66-278">Rattle can also perform cluster analysis.</span></span> <span data-ttu-id="74c66-279">Umožňuje vyloučit některé funkce usnadnění výstup.</span><span class="sxs-lookup"><span data-stu-id="74c66-279">Let's exclude some features to make the output easier to read.</span></span> <span data-ttu-id="74c66-280">Na **Data** , zvolte **Ignorovat** vedle každého proměnné s výjimkou těchto deset položek:</span><span class="sxs-lookup"><span data-stu-id="74c66-280">On the **Data** tab, choose **Ignore** next to each of the variables except these ten items:</span></span>

* <span data-ttu-id="74c66-281">word_freq_hp</span><span class="sxs-lookup"><span data-stu-id="74c66-281">word_freq_hp</span></span>
* <span data-ttu-id="74c66-282">word_freq_technology</span><span class="sxs-lookup"><span data-stu-id="74c66-282">word_freq_technology</span></span>
* <span data-ttu-id="74c66-283">word_freq_george</span><span class="sxs-lookup"><span data-stu-id="74c66-283">word_freq_george</span></span>
* <span data-ttu-id="74c66-284">word_freq_remove</span><span class="sxs-lookup"><span data-stu-id="74c66-284">word_freq_remove</span></span>
* <span data-ttu-id="74c66-285">word_freq_your</span><span class="sxs-lookup"><span data-stu-id="74c66-285">word_freq_your</span></span>
* <span data-ttu-id="74c66-286">word_freq_dollar</span><span class="sxs-lookup"><span data-stu-id="74c66-286">word_freq_dollar</span></span>
* <span data-ttu-id="74c66-287">word_freq_money</span><span class="sxs-lookup"><span data-stu-id="74c66-287">word_freq_money</span></span>
* <span data-ttu-id="74c66-288">capital_run_length_longest</span><span class="sxs-lookup"><span data-stu-id="74c66-288">capital_run_length_longest</span></span>
* <span data-ttu-id="74c66-289">word_freq_business</span><span class="sxs-lookup"><span data-stu-id="74c66-289">word_freq_business</span></span>
* <span data-ttu-id="74c66-290">nevyžádané pošty</span><span class="sxs-lookup"><span data-stu-id="74c66-290">spam</span></span>

<span data-ttu-id="74c66-291">Poté přejděte zpět na **clusteru** , zvolte **KMeans**a nastavte *počtu clusterů, které* na 4.</span><span class="sxs-lookup"><span data-stu-id="74c66-291">Then go back to the **Cluster** tab, choose **KMeans**, and set the *Number of clusters* to 4.</span></span> <span data-ttu-id="74c66-292">Potom **provést**.</span><span class="sxs-lookup"><span data-stu-id="74c66-292">Then **Execute**.</span></span> <span data-ttu-id="74c66-293">Výsledky jsou zobrazeny v okně výstupu.</span><span class="sxs-lookup"><span data-stu-id="74c66-293">The results are displayed in the output window.</span></span> <span data-ttu-id="74c66-294">Jeden cluster má vysoká frekvence "Jirka" a "hp" a je pravděpodobně legitimní firemní e-mail.</span><span class="sxs-lookup"><span data-stu-id="74c66-294">One cluster has high frequency of "george" and "hp" and is probably a legitimate business email.</span></span>

<span data-ttu-id="74c66-295">K sestavení modelu strojového učení stromu jednoduché rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="74c66-295">To build a simple decision tree machine learning model:</span></span>

* <span data-ttu-id="74c66-296">Vyberte **modelu** kartě</span><span class="sxs-lookup"><span data-stu-id="74c66-296">Select the **Model** tab,</span></span>
* <span data-ttu-id="74c66-297">Zvolte **stromu** jako **typu**.</span><span class="sxs-lookup"><span data-stu-id="74c66-297">Choose **Tree** as the **Type**.</span></span>
* <span data-ttu-id="74c66-298">Vyberte **Execute** a zobrazení stromu v textové podobě, v okně výstupu.</span><span class="sxs-lookup"><span data-stu-id="74c66-298">Select **Execute** to display the tree in text form in the output window.</span></span>
* <span data-ttu-id="74c66-299">Vyberte **kreslení** tlačítko Zobrazit grafické verze.</span><span class="sxs-lookup"><span data-stu-id="74c66-299">Select the **Draw** button to view a graphical version.</span></span> <span data-ttu-id="74c66-300">To vypadá podobný tomu stromu jsme získali dříve pomocí *rpart*.</span><span class="sxs-lookup"><span data-stu-id="74c66-300">This looks quite similar to the tree we obtained earlier using *rpart*.</span></span>

<span data-ttu-id="74c66-301">Jeden z dobrý funkce Rattle je schopnost spustit několik metod machine learning a rychle vyhodnocovat jejich výsledky.</span><span class="sxs-lookup"><span data-stu-id="74c66-301">One of the nice features of Rattle is its ability to run several machine learning methods and quickly evaluate them.</span></span> <span data-ttu-id="74c66-302">Tady je postup:</span><span class="sxs-lookup"><span data-stu-id="74c66-302">Here is the procedure:</span></span>

* <span data-ttu-id="74c66-303">Zvolte **všechny** pro **typu**.</span><span class="sxs-lookup"><span data-stu-id="74c66-303">Choose **All** for the **Type**.</span></span>
* <span data-ttu-id="74c66-304">Vyberte **provést**.</span><span class="sxs-lookup"><span data-stu-id="74c66-304">Select **Execute**.</span></span>
* <span data-ttu-id="74c66-305">Po jejím dokončení můžete kliknout na všechny jedním **typ**, například **SVM**a zobrazit výsledky.</span><span class="sxs-lookup"><span data-stu-id="74c66-305">After it finishes you can click any single **Type**, like **SVM**, and view the results.</span></span>
* <span data-ttu-id="74c66-306">Také můžete porovnat výkon modely na ověřování, nastavit pomocí **Evaluate** kartě. Například **chyba matice** výběr ukazuje matice nejasnostem, celkový chyb a chyba zprůměrovanou třídy pro každý model na sadu ověření.</span><span class="sxs-lookup"><span data-stu-id="74c66-306">You can also compare the performance of the models on the validation set using the **Evaluate** tab. For example, the **Error Matrix** selection shows you the confusion matrix, overall error, and averaged class error for each model on the validation set.</span></span>
* <span data-ttu-id="74c66-307">Můžete také vykreslení křivek ROC, provádět analýzy velkých a malých písmen a dělat jiné typy modelu hodnocení.</span><span class="sxs-lookup"><span data-stu-id="74c66-307">You can also plot ROC curves, perform sensitivity analysis, and do other types of model evaluations.</span></span>

<span data-ttu-id="74c66-308">Jakmile budete hotovi, vytváření modelů, vyberte **protokolu** zobrazíte kód R spustit Rattle během relace.</span><span class="sxs-lookup"><span data-stu-id="74c66-308">Once you're finished building models, select the **Log** tab to view the R code run by Rattle during your session.</span></span> <span data-ttu-id="74c66-309">Můžete vybrat **exportovat** tlačítko ji uložit.</span><span class="sxs-lookup"><span data-stu-id="74c66-309">You can select the **Export** button to save it.</span></span>

> [!NOTE]
> <span data-ttu-id="74c66-310">V aktuální verzi Rattle je chyba.</span><span class="sxs-lookup"><span data-stu-id="74c66-310">There is a bug in current release of Rattle.</span></span> <span data-ttu-id="74c66-311">Chcete-li změnit skriptu nebo ji použít k vaší kroky opakujte později, musíte vložit znak # před * exportovat tento protokol... * v textu protokolu.</span><span class="sxs-lookup"><span data-stu-id="74c66-311">To modify the script or use it to repeat your steps later, you must insert a # character in front of *Export this log ... * in the text of the log.</span></span>
>
>

## <a name="postgresql--squirrel-sql"></a><span data-ttu-id="74c66-312">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="74c66-312">PostgreSQL & Squirrel SQL</span></span>
<span data-ttu-id="74c66-313">DSVM se dodává s PostgreSQL nainstalována.</span><span class="sxs-lookup"><span data-stu-id="74c66-313">The DSVM comes with PostgreSQL installed.</span></span> <span data-ttu-id="74c66-314">PostgreSQL je relační databáze sofistikované, open source.</span><span class="sxs-lookup"><span data-stu-id="74c66-314">PostgreSQL is a sophisticated, open-source relational database.</span></span> <span data-ttu-id="74c66-315">V této části ukazuje, jak načíst naší datové sadě nevyžádané pošty do PostgreSQL a pak zadat dotaz.</span><span class="sxs-lookup"><span data-stu-id="74c66-315">This section shows how to load our spam dataset into PostgreSQL and then query it.</span></span>

<span data-ttu-id="74c66-316">Před načtením dat, budete muset povolit ověřování hesla z localhost.</span><span class="sxs-lookup"><span data-stu-id="74c66-316">Before you can load the data, you need to allow password authentication from the localhost.</span></span> <span data-ttu-id="74c66-317">Na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="74c66-317">At a command prompt:</span></span>

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

<span data-ttu-id="74c66-318">V dolní části do konfiguračního souboru jsou několik řádků, které podrobností povolených připojení:</span><span class="sxs-lookup"><span data-stu-id="74c66-318">Near the bottom of the config file are several lines that detail the allowed connections:</span></span>

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

<span data-ttu-id="74c66-319">Změňte řádek "IPv4 místní připojení" pro použití md5 místo ident, takže jsme umožní přihlásit se pomocí uživatelského jména a hesla:</span><span class="sxs-lookup"><span data-stu-id="74c66-319">Change the "IPv4 local connections" line to use md5 instead of ident, so we can log in using a username and password:</span></span>

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

<span data-ttu-id="74c66-320">A restartujte službu postgres:</span><span class="sxs-lookup"><span data-stu-id="74c66-320">And restart the postgres service:</span></span>

    sudo systemctl restart postgresql

<span data-ttu-id="74c66-321">Spusťte psql, interaktivní terminálu PostgreSQL jako uživatel předdefinované postgres, spusťte následující příkaz z řádku:</span><span class="sxs-lookup"><span data-stu-id="74c66-321">To launch psql, an interactive terminal for PostgreSQL, as the built-in postgres user, run the following command from a prompt:</span></span>

    sudo -u postgres psql

<span data-ttu-id="74c66-322">Vytvořte nový uživatelský účet, pomocí stejného uživatelského jména jako Linux účet aktuálně přihlášeni jako a poskytněte heslo:</span><span class="sxs-lookup"><span data-stu-id="74c66-322">Create a new user account, using the same username as the Linux account you're currently logged in as, and give it a password:</span></span>

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

<span data-ttu-id="74c66-323">Pak se přihlaste k psql jako vaše uživatele:</span><span class="sxs-lookup"><span data-stu-id="74c66-323">Then log in to psql as your user:</span></span>

    psql

<span data-ttu-id="74c66-324">A importovat data do nové databáze:</span><span class="sxs-lookup"><span data-stu-id="74c66-324">And import the data into a new database:</span></span>

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

<span data-ttu-id="74c66-325">Nyní Pojďme data prozkoumat a spustit některé dotazy pomocí **Squirrel SQL**, grafický nástroj, který umožňuje pracovat s databází prostřednictvím ovladač JDBC.</span><span class="sxs-lookup"><span data-stu-id="74c66-325">Now, let's explore the data and run some queries using **Squirrel SQL**, a graphical tool that lets you interact with databases via a JDBC driver.</span></span>

<span data-ttu-id="74c66-326">Abyste mohli začít, spusťte Squirrel SQL v nabídce aplikace.</span><span class="sxs-lookup"><span data-stu-id="74c66-326">To get started, launch Squirrel SQL from the Applications menu.</span></span> <span data-ttu-id="74c66-327">Nastavení ovladače:</span><span class="sxs-lookup"><span data-stu-id="74c66-327">To set up the driver:</span></span>

* <span data-ttu-id="74c66-328">Vyberte **Windows**, pak **zobrazit ovladače**.</span><span class="sxs-lookup"><span data-stu-id="74c66-328">Select **Windows**, then **View Drivers**.</span></span>
* <span data-ttu-id="74c66-329">Klikněte pravým tlačítkem na **PostgreSQL** a vyberte **upravit ovladač**.</span><span class="sxs-lookup"><span data-stu-id="74c66-329">Right-click on **PostgreSQL** and select **Modify Driver**.</span></span>
* <span data-ttu-id="74c66-330">Vyberte **velmi třídy cesta**, pak **přidat**.</span><span class="sxs-lookup"><span data-stu-id="74c66-330">Select **Extra Class Path**, then **Add**.</span></span>
* <span data-ttu-id="74c66-331">Zadejte ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** pro **název souboru** a</span><span class="sxs-lookup"><span data-stu-id="74c66-331">Enter ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** for the **File Name** and</span></span>
* <span data-ttu-id="74c66-332">Vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="74c66-332">Select **Open**.</span></span>
* <span data-ttu-id="74c66-333">Vyberte ovladače seznamu a pak vyberte **org.postgresql.Driver** v **název třídy**a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="74c66-333">Choose List Drivers, then select **org.postgresql.Driver** in **Class Name**, and select **OK**.</span></span>

<span data-ttu-id="74c66-334">Nastavení připojení k místnímu serveru:</span><span class="sxs-lookup"><span data-stu-id="74c66-334">To set up the connection to the local server:</span></span>

* <span data-ttu-id="74c66-335">Vyberte **Windows**, pak **zobrazit aliasy.**</span><span class="sxs-lookup"><span data-stu-id="74c66-335">Select **Windows**, then **View Aliases.**</span></span>
* <span data-ttu-id="74c66-336">Vyberte  **+**  tlačítko Vytvořit nový alias.</span><span class="sxs-lookup"><span data-stu-id="74c66-336">Choose the **+** button to make a new alias.</span></span>
* <span data-ttu-id="74c66-337">Pojmenujte ji *nevyžádané pošty databáze*, zvolte **PostgreSQL** v **ovladač** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="74c66-337">Name it *Spam database*, choose **PostgreSQL** in the **Driver** drop-down.</span></span>
* <span data-ttu-id="74c66-338">Nastavení adresy URL *jdbc:postgresql://localhost/spam*.</span><span class="sxs-lookup"><span data-stu-id="74c66-338">Set the URL to *jdbc:postgresql://localhost/spam*.</span></span>
* <span data-ttu-id="74c66-339">Zadejte vaše *uživatelské jméno* a *heslo*.</span><span class="sxs-lookup"><span data-stu-id="74c66-339">Enter your *username* and *password*.</span></span>
* <span data-ttu-id="74c66-340">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="74c66-340">Click **OK**.</span></span>
* <span data-ttu-id="74c66-341">Otevřete **připojení** okno, dvakrát klikněte ***nevyžádané pošty databáze*** alias.</span><span class="sxs-lookup"><span data-stu-id="74c66-341">To open the **Connection** window, double-click the ***Spam database*** alias.</span></span>
* <span data-ttu-id="74c66-342">Vyberte **Connect** (Připojit).</span><span class="sxs-lookup"><span data-stu-id="74c66-342">Select **Connect**.</span></span>

<span data-ttu-id="74c66-343">Spuštění některých dotazů:</span><span class="sxs-lookup"><span data-stu-id="74c66-343">To run some queries:</span></span>

* <span data-ttu-id="74c66-344">Vyberte **SQL** kartě.</span><span class="sxs-lookup"><span data-stu-id="74c66-344">Select the **SQL** tab.</span></span>
* <span data-ttu-id="74c66-345">Například zadejte jednoduchý dotaz `SELECT * from data;` do textového pole dotazu v horní části na kartě SQL.</span><span class="sxs-lookup"><span data-stu-id="74c66-345">Enter a simple query such as `SELECT * from data;` in the query textbox at the top of the SQL tab.</span></span>
* <span data-ttu-id="74c66-346">Stiskněte klávesu **zadejte Ctrl** ji spustit.</span><span class="sxs-lookup"><span data-stu-id="74c66-346">Press **Ctrl-Enter** to run it.</span></span> <span data-ttu-id="74c66-347">Ve výchozím nastavení Squirrel SQL vrátí prvních 100 řádků z dotazu.</span><span class="sxs-lookup"><span data-stu-id="74c66-347">By default Squirrel SQL returns the first 100 rows from your query.</span></span>

<span data-ttu-id="74c66-348">Nejsou k dispozici mnoho další dotazy, že můžete spustit a prozkoumejte tato data.</span><span class="sxs-lookup"><span data-stu-id="74c66-348">There are many more queries you could run to explore this data.</span></span> <span data-ttu-id="74c66-349">Například jak funguje frekvence aplikace word *zkontrolujte* liší nevyžádané pošty se šunkou?</span><span class="sxs-lookup"><span data-stu-id="74c66-349">For example, how does the frequency of the word *make* differ between spam and ham?</span></span>

    SELECT avg(word_freq_make), spam from data group by spam;

<span data-ttu-id="74c66-350">Nebo jaké jsou vlastnosti e-mailů, které často obsahují *3d*?</span><span class="sxs-lookup"><span data-stu-id="74c66-350">Or what are the characteristics of email that frequently contain *3d*?</span></span>

    SELECT * from data order by word_freq_3d desc;

<span data-ttu-id="74c66-351">Většina e-mailů, které mají vysokou výskytem *3d* jsou zjevně nevyžádané pošty, takže může být užitečná pro vytvoření prediktivního modelu klasifikovat e-maily.</span><span class="sxs-lookup"><span data-stu-id="74c66-351">Most emails that have a high occurrence of *3d* are apparently spam, so it could be a useful feature for building a predictive model to classify the emails.</span></span>

<span data-ttu-id="74c66-352">Pokud jste chtěli provést machine learning s daty uloženými v databázi PostgreSQL, zvažte použití [MADlib](http://madlib.incubator.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="74c66-352">If you wanted to perform machine learning with data stored in a PostgreSQL database, consider using [MADlib](http://madlib.incubator.apache.org/).</span></span>

## <a name="sql-server-data-warehouse"></a><span data-ttu-id="74c66-353">SQL Server datového skladu</span><span class="sxs-lookup"><span data-stu-id="74c66-353">SQL Server Data Warehouse</span></span>
<span data-ttu-id="74c66-354">Azure SQL Data Warehouse je cloudová, škálovatelná databáze, která dokáže zpracovávat ohromné objemy dat, relačních i nerelačních.</span><span class="sxs-lookup"><span data-stu-id="74c66-354">Azure SQL Data Warehouse is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span> <span data-ttu-id="74c66-355">Další informace najdete v tématu [co je Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="74c66-355">For more information, see [What is Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span></span>

<span data-ttu-id="74c66-356">K připojení k datovému skladu a vytvořit tabulku, spusťte z příkazového řádku následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74c66-356">To connect to the data warehouse and create the table, run the following command from a command prompt:</span></span>

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

<span data-ttu-id="74c66-357">Pak do příkazového řádku sqlcmd:</span><span class="sxs-lookup"><span data-stu-id="74c66-357">Then at the sqlcmd prompt:</span></span>

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

<span data-ttu-id="74c66-358">Kopírování dat pomocí bcp:</span><span class="sxs-lookup"><span data-stu-id="74c66-358">Copy data with bcp:</span></span>

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> <span data-ttu-id="74c66-359">Konce řádků ve staženém souboru jsou styl systému Windows, ale bcp očekává stylu systému UNIX, takže potřebujeme říct bcp, s příznakem - r.</span><span class="sxs-lookup"><span data-stu-id="74c66-359">The line endings in the downloaded file are Windows-style, but bcp expects UNIX-style, so we need to tell bcp that with the -r flag.</span></span>
>
>

<span data-ttu-id="74c66-360">A dotazování pomocí sqlcmd:</span><span class="sxs-lookup"><span data-stu-id="74c66-360">And query with sqlcmd:</span></span>

    select top 10 spam, char_freq_dollar from spam;
    GO

<span data-ttu-id="74c66-361">Také můžete dotazovat pomocí Squirrel SQL.</span><span class="sxs-lookup"><span data-stu-id="74c66-361">You could also query with Squirrel SQL.</span></span> <span data-ttu-id="74c66-362">Podobný postup PostgreSQL, ovladač JDBC Microsoft MSSQL serveru, pomocí kterého lze nalézt v ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span><span class="sxs-lookup"><span data-stu-id="74c66-362">Follow similar steps for PostgreSQL, using the Microsoft MSSQL Server JDBC Driver, which can be found in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74c66-363">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74c66-363">Next steps</span></span>
<span data-ttu-id="74c66-364">Přehled témata, která vás provede procesem úlohy, které tvoří proces vědecké zpracování dat v Azure najdete v tématu [proces vědecké účely dat Team](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="74c66-364">For an overview of topics that walk you through the tasks that comprise the Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="74c66-365">Popis dalších návody začátku do konce, které ukazují kroků v procesu vědecké účely Team dat u konkrétních scénářů najdete v tématu [proces vědecké účely dat Team návody](data-science-process-walkthroughs.md).</span><span class="sxs-lookup"><span data-stu-id="74c66-365">For a description of other end-to-end walkthroughs that demonstrate the steps in the Team Data Science Process for specific scenarios, see [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md).</span></span> <span data-ttu-id="74c66-366">Názorné postupy také ukazují, jak kombinovat cloudové a místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace.</span><span class="sxs-lookup"><span data-stu-id="74c66-366">The walkthroughs also illustrate how to combine cloud and on-premises tools and services into a workflow or pipeline to create an intelligent application.</span></span>
