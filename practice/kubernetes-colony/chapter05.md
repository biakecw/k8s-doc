# kube-controller-manager部署

kube-controller-manager.service

```shell
cat > kube-controller-manager.service << \EOF
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/GoogleCloudPlatform/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-controller-manager   \
--address=0.0.0.0   \
--master=http://127.0.0.1:8080   \
--allocate-node-cidrs=true   \
--service-cluster-ip-range=10.1.0.0/16   \
--cluster-cidr=10.2.0.0/16   \
--cluster-name=kubernetes   \
--cluster-signing-cert-file=/etc/kubernetes/ssl/ca.pem   \
--cluster-signing-key-file=/etc/kubernetes/ssl/ca-key.pem   \
--service-account-private-key-file=/etc/kubernetes/ssl/ca-key.pem   \
--root-ca-file=/etc/kubernetes/ssl/ca.pem   \
--leader-elect=true   \
--v=2   \
--logtostderr=false   \
--log-dir=/var/log/kubernetes/

Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```
**参数**
- 其中还可以加`--service-cluster-ip-range`必须和`kube-apiserver`一致

启动 `kube-controller-manager`

```shell
mv kube-controller-manager.service /usr/lib/systemd/system/
systemctl daemon-reload
systemctl enable kube-controller-manager
systemctl start kube-controller-manager
systemctl status kube-controller-manager
```

## 检查

```
kubectl get componentstatuses
```
