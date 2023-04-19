# Crie seu servidor Apache NiFi com Docker-compose

Olá, se você está querendo criar um laboratório para lidar com dados em tempo real, uma opção popular é executar o Apache NiFi em contêineres. Ao fazer isso, você pode garantir que as dependências e configurações do host não afetem a capacidade do NiFi de processar fluxos de dados em tempo real. Além disso, a escalabilidade e flexibilidade proporcionadas pelos contêineres permitem que você ajuste facilmente a capacidade do NiFi de acordo com as necessidades de processamento de fluxo de dados.

No entanto, é importante lembrar que a execução do NiFi em contêineres pode apresentar desafios. Para garantir que tudo funcione corretamente, é necessário planejar cuidadosamente a implantação e configuração dos contêineres, garantindo que o NiFi seja dimensionado e configurado adequadamente para atender às necessidades de processamento de fluxo de dados da organização. Com o planejamento e configuração corretos, você pode executar o Apache NiFi em contêineres com eficiência e eficácia, permitindo que você processe grandes volumes de dados em tempo real.


# Requisitos

Docker 23.0.3 (Host)

Docker-Compose 1.25.0 (Host)

```yaml
version: "3.3"

services: 
  nifi-server:
    environment:
      SINGLE_USER_CREDENTIALS_USERNAME: user_nifi
      SINGLE_USER_CREDENTIALS_PASSWORD: HGd15bvfv8744ghbdhgdv7895agqERAo
    image: apache/nifi:1.19.0
    container_name: nifi-server
    ports:
      - "8443:8443"
    deploy:
      resources:
        limits:
          memory: 4G
    restart: on-failure
    volumes:
      - nifi-database_repository:/opt/nifi/nifi-current/database_repository
      - nifi-flowfile_repository:/opt/nifi/nifi-current/flowfile_repository
      - nifi-content_repository:/opt/nifi/nifi-current/content_repository
      - nifi-provenance_repository:/opt/nifi/nifi-current/provenance_repository
      - nifi-conf:/opt/nifi/nifi-current/conf
      - nifi-state:/opt/nifi/nifi-current/state
      - ./nifi/logs:/opt/nifi/nifi-current/logs
      - ./nifi/jdbc:/opt/nifi/nifi-current/jdbc
      - ./nifi/credentials:/opt/nifi/nifi-current/credentials
    networks:
      - nifi-network

volumes:
    nifi-database_repository:
    nifi-flowfile_repository:
    nifi-content_repository:
    nifi-provenance_repository:
    nifi-state:
    nifi-conf:

networks:
  nifi-network:
    driver: bridge
```


# Implementando o Serviço Apache Nifi

#### Ativando o serviço

```bash
docker-compose -f docker-compose.yaml --compatibility up -d
```


#### Desativando o serviço

```bash
docker-compose -f docker-compose.yaml --compatibility down
```

> ***Obs.:*** para acessar o Apache Nifi depois que o serviço estiver ativo, acesse: https://localhost:8443 e as credenciais de acesso são os valores existe na variáveis: ***SINGLE_USER_CREDENTIALS_USERNAME*** e ***SINGLE_USER_CREDENTIALS_PASSWORD***

# Referências:

Apache/Nifi, ***Docker Hub***. Disponível em: <https://hub.docker.com/r/apache/nifi>. Acesso em: 19 abr. 2023.

How to build a data lake from scratch — Part 1: The setup, ***Victor Seifert***. Disponível em: <https://towardsdatascience.com/how-to-build-a-data-lake-from-scratch-part-1-the-setup-34ea1665a06e>. acesso em: 19 abr. 2023.
