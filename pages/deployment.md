# Deployment
Deployment định nghĩa nên các POD và ReplicaSet. Khi một deployment được tạo ra thì một ReplicaSet sẽ được tạo ra và từ ReplicaSet nó sẽ quản lý số lượng POD được sinh ra từ Deployment.

*Ví dụ: nginx-deployment.yaml*
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
Trong ví dụ này
* Một Deplyment có tến `nginx-deployment` được tạo ra và nó được chỉ định bở trường `.metadata.name`.
* Deployment sẽ tạo ra 3 bản sao (replica) được chỉ định bởi trường `.spec.replicas`.
* Trường `.spec.selector` định nghĩa cách mà Deployment tìm kiếm những POD thuộc sự quản lý của nó. Trong trường hợp này, thì nó sẽ chọn các POD có nhãn trong POD template (`app: nginx`).

## Các trường (field) trong deployment
|Filed|Kiểu dữ liệu giá trị|Ý nghĩa|
|-----|-----|-------|
|`.spec.minReadySeconds`|integer|Thời gian tối thiểu (số giây) mà Pod mới được tạo với trạng thái READY, mặc định là 0. Có nghĩa là thời gian nó tối thiểu nó được tạo ra và chính thức được sử dụng.|
|`.spec.paused`|boolean|Cho biết deployment đã tạm dừng hay chưa|
|`.spec.progressDeadlineSeconds`|integer|Thời gian tối đa để Deployment triển khai, nếu vượt quá thời gian này mà nó chưa vào trạng thái Progress thì nó sẽ sang trạng thái Failed, nhưng nó sẽ không tính thời gian Deployment bị tạm dừng. Giá trị mặc định là 600|
|`.spec.repilcas`|integer|Quy định số bản sao của POD, giá trị mặc định là 1|
|`.spec.revisionHistoryLimit`|integer|Giới hạn số bản ghi lịch sử của ReplicaSets, để dễ dàng rollback, giá trị mặc định là 10|
|`.spec.selector`|object|field này nó phải match với lại field `.spec.template.metadata.labels` nếu không nó sẽ bị reject bới API. Nhờ vào trường này mà nó nhận biết được các POD mà nó đang quản lý.|
|`.spec.strategy`|object|Chỉ định chiến lược sử dụng các POD mới để thay thế cho các POD cũ. Có 2 chiến lược: "Recreate" và "RollingUpdate" trong đó "RollingUpdate" là giá trị mặc định|
|`.spec.template`|object|Chỉ định nên kịch bản xây dựng nên các POD|
* Chú ý: Trong để check được status, reason được đề cập bên trên thì dùng câu lệnh `kubectl describe deploy/<deploy-name>`
* Link tham khảo: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
