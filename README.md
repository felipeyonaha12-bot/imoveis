---
uid: doc_89859f9a
tipo: documentacao
dominio: imobiliario
tags:
  - imobiliario
data: 2026-09-27
---
# Canal Pro — Integração e Publicador Oficial VRSync (Gold Standard)

Módulo oficial de distribuição e publicação automatizada no **Canal Pro (ZAP Imóveis, VivaReal e OLX)** através de **Feed XML VRSync** hospedado no GitHub Pages.

---

## 📁 Estrutura Oficial Centralizada

```text
05_IMOBILIARIO/
├── CANALPRO/                          -> Espelho de Distribuição / GitHub Pages (felipeyonaha12-bot.github.io/imoveis)
│   ├── feed.xml                       -> Feed XML VRSync oficial consumido por ZAP, VivaReal e OLX
│   ├── index.html                     -> Hub / Vitrine pública do catálogo de imóveis
│   ├── .nojekyll                      -> Configuração do GitHub Pages
│   ├── fotos/santa-ines/              -> Mídias públicas referenciadas no feed
│   └── [slug]/index.html              -> Landing pages estáticas dedicadas
│
└── infra/publicadores/canalpro/       -> Motor de Processamento & Robôs Python (Isolado)
    ├── converter_imovel.py            -> Conversor de dados canônicos para schema Canal Pro
    ├── publicar_imovel.py             -> Validador canônico e injetor de nós no XML
    ├── test_converter_imovel.py       -> Suíte de testes unitários do schema
    └── payload_modelo_canalpro.json   -> Exemplo oficial de payload aceito pelo schema
```

---

## ⚡ Como Funciona o Canal Pro (Fluxo de Publicação)

Diferente do InfoImóveis (que exige automação de clique em formulário web), o **Canal Pro opera via Feed XML VRSync padronizado**:

1. **Geração do Candidato:** O script `publicar_imovel.py` valida as fotos (Pillow), dimensões, regras de negócio e compila o XML no padrão VRSync com integridade SHA-256.
2. **Hospedagem Pública:** As páginas dedicadas dos imóveis ficam em suas respectivas subpastas (`sala-select-park/`, `upper-grand-park/`, `village-parati/`) e o catálogo unificado na raiz (`index.html`).
3. **Distribuição Automática:** Ao sincronizar no repositório do GitHub Pages (`https://github.com/felipeyonaha12-bot/imoveis`), o feed atualiza em `https://felipeyonaha12-bot.github.io/imoveis/feed.xml`.
4. **Ingestão pelos Portais:** Os crawlers do Canal Pro (ZAP, VivaReal e OLX) leem o XML periodicamente e atualizam os portais automaticamente.

---

## 🛠️ Como Gerar um Novo Anúncio VRSync

```powershell
python publicar_imovel.py `
  --dados payload_modelo_canalpro.json `
  --feed feed.xml `
  --output "pacote_candidato"
```

---

## 🛡️ Regras e Restrições do Schema VRSync
- **Formato obrigatório:** VRSync v1.0 com namespace oficial VivaReal.
- **Imagens mínimas:** Exatamente 5 ou mais imagens JPEG válidas (até 7MB cada).
- **Capa:** Exatamente uma imagem com `"capa": true`.
- **DetailViewUrl:** Toda listagem precisa apontar para uma landing page HTTPS pública válida (para evitar erro 404 no robô indexador do Grupo OLX).
## Conexões e Rastreabilidade

- Preparação e validações locais do feed: [[05_IMOBILIARIO/AUTOMATIZAR_E_PUBLICAR/04_PUBLICAR_CANALPRO/legado e pesquisa/ESPECIFICACAO_CLI_SKILL|especificação da CLI/skill]].
- Requisitos de mídia e VRSync: [[SEO e Legendas|regras Canal Pro]].
