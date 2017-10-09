## <a name="scenario"></a><span data-ttu-id="c14bd-101">Scénář</span><span class="sxs-lookup"><span data-stu-id="c14bd-101">Scenario</span></span>
<span data-ttu-id="c14bd-102">toobetter ilustrují, jak toocreate virtuální sítě a podsítě, tento dokument se bude používat hello scénář níže.</span><span class="sxs-lookup"><span data-stu-id="c14bd-102">toobetter illustrate how toocreate a VNet and subnets, this document will use hello scenario below.</span></span>

![Scénář sítě VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

<span data-ttu-id="c14bd-104">V tomto scénáři vytvoříte virtuální síť VNet s názvem **TestVNet** s vyhrazeným blokem CIDR **192.168.0.0./16**.</span><span class="sxs-lookup"><span data-stu-id="c14bd-104">In this scenario you will create a VNet named **TestVNet** with a reserved CIDR block of **192.168.0.0./16**.</span></span> <span data-ttu-id="c14bd-105">Vaše síť VNet bude obsahovat hello následující podsítě:</span><span class="sxs-lookup"><span data-stu-id="c14bd-105">Your VNet will contain hello following subnets:</span></span> 

* <span data-ttu-id="c14bd-106">**FrontEnd** s blokem CIDR **192.168.1.0/24**</span><span class="sxs-lookup"><span data-stu-id="c14bd-106">**FrontEnd**, using **192.168.1.0/24** as its CIDR block.</span></span>
* <span data-ttu-id="c14bd-107">**BackEnd** s blokem CIDR **192.168.2.0/24**</span><span class="sxs-lookup"><span data-stu-id="c14bd-107">**BackEnd**, using **192.168.2.0/24** as its CIDR block.</span></span>

