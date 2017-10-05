---
title: "Kurz: Azure Active Directory integrace s přenosy MOVEit – integrace Azure AD | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a přenos MOVEit – integrace Azure AD."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: d35aceb9be2d0ff49f86a00cc84f5deb198d88f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a><span data-ttu-id="83f8f-103">Kurz: Azure Active Directory integrace s přenosy MOVEit – integrace Azure AD</span><span class="sxs-lookup"><span data-stu-id="83f8f-103">Tutorial: Azure Active Directory integration with MOVEit Transfer - Azure AD integration</span></span>

<span data-ttu-id="83f8f-104">V tomto kurzu zjistěte, jak integrovat MOVEit přenos – integrace Azure AD s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="83f8f-104">In this tutorial, you learn how to integrate MOVEit Transfer - Azure AD integration with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="83f8f-105">Integrace MOVEit přenos – Azure AD integraci s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="83f8f-105">Integrating MOVEit Transfer - Azure AD integration with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="83f8f-106">Můžete ovládat ve službě Azure AD, který má přístup k MOVEit přenos – integrace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83f8f-106">You can control in Azure AD who has access to MOVEit Transfer - Azure AD integration.</span></span>
- <span data-ttu-id="83f8f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k MOVEit přenos – integrace Azure AD (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83f8f-107">You can enable your users to automatically get signed-on to MOVEit Transfer - Azure AD integration (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="83f8f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="83f8f-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="83f8f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="83f8f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83f8f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83f8f-110">Prerequisites</span></span>

<span data-ttu-id="83f8f-111">Konfigurace integrace Azure AD s přenosy MOVEit – integrace služby Azure AD, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="83f8f-111">To configure Azure AD integration with MOVEit Transfer - Azure AD integration, you need the following items:</span></span>

- <span data-ttu-id="83f8f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="83f8f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="83f8f-113">Přenos MOVEit – Azure AD integrace jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="83f8f-113">A MOVEit Transfer - Azure AD integration single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="83f8f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="83f8f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="83f8f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="83f8f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="83f8f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="83f8f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="83f8f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="83f8f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="83f8f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="83f8f-118">Scenario description</span></span>
<span data-ttu-id="83f8f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="83f8f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="83f8f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="83f8f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="83f8f-121">Přidání MOVEit přenos – integrace Azure AD z Galerie</span><span class="sxs-lookup"><span data-stu-id="83f8f-121">Adding MOVEit Transfer - Azure AD integration from the gallery</span></span>
2. <span data-ttu-id="83f8f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="83f8f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a><span data-ttu-id="83f8f-123">Přidání MOVEit přenos – integrace Azure AD z Galerie</span><span class="sxs-lookup"><span data-stu-id="83f8f-123">Adding MOVEit Transfer - Azure AD integration from the gallery</span></span>
<span data-ttu-id="83f8f-124">Konfigurace integrace MOVEit přenos – integrace Azure AD do služby Azure AD, musíte přidat MOVEit přenos – Azure AD integrace z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="83f8f-124">To configure the integration of MOVEit Transfer - Azure AD integration into Azure AD, you need to add MOVEit Transfer - Azure AD integration from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="83f8f-125">**Chcete-li přidat MOVEit přenos – integrace Azure AD z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83f8f-125">**To add MOVEit Transfer - Azure AD integration from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="83f8f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="83f8f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="83f8f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="83f8f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="83f8f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83f8f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="83f8f-133">Do vyhledávacího pole zadejte **MOVEit přenos – integrace Azure AD**, vyberte **MOVEit přenos – integrace Azure AD** z panelu výsledků klikněte **přidat** tlačítko přidáte aplikace.</span><span class="sxs-lookup"><span data-stu-id="83f8f-133">In the search box, type **MOVEit Transfer - Azure AD integration**, select **MOVEit Transfer - Azure AD integration** from result panel then click **Add** button to add the application.</span></span>

    ![Přenos MOVEit – integrace Azure AD v seznamu výsledků](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="83f8f-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="83f8f-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="83f8f-136">V této části konfiguraci a testování Azure AD jednotné přihlašování s přenosy MOVEit – integrace Azure AD na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="83f8f-136">In this section, you configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="83f8f-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v MOVEit přenos – integrace Azure AD je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83f8f-137">For single sign-on to work, Azure AD needs to know what the counterpart user in MOVEit Transfer - Azure AD integration is to a user in Azure AD.</span></span> <span data-ttu-id="83f8f-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v MOVEit přenos – integrace Azure AD je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="83f8f-138">In other words, a link relationship between an Azure AD user and the related user in MOVEit Transfer - Azure AD integration needs to be established.</span></span>

<span data-ttu-id="83f8f-139">V MOVEit přenos – integrace služby Azure AD, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="83f8f-139">In MOVEit Transfer - Azure AD integration, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="83f8f-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s přenosy MOVEit – integrace služby Azure AD, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="83f8f-140">To configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="83f8f-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="83f8f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="83f8f-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83f8f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="83f8f-143">**[Vytvoření MOVEit přenos – Azure AD integrace testovacího uživatele](#create-a-moveit-transfer---azure-ad-integration-test-user)**  – Pokud chcete mít protějšek Britta Simon v MOVEit přenos – integrace Azure AD, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="83f8f-143">**[Create a MOVEit Transfer - Azure AD integration test user](#create-a-moveit-transfer---azure-ad-integration-test-user)** - to have a counterpart of Britta Simon in MOVEit Transfer - Azure AD integration that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="83f8f-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="83f8f-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="83f8f-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="83f8f-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="83f8f-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="83f8f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="83f8f-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v MOVEit přenos – integrace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83f8f-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MOVEit Transfer - Azure AD integration application.</span></span>

<span data-ttu-id="83f8f-148">**Ke konfiguraci Azure AD jednotné přihlašování s přenosy MOVEit – integrace služby Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83f8f-148">**To configure Azure AD single sign-on with MOVEit Transfer - Azure AD integration, perform the following steps:**</span></span>

1. <span data-ttu-id="83f8f-149">Na portálu Azure na **MOVEit přenos – integrace Azure AD** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-149">In the Azure portal, on the **MOVEit Transfer - Azure AD integration** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="83f8f-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="83f8f-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. <span data-ttu-id="83f8f-153">Na **MOVEit přenos – integrace Azure AD domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="83f8f-153">On the **MOVEit Transfer - Azure AD integration Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    <span data-ttu-id="83f8f-155">a.</span><span class="sxs-lookup"><span data-stu-id="83f8f-155">a.</span></span> <span data-ttu-id="83f8f-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://contoso.com`</span><span class="sxs-lookup"><span data-stu-id="83f8f-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://contoso.com`</span></span>

    <span data-ttu-id="83f8f-157">b.</span><span class="sxs-lookup"><span data-stu-id="83f8f-157">b.</span></span> <span data-ttu-id="83f8f-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://contoso.com/<tenatid>`</span><span class="sxs-lookup"><span data-stu-id="83f8f-158">In the **Identifier** textbox, type a URL using the following pattern: `https://contoso.com/<tenatid>`</span></span>

    <span data-ttu-id="83f8f-159">c.</span><span class="sxs-lookup"><span data-stu-id="83f8f-159">c.</span></span> <span data-ttu-id="83f8f-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span><span class="sxs-lookup"><span data-stu-id="83f8f-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="83f8f-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="83f8f-161">These values are not real.</span></span> <span data-ttu-id="83f8f-162">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="83f8f-162">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="83f8f-163">Najdete tyto hodnoty později v **adresa URL služby zprostředkovatele metadat** části nebo kontaktujte [MOVEit přenos – tým podpory pro Azure AD integrace klienta](https://community.ipswitch.com/s/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="83f8f-163">You can refer these values later in **Service Provider Metadata URL** section or contact [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support) to get these values.</span></span>

4. <span data-ttu-id="83f8f-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="83f8f-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. <span data-ttu-id="83f8f-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="83f8f-166">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="83f8f-168">Přihlásit se jako správce klienta MOVEit přenosu.</span><span class="sxs-lookup"><span data-stu-id="83f8f-168">Sign on to your MOVEit Transfer tenant as an administrator.</span></span>

7. <span data-ttu-id="83f8f-169">V levém navigačním podokně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-169">On the left navigation pane, click **Settings**.</span></span>

    ![Nastavení oddílu na aplikaci straně](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. <span data-ttu-id="83f8f-171">Klikněte na tlačítko **jeden Sign-on** odkaz, což je pod **zásady zabezpečení -> Auth uživatele**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-171">Click **Single Signon** link, which is under **Security Policies -> User Auth**.</span></span>

    ![Zabezpečení straně zásady na aplikace](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. <span data-ttu-id="83f8f-173">Klikněte na adresu URL metadat odkaz ke stažení dokument metadat.</span><span class="sxs-lookup"><span data-stu-id="83f8f-173">Click the Metadata URL link to download the metadata document.</span></span>

    ![Adresa URL služby zprostředkovatele metadat](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * <span data-ttu-id="83f8f-175">Ověřte **entityID** odpovídá **identifikátor** v **MOVEit přenos – integrace Azure AD domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="83f8f-175">Verify **entityID** matches **Identifier** in the **MOVEit Transfer - Azure AD integration Domain and URLs** section .</span></span>
    * <span data-ttu-id="83f8f-176">Ověřte **AssertionConsumerService** umístění URL odpovídá **adresa URL odpovědi** v **MOVEit přenos – integrace Azure AD domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="83f8f-176">Verify **AssertionConsumerService** Location URL matches **REPLY URL** in the **MOVEit Transfer - Azure AD integration Domain and URLs** section.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. <span data-ttu-id="83f8f-178">Klikněte na tlačítko **přidat zprostředkovatele Identity** tlačítko Přidat nového poskytovatele federovaných identit.</span><span class="sxs-lookup"><span data-stu-id="83f8f-178">Click **Add Identity Provider** button to add a new Federated Identity Provider.</span></span>

    ![Přidejte zprostředkovatele Identity](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. <span data-ttu-id="83f8f-180">Klikněte na tlačítko **Procházet...**  a vyberte soubor metadat, který jste si stáhli z portálu Azure, potom klikněte na **přidat zprostředkovatele Identity** nahrát stažený soubor.</span><span class="sxs-lookup"><span data-stu-id="83f8f-180">Click **Browse...** to select the metadata file which you downloaded from Azure portal, then click **Add Identity Provider** to upload the downloaded file.</span></span>

    ![Poskytovatele Identity SAML](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. <span data-ttu-id="83f8f-182">Vyberte "**Ano**" jako **povoleno** v **Upravit federované nastavení zprostředkovatele Identity...**  a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-182">Select "**Yes**" as **Enabled** in the **Edit Federated Identity Provider Settings...** page and click **Save**.</span></span>

    ![Nastavení zprostředkovatele federovaných identit](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. <span data-ttu-id="83f8f-184">V **Upravit federované Identity uživatele nastavení zprostředkovatele** proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="83f8f-184">In the **Edit Federated Identity Provider User Settings** page, perform the following actions:</span></span>
    
    ![Upravit nastavení zprostředkovatele federovaných identit](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    <span data-ttu-id="83f8f-186">a.</span><span class="sxs-lookup"><span data-stu-id="83f8f-186">a.</span></span> <span data-ttu-id="83f8f-187">Vyberte **SAML NameID** jako **přihlašovací jméno**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-187">Select **SAML NameID** as **Login name**.</span></span>
    
    <span data-ttu-id="83f8f-188">b.</span><span class="sxs-lookup"><span data-stu-id="83f8f-188">b.</span></span> <span data-ttu-id="83f8f-189">Vyberte **jiných** jako **úplný název** a v **název atributu** vložte hodnotu do textového pole: `http://schemas.microsoft.com/identity/claims/displayname`.</span><span class="sxs-lookup"><span data-stu-id="83f8f-189">Select **Other** as **Full name** and in the **Attribute name** textbox put the value: `http://schemas.microsoft.com/identity/claims/displayname`.</span></span>
    
    <span data-ttu-id="83f8f-190">c.</span><span class="sxs-lookup"><span data-stu-id="83f8f-190">c.</span></span> <span data-ttu-id="83f8f-191">Vyberte **jiných** jako **e-mailu** a v **název atributu** vložte hodnotu do textového pole: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="83f8f-191">Select **Other** as **Email** and in the **Attribute name** textbox put the value: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="83f8f-192">d.</span><span class="sxs-lookup"><span data-stu-id="83f8f-192">d.</span></span> <span data-ttu-id="83f8f-193">Vyberte **Ano** jako **automatické vytvoření účtu na Sign-on**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-193">Select **Yes** as **Auto-create account on signon**.</span></span>
    
    <span data-ttu-id="83f8f-194">e.</span><span class="sxs-lookup"><span data-stu-id="83f8f-194">e.</span></span> <span data-ttu-id="83f8f-195">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="83f8f-195">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="83f8f-196">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="83f8f-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="83f8f-197">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="83f8f-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="83f8f-198">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="83f8f-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="83f8f-199">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="83f8f-199">Create an Azure AD test user</span></span>

<span data-ttu-id="83f8f-200">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83f8f-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="83f8f-202">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83f8f-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="83f8f-203">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="83f8f-203">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="83f8f-205">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-205">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="83f8f-207">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83f8f-207">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="83f8f-209">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="83f8f-209">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    <span data-ttu-id="83f8f-211">a.</span><span class="sxs-lookup"><span data-stu-id="83f8f-211">a.</span></span> <span data-ttu-id="83f8f-212">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-212">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="83f8f-213">b.</span><span class="sxs-lookup"><span data-stu-id="83f8f-213">b.</span></span> <span data-ttu-id="83f8f-214">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83f8f-214">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="83f8f-215">c.</span><span class="sxs-lookup"><span data-stu-id="83f8f-215">c.</span></span> <span data-ttu-id="83f8f-216">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="83f8f-216">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="83f8f-217">d.</span><span class="sxs-lookup"><span data-stu-id="83f8f-217">d.</span></span> <span data-ttu-id="83f8f-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-218">Click **Create**.</span></span>
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a><span data-ttu-id="83f8f-219">Vytvoření MOVEit přenos – Azure AD integrace testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="83f8f-219">Create a MOVEit Transfer - Azure AD integration test user</span></span>

<span data-ttu-id="83f8f-220">Cílem této části je vytvoření uživatele volal Britta Simon v MOVEit přenos – integrace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83f8f-220">The objective of this section is to create a user called Britta Simon in MOVEit Transfer - Azure AD integration.</span></span> <span data-ttu-id="83f8f-221">Přenos MOVEit – integrace Azure AD podporuje zřizování za běhu, které jste povolili.</span><span class="sxs-lookup"><span data-stu-id="83f8f-221">MOVEit Transfer - Azure AD integration supports just-in-time provisioning, which you have enabled.</span></span> <span data-ttu-id="83f8f-222">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="83f8f-222">There is no action item for you in this section.</span></span> <span data-ttu-id="83f8f-223">Nový uživatel se vytvoří během pokusu o přístup k MOVEit přenos – integrace Azure AD, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="83f8f-223">A new user is created during an attempt to access MOVEit Transfer - Azure AD integration if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="83f8f-224">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [MOVEit přenos – tým podpory pro Azure AD integrace klienta](https://community.ipswitch.com/s/support).</span><span class="sxs-lookup"><span data-stu-id="83f8f-224">If you need to create a user manually, you need to contact the [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="83f8f-225">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="83f8f-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="83f8f-226">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu MOVEit přenos – integrace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83f8f-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to MOVEit Transfer - Azure AD integration.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="83f8f-228">**Britta Simon přiřadit přenosy MOVEit – integrace služby Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83f8f-228">**To assign Britta Simon to MOVEit Transfer - Azure AD integration, perform the following steps:**</span></span>

1. <span data-ttu-id="83f8f-229">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="83f8f-231">V seznamu aplikací vyberte **MOVEit přenos – integrace Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-231">In the applications list, select **MOVEit Transfer - Azure AD integration**.</span></span>

    ![Přenos MOVEit – integrace Azure AD odkaz v seznamu aplikací](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. <span data-ttu-id="83f8f-233">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="83f8f-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="83f8f-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="83f8f-235">Click **Add** button.</span></span> <span data-ttu-id="83f8f-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83f8f-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="83f8f-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="83f8f-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="83f8f-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83f8f-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="83f8f-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83f8f-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="83f8f-241">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="83f8f-241">Test single sign-on</span></span>

<span data-ttu-id="83f8f-242">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="83f8f-242">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="83f8f-243">Po kliknutí na tlačítko MOVEit přenos – integrace Azure AD dlaždici na přístupovém panelu jste měli získat automaticky přihlášení k MOVEit přenos – integrace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83f8f-243">When you click the MOVEit Transfer - Azure AD integration tile in the Access Panel, you should get automatically signed-on to your MOVEit Transfer - Azure AD integration application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="83f8f-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="83f8f-244">Additional resources</span></span>

* [<span data-ttu-id="83f8f-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="83f8f-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="83f8f-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="83f8f-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

