**Plano: Testes da API FastAPI**
Criar uma suíte pytest em `tests/` que valide as rotas HTTP e regras de inscrição existentes, sem alterar a lógica da aplicação.

**Etapas**
1. Adicionar `pytest` a [requirements.txt](requirements.txt), seguindo o padrão atual sem versões fixadas.
2. Criar `tests/test_app.py` com `fastapi.testclient.TestClient`, cobrindo:
   - `GET /activities`.
   - Inscrição bem-sucedida, atividade inexistente (`404`), duplicidade (`400`) e atividade lotada (`400`).
   - Cancelamento bem-sucedido, atividade inexistente (`404`) e participante não inscrito (`404`).
3. Estruturar cada teste explicitamente em três blocos AAA: Arrange prepara a fixture e os dados específicos; Act faz uma única requisição HTTP; Assert verifica status, resposta e estado resultante. Usar uma fixture com `monkeypatch` e `deepcopy` para restaurar `activities` ao fim de cada teste.
4. Manter [pytest.ini](pytest.ini) como está: `pythonpath = .` já permite importar `src.app`.
5. Executar `python -m pytest -q` da raiz e confirmar que os testes passam sem depender da ordem de execução.

**Arquivos relevantes**
- [requirements.txt](requirements.txt): incluir pytest.
- [pytest.ini](pytest.ini): configuração existente a reutilizar.
- `tests/test_app.py`: novo arquivo de testes.
- [src/app.py](src/app.py): referência para rotas, regras e respostas HTTP; sem mudança prevista.
- [.github/workflows/4-step.yml](.github/workflows/4-step.yml): verifica a presença de pytest e do diretório de testes, mas não executa a suíte; manter fora do escopo.

**Decisões**
- Testar via `TestClient`, cobrindo roteamento, parâmetros, serialização e status HTTP, em vez de chamar as funções diretamente.
- Priorizar as regras da API; o redirect da página inicial e os arquivos estáticos ficam fora deste plano.
- No ambiente atual, `TestClient` funciona, mas emite um aviso de depreciação Starlette/httpx. Só tratar compatibilidade de dependências se isso bloquear a execução.

O plano está registrado em `/memories/session/plan.md` e pronto para revisão antes da implementação.
