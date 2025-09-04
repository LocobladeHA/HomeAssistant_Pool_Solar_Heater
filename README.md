# HomeAssistant Pool Solar Heater Automation
Home Assistant automations to control a DIY pool solar heating system. The automation controls a water pump that circlates water through the solar panels based on temperature feeds from 3 temperature sensors such as Sonoff TH Elite with waterproof temperature probe.

#### **solar_manager.yaml** 
This is the main automation that controls a water pump, circulating pool water around the solar panels when they're warm enough to heat the water. The automation triggers whenever the solar panel air temperature changes with some conditions to prevent it running at night etc. Note all temperatures are in celcius.

Automation Basic Logic: 
* Trigger on change to solar panel air temperature
* Compare solar panel internal air temperature and pool temperature
* If the solar panel internal air temperature is more than 5c above the pool water temperature > start the pump for 8 minutes (to get a reliable return flow temperature from the panels). 
* Start running a loop every 15 seconds to compare the pool water temperature and the water returning from the solar panels. 
* If the return water is warmer than the pool water repeat the loop, the pump continues to run.
* If the return water is cooler than the pool water, break out of the loop and stop the pump.

The 5c offset against pool temperature means if the pool is at 25c for example, and the panel is at 30c, for my panels the water being returned will almost always end up being warmer than 25c. You may need to adjust this threshold via the sensor.solar_min_target_temp sensor template (Required_Sensor.yaml) to suit your panels.

#### **Overnight_Cooling.yaml**
Optional automation, this can be run overnight when the pool is too warm by pumping water through the solar panels overnight to have a cooling effect. For me, the return water still comes back about 2c lower than it left the pool when run on a warm summer night, and if run for 8 hours it almost doubles the overnight pool water temperature loss compared to natural overnight cooling alone.

#### **Required_Sensor.yaml**
Sensor that updates sensor.solar_min_target_temp. The main automation uses this as a trigger to start the pump and keep the pump running all the time the panel is warm enough to generate heat. Create in yaml or as a Helper Template Sensor

#### **Optional_Sensors.yaml**
Not needed for the main automation but you can add these sensors to caclulate the realtime heat output of your solar panels in kW for display on your dashboard. Create in yaml or as a Helper Template Sensor and Filter Sensor



### **To run the automations you'll need to provide the following input entities and helpers:**

#### **sensor.<solar_panel_air_temp>**
Temperature sensor entity monitoring the air temperature of the solar panel. My panels are enclosed with glass and reach 65c+ in sunny weather and 30+c even when overcast (when it can still generate some heat) meaning it's easy to detect when enough sun is hitting the panel to likely generate heat and start the automation. If your panels are open to the air not enclosed in glass / plastic then I'd suggest putting your sensor in a clear sealed box next to the panels to try and replicate the greenhouse effect because open air temperature itself won't necessarily be significantly higher when the sun is out and free air temperature won't give a good indicator of solar generation potential so the automation won't trigger reliably.

#### **sensor.<pool_water_temp>**
Temperature sensor entity monitoring the pool water temperature

#### **sensor.<solar_water_return_temp>**
Temperature sensor entity monitoring the return water temperature from the solar panels. Can be tricky to monitor, I bought a Tee that fits the 18mm hose returning from the panels then glued the sensor into this so the probe sits almost directly in the return flow.

#### **switch.<solar_water_pump>**
Smart switch controlling the pump that circulates water around the solar panels

#### **sensor.solar_min_target_temp**
Sensor updated by solar_min_target_temp sensor template. Adds 5c to the current pool temperature and the output is used as a threshold to compare the water temp against the solar panel air temp for triggering the automation. Adjust the threshold to suit your needs.

#### **input_<boolean_pool_in_use>**
Helper updated by a switch on my dashbaord which we can activate to turn off all pumps whilst the pool is in use or other times when we don't want the automation to run.

Solar Heating Layout:
![Solar Heating Diagram](https://github.com/LocobladeHA/HomeAssistant_Pool_Solar_Heater/blob/main/Solar%20Heater%20Layout.png)
My Panels:
![My Solar Panels](https://github.com/LocobladeHA/HomeAssistant_Pool_Solar_Heater/blob/main/MySolarPanels.jpg)
![PanelBuild](https://github.com/LocobladeHA/HomeAssistant_Pool_Solar_Heater/blob/main/PanelBuild.jpg)

