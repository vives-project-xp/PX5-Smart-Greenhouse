````YAML
# Loads default set of integrations. Do not remove.
default_config:

# Load frontend themes from the themes folder
frontend:
  themes: !include themes/mytheme/mytheme.yaml

  extra_module_url:
    - /hacsfiles/lovelace-card-mod/card-mod.js

automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml


mqtt:
  sensor:
    
  - name: "Vat Afstand"
    state_topic: "sensor/afstand/cm"
    unit_of_measurement: "cm"
    value_template: "{{ value | float }}"
    device_class: distance
  
  - name: "Vat Inhoud"
    state_topic: "sensor/vat/liter"
    unit_of_measurement: "L"
    value_template: "{{ value | float }}"
    device_class: volume

template:
  - sensor:
      - name: "Inhoud Vat Dynamisch"
        unique_id: inhoud_vat_dynamisch
        state: >
          {% set vorm = states('input_select.vat_vorm') %}
          {% set hoogte = states('input_number.vat_hoogte_cm') | float(0) %}
          {% set afstand = states('sensor.vat_afstand') | float(999) %}
          {% set niveau = [hoogte - afstand, 0] | max %}

          {% if vorm == 'Cilinder' %}
            {% set diameter = states('input_number.vat_diameter_cm') | float(0) %}
            {% set radius = diameter / 2 %}
            {% set inhoud_cm3 = 3.1416 * (radius ** 2) * niveau %}
          {% elif vorm == 'Balk' %}
            {% set lengte = states('input_number.vat_lengte_cm') | float(0) %}
            {% set breedte = states('input_number.vat_breedte_cm') | float(0) %}
            {% set inhoud_cm3 = lengte * breedte * niveau %}
          {% else %}
            {% set inhoud_cm3 = 0 %}
          {% endif %}

          {{ (inhoud_cm3 / 1000) | round(2) }}
        unit_of_measurement: "L"
        device_class: volume
        state_class: measurement
