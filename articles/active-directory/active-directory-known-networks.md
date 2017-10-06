---
title: "aaaKnown sítě v hello portál Azure classic | Microsoft Docs"
description: "Konfigurací sítě známé, nemusíte mít IP adresy, které jsou vlastněny organizaci součástí hello in přihlášení z několika zeměpisných oblastí a in přihlášení z IP adres s podezřelou aktivitu sestavy."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: ec636cdda172cd3baeb1e606dd8d6e1949fbc63b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="known-networks"></a><span data-ttu-id="672d4-103">Známé sítě</span><span class="sxs-lookup"><span data-stu-id="672d4-103">Known networks</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="672d4-104">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="672d4-104">Azure classic portal</span></span>](active-directory-known-networks.md)
> * [<span data-ttu-id="672d4-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="672d4-105">Azure portal</span></span>](active-directory-known-networks-azure-portal.md)
> 
> 


<span data-ttu-id="672d4-106">Můžete použít Azure Active Directory přístup a použití sestav toogain přehled hello integrity a zabezpečení adresáře vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="672d4-106">You can use Azure Active Directory's access and usage reports toogain visibility into hello integrity and security of your organization’s directory.</span></span> <span data-ttu-id="672d4-107">Tyto informace a správce directory pomohou určit, kde může být bezpečnostním rizikům, tak, aby se adekvátní naplánovat toomitigate těchto rizik.</span><span class="sxs-lookup"><span data-stu-id="672d4-107">With this information, a directory admin can better determine where possible security risks may lie so that they can adequately plan toomitigate those risks.</span></span>

<span data-ttu-id="672d4-108">Je možné, že hello "*přihlášení z několika zeměpisných oblastí*"a"*přihlášení z IP adres s podezřelou aktivitou*" sestavy nesprávně příznak IP adresy, které jsou ve skutečnosti vlastněny vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="672d4-108">It is possible that hello “*Sign ins from multiple geographies*” and “*Sign ins from IP addresses with suspicious activity*” reports incorrectly flag IP addresses that are actually owned by your organization.</span></span> 

<span data-ttu-id="672d4-109">Může k tomu, například dojít při:</span><span class="sxs-lookup"><span data-stu-id="672d4-109">This can, for example, happen when:</span></span> 

* <span data-ttu-id="672d4-110">Uživatel v centrále Boston má přihlášený vzdáleně tooyour datového centra v Brno aktivační události hello "Sign in z několika zeměpisných oblastí" sestavy</span><span class="sxs-lookup"><span data-stu-id="672d4-110">A user in your Boston office has signed in remotely tooyour data center in San Francisco triggers hello “Sign ins from multiple geographies” report</span></span> 
* <span data-ttu-id="672d4-111">Uživatel vaší organizace pokusí toosign v s hello nesprávné heslo aktivační události "Sign in z IP adres s podezřelou aktivitou" sestavy</span><span class="sxs-lookup"><span data-stu-id="672d4-111">A user of your organization tries toosign-on several times with an incorrect password triggers hello “Sign ins from IP addresses with suspicious activity” report</span></span> 

<span data-ttu-id="672d4-112">sestavy tooprevent těchto případech generování zavádějící zabezpečení, měli byste přidat IP adresu rozsahy toohello seznam se známými a veřejné IP adresy vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="672d4-112">tooprevent these cases from generating misleading security reports, you should add known IP address ranges toohello list of your organization's public IP address.</span></span>    

### <a name="tooadd-your-organizations-public-ip-address-ranges-perform-hello-following-steps"></a><span data-ttu-id="672d4-113">rozsahy tooadd veřejnou IP adresu vaší organizace, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="672d4-113">tooadd your organization’s public IP address ranges, perform hello following steps:</span></span>

1. <span data-ttu-id="672d4-114">Přihlášení toohello [portál pro správu Azure](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="672d4-114">Sign-on toohello [Azure management portal](https://manage.windowsazure.com).</span></span>

2. <span data-ttu-id="672d4-115">V levém podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="672d4-115">In hello left pane, click **Active Directory**.</span></span> 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-01.png)

3. <span data-ttu-id="672d4-117">V hello **Directory** vyberte adresáře.</span><span class="sxs-lookup"><span data-stu-id="672d4-117">In hello **Directory** tab, select your directory.</span></span>

4. <span data-ttu-id="672d4-118">V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="672d4-118">In hello menu on hello top, click **Configure**.</span></span> 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-02.png)

5. <span data-ttu-id="672d4-120">Na kartě Konfigurace hello přejděte příliš**vaší organizace rozsahů veřejných IP adres**</span><span class="sxs-lookup"><span data-stu-id="672d4-120">On hello Configure tab, go too**your organizations public IP address ranges**</span></span> 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-03.png)

6. <span data-ttu-id="672d4-122">Klikněte na tlačítko **přidat rozsahy známé IP adres**.</span><span class="sxs-lookup"><span data-stu-id="672d4-122">Click **Add Known IP Address Ranges**.</span></span>

7. <span data-ttu-id="672d4-123">Přidejte vaše rozsahy adres v hello dialogu, který se zobrazí a potom klepněte na tlačítko zaškrtnutí hello po dokončení.</span><span class="sxs-lookup"><span data-stu-id="672d4-123">Add your address ranges in hello dialog box that appears, and then click hello check button  when you are done.</span></span> 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-04.png)

<span data-ttu-id="672d4-125">**Další zdroje informací:**</span><span class="sxs-lookup"><span data-stu-id="672d4-125">**Additional resources:**</span></span>

* [<span data-ttu-id="672d4-126">Zobrazení sestav přístupů a používání</span><span class="sxs-lookup"><span data-stu-id="672d4-126">View your access and usage reports</span></span>](active-directory-view-access-usage-reports.md)
* [<span data-ttu-id="672d4-127">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="672d4-127">Sign ins from IP addresses with suspicious activity</span></span>](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [<span data-ttu-id="672d4-128">Přihlášení z několika zeměpisných oblastí</span><span class="sxs-lookup"><span data-stu-id="672d4-128">Sign ins from multiple geographies</span></span>](active-directory-reporting-sign-ins-from-multiple-geographies.md)

