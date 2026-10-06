[README.md](https://github.com/user-attachments/files/33127820/README.md)
# Controle de Estoque - Engenharia Clínica

Aplicativo local de controle de estoque para Engenharia Clínica, com perfis de usuário, fornecedores e unidades, entradas, saídas e relatórios.

## Windows compatível

O projeto gera executáveis e instaladores separados para Windows x86 (32 bits) e x64 (64 bits). A versão mínima prevista é **Windows 7 SP1**; também são visados Windows 8, 8.1, 10 e 11.

| Sistema do computador | Arquivo |
| --- | --- |
| Windows de 32 bits | Instalador `...-x86.exe` |
| Windows de 64 bits | Instalador `...-x64.exe` |

O instalador é por usuário e não requer privilégios de administrador. Ele cria atalhos no menu Iniciar e oferece a opção de criar um atalho na área de trabalho. A desinstalação remove o aplicativo, mas preserva os dados locais.

Windows 7 e Python 3.8 estão fora de suporte de segurança. Embora os pacotes sejam compilados com uma base destinada a Windows 7 SP1, a execução ainda deve ser confirmada em computadores com cada versão antiga do Windows.

## Dados locais

Usuários, unidades, fornecedores e movimentações ficam no banco local:

```text
%LOCALAPPDATA%\NexusTech\ControleEstoque\auth.sqlite3
```

Não inclua esse banco, dados reais de pacientes/unidades, senhas ou outros dados confidenciais no repositório GitHub.

## Publicar o projeto neste repositório

O atualizador e a publicação automática só funcionam depois que o código do aplicativo e o fluxo do GitHub Actions estiverem no repositório:

```text
https://github.com/haydenxt6-ops/CONTROLE-ENGENHARIA-CL-NICA-
```

Enviar apenas este README **não** habilita atualizações. Na raiz do repositório, publique os arquivos do projeto, incluindo pelo menos:

- `app.py`, `app_update.py`, `launcher.py` e `version.py`;
- `requirements.txt` e `requirements-build.txt`;
- `templates/` e `static/`;
- `installer.nsi` e `build.bat`;
- `update_config.json`;
- `.github/workflows/release.yml`.

O arquivo `update_config.json` deve apontar para este repositório público:

```json
{
  "repository": "haydenxt6-ops/CONTROLE-ENGENHARIA-CL-NICA-"
}
```

Se enviar os arquivos pela página do GitHub em vez de usar Git, crie o arquivo `.github/workflows/release.yml` pela opção **Add file > Create new file**, usando exatamente esse caminho, e copie o fluxo de publicação fornecido junto com o projeto.

Não envie `data/`, bancos `.sqlite3`, ambientes virtuais (`.venv*`), `__pycache__/` ou credenciais. O `.gitignore` do repositório deve continuar excluindo esses arquivos.

## Publicar uma versão e gerar os instaladores

O fluxo `.github/workflows/release.yml` é acionado por uma tag que começa com `v`. Ele compila as versões x86 e x64, calcula os checksums SHA-256, cria os instaladores e publica os arquivos em **Releases** do GitHub.

Para publicar:

1. Atualize `APP_VERSION` em `version.py`, por exemplo para `1.0.1`.
2. Envie o código e o fluxo de trabalho para o branch principal.
3. Crie e envie uma tag com o mesmo número, por exemplo `v1.0.1`.
4. Abra a aba **Actions** do GitHub e aguarde o fluxo **Build Windows releases** terminar com sucesso.
5. Abra **Releases** e confirme que foram publicados os executáveis e instaladores x86/x64 e `update-manifest.json`.

A versão na tag precisa corresponder ao valor de `APP_VERSION`; se forem diferentes, a publicação é interrompida. O repositório deve permanecer público para que os aplicativos consultem e baixem as atualizações sem credenciais.

## Atualizações nos computadores

Ao iniciar, o aplicativo consulta a última Release pública. Se houver uma versão mais recente, pede confirmação no console, baixa o executável da arquitetura correta, confere o SHA-256 e reinicia para aplicar a atualização. A pasta de instalação deve permitir gravação pelo usuário atual.

Reinstalar o aplicativo preserva o `update_config.json` que já estiver instalado, e atualizações não removem o banco local. As atualizações automáticas dos executáveis distribuídos pelo GitHub só estarão disponíveis depois que o código, o fluxo de trabalho e pelo menos uma Release forem publicados.

## Compilar localmente

Instale Python 3.8 na arquitetura desejada e instale as dependências:

```powershell
python -m pip install -r requirements-build.txt
```

Execute `build.bat` para gerar o executável correspondente à arquitetura do Python instalado. Se NSIS estiver instalado e disponível no `PATH`, o script também gera o instalador. Os arquivos ficam em `dist/`.

## Testes

Execute os testes automatizados com:

```powershell
python -m unittest discover -s tests -v
```
