---
title: "Správa zařízení pomocí Azure portal – preview | Microsoft Docs"
description: "Další informace o použití portálu Azure ke správě zařízení."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 4b46e1627a229b0649d9ccd2550cd28fda9849f8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="managing-devices-using-the-azure-portal---preview"></a><span data-ttu-id="d55e6-103">Správa zařízení pomocí Azure portal – náhled</span><span class="sxs-lookup"><span data-stu-id="d55e6-103">Managing devices using the Azure portal - preview</span></span>

>[!NOTE]
><span data-ttu-id="d55e6-104">Tato funkce je aktuálně ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="d55e6-104">This capability currently is in public preview.</span></span> <span data-ttu-id="d55e6-105">Připravte se na obnovit nebo odeberte všechny změny.</span><span class="sxs-lookup"><span data-stu-id="d55e6-105">Be prepared to revert or remove any changes.</span></span> <span data-ttu-id="d55e6-106">Tato funkce je k dispozici v libovolné předplatné služby Azure Active Directory (Azure AD) verzi public Preview.</span><span class="sxs-lookup"><span data-stu-id="d55e6-106">The feature is available in any Azure Active Directory (Azure AD) subscription during public preview.</span></span> <span data-ttu-id="d55e6-107">Když je tato funkce obecně dostupná, může vyžadovat některé aspekty funkce však předplatné služby Azure Active Directory premium.</span><span class="sxs-lookup"><span data-stu-id="d55e6-107">However, when the feature becomes generally available, some aspects of the feature might require an Azure Active Directory premium subscription.</span></span>


<span data-ttu-id="d55e6-108">Se správou zařízení ve službě Azure Active Directory (Azure AD) můžete zajistit, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="d55e6-108">With device management in Azure Active Directory (Azure AD), you can ensure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> 

<span data-ttu-id="d55e6-109">V tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="d55e6-109">This topic:</span></span>

- <span data-ttu-id="d55e6-110">Předpokládá, že jste obeznámeni s [Úvod do správy zařízení v Azure Active Directory](device-management-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="d55e6-110">Assumes that you are familiar with the [introduction to device management in Azure Active Directory](device-management-introduction.md)</span></span>

- <span data-ttu-id="d55e6-111">Poskytuje informace o správě zařízení pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d55e6-111">Provides you with information about managing your devices using the Azure portal</span></span>


<span data-ttu-id="d55e6-112">Ke správě zařízení na portálu Azure, je třeba kliknout na **zařízení** v **spravovat** části **Azure Active Directory** okno.</span><span class="sxs-lookup"><span data-stu-id="d55e6-112">To manage devices in the Azure portal, you need to click **Devices** in the **Manage** section of the the **Azure Active Directory** blade.</span></span>

![Spravovat zařízení s Intune](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a><span data-ttu-id="d55e6-114">Konfigurace nastavení zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-114">Configure device settings</span></span>

<span data-ttu-id="d55e6-115">Ke správě zařízení pomocí portálu Azure, musí být buď zaregistrované, nebo připojený k Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-115">To manage your devices using the Azure portal, they need to be either registered or joined to Azure AD.</span></span> <span data-ttu-id="d55e6-116">Jako správce můžete optimalizovat proces registrace a připojení zařízení tak, že nakonfigurujete nastavení zařízení.</span><span class="sxs-lookup"><span data-stu-id="d55e6-116">As an administrator, you can fine-tune the process of registering and joining devices by configuring the device settings.</span></span>

![Spravovat zařízení s Intune](./media/device-management-azure-portal/22.png)


<span data-ttu-id="d55e6-118">V okně Nastavení zařízení můžete konfigurovat:</span><span class="sxs-lookup"><span data-stu-id="d55e6-118">The device settings blade enables you to configure:</span></span>

- <span data-ttu-id="d55e6-119">**Uživatelé mohou připojit zařízení ke službě Azure AD** – toto nastavení umožňuje vybrat uživatele, kteří mohou připojit zařízení ke službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-119">**Users may join devices to Azure AD** - This settings enables you to select the users who can join devices to Azure AD.</span></span> <span data-ttu-id="d55e6-120">Výchozí hodnota je **všechny**.</span><span class="sxs-lookup"><span data-stu-id="d55e6-120">The default is **All**.</span></span>

- <span data-ttu-id="d55e6-121">**Zařízení připojená k další místní správci v Azure AD** – můžete vybrat uživatele, kteří jsou udělena práva místního správce v zařízení.</span><span class="sxs-lookup"><span data-stu-id="d55e6-121">**Additional local administrators on Azure AD joined devices** - You can select the users that are granted local administrator rights on a device.</span></span> <span data-ttu-id="d55e6-122">Jsou přidáni uživatelé tady přidat *Správci zařízení* role ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-122">Users added here are added to the *Device Administrators* role in Azure AD.</span></span> <span data-ttu-id="d55e6-123">Globální správci ve službě Azure AD a vlastníci zařízení jsou udělena práva místního správce ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d55e6-123">Global administrators in Azure AD and device owners are granted local administrator rights by default.</span></span> <span data-ttu-id="d55e6-124">Tato možnost je k dispozici prostřednictvím produkty, jako je například Azure AD Premium nebo Enterprise Mobility Suite (EMS) funkce edice premium.</span><span class="sxs-lookup"><span data-stu-id="d55e6-124">This option is a premium edition capability available through products such as Azure AD Premium or the Enterprise Mobility Suite (EMS).</span></span> 

- <span data-ttu-id="d55e6-125">**Uživatelé mohou registrovat svá zařízení s Azure AD** -je nutné nakonfigurovat tato nastavení mají zařízení zaregistrovat u služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-125">**Users may register their devices with Azure AD** - You need to configure this setting to allow devices to be registered with Azure AD.</span></span> <span data-ttu-id="d55e6-126">Pokud vyberete **žádné**, zařízení není povoleno zaregistrovat, které nejsou připojené k Azure AD nebo hybridní připojený k Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-126">If you select **None**, devices are not allowed to register when they are not Azure AD joined or hybrid Azure AD joined.</span></span> <span data-ttu-id="d55e6-127">Registrace s Microsoft Intune nebo správy mobilních zařízení (MDM) pro Office 365 vyžaduje registraci.</span><span class="sxs-lookup"><span data-stu-id="d55e6-127">Enrollment with Microsoft Intune or Mobile Device Management (MDM) for Office 365 requires registration.</span></span> <span data-ttu-id="d55e6-128">Pokud jste nakonfigurovali některou z těchto služeb **všechny** je vybraná a **NONE** není k dispozici...</span><span class="sxs-lookup"><span data-stu-id="d55e6-128">If you have configured either of these services, **ALL** is selected and **NONE** is not available..</span></span>

- <span data-ttu-id="d55e6-129">**Vyžadovat vícefaktorového ověřování pro připojení zařízení** – můžete zvolit, jestli uživatelé jsou nutné k zajištění druhý faktor ověřování pro připojení zařízení do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-129">**Require Multi-Factor Auth to join devices** - You can choose whether users are required to provide a second authentication factor to join their device to Azure AD.</span></span> <span data-ttu-id="d55e6-130">Výchozí hodnota je **ne**.</span><span class="sxs-lookup"><span data-stu-id="d55e6-130">The default is **No**.</span></span> <span data-ttu-id="d55e6-131">Doporučujeme, abyste při registraci zařízení, které vyžadují služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="d55e6-131">We recommend requiring multi-factor authentication when registering a device.</span></span> <span data-ttu-id="d55e6-132">Než povolíte službu Multi-Factor authentication pro tuto službu, je nutné zajistit, že aplikace Multi-Factor authentication je nakonfigurován pro uživatele, kteří registrovat svá zařízení.</span><span class="sxs-lookup"><span data-stu-id="d55e6-132">Before you enable multi-factor authentication for this service, you must ensure that multi-factor authentication is configured for the users that register their devices.</span></span> <span data-ttu-id="d55e6-133">Další informace o službám různých Azure Multi-Factor authentication, naleznete v části [Začínáme s Azure Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d55e6-133">For more information on different Azure multi-factor authentication services, see [getting started with Azure multi-factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md).</span></span> 

- <span data-ttu-id="d55e6-134">**Maximální počet zařízení** – toto nastavení umožňuje vybrat maximální počet zařízení, které uživatel může mít ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-134">**Maximum number of devices** - This setting enables you to select the maximum number of devices that a user can have in Azure AD.</span></span> <span data-ttu-id="d55e6-135">Pokud uživatel dosáhne této kvóty, budou se moct přidávat další zařízení, dokud jeden nebo více existující zařízení se odeberou.</span><span class="sxs-lookup"><span data-stu-id="d55e6-135">If a user reaches this quota, they are not be able to add additional devices until one or more of the existing devices are removed.</span></span> <span data-ttu-id="d55e6-136">Uvozovky zařízení se počítá pro všechna zařízení, které jsou připojené k Azure AD nebo dnes zaregistrovat Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-136">The device quote is counted for all devices that are either Azure AD joined or Azure AD registered today.</span></span> <span data-ttu-id="d55e6-137">Výchozí hodnota je **20**.</span><span class="sxs-lookup"><span data-stu-id="d55e6-137">The default value is **20**.</span></span>

- <span data-ttu-id="d55e6-138">**Uživatelé můžou synchronizovat nastavení a dat aplikací mezi zařízeními** – ve výchozím nastavení, toto nastavení je **NONE**.</span><span class="sxs-lookup"><span data-stu-id="d55e6-138">**Users may sync settings and app data across devices** - By default, this setting is set to **NONE**.</span></span> <span data-ttu-id="d55e6-139">Výběr konkrétní uživatele nebo skupiny nebo všechny umožňuje uživatelské nastavení a dat aplikací na synchronizaci přes své zařízení s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="d55e6-139">Selecting specific users or groups or ALL allows the user’s settings and app data to sync across their Windows 10 devices.</span></span> <span data-ttu-id="d55e6-140">Další informace o tom, jak funguje synchronizace ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="d55e6-140">Learn more on how sync works in Windows 10.</span></span>
<span data-ttu-id="d55e6-141">Tato možnost je k dispozici prostřednictvím produkty, jako je například Azure AD Premium nebo Enterprise Mobility Suite (EMS) premium funkce.</span><span class="sxs-lookup"><span data-stu-id="d55e6-141">This option is a premium capability available through products such as Azure AD Premium or the Enterprise Mobility Suite (EMS).</span></span>
 
    ![Spravovat zařízení s Intune](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a><span data-ttu-id="d55e6-143">Vyhledávání zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-143">Locate devices</span></span>

<span data-ttu-id="d55e6-144">Jako správce portálu Azure, máte dvě možnosti vyhledávání zaregistrovaná a připojené k zařízení:</span><span class="sxs-lookup"><span data-stu-id="d55e6-144">As an administrator, in the Azure portal, you have two options to locate registered and joined devices:</span></span>

- <span data-ttu-id="d55e6-145">**Všechna zařízení** v **spravovat** části **zařízení** okno</span><span class="sxs-lookup"><span data-stu-id="d55e6-145">**All devices** in the **Manage** section of the **Devices** blade</span></span>  

    ![Všechna zařízení](./media/device-management-azure-portal/41.png)


- <span data-ttu-id="d55e6-147">**Zařízení** v **spravovat** části **uživatele** okno</span><span class="sxs-lookup"><span data-stu-id="d55e6-147">**Devices** in the **Manage** section of a **User** blade</span></span>
 
    ![Všechna zařízení](./media/device-management-azure-portal/43.png)



<span data-ttu-id="d55e6-149">Obě možnosti můžete získat zobrazení který:</span><span class="sxs-lookup"><span data-stu-id="d55e6-149">With both options, you can get to a view that:</span></span>


- <span data-ttu-id="d55e6-150">Umožňuje vyhledat zařízení pomocí zobrazovaný název jako filtr.</span><span class="sxs-lookup"><span data-stu-id="d55e6-150">Enables you to search for devices using the display name as filter.</span></span>

- <span data-ttu-id="d55e6-151">Poskytuje podrobný přehled o zaregistrovaná a připojené k zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-151">Provides you with detailed overview of registered and joined devices</span></span>

- <span data-ttu-id="d55e6-152">Umožňuje provádět běžné úlohy správy zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-152">Enables you to perform common device management tasks</span></span>
   

![Všechna zařízení](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a><span data-ttu-id="d55e6-154">Úlohy správy zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-154">Device management tasks</span></span>

<span data-ttu-id="d55e6-155">Jako správce můžete spravovat registrovaná nebo připojeného k zařízení.</span><span class="sxs-lookup"><span data-stu-id="d55e6-155">As an administrator, you can manage the registered or joined devices.</span></span> <span data-ttu-id="d55e6-156">Tato část poskytuje informace o běžných úlohách správy zařízení.</span><span class="sxs-lookup"><span data-stu-id="d55e6-156">This section provides you with information about common device management tasks.</span></span>


<span data-ttu-id="d55e6-157">**Spravovat zařízení s Intune** – Pokud jste správce služby Intune, můžete spravovat zařízení, které jsou označeny jako **Microsoft Intune**.</span><span class="sxs-lookup"><span data-stu-id="d55e6-157">**Manage an Intune device** - If you are an Intune administrator, you can manage devices marked as **Microsoft Intune**.</span></span> <span data-ttu-id="d55e6-158">Správce můžete zobrazit další zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-158">An administrator can see additional device</span></span> 

![Spravovat zařízení s Intune](./media/device-management-azure-portal/31.png)


<span data-ttu-id="d55e6-160">**Povolit / zakázat zařízení s Azure AD**</span><span class="sxs-lookup"><span data-stu-id="d55e6-160">**Enable / disable an Azure AD device**</span></span>

<span data-ttu-id="d55e6-161">K povolení nebo zakázání zařízení, musíte být globálním správcem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-161">To enable or disable a device, you need to be a global administrator in Azure  AD.</span></span> <span data-ttu-id="d55e6-162">Zakázáním zařízení tomuto zařízení brání přístupu k prostředkům Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-162">Disabling a device prevents a device from accessing your Azure AD resources.</span></span>  <span data-ttu-id="d55e6-163">Chcete-li zakázat zařízení, můžete buďto kliknout na *...*</span><span class="sxs-lookup"><span data-stu-id="d55e6-163">To disable the device, you can either click *…*</span></span> <span data-ttu-id="d55e6-164">Klikněte na zařízení pro další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d55e6-164">click the device for additional details.</span></span>

 
![Spravovat zařízení s Intune](./media/device-management-azure-portal/33.png)

<span data-ttu-id="d55e6-166">Zakázáním zařízení se stavem v změní **povoleno** sloupec, který se **ne**.</span><span class="sxs-lookup"><span data-stu-id="d55e6-166">Disabling a device changes the state in the **ENABLED** column to **No**.</span></span>

![Zakázat zařízení](./media/device-management-azure-portal/32.png)


<span data-ttu-id="d55e6-168">**Odstranění zařízení s Azure AD** – Pokud chcete odstranit zařízení, musíte být globálním správcem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-168">**Delete an Azure AD device** - To delete a device, you need to be a global administrator in Azure AD.</span></span>  
<span data-ttu-id="d55e6-169">Odstranění zařízení:</span><span class="sxs-lookup"><span data-stu-id="d55e6-169">Deleting a device:</span></span>
 
- <span data-ttu-id="d55e6-170">Tomuto zařízení brání přístupu k prostředkům Azure AD</span><span class="sxs-lookup"><span data-stu-id="d55e6-170">Prevents a device from accessing your Azure AD resources</span></span> 

- <span data-ttu-id="d55e6-171">Odebere všechny podrobnosti, které jsou připojené k zařízení, například, klíče Bitlockeru pro zařízení s Windows</span><span class="sxs-lookup"><span data-stu-id="d55e6-171">Removes all details that are attached to the device, for example, BitLocker keys for Windows devices</span></span>  

- <span data-ttu-id="d55e6-172">Reprezentuje neobnovitelná aktivity a se nedoporučuje, pokud je potřeba</span><span class="sxs-lookup"><span data-stu-id="d55e6-172">Represents a non-recoverable activity and is not recommended unless it is required</span></span>

<span data-ttu-id="d55e6-173">Pokud se zařízení spravuje jiná autoritu pro správu (např. Microsoft Intune), ujistěte se, že má zařízení vymazání / vyřazení před odstraněním zařízení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-173">If a device is managed by another management authority (e.g. Microsoft Intune), please make sure that the device has been wiped / retired before deleting the device in Azure AD.</span></span>

<span data-ttu-id="d55e6-174">Můžete buď vybrat "..."</span><span class="sxs-lookup"><span data-stu-id="d55e6-174">You can either select “…”</span></span> <span data-ttu-id="d55e6-175">Odstranit zařízení nebo klikněte na zařízení pro další podrobnosti</span><span class="sxs-lookup"><span data-stu-id="d55e6-175">to delete the device or click on the device for additional details</span></span>
 
![Odstranění zařízení](./media/device-management-azure-portal/34.png)


<span data-ttu-id="d55e6-177">**Zobrazení nebo zkopírujte ID zařízení** -ID zařízení můžete použít k ověření informace ID zařízení na zařízení nebo pomocí prostředí PowerShell při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="d55e6-177">**View or copy device ID** - You can use a device ID to verify the device ID details on the device or using PowerShell during troubleshooting.</span></span> <span data-ttu-id="d55e6-178">Pro přístup k možnosti kopírování, klikněte na zařízení.</span><span class="sxs-lookup"><span data-stu-id="d55e6-178">To access the copy option, click the device.</span></span>

![Zobrazit ID zařízení](./media/device-management-azure-portal/35.png)
  

<span data-ttu-id="d55e6-180">**Zobrazení nebo kopírování klíče Bitlockeru** – Pokud jste správce, můžete zobrazit a zkopírujte tyto klíče nástroje BitLocker pomoct uživatelům obnovit svá šifrované jednotce.</span><span class="sxs-lookup"><span data-stu-id="d55e6-180">**View or copy BitLocker keys** - If you are an administrator, you can view and copy the BitLocker keys to help users to recover their encrypted drive.</span></span> <span data-ttu-id="d55e6-181">Tyto klíče jsou dostupné pouze pro zařízení s Windows, které jsou zašifrované a jejich klíče uloženého ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d55e6-181">These keys are only available for Windows devices that are encrypted and have their keys stored in Azure AD.</span></span> <span data-ttu-id="d55e6-182">Při přístupu k podrobnosti o zařízení, můžete zkopírovat tyto klíče.</span><span class="sxs-lookup"><span data-stu-id="d55e6-182">You can copy these keys when accessing details of the device.</span></span>
 
![Klíče Bitlockeru zobrazení](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a><span data-ttu-id="d55e6-184">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="d55e6-184">Audit logs</span></span>


<span data-ttu-id="d55e6-185">Aktivity zařízení jsou dostupné přes protokoly aktivity.</span><span class="sxs-lookup"><span data-stu-id="d55e6-185">The device activities are available through the activity logs.</span></span> <span data-ttu-id="d55e6-186">To zahrnuje aktivity aktivaci služby registrace zařízení nebo uživatele:</span><span class="sxs-lookup"><span data-stu-id="d55e6-186">This includes activities triggered by the device registration service or by the user:</span></span>

- <span data-ttu-id="d55e6-187">Vytváření zařízení a přidání vlastníků/uživatelů na zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-187">Device creation and adding owners/users on the device</span></span>

- <span data-ttu-id="d55e6-188">Změny nastavení zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-188">Changes to device settings</span></span>

- <span data-ttu-id="d55e6-189">Operace zařízení například odstranění nebo aktualizaci zařízení</span><span class="sxs-lookup"><span data-stu-id="d55e6-189">Device operations such as deleting or updating a device</span></span>
 
<span data-ttu-id="d55e6-190">Vašim vstupním bodem k datům auditování je **protokoly auditu** v **aktivity** části **zařízení* okno.</span><span class="sxs-lookup"><span data-stu-id="d55e6-190">Your entry point to the auditing data is **Audit logs** in the **Activity** section of the **Devices* blade.</span></span>

![Protokoly auditu](./media/device-management-azure-portal/61.png)


<span data-ttu-id="d55e6-192">Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="d55e6-192">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="d55e6-193">datum a čas výskytu</span><span class="sxs-lookup"><span data-stu-id="d55e6-193">the date and time of the occurrence</span></span>

- <span data-ttu-id="d55e6-194">cíle</span><span class="sxs-lookup"><span data-stu-id="d55e6-194">the targets</span></span>

- <span data-ttu-id="d55e6-195">iniciátoru nebo objektu actor (který) aktivity</span><span class="sxs-lookup"><span data-stu-id="d55e6-195">the initiator / actor (who) of an activity</span></span>

- <span data-ttu-id="d55e6-196">aktivity (co)</span><span class="sxs-lookup"><span data-stu-id="d55e6-196">the activity (what)</span></span>

![Protokoly auditu](./media/device-management-azure-portal/63.png)

<span data-ttu-id="d55e6-198">Zobrazení seznamu můžete upravit kliknutím na **Sloupce** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="d55e6-198">You can customize the list view by clicking **Columns** in the toolbar.</span></span>
 
![Protokoly auditu](./media/device-management-azure-portal/64.png)


<span data-ttu-id="d55e6-200">Abyste omezili zobrazovaná data na úroveň, která vám vyhovuje, můžete filtrovat data přihlašování s využitím následujících polí:</span><span class="sxs-lookup"><span data-stu-id="d55e6-200">To narrow down the reported data to a level that works for you, you can filter the audit data using the following fields:</span></span>

- <span data-ttu-id="d55e6-201">Catergory</span><span class="sxs-lookup"><span data-stu-id="d55e6-201">Catergory</span></span>
- <span data-ttu-id="d55e6-202">Typ prostředku aktivity</span><span class="sxs-lookup"><span data-stu-id="d55e6-202">Activity resource type</span></span>
- <span data-ttu-id="d55e6-203">Aktivita</span><span class="sxs-lookup"><span data-stu-id="d55e6-203">Activity</span></span>
- <span data-ttu-id="d55e6-204">Rozsah dat</span><span class="sxs-lookup"><span data-stu-id="d55e6-204">Date range</span></span>
- <span data-ttu-id="d55e6-205">cíl</span><span class="sxs-lookup"><span data-stu-id="d55e6-205">Target</span></span>
- <span data-ttu-id="d55e6-206">Zahájit (objektu Actor)</span><span class="sxs-lookup"><span data-stu-id="d55e6-206">Initiated By (Actor)</span></span>

<span data-ttu-id="d55e6-207">Kromě filtry můžete vyhledat konkrétní položky.</span><span class="sxs-lookup"><span data-stu-id="d55e6-207">In addition to the filters, you can search for specific entries.</span></span>

![Protokoly auditu](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a><span data-ttu-id="d55e6-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d55e6-209">Next steps</span></span>

* [<span data-ttu-id="d55e6-210">Úvod do správy zařízení v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d55e6-210">Introduction to device management in Azure Active Directory</span></span>](device-management-introduction.md)



