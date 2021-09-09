# victron-openEVSE
openEVSE controller for Victron PV / battery system

This victron-openEVSE controller is a Node-Red implementation using Victron's Venus OS v2.70~5-large-18 running on a MultiPlus II GX.  It communicates with the openEVSE charger via a MQTT broker that happens to be Mosquito running on a Raspberry Pi.  

The software assumes a PV array (in my case 5kW but needs to be at least 1.5kW) and a BYD (Cobalt Free Lithium Iron Phosphate) battery of 8kWh of storage.  Other configurations could be supported by changing some software parameters.  In addition the PV solar system comprises a MultiPlus II GX 48/5000/70-50 and SmartSolar MPPT RS 450|100, both from Victron Energy.

There are three modes of operation with two behaving similarly:
1. 32A (7.6kW in the UK).  The MultiPlus will use the remaining battery storage supplemented with grid mains.  When the battery reaches its shut-off level the charging will be all mains.
2. As above but at 16A (3.8kW in the UK).
3. This option uses PV supplemented by grid mains but it is constantly monitoring the PV power being generated and adjusting the openEVSE charging current so as to try and not draw any current from either the battery or the grid mains.  In this mode the software communicates with the openEVSE charger (using MQTT messaging via a broker) to instruct it to draw between 6A and 20A (1.4kW to 4.8kW) to maximise the available power from the PV.  This mode cannot charge at a higher rate than 20A.  If the PV power drops below ~1.5 kW (6A) then the battery supplements the PV until the battery reaches a variable level below which it will not be able to keep various additional appliances running through the night.  When this lower threshold is reached the openEVSE is put into sleep mode allowing the PV to charge the battery again until it reaches an upper threshold when it switches on the openEVSE charger again.

The software is controlled via a Node-Red dashboard where various informative dials and gauges show what is happening in the system.
