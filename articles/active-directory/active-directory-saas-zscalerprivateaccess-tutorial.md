---
title: "Kurz: Azure Active Directory integrace s Zscaler privátní přístup (ZPA) | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Zscaler privátní přístup (ZPA)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 5c598bfa5b6725d21a89df54dbcb3314cc631d80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a><span data-ttu-id="af38d-103">Kurz: Azure Active Directory integrace s Zscaler privátní přístup (ZPA)</span><span class="sxs-lookup"><span data-stu-id="af38d-103">Tutorial: Azure Active Directory integration with Zscaler Private Access (ZPA)</span></span>

<span data-ttu-id="af38d-104">V tomto kurzu zjistěte, jak integrovat Zscaler privátní přístup (ZPA) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="af38d-104">In this tutorial, you learn how to integrate Zscaler Private Access (ZPA) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="af38d-105">Integrace Zscaler privátní přístup (ZPA) s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="af38d-105">Integrating Zscaler Private Access (ZPA) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="af38d-106">Můžete řídit ve službě Azure AD, který má přístup k Zscaler privátní přístup (ZPA)</span><span class="sxs-lookup"><span data-stu-id="af38d-106">You can control in Azure AD who has access to Zscaler Private Access (ZPA)</span></span>
- <span data-ttu-id="af38d-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k Zscaler privátní přístup (ZPA) (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="af38d-107">You can enable your users to automatically get signed-on to Zscaler Private Access (ZPA) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="af38d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="af38d-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="af38d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="af38d-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af38d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="af38d-110">Prerequisites</span></span>

<span data-ttu-id="af38d-111">Ke konfiguraci integrace služby Azure AD s Zscaler privátní přístup (ZPA), potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="af38d-111">To configure Azure AD integration with Zscaler Private Access (ZPA), you need the following items:</span></span>

- <span data-ttu-id="af38d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="af38d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="af38d-113">Zscaler privátní přístup (ZPA) jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="af38d-113">A Zscaler Private Access (ZPA) single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="af38d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="af38d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="af38d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="af38d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="af38d-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="af38d-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="af38d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="af38d-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="af38d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="af38d-118">Scenario description</span></span>
<span data-ttu-id="af38d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="af38d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="af38d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="af38d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="af38d-121">Přidání Zscaler privátní přístup (ZPA) z Galerie</span><span class="sxs-lookup"><span data-stu-id="af38d-121">Adding Zscaler Private Access (ZPA) from the gallery</span></span>
2. <span data-ttu-id="af38d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="af38d-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a><span data-ttu-id="af38d-123">Přidání Zscaler privátní přístup (ZPA) z Galerie</span><span class="sxs-lookup"><span data-stu-id="af38d-123">Adding Zscaler Private Access (ZPA) from the gallery</span></span>
<span data-ttu-id="af38d-124">Při konfiguraci integrace služby Zscaler privátní přístup (ZPA) do služby Azure AD, potřebujete přidat Zscaler privátní přístup (ZPA) z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="af38d-124">To configure the integration of Zscaler Private Access (ZPA) into Azure AD, you need to add Zscaler Private Access (ZPA) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="af38d-125">**Pokud chcete přidat Zscaler privátní přístup (ZPA) z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="af38d-125">**To add Zscaler Private Access (ZPA) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="af38d-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="af38d-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="af38d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="af38d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="af38d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="af38d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="af38d-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af38d-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="af38d-133">Do vyhledávacího pole zadejte **Zscaler privátní přístup (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="af38d-133">In the search box, type **Zscaler Private Access (ZPA)**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. <span data-ttu-id="af38d-135">Na panelu výsledků vyberte **Zscaler privátní přístup (ZPA)**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af38d-135">In the results panel, select **Zscaler Private Access (ZPA)**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="af38d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="af38d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="af38d-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler privátní přístup (ZPA) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="af38d-138">In this section, you configure and test Azure AD single sign-on with Zscaler Private Access (ZPA) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="af38d-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Zscaler privátní přístup (ZPA) je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="af38d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler Private Access (ZPA) is to a user in Azure AD.</span></span> <span data-ttu-id="af38d-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="af38d-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler Private Access (ZPA) needs to be established.</span></span>

<span data-ttu-id="af38d-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="af38d-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zscaler Private Access (ZPA).</span></span>

<span data-ttu-id="af38d-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler privátní přístup (ZPA), je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="af38d-142">To configure and test Azure AD single sign-on with Zscaler Private Access (ZPA), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="af38d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="af38d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="af38d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="af38d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="af38d-145">**[Vytvoření zkušebního uživatele Zscaler privátní přístup (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**  – Pokud chcete mít v Zscaler privátní přístup (ZPA) propojeném s Azure AD reprezentace jí protějšek Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="af38d-145">**[Creating a Zscaler Private Access (ZPA) test user](#creating-a-zscaler-private-access-(zpa)-test-user)** - to have a counterpart of Britta Simon in Zscaler Private Access (ZPA) that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="af38d-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="af38d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="af38d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="af38d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="af38d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="af38d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="af38d-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="af38d-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Zscaler Private Access (ZPA) application.</span></span>

<span data-ttu-id="af38d-150">**Pokud chcete konfigurovat Azure AD jednotné přihlašování s Zscaler privátní přístup (ZPA), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="af38d-150">**To configure Azure AD single sign-on with Zscaler Private Access (ZPA), perform the following steps:**</span></span>

1. <span data-ttu-id="af38d-151">Na portálu Azure Management portal na **Zscaler privátní přístup (ZPA)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="af38d-151">In the Azure Management portal, on the **Zscaler Private Access (ZPA)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="af38d-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="af38d-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="af38d-155">Na **Zscaler privátní přístup (ZPA) domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="af38d-155">On the **Zscaler Private Access (ZPA) Domain and URLs** section, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    <span data-ttu-id="af38d-157">a.</span><span class="sxs-lookup"><span data-stu-id="af38d-157">a.</span></span> <span data-ttu-id="af38d-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span><span class="sxs-lookup"><span data-stu-id="af38d-158">In the **Sign On URL** textbox, type a URL using the following pattern: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span></span>

    <span data-ttu-id="af38d-159">b.</span><span class="sxs-lookup"><span data-stu-id="af38d-159">b.</span></span> <span data-ttu-id="af38d-160">V **identifikátor** textovému poli, typ:`https://samlsp.private.zscaler.com/auth/metadata`</span><span class="sxs-lookup"><span data-stu-id="af38d-160">In the **Identifier** textbox, type: `https://samlsp.private.zscaler.com/auth/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="af38d-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="af38d-161">Please note that these are not the real values.</span></span> <span data-ttu-id="af38d-162">Budete muset aktualizovat tyto hodnoty se skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="af38d-162">You have to update these values with the actual Sign On URL and Identifier.</span></span> <span data-ttu-id="af38d-163">Zde, doporučujeme vám použít jedinečnou hodnotu adresy URL v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="af38d-163">Here we suggest you to use the unique value of URL in the Identifier.</span></span> <span data-ttu-id="af38d-164">Obraťte se na [tým podpory Zscaler privátní přístup (ZPA)](https://help.zscaler.com/zpa-submit-ticket) získat tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="af38d-164">Contact [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) to get these values.</span></span>

4. <span data-ttu-id="af38d-165">Na **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="af38d-165">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. <span data-ttu-id="af38d-167">Na **vytvořit nový certifikát** dialogové okno, klikněte na ikonu kalendáři a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="af38d-167">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="af38d-168">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="af38d-168">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="af38d-170">Na **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="af38d-170">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. <span data-ttu-id="af38d-172">V místní nabídce **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="af38d-172">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="af38d-174">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="af38d-174">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. <span data-ttu-id="af38d-176">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="af38d-176">In a different web browser window, log into your Zscaler Private Access (ZPA) company site as an administrator.</span></span>

10. <span data-ttu-id="af38d-177">Přejděte na **správce** a pak klikněte na **Idp konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="af38d-177">Navigate to **Administrator** and then click **Idp Configuration**.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. <span data-ttu-id="af38d-179">V **Idp konfigurace** klikněte na tlačítko **přidat novou konfiguraci IDP**.</span><span class="sxs-lookup"><span data-stu-id="af38d-179">In the **Idp Configuration** section, click **Add New IDP Configuration**.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. <span data-ttu-id="af38d-181">V **novou konfiguraci IDP** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="af38d-181">In the **New IDP Configuration** section, perform the following steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    <span data-ttu-id="af38d-183">a.</span><span class="sxs-lookup"><span data-stu-id="af38d-183">a.</span></span> <span data-ttu-id="af38d-184">Klikněte na tlačítko **vyberte soubor** a nahrát soubor stažený metadat.</span><span class="sxs-lookup"><span data-stu-id="af38d-184">Click **Select File** and upload your downloaded metadata file.</span></span>

    <span data-ttu-id="af38d-185">b.</span><span class="sxs-lookup"><span data-stu-id="af38d-185">b.</span></span> <span data-ttu-id="af38d-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="af38d-186">Click **Save** button.</span></span>
    


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="af38d-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="af38d-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="af38d-188">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="af38d-188">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="af38d-190">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="af38d-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="af38d-191">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="af38d-191">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="af38d-193">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="af38d-193">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="af38d-195">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af38d-195">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="af38d-197">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="af38d-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="af38d-199">a.</span><span class="sxs-lookup"><span data-stu-id="af38d-199">a.</span></span> <span data-ttu-id="af38d-200">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="af38d-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="af38d-201">b.</span><span class="sxs-lookup"><span data-stu-id="af38d-201">b.</span></span> <span data-ttu-id="af38d-202">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="af38d-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="af38d-203">c.</span><span class="sxs-lookup"><span data-stu-id="af38d-203">c.</span></span> <span data-ttu-id="af38d-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="af38d-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="af38d-205">d.</span><span class="sxs-lookup"><span data-stu-id="af38d-205">d.</span></span> <span data-ttu-id="af38d-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="af38d-206">Click **Create**.</span></span> 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a><span data-ttu-id="af38d-207">Vytvoření zkušebního uživatele Zscaler privátní přístup (ZPA)</span><span class="sxs-lookup"><span data-stu-id="af38d-207">Creating a Zscaler Private Access (ZPA) test user</span></span>

<span data-ttu-id="af38d-208">V této části vytvoříte uživatele názvem Britta Simon v Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="af38d-208">In this section, you create a user called Britta Simon in Zscaler Private Access (ZPA).</span></span> <span data-ttu-id="af38d-209">Spojte se s [tým podpory Zscaler privátní přístup (ZPA)](https://help.zscaler.com/zpa-submit-ticket) přidat uživatele do platformy Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="af38d-209">Please work with [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) to add the users in the Zscaler Private Access (ZPA) platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="af38d-210">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="af38d-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="af38d-211">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="af38d-211">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Zscaler Private Access (ZPA).</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="af38d-213">**Pokud chcete přiřadit Britta Simon k Zscaler privátní přístup (ZPA), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="af38d-213">**To assign Britta Simon to Zscaler Private Access (ZPA), perform the following steps:**</span></span>

1. <span data-ttu-id="af38d-214">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="af38d-214">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="af38d-216">V seznamu aplikací vyberte **Zscaler privátní přístup (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="af38d-216">In the applications list, select **Zscaler Private Access (ZPA)**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. <span data-ttu-id="af38d-218">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="af38d-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="af38d-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="af38d-220">Click **Add** button.</span></span> <span data-ttu-id="af38d-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af38d-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="af38d-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="af38d-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="af38d-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af38d-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="af38d-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="af38d-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="af38d-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="af38d-226">Testing single sign-on</span></span>

<span data-ttu-id="af38d-227">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="af38d-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="af38d-228">Když kliknete na dlaždici Zscaler privátní přístup (ZPA) na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="af38d-228">When you click the Zscaler Private Access (ZPA) tile in the Access Panel, you should get automatically signed-on to your Zscaler Private Access (ZPA) application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="af38d-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="af38d-229">Additional resources</span></span>

* [<span data-ttu-id="af38d-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="af38d-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="af38d-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="af38d-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png