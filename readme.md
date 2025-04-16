# Modelo de Dados da FarmTech Solutions

## Identificação de Informações Relevantes
Dados coletados por sensores de umidade (S1), pH (S2) e nutrientes (S3) para monitorar plantações e otimizar recursos, com foco principal no plantio de soja.

## Entidades e Atributos

### Sensor
- SensorID (PK): Identificador único do sensor
- TipoSensor: Tipo do sensor (S1 - umidade, S2 - pH, S3 - nutrientes)
- Localizacao: Localização do sensor no talhão
- DataInstalacao: Data de instalação do sensor
- Status: Status operacional do sensor

### TipoCultura
- TipoCulturaID (PK): Identificador único do tipo de cultura
- NomeCultura: Nome da cultura (soja, milho, algodão, etc.)
- DescricaoCultura: Descrição detalhada da cultura
- TempoMedioCiclo: Tempo médio do ciclo de desenvolvimento em dias
- NecessidadeHidricaMedia: Necessidade hídrica média em mm/ciclo
- NecessidadeNutrientesMedia: Necessidade média de nutrientes

### Cultura
- CulturaID (PK): Identificador único da cultura
- TipoCulturaID (FK): Referência ao tipo de cultura plantada
- Variedade: Variedade específica da cultura
- DataPlantio: Data do plantio
- DataColheitaPrevista: Data prevista da colheita
- AreaHectare: Área ocupada pela cultura em hectares
- ProducaoEstimada: Produção estimada em toneladas

### Talhao
- TalhaoID (PK): Identificador único do talhão
- CulturaID (FK): Referência à cultura plantada
- NomeTalhao: Nome identificador do talhão
- AreaHectare: Área do talhão em hectares
- DensidadePlantio: Densidade do plantio (plantas/m²)
- EspacamentoEntreLinhas: Espaçamento entre linhas em centímetros
- CicloVegetativo: Ciclo vegetativo da cultura (precoce, médio, tardio)
- Latitude: Coordenada de latitude do talhão
- Longitude: Coordenada de longitude do talhão
- Altitude: Altitude do talhão em metros
- TipoSolo: Tipo de solo do talhão

### LeituraSensor
- LeituraID (PK): Identificador único da leitura
- SensorID (FK): Referência ao sensor que fez a leitura
- TalhaoID (FK): Referência ao talhão monitorado
- DataHora: Data e hora da leitura
- ValorLeitura: Valor genérico da leitura
- Unidade: Unidade de medida da leitura

### LeituraUmidade
- LeituraUmidadeID (PK): Identificador único da leitura de umidade
- LeituraID (FK): Referência à leitura genérica do sensor
- PercentualUmidade: Percentual de umidade do solo
- ProfundidadeMedicao: Profundidade da medição em centímetros
- TemperaturaAmbiente: Temperatura ambiente no momento da leitura

### LeituraPH
- LeituraPHID (PK): Identificador único da leitura de pH
- LeituraID (FK): Referência à leitura genérica do sensor
- ValorPH: Valor do pH do solo
- ProfundidadeMedicao: Profundidade da medição em centímetros

### LeituraNutrientes
- LeituraNutrientesID (PK): Identificador único da leitura de nutrientes
- LeituraID (FK): Referência à leitura genérica do sensor
- NivelFosforo: Nível de fósforo (P) no solo
- NivelPotassio: Nível de potássio (K) no solo
- NivelNitrogenio: Nível de nitrogênio (N) no solo
- ProfundidadeMedicao: Profundidade da medição em centímetros

### Irrigacao
- IrrigacaoID (PK): Identificador único do evento de irrigação
- TalhaoID (FK): Referência ao talhão irrigado
- DataHora: Data e hora da irrigação
- QuantidadeAgua: Quantidade de água aplicada em litros/m²
- DuracaoMinutos: Duração da irrigação em minutos
- TipoIrrigacao: Método de irrigação utilizado
- PressaoAgua: Pressão da água durante a irrigação
- FonteAgua: Fonte da água utilizada na irrigação

### AplicacaoNutrientes
- AplicacaoID (PK): Identificador único da aplicação
- TalhaoID (FK): Referência ao talhão que recebeu os nutrientes
- DataHora: Data e hora da aplicação
- TipoNutriente: Tipo de nutriente aplicado
- Quantidade: Quantidade de nutriente aplicado
- MetodoAplicacao: Método de aplicação utilizado
- CondicaoClimatica: Condição climática durante a aplicação
- Formulacao: Formulação do nutriente aplicado

### HistoricoClimatico
- HistoricoID (PK): Identificador único do registro climático
- TalhaoID (FK): Referência ao talhão monitorado
- DataHora: Data e hora do registro
- Temperatura: Temperatura em graus Celsius
- Precipitacao: Volume de chuva em milímetros
- UmidadeAr: Percentual de umidade do ar
- VelocidadeVento: Velocidade do vento em km/h
- DirecaoVento: Direção predominante do vento
- RadiacaoSolar: Nível de radiação solar em W/m²

### PrevisaoRecomendacao
- PrevisaoID (PK): Identificador único da previsão/recomendação
- TalhaoID (FK): Referência ao talhão
- DataHoraGeracao: Data e hora da geração da previsão
- TipoPrevisao: Tipo de previsão (irrigação, nutrientes, etc.)
- DescricaoRecomendacao: Descrição detalhada da recomendação
- PrioridadeAcao: Nível de prioridade da ação recomendada
- DataLimiteAcao: Data limite para execução da ação
- EconomiaEstimada: Economia estimada pela implementação da recomendação

## Cardinalidade dos Relacionamentos

### Relações 1:N (Um para Muitos)

- **TipoCultura → Cultura (1:N)**: Um tipo de cultura (como soja) pode ser plantado em várias culturas específicas, cada uma com suas características particulares de variedade e ciclo.

- **Cultura → Talhao (1:N)**: Uma cultura pode ser dividida em vários talhões para melhor gerenciamento e monitoramento. Cada talhão pertence a apenas uma cultura.

- **Sensor → LeituraSensor (1:N)**: Um sensor realiza múltiplas leituras ao longo do tempo, gerando um histórico de dados. Cada leitura é feita por apenas um sensor.

- **Talhao → LeituraSensor (1:N)**: Um talhão pode ter várias leituras de diferentes sensores ou do mesmo sensor em momentos distintos. Cada leitura está associada a um único talhão.

- **Talhao → Irrigacao (1:N)**: Um talhão recebe múltiplas irrigações ao longo do ciclo da cultura. Cada evento de irrigação é aplicado a um talhão específico.

- **Talhao → AplicacaoNutrientes (1:N)**: Um talhão recebe diversas aplicações de nutrientes durante o desenvolvimento da cultura. Cada aplicação é direcionada a um talhão específico.

- **Talhao → HistoricoClimatico (1:N)**: Um talhão tem múltiplos registros climáticos ao longo do tempo. Cada registro climático está associado a um talhão específico.

- **Talhao → PrevisaoRecomendacao (1:N)**: Um talhão pode receber várias recomendações baseadas na análise dos dados coletados. Cada recomendação é direcionada a um talhão específico.

### Relações 1:1 (Um para Um)

- **LeituraSensor → LeituraUmidade/LeituraPH/LeituraNutrientes (1:1)**: Cada leitura genérica de sensor tem exatamente uma leitura especializada correspondente, dependendo do tipo de sensor. Esta abordagem permite armazenar dados comuns a todos os sensores na tabela LeituraSensor e dados específicos em tabelas especializadas.

## Conexões Entre Tabelas

- TipoCultura → Cultura: TipoCulturaID (FK_Cultura_TipoCultura)
- Cultura → Talhao: CulturaID (FK_Talhao_Cultura)
- Sensor → LeituraSensor: SensorID (FK_LeituraSensor_Sensor)
- Talhao → LeituraSensor: TalhaoID (FK_LeituraSensor_Talhao)
- LeituraSensor → LeituraUmidade: LeituraID (FK_LeituraUmidade_Leitura)
- LeituraSensor → LeituraPH: LeituraID (FK_LeituraPH_Leitura)
- LeituraSensor → LeituraNutrientes: LeituraID (FK_LeituraNutrientes_Leitura)
- Talhao → Irrigacao: TalhaoID (FK_Irrigacao_Talhao)
- Talhao → AplicacaoNutrientes: TalhaoID (FK_AplicacaoNutrientes_Talhao)
- Talhao → HistoricoClimatico: TalhaoID (FK_HistoricoClimatico_Talhao)
- Talhao → PrevisaoRecomendacao: TalhaoID (FK_PrevisaoRecomendacao_Talhao)

## Fluxo de Dados
1. Dados coletados pelos sensores (S1, S2, S3) são registrados na tabela LeituraSensor
2. Cada tipo de sensor tem sua tabela especializada (LeituraUmidade, LeituraPH, LeituraNutrientes)
3. Os dados são associados aos talhões onde os sensores estão instalados
4. Um sistema externo analisa os dados armazenados e calcula recomendações que são armazenadas na tabela PrevisaoRecomendacao
5. As ações de irrigação e aplicação de nutrientes são registradas em suas respectivas tabelas
6. Dados climáticos são registrados para auxiliar na análise e previsão

## Recursos Adicionais do Banco de Dados

- Sequências: Criadas para todas as tabelas (Sensor_Seq, TipoCultura_Seq, etc.)
- Triggers: Implementados para auto-incremento de chaves primárias em todas as tabelas

## Consultas Principais
O modelo permite realizar consultas como:

1. Consumo total de água por talhão e período
2. Variação de pH por talhão ao longo do tempo
3. Correlação entre níveis de nutrientes e produtividade
4. Impacto das condições climáticas na irrigação

## Diagrama Entidade-Relacionamento
Modelo visual criado com SQL Developer Data Modeler disponível como imagem no repositório.
