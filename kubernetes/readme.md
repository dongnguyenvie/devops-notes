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
ReplicaSet là một điều khiển Controller - nó đảm bảo ổn định các nhân bản (số lượng và tình trạng của POD, replica) khi đang chạy.
---
Deployment quản lý một nhóm các Pod - các Pod được nhân bản, nó tự động thay thế các Pod bị lỗi, không phản hồi bằng pod mới nó tạo ra. Như vậy, deployment đảm bảo ứng dụng của bạn có một (hay nhiều) Pod để phục vụ các yêu cầu.

Deployment sử dụng mẫu Pod (Pod template - chứa định nghĩa / thiết lập về Pod) để tạo các Pod (các nhân bản replica), khi template này thay đổi, các Pod mới sẽ được tạo để thay thế Pod cũ ngay lập tức.
---
Các POD được quản lý trong Kubernetes, trong vòng đời của nó chỉ diễn ra theo hướng - được tạo ra, chạy và khi nó kết thúc thì bị xóa và khởi tạo POD mới thay thế. ! Có nghĩa ta không thể có tạm dừng POD, chạy lại POD đang dừng ...

Mặc dù mỗi POD khi tạo ra nó có một IP để liên lạc, tuy nhiên vấn đề là mỗi khi POD thay thế thì là một IP khác, nên các dịch vụ truy cập không biết IP mới nếu ta cấu hình nó truy cập đến POD nào đó cố định. Để giải quết vấn đề này sẽ cần đến Service.

Service (micro-service) là một đối tượng trừu tượng nó xác định ra một nhóm các POD và chính sách để truy cập đến POD đó. Nhóm cá POD mà Service xác định thường dùng kỹ thuật Selector (chọn các POD thuộc về Service theo label của POD).

Cũng có thể hiểu Service là một dịch vụ mạng, tạo cơ chế cân bằng tải (load balancing) truy cập đến các điểm cuối (thường là các Pod) mà Service đó phục vụ.
---
Các Service trước bạn tạo nó có một địa chỉ IP riêng của Service, nó dùng cơ chế cân bằng tải để liên kết với các POD. Tuy nhiên nếu nuốn không dùng cơ chế cân bằng tải, mỗi lần truy cập tên Service nó truy cập thẳng tới IP của PO thì dùng loại Headless Service.

Một Headless Service (Service không IP) nó liên kết thẳng với IP của POD, có nghĩa bạn sẽ không tương tác trực tiếp với POD qua proxy. Tạo loại Service này giống như Service khác, chỉ việc thiết lập thêm .spec.clusterIP có giá trị None
---
DaemonSet (ds) đảm bảo chạy trên mỗi NODE một bản copy của POD. Triển khai DaemonSet khi cần ở mỗi máy (Node) một POD, thường dùng cho các ứng dụng như thu thập log, tạo ổ đĩa trên mỗi Node ... Dưới đây là ví dụ về DaemonSet, nó tạo tại mỗi Node một POD chạy nginx
---
Job (jobs) có chức năng tạo các POD đảm bảo nó chạy và kết thúc thành công. Khi các POD do Job tạo ra chạy và kết thúc thành công thì Job đó hoàn thành. Khi bạn xóa Job thì các Pod nó tạo cũng xóa theo. Một Job có thể tạo các Pod chạy tuần tự hoặc song song. Sử dụng Job khi muốn thi hành một vài chức năng hoàn thành xong thì dừng lại (ví dụ backup, kiểm tra ...)

Khi Job tạo Pod, Pod chưa hoàn thành nếu Pod bị xóa, lỗi Node ... nó sẽ thực hiện tạo Pod khác để thi hành tác vụ.
---
CronJob (cj) - chạy các Job theo một lịch định sẵn. Việc lên lịch cho CronJob khai báo giống Cron của Linux. Xem Sử dụng Cron, Crontab từ động chạy script trên Server Linux
---
Ingress là thành phần được dùng để điều hướng các yêu cầu traffic giao thức HTTP và HTTPS từ bên ngoài (interneet) vào các dịch vụ bên trong Cluster.

```

### Cronbtab

![cronbtab](./assests/crontab.png)

```
Vậy mỗi dòng thường có 6 cột dữ liệu, 5 cột đầu để xác định thời điểm chạy (thời gian). Cột thứ 6 là lệnh chạy (thường là một script).

Các cột thời gian, loại thời gian nào luôn xảy ra để dấu * (ví dụ mọi phút thì cột phút để dấu *, mọi giờ thì cột giờ để *, mọi ngày thì cột ngày để * ....) còn muốn xảy ra ở một thời điểm cụ thể thì điền thời điểm đó vào.
```

### Keywords:

```
K02 - Cài đặt và sử dụng Kubernetes Dashboard by XuanThuLab 21 minutes

K03 - Sử dụng công cụ K9S quản lý K8S - Kubernetes

K06 - POD trong Kubernetes, Pod nhiều container, Volume trong POD (phần 2)

K07 - ReplicaSet và HPA trong Kubernetes

K08 - Deployment trong Kubernetes triển khai cập nhật và scale

K09 - Triển khai Metrics Server trên Kubernetes

K10 - Sử dụng Service và Secret Tls trong Kubernetes

K11 - DaemonSet, Job, CronJob trong Kubernetes

K12 - Sử dụng Persistent Volume (pv) và Persistent Volume Claim (pvc) trong Kubernetes

K13 - Sử dụng Persistent Volume (pv) NFS trong kubernetees

K14 - Sử dụng Ingress trong Kubernetes

K15 - Triển khai NGINX Ingress Controller trong Kubernetes

Sử dụng Rancher để quản lý Kubernetes Cluster
```
