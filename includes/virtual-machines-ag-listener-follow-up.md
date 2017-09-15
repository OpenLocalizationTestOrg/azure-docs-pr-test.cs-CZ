<span data-ttu-id="421b0-101">Po vytvoření naslouchacího procesu skupiny dostupnosti, může být nezbytné upravit parametry clusteru RegisterAllProvidersIP a HostRecordTTL pro prostředek naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="421b0-101">After you create the availability group listener, it might be necessary to adjust the RegisterAllProvidersIP and HostRecordTTL cluster parameters for the listener resource.</span></span> <span data-ttu-id="421b0-102">Tyto parametry můžete zkrátit dobu opětovného připojení po selhání, což by mohlo zabránit vypršení časových limitů připojení.</span><span class="sxs-lookup"><span data-stu-id="421b0-102">These parameters can reduce reconnection time after a failover, which might prevent connection timeouts.</span></span> <span data-ttu-id="421b0-103">Další informace o těchto parametrů, a také ukázkový kód, najdete v části [vytvořit nebo nakonfigurovat naslouchací proces skupiny dostupnosti](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span><span class="sxs-lookup"><span data-stu-id="421b0-103">For more information about these parameters, as well as sample code, see [Create or configure an availability group listener](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).</span></span>
