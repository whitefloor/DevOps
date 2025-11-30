# Certified Kubernetes Administrator(CKA)
A note to record useful tips for CKA

## 事前準備
1. 折價券（尤其是黑五折扣）：https://github.com/techiescamp/linux-foundation-coupon
2. 課程推薦：Udemy 上的 Mumshad Mannambeth 講師，我是更新考試附近的時候買的  
   課程還有更新 2025 的考試內容、沒有英文口音、可以用中文翻譯字幕、還有免費 LAB

## 考試規則
1. 第一次沒通過，12個月內可以重考

## 通用小技巧
* 官方文件：https://kubernetes.io/docs/home/
```
最重要，一定要習慣在官方文件查資料，考試的時候有限定只能從哪裡翻閱資料
官方文件裡面也有關於主題的大量敘述跟技巧
```

* k8s 好用通俗習慣：https://kubernetes.io/docs/reference/kubectl/conventions/
### Command
* 解釋資源的字段解釋與結構說明：kubectl explain rs
* 簡易創造 YAML 方法：kubectl create rs --dry-run=client -o > rs.yaml

## Pod
* k8s 中最小的資源單位
### Command
* k get pod -o wide
* k run podName --image=nginx
* k describe pod podName
* k create -f xxx.yaml

## ReplicaSet
* 維護一組穩定數量的 Pod
* 通常會由 Deployment 管理 ReplicaSet
* 可以用 HAP 進行水平擴容的目標
* ReplicaSet 是透過 selector 來進行管理的，selector 與 Label 不一致會無法佈署
### Command
* k get rs
```
注意，會有問題現在有幾個 ReplicaSet，是看 ReplicaSet 的數量而不是 Pod有多少個
```
* k scale rs rsName --replicas=2
```
這樣不用 edit 進去修改 YAML 就能執行 scale
```

## Deployment
* 佈署 Pod 用的
* 通常還會搭配 Replicaset 來管理 scale
### Command
* k describe deploy
* k create deployment nginx --image=nginx --replicas=4
* k scale deployment nginx --replicas=4

## Service
* 將你的 Pod 暴露端口在網路上的管理方法
### Command
* k describe svc <service-name>
* k expose pod redis --port=6379 --name=redis-service --type=ClusterIP
* k run httpd --image=httpd:alpine --port=80 --expose

## Namespace
* 用來隔離、呼叫資源的方式
* 在 Cluster 的範圍底下並不適用
### Command
* k create namespace <namespace-name>
* k get pods --namespace kube-system

## Scheduling
* 用來確定pod和node能夠匹配
* kube-scheduler是k8s預設的Scheduler
* Pod.yaml > 底下的 spec field > 定義 nodeName: nodeName
### Command
* k replace --force -f nginx.yaml

## Selector / Label
* 對資源進行篩選、標記

## Taint / Tolerations
* 在進行 Scheduling 的時候來控管調度的方法
* 被 Taint 的 Node，Pod 要 Toleration 才能調度
* 通常和 Node Affinity 搭配使用

## Node Affinity
* 在進行 Scheduling 的時候來控管調度的方法
* 在 Pod 上設定對哪些 Node 具有 Affinity 進行調度
* 通常和 Taint / Toleration 搭配使用
* 配置 YAML 要參考官方文檔

## Resource Limits
* 在 Yaml 中限制 Pod 能夠用多少的資源量
* Resource Limit就是修改文件敘述然後 Replace Pod 即可生效

## Demonsets
* 設定守護背景程序，會自動由 k8s 回收及調度
* 從手動調度的東西變成 k8s 處理的方法
* Demonsets 創建的時候可以用 Deployment 的模版  
  然後刪除 status / replica / strategy 即可使用

## Static Pod
* YAML 只能在 /etc/kubernetes/manifests 看到
* 如果預設路徑找不到 YAML，到 /var/lib/kubelet/config.yaml 看 Static Pod 的路徑在哪

## Priority Class
* 用來決定 Pod 內調度的優先及別
* 如果要更正pod的priority，不能直接改，要用priorityclass指定檔案重啟pod
### Command
* kubectl get priorityclass

## Multiple Schedulers
* 用來過濾、校驗、修正對 API Server 的請求，在進入 RBAC 裡面之前調用
* 可以在 Pod Yaml 指定要運行的scheduler:`schedulerName: yourSchedulerName`
### Command
* kubectl get pods --namespace=kube-system