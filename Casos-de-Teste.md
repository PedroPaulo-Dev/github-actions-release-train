### 🧪 Bateria de Testes Avançados e Validação do Release Train

Para homologar a esteira (`selecao_prs_virada.yml`) e compreender a arquitetura de branches no Git, executei um ciclo completo de testes simulando desde o fluxo padrão até cenários de virada de versão e contaminação de código.

---

#### 📌 Casos de Teste Executados:

1. **Caso 1: Branch Limpa sem Pedidos de Correção (`feature/ajuste-1`)**
   - **Fluxo:** Criada a partir da `master`, apontada e mergeada na `develop`.
   - **Resultado:** Aprovada pelo QA com `testado com sucesso`. A esteira criou a branch `selecionados-v1` e abriu o PR acumulativo contra a `master`.

2. **Caso 2: Branch com Correção (`feature/ajuste-1-fix`)**
   - **Fluxo:** Criada a partir da branch original (`feature/ajuste-1`) para manter o histórico de commits da correção.
   - **Resultado:** Mergeada na `develop` e incorporada com sucesso pela esteira na release v1.

3. **Caso 3: Branches com Códigos que se Completam (`feature/ajuste-2`)**
   - **Fluxo:** Criada a partir da `feature/ajuste-1-fix` devido à dependência funcional.
   - **Resultado:** Empilhada no PR de virada sem quebrar dependências e sem requerer o histórico da `develop`.

---

#### 🚀 Validação do Ciclo de Vida da Release:

* **CASO A: Fechar a Release v1 e Mergear no Master (Simulando a Ida pra Produção)**
  - Realizado o merge do PR acumulativo da `selecionados-v1` diretamente na `master`. A release v1 foi oficialmente concluída em produção.

* **CASO B: Testar a Criação Automática da selecionados-v2 (Nova Virada)**
  - Subida uma nova funcionalidade no próximo ciclo. A esteira identificou que a `selecionados-v1` já estava integrada à `master` (`ahead_by === 0`) e **gerou automaticamente a branch `selecionados-v2`** para a nova release.

* **CASO C: O Desafio da Branch Contaminada (Simulação de Merge Indevido)**
  - **Cenário:** Simulei o erro de um dev ao rodar `git merge origin/develop` dentro da branch da feature (`feature/ajuste-contaminado`), trazendo alterações de terceiros em testes (`feat: erro de suposto dev`).
  - **Resultado na Esteira:** Ao aprovar o PR, a esteira puxou a branch para a `selecionados-v2`. Na aba *Files Changed* do PR de Virada, foi comprovado que **o código não testado do outro dev "pegou carona" para o PR de produção**.
  - **Conclusão:** O teste provou na prática o perigo do vazamento de código em produção e a necessidade de garantir que as branches nasçam estritamente puras a partir da `master`.

---
✅ **Todos os cenários foram simulados no Git Bash, validados no GitHub Actions e aprovados.**