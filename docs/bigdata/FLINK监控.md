# FLINK监控

flink监控借助组件**`pushgateway`**、**`Pushgateway`**、**`Grafana`**完成

##### 1、配置flink任务

其中flink-conf.yaml文件夹添加以下配置:

```yaml
##### 与Prometheus集成配置 #####
metrics.reporter.promgateway.class: org.apache.flink.metrics.prometheus.PrometheusPushGatewayReporter
metrics.reporter.promgateway.host: xxx
metrics.reporter.promgateway.port: 9091
metrics.reporter.promgateway.jobName: flink-metrics
metrics.reporter.promgateway.randomJobNameSuffix: true
metrics.reporter.promgateway.deleteOnShutdown: true
metrics.reporter.promgateway.interval: 60 SECONDS
```

flink启动参数添加以下几个参数：

````
-yD metrics.reporter.promgateway.groupingKey="jobname=your job name" 
-yD metrics.reporter.promgateway.jobName=your job name
````

在grafana  dashboards中就可以看到相应的flink任务


##### 2、配置监控预警

在grafana-Alterting中可以配置预警模板和触发预警的条件。

预警模板配置如下：

```GO
{{ define "wechat.default.title" -}}{{ if gt (len .Alerts.Firing) 0 -}}<font color="warning">Grafana告警:</font>{{- end }}{{ if gt (len .Alerts.Resolved) 0 -}}<font color="info">Grafana告警解除:</font>{{- end }}{{- end }}
{{ define "wechat.default.message" }}{{ if gt (len .Alerts.Firing) 0 -}}{{- range .Alerts -}}>触发警报: <font color="comment">{{ .Labels.alertname }}</font>
  >当前状态: <font color="warning">{{ .Status }}</font>
  >触发时间: <font color="comment">{{ .StartsAt.Format "2006-01-02 15:04:05" }}</font>
  >警报链接: <font color="comment">{{ .GeneratorURL }}</font>
  >事件信息: {{ range .Annotations.SortedPairs }}
  {{ .Name }}: {{ .Value }}{{- end }}{{- end }}{{- end }}{{ if gt (len .Alerts.Resolved) 0 -}}{{- range .Alerts -}}>触发警报: <font color="comment">{{ .Labels.alertname }}</font>
  >当前状态: <font color="info">{{ .Status }}</font>
  >触发时间: <font color="comment">{{ .StartsAt.Format "2006-01-02 15:04:05" }}</font>
  >恢复时间: <font color="comment">{{ .EndsAt.Format "2006-01-02 15:04:05" }}</font>
  >事件信息: {{ range .Annotations.SortedPairs }}
  {{ .Name }}: {{ .Value }}{{- end }} {{- end }}{{- end }}{{- end }}
```

如需添加额外事件可以参考Go templating language

触发预警的条件可以在flink dashboards点击你想监控的事件图表，点击Edit，在Alert创建新的rule


选择想要触发的指标和触发时间，点击保存即可。