# Active MQ Integration - Mule 4

![Mule 4](https://img.shields.io/badge/Mule-4.9.0-blue)
![Java](https://img.shields.io/badge/Java-17-orange)
![Maven](https://img.shields.io/badge/Maven-3.8+-green)
![ActiveMQ](https://img.shields.io/badge/ActiveMQ-5.15.16-red)

## 📋 Descrição do Projeto

Este é um projeto de integração desenvolvido com **MuleSoft Mule 4** que implementa comunicação assíncrona usando **Apache ActiveMQ**. A aplicação demonstra um padrão produtor-consumidor, onde mensagens são recebidas via HTTP REST API e processadas através de filas JMS.

## 🏗️ Arquitetura

```
HTTP Request → HTTP Listener → Transform → JMS Publisher → ActiveMQ Queue
                                                              ↓
JMS Consumer ← Logger ← Transform ← JMS Listener ← ActiveMQ Queue
```

### Componentes Principais:

1. **HTTP Listener**: Recebe requisições REST na porta 3003
2. **JMS Publisher**: Publica mensagens na fila `queue01`
3. **JMS Consumer**: Consome mensagens da fila `queue01`
4. **ActiveMQ Broker**: Gerencia as filas de mensagens

## 🚀 Tecnologias Utilizadas

- **MuleSoft Mule Runtime**: 4.9.0
- **Apache ActiveMQ**: 5.15.16
- **Java**: 17
- **Maven**: 4.3.0
- **Conectores Mule**:
  - HTTP Connector: 1.10.3
  - JMS Connector: 1.9.7
  - Sockets Connector: 1.2.5

## 📁 Estrutura do Projeto

```
active-mq/
├── src/main/
│   ├── mule/
│   │   ├── active-mq.xml              # Fluxos principais da aplicação
│   │   └── config/
│   │       └── global-connections.xml  # Configurações globais
│   └── resources/
│       ├── application-types.xml       # Tipos de dados da aplicação
│       └── log4j2.xml                 # Configuração de logs
├── pom.xml                            # Configuração Maven
└── mule-artifact.json                 # Configuração do artefato Mule
```

## ⚙️ Configuração

### Pré-requisitos

1. **Java 17** ou superior
2. **Maven 3.8** ou superior
3. **Anypoint Studio** 
4. **Apache ActiveMQ** rodando na porta `61616`

### Configuração do ActiveMQ

A aplicação está configurada para conectar ao ActiveMQ com as seguintes configurações:

```xml
<jms:active-mq-connection username="admin" password="admin">
    <jms:factory-configuration brokerUrl="tcp://0.0.0.0:61616" />
</jms:active-mq-connection>
```

### Configuração HTTP

O HTTP Listener está configurado para:
- **Host**: 0.0.0.0
- **Porta**: 3003
- **Endpoint**: `/users`

## 🔧 Como Executar

### 1. Iniciar o ActiveMQ

```bash
# Download e extração do ActiveMQ
wget https://archive.apache.org/dist/activemq/5.15.16/apache-activemq-5.15.16-bin.tar.gz
tar -xzf apache-activemq-5.15.16-bin.tar.gz
cd apache-activemq-5.15.16

# Iniciar o broker
./bin/activemq start
```

### 2. Compilar e Executar a Aplicação

```bash
# Clone o repositório e navegue até o diretório
cd active-mq

# Compilar a aplicação
mvn clean compile

# Executar a aplicação
mvn mule:run
```

### 3. Testando a Aplicação

```bash
# Enviar mensagem via HTTP POST
curl -X POST http://localhost:3003/users \
  -H "Content-Type: application/json" \
  -d '{"name": "João Silva", "email": "joao@email.com"}'
```

## 📊 Fluxos de Dados

### Fluxo 1: Producer (active-mqFlow)
1. Recebe requisição HTTP POST em `/users`
2. Registra o payload recebido
3. Transforma a mensagem para JSON
4. Publica na fila `queue01` do ActiveMQ

### Fluxo 2: Consumer (active-mqFlow1)
1. Escuta mensagens da fila `queue01`
2. Processa mensagens automaticamente (AUTO ACK)
3. Registra o payload recebido nos logs

## 📝 Logs

A aplicação utiliza **Log4j2** para logging. Os logs incluem:
- Payload das mensagens HTTP recebidas
- Payload das mensagens JMS processadas
- Informações de debug dos conectores

## 🔍 Monitoramento

### ActiveMQ Web Console
- **URL**: http://localhost:8161/admin
- **Usuário**: admin
- **Senha**: admin

### Métricas Disponíveis
- Número de mensagens na fila
- Taxa de produção/consumo
- Tempo de processamento
- Status das conexões

## 🚦 Status da Aplicação

Para verificar se a aplicação está funcionando:

1. **Health Check**: `GET http://localhost:3003/users`
2. **ActiveMQ Console**: Verificar filas em http://localhost:8161/admin
3. **Logs da Aplicação**: Verificar saída do console

## 🔧 Desenvolvimento

### Executar em Modo Debug

```bash
mvn mule:run -Dmule.debug.enable=true
```

### Executar Testes

```bash
mvn test
```

### Empacotar para Deploy

```bash
mvn clean package
```

## 📚 Documentação Adicional

- [MuleSoft Documentation](https://docs.mulesoft.com/)
- [Apache ActiveMQ Documentation](https://activemq.apache.org/)
- [JMS Connector Guide](https://docs.mulesoft.com/jms-connector/)

## 🤝 Contribuindo

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -m 'Adiciona nova feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 👥 Autores

- **Desenvolvedor** - [Seu Nome](https://github.com/seuusuario)

## 🏷️ Versão

- **Versão Atual**: 1.0.0-SNAPSHOT
- **Última Atualização**: Janeiro 2026

---

**Nota**: Certifique-se de que o ActiveMQ está rodando antes de executar a aplicação. A aplicação irá falhar se não conseguir conectar ao broker JMS.