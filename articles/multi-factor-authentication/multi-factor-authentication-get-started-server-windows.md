---
title: "aaaWindows ověřování a Azure MFA serveru | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, která vám pomůže při nasazení ověření Windows a serveru Azure Multi-Factor Authentication Server."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 19a4043f-c4ce-43c0-80e7-2548ee92cb74
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 0fc38fd751966bf883d4eae7c48055988922af80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a><span data-ttu-id="6b916-103">Ověření Windows a server Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="6b916-103">Windows Authentication and Azure Multi-Factor Authentication Server</span></span>
<span data-ttu-id="6b916-104">Použijte část ověřování systému Windows hello tooenable Azure Multi-Factor Authentication Server hello a konfigurovat ověřování systému Windows pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="6b916-104">Use hello Windows Authentication section of hello Azure Multi-Factor Authentication Server tooenable and configure Windows authentication for applications.</span></span> <span data-ttu-id="6b916-105">Před nastavením ověřování systému Windows, Udržovat hello seznamu na paměti následující:</span><span class="sxs-lookup"><span data-stu-id="6b916-105">Before you set up Windows Authentication, keep hello following list in mind:</span></span>

* <span data-ttu-id="6b916-106">Po dokončení instalace restartování hello Azure Multi-Factor Authentication pro Terminálové služby tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="6b916-106">After setup, reboot hello Azure Multi-Factor Authentication for Terminal Services tootake effect.</span></span>
* <span data-ttu-id="6b916-107">Pokud je zaškrtnuta možnost 'shodu uživatele vyžadují ověřování Azure Multi-Factor Authentication, a nejsou v seznamu uživatelů hello, nebudete moct toolog do počítače hello po restartování.</span><span class="sxs-lookup"><span data-stu-id="6b916-107">If ‘Require Azure Multi-Factor Authentication user match’ is checked and you are not in hello user list, you will not be able toolog into hello machine after reboot.</span></span>
* <span data-ttu-id="6b916-108">Důvěryhodné že IP adresy jsou závislé na tom, zda text hello aplikace může poskytovat hello IP adresu klienta s ověřováním hello.</span><span class="sxs-lookup"><span data-stu-id="6b916-108">Trusted IPs is dependent on whether hello application can provide hello client IP with hello authentication.</span></span> <span data-ttu-id="6b916-109">Momentálně jsou podporovány pouze terminálové služby.</span><span class="sxs-lookup"><span data-stu-id="6b916-109">Currently only Terminal Services is supported.</span></span>  

> [!NOTE]
> <span data-ttu-id="6b916-110">Tato funkce není podporovaná toosecure Terminálové služby v systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="6b916-110">This feature is not supported toosecure Terminal Services on Windows Server 2012 R2.</span></span>

## <a name="toosecure-an-application-with-windows-authentication-use-hello-following-procedure"></a><span data-ttu-id="6b916-111">toosecure aplikaci pomocí ověřování systému Windows hello použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="6b916-111">toosecure an application with Windows Authentication, use hello following procedure.</span></span>
1. <span data-ttu-id="6b916-112">V hello Azure Multi-Factor Authentication serveru klikněte na ikonu ověřování Windows hello.</span><span class="sxs-lookup"><span data-stu-id="6b916-112">In hello Azure Multi-Factor Authentication Server click hello Windows Authentication icon.</span></span>
   <span data-ttu-id="6b916-113">![Ověřování systému Windows](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span><span class="sxs-lookup"><span data-stu-id="6b916-113">![Windows Authentication](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span></span>
2. <span data-ttu-id="6b916-114">Zkontrolujte hello **povolit ověřování systému Windows** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="6b916-114">Check hello **Enable Windows Authentication** checkbox.</span></span> <span data-ttu-id="6b916-115">Ve výchozím nastavení je toto políčko zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="6b916-115">By default, this box is unchecked.</span></span>
3. <span data-ttu-id="6b916-116">na kartě aplikace Hello může správce tooconfigure hello jeden nebo více aplikací pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6b916-116">hello Applications tab allows hello administrator tooconfigure one or more applications for Windows Authentication.</span></span>
4. <span data-ttu-id="6b916-117">Vyberte server nebo aplikaci – určete, zda hello server/aplikace povolena.</span><span class="sxs-lookup"><span data-stu-id="6b916-117">Select a server or application – specify whether hello server/application is enabled.</span></span> <span data-ttu-id="6b916-118">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b916-118">Click **OK**.</span></span>
5. <span data-ttu-id="6b916-119">Klikněte na **Přidat...**</span><span class="sxs-lookup"><span data-stu-id="6b916-119">Click **Add…**</span></span>
6. <span data-ttu-id="6b916-120">Karta Důvěryhodné IP adresy Hello vám umožní tooskip Azure vícefaktorového ověřování pro relace systému Windows pocházející z konkrétních IP adres.</span><span class="sxs-lookup"><span data-stu-id="6b916-120">hello Trusted IPs tab allows you tooskip Azure Multi-Factor Authentication for Windows sessions originating from specific IPs.</span></span> <span data-ttu-id="6b916-121">Například pokud zaměstnanci používají aplikace hello z hello kanceláře i z domova, můžete rozhodnout, že nechcete, aby jejich telefony vyzváněly pro ověřování Azure Multi-Factor Authentication v hello office.</span><span class="sxs-lookup"><span data-stu-id="6b916-121">For example, if employees use hello application from hello office and from home, you may decide you don't want their phones ringing for Azure Multi-Factor Authentication while at hello office.</span></span> <span data-ttu-id="6b916-122">V takovém případě zadáte podsíť office hello jako položku důvěryhodných IP adres.</span><span class="sxs-lookup"><span data-stu-id="6b916-122">For this, you would specify hello office subnet as Trusted IPs entry.</span></span>
7. <span data-ttu-id="6b916-123">Klikněte na **Přidat...**</span><span class="sxs-lookup"><span data-stu-id="6b916-123">Click **Add…**</span></span>
8. <span data-ttu-id="6b916-124">Vyberte **jednu IP adresu** Pokud byste chtěli tooskip jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6b916-124">Select **Single IP** if you would like tooskip a single IP address.</span></span>
9. <span data-ttu-id="6b916-125">Vyberte **rozsah IP adres** Pokud byste chtěli tooskip celý rozsah IP adres.</span><span class="sxs-lookup"><span data-stu-id="6b916-125">Select **IP Range** if you would like tooskip an entire IP range.</span></span> <span data-ttu-id="6b916-126">Příklad 10.63.193.1–10.63.193.100.</span><span class="sxs-lookup"><span data-stu-id="6b916-126">Example 10.63.193.1-10.63.193.100.</span></span>
10. <span data-ttu-id="6b916-127">Vyberte **podsíť** Pokud byste chtěli toospecify rozsah IP adres pomocí notace podsítě.</span><span class="sxs-lookup"><span data-stu-id="6b916-127">Select **Subnet** if you would like toospecify a range of IPs using subnet notation.</span></span> <span data-ttu-id="6b916-128">Zadejte počáteční IP adresu podsítě hello a vyberte příslušnou síťovou masku hello z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="6b916-128">Enter hello subnet's starting IP and pick hello appropriate netmask from hello drop-down list.</span></span>
11. <span data-ttu-id="6b916-129">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b916-129">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b916-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b916-130">Next steps</span></span>

- [<span data-ttu-id="6b916-131">Konfigurace zařízení sítě VPN třetích stran pro Azure MFA Server</span><span class="sxs-lookup"><span data-stu-id="6b916-131">Configure third-party VPN appliances for Azure MFA Server</span></span>](multi-factor-authentication-advanced-vpn-configurations.md)

- [<span data-ttu-id="6b916-132">Posílení vaší stávající infrastruktury pro ověřování s hello NPS rozšíření pro Azure MFA</span><span class="sxs-lookup"><span data-stu-id="6b916-132">Augment your existing authentication infrastructure with hello NPS extension for Azure MFA</span></span>](multi-factor-authentication-nps-extension.md)
