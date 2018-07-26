# Alert Manager
## 設定步驟
1. Peoject-> Resources-> Config Maps
到 prometheus-server 設定 ConfigMap
![](./Alert_Manager/1.png)
2. 將設定寫在 alerts 欄位
![](Alert_Manager\2.png)
3. 到 LoadBalancing 會找到三個管理網頁的網址，選 prometheus-server 的 URL
![](Alert_Manager\3.PNG)
4. 再到 Status-> Rule 可以找到剛剛設定的 Rule
![](Alert_Manager\4.PNG)
5. 等一段時間後，prometheus-alertmanageer 的 頁面，可以看到被觸發的警告事件
![](Alert_Manager\5.PNG)


## Rule example
- `groups.rules[].alert`  
alert名稱  
- `groups.rules[].expr`  
監控條件  (這裡設為 10 模擬異常狀態)
- `groups.rules[].for`  
符合條件，多久後視為異常狀態
- `groups.rules[].annotations`
警告詳細敘述 (會顯示在 AlertManager 管理頁面上)
```yaml
groups:
- name: test-rule
  rules:
  # --- alert定義 ---
  - alert: NodeFilesystemUsage
    expr: avg by (instance, kubernetes_name, kubernetes_namespace)((node_filesystem_size{device="/dev/sda2"} - node_filesystem_free{device="/dev/sda2"}) / node_filesystem_size{device="/dev/sda2"} * 100) > 10  
    for: 1m
    labels:
      team: node
    annotations:
      summary: "{{$labels.instance}}: High Filesystem usage detected"
      description: "{{$labels.instance}}: Filesystem usage is above 80% (current value is: {{ $value }}"
  # ----------
  - alert: NodeMemoryUsage
    expr: (node_memory_MemTotal - (node_memory_MemFree+node_memory_Buffers+node_memory_Cached )) / node_memory_MemTotal * 100 > 80
    for: 1m
    labels:
      team: node
    annotations:
      summary: "{{$labels.instance}}: High Memory usage detected"
      description: "{{$labels.instance}}: Memory usage is above 80% (current value is: {{ $value }}"
  # ----------
  - alert: NodeCPUUsage
    expr: (100 - (avg by (instance) (irate(node_cpu{mode="idle"}[5m])) * 100)) > 80
    for: 2m
    labels:
      team: node
    annotations:
      summary: "{{$labels.instance}}: High CPU usage detected"
      description: "{{$labels.instance}}: CPU usage is above 80% (current value is: {{ $value }}"
```

