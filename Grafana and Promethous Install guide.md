# Rancher 2.0 上架設 Grafana, Prometheus, AlertManager

### 準備作業

先檢查有沒有開啟 Rancher 的 Catalog 功能，預設是開啟的
![1](Grafana_and_Promethous_Install_guide\1.PNG)

---

### 安裝
1.選好要安裝的位置 Cluster(test) > Project(Default)  
進入 Catalog Apps，點選 Launch


![1](Grafana_and_Promethous_Install_guide\2.PNG)
2. 找到安裝 prometheus，點選 View Details


![2](Grafana_and_Promethous_Install_guide\3.PNG)
3. 這裡要設定一些參數(**\*必填**)，也可以直接匯入已經設定好的 YAML 檔
   - `Grafana Admin Password` 記得設定，之後才能用來登入 Dashboard

   - `Persistent Volume` 依需求選擇，可選可不選

   - `Expose Prometheus using Layer 7 Load Balancer` 是否透過 Ingress 來開放管理介面，選 `True`

   - `Hostname` 選 `Automatic generate a .xip.io hostname` ，

     也可以用自己的 domain name，但是就要有 DNS 能解析到這個位置

![3-2](Grafana_and_Promethous_Install_guide\4.png)
完成之後按 Launch

4.接著會跳回 Catalog Apps 那頁，等到右上角出現綠色的 Active 之後就成功了！

![5](Grafana_and_Promethous_Install_guide\5.PNG)



到 Load Balancing 會看到三條規則，中間的 domain 可以連到各個使用介面
![6](Grafana_and_Promethous_Install_guide\6.PNG)

---
