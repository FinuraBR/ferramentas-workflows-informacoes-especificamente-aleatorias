# 🛠️ Ferramentas, Workflows e Referências Específicas

Uma coleção pessoal de ferramentas, configurações e workflows voltados para
modding, tradução e engenharia reversa de jogos. O foco é utilidade e
preservação de coisas que levaram tempo pra descobrir e valem ser documentadas.

---

## 📁 Conteúdo

### `Source.cfg` — Config de Qualidade Máxima para Source Engine

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