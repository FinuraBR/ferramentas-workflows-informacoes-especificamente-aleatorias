# 🛠️ Ferramentas, Workflows e Referências Específicas

Uma coleção pessoal de ferramentas, configurações e workflows voltados para
modding, tradução e engenharia reversa de jogos. O foco é utilidade e
preservação de coisas que levaram tempo pra descobrir e valem ser documentadas.

---

## 📋 Índice

- [Source.cfg — Config de Qualidade Máxima para Source Engine](#source)
- [UE4.ini — Config de Qualidade Máxima para Unreal Engine 4/5](#ue4ini)
- [Tradução jogos UE Workflow.txt — Pipeline de Tradução Automatizada para Jogos UE](#tradução-jogos-ue-workflow)
- [r.ScreenPercentage calculator UE games.html — Calculadora de r.ScreenPercentage para Jogos UE](#rscreenpercentage-calculator-ue-games)
- [HexKit.html — Calculadora Hexadecimal](#HexKit)
- [Tabela de Tipos de Dados e Limites.md](#tabela-de-tipos-de-dados-e-limites)

---

## 📁 Conteúdo

### <a name="source"></a>`Source.cfg` — Config de Qualidade Máxima para Source Engine

Arquivo de configuração base para jogos da Source Engine (CS:S, HL2, GMod, etc.)
com foco em fidelidade visual máxima. Organizado por categoria:

- **DirectX**: Força DX11.2 (`mat_dxlevel 112`)
- **Texturas**: Desativa compressão HDR e difusa, filtragem anisotrópica 16x, trilinear ativo, `mat_picmip -10`
- **Anti-Aliasing**: MSAA 16x + AA por software com thresholds de borda e blur ajustados
- **Água**: Reflexo e refração completos, reflexo de entidades ativo, splashes visíveis
- **Sombras**: Render-to-texture, alta distância e contagem máxima de sombras
- **Iluminação**: 300 luzes dinâmicas, radiosity nível 3, bloom ativo, ambient boost habilitado
- **Decais**: Contagem máxima de decais de superfície e modelo
- **LOD / Distância**: Maior detalhe em todas as distâncias, props sem limite de distância (`r_propsmaxdist 99999`)
- **Detalhe de Personagem**: Olhos, flexões faciais, dentes e língua forçados ativos
- **Cordas e Cabos**: Subdivisão máxima, colisão e translucência ativas
- **Céu**: Sky 3D habilitado (`r_3dsky 1`)
- **Multi-core**: Partículas e renderizáveis em threads separadas
- **Screenshot**: Qualidade JPEG travada em 100

> ⚠️ Requer `sv_cheats 1` para que várias configurações sejam aplicadas.
> Gamma, motion blur, mouse, HUD e FOV estão incluídos mas comentados — descomente conforme necessário.

**Como usar:** Coloque o `Source.cfg` na pasta `cfg/` do seu jogo Source
e execute `exec Source` no console, ou adicione ao seu `autoexec.cfg`.

---
 
### <a name="ue4ini"></a>`UE4.ini` — Config de Qualidade Máxima para Unreal Engine 4/5
 
Arquivo de configuração base para jogos em Unreal Engine com foco em fidelidade
visual máxima via `[SystemSettings]`. Compatível com UE4 e em grande parte com
UE5 (exceto onde Lumen/Nanite substituem os sistemas equivalentes).
Organizado por categoria:
 
- **Qualidade Base**: `DetailMode=2` (máximo), `MaterialQualityLevel=3` (Epic), GBuffer de alta precisão (`GBufferFormat=5`), filtragem anisotrópica 16x
- **Iluminação e Pós-Processamento**: Unidades físicas de luz, range de auto-exposição estendido, motion blur desativado, bloom e lens flare na qualidade máxima, aberração cromática desativada (`SceneColorFringeQuality=0`), TAA na qualidade máxima (`PostProcessAAQuality=6`), sharpening leve no tonemapper
- **Ambient Occlusion (GTAO)**: Método Ground Truth (`AmbientOcclusion.Method=1`), qualidade máxima, oclusão especular ativa, 16 ângulos de amostragem
- **Sombras**: Escala de distância 4×, resolução máxima de 4096px, threshold de radius reduzido para sombras mais detalhadas, Distance Field shadows em resolução total, capsule shadows ativas
- **Reflexos (SSR)**: Screen Space Reflections em qualidade máxima (`SSR.Quality=4`), aplicados em superfícies com roughness até 1.0, resolução de reflection captures em 1024
- **Volumetria**: Volumetric fog ativado, grid de alta resolução (`GridPixelSize=4`)
- **Distância e LOD**: Escala de view distance 5×, LODs estáticos em distância máxima (`StaticMeshLODDistanceScale=0.1`), foliage LOD 5×, densidade de grama máxima
- **Streaming de Texturas**: Pool de texturas não limitado pela VRAM, sem cap por tamanho de tela efetivo
- **Subsurface e Partículas**: Subsurface scattering de qualidade alta, iluminação de partículas no nível máximo
 
> ⚠️ Nem todas as CVars funcionam em todos os jogos — títulos com anti-cheat ou
> builds de shipping bloqueados podem ignorar parte das configurações.
> Em UE5 com Lumen ativo, as configurações de SSR e Distance Field Shadows são
> substituídas pelo pipeline do Lumen e podem não ter efeito.
 
**Como usar:** Coloque o conteúdo em `[SystemSettings]` dentro de
`Engine\Config\ConsoleVariables.ini` ou no `DefaultEngine.ini` do jogo
(geralmente em `<Jogo>\Engine\Config\`). Alguns jogos também aceitam via
console em runtime se não houver restrição.

---
 
### <a name="tradução-jogos-ue-workflow"></a>`Tradução jogos UE Workflow.txt` — Pipeline de Tradução Automatizada para Jogos UE
 
Referência para um pipeline completo de tradução via Python + IA, desenvolvido
originalmente para *3 out of 10* (UE 4.24.3) e generalizável para qualquer jogo
Unreal Engine que não ofusque seus assets. Potencialmente adaptável a outras
engines com ferramentas equivalentes de extração e injeção de assets.
 
O arquivo contém links para o repositório de referência (branch principal e commit
fixo de backup). A implementação completa, incluindo todos os scripts e instruções,
está no repositório linkado.
 
**Duas trilhas de processamento, dependendo do tipo de arquivo:**
 
- **Trilha `.locres`** (strings de UI, legendas, menus via sistema de localização nativo da UE): exportação para CSV pelo *UE4 Localizations Tool* → divisão em partes → tradução via IA → merge → reimportação
- **Trilha `.uasset`** (DataTables, Blueprints, strings embutidas): extração binária via *FModel* → filtro por keywords em mmap → conversão para JSON via *UAssetGUI CLI* → extração recursiva de strings traduzíveis → divisão em partes → tradução via IA → injeção de volta no JSON mestre → reconversão para `.uasset`
 
**Destaques técnicos do pipeline:**
 
- **Filtragem inteligente em dois estágios**: varredura binária com `mmap` (rápida, descarta assets sem texto antes de qualquer conversão) seguida de análise semântica do JSON convertido (descarta strings técnicas, IDs, paths e flags `Immutable`)
- **Orquestrador com códigos de saída**: `6_processar_tudo.py` gerencia a fila completa de arquivos, detecta o que já foi processado e pausa automaticamente quando novos termos técnicos precisam ser adicionados à blacklist
- **Resiliência da chamada de IA**: threads com timeout configurável e retry automático; proteção contra loop de gasto em erros de saldo insuficiente
- **Validação pós-tradução**: verificação de contagem de itens e integridade de tags técnicas (`{0}`, `%s`, `<tags>`) antes de salvar qualquer arquivo
- **Aprendizado de blacklist**: quando a IA retorna o texto sem traduzir (original == traduzido), o termo é registrado automaticamente em `sugestoes_blacklist.txt` para revisão
- **Script de QA automatizado** (opcional): abre o jogo via subprocesso, empacota o mod em `.pak` e monitora crashes e estado "Não Respondendo" via `pyautogui` + `psutil`
 
**Ferramentas externas utilizadas:** FModel, UAssetGUI / UAssetAPI, UE4 Localizations Tool, UnrealPak
 
**Modelos de IA suportados:** qualquer modelo via Ollama (local) ou API compatível com OpenAI (cloud via Puter); testado com DeepSeek e Qwen
 
> ⚠️ O pipeline depende da estrutura de metadados que o UAssetGUI expõe no JSON
> (campos como `HistoryType`, `Flags`, `Type`). Jogos com versões de UE muito antigas
> ou muito novas podem requerer ajustes nas regras de filtragem do `config.py`.
> Assets com IO Store / Zen Loader (UE 5.0+) exigem ferramentas adicionais para extração.

---

### <a name="rscreenpercentage-calculator-ue-games"></a>`r.ScreenPercentage calculator UE games.html` — Calculadora de r.ScreenPercentage para Jogos UE

Ferramenta HTML standalone para calcular o valor correto de `r.ScreenPercentage`
em jogos Unreal Engine. Útil para configurar renderização interna abaixo ou acima
da resolução nativa do monitor (downscaling para performance ou supersampling para qualidade).

**Como funciona:**

Selecione a resolução nativa do seu monitor e a resolução alvo de renderização.
A calculadora retorna:

- O valor exato de `r.ScreenPercentage` (com casas decimais quando necessário)
- Total de megapixels renderizados vs nativo
- Percentual de pixels economizados (downscale) ou adicionados (supersampling)
- O comando completo pronto para copiar e colar no console do jogo ou no `Engine.ini`

Suporta resoluções de 128×72 até 8K (7680×4320), incluindo atalhos para
resolucões comuns como 720p, 1080p, 1440p, 4K e 5K.

> Funciona diretamente no navegador, sem instalação ou dependências.
> Abra o arquivo `.html` localmente e use offline.

**Como usar:** Abra o arquivo no navegador, selecione as resoluções e copie
o comando gerado para o console do jogo (`~`) ou adicione ao `Engine.ini`
dentro de `[SystemSettings]`.

---

### <a name="HexKit"></a>`HexKit.html` — Calculadora Hexadecimal
 
Ferramenta HTML standalone para operações com valores hexadecimais. Serve para qualquer contexto que envolva aritmética e lógica em hex, offsets de memória, endereços, máscaras de bits, scripts de assembly, conversão de floats, etc. Tema claro/escuro persistente, funciona completamente offline.
 
**Aba: Calculadora Hex**
 
Realiza operações binárias entre dois endereços hex (Final e Inicial):
 
`B − A` `B + A` `B × A` `B ÷ A` `B mod A` `B & A` `B | A` `B ^ A` `B << A` `B >> A`
 
Também suporta operações unárias sobre o campo Inicial:
NOT, NEG, Align 4, Align 16 e próxima potência de 2.
 
O resultado é exibido em HEX, DEC, BIN e em bytes (com conversão para KB). Inclui:
 
- **Visualizador de bits**: grade visual dos bits individuais agrupados em nibbles, com separação por byte e toggle entre 32-bit e 64-bit
- **Recomendação de alloc**: indica qual tamanho de bloco CE é insuficiente, apertado ou ideal para o valor calculado (`$40` até `$8000`)
- **Histórico**: últimos 8 cálculos persistidos entre sessões — clique para restaurar os campos automaticamente
- **Botão swap**: troca Final e Inicial sem redigitar
- **Validação visual**: borda vermelha em tempo real para entrada hex inválida
 
**Aba: Conversor Float**
 
Conversão bidirecional entre representação hexadecimal e ponto flutuante IEEE 754:
 
- **Hex → Float/Double**: dado um valor hex, exibe a interpretação como Float (32-bit) e Double (64-bit) com destaque para valores especiais (NaN, ±Inf)
- **Float/Double → Hex**: dado um valor decimal, retorna o hex correspondente em ambas as precisões. Aceita notação científica e valores especiais (`Infinity`, `NaN`)
- **Tabela de referência clicável**: 12 valores notáveis (±Inf, NaN, ±0, 1.0, -1.0, 0.5, 2.0, 100.0, máximo e mínimo de Float) — clique numa linha para carregar no conversor
 
**Aba: Analisar Código CE**
 
Cole um bloco de auto-assembler do Cheat Engine (de `newmem:` até o último `jmp return`) e a ferramenta estima o tamanho em bytes de cada instrução, exibindo um breakdown linha a linha. Suporta:
 
- Instruções comuns x64: `mov`, `lea`, `push`, `pop`, `add`, `sub`, `cmp`, `xchg`, `ret`, `inc`, `dec`, `imul`, `cdq`, `cqo` etc.
- Flags: `pushfq`, `popfq`, `lahf`, `sahf`
- SSE: `movss`, `movsd`, `mulss`, `addss`, `movaps`, `movdqa` e variantes
- x87 FPU: `fld`, `fstp`, `fadd`, `fmul` e variantes
- Saltos e calls: `jmp`, `call`, `jcc` (rel32 e via registrador)
- Diretivas: `align N`, `db`, `dq`, `dd`, `dw`, `nop`
- Comentários `//` e `;` ignorados automaticamente
 
Exibe o total estimado de bytes e a mesma recomendação de `alloc` da aba Hex.
 
> Funciona diretamente no navegador, sem instalação ou dependências.
> Abra o arquivo `.html` localmente e use offline.

---

### <a name="tabela-de-tipos-de-dados-e-limites"></a>`Tabela de Tipos de Dados e Limites.md`

Referência completa de tipos de dados para uso em reverse
engineering e programação geral. Cobre todos os tipos relevantes com valores mínimos, máximos e
comportamento em overflow.

**Seções:**

- **Inteiros sem sinal (Unsigned)** — Byte, Word, DWORD, QWORD: faixa e comportamento de overflow
- **Inteiros com sinal (Signed)** — Int8, Short, Int32, Int64: faixa negativa e wrap em overflow
- **Ponto Flutuante** — Float e Double com valor mínimo/máximo, representações hex de +Inf, -Inf, NaN e Zero negativo
- **Texto e Caractere** — Char, WChar, String e WString com encoding e terminação
- **Tipos Lógicos e Especiais** — Bool (1 e 4 bytes), ponteiros 32/64-bit e Void
- **Representações Hex Especiais** — Tabela de valores Float/Double notáveis em hex (1.0, -1.0, 0.5, máximo finito, zeros, NaN)
- **Tamanhos por Arquitetura** — Diferenças de `int`, `long`, `pointer` e `size_t` entre x86 e x64

> Todos os valores fracionários usam ponto (.) como separador decimal,
> adequado para uso geral e ferramentas de debug.
