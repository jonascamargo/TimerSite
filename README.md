
# Extension - TimerSite

Uma extensão para navegadores que monitora o tempo gasto em sites, controla acesso por horário, bloqueia domínios e permite configurar regras individuais para cada URL.

<br>

---

<br>

## #Funcionalidades Principais

### Detecção de novos domínios
- Ao acessar um domínio pela primeira vez, a extensão pergunta se você deseja:
  - Ignorar  
  - Monitorar (Watched)  
  - Bloquear (Blacklist)

<br>

## #Página de Gerenciamento

A página da extensão exibe todas as URLs registradas, com filtros:

- **All** – todas as URLs  
- **Blacklist** – sites bloqueados  
- **Watched** – monitorados  
- **Ignored** – ignorados  
- **Restricted** – com horário/dias definidos  

Cada item pode ser **editado** ou **excluído**.

<br>

## #Registro de Tempo

A extensão registra tempo para URLs classificadas como **WATCHED** ou **RESTRICTED**, incluindo:

- **Tempo individual da URL**
- **Tempo total do domínio base**  
  Ex.: youtube.com recebe a soma de todas as páginas como /watch, /shorts etc.

<br>

## #Gerenciamento de Múltiplas Abas

### URLs diferentes do mesmo domínio  
- Cada URL registra seu tempo individual.  
- O domínio recebe a soma de todas.

### Várias abas com a mesma URL  
- Apenas a **primeira aba aberta** é registrada.  
- Abas duplicadas são ignoradas para evitar duplicação de tempo.
- Se a aba principal fechar, outra aba idêntica é **promovida automaticamente**.
- O tracking continua sem interrupções.

<br>

## #Estrutura do Registro de Eventos

Os eventos são armazenados assim:
Cada aba possui seu próprio tracking interno, mas apenas a **aba principal** de cada URL contribui para o total.

<br>

## #Bloqueios e Restrições

### BLACKLIST
- O acesso é bloqueado imediatamente.
- O popup não aparece.
- Uma página de bloqueio personalizada é exibida.

### RESTRICTED
- Fora do horário ou dia permitido, o site não é carregado.
- O popup não aparece.
- Uma página informa quando o acesso será liberado.

<br>

## #Ações por URL

### Editar
- Definir status  
- Se **RESTRICTED**:  
  - selecionar dias  
  - selecionar intervalo de horas  
- Se **RESTRICTED** ou **BLACKLIST**:  
  - definir se o usuário pode ignorar o bloqueio (bypass)

### Excluir
- Remove completamente o registro da URL.

<br>

## #Comportamento do Popup

Ao clicar no ícone da extensão:

- **BLACKLIST** → página bloqueada, popup não aparece  
- **WATCHED** → mostra tempo total do dia + tempo histórico  
- **IGNORED** → indica que o site não é monitorado  
- **RESTRICTED** → site bloqueado, popup não aparece  

<br>

## #Exportar e Importar Configurações

Toda a configuração e histórico podem ser exportados ou importados via **arquivo JSON**.

<br>

## #Fechamento de Abas e Navegador

Sempre que:
- uma aba é fechada  
- o navegador é encerrado  

#### A extensão atualiza e salva os tempos pendentes antes de remover os dados da aba.

<br>

## #Configurações Especiais

- Intervalo mínimo de rastreamento: **5 segundos**  
- URLs com **bypass permitido** registram data/hora do bypass  
- Domínios acumulam o uso de todas as páginas  
- URLs idênticas utilizam apenas a primeira aba aberta para rastreamento  

<br>

---

<br>

## #JSON de Exemplo (resumido)

```json
{
  "config": {
    "watchlist": ["youtube.com", "instagram.com"],
    "blacklist": ["tiktok.com"],
    "ignored_domains": ["localhost"],
    "tracking_interval_seconds": 5,
    "default_status": "watched",
    "default_allowed_hours": {
      "start": "08:00",
      "end": "22:00"
    },
    "language": "pt-br",
    "theme": "dark",
    "available_themes": ["white", "dark", "purple"]
  },

  "sessions": [
    {
      "url": "https://exemplo.com/pagina",
      "events": {
        "20251201": {
          "focus": {
            "143210": 1370
          },
          "blur": {
            "145500": 525
          }
        }
      }
    }
  ]
}
````

<br>

---

<br>

## #1 Base

### Sistema de Detecção de Novos Domínios
- [ ] Interceptar domínio acessado pela aba
- [ ] Exibir modal de decisão (ignore / watch / restrict / block)
- [ ] Salvar status inicial no storage

### Mecanismo Principal de Tracking
- [ ] Registrar focus/blur por aba
- [ ] Garantir tracking mínimo de 5 segundos
- [ ] Somente a primeira aba por URL ativa conta tempo
- [ ] Ignorar abas duplicadas
- [ ] Implementar promoção de aba (tab promotion)
- [ ] Registrar tempo individual da URL
- [ ] Registrar tempo acumulado do domínio base

### Sistema de Bloqueio
- [ ] Impedir carregamento de URLs BLACKLIST
- [ ] Mostrar página de bloqueio personalizada
- [ ] Implementar restrição por horário/dias
- [ ] Exibir página com motivo e horário permitido
- [ ] Controle de bypass + registro de timestamps

<br>

## #2 Interface e Interação

### Popup
- [ ] Exibir tempo total do dia + histórico em URLs WATCHED
- [ ] Exibir mensagem de ignorado em URLs IGNORED
- [ ] Bloquear popup em BLACKLIST/RESTRICTED

### Página de Gerenciamento
- [ ] Listagem completa das URLs
- [ ] Filtros (all, watched, ignored, blacklist, restricted)
- [ ] Botão editar (status + campos especiais)
- [ ] Botão excluir

### Edição de URL
- [ ] Alterar status
- [ ] Mostrar/ocultar dias da semana e intervalo de horas
- [ ] Mostrar opção de bypass para restrict/blacklist
- [ ] Botão salvar

<br>

## #3 Persistência

### Sessões e Salvar ao Fechar
- [ ] Registrar eventos pendentes ao fechar aba
- [ ] Registrar ao fechar browser
- [ ] Atualizar tempos no storage automaticamente

### Exportar/Importar JSON
- [ ] Exportar todas as configurações + histórico completo
- [ ] Importar arquivo JSON validado
- [ ] Atualizar interface após importação

<br>

## #4 Qualidade e Robustez

### Validações
- [ ] Garantir que tracking_interval_seconds ≥ 5
- [ ] Validar formato de horários
- [ ] Validar duplicações de URLs

### Performance
- [ ] Evitar loops intensivos
- [ ] Compactar eventos antigos
- [ ] Limpar registros de abas fechadas

### Testes
- [ ] Testar em Chrome
- [ ] Testar em Firefox
- [ ] Testar comportamento em abas duplicadas
- [ ] Testar bloqueios e horários

<br>

## #5 Futuro (Melhorias)

- [ ] Dashboard com gráficos
- [ ] Notificações de limite diário
- [ ] Estatísticas semanais/mensais
- [ ] Modo criança (senha para editar configurações)
- [ ] Sincronizar dados entre dispositivos
- [ ] Integração com serviços de terceiros
- [ ] Opção “Sempre registrar domínio base”
