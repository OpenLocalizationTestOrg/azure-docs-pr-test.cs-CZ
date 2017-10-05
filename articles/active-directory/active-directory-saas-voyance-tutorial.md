---
title: 'Kurz: Azure Active Directory integrace s Voyance | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Voyance."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 539dc1f9-64c9-4dce-b259-2b0b49dcf857
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: e860b810904fb7972d75d55d913d5622ff9a406a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-voyance"></a><span data-ttu-id="0752b-103">Kurz: Azure Active Directory integrace s Voyance</span><span class="sxs-lookup"><span data-stu-id="0752b-103">Tutorial: Azure Active Directory integration with Voyance</span></span>

<span data-ttu-id="0752b-104">V tomto kurzu zjistěte, jak integrovat Voyance s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0752b-104">In this tutorial, you learn how to integrate Voyance with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0752b-105">Integrace Voyance s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0752b-105">Integrating Voyance with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0752b-106">Můžete řídit ve službě Azure AD, který má přístup k Voyance</span><span class="sxs-lookup"><span data-stu-id="0752b-106">You can control in Azure AD who has access to Voyance</span></span>
- <span data-ttu-id="0752b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Voyance (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0752b-107">You can enable your users to automatically get signed-on to Voyance (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0752b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0752b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0752b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0752b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0752b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0752b-110">Prerequisites</span></span>

<span data-ttu-id="0752b-111">Konfigurace integrace Azure AD s Voyance, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="0752b-111">To configure Azure AD integration with Voyance, you need the following items:</span></span>

- <span data-ttu-id="0752b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0752b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0752b-113">Voyance jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0752b-113">A Voyance single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0752b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0752b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0752b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0752b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0752b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0752b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0752b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0752b-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0752b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0752b-118">Scenario description</span></span>
<span data-ttu-id="0752b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0752b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0752b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0752b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0752b-121">Přidání Voyance z Galerie</span><span class="sxs-lookup"><span data-stu-id="0752b-121">Adding Voyance from the gallery</span></span>
2. <span data-ttu-id="0752b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0752b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-voyance-from-the-gallery"></a><span data-ttu-id="0752b-123">Přidání Voyance z Galerie</span><span class="sxs-lookup"><span data-stu-id="0752b-123">Adding Voyance from the gallery</span></span>
<span data-ttu-id="0752b-124">Při konfiguraci integrace Voyance do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Voyance z galerie.</span><span class="sxs-lookup"><span data-stu-id="0752b-124">To configure the integration of Voyance into Azure AD, you need to add Voyance from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0752b-125">**Pokud chcete přidat Voyance z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0752b-125">**To add Voyance from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0752b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0752b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="0752b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0752b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0752b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0752b-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="0752b-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0752b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="0752b-133">Do vyhledávacího pole zadejte **Voyance**, vyberte **Voyance** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0752b-133">In the search box, type **Voyance**, select  **Voyance**  from result panel then click **Add** button to add the application.</span></span>

    ![Voyance v seznamu výsledků](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="0752b-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0752b-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="0752b-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Voyance podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="0752b-136">In this section, you configure and test Azure AD single sign-on with Voyance based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0752b-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Voyance je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0752b-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Voyance is to a user in Azure AD.</span></span> <span data-ttu-id="0752b-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Voyance musí navázat.</span><span class="sxs-lookup"><span data-stu-id="0752b-138">In other words, a link relationship between an Azure AD user and the related user in Voyance needs to be established.</span></span>

<span data-ttu-id="0752b-139">V Voyance, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="0752b-139">In Voyance, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0752b-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Voyance, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="0752b-140">To configure and test Azure AD single sign-on with Voyance, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0752b-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="0752b-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0752b-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0752b-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0752b-143">**[Vytvoření zkušebního uživatele Voyance](#create-a-voyance-test-user)**  – Pokud chcete mít protějšek Britta Simon v Voyance propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0752b-143">**[Create a Voyance test user](#create-a-voyance-test-user)** - to have a counterpart of Britta Simon in Voyance that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0752b-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0752b-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0752b-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0752b-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="0752b-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0752b-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="0752b-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Voyance.</span><span class="sxs-lookup"><span data-stu-id="0752b-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Voyance application.</span></span>

<span data-ttu-id="0752b-148">**Ke konfiguraci Azure AD jednotné přihlašování s Voyance, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0752b-148">**To configure Azure AD single sign-on with Voyance, perform the following steps:**</span></span>

1. <span data-ttu-id="0752b-149">Na portálu Azure na **Voyance** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0752b-149">In the Azure portal, on the **Voyance** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="0752b-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0752b-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_samlbase.png)

3. <span data-ttu-id="0752b-153">Na **Voyance domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="0752b-153">On the **Voyance Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Voyance domény a adresy URL jednotné přihlašování informace o rozšíření IDP](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url1.png)

    <span data-ttu-id="0752b-155">a.</span><span class="sxs-lookup"><span data-stu-id="0752b-155">a.</span></span> <span data-ttu-id="0752b-156">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.nyansa.com`</span><span class="sxs-lookup"><span data-stu-id="0752b-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.nyansa.com`</span></span>

    <span data-ttu-id="0752b-157">b.</span><span class="sxs-lookup"><span data-stu-id="0752b-157">b.</span></span> <span data-ttu-id="0752b-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.nyansa.com/saml/create/`</span><span class="sxs-lookup"><span data-stu-id="0752b-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.nyansa.com/saml/create/`</span></span>

4. <span data-ttu-id="0752b-159">Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provést následující krok, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="0752b-159">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Voyance domény a adresy URL jednotné přihlašování informace pro poskytovatele služeb](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url2.png)

    <span data-ttu-id="0752b-161">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.nyansa.com/`</span><span class="sxs-lookup"><span data-stu-id="0752b-161">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.nyansa.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="0752b-162">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="0752b-162">These values are not real.</span></span> <span data-ttu-id="0752b-163">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="0752b-163">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="0752b-164">Obraťte se na [tým podpory Voyance klienta](mailto:support@nyansa.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="0752b-164">Contact [Voyance Client support team](mailto:support@nyansa.com) to get these values.</span></span> 

5. <span data-ttu-id="0752b-165">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="0752b-165">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_certificate.png) 

6. <span data-ttu-id="0752b-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0752b-167">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-voyance-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="0752b-169">Na **Voyance konfigurace** klikněte na tlačítko **konfigurace Voyance** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0752b-169">On the **Voyance Configuration** section, click **Configure Voyance** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0752b-170">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0752b-170">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Voyance](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_configure.png) 

8. <span data-ttu-id="0752b-172">V okně prohlížeče jiných webových přihlašování ke klientovi Voyance jako správce.</span><span class="sxs-lookup"><span data-stu-id="0752b-172">In a different web browser window, sign-on to your Voyance tenant as an administrator.</span></span>

9. <span data-ttu-id="0752b-173">Přejděte na pravém horním rohu na navigačním panelu a klikněte na rozevírací seznam, která uvádí, že "**pokusná univerzity**".</span><span class="sxs-lookup"><span data-stu-id="0752b-173">Go to the top right corner of the navigation bar and click on the drop-down that says "**Acme University**".</span></span>
    
    ![Konfigurovat jednotné přihlašování v aplikaci straně pokusná univerzity](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_001.png) 

10. <span data-ttu-id="0752b-175">Klikněte na tlačítko "**nastavení správce**".</span><span class="sxs-lookup"><span data-stu-id="0752b-175">Click "**Admin Settings**".</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace Správce nastavení](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_002.png)

11. <span data-ttu-id="0752b-177">Klikněte na tlačítko "**přístup uživatelů**" kartě.</span><span class="sxs-lookup"><span data-stu-id="0752b-177">Click "**User Access**" tab.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace uživatele přístup](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_003.png)

12. <span data-ttu-id="0752b-179">Klikněte na tlačítko "**jednotné přihlašování je zakázána**" tlačítko Konfigurovat Azure AD jako deklarací identity pomocí SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="0752b-179">Click the "**SSO is disabled**" button to configure Azure AD as an IdP using SAML 2.0.</span></span>

    ![Konfigurovat jednotné přihlašování v aplikaci straně SSO vypnutá tlačítko](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_004.png)

13. <span data-ttu-id="0752b-181">Přejděte na **SAML v2** části a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0752b-181">Go to **SAML v2** section and perform below steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace SAML v2](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_005.png)
    
    <span data-ttu-id="0752b-183">a.</span><span class="sxs-lookup"><span data-stu-id="0752b-183">a.</span></span> <span data-ttu-id="0752b-184">Vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="0752b-184">Select **Enabled**.</span></span>
    
    <span data-ttu-id="0752b-185">b.</span><span class="sxs-lookup"><span data-stu-id="0752b-185">b.</span></span> <span data-ttu-id="0752b-186">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure do **IdP přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0752b-186">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal Into the **IdP Login URL** textbox.</span></span>

    <span data-ttu-id="0752b-187">c.</span><span class="sxs-lookup"><span data-stu-id="0752b-187">c.</span></span> <span data-ttu-id="0752b-188">V poznámkovém bloku otevřete váš stažený certifikát kódovaný v Base64, zkopírujte obsah ho do schránky a vložte jej do **IdP Cert** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0752b-188">Open your downloaded Base64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **IdP Cert** textbox.</span></span>
    
    <span data-ttu-id="0752b-189">d.</span><span class="sxs-lookup"><span data-stu-id="0752b-189">d.</span></span> <span data-ttu-id="0752b-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0752b-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="0752b-191">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="0752b-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0752b-192">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="0752b-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0752b-193">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0752b-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="0752b-194">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0752b-194">Create an Azure AD test user</span></span>

<span data-ttu-id="0752b-195">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0752b-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="0752b-197">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0752b-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0752b-198">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0752b-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-voyance-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0752b-200">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0752b-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-voyance-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0752b-202">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0752b-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-voyance-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0752b-204">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0752b-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-voyance-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0752b-206">a.</span><span class="sxs-lookup"><span data-stu-id="0752b-206">a.</span></span> <span data-ttu-id="0752b-207">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0752b-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0752b-208">b.</span><span class="sxs-lookup"><span data-stu-id="0752b-208">b.</span></span> <span data-ttu-id="0752b-209">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0752b-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0752b-210">c.</span><span class="sxs-lookup"><span data-stu-id="0752b-210">c.</span></span> <span data-ttu-id="0752b-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0752b-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0752b-212">d.</span><span class="sxs-lookup"><span data-stu-id="0752b-212">d.</span></span> <span data-ttu-id="0752b-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0752b-213">Click **Create**.</span></span>
 
### <a name="create-a-voyance-test-user"></a><span data-ttu-id="0752b-214">Vytvoření zkušebního uživatele Voyance</span><span class="sxs-lookup"><span data-stu-id="0752b-214">Create a Voyance test user</span></span>

<span data-ttu-id="0752b-215">Cílem této části je vytvoření uživatele v Voyance nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0752b-215">The objective of this section is to create a user called Britta Simon in Voyance.</span></span> <span data-ttu-id="0752b-216">Voyance podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="0752b-216">Voyance supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="0752b-217">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="0752b-217">There is no action item for you in this section.</span></span> <span data-ttu-id="0752b-218">Nový uživatel se vytvoří během pokusu o přístup k Voyance, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="0752b-218">A new user is created during an attempt to access Voyance if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="0752b-219">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory Voyance](maiLto:support@nyansa.com).</span><span class="sxs-lookup"><span data-stu-id="0752b-219">If you need to create a user manually, you need to contact [Voyance support team](maiLto:support@nyansa.com).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="0752b-220">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0752b-220">Assign the Azure AD test user</span></span>

<span data-ttu-id="0752b-221">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Voyance.</span><span class="sxs-lookup"><span data-stu-id="0752b-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Voyance.</span></span>

![Přiřadit role uživatele][200]

<span data-ttu-id="0752b-223">**Pokud chcete přiřadit Britta Simon Voyance, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0752b-223">**To assign Britta Simon to Voyance, perform the following steps:**</span></span>

1. <span data-ttu-id="0752b-224">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0752b-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0752b-226">V seznamu aplikací vyberte **Voyance**.</span><span class="sxs-lookup"><span data-stu-id="0752b-226">In the applications list, select **Voyance**.</span></span>

    ![V seznamu aplikací na Voyance odkaz](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_app.png) 

3. <span data-ttu-id="0752b-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0752b-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="0752b-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0752b-230">Click **Add** button.</span></span> <span data-ttu-id="0752b-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0752b-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="0752b-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0752b-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0752b-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0752b-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0752b-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0752b-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="0752b-236">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0752b-236">Test single sign-on</span></span>

<span data-ttu-id="0752b-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0752b-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0752b-238">Když kliknete na dlaždici Voyance na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Voyance.</span><span class="sxs-lookup"><span data-stu-id="0752b-238">When you click the Voyance tile in the Access Panel, you should get automatically signed-on to your Voyance application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0752b-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0752b-239">Additional resources</span></span>

* [<span data-ttu-id="0752b-240">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0752b-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0752b-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0752b-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_203.png

