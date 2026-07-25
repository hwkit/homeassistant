# Home Assistant (Hongkong bus routing information)
# Edition:2026-07-24
Getting kmb/ ctb information from public api.

# Requirement:<br>

Install: 

    1. HACS (Home Assistant Community Store)
       - Install Public Transport Departures
       - Departures Card 

# Setup configuration.yaml
    - Add sendor:
        sensor:
          - platform: rest
            name: {{ name of sensor }}
            resource: https://data.etabus.gov.hk/v1/transport/kmb/eta/{bus stop}/{route}/1
            scan_interval: 30
            value_template: "OK"
            json_attributes:
              - data
              
    - Add template sensor
        template:
          - sensor:
              - name: {{ name of template }}
                state: >
                  {% set raw = state_attr('sensor.{{ name of sensor }}', 'data') %}
                  {% if raw and raw | length > 0 %}
                    {{ raw | selectattr('route', 'eq', '{route}') | selectattr('eta', 'ne', None) | list | length }} departures
                  {% else %}
                    No service
                  {% endif %}
                attributes:
                  transport: BUS
                  direction: {{ name of director }}
                  line_id: {{ route id }}
                  line_name: {{ route }}
                  times: >-
                    {% set raw = state_attr('sensor.{{ name of sensor }}', 'data') %}
                    {% set ns = namespace(departures=[]) %}
                    {% if raw %}
                      {% for bus in raw if bus.route == '{route}' and bus.eta %}
                        {% set ns.departures = ns.departures + [{
                          "trip_id": bus.eta ~ bus.route, 
                          "line": { route },
                          "head_sign": bus.dest_tc,
                          "planned": bus.eta,
                          "estimated": bus.eta,
                          "delay": 0,
                          "cancel": false,
                          "alert": false,
                          "direction": bus.dir | default(''),
                          "remark": bus.rmk_tc | default('')
                        }] %}
                      {% endfor %}
                    {% endif %}
                    {{ ns.departures }}

** Change {{ name of sensor }}
** Change {{ name of template }}
** Change {route} : 巴士編號
** {{ route id }} : route unique name.

# How to use ..
1. Dashboard --> Edit --> New
2. 檢視所有面板
3. Departures Card
4. 在 Entities(required) 加
5. 巴士路線

# 有關資料
1. https://data.gov.hk
2. 九巴找尋巴士站資料
   - 
4. 城巴找尋巴士詀資料
   - https://rt.data.gov.hk//v1/transport/citybus-nwfb/route-stop/{co}/{route}/{inbound/outbound}
     - {co}：營運巴士公司名稱 (ctb/)
     - {route}： 巴士編號, ...
     - {inbound/outboud}: 出/入
   - 回應：
     - {"co": "CTB", "route": "952", "dir": "O", "seq": 1, "stop": "001913", "data_timestamp": "2026-07-22T05:00:03+08:00"}
     - co : 城巴/新巴
     - route : 路線
     - dir: 方向 (0/1)
     - seq: 巴士站次序
     - stop: 巴士站編號
     - date_timestamp: 取得資料時間
6. Departures Card 資料配對

   

