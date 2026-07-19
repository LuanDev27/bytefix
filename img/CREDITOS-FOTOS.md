# Créditos das fotos das demos

Todas do **Unsplash**, sob a [Unsplash License](https://unsplash.com/license) — uso comercial
liberado, sem necessidade de atribuição. O crédito abaixo é cortesia e serve de rastro de
procedência caso alguém precise reconferir a licença.

## ⚠️ Antes de trocar qualquer foto: cheque a licença

**Nenhuma destas é Unsplash+ / Getty.** Três candidatas foram descartadas justamente por serem
Unsplash+ (licença **paga**, creditadas a Getty Images) — não podem ir para um site comercial
no ar. A busca do Unsplash mistura as duas coisas sem avisar.

```bash
curl -s -H "Accept: application/json" https://unsplash.com/napi/photos/<ID> \
  | python -c "import json,sys;p=json.load(sys.stdin);print('PAGA' if (p.get('plus') or p.get('premium')) else 'livre')"
```

## Pizzaria (`demo-pizzaria.html`)

Recortadas em cunha a partir do **centro da pizza** — ver `cx/cy/r` no `<script>` da página.

| Arquivo | Sabor | ID | Autor |
|---|---|---|---|
| `pizza-margherita.jpg` | Margherita | `T0q4_yyQ4Hg` | Joanie Simon |
| `pizza-calabresa.jpg` | Calabresa Especial | `dFE0FNVd4k0` | Farhad Ibrahimzade |
| `pizza-pepperoni.jpg` | Pepperoni | `w1iMfs6yxuo` | amirali mirhashemian |
| `pizza-funghi.jpg` | Funghi | `zCs_er8TCPI` | David Foodphototasty |
| `pizza-vegetariana.jpg` | Vegetariana | `BAxltdUdVsc` | Pranjall Kumar |

## Barbearia (`demo-barbearia.html`)

| Arquivo | Serviço | ID | Autor |
|---|---|---|---|
| `barbearia-corte-tesoura.jpg` | Corte Masculino | `tgPrIYnW3g4` | Nate Johnston |
| `barbearia-barba.jpg` | Barba Completa | `IvQeAVeJULw` | Allef Vinicius |
| `barbearia-barba-desenhada.jpg` | Combo Corte + Barba | `S3GxGKyGPWM` | Ahmad Ebadi |
| `barbearia-infantil.jpg` | Corte Infantil | `4brY7Bwss1I` | Ibrahim guetar |
| `barbearia-corte-barba.jpg` | Finalização & Pezinho | `vI3m5UnZ0aQ` | Chris Knight |
| `barbearia-degrade.jpg` | **não usada** | `wJoB8D3hnzc` | delfina pan |

## Academia (`demo-academia.html`)

O slider mostra **modalidades** (sem preço); os **planos** com preço seguem na seção de baixo.

| Arquivo | Modalidade | ID | Autor |
|---|---|---|---|
| `academia-musculacao.jpg` | Musculação | `JNeYWQncbj8` | Alora Griffiths |
| `academia-funcional.jpg` | Treino Funcional | `0Wra5YYVQJE` | Karsten Winegeart |
| `academia-cardio.jpg` | Cardio | `06EpjZiMz_E` | Mike Cox |
| `academia-livre.jpg` | Peso Livre | `qhBpzGubi1I` | Eduardo Cano Photo Co. |
| `academia-coletiva.jpg` | Aulas Coletivas | `3ckWUnaCxzc` | Danielle Cerullo |

## Estética (`demo-estetica.html`)

| Arquivo | Tratamento | ID | Autor |
|---|---|---|---|
| `estetica-facial.jpg` | Limpeza de Pele | `u93nTfWqR9w` | kimia kazemi |
| `estetica-sobrancelha.jpg` | Design de Sobrancelhas | `cowLgyb63c4` | Rune Enstad |
| `estetica-manicure.jpg` | Manicure & Pedicure | `Xa8fX8bQCgs` | Kris Atomic |
| `estetica-massagem.jpg` | Massagem Relaxante | `-AakIaAPV0w` | Simon HUMLER |
| `estetica-cilios.jpg` | Extensão de Cílios | `CqEGy4zAmbI` | Bermix Studio |

## Regra que guiou a escolha: a foto tem que mostrar o que o rótulo diz

O termo de busca **engana**. Ao conferir foto por foto, várias não eram o que a busca sugeria —
a "quatro queijos" era uma margherita, a "manicure" era um close de agulha, a "pepperoni" era
calabresa. Quando não havia foto livre que mostrasse o serviço, o **rótulo foi mudado para
bater com a foto**, nunca o contrário:

- Pizzaria: "Quatro Queijos" e "Portuguesa" viraram **Funghi** e **Vegetariana**.
- Barbearia e Estética: ganharam um 5º serviço (**Finalização & Pezinho**, **Extensão de
  Cílios**) — acrescentado **também à lista de serviços**, para o slider não contradizer o
  cardápio.

## Se alguma demo virar site de cliente real

**Troque estas fotos pelas do próprio cliente.** Foto de banco num cardápio ou numa lista de
serviços real é o mesmo problema do rótulo mentiroso, só que mais convincente — e portanto
pior. Aqui elas existem só para o exemplo ficar apresentável.
