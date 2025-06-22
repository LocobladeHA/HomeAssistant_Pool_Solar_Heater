# HomeAssistant Pool Solar Heater Automation
My Home Assistant automations to control a DIY pool solar heating system. Controls a water pump that circlates water through the solar panels based on temperature feeds from 3 temperature sensors such as Sonoff TH Elite with waterproof temperature probe.

#### **solar_manager.yaml** 
This is the main automation that controls a water pump, circulating pool water around the solar panels when they're warm enough to heat the water. The automation triggers whenever the solar panel air temperature changes with some conditions to prevent it running at night etc. Note all temperatures are in celcius.

Automation Logic: If the solar panel internal air temperature is 5c or more above the pool water temperature it will start the pump for 8 minutes to get a reliable return flow temperature from the panels. It then runs a while loop to monitor the pool water temperature and the water returning from the solar panels every 15 seconds. If the return water is warmer than the pool water it will stay in the loop, letting the pump continue to run. If the return water isn't warmer than the pool water it will break out of the loop and stop the pump. The 5c offset against pool temperature works for my panels but you may need to play with the threshold via the sensor.solar_min_target_temp sensor template to suit your needs.

#### **Overnight_Cooling.yaml**
Optional automation, can be used overnight when the pool is too warm by running water through your solar panels overnight which has a cooling effect. My return water comes back about 2c lower than it left the pool when run on a summer night and if drops the overall pool temperature significantly more than natural overnight cooling alone.

#### **Required_Sensor.yaml**
Sensor that updates sensor.solar_min_target_temp. The main automation uses this as a trigger to start the pump and keep the pump running all the time the panel is warm enough to generate heat. Create in yaml or as a Helper Template Sensor

#### **Optional_Sensors.yaml**
Not needed for the main automation but you can add these sensors to caclulate the realtime heat output of your solar panels in kW for display on your dashboard. Create in yaml or as a Helper Template Sensor and Filter Sensor



### **To run the automations you'll need to provide the following input entities and helpers:**

#### **sensor.<solar_panel_air_temp>**
Temperature sensor entity monitoring the air temperature of the solar panel. My panels are enclosed with glass and reach 65c+ in sunny weather and 30+c even when overcast (when it can still generate some heat) meaning it's easy to detect when enough sun is hitting the panel to likely generate heat and start the automation. If your panels are open the air and aren't enclosed in glass / plastic then I'd suggest putting your sensor in a clear sealed box next to the panels to try and replicate the greenhouse effect because open air temperature itself won't necessarily be significantly higher when the sun is out and it won't give a good indicator of solar generation potential so the automation won't trigger reliably.

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

My Panels:
![alt text](https://github.com/LocobladeHA/HomeAssistant_Pool_Solar_Heater/blob/cacfb90a23c6a91f953364447e5dc6ef1134b21/MySolarPanels.jpg?raw=true "Pool Solar Panels")
