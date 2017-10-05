---
title: "Kurz: Azure Active Directory integrace s vyrovná se se zatížením pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a vyrovná se se zatížením pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 3c68c3ac7d6be593476d419f8c015931b206eead
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a><span data-ttu-id="87bf2-103">Kurz: Azure Active Directory integrace s vyrovná se se zatížením pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="87bf2-103">Tutorial: Azure Active Directory integration with Absorb LMS</span></span>

<span data-ttu-id="87bf2-104">V tomto kurzu zjistěte, jak integrovat vyrovná se se zatížením pro správu vzdělávacího procesu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="87bf2-104">In this tutorial, you learn how to integrate Absorb LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="87bf2-105">Integrace vyrovná se se zatížením pro správu vzdělávacího procesu s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="87bf2-105">Integrating Absorb LMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="87bf2-106">Můžete řídit ve službě Azure AD, který má přístup k vyrovná se se zatížením pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="87bf2-106">You can control in Azure AD who has access to Absorb LMS</span></span>
- <span data-ttu-id="87bf2-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k vyrovná se se zatížením vzdělávacího procesu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="87bf2-107">You can enable your users to automatically get signed-on to Absorb LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="87bf2-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="87bf2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="87bf2-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="87bf2-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="87bf2-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="87bf2-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87bf2-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87bf2-111">Prerequisites</span></span>

<span data-ttu-id="87bf2-112">Konfigurace integrace Azure AD s vyrovná se se zatížením pro správu vzdělávacího procesu, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="87bf2-112">To configure Azure AD integration with Absorb LMS, you need the following items:</span></span>

- <span data-ttu-id="87bf2-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="87bf2-113">An Azure AD subscription</span></span>
- <span data-ttu-id="87bf2-114">Vyrovná se se zatížením pro správu vzdělávacího procesu jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="87bf2-114">An Absorb LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="87bf2-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="87bf2-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="87bf2-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="87bf2-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="87bf2-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="87bf2-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="87bf2-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="87bf2-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="87bf2-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="87bf2-119">Scenario description</span></span>
<span data-ttu-id="87bf2-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="87bf2-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="87bf2-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="87bf2-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="87bf2-122">Přidání vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie</span><span class="sxs-lookup"><span data-stu-id="87bf2-122">Adding Absorb LMS from the gallery</span></span>
2. <span data-ttu-id="87bf2-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="87bf2-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-absorb-lms-from-the-gallery"></a><span data-ttu-id="87bf2-124">Přidání vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie</span><span class="sxs-lookup"><span data-stu-id="87bf2-124">Adding Absorb LMS from the gallery</span></span>
<span data-ttu-id="87bf2-125">Pokud chcete nakonfigurovat integraci vyrovná se se zatížením pro správu vzdělávacího procesu v do Azure AD, potřebujete přidat vyrovná se se zatížením pro správu vzdělávacího procesu z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="87bf2-125">To configure the integration of Absorb LMS in to Azure AD, you need to add Absorb LMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="87bf2-126">**Pokud chcete přidat vyrovná se se zatížením pro správu vzdělávacího procesu z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87bf2-126">**To add Absorb LMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="87bf2-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="87bf2-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="87bf2-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-130">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="87bf2-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87bf2-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="87bf2-134">Do vyhledávacího pole zadejte **vyrovná se se zatížením pro správu vzdělávacího procesu**, vyberte **vyrovná se se zatížením pro správu vzdělávacího procesu** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="87bf2-134">In the search box, type **Absorb LMS**, select **Absorb LMS** from result panel then click **Add** button to add the application.</span></span>

    ![Vyrovná se se zatížením pro správu vzdělávacího procesu v seznamu výsledků](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="87bf2-136">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="87bf2-136">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="87bf2-137">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s vyrovná se se zatížením vzdělávacího procesu na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="87bf2-137">In this section, you configure and test Azure AD single sign-on with Absorb LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="87bf2-138">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v vyrovná se se zatížením pro správu vzdělávacího procesu je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="87bf2-138">For single sign-on to work, Azure AD needs to know what the counterpart user in Absorb LMS is to a user in Azure AD.</span></span> <span data-ttu-id="87bf2-139">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v vyrovná se se zatížením pro správu vzdělávacího procesu musí navázat.</span><span class="sxs-lookup"><span data-stu-id="87bf2-139">In other words, a link relationship between an Azure AD user and the related user in Absorb LMS needs to be established.</span></span>

<span data-ttu-id="87bf2-140">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-140">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Absorb LMS.</span></span>

<span data-ttu-id="87bf2-141">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s vyrovná se se zatížením pro správu vzdělávacího procesu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="87bf2-141">To configure and test Azure AD single sign-on with Absorb LMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="87bf2-142">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="87bf2-142">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="87bf2-143">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87bf2-143">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="87bf2-144">**[Vytvořit testovací uživatele s vyrovná se se zatížením pro správu vzdělávacího procesu](#create-an-absorb-lms-test-user)**  – Pokud chcete mít protějšek Britta Simon v vzdělávacího procesu vyrovná se se zatížením, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="87bf2-144">**[Create an Absorb LMS test user](#create-an-absorb-lms-test-user)** - to have a counterpart of Britta Simon in Absorb LMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="87bf2-145">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="87bf2-145">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="87bf2-146">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87bf2-146">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="87bf2-147">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="87bf2-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="87bf2-148">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Absorb LMS application.</span></span>

<span data-ttu-id="87bf2-149">**Ke konfiguraci Azure AD jednotné přihlašování s vyrovná se se zatížením pro správu vzdělávacího procesu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87bf2-149">**To configure Azure AD single sign-on with Absorb LMS, perform the following steps:**</span></span>

1. <span data-ttu-id="87bf2-150">Na portálu Azure na **vyrovná se se zatížením pro správu vzdělávacího procesu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-150">In the Azure portal, on the **Absorb LMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="87bf2-152">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="87bf2-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. <span data-ttu-id="87bf2-154">Na **vyrovná se se zatížením pro správu vzdělávacího procesu domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87bf2-154">On the **Absorb LMS Domain and URLs** section, perform the following steps:</span></span>

    ![Pro správu vzdělávacího procesu adresy URL jeden přihlašování informace o doméně a vyrovná se se zatížením](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    <span data-ttu-id="87bf2-156">a.</span><span class="sxs-lookup"><span data-stu-id="87bf2-156">a.</span></span> <span data-ttu-id="87bf2-157">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="87bf2-157">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>

    <span data-ttu-id="87bf2-158">b.</span><span class="sxs-lookup"><span data-stu-id="87bf2-158">b.</span></span> <span data-ttu-id="87bf2-159">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="87bf2-159">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="87bf2-160">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="87bf2-160">These values are not the real.</span></span> <span data-ttu-id="87bf2-161">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="87bf2-161">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="87bf2-162">Obraťte se na [tým podpory vyrovná se se zatížením klienta pro správu vzdělávacího procesu](https://www.absorblms.com/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="87bf2-162">Contact [Absorb LMS Client support team](https://www.absorblms.com/support) to get these values.</span></span> 

4. <span data-ttu-id="87bf2-163">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="87bf2-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. <span data-ttu-id="87bf2-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="87bf2-165">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="87bf2-167">Na **vyrovná se se zatížením konfigurace pro správu vzdělávacího procesu** klikněte na tlačítko **konfigurace vyrovná se se zatížením vzdělávacího procesu** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="87bf2-167">On the **Absorb LMS Configuration** section, click **Configure Absorb LMS** to open **Configure sign-on** window.</span></span> <span data-ttu-id="87bf2-168">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="87bf2-168">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Vyrovná se se zatížením konfigurace pro správu vzdělávacího procesu](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. <span data-ttu-id="87bf2-170">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti vyrovná se se zatížením pro správu vzdělávacího procesu jako správce.</span><span class="sxs-lookup"><span data-stu-id="87bf2-170">In a different web browser window, log in to your Absorb LMS company site as an administrator.</span></span>

9. <span data-ttu-id="87bf2-171">Klikněte **ikonu účtu** v rozhraní správce.</span><span class="sxs-lookup"><span data-stu-id="87bf2-171">Click the **Account Icon** on the admin interface.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/1.png)

10. <span data-ttu-id="87bf2-173">Klikněte na tlačítko **nastavení portálu**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-173">Click **Portal Settings**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. <span data-ttu-id="87bf2-175">Klikněte **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="87bf2-175">Click the **Users** tab.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/3.png)

12. <span data-ttu-id="87bf2-177">Proveďte následující kroky pro přístup k konfigurační pole jednotné přihlašování:</span><span class="sxs-lookup"><span data-stu-id="87bf2-177">Perform the following steps to access the Single Sign-On configuration fields:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/4.png)

    <span data-ttu-id="87bf2-179">a.</span><span class="sxs-lookup"><span data-stu-id="87bf2-179">a.</span></span> <span data-ttu-id="87bf2-180">Vyberte odpovídající **režimu**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-180">Select the appropriate **Mode**.</span></span>

    <span data-ttu-id="87bf2-181">b.</span><span class="sxs-lookup"><span data-stu-id="87bf2-181">b.</span></span> <span data-ttu-id="87bf2-182">Otevřete odebrat certifikát, který jste stáhli z portálu Azure v poznámkovém bloku **---BEGIN CERTIFICATE,** a **---END CERTIFICATE---** značku a potom vložte zbývající obsah **klíč** textové pole.</span><span class="sxs-lookup"><span data-stu-id="87bf2-182">Open the Certificate that you have downloaded from the Azure portal in notepad, remove the **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste the remaining content in the **Key** textbox.</span></span>
    
    <span data-ttu-id="87bf2-183">c.</span><span class="sxs-lookup"><span data-stu-id="87bf2-183">c.</span></span> <span data-ttu-id="87bf2-184">V **Vlastnost Id**, vyberte odpovídající atribut, který jste nakonfigurovali jako identifikátor uživatele ve službě Azure AD (např. Pokud userprinciplename je vybrána ve službě Azure AD, pak uživatelské jméno by zde vybraný.)</span><span class="sxs-lookup"><span data-stu-id="87bf2-184">In the **Id Property**, select the appropriate attribute which you have configured as the user identifier in the Azure AD (For example, If the userprinciplename is selected in Azure AD, then Username would be selected here.)</span></span>

    <span data-ttu-id="87bf2-185">d.</span><span class="sxs-lookup"><span data-stu-id="87bf2-185">d.</span></span> <span data-ttu-id="87bf2-186">V **přihlašovací adresa URL**, vložte **"SAML jeden přihlašování adresa URL služby"** hodnotu zkopírovanou z **konfigurovat přihlášení** okno portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87bf2-186">In the **Login URL**, paste the **“SAML Single Sign-On Service URL”** value you have copied from the **Configure sign-on** window of the Azure portal.</span></span>

    <span data-ttu-id="87bf2-187">e.</span><span class="sxs-lookup"><span data-stu-id="87bf2-187">e.</span></span> <span data-ttu-id="87bf2-188">V **adresy URL odhlašovací**, vložte **"Adresa URL Sign-Out"** hodnotu zkopírovanou z **konfigurovat přihlášení** okno portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87bf2-188">In the **Logout URL**, paste the **“Sign-Out URL”** value you have copied from the **Configure sign-on** window of the Azure portal.</span></span>

13. <span data-ttu-id="87bf2-189">Povolit **"Povolit jenom přihlášení SSO"**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-189">Enable **‘Only Allow SSO Login’**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-absorblms-tutorial/5.png)

14. <span data-ttu-id="87bf2-191">Klikněte na tlačítko **"Uložit."**</span><span class="sxs-lookup"><span data-stu-id="87bf2-191">Click **"Save."**</span></span>

> [!TIP]
> <span data-ttu-id="87bf2-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="87bf2-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="87bf2-193">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="87bf2-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="87bf2-194">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="87bf2-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="87bf2-195">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="87bf2-195">Create an Azure AD test user</span></span>

<span data-ttu-id="87bf2-196">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87bf2-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="87bf2-198">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87bf2-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="87bf2-199">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="87bf2-201">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="87bf2-203">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87bf2-203">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="87bf2-205">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87bf2-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="87bf2-207">a.</span><span class="sxs-lookup"><span data-stu-id="87bf2-207">a.</span></span> <span data-ttu-id="87bf2-208">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="87bf2-209">b.</span><span class="sxs-lookup"><span data-stu-id="87bf2-209">b.</span></span> <span data-ttu-id="87bf2-210">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="87bf2-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="87bf2-211">c.</span><span class="sxs-lookup"><span data-stu-id="87bf2-211">c.</span></span> <span data-ttu-id="87bf2-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="87bf2-213">d.</span><span class="sxs-lookup"><span data-stu-id="87bf2-213">d.</span></span> <span data-ttu-id="87bf2-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-214">Click **Create**.</span></span>

### <a name="create-an-absorb-lms-test-user"></a><span data-ttu-id="87bf2-215">Vytvořit testovací uživatele s vyrovná se se zatížením pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="87bf2-215">Create an Absorb LMS test user</span></span>

<span data-ttu-id="87bf2-216">Pokud chcete povolit uživatelům Azure AD přihlášení do vyrovná se se zatížením pro správu vzdělávacího procesu, se musí být zřízená v k vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-216">To enable Azure AD users to log in to Absorb LMS, they must be provisioned in to Absorb LMS.</span></span>  
<span data-ttu-id="87bf2-217">Pro vyrovná se se zatížením vzdělávacího procesu zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="87bf2-217">For Absorb LMS, provisioning is a manual task.</span></span>

<span data-ttu-id="87bf2-218">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87bf2-218">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="87bf2-219">Přihlaste se k serveru vaší společnosti vyrovná se se zatížením pro správu vzdělávacího procesu jako správce.</span><span class="sxs-lookup"><span data-stu-id="87bf2-219">Log in to your Absorb LMS company site as an administrator.</span></span>

2. <span data-ttu-id="87bf2-220">Klikněte na tlačítko **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="87bf2-220">Click **Users** tab.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. <span data-ttu-id="87bf2-222">Klikněte na tlačítko **uživatelé** pod **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="87bf2-222">Click **Users** under the **Users** tab.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  <span data-ttu-id="87bf2-224">Vyberte **uživatele** z **přidat nové** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-224">Select **User** from **Add New** drop-down.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. <span data-ttu-id="87bf2-226">Na **přidat uživatele** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87bf2-226">On the **Add User** page, perform the following steps:</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-absorblms-tutorial/user.png)

    <span data-ttu-id="87bf2-228">a.</span><span class="sxs-lookup"><span data-stu-id="87bf2-228">a.</span></span> <span data-ttu-id="87bf2-229">V **křestní jméno** textovému poli, název typu první jako Britta.</span><span class="sxs-lookup"><span data-stu-id="87bf2-229">In the **First Name** textbox, type the first name like Britta.</span></span>

    <span data-ttu-id="87bf2-230">b.</span><span class="sxs-lookup"><span data-stu-id="87bf2-230">b.</span></span> <span data-ttu-id="87bf2-231">V **příjmení** textovému poli, zadejte příjmení jako Simon.</span><span class="sxs-lookup"><span data-stu-id="87bf2-231">In the **Last Name** textbox, type the last name like Simon.</span></span>
    
    <span data-ttu-id="87bf2-232">c.</span><span class="sxs-lookup"><span data-stu-id="87bf2-232">c.</span></span> <span data-ttu-id="87bf2-233">V **uživatelské jméno** textovému poli, zadejte uživatelské jméno jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87bf2-233">In the **Username** textbox, type the user name like Britta Simon.</span></span>

    <span data-ttu-id="87bf2-234">d.</span><span class="sxs-lookup"><span data-stu-id="87bf2-234">d.</span></span> <span data-ttu-id="87bf2-235">V **heslo** textovému poli, zadejte heslo Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87bf2-235">In the **Password** textbox, type the password of Britta Simon.</span></span>

    <span data-ttu-id="87bf2-236">e.</span><span class="sxs-lookup"><span data-stu-id="87bf2-236">e.</span></span> <span data-ttu-id="87bf2-237">V **Potvrdit heslo** textovému poli, zadejte stejné heslo.</span><span class="sxs-lookup"><span data-stu-id="87bf2-237">In the **Confirm Password** textbox, type the same password.</span></span>
    
    <span data-ttu-id="87bf2-238">f.</span><span class="sxs-lookup"><span data-stu-id="87bf2-238">f.</span></span> <span data-ttu-id="87bf2-239">Nastavit jej jako **ACTIVE**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-239">Make it as **ACTIVE**.</span></span>   

6. <span data-ttu-id="87bf2-240">Klikněte na tlačítko **"Uložit."**</span><span class="sxs-lookup"><span data-stu-id="87bf2-240">Click **"Save."**</span></span>
 
### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="87bf2-241">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="87bf2-241">Assign the Azure AD test user</span></span>

<span data-ttu-id="87bf2-242">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Absorb LMS.</span></span>

![Přiřadit role uživatele][200]

<span data-ttu-id="87bf2-244">**Pokud chcete přiřadit Britta Simon vyrovná se se zatížením pro správu vzdělávacího procesu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87bf2-244">**To assign Britta Simon to Absorb LMS, perform the following steps:**</span></span>

1. <span data-ttu-id="87bf2-245">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="87bf2-247">V seznamu aplikací vyberte **vyrovná se se zatížením pro správu vzdělávacího procesu**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-247">In the applications list, select **Absorb LMS**.</span></span>

    ![Odkaz vyrovná se se zatížením pro správu vzdělávacího procesu v seznamu aplikací](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. <span data-ttu-id="87bf2-249">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="87bf2-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="87bf2-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="87bf2-251">Click **Add** button.</span></span> <span data-ttu-id="87bf2-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87bf2-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="87bf2-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="87bf2-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="87bf2-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87bf2-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="87bf2-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87bf2-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="87bf2-257">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="87bf2-257">Test single sign-on</span></span>

<span data-ttu-id="87bf2-258">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-258">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="87bf2-259">Klikněte na dlaždici vyrovná se se zatížením pro správu vzdělávacího procesu na přístupovém panelu, můžete se získat automaticky přihlášení k aplikaci vyrovná se se zatížením pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="87bf2-259">Click the Absorb LMS tile in the Access Panel, you will get automatically signed-on to your Absorb LMS application.</span></span> <span data-ttu-id="87bf2-260">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="87bf2-260">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87bf2-261">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="87bf2-261">Additional resources</span></span>

* [<span data-ttu-id="87bf2-262">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="87bf2-262">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="87bf2-263">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="87bf2-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

