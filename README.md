# HomeAssistant Pool Solar Heater Automation
Home Assistant automations to control a DIY pool solar heating system. Controls a water pump that circlates water through the solar panels based on temperature feeds from 3 temperature sensors such as Sonoff TH Elite with waterproof temperature probe.

#### **solar_manager.yaml** 
This is the main automation that controls a water pump, circulating pool water around the solar panels when they're warm enough to heat the water. Note all temperatures are in celcius.

#### **pool_temp_add_5c.yaml**
Helper automation that updates the input_<boolean_pool_water_temp_plus5c>`

**For these automations to run you'll need to provide the following inputs and helpers:**

#### **sensor.<solar_panel_air_temp>**
Temperature sensor entity monitoring the air temperature of the solar panel. My panels are enclosed with glass and reach 65c+ in sunny weather meaning it's easy to detect when the sun is hitting the panel and they're likely to generate heat. If your panels are open the air and aren't enclosed in glass / plastic then I'd suggest putting your sensor in a clear sealed box next to the panels to replicate the greenhouse effect because open air temperature itself won't necessarily be significantly higher when the sun is out and won't give a good indicator of solar generation potential so the automation won't trigger reliably.

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
