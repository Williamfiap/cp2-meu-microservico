# Meu Microserviço

## Descrição
Este é um microserviço desenvolvido para atender às necessidades do projeto CP2. Ele foi criado utilizando Python e Docker, permitindo fácil implantação e escalabilidade. O microserviço está configurado para rodar na porta 5000 e pode ser gerenciado utilizando os comandos Docker descritos abaixo.

## Requisitos
- Python 3.8 ou superior
- Docker
- Git

## Como executar
1. Certifique-se de que o Docker está instalado e em execução na sua máquina.
2. Construa a imagem Docker do microserviço:
   ```bash
   docker build -t meu-microservico .
   ```
3. Execute o container do microserviço:
   ```bash
   docker run -d --name container-meu-microservico -p 5000:5000 meu-microservico
   ```
4. Para acessar o container em execução:
   ```bash
   docker exec -it container-meu-microservico bash
   ```
5. Verifique o status do microserviço:
   ```bash
   curl http://localhost:5000/status
   ```
   Exemplo de resposta:
   ```json
   {
     "message": "O microsservico esta OK", 
     "status": "online"
   }
   ```
6. Verifique os containers em execução:
   ```bash
   docker ps
   ```
   Exemplo de saída:
   ```plaintext
   CONTAINER ID   IMAGE              COMMAND           CREATED          STATUS          PORTS                    NAMES
   8825b60abcbf   meu-microservico   "python app.py"   15 minutes ago   Up 15 minutes   0.0.0.0:5000->5000/tcp   container-meu-microservico
   ```

## Histórico de Comandos
Abaixo está o histórico de comandos utilizados durante o desenvolvimento e execução do microserviço:

```plaintext
  Id CommandLine
  -- -----------
   1 try { . "c:\Users\Coelho\AppData\Local\Programs\Microsoft VS Code\resources\app\out\vs\workbench\contrib\terminal\common\scripts\shellIntegration.ps1" } catch {}
   2 git status
   3 git add .
   4 git commit -m "iniciando CP"
   5 git push
   6 docker build -t meu-microservico .
   7 docker run -d --name container-meu-microservico -p 5000:5000 meu-microservico
   8 docker exec -it container-meu-microservico bash
   9 docker ps
  10 git checkout feature/exercicio6
  11 git checkout -b feature/exercicio6
```

## Outra maneira de verificar a rota `/status`
Para verificar se a rota `/status` está funcionando corretamente use o navegador, siga os passos abaixo:

1. Certifique-se de que o container do microserviço está em execução.
2. Abra o navegador de sua preferência.
3. Digite o seguinte endereço na barra de URL:
   ```
   http://localhost:5000/status
   ```
4. Você deverá ver a seguinte resposta na tela:
   ```json
   {
     "message": "O microsservico esta OK", 
     "status": "online"
   }
   ```
