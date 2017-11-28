---
title: "Brána dat aaaInstall místní - Azure Logic Apps | Microsoft Docs"
description: "Než budete přistupovat ke zdrojům dat místně, nainstalujte bránu dat hello místní pro přenos dat rychlý a šifrování mezi zdrojů dat na místní a aplikacích logiky"
keywords: "přístup k datům na místní, přenos dat, šifrování, zdroje dat"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 01386a904d856ff545f2eca8eb1b008dcdc08574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-on-premises-data-gateway-for-azure-logic-apps"></a><span data-ttu-id="9d7d1-104">Nainstalovat bránu dat hello místní pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="9d7d1-104">Install hello on-premises data gateway for Azure Logic Apps</span></span>

<span data-ttu-id="9d7d1-105">Předtím, než aplikace logiky můžete přistupovat ke zdrojům dat místní, musíte nainstalovat a nastavit hello místní data gateway.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-105">Before your logic apps can access data sources on premises, you must install and set up hello on-premises data gateway.</span></span> <span data-ttu-id="9d7d1-106">Hello brány funguje jako mostu, který poskytuje přenos rychlé dat a šifrování mezi místními systémy a aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-106">hello gateway acts as a bridge that provides quick data transfer and encryption between on-premises systems and your logic apps.</span></span> <span data-ttu-id="9d7d1-107">Brána Hello předává data z místního zdroje na šifrované kanály prostřednictvím hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-107">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="9d7d1-108">Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-108">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="9d7d1-109">Další informace o [fungování brány dat hello](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-109">Learn more about [how hello data gateway works](#gateway-cloud-service).</span></span>

<span data-ttu-id="9d7d1-110">Hello brána podporuje místní zdroje dat toothese připojení:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-110">hello gateway supports connections toothese data sources on premises:</span></span>

*   <span data-ttu-id="9d7d1-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="9d7d1-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="9d7d1-112">DB2</span><span class="sxs-lookup"><span data-stu-id="9d7d1-112">DB2</span></span>  
*   <span data-ttu-id="9d7d1-113">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="9d7d1-113">File System</span></span>
*   <span data-ttu-id="9d7d1-114">Informix</span><span class="sxs-lookup"><span data-stu-id="9d7d1-114">Informix</span></span>
*   <span data-ttu-id="9d7d1-115">MQ</span><span class="sxs-lookup"><span data-stu-id="9d7d1-115">MQ</span></span>
*   <span data-ttu-id="9d7d1-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="9d7d1-116">MySQL</span></span>
*   <span data-ttu-id="9d7d1-117">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="9d7d1-117">Oracle Database</span></span>
*   <span data-ttu-id="9d7d1-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="9d7d1-118">PostgreSQL</span></span>
*   <span data-ttu-id="9d7d1-119">Aplikace serveru SAP</span><span class="sxs-lookup"><span data-stu-id="9d7d1-119">SAP Application Server</span></span> 
*   <span data-ttu-id="9d7d1-120">Zpráva serveru SAP</span><span class="sxs-lookup"><span data-stu-id="9d7d1-120">SAP Message Server</span></span>
*   <span data-ttu-id="9d7d1-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="9d7d1-121">SharePoint</span></span>
*   <span data-ttu-id="9d7d1-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="9d7d1-122">SQL Server</span></span>
*   <span data-ttu-id="9d7d1-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="9d7d1-123">Teradata</span></span>

<span data-ttu-id="9d7d1-124">Tyto kroky ukazují, jak instalace hello toofirst místní brána dat před [nastavit připojení mezi bránou hello a aplikace logiky](./logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-124">These steps show how toofirst install hello on-premises data gateway before you [set up a connection between hello gateway and your logic apps](./logic-apps-gateway-connection.md).</span></span> <span data-ttu-id="9d7d1-125">Další informace o podporované konektory najdete v tématu [konektory pro Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).</span></span> 

<span data-ttu-id="9d7d1-126">Informace o tom, jak toouse hello brány s jinými službami najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-126">For information about how toouse hello gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="9d7d1-127">Microsoft Power BI místní brány dat</span><span class="sxs-lookup"><span data-stu-id="9d7d1-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="9d7d1-128">Služba Azure gateway místní dat služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="9d7d1-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="9d7d1-129">Microsoft Flow místní brány dat</span><span class="sxs-lookup"><span data-stu-id="9d7d1-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="9d7d1-130">Microsoft PowerApps místní brány dat</span><span class="sxs-lookup"><span data-stu-id="9d7d1-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>
## <a name="requirements"></a><span data-ttu-id="9d7d1-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9d7d1-131">Requirements</span></span>

<span data-ttu-id="9d7d1-132">**Minimální**:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-132">**Minimum**:</span></span>

* <span data-ttu-id="9d7d1-133">Rozhraní .NET 4.5 framework</span><span class="sxs-lookup"><span data-stu-id="9d7d1-133">.NET 4.5 Framework</span></span>
* <span data-ttu-id="9d7d1-134">64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="9d7d1-134">64-bit version of Windows 7 or Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="9d7d1-135">**Doporučená**:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-135">**Recommended**:</span></span>

* <span data-ttu-id="9d7d1-136">8 jader procesoru</span><span class="sxs-lookup"><span data-stu-id="9d7d1-136">8 Core CPU</span></span>
* <span data-ttu-id="9d7d1-137">8 GB paměti</span><span class="sxs-lookup"><span data-stu-id="9d7d1-137">8 GB Memory</span></span>
* <span data-ttu-id="9d7d1-138">64bitová verze systému Windows 2012 R2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="9d7d1-138">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="9d7d1-139">**Je důležité zvážit**:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-139">**Important considerations**:</span></span>

* <span data-ttu-id="9d7d1-140">Nainstalujte bránu dat místní hello pouze v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-140">Install hello on-premises data gateway only on a local computer.</span></span>
<span data-ttu-id="9d7d1-141">Hello bránu nejde nainstalovat na řadič domény.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-141">You can't install hello gateway on a domain controller.</span></span>

   > [!TIP]
   > <span data-ttu-id="9d7d1-142">Nemáte tooinstall hello brány na hello stejného počítače jako zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-142">You don't have tooinstall hello gateway on hello same computer as your data source.</span></span> <span data-ttu-id="9d7d1-143">latence toominimize, můžete nainstalovat hello brány jako zavřete jako zdroj dat možné tooyour nebo na hello stejný počítač, za předpokladu, že máte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-143">toominimize latency, you can install hello gateway as close as possible tooyour data source, or on hello same computer, assuming that you have permissions.</span></span>

* <span data-ttu-id="9d7d1-144">Neinstalujte hello brána na počítači, který vypne, přejde toosleep nebo toohello Internet není připojit, protože v těchto případech nelze spustit hello brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-144">Don't install hello gateway on a computer that turns off, goes toosleep, or doesn't connect toohello Internet because hello gateway can't run under those circumstances.</span></span> <span data-ttu-id="9d7d1-145">Navíc může sníží výkon brány přes bezdrátové sítě.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-145">Also, gateway performance might suffer over a wireless network.</span></span>

* <span data-ttu-id="9d7d1-146">Během instalace, musíte se odhlásit se [pracovní nebo školní účet](https://docs.microsoft.com/azure/active-directory/sign-up-organization) , který je spravován službou Azure Active Directory (Azure AD), nikoli účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-146">During installation, you must sign in with a [work or school account](https://docs.microsoft.com/azure/active-directory/sign-up-organization) that's managed by Azure Active Directory (Azure AD), not a Microsoft account.</span></span> 

  <span data-ttu-id="9d7d1-147">Máte toouse hello stejný pracovní nebo školní účet později v hello Azure portál při vytváření a přidružte prostředek brány k vaší instalace brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-147">You have toouse hello same work or school account later in hello Azure portal when you create and associate a gateway resource with your gateway installation.</span></span> <span data-ttu-id="9d7d1-148">Když vytvoříte hello připojení mezi logiku aplikace a hello místní zdroje dat pak vyberete tento prostředek brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-148">You then select this gateway resource when you create hello connection between your logic app and hello on-premises data source.</span></span> [<span data-ttu-id="9d7d1-149">Proč musí používat Azure AD pracovní nebo školní účet?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-149">Why must I use an Azure AD work or school account?</span></span>](#why-azure-work-school-account)

  > [!TIP]
  > <span data-ttu-id="9d7d1-150">Pokud jste se zaregistrovali do nabídky služeb Office 365 a nedodal e-mailu samotnou práci, může vypadat přihlašovací adresa jeff@contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-150">If you signed up for an Office 365 offering and didn't supply your actual work email, your sign-in address might look like jeff@contoso.onmicrosoft.com.</span></span> 

* <span data-ttu-id="9d7d1-151">Pokud máte existující bránu, kterou jste vytvořili pomocí Instalační program, který je starší než verze 14.16.6317.4, nelze změnit umístění vaše brána spuštěná hello nejnovější verzi Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-151">If you have an existing gateway that you set up with an installer that's earlier than version 14.16.6317.4, you can't change your gateway's location by running hello latest installer.</span></span> <span data-ttu-id="9d7d1-152">Můžete však použít hello nejnovější instalační program tooset novou bránu s hello umístění, které chcete místo toho.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-152">However, you can use hello latest installer tooset up a new gateway with hello location that you want instead.</span></span>
  
  <span data-ttu-id="9d7d1-153">Pokud máte bránu instalační program, který je starší než verze 14.16.6317.4, ale nemáte nainstalovanou bránu ještě, můžete stáhnout a použít nejnovější verzi instalačního programu hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-153">If you have a gateway installer that's earlier than version 14.16.6317.4, but you haven't installed your gateway yet, you can download and use hello latest installer.</span></span>

<a name="install-gateway"></a>

## <a name="install-hello-data-gateway"></a><span data-ttu-id="9d7d1-154">Nainstalovat bránu dat hello</span><span class="sxs-lookup"><span data-stu-id="9d7d1-154">Install hello data gateway</span></span>

1.  <span data-ttu-id="9d7d1-155">[Stáhněte a spusťte instalační program brány hello v místním počítači](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-155">[Download and run hello gateway installer on a local computer](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span></span>

2. <span data-ttu-id="9d7d1-156">Přečtěte si a přijměte podmínky použití a ochrana osobních údajů příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-156">Review and accept hello terms of use and privacy statement.</span></span>

3. <span data-ttu-id="9d7d1-157">Zadejte hello cestu v místním počítači místo, kam chcete bránu tooinstall hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-157">Specify hello path on your local computer where you want tooinstall hello gateway.</span></span>

4. <span data-ttu-id="9d7d1-158">Pokud budete vyzváni, přihlaste se pomocí vaší Azure pracovního nebo školního účtu, nikoli účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-158">When prompted, sign in with your Azure work or school account, not a Microsoft account.</span></span>

   ![Přihlaste se pomocí Azure pracovní nebo školní účet](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. <span data-ttu-id="9d7d1-160">Nyní zaregistrovat nainstalovanou bránu s hello [cloudové službě Brána pro](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-160">Now register your installed gateway with hello [gateway cloud service](#gateway-cloud-service).</span></span> <span data-ttu-id="9d7d1-161">Zvolte **registrace nové brány na tomto počítači**.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-161">Choose **Register a new gateway on this computer**.</span></span>

   <span data-ttu-id="9d7d1-162">cloudové službě Brána pro Hello šifruje a ukládá pověření ke zdroji dat a podrobnosti brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-162">hello gateway cloud service encrypts and stores your data source credentials and gateway details.</span></span> 
   <span data-ttu-id="9d7d1-163">Hello služby také směrování dotazy a jejich výsledky mezi svou aplikaci logiky, hello místní data brány a zdroje dat místně.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-163">hello service also routes queries and their results between your logic app, hello on-premises data gateway, and your data source on premises.</span></span>

6. <span data-ttu-id="9d7d1-164">Zadejte název pro instalaci brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-164">Provide a name for your gateway installation.</span></span> <span data-ttu-id="9d7d1-165">Vytvořit klíče pro obnovení a pak potvrďte obnovovací klíč.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-165">Create a recovery key, then confirm your recovery key.</span></span> 

   > [!IMPORTANT] 
   > <span data-ttu-id="9d7d1-166">Obnovovací klíč musí obsahovat alespoň osm znaků.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-166">Your recovery key must contain at least eight characters.</span></span> <span data-ttu-id="9d7d1-167">Zajistěte, aby si uložit a zachovat hello klíč na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-167">Make sure that you save and keep hello key in a safe place.</span></span> <span data-ttu-id="9d7d1-168">Musíte také tento klíč, když chcete toomigrate, obnovení nebo převzetí existující brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-168">You also need this key when you want toomigrate, restore, or take over an existing gateway.</span></span>

   1. <span data-ttu-id="9d7d1-169">Zvolte toochange hello výchozí oblast pro cloudové službě Brána pro hello a Azure Service Bus používá vaše instalace brány **změnu oblast**.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-169">toochange hello default region for hello gateway cloud service and Azure Service Bus used by your gateway installation, choose **Change Region**.</span></span>

      ![Oblast změny](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      <span data-ttu-id="9d7d1-171">Hello výchozí oblast je oblast hello přidružené klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-171">hello default region is hello region associated with your Azure AD tenant.</span></span>

   2. <span data-ttu-id="9d7d1-172">Na další podokno hello, otevřete hello **vyberte oblast** příliš zvolte jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-172">On hello next pane, open hello **Select Region** too choose a different region.</span></span>

      ![Vyberte jinou oblast](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      <span data-ttu-id="9d7d1-174">Například je může vyberte hello stejné oblasti jako svou aplikaci logiky, nebo vyberte hello oblast nejbližší tooyour na místní datové zdroje, můžete snížit latenci.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-174">For example, you might select hello same region as your logic app, or select hello region closest tooyour on-premises data source so you can reduce latency.</span></span> <span data-ttu-id="9d7d1-175">Brána prostředků a logiku aplikace může mít jiné umístění.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-175">Your gateway resource and logic app can have different locations.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="9d7d1-176">Tato oblast nelze změnit po instalaci.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-176">You can't change this region after installation.</span></span> <span data-ttu-id="9d7d1-177">Tato oblast také určuje a omezuje hello umístění, kde si můžete vytvořit hello prostředků Azure pro bránu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-177">This region also determines and restricts hello location where you can create hello Azure resource for your gateway.</span></span> <span data-ttu-id="9d7d1-178">Při vytváření prostředku brány v Azure, tak zkontrolujte, že umístění prostředku hello odpovídá hello oblast, kterou jste vybrali v průběhu instalace brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-178">So when you create your gateway resource in Azure, make sure that hello resource location matches hello region that you selected during gateway installation.</span></span>
      > 
      > <span data-ttu-id="9d7d1-179">Pokud chcete toouse v jiné oblasti pro bránu později, musíte nastavit novou bránu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-179">If you want toouse a different region for your gateway later, you must set up a new gateway.</span></span>

   3. <span data-ttu-id="9d7d1-180">Až budete připraveni, zvolte **provádí**.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-180">When you're ready, choose **Done**.</span></span>

7. <span data-ttu-id="9d7d1-181">Nyní postupujte podle těchto kroků v hello portálu Azure, abyste mohli [vytvořit prostředek služby Azure pro bránu](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-181">Now follow these steps in hello Azure portal so you can [create an Azure resource for your gateway](../logic-apps/logic-apps-gateway-connection.md).</span></span> 

<span data-ttu-id="9d7d1-182">Další informace o [fungování brány dat hello](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-182">Learn more about [how hello data gateway works](#gateway-cloud-service).</span></span>

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a><span data-ttu-id="9d7d1-183">Migrace, obnovení nebo převzetí existující brány</span><span class="sxs-lookup"><span data-stu-id="9d7d1-183">Migrate, restore, or take over an existing gateway</span></span>

<span data-ttu-id="9d7d1-184">tooperform tyto úlohy, musí mít hello obnovovací klíč, který byl zadán při instalaci brány hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-184">tooperform these tasks, you must have hello recovery key that was specified when hello gateway was installed.</span></span>

1. <span data-ttu-id="9d7d1-185">Z nabídky Start v počítači, zvolte **místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-185">From your computer's Start menu, choose **On-premises data gateway**.</span></span>

2. <span data-ttu-id="9d7d1-186">Po hello instalační program se otevře, přihlaste se pomocí hello stejné Azure pracovní nebo školní účet, který byl dříve používá tooinstall hello brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-186">After hello installer opens, sign in with hello same Azure work or school account that was previously used tooinstall hello gateway.</span></span>

3. <span data-ttu-id="9d7d1-187">Zvolte **migrace, obnovení nebo převzetí existující brány**.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-187">Choose **Migrate, restore, or take over an existing gateway**.</span></span>

4. <span data-ttu-id="9d7d1-188">Zadejte název a obnovení klíč hello hello brány, kterou chcete toomigrate, obnovení nebo proveďte přes.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-188">Provide hello name and recovery key for hello gateway that you want toomigrate, restore, or take over.</span></span>

<a name="restart-gateway"></a>
## <a name="restart-hello-gateway"></a><span data-ttu-id="9d7d1-189">Restartujte bránu hello</span><span class="sxs-lookup"><span data-stu-id="9d7d1-189">Restart hello gateway</span></span>

<span data-ttu-id="9d7d1-190">Hello brány se spouští jako služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-190">hello gateway runs as a Windows service.</span></span> <span data-ttu-id="9d7d1-191">Stejně jako ostatní služby systému Windows můžete spustit a zastavit službu hello několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-191">Like any other Windows service, you can start and stop hello service in multiple ways.</span></span> <span data-ttu-id="9d7d1-192">Můžete například otevřete příkazový řádek se zvýšenými oprávněními v počítači hello se spuštěným systémem hello brány a spusťte buď tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-192">For example, you can open a command prompt with elevated permissions on hello computer where hello gateway is running, and run either these commands:</span></span>

* <span data-ttu-id="9d7d1-193">toostop hello služba, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-193">toostop hello service, run this command:</span></span>
  
    `net stop PBIEgwService`

* <span data-ttu-id="9d7d1-194">toostart hello služba, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-194">toostart hello service, run this command:</span></span>
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a><span data-ttu-id="9d7d1-195">Účet služby systému Windows</span><span class="sxs-lookup"><span data-stu-id="9d7d1-195">Windows service account</span></span>

<span data-ttu-id="9d7d1-196">Brána dat Hello místní nastavení toouse `NT SERVICE\PBIEgwService` pro hello Windows služby přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-196">hello on-premises data gateway is set up toouse `NT SERVICE\PBIEgwService` for hello Windows service logon credentials.</span></span> <span data-ttu-id="9d7d1-197">Ve výchozím nastavení hello brány má pro počítač hello právo "Přihlásit jako službu" hello, kde instalujete hello brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-197">By default, hello gateway has hello "Log on as a service" right for hello machine where you install hello gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="9d7d1-198">účet služby systému Windows Hello se liší od účtu hello použité pro připojování tooon místní zdroje dat a od hello Azure pracovní nebo školní účet použít toosign v toocloud služby.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-198">hello Windows service account differs from hello account used for connecting tooon-premises data sources, and from hello Azure work or school account used toosign in toocloud services.</span></span>

## <a name="configure-a-firewall-or-proxy"></a><span data-ttu-id="9d7d1-199">Konfigurace brány firewall nebo proxy server</span><span class="sxs-lookup"><span data-stu-id="9d7d1-199">Configure a firewall or proxy</span></span>

<span data-ttu-id="9d7d1-200">Hello brány vytvoří odchozí připojení příliš [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-200">hello gateway creates an outbound connection too [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="9d7d1-201">informace o proxy serveru tooprovide pro bránu, najdete v části [konfigurace nastavení proxy serveru](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-201">tooprovide proxy information for your gateway, see [Configure proxy settings](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span></span>

<span data-ttu-id="9d7d1-202">toocheck zda brána firewall nebo proxy, může blokovat připojení, zkontrolujte, jestli váš počítač může připojit ve skutečnosti toohello Internetu a hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-202">toocheck whether your firewall, or proxy, might block connections, confirm whether your machine can actually connect toohello internet and hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="9d7d1-203">Z řádku prostředí PowerShell spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-203">From a PowerShell prompt, run this command:</span></span>

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> <span data-ttu-id="9d7d1-204">Tento příkaz pouze Otestuje připojení k síti a toohello připojení k Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-204">This command only tests network connectivity and connectivity toohello Azure Service Bus.</span></span> <span data-ttu-id="9d7d1-205">Takže příkaz hello neobsahuje nic toodo s hello bránu nebo hello cloudové službě brány, který šifruje a ukládá přihlašovací údaje a podrobnosti brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-205">So hello command doesn't have anything toodo with hello gateway or hello gateway cloud service that encrypts and stores your credentials and gateway details.</span></span> 
>
> <span data-ttu-id="9d7d1-206">Tento příkaz je taky pouze k dispozici v systému Windows Server 2012 R2 nebo novější a Windows 8.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-206">Also, this command is only available on Windows Server 2012 R2 or later, and Windows 8.1 or later.</span></span> <span data-ttu-id="9d7d1-207">V dřívějších verzích operačního systému, můžete použít Telnet příliš test připojení.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-207">On earlier OS versions, you can use Telnet too test connectivity.</span></span> <span data-ttu-id="9d7d1-208">Další informace o [Azure Service Bus a hybridní řešení](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-208">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

<span data-ttu-id="9d7d1-209">Výsledky by měl vypadat podobně jako příklad toothis:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-209">Your results should look similar toothis example:</span></span>

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

<span data-ttu-id="9d7d1-210">Pokud **TcpTestSucceeded** není nastaven příliš**True**, vám může blokována bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-210">If **TcpTestSucceeded** is not set too**True**, you might be blocked by a firewall.</span></span> <span data-ttu-id="9d7d1-211">Pokud chcete toobe komplexní, nahraďte hello **ComputerName** a **Port** hodnoty s hodnotami hello uvedené v části [konfigurace portů](#configure-ports) v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-211">If you want toobe comprehensive, substitute hello **ComputerName** and **Port** values with hello values listed under [Configure ports](#configure-ports) in this topic.</span></span>

<span data-ttu-id="9d7d1-212">brány firewall Hello také mohou blokovat připojení této hello díky toohello datových centrech Azure Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-212">hello firewall might also block connections that hello Azure Service Bus makes toohello Azure datacenters.</span></span> <span data-ttu-id="9d7d1-213">Pokud tento scénář se stane, schválit (odblokovat) všechny hello IP adres pro tyto datových center ve vašem regionu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-213">If this scenario happens, approve (unblock) all hello IP addresses for those datacenters in your region.</span></span> <span data-ttu-id="9d7d1-214">Pro tyto IP adresy [získání hello Azure IP adresy seznamu zde](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-214">For those IP addresses, [get hello Azure IP addresses list here](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

## <a name="configure-ports"></a><span data-ttu-id="9d7d1-215">Konfigurace portů</span><span class="sxs-lookup"><span data-stu-id="9d7d1-215">Configure ports</span></span>

<span data-ttu-id="9d7d1-216">Hello brány vytvoří odchozí připojení příliš[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) a komunikuje na odchozí porty: TCP 443 (výchozí), 5671, 5672, 9350 prostřednictvím 9354.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-216">hello gateway creates an outbound connection too[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) and communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span> <span data-ttu-id="9d7d1-217">Brána Hello nevyžaduje příchozí porty.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-217">hello gateway doesn't require inbound ports.</span></span> <span data-ttu-id="9d7d1-218">Další informace o [Azure Service Bus a hybridní řešení](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-218">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

| <span data-ttu-id="9d7d1-219">NÁZVY DOMÉN</span><span class="sxs-lookup"><span data-stu-id="9d7d1-219">DOMAIN NAMES</span></span> | <span data-ttu-id="9d7d1-220">ODCHOZÍ PORTY</span><span class="sxs-lookup"><span data-stu-id="9d7d1-220">OUTBOUND PORTS</span></span> | <span data-ttu-id="9d7d1-221">POPIS</span><span class="sxs-lookup"><span data-stu-id="9d7d1-221">DESCRIPTION</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9d7d1-222">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="9d7d1-222">*.analysis.windows.net</span></span> | <span data-ttu-id="9d7d1-223">443</span><span class="sxs-lookup"><span data-stu-id="9d7d1-223">443</span></span> | <span data-ttu-id="9d7d1-224">HTTPS</span><span class="sxs-lookup"><span data-stu-id="9d7d1-224">HTTPS</span></span> | 
| <span data-ttu-id="9d7d1-225">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="9d7d1-225">*.login.windows.net</span></span> | <span data-ttu-id="9d7d1-226">443</span><span class="sxs-lookup"><span data-stu-id="9d7d1-226">443</span></span> | <span data-ttu-id="9d7d1-227">HTTPS</span><span class="sxs-lookup"><span data-stu-id="9d7d1-227">HTTPS</span></span> | 
| <span data-ttu-id="9d7d1-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="9d7d1-228">*.servicebus.windows.net</span></span> | <span data-ttu-id="9d7d1-229">5671-5672</span><span class="sxs-lookup"><span data-stu-id="9d7d1-229">5671-5672</span></span> | <span data-ttu-id="9d7d1-230">Pokročilé zpráv služby Řízení front Protocol (AMQP)</span><span class="sxs-lookup"><span data-stu-id="9d7d1-230">Advanced Message Queuing Protocol (AMQP)</span></span> | 
| <span data-ttu-id="9d7d1-231">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="9d7d1-231">*.servicebus.windows.net</span></span> | <span data-ttu-id="9d7d1-232">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="9d7d1-232">443, 9350-9354</span></span> | <span data-ttu-id="9d7d1-233">Moduly pro naslouchání na předávání přes Service Bus přes TCP (vyžaduje 443 pro získání tokenu řízení přístupu)</span><span class="sxs-lookup"><span data-stu-id="9d7d1-233">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> | 
| <span data-ttu-id="9d7d1-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="9d7d1-234">*.frontend.clouddatahub.net</span></span> | <span data-ttu-id="9d7d1-235">443</span><span class="sxs-lookup"><span data-stu-id="9d7d1-235">443</span></span> | <span data-ttu-id="9d7d1-236">HTTPS</span><span class="sxs-lookup"><span data-stu-id="9d7d1-236">HTTPS</span></span> | 
| <span data-ttu-id="9d7d1-237">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="9d7d1-237">*.core.windows.net</span></span> | <span data-ttu-id="9d7d1-238">443</span><span class="sxs-lookup"><span data-stu-id="9d7d1-238">443</span></span> | <span data-ttu-id="9d7d1-239">HTTPS</span><span class="sxs-lookup"><span data-stu-id="9d7d1-239">HTTPS</span></span> | 
| <span data-ttu-id="9d7d1-240">Login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="9d7d1-240">login.microsoftonline.com</span></span> | <span data-ttu-id="9d7d1-241">443</span><span class="sxs-lookup"><span data-stu-id="9d7d1-241">443</span></span> | <span data-ttu-id="9d7d1-242">HTTPS</span><span class="sxs-lookup"><span data-stu-id="9d7d1-242">HTTPS</span></span> | 
| <span data-ttu-id="9d7d1-243">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="9d7d1-243">*.msftncsi.com</span></span> | <span data-ttu-id="9d7d1-244">443</span><span class="sxs-lookup"><span data-stu-id="9d7d1-244">443</span></span> | <span data-ttu-id="9d7d1-245">Připojení k Internetu tootest použít, když brána hello nedostupná pro hello služby Power BI.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-245">Used tootest internet connectivity when hello gateway is unreachable by hello Power BI service.</span></span> | 

<span data-ttu-id="9d7d1-246">Pokud máte tooapprove IP adresy místo hello domény, můžete stáhnout a použít hello [rozsahy IP Datacentra Azure Microsoft seznamu](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-246">If you have tooapprove IP addresses instead of hello domains, you can download and use hello [Microsoft Azure Datacenter IP ranges list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="9d7d1-247">V některých případech jsou vytvářeny hello Azure Service Bus připojení s IP adresou, nikoli plně kvalifikované názvy domény.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-247">In some cases, hello Azure Service Bus connections are made with IP Address rather than fully qualified domain names.</span></span>

<a name="gateway-cloud-service"></a>
## <a name="how-does-hello-data-gateway-work"></a><span data-ttu-id="9d7d1-248">Jak funguje hello bránu dat?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-248">How does hello data gateway work?</span></span>

<span data-ttu-id="9d7d1-249">Brána dat Hello usnadňuje rychle a bezpečně komunikaci mezi svou aplikaci logiky, cloudové službě Brána pro hello a zdroje dat na místě.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-249">hello data gateway facilitates quick and secure communication between your logic app, hello gateway cloud service, and your on-premises data source.</span></span> 

![Diagram-for-On-Premises-data-Gateway-Flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

<span data-ttu-id="9d7d1-251">Proto když uživatel hello v cloudu hello komunikuje element, který byl připojen tooyour místní zdroj dat:</span><span class="sxs-lookup"><span data-stu-id="9d7d1-251">So when hello user in hello cloud interacts with an element that's connected tooyour on-premises data source:</span></span>

1. <span data-ttu-id="9d7d1-252">cloudové službě Brána pro Hello vytvoří dotaz, společně s hello šifrovat přihlašovací údaje pro zdroj dat hello a odešle hello dotazu toohello fronty pro tooprocess hello brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-252">hello gateway cloud service creates a query, along with hello encrypted credentials for hello data source, and sends hello query toohello queue for hello gateway tooprocess.</span></span>

2. <span data-ttu-id="9d7d1-253">cloudové službě Brána pro Hello analyzuje hello dotazu a nabízených oznámení hello požadavek toohello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-253">hello gateway cloud service analyzes hello query and pushes hello request toohello Azure Service Bus.</span></span>

3. <span data-ttu-id="9d7d1-254">Brána dat místní Hello dotazuje hello Azure Service Bus pro žádosti čekající na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-254">hello on-premises data gateway polls hello Azure Service Bus for pending requests.</span></span>

4. <span data-ttu-id="9d7d1-255">Brána Hello získá hello dotazu, dešifruje hello přihlašovací údaje a připojí zdroj dat toohello tyto přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-255">hello gateway gets hello query, decrypts hello credentials, and connects toohello data source with those credentials.</span></span>

5. <span data-ttu-id="9d7d1-256">Brána Hello odešle datový zdroj toohello hello dotazu pro provedení.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-256">hello gateway sends hello query toohello data source for execution.</span></span>

6. <span data-ttu-id="9d7d1-257">výsledky Hello jsou odesílány z hello zdroj dat, back toohello brány a cloudové službě Brána pro toohello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-257">hello results are sent from hello data source, back toohello gateway, and then toohello gateway cloud service.</span></span> <span data-ttu-id="9d7d1-258">Hello cloudové službě Brána pak použije hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-258">hello gateway cloud service then uses hello results.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="9d7d1-259">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="9d7d1-259">Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="9d7d1-260">Obecné</span><span class="sxs-lookup"><span data-stu-id="9d7d1-260">General</span></span>

<span data-ttu-id="9d7d1-261">**Q**: Potřebuji bránu pro zdroje dat v cloudu hello, jako je například SQL Azure?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-261">**Q**: Do I need a gateway for data sources in hello cloud, such as SQL Azure?</span></span> <br/><span data-ttu-id="9d7d1-262">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-262">
**A**: No.</span></span> <span data-ttu-id="9d7d1-263">Brána se připojuje pouze zdroje dat tooon místní.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-263">A gateway connects tooon-premises data sources only.</span></span>

<span data-ttu-id="9d7d1-264">**Q**: nemá hello bránu nainstalovat na stejný počítač jako zdroj dat hello hello toobe?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-264">**Q**: Does hello gateway have toobe installed on hello same machine as hello data source?</span></span> <br/><span data-ttu-id="9d7d1-265">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-265">
**A**: No.</span></span> <span data-ttu-id="9d7d1-266">Brána Hello připojí toohello zdroje dat pomocí hello informace o připojení, který byl poskytnut.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-266">hello gateway connects toohello data source using hello connection information that was provided.</span></span> <span data-ttu-id="9d7d1-267">Vezměte v úvahu hello brány jako klientskou aplikaci v tomto smyslu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-267">Consider hello gateway as a client application in this sense.</span></span> <span data-ttu-id="9d7d1-268">bránu Hello je právě hello schopností tooconnect toohello název serveru, který byl poskytnut.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-268">hello gateway just needs hello capability tooconnect toohello server name that was provided.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="9d7d1-269">**Q**: Proč musí I používáte Azure pracovní nebo školní účet toosign v?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-269">**Q**: Why must I use an Azure work or school account toosign in?</span></span> <br/><span data-ttu-id="9d7d1-270">
**A**: pouze můžete použít Azure pracovní nebo školní účet, když instalujete hello místní data gateway.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-270">
**A**: You can only use an Azure work or school account when you install hello on-premises data gateway.</span></span> <span data-ttu-id="9d7d1-271">Váš přihlašovací účet je uložený v klientovi, který je spravován pomocí služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9d7d1-271">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="9d7d1-272">Obvykle účet Azure AD hlavní název uživatele (UPN) odpovídá hello e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-272">Usually, your Azure AD account's user principal name (UPN) matches hello email address.</span></span>

<span data-ttu-id="9d7d1-273">**Q**: uložení pověření?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-273">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="9d7d1-274">
**A**: hello přihlašovací údaje, které zadáte pro zdroj dat jsou zašifrovány a uložené v cloudové službě Brána pro hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-274">
**A**: hello credentials that you enter for a data source are encrypted and stored in hello gateway cloud service.</span></span> <span data-ttu-id="9d7d1-275">přihlašovací údaje Hello se dešifrují v hello místní data gateway.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-275">hello credentials are decrypted at hello on-premises data gateway.</span></span>

<span data-ttu-id="9d7d1-276">**Q**: existují všechny požadavky pro šířku pásma sítě?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-276">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="9d7d1-277">
**A**: doporučujeme, aby připojení k síti dobrý propustnost.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-277">
**A**: We recommend that your network connection has good throughput.</span></span> <span data-ttu-id="9d7d1-278">Každé prostředí je jiné a hello množství dat odesílaných ovlivňuje hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-278">Every environment is different, and hello amount of data being sent affects hello results.</span></span> <span data-ttu-id="9d7d1-279">Pomocí ExpressRoute může pomoct tooguarantee úrovní propustnosti mezi místními a hello datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-279">Using ExpressRoute could help tooguarantee a level of throughput between on-premises and hello Azure datacenters.</span></span>
<span data-ttu-id="9d7d1-280">Můžete vytvořit hello nástroj třetí strany Azure rychlost testovací aplikace toohelp měřidla vaší propustnost.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-280">You can use hello third-party tool Azure Speed Test app toohelp gauge your throughput.</span></span>

<span data-ttu-id="9d7d1-281">**Q**: co je hello latence pro zdroj dat tooa spuštěné dotazy z brány hello?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-281">**Q**: What is hello latency for running queries tooa data source from hello gateway?</span></span> <span data-ttu-id="9d7d1-282">Co je nejlepší architektura hello?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-282">What is hello best architecture?</span></span> <br/><span data-ttu-id="9d7d1-283">
**A**: tooreduce latence sítě, brána hello instalace jako zdroj dat zavřít toohello nejdříve.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-283">
**A**: tooreduce network latency, install hello gateway as close toohello data source as possible.</span></span> <span data-ttu-id="9d7d1-284">Pokud nainstalujete hello brány na zdroj dat skutečné hello tento blízkosti minimalizuje latenci hello zavedená.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-284">If you can install hello gateway on hello actual data source, this proximity minimizes hello latency introduced.</span></span> <span data-ttu-id="9d7d1-285">Zvažte příliš hello datových centrech.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-285">Consider hello datacenters too.</span></span> <span data-ttu-id="9d7d1-286">Například pokud používá službu hello západní USA datacenter a vy musíte SQL Server hostované ve virtuálním počítači Azure, svého virtuálního počítače Azure musí být v hello západní USA příliš.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-286">For example, if your service uses hello West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in hello West US too.</span></span> <span data-ttu-id="9d7d1-287">Tato blízkosti minimalizuje latenci a zabraňuje s nimi spojeným nákladům na hello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-287">This proximity minimizes latency and avoids egress charges on hello Azure VM.</span></span>

<span data-ttu-id="9d7d1-288">**Q**: jak jsou výsledky odeslána zpět toohello cloudu?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-288">**Q**: How are results sent back toohello cloud?</span></span> <br/><span data-ttu-id="9d7d1-289">
**A**: výsledky se odesílají přes hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-289">
**A**: Results are sent through hello Azure Service Bus.</span></span>

<span data-ttu-id="9d7d1-290">**Q**: existují jakékoli brány toohello příchozí připojení z cloudu hello?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-290">**Q**: Are there any inbound connections toohello gateway from hello cloud?</span></span> <br/><span data-ttu-id="9d7d1-291">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-291">
**A**: No.</span></span> <span data-ttu-id="9d7d1-292">Brána Hello používá tooAzure odchozí připojení služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-292">hello gateway uses outbound connections tooAzure Service Bus.</span></span>

<span data-ttu-id="9d7d1-293">**Q**: Co když blokovat odchozí připojení?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-293">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="9d7d1-294">Co dělat, je potřeba tooopen?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-294">What do I need tooopen?</span></span> <br/><span data-ttu-id="9d7d1-295">
**A**: najdete v části hello porty a hostitelů, které hello používá bránu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-295">
**A**: See hello ports and hosts that hello gateway uses.</span></span>

<span data-ttu-id="9d7d1-296">**Q**: co je skutečný služba systému Windows hello volána?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-296">**Q**: What is hello actual Windows service called?</span></span><br/><span data-ttu-id="9d7d1-297">
**A**: V rámci služby Services hello brána nazývá služba Power BI Enterprise Gateway.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-297">
**A**: In Services, hello gateway is called Power BI Enterprise Gateway Service.</span></span>

<span data-ttu-id="9d7d1-298">**Q**: můžete hello služba Windows Brána pro spuštění pomocí účtu Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-298">**Q**: Can hello gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="9d7d1-299">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-299">
**A**: No.</span></span> <span data-ttu-id="9d7d1-300">služba systému Windows Hello musí mít platný účet systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-300">hello Windows service must have a valid Windows account.</span></span> <span data-ttu-id="9d7d1-301">Ve výchozím nastavení spouští hello služby s hello SID služby NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-301">By default, hello service runs with hello Service SID, NT SERVICE\PBIEgwService.</span></span>

### <a name="high-availability-and-disaster-recovery"></a><span data-ttu-id="9d7d1-302">Vysoká dostupnost a zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="9d7d1-302">High availability and disaster recovery</span></span>

<span data-ttu-id="9d7d1-303">**Q**: jaké možnosti jsou dostupné pro zotavení po havárii?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-303">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="9d7d1-304">
**A**: můžete použít hello obnovení klíče toorestore nebo přesunout bránu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-304">
**A**: You can use hello recovery key toorestore or move a gateway.</span></span> <span data-ttu-id="9d7d1-305">Když instalujete hello brány, zadejte hello obnovovací klíč.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-305">When you install hello gateway, specify hello recovery key.</span></span>

<span data-ttu-id="9d7d1-306">**Q**: co je hello výhodou hello obnovovací klíč?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-306">**Q**: What is hello benefit of hello recovery key?</span></span> <br/><span data-ttu-id="9d7d1-307">
**A**: hello obnovovací klíč poskytuje způsob toomigrate nebo obnovení po havárii nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-307">
**A**: hello recovery key provides a way toomigrate or recover your gateway settings after a disaster.</span></span>

<span data-ttu-id="9d7d1-308">**Q**: existují všechny plány pro povolení scénáře s vysokou dostupností s bránou hello?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-308">**Q**: Are there any plans for enabling high availability scenarios with hello gateway?</span></span> <br/><span data-ttu-id="9d7d1-309">
**A**: tyto scénáře jsou na hello plán, ale ještě nemáme časové osy.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-309">
**A**: These scenarios are on hello roadmap, but we don't have a timeline yet.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9d7d1-310">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="9d7d1-310">Troubleshooting</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

<span data-ttu-id="9d7d1-311">**Q**: Jak můžu zjistit, jaké dotazy jsou odesílány zdroj dat pro místní toohello?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-311">**Q**: How can I see what queries are being sent toohello on-premises data source?</span></span> <br/><span data-ttu-id="9d7d1-312">
**A**: můžete povolit trasování dotazů, což zahrnuje hello dotazy, které se odesílají.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-312">
**A**: You can enable query tracing, which includes hello queries that are sent.</span></span> <span data-ttu-id="9d7d1-313">Mějte na paměti, toochange dotazu trasování zpět toohello původní hodnotu po dokončení odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-313">Remember toochange query tracing back toohello original value when done troubleshooting.</span></span> <span data-ttu-id="9d7d1-314">Trasování dotazů, které jsou zapnuté vytvoří větší protokoly.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-314">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="9d7d1-315">Můžete také zobrazit nástroje, které má váš zdroj dat pro trasování dotazů.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-315">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="9d7d1-316">Můžete například použít rozšířených událostí nebo profileru SQL pro SQL Server a služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-316">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="9d7d1-317">**Q**: kde jsou protokoly hello gateway?</span><span class="sxs-lookup"><span data-stu-id="9d7d1-317">**Q**: Where are hello gateway logs?</span></span> <br/><span data-ttu-id="9d7d1-318">
**A**: viz nástroje později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-318">
**A**: See Tools later in this topic.</span></span>

### <a name="update-toohello-latest-version"></a><span data-ttu-id="9d7d1-319">Aktualizace toohello nejnovější verzi</span><span class="sxs-lookup"><span data-stu-id="9d7d1-319">Update toohello latest version</span></span>

<span data-ttu-id="9d7d1-320">Mnohé problémy můžete surface při verze brány hello stanou zastaralými.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-320">Many issues can surface when hello gateway version becomes outdated.</span></span> <span data-ttu-id="9d7d1-321">Jako vhodné obecné Ujistěte se, že používáte nejnovější verzi hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-321">As good general practice, make sure that you use hello latest version.</span></span> <span data-ttu-id="9d7d1-322">Pokud jste neprovedli aktualizaci brány hello dobu jednoho měsíce nebo déle, můžete zvažte instalaci hello nejnovější verzi této brány hello a v tématu, pokud jste reprodukujte problém hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-322">If you haven't updated hello gateway for a month or longer, you might consider installing hello latest version of hello gateway, and see if you can reproduce hello issue.</span></span>

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="9d7d1-323">Chyba: Nepodařilo toogroup tooadd uživatele.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-323">Error: Failed tooadd user toogroup.</span></span> <span data-ttu-id="9d7d1-324">(-2147463168 PBIEgwService výkonu protokolu uživatelů)</span><span class="sxs-lookup"><span data-stu-id="9d7d1-324">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="9d7d1-325">Může se tato chyba, pokud se pokusíte tooinstall hello brány na řadiči domény, což není podporováno.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-325">You might get this error if you try tooinstall hello gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="9d7d1-326">Ujistěte se, že nasazujete hello brána na počítači, který není řadičem domény.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-326">Make sure that you deploy hello gateway on a machine that isn't a domain controller.</span></span>

## <a name="tools"></a><span data-ttu-id="9d7d1-327">Nástroje</span><span class="sxs-lookup"><span data-stu-id="9d7d1-327">Tools</span></span>

### <a name="collect-logs-from-hello-gateway-configurer"></a><span data-ttu-id="9d7d1-328">Shromažďování protokolů z brány configurer hello</span><span class="sxs-lookup"><span data-stu-id="9d7d1-328">Collect logs from hello gateway configurer</span></span>

<span data-ttu-id="9d7d1-329">Můžete shromáždit několik protokolů pro bránu hello.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-329">You can collect several logs for hello gateway.</span></span> <span data-ttu-id="9d7d1-330">Vždy začínejte hello protokoly!</span><span class="sxs-lookup"><span data-stu-id="9d7d1-330">Always start with hello logs!</span></span>

#### <a name="installer-logs"></a><span data-ttu-id="9d7d1-331">Instalační protokoly</span><span class="sxs-lookup"><span data-stu-id="9d7d1-331">Installer logs</span></span>

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a><span data-ttu-id="9d7d1-332">Konfigurace protokolů</span><span class="sxs-lookup"><span data-stu-id="9d7d1-332">Configuration logs</span></span>

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="9d7d1-333">Protokoly služby Enterprise gateway</span><span class="sxs-lookup"><span data-stu-id="9d7d1-333">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a><span data-ttu-id="9d7d1-334">Protokoly událostí</span><span class="sxs-lookup"><span data-stu-id="9d7d1-334">Event logs</span></span>

<span data-ttu-id="9d7d1-335">Můžete najít hello protokoly Brána pro správu dat a PowerBIGateway pod **protokoly aplikací a služeb**.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-335">You can find hello Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>

### <a name="fiddler-trace"></a><span data-ttu-id="9d7d1-336">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="9d7d1-336">Fiddler Trace</span></span>

<span data-ttu-id="9d7d1-337">[Fiddler](http://www.telerik.com/fiddler) je bezplatný nástroj na webu Telerik, který monitoruje provoz protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-337">[Fiddler](http://www.telerik.com/fiddler) is a free tool from Telerik that monitors HTTP traffic.</span></span> <span data-ttu-id="9d7d1-338">Zobrazí se tyto přenosy s hello služby Power BI z hello klientský počítač.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-338">You can see this traffic with hello Power BI service from hello client machine.</span></span> <span data-ttu-id="9d7d1-339">Tato služba může zobrazit chyby a další související informace.</span><span class="sxs-lookup"><span data-stu-id="9d7d1-339">This service might show errors and other related information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d7d1-340">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d7d1-340">Next steps</span></span>
    
* [<span data-ttu-id="9d7d1-341">Připojení místní tooon dat z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="9d7d1-341">Connect tooon-premises data from logic apps</span></span>](../logic-apps/logic-apps-gateway-connection.md)
* [<span data-ttu-id="9d7d1-342">Funkce Integrace organizace</span><span class="sxs-lookup"><span data-stu-id="9d7d1-342">Enterprise integration features</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="9d7d1-343">Konektory pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="9d7d1-343">Connectors for Azure Logic Apps</span></span>](../connectors/apis-list.md)
