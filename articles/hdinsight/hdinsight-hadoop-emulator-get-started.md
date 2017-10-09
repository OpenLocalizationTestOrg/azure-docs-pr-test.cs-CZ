---
title: "aaaLearn pomocí izolovaném prostoru – emulátor – Azure HDInsight Hadoop | Microsoft Docs"
description: "získávání informací o použití toostart hello ekosystém Hadoop, můžete nastavit izolovaného prostoru Hadoop z Hortonworks na virtuální počítač Azure. "
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
ms.openlocfilehash: 91e74f0823fd02e9bb812155a7d09357a77b0736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a><span data-ttu-id="398bd-104">Začínáme s Hadoop izolovaném prostoru, emulátoru na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="398bd-104">Get started with a Hadoop sandbox, an emulator on a virtual machine</span></span>

<span data-ttu-id="398bd-105">Zjistěte, jak tooinstall hello izolovaného prostoru Hadoop z Hortonworks na virtuální počítač toolearn o ekosystém Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="398bd-105">Learn how tooinstall hello Hadoop sandbox from Hortonworks on a virtual machine toolearn about hello Hadoop ecosystem.</span></span> <span data-ttu-id="398bd-106">Hello izolovaného prostoru obsahuje místním vývojovém prostředí toolearn o Hadoop, Hadoop Distributed File System (HDFS) a odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="398bd-106">hello sandbox provides a local development environment toolearn about Hadoop, Hadoop Distributed File System (HDFS), and job submission.</span></span> <span data-ttu-id="398bd-107">Jakmile se seznámíte s Hadoop, můžete začít používat Hadoop v Azure vytvořením clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="398bd-107">Once you are familiar with Hadoop, you can start using Hadoop on Azure by creating an HDInsight cluster.</span></span> <span data-ttu-id="398bd-108">Další informace o tom, jak spustit tooget najdete v tématu [Začínáme s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="398bd-108">For more information on how tooget started, see [Get started with Hadoop on HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="398bd-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="398bd-109">Prerequisites</span></span>
* <span data-ttu-id="398bd-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span><span class="sxs-lookup"><span data-stu-id="398bd-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span></span> <span data-ttu-id="398bd-111">Stáhněte a nainstalujte ji z [zde](https://www.virtualbox.org/wiki/Downloads).</span><span class="sxs-lookup"><span data-stu-id="398bd-111">Download and install it from [here](https://www.virtualbox.org/wiki/Downloads).</span></span>



## <a name="download-and-install-hello-virtual-machine"></a><span data-ttu-id="398bd-112">Stáhněte a nainstalujte hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="398bd-112">Download and install hello virtual machine</span></span>
1. <span data-ttu-id="398bd-113">Procházet toohello [Hortonworks stáhne](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="398bd-113">Browse toohello [Hortonworks downloads](http://hortonworks.com/downloads/#sandbox).</span></span>

2. <span data-ttu-id="398bd-114">Klikněte na tlačítko **stáhnout VIRTUALBOX** toodownload hello nejnovější Hortonworks karanténě na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="398bd-114">Click **DOWNLOAD FOR VIRTUALBOX** toodownload hello latest Hortonworks Sandbox on a VM.</span></span> <span data-ttu-id="398bd-115">Před zahájením hello stažení jste výzvami tooregister s Hortonworks.</span><span class="sxs-lookup"><span data-stu-id="398bd-115">You are prompted tooregister with Hortonworks before hello download begins.</span></span> <span data-ttu-id="398bd-116">Jak dlouho trvá jeden toodownload tootwo hodin v závislosti na rychlosti sítě.</span><span class="sxs-lookup"><span data-stu-id="398bd-116">It takes one tootwo hours toodownload depending on your network speed.</span></span>
   
    ![Propojit bitovou kopii pro stažení Hortonworks karanténě pro VirtualBox](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. <span data-ttu-id="398bd-118">Z hello stejné webové stránce, klikněte na tlačítko hello **Import na virtuální,** odkaz toodownload PDF, který obsahuje pokyny k instalaci pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="398bd-118">From hello same web page, click hello **Import on Virtual Box** link toodownload a PDF containing installation instructions for hello virtual machine.</span></span>

<span data-ttu-id="398bd-119">toodownload starší HDP verze izolovaného, rozbalte archivu hello:</span><span class="sxs-lookup"><span data-stu-id="398bd-119">toodownload an older HDP version sandbox, expand hello archive:</span></span>

![Hortonworks karanténě archivu](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-hello-virtual-machine"></a><span data-ttu-id="398bd-121">Spustit virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="398bd-121">Start hello virtual machine</span></span>

1. <span data-ttu-id="398bd-122">Otevřete VirtualBox Oracle virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="398bd-122">Open Oracle VM VirtualBox.</span></span>
2. <span data-ttu-id="398bd-123">Z hello **soubor** nabídky, klikněte na tlačítko **Import zařízení**a pak zadejte hello Hortonworks karanténě image.</span><span class="sxs-lookup"><span data-stu-id="398bd-123">From hello **File** menu, click **Import Appliance**, and then specify hello Hortonworks Sandbox image.</span></span>
1. <span data-ttu-id="398bd-124">Klikněte na tlačítko vyberte hello Hortonworks karanténě **spustit**a potom **normální spuštění**.</span><span class="sxs-lookup"><span data-stu-id="398bd-124">Select hello Hortonworks Sandbox, click **Start**, and then **Normal Start**.</span></span> <span data-ttu-id="398bd-125">Po dokončení procesu spouštění hello hello virtuální počítač, zobrazí pokyny přihlášení.</span><span class="sxs-lookup"><span data-stu-id="398bd-125">Once hello virtual machine has finished hello boot process, it displays login instructions.</span></span>
   
    ![Normální spuštění](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. <span data-ttu-id="398bd-127">Otevřete webový prohlížeč a přejděte toohello adresu URL zobrazené (obvykle http://127.0.0.1:8888).</span><span class="sxs-lookup"><span data-stu-id="398bd-127">Open a web browser and navigate toohello URL displayed (usually http://127.0.0.1:8888).</span></span>

## <a name="set-sandbox-passwords"></a><span data-ttu-id="398bd-128">Nastavit heslo izolovaného prostoru</span><span class="sxs-lookup"><span data-stu-id="398bd-128">Set Sandbox passwords</span></span>

1. <span data-ttu-id="398bd-129">Z hello **Začínáme** krok hello Hortonworks karanténě stránky, vyberte **pokročilé možnosti zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="398bd-129">From hello **get started** step of hello Hortonworks Sandbox page, select **View Advanced Options**.</span></span> <span data-ttu-id="398bd-130">Použijte hello informace na této stránce toolog v izolovaném prostoru toohello pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="398bd-130">Use hello information on this page toolog in toohello sandbox using SSH.</span></span> <span data-ttu-id="398bd-131">Použijte název hello a zadat heslo.</span><span class="sxs-lookup"><span data-stu-id="398bd-131">Use hello name and password provided.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="398bd-132">Pokud nemáte nainstalovaného klienta SSH, můžete použít hello webové SSH uvedených v hello virtuální počítač na **http://localhost:4200 /**.</span><span class="sxs-lookup"><span data-stu-id="398bd-132">If you do not have an SSH client installed, you can use hello web-based SSH provided at by hello virtual machine at **http://localhost:4200/**.</span></span>
   > 
   
    <span data-ttu-id="398bd-133">Hello poprvé, ke kterým se připojujete pomocí protokolu SSH, jste výzvami toochange hello heslo pro hello kořenového účtu.</span><span class="sxs-lookup"><span data-stu-id="398bd-133">hello first time you connect using SSH, you are prompted toochange hello password for hello root account.</span></span> <span data-ttu-id="398bd-134">Zadejte nové heslo, které můžete použít při přihlášení pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="398bd-134">Enter a new password, which you use when you log in using SSH.</span></span>

2. <span data-ttu-id="398bd-135">Po přihlášení, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="398bd-135">Once logged in, enter hello following command:</span></span>
   
        ambari-admin-password-reset
   
    <span data-ttu-id="398bd-136">Po zobrazení výzvy zadejte heslo pro účet správce Ambari hello.</span><span class="sxs-lookup"><span data-stu-id="398bd-136">When prompted, provide a password for hello Ambari admin account.</span></span> <span data-ttu-id="398bd-137">Používá se při přístupu k hello webové uživatelské rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="398bd-137">This is used when you access hello Ambari Web UI.</span></span>

## <a name="use-hive-commands"></a><span data-ttu-id="398bd-138">Použití Hive příkazů</span><span class="sxs-lookup"><span data-stu-id="398bd-138">Use Hive commands</span></span>

1. <span data-ttu-id="398bd-139">Ze SSH připojení toohello karantény použijte následující příkaz toostart hello Hive prostředí hello:</span><span class="sxs-lookup"><span data-stu-id="398bd-139">From an SSH connection toohello sandbox, use hello following command toostart hello Hive shell:</span></span>
   
        hive
2. <span data-ttu-id="398bd-140">Po zahájení hello prostředí, použijte hello následující tooview hello tabulek, které jsou k dispozici s hello izolovaného prostoru:</span><span class="sxs-lookup"><span data-stu-id="398bd-140">Once hello shell has started, use hello following tooview hello tables that are provided with hello sandbox:</span></span>
   
        show tables;
3. <span data-ttu-id="398bd-141">Použití hello následujících tooretrieve 10 řádků z hello `sample_07` tabulky:</span><span class="sxs-lookup"><span data-stu-id="398bd-141">Use hello following tooretrieve 10 rows from hello `sample_07` table:</span></span>
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a><span data-ttu-id="398bd-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="398bd-142">Next steps</span></span>
* [<span data-ttu-id="398bd-143">Zjistěte, jak toouse Visual Studio s hello Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="398bd-143">Learn how toouse Visual Studio with hello Hortonworks Sandbox</span></span>](hdinsight-hadoop-emulator-visual-studio.md)
* [<span data-ttu-id="398bd-144">Learning hello LAN Dobrý den Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="398bd-144">Learning hello ropes of hello Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="398bd-145">Hadoop kurz – Začínáme s HDP</span><span class="sxs-lookup"><span data-stu-id="398bd-145">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

