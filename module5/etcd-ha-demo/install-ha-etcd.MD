### update your golang to 1.15+
### install cfssl
```
apt install golang-cfssl
```
### generate tls certs
```
mkdir /root/go/src/github.com/etcd-io
cd /root/go/src/github.com/etcd-io
```
```
git clone https://github.com/etcd-io/etcd.git
```
```
cd etcd/hack/tls-setup
```
### edit req-csr.json and keep 127.0.0.1 and localhost only for single cluster setup.
```
vi config/req-csr.json
```
### generate certs
```
export infra0=127.0.0.1
export infra1=127.0.0.1
export infra2=127.0.0.1
make
mkdir /tmp/etcd-certs
mv certs /tmp/etcd-certs
```
### start etcd cluster member1
```
./start-all.sh
```
### member list
```
etcdctl --endpoints https://127.0.0.1:3379 --cert /tmp/etcd-certs/certs/127.0.0.1.pem --key /tmp/etcd-certs/certs/127.0.0.1-key.pem --cacert /tmp/etcd-certs/certs/ca.pem member list
```
### backup
```
./backup.sh
```
### delete data
```
rm -rf /tmp/etcd
```
### kill process
```
kill process of infra0 infra1 infra2
```
### restore
```
./restore.sh
```