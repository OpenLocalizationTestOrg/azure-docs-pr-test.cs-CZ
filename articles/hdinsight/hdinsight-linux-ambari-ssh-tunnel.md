---
title: "Použití SSH tunelového propojení pro přístup k Azure HDInsight | Microsoft Docs"
description: "Další informace o použití tunelového propojení SSH bezpečně procházet webové prostředky, které jsou hostované na uzly HDInsight se systémem Linux."
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
ms.openlocfilehash: 4b606ea3797d685b9deacf72f1bd31e0ef007f98
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-ssh-tunneling-to-access-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a><span data-ttu-id="2c714-103">Pomocí tunelového propojení SSH pro přístup k webovému uživatelskému rozhraní Ambari, JobHistory, NameNode, Oozie a jiným webovým uživatelská rozhraní</span><span class="sxs-lookup"><span data-stu-id="2c714-103">Use SSH Tunneling to access Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs</span></span>

<span data-ttu-id="2c714-104">Clustery HDInsight se systémem Linux poskytnout přístup k webovému uživatelskému rozhraní Ambari přes Internet, ale nejsou některé funkce uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2c714-104">Linux-based HDInsight clusters provide access to Ambari web UI over the Internet, but some features of the UI are not.</span></span> <span data-ttu-id="2c714-105">Například webového uživatelského rozhraní pro jiné služby, které jsou prezentované pomocí Ambari.</span><span class="sxs-lookup"><span data-stu-id="2c714-105">For example, the web UI for other services that are surfaced through Ambari.</span></span> <span data-ttu-id="2c714-106">Pro plnou funkčnost webovému uživatelskému rozhraní Ambari je nutné použít tunelového propojení SSH do clusteru head.</span><span class="sxs-lookup"><span data-stu-id="2c714-106">For full functionality of the Ambari web UI, you must use an SSH tunnel to the cluster head.</span></span>

## <a name="why-use-an-ssh-tunnel"></a><span data-ttu-id="2c714-107">Proč používat tunelového propojení SSH</span><span class="sxs-lookup"><span data-stu-id="2c714-107">Why use an SSH tunnel</span></span>

<span data-ttu-id="2c714-108">Několik nabídek v Ambari fungovat pouze pomocí tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="2c714-108">Several of the menus in Ambari only work through an SSH tunnel.</span></span> <span data-ttu-id="2c714-109">Tyto nabídky spoléhají na weby a služby běžící na jiné typy uzlů, například uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="2c714-109">These menus rely on web sites and services running on other node types, such as worker nodes.</span></span> <span data-ttu-id="2c714-110">Často není zabezpečená tyto webové servery, takže není bezpečné přímo vystavit je na Internetu.</span><span class="sxs-lookup"><span data-stu-id="2c714-110">Often, these web sites are not secured, so it is not safe to directly expose them on the internet.</span></span>

<span data-ttu-id="2c714-111">Následující uživatelská rozhraní Web vyžadovat tunelového propojení SSH:</span><span class="sxs-lookup"><span data-stu-id="2c714-111">The following Web UIs require an SSH tunnel:</span></span>

* <span data-ttu-id="2c714-112">JobHistory</span><span class="sxs-lookup"><span data-stu-id="2c714-112">JobHistory</span></span>
* <span data-ttu-id="2c714-113">NameNode</span><span class="sxs-lookup"><span data-stu-id="2c714-113">NameNode</span></span>
* <span data-ttu-id="2c714-114">Zásobníky vlákna</span><span class="sxs-lookup"><span data-stu-id="2c714-114">Thread Stacks</span></span>
* <span data-ttu-id="2c714-115">Oozie webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2c714-115">Oozie web UI</span></span>
* <span data-ttu-id="2c714-116">Hlavní HBase a protokoly uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="2c714-116">HBase Master and Logs UI</span></span>

<span data-ttu-id="2c714-117">Pokud pomocí akcí skriptů přizpůsobení vašeho clusteru, vyžadují jakékoli služby nebo nástroje, které nainstalujete, které zveřejňují webového uživatelského rozhraní tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="2c714-117">If you use Script Actions to customize your cluster, any services or utilities that you install that expose a web UI require an SSH tunnel.</span></span> <span data-ttu-id="2c714-118">Například pokud nainstalujete Hue pomocí akce skriptu, musíte použít tunelového propojení SSH pro přístup k webu Hue uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2c714-118">For example, if you install Hue using a Script Action, you must use an SSH tunnel to access the Hue web UI.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c714-119">Pokud máte přímý přístup do HDInsight prostřednictvím virtuální sítě, není potřeba použít tunely SSH.</span><span class="sxs-lookup"><span data-stu-id="2c714-119">If you have direct access to HDInsight through a virtual network, you do not need to use SSH tunnels.</span></span> <span data-ttu-id="2c714-120">Příklad přímý přístup k HDInsight prostřednictvím virtuální sítě, naleznete v části [HDInsight připojit k místní síti](connect-on-premises-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2c714-120">For an example of directly accessing HDInsight through a virtual network, see the [Connect HDInsight to your on-premises network](connect-on-premises-network.md) document.</span></span>

## <a name="what-is-an-ssh-tunnel"></a><span data-ttu-id="2c714-121">Co je tunelového propojení SSH</span><span class="sxs-lookup"><span data-stu-id="2c714-121">What is an SSH tunnel</span></span>

<span data-ttu-id="2c714-122">[Secure Shell (SSH) tunelování](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) směruje provoz posílají do portu na místní pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="2c714-122">[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) routes traffic sent to a port on your local workstation.</span></span> <span data-ttu-id="2c714-123">Přenos je směrován přes připojení SSH pro váš hlavního uzlu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2c714-123">The traffic is routed through an SSH connection to your HDInsight cluster head node.</span></span> <span data-ttu-id="2c714-124">Žádost se vyřeší, jako by bylo provedeno z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="2c714-124">The request is resolved as if it originated on the head node.</span></span> <span data-ttu-id="2c714-125">Odpověď se pak směruje zpět prostřednictvím tunelu do pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="2c714-125">The response is then routed back through the tunnel to your workstation.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c714-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2c714-126">Prerequisites</span></span>

* <span data-ttu-id="2c714-127">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="2c714-127">An SSH client.</span></span> <span data-ttu-id="2c714-128">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2c714-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="2c714-129">Webový prohlížeč, který může být nakonfigurován k používání SOCKS5 proxy.</span><span class="sxs-lookup"><span data-stu-id="2c714-129">A web browser that can be configured to use a SOCKS5 proxy.</span></span>

    > [!WARNING]
    > <span data-ttu-id="2c714-130">Podpora proxy SOCKS součástí systému Windows nepodporuje SOCKS5 a nefunguje s kroky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2c714-130">The SOCKS proxy support built into Windows does not support SOCKS5, and does not work with the steps in this document.</span></span> <span data-ttu-id="2c714-131">Následující prohlížeče závisí na nastavení proxy serveru systému Windows a aktuálně nefungují s kroky v tomto dokumentu:</span><span class="sxs-lookup"><span data-stu-id="2c714-131">The following browsers rely on Windows proxy settings, and do not currently work with the steps in this document:</span></span>
    >
    > * <span data-ttu-id="2c714-132">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="2c714-132">Microsoft Edge</span></span>
    > * <span data-ttu-id="2c714-133">Aplikace Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="2c714-133">Microsoft Internet Explorer</span></span>
    >
    > <span data-ttu-id="2c714-134">Google Chrome také závisí na nastavení proxy serveru systému Windows.</span><span class="sxs-lookup"><span data-stu-id="2c714-134">Google Chrome also relies on the Windows proxy settings.</span></span> <span data-ttu-id="2c714-135">Můžete však nainstalovat rozšíření, které podporují SOCKS5.</span><span class="sxs-lookup"><span data-stu-id="2c714-135">However, you can install extensions that support SOCKS5.</span></span> <span data-ttu-id="2c714-136">Doporučujeme, abyste [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).</span><span class="sxs-lookup"><span data-stu-id="2c714-136">We recommend [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).</span></span>

## <span data-ttu-id="2c714-137"><a name="usessh"></a>Vytvořit tunelové propojení příkazu SSH</span><span class="sxs-lookup"><span data-stu-id="2c714-137"><a name="usessh"></a>Create a tunnel using the SSH command</span></span>

<span data-ttu-id="2c714-138">Použijte následující příkaz pro vytvoření SSH tunelování, pomocí `ssh` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2c714-138">Use the following command to create an SSH tunnel using the `ssh` command.</span></span> <span data-ttu-id="2c714-139">Nahraďte **uživatelské jméno** s uživatelem SSH pro váš HDInsight cluster a nahraďte **CLUSTERNAME** s názvem clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="2c714-139">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with the name of your HDInsight cluster:</span></span>

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

<span data-ttu-id="2c714-140">Tento příkaz vytvoří připojení, který směruje provoz do místního portu 9876 do clusteru prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="2c714-140">This command creates a connection that routes traffic to local port 9876 to the cluster over SSH.</span></span> <span data-ttu-id="2c714-141">Dostupné možnosti:</span><span class="sxs-lookup"><span data-stu-id="2c714-141">The options are:</span></span>

* <span data-ttu-id="2c714-142">**D 9876** -místní port, který směruje provoz prostřednictvím tunelu.</span><span class="sxs-lookup"><span data-stu-id="2c714-142">**D 9876** - The local port that routes traffic through the tunnel.</span></span>
* <span data-ttu-id="2c714-143">**C** -komprimovat všechna data, protože webový provoz je většinou text.</span><span class="sxs-lookup"><span data-stu-id="2c714-143">**C** - Compress all data, because web traffic is mostly text.</span></span>
* <span data-ttu-id="2c714-144">**2** -force SSH pokusit protocol verze 2 pouze.</span><span class="sxs-lookup"><span data-stu-id="2c714-144">**2** - Force SSH to try protocol version 2 only.</span></span>
* <span data-ttu-id="2c714-145">**q** -tichém režimu.</span><span class="sxs-lookup"><span data-stu-id="2c714-145">**q** - Quiet mode.</span></span>
* <span data-ttu-id="2c714-146">**T** -zakázat pseudo tty přidělení, protože jsme se právě předávání port.</span><span class="sxs-lookup"><span data-stu-id="2c714-146">**T** - Disable pseudo-tty allocation, since we are just forwarding a port.</span></span>
* <span data-ttu-id="2c714-147">**n**-Zabránit čtení STDIN, protože jsme se právě předávání port.</span><span class="sxs-lookup"><span data-stu-id="2c714-147">**n** - Prevent reading of STDIN, since we are just forwarding a port.</span></span>
* <span data-ttu-id="2c714-148">**N** -nespouštějte vzdáleného příkazu, protože jsme se právě předávání port.</span><span class="sxs-lookup"><span data-stu-id="2c714-148">**N** - Do not execute a remote command, since we are just forwarding a port.</span></span>
* <span data-ttu-id="2c714-149">**f** -spuštěný na pozadí.</span><span class="sxs-lookup"><span data-stu-id="2c714-149">**f** - Run in the background.</span></span>

<span data-ttu-id="2c714-150">Po dokončení příkazu posílají do portu 9876 v místním počítači provoz se směruje na hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="2c714-150">Once the command finishes, traffic sent to port 9876 on the local computer is routed to the cluster head node.</span></span>

## <span data-ttu-id="2c714-151"><a name="useputty"></a>Vytvořit tunelové propojení PuTTY</span><span class="sxs-lookup"><span data-stu-id="2c714-151"><a name="useputty"></a>Create a tunnel using PuTTY</span></span>

<span data-ttu-id="2c714-152">[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) je grafický klient SSH pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="2c714-152">[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) is a graphical SSH client for Windows.</span></span> <span data-ttu-id="2c714-153">Pro vytvoření tunelu SSH pomocí klienta PuTTY použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="2c714-153">Use the following steps to create an SSH tunnel using PuTTY:</span></span>

1. <span data-ttu-id="2c714-154">Otevřete PuTTY a zadejte informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="2c714-154">Open PuTTY, and enter your connection information.</span></span> <span data-ttu-id="2c714-155">Pokud nejste obeznámeni s PuTTY, najdete v článku [PuTTY dokumentace (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).</span><span class="sxs-lookup"><span data-stu-id="2c714-155">If you are not familiar with PuTTY, see the [PuTTY documentation (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).</span></span>

2. <span data-ttu-id="2c714-156">V **kategorie** nalevo dialogového okna, rozbalte položku **připojení**, rozbalte položku **SSH**a potom vyberte **tunely**.</span><span class="sxs-lookup"><span data-stu-id="2c714-156">In the **Category** section to the left of the dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>

3. <span data-ttu-id="2c714-157">Zadejte následující informace na **možnosti řízení SSH port předávání** formuláře:</span><span class="sxs-lookup"><span data-stu-id="2c714-157">Provide the following information on the **Options controlling SSH port forwarding** form:</span></span>
   
   * <span data-ttu-id="2c714-158">**Zdrojový port** – port na straně klienta, který chcete přesměrovat.</span><span class="sxs-lookup"><span data-stu-id="2c714-158">**Source port** - The port on the client that you wish to forward.</span></span> <span data-ttu-id="2c714-159">Například **9876**.</span><span class="sxs-lookup"><span data-stu-id="2c714-159">For example, **9876**.</span></span>

   * <span data-ttu-id="2c714-160">**Cílový** -SSH adresu pro cluster HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="2c714-160">**Destination** - The SSH address for the Linux-based HDInsight cluster.</span></span> <span data-ttu-id="2c714-161">Například **mycluster-ssh.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="2c714-161">For example, **mycluster-ssh.azurehdinsight.net**.</span></span>

   * <span data-ttu-id="2c714-162">**Dynamicky** – umožňuje dynamické směrování proxy SOCKS.</span><span class="sxs-lookup"><span data-stu-id="2c714-162">**Dynamic** - Enables dynamic SOCKS proxy routing.</span></span>
     
     ![Obrázek možnosti tunelové propojení](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. <span data-ttu-id="2c714-164">Klikněte na tlačítko **přidat** přidejte nastavení a potom klikněte na **otevřete** otevřít připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="2c714-164">Click **Add** to add the settings, and then click **Open** to open an SSH connection.</span></span>

5. <span data-ttu-id="2c714-165">Pokud budete vyzváni, přihlaste se k serveru.</span><span class="sxs-lookup"><span data-stu-id="2c714-165">When prompted, log in to the server.</span></span>

## <a name="use-the-tunnel-from-your-browser"></a><span data-ttu-id="2c714-166">Používání tunelového propojení z prohlížeče</span><span class="sxs-lookup"><span data-stu-id="2c714-166">Use the tunnel from your browser</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c714-167">Postup v této části pomocí prohlížeče Mozilla FireFox, protože poskytuje stejné nastavení proxy serveru ve všech platformách.</span><span class="sxs-lookup"><span data-stu-id="2c714-167">The steps in this section use the Mozilla FireFox browser, as it provides the same proxy settings across all platforms.</span></span> <span data-ttu-id="2c714-168">Jiné moderní prohlížeče, jako je například Google Chrome, může vyžadovat rozšíření například FoxyProxy pro práci s tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="2c714-168">Other modern browsers, such as Google Chrome, may require an extension such as FoxyProxy to work with the tunnel.</span></span>

1. <span data-ttu-id="2c714-169">Prohlížeč nakonfigurovat pro použití **localhost** a port, který jste použili při vytvoření tunelu jako **SOCKS v5** proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="2c714-169">Configure the browser to use **localhost** and the port you used when creating the tunnel as a **SOCKS v5** proxy.</span></span> <span data-ttu-id="2c714-170">Zde je, jak vypadají nastavení Firefox.</span><span class="sxs-lookup"><span data-stu-id="2c714-170">Here's what the Firefox settings look like.</span></span> <span data-ttu-id="2c714-171">Pokud jste použili jiný port než 9876, změňte port, které jste použili:</span><span class="sxs-lookup"><span data-stu-id="2c714-171">If you used a different port than 9876, change the port to the one you used:</span></span>
   
    ![Obrázek nastavení Firefox](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > <span data-ttu-id="2c714-173">Výběr **DNS vzdálené** přeloží požadavky systému DNS (Domain Name) pomocí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2c714-173">Selecting **Remote DNS** resolves Domain Name System (DNS) requests by using the HDInsight cluster.</span></span> <span data-ttu-id="2c714-174">Toto nastavení vyřeší DNS pomocí hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="2c714-174">This setting resolves DNS using the head node of the cluster.</span></span>

2. <span data-ttu-id="2c714-175">Ověřte, že funguje tunelu návštěvou lokality [http://www.whatismyip.com/](http://www.whatismyip.com/).</span><span class="sxs-lookup"><span data-stu-id="2c714-175">Verify that the tunnel works by visiting a site such as [http://www.whatismyip.com/](http://www.whatismyip.com/).</span></span> <span data-ttu-id="2c714-176">Vrácená IP adresa by měl být jeden používané datového centra Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="2c714-176">The IP returned should be one used by the Microsoft Azure datacenter.</span></span>

## <a name="verify-with-ambari-web-ui"></a><span data-ttu-id="2c714-177">Ověřte si u webovému uživatelskému rozhraní Ambari</span><span class="sxs-lookup"><span data-stu-id="2c714-177">Verify with Ambari web UI</span></span>

<span data-ttu-id="2c714-178">Po vytvoření clusteru ověřte, že jste z webu Ambari přístup webové služby uživatelská pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="2c714-178">Once the cluster has been established, use the following steps to verify that you can access service web UIs from the Ambari Web:</span></span>

1. <span data-ttu-id="2c714-179">V prohlížeči přejděte na http://headnodehost:8080.</span><span class="sxs-lookup"><span data-stu-id="2c714-179">In your browser, go to http://headnodehost:8080.</span></span> <span data-ttu-id="2c714-180">`headnodehost` Adresa je odeslána přes tunel ke clusteru a vyřešte k headnode, která Ambari běží na.</span><span class="sxs-lookup"><span data-stu-id="2c714-180">The `headnodehost` address is sent over the tunnel to the cluster and resolve to the headnode that Ambari is running on.</span></span> <span data-ttu-id="2c714-181">Po zobrazení výzvy zadejte uživatelské jméno správce (správce) a heslo pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="2c714-181">When prompted, enter the admin user name (admin) and password for your cluster.</span></span> <span data-ttu-id="2c714-182">Můžete být vyzváni podruhé pomocí Ambari webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2c714-182">You may be prompted a second time by the Ambari web UI.</span></span> <span data-ttu-id="2c714-183">Pokud ano, zadejte informace znovu.</span><span class="sxs-lookup"><span data-stu-id="2c714-183">If so, reenter the information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2c714-184">Při použití adresu http://headnodehost:8080 se připojit ke clusteru, se připojují prostřednictvím tunelu.</span><span class="sxs-lookup"><span data-stu-id="2c714-184">When using the http://headnodehost:8080 address to connect to the cluster, you are connecting through the tunnel.</span></span> <span data-ttu-id="2c714-185">Komunikace je zabezpečena pomocí tunelového propojení SSH místo protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2c714-185">Communication is secured using the SSH tunnel instead of HTTPS.</span></span> <span data-ttu-id="2c714-186">Chcete-li připojit přes internet pomocí protokolu HTTPS, použijte https://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je název clusteru.</span><span class="sxs-lookup"><span data-stu-id="2c714-186">To connect over the internet using HTTPS, use https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of the cluster.</span></span>

2. <span data-ttu-id="2c714-187">Webové uživatelské rozhraní Ambari vyberte ze seznamu na levé straně stránky HDFS.</span><span class="sxs-lookup"><span data-stu-id="2c714-187">From the Ambari Web UI, select HDFS from the list on the left of the page.</span></span>

    ![Bitové kopie s HDFS vybrané](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. <span data-ttu-id="2c714-189">Když se zobrazí informace o službě HDFS, vyberte **rychlé odkazy**.</span><span class="sxs-lookup"><span data-stu-id="2c714-189">When the HDFS service information is displayed, select **Quick Links**.</span></span> <span data-ttu-id="2c714-190">Zobrazí se seznam hlavních uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="2c714-190">A list of the cluster head nodes appears.</span></span> <span data-ttu-id="2c714-191">Vyberte jeden z hlavních uzlech a pak vyberte **uživatelského rozhraní NameNode**.</span><span class="sxs-lookup"><span data-stu-id="2c714-191">Select one of the head nodes, and then select **NameNode UI**.</span></span>

    ![Rozšířit bitové kopie s nabídce Rychlé odkazy](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > <span data-ttu-id="2c714-193">Když vyberete __rychlé odkazy__, může dojít k čekání indikátoru.</span><span class="sxs-lookup"><span data-stu-id="2c714-193">When you select __Quick Links__, you may get a wait indicator.</span></span> <span data-ttu-id="2c714-194">K tomuto stavu může dojít, pokud máte pomalé připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="2c714-194">This condition can occur if you have a slow internet connection.</span></span> <span data-ttu-id="2c714-195">Počkejte minutu nebo dvě pro data, která mají být přijata ze serveru, a akci opakujte seznamu.</span><span class="sxs-lookup"><span data-stu-id="2c714-195">Wait a minute or two for the data to be received from the server, then try the list again.</span></span>
   >
   > <span data-ttu-id="2c714-196">Některé položky v **rychlé odkazy** nabídky, může být zkrácené pravé straně obrazovky.</span><span class="sxs-lookup"><span data-stu-id="2c714-196">Some entries in the **Quick Links** menu may be cut off by the right side of the screen.</span></span> <span data-ttu-id="2c714-197">Pokud ano, rozbalte nabídku pomocí myši a pomocí klávesy šipka doprava přejděte na obrazovce napravo na zobrazení další obrazovky nabídky.</span><span class="sxs-lookup"><span data-stu-id="2c714-197">If so, expand the menu using your mouse and use the right arrow key to scroll the screen to the right to see the rest of the menu.</span></span>

4. <span data-ttu-id="2c714-198">Zobrazí se stránka podobná na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="2c714-198">A page similar to the following image is displayed:</span></span>

    ![Obrázek NameNode uživatelského rozhraní](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > <span data-ttu-id="2c714-200">Všimněte si adresu URL pro tuto stránku; by měl vypadat přibližně **http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/clusteru**.</span><span class="sxs-lookup"><span data-stu-id="2c714-200">Notice the URL for this page; it should be similar to **http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster**.</span></span> <span data-ttu-id="2c714-201">Tento identifikátor URI interní plně kvalifikovaný název domény (FQDN) je pomocí uzlu a je k dispozici pouze při používání tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="2c714-201">This URI is using the internal fully qualified domain name (FQDN) of the node, and is only accessible when using an SSH tunnel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c714-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c714-202">Next steps</span></span>

<span data-ttu-id="2c714-203">Teď, když jste se naučili postup vytvoření a používání tunelového propojení SSH, najdete v následujícím dokumentu pro další způsoby použití Ambari:</span><span class="sxs-lookup"><span data-stu-id="2c714-203">Now that you have learned how to create and use an SSH tunnel, see the following document for other ways to use Ambari:</span></span>

* [<span data-ttu-id="2c714-204">Správa clusterů HDInsight pomocí Ambari</span><span class="sxs-lookup"><span data-stu-id="2c714-204">Manage HDInsight clusters by using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

<span data-ttu-id="2c714-205">Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2c714-205">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

