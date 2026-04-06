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

---
 
### `UE4.ini` — Config de Qualidade Máxima para Unreal Engine 4/5
 
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
