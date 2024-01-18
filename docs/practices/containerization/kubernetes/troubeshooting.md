# Troubleshooting

kubectl logs [pod-name]
kubectl logs [pod-name] -c [container-name]
kubectl logs -p [pod-name]
kubectl logs -f [pod-name]
kubectl describe [pod-name]
kubectl describe [pod-name] -o yaml | json # defaults to yaml

