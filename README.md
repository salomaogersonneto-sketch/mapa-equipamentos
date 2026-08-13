# Mapa de Equipamentos — Marka Engenharia · TFPM

Painel web em tempo real para rastrear a frota de equipamentos da obra no Terminal Ferroviário da Ponta da Madeira (São Luís — MA). Cada equipamento é um pino no mapa satélite, com status de disponibilidade, filtros e indicadores. Uma pessoa muda o status no celular e todo mundo vê na hora.

Hospedagem no GitHub Pages + banco Firebase (plano gratuito): sem servidor próprio, sem custo.

## Publicar (uma vez só)

1. Crie um repositório no GitHub (ex.: `mapa-equipamentos`) e suba os arquivos deste pacote na raiz.
2. **Settings → Pages → Branch: main → Save**. Em 1 a 2 minutos o painel fica no ar em `https://SEU-USUARIO.github.io/mapa-equipamentos/`.
3. Siga o **SETUP-FIREBASE.md** (~20 min) para ativar o tempo real. Sem isso o painel roda em modo demonstração: mostra tudo, mas não salva.
4. Compartilhe o link com a equipe. Funciona no celular, sem login e sem instalar nada.

## Como a equipe usa (rotina diária)

Abrir o link e editar, só isso:

- Mudar status: abrir o equipamento (pino ou lista) → Editar → escolher o status → Salvar.
- Reposicionar: arrastar o pino até onde o equipamento está.
- Incluir: **+ Equipamento** → preencher → o pino nasce no centro do mapa, é só arrastar.

O selo no topo indica o estado: **Tempo real ativo** (verde) · **Modo demonstração** (dourado) · **Sem conexão** (vermelho).

## Funcionalidades

- Mapa satélite real (Esri) com alternância para mapa de ruas
- Pinos por status: Disponível (verde), Em operação (azul), Manutenção (âmbar), Parado (vermelho)
- Sincronização em tempo real entre todos os aparelhos conectados
- KPI de Disponibilidade Física: (Disponível + Em operação) ÷ Total
- Filtros por status, tipo, local e busca por texto
- Validação de tag duplicada e sugestões de locais do TFPM (Cabeceiras, Pátios, Lajes…)
- **Backup**: baixa um `equipamentos.json` com a foto atual da frota (recomendo 1x por semana)
- **Importar**: sobe um `equipamentos.json` inteiro para a nuvem (carga inicial ou restauração)
- Layout responsivo: no celular, abas Mapa / Lista

## Arquivos

| Arquivo | Função |
|---|---|
| `index.html` | O painel completo (app inteiro em um arquivo) |
| `equipamentos.json` | Frota de exemplo — usada no modo demonstração e como modelo de importação |
| `SETUP-FIREBASE.md` | Passo a passo para ativar o tempo real |
| `README.md` | Este arquivo |

## Formato do equipamentos.json (backup/importação)

```json
{
  "atualizado_em": "2026-08-13T07:00:00-03:00",
  "equipamentos": [
    {
      "id": "e1",
      "tag": "ESC-01",
      "tipo": "Escavadeira",
      "local": "Cabeceira Leste",
      "status": "operando",
      "obs": "",
      "lat": -2.5832,
      "lng": -44.3598
    }
  ]
}
```

Valores aceitos em `status`: `disponivel` · `operando` · `manutencao` · `parado`.

## Observações

- O centro inicial do mapa está no TFPM; ajuste a constante `CENTRO` no `index.html` se necessário.
- Os valores do `FIREBASE_CONFIG` podem ficar públicos no repositório; a proteção do banco vem das regras do Firestore (SETUP-FIREBASE.md, passo 4).
- Se duas pessoas editarem o mesmo equipamento no mesmo segundo, vale a última gravação.

---

Marka Engenharia Ltda · Contrato Vale S.A. — TFPM · São Luís — MA
