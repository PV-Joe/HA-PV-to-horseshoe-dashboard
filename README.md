# HA-PV-to-horseshoe-dashboard
📘 This tutorial explains the process to create a [Home Assistant](https://www.home-assistant.io) horseshoe dashboard for your photovoltaic system.

🔧 Dependencies:
📥 Install these before pasting the code into a manual card:

1. 🧩 [Flexible Horseshoe Card for Lovelace](https://github.com/AmoebeLabs/flex-horseshoe-card)
2. 🎨 [card-mod](https://github.com/thomasloven/lovelace-card-mod)
3. ☀️ [Forecast.Solar](https://www.home-assistant.io/integrations/forecast_solar) (for 'REMAINING')
4. 💰 Create a custom sensor to calculate Grid import in currency
5. ✏️ Modify the entities from the code to match your sensor names
6. 📋 Paste the code into a manual card

Dashboard optimized for mobile phones:

![Horseshoe](https://github.com/user-attachments/assets/ca4edfbb-2bb6-426a-9685-9bb7a235eb98)



```
type: vertical-stack
cards:
  - type: picture
    tap_action:
      action: none
    hold_action:
      action: none
    image: local/Grafiken/Deye.jpg
  - type: custom:mushroom-chips-card
    chips:
      - type: entity
        entity: switch.pc_192_168_178_89_internet_access
        icon_color: green
      - type: entity
        entity: binary_sensor.solarman_connection
        icon_color: green
        icon: mdi:connection
      - type: entity
        entity: sensor.solarman_device_state
        icon_color: green
      - type: entity
        entity: sensor.solarman_battery_state
        card_mod:
          style: |
            ha-card {
              {% if states('sensor.solarman_battery_state') == 'idle' %}
                --card-mod-icon-color: #43A047;
              {% elif states('sensor.solarman_battery_state') == 'charging' %}
                --card-mod-icon-color: orange;
              {% elif states('sensor.solarman_battery_state') == 'discharging' %}
                --card-mod-icon-color: #DB4437;
              {% endif %}
            } 
    alignment: justify
    layout_options:
      grid_columns: 4
      grid_rows: 1
  - type: horizontal-stack
    cards:
      - type: custom:flex-horseshoe-card
        view_layout: null
        entities:
          - entity: sensor.solarman_pv_power
            decimals: 0
            unit: W
            name: Solar
          - entity: sensor.solarman_pv1_power
            decimals: 0
            unit: W
            name: East
          - entity: sensor.solarman_pv2_power
            decimals: 0
            unit: W
            name: West
          - entity: sensor.solarman_pv1_voltage
            decimals: 1
            unit: V
          - entity: sensor.solarman_pv2_voltage
            decimals: 1
            unit: V
          - entity: sensor.solarman_pv1_current
            decimals: 1
            unit: A
          - entity: sensor.solarman_pv2_current
            decimals: 1
            unit: A
          - entity: sensor.solarman_today_production
            unit: kWh
            name: Production
          - entity: sensor.energy_production_today_remaining
            unit: kWh
            name: Remaining
        show:
          horseshoe_style: lineargradient
        layout:
          hlines:
            - id: 0
              xpos: 50
              ypos: 40
              length: 70
              styles:
                - stroke: rgb(59, 60, 62);
          vlines:
            - id: 0
              xpos: 50
              ypos: 60
              length: 39
              styles:
                - stroke: rgb(59, 60, 62);
          states:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 34
              styles:
                - font-size: 3em;
                - opacity: 0.9;
            - id: 1
              entity_index: 1
              xpos: 44
              ypos: 55
              styles:
                - font-size: 1.6em;
                - text-anchor: end;
            - id: 2
              entity_index: 2
              xpos: 56
              ypos: 55
              styles:
                - text-anchor: start;
                - font-size: 1.6em;
            - id: 3
              entity_index: 3
              xpos: 44
              ypos: 65
              styles:
                - text-anchor: end;
                - font-size: 1.3em;
            - id: 4
              entity_index: 4
              xpos: 56
              ypos: 65
              styles:
                - text-anchor: start;
                - font-size: 1.3em;
            - id: 5
              entity_index: 5
              xpos: 44
              ypos: 75
              styles:
                - text-anchor: end;
                - font-size: 1.3em;
            - id: 6
              entity_index: 6
              xpos: 56
              ypos: 75
              styles:
                - text-anchor: start;
                - font-size: 1.3em;
            - id: 7
              entity_index: 7
              xpos: 0
              ypos: 5
              styles:
                - text-anchor: start;
                - font-size: 1.1em;
            - id: 8
              entity_index: 8
              xpos: 100
              ypos: 5
              styles:
                - text-anchor: end;
                - font-size: 1.1em;
          icons:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 15
              align: center
              icon_size: 1.2
              styles:
                - color: orange;
          names:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 95
              styles:
                - font-size: 1.2em;
            - id: 1
              entity_index: 1
              xpos: 15
              ypos: 45
              styles:
                - text-anchor: start;
                - font-size: 0.5em;
            - id: 2
              entity_index: 2
              xpos: 85
              ypos: 45
              styles:
                - text-anchor: end;
                - font-size: 0.5em;
            - id: 3
              entity_index: 7
              xpos: 0
              ypos: 9
              styles:
                - text-anchor: start;
                - font-size: 0.5em;
            - id: 4
              entity_index: 8
              xpos: 100
              ypos: 9
              styles:
                - font-size: 0.5em;
                - text-anchor: end;
        horseshoe_scale:
          min: 0
          max: 7380
          width: 6
          color: '#6F6F6F'
        color_stops:
          '0': '#DB4437'
          '1000': orange
        card_mod:
          style: |
            ha-card {
              --ha-card-background: var(--card-background-color);
              color: var(--primary-color); 
            }
      - type: custom:flex-horseshoe-card
        view_layout: null
        entities:
          - entity: sensor.solarman_grid_power
            unit: W
            decimals: 0
            name: Grid
          - entity: sensor.solarman_grid_l1_power
            decimals: 0
            unit: W
          - entity: sensor.solarman_grid_l1_voltage
            decimals: 1
            unit: V
          - entity: sensor.solarman_grid_l2_power
            decimals: 0
            unit: W
          - entity: sensor.solarman_grid_l2_voltage
            decimals: 1
            unit: V
          - entity: sensor.solarman_grid_l3_power
            decimals: 0
            unit: W
          - entity: sensor.solarman_grid_l3_voltage
            decimals: 1
            unit: V
          - entity: sensor.solarman_grid_frequency
            decimals: 2
            unit: Hz
          - entity: sensor.ek_netz_heute_euro
            decimals: 2
            unit: €
            name: Import
          - entity: sensor.solarman_today_energy_export
            unit: kWh
            name: Export
          - entity: sensor.solarman_today_energy_import
            unit: kWh
            name: Import
        show:
          horseshoe_style: autominmax
        layout:
          hlines:
            - id: 0
              xpos: 50
              ypos: 40
              length: 70
              styles:
                - stroke: rgb(59, 60, 62);
            - id: 1
              xpos: 50
              ypos: 60
              length: 70
              styles:
                - stroke: rgb(59, 60, 62);
          vlines:
            - id: 0
              xpos: 37
              ypos: 50
              length: 18
              styles:
                - stroke: rgb(59, 60, 62);
                - stroke-linecap: square;
            - id: 1
              xpos: 63
              ypos: 50
              length: 18
              styles:
                - stroke: rgb(59, 60, 62);
                - stroke-linecap: square;
          states:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 34
              styles:
                - font-size: 3em;
                - opacity: 0.9;
            - id: 1
              entity_index: 1
              xpos: 34
              ypos: 49
              styles:
                - font-size: 1.2em;
                - text-anchor: end;
            - id: 2
              entity_index: 2
              xpos: 34
              ypos: 56
              styles:
                - font-size: .9em;
                - text-anchor: end;
            - id: 3
              entity_index: 3
              xpos: 51
              ypos: 49
              styles:
                - font-size: 1.2em;
            - id: 4
              entity_index: 4
              xpos: 51
              ypos: 56
              styles:
                - font-size: .9em;
            - id: 5
              entity_index: 5
              xpos: 66
              ypos: 49
              styles:
                - font-size: 1.2em;
                - text-anchor: start;
            - id: 6
              entity_index: 6
              xpos: 66
              ypos: 56
              styles:
                - font-size: .9em;
                - text-anchor: start;
            - id: 7
              entity_index: 7
              xpos: 50
              ypos: 16
              styles:
                - font-size: .8em;
            - id: 8
              entity_index: 8
              xpos: 50
              ypos: 75
              styles:
                - font-size: 2em;
            - id: 9
              entity_index: 9
              xpos: 100
              ypos: 5
              styles:
                - text-anchor: end;
                - font-size: 1.1em;
            - id: 10
              entity_index: 10
              xpos: 0
              ypos: 5
              styles:
                - text-anchor: start;
                - font-size: 1.1em;
          names:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 95
              styles:
                - font-size: 1.2em;
            - id: 1
              entity_index: 8
              xpos: 50
              ypos: 80
              styles:
                - font-size: 0.5em;
            - id: 2
              entity_index: 9
              xpos: 100
              ypos: 9
              styles:
                - font-size: 0.5em;
                - text-anchor: end;
            - id: 3
              entity_index: 10
              xpos: 0
              ypos: 9
              styles:
                - font-size: 0.5em;
                - text-anchor: start;
        horseshoe_scale:
          min: 0
          max: 12000
          width: 6
          color: '#6F6F6F'
        color_stops:
          '0': '#43A047'
          '300': '#DB4437'
        card_mod:
          style: |
            ha-card {
              --ha-card-background: var(--card-background-color);
              color: var(--primary-color); 
            }
  - type: horizontal-stack
    cards:
      - type: custom:flex-horseshoe-card
        view_layout: null
        entities:
          - entity: sensor.solarman_battery_power
            decimals: 0
            unit: W
            name: Battery
          - entity: sensor.solarman_battery_voltage
            decimals: 2
            unit: V
          - entity: sensor.solarman_battery_current
            decimals: 2
            unit: A
          - entity: sensor.solarman_battery
            decimals: 0
            unit: '%'
          - entity: sensor.solarman_battery_state
          - entity: sensor.solarman_today_battery_life_cycles
            decimals: 1
          - entity: sensor.solarman_battery_soh
            decimals: 1
          - entity: sensor.solarman_battery_temperature
            decimals: 1
            unit: °C
          - entity: sensor.solarman_today_battery_discharge
            unit: kWh
            name: Discharge
            decimals: 1
          - entity: sensor.solarman_today_battery_charge
            unit: kWh
            name: Charge
            decimals: 1
        show:
          horseshoe_style: lineargradient
        layout:
          hlines:
            - id: 0
              xpos: 50
              ypos: 40
              length: 70
              styles:
                - stroke: rgb(59, 60, 62);
            - id: 0
              xpos: 50
              ypos: 60
              length: 70
              styles:
                - stroke: rgb(59, 60, 62);
          vlines:
            - id: 0
              xpos: 50
              ypos: 50
              length: 18
              styles:
                - stroke: rgb(59, 60, 62);
                - stroke-linecap: square;
          states:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 34
              styles:
                - font-size: 3em;
                - opacity: 0.9;
            - id: 1
              entity_index: 1
              xpos: 44
              ypos: 53
              styles:
                - font-size: 1.4em;
                - text-anchor: end;
            - id: 2
              entity_index: 2
              xpos: 56
              ypos: 53
              styles:
                - text-anchor: start;
                - font-size: 1.4em;
            - id: 3
              entity_index: 3
              xpos: 50
              ypos: 75
              styles:
                - font-size: 2em;
            - id: 4
              entity_index: 4
              xpos: 50
              ypos: 80
              styles:
                - font-size: 0.5em;
                - text-transform: uppercase;
            - id: 5
              entity_index: 5
              xpos: 21
              ypos: 65
              styles:
                - text-anchor: start;
                - font-size: .7em;
            - id: 6
              entity_index: 6
              xpos: 85
              ypos: 65
              styles:
                - text-anchor: end;
                - font-size: .7em;
            - id: 7
              entity_index: 7
              xpos: 50
              ypos: 16
              styles:
                - font-size: .8em;
            - id: 8
              entity_index: 8
              xpos: 100
              ypos: 5
              styles:
                - text-anchor: end;
                - font-size: 1.1em;
            - id: 9
              entity_index: 9
              xpos: 0
              ypos: 5
              styles:
                - text-anchor: start;
                - font-size: 1.1em;
          icons:
            - id: 0
              entity_index: 5
              xpos: 4
              ypos: 65.5
              align: start
              size: 0
              styles:
                - color: white;
                - '--mdc-icon-size': 0.65em;
            - id: 0
              entity_index: 6
              xpos: 80
              ypos: 65.5
              align: end
              size: 0
              styles:
                - color: white;
                - '--mdc-icon-size': 0.65em;
          names:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 95
              styles:
                - font-size: 1.2em;
            - id: 1
              entity_index: 8
              xpos: 100
              ypos: 9
              styles:
                - font-size: 0.5em;
                - text-anchor: end;
            - id: 2
              entity_index: 9
              xpos: 0
              ypos: 9
              styles:
                - font-size: 0.5em;
                - text-anchor: start;
        horseshoe_scale:
          min: 0
          max: 2000
          width: 6
          color: '#6F6F6F'
        color_stops:
          '0': '#43A047'
          '500': '#DB4437'
        card_mod:
          style: |
            ha-card {
              --ha-card-background: var(--card-background-color);
              color: var(--primary-color);
            }
      - type: custom:flex-horseshoe-card
        view_layout: null
        entities:
          - entity: sensor.solarman_load_power
            unit: W
            decimals: 0
            name: Load
          - entity: sensor.solarman_load_l1_power
            decimals: 0
            unit: W
          - entity: sensor.solarman_load_l1_voltage
            decimals: 1
            unit: V
          - entity: sensor.solarman_load_l2_power
            decimals: 0
            unit: W
          - entity: sensor.solarman_load_l2_voltage
            decimals: 1
            unit: V
          - entity: sensor.solarman_load_l3_power
            decimals: 0
            unit: W
          - entity: sensor.solarman_load_l3_voltage
            decimals: 1
            unit: V
          - entity: sensor.solarman_temperature
            unit: °C
          - entity: sensor.solarman_power_losses
            decimals: 0
            unit: W
            name: Losses
          - entity: sensor.solarman_today_losses
            unit: kWh
            name: Losses
          - entity: sensor.solarman_today_load_consumption
            unit: kWh
            name: Consumption
        show:
          horseshoe_style: autominmax
        layout:
          hlines:
            - id: 0
              xpos: 50
              ypos: 40
              length: 70
              styles:
                - stroke: rgb(59, 60, 62);
            - id: 1
              xpos: 50
              ypos: 60
              length: 70
              styles:
                - stroke: rgb(59, 60, 62);
          vlines:
            - id: 0
              xpos: 37
              ypos: 50
              length: 18
              styles:
                - stroke: rgb(59, 60, 62);
                - stroke-linecap: square;
            - id: 1
              xpos: 63
              ypos: 50
              length: 18
              styles:
                - stroke: rgb(59, 60, 62);
                - stroke-linecap: square;
          states:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 34
              styles:
                - font-size: 3em;
                - opacity: 0.9;
            - id: 1
              entity_index: 1
              xpos: 34
              ypos: 49
              styles:
                - font-size: 1.2em;
                - text-anchor: end;
            - id: 2
              entity_index: 2
              xpos: 34
              ypos: 56
              styles:
                - font-size: .9em;
                - text-anchor: end;
            - id: 3
              entity_index: 3
              xpos: 51
              ypos: 49
              styles:
                - font-size: 1.2em;
            - id: 4
              entity_index: 4
              xpos: 51
              ypos: 56
              styles:
                - font-size: .9em;
            - id: 5
              entity_index: 5
              xpos: 66
              ypos: 49
              styles:
                - font-size: 1.2em;
                - text-anchor: start;
            - id: 6
              entity_index: 6
              xpos: 66
              ypos: 56
              styles:
                - font-size: .9em;
                - text-anchor: start;
            - id: 7
              entity_index: 7
              xpos: 50
              ypos: 16
              styles:
                - font-size: .8em;
            - id: 8
              entity_index: 8
              xpos: 50
              ypos: 75
              styles:
                - font-size: 2em;
            - id: 9
              entity_index: 9
              xpos: 100
              ypos: 5
              styles:
                - text-anchor: end;
                - font-size: 1.1em;
            - id: 10
              entity_index: 10
              xpos: 0
              ypos: 5
              styles:
                - text-anchor: start;
                - font-size: 1.1em;
          icons:
            - id: 0
              entity_index: 1
              xpos: 30
              ypos: 52
              align: start
              size: 1
          names:
            - id: 0
              entity_index: 0
              xpos: 50
              ypos: 95
              styles:
                - font-size: 1.2em;
            - id: 1
              entity_index: 8
              xpos: 50
              ypos: 80
              styles:
                - font-size: 0.5em;
            - id: 2
              entity_index: 9
              xpos: 100
              ypos: 9
              styles:
                - font-size: 0.5em;
                - text-anchor: end;
            - id: 3
              entity_index: 10
              xpos: 0
              ypos: 9
              styles:
                - font-size: 0.5em;
                - text-anchor: start;
        horseshoe_scale:
          min: 0
          max: 5000
          width: 6
          color: '#6F6F6F'
        color_stops:
          '0': '#43A047'
          '300': '#DB4437'
        card_mod:
          style: |
            ha-card {
              --ha-card-background: var(--card-background-color);
              color: var(--primary-color); 
            }

``` 
