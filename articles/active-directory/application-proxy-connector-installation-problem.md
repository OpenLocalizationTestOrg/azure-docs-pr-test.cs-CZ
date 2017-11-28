---
title: "Problém instalace agenta konektor Proxy aplikace | Microsoft Docs"
description: "Řešení potíží, která vám může při instalaci agenta konektor Proxy aplikace"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 91b3f6f3c8339647f568a509e9efd8e1fffb13dd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a><span data-ttu-id="d482b-103">Problém instalace agenta konektor Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d482b-103">Problem installing the Application Proxy Agent Connector</span></span>

<span data-ttu-id="d482b-104">Microsoft AAD Application Proxy Connector je součást interní doméně, která používá odchozí připojení k navázání připojení z koncového bodu cloudu k dispozici k interní doméně.</span><span class="sxs-lookup"><span data-stu-id="d482b-104">Microsoft AAD Application Proxy Connector is an internal domain component that uses outbound connections to establish the connectivity from the cloud available endpoint to the internal domain.</span></span>

## <a name="general-problem-areas-with-connector-installation"></a><span data-ttu-id="d482b-105">Obecné problémových oblastí pomocí instalace konektoru</span><span class="sxs-lookup"><span data-stu-id="d482b-105">General Problem Areas with Connector installation</span></span>

<span data-ttu-id="d482b-106">Při instalaci konektoru selže, hlavní příčinou je obvykle jednu z těchto oblastí:</span><span class="sxs-lookup"><span data-stu-id="d482b-106">When the installation of a connector fails, the root cause is usually one of the following areas:</span></span>

1.  <span data-ttu-id="d482b-107">**Připojení** – dokončení úspěšné instalaci se nové konektor musí zaregistrovat a vytvořit vlastnosti budoucí vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="d482b-107">**Connectivity** – to complete a successful installation, the new connector needs to register and establish future trust properties.</span></span> <span data-ttu-id="d482b-108">K tomu je potřeba připojení ke cloudové službě AAD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="d482b-108">This is done by connecting to the AAD Application Proxy cloud service.</span></span>

2.  <span data-ttu-id="d482b-109">**Navázání vztahu důvěryhodnosti** – nový konektor vytvoří certifikátu podepsaného svým držitelem a zaregistruje do cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d482b-109">**Trust Establishment** – the new connector creates a self-signed cert and registers to the cloud service.</span></span>

3.  <span data-ttu-id="d482b-110">**Ověřování správce** – během instalace, uživatel musí poskytnout přihlašovací údaje správce pro dokončení instalace konektoru.</span><span class="sxs-lookup"><span data-stu-id="d482b-110">**Authentication of the admin** – during installation, the user must provide admin credentials to complete the Connector installation.</span></span>

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a><span data-ttu-id="d482b-111">Ověření připojení k Proxy aplikace cloudové služby a Microsoft Login stránku</span><span class="sxs-lookup"><span data-stu-id="d482b-111">Verify connectivity to the Cloud Application Proxy service and Microsoft Login page</span></span>

<span data-ttu-id="d482b-112">**Cíl:** ověřte, zda počítač konektoru může připojit k koncový bod registrace AAD aplikace Proxy, jakož i Microsoft přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="d482b-112">**Objective:** Verify that the connector machine can connect to the AAD Application Proxy registration endpoint as well as Microsoft login page.</span></span>

1.  <span data-ttu-id="d482b-113">Otevřete prohlížeč a přejděte na následující webové stránce: <https://aadap-portcheck.connectorporttest.msappproxy.net> a ověřte, zda je funkční připojení k datová centra s porty 9090 a 9091 střed USA a východní USA.</span><span class="sxs-lookup"><span data-stu-id="d482b-113">Open a browser and go to the following web page: <https://aadap-portcheck.connectorporttest.msappproxy.net> , and verify that the connectivity to Central US and East US datacenters with ports 9090 and 9091 is working.</span></span>

2.  <span data-ttu-id="d482b-114">Pokud některé z těchto portů neproběhne úspěšně (nemá zeleného zaškrtnutí), ověřte, zda má proxy server brány Firewall nebo back-end \*. msappproxy.net s porty 9090 a 9091 správně definované.</span><span class="sxs-lookup"><span data-stu-id="d482b-114">If any of those ports is not successful (doesn’t have a green checkmark), verify that the Firewall or backend proxy has \*.msappproxy.net with ports 9090 and 9091 defined correctly.</span></span>

3.  <span data-ttu-id="d482b-115">Otevřete prohlížeč (samostatné kartě) a přejděte na následující webové stránce: <https://login.microsoftonline.com>, ujistěte se, že se může přihlásit k této stránce.</span><span class="sxs-lookup"><span data-stu-id="d482b-115">Open a browser (separate tab) and go to the following web page: <https://login.microsoftonline.com>, make sure that you can login to that page.</span></span>

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a><span data-ttu-id="d482b-116">Ověřte, zda počítač a back-end součásti Podpora pro cert vztah důvěryhodnosti Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d482b-116">Verify Machine and backend components support for Application Proxy trust cert</span></span>

<span data-ttu-id="d482b-117">**Cíl:** ověřte, zda počítač konektoru, back-end proxy a firewall podporuje certifikát vytvořený konektor pro budoucí vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="d482b-117">**Objective:** Verify that the connector machine, backend proxy and firewall can support the certificate created by the connector for future trust.</span></span>

>[!NOTE]
><span data-ttu-id="d482b-118">Konektor se pokusí vytvořit SHA512 certifikátu, který podporuje TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="d482b-118">The connector tries to create a SHA512 cert that is supported by TLS1.2.</span></span> <span data-ttu-id="d482b-119">Pokud je počítač nebo back-end brány firewall a proxy server nepodporuje TLS1.2, instalace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="d482b-119">If the machine or the backend firewall and proxy does not support TLS1.2, the installation fail.</span></span>
>
>

<span data-ttu-id="d482b-120">**K vyřešení problému:**</span><span class="sxs-lookup"><span data-stu-id="d482b-120">**To resolve the issue:**</span></span>

1.  <span data-ttu-id="d482b-121">Ověřte počítač podporuje TLS1.2 – verze všechny systémy Windows po 2012 R2 by měly podporovat protokol TLS 1.2.</span><span class="sxs-lookup"><span data-stu-id="d482b-121">Verify the machine supports TLS1.2 – All Windows versions after 2012 R2 should support TLS 1.2.</span></span> <span data-ttu-id="d482b-122">Pokud váš počítač konektor z verze 2012 R2 nebo před, ujistěte se, zda jsou v počítači nainstalovány následující články znalostní báze: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span><span class="sxs-lookup"><span data-stu-id="d482b-122">If your connector machine is from a version of 2012 R2 or prior, make sure that the following KBs are installed on the machine: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span></span>

2.  <span data-ttu-id="d482b-123">Obraťte se na správce sítě a požádejte o ověření, že back-end proxy a firewall neblokují SHA512 pro odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="d482b-123">Contact your network admin and ask to verify that the backend proxy and firewall do not block SHA512 for outgoing traffic.</span></span>

## <a name="verify-admin-is-used-to-install-the-connector"></a><span data-ttu-id="d482b-124">Ověřte, že správce se používá k instalaci konektoru</span><span class="sxs-lookup"><span data-stu-id="d482b-124">Verify admin is used to install the connector</span></span>

<span data-ttu-id="d482b-125">**Cíl:** ověřte, že uživatel, který se snaží nainstalovat konektor je správce se správnými přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="d482b-125">**Objective:** Verify that the user who tries to install the connector is an administrator with correct credentials.</span></span> <span data-ttu-id="d482b-126">V současné době uživatel musí být globálním správcem pro instalaci proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="d482b-126">Currently, the user must be a global administrator for the installation to succeed.</span></span>

<span data-ttu-id="d482b-127">**Chcete-li ověřit správnost přihlašovací údaje:**</span><span class="sxs-lookup"><span data-stu-id="d482b-127">**To verify the credentials are correct:**</span></span>

<span data-ttu-id="d482b-128">Připojení k <https://login.microsoftonline.com> a používat stejné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="d482b-128">Connect to <https://login.microsoftonline.com> and use the same credentials.</span></span> <span data-ttu-id="d482b-129">Ověřte, zda je přihlášení úspěšné.</span><span class="sxs-lookup"><span data-stu-id="d482b-129">Make sure the login is successful.</span></span> <span data-ttu-id="d482b-130">Role uživatele můžete zkontrolovat přechodem na **Azure Active Directory**  - &gt; **uživatelů a skupin**  - &gt; **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d482b-130">You can check the user role by going to **Azure Active Directory** -&gt; **Users and Groups** -&gt; **All Users**.</span></span> 

<span data-ttu-id="d482b-131">Vyberte uživatelský účet, pak "Directory Role" v nabídce výsledné.</span><span class="sxs-lookup"><span data-stu-id="d482b-131">Select your user account, then “Directory Role” in the resulting menu.</span></span> <span data-ttu-id="d482b-132">Ověřte, zda vybranou roli "Globální správce".</span><span class="sxs-lookup"><span data-stu-id="d482b-132">Verify that the selected role is “Global administrator”.</span></span> <span data-ttu-id="d482b-133">Pokud není možné získat přístup k libovolnému stránek společně tyto kroky, nejste globálním správcem.</span><span class="sxs-lookup"><span data-stu-id="d482b-133">If you are unable to access any of the pages along these steps, you are not a global administrator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d482b-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d482b-134">Next steps</span></span>
[<span data-ttu-id="d482b-135">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d482b-135">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
