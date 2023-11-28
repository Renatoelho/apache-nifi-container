# Apache NiFi em contêiner com Docker Compose

Olá, se você está querendo criar um laboratório para lidar com dados em tempo real, uma opção é executar o Apache NiFi em contêineres. Ao fazer isso, você pode garantir que as dependências e configurações do host não afetem a capacidade do NiFi de processar fluxos de dados em tempo real. Além disso, a escalabilidade e flexibilidade proporcionadas pelos contêineres permitem que você ajuste facilmente a capacidade do NiFi de acordo com as necessidades de processamento dos seus fluxos de dados.

No entanto, é importante lembrar que a execução do NiFi em contêineres pode apresentar desafios. Para garantir que tudo funcione corretamente, é necessário planejar a implantação e configuração dos contêineres, garantindo que o NiFi seja dimensionado e configurado adequadamente para atender às necessidades de processamento. Com o planejamento e configurações corretas, você pode executar o Apache NiFi em contêineres com eficiência e eficácia, permitindo o processamento de grandes volumes de dados em tempo real.


### Requisitos

+ ![Docker](https://img.shields.io/badge/Docker-23.0.3-E3E3E3)

+ ![Docker-compose](https://img.shields.io/badge/Docker--compose-1.25.0-E3E3E3)

+ ![Git](https://img.shields.io/badge/Git-2.25.1%2B-E3E3E3)

+ ![Ubuntu](https://img.shields.io/badge/Ubuntu-20.04-E3E3E3)


### Arquivo Yaml para ativação do Apache Nifi

```yaml
version: "3.3"

services: 
  apache-nifi:
    environment:
      SINGLE_USER_CREDENTIALS_USERNAME: nifi
      SINGLE_USER_CREDENTIALS_PASSWORD: HGd15bvfv8744ghbdhgdv7895agqERAo
    image: apache/nifi:1.23.0
    container_name: apache-nifi
    ports:
      - "8443:8443"
    deploy:
      resources:
        limits:
          memory: 4G
    restart: on-failure
    volumes: 
      - ./nifi/jdbc:/opt/nifi/nifi-current/jdbc
      - nifi-logs:/opt/nifi/nifi-current/logs
      - nifi-conf:/opt/nifi/nifi-current/conf
      - nifi-state:/opt/nifi/nifi-current/state
      - nifi-content:/opt/nifi/nifi-current/content_repository
      - nifi-database:/opt/nifi/nifi-current/database_repository
      - nifi-flowfile:/opt/nifi/nifi-current/flowfile_repository
      - nifi-provenance:/opt/nifi/nifi-current/provenance_repository
    networks:
      - nifi-network

volumes:
    nifi-logs:
    nifi-conf:
    nifi-state:
    nifi-content:
    nifi-database:
    nifi-flowfile:
    nifi-provenance:

networks:
  nifi-network:
    driver: bridge
```

### Implantando o Serviço Apache Nifi

+ Clonando o repositório:

```bash
git clone https://github.com/Renatoelho/apache-nifi-container.git apache-nifi
```

+ Ativando o serviço:

```bash
cd apache-nifi
```

```bash
docker compose -p apache-nifi -f docker-compose.yaml up -d
```

+ Desativando o serviço:

```bash
docker compose -p apache-nifi -f docker-compose.yaml down
```

Para acessar o Apache Nifi depois que o serviço estiver ativo, acesse: [https://localhost:8443](https://localhost:8443) e as credenciais de acesso são os valores existentes nas variáveis: ```SINGLE_USER_CREDENTIALS_USERNAME``` e ```SINGLE_USER_CREDENTIALS_PASSWORD```.

# Referências:

Apache/Nifi, ***Docker Hub***. Disponível em: <https://hub.docker.com/r/apache/nifi>. Acesso em: 19 abr. 2023.

NiFi System Administrator’s Guide, ***Apache NiFi***. Disponível em: <https://nifi.apache.org/docs/nifi-docs/html/administration-guide.html>. Acesso em: 22 abr. 2023.

