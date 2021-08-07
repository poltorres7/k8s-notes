### Velero  

#### [Instalacion](https://velero.io/docs/v1.6/basic-install/)  
- osx  
`brew install velero`  
- linux  
```
Descargar la ultima version
wget https://github.com/vmware-tanzu/velero/releases/download/v1.6.2/velero-v1.6.2-linux-amd64.tar.gz
tar -xvf velero-v1.6.2-linux-amd64.tar.gz
cd velero-v1.6.2-linux-amd64/
sudo mv velero /usr/local/bin/
```  
- Windows - Chocolatey  
`choco install velero`  

#### Instalacion velero  
![alt velero](images/velero.png)  

crear bucket para respaldar velero:
```
aws s3 mb s3://<S3_BUCKET_NAME> --region eu-east-1 --profile cerouno
aws s3api get-bucket-location --bucket <S3_BUCKET_NAME>
```  

Creacion de politica para S3  
```
cat > velero-policy.json <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeVolumes",
                "ec2:DescribeSnapshots",
                "ec2:CreateTags",
                "ec2:CreateVolume",
                "ec2:CreateSnapshot",
                "ec2:DeleteSnapshot"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:PutObject",
                "s3:AbortMultipartUpload",
                "s3:ListMultipartUploadParts"
            ],
            "Resource": [
                "arn:aws:s3:::<S3_BUCKET_NAME>/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::<S3_BUCKET_NAME>"
            ]
        }
    ]
}
EOF

aws iam create-policy \
  --policy-name VeleroBackupPolicy \
  --policy-document file://velero-policy.json \
  --profile <AWS_PROFILE>
```

#### Crear IRSA  
![alt IRSA](images/IRSA.png)  
```
eksctl utils associate-iam-oidc-provider --cluster=cerouno --approve
kubectl create ns velero
eksctl create iamserviceaccount --cluster=cerouno --name=veleros3 --namespace=velero --attach-policy-arn=arn:aws:iam::<AWS_ACCOUNT_ID>:policy/VeleroBackupPolicy --approve
```  

#### Instalacion Velero  
```
# Agregar los siguientes valores a la configuracion
initContainers:
  - name: velero-plugin-for-aws
    image: velero/velero-plugin-for-aws:v1.2.0
    imagePullPolicy: IfNotPresent
    volumeMounts:
      - mountPath: /target
        name: plugins
configuration:
  provider: aws
  backupStorageLocation:
    bucket: S3_BUCKET_NAME
serviceAccount:
  server:
    create: false
    name: veleros3


helm repo add vmware-tanzu https://vmware-tanzu.github.io/helm-charts
helm repo update
helm upgrade --install velero vmware-tanzu/velero \
  --namespace velero \
  -f values.yaml
# Verificar la instalacion
kubectl get all -n velero
```  

#### Uso velero  
```
kubectl create ns <NAME>
kubectl apply -f nginx-base.yaml -n <NAME>
velero backup create nginx-backup-<NAME> --include-namespaces <NAME>
velero backup describe nginx-backup-<NAME>

kubectl delete namespaces <NAME>
velero restore create --from-backup nginx-backup-<NAME>


```
