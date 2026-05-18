# PCAI Pod Restart Info Collector

A simple K8s customer controller that watches for Pods changes and collects K8s Pod restart reasons, logs, and events to Slack channel when Pods restart in HPE Private Cloud AI environment.

Download the latest helm chart via this [link](https://github.com/airwallex/k8s-pod-restart-info-collector/tree/master/helm) developed and maintained by *Airwallex*.

## Changes required for PCAI

   * Update application helm chart *values.yaml* file with *ezua.virtualService* section.
   * Configure Application *istio* VirtualService
   * Configure *Kyverno* ClusterPolicy to add *hpe-ezua.labels* in the helm chart workloads

## Helm Parameters

| Name                                | Description                                        | Value         |
| ------------------------------------| -------------------------------------------------- | ------------- |
| `clusterName`                       | K8s cluster name (Display on slack message)                        | required         |
| `slackUsername`                     | Slack username (Display on slack message) | default: `"k8s-pod-restart-info-collector"`          |
| `slackChannel`                      | Slack channel name | default: `"restart-info-nonprod"`          |
| `muteSeconds`                       | The time to mute duplicate pod alerts | default: `"600"`    
| `ignoreRestartCount`                | The number of pod restart count to ignore | default: `"30"`
| `ignoredNamespaces`                 | A set of namespaces to be ignored. This should be provided as a comma-separated list or a regular expression. | default: `""`    
| `ignoredPodNamePrefixes`            | A set of pod name prefixes to be ignored. This should be provided as a comma-separated list or a regular expression. | default: `""`   
| `watchedNamespaces`                 | A set of namespaces to be watched. This should be provided as a comma-separated list or a regular expression. | default: `""`    
| `watchedPodNamePrefixes`            | A set of pod name prefixes to be watched. This should be provided as a comma-separated list or a regular expression. | default: `""`   
| `ignoreRestartsWithExitCodeZero`    | Whether restart events with an exit code of 0 should be ignored | default: `false`
| `slackWebhookUrl`                   | Slack webhook URL | required if slackWebhooUrlSecretKeyRef is not present                       |
| `slackWebhookurlSecretKeyRef.key`   | Slack webhook URL SecretKeyRef.key                 | |
| `slackWebhookurlSecretKeyRef.name`  | Slack webhook URL SecretKeyRef.name                | |

## FAQ

1. When will the collector send Pod restart messages to Slack channel?

   When a Pod restarts. However, if one of the following conditions is met, the messages are not sent.
   1. Pod restartCount > 30
   2. In the previous 10 minutes, the same Pod restart message was sent

2. How to customize slack channel for each pods

   Adding `alert-slack-channel: "your-slack-channel-name"` to Pod annotations or labels.
   For example, a label: `alert-slack-channel: "pcai-restart-info"`
