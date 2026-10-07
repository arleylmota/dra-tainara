# Dra. Tainara Rodrigues — Link na Bio

Landing page pessoal (estilo "link na bio") da **Dra. Tainara Rodrigues**, médica especialista em Ultrassonografia — CRM-BA 43828.

**Site publicado:** https://dra-tainara-rodrigues.pages.dev/

---

## 1. Como o projeto funciona

É um **site 100% estático**: não há framework, build ou dependências. São apenas arquivos servidos direto pelo CDN da Cloudflare Pages.

```
dra-tainara/
├── index.html               → toda a página (HTML + CSS + JS embutidos)
├── tainara.jpg              → foto da Dra. Tainara (perfil e lightbox)
├── logo-tr-watermark.png    → marca TR usada como marca-d'água e favicon
├── logo-tr.png              → TR em alta resolução (uso futuro)
├── logo-dra-tainara.png     → logo alternativo (não usado na página)
└── .gitignore               → ignora pastas de configuração local (.vercel)
```

### O que existe dentro do `index.html`

| Recurso | Como funciona |
|---|---|
| **Perfil** | Foto (clicável, abre em tela cheia via lightbox), nome, CRM, especialidade e frase curta |
| **Botão Instagram** | Link externo para `instagram.com/tainarards` (abre em nova aba com `rel="noopener noreferrer"`) |
| **Botão WhatsApp** | Link `wa.me/557799918361` — o número só existe no link, nunca visível na página |
| **Compartilhar** | Botão "Compartilhar este link" usa a API nativa `navigator.share` (celular). Em navegadores sem suporte, copia o link para a área de transferência e mostra um aviso ("Link copiado!") |
| **Marca-d'água TR** | Imagem fixa no canto inferior direito, opacidade ~13%, decorativa (`pointer-events:none`) |
| **Tema ultrassom** | Anéis de "eco" animados atrás da foto e onda de sinal entre o perfil e os botões (respeitam `prefers-reduced-motion`) |
| **SEO/OG** | `meta description`, Open Graph e favicon configurados no `<head>` |

Todo o CSS e o JS ficam embutidos no próprio `index.html` — para editar conteúdo, cores ou links, basta alterar esse único arquivo.

---

## 2. Como publicar uma alteração

### Passo a passo (fluxo padrão)

```bash
# 1. Entre na pasta do projeto
cd "/Users/controle/Arley/Meus Projetos - Sistemas/dra-tainara"

# 2. Confira o que mudou
git status
git diff

# 3. Salve as mudanças no Git (commit)
git add .
git commit -m "Descreva o que mudou"

# 4. Envie para o GitHub
git push origin main
```

O repositório remoto é `https://github.com/arleylmota/dra-tainara.git` (branch `main`).

### Como o site chega à Cloudflare

O deploy é feito na **Cloudflare Pages** (que gera o endereço `*.pages.dev`). Duas formas:

1. **Deploy automático (Git integration)** — se o projeto da Cloudflare estiver conectado ao repositório do GitHub, cada `git push` na branch `main` dispara o deploy automaticamente em ~1 minuto. Nada mais precisa ser feito.

2. **Deploy manual (Wrangler)** — usado quando o push não atualiza o site sozinho. O Wrangler é a CLI oficial da Cloudflare e já está autenticado nesta máquina (`~/.wrangler/config/default.toml`):

   ```bash
   npx wrangler pages deploy . --project-name dra-tainara-rodrigues --branch main
   ```

   - `.` = publicar a pasta atual inteira (HTML + imagens)
   - `--project-name dra-tainara-rodrigues` = nome do projeto na Cloudflare (define a URL `dra-tainara-rodrigues.pages.dev`)
   - Se pedir login, rode `npx wrangler login` uma única vez.

### Como verificar se a versão nova está no ar

```bash
# Compare o tamanho do arquivo local com o publicado
curl -s https://dra-tainara-rodrigues.pages.dev/ -o /tmp/live.html
diff /tmp/live.html index.html && echo "Site atualizado ✔"
```

> Dica: se o site continuar mostrando a versão antiga após o `git push`, é sinal de que a integração Git da Cloudflare não está ativa — use o deploy manual com Wrangler (opção 2).

---

## 3. Regras do projeto (importante manter)

- **Marca:** usar somente o logo **TR** (`logo-tr-watermark.png`), como marca-d'água discreta no canto inferior direito.
- **Não exibir telefone** na página — ele existe apenas dentro do link do WhatsApp.
- **Rodapé minimalista:** apenas "© 2026 Dra. Tainara Rodrigues".
- **Não alterar:** nome, CRM-BA 43828, link do Instagram ou número do WhatsApp.
- Manter o design minimalista premium: fundo creme, verde escuro como cor principal, muito espaço em branco.
