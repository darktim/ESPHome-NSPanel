### Overview

The [Sonoff NSPanel](https://sonoff.tech/product/smart-wall-swtich/nspanel/) gets a lot of attention right now. This is a collection of snippets for configurations of [ESPHome](https://esphome.io). Feel free to use them, and let me know of other neat tricks ;)

# Wifi indicator

I have created 5 PNG imges representing the state of the signal. I am using the ESPHome component [Wifi signal](https://esphome.io/components/sensor/wifi_signal.html) to change the picture on the display according to the RSSI value:
![wLAN_0](https://github.com/darktim/ESPHome-NSPanel/blob/main/images/wlan_0.png "wLAN_1") ![wLAN_1](https://github.com/darktim/ESPHome-NSPanel/blob/main/images/wlan_1.png "wLAN_1") ![wLAN_2](https://github.com/darktim/ESPHome-NSPanel/blob/main/images/wlan_2.png "wLAN_2") ![wLAN_3](https://github.com/darktim/ESPHome-NSPanel/blob/main/images/wlan_3.png "wLAN_3") ![wLAN_4](https://github.com/darktim/ESPHome-NSPanel/blob/main/images/wlan_4.png "wLAN_4") 

In the  [Nextion Editor](https://nextion.tech/nextion-editor/) (only available as Microsoft Software) I added these in the "Picture" menu. Please note the image IDs. In my case the image wlan_0 has got ID 1 and wlan_4 has got ID 5.
Then I added the wlan_0 image on the Display. You will need the "objname" of this new object - I changed mine to the name "wlan"

After uploading the tft file to the display, you should see the wlan_0 picture.

#### ESPHome configuration wifi indicator

    - platform: wifi_signal
      name: "$devicename WiFi Signal"
      id: wifi_rssi
      update_interval: 15s
      on_value:
        # Push it to the display
        # RSSI is negative
        #  -0 - -50 => excellent  - wlan4 - pic id 5
        # -50 - -60 => good       - wlan3 - pic id 4
        # -60 - -70 => fair       - wlan2 - pic id 3
        # -70 - -85 => weak       - wlan1 - pic id 2
        # -85 - -90 => offline    - wlan0 - pic id 1
        then:
          - lambda: |-
              if (id(wifi_rssi).state >= -50)  {id(display1).set_component_pic("wlan", 5);}
              if ((id(wifi_rssi).state < -50) && (id(wifi_rssi).state >= -60)) id(display1).set_component_pic("wlan", 4);}
              if ((id(wifi_rssi).state < -60) && (id(wifi_rssi).state >= -70)) {id(display1).set_component_pic("wlan", 3);}
              if ((id(wifi_rssi).state < -70) && (id(wifi_rssi).state >= -85)) {id(display1).set_component_pic("wlan", 2);}
              if (id(wifi_rssi).state < -85) {id(display1).set_component_pic("wlan", 1);}

