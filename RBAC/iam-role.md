eksctl create iamidentitymapping \
  --cluster cerouno \
  --arn arn:aws:iam::${ACCOUNT_ID}:role/cerouno-k8s \
  --username admin \
  --group system:masters
