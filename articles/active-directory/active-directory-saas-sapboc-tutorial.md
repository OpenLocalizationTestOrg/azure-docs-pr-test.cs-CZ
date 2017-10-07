---
title: 'Kurz: Azure Active Directory integrace s SAP Business objektu cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP Business objektu cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: a3e9bd93897271531f91bcbc50cd361e8a20551e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a><span data-ttu-id="9dfd7-103">Kurz: Azure Active Directory integrace s SAP Business objektu cloudu</span><span class="sxs-lookup"><span data-stu-id="9dfd7-103">Tutorial: Azure Active Directory integration with SAP Business Object Cloud</span></span>

<span data-ttu-id="9dfd7-104">V tomto kurzu zjistíte, jak toointegrate SAP Business objektu cloudu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-104">In this tutorial, you learn how toointegrate SAP Business Object Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9dfd7-105">Můžete získat hello při integraci SAP Business objektu cloudu s Azure AD následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-105">You get hello following benefits when you integrate SAP Business Object Cloud with Azure AD:</span></span>

- <span data-ttu-id="9dfd7-106">Ve službě Azure AD můžete řídit, kdo má přístup tooSAP obchodní objekt cloudu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-106">In Azure AD, you can control who has access tooSAP Business Object Cloud.</span></span>
- <span data-ttu-id="9dfd7-107">Vaši uživatelé tooSAP obchodní objekt cloudu můžete automaticky přihlásit pomocí jednotného přihlašování a účtu uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-107">You can automatically sign in your users tooSAP Business Object Cloud by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="9dfd7-108">Můžete spravovat své účty pomocí nich centrální umístění, hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-108">You can manage your accounts in one, central location, hello Azure portal.</span></span>

<span data-ttu-id="9dfd7-109">toolearn Další informace o softwaru, služba (SaaS) aplikace integraci s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-109">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9dfd7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9dfd7-110">Prerequisites</span></span>

<span data-ttu-id="9dfd7-111">tooset až integrace Azure AD s SAP Business objektu cloudu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-111">tooset up Azure AD integration with SAP Business Object Cloud, you need hello following items:</span></span>

- <span data-ttu-id="9dfd7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dfd7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9dfd7-113">SAP Business objektu cloudu, pomocí jednotného přihlašování zapnutý</span><span class="sxs-lookup"><span data-stu-id="9dfd7-113">An SAP Business Object Cloud, with single sign-on turned on</span></span>

> [!NOTE]
> <span data-ttu-id="9dfd7-114">Pokud testujete hello kroky v tomto kurzu, doporučujeme, abyste si je vyzkoušeli v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-114">If you test hello steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="9dfd7-115">Doporučení pro testování hello kroky v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-115">Recommendations for testing hello steps in this tutorial:</span></span>

- <span data-ttu-id="9dfd7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="9dfd7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9dfd7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9dfd7-118">Scenario description</span></span>
<span data-ttu-id="9dfd7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="9dfd7-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9dfd7-121">Přidejte SAP Business objektu cloudu z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-121">Add SAP Business Object Cloud from hello gallery.</span></span>
2. <span data-ttu-id="9dfd7-122">Nastavte a otestujte Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-sap-business-object-cloud-from-hello-gallery"></a><span data-ttu-id="9dfd7-123">Přidat SAP Business objektu cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9dfd7-123">Add SAP Business Object Cloud from hello gallery</span></span>
<span data-ttu-id="9dfd7-124">tooset až hello integrace SAP cloudu obchodní objekt s Azure AD v galerii hello přidat SAP Business objektu cloudu tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-124">tooset up hello integration of SAP Business Object Cloud with Azure AD, in hello gallery, add SAP Business Object Cloud tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9dfd7-125">tooadd SAP Business objektu cloudu z Galerie hello:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-125">tooadd SAP Business Object Cloud from hello gallery:</span></span>

1. <span data-ttu-id="9dfd7-126">V hello [portál Azure](https://portal.azure.com), v levé nabídce text hello, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-126">In hello [Azure portal](https://portal.azure.com), in hello left menu, select **Azure Active Directory**.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="9dfd7-128">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![stránka aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="9dfd7-130">tooadd novou aplikaci, vyberte **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-130">tooadd a new application, select **New application**.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="9dfd7-132">Hello vyhledávacího pole zadejte **SAP Business objektu cloudu**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-132">In hello search box, enter **SAP Business Object Cloud**.</span></span>

    ![Hello vyhledávacího pole](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. <span data-ttu-id="9dfd7-134">Na panelu výsledků hello vyberte **SAP Business objektu cloudu**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-134">In hello results panel, select **SAP Business Object Cloud**, and then select **Add**.</span></span>

    ![SAP Business objektu cloudu v seznamu výsledků hello](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9dfd7-136">Nastavte a otestujte Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9dfd7-136">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="9dfd7-137">V této části můžete nastavit a testovací Azure AD jednotného přihlášení SAP Business objektu cloudu podle testovacího uživatele s názvem *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-137">In this section, you set up and test Azure AD single sign-on with SAP Business Object Cloud based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="9dfd7-138">Pro toowork jeden přihlašování musí Azure AD tooknow hello Azure AD příslušného uživatele v cloudu objekt SAP Business.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-138">For single sign-on toowork, Azure AD needs tooknow hello Azure AD counterpart user in SAP Business Object Cloud.</span></span> <span data-ttu-id="9dfd7-139">Je nutné vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské hello v SAP Business objektu cloudu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-139">A link relationship between an Azure AD user and hello related user in SAP Business Object Cloud must be established.</span></span>

<span data-ttu-id="9dfd7-140">tooestablish hello propojení vztahu v rámci cloudu objekt SAP Business, pro **uživatelské jméno**, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-140">tooestablish hello link relationship, in SAP Business Object Cloud, for **Username**, assign hello value of hello **user name** in Azure AD.</span></span>

<span data-ttu-id="9dfd7-141">tooconfigure a testování Azure AD jednotného přihlášení SAP Business objektu cloudu, dokončení hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-141">tooconfigure and test Azure AD single sign-on with SAP Business Object Cloud, complete hello following tasks:</span></span>

1. <span data-ttu-id="9dfd7-142">[Nastavení Azure AD jednotné přihlašování](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-142">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="9dfd7-143">Nastaví uživatele toouse tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-143">Sets up a user toouse this feature.</span></span>
2. <span data-ttu-id="9dfd7-144">[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-144">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="9dfd7-145">Testy Azure AD jednotné přihlašování s uživatelem hello Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-145">Tests Azure AD single sign-on with hello user Britta Simon.</span></span>
3. <span data-ttu-id="9dfd7-146">[Vytvořit testovací uživatele s SAP Business objektu cloudu](#create-an-sap-business-object-cloud-test-user).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-146">[Create an SAP Business Object Cloud test user](#create-an-sap-business-object-cloud-test-user).</span></span> <span data-ttu-id="9dfd7-147">Vytvoří protějšek Britta Simon SAP Business objektu cloudu, který je propojený toohello reprezentace hello uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-147">Creates a counterpart of Britta Simon in SAP Business Object Cloud that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="9dfd7-148">[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-148">[Assign hello Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="9dfd7-149">Nastaví Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-149">Sets up Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9dfd7-150">[Test jednotného přihlašování](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-150">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="9dfd7-151">Ověřuje, že konfigurace hello funguje.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-151">Verifies that hello configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="9dfd7-152">Nastavení Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9dfd7-152">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="9dfd7-153">V této části můžete zapnout jedním Azure AD přihlášení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-153">In this section, you turn on Azure AD single sign-on in hello Azure portal.</span></span> <span data-ttu-id="9dfd7-154">Potom nastavíte jednotné přihlašování v aplikaci SAP Business objektu cloudu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-154">Then, you set up single sign-on in your SAP Business Object Cloud application.</span></span>

<span data-ttu-id="9dfd7-155">tooset do Azure AD jednotné přihlašování s SAP Business objektu cloudu:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-155">tooset up Azure AD single sign-on with SAP Business Object Cloud:</span></span>

1. <span data-ttu-id="9dfd7-156">V portálu Azure, na hello hello **SAP Business objektu cloudu** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-156">In hello Azure portal, on hello **SAP Business Object Cloud** application integration page, select **Single sign-on**.</span></span>

    ![Vybrat jednotné přihlašování][4]

2. <span data-ttu-id="9dfd7-158">Na hello **jednotného přihlašování** stránky, pro **režimu**, vyberte **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-158">On hello **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Vyberte na základě SAML přihlášení](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. <span data-ttu-id="9dfd7-160">V části **SAP Business objekt cloudové domény a adresy URL**, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-160">Under **SAP Business Object Cloud Domain and URLs**, complete hello following steps:</span></span>

    1. <span data-ttu-id="9dfd7-161">V hello **přihlašovací adresa URL** zadejte adresu URL, která má následující vzor hello:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-161">In hello **Sign-on URL** box, enter a URL that has hello following pattern:</span></span> 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. <span data-ttu-id="9dfd7-162">V hello **identifikátor** zadejte adresu URL, která má následující vzor hello:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-162">In hello **Identifier** box, enter a URL that has hello following pattern:</span></span>
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![Adresy URL stránky SAP Business objekt cloudové domény a adresy URL](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > <span data-ttu-id="9dfd7-164">Hello hodnoty v těchto adres URL jsou pouze jako ukázka.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-164">hello values in these URLs are for demonstration only.</span></span> <span data-ttu-id="9dfd7-165">Aktualizace hodnoty hello hello skutečné přihlašování adresy URL a identifikátoru adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-165">Update hello values with hello actual sign-on URL and identifier URL.</span></span> <span data-ttu-id="9dfd7-166">tooget hello přihlašovací adresa URL, kontaktujte hello [tým podpory SAP Business objektu cloudu klienta](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-166">tooget hello sign-on URL, contact hello [SAP Business Object Cloud Client support team](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span></span> <span data-ttu-id="9dfd7-167">Hello identifikátoru adresy URL můžete získat tak, že stáhnete hello SAP Business objektu cloudu metadat z konzoly Správce hello.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-167">You can get hello identifier URL by downloading hello SAP Business Object Cloud metadata from hello admin console.</span></span> <span data-ttu-id="9dfd7-168">To se vysvětluje dále v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-168">This is explained later in hello tutorial.</span></span> 

4. <span data-ttu-id="9dfd7-169">V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-169">Under **SAML Signing Certificate**, select **Metadata XML**.</span></span> <span data-ttu-id="9dfd7-170">Uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-170">Then, save hello metadata file on your computer.</span></span>

    ![Vyberte soubor XML s metadaty](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. <span data-ttu-id="9dfd7-172">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-172">Select **Save**.</span></span>

    ![Vyberte Uložit.](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9dfd7-174">V okně prohlížeče jiný web Přihlaste se jako správce v tooyour SAP Business objektu cloudu na webu společnosti.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-174">In a different web browser window, sign in tooyour SAP Business Object Cloud company site as an administrator.</span></span>

7. <span data-ttu-id="9dfd7-175">Vyberte **nabídky** > **systému** > **správy**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-175">Select **Menu** > **System** > **Administration**.</span></span>
    
    ![Vyberte nabídky, pak systém a Správa](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. <span data-ttu-id="9dfd7-177">Na hello **zabezpečení** karty, vyberte hello **upravit** ikona (pera).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-177">On hello **Security** tab, select hello **Edit** (pen) icon.</span></span>
    
    ![Na kartě zabezpečení hello vyberte ikonu pro úpravu hello](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. <span data-ttu-id="9dfd7-179">Pro **metodu ověřování**, vyberte **SAML jednotné přihlašování (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-179">For **Authentication Method**, select **SAML Single Sign-On (SSO)**.</span></span>

    ![Vyberte SAML jednotné přihlašování pro metodu ověřování hello](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. <span data-ttu-id="9dfd7-181">toodownload hello služby metadat zprostředkovatelů (krok 1), vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-181">toodownload hello service provider metadata (Step 1), select **Download**.</span></span> <span data-ttu-id="9dfd7-182">V souboru metadat hello, najít a zkopírovat hello **entityID** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-182">In hello metadata file, find and copy hello **entityID** value.</span></span> <span data-ttu-id="9dfd7-183">V hello Azure portálu pod **SAP Business objekt cloudové domény a adresy URL**, vložte hodnotu hello hello **identifikátor** pole.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-183">In hello Azure portal, under **SAP Business Object Cloud Domain and URLs**, paste hello value in hello **Identifier** box.</span></span>

    ![Zkopírujte a vložte hodnotu entityID hello](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. <span data-ttu-id="9dfd7-185">tooupload hello služby zprostředkovatele metadat (krok 2) v hello souboru, který jste stáhli z hello portál Azure, v části **nahrát zprostředkovatele Identity metadata**, vyberte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-185">tooupload hello service provider metadata (Step 2) in hello file that you downloaded from hello Azure portal, under **Upload Identity Provider metadata**, select **Upload**.</span></span>  

    ![V části metadata nahrát zprostředkovatele Identity vyberte možnost odeslat](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. <span data-ttu-id="9dfd7-187">V hello **atribut uživatele** seznamu, vyberte hello atribut uživatele (krok 3), chcete toouse týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-187">In hello **User Attribute** list, select hello user attribute (Step 3) that you want toouse for your implementation.</span></span> <span data-ttu-id="9dfd7-188">Tento atribut uživatele mapuje toohello zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-188">This user attribute maps toohello identity provider.</span></span> <span data-ttu-id="9dfd7-189">tooenter vlastní atribut na stránku hello uživatele, použijte hello **vlastní mapování SAML** možnost.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-189">tooenter a custom attribute on hello user's page, use hello **Custom SAML Mapping** option.</span></span> <span data-ttu-id="9dfd7-190">Nebo můžete vybrat buď **e-mailu** nebo **ID uživatele** jako atribut uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-190">Or, you can select either **Email** or **USER ID** as hello user attribute.</span></span> <span data-ttu-id="9dfd7-191">V našem příkladu jsme vybrali **e-mailu** vzhledem k tomu můžeme namapované hello deklarace identifikátoru uživatele s hello **userprincipalname** atribut v hello **uživatelské atributy** část v hello Portál Azure.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-191">In our example, we selected **Email** because we mapped hello user identifier claim with hello **userprincipalname** attribute in hello **User Attributes** section in hello Azure portal.</span></span> <span data-ttu-id="9dfd7-192">To poskytuje e-mail jedinečný uživatele, který se odešle toohello SAP Business objekt cloudových aplikací v každé úspěšné odpovědi SAML.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-192">This provides a unique user email, which is sent toohello SAP Business Object Cloud application in every successful SAML response.</span></span>

    ![Vyberte atribut uživatele](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. <span data-ttu-id="9dfd7-194">účet hello tooverify s hello zprostředkovatele identity (krok 4), v hello **přihlašovací pověření (e-mailu)** zadejte hello uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-194">tooverify hello account with hello identity provider (Step 4), in hello **Login Credential (Email)** box, enter hello user's email address.</span></span> <span data-ttu-id="9dfd7-195">Pak vyberte **ověřte účet**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-195">Then, select **Verify Account**.</span></span> <span data-ttu-id="9dfd7-196">Hello systému přidá přihlašovací údaje toohello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-196">hello system adds sign-in credentials toohello user account.</span></span>

    ![Zadejte e-mailu a vyberte účet, ověřte](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. <span data-ttu-id="9dfd7-198">Vyberte hello **Uložit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-198">Select hello **Save** icon.</span></span>

    ![Ikonu Uložit](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="9dfd7-200">Můžete si přečíst stručným verzi tyto pokyny v hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9dfd7-200">You can read a concise version of these instructions in hello [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="9dfd7-201">Po přidání aplikace hello výběrem **služby Active Directory** > **podnikové aplikace, které**, vyberte hello **jednotné přihlašování** kartě. Můžete získat přístup k dokumentaci hello vložených v hello **konfigurace** oddíl na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-201">After you add hello app by selecting **Active Directory** > **Enterprise Applications**, select hello **Single Sign-On** tab. You can access hello embedded documentation in hello **Configuration** section, at hello bottom of hello page.</span></span> <span data-ttu-id="9dfd7-202">Další informace najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-202">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9dfd7-203">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dfd7-203">Create an Azure AD test user</span></span>
<span data-ttu-id="9dfd7-204">V této části vytvoříte testovacího uživatele s názvem Britta Simon v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-204">In this section, you create a test user named Britta Simon in hello Azure portal.</span></span>

<span data-ttu-id="9dfd7-205">toocreate testovacího uživatele ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-205">toocreate a test user in Azure AD:</span></span>

1. <span data-ttu-id="9dfd7-206">V hello portál Azure, v levé nabídce hello, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-206">In hello Azure portal, in hello left menu, select **Azure Active Directory**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9dfd7-208">toodisplay hello seznam uživatelů, vyberte **uživatelů a skupin**a potom vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-208">toodisplay hello list of users, select **Users and groups**, and then select **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9dfd7-210">tooopen hello **uživatele** dialogové okno, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-210">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9dfd7-212">V hello **uživatele** dialogové okno, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-212">In hello **User** dialog box, complete hello following steps:</span></span>
 
    1. <span data-ttu-id="9dfd7-213">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-213">In hello **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="9dfd7-214">V hello **uživatelské jméno** zadejte hello e-mailovou adresu uživatele hello Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-214">In hello **User name** box, enter hello email address of hello user Britta Simon.</span></span>

    3. <span data-ttu-id="9dfd7-215">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-215">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    4. <span data-ttu-id="9dfd7-216">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-216">Select **Create**.</span></span>

        ![Dialogové okno uživatelského Hello](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Vytvořit uživatele Azure AD][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a><span data-ttu-id="9dfd7-219">Vytvořit testovací uživatele s SAP Business objektu cloudu</span><span class="sxs-lookup"><span data-stu-id="9dfd7-219">Create an SAP Business Object Cloud test user</span></span>

<span data-ttu-id="9dfd7-220">Azure AD Uživatelé musí být zřízená v cloudu objekt SAP Business, než se můžete přihlásit tooSAP obchodní objekt cloudu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-220">Azure AD users must be provisioned in SAP Business Object Cloud before they can sign in tooSAP Business Object Cloud.</span></span> <span data-ttu-id="9dfd7-221">V cloudu objekt SAP Business zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-221">In SAP Business Object Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="9dfd7-222">tooprovision uživatelský účet:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-222">tooprovision a user account:</span></span>

1. <span data-ttu-id="9dfd7-223">Přihlaste se tooyour SAP Business objektu cloudu na webu společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-223">Sign in tooyour SAP Business Object Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="9dfd7-224">Vyberte **nabídky** > **zabezpečení** > **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-224">Select **Menu** > **Security** > **Users**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. <span data-ttu-id="9dfd7-226">Na hello **uživatelé** , tooadd nové podrobnosti uživatele, vyberte  **+** .</span><span class="sxs-lookup"><span data-stu-id="9dfd7-226">On hello **Users** page, tooadd new user details, select **+**.</span></span> 

    ![Stránka Přidat uživatele](./media/active-directory-saas-sapboc-tutorial/user4.png)

    <span data-ttu-id="9dfd7-228">Dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-228">Then, complete hello following steps:</span></span>

    1. <span data-ttu-id="9dfd7-229">V hello **ID uživatele** zadejte ID uživatele hello hello uživatele, jako je třeba **Britta**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-229">In hello **USER ID** box, enter hello user ID of hello user, like **Britta**.</span></span>

    2. <span data-ttu-id="9dfd7-230">V hello **KŘESTNÍ jméno** zadejte hello křestní jméno uživatele hello, jako je třeba **Britta**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-230">In hello **FIRST NAME** box, enter hello first name of hello user, like **Britta**.</span></span>

    3. <span data-ttu-id="9dfd7-231">V hello **příjmení** zadejte příjmení hello hello uživatele, jako je třeba **Simon**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-231">In hello **LAST NAME** box, enter hello last name of hello user, like **Simon**.</span></span>

    4. <span data-ttu-id="9dfd7-232">V hello **ZOBRAZOVANÝ název** zadejte úplný název hello hello uživatele, jako je třeba **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-232">In hello **DISPLAY NAME** box, enter hello full name of hello user, like **Britta Simon**.</span></span>

    5. <span data-ttu-id="9dfd7-233">V hello **e-mailu** zadejte hello e-mailovou adresu uživatele hello, jako je třeba  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="9dfd7-233">In hello **E-MAIL** box, enter hello email address of hello user, like **brittasimon@contoso.com**.</span></span>

    6. <span data-ttu-id="9dfd7-234">Na hello **vybrat role** vyberte hello vhodnou roli pro uživatele hello a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-234">On hello **Select Roles** page, select hello appropriate role for hello user, and then select **OK**.</span></span>

      ![Výběr role](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. <span data-ttu-id="9dfd7-236">Vyberte hello **Uložit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-236">Select hello **Save** icon.</span></span>  


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9dfd7-237">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9dfd7-237">Assign hello Azure AD test user</span></span>

<span data-ttu-id="9dfd7-238">V této části můžete povolit uživateli hello Britta Simon toouse Azure AD jednotné přihlašování, poskytněte hello uživatele účtu přístup tooSAP obchodní objekt cloudu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-238">In this section, you allow hello user Britta Simon toouse Azure AD single sign-on by granting hello user account access tooSAP Business Object Cloud.</span></span>

<span data-ttu-id="9dfd7-239">tooassign tooSAP Britta Simon obchodní objekt cloudu:</span><span class="sxs-lookup"><span data-stu-id="9dfd7-239">tooassign Britta Simon tooSAP Business Object Cloud:</span></span>

1. <span data-ttu-id="9dfd7-240">V hello portálu Azure otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-240">In hello Azure portal, open hello applications view, and then go toohello directory view.</span></span> <span data-ttu-id="9dfd7-241">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-241">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9dfd7-243">V seznamu aplikace hello vyberte **SAP Business objektu cloudu**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-243">In hello applications list, select **SAP Business Object Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. <span data-ttu-id="9dfd7-245">V levé nabídce hello, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-245">In hello left menu, select **Users and groups**.</span></span>

    ![Vyberte uživatele a skupiny][202] 

4. <span data-ttu-id="9dfd7-247">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-247">Select **Add**.</span></span> <span data-ttu-id="9dfd7-248">Potom na hello **přidat přiřazení** vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-248">Then, on hello **Add Assignment** page, select **Users and groups**.</span></span>

    ![stránka Přidat přiřazení Hello][203]

5. <span data-ttu-id="9dfd7-250">Na hello **uživatelů a skupin** v hello seznam uživatelů, vyberte na stránce **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-250">On hello **Users and groups** page, in hello list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="9dfd7-251">Na hello **uživatelů a skupin** vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-251">On hello **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="9dfd7-252">Na hello **přidat přiřazení** vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-252">On hello **Add Assignment** page, select **Assign**.</span></span>

![Přiřadit role uživatele hello][200] 
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9dfd7-254">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9dfd7-254">Test single sign-on</span></span>

<span data-ttu-id="9dfd7-255">V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-255">In this section, you test your Azure AD single sign-on configuration by using hello access panel.</span></span>

<span data-ttu-id="9dfd7-256">Když vyberete dlaždici SAP Business objektu cloudu hello panelu hello přístup, musí být automaticky přihlášeni tooyour SAP Business objekt cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9dfd7-256">When you select hello SAP Business Object Cloud tile in hello access panel, you should be automatically signed in tooyour SAP Business Object Cloud application.</span></span>

<span data-ttu-id="9dfd7-257">Další informace o hello přístupového panelu najdete v tématu [Úvod toohello přístupový panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9dfd7-257">For more information about hello access panel, see [Introduction toohello access panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9dfd7-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9dfd7-258">Additional resources</span></span>

* [<span data-ttu-id="9dfd7-259">Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9dfd7-259">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9dfd7-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9dfd7-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

