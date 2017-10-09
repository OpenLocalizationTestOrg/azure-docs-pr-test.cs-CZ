## <a name="scenario"></a>Scénář
toobetter ilustrují, jak toocreate virtuální sítě a podsítě, tento dokument se bude používat hello scénář níže.

![Scénář sítě VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

V tomto scénáři vytvoříte virtuální síť VNet s názvem **TestVNet** s vyhrazeným blokem CIDR **192.168.0.0./16**. Vaše síť VNet bude obsahovat hello následující podsítě: 

* **FrontEnd** s blokem CIDR **192.168.1.0/24**
* **BackEnd** s blokem CIDR **192.168.2.0/24**

