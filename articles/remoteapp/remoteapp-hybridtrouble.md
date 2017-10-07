---
title: "Vytvoření hybridní kolekce vzdálené aplikace RemoteApp aaaTroubleshoot | Microsoft Docs"
description: "Zjistěte, jak selhání vytvoření tootroubleshoot vzdálené aplikace RemoteApp hybridní kolekce"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: b32033ee-8d52-4e74-bb78-86ca873c34e2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: cc426f24bd0c349a8862d54acbafa9cf84446f4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a><span data-ttu-id="bbdf0-103">Řešení potíží s vytvářením hybridních kolekcí Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="bbdf0-103">Troubleshoot creating Azure RemoteApp hybrid collections</span></span>
> [!IMPORTANT]
> <span data-ttu-id="bbdf0-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="bbdf0-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="bbdf0-106">Hybridní kolekce je hostován v a ukládá data v hello cloudu Azure, ale také umožňuje uživatelům přistupovat k datům a prostředkům uloženým v místní síti.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-106">A hybrid collection is hosted in and stores data in hello Azure cloud but also lets users access data and resources stored on your local network.</span></span> <span data-ttu-id="bbdf0-107">Uživatelé mohou získat přístup k aplikacím přihlášením pomocí svých podnikových přihlašovacích údajů synchronizovaných nebo sdružených se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-107">Users can access apps by logging in with their corporate credentials synchronized or federated with Azure Active Directory.</span></span> <span data-ttu-id="bbdf0-108">Můžete nasadit hybridní kolekci, která použije existující virtuální síť Azure, nebo můžete vytvořit novou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-108">You can deploy a hybrid collection that uses an existing Azure Virtual Network, or you can create a new virtual network.</span></span> <span data-ttu-id="bbdf0-109">Doporučujeme vytvořit nebo použít podsíť virtuální sítě s rozsahem CIDR dostatečně velký pro očekávané budoucímu růstu pro Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-109">We recommend that you create or use a virtual network subnet with a CIDR range large enough for expected future growth for Azure RemoteApp.</span></span>

<span data-ttu-id="bbdf0-110">Vaší kolekci ještě nevytvořili?</span><span class="sxs-lookup"><span data-stu-id="bbdf0-110">Haven't created your collection yet?</span></span> <span data-ttu-id="bbdf0-111">V tématu [vytvoření hybridní kolekce](remoteapp-create-hybrid-deployment.md) hello kroky.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-111">See [Create a hybrid collection](remoteapp-create-hybrid-deployment.md) for hello steps.</span></span>

<span data-ttu-id="bbdf0-112">Pokud máte potíže při vytvoření kolekce, nebo pokud hello kolekce nepracuje hello způsob, jak si myslíte, že by měl, podívejte se na hello následující informace.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-112">If you are having trouble creating your collection, or if hello collection isn't working hello way you think it should, check out hello following information.</span></span>

## <a name="your-image-is-invalid"></a><span data-ttu-id="bbdf0-113">Bitové kopie je neplatný</span><span class="sxs-lookup"><span data-stu-id="bbdf0-113">Your image is invalid</span></span>
<span data-ttu-id="bbdf0-114">Pokud při čekání Azure tooprovision kolekce se zobrazí zpráva podobná "GoldImageInvalid", znamená to, že image šablony nesplňuje hello [definované požadavky na image](remoteapp-imagereqs.md).</span><span class="sxs-lookup"><span data-stu-id="bbdf0-114">If you see a message like, "GoldImageInvalid" when you are waiting for Azure tooprovision your collection, it means that your template image doesn't meet hello [defined image requirements](remoteapp-imagereqs.md).</span></span> <span data-ttu-id="bbdf0-115">Ano, přejděte číst ty [požadavky](remoteapp-imagereqs.md)opravte bitové kopie a akci toocreate kolekci znovu.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-115">So, go read those [requirements](remoteapp-imagereqs.md), fix your image, and try toocreate your collection again.</span></span>

## <a name="does-your-vnet-have-network-security-groups-defined"></a><span data-ttu-id="bbdf0-116">Má vaše virtuální sítě definované skupiny zabezpečení sítě?</span><span class="sxs-lookup"><span data-stu-id="bbdf0-116">Does your VNET have network security groups defined?</span></span>
<span data-ttu-id="bbdf0-117">Pokud máte skupiny zabezpečení sítě definované v podsíti hello používáte pro svoji kolekci, ujistěte se, tyto [adres URL a portů](remoteapp-ports.md) jsou k dispozici v rámci dané podsítě.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-117">If you have network security groups defined on hello subnet you are using for your collection, make sure these [URLs and ports](remoteapp-ports.md) are accessible from within your subnet.</span></span>

<span data-ttu-id="bbdf0-118">Můžete přidat další síťové zabezpečení skupiny toohello nasazené virtuální počítače od vás v hello podsíť pro náročnější ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-118">You can add additional network security groups toohello VMs deployed by you in hello subnet for tighter control.</span></span>

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a><span data-ttu-id="bbdf0-119">Používáte vlastní servery DNS?</span><span class="sxs-lookup"><span data-stu-id="bbdf0-119">Are you using your own DNS servers?</span></span> <span data-ttu-id="bbdf0-120">A že jsou přístupné z podsíť virtuální sítě?</span><span class="sxs-lookup"><span data-stu-id="bbdf0-120">And are they accessible from your VNET subnet?</span></span>
> [!NOTE]
> <span data-ttu-id="bbdf0-121">Máte toomake zda hello, servery DNS ve vaší virtuální síti jsou vždy nahoru a vždy možné tooresolve hello virtuální počítače hostované v hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-121">You have toomake sure hello DNS servers in your VNET are always up and always able tooresolve hello virtual machines hosted in hello VNET.</span></span> <span data-ttu-id="bbdf0-122">Nepoužívejte Google DNS pro tento.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-122">Don't use Google DNS for this.</span></span>
> 
> 

<span data-ttu-id="bbdf0-123">Pro hybridní kolekce použijete vlastní servery DNS.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-123">For hybrid collections you use your own DNS servers.</span></span> <span data-ttu-id="bbdf0-124">Můžete je zadat ve schématu konfigurace vaší sítě nebo prostřednictvím portálu pro správu hello při vytváření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-124">You specify them in your network configuration schema or through hello management portal when you create your virtual network.</span></span> <span data-ttu-id="bbdf0-125">Servery DNS se používají v pořadí hello, že jsou zadané způsobem převzetí služeb při selhání (jako názvem na rozdíl od tooround robin).</span><span class="sxs-lookup"><span data-stu-id="bbdf0-125">DNS servers are used in hello order that they are specified in a failover manner (as opposed tooround robin).</span></span>  
<span data-ttu-id="bbdf0-126">Naleznete příliš[překlad názvů pro virtuální počítače a instance rolí](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) toomake se vaše servery DNS jsou nakonfigurované correcly.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-126">Please refer too[Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) toomake sure your DNS servers are configured correcly.</span></span>

<span data-ttu-id="bbdf0-127">Ujistěte se, že nejsou servery DNS hello kolekce přístupný a dostupný z podsítě virtuální sítě hello, které zadáte pro tuto kolekci.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-127">Make sure hello DNS servers for your collection are accessible and available from hello VNET subnet you specified for this collection.</span></span>

<span data-ttu-id="bbdf0-128">Například:</span><span class="sxs-lookup"><span data-stu-id="bbdf0-128">For example:</span></span>

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definování DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a><span data-ttu-id="bbdf0-130">Používáte řadič domény služby Active Directory v kolekci?</span><span class="sxs-lookup"><span data-stu-id="bbdf0-130">Are you using an Active Directory domain controller in your collection?</span></span>
<span data-ttu-id="bbdf0-131">Aktuálně jen jednu doménu služby Active Directory může být přidružen Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-131">Currently only one Active Directory domain can be associated with Azure RemoteApp.</span></span> <span data-ttu-id="bbdf0-132">Hello hybridní kolekce podporuje pouze účty Azure Active Directory, které byly synchronizovány pomocí nástroje DirSync z nasazení systému Windows Server Active Directory; Konkrétně buď synchronizované s možností synchronizace hesel hello nebo synchronizaci se službou federation Active Directory Federation Services (AD FS) nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-132">hello hybrid collection supports only Azure Active Directory accounts that have been synced using DirSync tool from a Windows Server Active Directory deployment; specifically, either synced with hello Password Synchronization option or synced with Active Directory Federation Services (AD FS) federation configured.</span></span> <span data-ttu-id="bbdf0-133">Potřebujete toocreate vlastní doménu, která odpovídá příponu domény hello UPN pro místní doméně a nastavení integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-133">You need toocreate a custom domain that matches hello UPN domain suffix for your on-premises domain and set up directory integration.</span></span>

<span data-ttu-id="bbdf0-134">V tématu [konfigurace služby Active Directory pro Azure RemoteApp](remoteapp-ad.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-134">See [Configuring Active Directory for Azure RemoteApp](remoteapp-ad.md) for more information.</span></span>

<span data-ttu-id="bbdf0-135">Zajistěte, aby podrobnosti hello domény zadali jsou platné a je hello řadič domény dostupný z hello vytvoření virtuálního počítače v podsíti hello používá pro vzdálenou aplikaci Azure.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-135">Make sure hello domain details provided are valid and hello domain controller is reachable from hello VM created in hello subnet used for Azure Remote App.</span></span> <span data-ttu-id="bbdf0-136">Také zkontrolujte, zda účet služby hello pověření mít oprávnění tooadd počítače toohello zadaný domény a zda text hello název Active Directory zadaný lze vyřešit z hello DNS součástí hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-136">Also make sure hello service account credentials supplied have permissions tooadd computers toohello provided domain and that hello AD name provided can be resolved from hello DNS provided in hello VNET.</span></span>

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a><span data-ttu-id="bbdf0-137">Zadejte název domény byl zadán při vytváření kolekce?</span><span class="sxs-lookup"><span data-stu-id="bbdf0-137">What domain name did you specify when you created your collection?</span></span>
<span data-ttu-id="bbdf0-138">název domény Hello vytvořit nebo přidat musí být název interní domény (není název domény služby Azure AD) a musí být ve formátu DNS je možné přeložit (contoso.local).</span><span class="sxs-lookup"><span data-stu-id="bbdf0-138">hello domain name you created or added must be an internal domain name (not your Azure AD domain name) and must be in resolvable DNS format (contoso.local).</span></span> <span data-ttu-id="bbdf0-139">Například máte interní název služby Active Directory (contoso.local) a Active Directory UPN (contoso.com) - máte toouse hello interní název při vytváření kolekce.</span><span class="sxs-lookup"><span data-stu-id="bbdf0-139">For example, you have an Active Directory internal name (contoso.local) and an Active Directory UPN (contoso.com) - you have toouse hello internal name when you create your collection.</span></span>

