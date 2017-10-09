---
title: aaaUse SSH s Hadoop - Azure HDInsight | Microsoft Docs
description: "Pro přístup k systému HDInsight můžete použít Secure Shell (SSH). Tento dokument obsahuje informace o připojení z klientů Windows, Linux, Unix nebo systému macOS tooHDInsight pomocí hello ssh a scp příkazy."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "příkazy hadoop v linuxu, příkazy hadoop linux, hadoop macos, ssh hadoop, ssh hadoop cluster"
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: ac9e70ce3c70693c1b81c9514ba4fd47686070ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toohdinsight-hadoop-using-ssh"></a><span data-ttu-id="82f2d-105">Připojit tooHDInsight (Hadoop) pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="82f2d-105">Connect tooHDInsight (Hadoop) using SSH</span></span>

<span data-ttu-id="82f2d-106">Zjistěte, jak toouse [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) toosecurely připojit tooHadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82f2d-106">Learn how toouse [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) toosecurely connect tooHadoop on Azure HDInsight.</span></span> 

<span data-ttu-id="82f2d-107">HDInsight můžete použít Linux (Ubuntu) jako hello operační systém pro uzly v clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-107">HDInsight can use Linux (Ubuntu) as hello operating system for nodes within hello Hadoop cluster.</span></span> <span data-ttu-id="82f2d-108">Hello následující tabulka obsahuje hello adresu a port informace potřebné pro připojení na základě tooLinux HDInsight pomocí SSH klienta:</span><span class="sxs-lookup"><span data-stu-id="82f2d-108">hello following table contains hello address and port information needed when connecting tooLinux-based HDInsight using an SSH client:</span></span>

| <span data-ttu-id="82f2d-109">Adresa</span><span class="sxs-lookup"><span data-stu-id="82f2d-109">Address</span></span> | <span data-ttu-id="82f2d-110">Port</span><span class="sxs-lookup"><span data-stu-id="82f2d-110">Port</span></span> | <span data-ttu-id="82f2d-111">Připojení...</span><span class="sxs-lookup"><span data-stu-id="82f2d-111">Connects to...</span></span> |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | <span data-ttu-id="82f2d-112">22</span><span class="sxs-lookup"><span data-stu-id="82f2d-112">22</span></span> | <span data-ttu-id="82f2d-113">Hraniční uzel (R Server v HDInsightu)</span><span class="sxs-lookup"><span data-stu-id="82f2d-113">Edge node (R Server on HDInsight)</span></span> |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="82f2d-114">22</span><span class="sxs-lookup"><span data-stu-id="82f2d-114">22</span></span> | <span data-ttu-id="82f2d-115">Hraniční uzel (všechny ostatní typy clusteru, pokud hraniční uzel existuje)</span><span class="sxs-lookup"><span data-stu-id="82f2d-115">Edge node (any other cluster type, if an edge node exists)</span></span> |
| `<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="82f2d-116">22</span><span class="sxs-lookup"><span data-stu-id="82f2d-116">22</span></span> | <span data-ttu-id="82f2d-117">Primární hlavní uzel</span><span class="sxs-lookup"><span data-stu-id="82f2d-117">Primary headnode</span></span> |
| `<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="82f2d-118">23</span><span class="sxs-lookup"><span data-stu-id="82f2d-118">23</span></span> | <span data-ttu-id="82f2d-119">Sekundární hlavní uzel</span><span class="sxs-lookup"><span data-stu-id="82f2d-119">Secondary headnode</span></span> |

> [!NOTE]
> <span data-ttu-id="82f2d-120">Nahraďte `<edgenodename>` s názvem hello hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="82f2d-120">Replace `<edgenodename>` with hello name of hello edge node.</span></span>
>
> <span data-ttu-id="82f2d-121">Nahraďte `<clustername>` s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="82f2d-121">Replace `<clustername>` with hello name of your cluster.</span></span>
>
> <span data-ttu-id="82f2d-122">Pokud cluster obsahuje hraniční uzel, doporučujeme vám __toohello hraniční uzel se vždy připojují__ pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-122">If your cluster contains an edge node, we recommend that you __always connect toohello edge node__ using SSH.</span></span> <span data-ttu-id="82f2d-123">Hello hlavních uzlech hostitel služby, které jsou kritické toohello stavu systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="82f2d-123">hello head nodes host services that are critical toohello health of Hadoop.</span></span> <span data-ttu-id="82f2d-124">Hello hraniční uzel se spustí pouze co zadávat na něm.</span><span class="sxs-lookup"><span data-stu-id="82f2d-124">hello edge node runs only what you put on it.</span></span>
>
> <span data-ttu-id="82f2d-125">Další informace o použití hraničních uzlů najdete v tématu věnovaném [použití hraničních uzlů v HDInsightu](hdinsight-apps-use-edge-node.md#access-an-edge-node).</span><span class="sxs-lookup"><span data-stu-id="82f2d-125">For more information on using edge nodes, see [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node).</span></span>

## <a name="ssh-clients"></a><span data-ttu-id="82f2d-126">Klienti SSH</span><span class="sxs-lookup"><span data-stu-id="82f2d-126">SSH clients</span></span>

<span data-ttu-id="82f2d-127">Systémy Linux, Unix a systému macOS zadejte hello `ssh` a `scp` příkazy.</span><span class="sxs-lookup"><span data-stu-id="82f2d-127">Linux, Unix, and macOS systems provide hello `ssh` and `scp` commands.</span></span> <span data-ttu-id="82f2d-128">Hello `ssh` klienta je běžně používané toocreate vzdálenou relaci příkazového řádku systému Unix nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="82f2d-128">hello `ssh` client is commonly used toocreate a remote command-line session with a Linux or Unix-based system.</span></span> <span data-ttu-id="82f2d-129">Hello `scp` klienta je použité toosecurely kopírovat soubory mezi klientem a hello vzdáleného systému.</span><span class="sxs-lookup"><span data-stu-id="82f2d-129">hello `scp` client is used toosecurely copy files between your client and hello remote system.</span></span>

<span data-ttu-id="82f2d-130">Microsoft Windows ve výchozím nastavení neposkytuje žádné klienty SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-130">Microsoft Windows does not provide any SSH clients by default.</span></span> <span data-ttu-id="82f2d-131">Hello `ssh` a `scp` klienti jsou k dispozici pro systém Windows prostřednictvím hello následující balíčky:</span><span class="sxs-lookup"><span data-stu-id="82f2d-131">hello `ssh` and `scp` clients are available for Windows through hello following packages:</span></span>

* <span data-ttu-id="82f2d-132">[Prostředí Azure Cloud](../cloud-shell/quickstart.md): hello cloudové prostředí poskytuje prostředí Bash ve vašem prohlížeči a poskytuje hello `ssh`, `scp`a další běžné příkazy Linux.</span><span class="sxs-lookup"><span data-stu-id="82f2d-132">[Azure Cloud Shell](../cloud-shell/quickstart.md): hello Cloud Shell provides a Bash environment in your browser, and provides hello `ssh`, `scp`, and other common Linux commands.</span></span>

* <span data-ttu-id="82f2d-133">[Bash na Ubuntu na Windows 10](https://msdn.microsoft.com/commandline/wsl/about): hello `ssh` a `scp` jsou k dispozici prostřednictvím na příkazovém řádku Windows hello Bash příkazy.</span><span class="sxs-lookup"><span data-stu-id="82f2d-133">[Bash on Ubuntu on Windows 10](https://msdn.microsoft.com/commandline/wsl/about): hello `ssh` and `scp` commands are available through hello Bash on Windows command line.</span></span>

* <span data-ttu-id="82f2d-134">[Git (https://git-scm.com/)](https://git-scm.com/): hello `ssh` a `scp` jsou k dispozici prostřednictvím příkazového řádku hello Git Bash příkazy.</span><span class="sxs-lookup"><span data-stu-id="82f2d-134">[Git (https://git-scm.com/)](https://git-scm.com/): hello `ssh` and `scp` commands are available through hello GitBash command line.</span></span>

* <span data-ttu-id="82f2d-135">[Plocha GitHub (https://desktop.github.com/)](https://desktop.github.com/) hello `ssh` a `scp` příkazy jsou k dispozici prostřednictvím hello Githubu příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="82f2d-135">[GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/) hello `ssh` and `scp` commands are available through hello GitHub Shell command line.</span></span> <span data-ttu-id="82f2d-136">GitHub plochy může být nakonfigurované toouse Bash, hello příkazového řádku systému Windows nebo prostředí PowerShell jako hello příkazového řádku pro hello Git prostředí.</span><span class="sxs-lookup"><span data-stu-id="82f2d-136">GitHub Desktop can be configured toouse Bash, hello Windows Command Prompt, or PowerShell as hello command line for hello Git Shell.</span></span>

* <span data-ttu-id="82f2d-137">[OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): tým hello prostředí PowerShell je portování OpenSSH tooWindows a poskytuje zkušební verze.</span><span class="sxs-lookup"><span data-stu-id="82f2d-137">[OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): hello PowerShell team is porting OpenSSH tooWindows, and provides test releases.</span></span>

    > [!WARNING]
    > <span data-ttu-id="82f2d-138">Hello OpenSSH balíček obsahuje součást serveru hello SSH, `sshd`.</span><span class="sxs-lookup"><span data-stu-id="82f2d-138">hello OpenSSH package includes hello SSH server component, `sshd`.</span></span> <span data-ttu-id="82f2d-139">Tato součást spustí serverem SSH v systému, jiné povolení tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="82f2d-139">This component starts an SSH server on your system, allowing others tooconnect tooit.</span></span> <span data-ttu-id="82f2d-140">Tuto komponentu nakonfigurovat nebo otevřít port 22, pokud chcete, aby toohost serverem SSH v systému.</span><span class="sxs-lookup"><span data-stu-id="82f2d-140">Do not configure this component or open port 22 unless you want toohost an SSH server on your system.</span></span> <span data-ttu-id="82f2d-141">Není požadováno toocommunicate s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82f2d-141">It is not required toocommunicate with HDInsight.</span></span>

<span data-ttu-id="82f2d-142">Existuje také několik grafických klientů SSH, jako je [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) a [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/).</span><span class="sxs-lookup"><span data-stu-id="82f2d-142">There are also several graphical SSH clients, such as [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) and [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/).</span></span> <span data-ttu-id="82f2d-143">Tito klienti mohou být použité tooconnect tooHDInsight, hello proces připojení se liší od používání hello `ssh` nástroj.</span><span class="sxs-lookup"><span data-stu-id="82f2d-143">While these clients can be used tooconnect tooHDInsight, hello process of connecting is different than using hello `ssh` utility.</span></span> <span data-ttu-id="82f2d-144">Další informace najdete v tématu naleznete v dokumentaci hello hello grafický klient, který používáte.</span><span class="sxs-lookup"><span data-stu-id="82f2d-144">For more information, see hello documentation of hello graphical client you are using.</span></span>

## <span data-ttu-id="82f2d-145"><a id="sshkey"></a>Ověřování: Klíče SSH</span><span class="sxs-lookup"><span data-stu-id="82f2d-145"><a id="sshkey"></a>Authentication: SSH Keys</span></span>

<span data-ttu-id="82f2d-146">SSH klíče používat [Public key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) tooauthenticate relace SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-146">SSH keys use [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) tooauthenticate SSH sessions.</span></span> <span data-ttu-id="82f2d-147">Klíče SSH jsou bezpečnější než hesla a poskytnout clusteru Hadoop tooyour služby snadno toosecure přístup.</span><span class="sxs-lookup"><span data-stu-id="82f2d-147">SSH keys are more secure than passwords, and provide an easy way toosecure access tooyour Hadoop cluster.</span></span>

<span data-ttu-id="82f2d-148">Váš účet SSH je zabezpečena pomocí klíče, musí klient hello poskytnout hello odpovídající privátní klíč při připojení:</span><span class="sxs-lookup"><span data-stu-id="82f2d-148">If your SSH account is secured using a key, hello client must provide hello matching private key when you connect:</span></span>

* <span data-ttu-id="82f2d-149">Většina klienti mohou být nakonfigurované toouse __výchozí klíč__.</span><span class="sxs-lookup"><span data-stu-id="82f2d-149">Most clients can be configured toouse a __default key__.</span></span> <span data-ttu-id="82f2d-150">Například hello `ssh` hledá privátní klíč v `~/.ssh/id_rsa` v prostředích Linux a Unix.</span><span class="sxs-lookup"><span data-stu-id="82f2d-150">For example, hello `ssh` client looks for a private key at `~/.ssh/id_rsa` on Linux and Unix environments.</span></span>

* <span data-ttu-id="82f2d-151">Můžete zadat hello __cesta tooa privátní klíč__.</span><span class="sxs-lookup"><span data-stu-id="82f2d-151">You can specify hello __path tooa private key__.</span></span> <span data-ttu-id="82f2d-152">S hello `ssh` klienta, hello `-i` parametr je použité toospecify hello cesta tooprivate klíč.</span><span class="sxs-lookup"><span data-stu-id="82f2d-152">With hello `ssh` client, hello `-i` parameter is used toospecify hello path tooprivate key.</span></span> <span data-ttu-id="82f2d-153">Například, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="82f2d-153">For example, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.</span></span>

* <span data-ttu-id="82f2d-154">Pokud máte __více privátních klíčů__, které používáte na různých serverech, zvažte používání nástroje, jako je [SSH agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent).</span><span class="sxs-lookup"><span data-stu-id="82f2d-154">If you have __multiple private keys__ for use with different servers, consider using a utility such as [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent).</span></span> <span data-ttu-id="82f2d-155">Hello `ssh-agent` nástroj může být použité tooautomatically vyberte hello klíče toouse při navazování relace SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-155">hello `ssh-agent` utility can be used tooautomatically select hello key toouse when establishing an SSH session.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="82f2d-156">Pokud se zabezpečením váš privátní klíč pomocí přístupového hesla, je nutné zadat heslo hello při použití klíče hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-156">If you secure your private key with a passphrase, you must enter hello passphrase when using hello key.</span></span> <span data-ttu-id="82f2d-157">Nástroje, jako `ssh-agent` hello hesla pro usnadnění vaší práce může do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="82f2d-157">Utilities such as `ssh-agent` can cache hello password for your convenience.</span></span>

### <a name="create-an-ssh-key-pair"></a><span data-ttu-id="82f2d-158">Vytvoření páru klíčů SSH</span><span class="sxs-lookup"><span data-stu-id="82f2d-158">Create an SSH key pair</span></span>

<span data-ttu-id="82f2d-159">Použití hello `ssh-keygen` příkaz toocreate soubory veřejného a privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="82f2d-159">Use hello `ssh-keygen` command toocreate public and private key files.</span></span> <span data-ttu-id="82f2d-160">Hello následující příkaz generuje 2048 bitů pár klíčů RSA, který lze použít s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="82f2d-160">hello following command generates a 2048-bit RSA key pair that can be used with HDInsight:</span></span>

    ssh-keygen -t rsa -b 2048

<span data-ttu-id="82f2d-161">Zobrazí se výzva informace během procesu vytvoření klíče hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-161">You are prompted for information during hello key creation process.</span></span> <span data-ttu-id="82f2d-162">Například, kde jsou uloženy hello klíče nebo jestli toouse přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="82f2d-162">For example, where hello keys are stored or whether toouse a passphrase.</span></span> <span data-ttu-id="82f2d-163">Po dokončení procesu hello jsou vytvořeny dva soubory; veřejný klíč a soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="82f2d-163">After hello process completes, two files are created; a public key and a private key.</span></span>

* <span data-ttu-id="82f2d-164">Hello __veřejný klíč__ je použité toocreate clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="82f2d-164">hello __public key__ is used toocreate an HDInsight cluster.</span></span> <span data-ttu-id="82f2d-165">veřejný klíč Hello s příponou `.pub`.</span><span class="sxs-lookup"><span data-stu-id="82f2d-165">hello public key has an extension of `.pub`.</span></span>

* <span data-ttu-id="82f2d-166">Hello __privátní klíč__ je použité tooauthenticate clusteru HDInsight toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="82f2d-166">hello __private key__ is used tooauthenticate your client toohello HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="82f2d-167">Klíče můžete zabezpečit pomocí přístupového hesla.</span><span class="sxs-lookup"><span data-stu-id="82f2d-167">You can secure your keys using a passphrase.</span></span> <span data-ttu-id="82f2d-168">Přístupové heslo je ve skutečnosti heslo k privátnímu klíči.</span><span class="sxs-lookup"><span data-stu-id="82f2d-168">A passphrase is effectively a password on your private key.</span></span> <span data-ttu-id="82f2d-169">I když někdo získá privátního klíče, musí mít hello přístupové heslo toouse hello klíč.</span><span class="sxs-lookup"><span data-stu-id="82f2d-169">Even if someone obtains your private key, they must have hello passphrase toouse hello key.</span></span>

### <a name="create-hdinsight-using-hello-public-key"></a><span data-ttu-id="82f2d-170">Vytvoření HDInsight pomocí veřejného klíče hello</span><span class="sxs-lookup"><span data-stu-id="82f2d-170">Create HDInsight using hello public key</span></span>

| <span data-ttu-id="82f2d-171">Metoda vytvoření</span><span class="sxs-lookup"><span data-stu-id="82f2d-171">Creation method</span></span> | <span data-ttu-id="82f2d-172">Jak toouse hello veřejný klíč</span><span class="sxs-lookup"><span data-stu-id="82f2d-172">How toouse hello public key</span></span> |
| ------- | ------- |
| <span data-ttu-id="82f2d-173">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="82f2d-173">**Azure portal**</span></span> | <span data-ttu-id="82f2d-174">Zrušte zaškrtnutí políčka __použijte stejné heslo jako přihlašovací údaje clusteru__a potom vyberte __veřejný klíč__ jako hello typ ověřování SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-174">Uncheck __Use same password as cluster login__, and then select __Public Key__ as hello SSH authentication type.</span></span> <span data-ttu-id="82f2d-175">Nakonec vyberte soubor veřejného klíče hello nebo vložte obsah textu hello hello souboru hello __veřejný klíč SSH__ pole.</span><span class="sxs-lookup"><span data-stu-id="82f2d-175">Finally, select hello public key file or paste hello text contents of hello file in hello __SSH public key__ field.</span></span></br><span data-ttu-id="82f2d-176">![Dialogové okno Veřejný klíč SSH při vytváření clusteru HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png)</span><span class="sxs-lookup"><span data-stu-id="82f2d-176">![SSH public key dialog in HDInsight cluster creation](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png)</span></span> |
| <span data-ttu-id="82f2d-177">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="82f2d-177">**Azure PowerShell**</span></span> | <span data-ttu-id="82f2d-178">Použití hello `-SshPublicKey` parametr hello `New-AzureRmHdinsightCluster` rutiny a předejte jí obsah hello hello veřejný klíč jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="82f2d-178">Use hello `-SshPublicKey` parameter of hello `New-AzureRmHdinsightCluster` cmdlet and pass hello contents of hello public key as a string.</span></span>|
| <span data-ttu-id="82f2d-179">**Azure CLI 1.0**</span><span class="sxs-lookup"><span data-stu-id="82f2d-179">**Azure CLI 1.0**</span></span> | <span data-ttu-id="82f2d-180">Použití hello `--sshPublicKey` parametr hello `azure hdinsight cluster create` příkazů a předat hello obsah hello veřejný klíč jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="82f2d-180">Use hello `--sshPublicKey` parameter of hello `azure hdinsight cluster create` command and pass hello contents of hello public key as a string.</span></span> |
| <span data-ttu-id="82f2d-181">**Šablona Resource Manageru**</span><span class="sxs-lookup"><span data-stu-id="82f2d-181">**Resource Manager Template**</span></span> | <span data-ttu-id="82f2d-182">Příklad použití klíčů SSH s využití šablony najdete v části věnované [nasazení HDInsightu v Linuxu pomocí klíče SSH](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/).</span><span class="sxs-lookup"><span data-stu-id="82f2d-182">For an example of using SSH keys with a template, see [Deploy HDInsight on Linux with SSH key](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/).</span></span> <span data-ttu-id="82f2d-183">Hello `publicKeys` element v hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) soubor je tooAzure klíče hello toopass použité při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-183">hello `publicKeys` element in hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) file is used toopass hello keys tooAzure when creating hello cluster.</span></span> |

## <span data-ttu-id="82f2d-184"><a id="sshpassword"></a>Ověřování: Heslo</span><span class="sxs-lookup"><span data-stu-id="82f2d-184"><a id="sshpassword"></a>Authentication: Password</span></span>

<span data-ttu-id="82f2d-185">Účty SSH je možné zabezpečit pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="82f2d-185">SSH accounts can be secured using a password.</span></span> <span data-ttu-id="82f2d-186">Při připojení pomocí protokolu SSH tooHDInsight jste výzvami tooenter hello heslo.</span><span class="sxs-lookup"><span data-stu-id="82f2d-186">When you connect tooHDInsight using SSH, you are prompted tooenter hello password.</span></span>

> [!WARNING]
> <span data-ttu-id="82f2d-187">Ověřování heslem pro SSH nedoporučujeme.</span><span class="sxs-lookup"><span data-stu-id="82f2d-187">We do not recommend using password authentication for SSH.</span></span> <span data-ttu-id="82f2d-188">Hesla můžete uhádnout a jsou snadno napadnutelný toobrute útokům.</span><span class="sxs-lookup"><span data-stu-id="82f2d-188">Passwords can be guessed and are vulnerable toobrute force attacks.</span></span> <span data-ttu-id="82f2d-189">Místo toho doporučujeme využívat [klíče SSH k ověřování](#sshkey).</span><span class="sxs-lookup"><span data-stu-id="82f2d-189">Instead, we recommend that you use [SSH keys for authentication](#sshkey).</span></span>

### <a name="create-hdinsight-using-a-password"></a><span data-ttu-id="82f2d-190">Vytvoření HDInsight s využitím hesla</span><span class="sxs-lookup"><span data-stu-id="82f2d-190">Create HDInsight using a password</span></span>

| <span data-ttu-id="82f2d-191">Metoda vytvoření</span><span class="sxs-lookup"><span data-stu-id="82f2d-191">Creation method</span></span> | <span data-ttu-id="82f2d-192">Jak toospecify hello heslo</span><span class="sxs-lookup"><span data-stu-id="82f2d-192">How toospecify hello password</span></span> |
| --------------- | ---------------- |
| <span data-ttu-id="82f2d-193">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="82f2d-193">**Azure portal**</span></span> | <span data-ttu-id="82f2d-194">Ve výchozím nastavení, hello SSH uživatelský účet má hello stejné heslo jako účet pro přihlášení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="82f2d-194">By default, hello SSH user account has hello same password as hello cluster login account.</span></span> <span data-ttu-id="82f2d-195">Zrušte zaškrtnutí políčka toouse jiné heslo, __použijte stejné heslo jako přihlašovací údaje clusteru__a pak zadejte heslo hello v hello __heslo SSH__ pole.</span><span class="sxs-lookup"><span data-stu-id="82f2d-195">toouse a different password, uncheck __Use same password as cluster login__, and then enter hello password in hello __SSH password__ field.</span></span></br><span data-ttu-id="82f2d-196">![Dialogové okno Heslo SSH při vytváření clusteru HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)</span><span class="sxs-lookup"><span data-stu-id="82f2d-196">![SSH password dialog in HDInsight cluster creation](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)</span></span>|
| <span data-ttu-id="82f2d-197">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="82f2d-197">**Azure PowerShell**</span></span> | <span data-ttu-id="82f2d-198">Použití hello `--SshCredential` parametr hello `New-AzureRmHdinsightCluster` rutiny a předejte `PSCredential` objekt, který obsahuje název hello SSH uživatelského účtu a heslo.</span><span class="sxs-lookup"><span data-stu-id="82f2d-198">Use hello `--SshCredential` parameter of hello `New-AzureRmHdinsightCluster` cmdlet and pass a `PSCredential` object that contains hello SSH user account name and password.</span></span> |
| <span data-ttu-id="82f2d-199">**Azure CLI 1.0**</span><span class="sxs-lookup"><span data-stu-id="82f2d-199">**Azure CLI 1.0**</span></span> | <span data-ttu-id="82f2d-200">Použití hello `--sshPassword` parametr hello `azure hdinsight cluster create` příkazů a zadejte hodnotu hello heslo.</span><span class="sxs-lookup"><span data-stu-id="82f2d-200">Use hello `--sshPassword` parameter of hello `azure hdinsight cluster create` command and provide hello password value.</span></span> |
| <span data-ttu-id="82f2d-201">**Šablona Resource Manageru**</span><span class="sxs-lookup"><span data-stu-id="82f2d-201">**Resource Manager Template**</span></span> | <span data-ttu-id="82f2d-202">Příklad použití hesla s využitím šablony najdete v části věnované [nasazení HDInsightu v Linuxu pomocí hesla SSH](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/).</span><span class="sxs-lookup"><span data-stu-id="82f2d-202">For an example of using a password with a template, see [Deploy HDInsight on Linux with SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/).</span></span> <span data-ttu-id="82f2d-203">Hello `linuxOperatingSystemProfile` element v hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) soubor je použité toopass hello SSH účet jméno a heslo tooAzure při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-203">hello `linuxOperatingSystemProfile` element in hello [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) file is used toopass hello SSH account name and password tooAzure when creating hello cluster.</span></span>|

### <a name="change-hello-ssh-password"></a><span data-ttu-id="82f2d-204">Změnit heslo SSH hello</span><span class="sxs-lookup"><span data-stu-id="82f2d-204">Change hello SSH password</span></span>

<span data-ttu-id="82f2d-205">Informace o změně hesla uživatelského účtu hello SSH naleznete v tématu hello __změnit hesla__ části hello [spravovat HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82f2d-205">For information on changing hello SSH user account password, see hello __Change passwords__ section of hello [Manage HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) document.</span></span>

## <span data-ttu-id="82f2d-206"><a id="domainjoined"></a>Ověřování: HDInsight připojený k doméně</span><span class="sxs-lookup"><span data-stu-id="82f2d-206"><a id="domainjoined"></a>Authentication: Domain-joined HDInsight</span></span>

<span data-ttu-id="82f2d-207">Pokud používáte __připojený k doméně clusteru HDInsight__, je nutné použít hello `kinit` příkaz po připojení pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-207">If you are using a __domain-joined HDInsight cluster__, you must use hello `kinit` command after connecting with SSH.</span></span> <span data-ttu-id="82f2d-208">Tento příkaz vás vyzve k zadání uživatele domény a heslo a ověřuje platnost vaší relace s doménou služby Active Directory Azure hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="82f2d-208">This command prompts you for a domain user and password, and authenticates your session with hello Azure Active Directory domain associated with hello cluster.</span></span>

<span data-ttu-id="82f2d-209">Další informace najdete v tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="82f2d-209">For more information, see [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md).</span></span>

## <a name="connect-toonodes"></a><span data-ttu-id="82f2d-210">Připojit toonodes</span><span class="sxs-lookup"><span data-stu-id="82f2d-210">Connect toonodes</span></span>

<span data-ttu-id="82f2d-211">Hello hlavních uzlech a hraniční uzel (pokud existuje) je přístupná přes hello internet na portech 22 a 23.</span><span class="sxs-lookup"><span data-stu-id="82f2d-211">hello head nodes and edge node (if there is one) can be accessed over hello internet on ports 22 and 23.</span></span>

* <span data-ttu-id="82f2d-212">Při připojování toohello __hlavní uzly__, použití portu __22__ tooconnect toohello primární hlavního uzlu a port __23__ tooconnect toohello sekundárního hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="82f2d-212">When connecting toohello __head nodes__, use port __22__ tooconnect toohello primary head node and port __23__ tooconnect toohello secondary head node.</span></span> <span data-ttu-id="82f2d-213">je technologie Hello toouse název plně kvalifikované domény `clustername-ssh.azurehdinsight.net`, kde `clustername` je hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="82f2d-213">hello fully qualified domain name toouse is `clustername-ssh.azurehdinsight.net`, where `clustername` is hello name of your cluster.</span></span>

    ```bash
    # Connect tooprimary head node
    # port not specified since 22 is hello default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect toosecondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* <span data-ttu-id="82f2d-214">Když connectiung toohello __hraniční uzel__, používat port 22.</span><span class="sxs-lookup"><span data-stu-id="82f2d-214">When connectiung toohello __edge node__, use port 22.</span></span> <span data-ttu-id="82f2d-215">Hello plně kvalifikovaný název domény je `edgenodename.clustername-ssh.azurehdinsight.net`, kde `edgenodename` je název, který jste zadali při vytváření hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="82f2d-215">hello fully qualified domain name is `edgenodename.clustername-ssh.azurehdinsight.net`, where `edgenodename` is a name you provided when creating hello edge node.</span></span> <span data-ttu-id="82f2d-216">`clustername`je název hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="82f2d-216">`clustername` is hello name of hello cluster.</span></span>

    ```bash
    # Connect tooedge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]
> <span data-ttu-id="82f2d-217">předchozí příklady Hello předpokládají, že používáte ověřování hesla nebo, certifikát ověření dochází automaticky.</span><span class="sxs-lookup"><span data-stu-id="82f2d-217">hello previous examples assume that you are using password authentication, or that certificate authentication is occuring automatically.</span></span> <span data-ttu-id="82f2d-218">Pokud použijete pro ověřování SSH dvojici klíčů a certifikátů hello nepoužívá automaticky, použijte hello `-i` parametr toospecify hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="82f2d-218">If you use an SSH key-pair for authentication, and hello certificate is not used automatically, use hello `-i` parameter toospecify hello private key.</span></span> <span data-ttu-id="82f2d-219">Například, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="82f2d-219">For example, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.</span></span>

<span data-ttu-id="82f2d-220">Po připojení se mění hello řádku tooindicate hello SSH uživatelské jméno a hello uzlu, které jste připojeni k.</span><span class="sxs-lookup"><span data-stu-id="82f2d-220">Once connected, hello prompt changes tooindicate hello SSH user name and hello node you are connected to.</span></span> <span data-ttu-id="82f2d-221">Například při připojení toohello primární hlavního uzlu jako `sshuser`, výzva hello je `sshuser@hn0-clustername:~$`.</span><span class="sxs-lookup"><span data-stu-id="82f2d-221">For example, when connected toohello primary head node as `sshuser`, hello prompt is `sshuser@hn0-clustername:~$`.</span></span>

### <a name="connect-tooworker-and-zookeeper-nodes"></a><span data-ttu-id="82f2d-222">Připojit tooworker a uzly Zookeeper</span><span class="sxs-lookup"><span data-stu-id="82f2d-222">Connect tooworker and Zookeeper nodes</span></span>

<span data-ttu-id="82f2d-223">Hello uzlů pracovního procesu a hello Zookeeper uzly nejsou přímo přístupné z Internetu.</span><span class="sxs-lookup"><span data-stu-id="82f2d-223">hello worker nodes and Zookeeper nodes are not directly accessible from hello internet.</span></span> <span data-ttu-id="82f2d-224">Lze k nim z hlavních uzlech clusteru hello nebo uzly okraj.</span><span class="sxs-lookup"><span data-stu-id="82f2d-224">They can be accessed from hello cluster head nodes or edge nodes.</span></span> <span data-ttu-id="82f2d-225">Hello následují hello obecné kroky tooconnect tooother uzly:</span><span class="sxs-lookup"><span data-stu-id="82f2d-225">hello following are hello general steps tooconnect tooother nodes:</span></span>

1. <span data-ttu-id="82f2d-226">Použijte uzel head nebo Microsoft edge tooconnect tooa SSH:</span><span class="sxs-lookup"><span data-stu-id="82f2d-226">Use SSH tooconnect tooa head or edge node:</span></span>

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. <span data-ttu-id="82f2d-227">Hello head toohello připojení SSH nebo hraniční uzel, použije hello `ssh` příkaz tooconnect tooa pracovního uzlu v clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="82f2d-227">From hello SSH connection toohello head or edge node, use hello `ssh` command tooconnect tooa worker node in hello cluster:</span></span>

        ssh sshuser@wn0-myhdi

    <span data-ttu-id="82f2d-228">tooretrieve seznam názvů domén hello hello uzlů v clusteru hello, najdete v části hello [hello spravovat HDInsight pomocí Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="82f2d-228">tooretrieve a list of hello domain names of hello nodes in hello cluster, see hello [Manage HDInsight by using hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) document.</span></span>

<span data-ttu-id="82f2d-229">Pokud hello účtu SSH zabezpečené pomocí __heslo__, zadejte heslo hello, pokud se připojujete.</span><span class="sxs-lookup"><span data-stu-id="82f2d-229">If hello SSH account is secured using a __password__, enter hello password when connecting.</span></span>

<span data-ttu-id="82f2d-230">Pokud hello účtu SSH zabezpečené pomocí __klíče SSH__, ujistěte se, zda je povoleno předávání SSH na klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-230">If hello SSH account is secured using __SSH keys__, make sure that SSH forwarding is enabled on hello client.</span></span>

> [!NOTE]
> <span data-ttu-id="82f2d-231">Dalším způsobem toodirectly přístup všechny uzly v clusteru hello je tooinstall HDInsight do Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="82f2d-231">Another way toodirectly access all nodes in hello cluster is tooinstall HDInsight into an Azure Virtual Network.</span></span> <span data-ttu-id="82f2d-232">Pak můžete připojit vaše toohello vzdáleném počítači stejný virtuální sítě a přímý přístup k všechny uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-232">Then, you can join your remote machine toohello same virtual network and directly access all nodes in hello cluster.</span></span>
>
> <span data-ttu-id="82f2d-233">Další informace najdete v tématu [Použití virtuální sítě s HDInsightem](hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="82f2d-233">For more information, see [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md).</span></span>

### <a name="configure-ssh-agent-forwarding"></a><span data-ttu-id="82f2d-234">Konfigurace přesměrování agenta SSH</span><span class="sxs-lookup"><span data-stu-id="82f2d-234">Configure SSH agent forwarding</span></span>

> [!IMPORTANT]
> <span data-ttu-id="82f2d-235">Hello následující kroky předpokládají systému UNIX nebo Linux a pracovat s Bash ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="82f2d-235">hello following steps assume a Linux or UNIX-based system, and work with Bash on Windows 10.</span></span> <span data-ttu-id="82f2d-236">Pokud tyto kroky nefungují pro váš systém, může být nutné tooconsult hello dokumentace pro vašeho klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-236">If these steps do not work for your system, you may need tooconsult hello documentation for your SSH client.</span></span>

1. <span data-ttu-id="82f2d-237">V textovém editoru otevřete `~/.ssh/config`.</span><span class="sxs-lookup"><span data-stu-id="82f2d-237">Using a text editor, open `~/.ssh/config`.</span></span> <span data-ttu-id="82f2d-238">Pokud tento soubor neexistuje, můžete ho vytvořit tak, že na příkazovém řádku zadáte `touch ~/.ssh/config`.</span><span class="sxs-lookup"><span data-stu-id="82f2d-238">If this file doesn't exist, you can create it by entering `touch ~/.ssh/config` at a command line.</span></span>

2. <span data-ttu-id="82f2d-239">Přidejte následující text toohello hello `config` souboru.</span><span class="sxs-lookup"><span data-stu-id="82f2d-239">Add hello following text toohello `config` file.</span></span>

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    <span data-ttu-id="82f2d-240">Nahraďte hello __hostitele__ informace s adresou hello hello uzlu připojíte toousing SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-240">Replace hello __Host__ information with hello address of hello node you connect toousing SSH.</span></span> <span data-ttu-id="82f2d-241">předchozí příklad Hello používá hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="82f2d-241">hello previous example uses hello edge node.</span></span> <span data-ttu-id="82f2d-242">Tato položka se nakonfiguruje přesměrování agenta SSH pro hello určeného uzlu.</span><span class="sxs-lookup"><span data-stu-id="82f2d-242">This entry configures SSH agent forwarding for hello specified node.</span></span>

3. <span data-ttu-id="82f2d-243">Test pomocí následujících příkazu z terminálu hello hello přesměrování agenta SSH:</span><span class="sxs-lookup"><span data-stu-id="82f2d-243">Test SSH agent forwarding by using hello following command from hello terminal:</span></span>

        echo "$SSH_AUTH_SOCK"

    <span data-ttu-id="82f2d-244">Tento příkaz vrátí informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="82f2d-244">This command returns information similar toohello following text:</span></span>

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    <span data-ttu-id="82f2d-245">Když se nic nevrátí, znamená to, že nástroj `ssh-agent` není spuštěný.</span><span class="sxs-lookup"><span data-stu-id="82f2d-245">If nothing is returned, then `ssh-agent` is not running.</span></span> <span data-ttu-id="82f2d-246">Další informace najdete v tématu hello agenta spuštění skriptů informace [Using ssh-agent s ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) nebo dokumentaci klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-246">For more information, see hello agent startup scripts information at [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) or consult your SSH client documentation.</span></span>

4. <span data-ttu-id="82f2d-247">Jakmile si ověříte, **ssh-agent** je spuštěna, následující tooadd hello použití privátního klíče toohello agenta SSH:</span><span class="sxs-lookup"><span data-stu-id="82f2d-247">Once you have verified that **ssh-agent** is running, use hello following tooadd your SSH private key toohello agent:</span></span>

        ssh-add ~/.ssh/id_rsa

    <span data-ttu-id="82f2d-248">Pokud váš privátní klíč je uložený v jiném souboru, nahraďte `~/.ssh/id_rsa` s toohello hello cestě k souboru.</span><span class="sxs-lookup"><span data-stu-id="82f2d-248">If your private key is stored in a different file, replace `~/.ssh/id_rsa` with hello path toohello file.</span></span>

5. <span data-ttu-id="82f2d-249">Připojte toohello clusteru hraniční uzel nebo hlavních uzlech pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-249">Connect toohello cluster edge node or head nodes using SSH.</span></span> <span data-ttu-id="82f2d-250">Pak pomocí hello SSH příkaz tooconnect tooa worker nebo zookeeper uzlu.</span><span class="sxs-lookup"><span data-stu-id="82f2d-250">Then use hello SSH command tooconnect tooa worker or zookeeper node.</span></span> <span data-ttu-id="82f2d-251">Hello připojení pomocí hello předávaných klíče.</span><span class="sxs-lookup"><span data-stu-id="82f2d-251">hello connection is established using hello forwarded key.</span></span>

## <a name="copy-files"></a><span data-ttu-id="82f2d-252">Kopírování souborů</span><span class="sxs-lookup"><span data-stu-id="82f2d-252">Copy files</span></span>

<span data-ttu-id="82f2d-253">Hello `scp` nástroj lze použít toocopy tooand soubory z jednotlivých uzlů v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-253">hello `scp` utility can be used toocopy files tooand from individual nodes in hello cluster.</span></span> <span data-ttu-id="82f2d-254">Například hello následující příkaz kopie hello `test.txt` adresář z hello místní systém toohello primární hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="82f2d-254">For example, hello following command copies hello `test.txt` directory from hello local system toohello primary head node:</span></span>

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

<span data-ttu-id="82f2d-255">Vzhledem k tomu, že není zadána žádná cesta po hello `:`, hello soubor je umístěn v hello `sshuser` domovský adresář.</span><span class="sxs-lookup"><span data-stu-id="82f2d-255">Since no path is specified after hello `:`, hello file is placed in hello `sshuser` home directory.</span></span>

<span data-ttu-id="82f2d-256">Následující příklad kopie hello Hello `test.txt` soubor z hello `sshuser` domovský adresář v místním systému toohello hello primární hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="82f2d-256">hello following example copies hello `test.txt` file from hello `sshuser` home directory on hello primary head node toohello local system:</span></span>

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]
> <span data-ttu-id="82f2d-257">`scp`přístup jenom k systému souborů hello jednotlivých uzlů v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="82f2d-257">`scp` can only access hello file system of individual nodes within hello cluster.</span></span> <span data-ttu-id="82f2d-258">Nejde použít tooaccess data v hello HDFS kompatibilní úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="82f2d-258">It cannot be used tooaccess data in hello HDFS-compatible storage for hello cluster.</span></span>
>
> <span data-ttu-id="82f2d-259">Použít `scp` když potřebujete tooupload prostředku pro použití z relace SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-259">Use `scp` when you need tooupload a resource for use from an SSH session.</span></span> <span data-ttu-id="82f2d-260">Například odeslat skript v jazyce Python a pak spusťte skript hello z relace SSH.</span><span class="sxs-lookup"><span data-stu-id="82f2d-260">For example, upload a Python script and then run hello script from an SSH session.</span></span>
>
> <span data-ttu-id="82f2d-261">Informace k přímo načítání dat do hello HDFS kompatibilní úložiště najdete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="82f2d-261">For information on directly loading data into hello HDFS-compatible storage, see hello following documents:</span></span>
>
> * <span data-ttu-id="82f2d-262">[HDInsight využívající Azure Storage](hdinsight-hadoop-use-blob-storage.md)</span><span class="sxs-lookup"><span data-stu-id="82f2d-262">[HDInsight using Azure Storage](hdinsight-hadoop-use-blob-storage.md).</span></span>
>
> * <span data-ttu-id="82f2d-263">[HDInsight využívající Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md)</span><span class="sxs-lookup"><span data-stu-id="82f2d-263">[HDInsight using Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="82f2d-264">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82f2d-264">Next steps</span></span>

* [<span data-ttu-id="82f2d-265">Použití tunelování SSH s HDInsightem</span><span class="sxs-lookup"><span data-stu-id="82f2d-265">Use SSH tunneling with HDInsight</span></span>](hdinsight-linux-ambari-ssh-tunnel.md)
* [<span data-ttu-id="82f2d-266">Použití virtuální sítě s HDInsightem</span><span class="sxs-lookup"><span data-stu-id="82f2d-266">Use a virtual network with HDInsight</span></span>](hdinsight-extend-hadoop-virtual-network.md)
* [<span data-ttu-id="82f2d-267">Použití hraničních uzlů v HDInsightu</span><span class="sxs-lookup"><span data-stu-id="82f2d-267">Use edge nodes in HDInsight</span></span>](hdinsight-apps-use-edge-node.md#access-an-edge-node)