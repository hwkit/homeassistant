# Home Assistant (Hongkong bus routing information)
# Edition:2026-07-24
Getting kmb/ ctb information from public api.

# Requirement:<br>

Install: 

    1. HACS (Home Assistant Community Store)
       - Install Public Transport Departures
       - Departures Card 

# Setup configuration.yaml
    - Add sensor
    
    - Add template

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

   

