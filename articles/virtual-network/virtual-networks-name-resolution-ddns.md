---
title: "aaaUsing hostnames tooregister dynamického DNS"
description: "Tato stránka obsahuje údaje o tom, tooset až dynamického DNS tooregister názvy hostitelů v serverech DNS."
services: dns
documentationcenter: na
author: GarethBradshawMSFT
manager: timlt
editor: 
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: garbrad
ms.openlocfilehash: 8d4b44265714e6976f26bfb3446e8101aa70996a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-dynamic-dns-tooregister-hostnames-in-your-own-dns-server"></a><span data-ttu-id="e8832-103">Použití dynamického DNS tooregister názvy hostitelů v serveru DNS</span><span class="sxs-lookup"><span data-stu-id="e8832-103">Using Dynamic DNS tooregister hostnames in your own DNS server</span></span>
<span data-ttu-id="e8832-104">[Azure poskytuje překlad](virtual-networks-name-resolution-for-vms-and-role-instances.md) pro virtuální počítače (VM) a instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="e8832-104">[Azure provides name resolution](virtual-networks-name-resolution-for-vms-and-role-instances.md) for virtual machines (VMs) and role instances.</span></span> <span data-ttu-id="e8832-105">Ale pokud vaše překlad musí přejít nad rámec těch, které poskytuje Azure, můžete zadat vlastní servery DNS.</span><span class="sxs-lookup"><span data-stu-id="e8832-105">However, when your name resolution needs go beyond those provided by Azure, you can provide your own DNS servers.</span></span> <span data-ttu-id="e8832-106">Tato poskytuje hello power tootailor vaše řešení toosuit DNS specifické potřeby.</span><span class="sxs-lookup"><span data-stu-id="e8832-106">This gives you hello power tootailor your DNS solution toosuit your own specific needs.</span></span> <span data-ttu-id="e8832-107">Například může být nutné tooaccess místních prostředků pomocí řadiče domény služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e8832-107">For example, you may need tooaccess on-premises resources via your Active Directory domain controller.</span></span>

<span data-ttu-id="e8832-108">Pokud vaše vlastní servery DNS jsou hostované jako virtuální počítače Azure, které může předávat název hostitele dotazuje na hello stejné názvy hostitelů tooresolve tooAzure virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e8832-108">When your custom DNS servers are hosted as Azure VMs you can forward hostname queries for hello same vnet tooAzure tooresolve hostnames.</span></span> <span data-ttu-id="e8832-109">Pokud nechcete, aby toouse této trase, můžete zaregistrovat vaše názvy hostitelů virtuálních počítačů v serveru DNS pomocí dynamického DNS.</span><span class="sxs-lookup"><span data-stu-id="e8832-109">If you do not wish toouse this route, you can register your VM hostnames in your DNS server using Dynamic DNS.</span></span>  <span data-ttu-id="e8832-110">Azure nemá toodirectly možnost (například přihlašovací údaje) hello alternativní uspořádání často je třeba vytvořit záznamy v vaše servery DNS.</span><span class="sxs-lookup"><span data-stu-id="e8832-110">Azure doesn't have hello ability (e.g. credentials) toodirectly create records in your DNS servers, so alternative arrangements are often needed.</span></span> <span data-ttu-id="e8832-111">Tady jsou některé běžné scénáře s alternativy.</span><span class="sxs-lookup"><span data-stu-id="e8832-111">Here are some common scenarios with alternatives.</span></span>

## <a name="windows-clients"></a><span data-ttu-id="e8832-112">Klienti systému Windows</span><span class="sxs-lookup"><span data-stu-id="e8832-112">Windows clients</span></span>
<span data-ttu-id="e8832-113">Klienty připojené k jiné doméně systému Windows pokus zabezpečená aktualizace dynamického DNS (DDNS), když se budou spouštět nebo když se změní své IP adresy.</span><span class="sxs-lookup"><span data-stu-id="e8832-113">Non-domain-joined Windows clients attempt unsecured Dynamic DNS (DDNS) updates when they boot or when their IP address changes.</span></span> <span data-ttu-id="e8832-114">název DNS Hello je název hostitele hello plus hello primární příponu DNS.</span><span class="sxs-lookup"><span data-stu-id="e8832-114">hello DNS name is hello hostname plus hello primary DNS suffix.</span></span> <span data-ttu-id="e8832-115">Azure zůstane prázdné hello primární příponu DNS, ale to můžete nastavit v hello virtuálních počítačů, prostřednictvím hello [uživatelského rozhraní](https://technet.microsoft.com/library/cc794784.aspx) nebo [pomocí automatizace](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).</span><span class="sxs-lookup"><span data-stu-id="e8832-115">Azure leaves hello primary DNS suffix blank, but you can set this in hello VM, via hello [UI](https://technet.microsoft.com/library/cc794784.aspx) or [by using automation](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).</span></span>

<span data-ttu-id="e8832-116">Klienty připojené k doméně systému Windows registrovat IP adresy s řadičem domény hello pomocí zabezpečené dynamické DNS.</span><span class="sxs-lookup"><span data-stu-id="e8832-116">Domain-joined Windows clients register their IP addresses with hello domain controller by using secure Dynamic DNS.</span></span> <span data-ttu-id="e8832-117">Proces připojení k doméně Hello nastaví hello primární příponu DNS na klientovi hello a vytvoří a udržuje hello vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="e8832-117">hello domain-join process sets hello primary DNS suffix on hello client and creates and maintains hello trust relationship.</span></span>

## <a name="linux-clients"></a><span data-ttu-id="e8832-118">Klienti Linux</span><span class="sxs-lookup"><span data-stu-id="e8832-118">Linux clients</span></span>
<span data-ttu-id="e8832-119">Klienti Linux obecně nezaregistrujete sami hello serveru DNS při spuštění, se předpokládá, že hello DHCP server dělá.</span><span class="sxs-lookup"><span data-stu-id="e8832-119">Linux clients generally don't register themselves with hello DNS server on startup, they assume hello DHCP server does it.</span></span> <span data-ttu-id="e8832-120">Servery DHCP Azure nemají možnost hello nebo přihlašovací údaje tooregister záznamy v serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="e8832-120">Azure's DHCP servers do not have hello ability or credentials tooregister records in your DNS server.</span></span>  <span data-ttu-id="e8832-121">Můžete použít nástroj nazvaný *nsupdate*, který je součástí balíčku hello vazby, aktualizuje toosend dynamického DNS.</span><span class="sxs-lookup"><span data-stu-id="e8832-121">You can use a tool called *nsupdate*, which is included in hello Bind package, toosend Dynamic DNS updates.</span></span> <span data-ttu-id="e8832-122">Protože je standardizovaný hello protokol dynamické DNS, můžete použít *nsupdate* i když nepoužíváte vazby na serveru DNS hello.</span><span class="sxs-lookup"><span data-stu-id="e8832-122">Because hello Dynamic DNS protocol is standardized, you can use *nsupdate* even when you're not using Bind on hello DNS server.</span></span>

<span data-ttu-id="e8832-123">Můžete použít hello háky, které jsou poskytovány toocreate klienta DHCP hello a udržovat hello položka názvu hostitele v serveru DNS hello.</span><span class="sxs-lookup"><span data-stu-id="e8832-123">You can use hello hooks that are provided by hello DHCP client toocreate and maintain hello hostname entry in hello DNS server.</span></span> <span data-ttu-id="e8832-124">Během cyklu hello DHCP, spustí klienta hello hello skripty v */etc/dhcp/dhclient-exit-hooks.d/*.</span><span class="sxs-lookup"><span data-stu-id="e8832-124">During hello DHCP cycle, hello client executes hello scripts in */etc/dhcp/dhclient-exit-hooks.d/*.</span></span> <span data-ttu-id="e8832-125">To může být použit tooregister hello novou IP adresu pomocí *nsupdate*.</span><span class="sxs-lookup"><span data-stu-id="e8832-125">This can be used tooregister hello new IP address by using *nsupdate*.</span></span> <span data-ttu-id="e8832-126">Například:</span><span class="sxs-lookup"><span data-stu-id="e8832-126">For example:</span></span>

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on hello primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        
        

<span data-ttu-id="e8832-127">Můžete taky hello *nsupdate* tooperform příkaz DNS zabezpečené dynamické aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e8832-127">You can also use hello *nsupdate* command tooperform secure Dynamic DNS updates.</span></span> <span data-ttu-id="e8832-128">Například pokud používáte server DNS vytvořit vazbu, páru veřejného a privátního klíče RSA je [generované](http://linux.yyz.us/nsupdate/).</span><span class="sxs-lookup"><span data-stu-id="e8832-128">For example, when you're using a Bind DNS server, a public-private key pair is [generated](http://linux.yyz.us/nsupdate/).</span></span>  <span data-ttu-id="e8832-129">je Hello DNS server [nakonfigurované](http://linux.yyz.us/dns/ddns-server.html) s hello veřejné součástí klíče hello tak to můžete ověřit podpis hello u hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="e8832-129">hello DNS server is [configured](http://linux.yyz.us/dns/ddns-server.html) with hello public part of hello key so that it can verify hello signature on hello request.</span></span> <span data-ttu-id="e8832-130">Je nutné použít hello *-k* tooprovide možnost hello dvojice klíč příliš*nsupdate* aby hello požadavek toobe podepsané aktualizace dynamického DNS.</span><span class="sxs-lookup"><span data-stu-id="e8832-130">You must use hello *-k* option tooprovide hello key-pair too*nsupdate* in order for hello Dynamic DNS update request toobe signed.</span></span>

<span data-ttu-id="e8832-131">Pokud používáte server DNS systému Windows, můžete použít ověřování pomocí protokolu Kerberos s hello *-g* parametr v *nsupdate* (není k dispozici ve verzi Windows hello *nsupdate* ).</span><span class="sxs-lookup"><span data-stu-id="e8832-131">When you're using a Windows DNS server, you can use Kerberos authentication with hello *-g* parameter in *nsupdate* (not available in hello Windows version of *nsupdate*).</span></span> <span data-ttu-id="e8832-132">toodo tuto, použijte *kinit* tooload hello přihlašovací údaje (například z [keytab souboru](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)).</span><span class="sxs-lookup"><span data-stu-id="e8832-132">toodo this, use *kinit* tooload hello credentials (e.g. from a [keytab file](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)).</span></span> <span data-ttu-id="e8832-133">Potom *nsupdate -g* se vyzvedávat hello přihlašovací údaje z mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="e8832-133">Then *nsupdate -g* will pick up hello credentials from hello cache.</span></span>

<span data-ttu-id="e8832-134">V případě potřeby můžete přidat tooyour přípon vyhledávání DNS virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e8832-134">If needed, you can add a DNS search suffix tooyour VMs.</span></span> <span data-ttu-id="e8832-135">přípona DNS Hello je uveden v hello */etc/resolv.conf* souboru.</span><span class="sxs-lookup"><span data-stu-id="e8832-135">hello DNS suffix is specified in hello */etc/resolv.conf* file.</span></span> <span data-ttu-id="e8832-136">Většina distribucích systému Linux automaticky spravovat hello obsah tohoto souboru, takže obvykle nelze jej upravovat.</span><span class="sxs-lookup"><span data-stu-id="e8832-136">Most Linux distros automatically manage hello content of this file, so usually you can't edit it.</span></span> <span data-ttu-id="e8832-137">Přípona hello však můžete přepsat pomocí klienta DHCP hello *nahrazují* příkaz.</span><span class="sxs-lookup"><span data-stu-id="e8832-137">However, you can override hello suffix by using hello DHCP client's *supersede* command.</span></span> <span data-ttu-id="e8832-138">toodo to v */etc/dhcp/dhclient.conf*, přidejte:</span><span class="sxs-lookup"><span data-stu-id="e8832-138">toodo this, in */etc/dhcp/dhclient.conf*, add:</span></span>

        supersede domain-name <required-dns-suffix>;

