# Auditoria do simulado de Física (Máquinas térmicas e Termodinâmica)

Auditoria de 2026-11-26. Fonte conferida: a aula em áudio (`E:\estudos gabriel\calor\aula fisica.ogg`, 46 min 24 s) e a sua transcrição completa (`transcricao-aula.txt`, Whisper medium, cobre de 00:00 a 45:31; o conteúdo da aula vai até ~40:30, depois é conversa de sala).

## Resultado

- **Gabarito**: as 28 questões originais foram conferidas (inclusive as contas das Q16, Q17, Q19, Q23, Q27 e Q28). Nenhum gabarito errado.
- **Defeitos corrigidos nas originais**
  - Q9 (1ª lei): a alternativa `Q = ΔU + W` era a MESMA equação da resposta certa (`ΔU = Q − W`), ou seja, havia duas corretas. Agora é `Q = ΔU − W` (errada).
  - Q12: explicação sem sentido sobre "isotérmica" (dizia que a temperatura "varia junto com o meio"). Reescrita.
  - Q6: dizia que expansão e compressão "se cancelam no ciclo completo" (o trabalho líquido do ciclo não é zero). Reescrita.
  - Q20 e aula `carnot`: dizia que as adiabáticas reversíveis são "infinitamente rápidas" (erro; reversível = quase estático, com isolamento perfeito). Corrigido.
  - Q18 (alternativa a) citava "ciclo de Otto" de forma errada; Q1 (alternativa d) tinha explicação confusa; Q15 tinha typo ("o que saída"); Q25 ajustada aos números da aula (gasolina 20 a 25%, diesel 30 a 35%); Q13 sem o termo "integral".
  - Aula `energia-interna`: U = (3/2)nRT só vale para gás monoatômico (os "gases nobres" citados pelo professor). Avisado e com exemplo numérico.
  - Aula `transformacoes`: "panela de pressão" trocada por "recipiente fechado e rígido".
- **Cobertura completa da aula (+20 questões, 28 → 48)**, cada uma com bizu e aula: equilíbrio térmico / lei zero; sinais do calor; por que o gás empurra o pistão; W = P·ΔV com número; trabalho líquido do ciclo = área no gráfico P×V; ΔU acompanha ΔT; ΔU = (3/2)nRΔT; ciclo completo (ΔU = 0); rendimento entre 0 e 1; calor rejeitado (Q₂ = Q₁ − W); Carnot com temperaturas em Celsius; como aumentar o rendimento de Carnot; teste real × Carnot (20% × 70%); calor despejado pela geladeira; ar-condicionado como refrigerador; carregador do celular; usina nuclear como máquina térmica; Revolução Industrial (vantagens e poluição); isovolumétrica e isotérmica com número.
- **Aulas**: 8 das 12 ganharam blocos novos (equilíbrio térmico, por que o gás empurra, exemplos resolvidos, usinas, Revolução Industrial, etc.).
- **Tempo**: 1,5 min por questão (`SEG_POR_QUESTAO = 90`), hoje 48 × 1,5 = **72 minutos**.
- **Sorteio**: no máximo 2 respostas certas seguidas na mesma letra (300 sorteios testados: maior sequência 2; cada letra 9 a 10 vezes em 48).
- A `licao` agora fica dentro de cada questão (antes era uma lista por posição, que quebrava ao inserir questões).

## Testes feitos (navegador, via servidor local)

- `verificar.js`: OK (48 questões, 12 aulas, 1 correta e 5 alternativas em todas, `porque` em todas).
- 300 sorteios: nenhuma aula perdida; sem erro no console; tela inicial mostra 48 questões e 72 minutos; cronômetro começa em 72:00; responder, confirmar e abrir a aula funcionam.

## Fora do escopo desta auditoria

- Os 13 slides fotografados na pasta `calor` são de **Calorimetria** (calor sensível, latente, trocas de calor, potência) e pertencem ao simulado `simulado-calor` (43 questões), que não foi mexido.
- `prova.html` (10 questões no formato da prova) não foi alterado.

## Publicação

Não publicado. Para publicar: `bash publicar.sh "Auditoria: cobertura completa da aula"`.
