# Instruções de publicação — Simulado de Física

> **Estado atual:** publicado e verificado (commit `26aa80d`). Este documento serve para as próximas atualizações.

Documento para outra IA (ou qualquer pessoa) assumir a atualização deste simulado.
Leia até o fim antes de tocar em qualquer arquivo.

---

## 1. Situação em que este projeto está

| Item | Estado |
|---|---|
| Arquivo de trabalho (novo, com figuras) | `saida/simulado-com-figuras.html` — 123.049 bytes em disco (**121.085** após normalizar CRLF) |
| Arquivo de referência (antigo, sem figuras) | `simulado.html` — 115.218 bytes — **NÃO ALTERAR** |
| O que o site publica hoje | **já atualizado** — 121.085 bytes, 4 SVG, commit `26aa80d` |
| Link público | https://maximojota-hue.github.io/simulado-fisica/ |
| Repositório | https://github.com/maximojota-hue/simulado-fisica |
| Branch | `main` |

**Resumo:** a versão com os 4 diagramas SVG **já está publicada e verificada ao vivo** (commit `26aa80d`).
O site serve exatamente o conteúdo de `saida/simulado-com-figuras.html`. As seções abaixo são o
procedimento para as **próximas** atualizações.

---

## 2. Diferença entre o arquivo antigo e o novo

O novo tem tudo do antigo **mais**:

- 4 constantes de SVG embutido no topo do `<script>`:
  `SVG_PV_ISOBARICA`, `SVG_PV_EXPANSAO`, `SVG_PV_COMPRESSAO`, `SVG_CARNOT`
- Campo `"figura"` nas questões **4, 5, 6 e 18**
- O mesmo diagrama dentro das aulas `trabalho` e `carnot`
- `renderQuestion()` renderiza `q.figura` logo após o enunciado
- `montarQuiz()` preserva o campo `figura` no embaralhamento
- `renderLicao()` entende o bloco `{t:"figura", v: SVG_...}`
- Classe CSS `.figura` no `<style>`

Confira antes de publicar:

```bash
grep -c "const SVG_" saida/simulado-com-figuras.html     # deve dar 4
grep -c 'figura: SVG_' saida/simulado-com-figuras.html  # deve dar 4
grep -c 'figura: q.figura' saida/simulado-com-figuras.html  # deve dar 1
```

---

## 3. Regras invioláveis

1. **Nunca edite `simulado.html`.** É a referência congelada. Só `saida/simulado-com-figuras.html`.
2. **Nunca edite `index.html` à mão.** Ele é gerado pelo script — sobrescrito a cada publicação.
3. **Não altere gabaritos** (`ok: true`) nem o texto das questões sem pedido explícito.
4. **Nenhuma imagem externa.** Todo diagrama é SVG inline. Sem CDN, sem `<img src>`, sem fonte externa. O arquivo tem de funcionar offline.
5. **Para criar diagrama novo**, copie o padrão de uma das constantes `SVG_*` existentes (viewBox 320×220 ou 340×240, `text` com `fill="#4b5060"`, rótulos de eixo em itálico). **Não gere imagem por IA** — ela erra eixos, escalas e rótulos.

---

## 4. Publicar (o caminho normal)

O script já faz validação, sincronização, commit, push e espera o Pages.

```bash
cd "C:/Users/jcarlos/Documents/simulado-fisica"

# 1. o arquivo novo precisa ser o que será publicado
cp saida/simulado-com-figuras.html simulado-publicar.html

# 2. valide (pega gabarito duplicado, figura externa, aula inexistente, etc.)
node verificar.js simulado-publicar.html

# 3. se passar, publique
bash publicar.sh "Adiciona diagramas P×V e ciclo de Carnot"
```

> O script `publicar.sh` espera o arquivo `simulado.html`. Como a regra aqui é
> **não tocar no original**, use o passo a passo manual da seção 5 — ou peça
> autorização ao dono antes de sobrescrever `simulado.html`.

---

## 5. Publicar manualmente (sem tocar no arquivo de referência)

Faça nesta ordem. Cada passo é verificável.

```bash
cd "C:/Users/jcarlos/Documents/simulado-fisica"

# --- validar ---
node verificar.js saida/simulado-com-figuras.html
# Esperado: "OK: banco de questoes valido. Pode publicar."
# e a linha "Figuras: 4 questoes com diagrama"

# --- colocar no lugar do que o site serve ---
cp saida/simulado-com-figuras.html index.html

# --- conferir que o index tem as figuras ---
grep -c "const SVG_" index.html          # tem de dar 4
grep -o "<title>[^<]*</title>" index.html

# --- regenerar o ZIP de distribuição ---
rm -f simulado-fisica.zip
powershell -NoProfile -Command "Compress-Archive -Path 'simulado.html','ABRIR-SIMULADO.bat','LEIA-ME.txt' -DestinationPath 'simulado-fisica.zip' -Force"
# ATENÇÃO: dentro do ZIP vai o simulado.html (versão de referência, sem figuras).
# Se o ZIP deve levar a versão nova, troque o primeiro -Path por 'index.html'
# e renomeie dentro do zip, ou combine com o dono qual versão distribuir.

# --- commit e envio ---
git add -A
git -c user.email="simulado@local" -c user.name="Simulado" commit -m "Publica simulado com diagramas P×V e ciclo de Carnot"
git push origin main

# --- esperar o Pages reconstruir (leva ~40 s) ---
gh api repos/maximojota-hue/simulado-fisica/pages/builds/latest | grep -o '"status":"[a-z]*"' | head -1
# repita até aparecer "built"
```

---

## 6. Verificar que publicou de verdade

Não confie no commit — confira o que o servidor está entregando.

```bash
# o site tem de ter as 4 constantes SVG
curl -sL https://maximojota-hue.github.io/simulado-fisica/ | grep -c "const SVG_"
# esperado: 4

# tamanho esperado: ~121085 bytes (o disco mostra 123049 por causa do CRLF)
curl -sL https://maximojota-hue.github.io/simulado-fisica/ -o /tmp/x.html -w "%{size_download}\n"

# o título
curl -sL https://maximojota-hue.github.io/simulado-fisica/ | grep -o "<title>[^<]*</title>"
```

Se o número de `const SVG_` ainda for **0**, o Pages não reconstruiu ainda (medido: leva ~50 s):
aguarde um minuto, rode `gh api repos/maximojota-hue/simulado-fisica/pages/builds` e
peça ao dono para recarregar com **Ctrl+Shift+R** (cache do navegador).

---

## 7. Teste no navegador antes de dizer "pronto"

Abra o arquivo e rode no console (F12). Todos devem passar:

```js
QUESTIONS.length === 28                                        // true
Object.keys(LICOES).length === 12                              // true
QUESTIONS.filter(q => q.figura).length === 4                   // true
QUESTIONS.map((q,i) => q.figura ? i+1 : null).filter(Boolean)   // [4,5,6,18]
QUESTIONS.every(q => q.alternativas.filter(a => a.ok).length === 1)  // true
QUESTIONS.every(q => q.alternativas.every(a => a.porque && a.porque.length >= 25))  // true
```

Depois, na tela:

1. Clique **Iniciar simulado** — cronômetro de 50:00 tem de começar a correr.
2. Vá até a **questão 4** — o gráfico P×V aparece **abaixo do enunciado**, com legenda.
3. Clique numa alternativa → **não** pode revelar nada, só abre a caixa de confirmação.
4. Clique **Confirmar resposta** → aí sim marca verde/vermelho e lista as 5 explicações.
5. Clique **📘 Aprender o assunto do zero** → abre a aula em modal, com figura.
6. Feche com **Esc** → volta na mesma questão, mesma posição de rolagem.
7. Redimensione para 390 px de largura → nada de rolagem horizontal.

---

## 8. O que NÃO fazer

| Erro | Por que quebra |
|---|---|
| Editar `index.html` direto | É sobrescrito na próxima publicação; você perde a edição e o site fica inconsistente |
| Copiar `simulado.html` de Química para cá | Foi assim que a pasta de Química foi corrompida uma vez |
| Colocar a figura como `<img src="fig.png">` | Quebra o offline; o validador rejeita |
| Remover `montarQuiz()` | O gabarito fica todo na letra "a" e o aluno gabarita sem ler |
| Criar campo novo na questão sem incluir em `montarQuiz()` | O campo some depois do embaralhamento (foi o que apagou o botão de aulas) |
| Publicar sem rodar `verificar.js` | O script é o que pega erro de gabarito antes do aluno ver |

---

## 9. Se algo der errado

```bash
# ver o que mudou desde o último envio
git status --short && git diff --stat

# desfazer uma publicação ruim (volta ao commit anterior)
git revert --no-edit HEAD && git push origin main

# ver histórico
git log --oneline -10
```

O commit anterior é `882da50` (versão sem figuras). O atual, com figuras, é `26aa80d`. Serve de
ponto de retorno se a versão nova apresentar problema em algum aparelho.

---

## 10. Resumo em uma linha

```bash
cd "C:/Users/jcarlos/Documents/simulado-fisica" && \
node verificar.js saida/simulado-com-figuras.html && \
cp saida/simulado-com-figuras.html index.html && \
git add -A && git commit -m "Publica simulado com diagramas" && git push origin main
```

Depois confirme com `curl -sL https://maximojota-hue.github.io/simulado-fisica/ | grep -c "const SVG_"` → tem de retornar **4**.
