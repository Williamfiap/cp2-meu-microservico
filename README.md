# Meu Microserviço

## Descrição
Este é um microserviço desenvolvido para atender às necessidades do projeto CP2. Ele utiliza Docker para gerenciar um banco de dados MySQL e um backend Python. O microserviço está configurado para rodar na porta 5000 e pode ser gerenciado utilizando os comandos descritos abaixo.

---

## 📦 Parte 1: Persistência com Volumes

### Implementação
Foi utilizado um **Docker Volume** chamado `dados_mysql` para persistir os dados do banco de dados MySQL. A escolha foi feita porque volumes são gerenciados diretamente pelo Docker, oferecendo maior simplicidade e portabilidade em comparação com bind mounts, além de serem mais seguros para armazenar dados sensíveis.

Comando para criar o volume:
```bash
docker volume create dados_mysql
```

## 🛢️ Parte 2: Container com MySQL

### Configuração do Banco de Dados


Comando para iniciar o container com o volume (estilo multi-linha):
```bash
docker run -d --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=mysql_db \
  -e MYSQL_USER=william \
  -e MYSQL_PASSWORD=fiap123@ \
  -p 3307:3306 \
  -v dados_mysql:/var/lib/mysql \
  mysql:8.0
```

Comando para iniciar o container com o volume (estilo linha única):
```bash
docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=mysql_db -e MYSQL_USER=william -e MYSQL_PASSWORD=fiap123@ -p 3307:3306 -v dados_mysql:/var/lib/mysql mysql:8.0
```

---


1. Copie o script de inicialização para o container:
   ```bash
   docker cp ./init.sql mysql-container:/docker-entrypoint-initdb.d/init.sql
   ```

2. Acesse o container:
   ```bash
   docker exec -it mysql-container bash
   ```

3. No terminal do container, acesse o MySQL:
   ```bash
   mysql -u root -p
   ```
   Digite a senha `root` quando solicitado.

4. Execute os seguintes comandos no MySQL para inicializar o banco de dados:
   ```sql
   use mysql_db; show tables; source /docker-entrypoint-initdb.d/init.sql;
   ```

5. Verifique as tabelas e os dados inseridos:
   ```sql
   use mysql_db; show tables; select * from clientes;
   ```

Exemplo de saída:
```plaintext
+----+----------------+------------------+-------------+------------+
| id | nome           | email            | telefone    | endereco   |
+----+----------------+------------------+-------------+------------+
|  1 | Carlos Silva   | carlos@email.com | 11999999999 | Rua A, 123 |
|  2 | Ana Souza      | ana@email.com    | 11988888888 | Rua B, 456 |
|  3 | Pedro Lima     | pedro@email.com  | 11977777777 | Rua C, 789 |
|  4 | Maria Oliveira | maria@email.com  | 11966666666 | Rua D, 101 |
|  5 | João Santos    | joao@email.com   | 11955555555 | Rua E, 202 |
+----+----------------+------------------+-------------+------------+
```

---

## 🧱 Parte 3: Imagem Personalizada

### Criação de Imagens Personalizadas
1. Comite o container para criar imagens personalizadas:
   ```bash
   docker commit mysql-container mysql_db:v1
   docker commit mysql-container mysql_db:v2
   ```

2. Liste as imagens criadas:
   ```bash
   docker images | Select-String mysql_db
   ```

3. Adicione tags às imagens:
   ```bash
   docker tag mysql_db:v1 william201192/mysql_db:v1
   docker tag mysql_db:v2 william201192/mysql_db:v2
   ```

---

## ☁️ Parte 4: Docker Hub

### Publicação das Imagens
1. Faça login no Docker Hub:
   ```bash
   docker login
   ```

2. Envie as imagens para o Docker Hub:
   ```bash
   docker push william201192/mysql_db:v1
   docker push william201192/mysql_db:v2
   ```

Links das imagens:
- [mysql_db:v1](https://hub.docker.com/repository/docker/william201192/mysql_db/tags?page=1&name=v1)
- [mysql_db:v2](https://hub.docker.com/repository/docker/william201192/mysql_db/tags?page=1&name=v2)


LINK:
https://hub.docker.com/repository/docker/william201192/mysql_db/tags


---

## 🔁 Parte 5: Testar Persistência

### Demonstração
Os dados foram preservados após reiniciar e recriar o container, graças ao uso do volume `dados_mysql`.

#### Reinicializar o Container
```bash
docker stop mysql-container
docker start mysql-container
```

#### Verificar os Dados Após Reinicialização
```bash
docker exec -it mysql-container bash
mysql -u root -p
use mysql_db;
show tables;
select * from clientes;
```

#### Recriar o Container
```bash
docker stop mysql-container
docker rm mysql-container
docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=mysql_db -e MYSQL_USER=william -e MYSQL_PASSWORD=fiap123@ -p 3307:3306 -v dados_mysql:/var/lib/mysql mysql:8.0
```

#### Verificar os Dados Após Recriação
```bash
docker exec -it mysql-container bash
mysql -u root -p
use mysql_db;
show tables;
select * from clientes;
```

Exemplo de saída:
```plaintext
+----+----------------+------------------+-------------+------------+
| id | nome           | email            | telefone    | endereco   |
+----+----------------+------------------+-------------+------------+
|  1 | Carlos Silva   | carlos@email.com | 11999999999 | Rua A, 123 |
|  2 | Ana Souza      | ana@email.com    | 11988888888 | Rua B, 456 |
|  3 | Pedro Lima     | pedro@email.com  | 11977777777 | Rua C, 789 |
|  4 | Maria Oliveira | maria@email.com  | 11966666666 | Rua D, 101 |
|  5 | João Santos    | joao@email.com   | 11955555555 | Rua E, 202 |
+----+----------------+------------------+-------------+------------+
```
