Livenesss Readiness

Start Checking After = initialDelaySeconds

Check Interval = periodSeconds





```yaml
groups:
- name: test-rule
  rules:
  # --- alert定義 ---
  - alert: NodeFilesystemUsage  # alert名稱
    expr: avg by (instance, kubernetes_name, kubernetes_namespace)((node_filesystem_size{device="/dev/sda2"} - node_filesystem_free{device="/dev/sda2"}) / node_filesystem_size{device="/dev/sda2"} * 100) > 80  # 監控條件
    for: 1m
    labels:
      team: node
    annotations:  # 警告敘述
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

