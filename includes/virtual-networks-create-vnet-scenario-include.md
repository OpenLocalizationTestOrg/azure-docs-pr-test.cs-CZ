## <a name="scenario"></a><span data-ttu-id="62e67-101">Scénář</span><span class="sxs-lookup"><span data-stu-id="62e67-101">Scenario</span></span>
<span data-ttu-id="62e67-102">V tomto dokumentu budeme používat následující scénář, abychom vám lépe předvedli, jak vytvořit síť VNet a podsítě.</span><span class="sxs-lookup"><span data-stu-id="62e67-102">To better illustrate how to create a VNet and subnets, this document will use the scenario below.</span></span>

![Scénář sítě VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

<span data-ttu-id="62e67-104">V tomto scénáři vytvoříte virtuální síť VNet s názvem **TestVNet** s vyhrazeným blokem CIDR **192.168.0.0./16**.</span><span class="sxs-lookup"><span data-stu-id="62e67-104">In this scenario you will create a VNet named **TestVNet** with a reserved CIDR block of **192.168.0.0./16**.</span></span> <span data-ttu-id="62e67-105">Vaše síť VNet bude obsahovat tyto podsítě:</span><span class="sxs-lookup"><span data-stu-id="62e67-105">Your VNet will contain the following subnets:</span></span> 

* <span data-ttu-id="62e67-106">**FrontEnd** s blokem CIDR **192.168.1.0/24**</span><span class="sxs-lookup"><span data-stu-id="62e67-106">**FrontEnd**, using **192.168.1.0/24** as its CIDR block.</span></span>
* <span data-ttu-id="62e67-107">**BackEnd** s blokem CIDR **192.168.2.0/24**</span><span class="sxs-lookup"><span data-stu-id="62e67-107">**BackEnd**, using **192.168.2.0/24** as its CIDR block.</span></span>

