# 🛠️ Ferramentas, Workflows e Referências Específicas

Uma coleção pessoal de ferramentas, configurações e workflows voltados para
modding, tradução e engenharia reversa de jogos. O foco é utilidade e
preservação — coisas que levaram tempo pra descobrir e valem ser documentadas.

---

## 📁 Conteúdo

### `Source.cfg` — Config de Qualidade Máxima para Source Engine

Arquivo de configuração base para jogos da Source Engine (CS:S, HL2, GMod, etc.)
com foco em fidelidade visual máxima. Cobre:

- **DirectX**: Força DX11.2 (`mat_dxlevel 112`)
- **Texturas**: Desativa compressão para HDR e mapas difusos em qualidade total
- **Anti-Aliasing**: MSAA 16x + AA por software com thresholds de borda ajustados
- **Sombras**: Sombras renderizadas em textura, alta distância e contagem máxima
- **Água**: Reflexo e refração completos com reflexo de entidades ativo
- **Iluminação**: Mais luzes dinâmicas, radiosity nível 3, world lights estendido
- **LOD / Distância de Desenho**: Força maior detalhe em todas as distâncias (`r_lod -10`, `mat_picmip -10`)
- **Multi-core**: Partículas e renderizáveis em threads separadas
- **Decais**: Contagem máxima de decais para marcas de bala e superfícies
- **Screenshot**: Qualidade JPEG travada em 100

> ⚠️ Requer `sv_cheats 1` para que várias configurações sejam aplicadas.
> Configurações de mouse, FOV e HUD estão incluídas mas comentadas — descomente conforme necessário.

**Como usar:** Coloque o `Source.cfg` na pasta `cfg/` do seu jogo Source
e execute `exec Source` no console, ou adicione ao seu `autoexec.cfg`.