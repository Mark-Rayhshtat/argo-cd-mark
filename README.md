[ssm-user@ip-10-10-2-145 ~]$ k get cm -n argocd argocd-notifications-cm -o yaml
apiVersion: v1
data:
  context: |
    argocdUrl: https://argocd.auth.rayhshtat.com
  service.slack: |
    token: $slack-token
  service.webhook.jenkins: |
    url: 'https://jenkins.auth.rayhshtat.com'
    headers:
      - name: Content-Type
        value: application/json
    basicAuth:
      username: admin
      password: $jenkins-tocken
  template.app-deployed: |
    message: |
      Application {{.app.metadata.name}} is now running new version of deployments manifests.
    slack:
      attachments: |
        [{
          "title": "{{ .app.metadata.name}}",
          "title_link":"{{.context.argocdUrl}}/applications/{{.app.metadata.name}}",
          "color": "#18be52",
          "fields": [
          {
            "title": "Sync Status",
            "value": "{{.app.status.sync.status}}",
            "short": true
          },
          {
            "title": "Repository",
            "value": "{{.app.spec.source.repoURL}}",
            "short": true
          },
          {
            "title": "Revision",
            "value": "{{.app.status.sync.revision}}",
            "short": true
          },
          {
            "title": "Image",
            "value": "{{index .app.status.summary.images 0}}",
            "short": true
          }
          {{range $index, $c := .app.status.conditions}}
          ,
          {
            "title": "{{$c.type}}",
            "value": "{{$c.message}}",
            "short": true
          }
          {{end}}
          ]
        }]
      deliveryPolicy: Post
      groupingKey: ""
      notifyBroadcast: false
  template.app-health-degraded: |
    message: |
      Application {{.app.metadata.name}} has degraded.
      Application details: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}.
    slack:
      attachments: |
        [{
          "title": "{{ .app.metadata.name}}",
          "title_link": "{{.context.argocdUrl}}/applications/{{.app.metadata.name}}",
          "color": "#f4c030",
          "fields": [
          {
            "title": "Health Status",
            "value": "{{.app.status.health.status}}",
            "short": true
          },
          {
            "title": "Repository",
            "value": "{{.app.spec.source.repoURL}}",
            "short": true
          }
          {{range $index, $c := .app.status.conditions}}
          ,
          {
            "title": "{{$c.type}}",
            "value": "{{$c.message}}",
            "short": true
          }
          {{end}}
          ]
        }]
      deliveryPolicy: Post
      groupingKey: ""
      notifyBroadcast: false
  template.app-sync-failed: |
    message: |
      The sync operation of application {{.app.metadata.name}} has failed at {{.app.status.operationState.finishedAt}} with the following error: {{.app.status.operationState.message}}
      Sync operation details are available at: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}?operation=true .
    slack:
      attachments: |
        [{
          "title": "{{ .app.metadata.name}}",
          "title_link":"{{.context.argocdUrl}}/applications/{{.app.metadata.name}}",
          "color": "#E96D76",
          "fields": [
          {
            "title": "Sync Status",
            "value": "{{.app.status.sync.status}}",
            "short": true
          },
          {
            "title": "Repository",
            "value": "{{.app.spec.source.repoURL}}",
            "short": true
          }
          {{range $index, $c := .app.status.conditions}}
          ,
          {
            "title": "{{$c.type}}",
            "value": "{{$c.message}}",
            "short": true
          }
          {{end}}
          ]
        }]
      deliveryPolicy: Post
      groupingKey: ""
      notifyBroadcast: false
  template.app-sync-running: |
    message: |
      The sync operation of application {{.app.metadata.name}} has started at {{.app.status.operationState.startedAt}}.
      Sync operation details are available at: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}?operation=true .
    slack:
      attachments: |
        [{
          "title": "{{ .app.metadata.name}}",
          "title_link":"{{.context.argocdUrl}}/applications/{{.app.metadata.name}}",
          "color": "#0DADEA",
          "fields": [
          {
            "title": "Sync Status",
            "value": "{{.app.status.sync.status}}",
            "short": true
          },
          {
            "title": "Repository",
            "value": "{{.app.spec.source.repoURL}}",
            "short": true
          }
          {{range $index, $c := .app.status.conditions}}
          ,
          {
            "title": "{{$c.type}}",
            "value": "{{$c.message}}",
            "short": true
          }
          {{end}}
          ]
        }]
      deliveryPolicy: Post
      groupingKey: ""
      notifyBroadcast: false
  template.app-sync-status-unknown: |
    message: |
      Application {{.app.metadata.name}} sync is 'Unknown'.
      Application details: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}.
    slack:
      attachments: |
        [{
          "title": "{{ .app.metadata.name}}",
          "title_link":"{{.context.argocdUrl}}/applications/{{.app.metadata.name}}",
          "color": "#E96D76",
          "fields": [
          {
            "title": "Sync Status",
            "value": "{{.app.status.sync.status}}",
            "short": true
          },
          {
            "title": "Repository",
            "value": "{{.app.spec.source.repoURL}}",
            "short": true
          }
          {{range $index, $c := .app.status.conditions}}
          ,
          {
            "title": "{{$c.type}}",
            "value": "{{$c.message}}",
            "short": true
          }
          {{end}}
          ]
        }]
      deliveryPolicy: Post
      groupingKey: ""
      notifyBroadcast: false
  template.app-sync-succeeded: |
    message: |
      Application {{.app.metadata.name}} has been successfully synced at {{.app.status.operationState.finishedAt}}.
      Sync operation details are available at: {{.context.argocdUrl}}/applications/{{.app.metadata.name}}?operation=true .
    slack:
      attachments: |
        [{
          "title": "{{ .app.metadata.name}}",
          "title_link":"{{.context.argocdUrl}}/applications/{{.app.metadata.name}}",
          "color": "#18be52",
          "fields": [
          {
            "title": "Sync Status",
            "value": "{{.app.status.sync.status}}",
            "short": true
          },
          {
            "title": "Repository",
            "value": "{{.app.spec.source.repoURL}}",
            "short": true
          }
          {{range $index, $c := .app.status.conditions}}
          ,
          {
            "title": "{{$c.type}}",
            "value": "{{$c.message}}",
            "short": true
          }
          {{end}}
          ]
        }]
      deliveryPolicy: Post
      groupingKey: ""
      notifyBroadcast: false
  template.jenkins-on-success: |
    webhook:
      jenkins:
        method: POST
        path: /job/argocd/buildWithParameters?name={{.app.metadata.name}}&project={{.app.spec.project}}&image={{index .app.status.summary.images 0}}&version={{ regexFind "[0-9]+\\.[0-9]+" (index .app.status.summary.images 0) }}
  trigger.jenkins-on-success: |
    - description: Application is synced and healthy. Triggered once per commit.
      oncePer: app.status.operationState?.syncResult?.revision
      send:
      - jenkins-on-success
      when: app.status.operationState != nil and app.status.operationState.phase in ['Succeeded'] and app.status.health.status == 'Healthy' and time.Now().Sub(time.Parse(app.status.operationState.startedAt)).Minutes() >= 1
  trigger.on-deployed: |
    - description: Application is synced and healthy. Triggered once per commit.
      oncePer: app.status.operationState?.syncResult?.revision
      send:
      - app-deployed
      when: app.status.operationState != nil and app.status.operationState.phase in ['Succeeded'] and app.status.health.status == 'Healthy' and time.Now().Sub(time.Parse(app.status.operationState.startedAt)).Minutes() >= 1
  trigger.on-health-degraded: |
    - description: Application has degraded
      send:
      - app-health-degraded
      when: app.status.health.status == 'Degraded'
  trigger.on-sync-failed: |
    - description: Application syncing has failed
      send:
      - app-sync-failed
      when: app.status.operationState != nil and app.status.operationState.phase in ['Error',
        'Failed']
  trigger.on-sync-running: |
    - description: Application is being synced
      send:
      - app-sync-running
      when: app.status.operationState != nil and app.status.operationState.phase in ['Running']
  trigger.on-sync-status-unknown: |
    - description: Application status is 'Unknown'
      send:
      - app-sync-status-unknown
      when: app.status.sync.status == 'Unknown'
  trigger.on-sync-succeeded: |
    - description: Application syncing has succeeded
      send:
      - app-sync-succeeded
      when: app.status.operationState != nil and app.status.operationState.phase in ['Succeeded']
kind: ConfigMap
metadata:
  annotations:
    meta.helm.sh/release-name: argo-cd
    meta.helm.sh/release-namespace: argocd
  creationTimestamp: "2024-11-20T13:35:27Z"
  labels:
    app.kubernetes.io/component: notifications-controller
    app.kubernetes.io/instance: argo-cd
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: argocd-notifications-controller
    app.kubernetes.io/part-of: argocd
    app.kubernetes.io/version: v2.13.0
    helm.sh/chart: argo-cd-7.7.3
  name: argocd-notifications-cm
  namespace: argocd
  resourceVersion: "1833547"
  uid: 680010bb-3297-4007-9fe0-ead1036116f9
[ssm-user@ip-10-10-2-145 ~]$

---------

[ssm-user@ip-10-10-2-145 ~]$ k get secret -n argocd argocd-notifications-secret -o yaml
apiVersion: v1
data:
  jenkins-tocken: <value>
  slack-token: <value>
kind: Secret
metadata:
  annotations:
    meta.helm.sh/release-name: argo-cd
    meta.helm.sh/release-namespace: argocd
  creationTimestamp: "2024-11-20T13:35:27Z"
  labels:
    app.kubernetes.io/component: notifications-controller
    app.kubernetes.io/instance: argo-cd
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: argocd-notifications-controller
    app.kubernetes.io/part-of: argocd
    app.kubernetes.io/version: v2.13.0
    helm.sh/chart: argo-cd-7.7.3
  name: argocd-notifications-secret
  namespace: argocd
  resourceVersion: "1436504"
  uid: 9ef49ad7-5d5d-4187-b39d-468493672ebf
type: Opaque