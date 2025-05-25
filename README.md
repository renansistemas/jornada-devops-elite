# jornada-devops-elite


### Comando para acessar o Grafana

```
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

---

## Processo de Criação e Deploy no Kubernetes na Nuvem (Digital Ocean)

1. **Provisionamento do Cluster Kubernetes**
    - Utilizamos um provedor de nuvem para criar um cluster Kubernetes gerenciado.
    - Após a criação, configuramos o acesso local ao cluster usando o comando:
      ```
      az aks get-credentials --resource-group <nome-do-resource-group> --name <nome-do-cluster>
      ```
      ou conforme o provedor utilizado.

2. **Uso do kubectl**
    - Validamos o acesso ao cluster com:
      ```
      kubectl get nodes
      ```
    - Utilizamos o `kubectl` para gerenciar recursos, aplicar configurações e monitorar o estado dos pods e serviços.

3. **Criação do deployment.yaml**
    - Criamos um arquivo `deployment.yaml` contendo as definições de Deployment e Service para a aplicação, por exemplo:
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
         name: minha-aplicacao
      spec:
         replicas: 2
         selector:
            matchLabels:
              app: minha-aplicacao
         template:
            metadata:
              labels:
                 app: minha-aplicacao
            spec:
              containers:
              - name: minha-aplicacao
                 image: <imagem-da-aplicacao>
                 ports:
                 - containerPort: 80
      ---
      apiVersion: v1
      kind: Service
      metadata:
         name: minha-aplicacao-service
      spec:
         type: LoadBalancer
         selector:
            app: minha-aplicacao
         ports:
            - protocol: TCP
              port: 80
              targetPort: 80
      ```
    - Aplicamos o arquivo com:
      ```
      kubectl apply -f deployment.yaml
      ```

4. **Exposição da Aplicação via IP Público**
    - O serviço do tipo `LoadBalancer` solicita ao provedor de nuvem um IP público.
    - Consultamos o IP público com:
      ```
      kubectl get service minha-aplicacao-service
      ```
    - Após alguns instantes, o campo `EXTERNAL-IP` é preenchido. Utilizamos esse IP para acessar a aplicação diretamente pela internet.

---

Esse processo garante o deploy e acesso à aplicação rodando em Kubernetes na nuvem, utilizando as melhores práticas de provisionamento, gerenciamento e exposição de serviços.