# Como publicar o Econom-IA no Hugging Face Spaces (sem GitHub)

O app fica online 24/7, de graça, e não depende do seu computador.

## Passo a passo

1. Crie uma conta gratuita em **https://huggingface.co/join** (pode entrar com
   e-mail ou Google).

2. Acesse **https://huggingface.co/new-space** para criar um novo Space:
   - **Owner:** seu usuário.
   - **Space name:** `econom-ia` (ou outro nome).
   - **License:** MIT (ou a que preferir).
   - **Select the Space SDK:** escolha **Streamlit**.
   - **Hardware:** deixe o gratuito (CPU basic).
   - **Visibility:** **Public**.
   - Clique em **Create Space**.

3. O Space abre vazio. Vá na aba **Files** → botão **Add file** →
   **Upload files**.

4. Arraste para a área de upload **todo o conteúdo desta pasta `deploy_hf`**:
   - `app.py`
   - `requirements.txt`
   - `README.md`
   - a pasta `.streamlit` (com `config.toml`)
   - a pasta `dados` (com os 2 arquivos .parquet)
   - a pasta `modelos` (com `random_forest.pkl` e `metricas.json`)

   > Dica: você pode arrastar as pastas inteiras; o site mantém a estrutura.
   > Se o `README.md` já existir no Space, pode substituir pelo desta pasta.

5. Clique em **Commit changes to main** (botão no fim da página).

6. O Hugging Face vai **construir o app automaticamente** (acompanhe na aba
   **Logs**). Em 2 a 5 minutos ele fica no ar.

7. O endereço público será algo como:
   `https://huggingface.co/spaces/SEU-USUARIO/econom-ia`
   É só enviar esse link para a outra pessoa.

## Observações

- Se as cotações ao vivo (Yahoo Finance) forem bloqueadas no servidor, o app
  usa automaticamente os dados locais (Parquet 2019–2025) e o modelo continua
  funcionando — não quebra.
- Para atualizar o app depois, basta subir os arquivos alterados pela mesma aba
  **Files**.
- O tema escuro já vai junto (pasta `.streamlit`).