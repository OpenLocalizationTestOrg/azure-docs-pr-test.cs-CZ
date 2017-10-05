---
title: "Známý sítí v portálu Azure classic | Microsoft Docs"
description: "Konfigurací sítě známé, nemusíte mít IP adresy, které jsou vlastněny organizaci součástí Sign in z několika zeměpisných oblastí a in přihlášení z IP adres s podezřelou aktivitu sestavy."
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
ms.openlocfilehash: e4d51d1d2f09fca34d749879e21d49f785eac35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="known-networks"></a><span data-ttu-id="9be10-103">Známé sítě</span><span class="sxs-lookup"><span data-stu-id="9be10-103">Known networks</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9be10-104">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="9be10-104">Azure classic portal</span></span>](active-directory-known-networks.md)
> * [<span data-ttu-id="9be10-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9be10-105">Azure portal</span></span>](active-directory-known-networks-azure-portal.md)
> 
> 


<span data-ttu-id="9be10-106">Přístup k Azure Active Directory a sestavy využití, které slouží k získat přehled o integrity a zabezpečení adresáři vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="9be10-106">You can use Azure Active Directory's access and usage reports to gain visibility into the integrity and security of your organization’s directory.</span></span> <span data-ttu-id="9be10-107">Tyto informace a správce directory pomohou určit, kde může být bezpečnostním rizikům, tak, aby adekvátní můžete naplánovat zmírnění.</span><span class="sxs-lookup"><span data-stu-id="9be10-107">With this information, a directory admin can better determine where possible security risks may lie so that they can adequately plan to mitigate those risks.</span></span>

<span data-ttu-id="9be10-108">Je možné, který "*přihlášení z několika zeměpisných oblastí*"a"*přihlášení z IP adres s podezřelou aktivitou*" sestavy nesprávně příznak IP adresy, které jsou ve skutečnosti vlastněny vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="9be10-108">It is possible that the “*Sign ins from multiple geographies*” and “*Sign ins from IP addresses with suspicious activity*” reports incorrectly flag IP addresses that are actually owned by your organization.</span></span> 

<span data-ttu-id="9be10-109">Může k tomu, například dojít při:</span><span class="sxs-lookup"><span data-stu-id="9be10-109">This can, for example, happen when:</span></span> 

* <span data-ttu-id="9be10-110">Uživatel ve vaší Boston se office přihlásil vzdáleně do vašeho datového centra v San Franciscu aktivuje sestavy "Přihlášení z několika zeměpisných oblastí"</span><span class="sxs-lookup"><span data-stu-id="9be10-110">A user in your Boston office has signed in remotely to your data center in San Francisco triggers the “Sign ins from multiple geographies” report</span></span> 
* <span data-ttu-id="9be10-111">Uživatel vaší organizace pokusí o přihlašování pomocí aktivačních událostí nesprávné heslo sestavy "Přihlášení z IP adres s podezřelou aktivitou"</span><span class="sxs-lookup"><span data-stu-id="9be10-111">A user of your organization tries to sign-on several times with an incorrect password triggers the “Sign ins from IP addresses with suspicious activity” report</span></span> 

<span data-ttu-id="9be10-112">Abyste zabránili těchto případech generování sestav zavádějící zabezpečení, měli byste přidat známé rozsahy IP adres do seznamu veřejnou IP adresu vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="9be10-112">To prevent these cases from generating misleading security reports, you should add known IP address ranges to the list of your organization's public IP address.</span></span>    

### <a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a><span data-ttu-id="9be10-113">Pokud chcete přidat vaší organizace veřejné rozsahy IP adres, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9be10-113">To add your organization’s public IP address ranges, perform the following steps:</span></span>

1. <span data-ttu-id="9be10-114">Přihlášení k [portál pro správu Azure](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9be10-114">Sign-on to the [Azure management portal](https://manage.windowsazure.com).</span></span>

2. <span data-ttu-id="9be10-115">V levém podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9be10-115">In the left pane, click **Active Directory**.</span></span> 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-01.png)

3. <span data-ttu-id="9be10-117">V **Directory** vyberte adresáře.</span><span class="sxs-lookup"><span data-stu-id="9be10-117">In the **Directory** tab, select your directory.</span></span>

4. <span data-ttu-id="9be10-118">V nabídce v horní části, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="9be10-118">In the menu on the top, click **Configure**.</span></span> 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-02.png)

5. <span data-ttu-id="9be10-120">Na kartě Konfigurace, přejděte na **vaší organizace rozsahů veřejných IP adres**</span><span class="sxs-lookup"><span data-stu-id="9be10-120">On the Configure tab, go to **your organizations public IP address ranges**</span></span> 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-03.png)

6. <span data-ttu-id="9be10-122">Klikněte na tlačítko **přidat rozsahy známé IP adres**.</span><span class="sxs-lookup"><span data-stu-id="9be10-122">Click **Add Known IP Address Ranges**.</span></span>

7. <span data-ttu-id="9be10-123">V zobrazeném dialogu přidat vaše rozsahy adres a po dokončení klikněte na tlačítko se zaškrtnutím.</span><span class="sxs-lookup"><span data-stu-id="9be10-123">Add your address ranges in the dialog box that appears, and then click the check button  when you are done.</span></span> 

    ![Známé sítě](./media/active-directory-known-networks/known-netwoks-04.png)

<span data-ttu-id="9be10-125">**Další zdroje informací:**</span><span class="sxs-lookup"><span data-stu-id="9be10-125">**Additional resources:**</span></span>

* [<span data-ttu-id="9be10-126">Zobrazení sestav přístupů a používání</span><span class="sxs-lookup"><span data-stu-id="9be10-126">View your access and usage reports</span></span>](active-directory-view-access-usage-reports.md)
* [<span data-ttu-id="9be10-127">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="9be10-127">Sign ins from IP addresses with suspicious activity</span></span>](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [<span data-ttu-id="9be10-128">Přihlášení z několika zeměpisných oblastí</span><span class="sxs-lookup"><span data-stu-id="9be10-128">Sign ins from multiple geographies</span></span>](active-directory-reporting-sign-ins-from-multiple-geographies.md)

