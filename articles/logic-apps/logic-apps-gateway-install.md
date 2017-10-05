---
title: "Nainstalovat bránu dat místní - Azure Logic Apps | Microsoft Docs"
description: "Než budete přistupovat ke zdrojům dat místně, nainstalujte bránu místní dat pro přenos dat rychlý a šifrování mezi zdrojů dat na místní a aplikacích logiky"
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
ms.openlocfilehash: 34e68ae7d35019848b35c785a2715ec458dc6e73
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="install-the-on-premises-data-gateway-for-azure-logic-apps"></a><span data-ttu-id="b2a35-104">Nainstalujte bránu dat na místě pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="b2a35-104">Install the on-premises data gateway for Azure Logic Apps</span></span>

<span data-ttu-id="b2a35-105">Předtím, než aplikace logiky můžete přistupovat ke zdrojům dat místní, musíte nainstalovat a nastavit bránu dat na místě.</span><span class="sxs-lookup"><span data-stu-id="b2a35-105">Before your logic apps can access data sources on premises, you must install and set up the on-premises data gateway.</span></span> <span data-ttu-id="b2a35-106">Brána funguje jako mostu, který poskytuje přenos rychlé dat a šifrování mezi místními systémy a aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="b2a35-106">The gateway acts as a bridge that provides quick data transfer and encryption between on-premises systems and your logic apps.</span></span> <span data-ttu-id="b2a35-107">Brána předává data z místního zdroje na šifrované kanály přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b2a35-107">The gateway relays data from on-premises sources on encrypted channels through the Azure Service Bus.</span></span> <span data-ttu-id="b2a35-108">Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-108">All traffic originates as secure outbound traffic from the gateway agent.</span></span> <span data-ttu-id="b2a35-109">Další informace o [fungování bránu dat](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="b2a35-109">Learn more about [how the data gateway works](#gateway-cloud-service).</span></span>

<span data-ttu-id="b2a35-110">Brána podporuje připojení k těmto zdrojům dat místně:</span><span class="sxs-lookup"><span data-stu-id="b2a35-110">The gateway supports connections to these data sources on premises:</span></span>

*   <span data-ttu-id="b2a35-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="b2a35-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="b2a35-112">DB2</span><span class="sxs-lookup"><span data-stu-id="b2a35-112">DB2</span></span>  
*   <span data-ttu-id="b2a35-113">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="b2a35-113">File System</span></span>
*   <span data-ttu-id="b2a35-114">Informix</span><span class="sxs-lookup"><span data-stu-id="b2a35-114">Informix</span></span>
*   <span data-ttu-id="b2a35-115">MQ</span><span class="sxs-lookup"><span data-stu-id="b2a35-115">MQ</span></span>
*   <span data-ttu-id="b2a35-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="b2a35-116">MySQL</span></span>
*   <span data-ttu-id="b2a35-117">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="b2a35-117">Oracle Database</span></span>
*   <span data-ttu-id="b2a35-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b2a35-118">PostgreSQL</span></span>
*   <span data-ttu-id="b2a35-119">Aplikace serveru SAP</span><span class="sxs-lookup"><span data-stu-id="b2a35-119">SAP Application Server</span></span> 
*   <span data-ttu-id="b2a35-120">Zpráva serveru SAP</span><span class="sxs-lookup"><span data-stu-id="b2a35-120">SAP Message Server</span></span>
*   <span data-ttu-id="b2a35-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="b2a35-121">SharePoint</span></span>
*   <span data-ttu-id="b2a35-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b2a35-122">SQL Server</span></span>
*   <span data-ttu-id="b2a35-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="b2a35-123">Teradata</span></span>

<span data-ttu-id="b2a35-124">Tyto kroky ukazují, jak nejprve nainstalovat bránu dat na místě před [nastavit připojení mezi bránou rozhraní a aplikace logiky](./logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="b2a35-124">These steps show how to first install the on-premises data gateway before you [set up a connection between the gateway and your logic apps](./logic-apps-gateway-connection.md).</span></span> <span data-ttu-id="b2a35-125">Další informace o podporované konektory najdete v tématu [konektory pro Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).</span><span class="sxs-lookup"><span data-stu-id="b2a35-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).</span></span> 

<span data-ttu-id="b2a35-126">Informace o tom, jak používat bránu s jinými službami, najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="b2a35-126">For information about how to use the gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="b2a35-127">Microsoft Power BI místní brány dat</span><span class="sxs-lookup"><span data-stu-id="b2a35-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="b2a35-128">Služba Azure gateway místní dat služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="b2a35-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="b2a35-129">Microsoft Flow místní brány dat</span><span class="sxs-lookup"><span data-stu-id="b2a35-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="b2a35-130">Microsoft PowerApps místní brány dat</span><span class="sxs-lookup"><span data-stu-id="b2a35-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>
## <a name="requirements"></a><span data-ttu-id="b2a35-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b2a35-131">Requirements</span></span>

<span data-ttu-id="b2a35-132">**Minimální**:</span><span class="sxs-lookup"><span data-stu-id="b2a35-132">**Minimum**:</span></span>

* <span data-ttu-id="b2a35-133">Rozhraní .NET 4.5 framework</span><span class="sxs-lookup"><span data-stu-id="b2a35-133">.NET 4.5 Framework</span></span>
* <span data-ttu-id="b2a35-134">64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="b2a35-134">64-bit version of Windows 7 or Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="b2a35-135">**Doporučená**:</span><span class="sxs-lookup"><span data-stu-id="b2a35-135">**Recommended**:</span></span>

* <span data-ttu-id="b2a35-136">8 jader procesoru</span><span class="sxs-lookup"><span data-stu-id="b2a35-136">8 Core CPU</span></span>
* <span data-ttu-id="b2a35-137">8 GB paměti</span><span class="sxs-lookup"><span data-stu-id="b2a35-137">8 GB Memory</span></span>
* <span data-ttu-id="b2a35-138">64bitová verze systému Windows 2012 R2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="b2a35-138">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="b2a35-139">**Je důležité zvážit**:</span><span class="sxs-lookup"><span data-stu-id="b2a35-139">**Important considerations**:</span></span>

* <span data-ttu-id="b2a35-140">Nainstalujte bránu dat místně pouze v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="b2a35-140">Install the on-premises data gateway only on a local computer.</span></span>
<span data-ttu-id="b2a35-141">Nelze nainstalovat bránu na řadiči domény.</span><span class="sxs-lookup"><span data-stu-id="b2a35-141">You can't install the gateway on a domain controller.</span></span>

   > [!TIP]
   > <span data-ttu-id="b2a35-142">Nemáte bránu nainstalovat na stejném počítači jako zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="b2a35-142">You don't have to install the gateway on the same computer as your data source.</span></span> <span data-ttu-id="b2a35-143">Pro zajištění minimální latence, můžete nainstalovat bránu jak nejblíže ke zdroji dat, nebo na stejném počítači, za předpokladu, že máte oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b2a35-143">To minimize latency, you can install the gateway as close as possible to your data source, or on the same computer, assuming that you have permissions.</span></span>

* <span data-ttu-id="b2a35-144">Neinstalujte bránu v počítači, který vypne, přejde do režimu spánku, nebo nemá připojení k Internetu, protože v těchto případech nelze spustit bránu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-144">Don't install the gateway on a computer that turns off, goes to sleep, or doesn't connect to the Internet because the gateway can't run under those circumstances.</span></span> <span data-ttu-id="b2a35-145">Navíc může sníží výkon brány přes bezdrátové sítě.</span><span class="sxs-lookup"><span data-stu-id="b2a35-145">Also, gateway performance might suffer over a wireless network.</span></span>

* <span data-ttu-id="b2a35-146">Během instalace, musíte se odhlásit se [pracovní nebo školní účet](https://docs.microsoft.com/azure/active-directory/sign-up-organization) , který je spravován službou Azure Active Directory (Azure AD), nikoli účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b2a35-146">During installation, you must sign in with a [work or school account](https://docs.microsoft.com/azure/active-directory/sign-up-organization) that's managed by Azure Active Directory (Azure AD), not a Microsoft account.</span></span> 

  <span data-ttu-id="b2a35-147">Musíte použít stejný pracovní nebo školní účet později na portálu Azure při vytváření a přidružte prostředek brány k vaší instalace brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-147">You have to use the same work or school account later in the Azure portal when you create and associate a gateway resource with your gateway installation.</span></span> <span data-ttu-id="b2a35-148">Když vytvoříte připojení mezi svou aplikaci logiky a místní zdroj dat pak vyberete tento prostředek brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-148">You then select this gateway resource when you create the connection between your logic app and the on-premises data source.</span></span> [<span data-ttu-id="b2a35-149">Proč musí používat Azure AD pracovní nebo školní účet?</span><span class="sxs-lookup"><span data-stu-id="b2a35-149">Why must I use an Azure AD work or school account?</span></span>](#why-azure-work-school-account)

  > [!TIP]
  > <span data-ttu-id="b2a35-150">Pokud jste se zaregistrovali do nabídky služeb Office 365 a nedodal e-mailu samotnou práci, může vypadat přihlašovací adresa jeff@contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="b2a35-150">If you signed up for an Office 365 offering and didn't supply your actual work email, your sign-in address might look like jeff@contoso.onmicrosoft.com.</span></span> 

* <span data-ttu-id="b2a35-151">Pokud máte existující bránu, kterou jste vytvořili pomocí Instalační program, který je starší než verze 14.16.6317.4, nelze změnit umístění vaší brány spuštěním nejnovější verzi Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="b2a35-151">If you have an existing gateway that you set up with an installer that's earlier than version 14.16.6317.4, you can't change your gateway's location by running the latest installer.</span></span> <span data-ttu-id="b2a35-152">Můžete však použít nejnovější verzi instalačního programu nastavit novou bránu s umístění, které chcete místo toho.</span><span class="sxs-lookup"><span data-stu-id="b2a35-152">However, you can use the latest installer to set up a new gateway with the location that you want instead.</span></span>
  
  <span data-ttu-id="b2a35-153">Pokud máte bránu instalační program, který je starší než verze 14.16.6317.4, ale nemáte nainstalovanou bránu ještě, můžete stáhnout a použít nejnovější verzi Instalační služby.</span><span class="sxs-lookup"><span data-stu-id="b2a35-153">If you have a gateway installer that's earlier than version 14.16.6317.4, but you haven't installed your gateway yet, you can download and use the latest installer.</span></span>

<a name="install-gateway"></a>

## <a name="install-the-data-gateway"></a><span data-ttu-id="b2a35-154">Nainstalujte bránu dat</span><span class="sxs-lookup"><span data-stu-id="b2a35-154">Install the data gateway</span></span>

1.  <span data-ttu-id="b2a35-155">[Stáhněte a spusťte instalační program brány na místním počítači](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="b2a35-155">[Download and run the gateway installer on a local computer](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span></span>

2. <span data-ttu-id="b2a35-156">Přečtěte si a přijměte podmínky použití a ochrana osobních údajů příkaz.</span><span class="sxs-lookup"><span data-stu-id="b2a35-156">Review and accept the terms of use and privacy statement.</span></span>

3. <span data-ttu-id="b2a35-157">Zadejte cestu v místním počítači, kam chcete bránu nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="b2a35-157">Specify the path on your local computer where you want to install the gateway.</span></span>

4. <span data-ttu-id="b2a35-158">Pokud budete vyzváni, přihlaste se pomocí vaší Azure pracovního nebo školního účtu, nikoli účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b2a35-158">When prompted, sign in with your Azure work or school account, not a Microsoft account.</span></span>

   ![Přihlaste se pomocí Azure pracovní nebo školní účet](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. <span data-ttu-id="b2a35-160">Nyní zaregistrovat bránu nainstalované s [cloudové službě Brána pro](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="b2a35-160">Now register your installed gateway with the [gateway cloud service](#gateway-cloud-service).</span></span> <span data-ttu-id="b2a35-161">Zvolte **registrace nové brány na tomto počítači**.</span><span class="sxs-lookup"><span data-stu-id="b2a35-161">Choose **Register a new gateway on this computer**.</span></span>

   <span data-ttu-id="b2a35-162">Cloudové služby brány šifruje a ukládá pověření ke zdroji dat a podrobnosti brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-162">The gateway cloud service encrypts and stores your data source credentials and gateway details.</span></span> 
   <span data-ttu-id="b2a35-163">Služba také směrování dotazy a jejich výsledky mezi svou aplikaci logiky, místní brána dat a zdroj dat místně.</span><span class="sxs-lookup"><span data-stu-id="b2a35-163">The service also routes queries and their results between your logic app, the on-premises data gateway, and your data source on premises.</span></span>

6. <span data-ttu-id="b2a35-164">Zadejte název pro instalaci brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-164">Provide a name for your gateway installation.</span></span> <span data-ttu-id="b2a35-165">Vytvořit klíče pro obnovení a pak potvrďte obnovovací klíč.</span><span class="sxs-lookup"><span data-stu-id="b2a35-165">Create a recovery key, then confirm your recovery key.</span></span> 

   > [!IMPORTANT] 
   > <span data-ttu-id="b2a35-166">Obnovovací klíč musí obsahovat alespoň osm znaků.</span><span class="sxs-lookup"><span data-stu-id="b2a35-166">Your recovery key must contain at least eight characters.</span></span> <span data-ttu-id="b2a35-167">Zajistěte, aby si uložit a zachovat klíč na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="b2a35-167">Make sure that you save and keep the key in a safe place.</span></span> <span data-ttu-id="b2a35-168">Tento klíč je také nutné, pokud chcete migrovat, obnovení nebo převzetí existující brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-168">You also need this key when you want to migrate, restore, or take over an existing gateway.</span></span>

   1. <span data-ttu-id="b2a35-169">Chcete-li změnit výchozí oblast pro cloudové službě Brána a používané při instalaci vaší brány Azure Service Bus, zvolte **změnit oblast**.</span><span class="sxs-lookup"><span data-stu-id="b2a35-169">To change the default region for the gateway cloud service and Azure Service Bus used by your gateway installation, choose **Change Region**.</span></span>

      ![Oblast změny](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      <span data-ttu-id="b2a35-171">Výchozí oblast je oblast přidruženou klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b2a35-171">The default region is the region associated with your Azure AD tenant.</span></span>

   2. <span data-ttu-id="b2a35-172">V podokně Další otevřete **vyberte oblast** vybrat jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="b2a35-172">On the next pane, open the **Select Region** to choose a different region.</span></span>

      ![Vyberte jinou oblast](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      <span data-ttu-id="b2a35-174">Například můžete vybrat stejné oblasti jako svou aplikaci logiky nebo vyberte oblast, která je nejblíže k místní zdroje dat, můžete snížit latenci.</span><span class="sxs-lookup"><span data-stu-id="b2a35-174">For example, you might select the same region as your logic app, or select the region closest to your on-premises data source so you can reduce latency.</span></span> <span data-ttu-id="b2a35-175">Brána prostředků a logiku aplikace může mít jiné umístění.</span><span class="sxs-lookup"><span data-stu-id="b2a35-175">Your gateway resource and logic app can have different locations.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="b2a35-176">Tato oblast nelze změnit po instalaci.</span><span class="sxs-lookup"><span data-stu-id="b2a35-176">You can't change this region after installation.</span></span> <span data-ttu-id="b2a35-177">Tato oblast také určuje a omezuje umístění, kde si můžete vytvořit prostředků Azure pro bránu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-177">This region also determines and restricts the location where you can create the Azure resource for your gateway.</span></span> <span data-ttu-id="b2a35-178">Když vytvoříte prostředek brány v Azure, proto ujistěte, že umístění prostředků odpovídá oblast, kterou jste vybrali v průběhu instalace brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-178">So when you create your gateway resource in Azure, make sure that the resource location matches the region that you selected during gateway installation.</span></span>
      > 
      > <span data-ttu-id="b2a35-179">Pokud chcete použít v jiné oblasti pro bránu později, musíte nastavit novou bránu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-179">If you want to use a different region for your gateway later, you must set up a new gateway.</span></span>

   3. <span data-ttu-id="b2a35-180">Až budete připraveni, zvolte **provádí**.</span><span class="sxs-lookup"><span data-stu-id="b2a35-180">When you're ready, choose **Done**.</span></span>

7. <span data-ttu-id="b2a35-181">Teď můžete pomocí těchto kroků na portálu Azure, abyste mohli [vytvořit prostředek služby Azure pro bránu](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="b2a35-181">Now follow these steps in the Azure portal so you can [create an Azure resource for your gateway](../logic-apps/logic-apps-gateway-connection.md).</span></span> 

<span data-ttu-id="b2a35-182">Další informace o [fungování bránu dat](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="b2a35-182">Learn more about [how the data gateway works](#gateway-cloud-service).</span></span>

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a><span data-ttu-id="b2a35-183">Migrace, obnovení nebo převzetí existující brány</span><span class="sxs-lookup"><span data-stu-id="b2a35-183">Migrate, restore, or take over an existing gateway</span></span>

<span data-ttu-id="b2a35-184">K provedení těchto úloh, musí mít obnovovací klíč, který byl zadán při instalaci brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-184">To perform these tasks, you must have the recovery key that was specified when the gateway was installed.</span></span>

1. <span data-ttu-id="b2a35-185">Z nabídky Start v počítači, zvolte **místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="b2a35-185">From your computer's Start menu, choose **On-premises data gateway**.</span></span>

2. <span data-ttu-id="b2a35-186">Po otevření instalační program, přihlaste se pomocí Azure stejný pracovní nebo školní účet, který byl dříve používán se nainstalovat bránu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-186">After the installer opens, sign in with the same Azure work or school account that was previously used to install the gateway.</span></span>

3. <span data-ttu-id="b2a35-187">Zvolte **migrace, obnovení nebo převzetí existující brány**.</span><span class="sxs-lookup"><span data-stu-id="b2a35-187">Choose **Migrate, restore, or take over an existing gateway**.</span></span>

4. <span data-ttu-id="b2a35-188">Zadejte název a obnovení klíče pro bránu, kterou chcete migrovat, obnovení nebo převzít.</span><span class="sxs-lookup"><span data-stu-id="b2a35-188">Provide the name and recovery key for the gateway that you want to migrate, restore, or take over.</span></span>

<a name="restart-gateway"></a>
## <a name="restart-the-gateway"></a><span data-ttu-id="b2a35-189">Restartujte bránu</span><span class="sxs-lookup"><span data-stu-id="b2a35-189">Restart the gateway</span></span>

<span data-ttu-id="b2a35-190">Brána používá jako služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="b2a35-190">The gateway runs as a Windows service.</span></span> <span data-ttu-id="b2a35-191">Stejně jako ostatní služby systému Windows můžete spustit a zastavit službu několika způsoby.</span><span class="sxs-lookup"><span data-stu-id="b2a35-191">Like any other Windows service, you can start and stop the service in multiple ways.</span></span> <span data-ttu-id="b2a35-192">Můžete například otevřete příkazový řádek se zvýšenými oprávněními v počítači se spuštěným systémem brány a spusťte buď tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="b2a35-192">For example, you can open a command prompt with elevated permissions on the computer where the gateway is running, and run either these commands:</span></span>

* <span data-ttu-id="b2a35-193">Chcete-li zastavit službu, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b2a35-193">To stop the service, run this command:</span></span>
  
    `net stop PBIEgwService`

* <span data-ttu-id="b2a35-194">Chcete-li spustit službu, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b2a35-194">To start the service, run this command:</span></span>
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a><span data-ttu-id="b2a35-195">Účet služby systému Windows</span><span class="sxs-lookup"><span data-stu-id="b2a35-195">Windows service account</span></span>

<span data-ttu-id="b2a35-196">Místní brána dat je nastavit tak, aby použít `NT SERVICE\PBIEgwService` Windows služby přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="b2a35-196">The on-premises data gateway is set up to use `NT SERVICE\PBIEgwService` for the Windows service logon credentials.</span></span> <span data-ttu-id="b2a35-197">Ve výchozím nastavení brány má právo "Přihlásit jako službu" pro počítač, kde instalujete bránu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-197">By default, the gateway has the "Log on as a service" right for the machine where you install the gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="b2a35-198">Účet služby systému Windows se liší od účtu použité pro připojování k místní datové zdroje a z Azure pracovní nebo školní účet použitý k přihlášení do cloudových služeb:</span><span class="sxs-lookup"><span data-stu-id="b2a35-198">The Windows service account differs from the account used for connecting to on-premises data sources, and from the Azure work or school account used to sign in to cloud services.</span></span>

## <a name="configure-a-firewall-or-proxy"></a><span data-ttu-id="b2a35-199">Konfigurace brány firewall nebo proxy server</span><span class="sxs-lookup"><span data-stu-id="b2a35-199">Configure a firewall or proxy</span></span>

<span data-ttu-id="b2a35-200">Vytvoří odchozí připojení k bráně [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="b2a35-200">The gateway creates an outbound connection to [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="b2a35-201">Pokud chcete zadat informace o proxy serveru pro bránu, najdete v části [konfigurace nastavení proxy serveru](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span><span class="sxs-lookup"><span data-stu-id="b2a35-201">To provide proxy information for your gateway, see [Configure proxy settings](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span></span>

<span data-ttu-id="b2a35-202">Pokud chcete zkontrolovat, zda brána firewall nebo proxy, může blokovat připojení, zkontrolujte, jestli váš počítač ve skutečnosti připojit k Internetu a [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="b2a35-202">To check whether your firewall, or proxy, might block connections, confirm whether your machine can actually connect to the internet and the [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="b2a35-203">Z řádku prostředí PowerShell spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b2a35-203">From a PowerShell prompt, run this command:</span></span>

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> <span data-ttu-id="b2a35-204">Tento příkaz pouze Otestuje připojení k síti a připojení k Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b2a35-204">This command only tests network connectivity and connectivity to the Azure Service Bus.</span></span> <span data-ttu-id="b2a35-205">Příkaz proto nemá nic dělat s bránu nebo cloudové službě brány, který šifruje a ukládá přihlašovací údaje a podrobnosti brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-205">So the command doesn't have anything to do with the gateway or the gateway cloud service that encrypts and stores your credentials and gateway details.</span></span> 
>
> <span data-ttu-id="b2a35-206">Tento příkaz je taky pouze k dispozici v systému Windows Server 2012 R2 nebo novější a Windows 8.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b2a35-206">Also, this command is only available on Windows Server 2012 R2 or later, and Windows 8.1 or later.</span></span> <span data-ttu-id="b2a35-207">V dřívějších verzích operačního systému můžete k testování připojení služby Telnet.</span><span class="sxs-lookup"><span data-stu-id="b2a35-207">On earlier OS versions, you can use Telnet to test connectivity.</span></span> <span data-ttu-id="b2a35-208">Další informace o [Azure Service Bus a hybridní řešení](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="b2a35-208">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

<span data-ttu-id="b2a35-209">Výsledky by měl vypadat podobně jako tento příklad:</span><span class="sxs-lookup"><span data-stu-id="b2a35-209">Your results should look similar to this example:</span></span>

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

<span data-ttu-id="b2a35-210">Pokud **TcpTestSucceeded** není nastavený na **True**, vám může blokována bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="b2a35-210">If **TcpTestSucceeded** is not set to **True**, you might be blocked by a firewall.</span></span> <span data-ttu-id="b2a35-211">Pokud chcete být komplexní, nahraďte **ComputerName** a **Port** a hodnoty uvedené v části [konfigurace portů](#configure-ports) v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-211">If you want to be comprehensive, substitute the **ComputerName** and **Port** values with the values listed under [Configure ports](#configure-ports) in this topic.</span></span>

<span data-ttu-id="b2a35-212">Brána firewall, mohou také blokovat připojení, které provádí Azure Service Bus v datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="b2a35-212">The firewall might also block connections that the Azure Service Bus makes to the Azure datacenters.</span></span> <span data-ttu-id="b2a35-213">Pokud tento scénář se stane, schválit (odblokovat) všechny IP adresy pro těchto datových center ve vašem regionu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-213">If this scenario happens, approve (unblock) all the IP addresses for those datacenters in your region.</span></span> <span data-ttu-id="b2a35-214">Pro tyto IP adresy [získat seznam adres Azure IP zde](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="b2a35-214">For those IP addresses, [get the Azure IP addresses list here](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

## <a name="configure-ports"></a><span data-ttu-id="b2a35-215">Konfigurace portů</span><span class="sxs-lookup"><span data-stu-id="b2a35-215">Configure ports</span></span>

<span data-ttu-id="b2a35-216">Vytvoří odchozí připojení k bráně [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) a komunikuje na odchozí porty: TCP 443 (výchozí), 5671, 5672, 9350 prostřednictvím 9354.</span><span class="sxs-lookup"><span data-stu-id="b2a35-216">The gateway creates an outbound connection to [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) and communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span> <span data-ttu-id="b2a35-217">Brána nevyžaduje příchozí porty.</span><span class="sxs-lookup"><span data-stu-id="b2a35-217">The gateway doesn't require inbound ports.</span></span> <span data-ttu-id="b2a35-218">Další informace o [Azure Service Bus a hybridní řešení](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="b2a35-218">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

| <span data-ttu-id="b2a35-219">NÁZVY DOMÉN</span><span class="sxs-lookup"><span data-stu-id="b2a35-219">DOMAIN NAMES</span></span> | <span data-ttu-id="b2a35-220">ODCHOZÍ PORTY</span><span class="sxs-lookup"><span data-stu-id="b2a35-220">OUTBOUND PORTS</span></span> | <span data-ttu-id="b2a35-221">POPIS</span><span class="sxs-lookup"><span data-stu-id="b2a35-221">DESCRIPTION</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b2a35-222">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="b2a35-222">*.analysis.windows.net</span></span> | <span data-ttu-id="b2a35-223">443</span><span class="sxs-lookup"><span data-stu-id="b2a35-223">443</span></span> | <span data-ttu-id="b2a35-224">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b2a35-224">HTTPS</span></span> | 
| <span data-ttu-id="b2a35-225">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="b2a35-225">*.login.windows.net</span></span> | <span data-ttu-id="b2a35-226">443</span><span class="sxs-lookup"><span data-stu-id="b2a35-226">443</span></span> | <span data-ttu-id="b2a35-227">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b2a35-227">HTTPS</span></span> | 
| <span data-ttu-id="b2a35-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="b2a35-228">*.servicebus.windows.net</span></span> | <span data-ttu-id="b2a35-229">5671-5672</span><span class="sxs-lookup"><span data-stu-id="b2a35-229">5671-5672</span></span> | <span data-ttu-id="b2a35-230">Pokročilé zpráv služby Řízení front Protocol (AMQP)</span><span class="sxs-lookup"><span data-stu-id="b2a35-230">Advanced Message Queuing Protocol (AMQP)</span></span> | 
| <span data-ttu-id="b2a35-231">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="b2a35-231">*.servicebus.windows.net</span></span> | <span data-ttu-id="b2a35-232">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="b2a35-232">443, 9350-9354</span></span> | <span data-ttu-id="b2a35-233">Moduly pro naslouchání na předávání přes Service Bus přes TCP (vyžaduje 443 pro získání tokenu řízení přístupu)</span><span class="sxs-lookup"><span data-stu-id="b2a35-233">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> | 
| <span data-ttu-id="b2a35-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="b2a35-234">*.frontend.clouddatahub.net</span></span> | <span data-ttu-id="b2a35-235">443</span><span class="sxs-lookup"><span data-stu-id="b2a35-235">443</span></span> | <span data-ttu-id="b2a35-236">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b2a35-236">HTTPS</span></span> | 
| <span data-ttu-id="b2a35-237">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="b2a35-237">*.core.windows.net</span></span> | <span data-ttu-id="b2a35-238">443</span><span class="sxs-lookup"><span data-stu-id="b2a35-238">443</span></span> | <span data-ttu-id="b2a35-239">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b2a35-239">HTTPS</span></span> | 
| <span data-ttu-id="b2a35-240">Login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="b2a35-240">login.microsoftonline.com</span></span> | <span data-ttu-id="b2a35-241">443</span><span class="sxs-lookup"><span data-stu-id="b2a35-241">443</span></span> | <span data-ttu-id="b2a35-242">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b2a35-242">HTTPS</span></span> | 
| <span data-ttu-id="b2a35-243">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="b2a35-243">*.msftncsi.com</span></span> | <span data-ttu-id="b2a35-244">443</span><span class="sxs-lookup"><span data-stu-id="b2a35-244">443</span></span> | <span data-ttu-id="b2a35-245">Použít k testování připojení k Internetu, když brána nedostupná pro službu Power BI.</span><span class="sxs-lookup"><span data-stu-id="b2a35-245">Used to test internet connectivity when the gateway is unreachable by the Power BI service.</span></span> | 

<span data-ttu-id="b2a35-246">Pokud budete muset schválit IP adresy místo domény, můžete stáhnout a použít [rozsahy IP Datacentra Azure Microsoft seznamu](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="b2a35-246">If you have to approve IP addresses instead of the domains, you can download and use the [Microsoft Azure Datacenter IP ranges list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="b2a35-247">V některých případech jsou vytvářeny připojení Azure Service Bus s IP adresou, nikoli plně kvalifikované názvy domény.</span><span class="sxs-lookup"><span data-stu-id="b2a35-247">In some cases, the Azure Service Bus connections are made with IP Address rather than fully qualified domain names.</span></span>

<a name="gateway-cloud-service"></a>
## <a name="how-does-the-data-gateway-work"></a><span data-ttu-id="b2a35-248">Jak funguje bránu dat?</span><span class="sxs-lookup"><span data-stu-id="b2a35-248">How does the data gateway work?</span></span>

<span data-ttu-id="b2a35-249">Bránu dat umožňuje rychle a bezpečně komunikaci mezi svou aplikaci logiky, cloudové službě brány a zdroje dat na místě.</span><span class="sxs-lookup"><span data-stu-id="b2a35-249">The data gateway facilitates quick and secure communication between your logic app, the gateway cloud service, and your on-premises data source.</span></span> 

![Diagram-for-On-Premises-data-Gateway-Flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

<span data-ttu-id="b2a35-251">Takže když uživatel v cloudu komunikuje element, který je připojen k místní zdroj dat:</span><span class="sxs-lookup"><span data-stu-id="b2a35-251">So when the user in the cloud interacts with an element that's connected to your on-premises data source:</span></span>

1. <span data-ttu-id="b2a35-252">Cloudové služby brány vytvoří dotaz, společně s zašifrované přihlašovací údaje pro zdroje dat a odešle dotaz do fronty pro bránu ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="b2a35-252">The gateway cloud service creates a query, along with the encrypted credentials for the data source, and sends the query to the queue for the gateway to process.</span></span>

2. <span data-ttu-id="b2a35-253">Cloudové služby brány analyzuje dotaz a nabízených oznámení požadavek na Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b2a35-253">The gateway cloud service analyzes the query and pushes the request to the Azure Service Bus.</span></span>

3. <span data-ttu-id="b2a35-254">Bránu dat místní dotáže Azure Service Bus pro žádosti čekající na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="b2a35-254">The on-premises data gateway polls the Azure Service Bus for pending requests.</span></span>

4. <span data-ttu-id="b2a35-255">Brána získá dotaz, dešifruje přihlašovací údaje a připojí se ke zdroji dat pomocí těchto přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b2a35-255">The gateway gets the query, decrypts the credentials, and connects to the data source with those credentials.</span></span>

5. <span data-ttu-id="b2a35-256">Brána odešle dotaz na zdroj dat pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="b2a35-256">The gateway sends the query to the data source for execution.</span></span>

6. <span data-ttu-id="b2a35-257">Výsledky jsou odesílány ze zdroje dat zpět k bráně a ke cloudové službě brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-257">The results are sent from the data source, back to the gateway, and then to the gateway cloud service.</span></span> <span data-ttu-id="b2a35-258">Cloudové služby Brána pak použije výsledky.</span><span class="sxs-lookup"><span data-stu-id="b2a35-258">The gateway cloud service then uses the results.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="b2a35-259">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="b2a35-259">Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="b2a35-260">Obecné</span><span class="sxs-lookup"><span data-stu-id="b2a35-260">General</span></span>

<span data-ttu-id="b2a35-261">**Q**: Potřebuji bránu pro zdroje dat v cloudu, jako je například SQL Azure?</span><span class="sxs-lookup"><span data-stu-id="b2a35-261">**Q**: Do I need a gateway for data sources in the cloud, such as SQL Azure?</span></span> <br/><span data-ttu-id="b2a35-262">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="b2a35-262">
**A**: No.</span></span> <span data-ttu-id="b2a35-263">Bránu se připojí k místním zdrojům dat pouze.</span><span class="sxs-lookup"><span data-stu-id="b2a35-263">A gateway connects to on-premises data sources only.</span></span>

<span data-ttu-id="b2a35-264">**Q**: nemá brány musí být instalovány na stejném počítači jako zdroj dat?</span><span class="sxs-lookup"><span data-stu-id="b2a35-264">**Q**: Does the gateway have to be installed on the same machine as the data source?</span></span> <br/><span data-ttu-id="b2a35-265">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="b2a35-265">
**A**: No.</span></span> <span data-ttu-id="b2a35-266">Se brána připojí ke zdroji dat pomocí informací o připojení, který byl poskytnut.</span><span class="sxs-lookup"><span data-stu-id="b2a35-266">The gateway connects to the data source using the connection information that was provided.</span></span> <span data-ttu-id="b2a35-267">Vezměte v úvahu brány jako klientskou aplikaci v tomto smyslu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-267">Consider the gateway as a client application in this sense.</span></span> <span data-ttu-id="b2a35-268">Brány musí právě schopnost připojit k názvu serveru, který byl poskytnut.</span><span class="sxs-lookup"><span data-stu-id="b2a35-268">The gateway just needs the capability to connect to the server name that was provided.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="b2a35-269">**Q**: Proč musí I používáte Azure pracovní nebo školní účet pro přihlášení?</span><span class="sxs-lookup"><span data-stu-id="b2a35-269">**Q**: Why must I use an Azure work or school account to sign in?</span></span> <br/><span data-ttu-id="b2a35-270">
**A**: pouze můžete použít Azure pracovní nebo školní účet, když instalujete místní brána data.</span><span class="sxs-lookup"><span data-stu-id="b2a35-270">
**A**: You can only use an Azure work or school account when you install the on-premises data gateway.</span></span> <span data-ttu-id="b2a35-271">Váš přihlašovací účet je uložený v klientovi, který je spravován pomocí služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b2a35-271">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="b2a35-272">Obvykle účet Azure AD hlavní název uživatele (UPN) odpovídá e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-272">Usually, your Azure AD account's user principal name (UPN) matches the email address.</span></span>

<span data-ttu-id="b2a35-273">**Q**: uložení pověření?</span><span class="sxs-lookup"><span data-stu-id="b2a35-273">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="b2a35-274">
**A**: přihlašovací údaje, které zadáte pro zdroj dat jsou zašifrovány a uložené v cloudové službě brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-274">
**A**: The credentials that you enter for a data source are encrypted and stored in the gateway cloud service.</span></span> <span data-ttu-id="b2a35-275">Přihlašovací údaje jsou v bráně místní data dešifrovat.</span><span class="sxs-lookup"><span data-stu-id="b2a35-275">The credentials are decrypted at the on-premises data gateway.</span></span>

<span data-ttu-id="b2a35-276">**Q**: existují všechny požadavky pro šířku pásma sítě?</span><span class="sxs-lookup"><span data-stu-id="b2a35-276">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="b2a35-277">
**A**: doporučujeme, aby připojení k síti dobrý propustnost.</span><span class="sxs-lookup"><span data-stu-id="b2a35-277">
**A**: We recommend that your network connection has good throughput.</span></span> <span data-ttu-id="b2a35-278">Každé prostředí je jiné a množství dat odesílaných ovlivňuje výsledky.</span><span class="sxs-lookup"><span data-stu-id="b2a35-278">Every environment is different, and the amount of data being sent affects the results.</span></span> <span data-ttu-id="b2a35-279">Pomocí ExpressRoute může pomoct zajistit úrovní propustnosti mezi místními a datových centrech Azure.</span><span class="sxs-lookup"><span data-stu-id="b2a35-279">Using ExpressRoute could help to guarantee a level of throughput between on-premises and the Azure datacenters.</span></span>
<span data-ttu-id="b2a35-280">Můžete aplikaci Azure rychlost testovací nástroj třetí strany ke měřidla vaší propustnost.</span><span class="sxs-lookup"><span data-stu-id="b2a35-280">You can use the third-party tool Azure Speed Test app to help gauge your throughput.</span></span>

<span data-ttu-id="b2a35-281">**Q**: co je latence spouštět dotazy na zdroj dat z brány?</span><span class="sxs-lookup"><span data-stu-id="b2a35-281">**Q**: What is the latency for running queries to a data source from the gateway?</span></span> <span data-ttu-id="b2a35-282">Co je nejlepší architektura?</span><span class="sxs-lookup"><span data-stu-id="b2a35-282">What is the best architecture?</span></span> <br/><span data-ttu-id="b2a35-283">
**A**: Chcete-li snížit latenci sítě, nainstalujte bránu co nejblíže zdroji dat jako možné.</span><span class="sxs-lookup"><span data-stu-id="b2a35-283">
**A**: To reduce network latency, install the gateway as close to the data source as possible.</span></span> <span data-ttu-id="b2a35-284">Pokud nainstalujete brány na skutečná data zdroj tento blízkosti minimalizuje latenci zavedená.</span><span class="sxs-lookup"><span data-stu-id="b2a35-284">If you can install the gateway on the actual data source, this proximity minimizes the latency introduced.</span></span> <span data-ttu-id="b2a35-285">Příliš zvažte v datacentrech.</span><span class="sxs-lookup"><span data-stu-id="b2a35-285">Consider the datacenters too.</span></span> <span data-ttu-id="b2a35-286">Například pokud používá službu západní USA datacenter a vy musíte SQL Server hostované ve virtuálním počítači Azure, svého virtuálního počítače Azure musí být v západní USA příliš.</span><span class="sxs-lookup"><span data-stu-id="b2a35-286">For example, if your service uses the West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in the West US too.</span></span> <span data-ttu-id="b2a35-287">Tato blízkosti minimalizuje latenci a zabraňuje s nimi spojeným nákladům na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="b2a35-287">This proximity minimizes latency and avoids egress charges on the Azure VM.</span></span>

<span data-ttu-id="b2a35-288">**Q**: jak se výsledky odesílají zpět do cloudu?</span><span class="sxs-lookup"><span data-stu-id="b2a35-288">**Q**: How are results sent back to the cloud?</span></span> <br/><span data-ttu-id="b2a35-289">
**A**: výsledky se odesílají přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b2a35-289">
**A**: Results are sent through the Azure Service Bus.</span></span>

<span data-ttu-id="b2a35-290">**Q**: existují všechna příchozí připojení k bráně z cloudu?</span><span class="sxs-lookup"><span data-stu-id="b2a35-290">**Q**: Are there any inbound connections to the gateway from the cloud?</span></span> <br/><span data-ttu-id="b2a35-291">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="b2a35-291">
**A**: No.</span></span> <span data-ttu-id="b2a35-292">Brána používá odchozí připojení k Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b2a35-292">The gateway uses outbound connections to Azure Service Bus.</span></span>

<span data-ttu-id="b2a35-293">**Q**: Co když blokovat odchozí připojení?</span><span class="sxs-lookup"><span data-stu-id="b2a35-293">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="b2a35-294">Co je potřeba otevřít?</span><span class="sxs-lookup"><span data-stu-id="b2a35-294">What do I need to open?</span></span> <br/><span data-ttu-id="b2a35-295">
**A**: najdete v části Nastavení portů a hostitelů, které používá bránu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-295">
**A**: See the ports and hosts that the gateway uses.</span></span>

<span data-ttu-id="b2a35-296">**Q**: co je aktuální služby Windows volána?</span><span class="sxs-lookup"><span data-stu-id="b2a35-296">**Q**: What is the actual Windows service called?</span></span><br/><span data-ttu-id="b2a35-297">
**A**: V služby brány se nazývá služba Power BI Enterprise Gateway.</span><span class="sxs-lookup"><span data-stu-id="b2a35-297">
**A**: In Services, the gateway is called Power BI Enterprise Gateway Service.</span></span>

<span data-ttu-id="b2a35-298">**Q**: můžete bránu služby systému Windows spustit pomocí účtu Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b2a35-298">**Q**: Can the gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="b2a35-299">
**A**: Ne.</span><span class="sxs-lookup"><span data-stu-id="b2a35-299">
**A**: No.</span></span> <span data-ttu-id="b2a35-300">Služba systému Windows musí mít platný účet systému Windows.</span><span class="sxs-lookup"><span data-stu-id="b2a35-300">The Windows service must have a valid Windows account.</span></span> <span data-ttu-id="b2a35-301">Ve výchozím nastavení spouští službu s SID služby, NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="b2a35-301">By default, the service runs with the Service SID, NT SERVICE\PBIEgwService.</span></span>

### <a name="high-availability-and-disaster-recovery"></a><span data-ttu-id="b2a35-302">Vysoká dostupnost a zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="b2a35-302">High availability and disaster recovery</span></span>

<span data-ttu-id="b2a35-303">**Q**: jaké možnosti jsou dostupné pro zotavení po havárii?</span><span class="sxs-lookup"><span data-stu-id="b2a35-303">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="b2a35-304">
**A**: obnovovací klíč můžete použít k obnovení nebo přesunout bránu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-304">
**A**: You can use the recovery key to restore or move a gateway.</span></span> <span data-ttu-id="b2a35-305">Při instalaci brány, zadejte obnovovací klíč.</span><span class="sxs-lookup"><span data-stu-id="b2a35-305">When you install the gateway, specify the recovery key.</span></span>

<span data-ttu-id="b2a35-306">**Q**: co je výhodou obnovovací klíč?</span><span class="sxs-lookup"><span data-stu-id="b2a35-306">**Q**: What is the benefit of the recovery key?</span></span> <br/><span data-ttu-id="b2a35-307">
**A**: obnovovací klíč poskytuje způsob, jak migrovat nebo obnovení po havárii nastavení brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-307">
**A**: The recovery key provides a way to migrate or recover your gateway settings after a disaster.</span></span>

<span data-ttu-id="b2a35-308">**Q**: existují všechny plány pro povolení scénáře s vysokou dostupností s bránou?</span><span class="sxs-lookup"><span data-stu-id="b2a35-308">**Q**: Are there any plans for enabling high availability scenarios with the gateway?</span></span> <br/><span data-ttu-id="b2a35-309">
**A**: tyto scénáře jsou plánovaná, ale ještě nemáme časové osy.</span><span class="sxs-lookup"><span data-stu-id="b2a35-309">
**A**: These scenarios are on the roadmap, but we don't have a timeline yet.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b2a35-310">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="b2a35-310">Troubleshooting</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

<span data-ttu-id="b2a35-311">**Q**: jak můžete zjistit, co jsou dotazy se posílá místní zdroj dat?</span><span class="sxs-lookup"><span data-stu-id="b2a35-311">**Q**: How can I see what queries are being sent to the on-premises data source?</span></span> <br/><span data-ttu-id="b2a35-312">
**A**: můžete povolit trasování dotazů, který obsahuje dotazy, které se odesílají.</span><span class="sxs-lookup"><span data-stu-id="b2a35-312">
**A**: You can enable query tracing, which includes the queries that are sent.</span></span> <span data-ttu-id="b2a35-313">Nezapomeňte změnit dotaz trasování zpět na původní hodnotu po dokončení odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="b2a35-313">Remember to change query tracing back to the original value when done troubleshooting.</span></span> <span data-ttu-id="b2a35-314">Trasování dotazů, které jsou zapnuté vytvoří větší protokoly.</span><span class="sxs-lookup"><span data-stu-id="b2a35-314">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="b2a35-315">Můžete také zobrazit nástroje, které má váš zdroj dat pro trasování dotazů.</span><span class="sxs-lookup"><span data-stu-id="b2a35-315">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="b2a35-316">Můžete například použít rozšířených událostí nebo profileru SQL pro SQL Server a služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="b2a35-316">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="b2a35-317">**Q**: kde jsou protokoly gateway?</span><span class="sxs-lookup"><span data-stu-id="b2a35-317">**Q**: Where are the gateway logs?</span></span> <br/><span data-ttu-id="b2a35-318">
**A**: viz nástroje později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-318">
**A**: See Tools later in this topic.</span></span>

### <a name="update-to-the-latest-version"></a><span data-ttu-id="b2a35-319">Aktualizovat na nejnovější verzi</span><span class="sxs-lookup"><span data-stu-id="b2a35-319">Update to the latest version</span></span>

<span data-ttu-id="b2a35-320">Mnohé problémy můžete surface, když se stane zastaralé verze brány.</span><span class="sxs-lookup"><span data-stu-id="b2a35-320">Many issues can surface when the gateway version becomes outdated.</span></span> <span data-ttu-id="b2a35-321">Jako vhodné obecné Ujistěte se, že používáte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="b2a35-321">As good general practice, make sure that you use the latest version.</span></span> <span data-ttu-id="b2a35-322">Pokud jste neprovedli aktualizaci brány pro měsíc i déle, můžete zvažte instalaci nejnovější verze brány a v tématu, pokud jste reprodukujte problém.</span><span class="sxs-lookup"><span data-stu-id="b2a35-322">If you haven't updated the gateway for a month or longer, you might consider installing the latest version of the gateway, and see if you can reproduce the issue.</span></span>

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="b2a35-323">Chyba: Nepodařilo se přidat uživatele do skupiny.</span><span class="sxs-lookup"><span data-stu-id="b2a35-323">Error: Failed to add user to group.</span></span> <span data-ttu-id="b2a35-324">(-2147463168 PBIEgwService výkonu protokolu uživatelů)</span><span class="sxs-lookup"><span data-stu-id="b2a35-324">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="b2a35-325">Může se tato chyba, pokud se pokusíte nainstalovat bránu na řadič domény, což není podporováno.</span><span class="sxs-lookup"><span data-stu-id="b2a35-325">You might get this error if you try to install the gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="b2a35-326">Ujistěte se, která můžete nasadit bránu na počítači, který není řadičem domény.</span><span class="sxs-lookup"><span data-stu-id="b2a35-326">Make sure that you deploy the gateway on a machine that isn't a domain controller.</span></span>

## <a name="tools"></a><span data-ttu-id="b2a35-327">Nástroje</span><span class="sxs-lookup"><span data-stu-id="b2a35-327">Tools</span></span>

### <a name="collect-logs-from-the-gateway-configurer"></a><span data-ttu-id="b2a35-328">Shromažďování protokolů z brány configurer</span><span class="sxs-lookup"><span data-stu-id="b2a35-328">Collect logs from the gateway configurer</span></span>

<span data-ttu-id="b2a35-329">Můžete shromáždit několik protokolů pro bránu.</span><span class="sxs-lookup"><span data-stu-id="b2a35-329">You can collect several logs for the gateway.</span></span> <span data-ttu-id="b2a35-330">Vždy spusťte s protokoly!</span><span class="sxs-lookup"><span data-stu-id="b2a35-330">Always start with the logs!</span></span>

#### <a name="installer-logs"></a><span data-ttu-id="b2a35-331">Instalační protokoly</span><span class="sxs-lookup"><span data-stu-id="b2a35-331">Installer logs</span></span>

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a><span data-ttu-id="b2a35-332">Konfigurace protokolů</span><span class="sxs-lookup"><span data-stu-id="b2a35-332">Configuration logs</span></span>

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="b2a35-333">Protokoly služby Enterprise gateway</span><span class="sxs-lookup"><span data-stu-id="b2a35-333">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a><span data-ttu-id="b2a35-334">Protokoly událostí</span><span class="sxs-lookup"><span data-stu-id="b2a35-334">Event logs</span></span>

<span data-ttu-id="b2a35-335">Můžete najít v protokolech Brána pro správu dat a PowerBIGateway pod **protokoly aplikací a služeb**.</span><span class="sxs-lookup"><span data-stu-id="b2a35-335">You can find the Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>

### <a name="fiddler-trace"></a><span data-ttu-id="b2a35-336">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="b2a35-336">Fiddler Trace</span></span>

<span data-ttu-id="b2a35-337">[Fiddler](http://www.telerik.com/fiddler) je bezplatný nástroj na webu Telerik, který monitoruje provoz protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b2a35-337">[Fiddler](http://www.telerik.com/fiddler) is a free tool from Telerik that monitors HTTP traffic.</span></span> <span data-ttu-id="b2a35-338">Zobrazí se tato komunikace se službou Power BI v klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="b2a35-338">You can see this traffic with the Power BI service from the client machine.</span></span> <span data-ttu-id="b2a35-339">Tato služba může zobrazit chyby a další související informace.</span><span class="sxs-lookup"><span data-stu-id="b2a35-339">This service might show errors and other related information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2a35-340">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2a35-340">Next steps</span></span>
    
* [<span data-ttu-id="b2a35-341">Připojit k místním datům z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="b2a35-341">Connect to on-premises data from logic apps</span></span>](../logic-apps/logic-apps-gateway-connection.md)
* [<span data-ttu-id="b2a35-342">Funkce Integrace organizace</span><span class="sxs-lookup"><span data-stu-id="b2a35-342">Enterprise integration features</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="b2a35-343">Konektory pro Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="b2a35-343">Connectors for Azure Logic Apps</span></span>](../connectors/apis-list.md)
