---
title: "Azure Active Directory Domain Services: Aktualizace nastavení DNS pro hello virtuální síť Azure | Microsoft Docs"
description: "Začínáme se službou Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.openlocfilehash: e6eaff555cb9b7bb89ab7581d8de0b8cfc844529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-preview"></a><span data-ttu-id="f3164-103">Povolení služby Azure Active Directory Domain Services (Preview)</span><span class="sxs-lookup"><span data-stu-id="f3164-103">Enable Azure Active Directory Domain Services (Preview)</span></span>

## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="f3164-104">Úloha 4: aktualizace nastavení DNS pro hello virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="f3164-104">Task 4: update DNS settings for hello Azure virtual network</span></span>
<span data-ttu-id="f3164-105">V předchozích úlohy konfigurace hello povolíte úspěšně Azure Active Directory Domain Services pro svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="f3164-105">In hello preceding configuration tasks, you have successfully enabled Azure Active Directory Domain Services for your directory.</span></span> <span data-ttu-id="f3164-106">Další úlohou Hello je tooensure, můžete počítače v rámci virtuální sítě hello připojit a využívat tyto služby.</span><span class="sxs-lookup"><span data-stu-id="f3164-106">hello next task is tooensure that computers within hello virtual network can connect and consume these services.</span></span> <span data-ttu-id="f3164-107">V tomto článku aktualizace hello nastavení serveru DNS pro vaši virtuální síť toopoint toohello dvě IP adresy kde je k dispozici ve virtuální síti hello Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="f3164-107">In this article, you update hello DNS server settings for your virtual network toopoint toohello two IP addresses where Azure Active Directory Domain Services is available on hello virtual network.</span></span>

<span data-ttu-id="f3164-108">tooupdate hello serveru DNS pro hello virtuální síť, ve kterém jste povolili Azure Active Directory Domain Services dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3164-108">tooupdate hello DNS server setting for hello virtual network in which you have enabled Azure Active Directory Domain Services, complete hello following steps:</span></span>

1. <span data-ttu-id="f3164-109">Hello **přehled** karta obsahuje sadu **požadované kroky konfigurace** toobe provést po vaší spravované domény plně zřízený.</span><span class="sxs-lookup"><span data-stu-id="f3164-109">hello **Overview** tab lists a set of **Required configuration steps** toobe performed after your managed domain is fully provisioned.</span></span> <span data-ttu-id="f3164-110">prvním krokem konfigurace Hello je **nastavení serveru DNS aktualizace pro vaši virtuální síť**.</span><span class="sxs-lookup"><span data-stu-id="f3164-110">hello first configuration step is **Update DNS server settings for your virtual network**.</span></span>

    ![Domain Services – Karta Přehled po úplném zřízení](./media/getting-started/domain-services-provisioned-overview.png)

2. <span data-ttu-id="f3164-112">Když je doména úplně zřízená, zobrazují se na této dlaždici dvě IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f3164-112">When your domain is fully provisioned, two IP addresses are displayed in this tile.</span></span> <span data-ttu-id="f3164-113">Každý z těchto IP adres představuje řadič vaší spravované domény.</span><span class="sxs-lookup"><span data-stu-id="f3164-113">Each of these IP addresses represents a domain controller for your managed domain.</span></span>

3. <span data-ttu-id="f3164-114">toocopy hello první IP adresa tooclipboard, klikněte na další tooit tlačítko hello kopírování.</span><span class="sxs-lookup"><span data-stu-id="f3164-114">toocopy hello first IP address tooclipboard, click hello copy button next tooit.</span></span> <span data-ttu-id="f3164-115">Pak klikněte na tlačítko hello **servery DNS nakonfigurujte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3164-115">Then click hello **Configure DNS servers** button.</span></span>

4. <span data-ttu-id="f3164-116">Vložte hello první IP adresu do hello **server DNS přidat** textového pole v hello **servery DNS** okno.</span><span class="sxs-lookup"><span data-stu-id="f3164-116">Paste hello first IP address into hello **Add DNS server** textbox in hello **DNS servers** blade.</span></span> <span data-ttu-id="f3164-117">Posuňte vodorovně toohello zbývajících toocopy hello druhou IP adresu a vložte jej do hello **server DNS přidat** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f3164-117">Scroll horizontally toohello left toocopy hello second IP address and paste it into hello **Add DNS server** textbox.</span></span>

    ![Domain Services – Aktualizace DNS](./media/getting-started/domain-services-update-dns.png)

5. <span data-ttu-id="f3164-119">Klikněte na tlačítko **Uložit** po dokončení tooupdate hello servery DNS pro virtuální síť hello.</span><span class="sxs-lookup"><span data-stu-id="f3164-119">Click **Save** when you are done tooupdate hello DNS servers for hello virtual network.</span></span>

> [!NOTE]
> <span data-ttu-id="f3164-120">Virtuální počítače v síti hello získat nové nastavení DNS hello pouze po restartu.</span><span class="sxs-lookup"><span data-stu-id="f3164-120">Virtual machines in hello network only get hello new DNS settings after a restart.</span></span> <span data-ttu-id="f3164-121">Chcete-li nastavení DNS tooget hello aktualizovat hned, aktivovat restartování hello portál, prostředí PowerShell nebo hello rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f3164-121">If you need them tooget hello updated DNS settings right away, trigger a restart either by hello portal, PowerShell, or hello CLI.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="f3164-122">Další krok</span><span class="sxs-lookup"><span data-stu-id="f3164-122">Next step</span></span>
[<span data-ttu-id="f3164-123">Úloha 5: povolení tooAzure synchronizace hesla Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="f3164-123">Task 5: enable password synchronization tooAzure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-password-sync.md)
