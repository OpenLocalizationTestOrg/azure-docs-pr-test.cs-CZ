---
title: "instalace aaaProblem hello konektor agenta Proxy aplikací. | Microsoft Docs"
description: "Jak tootroubleshoot problémů může hello setkávají při instalaci agenta konektor Proxy aplikace"
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
ms.openlocfilehash: 07ac366a429083af0c9b87aa9df9cf3876132b90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-proxy-agent-connector"></a><span data-ttu-id="09d35-103">Problém instalace hello agenta konektor Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="09d35-103">Problem installing hello Application Proxy Agent Connector</span></span>

<span data-ttu-id="09d35-104">Microsoft AAD Application Proxy Connector je součást interní doméně, která používá připojení hello tooestablish odchozí připojení z hello cloudu dostupný koncový bod toohello interní domény.</span><span class="sxs-lookup"><span data-stu-id="09d35-104">Microsoft AAD Application Proxy Connector is an internal domain component that uses outbound connections tooestablish hello connectivity from hello cloud available endpoint toohello internal domain.</span></span>

## <a name="general-problem-areas-with-connector-installation"></a><span data-ttu-id="09d35-105">Obecné problémových oblastí pomocí instalace konektoru</span><span class="sxs-lookup"><span data-stu-id="09d35-105">General Problem Areas with Connector installation</span></span>

<span data-ttu-id="09d35-106">Při hello instalace konektoru selže, hello hlavní příčinou je obvykle jednu z následujících oblastí hello:</span><span class="sxs-lookup"><span data-stu-id="09d35-106">When hello installation of a connector fails, hello root cause is usually one of hello following areas:</span></span>

1.  <span data-ttu-id="09d35-107">**Připojení** – toocomplete úspěšnou instalaci hello tooregister potřebám nový konektor a vytvořit vlastnosti budoucí vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="09d35-107">**Connectivity** – toocomplete a successful installation, hello new connector needs tooregister and establish future trust properties.</span></span> <span data-ttu-id="09d35-108">K tomu je potřeba připojení toohello cloudové služby AAD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="09d35-108">This is done by connecting toohello AAD Application Proxy cloud service.</span></span>

2.  <span data-ttu-id="09d35-109">**Navázání vztahu důvěryhodnosti** – nový konektor hello vytvoří certifikátu podepsaného svým držitelem a zaregistruje toohello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="09d35-109">**Trust Establishment** – hello new connector creates a self-signed cert and registers toohello cloud service.</span></span>

3.  <span data-ttu-id="09d35-110">**Ověřování Dobrý den, správce** – během instalace, hello uživatel musí poskytnout přihlašovací údaje Správce instalace konektoru toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="09d35-110">**Authentication of hello admin** – during installation, hello user must provide admin credentials toocomplete hello Connector installation.</span></span>

## <a name="verify-connectivity-toohello-cloud-application-proxy-service-and-microsoft-login-page"></a><span data-ttu-id="09d35-111">Zkontrolujte připojení k toohello Cloudový Proxy aplikací služby a Microsoft Login stránku</span><span class="sxs-lookup"><span data-stu-id="09d35-111">Verify connectivity toohello Cloud Application Proxy service and Microsoft Login page</span></span>

<span data-ttu-id="09d35-112">**Cíl:** ověřte, zda tento počítač konektor hello můžete připojit koncový bod registrace AAD aplikace Proxy toohello, jakož i Microsoft přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="09d35-112">**Objective:** Verify that hello connector machine can connect toohello AAD Application Proxy registration endpoint as well as Microsoft login page.</span></span>

1.  <span data-ttu-id="09d35-113">Otevřete prohlížeč a přejděte toohello následující webové stránce: <https://aadap-portcheck.connectorporttest.msappproxy.net> a ověřte, že hello připojení tooCentral USA a datová centra východní USA s porty 9090 a 9091 pracuje.</span><span class="sxs-lookup"><span data-stu-id="09d35-113">Open a browser and go toohello following web page: <https://aadap-portcheck.connectorporttest.msappproxy.net> , and verify that hello connectivity tooCentral US and East US datacenters with ports 9090 and 9091 is working.</span></span>

2.  <span data-ttu-id="09d35-114">Pokud některé z těchto portů neproběhne úspěšně (nemá zeleného zaškrtnutí), ověřte, že hello brány Firewall nebo proxy serveru back-end má \*. msappproxy.net s porty 9090 a 9091 správně definované.</span><span class="sxs-lookup"><span data-stu-id="09d35-114">If any of those ports is not successful (doesn’t have a green checkmark), verify that hello Firewall or backend proxy has \*.msappproxy.net with ports 9090 and 9091 defined correctly.</span></span>

3.  <span data-ttu-id="09d35-115">Otevřete prohlížeč (samostatné kartě) a přejděte na následující webové stránce toohello: <https://login.microsoftonline.com>, ujistěte se, že můžete se přihlásit toothat stránky.</span><span class="sxs-lookup"><span data-stu-id="09d35-115">Open a browser (separate tab) and go toohello following web page: <https://login.microsoftonline.com>, make sure that you can login toothat page.</span></span>

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a><span data-ttu-id="09d35-116">Ověřte, zda počítač a back-end součásti Podpora pro cert vztah důvěryhodnosti Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="09d35-116">Verify Machine and backend components support for Application Proxy trust cert</span></span>

<span data-ttu-id="09d35-117">**Cíl:** ověřte, zda počítač hello konektor, back-end proxy a firewall podporuje hello certifikát vytvořený hello konektor pro budoucí vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="09d35-117">**Objective:** Verify that hello connector machine, backend proxy and firewall can support hello certificate created by hello connector for future trust.</span></span>

>[!NOTE]
><span data-ttu-id="09d35-118">konektor Hello pokusí toocreate SHA512 certifikátu, který podporuje TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="09d35-118">hello connector tries toocreate a SHA512 cert that is supported by TLS1.2.</span></span> <span data-ttu-id="09d35-119">Pokud počítač hello nebo hello back-end brány firewall a proxy server nepodporuje TLS1.2, hello instalace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="09d35-119">If hello machine or hello backend firewall and proxy does not support TLS1.2, hello installation fail.</span></span>
>
>

<span data-ttu-id="09d35-120">**problém tooresolve hello:**</span><span class="sxs-lookup"><span data-stu-id="09d35-120">**tooresolve hello issue:**</span></span>

1.  <span data-ttu-id="09d35-121">Ověřte hello počítač podporuje TLS1.2 – verze všechny systémy Windows po 2012 R2 by měly podporovat protokol TLS 1.2.</span><span class="sxs-lookup"><span data-stu-id="09d35-121">Verify hello machine supports TLS1.2 – All Windows versions after 2012 R2 should support TLS 1.2.</span></span> <span data-ttu-id="09d35-122">Pokud je váš počítač konektor z verze 2012 R2 nebo před, ujistěte se, že hello následující články znalostní báze jsou nainstalovány na počítači hello: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span><span class="sxs-lookup"><span data-stu-id="09d35-122">If your connector machine is from a version of 2012 R2 or prior, make sure that hello following KBs are installed on hello machine: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2></span></span>

2.  <span data-ttu-id="09d35-123">Obraťte se na správce sítě a požádejte tooverify jestli hello back-end proxy a firewall neblokují SHA512 pro odchozí provoz.</span><span class="sxs-lookup"><span data-stu-id="09d35-123">Contact your network admin and ask tooverify that hello backend proxy and firewall do not block SHA512 for outgoing traffic.</span></span>

## <a name="verify-admin-is-used-tooinstall-hello-connector"></a><span data-ttu-id="09d35-124">Ujistěte se, že správce použít tooinstall hello konektoru</span><span class="sxs-lookup"><span data-stu-id="09d35-124">Verify admin is used tooinstall hello connector</span></span>

<span data-ttu-id="09d35-125">**Cíl:** ověřte, že hello uživatel při pokusu o tooinstall hello konektor je správce se správnými přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="09d35-125">**Objective:** Verify that hello user who tries tooinstall hello connector is an administrator with correct credentials.</span></span> <span data-ttu-id="09d35-126">V současné době hello uživatel musí být globálním správcem pro instalaci toosucceed hello.</span><span class="sxs-lookup"><span data-stu-id="09d35-126">Currently, hello user must be a global administrator for hello installation toosucceed.</span></span>

<span data-ttu-id="09d35-127">**tooverify hello pověření jsou správná:**</span><span class="sxs-lookup"><span data-stu-id="09d35-127">**tooverify hello credentials are correct:**</span></span>

<span data-ttu-id="09d35-128">Připojit příliš<https://login.microsoftonline.com> a používání hello stejné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="09d35-128">Connect too<https://login.microsoftonline.com> and use hello same credentials.</span></span> <span data-ttu-id="09d35-129">Ujistěte se, že hello přihlášení úspěšné.</span><span class="sxs-lookup"><span data-stu-id="09d35-129">Make sure hello login is successful.</span></span> <span data-ttu-id="09d35-130">Role uživatele hello můžete zkontrolovat tak, že přejdete příliš**Azure Active Directory**  - &gt; **uživatelů a skupin**  - &gt; **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="09d35-130">You can check hello user role by going too**Azure Active Directory** -&gt; **Users and Groups** -&gt; **All Users**.</span></span> 

<span data-ttu-id="09d35-131">Vyberte uživatelský účet, pak "Directory Role" v nabídce výsledné hello.</span><span class="sxs-lookup"><span data-stu-id="09d35-131">Select your user account, then “Directory Role” in hello resulting menu.</span></span> <span data-ttu-id="09d35-132">Ověřte, zda že je tento hello vybranou roli "Globální správce".</span><span class="sxs-lookup"><span data-stu-id="09d35-132">Verify that hello selected role is “Global administrator”.</span></span> <span data-ttu-id="09d35-133">Pokud jste nelze tooaccess žádné hello stránky společně tyto kroky, nejste globálním správcem.</span><span class="sxs-lookup"><span data-stu-id="09d35-133">If you are unable tooaccess any of hello pages along these steps, you are not a global administrator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09d35-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09d35-134">Next steps</span></span>
[<span data-ttu-id="09d35-135">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="09d35-135">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
