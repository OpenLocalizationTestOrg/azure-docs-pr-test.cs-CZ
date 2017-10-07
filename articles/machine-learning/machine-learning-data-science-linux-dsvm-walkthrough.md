---
title: "vědecké účely aaaData na hello Linux datové vědy virtuálního počítače | Microsoft Docs"
description: "Jak tooperform několik běžných vědecké zpracování dat úlohy s hello virtuálního počítače s Linuxem dat vědecké účely."
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
ms.openlocfilehash: 78764825f2e834fa4ddb7fdc2f59418dbe736e1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-on-hello-linux-data-science-virtual-machine"></a><span data-ttu-id="69034-103">Vědecké zpracování dat na hello Linux datové vědy virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="69034-103">Data science on hello Linux Data Science Virtual Machine</span></span>
<span data-ttu-id="69034-104">Tento návod ukazuje, jak tooperform několik běžných vědecké zpracování dat úlohy s hello virtuálního počítače s Linuxem dat vědecké účely.</span><span class="sxs-lookup"><span data-stu-id="69034-104">This walkthrough shows you how tooperform several common data science tasks with hello Linux Data Science VM.</span></span> <span data-ttu-id="69034-105">Hello Linux datové vědy virtuálního počítače (DSVM) je bitová kopie virtuálního počítače, která je k dispozici v Azure, který je předem nainstalovaná s kolekcí nástrojů pro běžně používané k analýze dat a strojové učení.</span><span class="sxs-lookup"><span data-stu-id="69034-105">hello Linux Data Science Virtual Machine (DSVM) is a virtual machine image available on Azure that is pre-installed with a collection of tools commonly used for data analytics and machine learning.</span></span> <span data-ttu-id="69034-106">Hello klíčové softwarové součásti je uvedeno v hello [zřídit hello Linux datové vědy virtuálního počítače](machine-learning-data-science-linux-dsvm-intro.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="69034-106">hello key software components are itemized in hello [Provision hello Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md) topic.</span></span> <span data-ttu-id="69034-107">Hello image virtuálního počítače umožňuje snadno tooget spustit provádění vědecké zpracování dat v minutách, bez nutnosti tooinstall a každý z nástrojů hello nakonfigurovat jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="69034-107">hello VM image makes it easy tooget started doing data science in minutes, without having tooinstall and configure each of hello tools individually.</span></span> <span data-ttu-id="69034-108">Můžete snadno škálovat hello virtuálních počítačů, v případě potřeby a zastavte ji když není používán.</span><span class="sxs-lookup"><span data-stu-id="69034-108">You can easily scale up hello VM, if needed, and stop it when not in use.</span></span> <span data-ttu-id="69034-109">Proto tento prostředek je elastické a nákladově efektivní.</span><span class="sxs-lookup"><span data-stu-id="69034-109">So this resource is both elastic and cost-efficient.</span></span>

<span data-ttu-id="69034-110">Hello ukázáno v tomto názorném postupu úloh vědecké účely dat postupujte podle hello kroků uvedených v hello [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span><span class="sxs-lookup"><span data-stu-id="69034-110">hello data science tasks demonstrated in this walkthrough follow hello steps outlined in hello [Team Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).</span></span> <span data-ttu-id="69034-111">Tento proces zajišťuje toodata systematicky vědecké účely, které týmům datových vědců tooeffectively spolupracovat přes životního cyklu hello vytváření inteligentní aplikace.</span><span class="sxs-lookup"><span data-stu-id="69034-111">This process provides a systematic approach toodata science that enables teams of data scientists tooeffectively collaborate over hello lifecycle of building intelligent applications.</span></span> <span data-ttu-id="69034-112">proces vědecké účely dat Hello také poskytuje iterativní framework vědecké zpracování dat, který může následovat jednotlivce.</span><span class="sxs-lookup"><span data-stu-id="69034-112">hello data science process also provides an iterative framework for data science that can be followed by an individual.</span></span>

<span data-ttu-id="69034-113">Budeme analyzovat hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) datovou sadu v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="69034-113">We analyze hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset in this walkthrough.</span></span> <span data-ttu-id="69034-114">Je to sada e-mailů, které jsou označeny jako nevyžádané pošty nebo šunkou (tj. nejsou nevyžádané pošty), a také obsahuje statistikami o hello obsah e-mailů hello.</span><span class="sxs-lookup"><span data-stu-id="69034-114">This is a set of emails that are marked as either spam or ham (meaning they are not spam), and also contains some statistics on hello content of hello emails.</span></span> <span data-ttu-id="69034-115">statistiky Hello zahrnuté jsou popsané v hello vedle ale jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="69034-115">hello statistics included are discussed in hello next but one section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69034-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="69034-116">Prerequisites</span></span>
<span data-ttu-id="69034-117">Než budete moct použít virtuální počítač s Linuxem dat vědecké účely, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="69034-117">Before you can use a Linux Data Science Virtual Machine, you must have hello following:</span></span>

* <span data-ttu-id="69034-118">**Předplatné**.</span><span class="sxs-lookup"><span data-stu-id="69034-118">An **Azure subscription**.</span></span> <span data-ttu-id="69034-119">Pokud není již nemáte, přečtěte si téma [vytvořit účet Azure zdarma Dnes](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="69034-119">If you do not already have one, see [Create your free Azure account today](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="69034-120">A [ **Linux vědecké zpracování dat virtuálních počítačů**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="69034-120">A [**Linux data science VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm).</span></span> <span data-ttu-id="69034-121">Informace o zřizování tohoto virtuálního počítače najdete v tématu [zřídit hello Linux datové vědy virtuální počítač](machine-learning-data-science-linux-dsvm-intro.md).</span><span class="sxs-lookup"><span data-stu-id="69034-121">For information on provisioning this VM, see [Provision hello Linux Data Science Virtual Machine](machine-learning-data-science-linux-dsvm-intro.md).</span></span>
* <span data-ttu-id="69034-122">[X2Go](http://wiki.x2go.org/doku.php) v počítači nainstalován a otevřít relaci XFCE.</span><span class="sxs-lookup"><span data-stu-id="69034-122">[X2Go](http://wiki.x2go.org/doku.php) installed on your computer and opened an XFCE session.</span></span> <span data-ttu-id="69034-123">Informace o instalaci a konfiguraci **X2Go klienta**, najdete v části [instalace a konfigurace klienta X2Go](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span><span class="sxs-lookup"><span data-stu-id="69034-123">For information on installing and configuring an **X2Go client**, see [Installing and configuring X2Go client](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).</span></span> 
* <span data-ttu-id="69034-124">**AzureML účet**.</span><span class="sxs-lookup"><span data-stu-id="69034-124">An **AzureML account**.</span></span> <span data-ttu-id="69034-125">Pokud jste ještě nemáte, zaregistrujte si novým v hello [domovské stránky AzureML](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="69034-125">If you don't already have one, sign up for new one at hello [AzureML homepage](https://studio.azureml.net/).</span></span> <span data-ttu-id="69034-126">Není toohelp vrstvy volné využití, které začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="69034-126">There is a free usage tier toohelp you get started.</span></span>

## <a name="download-hello-spambase-dataset"></a><span data-ttu-id="69034-127">Stáhnout hello spambase datové sady</span><span class="sxs-lookup"><span data-stu-id="69034-127">Download hello spambase dataset</span></span>
<span data-ttu-id="69034-128">Hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) datové sady je poměrně malý sada dat, která obsahuje jenom 4601 příklady.</span><span class="sxs-lookup"><span data-stu-id="69034-128">hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset is a relatively small set of data that contains only 4601 examples.</span></span> <span data-ttu-id="69034-129">To je vhodné velikost toouse při demonstraci toho, že některé klíčové funkce hello hello vědecké účely Data virtuálního počítače jako jeho uchovává požadavky na prostředky hello mírné.</span><span class="sxs-lookup"><span data-stu-id="69034-129">This is a convenient size toouse when demonstrating that some of hello key features of hello Data Science VM as it keeps hello resource requirements modest.</span></span>

> [!NOTE]
> <span data-ttu-id="69034-130">Tento návod byl vytvořen na D2 v2 velikost datové vědy virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="69034-130">This walkthrough was created on a D2 v2-sized Linux Data Science Virtual Machine.</span></span> <span data-ttu-id="69034-131">Tato velikost DSVM je schopná zpracovávat hello postupy v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="69034-131">This size DSVM is capable of handling hello procedures in this walkthrough.</span></span>
>
>

<span data-ttu-id="69034-132">Pokud potřebujete další prostor úložiště, můžete vytvořit další disky a připojte je tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="69034-132">If you need more storage space, you can create additional disks and attach them tooyour VM.</span></span> <span data-ttu-id="69034-133">Tyto disky používají trvalé úložiště Azure, takže jejich uchování dat i v případě, že hello server je znovu poskytnuto kvůli tooresizing nebo je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="69034-133">These disks use persistent Azure storage, so their data is preserved even when hello server is reprovisioned due tooresizing or is shut down.</span></span> <span data-ttu-id="69034-134">tooadd disku a jeho připojení tooyour virtuálních počítačů, postupujte podle pokynů hello v [přidat tooa disku virtuálního počítače s Linuxem](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="69034-134">tooadd a disk and attach it tooyour VM, follow hello instructions in [Add a disk tooa Linux VM](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="69034-135">Tyto kroky použijte rozhraní příkazového řádku Azure (Azure CLI), který je již nainstalován na hello DSVM hello.</span><span class="sxs-lookup"><span data-stu-id="69034-135">These steps use hello Azure Command-Line Interface (Azure CLI), which is already installed on hello DSVM.</span></span> <span data-ttu-id="69034-136">Proto tyto postupy lze provést zcela z hello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="69034-136">So these procedures can be done entirely from hello VM itself.</span></span> <span data-ttu-id="69034-137">Jiná možnost tooincrease úložiště je toouse [soubory Azure](../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="69034-137">Another option tooincrease storage is toouse [Azure files](../storage/files/storage-how-to-use-files-linux.md).</span></span>

<span data-ttu-id="69034-138">toodownload hello data, otevřete okno terminálu a spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="69034-138">toodownload hello data, open a terminal window and run this command:</span></span>

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

<span data-ttu-id="69034-139">stažený soubor Hello nemá řádek záhlaví, takže vytvoříme jiný soubor, který má hlavičku.</span><span class="sxs-lookup"><span data-stu-id="69034-139">hello downloaded file does not have a header row, so let's create another file that does have a header.</span></span> <span data-ttu-id="69034-140">Spusťte tento příkaz toocreate soubor s příslušnou hlavičky hello:</span><span class="sxs-lookup"><span data-stu-id="69034-140">Run this command toocreate a file with hello appropriate headers:</span></span>

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

<span data-ttu-id="69034-141">Potom řetězení hello společně s hello příkaz dva soubory:</span><span class="sxs-lookup"><span data-stu-id="69034-141">Then concatenate hello two files together with hello command:</span></span>

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

<span data-ttu-id="69034-142">datovou sadu Hello má několik typů statistik na každou e-mailu:</span><span class="sxs-lookup"><span data-stu-id="69034-142">hello dataset has several types of statistics on each email:</span></span>

* <span data-ttu-id="69034-143">Sloupce jako ***word\_frekvence\_WORD*** znamenat hello procento slova v hello e-mailu, které odpovídají *WORD*.</span><span class="sxs-lookup"><span data-stu-id="69034-143">Columns like ***word\_freq\_WORD*** indicate hello percentage of words in hello email that match *WORD*.</span></span> <span data-ttu-id="69034-144">Například pokud *word\_frekvence\_zkontrolujte* je 1, 1 % všechna slova v e-mailu hello byly *zkontrolujte*.</span><span class="sxs-lookup"><span data-stu-id="69034-144">For example, if *word\_freq\_make* is 1, then 1% of all words in hello email were *make*.</span></span>
* <span data-ttu-id="69034-145">Sloupce jako ***char\_frekvence\_CHAR*** znamenat hello procento všechny znaky v hello e-mailu, které byly *CHAR*.</span><span class="sxs-lookup"><span data-stu-id="69034-145">Columns like ***char\_freq\_CHAR*** indicate hello percentage of all characters in hello email that were *CHAR*.</span></span>
* <span data-ttu-id="69034-146">***kapitálové\_spustit\_délka\_nejdelší*** je hello nejdelší délka posloupnosti velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="69034-146">***capital\_run\_length\_longest*** is hello longest length of a sequence of capital letters.</span></span>
* <span data-ttu-id="69034-147">***kapitálové\_spustit\_délka\_průměrná*** je hello průměrnou délku všech pořadí velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="69034-147">***capital\_run\_length\_average*** is hello average length of all sequences of capital letters.</span></span>
* <span data-ttu-id="69034-148">***kapitálové\_spustit\_délka\_celkový*** je celková délka hello všech pořadí velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="69034-148">***capital\_run\_length\_total*** is hello total length of all sequences of capital letters.</span></span>
* <span data-ttu-id="69034-149">***nevyžádané pošty*** Určuje, zda e-mailu hello byl úvahy nevyžádané pošty nebo ne (1 = nevyžádané pošty, 0 = není nevyžádané poště).</span><span class="sxs-lookup"><span data-stu-id="69034-149">***spam*** indicates whether hello email was considered spam or not (1 = spam, 0 = not spam).</span></span>

## <a name="explore-hello-dataset-with-microsoft-r-open"></a><span data-ttu-id="69034-150">Prozkoumejte hello datovou sadu s Microsoft R otevřete</span><span class="sxs-lookup"><span data-stu-id="69034-150">Explore hello dataset with Microsoft R Open</span></span>
<span data-ttu-id="69034-151">Umožňuje zkontrolovat hello data a provádět některé základní machine learning s R. hello vědecké účely dat virtuálních počítačů se dodává s [Microsoft R otevřete](https://mran.revolutionanalytics.com/open/) předinstalován.</span><span class="sxs-lookup"><span data-stu-id="69034-151">Let's examine hello data and do some basic machine learning with R. hello Data Science VM comes with [Microsoft R Open](https://mran.revolutionanalytics.com/open/) pre-installed.</span></span> <span data-ttu-id="69034-152">Hello matematické vícevláknové knihovny v této verzi R nabízí lepší výkon než různých verzí jednovláknové.</span><span class="sxs-lookup"><span data-stu-id="69034-152">hello multithreaded math libraries in this version of R offer better performance than various single-threaded versions.</span></span> <span data-ttu-id="69034-153">Microsoft R otevřete také poskytuje reprodukovatelnosti pomocí snímku úložiště balíčků CRAN hello.</span><span class="sxs-lookup"><span data-stu-id="69034-153">Microsoft R Open also provides reproducibility by using a snapshot of hello CRAN package repository.</span></span>

<span data-ttu-id="69034-154">Ukázky v tomto návodu, klonování hello používá kódu tooget kopie hello **Azure-Machine-Learning--vědecké zpracování dat** úložiště pomocí git, který je předem nainstalovaná na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="69034-154">tooget copies of hello code samples used in this walkthrough, clone hello **Azure-Machine-Learning-Data-Science** repository using git, which is pre-installed on hello VM.</span></span> <span data-ttu-id="69034-155">Z příkazového řádku hello git spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="69034-155">From hello git command line, run:</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="69034-156">Otevřete okno terminálu a spusťte novou relaci R s hello R interaktivní konzoly.</span><span class="sxs-lookup"><span data-stu-id="69034-156">Open a terminal window and start a new R session with hello R interactive console.</span></span>

> [!NOTE]
> <span data-ttu-id="69034-157">Můžete taky Rstudia pro hello následující postupy.</span><span class="sxs-lookup"><span data-stu-id="69034-157">You can also use RStudio for hello following procedures.</span></span> <span data-ttu-id="69034-158">tooinstall Rstudia, provedení tohoto příkazu v terminálu:`./Desktop/DSVM\ tools/installRStudio.sh`</span><span class="sxs-lookup"><span data-stu-id="69034-158">tooinstall RStudio, execute this command at a terminal: `./Desktop/DSVM\ tools/installRStudio.sh`</span></span>
>
>

<span data-ttu-id="69034-159">tooimport hello data a nastavení prostředí hello, spusťte:</span><span class="sxs-lookup"><span data-stu-id="69034-159">tooimport hello data and set up hello environment, run:</span></span>

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

<span data-ttu-id="69034-160">toosee souhrnné statistické údaje o jednotlivých sloupců:</span><span class="sxs-lookup"><span data-stu-id="69034-160">toosee summary statistics about each column:</span></span>

    summary(data)

<span data-ttu-id="69034-161">Pro jiné zobrazení dat hello:</span><span class="sxs-lookup"><span data-stu-id="69034-161">For a different view of hello data:</span></span>

    str(data)

<span data-ttu-id="69034-162">Ukazuje to hello typ pro každou proměnnou a hello nejprve několik hodnot v datové sadě hello.</span><span class="sxs-lookup"><span data-stu-id="69034-162">This shows you hello type of each variable and hello first few values in hello dataset.</span></span>

<span data-ttu-id="69034-163">Hello *nevyžádané pošty* sloupec byl načten jako celé číslo, ale je ve skutečnosti kategorií proměnné (nebo multi-Factor).</span><span class="sxs-lookup"><span data-stu-id="69034-163">hello *spam* column was read as an integer, but it's actually a categorical variable (or factor).</span></span> <span data-ttu-id="69034-164">tooset typ:</span><span class="sxs-lookup"><span data-stu-id="69034-164">tooset its type:</span></span>

    data$spam <- as.factor(data$spam)

<span data-ttu-id="69034-165">toodo některé průzkumné analýzy, použijte hello [ggplot2](http://ggplot2.org/) balíček oblíbených grafovým knihovny pro R, který už je nainstalovaný na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="69034-165">toodo some exploratory analysis, use hello [ggplot2](http://ggplot2.org/) package, a popular graphing library for R that is already installed on hello VM.</span></span> <span data-ttu-id="69034-166">Poznámka od hello souhrnná data zobrazit dřív, máme souhrnné statistiky na hello frekvenci hello vykřičník znak.</span><span class="sxs-lookup"><span data-stu-id="69034-166">Note, from hello summary data displayed earlier, that we have summary statistics on hello frequency of hello exclamation mark character.</span></span> <span data-ttu-id="69034-167">Umožňuje vykreslení těchto frekvencí zde s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="69034-167">Let's plot those frequencies here with hello following commands:</span></span>

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="69034-168">Vzhledem k tomu, že hello nula panelu je zkosení hello výkresu, budeme se jich zbavit:</span><span class="sxs-lookup"><span data-stu-id="69034-168">Since hello zero bar is skewing hello plot, let's get rid of it:</span></span>

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="69034-169">Je větší než 1, která vypadá zajímavé netriviální hustotu.</span><span class="sxs-lookup"><span data-stu-id="69034-169">There is a non-trivial density above 1 that looks interesting.</span></span> <span data-ttu-id="69034-170">Podívejme se na právě tato data:</span><span class="sxs-lookup"><span data-stu-id="69034-170">Let's look at just that data:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

<span data-ttu-id="69034-171">Potom ji rozdělte podle šunkou vs nevyžádané pošty:</span><span class="sxs-lookup"><span data-stu-id="69034-171">Then split it by spam vs ham:</span></span>

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

<span data-ttu-id="69034-172">Tyto příklady by měl umožňují toomake podobné pozemků Dobrý den dalších sloupců tooexplore hello data obsažená v nich.</span><span class="sxs-lookup"><span data-stu-id="69034-172">These examples should enable you toomake similar plots of hello other columns tooexplore hello data contained in them.</span></span>

## <a name="train-and-test-an-ml-model"></a><span data-ttu-id="69034-173">Natrénuje a otestuje ML model</span><span class="sxs-lookup"><span data-stu-id="69034-173">Train and test an ML model</span></span>
<span data-ttu-id="69034-174">Nyní Pojďme train několika machine learning modelů tooclassify hello e-mailů v datové sadě hello jako obsahující buď span nebo šunkou.</span><span class="sxs-lookup"><span data-stu-id="69034-174">Now let's train a couple of machine learning models tooclassify hello emails in hello dataset as containing either span or ham.</span></span> <span data-ttu-id="69034-175">Jsme učení rozhodovacího stromu modelu a modelu náhodných doménové struktury v této části a pak testování jejich přesnost své předpovědi.</span><span class="sxs-lookup"><span data-stu-id="69034-175">We train a decision tree model and a random forest model in this section and then test their accuracy of their predictions.</span></span>

> [!NOTE]
> <span data-ttu-id="69034-176">balíček rpart (rekurzivní vytváření oddílů a regresní stromy) Hello používá v hello následující kód je již nainstalován na hello vědecké účely dat virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="69034-176">hello rpart (Recursive Partitioning and Regression Trees) package used in hello following code is already installed on hello Data Science VM.</span></span>
>
>

<span data-ttu-id="69034-177">První můžeme rozdělit hello datové sady na školení a testovací sady:</span><span class="sxs-lookup"><span data-stu-id="69034-177">First, let's split hello dataset into training and test sets:</span></span>

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

<span data-ttu-id="69034-178">A pak vytvořte rozhodovací strom tooclassify hello e-mailů.</span><span class="sxs-lookup"><span data-stu-id="69034-178">And then create a decision tree tooclassify hello emails.</span></span>

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

<span data-ttu-id="69034-179">Tady je výsledek hello:</span><span class="sxs-lookup"><span data-stu-id="69034-179">Here is hello result:</span></span>

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

<span data-ttu-id="69034-181">toodetermine její výkon na školení hello nastavení, použijte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="69034-181">toodetermine how well it performs on hello training set, use hello following code:</span></span>

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="69034-182">toodetermine jak dobře provádí na testovací sada hello:</span><span class="sxs-lookup"><span data-stu-id="69034-182">toodetermine how well it performs on hello test set:</span></span>

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

<span data-ttu-id="69034-183">Zkuste taky umožňuje model náhodných doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="69034-183">Let's also try a random forest model.</span></span> <span data-ttu-id="69034-184">Náhodné doménovými strukturami cvičení velkého množství rozhodovací stromy a výstup třídu, která hello režim hello klasifikace ze všech jednotlivých rozhodovací stromy hello.</span><span class="sxs-lookup"><span data-stu-id="69034-184">Random forests train a multitude of decision trees and output a class that is hello mode of hello classifications from all of hello individual decision trees.</span></span> <span data-ttu-id="69034-185">Poskytují účinnější strojového učení a přístup podle jejich odstraňte hello tendence rozhodovacího stromu modelu toooverfit školení datové sady.</span><span class="sxs-lookup"><span data-stu-id="69034-185">They provide a more powerful machine learning approach as they correct for hello tendency of a decision tree model toooverfit a training dataset.</span></span>

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-tooazure-ml"></a><span data-ttu-id="69034-186">Nasazení tooAzure modelu ML</span><span class="sxs-lookup"><span data-stu-id="69034-186">Deploy a model tooAzure ML</span></span>
<span data-ttu-id="69034-187">[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) je Cloudová služba, která umožňuje snadno toobuild a nasadit řešení prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="69034-187">[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) is a cloud service that makes it easy toobuild and deploy predictive analytics models.</span></span> <span data-ttu-id="69034-188">Jednou z funkcí hello dobrý AzureML je jeho možnost toopublish žádné R fungovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="69034-188">One of hello nice features of AzureML is its ability toopublish any R function as a web service.</span></span> <span data-ttu-id="69034-189">Hello balíček AzureML R vytváří snadno toodo nasazení přímo naší relace R na hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="69034-189">hello AzureML R package makes deployment easy toodo right from our R session on hello DSVM.</span></span>

<span data-ttu-id="69034-190">toodeploy hello rozhodovací strom kód z předchozí části hello, musíte toosign v tooAzure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="69034-190">toodeploy hello decision tree code from hello previous section, you need toosign in tooAzure Machine Learning Studio.</span></span> <span data-ttu-id="69034-191">Potřebujete ID vašeho pracovního prostoru a tokenu toosign autorizace v.</span><span class="sxs-lookup"><span data-stu-id="69034-191">You need your workspace ID and an authorization token toosign in.</span></span> <span data-ttu-id="69034-192">toofind tyto hodnoty a proměnné AzureML hello inicializovat s nimi:</span><span class="sxs-lookup"><span data-stu-id="69034-192">toofind these values and initialize hello AzureML variables with them:</span></span>

<span data-ttu-id="69034-193">Vyberte **nastavení** v levé nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="69034-193">Select **Settings** on hello left-hand menu.</span></span> <span data-ttu-id="69034-194">Poznámka: vaše **ID pracovního prostoru**.</span><span class="sxs-lookup"><span data-stu-id="69034-194">Note your **WORKSPACE ID**.</span></span> <span data-ttu-id="69034-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span><span class="sxs-lookup"><span data-stu-id="69034-195">![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)</span></span>

<span data-ttu-id="69034-196">Vyberte **autorizace tokeny** z nabídky režijní hello a Poznámka vaše **primární autorizační Token**.![ 3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span><span class="sxs-lookup"><span data-stu-id="69034-196">Select **Authorization Tokens** from hello overhead menu and note your **Primary Authorization Token**.![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)</span></span>

<span data-ttu-id="69034-197">Zatížení hello **AzureML** balíček a potom nastavit hodnoty proměnných hello se vaše ID token a pracovního prostoru v relaci prostředí R na hello DSVM:</span><span class="sxs-lookup"><span data-stu-id="69034-197">Load hello **AzureML** package and then set values of hello variables with your token and workspace ID in your R session on hello DSVM:</span></span>

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


<span data-ttu-id="69034-198">Umožňuje zjednodušit hello modelu toomake jednodušší tooimplement této ukázce.</span><span class="sxs-lookup"><span data-stu-id="69034-198">Let's simplify hello model toomake this demonstration easier tooimplement.</span></span> <span data-ttu-id="69034-199">Vyberte hello tří proměnných v hello rozhodovací strom nejbližší toohello kořenové a vytvořit novou větev pomocí právě těchto tří proměnných:</span><span class="sxs-lookup"><span data-stu-id="69034-199">Pick hello three variables in hello decision tree closest toohello root and build a new tree using just those three variables:</span></span>

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

<span data-ttu-id="69034-200">Potřebujeme předpovědi funkce, která přebírá hello funkce jako vstup a vrátí hello předpovězené hodnoty:</span><span class="sxs-lookup"><span data-stu-id="69034-200">We need a prediction function that takes hello features as an input and returns hello predicted values:</span></span>

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

<span data-ttu-id="69034-201">Publikování hello predictSpam funkce tooAzureML pomocí hello **publishWebService** funkce:</span><span class="sxs-lookup"><span data-stu-id="69034-201">Publish hello predictSpam function tooAzureML using hello **publishWebService** function:</span></span>

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

<span data-ttu-id="69034-202">Tato funkce přebírá hello **predictSpam** fungovat, vytvoří webové služby s názvem **spamWebService** s definované vstupy a výstupy a vrátí informace o nový koncový bod hello.</span><span class="sxs-lookup"><span data-stu-id="69034-202">This function takes hello **predictSpam** function, creates a web service named **spamWebService** with defined inputs and outputs, and returns information about hello new endpoint.</span></span>

<span data-ttu-id="69034-203">Zobrazení podrobností hello publikované webové služby, včetně jeho klíče rozhraní API koncového bodu a přístup pomocí příkazu hello:</span><span class="sxs-lookup"><span data-stu-id="69034-203">View details of hello published web service, including its API endpoint and access keys with hello command:</span></span>

    spamWebService[[2]]

<span data-ttu-id="69034-204">tootry ho na hello prvních 10 řádků hello testovací sada:</span><span class="sxs-lookup"><span data-stu-id="69034-204">tootry it out on hello first 10 rows of hello test set:</span></span>

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a><span data-ttu-id="69034-205">Použít jiné nástroje, které jsou k dispozici</span><span class="sxs-lookup"><span data-stu-id="69034-205">Use other tools available</span></span>
<span data-ttu-id="69034-206">Hello zbývající části ukazují, jak hello toouse některé nástroje hello nainstalovat na virtuální počítač s Linuxem dat vědecké účely. Tady je hello seznam nástrojů, které jsou popsané:</span><span class="sxs-lookup"><span data-stu-id="69034-206">hello remaining sections show how toouse some of hello tools installed on hello Linux Data Science VM.Here is hello list of tools discussed:</span></span>

* <span data-ttu-id="69034-207">XGBoost</span><span class="sxs-lookup"><span data-stu-id="69034-207">XGBoost</span></span>
* <span data-ttu-id="69034-208">Python</span><span class="sxs-lookup"><span data-stu-id="69034-208">Python</span></span>
* <span data-ttu-id="69034-209">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="69034-209">Jupyterhub</span></span>
* <span data-ttu-id="69034-210">Rattle</span><span class="sxs-lookup"><span data-stu-id="69034-210">Rattle</span></span>
* <span data-ttu-id="69034-211">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="69034-211">PostgreSQL & Squirrel SQL</span></span>
* <span data-ttu-id="69034-212">SQL Server datového skladu</span><span class="sxs-lookup"><span data-stu-id="69034-212">SQL Server Data Warehouse</span></span>

## <a name="xgboost"></a><span data-ttu-id="69034-213">XGBoost</span><span class="sxs-lookup"><span data-stu-id="69034-213">XGBoost</span></span>
<span data-ttu-id="69034-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) je nástroj, který poskytuje rychlé a přesné boosted stromu implementaci.</span><span class="sxs-lookup"><span data-stu-id="69034-214">[XGBoost](https://xgboost.readthedocs.org/en/latest/) is a tool that provides a fast and accurate boosted tree implementation.</span></span>

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

<span data-ttu-id="69034-215">XGBoost můžete také volat z pythonu nebo příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="69034-215">XGBoost can also call from python or a command line.</span></span>

## <a name="python"></a><span data-ttu-id="69034-216">Python</span><span class="sxs-lookup"><span data-stu-id="69034-216">Python</span></span>
<span data-ttu-id="69034-217">Pro vývoj pomocí Python se nainstalovaly v hello DSVM hello Anaconda Python distribuce 2.7 a 3.5.</span><span class="sxs-lookup"><span data-stu-id="69034-217">For development using Python, hello Anaconda Python distributions 2.7 and 3.5 have been installed in hello DSVM.</span></span>

> [!NOTE]
> <span data-ttu-id="69034-218">zahrnuje Hello distribuční Anaconda [Condas](http://conda.pydata.org/docs/index.html), což může být použité toocreate vlastní prostředí pro jazyk Python, které mají různé verze nebo balíčky nainstalované v nich.</span><span class="sxs-lookup"><span data-stu-id="69034-218">hello Anaconda distribution includes [Condas](http://conda.pydata.org/docs/index.html), which can be used toocreate custom environments for Python that have different versions and/or packages installed in them.</span></span>
>
>

<span data-ttu-id="69034-219">Umožňuje číst v některých hello spambase datovou sadu a klasifikovat hello e-mailů s support vector počítačů v scikit-Další informace:</span><span class="sxs-lookup"><span data-stu-id="69034-219">Let's read in some of hello spambase dataset and classify hello emails with support vector machines in scikit-learn:</span></span>

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="69034-220">toomake předpovědi:</span><span class="sxs-lookup"><span data-stu-id="69034-220">toomake predictions:</span></span>

    clf.predict(X.ix[0:20, :])

<span data-ttu-id="69034-221">tooshow jak toopublish koncového bodu AzureML umožňuje zpřístupnění jednodušší model hello tří proměnných, jako jsme to udělali při jsme dříve publikované hello R modelu.</span><span class="sxs-lookup"><span data-stu-id="69034-221">tooshow how toopublish an AzureML endpoint, let's make a simpler model hello three variables as we did when we published hello R model previously.</span></span>

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

<span data-ttu-id="69034-222">toopublish hello modelu tooAzureML:</span><span class="sxs-lookup"><span data-stu-id="69034-222">toopublish hello model tooAzureML:</span></span>

    # Publish hello model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about hello resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call hello model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> <span data-ttu-id="69034-223">To je dostupná jenom pro python 2.7 a není dosud nepodporováno u 3.5.</span><span class="sxs-lookup"><span data-stu-id="69034-223">This is only available for python 2.7 and is not yet supported on 3.5.</span></span> <span data-ttu-id="69034-224">Spustit s **/anaconda/bin/python2.7**.</span><span class="sxs-lookup"><span data-stu-id="69034-224">Run with **/anaconda/bin/python2.7**.</span></span>
>
>

## <a name="jupyterhub"></a><span data-ttu-id="69034-225">Jupyterhub</span><span class="sxs-lookup"><span data-stu-id="69034-225">Jupyterhub</span></span>
<span data-ttu-id="69034-226">distribuce Anaconda Hello v hello DSVM se dodává s poznámkového bloku Jupyter, kód pro Python, R nebo Dita tooshare prostředí napříč platformami a analýzy.</span><span class="sxs-lookup"><span data-stu-id="69034-226">hello Anaconda distribution in hello DSVM comes with a Jupyter notebook, a cross-platform environment tooshare Python, R, or Julia code and analysis.</span></span> <span data-ttu-id="69034-227">Poznámkový blok Jupyter Hello je přístupné přes JupyterHub.</span><span class="sxs-lookup"><span data-stu-id="69034-227">hello Jupyter notebook is accessed through JupyterHub.</span></span> <span data-ttu-id="69034-228">Přihlášení pomocí místních Linux uživatelské jméno a heslo v ***https://\<název DNS virtuálního počítače nebo IP adresu\>: 8000 /***.</span><span class="sxs-lookup"><span data-stu-id="69034-228">You sign in using your local Linux user name and password at ***https://\<VM DNS name or IP Address\>:8000/***.</span></span> <span data-ttu-id="69034-229">Všechny konfigurační soubory pro JupyterHub se nacházejí v adresáři **/etc/jupyterhub**.</span><span class="sxs-lookup"><span data-stu-id="69034-229">All configuration files for JupyterHub are found in directory **/etc/jupyterhub**.</span></span>

<span data-ttu-id="69034-230">Několik ukázkových poznámkových bloků jsou již nainstalovány na hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="69034-230">Several sample notebooks are already installed on hello VM:</span></span>

* <span data-ttu-id="69034-231">V tématu hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) pro ukázkové Python Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="69034-231">See hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) for a sample Python notebook.</span></span>
* <span data-ttu-id="69034-232">V tématu [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) pro ukázku **R** poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="69034-232">See [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) for a sample **R** notebook.</span></span>
* <span data-ttu-id="69034-233">V tématu hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) pro jiné ukázku **Python** poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="69034-233">See hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) for another sample **Python** notebook.</span></span>

> [!NOTE]
> <span data-ttu-id="69034-234">Dále je dostupný z příkazového řádku hello na hello virtuálního počítače s Linuxem datové vědy Hello Dita jazyk.</span><span class="sxs-lookup"><span data-stu-id="69034-234">hello Julia language is also available from hello command line on hello Linux Data Science VM.</span></span>
>
>

## <a name="rattle"></a><span data-ttu-id="69034-235">Rattle</span><span class="sxs-lookup"><span data-stu-id="69034-235">Rattle</span></span>
<span data-ttu-id="69034-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (hello Analytical nástroj R tooLearn snadno) je grafický nástroj pro dolování dat R.</span><span class="sxs-lookup"><span data-stu-id="69034-236">[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (hello R Analytical Tool tooLearn Easily) is a graphical R tool for data mining.</span></span> <span data-ttu-id="69034-237">Obsahuje intuitivní rozhraní, které umožňuje snadno tooload, prozkoumat a transformovat data a vytvářet a vyhodnocení modelů.</span><span class="sxs-lookup"><span data-stu-id="69034-237">It has an intuitive interface that makes it easy tooload, explore, and transform data and build and evaluate models.</span></span>  <span data-ttu-id="69034-238">článek Hello [Rattle: A Data Mining grafického uživatelského rozhraní pro R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) poskytuje návod, který ukazuje její funkce.</span><span class="sxs-lookup"><span data-stu-id="69034-238">hello article [Rattle: A Data Mining GUI for R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) provides a walkthrough that demonstrates its features.</span></span>

<span data-ttu-id="69034-239">Instalace a spuštění Rattle s hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="69034-239">Install and start Rattle with hello following commands:</span></span>

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> <span data-ttu-id="69034-240">Není nutné instalovat na hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="69034-240">Installation is not required on hello DSVM.</span></span> <span data-ttu-id="69034-241">Ale Rattle můžete být vyzváni další balíčky tooinstall až ho načte.</span><span class="sxs-lookup"><span data-stu-id="69034-241">But Rattle may prompt you tooinstall additional packages when it loads.</span></span>
>
>

<span data-ttu-id="69034-242">Rattle používá rozhraní založené na kartě.</span><span class="sxs-lookup"><span data-stu-id="69034-242">Rattle uses a tab-based interface.</span></span> <span data-ttu-id="69034-243">Většina hello karty odpovídají toosteps v hello [proces vědecké účely dat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), jako jsou načítání dat nebo zkoumat ho.</span><span class="sxs-lookup"><span data-stu-id="69034-243">Most of hello tabs correspond toosteps in hello [Data Science Process](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), like loading data or exploring it.</span></span> <span data-ttu-id="69034-244">proces vědecké účely dat Hello toky z levé tooright prostřednictvím karty hello.</span><span class="sxs-lookup"><span data-stu-id="69034-244">hello data science process flows from left tooright through hello tabs.</span></span> <span data-ttu-id="69034-245">Ale hello poslední karta obsahuje protokolu hello R příkazy spustit Rattle.</span><span class="sxs-lookup"><span data-stu-id="69034-245">But hello last tab contains a log of hello R commands run by Rattle.</span></span>

<span data-ttu-id="69034-246">tooload a nakonfigurujte datovou sadu hello:</span><span class="sxs-lookup"><span data-stu-id="69034-246">tooload and configure hello dataset:</span></span>

* <span data-ttu-id="69034-247">tooload hello soubor, vyberte hello **Data** kartě, pak</span><span class="sxs-lookup"><span data-stu-id="69034-247">tooload hello file, select hello **Data** tab, then</span></span>
* <span data-ttu-id="69034-248">Zvolte hello selektor další příliš**Filename** a zvolte **spambaseHeaders.data**.</span><span class="sxs-lookup"><span data-stu-id="69034-248">Choose hello selector next too**Filename** and choose **spambaseHeaders.data**.</span></span>
* <span data-ttu-id="69034-249">soubor tooload hello.</span><span class="sxs-lookup"><span data-stu-id="69034-249">tooload hello file.</span></span> <span data-ttu-id="69034-250">Vyberte **Execute** v horní řádek hello tlačítek.</span><span class="sxs-lookup"><span data-stu-id="69034-250">select **Execute** in hello top row of buttons.</span></span> <span data-ttu-id="69034-251">Měli byste vidět souhrn jednotlivých sloupců, včetně jeho identifikovaných datového typu, ať už se jedná o vstup, cíl nebo jiný typ proměnné a hello počet jedinečných hodnot.</span><span class="sxs-lookup"><span data-stu-id="69034-251">You should see a summary of each column, including its identified data type, whether it's an input, a target, or other type of variable, and hello number of unique values.</span></span>
* <span data-ttu-id="69034-252">Rattle má identifikovány správně hello **nevyžádané pošty** sloupec jako cíl hello.</span><span class="sxs-lookup"><span data-stu-id="69034-252">Rattle has correctly identified hello **spam** column as hello target.</span></span> <span data-ttu-id="69034-253">Vyberte hello nevyžádané pošty sloupec a potom sadu hello **cílový datový typ** příliš**Categoric**.</span><span class="sxs-lookup"><span data-stu-id="69034-253">Select hello spam column, then set hello **Target Data Type** too**Categoric**.</span></span>

<span data-ttu-id="69034-254">tooexplore hello data:</span><span class="sxs-lookup"><span data-stu-id="69034-254">tooexplore hello data:</span></span>

* <span data-ttu-id="69034-255">Vyberte hello **prozkoumat** kartě.</span><span class="sxs-lookup"><span data-stu-id="69034-255">Select hello **Explore** tab.</span></span>
* <span data-ttu-id="69034-256">Klikněte na tlačítko **souhrnné**, pak **Execute**, toosee některé informace o hello typy proměnných a některé souhrnné statistiky.</span><span class="sxs-lookup"><span data-stu-id="69034-256">Click **Summary**, then **Execute**, toosee some information about hello variable types and some summary statistics.</span></span>
* <span data-ttu-id="69034-257">tooview jiné typy Statistika každou proměnnou, vyberte jiné možnosti jako **popisujících** nebo **Základy**.</span><span class="sxs-lookup"><span data-stu-id="69034-257">tooview other types of statistics about each variable, select other options like **Describe** or **Basics**.</span></span>

<span data-ttu-id="69034-258">Hello **prozkoumat** karta vám umožní toogenerate mnoho pronikavého ukazuje zeměpisný.</span><span class="sxs-lookup"><span data-stu-id="69034-258">hello **Explore** tab also allows you toogenerate many insightful plots.</span></span> <span data-ttu-id="69034-259">tooplot histogram hello dat:</span><span class="sxs-lookup"><span data-stu-id="69034-259">tooplot a histogram of hello data:</span></span>

* <span data-ttu-id="69034-260">Vyberte **distribuce**.</span><span class="sxs-lookup"><span data-stu-id="69034-260">Select **Distributions**.</span></span>
* <span data-ttu-id="69034-261">Zkontrolujte **Histogram** pro **word_freq_remove** a **word_freq_you**.</span><span class="sxs-lookup"><span data-stu-id="69034-261">Check **Histogram** for **word_freq_remove** and **word_freq_you**.</span></span>
* <span data-ttu-id="69034-262">Vyberte **provést**.</span><span class="sxs-lookup"><span data-stu-id="69034-262">Select **Execute**.</span></span> <span data-ttu-id="69034-263">Měli byste vidět, že oba hustotu ukazuje zeměpisný v okně jednoho grafu, pokud je zřejmé, že hello slovo "vy" se zobrazí mnohem častěji v e-mailů než "Odebrat".</span><span class="sxs-lookup"><span data-stu-id="69034-263">You should see both density plots in a single graph window, where it is clear that hello word "you" appears much more frequently in emails than "remove".</span></span>

<span data-ttu-id="69034-264">Hello korelace pozemků jsou také zajímavé.</span><span class="sxs-lookup"><span data-stu-id="69034-264">hello Correlation plots are also interesting.</span></span> <span data-ttu-id="69034-265">toocreate jeden:</span><span class="sxs-lookup"><span data-stu-id="69034-265">toocreate one:</span></span>

* <span data-ttu-id="69034-266">Zvolte **korelace** jako hello **typu**, pak</span><span class="sxs-lookup"><span data-stu-id="69034-266">Choose **Correlation** as hello **Type**, then</span></span>
* <span data-ttu-id="69034-267">Vyberte **provést**.</span><span class="sxs-lookup"><span data-stu-id="69034-267">Select **Execute**.</span></span>
* <span data-ttu-id="69034-268">Rattle vás varuje, že doporučí maximálně 40 proměnné.</span><span class="sxs-lookup"><span data-stu-id="69034-268">Rattle warns you that it recommends a maximum of 40 variables.</span></span> <span data-ttu-id="69034-269">Vyberte **Ano** tooview hello vykreslení.</span><span class="sxs-lookup"><span data-stu-id="69034-269">Select **Yes** tooview hello plot.</span></span>

<span data-ttu-id="69034-270">Existují některé zajímavé korelací, které se spustit: "technologie" důrazně korelační příliš "HP" a "labs", např.</span><span class="sxs-lookup"><span data-stu-id="69034-270">There are some interesting correlations that come up: "technology" is strongly correlated too"HP" and "labs", for example.</span></span> <span data-ttu-id="69034-271">Je také důrazně korelační příliš "650", protože je 650 hello směrové číslo oblasti dárců hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="69034-271">It is also strongly correlated too"650", because hello area code of hello dataset donors is 650.</span></span>

<span data-ttu-id="69034-272">v okně Explore hello jsou k dispozici Hello číselné hodnoty hello korelací mezi slovy.</span><span class="sxs-lookup"><span data-stu-id="69034-272">hello numeric values for hello correlations between words are available in hello Explore window.</span></span> <span data-ttu-id="69034-273">Je zajímavé toonote, například, že je "technologie" negativní korelační s "vaše" a "peníze".</span><span class="sxs-lookup"><span data-stu-id="69034-273">It is interesting toonote, for example, that "technology" is negatively correlated with "your" and "money".</span></span>

<span data-ttu-id="69034-274">Rattle můžete převést hello datovou sadu toohandle některé běžné problémy.</span><span class="sxs-lookup"><span data-stu-id="69034-274">Rattle can transform hello dataset toohandle some common issues.</span></span> <span data-ttu-id="69034-275">Například, ale umožňuje vám toorescale funkce, dává chybějící hodnoty, zpracování extrémních a odebrat proměnné nebo připomínky s chybějící data.</span><span class="sxs-lookup"><span data-stu-id="69034-275">For example, it allows you toorescale features, impute missing values, handle outliers, and remove variables or observations with missing data.</span></span> <span data-ttu-id="69034-276">Rattle můžete také určit pravidla přidružení mezi připomínky nebo proměnné.</span><span class="sxs-lookup"><span data-stu-id="69034-276">Rattle can also identify association rules between observations and/or variables.</span></span> <span data-ttu-id="69034-277">Tyto karty jsou nad rámec této úvodní prohlídka.</span><span class="sxs-lookup"><span data-stu-id="69034-277">These tabs are out of scope for this introductory walkthrough.</span></span>

<span data-ttu-id="69034-278">Rattle můžete také provést analýzu clusteru.</span><span class="sxs-lookup"><span data-stu-id="69034-278">Rattle can also perform cluster analysis.</span></span> <span data-ttu-id="69034-279">Umožňuje vyloučit některé funkce toomake hello výstup jednodušší tooread.</span><span class="sxs-lookup"><span data-stu-id="69034-279">Let's exclude some features toomake hello output easier tooread.</span></span> <span data-ttu-id="69034-280">Na hello **Data** , zvolte **Ignorovat** další tooeach hello proměnných s výjimkou těchto deset položek:</span><span class="sxs-lookup"><span data-stu-id="69034-280">On hello **Data** tab, choose **Ignore** next tooeach of hello variables except these ten items:</span></span>

* <span data-ttu-id="69034-281">word_freq_hp</span><span class="sxs-lookup"><span data-stu-id="69034-281">word_freq_hp</span></span>
* <span data-ttu-id="69034-282">word_freq_technology</span><span class="sxs-lookup"><span data-stu-id="69034-282">word_freq_technology</span></span>
* <span data-ttu-id="69034-283">word_freq_george</span><span class="sxs-lookup"><span data-stu-id="69034-283">word_freq_george</span></span>
* <span data-ttu-id="69034-284">word_freq_remove</span><span class="sxs-lookup"><span data-stu-id="69034-284">word_freq_remove</span></span>
* <span data-ttu-id="69034-285">word_freq_your</span><span class="sxs-lookup"><span data-stu-id="69034-285">word_freq_your</span></span>
* <span data-ttu-id="69034-286">word_freq_dollar</span><span class="sxs-lookup"><span data-stu-id="69034-286">word_freq_dollar</span></span>
* <span data-ttu-id="69034-287">word_freq_money</span><span class="sxs-lookup"><span data-stu-id="69034-287">word_freq_money</span></span>
* <span data-ttu-id="69034-288">capital_run_length_longest</span><span class="sxs-lookup"><span data-stu-id="69034-288">capital_run_length_longest</span></span>
* <span data-ttu-id="69034-289">word_freq_business</span><span class="sxs-lookup"><span data-stu-id="69034-289">word_freq_business</span></span>
* <span data-ttu-id="69034-290">nevyžádané pošty</span><span class="sxs-lookup"><span data-stu-id="69034-290">spam</span></span>

<span data-ttu-id="69034-291">Potom přejděte zpět toohello **clusteru** , zvolte **KMeans**a sadu hello *počtu clusterů, které* too4.</span><span class="sxs-lookup"><span data-stu-id="69034-291">Then go back toohello **Cluster** tab, choose **KMeans**, and set hello *Number of clusters* too4.</span></span> <span data-ttu-id="69034-292">Potom **provést**.</span><span class="sxs-lookup"><span data-stu-id="69034-292">Then **Execute**.</span></span> <span data-ttu-id="69034-293">v okně výstupu hello se nezobrazí výsledky Hello.</span><span class="sxs-lookup"><span data-stu-id="69034-293">hello results are displayed in hello output window.</span></span> <span data-ttu-id="69034-294">Jeden cluster má vysoká frekvence "Jirka" a "hp" a je pravděpodobně legitimní firemní e-mail.</span><span class="sxs-lookup"><span data-stu-id="69034-294">One cluster has high frequency of "george" and "hp" and is probably a legitimate business email.</span></span>

<span data-ttu-id="69034-295">toobuild jednoduché rozhodovací strom strojové učení modelu:</span><span class="sxs-lookup"><span data-stu-id="69034-295">toobuild a simple decision tree machine learning model:</span></span>

* <span data-ttu-id="69034-296">Vyberte hello **modelu** kartě</span><span class="sxs-lookup"><span data-stu-id="69034-296">Select hello **Model** tab,</span></span>
* <span data-ttu-id="69034-297">Zvolte **stromu** jako hello **typu**.</span><span class="sxs-lookup"><span data-stu-id="69034-297">Choose **Tree** as hello **Type**.</span></span>
* <span data-ttu-id="69034-298">Vyberte **Execute** toodisplay hello stromu v textové podobě v hello výstup okna.</span><span class="sxs-lookup"><span data-stu-id="69034-298">Select **Execute** toodisplay hello tree in text form in hello output window.</span></span>
* <span data-ttu-id="69034-299">Vyberte hello **kreslení** tlačítko tooview grafické verze.</span><span class="sxs-lookup"><span data-stu-id="69034-299">Select hello **Draw** button tooview a graphical version.</span></span> <span data-ttu-id="69034-300">To vypadá velmi podobné stromu toohello jsme získali dříve pomocí *rpart*.</span><span class="sxs-lookup"><span data-stu-id="69034-300">This looks quite similar toohello tree we obtained earlier using *rpart*.</span></span>

<span data-ttu-id="69034-301">Jednou z funkcí hello dobrý Rattle je jeho možnost toorun metody několik strojové učení a rychle vyhodnotit jejich.</span><span class="sxs-lookup"><span data-stu-id="69034-301">One of hello nice features of Rattle is its ability toorun several machine learning methods and quickly evaluate them.</span></span> <span data-ttu-id="69034-302">Tady je postup hello:</span><span class="sxs-lookup"><span data-stu-id="69034-302">Here is hello procedure:</span></span>

* <span data-ttu-id="69034-303">Zvolte **všechny** pro hello **typu**.</span><span class="sxs-lookup"><span data-stu-id="69034-303">Choose **All** for hello **Type**.</span></span>
* <span data-ttu-id="69034-304">Vyberte **provést**.</span><span class="sxs-lookup"><span data-stu-id="69034-304">Select **Execute**.</span></span>
* <span data-ttu-id="69034-305">Po jejím dokončení můžete kliknout na všechny jedním **typ**, například **SVM**a zobrazit výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="69034-305">After it finishes you can click any single **Type**, like **SVM**, and view hello results.</span></span>
* <span data-ttu-id="69034-306">Také můžete porovnat výkon hello hello modely na ověření hello nastavit pomocí hello **Evaluate** kartě. Například hello **chyba matice** výběr uvádí hello nedorozuměním matice, celkový chyby a chyba zprůměrovanou třídy pro každý model hello ověření sadu.</span><span class="sxs-lookup"><span data-stu-id="69034-306">You can also compare hello performance of hello models on hello validation set using hello **Evaluate** tab. For example, hello **Error Matrix** selection shows you hello confusion matrix, overall error, and averaged class error for each model on hello validation set.</span></span>
* <span data-ttu-id="69034-307">Můžete také vykreslení křivek ROC, provádět analýzy velkých a malých písmen a dělat jiné typy modelu hodnocení.</span><span class="sxs-lookup"><span data-stu-id="69034-307">You can also plot ROC curves, perform sensitivity analysis, and do other types of model evaluations.</span></span>

<span data-ttu-id="69034-308">Jakmile budete hotovi, vytváření modelů, vyberte hello **protokolu** kartě kód hello R tooview spustit Rattle během relace.</span><span class="sxs-lookup"><span data-stu-id="69034-308">Once you're finished building models, select hello **Log** tab tooview hello R code run by Rattle during your session.</span></span> <span data-ttu-id="69034-309">Můžete vybrat hello **exportovat** toosave tlačítko ji.</span><span class="sxs-lookup"><span data-stu-id="69034-309">You can select hello **Export** button toosave it.</span></span>

> [!NOTE]
> <span data-ttu-id="69034-310">V aktuální verzi Rattle je chyba.</span><span class="sxs-lookup"><span data-stu-id="69034-310">There is a bug in current release of Rattle.</span></span> <span data-ttu-id="69034-311">toomodify hello skriptu nebo ho používat toorepeat vaše kroky později, je třeba vložit znak # před * exportovat tento protokol... * v textu hello hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="69034-311">toomodify hello script or use it toorepeat your steps later, you must insert a # character in front of *Export this log ... * in hello text of hello log.</span></span>
>
>

## <a name="postgresql--squirrel-sql"></a><span data-ttu-id="69034-312">PostgreSQL & Squirrel SQL</span><span class="sxs-lookup"><span data-stu-id="69034-312">PostgreSQL & Squirrel SQL</span></span>
<span data-ttu-id="69034-313">Hello DSVM se dodává s PostgreSQL nainstalována.</span><span class="sxs-lookup"><span data-stu-id="69034-313">hello DSVM comes with PostgreSQL installed.</span></span> <span data-ttu-id="69034-314">PostgreSQL je relační databáze sofistikované, open source.</span><span class="sxs-lookup"><span data-stu-id="69034-314">PostgreSQL is a sophisticated, open-source relational database.</span></span> <span data-ttu-id="69034-315">Tato část uvádí, jak tooload naše spamu datové sady do PostgreSQL a pak zadat dotaz.</span><span class="sxs-lookup"><span data-stu-id="69034-315">This section shows how tooload our spam dataset into PostgreSQL and then query it.</span></span>

<span data-ttu-id="69034-316">Před načtením dat hello, je nutné tooallow ověřování hesla z hello localhost.</span><span class="sxs-lookup"><span data-stu-id="69034-316">Before you can load hello data, you need tooallow password authentication from hello localhost.</span></span> <span data-ttu-id="69034-317">Na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="69034-317">At a command prompt:</span></span>

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

<span data-ttu-id="69034-318">Téměř hello dolní části hello konfiguračním souboru se několik řádků, které podrobností hello povoleno připojení:</span><span class="sxs-lookup"><span data-stu-id="69034-318">Near hello bottom of hello config file are several lines that detail hello allowed connections:</span></span>

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

<span data-ttu-id="69034-319">Změna hello "IPv4 místní připojení" řádku toouse md5 místo ident, takže jsme umožní přihlásit se pomocí uživatelského jména a hesla:</span><span class="sxs-lookup"><span data-stu-id="69034-319">Change hello "IPv4 local connections" line toouse md5 instead of ident, so we can log in using a username and password:</span></span>

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

<span data-ttu-id="69034-320">A restartujte službu postgres hello:</span><span class="sxs-lookup"><span data-stu-id="69034-320">And restart hello postgres service:</span></span>

    sudo systemctl restart postgresql

<span data-ttu-id="69034-321">toolaunch psql, interaktivní terminálu PostgreSQL jako uživatel předdefinované postgres hello, spusťte následující příkaz z řádku hello:</span><span class="sxs-lookup"><span data-stu-id="69034-321">toolaunch psql, an interactive terminal for PostgreSQL, as hello built-in postgres user, run hello following command from a prompt:</span></span>

    sudo -u postgres psql

<span data-ttu-id="69034-322">Vytvořit nový uživatelský účet, pomocí hello stejné uživatelské jméno jako hello Linux účet, který jste aktuálně přihlášeni jako a zadejte pro něj heslo:</span><span class="sxs-lookup"><span data-stu-id="69034-322">Create a new user account, using hello same username as hello Linux account you're currently logged in as, and give it a password:</span></span>

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

<span data-ttu-id="69034-323">Znovu přihlásili toopsql jako vaše uživatele:</span><span class="sxs-lookup"><span data-stu-id="69034-323">Then log in toopsql as your user:</span></span>

    psql

<span data-ttu-id="69034-324">A importovat hello data do nové databáze:</span><span class="sxs-lookup"><span data-stu-id="69034-324">And import hello data into a new database:</span></span>

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

<span data-ttu-id="69034-325">Teď umožňuje procházet hello data a spustit některé dotazy pomocí **Squirrel SQL**, grafický nástroj, který umožňuje pracovat s databází prostřednictvím ovladač JDBC.</span><span class="sxs-lookup"><span data-stu-id="69034-325">Now, let's explore hello data and run some queries using **Squirrel SQL**, a graphical tool that lets you interact with databases via a JDBC driver.</span></span>

<span data-ttu-id="69034-326">tooget spuštěna, spusťte Squirrel SQL z nabídky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="69034-326">tooget started, launch Squirrel SQL from hello Applications menu.</span></span> <span data-ttu-id="69034-327">tooset až hello ovladače:</span><span class="sxs-lookup"><span data-stu-id="69034-327">tooset up hello driver:</span></span>

* <span data-ttu-id="69034-328">Vyberte **Windows**, pak **zobrazit ovladače**.</span><span class="sxs-lookup"><span data-stu-id="69034-328">Select **Windows**, then **View Drivers**.</span></span>
* <span data-ttu-id="69034-329">Klikněte pravým tlačítkem na **PostgreSQL** a vyberte **upravit ovladač**.</span><span class="sxs-lookup"><span data-stu-id="69034-329">Right-click on **PostgreSQL** and select **Modify Driver**.</span></span>
* <span data-ttu-id="69034-330">Vyberte **velmi třídy cesta**, pak **přidat**.</span><span class="sxs-lookup"><span data-stu-id="69034-330">Select **Extra Class Path**, then **Add**.</span></span>
* <span data-ttu-id="69034-331">Zadejte ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** pro hello **název souboru** a</span><span class="sxs-lookup"><span data-stu-id="69034-331">Enter ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** for hello **File Name** and</span></span>
* <span data-ttu-id="69034-332">Vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="69034-332">Select **Open**.</span></span>
* <span data-ttu-id="69034-333">Vyberte ovladače seznamu a pak vyberte **org.postgresql.Driver** v **název třídy**a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="69034-333">Choose List Drivers, then select **org.postgresql.Driver** in **Class Name**, and select **OK**.</span></span>

<span data-ttu-id="69034-334">tooset hello připojení toohello místní server:</span><span class="sxs-lookup"><span data-stu-id="69034-334">tooset up hello connection toohello local server:</span></span>

* <span data-ttu-id="69034-335">Vyberte **Windows**, pak **zobrazit aliasy.**</span><span class="sxs-lookup"><span data-stu-id="69034-335">Select **Windows**, then **View Aliases.**</span></span>
* <span data-ttu-id="69034-336">Zvolte hello  **+**  toomake tlačítko Nový alias.</span><span class="sxs-lookup"><span data-stu-id="69034-336">Choose hello **+** button toomake a new alias.</span></span>
* <span data-ttu-id="69034-337">Pojmenujte ji *nevyžádané pošty databáze*, zvolte **PostgreSQL** v hello **ovladač** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="69034-337">Name it *Spam database*, choose **PostgreSQL** in hello **Driver** drop-down.</span></span>
* <span data-ttu-id="69034-338">Nastavení adresy URL hello příliš*jdbc:postgresql://localhost/spam*.</span><span class="sxs-lookup"><span data-stu-id="69034-338">Set hello URL too*jdbc:postgresql://localhost/spam*.</span></span>
* <span data-ttu-id="69034-339">Zadejte vaše *uživatelské jméno* a *heslo*.</span><span class="sxs-lookup"><span data-stu-id="69034-339">Enter your *username* and *password*.</span></span>
* <span data-ttu-id="69034-340">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="69034-340">Click **OK**.</span></span>
* <span data-ttu-id="69034-341">tooopen hello **připojení** okna, klikněte dvakrát na hello ***nevyžádané pošty databáze*** alias.</span><span class="sxs-lookup"><span data-stu-id="69034-341">tooopen hello **Connection** window, double-click hello ***Spam database*** alias.</span></span>
* <span data-ttu-id="69034-342">Vyberte **Connect** (Připojit).</span><span class="sxs-lookup"><span data-stu-id="69034-342">Select **Connect**.</span></span>

<span data-ttu-id="69034-343">toorun některé dotazy:</span><span class="sxs-lookup"><span data-stu-id="69034-343">toorun some queries:</span></span>

* <span data-ttu-id="69034-344">Vyberte hello **SQL** kartě.</span><span class="sxs-lookup"><span data-stu-id="69034-344">Select hello **SQL** tab.</span></span>
* <span data-ttu-id="69034-345">Například zadejte jednoduchý dotaz `SELECT * from data;` v textovém poli dotazu hello hello horní části karty SQL hello.</span><span class="sxs-lookup"><span data-stu-id="69034-345">Enter a simple query such as `SELECT * from data;` in hello query textbox at hello top of hello SQL tab.</span></span>
* <span data-ttu-id="69034-346">Stiskněte klávesu **zadejte Ctrl** toorun ho.</span><span class="sxs-lookup"><span data-stu-id="69034-346">Press **Ctrl-Enter** toorun it.</span></span> <span data-ttu-id="69034-347">Ve výchozím nastavení Squirrel SQL vrátí hello prvních 100 řádků z dotazu.</span><span class="sxs-lookup"><span data-stu-id="69034-347">By default Squirrel SQL returns hello first 100 rows from your query.</span></span>

<span data-ttu-id="69034-348">Nejsou k dispozici mnoho další dotazy, že můžete spustit tooexplore tato data.</span><span class="sxs-lookup"><span data-stu-id="69034-348">There are many more queries you could run tooexplore this data.</span></span> <span data-ttu-id="69034-349">Například jak funguje hello frekvence hello slov *zkontrolujte* liší nevyžádané pošty se šunkou?</span><span class="sxs-lookup"><span data-stu-id="69034-349">For example, how does hello frequency of hello word *make* differ between spam and ham?</span></span>

    SELECT avg(word_freq_make), spam from data group by spam;

<span data-ttu-id="69034-350">Nebo jaké jsou vlastnosti hello e-mailů, které často obsahují *3d*?</span><span class="sxs-lookup"><span data-stu-id="69034-350">Or what are hello characteristics of email that frequently contain *3d*?</span></span>

    SELECT * from data order by word_freq_3d desc;

<span data-ttu-id="69034-351">Většina e-mailů, které mají vysokou výskytem *3d* jsou zjevně nevyžádané pošty, tak může být užitečná pro tvorbu prediktivního modelu tooclassify hello e-mailů.</span><span class="sxs-lookup"><span data-stu-id="69034-351">Most emails that have a high occurrence of *3d* are apparently spam, so it could be a useful feature for building a predictive model tooclassify hello emails.</span></span>

<span data-ttu-id="69034-352">Pokud jste chtěli tooperform machine learning s daty uloženými v databázi PostgreSQL, zvažte použití [MADlib](http://madlib.incubator.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="69034-352">If you wanted tooperform machine learning with data stored in a PostgreSQL database, consider using [MADlib](http://madlib.incubator.apache.org/).</span></span>

## <a name="sql-server-data-warehouse"></a><span data-ttu-id="69034-353">SQL Server datového skladu</span><span class="sxs-lookup"><span data-stu-id="69034-353">SQL Server Data Warehouse</span></span>
<span data-ttu-id="69034-354">Azure SQL Data Warehouse je cloudová, škálovatelná databáze, která dokáže zpracovávat ohromné objemy dat, relačních i nerelačních.</span><span class="sxs-lookup"><span data-stu-id="69034-354">Azure SQL Data Warehouse is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span> <span data-ttu-id="69034-355">Další informace najdete v tématu [co je Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="69034-355">For more information, see [What is Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)</span></span>

<span data-ttu-id="69034-356">tooconnect toohello datového skladu a vytvořte hello tabulky hello spusťte následující příkaz z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="69034-356">tooconnect toohello data warehouse and create hello table, run hello following command from a command prompt:</span></span>

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

<span data-ttu-id="69034-357">Pak v příkazovém řádku sqlcmd hello:</span><span class="sxs-lookup"><span data-stu-id="69034-357">Then at hello sqlcmd prompt:</span></span>

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

<span data-ttu-id="69034-358">Kopírování dat pomocí bcp:</span><span class="sxs-lookup"><span data-stu-id="69034-358">Copy data with bcp:</span></span>

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> <span data-ttu-id="69034-359">konce řádků Hello ve staženém souboru hello jsou styl systému Windows, ale bcp očekává stylu systému UNIX, takže potřebujeme tootell bcp, která s hello příznak - r.</span><span class="sxs-lookup"><span data-stu-id="69034-359">hello line endings in hello downloaded file are Windows-style, but bcp expects UNIX-style, so we need tootell bcp that with hello -r flag.</span></span>
>
>

<span data-ttu-id="69034-360">A dotazování pomocí sqlcmd:</span><span class="sxs-lookup"><span data-stu-id="69034-360">And query with sqlcmd:</span></span>

    select top 10 spam, char_freq_dollar from spam;
    GO

<span data-ttu-id="69034-361">Také můžete dotazovat pomocí Squirrel SQL.</span><span class="sxs-lookup"><span data-stu-id="69034-361">You could also query with Squirrel SQL.</span></span> <span data-ttu-id="69034-362">Podobný postup PostgreSQL, hello ovladač JDBC MSSQL serveru Microsoft, pomocí kterého lze nalézt v ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span><span class="sxs-lookup"><span data-stu-id="69034-362">Follow similar steps for PostgreSQL, using hello Microsoft MSSQL Server JDBC Driver, which can be found in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69034-363">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69034-363">Next steps</span></span>
<span data-ttu-id="69034-364">Přehled témata, která vás provede procesem hello úlohy, které tvoří hello procesu vědecké zpracování dat v Azure najdete v tématu [proces vědecké účely dat Team](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="69034-364">For an overview of topics that walk you through hello tasks that comprise hello Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="69034-365">Popis dalších návody začátku do konce, které ukazují postup hello v hello proces vědecké účely Team dat u konkrétních scénářů najdete v tématu [proces vědecké účely dat Team návody](data-science-process-walkthroughs.md).</span><span class="sxs-lookup"><span data-stu-id="69034-365">For a description of other end-to-end walkthroughs that demonstrate hello steps in hello Team Data Science Process for specific scenarios, see [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md).</span></span> <span data-ttu-id="69034-366">návody Hello také ilustrují, jak toocombine cloudové a místní nástrojů a služeb do pracovního postupu nebo kanálu toocreate inteligentního aplikace.</span><span class="sxs-lookup"><span data-stu-id="69034-366">hello walkthroughs also illustrate how toocombine cloud and on-premises tools and services into a workflow or pipeline toocreate an intelligent application.</span></span>
