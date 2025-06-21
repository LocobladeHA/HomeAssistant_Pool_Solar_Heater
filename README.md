# HomeAssistant Pool Solar Heater Automation
Home Assistant automations to control a DIY pool solar heating system. Controls a water pump that circlates water through the solar panels based on temperature feeds from 3 temperature sensors such as Sonoff TH Elite with waterproof temperature probe.

#### **solar_manager.yaml** 
This is the main automation that controls a water pump, circulating pool water around the solar panels when they're warm enough to heat the water. The automation triggers whenever the solar panel air temperature changes with some conditions to prevent it running at night etc. Note all temperatures are in celcius.

Automation Logic: If the solar panel internal air temperature is 5c or more above the pool water temperature it will start the pump for 8 minutes to get a reliable return flow temperature from the panels. It then runs a while loop to monitor the pool water temperature and the water returning from the solar panels every 15 seconds. If the return water remains warmer than the pool water it will stay in the loop, letting the pump continue to run. If the return water isn't warmer than the pool water it will break out of the loop and stop the pump. The 5c offset works for my panels but you may need to play with the threshold via the helper input pool_temp_add_5c.yaml automation to suit your setup.

#### **pool_temp_add_5c.yaml**
Helper automation that updates the input_<boolean_pool_water_temp_plus5c>

#### **Optional_Sensors.yaml**
Not needed for the main automation but you can add these sensors to caclulate the realtime heat output of your solar panels in kW for display on your dashboard.



### **To run the automations you'll need to provide the following input entities and helpers:**

#### **sensor.<solar_panel_air_temp>**
Temperature sensor entity monitoring the air temperature of the solar panel. My panels are enclosed with glass and reach 65c+ in sunny weather and 30+c even when overcast meaning it's easy to detect when enoughsun is hitting the panel to generate heat. If your panels are open the air and aren't enclosed in glass / plastic then I'd suggest putting your sensor in a clear sealed box next to the panels to replicate the greenhouse effect because open air temperature itself won't necessarily be significantly higher when the sun is out and won't give a good indicator of solar generation potential so the automation won't trigger reliably.

#### **sensor.<pool_water_temp>**
Temperature sensor entity monitoring the pool water temperature

#### **sensor.<solar_water_return_temp>**
Temperature sensor entity monitoring the return water temperature from the solar panels. Can be tricky to monitor, I bought a Tee that fits the 18mm hose returning from the panels then glued the sensor into this so the probe sits almost directly in the return flow.

#### **switch.<solar_water_pump>**
Smart switch controlling the pump that circulates water around the solar panels

#### **input_<boolean_pool_water_temp_plus5c>**
Helper updated by pool_temp_add_5c.yaml automation, adds 5c to the current pool temperature and the output is used as a threshold to compare the water temp against the solar panel air temp for triggering the automation. Adjust the threshold to suit your solution.

#### **input_<boolean_pool_in_use>**
Helper updated by a switch on my front end which we can activate to turn off all pumps whilst the pool is in use or other times when we don't want the automation to run.

My Panels:
![alt text](https://github.com/LocobladeHA/HomeAssistant_Pool_Solar_Heater/blob/cacfb90a23c6a951f953364447e5dc6ef1134b21/MySolarPanels.jpg?raw=true "Pool Solar Panels")
