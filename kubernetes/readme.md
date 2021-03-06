### Auto scale

```
hba(HorizontalPodAutoscaler) > rs(ReplicaSet, it manage pods by label) > pod
deploy -> rs -> pods

hba, rs, pods
kubectl get no -o wide
kubectl get pods -o wide
kubectl get rs
kubectl get pv
kubectl get pvc
kubectl get all -o wide
kubectl get hpa -o wide
kubectl apply -f [File_path]
kubectl edit
kubectl get po -l "app=app1" -o wide

kubectl get ingress -n ingress-controller

kubectl get ns
kubectl create ns ingress-controller

kubectl label node master.xtl role=ingress-controller

kubectl get secret -n kubernetes-dashboard

kubectl rollout history deploy/deployapp  --revision=2
kubectl rollout undo deploy/deployapp  --revision=2
kubectl scale deploy/deployapp --replicas=5 (manually way)
kubectl autoscale deploy/deployapp --min=3 --max=8 (autoscale way, this script will create HPA)
kubectl get hpa/deployapp -o yaml
kubectl get hpa/deployapp -o yaml > 2.hpa.yaml (save yaml to file)

kubectl get endpoints

docker build -t dongnguyenvie/public:kb_nginx -f Dockerfile
docker push dongnguyenvie/public:kb_nginx

kubectl create secret tls dongnguyen-test --cert=fullchain.pem --key=privkey.pem -n ingress-controller

https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/
```

### metric

```
https://github.com/kubernetes-sigs/metrics-server
use Metric for analyzing and show infomation CPU into monitor
and the important thing, setup metric for HPA to use for check CPU and scale system according to CPU,...

metric
kubectl top node
kubectl top pod
```

### Service

```
forward requuesting to pods
kubectl get svc
kubectl get svc/svc1 -o yaml
if the service don't define target pods(endpoint), it will pick endpoint the same name.
case1: it's active the same proxy, it will forward requesting to another server
case2: it will pick pods, pods will be endpoint of it
```

### Terminal

```
mac host path: /etc/hosts
watch -n 1 [shell]
yum install nfs-utils

openssl req -x509 -newkey rsa:2048 -nodes -days 365 -keyout privkey.pem -out fullchain.pem -subj '/CN=dongnguyen.test'
kubectl create secret tls dongnguyen-test --cert=fullchain.pem --key=privkey.pem -n ingress-controller

```

## vim

```
exist: esc
insert: i
save and exist: :wq
```

### Note

```
pods was created by replicateRest or deployment
---
ReplicaSet l?? m???t ??i???u khi???n Controller - n?? ?????m b???o ???n ?????nh c??c nh??n b???n (s??? l?????ng v?? t??nh tr???ng c???a POD, replica) khi ??ang ch???y.
---
Deployment qu???n l?? m???t nh??m c??c Pod - c??c Pod ???????c nh??n b???n, n?? t??? ?????ng thay th??? c??c Pod b??? l???i, kh??ng ph???n h???i b???ng pod m???i n?? t???o ra. Nh?? v???y, deployment ?????m b???o ???ng d???ng c???a b???n c?? m???t (hay nhi???u) Pod ????? ph???c v??? c??c y??u c???u.

Deployment s??? d???ng m???u Pod (Pod template - ch???a ?????nh ngh??a / thi???t l???p v??? Pod) ????? t???o c??c Pod (c??c nh??n b???n replica), khi template n??y thay ?????i, c??c Pod m???i s??? ???????c t???o ????? thay th??? Pod c?? ngay l???p t???c.
---
C??c POD ???????c qu???n l?? trong Kubernetes, trong v??ng ?????i c???a n?? ch??? di???n ra theo h?????ng - ???????c t???o ra, ch???y v?? khi n?? k???t th??c th?? b??? x??a v?? kh???i t???o POD m???i thay th???. ! C?? ngh??a ta kh??ng th??? c?? t???m d???ng POD, ch???y l???i POD ??ang d???ng ...

M???c d?? m???i POD khi t???o ra n?? c?? m???t IP ????? li??n l???c, tuy nhi??n v???n ????? l?? m???i khi POD thay th??? th?? l?? m???t IP kh??c, n??n c??c d???ch v??? truy c???p kh??ng bi???t IP m???i n???u ta c???u h??nh n?? truy c???p ?????n POD n??o ???? c??? ?????nh. ????? gi???i qu???t v???n ????? n??y s??? c???n ?????n Service.

Service (micro-service) l?? m???t ?????i t?????ng tr???u t?????ng n?? x??c ?????nh ra m???t nh??m c??c POD v?? ch??nh s??ch ????? truy c???p ?????n POD ????. Nh??m c?? POD m?? Service x??c ?????nh th?????ng d??ng k??? thu???t Selector (ch???n c??c POD thu???c v??? Service theo label c???a POD).

C??ng c?? th??? hi???u Service l?? m???t d???ch v??? m???ng, t???o c?? ch??? c??n b???ng t???i (load balancing) truy c???p ?????n c??c ??i???m cu???i (th?????ng l?? c??c Pod) m?? Service ???? ph???c v???.
---
C??c Service tr?????c b???n t???o n?? c?? m???t ?????a ch??? IP ri??ng c???a Service, n?? d??ng c?? ch??? c??n b???ng t???i ????? li??n k???t v???i c??c POD. Tuy nhi??n n???u nu???n kh??ng d??ng c?? ch??? c??n b???ng t???i, m???i l???n truy c???p t??n Service n?? truy c???p th???ng t???i IP c???a PO th?? d??ng lo???i Headless Service.

M???t Headless Service (Service kh??ng IP) n?? li??n k???t th???ng v???i IP c???a POD, c?? ngh??a b???n s??? kh??ng t????ng t??c tr???c ti???p v???i POD qua proxy. T???o lo???i Service n??y gi???ng nh?? Service kh??c, ch??? vi???c thi???t l???p th??m .spec.clusterIP c?? gi?? tr??? None
---
DaemonSet (ds) ?????m b???o ch???y tr??n m???i NODE m???t b???n copy c???a POD. Tri???n khai DaemonSet khi c???n ??? m???i m??y (Node) m???t POD, th?????ng d??ng cho c??c ???ng d???ng nh?? thu th???p log, t???o ??? ????a tr??n m???i Node ... D?????i ????y l?? v?? d??? v??? DaemonSet, n?? t???o t???i m???i Node m???t POD ch???y nginx
---
Job (jobs) c?? ch???c n??ng t???o c??c POD ?????m b???o n?? ch???y v?? k???t th??c th??nh c??ng. Khi c??c POD do Job t???o ra ch???y v?? k???t th??c th??nh c??ng th?? Job ???? ho??n th??nh. Khi b???n x??a Job th?? c??c Pod n?? t???o c??ng x??a theo. M???t Job c?? th??? t???o c??c Pod ch???y tu???n t??? ho???c song song. S??? d???ng Job khi mu???n thi h??nh m???t v??i ch???c n??ng ho??n th??nh xong th?? d???ng l???i (v?? d??? backup, ki???m tra ...)

Khi Job t???o Pod, Pod ch??a ho??n th??nh n???u Pod b??? x??a, l???i Node ... n?? s??? th???c hi???n t???o Pod kh??c ????? thi h??nh t??c v???.
---
CronJob (cj) - ch???y c??c Job theo m???t l???ch ?????nh s???n. Vi???c l??n l???ch cho CronJob khai b??o gi???ng Cron c???a Linux. Xem S??? d???ng Cron, Crontab t??? ?????ng ch???y script tr??n Server Linux
---
Ingress l?? th??nh ph???n ???????c d??ng ????? ??i???u h?????ng c??c y??u c???u traffic giao th???c HTTP v?? HTTPS t??? b??n ngo??i (interneet) v??o c??c d???ch v??? b??n trong Cluster.

```

### Cronbtab

![cronbtab](./assests/crontab.png)

```
V???y m???i d??ng th?????ng c?? 6 c???t d??? li???u, 5 c???t ?????u ????? x??c ?????nh th???i ??i???m ch???y (th???i gian). C???t th??? 6 l?? l???nh ch???y (th?????ng l?? m???t script).

C??c c???t th???i gian, lo???i th???i gian n??o lu??n x???y ra ????? d???u * (v?? d??? m???i ph??t th?? c???t ph??t ????? d???u *, m???i gi??? th?? c???t gi??? ????? *, m???i ng??y th?? c???t ng??y ????? * ....) c??n mu???n x???y ra ??? m???t th???i ??i???m c??? th??? th?? ??i???n th???i ??i???m ???? v??o.
```

### Keywords:

```
K02 - C??i ?????t v?? s??? d???ng Kubernetes Dashboard

K03 - S??? d???ng c??ng c??? K9S qu???n l?? K8S - Kubernetes

K06 - POD trong Kubernetes, Pod nhi???u container, Volume trong POD (ph???n 2)

K07 - ReplicaSet v?? HPA trong Kubernetes

K08 - Deployment trong Kubernetes tri???n khai c???p nh???t v?? scale

K09 - Tri???n khai Metrics Server tr??n Kubernetes

K10 - S??? d???ng Service v?? Secret Tls trong Kubernetes

K11 - DaemonSet, Job, CronJob trong Kubernetes

K12 - S??? d???ng Persistent Volume (pv) v?? Persistent Volume Claim (pvc) trong Kubernetes

K13 - S??? d???ng Persistent Volume (pv) NFS trong kubernetees

K14 - S??? d???ng Ingress trong Kubernetes

K15 - Tri???n khai NGINX Ingress Controller trong Kubernetes

S??? d???ng Rancher ????? qu???n l?? Kubernetes Cluster
```
