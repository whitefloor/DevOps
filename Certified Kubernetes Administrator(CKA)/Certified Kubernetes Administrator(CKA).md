# Certified Kubernetes Administrator(CKA)
A note to record useful tips for CKA

## 通用小技巧
* 官方文件：https://kubernetes.io/docs/home/
```
最重要，一定要習慣在官方文件查資料，考試的時候有限定只能從哪裡翻閱資料
官方文件裡面也有關於主題的大量敘述跟技巧
```

* k8s 好用通俗習慣：https://kubernetes.io/docs/reference/kubectl/conventions/
### Command
* 解釋資源的字段解釋與結構說明：`kubectl explain rs`
* 簡易創造 YAML 方法：`kubectl create rs --dry-run=client -o > rs.yaml`

## Pod
* k8s 中最小的資源單位
### Command
* 

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
