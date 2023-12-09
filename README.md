# DOCUMENTAÇÃO

### Autores

Danilo Aparecido Namitala - 202011125

Fabrício de Oliveira Carvalho - 201910388

### Sumário

* [Criação do App](#Criação-do-App)
* [Dockerfile](#Dockerfile)
* [Compilação e publicação](#Compilação-e-publicação)
* [Helm Chart](#Helm-Chart)

### Criação do App

Referencia: https://medium.com/@diogo.fg.pinheiro/simple-to-do-list-app-with-node-js-and-mongodb-chapter-2-3780a1c5b039

### Dockerfile

Arquivo simples usando a imagem `node:slim` 

- Defina a pasta de trabalho
- Copia os arquivos para a pasta
- Instala as dependencias
- Expõe a porta da aplicação
- Define o comando de execução

### Compilação e publicação

compilar:
```bash
docker build -t daniloufla/trab_final:1.0.1 .
```

login: 
```bash 
docker login
```

publicação:
```bash
docker push daniloufla/trab_final:1.0.1
```
Execução (testar a imagem):
```bash
docker run -d -p 3000:3000 daniloufla/trab_final:1.0.1
```

### Helm Chart

Para iniciar o helm chart usamos o comando
```bash
helm create helm-chart
```
Agora ja temos uma pasta com os templates criados

no arquivo `Values.yaml` coloco o container que vai ser usado
```yaml
image:
  repository: daniloufla/trab_final
  pullPolicy: IfNotPresent
  tag: "1.0.1"
```

e mudo a porta do service para a porta da minha aplicação:
```yaml
service:
  type: ClusterIP
  port: 3000
```
Adiciono um template `configmap.yaml` para criar o config map e nele adiciono um valor representando a versão do app:
```yaml
appversion: {{ .Chart.AppVersion }}
```

Agora posso instalar o helm usando:
```bash
helm install trabalho-final ./helm-chart
```

Para ver o valor do config map posso usar o comando do kubectl e usando o nome do meu config map (posso ver o nome usando o `kubectl get configmap`)
```bash
kubectl describe configmap trabalho-final-configmap
```

Para ver o valor do secrets posso usar o comando do kubectl e usando o nome do meu secrets (posso ver o nome usando o `kubectl get secrets`)
```bash
kubectl describe secret meu-secret
```

Para testar a aplicação uso o comando `kubectl get pods` e encontro o nome do meu pod, então faço o port forward usando:
```bash
kubectl port-forward <nome-pod> 3000:3000
```

e então abrir o endereço http://localhost:3000 no navegador
