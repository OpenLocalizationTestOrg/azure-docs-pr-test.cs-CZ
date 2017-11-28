---
title: "Další informace pomocí izolovaném prostoru – emulátor – Azure HDInsight Hadoop | Microsoft Docs"
description: "Pokud chcete spustit, získávání informací o použití ekosystému Hadoop, nastavením izolovaného prostoru Hadoop z Hortonworks na virtuální počítač Azure. "
keywords: "hadoop emulátoru, izolovaného prostoru hadoop"
editor: cgronlun
manager: jhubbard
services: hdinsight
author: nitinme
documentationcenter: 
tags: azure-portal
ms.assetid: 6ad5bb58-8215-4e3d-a07f-07fcd8839cc6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: b701879464205860edd1c097651b532f87bae388
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a><span data-ttu-id="18494-104">Začínáme s Hadoop izolovaném prostoru, emulátoru na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="18494-104">Get started with a Hadoop sandbox, an emulator on a virtual machine</span></span>

<span data-ttu-id="18494-105">Naučte se instalovat izolovaného prostoru Hadoop z Hortonworks na virtuálním počítači, další informace o ekosystému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="18494-105">Learn how to install the Hadoop sandbox from Hortonworks on a virtual machine to learn about the Hadoop ecosystem.</span></span> <span data-ttu-id="18494-106">Izolovaný prostor poskytuje prostředí pro místní vývoj Další informace o Hadoop, Hadoop Distributed File System (HDFS) a odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="18494-106">The sandbox provides a local development environment to learn about Hadoop, Hadoop Distributed File System (HDFS), and job submission.</span></span> <span data-ttu-id="18494-107">Jakmile se seznámíte s Hadoop, můžete začít používat Hadoop v Azure vytvořením clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="18494-107">Once you are familiar with Hadoop, you can start using Hadoop on Azure by creating an HDInsight cluster.</span></span> <span data-ttu-id="18494-108">Další informace o tom, jak začít, najdete v části [Začínáme s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="18494-108">For more information on how to get started, see [Get started with Hadoop on HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18494-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="18494-109">Prerequisites</span></span>
* <span data-ttu-id="18494-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span><span class="sxs-lookup"><span data-stu-id="18494-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span></span> <span data-ttu-id="18494-111">Stáhněte a nainstalujte ji z [zde](https://www.virtualbox.org/wiki/Downloads).</span><span class="sxs-lookup"><span data-stu-id="18494-111">Download and install it from [here](https://www.virtualbox.org/wiki/Downloads).</span></span>



## <a name="download-and-install-the-virtual-machine"></a><span data-ttu-id="18494-112">Stáhněte a nainstalujte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="18494-112">Download and install the virtual machine</span></span>
1. <span data-ttu-id="18494-113">Vyhledejte [Hortonworks stáhne](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="18494-113">Browse to the [Hortonworks downloads](http://hortonworks.com/downloads/#sandbox).</span></span>

2. <span data-ttu-id="18494-114">Klikněte na tlačítko **stáhnout VIRTUALBOX** stáhnout nejnovější Hortonworks karanténě na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="18494-114">Click **DOWNLOAD FOR VIRTUALBOX** to download the latest Hortonworks Sandbox on a VM.</span></span> <span data-ttu-id="18494-115">Zobrazí se výzva k registraci s Hortonworks před zahájením stahování.</span><span class="sxs-lookup"><span data-stu-id="18494-115">You are prompted to register with Hortonworks before the download begins.</span></span> <span data-ttu-id="18494-116">Bude trvat jednu až dvě hodiny ke stažení v závislosti na rychlosti sítě.</span><span class="sxs-lookup"><span data-stu-id="18494-116">It takes one to two hours to download depending on your network speed.</span></span>
   
    ![Propojit bitovou kopii pro stažení Hortonworks karanténě pro VirtualBox](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. <span data-ttu-id="18494-118">Stejné webové stránce, klikněte **Import na virtuální,** odkaz ke stažení PDF, který obsahuje pokyny k instalaci pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="18494-118">From the same web page, click the **Import on Virtual Box** link to download a PDF containing installation instructions for the virtual machine.</span></span>

<span data-ttu-id="18494-119">Chcete-li stáhnout izolovaného prostoru starší verzi softwaru HDP, rozbalte archivu:</span><span class="sxs-lookup"><span data-stu-id="18494-119">To download an older HDP version sandbox, expand the archive:</span></span>

![Hortonworks karanténě archivu](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-the-virtual-machine"></a><span data-ttu-id="18494-121">Spuštění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="18494-121">Start the virtual machine</span></span>

1. <span data-ttu-id="18494-122">Otevřete VirtualBox Oracle virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="18494-122">Open Oracle VM VirtualBox.</span></span>
2. <span data-ttu-id="18494-123">Z **soubor** nabídky, klikněte na tlačítko **Import zařízení**a pak zadejte bitovou kopii Hortonworks karanténě.</span><span class="sxs-lookup"><span data-stu-id="18494-123">From the **File** menu, click **Import Appliance**, and then specify the Hortonworks Sandbox image.</span></span>
1. <span data-ttu-id="18494-124">Vyberte Hortonworks karanténě, klikněte na **spustit**a potom **normální spuštění**.</span><span class="sxs-lookup"><span data-stu-id="18494-124">Select the Hortonworks Sandbox, click **Start**, and then **Normal Start**.</span></span> <span data-ttu-id="18494-125">Po dokončení procesu spouštění virtuálního počítače zobrazí pokyny přihlášení.</span><span class="sxs-lookup"><span data-stu-id="18494-125">Once the virtual machine has finished the boot process, it displays login instructions.</span></span>
   
    ![Normální spuštění](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. <span data-ttu-id="18494-127">Otevřete webový prohlížeč a přejděte na adresu URL zobrazené (obvykle http://127.0.0.1:8888).</span><span class="sxs-lookup"><span data-stu-id="18494-127">Open a web browser and navigate to the URL displayed (usually http://127.0.0.1:8888).</span></span>

## <a name="set-sandbox-passwords"></a><span data-ttu-id="18494-128">Nastavit heslo izolovaného prostoru</span><span class="sxs-lookup"><span data-stu-id="18494-128">Set Sandbox passwords</span></span>

1. <span data-ttu-id="18494-129">Z **Začínáme** krok Hortonworks karanténě stránky, vyberte **pokročilé možnosti zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="18494-129">From the **get started** step of the Hortonworks Sandbox page, select **View Advanced Options**.</span></span> <span data-ttu-id="18494-130">Pomocí informací na této stránce pro přihlášení do izolovaného prostoru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="18494-130">Use the information on this page to log in to the sandbox using SSH.</span></span> <span data-ttu-id="18494-131">Pomocí zadaného názvu a hesla.</span><span class="sxs-lookup"><span data-stu-id="18494-131">Use the name and password provided.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="18494-132">Pokud nemáte nainstalovaného klienta SSH, můžete použít webové SSH, uvedených v pro virtuální počítač na **http://localhost:4200 /**.</span><span class="sxs-lookup"><span data-stu-id="18494-132">If you do not have an SSH client installed, you can use the web-based SSH provided at by the virtual machine at **http://localhost:4200/**.</span></span>
   > 
   
    <span data-ttu-id="18494-133">Při prvním připojení pomocí protokolu SSH, budete vyzváni ke změně hesla pro kořenový účet.</span><span class="sxs-lookup"><span data-stu-id="18494-133">The first time you connect using SSH, you are prompted to change the password for the root account.</span></span> <span data-ttu-id="18494-134">Zadejte nové heslo, které můžete použít při přihlášení pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="18494-134">Enter a new password, which you use when you log in using SSH.</span></span>

2. <span data-ttu-id="18494-135">Po přihlášení, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="18494-135">Once logged in, enter the following command:</span></span>
   
        ambari-admin-password-reset
   
    <span data-ttu-id="18494-136">Po zobrazení výzvy zadejte heslo pro účet správce Ambari.</span><span class="sxs-lookup"><span data-stu-id="18494-136">When prompted, provide a password for the Ambari admin account.</span></span> <span data-ttu-id="18494-137">Používá se při přístupu k webové uživatelské rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="18494-137">This is used when you access the Ambari Web UI.</span></span>

## <a name="use-hive-commands"></a><span data-ttu-id="18494-138">Použití Hive příkazů</span><span class="sxs-lookup"><span data-stu-id="18494-138">Use Hive commands</span></span>

1. <span data-ttu-id="18494-139">Z připojení SSH k izolovanému prostoru použijte následující příkaz spusťte prostředí Hive:</span><span class="sxs-lookup"><span data-stu-id="18494-139">From an SSH connection to the sandbox, use the following command to start the Hive shell:</span></span>
   
        hive
2. <span data-ttu-id="18494-140">Po zahájení prostředí, použijte následující postupy k zobrazení tabulky, které jsou k dispozici s izolovaný prostor:</span><span class="sxs-lookup"><span data-stu-id="18494-140">Once the shell has started, use the following to view the tables that are provided with the sandbox:</span></span>
   
        show tables;
3. <span data-ttu-id="18494-141">K načtení 10 řádků z použijte následující postupy `sample_07` tabulce:</span><span class="sxs-lookup"><span data-stu-id="18494-141">Use the following to retrieve 10 rows from the `sample_07` table:</span></span>
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a><span data-ttu-id="18494-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18494-142">Next steps</span></span>
* [<span data-ttu-id="18494-143">Další informace o použití sady Visual Studio s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="18494-143">Learn how to use Visual Studio with the Hortonworks Sandbox</span></span>](hdinsight-hadoop-emulator-visual-studio.md)
* [<span data-ttu-id="18494-144">Učení LAN Hortonworks izolovaného prostoru</span><span class="sxs-lookup"><span data-stu-id="18494-144">Learning the ropes of the Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="18494-145">Hadoop kurz – Začínáme s HDP</span><span class="sxs-lookup"><span data-stu-id="18494-145">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

