# saude-legal-ai

Projeto para facilitar o controle de informações de saúde individual e/ou familiar.

> **Status: descoberta.** Ainda não há código. Este documento registra o que já foi decidido, os princípios, o contexto legal e as perguntas em aberto. Ele deve ser atualizado a cada decisão.

## O que é

Um app de celular que funciona como **registro pessoal de saúde** (PHR: *personal health record*), mantido pelo próprio paciente. Substitui uma planilha onde hoje se marcam médicos, exames, diagnósticos e afins.

Não é o "prontuário" no sentido legal (o registro que o médico, a clínica ou o hospital mantém sobre o paciente). Essa distinção importa porque as normas mais pesadas miram o prontuário do profissional, não o registro do paciente.

## O nome

`saúde legal, aí!` é um trocadilho proposital com três camadas:

- **saúde legal**: cuidar da saúde é legal (bacana) e tem amparo legal;
- **aí**: "é bom ter saúde legal aí", como interjeição e lugar;
- **.ai**: a camada de inteligência artificial, que fica plausível sem precisar ser prometida.

## Princípios

1. **O app nunca interpreta clinicamente.** Guarda e mostra; não diz "seu colesterol está alto" nem sugere diagnóstico ou dose. A faixa de referência impressa no laudo pode ser exibida; calcular risco, não. Isso mantém o projeto fora de *software como dispositivo médico* (SaMD).
2. **IA, quando houver, organiza o que já está no documento** (extrair dados de um laudo, busca, agrupar). Nunca conclui algo sobre a saúde da pessoa.
3. **Regra de negócio fica numa camada única (backend/domínio).** A tela só renderiza e consome; nunca decide por etiqueta ou nome de status.
4. **Modelo de dados não muda sem discussão.** Criar, renomear ou remover campos, chaves ou itens exige proposta e aprovação.
5. **Demo antes de tela.** Nenhuma tela é implementada antes de uma demo aprovada. Celular primeiro, seletores nativos em vez de muitos controles à vista.
6. **Privacidade por padrão.** Dado de saúde é dado pessoal sensível; segurança, auditoria e exclusão entram na modelagem desde o início, não depois.

## Contexto legal (resumo, a confirmar com fontes)

| Norma | Relevância |
|---|---|
| **LGPD (Lei 13.709/2018)** | Dado de saúde é sensível (art. 11). O art. 4º exclui tratamento por pessoa física para fins exclusivamente pessoais e não econômicos; distribuir o app a terceiros torna o autor **controlador**. |
| **Lei 13.787/2018 e normas do CFM** | Prontuário eletrônico e guarda de 20 anos. Valem para profissionais e clínicas, não para o registro pessoal. |
| **ANVISA RDC 657/2022** | Software como dispositivo médico. Só se aplica se o app interpretar dados clínicos (ver princípio 1). |
| **RNDS (Portaria GM/MS 1.434/2020)** | Padrão **HL7 FHIR**. Integração é opcional. |
| **Políticas Apple/Google** | Política de privacidade e declaração de dados de saúde, se publicado nas lojas. |

> Este resumo vem de conhecimento geral, sem consulta a fontes. Antes de qualquer decisão que dependa dele, pesquisar os textos e registrar as referências. Isto não é aconselhamento jurídico.

## O que existe hoje

- **Meu SUS Digital**: oficial, puxa dados do SUS via RNDS; cobre pouco do atendimento particular.
- **Apps de hospitais e laboratórios**: cada grupo mostra só o que gerou.
- **Apple Saúde / Health Connect**: medições; pouco útil para consultas e diagnósticos brasileiros.
- **Prontuários do profissional** (iClinic, Amplimed, Memed): fechados para o paciente.

**Lacuna:** nenhum agrega, num só lugar controlado pelo paciente, consultas particulares, exames de vários laboratórios, diagnósticos e medicamentos.

## Padrões de apps sérios (checklist)

Autenticação forte e bloqueio por inatividade; consentimento granular e revogável; log de auditoria; compartilhamento temporário (link/QR com validade); exportação (PDF e, idealmente, FHIR) e exclusão de conta; criptografia em trânsito e em repouso; nada de dado sensível em logs ou analytics; aviso de "não substitui orientação médica".

## Hipótese de modelo (a validar, nada aprovado)

Nomear e estruturar pensando em FHIR deixa a porta aberta para RNDS e exportação, sem exigir implementar o padrão:

**Paciente** (separado de usuário, pois o escopo é individual e/ou familiar) → **Evento clínico** (consulta, exame, internação, cirurgia) → **Resultado** (valor, unidade, referência), **Diagnóstico**, **Medicamento/Prescrição**, **Anexo**, **Lembrete**; mais **Profissional** (médico).

## Decisões em aberto

1. **Quem usa:** só uma pessoa, ou família? Alguém mais (cuidador, médico) precisa ver, e com que permissões?
2. **A planilha atual:** estrutura (abas, colunas, volume), o que incomoda ou falta, e se o histórico será migrado.
3. **Escopo do MVP:** só registrar ou também lembrar? Anexos (PDF, foto)? Resultados estruturados com gráfico de evolução? Medicamentos, alergias, vacinas, medições? Resumo para levar ao médico?
4. **Dados:** nuvem (AWS) ou só no aparelho? Backup e troca de celular? Biometria/PIN? Acesso offline?
5. **Plataforma:** Android, iOS ou ambos? Loja ou instalação direta/PWA?
6. **Ciclo de vida:** projeto pessoal privado ou possível produto? Isso decide a obrigação da LGPD e o nível de compliance.
7. **Stack:** reaproveitar a do financas-ai (SAM + Go + DynamoDB + Cognito) ou outra?
8. **Escrita do médico:** algum dia o profissional escreve no app, ou só o paciente registra?

## Como vamos trabalhar

1. Responder as decisões em aberto, por blocos, registrando cada resposta aqui.
2. Escrever **intenção e escopo do MVP**, depois plano e especificação por feature (`intent` → `plan` → `spec`, com aprovação entre cada etapa).
3. Só então decidir a stack e mostrar **demo das telas** antes de qualquer código.
