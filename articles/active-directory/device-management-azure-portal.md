---
title: "hello aaaManaging zařízení pomocí portálu Azure - preview | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure portálu toomanage zařízení."
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
ms.openlocfilehash: a39d14e4ce8bb79f0223a9de40d5f1259a869927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-devices-using-hello-azure-portal---preview"></a><span data-ttu-id="924da-103">Správa zařízení pomocí portálu Azure hello – náhled</span><span class="sxs-lookup"><span data-stu-id="924da-103">Managing devices using hello Azure portal - preview</span></span>

>[!NOTE]
><span data-ttu-id="924da-104">Tato funkce je aktuálně ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="924da-104">This capability currently is in public preview.</span></span> <span data-ttu-id="924da-105">Připravit toorevert nebo odeberte všechny změny.</span><span class="sxs-lookup"><span data-stu-id="924da-105">Be prepared toorevert or remove any changes.</span></span> <span data-ttu-id="924da-106">Hello funkce je dostupná v libovolné předplatné služby Azure Active Directory (Azure AD) verzi public Preview.</span><span class="sxs-lookup"><span data-stu-id="924da-106">hello feature is available in any Azure Active Directory (Azure AD) subscription during public preview.</span></span> <span data-ttu-id="924da-107">Když funkce hello je obecně dostupná, může vyžadovat některé aspekty funkcí hello však předplatné služby Azure Active Directory premium.</span><span class="sxs-lookup"><span data-stu-id="924da-107">However, when hello feature becomes generally available, some aspects of hello feature might require an Azure Active Directory premium subscription.</span></span>


<span data-ttu-id="924da-108">Se správou zařízení ve službě Azure Active Directory (Azure AD) můžete zajistit, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="924da-108">With device management in Azure Active Directory (Azure AD), you can ensure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> 

<span data-ttu-id="924da-109">V tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="924da-109">This topic:</span></span>

- <span data-ttu-id="924da-110">Předpokládá, že jste obeznámeni s hello [Úvod toodevice správy ve službě Azure Active Directory](device-management-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="924da-110">Assumes that you are familiar with hello [introduction toodevice management in Azure Active Directory](device-management-introduction.md)</span></span>

- <span data-ttu-id="924da-111">Zajišťuje, že jste se informace o správě zařízení pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="924da-111">Provides you with information about managing your devices using hello Azure portal</span></span>


<span data-ttu-id="924da-112">toomanage zařízení v hello portálu Azure, budete potřebovat tooclick **zařízení** v hello **spravovat** části hello hello **Azure Active Directory** okno.</span><span class="sxs-lookup"><span data-stu-id="924da-112">toomanage devices in hello Azure portal, you need tooclick **Devices** in hello **Manage** section of hello hello **Azure Active Directory** blade.</span></span>

![Spravovat zařízení s Intune](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a><span data-ttu-id="924da-114">Konfigurace nastavení zařízení</span><span class="sxs-lookup"><span data-stu-id="924da-114">Configure device settings</span></span>

<span data-ttu-id="924da-115">toomanage zařízení pomocí hello portálu Azure, které potřebují toobe zaregistrovaný nebo připojený k tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-115">toomanage your devices using hello Azure portal, they need toobe either registered or joined tooAzure AD.</span></span> <span data-ttu-id="924da-116">Jako správce můžete upřesnit hello proces registrace a připojení zařízení tak, že nakonfigurujete nastavení zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="924da-116">As an administrator, you can fine-tune hello process of registering and joining devices by configuring hello device settings.</span></span>

![Spravovat zařízení s Intune](./media/device-management-azure-portal/22.png)


<span data-ttu-id="924da-118">okno nastavení zařízení Hello umožňuje tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="924da-118">hello device settings blade enables you tooconfigure:</span></span>

- <span data-ttu-id="924da-119">**Uživatelé mohou připojit zařízení tooAzure AD** – toto nastavení vám umožní tooselect hello uživatelů, kteří mohou připojit zařízení tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-119">**Users may join devices tooAzure AD** - This settings enables you tooselect hello users who can join devices tooAzure AD.</span></span> <span data-ttu-id="924da-120">Výchozí hodnota Hello je **všechny**.</span><span class="sxs-lookup"><span data-stu-id="924da-120">hello default is **All**.</span></span>

- <span data-ttu-id="924da-121">**Zařízení připojená k další místní správci v Azure AD** -vyberete hello uživatele, kteří jsou udělena práva místního správce v zařízení.</span><span class="sxs-lookup"><span data-stu-id="924da-121">**Additional local administrators on Azure AD joined devices** - You can select hello users that are granted local administrator rights on a device.</span></span> <span data-ttu-id="924da-122">Přidá toohello uživatelů tady přidat *Správci zařízení* role ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-122">Users added here are added toohello *Device Administrators* role in Azure AD.</span></span> <span data-ttu-id="924da-123">Globální správci ve službě Azure AD a vlastníci zařízení jsou udělena práva místního správce ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="924da-123">Global administrators in Azure AD and device owners are granted local administrator rights by default.</span></span> <span data-ttu-id="924da-124">Tato možnost je k dispozici prostřednictvím produkty, jako je například Azure AD Premium nebo Enterprise Mobility Suite (EMS) hello funkce edice premium.</span><span class="sxs-lookup"><span data-stu-id="924da-124">This option is a premium edition capability available through products such as Azure AD Premium or hello Enterprise Mobility Suite (EMS).</span></span> 

- <span data-ttu-id="924da-125">**Uživatelé mohou registrovat svá zařízení s Azure AD** -potřebujete tooconfigure tato nastavení tooallow zařízení toobe registrované s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-125">**Users may register their devices with Azure AD** - You need tooconfigure this setting tooallow devices toobe registered with Azure AD.</span></span> <span data-ttu-id="924da-126">Pokud vyberete **žádné**, zařízení nejsou povoleny tooregister, které nejsou připojené k Azure AD nebo hybridní připojený k Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-126">If you select **None**, devices are not allowed tooregister when they are not Azure AD joined or hybrid Azure AD joined.</span></span> <span data-ttu-id="924da-127">Registrace s Microsoft Intune nebo správy mobilních zařízení (MDM) pro Office 365 vyžaduje registraci.</span><span class="sxs-lookup"><span data-stu-id="924da-127">Enrollment with Microsoft Intune or Mobile Device Management (MDM) for Office 365 requires registration.</span></span> <span data-ttu-id="924da-128">Pokud jste nakonfigurovali některou z těchto služeb **všechny** je vybraná a **NONE** není k dispozici...</span><span class="sxs-lookup"><span data-stu-id="924da-128">If you have configured either of these services, **ALL** is selected and **NONE** is not available..</span></span>

- <span data-ttu-id="924da-129">**Vyžadovat, aby zařízení toojoin Multi-Factor Auth** – můžete zvolit, jestli uživatelé jsou požadované tooprovide druhé ověření zohlednit toojoin jejich zařízení tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-129">**Require Multi-Factor Auth toojoin devices** - You can choose whether users are required tooprovide a second authentication factor toojoin their device tooAzure AD.</span></span> <span data-ttu-id="924da-130">Výchozí hodnota Hello je **ne**.</span><span class="sxs-lookup"><span data-stu-id="924da-130">hello default is **No**.</span></span> <span data-ttu-id="924da-131">Doporučujeme, abyste při registraci zařízení, které vyžadují služby Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="924da-131">We recommend requiring multi-factor authentication when registering a device.</span></span> <span data-ttu-id="924da-132">Než povolíte službu Multi-Factor authentication pro tuto službu, je nutné zajistit, že aplikace Multi-Factor authentication je nakonfigurován pro hello uživatele, kteří registrovat svá zařízení.</span><span class="sxs-lookup"><span data-stu-id="924da-132">Before you enable multi-factor authentication for this service, you must ensure that multi-factor authentication is configured for hello users that register their devices.</span></span> <span data-ttu-id="924da-133">Další informace o službám různých Azure Multi-Factor authentication, naleznete v části [Začínáme s Azure Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="924da-133">For more information on different Azure multi-factor authentication services, see [getting started with Azure multi-factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md).</span></span> 

- <span data-ttu-id="924da-134">**Maximální počet zařízení** – toto nastavení vám umožní tooselect hello maximální počet zařízení, které uživatel může mít ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-134">**Maximum number of devices** - This setting enables you tooselect hello maximum number of devices that a user can have in Azure AD.</span></span> <span data-ttu-id="924da-135">Pokud uživatel dosáhne této kvóty, ale nejsou měly mít tooadd další zařízení až jeden nebo více hello stávajících zařízení se odeberou.</span><span class="sxs-lookup"><span data-stu-id="924da-135">If a user reaches this quota, they are not be able tooadd additional devices until one or more of hello existing devices are removed.</span></span> <span data-ttu-id="924da-136">pro všechna zařízení, které jsou připojené k Azure AD nebo dnes zaregistrovat Azure AD se počítá Hello zařízení uvozovky.</span><span class="sxs-lookup"><span data-stu-id="924da-136">hello device quote is counted for all devices that are either Azure AD joined or Azure AD registered today.</span></span> <span data-ttu-id="924da-137">Hello výchozí hodnota je **20**.</span><span class="sxs-lookup"><span data-stu-id="924da-137">hello default value is **20**.</span></span>

- <span data-ttu-id="924da-138">**Uživatelé můžou synchronizovat nastavení a dat aplikací mezi zařízeními** – ve výchozím nastavení, toto nastavení je příliš**NONE**.</span><span class="sxs-lookup"><span data-stu-id="924da-138">**Users may sync settings and app data across devices** - By default, this setting is set too**NONE**.</span></span> <span data-ttu-id="924da-139">Výběr konkrétní uživatele nebo skupiny nebo všechny umožňuje nastavení a data toosync aplikace hello uživatele na jejich zařízeních Windows 10.</span><span class="sxs-lookup"><span data-stu-id="924da-139">Selecting specific users or groups or ALL allows hello user’s settings and app data toosync across their Windows 10 devices.</span></span> <span data-ttu-id="924da-140">Další informace o tom, jak funguje synchronizace ve Windows 10.</span><span class="sxs-lookup"><span data-stu-id="924da-140">Learn more on how sync works in Windows 10.</span></span>
<span data-ttu-id="924da-141">Tato možnost je k dispozici prostřednictvím produkty, jako je například Azure AD Premium nebo Enterprise Mobility Suite (EMS) hello premium funkce.</span><span class="sxs-lookup"><span data-stu-id="924da-141">This option is a premium capability available through products such as Azure AD Premium or hello Enterprise Mobility Suite (EMS).</span></span>
 
    ![Spravovat zařízení s Intune](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a><span data-ttu-id="924da-143">Vyhledávání zařízení</span><span class="sxs-lookup"><span data-stu-id="924da-143">Locate devices</span></span>

<span data-ttu-id="924da-144">Jako správce v hello portál Azure, máte dvě možnosti toolocate zaregistrován a připojené k zařízení:</span><span class="sxs-lookup"><span data-stu-id="924da-144">As an administrator, in hello Azure portal, you have two options toolocate registered and joined devices:</span></span>

- <span data-ttu-id="924da-145">**Všechna zařízení** v hello **spravovat** části hello **zařízení** okno</span><span class="sxs-lookup"><span data-stu-id="924da-145">**All devices** in hello **Manage** section of hello **Devices** blade</span></span>  

    ![Všechna zařízení](./media/device-management-azure-portal/41.png)


- <span data-ttu-id="924da-147">**Zařízení** v hello **spravovat** části **uživatele** okno</span><span class="sxs-lookup"><span data-stu-id="924da-147">**Devices** in hello **Manage** section of a **User** blade</span></span>
 
    ![Všechna zařízení](./media/device-management-azure-portal/43.png)



<span data-ttu-id="924da-149">Obě možnosti můžete získat zobrazení tooa který:</span><span class="sxs-lookup"><span data-stu-id="924da-149">With both options, you can get tooa view that:</span></span>


- <span data-ttu-id="924da-150">Umožní vám toosearch pro zařízení pomocí hello zobrazovaný název jako filtr.</span><span class="sxs-lookup"><span data-stu-id="924da-150">Enables you toosearch for devices using hello display name as filter.</span></span>

- <span data-ttu-id="924da-151">Poskytuje podrobný přehled o zaregistrovaná a připojené k zařízení</span><span class="sxs-lookup"><span data-stu-id="924da-151">Provides you with detailed overview of registered and joined devices</span></span>

- <span data-ttu-id="924da-152">Umožní vám tooperform běžné úlohy správy zařízení</span><span class="sxs-lookup"><span data-stu-id="924da-152">Enables you tooperform common device management tasks</span></span>
   

![Všechna zařízení](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a><span data-ttu-id="924da-154">Úlohy správy zařízení</span><span class="sxs-lookup"><span data-stu-id="924da-154">Device management tasks</span></span>

<span data-ttu-id="924da-155">Jako správce, můžete spravovat hello zaregistrovaný nebo zařízení připojená k.</span><span class="sxs-lookup"><span data-stu-id="924da-155">As an administrator, you can manage hello registered or joined devices.</span></span> <span data-ttu-id="924da-156">Tato část poskytuje informace o běžných úlohách správy zařízení.</span><span class="sxs-lookup"><span data-stu-id="924da-156">This section provides you with information about common device management tasks.</span></span>


<span data-ttu-id="924da-157">**Spravovat zařízení s Intune** – Pokud jste správce služby Intune, můžete spravovat zařízení, které jsou označeny jako **Microsoft Intune**.</span><span class="sxs-lookup"><span data-stu-id="924da-157">**Manage an Intune device** - If you are an Intune administrator, you can manage devices marked as **Microsoft Intune**.</span></span> <span data-ttu-id="924da-158">Správce můžete zobrazit další zařízení</span><span class="sxs-lookup"><span data-stu-id="924da-158">An administrator can see additional device</span></span> 

![Spravovat zařízení s Intune](./media/device-management-azure-portal/31.png)


<span data-ttu-id="924da-160">**Povolit / zakázat zařízení s Azure AD**</span><span class="sxs-lookup"><span data-stu-id="924da-160">**Enable / disable an Azure AD device**</span></span>

<span data-ttu-id="924da-161">tooenable nebo zakázat zařízení, je třeba toobe globálního správce ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-161">tooenable or disable a device, you need toobe a global administrator in Azure  AD.</span></span> <span data-ttu-id="924da-162">Zakázáním zařízení tomuto zařízení brání přístupu k prostředkům Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-162">Disabling a device prevents a device from accessing your Azure AD resources.</span></span>  <span data-ttu-id="924da-163">toodisable hello zařízení, můžete buďto kliknout na *...*</span><span class="sxs-lookup"><span data-stu-id="924da-163">toodisable hello device, you can either click *…*</span></span> <span data-ttu-id="924da-164">Klikněte na zařízení hello další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="924da-164">click hello device for additional details.</span></span>

 
![Spravovat zařízení s Intune](./media/device-management-azure-portal/33.png)

<span data-ttu-id="924da-166">Zakázáním zařízení se změní stav hello v hello **povoleno** sloupec příliš**ne**.</span><span class="sxs-lookup"><span data-stu-id="924da-166">Disabling a device changes hello state in hello **ENABLED** column too**No**.</span></span>

![Zakázat zařízení](./media/device-management-azure-portal/32.png)


<span data-ttu-id="924da-168">**Odstranění zařízení s Azure AD** -toodelete zařízení, je nutné toobe globálního správce ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-168">**Delete an Azure AD device** - toodelete a device, you need toobe a global administrator in Azure AD.</span></span>  
<span data-ttu-id="924da-169">Odstranění zařízení:</span><span class="sxs-lookup"><span data-stu-id="924da-169">Deleting a device:</span></span>
 
- <span data-ttu-id="924da-170">Tomuto zařízení brání přístupu k prostředkům Azure AD</span><span class="sxs-lookup"><span data-stu-id="924da-170">Prevents a device from accessing your Azure AD resources</span></span> 

- <span data-ttu-id="924da-171">Odebere všechny podrobnosti, které jsou připojené toohello zařízení, například, klíče Bitlockeru pro zařízení s Windows</span><span class="sxs-lookup"><span data-stu-id="924da-171">Removes all details that are attached toohello device, for example, BitLocker keys for Windows devices</span></span>  

- <span data-ttu-id="924da-172">Reprezentuje neobnovitelná aktivity a se nedoporučuje, pokud je potřeba</span><span class="sxs-lookup"><span data-stu-id="924da-172">Represents a non-recoverable activity and is not recommended unless it is required</span></span>

<span data-ttu-id="924da-173">Pokud se zařízení spravuje jiná autoritu pro správu (např. Microsoft Intune), Zkontrolujte prosím, že zařízení hello byl vymazání / vyřazení před odstraněním hello zařízení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-173">If a device is managed by another management authority (e.g. Microsoft Intune), please make sure that hello device has been wiped / retired before deleting hello device in Azure AD.</span></span>

<span data-ttu-id="924da-174">Můžete buď vybrat "..."</span><span class="sxs-lookup"><span data-stu-id="924da-174">You can either select “…”</span></span> <span data-ttu-id="924da-175">Klikněte na zařízení hello další podrobnosti nebo toodelete hello zařízení</span><span class="sxs-lookup"><span data-stu-id="924da-175">toodelete hello device or click on hello device for additional details</span></span>
 
![Odstranění zařízení](./media/device-management-azure-portal/34.png)


<span data-ttu-id="924da-177">**Zobrazení nebo zkopírujte ID zařízení** -ID podrobnosti zařízení ID tooverify hello zařízení můžete použít na hello zařízení nebo pomocí prostředí PowerShell při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="924da-177">**View or copy device ID** - You can use a device ID tooverify hello device ID details on hello device or using PowerShell during troubleshooting.</span></span> <span data-ttu-id="924da-178">tooaccess hello kopie možnost, klikněte na tlačítko hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="924da-178">tooaccess hello copy option, click hello device.</span></span>

![Zobrazit ID zařízení](./media/device-management-azure-portal/35.png)
  

<span data-ttu-id="924da-180">**Zobrazení nebo kopírování klíče Bitlockeru** – Pokud jste správce, můžete zobrazit kopie hello BitLocker klíče toohelp uživatelé toorecover jejich šifrované jednotce.</span><span class="sxs-lookup"><span data-stu-id="924da-180">**View or copy BitLocker keys** - If you are an administrator, you can view and copy hello BitLocker keys toohelp users toorecover their encrypted drive.</span></span> <span data-ttu-id="924da-181">Tyto klíče jsou dostupné pouze pro zařízení s Windows, které jsou zašifrované a jejich klíče uloženého ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="924da-181">These keys are only available for Windows devices that are encrypted and have their keys stored in Azure AD.</span></span> <span data-ttu-id="924da-182">Při přístupu k podrobnosti hello zařízení, můžete zkopírovat tyto klíče.</span><span class="sxs-lookup"><span data-stu-id="924da-182">You can copy these keys when accessing details of hello device.</span></span>
 
![Klíče Bitlockeru zobrazení](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a><span data-ttu-id="924da-184">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="924da-184">Audit logs</span></span>


<span data-ttu-id="924da-185">Hello zařízení aktivity jsou dostupné přes protokoly aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="924da-185">hello device activities are available through hello activity logs.</span></span> <span data-ttu-id="924da-186">To zahrnuje aktivity spustí služba hello registrace zařízení nebo uživatel hello:</span><span class="sxs-lookup"><span data-stu-id="924da-186">This includes activities triggered by hello device registration service or by hello user:</span></span>

- <span data-ttu-id="924da-187">Vytváření zařízení a přidání vlastníků/uživatelů zařízení hello</span><span class="sxs-lookup"><span data-stu-id="924da-187">Device creation and adding owners/users on hello device</span></span>

- <span data-ttu-id="924da-188">Změny nastavení toodevice</span><span class="sxs-lookup"><span data-stu-id="924da-188">Changes toodevice settings</span></span>

- <span data-ttu-id="924da-189">Operace zařízení například odstranění nebo aktualizaci zařízení</span><span class="sxs-lookup"><span data-stu-id="924da-189">Device operations such as deleting or updating a device</span></span>
 
<span data-ttu-id="924da-190">Vaše vstupní bod toohello auditování dat je **protokoly auditu** v hello **aktivity** části hello **zařízení* okno.</span><span class="sxs-lookup"><span data-stu-id="924da-190">Your entry point toohello auditing data is **Audit logs** in hello **Activity** section of hello **Devices* blade.</span></span>

![Protokoly auditu](./media/device-management-azure-portal/61.png)


<span data-ttu-id="924da-192">Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="924da-192">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="924da-193">Hello datum a čas výskytu hello</span><span class="sxs-lookup"><span data-stu-id="924da-193">hello date and time of hello occurrence</span></span>

- <span data-ttu-id="924da-194">Hello cíle</span><span class="sxs-lookup"><span data-stu-id="924da-194">hello targets</span></span>

- <span data-ttu-id="924da-195">Hello iniciátor nebo objektu actor (který) aktivity</span><span class="sxs-lookup"><span data-stu-id="924da-195">hello initiator / actor (who) of an activity</span></span>

- <span data-ttu-id="924da-196">Hello aktivity, (co)</span><span class="sxs-lookup"><span data-stu-id="924da-196">hello activity (what)</span></span>

![Protokoly auditu](./media/device-management-azure-portal/63.png)

<span data-ttu-id="924da-198">Kliknutím můžete přizpůsobit zobrazení seznamu hello **sloupce** v panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="924da-198">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>
 
![Protokoly auditu](./media/device-management-azure-portal/64.png)


<span data-ttu-id="924da-200">toonarrow dolů hello nahlášené tooa dat na úrovni, funguje pro vás, můžete filtrovat data auditu hello pomocí hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="924da-200">toonarrow down hello reported data tooa level that works for you, you can filter hello audit data using hello following fields:</span></span>

- <span data-ttu-id="924da-201">Catergory</span><span class="sxs-lookup"><span data-stu-id="924da-201">Catergory</span></span>
- <span data-ttu-id="924da-202">Typ prostředku aktivity</span><span class="sxs-lookup"><span data-stu-id="924da-202">Activity resource type</span></span>
- <span data-ttu-id="924da-203">Aktivita</span><span class="sxs-lookup"><span data-stu-id="924da-203">Activity</span></span>
- <span data-ttu-id="924da-204">Rozsah dat</span><span class="sxs-lookup"><span data-stu-id="924da-204">Date range</span></span>
- <span data-ttu-id="924da-205">cíl</span><span class="sxs-lookup"><span data-stu-id="924da-205">Target</span></span>
- <span data-ttu-id="924da-206">Zahájit (objektu Actor)</span><span class="sxs-lookup"><span data-stu-id="924da-206">Initiated By (Actor)</span></span>

<span data-ttu-id="924da-207">Kromě toho toohello filtrů, můžete vyhledat konkrétní položky.</span><span class="sxs-lookup"><span data-stu-id="924da-207">In addition toohello filters, you can search for specific entries.</span></span>

![Protokoly auditu](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a><span data-ttu-id="924da-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="924da-209">Next steps</span></span>

* [<span data-ttu-id="924da-210">Úvod toodevice správy ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="924da-210">Introduction toodevice management in Azure Active Directory</span></span>](device-management-introduction.md)



