# Utilix — Resumo do Projeto

## O que é

Site estático (HTML/CSS/JS puro, sem backend) com ferramentas gratuitas de segurança digital, voltado para usuários comuns. Monetização planejada via Google AdSense. Domínio: **utilix.seg.br** (registrado no registro.br, pagamento em processamento).

Objetivo: renda passiva/extra — tráfego orgânico via SEO, baixa manutenção, sem ferramentas pagas ou complexidade de backend.

## Stack técnica

- Arquivo único `index.html`, sem dependências de build, hospedado via **GitHub Pages**
- Bibliotecas externas via CDN: `qrcode.js` (geração de QR Code), Google Fonts (Space Grotesk, Inter, JetBrains Mono)
- APIs externas usadas (todas gratuitas, sem chave):
  - `ipwho.is` e `ipapi.co` (fallback) — consulta de geolocalização de IP
  - `api.pwnedpasswords.com` — checagem de senha vazada (k-anonymity, nunca envia a senha completa)
- Design: tema escuro, paleta laranja (#FF6A35) + teal (#3FBAC2) + verde (#7CC576), tipografia Space Grotesk/JetBrains Mono

## Ferramentas implementadas

1. **Gerador de senha** — senha aleatória com controle de tamanho e tipos de caractere, medidor de força visual
2. **Gerador de QR Code** — qualquer link/texto vira QR Code, com download em PNG
3. **Consulta de IP** — mostra IP público, localização aproximada, provedor (ISP) e estimativa de tipo de conexão (residencial vs. hosting/datacenter); permite consultar IP de terceiros também
4. **Verificador de força de senha** — estima tempo de quebra por força bruta + checagem real de vazamento via Pwned Passwords API
5. **Verificador de link suspeito** — heurística que detecta sinais de phishing: ausência de HTTPS, IP no lugar de domínio, truque do "@", encurtadores, excesso de subdomínios, imitação de marcas conhecidas (typosquatting)
6. **Exemplos reais de golpe** — 4 modelos de mensagens de golpe comuns no Brasil (Pix falso, falso banco, falsos Correios, falso prêmio) com as partes suspeitas destacadas e explicadas

**Removido do escopo:** gerador de CPF/CNPJ de teste — saía do foco temático de segurança digital.

## Bugs corrigidos ao longo do desenvolvimento

- Cálculo do segundo dígito verificador do CNPJ usava tabela de pesos errada (cortada incorretamente de um array de 12 posições) — corrigido com tabela própria de 13 posições, validado contra 2000 gerações (antes de o CPF/CNPJ ser removido do site)
- Consulta de "Meu IP" travava permanentemente em "consultando..." quando a API `ipapi.co` falhava (bloqueio de CORS/adblocker) sem timeout nem fallback — corrigido com `ipwho.is` como API primária, timeout de 6s e fallback automático para `ipapi.co`
- Verificador de link: lógica de detecção de marca falsa tinha brecha — domínios que continham o nome da marca como substring (ex: `itau-cliente.seguranca-conta.com`) eram tratados como seguros por engano — corrigido com lista de domínios oficiais exatos

## SEO técnico já implementado

- Textos explicativos (SEO) abaixo de cada ferramenta, em português, sem keyword stuffing
- Meta tags: title, description, canonical, Open Graph (og:title, og:description, og:type, og:url, og:locale)
- `robots.txt` — libera indexação e aponta pro sitemap
- `sitemap.xml` — lista a página principal + âncoras de cada ferramenta
- `CNAME` — arquivo necessário pro GitHub Pages reconhecer o domínio customizado `utilix.seg.br`

## Status atual (o que falta)

- [ ] Confirmar pagamento do domínio `utilix.seg.br` no registro.br
- [ ] Subir os arquivos (`index.html`, `robots.txt`, `sitemap.xml`, `CNAME`) no repositório do GitHub
- [ ] Ativar GitHub Pages (Settings → Pages → branch `main`, pasta `/root`)
- [ ] Configurar DNS do domínio (assim que ativo):
  - 4 registros **A** para `utilix.seg.br` apontando para `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
  - Registro **CNAME** para `www.utilix.seg.br` apontando para `seu-usuario.github.io`
- [ ] Marcar "Enforce HTTPS" no GitHub Pages após o DNS propagar
- [ ] Cadastrar no Google Search Console e pedir indexação manual
- [ ] Aguardar tráfego mínimo / maturação do site (algumas semanas)
- [ ] Aplicar para o Google AdSense e substituir os blocos `[ espaço de anúncio ]` pelo código real

## Ideias futuras (backlog, não implementadas)

- Checklist interativo "Você está seguro online?" (quiz com pontuação final)
- Verificador de vazamento por e-mail (Have I Been Pwned) — **requer API paga** desde 2019, avaliar custo-benefício depois
- Camada paga via API com chave (modelo SaaS) — descartado por enquanto, foco é renda passiva via AdSense, não produto pago

## Decisões de produto importantes

- Site é **100% focado em segurança digital** — qualquer ferramenta fora desse tema (como o CPF/CNPJ) deve ser removida ou recusada, pra manter relevância temática e ajudar no SEO
- Prioridade é **simplicidade e baixa manutenção** — sem complicar com APIs pagas, contas adicionais ou infraestrutura de backend
- Todas as ferramentas processam dados localmente no navegador sempre que possível; quando usam API externa, isso é declarado com transparência ao usuário
