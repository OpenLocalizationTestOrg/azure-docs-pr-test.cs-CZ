---
title: aaaUse SSH tunelu tooaccess Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak procházet toouse toosecurely tunelového propojení SSH webové prostředky, které jsou hostované na uzly HDInsight se systémem Linux."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a315c53751bbe6950a182674f4108c67c2f2f8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-ssh-tunneling-tooaccess-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a><span data-ttu-id="93c98-103">Použít webovému uživatelskému rozhraní Ambari tooaccess tunelového propojení SSH, JobHistory, NameNode, Oozie a jiným webovým uživatelská rozhraní</span><span class="sxs-lookup"><span data-stu-id="93c98-103">Use SSH Tunneling tooaccess Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs</span></span>

<span data-ttu-id="93c98-104">Clustery se systémem Linux HDInsight poskytují přístup tooAmbari webového uživatelského rozhraní nad hello Internet, ale nejsou některé funkce hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="93c98-104">Linux-based HDInsight clusters provide access tooAmbari web UI over hello Internet, but some features of hello UI are not.</span></span> <span data-ttu-id="93c98-105">Například hello webového uživatelského rozhraní pro jiné služby, které jsou prezentované pomocí Ambari.</span><span class="sxs-lookup"><span data-stu-id="93c98-105">For example, hello web UI for other services that are surfaced through Ambari.</span></span> <span data-ttu-id="93c98-106">Pro plnou funkčnost hello webovému uživatelskému rozhraní Ambari je nutné použít SSH tunel toohello clusteru head.</span><span class="sxs-lookup"><span data-stu-id="93c98-106">For full functionality of hello Ambari web UI, you must use an SSH tunnel toohello cluster head.</span></span>

## <a name="why-use-an-ssh-tunnel"></a><span data-ttu-id="93c98-107">Proč používat tunelového propojení SSH</span><span class="sxs-lookup"><span data-stu-id="93c98-107">Why use an SSH tunnel</span></span>

<span data-ttu-id="93c98-108">Několik nabídek hello v Ambari fungovat pouze pomocí tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="93c98-108">Several of hello menus in Ambari only work through an SSH tunnel.</span></span> <span data-ttu-id="93c98-109">Tyto nabídky spoléhají na weby a služby běžící na jiné typy uzlů, například uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="93c98-109">These menus rely on web sites and services running on other node types, such as worker nodes.</span></span> <span data-ttu-id="93c98-110">Často tyto weby nejsou zabezpečené, takže není bezpečné toodirectly zveřejněte je na Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-110">Often, these web sites are not secured, so it is not safe toodirectly expose them on hello internet.</span></span>

<span data-ttu-id="93c98-111">Hello následující webové uživatelská vyžadovat tunelového propojení SSH:</span><span class="sxs-lookup"><span data-stu-id="93c98-111">hello following Web UIs require an SSH tunnel:</span></span>

* <span data-ttu-id="93c98-112">JobHistory</span><span class="sxs-lookup"><span data-stu-id="93c98-112">JobHistory</span></span>
* <span data-ttu-id="93c98-113">NameNode</span><span class="sxs-lookup"><span data-stu-id="93c98-113">NameNode</span></span>
* <span data-ttu-id="93c98-114">Zásobníky vlákna</span><span class="sxs-lookup"><span data-stu-id="93c98-114">Thread Stacks</span></span>
* <span data-ttu-id="93c98-115">Oozie webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="93c98-115">Oozie web UI</span></span>
* <span data-ttu-id="93c98-116">Hlavní HBase a protokoly uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="93c98-116">HBase Master and Logs UI</span></span>

<span data-ttu-id="93c98-117">Pokud používáte cluster toocustomize akcí skriptů, vyžadují jakékoli služby nebo nástroje, které nainstalujete, které zveřejňují webového uživatelského rozhraní tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="93c98-117">If you use Script Actions toocustomize your cluster, any services or utilities that you install that expose a web UI require an SSH tunnel.</span></span> <span data-ttu-id="93c98-118">Například pokud nainstalujete Hue pomocí akce skriptu, musíte použít SSH tunel tooaccess hello uživatelské rozhraní webu Hue.</span><span class="sxs-lookup"><span data-stu-id="93c98-118">For example, if you install Hue using a Script Action, you must use an SSH tunnel tooaccess hello Hue web UI.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93c98-119">Pokud máte tooHDInsight přímý přístup přes virtuální síť, není nutné toouse tunely SSH.</span><span class="sxs-lookup"><span data-stu-id="93c98-119">If you have direct access tooHDInsight through a virtual network, you do not need toouse SSH tunnels.</span></span> <span data-ttu-id="93c98-120">Příklad přímo přístup k HDInsight prostřednictvím virtuální sítě, naleznete v části hello [připojit HDInsight tooyour do místní sítě](connect-on-premises-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="93c98-120">For an example of directly accessing HDInsight through a virtual network, see hello [Connect HDInsight tooyour on-premises network](connect-on-premises-network.md) document.</span></span>

## <a name="what-is-an-ssh-tunnel"></a><span data-ttu-id="93c98-121">Co je tunelového propojení SSH</span><span class="sxs-lookup"><span data-stu-id="93c98-121">What is an SSH tunnel</span></span>

<span data-ttu-id="93c98-122">[Secure Shell (SSH) tunelování](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) směruje provoz odeslaný tooa port na místní pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="93c98-122">[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) routes traffic sent tooa port on your local workstation.</span></span> <span data-ttu-id="93c98-123">Hello provoz se směruje pomocí SSH připojení tooyour hlavního uzlu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="93c98-123">hello traffic is routed through an SSH connection tooyour HDInsight cluster head node.</span></span> <span data-ttu-id="93c98-124">žádost o Hello se vyřeší, jako by bylo provedeno hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="93c98-124">hello request is resolved as if it originated on hello head node.</span></span> <span data-ttu-id="93c98-125">odpověď Hello je pak směrovat zpět pomocí hello tunel tooyour pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="93c98-125">hello response is then routed back through hello tunnel tooyour workstation.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93c98-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="93c98-126">Prerequisites</span></span>

* <span data-ttu-id="93c98-127">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="93c98-127">An SSH client.</span></span> <span data-ttu-id="93c98-128">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="93c98-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="93c98-129">Webový prohlížeč, který může být nakonfigurován toouse SOCKS5 proxy.</span><span class="sxs-lookup"><span data-stu-id="93c98-129">A web browser that can be configured toouse a SOCKS5 proxy.</span></span>

    > [!WARNING]
    > <span data-ttu-id="93c98-130">Podpora proxy SOCKS Hello je součástí systému Windows nepodporuje SOCKS5 a nefunguje s hello kroky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="93c98-130">hello SOCKS proxy support built into Windows does not support SOCKS5, and does not work with hello steps in this document.</span></span> <span data-ttu-id="93c98-131">Hello následujících prohlížečů závisí na nastavení proxy serveru systému Windows a ne aktuálně pracují s hello kroky v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="93c98-131">hello following browsers rely on Windows proxy settings, and do not currently work with hello steps in this document:</span></span>
    >
    > * <span data-ttu-id="93c98-132">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="93c98-132">Microsoft Edge</span></span>
    > * <span data-ttu-id="93c98-133">Aplikace Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="93c98-133">Microsoft Internet Explorer</span></span>
    >
    > <span data-ttu-id="93c98-134">Google Chrome také závisí na nastavení proxy serveru pro Windows hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-134">Google Chrome also relies on hello Windows proxy settings.</span></span> <span data-ttu-id="93c98-135">Můžete však nainstalovat rozšíření, které podporují SOCKS5.</span><span class="sxs-lookup"><span data-stu-id="93c98-135">However, you can install extensions that support SOCKS5.</span></span> <span data-ttu-id="93c98-136">Doporučujeme, abyste [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).</span><span class="sxs-lookup"><span data-stu-id="93c98-136">We recommend [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).</span></span>

## <span data-ttu-id="93c98-137"><a name="usessh"></a>Vytvořit tunelové propojení příkazu SSH hello</span><span class="sxs-lookup"><span data-stu-id="93c98-137"><a name="usessh"></a>Create a tunnel using hello SSH command</span></span>

<span data-ttu-id="93c98-138">Použití hello následující příkaz toocreate tunelového propojení SSH pomocí hello `ssh` příkaz.</span><span class="sxs-lookup"><span data-stu-id="93c98-138">Use hello following command toocreate an SSH tunnel using hello `ssh` command.</span></span> <span data-ttu-id="93c98-139">Nahraďte **uživatelské jméno** s uživatelem SSH pro váš HDInsight cluster a nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="93c98-139">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with hello name of your HDInsight cluster:</span></span>

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

<span data-ttu-id="93c98-140">Tento příkaz vytvoří připojení, který směruje provoz toolocal port 9876 toohello clusteru prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="93c98-140">This command creates a connection that routes traffic toolocal port 9876 toohello cluster over SSH.</span></span> <span data-ttu-id="93c98-141">Hello možnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="93c98-141">hello options are:</span></span>

* <span data-ttu-id="93c98-142">**D 9876** -hello místního portu, který směruje provoz tunelem hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-142">**D 9876** - hello local port that routes traffic through hello tunnel.</span></span>
* <span data-ttu-id="93c98-143">**C** -komprimovat všechna data, protože webový provoz je většinou text.</span><span class="sxs-lookup"><span data-stu-id="93c98-143">**C** - Compress all data, because web traffic is mostly text.</span></span>
* <span data-ttu-id="93c98-144">**2** -force SSH tootry protocol verze 2 pouze.</span><span class="sxs-lookup"><span data-stu-id="93c98-144">**2** - Force SSH tootry protocol version 2 only.</span></span>
* <span data-ttu-id="93c98-145">**q** -tichém režimu.</span><span class="sxs-lookup"><span data-stu-id="93c98-145">**q** - Quiet mode.</span></span>
* <span data-ttu-id="93c98-146">**T** -zakázat pseudo tty přidělení, protože jsme se právě předávání port.</span><span class="sxs-lookup"><span data-stu-id="93c98-146">**T** - Disable pseudo-tty allocation, since we are just forwarding a port.</span></span>
* <span data-ttu-id="93c98-147">**n**-Zabránit čtení STDIN, protože jsme se právě předávání port.</span><span class="sxs-lookup"><span data-stu-id="93c98-147">**n** - Prevent reading of STDIN, since we are just forwarding a port.</span></span>
* <span data-ttu-id="93c98-148">**N** -nespouštějte vzdáleného příkazu, protože jsme se právě předávání port.</span><span class="sxs-lookup"><span data-stu-id="93c98-148">**N** - Do not execute a remote command, since we are just forwarding a port.</span></span>
* <span data-ttu-id="93c98-149">**f** -spustit hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="93c98-149">**f** - Run in hello background.</span></span>

<span data-ttu-id="93c98-150">Po dokončení příkazu hello je provoz odeslaný tooport 9876 na místním počítači hello směrované toohello hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="93c98-150">Once hello command finishes, traffic sent tooport 9876 on hello local computer is routed toohello cluster head node.</span></span>

## <span data-ttu-id="93c98-151"><a name="useputty"></a>Vytvořit tunelové propojení PuTTY</span><span class="sxs-lookup"><span data-stu-id="93c98-151"><a name="useputty"></a>Create a tunnel using PuTTY</span></span>

<span data-ttu-id="93c98-152">[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) je grafický klient SSH pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="93c98-152">[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) is a graphical SSH client for Windows.</span></span> <span data-ttu-id="93c98-153">Použijte následující postup toocreate tunelového propojení SSH pomocí klienta PuTTY hello:</span><span class="sxs-lookup"><span data-stu-id="93c98-153">Use hello following steps toocreate an SSH tunnel using PuTTY:</span></span>

1. <span data-ttu-id="93c98-154">Otevřete PuTTY a zadejte informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="93c98-154">Open PuTTY, and enter your connection information.</span></span> <span data-ttu-id="93c98-155">Pokud nejste obeznámeni s PuTTY, najdete v části hello [PuTTY dokumentace (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).</span><span class="sxs-lookup"><span data-stu-id="93c98-155">If you are not familiar with PuTTY, see hello [PuTTY documentation (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).</span></span>

2. <span data-ttu-id="93c98-156">V hello **kategorie** levé části toohello hello dialogového okna, rozbalte položku **připojení**, rozbalte položku **SSH**a potom vyberte **tunely**.</span><span class="sxs-lookup"><span data-stu-id="93c98-156">In hello **Category** section toohello left of hello dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>

3. <span data-ttu-id="93c98-157">Zadejte následující informace na hello hello **možnosti řízení SSH port předávání** formuláře:</span><span class="sxs-lookup"><span data-stu-id="93c98-157">Provide hello following information on hello **Options controlling SSH port forwarding** form:</span></span>
   
   * <span data-ttu-id="93c98-158">**Zdrojový port** -hello na klienta hello chcete tooforward port.</span><span class="sxs-lookup"><span data-stu-id="93c98-158">**Source port** - hello port on hello client that you wish tooforward.</span></span> <span data-ttu-id="93c98-159">Například **9876**.</span><span class="sxs-lookup"><span data-stu-id="93c98-159">For example, **9876**.</span></span>

   * <span data-ttu-id="93c98-160">**Cílový** -hello adresa SSH pro cluster HDInsight se systémem Linux hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-160">**Destination** - hello SSH address for hello Linux-based HDInsight cluster.</span></span> <span data-ttu-id="93c98-161">Například **mycluster-ssh.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="93c98-161">For example, **mycluster-ssh.azurehdinsight.net**.</span></span>

   * <span data-ttu-id="93c98-162">**Dynamicky** – umožňuje dynamické směrování proxy SOCKS.</span><span class="sxs-lookup"><span data-stu-id="93c98-162">**Dynamic** - Enables dynamic SOCKS proxy routing.</span></span>
     
     ![Obrázek možnosti tunelové propojení](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. <span data-ttu-id="93c98-164">Klikněte na tlačítko **přidat** tooadd hello nastavení a potom klikněte na **otevřete** tooopen připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="93c98-164">Click **Add** tooadd hello settings, and then click **Open** tooopen an SSH connection.</span></span>

5. <span data-ttu-id="93c98-165">Pokud budete vyzváni, přihlaste se toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="93c98-165">When prompted, log in toohello server.</span></span>

## <a name="use-hello-tunnel-from-your-browser"></a><span data-ttu-id="93c98-166">Použití hello tunel z prohlížeče</span><span class="sxs-lookup"><span data-stu-id="93c98-166">Use hello tunnel from your browser</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93c98-167">Hello kroky v této části použijte hello prohlížeče Mozilla FireFox, jak stejné nastavení proxy serveru poskytuje hello ve všech platformách.</span><span class="sxs-lookup"><span data-stu-id="93c98-167">hello steps in this section use hello Mozilla FireFox browser, as it provides hello same proxy settings across all platforms.</span></span> <span data-ttu-id="93c98-168">Jiné moderní prohlížeče, jako je například Google Chrome, může vyžadovat rozšíření například FoxyProxy toowork s hello tunel.</span><span class="sxs-lookup"><span data-stu-id="93c98-168">Other modern browsers, such as Google Chrome, may require an extension such as FoxyProxy toowork with hello tunnel.</span></span>

1. <span data-ttu-id="93c98-169">Konfigurace prohlížeče toouse hello **localhost** a port hello použité při vytváření tunelového propojení hello jako **SOCKS v5** proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="93c98-169">Configure hello browser toouse **localhost** and hello port you used when creating hello tunnel as a **SOCKS v5** proxy.</span></span> <span data-ttu-id="93c98-170">Zde je, jak vypadají hello Firefox nastavení.</span><span class="sxs-lookup"><span data-stu-id="93c98-170">Here's what hello Firefox settings look like.</span></span> <span data-ttu-id="93c98-171">Pokud jste použili jiný port než 9876, změna toohello hello port, kterého jste jeden:</span><span class="sxs-lookup"><span data-stu-id="93c98-171">If you used a different port than 9876, change hello port toohello one you used:</span></span>
   
    ![Obrázek nastavení Firefox](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > <span data-ttu-id="93c98-173">Výběr **DNS vzdálené** přeloží požadavky systému DNS (Domain Name) pomocí hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="93c98-173">Selecting **Remote DNS** resolves Domain Name System (DNS) requests by using hello HDInsight cluster.</span></span> <span data-ttu-id="93c98-174">Toto nastavení vyřeší DNS pomocí hello hlavního uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-174">This setting resolves DNS using hello head node of hello cluster.</span></span>

2. <span data-ttu-id="93c98-175">Ověřte, že hello tunel funguje návštěvou lokality [http://www.whatismyip.com/](http://www.whatismyip.com/).</span><span class="sxs-lookup"><span data-stu-id="93c98-175">Verify that hello tunnel works by visiting a site such as [http://www.whatismyip.com/](http://www.whatismyip.com/).</span></span> <span data-ttu-id="93c98-176">Hello IP vrátí, má být jeden používána datacenter hello Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="93c98-176">hello IP returned should be one used by hello Microsoft Azure datacenter.</span></span>

## <a name="verify-with-ambari-web-ui"></a><span data-ttu-id="93c98-177">Ověřte si u webovému uživatelskému rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="93c98-177">Verify with Ambari web UI</span></span>

<span data-ttu-id="93c98-178">Po vytvoření clusteru hello, použijte následující postup tooverify, že máte přístup z hello Ambari Web webové služby uživatelská rozhraní hello:</span><span class="sxs-lookup"><span data-stu-id="93c98-178">Once hello cluster has been established, use hello following steps tooverify that you can access service web UIs from hello Ambari Web:</span></span>

1. <span data-ttu-id="93c98-179">V prohlížeči přejděte toohttp://headnodehost:8080.</span><span class="sxs-lookup"><span data-stu-id="93c98-179">In your browser, go toohttp://headnodehost:8080.</span></span> <span data-ttu-id="93c98-180">Hello `headnodehost` adresu odesílané přes hello tunel toohello clusteru a vyřešit headnode toohello, která Ambari běží na.</span><span class="sxs-lookup"><span data-stu-id="93c98-180">hello `headnodehost` address is sent over hello tunnel toohello cluster and resolve toohello headnode that Ambari is running on.</span></span> <span data-ttu-id="93c98-181">Po zobrazení výzvy zadejte uživatelské jméno správce hello (správce) a heslo pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="93c98-181">When prompted, enter hello admin user name (admin) and password for your cluster.</span></span> <span data-ttu-id="93c98-182">Můžete být vyzváni podruhé podle webovému uživatelskému rozhraní Ambari hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-182">You may be prompted a second time by hello Ambari web UI.</span></span> <span data-ttu-id="93c98-183">Pokud ano, znovu zadejte informace hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-183">If so, reenter hello information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="93c98-184">Při použití hello http://headnodehost:8080 adresu tooconnect toohello clusteru, připojení prostřednictvím tunelového propojení hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-184">When using hello http://headnodehost:8080 address tooconnect toohello cluster, you are connecting through hello tunnel.</span></span> <span data-ttu-id="93c98-185">Komunikace je zabezpečena pomocí tunelu SSH hello místo protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="93c98-185">Communication is secured using hello SSH tunnel instead of HTTPS.</span></span> <span data-ttu-id="93c98-186">tooconnect přes hello internet pomocí protokolu HTTPS, použijte https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je název hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="93c98-186">tooconnect over hello internet using HTTPS, use https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of hello cluster.</span></span>

2. <span data-ttu-id="93c98-187">Hello webové uživatelské rozhraní Ambari vyberte ze seznamu hello na hello nalevo od stránku hello HDFS.</span><span class="sxs-lookup"><span data-stu-id="93c98-187">From hello Ambari Web UI, select HDFS from hello list on hello left of hello page.</span></span>

    ![Bitové kopie s HDFS vybrané](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. <span data-ttu-id="93c98-189">Když se zobrazí informace o službě HDFS hello, vyberte **rychlé odkazy**.</span><span class="sxs-lookup"><span data-stu-id="93c98-189">When hello HDFS service information is displayed, select **Quick Links**.</span></span> <span data-ttu-id="93c98-190">Zobrazí se seznam hlavních uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="93c98-190">A list of hello cluster head nodes appears.</span></span> <span data-ttu-id="93c98-191">Vyberte jeden z hlavních uzlech hello a pak vyberte **uživatelského rozhraní NameNode**.</span><span class="sxs-lookup"><span data-stu-id="93c98-191">Select one of hello head nodes, and then select **NameNode UI**.</span></span>

    ![Rozšířit bitové kopie s hello rychlé odkazy nabídky](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > <span data-ttu-id="93c98-193">Když vyberete __rychlé odkazy__, může dojít k čekání indikátoru.</span><span class="sxs-lookup"><span data-stu-id="93c98-193">When you select __Quick Links__, you may get a wait indicator.</span></span> <span data-ttu-id="93c98-194">K tomuto stavu může dojít, pokud máte pomalé připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="93c98-194">This condition can occur if you have a slow internet connection.</span></span> <span data-ttu-id="93c98-195">Počkejte minutu nebo dvě pro toobe hello dat přijatých ze serveru hello, a akci opakujte hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="93c98-195">Wait a minute or two for hello data toobe received from hello server, then try hello list again.</span></span>
   >
   > <span data-ttu-id="93c98-196">Některé položky v hello **rychlé odkazy** nabídky, může být zkrácené hello pravé straně úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="93c98-196">Some entries in hello **Quick Links** menu may be cut off by hello right side of hello screen.</span></span> <span data-ttu-id="93c98-197">Pokud ano, rozbalte položku nabídky hello pomocí myši a použít hello šipku vpravo klíče tooscroll hello obrazovky toohello správné toosee hello zbytek hello nabídky.</span><span class="sxs-lookup"><span data-stu-id="93c98-197">If so, expand hello menu using your mouse and use hello right arrow key tooscroll hello screen toohello right toosee hello rest of hello menu.</span></span>

4. <span data-ttu-id="93c98-198">Zobrazí se stránka podobné toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="93c98-198">A page similar toohello following image is displayed:</span></span>

    ![Obrázek hello NameNode uživatelského rozhraní](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > <span data-ttu-id="93c98-200">Všimněte si hello adresu URL pro tuto stránku; by mělo být podobné příliš**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/clusteru**.</span><span class="sxs-lookup"><span data-stu-id="93c98-200">Notice hello URL for this page; it should be similar too**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster**.</span></span> <span data-ttu-id="93c98-201">Tento identifikátor URI hello interní plně kvalifikovaný název domény (FQDN) uzlu hello používá a je k dispozici pouze při používání tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="93c98-201">This URI is using hello internal fully qualified domain name (FQDN) of hello node, and is only accessible when using an SSH tunnel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93c98-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93c98-202">Next steps</span></span>

<span data-ttu-id="93c98-203">Teď, když jste se naučili, jak toocreate a používání tunelového propojení SSH, najdete v následujícím dokumentu pro jiné způsoby toouse Ambari hello:</span><span class="sxs-lookup"><span data-stu-id="93c98-203">Now that you have learned how toocreate and use an SSH tunnel, see hello following document for other ways toouse Ambari:</span></span>

* [<span data-ttu-id="93c98-204">Správa clusterů HDInsight pomocí Ambari</span><span class="sxs-lookup"><span data-stu-id="93c98-204">Manage HDInsight clusters by using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

<span data-ttu-id="93c98-205">Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="93c98-205">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

