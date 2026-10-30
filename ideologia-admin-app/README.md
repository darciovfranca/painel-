# Ideologia — Memorial do Rap (versão com painel administrativo)

Aplicação React + Vite + Supabase. Inclui login Google, papéis de usuário, painel de administração, upload de mídia, dois álbuns, seis capítulos, 52 figurinhas e validação de senha no servidor.

## Configuração necessária

1. Crie um projeto no Supabase.
2. No SQL Editor, execute o arquivo `supabase/schema.sql` inteiro. Ele cria as tabelas, políticas de segurança, função de desbloqueio, bucket de arquivos e dados históricos iniciais.
3. Em Authentication → Providers, habilite Google e configure as credenciais OAuth do Google Cloud.
4. Em Authentication → URL Configuration, informe o domínio publicado e `http://localhost:5173` como URL de redirecionamento permitida durante testes.
5. Copie `.env.example` para `.env` e preencha `VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY` com os dados do projeto Supabase.
6. Instale e execute:
   ```bash
   npm install
   npm run dev
   ```
7. O primeiro usuário que concluir o login será promovido a administrador automaticamente. Os próximos entram como usuários comuns.
8. Publique na Vercel ou outro serviço compatível com Vite e configure as mesmas variáveis de ambiente no painel de hospedagem.

## O que o administrador pode alterar

- Nome do projeto, textos de abertura, cores, imagem de fundo, logotipo e imagem de destaque.
- Capas e descrições dos dois álbuns.
- Textos, títulos e ordem dos capítulos.
- Nome, dica, raridade, imagem e senha de cada figurinha.
- Títulos e arquivos de áudio das músicas.
- Upload de imagens e áudios para o Storage do Supabase.

## Segurança e limites importantes

- A primeira conta é definida como administradora pelo gatilho do banco, com bloqueio transacional para evitar corrida entre primeiros cadastros.
- As respostas das figurinhas ficam em `sticker_secrets`, sem política de leitura pública. A verificação é feita pela função `unlock_sticker` no banco; a senha não é enviada ao navegador dos participantes.
- A coleção é salva por usuário no banco e pode ser acessada em outros dispositivos após login.
- O projeto não inclui chaves privadas no frontend. Use apenas a chave pública/anon no `.env`; nunca coloque a `service_role` no navegador.
- Para trocar o administrador inicial, faça isso diretamente no banco com controle de acesso, não por uma opção pública do aplicativo.
- O primeiro usuário será administrador apenas se não existir nenhum perfil com papel `admin` no momento do cadastro. Se já houver administrador, novas contas serão usuários comuns.

## Observação

Os nomes, dicas e senhas foram preenchidos a partir do texto fornecido. Os arquivos reais das figurinhas, capas e músicas não estavam no ZIP; o painel permite adicioná-los depois. Os títulos das 26 faixas começam como “Faixa 01” etc., aguardando os nomes e áudios definitivos.
