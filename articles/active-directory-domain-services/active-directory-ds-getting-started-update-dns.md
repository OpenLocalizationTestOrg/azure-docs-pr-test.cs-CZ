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
ms.openlocfilehash: 484ff1a197a651bccb2b416448056acf69b0d8c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="53f85-103">Aktualizace nastavení DNS pro hello virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="53f85-103">Update DNS settings for hello Azure virtual network</span></span>
## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="53f85-104">Úloha 4: Aktualizace nastavení DNS pro hello virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="53f85-104">Task 4: Update DNS settings for hello Azure virtual network</span></span>
<span data-ttu-id="53f85-105">V předchozích úlohy konfigurace hello povolíte úspěšně Azure Active Directory Domain Services pro svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="53f85-105">In hello preceding configuration tasks, you have successfully enabled Azure Active Directory Domain Services for your directory.</span></span> <span data-ttu-id="53f85-106">Další úlohou Hello je tooensure, můžete počítače v rámci virtuální sítě hello připojit a využívat tyto služby.</span><span class="sxs-lookup"><span data-stu-id="53f85-106">hello next task is tooensure that computers within hello virtual network can connect and consume these services.</span></span> <span data-ttu-id="53f85-107">V tomto článku aktualizace hello nastavení serveru DNS pro vaši virtuální síť toopoint toohello dvě IP adresy kde je k dispozici ve virtuální síti hello Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="53f85-107">In this article, you update hello DNS server settings for your virtual network toopoint toohello two IP addresses where Azure Active Directory Domain Services is available on hello virtual network.</span></span>

> [!NOTE]
> <span data-ttu-id="53f85-108">Poté, co jste povolili Azure Active Directory Domain Services pro adresář hello, Všimněte si hello IP adresy pro Azure Active Directory Domain Services, zobrazí se na hello **konfigurace** svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="53f85-108">After you've enabled Azure Active Directory Domain Services for hello directory, note hello IP addresses for Azure Active Directory Domain Services that are displayed on hello **Configure** tab of your directory.</span></span>
>
>

<span data-ttu-id="53f85-109">tooupdate hello serveru DNS pro hello virtuální síť, ve kterém jste povolili Azure Active Directory Domain Services dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="53f85-109">tooupdate hello DNS server setting for hello virtual network in which you have enabled Azure Active Directory Domain Services, complete hello following steps:</span></span>

1. <span data-ttu-id="53f85-110">Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="53f85-110">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="53f85-111">V levém podokně hello vyberte **sítě**.</span><span class="sxs-lookup"><span data-stu-id="53f85-111">In hello left pane, select **Networks**.</span></span>  
    <span data-ttu-id="53f85-112">Hello **sítě** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="53f85-112">hello **Networks** window opens.</span></span>

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. <span data-ttu-id="53f85-114">Na hello **virtuální sítě** karty, vyberte hello virtuální síť, ve kterém jste povolili službu Azure Active Directory Domain Services tooview jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="53f85-114">On hello **Virtual Networks** tab, select hello virtual network in which you enabled Azure Active Directory Domain Services tooview its properties.</span></span>
4. <span data-ttu-id="53f85-115">Klikněte na tlačítko hello **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="53f85-115">Click hello **Configure** tab.</span></span>

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. <span data-ttu-id="53f85-117">V hello **servery DNS** zadejte obě hello IP adresy, které byly zobrazeny v hello **Domain Services** část hello **konfigurace** svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="53f85-117">In hello **DNS servers** section, enter both of hello IP addresses that were displayed in hello **Domain Services** section on hello **Configure** tab of your directory.</span></span>
6. <span data-ttu-id="53f85-118">Klikněte na tlačítko toosave hello nastavení serveru DNS této virtuální sítě, v podokně úloh hello v hello dolní části okna hello **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="53f85-118">toosave hello DNS server settings for this virtual network, in hello task pane at hello bottom of hello window, click **Save**.</span></span>

   ![Aktualizovat nastavení serveru DNS hello hello virtuální sítě](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  <span data-ttu-id="53f85-120">Virtuální počítače v síti hello získat nové nastavení DNS hello pouze po restartu.</span><span class="sxs-lookup"><span data-stu-id="53f85-120">Virtual machines in hello network only get hello new DNS settings after a restart.</span></span> <span data-ttu-id="53f85-121">Chcete-li nastavení DNS tooget hello aktualizovat hned, aktivovat restartování hello portál, prostředí PowerShell nebo hello rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="53f85-121">If you need them tooget hello updated DNS settings right away, trigger a restart either by hello portal, PowerShell, or hello CLI.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="53f85-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53f85-122">Next steps</span></span>
<span data-ttu-id="53f85-123">Úloha 5: [povolit tooAzure synchronizace hesla Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="53f85-123">Task 5: [Enable password synchronization tooAzure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)</span></span>
