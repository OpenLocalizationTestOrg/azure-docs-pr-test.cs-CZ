---
title: "aaaHow tooconfigure Automatická registrace zařízení připojených k doméně systému Windows s Azure Active Directory | Microsoft Docs"
description: "Nastavte vaší doméně Windows zařízení tooregister automaticky a bezobslužně s Azure Active Directory."
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
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1d736eba734418231f12e23a8fc1a93405f129c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a><span data-ttu-id="5718d-103">Začínáme s registrací zařízení ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5718d-103">Get started with Azure Active Directory device registration</span></span>

<span data-ttu-id="5718d-104">Registrace zařízení služby Azure Active Directory je hello základem pro scénáře podmíněného přístupu podle zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-104">Azure Active Directory device registration is hello foundation for device-based conditional access scenarios.</span></span> <span data-ttu-id="5718d-105">Když je zařízení registrováno, poskytne mu nástroj registrace zařízení služby Azure Active Directory hello zařízení s identitou, která je použité tooauthenticate hello zařízení při přihlášení uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="5718d-105">When a device is registered, Azure Active Directory device registration provides hello device with an identity which is used tooauthenticate hello device when hello user signs in.</span></span> <span data-ttu-id="5718d-106">Hello ověření zařízení a atributy hello hello zařízení, lze potom použít tooenforce zásady podmíněného přístupu pro aplikace, které jsou hostované v cloudu hello a místní.</span><span class="sxs-lookup"><span data-stu-id="5718d-106">hello authenticated device, and hello attributes of hello device, can then be used tooenforce conditional access policies for applications that are hosted in hello cloud and on-premises.</span></span>

<span data-ttu-id="5718d-107">V kombinaci s řešením správy mobilních zařízení, jako je například Microsoft Intune, hello atributy zařízení ve službě Azure Active Directory jsou aktualizované o další informace o zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="5718d-107">When combined with a mobile device management(MDM) solution such as Microsoft Intune, hello device attributes in Azure Active Directory are updated with additional information about hello device.</span></span> <span data-ttu-id="5718d-108">Díky tomu můžete toocreate pravidla podmíněného přístupu, které vynucují přístup ze zařízení toomeet vaše standardy zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="5718d-108">This allows you toocreate conditional access rules that enforce access from devices toomeet your standards for security and compliance.</span></span> <span data-ttu-id="5718d-109">Další informace o registraci zařízení v Microsoft Intune najdete v tématu registrace zařízení pro správu v Intune.</span><span class="sxs-lookup"><span data-stu-id="5718d-109">For more information on enrolling devices in Microsoft Intune, see Enroll devices for management in Intune.</span></span>
<span data-ttu-id="5718d-110">Scénáře pro nástroj pomocí Azure Active Directory zařízení registrace Azure Active Directory Device Registration zahrnuje podporu pro iOS, Android a Windows zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-110">Scenarios enabled by Azure Active Directory Device Registration Azure Active Directory Device Registration includes support for iOS, Android, and Windows devices.</span></span> <span data-ttu-id="5718d-111">Hello jednotlivé scénáře použití nástroje registrace zařízení služby Azure AD mohou mít konkrétněji určené požadavky a podporované platformy.</span><span class="sxs-lookup"><span data-stu-id="5718d-111">hello individual scenarios that utilize Azure AD Device Registration may have more specific requirements and platform support.</span></span> 

<span data-ttu-id="5718d-112">Jedná se o následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="5718d-112">These scenarios are as follows:</span></span>

- <span data-ttu-id="5718d-113">**Podmíněný přístup pro aplikace Office 365 s Microsoft Intune:** správci IT mohou poskytnout podmíněného přístupu zařízení zásady toosecure podnikovým prostředkům, zatímco v hello stejný čas povolení pracovníkům s vhodnými zařízeními tooaccess hello služby.</span><span class="sxs-lookup"><span data-stu-id="5718d-113">**Conditional access for Office 365 applications with Microsoft Intune:** IT admins can provision conditional access device policies toosecure corporate resources, while at hello same time allowing information workers on compliant devices tooaccess hello services.</span></span> <span data-ttu-id="5718d-114">Další informace naleznete v tématu Zásady podmíněného přístupu zařízení pro služby Office 365.</span><span class="sxs-lookup"><span data-stu-id="5718d-114">For more information, see Conditional Access Device Policies for Office 365 services.</span></span>

- <span data-ttu-id="5718d-115">**Tooapplications podmíněného přístupu, které jsou hostované na místě:** registrovaná zařízení můžete použít se zásadami přístupu pro aplikace, které jsou nakonfigurované toouse služby AD FS v systému Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="5718d-115">**Conditional access tooapplications that are hosted on-premises:** You can use registered devices with access policies for applications that are configured toouse AD FS with Windows Server 2012 R2.</span></span> <span data-ttu-id="5718d-116">Další informace o nastavení podmíněného přístupu pro místní úložiště naleznete v tématu [Nastavení místního podmíněného přístupu pomocí registrace zařízení služby Azure Active Directory](active-directory-device-registration-on-premises-setup.md).</span><span class="sxs-lookup"><span data-stu-id="5718d-116">For more information about setting up conditional access for on-premises, see [Setting up On-premises Conditional Access using Azure Active Directory device registration](active-directory-device-registration-on-premises-setup.md).</span></span>

## <a name="setting-up-azure-active-directory-device-registration"></a><span data-ttu-id="5718d-117">Nastavení nástroje Registrace zařízení ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5718d-117">Setting up Azure Active Directory Device Registration</span></span>

<span data-ttu-id="5718d-118">registrace zařízení toosetup, máte několik možností:</span><span class="sxs-lookup"><span data-stu-id="5718d-118">toosetup device registration, you have multiple options:</span></span>

- <span data-ttu-id="5718d-119">Můžete zaregistrovat zařízení, kdy připojený k tooAzure AD domény.</span><span class="sxs-lookup"><span data-stu-id="5718d-119">Devices can register when joined tooAzure AD domain.</span></span> <span data-ttu-id="5718d-120">Další informace v tomto tématu můžete další informace o nastavení připojení ke službě Azure AD a hello pro uživatele domény toojoin Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5718d-120">For more on this topic, you can Learn more about Azure AD Join and hello settings required for users toojoin Azure AD domain.</span></span>

- <span data-ttu-id="5718d-121">Zařízení můžete zaregistrovaná, když uživatelé přidají pracovní nebo školní účty tooWindows na osobním zařízení nebo při připojení tooa pracovním prostředkům nutnosti registrace mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-121">Devices can be registered when users add work or school accounts tooWindows on a personal device or when mobile devices connect tooa work resources requiring registration.</span></span> <span data-ttu-id="5718d-122">tooensure, musíte povolit registraci zařízení služby Azure AD ve hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5718d-122">tooensure this, you must enable Azure AD Device Registration in hello Azure Portal.</span></span> 

- <span data-ttu-id="5718d-123">Zařízení lze registrovat pomocí automatické registrace zařízení pro tradiční počítače připojené k doméně.</span><span class="sxs-lookup"><span data-stu-id="5718d-123">Devices can be registered using automatic device registration for traditional domain-joined machines.</span></span> <span data-ttu-id="5718d-124">tooensure tím, než budete pokračovat s Automatická registrace zařízení se musí první instalaci Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="5718d-124">tooensure this, you must first Setup Azure AD Connect before you continue with automatic device registration.</span></span>

<span data-ttu-id="5718d-125">Nejnovější pokyny najdete v tématu [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="5718d-125">For latest instructions, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>  
<span data-ttu-id="5718d-126">Můžete také zkontrolovat a povolit nebo zakázat registrovaná zařízení pomocí portálu správce hello v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5718d-126">You can also review and enable or disable registered devices using hello Administrator Portal in Azure Active Directory.</span></span>

## <a name="enable-hello-azure-active-directory-device-registration-service"></a><span data-ttu-id="5718d-127">Povolení služby registrace zařízení služby Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="5718d-127">Enable hello Azure Active Directory device registration service</span></span>

<span data-ttu-id="5718d-128">**tooenable hello služby Azure Active Directory device registration service:**</span><span class="sxs-lookup"><span data-stu-id="5718d-128">**tooenable hello Azure Active Directory device registration service:**</span></span>

1.  <span data-ttu-id="5718d-129">Přihlaste se toohello portálu Microsoft Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="5718d-129">Sign in toohello Microsoft Azure portal as administrator.</span></span>

2.  <span data-ttu-id="5718d-130">V levém podokně hello vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5718d-130">On hello left pane, select **Active Directory**.</span></span>

3.  <span data-ttu-id="5718d-131">Na kartě hello adresář vyberte svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="5718d-131">On hello Directory tab, select your directory.</span></span>

4.  <span data-ttu-id="5718d-132">Klikněte na **Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="5718d-132">Click **Configure**.</span></span>

5.  <span data-ttu-id="5718d-133">Posuňte se příliš**zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5718d-133">Scroll too**Devices**.</span></span>

6.  <span data-ttu-id="5718d-134">Vyberte možnost Všichni uživatelé mohou REGISTROVAT SVÁ zařízení s AZURE AD.</span><span class="sxs-lookup"><span data-stu-id="5718d-134">Select ALL for USERS MAY REGISTER THEIR DEVICES WITH AZURE AD.</span></span>

7.  <span data-ttu-id="5718d-135">Vyberte maximální počet hello zařízení chcete tooauthorize na uživatele.</span><span class="sxs-lookup"><span data-stu-id="5718d-135">Select hello maximum number of devices you want tooauthorize per user.</span></span>

<span data-ttu-id="5718d-136">Hello registrace s Microsoft Intune nebo Správa mobilních zařízení pro Office 365 vyžaduje registraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-136">hello enrollment with Microsoft Intune or Mobile Device Management for Office 365 requires device registration.</span></span> <span data-ttu-id="5718d-137">Pokud jste nakonfigurovali některou z těchto služeb **všechny** je vybraná a **NONE** je zakázána.</span><span class="sxs-lookup"><span data-stu-id="5718d-137">If you have configured either of these services, **ALL** is selected and **NONE** is disabled.</span></span> <span data-ttu-id="5718d-138">Zkontrolujte, zda jsou správně nakonfigurované a mít odpovídající licencování hello.</span><span class="sxs-lookup"><span data-stu-id="5718d-138">Please ensure that they are configured correctly and have hello appropriate licensing.</span></span>

<span data-ttu-id="5718d-139">Ve výchozím nastavení není povoleno dvoufaktorové ověřování pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="5718d-139">By default, two-factor authentication is not enabled for hello service.</span></span> <span data-ttu-id="5718d-140">Dvoufaktorové ověřování ale doporučujeme při registraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-140">However, two-factor authentication is recommended when registering a device.</span></span>

- <span data-ttu-id="5718d-141">Před pro tuto službu, které vyžadují dvoufaktorové ověřování, musíte nakonfigurovat poskytovatele dvoufaktorového ověřování ve službě Azure Active Directory a nakonfigurovat uživatelské účty pro službu Multi-Factor Authentication, najdete v části Přidání služby Multi-Factor Authentication tooAzure služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="5718d-141">Before requiring two-factor authentication for this service, you must configure a two-factor authentication provider in Azure Active Directory and configure your user accounts for Multi-Factor Authentication, see Adding Multi-Factor Authentication tooAzure Active Directory</span></span>

- <span data-ttu-id="5718d-142">Pokud používáte službu AD FS v systému Windows Server 2012 R2, musíte modul dvoufaktorového ověřování nakonfigurovat ve službě AD FS, najdete v části Použití vícefaktorového ověřování službou Active Directory Federation Services.</span><span class="sxs-lookup"><span data-stu-id="5718d-142">If you are using AD FS with Windows Server 2012 R2, you must configure a two-factor authentication module in AD FS, see Using Multi-Factor Authentication with Active Directory Federation Services.</span></span>

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a><span data-ttu-id="5718d-143">Zobrazení a správa objektů zařízení ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5718d-143">View and manage device objects in Azure Active Directory</span></span>

<span data-ttu-id="5718d-144">Z portálu správce Azure hello můžete zobrazit, zablokovat a odblokovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-144">From hello Azure Administrator portal, you can view, block, and unblock devices.</span></span> <span data-ttu-id="5718d-145">Zařízení, který je blokovaný nebude mít přístup tooapplications, které jsou nakonfigurované tooallow pouze registrovaná zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-145">A device that is blocked will no longer have access tooapplications that are configured tooallow only registered devices.</span></span>

<span data-ttu-id="5718d-146">**tooview a Správa objektů zařízení ve službě Azure Active Directory:**</span><span class="sxs-lookup"><span data-stu-id="5718d-146">**tooview and manage device objects in Azure Active Directory:**</span></span>
 
1.  <span data-ttu-id="5718d-147">Přihlaste se na toohello portálu Microsoft Azure jako správce.</span><span class="sxs-lookup"><span data-stu-id="5718d-147">Log on toohello Microsoft Azure portal as administrator.</span></span>

2.  <span data-ttu-id="5718d-148">V levém podokně hello vyberte **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5718d-148">On hello left pane, select **Active Directory**.</span></span>

3.  <span data-ttu-id="5718d-149">Vyberte svůj adresář.</span><span class="sxs-lookup"><span data-stu-id="5718d-149">Select your directory.</span></span>

4.  <span data-ttu-id="5718d-150">Vyberte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5718d-150">Select **Users**.</span></span> 

5.  <span data-ttu-id="5718d-151">Klikněte na tlačítko hello uživatele, pro které chcete toosee hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-151">Click hello user for which you want toosee hello devices.</span></span>

6.  <span data-ttu-id="5718d-152">Vyberte **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5718d-152">Select **Devices**.</span></span>

7.  <span data-ttu-id="5718d-153">Vyberte **registrované zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5718d-153">Select **Registered Devices**.</span></span>

<span data-ttu-id="5718d-154">Nyní můžete zkontrolovat, blokování nebo odblokování hello uživatele registrovaná zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-154">Now, you can review, block, or unblock hello user's registered devices.</span></span>
<span data-ttu-id="5718d-155">Zařízení s Windows 10, které jsou místně připojených k doméně a automaticky registrovaný se nezobrazí v kartě Uživatelé hello. Použijte toofind příkaz prostředí PowerShell Get-MsolDevice hello všechna podniková zařízení.</span><span class="sxs-lookup"><span data-stu-id="5718d-155">Windows 10 devices that are on-premises domain-joined and automatically registered do not appear under hello Users tab. Please use hello Get-MsolDevice PowerShell command toofind all your enterprise devices.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="5718d-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5718d-156">Next steps</span></span>

<span data-ttu-id="5718d-157">toosetup automatické registrace zařízení, najdete v části [jak tooconfigure automatické registrace Windows připojených k doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="5718d-157">toosetup automated device registration, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>


